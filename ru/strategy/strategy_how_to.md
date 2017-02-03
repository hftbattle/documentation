## Как написать простую стратегию

Реализуем следующую стратегию: будем поддерживать наши [заявки](/terms.md#order) на лучшей цене в обоих направлениях.

Сначала научимся ставить заявку.
Для этого предназначена функция:

{% codetabs name="C++", type="c++" -%}
bool add_limit_order(Dir dir, Price price, Amount amount);
{%- language name="Python", type="py" -%}
def add_limit_order(self, dir, price, amount)
{%- endcodetabs %}

Внимание: в **Python** функции *trading_book_update*, *trading_deals_update*, *execution_report_update* являются свободными, поэтому в них первым параметром передаётся стратегия **strat**, от которой и нужно вызывать методы стратегии.

Функция [add_limit_order](/api/ParticipantStrategy.md#add_limit_order) выставляет нашу [лимитную заявку](/terms.md#limit_order), где:

- *dir* - направление (BID = 0 - покупка, ASK = 1 - продажа)
- *price* - цена, по которой заявка будет выставлена
- *amount* - размер заявки

Будем выставлять нашу заявку внутри функции [trading_book_update](/api/ParticipantStrategy.md#trading_book_update), когда нам приходит новый стакан `const OrderBook& order_book`.
Для определения лучшей цены используем метод [best_price](/api/OrderBook.md#best_price) класса [OrderBook](/api/OrderBook.md):

{% codetabs name="C++", type="c++" -%}
void trading_book_update(const OrderBook& order_book) override {
  for (Dir dir : {BID, ASK}) {
    const Price best_price = order_book.best_price(dir);
    const Amount amount = 1;
    add_limit_order(dir, best_price, amount);
  }
}
{%- language name="Python", type="py" -%}
def trading_book_update(strat, order_book):
    for dir in (BID, ASK):
        best_price = order_book.best_price(dir)
        amount = 1
        strat.add_limit_order(dir, best_price, amount)
{%- endcodetabs %}


В тот момент, когда у нас вызывается функция [trading_book_update](/api/ParticipantStrategy.md#trading_book_update), наши заявки, поставленные в прошлых вызовах этой функции, всё еще могут быть не исполнены.
В итоге, у нас может скопиться огромное количество заявок и мы превысим лимит на количество заявок в день.
К счастью, у нас есть возможность посмотреть все наши активные заявки:

{% codetabs name="C++", type="c++" -%}
const auto& our_orders = order_book.orders();
{%- language name="Python", type="py" -%}
our_orders = order_book.orders()
{%- endcodetabs %}

Здесь мы используем метод [orders](/api/OrderBook.md#orders), возвращающий ссылку на объект типа [SecurityOrdersSnapshot](/api/SecurityOrdersSnapshot.md).

> Замечание 1: Определенная выше переменная `orders` содержит и те заявки, которые мы уже отправили, но на которые ещё не отправили запрос на удаление.
> Поэтому если для какой-то заявки будет вызван метод [delete_order](/api/ParticipantStrategy.md#delete_order), то к следующему обновлению в `orders` этой заявки точно не будет, даже если в реальности она ещё не успела удалиться.
>
> Замечание 2: Обновление объекта [order_book.orders()](/api/OrderBook.md#orders) происходит только между апдейтами, внутри апдейта она не меняется.

Будем ставить заявку, если не существует активной заявки по этому направлению.
Используем для этого метод [active_orders_count(Dir dir)](/api/SecurityOrdersSnapshot.md#active_orders_count) класса [SecurityOrdersSnapshot](/api/SecurityOrdersSnapshot.md), возвращающий количество наших активных заявок по направлению:

{% codetabs name="C++", type="c++" -%}
void trading_book_update(const OrderBook& order_book) override {
  const auto& our_orders = order_book.orders();
  for (Dir dir : {BID, ASK}) {
    if (our_orders.active_orders_count(dir) == 0) {
      const Price best_price = order_book.best_price(dir);
      const Amount amount = 1;
      add_limit_order(dir, best_price, amount);
    }
  }
}
{%- language name="Python", type="py" -%}
def trading_book_update(strat, order_book):
    our_orders = order_book.orders()
    for dir in (BID, ASK):
        if our_orders.active_orders_count(dir) == 0:
            best_price = order_book.best_price(dir)
            amount = 1
            strat.add_limit_order(dir, best_price, amount)
{%- endcodetabs %}

В такой реализации есть минус – если лучшая цена изменится, то мы на это не отреагируем.
Это может привести к тому, что мы долго не будем торговать по одному из направлений.
Чтобы получить цену нашей активной заявки используем поле [orders_by_dir](/api/SecurityOrdersSnapshot.md#orders_by_dir) класса [SecurityOrdersSnapshot](/api/SecurityOrdersSnapshot.md#).
Полный код стратегии будет выглядеть так:

{% codetabs name="C++", type="c++" -%}
#include "participant_strategy.h"

using namespace hftbattle;

class UserStrategy : public ParticipantStrategy {
public:
  explicit UserStrategy(const JsonValue& /*config*/) { }

  void trading_book_update(const OrderBook& order_book) override {
    const auto& our_orders = order_book.orders();
    for (Dir dir : {BID, ASK}) {
      const Price best_price = order_book.best_price(dir);
      const Amount amount = 1;
      if (our_orders.active_orders_count(dir) == 0) {
        add_limit_order(dir, best_price, amount);
      } else {  // есть хотя бы одна наша активная заявка
        auto first_order = our_orders.orders_by_dir(dir)[0];
        const bool on_best_price = (first_order->price() == best_price);
        if (!on_best_price) {  // наша заявка стоит, но не на текущей лучшей цене
          delete_order(first_order);
          add_limit_order(dir, best_price, amount);
        }
      }
    }
  }
};

REGISTER_CONTEST_STRATEGY(UserStrategy, user_strategy)
{%- language name="Python", type="py" -%}
# -*- coding: utf-8 -*-

from py_defs import *
from py_defs import Decimal as Price
from common_enums import *


def trading_book_update(strat, order_book):
    our_orders = order_book.orders()
    for dir in (BID, ASK):
        best_price = order_book.best_price(dir)
        amount = 1
        if our_orders.active_orders_count(dir) == 0:
            strat.add_limit_order(dir, best_price, amount)
        else:
            first_order = our_orders.orders_by_dir(dir)[0]
            on_best_price = (first_order.price() == best_price)
            if not on_best_price:  # наша заявка стоит, но не на текущей лучшей цене
                strat.delete_order(first_order)
                strat.add_limit_order(dir, best_price, amount)
{%- endcodetabs %}

Теперь вы можете писать простейшие стратегии.
