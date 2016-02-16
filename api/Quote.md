#Quote
Путь в Local Pack-е: `include/quote/quote.h`

Котировка (или квота) - это термин для обозначения одного ценового уровня в стакане.
Она характеризуется
- направлением (покупка или продажа),
- ценой,
- объемом, т.е. количеством лотов, которые доступны для покупки/продажи по данной цене.
В стратегии предлагается использовать один из методов класса OrderBook для получения
объекта типа Quote (например, get_quote_by_idx).

###Методы

|Имя| Описание|
|------------------|--------------------|
|[get_dir()](#get_dir)|Направление котировки.|
|[get_price()](#get_price)|Цена котировки.|
|[get_volume()](#get_volume)|Объем лотов котировки.|
|[get_last_moment_ticks()](#get_last_moment_ticks)|Биржевое время последнего изменения (в сотнях наносекунд).|
|[get_last_tsc()](#get_last_tsc)|Локальное время последнего изменения (в микросекундах).|

###Описание методов
<a name="get_dir"></a>
####get_dir()
```c++
inline Dir get_dir() const;
```
Направление котировки.

<a name="get_price"></a>
####get_price()
```c++
inline Price get_price() const;
```
Цена котировки.

<a name="get_volume"></a>
####get_volume()
```c++
inline Amount get_volume() const;
```
Объем лотов котировки.

<a name="get_last_moment_ticks"></a>
####get_last_moment_ticks()
```c++
int64_t get_last_moment_ticks() const;
```
Биржевое время последнего изменения (в сотнях наносекунд).

<a name="get_last_tsc"></a>
####get_last_tsc()
```c++
int64_t get_last_tsc() const;
```
Локальное время последнего изменения (в микросекундах).


