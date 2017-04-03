# ExecutionReport

Path in Local Pack `include/execution_report.h`

Report on a deal with your order.

### Methods

| Name | Description |
| --- | --- |
| [order()](#order) | A pointer to your executed order. |
| [price()](#price) | Deal price. |
| [amount()](#amount) | Deal volume. |
| [dir()](#dir) | Your executed order direction. |

### Methods description

#### order() {#order}

Returns a pointer to your order, that was matched in the deal.

{% codetabs name="C++", type="c++" -%}
Order* order() const;
{%- language name="Python", type="py" -%}
def order(self)
{%- endcodetabs %}

#### price() {#price}

Returns a price of the deal.

{% codetabs name="C++", type="c++" -%}
Price price() const;
{%- language name="Python", type="py" -%}
def price(self)
{%- endcodetabs %}

#### amount() {#amount}

Returns a volume of the deal.

{% codetabs name="C++", type="c++" -%}
Amount amount() const;
{%- language name="Python", type="py" -%}
def amount(self)
{%- endcodetabs %}

#### dir() {#dir}

Returns a direction of your executed order.

{% codetabs name="C++", type="c++" -%}
Dir dir() const;
{%- language name="Python", type="py" -%}
def dir(self)
{%- endcodetabs %}
