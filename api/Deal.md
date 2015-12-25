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

<a id="server_time"></a>
###server_time
```c++
xor_platform::Microseconds server_time;
```
биржевое время совершения сделки

<a id="passive_order_server_time"></a>
###passive_order_server_time
```c++
xor_platform::Microseconds passive_order_server_time;
```
время постановки "пассивного" ордера, если оно известно

<a id="price"></a>
###price
```c++
xor_platform::Decimal price;
```
цена по которой произошла сделка

<a id="amount"></a>
###amount
```c++
xor_platform::Amount amount;
```
количество лотов в сделке

<a id="implied_amount"></a>
###implied_amount
```c++
xor_platform::Amount implied_amount;
```
количество лотов, которые предположительно должны были свестись в случае, когда налетающая заявка была нашей

<a id="dir"></a>
###dir
```c++
xor_platform::Dir dir;
```
направление сделки (покупка или продажа)

<a id="get_comments"></a>
###get_comments
```c++
std::vector<std::string> const& get_comments() const;
```
комментарии к заявкам, участвующим в сделке

<a id="get_orders"></a>
###get_orders
```c++
std::vector<std::shared_ptr<Order>> const& get_orders() const;
```
вектор заявок (на продажу и на покупку), участвующих в сделке

<a id="get_tsc"></a>
###get_tsc
```c++
inline int64_t get_tsc() const;
```
время машины, на которой сохранялись данные

<a id="get_agressor_side"></a>
###get_agressor_side
```c++
inline xor_platform::Dir get_agressor_side() const;
```
направление налетающей заявки

<a id="is_our"></a>
###is_our
```c++
bool is_our() const;
```
наша ли это сделка

