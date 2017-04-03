# CommonEnums

## Dir

Path in Local Pack `include/base/common_enums.h`

The enum describing a direction of deals, orders and quotes.

- BID, BUY, 0 means purchase.
- ASK, SELL, 1 means selling.

## OrderStatus

The enum class describing possible order statuses:

- Adding — the order has been created but has not been placed yet.
  This happens because of the delay between order addition request and order appearance on the market.
  This delay is called round-trip.
- Active — the order is placed.
- Deleting — there had been a request to delete the order, but it is has not been deleted yet.
  This happens because of the delay between order deletion request and order deletion on the market.
- Deleted — the order was deleted.

## ChartYAxisType

Enum class describing a side which to draw Y-axis on (left or right).

- Left — on the left side.
- Right — on the right side.

## opposite_dir

Takes a direction.

Returns the opposite direction.

{% codetabs name="C++", type="c++" -%}
inline Dir opposite_dir(Dir dir);
{%- language name="Python", type="py" -%}
def opposite_dir(dir)
{%- endcodetabs %}

## dir_sign

Takes a direction.

Returns a direction sign (1 for BID and -1 for ASK).

{% codetabs name="C++", type="c++" -%}
inline int32_t dir_sign(Dir dir);
{%- language name="Python", type="py" -%}
def dir_sign(dir)
{%- endcodetabs %}
