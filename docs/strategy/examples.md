##Примеры стратегий

Рассмотрим здесь несколько примеров торговых стратегий:

* [Strategy template](#strategy_template)
* [Stay on each direction strategy](#stay_on_each_dir)
    * [Base](#stay_on_each_dir_base)
    * [Gap](#stay_on_each_dir_with_gap)
* [Deals count diff strategy](#deals_count_diff)
    * [Base](#deals_count_diff_base)
    * [Limit](#deals_count_diff_with_limit)

<a name="strategy_template"></a>
### Strategy template

Приведенный ниже код представляет собой шаблон для написания стратегий, торгующих на одном инструменте.
```cpp
#include "strategy/participant_strategy_layer.h"

using namespace contest_platform;

class UserStrategy : public ParticipantStrategy {
public:
  // В конструктор стратегии участника передается конфиг.
  // В конфиг из веб-интерфейса можно передать параметры стратегии.
  UserStrategy(JsonValue config) {}

  // Вызывается при получении нового стакана торгового инструмента:
  // @order_book – новый стакан.
  void trading_book_update(const OrderBook& order_book) override {
    /* написать свою реализацию здесь */
  }

  // Вызывается при получении новых сделок торгового инструмента:
  // @deals - вектор новых сделок.
  void trading_deals_update(const std::vector<Deal>& deals) override {
    /* написать свою реализацию здесь */
  }

  // Вызывается при получении отчета о сделке с участием вашего ордера:
  // @snapshot – структура-отчет о совершенной сделке.
  void execution_report_update(const ExecutionReportSnapshot& snapshot) override {
    /* написать свою реализацию здесь */
  }

};
```

Для стратегий, торгующих на одном инструменте и получающих обновления от некоторого сигнального инструмента понадобятся еще две функции:
```cpp
  // Вызывается при получении нового стакана сигнального инструмента:
  // @order_book – новый стакан.
  void signal_book_update(const OrderBook& order_book) override {
    /* написать свою реализацию здесь */
  }

  // Вызывается при получении новых сделок сигнального инструмента:
  // @deals - вектор новых сделок.
  void signal_deals_update(const std::vector<Deal>& deals) override {
    /* написать свою реализацию здесь */
  }
```

<a name="stay_on_each_dir"></a>
### Stay on each direction strategy

Идея стратегии состоит в том, чтобы поддерживать на каждом направлении (*BID* и *ASK*) по одной нашей заявке. В случае, если на направлении нет наших активных заявок, она ставит заявку объемом 1 на определенную цену. 

<a name="stay_on_each_dir_base"></a>
Рассмотрим базовый вариант стратегии, когда заявка ставится на лучшую цену по данному направлению:

```cpp
#include "strategy/participant_strategy_layer.h"

using namespace contest_platform;

class UserStrategy : public ParticipantStrategy {
public:
  UserStrategy(JsonValue config) {}

  // Вызывается при получении нового стакана торгового инструмента:
  // @order_book – новый стакан.
  void trading_book_update(const OrderBook& order_book) override {
    for (Dir dir : {BID, ASK}) {
      if (trading_book_info.orders().active_orders_count(dir) == 0) {
        const Price price = trading_book_info.best_price(dir);
        const Amount amount = 1;
        add_limit_order(dir, price, amount);
      }
    }
  }
  
};
```

<a name="stay_on_each_dir_with_gap"></a>
Модифицируем предыдущую стратегию. Пусть цена выставляемой заявки определяется как цена, отстоящая от лучшей цены на *gap_from_best_price* минимальных шагов цены. Параметр *gap_from_best_price* по умолчанию равен нулю, однако его значение можно поменять извне, передав в стратегию значение или список значений для *gap_from_best_price*. Подробнее в разделе [Перебор параметров](../../web-interface/params.md).

```cpp
#include "strategy/participant_strategy_layer.h"

using namespace contest_platform;

class UserStrategy : public ParticipantStrategy {
public:
  UserStrategy(JsonValue config) {
    gap_from_best_price_ = config["gap_from_best_price"].as<int>(0);
    std::cout << "cout: gap_from_best_price_ = " << gap_from_best_price_ << std::endl;
    INFO() << "info: gap_from_best_price_ = " << gap_from_best_price_;
  }

  // Вызывается при получении нового стакана торгового инструмента:
  // @order_book – новый стакан.
  void trading_book_update(const OrderBook& order_book) override {
    for (Dir dir : {BID, ASK}) {
      if (trading_book_info.orders().active_orders_count(dir) == 0) {
        Price price_shift = dir_sign(dir) *
                            gap_from_best_price_ *
                            trading_book_info.min_step();
        const Price price = trading_book_info.best_price(dir) - price_shift;
        const Amount amount = 1;
        add_limit_order(dir, price, amount);
      }
    }
  }
  
private:
  int gap_from_best_price_;
  
};

```

<a name="deals_count_diff"></a>
### Deals count diff strategy

Данная стратегия торгует на основе произошедших на бирже сделок: если абсолютная разность количества сделок на биде и аске за определенное время (*deals\_reset\_period\_ms\_*) больше некоторойфиксированной величины (*min\_deals\_count\_diff\_*), то стратегия выставляет заявку типа [IOC (Immediate-Or-Cancel)](../../glossary.md#ioc_order) по преобладающему направлению и обнуляет счетчики.
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

<a name="deals_count_diff_with_limit"></a>
Модифицируем предыдущую стратегию следующим образом: ограничим суммарный объем наших сделок, используя информацию, приходящую в функции [execution_report_update](../api/ParticipantStrategyLayer.md#execution_report_update).


```cpp

#include "strategy/participant_strategy_layer.h"

using namespace contest_platform;
class UserStrategy : public ParticipantStrategy {
private:
  Microseconds last_reset_time_;
  Amount our_deals_total_amount_;
  bool trading_finished_;
  std::array<int, 2> deals_count_by_dir_;

  int min_deals_count_diff_;
  Milliseconds deals_reset_period_ms_;
  Amount our_deals_max_total_amount_;

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

};
```
    