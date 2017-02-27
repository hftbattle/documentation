## Установка зависимостей

В данном разделе описаны зависимости, необходимые для следующих операционных систем:

- [Ubuntu](#ubuntu)
- [macOS](#macos)
- [Windows](#windows)

### Ubuntu {#ubuntu}

Для установки зависимостей на Ubuntu можно запустить скрипт *packages_ubuntu.sh*, находящийся в корне репозитория:

```bash
sudo ./packages_ubuntu.sh
```

Скрипт устанавливает компилятор g++, систему сборки [CMake]({{ book["cmake.url"] }}) и Python3:

```bash
sudo apt-get update && sudo apt-get install g++ cmake python3 --yes
```

### macOS {#macos}

Для установки зависимостей на macOS можно запустить скрипт *packages_mac.sh*, находящийся в корне репозитория:

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

- [CMake]({{ book["cmake.url"] }}) и Python3:

  ```bash
  brew install cmake python3
  ```

### Windows {#windows}

Для запуска под Windows необходимо иметь:

- Компилятор [TDM64-GCC]({{ book["tdm-gcc.url"] }}) версии `5.1.0` и выше:

  Скачать можно [здесь]({{ book["tdm-gcc-download.url"] }}).

- [CMake]({{ book["cmake.url"] }}) версии `2.8.4` и выше:

  Например, можно поставить [CLion]({{ book["clion-download.url"] }}), *CMake* при этом будет в комплекте.

  Или же [скачать CMake]({{ book["cmake.url"] }}) с официального сайта.

- Python `2.7` и выше:

  Скачать можно [здесь]({{ book["python.url"] }}).

Убедитесь, что пути до *cmake.exe*, *python.exe*, *g++.exe* и *mingw32-make.exe*/*make.exe* добавлены в переменную окружения *PATH*.
