### Logs

Mining and analysis of trading information is a significant part of strategy development.
Since the number of atomic market events can be extremely large (about {{ book["trading.daily_deals"] }} million), there is a need in extracting the most important details, features and strategy actions.

There are 3 files in deals and orders archive:
**orders.tsv** and **deals.tsv** — charts containing all the strategy orders and deals,
**logs.txt** containing custom output.
Here you can explore it:

- [Orders](#orders)
- [Deals](#deals)
- [Custom output](#custom_output)

#### Orders {#orders}

Here's the description of one row in **orders.tsv**:

| field | value | description |
| --- | --- | --- |
| action | add | add or del |
| time | 2016-11-01 09:15:00.223000 | action server time |
| symbol | {{ book["contest.instrument"] }} | instrument name |
| price | 8639 | order price |
| amount | 10 | order volume |
| executed_amount | 10 | number of executed lots |
| dir | BID | order [direction](/terms.md#bid_and_ask) |
| order_meaning | Quote | Quote for your [limit](/terms.md#limit_order) orders and Beat for [IOC](/terms.md#ioc_order) ones |
| order_type | Limit | [Limit](/terms.md#limit_order) / [IOC](/terms.md#ioc_order) |
| status | added | transaction status |
| reject_reason | "" | reason of rejection, if any |
| order_id | 320000001 | unique order identifier |

#### Deals {#deals}

Similarly one row in **deals.tsv**:

| field | value | description |
| --- | --- | --- |
| time | 2016-11-01 09:15:00.223000 | deal server time |
| symbol | {{ book["contest.instrument"] }} | instrument name |
| price | 8639 | deal price |
| amount | 10 | number of executed lots |
| dir | BID | your order [direction](/terms.md#bid_and_ask) in this deal |
| order_meaning | Quote | Quote for your [limit](/terms.md#limit_order) orders and Beat for [IOC](/terms.md#ioc_order) ones |
| our_order_status | active | status of our order |
| executed_amount | 10 | signed executed amount: negative for sold lots and positive for bought |
| approx_result | -127.5 | approximate result after a packet of simultaneous deals |
| saldo | -86390 | balance of **all** deals performed by the strategy |
| order_id_bid | 320000001 | bid order unique identifier |
| order_id_ask | 300000000 | ask order unique identifier |
| aggressor_side | BID | aggressor order [direction](/terms.md#bid_and_ask) |

#### Custom output {#custom_output}

You can use the following ways to display information from your strategy:

1. Output to `stdout` and `stderr`.
  It's recommended to use it for printing small amount of information (running parameters, config, etc.).
  Logs in stdout and stderr are **limited with {{ book["constraint.symbol"] }} symbols, {{ book["constraint.string"] }} lines and {{ book["constraint.symbol_in_string"] }} symbols per line**.
2. Logging to files with our log streams, e.g. `SCREEN() <<` (**C++ only**):

  ```c++
  SCREEN() << "This is my awesome strategy";
  ```

  It's recommended to use these [streams](/api/Other.md#logs) to log large amount of information.

  The total size of these 3 files is **limited with {{ book["constraint.logs_size"] }} MB** for one simulation.
