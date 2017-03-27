## Dependency handling

Here you can find dependency handling suggestions for the following operating systems:

- [Ubuntu](#ubuntu)
- [macOS](#macos)
- [Windows](#windows)

### Ubuntu {#ubuntu}

To install dependencies run *packages_ubuntu.sh*:

```bash
sudo ./packages_ubuntu.sh
```

The script installs g++, [CMake]({{ book["cmake.url"] }}) and Python 2:

```bash
sudo apt-get update && sudo apt-get install g++ cmake python --yes
```

### macOS {#macos}

To install dependencies run *packages_mac.sh*:

```bash
./packages_mac.sh
```
The script installs:

- Apple LLVM compilator (you may find detailed installation description [here](http://osxdaily.com/2014/02/12/install-command-line-tools-mac-os-x/)):

  ```bash
  xcode-select --install
  ```

- [Homebrew]({{ book["brew.url"] }}) packet manager:

  ```bash
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```

- [CMake]({{ book["cmake.url"] }}), [JsonCpp]({{ book["jsoncpp.url"] }}) and Python 2:

  ```bash
  brew install cmake jsoncpp python
  ```

### Windows {#windows}

To run local pack under Windows, it's essential to have:

- [TDM64-GCC]({{ book["tdm-gcc.url"] }}) compliator at least `5.1.0` version:

  You may download it [here]({{ book["tdm-gcc-download.url"] }}).
  Note that you need **TDM64-gcc**.

- [CMake]({{ book["cmake.url"] }}) at least `2.8.4`:

  You may install [CLion]({{ book["clion-download.url"] }}), or [download]({{ book["cmake.url"] }}) **CMake** from official website.

- Python `2.7`:

  You may download it [here]({{ book["python.url"] }}).

Get sure that paths to *cmake.exe*, *python.exe*, *g++.exe* and *mingw32-make.exe*/*make.exe* are added to environmental variable *PATH*.
