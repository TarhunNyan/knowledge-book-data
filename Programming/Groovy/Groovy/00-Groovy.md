# Groovy

Groovy - это Python от мира Java. Ситаксис Java работает в Groovy

# Структуры

## Переменные и типизация

Groovy поддерживает синтаксис Java, при этом позволяет свои особенности:

-   [Примитивы - точнее не возможность их получить](#переменные-и-типизация---примитивы)
-   [Типизация](#переменные-и-типизация---типизация)
-   [Контракты - ограничения и требования для переменных: >= groovy4.0](#переменные-и-типизация---контракты)

## Циклы

По сути работа циклов такая же, как и в java:

-   [Циклы - for/foreach](#циклы---forforeach)

## Категории(use)

Категории(Category) - пришло из Objective-C, где называлось Categories:

-   позволяют добавить поля и методы объекту, через внешний
-   эти правила работают внутри своего блока use

Категории(Category) - примеры:

-   [Категории - пример с TimeCategory](#категорииuse---пример-с-timecategory)
-   [Категории - пример custom категории](#категорииuse---пример-custom-категории)

# Ошибки

# Ссылочные типы

Так вышло, что в Groovy все - ссылочные типы

## GroovyObject

В Groovy любой класс, реализует интерфейс GroovyObject:

-   [Особенности создания классов в Groovy](#groovyobject---создание-класса)
-   [Разница Java и Groovy при перегрузке методов](#groovyobject---перегрузка-методов)
-   [Сравнение на эквивалентность](#groovyobject---сравнение-на-эквивалентность)
-   [Сравнение ссылок](#groovyobject---сравнение-ссылок)

Переопределение методво:

-   [invokeMethod - перехват вызова любого метода](#groovyobject---invokemethod)
-   [Методы отвечающие за вызов метода/параметра](#groovyobject---переопредление-вызова-методапараметра)
-   [Переопределение операторов](#groovyobject---переопределение-операторов)
-   [Подсовывание нового метода как в js](#groovyobject---замена-метода)

## Строки

Строки в Groovy имеют различное поведение в зависимости от использованных ковычек:

-   [String('') и f-string("")](#строки---string-и-f-string)

## Array

Array и Map - в Groovy их можно задать через короткий синтаксис:

-   [Создание Array](#array---задание-значений-массиву)
-   [Получение нескольких элементов из Array](#array---получение-нескольких-элементов)

## Map

Array и Map - в Groovy их можно задать через короткий синтаксис:

-   [Создание Map](#map---задание-значений-словарю)

## Range

Range - как в python, только доступен для любого объекта:

-   [Range - Примеры использования Integer/String Range](#range---примеры-использования-range)
-   [Range - использование для получения элементов Array](#range---получение-элементов-списка)
-   [Range - переопределение поведения](#range---создаем-свой-range)

# Функциональщина

## Функции

В отличии от Java можно создавать просто функции и методы не привязанные к классу:

-   [Функции - пример создания](#функциональщина---пример-функции)
-   [Функции/Closure - произвольное число параметров](#функциональщина---произвольное-число-параметров)
-   [Ссылка на метод объекта](#функциональщина---ссылка-на-метод)

## Closure(Замыкания)

Closure - классическая анонимная функция, не знаю почему так много внимания к этому приковано:

-   [Closure - пример создания](#функциональщина---closure-пример)
-   [Closure - поведение без параметров](#функциональщина---closure-без-параметров)
-   [Closure - присвоение переменной и вызов](#функциональщина---присвоение-переменной-и-вызов)

# Синтаксический сахар

Приколы Groovy:

-   [Цепочка комманд](#синтаксический-сахар---цепочка-комманд)
-   [Immutable Class - класс с неизменяемым состоянием](#синтаксический-сахар---immutable-class)

Операторы:

-   [as - приведение типов](#синтаксический-сахар---приведение-типов)
-   [in - проверка содержания объекта(isCase)](#синтаксический-сахар---содержится-ли)
-   [elvis(?:) - оператор подстановки значения если null](#синтаксический-сахар---elvis)
-   [safe navigation(?.) - не выполняющий операцию если дальше null](#синтаксический-сахар---safe-navigation)
-   [spread(\*.) - оператор раскрывающий последовательности](#синтаксический-сахар---spread)
-   [Сравнение на эквивалентность](#groovyobject---сравнение-на-эквивалентность)
-   [Сравнение ссылок](#groovyobject---сравнение-ссылок)

# Примеры

## Строки - String и f-string

Строки в groovy по умолчанию f-string, но есть и классические:

-   javaString = 'java' - просто строка
-   groovyString = "${javaString}" - строка с подстановкой переменных(f-string)

```groovy
javaString = 'java'
groovyString = "${javaString}"
```

## Array - Получение нескольких элементов

Получение нескольких элементов:

-   к сожалению, как в python получение значений с шагом я не нашел
-   есть несколько способов это сделать

Указвыаем несколько позиций из которых получаем элемент:

```groovy
a = "0123456789"
a[1, 3, 5]
// => 135
```

Используем Range:

```groovy
a = "0123456789"
a[1..9]
// => 123456789
```

Используем Spread оператор:

```groovy
positions = [1, 4, 5]
someArray = [10, 20, 30, 40, 50, 60]
println(someArray[*positions])
// => [20, 50, 60]
```

## Array - Задание значений массиву

В groovy нельзя использовать фигурные кавычки, для создания массива:

-   {} - используются для замыканий

```groovy
int[] array = {1, 2, 3} // в отличии от Java, такой синтаксис нельзя
int[] array = [1, 2, 3]
```

## Map - Задание значений словарю

Создание map:

```groovy
b = [1: true, 0: false]
```

## Range - Примеры использования Range

Примеры использования Range:

```groovy
0...6
// => [1, 2, 3, 4, 5, 6]
-6...0
// => [6, 5, 4, 3, 2, 1]
0...<6
// => [1, 2, 3, 4, 5]
0<...<6 // >= groovy4.0
// => [2, 3, 4, 5]
'a'..'d'
// => ['a', 'b', 'c', 'd']
```

## Range - Создаем свой Range

Создаем свой Range:

-   чтобы создать свой Range необходимо:
    -   чтобы класс был Comparable и CompareTo
    -   были переопределены next и previous

```groovy
class RangeObject implements Comparable {
  Integer value;

  int compareTo(Object o) {
    if(o instanceof RangeObject) { return this.value - o.value }
    return 0
  }

  def RangeObject(Integer value) { this.value = value }
  def next() { return new RangeObject(this.value + 2) }
  def previous() { return new RangeObject(this.value - 2) }
  public String toString() { return value }
}
```

```groovy
RangeObject obj1 = new RangeObject(10)
RangeObject obj2 = new RangeObject(23)
range = obj1..obj2
println range.collect{it.toString()}.join(', ')
// => 10, 12, 14, 16, 18, 20, 22
```

## Range - Получение элементов списка

Получаем элементы списка через Range:

```groovy
a = "0123456789"

a[1..-1]
// => 123456789

b = 1..5
a[b]
// => 12345
```

## Категории(use) - пример с TimeCategory

Пример с TimeCategory:

```groovy
import groovy.time.TimeCategory

use(TimeCategory)  {
    println 1.minute.from.now
    // => Wed Jan 17 10:42:17 KRAT 2024
    println 10.hours.ago
    // => Wed Jan 17 00:41:17 KRAT 2024

    def someDate = new Date()
    println someDate - 3.months
    // => Tue Oct 17 10:41:17 KRAT 2023
}
```

## Категории(use) - Пример custom категории

Пример custom категории:

```groovy
class SomeClass {
  private value

  SomeClass(Integer value) {
    this.value = 'another'
    if(value == 0 ) this.value = 'zero'
    if(value == 1 ) this.value = 'one'
  }

  @Override
  String toString() { return this.value }
}

class UseClassExample {
  public static SomeClass getSomeValue(Integer self) {
    return new SomeClass(self);
  }
}

use(UseClassExample)  {
  println 3.someValue
}
```

## Циклы - for/foreach

Циклы:

```groovy
for (i in 0..9) {
    print i
}

for (int i = 0; i < 9; ++i) {
    print i
}

for (Integer i : 0..9) {
    print i
}
```

## Функциональщина - Произвольное число параметров

Произвольное число параметров:

```groovy
def addAll = { int... args -> return args.sum() }
```

## Функциональщина - Ссылка на метод

Ссылка на метод:

-   видимо сначало можно было ссылаться на метод в groovy через .&
-   а потом добавили в возможность ссылаться на метод в java через ::

```groovy
list.forEach(System.out::println); // работает в: java, groovy

list.each { println it }           // работает в: groovy
list.each(this.&println)           // работает в: groovy
```

## Функциональщина - Пример функции

Функции:

-   ключевое слово return указывать не обязательно
    -   будет возвращено значение последней упомянутой переменной в фукции

```grovy
def functionA(argA) {
    print ArgA
}

int functionB(int argB) {
    print argB
    return argB
}

String fuctionC() {
    "Hello World"
}
```

## Функциональщина - Closure пример

Closure - это анонимная функция:

-   без return будет возвращено it

```groovy
def cl = {a, b ->
    println a
    println b
}

cl(1, 2)
// => 1
// => 2
```

## Функциональщина - Closure без параметров

Closure без параметров:

-   переданный в такой метод параметр попадет в переменную it
-   без return будет возвращено it

```groovy
println( [1,2,3,4].collect{it += 2} );
// => [3, 4, 5, 6]
```

## Функциональщина - Присвоение переменной и вызов

Присвоение переменной и вызов:

-   можно вызвать, как обычную функцию
-   можно вызвать, через метод call

```groovy
def cl = {a, b -> a+b }
cl(20, 30)
cl.call(20, 30)
```

## GroovyObject - Создание класса

Для классов Groovy автоматически создает конструктор, setter и getter:

```groovy
class Account {
    String name
    BigDecimal value
}

a = new Account(name : "Account #1", value : new BigDecimal(10))
def name = a.getName()
a.setName("Account #2")
```

## GroovyObject - Перегрузка методов

В Java перегруженный метод выбирается на этапе компиляции. В Groovy в момент выполнения программы:

-   приведенный код работает и в Java и в Groovy, но работает по разному

```groovy
int method(String arg) {
    return 1;
}
int method(Object arg) {
    return 2;
}
Object o = "Object";
int result = method(o);

System.out.println(result);
// java => 2
// groovy => 1
```

## GroovyObject - InvokeMethod

Перехватывает вызов любого метода:

-   я так и не понял как это использовать
    -   вызов любого метода вызывает переполнение стека
-   без GroovyInterceptable - вызывается только если метод отсутствует
-   с GroovyInterceptable - вызывается для любого метода

```groovy
class Interceptable implements GroovyInterceptable {
  def invokeMethod(String name, Object obj) {
    return "${name}: invokeMethod;\nObject: ${obj}"
  }
}

interceptable = new Interceptable();
println interceptable.test();
// test: invokeMethod;
// Object: []
println interceptable.test(2);
// test: invokeMethod;
// Object: [2]
println interceptable.test(100,200,30);
// test: invokeMethod;
// Object: [100, 200, 30]
```

## GroovyObject - Переопределение операторов

Перегрузка математических операторов(Operator overloading):

-   прямо как в Python

```groovy
class MathPlusOverload {

  private value

  Mathtest(value) {
    this.value = value;
  }

  def plus(b) {
    this.value - b
  }

}

plusOverload = new MathPlusOverload(10);
println plusOverload + 2
// => 8
```

Списое перегружаемых операторов:

-   математические операторы
    -   plus(other) - a + b
    -   a.minus(other) - a - b
    -   a.multiply(other) - a \* b
    -   a.power(other) - a \*\* b
    -   a.div(other) - a / b
    -   a.mod(other) - a % b
    -   bitwiseNegate() - ~a
-   математические побитовые операторы
    -   or(other) - a | b
    -   and(other) - a & b
    -   xor(other) - a ^ b
    -   leftShift(other) - a << b
    -   rightShift(other) - a >> b
    -   rightShiftUnsigned(other) - a >>> b
-   следующий/предыдущий элемент(важно Range)
    -   next() - a++ или ++a
    -   previous() - a-- или --a
-   оператор получение по индексу
    -   getAt(inex) - a[b]
    -   putAt(inex, other) - a[b] = c
-   положительное/негативное значение
    -   negative() - -a
    -   positive() - +a
-   switch
    -   isCase(other) - switch(a) { case(b) : }
-   логические
    -   equals(other) - a == b
    -   equals(other) - a != b
    -   compareTo(other) - a <=> b
    -   compareTo(other) - a > b
    -   compareTo(other) - a >= b
    -   compareTo(other) - a \< b
    -   compareTo(other) - a <= b
    -   asBoolean() - if(a)
    -   asType(other) - a as b

## GroovyObject - Замена метода

Замена метода - прямо как в js, groovy может заменять методы прямо в RunTime:

```groovy
Person.metaClass.buyHouse = lender.&borrowMoney
```

## GroovyObject - Переопредление вызова метода/параметра

-   propertyMissing(String name)
    -   вызывается если объекта отсутствует параметр при попытке получить
-   propertyMissing(String name, def arg)
    -   вызывается если у объекта отсутствует параметр для задания
-   methodMissing(String name, def args)
    -   вызывается если у объекта отсутствует вызываемый метод
-   $static_methodMissing(String name, Object args)
    -   вызывается если у класса отсутствует вызываемый метод
-   $static_propertyMissing(String name)
    -   вызывается если у класса отсутствует свойство

```groovy
class MethodAndPropertyMising {
    def propertyMissing(String name) {
        println 'getPropertyMissing';
        return null
    }

    def propertyMissing(String name, def arg) {
        println 'setPpropertyMissing';
        return null
    }

    def methodMissing(String name, def args) {
        println 'methodMissing';
        return null
    }

    static def $static_methodMissing(String name, Object args) {
        return "staticMethodMissing"
    }

    static def $static_propertyMissing(String name) {
        return "Missing static property name is $name"
    }
}

methodAndPropertyMising = new MethodAndPropertyMising();

methodAndPropertyMising.ha();
// => methodMissing
methodAndPropertyMising.m;
// => getPropertyMissing
methodAndPropertyMising.m = 2;
// => setPpropertyMissing
```

## GroovyObject - Сравнение на эквивалентность

Сравнения на эквивалентность в Groovy отличается от Java:

-   считай то же, что и .isEqual в java

```groovy
a == b
```

## GroovyObject - Сравнение ссылок

Сравнения ссылок в Groovy отличается от Java:

-   считай то же, что и .is в java

```groovy
a === b
```

## Переменные и типизация - Типизация

Типизация - возможно использовать типизацию как в Java, при этом можно вообще не объявлять типы:

```groovy
Float a = 100.0; // java-like
a = 100.0;       // groovy-like
```

## Переменные и типизация - Контракты

Контракты - аннотации которые проверяют утверждения во время испольнения приложения:

-   @Invariant - проверяет утверждение после вызова конструктора, до и после вызова метода
-   @Requires - утверждение, выполняемое перед вызовом метода
-   @Ensures - утверждение, выполняемое после вызова метода

```groovy
Float a = 100.0; // java-like
a = 100.0;       // groovy-like
```

## Переменные и типизация - Примитивы

Примитивы - объявляя примитивы, получим сразу объекты обертки:

```groovy
int a = 10
println a.class
// => class java.lang.Integer
```

## Синтаксический сахар - Цепочка комманд

Цепочка комманд - избавившись от точек и скобок, это превратилось тупо в английский текст:

-   paint(wall).with(red, green).and(yellow) - эквивалентно тому, что в примере

```groovy
paint wall with red, green and yellow
```

## Синтаксический сахар - Приведение типов

Приведение типов - ключевое слово as ответсвенно за приведение типов:

-   по сути:
    -   вызвает asType
    -   передает классом объекта в который переводим

```groovy
String input = '42'
Integer num = (Integer) input // error
Integer num = input as Integer
```

## Синтаксический сахар - Содержится ли

Содержится ли - ключевое слово in проверяет наличие объекта в другом объекте:

-   по сути вызвает isCase

```groovy
array = [1,2,34]
print(34 in array)
// => true
```

## Синтаксический сахар - Elvis

Elvis:

-   положит в b переменную a, если a == null, иначе "b"

```groovy
def b = a ?: "b"
```

## Синтаксический сахар - Safe navigation

Safe navigation:

-   избегаем NullException, вернет null

```groovy
def b = Users.get("a")?.posts
array?[10] = 100
```

## Синтаксический сахар - Spread

Spread operator:

-   можно сказать, что вызвали у всех элементов Itterable объекта parent, метод метод action и сложили в новый array
-   так же используется для раскрытия массива

```groovy
def sizes = ['string', 'long string']*.size()
println sizes
// => [6, 11]

def x = [2, 3]
def y = [0, 1, *x, 4]
println y
// => [0, 1, 2, 3, 4]

def a = [3 : 'c', 4 : 'd']
def b = [1 : 'a', 2: 'b', * : a, 5 : 'e']
println b
// => [1:a, 2:b, 3:c, 4:d, 5:e]
```

## Синтаксический сахар - Immutable Class

Immutable Class - неизменяемые классы. Должны иметь типы для полей:

```groovy
@Immutable
class ImmutableClass {
    String a
    Integer b
}

def ic = new ImmutableClass(a : "a", b : 1)
```
