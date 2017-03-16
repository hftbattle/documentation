## Примеры стратегий
<!-- TODO(asalikhov): add good strategies when they will be completely finished -->

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
Согласно правилам, [позиция](/terms.md#position) не может быть превышать {{ book["constraint.position"] }}, подробнее см. [Ограничения симулятора](/simulator/restrictions.md).
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
    set_max_total_amount(config["max_total_amount"].as<int>(100));
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
