# Сериализация/Десериализация

В java это все называется Serializable:

-   Сериализация - запись java-объекта в виде потока байт
-   Десериализация - превращение потока байт, обратно в java-объект

# Serializable

Serializable - магический интерфейс java, который автоматически сериализует/дисериализует java объект:

-   сериализация работает через Reflection api
    -   плюс: ничего не надо реализовывать руками
    -   минус: не оптимальная производительность
-   writeObject и readObject - методы в которых можно описать то, как должны сериализовываться и дисериализовываться объекты
    -   эти методы - костыль, и являются необязательными для переопределения

## Serializable - Сериализация

Для сериализации необходимо:

-   serialVersionUID - ID по которому идет проверка при десериализации
-   Поля
    -   Объекты хранящиеся в полях ДОЛЖНЫ быть Serilizable
    -   Поле может быть не Serilizable, но тогда оно должно быть transient
    -   Поля помеченные как static - не сериализуются
-   Методы
    -   нет необходимости в конструкторе
    -   необходим конструктор без параметров у родителя(если родитель есть)

## Serializable - Десериализация

Для десериализации необходимо:

-   serialVersionUID - поле с ID, по которому идет проверка при десериализации
    -   значение serialVersionUID должны совпадать, в "классе" и в "java-объекте в виде потока байт"
    -   serialVersionUID - вычисляется автоматически, и для объектов с одинаковыми полями, будет одинаково
    -   serialVersionUID - можно задать самому, например как константу, но тогда за версией следить придется самому
-   Поля
    -   При десериализации поля помеченные, как transient будут заполненны null
    -   Поля помеченные как static - не десериализуются
-   Методы
    -   нет необходимости в конструкторе
    -   необходим конструктор без параметров у родителя(если родитель есть)

# Externalizable

Externalizable - интерфейс java, который сериализует/дисериализует java объекты используя методы writeExternal/readExternal:

-   сериализация реализуется программистом
    -   плюс: можно написать производительнее чем стандартная
    -   минус: нужно писать, да еще и лучше чем стандартный
-   writeObject и readObject - методы в которых описываем то, как должны сериализовываться и дисериализовываться объекты
    -   эти методы - обязательны для переопределения

Для сериализации/десериализации необходимо:

-   необходимо так же объявить пустой конструктор без параметров
-   writeExternal
-   readExternal
-   в отличии от Serializable не способен десерилизовать final поле
    Externalizable -

# Примеры

## Serializable - Пример сериализации

Пример сериализации:

-   SavedGame - сериализуемый класс

```java
public class Main {

   public static void main(String[] args) throws IOException {

       //создаем наш объект
       String[] territoryInfo = {"У Испании 6 провинций", "У России 10 провинций", "У Франции 8 провинций"};
       String[] resourcesInfo = {"У Испании 100 золота", "У России 80 золота", "У Франции 90 золота"};
       String[] diplomacyInfo = {"Франция воюет с Россией, Испания заняла позицию нейтралитета"};

       SavedGame savedGame = new SavedGame(territoryInfo, resourcesInfo, diplomacyInfo);

       //создаем 2 потока для сериализации объекта и сохранения его в файл
       FileOutputStream outputStream = new FileOutputStream("C:\\Users\\Username\\Desktop\\save.ser");
       ObjectOutputStream objectOutputStream = new ObjectOutputStream(outputStream);

       // сохраняем игру в файл
       objectOutputStream.writeObject(savedGame);

       //закрываем поток и освобождаем ресурсы
       objectOutputStream.close();
   }
}
```

## Serializable - Пример десериализации

Пример десериализации:

-   SavedGame - сериализуемый класс

```java
public class Main {

   public static void main(String[] args) throws IOException, ClassNotFoundException {

       FileInputStream fileInputStream = new FileInputStream("C:\\Users\\Username\\Desktop\\save.ser");
       ObjectInputStream objectInputStream = new ObjectInputStream(fileInputStream);

       SavedGame savedGame = (SavedGame) objectInputStream.readObject();

       System.out.println(savedGame);
   }
}
```
