## Примеры стратегий
<!-- TODO(asalikhov): rewrite -->

Рассмотрим здесь несколько примеров торговых стратегий:

- [Stay on best price strategy](#stay_on_best_price)
  - [Base](#stay_on_best_price)
  - [Improved](#stay_on_best_price_improved)

<!-- - [Deals count diff strategy](#deals_count_diff)
  - [Base](#deals_count_diff_base)
  - [Limited](#deals_count_diff_limited)
- [Improved ideas strategy](#improved_ideas) -->

#### Stay on best price strategy {#stay_on_best_price}

Идея данной стратегии состоит в том, чтобы поддерживать на каждом направлении (*BID* и *ASK*) по одной нашей заявке на лучшей цене.
В случае, если на направлении нет наших активных заявок, она ставит заявку объёмом 1 на лучшую цену.
Если заявка уже есть, но она стоит не на лучшей - мы её снимаем и ставим новую заявку на лучшую цену.

##### Рассмотрим базовый вариант стратегии: {#stay_on_best_price_base}

```c++
#include "participant_strategy.h"

using namespace hftbattle;

class UserStrategy : public ParticipantStrategy {
public:
  explicit UserStrategy(const JsonValue& config) { }

  // Вызывается при получении нового стакана торгового инструмента:
  // @order_book — новый стакан.
  void trading_book_update(const OrderBook& order_book) override {
    const auto& our_orders = order_book.orders();
    for (Dir dir : {BID, ASK}) {
      Price best_price = order_book.best_price(dir);
      Amount amount = 1;
      if (our_orders.active_orders_count(dir) == 0) {
        add_limit_order(dir, best_price, amount);
      } else {  // есть хотя бы одна наша активная заявка
        auto first_order = our_orders.orders_by_dir(dir)[0];
        bool on_best_price = (first_order->price() == best_price);
        if (!on_best_price) {  // наша заявка стоит, но не на текущей лучшей цене
          delete_order(first_order);
          add_limit_order(dir, best_price, amount);
        }
      }
    }
  }
};
```

##### Модифицируем предыдущую стратегию. {#stay_on_best_price_improved}

Поскольку стратегия поддерживает только 1 заявку размером в 1 лот по каждому направлению, то свою позицию она меняет очень медленно.
Для HFT-стратегий очень важна возможность быстро вернуться к нулевой позиции, поэтому мы попробуем ограничить максимально допустимую открытую позицию.
Для этого достаточно в параметрах передать желаемое ограничение максимальной позиции, а затем воспользоваться методом [set_max_total_amount](/api/ParticipantStrategy.md#set_max_total_amount).
Согласно правилам, [позиция](/terms.md#position) не может быть превышать 100, подробнее см. [Ограничения симулятора](/simulator/restrictions.md).
Оптимальное значение можно подобрать, перебрав разные варианты в системе.
Подробнее в разделе [Перебор параметров](/interface/params.md).

Применим следующую простую оптимизацию: если на лучшей цене стоит объём меньший, чем *`min_amount_to_stay_on_best_`*, то мы не будем на неё выставляться.
Если же на этой цене уже стоит наша заявка, то мы её снимем:

```c++
#include "participant_strategy.h"

using namespace hftbattle;

class UserStrategy : public ParticipantStrategy {
public:
  explicit UserStrategy(const JsonValue& config) {
    min_volume_to_stay_on_best_ = config["min_volume_to_stay_on_best"].as<int>(10);
    set_max_total_amount(config["max_total_amount"].as<int>());
  }

  // Вызывается при получении нового стакана торгового инструмента:
  // @order_book — новый стакан.
  void trading_book_update(const OrderBook& order_book) override {
    const auto& our_orders = order_book.orders();
    for (Dir dir : {BID, ASK}) {
      Price best_price = order_book.best_price(dir);
      Amount best_volume = order_book.best_volume(dir);
      bool can_stay_on_best = (best_volume >= min_volume_to_stay_on_best_);
      if (our_orders.active_orders_count(dir) == 0) {
        add_limit_order_if(dir, best_price, 1, can_stay_on_best);
      } else {  // есть хотя бы одна наша активная заявка
        auto first_order = our_orders.orders_by_dir(dir)[0];
        bool on_best_price = (first_order->price() == best_price);
        if (!on_best_price || !can_stay_on_best) {
          delete_order(first_order);
          add_limit_order_if(dir, best_price, 1, can_stay_on_best);
        }
      }
    }
  }

  void add_limit_order_if(Dir dir, Price price, Amount amount, bool condition) {
    if (condition) {
      add_limit_order(dir, price, amount);
    }
  }

private:
    Amount min_volume_to_stay_on_best_;
};
```

<!-- #### Deals count diff strategy {#deals_count_diff}

Данная стратегия торгует на основе произошедших на бирже сделок: если абсолютная разность количества сделок на биде и аске за определенное время (*`deals_reset_period_ms_`*) больше некоторой фиксированной величины (*`min_deals_count_diff_`*), то стратегия выставляет заявку типа [IOC (Immediate-Or-Cancel)](/terms.md#ioc_order) по преобладающему направлению и обнуляет счетчики.

> Замечание 1: направление сделки - это направление агрессора, то есть активной стороны, спровоцировавшей сделку.
>
> Замечание 2: заявка типа IOC сразу после выполнения всех возможный сведений автоматически удаляется из стакана, в отличие от лимитной заявки, которая остается в стакане после выполнения всех возможных сведений.

##### Рассмотрим базовый вариант стратегии: {#deals_count_diff_base}

```c++
#include "participant_strategy.h"

using namespace hftbattle;

class UserStrategy : public ParticipantStrategy {
public:
  explicit UserStrategy(const JsonValue& config) : last_reset_time_(0) {
    deals_count_by_dir_.fill(0);
    min_deals_count_diff_ = config["min_deals_count_diff"].as<int>(100);
    deals_reset_period_ms_ = config["deals_reset_period_ms"].as<Milliseconds>(10ms);

    std::cout << "min_deals_count_diff_: " << min_deals_count_diff_ << std::endl;
    std::cout << "deals_reset_period_ms_: " << deals_reset_period_ms_.count() << std::endl;
  }

  // Вызывается при получении новых сделок торгового инструмента:
  // @deals - вектор новых сделок.
  void trading_deals_update(const std::vector<Deal>& deals) override {
    for (const auto& deal : deals) {
      deals_count_by_dir_[deal.dir] += 1;
    }
    int deals_count_diff = std::abs(deals_count_by_dir_[ASK] -
                                    deals_count_by_dir_[BID]);
    bool trade = (deals_count_diff >= min_deals_count_diff_);
    if (trade) {
      Dir dir_to_beat = deals_count_by_dir_[ASK] >= deals_count_by_dir_[BID]
                              ? ASK
                              : BID;
      Price price_to_beat = trading_book_info.best_price(opposite_dir(dir_to_beat));
      Amount amount_to_beat = 1;
      add_ioc_order(dir_to_beat, price_to_beat, amount_to_beat);
    }

    Microseconds current_time = get_server_time();
    Microseconds time_diff_us = current_time - last_reset_time_;
    // Далее сравнивается величина time_diff_us в микросекундах
    // с величиной deals_reset_period_ms_ в миллисекундах -
    // это нормально, система сама понимает как сравнить два времени,
    // выраженные в разных единицах измерения.
    if (trade || time_diff_us >= deals_reset_period_ms_) {
      last_reset_time_ = current_time;
      deals_count_by_dir_[BID] = 0;
      deals_count_by_dir_[ASK] = 0;
    }
  }

private:
  Microseconds last_reset_time_;
  std::array<int, 2> deals_count_by_dir_;

  int min_deals_count_diff_;
  Milliseconds deals_reset_period_ms_;
};
```

##### Модифицируем предыдущую стратегию. {#deals_count_diff_limited}

Модифицируем предыдущую стратегию следующим образом: ограничим суммарный объём наших сделок, используя информацию, приходящую в функции [execution_report_update](/api/ParticipantStrategy.md#execution_report_update).

```c++
#include "participant_strategy.h"

using namespace hftbattle;

class UserStrategy : public ParticipantStrategy {
public:
  explicit UserStrategy(const JsonValue& config) :
      last_reset_time_(0),
      our_deals_total_amount_(0),
      trading_finished_(false) {
    deals_count_by_dir_.fill(0);
    min_deals_count_diff_ = config["min_deals_count_diff"].as<int>(100);
    deals_reset_period_ms_ = config["deals_reset_period_ms"].as<Milliseconds>(10ms);
    our_deals_max_total_amount_ = config["our_deals_max_total_amount"].as<Amount>(1000);

    std::cout << "min_deals_count_diff_: " << min_deals_count_diff_ << std::endl;
    std::cout << "deals_reset_period_ms_: " << deals_reset_period_ms_.count() << std::endl;
    std::cout << "our_deals_max_total_amount_: " << our_deals_max_total_amount_ << std::endl;
  }

  // Вызывается при получении нового стакана торгового инструмента:
  // @order_book — новый стакан.
  void trading_book_update(const OrderBook& order_book) override {
    /* написать свою реализацию здесь */
  }

  // Вызывается при получении новых сделок торгового инструмента:
  // @deals - вектор новых сделок.
  void trading_deals_update(const std::vector<Deal>& deals) override {
    if (trading_finished_) {
      return;
    }

    for (const auto& deal : deals) {
      deals_count_by_dir_[deal.dir] += 1;
    }
    int deals_count_diff = std::abs(deals_count_by_dir_[ASK] -
                                    deals_count_by_dir_[BID]);
    bool trade = (deals_count_diff >= min_deals_count_diff_);
    if (trade) {
      Dir dir_to_beat = deals_count_by_dir_[ASK] >= deals_count_by_dir_[BID]
                              ? ASK
                              : BID;
      Price price_to_beat = trading_book_info.best_price(opposite_dir(dir_to_beat));
      Amount amount_to_beat = 1;
      add_ioc_order(dir_to_beat, price_to_beat, amount_to_beat);
    }
    Microseconds current_time = get_server_time();
    Microseconds time_diff_us = current_time - last_reset_time_;
    // Далее сравнивается величина time_diff_us в микросекундах
    // с величиной deals_reset_period_ms_ в миллисекундах -
    // это нормально, система сама понимает как сравнить два времени,
    // выраженные в разных единицах измерения.
    if (trade || time_diff_us >= deals_reset_period_ms_) {
      last_reset_time_ = current_time;
      deals_count_by_dir_[BID] = 0;
      deals_count_by_dir_[ASK] = 0;
    }
  }

  // Вызывается при получении отчёта о сделке с участием вашего ордера:
  // @execution_report — структура-отчёт о совершённой сделке.
  void execution_report_update(const ExecutionReport& execution_report) override {
    our_deals_total_amount_ += execution_report.deal_amount();
    if (our_deals_total_amount_ > our_deals_max_total_amount_) {
      trading_finished_ = true;
    }
  }

private:
  Microseconds last_reset_time_;
  Amount our_deals_total_amount_;
  bool trading_finished_;
  std::array<int, 2> deals_count_by_dir_;

  int min_deals_count_diff_;
  Milliseconds deals_reset_period_ms_;
  Amount our_deals_max_total_amount_;
};
```

#### Improved ideas strategy {#improved_ideas}

Идея этой стратегии мы подробно описали в [блоге]({{ book["blog.url"] }}).
Стратегия даже в неизменённом виде позволяет набрать результат более 2000 на контрольной выборке.

```c++
#include "participant_strategy.h"
#include <set>

using namespace hftbattle;

class UserStrategy : public ParticipantStrategy {
public:
  explicit UserStrategy(const JsonValue& config) :
      max_executed_amount_(config["max_executed_amount"].as<Amount>(50)),
      max_amount_at_price_(config["max_amount_at_price"].as<Amount>(3)),
      max_amount_to_run_from_best_(config["max_amount_to_run_from_best"].as<Amount>(20)),
      max_soft_amount_to_run_from_best_(config["max_soft_amount_to_run_from_best"].as<Amount>(64)),
      max_after_amount_to_run_(config["max_after_amount_to_run"].as<Amount>(42)) {
    set_stop_loss_result(Decimal(-15000.0));
  }

  bool should_strategy_run_from_best_price(Dir dir) const {
    double dir_events = 1.0 * deletions_count(dir) + 1.0 * additions_count(opposite_dir(dir));
    // возможная оценка количества событий, которые могут сигнализировать о том, что скоро этой цены не станет
    double opposite_dir_events = 1.0 * deletions_count(opposite_dir(dir)) + 1.0 * additions_count(dir);
    // возможная оценка количества событий, которые могут сигнализировать о том, что скоро противоположной цены не станет
    return false;
    // Предлагается вместо false возвращать значение некоторого критерия, который будет говорить
    // о том, что есть большая вероятность исчезновения этой цены из стакана, и с нее лучше сняться.
    // Например, так: return (dir_events > 0.0); - если удалений + снятий больше 0, то стоит уходить с цены.
    // Подробности у нас в блоге http://blog.hftbattle.com.
    // Правильно реализовав это правило и подобрав параметры, мы получили результат
    // более 10000 на контрольной выборке, чего желаем и всем участникам!
  }

  // Удаление заявки с лучшей цены с учётом её положения в очереди. Снимаем заявку только в том случае,
  // когда она стоит в хвосте очереди.
  void delete_soft_second_version(Dir dir, Price price, Amount current_amount, Amount wanted_amount,
                                  const std::vector<OrderSnapshot>& current_orders) {
    if (!should_strategy_run_from_best_price(dir)) {
      // Сюда же можно добавить отсечение по нашим сделкам: если по текущей цене недавно
      // была проведена сделка, то, скорее всего, выставлять заявку на эту цену будет невыгодно.
      // Подробное описание идеи читайте в блоге!
      // Скажем лишь, что реализация этой идеи позволила нам увеличить результат на 4000.
      // Мы уже сохранили за вас цену и время последней сделки в полях last_our_deal_price_
      // и last_our_deal_moment_, так что вам осталось только написать правильное условие!
      manage_amount_on_price(dir, price, wanted_amount, current_orders);
      return;
    }
    for (auto it = current_orders.begin(); it != current_orders.end(); ++it) {
      auto order = *it;
      if (order->status() != OrderStatus::Active) {
        continue;
      }
      // считаем объём, стоящий после нашей заявки
      Amount amount_after_order = trading_book_info.best_volume(dir) - get_amount_before_order(order);
      // если с цены хорошо бы сняться и мы не сильно теряем очередь - то снимаем заявку
      if (should_strategy_run_from_best_price(dir) && amount_after_order < max_after_amount_to_run_) {
        delete_order(order);
      }
    }
  }

  // В этой функции мы снимаем заявку с лучшей цены при выполнении определённых условий.
  // В частности, если на котировке стоят заявки с малым суммарным объёмом или наши заявки находятся в конце очереди.
  // В противном случае (то есть если мы считаем что стоять на лучшей сейчас выгодно) -
  // мы просто обрабатываем ее как обычную цену.
  void manage_amount_on_best_price(Dir dir, Price price, Amount wanted_amount, const std::vector<OrderSnapshot>& current_orders) {
    auto current_amount = std::accumulate(current_orders.cbegin(), current_orders.cend(), 0,
                                          [](Amount amount, const OrderSnapshot &order) {
                                            return amount + order->amount;
                                          });
    // если объём на цене совсем маленький - то снимаемся с нее
    if (trading_book_info.best_volume(dir) - current_amount < max_amount_to_run_from_best_) {
      delete_all_at_price(dir, price);
      return;
    }
    // если объём не превышает какого-то - то думаем, стоит ли сняться, или все же стоит держать заявки
    if (trading_book_info.best_volume(dir) - current_amount < max_soft_amount_to_run_from_best_) {
      delete_soft_second_version(dir, price, current_amount, wanted_amount, current_orders);
      return;
    }
    // если объём достаточно большой - то просто обрабатываем цену как все другие
    manage_amount_on_price(dir, price, wanted_amount, current_orders);
  }

  // В этой функции мы поддерживаем необходимый объём заявок на ценовом уровне.
  void manage_amount_on_price(Dir dir, Price price, Amount wanted_amount, const std::vector<OrderSnapshot>& current_orders) {
    auto current_amount = std::accumulate(current_orders.cbegin(), current_orders.cend(), 0,
                                          [](Amount amount, const OrderSnapshot &order) {
                                            return amount + order->amount;
                                          });
    for(auto it = current_orders.rbegin(); it != current_orders.rend() && current_amount > wanted_amount; ++it) {
      auto order = *it;
      current_amount -= order->amount_rest();
      delete_order(order);
    }
    for(int i = 0; i < wanted_amount - current_amount; ++i) {
      // Мы выставляем заявки объёмом 1 лот, чтобы мы могли снимать только часть объёма,
      // выставленного на ценовом уровне, без потери места в очереди внутри котировки.
      add_limit_order(dir, price, 1);
    }
  }

  void trading_book_update(const OrderBook& order_book) override {
    // Добавляем отсечение по времени, чтобы торговать только в определённый период дня.
    if (get_server_time_tm().tm_hour < 12) {
      return;
    }
    // Обновляем списки недавних постановок и снятий на лучшие цены
    update_deletions_and_additions();
    for (Dir dir : {BID, ASK}) {
      // Удаляем заявки с далёких ценовых уровней, чтобы не попадать под ограничение на набираемую позицию.
      for(auto& order: trading_book_info.orders().orders_by_dir[dir]) {
        if (trading_book().get_index_by_price(dir, order->price) > 9) {
          delete_order(order);
        }
      }
      // Выставляем заявки на все видимые котировки в стакане.
      auto active_orders = trading_book_info.orders().get_orders_by_dir_to_map(dir);
      for (const auto& quote : trading_book().all_quotes(dir)) {
        Price quote_price = quote.get_price();
        Amount amount = get_wanted_amount(dir, quote_price);
        // По отдельности обрабатываем лучшие ценовые уровни и все остальные.
        if (quote_price == trading_book_info.best_price(dir)) {
          manage_amount_on_best_price(dir, quote_price, amount, active_orders[quote_price]);
        }
        else {
          manage_amount_on_price(dir, quote_price, amount, active_orders[quote_price]);
        }
      }
    }
  }

  void execution_report_update(const ExecutionReport& snapshot) override {
    auto dir = snapshot.dir();
    last_our_deal_price_[dir] = snapshot.deal_price();
    last_our_deal_moment_[dir] = get_server_time();
  }

private:
  using Events = std::set<Microseconds>;

  Amount max_executed_amount_;
  Amount max_amount_at_price_;
  Amount max_amount_to_run_from_best_;
  Amount max_soft_amount_to_run_from_best_;
  Amount max_after_amount_to_run_;

  std::array<Price, 2> last_best_price_;
  std::array<Events, 2> deletions_by_dir_;
  std::array<Events, 2> additions_by_dir_;
  Microseconds last_reset_time_;

  std::array<Price, 2> last_our_deal_price_;
  std::array<Microseconds, 2> last_our_deal_moment_;

  // Возвращает количество снятых заявок с лучшей цены по направлению @dir за определённый промежуток времени.
  size_t deletions_count(Dir dir) const {
    return deletions_by_dir_[dir].size();
  }

  // Возвращает количество добавленных заявок на лучшую цену по направлению @dir за определённый промежуток времени.
  size_t additions_count(Dir dir) const {
    return additions_by_dir_[dir].size();
  }

  void delete_all_at_price(Dir dir, Price price) {
    auto orders_map = trading_book_info.orders().get_orders_by_dir_to_map(dir);
    auto it = orders_map.find(price);
    if (it == orders_map.end()) {
      return;
    }
    auto& active_orders = it->second;
    for (auto& order : active_orders) {
      delete_order(order);
    }
  }

  // Возвращает объём, который нужно поддерживать на ценовом уровне.
  // В этом примере представлена очень простая версия этой функции, но зависимость объёма
  // от цены и направления может быть куда более сложной.
  Amount get_wanted_amount(Dir dir, Price price) const {
    Amount wanted_amount = std::min(max_executed_amount_ - dir_sign(dir) * trading_book_info.total_amount(), max_amount_at_price_);
    return std::max(0, wanted_amount);
  }

  // Обновление объёмов снятых и добавленных заявок на лучших ценах.
  void update_deletions_and_additions() {
    for (Dir dir : {BID, ASK}) {
      Price best_price = trading_book_info.best_price(dir);
      auto& deletions = deletions_by_dir_[dir];
      auto& additions = additions_by_dir_[dir];
      // Если лучшая цена изменилась, то сбросим объёмы.
      if (best_price != last_best_price_[dir]) {
        deletions.clear();
        additions.clear();
        last_best_price_[dir] = best_price;
      }
      update_events(deletions, trading_book_info.get_deleted_volume_at_price(dir, best_price));
      update_events(additions, trading_book_info.get_added_volume_at_price(dir, best_price));
    }
  }

  void update_events(Events& events, Amount changed_volume) const {
    Microseconds current_time = get_server_time();
    // если событие было давно - то перестаем его учитывать
    while (!events.empty() && current_time - *events.begin() > 20ms) {
      events.erase(events.begin());
    }
    if (changed_volume > 0) {
      events.emplace(current_time);
    }
  }
};
```
 -->