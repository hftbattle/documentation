#ContestBookInfo
Путь в Local Pack-е: `include/simulator/strategy/contest_book_info.h`

Класс ContestBookInfo служит агрегатором информации по текущему состоянию стакана.
Он не является полноценной заменой стакана, но содержит часть информации о нем.
Предпочтительно пользоваться именно этим классом, так как это значительно быстрее,
чем запрашивать у стакана.

###Методы

|Имя| Описание|
|------------------|--------------------|
|[best_price(Dir dir)](#best_price)|Лучшая цена в стакане по направлению *dir*.|
|[best_volume(Dir dir)](#best_volume)|Суммарный объем лотов на лучшей цене по направлению *dir*.|
|[executed_amount()](#executed_amount)|Наша текущая позиция.|
|[statistics_.total_amount()](#statistics_.total_amount)|Наша общая позиция: учитывается как реальная позиция на руках, так и предполагаемая.|
|[orders()](#orders)|Ссылка на структуру, содержащая наши текущие заявки.|
|[middle_price()](#middle_price)|Полусумма лучших цен.|
|[min_step()](#min_step)|Минимальный шаг цены в стакане (минимальная возможная разница между ценами).|
|[spread_in_min_steps()](#spread_in_min_steps)|Расстояние между лучшим аском и лучшим бидом в минимальных шагах цены.|
|[book_updates_count()](#book_updates_count)|Количество апдейтов стакана с начала дня.|
|[server_time()](#server_time)|Биржевое время последнего обновления.|
|[security_id()](#security_id)|Инструмент, которому соответствует данный класс.|
|[statistics()](#statistics)|Структура, содержащая статистику по нашей текущей позиции.|

###Описание методов
<a name="best_price"></a>
####best_price()
```c++
const Price best_price(Dir dir) const;
```
Лучшая цена в стакане по направлению *dir*.

<a name="best_volume"></a>
####best_volume()
```c++
const Amount best_volume(Dir dir) const;
```
Суммарный объем лотов на лучшей цене по направлению *dir*.

<a name="executed_amount"></a>
####executed_amount()
```c++
Amount executed_amount() const;
```
Наша текущая позиция.

<a name="statistics_.total_amount"></a>
####statistics_.total_amount()
```c++
Amount total_amount() const;
```
Наша общая позиция: учитывается как реальная позиция на руках, так и предполагаемая.

<a name="orders"></a>
####orders()
```c++
SecurityOrdersSnapshot& orders();
```
Ссылка на структуру, содержащая наши текущие заявки.

<a name="middle_price"></a>
####middle_price()
```c++
Price middle_price() const;
```
Полусумма лучших цен.

<a name="min_step"></a>
####min_step()
```c++
Price min_step() const;
```
Минимальный шаг цены в стакане (минимальная возможная разница между ценами).

<a name="spread_in_min_steps"></a>
####spread_in_min_steps()
```c++
int32_t spread_in_min_steps() const;
```
Расстояние между лучшим аском и лучшим бидом в минимальных шагах цены.

<a name="book_updates_count"></a>
####book_updates_count()
```c++
int32_t book_updates_count() const;
```
Количество апдейтов стакана с начала дня.

<a name="server_time"></a>
####server_time()
```c++
Microseconds server_time() const;
```
Биржевое время последнего обновления.

<a name="security_id"></a>
####security_id()
```c++
SecurityId security_id() const;
```
Инструмент, которому соответствует данный класс.

<a name="statistics"></a>
####statistics()
```c++
StatisticsSnapshot statistics();
```
Структура, содержащая статистику по нашей текущей позиции.


