##  Strategy submission

After submitting your strategy in the testing mode its trading simulation on some set of days starts.

In case your strategy depends on parameters you may define [parameter tuning](params.md) before submitting the strategy into the system.
If you do so, the system will build all possible combinations and will run a strategy for each parameter set on each of the days selected.
Total number of simulations equals to `(Total number of parameter sets) x (number of days selected)`.

Submission with a lower number of simulations have higher priority in the contest system:

- Submissions with a number of simulations no more than {{ book["high.priority"] }} have high priority.
- Submissions with a number of simulations no more than {{ book["medium.priority"] }} have an average priority.
- Others have a standard priority.
