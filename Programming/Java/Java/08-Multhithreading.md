# Multithreading

Java многопоточность

# Примеры

# АААААААААААа

# Immutable

объект, который не позволяет себя менять. А все попытки провести изменения создают новый объект

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
