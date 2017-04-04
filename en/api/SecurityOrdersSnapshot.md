# SecurityOrdersSnapshot

Path in Local Pack `include/security_orders_snapshot.h`

Description of your current orders.
Only orders with Adding or Active status are taken into account.
In deleting_amount_by_dir method orders with Deleting status are also counted.

The snapshot is updated before every update in strategy.
It remains unchanged during single update processing.

### Methods

| Name | Description |
| --- | --- |
| [volume()](#volume) | Volume of the orders with given price and direction. |
| [size_by_dir()](#size_by_dir) | Number of your orders with given direction. |
| [active_orders_count()](#active_orders_count) | Number of your active orders with given direction. |
| [active_orders_volume()](#active_orders_volume) | Volume of your active orders with given direction. |
| [orders_by_dir()](#orders_by_dir) | Vector of your orders with given direction. |
| [orders_by_dir_as_map()](#orders_by_dir_as_map) | Your orders with given direction as a map. |
| [deleting_amount_by_dir()](#deleting_amount_by_dir) | Volume of your orders which have been sent to deletion. |

### Methods description

#### volume() {#volume}

Takes a direction and a price.

Returns total volume of the orders with given price and direction.

{% codetabs name="C++", type="c++" -%}
Amount volume(Dir dir, Price price) const;
{%- language name="Python", type="py" -%}
def volume(self, dir, price)
{%- endcodetabs %}

#### size_by_dir() {#size_by_dir}

Takes a direction.

Returns a number of your current orders with given direction.

{% codetabs name="C++", type="c++" -%}
size_t size_by_dir(Dir dir) const;
{%- language name="Python", type="py" -%}
def size_by_dir(self, dir)
{%- endcodetabs %}

#### active_orders_count() {#active_orders_count}

Takes a direction.

Returns a number of your active orders with given direction, i.e. orders with Active status.

{% codetabs name="C++", type="c++" -%}
size_t active_orders_count(Dir dir) const;
{%- language name="Python", type="py" -%}
def active_orders_count(self, dir)
{%- endcodetabs %}

#### active_orders_volume() {#active_orders_volume}

Takes a direction.

Returns total volume of your active orders with given direction, i.e. orders with Active status.

{% codetabs name="C++", type="c++" -%}
Amount active_orders_volume(Dir dir) const;
{%- language name="Python", type="py" -%}
def active_orders_volume(self, dir)
{%- endcodetabs %}

#### orders_by_dir() {#orders_by_dir}

Takes a direction.

Returns a vector of pointers to your orders with given direction.

{% codetabs name="C++", type="c++" -%}
const Orders& orders_by_dir(Dir dir) const;
{%- language name="Python", type="py" -%}
def orders_by_dir(self, dir)
{%- endcodetabs %}

#### orders_by_dir_as_map() {#orders_by_dir_as_map}

Takes a direction.

Returns a map from price to vector of pointers to your orders with given direction.
Note: the map is always ordered by price in ascending order.

{% codetabs name="C++", type="c++" -%}
OrdersMap orders_by_dir_as_map(Dir dir) const;
{%- language name="Python", type="py" -%}
def orders_by_dir_as_map(self, dir)
{%- endcodetabs %}

Here is an example of using this method.
We iterate through all of our bids and output the current number of bids with given price.

{% codetabs name="C++", type="c++" -%}
auto orders_map = order_book.orders().orders_by_dir_as_map(BID);
for (auto it = orders_map.cbegin(); it != orders_map.cend(); ++it) {
  SCREEN() << "price: " << it->first << " amount: " << it->second.size()
}
{%- language name="Python", type="py" -%}
orders_map = order_book.orders().orders_by_dir_as_map(BID)
for price, orders in orders_map.iteritems():
    print 'price: %s amount: %s' % (price, len(orders))
{%- endcodetabs %}

#### deleting_amount_by_dir() {#deleting_amount_by_dir}

Takes a direction.

Returns a total volume of your orders with given direction, which had been sent to deletion, but have not been deleted (the ones with Deleting status) yet.

{% codetabs name="C++", type="c++" -%}
Amount deleting_amount_by_dir(Dir dir) const;
{%- language name="Python", type="py" -%}
def deleting_amount_by_dir(self, dir)
{%- endcodetabs %}
