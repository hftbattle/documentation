# ExecutionReport

Путь в Local Pack `include/execution_report.h`

Описание отчёта о проведении сделки с участием вашей заявки.

### Методы

| Имя | Описание |
| --- | --- |
| [order()](#order) | Указатель на вашу сведённую заявку. |
| [price()](#price) | Цена произошедшей сделки. |
| [amount()](#amount) | Объём произошедшей сделки. |
| [dir()](#dir) | Направление вашей сведённой заявки. |

### Описание методов

#### order() {#order}

Возвращает указатель на вашу заявку (т.е. указатель на объект класса Order), которая была сведена в результате совершения сделки.

{% codetabs name="C++", type="c++" -%}
Order* order() const;
{%- language name="Python", type="py" -%}
def order(self)
{%- endcodetabs %}

#### price() {#price}

Возвращает цену, по которой была совершена сделка.

{% codetabs name="C++", type="c++" -%}
Price price() const;
{%- language name="Python", type="py" -%}
def price(self)
{%- endcodetabs %}

#### amount() {#amount}

Возвращает объём сделки.

{% codetabs name="C++", type="c++" -%}
Amount amount() const;
{%- language name="Python", type="py" -%}
def amount(self)
{%- endcodetabs %}

#### dir() {#dir}

Возвращает направление вашей сведённой заявки.

{% codetabs name="C++", type="c++" -%}
Dir dir() const;
{%- language name="Python", type="py" -%}
def dir(self)
{%- endcodetabs %}
