# Order

Path in Local Pack `include/order.h`

Description of the market order.

### Methods

| Name | Description |
| --- | --- |
| [id()](#id) | Unique order identifier. |
| [dir()](#dir) | Order direction. |
| [price()](#price) | Order price. |
| [amount()](#amount) | Order initial amount. |
| [amount_rest()](#amount_rest) | Order current amount. |
| [status()](#status) | Order status. |
| [server_time()](#server_time) | Order placement server time. |

### Methods description

#### id() {#id}

Returns the unique numeric identifier of an order, which was received during a simulation.
It can be used to aggregate some information about the order.

{% codetabs name="C++", type="c++" -%}
Id id() const;
{%- language name="Python", type="py" -%}
def id(self)
{%- endcodetabs %}

#### dir() {#dir}

Returns a direction of the order.

{% codetabs name="C++", type="c++" -%}
Dir dir() const;
{%- language name="Python", type="py" -%}
def dir(self)
{%- endcodetabs %}

#### price() {#price}

Returns a price of the order.

{% codetabs name="C++", type="c++" -%}
Price price() const;
{%- language name="Python", type="py" -%}
def price(self)
{%- endcodetabs %}

#### amount() {#amount}

Returns an initial volume of the order (the amount of lots).

{% codetabs name="C++", type="c++" -%}
Amount amount() const;
{%- language name="Python", type="py" -%}
def amount(self)
{%- endcodetabs %}

#### amount_rest() {#amount_rest}

Returns current volume of the order (the amount of lots which have not been matched with other orders yet).

{% codetabs name="C++", type="c++" -%}
Amount amount_rest() const;
{%- language name="Python", type="py" -%}
def amount_rest(self)
{%- endcodetabs %}

#### status() {#status}

Returns an enum class value — order's status.

Possible statuses are: Adding, Active, Deleting, Deleted.
Please read more in the OrderStatus class description: <https://docs.hftbattle.com/en/api/CommonEnums.html#orderstatus>.

{% codetabs name="C++", type="c++" -%}
OrderStatus status() const;
{%- language name="Python", type="py" -%}
def status(self)
{%- endcodetabs %}

#### server_time() {#server_time}

Returns the server time, when the order was placed, in microseconds.

{% codetabs name="C++", type="c++" -%}
Microseconds server_time() const;
{%- language name="Python", type="py" -%}
def server_time(self)
{%- endcodetabs %}
