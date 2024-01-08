# curl

curl - браузер командной строки

# Пример

## Response - Header

Чтобы увидеть все header ответа вместе с ответом:

-   -i/--include - показывает все header

```bash
curl -i google.com
curl --include google.com
```

## Response - Header

Получаем только header ответа:

-   -I/--head - показывает все header

```bash
curl -I google.com
curl --head google.com
```

## Request - Redirect

Разрешаем сделать сайту redirect:

-   -L/--location - разрешает сделать redirect

```bash
curl -L google.com
curl --location google.com
```

## Request - Redirect

Разрешаем сделать сайту redirect:

-   -k/--insecure - разрешаить незащищенные подключения

```bash
curl -k google.com
curl --insecure google.com
```

## Request - Verbose

Полная информация, о том как и куда подключались:

-   -v - информация о подключениях

```bash
curl -v google.com
```

## Request - GET

Простой get запрос к странице:

```bash
curl google.com
```

## Request - Скачиваем файл

Скачиваем файл:

-   -o/--output myfile.jpg - указывает куда записываем полученный ответ

```bash
curl google.com/icon -o myfile.jpg
curl google.com/icon --output myfile.jpg
```

Скачиваем файл с тем же именем, что указан в адресе:

-   -O/--remote-name - указываем, что сохраняем файл в файл с таким же именем

```bash
curl google.com/icon -o
curl google.com/icon --remote-name
```

## Request - Продолжаем прерванное скачивание

Продолжаем прерванное скачивание:

-   -С/--continue-at
    -   флаг указывает что продолжаем скачивание
    -   после него должно идти число, указывающее с какого места продолжаем скачивание
    -   после флага вместо числа может идти -, тогда curl сам попытается понять откуда продолжаем скачивать

```bash
curl -o -С - google.com/somefile.iso
curl -o --continue-at - google.com/somefile.iso
curl -o -С 160 google.com/somefile.iso
curl -o --continue-at 160 google.com/somefile.iso
```

## Request - Аутентификация

Аутентификация, по методу HTTP Basic:

-   -u login:password/--user login:password
    -   что обращение с логином и паролем
    -   нужно указать и логин и пароль
    -   можно указать только логин, тогда пароль придется ввести позже

```bash
curl -u gs01:12345678 google.com
curl --user gs01:12345678 google.com
```

## Request - Использование файлов конфигурации

Использование файлов конфигурации:

-   файл конфигурации с именем вида:
    -   .curlrc
    -   \_curlrc
    -   .netrc
-   -K/--config
    -   флаг указывающий на использование файла конфигурации

```bash
curl -u gs01:12345678 google.com
curl --user gs01:12345678 google.com
```

## Request - Убираем вывод статистики

Убираем вывод статистики, полезно в скриптах:

-   -s/--silent - флаг указывающий скачивание по тихому(silent)
-   --output myfile.jpg - указывает куда записываем полученный ответ

```bash
curl -s google.com/icon --output myfile.jpg
curl -silent google.com/icon --output myfile.jpg
```

## Request - Отправляем данные

Отправляем даные в запросе:

-   -d '...' - флаг указывающий что отправляем данные и сами данные

```bash
curl google.com/icon -d '{"username":"name", "password":"12345678"}'
```

## Request - Отправляем данные из файла

Отправляем даные в запросе, из файла:

-   -d @folder/file.json - флаг указывающий что отправляем данные и путь до самих данных(обязательно через @)

```bash
curl google.com/icon -d @folder/file.json
```

## Request - Задаем header

Задаем header:

-   -A/--user-agent Firefox - задаем в header поле user-agent равное стандартному user-agent для Chrome

```bash
curl -A "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/106.0.0.0" google.com
curl --user-agent "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/106.0.0.0" google.com
```

## Выгрузка файла

Выгрузка файла на сервер, по FTP:

-   k - устраняет проблемы с сертификатом
-   -T C:\Users\Surender\Downloads\sample1.zip - отправляемый файл
-   -u testlab\surender - пользователь
-   ftps://192.168.0.80/awesomewebsite.com/files/ - куда отправляем файл

```bash
curl -kT C:\Users\Surender\Downloads\sample1.zip -u testlab\surender ftps://192.168.0.80/awesomewebsite.com/files/
```

Выгрузка набора файлов:

```bash
curl -kT sample[1-5].zip -u testlab\surender ftps://192.168.0.80/awesomewebsite.com/files/
```

## Request - Отправляем Cookie

Отправляем Cookie в формате клю-значение:

```bash
curl -b 'session=xyz' -b 'loggedin=true' google.com
```

## Request - Отправляем Cookie из файла

Отправляем Cookie из файла:

-   -b path/cookie_file
    -   флаг указывает, что сохраняем cookie
    -   прописываем путь до файла

```bash
curl -b path/cookie_file google.com
```

## Request - Сохраняем Cookie

Сохраняем Cookie в файл:

-   -c path/cookie_file
    -   флаг указывает, что сохраняем cookie
    -   прописываем путь до файла

```bash
curl -c path/cookie_file google.com
```

## Request - Используем прокси сервер

Используем прокси серве:

-   -x 192.168.0.250:8088/--proxy 192.168.0.250:8088
    -   флаг указывает, что используем прокси сервер
    -   указываем адрес то прокси сервера
-   -U username:password/-proxy-user username:password
    -   флаг указывает, логин и пароль для прокси сервера
    -   указываем логин и пароль

```bash
curl -x 192.168.0.250:8088 -U username:password https://awesomewebsite.com/
curl --proxy 192.168.0.250:8088 -proxy-user username:password https://awesomewebsite.com/
```

## Request - Дополнительный Header

Дополнительный Header:

-   -H "x-client-os: Windows 11 Enterprise (x64)"
    -   флаг добавляющий заголовок к запросу

```bash
curl -H "x-client-os: Windows 11 Enterprise (x64)" https://awesomewebsite.com
```

## Request - Указываем протокол

Указываем протокол - если не пролучается curl попробробует другое соединение:

-   --http0.9 - протокол

```bash
curl --http0.9 Firefox google.com
curl --http1.0 Firefox google.com
curl --http1.1 Firefox google.com
curl --http2.0 Firefox google.com
```
