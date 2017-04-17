# ParticipantStrategy

Path in Local Pack `include/participant_strategy.h`

A wrapper class for your strategy, which interacts with a trading simulator.

### Methods

| Name | Description |
| --- | --- |
| [trading_book_update()](#trading_book_update) | This method is called after getting a new order book of a trading instrument. |
| [trading_deals_update()](#trading_deals_update) | This method is called after getting new deals on a trading instrument. |
| [execution_report_update()](#execution_report_update) | This method is called after getting a report on your order execution. |
| [add_limit_order()](#add_limit_order) | Places the new limit order. |
| [add_ioc_order()](#add_ioc_order) | Places the new IOC (Immediate-Or-Cancel) order. |
| [delete_order()](#delete_order) | Sends a request to remove your order from the auction. |
| [delete_all_orders_at_dir()](#delete_all_orders_at_dir) | Sends a request to remove all your orders with given direction. |
| [delete_all_orders_at_price()](#delete_all_orders_at_price) | Sends a request to remove all your orders with given direction and price. |
| [amount_before_order()](#amount_before_order) | Number of lots before your order. |
| [volume_by_price()](#volume_by_price) | Number of lots in your active orders with given direction and price. |
| [add_chart_point()](#add_chart_point) | Adds point to the chart. |
| [current_result()](#current_result) | Strategy's current result, executed and placed orders are taken into account. |
| [server_time()](#server_time) | Current server time. |
| [server_time_tm()](#server_time_tm) | Current server time in struct tm format. |
| [set_max_total_amount()](#set_max_total_amount) | Sets the maximum position. |
| [set_stop_loss_result()](#set_stop_loss_result) | Sets the minimum strategy result. |
| [executed_amount()](#executed_amount) | Your current position, only executed orders are taken into account. |
| [fix_moment_in_viewer()](#fix_moment_in_viewer) | Saves link to Viewer in the web system after data is written there. |
| [is_our()](#is_our) | Whether this order is yours. |
| [is_our()](#is_our) | Whether this deal is yours. |
| [trading_book()](#trading_book) | Your current trading order book. |

### Methods description

#### trading_book_update() {#trading_book_update}

This method is called after getting a new order book of a trading instrument.

{% codetabs name="C++", type="c++" -%}
virtual void trading_book_update(const OrderBook& order_book);
{%- endcodetabs %}

#### trading_deals_update() {#trading_deals_update}

This method is called after getting new deals on a trading instrument.

{% codetabs name="C++", type="c++" -%}
virtual void trading_deals_update(std::vector<Deal>&& deals);
{%- endcodetabs %}

#### execution_report_update() {#execution_report_update}

This method is called after getting a report on your order execution.

{% codetabs name="C++", type="c++" -%}
virtual void execution_report_update(const ExecutionReport& execution_report);
{%- endcodetabs %}

#### add_limit_order() {#add_limit_order}

Takes a direction, a price and an amount of the new order.

Places the new limit order.

Returns bool value — was the placement successful or not.
Note: the order can be rejected if certain restrictions are not met.
Please read about it here: <https://docs.hftbattle.com/en/HFAQ.html#simulator>

{% codetabs name="C++", type="c++" -%}
bool add_limit_order(Dir dir, Price price, Amount amount) const;
{%- language name="Python", type="py" -%}
def add_limit_order(self, dir, price, amount)
{%- endcodetabs %}

#### add_ioc_order() {#add_ioc_order}

Takes a direction, a price and an amount of the new order.

Places the new IOC (Immediate-Or-Cancel) order.

Returns bool value — was the placement successful or not.
Note: the order can be rejected if certain restrictions are not met.
Please read about it here: <https://docs.hftbattle.com/en/HFAQ.html#simulator>

{% codetabs name="C++", type="c++" -%}
bool add_ioc_order(Dir dir, Price price, Amount amount) const;
{%- language name="Python", type="py" -%}
def add_ioc_order(self, dir, price, amount)
{%- endcodetabs %}

#### delete_order() {#delete_order}

Takes a pointer to your order.

Sends a request to remove this order from the auction.
Note: the deletion is not executed instantly.
You can read more about restrictions here: <https://docs.hftbattle.com/en/simulator/restrictions.html>.

{% codetabs name="C++", type="c++" -%}
void delete_order(Order* order) const;
{%- language name="Python", type="py" -%}
def delete_order(self, order)
{%- endcodetabs %}

#### delete_all_orders_at_dir() {#delete_all_orders_at_dir}

Takes a direction.

Sends a request to remove all your orders with given direction.

{% codetabs name="C++", type="c++" -%}
void delete_all_orders_at_dir(Dir dir) const;
{%- language name="Python", type="py" -%}
def delete_all_orders_at_dir(self, dir)
{%- endcodetabs %}

#### delete_all_orders_at_price() {#delete_all_orders_at_price}

Takes a direction and a price.

Sends a request to remove all your orders with given direction and price.

{% codetabs name="C++", type="c++" -%}
void delete_all_orders_at_price(Dir dir, Price price) const;
{%- language name="Python", type="py" -%}
def delete_all_orders_at_price(self, dir, price)
{%- endcodetabs %}

#### amount_before_order() {#amount_before_order}

Takes a pointer to your order.

Returns total number of lots queued before your order in the quote with given price.

{% codetabs name="C++", type="c++" -%}
Amount amount_before_order(const Order* order) const;
{%- language name="Python", type="py" -%}
def amount_before_order(self, order)
{%- endcodetabs %}

#### volume_by_price() {#volume_by_price}

Takes a direction and a price.

Returns total number of lots in your active orders with given direction and price.

{% codetabs name="C++", type="c++" -%}
Amount volume_by_price(Dir dir, Price price) const;
{%- language name="Python", type="py" -%}
def volume_by_price(self, dir, price)
{%- endcodetabs %}

#### add_chart_point() {#add_chart_point}

Takes a line_name string (name of the chart), double or Decimal value, y_axis_type — side which Y axis will be drawn on and chart_number — the number of your chart.

Adds point to the chart at the current moment with given value.
Each two adjacent points are connected.
The resulting polygonal chain is your chart.
chart_number allows you to create several charts and line_name allows you to draw several lines on the single image.

{% codetabs name="C++", type="c++" -%}
void add_chart_point(const std::string& line_name, Decimal value, ChartYAxisType y_axis_type = ChartYAxisType::Left, uint8_t chart_number = 1) const;
{%- language name="Python", type="py" -%}
def add_chart_point(self, line_name, value, 0, 1)
{%- endcodetabs %}

Here is an example of using this method.
We will add a new best bid price chart.

{% codetabs name="C++", type="c++" -%}
add_chart_point("best_bid", order_book.best_price(BID));
{%- language name="Python", type="py" -%}
strat.add_chart_point("best_bid", order_book.best_price(BID))
{%- endcodetabs %}

#### current_result() {#current_result}

Returns current result of your strategy (your profit).
Executed and placed orders are taken into account.
When calculating the result, we assume that all active orders are executed at the opposite best price.

{% codetabs name="C++", type="c++" -%}
Decimal current_result() const;
{%- language name="Python", type="py" -%}
def current_result(self)
{%- endcodetabs %}

#### server_time() {#server_time}

Returns current server time in microseconds.

{% codetabs name="C++", type="c++" -%}
Microseconds server_time() const;
{%- language name="Python", type="py" -%}
def server_time(self)
{%- endcodetabs %}

#### server_time_tm() {#server_time_tm}

Returns a server time of struct tm type with seconds precision.
Note: server time in this format is useful to determine the time of the day.

{% codetabs name="C++", type="c++" -%}
tm server_time_tm() const;
{%- language name="Python", type="py" -%}
def server_time_tm(self)
{%- endcodetabs %}

#### set_max_total_amount() {#set_max_total_amount}

Takes desired value of maximum position — non-negative number which must not be greater than 100.

Sets this position as maximum and doesn't allow your strategy to exceed it.

{% codetabs name="C++", type="c++" -%}
void set_max_total_amount(const Amount max_total_amount);
{%- language name="Python", type="py" -%}
def set_max_total_amount(self, max_total_amount)
{%- endcodetabs %}

#### set_stop_loss_result() {#set_stop_loss_result}

Takes non-positive number — desired minimum result.

If your strategy reaches this result, simulator automatically liquidates your position and stops your strategy.
Note: your position liquidation is not executed instantly, so your result may significantly differ from *stop_loss*.
You can read more here: <https://docs.hftbattle.com/en/HFAQ.html#simulator>

{% codetabs name="C++", type="c++" -%}
void set_stop_loss_result(const Decimal stop_loss_result);
{%- language name="Python", type="py" -%}
def set_stop_loss_result(self, stop_loss_result)
{%- endcodetabs %}

#### executed_amount() {#executed_amount}

Returns your current position.
Only executed orders are taken into account.

{% codetabs name="C++", type="c++" -%}
Amount executed_amount() const;
{%- language name="Python", type="py" -%}
def executed_amount(self)
{%- endcodetabs %}

#### fix_moment_in_viewer() {#fix_moment_in_viewer}

Takes any string you want to be the name of current moment.

Saves link to Viewer in the web system after data is written there.

{% codetabs name="C++", type="c++" -%}
void fix_moment_in_viewer(const std::string& name);
{%- language name="Python", type="py" -%}
def fix_moment_in_viewer(self, name)
{%- endcodetabs %}

#### is_our() {#is_our}

Takes a pointer to the order.

Returns bool value — whether this order is yours or not.

{% codetabs name="C++", type="c++" -%}
bool is_our(const Order* order) const;
{%- language name="Python", type="py" -%}
def is_our(self, order)
{%- endcodetabs %}

#### is_our() {#is_our}

Takes a reference to the deal.

Returns bool value — whether your order was matched in the deal.

{% codetabs name="C++", type="c++" -%}
bool is_our(const Deal& deal) const;
{%- language name="Python", type="py" -%}
def is_our(self, deal)
{%- endcodetabs %}

#### trading_book() {#trading_book}

Your current trading order book.
Note: you cannot get trading order book before first `trading_book_update` call.

{% codetabs name="C++", type="c++" -%}
const OrderBook& trading_book() const;
{%- language name="Python", type="py" -%}
def trading_book(self)
{%- endcodetabs %}
