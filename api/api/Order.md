#Order

Описание класса Order (объявлен в include/simulator/order/order.h)
---------------

Класс Order представляет собой реализацию заявки (надеемся, вы уже знакомы с тем что это такое! Если нет - то вам сюда). Ничего необычного, поэтому переходим сразу к

#Основные поля и методы класса

- inline xor_platform::Id outer_id() const;
id заявки

- int64_t origin_server_time;
биржевое время постановки заявки в стакан в тиках

- const xor_platform::Decimal price;
цена заявки

- const int32_t amount;
количество лотов

- const Dir dir;
направление

- const xor_platform::OrderTimeInForce time_in_force;
тип заявки - Limit или IOC

- const Security * security;
инструмент, к которому относится заявка

- inline int32_t amount_rest() const;
текущее количество лотов в заявке (может быть меньше начального, если были сделки с ее участием)

- inline int32_t implied_amount() const;
количество лотов, которое предположительно будет сведено

- inline xor_platform::OrderStatus status() const;
статус заявки - активная, ждущая удаления, удаленная
