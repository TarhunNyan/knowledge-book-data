# Lombok

Помогает писать меньше однотипного кода

# Конструктор

Конструкторы для класса:

-   [NoArgsConstructor](#constructor---noargsconstructor)
-   [RequiredArgsConstructor](#constructor---requiredargsconstructor)
-   [AllArgsConstructor](#constructor---allargsconstructor)

Комбинированные аннатации:

-   [Data - в него входят Setter/Getter, toString, equals, hashCode, constructor](#methods---data)
-   [Value - объект immutable, в него входят Setter/Getter, toString, equals, hashCode, constructor](#methods---value)

# Методы

Методы, для работы с полями:

-   [Getter/Setter - генерация геттеров и сеттеров](#methods---gettersetter)
-   [Data - генерация геттеров и сеттеров](#methods---data)

Базовые методы:

-   [ToString - генерация toString](#methods---tostring)
-   [EqualsAndHashCode - генерация equals и hashCode](#methods---equalsandhashcode)

# Поля

Аннотации для полей:

-   [NonNull - указываем многим аннотациям, что необходимо проводить проверку на Null](#fields---nonnull)
-   [Cleanup - оборачивает поле в try-with-resource](#)

# Остальное

Остальное - тут аннотация, смысла в которых я не увидел:

-   [Synchronized - помеченный метод оборачивается в Synchronized](#)
-   [SneakyThrows - тихие исключения, в доке описано два случая когда стоит использовать](https://projectlombok.org/features/SneakyThrows)
-   [Builder](https://projectlombok.org/features/Builder)
-   [Log](https://projectlombok.org/features/log)

# Пример

## Fields - Cleanup

Cleanup - поле оборачивается в try-with-resources:

-   поле помеченное аннотацией оборачивается в try-with-resources
-   при очистки по умолчанию вызывает метод close
-   @Cleanup("dispose") - указывает что при очистке нужно вызвать dispose

```java
public class CleanupExample {
    public static void main(String[] args) throws IOException {
        @Cleanup InputStream
        in = new FileInputStream(args[0]);

        @Cleanup OutputStream("dispose")
        out = new CustomStream(args[1]);

        byte[] b = new byte[10000];
        while (true) {
            int r = in.read(b);
            if (r == -1) break;
            out.write(b, 0, r);
        }
    }
}
```

## Fields - NonNull

NonNull - указываем многим аннотациям, что необходимо проводить проверку на Null:

```java
public static class NoArgsExample {
    @NonNull private String field;
}
```

## Constructor - NoArgsConstructor

NoArgsConstructor - аннотации генерируют конструктор без аргументов, пишется у класса:

-   пишется у класса
-   статические поля игнорируются
-   наличие final полей роняет build
    -   чтобы не падало, используй @NoArgsConstructor(force = true)
        -   инициализирует final поля
        -   final поля заполняются: 0/false/null

```java
@NoArgsConstructor
public static class NoArgsExample {
    @NonNull private String field;
}
```

Без Lombok:

```java
public static class NoArgsExample {
    @NonNull private String field;
    public NoArgsExample() { }
}
```

## Constructor - RequiredArgsConstructor

RequiredArgsConstructor - аннотация генерирует конструктор, для всех полей:

-   работаем с неинициализированными полями final
-   добавляем для параметра @NonNull проверку

```java
@RequiredArgsConstructor
public static class NoArgsExample {
    private String field;
}
```

## Constructor - AllArgsConstructor

AllArgsConstructor - аннотация генерирует конструктор, для всех полей:

```java
@AllArgsConstructor
public static class NoArgsExample {
    private String field;
}
```

## Methods - EqualsAndHashCode

EqualsAndHashCode - создает методы equals и hashCode:

-   Можно использовать специальные аннотации для генерации hashCode
    -   @EqualsAndHashCode - для этой аннотации, можно помечать поля
        -   @EqualsAndHashCode.Exclude - отключаем ненужные поля для hashCode
    -   @EqualsAndHashCode(onlyExplicitlyIncluded = true) - для этой аннотации, можно помечать поля
        -   @EqualsAndHashCode.Include - подключаем нужные поля для hashCode

```java
@Getter
@Setter
@EqualsAndHashCode
public class Author {
    private int id;
    private String name;
    private String surname;
}
```

Без Lombok:

```java
public class Author {

    // геттеры и сеттеры ...

    @Override
    public int hashCode() {
       final int PRIME = 31;
       int result = 1;
       result = prime * result + id;
       result = prime * result + ((name == null) ? 0 : name.hashCode());
       result = prime * result + ((surname == null) ? 0 : surname.hashCode());
       return result;
    }

    @Override
    public boolean equals(Object o) {
       if (o == this) return true;
       if (!(o instanceof Author)) return false;
       Author other = (Author) o;
       if (!other.canEqual((Object)this)) return false;
       if (this.getId() == null ? other.getId() != null : !this.getId().equals(other.getId())) return false;
       if (this.getName() == null ? other.getName() != null : !this.getName().equals(other.getName())) return false;
       if (this.getSurname() == null ? other.getSurname() != null : !this.getSurname().equals(other.getSurname())) return false;
       return true;
    }
}
```

## Methods - Getter/Setter

Getter/Setter - автоматически генерируется getter/setter:

-   для boolean у getter имя будет isValue()
-   реализация
    -   getter - возвращает значение поля
    -   setter - принимает одно значение и задает его полю
-   аннотацию Getter/Setter можно задать для поля
    -   можно задать acces-modifier: PUBLIC, PROTECTED, PACKAGE и PRIVATE
-   аннотацию Getter/Setter можно задать для класса

```java
@Getter
@Setter
public class Author {
    private int id;
    private String name;
    @Setter(AccessLevel.PROTECTED)
    private String surname;
}
```

## Methods - Data

Data - совмещает в себе [ToString](#methods---tostring), [EqualsAndHashCode](#methods---equalsandhashcode), [Getter/Setter](#methods---gettersetter) и [RequiredArgsConstructor](#constructor---requiredargsconstructor)

```java
@Data
public class Author {
    private int id;
    private String name;
    @Setter(AccessLevel.PROTECTED)
    private String surname;
}
```

## Methods - Value

Value - создает immutable объект, совмещает в себе [ToString](#methods---tostring), [EqualsAndHashCode](#methods---equalsandhashcode), [Getter/Setter](#methods---gettersetter) и [RequiredArgsConstructor](#constructor---requiredargsconstructor):

-   все поля становятся final и private
-   класс помечается как final
-   сеттеры не генерируются

```java
@Value
public class Author {
    private int id;
    private String name;
    @Setter(AccessLevel.PROTECTED)
    private String surname;
}
```

## Methods - ToString

ToString - класс аннотированный ToString, сгенерирует метод для всех полей:

-   генерирует строку в таком формате:
    -   "{Classname}({FieldValue1},{FieldValue2},{...})"
-   includeFieldNames=true - генерирует строку в таком формате:
    -   "{Classname}({FieldName1}={FieldValue1},{FieldName1}={FieldValue2},{...})"

```java
@ToString(includeFieldNames=true)
public class Author {
    private int id;
    private String name;
    private String surname;
}
```

Без Lombok:

```java
public class Author {
    private int id;
    private String name;
    private String surname;

    @Override
    public String toString() {
      return "Author(id=" + this.id + ", name=" + this.name + ", surnname=" + this.surname + ")";
  }
}
```

## With

With - аннотация immutable объектов, чтобы быстро создавать их копии, при изменении всего одного параметра:

-   необходим конструктор со всеми полями

```java
public class User {
    private final String username;
    private final String emailAddress;
    @With
    private final boolean isAuthenticated;

    //getters, constructors
}
```

По сути делает следующее:

```java
public class User {
    private final String username;
    private final String emailAddress;
    private final boolean isAuthenticated;

    //getters, constructors

    public User withAuthenticated(boolean isAuthenticated) {
        return this.isAuthenticated == isAuthenticated ? this : new User(this.username, this.emailAddress, isAuthenticated);
    }
}
```
