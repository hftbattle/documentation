# QuotesHolder

Путь в Local Pack `include/quotes_holder.h`

Этот класс позволяет итерироваться по всем котировкам по одному из направлений.
Котировки отсортированы в порядке убывания цены для BID (покупки) и в порядке возрастания цены для ASK (продажи).

### Методы

| Имя | Описание |
| --- | --- |
| [begin()](#begin) | Итератор на первый элемент списка. |
| [end()](#end) | Итератор на элемент, следующий за последним элементом списка. |
| [rbegin()](#rbegin) | Обратный итератор на последний элемент списка. |
| [rend()](#rend) | Обратный итератор на элемент, предшествующий первому элементу списка. |

### Описание методов

#### begin() {#begin}

Итератор на первый элемент списка.

{% codetabs name="C++", type="c++" -%}
const_iterator begin() const;
{%- endcodetabs %}

#### end() {#end}

Итератор на элемент, следующий за последним элементом списка.

{% codetabs name="C++", type="c++" -%}
const_iterator end() const;
{%- endcodetabs %}

#### rbegin() {#rbegin}

Обратный итератор на последний элемент списка.

{% codetabs name="C++", type="c++" -%}
const_reverse_iterator rbegin() const;
{%- endcodetabs %}

#### rend() {#rend}

Обратный итератор на элемент, предшествующий первому элементу списка.

{% codetabs name="C++", type="c++" -%}
const_reverse_iterator rend() const;
{%- endcodetabs %}
