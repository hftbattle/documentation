#Order
`include/simulator/order/order.h`


Класс Order представляет собой реализацию заявки (надеемся, вы уже знакомы с тем что это такое! Если нет - то вам сюда). Ничего необычного, поэтому переходим сразу к


|Имя| Описание|
|------------------|--------------------|
|[Order.outer_id() const](#outer_id)|id заявки|
|[Order.origin_server_time](#origin_server_time)|биржевое время постановки заявки в стакан в тиках|
|[Order.price](#price)|цена заявки|
|[Order.amount](#amount)|количество лотов|
|[Order.dir](#dir)|направление|
|[Order.time_in_force](#time_in_force)|тип заявки - Limit или IOC|
|[Order.security](#security)|инструмент, к которому относится заявка|
|[Order.amount_rest() const](#amount_rest)|текущее количество лотов в заявке (может быть меньше начального, если были сделки с ее участием)|
|[Order.implied_amount() const](#implied_amount)|количество лотов, которое предположительно будет сведено|
|[Order.status() const](#status)|статус заявки - активная, ждущая удаления, удаленная|

#Основные поля и методы класса

```c++
inline xor_platform::Id outer_id() const;
```
id заявки

```c++
int64_t origin_server_time;
```
биржевое время постановки заявки в стакан в тиках

```c++
const xor_platform::Decimal price;
```
цена заявки

```c++
const int32_t amount;
```
количество лотов

```c++
const Dir dir;
```
направление

```c++
const xor_platform::OrderTimeInForce time_in_force;
```
тип заявки - Limit или IOC

```c++
const Security * security;
```
инструмент, к которому относится заявка

```c++
inline int32_t amount_rest() const;
```
текущее количество лотов в заявке (может быть меньше начального, если были сделки с ее участием)

```c++
inline int32_t implied_amount() const;
```
количество лотов, которое предположительно будет сведено

```c++
inline xor_platform::OrderStatus status() const;
```
статус заявки - активная, ждущая удаления, удаленная

