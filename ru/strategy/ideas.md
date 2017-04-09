## Улучшенная стратегия

В [базовой стратегии](base_strategy.md) мы ставим заявку на уровень, который отстоит от средней цены `middle_price` на фиксированную разницу `offset`.
Однако, такое поведение не является разумным, если, например, на одном из направлений лучшая цена является несправедливой.
`middle_price` очень чувствителен к колебаниям в стакане: если на направлении появится заявка, стоящая перед предыдущей лучшей ценой, то изменится `middle_price`, и, соответственно, изменятся уровни, на которые мы хотим выставить заявку.
В том числе `middle_price` изменится, если, например, заявка размером 1 лот встанет сильно выше предыдущей лучшей цены покупки.
Понятно, что в большинстве случаев в такой ситуации заявка не отражает справедливую цену покупки инструмента.
Рекомендуем подумать о том, как по-другому можно считать middle_price, чтобы снизить зависимость от "случайных" заявок.

Отметим, что стакан на предложенном инструменте является разреженным — между котировками могут быть большие промежутки (т.е. большое количество ценовых уровней без заявок), а также на многих котировках стоит небольшой объём.
Запомним это на будущее, ведь понятно, что вставать на пустую цену, в целом, лучше, чем на уже занятую — в таком случае мы будем иметь приоритет в очереди.

Ценовой уровень, на который выставим заявку, будем выбирать таким образом, чтобы перед нашей заявкой по данному направлению уже стоял определённый объём лотов — в нашем случае за это отвечает параметр `volume_before_our_order`.
Это позволит нам исключить возможность того, что мы встанем на "несправедливую" цену.

Если написать стратегию в таком виде, мы столкнёмся с двумя проблемами:

1. Если мы будем стоять на той же цене, на которой достигается `volume_before_our_order`, то наша стратегия будет заведомо стоять в очереди за чужими заявками.
2. Помимо ограничения на объём перед нашей заявкой, стоит следить за тем, чтобы наши заявки (на покупку и продажу) не стояли слишком близко друг к другу.
  Если обе заявки исполнятся почти одновременно, то мы рискуем оказаться в минусе из-за того, что мы дважды заплатим комиссию за проведение сделки.

Вот как мы попытаемся решить эти проблемы:

1. Будем стоять на цене, которая расположена на один `min_step` ближе к `middle_price` чем та, которую мы выбрали изначально.
  Есть вероятность, что это будет пустая цена, что хорошо для нас.
  Это упрощенная версия более сложной идеи: в целом, нужно выбирать цену так, чтобы объём лотов на следующей за нами цене существенно превосходил объём лотов на нашей котировке.
2. Мы не будем ставить заявку, если от противоположной лучшей цены она отстоит меньше, чем на заранее заданное значение.
  (Это условие имеет смысл ослабить, если окажется, что из-за его невыполнения зачастую на протяжении длительного периода времени мы вообще не выставляем заявки на данном направлении).
  Это значение мы также назовём `offset`, т.к. оно имеет почти такой же смысл, как и в предыдущей стратегии.

В итоге мы получим следующую стратегию:

{% codetabs name="C++", type="c++" -%}
#include "participant_strategy.h"

using namespace hftbattle;

namespace {

class UserStrategy : public ParticipantStrategy {
public:
  UserStrategy(const JsonValue& config) :
      volume_(config["volume"].as<Amount>(2)),
      max_pos_(config["max_pos"].as<Amount>(1)),
      offset_(config["offset"].as<Price>(17)),
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
{%- language name="Python", type="py" -%}
# -*- coding: utf-8 -*-

from py_defs import *
from py_defs import Decimal as Price
from common_enums import *


class Params:
    pass


def init(strat, config):
    Params.VOLUME = config.get('VOLUME', 2)
    Params.MAX_POS = config.get('MAX_POS', 1)
    Params.OFFSET = Price(config.get('OFFSET', 17))
    Params.VOLUME_BEFORE_OUR_ORDER = config.get('VOLUME_BEFORE_OUR_ORDER', 2)

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
        accumulated_volume = 0

        for idx in xrange(order_book.depth()):
            accumulated_volume += order_book.volume_by_index(dir, idx)

            if accumulated_volume >= Params.VOLUME_BEFORE_OUR_ORDER:
                break

        target_price = order_book.price_by_index(dir, idx) + dir_sign(dir) * order_book.min_step()
        diff = abs(target_price - order_book.best_price(opposite_dir(dir)))
        order_amount = max_available_order_amount(pos, dir)

        if orders.active_orders_count(dir) == 0:
            if order_amount > 0 and diff > Params.OFFSET:
                strat.add_limit_order(dir, target_price, order_amount)
        else:
            current_order = orders.orders_by_dir(dir)[0]
            if current_order.price() != target_price:
                strat.delete_order(current_order)
                if order_amount > 0 and diff > Params.OFFSET:
                    strat.add_limit_order(dir, target_price, order_amount)
{%- endcodetabs %}

## Идеи для реализации {#ideas}

1. Серьёзный фактор, ограничивающий результат стратегии — низкое значение максимальной позиции.
  Увеличение данного параметра позволит стратегии совершать большее количество сделок, тем самым (почти) пропорционально увеличить заработок.
  Тем не менее увеличение максимальной позиции повышает риск длительного нахождения стратегии в односторонней позиции, что отрицательно влияет на итоговый результат.
2. Описанные выше стратегии поддерживают лишь одну заявку на одном ценовом уровне по каждому направлению.
  Заняв несколько различных ценовых уровней в стакане, можно совершать значительно больше выгодных сделок.
3. Выше уже отмечалось, что очень важно быстро закрывать позицию.
  Одним из таких методов является динамический выбор цены и объёма выставляемых заявок.
  Чем выше текущая позиция вдоль некоторого направления, тем активнее необходимо действовать в противоположном направлении и, наоборот, стараться воздерживаться от совершения сделок в текущем направлении.
4. Помимо поддержания "котирующих" заявок на каждом из направлений, в определенные моменты можно забирать "чужой объём", который находится в стакане.
  Это можно делать как для уменьшения текущей позиции, так и увидев, что какой-то участник торгов стоит на несправедливой цене.
5. Выше уже отмечалось, что `middle_price` плохо отражает справедливую цену в случае разреженного стакана.
  Рекомендуем подумать над метрикой, которая будет правильнее оценивать реальную цену инструмента.
6. У описанной выше стратегии есть проблема — при подсчетах объемов она учитывает свои заявки в стакане.
  Это может плохо повлиять на поведение стратегии: например, мы можем начать перемещать свою заявку на предыдущую цену из-за того, что при подсчёте объёма учитываем объём своих заявок на ценовых уровнях.

В ходе контеста идеи будут дополняться и описываться более подробно.
