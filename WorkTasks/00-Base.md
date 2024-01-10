# Всякие базовые штуки по работе

## Контроль над колонкой

Контроль над колонкой:

-   http://192.168.5.214/juke

## Получаем себе backup-чик Cache

Установка бэкапа БД:

-   Останавливаем кубик
-   Переименовываем старую базу, новую базу называем именем старой
-   Стартуем кубик
-   Радуемся новой базе

Где хранятся бэкапы:

-   \\192.168.5.4\backups

## Получаем себе backup-чик Jetalon

Устанавливаем на локалку:

-   \\192.168.5.30\JET-01_backups - тут храняться backup
-   делаем что-то в духе того, что написано в батнике ниже

```bat
cd %1

set PGPASSWORD=12345678
type jetauth.sql | "C:\Program Files\PostgreSQL\14\bin\psql.exe" --username=postgres --no-password
type jetafiles.sql | "C:\Program Files\PostgreSQL\14\bin\psql.exe" --username=postgres --no-password
type jetalabor.sql | "C:\Program Files\PostgreSQL\14\bin\psql.exe" --username=postgres --no-password
type jetaudit.sql | "C:\Program Files\PostgreSQL\14\bin\psql.exe" --username=postgres --no-password
```

Или используй новый dblab:

-   http://192.168.5.59:16100/instance
-   dblab-token-sis - пароль на вход

## Как доабвить полностью локализированную версию

Нужно локализировать сервер:

-   это для Cache
-   нужно прописать в консоле Cache

```cache
zn "SOU"
d ##class(Helpers.Utils.Locale).ReloadCache()
```

Нужно собрать локализированный фронт:

-   не забудь в пути браузера, поменять ru на en

```bash
cd ./webClient
npm run dev:loc
```

Генерируем локализированный словарь:

```bash
gradle packingEnLocalConfig
```

Импортируем сгенерированный словарь:

-   в запущенном Эталоне заходим в конфигурирование
-   импортируем сгенерированный словарь
    -   etalon/.configs/en_locale_config.zip - сгенерированная конфигурация

## Как собирать проект на Cashe

Чтобы собрать проект на Cache:

-   gradle fastUpdate/FullUpdate - очищает кэш Cache и перекомпилирует проект. Ибо в Cache постоянно что-то отваливается
-   generateSwagger - генерируем swagger, в котором описано API приложения
    -   потом по swagger файлу, проверяется api клиента
    -   потом по swagger файлу, проверяется back для jetalon
-   Запускаем таски gradle, чтобы понаустанавливать все node_module
    -   npmInstall
    -   yarn
    -   yarnSetup
-   npm run dev:loc - запускаем в webClient. Для генерации клиента

Адрес:

-   http://localhost:80/csp/sou/web/ru/

## Как собирать проект на Java

Чтобы собрать проект на Java:

-   gradle grabWebClient - подхватываем сбилженный клиент, который был создан на Cache
-   gradle generateSwaggerCode - получаем swagger, без этого api может не провериться
-   gradle generateJooq
-   gradle workwork
-   Для запуска через service из spring boot
    -   для запуска через service
        -   нужно указать удаленную БД (http://192.168.5.12:16100/status/)
            -   databaseUrl = getEnv("POSTGRES_URL","jdbc:postgresql://192.168.5.12:16118/")
        -   нужно указать логин(GS_01)
            -   databaseUser = getEnv("POSTGRES_USER",'GS_01')
        -   нужно указать пароль(GS_01)
            -   databasePassword = getEnv("POSTGRES_USER",'GS_01')
    -   где настрйоки
        -   заходим в ./build.gradle
        -   ищем что-то вроде "databaseUser = getEnv("POSTGRES_USER",'postgres')"

Адрес:

-   http://localhost:4221/csp/sou/web/ru/

Пример gradle:

```gradle
// старый
databaseUrl=getEnv("POSTGRES_URL","jdbc:postgresql://192.168.5.12:16118/")

databaseUser = getEnv("POSTGRES_USER",'GS_01')
databasePassword = getEnv("POSTGRES_USER",'GS_01')

// новый
databaseUrl=getEnv("POSTGRES_URL","jdbc:postgresql://192.168.5.59:16104/")

databaseUser = getEnv("POSTGRES_USER",'GS_01')
databasePassword = getEnv("POSTGRES_USER",'!@#QWEASDZXC456')
```

## Как собирать jetalon-libs локально

Чтобы собрать проект на jetalon-libs локально:

-   В jetalon-libs дергаем publishToMavenLocal
    -   находится в gradle -> Tasks -> publishing -> publishToMavenLocal
-   Идем в Jetalon, build.gradle
-   раскоментируем jetalibsVersion = libs.getDepVersion(libs.jet.jetalibsLatestVersion, true)
-   комментируем jetalibsVersion = '1.30.5-RELEASE'
-   обновляем gradle

## Как собирать jetalon-libs

[Тута сборка](http://servergit:8080/job/libs.java.jetalon/)

## Как запустить админку

Как запустить админку:

-   server/src/main/resources/application-dev.properties

```
spring.datasource.url=jdbc:postgresql://192.168.5.59:16104/jetauth
spring.datasource.username=GS_01
spring.datasource.password=!@#QWEASDZXC456
```

Чтобы хоть что-то увидать, нужно собрать клиент:

-   gradle buildAngular

Адрес:

-   http://localhost:4222/web/home

## У всех собралось, у меня нет (

У всех собралось, у меня нет:

-   подергай обновление gradle
-   maven запустил?
    -   cd C:\tools\apache-activemq-5.18.3\bin
    -   activemq start
-   nginx запустил?

Все вышеописанное сделал? Тогда:

-   почисти cache у maven
    -   user/.m2
-   почисти cache у gradle
    -   user/.gradle

# Конфиги - Cache

## Ищем "Тип ОК" по "ОК"

Чтобы на Cache, найти "Тип ОК" по "ОК", мы делаем:

-   Кубик -> Портал управления системой -> Обозреватель системы -> SQL -> Запустить
-   Меняем область с SYS на SOU
-   Разварачиваем вкладку - Таблицы
-   Ищем: ObjectOfControlShort
-   Достаем SQL, и дописываем в конце
    -   ... WHERE Code="Тут код ОК"
-   Довольный смотришь ObjectType

# Подмеченные приколы

-   При получение данных из таблицы, всегда их сортируй. Это приведет к меньшему числу оишбок в будующем
    -   если данных много, то тут конечно без сортировки, но обычно их не так много )
-   У тебя должен быть свой NULL
    -   программный null - указывает, что тут какой-то еррор, и данные тупо не обработались
    -   самопальный null - указывает, что тут нет данных, но все отработало
