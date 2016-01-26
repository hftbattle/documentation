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

- python

опять же не забыть добавить путь до python.exe в PATH.