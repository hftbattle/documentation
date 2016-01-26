#Local pack

Мы подготовили для вас локальный пакет – набор библиотек и файлов, достаточный для разработки стратегий на вашем компьютере. Его можно скачать на [GitHub]({{ book["contest.local-pack.url"]}}). Для тех, у кого установлен [Git]({{ book["git-download.url"] }}), можно выполнить следующую команду в консоли:
```
git clone https://github.com/hftbattle/local_pack.git
```



### Данные
По умолчанию в репозитории Local pack-а находится небольшой 
Разархивирвать архив data.zip (unzip data.zip), убедиться что папка data лежит в local_pack

Для компиляции стратегий надо исполнить команду ./build.py
Она соберет все стратегии (файлы, имеющие название *_strategy.cpp) в библиотеки в папку build

Для запуска стратегии вызвать ./run.py config_name.json,
например ./run.py examples/example1_strategy.json

Файл со своей стратегий надо назвать user_strategy.cpp, убедиться что в самом низу написано REGISTER_CONTEST_STRATEGY(YourClassName, user_strategy)
В конфиге также стоит указать в поле "common"/"lib_filename" значение "./libuser_strategy.so"
Пример того как это сделано можно посмотреть в папке examples
