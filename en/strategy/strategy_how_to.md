## How to write a simple strategy

Let's write the following strategy: we will keep our [orders] (/terms.md#order) on the best price for both directions.

First, we learn how to place an order.
This is what the method:

{% codetabs name="C++", type="c++" -%}
bool add_limit_order(Dir dir, Price price, Amount amount);
{%- language name="Python", type="py" -%}
def add_limit_order(self, dir, price, amount)
{%- endcodetabs %}

Note: в **Python** methods *trading_book_update*, *trading_deals_update*, *execution_report_update* are free, so the first parameter to pass into them is an object **strat** of *ParticipantStrategy* type, using which you should call your strategy methods.

Method [add_limit_order](/api/ParticipantStrategy.md#add_limit_order) sets our [limit order](/terms.md#limit_order), with:

- *dir* — direction (BID = 0 — buy, ASK = 1 — sell).
- *price* — price to set the order.
- *amount* — order size.

When a new order book update event  `order_book`  is delivered we place our order with the [trading_book_update](/api/ParticipantStrategy.md#trading_book_update) method.

To get the best price we use best_price method [best_price](/api/OrderBook.md#best_price)  of the [OrderBook](/api/OrderBook.md) class:

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

At the moment we call  [trading_book_update](/api/ParticipantStrategy.md#trading_book_update) method, the orders which have been placed in the previous calls, could still be not executed.

Therefore it could happen that the amount of orders placed exceeds the daily limit.
Luckily, we might see all our active orders:

{% codetabs name="C++", type="c++" -%}
const auto& our_orders = order_book.orders();
{%- language name="Python", type="py" -%}
our_orders = order_book.orders()
{%- endcodetabs %}

Here we use a [orders](/api/OrderBook.md#orders) method that returns a reference to an object of [SecurityOrdersSnapshot](/api/SecurityOrdersSnapshot.md) type.



> Note 1: A variable  orders  defined above contains orders we have already sent, but have not yet sent a corresponding deletion request..
> So, if for an order a [delete_order](/api/ParticipantStrategy.md#delete_order) method is called, then upon the very next update, the  orders  variable will have no such an order even as in reality it can still be there.



> Note 2: The object  [order_book.orders()](/api/OrderBook.md#orders) is being refreshed only between updates. During a single update the structure stays unchanged.


We are going to place an order in case there is an active order at the same direction.
To find out whether an active order actually exists we use [active_orders_count(Dir dir)](/api/SecurityOrdersSnapshot.md#active_orders_count) method of the [SecurityOrdersSnapshot](/api/SecurityOrdersSnapshot.md) class. The method returns number of active orders by a given direction:

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

A drawback of this approach - if the best price is changed, but we do not react on it.
It can lead for us not to trade for a considerable time along one of the directions.
To get the price of our active order we use the [orders_by_dir()](/api/SecurityOrdersSnapshot.md#orders_by_dir) method of the  [SecurityOrdersSnapshot](/api/SecurityOrdersSnapshot.md#) class.

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
      } else {  //  if at least one of our active orders
        auto first_order = our_orders.orders_by_dir(dir).front();
        bool on_best_price = (first_order->price() == best_price);
        if (!on_best_price) {  // the order is placed, but not at the best price 
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
            if not on_best_price:  # the order is placed, but not at the best price 
                strat.delete_order(first_order)
                strat.add_limit_order(dir, best_price, amount)
{%- endcodetabs %}

Now, you can write basic strategies.

