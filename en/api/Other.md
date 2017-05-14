# Other

## Decimal

Path in Local Pack `include/base/decimal.h`

Decimal class is used for performing high precision calculations and for price representation (for convenience, it has Price alias).

You can also get double using get_double() method.

Note: in C++ you can't print Decimal using cout/cerr/printf, however, you can use our output streams.
In Python you can use print.

## JsonValue

Path in Local Pack `include/base/json.h`

JSON data format is described here: <https://en.wikipedia.org/wiki/JSON>.
In our implementation you can edit your strategy configuration in .json file.

To get the value from the Json use "as" method.

This class is only used in C++, in **Python** standard dict is used.

### Methods

| Name | Description |
| --- | --- |
| [as()](#as) | Getting a value from JsonValue. |

### Methods description

#### as() {#as}

Transforms json-value to a value of given type and returns it.
The usage example to get a string — `config["field_in_json"].as<std::string>()`.

You are able to set a default return value in brackets (it will be used if there's no such field in config):

- `config["field_in_json"].as<std::string>("default_string")`.
- `config["time"].as<Microseconds>(10us)`.

## Amount

Path in Local Pack `include/base/constants.h`

An alias for int32_t for better code readability.
This alias is only used in C++.

## Price

Path in Local Pack `include/base/constants.h`

An alias for Decimal class, you can read about Decimal class here: <https://docs.hftbattle.com/en/api/Other.html#decimal>.

## Id

Path in Local Pack `include/base/constants.h`

A numeric identifier, an alias for uint64_t.
This alias is only used in C++.

## Microseconds

Path in Local Pack `include/base/constants.h`

An alias for std::chrono::microseconds.
Read more information here: <http://en.cppreference.com/w/cpp/chrono/duration>.

This class is only used in C++, in **Python** standard int type is used.

## Logs

Path in Local Pack `include/base/log.h`

Logs are a convenient way to output information about strategy's behaviour.
They allow you to control the level of records detalization: if you want you can turn on the most detailed log level, or, vice versa, you can only log the most important information only.
Logs are also dumped automatically in a file.

Here is an example of using this method.
Let's output the middle price using SCREEN() output stream.

{% codetabs name="C++", type="c++" -%}
SCREEN() << order_book.middle_price();
{%- endcodetabs %}

## LogLevel

Path in Local Pack `include/base/log.h`

This enum represents the log level.
There is a corresponding output stream for each log level.
For example, for Error log level there is an ERROR() output stream.

Let's assume you want to choose the Error log level.
Then you need to set "log_level": "error" in your strategy's configuration file.
In this situation, SCREEN() and FATAL() will also work.

> Note 1: output streams ERROR() and below are dumped in a file only, while SCREEN() and FATAL() are also displayed on the screen.
>
> Note 2: FATAL() output stream terminates program.
> Use it with caution.
