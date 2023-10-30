# PostgreSQL

OpenSource БД

# Установка на Linux

```bash
sudo apt update
sudo apt install postgresql postgresql-contrib
```

После установки создается супер пользователь postgres, меняем ему пароль:

```bash
sudo passwd userName
```

Теперь можно просто сменить пользователя на postgres и работать в нем или использовать командный режим пользователя:

```bash
sudo -i -u postgres
```

Выйти из режима:

```bash
exit
logout
```

# Работа чз терминал

## Команды start/.../end бд

Получить список команд:

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

Запусе комманд:

-   например команда start
-   также вместо service можно указать путь до файла postgresql, его показывает service postgresql

```bash
service postgresql start
```

# Команды для PostgreSQL

Создаем БД в PostgreSQL:

-   createdb - команда создания БД
-   database_new - имя базы данных

```bash
createdb database_name
```

Удаляем БД в PostgreSQL:

-   dropdb - команда создания БД
-   database_new - имя базы данных

```bash
createdb database_name
```

# Консоль postgres

Запуск консоли PostgreSQL:

```bash
psql
```

Вывести список баз данных:

```bash
\l
```

Выход из консоли:

```bash
\q
```
