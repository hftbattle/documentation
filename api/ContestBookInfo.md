#ContestBookInfo

`include/simulator/strategy/contest_book_info.h`


Класс ContestBookInfo служит агрегатором информации по текущему состоянию стакана. Он не является полноценной заменой стакана, но содержит часть информации о нем. Предпочтительно пользоваться именно этим классом, например, для определения лучших цен, это значительно быстрее, чем запрашивать у стакана. Также именно тут можно узнать наши активные [заявки](../docs/glossary.md#order), [позицию](../docs/glossary.md#position) и [сальдо](../docs/glossary.md#saldo).


###Методы


|Имя| Описание|
|------------------|--------------------|
|[security_id()](#security_id)|Инструмент которому соответствует данный класс.|
|[active_orders()](#active_orders)|Структура, содержащая наши заявки. Отдельно описана в разделе [SecurityOrdersSnapshot](SecurityOrdersSnapshot.md).|
|[total_amount()](#total_amount)|Наша "предполагаемая" позиция - учитывается и реальная позиция на руках, и та, что мы ожидаем что будет проторгована.|
|[executed_amount()](#executed_amount)|Наша текущая позиция.|
|[best_price()](#best_price)|Лучшие цены (минимальная цена продажи и максимальная цена покупки) в момент последнего обновления стакана.|
|[best_price_volume()](#best_price_volume)|Объем лотов на лучших ценах.|
|[middle_price()](#middle_price)|Полусумма лучших цен.|
|[spread()](#spread)|Расстояние между лучшим аском и лучшим бидом в минимальных шагах цены.|
|[book_updates_count()](#book_updates_count)|Количество апдейтов стакана с начала дня.|
|[statistics()](#statistics)|Структура, содержащая статистику по нашей текущей позиции.|
|[server_time()](#server_time)|Биржевое время последнего апдейта.|
|[min_step()](#min_step)|Минимальный шаг цены в стакане (минимальная возможная разница между ценами).|

###Описание методов

<a id="security_id"></a>
####security_id()
```c++
SecurityId security_id() const;
```
Инструмент которому соответствует данный класс.

<a id="active_orders"></a>
####active_orders()
```c++
SecurityOrdersSnapshot& active_orders();
```
Структура, содержащая наши заявки. Отдельно описана в разделе [SecurityOrdersSnapshot](SecurityOrdersSnapshot.md).

<a id="total_amount"></a>
####total_amount()
```c++
int32_t total_amount() const;
```
Наша "предполагаемая" позиция - учитывается и реальная позиция на руках, и та, что мы ожидаем что будет проторгована.

<a id="executed_amount"></a>
####executed_amount()
```c++
int32_t executed_amount() const;
```
Наша текущая позиция.

<a id="best_price"></a>
####best_price()
```c++
const std::array<Decimal, 2>& best_price() const;
```
Лучшие цены (минимальная цена продажи и максимальная цена покупки) в момент последнего обновления стакана.

<a id="best_price_volume"></a>
####best_price_volume()
```c++
const std::array<int64_t, 2>& best_price_volume() const;
```
Объем лотов на лучших ценах.

<a id="middle_price"></a>
####middle_price()
```c++
Decimal middle_price() const;
```
Полусумма лучших цен.

<a id="spread"></a>
####spread()
```c++
int32_t spread() const;
```
Расстояние между лучшим аском и лучшим бидом в минимальных шагах цены.

<a id="book_updates_count"></a>
####book_updates_count()
```c++
int32_t book_updates_count() const;
```
Количество апдейтов стакана с начала дня.

<a id="statistics"></a>
####statistics()
```c++
StatisticsSnapshot statistics();
```
Структура, содержащая статистику по нашей текущей позиции.

<a id="server_time"></a>
####server_time()
```c++
Microseconds server_time() const;
```
Биржевое время последнего апдейта.

<a id="min_step"></a>
####min_step()
```c++
Decimal min_step() const;
```
Минимальный шаг цены в стакане (минимальная возможная разница между ценами).


