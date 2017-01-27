# CommonEnums

## Dir

Путь в Local Pack `include/base/common_enums.h`

Enum, описывающий направление сделок, котировок.
- BID, BUY, 0 означают покупку.
- ASK, SELL, 1 означают продажу.

```c++
enum Dir : uint8_t;
```

## OrderStatus

Путь в Local Pack `include/base/common_enums.h`

Enum class, описывающий возможное состояние заявки:
- Adding — заявка была послана на добавление, но ещё не была добавлена.
  Такое происходит из-за того, что есть задержка между тем, как заявка была послана и тем, как она действительно появилась на бирже.
- Active — заявка добавлена.
- Deleting — заявка была послана на удаление, но ещё не удалена.
  Такое происходит из-за того, что есть задержка между тем, как заявка была послана на удаление и тем, как она действительно удалилась на бирже.
- Deleted — заявка была удалена.

```c++
enum class OrderStatus : uint8_t;
```

## ChartYAxisType

Путь в Local Pack `include/base/common_enums.h`

Enum class, описывающий с какой стороны рисовать ось y для графика (слева или справа).
- Left - слева.
- Right - справа.

```c++
enum class ChartYAxisType : int8_t;
```

## opposite_dir

Путь в Local Pack `include/base/common_enums.h`

Принимает направление dir (BID (покупка) или ASK (продажа)).
Возвращает противоположное направление.

```c++
inline Dir opposite_dir(Dir dir);
```

## dir_sign

Путь в Local Pack `include/base/common_enums.h`

Принимает направление dir (BID (покупка) или ASK (продажа)).
Возвращает знак этого направления (1 для BID и -1 для ASK).

```c++
inline int32_t dir_sign(Dir dir);
```
