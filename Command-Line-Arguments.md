# Command-Line Arguments

## This page applies to ASF V3 only. If you're using ASF V2, please visit **[ASF V2](https://github.com/JustArchi/ArchiSteamFarm/wiki/_Command-Line-Arguments-(ASF-V2))** page instead.

ASF includes support for several command-line arguments that can affect the program runtime. Those can be used by advanced users in order to specify how program should run. In comparison with default way of `ASF.json` configuration file, command-line arguments are used for core initialization (e.g. `--path`) or sensitive data (e.g. `--cryptkey`).

---

## Usage

Usage depends on your OS and ASF flavour.

Generic:

```
dotnet ArchiSteamFarm.dll -- --argument --otherOne
```

Windows:

```
ArchiSteamFarm.exe --argument --otherOne
```

Linux/OS X

```
./ArchiSteamFarm --argument --otherOne
```

## Arguments

`--cryptkey=XXX` - will start ASF with custom cryptographic key of `XXX` value. This option affects **[Security](https://github.com/JustArchi/ArchiSteamFarm/wiki/Security)** and will cause ASF to use your custom provided `XXX` key instead of default one hardcoded into the executable. Keep in mind that passwords encrypted with this key will require it to be passed on each ASF run.

`--path=XXX` - ASF always navigates to its own directory on startup. By specifying this argument, ASF will navigate to given directory after initialization, which allows you to use custom directory for `config` and `log.txt` without a need of duplicating binary in that directory. It might come especially useful if you'd like to separate binary from actual config, as it's done in Linux-like packaging - this way you can use one (up-to-date) binary with several different setups. The path can be either relative according to current place of ASF binary, or absolute.

`--server` - will start ASF in `Server` mode. This option affects **[IPC](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC)**, as well as disables auto-exit of ASF process when no bots are enabled (which is wanted behaviour in combination with IPC). It also tweaks runtime to be better suited for server usage, for example by not allowing Windows OS to go into sleep mode.