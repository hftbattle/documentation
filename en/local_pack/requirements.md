## Dependency management

Here you can find dependency management suggestions for the following operating systems:

- [Ubuntu](#ubuntu)
- [macOS](#macos)
- [Windows](#windows)

### Ubuntu {#ubuntu}

Run *packages_ubuntu.sh* to install dependencies:

```bash
sudo ./packages_ubuntu.sh
```

The script installs g++, [CMake]({{ book["cmake.url"] }}) and Python 2:

```bash
sudo apt-get update && sudo apt-get install g++ cmake python --yes
```

### macOS {#macos}

Run *packages_mac.sh* to install dependencies:

```bash
./packages_mac.sh
```
The script installs:

- Apple LLVM compiler (you can find detailed installation instruction [here](http://osxdaily.com/2014/02/12/install-command-line-tools-mac-os-x/)):

  ```bash
  xcode-select --install
  ```

- [Homebrew]({{ book["brew.url"] }}) package manager:

  ```bash
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```

- [CMake]({{ book["cmake.url"] }}), [JsonCpp]({{ book["jsoncpp.url"] }}) and Python 2:

  ```bash
  brew install cmake jsoncpp python
  ```

### Windows {#windows}

It's essential to have the following to run local pack under Windows:

- [TDM64-GCC]({{ book["tdm-gcc.url"] }}) compiler at least version `5.1.0`:

  You may download it [here]({{ book["tdm-gcc-download.url"] }}).
  Note that you need **TDM64-gcc**.

- [CMake]({{ book["cmake.url"] }}) at least version `2.8.4`:

  You may install [CLion]({{ book["clion-download.url"] }}), or [download]({{ book["cmake.url"] }}) **CMake** from the official website.

- Python `2.7`:

  You may download it [here]({{ book["python.url"] }}).

Make sure that paths to *cmake.exe*, *python.exe*, *g++.exe* and *mingw32-make.exe*/*make.exe* are added to *PATH* environment variable.
