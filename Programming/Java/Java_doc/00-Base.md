# JavaDoc

через комментарий позволяет создать документацию к Class, Interface, методу или полю

```java
/**
 * Method - Описание метода
 * @param x - первый параметр
 * @param y - второй параметр
 * @return int - возвращаемое значение
 **/
int kek() { return 0; };
```

JavaDoc для Class или Interface:

```java
/**
 * @author Cucmber
 * @version 0.0.1a
 * @see JavaDocLink - кликабельный текст-ссылка на что-то в проекте
 */
class DocumentationClass {}

/**
 * @deprecated если интерфейс или класс устарел
 */
@Deprecated interface DprecatedInterface {}
```

JavaDoc для метода(конструктора):

```java
/**
 * @param x описание первого аттрибута
 * @param y описание второго аттрибута
 * @throws PrinterException какое-то пояснение к ошибке. Вместо @trows можно использовать @exception, это одно и то же
 * @see JavaDocLink - кликабельный текст-ссылка на что-то в проекте
 * @return описание того что возвращаем
 */
int documentationMethod(int x, int y) throws PrinterException {
    return 0;
}

/**
 * @deprecated если метод устаред
 */
@Deprecated void deprecatedMethod(){}
```

JavaDoc для поля:

```java
/**
 * Текстовое описание поля
 * @see JavaDocLink - кликабельный текст-ссылка на что-то в проекте
 */
public String path;
```

JavaDoc использование link:
можно сделать ссылку на какой-то объект в проекте или какой-то модуль. Особено актуально для полей see и deprecate. Например:

```java
* @see package.Class#method(Type, Type,...)
```

или

```java
* @deprecated если интерфейс или класс устарел {@link Class#field}
```

Как прописывать ссылки до интересующих мест:

-   #field
-   #Constructor()
-   #method()
-   Class
-   Class#field
-   Class#Constructor()
-   Class#method()
-   package
-   package.Class
-   package.Class#field
-   package.Class#Constructor()
-   package.Class#method()
