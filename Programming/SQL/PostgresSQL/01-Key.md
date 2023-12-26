# Foreign key/Primary key

Foreign Key - внещний ключ:

-   когда поле в нашей таблице ссылается на строку в другой таблице
-   по сути прописываем какой-то индекс из дргуой таблицы
-   современные БД настолько круты, что помнят кто на кого ссылается и сами могут найти значение внешнего ключа

Primary key - внутриенний ключ:

-   колонка/набор-колонок, значения в которой уникальные и не NULL
-   указываются в таблице один раз

# FOREIGN KEY - Внешние ключи

Базовые примеры использования внешнего ключа:

-   [Задаем ForeignKey для колонки](#foreign-key---пример-задания)
-   [Вставляем значение в таблицу(с ForeignKey)](#foreign-key---пример-вставки)
-   [Удаление значений из таблицы(с ForeignKey)](#foreign-key---delete-связанных-строк)
-   [Изменение значений из таблицы(с ForeignKey)](#foreign-key---update-связанных-строк)

Продвинутые примеры:

-   [Задание ForeignKey по группе колонок](#foreign-key---пример-задания-по-нескольким-столбцам)
-   [Ссылаться ForeignKey на эту же таблицу(создаем дерево)](#foreign-key---ссылка-на-самого-себя)

# Примеры

## FOREIGN KEY - Пример задания

Задаем внешний ключ:

-   references - указываем внешний ключ

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

## FOREIGN KEY - Пример вставки

Вставляем внешний ключ:

-   PostgreSQL - сам проверит наличие ключа в таблице cities
-   в примере
    -   wether.city ссылается на таблицу cities.name
    -   пример таблицы [отсюда](#foreign-key---пример-задания)

```dml
INSERT INTO weather VALUES ('Berkeley', 45, 53, 0.0, '1994-11-28');
```

## FOREIGN KEY - Пример задания по нескольким столбцам

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

Удаление строк, связаннх через ForeignKey:

-   можно задать поведение при удалении строки из таблицы
    -   DELETE CASCADE - при удалении строки, связанная так же удалится
    -   не CASCADE - запрет на удаление связанной строки
        -   DELETE NO ACTION - откладывает проверку до конца транзакции
        -   DELETE RESTRICT - проверяем сразу
-   подобное поведение можно задать и для UPDATE

```ddl
CREATE TABLE order_items (
    product_no1 integer REFERENCES products1 ON DELETE CASCADE,
    product_no2 integer REFERENCES products2 ON DELETE RESTRICT,
    product_no3 integer REFERENCES products3 ON DELETE NO ACTION,
    ...
)
```

## FOREIGN KEY - UPDATE связанных строк

Обновление строк, связанных через ForeignKey:

-   можно задать поведение при обновлении строки таблицы
    -   UPDATE CASCADE - изменённые значения связанных столбцов будут скопированы в зависимые строки
    -   не CASCADE - запрет на изменение связанной строки
        -   UPDATE NO ACTION - откладывает проверку до конца транзакции
        -   UPDATE RESTRICT - проверяем сразу
-   подобное поведение можно задать и для DELETE

```ddl
CREATE TABLE order_items (
    product_no1 integer REFERENCES products1 ON UPDATE CASCADE,
    product_no2 integer REFERENCES products2 ON UPDATE RESTRICT,
    product_no3 integer REFERENCES products3 ON UPDATE NO ACTION,
    ...
)
```
