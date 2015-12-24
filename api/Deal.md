#Deal

Описание класса Order (объявлен в include/simulator/deal/deal.h)
---------------

Сделка - это акт (ссылка) купли-продажи определенного инструмента.

#Основные поля и методы:

- xor_platform::Microseconds server_time;
биржевое время совершения сделки

- xor_platform::Microseconds passive_order_server_time;
время постановки "пассивного" ордера, если оно известно

- xor_platform::Decimal price;
цена по которой произошла сделка

- xor_platform::Amount amount;
количество лотов в сделке

- xor_platform::Amount implied_amount;
количество лотов, которые предположительно должны были свестись в случае, когда налетающая заявка была нашей

- xor_platform::Dir dir;
направление сделки (покупка или продажа)

- std::vector<std::string> const& get_comments() const;
комментарии к заявкам, участвующим в сделке

- std::vector<std::shared_ptr<Order>> const& get_orders() const;
вектор заявок (на продажу и на покупку), участвующих в сделке

- inline int64_t get_tsc() const;
время машины, на которой сохранялись данные

- inline xor_platform::Dir get_agressor_side() const;
направление налетающей заявки

- bool is_our() const;
наша ли это сделка
