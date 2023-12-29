# PostgreSQL - Установка и работа через консоль

Установка:

-

Работа через консоль - хотя все работают через интерфейс PGAdmin, это не уважаемо

# Install

Install - установка на:

-   [Установка на windows](#install---windows)
-   [Установка на linux](#install---linux)

# Console

В PostgresSQL - своя рабочая консоль(открывается в Terminal/Powershell/Shell/...):

## Работа в Terminal

Windows:

-   [Настройка работы команд в windows-shell](#console---start-consolewindows)

Доступные команды:

-   [CreateDB - создаем БД](#commands---createdb)
-   [DropDB - удаляем БД](#commands---dropdb)

## Запуск PostgreSQL консоли

Запуск консоли для Windows:

-   Не забываем пробрасывать переменные окружения
-   [Запуск консоли для Windows](#console---start-consolewindows)
-   [Запуск консоли для Windows#2](#console---start-consolewindows)
-   [Чиним проблемы с русским языком](#console---locale-consolewindows)

Запуск консоли для Linux:

-   Не забываем пробрасывать переменные окружения
-   [Включаем сам PostgreSQL, чтобы слушала порты](#console---start-consolelinux---ports)
-   [Запуск консоли для Linux](#console---start-consolelinux)

## Работа с psql

Работа с psql:

-   [Правим psql для нормальной работы](#console---правим-psql-под-себя)

Работа с psql:

-   [\h - помощь по коммандам языка](#psql-console---помощь)
-   [\? - помощь по коммандам psql](#psql-console---помощь2)
-   [\l - список БД](#psql-console---список-бд)
-   [\q - выход из консоли](#psql-console---выход-из-консоли)
-   [\! - исполнить команду Windows/Linux](#psql-console---исполнить-команду)

Вводим SQL:

-   [Ввод SQL в косноли](#psql-console---ввод-sql)
-   [\e - редактирование последнего запроса в редакторе](#psql-console---редактирование-последнего-запроса)

# Примеры

## PSQL Console - Редактирование последнего запроса

Для редактирования последенго SQL в редакторе:

-   [Можно установить нужный редактор](#console---правим-psql-под-себя)

```bat
\e
```

## PSQL Console - Ввод SQL

Для ввода SQL в psql достачно просто ввести SQL:

```bat
SELECT current_date;
```

## PSQL Console - Исполнить команду

Исполнить команду - исполняет однострочную команду Windows/Linux, внутри консоли psql:

```bat
\! dir
\! cd d:
```

## PSQL Console - Помощь

Помощь по командам языка:

```bash
\h
```

## PSQL Console - Помощь#2

Помощь по командам языка:

```bash
\?
```

## PSQL Console - Список БД

Вывести список баз данных:

```bash
\l
```

## PSQL Console - Выход из консоли

Выход из консоли:

```bash
\q
```

## Commands - CreateDB

CreateDB - создаем БД:

```bash
# для Linux
createdb
# для Windows
createdb -U postgres -h localhost dbname
```

## Commands - DropDB

DropDB - удаляем БД:

-   прямо физический файл полностью удаляется
-   удаляются данные БД
-   удаляется структура БД

```bash
# для Linux
dropdb
# для Windows
dropdb -U postgres -h localhost dbname
```

## Commands - Windows-shell comand

Windows-shell comand - какого-то лешего, команды PostgresSQL не работают под стандартным пользователем, поэтому должны иметь дополнительные флаги:

-   createdb - команда
-   -U postgres -h localhost - дополнительные флаги без которых не работает
-   dbname - значение переданное команде

```bash
createdb -U postgres -h localhost dbname
```

## Console - Console cmd(Windows)

Чтобы работали консольные команды, нужно:

-   пробросить переменные окружения
-   вызвать команды с указанием пользователя

## Console - Start console(Windows)

Start console(Windows) - запускаем консоль на Windows:

-   psql - команда
-   -U postgres -h localhost - дополнительные флаги без которых не работает

```bash
psql -U postgres -h localhost
```

## Console - Правим psql под себя

Какие проблемы решает изменение bat:

-   Вместо руских букв выводятся кракозябры:
-   Использование нужного редактора текста

Чиним bat, который psql Shell:

-   C:\Program Files\PostgreSQL\16
    -   путь до bat файла

```bat
...
SET /P username="Username [%username%]: "


:: Добавляем
:: vvv
chcp 1251
SET CLIENT_ENCODING = 'UTF8';
set PSQL_EDITOR=C:\\Users\\g.stepanov\\AppData\\Local\\Programs\\Microsoft VS Code\\Code.exe
:: ^^^
:: Добавляем

for /f "delims=" %%a in ('chcp ^|find /c "932"') do @ SET CLIENTENCODING_JP=%%a
...
```

## Console - Locale console(Windows)

Вместо руских букв выводятся кракозябры:

-   это проблема с кодировкой

Решение внутри psql консоли:

```bat
\! chcp 1251
```

Чиним bat, который psql Shell:

-   C:\Program Files\PostgreSQL\16
    -   путь до bat файла

```bat
...
SET /P username="Username [%username%]: "


:: Добавляем
:: vvv
chcp 1251
:: ^^^
:: Добавляем

for /f "delims=" %%a in ('chcp ^|find /c "932"') do @ SET CLIENTENCODING_JP=%%a
...
```

## Console - Start console(Windows)#2

Start console(Windows) - запускаем консоль на Windows, через bat-ник:

-   просто заходим в пуск и вбиваем:
    -   psql (Shell)
-   C:\Program Files\PostgreSQL\16
    -   путь до bat файла

## Commands - Консоль postgres

Консоль Postgres запускается командой:

## Console - Start console(Linux)

Start console(Linux) - стартуем консоль в Linux:

```bash
psql
```

## Console - Start console(Linux - ports)

Start console(Linux) - чтобы начать работать необходимо запустить postgres, чтобы он начал слушать порты:

-   В примере, получаем список команд, для запуска Postgres

```bash
service postgresql
```

Список команд:

-   status - статус бд. Покажет включена бд или нет + история запусков
-   start - запустить бд
-   stop - остановить работу бд
-   restart - перезапустить бд
-   reload - хз
-   force-reload - хз

Запуск комманд:

-   например команда start
-   также вместо service можно указать путь до файла postgresql, его показывает service postgresql

```bash
service postgresql start
```

## Install - Windows

Просто идем на сайт Postgres и качаем установщик:

-   устанавливаем Postgres
-   закидываем в EnvironmentVariable путь до папки bin в Postgres
    -   C:\Program Files\PostgreSQL\16\bin - путь по умолчанию

## Install - Linux

Установка на Linux:

```bash
sudo apt update
sudo apt install postgresql postgresql-contrib
```

После установки создается супер пользователь postgres, меняем ему пароль:

```bash
sudo passwd userName
```

Теперь можно просто сменить пользователя на postgres и работать в нем, или использовать командный режим пользователя:

```bash
sudo -i -u postgres
```

Выйти из режима:

```bash
exit
logout
```
