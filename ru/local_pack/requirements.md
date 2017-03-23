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

Скрипт устанавливает компилятор g++, систему сборки [CMake]({{ book["cmake.url"] }}) и Python 2:

```bash
sudo apt-get update && sudo apt-get install g++ cmake python --yes
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

- [CMake]({{ book["cmake.url"] }}), [JsonCpp]({{ book["jsoncpp.url"] }}) и Python 2:

  ```bash
  brew install cmake jsoncpp python
  ```

### Windows {#windows}

Для запуска под Windows необходимо иметь:

- Компилятор [TDM64-GCC]({{ book["tdm-gcc.url"] }}) версии `5.1.0` и выше:

  Скачать можно [здесь]({{ book["tdm-gcc-download.url"] }}).
  Просим вас обратить внимание на то, что нужна именно версия **TDM64-gcc**.

- [CMake]({{ book["cmake.url"] }}) версии `2.8.4` и выше:

  Например, можно поставить [CLion]({{ book["clion-download.url"] }}), *CMake* при этом будет в комплекте.

  Или же [скачать CMake]({{ book["cmake.url"] }}) с официального сайта.

- Python версии `2.7`:

  Скачать можно [здесь]({{ book["python.url"] }}).

Убедитесь, что пути до *cmake.exe*, *python.exe*, *g++.exe* и *mingw32-make.exe*/*make.exe* добавлены в переменную окружения *PATH*.
