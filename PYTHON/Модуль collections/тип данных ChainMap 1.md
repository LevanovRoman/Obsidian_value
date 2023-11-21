В Python 3.3 в модуль `collections` добавили новый тип данных `ChainMap`, который представляет из себя объединение нескольких словарей. Этот объект группирует несколько словарей вместе, что позволяет рассматривать их как единое целое.

### Создание ChainMap объекта

Объекты типа `ChainMap` обычно создаются на основе словарей.

Приведенный ниже код:

```python
from collections import ChainMap

empty_chain_map = ChainMap()                      # создаем пустой ChainMap объект
print(empty_chain_map)

numbers = {'one': 1, 'two': 2}
letters = {'a': 'A', 'b': 'B'}

chain_map = ChainMap(numbers, letters)            # создаем ChainMap объект на основе словарей numbers и letters
print(chain_map)
```

выводит:

```no-highlight
ChainMap({})
ChainMap({'one': 1, 'two': 2}, {'a': 'A', 'b': 'B'})
```

Мы также можем создавать объекты типа `ChainMap`, используя метод `fromkeys()`.

Приведенный ниже код:

```python
from collections import ChainMap

chain_map1 = ChainMap.fromkeys(['one', 'two', 'three'])
chain_map2 = ChainMap.fromkeys(['one', 'two', 'three'], -1)

print(chain_map1)
print(chain_map2)
```

выводит:

```no-highlight
ChainMap({'one': None, 'two': None, 'three': None})
ChainMap({'one': -1, 'two': -1, 'three': -1})
```

### Доступ к элементам ChainMap объекта

Для получения значений по ключу в `ChainMap` объектах используется такой же механизм, как и в обычных словарях (тип `dict`). Либо мы используем квадратные скобки, либо метод `get()`.

Приведенный ниже код:

```python
from collections import ChainMap

numbers = {'one': 1, 'two': 2}
letters = {'a': 'A', 'b': 'B'}

alpha_num = ChainMap(numbers, letters)


print(alpha_num['one'])
print(alpha_num['b'])
print(alpha_num.get('a'))
print(alpha_num.get('c'))
print(alpha_num.get('d', False))
```

выводит:

```no-highlight
1
B
A
None
False
```

Рассмотрим `ChainMap` объект в котором есть повторяющиеся ключи в объединяемых словарях.

Приведенный ниже код:

```python
from collections import ChainMap

for_adoption = {'dogs': 15, 'cats': 8, 'pythons': 9}
vet_treatment = {'dogs': 7, 'cats': 2, 'tigers': 3}

pets = ChainMap(for_adoption, vet_treatment)

print(pets['dogs'])
print(pets['cats'])

print(pets['pythons'])
print(pets['tigers'])
```

выводит: 

```no-highlight
15
8
9
3
```

Как мы видим, в ситуации когда у объединяемых словарей есть повторяющиеся ключи, мы получаем только значение из первого словаря, в котором встречается этот ключ. Таким образом, поиск по `ChainMap` объекту всегда осуществляется в том же порядке, в котором словари были указаны при создании `ChainMap` объекта, при этом поиск останавливается, как только значение по нужному ключу найдено.

### Итерирование по ChainMap объекту

Итерирование по `ChainMap` объекту происходит **в обратном порядке** от последнего указанного словаря к первому.

Приведенный ниже код:

```python
from collections import ChainMap

numbers = {'one': 1, 'two': 2}
letters = {'a': 'A', 'b': 'B'}

alpha_num = ChainMap(numbers, letters)

for key in alpha_num:
    print(key, '->', alpha_num[key])
```

выводит:

```no-highlight
a -> A
b -> B
one -> 1
two -> 2
```

При этом если в `ChainMap` объекте есть повторяющиеся ключи в объединяемых словарях, то мы будем получать первое из значений.

Приведенный ниже код:

```python
from collections import ChainMap

for_adoption = {'dogs': 15, 'cats': 8, 'pythons': 9}
vet_treatment = {'dogs': 7, 'cats': 2, 'tigers': 3}

pets = ChainMap(for_adoption, vet_treatment)

for key in pets:
    print(key, '->', pets[key])
```

выводит: 

```no-highlight
dogs -> 15
cats -> 8
tigers -> 3
pythons -> 9
```

При итерировании по `ChainMap` объекту мы можем использовать методы `keys(), values(), items()`.

Приведенный ниже код:

```python
from collections import ChainMap

for_adoption = {'dogs': 15, 'cats': 8, 'pythons': 9}
vet_treatment = {'dogs': 7, 'cats': 2, 'tigers': 3}

pets = ChainMap(for_adoption, vet_treatment)

for key in pets.keys():
    print(key, '->', pets[key])

print()

for value in pets.values():
    print(value)

print()

for key, value in pets.items():
    print(key, '->', value)
```

выводит:

```no-highlight
dogs -> 15
cats -> 8
tigers -> 3
pythons -> 9

15
8
3
9

dogs -> 15
cats -> 8
tigers -> 3
pythons -> 9
```

### Изменение данных в ChainMap объекте

Для изменения объектов типа `ChainMap` мы можем использовать те же способы, что и для изменения обычного словаря (тип `dict`). Мы можем обновлять, добавлять, удалять и извлекать элементы из `ChainMap` объекта. При этом нужно знать, что все эти операции действуют только на первый из объединяемых словарей.

Приведенный ниже код:

```python
from collections import ChainMap

numbers = {'one': 1, 'two': 2}
letters = {'a': 'A', 'b': 'B'}

alpha_num = ChainMap(numbers, letters)
print(alpha_num)

alpha_num['c'] = 'C'
print(alpha_num)

alpha_num['b'] = 'b'
print(alpha_num)

alpha_num.pop('two')
print(alpha_num)

del alpha_num['c']
print(alpha_num)

alpha_num.clear()
print(alpha_num)
```

выводит:

```no-highlight
ChainMap({'one': 1, 'two': 2}, {'a': 'A', 'b': 'B'})
ChainMap({'one': 1, 'two': 2, 'c': 'C'}, {'a': 'A', 'b': 'B'})
ChainMap({'one': 1, 'two': 2, 'c': 'C', 'b': 'b'}, {'a': 'A', 'b': 'B'})
ChainMap({'one': 1, 'c': 'C', 'b': 'b'}, {'a': 'A', 'b': 'B'})
ChainMap({'one': 1, 'b': 'b'}, {'a': 'A', 'b': 'B'})
ChainMap({}, {'a': 'A', 'b': 'B'})
```

При попытке удаления значения по ключу, которого нет в первом словаре возникает ошибка `KeyError`. Даже если указанный ключ есть в одном из объединяемых словарей, кроме первого.

Поскольку все изменения `ChainMap` объекта действуют только на первый из объединяемых словарей, то приведенный ниже код:

```python
from collections import ChainMap

numbers = {'one': 1, 'two': 2}
letters = {'a': 'A', 'b': 'B'}

alpha_num = ChainMap({}, numbers, letters)
print(alpha_num)

alpha_num['colon'] = ':'
alpha_num['comma'] = ','
alpha_num['period'] = '.'
print(alpha_num)
```

выводит:

```no-highlight
ChainMap({}, {'one': 1, 'two': 2}, {'a': 'A', 'b': 'B'})
ChainMap({'colon': ':', 'comma': ',', 'period': '.'}, {'one': 1, 'two': 2}, {'a': 'A', 'b': 'B'})
```

Таким образом, указывая в качестве первого аргумента для `ChainMap` пустой словарь, мы получаем поведение, при котором все изменения `ChainMap` объекта не затрагивают объединяемые (исходные) словари.

## Примечания

**Примечание 1.** В качестве альтернативы объединению нескольких словарей с помощью `ChainMap` мы можем объединять их с помощью словарного метода `update()`.

Приведенный ниже код:

```python
from collections import ChainMap

for_adoption = {'dogs': 15, 'cats': 8, 'pythons': 9}
vet_treatment = {'hamsters': 2, 'tigers': 3}

pets = ChainMap(for_adoption, vet_treatment)
```

можно переписать так:

```python
for_adoption = {'dogs': 15, 'cats': 8, 'pythons': 9}
vet_treatment = {'hamsters': 2, 'tigers': 3}

pets = {}
pets.update(for_adoption)
pets.update(vet_treatment)
```

В приведенном конкретно выше примере поиск по результирующему объекту `pets` будет работать одинаково. Объединение словарей с помощью метода `update()` имеет существенный недостаток по сравнению с объединением их с помощью `ChainMap`. Недостаток заключается в том, что мы теряем возможность управлять доступом к повторяющимся ключам. При использовании метода `update()` новые значения перетирают старые.

Приведенный ниже код:

```python
for_adoption = {'dogs': 15, 'cats': 8, 'pythons': 9}
vet_treatment = {'dogs': 7, 'cats': 2, 'tigers': 3}

pets = {}
pets.update(for_adoption)
pets.update(vet_treatment)

print(pets)
```

выводит:

```no-highlight
{'dogs': 7, 'cats': 2, 'pythons': 9, 'tigers': 3}
```

**Примечание 2.** Нам ничего не мешает использовать любой из ранее изученных словарей: `defaultdict, OrderedDict, Counter`.

```python
from collections import defaultdict, OrderedDict, Counter, ChainMap

numbers = OrderedDict(one=1, two=2)
letters = defaultdict(str, {'a': 'A', 'b': 'B'})
counter = Counter('aabbbcccc')

chain_map = ChainMap(numbers, letters, counter)

print(chain_map)
```

выводит:

```no-highlight
ChainMap(OrderedDict([('one', 1), ('two', 2)]), defaultdict(<class 'str'>, {'a': 'A', 'b': 'B'}), Counter({'c': 4, 'b': 3, 'a': 2}))
```

При этом нужно понимать, что поиск по `ChainMap` объекту будет учитывать особенность поиска по соответствующим словарям. Для `defaultdict`, в случае если ключ отсутствует, мы будем получать значение по умолчанию, для `Counter` – нулевое значение.

**Примечание 3.** Встроенная функция `len()` возвращает количество уникальных ключей `ChainMap` объекта.

Приведенный ниже код:

```
from collections import ChainMap

for_adoption = {'dogs': 15, 'cats': 8, 'pythons': 9}
vet_treatment = {'dogs': 7, 'cats': 2, 'tigers': 3}

pets = ChainMap(for_adoption, vet_treatment)

print(len(pets))
```

выводит:

```no-highlight
4
```

**Примечание 4.** Изменение содержания любого словаря, на основании которого создан `ChainMap`, изменяет и сам `ChainMap` объект.

Приведенный ниже код:

```python
from collections import ChainMap

for_adoption = {'dogs': 15, 'cats': 8, 'pythons': 9}
vet_treatment = {'dogs': 7, 'cats': 2, 'tigers': 3}

pets = ChainMap(for_adoption, vet_treatment)
print(pets)

for_adoption['dogs'] = 10000
for_adoption['lions'] = 9

print(pets)
```

выводит:

```no-highlight
ChainMap({'dogs': 15, 'cats': 8, 'pythons': 9}, {'dogs': 7, 'cats': 2, 'tigers': 3})
ChainMap({'dogs': 10000, 'cats': 8, 'pythons': 9, 'lions': 9}, {'dogs': 7, 'cats': 2, 'tigers': 3})
```

Аналогично, изменение `ChainMap` объекта приводит к изменению словаря, на основании которого он создан.

​**Примечание 5.** ​​Обратите внимание на то, что `ChainMap` может обновлять (изменять, вставлять и удалять) значения ключей только в первом словаре, в то время как поиск ключей осуществляется по всем словарям в списке.

**Примечание 6.** Документация по `ChainMap` на английском языке доступна по [ссылке](https://docs.python.org/3/library/collections.html#collections.ChainMap).

**Примечание 7.** Документация по `ChainMap` на русском языке доступна по [ссылке](https://docs-python.ru/standart-library/modul-collections-python/klass-chainmap-modulja-collections/).

**Примечание 8.** Исходный код `ChainMap` доступен по [ссылке](https://github.com/python/cpython/blob/main/Lib/collections/__init__.py). Рекомендуем вам ознакомиться с ним, тогда вопросов точно не останется 😎.
