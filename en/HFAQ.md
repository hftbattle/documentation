# HFAQ

Questions are categorized:

- [Organizational questions](#org)
- [Arena's interface](#interface)
- [Off-line development package](#local_pack)
- [Trading simulator](#simulator)
- [Simulator's API](#api)
- [Miscellaneous](#misc)

---

## Organizational questions {#org}

##### Q: Can a team participate in the competition?

A: Yes. To do this you need to create one account for the entire team, specifying the data of one of the participants.
Multiple logins are forbidden. Final versions will be checked for plagiarism, offenders will be excluded from the competition.

---

## Arena's interface {#interface}

##### Q: How to submit a strategy for the control testing?

A: To submit a strategy for the control testing, change the "Day set" switch on the main arena's page from "Training" to the "Control" value.
A participant is provided with {{ book["constraint.daily_control_submissions.en"] }} submissions in the control mode.

---

## Off-line development package {#local_pack}

##### Q: I can not run strategies from the off-line development package. What should I do?

A: Make sure your system has:

- python `2.7.x` or `3.x` (`python --version`)
- cmake `2.8.4` or above (`cmake --version`)
- C++ compiler:
  - for Linux — g++ compiler `5.0` or above (`g++ --version`)
  - for macOS — Apple LLVM version `6.0` or above, (`c++ --version`)
  - for Windows — TDM64-GCC `5.1.0` or above

More on that in [Setting dependences](/local_pack/requirements.md) section.

---

## Trading simulator {#simulator}

##### Q: Is there a lag between a moment when an order was sent and the moment when it was placed in the order book?

A: Time between the moment an order is sent and the moment it is accepted by the market is called *round-trip*.

The simulator has a *round-trip* set to {{ book["constraint.round-trip"] }} microseconds.
That means all your requests for the orders adding / removal happen with a little time lag as after if it is sent to the real market.

---

##### Q: I set *stop_loss* with a [set_stop_loss_result](api/ParticipantStrategy.md#set_stop_loss_result) method. Why did I get lower result?

A: A position is being closed in some time period. During that period the close price can be varied significantly and so your total result may vary (be less or more) than the one you set as *stop_loss*.

---

##### Q: How is it possible that my order is rejected by the system?

A: There could be a number of reasons for that:

1. It is possible that the order exceeds an allowed maximum position limit.
2. A price level is not within whe the allowed limits, there is no such a price for the security.
3. Number of lots is not a natural number.
4. The order could be matched with your another order in the opposite direction.
5. You are trying to set the order in the hidden part of the order book.

##### Q: The strategy position is being closed. What does it mean? {#close_position}

A: The trading simulator does not allow the strategy to set new orders and deletes the active ones. Once it is done, the trading simulator sets orders in a such way that the resulting position of the strategy equals to zero.

---

## Simulator's API {#api}

##### Q: How to know the best price for a direction?

A: The current best price can be known from the order book [trading_book](api/ParticipantStrategy.md#trading_book) with a [best_price](api/OrderBook.md#best_price) method:

{% codetabs name="C++", type="c++" -%}
Price best_price = trading_book().best_price(dir);
{%- language name="Python", type="py" -%}
best_price = strat.trading_book().best_price(dir)
{%- endcodetabs %}

---

## Miscellanous {#misc}

##### Q: How does the order book work?

A: When a new order X comes into the order book, it is placed into the order queue according to its direction and price.
Inside each price all orders are sorted by the time they were placed in the queue.
In case the best bid price is strictly less than the best ask price, the order book processing is stopped.
Otherwise the process of the orders execution starts.

Suppose the X order was a bid, i.e. an order to buy.
Then opposite asks are taken one by one to execute against the X, in order of the price increase or, inside each price — in order of the placement time increase. The execution results in a new deal with the price of the passive executed order (ask, in this case) with the volume of minimal of the two orders, involved in the deal.
The volume of the deal is subtracted from the volumes of the orders involved in the deal.
And so the process goes, until the best bid price is less than the best ask price.

##### Q: What types of orders are used?

A: Two types of orders are used for the contest: [limit order](terms.md#limit_order) (limit order) and [IOC](terms.md#ioc_order) (Immediate-Or-Cancel).
Consider what will happen if there is still some volume left after all processes are done for an order.

- if the order was limit, it will stay in the order book by the specified price.
- if the order was IOC, it will be immediately removed from the order book.

For example.
Let's say the order book has 3 orders to sell:

- volume of 10 on the price of 101, placed at 11:00
- volume of 20 on the price of 101, placed at 11:01
- volume of 10 on the price of 102, placed at 11:00

| Price| Volume (placement time) |
| --- | --- |
| 101 | 10 (11:00), 20 (11:01) |
| 102 | 10 (11:00) |

Let's say a trader places a bid X on some price with a volume of 35.
 Consider several options:

1. X — limit order on the price of 101.
  2 deals are going to happen, one with the volume of 10 and another with the volume of 20. The rest of the X stays in the order book on the price of 101.
2. X — of IOC type on the price of 101.
  2 deals are going to happen, with the rest of the X to be removed from the order book.
3. X — of IOC type on the price of 102.
  3 deals are going to happen with the volume of 10, 20 and 5 on the price of 101, 101 and 102 and the X will have nothing left.
