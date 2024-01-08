# Java библиотеки

Речь идет о стандартных библиотеках

# История Java

Версии до Java8 назывались типа: 1.1, 1.2, 1.3, ..., 1.8, а потом стали называться 9, 10 и т.д.

История Java, какие возможности в какой версии добавили:

-   Java8 - 2014 год
    <!-- -   Добавили возможность default реализации методов внутри interface
    -   Функциональщина
        -   Добавили Lambda выражения
        -   Ссылки на методы и конструкторы
        -   Функциональные интерфейсы
    -   Optional - опциональные значения -->
    -   Stream API
-   Java9 - 2017 год
    -   добавили литералы(быстрое создаение) коллекции, типа of
    -   добавлил пару методов в Optional
    <!-- -   добавили работу с null в класс Objects
        -   речь об операторе elvis -->
    -   добавили целый набор методов в StreamAPI
    -   Появилась возможность перенаправления Stream-ов
    -   Работа с регулярками(?)
    -   Интерфейсы теперь могут иметь методы с модификаторами доступа private, private static
    -   Добавили ProcessHandler который должен заменить Process(работа с запущенными процесвми)
    -   jsaw(Модульность) - теперь можно не компилить то, что не используется(лол, а раньше видимо не так было)
-   Java10 - 2018 год
    <!-- -   var - теперь можно не указывать тип, java через ключевое слово var сама поймет какой тип у переменной -->
    -   parallel garbage collector - сборщик мусора в нескольких потоках
    -   добавлен Application class-data sharing
        -   штука для борьбы с проблемой, когда в нескольких class-loader есть несколько экземпляров одного класса
        -   в общем я без понятия что это
    -   добавили JIT клмпилятор(в runtime компилирует часть кода в код для native платформы, типа windows или linux)
-   Java11 - 2018 год
    -   псоледняя LTS версия(на текущий момент)
    -   var для lambda(?)
    -   HttpClient API был стандартизован
    -   Удалили JavaEE и Corba - убрали
    -   Добавили некоторые методы в String
    -   ZGC - новый Garbage collector запускаемый с ключами
-   Java12 -
    -   Long-time-garbage-collector - новый garbage collector
    -   новый синтаксис для Seitch(пиздатый)
    -   garbage collector - при долгой работе garbage collector машина java его может прерывать на какое-то время
    -   garbage collector - неиспользуемая часть кода возвращается в систему
    -   micrbanchmark suite - можно теперь через встроенный инструмент профилировать код
    -   default cds archive - улучшение модульности, теперь можно собирать только нужной кусок java машины
-   Java13 - 2019 год
    -   ZGC - улучшен
    -   cds - улучшен
    -   legacy socket api - переписан, теперь работает на java, а не на нативных вызовах
    -   switch - теперь можно присваивать переменной и использовать yeild для возвращения результата работы switch, в режиме preview
    -   text-block - текстовый блок в тройных кавычках. Только в режиме preview(то есть надо настраивать флаги)
-   Java14 - 2020
    <!-- -   record - добавили record, но только в режиме preview -->
    -   NullPointerException - более подробный staketrace, в режиме preview, конкретно показывает, в каком месте строки был вызван NullPointerException
    -   text-block - добавили возможность использовать пустые строки, литерали и т.д.
    <!-- -   switch - присвоение переменной теперь возможно без preview мода -->
    -   instanceof - поменяли синтаксис, в preview режиме
    -   foreign memory access api - можно теперь получать доступ к памяти других процессов(вообще не безопасно)
-   Java15 - 2020
    -   text-block - больше не preview фича
    -   sealed классы - в режиме preview. Классы от которых могут расширяться только назаванные классы
    -   ZGC - теперь включен по умолчанию, вышел из preview мода. Обещают работу менее 10мс
    -   добавлен еще один garbage collector (какой-то shanon-do)
    -   удалили порты Solaris и Spark из корневых библиотек (это древние ОС)
    -   добавили скрытые классы - не видиен в staketrace, не виден для reflection api, для разрабов библиотек
    -   удалили java nashorn js - даже хз что это
    -