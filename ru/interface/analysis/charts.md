### Графики

Графики являются полезным инструментом для анализа поведения стратегии.

В данном разделе будет рассказано про:

- [Графики прибыли и позиции](#sum_and_pos_chart)
- [Переход во Viewer](#links)
- [Добавление собственных графиков](#custom)

#### Графики прибыли и позиции {#sum_and_pos_chart}

<!-- TODO(asalikhov): change Chart 0 to sth. else when changed in web system -->
По умолчанию для каждой тренировочной посылки строится график "Chart 0", на котором изображены 2 линии:

- *sum* — ваша прибыль в течение торговой сессии, её значения отмечены на левой вертикальной оси.
- *[symbol_name]-pos* — ваша позиция, то есть разность числа купленных и проданных вами лотов, её значения отмечены на правой вертикальной оси.

<img src="{{ book["gitbook.img"] }}/base_chart.png" title="График лучшей цены" align="center">

> Замечание: графики отображают линии со сглаживанием.
> Если вам нужно узнать конкретное значение, то вы можете увеличить масштаб, выделив необходимую область на графике.

#### Переход во Viewer {#links}

Зачастую, глядя на графики, хочется проанализировать ситуацию, происходившую в тот или иной момент в реальном торговом стакане.
Для этого предусмотрен переход из графиков в инструмент для просмотра реального торгового стакана и ваших сделок — [Viewer](viewer.md).
Нажав на интересующую вас точку на графике, можно открыть этот момент во Viewer.

#### Добавление собственных графиков {#custom}

Вы можете добавлять свои графики из стратегии с помощью метода *add_chart_point*:

{% codetabs name="C++", type="c++" -%}
void add_chart_point(const std::string& line_name,
                     Decimal value,  // вместо Decimal можно передавать double
                     ChartYAxisType y_axis_type = ChartYAxisType::Left,
                     uint8_t chart_number = 1)
{%- language name="Python", type="py" -%}
def add_chart_point(self,
                    line_name,
                    value,
                    y_axis_type = ChartYAxisType.Left,
                    chart_number = 1)
{%- endcodetabs %}

- *line_name* — имя оси, отображается в легенде графика.
- *value* — вещественное значение, которое нужно отобразить на графике.
- *y_axis_type* — сторона, с которой изображается вертикальная ось: *ChartYAxisType::Left* или *ChartYAxisType::Right* (ChartYAxisType.Left или ChartYAxisType.Right для *Python*).
- *chart_number* — номер графика (0 — график по умолчанию с результатом и позицией, с номерами 1 и более — ваши собственные графики).

Модифицируем стратегию, стоящую на каждом из направлений на лучшей цене, так, чтобы строился график лучшей цены:

{% codetabs name="C++", type="c++" -%}
#include "participant_strategy.h"
#include <array>
#include <string>

using namespace hftbattle;

namespace {

class UserStrategy : public ParticipantStrategy {
public:
  explicit UserStrategy(const JsonValue& config) {
    axis_name[BID] = "best_bid";
    axis_name[ASK] = "best_ask";
  }

  void trading_book_update(const OrderBook& order_book) override {
    const auto& our_orders = order_book.orders();
    for (Dir dir : {BID, ASK}) {
      Price best_price = order_book.best_price(dir);
      Amount amount = 1;
      if (our_orders.active_orders_count(dir) == 0) {
        add_limit_order(dir, best_price, amount);
      } else {  // есть хотя бы одна наша активная заявка
        auto first_order = our_orders.orders_by_dir(dir).front();
        bool on_best_price = (first_order->price() == best_price);
        if (!on_best_price) {  // наша заявка стоит, но не на текущей лучшей цене
          delete_order(first_order);
          add_limit_order(dir, best_price, amount);
        }
      }

      if (best_price != best_price_by_dir[dir]) {  // лучшая цена изменилась
        add_chart_point(axis_name[dir],            // имя оси, отображается в легенде
                        best_price);
        best_price_by_dir[dir] = best_price;
      }
    }
  }

private:
  std::array<Price, 2> best_price_by_dir;
  std::array<std::string, 2> axis_name;
};

}  // namespace

REGISTER_CONTEST_STRATEGY(UserStrategy, user_strategy)
{%- language name="Python", type="py" -%}
# -*- coding: utf-8 -*-

from py_defs import *
from py_defs import Decimal as Price
from common_enums import *

best_price_by_dir = [Price(), Price()]
axis_name = ["best_bid", "best_ask"]


def trading_book_update(strat, order_book):
    global best_price_by_dir
    our_orders = order_book.orders()
    for dir in (BID, ASK):
        best_price = order_book.best_price(dir)
        amount = 1
        if our_orders.active_orders_count(dir) == 0:
            strat.add_limit_order(dir, best_price, amount)
        else:
            first_order = our_orders.orders_by_dir(dir)[0]
            on_best_price = (first_order.price() == best_price)
            if not on_best_price:  # наша заявка стоит, но не на текущей лучшей цене
                strat.delete_order(first_order)
                strat.add_limit_order(dir, best_price, amount)

        if best_price != best_price_by_dir[dir]:
            strat.add_chart_point(axis_name[dir],
                                  best_price)
            best_price_by_dir[dir] = best_price
{%- endcodetabs %}

В результате на графике "Chart 1" получим следующую картинку:

<img src="{{ book["gitbook.img"] }}/best_price_chart.png" title="График лучшей цены" align="center">
