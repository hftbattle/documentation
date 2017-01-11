##Отправка стратегии

После того, как вы отправили свою стратегию в систему контеста в тренировочном режиме, начинается симуляция работы этой стратегии на некотором наборе дней. По умолчанию будет использован 1 день.

Посылки с небольшим числом дней (меньше пяти) имеют повышенный приоритет в системе контеста и будут выполнены быстрее.

Если ваша стратегия зависит от параметров, вы можете настроить [перебор их значений](./params.md) перед отправкой стратегии в систему.  В этом случае система построит все возможные комбинации значений параметров и запустит по экземпляру стратегии для каждого набора параметров на каждом из указанных дней. Общее количество запущенных симуляций в этом случае будет равно произведению `(общее число комбинаций параметров) x (количество выбранных дней)`.

Посылки с большим числом комбинаций параметров могут работать значительно дольше, чем посылки без параметров. Такие посылки получают уменьшенный приоритет в сравнении с посылками без перебора параметров.