# JS - RxJS

Позволяет писать [рективные](../../Base/01-Base.md#реактивный-стиль) приложения

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

-   по достижению числа stream перестает сществовать

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
const stream$ = fromEvent(document.querySelector('canvas'), 'mousemove');
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
