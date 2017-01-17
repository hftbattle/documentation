### Графики

Полезным инструментом для анализа поведения стратегии являются графики различных характеристик.

В данном разделе будут обсуждены:

- [Графики прибыли и позиции](#sum_and_pos_chart)
- [Переход во Viewer](#links)
- [Добавление собственных графиков](#custom)

#### Графики прибыли и позиции<a id="sum_and_pos_chart"></a>

По умолчанию для каждой тренировочной посылки строится график "Chart 0", на котором изображены 2 линии:

- *sum* – ваша прибыль в долларах в течение торговой сессии, её значения отмечены на левой вертикальной оси
- *[symbol_name]-pos* – ваша позиция, то есть разность числа купленных и проданных вами лотов

Значениям позиции соответствует правая вертикальная ось.

<img src="/img/base_chart.png" alt="График лучшей цены" align="center">

> Замечание: данные графики отображают линии со сглаживанием.
Поэтому при наведении курсора на конкретную точку, вы можете получить как конкретное значение величины в данный момент времени, так и её среднее (сглаженное) значение за указанный промежуток.
Если вам нужно узнать конкретное значение, то вы можете увеличить масштаб, выделив необходимую область на графике.

#### Переход во Viewer<a id="links"></a>

Зачастую, глядя на графики, хочется проанализировать ситуацию, происходившую в тот или иной момент в реальном торговом стакане.
Для этого предусмотрен переход из графиков в инструмент для просмотра реального торгового стакана и ваших сделок – [Viewer](viewer.md).
Нажав на интересующую вас точку на графике, можно открыть этот момент во Viewer-е.

#### Добавление собственных графиков<a id="custom"></a>

Также имеется возможность добавлять свои графики из стратегии с помощью функции *add_chart_point*:

```c++
void add_chart_point(const std::string& line_name,
                     double value,
                     ChartYAxisType y_axis_type,
                     uint8_t chart_number)
```

- *line_name* - имя оси, отображается в легенде
- *value* - вещественное значение
- *y_axis_type* - с какой стороны изображать вертикальную ось: *ChartYAxisType::Left* или *ChartYAxisType::Right*
- *chart_number* - номер графика (0 - график по умолчанию с результатом и позой, 1 и более - ваши собственные графики)

Модифицируем, например, стратегию, стоящую на каждом из направлений на лучшей цене так, чтобы она "рисовала" график лучшей цены:

```c++
#include "participant_strategy.h"
#include <string>

using namespace hftbattle;

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
    auto our_orders = trading_book->orders();
    for (Dir dir: {BID, ASK}) {
      const Price best_price = trading_book->best_price(dir);
      const Amount amount = 1;
      if (our_orders.active_orders_count(dir) == 0) {
        add_limit_order(dir, best_price, amount);
      } else {  // есть хотя бы одна наша активная заявка
        auto first_order = our_orders.orders_by_dir(dir)[0];
        const bool on_best_price = (first_order.price() == best_price);
        if (!on_best_price) {  // наша заявка стоит, но не на текущей лучшей цене
          delete_order(first_order);
          add_limit_order(dir, best_price, amount);
        }
      }

      if (best_price != best_price_by_dir[dir]) { // лучшая цена изменилась
        add_chart_point(axis_name[dir],           // имя оси, отображается в легенде
                        best_price.get_double(),  // переводим тип Price в double
                        ChartYAxisType::Left,     // используем левую вертикальную ось
                        1);                       // 1 - номер графика
        best_price_by_dir[dir] = order_book->best_price(dir);
      }
    }
  }

};

REGISTER_CONTEST_STRATEGY(UserStrategy, user_strategy)
```

В результате на графике "Chart 1" получим следующую картинку:

<img src="/img/best_price_chart.png" alt="График лучшей цены" align="center">
