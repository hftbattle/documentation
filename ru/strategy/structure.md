## Общая структура стратегий

Торговая стратегия получает на вход информацию об изменениях, произошедших с [торговым инструментом](/terms.md#instrument) на [бирже](/terms.md#exchange).
В зависимости от этой информации стратегия совершает какие-то действия.
Это может быть постановка новых [заявок](/terms.md#order) и/или запрос на удаление старых.

В данной главе мы обсудим:

- [Структуру стратегий на языке C++](#cpp)
- [Структуру стратегий на языке Python](#python)

А также методы, подлежащие реализации:

- [Обновление стакана](#book_update)
- [Обновление сделок](#deals_update)
- [Отчёт об исполнении вашей заявки](#execution_report_update)

### Структура стратегий на языке C++ {#cpp}

Рассмотрим класс-шаблон **UserStrategy**, предназначенный для написания стратегий.
Он наследуется от класса-интерфейса [ParticipantStrategy](/api/ParticipantStrategy.md), где объявлено 3 виртуальных функции, которые вы можете реализовать в своей стратегии:

- [trading_book_update](/api/ParticipantStrategy.md#trading_book_update)
- [trading_deals_update](/api/ParticipantStrategy.md#trading_deals_update)
- [execution_report_update](/api/ParticipantStrategy.md#execution_report_update)

Сам класс выглядит следующим образом:

```c++
#include "participant_strategy.h"
#include <vector>

using namespace hftbattle;

class UserStrategy : public ParticipantStrategy {
public:
  // В конструктор стратегии участника передается файл конфигурации.
  // В файл конфигурации из веб-интерфейса можно передать параметры стратегии.
  explicit UserStrategy(const JsonValue& config) { }

  // Вызывается симулятором при получении нового стакана торгового инструмента.
  void trading_book_update(const OrderBook& order_book) override { }

  // Вызывается симулятором при получении новых сделок торгового инструмента.
  void trading_deals_update(std::vector<Deal>&& deals) override { }

  // Вызывается симулятором при получении отчёта о сделке с участием вашей заявки.
  void execution_report_update(const ExecutionReport& execution_report) override { }
};

REGISTER_CONTEST_STRATEGY(UserStrategy, user_strategy)
```

Далее вам нужно реализовать эти методы.

### Структура стратегий на языке Python {#python}

Пустая стратегия выглядит следующим образом.
Здесь также есть 3 функции, которые вы можете реализовать, а также функция *init*, которая позволяет получать информацию из файла конфигурации или из веб-интерфейса.

<!-- TODO(asalikhov): check if there is a speed improvement -->
<!-- Обратите внимание: **если вы не собираетесь переопределять какие-то из этих функций, мы советуем удалить их, так как это даст возможность немного ускорить ваш код**:
 -->
```py
# -*- coding: utf-8 -*-

from py_defs import *
from py_defs import Decimal as Price
from common_enums import *


# Вызывается симулятором при получении новых сделок торгового инструмента.
def trading_deals_update(strat, deals):
    pass


# Вызывается симулятором при получении нового стакана торгового инструмента.
def trading_book_update(strat, order_book):
    pass


# Вызывается симулятором при получении отчёта о сделке с участием вашей заявки.
def execution_report_update(strat, execution_report):
    pass


# В конструктор стратегии участника передается файл конфигурации.
# В файл конфигурации из веб-интерфейса можно передать параметры стратегии.
def init(strat, config):
    pass
```

#### Обновление стакана {#book_update}

При торговле на бирже постоянно происходят какие-то события в [стакане](/terms.md#order_book) инструмента.
Для информирования стратегии о произошедших изменениях используется функция [trading_book_update](/api/ParticipantStrategy.md#trading_book_update).
Она принимает на вход ссылку на [OrderBook](/api/OrderBook.md), который является новой версией торгового стакана.

> Стакан, пришедший в метод *trading_book_update* совпадает со стаканом, который вы получите, вызвав метод [trading_book](/api/ParticipantStrategy.md).
  Этот метод может быть использован для получения стакана при обновлениях других типов.

#### Обновление сделок {#deals_update}

Помимо изменений стакана полезной информацией являются совершённые сделки.
Для того, чтобы получать и обрабатывать эти изменения, применяется функция [trading_deals_update](/api/ParticipantStrategy.md#trading_deals_update), которая принимает в качестве аргумента вектор элементов класса [Deal](/api/Deal.md).
Каждый элемент этого вектора хранит в себе информацию об одной совершённой сделке.
Заметьте, что по каждой совершенной сделке информация о ней приходит **ровно один раз**.
Однако, одна заявка **может участвовать в нескольких сделках**.

#### Отчёт об исполнении вашей заявки {#execution_report_update}

Функция [execution_report_update](/api/ParticipantStrategy.md#execution_report_update) предназначена для того, чтобы получать информацию о сделках с участием наших заявок.
На каждую такую сделку приходит отдельный *execution_report_update*.
