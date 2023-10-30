# RVM

Система контроля версий для Ruby. Дело в том что установив Ruby в систему и работая из него, будет куча проблем. Так что контроль версий наше все

## Установка

На сайте RVM.io, получаем ключ шифрования(выполняем команду в консоле)

-   install GPG keys

На сайте RVM.io, устанавливаем RVM(выполняем команду в консоле)

-   install RVM

Ставим галку на пункте:

```bash
terminal -> preference -> /* select profile */ -> command -> Run command as a login shell
```

Перезапуск консоли. Если все хорошо, то команда вернет путь:

```bash
which rvm
# => /home/userName/.rvm/bin/rvm
```

## Список Ruby

Список установленных версий Ruby:

```bash
rvm list
```

Список всех версий Ruby:

```bash
rvm list known
```

## Установить Ruby

Уствноаить Ruby. В примере версия 3.0.2:

```bash
rvm install 3.0.2
```

# Ruby - cli

Примеры:

-   [Запуск repl](#ruby---запуск-repl)
-   [Запуск скрипта](#ruby---запуск-скрипта)

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
