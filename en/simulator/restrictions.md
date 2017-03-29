## Limitations  of simulator

Simulator has a number of limitations that aim to make it similar to the real trading:

- Maximum size of the open [position](/terms.md#position) is limited {{ book["constraint.position"] }} of lots to buy or sell.
- There is no starting capital per se, the limit for a possible position serves this role.
- If your strategy hits {{ book["constraint.stop_loss"] }} the low on any day, the simulator closes the position and runs no more.
- Commission for each [lot](/terms.md#lot) traded is {{ book["constraint.comission"] }}.

So, if you strategy make, let’s say, 2000 deals with a 500 as a total number of lots traded, your commision is {{ book["comission.equation"] }}.
- Round-trip of {{ book["constraint.round-trip"] }} microseconds - the time period from a moment you want some action (to add or to remove an order) to perform, until it is actually done.
- The strategy simulation can not run on a single day longer than {{ book["constraint.simulation_time"] }} minutes.
- At the end of the trading day the simulator closes your [position](/terms.md#position) automatically.
In case your strategy makes large number of deals during a day that will be just another convenience.

