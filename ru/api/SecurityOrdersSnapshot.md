# SecurityOrdersSnapshot

Путь в Local Pack `include/security_orders_snapshot.h`

Описание текущих заявок стратегии.
Учитываются только ваши заявки со статусом Adding и Active.
В методе deleting_amount_by_dir также учитываются заявки со статусом Deleting.

Данные обновляются перед приходом каждого апдейта в стратегию.
В процессе обработки одного апдейта данные гарантированно не меняются.

### Методы

| Имя | Описание |
| --- | --- |
| [volume()](#volume) | Суммарный объём заявок с заданной ценой и направлением. |
| [size_by_dir()](#size_by_dir) | Количество текущих заявок в данном направлении. |
| [active_orders_count()](#active_orders_count) | Количество активных заявок в данном направлении. |
| [active_orders_volume()](#active_orders_volume) | Суммарный объём активных заявок в данном направлении. |
| [orders_by_dir()](#orders_by_dir) | Вектор ваших заявок в данном направлении. |
| [orders_by_dir_as_map()](#orders_by_dir_as_map) | Ваши заявки в данном направлении в виде map. |
| [deleting_amount_by_dir()](#deleting_amount_by_dir) | Суммарный объём заявок, отправленных на удаление. |

### Описание методов

#### volume() {#volume}

Принимает направление dir и цену price.

Возвращает суммарный объём ваших заявок с заданной ценой price по направлению dir.

{% codetabs name="C++", type="c++" -%}
Amount volume(Dir dir, Price price) const;
{%- language name="Python", type="py" -%}
def volume(self, dir, price)
{%- endcodetabs %}

#### size_by_dir() {#size_by_dir}

Принимает направление dir.

Возвращает количество всех ваших текущих заявок по направлению dir.

{% codetabs name="C++", type="c++" -%}
size_t size_by_dir(Dir dir) const;
{%- language name="Python", type="py" -%}
def size_by_dir(self, dir)
{%- endcodetabs %}

#### active_orders_count() {#active_orders_count}

Принимает направление dir.

Возвращает количество ваших активных заявок по направлению dir, т.е. заявок со статусом Active.

{% codetabs name="C++", type="c++" -%}
size_t active_orders_count(Dir dir) const;
{%- language name="Python", type="py" -%}
def active_orders_count(self, dir)
{%- endcodetabs %}

#### active_orders_volume() {#active_orders_volume}

Принимает направление dir.

Возвращает суммарный объём ваших активных заявок по направлению dir, т.е. заявок со статусом Active.

{% codetabs name="C++", type="c++" -%}
Amount active_orders_volume(Dir dir) const;
{%- language name="Python", type="py" -%}
def active_orders_volume(self, dir)
{%- endcodetabs %}

#### orders_by_dir() {#orders_by_dir}

Принимает направление dir.

Возвращает vector указателей на ваши заявки, т.е. vector указателей на объекты класса Order по направлению dir.

{% codetabs name="C++", type="c++" -%}
const Orders& orders_by_dir(Dir dir) const;
{%- language name="Python", type="py" -%}
def orders_by_dir(self, dir)
{%- endcodetabs %}

#### orders_by_dir_as_map() {#orders_by_dir_as_map}

Принимает направление dir.

Возвращает map, в котором каждой цене соответствует vector заявок по направлению dir.
Внимание: map всегда упорядочен по возрастанию цены вне зависимости от dir.

{% codetabs name="C++", type="c++" -%}
OrdersMap orders_by_dir_as_map(Dir dir) const;
{%- language name="Python", type="py" -%}
def orders_by_dir_as_map(self, dir)
{%- endcodetabs %}

#### deleting_amount_by_dir() {#deleting_amount_by_dir}

Принимает направление dir.

Возвращает суммарный объём заявок по направлению dir, отправленных на удаление, но еще не удалённых, т.е. со статусом Deleting.

{% codetabs name="C++", type="c++" -%}
Amount deleting_amount_by_dir(Dir dir) const;
{%- language name="Python", type="py" -%}
def deleting_amount_by_dir(self, dir)
{%- endcodetabs %}
