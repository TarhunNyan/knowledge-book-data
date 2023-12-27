# Windows - Администрирование

## Environment variable

Environment variable - переменные окружения. Это переменные доступные из любого места в windows. Чтобы посмотреть:

```bash
# Открываем панель System
Control Panel | >  System and Security  |    Security and Maintenance  |
              |    Network and Internet |    Windows Defender Firewall |
              |    Hardware and sound   | >  System                    |
              |    ...                  |    ...                       |

# Открываем SystemProperties -> Advanced
/*Left Panel*/ |   ...            | /*Right Panel*/ |   ...                      | > Environmet Variables...
               |   Clipboard      |                 |   System Protection        |
               |   Remote Desktop |                 | > Advenced System settings |
               | > About          |                 |   Rename this PC           |
```

Так же можно открыть это окно через команду в консоли:

```cmd
SystemPropertiesAdvanced
```

## Выходим в BIOS

При перезагрузке зажимаем:

-   acer
    -   <F2> или <Fn+F2>
-   ASRock и ПК
    -   <F2> или <Fn+F2>
    -   <DEL>
-   lenovo
    -   <F1> или <Fn+F1>

## Отключаем f1-f12

Заходим в BIOS и в нем:

```bash
/*Top Panel*/ |   Information |   ...                   |   Media Key      |
              | > Main        |   Touchpad              | > Functional Key |
              |   Advanced    | > Function Key Behavior |   -------------- |
              |   ...         |   ...                   |                  |
```
