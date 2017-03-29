# A sample strategy


- [Description of the strategy and its source code](#strategy)
- [The strategy analysis](#analysis)
- [Ideas to implement](#ideas)



## Description of the strategy and its source code {#strategy}

The idea of the strategy is to keep one order on the best price at each direction (BID и ASK) with the price of `middle_price`and a constant offset `offset` in the corresponding direction.
In case a direction has none of our active orders, the strategy places an order with volume of `volume`.
In case an order exists, but happens to be of other price, we cancel it and replace it on the price we need.
The volume of the placed order can be decreased for us not to get out of the maximum volume size.
For HFT strategies being able to quickly nullify the position amount, is very important. So, we will try to limit the maximum amount of the open position to `max_pos` number.

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

Since the strategy keeps only 1 order with a volume of 1 lot for each direction, it does not make lots of deals in a given one. 
On the other hand, if a limit is set tight the strategy will not gather a volume so big for it to lose large sum of money when the volatility increases.

## Ideas to implement {#ideas}

A serious factor that limits the strategy result - maximum position volume set low.
Increase this parameter and your strategy will make a larger number of deals, so your profit rises proportionally.
On the other hand the bigger your maximum position volume the longer your strategy keeps a position in one direction and this affects negatively on the overall result.
2. Current strategy keeps just 1 order on 1 price level for each direction. Go in for several different price levels in the order book and you make much more profitable deals. 
3. It has already been said that it’s very important to quickly close the position.
One of such methods is a dynamic price and volume selection for new orders being set. 
The larger the current position along a given direction the more active you need to act in the opposite direction and refrain from acting in the current direction. 
4. Keep an eye on “quoting” orders on each direction and do not forget to collect “foreign volume” that is in the order book. 
You may do it both to reduce the current position and if noticed that another participant stays on unfair price. 
This is just a part of our experience we wanted to share. 
During the contest new ideas will come up and we will describe them in details.
