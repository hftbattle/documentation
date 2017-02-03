# Order

Путь в Local Pack `include/order.h`

Описание биржевой заявки.

### Методы

| Имя | Описание |
| --- | --- |
| [id()](#id) | Уникальный идентификатор заявки. |
| [dir()](#dir) | Направление заявки. |
| [price()](#price) | Цена заявки. |
| [amount()](#amount) | Изначальный объём заявки. |
| [amount_rest()](#amount_rest) | Текущий объём заявки. |
| [security_id()](#security_id) | Указатель на инструмент, соответствующий заявке. |
| [status()](#status) | Статус заявки. |
| [origin_server_time()](#origin_server_time) | Биржевое время постановки заявки в стакан. |
| [local_time()](#local_time) | Локальное время постановки заявки в стакан. |

### Описание методов

#### id() {#id}

Возвращает уникальный числовой идентификатор заявки, полученный во время симуляции.
Он может быть использован для сохранения какой-либо информации о заявке.

{% codetabs name="C++", type="c++" -%}
Id id() const;
{%- language name="Python", type="py" -%}
def id(self)
{%- endcodetabs %}

#### dir() {#dir}

Возвращает направление заявки (BID (покупка) или ASK (продажа)).

{% codetabs name="C++", type="c++" -%}
Dir dir() const;
{%- language name="Python", type="py" -%}
def dir(self)
{%- endcodetabs %}

#### price() {#price}

Возвращает цену заявки.

{% codetabs name="C++", type="c++" -%}
Price price() const;
{%- language name="Python", type="py" -%}
def price(self)
{%- endcodetabs %}

#### amount() {#amount}

Возвращает изначальный объём заявки (количество лотов).

{% codetabs name="C++", type="c++" -%}
Amount amount() const;
{%- language name="Python", type="py" -%}
def amount(self)
{%- endcodetabs %}

#### amount_rest() {#amount_rest}

Возвращает текущий объём заявки (количество лотов этой заявки, которые ещё не были сведены).

{% codetabs name="C++", type="c++" -%}
Amount amount_rest() const;
{%- language name="Python", type="py" -%}
def amount_rest(self)
{%- endcodetabs %}

#### security_id() {#security_id}

Возвращает указатель на инструмент, к которому относится заявка (нужен для того, чтобы понимать, по какому из инструментов была поставлена заявка).

{% codetabs name="C++", type="c++" -%}
SecurityId security_id() const;
{%- language name="Python", type="py" -%}
def security_id(self)
{%- endcodetabs %}

#### status() {#status}

Возвращает значение enum class, статус заявки.

Возможные статусы: в процессе добавления (Adding), активная (Active), в процессе удаления, но ещё не удалённая (Deleting) и удалённая (Deleted).
Подробнее читайте в описании класса OrderStatus.
TODO(asalikhov): add links to docs.

{% codetabs name="C++", type="c++" -%}
OrderStatus status() const;
{%- language name="Python", type="py" -%}
def status(self)
{%- endcodetabs %}

#### origin_server_time() {#origin_server_time}

Возвращает биржевое время постановки заявки в стакан в микросекундах.

{% codetabs name="C++", type="c++" -%}
Microseconds origin_server_time() const;
{%- language name="Python", type="py" -%}
def origin_server_time(self)
{%- endcodetabs %}

#### local_time() {#local_time}

Возвращает локальное время постановки заявки в стакан в микросекундах.

{% codetabs name="C++", type="c++" -%}
Microseconds local_time() const;
{%- language name="Python", type="py" -%}
def local_time(self)
{%- endcodetabs %}
