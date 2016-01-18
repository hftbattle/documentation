#OrderBook
`simulator/orderbook/order_book.h`

Биржевой стакан - это агрегатор всех заявок по конкретному инструменту.
Класс OrderBook представляет собой реализацию биржевого стакана.
Для котировок в стакане используется 0-нумерация, начиная от лучшей цены.

###Методы

|Имя| Описание|
|------------------|--------------------|
|[quote_view_by_idx(Dir dir, int index)](#quote_view_by_idx)|Котировка с индексом @index в стакане по направлению @dir.|
|[quote_view_by_price(Dir dir, Price price)](#quote_view_by_price)|Котировка по цене @price по направлению @dir.|
|[all_quotes(Dir dir)](#all_quotes)|Все котировки по направлению @dir.|
|[best_price(Dir dir)](#best_price)|Лучшая цена в стакане по направлению @dir.|
|[best_volume(Dir dir)](#best_volume)|Суммарный объем лотов на лучшей цене по направлению @dir.|
|[get_volume_by_idx(Dir dir, int index)](#get_volume_by_idx)|Суммарный объем лотов котировки с индексом @index по направлению @dir.|
|[get_volume_by_price(Dir dir, Price price)](#get_volume_by_price)|Суммарный объем лотов на цене @price по направлению @dir.|
|[get_price_by_idx(Dir dir, int index)](#get_price_by_idx)|Цена котировки с индексом @index по направлению @dir.|
|[index_by_price(Dir dir, Price price)](#index_by_price)|Индекс котировки с ценой @price по направлению @dir.|
|[quotes_count(Dir dir)](#quotes_count)|Количество котировок по направлению @dir.|
|[contains_price(Dir dir, Price price)](#contains_price)|Есть ли в стакане цена @price по направлению @dir.|
|[depth()](#depth)|Максимальная глубина отображаемого стакана.|
|[get_quote_server_time(Dir dir, int index)](#get_quote_server_time)|Биржевое время последнего изменения котировки с индексом @index по направлению @dir.|

###Описание методов
<a id="quote_view_by_idx"></a>
####quote_view_by_idx()
```c++
const Quote& quote_view_by_idx(Dir dir, int index) const;
```
Котировка с индексом @index в стакане по направлению @dir.

<a id="quote_view_by_price"></a>
####quote_view_by_price()
```c++
const Quote& quote_view_by_price(Dir dir, Price price) const;
```
Котировка по цене @price по направлению @dir.

<a id="all_quotes"></a>
####all_quotes()
```c++
QuotesHolder all_quotes(Dir dir) const 
```
Все котировки по направлению @dir.

<a id="best_price"></a>
####best_price()
```c++
inline Price best_price(Dir dir) const 
```
Лучшая цена в стакане по направлению @dir.

<a id="best_volume"></a>
####best_volume()
```c++
inline Amount best_volume(Dir dir) const 
```
Суммарный объем лотов на лучшей цене по направлению @dir.

<a id="get_volume_by_idx"></a>
####get_volume_by_idx()
```c++
inline Amount get_volume_by_idx(Dir dir, int index) const 
```
Суммарный объем лотов котировки с индексом @index по направлению @dir.

<a id="get_volume_by_price"></a>
####get_volume_by_price()
```c++
inline Amount get_volume_by_price(Dir dir, Price price) const 
```
Суммарный объем лотов на цене @price по направлению @dir.

<a id="get_price_by_idx"></a>
####get_price_by_idx()
```c++
inline Price get_price_by_idx(Dir dir, int index) const 
```
Цена котировки с индексом @index по направлению @dir.

<a id="index_by_price"></a>
####index_by_price()
```c++
size_t index_by_price(Dir dir, Price price) const;
```
Индекс котировки с ценой @price по направлению @dir.

<a id="quotes_count"></a>
####quotes_count()
```c++
virtual size_t quotes_count(Dir dir) const;
```
Количество котировок по направлению @dir.

<a id="contains_price"></a>
####contains_price()
```c++
bool contains_price(Dir dir, Price price) const;
```
Есть ли в стакане цена @price по направлению @dir.

<a id="depth"></a>
####depth()
```c++
size_t depth() const;
```
Максимальная глубина отображаемого стакана.

<a id="get_quote_server_time"></a>
####get_quote_server_time()
```c++
inline int64_t get_quote_server_time(Dir dir, int index) const 
```
Биржевое время последнего изменения котировки с индексом @index по направлению @dir.


