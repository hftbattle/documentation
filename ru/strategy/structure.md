## Общая структура стратегий

Торговая стратегия получает на вход информацию об изменениях, произошедших с [торговым инструментом](/terms.md#instrument) на [бирже](/terms.md#exchange).
В зависимости от этой информации стратегия совершает какие-то действия.
Это может быть постановка новых [заявок](/terms.md#order) и/или запрос на удаление старых.

> Инструменты, на которых стратегия осуществляет торговлю, будем называть торговыми.
Названия полей и методов, относящихся к торговым инструментам, будут содержать слово *trade*.
По каким-то инструментам стратегия только получает информацию, чтобы использовать ее для торговли на других.
Соответствующие инструменты – сигнальные, а названия полей и методов будут содержать слово *signal*.

#### Класс-шаблон UserStrategy<a id = "user_strategy"></a>

Рассмотрим класс-шаблон **UserStrategy**, предназначенный для написания стратегий.
Он наследует от класса-интерфейса [ParticipantStrategy](/api/ParticipantStrategy.md), где объявлено 3 виртуальных функции, которые вы можете реализовать в своей стратегии:

- [trading_book_update](/api/ParticipantStrategy.md#trading_book_update),
- [trading_deals_update](/api/ParticipantStrategy.md#trading_deals_update),
- [execution_report_update](/api/ParticipantStrategy.md#execution_report_update).

Сам класс выглядит следующим образом:
```c++
#include "./participant_strategy.h"

using namespace hftbattle;

class UserStrategy : public ParticipantStrategy {
public:
  // В конструктор стратегии участника передается файл конфигурации.
  // В файл конфигурации из веб-интерфейса можно передать параметры стратегии.
  UserStrategy(JsonValue config) {}

  // Вызывается при получении нового стакана торгового инструмента:
  // @order_book – новый стакан.
  void trading_book_update(const OrderBook& order_book) override {
    /* написать свою реализацию здесь */
  }

  // Вызывается при получении новых сделок торгового инструмента:
  // @deals - вектор новых сделок.
  void trading_deals_update(const std::vector<Deal>& deals) override {
    /* написать свою реализацию здесь */
  }

  // Вызывается при получении отчета о сделке с участием вашего ордера:
  // @execution_report – структура-отчет о совершенной сделке.
  void execution_report_update(const ExecutionReport& execution_report) override {
    /* написать свою реализацию здесь */
  }

};
```

> Для стратегий, торгующих на одном инструменте и получающих обновления от некоторого сигнального инструмента понадобятся еще две функции.

Рассмотрим методы, подлежащие реализации, подробнее.

#### Обновление стаканов<a id="book_update"></a>

При торговле на бирже постоянно происходят какие-то события в [стакане](/terms.md#order_book) инструмента.
Для информирования стратегии о произошедших изменениях используется функция [trading_book_update](/api/ParticipantStrategy.md#trading_book_update).
Они принимают на вход ссылку на элемент типа [OrderBook](/api/OrderBook.md), который является новой версией торгового стакана.

**Замечание 1:**

В классе [ParticipantStrategy](/api/ParticipantStrategy.md) есть 2 поля, отвечающие торговому стакану:
- [trading_book](/api/ParticipantStrategy.md#trading_book) – указатель на актуальный торговый стакан,
- [trading_book_snapshot](/api/ParticipantStrategy.md#trading_book_snapshot) – умный указатель на структуру, содержащую этот стакан.

Если в вашей стратегии вы явно не сохраните копию [trading_book_snapshot](/api/ParticipantStrategy.md#trading_book_snapshot), то при следующем вызове [trading_book_update](/api/ParticipantStrategy.md#trading_book_update) поля [trading_book](/api/ParticipantStrategy.md#trading_book) и [trading_book_snapshot](/api/ParticipantStrategy.md#trading_book_snapshot) обновятся, и предыдущий актуальный стакан будет недоступен, потому что на предыдущий стакан останется 0 активных ссылок.
> Для [signal_book_snapshot](/api/ParticipantStrategy.md#signal_book_snapshot) всё аналогично.

**Замечание 2:**

[trading_book](/api/ParticipantStrategy.md#trading_book) содержит указатель на копию того стакана, с которым работает симулятор, поэтому эта копия никогда не меняется.

#### Обновление сделок<a id="deals_update"></a>

Помимо изменений стакана полезной информацией являются совершенные сделки.
Для того, чтобы получать и обрабатывать эти изменения, применяется функция [trading_deals_update](/api/ParticipantStrategy.md#trading_deals_update), которая принимает в качестве аргумента вектор элементов класса [Deal](/api/Deal.md), который хранит в себе информацию об одной совершённой сделке.
Заметьте, что по каждой совершенной сделке информация о ней приходит **ровно один раз**.

#### Отчёт о наших сделках<a id="execution_report"></a>

Функция [execution_report_update](/api/ParticipantStrategy.md#execution_report_update) предназначена для того, чтобы получать информацию о сделках, с участием наших заявок.
На каждую такую сделку приходит отдельный *execution_report_update*.
