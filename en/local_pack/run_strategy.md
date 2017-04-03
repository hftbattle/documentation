## Execution of your strategy

This chapter describes execution of your strategy:

- [Execution in command line](#command_line)
- [CLion usage](#clion)

We will assume, that your strategy and folder are called **user_strategy**.
Read [here](add_strategy.md) about creating new strategies.

### Execution in command line {#command_line}

You need to:

- **compile all the strategies you have**:

  ```bash
  ./build.py
  ```

  This will create libraries for your strategies in the *build* folder.
- **run a simulation**.

  ```bash
  ./run.py user_strategy
  ```

### CLion usage {#clion}

To launch simulation in [CLion](({{ book["clion-download.url"] }}) you need to:

- **specify executable file**:

  open *Run > Edit configurations*, specify executable file in the root of directory as *Executable*:

  - *mac_launcher* for macOS
  - *windows_launcher* for Windows
  - *linux_launcher* for Linux

- **specify path to your configuration file** in the arguments of command line:

  in the same tab *Run > Edit configurations* you need to fill *Program arguments* with relative path to configuration file of your strategy.
  For example:

  ```bash
  strategies/user_strategy/user_strategy.json
  ```

- **start building the project and simulating strategy** by pressing *Run* button.
