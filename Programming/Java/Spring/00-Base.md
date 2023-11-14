# Spring

Spring - древний framework. Огромный комбайн для всего. Есть во всех вакансиях на Java-разработчика

SpringBoot - объединяет популярные Spring-фреймворки, и предоставляет некоторые обертки:

-   Spring IoC - основной фреймворк, на нем работают все остальные
-   Spring Data - фреймворк для работы с БД
-   Spring MVC - фреймворк для создания веб-приложений

## Структура Bean проекта

Структура Bean проекта, которая используется везде:

-   БД - база данных с нужными нам данными
-   Repository - слой доступа к данным. Короче каждый Repository, типа сиквел запроса. Хранит или дает доступ к каким-то данным
-   Service - манипуляция с данными и какая-то "бизнес-логика"
-   Component - выдает какой-то финальный результ, типа отправляет данные клинету

# Spring - IoC

Spring IoC - простенькая система, где у нас есть объект Context, который хранит в себе bean(заинициализированные объекты)

-   bean - заинициализированный объект внутри Context
-   Context - штука в которой храняться объекты
    -   BeanFactory - старперский класс, который только создает Bean
    -   ApplicationContext - новый класс, который не только создает но и управляет Bean
-   IoC и DI - принципе на совмещение которых работет Spring IoC
    -   IoC(Inversion of Control) - принцип, суть которого в деллигировании обязанностей внешнему объекту(Context)
    -   DI(Dependency Injection) - например, если в конструкторе записать ссылку на другой класс

Рабочий процесс Spring IoC:

-   создаем конфигурации
    -   используя XML или через Java Annotation
    -   в конфигурациях указываем классы, которые будут Bean
    -   указываем какие файлы конфигурации нужны, для создания context
-   при старте приложения вызваем Spring
    -   Spring создает все указанные bean
    -   Spring записывает все bean внутрь Context

## Record

Record - это вообще не Spring, но пихнуть куда-то надо было

-   прикол для JDK14 и старше
-   позволяет быстро создать класс с конструтором

```java
record Adress(String firstLine, String city) {};
```

По стуи полная запись выглядит так:

```java
class Adress {
    Adress(String firstLine, String city) {
        this.firstLine = firstLine;
        this.city = city;
    }
}
```

## Bean

Bean - класс который автоматически создается Spring и храниться в Context

## Bean - Создание

Spring - когда создает Bean, в качестве аргументов Construcotor-класса или метода подставляет САМ другие Bean:

-   если есть Bean с типом и именем как у аргумента, подставляет его
-   если есть один Bean с указанным типом как у аргумента, подставляет его
-   если есть один Bean потомок от указанного типа, подставляет его
-   если несколько Bean с указанным типом как у аргумента, ищет Bean с пометкой @Primary
-   если несколько Bean потомков от указанного типа, ищет Bean с пометкой @Primary
-   также может повлиять на выбор: @Qualifier

## Bean - @Component

Помечаем Class как bean:

```java
@Component
class SomeClassName { ... }

@Component(name="anotherNameToBean")
class SomeClassName2 { ... }
```

## Bean - @Bean

Помечаем Method как Bean:

-   Spring будет хранить то, что вернул Method внутри Context

```java
@Bean
public Person someBean(String name, int age) {
    return new Person(name, age);
}

@Bean(name="anotherNameToBean")
public Person someBean(String name, int age) {
    return new Person(name, age);
}
```

## Bean - Primary

Primary - в случаем когда выбор между несколькими Bean, выбирает с пометкой Primary:

-   bean1, bean2 - два Bean которые возвращают тип Person
-   someBean - интересующий нас Bean
-   (Person somePerson) - аргумент, который подставляется из Spring. Подставиться bean1, потому что он Primary

```java
@Bean
@Primary
public Person bean1() { return new Person(...); }

@Bean
public Person bean2() { return new Person(...); }

@Bean
public String someBean(Person somePerson) { ... }
```

## Bean - Qualifier

Qualifier - разрешает проблемму неоднозначности Bean:

-   @Autowired - аннотация, автоматически присваивает Bean в поле marketPlace
-   @Qualifier("ios") - если есть несколько Bean типа MarketPlace, выберет с именем "ios"

```java
@Autowired
@Qualifier("ios")
private MaretPlace marketPlace;
```

Использование Qualifier с методом:

-   @Bean - указываем что метод является Bean
-   @Qualifier("address") Address from - засунет во from объект Bean с именем "address"

```java
@Bean
public Person person(
    String name,
    @Qualifier("address") Address from
) { ... }
```

## Bean - Autowired

Autowired - задает значение автоматически, подходящим Bean:

-   работает только в пределах Configurtion

```java
@Autowired
final Person person;

@Autowired
void setSomeVar(Person person) { ... }
```

## Bean - Value

Value - задает указанное значение:

-   работает только в пределах Configurtion

```java
@Value("8")
int cylinderCount;

@Vaule("8")
void setCylinderCount(int cylinderCount) { ... }

void setCylinderCount(@Value("8") int cylinderCount) { ... }
```

## Bean - DependsOn

DependsOn - укзаывает, что этот Bean надо инициализировать после всех остальных:

-   @DependsOn("engine") - инициализировать car(Class) после engine(Bean)
-   @DependsOn("fuel") - инициализировать engine(Bean) после fuel

```java
@DependsOn("engine")
class Car implements Vehicle { ... }

@Bean
@DependsOn("fuel")
Engine engine() {
    return new Engine();
}
```

## Bean - Lazy

Lazy - обычно Bean создаются при запуске, но с аннотацией Lazy, инициализация откладывается до первого вызова:

-   @Lazy(false) - не применять режим Lazy

```java
@Configuration
@Lazy
class VehicleConfig {
    @Bean
    @Lazy(false)
    Engine engine() { return new Engine(); }
}
```

Lazy для конкретного Bean:

-   @Lazy - применить режим Lazy

```java
@Configuration
class VehicleConfig {
    @Bean
    @Lazy
    Engine engine() { return new Engine(); }
}
```

## Bean - Scope

Scope - определяет как Bean будет создаваться:

-   singleton - поведение по умолчанию. Создает Bean один раз, и потом его просто возвращает
-   prototype - при каждом вызове создается новый экземпляр Bean. Каждый из них можно менять независимо друг от друга
-   RequestScope - создает новый экземпляр Bean на каждый HTTP запрос(хз как это работает)
-   SessionScope - создает новый экземпляр Bean на каждую сессию(хз как это работает)
-   ApplicationScope - хз как это работает

```java
//
@Component
@Scope("singleton")
class Engine{}

//
@Component
@Scope("prototype")
class Engine{}

//
@Component
@RequestScope
class Engine{}

//
@Component
@SessionScope
class Engine{}

//
@Component
@ApplicationScope
class Engine{}
```

## Bean - Profile

Proflie - можно указать, при каком Profile использовать Bean:

-   @Profile("dev") - использовать эти Bean если Profile является "dev"
-   @Profile("!dev") - использовать эти Bean если Profile не "dev"

```java
@Component
@Profile("dev")
class SomeClass { ... }

@Component
@Profile("!dev")
class SomeClass { ... }
```

Задание Profile через задание параметров запуска:

-   можно как-то програмно, но я не понимаю как там это делается
-   приведенный вариант можно в настройках конфигурации, в environment variable

```bash
spring.profiles.active=dev
```

## Configuration - AnnotationConfigpplicationContext

AnnotationConfigApplicationContext - считывает указанный класс(должен быть пмечен как Configuration), и загружает его Bean в текущий Context:

-   context - переменная в которую положили Context
-   SomeContextClass - класс в котором есть @Configuration
-   по хорошему надо оборачивать в try...resources
-   получаем Bean(это можно использовать для тестов, но в коде такого быть не должно)
    -   context.getBean("address") - получаем Bean по его имени
    -   context.getBean(Address.class) - получаем Bean по типу класса. Если таких несколько, [разрешает конфликт как при создании](#bean---создание)
    -   context.getBeanDefinitionNames - имена всех Bean
-   подходит, скорее для тестов

```java
public clss SomeMainClass {
    public static void main(String[] atgs) {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(
            SomeContextClass.class
        );

        context.getBean("address");
        context.getBean(Adress.class);
        context.getBeanDefinitionNames();
    }
}
```

## Configuration - Configuration

Configuration - аннотация, которая указывает, что в это классе есть Bean:

-   при прочтении класса загрузит Bean в Context

```java
@Configuration
class ConfClass { ... }
```
