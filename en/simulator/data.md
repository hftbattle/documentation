## Data for simulation

Market data from one of the largest world exchanges is used to simulate the trading.

The data has been provided by exchange and is completely realistic in this regard.
At the same time, when you strategy starts "trading", we are going to influence on what is happening with the symbol.
One of the major assumptions for the work of the simulator is that an impact of the strategy on the overall instrument's behavior is insignificant.

Therefore two order book states are maintained in the simulator:

- **Real state** – what was really going on the market.
  The strategy has no affect on the real state.

- **Virtual state** – takes strategy actions into account.

The simulator uses the exchange trading time.
Trading session starts at {{ book["trading.begin_time"] }} and ends at {{ book["trading.end_time"] }}.
