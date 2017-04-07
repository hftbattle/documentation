### Statistics

There's a vast interface for the strategy analysis on the submission page.
Basic sections of the page are:

- [Common results](#common_result)
- [Results by day](#results_by_day)
- [Statistics by day](#stats_by_day)

#### Common results {#common_result}

“Common results” table shows overall evaluation of the strategy or each parameter set if you are using parameter tuning.

The strategy is evaluated according to this formula:<a id="result_formula"></a>

```py
SCORE = AVG — (STD / 10.0) + min(0, MIN_INTRADAY_RESULT) / 5.0
```

- `AVG` — average strategy score calculated on all of the days (taking fee of {{ book["constraint.fee"] }} per lot into account).
- `STD` — [standard deviation]({{ book["std_deviation.en_url"] }}) of result on all sample days.
- `MIN_INTRADAY_RESULT` — the minimum result of the strategy that was reached at any given moment during a day (not necessarily at the close of the trading session), among all of the days.

Click “Details” in a table row to get particular values of these components.

#### Results by day {#results_by_day}

“Results by day” table shows results of the strategy or selected paramset by day providing some additional information:

- [charts](charts.md) of your strategy.
- [logs](logs.md) — link to the archive with files of your strategy's logs, order and deal lists.
- [viewer](viewer.md) — an opportunity to get the visualization of virtual trading, i.e. trading with your participation.
- stdout — standard output of the strategy and simulator.

#### Statistics by day {#stats_by_day}

“Statistics by day” table shows some more strategy properties for each day.

Columns:

- min_result / max_result — the minimum / maximum result of the strategy during a day.
- our_deals_count — the number of deals made by the strategy.
- our_deals_volume — the total volume of deals made by the strategy.
- result — the final result of the strategy at the end of the trading day.
- transactions_count — the number of transactions (order additions and deletions) made by the strategy.
- fee — the total fee of the strategy.
- work_time — the net time period of the simulation in seconds excluding preparation expenses.
