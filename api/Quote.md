#Quote
include/simulator/quote/quote.h



Котировка (или квота) - это термин для обозначения одного ценового уровня в стакане.
Обычно она характеризуется направлением (покупка или продажа), ценой и текущим
количеством лотов, которые доступны для покупки/продажи по данной цене.

В стратегии предлагается использовать один из методов класса OrderBook для получения
объекта типа Quote (таких как get_quote_by_idx).


* [Quote.get_volume() const](#get_volume)
* [Quote.get_price() const](#get_price)
* [Quote.get_dir() const](#get_dir)

#Основные методы класса

```cpp
inline Amount get_volume() const;
```объем лотов на цене

```cpp
inline Decimal get_price() const;
```цена

```cpp
Dir get_dir() const;
```направление котировки

