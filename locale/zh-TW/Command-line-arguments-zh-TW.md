# 命令列引數

ASF 支援一些能夠影響程式執行階段的命令列引數。 進階使用者可以使用這些引數，來定義程式的執行方式。 相較於使用 `ASF.json` 設定檔的預設方式，命令列引數可用於核心初始化（如 `--path`）、平台特定設定（如 `--system-required`）或敏感資料（如 `--cryptkey`）。

---

## 使用方法

使用方法取決於您的作業系統及 ASF 的版本。

通用：

```shell
dotnet ArchiSteamFarm.dll --引數 --另一個引數
```

Windows：

```powershell
.\ArchiSteamFarm.exe --引數 --另一個引數
```

Linux/macOS：

```shell
./ArchiSteamFarm --引數 --另一個引數
```

命令列引數也可用於通用輔助腳本中，例如 `ArchiSteamFarm.cmd` 或 `ArchiSteamFarm.sh`。 除此之外，如在 **[Docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker-zh-TW#命令列引數)** 章節中所述，使用輔助腳本時，您也可使用 `ASF_ARGS` 環境屬性。

若您的引數中存在空格，別忘了使用引號將其括住。 兩個錯誤的範例：

```shell
./ArchiSteamFarm --path /home/archi/My Downloads/ASF # 這是錯的！
./ArchiSteamFarm --path=/home/archi/My Downloads/ASF # 這是錯的！
```

下面兩個才是正確的：

```shell
./ArchiSteamFarm --path "/home/archi/My Downloads/ASF" # 這樣才正確
./ArchiSteamFarm "--path=/home/archi/My Downloads/ASF" # 這樣才正確
```

## 引數

`--cryptkey<key>` 或 `--cryptkey=<key>`──將以自訂金鑰 `<key>` 啟動 ASF。 此選項將會影響**[安全性](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security-zh-TW)**，並使 ASF 使用您的自訂金鑰 `<key>`，而不是程式中硬碼的預設值。 因為此屬性會影響預設加密鍵（用於加密）及<0>

鹽</1></0>（用於雜湊），請注意使用此金鑰加密／雜湊出的一切，ASF 在每次執行時都需要給入相同的值。</p> 

如果您想從檔案中提供 `<key>`，請改用 `--cryptkey-file`，如下所述。

由於此屬性的性質，它還能透過宣告 `ASF_CRYPTKEY` 環境變數來設定 cryptkey，這更適合希望在程序引數中，不包含敏感資訊的使用者。



---

`--cryptkey-file<path>` 或 `--cryptkey-file=<path>`──將使用從 `<path>` 檔案中讀取的自訂加密金鑰 <0><1></0> 啟動 ASF。 這與上述說明的 `--cryptkey<key>` 的目的相同。但機制不同，因為此屬性是從 `<path>` 讀取 `<key>`。

由於此屬性的性質，它還能透過宣告 `ASF_CRYPTKEY_FILE` 環境變數來設定 cryptkey 檔案，這更適合希望在程序引數中，不包含敏感資訊的使用者。



---

`--ignore-unsupported-environment`──使 ASF 忽略在不支援環境中執行的各種問題。而在原本的情形下，會顯示程式出錯並強制退出。 不支援的環境包含並例如，應該在 .NET (Core) 平台執行的建置版本，卻執行於 .NET Framework 平台上。 雖然此旗標允許 ASF 嘗試在這些情境中執行，但請注意，我們不正式支援此行為。若您強制 ASF 這樣做，**後果自負**。 截至今日，**所有**不受支援環境的情形都可以被修正，例如執行 `generic` 建置版本，而不是 `generic-netf`。 我們強烈建議從根本上修正問題，而非去使用這個引數。



---

`--network-group <group>` 或 `--network-group=<group>`──使 ASF 使用 `<group>` 值的自訂網路群組來初始化其限制器。 This option affects running ASF in **[multiple instances](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Management#multiple-instances)** by signalizing that given instance is dependent only on instances sharing the same network group, and independent of the rest. 通常只有在您透過自訂機制（如不同的 IP 位址）來設定 ASF 的路由請求時，才需要使用本選項來設定網路群組，不再依 ASF 自動執行此操作（目前只包含 <0 >WebProxy</code>）。 請注意，在使用自訂網路群組時，這是本機電腦中的唯一識別碼，ASF 將不考慮任何其他細節。例如 `WebProxy` 的值，您可以使用不同的 `WebProxy` 值來執行兩個實例，但它們仍會互相影響。

由於此屬性的性質，它還能透過宣告 `ASF_NETWORK_GROUP` 環境變數來設定其值，這更適合希望在程序引數中，不包含敏感資訊的使用者。



---

`--no-config-migrate`──在預設情形下，ASF 會自動將您的設定檔遷移成最新的語法。 遷移包含：將廢止的屬性轉換成最新的屬性，刪除值為預設的屬性（因為它們沒有意義），以及清理檔案內容（修正縮排等）。 這通常是個好方法，但您可能會遇到特殊情形，以致於希望 ASF 不會去自動覆蓋設定檔。 舉例來說，您可能想要對設定檔 `chmod 400`（僅擁有者可讀取），或使用 `chattr +i` 禁止任何人寫入，來作為安全措施。 一般而言，我們建議保留啟用設定遷移，但如果您有特定的理由停用它，並希望 ASF 不遷移設定，您可以使用此開關來達成。



---

`--no-config-watch`──在預設情形下，ASF 會在您的 `config` 資料夾中設定 `FileSystemWatcher`，以監聽更動檔案的事件 ，因此才能夠動態地適應這些改動。 例如，在刪除設定檔後停止 Bot，更改設定後重新啟動 Bot，或在您將序號加入至 config 資料夾後，載入至 **[背景序號啟動器](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer-zh-TW)** 中。 這個開關允許您停用此行為，使 ASF 完全忽略 `config` 資料夾中的所有變更。若情形需要，您必須手動執行此操作。 一般而言，我們建議保留啟用設定檔監聽，但如果您有特定的理由停用它，並希望 ASF 不監聽事件，您可以使用此開關來達成。



---

`--no-restart`──這個開關主要由 **[Docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker-zh-TW)** 容器所使用，它會強制將 `AutoRestart` 設定成 `false`。 除非您有特殊需求，否則您應該直接在設定中設定 `AutoRestart` 屬性。 這個開關使 Docker 腳本不需要修改您的全域設定，來適應它的環境。 當然，如果是在腳本中執行 ASF，您也可以使用此開關（否則您最好使用全域設定屬性）。



---

`--no-steam-parental-generation`──在預設情形下，ASF 會自動嘗試生成 Steam 家長監護 PIN 碼，如 **[`SteamParentalCode`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-TW#SteamParentalCode)** 組態屬性中所述。 但由於這可能需要過多的作業系統資源，因此這個開關允許您停用此行為，這將使 ASF 跳過自動生成，並直接向使用者詢問 PIN 碼，與一般情形下的自動生成失敗時相同。 一般而言，我們建議保留啟用生成，但如果您有特定的理由停用它，並希望 ASF 不生成代碼，您可以使用此開關來達成。



---

`--path <path>` 或 `--path=<path>`──在預設情形下，ASF 總是會在啟動後，使用程式本身所在的資料夾。 透過指定此引數，ASF 會在初始化後使用指定的資料夾，讓您能為不同的設定（包含 `config`、`plugins` 及 `www` 資料夾，與 `NLog.config` 檔案）使用不同的應用程式檔案，而不用複製 ASF 至不同檔案的資料夾中。 若您想將二進制檔案和實際使用的設定檔分開，這可能會非常有用。就像是 Linux 的封裝機制一樣：這樣您就可以在多個設定檔間，共用一個（最新的）二進制檔案。 這個路徑既可以是基於 ASF 二進制檔案所在位置的相對路徑，亦可以是絕對路徑。 請注意，這個指令會指向新的「ASF 主資料夾」：與原本的 ASF 具有相同的資料夾結構，其中包含 config 資料夾，請參閱下面的說明範例。

由於此屬性的性質，它還能透過宣告 `ASF_PATH` 環境變數來設定路徑，這更適合希望在程序引數中，不包含敏感資訊的使用者。

If you're considering using this command-line argument for running multiple instances of ASF, we recommend reading our **[management page](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Management#multiple-instances)** on this manner.

範例：



```shell
dotnet /opt/ASF/ArchiSteamFarm.dll --path /opt/TargetDirectory # 使用絕對路徑
dotnet /opt/ASF/ArchiSteamFarm.dll --path ../TargetDirectory # 或使用相對路徑
ASF_PATH=/opt/TargetDirectory dotnet /opt/ASF/ArchiSteamFarm.dll # 或使用環境變數
```




```text
├── /opt
│     ├── ASF
│     │     ├── ArchiSteamFarm.dll
│     │     └── ...
│     └── TargetDirectory
│           ├── config
│           ├── logs（自動產生）
│           ├── plugins（選擇性）
│           ├── www（選擇性）
│           ├── log.txt（自動產生）
│           └── NLog.config（選擇性）
└── ...
```




---

`--process-required`──在預設情形下，ASF 會在沒有 Bot 執行時關閉，使用這個開關會停用此行為。 這項功能與 **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-zh-TW)** 一起使用時特別有用，大多數使用者都會希望他們的 Web 服務，不管在多少個 Bot 啟用時，都能夠正常執行。 若您正在使用 IPC，或需要 ASF 程序一直執行到您手動關閉它，那麼您就該使用此選項。

若您沒有打算執行 IPC，則這個選項對您來說沒有用處，因為您可以在需要時重新啟動程序（與始終監聽來傳送指令的 ASF Web 伺服器相反）。



---

`--service`──這個開關主要由 `systemd` 服務所使用，它會強制將 `Headless` 設定成 `true`。 除非您有特殊需求，否則您應該直接在設定中設定 `Headless` 屬性。 這個開關使 `systemd` 服務不需要修改您的全域設定，來適應它的環境。 當然，如果您有類似需求，您也可以使用此開關（否則您最好使用全域設定屬性）。



---

`--system-required`──使用這個開關，將使 ASF 嘗試通知作業系統，表明本程序需要在系統執行期間，保持啟動與正常執行狀態。 目前這個開關只對 Windows 系統有效，只要程序正在執行，它就會阻止您的系統進入睡眠模式。 當您在夜間使用桌機或筆電掛卡時，這個功能是相當實用的，因為 ASF 將會在掛卡期間讓您的系統保持喚醒狀態。一旦 ASF 完成掛卡，它會像平常一樣自動關閉，使您的系統可以再次進入睡眠模式，以此節省電力消耗。

請注意，要正確地使 ASF 自動關閉，您還需要其他設定：特別是避免使用 `--process-required`，並確保您的所有 Bot 都設定了 `ShutdownOnFarmingFinished`。 當然，自動關閉只是這個功能的其中一種用法，因為您還可以配合 `--process-required` 使用，讓您的系統在 ASF 啟動之後永遠地保持喚醒狀態。