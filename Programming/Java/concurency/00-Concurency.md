# Concurency

Есть OC(операционная система)

Process(процес) - обычно это любое приложение запущенное в OC:

-   У Process есть выделенная системой память, которая не пересекается с другими Process

Thread(поток) - концепция в программировании:

-   Работают с памятью Process внутри которого были созданы
-   программа сама переключается между Thread
-   Thread реализуют и на однопроцессорных системах
-   Основная задача которую решают threads - создать ощущение непрерывности для пользователя при работе с переферийными устройствами
-   создание и уничтожение thread очевидно намного дешевле чем создание и удаление Process
-   threads обычно квантуются по времени, то есть 1ms на первый thread, потом 1ms на второй thread и т.д. по кругу пока thread не закончит работу
-   поскольку у thread память одна на всех и происходит квантование, то при изменении памяти из одного потока может произойти полный трындец в другом потоке

В Java, у каждого потока свой Stack, но к тому, что лежит в Heap можно обращаться из нескольких потоков, чем повредить объект

Stack и Heap:

-   Stack
    -   очередь типа fifo
    -   быстрее чем Heap
    -   хранит значения примитивов и ссылки на объекты(и еще вызванные методы)
    -   у каждого потока свой
-   Heap
    -   куча из данных
    -   медленнее чем Stack
    -   хранит объекты
    -   общий для всех потоков

# Java

В Java приложение запускается в неявно созданном thread с названием main. Этот thread можно получить:

```java
Thread.currentThread()
```

## Создаем свой thread

Создаем свой thread(лучше использовать анонимный класс):

-   достаточно создать класс унаследованный от класса Thread
-   переопределить метод run, код которого и будет запускаться в новом потоке
-   вызвать у экземпляра нашего класса метод start, который создаст новый thread и запустит в нем код из метода run

```java
public final class MyThread extends Thread {
    @Override
    public void run() {
        // code is here
    }
}

Thread thread = new MyThread();
thread.start();
```

Создаем свой thread через Runnable:

-   Runnable это функциональный класс, в котором есть только метод Run, так что можем использовтаь лямбда-выражение
-   Инициализируем Thread с переданным Runnable
-   через метод start создаем новый thread

```java
final Runnable task = () -> {
    // code is here
}
final Thread thread = new Thread(task);
thread.start();
```

## Состояние потока

Поток находиться в одном из 6 состояний:

-   NEW - создан но не запущен
-   RUNNABLE - запущен
-   BLOCKED - ждет когда у объекта освободится Monitor
-   WAITING - если поток ожидает(join/wait) завершение другого потока
-   TIMEDWAITING - если поток ожидает(join/wait с указанием веремени) завершение другого потока
-   TERMINATED - поток заверщен/упал и т.д.

Получить состояние потока можно методом getState:

```java
Thread thread = new Thread(someRunnable);
thread.getState()
// => NEW
```

## Получить поток

Статический метод currentThread возвращает поток в котором этот метод был вызван

## Прерывание работы потока

stop - deprecated метод который принудительно завершает работу потоку:

```java
Thread thread = new Thread(someRunnable);
thread.start();
thread.stop();
```

interrupt - метод который просит поток прерваться. По сути устанавливает флажок interrupt, который thread должен проверять сам:

```java
Thread thread = new Thread(someRunnable);
thread.start();
thread.isInterrupt();
// => false
thread.interrupt();
thread.isInterrupted();
// => true
```

## Прерывание работы потока через InterruptException

Некоторые методы, например sleep или join, выбрасывают исключение interruptException, если у потока флаг interrupt сменился. Это исключение можно поймать через catch и завершить работу потока

Есть метод interrupted, который возвращает то был ли прерван поток и флаг interrupt ставит в false. Так вот, когда вызывается exception - флаг interrupt сбрасывается. Это не совсем логично, так что стоит в блоке catch опять устанавливать флаг interrupt

```java
try {
    ...
} catch (final InterruptException interruptException) {
    currentThread().interrupt()
    System.out.println("This is thread is interrupt")
}
```

# TimeUnit

Там не только единицы измерения времени, но и всякие приколы, типа уснуть на n времени:

-   в примере засыпаем на 2 дня

```java
TimeUnit.HOURS.sleep(2)
```

## Приоритет работы потоков

-   Процессор выбирает какой поток он будет выполнять исходя из приоритета потоков
-   JVM не определяет какой поток будет выполняться. Этим занимается операционка
-   можно установить приоритет потоку:
    -   MIN_PRORITY - равен 1
    -   NORM_PRIORITY - равен 5
    -   MAX_PRIORITY - равен 10
-   если несколько потоков обладают высоким приоритетом, то поток с низким проитетом может вообще не выполниться
    -   так что обычно расстановка приоритетов, это ошибка
-   поток наследует приоритет того потока, который его создал

```java
System.out.println(Thread.currentThread().getName(), Thread.currentThread().getPriority())
// => main 5

Thread.currentThread().setPriority(Thread.MAX_PRIORITY)
System.out.println(Thread.currentThread().getName(), Thread.currentThread().getPriority())
// => main 10
```

## Потоки демоны

В Java потоки делятся на основные-потки и на потоки-демоны:

-   Программа завершиться когда отработают основные-потоки
-   Программа завершиться не смотря на то, есть ли еще потоки-демоны
    -   К потокам-демонам относятся
        -   сборщик муссора - garbage-collector
-   Потоки наследуют статус создавшего его потока

Пример:

-   isDaemon - проверяет является ли поток потоком-демоном
-   setDaemon - задаем, является ли поток потоком-демоном

```java
Thread.CurrentThread().isDaemon()
// => false

Thread thread = new Thread(new Task())
thread.setDaemon(true)
thread.isDaemon()
// => true
```

## Необрабатываемы исключения

Существуют в Java необрабатываемые исключения, например RuntimeException. Для их обработки у потоков существует специальный механизм:

-   thread, thread2 - поток который выбрасывает RuntimeException
-   UncaughtExceptionHandler - класс который умеет обрабатывать Exception пойманных в Thread
-   setUncaughtExceptionHandler - устанавливаем потоку обработчик исключения
-   Thread.setDefaultUncaughtExceptionHandler - устанавливает указанный обработчик по умолчанию для всех новых потоков

```java
final UncaughtExceptionHandler uncaughtExceptionHandler = (thread, exception) -> {
    System.out.printf("%s : %s\n", thread.getName(), exception.getMessage())
}

Thread thread = new Thread(new Task())
thread.setUncaughtExceptionHandler(uncaughtExceptionHandler);
thread.start()
// => Thread-0 : RuntimeException

Thread.setDefaultUncaughtExceptionHandler(uncaughtExceptionHandler)
Thread thread2 = new Thread(new Task())
thread.start()
// => Thread-1 : RuntimeException
```

## Фабрика потоков

Можно создать фабрику, в которой будет определен метод с настройками для Tread и потом просто создавать Thread передавая Runnable:

```java
class ... {
    public static void main(String[] args) {
        ThreadFactory threadFactory = new DeamonThreadFactory();

        Thread thread1 = threadFactory.newThread(new Task());
        thread1.start();
        Thread thread2 = threadFactory.newThread(new Task());
        thread2.start();
        Thread thread3 = threadFactory.newThread(new Task());
        thread3.start();
    }
}

private static final class DeamonThreadFactory implements ThreadFactory {

    @OVerride
    public Thread newThread(Runnable runnable) {
        final Thread thread = new Thread(runnable);
        thread.setDaemon(true);
        return thread;
    }
}
```

## Atomic

смотри javapractice g04_MulthiThreads_AtomicProblem

Атомарными операциями являются:

-   присваивание с следующими примитивными типами(все примитивы кроме long и double):
    -   byte
    -   short
    -   int
    -   float
    -   char
    -   String
    -   boolean
-   присваивание ссылок
-   += в переменную(но это не точно, просто наблюдение)

Чтобы сделать метод атомарным, достаточно добавить ему ключевое слово Synchronized:

```java
public static synchronized MyIncrement() {
    counter++;
}
```

synchronized - говорит, что первый поток который достучится до этого метода, захватывает Monitor объекта в котором этот метод определен

-   в нашем примере метод статический и потому захвачен будет Runner.class(Runner - это class в котором определен метод MyIncrement)
-   в не статическом методе Monitor будет взят по this

## Monitor

Monitor - у каждого объекта в java есть ровно один Monitor:

-   поток может взять Monitor
-   если Monitor взят потоком A, то все остальные потоки при попытки получить доступ к объекту, будут ждать освобождение Monitor потоком A
-   если поток не может получить объект из-за захвата Monitor другим объектом, он переходит в состояние Blocked

## wait и notify

wait(поток приостанавливает свою работу) и notify(поток продолжает своб работу) - вызываются только в методе, владеющим монитором объекта(?):

```java

```
