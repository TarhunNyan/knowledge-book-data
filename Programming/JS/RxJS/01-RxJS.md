# JS - RxJS

Позволяет писать [рективные](../../Base/01-Base.md#реактивный-стиль) приложения

# Observable

Observable(наблюдатель) - объект который как-то получает данные а потом расскидывает по subscriber(подписчикам):

-   [Именование Observable](#observable---именование)

Создание Observable из данных:

-   [of - Создаем observable из данных](#observable---of)
-   [from - Создаем observable из объекта(array-like/itterator/Promise)](#observable---from)
-   [range - создает count чисел типа integer, начиная от start, через 1](#observable---range)

Работа со временем в Observable:

-   [interval - создает Observable, работающий с указанным интервалом](#observable---interval)
-   [timer - создает Observable со сдвигом во времени выполнения](#observable---timer)
-   [delay - задержка в отдаче данных Observable](#observable---delay)

Создание Observable из функций:

-   [bindCallback - создаем из функции Observable](#observable---bindcallback)
-   [bindNodeCallback - создаем из функции Observable, но с учетом возможности выброса ошибки](#observable---bindnodecallback)

Создание Observable из событий браузера:

-   [fromEvent - создает Observable, который отслеживает срабатывания события](#observable---fromevent)
-   [fromEventPattern - создает Observable, который умеет удалять события](#observable---fromeventpattern)

Объединяем Observable в последовательный Observable:

-   [concatMap - к каждому вызову применяет функцию, а потом соединяет в последовательный Observable](#observable---concatmap)
-   [concatAll - последовательно соединяет все Observable](#observable---concatmap)

Запрет запуска новых Observable пока не отработал старый:

-   [exhaustMap - пока прошлый запущенный Observable не отработал, не запускает новый](#observable---exhaustmap)
-   [exhaustAll - пока Observable запущенный прошлым не отработал, не запускает новый](#observable---exhaustall)

Объединяем Observable в параллельный Observable:

-   [merge - перечисленные Observable соединяет в параллельный](#observable---merge)
-   [mergeAll - Observable возвращаемые Observable, соединяет в параллельный](#observable---mergeall)
-   [mergeMap - к каждому значению применяет функцию, а потом соединяет в параллельный Observable](#observable---mergemap)

Scan - это reducer
mergeScan - делаем reduce, а потом merge

# Scheduler

# Примеры

## Observable - mergeScan

## Observable - mergeMap

mergeMap - к каждому значению применяет функцию, а потом соединяет в параллельный Observable:

-   запускает параллельно Observable

## Observable - mergeAll

mergeAll - Observable, которые возвращает Observable, запускает параллельно:

-   запускает параллельно Observable

```js
const clicks$ = fromEvent(document, 'click');
const higherOrder$ = clicks$.pipe(
    map(() => interval(1000)),
    mergeAll()
);

higherOrder$.subscribe((x) => console.log(x));
```

## Observable - merge

merge - объединяет указанные Observable в один, значения будут выдаваться в том же порядке, как если бы Observable былии зпущены паралельно:

-   последним аргуменотом, можно передать сколько Observable будут работать одновременно

```js
const timer1 = interval(1000).pipe(
    map((el) => 't1: ' + el),
    take(5)
);
const timer2 = interval(2000).pipe(
    map((el) => 't2: ' + el),
    take(2)
);
const timer3 = interval(500).pipe(
    map((el) => 't3: ' + el),
    take(5)
);

const merged = merge(timer1, timer2, timer3, timer1, 2);
merged.subscribe((x) => console.log(x));
// ( 1.0s) => t1: 0
// ( 2.0s) => t1: 1
// ( 2.0s) => t2: 0
// ( 3.0s) => t1: 2
// ( 4.0s) => t1: 3
// ( 4.0s) => t2: 1
// ( 4.0s - timer2 отработал запускаем timer3)
// ( 4.5s) => t3: 0
// ( 5.0s) => t1: 4
// ( 5.0s) => t3: 1
// ( 5.0s - timer1 отработал запускаем timer1)
// ( 5.5s) => t3: 2
// ( 6.0s) => t3: 3
// ( 6.5s) => t3: 3
// ( 6.0s) => t1: 0
// ( 6.5s) => t3: 4
// ( 7.0s) => t1: 1
// ( 8.0s) => t1: 2
// ( 9.0s) => t1: 3
// (10.0s) => t1: 4
```

## Observable - exhaustAll

exhaustAll - пока Observable запущенный прошлым не отработал, не запускает новый:

-   короче смотри пример

```js
const clicks$ = fromEvent(document, 'click');
const higherOrder$ = clicks$.pipe(
    map(() => interval(1000).pipe(take(5)))
);
const result$ = higherOrder$.pipe(exhaustAll());
result$.subscribe((x) => console.log(x));
// (кликнули)
// (задержка в секунду) => 0
// (задержка в секунду) => 1
// (задержка в секунду) => 2
// (кликнули)
// (задержка в секунду) => 3
//
// (кликнули)
// (задержка в секунду) => 0
// (задержка в секунду) => 1
// ...
```

## Observable - exhaustMap

exhaustMap - применяет map к Observable, и не даст запускать новые, пока не отработал предыдущий:

-   короче смотри пример

```js
const clicks$ = fromEvent(document, 'click');
const result$ = clicks.pipe(
    exhaustMap(() => interval(1000).pipe(take(5)))
);
result$.subscribe((x) => console.log(x));
// (кликнули)
// (задержка в секунду) => 0
// (задержка в секунду) => 1
// (задержка в секунду) => 2
// (кликнули)
// (задержка в секунду) => 3
//
// (кликнули)
// (задержка в секунду) => 0
// (задержка в секунду) => 1
// ...
```

## Observable - concatAll

concatAll - для Observable возвращающего другие Observable, concatAll соединит все Observable в один:

-   объединяет не в порядке возврата данных, а в порядке вернувшихся Observable(короче смотри пример)

```js
const clicks$ = fromEvent(document, 'click');
const higherOrder$ = clicks$.pipe(
    map(() => interval(1000).pipe(take(4)))
);
const firstOrder$ = higherOrder$.pipe(concatAll());
firstOrder.subscribe((x) => console.log(x));
// (кликнули)
// (задержка в секунду) => 0
// (задержка в секунду) => 1
// (кликнули)
// (задержка в секунду) => 2
// (задержка в секунду) => 3
// (задержка в секунду) => 0
// (задержка в секунду) => 1
// (задержка в секунду) => 2
// (задержка в секунду) => 3
```

## Observable - concat

concat - последовательно соединяет Observable:

```js
const timer$ = interval(1000).pipe(take(4));
const sequence$ = range(1, 10);
const concat$ = concat(timer$, sequence$);
concat$.subscribe((x) => console.log(x));
// (задержка в секунду) => 0
// (задержка в секунду) => 1
// (задержка в секунду) => 2
// (задержка в секунду) => 3
// => 1
// ...
// => 10
```

## Observable - concatMap

concatMap - на каждый вызов из Observable, создает новый Observable, результаты новых Observable объединяеются в один новый Observable:

```js
const clicks = fromEvent(document, 'click');
const result = clicks.pipe(
    concatMap((ev) => interval(1000).pipe(take(4)))
);
result.subscribe((x) => console.log(x));
```

## Observable - timer

timer - создает Observable со сдвигом во времени:

```js
console.log('START!');

const observable$ = interval(1000).pipe(take(6));
const shiftObservable$ = timer(3000).pipe(
    concatMap(() => observable$)
);

observable$.subscribe((el) => console.log(el));
shiftObservable$.subscribe((el) => console.log('shift: ' + el));

console.log('END!');
// => START!
// => END!
// (задержка в секунду) => 0
// (задержка в секунду) => 1
// (задержка в секунду) => 2
// (задержка в секунду) => 3
// (задержка в секунду) => shift: 0
// (задержка в секунду) => 4
// (задержка в секунду) => shift: 1
// (задержка в секунду) => 5
// (задержка в секунду) => shift: 2
// (задержка в секунду) => shift: 3
// (задержка в секунду) => shift: 4
// (задержка в секунду) => shift: 5
```

## Observable - delay

delay - задержка, перед отпрвкой данных в Subscriber:

-   вывод в примере происходит с задержкой в 2 секунды
-   может быть использован с указанием [Scheduler](#scheduler---методы-использующие-scheduler)

```js
const interval$ = of(1, 2, 3, 4, 5).pipe(delay(2000));
interval$.subscribe((val) => console.log(val));
// => 1
// => 2
// => 3
// => 4
// => ...
```

## Scheduler - Методы использующие Scheduler

Пример использования приведен с from, но есть многие другие методы принимающие Scheduler:

-   asyncScheduler
    -   отправка данных в Observable, передается в очередь event-loop браузера
    -   поэтому сначала отрабатывает последовательный код
    -   когда у браузера появляется свободное время, он смотрит свою очередь event-loop
-   asapScheduler
    -   отправка данных в Observable, передается в очередь event-loop браузера
    -   asapScheduler старается выполниться самым первым
    -   "рассталкивает" в очереди event-loop браузера всех, кроме других asapScheduler
    -   когда у браузера появляется свободное время, он смотрит свою очередь event-loop

```js
console.log('start');

const asyncObservable$ = from([1, 2, 3], asyncScheduler).pipe(
    map((val) => 'async-' + val)
);
asyncObservable$.subscribe((x) => console.log(x));
const asapObservable$ = from([1, 2, 3], asapScheduler).pipe(
    map((val) => 'asap-' + val)
);
asapObservable$.subscribe((x) => console.log(x));

console.log('end');

// => start
// => end
// => asap-1
// => asap-2
// => asap-3
// => async-1
// => async-2
// => async-3
```

## Observable - interval

interval - счетчик, который возвращает значение через указанное время:

```js
const interval$ = interval(1000);
interval$.subscribe((val) => console.log(val));
// => 1
// => 2
// => 3
// => 4
// => ...
```

## Observable - bindCallback

bindCallback - оборачивает переданную функцию, и дает возможность создать из нее Observable:

-   someFunction - функция из которой сделаем Observable
    -   функция должна принимать последним аргументом callback
-   boundSomeFunction - функция внутри специальной обертки
-   boundSomeFunction(5, 'some string') - создаем observable с переданными параметрами
-   boundSomeFunction(5, 'some string').subscribe(...) - передаем callback, последним аргументов функции
    -   subscribe способен принимать только один параметр
    -   передав несколько параметров в Callback, они соберуться в один массив
-   способна принимать Scheduler

```js
const someFunction = (x, y, cb) => {
    cb(x, y, { someProperty: 'someValue' });
};

const boundSomeFunction = bindCallback(someFunction);
boundSomeFunction(5, 'some string').subscribe((values) => {
    console.log(values); // [5, 'some string', {someProperty: 'someValue'}]
});
```

## Observable - bindNodeCallback

bindNodeCallback - оборачивает переданную функцию, и дает возможность создать из нее Observable:

-   [работает как bindCallback](#observable---bindcallback)
    -   исключение составляет то, что первый параметр аргумент callback это ошибка
    -   если ошибки нет, надо передавать первым параметром null

```js
const someFunction = (x, y, cb) => {
    if (x === 0) {
        cb('error');
    }
    cb(null, x, y);
};

const boundSomeFunction = bindCallback(someFunction);
boundSomeFunction(5, 'string').subscribe((values) => {
    console.log(values);
});
// => [5, 'string']

boundSomeFunction(0, 'string').subscribe((values) => {
    console.log(values);
});
// => error
// Ошибка которую неплохо бы обработать
```

## Observable - Именование

Именование - принято именовать Observable со знаком доллара в конце:

-   observableName$ - имя для Observable

```js
const observableName$ = new Observable((subscriber) => {});
```

## Observable - of

of - создание Observable из каких-то данных:

```js
const observable$ = of(10, 20, 30);
```

## Observable - from

from - создание Observable из различных объектов:

-   объект из которых можно создать Observable
    -   array-like
    -   itterable
    -   Promise
-   может быть использован с указанием [Scheduler](#scheduler---методы-использующие-scheduler)

```js
const observable$ = from([10, 20, 30]);

const iterator = new generate(3);
const observable2$ = from(iterator);
```

## Observable - range

range - создает count чисел типа integer, начиная от start, через 1:

-   может быть использован с указанием [Scheduler](#scheduler---методы-использующие-scheduler)

```js
const numbers = range(10, 3);
// => 10
// => 11
// => 12
```

## Observable - fromEvent

fromEvent - создает Observable из события:

-   document - элемент DOM дерева
-   'click' - событие которое ждем

```js
const clicks = fromEvent(document, 'click');
```

## Observable - fromEventPattern

fromEventPattern - создает Observable из события, а также добавляет способ удаления собыития, в случае отписки от Observable:

-   addClickHandler - добавляем событие на элемент при появлении подписчика у Observable
-   removeClickHandler - удаляем событие на элементе при потписке от Observable
-   document - элемент DOM дерева
-   'click' - событие которое ждем

```js
function addClickHandler(handler) {
    document.addEventListener('click', handler);
}

function removeClickHandler(handler) {
    document.removeEventListener('click', handler);
}

const observable$ = fromEventPattern(
    addClickHandler,
    removeClickHandler
);
observable$.subscribe((x) => console.log(x));
```

####

## Stream

Stream - высокоуровневая обертка над асинхронными потоками. Позволяет работать с данными в удобном, функциональном стиле

-   в RxJS принято именовать Stream со знаком доллара в конце

Функции создающие потоки:

-   [Интервал времени](#stream---interval)
-   [Stream из набора данных](#stream---of)
-   [Stream из числового интервала](#stream---range)
-   [Stream из массива](#stream---from)
-   [Stream из js event](#stream---fromEvent)

Создать кастомный поток:

-   [Кастомный поток - который сами будем вызывать(Subject)](#stream---subject)
-   [Кастомный поток с начальным значением(BehaviourSubject)](#stream---behaviorsubject)
-   [Кастомный поток сохраняющий предыдущие значения(ReplaySubject)](#stream---replaysubject)

## Pipe

У любого stream есть метод pipe. Это классический PipeOperator, то есть передает результат слева в функцию справа. Он принимает набор функций через которые проходят данные

Базовые функции которые может использовать pipe:

-   [Ограничивает число вызовов stream указанным числом](#pipe---take)
-   [Оставляет только подходящее условию](#pipe---filter)
-   [Преобразуем массив](#pipe---map)
-   [Добавляет значения в массив](#pipe---scan)

## Subscribe

Subscribe - позволяет подписываться на stream, то есть получать результат работы stream когда это необходимо:

```js
let variable$ = interval(1000).pipe( ... );

let subscribe = variable$.subscribe((res) => display.textContent);
```

Вообще subscribe принимает три Callback:

-   next - что делать с каждым следующим элементом stream
-   error - callback на случай ошибки
-   complete - callback на случай завершения Stream

```js
let variable$ = interval(1000).pipe( ... )
	.subscribe(
		// next
		(res) => display.textContent,
		// error
		null,
		// complete
		null
	);
```

## Unsubscribe

Unsubscribe - отписаться от stream:

-   если все отписались от Stream, то он перестает работать

```js
const stream = of(1, 2, 3, 4).subscibe((v) => console.log(v));

stream.unsubscribe();
```

# Примеры

## Pipe - take

Ограничевает количество иттераций stream указанным числом:

-   по достижению числа stream перестает существовать

```js
import interval from 'rxjs';
import { filter, map } from 'rxjs/operators';

let variable$ = interval(1000).pipe(
    take(people.length),
    filter((v) => people[v].age >= 18),
    map((v) => people[v].name),
    scan((acc, v) => acc.concat(v), [])
);
```

## Pipe - filter

Проверяет, подходит ли значение условию:

-   если значение подходит условию, то прокидывает его дальше

```js
import interval from 'rxjs';
import { filter, map } from 'rxjs/operators';

let variable$ = interval(1000).pipe(
    take(people.length),
    filter((v) => people[v].age >= 18),
    map((v) => people[v].name),
    scan((acc, v) => acc.concat(v), [])
);
```

## Pipe - map

Применяет функцию к каждому полученному значению:

```js
import interval from 'rxjs';
import { filter, map } from 'rxjs/operators';

let variable$ = interval(1000).pipe(
    take(people.length),
    filter((v) => people[v].age >= 18),
    map((v) => people[v].name),
    scan((acc, v) => acc.concat(v), [])
);
```

## Pipe - scan

Добавляет значения полученные на каждой иттерации stream в один массив:

```js
import interval from 'rxjs';
import { filter, map } from 'rxjs/operators';

let variable$ = interval(1000).pipe(
    take(people.length),
    filter((v) => people[v].age >= 18),
    map((v) => people[v].name),
    scan((acc, v) => acc.concat(v), [])
);
```

## Stream - interval

interval - функция создает stream который срабатывает каждые n миллисекунд:

-   передает в pipe число, которое растет каждой иттерацией stream: 1, 2, 3, ...

```js
import interval from 'rxjs';

let variable$ = interval(1000).pipe();
```

## Stream - of

Создать Stream из набора данных:

```js
const stream$ = of(1, 2, 3, 4);
```

## Stream - range

Создать Stream из диапозона чисел:

-   42 - начальное значение
-   10 - число шагов

```js
const stream$ = range(42, 10);
```

## Stream - from

Создать Stream из массива:

```js
const stream$ = from([1, 2, 3, 4, 5, 6, 7, 8]);
```

## Stream - fromEvent

Создать Stream из событий JS:

-   document.querySelector('canvas') - это DOM Node которую будем прослушивать
-   'mousemove' - событие которое пихаем в Stream

```js
const stream$ = fromEvent(
    document.querySelector('canvas'),
    'mousemove'
);
```

## Stream - Subject

Subject - класс, который позволяет на него подписаться и отправлять данные всем подписчикам:

-   stream$.subscribe(...) - подписка на stream
-   stream$.next(...) - отправка данных всем подписчикам

```js
import { Subject } from 'rxjs';

const stream$ = new Subject();
stream$.subscribe((v) => console.log(v));
stream$.next('First Value');
// => First Value
stream$.next('Second Value');
// => Second Value
```

## Stream - BehaviorSubject

BehaviorSubject - класс, который позволяет на него подписаться и отправлять данные всем подписчикам, имеет хранимое значение(\_value):

-   'Default' - значение \_value, заданное при инициализации
-   stream$.subscribe(...) - подписка на stream
-   stream$.next(...) - каждый вызов next, не только передает данные но и меняет \_value на новое

```js
import { BehanioutSubject } from 'rxjs';

const stream$ = new BehanioutSubject('Default');
stream$.subscribe((v) => console.log(v));
// => Default
stream$.next('First Value');
// => First Value
stream$.next('Second Value');
// => Second Value

stream$.subscribe((v) => console.log(v));
// => Second Value
```

## Stream - ReplaySubject

ReplaySubject - класс, который позволяет на него подписаться и отправлять данные всем подписчикам:

-   запоминает отправленные значения
-   так же при инициалищации может принимать число, которое говорит сколько сообщений запоминать
-   stream$.subscribe(...) - подписка на stream
-   stream$.next(...) - отправка данных всем подписчикам

```js
import { ReplaySubject } from 'rxjs';

const stream$ = new ReplaySubject();
stream$.next('First Value');
stream$.next('Second Value');
stream$.subscribe((v) => console.log(v));
// => First Value
// => Second Value
```
