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

- *Dir* - направление заявки. 
- Принимает значения: *BID* (или *BUY*) и *ASK* (или *SELL*). Пример:
```cpp
for (Dir dir : { BID, ASK}) {
    Dir opposite_dir = opposite_dir(dir);
    // ...
}
```
- *Price* - цена
- *Amount* - объем
- 