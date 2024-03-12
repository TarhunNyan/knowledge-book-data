# ETA-67440 - Добавить кнопку в etalon

Нужно в окне "Мои отчеты", добавить кнопку. Для строк(отчетов) в состоянии "В ПРОЦЕССЕ" или "В ОЧЕРЕДЕ", нужно добавить кнопку с выпадающим меню, в котором будет пункт прервать

# Создание кнопки на фронте

Где надо подредактировать:

-   webClient/src/app/
    -   constants/
        -   core/resource-name.ts
        -   staff/sql-report-action.enum.ts
    -   staff/
        -   services/main-report.service.ts
        -   staff-routing.module.ts
        -   staff.module.ts

## Что приходит на фронт, в журнал?

С точки зрения фронта, есть компонент "журнал", в который приходит json с сервера:

-   Исходя из json, а точнее объектов в поле recordList, фронтом формируются строки в "журнале"
    -   actions - поле по объектам из которого, создаются кнопки для строк в журнале
    -   entityLink -> resourceLocation -> resourceName - имя ресурса
        -   по имени ресурса фронт определяет что нужно вывести
        -   по имени ресурса можно найти на беке место где он создается
        -   sql-report - пример имени ресурса

## Регестрируем ресурс на фронте

На фронте регестрируем ресурс:

-   webClient/src/app/constants/core/resource-name.ts - файл в котором происходит регестрация ресурса
-   sql-report - имя ресурса, которое будет приходить с бэка. Регестрируем его, чтобы с ним работать в дальнейшем

```typescript
static SqlReport = new Resource('sql-report', {});
```

## Регестрируем action на фронте

Создаем enum в котором перечисленны action используемые ресурсом. Для многих сущностей уже есть свои enum с кучей action-ов, у нашей не было. Поэтому создаем свой(правда пример только с одним action):

-   webClient/src/app/constants/staff/sql-report-action.enum.ts - файл который создали, для создания кнопок с иконкой и нужным action
-   'Interrupt' - название action приходящее с бэка
-   new SqlReportAction('Interrupt'); - enum в котором список действий для этого журнала(?). Регестрируем через конструктор, но наверное стоило его прописать прямо в enum'е

```typescript
export class SqlReportAction extends BaseAction {
    static store: EnumValuesStorage<SqlReportAction> =
        new EnumValuesStorage();

    static Interrupt = new SqlReportAction('Interrupt');

    private constructor(code: string, icon?: IconType) {
        super(Resource.SqlReport.code, code, icon);
        SqlReportAction.store.set(code, this);
    }
}
```

## Задаем поведение для actions

Создаем service, в котором обрабатываем поведение всех action для журнала:

-   webClient/src/app/staff/services/main-report.service.ts - файл который создали, для
-   CommonDialogsService - стандартное диалоговое окно, с текстом и выбором типа да/нет
-   SqlReportAction.Interrupt.code - код action, который обрабатываем черз case, может быть обработанно несколько кодов
-   journalName: JournalName.ReportJournal - журнал для которого описываем поведение

```typescript
@Injectable({ providedIn: 'root' })
export class MainReportService
    extends AbstractActionHandler
    implements JournalConfigurationProvider
{
    constructor(
        private profile: UserProfileService,
        private commonDialogs: CommonDialogsService,
        private backend: ReportControllerBackendService
    ) {
        super([SqlReportAction.Interrupt]);
    }

    getConfiguration(
        journalKey?: string
    ): Observable<ProvidedJournalConfiguration> {
        return of({
            journalName: JournalName.ReportJournal,
            actionsEnabled: true,
            journalMode: 'standalone',
            title: $localize`Мои отчеты`,
        });
    }

    execute(
        link: ResourceLink,
        action: Action
    ): Promise<boolean | void> {
        switch (action.code) {
            case SqlReportAction.Interrupt.code:
                return this.reportInterrupt(link);

            default:
                throw new Error(
                    `Action ${action.code} not implemented.`
                );
        }
    }

    reportInterrupt(link: ResourceLink): Promise<boolean> {
        return this.commonDialogs
            .confirm({
                text:
                    $localize`Вы действительно хотите прервать формирование отчета по шаблону ` +
                    link.name,
            })
            .closed.then(() =>
                lastValueFrom(
                    this.backend.interruptReportGeneration(link.id)
                )
            )
            .then(() => true);
    }
}
```

## Указываем routing для журнала

Указываем routing для журнала. Журналы могут быть статическими(кнопок нет) и динамическими

-   журналы могут быть
    -   статическими - кнопок нет
    -   динамическими - кнопки есть
-   webClient/src/app/staff/staff-routing.module.ts

Пример статики:

```typescript
{
    path: 'reports',
    component: JournalComponent,
    data: { journalName: JournalName.ReportJournal, title: $localize`Мои отчеты`, journalMode: 'standalone' },
    } as JournalStaticRoute,
```

Пример динамики(без динамики кнопки не появятся):

```typescript
{
    path: 'reports',
    component: JournalComponent,
    data: { configurationProvider: MainReportService },
} as JournalDynamicRoute,
```

## Подключаем action в typescript

Подключаем action в typescript:

-   webClient/src/app/staff/staff.module.ts - модуль в котором врубается весь typescript

```typescript

@NgModule({
            imports: [
              ...
            ],
            declarations: [
              ...
            ],
            providers: [
              ...
              { provide: ACTIONS_HANDLERS, useExisting: MainReportService, multi: true },
              ...
            ],
          })
export class StaffModule {
```

# Создание кнопки на беке Cache

Файлы которые тронули на Cache:

-   classes/SOU/
    -   ETA/
        -   Core/REST/DTO/
            -   ReportRecordJSONDTO.xml
            -   Converters/ResourceLinkJSONDTO/SQLReportConverter.xml
        -   Reports/
            -   Gateways/Report.xml
            -   StateCollectors/ReportStateCollector.xml
            -   REST/ReportRouter.xml
            -   REST/Controllers/ReportController.xml
            -   BL/Operations/InterruptReport.xml
    -   RESTRouter.xml
    -   Security.xml

Логика работы следующая:

-   webClient делает запрос на объекты которыми он заполнит журнал
    -   проверяем нужно ли добавлять action через Collector
    -   добавляем action в json
-   ждем запрос от клиента по API
    -   ловим по зарегестрированному Rest нужный запрос
    -   по ID запускаем controller
    -   controller запускает operation
        -   operation запускает collector для проверки, можем ли выполнить запрос
        -   operation выполняет нужное действие
    -   controller возвращает json для webClient

## Регистрируем ресурс

Регситрируем ресурс - этот ресурс потом выдается пользователю. По наличию ресурса мы производим проверку доступа у пользователя к действию:

-   classes/SOU/Update/Security.xml - путь до класса создающего ресурсы и роли
-   /// Ресурс прерывания генерации отчета по шаблону
    -   это не комментарий, а специальная мекта которая должна совпадать с описание в методе, где регестрируется ресурс
-   Parameter ResourceInterruptGenerateReport = "interrupt_generate_report";
    -   задаем рабочее название рессурсу, под которым он будет обтображаться
-   $$
    MakeResource(..#ResourceInterruptGenerateReport, "Прерывание генерации отчета по шаблону")
      - регестрируем ресурс, теперь мы его можем назначить роли
    $$
-   $$$AddResource($$$U(..#ResourceReadSqlReportType))
    -   добавляем зарегестрированный ресурс роли(работает как задание ресурса по умолчанию)

```cache
Class SOU.Update.Security Extends %RegisteredObject {
    ...
    /// Ресурс прерывания генерации отчета по шаблону
    Parameter ResourceInterruptGenerateReport = "interrupt_generate_report";
    ...

    Method UpdateRoles()
        {
            ...
            $$$StartRoleBuild("eta_servadmin", "Сервисный администратор СОУ Эталон")
                ...
                $$$AddResource($$$U(..#ResourceReadSqlReportType))
                ...
            $$$FinishRoleBuild
            ...
        }
    ...
    Method UpdateResources()
    {
        ...
        $$$MakeResource(..#ResourceInterruptGenerateReport, "Прерывание генерации отчета по шаблону")
        ...
    }
    ...
}
```

## Добавляем информацию о кнопке в JSON

Добавляем информацию о кнопке в JSON:

-   classes/SOU/ETA/Core/REST/DTO/ReportRecordJSONDTO.xml
-   dto.Actions - ключевой момент. Добавляем в DTO поле actions, куда запихиваем описание кнопки
-   stateCollector - нами созданный stateCollector

```cache
Class SOU.ETA.Core.REST.DTO.ReportRecordJSONDTO Extends (Helpers.Utils.Swagger.ApiModel, SOU.ETA.View.REST.DTO.BaseJournalRecordJSONDTO)
{
...
ClassMethod ConvertToTransport(entity As SOU.ETA.Reports.Entities.Report) As SOU.ETA.Core.REST.DTO.ReportRecordJSONDTO
    {
        $$$AssertObject(entity, $$$SOUError("32541"))
        s dto = ..%New()

        ...

        s stateCollector = ##class(SOU.ETA.Reports.StateCollectors.ReportStateCollector).%New(entity)
        s dto.Actions = ##class(SOU.ETA.Core.REST.DTO.ActionsListJSONDTO).ConvertToTransport(stateCollector.GetActionsInfo()).ActionsList

        q dto
    }
}
```

## JSON конвертеры

На фронте, прилетают по нажатию кнопки не все данные, а только сформированные в конвертере(хз почему так)

-   classes/SOU/ETA/Core/REST/DTO/Converters/ResourceLinkJSONDTO/SQLReportConverter.xml - пример конвертера котороый я правил

## Правим Gateway

В Gateway прячутся всякие связанные с сущностями методы

-   classes/SOU/ETA/Reports/Gateways/Report.xml - путь до gateway который я правил
-   ниже примеры методов, которые пихают в gateway

```cache
ClassMethod GetRunningReports(type As %String = "") As Helpers.Utils.QueryObject
{
	#dim qo As Helpers.Utils.QueryObject
	s qo = ..GetQm()
	d qo.Where("Type", type, $$$Not(type=""))
	d qo.Where("State->Code", ##class(SOU.ETA.Reports.Entities.Report).#StateRunning)
	d qo.Order("CreatedOn ASC")

	q qo
}

ClassMethod IsInterruptible(report As SOU.ETA.Reports.Entities.Report) As %Boolean
{
	$$$AssertObject(report, $$$SOUError("90004"))
	s finishedStatus = $LB(##class(SOU.ETA.Reports.Entities.Report).#StateError,##class(SOU.ETA.Reports.Entities.Report).#StateTerminated,##class(SOU.ETA.Reports.Entities.Report).#StateFinished)

	ret $$$Not($$$InList(finishedStatus, report.State.Code))
}
```

## Создаем Collector

Создаем Collector - хранит в себе имя кнопки, перевод и условия при которых этот action становиться доступен

-   classes/SOU/ETA/Reports/StateCollectors/ReportStateCollector.xml
-   my-reports-journal-action-interrupt - метка для перевода
-   ActionInterrupt - проверка условий
-   Вообще collector работает на магии Helpers.StateMachine.StateCollectorBase

```cache
Class SOU.ETA.Reports.StateCollectors.ReportStateCollector Extends Helpers.StateMachine.StateCollectorBase {
    Parameter ObjectClassName = "SOU.ETA.Reports.Entities.Report";

    Property Object As SOU.ETA.Reports.Entities.Report;

    /// Прервать
    Parameter ActionInterrupt = "Interrupt";

    /// @localName = Прервать
    /// @localCode = my-reports-journal-action-interrupt
    Method ActionInterrupt()
    {
        $$$StartScope
            $$$Condition(..ConditionReportInProcessGeneration)
            $$$Condition(..ConditionUserHasInterruptGenerateReportResource)
        $$$FinishScope
    }

    Method ConditionReportInProcessGeneration(condition) As %Boolean
    {
        q ##class(SOU.ETA.Reports.Gateways.Report).IsInterruptible(..Object)
    }

    Method ConditionUserHasInterruptGenerateReportResource()
    {
        q ##class(SOU.Security.AccessHelper).CurrentUserHasResource(##class(SOU.Update.Security).#ResourceInterruptGenerateReport, "U")
    }
}
```

## Создаем Route

Регестрируем prefix для нашего api:

-   classes/SOU/RESTRouter.xml - регестрируем prefix, ты поймешь куда его надо воткнуть
-   Forward="SOU.ETA.Reports.REST.ReportRouter" - в этом классе api

```cache
<Map Prefix="/reports" Forward="SOU.ETA.Reports.REST.ReportRouter"/>
```

В таких файлах регистрируются routes, для API. Обычно их много, в моем случае route всего один:

-   classes/SOU/ETA/Reports/REST/ReportRouter.xml - путь до rest-запроса, который
-   RouterPrefix = "/reports" - префикс относительно которого смотрим rest запрос
-   Url="/:id/interrupt" - рест запрос по которому получаем id и вызваем прерываение
-   SOU.ETA.Reports.REST.Controllers.ReportController:InvokeInterruptReportGeneration - метод который вызываем по переходу по route

```cache
Include CommonFuncs

Class SOU.ETA.Reports.REST.ReportRouter Extends SOU.ETA.Core.REST.Routers.BaseProxyRouter {

Parameter RouterPrefix = "/reports";

XData UrlMap
    {
        <Routes>
            <Route Url="/:id/interrupt" Method="PUT" Call="SOU.ETA.Reports.REST.Controllers.ReportController:InvokeInterruptReportGeneration"/>
        </Routes>
    }
}
```

## Создаем Controller

Создаем Controller - задача контроллера, вернуть какой-то json

-   classes/SOU/ETA/Reports/REST/Controllers/ReportController.xml - класс с Controller
-   контроллер обычно вызывает какой-то operations
    -   в operations лежит логика
-   задача контроллера, вернуть какой-то json
-   в примере Operation сам создает json, и мы его просто возвращаем

```cache
Class SOU.ETA.Reports.REST.Controllers.ReportController Extends SOU.ETA.Core.REST.ControllerBase {

    ClassMethod InterruptReportGeneration(id As %String) {
        #dim operation As SOU.ETA.Reports.BL.Operations.InterruptReport
        s operation = ##class(SOU.ETA.Reports.BL.Operations.InterruptReport).Create(id)
        d operation.Execute()
    }
}
```

## Создаем Operation

Создаем Operation - задача operation, выполнить какую-то логику, которая слишком жирная, чтобы пихать ее в controller

-   classes/SOU/ETA/Reports/BL/Operations/InterruptReport.xml - класс с нашим operation
-   в примере весь класс написан с нуля
-   Helpers.ClassTemplates.Operations.Transactional.Auditable.Base
    -   класс от которого наследуются все operation
    -   включает в себя магию вызова методов
        -   Create - метод который вызвается в controller и создает нащ operation
        -   DoAction - то что мы хотим сделать
        -   GetResult - то что мы вернем в Controller
        -   OnCheckSecurity - метод который НАДО определить, чтобы если нас дернули через postman, мы проверили, а кто это нас дернул
            -   d stateCollector.StaticCheckAction(..Report, action, 1) - магический метод
                -   магический метод, с помощью которого вызовем проверку метода из Collector
                    -   в нашем примере, проверка лежит в методе ActionInterrupt
                -   stateCollector - наш collector, в котором есть проверка
                -   ..Report - поле в котором лежат данные котороые мы хотим вернуть(вообще хз зачем это)
                -   action - имя action, который вызываем
                -   1 - указываем, что надо выдать warning, если проврека не пройденна
        -   OnBeforeAction - вызывается перед DoAction и в примере нужна чтобы проверит статус прерывания

```cache
Class SOU.ETA.Reports.BL.Operations.InterruptReport Extends Helpers.ClassTemplates.Operations.Transactional.Auditable.Base {
    Property Result As %RegisteredObject;

    Property Report As SOU.ETA.Reports.Entities.Report;

    ClassMethod Create(id As %String) As SOU.ETA.Reports.BL.Operations.InterruptReport
    {
        $$$AssertNotNull(id, $$$SOUError("90001"))

        #dim op As SOU.ETA.Reports.BL.Operations.InterruptReport
        s op = ##class(SOU.ETA.Reports.BL.Operations.InterruptReport).%New()
        d op.FillObject(id)

        q op
    }

    Method FillObject(id As %String)
    {
        s ..Report = ##class(SOU.ETA.Reports.Gateways.Report).GetById(id)
    }

    Method OnBeforeAction()
    {
        d ..CheckReportStatus()
    }

    Method OnCheckSecurity()
    {
        #dim stateCollector As SOU.ETA.Reports.StateCollectors.ReportStateCollector
        s stateCollector = ##class(SOU.ETA.Reports.StateCollectors.ReportStateCollector).%New()
        s action = ..GetOperationAction()

        d stateCollector.StaticCheckAction(..Report, action, 1)
    }

    Method DoAction()
    {
        d ##class(SOU.ETA.Reports.BL.ReportManager).TerminateReport(..Report)
    }

    Method GetOperationAction() As %String
    {
        return ##class(SOU.ETA.Reports.StateCollectors.ReportStateCollector).#ActionInterrupt
    }

    Method GetResult()
    {
        return ..Result
    }

    Method CheckReportStatus()
    {
        if $$$Not(##class(SOU.ETA.Reports.Gateways.Report).IsInterruptible(..Report)) {
            s state = ..Report.State.Name
            throw $$$SouExc($$$SOUError("90002"),$$$Format("my-reports-journal-interrupt-report-error", state))
        }
    }
}
```
