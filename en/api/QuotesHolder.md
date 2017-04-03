# QuotesHolder

Path in Local Pack `include/quotes_holder.h`

This class allows you to iterate through the quotes.
Quotes are sorted by price in descending order for BID (Buy) and in ascending order for ASK (Sell).

### Methods

| Name | Description |
| --- | --- |
| [begin()](#begin) | An iterator pointing to the first element in the collection. |
| [end()](#end) | An iterator referring to the past-the-end element in the collection. |
| [rbegin()](#rbegin) | A reverse iterator pointing to the last element. |
| [rend()](#rend) | A reverse iterator pointing to the theoretical element preceding the first element. |

### Methods description

#### begin() {#begin}

An iterator pointing to the first element in the collection.

{% codetabs name="C++", type="c++" -%}
const_iterator begin() const;
{%- endcodetabs %}

#### end() {#end}

An iterator referring to the past-the-end element in the collection.

{% codetabs name="C++", type="c++" -%}
const_iterator end() const;
{%- endcodetabs %}

#### rbegin() {#rbegin}

A reverse iterator pointing to the last element.

{% codetabs name="C++", type="c++" -%}
const_reverse_iterator rbegin() const;
{%- endcodetabs %}

#### rend() {#rend}

A reverse iterator pointing to the theoretical element preceding the first element.

{% codetabs name="C++", type="c++" -%}
const_reverse_iterator rend() const;
{%- endcodetabs %}
