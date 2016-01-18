#Order
`simulator/order/order.h`

Заявка - это пара <цена, количество лотов>.
Класс Order представляет собой реализацию биржевой заявки.

###Поля

|Имя| Описание|
|------------------|--------------------|
|[20](#20)|Максимальная длина комментария к заявке.|
|[origin_server_time](#origin_server_time)|Биржевое время постановки заявки в стакан в тиках.|
|[origin_time](#origin_time)|Локальное время постановки заявки в стакан в тиках.|
|[id](#id)|Наш (внутренний) ID заявки.|
|[dir](#dir)|Направление заявки.|
|[price](#price)|Цена заявки.|
|[amount](#amount)|Объем заявки (количество лотов).|
|[time_in_force](#time_in_force)|Тип заявки - Limit или IOC.|
|[market_id](#market_id)|ID биржи.|
|[*security](#*security)|Инструмент, к которому относится заявка.|

###Методы

|Имя| Описание|
|------------------|--------------------|
|[amount_rest()](#amount_rest)|Текущий объем заявки. Может быть меньше начального, если были сделки с ее участием.|
|[implied_amount()](#implied_amount)|Количество лотов, которое предположительно будет сведено.|
|[status()](#status)|Статус заявки: в процессе добавления, активная, ждущая удаления, удаленная.|
|[outer_id()](#outer_id)|Биржевой (внешний) ID заявки.|

###Описание полей
<a id="20"></a>
####20
```c++
static const size_t kMaxCommentLength = 20;
```
Максимальная длина комментария к заявке.

<a id="origin_server_time"></a>
####origin_server_time
```c++
int64_t origin_server_time;
```
Биржевое время постановки заявки в стакан в тиках.

<a id="origin_time"></a>
####origin_time
```c++
const Ticks origin_time;
```
Локальное время постановки заявки в стакан в тиках.

<a id="id"></a>
####id
```c++
const Id id;
```
Наш (внутренний) ID заявки.

<a id="dir"></a>
####dir
```c++
const Dir dir;
```
Направление заявки.

<a id="price"></a>
####price
```c++
const Price price;
```
Цена заявки.

<a id="amount"></a>
####amount
```c++
const Amount amount;
```
Объем заявки (количество лотов).

<a id="time_in_force"></a>
####time_in_force
```c++
const OrderTimeInForce time_in_force;
```
Тип заявки - Limit или IOC.

<a id="market_id"></a>
####market_id
```c++
const MarketId market_id;
```
ID биржи.

<a id="*security"></a>
####*security
```c++
const Security *security;
```
Инструмент, к которому относится заявка.


###Описание методов
<a id="amount_rest"></a>
####amount_rest()
```c++
inline Amount amount_rest() const 
```
Текущий объем заявки. Может быть меньше начального, если были сделки с ее участием.

<a id="implied_amount"></a>
####implied_amount()
```c++
inline Amount implied_amount() const 
```
Количество лотов, которое предположительно будет сведено.

<a id="status"></a>
####status()
```c++
inline OrderStatus status() const 
```
Статус заявки: в процессе добавления, активная, ждущая удаления, удаленная.

<a id="outer_id"></a>
####outer_id()
```c++
inline Id outer_id() const 
```
Биржевой (внешний) ID заявки.


