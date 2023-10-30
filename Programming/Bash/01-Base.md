# Bash

Скриптовый язык для работы с системой Linux. Может использовать любые команды системы Linux

# Скрипты

## $PATH

В Linux есть переменная окружения, как в Windows, которая хранит папки в которых ищутся скрипты(комманды)

Посмотреть переменную окружения:

```bash
echo $PATH
```

## Как начать писать

Скрипт должен начинаться с:

```bash
#!/bin/bash
```

## Вывод в консоль

Вывод в консоль:

```bash
echo "Hello world!"
```

Вывод в консоль, без новой строки:

````bash
echo -n "This begin ..."
echo "... this end"
echo "This new line"
# => This begin ...... this end
# => Thie new line
```

## Переменная

Переменная должна быть без пробелов:

```bash
var="It's variable"
````

## Запуск команды Linux

Запуск команды Linux:

```
ping -c 4 www.google.com

hostname="8.8.8.8"
ping -c 4 $hostname
```

## Запуск команды Linux и запись результата в переменную

Запуск команды Linux, и запись результата в переменную:

```bash
list=`ls -l`
```

## Передаваемые значения

В скрипт передаются такие значения, как:

-   \$0 - имя скрипта(файла в котором скрипт)
-   \$1, $2, ... $n - передаваемые аргументы скрипту

Положить значение в переменную:

```bash
x=$1
```

## Получить ответ от пользователя

Получить ответ от пользователя:

```bash
echo "Enter please something..."
read x
```

Вариант в одну строку:

```bash
read -p "Enter please something..." x
```

## f-строки

Любая string переменная по умолчанию f-строка. Переменные подставляются вместо $varname:

```bash
num=45
echo "Value is $num"
# => Value is 45
```

## if ... elif ... else ... fi

Условия пишутся так:

```bash
if [ "$1" == "Vasia" ]; then
    echo "Привет, $1"
elif [ "$1" == "John" ]; then
    echo "Hello, $1"
else echo "You are unknown"
fi
```

## case

Case пишется так:

```bash
x=10

case $x in
          1) echo "This is one";;
      [2-9]) echo "This is Two -> Nine";;
    "Petya") echo "Hello, Petya!";;
          *) echo "Default"
esac
```

## Математические операции

Складываем две переменные:

```bash
num1=45
num2=20
summ=$(($num1+$num2))
```

Или более удобный способ:

```bash
num1=45
let num1=num1*3
```

## while

```bash
$COUNTER = 0

while [ $COUNTER -lt 10 ]; do
    echo "COUNTER = $COUNTER"
    COUNTER=$(($COUNTER+1))
done
```

## for

Классический for:

```bash
for (( i=1; i<=10; i++ )); do
    echo "i = $i"
done
```

Проходимся по строкам:

```bash
for myfile in `ls *.txt`; do
    cat $myfile
done
```

Проходимся по range

```bash
for x in {1..10}; do
    echo "x = $x"
done
```

## Методы(функции)

Функций как таковых нет, есть методы:

```bash
myMethod()
{
    echo "It is my method"
}
```

Передаем параметры:

```bash
summ=0

myMethod()
{
    let summ=$1+$2
}

myMethod 20 40
echo "$summ"
# => 60
```
