# Rails

Специальный фреймворк для Backend

-   [Создание проекта](#console---создание-проекта)
-   [Запуск локального сервера](#console---запуск-локально)
-   [Список routes в проекте](#console---список-route)
-   [Вызов дебагера](#debugger)
-   [Структура файлов и папок проекта](#debugger)

# Tests

Тесты поддерживаются в rails по умолчанию. Все валяются в директории /test

Примеры:

-   [](#)

## Fixtures

При исползовании тестов, создается тестовая база данных. Она заполняется значениями, которые прописаны в /test/fixtures. После завершения тестов откатывается к реальной. Пример:

```yaml
rubyonrails:
    id: 1
    name: Ruby on Rails
    url: http://www.rubyonrails.org

google:
    id: 2
    name: Google
    url: http://www.google.com
```

Также для Fixture можно использовать erb:

```erb
<%Q1.upto(1000) do |i| %Q<
fix_<%= i %>:
  id: <%= i %>
  name: guy_<%= 1 %>
<% end %>
```

## has_one/has_many/belongs_to

Это классические связи в SQL:

-   has_one - один к одному
-   has_many - один ко многим
-   belongs_to - принадлежит к...

И так, у тебя есть Model полльзвателя(user), карты(card) и заказов(order). Тогда в Model, можно прописать отношения между таблицами:

-   в Model карты(card) и заказов(order) поле user_id, будет во многих случаях заполняться автоматически
-   после изменений в /app/model не забудь запустить миграцию

```ruby
# Для файла /app/models/user.rb
class User < ApplicationRecord
    ...
    has_one :card
    has_many :orders
    ...
end

# Для файла /app/models/order.rb
class Order < ApplicationRecord
    ...
    belongs_to :user
    ...
end
```

## Отправка post/get запросов

Внутри тестов можно отправлять запросы на сервер. Отправка get запроса:

```ruby
require "test_helper"

class RequestsControllerTest < ActionDispatch::IntegrationTest
    test "get test" do
        get "/requests"
    end
end
```

Отправка post запроса:

```ruby
require "test_helper"

class RequestsControllerTest < ActionDispatch::IntegrationTest
    test "post test" do
        post "/requests", params: { requests: { user_id:1, body: "text" }}
    end
end
```

# Models

Объекты, которые предоставляют удобный интерфейс к базе данных по принципу: 1 Model => 1 таблица БД

Просмотреть структуру БД можно в:

```bash
/db/scheme.rb
```

## Migration/Работа с БД

Migration - файлы с ruby кодом, которые приводят БД в соответствовии с моделью использемой в коде. То есть настраивают структуру самой БД:

-   Файлы миграции - история изменений СТРУКТУРЫ БД
-   Файлы миграции в названии хранят дату миграции, чтобы знать в каком порядке их накатывать
-   Сами миграции менять не стоит, облажаешся так, что не исправишь потом

Примеры:

-   [Создать Model](#generator---model)
-   [Удалить Model](#generator---destroy-model)
-   [Применить миграции](#migration---применить-миграции)
-   [Откатить миграцию](#migration---откатить-миграцию)
-   [Добавить поле в БД](#migration---добавить-поле)
-   [Изменить поле в БД](#migration---изменить-тип-поля)
-   [Удалить поле в БД](#migration---удалить-поле)

## CRUD

CRUD - аббривиатура, расшифровывается как: Create Read Update Delete. Под CRUD понимают механизм, который позволяет избежать взаимодействие с БД через SQL, а вместо этого использовать Ruby:

-   CRUD реализуется через gem с название ActiveRecord
-   ActiveRecord позволяет написать один код, для разных БД, и потом если надо, поменять БД не меняя код
-   По умному, то что раелизует ActiveRecord называется ORM - object-relation mapping(превратить объект в строку таблицы и обратное преобразование)

Пример:

-   [Create используя болванку](#crud-create---через-болванку)
-   [Create через функцию create](#crud-create)
-   [Read - основные методы](#crud-read)
-   [Read - одна запись](#crud-read---одна-запись)
-   [Read - множество записей](#crud-read---множество-записей)
-   [Read - получить последнюю запись](#crud-read---последняя-запись)
-   [Read - получить все записи](#crud-read---все-записи)
-   [Update - основные методы](#crud-update)
-   [Delete - основные методы](#crud-delete)

## Validation

Пример валидации для файла /app/models/user.rb:

-   валидация данных происходит при попытке сделать запись в бд

```ruby
class User < ApplicationRecord
  # Проверка на наличие поля (обязательное поле)
  validates :email, presence: true

  # Проверка на длину
  validates :name, length: { maximum: 15 }

  # Проверка на соответствие регулярке
  validates :nickname, format: { with: /\A[a-z_]+\z/ }

  # проверка email на уникальность в БД
  validates :email, uniqueness: true

  # проверка сразу нескольких условий
  validates :email, uniqueness: true, presence: true
end
```

Можно прямо кодом посмотреть проходит ли запись валидацию:

```ruby
# Создаем запись
u = User.create()
# Смотрим, проходит ли валидацию
u.valid?
# => false
```

Просмотреть ошибки валидации:

```ruby
# Создаем запись
u = User.create()
# Смотрим, ошибки
u.errors
# => <ActiveModel::Errors [#<ActiveModel::Error attribute=password, type=blank, options={}]>
```

Получить человекочитаемые ошибки валидации:

-   только на английском

```ruby
u.errors.full_message
# => ["Password can't be blank"]
```

При сохранении в БД, функция save возвращает true или false, в зависимости от того прошли ли данные проверку на валидацию или нет. Один из вариантов это использовать:

```erb
<% if user.save == true %>
    # валидация прошла
<% else %>
    # валидация не прошла
<% end %>
```

Просьба ввести пароль еще раз. Если у нас есть has_secure_password, то подключается "Rails Magic". Можно просто создать в форме поле:

```ruby
:password_confirmation
```

## Callbacks

Коллбеков много, вот список некоторых:

-   before_validation - срабатывает перед отправкой данных в БД
-   after_validation - после валидации, но до записи в БД
-   before_create - перед созданием записи( не срабатывает при обновлении)
-   before_save - перед созданием/обновлением записи
-   after_commit - после попадания(до сохранения) записи в БД
-   after_save - после совершения записи в БД

Пример написания Callback:

```ruby
class CreditRequest < ApplicationRecord
    after_save :calculate_score

    ...

    def calculate_score
        @question = Question.find(params[:id])
    end

    ...
end
```

## Assosiations

# Action(Controller)

Controller - отвечают за передачу данных в БД. Прослойка между View и Model. Обычно, организованны по принципу: 1 Model => 1 Controller

Action - код выполняющий какие-то действия в ответ на конкретный запрос. По сути:

-   Controller - класс наследуемый от ApplicationController
-   Action - метод класса Controller

## Controller

Все Controller наследуются от ApplicationController. Добавив новый метод в ApplcationController, мы добваим одинаковый Action во все Controller

Примеры:

-   [Создать Controller](#generator---controller)
-   [Создать Action для всех Controller](#controller---создать-action-для-всех-controller)
-   [Создать helper в Controller](#controller---создать-helper)

## Action

Метод Controller. Добавляет какие-то данные в БД и отправляет пользователю ответ. Параметры доступные внутри Action:

-   request - запрос от пользователя
    -   request.method - метод запроса
    -   request.path - route запроса
    -   request.headers - заголовок запроса
        -   request.headers[:host] - имя хоста(например: google.com)
-   params - параметры отправленные из формы, текущий Controller и текущий Action

    -   params[:body] - параметр :body, если был такой отправлен
    -   params[:action] - текущий Action
    -   params[:controller] - текущий Controller

Примеры:

-   [Правильное получение параметров](#action---permitted_params)
-   [Убрать требование Token'а](#action---отключить-token)
-   [Ответ пользователю: default](#action---ответ-пользователю-default)
-   [Ответ пользователю: render](#action---ответ-пользователю-render)
-   [Ответ пользователю: redirect_to](#action---ответ-пользователю-redirect_to)

## Flash

Можно передавать данные для того же пользователя. Работает через cookie, просто упрощенный инструмент

Примеры:

-   [Заполнение Flash данными и передача его](#flash---заполнение)
-   [Получение Flash](#flash---получение)
-   [Flash - решение проблемы при использовании с render](#flash---решение-проблемы-при-использовании-с-render)

## Cookie

Cookie - чтобы при переходе между страницами сайта сохранять данные, используются Cookie

Примеры:

-   [О Cookie](#cookie---информация)
-   [Добавление данных в Cookie](#cookie---добавление-данных)
-   [Просмотр данных из Cookie](#cookie---чтение-данных)

## REST

Вот такие URL пути одобряет REST:

-   Пример основан на создание аккаунта
-   Вместо 1 имеется ввиду какое-то id

| Действие                           | Метод  | Путь          | Controller#экшен |
| :--------------------------------- | :----- | :------------ | :--------------- |
| Создание аккаунта                  | POST   | /users        | users#create     |
| Форма создания аккаунта            | GET    | /users/new    | users#new        |
| Форма редактирования аккаунта      | GET    | /users/1/edit | users#edit       |
| Просмотр аккаунта                  | GET    | /users/1      | users#show       |
| Изменения аккаунта                 | PATCH  | /users/1      | users#update     |
| Удаление аккаунта                  | DELETE | /users/1      | users#destroy    |
| Получить список/несколько аккаунта | GET    | /users        | users#index      |

-   [REST - быстрое создание всех путей](#rest---быстрое-создание-всех-путей)
-   [REST - быстрое создание определенных путей](#rest---быстрое-создание-определенных-путей)
-   [REST - быстрое создание путей безсписочных объектов](#rest---быстрое-создание-путей-безсписочных-объектов)

## Route

Route - наборы путей, по которому происходят вызовы и то какие Action эти вызовы дергают

-   [Определить куда ведет root запрос](#route---прописать-root)
-   [Определяем куда ведет URL запрос](#route---создаем-route)
-   [Создание(всех) путей по REST методологии](#rest---быстрое-создание-всех-путей)
-   [Создание(всех) путей для безсписочных объектов по REST методологии](#rest---быстрое-создание-путей-безсписочных-объектов)
-   [Создание(выборочно) путей по REST методологии](#rest---быстрое-создание-определенных-путей)

## Before_action

Перед выполнением action можно задать функцию, которая исполниться перед ним. Это нужно например, для получение каких-то значенией из БД

-   [before_action - перед всеми action](#before_action---перед-всеми-action)
-   [before_action - перед конкретными action](#before_action---перед-конкретными-action)

# Views

Шаблоны для html страниц написанные на erb. Лежат в:

```bash
/app/views
```

## Helper

Особенность helper, в том что их можно вызвать во view. Если есть хоть чуточку сложная логика должна перемещаться из view в helper

Примеры:

-   [Создание своего helper](#helper---создание-своего)
-   [Создать Form, через helper](#views---поля-формы)
-   [Обернуть html tag'ом, через helper](#helper---обернуть-html-tagом)
-   [Создать ссылку, через helper](#helper---создать-href)
-   [Создать путь по методологии REST, через helper](#helper---создать-path)

## erb

Разметка для создания шаблонов. Помесь Ruby и HTML

Примеры:

-   [Layout - шаблон для шаблонов](#view---layout)
-   [Синтаксис erb](#view---синтаксис-erb)
-   [Использование переменных из Action во Views](#views---вставка-данных-в-шаблон)
-   [Пример - вставка списка через шаблон](#view---вставка-списка)
-   [Сборка CSS стлией](#view---сборка-css-стилей)
-   [Вставляем erb файл в erb](#view---partial)

## Form

Form - обычная HTML Form. По пользовательскому функционалу тоже. По принципу работы немного отличается

Примеры:

-   [Пример создания Form](#views---поля-формы)
-   [Хранение паролей](#views---пароли)

## webpack

## variables

## layouts

## partials

## assets pipeline

# Всякое

## Generators

В rails многие вещи можно не писать руками, а использовать генераторы:

-   [Генератор Model](#generator---model)
-   [Генератор Controller](#generator---controller)

## envirements

## Rails console

Это repl для ruby, но с подгруженным окружением проекта rails и его моделями:

-   [Запуск rails-консоли](#rails-консоль---запуск)
-   [Перезагрузка rails-консоли](#перезагрузка-rails-консоли)

## logs

## tests

## config

# Примеры

## before_action - перед всеми action

Можно указать какой метод нужно выполнять перед всеми action'ами:

```ruby
class QuestionsController < ApplicationController
    before_action :set_question

    ...

    private

    def set_question
        @question = Question.find(params[:id])
    end
end
```

## before_action - перед конкретными action

Или указать для каких конкретно методов он должен работать:

```ruby
class QuestionsController < ApplicationController
    before_action :set_question, only: [:show, :edit, :update, :destroy]

    ...

    private

    def set_question
        @question = Question.find(params[:id])
    end
end
```

## REST - быстрое создание всех путей

Поскольку соглашение стандартное, то в rails можно создать сразу все REST запрсы:

-   Код для - routes.rb

```ruby
Rails.application.routes.draw do
    resources :questions
end
```

## REST - быстрое создание определенных путей

Можно указать конкретный набор действий, которые нужно создать из списка REST:

-   Код для - routes.rb

```ruby
Rails.application.routes.draw do
    resources :questions, only: [:new, :create]
end
```

## REST - быстрое создание путей безсписочных объектов

Когда у объекта нет смысла отображать список есть специальный синтаксис:

-   по сути работает так же как rosurces, но не создает sessions#index
-   в качестве примера выступают пользовательские сессии

```ruby
Rails.application.routes.draw do
    resource :session
end
```

## rails-консоль - запуск

Запуск rails-консоли:

```bash
rails console

rails c
```

## rails-консоль - перезагрузка

Перезагружаем консоль, чтобы подгрузить сохраненные данные:

```ruby
reload!
```

## (CRUD) Create

Создать строчку в таблице, это просто инициализация объекта с указанием полей:

```ruby
question = Question.create(body: "Текст вопроса", user_id: 1)
```

## (CRUD) Create - через болванку

Создать запись в БД через болванку:

-   болванка - все поля БД становятся параметрами болванки
-   все параметры nill

```ruby
# Создать болванку
question = Question.new
# Задать параметр body
question[:body] = "Text"
# Сделать запись в БД
question.save
```

## (CRUD) Read - одна запись

Находим запись по id:

```ruby
question = Question.find(1)
```

Находим запись по параметру:

```ruby
question = Question.find_by(email: "example@mail.com")
```

## (CRUD) Read - множество записей

Находим множество записей соответствующих параметрам:

```ruby
# name = "Vasia" и body = "Вопрос"
question = Question.where(name: "Vasia", body: "Вопрос")
# Длина title больше 20
books = Book.where("LENGTH(title) > 20")
```

Комбинированный запрос:

-   or/and/not - логические функции

```ruby
books = Book.where(category: "Programming").or(Book.where(category: "Ruby"))
```

Запрос с подстановкой:

```ruby
# первый способ
user = User.where(["name = ? and email = ?", "Joe", "joe@example.com"])
# второй способ
user = User.where(["name = :name and email = :email", { name: "Joe", email: "joe@example.com" }])
```

Примеры:

-   [Примеры с where](https://apidock.com/rails/ActiveRecord/QueryMethods/where)

## (CRUD) Read - последняя запись

Получить последнюю запись:

```ruby
question = Question.last
```

## (CRUD) Read - все записи

Получить всю таблицу, результатом можно пользоваться как массивом:

```ruby
all_questions = Question.all
```

## (CRUD) Update

Меняем содержание строки БД:

```ruby
# READ - получаем запись
question = Question.find(1)
# Меняем значение поля
question.body = "Другой вопрос"
# Записываем
question.save
```

## (CRUD) Delete

Удалить строку:

```ruby
# READ - получаем запись
question = Question.find(1)
# DELETE - удалить запись
question.destroy
```

## Migration - Применить миграции

Созданные миграции нужно применить к базу данных, чтобы в ней произошли изменения:

```bash
rails db:migrate
```

## Migration - Откатить миграцию

Есть смысл использовать если еще не закинул в репозиторий, а поменять структуру таблицы надо

```bash
rails db:rollback
```

## Migration - Добавить поле

Итак:

-   password - поле password которое добавится
-   users - таблица в которую добавляем поле
-   password:string - тип добавляемого поля

```bash
rails g migration add_password_to_users users:string
```

## Migration - Изменить тип поля

Итак:

-   telephone - поле в таблице users у которое меняем тип
-   users - таблица в которой меняем поле
-   string - тип на который меняем поле

```bash
rails g migration change_telephone_to_be_string_in_users
```

## Migration - удалить поле

Итак:

-   field_name - поле в таблице users у которое меняем тип
-   users - таблица в которой меняем поле
-   integer - тип на поля, которое удаляем
-   после удаления поля не забудь изменить Fixtures

```bash
rails g migration remove_field_name_from_users field_name:integer
```

## Generator - Model

Таблицы генерируются с дополнительными полями:

-   id - самоинкрементируемое
-   created_at - дата создания
-   updated_at - дата последнего изменения

Используем generator для создания Model:

-   rails generate model - попросить rails сгенерировать Model
-   question - название модели(Question)/таблицы(Questions)
-   body:text - с полем body типа text
-   user_id:integer - с полем user_id типа integer

```bash
rails generate model question body:text user_id:integer
```

## Generator - Destroy Model

Чтобы удалить таблицу, надо откатить миграцию, и после:

```bash
rails destroy model question
```

## Generator - Controller

Генератор для контроллеров:

-   rails generate controller - попросить rails сгенериравть Controller
-   questions - название controller

```bash
rails generate controller questions
```

## Views - поля формы

В Action, нужно создать инстанс переменную в которой будут указаны поля для формы, тогда можно будет в erb-файле:

-   Создавая форму таким образом, будет добавленно скрытое поле token с сгенерированным ключом. Без ключа запрос не пройдет на сервер не пройдет в Action
-   Значения инстанс переменной попадают - попадают в качестве значений в поля
-   @question - инстанс переменная

```erb
<%= form_with model: @question do |form| %>
    <!-- Label для параметра :body-->
    <%= form.label :body, 'Текст вопроса' %>
    <!-- Текстовое поле для параметра :body-->
    <%= form.text_area :body, rows: 5 %>

    <!-- Текстовое поле для параметра :user_id -->
    <%= form.text_field :user_id %>

    <!-- Поле для ввода пароля, со звездочками вместо текста -->
    <%= form.password_fied :password %>
    <!-- Поле для подтверждения пароля -->
    <%= form.password_fied :password_confirmation %>

    <!-- Кнопка отправки формы -->
    <%= form.submit 'Create' %>
<% end %>
```

## Views - Пароли

Есть специальный gem для хранения паролей. Его надо расскоментить в Gemfile:

```ruby
gem "bcrypt"
```

Не забудь установить Gem:

```bash
bundle
```

Создаем пароль:

```ruby
p = BCrypt::Password.create("password")
# => '$2aAISDJja81uw82ijeladO*@UROQJs...'
```

Проверка пароля:

```ruby
p = BCrypt::Password.create("password")
p == "password"
# => true
p == "password2"
# => false
```

Используем внутренний механизм Rails для хранения паролей, для этого в модели прописываем:

-   В БД для пользователей нужно для пароля использовать столбец password_digits

```ruby
class User < AllpicationRecord
    has_secure_password
end
```

## Console - Создание проекта

Создание проекта:

```bush
rails new project_name
```

Необходимо удалить файл:

-   с ним в будующем могут возникнуть проблемы

```bash
/.ruby-version
```

Если работешь на Ubunt'а, то файле gemfile закоментируй строку:

-   чтобы Ruby не спамил warning

```ruby
gem "tzinfo-data",  platforms: %i ...
```

## Console - Запуск локально

Запуск проекта на локальном сервере:

```bash
rails s
```

## Console - Список route

Посмотреть список route в проекте:

```bash
rails routes
```

Вернет только routes с именем users:

```bash
rails routes -g users
```

## Debugger

Переход в режим отладки:

-   Пишется в ruby коде. Например в Action

```ruby
debugger
```

Запускает в консоли repl режим. Чтобы продолжить работу приложения нужно прописать:

```bash
c

continue
```

## Helper - создание своего

Можно создавать свои helper, точнее даже нужно, ибо в шаблонох логика должна быть минимальна

Чотбы создать helper идем в:

```bash
/app/helpers
```

Общая для всех шаблонов логика, прописывается в application_helper. Логико для конкретного view в отдельном файле в той же папке:

-   helper_name - имя по которому helper можно вызвать

```ruby
module ApplicationHelper
    ...

    def helper_name(variable1, variable2, ...)
        ...
    end

    ...
end
```

## Route - прописать root

Прописываем куда ведет root запрос:

-   root запрос - это URL типа: http://www.google.com
-   изменеия в файле config/routes.rb
-   в примере вызовется Action: index, у Controller: QuestionsController

```ruby
Rails.application.routes.draw do
    root to: 'questions#index'
end
```

## Route - создаем route

В файле config/routes.rb добавляем:

-   post - тип HTTP запроса. Может быть, например: post/get/put/delete
-   /questions - URL путь
-   /questions/:id - путь типа /questions/1, где 1 передастся в params[:id]
-   questions#create - дергаем модель Action: create, у Controller: QuestionsController

```ruby
Rails.application.routes.draw do
    post '/questions', to: 'questions#create'
    patch '/questions/:id', to 'questions#update'
end
```

## Controller - создать Action для всех Controller

Чтобы создать Action сразу для всех Controller, заходим в:

```bash
/app/controllers/application_controller.rb
```

Добавляем код:

-   в примере мы получаем запись полльзователя из БД, если сессия с пользователем открыта(в session есть user_id)

```ruby
class ApplicationController < ActionController::Base
    private

    def current_user
        @current_user ||= User.find_by(id: session[:user_id]) if session[:user_id]
    end
end
```

## Controller - создать helper

Чтобы создать helper внутри Controller, заходим в:

```bash
/app/controllers/application_controller.rb
```

Добавляем код:

-   в примере мы получаем запись полльзователя из БД, если сессия с пользователем открыта(в session есть user_id)
-   функцию current_user можно использовать внутри view

```ruby
class ApplicationController < ActionController::Base
    helper_method :current_user

    private

    def current_user
        @current_user ||= User.find_by(id: session[:user_id]) if session[:user_id]
    end
end
```

## Action - permitted_params

Злоумышленник может передать лишние параметры, которые мы можем случайно записать в БД. Чтобы такого не произошло подключается "Защита от дурака". Суть в том, что мы выбираем какие параметры из пришедших мы дальше будем использовать:

-   .require(:qeustion) - требуем наличия параметра qeustion
-   .permit(:body, :user_id) - разрешаем только params.qeustion.body и params.qeustion.user_id

```ruby
class QuestionsController < ApplicationController
    def create
        question_params = params.require(:qeustion).permit(:body, :user_id)
        question = Question.create(question_params)

        ...
    end
end
```

Совсем правильно создавать отдельный метод внутри Controller, в котором мы будем получать нужные параметры:

```ruby
class QuestionsController < ApplicationController
    def create
        question = Question.create(question_params)

        ...
    end

    private

    def question_params
        params.require(:qeustion).permit(:body, :user_id)
    end
end
```

## Action - отключить Token

Поведение Action по умолчанию таково, что он ждет правильный Token. Если пришедшй Token не совпадает с нужным, то Action не произойдет. [Token генерирутеся во View -> Form](#views---поля-формы), но бывают ситуации, когда такое поведение нежелательно. Мы можем отключить Token

-   skip_before_action :verify_authentificity_token - отключаем требование Token

```ruby
class QuestionController < ApplicationController
    skip_before_action :verify_authentificity_token

    ...
end
```

## Action - ответ пользователю default

По умолчанию(default) Rails вернут в качестве ответа файл:

-   ControllerName - имя контроллера
-   ActionName - имя метода

```bash
app/views/ControllerName/ActionName.html.rb
```

## Action - ответ пользователю redirect_to

Перенаправить пользоваетеля на другую страницу(redirect):

-   QuestionController - текущий Controller
-   create - текущий Action
-   redirect_to - функция перенаправления
-   '/' - адресс перенаправления. В примере, направляем в root

```ruby
class QuestionController < ApplicationController
    ...

    def create
        ...

        redirect_to '/'
    end

    ..
end
```

## Action - ответ пользователю render

Отрендерить views:

-   QuestionController - текущий Controller
-   create - текущий Action
-   render - функция ответа
-   любой закоментированный вариант отрендерит шаблон находящйися в: app/views/questions/new.html.rb
-   render :new - рекомендован к использованию

```ruby
class QuestionController < ApplicationController
    ...

    def create
        ...

        render :new
        # render :new
        # render action: :new
        # render "new"
        # render action: "new"
        # render "questions/new"
        # render template: "questions/new"
    end

    ...
end
```

Другие варианты использования render:

| Что делает?                     | Ruby код                                                                   |
| :------------------------------ | :------------------------------------------------------------------------- |
| Вернет статичный файл           | render file: "#{Rails.root}/public/404.html", layout: false                |
| Вернет HTML                     | render html: helpers.tag.strong('Not Found')                               |
| Вернет отрендеренный erb        | render inline: "<% products.each do \|p\| %><p><%= p.name %></p><% end %>" |
|                                 |                                                                            |
| Вернет необработанные данные    | render body: "raw"                                                         |
| Вернет текст без разметки       | render plain: "OK"                                                         |
| Вернет XML(product - это hash)  | render xml: @product                                                       |
| Вернет JSON(product - это hash) | render json: @product                                                      |
| Вернет JS                       | render js: "alert('Hello Rails');"                                         |

## Flash - заполнение

Заполнение Flash, происходит в Action:

-   flash[:notice] - заполняем flash.notice
-   redirect_to - происходит redirect

```ruby
flash[:notice] = 'It is message'
redirect_to question_path(question)
```

Заполнение Flash, происходит в Action:

-   redirect_to - происходит redirect
-   notice: 'It is message' - заполняем flash.notice

```ruby
redirect_to question_path(question), notice: 'It is message'
```

## Flash - получение

Flash можно получить в action:

```ruby
flash_notice = flash[:notice]
```

Flash можно получить в erb:

-   в примере: если flash[:notice] существует, то создает параграф

```erb
<% if flash[:notice].present? %>
    <p><%= flash[:notice] %></p>
<% end %>
```

## Flash - решение проблемы при использовании с render

Когда мы используем render, то на страницу приходит новый flash. Но при любом переходе пользователя на другую страницу, этот flash тоже отправиться. Это не желательное поведение можно избежать:

-   QuestionController - текущий Controller
-   create - текущий Action
-   flash.now - заполнит flash, который будет использован только для redner

```ruby
class QuestionController < ApplicationController

    def create
        flash.now[:alert] = "It is alert"
        # отрендерит new.html.erb
        render :new
    end
end
```

## Cookie - информация

Rails по умолчанию использует Cookie и начего настраивать для этого не надо. Cookie отправляются браузеру в зашифрованном виде, в таком виде он из и хранит. Ключ шифрования храниться в файле:

```bash
/tmp/development_secret.txt
```

Самое стандартное поведение, которое добавляют в cookie - использование user_id, для сохранения сессии

## Cookie - добавление данных

Добавить новый параметр в Coockie:

```ruby
session[:current_time] = Time.now
```

## Cookie - чтение данных

Чтобы из Cookie получить hash с которым можно будет работать, нужно:

```ruby
session.to_h
# => {"session_id"=>"dafs21423qrar1...", "_csrf_token"=>"567hdajuIAUH7167hkhUHA..."}
```

## View - вставка списка

Пример со вставкой списка:

```erb
<h2>Вопросы:</h2>

<% @questions.each do |question| %>
    <p>Вопрос: <%= question.id %></p>
<% end %>
```

## View - синтаксис erb

Синтаксис erb состоит из html и вставок на Ruby. Они бывают двух типов:

-   <% ... %> - просто исполняется код
-   <%= ... %> - вставляет результат выполнения кода
-   ... - вместо троеточия код на Ruby

```html
<html>
	<body>
		<% ... %> <%= ... %>
	</body>
</html>
```

## View - Layout

Существует erb шаблон используемый в качестве основы для остальных шаблонов. Он находится в:

```bash
/views/layouts/application.html.erb
```

Место куда вставляются другие шаблоны:

```erb
...
<%= yield %>
...
```

## Views - вставка данных в шаблон

Создаем в Action инстанс переменную:

```ruby
@question = Question.find(params[:id])
```

Используем переменную во view:

```erb
<html>
	<body>
		<%= @question.id %>
	</body>
</html>
```

## View - Сборка CSS стилей

Когда Rails видит в erb:

-   stylesheet_link_tag - это helper, котрый отправляется собирать стили
-   'application' - это файл с путем app/assets/stylesheets/application.css
    Сборка стилей работает следующим образом. Rails когда видит в erb:

```erb
<%= stylesheet_link_tag 'application' %>
```

Файл: app/assets/stylesheets/application.css, не просто CSS, а имеет несколько дополнительный синтаксис:

-   \*= require_tree ./modules - подключить все CSS из папки ./modules
-   \*= reuqre_self - подключить инструкции из текущего файла
-   \*= reuqre text - подключить ./text.css
-   \*= reuqre gem_style - если такого css файла нет в папке, начнет искать в установленных gem

```erb'
/*
 *= require_tree .
 *= reuqre_self
*/
```

## View - partial

Partial - механизм, благодаря которому можно вынести часть erb кода, и вставить его в текущий erb файл. Для этого:

-   В папке /app/views/route_folder
-   Создаем файл начинающийся с \_, например: \_form.html.erb
-   Теперь в месте где хотим рендерить Partial прописываем имя файла без \_

```erb
<%= render 'form' %>
```

Так же мы можем передать переменные внутрь Partial и использовать их:

-   @question - переменная которую передаем из основного erb
-   question - имя локальной переменной внутри partial

```erb
<%= render 'form', question: @question %>
```

## Helper - обернуть html tag'ом

Обернуть в tag какой-то content:

-   'Test in span' - это content, который оборачиваем в тег
-   'span' - тег в который оборачиваем
-   content_tag - это helper, который оборачивает контент в тег
-   class: 'mr-sm' - добавляем аттрибут class со значение mr-sm

```erb
<%= content_tag 'span', 'Test in span', class: 'mr-sm' %>
```

## Helper - создать href

Создаем href через helper:

-   link_to - это helper
-   'Вопрос #{question.id}' - это текст ссылки
-   '/questions/#{question.id}' - ссылка

```html
<%= link_to 'Вопрос #{question.id}', '/questions/#{question.id}' %>
```

Добавляем то, каклой метод использовать. На самом деле всегда использует POST запрос, но в заголовке зпроса прописывает специальные параметры, благодаря которым Rails понимает как трактовать этот запрос, и делает небольшую подтасовку на стороне сервера:

-   method: :delete - указываем тип http запроса, в данном случае delete

```html
<%= link_to 'Вопрос #{question.id}', '/questions/#{question.id}', method:
:delete %>
```

Иногда нужна не подтасовка, а настоящий GET запрос. Тогда есть хитрость, нужно использовать кнопку:

-   button_to - создает кнопку
-   'Выйти' - текст кнопки
-   '/questions/#{question.id}' - ссылка
    link_to отправляет Post запрос а это проблема с точки зрения REST, поэтому если нужен GET запрос, создаем кнопку:

```erb
<%= button_to 'Выйти', '/questions/#{question.id}' %>
```

## Helper - создать path

При запуске сервера, исзодя из route автоматически создет helper'ы типа path. Это сделано чтобы вы не hardcode'ить ссылки внутри шаблонов:

-   questions_path - сгенерированное название helper'а. Часть questions это имя route, а \_path указывает что это helper
-   (id: question.id) - передаваемый параметр

```html
<%= link_to 'Вопрос #{question.id}', questions_path(id: question.id) %>
```

Вместе со [списком route](#console---список-route) так же выводятся имена сгенерированных helper

## Rails - Структура проекта

Базовые git файлы:

-   .git - это папка созданная git'ом
-   .gitattributes - файл настройки git'a
-   .gitignore - файл игнорирования для git'а
-   README - ну это база

БД:

-   db - базы данных и миграции
    -   migrate - папка с миграциями базы данных
    -   schema.rb - хранит структуру бд в ruby коде
-   config - конфиги
    -   routes.rb - руты и endpoints. То бишь какой запрос ка обрабатывать
    -   database.yaml - конфиг базы данных. Этот файл не должен попасть в репозиторий, там для БД храняться ключи шифровки

Приложение:

-   app - папка нашего приложения. Тут будет лежать код контроллеров, моделей, шаблонов, view и т.д.
-   public - публичные файлы, которые можно запросить(localhost:3000/index.html) у сервера и он их сразу отдаст. Типа всякие каритинки, html статика, css и т.д.

Всякие вспомогательные штучки-дрючки:

-   tmp - папка для всяких временных файлов
-   test - папка для тестов, но только для фреймворка mini-test
-   log - логи сервера
-   config.ru - файл описывающий как запустить наше приложение, но бычно им не пользуются

Обычно не используется:

-   storage - специальная папка, туда rails подгружает файлы пользователей с помощью специального функционала
-   bin - папка для бинарников. Обычно не нужна
-   lib - дополнительные ruby файлы, обычно там пусто
-   vendor - папка для пакетов, которые почему-то установили не через gem. Обычно не используется
