### Logs 

Collection and analysis of trading information is significant part of developing a strategy.
As market events count could be very big(about {{ book["trading.daily_deals"] }} million), there is a need to extract most important details, features and strategy actions.

Deals and orders archive contains 3 files: orders.tsv and deals.tsv - tables containing all the strategy's orders and deals and logs.txt containing custom output. Here you can explore it:

- [Orders](#orders)
- [Deals](#deals)
- [Custom output](#custom_output)

#### Orders {#orders}

Here's the description of one row in **orders.tsv**:

| field | value | description |
| --- | --- | --- |
| action | add | add or del |
| time | 2016-11-01 09:15:00.223000 | Exchange action time |
| symbol | {{ book["contest.instrument"] }} | intrument name |
| price | 8639 | order price |
| amount | 10 | order's lot amount |
| executed_amount | 10 | amount of executed lots |
| dir | BID | order [direction](/terms.md#bid_and_ask) |
| order_meaning | Quote | Quote for your [limit](/terms.md#limit_order) orders and Beat for [IOC](/terms.md#ioc_order) ones |
| order_type | Limit | [Limit](/terms.md#limit_order) / [IOC](/terms.md#ioc_order) |
| status | added | Transaction status |
| reject_reason | "" | Reason of rejection, if one |
| order_id | 320000001 | unique indentifier of order |

#### Deals {#deals}

Similarly one row in **deals.tsv**:

| field | value | description |
| --- | --- | --- |
| time | 2016-11-01 09:15:00.223000 | Exchange deal time |
| symbol | {{ book["contest.instrument"] }} | intrument name |
| price | 8639 | deal price |
| amount | 10 | deal's lot amount |
| dir | BID | [direction](/terms.md#bid_and_ask) of your order in this deal |
| order_meaning | Quote | Quote for your [limit](/terms.md#limit_order) orders and Beat for [IOC](/terms.md#ioc_order) ones |
| our_order_status | active | status of our order |
| executed_amount | 10 | signed executed amount: negative for selled lots and positive for buyed |
| approx_result | -127.5 | approximate result after a pack of simultaneous deals |
| saldo | -86390 | balance of **all** the deals performed by the strategy |
| order_id_bid | 320000001 | unique indentifier of bid order |
| order_id_ask | 300000000 | unique indentifier of ask order |
| aggressor_side | BID | [direction](/terms.md#bid_and_ask) of the aggressor order |

#### Custom output {#custom_output}

You can use the following ways to display information from your strategy:

1. Output to `stdout` and `stderr`.
  It's advised to use for printing little information (running parameters, config, etc.)
  Logs in stdout and stderr are **limited with {{ book["constraint.symbol"] }} symbols, {{ book["constraint.string"] }} lines and {{ book["constraint.symbol_in_string"] }} symbols per line**.
2. Logging to files with our log streams, e.g. `SCREEN() <<` (**C++ only**):

  ```c++
  SCREEN() << "This is my awesome strategy";
  ```

  It's advised to use these [streams](/api/Other.md#logs) to log much information.

  Total amount of these 3 files is **limited with {{ book["constraint.logs_size"] }} megabytes** for one simulation.
