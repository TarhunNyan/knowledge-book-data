# JS - Angular

Framework для singlepage страниц в вебе

-   Поддерживается Google
-   Использует typescript
-   Использует RxJS
-   Очень строгий в своей структуре
-   Все есть из коробки

# Component

## Lifecycle

Жизненный цикл Component основан вокруг трех событиях:

-   [ngOnInit](#lifecycle---ngoninit)
-   [ngOnChanges](#lifecycle---ngonchange)
-   [ngOnDestroy](#lifecycle---ngondestroy)

## ChangeDetect

ChangeDetect - отслеживание изменений происходит несколькими способами:

-   [BindingData](#)

Шаблонные переменные:

-   [Шаблонные переменные - используем Component из шаблона внутри этого же шаблона]()
-   [ViewChild - получаем любой Component из шаблона для использования в коде](#decorator---viewchild)
-   [ContentChild - получаем весть переданный в Component контент(Content-block)](#decorator---contentchild)

# Template

-   [Размещение переданного Content внутри шаблона](#template---ng-content)

# Примеры

## Template - ng-content

Если предположить, что код ниже это шаблон app-component:

```html
<div>projected content begins</div>
<ng-content></ng-content>
<div>projected content ends</div>
```

Шаблон ниже:

```html
<app-component>
    <p>Это контент</p>
</app-component>
```

Создаст такую структуру страницы:

```html
<div>projected content begins</div>
<p>Это контент</p>
<div>projected content ends</div>
```

## ChangeDetect - Шаблонные переменные

Шаблонные переменные - дают возможность использовать Component из шаблона внутри этого же шаблона:

-   #userName - переменная в ts файле через которую теперь можно манипулировать DOM-элементом

Пример использования:

-   у child-comp есть методы increment и decrement и мы этим можем пользоваться прямо внутри шаблона

```html
<child-comp #counter></child-comp>
<button (click)="counter.increment()">+</button>
<button (click)="counter.decrement()">-</button>
```

## Decorator - ViewChild

ViewChild - дают возможность использовать Component из шаблона внутри кода typescript:

-   @ViewChild(ChildComponent, {static: false}) - декоратор ViewChild отслеживающий <child-comp>
    -   ChildComponent - имя класса с декоратором @Component
    -   selector - можно использовтаь вместо ChildComponent
    -   static
        -   если true, то можно обращаться к элементу уже на этапе ngOnInit
        -   если false, то можно обращаться к элементу начиная с этапа ngAfterViewInit
            -   полезно если Component создается не сразу, а нпример Directive такой как ngIf
    -   read
        -   работает только при связи с "Шаблонными переменными"
        -   указывает тип компонента, иначе вернет ElementRef

```ts
import { Component, ViewChild } from '@angular/core';
import { ChildComponent } from './child.component';

@Component({
    selector: 'my-app',
    standalone: true,
    imports: [ChildComponent],
    template: `
        <child-comp-first></child-comp>
        <child-comp-second></child-comp>
        <child-comp-third #templateVar></child-comp>
        <button (click)="increment()">+</button>
        <button (click)="decrement()">-</button>
    `,
})
export class AppComponent {
    @ViewChild(ChildComponent)
    private childCompFirst: ChildComponent | undefined;

    @ViewChild({ static: false, selector: 'child-comp-second' })
    private childCompSecond: ChildComponent | undefined;

    @ViewChild('templateVar', { read: ChildComponent })
    private childCompThird: ChildComponent | undefined;

    increment() {
        this.childCompFirst?.increment();
    }
    decrement() {
        this.childCompFirst?.decrement();
    }
}
```

## Decorator - ContentChild

ContentChild - получаем содержимое текущего Component:

-   ChildComponent - компонент который в каком-то шаблоне применяется так:
    -   <child-comp><p>Наш искомый header</p></child-comp>

```js
import { Component, ContentChild, ElementRef } from '@angular/core';

@Component({
    selector: 'child-comp',
    standalone: true,
    template: `<ng-content></ng-content>
        <button (click)="change()">Изменить</button>`,
})
export class ChildComponent {
    @ContentChild('headerContent', { static: false })
    header: ElementRef | undefined;

    change() {
        if (this.header !== undefined) {
            console.log(this.header);
            this.header.nativeElement.textContent = 'Hell to world!';
        }
    }
}
```

## Lifecycle - ngOnInit

Для создания компонента, используется ngOnInit и вызывается следующая цепочка вызовов:

-   constructor - обычный constructor для javascript
-   ngOnChange - вызываетя до ngOnInit, об этом и в доке написано
-   ngOnInit - наше основное событие по инициализации
-   ngDoCheck
-   ngAfterContentInit
-   ngAfterContentChecked
-   ngAfterViewInit
-   ngAfterViewChecked

## Lifecycle - ngOnChange

Для создания компонента, используется ngOnInit и вызывается следующая цепочка вызовов:

-   ngOnChange - наше основное событие, в котором отрабатываем изменения
-   ngDoCheck
-   ngAfterContentChecked
-   ngAfterViewChecked

## Lifecycle - ngOnDestroy

Перед удалением компонента из DOM дерева, используется ngOnDestroy и вызывается следующая цепочка вызовов:

-   ngOnDestroy - наше основное событие, в котором освобождаем память перед уничтожением Component

-   [ngOnChanges - может отработать до ngOnInit, отлавливает изменения связанные с BindingData]()
-   [ngOnInit - инициализация Component, происходит после constructor]()
-   [ngDoCheck - срабатывает после каждого ngOnChange и ngOnInit]()
-   [ngAfterContentInit - срабатывает однажды, после первого ngDoCheck]()
-   [ngAfterContentChecked - срабатывает после первого ngAfterContentInit и каждого ngDoCheck]()
-   [ngAfterViewInit - срабатывает однажды, после первого ngAfterContentChecked]()
-   [ngAfterViewChecked - срабатывает после первого ngAfterViewInit и каждого ngAfterContentChecked]()
-   [ngOnDestroy - вызывается для очистки, перед уничтожением Component]()

## Настройка окружения

Окружение можно настраивать для Production и Development состояний:

-   [Глобальный config](#environment---global)

## Environment - global

Глобальный config:

-   /src/environment/environment.ts - настройки для Develop
-   /src/environment/environment.prod.ts - настройки для Production

Чтобы в коде получить доступ к настройкам config:

-   пример из /src/main.ts

```js
import { enableProdMode } from '@angular/core';
import { environment } from './environments/environment';

if (environment.production) {
    enableProdMode();
}
```

# Элементы Angular

Устройство Angular:

-   Module - в Angular это то из чего строится приложение. Что-то вроде экранов в android разработке
-   Component - в Angular это то из чего строится module. В нем хранится логика
-   Template - шаблон для component
    -   Directive - директивы, помогают выстраивать шаблон
    -   Pipe - позволяют применять всякие разные функции внутри шаблона
-   Service - некая внешняя логика, которая не входит в верстку. К ней относятся запросы на сервер и т.п.

Важные элементы:

## Module

Module - в Angular это то из чего строится приложение, это как экраны в Android разработке:

-   /src/app/name.module.ts - пример пути до module
    -   name - имя модуля

Создание module:

-   [Инициализация Module](#module---инициализация)
-   [Настройка NgModule](#module---ngmodule)

## Standalone

Standalone - аналог ngModule, если модуль тащит с собой все компоненты привязанные к нему, то standalone это одиночный компонент:

-   начиная с Angular17 проект создается в standalone стиле
-   чтобы создавать проект как и ранее, просто добавь параметр при запуске --no-standalone

```bash
ng new --no-standalone
```

## Component

Component - это элементы module, то есть компоненты расположенные на экране:

-   /src/app/components/name/name.component.ts - пример пути до component
-   /src/app/components/name/name.component.html - пример пути до template
-   /src/app/components/name/name.component.css - пример пути до css
-   /src/app/components/name/name.component.spec.ts - пример пути до test
-   name в путях - имя компонента

Создание component:

-   [Инициализация Component](#component---инициализация)
-   [Инициализация Component через cli](#component---cli)
-   [Настройка Component](#component---component)
-   [Жизненный цикл Component(lifecycle hooks)](#component---lifecycle)
-   [Передача аттрибутов в Component](#component---передача-аттрибутов)

Связывание component и template:

-   [Получить переменную из component(content)](#component---передать-переменную-в-template)
-   [Передача аттрибутов из component в template(attributes)](#component---передача-аттрибутов)
-   [Передача данных из template в component(event)](#template---eventlistener)

Связывание Component и Component:

-   [Используем Angular события(eventEmitter)](#component---eventemitter)

## Component - eventEmitter

Для передачи данных из нижнего уровня на верхний используется eventEmitter. Он вызвается на нижнем уровне и отлавливается на верхнем:

-   вызываем событие

```typescript
@Output
changeStage: EventEmitter<Task> = new EventEmitter<Task>();

onSelected(task: Task) {
    this.changeStage.emit(task);
}
```

Добавляем событие в компонент:

```html
...
<div (click)="onSelected()">Some div</div>
```

Теперь необходимо поймать событие на верхнем уровне:

-   прописываем в родительском template
-   $event - через доллар принято обозначать события

```typescript
<child-component (select)="someParentMethod($event)"></child-component>
```

## Template

Template - шаблоны, через которые определяется верстка Component:

-   /src/app/components/name/name.component.ts - пример пути до component
-   /src/app/components/name/name.component.html - пример пути до template
-   /src/app/components/name/name.component.css - пример пути до css
-   /src/app/components/name/name.component.spec.ts - пример пути до test
-   name в путях - имя компонента

Связывание template и component:

-   [Получить переменную из Component(content)](#component---передать-переменную-в-template)
-   [Передача аттрибутов из Component в Template(attributes)](#component---передача-аттрибутов)
-   [Передача данных из Template в Component(event)](#template---eventlistener)
-   [Использование Pipe внутри шаблона](#template---pipe)
-   [Template reference variable(перемменные через решетку)](#template---reference-variable)

Связать шаблоны:

-   [Получить content компоненты переданный ему из другого шаблона](#template---ng-content)

Directive:

-   [Directive, что это?](#template---directive)
-   [Directive - ngIf](#template---ngif)
-   [Directive - ngFor](#template---ngfor)
-   [Directive - ngSwitch](#template---ngswitch)
-   [Directive - ngStyle](#template---ngstyle)
-   [Directive - ngClass](#template---ngclass)

Создание Pipe и Directive:

-   [Создаем свой(Custom) Pipe](#template---pipe)
-   [Создаем свой(Custom) Directive](#template---custom-directive)

## Service

Service - это такие части кода, например как получение данных с сервера:

-   /src/app/services/name.service.ts - пример пути до service
-   name в путях - имя srvice

Создание service:

-   [Инициализация Service](#service---инициализация)
-   [Инициализация Service через cli](#service---cli)
-   [Настройка Service](#service---injectable)
-   [Пример Service работающтй с http(get)](#service---httprequest)

## Routing

Routing - в Angular мы создаем одностраничное приложение, и навигация работает именно на стороне frontend-а:

Создаем ngModule с подключенным Routing:

```bash
ng g m ./path/module --routing
```

## Resolver

Resolver - подгружает данные до загрузки самой страницы:

-   нужен чтобы по 10 раз не перерисовывать одно и тоже, сначало без данных а потом с данными

# Дополнительно

## Откуда мы начинаем? Main.ts

/src/main.ts - точка входа

В нем:

-   AppModule - импортированный модуль, который запускаем

```js
import AppModule from './app/app.module';

platformBrowserDynamic()
    .bootstrapModule(AppModule)
    .catch((err) => console.error(err));
```

## Заглушки и модели данных

/app/src/models - описание структур данных на typescript
/app/src/data - заглушки для тестовых данных

## Forms

Ну классика же, опять формы. Внутри Component:

-   [ Validators.required, ...] - валидация полей

```typescript
cstmform = new FormGroup({
	title: new FormControl()<string>('', [
		Validators.required,
		Validators.minLength(6)
	])
});

submit() {
	console.log( this.form.value )
}
```

Теперь в шаблоне указываем форму:

-   [formGroup]="form" - связываем form с переменной в Component с названием cstmform
-   (ngSubmit)="submit" - связываем событие Submit(отправка формы) с функцией submit
-   formControlName="title" - связываем полу input с полем title внутри cstmform

```html
<form [formGroup]="cstmform" (ngSubmit)="submit">
    <input type="text" formControlName="title" />

    <button type="submit">Create</button>
</form>
```

Приколы:

-   title.error?.minlength - произошла ли ошибка валидации minlength
-   title.touched - было ли поле уже в фокусе

```js
title = this.form.controls.title;
title.error?.minlength;
title.touched;
```

## Обработчики

Двустороннее связывание, чтобы заработало надо подключить FormsModule:

-   [(ngModel)] - создает двустороннее связывание
-   term - поле класса типа строка. То есть при изменении в input сразу произойдут изменения в component

```html
<input [(ngModel)]="term" />
```

# Примеры

## Module - инициализация

Module - класс обернутый в декоратор NgModule:

-   @NgModule - декоратор в котором происходит настройка Module
-   export class AppModule {} - сам класс Module

```js
import { NgModule } from '@angular/core';

// настройки модуля
@NgModule({
    ...
})
// экспорт модуля
export class AppModule {}
```

## Module - NgModule

NgModule - принимает js объект в полях которого указываются настройки:

-   declarations - список component, directive и pipe
-   imports - список дополнительных module
-   bootstrap - то с чего начинается все приложение
-   providers - сюда можно регистрировать services

```js
import { NgModule } from '@angular/core';

@NgModule({
    declarations: [ AppComponent ],
    imports: [ BrowserModule, AppRoutingModule ],
    providers: [],
    bootstrap: [ AppComponent ],
})
```

## Component - Инициализация

Component - класс обернутый в декоратор Component:

-   @Component - декоратор в котором происходит настройка Component
-   export class AppComponent {} - сам класс Component
-   Чтобы все заработало, также необходимо в настройках Module добавить Component, чтобы Module знал о его существовании

```typescript
import Component from '@angular/core';

@Component({
	...
})
export class AppComponent {
	...
}
```

## Component - cli

Генерируем component через cli:

-   ng - cli
-   generate - говорим что генерируем
-   component - генерируем component
-   components - путь в котором создаем component
-   global-error - имя component
-   --skip-tests - пропускаем(не создаем) тесты

```bash
ng generate component components/global-error --skip-tests
# с short флагами
ng g c components/global-error --skip-tests
```

## Component - Component

Component - принимает js объект в полях которого указываются настройки:

-   selector - ищет в /src/index.html тег app-root и рендерим в него component
-   templateUrl - шаблон в на основе которого рендерим компонент
-   styleUrls - стили компонента

```typescript
import Component from '@angular/core';

@Component({
    selector: 'app-root',
    templateUrl: './app.component.html',
    styleUrls: ['./app.component.scss'],
})
export class AppComponent {
    tittle = 'name-of-project';
}
```

## Component - передача аттрибутов

Чтобы у Component появилась возможномсть получать аттрибуты из template:

-   Input - класс который создает аттрибуты в модуле
-   'my-product' - имя аттрибута, можно не указывать, тогда возьмет имя переменной как имя аттрибута
-   product: IProduct - переменная, у которой задан интерфейс(все таки это typescript)

```typescript
import {Component, Input} from '@angular/core';

@Component({
	...
})
export class AppComponent {
	@Input('my-product') product: IProduct
}
```

Тогда в шаблоне Component мы можем прописать:

-   [my-product] - название аттрибута определенного в классе
-   "products[0]" - динамический js

```html
<app-product [my-product]="products[0]"></app-product>
```

## Component - передать переменную в template

Переменные оперделенные в Component можно использовать внутри template, только для содержимого:

-   title - переменная определенная в Component
-   вместо title[0] может быть любой однострочный js-код

```html
<h1>{{ title[0] }}</h1>
```

Объявление переменной в Component:

```typescript
import Component from '@angular/core';

@Component({
	...
})
export class AppComponent {
	tittle = ['name-of-project'];
}
```

## Component - Lifecycle

У моделей есть жизненный цикл. Чтобы восопользоваться, просто имплементируем интерфейс одного из жизненных циклов, после реализуем требуемые функции:

-   constructor(private productsService: ProductsService) {} - подключаем какой-нибудь Service в наш Component
-   implements OnInit - имплементируем метод срабатывающий при инициализации Component
-   ngOnInit() {} - метод срабатывающий при инициализации Component

```js
export class AppComponent implements OnInit {
	constructor(private productsService: ProductsService) {}

	ngOnInit(): void {
		...
	}
}
```

## Template - ng-content

ng-content - позволяет получить content переданный из другого шаблона:

```html
<ng-content></ng-content>
```

Передаем content внутрь component:

-   <div>Внутренний контент</div> - content который вложили в шаблон

```html
<app-module>
    <div>Внутренний контент</div>
</app-module>
```

## Template - Directive

В шаблонах Angular можно использовать так наызваемые директивы. Есть структурные(меняют структуру html) и обычные(не влият на шаблон):

-   структурные - начинаются со звездочки и меняют структуру шаблона
-   обычные - начинаются не со звездочки

## Template - Pipe

Pipe, применяет функцию к переменной:

-   lowercase - приводит к нижнему регистру

```html
<h1>{{ title | lowercase }}</h1>
```

Пример с передачей параметров в pipe функцию:

-   number:'2.2-4' - pipe-функция с параметром
    -   number - функция
    -   : - дальше пойдут параметры
    -   '2.2-4' - значение переданного параметра

```html
<h1>{{ title | number:'2.2-4' }}</h1>
```

## Template - Reference Variable

Редко используется, можно создать двутороннюю связь через специальный синтаксис:

-   #phone - создаст переменную phone, которую можно использовать
-   phone.value - использование переменной, которой лежит input
-   создает переменную во всем шаблоне

```html
<input #phone placeholder="phone number" />
<button type="bytton" (click)="callPhone(phone.value)">Call</button>
```

## Template - ngIf

ngIf - директива условия. Отображается если условие выполняется:

```html
<div *ngIf="count >= 4">
    <span>Some content</span>
</div>
```

## Template - ngFor

ngFor - директива цикла:

-   "let product of products" - создает локальную переменную product
-   ="product" - js код который видит локальную переменную

```html
<!-- Вместо hardcode -->
<app-product [product]="products[0]"></app-product>
<app-product [product]="products[1]"></app-product>
<app-product [product]="products[2]"></app-product>

<!-- Используем иттерацию по по массиву -->
<app-product
    *ngFor="let product of products"
    [some-attr]="product"
></app-product>
```

## Template - ngSwitch

ngSwitch - директива swich'а:

-   ="tab" - указываем поле по которому смотрим значения
-   ="skills" и ="hobbies" - значения которые проверяем в поле

```html
<div [ngSwitch]="tab">
    <div *ngSwitchCase="skills">Read, write</div>
    <div *ngSwitchCase="hobbies">Iadernaya Physica</div>
</div>
```

## Template - ngClass

ngClass - передаем в него js-объект, через который можем добавлять классы по условию:

-   ключи - имена классов, значения - условия при котором класс будет добавлен
-   'first_class' - имя класса которое добавляем
-   10 < 20 - условие, которое если true, то добавляем если false то кдасс не добавляем

```html
<button
    class="some_class"
    [ngClass]="{
    'first_class': 10 < 20,
    'second_class': isSecondClass
}"
></button>
```

## Template - ngStyle

ngStyle - передаем в него js-объект, через который можем добавлять классы по условию:

-   ключи - имена свойств, значения - значения свойств
-   'first_class' - имя класса которое добавляем
-   10 < 20 - условие, которое если true, то добавляем если false то кдасс не добавляем

```html
<button
    style="font-size: 14pt;"
    [ngStyle]="{
    'fontWeight': 'bold',
    'fontColor': color
}"
></button>
```

## Template - eventListener

Обработчик событий внутри шаблона:

-   details - переменная boolean внутри component

```html
<button
    (click)="details = !details"
    (mousedown)="details = !details"
    (keydown)="details = !details"
></button>
```

## Template - Custom Directive

Создаем Directive через cli:

```bash
ng generate directive directives/focus --skip-tests
ng g d directives/focus --skip-tests
```

Регистрируем Directive в Moudle через Declaration:

```typescript
import { NgModule } from '@angular/core';

@NgModule({
    declarations: [ FocusDirective ],
	...
})
```

Правим логику Directive:

-   ElementRef - ссылка на элемент, в котором указана дирректива
-   selector: '[appFocus]' - указывает, что Directive имеет имя appFocus
-   пример - создает Directive которая фокусируется на элементе

```typescript
@Directive({
	selector: '[appFocus]'
})
export class FocusDirective implements OnInit, AfterViewInit {
	constructor(private el: ElementRef ) { }
	ngOnInit(): void {
		this el.nativeElement.focus()
	}
}
```

## Template - Custom Pipe

Создаем pipe через cli:

```bash
ng generate pipe pipes/filter-product --skip-tests
# с short флагами
ng g p pipes/filter-product --skip-tests
```

Регистрируем Pipe в Moudle через Declaration:

```typescript
import { NgModule } from '@angular/core';

@NgModule({
    declarations: [ FilterProductsPipe ],
	...
})
```

Дальше прописываем логику:

-   name: 'filterProduct' - имя Pipe при использовании в шаблонах и коде
-   @Pipe - обязательный декоратор для объявление Pipe
-   implements PipeTransform - interface от которого нужно имплементировать Pipe
-   transform(...) {...} - единственный метод который обязательно надо реализовать в Pipe

```js
import {IProduct} from '../models/product'

@Pipe({
	name: 'filterProduct'
})
export class FilterProductsPipe implements PipeTransform {
	transform( products: IProducts[], search: string): IProduct[] {
		return products.filter(...)
	}
}
```

Пример использования в шаблоне:

-   product - подставится в качестве первой переменной Pipe
-   'str to search' - подставится в качестве второй переменной Pipe

```html
<div *ngIf="products$ | async as products">
    <app-product
        *ngFor="let product of products | filterProduct: 'str to search';"
    ></app-product>
</div>
```

## Service - инициализация

Service - класс обернутый в декоратор Injectable:

-   @Injectable - декоратор в котором происходит настройка Component
-   export class ProductService {} - сам класс Service
-   Чтобы все заработало, также необходимо в настройках module добавить service, чтобы module знал о его существовании

```typescript
import { Injectable } from '@angular/core';

@Injectable({
	...
})
export class ProductService {
	...
}
```

## Service - cli

Генерируем service через cli:

-   ng - cli
-   generate - говорим что генерируем
-   service - генерируем component
-   services - путь в котором создаем srvice
-   global-error - имя service
-   --skip-tests - пропускаем(не создаем) тесты

```bash
ng generate service services/error --skip-tests
# с short флагами
ng g s services/error --skip-tests
```

## Service - Injectable

Injectable - принимает js объект в полях которого указываются настройки:

-   providedIn: 'root' - регестрируем service в нашем module, хотя это можно сделать и внутри module

```typescript
import { Injectable } from '@angular/core';

@Injectable({
	providedIn: 'root',
})
export class ProductService {
	...
}
```

## Service - httpRequest

Для запрососв на сервер используется специальный Angular сервис, HttpClient. Для запросов мы используем в service вот такую конструкцию:

```js
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';

@Injectable({
	providedIn: 'root',
})

export class ProductService {
	constructor(private http: HttpClient) {}

	getAll() {
		return this.http.get<IProduct[]>('http://...', {
			// fromString: 'limit=5'
			// fromObject: { limit: 5 }
			params: new HttpParams().append('limit', 5)
		}).pipe(
			delay(2000)
		);
	}
}
```

После чего в Component, нам нужно подписаться на Stream(RxJS):

```js
this.productsService.getAll().subscribe((products) => {
    this.product = products;
});
```
