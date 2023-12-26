# PostgreSQL

PostgreSQL - приложение берущее на себя хранение и структурирование данных:

-   является OpenSource
-   хранит данные в виде табличек с прописанным отношением между таблицами(оттого и реляционная БД)
-   Как-то записывает данные в файлыЮ а потом их считывает, по сути черный ящик
-   Работает по принципу клиент-сервер
    -   по сути, отдельное приложение
    -   есть api, по которому можно обратиться
    -   сидеть прослушивает какой-то там порт

# Install

## Install - Windows

Просто идем на сайт Postgres и качаем установщик:

-   устанавливаем Postgres
-   закидываем в EnvironmentVariable путь до папки bin в Postgres
    -   C:\Program Files\PostgreSQL\16\bin - путь по умолчанию

## Установка на Linux

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

## Стартуем на Linux

Стартуем на Linux - чтобы начать работать необходимо запустить postgres, чтобы он начал слушать порты:

-   В примере, получаем список команд, для запуска Postgre

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

# Commands

## Commands - Windows

Какого-то лешего, команды PostgresSQL не работают под стандартным пользователем, поэтому должны иметь дополнительные флаги:

-   createdb - команда
-   -U postgres -h localhost - дополнительные флаги без которых не работает
-   dbname - значение переданное команде

```bash
createdb -U postgres -h localhost dbname
```

## Commands - Консоль postgres

Консоль Postgres запускается командой:

```bash
# для Linux
psql
# для Windows
psql -U postgres -h localhost
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

# PSQL Console

PSQL Console - консоль Postgres, позволяет работать с БД:

-   к счастью, одинаково работает на windows и linux

## PSQL Console - Запуск

PSQL Console - запуск:

```bash
# для Linux
psql
# для Windows
psql -U postgres -h localhost
```

## PSQL Console - Помощь

Помощь:

```bash
\h
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

# DDL - Определени структуры БД

DDL - язык описания структуры таблиц в БД

## DDL - Data Type

Наиболее часто используемые типы данных:

-   integer - целые числа
-   numeric - для чисел, которые могут быть дробными. Обычно это денежные суммы
-   text - для текстовых строк
-   date - дата, без времени
-   time - время, без даты
-   timestamp - дата и время

## DDL - CreateTable

CreateTable - создает таблицу:

```ddl
CREATE TABLE my_first_table (
    first_column text,
    second_column integer
);
```

Значение по умолчанию:

-   если пользователь не задал значение по умолчанию, то postgres ставит NULL

```ddl
CREATE TABLE products (
    product_no integer,
    name text,
    price numeric DEFAULT 9.99
);
```

Значение по умолчанию - выражение:

-   выражение будет вычисляться при добавление строки

```ddl
CREATE TABLE products (
    create_time timestamp DEFAULT CURRENT_TIMESTAMP
);
```

Выбор значения из последовательности:

-   nextval('products_product_no_seq') - следующее значение из последовательности

```ddl
CREATE TABLE products (
    product_no integer DEFAULT nextval('products_product_no_seq'),
    ...
);

-- короткая запись
CREATE TABLE products (
    product_no SERIAL,
    ...
);
```

## DDL- DropTable

DropTable - удаляем таблицу:

```ddl
DROP TABLE products;
```

# DML - Манипуулирование данными БД

DML - язык манипулирования данными:

## DML - SELECT

Select - оператор выбора данных:

-   [статья по Select](./02-SQL.md)

## DML - INSERT

Insert - втсавляет данные в указанную таблицу:

```dml
INSERT INTO weather VALUES ('San Francisco', 46, 50, 0.25, '1994-11-27');
```

Insert - втсавляет данные в указанную таблицу, по указанным столбцам:

-   (city, temp_lo, temp_hi, prcp, date) - стлбцы и порядок, в котором вставляются данные

```dml
INSERT
    INTO weather (city, temp_lo, temp_hi, prcp, date)
    VALUES ('San Francisco', 43, 57, 0.0, '1994-11-29');
```

## DML - UPDATE

Update - обновляет значения в таблице:

-   в примере, уменьшаем температуру на 2 градуса для дней после 1994-11-28

```dml
UPDATE weather
    SET temp_hi = temp_hi - 2,  temp_lo = temp_lo - 2
    WHERE date > '1994-11-28';
```

## DML - DELETE

Delete - удаление строк из таблицы:

```dml
DELETE FROM weather WHERE city = 'Hayward';
```

## DML - MERGE

## DML - VIEW

View - представления, фактически мы присваиваем имя запросу, чтобы его можно было использовать в других запросах:

-   активное использование представлений — это ключевой аспект хорошего проектирования баз данных SQL

```dml
CREATE VIEW myview AS
    SELECT name, temp_lo, temp_hi, prcp, date, location
        FROM weather, cities
        WHERE city = name;

SELECT * FROM myview;
```

## DML - Commit

Commit - транзакция:

-   BEGIN; - начало транзакции
-   COMMIT; - конец транзакции

```dml
BEGIN;
UPDATE accounts SET balance = balance - 100.00
    WHERE name = 'Alice';
-- ...
COMMIT;
```

Rollback(откат транзакции) и Savepoint(точка сохранения транзакции):

-   SAVEPOINT my_savepoint;
    -   создаем savepoint
-   ROLLBACK TO my_savepoint;
    -   откатываемся к указаному savepoint

```dml
BEGIN;
UPDATE accounts SET balance = balance - 100.00
    WHERE name = 'Alice';
SAVEPOINT my_savepoint;
UPDATE accounts SET balance = balance + 100.00
    WHERE name = 'Bob';
-- ошибочное действие... забыть его и использовать счёт Уолли
ROLLBACK TO my_savepoint;
UPDATE accounts SET balance = balance + 100.00
    WHERE name = 'Wally';
COMMIT;
```

# Типы данных

Типы данных в PostgresSQL

## Типы данных

| Название          | Описание                            |
| :---------------- | :---------------------------------- |
| bigserial         | Это Long с автоинкрементом          |
| character vatying | Это String, с ограничением на длину |

# FOREIGN KEY - Внешние ключи

Внешние ключи:

```ddl
CREATE TABLE cities (
        name     varchar(80) primary key,
        location point
);

CREATE TABLE weather (
        city      varchar(80) references cities(name),
        temp_lo   int,
        temp_hi   int,
        prcp      real,
        date      date
);
```

Теперь мы можем вставить в таблицу weather что-то вроде:

-   PostgreSQL - сам проверит наличие ключа в таблице cities

```dml
INSERT INTO weather VALUES ('Berkeley', 45, 53, 0.0, '1994-11-28');
```

## FOREIGN KEY - По нескольким столбцам

Можно связать несколько столбцов через внешний ключ:

```ddl
CREATE TABLE t1 (
  a integer PRIMARY KEY,
  b integer,
  c integer,
  FOREIGN KEY (b, c) REFERENCES other_table (c1, c2)
);
```

## FOREIGN KEY - Ссылка на самого себя

Возможно ссылаться по внешнему ключу на эту же таблицу:

-   таким образом можно сформировать древовидную структуру
-   элементы которые являются корневыми, будут ссылаться на NULL

```ddl
CREATE TABLE tree (
    node_id integer PRIMARY KEY,
    parent_id integer REFERENCES tree,
    name text,
    ...
);
```

## FOREIGN KEY - DELETE связанных строк

При удалении строки, связанная так же удалится:

```ddl
CREATE TABLE order_items (
    product_no integer REFERENCES products ON DELETE CASCADE,
    ...
)
```

Запрет удаления связанной строки:

-   RESTRICT - проверяет сразу

```ddl
CREATE TABLE order_items (
    product_no integer REFERENCES products ON DELETE RESTRICT,
    ...
)
```

Запрет удаления связанной строки:

-   NO ACTION - откладывает проверку до конца транзакции

```ddl
CREATE TABLE order_items (
    product_no integer REFERENCES products ON DELETE NO ACTION,
    ...
)
```

## FOREIGN KEY - UPDATE связанных строк

# Оконная функция

Оконная функция - похожа на агрегатную, но строки не групируются в одну

-   OVER - говорит что это оконная функция
-   (PARTITION BY depname) - указывает по какому принципу мы делим строки на группы
-   оконная функция обрабатывает строки уже полсе прохождения WHERE, HAVING и ORDER BY
-   оконные функции можно использовать только внутри SELECT и ORDER BY

```sql
SELECT depname, empno, salary, avg(salary) OVER (PARTITION BY depname)
    FROM empsalary;

-- =>
--   depname  | empno | salary |          avg
-- -----------+-------+--------+-----------------------
--  develop   |    11 |   5200 | 5020.0000000000000000
--  develop   |     7 |   4200 | 5020.0000000000000000
--  develop   |     9 |   4500 | 5020.0000000000000000
--  develop   |     8 |   6000 | 5020.0000000000000000
--  develop   |    10 |   5200 | 5020.0000000000000000
--  personnel |     5 |   3500 | 3700.0000000000000000
--  personnel |     2 |   3900 | 3700.0000000000000000
--  sales     |     3 |   4800 | 4866.6666666666666667
--  sales     |     1 |   5000 | 4866.6666666666666667
--  sales     |     4 |   4800 | 4866.6666666666666667
```

Window - выделение оконной функции в подфункцию:

```sql
SELECT sum(salary) OVER w, avg(salary) OVER w
    FROM empsalary
    WINDOW w AS (PARTITION BY depname ORDER BY salary DESC);
```

# Наследование таблиц

Наследование таблиц - позволяет не только просматривать как одну таблицу, но и редактировать как одну таблицу:

-   INHERITS (cities); - указываем что capitals нследуется от cities
-   capitals - получает все столбцы cities и добавляет столбец state

```ddl
CREATE TABLE cities (
  name       text,
  population real,
  altitude   int     -- (высота в футах)
);

CREATE TABLE capitals (
  state      char(2)
) INHERITS (cities);
```

Теперь запросы такого вида, будут включать в себя данные еще и из наследников:

```sql
SELECT name, altitude
    FROM cities
    WHERE altitude > 500;
```

Чтобы сделать запрос игнорируя наследников, можно использовать ONLY:

-   в примере, работаем с таблицей cities, игнорируя наследников
-   SELECT, UPDATE и DELETE - поддерживают ONLY

```sql
SELECT name, altitude
    FROM ONLY cities
    WHERE altitude > 500;
```

PostgresSQL поддерживает множественное наследование

# Функции

Функции - попадают в папку routines

# Тригерные процедуры

Тригерные процедуры - когда срабатывает определенное событие(например UPDATE), вызвается тригерная процедура:

-   попадают в папку trigger
-   перекладывать логику в БД - очень плохо
-   используй когда:
    -   есть легаси, который переписывать себе дороже
        -   дублируем события таблиц для легаси таблиц в новые таблицы
    -   у нас есть часто используемое выражение, которые мы постоянно перещитываем, но можно выделить в отдельный столбец
        -   например, есть столбцы имя и фамилия
        -   через триггеры можно добавить столбец ФИО, где объединить имя и фамилию

Пример:

-   CREATE OR REPLACE FUNCTION - создаем функцию
    -   LANGUAGE plpgsql - язык plpgsql
    -   VALUE cost 100 - даем ей оценку сложности
-   CREATE TRIGGER - регестрируем триггер вызывающий метод
    -   BEFORE UPDATE ON users - когда изменяем таблицу users
    -   BEFORE UPDATE first_name, last_name ON users - когда изменяем first_name и last_name таблицы users
    -   BEFORE INSERT ON users - когда вставляем строку в таблицу users

```dml
CREATE OR REPLACE FUNCTION public.update_user_name() RETURNS trigger AS
$BODY$
    BEGIN
        NEW.name = CONCAT(NEW.first_name, ' ', NEW.last_name);
        RETURN NEW;
    END
$BODY$
LANGUAGE plpgsql VALUE cost 100;

CREATE TRIGGER "update_user_name_on_update_trigger"
    BEFORE UPDATE ON users
        FOR EACH ROw EXECUTE PROCEDURE "update_user_name"();
```

# Комментарий

Комментарий

```ddl
-- это комментарий
/*
    Это многострочный
    комментарий
*/
```

# Генерируемые столбцы

## Сохраненный генерируемый столбец

Сохраненный генерируемый столбец:

-   Хранится в памяти таблицы
-   Вычисляется из других
-   Можно задавать расчет при добавлении или изменении
-   Нельзя задать значение по умолчанию
-   Триггеры и другие генерируемые столбцы не могут обращаться к генерируемым столбцам

Пример:

-

```ddl
CREATE TABLE people (
    ...,
    height_cm numeric,
    height_in numeric GENERATED ALWAYS AS (height_cm / 2.54) STORED
);
```

## Виртуальный генерируемый столбец

Сохраненный генерируемый столбец:

-   Остались только в старых версиях Potgres )
-   Вычисляется в реалтайме при запросе
-   Вычисляется из других
-   Можно задавать расчет при добавлении или изменении

# Ограничения

Ограничения - можно задать проверку значениям:

-   работает только с добалвенной или измененной строкой
-   CHECK (price > 0) - ограничение, что значение должно быть больше 0
-   CONSTRAINT positive_price - задаем имя ограничению
    -   улучшает сообщения об ошибках
    -   можно ссылаться на это ограничение, если необходимо его изменить
-   CHECK (discounted_price > 0 AND price > discounted_price) - ограничение таблицы
    -   тут может быть сложная проверка, для таблицы

```ddl
CREATE TABLE products (
    product_no integer,
    name text,
    price numeric CHECK (price > 0),
    price numeric CONSTRAINT positive_price CHECK (price > 0),

    discounted_price numeric,
    CONSTRAINT valid_discount CHECK (discounted_price > 0 AND price > discounted_price)
);
```

## Ограничение - NOT NULL

NOT NULL - не позволяет записывать с толбец NULL:

-   NOT NULL можно использовать вместе с CHECK

```ddl
CREATE TABLE products (
    product_no integer NOT NULL,
    name text NOT NULL,
    price numeric NOT NULL CHECK (price > 0)
);
```

## Ограничение - UNIQUE

UNIQUE - гарантирует, что колонка будет заполнена уникальными значениями, для всех строк таблицы:

-   product_no integer UNIQUE - ограничение стобца
    -   может содержать повторы для NULL
-   UNIQUE (product_no) - ограничение таблицы
    -   может содержать повторы для NULL
-   customer_no integer UNIQUE NULLS NOT DISTINCT
    -   запрещает повторы для NULL

```ddl
CREATE TABLE products (
    product_no integer UNIQUE,
    customer_no integer UNIQUE NULLS NOT DISTINCT,
    ...

    UNIQUE (product_no)
);
```

## Ограничение - UNIQUE-сочетания

UNIQUE-сочетания - уникальные сочетания значений столбцов:

-   UNIQUE (a, c) - указываем что сочетание столбцов должно быть уникальным
    -   a и c - столбцы могут быть не уникальны
    -   a и c - комбинация должна быть уникаотной
    -   если оба столбца содержат NULL, то такое ограничение не работает

```ddl
CREATE TABLE example (
    a integer,
    b integer,
    c integer,
    UNIQUE (a, c)
);
```

## Ограничение - Первичные ключи

Первичные ключи:

-   должны быть в каждой таблице
    -   в postgres не обязательно для каждой таблицы
-   можно обозначить только один раз в табллице
-   по сути это
    -   UNIQUE
    -   NOT NULL
    -   С индексом B-дерева

```ddl
CREATE TABLE products (
    product_no integer PRIMARY KEY,
    name text,
    price numeric
);

CREATE TABLE example (
    a integer,
    b integer,
    c integer,
    PRIMARY KEY (a, c)
);
```
