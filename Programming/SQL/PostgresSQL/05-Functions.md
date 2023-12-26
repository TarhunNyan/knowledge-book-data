# Functions

Функции в PostgresSQL

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
