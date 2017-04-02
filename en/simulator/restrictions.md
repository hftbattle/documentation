## Simulator restrictions

The simulator has a number of limitations that make it work similar to the real trading:

- Maximum size of the open [position](/terms.md#position) is limited by {{ book["constraint.position"] }} lots to bid or ask.
- There is no starting capital - the limit for a possible position serves this role.
- If your strategy hits the result of {{ book["constraint.stop_loss"] }} on any day, the simulator closes the position and stops trading.
- Commission for each traded [lot](/terms.md#lot) is {{ book["constraint.comission"] }}.

For example, if you strategy makes 2000 deals with a 500 as a total number of traded lots, your commission is {{ book["comission.equation"] }}.
- Round-trip is {{ book["constraint.round-trip"] }} microseconds. This is the time period from a moment you want some action (to add or to remove an order) to perform, to the moment when it is actually done.
- The strategy simulation can not run on a single day longer than {{ book["constraint.simulation_time"] }} minutes.
- At the end of the trading day the simulator closes your [position](/terms.md#position) automatically.
In case your strategy makes large number of deals during a day that will be just another convenience.
