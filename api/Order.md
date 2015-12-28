#Order

`include/simulator/order/order.h`


Класс Order представляет собой реализацию заявки (надеемся, вы уже знакомы с тем что это такое! Если нет - то вам сюда). Ничего необычного, поэтому переходим сразу к


###Поля


|Имя| Описание|
|------------------|--------------------|
|[origin_server_time](#origin_server_time)|Биржевое время постановки заявки в стакан в тиках.|
|[price](#price)|Цена заявки.|
|[amount](#amount)|Количество лотов.|
|[dir](#dir)|Направление.|
|[time_in_force](#time_in_force)|Тип заявки - Limit или IOC.|
|[security](#security)|Инструмент, к которому относится заявка.|

###Методы


|Имя| Описание|
|------------------|--------------------|
|[outer_id()](#outer_id)|Id заявки.|
|[amount_rest()](#amount_rest)|Текущее количество лотов в заявке (может быть меньше начального, если были сделки с ее участием).|
|[implied_amount()](#implied_amount)|Количество лотов, которое предположительно будет сведено.|
|[status()](#status)|Статус заявки - активная, ждущая удаления, удаленная.|

###Описание полей

<a id="origin_server_time"></a>
####origin_server_time
```c++
int64_t origin_server_time;
```
Биржевое время постановки заявки в стакан в тиках.

<a id="price"></a>
####price
```c++
const xor_platform::Decimal price;
```
Цена заявки.

<a id="amount"></a>
####amount
```c++
const int32_t amount;
```
Количество лотов.

<a id="dir"></a>
####dir
```c++
const Dir dir;
```
Направление.

<a id="time_in_force"></a>
####time_in_force
```c++
const xor_platform::OrderTimeInForce time_in_force;
```
Тип заявки - Limit или IOC.

<a id="security"></a>
####security
```c++
const Security * security;
```
Инструмент, к которому относится заявка.


###Описание методов

<a id="outer_id"></a>
####outer_id()
```c++
inline xor_platform::Id outer_id() const;
```
Id заявки.

<a id="amount_rest"></a>
####amount_rest()
```c++
inline int32_t amount_rest() const;
```
Текущее количество лотов в заявке (может быть меньше начального, если были сделки с ее участием).

<a id="implied_amount"></a>
####implied_amount()
```c++
inline int32_t implied_amount() const;
```
Количество лотов, которое предположительно будет сведено.

<a id="status"></a>
####status()
```c++
inline xor_platform::OrderStatus status() const;
```
Статус заявки - активная, ждущая удаления, удаленная.

