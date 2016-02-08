##Примеры стратегий

Рассмотрим здесь несколько примеров торговых стратегий:

* [Strategy template](#strategy_template)
* [Stay on each direction strategy](#stay_on_each_dir_strategy)
* [Stay on each direction with gap strategy](#stay_on_each_dir_strategy)
* [Deals count diff strategy](#deals_count_diff_strategy)


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

<a name="stay_on_each_dir_strategy"></a>
### Stay on each direction strategy

Данная стратегия состоит в том, чтобы поддерживать на каждом направлении (*BID* и *ASK*) по одной нашей заявке. В случае, если на направлении нет наших активных заявок, она ставит заявку объемом 1 на лучшую в данный момент цену по этому направлению.

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


###Stay on each direction with gap strategy
Данная стратегия - модификация предыдущей стратегии. Здесь цена выставляемой заявки определяется как цена, отстоящая от лучшей цены на *gap_from_best_price* минимальных шагов цены. Параметр *gap_from_best_price* по умолчанию равен нулю, однако его значение можно поменять извне, передав в стратегию значение или список значений для *gap_from_best_price*. Подробнее в разделе [Перебор параметров](../../web-interface/params.md).

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
