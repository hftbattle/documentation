## Strategy starting

There are two ways to to start the strategy:

- [Start from command line interface](#command_line)
- [Start from CLion](#clion)

We are going to use **user_strategy** as the strategy and its folder name.
Read [this](add_strategy.md) section to find out more on how to create new strategies.

### Starting from a command line interface {#command_line}

To start from a command line interface you should:

- **compile all existing strategies**:

  ```bash
  ./build.py
  ```

Libraries for your strategies will be created in the *build* folder.
- **start simulation**:

  ```bash
  ./run.py user_strategy
  ```

### Start from CLion {#clion}

To start from [CLion]({{ book["clion-download.url"] }}):

- **define the file to execute**:

 Open Run > Edit configurations and select *Executable* as an executable file in the root of the directory, depending of your operating system:

  - *mac_launcher* for macOS
  - *windows_launcher* for Windows
  - *linux_launcher* for Linux

- **specify strategy name** in the parameters of the command line:

To do that, you may specify a path to your strategy`s configuration file on the same tab *Run > Edit configurations* in the line *Program arguments*.

  ```bash
  strategies/user_strategy/user_strategy.json
  ```
- **start building the project and simulation** by clicking on the *Run* button.
