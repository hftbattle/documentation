#OrderBook

`include/simulator/orderbook/order_book.h`


Стакан – это агрегатор всех заявок по конкретному финансовому инструменту (подробнее можно почитать в описании биржевых терминов). Класс OrderBook как раз и является реализацией биржевого стакана в нашем симуляторе.


 |Имя| Описание|
 |------------------|--------------------|
 |[get_quote_by_idx](#get_quote_by_idx)|котировка номер index в стакане по выбранному направлению |
 |[quote_view_by_price](#quote_view_by_price) | котировка по цене price по направлению dir |


* Основные поля и методы класса
    * [get_quote_by_idx](#get_quote_by_idx) 
    * [quote_view_by_price](#quote_view_by_price)
    * [quote_by_price](#quote_by_price)
    * [quote_by_index](#quote_by_index)
    * [all_quotes](#all_quotes)
    * [best_price](#best_price)


###get_quote_by_idx
```cpp
const Quote& get_quote_by_idx(Dir dir, int index) const;
```
котировка номер index в стакане по выбранному направлению

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

```.cpp
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

