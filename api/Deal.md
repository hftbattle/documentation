#Deal

`include/simulator/deal/deal.h`


Сделка - это акт (ссылка) купли-продажи определенного инструмента.


###Поля


|Имя| Описание|
|------------------|--------------------|
|[server_time](#server_time)|Биржевое время совершения сделки.|
|[passive_order_server_time](#passive_order_server_time)|Время постановки "пассивного" ордера, если оно известно.|
|[price](#price)|Цена по которой произошла сделка.|
|[amount](#amount)|Количество лотов в сделке.|
|[implied_amount](#implied_amount)|Количество лотов, которые предположительно должны были свестись в случае, когда налетающая заявка была нашей.|
|[dir](#dir)|Направление сделки (покупка или продажа).|

###Методы


|Имя| Описание|
|------------------|--------------------|
|[get_comments()](#get_comments)|Комментарии к заявкам, участвующим в сделке.|
|[get_orders()](#get_orders)|Вектор заявок (на продажу и на покупку), участвующих в сделке.|
|[get_tsc()](#get_tsc)|Время машины, на которой сохранялись данные.|
|[get_agressor_side()](#get_agressor_side)|Направление налетающей заявки.|
|[is_our()](#is_our)|Наша ли это сделка.|

###Описание полей
<a id="server_time"></a>
####server_time
```c++
xor_platform::Microseconds server_time;
```
Биржевое время совершения сделки.

<a id="passive_order_server_time"></a>
####passive_order_server_time
```c++
xor_platform::Microseconds passive_order_server_time;
```
Время постановки "пассивного" ордера, если оно известно.

<a id="price"></a>
####price
```c++
xor_platform::Decimal price;
```
Цена по которой произошла сделка.

<a id="amount"></a>
####amount
```c++
xor_platform::Amount amount;
```
Количество лотов в сделке.

<a id="implied_amount"></a>
####implied_amount
```c++
xor_platform::Amount implied_amount;
```
Количество лотов, которые предположительно должны были свестись в случае, когда налетающая заявка была нашей.

<a id="dir"></a>
####dir
```c++
xor_platform::Dir dir;
```
Направление сделки (покупка или продажа).


###Описание методов
<a id="get_comments"></a>
####get_comments()
```c++
std::vector<std::string> const& get_comments() const;
```
Комментарии к заявкам, участвующим в сделке.

<a id="get_orders"></a>
####get_orders()
```c++
std::vector<std::shared_ptr<Order>> const& get_orders() const;
```
Вектор заявок (на продажу и на покупку), участвующих в сделке.

<a id="get_tsc"></a>
####get_tsc()
```c++
inline int64_t get_tsc() const;
```
Время машины, на которой сохранялись данные.

<a id="get_agressor_side"></a>
####get_agressor_side()
```c++
inline xor_platform::Dir get_agressor_side() const;
```
Направление налетающей заявки.

<a id="is_our"></a>
####is_our()
```c++
bool is_our() const;
```
Наша ли это сделка.

