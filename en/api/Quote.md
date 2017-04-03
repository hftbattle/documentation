# Quote

## default_quote_price

Path in Local Pack `include/quote.h`

A default price to describe an empty level.
Takes a direction.

Returns a default price for the quote with given direction.

Note: if the quote doesn't exist, then it's price is equal to default_quote_price(dir).

## Quote

Path in Local Pack `include/quote.h`

Description of a price level in the order book.

### Methods

| Name | Description |
| --- | --- |
| [dir()](#dir) | Quote direction. |
| [price()](#price) | Quote price. |
| [volume()](#volume) | Quote volume. |
| [server_time()](#server_time) | Quote latest change server time. |

### Methods description

#### dir() {#dir}

Returns a direction of the quote.

{% codetabs name="C++", type="c++" -%}
Dir dir() const;
{%- language name="Python", type="py" -%}
def dir(self)
{%- endcodetabs %}

#### price() {#price}

Returns a price of the quote.

{% codetabs name="C++", type="c++" -%}
Price price() const;
{%- language name="Python", type="py" -%}
def price(self)
{%- endcodetabs %}

#### volume() {#volume}

Returns total volume of lots in orders in this quote.

{% codetabs name="C++", type="c++" -%}
Amount volume() const;
{%- language name="Python", type="py" -%}
def volume(self)
{%- endcodetabs %}

#### server_time() {#server_time}

Returns the latest server time, when the quote has been changed, in microseconds.

{% codetabs name="C++", type="c++" -%}
Microseconds server_time() const;
{%- language name="Python", type="py" -%}
def server_time(self)
{%- endcodetabs %}
