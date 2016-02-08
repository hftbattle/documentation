# Общая структура стратегий

* [Торговые и сигнальные инструменты](#trade_and_feed_instruments)
* [Класс-шаблон UserStrategy](#user_strategy)
    * [Обновление стаканов](#book_update)
      * [trading_book_update](#book_update)
      * [signal_book_update](#book_update)
    * [Обновление сделок](#deals_update)
      * [trading_deals_update](#deals_update)
      * [signal_deals_update](#deals_update)
    * [Отчет о наших сделках](#execution_report)
      * [execution_report_update](#execution_report)

<a name = "trade_and_feed_instruments"></a>
## Торговые и сигнальные инструменты
Торговая стратегия получает на вход информацию об изменениях, произошедших с какими-то [инструментами](../glossary.md#instrument) на [бирже](../glossary.md#exchange). В зависимости от этой информации стратегия совершает какие-то действия. Это может быть постановка новых [заявок](../glossary.md#order) и/или запрос на удаление старых. 

Инструменты, на которых стратегия осуществляет торговлю, будем называть торговыми. Названия полей и методов, относящихся к торговым инструментам, будут содержать слово *trade*. 

По каким-то инструментам стратегия только получает информацию, чтобы использовать ее для торговли на других. Соответствующие инструменты – сигнальные, а названия полей и методов будут содержать слово *signal*.

Далее считаем, что есть ровно один торговый инструмент и не более одного сигнального.

<a name = "user_strategy"></a>
## Класс-шаблон UserStrategy
Рассмотрим класс-шаблон **UserStrategy**, предназначенный для написания стратегий. Он наследует от класса-интерфейса [ParticipantStrategy](../../api/ParticipantStrategy.md), где объявлено 6 виртуальных функций, которые вы можете реализовать в своей стратегии.

Для стратегий, торгующих на одном инструменте:
```cpp
#include "strategy/participant_strategy_layer.h"

using namespace contest_platform;

class UserStrategy : public ParticipantStrategy {
public:
  // В конструктор стратегии участника передается конфиг.
  // В конфиг из веб-интерфейса можно передать параметры стратегии.
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
  // @snapshot – структура-отчет о совершенной сделке.
  void execution_report_update(const ExecutionReportSnapshot& snapshot) override {
    /* написать свою реализацию здесь */
  }

};
```

Для стратегий, торгующих на одном инструменте и получающих обновления от некоторого сигнального инструмента понадобятся еще две функции:
```cpp
  // Вызывается при получении нового стакана сигнального инструмента:
  // @order_book – новый стакан.
  void signal_book_update(const OrderBook& order_book) override {
    /* написать свою реализацию здесь */
  }

  // Вызывается при получении новых сделок сигнального инструмента:
  // @deals - вектор новых сделок.
  void signal_deals_update(const std::vector<Deal>& deals) override {
    /* написать свою реализацию здесь */
  }
```

Рассмотрим методы, подлежащие реализации, подробнее.

<a name="book_update"></a>
### Обновление стаканов
При торговле на бирже постоянно происходят какие-то события в [стаканах](../glossary.md#order_book) инструментов. Для информирования стратегии о произошедших изменениях используются функции [trading_book_update](../../api/ParticipantStrategy.md#trading_book_update) и [signal_book_update](../../api/ParticipantStrategy.md#signal_book_update). Они принимают на вход ссылку на элемент типа [OrderBook](../../api/OrderBook.md), который является новой версией стакана для торгового и сигнального инструментов соответственно.

>**Замечание 1**: в [ParticipantStrategy](../../api/ParticipantStrategy.md) есть поле [trading_book](../../api/ParticipantStrategy.md#trading_book) – указатель на актуальный торговый стакан, и поле [trading_book_snapshot](../../api/ParticipantStrategy.md#trading_book_snapshot) – умный указатель на структуру, содержащую этот стакан. Если в вашей стратегии вы явно не сохраните копию [trading_book_snapshot](../../api/ParticipantStrategy.md#trading_book_snapshot), то при следующем вызове [trading_book_update](../../api/ParticipantStrategy.md#trading_book_update) поля [trading_book](../../api/ParticipantStrategy.md#trading_book) и [trading_book_snapshot](../../api/ParticipantStrategy.md#trading_book_snapshot) обновятся, и предыдущий актуальный стакан будет недоступен (потому что на предыдущий стакан останется 0 активных ссылок). Для [signal_book_snapshot](../../api/ParticipantStrategy.md#signal_book_snapshot) все аналогично.

>**Замечание 2**: [trading_book](../../api/ParticipantStrategy.md#trading_book) содержит указатель на копию того стакана, с которым работает симулятор, поэтому эта копия никогда не меняется.

<a name="deals_update"></a>
### Обновление сделок
Помимо изменений стакана полезной информацией являются совершенные сделки. Для того, чтобы получать и обрабатывать эти изменения, вам предоставлены функции [trading_deals_update](../../api/ParticipantStrategy.md#trading_deals_update) и [signal_deals_update](../../api/ParticipantStrategy.md#signal_deals_update), которые принимают в качестве аргумента вектор элементов класса [Deal](../../api/Deal.md), который хранит в себе информацию об одной совершённой сделке. Заметьте, что для каждой совершенной сделки информация о ней приходит **ровно один раз**. Различие этих функций абсолютно такое же, как и у book-функций: [trading_deals_update](../../api/ParticipantStrategy.md#trading_deals_update) позволяет обрабатывать сделки торгового инструмента, [signal_deals_update](../../api/ParticipantStrategy.md#signal_deals_update) - сделки сигнального инструмента.

<a name="execution_report"></a>
### Отчет о наших сделках
Функция [execution_report_update](../../api/ParticipantStrategy.md#execution_report_update) предназначена для того, чтобы получать информацию о совершенных сделках с участием наших заявок.



