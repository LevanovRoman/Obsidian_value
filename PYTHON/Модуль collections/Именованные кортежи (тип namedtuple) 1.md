Именованные кортежи (тип `namedtuple`) — это подтип обычных кортежей в Python. У них те же функции, что и у обычных, но их значения можно получать как с помощью индекса (например, `[0]`), так и с помощью имени через точку (например, `.name`).

   Основное предназначение именованных кортежей — это улучшение читаемости программного кода.

Рассмотрим пример, в котором происходит работа с точкой на плоскости, имеющей две координаты `x` и `y`.

Приведенный ниже код использует функционал обычных кортежей (тип `tuple`):

```python
point = (3, 7)

print(point)
print(point[0], point[1])
print(type(point))
```

и выводит:

```no-highlight
(3, 7)
3 7
<class 'tuple'>
```

Не забывайте, что кортежи являются неизменяемыми, при попытке изменить значение кортежа мы получим ошибку `TypeError`.

Приведенный выше код работает как и полагается: он создает точку с двумя координатами. Однако является ли данный код читабельным? Можно ли заранее сказать, за что отвечают индексы `0` и `1`? Всегда ли нулевой индекс — это значение `x`, а первый индекс — значение `y`?

Чтобы предотвратить эту двусмысленность и сделать код читабельнее, мы можем использовать именованный кортеж.

Приведенный ниже код:

```python
from collections import namedtuple

Point = namedtuple('Point', ['x', 'y'])     # объявляем тип Point именованного кортежа

point = Point(3, 7)                         # создаем именованный кортеж Point

print(point)
print(point.x, point.y)
print(point[0], point[1])
print(type(point))
```

выводит:

```no-highlight
Point(x=3, y=7)
3 7
3 7
<class '__main__.Point'>
```

В приведенном выше коде мы создали объект типа `Point`, который является именованным кортежем. Обратиться к полям именованного кортежа можно через точку (`point.x, point.y`) или по индексу (`point[0], point[1]`), как и в обычных кортежах.

Важно отметить, что, хотя кортежи и именованные кортежи неизменяемы, сохраняемые в них значения не обязательно должны быть неизменяемыми. Совершенно законно создать кортеж или именованный кортеж, содержащий изменяемые значения.

Приведенный ниже код:

```python
from collections import namedtuple

Person = namedtuple('Person', ['name', 'children'])

sveta = Person('Sveta Gueva', ['Larisa', 'Timur'])
print(sveta)

sveta.children.append('Soslan')
print(sveta)
```

выводит:

```no-highlight
Person(name='Sveta Gueva', children=['Larisa', 'Timur'])
Person(name='Sveta Gueva', children=['Larisa', 'Timur', 'Soslan'])
```

При этом код:

```python
from collections import namedtuple

Person = namedtuple('Person', ['name', 'children'])

sveta = Person('Sveta Gueva', ['Larisa', 'Timur'])
sveta.children = ['Larisa', 'Timur', 'Soslan']
```

приведет к ошибке `AttributeError`.

Таким образом, мы можем создавать именованные кортежи, содержащие изменяемые объекты. Мы можем изменять изменяемые объекты в исходном кортеже. Однако это не означает, что мы изменяем сам кортеж. Кортеж продолжит содержать те же ссылки на память.

Кортежи и именованные кортежи с изменяемыми значениями не могут быть [хешированы](https://docs.python.org/3/glossary.html#term-hashable), поэтому не могут быть элементами множеств и ключами в словарях. Кортежи и именованные кортежи без изменяемых значений могут быть хешированы, поэтому могут быть элементами множеств и ключами в словарях.

## Функция namedtuple()

Функция `namedtuple()` выступает в роли **фабричной функции**, порождающей новые типы данных.

Сигнатура данной функции имеет вид: 

```python
namedtuple(typename, field_names, *, rename=False, defaults=None, module=None)
```

То есть функция принимает два обязательных параметра `typename` и `field_names` и три необязательных `rename`, `defaults`, `module`, имеющих значения по умолчанию `False`, `None`, `None` соответственно.

### Параметры typename и field_names

Параметр `typename` отвечает за имя создаваемого типа, параметр `field_names` за названия полей. Имя типа — это строка с типом, который нужно сделать именованным кортежем. В качестве параметра `field_names` можно использовать:

- список
- словарь
- кортеж
- строка
- множество

**Параметр `field_names` является списком:**

```python
from collections import namedtuple

Point = namedtuple('Point', ['x', 'y'])    # в качестве второго параметра передаем список
point =  Point(2, 4)
print(point)                               # выводит Point(x=2, y=4)
```

**Параметр `field_names` является кортежем:**

```python
from collections import namedtuple

Point = namedtuple('Point', ('x', 'y'))    # в качестве второго параметра передаем кортеж
point =  Point(2, 4)
print(point)                               # выводит Point(x=2, y=4)
```

**Параметр `field_names` является словарем:**

В этом случае для полей именованного кортежа используются **ключи словаря**, поэтому в качестве значений можно указать, все что угодно.

```python
from collections import namedtuple

Point = namedtuple('Point', {'x': 0, 'y': 69})    # в качестве второго параметра передаем словарь
point =  Point(2, 4)
print(point)                                      # выводит Point(x=2, y=4)
```

**Параметр `field_names` является строкой:**

При создании именованного кортежа с помощью строки мы указываем поля либо через символ пробела, либо разделяя их символом `,`.

```python
from collections import namedtuple

Point = namedtuple('Point', 'x y')    # в качестве второго параметра передаем строку
point =  Point(2, 4)
print(point)                          # выводит Point(x=2, y=4)
```

либо:

```python
from collections import namedtuple

Point = namedtuple('Point', 'x,y')     # в качестве второго параметра передаем строку
point =  Point(2, 4)
print(point)                           # выводит Point(x=2, y=4)
```

**Параметр `field_names` является множеством:**

Мы можем создать именованный кортеж с помощью множества, однако делать это не рекомендуется. Как мы знаем, множество – это неупорядоченный набор данных. Когда мы используем неупорядоченный набор данных для предоставления полей именованному кортежу, мы можем получить неожиданный результат.

Результатом работы кода:

```python
from collections import namedtuple

Point = namedtuple('Point', {'x', 'y'})    # в качестве второго параметра передаем множество
point =  Point(2, 4)
print(point)
```

может быть как:

```no-highlight
Point(x=2, y=4)
```

так и:

```no-highlight
Point(y=2, x=4)
```

В приведенном выше примере имена координат меняются местами, что может не подходить для вашего варианта использования.

 В качестве параметра `field_names` можно передавать любой итерируемый объект, например, результат вызова функций `map()` и `filter()`.

Обратите также внимание на то, что создавать именованные кортежи можно не только с помощью позиционных аргументов, но и с помощью именованных.

Приведенный ниже код:

```python
from collections import namedtuple

Point = namedtuple('Point', ['x', 'y'])
point1 = Point(2, 4)                          # позиционные аргументы
point2 = Point(y=10, x=3)                     # именованные аргументы

print(point1)
print(point2)
```

выводит:

```no-highlight
Point(x=2, y=4)
Point(x=3, y=10)
```

В качестве названия полей для именованных кортежей мы можем использовать любое корректное название имени переменной, за исключением:

- имен, начинающихся с подчеркивания (`_`)
- ключевых слов языка Python (`if, with, else, class, ...`)

Приведенный ниже код:

```python
from collections import namedtuple

Point = namedtuple('Point', ['x', '_y'])
```

приводит к возникновению ошибки: `ValueError: Field names cannot start with an underscore: '_y'`.

Приведенный ниже код:

```python
from collections import namedtuple

Point = namedtuple('Point', ['x', 'if'])
```

приводит к возникновению ошибки: `ValueError: Type names and field names cannot be a keyword: 'if'`.

### Параметр rename

Допустим, мы импортируем данные из CSV-файла и превращаем каждую строку в именованный кортеж. Структура файла имеет вид:

```no-highlight
name,surname,age,class
Timur,Guev,28,11
Ruslan,Chaniev,22,9
...
```

Названия полей мы берем из заголовка CSV-файла:

```python
from collections import namedtuple

headers = ('name', 'surname', 'age', 'class')

Student = namedtuple('Student', headers)
```

Поскольку одно поле имеет название `class` (ключевое слово языка Python) мы получаем ошибку: `ValueError: Type names and field names cannot be a keyword: 'class'`.

Проблема заключается в том, что мы не знаем, будут ли в качестве названий полей у нас ключевые слова языка Python или нет. Для решения данной проблемы можно использовать параметр `rename` со значением `True`.

Приведенный ниже код:

```python
from collections import namedtuple

headers = ('name', 'surname', 'age', 'class')

Student = namedtuple('Student', headers, rename=True)

stud = Student('Роман', 'Белых', 26, 10)
print(stud)
```

выводит:

```no-highlight
Student(name='Роман', surname='Белых', age=26, _3=10)
```

Обратите внимание на то, что Python автоматически переименовал поле `class` в `_3`.

Приведенный ниже код:

```python
from collections import namedtuple

headers = ('name', 'surname', 'age', 'class', 'with', 'color', 'name', 'class', 'if')

Student = namedtuple('Student', headers, rename=True)

stud = Student('Тимур', 'Гуев', 28, 11, 'sister', 'green', 'Tim', '11A', 'else')
print(stud)
```

выводит:

```no-highlight
Student(name='Тимур', surname='Гуев', age=28, _3=11, _4='sister', color='green', _6='Tim', _7='11A', _8='else')
```

Как мы видим, неудачные имена полей переименовались в соответствии с их порядковыми номерами, причем перед порядковым номером используется символ подчеркивания.

### Параметр defaults

Параметр `defaults` используется для того, чтобы установить значения по умолчанию для полей именованного кортежа.

Приведенный ниже код:

```python
from collections import namedtuple

Point = namedtuple('Point', ['x', 'y'], defaults=(0, 0))
point1 = Point()      # используем значения по умолчанию
point2 = Point(1, 9)

print(point1)
print(point2)
```

выводит:

```no-highlight
Point(x=0, y=0)
Point(x=1, y=9)
```

Можно указать значение по умолчанию только для некоторых полей, при этом `defaults` присваивает значения по умолчанию с конца.

Приведенный ниже код:

```python
from collections import namedtuple

Point = namedtuple('Point', ['x', 'y'], defaults=(0,))
point =  Point(7)      # используем значения по умолчанию для y
print(point)
```

выводит:

```no-highlight
Point(x=7, y=0)
```

   Имейте в виду, что параметр `defaults` работает только в Python 3.7+.

### Параметр module

Приведенный ниже код:

```python
from collections import namedtuple

Point = namedtuple('Point', ['x', 'y'])
point = Point(1, 2)
print(type(point))
```

выводит:

```no-highlight
<class '__main__.Point'>
```

Если мы укажем допустимое имя модуля для этого аргумента, тогда атрибуту `.__ module__` результирующего именованного кортежа будет присвоено это значение.

Приведенный ниже код:

```python
from collections import namedtuple

Point = namedtuple('Point', ['x', 'y'], module='customtypes')
point = Point(1, 2)
print(type(point))
```

выводит:

```no-highlight
<class 'customtypes.Point'>
```

Параметр `module` был добавлен в Python 3.6 для того, чтобы появилась возможность сериализовать/десериализовать именованные кортежи с помощью модуля `pickle` в разных реализациях Python (IronPython, Jython и т.д.)

## Распаковка именованного кортежа

Мы можем распаковывать именованный кортеж, также как и обычный.

Приведенный ниже код:

```python
from collections import namedtuple

Person = namedtuple('Person', ['name', 'age', 'height'])

timur = Person('Тимур', 29, 170)

name, age, height = timur

print(name)
print(age)
print(height)
```

выводит:

```no-highlight
Тимур
29
170
```

## Примечания

**Примечание 1.** Официальная документация по именованным кортежам доступна по [ссылке](https://docs.python.org/3.7/library/collections.html#collections.namedtuple).

**Примечание 2.** Прекрасная статья по именованным кортежам доступна по [ссылке](https://realpython.com/python-namedtuple/).

**Примечание 3.** Прекрасная статья по модулю `collections` доступна по [ссылке](https://realpython.com/python-collections-module/).

**Примечание 4.** Хорошая статья по именованным кортежам доступна по [ссылке](https://antonz.ru/namedtuple/).

**Примечание 5.** При работе с именованным кортежами мы можем использовать срезы.

Приведенный ниже код:

```python
from collections import namedtuple

Point = namedtuple('Point3D', ['x', 'y', 'z'])

point = Point(89, 54, -34)

print(point[1:])
print(point[:2])
print(point[1:2])
```

выводит:

```no-highlight
(54, -34)
(89, 54)
(54,)
```

Обратите внимание на то, что результатом среза является обычный кортеж (тип данных `tuple`).

**Примечание 6.** Как уже упоминалось, функция `namedtuple()` является фабричной, то есть порождающей новые типы данных. Важно понимать, что названием типа является строка, передаваемая в функцию в качестве  первого аргумента, а не название переменной, в которой содержится результат вызова функции.

Приведенный ниже код:

```python
from collections import namedtuple

unknowntype = namedtuple('Point', ['x', 'y'])

point = unknowntype(2, 4)

print(type(point))
```

выводит:

```no-highlight
<class '__main__.Point'>
```

Дабы избежать путаницы, старайтесь давать переменным название типа.

**Примечание 7.** Именованные кортежи (тип `namedtuple`) являются производными от обычных кортежей (тип `tuple`), поэтому наследует все их методы.

**Примечание 8.** Обратите внимание на то, как выводятся именованные кортежи: сначала выводится имя, а затем поля и их значения в том порядке, в котором они были указаны при создании.

Приведенный ниже код:

```python
from collections import namedtuple

Point = namedtuple('Point', ['x', 'y'])

point = Point(1, 5)

print(point)
```

выводит: 

```no-highlight
Point(x=1, y=5)
```

**Примечание 9.** Исходный код `namedtuple`доступен по [ссылке](https://github.com/python/cpython/blob/main/Lib/collections/__init__.py). Рекомендуем вам ознакомиться с ним, тогда вопросов точно не останется 😎
