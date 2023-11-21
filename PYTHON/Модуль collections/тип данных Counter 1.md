## Подсчет объектов

Иногда нам нужно узнать, как часто некоторый объект встречается в каком-либо источнике данных (список, кортеж, строка и т.д). Другими словами, нам нужно посчитать количество вхождений объекта. Обычно для таких целей используется переменная-счетчик, значение которой увеличивается на единицу при каждом обнаружении нужного объекта.

Приведенный ниже код:

```python
data = 'mississippi'
counter = 0

for letter in data:
    if letter == 's':
        counter += 1

print(counter)
```

выводит количество вхождений символа `s` в строке `mississippi`:

```no-highlight
4
```

Если нам нужно посчитать количество нескольких объектов, то нам нужно создать большое количество счетчиков. Однако вместо этого куда удобнее воспользоваться одним словарем. В ключах такого словаря будут храниться объекты, которые мы считаем, а значения по этим ключам будут содержать количество повторений данного объекта.

Приведенный ниже код:

```python
data = 'mississippi'
counter = {}

for letter in data:
     if letter not in counter:
         counter[letter] = 0
     counter[letter] += 1

print(counter)
```

выводит количество различных символов в строке `mississippi`:

```no-highlight
{'m': 1, 'i': 4, 's': 4, 'p': 2}
```

Использования словаря для решения задачи подсчета накладывает некоторые ограничения на ключи. Ключами словаря должны быть хешируемые значения. В Python неизменяемые объекты являются хешируемыми.

Для упрощения предыдущего кода мы можем использовать словарный метод `get()`.

Приведенный ниже код:

```python
data = 'mississippi'
counter = {}

for letter in data:
     counter[letter] = counter.get(letter, 0) + 1

print(counter)
```

выводит:

```no-highlight
{'m': 1, 'i': 4, 's': 4, 'p': 2}
```

Мы также можем использовать уже изученный ранее тип словаря `defaultdict`.

Приведенный ниже код:

```python
from collections import defaultdict

data = 'mississippi'
counter = defaultdict(int)

for letter in data:
     counter[letter] += 1

print(dict(counter))
```

выводит:

```no-highlight
{'m': 1, 'i': 4, 's': 4, 'p': 2}
```

Из приведенных трех решений последнее, пожалуй, является наиболее читабельным, коротким и лаконичным. Тем не менее, учитывая, что задача подсчета объектов является достаточно распространенной в программировании, язык Python предлагает лучший способ ее решения. Для подсчета объектов в модуле `collections` есть специальный тип данных под названием `Counter`, о котором пойдет речь далее.

## Тип данных Counter

Тип `Counter` является подтипом типа `dict`, специально разработанный для подсчета хешируемых объектов в Python. `Counter` хранит объекты в качестве ключей, а их количество — в качестве значений.

### Создание Counter

Существует несколько способов создания объектов типа `Counter`. Самый простой и распространенный способ основан на передаче коллекции с данными (список, строка, кортеж и т.д.) в конструктор типа.

Приведенный ниже код:

```python
from collections import Counter

counter = Counter('mississippi')     # создаем счетчик на основе строки
print(counter)
```

выводит:

```no-highlight
Counter({'i': 4, 's': 4, 'p': 2, 'm': 1})
```

   Мы можем преобразовать объект типа `Counter` в обычный словарь с помощью функции `dict()`.

Мы можем создавать объекты типа `Counter`, явно указывая начальные значения количества объектов.

Приведенный ниже код:

```python
from collections import Counter

counter1 = Counter({'i': 4, 's': 4, 'p': 2, 'm': 1})
counter2 = Counter(i=4, s=4, p=2, m=1)

print(counter1)
print(counter2)
```

выводит:

```no-highlight
Counter({'i': 4, 's': 4, 'p': 2, 'm': 1})
Counter({'i': 4, 's': 4, 'p': 2, 'm': 1})
```

Тип `Counter`, будучи подтипом типа `dict`, наследует все методы, предоставляемые обычным словарем. При этом вызов метода `fromkeys()` всегда будет приводить к возникновению ошибки:

```no-highlight
NotImplementedError: Counter.fromkeys() is undefined.  Use Counter(iterable) instead.
```

Такое поведение не случайно, оно позволяет избежать ошибок неоднозначности при создании объектов типа `Counter`.

Например, код:

```python
from collections import Counter

counter = Counter.fromkeys('mississippi', 2)
```

мог бы создать объект типа `Counter` на основе строки `mississippi` со значением по умолчанию равным 22 для всех символов строки, несмотря на реальное количество вхождений символов в строке `mississippi`.

Тип данных `Counter` накладывает такие же ограничения на ключи, как и обычные словари (тип `dict`): ключи должны быть хешируемыми. При этом нет никаких ограничений на объекты, которые мы будем хранить в качестве значений.

Приведенный ниже код:

```python
from collections import Counter

inventory = Counter(apple=5, orange=7, banana=0, tomato=-2, watermelon='N/A')

print(inventory)
```

выводит:

```no-highlight
Counter({'apple': 5, 'orange': 7, 'banana': 0, 'tomato': -2, 'watermelon': 'N/A'})
```

Тип `Counter` позволяет нам писать такой код, однако нужно понимать, что для правильной работы в качестве подсчета объектов значения должны быть целыми неотрицательными числами, представляющими количество.

### Обновление данных

Для изменения объектов типа `Counter` мы можем использовать метод `update()`. Реализация данного метода не заменяет значения как у обычных словарей (тип `dict`), а суммирует существующие значения. При этом для новых объектов метод `update()` создает новые пары `ключ: количество`.

Приведенный ниже код:

```python
from collections import Counter

letters = Counter({'i': 4, 's': 4, 'p': 2, 'm': 1})
print(letters)

letters.update('missouri')
print(letters)
```

выводит:

```no-highlight
Counter({'i': 4, 's': 4, 'p': 2, 'm': 1})
Counter({'i': 6, 's': 6, 'p': 2, 'm': 2, 'o': 1, 'u': 1, 'r': 1})
```

   Метод `update()` принимает любой итерируемый объект: список, строку, кортеж и т.д.

Мы также можем передавать методу `update()` другой объект типа `Counter`, либо обычный словарь (тип `dict`).

Приведенный ниже код:

```python
from collections import Counter

sales = Counter(apple=20, orange=5, banana=10)
monday_sales = Counter(apple=3, orange=12, banana=7)
tuesday_sales = {'apple': 4, 'orange': 5, 'tomato': 6}

print(sales)

sales.update(monday_sales)
print(sales)

sales.update(tuesday_sales)
print(sales)
```

выводит:

```no-highlight
Counter({'apple': 20, 'banana': 10, 'orange': 5})
Counter({'apple': 23, 'orange': 17, 'banana': 17})
Counter({'apple': 27, 'orange': 22, 'banana': 17, 'tomato': 6})
```

Мы также можем использовать метод `update()` с именованными аргументами. К примеру, вызов метода `sales.update(apple=3, orange=12, banana=7)` равнозначен вызову метода `sales.update(monday_sales)`.

### Доступ к элементам и итерирование

Доступ к элементам и итерирование по `Counter` словарям работает так же, как и у обычных словарей. Мы можем перебирать ключи напрямую или можем использовать словарные методы `items()`, `keys()` и `values()`.

Приведенный ниже код:

```python
from collections import Counter

letters = Counter('mississippi')

# обращение по ключу
print(letters['p'])
print(letters['i'])

print()

# перебор ключей напрямую
for letter in letters:
    print(letter, '->', letters[letter])

print()

# перебор ключей через метод
for letter in letters.keys():
    print(letter, '->', letters[letter])

print()

# перебор значений через метод
for count in letters.values():
    print(count)

print()

# перебор пар (ключ, значение) через метод
for letter, count in letters.items():
    print(letter, '->', count)
```

выводит:

```no-highlight
2
4

m -> 1
i -> 4
s -> 4
p -> 2

m -> 1
i -> 4
s -> 4
p -> 2

1
4
4
2

m -> 1
i -> 4
s -> 4
p -> 2
```

Если обратиться по ключу, которого нет в `Counter` словаре, то ошибка `KeyError` возникать не будет. Будет возвращено нулевое значение. При этом ключ создан не будет.

## Примечания

**Примечание 1.** Для подсчета объектов тип `Counter` использует высокооптимизированную функцию, написанную на языке C. Поэтому беспокоиться об эффективности использования данного типа не стоит.

**Примечание 2.** Для удаления всех элементов из `Counter` словаря используется уже знакомый нам метод `clear()`.

Приведенный ниже код:

```python
from collections import Counter

counter = Counter(i=4, s=40, a=1, p=20, b=98, z=69)

counter.clear()

print(counter)
```

выводит:

```no-highlight
Counter()
```

**Примечание 3.** Объекты типа `Counter` можно сравнивать между собой. Равные объекты имеют одинаковое количество элементов и содержат равные элементы (`ключ: количество`). Для сравнения используются операторы `==` и `!=`.

Приведенный ниже код:

```python
from collections import Counter

counter1 = Counter({'i': 4, 's': 4, 'p': 2, 'm': 1})
counter2 = Counter(m=1, s=4, i=4, p=2)
counter3 = Counter('iiiisspm')

print(counter1 == counter2)
print(counter1 == counter3)
```

выводит:

```no-highlight
True
False
```

До версии Python 3.10 словари `Counter(i=4)` и `Counter(i=4, s=0)` считались разными. Начиная с Python 3.10 сравнение рассматривает отсутствующие элементы как имеющие нулевое значение, поэтому приведенный ниже код:

```python
from collections import Counter

counter1 = Counter(i=4)
counter2 = Counter(i=4, s=0)

print(counter1 == counter2)
```

выводит:

```no-highlight
True
```

**Примечание 4.** Обратите внимание на то, что если значения по ключам будут иметь тип, отличный от числового, но при этом допускающий сложение (например, строки), то ошибок при вызове метода `update()` возникать не будет.

Приведенный ниже код:

```python
from collections import Counter

counter1 = Counter(i=4, s='4')
counter2 = Counter(i=5, s='5')

counter1.update(counter2)

print(counter1)
```

выводит:

```no-highlight
Counter({'i': 9, 's': '54'})
```

**Примечание 5.** Исходный код `Counter` доступен по [ссылке](https://github.com/python/cpython/blob/main/Lib/collections/__init__.py). Рекомендуем вам ознакомиться с ним, тогда вопросов точно не останется 😎.