#OrderBook
`include/simulator/orderbook/order_book.h`


Стакан - это агрегатор всех заявок по конкретному финансовому инструменту (подробнее можно почитать в описании биржевых терминов). Класс OrderBook как раз и является реализацией биржевого стакана в нашем симуляторе.


|Имя| Описание|
|------------------|--------------------|
|[OrderBook.get_quote_by_idx(Dir dir, int index) const](#get_quote_by_idx)|котировка номер index в стакане по выбранному направлению|
|[OrderBook.quote_view_by_price(Dir dir, xor_platform::Decimal price) const](#quote_view_by_price)|котировка по цене price по направлению dir|
|[OrderBook.quote_by_price(Dir dir, xor_platform::Decimal price)](#quote_by_price)|котировка по цене price по направлению dir|
|[OrderBook.quote_by_index(Dir dir, int index)](#quote_by_index)|котировка номер index в стакане по выбранному направлению|
|[OrderBook.all_quotes(Dir dir) const](#all_quotes)|все котировки по выбранному направлению|
|[OrderBook.best_price(Dir dir)](#best_price)|лучшая цена в стакане по направлению dir|
|[OrderBook.best_amount(Dir dir) const](#best_amount)|объем лучшей котировки по направлению dir|
|[OrderBook.get_volume_by_price(Dir dir, xor_platform::Decimal price) const](#get_volume_by_price)|суммарный объем лотов на цене|
|[OrderBook.get_volume_by_idx(Dir dir, int index) const](#get_volume_by_idx)|объем котировки по индексу (нумерация с нуля, начиная от лучшей цены)|
|[OrderBook.get_price_by_idx(Dir dir, int index) const](#get_price_by_idx)|цена котировки по индексу (нумерация с нуля, начиная от лучшей цены)|
|[OrderBook.index_by_price(Dir dir, xor_platform::Decimal price) const](#index_by_price)|узнать индекс (нумерация с нуля, начиная от лучшей цены) по цене|
|[OrderBook.quotes_count(Dir dir) const](#quotes_count)|количество отображаемых котировок по направлению|
|[OrderBook.contains_price(Dir dir, xor_platform::Decimal price) const](#contains_price)|есть ли такая цена в стакане по направлению|
|[OrderBook.depth() const](#depth)|максимальная глубина отображаемого стакана|

#Основные поля и методы класса

```c++
const Quote& get_quote_by_idx(Dir dir, int index) const;
```
котировка номер index в стакане по выбранному направлению

```c++
const Quote& quote_view_by_price(Dir dir, xor_platform::Decimal price) const;
```
котировка по цене price по направлению dir

```c++
Quote* quote_by_price(Dir dir, xor_platform::Decimal price);
```
котировка по цене price по направлению dir

```c++
Quote* quote_by_index(Dir dir, int index);
```
котировка номер index в стакане по выбранному направлению

```c++
QuotesHolder all_quotes(Dir dir) const;
```
все котировки по выбранному направлению

```c++
inline xor_platform::Decimal best_price(Dir dir);
```
лучшая цена в стакане по направлению dir

```c++
inline Amount best_amount(Dir dir) const;
```
объем лучшей котировки по направлению dir

```c++
inline Amount get_volume_by_price(Dir dir, xor_platform::Decimal price) const;
```
суммарный объем лотов на цене

```c++
inline Amount get_volume_by_idx(Dir dir, int index) const;
```
объем котировки по индексу (нумерация с нуля, начиная от лучшей цены)

```c++
inline xor_platform::Decimal get_price_by_idx(Dir dir, int index) const;
```
цена котировки по индексу (нумерация с нуля, начиная от лучшей цены)

```c++
size_t index_by_price(Dir dir, xor_platform::Decimal price) const;
```
узнать индекс (нумерация с нуля, начиная от лучшей цены) по цене

```c++
virtual size_t quotes_count(Dir dir) const;
```
количество отображаемых котировок по направлению

```c++
bool contains_price(Dir dir, xor_platform::Decimal price) const;
```
есть ли такая цена в стакане по направлению

```c++
size_t depth() const;
```
максимальная глубина отображаемого стакана

