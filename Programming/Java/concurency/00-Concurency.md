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
-   BLOCKED - связано с Lock и Monitor
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
