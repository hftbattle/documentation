##Execution of your strategy

This chapter describes execution of your strategy:
- [Execution in command line](#command_line)
- [CLion usage](#clion)

We will assume, that your strategy and folder are called **user_strategy**.
Read [here](add_strategy.md) about creating new strategies.

###Execution in command line<a id="command_line"></a>

You need to:
- **compile all the strategies you have**:
```bash
./build.py
```

This will create libraries for your strategies in *build* folder.

- *run a simulation**.
Attention: your strategy may have another name:
```
./run.py user_strategy
```

###CLion usage<a id="clion"></a>

To launch simulation in [CLion](({{ book["clion.url"] }}) you need to:

- **specify executable file**:
open *Run > Edit configurations*, specify executable file in the root of directory as *Executable* (*mac_launcher* for MacOS, *windows_launcher* for Windows and *linux_launcher* for Linux).

- **specify path to your configuration file** in the arguments of command line:
in the same tab *Run > Edit configurations* you need to fill *Program arguments* with relative path to configuration file of your strategy.
For example:
```
strategies/user_strategy/user_strategy.json
```

- **to start the project build and simulation**, by pressing *Run* button.
