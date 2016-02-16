### Графики
По умолчанию для каждой тренировочной посылки строится график результата и позиции в течение дня. Также имеется возможность добавлять свои графики из стратегии с помощью функции *add_chart_point*:
```c++
void add_chart_point(const std::string& line_name, 
                     double value, 
                     ChartYAxisType y_axis_type, 
                     int8_t chart_number) {
```
Например, стратегия, "рисующая" график лучшей цены выглядит так:
```c++
add_chart_point("price_of_added_ask", price.get_double(), ChartYAxisType::Left, 1);
```