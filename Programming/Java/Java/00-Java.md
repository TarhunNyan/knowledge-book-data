# Java

Java - это база, это знать надо

# Java - SDK

SDK - набор инструментов для разработки. Туда входят:

-   API
-   Libriries
-   Documentation
-   Debugging
-   Tutorials

Например, может быть специальная SDK - для аторизации. Это не только библиотека, а уже полностью сформированный инструмент

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

# Синтаксический сахар

Обработка undefined:

-   [Если false, то новое значение](#syntaxsugar---double-pipe)
-   [Если undefined, то новое значение](#syntaxsugar---double-qestion)

# Асинхронность

# Типы данных

## Примитивные типы

Примитивные типы - НЕ ссылочные типы:

-   [Целые числа - byte, short, int, long](#примитивные-типы---byte-short-int-long)
-   [Числа с плавающей точкой - float, double](#примитивные-типы---float-double)
-   [Логический - boolean](#примитивные-типы---boolean)
-   [Символьный - char](#примитивные-типы---char)

## String

Array - строка. Создать строку:

-   [Создать строку](#string---создать-строку)

## Array

Array - массив, длина не изменяется. Создать массив:

-   [Создать массив с указанным типом](#array---создать)
-   [Создать массив с конкретными значениями](#array---создать-со-значениями)
-   [Создать двумерный массив](#array---создать-двумерный-массив)

# Collections

Ооооо, колекции в Java это пипец:

<img src="./source/collections.png" width=500 style="display: block; margin: auto;"/>

## Iterable

Iterable - базовый интерфейс коллекций. Реализует одно поле - Iterator:

-   list.Iterator() - вернет объект [класса Iterator](#iterator)

```java
ArrayList<String> list = new ArrayList<String>;
Iterator iterList = list.Iterator();
```

## Iterator

Iterator - класс, с помощью которого можно иттерироваться по другому объекту:

-   связан с [интерфейсом Iterable](#iterable)

Реализует 3 метода:

-   [Получить следующий элемент иттерации(next)](#)
-   [Удалить последний полученный элемент иттератора(remove)](#)
-   [Узнать, закончился ли иттератор(hasNext)](#)

## Collection

Collection - базовый интерфейс коллекций. Реализует такие методы как:

-   [Добавить элемент(add)](#collection---add)
-   [Удалить элемент(remove)](#collection---remove)
-   [Является ли, коллекция пустой(isEmpty)](#collection---isEmpty)
-   [Получить размер коллекции(size)](#collection---size)

## List

List - интерфейс позволяющий хранить упорядоченную последовательность элементов(дубликаты и null). Элементы индексированны:

Методы полученные от [интерфейса Collection](#collection):

-   [Добавить элемент(add)](#collection---add)
-   [Удалить элемент(remove)](#collection---remove)
-   [Является ли, коллекция пустой(isEmpty)](#collection---isEmpty)
-   [Получить размер коллекции(size)](#collection---size)

## Queue

Queue - интерфейс реализующий очередь по правилу FIFO(first-in first-out):

Методы полученные от [интерфейса Collection](#collection):

-   [Добавить элемент(add)](#collection---add)
-   [Удалить элемент(remove)](#collection---remove)
-   [Является ли, коллекция пустой(isEmpty)](#collection---isEmpty)
-   [Получить размер коллекции(size)](#collection---size)

Реализует методы, которые аналогичны стандартным, но ошибки не выбрасывает

1. poll вместо remove
2. offer вместо add (для ограниченный очередей ошибка)
3. peek вместо element

## Deque

Deque(DoubleEndQue) - двунаправленная очередь. В такой очереди элементы могут использоваться с обоих концов. Здесь работает как FIFO так и LIFO

Методы:

1. addFirst / offerFirst
2. addLast / offerLast
3. removeFirst / pollFirst
4. removeLast / pollLast
5. getFirst / peeckFirst
6. getLast / peeckLast

## Set

Set - интерфейс реализующий коллекцию уникальных элементов(множество):

Методы полученные от [интерфейса Collection](#collection):

-   [Добавить элемент(add)](#collection---add)
-   [Удалить элемент(remove)](#collection---remove)
-   [Является ли, коллекция пустой(isEmpty)](#collection---isEmpty)
-   [Получить размер коллекции(size)](#collection---size)

## ArrayList

ArrayList - массив изменяющий свою длинну:

-   Плюсы:
    -   Быстро ищет элементы
-   Минусы:
    -   Долго добавляет элементы
    -   Нет синхронизации

Принцип работы:

-   initialCapacity - резервирует память на указанное число элементов элементов. В примере: (2);
-   size - при превышении initialCapacity, создает новый массив, размер которого = размер \* 1.5 + 1

```java
ArrayList<String> strings = new ArrayList<String>();
ArrayList<String> strings = new ArrayList<String>(2);
```

Методы полученные от [интерфейса Collection](#collection):

-   [Добавить элемент(add)](#collection---add)
-   [Удалить элемент(remove)](#collection---remove)
-   [Является ли, коллекция пустой(isEmpty)](#collection---isEmpty)
-   [Получить размер коллекции(size)](#collection---size)

## SynchronizedList

SynchronizedList - массив изменяющий свою длинну, синхронизированны методы add и remove:

-   Плюсы:
    -   Быстро ищет элементы
    -   Синхронизированны методы add и remove
-   Минусы:
    -   Долго добавляет элементы
    -   Нет синхронизации у Iterator

```java
ArrayList<String> strings = new ArrayList<String>();
ArrayList<String> strings = new ArrayList<String>(2);
```

Методы полученные от [интерфейса Collection](#collection):

-   [Добавить элемент(add)](#collection---add)
-   [Удалить элемент(remove)](#collection---remove)
-   [Является ли, коллекция пустой(isEmpty)](#collection---isEmpty)
-   [Получить размер коллекции(size)](#collection---size)

## LinkedList

LinkedList - цепочка из данных:

-   Плюсы:
    -   Долго ищет элементы
-   Минусы:
    -   Быстро добавляет элементы(особенно если они находятся вначале или в конце коллекции)

Принцип работы - цепочка из данных, где каждый элемент хранит определенные данные и ссылки на предыдущий и следующий элементы:

-   Head - первый элемент LinkedList, и для него предыдущий элемент null
-   Tail - последний элемент LinkedList, и для него последующий элемент null

```java

```

Методы полученные от [интерфейса Collection](#collection):

-   [Добавить элемент(add)](#collection---add)
-   [Удалить элемент(remove)](#collection---remove)
-   [Является ли, коллекция пустой(isEmpty)](#collection---isEmpty)
-   [Получить размер коллекции(size)](#collection---size)

## Vector

Vector(УСТАРЕЛ) - массив изменяющий свою длинну:

-   По сути тоже что и ArrayList
-   имплементирует интерфейс List
-   Класс устарел и не рекомендуется к использованию
-   Является [synchronized](#collections---synchronized)

## synchronized lock

synchronized - ставит Lock для других потоков, чтобы переменные не конфликтовали. Например при иттерировании по коллекции это обязательно:

-   (synchList) - переменная для которой поставили Lock

```java
synchronized (synchList) {
    Iterator<Integer> iterator = synchList.iterator();
    while (iterator.hasNext()) {
        System.out.println( iterator.next() );
    }
}
```

# Важные вещи

Важные вещи:

-   [Вывод в косоль](#важные-вещи---consolelog)
<!-- -   [Комментарии](#важные-вещи---комментарии) -->

# Примеры

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

Возвращает значение слева, если это false, то тогда значение справа:

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

Возвращает значение слева, если это undefined, то тогда значение справа:

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
    -   Занчение: от -128 до 127
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

add - метод [интерфейса Collection](#collection), который добавляет элемент в коллекцию(в конец):

```java
ArrayList<Cat> cats = new ArrayList<Cat>();
cats.add(new Cat("Бегемот"));
```

## Collection - remove

remove - метод [интерфейса Collection](#collection), который удаляет элемент из коллекции(по индексу):

```java
ArrayList<Cat> cats = new ArrayList<Cat>();
cats.add(new Cat("Бегемот"));

cats.remove(0);
```

## Collection - isEmpty

isEmpty - метод [интерфейса Collection](#collection), который проверяет пуста ли коллекция:

```java
ArrayList<Cat> cats = new ArrayList<Cat>();
cats.isEmpty();
// => true
```

## Collection - size

size - метод [интерфейса Collection](#collection), возвращает колличество эллементов коллекции:

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

## WildCard

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

# Access Modifier для Class

Access Modifier для Class:

-   public - доступен классам даже из других пакетов
-   default(ничего не пишем перед именем класса) - доступен только классам из тогоже пакета

# Access Modifier для конструкторов, методов и полей

Access Modifier для конструкторов, методов и полей:

-   public - доступно всем классам
-   private - доступно только внутри класса, в котором объявлен
-   default(ничгего не пишешь) - доступен только внутри этого пакета
-   protected - доступен только внутри этого пакета, а так же в subclasses и superclasses

# Modifiers бывают

Modifiers бывают:

-   Access Modifier
-   Non-Access Modifier

## Non-Access Modifier для Classes

Non-Access Modifier для Classes:

-   final - от класса нельзя унаследоваться
-   abstract - от класса можно унаследоваться, но нельзя создать объект данного класса

## Non-Access Modifier для методов и аттрибутов

Non-Access Modifier для методов и аттрибутов:

-   final - не могут быть переопределены(пишутся большими буквами)
-   static - аттрибуты и методы относятся к классу, а не объекту(к ним можно обращаться без new). Поэтому внутри static метода нельзя использовать аттрибуты и методы без модификатора static
-   abstract - только для методов и только в абстрактном классе. Такой метод нельзя вызвать, и у него нет тела. Только переопределять в наследнике
-   transient - что-то о сериализации
-   synchronized - что-то о потоках
-   volatile - что-то про кэширование

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

# Lambda выражения

появилась в 8 JAVA. Выглядит как функциональное, программирование но по сути им не является. Обычно, чтоб передать метод, надо:

1. Создать интерфейс с нужным методом (функциональный интерфейс)
2. В месте куда передаём определить, что получаемый объект должен имплементировать новый интерфейс
3. Имплементировать новый класс от интерфейса
4. В классе определить этот метод
5. Передать этот класс туда, где нужно данный метод

Для упрощения можно не создавать отдельный класс а создать абстрактный класс. А для уменьшения кода не абстрактный класс, а лямбда функцию, которая по сути является абстрактным классом и просто уменьшает код.

(String s) -> { return s.length; }

Короткая запись с одним параметром:
s -> s.length

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

# Stream

появились в JAVA 8. Последовательность элементов поддерживающая последовательные и паралельные опперации над ними, такие как:

-   map
-   filter
-   reduce
-   forEach

## Stream - filter

Возвращает только значения прошедшие проверку:

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

пробегаемся по Stream и что-то делаем, но ничего не возвращает. Например можно что-либо вывести:

```java
Arrays.stream(array).forEachOrdered(el -> System.out.println(el * 2));
```

## Stream - map

применяет переданную функцию последовательно к каждому элементу

Подробный вариант использования map:

```java
ArrayList<String> arrayList1 = new ArrayList<>();
arrayList1.add("privet");
// ...
arrayList1.add("ok");

Stream<String> fooStream = arrayList1.stream();
Stream<Integer> fooStream2 = fooStream.map( el -> el.length() );
ArrayList<Integer> arrayList2 = (ArrayList<Integer>) fooStream2.collect(Collectors.toList());
```

Сокращенный вариант:

```java
ArrayList<String> arrayList1 = new ArrayList<>();
arrayList1.add("privet");
// ...
arrayList1.add("ok");

ArrayList<Integer> arrayList2 = (ArrayList<Integer>) arrayList1.stream().map(
        el -> el.length()
).collect(Collectors.toList());
```

Хотя к массивам по другому применяется:

```java
int[] array = {1,2,4,6,78};
int[] array2 = Arrays.stream(array).map(
        el -> el * 2
).toArray();
```

## Stream - reduce

возвращает из набора элементов один, путем кумулятивного(накопительно) применения функции к последовательнсоти. По факту возвращает Optional, это связано с тем что может reduce может вернуть null:

```java
int result = 0;
Optional<Integer> reduce = list.stream().reduce((accumulator, el) -> accumulator*el);
if(reduce.isPresent()) {
    result = reduce.get();
} else {
    // значение Stream.reduce null... Короче говно случилось
}
```

# Строение проекта

## Точка входа

при запуске из командной строки, типа: "java ClassName", запускается метод main в class'е ClassName. Этот самый метод main и есть точкой входа

Точек входа в проектк может быть сколько угодно, главное описать метод main:

```java
public static void main(String[] args) { /* код */}
```

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

# Class

## Порядок выполнения блоков в JAVA классе

в JAVA классе есть:

1. Инициализатор - выполняется до конструктора

```java
{
    type = "Кот";
    age = 0;
}
```

2. Конструктор - функция с тем же теменем что и класс. Выполняется при создании объекта. Его монжо перегрузить (overloading), что очень полезно. Если конструктор не прописан, то автоматически создается такой, что все поля заполняет либо null либо 0:

```java
Pet() {
    name = null;
    type = null;
    age = 0;
}
```

3. main - эта штука является точкой входа для даного класса

```java
public class Class {
    public static void main(String[] args) { }
}
```

## Class - Enum

перечисление. Чтобы не писать класс с кучей static полей придумали enum. В простейшем случае:

```java
public enum Enum {
    // К enum можно обращатся в стиле: Enum.DOG == Enum.FROG   ->   False
    CAT, DOG, WOLD, FROG
}
```

но все немного хитрее, и каждое такое поле в enum реализует class описываемым этим enum. Например:

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

ООП'ешная фиговина. В интерфейсах описываются static поля и контракты методов(без тела). Классы можно расширять неограниченным числом interface. Класс должен реализовывать все методы и поля интерфесов, от которых он расширен:

1. Поля по умолчанию - public static final
2. Методы по умолчанию - public abstract

Объявление интерфейса:
interface ExInterface

Расширение класса:
class ClInterface implements Interface1, Interface2

В остальном, все точно так же как у классов. Даже есть своеобразный downcasting/upcasting:
Interface obj = new ObjImplementFromInterface();

## Setter/Getter

ООП инкапсуляция, все дела... Поэтому к полям объектов обращаемся только через функции setParamName и getParamName

## Upcasting/Downcasting

прием в ООП, когда мы преобразуем parent в child(Downcasting) или child в parent(Upcasting)

Часто используется Downcasting, особенно Когда надо найти что-то в списке с разными типами элементов:

TextView myTextView = (TextView) findViewById(R.id.myText);

## Анонимный класс

класс созданный "находу", для создания одного объекта

```java
Dog dog = new Dog() {
    public void newFuncInsideAnonimClass() {
        System.out.println("Так создается объект анонимного класса в котором переопределяют все что захотят");
    }
};
```

## Как сравнивать самописные классы

чтобы сравнить самописные классы, нужно имплементировать interface - Comparable, и перегрузить его методы

## Равенство

проверка на равенство это ==. Но корректно это работает только для примитивных типов. В случае с непримитивными типами сравниваются ссылки. Поэтому для сравнения используют метод equals, определенный(перегруженный) для всех Object

## Сравнение на равенство самописных классов

необходимо переопределить метод equals

## Перегрузка(Overloading) метода

когда метод с одним именем может принимать разный набор параметров. И в итоге от набора параметров зависит то, как метод исполнится

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
