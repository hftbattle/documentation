#ContestBookInfo
`simulator/strategy/contest_book_info.h`

Класс ContestBookInfo служит агрегатором информации по текущему состоянию стакана.
Он не является полноценной заменой стакана, но содержит часть информации о нем.
Предпочтительно пользоваться именно этим классом, например, для определения лучших цен,
это значительно быстрее, чем запрашивать у стакана.
Также именно тут можно узнать наши активные заявки, позицию и лучшие цены на биде и аске.

###Методы

|Имя| Описание|
|------------------|--------------------|
|[security_id()](#security_id)|Инструмент, которому соответствует данный класс.|
|[middle_price()](#middle_price)|Полусумма лучших цен.|
|[spread_in_min_steps()](#spread_in_min_steps)|Расстояние между лучшим аском и лучшим бидом в минимальных шагах цены.|
|[book_updates_count()](#book_updates_count)|Количество апдейтов стакана с начала дня.|
|[statistics()](#statistics)|Структура, содержащая статистику по нашей текущей позиции.|
|[server_time()](#server_time)|Биржевое время последнего обновления.|
|[min_step()](#min_step)|Минимальный шаг цены в стакане (минимальная возможная разница между ценами).|
|[best_price_by_dir()](#best_price_by_dir)|Лучшие цены (минимальная цена продажи и максимальная цена покупки) в момент последнего обновления стакана.|
|[best_volume_by_dir()](#best_volume_by_dir)|Объем лотов на лучших ценах.|
|[active_orders()](#active_orders)|Ссылка на структуру, содержащая наши активные заявки.|
|[executed_amount()](#executed_amount)|Наша текущая позиция.|
|[implied_amount()](#implied_amount)|Наша предполагаемая позиция, т.е. та, что мы ожидаем будет проторгована.|
|[total_amount()](#total_amount)|Наша общая позиция: учитывается как реальная позиция на руках, так и предполагаемая.|

###Описание методов
<a name="security_id"></a>
####security_id()
```c++
SecurityId security_id() const;
```
Инструмент, которому соответствует данный класс.

<a name="middle_price"></a>
####middle_price()
```c++
Price middle_price() const;
```
Полусумма лучших цен.

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

<a name="statistics"></a>
####statistics()
```c++
StatisticsSnapshot statistics();
```
Структура, содержащая статистику по нашей текущей позиции.

<a name="server_time"></a>
####server_time()
```c++
Microseconds server_time() const;
```
Биржевое время последнего обновления.

<a name="min_step"></a>
####min_step()
```c++
Price min_step() const;
```
Минимальный шаг цены в стакане (минимальная возможная разница между ценами).

<a name="best_price_by_dir"></a>
####best_price_by_dir()
```c++
const std::array<Price, 2>& best_price_by_dir() const;
```
Лучшие цены (минимальная цена продажи и максимальная цена покупки) в момент последнего обновления стакана.

<a name="best_volume_by_dir"></a>
####best_volume_by_dir()
```c++
const std::array<int64_t, 2>& best_volume_by_dir() const;
```
Объем лотов на лучших ценах.

<a name="active_orders"></a>
####active_orders()
```c++
SecurityOrdersSnapshot& active_orders();
```
Ссылка на структуру, содержащая наши активные заявки.

<a name="executed_amount"></a>
####executed_amount()
```c++
Amount executed_amount() const;
```
Наша текущая позиция.

<a name="implied_amount"></a>
####implied_amount()
```c++
Amount implied_amount() const;
```
Наша предполагаемая позиция, т.е. та, что мы ожидаем будет проторгована.

<a name="total_amount"></a>
####total_amount()
```c++
Amount total_amount() const;
```
Наша общая позиция: учитывается как реальная позиция на руках, так и предполагаемая.


