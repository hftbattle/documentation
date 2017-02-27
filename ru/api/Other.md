# Other

## Decimal

Путь в Local Pack `include/base/decimal.h`

Класс для хранения чисел с плавающей точкой с большой точностью.
В том числе используется для описания цен (для этого есть более удобный синоним — Price).
С объектами этого класса можно проделывать арифметические операции сложения, вычитания, умножения и деления и использовать их в арифметических операциях с числовыми типами.

Есть возможность получать значение в виде double с помощью метода get_double().

Внимание: напрямую выводить Decimal в cout/cerr/printf нельзя, однако можно воспользоваться SCREEN()

## Json

Путь в Local Pack `include/base/json.h`

Читайте про этот формат данных здесь: <https://ru.wikipedia.org/wiki/JSON>.
В нашей реализации вы можете изменять JSON вашей стратегии в JSON файле вашей стратегии.

Для получения значения переменной в стратегии нужно воспользоваться методом "as".

В **Python**, это обычный dict.

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

## Amount

Путь в Local Pack `include/base/constants.h`

Синоним int32_t для повышения читабельности кода.

## Price

Путь в Local Pack `include/base/constants.h`

Синоним класса Decimal, читайте подробнее про класс Decimal.

## Id

Путь в Local Pack `include/base/constants.h`

Числовой идентификатор, синоним uint64_t.

## Microseconds

Путь в Local Pack `include/base/constants.h`

Синоним std::chrono::microseconds.
Подробнее читайте здесь: <http://en.cppreference.com/w/cpp/chrono/duration>.

В **Python**, это просто int.

## kMinStopLossResult

Путь в Local Pack `include/base/constants.h`

Минимальное значение результата, которого может достичь ваша стратегия.

## kMaximumMaxExecutedAmount

Путь в Local Pack `include/base/constants.h`

Ваша стратегия не может сделать больше чем kMaximumMaxExecutedAmount заявок.

## SCREEN

Путь в Local Pack `include/base/log.h`

Один из потоков вывода.
Данные в этом потоке выводятся, если установлен уровень логирования SCREEN и ниже.

## ERROR

Путь в Local Pack `include/base/log.h`

Один из потоков вывода.
Данные в этом потоке выводятся, если установлен любой уровень логирования.
После этого программа прекращает свою работу.
