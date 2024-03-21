# NiFi

# Структура

Ni-fi состоит из нескольких основных элементов:

-   flow-file - файл записанный на жесткий диск. Всякие ответы от сервера, работа с файлами и т.д. создают или меняют flow-file
-   attribute - аттрибуты flow-file, храняться в оперативке и привязаны к flow-file
-   processor - блок, который обрабатывает flow-file и attribute
    -   process-goup
-   parameter -

# Процессоры

## Порождающие

GenerateFlowFile - с нифига создает FlowFile с указанным Content

## Логирование

LogMessage - пишем текст с использованием ExpressionLanguage

LogAttribute - логируем данные FlowFile

## Тесты

## Работа с файлами

GetFile/GetFTP/FetchFile/FetchFTP - загрузка файлов

## Преобразование

ConvertExcelToCSVProccessor - конвертация(распаковка) excel в csv

ConvertRecord - из коробки предоставляет несколько форматов, которые можно конвертировать друг в друга. Можно добавить и свои

SplitJson - указываем аттрбут, который является массивом, и на выходе получим для каждого элемента массива отдельный FlowFile

JoltTransformJSON - один json преобразуем в другой json, аналог xslt

## Условия

RouteOnContent - условие, которое выбирает маршрут в зависимости от содержания

RouteOnAttribute - условие, которое выбирает маршрут в зависимости от аттрибутов

## Внешнее взаимодействие
