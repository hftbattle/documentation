#Quote
`include/simulator/quote/quote.h`


Котировка (или квота) - это термин для обозначения одного ценового уровня в стакане.
Обычно она характеризуется направлением (покупка или продажа), ценой и текущим
количеством лотов, которые доступны для покупки/продажи по данной цене.

В стратегии предлагается использовать один из методов класса OrderBook для получения
объекта типа Quote (таких как get_quote_by_idx).


|Имя| Описание|
|------------------|--------------------|
|[Quote.get_volume() const](#get_volume)|объем лотов на цене|
|[Quote.get_price() const](#get_price)|цена|
|[Quote.get_dir() const](#get_dir)|направление котировки|

#Основные методы класса

<a id="get_volume"></a>
###get_volume
```c++
inline Amount get_volume() const;
```
объем лотов на цене

<a id="get_price"></a>
###get_price
```c++
inline Decimal get_price() const;
```
цена

<a id="get_dir"></a>
###get_dir
```c++
Dir get_dir() const;
```
направление котировки

