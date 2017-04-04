# OrderBook

Path in Local Pack `include/order_book.h`

This class aggregates all orders for a specific instrument.
Note: indices are counted from 0, beginning from the best price, i.e. in descending order for BID (Buy) and in ascending order for ASK (Sell).

**Only non-empty quotes** are taken into account.
So if a quote has index 3, that doesn't mean that it's price differs in exactly 3 minimum steps from the best price.

### Methods

| Name | Description |
| --- | --- |
| [quote_by_index()](#quote_by_index) | Quote with given direction and index. |
| [price_by_index()](#price_by_index) | Price of the quote with given direction and index. |
| [volume_by_index()](#volume_by_index) | Total volume of the orders in the quote with given direction and index. |
| [quote_by_price()](#quote_by_price) | Quote with given direction and price. |
| [index_by_price()](#index_by_price) | Index of the quote with given direction and price. |
| [volume_by_price()](#volume_by_price) | Volume of the quote with given direction and price. |
| [best_price()](#best_price) | Best price in the order book by given direction. |
| [best_volume()](#best_volume) | Volume of the quote with the best price by given direction. |
| [all_quotes()](#all_quotes) | All quotes with given direction as a QuotesHolder object. |
| [quotes_count()](#quotes_count) | Number of non-empty quotes with given direction |
| [depth()](#depth) | Maximum number of quote levels you can see in order book. |
| [server_time()](#server_time) | Order book latest update server time. |
| [orders()](#orders) | Reference to your orders as a SecurityOrdersSnapshot object. |
| [middle_price()](#middle_price) | Half-sum of the best prices. |
| [min_step()](#min_step) | Minimum price step. |
| [spread_in_min_steps()](#spread_in_min_steps) | Distance between the best prices in minimum steps. |
| [fee_per_lot()](#fee_per_lot) | Fee per one executed lot. |
| [book_updates_count()](#book_updates_count) | Order book updates since the beginning of the trading session. |
| [added_volume_at_price()](#added_volume_at_price) | Volume of the orders added since the previous order book update. |
| [deleted_volume_at_price()](#deleted_volume_at_price) | Volume of the orders deleted since the previous order book update. |

### Methods description

#### quote_by_index() {#quote_by_index}

Takes a direction and an index.

Returns a quote with given direction and index.

{% codetabs name="C++", type="c++" -%}
const Quote& quote_by_index(Dir dir, size_t index) const;
{%- language name="Python", type="py" -%}
def quote_by_index(self, dir, index)
{%- endcodetabs %}

#### price_by_index() {#price_by_index}

Takes a direction and an index.

Returns a price of the quote with given direction and index.
Note: if the quote doesn't exist, corresponding default_quote_price is returned.

{% codetabs name="C++", type="c++" -%}
Price price_by_index(Dir dir, size_t index) const;
{%- language name="Python", type="py" -%}
def price_by_index(self, dir, index)
{%- endcodetabs %}

#### volume_by_index() {#volume_by_index}

Takes a direction and an index.

Returns a volume of the quote with given direction and index.

{% codetabs name="C++", type="c++" -%}
Amount volume_by_index(Dir dir, size_t index) const;
{%- language name="Python", type="py" -%}
def volume_by_index(self, dir, index)
{%- endcodetabs %}

#### quote_by_price() {#quote_by_price}

Takes a direction and a price.

Returns a quote with given direction and price.

{% codetabs name="C++", type="c++" -%}
const Quote& quote_by_price(Dir dir, Price price) const;
{%- language name="Python", type="py" -%}
def quote_by_price(self, dir, price)
{%- endcodetabs %}

#### index_by_price() {#index_by_price}

Takes a direction and a price.

Returns an index of the quote with given direction and price.
If the quote with given price doesn't exist, maximum size_t value (i.e. `std::numeric_limits<size_t>::max()`) is returned.

{% codetabs name="C++", type="c++" -%}
size_t index_by_price(Dir dir, Price price) const;
{%- language name="Python", type="py" -%}
def index_by_price(self, dir, price)
{%- endcodetabs %}

#### volume_by_price() {#volume_by_price}

Takes a direction and a price.

Returns a volume of the quote with given direction and price.

{% codetabs name="C++", type="c++" -%}
Amount volume_by_price(Dir dir, Price price) const;
{%- language name="Python", type="py" -%}
def volume_by_price(self, dir, price)
{%- endcodetabs %}

#### best_price() {#best_price}

Takes a direction.

Returns the best price in order book with given direction.
Note: if the quote doesn't exist, corresponding default_quote_price is returned.

{% codetabs name="C++", type="c++" -%}
Price best_price(Dir dir) const;
{%- language name="Python", type="py" -%}
def best_price(self, dir)
{%- endcodetabs %}

#### best_volume() {#best_volume}

Takes a direction.

Returns a volume of the quote with the best price by given direction.

{% codetabs name="C++", type="c++" -%}
Amount best_volume(Dir dir) const;
{%- language name="Python", type="py" -%}
def best_volume(self, dir)
{%- endcodetabs %}

#### all_quotes() {#all_quotes}

Takes a direction.

Returns all quotes with given direction as a QuotesHolder object.
QuotesHolder is a container, which allows you to iterate you through all quotes.
Please read more about QuotesHolder here: <https://docs.hftbattle.com/en/api/QuotesHolder.html>.

{% codetabs name="C++", type="c++" -%}
QuotesHolder all_quotes(Dir dir) const;
{%- language name="Python", type="py" -%}
def all_quotes(self, dir)
{%- endcodetabs %}

Here is an example of using this method.
We iterate through all quotes with BID direction and print the current number of bids with given price.

{% codetabs name="C++", type="c++" -%}
for (const auto& quote : book.all_quotes(BID)) {
  SCREEN() << "price: " << quote.price() << " volume: " << quote.volume();
}
{%- language name="Python", type="py" -%}
for quote in order_book.all_quotes(BID):
    print 'price: %s amount: %s' % (quote.price(), quote.volume())
{%- endcodetabs %}

#### quotes_count() {#quotes_count}

Takes a direction.

Returns a number of non-empty quotes with given direction.

{% codetabs name="C++", type="c++" -%}
size_t quotes_count(Dir dir) const;
{%- language name="Python", type="py" -%}
def quotes_count(self, dir)
{%- endcodetabs %}

#### depth() {#depth}

Returns a maximum number of visible quote levels in the order book (it's the same for both directions).

{% codetabs name="C++", type="c++" -%}
size_t depth() const;
{%- language name="Python", type="py" -%}
def depth(self)
{%- endcodetabs %}

#### server_time() {#server_time}

Returns the latest server time, when the order book has been updated, in microseconds.

{% codetabs name="C++", type="c++" -%}
Microseconds server_time() const;
{%- language name="Python", type="py" -%}
def server_time(self)
{%- endcodetabs %}

#### orders() {#orders}

Returns a reference to a SecurityOrdersSnapshot object containing all your current quotes.
Please read more about SecurityOrdersSnapshot here: <https://docs.hftbattle.com/en/api/SecurityOrdersSnapshot.html>.

{% codetabs name="C++", type="c++" -%}
const SecurityOrdersSnapshot& orders() const;
{%- language name="Python", type="py" -%}
def orders(self)
{%- endcodetabs %}

#### middle_price() {#middle_price}

Returns a half-sum of the best prices in both directions.

{% codetabs name="C++", type="c++" -%}
Price middle_price() const;
{%- language name="Python", type="py" -%}
def middle_price(self)
{%- endcodetabs %}

#### min_step() {#min_step}

Returns a minimum price step in the order book (the least possible difference between prices).

{% codetabs name="C++", type="c++" -%}
Price min_step() const;
{%- language name="Python", type="py" -%}
def min_step(self)
{%- endcodetabs %}

#### spread_in_min_steps() {#spread_in_min_steps}

Returns a distance between the best buy and best sell prices in minimum steps.

{% codetabs name="C++", type="c++" -%}
size_t spread_in_min_steps() const;
{%- language name="Python", type="py" -%}
def spread_in_min_steps(self)
{%- endcodetabs %}

#### fee_per_lot() {#fee_per_lot}

Returns a fee per one executed lot.

{% codetabs name="C++", type="c++" -%}
Decimal fee_per_lot() const;
{%- language name="Python", type="py" -%}
def fee_per_lot(self)
{%- endcodetabs %}

#### book_updates_count() {#book_updates_count}

Returns a number of the order book updates since the beginning of the trading session.
Note: research of the change of this value can be used to understand the market activity.

{% codetabs name="C++", type="c++" -%}
size_t book_updates_count() const;
{%- language name="Python", type="py" -%}
def book_updates_count(self)
{%- endcodetabs %}

#### added_volume_at_price() {#added_volume_at_price}

Takes a direction and a price.

Returns a volume of the added orders with given direction and price in comparison with the previous order book update.

{% codetabs name="C++", type="c++" -%}

{%- endcodetabs %}

#### deleted_volume_at_price() {#deleted_volume_at_price}

Takes a direction and a price.

Returns a volume of the deleted orders with given direction and price in comparison with the previous order book update.

{% codetabs name="C++", type="c++" -%}

{%- endcodetabs %}
