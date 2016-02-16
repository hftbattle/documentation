# API

## Классы
Здесь содержится описание программного интерфейса основных классов симулятора.

* [ContestBookInfo](ContestBookInfo.md)
* [Deal](Deal.md)
* [Order](Order.md)
* [OrderBook](OrderBook.md)
* [ParticipantStrategy](ParticipantStrategy.md)
* [Quote](Quote.md)
* [SecurityOrdersSnapshot](SecurityOrdersSnapshot.md)

## Типы данных

- *Dir* - enum, отвечающий за направление заявки:
```cpp
enum Dir : uint8_t {
    BUY = 0,
    BID = 0,
    SELL = 1,
    ASK = 1,
    UNKNOWN = 3
};
```
Пример использования:
```cpp
for (Dir dir : { BID, ASK}) {
    Dir opposite_dir = opposite_dir(dir);  // BID -> ASK, ASK -> BID
    int32_t dir_sign = dir_sign(dir);  // BID = 1, ASK = -1.
    // ...
}
```
- *Price* - класс, хранящий вещественные числа с точностью 7 знаков после запятой и отвечающий за цену заявок.
- *Amount* - объем
- 