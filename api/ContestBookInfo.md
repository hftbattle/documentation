#ContestBookInfo

`include/simulator/strategy/contest_book_info.h`


Для каждого используемого инструмента есть разные параметры, описывающие его состояние -- начиная от стакана (описание тут) до нашей позиции. Класс ContestBookInfo служит агрегатором информации по текущему состоянию стакана. Он не является полноценной заменой стакана, но содержит часть информации о нем -- и предпочтительно пользоваться именно этим классом, например, для определения лучших цен, это значительно быстрее, чем запрашивать у стакана. Также именно тут можно узнать наши активные заявки, позицию и сальдо.




































 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
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
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
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
d
d
d
d
d
d
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
i
i
i
i
i
i
i
i
i
i
i
i
i
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
x
x
x
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
Б
В
И
И
И
Л
М
М
Н
Н
О
О
П
Р
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
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
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
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
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
х
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
ш
ш
ш
щ
щ
щ
щ
щ
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
ь
ь
ь
ь
ь
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
2
2
2
2
2
2
3
3
3
3
4
6
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
D
D
D
I
M
O
S
S
S
S
S
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
_
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
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
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
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
o
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
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
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
Б
В
И
И
Л
М
Н
Н
О
О
П
Р
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
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
а
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
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
о
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
х
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
ш
ш
ш
щ
щ
щ
щ
щ
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
ь
ь
ь
ь
ь
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
