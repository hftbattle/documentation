## Установка зависимостей

В данном разделе описаны зависимости, необходимые для следующих операционных систем:

- [Ubuntu](#ubuntu)
- [MacOS](#macos)
- [Windows](#windows)

### Ubuntu {#ubuntu}

Для установки зависимостей на Ubuntu можно запустить скрипт *packages_ubuntu.sh*, находящийся в корне репозитория:

```bash
./packages_ubuntu.sh
```

Скрипт устанавливает:

- Компилятор g++:

  ```bash
  sudo apt-get install gcc
  ```

- [CMake]({{ book["cmake.url"] }}):

  ```bash
  sudo apt-get install cmake
  ```

### MacOS {#macos}

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

### Windows {#windows}

Для запуска под Windows необходимо иметь:

- Компилятор [MinGW-w64]({{ book["mingw.url"] }}) версии `5.0` и выше:

  Необходимо установить скачанный компилятор с настройками:

  ```bash
  --rev=0 --bootstrap --jobs=4 --threads=posix --exceptions=seh --arch=x86_64
  ```

  Затем следует добавить файл с бинарниками в переменную окружения *path*.
  Не забудьте поменять версию компилятора:

  ```bash
  setx path "%path%;YOUR_PATH_TO_MINGW\x86_64-5.0.1-posix-seh-rt_v3-rev0\mingw64\bin"
  ```

- [CMake]({{ book["cmake.url"] }}) версии 2.8.4 и выше:

  Например, можно поставить [CLion]({{ book["clion.url"] }}), *CMake* при этом будет в комплекте.
  Далее необходимо добавить путь до *cmake.exe* в переменную окружения *PATH*.

- Python 2.7 и выше.

  Можно скачать [здесь]({{ book["python.url"] }}).
