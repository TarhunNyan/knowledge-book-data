# Reflection

Reflection - библиотека способная анализировать код в runtime и создавать новые классы в runtime. Является магией и основой многих бибилотек в Java

# Reflection-class

Reflection-class - это такие сущности, на основе которых в java строятся:

-   class
-   interface
-   примитивы(int, boolean, ...)

Reflection-class - содержит информацию о создаваемых объектах:

-   [Получить Reflection-class одним из 3 способов](#reflection-class---получить)

Reflection-class работаем с полями:

-   [Получить поле доступное в class](#reflection-class---получить-поле)
-   [Получить все публичные поля](#reflection-class---получить-все-public-поля)
-   [Получить все поля, включая приватные](#reflection-class---получить-все-поля)

# Примеры

## Reflection-class - Получить поле

Получить поле:

```java
Class cl_employee1 = Employee.Class;
Field id_field = cl_employee1.getField("id");
```

## Reflection-class - Получить все public поля

Получить все public поля:

```java
Class cl_employee1 = Employee.Class;
Field[] fields = cl_employee1.getFields();
```

## Reflection-class - Получить все поля

Получить все поля:

-   получаем так же private поля

```java
Class cl_employee1 = Employee.Class;
Field[] allFields = cl_employee1.getDeclaredFields();
```

## Reflection-class - Получить тип поля

Получить тип поля:

```java
Class cl_employee1 = Employee.Class;
Field id_field = cl_employee1.getField("id");
Field type_field = id_field.getType();
```

## Reflection-class - Получить

Можно получить Reflection-class тремя способами:

```java
// Способ 1
Class cl_employee1 = Employee.Class;

// Способ 2
Class cl_employee2 = Class.forName("com.example.org.package.Employee");

// Способ 3
Employee emplyee = new Employee();
Class cl_employee3 = emplyee.getClass();
```
