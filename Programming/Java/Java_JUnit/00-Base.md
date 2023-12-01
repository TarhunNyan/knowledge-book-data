# JUnit

Ниже информация по JUnit4, потому что на работе Junit4

# Test lifecycle

Test lifecycl - жизненный цикл тестов в JUnit:

-   [@Test - указывает что это тестовый метод](#annotation---test)
-   [@Before/@After - методы выполняемые перед/после @Test](#annotation---before/after)
-   [@BeforeClass/@AfterClass - методы выполняемые перед/после всех @Test](#annotation---beforeclass/afterclass)

# Assert

Assert функции проверяют является результатом ожидаемым или нет:

-   [JUnit - правильные пути import](#assert---import)
-   [AssertEquals/AssertNotEquals - проверяет на экивалентность примитивы/объекты](#assert---assertequals/assertnotequals)
-   [AssertTrue/AssertFalse - провряет на true/false](#assert---asserttrue/assertfalse)
-   [AssertNull/AssertNotNull - проверяет на null/не-null](#assert---asserttrue/assertfalse)
-   [AssertSame/AssertNotSame - проверяет на эквивалентность ссылок](#assert---assertsame/assertnotsame)
-   [fail - просто провал теста](#assert---fail)

# Примеры

## Assert - Import

Какие-то в IntelejiIDEA проблемы, он подключает не тот Class, вот правильный:

```java
import org.junit.jupiter.api.Test;
import static junit.framework.TestCase.assertEquals;
```

## Assert - Fail

fail - проваливает тест:

-   "Message" - сообщение с которым провалием тест

```java
fail();
fail("Message");
```

## Assert - AssertSame/AssertNotSame

AssertSame/AssertNotSame - проверяет на эквивалентность/не-эквивалентность ссылок:

-   obj1 - переменная с значением
-   obj2 - значение с которым сравниваем
-   "Message" - сообщение при ошибке

```java
AssertSame(obj1, obj2);
AssertSame("Message", obj1, obj2);
AssertNotSame(obj1, obj2);
AssertNotSame("Message", obj1, obj2);
```

## Assert - AssertTrue/AssertFalse

AssertTrue/AssertFalse - проверяет на то true или false:

-   a > 10 - условие которое проверяем или просто boolean
-   "Message" - сообщение при ошибке

```java
assertTrue(a > 10);
assertTrue("Message", a > 10);
assertFalse(a > 10);
assertFalse("Message", a > 10);
```

## Assert - AssertNull/AssertNotNull

AssertNull/AssertNotNull провряет на эквивалентность null/не-null:

```java
AssertNull(a);
AssertNull("Is null", a);
AssertNotNull(a);
AssertNotNull("Is not null", a);
```

## Assert - AssertEquals/AssertNotEquals

AssertEquals/AssertNotEquals - способен сравнивать примитивы и объекты с переопределенным equals, на эквивалентность/не-эквивалентность:

-   val1 - переменная с значением
-   val2 - значение с которым сравниваем
-   "Message" - сообщение при ошибке

```java
assertEquals(val1, val2);
assertEquals("Message", val1, val2);
```

## Assert - ArrayEquals

ArrayEquals - тест пройдет если элементы двух Array эквиваленты:

-   массивы могут содержать примитвы
-   массивы могут содержать Object с определенным методом equals
-   val1 - Array со значением
-   val2 - Array с которым сравниваем
-   "Message" - сообщение при ошибке

```java
assertArrayEquals(val1, val2);
assertArrayEquals("Message", val1, val2);
```

## Annotation - Test

Test - основная аннотация, указывающая что этот Method является тестовым:

-   при запуске тестирования, запускает все помеченные Method
    -   @Test - указывает что следющий метод это Test
    -   @Test(timeout=500) - тест с временем ожидания 500мс
    -   @Test(exception=IllegalArgumentException.class) - тест закончится положительно, если тело выдает проверяет выдает IllegalArgumentException

```java
class TestClass {

    @Test
    public void testSomeFunctionalaty() { }

    @Test(timeout=500)
    public void testSomeFunctionalaty() { }

    @Test(exception=IllegalArgumentException.class)
    public void testSomeFunctionalaty() { }
}
```

## Annotation - Before/After

Before/After - аннотация, указывающая что этот Method выполняется перед/после каждого тестового Method в классе:

```java
class TestClass {
    @Before
    public void setUp() {}

    @Test
    public void testSomeFunctionalaty() {
        // какой-то тест
    }

    @After
    public void tearDown() {}
}
```

## Annotation - BeforeClass/AfterClass

BeforeClass/AfterClass - аннотация, указывающая что этот Method выполняется один раз перед/после всеми тестового Method в этом классе:

```java
class TestClass {
    @BeforeClass
    public void setUp() {}

    @Test
    public void testSomeFunctionalaty() {
        // какой-то тест
    }

    @AfterClass
    public void tearDown() {}
}
```
