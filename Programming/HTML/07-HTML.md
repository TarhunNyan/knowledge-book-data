# HTML

Hyper Text Markup Language - язык гипертекстовой разметки

Современные версии HTML являются вариациями XML

# Аттрибуты

## Универсальные аттрибуты

Универсальные аттрибуты - есть у всех tag:

-   [class](#attribute---class)
-   [id](#attribute---id)
-   [style - стили](#attribute---style)
-   [tabindex - фокусировка по tab](#attribute---tabindex)
-   [accesskey - элемент по hotkey](#attribute---accesskey)
-   [hidden - спрятать элемент](#attribute---hidden)
-   [title - всплывающая подсказка](#attribute---title)
-   [contenteditable - редактируемость](#attribute---contenteditable)
-   [dir - направление текста](#attribute---dir)

## Аттрибуты событий

Внутри HTML:

```html
<p onclick="() => { console.log("OK") }"> ... </p>
```

Через JS:

```javascript
p.addEventListener('click', () => {});
```

Список событий(страница):

-   [Загрузка страницы](#event---onload)
-   [Закрытие страницы](#event---onunload)

Список событий(мышь):

-   [Клик по элементу(lmb)](#event---onclick)
-   [Двойной клик по элементу(lmb)](#event---ondblclick)
-   [Кнопка зажата(lmb)](#event---onmousedown)
-   [Кнопка отпущена(lmb)](#event---onmouseup)
-   [Курсор "зашел" на элемент](#event---onmouseover)
-   [Курсор "движется" внутри элемента](#event---onmousemove)
-   [Курсор "вышел" из элемента](#event---onmouseout)

Список событий(фокус):

-   [Элемент получил фокус](#event---onfocus)
-   [Элемент потерял фокус](#event---onblur)

Список событий(формы):

-   [Выделение текста в формк](#event---onselect)
-   [Изменение данных в форме](#event---onchange)
-   [Подтверждение формы](#event---onsubmit)
-   [Сброс(отмена) формы](#event---onreset)

# Базвая структура

Базовая структура:

-   <!DOCTYPE html> - версия DTD(правила XML). Исторически сохранившийся анохронизм
-   <html> ... </html> - указываем что тут документ
-   <head> ... </head> - блок head, помогает браузеру правильно настроить отображение документа
-   <body> ... </body> - содержание страницы

```html
<!DOCTYPE html>
<html>
	<head>
		...
	</head>
	<body>
		...
	</body>
</html>
```

## head

Примеры связанные с head:

-   [Микроформаты/Schema.org/Profile](#tag---head-аттрибут-profile)
-   [Указываем название страницы](#tag---title)

Примеры Meta тегов:

-   [Просим телефон сообзить настоящий размер экрана](#meta---для-телефонов)

Подключаем:

-   [JS внутри HTML](#подключаем---javascript)
-   [JS вне HTML](#подключаем---javascriptвнешний)
-   [CSS внутри HTML](#подключаем---css)
-   [CSS вне HTML](#подключаем---cssвнешний)

## body - семантика

Дело в том, что теги можно поделить на две большие группы:

-   семантические - нужны браузеру, чтобы понимать где что содержиться смысл содержимого в тегах
-   несемантические - нужны только для отображения content пользователю

Теги с семантическим смыслом(смещение внимания):

-   [Аббревиатура в тексте](#tag---abbr)
-   [Цитата в тексте](#tag---q)
-   [Цитата в виде отдельного блока](#tag---blockquote)
-   [Элемнт на котором фокус](#tag---em)
-   [Элемнт привлекающий внимение](#tag---strong)

Теги с семантическим смыслом(пометочки):

-   [Адресс](#tag---address)
-   [Дата или время](#tag---time)
-   [Картинка/видео/аудио](#tag---figure-figcaption)
-   [Прогресс бары](#tag---progress-meter)

Теги с семантическим смыслом(логические блоки):

-   [Блок - content на сайте](#tag---main)
-   [Блок - навигация на сайте](#tag---nav)
-   [Блок - с дополнительной информацией](#tag---aside)
-   [Блок - в конце](#tag---footer)
-   [НЕ самодостаточный блок](#tag---section)
-   [Самодостаточный блок](#tag---article)

## body - контейнеры

Теги-контейнеры:

-   [Контейнер для текста](#tag---span)
-   [Контейнер блок](#tag---div)

## body - cсылки

Ссылки:

-   [Ссылки на страницу + якорные ссылки](#tag---a)
-   [Открываем ссылку в новом окне](#tag---a-как-открывать-ссылку)
-   [Скачиваем файл по ссылке](#tag---a-скачиваем-файл)
-   [Отправляем e-mail](#tag---a-отправка-e-mail)
-   [Используем номер телефона](#tag---a-указываем-telephon)

## body - оформление текста

Теги оформления текста(семантические):

-   [Заголовки](#tag---h1)
-   [Параграф](#tag---p)

Теги оформления текста(несемантические):

-   [Жирный текст](#tag---b)
-   [Наклонный текст](#tag---i)
-   [Подчеркнутый текст](#tag---u)
-   [Верхний индекс](#tag---sup)
-   [Нижний индекс](#tag---sub)
-   [Отформатированный текст](#tag---pre)
-   [Оформление кода - блок](#tag---code)
-   [Оформление кода - переменная](#tag---code)
-   [Оформление кода - пользовательский ввод](#tag---kbd)
-   [Оформление кода - вывод программы](#tag---samp)

Теги-списки:

-   [Нумерованный список](#tag---ol)
-   [Маркированный список](#tag---ul)
-   [Список терминов](#tag---dl)

## body - таблицы

Таблицы:

-   [Создание таблицы](#tag---table-tr-th-td)
-   [Соединение ячеек таблицы](#table---соединяем-ячейки)
-   [Соединение ячеек таблицы](#table---задаем-стиль-столбцам)
-   [Задаем семантику таблице](#table---семантика)
-   [Полный пример таблицы](#table---семантика)

## form

# Entity

| Символ    | Entity name |
| :-------- | :---------- |
| whitespce | `&nbsp;`    |
| <         | `&lt;`      |
| >         | `&gt;`      |
| &         | `&amp;`     |
| "         | `&quot;`    |
| '         | `&apos;`    |
| ¢         | `&cent;`    |
| £         | `&pound;`   |
| ¥         | `&yen;`     |
| €         | `&euro;`    |
| ©         | `&copy;`    |
| ®         | `&reg;`     |

# Примеры

## meta - для телефонов

Ниже предаставленна специальная настройка для мобильных телефонов. Просим браузер телефона не врать о размере своего экрана

Историческая справка: дело в том, что когда интернет только появился, не делали специальные версии сайтов для мобильных телефонов. И чтобы телефоны более вменяемо отображали сайт, необходимо подменить информацию о размере экрана. Что и делали телефонные браузеры

```html
<meta name="viewport" content="width=device-width,initial-scale=1" />
```

## Подключаем - javascript

Скрипт внутри html:

```html
<body>
	<script type="text/javascript">
		// Пишем скрипт прямо внтури html
	</script>
</body>
```

## Подключаем - javascript(внешний)

Скрипт вне html:

```js
<script type="text/javascript" src="path-to-file.js"></script>
```

## Подключаем - CSS

CSS внутри html:

```html
<!DOCTYPE html>
<html>
	<head>
		<style>
			/* Прописываем стили */
		</style>
	</head>
</html>
```

## Подключаем - CSS(внешний)

CSS вне html:

```html
<link rel="stylesheet" href="styles.css" />
```

## Table - соединяем ячейки

Соединяем ячейки:

-   colspan="2" - объединить 2 ячейке в этом стобце
-   rowspan="2" - объединить 2 ячейке в этой строке

```html
<table>
	<tr>
		<th colspan="2">Имя</th>
		<th>Фамилия</th>
		<th>Долг(руб)</th>
	</tr>
	<tr>
		<td>Антон</td>
		<td rowspan="2">Павлов</td>
		<td>100</td>
	</tr>
	<tr>
		<td>Анастасия</td>
		<td></td>
		<td>200</td>
	</tr>
</table>
```

## Table - задаем стиль столбцам

Задаем стиль столбцам:

-   <colgroup> - указание, что сейчас будем описывать стили для столбцов таблицы
-   <col class=""/> - задает стиль для столбца
-   <col span="2" class=""/> - задает стиль для следующих 2 столбцов

```html
<colgroup>
	<col class="" />
	<col span="2" class="" />
	<col class="" />
</colgroup>
```

## Table - семантика

-   <caption> - подпись к таблице
-   <thead> - оборачиваем заголовки. Возможно будет несколько строк заголовков или не строка а столбец
-   <tfoot> - строка внутри этого тега, всего внизу таблицы. Всякие там ИТОГО и т.д.
-   <tbody> - разбивает таблицу на логические части. Может быть несколько штук в таблице

```html
<table>
	<caption>
		TableName
	</caption>
	<colgroup>
		<col class="" />
		<col span="2" class="" />
		<col class="" />
	</colgroup>
	<thead>
		<tr>
			<th></th>
			<th></th>
			<th></th>
			<th></th>
		</tr>
	</thead>
	<tfoot>
		<tr>
			<td></td>
			<td></td>
			<td></td>
			<td></td>
		</tr>
	</tfoot>
	<tbody>
		<tr>
			<td></td>
			<td></td>
			<td></td>
			<td></td>
		</tr>
		<tr>
			<td></td>
			<td></td>
			<td></td>
			<td></td>
		</tr>
	</tbody>
</table>
```

## Attribute - class

Задаем класс

```html
<div class="classname"></div>
```

## Attribute - id

Задаем id

```html
<div id="unique_Identifictor"></div>
```

## Attribute - style

Задаем стиль

```html
<div style="width: 20px;"></div>
```

## Attribute - tabindex

Используется для задания последовательности фокусировки через TAB

```html
<div tabindex="1"></div>
```

## Attribute - accesskey

Комбинация для быстрого доступа к элементу

```html
<div accesskey="c"></div>
```

## Attribute - hidden

Спрятать элемент

```html
<div hidden></div>
```

## Attribute - title

Описание во всплывающей подсказке

```html
<div title="Text"></div>
```

## Attribute - contenteditable

Можно редактировать или нет

```html
<div contenteditable="true"></div>
```

## Attribute - dir

Текст идет слева на право при "ltr" и справа на лево при "rtl"

```html
<div dir="ltr"></div>
```

## Event - onload

Срабатывает по окончанию загрузки страницы:

-   должны загрузиться: html, image, style и scripts

```html
onload
```

## Event - onunload

Срабатывает при закрытие страницы:

```html
<div onunload=""></div>
```

## Event - onclick

Клик левой кнопной мышки:

```html
<div>onclick=""></div>
```

## Event - ondblclick

Двойной клик левой кнопкой мышки:

```html
<div>ondblclick=""></div>
```

## Event - onmousedown

Кнопка нажата:

```html
<div>onmousedown=""></div>
```

## Event - onmouseup

Кнопка отпущена:

```html
<div>onmouseup=""></div>
```

## Event - onmouseover

Мышь "попала" на элемент:

```html
<div>onmouseover=""></div>
```

## Event - onmousemove

Мышь движется находясь на элементе:

```html
<div>onmousemove=""></div>
```

## Event - onmouseout

Мышь "вышла" с элемента:

```html
<div>onmouseout=""></div>
```

## Event - onfocus

Элемент получил фокус:

```html
<div onfocuse=""></div>
```

## Event - onblur

Элемент потерял фокус:

```html
<div onblur=""></div>
```

## Event - onselect

В форме выделен текст:

```html
<input onselect="" />

<textarea onselect=""></textarea>
```

## Event - onchange

В форме изменен текст:

```html
<input onchange="" />
<textarea onchange=""></textarea>
<select onchange=""></select>
```

## Event - onreset

Кнопка reset нажата на форме:

```html
<form onreset=""></form>
```

## Event - onsubmit

Кнопка подтвержления нажата на форме:

```html
<form onsubmit=""></form>
```

№№
Нажата кнопка на клаиатуре

```html
keypress
```

# Tag

## Tag - i

Italics:

-   используется для: иностранных слов, тех терминов, определений

```html
<i>...</i>
```

## Tag - b

Bold:

-   используется для: ключевых слов, названий продуктов

```html
<b>...</b>
```

## Tag - u

Underline:

-   используется для: имен и фамилий, ошибок в написании текста

```html
<u>...</u>
```

## Tag - sub

Нижний индекс:

```html
<sub>...</sub>
```

## Tag - sup

Верхнийи индекс:

```html
<sup>...</sup>
```

## Tag - code

Code - указывает что внутри него код:

-   inline элемент

```html
<code>...</code>
```

## Tag - var

var - указывает что внутри переменная:

```html
<code>
	<var>...</var>
</code>
```

## Tag - kbd

kbd - пользоательский ввод:

```html
<code>
	<kbd>...</kbd>
</code>
```

## Tag - samp

samp - программный вывод:

```html
<code>
	<samp>...</samp>
</code>
```

## Tag - pre

Preformating:

-   Пробелы и переносы строк отображаются на странице, так же как и в коде

```html
<pre>...</pre>
```

## Tag - span

span - текстовый контейнер:

-   inline элемент

```html
<span>...</span>
```

## Tag - div

div - блочный контейнер:

-   block элемент

```html
<div>
	...
	<div></div>
</div>
```

## Tag - ol

ol - Нумерованный список:

```html
<ol>
	<li>Пункт 1</li>
	<li>Пункт 2</li>
</ol>
```

## Tag - ul

ul - Маркироанный список:

```html
<ul>
	<li>Пункт 1</li>
	<li>Пункт 2</li>
</ul>
```

## Tag - dl

Список терминов:

-   dl - блок в котором список
-   dt - название термина
-   dd - определение термина

```html
<dl>
	<dt>Термин 1</dt>
	<dd>Определение термина 1</dd>
	<dt>Термин 2</dt>
	<dd>Определение термина 2</dd>
</dl>
```

## Tag - h1

Тегов заголовка текстов, шесть штук: h1, h2, ... h6:

-   имеют семантический смысл
-   заголовки имеют порядок "заголвковости": h1 > h2 > h3 > ... > h6
    Тексты заголовков

```html
<h1>Главный заголовок</h1>
...
<h6>Самый малый заголовок</h6>
```

## Tag - p

Параграф:

-   имеет семантический смысл

```html
<p>...</p>
```

## Tag - a

Ссылка:

-   href="http//website.com" - аттрибут указывающий куда переходит ссылка
-   href="#toelement" - скролим к элементу с id="toelement"
-   href="http//website.com#toelement" - переходим на http//website.com и скролим к элементу с id="toelement"

```html
<a href="http//website.com">Go to</a>
<a href="#toelement">Scroll to element</a>
<a href="http//website.com#toelement">Go to and Scroll</a>
```

## Tag - a, как открывать ссылку

Ссылка, где аттрибут target принимает одно из 4 значений:

-   "\_blank" - загружает страницу в новом окне
-   "\_self" - загружает страницу в текущем окне
-   "\_parent" - загружает во frame родитель
-   "\_top" - отменяет все frame и загружает в текущем окне браузера

```html
<a href="http//website.com" target="_self"> ... </a>
```

## Tag - a, скачиваем файл

Аттрибут download, указывает на то скачиваем ли мы файл на утсройство:

```html
<a href="http//website.com" download="download"> ... </a>
```

## Tag - a, отправка e-mail

Отправить e-mail:

```html
<a href="mailto:address@mail.ru"> Send message </a>
```

## Tag - a, указываем telephon

Телефон:

```html
<a href="tel:+78121111111">(812) 123-45-67</a
```

## Tag - head, аттрибут profile

У head есть аттрибут profile. Нужен для указания Shcema.org. В современном вебе для указания "микроформатов":

```html
<head profile="SchemeName">
	...
</head>
```

## Tag - title

Указываем подпись страницы. Отображается в браузере, на вкладке:

```html
<title>Заголовок</title>
```

## Tag - em

Элемент на котором фокус внимания:

```html
<em>...</em>
```

## Tag - strong

Элемент привлекающий внимение:

```html
<strong>...</strong>
```

## Tag - abbr

Аббривиатура:

-   по сути это текст со всплывающей подсказкой

```html
<abbr title="Here text">...</abbr>
```

## Tag - address

Аддрес:

-   это может быть адресс: автора веб-страницы, адресс организации и т.д.

```html
<address>...</address>
```

## Tag - q

Цитата(внутри текста):

-   inline-элемент
-   cite="https://www.huxley.net/bnw/four.html" - указывает URL или исходный документ, откуда была взята цитата

```html
<q cite="URL to Author or original">Текст цитаты</q>
```

## Tag - blockquote

Цитата(блок):

-   block-элемент
-   cite="https://www.huxley.net/bnw/four.html" - указывает URL или исходный документ, откуда была взята цитата
-   <p>...</p> - тело цитаты
-   <footer>...</footer> - поясняющий content
-   <cite>Brave New World</cite> - исходный документ, откуда была взята цитата

```html
<blockquote cite="https://www.huxley.net/bnw/four.html">
	<p>
		Words can be like X-rays, if you use them properly—they’ll go through
		anything. You read and you’re pierced.
	</p>
	<footer>—Aldous Huxley, <cite>Brave New World</cite></footer>
</blockquote>
```

## Tag - nav

Блок навигации по сайту:

```html
<nav>...</nav>
```

## Tag - main

main - контент страницы:

-   встречаться на странице должен один раз

```html
<main>...</main>
```

## Tag - article

Часть страницы, которая самодостаточна без остальной страницы:

-   внутри этого тега помещают: пост в блоге

```html
<article>...</article>
```

## Tag - section

Часть страницы, которая НЕ самодостаточна:

-   внутри этого тега помещают: миникарта, блок заголовка и т.д.

```html
<section>...</section>
```

## Tag - aside

Дополнительная информация:

-   внутри этого тега помещают: словарь, бибилография и т.д.

```html
<aside>...</aside>
```

## Tag - footer

Контент в конце страницы или другого блока:

-   внутри этого тега помещают: дата, контакты или правовая информация

```html
<footer>...</footer>
```

## Tag - figure, figcaption

Указывает на медиа-контент + описание:

-   <figure>...</figure> - медиаконтент. То есть картинки видосы и т.д.
-   <figcaption>...</figcaption> - описание медиа-контента

```html
<figure>
	<p><img />...</p>
	<figcaption>...</figcaption>
</figure>
```

## Tag - time

Указывает на дату:

-   datetime="2020-01-20" - пометка для браузера в формате(T означает просто букву T): ГГГГ-ММ-ДДTчч:мм:сс±чч:мм

```html
<time datetime="2020-01-20">20 Juanuary 2020</time>
```

## Tag - progress, meter

Прогресс бар:

-   progress и meter отличаются чисто семантически
-   <progress/> - это прогресс загрузки
-   <meter/> - это измерение чего-либо

```html
<progress value="10" max="100" />
<meter value="75" min="50" max="100" low="70" high="90" />
```

## Tag - table, tr, th, td

Создание таблицы:

-   table - тег-блок, внутри которого таблица
-   tr - тег-строка, внутри которого таблица
-   th - тег-заголовок, создает ячейку в которой content. Обычно перва строка или столбец
-   td - тег-контент, создает ячейку в которой content

```html
<table>
	<tr>
		<th>Имя</th>
		<th>Фамилия</th>
		<th>Долг(руб)</th>
	</tr>
	<tr>
		<td>Антон</td>
		<td>Павлов</td>
		<td>100</td>
	</tr>
</table>
```
