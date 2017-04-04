## Testing modes

There are three different testing modes, which can be used to develop your ideas:

- [training](#training_mode)
- [control](#control_mode)
- [weekly](#weekly_mode)

The [final testing](#final_test) starts in a month after the competition main phase is over.
Details on each mode are given below.

#### Training mode {#training_mode}

Training mode aims for a strategy testing, dealing with various issues, selecting optimal strategy parameters and so forth. Initially training dataset will contain {{ book["data.training_days"] }} days of September, October and November 2016 and then may be expanded during the competition.

While in the testing mode you are provided with comprehensive data on how your strategy performs during trading.
To learn more on how to analyze your strategy visit the [Result Analysis](analysis/README.md) section.
The mode allows you to optimize your strategy by tuning your initial parameter set. More on that in [Parameters tuning](params.md) section.
It also allows you to choose days for your strategy to run on.
For example, the best way to improve the results on a specific day is to run your strategy only on this day — this way you'll obtain the results as soon as possible.
Other participants will not be able to see your submissions in the training mode.
The daily total number of simulations is limited.
For more on limits visit your submissions page.

#### Control mode {#control_mode}

This mode is designed to run your strategy on a set of {{ book["data.control_days"] }} days. The set does not contain any of the training days and is not to be changed during the whole contest.
The mode aims for two purposes:

- Control submissions are used to calculate your current rating.
  Only strategies with a positive result are shown in the rating.
  All participants are able to see your result but not the strategy code.
- Control mode aims to be a testing set. You can use it to find out whether your strategy performs well on a new, out of sample, data. It also helps you to avoid overfitting.

Control mode has some restrictions:

- You are not allowed allowed to make more than {{ book["constraint.daily_control_submissions.en"] }} control submissions per day.
- You can not tune parameters values and all of the strategy default parameters values have to be specified.
- Just a very basic information about the strategy performance is provided to you for the rating days.

#### Weekly mode {#weekly_mode}

At the beginning of each week **one of your submissions** is run in a **weekly** testing on a new dataset.
To specify a submission you want to test **tick** (✔ icon) next to it in your submissions table.
Otherwise the system would automatically select the best one. System decision is based on the **control dataset** results.

> **Note 1**: None of your submissions will be used in the weekly testing unless you select it manually or make a control submission.
>
> **Note 2**: If you select a submission that tuned several sets of parameters in the testing mode the system will automatically choose the parameter set with the best result.

Therefore there are two different ratings, which are being maintained for all the participants: by control testing and by weekly testing.
Please note that those ratings have no effect on the **winners selection** and are just necessary to avoid overfitting and to evaluate results on the new available days.

#### Final testing {#final_test}

Strategy submission is stopped as soon as the main phase of the competition is over. Then the final testing begins. It will take **{{ book["final_testing_duration"] }} weeks**.
Each week we will publish your strategy results for the 5 most recent trading days.

**Final score** is the result on the final dataset consisting of **{{ book["data.final_days"] }} trading days** after the arena is frozen.
