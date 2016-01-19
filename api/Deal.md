#Deal
`simulator/deal/deal.h`

Сделка - это акт купли-продажи определенного инструмента.
Класс Deal представляет собой реализацию биржевой сделки.

###Поля

|Имя| Описание|
|------------------|--------------------|
|[server_time](#server_time)|Биржевое время совершения сделки.|
|[passive_order_server_time](#passive_order_server_time)|Время постановки "пассивного" ордера, если оно известно.|
|[price](#price)|Цена по которой произошла сделка.|
|[amount](#amount)|Количество лотов в сделке.|
|[implied_amount](#implied_amount)|Количество лотов, которые предположительно должны были свестись в случае, когда налетающая заявка была нашей.|
|[dir](#dir)|Направление сделки (покупка или продажа).|
|[origin_time](#origin_time)|Локальное время в тиках.|
|[outer_id](#outer_id)|Биржевой (внешний) ID сделки.|

###Методы

|Имя| Описание|
|------------------|--------------------|
|[get_comments()](#get_comments)|Комментарии к заявкам, участвующим в сделке.|
|[get_orders()](#get_orders)|Вектор заявок (на продажу и на покупку), участвующих в сделке.|
|[get_tsc()](#get_tsc)|Локальное время.|
|[get_agressor_side()](#get_agressor_side)|Направление налетающей заявки.|
|[is_our()](#is_our)|Наша ли это сделка.|
|[get_amount()](#get_amount)|Объем сделки.|
|[get_moment()](#get_moment)|Биржевое время сделки.|
|[get_price()](#get_price)|Цена, по которой произошла сделка.|

###Описание полей
<a name="server_time"></a>
####server_time
```c++
Microseconds server_time;
```
Биржевое время совершения сделки.

<a name="passive_order_server_time"></a>
####passive_order_server_time
```c++
Microseconds passive_order_server_time;
```
Время постановки "пассивного" ордера, если оно известно.

<a name="price"></a>
####price
```c++
Price price;
```
Цена по которой произошла сделка.

<a name="amount"></a>
####amount
```c++
Amount amount;
```
Количество лотов в сделке.

<a name="implied_amount"></a>
####implied_amount
```c++
Amount implied_amount;
```
Количество лотов, которые предположительно должны были свестись в случае, когда налетающая заявка была нашей.

<a name="dir"></a>
####dir
```c++
Dir dir;
```
Направление сделки (покупка или продажа).

<a name="origin_time"></a>
####origin_time
```c++
Ticks origin_time;
```
Локальное время в тиках.

<a name="outer_id"></a>
####outer_id
```c++
int64_t outer_id;
```
Биржевой (внешний) ID сделки.


###Описание методов
<a name="get_comments"></a>
####get_comments()
```c++
std::vector<std::string> const& get_comments() const;
```
Комментарии к заявкам, участвующим в сделке.

<a name="get_orders"></a>
####get_orders()
```c++
std::vector<std::shared_ptr<Order>> const& get_orders() const;
```
Вектор заявок (на продажу и на покупку), участвующих в сделке.

<a name="get_tsc"></a>
####get_tsc()
```c++
inline int64_t get_tsc() const;
```
Локальное время.

<a name="get_agressor_side"></a>
####get_agressor_side()
```c++
inline Dir get_agressor_side() const;
```
Направление налетающей заявки.

<a name="is_our"></a>
####is_our()
```c++
bool is_our() const;
```
Наша ли это сделка.

<a name="get_amount"></a>
####get_amount()
```c++
inline Amount get_amount() const;
```
Объем сделки.

<a name="get_moment"></a>
####get_moment()
```c++
inline int64_t get_moment() const;
```
Биржевое время сделки.

<a name="get_price"></a>
####get_price()
```c++
inline Price get_price() const;
```
Цена, по которой произошла сделка.


