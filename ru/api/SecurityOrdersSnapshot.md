## SecurityOrdersSnapshot

Путь в Local Pack-е: `include/security_orders_snapshot.h`

Класс SecurityOrdersSnapshot хранит текущие заявки стратегии.
Структура обновляется перед приходом каждого апдейта в стратегию.
В процессе обработки одного апдейта структура гарантированно не меняется.

### Поля

|Имя| Описание|
|------------------|--------------------|
|[orders_by_dir](#orders_by_dir)|Списки наших текущих заявок по направлению.|
|[deleting_amount](#deleting_amount)|Суммарный объем заявок по направлению, отправленных на удаление, но еще не удаленных.|

### Методы

|Имя| Описание|
|------------------|--------------------|
|[get_volume_by_price(Dir dir, Decimal price)](#get_volume_by_price)|Суммарный объем заявок с заданной ценой *price* по направлению *dir*.|
|[active_orders_count(Dir dir)](#active_orders_count)|Количество наших активных заявок по направлению *dir*.|
|[active_orders_volume(Dir dir)](#active_orders_volume)|Суммарный объем активных ордеров по направлению *dir*.|
|[get_orders_by_dir_to_map(Dir dir)](#get_orders_by_dir_to_map)|Возвращает std::map, в котором каждой цене соответствует std::vector заявок по направлению *dir*.|
|[size()](#size)|Суммарное количество наших заявок по обоим направлениям.|

### Описание полей

#### orders_by_dir<a id="orders_by_dir"></a>

```c++
std::array<std::vector<OrderSnapshot>, 2> orders_by_dir;
```

Списки наших текущих заявок по направлению.

#### deleting_amount<a id="deleting_amount"></a>

```c++
std::array<Amount, 2> deleting_amount;
```

Суммарный объем заявок по направлению, отправленных на удаление, но еще не удаленных.

### Описание методов

#### get_volume_by_price()<a id="get_volume_by_price"></a>

```c++
Amount get_volume_by_price(Dir dir, Decimal price) const;
```

Суммарный объем заявок с заданной ценой *price* по направлению *dir*.

#### active_orders_count()<a id="active_orders_count"></a>

```c++
size_t active_orders_count(Dir dir) const;
```

Количество наших активных заявок по направлению *dir*.

#### active_orders_volume()<a id="active_orders_volume"></a>

```c++
Amount active_orders_volume(Dir dir) const;
```

Суммарный объем активных ордеров по направлению *dir*.

#### get_orders_by_dir_to_map()<a id="get_orders_by_dir_to_map"></a>

```c++
OrdersMap get_orders_by_dir_to_map(Dir dir) const;
```

Возвращает std::map, в котором каждой цене соответствует std::vector заявок по направлению *dir*.

#### size()<a id="size"></a>

```c++
size_t size() const;
```

Суммарное количество наших заявок по обоим направлениям.
