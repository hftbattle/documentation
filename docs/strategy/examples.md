#Примеры стратегий

* [Stay on best prices strategy](#stay_on_best_prices_strategy)


<a name="stay_on_best_prices_strategy"></a>
## Stay on best prices strategy
Реализуем следующую стратегию: будем поддерживать наши заявки на лучшей цене в обоих направлениях. 

Сначала научимся ставить заявку. Для этого предназначена функция [add_order](../../api/ParticipantStrategy.md#add_order). Чтобы узнать лучшую цену по направлению, можно воспользоваться функцией [best_price](../../api/ParticipantStrategy.md#best_price). 
Будем выполнять все действия внутри функции [event_end_update](../../api/ParticipantStrategy.md#event_end_update), гарантируя тем самым, что биржевое событие полностью обработано:

```cpp
virtual void event_end_update() {
	for (Dir dir: {BID, ASK}) {
		add_order(best_price(dir), 1, dir);
	}
}
```

Однако в тот момент, когда у нас вызывается функция **process_event_end**, наши заявки, поставленные в прошлом вызове этой функции, всё еще могут быть нереализованными. В итоге, у нас может скопиться огромное количество заявок, вскоре мы превысим лимит на количество заявок в день, что явно не способствует хорошему результату. К счастью, у нас есть поле, в котором хранятся все наши активные заявки:
```cpp
std::array<std::vector<OrderSnapshot>, 2>& active_orders;
```
Важно отметить, что оно хранит те заявки, которые мы уже отправили, но на которые еще не отправили запрос на удаление. Поэтому если для какой-то заявки будет вызван метод delete(order), то к следующему апдейту этой заявки в active_orders точно не будет (даже если она еще не успела удалиться). Также отметим, что обновление active_orders происходит только между апдейтами, внутри апдейта она не меняется.

Будем ставить заявку, если не существует активной заявки по этому направлению:
```cpp
virtual void process_event_end() {
  for (Dir dir:{BID, ASK}) {
    if (active_orders[dir].size() == 0) {
      add_order(best_price(dir), 1, dir, 0);
    }
  }
}
```
В такой реализации есть минус - если лучшая цена изменится, то мы не реагируем на это, что может привести к тому, что мы очень долго не будем торговать по одному направлению. Исправим это:
```cpp
virtual void process_event_end() {
  for (Dir dir:{BID, ASK}) {
    if (active_orders[dir].size() == 0) {
      add_order(best_price(dir), 1, dir, 0);
    } else if (active_orders[dir][0]->price != best_price(dir)) {
      delete_order(active_orders[dir][0]);
      add_order(best_price(dir), 1, dir, 0);
    }
  }
}
```
Теперь сосредоточимся на общей структуре стратегии.
Так как мы торгуем всегда внутри одного дня, то к его концу нам необходимо закрыть все позиции, то есть за день количество проданных и купленных активов должно быть одинаковым. Вообще говоря, если у вас к концу дня остались открытые позиции, то они будут автоматически закрыты симулятором по рыночной стоимости в конце дня, и это напрямую повлияет на ваш результат. Поэтому хорошее закрытие в конце дня - важная часть стратегии.  

В нашем примере мы реализуем самый простой вариант закрытия позиций - при наступлении фиксированного времени мы просто снимем все свои заявки и закроем все позиции по текущей лучшей цене. Вот как это выглядит:
```cpp
void simple_liquidate() {
  for (Dir dir: {BID, ASK}) {
    for (auto order: active_orders[dir]) {
      delete_order(order);
    }
  }
  Dir dir_close = ((total_amount() > 0)?(ASK):(BID));
  if (total_amount() != 0) {
      add_ioc_order(best_price(opposite_dir(dir_close)),
                    abs(total_amount()),
                    dir_close,
                    abs(total_amount()));
  }
}

const int32_t close_time_hour = 19;

virtual void process_event_end() {
  if (get_local_time_tm().tm_hour < close_time_hour) {
    go_quoting();
  } else {
    simple_liquidate();
  }
}

void go_quoting() {
  for (Dir dir:{BID, ASK}) {
    if (active_orders[dir].size() == 0) {
      add_order(best_price(dir), 1, dir, 0);
    } else if (active_orders[dir][0]->price != best_price(dir)) {
      delete_order(active_orders[dir][0]);
      add_order(best_price(dir), 1, dir, 0);
    }
  }
}
```
В этом коде при закрытии позиции мы используем функцию **add_ioc_order**, которая работает следующим образом: она пытается реализовать данную заявку по обычным правилам, и если у неё не выходит это сделать, то она автоматически удаляется (ioc расшифровывается как immediately or cancel).

Добавим последний, но немаловажный штрих к нашей стратегии, а именно ограничим объём открытых позиций величиной **max_executed_amount = 10**. Если этого не сделать, то к концу дня может скопиться огромная поза, и фактически успех торгового дня будет зависеть от того, по какой стоимости эти позиции будут ликвидированы.

Итоговый код простейшей торговой стратегии:
```cpp
#include "strategy/participant_strategy_layer.h"

using namespace xor_platform;
using namespace contest_platform;

// Это основной класс, в котором пользователь реализует свою стратегию.
class UserStrategy : public ParticipantStrategy {
public:

  virtual void book_trade_update(const OrderBook &order_book) {
    // написать свою реализацию здесь
  }

  virtual void book_feed_update(const OrderBook &order_book) {
    // написать свою реализацию здесь
  }

  virtual void trades_trade_update(const std::vector<Trade> &trades) {
    // написать свою реализацию здесь
  }

  virtual void trades_feed_update(const std::vector<Trade> &trades) {
    // написать свою реализацию здесь
  }

  const int32_t close_time_hour = 19;
  const int32_t max_executed_amount = 10;

  void simple_liquidate() {
    for (Dir dir: {BID, ASK}) {
      for (auto order: active_orders[dir]) {
        delete_order(order);
      }
    }
    Dir dir_close = ((total_amount() > 0)?(ASK):(BID));
    if (total_amount() != 0) {
      add_ioc_order(best_price(opposite_dir(dir_close)),
                    abs(total_amount()),
                    dir_close,
                    abs(total_amount()));
    }
  }

  virtual void process_event_end() {
    if (get_local_time_tm().tm_hour < close_time_hour) {
      go_quoting();
    } else {
      simple_liquidate();
    }
  }

  void go_quoting() {
    for (Dir dir:{BID, ASK}) {
      int32_t dir_amount = ((dir == BID)?(1):(-1));
      bool enough = (total_amount() == max_executed_amount * dir_amount);
      if (active_orders[dir].size() == 0) {
        if (!enough)
          add_order(best_price(dir), 1, dir, 0);
      } else if (active_orders[dir][0]->price != best_price(dir)) {
        delete_order(active_orders[dir][0]);
        add_order(best_price(dir), 1, dir, 0);
      }
    }
  }

};
```
 Теперь вы можете писать простейшие стратегии. Для дальнейшего обучения смотрите примеры и документацию.
