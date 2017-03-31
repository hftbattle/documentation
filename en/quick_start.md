# Quick start

Key features of the contest system are given below.
This page is not a comprehensive description but rather a quick start overview!

- [Contest order](#org)
- [Trading simulator](#simulator)
- [Off-line development package](#local_pack)
- [Using Viewer to analyze order book](#viewer)
- [Quick Tips](#quick_tips)

## Contest order {#org}

There are three submission types in the competition::

- There is no practical limit on the number of [training submissions](interface/modes.md#training_mode) - use them to develop and to improve your strategies.
- [Control](interface/modes.md#control_mode) - your strategies will be run on a new set of days, which is not available in the testing mode. This aims for you to know whether your strategy performs well on out of sample data.
 Besides, control submissions are taken into account for a [overall rating].({{ book["contest.arena.leaderboard.url"] }}).
  You may upload up to {{ book["constraint.daily_control_submissions.ru.gen"] }} control submissions per a single day
- [Weekly](interface/modes.md#weekly_mode) - each week the strategy you selected (or the best among control runs) will be executed on a new dataset. 

[Final testing](interface/modes.md#final_test) will be carried out after the main competition phase is over.

## Trading simulator {#simulator}

- Four limitations approximate the trading simulator to the reality::
  - Maximum buy \ sell size for an open [position] is limited to (terms.md#position) lots.
    This limit serves confines a starting capital.
  - If your strategy hits negative {{ book["constraint.stop_loss"] }} the low on any day, the simulator closes the position and runs no more
- Commission = {{ book["constraint.comission"] }} for each  lot traded.
- Round-trip equals to {{ book["constraint.round-trip"] }} microseconds — the time period from a moment you want some action (to add or to remove an order) to perform until it is actually done.
- At the end of the day a [position](terms.md#position) is closed.
This happens automatically for your convenience.
  More on this [here](HFAQ.md#close_position).
- Your strategy deals with three types of the events:  [order book refresh](api/ParticipantStrategy.md#trading_book_update), [deals refresh](api/ParticipantStrategy.md#trading_deals_update) and [your order execution report](api/ParticipantStrategy.md#execution_report_update).
 So, when your order is executed, the first event is a refresh of the deals, then a refresh of the order book and the last one is the order book altered event.

Take particular attention to the strategy logs, they contain data on your orders and deals.


See [Trading simulator] for more information (simulator/README.md).

## Off-line development package {#local_pack}

- On [GitHub] we have published ({{ book["contest.local_pack.url"] }}) off-line development package:

  ```bash
  git clone https://github.com/hftbattle/hftbattle.git
  ```
- To work with the package you need a compiler g++ 5.0 or above, CMake 2.8.4 or above, too.

- If the strategy is written on C++, the single cpp file contains everything, if it is written on Python, its code is in the *python_strategy.py*.
  For each strategy has a corresponding json file.
- To collect all the *strategies* in the folder, it is necessary to run python-script *build.py*, that is in there root of the repository:

  ```bash
  ./build.py
  ```
- To execute the strategy chosen by you, run the python-script run.py,  giving to it the path for the configuration file of the strategy to run, as a parameter:

  ```bash
  ./run.py user_strategy
  ```
- When developing new strategies in the off-line development package, change the strategy registration line in the ccp-file:
  Note, *best_strategy_ever* — is the folder, cpp and json files name:

  ```c++
  REGISTER_CONTEST_STRATEGY(UserStrategy, best_strategy_ever)
  ```

More on that in the [off-line development kit](local_pack/README.md).

## Using Viewer to analyze the order {#viewer}

- On [{{ book["viewer.name"] }}]({{ book["viewer.url"] }}) you get market trading visualization tool.
 All events are shown atomically, deals are combined in a packages, as they were sent by the market.
- Please note, that the order book in the Viewer really is the market's online order book that is a list of pairs <price, total amount by the price> currently existing on the market.
You could not affect it.
- You can also have Viewer for your strategy. 
To do that click the button “Write data to Viewer” on the day of your interest.
  After a certain amount of time you will get an opportunity to see your strategy actions in the Viewer..
- Click at any point on the chart and you get into the Viewer. You deals and orders around that point in time will be shown..
<!-- TODO(asalikhov): this will be added: -->
- You may browse on your events as well as on anyone's. (see Settings tab).

More on that in the [Viewer] section (interface/analysis/viewer.md).

## Quick Tips {#quick_tips}

- In case you would like to find out how your strategy performs on a particular day, run it just for the selected date, this way you get results much faster.
- To get most out of your idea try [tuning parameters](interface/params.md).

See more that in the [corresponding section](interface/README.md).
