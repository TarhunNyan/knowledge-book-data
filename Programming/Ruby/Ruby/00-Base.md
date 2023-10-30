# Ruby

Японский язык программирования. На Ruby(Rails) написан githab

Примеры:

-   [Запуск repl](#ruby---запуск-repl)
-   [Запуск скрипта](#ruby---запуск-скрипта)

# Вывод в консоль

## puts

Вставляет в конец строки переход на следующую(\n):

```ruby
puts 30 / 4
puts 30 / 4
# => 7
# => 7
```

## print

Не вставляет в конец строки переход на следующую(\n):

```ruby
print 30 / 4
print 30 / 4
# => 77
```

# Переменные

Язык с динамической типизацией. Переменная:

```ruby
variable = "text"
```

Константа:

```ruby
VARIABLE = "const"
```

Глобальная переменная:

```ruby
$global_variable = "global variable"
```

Конвертация типов происходит следующими методами:

| Имя метода | Во что конвертирует | Пример                                          |
| :--------- | :------------------ | :---------------------------------------------- |
| to_i       | Integer             | "aaaaaa".to_i => 0                              |
| to_f       | Float               | 3.to_f => 3.0                                   |
| to_s       | String              | 40.to_s => "40"                                 |
| to_a       | Array               | (1..10).to_a => [1, 2, 3, 4, 5, 6, 7, 8, 9, 10] |

## Numbers

Есть int и float:

```ruby
# int
a = 4
# float
b = 4.0
```

Пример:

-   [Математические операции](#numbers---математические-операции)
-   [Абсолютные значения](#numbers---абсолютное-значение)

Примеры с float:

-   [Окргуление чисел](#float---округление)
-   [Число больше/меньше/равно знаения](#float---большеменьшеравно)

## String

```ruby
str = "Это строка"
```

Примеры:

-   [Конкатинация строк](#string---конкатинация)
-   [Длина строки](#string---длина)
-   [f-string](#string---f-string)
-   [Реверс](#string---реверс)
-   [Разбить строку на подстроки](#string---разбить-строку)
-   [Склеить подстроки в строку](#string---склеить-строки)
-   [Замена подстроки](#string---замена-подстроки)

## Array

Массив - набор элементов

```ruby
[32, 33, 40]
# => [32, 33, 40]
```

Пример:

-   [Получить элемент по индексу](#array---получить-элемент-по-индексу)
-   [Максимальное значение в массиве](#array---максимум)
-   [Сортировка массива](#array---сортировка)
-   [Проверить пустой ли массив](#array---пустой-ли-массив)
-   [Склеить массив строк](#string---склеить-строки)

## Range

Пример range:

-   1..10 - и происходит создание range

```ruby
for item in 1..10 do
  puts item
end
```

## Symbols

Для ключевых слов есть специальный синтаксис - Symbols. Это не строка, а именно что символ. Используется так:

```ruby
:symbolword
# => symbolword
test_hash = { :symbolword: "Значение"}
# => { :symbolword => "Значение"}
```

## Hash

То же самое, что объекты из JS, но в Ruby они называются Hash

Примеры:

-   [Создать Hash](#hash---создать-hash)
-   [Получить элемент](#hash---получить-элемент)
-   [Добавить/Поменять элемент](#hash---добавитьпоменять-элемент)
-   [Получить ключи](#hash---получить-ключи)
-   [Получить значения](#hash---получить-значения)
-   [Пробежаться по Hash](#hash---пробежаться-по-hash)
-   [Отфильтровать Hash](#hash---отфильтровать-hash)

# Структуры

## Комментарии

Однострочный

```ruby
# Однострочный комент
```

Многострочный

```ruby
=begin

Многострочный комментарий

Обычно им не пользуются

=end
```

## Условия

Однострочные условия:

```ruby
a=0
puts("okey") if a==0
```

Обычные условия:

```ruby
a = 0

if a == 100
    puts "Expression is true, but a is now: #{a}"
elsif a == 20
    puts "Okey"
else
    puts "#{a} is not equal to 100"
end
```

-   [Примеры](#условия---логика)

## Циклы

Примеры:

-   [ForEach для элементов массива](#циклы---foreach)
-   [Повторить N-раз](#циклы---times)
-   [Цикл while](#циклы---while)
-   [Цикл for-in](#циклы---for--in--do)
-   [Break/Next - прерывание и пропуск цикла](#циклы---breaknext)

## Циклы - break/next

Break - прерывание цикла:

```ruby
i = 1

while true
    if i >= 21
        break
    end
    i += 1
end
```

Break по условию:

```ruby
i = 1

while true
    break if i >= 21
    i += 1
end
```

Next - пропуск иттерации, как continue в других языках:

```ruby
for x  in  0..6
    if  x < 4 then
      next
    end

    puts "Value of x is : #{x}"
end
```

## Методы и функции

Примеры:

-   [Метод](#методы-и-функции---методы)
-   [Функция с явным возвратом](#методы-и-функции---функции)
-   [Функция с НЕ явным возвратом](#методы-и-функции---функциинеявные)
-   [Пометка мутирующих и булевых функций](#методы-и-функции---пометка)

## Классы

Вообще в Ruby как в Python, все есть классы

Настройка доступа к методам/аттрибутам:

-   [Инстанс переменные](#классы---объявление-класса)
-   [Доступ к пременной объекта](#классы---доступ-к-пременной-объекта)
-   [Static](#классы---static)
-   [Private](#классы---private)
-   [Protected](#классы---protected)

Примеры:

-   [Создание экземпляра класса](#классы---создание-экземпляра-класса)
-   [Объявление класса](#классы---объявление-класса)
-   [Проверка наличия метода в классе](#классы---проверка-наличия-метода)
-   [Получить список методов класса](#классы---получить-список-методов-класса)

## Синтаксический сахар

Всякие удобные штуки:

-[Получить аттрибут если не nill](#sugar---получить-аттрибут-если-не-nill) -[Записать если пусто](#sugar---записать-если-пусто)

# Примеры

## Ruby - Запуск repl

Запуск repl:

```bash
irb
```

## Ruby - Запуск скрипта

Запуск скрипта:

```bash
ruby script_name.rb
```

## Условия - Логика

Сравнения:

```ruby
5 <= 10
# => true
```

Равенство

```ruby
'abc' == 'def'
# => false
```

Преобразования:

```ruby
true
# => true

123456
# => true
0
# => true

nil
# => false

'xyz'.empty?
# => false
```

Ошибки:

```ruby
'a' > 5
# => error:
```

## Numbers - Математические операции

Математические операции:

```ruby
2 + 3
# => 5

12 - 5
# => 7

2 * 3
# => 6

10 / 7
# => 1

10 / 7
# => 1

10.0 / 7
# => 1.4285714285714286

20 % 3
# => 2

2 ** 3
# => 8

Math.sqrt(9)
# => 3
```

## Numbers - Абсолютное значение

Абсолютное значение:

```ruby
(-54.2).abs
# => 54.2
```

## Float - Округление

Округление:

```ruby
(-8.9).round
# => -9
(6.7).round
# =>  7

(7.65).round(1)
# =>  7.7
```

## Float - больше/меньше/равно

Прикольная функция языка:

```ruby
puts 2.1 <=> 4
# => -1
puts 4.0 <=> 4
# =>  0
puts 4.6 <=> 4
# =>  1
```

## String - Конкатинация

```ruby
"Jimmy" + " Olia"
# => Jimmy Olia

"Jimmy" * 2
# => JimmyJimmy
```

## String - Длина

```ruby
"Jimmy".length
# => 5
```

## String - f-string

Форматированная строка:

```ruby
a = 4
b = "a = #{a}"
```

## String - Реверс

```ruby
"Jimmy".reverse
# => ymmiJ
```

## String - Разбить строку

Разбить строку на строчки

```ruby
a = "Hello world!\nI'm grisha"
a.lines
# => ["Hello world!\n", "I'm grisha"]
```

## String - Склеить строки

Склеить строки(и числа) в массиве:

```ruby
["papa", " ", "mama"].join
# => papa mama
```

## String - Замена подстроки

Замена подстроки

```ruby
str = "This is toast for me"
strVar.gsub("toast", "honeydew")
# => "This is honeydew for me"
```

## Array - максимум

Максимум:

```ruby
[32, 33, 40].max
# => 40
```

## Array - сортировка

Сортировка, мутирует массив:

```ruby
arr = [32, 100, 33, 40]
arr.sort!
# => [32, 33, 40, 100]
```

## Array - получить элемент по индексу

Получить массив по индексу:

```ruby
arr = [32, 100, 33, 40]
arr[0]
# => 32
arr[-2]
# => 33
```

## Array - пустой ли массив?

Есть ли в массиве что-нибудь:

```ruby
[1,2,3].any?
# => true
```

## Hash - Создать Hash

```ruby
books = { "First" => "Book name", :John => "Sina" }
# => { "First" => "Book name", :John => "Sina" }
```

## Hash - Добавить/Поменять элемент

```ruby
books = { "First" => "Book name" }
books["Second"] = "Not book name"
# => "Not book name"
```

## Hash - Получить элемент

```ruby
books = { "First" => "Book name" }
books["First"]
# => "Book name"
```

## Hash - Получить ключи

```ruby
books = { "First" => "Book name" }
books.keys
# => ["First"]
```

## Hash - Получить значения

```ruby
books = { "First" => "Book name" }
books.values
# => ["Book name"]
```

## Hash - Пробежаться по Hash

```ruby
s["William Shakespeare"].each { |key, val|
  puts val["title"]
}
```

## Hash - Отфильтровать Hash

Вернет только то, что совпало:

```ruby
hashData.select { |k, v| v["finished"] == year }
```

## Циклы - ForEach

```ruby
books.values.each { |rate|
  ratings[rate] += 1
}
```

## Циклы - times

Повторить блок кода пять раз

```ruby
5.times { print "Odelay! " }
```

Использовать номер шага как переменную

```ruby
5.times { |time| puts time }
```

## Циклы - while

Цикл while:

```ruby
count = 1

while count <= 5 do
  puts count
  count += 1
end

puts "я иду искать!"
```

## Циклы - for ... in ... do

Цикл for ... in ...:

```ruby
array = [1,2,3,4,5]

for item in array do
  puts item
end
```

## Методы и функции - Методы

Без return:

```ruby
def func( variable1, variable2 )
  puts variable1
end

func(1, 2)
```

## Методы и функции - Функции

C return:

```ruby
def func( variable1, variable2 )
  return variable1 + variable2
end

puts func(1, 2)
# => 3
```

## Методы и функции - Функции(неявные)

Неявное поведение(возвращает результат последней отработавшей строки кода):

```ruby
def hungry?(time_of_day_in_hours)
  if time_of_day_in_hours > 0
    true
  else
    false
  end
end

puts hungry(10)
# => true
puts hungry(-10)
# => false
```

## Методы и функции - Пометка

Мутирующие методы в Ruby помечаются восклицательным знаком:

```ruby
arr = [32, 100, 33, 40]
arr.sort!

puts arr
# => [32, 33, 40, 100]
```

В программировании приянто, что функция возвращающая true/false начинается с is. В ruby вместо этого используется знак вопроса в конце названия функции:

```ruby
'xyz'.empty?
# => false
```

## Классы - Создание экземпляра класса

Создание экземпляра класса

```ruby
blurb = Blurb.new(:sick).time
```

## Классы - Объявление класса

Создаем класс:

-   attr_accessor - [смотри доступ к перемнной объекта](#классы---доступ-к-пременной-объекта)
-   initialize - метод срабатывающий при создании класса
-   @ - тоже что и this в других языках

```ruby
class Blurb
  attr_accessor :content, :time, :mood

  def initialize(mood, content="")
    @time    = Time.now
    @content = content[0..39]
    @mood    = mood
  end
end
```

## Классы - Проверка наличия метода

Вернет true, если такой метод есть, false если метода нет:

```ruby
ObjectInstance.respond_to?("join")
# => true
```

## Классы - Получить список методов класса

Получает все методы включая скрытые:

```ruby
CstmObject.instance_methods
# => ["method", "send", "object_id", "singleton_methods", ... ]
```

Получает все кастомные методы:

```ruby
CstmObject.instance_methods(false)
# => ["cstmMethod1", "cstmMethod2"]
```

## Классы - Доступ к пременной объекта

Создает метод для чтения, то бишь добавляет метод Person.name:

```ruby
class Person
  attr_reader :name
end
```

Создает метод для записи, то бишь добавляет метод Person.name=:

```ruby
class Person
  attr_writer :name
end
```

Создает метод для записи и чтения:

```ruby
class Person
  attr_accessor :name
end
```

## Классы - инстанс переменные

Переменные принадлежащие объекту. То что в других языках реализованно через this:

```ruby
class Bubble
  def set_name( name )
    @name = name
  end
end

bubble = Bubble.new
bubble.set_name "valera"
puts bubble.name
# => "valera"
```

## Классы - Static

Статические переменные/функции доступны всем экземплярам класса:

-   @@ - показываем что переменная статическая
-   geek_count - статическая переменная
-   self. - показываем что метод статический
-   getCount - статическая метод

```ruby
class Geeks
    @@geek_count = 0

    def initialize
        @@geek_count += 1
    end

    def self.getCount
      return @@geek_count
    end
end
```

Итого:

```ruby
test1 = Geeks.new
test2 = Geeks.new

puts test1.geek_count
# => error
puts test2.getCount
# => error

puts Geeks.geek_count
# => error
puts Geeks.getCount
# => 2
```

## Классы - Protected

Protected методы можно использовать внутри блока класса или у полученных из вне объектов :

-   protected - все методы и аттрибуты объявленныепосле protected, будут protected
-   name - аттрибут являющийся protected
-   other.name - получаем аттрибут из внешнего объекта

```ruby
class Food
  def initialize(name)
    @name = name
  end

  def ==(other)
    name == other.name
  end

  protected
  attr_reader :name
end
```

Итого:

```ruby
food = Food.new("chocolate")
puts food == food
# => true
```

## Классы - Private

Private методы можно использовать только внутри блока класса:

-   private - все методы объявленныепосле private, будут private
-   private_class_method - список методов после, будет private
-   eat - public метод
-   get, cook - private метод

```ruby
class Food
  def eat( food )
    get food
    cook food
  end

  private
  def get( food )
    ...
  end

  def cook( food )
    ...
  end
end

# Второй способ объявить private методы
class Food
  private_class_method :get :cook

  def eat( food )
    ...
  end

  def get( food )
    ...
  end

  def cook( food )
    ...
  end
end
```

Итого:

```ruby
food_orange = Food.new

food_orange.eat "orange"
# Все сработает

food_orange.get "orange"
# => error
food_orange.cook "orange"
# => error
```

## Sugar - Получить аттрибут если не nill

Вызвать метод если он не nill, тоже что ?. в js:

```ruby
variable&.method()
```

## Sugar - Записать если пусто

Если переменная nill, то запишет в нее значение, иначе не тронет:

```ruby
variable ||= 10
```
