# Пример стратегии

- [Описание стратегии и её исходный код](#strategy)
- [Анализ стратегии](#analysis)
- [Идеи для дальнейшей реализации](#ideas)
- [Улучшенная стратегия](#improved)

## Описание стратегии и её исходный код {#strategy}

Идея данной стратегии состоит в том, чтобы поддерживать на каждом направлении (BID и ASK) по одной нашей заявке на цене, равной `middle_price` с константным смещением `offset` в сторону соответствующего направления.
В случае, если по направлению нет наших активных заявок, мы выставляем заявку с объёмом `volume`.
Если заявка уже есть, но она стоит на другой цене, мы её снимаем и ставим новую заявку на нужную цену.
Объём выставляемой заявки может быть уменьшен для предотвращения выхода за максимальную позицию.
Для HFT-стратегий очень важна возможность быстро вернуться к нулевой позиции, поэтому в стратегии предусмотрено ограничение максимальной позиции до значения `max_pos`.

Описание параметров стратегии:

- volume — объём выставляемых заявок.
- max_pos — максимальная позиция.
- offset — отступ цены от middle_price.

{% codetabs name="C++", type="c++" -%}
#include "participant_strategy.h"

using namespace hftbattle;

namespace {

class UserStrategy : public ParticipantStrategy {
public:
  UserStrategy(const JsonValue& config) :
      volume_(config["volume"].as<Amount>(1)),
      max_pos_(config["max_pos"].as<Amount>(1)),
      offset_(config["offset"].as<Price>(18)) {
    set_max_total_amount(max_pos_);
  }

  Amount max_available_order_amount(Amount pos, Dir dir) {
    Amount max_amount = std::min(max_pos_ - dir_sign(dir) * pos, volume_);
    return std::max(0, max_amount);
  }

  void trading_book_update(const OrderBook& order_book) override {
    const auto& orders = order_book.orders();
    Price middle_price = order_book.middle_price();
    Amount pos = executed_amount();

    add_chart_point("middle_price", middle_price);

    for (Dir dir : {BID, ASK}) {
      Price target_price = middle_price - dir_sign(dir) * offset_;
      Amount order_amount = max_available_order_amount(pos, dir);

      if (orders.active_orders_count(dir) == 0) {
        if (order_amount > 0) {
          add_limit_order(dir, target_price, order_amount);
        }
      } else {
        Order* current_order = orders.orders_by_dir(dir).front();
        if (current_order->price() != target_price) {
          delete_order(current_order);
          if (order_amount > 0) {
            add_limit_order(dir, target_price, order_amount);
          }
        }
      }
    }
  }

private:
  Amount volume_;
  Amount max_pos_;
  Price offset_;
};

}  // namespace

REGISTER_CONTEST_STRATEGY(UserStrategy, user_strategy)
{%- language name="Python", type="py" -%}
# -*- coding: utf-8 -*-

from py_defs import *
from py_defs import Decimal as Price
from common_enums import *


class Params:
    pass


def init(strat, config):
    Params.VOLUME = config.get('VOLUME', 1)
    Params.MAX_POS = config.get('MAX_POS', 1)
    Params.OFFSET = Price(config.get('OFFSET', 18))

    strat.set_max_total_amount(Params.MAX_POS)


def max_available_order_amount(pos, dir):
    max_amount = min(Params.MAX_POS - dir_sign(dir) * pos, Params.VOLUME)
    return max(max_amount, 0)


def trading_book_update(strat, order_book):
    orders = order_book.orders()
    middle_price = order_book.middle_price()
    pos = strat.executed_amount()

    strat.add_chart_point('middle_price', middle_price)

    for dir in (BID, ASK):
        target_price = middle_price - dir_sign(dir) * Params.OFFSET
        order_amount = max_available_order_amount(pos, dir)

        if orders.active_orders_count(dir) == 0:
            if order_amount > 0:
                strat.add_limit_order(dir, target_price, order_amount)
        else:
            current_order = orders.orders_by_dir(dir)[0]
            if current_order.price() != target_price:
                strat.delete_order(current_order)
                if order_amount > 0:
                    strat.add_limit_order(dir, target_price, order_amount)
{%- endcodetabs %}

## Анализ стратегии {#analysis}

Поскольку стратегия поддерживает только 1 заявку размером в 1 лот по каждому направлению, это не даёт ей делать много сделок в одном направлении.
С другой стороны, при маленьких ограничениях максимальной позиции стратегия не будет долго находиться с большим количеством купленных (или проданных) лотов, что не позволит потерять много денег при существенном движении цены.

## Идеи для реализации {#ideas}

1. Серьёзный фактор, ограничивающий результат стратегии — низкое значение максимальной позиции.
  Увеличение данного параметра позволит стратегии совершать большее количество сделок, тем самым пропорционально увеличивая заработок.
  Тем не менее увеличение максимальной позиции повышает риск длительного нахождения стратегии в односторонней позиции, что отрицательно влияет на итоговый результат.
2. Текущая стратегия поддерживает лишь 1 заявку на 1 ценовом уровне по каждому направлению.
  Заняв несколько различных ценовых уровней в стакане, можно совершать значительно больше выгодных сделок.
3. Выше уже отмечалось, что очень важно быстро закрывать позицию.
  Одним из таких методов является динамический выбор цены и объёма выставляемых заявок.
  Чем выше текущая позиция вдоль некоторого направления, тем активнее необходимо действовать в противоположном направлении и, наоборот, стараться воздерживаться от совершения сделок в текущем направлении.
4. Помимо поддержания "котирующих" заявок на каждом из направлений, в определенные моменты можно забирать "чужой объём", который находится в стакане.
  Это можно делать как для уменьшения текущей позиции, так и увидев, что какой-то участник торгов стоит на несправедливой цене.

Это только часть того, чем мы хотели бы поделиться с вами.
В ходе контеста идеи будут дополняться и описываться более подробно.

## Улучшенная стратегия {#improved}

В предыдущей стратегии мы ставим заявку на уровень, который отстоит от средней цены на фиксированную разницу `offset`.
Однако, такое поведение не является разумным, если на одном из направлений лучшая цена является неадекватной.

Это происходит потому, что middle_price очень чувствителен по отношению к лучшим ценам — если появится даже одна заявка с ценой лучше, чем была, то изменится middle_price и изменятся уровни, на которые мы ставим заявку.

Отметим, что стакан является разреженным — между котировками могут быть большие промежутки (т.е. много котировок пустые), а также на многих котировках стоит небольшой объём.

Попробуем стоять на такой цене, чтобы перед нами стоял фиксированный объём заявок — в нашем случае за это отвечает параметр `volume_before_our_order_`.

Если написать стратегию в таком виде, мы столкнёмся с двумя проблемами:

1. Если мы будет стоять на той же цене, на которой достигается `volume_before_our_order_`, то наша стратегия будет стоять после других участников рынка.
2. Помимо ограничения на объём перед нашей заявкой, стоит проследить за тем, чтобы наши заявки не стояли близко друг к другу (на расстоянии меньше двух комиссий).
  Иначе может получиться так, что обе заявки исполнятся, а мы окажется в минусе просто заплатив комиссию.

Вот как мы попытаемся решить эти проблемы:

1. Будем стоять на цене, которая ближе к `middle_price` на один `min_step` чем та, которую мы нашли.
2. Мы не будем ставить заявку, если от противоположной лучшей цены она отстоит меньше, чем на заранее заданное значение.
  Это значение мы также назовём `offset_`, т.к. оно имеет почти такой же смысл, как и в предыдущей.

В итоге мы получим следующую стратегию:

{% codetabs name="C++", type="c++" -%}
#include "participant_strategy.h"

using namespace hftbattle;

namespace {

class UserStrategy : public ParticipantStrategy {
public:
  UserStrategy(const JsonValue& config) :
      volume_(config["volume"].as<Amount>(2)),
      max_pos_(config["max_pos"].as<Amount>(3)),
      offset_(config["offset"].as<Price>(26)),
      volume_before_our_order_(config["volume_before_our_order"].as<Amount>(2)) {
    set_max_total_amount(max_pos_);
  }

  Amount max_available_order_amount(Amount pos, Dir dir) {
    Amount max_amount = std::min(max_pos_ - dir_sign(dir) * pos, volume_);
    return std::max(0, max_amount);
  }

  void trading_book_update(const OrderBook& order_book) override {
    const auto& orders = order_book.orders();
    Price middle_price = order_book.middle_price();
    Amount pos = executed_amount();

    add_chart_point("middle_price", middle_price);

    for (Dir dir : {BID, ASK}) {
      Amount accumulated_volume = 0;
      size_t idx = 0;
      for (; idx < order_book.depth(); ++idx) {
        accumulated_volume += order_book.volume_by_index(dir, idx);

        if (accumulated_volume >= volume_before_our_order_) {
          break;
        }
      }

      Price target_price = order_book.price_by_index(dir, idx) + dir_sign(dir) * order_book.min_step();
      Price diff = abs(target_price - order_book.best_price(opposite_dir(dir)));
      Amount order_amount = max_available_order_amount(pos, dir);

      if (orders.active_orders_count(dir) == 0) {
        if (order_amount > 0 && diff > offset_) {
          add_limit_order(dir, target_price, order_amount);
        }
      } else {
        Order* current_order = orders.orders_by_dir(dir).front();
        if (current_order->price() != target_price) {
          delete_order(current_order);
          if (order_amount > 0 && diff > offset_) {
            add_limit_order(dir, target_price, order_amount);
          }
        }
      }
    }
  }

private:
  Amount volume_;
  Amount max_pos_;
  Price offset_;
  Amount volume_before_our_order_;
};

}  // namespace

REGISTER_CONTEST_STRATEGY(UserStrategy, user_strategy)
{%- endcodetabs %}