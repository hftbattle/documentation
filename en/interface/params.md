## Parameters fitting

- [Reason to fit parameters](#intro)
- [Pass parameters into the strategy](#to_strategy)
- [Read parameters from the strategy](#from_strategy)

#### Reason to fit parameters {#intro}

Finding an optimal value for a numeric parameter using historical data is a commonly used approach for strategy development.
Parameters fitting interface can be helpful for that.

#### Passing parameters into a strategy {#to_strategy}

The strategy submission page allows you to pass into a strategy dataset of numerical atomic parameters for fitting.
To do that at the bottom of the page find a field *Parameter Name* and enter the name of the parameter and its value in the next field.


Integer and real values with a dot as a separator are allowed.

Let's have a look at how parameters are set:

1. Constant:

  <img src="{{ book["gitbook.img"] }}/param_const_double_set.png" title="Constant real value">
2. Comma separated list of values:

  <img src="{{ book["gitbook.img"] }}/param_list_int_set.png" title="List of integer values">
3. Range of values with an increment: (start, end, step):

  <img src="{{ book["gitbook.img"] }}/param_range_int_set.png" title="List of values from 0 up to 10 with increment of 2">

Populate data values fields and click on *Add* button.

The parameters set by 1 and 2 methods are passed as is. The parameters set as range of values will be automatically expanded in the parameters list and displayed in the parameters table.

Parameters fitting interface allows you to add several various parameters. You may also turn the fitting off by setting a default value (*default value*):

- Setting up a number of several datasets.
 2 values for a *double_param* and 4 values for *int_param* combines gives you 4 * 2 = 8 combinations.
In case a parameter gets fitting you may skip setting its default value, obviously.  

  <img src="{{ book["gitbook.img"] }}/param_double_int_combo.png" title="Setting up a number of several datasets.">

- Turn off *double_param* parameter fitting example, default value = 0:
  
The *int_param* parameter’s default value is ignored since it has parameter fitting turned on:

  <img src="{{ book["gitbook.img"] }}/param_double_int_turn_off_double.png" title="Turn off parameter fitting example double_param">

#### Reading parameters from the strategy {#from_strategy}

All defined parameters of the strategy, can be read from the configuration file that is passed into the user's strategy constructor *UserStrategy*.
In **C++** all the parameters of the configuration file can be converted to the type required by  `as<param_type> method (deafult_value)`.
In case no parameter value is defined by you, the default value is used deafult_value.
With **Python** you can use the standard type conversion.

{% codetabs name="C++", type="c++" -%}
#include "participant_strategy.h"

using namespace hftbattle;

namespace {

class UserStrategy : public ParticipantStrategy {
public:
  // Strategy parameters to fit.
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
    // ! For a date-time parameter a time period symbol must be set (literal).
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
    # Strategy parameters to fit.
    # Parameter type is defined in the JSON, only.
    # Numeric values should not be put in quotes.
    global int_param, double_param, time_param
    int_param = config["int_param"]
    # Read real value. If it is not in the JSON file, it will be set as a default value 3.14.
    double_param = config.get("double_param", 3.14)
    # a number is stored here, too.
    time_param = config["time_param"]
{%- endcodetabs %}

