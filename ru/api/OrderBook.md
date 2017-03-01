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
| [fee_per_lot()](#fee_per_lot) | Комиссия за один проторгованный лот. |
| [book_updates_count()](#book_updates_count) | Количество обновлений стакана с начала дня. |
| [security_id()](#security_id) | Указатель на инструмент соответствующий стакану. |
| [added_volume_at_price()](#added_volume_at_price) | Объём новых добавленных заявок по данному направлению и цене. |
| [deleted_volume_at_price()](#deleted_volume_at_price) | Объём удалённых заявок по данному направлению и цене с прошлого обновления стакана. |

### Описание методов

#### quote_by_index() {#quote_by_index}

Принимает направление dir (BID (покупка) или ASK (продажа)) и порядковый номер index.

Возвращает котировку по направлению dir с порядковым номером index.

{% codetabs name="C++", type="c++" -%}
const Quote& quote_by_index(Dir dir, size_t index) const;
{%- language name="Python", type="py" -%}
def quote_by_index(self, dir, index)
{%- endcodetabs %}

#### price_by_index() {#price_by_index}

Принимает направление dir (BID (покупка) или ASK (продажа)) и порядковый номер index.

Возвращает цену котировки по направлению dir с порядковым номером index.

{% codetabs name="C++", type="c++" -%}
Price price_by_index(Dir dir, size_t index) const;
{%- language name="Python", type="py" -%}
def price_by_index(self, dir, index)
{%- endcodetabs %}

#### volume_by_index() {#volume_by_index}

Принимает направление dir (BID (покупка) или ASK (продажа)) и порядковый номер index.

Возвращает суммарный объём лотов по направлению dir с порядковым номером index.

{% codetabs name="C++", type="c++" -%}
Amount volume_by_index(Dir dir, size_t index) const;
{%- language name="Python", type="py" -%}
def volume_by_index(self, dir, index)
{%- endcodetabs %}

#### quote_by_price() {#quote_by_price}

Принимает направление dir (BID (покупка) или ASK (продажа)) и цену price.

Возвращает котировку по направлению dir с ценой price.

{% codetabs name="C++", type="c++" -%}
const Quote& quote_by_price(Dir dir, Price price) const;
{%- language name="Python", type="py" -%}
def quote_by_price(self, dir, price)
{%- endcodetabs %}

#### index_by_price() {#index_by_price}

Принимает направление dir (BID (покупка) или ASK (продажа)) и цену price.

Возвращает порядковый номер котировки по направлению dir с ценой price.

{% codetabs name="C++", type="c++" -%}
size_t index_by_price(Dir dir, Price price) const;
{%- language name="Python", type="py" -%}
def index_by_price(self, dir, price)
{%- endcodetabs %}

#### volume_by_price() {#volume_by_price}

Принимает направление dir (BID (покупка) или ASK (продажа)) и цену price.

Возвращает суммарный объём лотов по направлению dir с ценой price.

{% codetabs name="C++", type="c++" -%}
Amount volume_by_price(Dir dir, Price price) const;
{%- language name="Python", type="py" -%}
def volume_by_price(self, dir, price)
{%- endcodetabs %}

#### best_price() {#best_price}

Принимает направление dir (BID (покупка) или ASK (продажа)).

Возвращает лучшую цену в стакане по направлению dir.

{% codetabs name="C++", type="c++" -%}
Price best_price(Dir dir) const;
{%- language name="Python", type="py" -%}
def best_price(self, dir)
{%- endcodetabs %}

#### best_volume() {#best_volume}

Принимает направление dir (BID (покупка) или ASK (продажа)).

Возвращает суммарный объём лотов на лучшей цене по направлению dir.

{% codetabs name="C++", type="c++" -%}
Amount best_volume(Dir dir) const;
{%- language name="Python", type="py" -%}
def best_volume(self, dir)
{%- endcodetabs %}

#### all_quotes() {#all_quotes}

Принимает направление dir (BID (покупка) или ASK (продажа)).

Возвращает все котировки по направлению dir в виде объекта QuotesHolder.
Внимание: QuotesHolder — контейнер, по которому можно итерироваться.
Подробнее о QuotesHolder читайте здесь: <https://docs.hftbattle.com/ru/api/QuotesHolder.html>.

{% codetabs name="C++", type="c++" -%}
QuotesHolder all_quotes(Dir dir) const;
{%- endcodetabs %}

#### quotes_count() {#quotes_count}

Принимает направление dir (BID (покупка) или ASK (продажа)).

Возвращает количество котировок по направлению dir.

{% codetabs name="C++", type="c++" -%}
size_t quotes_count(Dir dir) const;
{%- language name="Python", type="py" -%}
def quotes_count(self, dir)
{%- endcodetabs %}

#### depth() {#depth}

Возвращает максимальное количество ценовых уровней, отображаемых в стакане (для обоих направлений оно одинаковое).

{% codetabs name="C++", type="c++" -%}
size_t depth() const;
{%- language name="Python", type="py" -%}
def depth(self)
{%- endcodetabs %}

#### server_time() {#server_time}

Возвращает биржевое время последнего изменения стакана, в микросекундах.

{% codetabs name="C++", type="c++" -%}
Microseconds server_time() const;
{%- language name="Python", type="py" -%}
def server_time(self)
{%- endcodetabs %}

#### local_time() {#local_time}

Возвращает локальное время последнего изменения стакана, в микросекундах.

{% codetabs name="C++", type="c++" -%}
Microseconds local_time() const;
{%- language name="Python", type="py" -%}
def local_time(self)
{%- endcodetabs %}

#### orders() {#orders}

Возвращает ссылку на объект типа SecurityOrdersSnapshot, содержащую ваши текущие заявки.
Подробнее о SecurityOrdersSnapshot читайте здесь: <https://docs.hftbattle.com/ru/api/SecurityOrdersSnapshot.html>.

{% codetabs name="C++", type="c++" -%}
const SecurityOrdersSnapshot& orders() const;
{%- language name="Python", type="py" -%}
def orders(self)
{%- endcodetabs %}

#### middle_price() {#middle_price}

Возвращает полусумму лучших цен по обоим направлениям.

{% codetabs name="C++", type="c++" -%}
Price middle_price() const;
{%- language name="Python", type="py" -%}
def middle_price(self)
{%- endcodetabs %}

#### min_step() {#min_step}

Возвращает минимальный шаг цены в стакане (минимальную возможную разницу между ценами).

{% codetabs name="C++", type="c++" -%}
Price min_step() const;
{%- language name="Python", type="py" -%}
def min_step(self)
{%- endcodetabs %}

#### spread_in_min_steps() {#spread_in_min_steps}

Возвращает расстояние между лучшими ценами покупки и продажи в терминах минимального шага цены.

{% codetabs name="C++", type="c++" -%}
size_t spread_in_min_steps() const;
{%- language name="Python", type="py" -%}
def spread_in_min_steps(self)
{%- endcodetabs %}

#### fee_per_lot() {#fee_per_lot}

Возвращает комиссию за один проторгованный лот.

{% codetabs name="C++", type="c++" -%}
Decimal fee_per_lot() const;
{%- language name="Python", type="py" -%}
def fee_per_lot(self)
{%- endcodetabs %}

#### book_updates_count() {#book_updates_count}

Возвращает количество обновлений стакана с начала дня.
Примечание: исследование изменения этой величины может быть использовано для получения информации об активности рынка.

{% codetabs name="C++", type="c++" -%}
size_t book_updates_count() const;
{%- language name="Python", type="py" -%}
def book_updates_count(self)
{%- endcodetabs %}

#### security_id() {#security_id}

Возвращает указатель на инструмент, которому соответствует данный стакан.

{% codetabs name="C++", type="c++" -%}
SecurityId security_id() const;
{%- language name="Python", type="py" -%}
def security_id(self)
{%- endcodetabs %}

#### added_volume_at_price() {#added_volume_at_price}

Принимает направление dir (BID (покупка) или ASK (продажа)) и цену price.

Возвращает объём добавленных заявок по направлению dir и цене price, по сравнению с предыдущим обновлением стакана.

{% codetabs name="C++", type="c++" -%}

{%- endcodetabs %}

#### deleted_volume_at_price() {#deleted_volume_at_price}

Принимает направление dir (BID (покупка) или ASK (продажа)) и цену price.

Возвращает объём удалённых заявок по направлению dir и цене price, по сравнению с предыдущим обновлением стакана.

{% codetabs name="C++", type="c++" -%}

{%- endcodetabs %}
