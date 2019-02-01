# 指令參數

ASF 支持一些能夠影響程式運行時的命令行參數。 高級用戶可使用這些參數以定義程式運行方式。 相較使用 `ASF.json `配置文件的預設方式，命令行參數可用於核心初始化（如` --path`）、平臺特定設置（如` --system-required`）或敏感數據（如` --cryptkey`）。

* * *

## 使用方法

使用方法取決於您的操作系統和ASF偏好。

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

命令行參數也可用於通用輔助腳本中，例如`ArchiSteamFarm.cmd`或`ArchiSteamFarm.sh`。 除此之外，如在**[Docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker#command-line-arguments)**一節中所述，使用輔助腳本時，您也可使用`ASF_ARGS`環境變數。

若您的參數中存在空格，請務必要使用引號將其括住。 兩個錯誤示例：

```shell
./ArchiSteamFarm --path /home/archi/My Downloads/ASF # Bad!
./ArchiSteamFarm --path=/home/archi/My Downloads/ASF # Bad!
```

兩個正確示例：

```shell
./ArchiSteamFarm --path "/home/archi/My Downloads/ASF" # OK
./ArchiSteamFarm "--path=/home/archi/My Downloads/ASF" # OK
```

## 參數

`--cryptkey<key>`  或 `--cryptkey=<key>`將以值為`<code><key>`</code>的自定義密鑰啟動 ASF。 此參數將導致 ASF 使用您所提供的自定義密鑰`<key>`， 而非硬編碼在程式中的預設密鑰, 且影響**[​安全性](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security)**​。 請謹記使用此密鑰加密的密碼在每次 ASF 運行時傳遞相同的密鑰才能正確解密。

* * *

`--no-restart` 此開關主要用於 **[Docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker)** 容器並將 `AutoRestart` 強制設置為 `false`。 除非有特殊的需要，否則您應直接在描述檔中配置 `AutoRestart` 屬性。這個開關使 Docker 腳本不必修改您的全局配置即可適應環境。 當然，如果是在腳本中運行 ASF，您也可以使用此開關（否則您最好使用全局配置屬性）。

* * *

`--path <path>` or `--path=<path>` - ASF always navigates to its own directory on startup. By specifying this argument, ASF will navigate to given directory after initialization, which allows you to use custom path for `config` (and optionally also `www`) directory without a need of duplicating binary in the same place. It might come especially useful if you'd like to separate binary from actual config, as it's done in Linux-like packaging - this way you can use one (up-to-date) binary with several different setups. The path can be either relative according to current place of ASF binary, or absolute. When running multiple instances of the same binary, keep in mind that you should typically disable auto-updates, as there is no synchronization between them. Also keep in mind that this command points to new "ASF home" - the directory that has the same structure as original ASF, with `config` directory inside.

範例:

```shell
dotnet /opt/ASF/ArchiSteamFarm.dll --path /opt/TargetDirectory # Absolute path
dotnet /opt/ASF/ArchiSteamFarm.dll --path ../TargetDirectory # Relative path works as well
```

    ├── /opt
    │     ├── ASF
    │     │     ├── ArchiSteamFarm.dll
    │     │     └── ...
    │     └── TargetDirectory
    │           ├── config
    │           └── www (optional)
    └── ...
    

* * *

`--process-required` - declaring this switch will disable default ASF behaviour of shutting down when no bots are running. No auto-shutdown behaviour is especially useful in combination with **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** where majority of users would expect their web service to be running regardless of the amount of bots that are enabled. If you're using IPC option or otherwise need ASF process to be running all the time until you close it yourself, this is the right option.

If you do not intend to run IPC, this option will be rather useless for you, as you can just start the process again when needed (as opposed to ASF's web server where you require it listening all the time in order to send commands).

* * *

`--system-required` - declaring this switch will cause ASF to try signalizing the OS that the process requires system to be up and running for its entire lifetime. Currently this switch has effect only on Windows machines where it'll forbid your system from going into sleep mode as long as the process is running. This can be proven especially useful when idling on your PC or laptop during night, as ASF will be able to keep your system awake while it's idling, then, once ASF is done, it'll shutdown itself like usual, making your system allowed to enter into sleep mode again, therefore saving power immediately once idling is finished.

Keep in mind that for proper auto-shutdown of ASF you need other settings - especially avoiding `--process-required` and ensuring that all your bots are following `ShutdownOnFarmingFinished`. Of course, auto-shutdown is only a possibility for this feature, not a requirement, since you can also use this flag together with e.g. `--process-required`, effectively making your system awake infinitely after starting ASF.