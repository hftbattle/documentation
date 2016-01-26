#Local pack

Мы подготовили для вас локальный пакет – набор библиотек и файлов, достаточный для разработки стратегий на вашем компьютере.

 на [GitHub]({{ book["contest.local-pack.url"]}}) для разработки у себя на компьютере. Для тех, у кого установлен [Git]({{ book["git-download.url"] }}), можно выполнить следующую команду в консоли:
```
git clone https://github.com/hftbattle/local_pack.git
```

##Запуск для разных операционных систем
### [Linux](#linux)

### [Mac OS X](#macosx)

### [Windows](#windows)



Для участия в контесте, убедитесь, что у вас на машине установлены
а) если у вас Ubuntu:
- cmake
- g++ 4.9
- python
б) если у вас MacOS:
- python
- cmake (brew install cmake)
- apple clang (идет с Command Line Tools)
в) если у вас Windows:
- mingw64 версии не ниже 4.9
(Установить MINGW-w64 http://sourceforge.net/projects/mingw-w64/
с настройками --rev=0 --bootstrap --jobs=4 --threads=posix --exceptions=seh --arch=x86_64
Добавить файл с бинарниками в path:
setx path "%path%;YOUR_PATH_TO_MINGW\x86_64-4.9.2-posix-seh-rt_v3-rev0\mingw64\bin")
- cmake (например, можно поставить CLion, cmake будет в комплекте
https://confluence.jetbrains.com/display/CLION/Early+Access+Program
добавить путь до cmake.exe в PATH)
- python (опять же не забыть добавить путь до python.exe в PATH)

Разархивирвать архив data.zip (unzip data.zip), убедиться что папка data лежит в local_pack

Для компиляции стратегий надо исполнить команду ./build.py
Она соберет все стратегии (файлы, имеющие название *_strategy.cpp) в библиотеки в папку build

Для запуска стратегии вызвать ./run.py config_name.json,
например ./run.py examples/example1_strategy.json

Файл со своей стратегий надо назвать user_strategy.cpp, убедиться что в самом низу написано REGISTER_CONTEST_STRATEGY(YourClassName, user_strategy)
В конфиге также стоит указать в поле "common"/"lib_filename" значение "./libuser_strategy.so"
Пример того как это сделано можно посмотреть в папке examples
