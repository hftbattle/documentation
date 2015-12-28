#OrderBook

`include/simulator/orderbook/order_book.h`


Стакан - это агрегатор всех заявок по конкретному финансовому инструменту (подробнее можно почитать в описании биржевых терминов). Класс OrderBook как раз и является реализацией биржевого стакана в нашем симуляторе.








































 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
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
:
:
:
:
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
D
D
D
D
D
D
D
D
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
q
q
q
q
q
q
q
q
q
q
q
q
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
v
v
v
v
v
v
w
w
x
x
x
x
x
x
x
x
x
x
x
x
x
x
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
y
y
y
y
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
В
Е
И
К
К
К
К
К
Л
М
М
О
О
О
С
У
Ц
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
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
ж
ж
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
ъ
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
:
:
:
:
:
:
:
:
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
H
Q
Q
Q
Q
Q
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
h
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
q
q
q
q
q
q
q
q
q
q
q
q
q
q
q
q
q
q
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
w
w
w
x
x
x
x
x
x
x
x
x
x
x
x
x
x
x
x
x
x
x
x
x
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
y
y
y
y
y
y
y
y
y
y
y
y
y
y
y
y
y
y
z
z
z
В
Е
К
К
К
К
К
Л
М
О
О
О
С
У
Ц
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
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
ж
ж
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
ъ
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
