# Kommandolinje argumenter

ASF includes support for several command-line arguments that can affect the program runtime. Those can be used by advanced users in order to specify how program should run. In comparison with default way of `ASF.json` configuration file, command-line arguments are used for core initialization (e.g. `--path`), platform-specific settings (e.g. `--system-required`) or sensitive data (e.g. `--cryptkey`).

* * *

## Usage

Usage depends on your OS and ASF flavour.

Generic:

```shell
dotnet ArchiSteamFarm.dll --argument --otherOne
```

Windows:

```powershell
.\ArchiSteamFarm.exe --argument --otherOne
```

Linux/OS X

```shell
./ArchiSteamFarm --argument --otherOne
```

Command-line arguments are also supported in generic helper scripts such as `ArchiSteamFarm.cmd` or `ArchiSteamFarm.sh`. In addition to that, when using helper scripts you can also use `ASF_ARGS` environment property, like stated in our **[docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker#command-line-arguments)** section.

If your argument includes spaces, don't forget to quote it. Those two are wrong:

```shell
./ArchiSteamFarm --path /home/archi/My Downloads/ASF # Bad!
./ArchiSteamFarm --path=/home/archi/My Downloads/ASF # Bad!
```

However, those two are completely fine:

```shell
./ArchiSteamFarm --path "/home/archi/My Downloads/ASF" # OK
./ArchiSteamFarm "--path=/home/archi/My Downloads/ASF" # OK
```

## Argumenter

`--cryptkey <key>` or `--cryptkey=<key>` - will start ASF with custom cryptographic key of `<key>` value. This option affects **[security](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security)** and will cause ASF to use your custom provided `<key>` key instead of default one hardcoded into the executable. Keep in mind that passwords encrypted with this key will require it to be passed on each ASF run.

Due to the nature of this property, it's also possible to set cryptkey by declaring `ASF_CRYPTKEY` environment variable, which may be more appropriate for people that would want to avoid sensitive details in the process arguments.

* * *

`--network-group <group>` or `--network-group=<group>` - will cause ASF to init its limiters with a custom network group of `<group>` value. This option affects running ASF in **[multiple instances](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#multiple-instances)** by signalizing that given instance is dependent only on instances sharing the same network group, and independent of the rest. Typically you want to use this property only if you're routing ASF requests through custom mechanism (e.g. different IP addresses) and you want to set networking groups yourself, without relying on ASF to do it automatically (which currently includes taking into account `WebProxy` only). Keep in mind that when using a custom network group, this is unique identifier within the local machine, and ASF will not take into account any other details, such as `WebProxy` value, allowing you to e.g. start two instances with different `WebProxy` values which are still dependent on each other.

Due to the nature of this property, it's also possible to set the value by declaring `ASF_NETWORK_GROUP` environment variable, which may be more appropriate for people that would want to avoid sensitive details in the process arguments.

* * *

`--no-restart` - this switch is mainly used by our **[docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker)** containers and forces `AutoRestart` of `false`. Unless you have a particular need, you should instead configure `AutoRestart` property directly in your config. This switch is here so our docker script won't need to touch your global config in order to adapt it to its own environment. Of course, if you're running ASF inside a script, you may also make use of this switch (otherwise you're better with global config property).

* * *

`--path <path>` or `--path=<path>` - ASF always navigates to its own directory on startup. By specifying this argument, ASF will navigate to given directory after initialization, which allows you to use custom path for various application parts (including `config`, `plugins` and `www` directories, as well as `NLog.config` file), without a need of duplicating binary in the same place. It may come especially useful if you'd like to separate binary from actual config, as it's done in Linux-like packaging - this way you can use one (up-to-date) binary with several different setups. The path can be either relative according to current place of ASF binary, or absolute. Husk, at denne kommando peger på et nyt "ASF-hjem" - det bibliotek, der har den samme struktur som den originale ASF, med konfigurationsmappen inde i, se eksemplet nedenfor til forklaring.

Due to the nature of this property, it's also possible to set expected path by declaring `ASF_PATH` environment variable, which may be more appropriate for people that would want to avoid sensitive details in the process arguments.

Hvis du overvejer at bruge dette kommandolinje argument til at køre flere forekomster af ASF, anbefaler vi at læse vores **[compatibility page](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#multiple-instances)** på denne måde.

Eksempler:

```shell
dotnet /opt/ASF/ArchiSteamFarm.dll --path /opt/TargetDirectory # Absolute path
dotnet /opt/ASF/ArchiSteamFarm.dll --path ../TargetDirectory # Relative path works as well
ASF_PATH=/opt/TargetDirectory dotnet /opt/ASF/ArchiSteamFarm.dll # Same as env variable
```

```text
├── /opt
│     ├── ASF
│     │     ├── ArchiSteamFarm.dll
│     │     └── ...
│     └── TargetDirectory
│           ├── config
│           ├── logs (generated)
│           ├── plugins (optional)
│           ├── www (optional)
│           ├── log.txt (generated)
│           └── NLog.config (optional)
└── ...
```

* * *

`--process-required` - hvis du erklærer denne switch, deaktiveres standard ASF opførsel ved at lukke ned, når ingen bots kører. Ingen auto-shutdown opførsel er især nyttig i kombination med **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)**, hvor flertallet af brugere forventer, at deres webservice kører uanset mængden af bots, der er aktiveret. Hvis du bruger IPC-indstilling eller på anden måde har brug for ASF-processen for at køre hele tiden, indtil du selv lukker den, er dette den rigtige mulighed.

Hvis du ikke har hensigt at køre IPC, vil denne indstilling være temmelig ubrugelig for dig, da du bare kan starte processen igen når det er nødvendigt (i modsætning til ASFs webserver, hvor du har brug for, at den lytter hele tiden for at sende kommandoer).

* * *

`--system-required` - Hvis du erklærer denne switch, vil ASF forsøge at signalere operativsystemet om, at processen kræver, at systemet er i gang i hele sin levetid. I øjeblikket har denne switch kun effekt på Windows-maskiner, hvor det vil forbyde dit system at gå i dvaletilstand, så længe processen kører. Dette kan bevises især nyttigt, når idling på din pc eller bærbar computer om natten, da ASF vil være i stand til at holde dit system vågent, mens det idler, så når ASF er færdig, lukker det sig som normalt, hvilket gør at dit system er tilladt at gå ind i dvaletilstand igen, og spare derfor strøm med det samme, når idling er færdig.

Husk, at for korrekt auto-shutdown af ASF har du brug for andre indstillinger - især undgå `--process-required` og sikre, at alle dine bots følger `ShutdownOnFarmingFinished`. Of course, auto-shutdown is only a possibility for this feature, not a requirement, since you can also use this flag together with e.g. `--process-required`, effectively making your system awake infinitely after starting ASF.