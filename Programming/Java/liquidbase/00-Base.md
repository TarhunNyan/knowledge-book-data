# Liquidbase

Liquidbase - библиотека для Java, котрая контролирует структуру БД в соответствии с версией:

-   DB2
-   H2
-   HyperSQL
-   Firebird
-   MariaDB
-   MS SqlServer
-   Oracle
-   PostgreSQL
-   Snowflake
-   SQLite

Принцип работы простой:

-   changelog.cumulative.xml - описываешь в файле начальную структуру БД
-   changelog - файл, который ты добавляешь, при изменении структуры БД, где описываешь изменения
-   запускаешь Liquidbase, который сам проверяет соответсвие БД с описанными тобой изменениями
-   если необходимо что-то изменить, Liquidbase создает SQL запрос, и сам меняет структуру БД
    -   если ты удаляешь какой-то столбец - данные теряются

# Установка

Есть какая-то установка на win, linux, docker... Но зачем это все, если можно просто установить как пакет для java через gradle и радоваться жизни?

# Файлы описание структуры БД

Вообще можно использовать:

-   SQL
-   JSON
-   YARN
-   XML
    -   рекомендуется, ибо единственный для кого есть автодополнения через Idea

В этой записке, описывается только XML

## Структура changelog.cumulative.xml

Структура changelog.cumulative.xml - рекомендуется здесь не прописывать changeset, подключать другие файлы:

-   include
-   includeAll

```xml
<?xml version="1.0" encoding="UTF-8"?>
<databaseChangeLog
    xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:ext="http://www.liquibase.org/xml/ns/dbchangelog-ext"
    xmlns:pro="http://www.liquibase.org/xml/ns/pro"
    xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog
        http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-latest.xsd
        http://www.liquibase.org/xml/ns/dbchangelog-ext http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-ext.xsd
        http://www.liquibase.org/xml/ns/pro http://www.liquibase.org/xml/ns/pro/liquibase-pro-latest.xsd">

    <include file="src/main/resources/db/changelog/init-custumer.xml"/>
    <include file="src/main/resources/db/changelog/init-items.xml"/>
    <includeAll path="src/main/resources/db/changelog/init-fodler"/>

</databaseChangeLog>
```

Структура папок для примера выше:

```bash
.
└── src
    └── main
        └── resources
            └── db
                ├── changelog
                |   ├── init-fodler
                |   |   ├── init-something.xml
                |   |   └── init-something2.xml
                |   ├── changelog.cumulative.xml
                |   ├── init-custumer.xml
                |   └── init-items.xml
                ├── DatabasePool.java
                └── AbstractDAO.java
```

## Структура файлов Liquidbase

Структура файлов Liquidbase:

-   databaseChangeLog - главный тег, который просто указывает что внутри него будут описания изменений
-   changeSet - блок с самими изменениями
    -   author
        -   указывается имя автора изменений
    -   id
        -   id изменения
        -   я не уверен, но должен быть уникальный для всех chabgelogs
        -   например, можно использовать структуру вида "имя_файла" + "." + "номер changeset в текущем файле"

```xml
<?xml version="1.0" encoding="UTF-8"?>
<databaseChangeLog xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    logicalFilePath="path-independent"
    xmlns="http://www.liquibase.org/xml/ns/dbchangelog" xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog
            http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-3.1.xsd">

    <changeSet author="author_name" id="4">
        <addColumn schemaName="org" tableName="profile_personal_data">
            <column name="address" type="VARCHAR(100)"/>
        </addColumn>
    </changeSet>

    <changeSet author="author_name" id="5">
        <addColumn schemaName="org" tableName="profile_personal_data">
            <column name="google_calendar_id" type="VARCHAR(100)"/>
        </addColumn>
    </changeSet>
</databaseChangeLog>
```

## Рекомендованная структура хранения изменений

[Рекомендованная файловая](https://www.liquibase.org/get-started/best-practices) структура хранения изменений:

```bash
.
└── src
    └── main
        └── resources
            └── db
                ├── changelog-root
                |   ├── db.changelog-root.xml
                |   ├── db.changelog-1.0.xml
                |   ├── db.changelog-2.0.xml
                |   └── db.changelog-2.1.xml
                ├── changelog-1.0
                |   └── ...
                ├── changelog-2.0
                |   └── ...
                ├── DatabasePool.java
                └── AbstractDAO.java
```

Описание структуры БД в liquidbase:
