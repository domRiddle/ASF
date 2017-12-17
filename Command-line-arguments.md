# Command-line arguments

ASF includes support for several command-line arguments that can affect the program runtime. Those can be used by advanced users in order to specify how program should run. In comparison with default way of `ASF.json` configuration file, command-line arguments are used for core initialization (e.g. `--path`) or sensitive data (e.g. `--cryptkey`).

---

## Usage

Usage depends on your OS and ASF flavour.

Generic:

```
dotnet ArchiSteamFarm.dll --argument --otherOne
```

Windows:

```
ArchiSteamFarm.exe --argument --otherOne
```

Linux/OS X

```
./ArchiSteamFarm --argument --otherOne
```

Command-line arguments are also supported in generic helper scripts such as `ArchiSteamFarm.cmd` or `ArchiSteamFarm.sh`. In addition to that, when using helper script you can also use `ASF_ARGS` environment property, like stated in our **[docker](https://github.com/JustArchi/ArchiSteamFarm/wiki/Docker#command-line-arguments)** section.

## Arguments

`--cryptkey <key>` or `--cryptkey=<key>` - will start ASF with custom cryptographic key of `<key>` value. This option affects **[security](https://github.com/JustArchi/ArchiSteamFarm/wiki/Security)** and will cause ASF to use your custom provided `<key>` key instead of default one hardcoded into the executable. Keep in mind that passwords encrypted with this key will require it to be passed on each ASF run.

`--path <path>` or `--path=<path>` - ASF always navigates to its own directory on startup. By specifying this argument, ASF will navigate to given directory after initialization, which allows you to use custom directory for `config` and `log.txt` without a need of duplicating binary in that directory. It might come especially useful if you'd like to separate binary from actual config, as it's done in Linux-like packaging - this way you can use one (up-to-date) binary with several different setups. The path can be either relative according to current place of ASF binary, or absolute. When running multiple instances of the same binary, keep in mind that only one should have `AutoUpdates: true` setting, as there is no synchronization between instances.

`--server` - will start ASF in `Server` mode. This option affects **[IPC](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC)**, as well as disables auto-exit of ASF process when no bots are enabled (which is wanted behaviour in combination with IPC).

`--service` - this switch is mainly used by our **[docker](https://github.com/JustArchi/ArchiSteamFarm/wiki/Docker)** containers and causes ASF to force `AutoRestart: false` and not exiting when all bots are stopped (like in `--server`).