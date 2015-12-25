#ParticipantStrategy
`include/simulator/strategy/participant_strategy_layer.h`


Стратегии наследуются от класса ParticipantStrategy, который служит прослойкой между симуляционным ядром и стратегией. Он обеспечивает обработку входящих сигналов от симуляции (апдейтов стаканов, сделок, сообщений о концах биржевых событий) и передачу в симуляцию сообщений о желаемых действий стратегии (постановка, снятие и перемещение заявок). Помимо реализации методов для описанных выше действий, класс также предоставляет некоторые вспомогательные методы для удобства работы.

В конструктор стратегии передается конфиг, в котором могут быть параметры, необходимые для работы стратегии, и которые могут перебираться в системе стратегии для подбора оптимального набора. Подробнее по параметры и их перебор можно почитать тут.


|Имя| Описание|
|------------------|--------------------|
|[ParticipantStrategy.book_trade](#book_trade)|Стакан инструмента, на котором торгуем.|
|[ParticipantStrategy.book_feed](#book_feed)|Стакан инструмента, на который только смотрим.|
|[ParticipantStrategy.trade_book_info](#trade_book_info)|Стуктура-аггрегатор основной информации о стакане, на котором торгуем.|
|[ParticipantStrategy.feed_book_info](#feed_book_info)|Стуктура-аггрегатор основной информации о стакане, на который смотрим.|
|[ParticipantStrategy.book_trade_update(const OrderBook& order_book)()](#book_trade_update)|Функция, которую должен реализовать участник в классе UserStrategy, которая вызывается при получении нового стакана инструмента, на котором мы торгуем: order_book - новый стакан|
|[ParticipantStrategy.book_feed_update(const OrderBook& order_book)()](#book_feed_update)|Функция, которую должен реализовать участник в классе UserStrategy, которая вызывается при получении нового стакана инструмента, на который мы смотрим: order_book - новый стакан|
|[ParticipantStrategy.trades_trade_update(const std::vector<Trade>& trades)()](#trades_trade_update)|Функция, которую должен реализовать участник в классе UserStrategy, которая вызывается при получении новой порции сделок инструмента, на котором мы торгуем. trades - вектор новых сделок|
|[ParticipantStrategy.trades_feed_update(const std::vector<Trade>& trades)()](#trades_feed_update)|Функция, которую должен реализовать участник в классе UserStrategy, которая вызывается при получении новой порции сделок инструмента, на который мы смотрим. trades - вектор новых сделок|
|[ParticipantStrategy.process_event_end()()](#process_event_end)|Функция, которую должен реализовать участник в классе UserStrategy. Вызывается, когда симуляция закончила обрабатывать все изменения, соответствующие одному биржевому событию.|
|[ParticipantStrategy.execution_report_update(const ExecutionReportSnapshot* snapshot)()](#execution_report_update)|Функция, которую должен реализовать участник в классе UserStrategy, которая вызывается при получении отчета о сделке с участием вашего ордера|
|[ParticipantStrategy.add_ioc_order(Price price, Amount amount, Dir dir)()](#add_ioc_order)|Функция, выставляющая нашу заявку по принципу immediate-or-close:|
|[ParticipantStrategy.add_ioc_order(Price price, Amount amount, Dir dir, Amount implied_amount)()](#add_ioc_order)|Функция, выставляющая нашу заявку по принципу immediate-or-close:|
|[ParticipantStrategy.delete_order(Order* order)()](#delete_order)|Функция, снимающая наш ордер с торгов:|
|[ParticipantStrategy.total_amount()()](#total_amount)|Функция, возвращающая нашу текущую позу.|
|[ParticipantStrategy.executed_amount()()](#executed_amount)|Функция, возвращающая нашу текущую позу без учета implied - заявок.|
|[ParticipantStrategy.total_active_amount(Dir dir)()](#total_active_amount)|Функция для подсчета текущего суммарного размера выставленных ордеров по фиксированному направлению: dir - направление ордеров|
|[ParticipantStrategy.count_active_orders(Dir dir)()](#count_active_orders)|Функция, возвращающая количество активных ордеров по фиксированному направлению: dir - направление ордеров|
|[ParticipantStrategy.(Dir dir, bool is_book_trade = true)()](#)|Функция, возвращающая лучшую цену по данному направлению:|
|[ParticipantStrategy.get_saldo()()](#get_saldo)|Функция, возвращающая текущее сальдо (текущий баланс без учета позы)|
|[ParticipantStrategy.get_current_result()()](#get_current_result)|Функция, возвращающая текущий результат (заработок)|
|[ParticipantStrategy.cmp(Price a, Price b, Dir dir)()](#cmp)|Функция сравнения двух цен по фиксированному направлению|
|[ParticipantStrategy.get_local_time_tm()()](#get_local_time_tm)|Метод, возвращающий локальное время типа tm c точностью до секунды.|

#Основные неприватные поля класса:

```cpp
const OrderBookL2* book_trade;
```Стакан инструмента, на котором торгуем.

```cpp
const OrderBookL2* book_feed;
```Стакан инструмента, на который только смотрим.

```cpp
ContestBookInfo trade_book_info;
```Стуктура-аггрегатор основной информации о стакане, на котором торгуем.

```cpp
ContestBookInfo feed_book_info;
```Стуктура-аггрегатор основной информации о стакане, на который смотрим.

#Методы, реализующие доставку апдейтов от симуляции к стратегии:

```cpp
virtual void book_trade_update(const OrderBook& order_book);
```Функция, которую должен реализовать участник в классе UserStrategy, которая вызывается при получении нового стакана инструмента, на котором мы торгуем: order_book - новый стакан

```cpp
virtual void book_feed_update(const OrderBook& order_book);
```Функция, которую должен реализовать участник в классе UserStrategy, которая вызывается при получении нового стакана инструмента, на который мы смотрим: order_book - новый стакан

```cpp
virtual void trades_trade_update(const std::vector<Trade>& trades);
```Функция, которую должен реализовать участник в классе UserStrategy, которая вызывается при получении новой порции сделок инструмента, на котором мы торгуем. trades - вектор новых сделок

```cpp
virtual void trades_feed_update(const std::vector<Trade>& trades);
```Функция, которую должен реализовать участник в классе UserStrategy, которая вызывается при получении новой порции сделок инструмента, на который мы смотрим. trades - вектор новых сделок

```cpp
virtual void process_event_end();
```Функция, которую должен реализовать участник в классе UserStrategy. Вызывается, когда симуляция закончила обрабатывать все изменения, соответствующие одному биржевому событию.

```cpp
virtual void execution_report_update(const ExecutionReportSnapshot* snapshot);
```Функция, которую должен реализовать участник в классе UserStrategy, которая вызывается при получении отчета о сделке с участием вашего ордера

#Методы для постановки, снятия и перемещения заявок:

bool add_order(Price price, Amount amount, Dir dir, Amount implied_amount = 0);
Функция, выставляющая наш заявку:
price - цена, по которой заявка будет выставлена
amount - размер заявки
dir - направление (BID = 0 - покупка, ASK = 1 - продажа)
implied_amount - ожидаемое реализованное количество (рекомендуется использовать значение 0)

```cpp
bool add_ioc_order(Price price, Amount amount, Dir dir);
```Функция, выставляющая нашу заявку по принципу immediate-or-close:
price - цена, по которой заявка будет выставлена
amount - размер заявки
dir - направление (BID = 0 - покупка, ASK = 1 - продажа)
ожидаемый реализованный объем совпадает с объемом заявки

```cpp
bool add_ioc_order(Price price, Amount amount, Dir dir, Amount implied_amount);
```Функция, выставляющая нашу заявку по принципу immediate-or-close:
price - цена, по которой заявка будет выставлена
amount - размер заявки
dir - направление (BID = 0 - покупка, ASK = 1 - продажа)
implied_amount ожидаемое реализованное количество (рекомендуется использовать значение amount)

```cpp
void delete_order(Order* order);
```Функция, снимающая наш ордер с торгов:
order - ордер, который мы хотим снять

#Вспомогательные методы:

```cpp
Amount total_amount();
```Функция, возвращающая нашу текущую позу.

```cpp
Amount executed_amount();
```Функция, возвращающая нашу текущую позу без учета implied - заявок.

```cpp
Amount total_active_amount(Dir dir);
```Функция для подсчета текущего суммарного размера выставленных ордеров по фиксированному направлению: dir - направление ордеров

```cpp
Amount count_active_orders(Dir dir);
```Функция, возвращающая количество активных ордеров по фиксированному направлению: dir - направление ордеров

```cpp
Price best_price (Dir dir, bool is_book_trade = true);
```Функция, возвращающая лучшую цену по данному направлению:
dir - направление
is_book_trade = true - торговый стакан, is_book_trade = false - сигнальный стакан

```cpp
Price get_saldo();
```Функция, возвращающая текущее сальдо (текущий баланс без учета позы)

```cpp
Price get_current_result();
```Функция, возвращающая текущий результат (заработок)

```cpp
bool cmp(Price a, Price b, Dir dir);
```Функция сравнения двух цен по фиксированному направлению

```cpp
tm get_local_time_tm();
```Метод, возвращающий локальное время типа tm c точностью до секунды.

