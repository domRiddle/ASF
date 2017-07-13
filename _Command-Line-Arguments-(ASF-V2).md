# Command-Line Arguments

ASF includes support for several command-line arguments that can affect the program runtime. Those can be used by advanced users in order to specify how program should run. In comparison with default way of ```ASF.json``` configuration file, command-line arguments are supposed to be dynamic and not static, allowing you to e.g. use one ASF config for running both ```Server``` and ```Client``` WCF mode of ASF, without duplicating your configs. It's also preferred way when it comes to sensitive data, such as ```cryptkey```, as defining it in config defeats whole purpose of it. They're also used for core ASF initialization, such as when specifying ```path```, as config can't be loaded without knowing it.

The order of specifying them matters, meaning that it's possible to combine e.g. ```--client exit --server``` in order to switch into client mode, execute ```!exit``` on remote ASF, then start another instance with ```--server```. This can be useful in several situations, like the one above, which would be impossible to do with only one program call otherwise.

---

## Usage

On Windows, as usual:

```
ASF.exe --argument --otherOne
```

On Mono:

```
mono ASF.exe --argument --otherOne
```

**Notice:** Keep in mind that arguments provided before ```ASF.exe``` in Mono will affect Mono runtime, therefore you must specify ASF command-line arguments after ```ASF.exe```, not before.

## Arguments

```--server``` - will start ASF in ```Server``` mode. This option affects **[WCF](https://github.com/JustArchi/ArchiSteamFarm/wiki/WCF)**, as well as disables auto-exit of ASF process when no bots are enabled (which is wanted behaviour in combination with WCF).

```--client``` - will start ASF in ```Client``` mode. This option affects **[WCF](https://github.com/JustArchi/ArchiSteamFarm/wiki/WCF)**, and will cause ASF to shutdown after last command specified, unless you switch the mode back to ```--server``` after executing last command.

```--cryptkey=XXX``` - will start ASF with custom cryptographic key of ```XXX``` value. This option affects **[Security](https://github.com/JustArchi/ArchiSteamFarm/wiki/Security)** and will cause ASF to use your custom provided ```XXX``` key instead of default one hardcoded into the executable.

```--path=XXX``` - ASF always navigates to its own directory on startup. By specifying this argument, ASF will navigate to given directory after initialization, which allows you to use custom directory for ```config``` and ```log.txt``` without a need of duplicating binary in that directory. It might come especially useful if you'd like to separate binary from actual config, as it's done in Linux-like packaging - this way you can use one (up-to-date) binary with several different setups. The path can be either relative according to current place of ASF binary, or absolute.

```"actual command"``` - will execute ```!actual command``` on remotely connected ASF running in ```Server``` mode. This option affects **[WCF](https://github.com/JustArchi/ArchiSteamFarm/wiki/WCF)**, and requires ```--client``` to be provided firstly. Please note that if command contains space, it should be quoted, as stated in WCF section.