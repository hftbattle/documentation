# Quote

## default_quote_price

Путь в Local Pack `include/quote.h`

Цена по умолчанию для обозначения пустого уровня.
Принимает направление dir.

Возвращает цену по умолчанию по направлению dir.

Внимание: если котировка не существует (в котировке нет заявок), то её цена совпадает с default_quote_price(dir).

## Quote

Путь в Local Pack `include/quote.h`

Описание ценового уровня в стакане.
Также называется котировка.

### Методы

| Имя | Описание |
| --- | --- |
| [dir()](#dir) | Направление котировки. |
| [price()](#price) | Цена котировки. |
| [volume()](#volume) | Объём котировки. |
| [server_time()](#server_time) | Биржевое время последнего изменения котировки. |

### Описание методов

#### dir() {#dir}

Возвращает направление котировки.

{% codetabs name="C++", type="c++" -%}
Dir dir() const;
{%- language name="Python", type="py" -%}
def dir(self)
{%- endcodetabs %}

#### price() {#price}

Возвращает цену котировки.

{% codetabs name="C++", type="c++" -%}
Price price() const;
{%- language name="Python", type="py" -%}
def price(self)
{%- endcodetabs %}

#### volume() {#volume}

Возвращает суммарное количество лотов в заявках на данном ценовом уровне.

{% codetabs name="C++", type="c++" -%}
Amount volume() const;
{%- language name="Python", type="py" -%}
def volume(self)
{%- endcodetabs %}

#### server_time() {#server_time}

Возвращает биржевое время последнего изменения котировки в микросекундах.

{% codetabs name="C++", type="c++" -%}
Microseconds server_time() const;
{%- language name="Python", type="py" -%}
def server_time(self)
{%- endcodetabs %}
