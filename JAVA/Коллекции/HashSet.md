Класс `HashSet` является типичным представителем коллекций типа «множество». Во многом он похож на класс `ArrayList`, и в каком-то смысле является его более примитивной версией.

Создать объект типа `HashSet` можно с помощью команды вида:

```java
HashSet<Тип> имя = new HashSet<Тип>();
```

Где тип — это тип элементов, которые можно хранить в коллекции `HashSet`.

У класса `HashSet` есть такие методы:

|Метод|Описание|
|---|---|
|```java<br>boolean add(Тип value)<br>```|Добавляет элемент `value` в коллекцию|
|```java<br>boolean remove(Тип value)<br>```|Удаляет элемент `value` из коллекции.  <br>Возвращает `true`, если там такой элемент был|
|```java<br>boolean contains(Тип value)<br>```|Проверяет, есть ли в коллекции элемент `value`|
|```java<br>void clear()<br>```|Очищает коллекцию: удаляет все элементы|
|```java<br>int size()<br>```|Возвращает количество элементов в коллекции|

