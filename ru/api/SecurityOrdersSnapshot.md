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
| [size_by_dir()](#size_by_dir) | Количество текущих заявок по данному направлению. |
| [active_orders_count()](#active_orders_count) | Количество активных заявок по данному направлению. |
| [active_orders_volume()](#active_orders_volume) | Суммарный объём активных заявок по данному направлениию. |
| [orders_by_dir()](#orders_by_dir) | Вектор ваших заявок по данному направлению. |
| [orders_by_dir_as_map()](#orders_by_dir_as_map) | Ваши заявки по данному направлению в виде map. |
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

Возвращает вектор указателей на ваши заявки по направлению dir, т.е. вектор указателей на объекты класса Order.

{% codetabs name="C++", type="c++" -%}
const Orders& orders_by_dir(Dir dir) const;
{%- language name="Python", type="py" -%}
def orders_by_dir(self, dir)
{%- endcodetabs %}

#### orders_by_dir_as_map() {#orders_by_dir_as_map}

Принимает направление dir.

Возвращает map, в котором каждой цене соответствует вектор заявок по направлению dir.
Внимание: map всегда упорядочен по возрастанию цены вне зависимости от dir.

{% codetabs name="C++", type="c++" -%}
OrdersMap orders_by_dir_as_map(Dir dir) const;
{%- language name="Python", type="py" -%}
def orders_by_dir_as_map(self, dir)
{%- endcodetabs %}

Приведём пример использования данного метода.
Мы пробежимся по всем нашим заявкам на покупку и выведем текущее количество заявок с такой ценой.

{% codetabs name="C++", type="c++" -%}
auto orders_map = order_book.orders().orders_by_dir_as_map(BID);
for (auto it = orders_map.cbegin(); it != orders_map.cend(); ++it) {
  SCREEN() << "price: " << it->first << " amount: " << it->second.size()
}
{%- language name="Python", type="py" -%}
orders_map = order_book.orders().orders_by_dir_as_map(BID)
for price, orders in orders_map.items():
    print('price: %s amount: %s' % (price, len(orders)))
{%- endcodetabs %}

#### deleting_amount_by_dir() {#deleting_amount_by_dir}

Принимает направление dir.

Возвращает суммарный объём заявок по направлению dir, отправленных на удаление, но ещё не удалённых, т.е. заявок со статусом Deleting.

{% codetabs name="C++", type="c++" -%}
Amount deleting_amount_by_dir(Dir dir) const;
{%- language name="Python", type="py" -%}
def deleting_amount_by_dir(self, dir)
{%- endcodetabs %}
