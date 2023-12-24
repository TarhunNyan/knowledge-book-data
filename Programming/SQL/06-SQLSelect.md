# SQL - SELECT

Select - основной запрос в реляционных БД, который вытаскивает данные

## Устройство SELECT-запроса

Устройство SELECT-запроса, описано в порядке выполнения запроса:

-   FROM - описываем таблицы с которыми работаем
-   WHERE - проверка для каждой строки
    -   определяет для каких строк выполнять агрегатную функцию
-   GROUP BY - создаем группы
-   HAVING - делаем проверки для элементов группы
-   SELECT - тут мы выбираем столбцы и даем им имена
-   ORDER BY - сортируем

## Структура SELECT-запроса

Структура SELECT-запроса:

```sql
SELECT[DISTINCT|ALL]
{ * | [<выражение для стобца> [[AS}] <псевдоним>]] [, ...] }
FROM <имя таблицы> [[AS] <псевдоним>] [, ...]
[WHERE <предикат>]
[[GROUP BY <список столбцов>]
[HAVING <условие на агрегатные состояния>]]
[ORDER BY <список столбцов>]
```

# FROM

From - примеры:

-   [Исполльзование точечной аннотации (таблица.столбец)](#from---точечная-аннотация)
-   [Подзапрос внутри FROM](#from---подзапрос)

# Пример

## FROM - Точечная аннотация

Точечная нотация. Если берем данные из нескольких таблиц, то у них могут пересекаться названия столбцов. Для однозначности используют точечную нотацию:

```sql
SELECT PC.price, Device.price, monitor FROM PC, Device;
```

## FROM - Подзапрос

Подзапрос внутри FROM:

```sql
SELECT DISTINCT PC.model, maker
FROM PC, (SELECT maker, model FROM Product) AS prod
WHERE PC.model = prod.model AND price > 600;
```

# WHERE

Where - примеры:

-   [Пример условия для WHERE](#where---выбор-с-условием)
-   [Делаем расчеты внутри WHERE](#where---выбор-с-вычислением)

# Пример

## WHERE - Выбор с условием

Выбирает только те строки, которые соответствуют условию:

```sql
SELECT DISTINCT speed, ram FROM Pc
WHERE price<500 AND cd = '24x';
```

## WHERE - Выбор с вычислением

Так же можно производить математические операции со значениями столбцов:

```sql
SELECT DISTINCT speed, ram FROM Pc
WHERE price-100<500 AND price <= speed*2 ;
```

# GROUP BY

GROUP BY - создает в таблице группы, по значениям в выбранном столбце:

-   [Пример использования GROUP BY](#group-by---создание-групп)

# Пример

## GROUP BY - Создание групп

Создает в таблице группы, по значениям из выбранного столбца

```sql
SELECT model, COUNT(model) AS Qty_model, AVG(price) AS Avg_price FROM PC
GROUP BY model;
```

## HAVING

HAVING - как WHERE, но проверка делается для каждой группы полученной GROUP BY отдельно:

```sql
SELECT model, COUNT(model) AS Qty_model, AVG(price) AS Avg_price
FROM PC
GROUP BY model
HAVING AVG(price) < 800;
```

## SELECT

Вытащить столбцы, в произвольном порядке:

```sql
SELECT * FROM Pc;
```

Вытащить столбцы, упорядоченно:

```sql
SELECT speed, ram FROM Pc;
```

Вытащить столбцы,с уникальными строками:

```sql
SELECT DISTINCT speed, ram FROM Pc;
```

Получить строки с пустыми значениями:

```sql
SELECT * FROM Pc WHERE price IS NULL;
```

Переименовать столбцы:

```sql
SELECT ram AS Mb, hd Gb FROM ... WHERE ...;
```

Горизонтальная выборка:

```sql
SELECT DISTINCT speed, ram
FROM Pc
WHERE price<500
ORDER BY 2 DESC;
```

Применить математическое преобразование к столбцу

```sql
SELECT ram * 1024 AS Kb, hd Gb
FROM Pc
WHERE cd = '24x';
```

Столбец заполненный константами. В примере, каждая ячейка столбца ram_units будет содержать 'Mb':

```sql
SELECT ram, 'Mb' AS ram_units, hd
FROM Pc
WHERE cd = '24x';
```

## ORDER BY

Представляет из себя сортировку данных. Можно сортировать:

-   ASC - по возрастанию. Используется по усолчанию, так что можно не писать
-   DESC - по убывнаию

Пример:

```sql
SELECT speed, ram FROM Pc ORDER BY ram DESC;

SELECT speed, ram FROM Pc ORDER BY ram ASC;
```

Также можно сортировать не по имени столбца а по порядкову номеру:

```sql
SELECT speed, ram FROM Pc ORDER BY 2 ASC;
```

При упорядочивании по нескольким столбцам. Сначала сортирует по первому прописанному, а потом, если значения равны, то по второму:

```sql
Упорядочить по нескольким столбцам
SELECT speed, ram FROM Pc
ORDER BY ram DESC, speed ASC;
```

Из-за особенности в порядке обработки запроса селект, агргетные функции которые были прописаны в начале работают только в разделе ORDER BY

## Агрегатные функции

| Название функции  | Что делает                                          |
| :---------------- | :-------------------------------------------------- |
| COUNT(\*)         | Возвращает количество строк источника записей       |
| COUNT(<имя поля>) | Возвращает количество значений в указанном столбце  |
| SUM(<имя поля>)   | Возвращает сумму значений в указанном столбце       |
| AVG(<имя поля>)   | Возвращает среднее значение в указанном столбце     |
| MIN(<имя поля>)   | Возвращает минимальное значение в указанном столбце |
| MAX(<имя поля>)   | Возвращает максимальное значение в указанном столбц |

Пример:

```sql
SELECT MIN(price) AS Min_price, MAX(price) AS Max_price FROM PC;
```