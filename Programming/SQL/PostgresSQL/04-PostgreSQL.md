# PostgreSQL

PostgreSQL - приложение берущее на себя хранение и структурирование данных:

-   является OpenSource
-   хранит данные в виде табличек с прописанным отношением между таблицами(оттого и реляционная БД)
-   Как-то записывает данные в файлыЮ а потом их считывает, по сути черный ящик
-   Работает по принципу клиент-сервер
    -   по сути, отдельное приложение
    -   есть api, по которому можно обратиться
    -   сидеть прослушивает какой-то там порт

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
