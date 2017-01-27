# OrderBook

Путь в Local Pack `include/order_book.h`

Класс OrderBook — это агрегатор всех заявок по конкретному инструменту.
Внимание: нумерация индексов (порядковых номеров) идёт с 0, начиная с лучшей цены отдельно по каждому направлению, т.е. в порядке возрастания цены для BID (покупки) и в порядке убывания цены для ASK (продажи).

При этом **учитываются только непустые котировки!**
То есть квота с порядковым номером 3 не обязательно отстоит ровно на 3 минимальных шага цены от квоты с лучшей ценой.

### Методы

| Имя | Описание |
| --- | --- |
| [quote_by_index()](#quote_by_index) | Котировка с данным порядковым номером и направлением. |
| [price_by_index()](#price_by_index) | Цена котировки с данным порядковым номером и направлением. |
| [volume_by_index()](#volume_by_index) | Объём лотов в котировке с данным порядковым номером и направлением. |
| [quote_by_price()](#quote_by_price) | Котировка с данным направлением и ценой. |
| [index_by_price()](#index_by_price) | Порядковый номер котировки с данным направлением и ценой. |
| [volume_by_price()](#volume_by_price) | Объём лотов с данным направлением и ценой. |
| [best_price()](#best_price) | Лучшая цена в стакане с данным направлением. |
| [best_volume()](#best_volume) | Объём лотов с данной ценой и направлением. |
| [all_quotes()](#all_quotes) | Все квоты с данным направлением в виде QuotesHolder. |
| [quotes_count()](#quotes_count) | Количество котировок по направлению dir. |
| [depth()](#depth) | Количество ценовых уровней, отображаемых в стакане. |
| [server_time()](#server_time) | Биржевое время последнего изменения стакана. |
| [local_time()](#local_time) | Локальное время последнего изменения стакана. |
| [orders()](#orders) | Ссылка на ваши текущие заявки в виде SecurityOrdersSnapshot. |
| [middle_price()](#middle_price) | Полусумма лучших цен. |
| [min_step()](#min_step) | Минимальный шаг цены. |
| [spread_in_min_steps()](#spread_in_min_steps) | Расстояние между лучшими ценами в минимальных шагах цены. |
| [book_updates_count()](#book_updates_count) | Количество обновлений стакана с начала дня. |
| [security_id()](#security_id) | Указатель на инструмент соответствующий стакану. |
| [added_volume_at_price()](#added_volume_at_price) | Объём новых добавленных заявок по данному направлению и цене. |
| [deleted_volume_at_price()](#deleted_volume_at_price) | Объём удалённых заявок по данному направлению и цене с прошлого обновления стакана. |

### Описание методов

#### quote_by_index() {#quote_by_index}

Принимает направление dir (BID (покупка) или ASK (продажа)) и порядковый номер index.

Возвращает котировку по направлению dir с порядковым номером index.

```c++
const Quote& quote_by_index(Dir dir, size_t index) const;
```

#### price_by_index() {#price_by_index}

Принимает направление dir (BID (покупка) или ASK (продажа)) и порядковый номер index.

Возвращает цену котировки по направлению dir с порядковым номером index.

```c++
Price price_by_index(Dir dir, size_t index) const;
```

#### volume_by_index() {#volume_by_index}

Принимает направление dir (BID (покупка) или ASK (продажа)) и порядковый номер index.

Возвращает суммарный объём лотов по направлению dir с порядковым номером index.

```c++
Amount volume_by_index(Dir dir, size_t index) const;
```

#### quote_by_price() {#quote_by_price}

Принимает направление dir (BID (покупка) или ASK (продажа)) и цену price.

Возвращает котировку по направлению dir с ценой price.

```c++
const Quote& quote_by_price(Dir dir, Price price) const;
```

#### index_by_price() {#index_by_price}

Принимает направление dir (BID (покупка) или ASK (продажа)) и цену price.

Возвращает порядковый номер котировки по направлению dir с ценой price.

```c++
size_t index_by_price(Dir dir, Price price) const;
```

#### volume_by_price() {#volume_by_price}

Принимает направление dir (BID (покупка) или ASK (продажа)) и цену price.

Возвращает суммарный объём лотов по направлению dir с ценой price.

```c++
Amount volume_by_price(Dir dir, Price price) const;
```

#### best_price() {#best_price}

Принимает направление dir (BID (покупка) или ASK (продажа)).

Возвращает лучшую цену в стакане по направлению dir.

```c++
Price best_price(Dir dir) const;
```

#### best_volume() {#best_volume}

Принимает направление dir (BID (покупка) или ASK (продажа)).

Возвращает суммарный объём лотов на лучшей цене по направлению dir.

```c++
Amount best_volume(Dir dir) const;
```

#### all_quotes() {#all_quotes}

Принимает направление dir (BID (покупка) или ASK (продажа)).

Возвращает все котировки по направлению dir в виде объекта QuotesHolder.
Внимание: QuotesHolder — контейнер, по которому можно итерироваться.
Подробнее читайте в документации.
TODO(asalikhov): add links to docs.

```c++
QuotesHolder all_quotes(Dir dir) const;
```

#### quotes_count() {#quotes_count}

Принимает направление dir (BID (покупка) или ASK (продажа)).

Возвращает количество котировок по направлению dir.

```c++
size_t quotes_count(Dir dir) const;
```

#### depth() {#depth}

Возвращает максимальное количество ценовых уровней, отображаемых в стакане (для обоих направлений оно одинаковое).

```c++
size_t depth() const;
```

#### server_time() {#server_time}

Возвращает биржевое время последнего изменения стакана, в микросекундах.

```c++
Microseconds server_time() const;
```

#### local_time() {#local_time}

Возвращает локальное время последнего изменения стакана, в микросекундах.

```c++
Microseconds local_time() const;
```

#### orders() {#orders}

Возвращает ссылку на объект типа SecurityOrdersSnapshot, содержащую ваши текущие заявки.
TODO(asalikhov): add links to docs.

```c++
const SecurityOrdersSnapshot& orders() const;
```

#### middle_price() {#middle_price}

Возвращает полусумму лучших цен по обоим направлениям.

```c++
Price middle_price() const;
```

#### min_step() {#min_step}

Возвращает минимальный шаг цены в стакане (минимальную возможную разницу между ценами).

```c++
Price min_step() const;
```

#### spread_in_min_steps() {#spread_in_min_steps}

Возвращает расстояние между лучшими ценами покупки и продажи в терминах минимального шага цены.

```c++
size_t spread_in_min_steps() const;
```

#### book_updates_count() {#book_updates_count}

Возвращает количество обновлений стакана с начала дня.
Примечание: исследование изменения этой величины может быть использовано для получения информации об активности рынка.

```c++
size_t book_updates_count() const;
```

#### security_id() {#security_id}

Возвращает указатель на инструмент, которому соответствует данный стакан.

```c++
SecurityId security_id() const;
```

#### added_volume_at_price() {#added_volume_at_price}

Принимает направление dir (BID (покупка) или ASK (продажа)) и цену price.

Возвращает объём добавленных заявок по направлению dir и цене price, по сравнению с предыдущим обновлением стакана.

```c++

```

#### deleted_volume_at_price() {#deleted_volume_at_price}

Принимает направление dir (BID (покупка) или ASK (продажа)) и цену price.

Возвращает объём удалённых заявок по направлению dir и цене price, по сравнению с предыдущим обновлением стакана.

```c++

```
