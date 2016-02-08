
##Перебор параметров

* [Зачем перебирать параметры](#intro)
* [Передача параметров в стратегию](#to_strategy)
* [Чтение параметров из стратегии](#from_strategy)
* [Ограничения](#restrictions)

<a name="intro"></a>
####Зачем перебирать параметры
Зачастую в торговых стратегиях возникает необходимость подобрать какой-либо числовой параметр на основе исторических данных. Для этого оказывается полезным интерфейс перебора параметров.

<a name="to_strategy"></a>
####Передача параметров в стратегию
На странице с отправкой стратегии имеется возможность передавать стратегии набор числовых атомарных параметров для перебора. Для этого нужно открыть вкладку *Параметры запуска*, далее в поле *Parameter* ввести имя параметра, а в соседнее поле ввести значения параметра. Все значения могут быть целыми или вещественными с точкой в качестве разделителя. 

Рассмотрим способы задания параметров: 

[//]: <> (TODO: переделать на локальные изображения ![Screenshot](img/screenshot.png))
    
1. Постоянная:  
![param_const_double_set](https://lh3.googleusercontent.com/-JVRLRYRSSKQ/VnA6C_4IrWI/AAAAAAAAAFA/SbOoXN2JeaQ/s0/param_const_double_set.png "Константный вещественный параметр")
2. Список значений через запятую:
![list_int](https://lh3.googleusercontent.com/-_4dkBZBEkGg/VnA2I27c88I/AAAAAAAAADE/CCnaD-bfp10/s0/param_list_int_set.png "Список целых значений")
3. Диапазон значений с шагом: (start, end, step)
![param_range_int_set](https://lh3.googleusercontent.com/-xcBtI-CIZBM/VnA3L-BOX3I/AAAAAAAAADw/rLAUPLcjoSo/s0/param_range_int_set.png "Диапазон значений от 0 до 10 с шагом 2")


После ввода поля значений параметра нужно добавить эти значения, нажав на кнопку *Add*. В таком случае параметры, заданные способами 1 и 2 передадутся как есть, а параметры, заданные диапазоном значений с шагом раскроются в список параметров автоматически и отобразятся в таблице параметров. После добавления параметров для перебора под таблицей параметров отобразится поле *Total combinations*,  означающее общее число комбинаций параметров, то есть декартово произведение всех наборов параметров. 

В результате получим:

 1. Добавили постоянную double_param = 3.14. Всего комбинаций – 1.
![param_const_double_res](https://lh3.googleusercontent.com/-QF4NPJo_0Xk/VnA54K82etI/AAAAAAAAAE0/ye-PdSfb3FA/s0/param_const_double_res.png "Добавили постоянную double_param = 3.14]")

 2. Добавили список целых значений от 1 до 4. Всего комбинаций – 4.
![param_list_int_res](https://lh3.googleusercontent.com/-spMSUklQz5Q/VnA3wLSDzPI/AAAAAAAAAEA/jWbtGY7Vgq8/s0/param_list_int_res.png "Добавили список целых значений от 1 до 4")

 3. Добавили диапазон значений от 0 до 10 с шагом 2. Всего комбинаций – 6.
![param_range_int_res](https://lh3.googleusercontent.com/-LjXXdMWBtlc/VnA336r4n2I/AAAAAAAAAEM/ILgG_Q_l5k8/s0/param_range_int_res.png "Добавили диапазон значений от 0 до 10 с шагом 2")

Интерфейс перебора параметров позволяет добавлять несколько различных параметров, а также отключать перебор, задавая значение по умолчанию (*default value*):

 - Пример задания нескольких серий параметров. 2 значения для *double_param* и 4 значения для *int_param* дают 4 * 2 = 8 комбинаций.
![param_double_int_combo](https://lh3.googleusercontent.com/-sdQu4WaHgMk/VnA8SjdGwnI/AAAAAAAAAF0/P8sD8D_9u9A/s0/param_double_int_combo.png "Пример задания нескольких серий параметров")

 - Пример отключения перебора параметра double_param, значение по умолчанию = 0:
![param_double_int_turn_off_double](https://lh3.googleusercontent.com/-cf2RTnPdqKY/VnE5LydbgoI/AAAAAAAAAGk/Y1ZhCTFsE1o/s0/Screen+Shot+2015-12-16+at+13.11.52.png "Пример отключения перебора параметра double_param  ")


<a name="from_strategy"></a>
####Чтение параметров из стратегии
Все переданные стратегии параметры можно извлечь из конфига, передаваемого в конструктор стратегии пользователя *UserStrategy*. Параметры конфига приводятся к нужному типу с помощью метода `as<param_type>(deafult_value)`. При этом если вы не выберете значения для параметра, для него будет использовано значение по умолчанию `deafult_value`.
```cpp
class UserStrategy : public ParticipantStrategy {
public:
  // Параметры стратегии, которые хочется подобрать.
  int int_param;
  double double_param;
  Microseconds time_param;
  
  UserStrategy(JsonValue config) {
	// Читаем целочисленный параметр.
	int_param = config["int_param"].as<int>(42);
	// Читаем вещественный параметр.
	double_param = config["double_param"].as<double>(3.14);
	// Читаем временной параметр. 
	// ! Временной параметр по умолчанию необходимо указывать с единицей измерения (литералом).
	// Возможные литералы: 
	// h - часы, 
	// min - минуты, 
	// s - секунда,
	// ms - миллисекунды, 
	// us - микросекунды.
	time_param = config["time_param"].as<Microseconds>(3s);
  }
 
}
```

