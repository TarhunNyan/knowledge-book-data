# Bat - файлы

Файлы выполняющиеся в окружении windows

# База

## Отключить вывод в консоль

Отключить вывод в консоль:

-   При выполнении команд, каждая из них показывается в консоли, чем сильно ее засирает.

```bat
@ECHO OFF
```

## Задать переменную

Задать переменную внутри .bat файла:

```bat
SET TEST="\wamp\bin\php\php5.4.16"
```

## Задать Environment Variable

Можно задать [Environment Virable](../Windows/02-Base.md#environment-variable) через консоль. Задаем User Variable:

```bat
setx VAR_NAME value
```

Задаем System Variable:

```bat
setx VAR_NAME value /m
```

## Комментарий

Комментарий, однострочный:

-   :: - начало комментария

```bat
:: %HOMEDRIVE% = C:\folder - path to folder
```

## Использование переменной

Использование переменной:

-   в примере, происходит вывод значения переменной в консоль

```bat
SET TEST="\wamp\bin\php\php5.4.16"
echo %TEST%
```

## Конкатинация значений

Конкатинация значений:

```bat
SET HOME="C"
SET PATH="\folder\file.txt"

echo %HOME%%PATH%
```

## Вывод в консоль

Вывод в консоль:

```bat
echo "Text output in console"
```

## Пауза

Пауза, остановка работы программы до введения любого символа:

```bat
PAUSE
```

## Список всех переменных

Список всех доступных переменных можно записать в файл:

```bat
set > env-var
```

## Получить переменные из файла

Получить переменные из файла:

-   env-var.txt - файл из которого вытаскиваем переменные

```bat
for /f "delims== tokens=1,2" %%G in (env-var.txt) do set %%G=%%H
```

Пример файла env-var.txt:

-   использование равно внутри переменной, заканчиавает переменную

```txt
PARAM1=value1 it's value
PARAM2=value2
```
