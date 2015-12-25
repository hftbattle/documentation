#Order
include/simulator/order/order.h



Класс Order представляет собой реализацию заявки (надеемся, вы уже знакомы с тем что это такое! Если нет - то вам сюда). Ничего необычного, поэтому переходим сразу к


* [Order.outer_id() const](#outer_id)
* [Order.origin_server_time](#origin_server_time)
* [Order.price](#price)
* [Order.amount](#amount)
* [Order.dir](#dir)
* [Order.time_in_force](#time_in_force)
* [Order.security](#security)
* [Order.amount_rest() const](#amount_rest)
* [Order.implied_amount() const](#implied_amount)
* [Order.status() const](#status)

#Основные поля и методы класса

```cpp
inline xor_platform::Id outer_id() const;
```id заявки

```cpp
int64_t origin_server_time;
```биржевое время постановки заявки в стакан в тиках

```cpp
const xor_platform::Decimal price;
```цена заявки

```cpp
const int32_t amount;
```количество лотов

```cpp
const Dir dir;
```направление

```cpp
const xor_platform::OrderTimeInForce time_in_force;
```тип заявки - Limit или IOC

```cpp
const Security * security;
```инструмент, к которому относится заявка

```cpp
inline int32_t amount_rest() const;
```текущее количество лотов в заявке (может быть меньше начального, если были сделки с ее участием)

```cpp
inline int32_t implied_amount() const;
```количество лотов, которое предположительно будет сведено

```cpp
inline xor_platform::OrderStatus status() const;
```статус заявки - активная, ждущая удаления, удаленная

