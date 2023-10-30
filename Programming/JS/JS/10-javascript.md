# JS

JavaScript - это база, это знать надо

# Структуры

## Объявление переменных

Объявление переменных:

-   [var - глобальная переменная](#объявление-переменных---var)
-   [let - локальная переменная](#объявление-переменных---let)
-   [const - константа](#объявление-переменных---const)

## Условия

Условия бывают:

-   [Условие типа: if ... else ...](#условия---ifelse)
-   [Условие типа: switch](#условия---switch)
-   [Тернарный оператор](#условия---тернарный-оператор)

## Циклы

Циклы while:

-   [Классический while](#цикл---while)
-   [Классический do...while](#цикл---dowhile)

Циклы for:

-   [Классический for](#цикл---for)
-   [Пройтись по иттерируемым объектам](#цикл---forof)
-   [Пройтись по ВСЕМ свойствам объекта в цикле](#цикл---forin)
-   [Цикл по асинхронно иттерируемым объектам](#цикл---for-awaitfor)

Break, continue и label:

-   [Break](#цикл---break)
-   [Continue](#цикл---continue)
-   [Label](#цикл---label)

## Ошибки

Ошибки отлавливаются через try...catch...finally:

```js
try {
    // блок кода который пытаемся выполнить

    // вызываем ошибку
    throw Error('messge');
} catch(error) {
    // блок кода при пойманной ошибке
    if(error instanceof TypeError) {
        ...
    }
} finally {
    // блок выполняющийся в любом случае
}
```

# Синтаксический сахар

Обработка undefined:

-   [Если false, то новое значение](#syntaxsugar---double-pipe)
-   [Если undefined, то новое значение](#syntaxsugar---double-qestion)

# Асинхронность

В JS есть много моментов, когда надо дождаться чего-то. Например загрузки данных, конца таймера и т.д. Но JS все делается в один поток. Чтобы все не тормозило есть асинхронность. Это когда ты указываешь функции, что она может начать выполняться послк какого-то события

## Callback

Callback - эта функция, которая передается в качестве аргумента и вызывается в момент, когда асинхронное событие закончилось:

```js
setTime(function Callback() {
	console.log('Прошло 2.5 секунды');
}, 2500);
```

Единственный способ использовать вложенную асинхронность - использовать Callback внутри Callback'а. Это приводит к проблеме которую называют: **Callback Hell**

## Promise

Promise - специальная обертка вокруг Callback, созданная чтобы избежать высокой вложенности. Вместо этого мы строим длинную цепочку вызовов:

-   [Базовый пример и синтаксис Promise](#promise---базовый-пример)
-   [Многоураовневые(вложенные) Promise](#promise---вложенность)
-   [Async-функции(async)](#promise---async)
-   [Ждем выполнения async-функции(await)](#promise---await)
-   [Ждем конца работы нескольких Promise](#promise---all)
-   [Ждем конца работы хотя бы одного Promise](#promise---race)

# Типы данных

## String

Array - строка. Создать строку:

-   [Создать строку](#string---создать-строку)
-   [Создать f-string](#string---f-string)

Получить элемент строки:

-   [Получить символ строки по индексу](#string---получить-символ-по-индексу)
-   [Получить подстроку по индексу](#string---substring)
-   [Получить подстроку по индексу#2](#string---slice)

Поиск подстроки в строке:

-   [Ищет вхождение подстроки в строке](#string---indexof)
-   [Ищет вхождение подстроки в строке(с конца)](#string---lastindexof)
-   [Проверяет, начинается ли строка с подстроки](#string---startswith)
-   [Проверяет, окончивается ли строка на подстроку](#string---endswith)
-   [Проверяет, наличие подстроки в строке](#string---includes)

Соединить строки:

-   [Соединить строки](#string---concat)
-   [Повторить строку n раз](#string---repeat)

Разбить строку:

-   [Разбить строку](#string---split)

Строку в число и обратно:

-   [Символ в число](#string---charcodeat)
-   [Числа в строку](#string---fromcharcode)

## String#2 - Форматирование строки

Форматирование строки:

-   [Удалить пробелы вконце строки](#string---trimend-trimright)
-   [Удалить пробелы вначале строки](#string---trimstart-trimleft)
-   [Удалить пробелы вначале и вконце строки](#string---trim)
-   [Добавить символ n раз вначале](#string---padstart)
-   [Добавить символ n раз вконце](#string---padend)

Строчные и заглавные буквы:

-   [Сделать все буквы строчными](#string---tolowercase)
-   [Сделать все буквы строчными#2](#string---toLocaleLowerCase)
-   [Сделать все буквы заглавынми](#string---touppercase)
-   [Сделать все буквы заглавынми#2](#string---toLocaleUpperCase)

Теги:

-   [Обрамление тегами](#string---обрамление-тегами)

## Array

Array - массив. Создать массив:

-   [Создать массив(стандартный синтаксис)](#array---создать)
-   [Создать массив из элементов(of)](#array---of)
-   [Создать массив из array-like объектов](#array---from)
-   [Создать массив из array-like асинхронных объектов](#array---fromasync)

Работа с элементами массива:

-   [Длина массива](#array---length)
-   [Заполнить массив указанными значениями](#array---fill)

Булевые функции:

-   [Проверяем, массив ли это](#array---isarray)
-   [Проверяем, есть ли элемент в массиве](#array---includes)

## Array#2

Получить элементы массива:

-   [Стандартный способ](#array---получить-элемент-массива)
-   [Получить элемент через at. Можно с конца](#array---at)
-   [Получить "срез" массива](#array---slice)
-   [Получить "срез" массива(удаляет)](#array---splice)

Добавить элемент массива:

-   [Добавить элемент в конец массива](#array---push)
-   [Добавить элемент в начало массива](#array---unshift)
-   [Добавить элементы вместо других](#array---splice)

Удалить элемент массива:

-   [Удалить элемент из конец массива](#array---pop)
-   [Удалить элемент из начала массива](#array---shift)
-   [Удалить элементы по индексам](#array---splice)

Заменить элементы массива:

-   [Заменяет элементы на новые](#array---splice)
-   [Заменяет элементы по индексу, на элементы из того же массива](#array---copywithin)

## Array#3

Поиск элемента:

-   [Ищет элемент в массиве, возвращает индекс](#array---indexof)
-   [Ищет элемент в массиве(с конца), возвращает индекс](#array---lastindexof)
-   [Ищет элемент в массиве, по условию, возвращает элемент](#array---find)
-   [Ищет элемент в массиве(с конца), по условию, возвращает элемент](#array---findlast)
-   [Ищет элемент в массиве, по условию, возвращает индекс](#array---findindex)
-   [Ищет элемент в массиве(с конца), по условию, возвращает индекс](#array---findlastindex)

Соединить:

-   [Соединить массив в строку](#array---join)
-   [Соединить массивы в один](#array---concat)

## Array#4

"Разравнивание" массива:

-   ["Разравниваем" массив на n уровней](#array---flat)
-   [Применяем map, а потом "разравниваем" массив](#array---flatmap)

Иттераторы keys, values:

-   [Иттератор - возвращающий значения](#array---values)
-   [Иттератор - возвращающий ключи](#array---keys)
-   [Иттератор - возвращающий пару: ключ, значение](#array---entries)

Элементы функционального программирование:

-   [Пройтись по массиву в циклу(forEach)](#array---foreach)
-   [Выбрать элементы(filter)](#array---filter)
-   [Отсортировать элементы(sort)](#array---sort)
-   [Применить функцию к каждому элементу(map)](#array---map)
-   [Аккумулирующая проходка по массиву(reduce)](#array---reduce)
-   [Аккумулирующая проходка(с конца) по массиву(reduceRight)](#array---reducerigth)
-   [Перевернуть массив(reverese)](#array---reverese)
-   [Хотя бы один элемент true(some)](#array---some)
-   [Все элементы true(every)](#array---every)

## Function

Объявление функции:

-   [Объявление функции](#function---объявление-функции)

Параметры функции:

-   [Получить имя функции](#function---name)
-   [Получить количество входных параметров функции](#function---length)
-   [Получить массив входных параметров внутри функции](#function---arguments)

Контекст(this):

-   [Задаем новый контекст](#function---bind)
-   [Переопределяем контекст(call)](#function---call)
-   [Переопределяем контекст(apply)](#function---apply)

Прототипы(объекты и классы):

-   [Прототипы](./04-Object.md#prototypes)

## Iterator

Ко встроенным иттераторам относятся:

-   String, Array, TypedArray, Map, Set

Иттераторы и генераторы, базовый синтаксис:

-   [Иттератор(объект с функцией next)](#iterator---иттератор)
-   [Иттератор(передать значение через next)](#iterator---иттератор)
-   [Иттератор(объект по которому можно иттерироваться)](#iterator---объект-со-встроенным-иттератором)
-   [Генератор(функция со звездочкой)](#iterator---генератор)
-   [Генератор(return)](#iterator---return-в-генераторах)

## Regexp

О регулярных выражениях в JS можно посмотреть здесь:

-   [Регулярные выражения в JS](../../Regexp/Regexp_JS.md)

# Collections

Коллекциибыли добавлены с ES6

## Map

Map отличается от Object тем что:

-   Не тащит с собой свойства prototype и дефолтные ключи
-   Может содержат ключи любого типа, а не только строки и символы
-   Сохраняет ключи в порядке добавления
-   Число элементов подсчитается в свойстве size
-   Иттерируемый

Когда использовать:

-   Если часто добавляются и удаляются ключи, то Map твой идеальный выбор

Базовые операции:

-   [Создаем map](#map---инициализируем)
-   [Получить размер коллекции](#map---размер-коллекции)
-   [Получить значение из map](#map---получаем-значения)
-   [Добавляем элемент в map](#map---добавить-элементы)
-   [Удаляем элемент коллекции](#map---удаляем)
-   [Проверяем наличие ключа](#map---проверяем-наличие-ключа)

Работа сразу с группой элементов:

-   [Иттерируемся по коллекции](#map---иттерируемся-по-коллекции)
-   [Копируем map](#map---копируем-коллекцию)
-   [Объединяем map](#map---объединяем-коллекции)
-   [Получаем ключи/значения](#map---получить-ключизначения)

## Set

## WeakMap

## WeakSet

# Важные вещи

Важные вещи:

-   [Вывод в косоль](#важные-вещи---consolelog)
-   [Комментарии](#важные-вещи---комментарии)

# Примеры

## Объявление переменных - var

var - инициализирует переменную ДО выполнения кода. Область видимости var - вся функция, когда у let - блок {...}

```js
function test() {
	console.log(x);
	{
		var a = 10;
	}
	console.log(x);
}
test();
// => undefined
// => 10
```

Объявив внутри цикла for переменную через var, будем видеть переменную и вне блока. С let, только внутри цикла

```js
for (var i = 0; i < 5; i++) {}
console.log(i);
// => 5

for (let j = 0; j < 5; j++) {}
console.log(j);
// => ERROR
```

## Объявление переменных - let

let - инициализирует переменную в runtime. Область видимости - блок {...}

```js
{
	let i = 10;
}
```

Директива let создает новую переменную каждый раз когда встречается в коде, и хоть имена у переменных одинаквые, это ссылки на разные ячейки памяти:

```js
for (var i = 0; i < 5; i++) {}
console.log(i);
// => 5

for (let j = 0; j < 5; j++) {}
console.log(j);
// => ERROR
```

## Объявление переменных - const

const - инициализирует переменную в runtime. Область видимости - блок {...}:

```js
const TEXT = 'It is constant text';
```

Работает точно так же как и let, за исключением того, что поместив туда ссылку, поменять ее нельзя

```js
const TEXT = 'It is constant text';
TEXT += ' 2';
// => ERROR
```

## Условия - if/else

Стандартное условие:

-   condition - логическое выражение
-   { ... } - тело условия

```js
if( condition ) { ... } else { ... }
```

Если у условия должно быть больше ветвлений:

-   condition - логическое выражение
-   { ... } - тело условия

```js
if( condition ) { ... }
else if( condition ) { ... }
else if( condition ) { ... }
else { ... }
```

## Условия - тернарный оператор

Условие в одну строку:

-   condition - логическое выражение
-   10 - переменная a будет 10, если condition вернет true
-   undefined - переменная a будет undefined, если condition вернет false

```js
let a = condition ? 10 : undefined;
```

## Условия - switch

Множественное ветвление:

-   value - переменная которую сравниваем
-   break - необходимо ставить, иначе не выйдет из switch и продолжит делать проверки

```js
switch (value) {
    case 10:
        // выполнить если value === 10
        break;
    case "ok":
        // выполнить если value === "ok"
        break;
    default:
        // выполнить если ни один случай не подошел
        break;
```

## Цикл - while

Цикл while:

-   пока выполняется условие condition, выполняет { ... }

```js
while ( condition ) { ... }
```

## Цикл - do...while

Цикл do...while:

-   отличается от while тем, что обязатель но выполнит { ... } хотя бы раз
-   выполняет { ... }, пока выполняется условие condition

```js
do { ... } while ( condition )
```

## Цикл - for

Классика, как в C:

```js
for(let i=0; i<obj.length; i++) { ... }
```

Цикл с несколькими инициализациями:

```js
for(let i=0, max=obj.length; i<max; i++) { ... }
```

Цикл с несколькими инициализациями и шагами:

```js
for(let i=0, j=10; i<max; i++, j--) { ... }
```

Цикл без инициализации:

```js
for(; i<obj.length; i++) { ... }
```

Цикл только с условием:

```js
for(; i<obj.length;) { ... }
```

Бесконечный цикл:

```js
for(;;;) { ... }
```

## Цикл - for...of

Обходит иттерируемые объекты:

-   иттерируемые: Array, String, Map, Object, Set, function\*, DOMCollection
-   [кастомный иттерируемый объект](./04-Object.md#иттерируемый-объект)

```js
for (const value of iterable) { ... }
```

## Цикл - for...in

Проходит по всем свойстам(enumerable не важен) объекта, включая сойства прототипов:

```js
var obj = { a: 1, b: 2, c: 3 };

for (var prop in obj) {
	console.log('obj.' + prop + ' = ' + obj[prop]);
}
// => "obj.a = 1"
// => "obj.b = 2"
// => "obj.c = 3"
```

## Цикл - for await...for

Цикл проходящий по асинхронно этерируемым объектам. В примере создается асинхронный иттератор:

```js
var asyncIterable = {
	[Symbol.asyncIterator]() {
		return {
			i: 0,
			next() {
				if (this.i < 3) {
					return Promise.resolve({ value: this.i++, done: false });
				}

				return Promise.resolve({ done: true });
			},
		};
	},
};

(async function () {
	for await (let num of asyncIterable) {
		console.log(num);
	}
})();

// => 0
// => 1
// => 2
```

## Цикл - break

break - прерывет цикл и начинает выполнять код идущий после него:

-   в примере мы доходим до 5, срабатывает break и цикл прекращает свою работу

```js
for (i = 0; i < 10; i++) {
	if (i === 5) {
		break;
	}
}
```

## Цикл - continue

continue - прыгает к следующей иттерации цикла:

-   в примере выведет цифры от 1 до 10 пропустив 5

```js
for (i = 0; i < 10; i++) {
	if (i === 5) {
		continue;
	}
	console.log(i);
}
```

## Цикл - label

label - метка, на которую мы перемещаемся при использовании brek или continue:

-   в примере проходимся по двумерному массиву, для каждого элемента и если какой-то элемент равен 1, переходим к след строке

```js
labelName: for (let i = 0; i < data.length; i++) {
	labelNoUse: for (let j = 0; j < data[i].length; j++) {
		if (data[i][j] === 1) {
			continue labelName;
		}
		data[i][j] = -1;
	}
}
```

## Promise - базовый пример

Базовый пример Promise:

```js
new Promise((resolve, reject) => {
	// Код, оканчание работы которого мы ждем
	setTimeout(function Main() {
		// resolve - функция которую надо вызвать если все сработало хорошо
		// перекинет data в блок then
		if (true) {
			resolve('Прошла 1 сек');
		}

		// reject -  - функция которую надо вызвать если все отработало плохо
		// перекинет в блок catch
		else {
			reject('Не прошла 1 сек! Все плохо!');
		}
	}, 1000);
})
	.then((data) => {
		// Код выше отработал
		// Обрабатываем переданные нам данные(data)
	})
	.catch((reason) => {
		// Код выдал ошибку
		// Обрабатываем причину(reason) ошибки
	})
	.finally(() => {
		// Код который срабатывает всегда
		// Вне зависимости от ошибок
	});
```

## Promise - вложенность

Вложенность построенна на том факте, что если функция в then/catch возвращает Promise, то этот Promise подставляется вместо текущего:

Пример:

```js
new Promise((resolve, reject) => {
	setTimeout(() => resolve('Прошла 1 сек'), 1000);
})
	.then((data) => {
		console.log(data);
		return new Promise((resolve, reject) => {
			setTimeout(() => resolve('Прошло еще 1 сек'), 1000);
		});
	})
	.then((data) => {
		console.log(data);
		return new Promise((resolve, reject) => {
			setTimeout(() => reject('Вызываем ошибк спустя 1 сек'), 1000);
		});
	})
	.catch(console.log);
```

## Promise - async

async - делает функцию асинхронной. Создан чтобы писать асинхронный код в синхронном стиле:

-   async - создает обертку вокруг функции. Оборачивает в объект Promise

```js
async function hello(a) {
	if (a === true) {
		return 'Hello';
	} else {
		throw 'Buy';
	}
}

hello(true)
	.then((data) => console.log('OK: ' + data))
	.catch((reason) => console.log('Error: ' + reason));
```

## Promise - await

await - заставляет программу не идти дальше, пока не функция не завершит работу:

-   в примере, не выведет "ok", пока getFromServer не получит ответ с сервера

```js
async function getFromServer() { ... };

await getFromServer();
console.log("ok")
```

Правильно в начале запустить все async функции, а только в конце ждать ответа от них:

```js
async function timeTest() {
	// Запускаем 1
	const timeoutPromise1 = timeoutPromise(3000);
	// Запускаем 2 промис
	const timeoutPromise2 = timeoutPromise(3000);
	// Запускаем 3 промис
	const timeoutPromise3 = timeoutPromise(3000);

	// Пока ждем, исполненяются сразу все промисы
	await timeoutPromise1;
	await timeoutPromise2;
	await timeoutPromise3;
}
```

## Promise - all

Ждет когда ВСЕ промисы в списке исполнятся:

```js
async function displayContent() {
	// Запускаем асинхронные функции
	let coffee = fetchAndDecode('coffee.jpg', 'blob');
	let tea = fetchAndDecode('tea.jpg', 'blob');
	let description = fetchAndDecode('description.txt', 'text');

	// Ждем выполнения всех асинхронных функций
	let values = await Promise.all([coffee, tea, description]);
}
```

## Promise - race

Ждет когда ХОТЬ ОДИН промис в списке исполнятся:

```js
async function displayContent() {
	// Запускаем асинхронные функции
	let coffee = fetchAndDecode('coffee.jpg', 'blob');
	let tea = fetchAndDecode('tea.jpg', 'blob');
	let description = fetchAndDecode('description.txt', 'text');

	// Ждем выполнения одной асинхронных функций
	let values = await Promise.race([coffee, tea, description]);
}
```

## Array - copyWithin

Последовательно заменяет элементы на те, что в выбраном месте:

-   Мутирует массив
-   Не меняет длину массива
-   array1.copyWithin(0, 3, 5) - у array1 заменяет элементы с 0 на элементы с [3; 5)
-   array1.copyWithin(1, 3) - у array1 заменяет элементы с 0 на элементы с 3 и до конца

```js
array1.copyWithin(0, 3, 5);
array1.copyWithin(1, 3);
```

## Array - splice

Удалить все элементы начиная с индекса A до B

-   Мутирует массив
-   Возвращает удаленные элементы
-   months.splice(2) - удаляет элементы начиная с 2
-   months.splice(2, 4) - удаляет 4 элемента начиная с 2
-   months.splice(2, 4, 'October', ...) - удаляет 4 элемента начиная с 2 и вставляет на их место: 'October', 'November', 'December'

```js
months.splice(2);
months.splice(2, 4);
months.splice(2, 4, 'October', 'November', 'December');
```

## Array - push

Добавить элемент в конец массива:

-   мутирует массив
-   возвращает количество элементов в получившемся массиве

```js
let arr = [1, 2, 3, 4];
arr.push(5);

console.log(arr);
// => [1, 2, 3, 4, 5]
```

## Array - unshift

Добавить элемент в начало массива:

-   мутирует массив
-   возвращает количество элементов в получившемся массиве

```js
let arr = [1, 2, 3, 4];
arr.unshift(0);

console.log(arr);
// => [0, 1, 2, 3, 4]
```

## Array - pop

Удалить элемент в конце:

-   мутирует массив
-   возвращает удаленный элемент

```js
let arr = [1, 2, 3, 4, 5];
arr.pop();

console.log(arr);
// => [1, 2, 3, 4]
```

## Array - shift

Удалить элемент в начале:

-   мутирует массив
-   возвращает удаленный элемент

```js
let arr = [1, 2, 3, 4, 5];
arr.shift();

console.log(arr);
// => [2, 3, 4, 5]
```

## Array - of

Создает массив из указаных значений:

-   создает массив
-   возвращает созданный массив

```js
console.log(Array.of('cat', 2, 'bar', true));
// => ["cat", 2, "bar", true]
```

## Array - length

Количество элементов массива:

-   НЕ мутирует массив

```js
const arr = [1, 2, 3, 4];
console.log(arr.length);
// => 4
```

## Array - Получить элемент массива

Получить элемент массива. Стнадартный способ:

-   НЕ мутирует массив

```js
const animals = ['ant', 'bison', 'camel', 'duck', 'elephant'];
console.log(animals[1]);
// => ["bison"]
```

## Array - at

Получить элемент массива, через at:

-   НЕ мутирует массив
-   можно использовать отрицательный индекс

```js
const animals = ['ant', 'bison', 'camel', 'duck', 'elephant'];
console.log(animals.at(2));
// => ["camel"]
console.log(animals.at(-1));
// => ["elephant"]
```

## Array - slice

Получить "срез" массива:

-   НЕ мутирует массив

```js
const animals = ['ant', 'bison', 'camel', 'duck', 'elephant'];

console.log(animals.slice(2));
// => ["camel", "duck", "elephant"]

console.log(animals.slice(2, 4));
// => ["camel", "duck"]

console.log(animals.slice(2, -1));
// => ["camel", "duck"]

console.log(animals.slice());
// => ["ant", "bison", "camel", "duck", "elephant"]

console.log(animals.slice(-2));
// => ["duck", "elephant"]
```

## Array - indexOf

Ищет элемент в массиве:

-   НЕ мутирует массив
-   возвращает индекс найденного элемета
-   если не нашел, возвращает -1
-   animals.indexOf('Dodo', 2); - ищет 'Dodo', начиная с 2 элемента

```js
const animals = ['Dodo', 'Tiger', 'Penguin', 'Dodo'];

animals.indexOf('Tiger');
// => 1
animals.indexOf('Dodo');
// => 0
animals.indexOf('Dodo', 2);
// => 3
animals.indexOf('UNDEFINED');
// => -1
```

## Array - lastIndexOf

Ищет элемент в массиве начиная с конца:

-   НЕ мутирует массив
-   возвращает индекс найденного элемента
-   если не нашел, возвращает -1
-   animals.indexOf('Dodo', 1); - ищет 'Dodo', начиная с -1 элемента

```js
const animals = ['Dodo', 'Tiger', 'Penguin', 'Dodo'];

animals.indexOf('Tiger');
// => 1
animals.indexOf('Dodo');
// => 3
animals.indexOf('Dodo', 1);
// => 0
animals.indexOf('UNDEFINED');
// => -1
```

## Array - find

Ищет элемент, подходящий условию:

-   НЕ мутирует массив
-   Принимае в качестве аргумента функцию, которая принимает:
    -   element - элемент
    -   index - индекс элемента
    -   array - массив который рассматриаем
-   возвращает элемент
-   если не нашел, возвращает undefined

```js
const animals = ['Dodo', 'Tiger', 'Penguin', 'Dodo'];

animals.find((el) => {
	el.length > 5;
});
// => Penguin
```

## Array - findLast

Ищет элемент(с конца), подходящий условию:

-   НЕ мутирует массив
-   Принимае в качестве аргумента функцию, которая принимает:
    -   element - элемент
    -   index - индекс элемента
    -   array - массив который рассматриаем
-   возвращает элемент
-   если не нашел, возвращает undefined

```js
const animals = ['Dodo', 'Tiger', 'Penguin', 'Dodo'];

animals.find((el) => {
	el.length > 4;
});
// => Penguin
```

## Array - findIndex

Ищет элемент, подходящий условию:

-   НЕ мутирует массив
-   Принимае в качестве аргумента функцию, которая принимает:
    -   element - элемент
    -   index - индекс элемента
    -   array - массив который рассматриаем
-   возвращает индекс
-   если не нашел, возвращает -1

Ищет элемент, Callback от которого вернет true. Вернет index. Callback как у [find](#find). Если не находят элемент возвращают -1

```js
const animals = ['Dodo', 'Tiger', 'Penguin', 'Dodo'];

animals.findIndex((el) => {
	el.length > 5;
});
// => 2
```

## Array - findLastIndex

Ищет элемент(с конца), подходящий условию:

-   НЕ мутирует массив
-   Принимае в качестве аргумента функцию, которая принимает:
    -   element - элемент
    -   index - индекс элемента
    -   array - массив который рассматриаем
-   возвращает индекс
-   если не нашел, возвращает -1

```js
const animals = ['Dodo', 'Tiger', 'Penguin', 'Dodo'];

animals.findIndex((el) => {
	el.length > 4;
});
// => 3
```

## Array - isArray

Проверяем, массив ли это:

```js
Array.isArray(arr);
```

## Array - includes

Проверяет есть ли элемент в массиве:

-   arr.includes(2); - искомый элемент 2
-   arr.includes(3, 4); - искомый элемент 3, начинает искать с 4 элемента

```js
let arr = [1, 2, 3, 4, 2, 6];

arr.includes(2);
// => true
arr.includes(3, 4);
// => false
```

## Array - concat

Соединяет массивы в один:

-   НЕ мутирует массив

```js
const array1 = ['a', 'b', 'c'];
const array2 = ['d', 'e', 'f'];
const array3 = array1.concat(array2);

console.log(array2);
// => Array ["a", "b", "c", "d", "e", "f"]
```

## Array - join

Соединяет элементы массива в строку:

-   НЕ мутирует массив
-   arr.join("-"); - в качестве соеденителя используется "-"

```js
const arr = ['Раз', 'Два', 'Три', 'Четыре'];
arr.join('-');
// => Раз-Два-Три-Четыре
```

## Array - forEach

forEach - пройдется по массиву и выполнит функцию для каждого элемента:

-   НЕ мутирует массив
-   возвращает undefined

```js
const array1 = ['a', 'b', 'c'];
array1.forEach((element) => { ... });
array1.forEach((element, index, array) => { ... });
```

## Array - filter

Пропускает через функцию значения, те что возращают true, попадают в выходной массив:

-   НЕ мутирует массив
-   возвращает отфильтрованный массив

```js
const words = [
	'spray',
	'limit',
	'elite',
	'exuberant',
	'destruction',
	'present',
];

const result = words.filter((word) => word.length > 6);

console.log(result);
// => Array ["exuberant", "destruction", "present"]
```

## Array - sort

sort - сортирует элементы в массиве:

-   по умолчанию сортирует элементы в алфавитном порядке
-   мутирует массив
-   возвращает отсортированный массив
-   array1.sort((a, b) => a - b); - передаем свою функцию сортировки. Должна возвращать число. Если вернуло меньше нуля - a < b

```js
const months = ['March', 'Jan', 'Feb', 'Dec'];
months.sort();
// => Array ["Dec", "Feb", "Jan", "March"]

const array1 = [1, 30, 4, 21, 100000];
array1.sort((a, b) => a - b);
// => Array [1, 4, 21, 30, 100000]
```

## Array - map

Применяем функцию к каждому элементу:

-   НЕ мутирует массив
-   вовзращает массив, гже к каждому элементу применили функцию

```js
const array1 = [1, 4, 9, 16];
array1.map((x) => x * 2);
// => [2, 8, 18, 32]
```

## Array - reduce

reduce - применяет функцию к аккумулятору и элементу. Элементы идут поочереди:

-   аккмулятор сначало равен intialValue, потом меняется на результат работы функции
-   НЕ мутирует массив
-   возвращает последний аккумулятор

```js
const array1 = [1, 2, 3, 4];

const initialValue = 0;
const sumWithInitial = array1.reduce(
	(accumulator, currentValue) => accumulator + currentValue,
	initialValue
);

console.log(sumWithInitial);
// => 10
```

## Array - reduceRigth

reduceRigth - применяет функцию к аккумулятору и элементу. Элементы идут с конца:

-   аккмулятор сначало равен intialValue, потом меняется на результат работы функции
-   НЕ мутирует массив
-   возвращает последний аккумулятор

```js
const array1 = [1, 2, 3, 4];

const initialValue = 0;
const sumWithInitial = array1.reduceRigth(
	(accumulator, currentValue) => accumulator + currentValue,
	initialValue
);

console.log(sumWithInitial);
// => 10
```

## Array - reverese

Переварачивает массив:

-   мутирует массив
-   возвращает переврнутый массив

```js
const array1 = ['one', 'two', 'three'];
array1.reverse();
// => ["three", "two", "one"]
```

## Array - some

some - если хоть один элемент пройдя функцию будет true, то вернет true:

-   НЕ мутирует массив

```js
const array1 = [1, 2, 3, 5];
const array2 = [1, 3, 5];
const even = (element) => element % 2 === 0;

array1.some(even);
// => true
array2.some(even);
// => false
```

## Array - every

every - вернет true если все элементы массива пройдя функцию вернут true:

-   НЕ мутирует массив

```js
const isBelowThreshold = (currentValue) => currentValue < 40;

const array1 = [1, 30, 39, 29, 10, 13];

console.log(array1.every(isBelowThreshold));
// => true
```

## Array - flat

flat - "разравнивает" массив:

-   НЕ мутирует массив
-   arr1.flat(); - "разравнивает" на 1 уровень
-   arr2.flat(2); - "разравнивает" на 2 уровеня

```js
const arr1 = [0, 1, 2, [3, 4]];
arr1.flat();
// => [0, 1, 2, 3, 4]

const arr2 = [0, 1, 2, [[[3, 4]]]];
arr2.flat(2);
// => [0, 1, 2, [3, 4]]
```

## Array - flatMap

flatMap - применяет к массиву [map](#array---map), а после [flat](#array---flat):

-   НЕ мутирует массив

```js
const arr1 = [1, 2, 1];

const result = arr1.flatMap((num) => (num === 2 ? [2, 2] : 1));
// => [1, 2, 2, 1]
```

## Array - values

keys - создает иттератор из значений массива:

-   для индексов без значения, указывает undefind

```js
const arr = ['a', 'b', 'c'];
const valueIterator = arr.values();

for (const value of valueIterator) {
	console.log(value);
}
// => 'a', 'b', 'c'
```

## Array - keys

keys - создает иттератор из ключей массива:

```js
const array1 = ['a', 'b', 'c'];
const iterator = array1.keys();

for (const key of iterator) {
	console.log(key);
}

// => 0
// => 1
// => 2
```

## Array - entries

entries - возвращает иттератор созданный из массива:

```js
const array1 = ['a', 'b', 'c'];

const iterator1 = array1.entries();

iterator1.next().value;
// => Array [0, "a"]
// => Array [1, "b"]
// => Array [2, "c"]
```

## Array - from

from - создает массив из Array-like объектов. В том числе из иттераторов:

```js
Array.from(ittertor);
// => [ ... ]

Array.from('foo');
// => ["f", "o", "o"]

Array.from([1, 2, 3], (x) => x + x);
// => [2, 4, 6]
```

## Array - fromAsync

fromAsync - создает массив изасинхронных объектов:

```js
const asyncIterable = (async function* () {
	for (let i = 0; i < 5; i++) {
		await new Promise((resolve) => setTimeout(resolve, 10 * i));
		yield i;
	}
})();

Array.fromAsync(asyncIterable).then((array) => console.log(array));
// => [0, 1, 2, 3, 4]
```

## Array - fill

fill - заполняет массив константами:

-   мутирует массив

```js
aconst array1 = [1, 2, 3, 4];

array1.fill(0, 2, 4);
// => [1, 2, 0, 0]

array1.fill(5, 1);
// => [1, 5, 5, 5]

array1.fill(6);
// => [6, 6, 6, 6]
```

## Array - Создать

Создать массив можно так:

```js
let array = [1, 2, 100, 10];
```

## String - trim

trim - удаляем пробелы вначале и вконце строки:

-   НЕ мутирует строку

```js
let line = '     Hello    ';
line.trim();
// => Hello
```

## String - trimEnd, trimRight

trimEnd, trimRight - удаляем пробелы вконце строки:

-   НЕ мутирует строку

```js
let line = '     Hello    ';
line.trim();
// => "     Hello"
```

## String - trimStart, trimLeft

trimStart, trimLeft - удаляем пробелы вначале строки:

-   НЕ мутирует строку

```js
let line = '     Hello    ';
line.trim();
// => "Hello    "
```

## String - padEnd

padEnd - заполняет n символов в конце строки, выбранным символом:

-   НЕ мутирует строку

```js
let line = 'Is a text';

line.padEnd(25, '.');
// => 'Is a text................'
```

## String - padStart

padStart - заполняет n символов в начале строки, выбранным символом:

-   НЕ мутирует строку

```js
let line = 'Is a text';

line.padStart(25, '.');
// => '................Is a text'
```

## String - concat

concat - соединяет строки в одну:

-   НЕ мутирует

```js
const str1 = 'Hello';
const str2 = 'World';

str1.concat(', ', str2, '!');
// => Hello, World!
```

## String - repeat

repeat - повторяет строку n раз и соединяет:

-   НЕ мутирует

```js
let repeatText = 'Happy!';
repeatText.repeat(3);
// => "Happy!Happy!Happy!"
```

## String - slice

slice - вырезаем подстроку по указанным индексам:

-   НЕ мутирует

```js
let line = 'Text here and here';

line.slice(5);
// => "here and here"
line.slice(5, 8);
// => "here"
line.slice(-4);
// => "here"
line.slice(-8, -6);
// => "and"
```

## String - substring

substring - вырезаем подстроку по указанным индексам:

-   НЕ мутирует

```js
let line = 'Text here and here';

line.substring(5);
// => "here and here"
line.substring(5, 8);
// => "here"
```

## String - получить символ по индексу

Получить символ по индексу через специальный синтаксис:

```js
let line = 'this string';
line[2];
// => "i"
```

Получить символ по индексу через функцию charAt:

-   можно передать отрицательный индекс

```js
let line = 'this string';
line.charAt(2);
// => "i"
line.at(-3);
// => "i"
```

## String - создать строку

Строка создается так:

```js
let s1 = 'строка текста';
let s2 = 'строка текста';
```

## String - f-string

f-string - форматированная строка:

-   поддерживает многострочность
-   поддерживает подстановку переменных из кода
    Строки. Определяются следующим образом

```js
let obj = 'еще текст';

let res = `многострочная строка текста так еще и
можно всталять значения переменных ${obj}`;
// => многострочная строка текста так еще и
// => можно всталять значения переменных еще текст
```

## String - toLowerCase

toLowerCase - делает все буквы строчными:

-   НЕ мутирует строку

```js
const sentence = 'The qUicK brOwn fOx';
sentence.toLowerCase();
// => the quick brown fox
```

## String - toUpperCase

toUpperCase - делает все буквы заглавными:

-   НЕ мутирует строку

```js
const sentence = 'The qUicK brOwn fOx';
sentence.toUpperCase();
// => THE QUICK BROWN FOX
```

## String - toLocaleLowerCase

toLocaleLowerCase - делает все буквы строчными:

-   НЕ мутирует строку

```js
const dotted = 'İstanbul';

dotted.toLocaleLowerCase('en-US');
// => "i̇stanbul"

dotted.toLocaleLowerCase('tr');
// => "istanbul"
```

## String - toLocaleUpperCase

toLocaleUpperCase - делает все буквы заглавными:

-   НЕ мутирует строку

```js
const city = 'istanbul';

city.toLocaleUpperCase('en-US');
// Expected output: "ISTANBUL"

city.toLocaleUpperCase('TR');
// Expected output: "İSTANBUL"
```

## String - Обрамление тегами

Все функции обрамления тегами:

-   НЕ мутирует строку

```js
let line = 'This is test string!';

line.link('www://...');
// => '<a href="www://...">This is test string!</a>'
line.fontColor('FF00');
// => '<font color="FF00">This is test string!</font>'
line.fontSize(7);
// => '<font size="7">This is test string!</font>'

line.small();
// => '<small>This is test string!</small>'
line.big();
// => '<big>This is test string!</big>'

line.italics();
// => '<i>This is test string!</i>'
line.bold();
// => '<b>This is test string!</b>'
line.blink();
// => '<blink>This is test string!</blink>'
line.strike();
// => '<strike>This is test string!</strike>'

line.sub();
// => '<sub>This is test string!</sub>'
line.sup();
// => '<sup>This is test string!</sup>'

line.fixed();
// => "<tt>This is test string!</tt>"
```

## String - fromCharCode

fromCharCode - вернет строку, символы в которой, это соответствующие переданным числам символы из ASCII:

```js
String.fromCharCode(65, 66, 67);
//  "ABC"
```

## String - charCodeAt

charCodeAt - указываем индекс символа в строке, вернет число из ASCII:

```js
let line = 'The quick';
line.charCodeAt(4);
// => 113
```

## String - split

split - разделяют строку на подстроки, по выбранному символу:

-   НЕ мутирует

```js
let line = 'Ok, we go to hub!';
line.split(' ');
// => ["Ok,", "we", "go", "to", "hub"]
```

## String - indexOf

indexOf - ищет вхождение подстроки в строке:

-   НЕ мутирует строку

```js
let line = 'This is test string!';

line.indexOf('is');
// => 2
line.includes('is', 3);
// => 5
line.includes('WHAT');
// => -1
```

## String - lastIndexOf

indexOf - ищет вхождение подстроки в строке(с конца):

-   НЕ мутирует строку

```js
let line = 'This is test string!';

line.lastIndexOf('is');
// => 5
line.lastIndexOf('is', 3);
// => 2
line.lastIndexOf('WHAT');
// => -1
```

## String - includes

includes - проверяет наличие подстроки в строке:

-   НЕ мутирует строку
-   регистро-чувствительная функция

```js
let line = 'This is test string!';

line.includes('is');
// => true
line.includes('is', 10);
// => false
```

## String - endsWith

endsWith - окончание строки:

-   НЕ мутирует строку

```js
let line = 'This is test string!';

line.endsWith('string!');
// => true
line.endsWith('test');
// => false
line.endsWith('test', 11);
// => true
```

## String - startsWith

startsWith - начало строки:

-   НЕ мутирует строку

```js
let line = 'This is test string!';

line.startsWith('This');
// => true
line.startsWith('is');
// => false
line.startsWith('is', 5);
// => true
```

## Iterator - объект со встроенным иттератором

Пользовательский объект, по которому можно иттерироваться:

```js
svar myIterable = {}
myIterable[Symbol.iterator] = function* () {
    yield 1;
    yield 2;
    yield 3;
};
```

## Iterator - Иттератор

Иттератор - объект типа фабрика. Каждый вызов функции next возвращает следующий элемент, при этом меняет внутри иттератора состояние:

```js
function makeIterator(array) {
	var nextIndex = 0;

	return {
		next: function () {
			return nextIndex < array.length
				? { value: array[nextIndex++], done: false }
				: { done: true };
		},
	};
}

var it = makeIterator(['yo', 'ya']);
it.next().value; // 'yo'
it.next().value; // 'ya'
it.next().done; // true
```

## Iterator - next с аргументом

Пожно передавать генератору аргументы уже после его создания:

```js
var reset = yield current;
```

Пример:

```js
function* fibonacci() {
	var fn1 = 1;
	var fn2 = 1;
	while (true) {
		var current = fn2;
		fn2 = fn1;
		fn1 = fn1 + current;
		var reset = yield current;
		if (reset) {
			fn1 = 1;
			fn2 = 1;
		}
	}
}

var sequence = fibonacci();
sequence.next().value; // => 1
sequence.next().value; // => 1
sequence.next().value; // => 2
sequence.next().value; // => 3
sequence.next().value; // => 5
sequence.next().value; // => 8
sequence.next().value; // => 13
sequence.next(true).value; // => 1
sequence.next().value; // => 1
sequence.next().value; // => 2
sequence.next().value; // => 3
```

## Iterator - Генератор

Генератор - функция со зездочкой, вместо return используется yield. Превращает функцию в [иттератор](#):

```js
function* idMaker() {
	var index = 0;
	while (true) yield index++;
}

var it = idMaker();

it.next().value; // 0
it.next().value; // 1
it.next().value; // 2
```

## Iterator - return в генераторах

````js
function* generator() {
  yield 1;
  return 2;
  yield 3;
}

var g = generator();

g.next();        // { value: 1, done: false }
g.next();        // { value: 2, done: true  }
g.next();        // { value: undefined, done: true  }


var g = generator();

g.return('foo'); // { value: 'foo', done: true  }
g.next();        // { value: undefined, done: true  }
```
````

## Function - length

Солько параметров принимает Функция

```js
function functionName(a, b, ...args) {}

console.log(functionName.length);
// => 2
```

## Function - name

Возвращает имя функции

```js
function functionName(a, b, ...args) {}

console.log(functionName.name);
// => "functionName"
```

## Function - arguments

arguments - массив, который доступен внутри функции и содержит все переданные функции значения:

```js
function nameFunction(a, b, c, ...arr) {
	console.log(arguments);
	return a + b;
}
```

## Function - объявление функции

Классический способ объявить фнукцию:

-   Еще называется: Function Declaration

```js
function funcName(a, b, c, ...arr) {}
```

Анонимная функция:

-   Еще называется: Function Expression

```js
let varName = function (a, b, c, ...arr) {};
```

Стрелочная функция:

-   не обладаем конткстом(this)

```js
let anonimFunction = (a, b) => {};
let nameFunction = (a) => 10 * a;
```

## Function - bind

Устанавливаеи новый контекст:

```js
let obj = {
	name: 'Vasia',
	hello: function (endText) {
		console.log(
			`Hello, ${this.name}! ${endText === undefined ? '' : endText}`
		);
	},
};

let anotherObj = {
	name: 'Petia',
};

let hello = obj.hello;
let helloVasia = hello.bind(obj);
let helloPetia = hello.bind(anotherObj);

// передаем контекст(this)
hello();
// => "Hello, indefined!"
helloVasia();
// => "Hello, Vasia!"
helloPetia();
// => "Hello, Petia!"

// передаем параметры
helloVasia('Good Luck!');
// => "Hello, Vasia! Good Luck!"
helloPetia('Good Luck!');
// => "Hello, Petia! Good Luck!"
```

## Function - call

Позоляет переопредлить контекст(this) функции и вызвает ее в новом контексте:

-   Очень похожа на [apply](#function---apply)

```js
let obj = {
	name: 'Vasia',
	hello: function (endText) {
		console.log(
			`Hello, ${this.name}! ${endText === undefined ? '' : endText}`
		);
	},
};

let anotherObj = {
	name: 'Petia',
};

let hello = obj.hello;

// передаем контекст(this)
hello();
// => "Hello, indefined!"
hello.call(obj);
// => "Hello, Vasia!"
hello.call(anotherObj);
// => "Hello, Petia!"

// передаем параметры
hello.call(obj, 'Good Luck!');
// => "Hello, Vasia! Good Luck!"
hello.call(anotherObj, 'Good Luck!');
// => "Hello, Petia! Good Luck!"
```

## Function - apply

Позоляет переопредлить контекст(this) функции и вызвает ее в новом контексте:

-   Очень похожа на [call](#function---call)

```js
let obj = {
	name: 'Vasia',
	hello: function (endText) {
		console.log(
			`Hello, ${this.name}! ${endText === undefined ? '' : endText}`
		);
	},
};

let anotherObj = {
	name: 'Petia',
};

let hello = obj.hello;

// передаем контекст(this)
hello();
// => "Hello, indefined!"
hello.apply(obj);
// => "Hello, Vasia!"
hello.apply(anotherObj);
// => "Hello, Petia!"

// передаем параметры
hello.apply(obj, ['Good Luck!']);
// => "Hello, Vasia! Good Luck!"
hello.apply(anotherObj, ['Good Luck!']);
// => "Hello, Petia! Good Luck!"
```

## Map - Инициализируем

Создаем коллекцию в которую потом добавим значения

```js
let map = new Map();
```

Инициализируем с заранее здаными значениями

```js
let map = new Map([
	['key1', 'value1'],
	['key2', 'value2'],
]);
```

## Map - Добавить элементы

```js
// создаем коллекцию
let map = new Map();

// добавляем элементы
map.set('first', 20);
map.set('second', { prop1: 10 });
map.set(['third', 'fourth'], 0);
```

## Map - Получаем значения

```js
map.get('second');
// => { prop1: 10 }
```

## Map - Проверяем наличие ключа

```js
map.has('first');
// => true
```

## Map - Удаляем

Удаляем элемент

```js
map.delete('first');
// => true
map.delete('undefined');
// => false
```

Удаляем все элементы

```js
map.clear();
```

## Map - Размер коллекции

```js
map.size;
// => 2
```

## Map - Получить ключи/значения

Получить ключи

```js
const map = new Map();

map.set('0', 'foo');
map.set(1, 'bar');

const iterator = map.keys();

iterator.next().value;
// => "0"
iterator.next().value;
// => "1"
```

Получить значения

```js
const map = new Map();

map.set('0', 'foo');
map.set(1, 'bar');

const iterator = map.values();

iterator.next().value;
// => "foo"
iterator.next().value;
// => "bar"
```

Получить пары ключ/значение

```js
const map = new Map();

map.set('0', 'foo');
map.set(1, 'bar');

const iterator = map.entries();

iterator.next().value;
// => ["0", "foo"]
iterator.next().value;
// => [1,   "bar"]
```

## Map - Иттерируемся по коллекции

Используем for..of

```js
for (const [key, value] of myMap) {
	console.log(`${key} = ${value}`);
}
```

Используем forEach

```js
myMap.forEach((value, key) => {
	console.log(`${key} = ${value}`);
});
```

## Map - Копируем коллекцию

```js
let map = new Map([
	[1, 100],
	[2, 0],
]);

let clone = new Map(map);
```

## Map - Объединяем коллекции

```js
let map1 = new Map([
	[1, 100],
	[2, 0],
]);

let map2 = new Map([
	[2, 100],
	[3, 0],
]);

let merge = new Map([...map1, ...map2, [4, 'ok']]);

console.log(merge);
// => Map(4) { 1 => 100, 2 => 100, 3 => 0, 4 => "ok" }
```

## SyntaxSugar - Double pipe

Возвращает значение слева, если это false, то тогда значение справа:

```js
function example(a) {
	a = a || 'Value';
	console.log(a);
}

example(10);
// => 10

example(undefined);
// => 'Value'
example(0);
// => 'Value'
example(false);
// => 'Value'
example('');
// => 'Value'
```

## SyntaxSugar - Double qestion

Возвращает значение слева, если это undefined, то тогда значение справа:

```js
function example(a) {
	a = a ?? 'Value';
	console.log(a);
}

example(10);
// => 10
example(0);
// => 0
example(false);
// => false
example('');
// => ''

example(undefined);
// => 'Value'
```

## Важные вещи - console.log

Вывод в консоль:

```js
console.log('Output data');
```

## Важные вещи - Комментарии

Комментарии:

```js
// Это коментарий однострочный

/* 
   Это коментарий 
   многострочный
*/
```
