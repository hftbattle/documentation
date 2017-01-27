# ParticipantStrategy

Путь в Local Pack `include/participant_strategy.h`

Класс-обёртка для стратегий участников для взаимодействия с торговым симулятором.

### Методы

| Имя | Описание |
| --- | --- |
| [trading_book_update()](#trading_book_update) | Вызывается симулятором при получении нового стакана торгового инструмента. |
| [trading_deals_update()](#trading_deals_update) | Вызывается симулятором при получении новых сделок торгового инструмента. |
| [execution_report_update()](#execution_report_update) | Вызывается симулятором при получении отчёта о сделке с участием вашей заявки. |
| [signal_book_update()](#signal_book_update) | Вызывается симулятором при получении нового стакана сигнального инструмента. |
| [signal_deals_update()](#signal_deals_update) | Вызывается симулятором при получении новых сделок сигнального инструмента. |
| [add_limit_order()](#add_limit_order) | Выставляет лимитную заявку. |
| [add_ioc_order()](#add_ioc_order) | Выставляет заявку типа IOC (Immediate-Or-Cancel). |
| [delete_order()](#delete_order) | Снимает вашу заявку с торгов. |
| [delete_all_orders_at_dir()](#delete_all_orders_at_dir) | Удаляет все ваши заявки в данном направлении. |
| [delete_all_orders_at_price()](#delete_all_orders_at_price) | Удаляет все ваши заявки с заданной ценой и направлением. |
| [amount_before_order()](#amount_before_order) | Объём заявок в очереди перед вашей заявкой. |
| [volume_by_price()](#volume_by_price) | Объём активных заявок с заданной ценой и направлением. |
| [add_chart_point()](#add_chart_point) | Добавляет точку на график. |
| [current_result()](#current_result) | Текущий заработок стратегии, учитывающий все заявки. |
| [saldo()](#saldo) | Текущий заработок стратегии, учитывающий только исполненные заявки. |
| [signal_security_exists()](#signal_security_exists) | Существует ли сигнальный инструмент |
| [local_time()](#local_time) | Текущее локальное время. |
| [server_time()](#server_time) | Текущее биржевое время. |
| [server_time_tm()](#server_time_tm) | Биржевое время типа tm. |
| [set_max_total_amount()](#set_max_total_amount) | Устанавливает желаемое значение максимальной позы. |
| [set_stop_loss_result()](#set_stop_loss_result) | Устанавливает желаемое значение минимального результата. |
| [executed_amount()](#executed_amount) | Наша текущая позиция, учитывающая только исполненные лоты. |
| [total_amount()](#total_amount) | Наша текущая позиция, учитывающая все лоты. |
| [is_our()](#is_our) | Является ли заявка вашей. |
| [is_our()](#is_our) | Является ли сделка вашей. |
| [trading_book()](#trading_book) | Ссылка на текущий стакан торгового инструмента. |
| [signal_book()](#signal_book) | Ссылка на текущий стакан сигнального инструмента. |

### Описание методов

#### trading_book_update() {#trading_book_update}

Вызывается симулятором при получении нового стакана торгового инструмента.

```c++
virtual void trading_book_update(const OrderBook& /*order_book*/);
```

#### trading_deals_update() {#trading_deals_update}

Вызывается симулятором при получении новых сделок торгового инструмента.

```c++
virtual void trading_deals_update(std::vector<Deal>&& /*deals*/);
```

#### execution_report_update() {#execution_report_update}

Вызывается симулятором при получении отчёта о сделке с участием вашей заявки.

```c++
virtual void execution_report_update(const ExecutionReport& /*execution_report*/);
```

#### signal_book_update() {#signal_book_update}

Вызывается симулятором при получении нового стакана сигнального инструмента.

```c++
virtual void signal_book_update(const OrderBook& /*order_book*/);
```

#### signal_deals_update() {#signal_deals_update}

Вызывается симулятором при получении новых сделок сигнального инструмента.

```c++
virtual void signal_deals_update(std::vector<Deal>&& /*deals*/);
```

#### add_limit_order() {#add_limit_order}

Принимает направление dir (BID (покупка) или ASK (продажа)), цену price и размер заявки amount.
Выставляет лимитную заявку.
Возвращает значение типа bool — была ли ваша заявка принята торговой системой.
Внимание: заявка может быть не принята по нескольким причинам.
Подробнее читайте в документации.
TODO(asalikhov): add links to docs.

```c++
bool add_limit_order(Dir dir, Price price, Amount amount);
```

#### add_ioc_order() {#add_ioc_order}

Принимает направление dir (BID (покупка) или ASK (продажа)), цену price и размер заявки amount.
Выставляет заявку типа IOC (Immediate-Or-Cancel).
Возвращает значение типа bool — была ли принята ваша заявка.
Внимание: заявка может быть не принята по нескольким причинам.
Подробнее читайте в документации.
TODO(asalikhov): add links to docs.

```c++
bool add_ioc_order(Dir dir, Price price, Amount amount);
```

#### delete_order() {#delete_order}

Принимает указатель на вашу заявку, т.е. указатель на объект класса Order.
Снимает эту заявку с торгов.
Внимание: удаление происходит немоментально.
Подробнее читайте в документации.
TODO(asalikhov): add links to docs.

```c++
void delete_order(Order* order);
```

#### delete_all_orders_at_dir() {#delete_all_orders_at_dir}

Принимает направление dir (BID (покупка) или ASK (продажа)).
Удаляет все ваши заявки по направлению dir.

```c++
void delete_all_orders_at_dir(Dir dir);
```

#### delete_all_orders_at_price() {#delete_all_orders_at_price}

Принимает направление dir (BID (покупка) или ASK (продажа)) и цену price.
Удаляет все ваши заявки по направлению dir по данной цене.

```c++
void delete_all_orders_at_price(Dir dir, Price price);
```

#### amount_before_order() {#amount_before_order}

Принимает указатель на вашу заявку, т.е. указатель на объект класса Order.
Возвращает суммарное количество лотов, стоящих в очереди перед вашей заявкой в котировке данного ценового уровня.

```c++
Amount amount_before_order(const Order* order) const;
```

#### volume_by_price() {#volume_by_price}

Принимает направление dir (BID (покупка) или ASK (продажа)) и цену price.
Возвращает суммарное количество лотов в ваших активных заявках, стоящих на определённой цене.

```c++
Amount volume_by_price(Dir dir, Price price) const;
```

#### add_chart_point() {#add_chart_point}

Принимает строку line_name (название графика), value типа double — значение, которое хочется добавить, y_axis_type — с какой стороны будет нарисована ось y и chart_number — номер графика.
Добавляет точку на график в данный момент времени с желаемым значением value.
Соседние точки на графике соединяются отрезком.
В итоге получаем ломаную, которая и является графиком.
Внимание: благодаря параметру chart_number можно создавать несколько графиков, а с помощью line_name — можно рисовать несколько графиков на одном изображении.

```c++
void add_chart_point(const std::string& line_name, double value, ChartYAxisType y_axis_type,
```

#### current_result() {#current_result}

Возвращает текущий результат стратегии (заработок).
Учитываются как исполненные, так и просто поставленные заявки.

```c++
Price current_result() const;
```

#### saldo() {#saldo}

Возвращает текущий результат стратегии (заработок).
Учитываются только исполненные заявки.

```c++
Price saldo();
```

#### signal_security_exists() {#signal_security_exists}

Возвращает значение типа bool — существует ли сигнальный инструмент.

```c++
bool signal_security_exists() const;
```

#### local_time() {#local_time}

Возвращает текущее локальное время в микросекундах.

```c++
Microseconds local_time() const;
```

#### server_time() {#server_time}

Возвращает текущее биржевое время в микросекундах.

```c++
Microseconds server_time() const;
```

#### server_time_tm() {#server_time_tm}

Возвращает биржевое время типа tm с точностью до секунды.
Примечание: биржевое время в таком формате может быть использовано для определения времени суток.

```c++
tm server_time_tm() const;
```

#### set_max_total_amount() {#set_max_total_amount}

Принимает желаемое значение максимальной позы — неотрицательное число не более 50.
Устанавливает данное значение позы максимальным и не разрешает стратегии превышать его по модулю.

```c++
void set_max_total_amount(const Amount max_total_amount);
```

#### set_stop_loss_result() {#set_stop_loss_result}

Принимает желаемое значение минимального результата.
Устанавливает это значение, при достижении которого позиция закрывается, и стратегия перестаёт торговать.

```c++
void set_stop_loss_result(const Decimal stop_loss_result);
```

#### executed_amount() {#executed_amount}

Возвращает вашу текущую позицию.
Учитываются только исполненные лоты.

```c++
Amount executed_amount() const;
```

#### total_amount() {#total_amount}

Возвращает вашу общую позицию: учитывается как исполненные, так и просто поставленные заявки.

```c++
Amount total_amount() const;
```

#### is_our() {#is_our}

Принимает указатель на заявку, т.е. указатель на объект класса Order.
Возвращает значение типа bool — является ли данная заявка вашей.

```c++
bool is_our(const Order* order) const;
```

#### is_our() {#is_our}

Принимает ссылку на сделку, т.е. ссылку на объект класса Deal.
Возвращает значение типа bool — участвует ли в сделке ваша заявка.

```c++
bool is_our(const Deal& deal) const;
```

#### trading_book() {#trading_book}

Ссылка на текущий стакан торгового инструмента.

```c++
OrderBook& trading_book();
```

#### signal_book() {#signal_book}

Ссылка на текущий стакан сигнального инструмента.

```c++
OrderBook& signal_book();
```
