## Simulator restrictions

The simulator has a number of limitations that make it work similar to the real trading:

- Maximum size of the open [position](/terms.md#position) is limited by {{ book["constraint.position"] }} lots to bid or ask.
- There is no starting capital — the limit for a maximum position serves this role.
- If your strategy losses reach {{ book["constraint.stop_loss"] }} on any day, the simulator closes the position and stops trading.
- Fee for each traded [lot](/terms.md#lot) is {{ book["constraint.fee"] }}.
  For example, if your strategy makes 2000 deals with 5000 as a total number of traded lots, your fee is {{ book["fee.equation"] }}.
- Round-trip equals to {{ book["constraint.round-trip"] }} microseconds.
  This is the time period from a moment you want some action (to add or to remove an order) to perform, to the moment when it is actually done.
- The strategy simulation can not run on a single day longer than {{ book["constraint.simulation_time"] }} minutes.
- At the end of the trading day the simulator closes your [position](/terms.md#position) automatically.
  In case your strategy makes large number of deals during a day that will be just another convenience.
- Orders count is limited by {{ book["constraint.max_orders"] }} per day.
