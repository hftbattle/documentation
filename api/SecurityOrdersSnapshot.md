#SecurityOrdersSnapshot
Путь в Local Pack-е: `include/xor/sender/security_orders_snapshot.h`

Класс SecurityOrdersSnapshot хранит текущие заявки стратегии.
Структура обновляется перед приходом каждого апдейта в стратегию.
В процессе обработки одного апдейта структура гарантированно не меняется.

###Поля

|Имя| Описание|
|------------------|--------------------|
|[orders_by_dir](#orders_by_dir)|Списки наших текущих заявок по направлению.|
|[deleting_amount](#deleting_amount)|Суммарный объем заявок по направлению, отправленных на удаление, но еще не удаленных.|

###Методы

|Имя| Описание|
|------------------|--------------------|
|[volume(Dir dir, Price price)](#volume)|Объем наших заявок на цене *price* и направлению *dir*.|
|[size()](#size)|Суммарное количество наших заявок по обоим направлениям.|
|[active_orders_count(Dir dir)](#active_orders_count)|Количество наших активных заявок по направлению *dir*.|

###Описание полей
<a name="orders_by_dir"></a>
####orders_by_dir
```c++
std::array<std::vector<OrderSnapshot>, 2> orders_by_dir;
```
Списки наших текущих заявок по направлению.

<a name="deleting_amount"></a>
####deleting_amount
```c++
std::array<Amount, 2> deleting_amount;
```
Суммарный объем заявок по направлению, отправленных на удаление, но еще не удаленных.


###Описание методов
<a name="volume"></a>
####volume()
```c++
Amount volume(Dir dir, Price price) const;
```
Объем наших заявок на цене *price* и направлению *dir*.

<a name="size"></a>
####size()
```c++
size_t size() const;
```
Суммарное количество наших заявок по обоим направлениям.

<a name="active_orders_count"></a>
####active_orders_count()
```c++
size_t active_orders_count(Dir dir) const;
```
Количество наших активных заявок по направлению *dir*.


