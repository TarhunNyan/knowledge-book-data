# Работа с сетью

## Получить IP

Получить IP и много сетевой информации:

```bash
ifconfig
```

В выдаче:

-   HWaddr - MAC адрес
-   inet addt - IP адрес
-   Bcast - broadcast адрес
-   Mask - маска

Так же возвращает все устройства:

-   enp0s3 - сетевая карта
-   lo - локальный адрес

## Подулючиться к Linux по SSH

В первю очередь два компа должны друг друга видеть. Проверить можно сделав ping:

```bash
# узнаем IP к которому подключаемся
ifconfig

# ping от ПК с которого подключаемя
ping 192.168.10.131
```

Проверяем наличие пакета SSH:

-   Loaded: not-found - если есть такая запись, значит надо установить ssh

```bash
service ssh status
# => ssh.service
# =>    Loaded: not-found
# =>    Active: inactive
```

Устанавливаем пакет ssh:

```bash
sudo apt-get install openssh-server
```

Проверяем активирован ли SSH:

-   Active: inactive - SSH неактивен, нужно активировать

```bash
service ssh status
# => ssh.service
# =>    Loaded: loaded
# =>    Active: inactive
```

Активируем SSH:

```bash
service ssh start
```

Подключаемся по ssh:

-   192.168.10.131 - это IP компьютера к которому подключаемся
-   petya - имя пользователя на другом ПК к которому коннектимся

```bash
ssh petya@192.168.10.131
```

## Другие команды

Че система делает с сетевыми запросами:

```bash
route
```

Пингует по адресу:

```bash
ping www.google.com

# пингует 4 раза
ping c 4 www.google.com
```

Вернет ip www.google.com:

```bash
host www.google.com
```

Че по портам:

```bash
netstat
```

Открыть порт 22:

```bash
sudo ufw allow 22
```

Подключение по SSH:

```bash
# устанавливаем пакет
sudo apt-get install openssh-server
# открываем порт
sudo ufw allow 22
# подключаемся
ssh LinuxName
```

Скачать файл из инета:

```bash
wget http://google.com/image1.png
```

Получить имя терминала:

```bash
hostname
```

Отправить POST/GET запрос:

```bash
curl -X POST http://localhost:3000/questions
```

Добавить даныне:

```bash
curl -X POST http://localhost:3000/questions -F 'body=My First Body' -F 'user_id=1'
```

Вложенные данные. Создасть question = { body: '...', user_id: 1}:

```bash
curl -X POST http://localhost:3000/questions -F 'question[body]=My First Body' -F 'question[user_id]=1'
```
