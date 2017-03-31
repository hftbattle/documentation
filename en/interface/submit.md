##  Strategy submission

After you submitted a strategy for the contest system in a testing mode, its work simulation on some set of days starts. 
By default a single day is used.

In case your strategy depends on parameters you may define [parameter tuning](params.md) before submitting the strategy into the system.
If you do so, the system will built all possible combinations and will run a strategy copy for each parameter set on each of the days selected.
Total number of simulations run equals to `(Total number  of parameters set) x (number of days selected)`.

Submission with a lower number of simulations have higher priority in the contest system:


- Submissions with a number of simulations no more than {{ book["high.priority"] }} have higher priority.
- Submissions with a number of simulations no more than {{ book["medium.priority"] }} have an average priority.
- Others have a standard priority.

