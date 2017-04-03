# API

## Classes

This section contains the description of the main simulator API classes:

- [CommonEnums](CommonEnums.md) gives you the definition of *Dir*, *OrderStatus*, *ChartYAxisType* and of the methods *opposite_dir* and *dir_sign*.
- [Deal](Deal.md) - description of a deal.
- [ExecutionReport](ExecutionReport.md) - order execution report.
- [Order](Order.md) - exchange order description.
- [OrderBook](OrderBook.md) - the aggregator of all orders of specific [instrument](/terms.md#instrument).
- [Other](Other.md) contains the information about *Decimal* and *Json* classes, describes *Amount*, *Price*, *Id* and *Microseconds* aliases and also *SCREEN()* and *ERROR()* output streams.
  Also class *Decimal* (*Price*) would be useful in **Python**.
- [ParticipantStrategy](ParticipantStrategy.md) - participants` strategies wrapper class for interaction with the trading simulator.
- [Quote](Quote.md) - description of a quote in the order book.
- [QuotesHolder](QuotesHolder.md) - container that allows you to iterate through all the quotes by given direction (**only for C++**).
  In **Python** the [all_quotes](OrderBook.md#all_quotes) method returns generator, making this class unnecessary.
- [SecurityOrdersSnapshot](SecurityOrdersSnapshot.md) - strategy`s current orders.
