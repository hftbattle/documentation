### Charts 

Charts are useful for the strategy analysis.

Key parts of this article are:

- [Profit and position charts](#sum_and_pos_chart)
- [Redirection to Viewer](#links)
- [Custom charts](#custom)

#### Profit and position charts {#sum_and_pos_chart}

<!-- TODO(asalikhov): change Chart 0 to sth. else when changed in web system -->
The default chart "Chart 0" is shown for each training submission with 2 following lines:

- *sum* — your profit during the trading session, values are marked on the left vertical axis.
- *[symbol_name]-pos* — your [position](/terms.md#position), values are marked on the right vertical axis.

<img src="{{ book["gitbook.img"] }}/base_chart.png" title="Best price chart" align="center">

> Note: charts tool uses antialiasing to display lines.
> To get exact values you can zoom in by selecting necessary area with mouse.

#### Redirection to Viewer {#links}

You can analyze the full situation in the order book in some particular moment.
For this purpose [Viewer](viewer.md) is provided: to open a moment there click the particular point of the chart.

#### Custom charts {#custom}

You can use *add_chart_point* function to add charts from your strategy:

{% codetabs name="C++", type="c++" -%}
void add_chart_point(const std::string& line_name,
                     Decimal value,  // you may use double instead of Decimal
                     ChartYAxisType y_axis_type = ChartYAxisType::Left,
                     uint8_t chart_number = 1)
{%- language name="Python", type="py" -%}
def add_chart_point(self,
                    line_name,
                    value,
                    y_axis_type = ChartYAxisType.Left,
                    chart_number = 1)
{%- endcodetabs %}

- *line_name* — unique name for each line you'd like to display.
- *value* — real number to be displayed on the chart.
- *y_axis_type* — side to display the vertical axis: *ChartYAxisType::Left* or *ChartYAxisType::Right* (ChartYAxisType.Left or ChartYAxisType.Right in *Python*).
- *chart_number* — 0 for the default chart, 1 or more for your custom charts.

Strategy standing on best price in each direction and displaying best price chart:

{% codetabs name="C++", type="c++" -%}
#include "participant_strategy.h"
#include <array>
#include <string>

using namespace hftbattle;

namespace {

class UserStrategy : public ParticipantStrategy {
public:
  explicit UserStrategy(const JsonValue& config) {
    axis_name[BID] = "best_bid";
    axis_name[ASK] = "best_ask";
  }

  void trading_book_update(const OrderBook& order_book) override {
    const auto& our_orders = order_book.orders();
    for (Dir dir : {BID, ASK}) {
      Price best_price = order_book.best_price(dir);
      Amount amount = 1;
      if (our_orders.active_orders_count(dir) == 0) {
        add_limit_order(dir, best_price, amount);
      } else {
        auto first_order = our_orders.orders_by_dir(dir).front();
        bool on_best_price = (first_order->price() == best_price);
        if (!on_best_price) {
          delete_order(first_order);
          add_limit_order(dir, best_price, amount);
        }
      }

      if (best_price != best_price_by_dir[dir]) {
        add_chart_point(axis_name[dir],
                        best_price);
        best_price_by_dir[dir] = best_price;
      }
    }
  }

private:
  std::array<Price, 2> best_price_by_dir;
  std::array<std::string, 2> axis_name;
};

}  // namespace

REGISTER_CONTEST_STRATEGY(UserStrategy, user_strategy)
{%- language name="Python", type="py" -%}
# -*- coding: utf-8 -*-

from py_defs import *
from py_defs import Decimal as Price
from common_enums import *

best_price_by_dir = [Price(), Price()]
axis_name = ["best_bid", "best_ask"]


def trading_book_update(strat, order_book):
    global best_price_by_dir
    our_orders = order_book.orders()
    for dir in (BID, ASK):
        best_price = order_book.best_price(dir)
        amount = 1
        if our_orders.active_orders_count(dir) == 0:
            strat.add_limit_order(dir, best_price, amount)
        else:
            first_order = our_orders.orders_by_dir(dir)[0]
            on_best_price = (first_order.price() == best_price)
            if not on_best_price:
                strat.delete_order(first_order)
                strat.add_limit_order(dir, best_price, amount)

        if best_price != best_price_by_dir[dir]:
            strat.add_chart_point(axis_name[dir],
                                  best_price)
            best_price_by_dir[dir] = best_price
{%- endcodetabs %}

It shows "Chart 1":

<img src="{{ book["gitbook.img"] }}/best_price_chart.png" title="Best price chart" align="center">
