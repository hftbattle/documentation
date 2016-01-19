#SecurityOrdersSnapshot
`xor/sender/security_orders_snapshot.h`

###Поля

|Имя| Описание|
|------------------|--------------------|
|[orders_by_dir](#orders_by_dir)|Списки наших текущих заявок по направлению.|
|[deleting_amount](#deleting_amount)|Суммарный объем заявок по направлению, отправленных на удаление, но еще не удаленных.|

###Методы

|Имя| Описание|
|------------------|--------------------|
|[amount(Dir dir, Price price)](#amount)|Объем наших заявок на цене *price* по направлению *dir*.|
|[size()](#size)|Суммарное количество наших заявок по обоим направлениям.|
|[count_added_orders(Dir dir)](#count_added_orders)|Количество наших добавленных заявок по направлению *dir*.|
|[clear()](#clear)|Очищает списки наших текущих заявок *orders_by_dir* и зануляет *deleting_amount*.|

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
<a name="amount"></a>
####amount()
```c++
Amount amount(Dir dir, Price price) const;
```
Объем наших заявок на цене *price* по направлению *dir*.

<a name="size"></a>
####size()
```c++
size_t size() const;
```
Суммарное количество наших заявок по обоим направлениям.

<a name="count_added_orders"></a>
####count_added_orders()
```c++
int32_t count_added_orders(Dir dir);
```
Количество наших добавленных заявок по направлению *dir*.

<a name="clear"></a>
####clear()
```c++
void clear();
```
Очищает списки наших текущих заявок *orders_by_dir* и зануляет *deleting_amount*.


