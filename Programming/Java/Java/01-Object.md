# Object

## Порядок выполнения классов

Порядок следующий:

-   [Initialize(Инициализатор) - выполняется до конструктора]
-   [Constructir(Конструктор) - выполняется при создании объекта]
-   [main(точка входа) - является точкой входа для даного класса]

## Сравнение объектов

Для примитивов работают стандартные ==, <, > и т.д.

Для объектов сравнение происходит по другому:

-   [equals - эквивалентность](#object---equals)
-   [hashCode - расчет hash](#object---hashcode)
-   [compare - сравнение объектов](#object---compare)

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

## Overloading метода

Overloading(перегрузка) метода - когда метод с одним именем может принимать разный набор параметров

-   то какой метод исполнится зависит от переданных параметров

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

# Modifiers

Modifiers - модиикаторы, которые определяют поведение полей, объектов и методов

## Modifiers - Class

Access Modifier для Class:

-   Access Modifier
    -   public - доступен классам даже из других пакетов
    -   default(ничего не пишем перед именем класса) - доступен только классам из того же пакета
-   Non-Access Modifier
    -   final - от класса нельзя унаследоваться
    -   abstract - от класса можно унаследоваться, но нельзя создать объект данного класса

## Modifiers - Methods/Fields

Access Modifier для конструкторов, методов и полей:

-   Access Modifier
    -   public - доступно всем классам
    -   private - доступно только внутри класса, в котором объявлен
    -   default(ничгего не пишешь) - доступен только внутри этого пакета
    -   protected - доступен только внутри этого пакета, а так же в subclasses и superclasses
-   Non-Access Modifier
    -   final - не могут быть переопределены
        -   короче константы
        -   пишутся имена таких полей пишуться большими буквами
    -   static - аттрибуты и методы относятся к классу, а не объекту
        -   обращаться можно через Class и через Instance(new Class_name())
        -   внутри static метода нельзя использовать аттрибуты и методы без модификатора static
    -   abstract - только для методов и только в абстрактном классе
        -   Такой метод нельзя вызвать
        -   Можно только переопределять в наследнике
        -   у метода нет тела
    -   transient - что-то о сериализации
    -   synchronized - что-то о потоках
    -   volatile - что-то про кэширование

# Примеры

## Object - equals

equals - функция, которая возвращает true или false:

-   Обязательно нужно определять, иначе невозможно проверить одинаковы ли объекты
-   Не обязательно если equals==true, не обязательно, что compare==0

equals, как работает:

-   Проверяем, одинаковые ли ссылки объектов
    -   используется instanceof или getClass
-   не забыть super.equals(o)
-   выполнить преобразование типа
-   выполнить сравнение нужных полей
-   вернуть true или false

## Object - hashCode

hashCode - функция, возвращаю из объекта hash:

-   Обязательно нужно определять, иначе HashMap и подобное не заработает

## Object - compare

compare - функция сравнение, которае должно возвращать:

-   отрицательное - a меньше b
-   положительное - a больше b
-   ноль - a равно b

compare:

-   Обязательно нужно определять, иначе filter и подобное не заработает

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

## Class - Interface

Interface - специальная конструкция, в которой есть поля и методы, но нет реализации:

-   в отличии от Class может поддерживает множественное наследование
-   поля описанные в Interface, являются static
-   заданные значения для полей - только константы
-   значения по умолчанию:
    -   Поля по умолчанию - public static final
    -   Методы по умолчанию - public abstract

```java
<!-- Объявление интерфейса с константой -->
interface ExInterface {
    int number = 7;
}

<!-- Расширение класса: -->
class ClInterface implements Interface1, Interface2 {}
```

## Class - Interface default-methods

Default-методы - в интерфейсах начиная с JAVA8 появилась возможнось реализовывать метод внутри интефейса:

```java
public interface Car {
    public default void gas() {
        System.out.println("GAS!");
    }
}

public class Sedan implements Car {}
```

## Class - Abstract

Abstract Class - специальная конструкция, в которой есть поля и методы, но нет реализации:

-   не поддерживает множественное наследование
-   заданные значения для полей - НЕ константы
-   методы могут иметь только Modifier - abstract

```java
public abstract class AbstractMethod {
    abstract string doSomething();
}
```

#####

## Upcasting/Downcasting

прием в ООП, когда мы преобразуем parent в child(Downcasting) или child в parent(Upcasting)

Часто используется Downcasting, особенно Когда надо найти что-то в списке с разными типами элементов:

TextView myTextView = (TextView) findViewById(R.id.myText);

## Создать Class или Interface принимающий generic

нужно при создании класса дописать generic. Правила описания такие же как в wildCards, только вместо ? пишем название для переменной в которой будет храниться класс, когда мы его прокиним через Generic при создании объекта

```java
class GenericClass<T> {
    public T findSmall() {
        //code
    }
}
class GenericClass<T extends SomeClass> {
    // code
}
```

то же самое с интерфейсами:

```java
interface GenericInterface<T> {
    T findSmall();
    T findLargest();
}
Generic класс с имплементированным Generic интерфейсом:
class GenericClass<T> implements GenericInterface<T>{
    // code
}
```

# Collection

## HashMap(принцип работы)

hashMap - имплементирует интерфейс Map. Представляет из себя набор элементов у которых есть key, value и hash. Hash для повышения скорости работы. Принимает в качестве параметров initialCapacity(число Bundle'ов) и loadFactor(когда число элементов станет больше чем  Mathjax , то Capacity увеличится в двое и произойдет рехеширование)

Важно чтобы key был immutable. Если мы положим key в Map, а потом его изменим, то хэш перестанет совпадать и мы не сможем его найти

---

HashMap это Array. Элементы Array это LinkedList. В LinkedList(иногда еще называют Bucket) хранятся экземпляры объектов созданных из внутреннго класса Node. Класс Node хранит key, value и hash

Кладем элемент в HashMap:

1. Ч/з алгоритм из key вычисляем index. Алгоритм для index - остаток от деления hash на число Bucket'ов
2. По index достаем LinkedList(Bucket)
3. Пробегаемся по LinkedList и сравниваем hash ключей
   3.1 (hash'ы ключей равны) Сравниваем key ч/з метод equals
   3.1.1 (equals'ы ключей равны) Заменяем в Bucket на новую Node'у
   3.1.2 (equals'ы ключей не равны) Смотри 3.2
   3.2 (hash'ы ключей не равны) Пихаем в LinkedList(Bucket) экземпляр Node с определнным key, value и hash

---

HashMap.entrySet

```java
HashMap<String, Double> map = new HashMap<>();
for(Map.Entry<String, Double> item: map.entrySet()) {
    item.getKey();
    item.getValue();
    item.hashCode();
}
```

## HashSet

HashSet - class имплементирующий интерфейс set. Является просто множеством. В его основе лежит HashMap, у которого отсутствуют value, поэтому необходимо переопределять hashCode и equals

из специфичных методов(есть у всех set):

1. addAll( hashSet ) - делает union для двух set
2. retainAll( hashSet ) - делает intersect для двух set
3. removeAll( hashSet ) - делает substract для двух set

## HashTable

HashTable - тоже что и HashMap, за исключением того, что Synchronized и нельзя исользовать null. Настолько устарел, что вместо него следует использовать CuncurentHashMap

## LinkedHashMap

LinkedHashMap - тоже что и HashMap, за исключением того, что хранит порядок добавления/использования (зависит от инициализатора)

Хранит значения по порядку добавления:

```java
LinkedHashMap<ClassFirst, ClassSecond> = new LinkedHashMap<>(initialCapacity16,loadFactor0.75f,accessOrder false);
```

Хранит значения по порядку использования. Последний использованный элемент идет в конец:

```java
LinkedHashMap<ClassFirst, ClassSecond> = new LinkedHashMap<>(initialCapacity16,loadFactor0.75f,accessOrder true);
```

## LinkedHashSet

LinkedHashSet - множество, хранящее порядок добавления элементов. Наследник HashSet. В основе лежит HashMap. НЕ МОЖЕТ ЗАПОМИНАТЬ ПОСЛЕЖОВАТЕЛЬНОСТЬ ИСПОЛЬЗОВАНИЯ КАК LinkedHashMap

## ListIterator

особенностью данной Collection, является наличие методов hasPrevious() и previous(). А стандартные методов hasNext() и next(), дают возможность иттерировать из любого места и в любую сторону

## Map

Map - интерфейс, суть которого в хранении пары значени: key и value. Key - должен быть уникальным. Так же может быть null

НЕ НАСЛЕДНИК КЛАССА Collection, НО СХОЖ ПО ПРИНЦИПУ

```java
Map<String, Integer> map = new HashMap<>();
```

---

1. put(key, value) - добавит или заменит
2. putIfAbsent(key, value) - добавить если нет такого ключа
3. get(key) - получить значение
4. remove(key)
5. containsKey(key) - проверить есть ли такой ключ
6. containsValue(value) - проверить есть ли такое значение
7. keySet() - вернуть set всех ключей
8. values() - вернуть все значения

---

Наследники

от интерфейса Map реализуются такие классы как:

Map -> HashMap
-> LinkedHashMap
Map -> SortedMap
-> NavigableMap
-> TreeMap
Map -> HashTable

## PriorityQueue

PriorityQueue - очередь с приоритетом. Должен быть определен Comporator или интерфейс Comparable. Ибо очередь сравнивается и элемент с наименьшим значением обладает наивысшим приорететом

## Stack

Stack - классические stack с принципом LIFO. Является synchronized. Как и его предок Vector, устарел

Метода:

-   push
-   pop (взять элемент и удалить)
-   peek (взять элемент и НЕ удалять)
-   isEmpty

## TreeMap

TreeMap- Class реализующий интерфейс Map. В нем элементы - пары key и value. Хранятся в отсортированном(по key) возрастающем порядке в виде красно-черного дерева

Если используешь самописные классы, необходимо их унаследовать от интерфейса Compareble. Чтобы дерево могло сравниать и сортировать, иначе не заработает (equals и hash переопределять не надо)

или использовать Comparator при инициализации TreeMap:

```java
TreeMap<ClassKey, ClassValue> treeMap = new TreeMap<ClassKey, ClassValue>(
        new Comparator<ClassKey>() {
            /**
             *
             * @param o1 первый объект для сравнения.
             * @param o2 второй объект для сравнения.
             * @return если int < 0, то первый объект меньше
             *         если int = 0, то равны
             *         если int > 0, то первый объект больше
             */
            @Override
            public int compare(ClassKey o1, ClassKey o2) {
                return 0;
            }
        }
);
```

1. descendingMap() - от наименьшего к наибольшему
2. tailMap( fromKey ) - все элементы key которых больше чем fromKey
3. headMap( fromKey ) - все элементы key которых меньше чем fromKey
4. fitstEntry() - возвращает первый элемент
5. lastEntry() - возвращает последний элемент

## TreeSet

TreeSet - множество, которое хранит элементы в отсортированном по возрастанию порядке. Поскольку в основе лежит TreeMap, то необходимо compareTo или Comparable

Методы(сильно схожи с TreeMap):

1. first
2. last
3. headSet( item )
4. tailSet( item )
5. subSet( item1, item2 )

## Коллекции для работы с многопоточностью

1. Synchronized collections - получены из оборачиванием обычных коллекций
2. Concurrent collections - изначально созданны для работы с многопоточностью
