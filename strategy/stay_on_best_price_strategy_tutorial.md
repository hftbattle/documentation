#Примеры стратегий
* [Stay on each direction strategy](#stay_on_best_prices_strategy)

<a name="stay_on_best_prices_strategy"></a>

## Stay on best prices strategy

Реализуем следующую стратегию: будем поддерживать наши заявки на лучшей цене в обоих направлениях. 
Сначала научимся ставить заявку. Для этого предназначена функция [add_limit_order](../../api/ParticipantStrategy.md#add_limit_order), принимающая на вход цену заявки, количество лотов (объем заявки) и направление.

Так как мы хотим поддерживать наши заявки на лучшей цене, нам нужно уметь узнавать лучшую цену по направлению. Для этого есть функция [best_price](../../api/ContestBookInfo.md#best_price), принимающая на вход направление. 

Будем выполнять все действия внутри функции [trading_book_update](../../api/ParticipantStrategy.md#trading_book_update), когда нам приходит новый стак

```cpp
void trading_book_update(const OrderBook& order_book) override {
	for (Dir dir: {BID, ASK}) {
		add_limit_order(dir, best_price(dir), 1);
	}
}
```

Однако в тот момент, когда у нас вызывается функция [trading_book_update](../../api/ParticipantStrategy.md#trading_book_update), наши заявки, поставленные в прошлых вызовах этой функции, всё еще могут быть не исполнены. В итоге, у нас может скопиться огромное количество заявок и мы превысим лимит на количество заявок в день. К счаан:стью, у нас есть возможность посмотреть все наши активные заявки. Для этого используем структуру [trading_book_info
](../../api/ParticipantStrategy.md#trading_book_info
) типа [ContestBookInfo](../../api/ContestBookInfo.md), у которого есть метод [orders](../../api/ContestBookInfo.mв#orders), возвращающий ссылку на структуру типа [SecurityOrdersSnapshot](../../api/SecurityOrdersSnapshot.md#):

```cpp
SecurityOrdersSnapshot& our_orders = trading_book_info
.orders();
```

> Замечание 1: Определенная выше переменная `orders` содержит те заявки, которые мы уже отправили, но на которые еще не отправили запрос на удаление. Поэтому если для какой-то заявки будет вызван метод [delete_order](../../api/ParticipantStrategy.md#delete_order), то к следующему обновлению этой заявки в `orders` точно не будет (даже если она еще не успела удалиться). 

> Замечание 2: Обновление структуры [trading_book_info
.orders()](../../api/ContestBookInfo.md#orders) происходит только между апдейтами, внутри апдейта она не меняется.

Разделим активные заявки по направлениям, используя поле [orders_by_dir](../../api/SecurityOrdersSnapshot.md#orders_by_dir) класса [SecurityOrdersSnapshot](../../api/SecurityOrdersSnapshot.md#):

```cpp
const std::array<std::vector<OrderSnapshot>, 2>& orders_by_dir = &trading_book_info
.orders().orders_by_dir;
```

Будем ставить заявку, если не существует активной заявки по этому направлению:

```cpp
void trading_book_update(const OrderBook& order_book) override {
  for (Dir dir:{BID, ASK}) {
    if (active_orders_by_dir()[dir].size() == 0) {
      add_order(best_price(dir), 1, dir);
    }
  }
}
```

В такой реализации есть минус - если лучшая цена изменится, то мы не реагируем на это, что может привести к тому, что мы очень долго не будем торговать по одному направлению. Исправим это, и заодно перепишем стратегию для наглядности:

```cpp
void trading_book_update(const OrderBook& order_book) override {
    for (Dir dir : {BID, ASK}) {
      const Price price = trading_book_info.best_price(dir);
      auto our_orders = trading_book_info.orders();
      bool no_our_orders = (our_orders.active_orders_count(dir) == 0);
      bool our_order_on_best_price = !no_our_orders && our_orders.orders_by_dir[dir][0]->price == price;
      // если заявка стоит, но не на лучшей цене - то сначала удаляем ее
      if (!no_our_orders && !our_order_on_best_price) {
        delete_order(our_orders.orders_by_dir[dir][0]);
      }
      // нужна новая заявка на лучшей цене, если там ничего не стоит либо
      if (!our_order_on_best_price) {
        const Amount amount = 1;
        add_limit_order(dir, price, amount);
      }
    }
  }
```

 Теперь вы можете писать простейшие стратегии. Для дальнейшего обучения смотрите примеры и документацию. 