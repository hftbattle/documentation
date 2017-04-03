
# Simple strategy example

- [Description of the strategy and its source code](#strategy)
- [The strategy analysis](#analysis)

## Description of the strategy and its source code {#strategy}

The idea of the strategy is to keep one order on the best price at each direction (BID и ASK) with the price of `middle_price` and a constant offset `offset` in the corresponding direction.
In case a direction has none of our active orders, the strategy places an order with volume of `volume`.
In case an order exists, but happens to be of other price, we cancel it and replace it with the new one.
The volume of the placed order can be decreased for us not to get out of the maximum volume size.
It is very important for HFT strategy to be able to quickly close the current position. So, we will try to limit the maximum open position to the value of `max_pos`.

Description of the strategy parameters:

- volume — volume of the orders being placed.
- max_pos — maximal position.
- offset — price difference from the middle_price.

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

  Amount amount_available(const Amount pos, const Dir dir) {
    auto max_volume = std::min(max_pos_ - dir_sign(dir) * pos, volume_);
    return std::max(0, max_volume);
  }

  void trading_book_update(const OrderBook& order_book) override {
    const auto& orders = order_book.orders();
    auto middle_price = order_book.middle_price();
    auto pos = executed_amount();

    add_chart_point("middle_price", middle_price);

    for (Dir dir : {BID, ASK}) {
      auto target_price = middle_price - dir_sign(dir) * offset_;
      auto order_amount = amount_available(pos, dir);

      if (orders.active_orders_count(dir) == 0) {
        if (order_amount > 0) {
          add_limit_order(dir, target_price, order_amount);
        }
      } else {
        auto current_order = orders.orders_by_dir(dir).front();
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


def amount_available(pos, dir):
    max_volume = min(Params.MAX_POS - dir_sign(dir) * pos, Params.VOLUME)
    return max(max_volume, 0)


def trading_book_update(strat, order_book):
    orders = order_book.orders()
    middle_price = order_book.middle_price()
    pos = strat.executed_amount()

    strat.add_chart_point('middle_price', middle_price)

    for dir in (BID, ASK):
        target_price = middle_price - dir_sign(dir) * Params.OFFSET
        order_amount = amount_available(pos, dir)

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

## The strategy analysis{#analysis}

Since the strategy keeps only 1 order with a volume of 1 lot for each direction, it does not make enough deals to produce a good profit.
On the other hand, limiting position would not allow the strategy to keep its position in one direction for a long time.
Staying in one-directed position leads to large losses in periods of order book fluctuations.

More complicated strategy and ideas for further realization are available [here](ideas.md)
