## Отправка стратегии

После того, как вы отправили свою стратегию в систему контеста в тренировочном режиме, начинается симуляция работы этой стратегии на некотором наборе дней.
По умолчанию будет использован 1 день.

Если ваша стратегия зависит от параметров, вы можете настроить [перебор их значений](params.md) перед отправкой стратегии в систему.
В этом случае система построит все возможные комбинации значений параметров и запустит по экземпляру стратегии для каждого набора параметров на каждом из указанных дней.
Общее количество запущенных симуляций в этом случае будет равно произведению `(общее число комбинаций параметров) x (количество выбранных дней)`.

Посылки с небольшим числом симуляций имеют повышенный приоритет в системе контеста:

- Посылки с не более чем {{ book["high.priority"] }} симуляциями имеют наиболее высокий приоритет.
- Посылки с не более чем {{ book["medium.priority"] }} симуляциями имеют средний приоритет.
- Все остальные посылки имеют обычный приоритет.
