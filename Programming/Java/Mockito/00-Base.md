# Mockito

Mockito - библиотека, которая является стандартом при разработке на Java. Создает обертку вокруг объекта, чтобы подсовывать нужные данные во время тестов

## Включаем Mockito

Первый способ подключить Mockito:

```java
@ExtendWith(MockitoExtension.class)
public clss Test { ... }
```

Второй способ подключить Mockito:

-   просто полезна знать что так тоже можно

```java
public class Test {
    @BeforeEach
    public void init() {
        MockitoAnnotations.openMocks(this);
    }
}
```

## Создать виртуальный объект

Создать виртуальный объект:

-   первая строка - создает объект у которого есть все методы экземпляра ArryList
-   mockList.add("Element"); - метод ArryList, который не выбросит ошибки, но и ничего не сдеает, потому что вызван у заглушки

```java
List mockList = Mockito.mock(ArrayList.class);
mockList.add("Element");
```

## Обоорачиваем объект

Spy - позволяет перехватывать вызовы методов у реального объектар:

-   первая строка - создает обертку вокруг объекта
-   mockList.add("Element"); - метод ArryList, который выполниться как обычно

```java
List<String> mockList = Mockito.spy(new ArrayList<String>());
mockList.add("Element");
```

## Работа с Mock-объектами

Вернуть нужный объект:

-   [Вернуть нужный объект, при любом вызове метода](#mockito---вернуть-нужный-объект)
-   [Выбросить исключение при вызове метода](#mockito---выбросить-исключение)
-   [Вернуть нужный объект, при вызове метода, с определенными параметрами](#mockito---вернуть-объект-по-параметрам)

# Примеры

## Mockito - Вернуть нужный объект

When - позволяет подсунуть результат по вызову

-   первая строка - создает обертку вокруг объекта
-   вторая строчка - нстраиваем поведение
    -   doReturn(10) - возвращаем 10
    -   when(mockList).size() - когда вызывем метод size

```java
List<String> mockList = Mockito.spy(new ArrayList<String>());
mockList.doReturn(10).when(mockList).size();
```

Второй способ:

-   первая строка - создает обертку вокруг объекта
-   вторая строчка - настраиваем поведение
    -   when(mockList.size()) - когда вызывем метод size
    -   thenReturn(10) - возвращаем 10

```java
List<String> mockList = Mockito.spy(new ArrayList<String>());
mockList.when(mockList.size()).thenReturn(10);
```

## Mockito - Выбросить исключения

Выбрасываем исключение:

-   первая строка - создает обертку вокруг объекта
-   вторая строчка - выбрасывает исключение при вызове метода
    -   mockList.size()) - метод при котором выскочит исключение
    -   IllegalStateException.class - выбрасываемое исключение

```java
List<String> mockList = Mockito.spy(new ArrayList<String>());
Mockito.when(mockList.size()).thenThrow(IllegalStateException.class);
```

Второй способ:

-   первая строка - создает обертку вокруг объекта
-   вторая строчка - выбрасывает исключение при вызове метода
    -   mockList.size() - метод при котором выскочит исключение
    -   doThrow(IllegalStateException.class) - выбрасываемое исключение

```java
List<String> mockList = Mockito.spy(new ArrayList<String>());
Mockito.doThrow(IllegalStateException.class).when(mockList.size());
```

## Mockito - Вернуть объект по параметрам

````java
List<String> mockList = Mockito.spy(new ArrayList<String>());
mockList.doReturn(10).when(mockList).get(500);
mockList.doReturn(10).when(mockList).get(Mockito.any());
mockList.doReturn(10).when(mockList).get(Mockito.any(ClassName.class));
mockList.doReturn(10).when(mockList).get(Mockito.anyInt());
mockList.doReturn(10).when(mockList).get(Mockito.anyBoolean());
mockList.doReturn(10).when(mockList).get(Mockito.anyDouble());
``

# Дополнительно

## Dynamic Proxy

Dynamic Proxy - создает Proxy вокруг объекта:

-   появился в Java5
-   Dynamic Proxy - работает на уровне интернфейса
-   на этом основана библиотека Mockito

Пример класса который будем оборачивать:

```java
// интерфейс для класса
public interface Person {
    public void introduce(String name);
    public void sayAge(int age);
    public void sayForm(String city, String country);
}

// класс, объект которого будем оборачивать
public class Man implements Person {
    private String name;
    ...

    public Man(String name, int age, String city, String country) { ... }

    @Override
    public void introduce(String name) { System.out.println("Меня зовут " + this.name); };

    @Override
    public void sayAge(int age) { System.out.println("Мне лет " + this.age); };

    @Override
    public void sayForm(String city) { System.out.println("Я из города " + this.city); };)
}
````

InvocationHandler - перехватчик вызова методов:

```java
public class PersonInvocationHandler implements IncovationHandler {
    private Person person;
    public PersonInvocationHandler(Person person) { this.person = person; }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.print("Hello!");
        return method.invoke(person, args);
    }
}
```

Собираем все вместе и создаем DynamicProxy:

```java
public class Main {
    public static void main(String[] args) {
        // Создаем объект
        Man = vasia = new Man("Вася", 30, "Красноярск", "Россия");

        // Получаем загрузчик класса
        ClassLoader vasiaClassLoader = vasia.getClass().getClasLoader();

        // Получаем все интерфейсы, которые реализует класс
        Class[] interfaces = vasia.getClass().getInterfaces();

        // засовываем объект в обертку
        Person proxyVasia = (Person) Proxy.newProxyInstance(
            vasiaClassLoader,
            inerfaces,
            new PersonInvocationHandler(vasia)
        );

        // Вызываем метод объекта
        proxyVasia.introduce();
        // => "Hello! Меня зовут Вася"
    }
}
```
