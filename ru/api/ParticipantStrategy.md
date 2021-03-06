# ParticipantStrategy

Путь в Local Pack `include/participant_strategy.h`

Класс-обёртка для стратегий участников для взаимодействия с торговым симулятором.

### Методы

| Имя | Описание |
| --- | --- |
| [trading_book_update()](#trading_book_update) | Вызывается симулятором при получении нового стакана торгового инструмента. |
| [trading_deals_update()](#trading_deals_update) | Вызывается симулятором при получении новых сделок по торговому инструменту. |
| [execution_report_update()](#execution_report_update) | Вызывается симулятором при получении отчёта о проведении сделки с участием вашей заявки. |
| [add_limit_order()](#add_limit_order) | Выставляет лимитную заявку. |
| [add_ioc_order()](#add_ioc_order) | Выставляет заявку типа IOC (Immediate-Or-Cancel). |
| [delete_order()](#delete_order) | Отправляет запрос на удаление вашей заявки. |
| [delete_all_orders_at_dir()](#delete_all_orders_at_dir) | Отправляет запрос на удаление всех ваших заявок по данному направлению. |
| [delete_all_orders_at_price()](#delete_all_orders_at_price) | Отправляет запрос на удаление всех ваших заявок с заданной ценой и направлением. |
| [amount_before_order()](#amount_before_order) | Суммарный объём заявок в очереди перед вашей заявкой. |
| [volume_by_price()](#volume_by_price) | Суммарный объём активных заявок с заданной ценой и направлением. |
| [add_chart_point()](#add_chart_point) | Добавляет точку на график. |
| [current_result()](#current_result) | Текущий заработок стратегии, учитывающий как исполненные, так и просто выставленные заявки. |
| [server_time()](#server_time) | Текущее биржевое время. |
| [server_time_tm()](#server_time_tm) | Биржевое время типа tm. |
| [set_max_total_amount()](#set_max_total_amount) | Устанавливает желаемое ограничение максимальной позиции. |
| [set_stop_loss_result()](#set_stop_loss_result) | Устанавливает желаемое ограничение минимального результата. |
| [executed_amount()](#executed_amount) | Ваша текущая позиция, учитывающая только исполненные заявки. |
| [fix_moment_in_viewer()](#fix_moment_in_viewer) | Сохраняет ссылку на Viewer в веб системе после того как данные записываются во Viewer. |
| [is_our()](#is_our) | Является ли заявка вашей. |
| [is_our()](#is_our) | Является ли сделка вашей. |
| [trading_book()](#trading_book) | Текущий стакан торгового инструмента. |

### Описание методов

#### trading_book_update() {#trading_book_update}

Вызывается симулятором при получении нового стакана торгового инструмента.

{% codetabs name="C++", type="c++" -%}
virtual void trading_book_update(const OrderBook& order_book);
{%- endcodetabs %}

#### trading_deals_update() {#trading_deals_update}

Вызывается симулятором при получении новых сделок по торговому инструменту.

{% codetabs name="C++", type="c++" -%}
virtual void trading_deals_update(std::vector<Deal>&& deals);
{%- endcodetabs %}

#### execution_report_update() {#execution_report_update}

Вызывается симулятором при получении отчёта о проведении сделки с участием вашей заявки.

{% codetabs name="C++", type="c++" -%}
virtual void execution_report_update(const ExecutionReport& execution_report);
{%- endcodetabs %}

#### add_limit_order() {#add_limit_order}

Принимает направление dir, цену price и размер заявки amount.

Выставляет лимитную заявку.

Возвращает значение типа bool — была ли ваша заявка принята торговым симулятором.
Внимание: заявка может быть не принята при нарушении некоторых ограничений.
Подробнее читайте в документации: <https://docs.hftbattle.com/ru/HFAQ.html#simulator>

{% codetabs name="C++", type="c++" -%}
bool add_limit_order(Dir dir, Price price, Amount amount) const;
{%- language name="Python", type="py" -%}
def add_limit_order(self, dir, price, amount)
{%- endcodetabs %}

#### add_ioc_order() {#add_ioc_order}

Принимает направление dir, цену price и размер заявки amount.

Выставляет заявку типа IOC (Immediate-Or-Cancel).

Возвращает значение типа bool — была ли ваша заявка принята торговым симулятором.
Внимание: заявка может быть не принята при нарушении некоторых ограничений.
Подробнее читайте в документации: <https://docs.hftbattle.com/ru/HFAQ.html#simulator>

{% codetabs name="C++", type="c++" -%}
bool add_ioc_order(Dir dir, Price price, Amount amount) const;
{%- language name="Python", type="py" -%}
def add_ioc_order(self, dir, price, amount)
{%- endcodetabs %}

#### delete_order() {#delete_order}

Принимает указатель на вашу заявку, т.е. указатель на объект класса Order.

Отправляет запрос на удаление вашей заявки.
Внимание: удаление происходит не моментально.
Подробнее об ограничениях симулятора читайте здесь: <https://docs.hftbattle.com/ru/simulator/restrictions.html>.

{% codetabs name="C++", type="c++" -%}
void delete_order(Order* order) const;
{%- language name="Python", type="py" -%}
def delete_order(self, order)
{%- endcodetabs %}

#### delete_all_orders_at_dir() {#delete_all_orders_at_dir}

Принимает направление dir.

Отправляет запрос на удаление всех ваших заявок по направлению dir.

{% codetabs name="C++", type="c++" -%}
void delete_all_orders_at_dir(Dir dir) const;
{%- language name="Python", type="py" -%}
def delete_all_orders_at_dir(self, dir)
{%- endcodetabs %}

#### delete_all_orders_at_price() {#delete_all_orders_at_price}

Принимает направление dir и цену price.

Отправляет запрос на удаление всех ваших заявок по направлению dir с заданной ценой.

{% codetabs name="C++", type="c++" -%}
void delete_all_orders_at_price(Dir dir, Price price) const;
{%- language name="Python", type="py" -%}
def delete_all_orders_at_price(self, dir, price)
{%- endcodetabs %}

#### amount_before_order() {#amount_before_order}

Принимает указатель на вашу заявку, т.е. указатель на объект класса Order.

Возвращает суммарное количество лотов, стоящих в очереди перед вашей заявкой на данном ценовом уровне.

{% codetabs name="C++", type="c++" -%}
Amount amount_before_order(const Order* order) const;
{%- language name="Python", type="py" -%}
def amount_before_order(self, order)
{%- endcodetabs %}

#### volume_by_price() {#volume_by_price}

Принимает направление dir и цену price.

Возвращает суммарное количество лотов в ваших активных заявках, стоящих на определённой цене.

{% codetabs name="C++", type="c++" -%}
Amount volume_by_price(Dir dir, Price price) const;
{%- language name="Python", type="py" -%}
def volume_by_price(self, dir, price)
{%- endcodetabs %}

#### add_chart_point() {#add_chart_point}

Принимает строку line_name (название графика), value типа double или Decimal — значение, которое хочется добавить, y_axis_type — сторона, с которой будет нарисована ось Y, и chart_number — номер графика.

Добавляет точку на график с желаемым значением value в текущий момент торговой сессии.
Соседние точки на графике соединяются отрезком.
В итоге получается ломаная, которая и является графиком.
Используя параметр chart_number, можно создавать несколько графиков, а с помощью line_name можно строить сразу несколько линий на одном графике.

{% codetabs name="C++", type="c++" -%}
void add_chart_point(const std::string& line_name, Decimal value, ChartYAxisType y_axis_type = ChartYAxisType::Left, uint8_t chart_number = 1) const;
{%- language name="Python", type="py" -%}
def add_chart_point(self, line_name, value, 0, 1)
{%- endcodetabs %}

Приведём пример использования данного метода.
Мы добавим новый график лучшей цены покупки.

{% codetabs name="C++", type="c++" -%}
add_chart_point("best_bid", order_book.best_price(BID));
{%- language name="Python", type="py" -%}
strat.add_chart_point("best_bid", order_book.best_price(BID))
{%- endcodetabs %}

#### current_result() {#current_result}

Возвращает текущий результат стратегии (заработок).
Учитываются как исполненные, так и просто выставленные заявки.
При подсчёте результата мы предполагаем, что выставленные заявки сводятся по противоположной лучшей цене.

{% codetabs name="C++", type="c++" -%}
Decimal current_result() const;
{%- language name="Python", type="py" -%}
def current_result(self)
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

Принимает желаемое ограничение максимальной позиции — неотрицательное число не более 100.

Устанавливает данное значение позиции максимальным и не разрешает стратегии превышать его по модулю.

{% codetabs name="C++", type="c++" -%}
void set_max_total_amount(const Amount max_total_amount);
{%- language name="Python", type="py" -%}
def set_max_total_amount(self, max_total_amount)
{%- endcodetabs %}

#### set_stop_loss_result() {#set_stop_loss_result}

Принимает неположительное число — желаемое ограничение минимального результата.

Устанавливает это значение, при достижении которого симулятор закрывает позицию и останавливает стратегию.
Внимание: закрытие позиции происходит не моментально, поэтому вы можете получить результат как меньше, так и больше ожидаемого.
Подробнее читайте здесь: <https://docs.hftbattle.com/ru/HFAQ.html#simulator>

{% codetabs name="C++", type="c++" -%}
void set_stop_loss_result(const Decimal stop_loss_result);
{%- language name="Python", type="py" -%}
def set_stop_loss_result(self, stop_loss_result)
{%- endcodetabs %}

#### executed_amount() {#executed_amount}

Возвращает вашу текущую позицию.
Учитываются только исполненные заявки.

{% codetabs name="C++", type="c++" -%}
Amount executed_amount() const;
{%- language name="Python", type="py" -%}
def executed_amount(self)
{%- endcodetabs %}

#### fix_moment_in_viewer() {#fix_moment_in_viewer}

Принимает любую строку — название момента Viwer.

Сохраняет ссылку на Viewer в веб системе после того как данные записываются во Viewer.

{% codetabs name="C++", type="c++" -%}
void fix_moment_in_viewer(const std::string& name);
{%- language name="Python", type="py" -%}
def fix_moment_in_viewer(self, name)
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

Принимает ссылку на объект класса Deal.

Возвращает значение типа bool — участвует ли в сделке ваша заявка.

{% codetabs name="C++", type="c++" -%}
bool is_our(const Deal& deal) const;
{%- language name="Python", type="py" -%}
def is_our(self, deal)
{%- endcodetabs %}

#### trading_book() {#trading_book}

Текущий стакан торгового инструмента.
Внимание: стакан торгового инструмента нельзя получить до первого вызова `trading_book_update`.

{% codetabs name="C++", type="c++" -%}
const OrderBook& trading_book() const;
{%- language name="Python", type="py" -%}
def trading_book(self)
{%- endcodetabs %}
