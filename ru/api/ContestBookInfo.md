# ContestBookInfo

Путь в Local Pack-е: `include/contest_book_info.h`

Класс ContestBookInfo служит агрегатором информации по текущему состоянию стакана.
Он не является полноценной заменой стакана, но содержит часть информации о нём.
Предпочтительно пользоваться именно этим классом, так как это значительно быстрее,
чем запрашивать у стакана.

### Методы

|Имя| Описание|
|------------------|--------------------|
|[best_price(Dir dir)](#best_price)|Лучшая цена в стакане по направлению *dir*.|
|[best_volume(Dir dir)](#best_volume)|Суммарный объем лотов на лучшей цене по направлению *dir*.|
|[executed_amount()](#executed_amount)|Наша текущая позиция.|
|[total_amount()](#total_amount)|Наша общая позиция: учитывается как реальная позиция на руках, так и предполагаемая.|
|[orders()](#orders)|Ссылка на структуру, содержащую наши текущие заявки.|
|[orders_const()](#orders_const)|Константная ссылка на структуру, содержащую наши текущие заявки.|
|[middle_price()](#middle_price)|Полусумма лучших цен.|
|[min_step()](#min_step)|Минимальный шаг цены в стакане (минимальная возможная разница между ценами).|
|[spread_in_min_steps()](#spread_in_min_steps)|Расстояние между лучшим аском и лучшим бидом в минимальных шагах цены.|
|[book_updates_count()](#book_updates_count)|Количество апдейтов стакана с начала дня.|
|[server_time()](#server_time)|Биржевое время последнего обновления.|
|[security_id()](#security_id)|Инструмент, которому соответствует данный класс.|
|[statistics()](#statistics)|Структура, содержащая статистику по нашей текущей позиции.|
|[get_added_volume_at_price(const Dir dir, const Price price)](#get_added_volume_at_price)|Объём добавленных заявок по направлению *dir* и цене *price*, по сравнению с предыдущим обновлением стакана.|
|[get_deleted_volume_at_price(const Dir dir, const Price price)](#get_deleted_volume_at_price)|Объём снятых заявок по направлению *dir* и цене *price*, по сравнению с предыдущим обновлением стакана.|

### Описание методов
<a id="best_price"></a>
#### best_price()
```c++
const Price best_price(Dir dir) const;
```
Лучшая цена в стакане по направлению *dir*.

<a id="best_volume"></a>
#### best_volume()
```c++
const Amount best_volume(Dir dir) const;
```
Суммарный объем лотов на лучшей цене по направлению *dir*.

<a id="executed_amount"></a>
#### executed_amount()
```c++
Amount executed_amount() const;
```
Наша текущая позиция.

<a id="total_amount"></a>
#### total_amount()
```c++
Amount total_amount() const;
```
Наша общая позиция: учитывается как реальная позиция на руках, так и предполагаемая.

<a id="orders"></a>
#### orders()
```c++
SecurityOrdersSnapshot& orders();
```
Ссылка на структуру, содержащую наши текущие заявки.

<a id="orders_const"></a>
#### orders_const()
```c++
const SecurityOrdersSnapshot& orders_const() const;
```
Константная ссылка на структуру, содержащую наши текущие заявки.

<a id="middle_price"></a>
#### middle_price()
```c++
Price middle_price() const;
```
Полусумма лучших цен.

<a id="min_step"></a>
#### min_step()
```c++
Price min_step() const;
```
Минимальный шаг цены в стакане (минимальная возможная разница между ценами).

<a id="spread_in_min_steps"></a>
#### spread_in_min_steps()
```c++
int32_t spread_in_min_steps() const;
```
Расстояние между лучшим аском и лучшим бидом в минимальных шагах цены.

<a id="book_updates_count"></a>
#### book_updates_count()
```c++
int32_t book_updates_count() const;
```
Количество апдейтов стакана с начала дня.

<a id="server_time"></a>
#### server_time()
```c++
Microseconds server_time() const;
```
Биржевое время последнего обновления.

<a id="security_id"></a>
#### security_id()
```c++
SecurityId security_id() const;
```
Инструмент, которому соответствует данный класс.

<a id="statistics"></a>
#### statistics()
```c++
StatisticsSnapshot statistics();
```
Структура, содержащая статистику по нашей текущей позиции.

<a id="get_added_volume_at_price"></a>
#### get_added_volume_at_price()
```c++
Amount get_added_volume_at_price(const Dir dir, const Price price) const;
```
Объём добавленных заявок по направлению *dir* и цене *price*, по сравнению с предыдущим обновлением стакана.

<a id="get_deleted_volume_at_price"></a>
#### get_deleted_volume_at_price()
```c++
Amount get_deleted_volume_at_price(const Dir dir, const Price price) const;
```
Объём снятых заявок по направлению *dir* и цене *price*, по сравнению с предыдущим обновлением стакана.
