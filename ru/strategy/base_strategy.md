
# Пример простой стратегии

- [Описание стратегии и её исходный код](#strategy)
- [Анализ стратегии](#analysis)

## Описание стратегии и её исходный код {#strategy}

Идея данной стратегии состоит в том, чтобы поддерживать на каждом направлении (BID и ASK) по одной нашей заявке на цене, равной `middle_price` с константным смещением `offset` в сторону соответствующего направления.
В случае, если по направлению нет наших активных заявок, мы выставляем заявку с объёмом `volume`.
Если заявка уже есть, но она стоит на другой цене, мы её снимаем и ставим новую заявку на нужную цену.
Объём выставляемой заявки может быть уменьшен для предотвращения выхода за максимальную позицию.
Для HFT-стратегий очень важна возможность быстро вернуться к нулевой позиции, поэтому в стратегии предусмотрено ограничение максимальной позиции до значения `max_pos`.

Описание параметров стратегии:

- volume — объём выставляемых заявок.
- max_pos — максимальная позиция.
- offset — отступ цены от `middle_price`.

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

Более сложная стратегия, а также идеи для дальнейшей реализации доступны [здесь](ideas.md).