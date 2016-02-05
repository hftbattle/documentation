## Компиляция и запуск стратегии

* [Запуск из командной строки](#command_line)
* [Запуск из CLion](#clion)


<a name="command_line"></a>
###Запуск из командной строки

Ваша стратегия должна быть реализована в одном файле *user_strategy.cpp*.

Для компиляции стратегии надо выполнить команду 
```
./build.py
```

Она соберет все стратегии (файлы вида *strategies/[strategy_dir]/[strategy_name].cpp*) в библиотеки в папку *build*.

Для запуска стратегии нужно выполнить команду
```
./run.py strategies/user_strategy/user_strategy.json
```

<a name="command_line"></a>
###Запуск из CLion

