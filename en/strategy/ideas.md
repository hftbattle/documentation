
## Improved strategy{#improved}

In [base strategy](base_strategy.md) we placed our orders on the `middle_price` with fixed `offset`.
However, this behaviour is not acceptable, for example, when the best price is unfair in some direction.
The `middle_price` is very sensitive to order book fluctuations: a new order placed before the previous best price level in some direction can change the `middle_price`, causing our strategy to place orders in wrong price levels.
Particularly, the `middle_price` will be dramatically changed if an order with value of 1 lot will be placed far above the previous best bid price.
It is clear that this order does not show the real instrument bid price in most cases. Here, by the way, it is reasonable to think about the "better" `middle_price` evaluation.

Note that the order book of given instrument is rarefied so there may be large gaps (i.e. large number of price levels without any order) and also many quotes with a small volume.
Let's remember that. It is quite clear that standing at the free price level is generally better than standing at the occupied one, because of the higher queue position.

Let's keep our orders on such a price, that will give us a fixed accumulated volume of orders standing on better prices. `volume_before_our_order` parameter will be used for this.
This allows us to exclude the possibility of staying on unfair price.

If we implement this strategy, we will face two problems:

1. Staying on the price reached by `volume_before_our_order` will certainly keep us behind the other's orders.
2. We should ensure that our orders (bid and ask) would not be placed too close to each other in the order book.
   Having both orders executed almost at the same time, we will be charged with a double fee, risking to lose money.

Here is the possible solution of these problems:

1. We would stay on the price, which is closer to `middle_price` by one `min_step` from the one achieved by `volume_before_our_order` constraint.
   There is a possibility of standing on a free price, which is definitely good for us. This is a simplified version of more complicated idea:
   generally, the price should be chosen in a such way, that will allow us to stay at price level which volume is significantly less than the volume of the next price.
2. We would not place the order, if its price difference with the opposite best price is less than some predetermined value.
   It is reasonable to change this later, when this constraint would not allow us to keep orders on some direction.
   Let it be called `offset`.

Finally we will get the following strategy:

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

## Ideas to implement {#ideas}

1. A serious factor limiting the strategy result is the low maximum position.
   Increasing this parameter will make your strategy create a larger number of deals, increasing your profit almost proportionally.
   On the other hand the larger maximum position value you set the longer your strategy keeps one-directed position affecting negatively on the overall result.
2. Current strategy keeps just 1 order on 1 price level for each direction.
   Keeping you orders on different price levels would allow you to make much more profitable deals.
3. It has already been said that it’s very important to close the position quickly.
   One of the methods to achieve this strategy behaviour is a dynamic price and volume selection for the new orders.
   The larger current position along a direction is, the more active you should be in the opposite direction and, on the contrary, less active in the current direction.
4. In addition to maintaining the “quoting” strategy, you can beat other participants' orders in the order book.
   You may do it both to reduce the current position and to collect other participants' orders staying on unfair price.
5. It is worth thinking about what price can be treated as fair in each time period.
   `middle_price` is a bad indicator so you should watch carefully for other participants' actions in the order book on any update.
6. The strategy above has a problem - it takes its orders into account while processing volumes at the quotes.
   This may lead to unexpected situations such as moving the target price level for the new orders because of the strategy's previous orders in the order book.

New ideas will come up during the contest and we are going to provide you the detailed overview.