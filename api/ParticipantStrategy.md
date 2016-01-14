#ParticipantStrategy

`include/simulator/strategy/participant_strategy_layer.h`


Стратегии наследуются от класса ParticipantStrategy, который служит прослойкой между симуляционным ядром и стратегией. Он обеспечивает обработку входящих сигналов от симуляции (апдейтов стаканов, сделок, сообщений о концах биржевых событий) и передачу в симуляцию сообщений о желаемых действий стратегии (постановка, снятие и перемещение заявок). Помимо реализации методов для описанных выше действий, класс также предоставляет некоторые вспомогательные методы для удобства работы.

В конструктор стратегии передается конфиг, в котором могут быть параметры, необходимые для работы стратегии, и которые могут перебираться в системе стратегии для подбора оптимального набора. Подробнее по параметры и их перебор можно почитать тут.


###Поля


|Имя| Описание|
|------------------|--------------------|
|[trade_book_info](#trade_book_info)|Стуктура-аггрегатор основной информации о торговом стакане.|
|[signal_book_info](#signal_book_info)|Стуктура-аггрегатор основной информации о сигнальном стакане.|
|[trade_book](#trade_book)|Указатель на стакан торгового инструмента.|
|[trade_book_snapshot](#trade_book_snapshot)|Указатель на структуру данных, в которой содержится текущий стакан торгового инструмента.|
|[signal_book](#signal_book)|Указатель на стакан сигнального инструмента.|
|[signal_book_snapshot](#signal_book_snapshot)|Указатель на структуру данных, в которой содержится текущий стакан сигнального инструмента.|

###Методы


|Имя| Описание|
|------------------|--------------------|
|[trade_book_update(const OrderBook& order_book)](#trade_book_update)|Функция, которую должен реализовать участник в классе UserStrategy, которая вызывается при получении нового стакана торгового инструмента:|
|[signal_book_update(const OrderBook& order_book)](#signal_book_update)|Функция, которую должен реализовать участник в классе UserStrategy, которая вызывается при получении нового стакана сигнального инструмента, на который мы смотрим: order_book - новый стакан.|
|[trades_trade_update(const std::vector<Trade>& trades)](#trades_trade_update)|Функция, которую должен реализовать участник в классе UserStrategy, которая вызывается при получении новой порции сделок инструмента, на котором мы торгуем. trades - вектор новых сделок.|
|[trades_feed_update(const std::vector<Trade>& trades)](#trades_feed_update)|Функция, которую должен реализовать участник в классе UserStrategy, которая вызывается при получении новой порции сделок инструмента, на который мы смотрим. trades - вектор новых сделок.|
|[process_event_end()](#process_event_end)|Функция, которую должен реализовать участник в классе UserStrategy. Вызывается, когда симуляция закончила обрабатывать все изменения, соответствующие одному биржевому событию.|
|[execution_report_update(const ExecutionReportSnapshot* snapshot)](#execution_report_update)|Функция, которую должен реализовать участник в классе UserStrategy, которая вызывается при получении отчета о сделке с участием вашего ордера.|
|[add_ioc_order(Price price, Amount amount, Dir dir)](#add_ioc_order)|Функция, выставляющая нашу заявку по принципу immediate-or-close.|
|[add_ioc_order(Price price, Amount amount, Dir dir, Amount implied_amount)](#add_ioc_order)|Функция, выставляющая нашу заявку по принципу immediate-or-close.|
|[delete_order(Order* order)](#delete_order)|Функция, снимающая наш ордер с торгов.|
|[total_amount()](#total_amount)|Функция, возвращающая нашу текущую позу.|
|[executed_amount()](#executed_amount)|Функция, возвращающая нашу текущую позу без учета implied - заявок.|
|[total_active_amount(Dir dir)](#total_active_amount)|Функция для подсчета текущего суммарного размера выставленных ордеров по фиксированному направлению.|
|[count_active_orders(Dir dir)](#count_active_orders)|Функция, возвращающая количество активных ордеров по фиксированному направлению.|
|[best_price(Dir dir, bool is_book_trade = true)](#best_price)|Функция, возвращающая лучшую цену по данному направлению.|
|[get_saldo()](#get_saldo)|Функция, возвращающая текущее сальдо (текущий баланс без учета позы).|
|[get_current_result()](#get_current_result)|Функция, возвращающая текущий результат (заработок).|
|[cmp(Price a, Price b, Dir dir)](#cmp)|Функция сравнения двух цен по фиксированному направлению.|
|[get_local_time_as_microseconds()](#get_local_time_as_microseconds)|Функция, возвращающая локальное время в микросекундах.|
|[get_server_time_as_microseconds()](#get_server_time_as_microseconds)|Функция, возвращающая биржевое время в микросекундах.|
|[get_server_time_as_tm()](#get_server_time_as_tm)|Функция, возвращающая биржевое время типа tm c точностью до секунды.|

###Описание полей

<a id="trade_book_info"></a>
####trade_book_info
```c++
ContestBookInfo trade_book_info;
```
Стуктура-аггрегатор основной информации о торговом стакане.

<a id="signal_book_info"></a>
####signal_book_info
```c++
ContestBookInfo signal_book_info;
```
Стуктура-аггрегатор основной информации о сигнальном стакане.

<a id="trade_book"></a>
####trade_book
```c++
const OrderBookL2* trade_book;
```
Указатель на стакан торгового инструмента.

<a id="trade_book_snapshot"></a>
####trade_book_snapshot
```c++
SharedPtr<DataFeedSnapshot> trade_book_snapshot;
```
Указатель на структуру данных, в которой содержится текущий стакан торгового инструмента. 
Будьте внимательны, с приходом очередного апдейта этого стакана указатель тоже обновляется, и объект внутри (стакан) разрушается.
Чтобы сохранить старый стакан, нужно явно в стратегии сохранить этот указатель.

<a id="signal_book"></a>
####signal_book
```c++
const OrderBookL2* signal_book;
```
Указатель на стакан сигнального инструмента.

<a id="signal_book_snapshot"></a>
####signal_book_snapshot
```c++
SharedPtr<DataFeedSnapshot> signal_book_snapshot;
```
Указатель на структуру данных, в которой содержится текущий стакан сигнального инструмента. 
Будьте внимательны, с приходом очередного апдейта этого стакана указатель тоже обновляется, и объект внутри (стакан) разрушается.
Чтобы сохранить старый стакан, нужно явно в стратегии сохранить этот указатель.



###Описание методов
<a id="trade_book_update"></a>
####trade_book_update()
```c++
virtual void trade_book_update(const OrderBook& order_book);
```
Функция, которую должен реализовать участник в классе UserStrategy, которая вызывается при получении нового стакана торгового инструмента: 

order_book - новый стакан.

<a id="signal_book_update"></a>
####signal_book_update()
```c++
virtual void signal_book_update(const OrderBook& order_book);
```
Функция, которую должен реализовать участник в классе UserStrategy, которая вызывается при получении нового стакана сигнального инструмента, на который мы смотрим: order_book - новый стакан.

<a id="trades_trade_update"></a>
####trades_trade_update()
```c++
virtual void trades_trade_update(const std::vector<Trade>& trades);
```
Функция, которую должен реализовать участник в классе UserStrategy, которая вызывается при получении новой порции сделок инструмента, на котором мы торгуем. trades - вектор новых сделок.

<a id="trades_feed_update"></a>
####trades_feed_update()
```c++
virtual void trades_feed_update(const std::vector<Trade>& trades);
```
Функция, которую должен реализовать участник в классе UserStrategy, которая вызывается при получении новой порции сделок инструмента, на который мы смотрим. trades - вектор новых сделок.

<a id="process_event_end"></a>
####process_event_end()
```c++
virtual void process_event_end();
```
Функция, которую должен реализовать участник в классе UserStrategy. Вызывается, когда симуляция закончила обрабатывать все изменения, соответствующие одному биржевому событию.

<a id="execution_report_update"></a>
####execution_report_update()
```c++
virtual void execution_report_update(const ExecutionReportSnapshot* snapshot);
```
Функция, которую должен реализовать участник в классе UserStrategy, которая вызывается при получении отчета о сделке с участием вашего ордера.

bool add_order(Price price, Amount amount, Dir dir, Amount implied_amount = 0);
Функция, выставляющая наш заявку.

price - цена, по которой заявка будет выставлена,

amount - размер заявки,

dir - направление (BID = 0 - покупка, ASK = 1 - продажа),
implied_amount - ожидаемое реализованное количество (рекомендуется использовать значение 0).

<a id="add_ioc_order"></a>
####add_ioc_order()
```c++
bool add_ioc_order(Price price, Amount amount, Dir dir);
```
Функция, выставляющая нашу заявку по принципу immediate-or-close.

price - цена, по которой заявка будет выставлена,

amount - размер заявки,

dir - направление (BID = 0 - покупка, ASK = 1 - продажа),

ожидаемый реализованный объем совпадает с объемом заявки.

<a id="add_ioc_order"></a>
####add_ioc_order()
```c++
bool add_ioc_order(Price price, Amount amount, Dir dir, Amount implied_amount);
```
Функция, выставляющая нашу заявку по принципу immediate-or-close.

price – цена, по которой заявка будет выставлена,

amount – размер заявки,

dir – направление (BID = 0 - покупка, ASK = 1 - продажа),

implied_amount – ожидаемое реализованное количество (рекомендуется использовать значение amount).

<a id="delete_order"></a>
####delete_order()
```c++
void delete_order(Order* order);
```
Функция, снимающая наш ордер с торгов.

order – ордер, который мы хотим снять.

<a id="total_amount"></a>
####total_amount()
```c++
Amount total_amount();
```
Функция, возвращающая нашу текущую позу.

<a id="executed_amount"></a>
####executed_amount()
```c++
Amount executed_amount();
```
Функция, возвращающая нашу текущую позу без учета implied - заявок.

<a id="total_active_amount"></a>
####total_active_amount()
```c++
Amount total_active_amount(Dir dir);
```
Функция для подсчета текущего суммарного размера выставленных ордеров по фиксированному направлению.

dir – направление ордеров.

<a id="count_active_orders"></a>
####count_active_orders()
```c++
Amount count_active_orders(Dir dir);
```
Функция, возвращающая количество активных ордеров по фиксированному направлению. 

dir – направление ордеров.

<a id="best_price"></a>
####best_price()
```c++
Price best_price(Dir dir, bool is_book_trade = true);
```
Функция, возвращающая лучшую цену по данному направлению.

dir – направление,

is_book_trade = true - торговый стакан, is_book_trade = false - сигнальный стакан.

<a id="get_saldo"></a>
####get_saldo()
```c++
Price get_saldo();
```
Функция, возвращающая текущее сальдо (текущий баланс без учета позы).

<a id="get_current_result"></a>
####get_current_result()
```c++
Price get_current_result();
```
Функция, возвращающая текущий результат (заработок).

<a id="cmp"></a>
####cmp()
```c++
bool cmp(Price a, Price b, Dir dir);
```
Функция сравнения двух цен по фиксированному направлению.

<a id="get_local_time_as_microseconds"></a>
####get_local_time_as_microseconds()
```c++
Microseconds get_local_time_as_microseconds();
```
Функция, возвращающая локальное время в микросекундах.
Локальное время здесь – это время на машине, получающей биржевые данные.

<a id="get_server_time_as_microseconds"></a>
####get_server_time_as_microseconds()
```c++
Microseconds get_server_time_as_microseconds();
```
Функция, возвращающая биржевое время в микросекундах.

<a id="get_server_time_as_tm"></a>
####get_server_time_as_tm()
```c++
tm get_server_time_as_tm();
```
Функция, возвращающая биржевое время типа tm c точностью до секунды.


