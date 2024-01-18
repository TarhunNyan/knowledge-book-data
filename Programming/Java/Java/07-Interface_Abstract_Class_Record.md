# Interface/Abstract/Class/Record

Interface/Abstract/Class/Record - основа основ java

# Class

## Стрктура Class

Class, разделен на несколько основных частей:

-   поля
-   конструкторы
-   методы

## Конструкторы

Конструкторы class бывают:

-   static
    -   срабатывают при подгрузке класс
    -   нужен для определния значений в статических переменных
-   non-static
    -   срабатывают при создании экземпляра класса

```java
public class Main {

    static private String someStaticField;
    final private String someField;

    // статический конструктор
    static {
        someStaticField = "value";
    }

    // конструктор без переменных
    public Main() {
        this.someField = "";
    }

    // конструктор с переменными
    public Main(value) {
        this.someField = value;
    }
}
```

# Enum

# Records

Records - появился начиная с java14(в режиме preview). Является классом с набором final полей:

```java
record Point(int x, int y) {}
```

Код аналогичный коду выше, но через Class:

-   очень странное решение вместо классического getX, исользовать x

```java
public class Point() {

    private final int x;
    private final int y;

    Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public int x() { return this.x; }

    public int y() { return this.y; }

    public toString() { ... }
    public equals() { ... }
    public hashCode() { ... }
}
```

# Interface и Abstract Class

Interface - служат для типизирования методов класса:

-   синтаксически:
    -   интерфейсы - типизируют методы класа
-   логически:
    -   интерфейсы - создают контракт, в котором описано, что обязан реализовывать класс
    -   логически это настольеко важаная вещь, что существуют библиотеки состоящие только из интерфейсов

Abstract Class - абстрактный клас, описывает состояние полей

-   синтаксически:
    -   интерфейсы - типизируют методы класа и состояния полей
-   логически:
    -   определяет - начальное состояние полей класса

## Interface

Interface - служат для типизирования методов класса:

-   поля:
    -   по умолчанию: public static final
    -   поля описанные в Interface, являются static
    -   заданные значения для полей - только константы
-   методы:
    -   по умолчанию: public abstract
    -   в современной java, через модификатор default, можно задать методам - поведение по умолчанию
-   в отличии от Class поддерживает множественное наследование

```java
// Объявление интерфейса с константой
interface Interface1 {
    int number = 7;
}

// Реализация стандартного поведения у метода
public interface Interface2 {
    public default void gas() {
        System.out.println("GAS!");
    }
}

// Расширение класса от нескольих интерфейсов
class ClInterface implements Interface1, Interface2 {}
```

## Class - Abstract

Abstract Class - специальная конструкция, в которой есть поля и методы, но нет реализации:

-   поля:
    -   по умолчанию: все как в обычном классе
    -   заданные значения для полей - НЕ константы
-   методы:
    -   по умолчанию: все как в обычном классе
    -   в современной java, через модификатор default, можно задать методам - поведение по умолчанию
    -   методы(кроме конструктора) могут быть реализованны, и тогда это будет считаться реализацией по умолчанию
    -   методы могут иметь модификатор abstract, тогда у них нет тела, и его должен реализовывать наследник
-   не поддерживает множественное наследование

```java
public abstract class AbstractMethod {
    abstract string doSomething();
}
```

## Как использовать Interface и Abstract?

Как использовать Interface и Abstract?

-   По опыту
    -   просто забудь о существовании Abstract Class и используй только Interface
    -   это связано с тем, что многие библиотеки, работают на механизмах Interface
    -   внедрение Abstract классов, ломает логику работы библиотек
-   По логике
    -   создаем множество нужных Interface
    -   имплементируем все нужные Interface в Abstract Class
    -   в Abstract Class определяем начальные значения
    -   создаем нелобходимый класс унаследованный от Abstract Class

## Methods

Контракт метода - спецификация метода и тип возвращаемого значения

```java
public void moveTo(int x, int y) throws IOException
```

Метод, который возвращает значение:

```java
// примитивный тип:
double oldAge() {
    return 15d;
}

// любой класс:
ClassIncapsulation example() {
    return new ClassIncapsulation();
}

// пример void: Метод, который ничего не возвращает
void displayInfo() {
    System.out.println( "Метод вызван" );
}
```

Методы в JAVA классе:
пишется модификатор. Тип возвращаемого значения(void - ничего не возвращает). Имя метода. Аргументы метода. Тело метода

```java
public static void methodName(String agr1) { /*тело*/ }
```

Сигнатура метода:

-   имя и параметры метода

```java
moveTo(int x, int y)
```
