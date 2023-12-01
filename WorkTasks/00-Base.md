# Всякие базовые штуки по работе

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
-   npm run dev:loc - запускаем в webClient. Для генерации клиента

## Как собирать проект на Java

Чтобы собрать проект на Java:

-   gradle grabWebClient - подхватываем сбилженный клиент, который был создан на Cache
-   gradle generateSwaggerCode - получаем swagger, без этого api может не провериться
-   Для запуска через service из spring boot
    -   для запуска через serviceь
        -   нужно указать удаленную БД (http://192.168.5.12:16100/status/)
            -   databaseUrl = getEnv("POSTGRES_URL","jdbc:postgresql://192.168.5.12:16118/")
        -   нужно указать логин(GS_01)
            -   databaseUser = getEnv("POSTGRES_USER",'GS_01')
        -   нужно указать пароль(GS_01)
            -   databasePassword = getEnv("POSTGRES_USER",'GS_01')
    -   где настрйоки
        -   заходим в ./build.gradle
        -   ищем что-то вроде "databaseUser = getEnv("POSTGRES_USER",'postgres')"

## Как собирать jetalon-libs локально

Чтобы собрать проект на jetalon-libs локально:

-   В jetalon-libs дергаем publishToMavenLocal
-   Идем в Jetalon, build.gradle
-   раскоментируем jetalibsVersion = libs.getDepVersion(libs.jet.jetalibsLatestVersion, true)
-   комментируем jetalibsVersion = '1.30.5-RELEASE'
