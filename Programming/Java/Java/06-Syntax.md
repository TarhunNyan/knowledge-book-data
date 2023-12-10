# Java

Java - это база, это знать надо

# Структуры

## Объявление переменных

Объявление переменных:

<!-- -   [var - глобальная переменная](#объявление-переменных---var)
-   [let - локальная переменная](#объявление-переменных---let)
-   [const - константа](#объявление-переменных---const) -->

## Условия

Условия бывают:

-   [Условие типа: if ... else if ... else](#условия---ifelse)
-   [Условие типа: switch](#условия---switch)
-   [Тернарный оператор](#условия---тернарный-оператор)

## Циклы

Циклы while:

-   [Классический while](#цикл---while)
-   [Классический do...while](#цикл---dowhile)

Циклы for:

-   [Классический for](#цикл---for)
-   [Классический forEach](#цикл---foreach)

Break, continue и label:

-   [Break](#цикл---break)
-   [Continue](#цикл---continue)
-   [Label](#цикл---label)

## Ошибки

Ошибки отлавливаются через try...catch...finally:

```java
try {
    // код
} catch (Exception e) {
    // поймали ошибку
} finally {
    // выполнится вне зависимости от наличия ошибки
}
```

# Типы данных

## Примитивные типы

Примитивные типы - НЕ ссылочные типы:

-   [Целые числа - byte, short, int, long](#примитивные-типы---byte-short-int-long)
-   [Числа с плавающей точкой - float, double](#примитивные-типы---float-double)
-   [Логический - boolean](#примитивные-типы---boolean)
-   [Символьный - char](#примитивные-типы---char)

## String

String - строка. Создать строку:

-   [Создать строку](#string---создать-строку)

## Array

Array - массив, длина которого не изменяется. Создать массив:

-   [Создать массив с указанным типом](#array---создать)
-   [Создать массив с конкретными значениями](#array---создать-со-значениями)
-   [Создать двумерный массив](#array---создать-двумерный-массив)

# Object

Object - так получается, что примитивные-типы, string и array, скорее исключение. Обычно все типы данных в Java - это Object:

-   [Object - базовый для всех объект объект](#object---object)

## Object - Object

Object - class в java, базовый для всех объектов

-   clone - protected Object clone()
    -   Вернет копию объекта
    -   у разных классов может быть разная реализация
-   equals - boolean equals(Object obj)
    -   Веренет то, одинаковы ли объекты внутри
    -   [Смотри тут](#object---сравнение-примитивовобъектов)
-   finalize - protected void finalize()
    -   этот метод вызвается перед удалением Объекта сборщиком мусора
    -   сборщик мусора вызывается System.gc()
    -   не рекомендуется переопределять этот метод
-   getClass - Class<T> getClass()
    -   веренет class, который сейчас у объекта(upcasting/downcasting)
    -   [отлчие getClass от obj.class](#object---getclass-vs-class)
    -   элемент Reflection API
-   hashCode - int hashCode()
    -   метод возвращаюищий hashcode объекта
    -   необходимо переопределять ручками
    -   [Смотри тут](#object---сравнение-примитивовобъектов)
-   toString - String toString()
    -   возвращает объект в формате строки

Методы object связанные с потоками:

-   notify - void notify()
    -   выводить поток поставленный на wait из состояния заморозки, говорит что поток может продолжать работу
        -   может быть вызван только у синхронизированного объекта
-   notifyAll - void notifyAll()
-   wait
    -   void wait()
        -   замараживаем работу потока, до тех пор пока не будет вызван notify
        -   может быть вызван только у синхронизированного объекта
        -   пример
            -   есть два потока
            -   в одном мы рисуем, в другом печатаем рисунок
            -   если мы закончили печатать, а рисовать еще не закончили, то поток печати должен будет спрашивать каждые n секунд, а закончил ли второй поток рисовать
            -   чтобы этого не делать, у потока печати можно вызвать wait, чем поставить его на ожидание
    -   void wait(long timeout)
        -   может быть вызван только у синхронизированного объекта
        -   замараживаем работу потока, до тех пор пока не будет вызван notify, или не закончется timeout в мс
    -   void wait(long timeout, int nanos)
        -   может быть вызван только у синхронизированного объекта
        -   замараживаем работу потока, до тех пор пока не будет вызван notify, или не закончется timeout в мс и nanos в нс

## Object - Сравнение примитивов/объектов

Для примитивов работают стандартные ==, <, > и т.д.

Для объектов сравнение происходит по другому:

-   [equals - эквивалентность](#object---equals)
-   [hashCode - расчет hash](#object---hashcode)
-   [compare - сравнение объектов](#object---compare)

# Функциональщина

## Lambda и Functional Interface

Functional Interface - функциональный интерфейс, это Interface с одним методом:

-   [Пример Functional Interface](#functional-interface---пример-functional-interface)

Список встроенных Functional Interface:

-   [Predicate](#functional-interface---predicate)
-   [UnaryOpertor](#functional-interface---unaryopertor)
-   [BinaryOperator](#functional-interface---binaryoperator)
-   [Function](#functional-interface---function)
-   [Consumer](#functional-interface---consumer)
-   [Suplier](#functional-interface---suplier)
-   [Predicate](#functional-interface---predicate)

Lambda - короткая запись Functional Interface или по-другому анонимная функция без класса:

-   появилась в Java8
-   этот синтаксис - обертка над Funciton Interface

Пример Lambda:

-   [Пример Lambda функции](#functional-interface---пример-lambda)

## Stream

Stream - позволяет проходить по объекту, как в функциональных языках:

-   существует распараллеливание Stream, но в инттернете пишут, что это дает + производительности от 10К элементов:
    -   stream.parallel() - превращаем Stream в паралельный
    -   collection.parallelStream() - превращаем Collection в паралельный Stream
-   состоит из 3 шагов:
    -   источник - сама Collection
    -   intermidiate(промежуточные) - возвращают stream:
        -   skip - пропускаем n-элементов
        -   limit - ограничивает Stream по числу элементов
        -   dropWhile - пропускает элементы которые проходят условие Predicate
        -   distinct - убирает повторяющиеся элементы
        -   [filter](#stream---filter) - возвращает только значения прошедшие проверку функции типа Predicate
        -   [sorted](#stream---sorted) - сортрует Stream
        -   [map](#stream---map) - ставит элемент в соответствие по функции типа Function<T, R>
        -   [peek](#stream---peek) - берет из Stream объект, пропускает через переданную функцию, и возвращет его
        -   flatMap - переданная во flatMap функция должна возвращать Stream. Ставит элемент в соответствие по функции типа Function<T, Stream<R>>
    -   terminal(терминальный) - конечный оператор, который и запускает обработку Stream
        -   forEach - производим действие над объектом ничего не возвращая. Принимает функцию типа Consumer
        -   count() - возвращает колличество элементов Stream
        -   [collect](#stream---collect) - собирает Stream в одну Collection. Принимает функцию типа Collector
        -   [reduce](#stream---reduce) - классика по последовательному применению к парам значениям
        -   min/max/findFirst - возвращает минимальный/максимальный/первый элемент
        -   allMatch/anyMatch/noneMatch - элементы все-удовлетворяют/хотя-бы-один/все-не-удовлетворяют условию типа Predicate

Примеры:

-   [Создаем Stream из разных объектов](#stream---create)
-   [Соединить два Stream](#stream---concat)

## Optional

Optional - для избегания проверочных условий был создан этот Class:

-   [Создание объекта типа Optional(of/ofNullable/empty)](#optional---создание)
-   [Проверка пусто ли внутри Optional(isPresent)](#optional---ispresent)
-   [Сделать что-то если в Optional есть значение(ifPresent)](#optional---ifpresent)
-   [Проверка на пустоту(orElse/orElseGet/orElseTrow)](#optional---orelse)
-   [Применить функцию к значению внутри Optional(map/flatMap)](#optional---map)

# Синтаксический сахар

Обработка undefined:

-   [Если false, то новое значение](#syntaxsugar---double-pipe)
-   [Если undefined, то новое значение](#syntaxsugar---double-qestion)

# Многопоточность

notify и wait

# Важные вещи

Важные вещи:

-   [Вывод в косоль](#важные-вещи---consolelog)
<!-- -   [Комментарии](#важные-вещи---комментарии) -->

# Примеры

## Object - equals

equals - функция, которая возвращает true или false:

-   По умолчанию, проверяет на равенство ссылки, как и ==
-   Обязательно нужно определять, иначе невозможно проверить одинаковы ли объекты
-   Не обязательно, что если equals==true, не обязательно, что compare==0

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

Compare - сравнение офьъектов. Возможно определить двумя путями:

-   Comparator - функциональнй интерфейс с методом compare, который сравнивает два любых объекта
-   Comparable - интерфейс с методом compareTo, который говорит что этот объект можно сравнивать с другими объектами того же типа
    -   Обязательно нужно определять, иначе filter и подобное не заработает

Методы Comparator и Comparable, работают по такому принципу:

-   отрицательное число, если a меньше b
-   положительное число, если a больше b
-   ноль, если a равно b

## Object - getClass vs class

И getClass и class, возвращают класс объекта. Но он у объекта не одинаковый:

-   getClass - веренет Class, который сейчас у объекта в runtime
-   class - вернет Class, определенн в сигнатуре поля

```java
Animal animal = new Monkey();
animal.getClass() => Monkey
animal.class => Animal
```

## Optional - isPresent

isPresent - проверяет есть ли значение:

-   возвращает true или false

```java
Optional<String> opt = Optional.empty();
opt.isPresent();
```

## Optional - ifPresent

ifPresent - если в Optional есть значение, то что-то выполняет функцию:

```java
Optional<String> opt = Optional.empty();
opt.ifPresent(() -> System.out.println("is not empty"));
```

## Optional - orElse

orElse - вернуть значение если Optional пустой:

```java
Optional<String> opt = Optional.empty();
opt.orElse("is empty");
```

orElseGet - вернуть результат функции, если Optional пустой:

-   Функция отрабатывает до места, где она

```java
Optional<String> opt = Optional.empty();
opt.orElseGet(() -> "is empty");
```

orElseThrow - бросить исключение, если Optional пустой:

```java
Optional<String> opt = Optional.empty();
opt.orElseTrow(() -> new RuntimeException());
```

## Optional - map

map - применяет функцию к значению внутри Optional, если оно там есть:

```java
Optional<String> opt = Optional.of("String");
Optional<String> res = opt.map((el) -> String.toLowerCase(el));
```

flatMap - применяет функцию к значению внутри Optional, если оно там есть:

-   отличие от map в том, что результат не оборачивается в Optional

```java
Optional<String> opt = Optional.of("String");
String res = opt.flatMap((el) -> String.toLowerCase(el));
```

## Optional - Создание

ofNullable - создаем обертку вокруг переданного объекта:

-   отличительная черта, может принимать null

```java
Optional.ofNullable(person);
```

of и empty - обчыно используются в связке:

-   в примере ищем login, если не находим то возвращаем пустой Optional

```java
public Optional<Person> findByLogin(String login) {
    for(Person person : persons.value()) {
        if(person.getLogin().equals(login)) {
            return Optional.of(person);
        }
    }
    return Optional.empty();
}
```

## Stream - Reduce

Reduce - классический Reduce, который применяется к элементам Stream:

-   10 - начальный элемент

```java
Stream.of(1, 2, 3, 4, 5).reduce(10, (acc, x) -> acc + x);
```

Тот же Reduce, но начальный элемент - первый элемент:

-   первый элемент массива становиться начальным

```java
Stream.of(1, 2, 3, 4, 5).reduce((acc, x) -> acc + x);
```

## Stream - Collect

Stream превращаем в List:

```java
list.stream().collect(Collectors.toList())
```

Stream превращаем в Set:

```java
list.stream().collect(Collectors.toSet())
```

Stream превращаем в Map:

```java
list.stream().collect(Collectors.toMap( el -> el[0], el -> el[1]))
```

Stream превращаем в строку:

```java
list.stream().collect(Collectors.joining())
// => "abc"
list.stream().collect(Collectors.joining(", "))
// => "a, b, c"
list.stream().collect(Collectors.joining(", ", "[", "]"))
// => "[a, b, c]"
```

## Stream - create

Пустой Stream:

```java
Stream.empty();
```

Stream из List:

```java
list.stream();
```

Stream элементов Map:

```java
map.entrySet.stream();
```

Stream из Array:

```java
Arrays.stream(array);
```

Stream из элементов:

```java
Stream.of("1", "2", "3", "4");
```

Strem из integer:

```java
IntStream.range(0, x);
```

Stream бесконечный stream:

-   generateNext - функция которая вызывается и значение которой попадает Stream

```java
Stream.generate(new Obj()::generateNext);
```

## Stream - filter

filter - возвращает только значения прошедшие проверку:

```java
ArrayList<String> arrayList = new ArrayList<>();
arrayList.add("aTraction");
arrayList.add("bInary");
arrayList.add("aOpsadlka");

arrayList.stream().filter(
        el -> el.charAt(0)=='a' && el.length()==9
);
```

## Stream - forEach

forEach - пробегаемся по Stream и что-то делаем, но ничего не возвращает. Например можно что-либо вывести:

```java
Arrays.stream(array).forEachOrdered(el -> System.out.println(el * 2));
```

## Stream - map

map - применяет переданную функцию последовательно к каждому элементу

-   [peek](#stream---peek) - в peek, не нужно указывать что вернуть. Вернет тот же объект

```java
ArrayList<String> arrayList1 = new ArrayList<>();
arrayList1.add("privet");
// ...
arrayList1.add("ok");

ArrayList<Integer> arrayList2 = (ArrayList<Integer>) arrayList1.stream()
                                                                .map(el -> el.length())
                                                                .collect(Collectors.toList());
```

## Stream - peek

peek - берет объект, пропускает через переданную функцию, и возвращает его. :

-   суть peek - в модифицировании состояния объекта
-   [map](#stream---map) - в map нужно указывать что вернуть. И вернуть можно что угодно

```java
Stream.of(new Address("London"), new Address("Moscow"))
  .peek(e -> e.city = e.city + "!")
  .collect(Collectors.toList());
```

## Stream - reduce

reduce - возвращает из набора элементов один, путем кумулятивного(накопительно) применения функции к последовательнсоти. По факту возвращает Optional, это связано с тем что может reduce может вернуть null:

```java
int result = 0;
Optional<Integer> reduce = list.stream().reduce((accumulator, el) -> accumulator*el);
if(reduce.isPresent()) {
    result = reduce.get();
} else {
    // значение Stream.reduce null... Короче говно случилось
}
```

## Stream - Sorted

Sorted - сортрует по возрастанию

-   можно передать функцию Comparator

```java
Stream.of(2, 3, 1, 0, 5, 4).sorted()
// => 0, 1, 2, 3, 4, 5
Stream.of(2, 3, 1, 0, 5, 4).sorted((a, b) -> { return b-a})
// => 5, 4, 3, 2, 1
```

## Stream - Concat

Concat - соединяет два Stream последовательно:

-   в качестве аргументов идет другой Stream:

```java
Stream.concat(str1, str2);
```

## Functional Interface - Predicate

Predicate<T> - проверка соблюдения условия:

```java
Prdicate<Integer> isPositive = x -> x > 0;
```

## Functional Interface - UnaryOpertor

UnaryOpertor<T> - выполняет над объектом операцию:

```java
UnaryOpertor<Integer> square = x -> x*x;
```

## Functional Interface - BinaryOperator

BinaryOperator<T> - выполняет над 2-мя объектами операцию:

```java
BinaryOperator<Integer> multiply = (x, y) -> x*y;
```

## Functional Interface - Function

Function<T, S> - функция перехода от T к S:

```java
Function<Integer, String> convert = x -> String.valueof(x);
```

## Functional Interface - Consumer

Consumer<T> - выполняет операцию над объектом, ничего не возвращает:

```java
Consumer<Integer> printer = x -> System.out.print(x);
```

## Functional Interface - Suplier

Suplier<T> - ничего е принимает, но возвращает объект типа T:

```java
Suplier<User> user = () -> new User(
    new Scanner(System.in).nextLine()
);
```

## Functional Interface - Пример Functional Interface

Пример Functional Interface:

-   @FunctionalInterface - аннотация, не позволяющая задать более одного метода

```java
@FunctionalInterface
interface MyInterface { String reverse(String n); }

public class main {
    public static void main() {
        MyInterface res = (s) -> s.reverse()
    }
}
```

## Functional Interface - Пример Lambda

Пример Lambda:

```java
// без аргументов
() -> System.out.println("Hello")


(int var1, int var2, int var3) -> var+var2+var3

(int var1, int var2, int var3) -> {
    return var1 + var2 + var3
}
// можем не указывать тип, если исполььзуем Functional Interface
SomeInterface res = s -> s.length
```

## Method Refrence

Method Reference - получаем ссылку на метод:

-   в примере каждый элемент list выведем через System.Out.pringtln в консоль

```java
list.forEach(System.out::println);
```

## Array - Создать со значениями

Создает массив из указаных значений:

```java
String[] sStrings = { "string A", "string B" };
```

## Array - создать двумерный массив

Создаем двумерный Array:

```java
int[][] sNumbers = {
     { 1, 2, 3, 4, 5},
     { 4, 5 },
     { 7, 8, 9}
};
System.out.println( sNumber[0][2] );
```

## Array - Создать

Создает массив из целых числе, длиной 5(заполнен нулями):

```java
int[] fNumbers = new int[5];
```

## String - создать строку

Строка создается так:

```java
String sVar = "значение";
// или инициализируем как объект
String sVar = new String("значение");
```

## Условия - if/else

Стандартное условие:

```java
if (5 > 10) {
    // блок кода
} else if (5 < 0) {
    // блок кода
} else {
    // блок кода
}
```

## Условия - тернарный оператор

Условие в одну строку:

-   age > 18 - логическое выражение
-   "Совершеннолетний" - переменная var будет "Совершеннолетний", если age > 18 вернет true
-   "Несовершеннолетний" - переменная var будет "Несовершеннолетний", если age > 18 вернет false

```java
var = age > 18 ? "Совершеннолетний" : "Несовершеннолетний";
```

## Условия - switch

Множественное ветвление:

```java
switch (age) {
    case 0:
        System.out.println("Ты родился");
        break;
    case 7:
        System.out.println("Ты пошел в школу");
        break;
    case 18:
        System.out.println("Ты совершеннолетний!");
        break;
    default: // этот блок необязателен
        System.out.println("Обычный год");
}
```

## Цикл - while

Цикл while:

```java
int value = 0;
while (value > 5) {
    System.out.println(value);
    value = value + 1;
}
```

## Цикл - do...while

Цикл do...while:

-   отличается от while тем, что обязатель но выполнит { ... } хотя бы раз
-   выполняет { ... }, пока выполняется условие condition

```java
do {
    System.out.println("Введите число 5: ");
    num = scanner.nextInt();
} while (num != 5);
```

## Цикл - for

Классика, как в C:

```java
for (int i = 0; i < 5; i = i + 1) {
    // блок кода
}
```

Цикл с несколькими инициализациями:

-   Переменные должны быть одного типа. Иначе никак

```java
for(int i=0, j=10; i<max; i++) { ... }
```

Цикл с несколькими инициализациями и шагами:

```java
for(int i=0, j=10; i<max; i++, j--) { ... }
```

Цикл без инициализации:

```java
for(; i<obj.length; i++) { ... }
```

Цикл только с условием:

```java
for(; i<obj.length;) { ... }
```

Бесконечный цикл:

```java
for(;;;) { ... }
```

## Цикл - forEach

Проходимся по элементам массива:

```java
int[] arr = {1, 2, 3, 4, 5, 6, 7};

for (int num : arr) {
    // блок кода
}
```

## Цикл - break

break - прерывет цикл и начинает выполнять код идущий после него:

-   в примере мы доходим до 5, срабатывает break и цикл прекращает свою работу

```java
for (int i = 0; i < 10; i++) {
	if (i == 5) {
		break;
	}
}
```

## Цикл - continue

continue - прыгает к следующей иттерации цикла:

-   в примере выведет цифры от 1 до 10 пропустив 5

```java
for (int i = 0; i < 10; i++) {
	if (i == 5) {
		continue;
	}
	System.out.println(i);
}
```

## Цикл - label

label - метка, на которую мы перемещаемся при использовании brek или continue:

-   в примере проходимся по двумерному массиву, для каждого элемента и если какой-то элемент равен 1, переходим к след строке

```java
labelName: for(int i = 0; i < 10; i++) {
    for (int let j = 0; j < data[i].length; j++) {
        if (data[i][j] == 1) {
			continue labelName;
		}
		data[i][j] = -1;
    }
}
```

## SyntaxSugar - Double pipe

Double pipe - возвращает значение слева. Eсли слева false, то тогда значение справа:

-   || - это Double pipe

```java
function example(a) {
	a = a || 'Value';
	console.log(a);
}

example(10);
// => 10

example(undefined);
// => 'Value'
example(0);
// => 'Value'
example(false);
// => 'Value'
example('');
// => 'Value'
```

## SyntaxSugar - Double qestion

Double qestion - возвращает значение слева. Eсли слева undefined, то тогда значение справа:

-   ?? - это Double qestion

```java
function example(a) {
	a = a ?? 'Value';
	console.log(a);
}

example(10);
// => 10
example(0);
// => 0
example(false);
// => false
example('');
// => ''

example(undefined);
// => 'Value'
```

## Важные вещи - console.log

Вывод в консоль:

```java
System.out.println("Ты родился")
```

## Важные вещи - Комментарии

Комментарии:

```java
// Это коментарий однострочный

/*
   Это коментарий
   многострочный
*/
```

## Примитивные типы - boolean

Стандартный логический тип:

-   значение по умолчанию: false

```java
boolean bVar = false;
boolean bVar2 = true;
```

## Примитивные типы - float, double

Примитивные числовые(дробные) типы:

-   float
    -   Необходим постфикс f, иначе создает double
    -   Занимает: 4 байт
    -   Класс обертка: Float
-   double
    -   Занимает: 8 байта
    -   Класс обертка: Double

```java
float fVar = 45.23f;
double dVar = 45.23;
```

## Примитивные типы - byte, short, int, long

Примитивные числовые(целые) типы:

-   byte
    -   Значение: от -128 до 127
    -   Занимает: 1 байт
    -   Класс обертка: Byte
-   short
    -   Занчение: от -32768 до 32767
    -   Занимает: 2 байта
    -   Класс обертка: Short
-   int
    -   Занчение: от -2147483648 до 2147483647
    -   Занимает: 4 байта
    -   Класс обертка: Integer
-   long
    -   Занчение: от "-много" до "+много"
    -   Занимает: 8 байт
    -   Класс обертка: Long

```java
byte bVar = 10;
short sVar = 10;
int iVar = 10;
long lVar = 10;
```

## Примитивные типы - char

char - примитивный тип хранящий символ:

-   инициализируется в одинарных кавычках
-   Класс обретка: Char

```java
char cVar = 'a';
```

## Collection - add

add - метод [интерфейса Collection](#collections---collection), который добавляет элемент в коллекцию(в конец):

```java
ArrayList<Cat> cats = new ArrayList<Cat>();
cats.add(new Cat("Бегемот"));
```

## Collection - remove

remove - метод [интерфейса Collection](#collections---collection), который удаляет элемент из коллекции(по индексу):

```java
ArrayList<Cat> cats = new ArrayList<Cat>();
cats.add(new Cat("Бегемот"));

cats.remove(0);
```

## Collection - isEmpty

isEmpty - метод [интерфейса Collection](#collections---collection), который проверяет пуста ли коллекция:

```java
ArrayList<Cat> cats = new ArrayList<Cat>();
cats.isEmpty();
// => true
```

## Collection - size

size - метод [интерфейса Collection](#collections---collection), возвращает колличество эллементов коллекции:

```java
ArrayList<Cat> cats = new ArrayList<Cat>();
cats.size();
// => 0
```

## Collections - synchronized

synchronized - если есть такая приписка у коллекции, значает, что при использовании многопоточности, есть возможность заблочить доступ всем потокам кроме одного

# AAAAAAAAAAAA

## throws

возможность в Java прокинуть исключения наверх. То есть, вместо обработки этого исключения внутри функции, его будет обрабатывать метод который вызвал данный. Пример использования(Exception как предок всех исключений способен использоваться как аналог ВСЕХ пойманых исключений):

```java
void method() throws java.lang.Exception {
    throw new ArithmeticException();
}
```

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

# JavaDoc

через комментарий позволяет создать документацию к Class, Interface, методу или полю

```java
/**
 * Method - Описание метода
 * @param x - первый параметр
 * @param y - второй параметр
 * @return int - возвращаемое значение
 **/
int kek() { return 0; };
```

JavaDoc для Class или Interface:

```java
/**
 * @author Cucmber
 * @version 0.0.1a
 * @see JavaDocLink - кликабельный текст-ссылка на что-то в проекте
 */
class DocumentationClass {}

/**
 * @deprecated если интерфейс или класс устарел
 */
@Deprecated interface DprecatedInterface {}
```

JavaDoc для метода(конструктора):

```java
/**
 * @param x описание первого аттрибута
 * @param y описание второго аттрибута
 * @throws PrinterException какое-то пояснение к ошибке. Вместо @trows можно использовать @exception, это одно и то же
 * @see JavaDocLink - кликабельный текст-ссылка на что-то в проекте
 * @return описание того что возвращаем
 */
int documentationMethod(int x, int y) throws PrinterException {
    return 0;
}

/**
 * @deprecated если метод устаред
 */
@Deprecated void deprecatedMethod(){}
```

JavaDoc для поля:

```java
/**
 * Текстовое описание поля
 * @see JavaDocLink - кликабельный текст-ссылка на что-то в проекте
 */
public String path;
```

JavaDoc использование link:
можно сделать ссылку на какой-то объект в проекте или какой-то модуль. Особено актуально для полей see и deprecate. Например:

```java
* @see package.Class#method(Type, Type,...)
```

или

```java
* @deprecated если интерфейс или класс устарел {@link Class#field}
```

Как прописывать ссылки до интересующих мест:

-   #field
-   #Constructor()
-   #method()
-   Class
-   Class#field
-   Class#Constructor()
-   Class#method()
-   package
-   package.Class
-   package.Class#field
-   package.Class#Constructor()
-   package.Class#method()

# Exception

исключения. Классы унаследованные от java.lang.Exception. Вызываются когда происходит некоторая ошибка. Делаятся на два вида:

1. Compile Exception - ошибки отлавливаемые при компиляции
2. Runtime Exception - ошибки которые невозможно отловить при компиляции, только при работе приложения

## Java, вывести цепочку ошибок

в коде используем try...catch и когда поймаем определенную ошибку или решим вызвать исключение, передаем в класс сообщение и пойманный Exception. Например:
try{
// ...
} catch (NumberFormatException e) {
throw new ClassExtendException("Message for Exception", e)
}

## Java, свои Exception

```java
создаем Class. Наследуем Class от Exception. Overloading методы Exception. Вызываем когда надо выбросить ошибку. Пример вызова:
throw new ClassExtendException("Message for Exception", e)
```

# hashCode

в Java не переопределенный hashCode использует ссылку в качестве основы для расчета. Поэтому у абсолютно одинаковых объектов hascode будет разным. Так что hashcode надо переопределять, ЭТО БАЗА

# Immutable

объект, который не позволяет себя менять. А все попытки провести изменения создают новый объект

# method reference

оказывается... На методы... Можно ссылаться... Прямо как в функциональных языках используя :: Например:

```java
class Hey {
    public double square(double num) {
        return Math.pow(num, 2);
    }
}

Hey hey = new Hey();
Function<Double, Double> square = hey::square;
double ans = square.apply(23d);
```

Аналогично:

```java
Function<Double, Double> square = (Double num) -> num * num;
double ans = square.apply(23d);
```

# Multithreading (Многопоточность)

чтобы код работал паралельно нужно создавать и запускать потоки. Поток может быть в нескольких состояниях, их можно получить методом getState(): NEW(создан но не запущен), RUNNABLE(запущен методом start и работет), TERMINATED(закончил свою работу)

Создать поток чз наследование:

```java
class ExThread extends Thread {
    @Override
    public void run() {
        System.out.println("privet");
        super.run();
    }
}

public class Main {
    public static void main(String[] args) {

        Thread th = new Thread(new ExThread());
        th.run();
    }
}
```

Создать поток чз интерфейс:

```java
class ExThread implements Runnable {
    @Override
    public void run() {
        System.out.println("privet");
    }
}

public class Main {
    public static void main(String[] args) {

        Thread th = new Thread(new ExThread());
        th.run();
    }
}
```

Создать поток чз Lambda функцию:

```java
Thread th = new Thread( () -> {
    System.out.println("privet");
});
th.run();
Создать поток чз абстрактный класс:
Thread th = new Thread(new Runnable() {
    @Override
    public void run() {
        System.out.println("privet");
    }
});
th.run();
```

## Stateless

объект, о котором мы точно знаем что он не будет изменен. Обычно в нем просто нет полей для изменения (связано с потоками)

## Thread - join

ждем окончания выполнения потока, и только после этого продолжаем идти по текущему потоку. Есть вариант с временем в миллисекундах, то есть если пройдет указанное время или вызванный поток закончит работу, то мы продолжим работу текущего потока

```java
class ExThread implements Runnable {
    @Override
    public void run() {
        System.out.println("privet");
    }
}

public class Main {
    public static void main(String[] args) throws InterruptedException {

        Thread th = new Thread(new ExThread());
        th.run();
        th.join();

        System.out.println("Thread th закончил работу");
    }
}
```

## Thread - sleep

просто заставляет поток остановиться(уснуть) на указанное число милисекунд:

```java

```

# Строение проекта

## Из чего состоит package

каждый файл package это Class. В файле обязательно должен быть одноименный public class

## Наименование package в JAVA

вначале казывают комерческий(com) или некомерческий(org) проект. А дальше уже по логической цели использования пакета.(но все это писано вилами по воде). Например вот так:
org.learn.android.messenger

## Строение проекта

```bash
.../Project
.../src
.../main
.../java- папка с packages
.../resources- image, const и т.д.
.../test- тесты
.../все остальное- gradle, git...
.../Externial Libries- библиотеки
```
