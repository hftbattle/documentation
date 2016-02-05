##Установка зависимостей
### [Ubuntu](#ubuntu)
Для установки зависимостей на Ubuntu можно запустить скрипт *packages_ubuntu.sh*:
```
./packages_ubuntu.sh
```

Скрипт устанавливает следующие пакеты:
- Компилятор g++ 4.9,

- [CMake](https://cmake.org/) 2.8.4 и выше:
```
sudo apt-get install cmake
```

### [Mac OS X](#macosx)
Для установки зависимостей на Mac OS X можно запустить скрипт *packages_mac.sh*:
```
./packages_mac.sh
```

Скрипт устанавливает следующие пакеты:
- Компилятор Apple LLVM 6.0 и выше (подробное описание процедуру установки [здесь](http://osxdaily.com/2014/02/12/install-command-line-tools-mac-os-x/)):
```
xcode-select --install
```
- Менеджер пакетов [Homebrew](http://brew.sh/):
```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
- [CMake](https://cmake.org/) 2.8.4 и выше:
```
brew install cmake
```

### [Windows](#windows)
- Компилятор [MinGW-w64](http://sourceforge.net/projects/mingw-w64/) версии 4.9 и выше:

Необходимо установить скачанный компилятор с настройками:
```
--rev=0 --bootstrap --jobs=4 --threads=posix --exceptions=seh --arch=x86_64
```
Затем следует добавить файл с бинарниками в переменную окружения *path*:
```
setx path "%path%;YOUR_PATH_TO_MINGW\x86_64-4.9.2-posix-seh-rt_v3-rev0\mingw64\bin"
```

- [CMake](https://cmake.org/) 2.8.4 и выше:

Например, можно поставить [CLion](https://www.jetbrains.com/clion/download/), *CMake* будет в комплекте. Далее необходимо добавить путь до *cmake.exe* в переменную окружения *PATH*.

- Python 2.7 и выше. Можно скачать [здесь](https://www.python.org/downloads/).