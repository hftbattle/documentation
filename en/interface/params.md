## Parameters tuning

- [Reason to tune parameters](#intro)
- [Pass parameters into the strategy](#to_strategy)
- [Read parameters from the strategy](#from_strategy)

#### Reason to tune parameters {#intro}

Finding an optimal value of a numeric parameter using backtesting on a historical data is a commonly used approach for strategy development.
Parameters tuning interface can be helpful for that.

#### Passing parameters into a strategy {#to_strategy}

The strategy submission page allows you to pass a set of atomic parameters into the strategy for tuning.
To do that you may find a field *Parameter Name* at the bottom of the page and enter the name of the parameter with its value in the next field.

Only integer and dot separated real values are allowed.

Let's have a look at how parameters are set:

1. Constant:

  <img src="{{ book["gitbook.img"] }}/param_const_double_set.png" title="Constant real value">
2. Comma separated list of values:

  <img src="{{ book["gitbook.img"] }}/param_list_int_set.png" title="List of integer values">
3. Range of values with an increment: (start, end, step):

  <img src="{{ book["gitbook.img"] }}/param_range_int_set.png" title="List of values from 0 up to 10 with increment of 2">

Populate data values fields and click on *Add* button.

The parameters set by 1 and 2 methods are passed as is. The parameters set as a range of values will be automatically expanded in the parameters list and displayed in the parameters table.

Parameters tuning interface allows you to add several parameters. You may also turn the tuning off for a specific parameter by setting a *default value*:

- Setting up a number of several parameter sets.
  2 values for a *double_param* and 4 values for *int_param* combines gives you 4 * 2 = 8 combinations.
  In case a parameter gets tuning you may skip setting its default value, obviously.

  <img src="{{ book["gitbook.img"] }}/param_double_int_combo.png" title="Setting up a number of several datasets.">
- Turn off *double_param* parameter tuning example, default value = 0:
  The *int_param* parameter’s default value is ignored since it has parameter tuning turned on:

  <img src="{{ book["gitbook.img"] }}/param_double_int_turn_off_double.png" title="Turn off parameter tuning example double_param">

#### Reading parameters from the strategy {#from_strategy}

All defined parameters of the strategy can be read from the configuration file passed into the user's strategy constructor *UserStrategy*.
In **C++** all the parameters of the configuration file can be converted to the type required by `as<param_type>(default_value)` method.
In case no parameter value is defined by you, the default_value is used instead.
In **Python** you can use the standard type conversion.

{% codetabs name="C++", type="c++" -%}
#include "participant_strategy.h"

using namespace hftbattle;

namespace {

class UserStrategy : public ParticipantStrategy {
public:
  // Strategy parameters to tune.
  int int_param;
  double double_param;
  Microseconds time_param;

  explicit UserStrategy(const JsonValue& config) {
    // Read integer value that must be specified in the JSON file.
    int_param = config["int_param"].as<int>();
    // Read real value
    // If it is not in the JSON file, it will be set as a default value 3.14.
    double_param = config["double_param"].as<double>(3.14);
    // Read a date-time parameter.
    // ! A time period symbol (literal) must be set for a date-time parameter when specifying default value in as<>() method.
    // possible time period literals:
    // h — hours,
    // min — minutes,
    // s — seconds,
    // ms — milliseconds,
    // us — microseconds.
    time_param = config["time_param"].as<Microseconds>(3s);
  }
};

}  // namespace

REGISTER_CONTEST_STRATEGY(UserStrategy, user_strategy)
{%- language name="Python", type="py" -%}
# -*- coding: utf-8 -*-

from py_defs import *
from py_defs import Decimal as Price
from common_enums import *


# A participant strategy constructor takes a configuration file as an argument.
# Strategy parameters can be passed in the configuration file from web interface.
def init(strat, config):
    # Strategy parameters to tune.
    # Parameter type is defined in the JSON, only.
    # Numeric values should not be put in quotes.
    global int_param, double_param, time_param
    int_param = config["int_param"]
    # Read real value. If it is not in the JSON file, it will be set as a default value 3.14.
    double_param = config.get("double_param", 3.14)
    # a number is stored here, too.
    time_param = config["time_param"]
{%- endcodetabs %}
