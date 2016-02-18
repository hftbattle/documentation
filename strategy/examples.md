##Примеры стратегий

Рассмотрим здесь несколько примеров торговых стратегий:

* [Stay on best price strategy](#stay_on_best_price)
    * [Base](#stay_on_best_price)
    * [Improved](#stay_on_best_price_improved) 
* [Deals count diff strategy](#deals_count_diff)
    * [Base](#deals_count_diff_base)
    * [Limited](#deals_count_diff_limited)


<a name="stay_on_best_price"></a>
#### Stay on best price strategy

Идея стратегии состоит в том, чтобы поддерживать на каждом направлении (*BID* и *ASK*) по одной нашей заявке на лучшей цене. В случае, если на направлении нет наших активных заявок, она ставит заявку объемом 1 на лучшую цену. Если заявка уже есть, но она стоит не на лучшей - мы ее снимаем и ставим новую на лучшую цену. 

<a name="stay_on_best_price"></a>
Рассмотрим базовый вариант стратегии:

```cpp
#include "strategy/participant_strategy_layer.h"

using namespace contest_platform;

class UserStrategy : public ParticipantStrategy {
public:
  UserStrategy(JsonValue config) {}

  void trading_book_update(const OrderBook& order_book) override {
    auto our_orders = trading_book_info.orders();
    for (Dir dir: {BID, ASK}) {
      const Price best_price = trading_book_info.best_price(dir);
      const Amount amount = 1;
      if (our_orders.active_orders_count(dir) == 0) {
        add_limit_order(dir, best_price, amount);
      } else {  // есть хотя бы одна наша активная заявка
        auto first_order = our_orders.orders_by_dir[dir][0];
        bool our_order_on_best_price = first_order->price == best_price;
        if (!our_order_on_best_price) {  // наша заявка стоит, но не на текущей лучшей цене
          delete_order(first_order);
          add_limit_order(dir, best_price, amount);
        }
      }
    }
  }

};
```
<a name="stay_on_best_price_improved"></a>
Модифицируем предыдущую стратегию. Поскольку стратегия поддерживает только 1 заявку размером в 1 лот по каждому направлению, то свою позицию она меняет очень медленно. Для HFT-стратегий очень важна возможность быстро вернуться к нулевой позиции, поэтому мы попробуем ограничить максимально допустимую открытую позицию. Для этого достаточно в конфиге задать парамет *max\_executed\_amount*. Согласно правилам оно не может быть превышать 50, подробнее см. [Ограничения симулятора](../simulator.md#restrictions). Оптимальное значение можно подобрать, перебрав разные варианты в системе. Подробнее в разделе [Перебор параметров](../interface/params.md). 

Также применим следующую простую оптимизацию: если на лучшей цене стоит объем меньший чем *min\_amount\_to\_stay\_on\_best\_*, то мы на нее выставляться не будем (и снимем заявку если уже там стоим).

```cpp
#include "strategy/participant_strategy_layer.h"

using namespace contest_platform;

class UserStrategy : public ParticipantStrategy {
public:
  UserStrategy(JsonValue config) {
    min_amount_to_stay_on_best_ = config["min_amount_to_stay_on_best_"].as<int>(10);
  }

  void trading_book_update(const OrderBook& order_book) override {
    for (Dir dir : {BID, ASK}) {
      const Price price = trading_book_info.best_price(dir);
      auto our_orders = trading_book_info.orders();
      bool no_our_orders = (our_orders.active_orders_count(dir) == 0);
      bool our_order_on_best_price = !no_our_orders &&
        our_orders.orders_by_dir[dir][0]->price == price;
      if (!no_our_orders && !our_order_on_best_price) {
        delete_order(our_orders.orders_by_dir[dir][0]);
      }
      if (our_order_on_best_price &&
        trading_book_info.best_volume(dir) < min_amount_to_stay_on_best_) {
        delete_order(our_orders.orders_by_dir[dir][0]);
      }
      if (!our_order_on_best_price &&
        trading_book_info.best_volume(dir) >= min_amount_to_stay_on_best_) {
        const Amount amount = 1;
        add_limit_order(dir, price, amount);
      }
    }
  }

private:
  Amount min_amount_to_stay_on_best_;
};

```

<a name="deals_count_diff"></a>
#### Deals count diff strategy

Данная стратегия торгует на основе произошедших на бирже сделок: если абсолютная разность количества сделок на биде и аске за определенное время (*deals\_reset\_period\_ms\_*) больше некоторойфиксированной величины (*min\_deals\_count\_diff\_*), то стратегия выставляет заявку типа [IOC (Immediate-Or-Cancel)](../terms.md#ioc_order) по преобладающему направлению и обнуляет счетчики.
>  Замечание 1: направление сделки - это направление агрессора, то есть активной стороны, спровоцировавшей сделку.

> Замечание 2: заявка типа IOC сразу после выполнения всех возможный сведений автоматически удаляется из стакана, в отличие от лимитной заявки, которая остается в стакане после выполнения всех возможных сведений.

<a name="deals_count_diff_base"></a>
```cpp
#include "strategy/participant_strategy_layer.h"

using namespace contest_platform;

class UserStrategy : public ParticipantStrategy {
public:
  UserStrategy(JsonValue config)
  : last_reset_time_(0)
  {
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
    const int deals_count_diff = std::abs(deals_count_by_dir_[ASK] -
                                          deals_count_by_dir_[BID]);
    bool trade = deals_count_diff >= min_deals_count_diff_;
    if (trade) {
      const Dir dir_to_beat = deals_count_by_dir_[ASK] >= deals_count_by_dir_[BID]
                              ? ASK
                              : BID;
      const Price price_to_beat = trading_book_info.best_price(opposite_dir(dir_to_beat));
      const Amount amount_to_beat = 1;
      add_ioc_order(dir_to_beat, price_to_beat, amount_to_beat);
    }

    const Microseconds current_time = get_server_time();
    const Microseconds time_diff_us = current_time - last_reset_time_;
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

<a name="deals_count_diff_limited"></a>
Модифицируем предыдущую стратегию следующим образом: ограничим суммарный объем наших сделок, используя информацию, приходящую в функции [execution_report_update](../api/ParticipantStrategy.md#execution_report_update).


```cpp

#include "strategy/participant_strategy_layer.h"

using namespace contest_platform;

class UserStrategy : public ParticipantStrategy {
public:
  UserStrategy(JsonValue config)
  : last_reset_time_(0)
  , our_deals_total_amount_(0)
  , trading_finished_(false)
  {
    deals_count_by_dir_.fill(0);
    min_deals_count_diff_ = config["min_deals_count_diff"].as<int>(100);
    deals_reset_period_ms_ = config["deals_reset_period_ms"].as<Milliseconds>(10ms);
    our_deals_max_total_amount_ = config["our_deals_max_total_amount"].as<Amount>(1000);

    std::cout << "min_deals_count_diff_: " << min_deals_count_diff_ << std::endl;
    std::cout << "deals_reset_period_ms_: " << deals_reset_period_ms_.count() << std::endl;
    std::cout << "our_deals_max_total_amount_: " << our_deals_max_total_amount_ << std::endl;
  }

  // Вызывается при получении нового стакана торгового инструмента:
  // @order_book – новый стакан.
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
    const int deals_count_diff = std::abs(deals_count_by_dir_[ASK] -
                                          deals_count_by_dir_[BID]);
    bool trade = deals_count_diff >= min_deals_count_diff_;
    if (trade) {
      const Dir dir_to_beat = deals_count_by_dir_[ASK] >= deals_count_by_dir_[BID]
                              ? ASK
                              : BID;
      const Price price_to_beat = trading_book_info.best_price(opposite_dir(dir_to_beat));
      const Amount amount_to_beat = 1;
      add_ioc_order(dir_to_beat, price_to_beat, amount_to_beat);
    }

    const Microseconds current_time = get_server_time();
    const Microseconds time_diff_us = current_time - last_reset_time_;
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

  // Вызывается при получении отчета о сделке с участием вашего ордера:
  // @snapshot – структура-отчет о совершенной сделке.
  void execution_report_update(const ExecutionReportSnapshot& snapshot) override {
    our_deals_total_amount_ += snapshot.deal_amount();
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
    