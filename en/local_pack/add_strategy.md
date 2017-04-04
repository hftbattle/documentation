## Strategy adding

Once you decide to try more ideas and develop more strategies, using the one strategy — one file approach may become necessary.
Let’s have a look at how to add new strategies in the off-line development package.

- [C++](#cpp)
- [Python](#python)

### Creating a new C++ strategy {#cpp}

#### 1. Create a strategy folder

- In the `strategies` folder create a copy of the `sample_strategy` folder
- Choose a name of your strategy.

Let’s call it *strategy_name*.

- Rename the new folder with json and cpp files inside with the *strategy_name*, *strategy_name.json*, *strategy_name.cpp* respectively.

#### 2. Register your strategy

Once the files are copied you need to edit *cpp* file.

**Register** your new strategy with the following command at the end of the cpp file:

```c++
REGISTER_CONTEST_STRATEGY(UserStrategy, strategy_name)
```

Let’s say your folder, .json-file and the strategy are called *best_strategy_ever*.
You will therefore need to add the following line at the end of the file *best_strategy_ever.cpp*:

```c++
REGISTER_CONTEST_STRATEGY(UserStrategy, best_strategy_ever)
```

This is necessary due to specifics of the simulator’s dynamic linkage process of the strategies.

#### 3. Update CMake configuration

- Those of you who work in cli run the following script *build.py*:
  ```bash
  ./build.py
  ```
- Those working in CLion, go to *Tools > CMake > Reload CMake Project*.

### Creating a new Python strategy {#python}

Unfortunately, there is currently no way to create new folders for Python strategies.
Please write your strategy in the
 `strategies/python_strategy/python_strategy.py` file.
You may find out [here](run_strategy.md) how to run the strategy.

<!-- TODO(asalikhov): it may be allowed to write in another files -->
<!-- To create a new strategy in Python:

#### 1. Create a strategy folder

- In the `strategies` folder create a `python_strategy` folder copy
- Choose a new for your strategy.
  Let’s call it *strategy_name*.
- Name a new folder, json and cpp files *strategy_name*, *strategy_name.json*, *strategy_name.cpp*.

**Note**: do not rename any other files.

Your strategy code should be in the *python_strategy.py* file.

Next you may [run](run_strategy.md) the new strategy. -->
