## Метод `Math.random()`
 Общий вид его вызова выглядит так:

```java
Math.random()
```

Этот метод не принимает никаких параметров, но возвращает результат — псевдослучайное вещественное число в диапазоне от `0` до `1`. Единица при этом в диапазон не входит.
Но что, если вам этот метод не очень подходит, а вы хотите, допустим, написать программу, которая имитирует выбрасывание кубика с шестью гранями. Как получить случайные целые числа в диапазоне 1..6, а не вещественные в диапазоне 0..1?

Это на самом деле довольно просто.

Сначала нужно превратить диапазон `[0,1)` в `[0, 6)`. Для этого нужно просто умножить результат функции `random()` на `6`. Ну а чтобы получить целые числа, нужно это все округлить. Если требуются именно числа из набора `1,2,3,4,5,6`, нужно просто ко всем случайным числам добавлять единицу

## Класс `Random`
**Метод `double nextDouble()`**

Этот метод возвращает случайное вещественное число в диапазоне `0.0` – `1.0`. Очень похоже на метод `Math.random()`. И ничего удивительного, ведь метод `Math.random()` просто вызывает метод `nextDouble()` у объекта типа `Random`.

**Метод `float nextFloat()`**

Метод очень похож на метод `nextDouble()`, только возвращаемое случайное число типа `float`. Оно также лежит в диапазоне `0.0` – `1.0`. И, как всегда, в Java диапазон не включает число `1.0`.

```java
Random r = new Random();
float f = r.nextFloat();
```

**Метод `int nextInt(int max)`**

Этот метод возвращает случайное целое число в диапазоне `[0, max)`. `0` входит в диапазон, `max` — не входит.

Т.е. если вы хотите получить случайное число из набора `1, 2, 3, 4, 5, 6`, вам нужно будет прибавить к полученному случайному числу единицу:

```java
Random r = new Random();
int x = r.nextInt(6) + 1;
```

**Метод `int nextInt()`**

Этот метод аналогичен предыдущему, но не принимает никаких параметров. Тогда в каком же диапазоне он выдает числа? От `-2 миллиарда` до `+2 миллиарда`.

Ну или если точнее, от `-2147483648` до `+2147483647`.

**Метод `long nextLong()`**

Этот метод аналогичен методу `nextInt()`, только возвращаемое значение будет из всего возможного диапазона значений типа `long`.

**Метод `boolean nextBoolean()`**

Этот метод возвращает случайное значение типа `boolean`: `false` или `true`. Очень удобно, если нужно получить длинную последовательность случайных логических значений.

**Метод `void nextBytes(byte[] data)`**

Этот метод ничего не возвращает (тип `void`). Вместо этого он заполняет переданный в него массив случайными значениями. Очень удобно, если нужен большой буфер, заполненный случайными данными.

**Метод `double nextGaussian()`**

Этот метод возвращает случайное вещественное число в диапазоне `0.0` — `1.0`. Вот только числа в этом диапазоне распределены не равномерно, а подчиняются нормальному распределению.

Числа ближе к середине диапазона (`0.5`) будут выпадать чаще, чем значение по краям диапазона.




