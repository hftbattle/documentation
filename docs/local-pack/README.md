#Local pack

Мы подготовили для вас локальный пакет – набор библиотек и файлов, достаточный для разработки стратегий на вашем компьютере. Его можно скачать на [GitHub]({{ book["contest.local-pack.url"]}}). Для тех, у кого установлен [Git]({{ book["git-download.url"] }}), можно выполнить следующую команду в консоли:
```
git clone https://github.com/hftbattle/local_pack.git
```

##Запуск на разных операционных системах
### [Linux](#linux)
- gcc 4.9 и выше
```
sudo apt-get install g++
```
- cmake 2.8.4 и выше
```
sudo apt-get install cmake
```
- python 2.7 и выше
```
sudo apt-get install python
```

### [Mac OS X](#macosx)
- Apple clang 6.0 и выше
```
xcode-select --install
```
- brew
```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
- cmake 2.8.4 и выше
```
brew install cmake
```
- python 2.7 и выше
```
brew install python
```

### [Windows](#windows)
- MinGW-w64 версии не ниже 4.9

Установить [MinGW-w64](http://sourceforge.net/projects/mingw-w64/)
с настройками 
```
--rev=0 --bootstrap --jobs=4 --threads=posix --exceptions=seh --arch=x86_64
```
Добавить файл с бинарниками в path:
```
setx path "%path%;YOUR_PATH_TO_MINGW\x86_64-4.9.2-posix-seh-rt_v3-rev0\mingw64\bin"
```

- cmake 2.8.4 и выше

Например, можно поставить [CLion](https://www.jetbrains.com/clion/download/#section=windows-version), cmake будет в комплекте. Далее необходимо добавить путь до cmake.exe в PATH.
```
- python

опять же не забыть добавить путь до python.exe в PATH.

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
