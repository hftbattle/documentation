# SecurityOrdersSnapshot

Путь в Local Pack `include/security_orders_snapshot.h`

Описание текущих заявок стратегии.
Учитываются только ваши заявки со статусом Adding и Active.
Данные обновляются перед приходом каждого апдейта в стратегию.
В процессе обработки одного апдейта данные гарантированно не меняются.

### Методы

| Имя | Описание |
| --- | --- |
| [amount()](#amount) | Объём заявок с заданной ценой и направлением. |
| [size_by_dir()](#size_by_dir) | Количество текущих заявок в данном направлении. |
| [active_orders_count()](#active_orders_count) | Количество активных заявок в данном направлении. |
| [active_orders_volume()](#active_orders_volume) | Объём активных заявок в данном направлении. |
| [orders_by_dir()](#orders_by_dir) | Вектор наших заявок. |
| [orders_by_dir_as_map()](#orders_by_dir_as_map) | Наши заявки в виде map. |
| [deleting_amount_by_dir()](#deleting_amount_by_dir) | Объём заявок отправленных на удаление. |

### Описание методов

#### amount() {#amount}

Принимает направление dir (BID (покупка) или ASK (продажа)) и цену price.
Возвращает суммарный объём ваших заявок с заданной ценой price по направлению dir.

```c++
Amount amount(Dir dir, Price price) const;
```

#### size_by_dir() {#size_by_dir}

Принимает направление dir (BID (покупка) или ASK (продажа)).
Возвращает количество всех ваших текущих заявок по направлению dir.

```c++
size_t size_by_dir(Dir dir) const;
```

#### active_orders_count() {#active_orders_count}

Принимает направление dir (BID (покупка) или ASK (продажа)).
Возвращает количество ваших активных заявок по направлению dir, т.е. заявок со статусом Active.

```c++
size_t active_orders_count(Dir dir) const;
```

#### active_orders_volume() {#active_orders_volume}

Принимает направление dir (BID (покупка) или ASK (продажа)).
Возвращает суммарный объём ваших активных заявок по направлению dir, т.е. заявок со статусом Active.

```c++
Amount active_orders_volume(Dir dir) const;
```

#### orders_by_dir() {#orders_by_dir}

Принимает направление dir (BID (покупка) или ASK (продажа)).
Возвращает vector указателей на ваши заявки, т.е. vector указателей на объекты класса Order, по направлению dir.

```c++
const Orders& orders_by_dir(Dir dir) const;
```

#### orders_by_dir_as_map() {#orders_by_dir_as_map}

Принимает направление dir (BID (покупка) или ASK (продажа)).
Возвращает map, в котором каждой цене соответствует vector заявок по направлению dir.
Внимание: map всегда упорядочен по возрастанию цены в независимости от dir.

```c++
OrdersMap orders_by_dir_as_map(Dir dir) const;
```

#### deleting_amount_by_dir() {#deleting_amount_by_dir}

Принимает направление dir (BID (покупка) или ASK (продажа)).
Возвращает суммарный объём заявок по направлению dir, отправленных на удаление, но еще не удалённых, т.е. со статусом Deleting.

```c++
Amount deleting_amount_by_dir(Dir dir) const;
```
