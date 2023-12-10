# Object

## Порядок выполнения классов

Порядок следующий:

-   Initialize(Инициализатор) - выполняется до конструктора
    -   Это задание значений переменным в классе
-   Constructor(Конструктор) - выполняется при создании объекта
    -   в нем задаются значения для констант(final)
-   main(точка входа) - является точкой входа для данного класса

## Overloading

Overloading(перегрузка) метода - когда метод с одним именем может принимать разный набор параметров

-   то какой метод исполнится зависит от переданных параметров

## Сокрытие

Сокрытие - старый метод остается в памяти программы, и если такой метод вызвать через Super, выполнится сокрытый метод

-   все static - становятся сокрытыми

## Enum

Enum - перечисление. Чтобы не писать класс с кучей static полей придумали enum. В простейшем случае:

```java
public enum Enum {
    // К enum можно обращатся в стиле: Enum.DOG == Enum.FROG   ->   False
    CAT, DOG, WOLD, FROG
}
```

Но все немного хитрее, и каждое такое поле в enum реализует class описываемым этим enum. Например:

```
enum Animal {
    // Данные константы просто static class, которые реализуют методы описанные ниже
    CAT, DOG("Собака"), WOLF("Волк"), FROG;

    private String name;
    static final String DEFAULT_NAME = "NoName";

    Animal() {
        this.name = DEFAULT_NAME;
    }

    Animal(String name) {
        this.name = name;
    }
}
```

## Interface

Реализует то, какие поля должны быть в классе. Опрелить в Interface можно ТОЛЬКО константы. Можно определить default methods:

-   [Interface - основые заметки](#class---interface)
-   [default методы в интерфейсах](#class---interface-default-methods)

## Abstract Class

Реализует то, какие поля должны быть в классе. Опрелить в Abstract Class, можно любое состояние. Нельзя определить methods:

-   [Abstract - основные заметки](#class---abstract)

## Anonim Class

Класс созданный "находу", для создания одного объекта:

```java
Dog dog = new Dog() {
    public void newFuncInsideAnonimClass() {
        System.out.println("Так создается объект анонимного класса в котором переопределяют все что захотят");
    }
};
```

## Setter/Getter

Setter/Getter - поля нельзя читать/писать и добавляются функции getter/setter, которые правильно обрабатывают данные:

```java
public class Cat {
    private String name;

    public void getName(String name) {
        this.name = name;
    }

    public void setName() {
        return this.name;
    }
}
```

Автоматические Setter/Getter:

```java
public class Cat {
    @Getter
    @Setter
    private String name;
}
```

# Generic

Generic - способ задать тип из вне. Это можно объяснить только на примерах :(

## Generic

Generic - позволяют при создании класса, передать нужные типы, которые будут автоматически подставлятся внутри класса:

-   создание класса с generic выглядит так: GenericClass<String> someVariable = new GenericClass<>(10);

Generic для класса:

```java
// просто generic
class GenericClass<T> {
    public T findSmall() {
        //code
    }
}

// generic с использованием wildcards
class GenericClass<T extends SomeClass> {
    public T findSmall() {
        //code
    }
}
```

Generic для интерфейса:

```java
// просто generic
interface GenericInterface<T> {
    T findSmall();
    T findLargest();
}

// generic класс с имплементированным generic интерфейсом
class GenericClass<T> implements GenericInterface<T>{
    // code
}
```

## Wildcards

возможность использовать generic в качестве аргумента функции или метода

WildCard наследников класса:

```java
// функция принимает list объектов класса Mammals и его наследников(Cat и Dog)
private static void test3(List<? extends Mammals> mammals) {}
```

WildCard объектов конкретного класса:

```java
// функция принимает list объектов класса Dog
private static void test(List<Dog> dogs) {}
```

WildCard объектов любого класса:

```java
// функция принимает list объектов любого класса
private static void test2(List<?> objects) {}
```

WildCard предков класса:

```java
// функция принимает list объектов являющимися предками класса Mammals(Animals и Object)
private static void test4(List<? super Mammals> animals) {}
```

# Примеры

## Object - initialize

Инициализатор - выполняется до конструктора:

```java
{
    type = "Кот";
    age = 0;
}
```

## Object - constructor

Конструктор - функция с тем же теменем что и класс:

-   Выполняется при создании объекта
-   Его монжо перегрузить(overloading), что очень полезно
-   Если конструктор не прописан, то автоматически создается такой, что все поля заполняет либо null либо 0:

```java
Pet() {
    name = null;
    type = null;
    age = 0;
}
```

## Object - main

main - эта штука является точкой входа для даного класса:

-   если вызвать Class, из консоли, то запуститься метод main

```java
public class Class {
    public static void main(String[] args) { }
}
```

#####

## Upcasting/Downcasting

прием в ООП, когда мы преобразуем parent в child(Downcasting) или child в parent(Upcasting)

Часто используется Downcasting, особенно Когда надо найти что-то в списке с разными типами элементов:

TextView myTextView = (TextView) findViewById(R.id.myText);
