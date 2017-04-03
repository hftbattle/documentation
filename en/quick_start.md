# Quick start

Core features of the contest system are given below.
This page is not a comprehensive description but rather a quick start overview!

- [Contest structure](#org)
- [Trading simulator](#simulator)
- [Off-line development package](#local_pack)
- [Using Viewer to analyze order book](#viewer)
- [Quick Tips](#quick_tips)

## Contest structure {#org}

There are three submission types in the competition:

- There is no practical limit on the number of [training submissions](interface/modes.md#training_mode) - use them to develop and to improve your strategies.
- [Control mode](interface/modes.md#control_mode) - your strategies will be run on a new set of days, which are not available in the testing mode. This helps you to determine whether your strategy performs well on out of sample data.
  Besides, control submissions are taken into account for [overall rating]({{ book["contest.arena.leaderboard.url"] }}).
  You may upload up to {{ book["constraint.daily_control_submissions.en"] }} control submissions per day.
- [Weekly testing](interface/modes.md#weekly_mode) - each week the selected strategy (or the best one among control runs) will be run on a new dataset.

[Final testing](interface/modes.md#final_test) will be carried out after the main competition phase is over.

## Trading simulator {#simulator}

- Four restrictions approximate the trading simulator to the reality:
  - Maximum bid / ask size for an open [position](terms.md#position) is limited to {{ book["constraint.position"] }} lots.
    This limit replaces the initial capital limit.
  - If your strategy losses reach {{ book["constraint.stop_loss"] }}, the simulator liquidates the position and stops trading.
- Fee = {{ book["constraint.comission"] }} per each executed lot.
- Round-trip equals to {{ book["constraint.round-trip"] }} microseconds. This is the time period from a moment you request some action (to add or to remove an order) to the moment when it is actually processed.
- At the end of the trading session the [position](terms.md#position) is liquidated.
  It happens automatically for your convenience.
  More on this [here](HFAQ.md#close_position).
- Your strategy deals with three types of events: [order book update](api/ParticipantStrategy.md#trading_book_update), [deals update](api/ParticipantStrategy.md#trading_deals_update) and [your order execution report](api/ParticipantStrategy.md#execution_report_update).
  So, when your order is executed, the first event is the order execution report and the next one is the deals update.

Take particular attention to the strategy logs as they contain data on your orders and deals.

See [Trading simulator](simulator/README.md) for more information.

## Off-line development package {#local_pack}

- We have published the off-line development package on [GitHub]({{ book["contest.local_pack.url"] }}):

  ```bash
  git clone https://github.com/hftbattle/hftbattle.git
  ```
- To work with the package you need a compiler g++ 5.0 or above, CMake 2.8.4 or above, too.

- If the strategy is written in C++, the single cpp file contains everything. If it is written in Python, its code is in the *python_strategy.py*.
  Each strategy has a corresponding json file.
- To build everything in the *strategies* folder it is necessary to run python script *build.py* located in the repository root to build all the strategies in the folder:

  ```bash
  ./build.py
  ```
- To run the strategy, run the python script *run.py* with the strategy name as a parameter to execute the chosen strategy:

  ```bash
  ./run.py user_strategy
  ```
- If you develop new strategies in the off-line development package, change the strategy registration line in the cpp-file.
  Note that *best_strategy_ever* is the folder, cpp and json files name:

  ```c++
  REGISTER_CONTEST_STRATEGY(UserStrategy, best_strategy_ever)
  ```

More on that in the [off-line development kit](local_pack/README.md).

## Using Viewer to analyze the order book {#viewer}

- On [{{ book["viewer.name"] }}]({{ book["viewer.url"] }}) you may get a trading visualization tool.
  All events are shown atomically, deals are combined in packets, as they were sent by the exchange.
- Please note, that the order book in Viewer is actually the market online order book that is a list of pairs <price, total amount by the price> currently existing on the market.
  You are not able to affect it.
- You can also see your strategy transactions in Viewer.
  Click the button “Write data to Viewer” on the particular day.
  After a certain amount of time you will get an opportunity to see your strategy actions in Viewer.
- You may browse through all events or through your strategy events only. (see Settings tab).
- Click at any point on the chart to get into Viewer. Order book state around the time moment corresponding to the chart point will be displayed.

More on that in the [Viewer](interface/analysis/viewer.md) section.

## Quick Tips {#quick_tips}

- In case you would like to find out how your strategy performs on a particular day, you may run it just for the selected date. This way you get results much faster.
- To get most out of your idea try [parameters tuning](interface/params.md).

More on the participant interface in the [corresponding section](interface/README.md).
