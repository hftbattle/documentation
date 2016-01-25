#SecurityOrdersSnapshot
`xor/sender/security_orders_snapshot.h`

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
|[count_active_orders(Dir dir)](#count_active_orders)|Количество наших добавленных заявок по направлению *dir*.|

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

<a name="count_active_orders"></a>
####count_active_orders()
```c++
size_t count_active_orders(Dir dir) const;
```
Количество наших добавленных заявок по направлению *dir*.


