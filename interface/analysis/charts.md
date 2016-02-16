### Графики
По умолчанию для каждой тренировочной посылки строится график результата и позиции в течение дня. Также имеется возможность добавлять свои графики из стратегии с помощью функции *add_chart_point*:
```c++
void add_chart_point(const std::string& line_name, 
                     double value, 
                     ChartYAxisType y_axis_type, 
                     int8_t chart_number)
```
- *line_name* - имя оси, отображается в легенде,
- 

Например, стратегия, "рисующая" график лучшей цены выглядит так:
```c++
#include "strategy/participant_strategy_layer.h"

using namespace contest_platform;

class UserStrategy : public ParticipantStrategy {
private:
  std::array<Price, 2> best_price_by_dir;
  std::array<std::string, 2> axis_name;
public:
  UserStrategy(JsonValue config) {
    axis_name[BID] = "best_bid";
    axis_name[ASK] = "best_ask";
  }

  void trading_book_update(const OrderBook& order_book) override {
    for (const auto dir : { BID, ASK }) {
      const Price best_price = order_book.best_price(dir);
      if (best_price != best_price_by_dir[dir]) {
        add_chart_point(axis_name[dir],           // имя оси, отображается в легенде
                        best_price.get_double(),  // переводим тип Price в double
                        ChartYAxisType::Left,     // используем левую вертикальную ось
                        1);                       // 1 - номер графика
        best_price_by_dir[dir] = order_book.best_price(dir);
      }
    }

  }

};
```