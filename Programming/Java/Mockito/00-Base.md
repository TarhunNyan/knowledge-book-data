# Mockito

Mockito - библиотека, которая является стандартом при разработке на Java. Создает обертку вокруг объекта, чтобы подсовывать нужные данные во время тестов

## Включаем Mockito

Первый способ подключить Mockito:

```java
@ExtendWith(MockitoExtension.class)
public clss Test { ... }
```

Второй способ подключить Mockito:

-   просто полезно знать что так тоже можно

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

-   Вернуть нужный объект, при любом вызове метода
    -   [Способ 1 - точно рабочий](#mockito---вернуть-нужный-объект2)
    -   [Способ 2 - почему-то не вышло](#mockito---вернуть-нужный-объект)
-   [Проверить, вызывает ли метод исключение](#mockito---assertthrows)
-   Выбросить исключение при вызове метода
    -   [Способ 1](#mockito---выбросить-исключения)
    -   [Способ 2](#mockito---выбросить-исключения2)
-   [Вернуть нужный объект, при вызове метода с определенными параметрами](#mockito---вернуть-объект-по-параметрам)
-   [Проверить число вызовов метода](#mockito---verify)
-   [Проверить порядок вызова методов](#mockito---oreder)
-   [Пишем тесты для статических методов](#mockito---mockstatic)

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
```

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

# Примеры

## Mockito - Виртуальная функция

Виртуальная функция:

-   первая строка - создает обертку вокруг объекта
-   doAnswer - задает функцию которая срабатывает по условию
-   .when(mockList).get(anyInt()) - условие срабатывания

```java
List<String> mockList = Mockito.spy(new ArrayList<String>());
Mockito.doAnswer(invocation -> {
    int parameter = invocation.getArgument(0);
    return parameter * parameter;
}).when(mockList).get(anyInt());
```

## Mockito - Вернуть нужный объект

When - позволяет подсунуть результат по вызову:

-   первая строка - создает обертку вокруг объекта
-   вторая строчка - нстраиваем поведение
    -   doReturn(10) - возвращаем 10
    -   when(mockList).size() - когда вызывем метод size

```java
List<String> mockList = Mockito.spy(new ArrayList<String>());
mockList.doReturn(10).when(mockList).size();
```

## Mockito - Вернуть нужный объект#2

When - позволяет подсунуть результат по вызову:

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
    -   mockList.size() - метод при котором выскочит исключение
    -   IllegalStateException.class - выбрасываемое исключение

```java
List<String> mockList = Mockito.spy(new ArrayList<String>());
Mockito.when(mockList.size()).thenThrow(IllegalStateException.class);
```

## Mockito - Выбросить исключения#2

Выбрасываем исключение:

-   первая строка - создает обертку вокруг объекта
-   вторая строчка - выбрасывает исключение при вызове метода
    -   mockList.size() - метод при котором выскочит исключение
    -   doThrow(IllegalStateException.class) - выбрасываемое исключение

```java
List<String> mockList = Mockito.spy(new ArrayList<String>());
Mockito.doThrow(IllegalStateException.class).when(mockList.size());
```

## Mockito - AssertThrows

AssertThrows - роняет тест, если не вызвалось исключение при вызове метода:

-   IllegalStateException.class - исключение
-   .size() - метод

```java
List<String> mockList = Mockito.spy(new ArrayList<String>());
assertThrows(IllegalStateException.class, () -> mockList.size());
```

## Mockito - Вернуть объект по параметрам

Можно указать, при каких параметрах мы должны вернуть определенное значение:

-   .get(500) - при значении 500 вернет 10
-   .get(Mockito.any()) - при любом переданном Object включая null вернет 10
-   .get(Mockito.any(ClassName.class)) - при переданной ссылке на Class вернет 10
-   .get(Mockito.anyInt()) - при любом int вернет 10
-   .get(Mockito.anyBoolean()) - при любом boolean вернет 10
-   .get(Mockito.anyDouble()) - при любом double вернет 10

```java
List<String> mockList = Mockito.spy(new ArrayList<String>());
mockList.doReturn(10).when(mockList).get(500);
mockList.doReturn(10).when(mockList).get(Mockito.any());
mockList.doReturn(10).when(mockList).get(Mockito.any(ClassName.class));
mockList.doReturn(10).when(mockList).get(Mockito.anyInt());
mockList.doReturn(10).when(mockList).get(Mockito.anyBoolean());
mockList.doReturn(10).when(mockList).get(Mockito.anyDouble());
```

## Mockito - Verify

Verify - валит тест если не совпадает указанное количество вызовов и колчиство вызовов, насчитанное в обертке:

-   never() - метод никогда не должен вызываться
-   only() - должен быть только один вызов и только к этому методу
-   atLeastOnce() - метод вызывался 1 или больше раз
-   atLeast(4) - метод вызывался n или больше раз
-   atMost(10) - метод вызывался n или меньше раз
-   times(3) - метод вызывался n раз

```java
List<String> mockList = Mockito.spy(new ArrayList<String>());

Mockito.verify(mockList, never()).get(anyInt());
Mockito.verify(mockList, only()).get(anyInt());
Mockito.verify(mockList, atLeastOnce()).get(anyInt());
Mockito.verify(mockList, atLeast(4)).get(anyInt());
Mockito.verify(mockList, atMost(10)).get(anyInt());
Mockito.verify(mockList, times(3)).get(anyInt());
```

## Mockito - Order

Order - позволяет проверить порядок вызова методов и переданных параметров:

Создаем mock-объект. Вызываем какие-то методы c параметрами:

```java
List<String> mockedList = mock(MyList.class);
mockedList.size();
mockedList.add("a parameter");
mockedList.clear();
```

Создаем объект InOrder, у которого делаем вызов проверок методов:

```java
InOrder inOrder = Mockito.inOrder(mockedList);
inOrder.verify(mockedList).size();
inOrder.verify(mockedList).add("a parameter");
inOrder.verify(mockedList).clear();
```

## Mockito - MockStatic

-   Mockito.mockStatic(StaticUtils.class) - оборачивает статичесие методы
    -   дело в том, что Static методы класса нельзя обернуть в [Dynamic Proxy](#dynamic-proxy)

```java
@Test
void givenStaticMethodWithNoArgs () {
    try (MockedStatic<StaticUtils> utilities =  Mockito.mockStatic(StaticUtils.class)) {
        //добавляем правило
         utilities.when(StaticUtils::name).thenReturn("Привет");

        //проверяем, что правило работает
        assertEquals("Привет", StaticUtils.name());
    }
}
```
