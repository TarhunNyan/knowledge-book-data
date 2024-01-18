# Gradle

Сборщик проектов для Java, работает на языке Groovy

# Директории Gradle

У Gradle есть две основные директории:

-   home
    -   хранит глобальные настройки для gradle
    -   хранит cache для gradle
    -   daemon, wrapper и т.д.
-   project
    -   settings.gradle - файл в котором прописываем где ищем build.gradle
    -   build.gradle - файды в подпроектах, где описана конфигурация task
    -   .gradle - версии gradle и всякие вспомогательные для него файлы
    -   build - папка в которую gradle собирает все artifact
    -   wrapper/gradlew
        -   волшебный скрипт, позполяющий не париться о версиях gradle
        -   к нему много что относиться: gradlew, gradlew.bat, gradle/wrapper и т.д.

# Lifecycle

Жизненный цикл:

-   Инициализация(settings.gradle)
    -   обработка settings.gradle
    -   создание Project сущностей
    -   создание Settings сущности
-   Конфигурация(build.gradle)
    -   регистрация Task
    -   конфигурация Task
        -   то что прописано в Closure, зарегестрированом в Task, и есть конфигурация
    -   построение дерева вызовов Task
-   Выполнение
    -   запуск всех Task в заданном порядке(параллельно)

# settings.gradle

settings.gradle - файл которй Gradle находит в первую очередь:

-   входная точка для gradle
-   указывает где лежат все подроекты
-   в нем прописано где лежат все необходимые build.gradle

```gradle
rootProject.name = 'jetalon'

include ':jetafiles'
```

Можно посмотреть структуру и подпроекты проекта gradle:

```bash
.\gradlew projects
```

# build.gradle

Описание подпроекта:

-   отображается например, при просмотре структуры через .\gradlew projects

```build.gradle
description = 'это описание подпроекта!'
```

# Plugin

Plugin - сторонний набор Task, которые можно подключить в проект и использовать:

```gradle
plugins {
    id 'nu.studer.jooq' version "${jooqPluginVersion}"
    id 'project-report'
}
```

# Task

Task - основная сущность в Gradle, из которой состоит наверное все:

-   создание Task
    -   сами создаем Task
    -   получаем Task из подключенных plugin

```gradle
task hello {
    // configuration

    doFirst {
		// делаем сначала
	}

	doLast {
		// делаем в конце
	}
}
```

Посмотреть все Tasks

```bash
.\gradlew tasks --all
```

# Выполнить Task

Для выполнения Task нужно в консоли:

-   task_name - имя Task, который выполняем
-   выполнит все Task с этим именем
    -   будет спускаться вниз по иерархии settings.gradle

```bash
.\gradlew task_name
```

Можно указать подпроект в котором хотим запускать Task:

-   packageName - имя подпроекта
-   packageName2 - имя подпроекта второго уровня
-   task_name - имя Task, который выполняем

```bash
.\gradlew packageName:packageName2:task_name
```

# Стандартные Tasks

build:

-   подтягивает и собирает нужные пакеты/подпроекты
-   запускает тесты только для тех подпроектов, в которых произошли изменения

buildNeeded:

-   подтягивает и собирает все пакеты/подпроекты
-   запускает тесты для всех подпроектов

buildDependents:

-   подтягивает и собирает все зависимые пакеты/подпроекты от указанного
-   запускает тесты для всех зависимых от указанного подпроектов
-   зависимость тестов указывается в testRuntime configuration
