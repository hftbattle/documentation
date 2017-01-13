##Execution of your strategy

This chapter describes execution of your strategy:
* [Execution in command line](#command_line)
* [CLion usage](#clion)

<a id="command_line"></a>
###Execution in command line

To create a strategy, create a folder with the name of your strategy in the strategies folder. We will use the name *user strategy* further. Your strategy can have different name.

Your strategy must be implemented in one file, in our case *user.strategy.cpp*. Corresponding configuration file must have the same name, so, in our case, *user_strategy.json*.

After that you need you need to **compile all the strategies you have**:
```
./build.py
```

This will create a library for your strategy in *build* folder.

To *run a simulation*, press:
```
./run.py strategies/user_strategy/user_strategy.json
```

<a id="clion"></a>
###CLion usage
To launch simulation in [CLion](https://www.jetbrains.com/clion/download/) you need to:
- **specify executable file**:
open *Run > Edit configurations*, specify executable file in the root of directory as *Executable* (*mac_launcher* for MacOS, *windows_launcher* for Windows and *linux_launcher* for Linux).

- **specify path to your configuration file in the arguments of command line**:
in the same tab *Run > Edit configurations* you need to fill *Program arguments* with relative path to configuration file of your strategy. For example:
```
strategies/user_strategy/user_strategy.json
```
- **to start the project build and simulation**, by pressing *Run* button.
