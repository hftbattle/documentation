# CommonEnums

## Dir

Путь в Local Pack `include/base/common_enums.h`

Enum, описывающий направления сделок, заявок и котировок.

- BID, BUY, 0 означают покупку.
- ASK, SELL, 1 означают продажу.

## OrderStatus

Enum class, описывающий возможные состояния заявки:

- Adding — заявка была послана на добавление, но ещё не была добавлена.
  Такое происходит из-за того, что существует задержка между тем, как заявка была послана, и тем, как она действительно появилась на бирже.
  Эта задержка называется round-trip.
- Active — заявка добавлена.
- Deleting — заявка была послана на удаление, но ещё не удалена.
  Такое происходит из-за того, что существует задержка между тем, как заявка была послана на удаление, и тем, как она действительно удалилась на бирже.
- Deleted — заявка была удалена.

## ChartYAxisType

Enum class, описывающий сторону, с которой нужно отобразить ось Y для графика (слева или справа).

- Left — слева.
- Right — справа.

## opposite_dir

Принимает направление dir.

Возвращает противоположное направление.

{% codetabs name="C++", type="c++" -%}
inline Dir opposite_dir(Dir dir);
{%- language name="Python", type="py" -%}
def opposite_dir(dir)
{%- endcodetabs %}

## dir_sign

Принимает направление dir.

Возвращает знак этого направления (1 для BID и -1 для ASK).

{% codetabs name="C++", type="c++" -%}
inline int32_t dir_sign(Dir dir);
{%- language name="Python", type="py" -%}
def dir_sign(dir)
{%- endcodetabs %}
