Расширенная поддержка потоков в Java 8 реализована с помощью интерфейса `Stream<T>`. Где `T` — это тип-параметр, обозначающий тип данных, которые передаются в потоке. Другими словами, поток полностью независим от типа данных, которые он передает.

Чтобы получить объект-поток у коллекции, достаточно вызвать у нее метод `stream()`. Выглядит этот код примерно так:

```java
Stream<Тип> имя = коллекция.stream();
```

Получение потока из коллекции

При этом коллекция будет считаться источником данных потока, а объект типа `Stream<Тип>` – инструментом по получению данных из коллекции именно в виде потока данных.

```java
ArrayList<String> list = new ArrayList<String>();
Collections.addAll(list, "Привет", "как", "дела?");
Stream<String> stream = list.stream();
```

Кстати, можно получить поток не только у коллекции, но и у массива. Для этого нужно воспользоваться методом `Arrays.stream();` Пример:

```java
Stream<Тип> имя = Arrays.stream(массив);
```

Получение потока из массива

При этом массив будет считаться источником данных для потока `имя`.

```java
Integer[] array = {1, 2, 3};
Stream<Integer> stream = Arrays.stream(array);
```

После создания объекта `Stream<Тип>` никакого движения данных не происходит. Мы просто получили объект-поток для того, чтобы начать строить цепочку из потоков-данных.
## Список методов типа `Stream`

Класс `Stream` был создан для того, чтобы можно было легко конструировать цепочки потоков данных. Для этого у объекта типа `Stream<T>` есть методы, которые возвращают новые объекты типа `Stream`.

Каждый из этих потоков данных умеет делать одно простое действие, зато если их объединить в цепочки, да еще и добавить к этому такую интересную вещь как лямбда-функции, на выходе можно получить очень мощную вещь. Скоро вы в этом сами убедитесь.

Вот какие методы есть у класса `Stream` (только самые основные):

|Методы|Описание|
|---|---|
|```<br>Stream<T> of()<br>```|Создает поток из набора объектов|
|```<br>Stream<T> generate()<br>```|Генерирует поток по заданному правилу|
|```<br>Stream<T> concat()<br>```|Объединяет вместе несколько потоков|
|```<br>Stream<T> filter()<br>```|Фильтрует данные: пропускает только данные, которые соответствуют заданному правилу|
|```<br>Stream<T> distinct()<br>```|Удаляет дубликаты: не пропускает данные, которые уже были|
|```<br>Stream<T> sorted()<br>```|Сортирует данные|
|```<br>Stream<T> peek()<br>```|Выполняет действие над каждым данным|
|```<br>Stream<T> limit(n)<br>```|Обрезает данные после достижения лимита|
|```<br>Stream<T> skip(n)<br>```|Пропускает первые n данных|
|```<br>Stream<R> map()<br>```|Преобразовывает данные из одного типа в другой|
|```<br>Stream<R> flatMap()<br>```|Преобразовывает данные из одного типа в другой|
|```<br>boolean anyMatch()<br>```|Проверяет, что среди данных потока есть хоть одно, которое соответствует заданному правилу|
|```<br>boolean allMatch()<br>```|Проверяет, что все данные в потоке соответствуют заданному правилу|
|```<br>boolean noneMatch()<br>```|Проверяет, что никакие данные в потоке не соответствуют заданному правилу|
|```<br>Optional<T> findFirst()<br>```|Возвращает первый найденный элемент, который соответствует правилу|
|```<br>Optional<T> findAny()<br>```|Возвращает любой элемент из потока, который соответствует правилу|
|```<br>Optional<T> min()<br>```|Ищет минимальный элемент в потоке данных|
|```<br>Optional<T> max()<br>```|Возвращает максимальный элемент в потоке данных|
|```<br>long count()<br>```|Возвращает количество элементов в потоке данных|
|```<br>R collect()<br>```|Вычитывает все данные из потока и возвращает их в виде коллекции|

---

## 2. Intermediate и terminal методы `Stream`

Как вы заметили, не все методы, представленные в таблице выше, возвращают `Stream`. Это связано с тем, что методы класса `Stream` можно разделить на промежуточные (intermediate, non‑terminal) и конечные (terminal).

### Промежуточные методы

Промежуточные методы возвращают объект, который имплементирует интерфейс `Stream`, и их можно выстроить в цепочку вызовов.

### Конечные методы

Конечные методы возвращают значение, тип которого отличен от типа `Stream`.

### Цепочка вызовов методов

Таким образом, вы можете строить цепочки вызовов из любого количества промежуточных методов и в конце вызывать один конечный. Такой подход позволяет реализовать довольно сложную логику, повышая при этом читаемость кода.

![](https://cdn.javarush.com/images/article/76f6adda-6881-4f70-a2ad-d49c402d225e/1080.webp)

Внутри потока данных, данные вообще не меняются. Цепочка промежуточных методов – это хитрый (декларативный) способ указания некой последовательности обработки данных, которая начнет выполняться после вызова терминального (конечного) метода.

То есть, без вызова конечного метода, данные в потоке данных никак не обрабатываются. И только после вызова терминального метода, данные начинают обрабатываться по правилам, заданным цепочкой вызовов методов.

```
stream()
  .intemediateOperation1()
  .intemediateOperation2()
  ...
  .intemediateOperationN()
  .terminalOperation();
```

Общий вид цепочки вызовов

Сравнение промежуточных и конечных методов:

|                                                                   |промежуточные|конечные|
|---|---|---|
|Тип возвращаемого значения                                         |`Stream`      |не `Stream`|
|Возможность объединения нескольких методов данного типа в цепочку вызовов|да    |нет|
|Количество методов в одной цепочке вызовов                         |любое  |не более одного|
|Производит конечный результат                                      |нет          |да|
|Запускает обработку данных в потоке                                |нет          |да|

## Создание потоков

Среди методов класса `Stream` есть три метода, которые мы еще не рассмотрели. Задача этих трех методов — создавать новые потоки.

**Метод `Stream<T>.of(T obj)`**

Метод `of()` создает поток, состоящий из одного элемента. Обычно это нужно, если, допустим, функция принимает в качестве параметра объект типа `Stream<T>`, а у вас есть только объект типа `T`. Тогда вы можете легко и просто с помощью метода `of()` получить поток, состоящий из одного элемента.

Пример:

```java
Stream<Integer> stream = Stream.of(1);
```

**Метод `Stream<T> Stream.of(T obj1, T obj2, T obj3, ...)`**

Метод `of()` создает поток, состоящий из переданных элементов. Количество элементов может быть любым. Пример:

```java
Stream<Integer> stream = Stream.of(1, 2, 3, 4, 5);
```

**Метод `Stream<T> Stream.generate(Supplier<T> obj)`**

Метод `generate()` позволяет задать правило, по которому будет генерироваться очередной элемент потока при его запросе. Например, можно каждый раз отдавать случайное число.

Пример:

```java
Stream<Double> s = Stream.generate(Math::random);
```

**Метод `Stream<T> Stream.concat(Stream<T> a, Stream<T> b)`**

Метод `concat()` объединяет два переданных потока в один. При чтении данных сначала будут прочитаны данные из первого потока, а затем из второго. Пример:

```java
Stream<Integer> stream1 = Stream.of(1, 2, 3, 4, 5);
Stream<Integer> stream2 = Stream.of(10, 11, 12, 13, 14);
Stream<Integer> result = Stream.concat(stream1, stream2);
```

---

## 4. Фильтрация данных

Еще 6 методов создают новые потоки данных, позволяющие объединять потоки в цепочки разной сложности.

**Метод `Stream<T> filter(Predicate<T>)`**

Этот метод возвращает новый поток данных, который фильтрует данные из потока-источника согласно переданному правилу. Метод нужно вызывать у объекта типа `Stream<T>`.

Для задания правила фильтрации можно использовать лямбда-функцию, которая затем будет преобразована компилятором в объект типа `Predicate<T>`.

Примеры:

|Цепочки потоков|Пояснение|
|---|---|
|```java<br>Stream<Integer> stream = Stream.of(1, 2, 3, 4, 5);<br>Stream<Integer> stream2 = stream.filter(x -> (x < 3));<br>```|Оставляем только числа меньше трех|
|```java<br>Stream<Integer> stream = Stream.of(1, -2, 3, -4, 5);<br>Stream<Integer> stream2 = stream.filter(x -> (x > 0));<br>```|Оставляем только числа больше нуля|

**Метод `Stream<T> sorted(Comparator<T>)`**

Этот метод возвращает новый поток данных, который сортирует данные из потока-источника. В качестве параметра можно передать компаратор, который будет задавать правила сравнения двух элементов потока данных.

**Метод `Stream<T> distinct()`**

Этот метод возвращает новый поток данных, который содержит только уникальные данные из потока данных источника. Все дублирующиеся данные отбрасываются. Пример:

```java
Stream<Integer> stream = Stream.of(1, 2, 3, 4, 5, 2, 2, 2, 3, 4);
Stream<Integer> stream2 = stream.distinct(); // 1, 2, 3, 4, 5
```

**Метод `Stream<T> peek(Consumer<T>)`**

Этот метод возвращает новый поток данных, хотя данные в нем те же, что и в потоке источнике. Но когда запрашивается очередной элемент из потока, для него вызывается функция, которую вы передали в метод `peek()`.

Если в метод `peek()` передать функцию `System.out::println`, тогда все объекты будут выводиться на экран в момент, когда они будут проходить через поток.

**Метод `Stream<T> limit(int n)`**

Этот метод возвращает новый поток данных, который содержит только первые `n` данных из потока данных источника. Все остальные данные отбрасываются. Пример:

```java
Stream<Integer> stream = Stream.of(1, 2, 3, 4, 5, 2, 2, 2, 3, 4);
Stream<Integer> stream2 = stream.limit(3); // 1, 2, 3
```

**Метод `Stream<T> skip(int n)`**

Этот метод возвращает новый поток данных, который содержит все те же данные, что и поток-источник, но пропускает (игнорирует) первые `n` данных. Пример:

```java
Stream<Integer> stream = Stream.of(1, 2, 3, 4, 5, 2, 2, 2, 3, 4);
Stream<Integer> stream2 = stream.skip(3); // 4, 5, 2, 2, 2, 3, 4
```

ПРимер:
public static Stream<Language> sortByRanking(ArrayList<Language> languages) {  
    Stream<Language> stream = languages.stream()  
            .sorted(Comparator.comparingDouble(Language::getRanking).reversed());  
    return stream;  
}

