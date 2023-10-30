# JS - Angular

Framework для singlepage страниц в вебе

-   Поддерживается Google
-   Использует typescript
-   Использует RxJS
-   Очень строгий в своей структуре
-   Все есть из коробки

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

Связать шаблоны:

-   [Получить content компоненты переданный ему из другого шаблона](#template---ng-content)

Directive:

-   [Directive, что это?](#template---directive)
-   [Directive - ngIf](#template---ngif)
-   [Directive - ngFor](#template---ngfor)
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
