### Statistics

There's a vast interface of strategy analysis on the page of submission. 
Basic sections of the page are:

- [Common results](#common_result)
- [Results by day](#results_by_day)
- [Statisctics by day](#stats_by_day)

#### Common results {#common_result}

“Common results” table shows overall evaluation of the strategy or if you're grid searching parameters of each parameters set.

Strategy evaluation is carried out according to this expression:<a id="result_formula"></a>

```py
SCORE = AVG - (STD / 10.0) + min(0, MIN_INTRADAY_RESULT) / 5.0
```

- `AVG` — average result of your strategy on all sample days (taking fee of {{ book["constraint.comission"] }} per lot into account).
- `STD` — [standard deviation]({{ book["std_deviation.en_url"] }}) of result on all sample days.
- `MIN_INTRADAY_RESULT` — minimal result, reached by your strategy any moment (not always the end) of any sample day.

To get exact values of these components click “Details” in table rows.

#### Results by day {#results_by_day}

“Results by day” table shows results of the whole strategy or selected paramset by day providing some additional information:

- [charts](charts.md) of your strategy.
- [logs](logs.md) — link to the archive with files of your strategy's logs, order and deal lists.
- [viewer](viewer.md) — opportunity to get visualization of virtual trading, i.e. trading with your participation.
- stdout — standard output of the strategy and simulator.

#### Statisctics by day {#stats_by_day}

“Statistics by day” table shows some more strategy properties for each day.

Columns:

- min_result / max_result — minimal / maximal result, reached by your strategy during the day.
- our_deals_count — number of deals made by strategy.
- our_deals_volume — sum deals made by strategy volume.
- result — final result of the strategy in the end of trading day.
- transactions_count — number of transactions (adds or deletions of orders), made by strategy.
- fee — sum fee of the strategy.
- work_time — pure time of simulation in seconds, excluding prepare expenses.
