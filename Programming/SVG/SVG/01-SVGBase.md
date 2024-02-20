# SVG

Все нижеописанные параметры можно так же прописывать внутри класса!

# Элементы

## Rect

Rect - прямоугольник:

-   x / y - позиция по x/y от 0
-   rx / ry - радиусы скругления по x/y
-   width / height - ширина/выысота
-   fill - цвет

```svg
<svg>
    <rect width="200" height="200" fill="gold"/>
    <rect width="200" height="200" fill="#FFAA00" x="0" y="100"/>
    <rect width="200" height="200" fill="#FFAA00" rx="10" ry="20"/>
</svg>
```

## Circle

Circle - круг:

-   r - радиус круга
-   cx / cy - координаты центра круга по x/y
-   fill - цвет

```svg
<svg>
    <circle r="100" fill="gold"/>
    <circle r="100" cx="50%" cy="30%" fill="#FFAA00"/>
</svg>
```

## Ellipse

Ellipse - эллипс:

-   rx / ry - радиус эллипса по x/y
-   fill - цвет

```svg
<svg>
    <ellipse rx="100" ry="50" fill="gold"/>
    <ellipse rx="100" ry="50" fill="#FFAA00"/>
</svg>
```

## Polygon

Polygon - многоугольник по точкам:

-   points - через проблем пишем координаты точек x,y
-   fill - цвет

```svg
<svg>
    <polygon points="100,100 300,100 200,200" fill="gold"/>
</svg>
```

## Line

Line - линия:

-   x1 и y1 / x2 и y2 - координаты первой/второй точки
-   stroke - цвет линии
-   stroke-width - толщина

```svg
<svg>
    <line x1="50" y1="50" x2="250" y2="150" stroke="gold"/>
</svg>
```

## Polyline

Polyline - ломаная линия:

-   points - через проблем пишем координаты точек x,y
-   fill - цвет, если задать то будет тоже что и polygon
-   stroke - цвет линии
-   stroke-width - толщина

```svg
<svg>
    <polyline points="100,100 300,100 200,200" fill="none" stroke="orange"/>
    <polyline points="100,100 300,100 200,200" fill="none" stroke="orange" stroke-width="20"/>
</svg>
```

## Text

Text - текст:

-   points - через проблем пишем координаты точек x,y
-   fill - цвет, если задать то будет тоже что и polygon
-   stroke - цвет линии
-   stroke-width - толщина

```svg
<svg>
    <text class="someText">Some text is here!<text/>
</svg>

<stylesheet>
    .someText {
        font-size: 24px;
        fill: #000;
    }
<stylesheet/>
```

## TextPath

TextPath - позволяет писать текст вдоль любой кривой:

-   href - ссылка на кривую, вдоль которой будет отрисован текст

```svg
<svg>
    <circle r="100" fill="gold" id="path" fill="none" stroke="transparent"/>
    <text class="someText">
        <textPath href="#path">
            Some text is here!
        </textPath>
    <text/>
</svg>
```

## Image

```
<img href="images/image.png" alt="text"/>
```

## ClipPath

ClipPath - позволяет создать маску:

```svg
<svg>
    <clipPath>
        <circle r="100" id="clip_object"/>
    </clipPath>
</svg>

<stylesheet>
    .image {
        width: 600px;
        clip-path: url(#clip_object);
    }
<stylesheet/>
```

Примерыч с использованием clipPath внутри SVG и заполнением элемента по контенту
https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/clipPathUnits

##

stroke-dasharray="50 20" - прерывистая линия
stroke-dashoffset="10" - смещение против часовой
opacity: 0.5 - свойство прозрачности для fill

В css можно указывать по классике относительные пути до элемента:

-   в примере берем элемент с классом apple
-   внутри найденного элемента ищем тег path

```css
.apple path: {
}
```

## SVG path

https://yoksel.github.io/svg-path/

## SVG viewbox

https://yoksel.github.io/svg-sizes/
