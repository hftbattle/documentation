# ParticipantStrategy

Путь в Local Pack-е: `include/participant_strategy.h`

ParticipantStrategy - класс-интерфейс для написания пользовательских стратегий.
Этот класс служит прослойкой между ядром симуляции и стратегией.

Он обеспечивает обработку входящих сигналов от симуляции:
обновления торгового стакана, сделок, отчеты о наших сделках.
Он же осуществляет передачу исходящих сигналов в симуляцию: постановка и снятие заявок.

Помимо реализации методов для описанных выше действий,
класс также предоставляет некоторые вспомогательные методы для удобства работы.

Классы-стратегии участников должны наследовать от класса ParticipantStrategy.

### Методы

|Имя| Описание|
|------------------|--------------------|
|[trading_book_update(const OrderBook& order_book)](#trading_book_update)|Вызывается при получении нового стакана торгового инструмента.|
|[trading_deals_update(const std::vector<Deal>& deals)](#trading_deals_update)|Вызывается при получении новых сделок торгового инструмента.|
|[execution_report_update(const ExecutionReport& execution_report)](#execution_report_update)|Вызывается при получении отчета о сделке с участием вашего ордера.|
|[signal_book_update(const OrderBook& order_book)](#signal_book_update)|Вызывается при получении нового стакана сигнального инструмента.|
|[signal_deals_update(const std::vector<Deal>& deals)](#signal_deals_update)|Вызывается при получении новых сделок сигнального инструмента.|
|[add_limit_order(Dir dir, Price price, Amount amount, const std::string& comment = {})](#add_limit_order)|Выставляет нашу лимитную заявку.|
|[add_ioc_order(Dir dir, Price price, Amount amount, const std::string& comment = {})](#add_ioc_order)|Выставляет нашу заявку типа Immediate-Or-Cancel (IOC).|
|[delete_order(Order* order)](#delete_order)|Снимает нашу заявку с торгов.|
|[delete_all_orders_by_dir(Dir dir)](#delete_all_orders_by_dir)|Снимает все наши заявки с торгов по направлению *dir*.|
|[get_amount_before_order(const Order* order)](#get_amount_before_order)|Возвращает количество лотов, стоящих в очереди перед нашей заявкой.|
|[get_volume_at_price(Dir dir, Price price)](#get_volume_at_price)|Возвращает суммарное количество лотов в наших активных заявках, стоящих на определённой цене.|
|[add_chart_point(const std::string& line_name, double value, ChartYAxisType y_axis_type, uint8_t chart_number)](#add_chart_point)|Добавляет точку на график.|
|[get_current_result()](#get_current_result)|Возвращает текущий результат (заработок).|
|[get_saldo()](#get_saldo)|Возвращает текущее сальдо, т.е. баланс без учета позы.|
|[signal_security_exists()](#signal_security_exists)|Возвращает true, если есть сигнальный инструмент. Иначе false.|
|[get_local_time()](#get_local_time)|Возвращает локальное время в микросекундах.|
|[get_server_time()](#get_server_time)|Возвращает биржевое время с точностью до микросекунд.|
|[get_server_time_tm()](#get_server_time_tm)|Возвращает биржевое время типа tm с точностью до секунды.|
|[set_max_total_amount(const uint32_t max_total_amount)](#set_max_total_amount)|Устанавливает максимальное разрешённое значение позиции (не более 50).|
|[set_stop_loss_result(const Decimal stop_loss_result)](#set_stop_loss_result)|Устанавливает минимально допустимое значение, при достижении которого позиция закрывается, и стратегия перестаёт торговать.|

### Поля

|Имя| Описание|
|------------------|--------------------|
|[trading_book_info](#trading_book_info)|Структура-агрегатор основной информации о торговом стакане.|
|[signal_book_info](#signal_book_info)|Структура-агрегатор основной информации о сигнальном стакане.|
|[trading_book](#trading_book)|Умный указатель на текущий стакан торгового инструмента.|
|[signal_book](#signal_book)|Аналогично trading_book для сигнального инструмента.|

### Описание методов

#### trading_book_update()<a id="trading_book_update"></a>

```c++
virtual void trading_book_update(const OrderBook& order_book);
```

Вызывается при получении нового стакана торгового инструмента:
- *order_book* – новый стакан.

#### trading_deals_update()<a id="trading_deals_update"></a>

```c++
virtual void trading_deals_update(const std::vector<Deal>& deals);
```

Вызывается при получении новых сделок торгового инструмента:
- *deals* - вектор новых сделок.

#### execution_report_update()<a id="execution_report_update"></a>

```c++
virtual void execution_report_update(const ExecutionReport& execution_report);
```

Вызывается при получении отчета о сделке с участием вашего ордера:
- *snapshot* – структура-отчет о совершенной сделке.

#### signal_book_update()<a id="signal_book_update"></a>

```c++
virtual void signal_book_update(const OrderBook& order_book);
```

Вызывается при получении нового стакана сигнального инструмента:
- *order_book* – новый стакан.

#### signal_deals_update()<a id="signal_deals_update"></a>

```c++
virtual void signal_deals_update(const std::vector<Deal>& deals);
```

Вызывается при получении новых сделок сигнального инструмента:
- *deals* - вектор новых сделок.

#### add_limit_order()<a id="add_limit_order"></a>

```c++
bool add_limit_order(Dir dir, Price price, Amount amount, const std::string& comment =;
```

Выставляет нашу лимитную заявку:
- *dir* - направление (BID = 0 - покупка, ASK = 1 - продажа),
- *price* - цена, по которой заявка будет выставлена,
- *amount* - размер заявки.
- *comment* - комментарий к заявке.

#### add_ioc_order()<a id="add_ioc_order"></a>

```c++
bool add_ioc_order(Dir dir, Price price, Amount amount, const std::string& comment =;
```

Выставляет нашу заявку типа Immediate-Or-Cancel (IOC):
- *dir* - направление (BID = 0 - покупка, ASK = 1 - продажа),
- *price* - цена, по которой заявка будет выставлена,
- *amount* - размер заявки.
- *comment* - комментарий к заявке.

#### delete_order()<a id="delete_order"></a>

```c++
void delete_order(Order* order);
```

Снимает нашу заявку с торгов:
- *order* - заявка, которую мы хотим снять.

#### delete_all_orders_by_dir()<a id="delete_all_orders_by_dir"></a>

```c++
void delete_all_orders_by_dir(Dir dir);
```

Снимает все наши заявки с торгов по направлению *dir*.
- *dir* - направление (BID = 0 - покупка, ASK = 1 - продажа).

#### get_amount_before_order()<a id="get_amount_before_order"></a>

```c++
Amount get_amount_before_order(const Order* order) const;
```

Возвращает количество лотов, стоящих в очереди перед нашей заявкой.
- *order* - заявка, для которой мы хотим узнать количество стоящих перед ней лотов.

#### get_volume_at_price()<a id="get_volume_at_price"></a>

```c++
Amount get_volume_at_price(Dir dir, Price price) const;
```

Возвращает суммарное количество лотов в наших активных заявках, стоящих на определённой цене.
- *dir* - направление (BID = 0 - покупка, ASK = 1 - продажа).
- *price* - цена, объём лотов на которой мы хотим узнать.

#### add_chart_point()<a id="add_chart_point"></a>

```c++
void add_chart_point(const std::string& line_name, double value, ChartYAxisType y_axis_type, uint8_t chart_number);
```

Добавляет точку на график.
- *line_name* - название графика, *value* - значение, *y_axis_type* - ось (левая или правая),
- *chart_number* - номер картинки на которой будет нарисован график.

#### get_current_result()<a id="get_current_result"></a>

```c++
Price get_current_result() const;
```

Возвращает текущий результат (заработок).

#### get_saldo()<a id="get_saldo"></a>

```c++
Price get_saldo();
```

Возвращает текущее сальдо, т.е. баланс без учета позы.

#### signal_security_exists()<a id="signal_security_exists"></a>

```c++
bool signal_security_exists() const;
```

Возвращает true, если есть сигнальный инструмент. Иначе false.

#### get_local_time()<a id="get_local_time"></a>

```c++
Microseconds get_local_time() const;
```

Возвращает локальное время в микросекундах. Локальное время здесь – это время на машине, получающей биржевые данные.

#### get_server_time()<a id="get_server_time"></a>

```c++
Microseconds get_server_time() const;
```

Возвращает биржевое время с точностью до микросекунд.

#### get_server_time_tm()<a id="get_server_time_tm"></a>

```c++
tm get_server_time_tm() const;
```

Возвращает биржевое время типа tm c точностью до секунды.

#### set_max_total_amount()<a id="set_max_total_amount"></a>

```c++
void set_max_total_amount(const uint32_t max_total_amount);
```

Устанавливает максимальное разрешённое значение позиции (не более 50).

#### set_stop_loss_result()<a id="set_stop_loss_result"></a>

```c++
void set_stop_loss_result(const Decimal stop_loss_result);
```

Устанавливает минимально допустимое значение, при достижении которого позиция закрывается, и стратегия перестаёт торговать.

### Описание полей

#### trading_book_info<a id="trading_book_info"></a>

```c++
ContestBookInfo trading_book_info;
```

Структура-агрегатор основной информации о торговом стакане.

#### signal_book_info<a id="signal_book_info"></a>

```c++
ContestBookInfo signal_book_info;
```

Структура-агрегатор основной информации о сигнальном стакане.

#### trading_book<a id="trading_book"></a>

```c++
std::shared_ptr<const OrderBook> trading_book;
```

Умный указатель на текущий стакан торгового инструмента. Они обновляются каждый раз с приходом очередного апдейта торгового стакана. При этом объект внутри (стакан) разрушается. Чтобы сохранить старый стакан, нужно явно в стратегии сохранить этот указатель.

#### signal_book<a id="signal_book"></a>

```c++
std::shared_ptr<const OrderBook> signal_book;
```

Аналогично trading_book для сигнального инструмента.
