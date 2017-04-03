# Deal

Path in Local Pack `include/deal.h`

Description of the deal.

### Methods

| Name | Description |
| --- | --- |
| [aggressor_side()](#aggressor_side) | Direction of the aggressor order. |
| [price()](#price) | Deal price. |
| [amount()](#amount) | Deal volume. |
| [server_time()](#server_time) | Deal server time. |
| [orders()](#orders) | Orders matched in the deal. |

### Methods description

#### aggressor_side() {#aggressor_side}

Returns a direction of the aggressor order, i.e. of the order which was placed later.

{% codetabs name="C++", type="c++" -%}
Dir aggressor_side() const;
{%- language name="Python", type="py" -%}
def aggressor_side(self)
{%- endcodetabs %}

#### price() {#price}

Returns a price of the deal.

{% codetabs name="C++", type="c++" -%}
Price price() const;
{%- language name="Python", type="py" -%}
def price(self)
{%- endcodetabs %}

#### amount() {#amount}

Returns a volume of the deal, i.e. lots' volume which were executed in this deal.

{% codetabs name="C++", type="c++" -%}
Amount amount() const;
{%- language name="Python", type="py" -%}
def amount(self)
{%- endcodetabs %}

#### server_time() {#server_time}

Returns a server time of the deal execution in microseconds.

{% codetabs name="C++", type="c++" -%}
Microseconds server_time() const;
{%- language name="Python", type="py" -%}
def server_time(self)
{%- endcodetabs %}

#### orders() {#orders}

Returns an array of two pointers to orders, which were matched in this deal.

{% codetabs name="C++", type="c++" -%}
const RawOrdersArray orders() const;
{%- language name="Python", type="py" -%}
def orders(self)
{%- endcodetabs %}
