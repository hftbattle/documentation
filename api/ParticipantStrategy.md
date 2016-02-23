#ParticipantStrategy
Путь в Local Pack-е: `include/strategy/participant_strategy_layer.h`

ParticipantStrategy - класс-интерфейс для написания пользовательских стратегий.
Этот класс служит прослойкой между симуляционным ядром и стратегией.

Он обеспечивает обработку входящих сигналов от симуляции:
обновления торговых и сигнальных стаканов, сделок,
отчеты о наших сделках, сообщения об окончании биржевого события.

Он же осуществляет передачу исходящих сигналов в симуляцию:
постановка, снятие и перемещение заявок.

Помимо реализации методов для описанных выше действий,
класс также предоставляет некоторые вспомогательные методы для удобства работы.

Внутри класса имеется разделение данных на 2 типа: торговые и сигнальные.
Первый тип означает, что данные относятся к инструменту, на котором стратегия торгует.
Второй тип означает, что данные отосятся к инструменту, на который стратегия смотрит.

Классы-стратегии участников должны наследовать от класса ParticipantStrategy.

###Методы

|Имя| Описание|
|------------------|--------------------|
|[trading_book_update(const OrderBook& order_book)](#trading_book_update)|Вызывается при получении нового стакана торгового инструмента.|
|[trading_deals_update(const std::vector< Deal >& deals)](#trading_deals_update)|Вызывается при получении новых сделок торгового инструмента.|
|[execution_report_update(const ExecutionReportSnapshot& snapshot)](#execution_report_update)|Вызывается при получении отчета о сделке с участием вашего ордера.|
|[add_limit_order(Dir dir, Price price, Amount amount)](#add_limit_order)|Выставляет нашу лимитную заявку.|
|[add_ioc_order(Dir dir, Price price, Amount amount)](#add_ioc_order)|Выставляет нашу заявку типа Immediate-Or-Cancel (IOC).|
|[delete_order(Order* order)](#delete_order)|Снимает нашу заявку с торгов.|
|[delete_all_orders_by_dir(Dir dir)](#delete_all_orders_by_dir)|Снимает все наши заявки с торгов по направлению *dir*.|
|[get_amount_before_order(Order* order)](#get_amount_before_order)|Возвращает количество лотов, стоящих в очереди перед нашей заявкой.|
|[get_current_result()](#get_current_result)|Возвращает текущий результат (заработок).|
|[get_saldo()](#get_saldo)|Возвращает текущее сальдо, т.е. баланс без учета позы.|
|[signal_security_exists()](#signal_security_exists)|Возвращает true, если есть сигнальный инструмент. Иначе false.|
|[get_local_time()](#get_local_time)|Возвращает локальное время в микросекундах.|
|[get_server_time()](#get_server_time)|Возвращает биржевое время с точностью до микросекунд.|
|[get_server_time_tm()](#get_server_time_tm)|Возвращает биржевое время типа tm c точностью до секунды.|

###Поля

|Имя| Описание|
|------------------|--------------------|
|[trading_book_info](#trading_book_info)|Стуктура-аггрегатор основной информации о торговом стакане.|
|[trading_book_snapshot](#trading_book_snapshot)|Умный указатель на текущий стакан торгового инструмента.|
|[trading_book](#trading_book)|Указатель на стакан торгового инструментов.|
|[signal_book](#signal_book)|Указатель на стакан сигнального инструментов.|

###Описание методов
<a name="trading_book_update"></a>
####trading_book_update()
```c++
virtual void trading_book_update(const OrderBook& order_book);
```
Вызывается при получении нового стакана торгового инструмента:
- *order_book* – новый стакан.

<a name="trading_deals_update"></a>
####trading_deals_update()
```c++
virtual void trading_deals_update(const std::vector<Deal>& deals);
```
Вызывается при получении новых сделок торгового инструмента:
- *deals* - вектор новых сделок.

<a name="execution_report_update"></a>
####execution_report_update()
```c++
virtual void execution_report_update(const ExecutionReportSnapshot& snapshot);
```
Вызывается при получении отчета о сделке с участием вашего ордера:
- *snapshot* – структура-отчет о совершенной сделке.

<a name="add_limit_order"></a>
####add_limit_order()
```c++
bool add_limit_order(Dir dir, Price price, Amount amount);
```
Выставляет нашу лимитную заявку:
- *dir* - направление (BID = 0 - покупка, ASK = 1 - продажа),
- *price* - цена, по которой заявка будет выставлена,
- *amount* - размер заявки.

<a name="add_ioc_order"></a>
####add_ioc_order()
```c++
bool add_ioc_order(Dir dir, Price price, Amount amount);
```
Выставляет нашу заявку типа Immediate-Or-Cancel (IOC):
- *dir* - направление (BID = 0 - покупка, ASK = 1 - продажа),
- *price* - цена, по которой заявка будет выставлена,
- *amount* - размер заявки.

<a name="delete_order"></a>
####delete_order()
```c++
void delete_order(Order* order);
```
Снимает нашу заявку с торгов:
- *order* - заявка, которую мы хотим снять.

<a name="delete_all_orders_by_dir"></a>
####delete_all_orders_by_dir()
```c++
void delete_all_orders_by_dir(Dir dir);
```
Снимает все наши заявки с торгов по направлению *dir*.
- *dir* - направление (BID = 0 - покупка, ASK = 1 - продажа).

<a name="get_amount_before_order"></a>
####get_amount_before_order()
```c++
Amount get_amount_before_order(Order* order) const;
```
Возвращает количество лотов, стоящих в очереди перед нашей заявкой.
- *order* - заявка, для которой мы хотим узнать количество стоящих перед ней лотов.

<a name="get_current_result"></a>
####get_current_result()
```c++
Price get_current_result() const;
```
Возвращает текущий результат (заработок).

<a name="get_saldo"></a>
####get_saldo()
```c++
Price get_saldo();
```
Возвращает текущее сальдо, т.е. баланс без учета позы.

<a name="signal_security_exists"></a>
####signal_security_exists()
```c++
bool signal_security_exists() const;
```
Возвращает true, если есть сигнальный инструмент. Иначе false.

<a name="get_local_time"></a>
####get_local_time()
```c++
Microseconds get_local_time() const;
```
Возвращает локальное время в микросекундах. Локальное время здесь – это время на машине, получающей биржевые данные.

<a name="get_server_time"></a>
####get_server_time()
```c++
Microseconds get_server_time() const;
```
Возвращает биржевое время с точностью до микросекунд.

<a name="get_server_time_tm"></a>
####get_server_time_tm()
```c++
tm get_server_time_tm() const;
```
Возвращает биржевое время типа tm c точностью до секунды.


###Описание полей
<a name="trading_book_info"></a>
####trading_book_info
```c++
ContestBookInfo trading_book_info;
```
Стуктура-аггрегатор основной информации о торговом стакане.

<a name="trading_book_snapshot"></a>
####trading_book_snapshot
```c++
SharedPtr<DataFeedSnapshot> trading_book_snapshot;
```
Умный указатель на текущий стакан торгового инструмента. Они обновляются каждый раз с приходом очередного апдейта торгового стакана. При этом объект внутри (стакан) разрушается. Чтобы сохранить старый стакан, нужно явно в стратегии сохранить этот указатель.

<a name="trading_book"></a>
####trading_book
```c++
const OrderBook* trading_book;
```
Указатель на стакан торгового инструментов.



