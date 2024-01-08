# Java

Java - это база, это знать надо

# Структуры

## Переменные и типизация

Объявление переменных происходит с указанием типа:

-   [Указание типа переменной](#переменные-и-типизация---тип-переменной)
-   [Указание типа переменной: Generic начиная с java5](#переменные-и-типизация---generic)
-   [Указание типа переменной: var начиная с java10](#переменные-и-типизация---var)

Casting - приведение типа:

-   [UpCasting - преобразуем Child в Parent](#переменные-и-типизация---upcasting)
-   [DownCasting - преобразуем Parent в Child](#переменные-и-типизация---downcasting)
-   [InstanceOf - проверка соответствия классу](#переменные-и-типизация---instanceof)
-   [InstanceOf - проверка соответствия классу: начиная с java14](#переменные-и-типизация---instanceofjava14)

## Модификаторы

Modifiers - ключевые слова определяющие поведение полей, объектов и методов:

-   [Modifiers - Class](#модификаторы---class)
-   [Modifiers - Method/Fields](#модификаторы---methodsfields)

## Условия

Условия бывают:

-   [Условие типа: if ... else if ... else](#условия---ifelse)
-   [Условие типа: switch](#условия---switch)
-   [Условие типа: стрелочный switch, начиная с java12](#условия---switchjava12)
-   [Условие типа: множественный switch, начиная с java13](#условия---switchjava13)
-   [Условие типа: switch начиная с java14](#условия---switchjava14)
-   [Тернарный оператор](#условия---тернарный-оператор)
-   [Оператор elvis](#условия---оператор-elvis)

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

# Ошибки

## Обработка ошибок

Ошибки - отлавливаются классическим try-catch-finally:

-   [try-catch-finally - синтаксис](#ошибки---try-catch-finally)

try-with-resources, закрываем Socket автоматически:

-   [try-with-resources - синтаксис](#ошибки---try-with-resources)
-   [try-with-resources - переписанный в развернутом виде](#ошибки---try-with-resources-в-стиле-try-catch-finally)

## Throwable(Error/Exception)

Throwable(бросающий исключение)([дерево](./source/throwable.svg)) - класс от которого наследуются Error и Exception:

-   Error - вызвается при нехватке ресурсов системы
-   Exception - вызвается как результат ошибки компиляции или выполнения программы

Exception - исключения, все исключения в Java унаследованы от java.lang.Exception. Вызываются когда происходит некоторая ошибка. Делаятся на два вида:

1. Compile Exception - ошибки отлавливаемые при компиляции
2. Runtime Exception - ошибки которые невозможно отловить при компиляции, только при работе приложения

Работа с ошибками:

-   [Создаем свои ошибки](#ошибки---создаем-свои-exception)
-   [Вывести цепочку ошибок](#ошибки---вывести-цепочку-exception)

Прокидываем ошибки наверх:

-   [Прокидываем ошибки наверх](#ошибки---прокидываем-ошибку-наверх)

# Неизменяемые типы

## Примитивные типы

Примитивные типы - НЕ ссылочные типы:

-   [Целые числа - byte, short, int, long](#примитивные-типы---byte-short-int-long)
-   [Числа с плавающей точкой - float, double](#примитивные-типы---float-double)
-   [Логический - boolean](#примитивные-типы---boolean)
-   [Символьный - char](#примитивные-типы---char)

## Array

Array - массив, длина которого не изменяется. Создать массив:

-   [Создать массив с указанным типом](#array---создать)
-   [Создать массив с конкретными значениями](#array---создать-со-значениями)
-   [Создать двумерный массив](#array---создать-двумерный-массив)

Работаем с Array:

-   [Получить элемент по индексу](#array---получить-элемент-по-индексу)
-   [Заменить элемент](#array---заменить-элемент)
-   [Получить длину](#array---получить-длину)

## String

String - строка, хоть и не является примитивом, но я ее все равно занес сюда. Создать строку:

-   [Создать строку](#string---создать-строку)

# Ссылочные типы

Object - так получается, String и Array - исключение. Обычно все типы данных в Java - это Object:

-   [Object - базовый для всех объект объект](#object---object)

## Object - Сравнение примитивов/объектов

Для примитивов работают стандартные ==, <, > и т.д.

Для объектов сравнение происходит по другому:

-   [equals - эквивалентность](#object---equals)
-   [hashCode - расчет hash](#object---hashcode)
-   [compare - сравнение объектов](#object---compare)

## Object - Object

Object - class в java, базовый для всех объектов. Содержит в себе методы:

-   [clone - вернет копию объекта](#object---clone)
-   [equals - веренет то, одинаковы ли объекты внутри](#object---equals)
-   [finalize - этот метод вызвается перед удалением Объекта сборщиком мусора](#object---finallize)
-   [getClass - веренет class, который сейчас у объекта(upcasting/downcasting)](#object---getclass)
-   [hashCode - метод возвращаюищий hashcode объекта](#object---hashcode)
-   [toString - возвращает объект в формате строки](#object---tostring)

Методы object связанные с потоками:

-   [notify/notifyAll - выводить поток поставленный из состояния заморозки](#object---notifynotifyall)
-   [wait - возвращает объект в формате строки](#object---wait)

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

Lambda - короткая запись Functional Interface:

-   [Пример Lambda функции](#functional-interface---пример-lambda)
-   [Method Reference - ссылка на метод](#functional-interface---method-refrence)

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
-   [Сделать что-то если в Optional есть значение(ifPresent/ifPresentOrElse)](#optional---ifpresent)
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

## Переменные и типизация - Тип переменной

Указание типа переменной:

-   int - тип
-   number - имя переменной

```java
int number;
```

## Переменные и типизация - Generic

Generic - подставляет класс в Wildcard:

-   синтаксис Generic, доступен начиная с 5 java
-   List<String>
    -   List - class/interface в котором определен generic
    -   String - class/тип подставляемый в generic

```java
List<String> l = new ArrayList<String>();
```

## Переменные и типизация - var

var - позволяет не указывать тип переменной:

-   синтаксис var, доступен начиная с 10 java
-   при объявлении позволяет не указывать тип явно
-   за вас тип объявит компилятор сам

```java
var number = 30;
```

## Переменные и типизация - UpCasting

UpCasting - преобразуем Child в Parent:

-   Animal является Parent для Cat
-   в примере Class с именем Cat, преобразуется к Class с именем Animal

```java
Animal cat = new Cat();
```

## Переменные и типизация - DownCasting

DownCasting - преобразуем Parent в Child:

-   Animal является Parent для Cat
-   Cat newCat = (Cat) cat; - переменная cat являющаяся Animal преобразуется к Cat

```java
Animal cat = new Cat();
Cat newCat = (Cat) cat;
```

## Переменные и типизация - InstanceOf

InstanceOf - позволяет определить к какому классу относится переменная:

-   instanceof доступен начиная с 2 java
-   animal - переменная
-   Cat - тип для которого проводим проверку

```java
if(animal instanceof Cat) {
    Cat cat = (Cat) animal;
    cat.meow();
}
```

## Переменные и типизация - InstanceOf(Java14)

InstanceOf - позволяет определить к какому классу относится переменная:

-   instanceof доступен начиная с 14 java
-   animal - переменная
-   Cat - тип для которого проводим проверку
-   newcat - переменная в которую кладем уже приведенную к Cat переменную animal

```java
if(animal instanceof Cat newcat) {
    newcat.meow();
}
```

## Модификаторы - Class

Modifiers-Class - модификаторы класса:

-   Access Modifier
    -   public - доступен классам даже из других пакетов
    -   default(ничего не пишем перед именем класса) - доступен только классам из того же пакета
-   Non-Access Modifier
    -   final - от класса нельзя унаследоваться
    -   abstract - от класса можно унаследоваться, но нельзя создать объект данного класса

## Модификаторы - Methods/Fields

Modifiers-Methods/Fields - модификаторы для конструкторов, методов и полей:

-   Access Modifier
    -   public - доступно всем классам
    -   private - доступно только внутри класса, в котором объявлен
    -   protected - доступен только внутри этого пакета, а так же в subclasses и superclasses
    -   default(ничего не пишешь) - доступен только внутри этого пакета
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
    -   transient - модификатор говорящий JVM что поле НЕ надо [сериализовывать](./04-Serialize.md)
    -   synchronized - создает блокировку потоков внутри метода
        -   только для методов
        -   то же, как если бы все тело метода было обернуто в synchronized
    -   volatile - что-то про кэширование

## Ошибки - Прокидываем ошибку наверх

-   throws InterruptedException
    -   перекинет ошибку в место вызова метода someMethod
    -   InterruptedException - тип ошибок коды перекидываются наверх

```java
public void someMethod() throws InterruptedException {
    ...
}
```

## Ошибки - Вывести цепочку Exception

Вывести цепочку ошибок:

-   ловим ошибку через try...catch
-   полученный StackTrace передаем в следующую ошибку

```java
try{
    ...
} catch (NumberFormatException e) {
    throw new ClassExtendException("Message for Exception", e)
}
```

## Ошибки - Создаем свои Exception

Создаем свои Exception:

-   создаем Class унаследованный от Exception
-   overload методы exception
-   вызываем когда хотим получить ошибку

```java
throw new ClassExtendException("Message for Exception", e)
```

## Ошибки - try-catch-finally

Ошибки отлавливаются через try-catch-finally:

```java
try {
    // код
} catch (Exception e) {
    // поймали ошибку
} finally {
    // выполнится вне зависимости от наличия ошибки
}
```

## Ошибки - try-with-resources

Работа с сокетами, заключается в использовании try-with-resources:

-   просто синтаксический сахар, чтобы не городить try-catch-finally
-   зачем нужен:
    -   чтобы неверняка закрыть socket
    -   в случае ошибки, может случиться так, что программа не дойдет до метод close у socket, тогда отработает try-wtith-resource

```java
try(FileOutputStream output = new FileOutputStream(path)) {
   output.write(1);
}
```

## Ошибки - try-with-resources в стиле try-catch-finally

try-with-resources переписанный в развернутом виде через try-catch-finally:

```java
FileOutputStream output = null;

try {
   output = new FileOutputStream(path);
   output.write(1);
}
catch (IOException e) {
   e.printStackTrace();
}
finally {
   if (output != null)
      output.close();
}
```

## Object - notify/notifyAll

notify/notifyAll - выводит поток/все-потоки поставленный из состояния заморозки

-   выводить поток поставленный на wait из состояния заморозки
    -   говорит что поток может продолжать работу
    -   может быть вызван только у синхронизированного объекта

## Object - wait

wait - замараживаем работу потока, до тех пор пока не будет вызван notify/notifyAll:

-   замараживаем работу потока, до тех пор пока не будет вызван notify
-   может быть вызван только у синхронизированного объекта
-   пример
    -   есть два потока
    -   в одном мы рисуем, в другом печатаем рисунок
    -   если мы закончили печатать, а рисовать еще не закончили, то поток печати должен будет спрашивать каждые n секунд, а закончил ли второй поток рисовать
    -   чтобы этого не делать, у потока печати можно вызвать wait, чем поставить его на ожидание

wait(long timeout):

    -   может быть вызван только у синхронизированного объекта
    -   замараживаем работу потока, до тех пор пока не будет вызван notify, или не закончется timeout в мс

wait(long timeout, int nanos):

    -   может быть вызван только у синхронизированного объекта
    -   замараживаем работу потока, до тех пор пока не будет вызван notify, или не закончется timeout в мс и nanos в нс

## Object - Clone

clone - вернет копию объекта:

-   protected Object clone()
-   у разных классов может быть разная реализация

## Object - Finallize

finalize - этот метод вызвается перед удалением Объекта сборщиком мусора:

-   protected void finalize()
-   сборщик мусора вызывается System.gc()
-   не рекомендуется переопределять этот метод

## Object - getClass

getClass - веренет class, который сейчас у объекта(upcasting/downcasting):

-   Class<T> getClass()
-   [отлчие getClass от obj.class](#object---getclass-vs-class)
-   элемент Reflection API

## Object - toString

toString - возвращает объект в формате строки:

-   String toString()
-   возвращает строку, построенную по объекту

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

hashCode - функция, возвращаю hash посчитанный на основе объекта:

-   int hashCode()
-   Обязательно нужно определять, иначе HashMap и подобное не заработает
-   в Java не переопределенный hashCode использует ссылку на объект, в качестве основы для сравнения

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

ifPresentOrElse - если в Optional есть значение, то выполняет первую функцию иначе вторую:

```java
Optional<String> opt = Optional.empty();
opt.ifPresentOrElse(
    () -> System.out.println("is not empty")
    () -> System.out.println("is empty")
);
```

## Optional - orElse

orElse - вернуть значение если Optional пустой:

-   Если передана функция, то она отрабатывает всегда

```java
Optional<String> opt = Optional.empty();
opt.orElse("is empty");
```

orElseGet - вернуть результат функции, если Optional пустой:

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

## Functional Interface - Method Refrence

Method Reference - получаем ссылку на метод:

-   в примере каждый элемент list выведем через System.Out.pringtln в консоль

```java
list.forEach(System.out::println);
```

Пример с импользованием interface:

```java
Function<Double, Double> square = (Double num) -> num * num;
double ans = square.apply(23d);
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

## Array - Получить элемент по индексу

Получить элемент по индексу:

```java
int[] numerics = {100, 200, 300};
int num = numerics[0];
// => 100
```

## Array - Заменить элемент

Заменить элемент по индексу:

```java
int[] numerics = {100, 200, 300};
numerics[0] = 400;
int num = numerics[0];
// => 400
```

## Array - Получить длину

Получить длину:

-   выведет именно зарезервированную длину array, не смотря на заполненость

```java
int[] numerics = new int[5];
int length = numerics.length;
// => 5
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

## Условия - switch(Java12)

Множественное ветвление:

-   стрелочный синтаксис, доступен начиная с 12 java
-   с этим синтаксисом НЕ НАДО использовать default
    -   компилятор подскажет, все ли значения enum охвачены, что полезно при изменении в enum
-   сразу присваивает значение переменной
-   не нужно использовать break
-   case FALSE, NULL - используем несколько меток
-   case EXCEPTION - пример с использованием блока кода
    -   yield - возвращает результат в переменную

```java
boolean result = switch (ternaryBool) {
    case TRUE -> true;
    case FALSE, NULL -> false;
    case FILE_NOT_FOUND -> throw new UncheckedIOException(
            "This is ridiculous!",
            new FileNotFoundException());
    case EXCEPTION -> {
        System.out.println("It is exception");
        int exception_num = 10;
        yield exception_num;
    }
    default -> throw new IllegalArgumentException("Seriously?!");
};
```

## Условия - switch(Java13)

Множественное ветвление:

-   синтаксис через двоеточие, доступен начиная с 13 java
-   с этим синтаксисом НЕ НАДО использовать default
    -   компилятор подскажет, все ли значения enum охвачены, что полезно при изменении в enum
-   сразу присваивает значение переменной
-   используем yield для возвращения значения
-   case FALSE, NULL: - используем несколько меток

```java
boolean result = switch (ternaryBool) {
    case TRUE:
        yield true;
    case FALSE, NULL:
        yield false;
    case FILE_NOT_FOUND:
        throw new UncheckedIOException(
                "This is ridiculous!",
                new FileNotFoundException());
    default:
        throw new IllegalArgumentException("Seriously?!");
};
```

## Условия - switch(Java14)

Множественное ветвление:

-   синтаксис паттерна типов, доступен начиная с 14 java

```java
boolean result = switch (obj) {
    case String str -> callStringMethod(str);
    case Number no -> callNumberMethod(no);
    default -> callObjectMethod(obj);
};
```

## Условия - Оператор Elvis

Оператор Elvis - заменяет null на стандартное значение:

-   если переменная не null, то возвращает ее
-   если переменная null, то задает значение по умолчанию
-   появился в java9
    В java не стали вводить классический оператор

```bash
?.
```

Вместо этого, в бибилотеку Objects добавили метод requireNonNullElse:

```java
String result = null;
str = Objects.requireNonNullElse(str, "vasia");
// => vasia
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
