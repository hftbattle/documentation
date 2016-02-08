##Примеры стратегий

Рассмотрим здесь несколько примеров торговых стратегий:

* [Strategy template](#strategy_template)
* [Stay on each direction strategy](#stay_on_each_dir_strategy)
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
