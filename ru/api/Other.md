# Other

## Decimal

Путь в Local Pack `include/base/decimal.h`

Класс для хранения чисел с плавающей точкой с большой точностью.
В том числе используется для описания цен (для этого есть более удобный синоним — Price).
С объектами этого класса можно проделывать арифметические операции сложения, вычитания, умножения и деления и использовать их в арифметических операциях с числовыми типами.

Есть возможность получать значение в виде double с помощью метода get_double().

Внимание: напрямую выводить Decimal в cout/cerr/printf нельзя, однако можно воспользоваться SCREEN()

{% codetabs name="C++", type="c++" -%}
class Decimal;
{%- language name="Python", type="py" -%}
cdef class Decimal
{%- endcodetabs %}

## Json

Путь в Local Pack `include/base/json.h`

Читайте про этот формат данных здесь: <https://ru.wikipedia.org/wiki/JSON>.
В нашей реализации вы можете изменять JSON вашей стратегии в JSON файле вашей стратегии.

Для получения значения переменной в стратегии нужно воспользоваться методом "as".

{% codetabs name="C++", type="c++" -%}
class JsonView;
{%- endcodetabs %}

### Методы

| Имя | Описание |
| --- | --- |
| [as()](#as) | Получение значения из Json. |

### Описание методов

#### as() {#as}

Преобразует json-значение в значение заданного типа и возвращает его.
Пример использования для получения объекта типа string — `config["имя_поля_в_json"].as<std::string>()`.

Есть возможность задавать значение по умолчанию в скобках (оно будет использовано, если в JSON нет такого поля):

- `config["имя_поля_в_json"].as<string>("строка_по_умолчанию")`.
- `config["time"].as<Microseconds>(Microseconds(10))`

{% codetabs name="C++", type="c++" -%}
template<typename T>
{%- endcodetabs %}

## Amount

Путь в Local Pack `include/base/constants.h`

Синоним int32_t для повышения читабельности кода.

{% codetabs name="C++", type="c++" -%}
using Amount = int32_t;
{%- endcodetabs %}

## Price

Путь в Local Pack `include/base/constants.h`

Синоним класса Decimal, читайте подробнее про класс Decimal.

{% codetabs name="C++", type="c++" -%}
using Price = Decimal;
{%- endcodetabs %}

## Id

Путь в Local Pack `include/base/constants.h`

Числовой идентификатор, синоним uint64_t.

{% codetabs name="C++", type="c++" -%}
using Id = uint64_t;
{%- endcodetabs %}

## Microseconds

Путь в Local Pack `include/base/constants.h`

Синоним std::chrono::microseconds.
Подробнее читайте здесь: <http://en.cppreference.com/w/cpp/chrono/duration>.

{% codetabs name="C++", type="c++" -%}
using Microseconds = std::chrono::microseconds;
{%- endcodetabs %}

## kMinStopLossResult

Путь в Local Pack `include/base/constants.h`

Минимальное значение результата, которого может достичь ваша стратегия.

{% codetabs name="C++", type="c++" -%}
static constexpr Decimal kMinStopLossResult = Decimal(-50000.0);
{%- endcodetabs %}

## kMaximumMaxExecutedAmount

Путь в Local Pack `include/base/constants.h`

Ваша стратегия не может сделать больше чем kMaximumMaxExecutedAmount заявок.

{% codetabs name="C++", type="c++" -%}
static constexpr Amount kMaximumMaxExecutedAmount = 50 * 75;
{%- endcodetabs %}

## SCREEN

Путь в Local Pack `include/base/log.h`

Один из потоков вывода.
Данные в этом потоке выводятся, если установлен уровень логгирования SCREEN и ниже.

{% codetabs name="C++", type="c++" -%}
#define SCREEN() PRIVATE_LOG(getCurrentLoggerId(), hftbattle::LogLevel::Screen)
{%- endcodetabs %}

## ERROR

Путь в Local Pack `include/base/log.h`

Один из потоков вывода.
Данные в этом потоке выводятся, если установлен любой уровень логгирования.
После этого программа прекращает свою работу.

{% codetabs name="C++", type="c++" -%}
#define ERROR() PRIVATE_LOG(getCurrentLoggerId(), hftbattle::LogLevel::Error)
{%- endcodetabs %}
