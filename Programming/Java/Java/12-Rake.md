# Java - неочевидное поведение

# Упаковка примитивов

Когда примитивы/объекты попадают в пул, а когда нет:

-   [Числовые примитивы](#упаковка-примитивов---числа)

# Примеры

## Упаковка примитивов - Числа

Когда числовые объекты попадают в пул, а когда нет:

-   для объектов типа Byte, гарантия, что ссылки на объекты одинаковы если значения одинаковые
-   для объектов типа Integer, гарантия, что ссылки на объекты одинаковы если значения от -128 до 127
    -   в стандерте есть пометка, что диапозон может быть больше, но не обязательно

```java
public static void main(String[] args) {
        System.out.println(Byte.valueOf((byte) 48)   == Byte.valueOf((byte) 48));  // => true
        System.out.println(Byte.valueOf((byte) 248)  == Byte.valueOf((byte) 248)); // => true
        System.out.println(Integer.valueOf(48)       == Integer.valueOf(48));      // => true
        System.out.println(Integer.valueOf(248)      == Integer.valueOf(248));     // => false
}
```

## Упаковка примитивов - Строк

Когда строковые объекты попадают в пул, а когда нет:

-   на этапе компиляции - все строковые литералы попадают в один пул
-   на этапе компиляции - все строковые полученные сложением попадают в один пул
-   в runtime - строки созданные во время выполнеиния НЕ попадают в пул
-   метод intern у строк
    -   если такой строки в пуле нет, то кладет строчку в пул и возвращает ссылку на него
    -   если такая строка в пуле есть, то возвращает ссылку на объект из пула
    -   в интернете сказано, что это выгодно при частых поисках одних и тех же значений

```java
//In File Other.java
package other;
public class Other { public static String hello = "Hello"; }

//In File Test.java
package testPackage;
import other.*;
class Test{
  public static void main(String[] args) {
    String hello = "Hello", lo = "lo";
    System.out.print((testPackage.Other.hello == hello) + " "); // => true
    System.out.print((other.Other.hello == hello) + " ");       // => true
    System.out.print((hello == ("Hel" + "lo")) + " ");          // => true
    System.out.print((hello == ("Hel" + lo)) + " ");            // runtime => false
    System.out.println(hello == ("Hel" + lo).intern());         // => true
  }
}
class Other { static String hello = "Hello"; }
```
