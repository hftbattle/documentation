## Примеры стратегий

### Default strategy

Приведенная ниже стратегия представляет собой 
 * UserStrategy - основной класс, в котором пользователь реализует свою стратегию.
 **/

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