# 指令參數

ASF 支持一些能夠影響程式運行時的指令參數。 高級用戶可使用這些參數以定義程式運行方式。 相較使用 `ASF.json `配置文件的預設方式，指令參數可用於核心初始化（如` --path`）、平臺特定設置（如` --system-required`）或敏感數據（如` --cryptkey`）。

* * *

## 使用

使用方法取決於您的作業系統和ASF偏好。

Generic（通用）:

```shell
dotnet ArchiSteamFarm.dll --參數 -- 另一個
```

Windows:

```powershell
.\ArchiSteamFarm.exe --參數--另一個
```

Linux/OS X

```shell
./ArchiSteamFarm --參數--另一個
```

指令參數也可用於通用輔助腳本中，例如`ArchiSteamFarm.cmd`或`ArchiSteamFarm.sh`。 除此之外，如在**[Docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker#command-line-arguments)**一節中所述，使用輔助腳本時，您也可使用`ASF_ARGS`環境屬性。

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

`--path <path>` 或 `--path=<path>` - ASF在啟動時始終會定位至自身所在的資料夾。 通過指定此參數，ASF 會在初始化完成後開始使用指定的資料夾，這讓您可以使用自定義路徑來定義`config`（或者 `www`）資料夾的位置，而無需複製二進位檔案至默认資料夾。 如果您想將二進位檔案和實際配置檔案分開，這可能會非常有用, 正如Linux 打包機制——這樣您就可以在多個配置中使用一個（最新的）二進位檔案。 此路徑既可以是基於 ASF 二進位檔案所在位置的相對路徑，也可以是絕對路徑。 請注意，當使用同一份二進位檔案運行不同實例時應禁用自動更新功能，因為它們之間不會進行同步。 也請注意，該命令指向新的「ASF 主資料夾」——與原始的 ASF 具有相同結構的資料夾，其中包含 `config` 資料夾。

範例:

```shell
dotnet /opt/ASF/ArchiSteamFarm.dll - 路徑
/opt/TargetDirectory # 絕對路徑
dotnet /opt/ASF/ArchiSteamFarm.dll - 路徑
../TargetDirectory # 相對路徑
```

    ├── /opt
    │     ├── ASF
    │     │     ├── ArchiSteamFarm.dll
    │     │     └── ...
    │     └── 目標目录
    │           ├── config（配置）
    │           └── www (可選）
    └── ...
    

* * *

`--process-required` 預設情況下，ASF 將會在無機器人運行的情況下關閉，聲明此開關將禁用這一行為。 當與 **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** 配合使用時停用自動關閉會非常有用，大部分用戶希望自己能夠正常運行網頁服務，無論啟用多少機器人。 如果您在使用 IPC 或者需要 ASF 進程持續運行直至手動關閉它，這就是正確的選項。

如果您不打算運行 IPC，那麼該選項對您來說毫無用處，因為您可以在需要時重新啟動進程（與需要始終監聽以接受命令的 ASF 網頁服務相反）。

* * *

`--system-required` 聲明這個開關將會導致 ASF 嘗試通知作業系統“此進程需要在運行過程中保持系統處於啟動狀態並正常運行”。 目前, 此開關僅對 windows 設備有效, 只要進程正在運行, 它就會阻止您的系統進入睡眠模式。 當您在夜間使用 PC 或者筆記本電腦掛卡時，這一功能是相當實用的，因為 ASF 將會在掛卡時保持電腦處於喚醒狀態，然後，一旦掛卡完成，ASF 就會像平常一樣自動退出，使您的作業系統可以進入睡眠模式，以此節約電力消耗。

請注意，要让 ASF正確地自動關閉，您還需要其他設置——特別是避免 `--process-required`，且確保所有機器人都已啟用 `ShutdownOnFarmingFinished`。 當然，自動關閉只是這個參數的用法之一，因為您還可以將此參數配合 `--process-required` 使用，使您的作業系統在 ASF 啟動之後無限運行下去。