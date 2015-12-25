#Deal
`include/simulator/deal/deal.h`


Сделка - это акт (ссылка) купли-продажи определенного инструмента.


|Имя| Описание|
|------------------|--------------------|
|[Deal.server_time](#server_time)|биржевое время совершения сделки|
|[Deal.passive_order_server_time](#passive_order_server_time)|время постановки "пассивного" ордера, если оно известно|
|[Deal.price](#price)|цена по которой произошла сделка|
|[Deal.amount](#amount)|количество лотов в сделке|
|[Deal.implied_amount](#implied_amount)|количество лотов, которые предположительно должны были свестись в случае, когда налетающая заявка была нашей|
|[Deal.dir](#dir)|направление сделки (покупка или продажа)|
|[Deal.get_comments() const](#get_comments)|комментарии к заявкам, участвующим в сделке|
|[Deal.get_orders() const](#get_orders)|вектор заявок (на продажу и на покупку), участвующих в сделке|
|[Deal.get_tsc() const](#get_tsc)|время машины, на которой сохранялись данные|
|[Deal.get_agressor_side() const](#get_agressor_side)|направление налетающей заявки|
|[Deal.is_our() const](#is_our)|наша ли это сделка|

#Основные поля и методы класса

```cpp
xor_platform::Microseconds server_time;
```
биржевое время совершения сделки

```cpp
xor_platform::Microseconds passive_order_server_time;
```
время постановки "пассивного" ордера, если оно известно

```cpp
xor_platform::Decimal price;
```
цена по которой произошла сделка

```cpp
xor_platform::Amount amount;
```
количество лотов в сделке

```cpp
xor_platform::Amount implied_amount;
```
количество лотов, которые предположительно должны были свестись в случае, когда налетающая заявка была нашей

```cpp
xor_platform::Dir dir;
```
направление сделки (покупка или продажа)

```cpp
std::vector<std::string> const& get_comments() const;
```
комментарии к заявкам, участвующим в сделке

```cpp
std::vector<std::shared_ptr<Order>> const& get_orders() const;
```
вектор заявок (на продажу и на покупку), участвующих в сделке

```cpp
inline int64_t get_tsc() const;
```
время машины, на которой сохранялись данные

```cpp
inline xor_platform::Dir get_agressor_side() const;
```
направление налетающей заявки

```cpp
bool is_our() const;
```
наша ли это сделка

