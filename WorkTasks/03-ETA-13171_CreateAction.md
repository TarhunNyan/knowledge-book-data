# ETA-67440 - Тогл не реагирует на привелегию

## Начальное состояние

Элемент интерфейса не реагирует на наличие/отсутствие привелегии:

-   как оказалось не прилетает action
    -   base-info - присылает нам action, а у нас не прилетает нужный action вообще

Проверяем front:

-   становится вопрос о том, а реализовано ли это место на каше
    -   привеленгия есть(sync_exchange_nested_entities), а action не выкидывает

Ползем в Cache:

-   выяснилось, что есть методы(GetAvailableActionCodes), которые проходятся по всем action и тупо проверяют доступен ли action пользователю
-   есть метод(GetActionsNames) который возвращает имена всех action по префиксу внутри класса
    -   у наследников Helpers.StateMachine.StateCollectorBase

Итого:

-   я так полагаю, должен быть реализован action в StateCollectors.ObjectOfControl, потому что в base-info прилетают разрешения ТОЛЬКО оттуда
    -   хотя логически должно лежать в NestedEntityTypeStateCollector

## Война с WebClient

Первый вопрос, ну и где ебучие acations?

-
