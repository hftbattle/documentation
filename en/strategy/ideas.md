## Ideas to implement

Several simple ideas that could help you develop the first successful strategy:

- study how a limitation for a maximum amount of the allowed [position] (/terms.md#position) affects the strategy result. 
- Try increasing drawdown limit of your strategy.
- Try placing order with larger volumes.
- If a considerable number of order has been canceled from the best price for a given direction it is possible that the price is going to move in this direction.
- Place order on several price levels, not only on the best prices.

Let's have a closer look at some of these ideas and how to implement them.

<!-- TODO(asalikhov): rewrite this with new security -->
Let's run strategy that [stays on the best prices](examples.md#stay_on_best_price), on the day of 1 September and have a look at the trading chart:

<img src="{{ book["gitbook.img"] }}/charts_1_september.png" title="the 1st of September chart" align="center">

On the chart above:

- the green line is our intraday profit,
- blue line - our [position](/terms.md#position).
- we added the black chart (more on how to add charts [here](/interface/analysis/charts.md)) of the mid_price - 176500’ variable. Where mid_price = 1/2 (best_ask + best_bid) .
176500  normalization done for the price and the current result to be in about the same range values.

No matter whether we go long or short, positions are closed after a long time, clearly.

Let's have a closer look at this particular intraday period:

<img src="{{ book["gitbook.img"] }}/charts_1_september_part.png" title="a part of the chart as of September the 1st" align="center">

On this chart it is evident that our position was negative during the very large period of time.
Such a behaviour is a bad sign for a HFT-strategy (as well as long positive position holding).

Second issue here - net position has a value of about 100..
In case the lot price rises out result declines quite fast.

> In case when we just stay on the best price it’s possible to try limiting the maximum position size to 1, 5 or 10.
> To do that use the method[set_max_total_amount](/api/ParticipantStrategy.md#set_max_total_amount).
> Check it out and see how much the current result is affected!

