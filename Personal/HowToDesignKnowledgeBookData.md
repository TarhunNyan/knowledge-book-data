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
