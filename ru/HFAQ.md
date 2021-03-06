# HFAQ

Вопросы разбиты по категориям:

- [Организационные вопросы](#org)
- [Интерфейс арены](#interface)
- [Пакет для локальной разработки](#local_pack)
- [Симулятор торгов](#simulator)
- [API симулятора](#api)
- [Разное](#misc)

---

## Организационные вопросы {#org}

##### Q: Можно ли участвовать в данном соревновании командой?

A: Да, для этого необходимо зарегистрировать один аккаунт для всей команды, указав данные одного из участников.
Приз будет выдан один на всю команду.
Участие с нескольких аккаунтов запрещено.
Все посылки будут проверяться на плагиат, нарушители будут исключены из соревнования.

---

## Интерфейс арены {#interface}

##### Q: Как послать стратегию на контрольное тестирование?

A: Чтобы послать стратегию на контрольное тестирование, нужно переключить триггер "Набор дней" на главной странице арены из режима "Тренировочный" в режим "Контрольный".
В день каждому участнику предоставляется {{ book["constraint.daily_control_submissions.ru.nom"] }} посылки в контрольном режиме.

---

## Пакет для локальной разработки {#local_pack}

##### Q: У меня не получается прогнать стратегии из пакета для локальной разработки. Что делать?

A: Прежде всего, убедитесь, что на вашей системе установлен:

- python `2.7.x` (`python --version`)

  Обратите внимание, что для запуска *Python*-стратегий необходим *Python* версии 2.
- cmake `2.8.4` и выше (`cmake --version`)
- компилятор C++:
  - для Linux — компилятор g++ `5.0` и выше (`g++ --version`)
  - для macOS — Apple LLVM version `6.0` и выше, (`c++ --version`)
  - для Windows — TDM64-GCC `5.1.0` и выше

Подробнее в разделе [Установка зависимостей](/local_pack/requirements.md).

---

## Симулятор торгов {#simulator}

##### Q: Есть ли временная задержка между моментом отправки заявки и временем ее постановки в стакан?

A: Время, прошедшее от отправки заявки на биржу до его приема биржей, называется *round-trip*-ом.
В симуляторе *round-trip* полагается равным {{ book["constraint.round-trip"] }} микросекундам.
Это значит, что все ваши запросы на добавление/удаление заявок производятся не мгновенно, а с небольшой задержкой по времени, имитирующей время "полёта" заявки до биржи.

---

##### Q: Я установил *stop_loss* c помощью метода [set_stop_loss_result](api/ParticipantStrategy.md#set_stop_loss_result). Почему я получил меньший результат?

A: Закрытие позиции происходит не моментально, и в момент закрытия цена может сильно меняться, поэтому ваш итоговый результат может оказаться как меньше, так и больше того, который вы установили в качестве *stop_loss*.

---

##### Q: Почему моя заявка может была отклонена торговой системой?

A: На это может быть несколько причин:

1. Возможно, ваша заявка может привести к превышению максимально допустимой позиции.
2. Цена заявки является некорректной, т.е. такой цены не существует для данного инструмента.
3. Количество лотов не является натуральным числом.
4. Эта заявка может свестись с другой вашей заявкой.
5. Вы пытаетесь поставить заявку в невидимую часть стакана.

##### Q: Что значит, что позиция стратегии закрывается? {#close_position}

A: Симулятор торгов не даёт стратегии выставлять новые заявки и удаляет текущие.
После этого он автоматически выставляет заявки так, чтобы размер позиции стратегии стал равен нулю.

---

## API симулятора {#api}

##### Q: Как узнать текущую лучшую цену по направлению?

A: Текущую лучшую цену можно узнать у стакана [trading_book](api/ParticipantStrategy.md#trading_book) c помощью метода [best_price](api/OrderBook.md#best_price):

{% codetabs name="C++", type="c++" -%}
Price best_price = trading_book().best_price(dir);
{%- language name="Python", type="py" -%}
best_price = strat.trading_book().best_price(dir)
{%- endcodetabs %}

---

## Разное {#misc}

##### Q: Какой механизм работы стакана?

A: После попадания заявки X в стакан она становится в очередь заявок по своему направлению и по своей цене.
Внутри каждой цены заявки отсортированы по времени постановки.
Если лучшая цены покупки строго меньше лучшей цены продажи, то обработка заявки на этом заканчивается.
В противном случае начинается сведение заявок.

Предположим, что заявка X была заявкой на покупку.
Тогда одна за одной берутся заявки на продажу в порядке возрастания цены, а внутри каждой цены — в порядке возрастания времени постановки заявки, и сводятся с заявкой X, в результате чего получаются сделки по цене сводимых пассивных заявок (в данном случае заявок на продажу) c объёмом, равным минимуму из участвующих в сделке заявок.
Объём сделки вычитается из объёмов участвующих в сделке заявок.
Так продолжается до тех пор, пока лучшая цена бида не станет меньше лучшей цены аска.

##### Q: Какие бывают типы заявок?

A: В рамках данного контеста используется 2 типа заявок: [лимитная заявка](terms.md#limit_order) (limit order) и [IOC](terms.md#ioc_order) (Immediate-Or-Cancel).
Рассмотрим ситуацию, когда в результате выполнения всех возможных сведений заявка не была сведена полностью:

- Если заявка была лимитной, она остаётся стоять в стакане по своей заявленной цене.
- Если же заявка имела тип IOC, она сразу после выполнения всех возможных сведений удаляется из стакана.

Рассмотрим пример.
Пусть в стакане было 3 заявки на продажу:

- объёмом 10 по цене 101, поставленная в 11:00
- объёмом 20 по цене 101, поставленная в 11:01
- объёмом 10 по цене 102, поставленная в 11:00

| Цена | Объём (время постановки) |
| --- | --- |
| 101 | 10 (11:00), 20 (11:01) |
| 102 | 10 (11:00) |

Пусть далее участник торгов ставит заявку на покупку X по некоторой цене с объёмом 35.
Рассмотрим несколько вариантов:

1. X — лимитная по цене 101.
  Тогда произойдёт 2 сделки объёмами 10 и 20, и оставшиеся 5 лотов от X останутся стоять в стакане по цене 101.
2. X — типа IOC по цене 101.
  Тогда произойдёт 2 сделки, а остаток X удалится из стакана.
3. X — типа IOC по цене 102.
  Тогда произойдёт 3 сделки объёмами 10, 20 и 5 по ценам 101, 101 и 102, и от X ничего не останется.
