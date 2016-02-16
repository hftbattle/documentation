### Графики
По умолчанию для каждой тренировочной посылки строится график результата и позиции в течение дня. Также имеется возможность добавлять свои графики из стратегии с помощью функции *add_chart_point*:
```cpp
void add_chart_point(const std::string& line_name, double value, ChartYAxisType y_axis_type, int8_t chart_number) {
```
```cpp
add_chart_point("price_of_added_ask", price.get_double(), ChartYAxisType::Left, 1);
```