# Как оформлять статьи в "Базе Знаний"

В этой статье - описание того, как вести базу знаний

## Пример файловой структуры

Пример файловой структуры:

```bash
.
└── src
    └── main
        └── resources
            └── db
                ├── changelog
                |   ├── db.changelog-root.xml
                |   ├── db.changelog-1.0.xml
                |   ├── db.changelog-2.0.xml
                |   └── db.changelog-2.1.xml
                ├── DatabasePool.java
                └── AbstractDAO.java
```

## Пример описания навигации по GUI

Пример описания навигации по GUI:

```bash
Areas -> 3D Viewport | /*Top Panel*/ |   View   |   ...         | > Reflection Cubemap |
                                     |   Select |   Light       |   Reflection Plane   |
                                     | > Add    | > Light Probe |   Irradiance Volume  |
                                     |   ...    |   ...         |   ------------------ |
```

## Пример вставки картинок

Просто картинка:

```markdown
<img src="./03-MeasureTheory/00.png"></img>
```

Картинка с выравниванием по центру:

```markdown
<img src="./source/03-Host.png" style="display: block; height: 300px; margin: auto;"/>
```

Три картинки в строку:

```markdown
<img src="./source/float-process-1.svg" style="display: block; height: 100px; float:left;"/>
<img src="./source/float-process-2.svg" style="display: block; height: 100px; float:left;"/>
<img src="./source/float-process-3.svg" style="display: block; height: 100px; float:left;"/>
```
