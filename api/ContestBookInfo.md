#ContestBookInfo
include/simulator/strategy/contest_book_info.h



Для каждого используемого инструмента есть разные параметры, описывающие его состояние -- начиная от стакана (описание тут) до нашей позиции. Класс ContestBookInfo служит агрегатором информации по текущему состоянию стакана. Он не является полноценной заменой стакана, но содержит часть информации о нем -- и предпочтительно пользоваться именно этим классом, например, для определения лучших цен, это значительно быстрее, чем запрашивать у стакана. Также именно тут можно узнать наши активные заявки, позицию и сальдо.


* [ContestBookInfo.best_price() const](#best_price)
* [ContestBookInfo.book_updates_count() const](#book_updates_count)
* [ContestBookInfo.best_price_volume() const](#best_price_volume)
* [ContestBookInfo.middle_price() const](#middle_price)
* [ContestBookInfo.spread() const](#spread)
* [ContestBookInfo.active_orders()](#active_orders)
* [ContestBookInfo.statistics()](#statistics)
* [ContestBookInfo.security_id() const](#security_id)
* [ContestBookInfo.server_time() const](#server_time)
* [ContestBookInfo.min_step() const](#min_step)
* [ContestBookInfo.total_amount() const](#total_amount)
* [ContestBookInfo.executed_amount() const](#executed_amount)

#Основные поля и методы класса

```cpp
/* лучшие цены (минимальная цена продажи и максимальная цена покупки) в момент  последнего обновления стакана */
const std::array<Decimal, 2>& best_price() const;
```

```cpp
/* Возвращает количество апдейтов стакана с начала дня */
int32_t book_updates_count() const;
```

```cpp
const std::array<int64_t, 2>& best_price_volume() const;
```объем лотов на лучших ценах

```cpp
Decimal middle_price() const;
```полусумма лучших цен

```cpp
int32_t spread() const;
```расстояние между лучшим аском и лучшим бидом в минимальных шагах цены

```cpp
SecurityOrdersSnapshot& active_orders();
```структура, содержащая наши заявки, описана отдельно тут

```cpp
StatisticsSnapshot statistics();
```структура, содержащая статистику по нашей текущей позиции. Используется из стратегии неявно, в вызовах executed_amount() и total_amount().

```cpp
SecurityId security_id() const;
```инструмент которому соответствует данная структура

```cpp
Microseconds server_time() const;
```биржевое время последнего апдейта

```cpp
xor_platform::Decimal min_step() const;
```минимальный шаг цены в стакане (минимальная возможная разница между ценами)

```cpp
int32_t total_amount() const;
```наша "предполагаемая" позиция - учитывается и реальная позиция на руках, и та, что мы ожидаем что будет проторгована

```cpp
int32_t executed_amount() const;
```наша текущая позиция

