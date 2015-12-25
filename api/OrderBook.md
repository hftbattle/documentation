#OrderBook
include/simulator/orderbook/order_book.h



Стакан - это агрегатор всех заявок по конкретному финансовому инструменту (подробнее можно почитать в описании биржевых терминов). Класс OrderBook как раз и является реализацией биржевого стакана в нашем симуляторе.


* [OrderBook.get_quote_by_idx(Dir dir, int index) const](#get_quote_by_idx)
* [OrderBook.quote_view_by_price(Dir dir, xor_platform::Decimal price) const](#quote_view_by_price)
* [OrderBook.quote_by_price(Dir dir, xor_platform::Decimal price)](#quote_by_price)
* [OrderBook.quote_by_index(Dir dir, int index)](#quote_by_index)
* [OrderBook.all_quotes(Dir dir) const](#all_quotes)
* [OrderBook.best_price(Dir dir)](#best_price)
* [OrderBook.best_amount(Dir dir) const](#best_amount)
* [OrderBook.get_volume_by_price(Dir dir, xor_platform::Decimal price) const](#get_volume_by_price)
* [OrderBook.get_volume_by_idx(Dir dir, int index) const](#get_volume_by_idx)
* [OrderBook.get_price_by_idx(Dir dir, int index) const](#get_price_by_idx)
* [OrderBook.index_by_price(Dir dir, xor_platform::Decimal price) const](#index_by_price)
* [OrderBook.quotes_count(Dir dir) const](#quotes_count)
* [OrderBook.contains_price(Dir dir, xor_platform::Decimal price) const](#contains_price)
* [OrderBook.depth() const](#depth)

#Основные поля и методы класса

```cpp
const Quote& get_quote_by_idx(Dir dir, int index) const;
```котировка номер index в стакане по выбранному направлению

```cpp
const Quote& quote_view_by_price(Dir dir, xor_platform::Decimal price) const;
```котировка по цене price по направлению dir

```cpp
Quote* quote_by_price(Dir dir, xor_platform::Decimal price);
```котировка по цене price по направлению dir

```cpp
Quote* quote_by_index(Dir dir, int index);
```котировка номер index в стакане по выбранному направлению

```cpp
QuotesHolder all_quotes(Dir dir) const;
```все котировки по выбранному направлению

```cpp
inline xor_platform::Decimal best_price(Dir dir);
```лучшая цена в стакане по направлению dir

```cpp
inline Amount best_amount(Dir dir) const;
```объем лучшей котировки по направлению dir

```cpp
inline Amount get_volume_by_price(Dir dir, xor_platform::Decimal price) const;
```суммарный объем лотов на цене

```cpp
inline Amount get_volume_by_idx(Dir dir, int index) const;
```объем котировки по индексу (нумерация с нуля, начиная от лучшей цены)

```cpp
inline xor_platform::Decimal get_price_by_idx(Dir dir, int index) const;
```цена котировки по индексу (нумерация с нуля, начиная от лучшей цены)

```cpp
size_t index_by_price(Dir dir, xor_platform::Decimal price) const;
```узнать индекс (нумерация с нуля, начиная от лучшей цены) по цене

```cpp
virtual size_t quotes_count(Dir dir) const;
```количество отображаемых котировок по направлению

```cpp
bool contains_price(Dir dir, xor_platform::Decimal price) const;
```есть ли такая цена в стакане по направлению

```cpp
size_t depth() const;
```максимальная глубина отображаемого стакана

