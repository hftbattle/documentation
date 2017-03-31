## Testing modes

There are three different testing modes, which can be used to develop your ideas:

- [training](#training_mode)
- [control](#control_mode)
- [weekly](#weekly_mode)

In a month after the competition main phase is over [a final testing] starts (#final_test).
Details on each mode are given below.

#### Training mode {#training_mode}

Training mode aims for a strategy testing, dealing with various issues, selecting optimal strategy parameters and so forth.It will have {{ book["data.training_days"] }} days of September, October and November 2016. This dataset is to be extended as the competition goes.

While in the testing mode you have got comprehensive data on how your strategy conducts during trading.
To learn more on how to analyze your strategy see the [Result Analysis] section (analysis/README.md).
The mode allows you to optimize your strategy by tuning your initial parameters set. More on the see [Parameters tuning] section (params.md).
It also allows you to choose days for your strategy to run on.
For example, the best way to improve the results on a specific day is to run your strategy only on this day only - this way you'll obtain the results as soon as possible and save a lot of your time.
Other participants will not be able to see your submissions in the training mode.
The daily total number of submissions is limited.
For more on limits see your page for submissions.

#### Control mode {#control_mode}

This mode is designed for your strategy to run on a set of {{ book["data.control_days"] }} days. The set neither contains any of the training days, nor is to be changed during the whole contest.
The mode aims for the two purposes:

- Control submissions are used to calculate your current rating.
- Only strategies with a positive result get into the rating.
- All participants are able to see your result but not the strategy code.
- Control mode aims to be a testing set. You can use it to find out whether your strategy performs well on a new, out of sample, data. It also makes it possible for you to avoid overfitting.

Control mode has some limitations:

- It allows for no more than {{ book["constraint.daily_control_submissions.ru.ins"] }} per a day**.
- You can not fit parameters values and you have to specify all of your strategy parameters values.
- Just very basic information on your strategy performance for the rating days is provided to you.

#### Weekly mode {#weekly_mode}

At the beginning of each week **one of your strategies** is run in a **weekly** testing on a new dataset.
To specify the strategy you want to test **tick** (✔ icon) next to it in your submissions table.
In case you did not, the system would automatically select the best strategy. System decision is based on the **control set** results.

> **Note 1:**:None of your strategies will be used in the weekly testing unless you select it manually or send any of your strategies for a weekly testing.
> **Note 2:**: If you select a strategy that tuned several sets of parameters on the testing mode the system will automatically choose the parameter set with the best result.

Therefore, there are two different ratings, which are being maintained for all the participants: by control set and by weekly testing.
Please pay attention, those ratings have no effect on the **winners selections** and just necessary to monitor overfitting process and to evaluate results on the new available days.

#### Final testing {#final_test}

Strategy submission is stopped as soon as the main phase of the competition is over. Then the final testing begins. It will **take {{ book["final_testing_duration"] }} weeks**.
Each week we will publish your strategies results for the 5 most recent trading days.

**Final score** is the result on the dataset consisting of **{{ book["data.final_days"] }} trading days**, after the arena is frozen.
