 
```
import java.util.Scanner; 
```

Чтобы использовать класс Scanner, создайте экземпляр класса, используя следующий синтаксис:

```
Scanner sc = new Scanner(System.in);
```


|Метод|Описание|
|---|---|
|```<br>nextByte()<br>```|Считывает данные и преобразует их в тип `byte`|
|```<br>nextShort()<br>```|Считывает данные и преобразует их в тип `short`|
|```<br>nextInt()<br>```|Считывает данные и преобразует их в тип `int`|
|```<br>nextLong()<br>```|Считывает данные и преобразует их в тип `long`|
|||
|```<br>nextFloat()<br>```|Считывает данные и преобразует их в тип `float`|
|```<br>nextDouble()<br>```|Считывает данные и преобразует их в тип `double`|
|```<br>nextBoolean()<br>```|Считывает данные и преобразует их в тип `boolean`|
||   |
|```<br>next()<br>```|Считывает одно «слово». Слова разделяются пробелами или enter|
|```<br>nextLine()<br>```|Считывает целую строку|

Есть еще методы, которые позволяют проверить тип еще не считанных данных (чтобы знать, каким методом их считывать).

|Метод|Описание|
|---|---|
|```<br>hasNextByte()<br>```|Там тип `byte`? Его можно будет преобразовать к `byte`?|
|```<br>hasNextShort()<br>```|Там тип `short`? Его можно будет преобразовать к `short`?|
|```<br>hasNextInt()<br>```|Там тип `int`? Его можно будет преобразовать к `int`?|
|```<br>hasNextLong()<br>```|Там тип `long`? Его можно будет преобразовать к `long`?|
||   |
|```<br>hasNextFloat()<br>```|Там тип `float`? Его можно будет преобразовать к `float`?|
|```<br>hasNextDouble()<br>```|Там тип `double`? Его можно будет преобразовать к `double`?|
|```<br>hasNextBoolean()<br>```|Там тип `boolean`? Его можно будет преобразовать к `boolean`?|
||   |
|```<br>hasNext()<br>```|Там есть еще одно слово?|
|```<br>hasNextLine()<br>```|Там есть еще одна строка?|

После использования сканер желательно закрыть командой **sc.close()**. На потоке System.in незакрытый сканер чаще всего ни на что не влияет, однако, в реальной практике, при работе с другими потоками (файл, драйвер, сетевой ресурс и т.д.) незакрытый сканер может привести к утечке памяти.

```java
import java.util.Scanner;
public class Solution {
   public static void main(String[] args)
   {
      Scanner console = new Scanner(System.in);
      String name = console.nextLine();
      int age = console.nextInt();

      System.out.println("Name: " + name);
      System.out.println("Age: " + age);
   }
}
```

---

##  Ввод данных из строки

Мы уже говорили выше, что класс `Scanner` умеет считывать данные из разных источников. И один из этих источников — строка текста.

Выглядеть это будет примерно так

```java
String str = "текст";
Scanner scanner = new Scanner(str);
```

Вместо объекта `System.in` мы при создании объекта типа `Scanner` передаем в него строку – `str`. И теперь объект `scanner` будет считывать данные из строки. Пример:

|Код программы:|Пояснение:|
|---|---|
|```java<br>import java.util.Scanner;<br>public class Solution {<br>   public static void main(String[] args)<br>   {<br>      String str = "10 20 40 60";<br>      Scanner scanner = new Scanner(str);<br>      int a = scanner.nextInt();<br>      int b = scanner.nextInt();<br><br>      System.out.println(a + b);<br>   }<br>}<br>```|// a == 10;<br>// b == 20;<br>На экран будет выведено: `30`|

---
Еще один важный метод, на который стоит обратить внимание — `useDelimiter()`. В этот метод передается строка, которую вы хотите использовать в качестве разделителя.

