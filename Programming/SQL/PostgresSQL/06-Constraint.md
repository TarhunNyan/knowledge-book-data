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
