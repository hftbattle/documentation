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
| [set_max_total_amount()](#set_max_total_amount) | Устанавливает желаемое значение максимальной позиции. |
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

{% codetabs name="C++", type="c++" -%}
virtual void trading_book_update(const OrderBook& order_book);
{%- language name="Python", type="py" -%}
def trading_book_update(strat, trading_book)
{%- endcodetabs %}

#### trading_deals_update() {#trading_deals_update}

Вызывается симулятором при получении новых сделок торгового инструмента.

{% codetabs name="C++", type="c++" -%}
virtual void trading_deals_update(std::vector<Deal>&& deals);
{%- language name="Python", type="py" -%}
def trading_deals_update(strat, deals)
{%- endcodetabs %}

#### execution_report_update() {#execution_report_update}

Вызывается симулятором при получении отчёта о сделке с участием вашей заявки.

{% codetabs name="C++", type="c++" -%}
virtual void execution_report_update(const ExecutionReport& execution_report);
{%- language name="Python", type="py" -%}
def execution_report_update(strat, execution_report)
{%- endcodetabs %}

#### signal_book_update() {#signal_book_update}

Вызывается симулятором при получении нового стакана сигнального инструмента.

{% codetabs name="C++", type="c++" -%}
virtual void signal_book_update(const OrderBook& order_book);
{%- endcodetabs %}

#### signal_deals_update() {#signal_deals_update}

Вызывается симулятором при получении новых сделок сигнального инструмента.

{% codetabs name="C++", type="c++" -%}
virtual void signal_deals_update(std::vector<Deal>&& deals);
{%- endcodetabs %}

#### add_limit_order() {#add_limit_order}

Принимает направление dir (BID (покупка) или ASK (продажа)), цену price и размер заявки amount.

Выставляет лимитную заявку.

Возвращает значение типа bool — была ли ваша заявка принята торговой системой.
Внимание: заявка может быть не принята по нескольким причинам.
Подробнее читайте в документации: <https://docs.hftbattle.com/ru/FAQ.html#simulator>

{% codetabs name="C++", type="c++" -%}
bool add_limit_order(Dir dir, Price price, Amount amount);
{%- language name="Python", type="py" -%}
def add_limit_order(self, dir, price, amount)
{%- endcodetabs %}

#### add_ioc_order() {#add_ioc_order}

Принимает направление dir (BID (покупка) или ASK (продажа)), цену price и размер заявки amount.

Выставляет заявку типа IOC (Immediate-Or-Cancel).

Возвращает значение типа bool — была ли принята ваша заявка.
Внимание: заявка может быть не принята по нескольким причинам.
Подробнее читайте в документации: <https://docs.hftbattle.com/ru/FAQ.html#simulator>

{% codetabs name="C++", type="c++" -%}
bool add_ioc_order(Dir dir, Price price, Amount amount);
{%- language name="Python", type="py" -%}
def add_ioc_order(self, dir, price, amount)
{%- endcodetabs %}

#### delete_order() {#delete_order}

Принимает указатель на вашу заявку, т.е. указатель на объект класса Order.

Снимает эту заявку с торгов.
Внимание: удаление происходит немоментально.
Подробнее об ограничениях симулятора читайте здесь: <https://docs.hftbattle.com/ru/simulator/restrictions.html>.

{% codetabs name="C++", type="c++" -%}
void delete_order(Order* order);
{%- language name="Python", type="py" -%}
def delete_order(self, order)
{%- endcodetabs %}

#### delete_all_orders_at_dir() {#delete_all_orders_at_dir}

Принимает направление dir (BID (покупка) или ASK (продажа)).

Удаляет все ваши заявки по направлению dir.

{% codetabs name="C++", type="c++" -%}
void delete_all_orders_at_dir(Dir dir);
{%- language name="Python", type="py" -%}
def delete_all_orders_at_dir(self, dir)
{%- endcodetabs %}

#### delete_all_orders_at_price() {#delete_all_orders_at_price}

Принимает направление dir (BID (покупка) или ASK (продажа)) и цену price.

Удаляет все ваши заявки по направлению dir по данной цене.

{% codetabs name="C++", type="c++" -%}
void delete_all_orders_at_price(Dir dir, Price price);
{%- language name="Python", type="py" -%}
def delete_all_orders_at_price(self, dir, price)
{%- endcodetabs %}

#### amount_before_order() {#amount_before_order}

Принимает указатель на вашу заявку, т.е. указатель на объект класса Order.

Возвращает суммарное количество лотов, стоящих в очереди перед вашей заявкой в котировке данного ценового уровня.

{% codetabs name="C++", type="c++" -%}
Amount amount_before_order(const Order* order) const;
{%- language name="Python", type="py" -%}
def amount_before_order(self, order)
{%- endcodetabs %}

#### volume_by_price() {#volume_by_price}

Принимает направление dir (BID (покупка) или ASK (продажа)) и цену price.

Возвращает суммарное количество лотов в ваших активных заявках, стоящих на определённой цене.

{% codetabs name="C++", type="c++" -%}
Amount volume_by_price(Dir dir, Price price) const;
{%- language name="Python", type="py" -%}
def volume_by_price(self, dir, price)
{%- endcodetabs %}

#### add_chart_point() {#add_chart_point}

Принимает строку line_name (название графика), value типа double — значение, которое хочется добавить, y_axis_type — с какой стороны будет нарисована ось y и chart_number — номер графика.

Добавляет точку на график в данный момент времени с желаемым значением value.
Соседние точки на графике соединяются отрезком.
В итоге получаем ломаную, которая и является графиком.
Внимание: благодаря параметру chart_number можно создавать несколько графиков, а с помощью line_name — можно рисовать несколько графиков на одном изображении.

{% codetabs name="C++", type="c++" -%}
void add_chart_point(const std::string& line_name, Decimal value, ChartYAxisType y_axis_type = ChartYAxisType::Left, uint8_t chart_number = 1);
{%- language name="Python", type="py" -%}
def add_chart_point(self, line_name, value, 0, 1)
{%- endcodetabs %}

#### current_result() {#current_result}

Возвращает текущий результат стратегии (заработок).
Учитываются как исполненные, так и просто поставленные заявки.

{% codetabs name="C++", type="c++" -%}
Decimal current_result() const;
{%- language name="Python", type="py" -%}
def current_result(self)
{%- endcodetabs %}

#### saldo() {#saldo}

Возвращает текущий результат стратегии (заработок).
Учитываются только исполненные заявки.

{% codetabs name="C++", type="c++" -%}
Decimal saldo();
{%- language name="Python", type="py" -%}
def saldo(self)
{%- endcodetabs %}

#### signal_security_exists() {#signal_security_exists}

Возвращает значение типа bool — существует ли сигнальный инструмент.

{% codetabs name="C++", type="c++" -%}
bool signal_security_exists() const;
{%- language name="Python", type="py" -%}
def signal_security_exists(self)
{%- endcodetabs %}

#### local_time() {#local_time}

Возвращает текущее локальное время в микросекундах.

{% codetabs name="C++", type="c++" -%}
Microseconds local_time() const;
{%- language name="Python", type="py" -%}
def local_time(self)
{%- endcodetabs %}

#### server_time() {#server_time}

Возвращает текущее биржевое время в микросекундах.

{% codetabs name="C++", type="c++" -%}
Microseconds server_time() const;
{%- language name="Python", type="py" -%}
def server_time(self)
{%- endcodetabs %}

#### server_time_tm() {#server_time_tm}

Возвращает биржевое время типа tm с точностью до секунды.
Примечание: биржевое время в таком формате может быть использовано для определения времени суток.

{% codetabs name="C++", type="c++" -%}
tm server_time_tm() const;
{%- language name="Python", type="py" -%}
def server_time_tm(self)
{%- endcodetabs %}

#### set_max_total_amount() {#set_max_total_amount}

Принимает желаемое значение максимальной позиции — неотрицательное число не более 50.

Устанавливает данное значение позиции максимальным и не разрешает стратегии превышать его по модулю.

{% codetabs name="C++", type="c++" -%}
void set_max_total_amount(const Amount max_total_amount);
{%- language name="Python", type="py" -%}
def set_max_total_amount(self, max_total_amount)
{%- endcodetabs %}

#### set_stop_loss_result() {#set_stop_loss_result}

Принимает желаемое значение минимального результата.

Устанавливает это значение, при достижении которого позиция закрывается, и стратегия перестаёт торговать.
Внимание: закрытие позиции происходит немоментально, поэтому вы можете получить результат как меньше, так и больше ожидаемого.
Подробнее читайте здесь: <https://docs.hftbattle.com/ru/FAQ.html#simulator>

{% codetabs name="C++", type="c++" -%}
void set_stop_loss_result(const Decimal stop_loss_result);
{%- language name="Python", type="py" -%}
def set_stop_loss_result(self, stop_loss_result)
{%- endcodetabs %}

#### executed_amount() {#executed_amount}

Возвращает вашу текущую позицию.
Учитываются только исполненные лоты.

{% codetabs name="C++", type="c++" -%}
Amount executed_amount() const;
{%- language name="Python", type="py" -%}
def executed_amount(self)
{%- endcodetabs %}

#### total_amount() {#total_amount}

Возвращает вашу общую позицию: учитывается как исполненные, так и просто поставленные заявки.

{% codetabs name="C++", type="c++" -%}
Amount total_amount() const;
{%- language name="Python", type="py" -%}
def total_amount(self)
{%- endcodetabs %}

#### is_our() {#is_our}

Принимает указатель на заявку, т.е. указатель на объект класса Order.

Возвращает значение типа bool — является ли данная заявка вашей.

{% codetabs name="C++", type="c++" -%}
bool is_our(const Order* order) const;
{%- language name="Python", type="py" -%}
def is_our(self, order)
{%- endcodetabs %}

#### is_our() {#is_our}

Принимает ссылку на сделку, т.е. ссылку на объект класса Deal.

Возвращает значение типа bool — участвует ли в сделке ваша заявка.

{% codetabs name="C++", type="c++" -%}
bool is_our(const Deal& deal) const;
{%- language name="Python", type="py" -%}
def is_our(self, deal)
{%- endcodetabs %}

#### trading_book() {#trading_book}

Ссылка на текущий стакан торгового инструмента.

{% codetabs name="C++", type="c++" -%}
OrderBook& trading_book();
{%- language name="Python", type="py" -%}
def trading_book(self)
{%- endcodetabs %}

#### signal_book() {#signal_book}

Ссылка на текущий стакан сигнального инструмента.

{% codetabs name="C++", type="c++" -%}
OrderBook& signal_book();
{%- language name="Python", type="py" -%}
def signal_book(self)
{%- endcodetabs %}
