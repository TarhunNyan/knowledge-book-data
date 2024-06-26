# Всякие базовые приколы по работе

## Контроль над колонкой

Контроль над колонкой:

-   http://192.168.5.214/juke

# Cache

## Получаем себе backup-чик Cache

Установка бэкапа БД:

-   Останавливаем кубик
-   Переименовываем старую базу, новую базу называем именем старой
-   Стартуем кубик
-   Радуемся новой базе

Где хранятся бэкапы:

-   \\192.168.5.4\backups

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

## А где стандартные настройки Cache?

Тут начальные настройки для Cache, типо ссылки на Matomo:

C:\InterSystems\Cache\mgr\SOU\Settings\SystemSettings.settings

## Делаем реоргу на Cache

Тут инфа как делать реоргу: https://docs.google.com/document/d/1zOLGEUuYYzz8MM8n_tfqjOTnhmbsZxDfz2PizFTdEDM/edit

Кратенько:

-   [тут](https://docs.google.com/spreadsheets/d/1QU6av3B4Dvtvo9U5EcsM0Oen_BSOmx7AKtrK3-uNYBg/edit#gid=219341529) на "Лист1" берем номер реорганизации
-   создаем два запроса
    -   запрос в котором будет реорганизация
        -   например: ETA-67997 Реализация. Исправить опечатку в Системном справочнике ContractType (R390)
        -   незабудь добавить в ReorganizationList: d ##class(SOU.Update.ReorganizationList)
    -   запрос где включаем реорганизацию в шаги реорганизации
        -   например: Включение реорганизации R390 в шаги обновления
-   незабудь отписать тестировщикам, когда будешь заливать "включение в шащи обновления", им надо знать какие логи смотреть!

## Console помощи на Cache

Console помощи на Cache:

```cache
do ##class(SOU.Support.SupportConsole).show()
```

## Открыть "Портал управления системой" на любой площадке

```cache
https://t01.sis-it.pro/csp/sys/UtilHome.csp
```

# Jetalon

## Новый реп с Jetalon

Новый реп с Jetalon:

-   деалем clone с gitlab

Подключаем jet.env:

-   создаем file с названием jet.env:

```env
POSTGRES_URL=jdbc:postgresql://192.168.5.59:16104/
POSTGRES_USER=GS_01
POSTGRES_PASSWORD=!@#QWEASDZXC456
```

В Idea идем в настройки шаблона для SpringBoot и протыкиваем:

-   добавляем jet.env
    -   Enable EnvFile -> true
    -   Add(плюсик)
    -   добавляем путь до jet.env
    -   ставим галочку около jet.env в появившемся списке
-   настраиваем Short Command
    -   Modify options -> Shorten Command Line
    -   Shorten Command Line: -> @argfile (Java 9+)

```bash
Edit configurations | Edit configuration templates... |   ...          |
                                                      |   Shell Script |
                                                      | > SpringBoot   |
                                                      |   ...          |
```

Добавляем Service:

```bash
/* Top Menu */ -> View | > Tool Windows     |   ...       | /* В открывшемся окне*/ -> Add Service | > Run Configuration Type                |   ...    |
                       |   Appearenc        |   Structure |                                        |   Docker Connection                     | > Spring |
                       |   Quick Definition | > Services  |                                        |   Docker Connection From Docker Context |   ...    |
                       |   ...              |   ...       |                                        |   Docke Registry                        |   ...    |
```

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

## Как собирать проект на Java

Чтобы собрать проект на Java:

-   gradle grabWebClient - подхватываем сбилженный клиент, который был создан на Cache
-   gradle generateSwaggerCode - получаем swagger, без этого api может не провериться
-   gradle generateJooq
-   gradle workwork
-   идем запускать админку, без нее ничего на интерфейсе не поделать
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
databaseUrl=getEnv("POSTGRES_URL","jdbc:postgresql://192.168.5.12:16104/")

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

## Ищем "Тип ОК" по "ОК"

Чтобы вытащить "Тип ОК" по "ОК", просто используй SQL ниже:

-   'АГ ОК с ВлС' - замени на имя твоего ОК

```sql
WITH
    names (ok_name, ok_code, ok_type_name, ok_type_code, meta_entity_name, meta_entity_uuid) AS (values (
        -- Название ОК
        'АГ ОК с ВлС',
        -- Код ОК
        '%',
        -- Название типа ОК
        '%',
        -- code типа ОК
        '%',
        -- название meta-сущности
        '%',
        -- UUID для meta-сущности
        '%'
    )),
    f_control_object AS (SELECT id, control_object_type_id, short_name, code FROM control.control_object, names WHERE short_name LIKE names.ok_name AND code LIKE names.ok_code),
    f_control_object_type AS (SELECT id, short_name, code FROM control.control_object_type, names WHERE short_name LIKE names.ok_type_name AND code LIKE names.ok_type_code),
    f_meta_entity AS (SELECT id, name, uuid FROM eav.meta_entity, names WHERE name LIKE names.meta_entity_name AND CAST(uuid AS varchar) LIKE names.meta_entity_uuid)
SELECT
    ok.id AS ok_id,
    ok.short_name AS ok_name,
    ok.code AS ok_code,
    ok_type.id AS type_id,
    ok_type.short_name AS type_short_name,
    ok_type.code AS type_code,
    meta_entity.id AS meta_entity_id,
    meta_entity.name AS meta_entity_name,
    meta_entity.uuid AS meta_entity_UUID
FROM
    control.control_object_type_meta_entity AS link_table,
    f_control_object_type AS ok_type,
    f_meta_entity AS meta_entity,
    f_control_object AS ok
WHERE
    ok_type.id=ok.control_object_type_id AND
    meta_entity.id=link_table.meta_entity_id AND
    ok_type.id=link_table.type_id;
```

##

Какого-то хрена вложки лежат в eav.entity

И какого-то хрена поля журнала грузятся и сейвятся вот тут. Я хуй знает как я должен был это найти DynamicalObjectSpringDataService

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
