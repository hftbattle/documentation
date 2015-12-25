#Deal
include/simulator/deal/deal.h



Сделка - это акт (ссылка) купли-продажи определенного инструмента.


* [Deal.server_time](#server_time)
* [Deal.passive_order_server_time](#passive_order_server_time)
* [Deal.price](#price)
* [Deal.amount](#amount)
* [Deal.implied_amount](#implied_amount)
* [Deal.dir](#dir)
* [Deal.get_comments() const](#get_comments)
* [Deal.get_orders() const](#get_orders)
* [Deal.get_tsc() const](#get_tsc)
* [Deal.get_agressor_side() const](#get_agressor_side)
* [Deal.is_our() const](#is_our)

#Основные поля и методы класса

```cpp
xor_platform::Microseconds server_time;
```биржевое время совершения сделки

```cpp
xor_platform::Microseconds passive_order_server_time;
```время постановки "пассивного" ордера, если оно известно

```cpp
xor_platform::Decimal price;
```цена по которой произошла сделка

```cpp
xor_platform::Amount amount;
```количество лотов в сделке

```cpp
xor_platform::Amount implied_amount;
```количество лотов, которые предположительно должны были свестись в случае, когда налетающая заявка была нашей

```cpp
xor_platform::Dir dir;
```направление сделки (покупка или продажа)

```cpp
std::vector<std::string> const& get_comments() const;
```комментарии к заявкам, участвующим в сделке

```cpp
std::vector<std::shared_ptr<Order>> const& get_orders() const;
```вектор заявок (на продажу и на покупку), участвующих в сделке

```cpp
inline int64_t get_tsc() const;
```время машины, на которой сохранялись данные

```cpp
inline xor_platform::Dir get_agressor_side() const;
```направление налетающей заявки

```cpp
bool is_our() const;
```наша ли это сделка

