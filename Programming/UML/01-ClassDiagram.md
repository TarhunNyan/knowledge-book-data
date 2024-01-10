# Диаграмма классов

Показывает взаимосвзяь классов:

-   какой класс владеет каким классом
-   какой класс знает о каком классе
-   взаимосвязи типа:
    -   один ко многим
    -   многие к одному
-   показвает какие поля и методы есть у класса

## Пример диаграммы классов

Пример диаграммы классов:

<img src="./source/02-ClassDiagram_Example.png" style="display: block; height: 300px; margin: auto;"/>

## Элементы диаграммы классов

Диаграмма классов состоит из элементов:

-   Class
-   AbstractClass
-   Interface
-   Comment

Более подробно смотри на картинке:

<img src="./source/03-ClassDiagram_Elements.png" style="display: block; height: 300px; margin: auto;"/>

## Отношения в диаграмме классов

Отношения в диаграмме классов:

-   Realization - наследование интерфейсов
-   Association - где-то внутри эта штука дрюка используется
-   Dependency - жесткая зависимость, при изменении в объекте, меняется поведение зависимого объекта
-   Aggregation - необязательное Depndecy Injection
-   Composition - строго обязательное Depndecy Injection
-   Generalization/Specialization - наследование

<img src="./source/04-ClassDiagram_Relations.png" style="display: block; height: 300px; margin: auto;"/>

## Кратность в диаграмме классов

Кратность в диаграмме классов:

-   один ко многим
-   один к одному
-   и т.д. и т.п.

<img src="./source/05-ClassDiagram_Multiplicity.png" style="display: block; height: 300px; margin: auto;"/>
