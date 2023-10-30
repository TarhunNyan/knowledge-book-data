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
