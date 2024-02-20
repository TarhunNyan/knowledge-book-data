# JS - RxJS

Позволяет писать [рективные](../../Base/01-Base.md#реактивный-стиль) приложения

# Основные понятия

Есть несоколько основных классов/понятий:

-   Observable - объект который как-то получает данные а потом отправляет их всем subscriber(подписчикам)
    -   pipe - преобразования данных, перед тем как они попадут в Subscriber
-   Subscriber - терминальный объект, который описывает что делаем в конце с данными
-   Subject - выступает и в роли Observable и в роли Subscriber

# Observable

Observable(наблюдатель) - объект который как-то получает данные а потом отправляет их всем subscriber(подписчикам):

-   [Именование Observable](#observable---именование)

Создание Observable из данных:

-   [of - Создаем observable из данных](#observable---of)
-   [from - Создаем observable из объекта(array-like/itterator/Promise)](#observable---from)
-   [range - создает count чисел типа integer, начиная от start, через 1](#observable---range)

Работа со временем в Observable:

-   [interval - создает Observable, работающий с указанным интервалом](#observable---interval)
-   [timer - создает Observable со сдвигом во времени выполнения](#observable---timer)
-   [delay - задержка в отдаче данных Observable](#observable---delay)
-   [delayWhen - задержка в отдаче данных Observable, пока не сработает другой Observable](#observable---delayWhen)

Создание Observable только в момент подписки:

-   [defer - отложенная инициализация Observable, до момента подписки](#observable---defer)

Создание Observable из функций:

-   [bindCallback - создаем из функции Observable](#observable---bindcallback)
-   [bindNodeCallback - создаем из функции Observable, но с учетом возможности выброса ошибки](#observable---bindnodecallback)

Создание Observable из событий браузера:

-   [fromEvent - создает Observable, который отслеживает срабатывания события](#observable---fromevent)
-   [fromEventPattern - создает Observable, который умеет удалять события](#observable---fromeventpattern)

Объединяем Observable в последовательный Observable:

-   [concatMap - к каждому вызову применяет функцию, а потом соединяет в последовательный Observable](#observable---concatmap)
-   [concatAll - последовательно соединяет все Observable](#observable---concatall)

Запрет запуска новых Observable пока не отработал старый:

-   [exhaustMap - пока прошлый запущенный Observable не отработал, не запускает новый](#observable---exhaustmap)
-   [exhaustAll - пока Observable запущенный прошлым не отработал, не запускает новый](#observable---exhaustall)
-   [raceWith - пропускает значения только из потока, который первый выдал какой-то результат](#observable---racewith)

    Объединяем Observable в параллельный Observable:

-   [merge - перечисленные Observable соединяет в параллельный](#observable---merge)
-   [mergeAll - Observable возвращаемые Observable, соединяет в параллельный](#observable---mergeall)
-   [mergeMap - к каждому значению применяет функцию, а потом соединяет в параллельный Observable](#observable---mergemap)

Scan - работает как reducer, только возвращает значение для каждого входного, а не только финальное:

-   [scan - выполняем reduce для каждого значения](#observable---scan)
-   [mergeScan - производим reduce, потом каждый результат merge](#observable---mergescan)
-   [switchScan - scan для функций возвращающих Observable](#observable---switchscan)

Сортровка:

-   [distinct - возвращает только неповторяющиеся значения](#observable---distinct)
-   [distinctUntilKeyChanged - из последовательно идущих объектов, у которых значения по ключу одинаковы, вернет только первый](#observable---distinctuntilkeychanged)
-   [distinctUntilChanged - из последовательно идущих равных значений, вернет только первый](#observable---distinctuntilchanged)

Reducer - классический reducer:

-   [reduce - при завершении Observable проходится по всему что в нем есть](#observable---reduce)

Рексурсия:

-   [expand - создает Observable который рекурсивно применяет функцию](#observable---expand)

Count:

-   [count - при завершении Observable считает количество элементов](#observable---count)
-   [min/max - при завершении Observable находит минимальный/максимальный элемент](#observable---minmax)

Take:

-   [elementAt - создает из элемента Observable под указанным номером новый Observable](#observable---elementat)
-   [first/last - по завершению работы Observable вернет первое/последние значение(или первое подходящее по условию)](#observable---firstlast)
-   [take/skip - после первых n значений завершает-Observable/перестает-skip-элементы](#observable---takeskip)
-   [takeLast/skipLast - по завершению работы Observable вернет-последние-n/пропустит-последние-n/вернет-последние-значение](#observable---takelastskiplast)
-   [takeUntil/skipUntil - когда другой Observable вернет результат, завершает-Observable/перестает-skip-элементы](#observable---takeUntilskipUntil)
-   [takeWhile/skipWhile - когда получит первый элемент не соответствующий условию, завершает-Observable/перестает-skip-элементы](#observable---takeWhileskipWhile)

Zip, Combine:

-   [zip - функция которая производит zip из набора Observable в новый Observable](#observable---zip)
-   [zipSwitch - pipe, который текущий Observable и переданные Observable делает zip в новый Observable](#observable---zipwith)
-   [zipAll - pipe, который набор Observable возвращаемых Observable делает zip в новый Observable](#observable---zipall)
-   [combineLatest/combineLatestAll/combineLatestWith - комбинирует набор Observable в один, который возвращает массив из последних значений набора Observable](#observable---combinelatestcombinelatestallcombinelatestwith)
-   [combineLatestAll - комбинирует набор Observable в один, который возвращает массив из последних значений набора Observable](#observable---combinelatestall)

Skip:

-   [skip - пропускает первые n значений](#observable---skip)

Buffer:

-   [buffer - собирает в массив значения Observable, когда срабатывает другой Observable](#observable---buffer)
-   [bufferCount - собирает в массив значения Observable, когда число новых элементов достигает bufferSize](#observable---buffercount)
-   [bufferTime - собирает в массив значения Observable, и возвращает их каждые n милисекунд](#observable---buffertime)
-   [bufferToggle - собирает в массив значения Observable, которые пришли в промежутке между двумя Observable](#observable---buffertoggle)
-   [bufferWhen - собирает в массив значения Observable, возвращает когла срабатал другой Observable](#observable---bufferwhen)
-   [pairwise - собирает элементы в пары вида: 1, 2, 3, 4, ... -> [1,2], [2,3], [3,4]...]()

## Observable - pairwise

pairwise - собирает элементы в пары вида: 1, 2, 3, 4, ... -> [1,2], [2,3], [3,4]...:

-   pairwise()

Пояснение к примеру:

-   начиная со второго клика будет показывать расстояние между точками

```js
fromEvent(document, 'click')
    .pipe(pairwise())
    .pipe(
        map(([first, second]) => {
            const x0 = first.clientX;
            const y0 = first.clientY;
            const x1 = second.clientX;
            const y1 = second.clientY;
            return Math.sqrt(
                Math.pow(x0 - x1, 2) + Math.pow(y0 - y1, 2)
            );
        })
    )
    .subscribe(console.log);
```

Switch:

-   [switchMap - позволяет запустить из значения Observable, новый Observable, до тех пока не придет новое значение](#observable---switchmap)

Audit:

-   [sample - возвращает последние значение Observable, находящееся между двумя срабатывания другого Observable](#observable---sample)
-   [sampleTime - возвращает последние значение Observable, попавшие в указанный временной период](#observable---sampletime)
-   [audit - возврщает последнее значение, при срабатывании другого Observable](#observable---audit)
-   [auditTimer - возвращает каждые n милилисекунд последнее значение](#observable---audittime)
-   [debounce - возвращает последний элемент из серии элементов(есть зависимость от другого Observable)](#observable---debounce)
-   [debounceTimer - если n-миллисекунд не вызывается Observable. то возвращает последний элемент](#observable---debouncetimer)

Catch:

-   [catchError - меняет поведение Observable после пойманной ошибки](#observable---catcherror)

Константы:

-   [EMPTY - возвращает пустой Observable который ничего не возвращается и сразу заканчивает работу](#observable---empty)
-   [defaultIfEmpty - возвращает Observable, который вернет переданное значение и сразу заканчивает работу](#observable---defaultempty)
-   [ignoreElements - после этого pipe у subscribe работают только методы complete и error](#observable---ignoreelements)

Repeat - не ловит ошибки/Retry - ловит ошибки:

-   [retry - перезапускает Observable несколько раз, когда тот завершается](#observable---retry)
-   [repeat - перезапускает Observable несколько раз](#observable---repeat)

tap:

-   [tap - добавляет side эффект](#observable---tap)

connect:

-   [connect - превращает Observable в несколько Observable, в пределах pipe(нужен для синхронных Observable)](#observable---connect)
-   [connectable - превращает Observable в несколько Observable, за пределами pipe(нужен для синхронных Observable)](#observable---connectable)

Начало/окончание Observable:

-   [startWith/endWith - то какие элементы будут отправлены при старте/окончании работы Observable](#observable---startwithendwith)
-   [finalize - выполняет callback перед завершением работы Observable](#observable---finalize)

Функциональщина:

-   [map - применяет функцию к каждому значению Observable](#observable---map)
-   [every - если все элементы Observable по окончанию его работы возвращают true, то и Observable возвращает true](#observable---every)
-   [filter - оставляет только те значения Observable, которые прошли проверку](#observable---filter)
-   [find - возвращает первое значение, проходящее условие](#observable---find)
-   [findIndex - возвращает индекс первого значения, проходящего условие](#observable---findIndex)

Разделение Observable:

-   [groupBy - разделяет Observable на несколько](#observable---groupby)

isEmpty:

-   [isEmpty - если Observable выдало значение, то isEmpty сразу вернет false, если Observable сразу завершился isEmpty и возвращает true](#observable---isempty)

# Горячие и холодные Observable

Холодный Observable:

-   данные формируются в кострукторе
-   источник данных для каждого подписчика свой, а значит, и значения тоже свои

Горячий Observable:

-   источник данных создается снаружи конструктора observable
-   данные могут меняться, даже если нет подписчиков
-   поток делится пришедшими ему данными со всеми своими подписчиками

# Multicast операторы

# БЕЗ ПОНЯТИЯ

-materialize и dematerialize

-   observeOn - совсем бесполезно. Не меняет планировщик Observable, а просто планирует пропуск значений дальше, новым Scheduler

# Observable - Constant

-   EMPTY - возвращает пустой Observable который ничего не возвращается и сразу заканчивает работу
-   defaultIfEmpty(42) - возвращает Observable, который вернет переданное значение(42) и сразу заканчивает работу

# Observable - Способные завершать Observable

-   take
-   takeUntil
-   takeWhile

# Observable - Функции работающие по окончанию Observable

-   reduce
-   count
-   max
-   min
-   takeLast
-   finilize
-   endWith
-   isEmpty

# Scheduler

# Примеры

## Observable - sequenceEqual

sequenceEqual - проверяет, эквивалентны ли два Observable:

-   sequenceEqual(compareTo, comparator)
    -   compareTo - параметр в котороый кладем Observable, с которым сравниваем
    -   comparator(a, b) - функция в которой мы описываем как сравнивать элементы из Observable

Пояснение к примеру:

-   первая часть примера - два эквивалентных Observable
-   вторая часть примера - два НЕ эквивалентных Observable
-   тертья часть примера - в t2 элементов меньше чем в t1, поэтому на 4 элементе t1 наш sequenceEqual закончится и вернет false

```js
const t1 = interval(300).pipe(take(5));
const t2 = interval(50).pipe(take(3), endWith(3, 4));
t1.pipe(sequenceEqual(t2)).subscribe(console.log);
// => true

const t1 = interval(300).pipe(take(5));
const t2 = interval(50).pipe(take(3), endWith(100, 200));
t1.pipe(sequenceEqual(t2)).subscribe(console.log);
// => false

const t1 = interval(300).pipe(take(5));
const t2 = interval(50).pipe(take(3));
t1.pipe(sequenceEqual(t2)).subscribe(console.log);
// => false
```

## Observable - isEmpty

isEmpty - если Observable выдало значение, то isEmpty сразу вернет false, если Observable сразу завершился isEmpty и возвращает true:

-   isEmpty()

```js
const source = new Subject();

source.subscribe(console.log);
source
    .pipe(isEmpty())
    .subscribe((x) => console.log('IS EMPTY: ' + x));

source.next('a');
source.next('b');
source.next('c');
source.complete();

// => a
// => IS EMPTY: false
// => b
// => c
```

## Observable - ignoreElements

ignoreElements - после этого pipe у subscribe работают только методы complete и error:

-   ignoreElements()

```js
of('you', 'talking', 'to', 'me')
    .pipe(ignoreElements())
    .subscribe({
        next: (word) => console.log(word),
        error: (err) => console.log('error:', err),
        complete: () => console.log('the end'),
    });
// => the end
```

## Observable - raceWith

raceWith - пропускает значения только из потока, который первый выдал какой-то результат:

-   raceWith(...otherSources)
    -   ...otherSources - набор Observable(плюс тот у которого срабатывает pipe), из которых выбирается тот, значения которго отправляем дальше

Пояснение к примеру:

-   запускается три потока
-   собирает их в raceWith
-   поскольку obs2 отработал первым, теперь будут пропускаться данные только из него

```js
const obs1 = interval(7000).pipe(map(() => 'Slow'));
const obs2 = interval(1000).pipe(map(() => 'Fast'));
const obs3 = interval(3000).pipe(map(() => 'Medium'));

obs1.pipe(raceWith(obs2, obs3)).subscribe(console.log);
// => 'Fast'
// => 'Fast'
// => 'Fast'
// ...
```

## Observable - groupBy

groupBy - разделяет Observable на несколько:

-   gpoupBy(keySelector, elementOrOptions, duration, connector)
    -   keySelector(value) - функция, которая принимает значение, а возвращает какоие-то данные, по которым произойдет группировка
    -   elementOrOptions - опционально
    -   duration - опционально
    -   connector - опционально

```js
of(
    { id: 1, name: 'JavaScript' },
    { id: 2, name: 'Parcel' },
    { id: 2, name: 'webpack' },
    { id: 1, name: 'TypeScript' },
    { id: 3, name: 'TSLint' }
)
    .pipe(
        groupBy((p) => p.id),
        mergeMap((group$) =>
            group$.pipe(reduce((acc, cur) => [...acc, cur], []))
        )
    )
    .subscribe(console.log);
// => [{ id: 1, name: 'JavaScript' }, { id: 1, name: 'TypeScript'}]
// => [{ id: 2, name: 'Parcel' }, { id: 2, name: 'webpack'}]
// => [{ id: 3, name: 'TSLint' }]
```

## Observable - findIndex

findIndex - возвращает индекс первого значения, проходящего условие:

-   findIndex(predicate, thisArg)
    -   predicate(value, index, source) - функция возвращает булевый результат проверки значения Observable
        -   value - значение Observable
        -   index - номер значения Observable
        -   source - это Observable который мы и запускали(нафига он нужон, непонятно...)
    -   thisArgs - опциональный аргумент, это объект который будет являться this для функции переданной в Project

```js
of(10, 20, 30, 40, 50, 60)
    .pipe(findIndex((value) => value > 30))
    .subscribe(console.log);
// => 3
```

## Observable - find

find - возвращает первое значение, проходящее условие:

-   find(predicate, thisArg)
    -   predicate(value, index, source) - функция возвращает булевый результат проверки значения Observable
        -   value - значение Observable
        -   index - номер значения Observable
        -   source - это Observable который мы и запускали(нафига он нужон, непонятно...)
    -   thisArgs - опциональный аргумент, это объект который будет являться this для функции переданной в Project

```js
of(1, 2, 3, 4, 5, 6)
    .pipe(find((value) => value > 3))
    .subscribe(console.log);
// => 4
```

## Observable - finalize

finalize - выполняет callback перед завершением работы Observable:

-   finalize(callback)
    -   callback() - функция которая ничего не принимает и ничего не возвращает

```js
interval(1000)
    .pipe(
        take(3),
        finalize(() => console.log('Sequence complete'))
    )
    .subscribe(console.log);

// 0
// 1
// 2
// 'Sequence complete'
```

## Observable - filter

filter - оставляет только те значения Observable, которые прошли проверку:

-   filter(predicate, thisArg)
    -   predicate(value, index) - функция возвращает булевый результат проверки значения Observable
        -   value - значение Observable
        -   index - номер значения Observable
    -   thisArgs - опциональный аргумент, это объект который будет являться this для функции переданной в Project

```js
of(1, 2, 3, 4, 5)
    .pipe(filter((el, index) => el % 2 === 1))
    .subscribe(console.log);
// => 1
// => 3
// => 5
```

## Observable - startWith/endWith

startWith/endWith - то какие элементы будут отправлены при старте/окончании работы Observable:

-   startWith(...values)/endWith(...values)
    -   ...values - параметры функции, которые будут отправлены при старте/окончании работы Observable

```js
of(1, 2, 3)
    .pipe(startWith('a', 'heh'), endWith('nooo', 'b'))
    .subscribe(console.log);
// => a
// => heh
// => 1
// => 2
// => 3
// => nooo
// => b
```

## Observable - every

every - если все элементы Observable по окончанию его работы возвращают true, то и Observable возвращает true:

-   every(predicate, thisArg)
    -   predicate(value, index, source) - функция возвращает булевый результат проверки значения Observable
        -   value - значение Observable
        -   index - номер значения Observable
        -   source - это Observable который мы и запускали(нафига он нужон, непонятно...)
    -   thisArgs - опциональный аргумент, это объект который будет являться this для функции переданной в Project

```js
of(10, 20, 30, 40)
    .pipe(map((el, index) => index))
    .subscribe(console.log);
```

## Observable - map

map - применяет функцию к каждому значению Observable:

-   map(project, thisArg)
    -   project(value, index) - функция, которая трансформирует значения Observable
        -   value - значение Observable
        -   index - номер значения Observable
    -   thisArgs - опциональный аргумент, это объект который будет являться this для функции переданной в Project

```js
of(10, 20, 30, 40)
    .pipe(map((el, index) => index))
    .subscribe(console.log);
```

## Observable - elementAt

elementAt - создает из элемента Observable под указанным номером новый Observable:

```js
fromEvent(document, 'click')
    .pipe(
        map((val, index) => index),
        elementAt(2)
    )
    .subscribe(console.log);
// (click) =>
// (click) =>
// (click) => 2
// (click) =>
// ...
```

## Observable - distinctUntilKeyChanged

distinctUntilKeyChanged - из последовательно идущих объектов, у которых значения по ключу одинаковы, вернет только первый

-   distinctUntilKeyChanged(key, compare)
    -   key - строка, с названием параметра
    -   compare(x, y) - опционально, функция возвращает булевый результат, равны ли значения, выдаваемые Observable
        -   x - значение поля предыдущего объекта
        -   y - значение поля текущего объекта

```js
const values = [
    { v: 1 },
    { v: 1 },
    { v: 2 },
    { v: 2 },
    { v: 2, s: 100 },
    { v: 1 },
    { v: 2, n: 20 },
    { v: 3 },
    { v: 4 },
    { v: 3 },
    { v: 2 },
    { v: 1 },
];
interval(1000)
    .pipe(
        take(values.length),
        map((value) => values[value]),
        distinctUntilKeyChanged('v', (prev, curr) => {
            return prev === curr;
        })
    )
    .subscribe(console.log);
// ( 1.0s) => {v: 1}
// ( 2.0s) =>
// ( 3.0s) => {v: 2}
// ( 4.0s) =>
// ( 5.0s) =>
// ( 6.0s) => {v: 1}
// ( 7.0s) => {v: 2, n: 20}
// ( 8.0s) => {v: 3}
// ( 9.0s) => {v: 4}
// (10.0s) => {v: 3}
// (11.0s) => {v: 2}
// (12.0s) => {v: 1}
```

## Observable - distinctUntilChanged

distinctUntilChanged - из последовательно идущих равных значений, вернет только одно:

-   distinctUntilChanged(comporator, keySelector)
    -   comparator(previous, current) - опционально, функция возвращает булевый результат, равны ли значения, выдаваемые Observable
    -   keySelector(value) - функция, которая вовзращает значения, по которым мы проверям повторы

Пояснение к примеру:

-   выводит значения values каждую секунду, где
    -   prev.v != curr.v
    -   или есть заполненное поле s или n

```js
const values = [
    { v: 1 },
    { v: 1 },
    { v: 2 },
    { v: 2 },
    { v: 2, s: 100 },
    { v: 1 },
    { v: 2, n: 20 },
    { v: 3 },
    { v: 4 },
    { v: 3 },
    { v: 2 },
    { v: 1 },
];
interval(1000)
    .pipe(
        take(values.length),
        map((value) => values[value]),
        distinctUntilChanged((prev, curr) => {
            return !(
                prev.v != curr.v ||
                !(curr.n === undefined) ||
                !(curr.s === undefined)
            );
        })
    )
    .subscribe(console.log);

// ( 1.0s) => {v: 1}
// ( 2.0s) =>
// ( 3.0s) => {v: 2}
// ( 4.0s) =>
// ( 5.0s) => {v: 2, s: 100}
// ( 6.0s) => {v: 1}
// ( 7.0s) => {v: 2, n: 20}
// ( 8.0s) => {v: 3}
// ( 9.0s) => {v: 4}
// (10.0s) => {v: 3}
// (11.0s) => {v: 2}
// (12.0s) => {v: 1}
```

## Observable - distinct

distinct - возвращает неповторяющиеся значения:

-   distinct(keySelector)
    -   keySelector(value) - функция, которая вовзращает значения, по которым мы проверям повторы
    -   flushes - опционально, можно передать Observable, при срабатывании которого будет происходить очистка значений, которые уже считаются повторами

```js
const values = [
    { v: 1 },
    { v: 1 },
    { v: 2, s: 100 },
    { v: 2 },
    { v: 2 },
    { v: 1 },
    { v: 2, n: 20 },
    { v: 3 },
    { v: 4 },
    { v: 3 },
    { v: 2 },
    { v: 1 },
];
interval(1000)
    .pipe(
        take(values.length),
        map((value) => values[value]),
        distinct((value) => value.v, interval(8000))
    )
    .subscribe(console.log);

// ( 1.0s) => {v: 1}
// ( 2.0s) =>
// ( 3.0s) => {v: 2, s: 100}
// ( 4.0s) =>
// ( 5.0s) =>
// ( 6.0s) =>
// ( 7.0s) => { v: 2, n: 20 }
// ( 8.0s) => {v: 3}
// ( 9.0s) => {v: 4}
// (10.0s) => {v: 3}
// (11.0s) => {v: 2}
// (12.0s) => {v: 1}
```

## Observable - defer

defer - отложенная инициализация Observable, до момента подписки:

-   defer(observableFactory)
    -   observableFactory() - метод, который вызывается в момент подписки и возвращает Observable

```js
const clicksOrInterval = defer(() => {
    return Math.random() > 0.5
        ? fromEvent(document, 'click')
        : interval(1000);
});

clicksOrInterval.subscribe(console.log);
```

## Observable - debounceTimer

debounceTimer - если n-миллисекунд не вызывается Observable. то возвращает последний элемент:

-   debounceTime(dueTime, scheduler)
    -   dueTime - время в миллисекундах
    -   scheduler - [Scheduler](#scheduler---методы-использующие-scheduler)
-   работает следующим образом
    -   при каждом срабатывании Observable, перезапускает таймер
    -   после окончания работы таймера, вернет последние значение Observable

```js
fromEvent(document, 'click').pipe(debounceTime(1000));
result.subscribe(console.log);
```

## Observable - debounce

debounce - возвращает последний элемент из серии элементов(есть зависимость от другого Observable):

-   debounce(durationSelector)
    -   durationSelector(value) - функция которая принимает значение и выдает Observable
-   работает следующим образом
    -   при каждом срабатывании Observable, запускает другой Observable
    -   после получения значения от другого Observable, вернет последние значение первичного Observable

Пояснение к примеру:

-   вернет последний клик из серии кликов

```js
fromEvent(document, 'click').pipe(debounce((i) => interval(200 * i)));
result.subscribe(console.log);
```

## Observable - connectable

connect - превращает Observable в несколько Observable, в пределах pipe:

-   connect(selector, config)
    -   selector(shared) - функция принимающая Observable и вовзращающая Observable
    -   config - опциональный параметр, хз что делает
-   наверное возник вопрос, а почему не сунуть Observable в переменную и не сделать несколько подписок ниже?
    -   ответ: если Observable синхронный, то в случае с переменной отработает первый, а следующие два ничего не сделают
    -   ответ: если Observable синхронный, то с Connect мы создадим 3 подписки, и ВСЕ три отработают как надо

Пояснение к примеру:

-   мы создаем Observable из 1, 2, 3, 4, 5
-   накладываем side эффект не меняющий значения, который выводит: "source emitted ${ n }"
-   после в коде, создаем организуем несколько подписок
-   когда вызываем connect, начинают работать подписки

```js
const connectableObservable = connectable(
    of(1, 2, 3, 4, 5).pipe(
        tap({
            subscribe: () => console.log('subscription started'),
            next: (n) => console.log(`source emitted ${n}`),
        })
    )
);

connectableObservable
    .pipe(map((n) => `all ${n}`))
    .subscribe(console.log);
connectableObservable
    .pipe(
        filter((n) => n % 2 === 0),
        map((n) => `even ${n}`)
    )
    .subscribe(console.log);
connectableObservable
    .pipe(
        filter((n) => n % 2 === 1),
        map((n) => `odd ${n}`)
    )
    .subscribe(console.log);

connectableObservable.connect();
```

## Observable - connect

connect - превращает Observable в несколько Observable, в пределах pipe:

-   connect(selector, config)
    -   selector(shared) - функция принимающая Observable и вовзращающая Observable
    -   config - опциональный параметр, хз что делает
-   наверное возник вопрос, а почему не сунуть Observable в переменную и не сделать несколько подписок ниже?
    -   ответ: если Observable синхронный, то в случае с переменной отработает первый, а следующие два ничего не сделают
    -   ответ: если Observable синхронный, то с Connect мы создадим 3 подписки, и ВСЕ три отработают как надо

Пояснение к примеру:

-   мы создаем Observable из 1, 2, 3, 4, 5
-   накладываем side эффект не меняющий значения, который выводит: "source emitted ${ n }"
-   внутри connect соединяем три Observable созданные из source

```js
const source$ = of(1, 2, 3, 4, 5).pipe(
    tap({
        subscribe: () => console.log('subscription started'),
        next: (n) => console.log(`source emitted ${n}`),
    })
);

source$
    .pipe(
        connect((shared$) =>
            merge(
                shared$.pipe(map((n) => `all ${n}`)),
                shared$.pipe(
                    filter((n) => n % 2 === 0),
                    map((n) => `even ${n}`)
                ),
                shared$.pipe(
                    filter((n) => n % 2 === 1),
                    map((n) => `odd ${n}`)
                )
            )
        )
    )
    .subscribe(console.log);
```

## Observable - tap

tap - добавляет side-эффект, не влияет на результат Observable:

-   tap(observerOrNext, error, complete)
    -   первый вариант заполнения
        -   observerOrNext - метод принимающий значение и ничего не возвращающий
        -   error - метод ошибки
        -   complete - метод завершения
    -   второй вариант заполнения
        -   observerOrNext - объект с полями:
            -   subscribe: () => void
            -   unsubscribe: () => void
            -   finalize: () => void
            -   next: (value: T) => void
            -   error: (err: any) => void
            -   complete: () => void

```js
of(1, 2, 3, 4, 5)
    .pipe(
        tap((n) => {
            console.log('value: ' + n);
        })
    )
    .subscribe(console.log);
```

## Observable - combineLatest/combineLatestAll/combineLatestWith

combineLatest/combineLatestAll/combineLatestWith - комбинирует набор Observable в один, который возвращает массив из последних значений набора Observable. Срабатывает при получении результата из ЛЮБОГО Observable из набора:

-   combineLatest(args)/combineLatestAll(args)/combineLatestWith(args)
    -   args - массив из Observable

Пояснение к примеру без использованием задержек:

-   какое-то странное поведение для Observable без задержки(смотри пример с of)
    -   у всех Observable, кроме последнего, берутся последние значение и кмбинируются со всеми значениями из последнего Observable

```js
const weight = of(70, 72, 76, 79, 75);
const height = of(1.76, 1.77, 1.78);

// combineLatest
const bmi = combineLatest([weight, height])
    .pipe(map(([w, h]) => w / (h * h)))
    .subscribe((x) => console.log('BMI is ' + x));

// combineLatestAll
const bmi = of(weight, height)
    .pipe(
        combineLatestAll(),
        map(([w, h]) => w / (h * h))
    )
    .subscribe((x) => console.log('BMI is ' + x));

// combineLatestWith
const bmi = weight.
    .pipe(
        combineLatestWith([height]),
        map(([w, h]) => w / (h * h))
    )
    .subscribe((x) => console.log('BMI is ' + x));
// => BMI is 24.212293388429753
// => BMI is 23.93948099205209
// => BMI is 23.671253629592222
```

Пояснение к примеру с использованием задержек:

-   пример этого не показывает, но если нет значения в Observable, то ждет пока во всех Observable появится хотя бы по одному значению

```js
const combinedTimers = combineLatest([
    interval(1000).pipe(map((value, index) => index)),
    interval(1500).pipe(map((value, index) => index * 10)),
    interval(2000).pipe(map((value, index) => index * 100)),
]).subscribe(console.log);
// => [1, 0, 0]
// => [2, 0, 0]
// => [2, 10, 0]
// => [3, 10, 0]
// => [3, 10, 100]
// => ...
```

## Observable - sample

sample - возвращает последние значение Observable, находящееся между двумя срабатывания другого Observable:

-   sample(notifier)
    -   notifier - другой Observable, по которому смотрим когда возвращать значения

```js
const clicks = fromEvent(document, 'click');
interval(1000)
    .pipe(sample(clicks))
    .subscribe((x) => console.log(x));
// (0.0s)  =>
// (1.0s)  =>
// (2.0s)  =>
// (click) => 2
// (3.0s)  =>
// (4.0s)  =>
// (click) => 4
// (click) =>
// (click) =>
// (5.0s)  =>
// (6.0s)  =>
// ...
```

## Observable - sampleTime

sampleTime - возвращает последние значение Observable, попавшие в указанный временной период:

```js
fromEvent(document, 'click')
    .pipe(map((value, index) => index))
    .pipe(sampleTime(1000))
    .subscribe(console.log);
// (0.0s)  =>
// (click) =>
// (click) =>
// (1.0s)  => 2
// (2.0s)  =>
// (click) =>
// (click) =>
// (click) =>
// (3.0s)  => 5
// ...
```

## Observable - repeat

repeat - перезапускает Observable несколько раз по его завершению:

-   retry(configOrCount)
    -   configOrCount
        -   передаем число
            -   по умолчанию infinit
            -   можно передать просто число перезупасков Observable
        -   передаем объект с полями count и delay
            -   можно передать объект вида { count: ..., delay: ... }
            -   delay - задержка в милисекундах между повторами
                -   в поле delay можно прокинуть функцию от номера повтора, которая возвращает другой Observable
            -   count - количество повторов

```js
of('Repeat')
    .pipe(
        repeat(3) // повторяем 3 раза
        // repeat(0) // вернет пустой observable
        // repeat() // будет повторять observable до посинения
        // repeat({ delay: 200 }) // будет повторять бесконечно с задержкой 200мс
        // repeat({ count: 2, delay: 400 }) // будет повторять 4 раза с задержкой 400мс
        // repeat({ delay: (count) => timer(count * 1000) }) // будет повторять бесконечно раз с задержкой count * 1000
    )
    .subscribe({
        next: console.log,
    });
// => Repeat
// => Repeat
// => Repeat
```

## Observable - retry

retry - будет перезапускать observable пока выдает ошибку, можно ограничить число перезапусков:

-   retry(configOrCount)
    -   configOrCount
        -   передаем число
            -   по умолчанию infinit
            -   можно передать просто число перезупасков Observable
        -   передаем объект с полями count и delay
            -   можно передать объект вида { count: ..., delay: ... }
            -   delay - задержка в милисекундах между повторами
                -   в поле delay можно прокинуть функцию от номера повтора, которая возвращает другой Observable
            -   count - количество повторов

```js
interval(1000)
    .pipe(
        mergeMap((val) => {
            // ошибка после 3 элемента
        }),
        retry(2) // повторяем 2 раза, пока ловим ошибку
        // retry(0) // вернет пустой observable
        // retry() // будет повторять observable до посинения
        // retry({ delay: 200 }) // будет повторять бесконечно с задержкой 200мс
        // retry({ count: 2, delay: 400 }) // будет повторять 4 раза с задержкой 400мс
        // retry({ delay: (count) => timer(count * 1000) }) // будет повторять бесконечно раз с задержкой count * 1000
    )
    .subscribe({
        next: (value) => console.log(value),
        error: (err) =>
            console.log(`${err}: Retried 2 times then quit!`),
    });
```

## Observable - catchError

catchError - меняет поведение Observable после пойманной ошибки

-   catchError(selector)
    -   selector(any, caught) - функция возвращающая Observable
        -   any
            -   сам текст ошибки
        -   caught
            -   сам Observable в котором была ошибка
            -   можно вызвать еще раз

Поянение к примеру:

-   меняем observable если поймали ошибку

```js
of(1, 2, 3, 4, 5)
    .pipe(
        map((n) => {
            /* возвращает ошибку если n===4 */
        }),
        catchError((err) => of('I', 'II', 'III', 'IV', 'V'))
    )
    .subscribe(console.log);
// => 1
// => 2
// => 3
// => I
// => II
// => III
// => IV
// => V
```

Поянение к примеру:

-   перезапускаем observable если поймали ошибку

```js
of(1, 2, 3, 4, 5)
    .pipe(
        map((n) => {
            /* возвращает ошибку если n===4 */
        }),
        catchError((err, caught) => caught),
        take(30)
    )
    .subscribe(console.log);
// => 1
// => 2
// => 3
// => 1
// => 2
// => 3
```

## Observable - auditTime

auditTime - возвращает каждые n милилисекунд последнее значение:

-   auditTime(duration, scheduler)
    -   duration - каждые n милисекунд, которые возвращает последнее значение
    -   scheduler - [Scheduler](#scheduler---методы-использующие-scheduler)

```js
fromEvent(document, 'click')
    .pipe(map((val, ind) => ind))
    .pipe(auditTime(1000))
    .subscribe(console.log);
```

## Observable - audit

audit - возврщает последнее значение Observable, когда наступает указанный Observable:

-   audit(durationSelector)
    -   durationSelector - это Observable, который определяет когда возвращаем значение

Пояснение к примеру:

-   interval работает, но его значения не проходят
-   когда кликаем, последнее значение interval проходит дальше

```js
const result = interval(1000)
    .pipe(map((val, ind) => ind))
    .pipe(audit((ev) => fromEvent(document, 'click')))
    .subscribe((x) => console.log(x));
// (0.0s) =>
// (1.0s) =>
// (2.0s) =>
// (3.0s) =>
// (click) => 3
// (4.0s) =>
// ...
```

## Observable - switchMap

switchMap - позволяет запустить из значения Observable, новый Observable, до тех пока не придет новое значение:

-   switchMap(project, resultSelector)
    -   project(value, index) - функция, которая возвращает новый Observable
    -   resultSelector(outerValue, innerValue, outerIndex, innerIndex) - опционально, устарело, по умолчанию undefined

Пояснения к примеру:

-   когда разок кликаешь, запускается interval
-   но если кликнуть еще разочек, то старый interval и запуститься новый interval

```js
fromEvent(document, 'click')
    .pipe(switchMap(() => interval(1000)))
    .subscribe(console.log);
```

## Observable - EMPTY

EMPTY - возвращает пустой Observable который ничего не возвращается и сразу заканчивает работу:

-   обычно используется с pipe которые должны возвращать Observable

```js
let subscriber = {
    next: (value) => console.log('Next: ' + value),
    complete: () => console.log('Complete!'),
};

EMPTY.subscribe(subscriber);
// => Complete!

EMPTY.pipe(startWith(7)).subscribe(subscriber);
// => Next: 7
// => Complete!
```

## Observable - bufferToggle

bufferToggle - собирает в массив значения Observable, только если они попадают в промежуток между первым и вторым сработавшими Observable:

-   bufferToggle(openings, closingSelector)
    -   openings - это Observable который затирает старый и открывает новый буффер(передний фронт сигнала)
    -   closingSelector - это Observable который закрывает новый буффер(задний фронт сигнала)

Пояснения к примеру:

-   когда разок кликаешь, запускается работа буффера
-   дальше тебе каждую секунду, дается интервал в пол секунды, чтобы накликать значений в буффер

```js
fromEvent(document, 'click')
    .pipe(
        bufferToggle(interval(1000), (i) =>
            i % 2 ? interval(500) : EMPTY
        )
    )
    .subscribe(console.log);
```

## Observable - bufferWhen

bufferWhen - собирает в массив значения Observable, и выбрасывает их только когда сработал другой Observable:

-   bufferToggle(openings, closingSelector)
    -   closingSelector - это Observable который закрывает буффер и создает новый, на заполнение

```js
fromEvent(document, 'click')
    .pipe(bufferWhen(() => interval(1000 + Math.random() * 4000)))
    .subscribe(console.log);
```

## Observable - bufferTime

bufferTime - собирает в массив значения Observable, по времени:

-   bufferTime(number, bufferCreationInterval, maxBufferSize, scheduler)
    -   number
        -   целое число(1000 - это одна секунда)
        -   время которое живет элемент внутри буффера
    -   bufferCreationInterval
        -   целое число(1000 - это одна секунда)
        -   как часто буффер отдается Observable
    -   maxBufferSize
        -   максимальный размер буффера
        -   если задан, то почему-то перестает работать буфферизация по вермени через number и bufferCreationInterval
    -   scheduler - [Scheduler](#scheduler---методы-использующие-scheduler)

```js
const clicks = fromEvent(document, 'click');
const buffered = clicks.pipe(bufferTime(5000, 1000));
buffered.subscribe((x) => console.log(x));
// (0.0s) => []
// (click)
// (1.0s) => [click]
// (2.0s) => [click]
// (3.0s) => [click]
// (click)
// (4.0s) => [click, click2]
// (5.0s) => [click, click2]
// (6.0s) => [click2]
// (7.0s) => [click2]
// (8.0s) => [click2]
// (9.0s) => []
// ...
```

## Observable - bufferCount

bufferCount - собирает в массив значения Observable, когда число новых элементов достигает bufferSize:

-   bufferCount(bufferSize, startBufferEvery)
    -   bufferSize - размер буффера(массива)
    -   startBufferEvery - после какого элемента инициализируется массив

```js
const clicks = fromEvent(document, 'click');
const buffered = clicks
    .pipe(
        map((value, index) => index),
        bufferCount(3)
    )
    .subscribe(console.log);
// (click)
// (click)
// (click)
// => [0, 1, 2]
// (click)
// (click)
// (click)
// => [3, 4, 5]
// (click)
// ...
```

## Observable - buffer

buffer - собирает в массив значения Observable, когда срабатывает другой Observable

Пояснения к примеру:

-   каждую секунду запоминаются данные, но не выводятся
-   по нажатия, выдает сразу массив с собранными данными

```js
const clicks = fromEvent(document, 'click');
const intervalEvents = interval(1000);
intervalEvents.pipe(buffer(clicks)).subscribe(console.log);
// (1.0s) =>
// (2.0s) =>
// (3.0s) =>
// (click) => [0, 1, 2]
// (4.0s) =>
// (5.0s) =>
// (click) => [4, 5]
// (click) => []
// ...
```

## Observable - zipAll

zipAll - производим zip для Observable выдаваемых Observable:

-   zipAll()

```js
of(
    of(1, 2, 3, 4),
    of('a', 'b', 'c', 'd', 'e', 'f', 'g'),
    of(100, 200, 300, 400)
)
    .pipe(zipAll())
    .subscribe(console.log);
// => [1, 'a', 100]
// => [2, 'b', 200]
// => [3, 'c', 300]
```

## Observable - zip

zip - производим zip для Observable в новый Observable:

-   zip(...args)
    -   args - набор Observable котрые будем zip

```zip
const age$ = of(27, 25, 29);
const name$ = of('Foo', 'Bar', 'Beer');
const isDev$ = of(true, true, false);

zip(age$, name$, isDev$).subscribe(x => console.log(x));
```

## Observable - zipWith

zipWith - производим zip для Observable в новый Observable:

-   zipWith(...args)
    -   args - набор Observable котрые будем zip

```js
of(100, 200, 300)
    .pipe(
        zipWith(
            of(1, 2, 3, 4, 5, 6),
            of('a', 'b', 'c', 'd', 'e', 'f', 'g')
        )
    )
    .subscribe(console.log);
// => [100, 1 'a']
// => [200, 2 'b']
// => [300, 3 'c']
```

## Observable - firstlast

first/last - по завершению работы Observable вернет первое/последние значение(или первое подходящее по условию):

-   first(predicate, defaultValue)/last(predicate, defaultValue)
    -   predicate - функция которая проверяет подходит ли значение
    -   defaultValue - стандартное значение, которое вернется если Observable завершен, а подходящего под predicate значения не найдено

```js
from(['x', 'y', 'z']).pipe(last());
// => z

from(['x', 'y', 'z']).pipe(last((char) => char === 'a', 'not found'));
// => not found
```

## Observable - skip

skip - пропускает первые n значений:

-   skip(n)
    -   n - количество первых пропускаемых значений

```js
interval(500).pipe(skip(10)).subscribe(console.log);
```

## Observable - takeLast/skipLast

takeLast/skipLast - по завершению работы Observable берет последние n значений:

-   takeLast/skipLast(number)
    -   number - целое число, указывающее сколько значений с конца будет взято по завершению работы Observable

```js
of(1, 2, 3, 4, 5, 6, 7).pipe(takeLast(2)).subscribe(console.log);
// => 6
// => 7
```

## Observable - takeUntil/skipUntil

takeUntil/takeWhile - когда другой Observable вернет результат, завершает-Observable/перестает-skip-элементы:

-   takeUntil(notifier)/takeUntil(notifier)
    -   notifier - другой Observable

Пояснения к примеру:

-   будет каждую секунду выводить значение пока не произойдет клик мышкой

```js
const clicks = fromEvent(document, 'click');
interval(1000).pipe(takeUntil(clicks)).subscribe(console.log);
// (1.0s) => 1
// (2.0s) => 2
// (3.0s) => 3
// (4.0s) => 4
// (click)
// (5.0s)
// (6.0s)
// ...
```

## Observable - takeWhile/skipWhile

takeWhile/skipWhile - когда получит первый элемент не соответствующий условию, завершает-Observable/перестает-skip-элементы:

-   takeWhile(predicate, inclusive)/takeWhile(predicate, inclusive)
    -   predicate(value, index) - функция возвращающая boolean, условие
    -   inclusive - если true, то вернет включительно со значением из-за которого нарушилось условие

```js
interval(1000)
    .pipe(takeWhile((x) => x < 5))
    .subscribe(console.log);
// (1.0s) => 0
// (2.0s) => 1
// (3.0s) => 2
// (4.0s) => 3
// (5.0s) => 4
```

## Observable - take/skip

take/skip - после первых n значений завершает-Observable/перестает-skip-элементы:

-   take(number)/skip(number)
    -   number - целое число, указывающее через сколько значений свою работу завершит Observable

```js
interval(1000).pipe(take(5)).subscribe(console.log);
// (1.0s) => 0
// (2.0s) => 1
// (3.0s) => 2
// (4.0s) => 3
// (5.0s) => 4
```

## Observable - min/max

min/max - при завершении Observable находит минимальный/максимальный элемент:

-   max(comparer)
    -   comparer(x, y) - то как сравнивать элементы

```js
const observable$ = of(5, 4, 7, 2, 8);
observable$.pipe(min()).subscribe(console.log);
// => 2
observable$.pipe(max()).subscribe(console.log);
// => 8
```

## Observable - switchScan

switchScan - scan для функций возвращающих Observable:

-   switchScan(accumulator, seed)
    -   accumulator(acc, value, index)
        -   acc - аккумулятор
        -   value - значение
        -   index - номер вызова
    -   seed - начальное значение которе подставится в acc внутрь функции accumulator в первый раз

```js
const transform = of(1, 2, 3, 4, 5, 6, 7, 8)
    .pipe(switchScan((acc, num) => of([...acc, num]), []))
    .subscribe(console.log);
// => [1]
// => [1, 2]
// => [1, 2, 3]
// => [1, 2, 3, 4]
// => [1, 2, 3, 4, 5]
// => [1, 2, 3, 4, 5, 6]
// => [1, 2, 3, 4, 5, 6, 7]
// => [1, 2, 3, 4, 5, 6, 7, 8]
```

## Observable - count

count - при завершении Observable считает количество элементов:

-   count(predicate)
    -   predicate(value, index) - возвращает boolean, который говорит считаем ли мы это значение или нет
        -   value - значение
        -   index - номер

```js
range(1, 7)
    .pipe(count((i) => i % 2 === 1))
    .subscribe(console.log);
// => 4
```

## Observable - reduce

reduce - проходится как обычный reduce, по всему что есть в Observable и завершает его:

-   reduce(accumulator, seed)
    -   accumulator(acc, value, index)
        -   acc - аккумулятор
        -   value - значение
        -   index - номер вызова
    -   seed - начальное значение которе подставится в acc внутрь функции accumulator в первый раз

Пояснения к примеру:

-   пояснения к примеру
    -   когда делаешь первый клик
    -   Observable пять секунд работает
    -   по оканчанию таймера завершает работу Observable(клики больше не работают)
    -   reducer понимает что Obsrvable отработал и запускается
    -   считаем число кликов

```js
fromEvent(document, 'click')
    .pipe(takeUntil(interval(5000)))
    .pipe(reduce((acc, one) => acc + 1, 0))
    .subscribe(console.log);
```

## Observable - expand

expand - запускает Observable который рекурсивно применяет функцию к значению:

-   expand(project, concurent, shceduler)
    -   project(value, index) - передаем функцию
    -   concurent - по умолчанию infinity, максимальное число подписчиков(?)
    -   scheduler - [Scheduler](#scheduler---методы-использующие-scheduler)

```js
fromEvent(document, 'click')
    .pipe(
        map(() => 1),
        expand((x) => of(2 * x).pipe(delay(100))),
        take(20)
    )
    .subscribe((x) => console.log(x));
// (click)
// => 1
// => 2
// => 4
// => 8
// => ...
```

## Observable - scan

scan - обычный reducer:

-   scan(accumulator, seed)
    -   accumulator(acc, value, index)
        -   acc - аккумулятор
        -   value - значение
        -   index - номер вызова
    -   seed - начальное значение которе подставится в acc внутрь функции accumulator в первый раз

```js
of(1, 1, 1, 1, 1, 1)
    .pipe(scan((total, n, index) => n * index + total, 1))
    .subscribe(console.log);
// => 1
// => 2
// => 4
// => 7
// => 11
// => 16
```

## Observable - mergeScan

mergeScan - сначала производим reduce с данными, а потом производим с ними merge:

-   mergeScan(accumulator, seed)
    -   accumulator(acc, value, index)
        -   acc - аккумулятор
        -   value - значение
        -   index - номер вызова
    -   seed - начальное значение которе подставится в acc внутрь функции accumulator в первый раз
    -   concurrent - опционально, максимальное число подписчиков(?)

```js
fromEvent(document, 'click');.pipe(
    map(() => 2),
    mergeScan((acc, multiple) => of(acc * multiple), 1)
).subscribe(console.log);
// (click) => 2
// (click) => 4
// (click) => 8
// (click) => 16
// (click) => ...
```

## Observable - mergeMap

mergeMap - к каждому значению применяет функцию, а потом соединяет в параллельный Observable:

-   mergeMap(project, resultSelector, concurrent)
    -   project(value, index) - передаем функцию
    -   resultSelector(outerValue, innerValue, outerIndex, innerIndex) - опционально, устарело, по умолчанию undefined
    -   concurrent - опционально, максимальное число подписчиков(?)

```js
of('a', 'b', 'c')
    .pipe(mergeMap((x) => interval(1000).pipe(map((i) => x + i))))
    .subscribe(console.log);
// (1.0с) => a0
// (1.0с) => b0
// (1.0с) => c0
// (2.0с) => a1
// (2.0с) => b1
// (2.0с) => c1
// ...
```

## Observable - mergeAll

mergeAll - Observable, которые возвращает Observable, запускает параллельно:

```js
fromEvent(document, 'click')
    .pipe(
        map(() => interval(1000)),
        mergeAll()
    )
    .subscribe(console.log);
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

merge(timer1, timer2, timer3, timer1, 2).subscribe(console.log);
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

```js
fromEvent(document, 'click')
    .pipe(map(() => interval(1000).pipe(take(5))))
    .pipe(exhaustAll())
    .subscribe(console.log);
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

-   exhaustMap(project, resultSelector)
    -   project(value, index) - функция которую map к значениям Observable
        -   value - значение
        -   index - номер вызова
    -   resultSelector(outerValue, innerValue, outerIndex, innerIndex) - опционально, устарело, по умолчанию undefined

```js
fromEvent(document, 'click')
    .pipe(exhaustMap(() => interval(1000).pipe(take(5))))
    .subscribe(console.log);
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
fromEvent(document, 'click')
    .pipe(map(() => interval(1000).pipe(take(4))))
    .pipe(concatAll())
    .subscribe(console.log);
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

-   concatMap(project, resultSelector)
    -   project(value, index) - функция которую map к значениям Observable
        -   value - значение
        -   index - номер вызова
    -   resultSelector(outerValue, innerValue, outerIndex, innerIndex) - опционально, устарело, по умолчанию undefined

```js
fromEvent(document, 'click')
    .pipe(concatMap((ev) => interval(1000).pipe(take(4))))
    .subscribe(console.log);
```

## Observable - timer

timer - создает Observable со сдвигом во времени:

-   timer(dueTime, scheduler)
    -   dueTime - целое число(1000 - это одна секунда)
    -   scheduler - [Scheduler](#scheduler---методы-использующие-scheduler)

```js
console.log('START!');

const observable$ = interval(1000).pipe(take(6));
const shiftObservable$ = timer(3000).pipe(
    concatMap(() => observable$)
);

observable$.subscribe(console.log(el));
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

delay - задержка, перед отправкой данных в Subscriber:

-   timer(due, scheduler)
    -   due - целое число(1000 - это одна секунда)
    -   scheduler - [Scheduler](#scheduler---методы-использующие-scheduler)

```js
const interval$ = of(1, 2, 3, 4, 5)
    .pipe(delay(2000))
    .subscribe(console.log);
// ( 2.0s) => 1
// ( 4.0s) => 2
// ( 6.0s) => 3
// ( 8.0s) => 4
// (10.0s) => ...
```

## Observable - delayWhen

delayWhen - отложенная инициализация Observable, до момента подписки:

-   delayWhen(delayDurationSelector, subscriptionDelay)
    -   delayDurationSelector(value, index)
        -   функция - вызывается при каждом новом значении Observable
        -   функция - возвращает Observable, который когда срабаатывает - возвращает результат

Пояснение к примеру:

-   каждому клику ставится в соответствие index, который возвращается со случайной задержкой от 0 до 5 секунд

```js
fromEvent(document, 'click')
    .pipe(
        scan((acc, value, index) => index, 0),
        delayWhen(() => interval(Math.random() * 5000))
    )
    .subscribe(console.log);
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

-   interval(period, scheduler)
    -   period - целое число(1000 - это одна секунда)
    -   scheduler - [Scheduler](#scheduler---методы-использующие-scheduler)

```js
interval(1000).subscribe(console.log);
// => 1
// => 2
// => 3
// => 4
// => ...
```

## Observable - bindCallback

bindCallback - оборачивает переданную функцию, и дает возможность создать из нее Observable:

-   bindCallback(callbackFunc, resultSelector, scheduler) - ыфв
    -   callbackFunc
        -   функция из которой сделаем Observable
        -   функция должна принимать последним аргументом callback
    -   resultSelector(outerValue, innerValue, outerIndex, innerIndex) - опционально, устарело, по умолчанию undefined
    -   scheduler - [Scheduler](#scheduler---методы-использующие-scheduler)

Пояснения к примеру:

-   someFunction - принимает последним аргумнтом callback
    -   в callback будут подставляться методы subscribe
-   boundSomeFunction - это someFunction внутри специальной обертки
-   boundSomeFunction(5, 'some string') - создаем observable с переданными параметрами
-   boundSomeFunction(5, 'some string').subscribe(...) - передаем callback, последним аргументов функции
    -   subscribe способен принимать только один параметр
    -   передав несколько параметров в Callback, они соберуться в один массив

```js
const someFunction = (x, y, callback) => {
    callback(x, y, { someProperty: 'someValue' });
};

const boundSomeFunction = bindCallback(someFunction);
boundSomeFunction(5, 'some string').subscribe((values) => {
    console.log(values);
});
// => [5, 'some string', {someProperty: 'someValue'}]
```

## Observable - bindNodeCallback

bindNodeCallback - оборачивает переданную функцию, и дает возможность создать из нее Observable:

-   bindNodeCallback(callbackFunc, resultSelector, scheduler) - ыфв

    -   callbackFunc
        -   функция из которой сделаем Observable
        -   функция должна принимать последним аргументом callback
        -   callback - первым аргументом получает ошибку(null если нет ошибки), а остальные аргументы это рещультат
    -   resultSelector(outerValue, innerValue, outerIndex, innerIndex) - опционально, устарело, по умолчанию undefined
    -   scheduler - [Scheduler](#scheduler---методы-использующие-scheduler)

Пояснения к примеру:

-   работает как bindCallback
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

-   of(..args)
    -   args - аргументы из которых будет создан Observable

```js
const observable$ = of(10, 20, 30);
```

## Observable - from

from - создание Observable из различных объектов(например Promise):

-   from(input, scheduler)
    -   input - объект из которых можно создать Observable
        -   array-like
        -   itterable
        -   Promise
-   scheduler - [Scheduler](#scheduler---методы-использующие-scheduler)

```js
const observable$ = from([10, 20, 30]);

const iterator = new generate(3);
const observable2$ = from(iterator);
```

## Observable - range

range - создает count чисел типа integer, начиная от start, через 1:

-   range(start, count, scheduler)
    -   start - integer число от которого начинаем
    -   count - integer число говорящая сколько чисел создаем
    -   scheduler - [Scheduler](#scheduler---методы-использующие-scheduler)

```js
const numbers = range(10, 3);
// => 10
// => 11
// => 12
```

## Observable - fromEvent

fromEvent - создает Observable из события:

-   fromEvent(target, eventName, EventListenerOptions, resultSelector)
    -   target - элемент DOM-дерева
    -   eventName - строка с названием event
    -   EventListenerOptions - опциональный аргумент
    -   resultSelector(outerValue, innerValue, outerIndex, innerIndex) - опционально, устарело, по умолчанию undefined

Пояснения к примеру:

-   document - элемент DOM дерева
-   'click' - событие которое ждем

```js
const clicks = fromEvent(document, 'click');
```

## Observable - fromEventPattern

fromEventPattern - создает Observable из события, а также добавляет способ удаления собыития, в случае отписки от Observable:

-   fromEventPattern(addHandler, removeHandler, resultSelector)
    -   addHandler - добавляем событие на элемент при появлении подписчика у Observable
    -   removeHandler - удаляем событие на элементе при потписке от Observable
    -   resultSelector(outerValue, innerValue, outerIndex, innerIndex) - опционально, устарело, по умолчанию undefined

Пояснения к примеру:

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
