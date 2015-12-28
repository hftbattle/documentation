#ParticipantStrategy

`include/simulator/strategy/participant_strategy_layer.h`


Стратегии наследуются от класса ParticipantStrategy, который служит прослойкой между симуляционным ядром и стратегией. Он обеспечивает обработку входящих сигналов от симуляции (апдейтов стаканов, сделок, сообщений о концах биржевых событий) и передачу в симуляцию сообщений о желаемых действий стратегии (постановка, снятие и перемещение заявок). Помимо реализации методов для описанных выше действий, класс также предоставляет некоторые вспомогательные методы для удобства работы.

В конструктор стратегии передается конфиг, в котором могут быть параметры, необходимые для работы стратегии, и которые могут перебираться в системе стратегии для подбора оптимального набора. Подробнее по параметры и их перебор можно почитать тут.




















 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
#
#
#
#
#
#
#
(
(
(
(
)
)
)
)
,
,
,
,
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
.
.
.
.
[
[
[
[
]
]
]
]
_
_
_
_
_
_
_
_
_
_
_
_
a
a
a
a
b
b
b
b
b
b
b
b
d
d
d
d
d
d
d
d
e
e
e
e
e
e
e
e
e
e
e
e
f
f
f
f
f
f
f
f
i
i
i
i
k
k
k
k
k
k
k
k
n
n
n
n
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
r
r
r
r
t
t
t
t
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
И
О
П
С
С
С
С
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
в
в
г
г
г
г
г
г
г
г
е
е
е
е
е
е
е
е
е
и
и
и
и
и
и
и
и
и
и
и
и
й
й
й
й
к
к
к
к
к
к
к
к
к
к
к
л
л
м
м
м
м
м
м
м
м
м
м
м
м
м
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
п
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
с
с
с
с
с
с
с
с
с
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
у
у
у
у
у
у
у
у
ф
ф
ц
ц
ы
ы
ь
я
я














































 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
&
&
&
&
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
*
*
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
:
:
:
:
:
:
<
<
=
>
>
A
A
A
B
B
D
D
D
D
D
D
E
O
O
O
P
P
P
P
R
S
S
S
S
S
S
S
T
T
U
U
U
U
U
U
[
[
[
[
[
[
[
[
[
[
[
[
[
[
[
[
[
[
]
]
]
]
]
]
]
]
]
]
]
]
]
]
]
]
]
]
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
b
b
b
b
b
b
b
b
b
b
b
b
b
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
f
f
f
f
g
g
g
g
g
g
g
g
g
g
g
g
h
h
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
k
k
k
k
k
k
k
k
k
k
k
l
l
l
l
l
l
l
l
l
l
l
l
l
l
l
l
l
l
l
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
p
p
p
p
p
p
p
p
p
p
p
p
p
p
p
p
p
p
p
p
p
p
p
p
p
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
v
v
v
v
v
v
v
v
x
x
x
x
x
y
y
y
y
y
y
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
|
В
И
М
М
О
Ф
Ф
Ф
Ф
Ф
Ф
Ф
Ф
Ф
Ф
Ф
Ф
Ф
Ф
Ф
Ф
Ф
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
б
б
б
б
б
б
б
б
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
г
г
г
г
г
г
г
г
г
д
д
д
д
д
д
д
д
д
д
д
д
д
д
д
д
д
д
д
д
д
д
д
д
д
д
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
ж
ж
ж
ж
ж
ж
ж
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
й
й
й
й
й
й
й
й
й
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
ф
ф
ф
х
х
х
х
х
ц
ц
ц
ц
ц
ц
ц
ц
ц
ц
ц
ц
ц
ц
ц
ц
ц
ц
ц
ц
ц
ц
ц
ц
ч
ч
ч
ч
ч
ч
ч
ч
ч
ч
ч
ч
ч
ч
ч
ч
ч
ч
ч
ч
ш
ш
ш
ш
ш
ш
ш
щ
щ
щ
щ
щ
щ
щ
щ
щ
щ
щ
щ
щ
щ
щ
щ
щ
щ
щ
щ
щ
щ
щ
щ
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ь
ь
ь
ь
ь
ь
ь
ь
ь
ь
ь
ю
ю
ю
ю
ю
ю
ю
ю
ю
ю
ю
ю
ю
ю
ю
ю
ю
ю
ю
ю
ю
ю
ю
ю
ю
ю
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я


























































 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
"
"
"
"
"
"
"
"
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
*
*
+
+
+
+
+
+
+
+
,
,
,
,
-
-
.
.
.
.
/
/
/
/
2
2
;
;
;
;
<
<
<
<
<
<
<
<
=
=
=
=
>
>
>
>
>
>
>
>
B
B
B
B
C
C
I
I
L
L
O
O
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
a
a
a
a
a
a
a
a
a
a
a
a
a
a
b
b
b
b
b
b
b
b
b
b
b
b
c
c
c
c
c
c
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
f
f
f
f
f
f
f
f
f
f
f
f
f
f
i
i
i
i
i
i
i
i
i
i
k
k
k
k
k
k
k
k
k
k
k
k
k
k
k
k
n
n
n
n
n
n
n
n
n
n
n
n
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
r
r
r
r
r
r
r
r
r
r
s
s
s
s
t
t
t
t
t
t
t
t
t
t
t
t
О
С
С
С
С
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
в
в
г
г
г
г
г
г
г
г
е
е
е
е
е
е
е
е
е
е
и
и
и
и
и
и
и
и
и
и
и
и
й
й
й
й
й
к
к
к
к
к
к
к
к
к
к
к
л
л
м
м
м
м
м
м
м
м
м
м
м
м
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
п
п
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
с
с
с
с
с
с
с
с
с
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
у
у
у
у
у
у
у
у
ф
ф
ц
ц
ы
ы
ь


































































































































































































































































































































 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
"
"
"
"
"
"
"
"
"
"
"
"
"
"
"
"
"
"
"
"
"
"
"
"
"
"
"
"
"
"
"
"
"
"
"
"
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
&
&
&
&
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
(
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
)
*
*
+
+
+
+
+
+
+
+
+
+
+
+
+
+
+
+
+
+
+
+
+
+
+
+
+
+
+
+
+
+
+
+
+
+
+
+
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
,
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
/
/
/
/
/
/
/
/
/
/
/
/
/
/
/
/
/
/
0
0
0
0
0
1
1
1
:
:
:
:
:
:
;
;
;
;
;
;
;
;
;
;
;
;
;
;
;
;
;
;
;
<
<
<
<
<
<
<
<
<
<
<
<
<
<
<
<
<
<
<
<
<
<
<
<
<
<
<
<
<
<
<
<
<
<
<
<
<
<
=
=
=
=
=
=
=
=
=
=
=
=
=
=
=
=
=
=
=
=
=
=
=
=
=
=
=
=
>
>
>
>
>
>
>
>
>
>
>
>
>
>
>
>
>
>
>
>
>
>
>
>
>
>
>
>
>
>
>
>
>
>
>
>
>
>
A
A
A
A
A
A
A
A
A
A
A
A
B
B
B
B
B
D
D
D
D
D
D
D
D
D
D
E
I
I
I
K
K
K
O
O
O
P
P
P
P
P
P
P
P
R
S
S
S
S
S
S
S
S
S
S
T
T
U
U
U
U
U
U
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
`
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
b
b
b
b
b
b
b
b
b
b
b
b
b
b
b
b
b
b
b
b
b
b
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
c
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
d
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
f
f
f
f
f
f
f
g
g
g
g
g
g
g
g
g
g
g
g
g
g
g
h
h
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
k
k
k
k
k
k
k
k
k
k
k
k
k
k
k
l
l
l
l
l
l
l
l
l
l
l
l
l
l
l
l
l
l
l
l
l
l
l
l
l
l
l
l
l
l
l
l
l
l
l
l
l
l
l
l
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
m
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
n
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
p
p
p
p
p
p
p
p
p
p
p
p
p
p
p
p
p
p
p
p
p
p
p
p
p
p
p
p
p
p
p
p
p
p
p
p
p
p
p
p
p
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
s
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
u
v
v
v
v
v
v
v
v
v
v
v
v
v
v
v
v
v
v
v
v
v
v
v
v
x
x
x
x
x
x
x
y
y
y
y
y
y
В
М
О
Ф
Ф
Ф
Ф
Ф
Ф
Ф
Ф
Ф
Ф
Ф
Ф
Ф
Ф
Ф
Ф
Ф
Ф
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
б
б
б
б
б
б
б
б
б
б
б
б
б
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
в
г
г
г
г
г
г
г
г
г
г
г
д
д
д
д
д
д
д
д
д
д
д
д
д
д
д
д
д
д
д
д
д
д
д
д
д
д
д
д
д
д
д
д
д
д
д
д
д
д
д
д
д
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
е
ж
ж
ж
ж
ж
ж
ж
ж
ж
ж
ж
ж
ж
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
з
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
и
й
й
й
й
й
й
й
й
й
й
й
й
й
й
й
й
й
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
к
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
л
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
м
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
н
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
п
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
р
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
с
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
т
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
у
ф
ф
ф
х
х
х
х
х
х
ц
ц
ц
ц
ц
ц
ц
ц
ц
ц
ц
ц
ц
ц
ц
ц
ц
ц
ц
ц
ц
ц
ц
ц
ц
ц
ц
ц
ч
ч
ч
ч
ч
ч
ч
ч
ч
ч
ч
ч
ч
ч
ч
ч
ч
ч
ч
ч
ч
ч
ч
ч
ш
ш
ш
ш
ш
ш
ш
ш
щ
щ
щ
щ
щ
щ
щ
щ
щ
щ
щ
щ
щ
щ
щ
щ
щ
щ
щ
щ
щ
щ
щ
щ
щ
ъ
ъ
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ы
ь
ь
ь
ь
ь
ь
ь
ь
ь
ь
ь
ь
ь
ь
ь
ь
ь
ю
ю
ю
ю
ю
ю
ю
ю
ю
ю
ю
ю
ю
ю
ю
ю
ю
ю
ю
ю
ю
ю
ю
ю
ю
ю
ю
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
я
–
–
–
–
–
–
–
–
