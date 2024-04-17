# NiFi

# Структура

## Processor/Processor-group/Queue/Relationships

Ni-fi состоит из нескольких основных элементов:

-   processor - штука которая обрабатывает данные и результаты работы отправляет дальше
-   queue - очередь, собирает в себе результаты работы процессора
    -   у одного processor может быть много очередей в зависимости от того
    -   результаты будут отправляться в разные очереди в зависимости от статуса, с которым отработал processor
    -   основные виды realationhsips:
        -   success - все прошло хорошо
        -   retry - то что определенно, как повторная отправка
        -   failure - то что определенно, как ошибка
        -   origin - то что пришло на processor
-   process-goup - блок внутри которого другие processor блоки

## Flow-file/Content/Attribute

Данные в ni-fi работают по следующему принципу:

-   flow-file - штука-дрюка подающаяся на вход процессорам и выходящая из них в очереди
    -   content - данные записанные на жесткий диск
        -   всякие ответы от сервера, работа с файлами и т.д. создают или меняют flow-file
        -   при перезапуске системы не будут потеряны, ибо лежат на харде
    -   attribute - аттрибуты flow-file, храняться в оперативке и привязаны к flow-file
        -   места где можно использовать переменные их attribute помечены как EL
        -   el - ExpressionLanguage, небольшой язык встроенный в ni-fi для преобразования данных
-   Context - штука которая хранит в себе перменные общие для Processgroup
    -   можно задать один context для разных processgroup
    -   места где можно использовать переменные их context помечены как PARAM
-   processor - блок, который обрабатывает flow-file и attribute

Работа с Content:

-   #{varname} - чтобы обратиться к переменным context где это возможно

```nifi
#{varname}
```

Работа с Attribute:

```ni-fi
{$atr_var1:multiply({$atr_var2})}
```

# Процессоры

Создать flowfile:

-   [GenerateFlowFile - создает flow-file с указанным текстом](#GenerateFlowFile)

Условия в ni-fi:

-   [RouteOnAttribute - кидает flow-file в разные relationship](#RouteOnAttribute)

Работа с attribute:

-   [UpdateAttribute - создание и вычисление attribute](#UpdateAttribute)
-   [EvaluateJsonPath/EvaluateXPath/EvaluateXQuery/ExtractText - на основнии выцепленного из content значения создает attribute](#EvaluateJsonPath/EvaluateXPath/EvaluateXQuery/ExtractText)

Работа с файлами с диска:

-   [GetFile - прочитать файл с диска](#GetFile)
-   [PutFile - записать файл](#PutFile)
-   [GroovyScript - запись в файл](#groovyscript---запись-в-файл)

# Примеры

## GenerateFlowFile

GenerateFlowFile - создает flow-file с указанным текстом:

-

## GetFile

GetFile - читает файл с диска(по умолчанию его удаляет):

-   Keep Source File - ставим true, чтобы не удалял ничего

```ni-fi
Input Directory: /opt/nifi/nifi-current/extra-libs/eta-channel-parser
File Filter: elasticsearch-error.log
Keep Source File: true
```

## PutFile

PutFile - записывает файл, довольно бесполезен поскольку всегд перезаписывает содержимое

## GroovyScript - запись в файл

GroovyScript - запись в файл:

-   filename - аттрибут flow-file в котором прописано имя файла на запись

```groovy
import org.apache.commons.io.IOUtils
import java.nio.charset.*
def flowFile = session.get()
if(!flowFile) return

def stringFilename = flowFile.filename.toString()
f = new File("/opt/nifi/nifi-current/extra-libs/eta-channel-parser/${stringFilename}")

flowFile = session.write(flowFile, {inputStream, outputStream ->
    try {
         f.append(IOUtils.toString(inputStream, StandardCharsets.UTF_8)+'\n')
    }
    catch(e) {
         log.error("Error during processing custom logging: ${filename}", e)
    }
} as StreamCallback)
session.transfer(flowFile, REL_SUCCESS)
```

## RouteOnAttribute

RouteOnAttribute - исходя из описанных условий кидает flow-file в разные relationship:

-   isElastiSearchError - кастомный attribute, которй также будет именем relationship
-   isInvokeHttpError - кастомный attribute, которй также будет именем relationship

```ni-fi
isElastiSearchError: ${elasticsearch.query.error:isEmpty():not()}
isInvokeHttpError: ${invokehttp.status.message:isEmpty():not()}
```

## UpdateAttribute

UpdateAttribute - создание и вычисление attribute:

-   вычисление attribute происходит на основе ПРИШЕДШИХ в UpdateAttribute значений
-   filename - кастомный attribute в который кидаем константу
-   value - кастомный attribute в значение которого вычисляем

```ni-fi
filename: elasticsearch-error.log
value: ${prev_value:multiply(10)}
```

## EvaluateJsonPath/EvaluateXPath/EvaluateXQuery/ExtractText

EvaluateJsonPath/EvaluateXPath/EvaluateXQuery/ExtractText - на основнии выцепленного из content значения создает attribute:

-   у flow-file в content должен быть json/xml/xml/text

Пример на основании EvaluateJsonPath:

-   chat_id - кастомный attribute
    -   $.chat_id - берем значение из поля chat_id
-   title - кастомный attribute
    -   $.book.title - берем полу title из поля book
-   countMessages - кастомный attribute
    -   messages - поле в котором массив
    -   length() - вернет число, обозначающее размер массива

```ni-fi
chat_id: $.chat_id
title: $.book.title
countMessages: $.messages.length()
```

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
