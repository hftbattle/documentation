#Order

`include/simulator/order/order.h`


Класс Order представляет собой реализацию заявки (надеемся, вы уже знакомы с тем что это такое! Если нет - то вам сюда). Ничего необычного, поэтому переходим сразу к
























 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
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
)
)
)
)
)
)
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
C
I
L
O
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
c
c
c
c
c
c
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
f
f
g
g
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
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
o
o
o
o
o
o
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
u
u
u
u
v
v
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
Б
И
И
К
Н
О
П
Т
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
о
о
о
о
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
т
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
х
ч
я
я
я
я
я
я
я
я


















 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
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
I
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
a
a
a
a
a
a
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
i
i
i
i
i
i
l
l
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
o
o
o
o
o
o
p
p
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
t
t
t
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
К
М
О
С
Т
а
а
а
а
а
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
ж
ж
ж
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
о
о
о
о
о
о
о
о
о
о
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
у
у
у
у
у
у
у
ч
ч
ч
ч
ш
щ
щ
ы
ы
ы
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
,
-
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
2
3
4
6
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
C
D
D
F
I
I
L
O
O
S
T
_
_
_
_
_
_
_
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
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
a
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
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
e
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
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
i
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
o
o
o
o
o
o
o
o
o
o
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
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
r
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
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
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
v
v
v
x
x
y
y
y
y
Б
И
К
Н
О
Т
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
е
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
л
л
л
л
л
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
о
о
о
о
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
т
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
х
ч
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
3
3
:
:
:
:
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
I
I
O
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
e
e
e
e
e
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
i
i
i
i
i
i
i
i
i
i
i
i
i
i
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
o
o
o
o
o
o
o
o
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
r
r
r
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
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
t
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
x
x
К
О
С
Т
а
а
а
а
а
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
ж
ж
ж
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
о
о
о
о
о
о
о
о
о
о
о
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
у
у
у
у
у
у
у
ч
ч
ч
ч
ш
щ
щ
ы
ы
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
