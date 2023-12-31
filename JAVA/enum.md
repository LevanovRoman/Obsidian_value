тип данных, который состоит из конечного набора значений — `enum`.
## Объявление типа

Выглядит объявление нового `enum` типа данных так:

```java
enum ИмяТипа
{
   ЗНАЧЕНИЕ1,
   ЗНАЧЕНИЕ2,
   ЗНАЧЕНИЕ3
}
```

Где `ИмяТипа` — это имя нового типа (класса), а в скобках через запятую перечислены возможные значения: `Значение1`, `Значение2`, `Значение3`.

создадим свой `enum` для типа `ДеньНедели`:

|Код|Примечание|
|---|---|
|```java<br>enum Day<br>{<br>   MONDAY,<br>   TUESDAY,<br>   WEDNESDAY,<br>   THURSDAY,<br>   FRIDAY,<br>   SATURDAY,<br>   SUNDAY<br>}<br>```|Новый тип `Day`  <br>  <br>Понедельник  <br>Вторник  <br>Среда  <br>Четверг  <br>Пятница  <br>Суббота  <br>Воскресенье|

Присваивать значения переменным этого типа можно таким образом:

```java
Day day = Day.MONDAY;
```

Пример:

|Код|Примечание|
|---|---|
|```java<br>Day day = Day.FRIDAY;<br>System.out.println(day);<br>```|На экран будет выведено:  <br><br>```<br>FRIDAY<br>```|

Присваивать значения переменным этого типа можно таким образом:

```java
Day day = Day.MONDAY;
```

Пример:

|Код|Примечание|
|---|---|
|```java<br>Day day = Day.FRIDAY;<br>System.out.println(day);<br>```|На экран будет выведено:  <br><br>```<br>FRIDAY<br>```|



