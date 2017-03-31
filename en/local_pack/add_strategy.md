## Adding a strategy

Once you decide to try more ideas and develop more strategies, one strategy - one file approach may become difficult to maintain. Let’s talk how to add new strategies in the off-line development package.

- [C++](#cpp)
- [Python](#python)

### Creating a new C++ strategy {#cpp}

To develop a new C++ strategy:

#### 1. Create a strategy folder

- In the `strategies` folder create a copy of the `sample_strategy` folder
- Choose a name of your strategy.

Let’s call it *strategy_name*.

- Rename the new folder, json and cpp files in it with the *strategy_name*, *strategy_name.json*, *strategy_name.cpp*.

#### 2. Register your strategy

Once the files are copied you need to edit *cpp* file a little bit.

**Register** your new strategy with the following command at the end of the cpp file:

```c++
REGISTER_CONTEST_STRATEGY(UserStrategy, strategy_name)
```

Let’s say your folder, .json-file and the strategy are called *best_strategy_ever*. You will therefore need to add the following line at the end of the file *best_strategy_ever.cpp*:

```c++
REGISTER_CONTEST_STRATEGY(UserStrategy, best_strategy_ever)
```

This is necessary due to specifics of the simulator’s dynamic linking process of the strategies.

#### 3. Restart CMake

- Those of you who work in cli run the following script *build.py*:
  ```bash
  ./build.py
  ```
- Those working with CLion, go to *Tools > CMake > Reload CMake Project*.

### Creating a new Python strategy {#python}

Currently there is no way to create new folders for `Python` strategies, unfortunately.
Please write your strategy in the
 `strategies/python_strategy/python_strategy.py` file.
How to run the strategy you may find out [here](run_strategy.md).

<!-- TODO(asalikhov): it may be allowed to write in another files -->
<!-- To create a new strategy in Python:

#### 1. Создайте папку стратегии

#### 1. Create a strategy folder

- In the `strategies` folder create a `python_strategy` folder copy
- Choose a new for your strategy.
  Let’s call it *strategy_name*.
- Name a new folder, json and cpp files *strategy_name*, *strategy_name.json*, *strategy_name.cpp*.

**Note**: do not rename any other files.

Your strategy code should be in the *python_strategy.py* file.

Next you may [run](run_strategy.md) the new strategy. -->
