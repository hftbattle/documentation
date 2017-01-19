## Quote

Путь в Local Pack-е: `include/quote.h`

Котировка (или квота) - это термин для обозначения одного ценового уровня в стакане.
Она характеризуется

- направлением (покупка или продажа)
- ценой
- объемом, т.е. количеством лотов, которые доступны для покупки/продажи по данной цене

В стратегии предлагается использовать один из методов класса OrderBook для получения
объекта типа Quote (например, get_quote_by_index).

### Методы

|Имя| Описание|
|------------------|--------------------|
|[get_dir()](#get_dir)|Направление котировки.|
|[get_price()](#get_price)|Цена котировки.|
|[get_volume()](#get_volume)|Объем лотов котировки.|
|[get_server_time()](#get_server_time)|Биржевое время последнего изменения в микросекундах.|
|[get_local_time()](#get_local_time)|Локальное время последнего изменения в микросекундах.|

### Описание методов

#### get_dir() {#get_dir}

```c++
inline Dir get_dir() const;
```

Направление котировки.

#### get_price() {#get_price}

```c++
inline Price get_price() const;
```

Цена котировки.

#### get_volume() {#get_volume}

```c++
inline Amount get_volume() const;
```

Объем лотов котировки.

#### get_server_time() {#get_server_time}

```c++
Microseconds get_server_time() const;
```

Биржевое время последнего изменения в микросекундах.

#### get_local_time() {#get_local_time}

```c++
Microseconds get_local_time() const;
```

Локальное время последнего изменения в микросекундах.
