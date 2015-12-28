#OrderBook

`include/simulator/orderbook/order_book.h`


Стакан - это агрегатор всех заявок по конкретному финансовому инструменту (подробнее можно почитать в описании биржевых терминов). Класс OrderBook как раз и является реализацией биржевого стакана в нашем симуляторе.


###Методы


|Имя| Описание|
|------------------|--------------------|
|[get_quote_by_idx(Dir dir, int index)](#get_quote_by_idx)|Котировка номер index в стакане по выбранному направлению.|
|[quote_view_by_price(Dir dir, Decimal price)](#quote_view_by_price)|Котировка по цене price по направлению dir.|
|[quote_by_price(Dir dir, Decimal price)](#quote_by_price)|Котировка по цене price по направлению dir.|
|[quote_by_index(Dir dir, int index)](#quote_by_index)|Котировка номер index в стакане по выбранному направлению.|
|[all_quotes(Dir dir)](#all_quotes)|Все котировки по выбранному направлению.|
|[best_price(Dir dir)](#best_price)|Лучшая цена в стакане по направлению dir.|
|[best_amount(Dir dir)](#best_amount)|Объем лучшей котировки по направлению dir.|
|[get_volume_by_price(Dir dir, Decimal price)](#get_volume_by_price)|Суммарный объем лотов на цене.|
|[get_volume_by_idx(Dir dir, int index)](#get_volume_by_idx)|Объем котировки по индексу (нумерация с нуля, начиная от лучшей цены).|
|[get_price_by_idx(Dir dir, int index)](#get_price_by_idx)|Цена котировки по индексу (нумерация с нуля, начиная от лучшей цены).|
|[index_by_price(Dir dir, Decimal price)](#index_by_price)|Узнать индекс (нумерация с нуля, начиная от лучшей цены) по цене.|
|[quotes_count(Dir dir)](#quotes_count)|Количество отображаемых котировок по направлению.|
|[contains_price(Dir dir, Decimal price)](#contains_price)|Есть ли такая цена в стакане по направлению.|
|[depth()](#depth)|Максимальная глубина отображаемого стакана.|

###Описание методов

<a id="get_quote_by_idx"></a>
####get_quote_by_idx()
```c++
const Quote& get_quote_by_idx(Dir dir, int index) const;
```
Котировка номер index в стакане по выбранному направлению.

<a id="quote_view_by_price"></a>
####quote_view_by_price()
```c++
const Quote& quote_view_by_price(Dir dir, Decimal price) const;
```
Котировка по цене price по направлению dir.

<a id="quote_by_price"></a>
####quote_by_price()
```c++
Quote* quote_by_price(Dir dir, Decimal price);
```
Котировка по цене price по направлению dir.

<a id="quote_by_index"></a>
####quote_by_index()
```c++
Quote* quote_by_index(Dir dir, int index);
```
Котировка номер index в стакане по выбранному направлению.

<a id="all_quotes"></a>
####all_quotes()
```c++
QuotesHolder all_quotes(Dir dir) const;
```
Все котировки по выбранному направлению.

<a id="best_price"></a>
####best_price()
```c++
inline Decimal best_price(Dir dir);
```
Лучшая цена в стакане по направлению dir.

<a id="best_amount"></a>
####best_amount()
```c++
inline Amount best_amount(Dir dir) const;
```
Объем лучшей котировки по направлению dir.

<a id="get_volume_by_price"></a>
####get_volume_by_price()
```c++
inline Amount get_volume_by_price(Dir dir, Decimal price) const;
```
Суммарный объем лотов на цене.

<a id="get_volume_by_idx"></a>
####get_volume_by_idx()
```c++
inline Amount get_volume_by_idx(Dir dir, int index) const;
```
Объем котировки по индексу (нумерация с нуля, начиная от лучшей цены).

<a id="get_price_by_idx"></a>
####get_price_by_idx()
```c++
inline Decimal get_price_by_idx(Dir dir, int index) const;
```
Цена котировки по индексу (нумерация с нуля, начиная от лучшей цены).

<a id="index_by_price"></a>
####index_by_price()
```c++
size_t index_by_price(Dir dir, Decimal price) const;
```
Узнать индекс (нумерация с нуля, начиная от лучшей цены) по цене.

<a id="quotes_count"></a>
####quotes_count()
```c++
virtual size_t quotes_count(Dir dir) const;
```
Количество отображаемых котировок по направлению.

<a id="contains_price"></a>
####contains_price()
```c++
bool contains_price(Dir dir, Decimal price) const;
```
Есть ли такая цена в стакане по направлению.

<a id="depth"></a>
####depth()
```c++
size_t depth() const;
```
Максимальная глубина отображаемого стакана.

