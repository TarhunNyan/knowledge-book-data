# Groovy

Groovy - это Python от мира Java. Ситаксис Java работает в Groovy

# Groovy vs Java

## Перегрузка методов

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

## Создание массивов

В groovy нельзя использовать фигурные кавычки, для создания массива:

-   {} - используются для замыканий

```groovy
int[] array = {1, 2, 3} // нельзя в отличии от Java
int[] array = [1, 2, 3]
```

## Создание map

Создание map:

```groovy
b = [1: true, 0: false]
```

## Проходимся по элементам списка

Проходимся по элементам списка:

```groovy
a = "0123456789"

a[1..-1]
// => 123456789
a[1, 3, 5]
// => 135

b = 1..5
a[b]
// => 12345
```

## Range

Range объект можно встроенным синтаксисом собрать из любого объекта, имеющего методы next() и prev()

```groovy
'a'..'aa'
// => a..aa

a = "0123456789"
a[1..4]
// => 1234
```

## Циклы

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

## Функции

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

## Замыкания(Closure)

Замыкания - это анонимная функция:

-   ключевое слово return указывать не обязательно
    -   будет возвращено it

```groovy
def cl = {a, b ->
    println a
    println b
}

cl(1, 2)
```

## Class

Для классов groovy автоматически создает конструктор, setter и getter:

```groovy
class Account {
    String name
    BigDecimal value
}

a = new Account(name : "Account #1", value : new BigDecimal(10))
def name = a.getName()
a.setName("Account #2")
```

## Immutable Class

Immutable Class - неизменяемые классы. Должны иметь типы для полей:

```groovy
@Immutable
class ImmutableClass {
    String a
    Integer b
}

def ic = new ImmutableClass(a : "a", b : 1)
```

## Операторы

Elvis:

-   положит в b переменную a, если a == null, иначе "b"

```groovy
def b = a ?: "b"
```

Safe navigation:

-   избегаем NullException, вернет null

```groovy
def b = Users.get("a")?.posts
```

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

## try-with-resource

В Groovy блоки try-with-resource реализованны по другому:

```groovy
new File('/path/to/file').eachLine('UTF-8') {
   println it
}
```

Java-совместимый вариант:

```java
new File('/path/to/file').withReader('UTF-8') { reader ->
   reader.eachLine {
       println it
   }
}
```

## Lambda

Создание lambda выражения:

```groovy
Runnable run = () -> System.out.println("Run"); // java
Runnable run = { println 'run' }                // groovy
```

## Ссылка на метод

```groovy
list.forEach(System.out::println); // java

list.each { println it }           // groovy
list.each(this.&println)           // groovy
```

## Сравнения

Сравнения на эквивалентность:

-   считай то же, что и .isEqual в java

```groovy
a == b
```

Сравнения на ссылок:

-   считай то же, что и .is в java

```groovy
a === b
```

# Переменные

В Groovy даже привитивные переменные, сразу обернуты в объекты:

```groovy
int a = 10
println a.class
// => class java.lang.Integer
```

# Строки

Строки в groovy по умолчанию f-string, но есть и классические:

-   javaString = 'java' - просто строка
-   groovyString = "${javaString}" - строка с подстановкой переменных(f-string)

```groovy
javaString = 'java'
groovyString = "${javaString}"
```

#
