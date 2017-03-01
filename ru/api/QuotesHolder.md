# QuotesHolder

Путь в Local Pack `include/quotes_holder.h`

Этот класс позволяет итерироваться по всем котировкам одного из направлений.
При этом можно пользоваться теми же методами, которые есть у класса Quote.
Котировки отсортированы в порядке возрастания цены для BID (покупки) и в порядке убывания цены для ASK (продажи).

### Методы

| Имя | Описание |
| --- | --- |
| [begin()](#begin) | Итератор начала списка. |
| [end()](#end) | Итератор конца списка. |
| [rbegin()](#rbegin) | Обратный итератор конца списка. |
| [rend()](#rend) | Обратный итератор конца списка. |

### Описание методов

#### begin() {#begin}

Итератор начала списка.

{% codetabs name="C++", type="c++" -%}
const_iterator begin() const;
{%- endcodetabs %}

#### end() {#end}

Итератор конца списка.

{% codetabs name="C++", type="c++" -%}
const_iterator end() const;
{%- endcodetabs %}

#### rbegin() {#rbegin}

Обратный итератор конца списка.

{% codetabs name="C++", type="c++" -%}
const_reverse_iterator rbegin() const;
{%- endcodetabs %}

#### rend() {#rend}

Обратный итератор конца списка.

{% codetabs name="C++", type="c++" -%}
const_reverse_iterator rend() const;
{%- endcodetabs %}
