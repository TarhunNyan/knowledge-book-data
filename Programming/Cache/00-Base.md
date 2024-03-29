# Cache

## Отладка

Отладка выводом:

-   [Отладка с выводом в консоль](#отладка---вывод-в-консоль)
-   [Запись в log файл](#отладка---запись-в-log)

## Использование Cache Studio

Перемещение по проекту в Cache:

-   [Провалиться внутрь класса/метода](#cache-studio---провалиться-внутрь)
-   [Вернуться из места куда провалились](#cache-studio---вернуться-из-места-куда-провалились)
-   [Открыть список методов](#cache-studio---открыть-список-методов)
-   [Поиск по тексту](#cache-studio---поиск-по-тексту)
-   [Список унаследованных классов](#cache-studio---список-унаследованных-классов)

# Язык

## Язык - Boolean

Boolean должен возвращать:

-   0 если false, есть макрос $$$NO
-   1 если true, есть макрос $$$YES

## Язык - Macros

Macros - это короткая замена в тексте

-   вызывется через три знака доллара

Полезные макросы

-   До обявления класса надо прописать подключение макросов
    -   Include CommonFuncs
-   Классический набор макросов для ошибок
    -   $$$AssertObject(object) - практически в начале каждой функции проверяем переменные на Null
    -   $$$SOUError("63226") - пометка уникальной ошибки
    -   $$
        Format("translate-name", arg1, arg2, ...)
          - достанет ru/eng строку из файла ru/eng в папке importData/csp/locale/
          - "translate-name" - пометка по которой вытаскиваем строку вида "Здесь текст, и все такое {1}, {2}"
              - {1}, {2} - вместо них подставляет аргументы
        $$
    -   $$
        SouExc - выкинет клиенту ошибку с сообщением
          - throw $$$SouExc($$$SOUError("63226"), $$$Format("my-reports-journal-interrupt-system-error", processId, sc))
        $$
-   Форматирование строки
    -   $$$FormatStringEx("REVOKE {1} ON {2} FROM {3}", Permissions, TableName, RoleName)
-   Полезные приколы
    -   $$$Not(arg) - логическое отрицание
    -   $$$LowerCase(arg) - перевести текст в нижний регистр
    -   $$$UpperCase(arg) - перевести текст в верхний регистр
    -   $$$AddToList - если в него спуститься, там рядом много полезных макросов для работы со списками
-   Математика
    -   $$$Min
    -   $$$Max
    -   $$$Abs
    -   $$$RandomBetween

## Язык - Встроенные конструкции

Задаем переменную:

```cache
set varName = 1
s varName = "variable"
```

Конкатинация строк:

-   \_ - символ конкатенации

```cache
s varStringFoo = "string Foo"
s varString = "string" _ " " _ varStringFoo
```

Создаем список:

```cache
$lb("string 1", "string 2")
```

Делаем из списка строку:

```cache
$LISTTOSTRING($lb("string 1", "string 2"))
```

## Пример

## Cache Studio - Провалиться внутрь

Провалиться внутрь класса/метода:

```bash
CTRL + SHIFT + G
```

## Cache Studio - Вернуться из места куда провалились

Вернуться из места куда провалились:

```bash
CTRL + ALT + G
```

## Cache Studio - Открыть список методов

Быстрый прыжок к методу:

```bash
CTRL + G
```

## Cache Studio - Поиск по тексту

Поиск по тексту:

```bash
CTRL + F
```

Поиск по всем классам:

-   отмена - кнопка которая выглядит как крестик, прерывает поиск

```bash
CTRL + SHIFT + F
```

## Cache Studio - Список унаследованных классов

Список унаследованных классов:

-   показывает список унаследованных классов
-   позволяет перейти к классу унаследованному от текущего

```bash
Верхнее меню |   ...     |   ...                   |
             |   Проект  |   Суперклассы           |
             | > Класс   | > Унаследованные классы |
             |   Собрать |   Рефакторинг           |
             |   ...     |   ...                   |
```

## Cache Studio - Окно навигации

Внизу окна навигации, присутствует три вкладки

-   Проект - не понимаю что там открывается
-   Окна - список открытых окон(очень удобная штука)
-   Область - весь проект со всеми ракетами

## Отладка - Запись в log

Запись в log, для этого используется макрос:

-   C://InterSystem/Cache/mgr/SOU/Logs/Debug2023-11-24.log - путь по которому появляется лог

```cache
$$$LogDebug("example")
```

## Отладка - Вывод в консоль

Вывод в консоль переменных:

```cache
zw columns
```

Вывод в консоль массивов:

```cache
z text,!
```

## Тест #EDIT

Unit тесты в cache заводятся так:

-   ищем по соседству пакет Test
-   в нем создаем/ищем класс в виде ClassNameTest
-   в нем создаем ClassMethod c префиксом test
-   незабываем унасследовать класс от Helpers.Mixins.Testable

```cache
ClassMethod testValidateGuid()
{
	set countGuid = 1000

	for {
		set checkableGuid = $System.Util.CreateGUID()
		set checkResult = ##class(Helpers.Utils.StringHelper).ValidateGuid(checkableGuid)
		Do ..AssertTrue(checkResult, $$$FormatStringEx("Guid: {1}, созданный встроенной утилитой не прошел валидацию", checkableGuid))
		set countGuid = countGuid - 1
		quit:countGuid<1
	}
}
```

Дергаем написанные тесты через консоль Cache:

```bash
do ##class(Helpers.Tests.StringHelper.GuidHelper).RunTests()
```

## Циклы

```cache
while ($$$IsNotNullOrEmpty(someList.getNext(.key))) {
set listItem = someList.getAt(key)
```

```cache

set string1="123 Some Text is here"
set len=$L(string1)
for index=1:1:len-1 {
    LogWrite($EXTRACT(string1, index))
}
```
