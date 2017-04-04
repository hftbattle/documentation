# API

## Классы

Здесь содержится описание программного интерфейса основных классов симулятора:

- [CommonEnums](CommonEnums.md) даёт представление о понятиях *Dir*, *OrderStatus*, *ChartYAxisType* и о функциях, обеспечивающих удобство: *opposite_dir* и *dir_sign*.
- [Deal](Deal.md) — описание произошедшей сделки.
- [ExecutionReport](ExecutionReport.md) — отчёт о сделке с участием вашей заявки.
- [Order](Order.md) — описание биржевой заявки.
- [OrderBook](OrderBook.md) — это агрегатор всех заявок по конкретному [инструменту](/terms.md#instrument).
- [Other](Other.md) содержит информацию о классах *Decimal*, *Json*, описывает псевдонимы *Amount*, *Price*, *Id*, *Microseconds* и потоки вывода *SCREEN()* и *ERROR()*.
  В **Python** будет полезен лишь класс *Decimal* (*Price*).
- [ParticipantStrategy](ParticipantStrategy.md) — класс-обёртка для стратегий участников для взаимодействия с торговым симулятором.
- [Quote](Quote.md) — описание ценового уровня в стакане.
- [QuotesHolder](QuotesHolder.md) — класс, позволяющий итерироваться по всем квотам одного из направлений (**только для C++**). В **Python** метод [all_quotes](OrderBook.md#all_quotes) возвращает генератор, поэтому отдельный класс не нужен.
- [SecurityOrdersSnapshot](SecurityOrdersSnapshot.md) — текущие заявки стратегии.
