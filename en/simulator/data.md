## Trading data

To simulate the trading one of the largest world exchange data is used.

The data has been provided by one of the largest exchanges and is completely real in this regard.

At the same time, when you strategy begins "trading", we are going to influence on what is happening with the symbol. One of the major assumptions for the work of the simulator, is that an impact of the strategy on the overall symbol's behavior is insignificant.

In the simulator therefore two states of the symbol are maintained.


-a real state – what really was going on on the market. The strategy has no affect whatsoever on the real state.

virtual state – the one that takes into account the strategy's behavior. 

The simulator uses trading time of the exchange, trading session starts at  {{ book["trading.begin_time"] }} ends at {{ book["trading.end_time"] }}.

