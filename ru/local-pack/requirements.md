##Установка зависимостей

В данном разделе описаны зависимости, необходимые для следующих операционных систем:
- [Ubuntu](#ubuntu)
- [MacOS](#macos)
- [Windows](#windows)

###Ubuntu<a id="ubuntu"></a>

Для установки зависимостей на Ubuntu можно запустить скрипт *packages_ubuntu.sh*, находящийся в корне репозитория:
```bash
./packages_ubuntu.sh
```

Скрипт устанавливает:
- Компилятор g++,

- [CMake]({{ book["cmake.url"] }}):
```bash
sudo apt-get install cmake
```

###MacOS<a id="macos"></a>

Для установки зависимостей на MacOS можно запустить скрипт *packages_mac.sh*, находящийся в корне репозитория:
```bash
./packages_mac.sh
```

Скрипт устанавливает:
- Компилятор Apple LLVM (подробное описание процедуру установки [здесь](http://osxdaily.com/2014/02/12/install-command-line-tools-mac-os-x/)):
```bash
xcode-select --install
```
- Менеджер пакетов [Homebrew]({{ book["brew.url"] }}):
```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
- [CMake]({{ book["cmake.url"] }}):
```bash
brew install cmake
```

###Windows<a id="windows"></a>

Для запуска под Windows необходимо иметь:
- Компилятор [MinGW-w64](http://sourceforge.net/projects/mingw-w64/) версии `4.9` и выше:

Необходимо установить скачанный компилятор с настройками:
```bash
--rev=0 --bootstrap --jobs=4 --threads=posix --exceptions=seh --arch=x86_64
```
Затем следует добавить файл с бинарниками в переменную окружения *path*.
Не забудьте поменять версию компилятора:
```
setx path "%path%;YOUR_PATH_TO_MINGW\x86_64-4.9.2-posix-seh-rt_v3-rev0\mingw64\bin"
```

- [CMake]({{ book["cmake.url"] }}) версии 2.8.4 и выше:

Например, можно поставить [CLion]({{ book["clion.url"] }}), *CMake* при этом будет в комплекте.
Далее необходимо добавить путь до *cmake.exe* в переменную окружения *PATH*.

- Python 2.7 и выше.
Можно скачать [здесь]({{ book["python.url"] }}).
