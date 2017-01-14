#API

##Классы
Здесь содержится описание программного интерфейса основных классов симулятора:

* [ContestBookInfo](ContestBookInfo.md)
* [Deal](Deal.md)
* [Order](Order.md)
* [OrderBook](OrderBook.md)
* [ParticipantStrategy](ParticipantStrategy.md)
* [Quote](Quote.md)
* [SecurityOrdersSnapshot](SecurityOrdersSnapshot.md)

##Типы данных
* [Dir](#dir)
* [Price](#price)
* [Amount](#amount)

Далее опишем каждый тип подробнее.

<a id="dir"></a>
####Dir
Enum, отвечающий за направление заявки:
```c++
enum Dir : uint8_t {
    BUY = 0,
    BID = 0,
    SELL = 1,
    ASK = 1,
    UNKNOWN = 3
};
```
Пример:
```c++
for (const auto& dir : { BID, ASK } ) {
    Dir opposite_dir = opposite_dir(dir);  // BID -> ASK, ASK -> BID
    int32_t dir_sign = dir_sign(dir);  // BID = 1, ASK = -1.
    // ...
}
```

<a id="price"></a>
####Price
Класс, хранящий вещественные числа с точностью 7 знаков после запятой и отвечающий за цену заявок.
> Замечание: цена в симуляторе традиционно хранится в пунктах, причем
```
1 пункт = 0.5$
```
Все операции с ценами производятся в пунктах, в то время как результат стратегии считается в долларах.

Рассмотрим пример, получающий лучшую и вторую после лучшей цены по каждому направлению:
```c++
void trading_book_update(const OrderBook& order_book) override {
    for (const auto& dir : { BID, ASK } ) {
        Price best_price = order_book.best_price(dir);
        Price min_step = trading_book_info.min_step();
        Price second_price = best_price - dir_sign(dir) * min_step;
        std::cout << dir << ": "
                  << "best = " << best_price << ", "
                  << "second = " << second_price << std::endl;
    }
}
```

<a id="Amount"></a>
####Amount
Объем заявок, выраженный в количестве лотов. Это псевдоним для *int32_t*.

Рассмотрим пример, получающий объем на лучшей и второй после лучшей цены по каждому направлению:
```c++
for (const auto& dir : { BID, ASK } ) {
    Amount best_volume = order_book.get_volume_by_index(dir, 0);  // == order_book.best_volume(dir)
    Amount second_volume = order_book.get_volume_by_index(dir, 1);
    std::cout << "best_volume: " << best_volume << ", "
              << "second_volume: " << second_volume << std::endl;
    // ...
}
```
