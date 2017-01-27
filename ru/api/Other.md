# Other

## Decimal

Путь в Local Pack `include/base/decimal.h`

Класс для хранения чисел с плавающей точкой с большой точностью.
В том числе используется для описания цен (для этого есть более удобный синоним — Price).
С объектами этого класса можно проделывать арифметические операции сложения, вычитания, умножения и деления и использовать их в арифметических операциях с числовыми типами.

Есть возможность получать значение в виде double с помощью метода get_double().

Внимание: напрямую выводить Decimal в cout/cerr/printf нельзя, однако можно воспользоваться SCREEN()

```c++
class Decimal;
```

## Json

Путь в Local Pack `include/base/json.h`

Читайте про этот формат данных здесь: <https://ru.wikipedia.org/wiki/JSON>.
В нашей реализации вы можете изменять JSON вашей стратегии в JSON файле вашей стратегии.

Для получения значения переменной в стратегии нужно воспользоваться методом "as".

```c++
class JsonView;
```

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

```c++
template<typename T>
```

## Amount

Путь в Local Pack `include/base/constants.h`

Синоним int32_t для повышения читабельности кода.

```c++
using Amount = int32_t;
```

## Price

Путь в Local Pack `include/base/constants.h`

Синоним класса Decimal, читайте подробнее про класс Decimal.

```c++
using Price = Decimal;
```

## Id

Путь в Local Pack `include/base/constants.h`

Числовой идентификатор, синоним uint64_t.

```c++
using Id = uint64_t;
```

## Microseconds

Путь в Local Pack `include/base/constants.h`

Синоним std::chrono::microseconds.
Подробнее читайте здесь: <http://en.cppreference.com/w/cpp/chrono/duration>.

```c++
using Microseconds = std::chrono::microseconds;
```

## kMinStopLossResult

Путь в Local Pack `include/base/constants.h`

Минимальное значение результата, которого может достичь ваша стратегия.

```c++
static constexpr Decimal kMinStopLossResult = Decimal(-50000.0);
```

## kMaximumMaxExecutedAmount

Путь в Local Pack `include/base/constants.h`

Ваша стратегия не может сделать больше чем kMaximumMaxExecutedAmount заявок.

```c++
static constexpr Amount kMaximumMaxExecutedAmount = 50 * 75;
```

## SCREEN

Путь в Local Pack `include/base/log.h`

Один из потоков вывода.
Данные в этом потоке выводятся, если установлен уровень логгирования SCREEN и ниже.

```c++
#define SCREEN() PRIVATE_LOG(getCurrentLoggerId(), hftbattle::LogLevel::Screen)
```

## ERROR

Путь в Local Pack `include/base/log.h`

Один из потоков вывода.
Данные в этом потоке выводятся, если установлен любой уровень логгирования.
После этого программа прекращает свою работу.

```c++
#define ERROR() PRIVATE_LOG(getCurrentLoggerId(), hftbattle::LogLevel::Error)
```
