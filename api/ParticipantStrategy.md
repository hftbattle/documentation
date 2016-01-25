#ParticipantStrategy
Путь в LocalPack-е: `include/simulator/strategy/participant_strategy_layer.h`

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

###Поля

|Имя| Описание|
|------------------|--------------------|
|[trading_book](#trading_book)|Указатель на стакан торгового инструментов.|
|[signal_book](#signal_book)|Указатель на стакан сигнального инструментов.|
|[trading_book_snapshot](#trading_book_snapshot)|Умный указатель на текущий стакан торгового инструмента.|
|[signal_book_snapshot](#signal_book_snapshot)|Аналогично trading_book_snapshot для сигнального инструмента.|
|[trading_book_info](#trading_book_info)|Стуктура-аггрегатор основной информации о торговом стакане.|
|[signal_book_info](#signal_book_info)|Стуктура-аггрегатор основной информации о сигнальном стакане.|

###Методы

|Имя| Описание|
|------------------|--------------------|
|[trading_book_update(const OrderBook &order_book)](#trading_book_update)|Вызывается при получении нового стакана торгового инструмента.|
|[signal_book_update(const OrderBook &order_book)](#signal_book_update)|Вызывается при получении нового стакана сигнального инструмента.|
|[trading_deals_update(const std::vector<Deal> &deals)](#trading_deals_update)|Вызывается при получении новых сделок торгового инструмента.|
|[signal_deals_update(const std::vector<Deal> &deals)](#signal_deals_update)|Вызывается при получении новых сделок сигнального инструмента.|
|[execution_report_update(const ExecutionReportSnapshot &snapshot)](#execution_report_update)|Вызывается при получении отчета о сделке с участием вашего ордера.|
|[event_end_update()](#event_end_update)|Вызывается, когда симуляция закончила обрабатывать все изменения, соответствующие одному биржевому событию.|
|[add_order(Price price, Amount amount, Dir dir, Amount implied_amount = 0)](#add_order)|Выставляет нашу заявку.|
|[add_ioc_order(Price price, Amount amount, Dir dir)](#add_ioc_order)|Выставляет нашу заявку по принципу Immediate-Or-Cancel.|
|[add_ioc_order(Price price, Amount amount, Dir dir, Amount implied_amount)](#add_ioc_order)|Выставляет нашу заявку по принципу Immediate-Or-Cancel.|
|[delete_order(Order* order)](#delete_order)|Снимает наш ордер с торгов.|
|[active_orders_by_dir()](#active_orders_by_dir)|Возвращает массив списков наших активных заявок, то есть заявок со статусом OrderStatus::Adding и OrderStatus::Active для бида и аска соответственно.|
|[total_amount()](#total_amount)|Возвращает нашу текущую позу.|
|[executed_amount()](#executed_amount)|Возвращает нашу текущую позу без учета implied-заявок.|
|[total_active_amount(Dir dir)](#total_active_amount)|Возвращает текущий суммарный объем выставленных ордеров по направлению.|
|[count_added_orders(Dir dir)](#count_added_orders)|Возвращает количество активных ордеров по направлению.|
|[best_price(Dir dir, bool is_book_trade = true)](#best_price)|Возвращает лучшую цену по направлению.|
|[get_saldo()](#get_saldo)|Возвращает текущее сальдо (текущий баланс без учета позы).|
|[get_current_result()](#get_current_result)|Возвращает текущий результат (заработок).|
|[signal_security_exists()](#signal_security_exists)|Есть ли сигнальный инструмент.|
|[get_local_time()](#get_local_time)|Возвращает локальное время в микросекундах.|
|[get_server_time()](#get_server_time)|Возвращает биржевое время с точностью до микросекунд.|
|[get_server_time_tm()](#get_server_time_tm)|Возвращает биржевое время типа tm c точностью до секунды.|

###Описание полей
<a name="trading_book"></a>
####trading_book
```c++
const OrderBookL2* trading_book;
```
Указатель на стакан торгового инструментов.

<a name="signal_book"></a>
####signal_book
```c++
const OrderBookL2* signal_book;
```
Указатель на стакан сигнального инструментов.

<a name="trading_book_snapshot"></a>
####trading_book_snapshot
```c++
SharedPtr<DataFeedSnapshot> trading_book_snapshot;
```
Умный указатель на текущий стакан торгового инструмента. Они обновляются каждый раз с приходом очередного апдейта торгового стакана. При этом объект внутри (стакан) разрушается. Чтобы сохранить старый стакан, нужно явно в стратегии сохранить этот указатель.

<a name="signal_book_snapshot"></a>
####signal_book_snapshot
```c++
SharedPtr<DataFeedSnapshot> signal_book_snapshot;
```
Аналогично trading_book_snapshot для сигнального инструмента.

<a name="trading_book_info"></a>
####trading_book_info
```c++
ContestBookInfo trading_book_info;
```
Стуктура-аггрегатор основной информации о торговом стакане.

<a name="signal_book_info"></a>
####signal_book_info
```c++
ContestBookInfo signal_book_info;
```
Стуктура-аггрегатор основной информации о сигнальном стакане.


###Описание методов
<a name="trading_book_update"></a>
####trading_book_update()
```c++
virtual void trading_book_update(const OrderBook &order_book);
```
Вызывается при получении нового стакана торгового инструмента:
- *order_book* – новый стакан.

<a name="signal_book_update"></a>
####signal_book_update()
```c++
virtual void signal_book_update(const OrderBook &order_book);
```
Вызывается при получении нового стакана сигнального инструмента:
- *order_book* – новый стакан.

<a name="trading_deals_update"></a>
####trading_deals_update()
```c++
virtual void trading_deals_update(const std::vector<Deal> &deals);
```
Вызывается при получении новых сделок торгового инструмента:
- *deals* - вектор новых сделок.

<a name="signal_deals_update"></a>
####signal_deals_update()
```c++
virtual void signal_deals_update(const std::vector<Deal> &deals);
```
Вызывается при получении новых сделок сигнального инструмента:
- *deals* - вектор новых сделок.

<a name="execution_report_update"></a>
####execution_report_update()
```c++
virtual void execution_report_update(const ExecutionReportSnapshot &snapshot);
```
Вызывается при получении отчета о сделке с участием вашего ордера:
- *snapshot* – структура-отчет о совершенной сделке.

<a name="event_end_update"></a>
####event_end_update()
```c++
virtual void event_end_update();
```
Вызывается, когда симуляция закончила обрабатывать все изменения, соответствующие одному биржевому событию.

<a name="add_order"></a>
####add_order()
```c++
bool add_order(Price price, Amount amount, Dir dir, Amount implied_amount = 0);
```
Выставляет нашу заявку:
- *price* - цена, по которой заявка будет выставлена,
- *amount* - размер заявки,
- *dir* - направление (BID = 0 - покупка, ASK = 1 - продажа),
- *implied_amount* - ожидаемое реализованное количество (по умолчанию 0).

<a name="add_ioc_order"></a>
####add_ioc_order()
```c++
bool add_ioc_order(Price price, Amount amount, Dir dir);
```
Выставляет нашу заявку по принципу Immediate-Or-Cancel:
- *price* - цена, по которой заявка будет выставлена,
- *amount* - размер заявки,
- *dir* - направление (BID = 0 - покупка, ASK = 1 - продажа), ожидаемое реализованное количество совпадает с объемом заявки.

<a name="add_ioc_order"></a>
####add_ioc_order()
```c++
bool add_ioc_order(Price price, Amount amount, Dir dir, Amount implied_amount);
```
Выставляет нашу заявку по принципу Immediate-Or-Cancel:
- *price* - цена, по которой заявка будет выставлена,
- *amount* - размер заявки,
- *dir* - направление (BID = 0 - покупка, ASK = 1 - продажа),
- *implied_amount* – ожидаемое реализованное количество.

<a name="delete_order"></a>
####delete_order()
```c++
void delete_order(Order* order);
```
Снимает наш ордер с торгов:
- *order* - ордер, который мы хотим снять.

<a name="active_orders_by_dir"></a>
####active_orders_by_dir()
```c++
const std::array<std::vector<OrderSnapshot>, 2>& active_orders_by_dir();
```
Возвращает массив списков наших активных заявок, то есть заявок со статусом OrderStatus::Adding и OrderStatus::Active для бида и аска соответственно.

<a name="total_amount"></a>
####total_amount()
```c++
Amount total_amount();
```
Возвращает нашу текущую позу.

<a name="executed_amount"></a>
####executed_amount()
```c++
Amount executed_amount();
```
Возвращает нашу текущую позу без учета implied-заявок.

<a name="total_active_amount"></a>
####total_active_amount()
```c++
Amount total_active_amount(Dir dir);
```
Возвращает текущий суммарный объем выставленных ордеров по направлению:
- *dir* - направление ордеров.

<a name="count_added_orders"></a>
####count_added_orders()
```c++
Amount count_added_orders(Dir dir);
```
Возвращает количество активных ордеров по направлению:
- *dir* - направление ордеров.

<a name="best_price"></a>
####best_price()
```c++
Price best_price(Dir dir, bool is_book_trade = true);
```
Возвращает лучшую цену по направлению:
- *dir* - направление,
- *is_book_trade* = true - торговый стакан (по умолчанию),
- *is_book_trade* = false - сигнальный стакан.

<a name="get_saldo"></a>
####get_saldo()
```c++
Price get_saldo();
```
Возвращает текущее сальдо (текущий баланс без учета позы).

<a name="get_current_result"></a>
####get_current_result()
```c++
Price get_current_result();
```
Возвращает текущий результат (заработок).

<a name="signal_security_exists"></a>
####signal_security_exists()
```c++
bool signal_security_exists() const;
```
Есть ли сигнальный инструмент.

<a name="get_local_time"></a>
####get_local_time()
```c++
Microseconds get_local_time();
```
Возвращает локальное время в микросекундах. Локальное время здесь – это время на машине, получающей биржевые данные.

<a name="get_server_time"></a>
####get_server_time()
```c++
Microseconds get_server_time();
```
Возвращает биржевое время с точностью до микросекунд.

<a name="get_server_time_tm"></a>
####get_server_time_tm()
```c++
tm get_server_time_tm();
```
Возвращает биржевое время типа tm c точностью до секунды.


