## General strategy structure

Trading strategy gets updates of [instrument](/terms.md#instrument) on the [exxhange](/terms.md#exchange) as an input.
According to this information it may perform some actions: adding new [orders](/terms.md#order) and/or requesting deletions of old ones.

Here we are going to discuss:

- [C++ strategy structure](#cpp)
- [Python strategy structure](#python)

And methods you have to implement:

- [Order book update](#book_update)
- [Deals update](#deals_update)
- [Order execution report](#execution_report_update)

### C++ strategy structure {#cpp}

**UserStrategy** class is designed to develop strategies.
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
  // You can add running parameters to it from the web interface.
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

Further you have to implement these methods.

### Python strategy structure {#python}

Empty strategy looks the following.
There are 3 functions you may implement and *init* function to get information from the configuration file.
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
# You can add running parameters to it from the web interface.
def init(strat, config):
    pass
```

#### Order book update {#book_update}

Some changes in [order book](/terms.md#order_book) are consistently happening during trading session.
[trading_book_update](/api/ParticipantStrategy.md#trading_book_update) method is used to notify strategy about such changes.
It accepts reference to [OrderBook](/api/OrderBook.md) as an input.

> Order book applied to the input of *trading_book_update* is the same as one you can get by calling [trading_book](/api/ParticipantStrategy.md) method.
> This method is useful to get order book during other updates (deals and execution report).

#### Deals update {#deals_update}

Deals made on exchange are also useful to analyze.
[trading_deals_update](/api/ParticipantStrategy.md#trading_deals_update) method is used to notify strategy about these changes.
It accepts vector of [Deals](/api/Deal.md) as an input.
Each element of this vector contains information on one completed deal.
This information is sent **exactly once** for each deal.
However, one order **may be referred in multiple deals**.

#### Order execution report {#execution_report_update}

[execution_report_update](/api/ParticipantStrategy.md#execution_report_update) is used to get strategy's order execution information.
For each deal containing strategy's order *execution_report_update* is called.
