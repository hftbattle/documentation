## Order

Путь в Local Pack-е: `include/order.h`

Заявка - это пара <цена, количество лотов>.
Класс Order представляет собой реализацию биржевой заявки.

### Поля

|Имя| Описание|
|------------------|--------------------|
|[id](#id)|Наш (внутренний) ID заявки.|
|[dir](#dir)|Направление заявки.|
|[price](#price)|Цена заявки.|
|[amount](#amount)|Объем заявки (количество лотов).|
|[time_in_force](#time_in_force)|Тип заявки - Limit или IOC.|
|[market_id](#market_id)|ID биржи.|
|[security](#security)|Инструмент, к которому относится заявка.|
|[kMaxCommentLength](#kMaxCommentLength)|Максимальная длина комментария к заявке.|
|[server_time](#server_time)|Биржевое время постановки заявки в стакан в тиках.|
|[origin_time](#origin_time)|Локальное время постановки заявки в стакан в тиках.|

### Методы

|Имя| Описание|
|------------------|--------------------|
|[amount_rest()](#amount_rest)|Текущий объем заявки. Может быть меньше начального, если были сделки с ее участием.|
|[get_server_time()](#get_server_time)|Биржевое время последнего изменения (в микросекундах).|
|[implied_amount()](#implied_amount)|Количество лотов, которое предположительно будет сведено.|
|[status()](#status)|Статус заявки: в процессе добавления, активная, ждущая удаления, удаленная.|
|[outer_id()](#outer_id)|Биржевой (внешний) ID заявки.|

### Описание полей

#### id<a id="id"></a>

```c++
const Id id;
```

Наш (внутренний) ID заявки.

#### dir<a id="dir"></a>

```c++
const Dir dir;
```

Направление заявки.

#### price<a id="price"></a>

```c++
const Price price;
```

Цена заявки.

#### amount<a id="amount"></a>

```c++
const Amount amount;
```

Объем заявки (количество лотов).

#### time_in_force<a id="time_in_force"></a>

```c++
const OrderTimeInForce time_in_force;
```

Тип заявки - Limit или IOC.

#### market_id<a id="market_id"></a>

```c++
const MarketId market_id;
```

ID биржи.

#### security<a id="security"></a>

```c++
const Security* security;
```

Инструмент, к которому относится заявка.

#### kMaxCommentLength<a id="kMaxCommentLength"></a>

```c++
static const size_t kMaxCommentLength = 20;
```

Максимальная длина комментария к заявке.

### Описание методов

#### amount_rest()<a id="amount_rest"></a>

```c++
inline Amount amount_rest() const;
```

Текущий объем заявки. Может быть меньше начального, если были сделки с ее участием.

#### get_server_time()<a id="get_server_time"></a>

```c++
Microseconds get_server_time() const;
```

Биржевое время последнего изменения (в микросекундах).

#### implied_amount()<a id="implied_amount"></a>

```c++
inline Amount implied_amount() const;
```

Количество лотов, которое предположительно будет сведено.

#### status()<a id="status"></a>

```c++
inline OrderStatus status() const;
```

Статус заявки: в процессе добавления, активная, ждущая удаления, удаленная.

#### outer_id()<a id="outer_id"></a>

```c++
inline Id outer_id() const;
```

Биржевой (внешний) ID заявки.

#### server_time<a id="server_time"></a>

```c++
int64_t server_time;
```

Биржевое время постановки заявки в стакан в тиках.

#### origin_time<a id="origin_time"></a>

```c++
const Ticks origin_time;
```

Локальное время постановки заявки в стакан в тиках.
