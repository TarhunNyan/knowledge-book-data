# JUnit

Ниже информация по JUnit4, потому что на работе Junit4

# Annotation

Библиотека работает через Annotation:

-   [@Test - указывает что это тестовый метод](#annotation---test)
-   [@Before/@After - методы выполняемые перед/после @Test](#annotation---before/after)
-   [@BeforeClass/@AfterClass - методы выполняемые перед/после всех @Test](#annotation---beforeclass/afterclass)

# Assert

Assert функции проверяют является результатом ожидаемым или нет:

-   [](#assert---assertequals)

# Примеры

## Assert - AssertEquals

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
