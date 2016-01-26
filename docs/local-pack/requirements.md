##Установка зависимостей
### [Linux](#linux)
Для установки зависимостей под Linux-ом можно запустить скрипт *packages_linux.sh*:
```
./packages_linux.sh
```

Скрипт устанавливает следующие пакеты:
- Компилятор g++ 4.9 и выше:
```
sudo apt-get update
sudo apt-get install g++-4.9
```
- CMake 2.8.4 и выше:
```
sudo apt-get install cmake
```

### [Mac OS X](#macosx)
- Компилятор Apple clang 6.0 и выше:
```
xcode-select --install
```
- Менеджер пакетов Homebrew:
```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
- CMake 2.8.4 и выше:
```
brew install cmake
```

### [Windows](#windows)
- Компилятор MinGW-w64 версии 4.9 и выше:

Установить [MinGW-w64](http://sourceforge.net/projects/mingw-w64/)
с настройками 
```
--rev=0 --bootstrap --jobs=4 --threads=posix --exceptions=seh --arch=x86_64
```
Добавить файл с бинарниками в переменную окружения *PATH*:
```
setx path "%path%;YOUR_PATH_TO_MINGW\x86_64-4.9.2-posix-seh-rt_v3-rev0\mingw64\bin"
```

- CMake 2.8.4 и выше:

Например, можно поставить [CLion](https://www.jetbrains.com/clion/download/), *CMake* будет в комплекте. Далее необходимо добавить путь до *cmake.exe* в переменную окружения *PATH*.

- Python 2.7 и выше. Можно скачать [здесь](https://www.python.org/downloads/).