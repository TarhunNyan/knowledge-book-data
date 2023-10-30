# Ruby - Классы

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

# Примеры

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
