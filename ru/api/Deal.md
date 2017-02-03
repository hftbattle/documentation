# Deal

Путь в Local Pack `include/deal.h`

Описание произошедшей сделки.

### Методы

| Имя | Описание |
| --- | --- |
| [aggressor_side()](#aggressor_side) | Направление более поздней заявки. |
| [price()](#price) | Цена сделки. |
| [amount()](#amount) | Объём сделки. |
| [server_time()](#server_time) | Биржевое время совершения сделки. |
| [local_time()](#local_time) | Локальное время совершения сделки. |
| [orders()](#orders) | Заявки, участвующие в сделке. |
| [id()](#id) | Уникальный идентификатор сделки. |

### Описание методов

#### aggressor_side() {#aggressor_side}

Возвращает направление агрессора сделки, т.е. направление той заявки, которая была поставлена позже.

{% codetabs name="C++", type="c++" -%}
Dir aggressor_side() const;
{%- language name="Python", type="py" -%}
def aggressor_side(self)
{%- endcodetabs %}

#### price() {#price}

Возвращает цену, по которой была совершена сделка.

{% codetabs name="C++", type="c++" -%}
Price price() const;
{%- language name="Python", type="py" -%}
def price(self)
{%- endcodetabs %}

#### amount() {#amount}

Возвращает объём сделки, т.е. количество лотов, которые были сведены в результате этой сделки.

{% codetabs name="C++", type="c++" -%}
Amount amount() const;
{%- language name="Python", type="py" -%}
def amount(self)
{%- endcodetabs %}

#### server_time() {#server_time}

Возвращает биржевое время совершения сделки в микросекундах.

{% codetabs name="C++", type="c++" -%}
Microseconds server_time() const;
{%- language name="Python", type="py" -%}
def server_time(self)
{%- endcodetabs %}

#### local_time() {#local_time}

Возвращает локальное время совершения сделки в микросекундах.

{% codetabs name="C++", type="c++" -%}
Microseconds local_time() const;
{%- language name="Python", type="py" -%}
def local_time(self)
{%- endcodetabs %}

#### orders() {#orders}

Возвращает массив из двух указателей на заявки (BID (покупка) или ASK (продажа)), участвующие в сделке.

{% codetabs name="C++", type="c++" -%}
const RawOrdersArray orders() const;
{%- language name="Python", type="py" -%}
def orders(self)
{%- endcodetabs %}

#### id() {#id}

Возвращает уникальный числовой идентификатор сделки, полученный во время симуляции.
Он может быть использован для сохранения какой-либо информации о заявке.

{% codetabs name="C++", type="c++" -%}
Id id() const;
{%- language name="Python", type="py" -%}
def id(self)
{%- endcodetabs %}
