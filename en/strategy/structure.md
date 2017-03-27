## General strategy structure

Trading strategy get's changes of [instrument](/terms.md#instrument) on the [exhange](/terms.md#exchange) as an input.
According to this information it may perform some actions.
It may be adding new [orders](/terms.md#order) and/or request deleting old ones.

Here we are going to discuss:

- [C++ strategy structure](#cpp)
- [Python strategy structure](#python)

And methods you have to implement:

- [Order book update](#book_update)
- [Deals update](#deals_update)
- [Report on order execution](#execution_report_update)

### C++ strategy structure {#cpp}

**UserStrategy** class is designed to write strategies.
It's inherited from [ParticipantStrategy](/api/ParticipantStrategy.md) class with 3 virtual functions declared.
You may implement them:

- [trading_book_update](/api/ParticipantStrategy.md#trading_book_update)
- [trading_deals_update](/api/ParticipantStrategy.md#trading_deals_update)
- [execution_report_update](/api/ParticipantStrategy.md#execution_report_update)

The class is the following:

```c++
#include "participant_strategy.h"
#include <vector>

using namespace hftbattle;

namespace {

class UserStrategy : public ParticipantStrategy {
public:
  // Config file is passed to constructor.
  // You can add running parameters to it from web interface.
  explicit UserStrategy(const JsonValue& config) { }

  // Is called after getting new order book of the trading instrument.
  void trading_book_update(const OrderBook& order_book) override { }

  // Is called after getting new deals on the trading instrument.
  void trading_deals_update(std::vector<Deal>&& deals) override { }

  // Is called after getting strategy's order execution report.
  void execution_report_update(const ExecutionReport& execution_report) override { }
};

}  // namespace

REGISTER_CONTEST_STRATEGY(UserStrategy, user_strategy)
```

Further you have to implement this methods.

### Python strategy structure {#python}

Empty strategy looks the following.
There are 3 fucntions you may implement and *init* function to get information from the configuration file.
```py
# -*- coding: utf-8 -*-

from py_defs import *
from py_defs import Decimal as Price
from common_enums import *


# Is called after getting new deals on the trading instrument.
def trading_deals_update(strat, deals):
    pass


# Is called after getting new order book of the trading instrument.
def trading_book_update(strat, order_book):
    pass


# Is called after getting strategy's order execution report.
def execution_report_update(strat, execution_report):
    pass


# Config file is passed to this function.
# You can add running parameters to it from web interface.
def init(strat, config):
    pass
```

#### Order book update {#book_update}

Some changes in [order book](/terms.md#order_book) are consistently happening during trading on exchange.
[trading_book_update](/api/ParticipantStrategy.md#trading_book_update) method is used to notify strategy about such changes.
It accepts reference to [OrderBook](/api/OrderBook.md) as an input.

> Order book applied to the input of *trading_book_update* is the same with one you can get by calling [trading_book](/api/ParticipantStrategy.md).
> This method is useful to get order book during other types of updates.

#### Deals update {#deals_update}

Deals made on exchage are also useful to analyze.
[trading_deals_update](/api/ParticipantStrategy.md#trading_deals_update) method is used for these changes.
It accepts std::vector of [Deal](/api/Deal.md) as an input.
Each element of this vector contains information on one completed deal.
This information is sent **exactly one time** for each deal.
However, one order **may be refered in multiple deals**.

#### Report on order execution {#execution_report_update}

[execution_report_update](/api/ParticipantStrategy.md#execution_report_update) is used to get strategy's order's execution information.
For each deal containing strategy's order *execution_report_update* is called.
