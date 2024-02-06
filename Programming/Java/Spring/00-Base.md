# Spring Boot

Spring - древний framework. Огромный комбайн для всего. Есть во всех вакансиях на Java-разработчика

SpringBoot - объединяет популярные Spring-фреймворки, и предоставляет некоторые обертки

# Spring - База

## Понятия Spring

IoC(Inversion of Control) - инверсия контроля:

-   обычно пользовательский код вызвает библиотеку, а в Spring наоборот, библиотека вызывает пользовательский код

DI(dependency injection) - внедрение зависимостей:

-   шаблон при котором объект, получает объекты от которых он зависит
-   отделяет создание объектов от их использования

IoC конейнер - так назвается реализация в Spring принципов IoC и DC:

-   ApplicationContext - штука в которой храняться объекты Bean, по сути и есть IoC

Bean - заинициализированный объект, хранится внутри ApplicationContext:

-   BeanDefenition - описывает Bean, который создаст BeanFactory
-   BeanFactory - интерфес, который создает Bean, на основе BeanDefinition
-   ApplicationContext - то куда попадают Bean, созданные в BeanFactory

## IoC конейнер

Рабочий процесс Spring IoC:

-   создаем BeanDefenition
    -   используя XML или через Java Annotation
    -   в конфигурациях BeanDefenition указываем классы, которые будут Bean
    -   указываем какие файлы конфигурации нужны, для создания context
-   при старте приложения вызваем Spring
    -   Spring создает все указанные Bean
    -   Spring записывает все Bean внутрь Context

## Bean

Bean - объект(экземпляр класса), который автоматически создается Spring и храниться в специальном контейнере(Context):

-   в конструктор класса, помеченного как Bean, в качестве аргументов, Spring сам подставляет Bean

Правила подстановки Bean:

-   подставляет Bean с **типом** и **именем** как у аргумента
-   подставляет Bean с **типом** как у аргумента
-   подставляет Bean, являющимся потомком от указанного типа
    -   это особенно часто встречается с интерфейсами, когда Spring автоматически подставляет имплементации от интерфейса

Если по правилам подстановки подходит несколько Bean:

-   ищет Bean с пометкой @Primary
-   через @Qualifier можно указать какой Bean мы ожидаем
-   через @Profile можно указать какой Bean мы ожидаем, в зависимости от Environment Variable
-   если не может определить нужный Bean, то просто падает

## Жизненный цикл ApplicationContext

ApplicationContextEvent - основной класс для событий, возникающих в процессе жизненного цикла ApplicationContext

-   ContextRefreshedEvent - (refresh)публикуется автоматически, после поднятия контекста
-   ContextStartedEvent - (start)публикуется методом ApplicationContext#start
-   ContextStoppedEvent - (stop)публикуется методом ApplicationContext#stop
-   ContextClosedEvent - (close)публикуется автоматически, перед закрытием контекста

## Жизненный цикл Bean

Жизненный цикл Bean:

-   Инициализация Bean
-   Запихивание Bean в контейнер
-   @PostConstruct - метод bean, вызываемый после инициализации Bean, обычно метод называют init
-   @PreDestroy - метод bean, при вызове у Container методв close, то есть при уничтожении Bean

# Spring - Boot

Spring Boot - предоставляет крутые аннотации облегчающие жизнь:

-   [@SpringBootApplication - запуск приложения](#spring-boot---springbootapplication)
-   [@ComponentScan - автоматический поиск Bean в пакетах приложения](#spring-boot---componentscan)
-   [@EnableAutoConfiguration - автоматическая настройка БД](#spring-boot---enableautoconfiguration)

# Spring - Core

Spring Core - раздел SpringBoot, в котором находятся основные аннотации для управления Bean

-   [@Bean - помечаем метод/класс, как создающий Bean](#spring-core---bean)
-   [@Primary - приорететный Bean](#spring-core---primary)
-   [@Qualifier - определяет приоритеты подстановки Bean](#spring-core---qualifier)
-   [@Profile - использовать различные Bean в зависимости от Environment Variable](#spring-core---profile)
-   [@DependsOn - укзывает после какого Bean инициализировать текущий](#spring-core---dependson)
-   [@Lazy - отложенная инициализация Bean, до первого вызова](#spring-core---lazy)

Аннотация связанные с типом(singlton/prototype) создаваемого Bean:

-   [@Scope - пометка, отвечающая за то, singleton/prototype будет Bean](#spring-core---scope)
-   [@LookUp - позволяет прокинуть prototoype в singleton](#spring-core---lookup)

Импортирование Bean/property:

-   [@Import - импортирование Bean или класса с Bean'ами](#spring-core---import)
-   [@PropertySource - импортрование файлов .properties](#spring-core---propertysource)

Устаревшие аннотации:

-   [@Required - требует указание в XML нужного Bean для метода]()
-   [@ImportResource - импортирует указанный xml]()

# Spring - Configuration

Spring Configuration - это вроде не отдельный раздел Spring, но даже в доке его выделяют отдельно:

-   [Подглядеть какие Bean появились в контейнере](#configuration---annotationconfigpplicationcontext)
-   [@Value - задает базовое значение для входных аттрибутов Bean](#spring-configuration---value)
-   [@Configuration - помечает, что в текущем классе есть Bean](#spring-configuration---configuration)
-   [@Autowired - автоматически подтягиваем Bean в @Configuration](#spring-configuration---autowired)

# Spring - Data

Spring Data - раздел SpringBoot, который реализует ORM

-   ORM(Object Relation Mapping) - концепция, взаимодействия с БД не через SQL, а через объекты в коде

Создание CRUDRepository:

-   [CRUDRepository - что это?](#spring-data---crudrepository)
-   [Reposittory - что это?](#spring-data---repository)
-   [Repostirory - создание запросов](#spring-data---создание-запросов-для-repository)
-   [Repostirory - создание запросов через @Query](#spring-data---query)
-   [Model - что это?](#spring-data---model)

Отношение таблиц(выбираются по принципу: "кто о ком НЕ должен знать"):

-   [Отношения таблиц(ManyToOne, OneToMany, ...)](#spring-data---отношения-таблиц)
-   [FetchType - дает доступ к ленивой подгрузке](#spring-data---fetchtype)

Аннотации связывающие БД, Model и Repository:

-   [@Entity - связываем java-класс с таблицей в БД](#spring-data---entity)
-   [@Id - указываем, какое поле является ID в твблице](#spring-data---id)
-   [@Table - указываем имя таблицы и schema в которой таблица лежит](#spring-data---table)
-   [@Column - позволяет указать имя колонки для таблице в БД](#spring-data---column)
-   [@Transient - поле которое присутствует в модели, но не храним в БД](#spring-data---transient)
-   [@Query - создание запросов для Repository](#spring-data---query)

Основные аннотации:

-   [@Transactional - метод работающий с БД, превращает в транзакцию](#spring-data---transactional)
-   [@NoRepository/@Repository - интерфейс становится не-ррепозиторием/репозиторием](#spring-data---norepositoryrepository)

Устаревшие аннотации:

-   [@EnableTransactionManagement - разрешаем транзакции](#spring-data---enabletransactionmanagement)

## JPA

JPA(Java Persistence API) - стандарт в JAVA, в котором прописан набор интерфейсов. Если библиотека реализует эти интерфейсы, то библиотека считается JPA. JPA состоит из:

-   API - способ взаимодействия с библиотекой
-   Метаданные - в случае языка Java это аннотации, которые помечают какие поля в объекте как связанны с таблицами в БД
-   JPQL - язык запросов библиотеки, который заменяет SQL
    -   HQL - язык запросов в Hibernate

Hibernate - популрная имплементация JPA:

-   по сути сначала появился Hibernate, а после вдохновляясь им создали JPA

## Устройство JPA

<img src="./source/00-JPA.jpg">

Persistence - интерфейс в JPA отвечает за инициализацию библиотеки реализующей JPA

-   сканит файлы, на описание того, какие классы являются сущностяими
-   создает EntityManagerFactory

EntityManagerFactory - тяжеловесный интерфейс задача которого создавать EntityManager

-   sessionFactory - так в Hibernate называется EntityManagerFactory

EntityManager - интерфейс, через который можно открыть транзакцию и совершить запись

-   session - так в Hibernate называется EntityManager

Entity - прокси класс, в который оборачиваются сущности

## Models

Models - в JPA это классы, с которыми мы работаем как с таблицами

Требования к Model:

-   POJO - быть объектам Java без всяких сложных оберток
-   Пустой конструктор, который public или private
-   Поля должны быть не final и быть доступны только через getter/setter
-   Не может быть вложенным классом или Enum
-   Должен иметь хотя бы одно поле с аннотацией @Id

# Примеры

## Spring Data - Создание запросов для Repository

Создание запросов для Repository, можно создавать запросы:

-   названиями методов и набором передаваемых параметоров
-   JPQL(SQL/HSQL/...) запросом

Создание запросов через имена методов:

-   Hibernate - аннализирует названия методов в интерфейсах и на их основе создает SQL-запросы
-   по сути через имена методов, генерируется запрос, где по породяку подставляются аргументы описанной функции
-   сложные запросы так писать не получится
-   Как из имен методов получается запрос: [ссылочка на документацию](https://docs.spring.io/spring-data/jpa/reference/jpa/query-methods.html)

```java
public interface BookRepository extends CrudRepository<Book, Long> {
    List<Book> findByYear(Integer someYear);
    List<Book> findByLastnameAndFirstname(String someLastname, String someFirstname);
    List<Book> findDistinctByLastnameAndFirstname(String someLastname, String someFirstname);
    List<Book> findByStartDateBetween(Date firstDate, Date secondDate);
}
```

## Spring Data - Query

Создание запросов через JPQL(SQL/HSQL/...):

-   вместо имени таблицы, используется имя класса
-   вместо имени колонки таблицы, используется поле класса
-   ?1 - использовать подстановку параметров метода

```java
public interface BookRepository extends CrudRepository<Book, Long> {
    @Query("SELECT aob.book from AuthorOfBook aob join aob.author "
         + "where aoub.author.lastname = ?1"
    )
    List<Book> findByAuthor(Integer someAuthor);
}
```

## Spring Data - FetchType

FetchType - указывает, как грузить таблицы:

-   этот параметр - есть у отношений между таблицами
-   EAGER - загрузит все одним большим SQL
-   LAZY - загрузит родителя а потомков заменит PROXY, пока их не вызовут, грузить их не будет

```java
@Entity
@Data
@Table(name = "geo_item_style_icon_file_link", schema = "geo")
public class GeoItemStyleIconLink extends AbstractFileLink {

    @OneToMany(fetch = FetchType.LAZY, mappedBy = "user")
    private GeoItemStyle geoItemStyle;

    @OneToMany(fetch = FetchType.EAGER, mappedBy = "user")
    private GeoItemStyle geoItemStyle;
}
```

## Spring Data - Отношения таблиц

@ManyToOne - покупатель может относиться к одной из групп(школьник, пенсионер, студент):

-   класс в котором стоит эта аннотация становится "владельцем"
-   класс с которым связан, не знает об этой связи
-   хранит id'шники внутри поля на "зависимую" сущность

@OneToMany - у пользователя может быть много адресов, а в адресах не нужна информация о пользователях:

-   класс в котором стоит эта аннотация становится "владельцем"
-   класс с которым связан, не знает об этой связи
-   хранит КОЛЛЕКЦИЮ с id'шниками внутри поля на "зависимую" сущность

@ManyToMany - у исполнителя может быть несколько задач, а у задач может быть несколько исполнителей:

-   обе сущности знают друг о друге
-   обе сущности аннотированны
-   указание сущности владельца(оптимизирует число таблиц, логически смысла не имеет)
    -   @ManyToMany(mappedBy = "roles")
        -   "roles" - сущность которая является владельцем

@OneToOne - исполнителю можно назначить задачу, а у задачи должен быть только один исполнитель:

-   класс в котором стоит эта аннотация становится "владельцем"
-   класс с которым связан, не знает об этой связи
-   Hibernate будет следить, чтобы на сущность ссылалились из аннотированного класса только один экземпляр

@OneToOne(cascade = CascadeType.ALL, mappedBy="employee") - исполнителю можно назначить задачу, а задаче можно назначить одного исполнителя:

-   Hibernate будет следить, чтобы на сущность ссылалились из аннотированного класса только один экземпляр и НАОБОРОТ

@Embedded:

-   связь типа один к одному
-   находим таблицу по аннотированному полю и "вставляет" поля из нее в аннотированную
-   в бд таблица одна, а в классах появляется иерархия

```java
@Entity
@Data
@Table(name = "geo_item_style_icon_file_link", schema = "geo")
public class GeoItemStyleIconLink extends AbstractFileLink {

    @OneToMany(mappedBy = "user")
    private GeoItemStyle geoItemStyle;
}
```

## Spring Data - Model

Model - класс, в котором есть описание полей этого класса и таблиц в БД:

-   [@Entity - связываем java-класс с таблицей в БД](#spring-data---entity)
-   [@Id - указываем, какое поле является ID в твблице](#spring-data---id)
-   [@Table - указываем имя таблицы и schema в которой таблица лежит](#spring-data---table)
-   [@Column - позволяет указать имя колонки для таблице в БД](#spring-data---column)
-   [@Transient - поле которое присутствует в модели, но не храним в БД](#spring-data---transient)

Всем полям модели, нужны Setter/Getter методы:

-   с Lombock можно использовать @Data

```java
@Data
@Entity(name="student")
public class Student {
    @Id
    @GeneratedValue(strategy=GenrationType.AUTO)
    private Long id;

    private String name;
}
```

## Spring Data - Transient

@Transient - поле которое присутствует в модели, но не храним в БД:

```java
@Entity
@Data
@Table(name = "Users")
public class User {

    @Id
    private Integer id;

    private String email;

    private String password;

    @Transient
    private Date loginTime;

}
```

## Spring Data - Column

Column - позволяет указать имя колонки для таблице в БД:

```java
@Entity
@Data
@Table(name = "view_column", schema = "jetaview")
public class ViewColumn {
    @Column(name = "order_index")
    private Integer orderIndex;
}
```

## Spring Data - Table

Table - указывает, имя таблицы(если не хотим стандартное) и schema(это для PostgreSQL):

-   @Table(name = "view_column", schema = "jetaview") - указываем name таблицы и в какой schema она находится
    -   можно указать только name или только schema

```java
@Entity
@Data
@Table(name = "view_column", schema = "jetaview")
public class ViewColumn { ... }
```

## Spring Data - Entity

Entity - указывает Hibernate, что надо связать объект с таблицей:

-   каждый экземпляр класса Student - предаставляет строку таблицы
-   @Entity - указывает что Hibernate надо связать объект с таблицей student
-   @Entity(name="student") - явно указывает что Hibernate надо связать объект с таблицей student

```java
@Entity
public class Student { ... }

@Entity(name="student")
public class Student { ... }
```

## Spring Data - ID

ID - указывает, какое поле является ID, для таблицы:

-   @Id - в Model обязательно должно быть хотябы одно поле помечено как @Id
-   @GeneratedValue - то как Id генерируется. Есть разные стратегии, по дефолту increment

```java
@Entity(name="student")
public class Student {
    @Id
    @GeneratedValue(strategy=GenrationType.AUTO)
    private Long id;

    private String name;
}
```

## Spring Data - CrudRepository

CrudRepository - особый интерфейс, являющийся базовым, предоствляющий возможности работаты с БД:

-   чтобы работать с БД нужно заинициализировать CrudRepository
-   интерфейсам, которые работают с БД нужно имплеменитировать CrudRepository, чтобы Hibernate создал связь с таблицами в БД

Создание CrudRepository в Spring:

-   @EnableJpaRepository(basePackages="pro.sisit.unt9.data") - указываем Hibernate чтобы начал работать
-   SpringDataApplication - класс, являющийся точкой входа в приложение
-   это просто шаг который надо сделать, а в современном SpringBoot возможно и не надо... не знаю )

```java
@EnableJpaRepository(basePackages="pro.sisit.unt9.data")
public class SpringDataApplication { ... }
```

CrudRepository по умолчанию имеет такие методы как:

-   count()
-   findAll()/findAllById(Iterable<ID> ids)/findById(ID id)
-   save(S entity)/saveAll(Iterable<S> entities)
-   delete(T entity)/deleteAll()/deleteAll(Iterable<? extends T> entities)/deleteAllById(Iterable<? extends ID> ids)/deleteById(ID id)
-   existsById(ID id)

## Spring Data - Repository

Repository - особый интерфейс, котрый Hebirnate считывает и связывает с таблицами в БД:

-   работа Hibernate идет через интерфейсы Repository
-   CrudRepository - имплементировав свой интерфейс от CrudRepository, запустишь магию Hibernate

Создание Repository в Spring Data:

-   ViewColumn - модель по которой Spring Data строит таблицу
-   Long - тип, который использует id этой таблицы

```java
public interface ViewColumnRepository extends CrudRepository<ViewColumn, Long> {
}
```

Использование Repository в Spring Data:

-   для использования Repository не нужно создавать имплементацию Repository
-   достаточно указать нужный тебе Repository и Spring сам подставит Bean, который сам же создал

```java
public interface ViewColumnRepository extends CrudRepository<ViewColumn, Long> {
}

@Service
public ViewColumnService {
    ViewColumnRepository viewColumnRepository;
}
```

## Spring Data - NoRepository/Repository

NoRepository/Repository

## Spring Data - EnableTransactionManagement

EnableTransactionManagement - использовав аннотацию на классе, мы разрешим транзакции в его Bean

-   можно использовать только в классе помеченном как @Configuration
-   в современных версиях Spring Boot, не надо, транзакции разрешены по умолчанию

```java
@Configuration
@EnableTransactionManagement
public class PersistenceJPAConfig{

   @Bean
   public LocalContainerEntityManagerFactoryBean entityManagerFactory() {
       //...
   }

   @Bean
   public PlatformTransactionManager transactionManager() {
      JpaTransactionManager transactionManager = new JpaTransactionManager();
      transactionManager.setEntityManagerFactory(entityManagerFactory().getObject());
      return transactionManager;
   }
}
```

## Spring Data - Transactional

Transactional - аннотация, которая делает чтение/запись в БД атомарной(либо все изменения, либо никаких)

-   все в пределах метода, будет работать с БД в пределах одной транзакции
-   проблемы
    -   если метод A, вызывает метод B помеченный как Transactional, из того же класса, то Transactional не сработает
    -   откат транзакции происходит не при всех Exception, для исправления это несправедливости, используй свойство rollbackFor
    -   метод помеченный Transactional, должен быть public
    -   Transactional забирает соединение с БД, поэтому то, что находится внутри метода, должно работать быстро и без задержек

Помечаем все методы Service как Transactional:

```java
@Service
@Transactional
public class FooService {
    //...
}
```

Помечаем метод как Transactional:

-   rollbackFor = { Exception.class } - откат транзакции при любых исключениях
-   noRollbackFor = { SQLException.class } - не откатывать транзакцию при исключении SQLException
-   readOnly = true - транзакция только на чтение

```java
@Service
public class FooService {
    @Transactional
    public void createCourseDeclarativeWithRuntimeException(Course course) {
        ...
    }

    @Transactional(rollbackFor = { Exception.class })
    public void createCourseDeclarativeWithCheckedException(Course course) throws SQLException {
        ...
    }

    @Transactional(noRollbackFor = { SQLException.class })
    public void createCourseDeclarativeWithNoRollBack(Course course) throws SQLException {
        ...
    }

    @Transactional(readOnly = true)
    public void createCourseDeclarativeWithNoRollBack(Course course) throws SQLException {
        ...
    }
}
```

## Spring Boot - EnableAutoConfiguration

EnableAutoConfiguration - автоматическая настройка БД. Входит в SpringBootApplication

## Spring Boot - SpringBootApplication

SpringBootApplication - точка запуска для приложений на основе SpringBoot

-   Содержит в себе @ComponentScan - сканирует пакет в котором запущен
-   Содержит в себе @Configuration - можно в этом же классе определить bean

```java
@SpringBootApplication
public class Application {

	public static void main(String[] args) {
		SpringApplication.run(Application.class, args);
	}

}
```

## Spring Boot - ComponentScan

ComponentScan - ищет Bean в пакетах и подпакетах:

-   ищет все, что помечено @Component, @Service, @Repository и @Configuration
-   ищет внутри пакет, в котором указан
-   basePackages - базовые пакеты в которых сканируем Bean
-   basePackages - базовые пакеты в которых сканируем Bean
-   excludeFilters - фильтры которые исключают пакеты для сканировния Bean

```java
@ComponentScan(
    basePackages = {
        "com.baeldung.componentscan.springapp.animals",
        "com.baeldung.componentscan.springapp.flowers"
    },
    excludeFilters = {
        @ComponentScan.Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
        @ComponentScan.Filter(type = FilterType.REGEX, pattern="com\\.baeldung\\.componentscan\\.springapp\\.flowers\\..*")
    }
)
public @interface SpringBootApplication {
    ...
}
```

## Spring Core - PropertySource

PropertySource - позволяет импортировать настройки для проекта лежащие в .properties:

-   application.properties или application.xml - стандартные файлы для свойств Spring-проекта

```java
@Configuration
@PropertySource("classpath:/annotations.properties")
@PropertySource("classpath:/vehicle-factory.properties")
class VehicleFactoryConfig {}

@Configuration
@PropertySources({
    @PropertySource("classpath:/annotations.properties"),
    @PropertySource("classpath:/vehicle-factory.properties")
})
class VehicleFactoryConfig {}
```

## Spring Core - Import

Import - позволяет импортировать Bean или класс с Bean'ами:

-   обычно все Bean импортируют через [@ComponentScan](#spring-boot---componentscan)

```java
@Import(VehiclePartSupplier.class)
class VehicleFactoryConfig {}
```

## Spring Core - Profile

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

## Spring Core - LookUp

LookUp - какого-то хрена, позволяет игнорировать тело метода и вместо него возвращает Bean, по типу указанному в сигнатуре метода

-   решает проблему, когда мы хотим внутри Bean типа Singleton, использовать Bean типа Prototype

Пример использования LookUp:

-   StudentServices - это Beab типа Singleton
-   getNotification и getAntoherBean - будут возвращать новый объект типа SchoolNotification
    -   можно вместо LookUp, использовать получение Bean через context, но это означает что класс значет о Spring(какой бред...)
-   @Lookup("SchoolNotification") - указывает конкретный Bean, который хотим возвращать в обход сигнатуры

```java
@Component
@Scope("prototype")
public class SchoolNotification {
    // ... prototype-scoped state
}

@Component
public class StudentServices {

    @Lookup
    public SchoolNotification getNotification() {
        return null;
    }

    @Lookup("SchoolNotification")
    public Penis getAntoherBean() {
        return null;
    }
}
```

## Spring Core - Scope

Scope - определяет как Bean будет создаваться:

-   singleton - создает Bean один раз, и потом его просто возвращает
    -   поведение по умолчанию
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

## Spring Core - Lazy

Lazy - обычно Bean создаются при запуске, но с аннотацией Lazy, инициализация откладывается до первого вызова:

-   @Bean с аннотацией @Lazy - откладывает создание Bean
-   @Configuration с аннотацией @Lazy - откладывает создание всех Bean внутри класса помеченного @Configuration
-   @Autowired с аннотацией @Lazy - закидывает proxy, до вызова Bean

Lazy для конкретного Bean:

-   @Lazy - применить режим Lazy
-   @Lazy(false) - не применять режим Lazy

```java
@Configuration
class VehicleConfig {
    @Bean
    @Lazy
    Engine engine() { return new Engine(); }

    @Bean
    @Lazy(false)
    Engine motor() { return new Motor(); }
}
```

## Spring Core - DependsOn

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

## Spring Core - Qualifier

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

## Spring Core - Bean

@Bean - помечает метод, как возвращающий Bean:

-   Spring будет хранить то, что вернул Method внутри Context
-   класс должен быть помечен как @Configuration

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

## Spring Core - Primary

@Primary - в случаем, когда выбор между несколькими Bean, выбирает с пометкой Primary:

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

## Spring Configuration - Value

Value - задает указанное значение:

-   работает только в пределах Configurtion

```java
@Value("8")
int cylinderCount;

@Vaule("8")
void setCylinderCount(int cylinderCount) { ... }

void setCylinderCount(@Value("8") int cylinderCount) { ... }
```

## Spring Configuration - Configuration

@Configuration - аннотация, которая указывает, что в этом классе есть Bean:

-   при прочтении класса загрузит методы помеченные @Bean в Context

```java
@Configuration
class ConfClass { ... }
```

## Spring Configuration - Autowired

Autowired - задает значение автоматически, подходящим Bean:

-   работает только в пределах Configuration
-   работает на полях, setter и конструкторе
-   required = false - говорит Spring, что если нет подходящего Bean, не падаем, а продолжаем работу

```java
@Configuration
class ... {
    @Autowired
    final Person person;

    @Autowired(required = false)
    final Cat cat;

    @Autowired
    void setSomeVar(Person person) { ... }
}
```

#

## Структура Bean проекта

Структура Bean проекта, которая используется везде:

-   БД - база данных с нужными нам данными
-   Repository - слой доступа к данным. Короче каждый Repository, типа сиквел запроса. Хранит или дает доступ к каким-то данным
-   Service - манипуляция с данными и какая-то "бизнес-логика"
-   Component - выдает какой-то финальный результ, типа отправляет данные клинету

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

## Bean - @Component

Помечаем Class как bean:

```java
@Component
class SomeClassName { ... }

@Component(name="anotherNameToBean")
class SomeClassName2 { ... }
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

#

---

-   @Component - указывает что из этого класса будет создан компонент Spring
-   @Controller - это как @Component, но включает объект model для передачи данных
-   @ResponseBody - через нее можно указать формат ответа(XML, JSON)
-   @RestController - совмещает @Controller и @ResponseBody

Mapping и приколы связанные с ними:

-   @RequestMapping(value = "/coffees", method = RequestMethod.GET)
    -   в классе помеченнои как RestController, можно метод пометить черезе @RequestMapping
    -   можно пометить сам класс аннотацией @RequestMapping с указанием value, тогда @RequestMapping для методов будет относительно классовой аннотации
    -   @RequestMapping - создает routing для помеченного метода
    -   то что веренет метод, помеченный @RequestMapping, SpringWeb преобразует в json(?)
    -   value = "/coffees/{id}" - в пути можно указать аттрибут(id), который передастся в наш метод в качестве параметра
-   @GetMapping/@PostMapping/@PutMapping/@PatchMapping/@DeleteMapping - как @RequestMapping, но метод по умолчанию стоит Get/Post/Put/Patch/Delete

```java
@GetMapping("/cofees/{id}")
Optional<Coffee> getCoffeeById(@PathVariable String id) {
    for(Coffee el: coffees) {
        if( el.getId().equals(id) ) {
            return Optional.of(el);
        }
    }
    return Optional.empty();
}
```

Пример создания объекта через post запрос:

-   @RequestBody - пометка, что JSON надо преобразовать в объект Coffee
-   return cofee; - объект Coffee в виде JSON

```java
@PostMapping("/cofee")
Coffee postCofee(@RequestBody Coffee coffee) {
    cofees.add(coffee);
    return coffee;
}
```

Пример получения значения из JSON:

-   @RequestParams() - пометка, что JSON надо преобразовать в объект Coffee
-   return cofee; - объект Coffee в виде JSON

```java
@PostMapping("/name")
String postName(@RequestParam(name = "name") String name) {
    return name;
}
```

---

-   @PostConstruct - срабатывает после инита у @Component
    -   автор предлагает использовать его для забивания в БД дефолтных значений в БД

---

Конфигурация

Создание конфигурации для Spring. Вообще много откуда тащаться значения для конфигурации

Через файлы application.yaml:

-   projectname/src/main/resources/application.yml
    -   путь до конфигурации наших проектов

Как получить значение из конфигурационного файла:

-   вытаскивает значение из application.yaml лежищще в etadata.broker-url
-   : Default - значение по умолчанию
-   SpEL - язык выражений в Spring, с помощью которого вытаскиваем значения

```java
@Value("${etadata.broker-url}")
private String name;

@Value("${etadata.broker-url: Default}")
private String name;
```

Вместо @Value можно использовать @ConfigurationProperties:

-   это application.properties

```properties
local-server=false
auth.security-dir=/security
auth.local-server=true
auth.issuer-uri=http://127.0.0.1:${server.port}
auth.jwks-uri=${auth.issuer-uri}/oauth2/jwks
```

-   это какой-то код в java
-   найдет в application.properties свойства с префиксом auth и подставит из них значения

```java
@Configuration
@ConfigurationProperties(prefix = "auth")
@Data
public class AuthProperties {
   private boolean localServer;
   private String securityDir;
}
```
