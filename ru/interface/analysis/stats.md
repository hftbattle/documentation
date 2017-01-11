###Статистика
На странице с тренировочной посылкой имеется обширный интерфейс для анализа поведения вашей стратегии. Основные разделы этой страницы:

* [Общий результат](#common_result)
* [Результаты по дням](#results_by_day)
* [Статистика по дням](#stats_by_day)

<a id="common_result"></a>
####Общий результат

В таблице “Общий результат” показана общая оценка всей стратегии, либо, когда присутствует перебор параметров, оценки всех комбинаций параметров.

Оценка стратегий происходит по следующей формуле:
```py
SCORE = AVG - (STD / 10.0) + min(0, MIN_INTRADAY_RESULT) / 5.0
```
- `AVG` - средний результат работы стратегии на всех днях (результаты уже с учетом комиссии 0.5 доллара за каждый лот в ваших сделках),
- `STD` - среднеквадратическое отклонение результата работы стратегии на всех днях,
- `MIN_INTRADAY_RESULT` - минимальный результат, достигнутый стратегией в любой момент дня (т.е. не обязательно в конце торгового дня), среди всех дней.

Нажав на кнопку “Подробнее” в строчках таблицы, можно узнать значения компонент, из которых складывается общая оценка стратегии.


<a id="results_by_day"></a>
####Результаты по дням

В таблице “Результаты по дням”, показаны результаты симуляций для каждого дня, а также приведены ссылки на дополнительную информацию по каждой симуляции:
- [графики](charts.md) вашего результата и позиции в течение дня,
- [viewer](viewer.md) - визуализатор реальных торгов, то есть торгов без вашего участия,
- [логи](logs.md) - ссылки на файлы с вашими логами, а также списками ваших заявок и сделок,
- stdout - стандартный вывод стратегии,
- логи компиляции.

<a id="stats_by_day"></a>
####Статистика по дням
Ниже, в таблице “Статистика по дням” приведены еще несколько характеристик симуляций, отдельно для каждого из дней.

Описание столбцов:
- min_result / max_result - минимальный / максимальный результат, достигнутый стратегией в пределах дня,
- our_deals_count - количество сделок, совершенных стратегией,
- our_deals_volume - суммарный объем,
- result - финальный результат в конце этого дня,
- transactions_count - количество транзакций (постановки и удаления заявок), совершенных стратегией,
- work_time - чистое время работы симуляции в секундах, за исключением накладных расходов на подготовку запуска.