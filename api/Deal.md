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

```c++
xor_platform::Microseconds server_time;
```
биржевое время совершения сделки

```c++
xor_platform::Microseconds passive_order_server_time;
```
время постановки "пассивного" ордера, если оно известно

```c++
xor_platform::Decimal price;
```
цена по которой произошла сделка

```c++
xor_platform::Amount amount;
```
количество лотов в сделке

```c++
xor_platform::Amount implied_amount;
```
количество лотов, которые предположительно должны были свестись в случае, когда налетающая заявка была нашей

```c++
xor_platform::Dir dir;
```
направление сделки (покупка или продажа)

```c++
std::vector<std::string> const& get_comments() const;
```
комментарии к заявкам, участвующим в сделке

```c++
std::vector<std::shared_ptr<Order>> const& get_orders() const;
```
вектор заявок (на продажу и на покупку), участвующих в сделке

```c++
inline int64_t get_tsc() const;
```
время машины, на которой сохранялись данные

```c++
inline xor_platform::Dir get_agressor_side() const;
```
направление налетающей заявки

```c++
bool is_our() const;
```
наша ли это сделка

