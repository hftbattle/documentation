#OrderBook

Путь в Local Pack-е: `include/order_book.h`

Биржевой стакан - это агрегатор всех заявок по конкретному инструменту.
Класс OrderBook представляет собой реализацию биржевого стакана.
! Для индексов в стакане используется 0-нумерация, начиная от лучшей цены.

###Методы

|Имя| Описание|
|------------------|--------------------|
|[get_quote_by_index(Dir dir, size_t index)](#get_quote_by_index)|Котировка с индексом *index* в стакане по направлению *dir*.|
|[get_price_by_index(Dir dir, size_t index)](#get_price_by_index)|Цена котировки с индексом *index* по направлению *dir*.|
|[get_volume_by_index(Dir dir, size_t index)](#get_volume_by_index)|Суммарный объем лотов котировки с индексом *index* по направлению *dir*.|
|[get_quote_server_time_by_index(Dir dir, size_t index)](#get_quote_server_time_by_index)|Биржевое время последнего изменения котировки с индексом *index* по направлению *dir*.|
|[get_quote_by_price(Dir dir, Price price)](#get_quote_by_price)|Котировка по цене *price* по направлению *dir*.|
|[get_index_by_price(Dir dir, Price price)](#get_index_by_price)|Индекс котировки с ценой *price* по направлению *dir*.|
|[get_volume_by_price(Dir dir, Price price)](#get_volume_by_price)|Суммарный объем лотов на цене *price* по направлению *dir*.|
|[get_quote_server_time_by_price(Dir dir, Price price)](#get_quote_server_time_by_price)|Биржевое время последнего изменения котировки по цене *price* по направлению *dir*.|
|[best_price(Dir dir)](#best_price)|Лучшая цена в стакане по направлению *dir*.|
|[best_volume(Dir dir)](#best_volume)|Суммарный объем лотов на лучшей цене по направлению *dir*.|
|[contains_price(Dir dir, Price price)](#contains_price)|Есть ли в стакане цена *price* по направлению *dir*.|
|[all_quotes(Dir dir)](#all_quotes)|Все котировки по направлению *dir*.|
|[quotes_count(Dir dir)](#quotes_count)|Количество котировок по направлению *dir*.|
|[depth()](#depth)|Максимальная по направлениям глубина отображаемого стакана.|
|[get_server_time()](#get_server_time)|Биржевое время последнего изменения стакана, в микросекундах.|
|[get_local_time()](#get_local_time)|Время последнего изменения стакана на машине, которая получала данные от биржи, в микросекундах.|

###Описание методов
<a id="get_quote_by_index"></a>
####get_quote_by_index()
```c++
const Quote& get_quote_by_index(Dir dir, size_t index) const;
```
Котировка с индексом *index* в стакане по направлению *dir*.

<a id="get_price_by_index"></a>
####get_price_by_index()
```c++
Price get_price_by_index(Dir dir, size_t index) const;
```
Цена котировки с индексом *index* по направлению *dir*.

<a id="get_volume_by_index"></a>
####get_volume_by_index()
```c++
Amount get_volume_by_index(Dir dir, size_t index) const;
```
Суммарный объем лотов котировки с индексом *index* по направлению *dir*.

<a id="get_quote_server_time_by_index"></a>
####get_quote_server_time_by_index()
```c++
int64_t get_quote_server_time_by_index(Dir dir, size_t index) const;
```
Биржевое время последнего изменения котировки с индексом *index* по направлению *dir*.

<a id="get_quote_by_price"></a>
####get_quote_by_price()
```c++
const Quote& get_quote_by_price(Dir dir, Price price) const;
```
Котировка по цене *price* по направлению *dir*.

<a id="get_index_by_price"></a>
####get_index_by_price()
```c++
size_t get_index_by_price(Dir dir, Price price) const;
```
Индекс котировки с ценой *price* по направлению *dir*.

<a id="get_volume_by_price"></a>
####get_volume_by_price()
```c++
Amount get_volume_by_price(Dir dir, Price price) const;
```
Суммарный объем лотов на цене *price* по направлению *dir*.

<a id="get_quote_server_time_by_price"></a>
####get_quote_server_time_by_price()
```c++
int64_t get_quote_server_time_by_price(Dir dir, Price price) const;
```
Биржевое время последнего изменения котировки по цене *price* по направлению *dir*.

<a id="best_price"></a>
####best_price()
```c++
Price best_price(Dir dir) const;
```
Лучшая цена в стакане по направлению *dir*.

<a id="best_volume"></a>
####best_volume()
```c++
Amount best_volume(Dir dir) const;
```
Суммарный объем лотов на лучшей цене по направлению *dir*.

<a id="contains_price"></a>
####contains_price()
```c++
bool contains_price(Dir dir, Price price) const;
```
Есть ли в стакане цена *price* по направлению *dir*.

<a id="all_quotes"></a>
####all_quotes()
```c++
QuotesHolder all_quotes(Dir dir) const;
```
Все котировки по направлению *dir*.

<a id="quotes_count"></a>
####quotes_count()
```c++
virtual size_t quotes_count(Dir dir) const;
```
Количество котировок по направлению *dir*.

<a id="depth"></a>
####depth()
```c++
size_t depth() const;
```
Максимальная по направлениям глубина отображаемого стакана.

<a id="get_server_time"></a>
####get_server_time()
```c++
Microseconds get_server_time() const;
```
Биржевое время последнего изменения стакана, в микросекундах.

<a id="get_local_time"></a>
####get_local_time()
```c++
Microseconds get_local_time() const;
```
Время последнего изменения стакана на машине, которая получала данные от биржи, в микросекундах.
