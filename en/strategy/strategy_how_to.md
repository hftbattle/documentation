## How to write a simple strategy

Let's write the following strategy: we will keep our [orders](/terms.md#order) on the best price for both directions.

Firstly, we will learn how to place an order.
The method is:

{% codetabs name="C++", type="c++" -%}
bool add_limit_order(Dir dir, Price price, Amount amount);
{%- language name="Python", type="py" -%}
def add_limit_order(self, dir, price, amount)
{%- endcodetabs %}

Note: **Python** methods *trading_book_update*, *trading_deals_update*, *execution_report_update* are free, so the first parameter is an object **strat** of *ParticipantStrategy* type, which you may use to call your strategy methods.

Method [add_limit_order](/api/ParticipantStrategy.md#add_limit_order) places our [limit order](/terms.md#limit_order), with:

- *dir* — direction (BID = 0 — buy, ASK = 1 — sell).
- *price* — price to set the order.
- *amount* — order size.

We would place our order inside of the [trading_book_update](/api/ParticipantStrategy.md#trading_book_update) method when the `order book` is being updated.
To get the best price we can use the [best_price](/api/OrderBook.md#best_price) method of the [OrderBook](/api/OrderBook.md) class:

{% codetabs name="C++", type="c++" -%}
void trading_book_update(const OrderBook& order_book) override {
  for (Dir dir : {BID, ASK}) {
    Price best_price = order_book.best_price(dir);
    Amount amount = 1;
    add_limit_order(dir, best_price, amount);
  }
}
{%- language name="Python", type="py" -%}
def trading_book_update(strat, order_book):
    for dir in (BID, ASK):
        best_price = order_book.best_price(dir)
        amount = 1
        strat.add_limit_order(dir, best_price, amount)
{%- endcodetabs %}

When the [trading_book_update](/api/ParticipantStrategy.md#trading_book_update) method is called, the orders placed in the previous calls could still be not executed.
As a result, a large amount of our orders can be accumulated in the order book.
Luckily, we are able to get all our active orders:

{% codetabs name="C++", type="c++" -%}
const auto& our_orders = order_book.orders();
{%- language name="Python", type="py" -%}
our_orders = order_book.orders()
{%- endcodetabs %}

Here we can use the [orders](/api/OrderBook.md#orders) method which returns a reference to an object of [SecurityOrdersSnapshot](/api/SecurityOrdersSnapshot.md) type.

> Note 1: The variable `our_orders` contains orders we had already placed but have not sent a corresponding deletion request yet.
> So, if the [delete_order](/api/ParticipantStrategy.md#delete_order) method is called for an order, then `our_orders` variable will not contain this order on the next update even if it is not deleted yet in the simulator due to the round-trip delay.

> Note 2: The object [order_book.orders()](/api/OrderBook.md#orders) is only being refreshed between updates. The structure stays unchanged during a single update.

We are going to place an order in case there is no active order at the same direction.
To find out whether an active order actually exists we use [active_orders_count(Dir dir)](/api/SecurityOrdersSnapshot.md#active_orders_count) method of the [SecurityOrdersSnapshot](/api/SecurityOrdersSnapshot.md) class. The method returns the number of active orders by given direction:

{% codetabs name="C++", type="c++" -%}
void trading_book_update(const OrderBook& order_book) override {
  const auto& our_orders = order_book.orders();
  for (Dir dir : {BID, ASK}) {
    if (our_orders.active_orders_count(dir) == 0) {
      Price best_price = order_book.best_price(dir);
      Amount amount = 1;
      add_limit_order(dir, best_price, amount);
    }
  }
}
{%- language name="Python", type="py" -%}
def trading_book_update(strat, order_book):
    our_orders = order_book.orders()
    for dir in (BID, ASK):
        if our_orders.active_orders_count(dir) == 0:
            best_price = order_book.best_price(dir)
            amount = 1
            strat.add_limit_order(dir, best_price, amount)
{%- endcodetabs %}

A drawback of this approach — if the best price is changed, we will not react on it.

It can lead to the risk that we will not be able to trade in one of the directions for a considerable time.
To get the list of active orders we would use the [orders_by_dir()](/api/SecurityOrdersSnapshot.md#orders_by_dir) method of the [SecurityOrdersSnapshot](/api/SecurityOrdersSnapshot.md#) class.

The complete code of the strategy is given below:

{% codetabs name="C++", type="c++" -%}
#include "participant_strategy.h"

using namespace hftbattle;

namespace {

class UserStrategy : public ParticipantStrategy {
public:
  explicit UserStrategy(const JsonValue& config) { }

  void trading_book_update(const OrderBook& order_book) override {
    const auto& our_orders = order_book.orders();
    for (Dir dir : {BID, ASK}) {
      Price best_price = order_book.best_price(dir);
      Amount amount = 1;
      if (our_orders.active_orders_count(dir) == 0) {
        add_limit_order(dir, best_price, amount);
      } else {  // if we have some active orders
        auto first_order = our_orders.orders_by_dir(dir).front();
        bool on_best_price = (first_order->price() == best_price);
        if (!on_best_price) { // the order has been already placed, but not at the best price
          delete_order(first_order);
          add_limit_order(dir, best_price, amount);
        }
      }
    }
  }
};

}  // namespace

REGISTER_CONTEST_STRATEGY(UserStrategy, user_strategy)
{%- language name="Python", type="py" -%}
# -*- coding: utf-8 -*-

from py_defs import *
from py_defs import Decimal as Price
from common_enums import *


def trading_book_update(strat, order_book):
    our_orders = order_book.orders()
    for dir in (BID, ASK):
        best_price = order_book.best_price(dir)
        amount = 1
        if our_orders.active_orders_count(dir) == 0:
            strat.add_limit_order(dir, best_price, amount)
        else:
            first_order = our_orders.orders_by_dir(dir)[0]
            on_best_price = (first_order.price() == best_price)
            if not on_best_price:  # the order has been already placed, but not at the best price
                strat.delete_order(first_order)
                strat.add_limit_order(dir, best_price, amount)
{%- endcodetabs %}

Now, you are able to write basic strategies.
