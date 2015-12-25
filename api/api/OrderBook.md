#OrderBook

Описание класса OrderBook (объявлен в include/simulator/orderbook/order_book.h)
--------------

Стакан - это агрегатор всех заявок по конкретному финансовому инструменту (подробнее можно почитать в описании биржевых терминов). Класс OrderBook как раз и является реализацией биржевого стакана в нашем симуляторе.

#Ключевые поля и методы класса:

- const Quote& get_quote_by_idx(Dir dir, int index) const;
котировка номер index в стакане по выбранному направлению

- const Quote& quote_view_by_price(Dir dir, xor_platform::Decimal price) const;
котировка по цене price по направлению dir

- Quote* quote_by_price(Dir dir, xor_platform::Decimal price);
котировка по цене price по направлению dir

- Quote* quote_by_index(Dir dir, int index);
котировка номер index в стакане по выбранному направлению

- QuotesHolder all_quotes(Dir dir) const;
все котировки по выбранному направлению

- inline xor_platform::Decimal best_price(Dir dir);
лучшая цена в стакане по направлению dir

- inline Amount best_amount(Dir dir) const;
объем лучшей котировки по направлению dir

- inline Amount get_volume_by_price(Dir dir, xor_platform::Decimal price) const;
суммарный объем лотов на цене

- inline Amount get_volume_by_idx(Dir dir, int index) const;
объем котировки по индексу (нумерация с нуля, начиная от лучшей цены)

- inline xor_platform::Decimal get_price_by_idx(Dir dir, int index) const;
цена котировки по индексу (нумерация с нуля, начиная от лучшей цены)

- size_t index_by_price(Dir dir, xor_platform::Decimal price) const;
узнать индекс (нумерация с нуля, начиная от лучшей цены) по цене

- virtual size_t quotes_count(Dir dir) const;
количество отображаемых котировок по направлению

- bool contains_price(Dir dir, xor_platform::Decimal price) const;
есть ли такая цена в стакане по направлению

- size_t depth() const;
максимальная глубина отображаемого стакана
