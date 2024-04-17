# Cache

Cache - имплементация такого языка программирования как MUMPS

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

## Анализ производительности

В каше есть инструменты анализа производительности, если кратко то надо :

-   компилируем проект, чтобы исходники были видны в результате анализа
-   подготавливаем классы которые будем анализировать
-   запускаем анализ

А теперь подробно. Компилируем проект с флагами "ckb":

```cache
Do $System.OBJ.CompilePackage("SOU.ETA.Organization","ckb")
```

Подготавливаем классы анализ которых будем проводить:

-   поскольку компиляция может один файл разбить на 6, необходимо добавлять \*

```
SOU.ETA.Organization.REST.Controllers.EmployeeController*
SOU.ETA.Organization.REST.DTO.EmployeeInfoListJSONDTO*
SOU.ETA.Organization.Gateways.UserCrudGateway*
SOU.ETA.Organization.REST.DTO.EmployeeInfoJSONDTO*
SOU.ETA.Organization.StateCollectors.Employee*
SOU.ETA.Organization.BL.FavouriteUserBL*
```

Вызываем анализ:

```cache
Do ^%SYS.MONLBL
```

Проходим примерно следующий путь при вызове:

-   в конце можно выбрать "3) Enter list of PIDs"
    -   будет отслежываться только вызовы из указанной консоли
    -   PID консоли находится у нее в названии

```bash
> 1.) Start Monitor       | (вводим подготовленные для анализа классы) | > 1) Monitor Minimal Metrics  | > 1) Monitor All Processes         |
  2.) Memory Requirements |                                            |   2) Monitor Lines (Coverage) |   2) Monitor Current Proccess Only |
                          |                                            |   3) Monitor Global Metrics   |   3) Enter list of PIDs            |
                          |                                            |   ...                         |                                    |
```

Теперь когда закончишь дергать методы, переходи в консоль и жми:

```cache
(жмем Enter) |   ...                                 | (прописываем *) | (прописываем папку и имя файла, например: C:\docs\cache\debug\longLoadUsers.csv)
             |   5.) Report - Summary                |                 |
             | > 6.) Report - Delimeter (CSV) Output |                 |
             |   7.) Report - Procedure Level        |                 |
```

# База

## Global

Global - это дерево, у которого значения лежат не только в листьях но и в узлах:

-   в своей основе это многомерный массив, могут быть
    -   persistent - сразу сохраняется на диск
    -   registered - хранится только в RAM памяти
-   отличается от простых переменных галочкой(^) перед названием
-   при добавлении значения, сразу происходит сортировка по ключу

Пример в котором кладем значение в Global:

-   ^a - глобал который называется a
    -   создается автоматически, если не было
-   "key1" - узел
    -   в "key1" лежит значение "value1"
    -   внутри узла "key1" лежит "keyLvl2"
    -   внутри узла "keyLvl2" лежит "keyLvl3"
-   "key1", "keyLvl2", "keyLvl3" - сумма длин всех ключей может достигать 511 байт
    -   значение лежащее в ключе может достигать 3.6мб
-   kill ^a("key2")
    -   удалит ветку "key2" и все что в ней

```cache
set ^a("key1") = "value1"
set ^a("key1", "keyLvl2", "keyLvl3") = "value2"
set ^a("key2", "keyLvl2", "keyLvl3") = "value3"
kill ^a("key2")
```

Получаем значение из Global:

```cache
Set data=^Employee(“MGW”,”UK”,”London”,1)
```

Список основных комманд:

-   Set - Установка ветвей до узла (если ещё неопределены) и значения узла
-   Merge - Копирование поддерева
-   Kill - Удаление поддерева/записи
-   ZKill - Удаление значения конкретного узла
-   $Query - Полный обход дерева с заходом вглубь
-   $Order - Обход веток конкретного узла
-   $Data - Проверка определён ли узел
-   $Increment - Атомарное инкрементирование значения узла. Чтобы не делать считывания и записи, для ACID. В последнее время рекомендуется менять на $Sequence

## Increment

$Increment - добавляет значение и возвращает результат:

```cache
Set ^counter = 0  ; установка счётчика
Set id=$Increment(^counter)
```

## Routines

Routines - то что cache в итоге исполняет:

-   Все классы компилируются в Routine, выглядит это так, что один класс может разбиться на несколько файлов Routines
    -   Если был /SOU/ETA/Classname то становится /SOU/ETA/Classname.1, /SOU/ETA/Classname.2, ...
-   Можно создать Rotine руками, и запустить
    -   В русской локализации, Routine перевели как программа
    -   В Студии жмем в навигаторе правой кнопкой и выбираем "создать программу"

Все Routine лежат в Global, и поэтому через Global их можно запустить

```bash
do ^routineName
```

## Order

$Order($O) - возвращает индекс следующего узла после указанного(просто смотри пример):

```cache
s ^test("a", "4") = 10
s ^test("a", "3") = 20
s ^test("a", "5") = 30
s k1 = $ORDER(^test("a", ""))
w k1
// => 3
s k2 = $ORDER(^test("a", k1))
w k2
// => 4
s k3 = $ORDER(^test("a", k2))
w k3
// => 5
```

Вытаскиваем так же значение:

-   targetVariable - переменная(не ссылка) в которую кладем значение из следующего индекса
-   $ORDER(^test("a", "4"), 1, targetVariable) - второй параметр 1, означет прямое направление обхода, 0 означает обратное направление

```cache
s ^test("a", "4") = 10
s ^test("a", "3") = 20
s ^test("a", "5") = 30
s k3 = $ORDER(^test("a", "4"), 1, targetVariable)

w k3
// => 5
w targetVariable
// => 30
```

# Язык

## Переменная по ссылке

Можно передать переменную по ссылке, например в метод:

-   в примере SomeAnotherMethod дергает SomeMethodName
-   .var - это ссылка на переменную var
-   в SomeMethodName мы задаем значение 10, при этом ничего не возвращаем
-   теперь в SomeAnotherMethod значение var будет 10

```cashe
{
    ClassMethod SomeMethodName(var) {
        var = 10;
    }

    Method SomeAnotherMethod() {
        ..SomeMethodName(.var)
        w var
        // => 10
    }
}
```

## Null и Empty

Empty - елсли нет значения в узле, то мы оттуда вытащим пустую строку:

-   бывает что каше возвращает Null(0 символ по ASCII)
-   поэтому лучше использовать макрос, $$$NullOrEmpty

## Циклы

Пример цикла for:

```cache

```

Пример цикла с query:

```cache
#Dim qo As Qm.QueryObject = ##class(SOU.ETA.Organization.Gateways.UserCrudGateway).GetQm()

#Dim usr As SOU.Organization.User
while (qo.Next(.usr))
{
    if $$$Not(##class(Security.Users).Exists(usr.CacheName))
    {
        $$$CheckStatus(##class(Security.Users).Create(usr.CacheName,,"123"))
        w "user created "_usr.CacheName,!
    }

}
```

```cache

set string1="123 Some Text is here"
set len=$L(string1)
for index=1:1:len-1 {
    LogWrite($EXTRACT(string1, index))
}
```

While в духе
while ($$$IsNotNullOrEmpty(someList.getNext(.key))) {
...
}

## Quit VS Return

Quit - выходит из контекста, то есть из циклов и Routines(методов)

Return - выходит только из Routines(методов)

## Условия

Есть короткие условия:

-   задаст значение b только если a равно 10

```cache
s a = 10
s:a=10 b = 20
```

## Язык - Boolean

Boolean должен возвращать:

-   0 если false, есть макрос $$$NO
-   1 если true, есть макрос $$$YES

## Циклы

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

## Удаляем пароли для внешних шлюзов

```cache
UPDATE SOU_Entities_Dictionaries.ImportGateway set Location = 'http://test.com', Password = '123'
```
