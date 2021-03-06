## Данные для торгов

Для симуляции торгов используются данные одной из крупнейших бирж мира.

Эти данные были получены непосредственно от биржи и в этом смысле являются полностью реалистичными.
В то же время, как только стратегия начнёт реагировать на изменения рынка, мы начнём влиять на то, что происходит с инструментом.
Одним из важных предположений работы симулятора является то, что влияние стратегии на общее поведение биржевого инструмента несущественно.

В связи с этим в симуляторе одновременно поддерживаются 2 состояния торгового стакана:

- **Реальное состояние** — то, что было на бирже.
  На него стратегия никак не влияет.
- **Виртуальное состояние** — то, которое учитывает поведение стратегии.

В симуляторе используется локальное время биржи, торговая сессия длится с {{ book["trading.begin_time"] }} до {{ book["trading.end_time"] }}.
