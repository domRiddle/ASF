# Аргументы командной строки

ASF поддерживает несколько аргументов командной строки, которые влияют на работу программы. Они могут использоваться опытными пользователями для изменения поведения программы. По сравнению с настройкой через файл конфигурации `ASF.json`, аргументы командной строки используются для инициализации ядра (например, `--path`), специфичные настройки для данной платформы (например, `--system-required`) или конфиденциальных данных (например, `--cryptkey`).

* * *

## Использование

Способ использования аргументов зависит от вашей операционной системы и вариации ASF.

Общий:

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

Аргументы командной строки также поддерживаются в вспомогательных скриптах, таких как `ArchiSteamFarm.cmd` или `ArchiSteamFarm.sh`. В дополнении к этому, при использовании вспомогательных скриптов вы можете использовать переменные окружения `ASF_ARGS`, как указано в разделе **[Docker](https://github. com/JustArchi/ArchiSteamFarm/wiki/Docker-ru-RU#Аргументы-командной-строки)**.

Если ваш аргумент содержит пробелы, не забудьте заключить его в кавычки. Эти два примера неправильные:

```shell
./ArchiSteamFarm --path /home/archi/My Downloads/ASF # Нельзя!
./ArchiSteamFarm --path=/home/archi/My Downloads/ASF # Нельзя!
```

Однако, эти два абсолютно корректны:

```shell
./ArchiSteamFarm --path "/home/archi/My Downloads/ASF" # Нормально
./ArchiSteamFarm "--path=/home/archi/My Downloads/ASF" # Тоже нормально
```

## Аргументы

`--cryptkey <key>` или `--cryptkey=<key>` - запустит ASF с пользовательским значением ключа шифрования `<key>`. Эта настройка влияет на **[безопасность](https://github. com/JustArchi/ArchiSteamFarm/wiki/Security-ru-RU)** и ASF будет использовать данный ключ шифрования `<key>` вместо внедрённого в исполняемый файл. Имейте в виду, что ключ, которым были зашифрованы пароли, должен передаваться при каждом запуске ASF.

* * *

`--no-restart` - данный параметр используется для контейнеров **[Docker](https://github.com/JustArchi/ArchiSteamFarm/wiki/Docker)** и принудительно задаёт параметру `AutoRestart` значение `false`. Unless you have a particular need, you should instead configure `AutoRestart` property directly in your config. This switch is here so our docker script won't need to touch your global config in order to adapt it to its own environment. Of course, if you're running ASF inside a script, you might also make use of this switch (otherwise you're better with global config property).

* * *

`--path <path>` or `--path=<path>` - ASF always navigates to its own directory on startup. By specifying this argument, ASF will navigate to given directory after initialization, which allows you to use custom path for `config` directory without a need of duplicating binary in the same place. It might come especially useful if you'd like to separate binary from actual config, as it's done in Linux-like packaging - this way you can use one (up-to-date) binary with several different setups. The path can be either relative according to current place of ASF binary, or absolute. When running multiple instances of the same binary, keep in mind that you should typically disable auto-updates, as there is no synchronization between them. Also keep in mind that this command points to new "ASF home" - the directory that has the same structure as original ASF, with `config` directory inside.

Пример:

```shell
dotnet /opt/ASF/ArchiSteamFarm.dll --path /opt/TargetDirectory # Absolute path
dotnet /opt/ASF/ArchiSteamFarm.dll --path ../TargetDirectory # Relative path works as well
```

    ├── /opt
    │     ├── ASF
    │     │     ├── ArchiSteamFarm.dll
    │     │     └── ...
    │     └── TargetDirectory
    │           └── config
    └── ...
    

* * *

`--process-required` - declaring this switch will disable default ASF behaviour of shutting down when no bots are running. No auto-shutdown behaviour is especially useful in combination with **[IPC](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC)** where majority of users would expect their web service to be running regardless of the amount of bots that are enabled. If you're using IPC option or otherwise need ASF process to be running all the time until you close it yourself, this is the right option.

If you do not intend to run IPC, this option will be rather useless for you, as you can just start the process again when needed (as opposed to ASF's web server where you require it listening all the time in order to send commands).

* * *

`--system-required` - declaring this switch will cause ASF to try signalizing the OS that the process requires system to be up and running for its entire lifetime. Currently this switch has effect only on Windows machines where it'll forbid your system from going into sleep mode as long as the process is running. This can be proven especially useful when idling on your PC or laptop during night, as ASF will be able to keep your system awake while it's idling, then, once ASF is done, it'll shutdown itself like usual, making your system allowed to enter into sleep mode again, therefore saving power immediately once idling is finished.

Keep in mind that for proper auto-shutdown of ASF you need other settings - especially avoiding `--process-required` and ensuring that all your bots are following `ShutdownOnFarmingFinished`. Of course, auto-shutdown is only a possibility for this feature, not a requirement, since you can also use this flag together with e.g. `--process-required`, effectively making your system awake infinitely after starting ASF.