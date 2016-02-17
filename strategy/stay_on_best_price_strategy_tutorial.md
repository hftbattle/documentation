#Примеры стратегий
* [Stay on each direction strategy](#stay_on_best_prices_strategy)

<a name="stay_on_best_prices_strategy"></a>

## Stay on best prices strategy

Реализуем следующую стратегию: будем поддерживать наши заявки на лучшей цене в обоих направлениях. 
Сначала научимся ставить заявку. Для этого предназначена функция [add_limit_order](../../api/ParticipantStrategy.md#add_limit_order), принимающая на вход цену заявки, количество лотов (объем заявки) и направление.

Так как мы хотим поддерживать наши заявки на лучшей цене, нам нужно уметь узнавать лучшую цену по направлению. Для этого есть функция [best_price](../../api/ContestBookInfo.md#best_price), принимающая на вход направление. 

Будем выполнять все действия внутри функции [trading_book_update](../../api/ParticipantStrategy.md#trading_book_update), гарантируя тем самым, что биржевое событие полностью обработано:

```cpp
void trading_book_update(const OrderBook& order_book) override {
	for (Dir dir: {BID, ASK}) {
		add_limit_order(dir, best_price(dir), 1);
	}
}
```

Однако в тот момент, когда у нас вызывается функция [event_end_update](../../api/ParticipantStrategy.md#event_end_update), наши заявки, поставленные в прошлых вызовах этой функции, всё еще могут быть не реализованы. В итоге, у нас может скопиться огромное количество заявок и мы превысим лимит на количество заявок в день. К счастью, у нас есть возможность посмотреть все наши активные заявки. Для этого используем структуру [trading_book_info
](../../api/ParticipantStrategy.md#trading_book_info
) типа [ContestBookInfo](../../api/ContestBookInfo.md), у которого есть метод [active_orders](../../api/ContestBookInfo.md#active_orders), возвращающий ссылку на структуру типа [SecurityOrdersSnapshot](../../api/SecurityOrdersSnapshot.md#):

```cpp
SecurityOrdersSnapshot& active_orders = trading_book_info
.active_orders();
```

> Замечание 1: Определенная выше переменная `active_orders` содержит те заявки, которые мы уже отправили, но на которые еще не отправили запрос на удаление. Поэтому если для какой-то заявки будет вызван метод [delete_order](../../api/ParticipantStrategy.md#delete_order), то к следующему обновлению этой заявки в `active_orders` точно не будет (даже если она еще не успела удалиться). 

> Замечание 2: Обновление структуры [trading_book_info
.active_orders()](../../api/ContestBookInfo.md#active_orders) происходит только между апдейтами, внутри апдейта она не меняется.

Разделим активные заявки по направлениям, используя поле [orders_by_dir](../../api/SecurityOrdersSnapshot.md#orders_by_dir) класса [SecurityOrdersSnapshot](../../api/SecurityOrdersSnapshot.md#):

```cpp
const std::array<std::vector<OrderSnapshot>, 2>& active_orders_by_dir = &trading_book_info
.active_orders().orders_by_dir;
```

Для удобства в классе [ParticipantStrategy](../../api/ParticipantStrategy.md) уже реализован метод [active_orders_by_dir](../../api/ParticipantStrategy.md#active_orders_by_dir), выполняющий то же, что и конструкция выше.  
Будем ставить заявку, если не существует активной заявки по этому направлению:

```cpp
void event_end_update() override {
  for (Dir dir:{BID, ASK}) {
    if (active_orders_by_dir()[dir].size() == 0) {
      add_order(best_price(dir), 1, dir);
    }
  }
}
```

В такой реализации есть минус - если лучшая цена изменится, то мы не реагируем на это, что может привести к тому, что мы очень долго не будем торговать по одному направлению. Исправим это:

```cpp
void event_end_update() override {
  for (Dir dir:{BID, ASK}) {
    if (active_orders_by_dir()[dir].size() == 0) {
      add_order(best_price(dir), 1, dir);
    } else if (active_orders_by_dir()[dir][0]->price != best_price(dir)) {
      delete_order(active_orders_by_dir()[dir][0]);
      add_order(best_price(dir), 1, dir);
    }
  }
}
```

Теперь сосредоточимся на общей структуре стратегии.
Так как мы торгуем всегда внутри одного дня, то к его концу нам необходимо закрыть все позиции, то есть за день количество проданных и купленных активов должно быть одинаковым. Вообще говоря, если у вас к концу дня остались открытые позиции, то они будут автоматически закрыты [симулятором](../simulator/README.md) по рыночной стоимости в конце дня, и это напрямую повлияет на ваш результат. Поэтому хорошее закрытие в конце дня - важная часть стратегии.
В нашем примере мы реализуем самый простой вариант закрытия позиций - при наступлении фиксированного времени мы просто снимем все свои заявки и закроем все позиции по текущей лучшей цене. Вот как это выглядит:

```cpp
const int32_t close_time_hour = 23;
void event_end_update() override {
  if (get_server_time_as_tm().tm_hour < close_time_hour) {
    go_quoting();
  } else {
    simple_liquidate();
  }
}
void go_quoting() {
  const auto my_active_orders = trading_book_info
  .active_orders();
  for (Dir dir:{BID, ASK}) {
    if (my_active_orders.orders_by_dir[dir].size() == 0) {
      add_order(best_price(dir), 1, dir);
    } else if (my_active_orders.orders_by_dir[dir][0]->price != best_price(dir)) {
      delete_order(my_active_orders.orders_by_dir[dir][0]);
      add_order(best_price(dir), 1, dir);
    }
  }
}
void simple_liquidate() {
  for (Dir dir: {BID, ASK}) {
    for (auto order: trading_book_info
    .active_orders().orders_by_dir[dir]) {
      delete_order(order);
    }
  }
  const Dir dir_to_close = (total_amount() > 0 ? ASK : BID);
  if (total_amount() != 0) {
      add_ioc_order(best_price(opposite_dir(dir_to_close)),
                    abs(total_amount()),
                    dir_to_close);
  }
}
```
В этом коде при закрытии позиции мы используем функцию [add_ioc_order](../../api/ParticipantStrategy.md#add_ioc_order), которая работает следующим образом: она пытается реализовать данную заявку по обычным правилам, и если у неё не выходит это сделать, то она автоматически удаляется (ioc значит immediately or cancel).
Добавим последний штрих к нашей стратегии, а именно ограничим объём открытых позиций величиной `max_executed_amount = 10`. Если этого не сделать, то к концу дня может скопиться огромная поза, и фактически успех торгового дня будет зависеть от того, по какой стоимости эти позиции будут ликвидированы.
Итоговый код простейшей торговой стратегии:
```cpp
#include "strategy/participant_strategy_layer.h"
using namespace contest_platform;
// UserStrategy - основной класс, в котором пользователь реализует свою стратегию.
class UserStrategy : public ParticipantStrategy {
public:
  const int32_t close_time_hour = 23;
  const int32_t max_executed_amount = 10;
    
  void event_end_update() override {
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
                    dir_close);
    }
  }
};
```
 Теперь вы можете писать простейшие стратегии. Для дальнейшего обучения смотрите примеры и документацию.