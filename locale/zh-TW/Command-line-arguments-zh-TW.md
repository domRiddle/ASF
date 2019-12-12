# 指令參數

ASF 支援一些能夠影響程式執行階段的指令參數。 進階使用者可使用這些參數以定義程式執行方式。 相較使用 `ASF.json `設定檔的預設方式，指令參數可用於核心初始化（如` --path`）、平台特定設定（如` --system-required`）或敏感資料（如` --cryptkey`）。

* * *

## 使用

使用方法取決於您的作業系統和ASF偏好。

通用：

```shell
dotnet ArchiSteamFarm.dll --參數 --另一個參數
```

Windows：

```powershell
.\ArchiSteamFarm.exe --參數 --另一個參數
```

Linux/OS X

```shell
./ArchiSteamFarm --參數 --另一個參數
```

指令參數也可用於通用輔助腳本中，例如`ArchiSteamFarm.cmd`或`ArchiSteamFarm.sh`。 除此之外，如在 **[Docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker-zh-TW#指令參數)** 章節中所述，使用輔助腳本時，您也可使用 `ASF_ARGS` 環境屬性。

若您的參數中存在空格，請務必要使用引號將其括住。 兩個錯誤示例：

```shell
./ArchiSteamFarm --path /home/archi/My Downloads/ASF # 錯誤！
./ArchiSteamFarm --path=/home/archi/My Downloads/ASF # 錯誤！
```

兩個正確示例：

```shell
./ArchiSteamFarm --path "/home/archi/My Downloads/ASF" # 正確
./ArchiSteamFarm "--path=/home/archi/My Downloads/ASF" # 正確
```

## 參數

`--cryptkey <key>` or `--cryptkey=<key>` - will start ASF with custom cryptographic key of `<key>` value. This option affects **[security](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security)** and will cause ASF to use your custom provided `<key>` key instead of default one hardcoded into the executable. Keep in mind that passwords encrypted with this key will require it to be passed on each ASF run.

Due to the nature of this property, it's also possible to set cryptkey by declaring `ASF_CRYPTKEY` environment variable, which may be more appropriate for people that would want to avoid sensitive details in the process arguments.

* * *

`--no-restart` - this switch is mainly used by our **[docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker)** containers and forces `AutoRestart` of `false`. 除非有特殊的需要，否則您應直接在描述檔中設定 `AutoRestart` 屬性。這個開關使 Docker 腳本不必修改您的全域設定檔即可適應環境。 Of course, if you're running ASF inside a script, you may also make use of this switch (otherwise you're better with global config property).

* * *

`--path <path>` or `--path=<path>` - ASF always navigates to its own directory on startup. By specifying this argument, ASF will navigate to given directory after initialization, which allows you to use custom path for various application parts (including `config`, `plugins` and `www` directories, as well as `NLog.config` file), without a need of duplicating binary in the same place. It may come especially useful if you'd like to separate binary from actual config, as it's done in Linux-like packaging - this way you can use one (up-to-date) binary with several different setups. 此路徑既可以是基於 ASF 二進位檔案所在位置的相對路徑，也可以是絕對路徑。 請注意，當使用同一份二進位檔案執行不同執行個體時應停用自動更新功能，因為它們之間不會進行同步。 Also keep in mind that this command points to new "ASF home" - the directory that has the same structure as original ASF, with `config` directory inside.

Due to the nature of this property, it's also possible to set expected path by declaring `ASF_PATH` environment variable, which may be more appropriate for people that would want to avoid sensitive details in the process arguments.

範例：

```shell
dotnet /opt/ASF/ArchiSteamFarm.dll --path /opt/TargetDirectory # 絕對路徑
dotnet /opt/ASF/ArchiSteamFarm.dll --path ../TargetDirectory # 相對路徑也可以工作
```

    ├── /opt
    │     ├── ASF
    │     │     ├── ArchiSteamFarm.dll
    │     │     └── ...
    │     └── TargetDirectory
    │           ├── config
    │           ├── logs（自動產生的）
    │           ├── plugins（可省略）
    │           ├── www（可省略）
    │           ├── log.txt（自動產生的）
    │           └── NLog.config（可省略）
    └── ...
    

* * *

`--process-required` - declaring this switch will disable default ASF behaviour of shutting down when no bots are running. No auto-shutdown behaviour is especially useful in combination with **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** where majority of users would expect their web service to be running regardless of the amount of bots that are enabled. If you're using IPC option or otherwise need ASF process to be running all the time until you close it yourself, this is the right option.

If you do not intend to run IPC, this option will be rather useless for you, as you can just start the process again when needed (as opposed to ASF's web server where you require it listening all the time in order to send commands).

* * *

`--system-required` - declaring this switch will cause ASF to try signalizing the OS that the process requires system to be up and running for its entire lifetime. Currently this switch has effect only on Windows machines where it'll forbid your system from going into sleep mode as long as the process is running. 當您在夜間使用 PC 或者筆記本電腦掛卡時，這一功能是相當實用的，因為 ASF 將會在掛卡時保持電腦處於喚醒狀態，然後，一旦掛卡完成，ASF 就會像平常一樣自動退出，使您的作業系統可以進入睡眠模式，以此節約電力消耗。

Keep in mind that for proper auto-shutdown of ASF you need other settings - especially avoiding `--process-required` and ensuring that all your bots are following `ShutdownOnFarmingFinished`. 當然，自動關閉只是這個參數的用法之一，因為您還可以將此參數配合 `--process-required` 使用，使您的作業系統在 ASF 啟動之後無限執行下去。