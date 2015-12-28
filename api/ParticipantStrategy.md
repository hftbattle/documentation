#ParticipantStrategy

`include/simulator/strategy/participant_strategy_layer.h`


Стратегии наследуются от класса ParticipantStrategy, который служит прослойкой между симуляционным ядром и стратегией. Он обеспечивает обработку входящих сигналов от симуляции (апдейтов стаканов, сделок, сообщений о концах биржевых событий) и передачу в симуляцию сообщений о желаемых действий стратегии (постановка, снятие и перемещение заявок). Помимо реализации методов для описанных выше действий, класс также предоставляет некоторые вспомогательные методы для удобства работы.

В конструктор стратегии передается конфиг, в котором могут быть параметры, необходимые для работы стратегии, и которые могут перебираться в системе стратегии для подбора оптимального набора. Подробнее по параметры и их перебор можно почитать тут.


##Основные поля класса


|Имя| Описание|
|------------------|--------------------|
|[book_trade](#book_trade)|Стакан инструмента, на котором торгуем.|
|[book_feed](#book_feed)|Стакан инструмента, на который только смотрим.|
|[trade_book_info](#trade_book_info)|Стуктура-аггрегатор основной информации о стакане, на котором торгуем.|
|[feed_book_info](#feed_book_info)|Стуктура-аггрегатор основной информации о стакане, на который смотрим.|

##Методы, реализующие доставку апдейтов от симуляции к стратегии


|Имя| Описание|
|------------------|--------------------|
|[book_trade_update(const OrderBook& order_book)](#book_trade_update)|Функция, которую должен реализовать участник в классе UserStrategy, которая вызывается при получении нового стакана инструмента, на котором мы торгуем: order_book - новый стакан.|
|[book_feed_update(const OrderBook& order_book)](#book_feed_update)|Функция, которую должен реализовать участник в классе UserStrategy, которая вызывается при получении нового стакана инструмента, на который мы смотрим: order_book - новый стакан.|
|[trades_trade_update(const std::vector<Trade>& trades)](#trades_trade_update)|Функция, которую должен реализовать участник в классе UserStrategy, которая вызывается при получении новой порции сделок инструмента, на котором мы торгуем. trades - вектор новых сделок.|
|[trades_feed_update(const std::vector<Trade>& trades)](#trades_feed_update)|Функция, которую должен реализовать участник в классе UserStrategy, которая вызывается при получении новой порции сделок инструмента, на который мы смотрим. trades - вектор новых сделок.|
|[process_event_end()](#process_event_end)|Функция, которую должен реализовать участник в классе UserStrategy. Вызывается, когда симуляция закончила обрабатывать все изменения, соответствующие одному биржевому событию.|
|[execution_report_update(const ExecutionReportSnapshot* snapshot)](#execution_report_update)|Функция, которую должен реализовать участник в классе UserStrategy, которая вызывается при получении отчета о сделке с участием вашего ордера.|

##Методы для постановки, снятия и перемещения заявок


|Имя| Описание|
|------------------|--------------------|
|[add_ioc_order(Price price, Amount amount, Dir dir)](#add_ioc_order)|Функция, выставляющая нашу заявку по принципу immediate-or-close:|
|[add_ioc_order(Price price, Amount amount, Dir dir, Amount implied_amount)](#add_ioc_order)|Функция, выставляющая нашу заявку по принципу immediate-or-close:|
|[delete_order(Order* order)](#delete_order)|Функция, снимающая наш ордер с торгов:|

##Вспомогательные методы


|Имя| Описание|
|------------------|--------------------|
|[total_amount()](#total_amount)|Функция, возвращающая нашу текущую позу.|
|[executed_amount()](#executed_amount)|Функция, возвращающая нашу текущую позу без учета implied - заявок.|
|[total_active_amount(Dir dir)](#total_active_amount)|Функция для подсчета текущего суммарного размера выставленных ордеров по фиксированному направлению: dir - направление ордеров.|
|[count_active_orders(Dir dir)](#count_active_orders)|Функция, возвращающая количество активных ордеров по фиксированному направлению: dir - направление ордеров.|
|[best_price(Dir dir, bool is_book_trade = true)](#best_price)|Функция, возвращающая лучшую цену по данному направлению:|
|[get_saldo()](#get_saldo)|Функция, возвращающая текущее сальдо (текущий баланс без учета позы).|
|[get_current_result()](#get_current_result)|Функция, возвращающая текущий результат (заработок).|
|[cmp(Price a, Price b, Dir dir)](#cmp)|Функция сравнения двух цен по фиксированному направлению.|
|[get_local_time_tm()](#get_local_time_tm)|Метод, возвращающий локальное время типа tm c точностью до секунды.|

####Основные поля класса

<a id="book_trade"></a>
###book_trade
```c++
const OrderBookL2* book_trade;
```
Стакан инструмента, на котором торгуем.

<a id="book_feed"></a>
###book_feed
```c++
const OrderBookL2* book_feed;
```
Стакан инструмента, на который только смотрим.

<a id="trade_book_info"></a>
###trade_book_info
```c++
ContestBookInfo trade_book_info;
```
Стуктура-аггрегатор основной информации о стакане, на котором торгуем.

<a id="feed_book_info"></a>
###feed_book_info
```c++
ContestBookInfo feed_book_info;
```
Стуктура-аггрегатор основной информации о стакане, на который смотрим.


####Методы, реализующие доставку апдейтов от симуляции к стратегии

<a id="book_trade_update"></a>
###book_trade_update()
```c++
virtual void book_trade_update(const OrderBook& order_book);
```
Функция, которую должен реализовать участник в классе UserStrategy, которая вызывается при получении нового стакана инструмента, на котором мы торгуем: order_book - новый стакан.

<a id="book_feed_update"></a>
###book_feed_update()
```c++
virtual void book_feed_update(const OrderBook& order_book);
```
Функция, которую должен реализовать участник в классе UserStrategy, которая вызывается при получении нового стакана инструмента, на который мы смотрим: order_book - новый стакан.

<a id="trades_trade_update"></a>
###trades_trade_update()
```c++
virtual void trades_trade_update(const std::vector<Trade>& trades);
```
Функция, которую должен реализовать участник в классе UserStrategy, которая вызывается при получении новой порции сделок инструмента, на котором мы торгуем. trades - вектор новых сделок.

<a id="trades_feed_update"></a>
###trades_feed_update()
```c++
virtual void trades_feed_update(const std::vector<Trade>& trades);
```
Функция, которую должен реализовать участник в классе UserStrategy, которая вызывается при получении новой порции сделок инструмента, на который мы смотрим. trades - вектор новых сделок.

<a id="process_event_end"></a>
###process_event_end()
```c++
virtual void process_event_end();
```
Функция, которую должен реализовать участник в классе UserStrategy. Вызывается, когда симуляция закончила обрабатывать все изменения, соответствующие одному биржевому событию.

<a id="execution_report_update"></a>
###execution_report_update()
```c++
virtual void execution_report_update(const ExecutionReportSnapshot* snapshot);
```
Функция, которую должен реализовать участник в классе UserStrategy, которая вызывается при получении отчета о сделке с участием вашего ордера.


####Методы для постановки, снятия и перемещения заявок

bool add_order(Price price, Amount amount, Dir dir, Amount implied_amount = 0);
Функция, выставляющая наш заявку:

price - цена, по которой заявка будет выставлена,

amount - размер заявки,

dir - направление (BID = 0 - покупка, ASK = 1 - продажа),
implied_amount - ожидаемое реализованное количество (рекомендуется использовать значение 0).

<a id="add_ioc_order"></a>
###add_ioc_order()
```c++
bool add_ioc_order(Price price, Amount amount, Dir dir);
```
Функция, выставляющая нашу заявку по принципу immediate-or-close:
price - цена, по которой заявка будет выставлена,

amount - размер заявки,

dir - направление (BID = 0 - покупка, ASK = 1 - продажа),

ожидаемый реализованный объем совпадает с объемом заявки.

<a id="add_ioc_order"></a>
###add_ioc_order()
```c++
bool add_ioc_order(Price price, Amount amount, Dir dir, Amount implied_amount);
```
Функция, выставляющая нашу заявку по принципу immediate-or-close:

price - цена, по которой заявка будет выставлена
amount - размер заявки,

dir - направление (BID = 0 - покупка, ASK = 1 - продажа),

implied_amount – ожидаемое реализованное количество (рекомендуется использовать значение amount).

<a id="delete_order"></a>
###delete_order()
```c++
void delete_order(Order* order);
```
Функция, снимающая наш ордер с торгов:

order - ордер, который мы хотим снять.


####Вспомогательные методы

<a id="total_amount"></a>
###total_amount()
```c++
Amount total_amount();
```
Функция, возвращающая нашу текущую позу.

<a id="executed_amount"></a>
###executed_amount()
```c++
Amount executed_amount();
```
Функция, возвращающая нашу текущую позу без учета implied - заявок.

<a id="total_active_amount"></a>
###total_active_amount()
```c++
Amount total_active_amount(Dir dir);
```
Функция для подсчета текущего суммарного размера выставленных ордеров по фиксированному направлению: dir - направление ордеров.

<a id="count_active_orders"></a>
###count_active_orders()
```c++
Amount count_active_orders(Dir dir);
```
Функция, возвращающая количество активных ордеров по фиксированному направлению: dir - направление ордеров.

<a id="best_price"></a>
###best_price()
```c++
Price best_price(Dir dir, bool is_book_trade = true);
```
Функция, возвращающая лучшую цену по данному направлению:

dir - направление,

is_book_trade = true - торговый стакан, is_book_trade = false - сигнальный стакан.

<a id="get_saldo"></a>
###get_saldo()
```c++
Price get_saldo();
```
Функция, возвращающая текущее сальдо (текущий баланс без учета позы).

<a id="get_current_result"></a>
###get_current_result()
```c++
Price get_current_result();
```
Функция, возвращающая текущий результат (заработок).

<a id="cmp"></a>
###cmp()
```c++
bool cmp(Price a, Price b, Dir dir);
```
Функция сравнения двух цен по фиксированному направлению.

<a id="get_local_time_tm"></a>
###get_local_time_tm()
```c++
tm get_local_time_tm();
```
Метод, возвращающий локальное время типа tm c точностью до секунды.

