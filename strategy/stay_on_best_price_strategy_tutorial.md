##Как написать простую стратегию

Реализуем следующую стратегию: будем поддерживать наши [заявки](../terms.md#order) на лучшей цене в обоих направлениях. 

Сначала научимся ставить заявку. Для этого предназначена функция 
```c++
bool add_limit_order(Dir dir, Price price, Amount amount);
```

Функция [add_limit_order](../api/ParticipantStrategy.md#add_limit_order) выставляет нашу [лимитную заявку](../terms.md#limit_order), где:
- *dir* - направление (BID = 0 - покупка, ASK = 1 - продажа),
- *price* - цена, по которой заявка будет выставлена,
- *amount* - размер заявки.

Будем выставлять нашу заявку внутри функции [trading_book_update](../api/ParticipantStrategy.md#trading_book_update), когда нам приходит новый стакан `const OrderBook& order_book`. Для определения лучшей цены используем метод [best_price](../api/OrderBook.md#best_price) класса [OrderBook](../api/OrderBook.md):
```c++
void trading_book_update(const OrderBook& order_book) override {
  for (Dir dir: {BID, ASK}) {
    const Price best_price = order_book.best_price(dir);
	const Amount amount = 1;
    add_limit_order(dir, best_price, amount);
  }
}
```

В тот момент, когда у нас вызывается функция [trading_book_update](../api/ParticipantStrategy.md#trading_book_update), наши заявки, поставленные в прошлых вызовах этой функции, всё еще могут быть не исполнены. В итоге, у нас может скопиться огромное количество заявок и мы превысим лимит на количество заявок в день. К счастью, у нас есть возможность посмотреть все наши активные заявки: 
```c++
auto our_orders = trading_book_info.orders();
```

Здесь мы используем структуру-аггрегатор информации о торговом стакане - [trading_book_info
](../api/ParticipantStrategy.md#trading_book_info
) типа [ContestBookInfo](../api/ContestBookInfo.md), у которой есть метод [orders](../api/ContestBookInfo.mв#orders), возвращающий ссылку на структуру типа [SecurityOrdersSnapshot](../api/SecurityOrdersSnapshot.md#).


>> Замечание 1: Определенная выше переменная `orders` содержит те заявки, которые мы уже отправили, но на которые еще не отправили запрос на удаление. Поэтому если для какой-то заявки будет вызван метод [delete_order](../api/ParticipantStrategy.md#delete_order), то к следующему обновлению этой заявки в `orders` точно не будет, даже если в реальность она еще не успела удалиться. 

> Замечание 2: Обновление структуры [trading_book_info.orders()](../api/ContestBookInfo.md#orders) происходит только между апдейтами, внутри апдейта она не меняется.

> Замечание 3: Лучшую цену на направлению можно узнать, вызвав метод 
[trading_book_info.best_price(Dir dir)](../api/ContestBookInfo.md#best_price). Такой вызов работает быстрее, чем соответствующий метод [OrderBook.best_price(Dir dir)](../api/OrderBook.md#best_price). 

Будем ставить заявку, если не существует активной заявки по этому направлению. Используем для этого метод [active_orders_count(Dir dir)](../api/SecurityOrdersSnapshot.md#active_orders_count) класса [SecurityOrdersSnapshot](../api/SecurityOrdersSnapshot.md), возвращающий количество наших активных заявок по направлению:

```c++
void trading_book_update(const OrderBook& order_book) override {
  auto our_orders = trading_book_info.orders();
  for (Dir dir: {BID, ASK}) {
    if (our_orders.active_orders_count(dir) == 0) {
      const Price best_price = trading_book_info.best_price(dir);
      const Amount amount = 1;
      add_limit_order(dir, best_price, amount);
    }
  }
}
```

В такой реализации есть минус – если лучшая цена изменится, то мы не реагируем на это. Это может привести к тому, что мы долго не будем торговать по одному из направлений. Чтобы получить цену нашей активной заявки используем поле [orders_by_dir](../api/SecurityOrdersSnapshot.md#orders_by_dir) класса [SecurityOrdersSnapshot](../api/SecurityOrdersSnapshot.md#):

```c++
void trading_book_update(const OrderBook& order_book) override {
  auto our_orders = trading_book_info.orders();
  for (Dir dir: {BID, ASK}) {
    const Price best_price = trading_book_info.best_price(dir);
    const Amount amount = 1;
    if (our_orders.active_orders_count(dir) == 0) {
      add_limit_order(dir, best_price, amount);
    } else {  // есть хотя бы одна наша активная заявка
      auto first_order = our_orders.orders_by_dir[dir][0];
      bool our_order_on_best_price = first_order->price == best_price;
      if (!our_order_on_best_price) {  // наша заявка стоит, но не на текущей лучшей цене
        delete_order(first_order);
        add_limit_order(dir, best_price, amount);
      }
    }
  }
}
```

Теперь вы можете писать простейшие стратегии.