# 命令行参数

ASF 包含对多个可影响程序运行时的命令行参数支持。 高级用户可使用这些参数以指定程序的运行。 与默认使用 `ASF.json` 配置文件的方式对比，命令行参数可用于核心初始化（如 `--path`）、指定平台设置（如 `--system-required`）或敏感数据（如 `--cryptkey`）。

* * *

## 用法

用法基于您的操作系统及 ASF 变体。

通用：

```shell
dotnet ArchiSteamFarm.dll --参数 --另一个参数
```

Windows：

```powershell
.\ArchiSteamFarm.exe --参数 --另一个参数
```

Linux/macOS

```shell
./ArchiSteamFarm --参数 --另一个参数
```

如 `ArchiSteamFarm.cmd` 或 `ArchiSteamFarm.sh` 的通用帮助脚本也可使用命令行参数。 除此之外，在使用帮助脚本时您也可以使用 `ASF_ARGS` 环境变量，如在 **[docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker#command-line-arguments)** 一节中使用的变量。

若您参数包括空格，请记得要使用引号。 下面是两个错误示例：

```shell
./ArchiSteamFarm --path /home/archi/My Downloads/ASF # 错！
./ArchiSteamFarm --path=/home/archi/My Downloads/ASF # 错！
```

然而，下面两个例子完全正确：

```shell
./ArchiSteamFarm --path "/home/archi/My Downloads/ASF" # OK
./ArchiSteamFarm "--path=/home/archi/My Downloads/ASF" # OK
```

## 参数

`--cryptkey <key>` 或 `--cryptkey=<key>` - 将通过值为 `<key>` 的自定义密钥启动。 此参数影响**[安全性](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security)**且将导致 ASF 使用您所提供的自定义密钥 `<key>` 而非硬编码进程序中的默认密钥。 请记住使用此密钥加密的密码将需要在每次 ASF 运行时进行传递密钥。

* * *

`--no-restart` - this switch is mainly used by our **[docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker)** containers and forces `AutoRestart` of `false`. Unless you have a particular need, you should instead configure `AutoRestart` property directly in your config. This switch is here so our docker script won't need to touch your global config in order to adapt it to its own environment. Of course, if you're running ASF inside a script, you might also make use of this switch (otherwise you're better with global config property).

* * *

`--path <path>` or `--path=<path>` - ASF always navigates to its own directory on startup. By specifying this argument, ASF will navigate to given directory after initialization, which allows you to use custom path for `config` directory without a need of duplicating binary in the same place. It might come especially useful if you'd like to separate binary from actual config, as it's done in Linux-like packaging - this way you can use one (up-to-date) binary with several different setups. The path can be either relative according to current place of ASF binary, or absolute. When running multiple instances of the same binary, keep in mind that you should typically disable auto-updates, as there is no synchronization between them. Also keep in mind that this command points to new "ASF home" - the directory that has the same structure as original ASF, with `config` directory inside.

范例:

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

`--process-required` - declaring this switch will disable default ASF behaviour of shutting down when no bots are running. No auto-shutdown behaviour is especially useful in combination with **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** where majority of users would expect their web service to be running regardless of the amount of bots that are enabled. If you're using IPC option or otherwise need ASF process to be running all the time until you close it yourself, this is the right option.

If you do not intend to run IPC, this option will be rather useless for you, as you can just start the process again when needed (as opposed to ASF's web server where you require it listening all the time in order to send commands).

* * *

`--system-required` - declaring this switch will cause ASF to try signalizing the OS that the process requires system to be up and running for its entire lifetime. Currently this switch has effect only on Windows machines where it'll forbid your system from going into sleep mode as long as the process is running. This can be proven especially useful when idling on your PC or laptop during night, as ASF will be able to keep your system awake while it's idling, then, once ASF is done, it'll shutdown itself like usual, making your system allowed to enter into sleep mode again, therefore saving power immediately once idling is finished.

Keep in mind that for proper auto-shutdown of ASF you need other settings - especially avoiding `--process-required` and ensuring that all your bots are following `ShutdownOnFarmingFinished`. Of course, auto-shutdown is only a possibility for this feature, not a requirement, since you can also use this flag together with e.g. `--process-required`, effectively making your system awake infinitely after starting ASF.