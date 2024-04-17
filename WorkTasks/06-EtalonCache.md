# Etalon Cache

Исследуем то, как устроен Эталон

# Общая структура

## Gateway

../Gateways/(.\*)Gateway - файлы в которых опиывается CRUD какой-то [Entity](#entities)

-   (.\*) - имя [Entity](#entities) которую описываем
    -   полный путь до [Entity](#entities) с которой работаем лежит в поле EntityClassName класса ../Gateways/(.\*)Gateway
-   ../Gateways/(.\*)Gateway наследуются от SOU.Gateways.Base.GatewayBase
    -   SOU.Gateways.Base.GatewayBase наследуются от %RegisteredObject

## Entities

../Entities/(.\*) - описание сущности в базе:

-   наследуется от %Persistent

# Структура пакетов

## Organization

Organizaition - организация, ее структура, информация о работниках(не всегда пользователь является работником):

-   хранит в себе информацию о работниках организации
    -   UserTraining/UserStructure/UserEducation/UserCareer
-   ветки организации(CompanyBranch), но это не точно
-   работ над целями(?)
    -   Objective/ObjectiveDirections/ObjectiveFile/ObjectiveSet/ObjectiveSetFile

## Integration

/SOU/Integration - интеграции с другими сервисами, например:

-   Seldon - сервис предоставляющий информацию по закупкам/контрагентам/ценам/контркатам и т.д.
-   asv/АСВ - Агенство Страхования Вкладов
-   EIS/ЕИС - Единая Информационная Система(в сфере закупок)
-   google - че-то пусто там
-   и т.д.

## Indicators

/SOU/Indicators - показатели?

# Endpoints

## Code

Как работает code и filter в sit-auto-complete:

-   Есть директива, <sis-auto-complete>
    -   директива лежит в ngx-shared, вот тут: projects/lib/src/inputs/auto-complete/auto-complete-data-provider.directive.ts
        -   внутри директивы инжектится AutoCompleteDialogDataProviderService, который определен в эталоне: src/app/core/data-providers/auto-complete-dialog-data-provider.service.ts
            -   prepareDialogData - подгатавливает config по code
            -   в поле code лежит мапинг, который из SearchEntity формирует config
                -   SearchEntity мапится из src/app/constants/core/search-entity.enum.ts
            -   prepareDictionaryFilters - подглотовка фильтров в запросе
            -   getJournalSelectionConfig - добавляет фильтры к запросу
                -   по сути просто все поля объекта пихает в поле additionalFilters у dto-шки которую отправляют на сервер
    -   у дериктивы есть поле code, которое мапится из src/app/constants/core/search-entity.enum.ts

## Filter

Для endpoint-а можно настроить фильтр. То есть, с frontend приходит какой-то объект фильтр, и мы работаем с ним на беке. На каше это выглядит так:

-   request - просто какой-то json с фронта
-   ToComboBoxFilterDTO - метод у нашего DTO который умеет доставать фильтры
-   d filter.FillFilterValue("IsNeededStateCollector", .isNeededStateCollector) - общепринятая конструкция
    -   d filter.FillFilterValue - метод который достает значение фильтра и кладет в переменную
    -   .isNeededStateCollector - переменная в которую положили значение фильтра

```cache
ClassMethod GetEmployeesBySearchRequest(..., request As SOU.ETA.Core.REST.DTO.SearchRequestJSONDTO) As ... {
    #dim filter As SOU.ETA.Core.DTO.ComboBoxFilterDTO = request.ToComboBoxFilterDTO()
	#dim isNeededStateCollector As %Boolean
	d filter.FillFilterValue("IsNeededStateCollector", .isNeededStateCollector)
}
```

На фронте это выглядит так:

```js

```

# Jetalon

## MetaEntity

eav.meta_entity - хранит в себе все "объекты системы"(вложки, ок, типы ок, типы вложек и т.д.):

-   type - указывает тип "объекта системы", посмотреть что туда пихают можно в DynamicObjectResourceName(либы)
    -   dynamic-entity
        -   control.control_object_type - "тип ОК"
        -   control.control_object_type_meta_entity - связь control.control_object_type с eav.meta_entity
    -   nested-entity-object
        -   ВлС
    -   nested-entity-object-column
        -   ссылка на ВлС в таблице
    -   nested-entity-type
        -   тип ВлС
    -   activity-node-type
        -   тип этапа сетевого графика
    -   loaded-meta-entity-version
        -   версия метасущности
    -   meta-entity
        -   метасущность
    -   meta-entity-attribute
        -   атрибут версии метасущности
-   UUID это основной параметр задаваемый объекту
    -   в зависимости от типа становится UUID для "типа ОК"/"ВлС"/"типа ВлС"/...
-   name - имя "объекта системы"
