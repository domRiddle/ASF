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

由於此屬性的性質，它還能透過宣告 `ASF_CRYPTKEY` 環境變數來設定 cryptkey，這更適合希望在程序引數中，不包含敏感資訊的使用者。



---

`--ignore-unsupported-environment`──使 ASF 忽略在不支援環境中執行的各種問題。而在原本的情形下，會顯示程式出錯並強制退出。 不支援的環境包含並例如，應該在 .NET (Core) 平台執行的建置版本，卻執行於 .NET Framework 平台上。 雖然此旗標允許 ASF 嘗試在這些情境中執行，但請注意，我們不正式支援此行為。若您強制 ASF 這樣做，**後果自負**。 截至今日，**所有**不受支援環境的情形都可以被修正，例如執行 `generic` 建置版本，而不是 `generic-netf`。 我們強烈建議從根本上修正問題，而非去使用這個引數。



---

`--network-group <group>` 或 `--network-group=<group>`──使 ASF 使用 `<group>` 值的自訂網路群組來初始化其限制器。 這個選項會影響執行**[多個實例](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-zh-TW#多個實例)**的 ASF，使用此選項指定的實例只會與相同網路群組的實例相互分享資訊，而與其他實例獨立。 通常只有在您透過自訂機制（如不同的 IP 位址）來設定 ASF 的路由請求時，才需要使用本選項來設定網路群組，不再依 ASF 自動執行此操作（目前只包含 <0 >WebProxy</code>）。 請注意，在使用自訂網路群組時，這是本機電腦中的唯一識別碼，ASF 將不考慮任何其他細節。例如 `WebProxy` 的值，您可以使用不同的 `WebProxy` 值來執行兩個實例，但它們仍會互相影響。

由於此屬性的性質，它還能透過宣告 `ASF_NETWORK_GROUP` 環境變數來設定其值，這更適合希望在程序引數中，不包含敏感資訊的使用者。



---

`--no-config-migrate`──在預設情形下，ASF 會自動將您的設定檔遷移成最新的語法。 遷移包含：將廢止的屬性轉換成最新的屬性，刪除值為預設的屬性（因為它們沒有意義），以及清理檔案內容（修正縮排等）。 這通常是個好方法，但您可能會遇到特殊情形，以致於希望 ASF 不會去自動覆蓋設定檔。 舉例來說，您可能想要對設定檔 `chmod 400`（僅擁有者可讀取），或使用 `chattr +i` 禁止任何人寫入，來作為安全措施。 一般而言，我們建議保留啟用設定遷移，但如果您有特定的理由停用它，並希望 ASF 不遷移設定，您可以使用此開關來達成。



---

`--no-config-watch`──在預設情形下，ASF 會在您的 `config` 資料夾中設定 `FileSystemWatcher`，以監聽更動檔案的事件 ，因此才能夠動態的適應這些改動。 例如，在刪除設定檔後停止 Bot，更改設定後重新啟動 Bot，或在您將序號加入至 config 資料夾後，載入至 **[背景序號啟動器](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer-zh-TW)** 中。 這個開關允許您停用此行為，使 ASF 完全忽略 `config` 資料夾中的所有變更。若情形需要，您必須手動執行此操作。 一般而言，我們建議保留啟用設定檔監聽，但如果您有特定的理由停用它，並希望 ASF 不監聽事件，您可以使用此開關來達成。



---

`--no-restart` - this switch is mainly used by our **[docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker)** containers and forces `AutoRestart` of `false`. Unless you have a particular need, you should instead configure `AutoRestart` property directly in your config. This switch is here so our docker script won't need to touch your global config in order to adapt it to its own environment. 當然，如果是在腳本中執行 ASF，您也可以使用此開關（否則您最好使用全域設定檔內容）。



---

`--no-steam-parental-generation` - by default ASF will automatically attempt to generate Steam parental PINs, as described in **[`SteamParentalCode`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#steamparentalcode)** configuration property. However, since that might require excessive amount of OS resources, this switch allows you to disable that behaviour, which will result in ASF skipping auto-generation and go straight to asking user for PIN instead, which is what would normally happen only if the auto-generation has failed. Usually we recommend to keep the generation enabled, but if you have a particular reason for disabling it and would instead prefer ASF to not do that, you can use this switch for achieving that purpose.



---

`--path <path>` or `--path=<path>` - ASF 會在啟動後使用程式所在的資料夾。 通過設定此參數，ASF 會在初始化後使用指定的資料夾，讓您能為不同設定（包括 `config`、`plugins` 和 `www` 資料夾，以及 `NLog.config` 檔案）使用不同的資料夾而不用重複複製執行檔至各別資料夾。 如果您想將二進位檔和實際設定檔分開，這可能會非常有用，正如 Linux 封裝機制——這樣您就可以在多個設定檔中使用一個（最新的）二進位檔。 此路徑既可以是基於 ASF 二進位檔案所在位置的相對路徑，也可以是絕對路徑。 Keep in mind that this command points to new "ASF home" - the directory that has the same structure as original ASF, with config directory inside, see below example for explanation.

Due to the nature of this property, it's also possible to set expected path by declaring `ASF_PATH` environment variable, which may be more appropriate for people that would want to avoid sensitive details in the process arguments.

If you're considering using this command-line argument for running multiple instances of ASF, we recommend reading our **[compatibility page](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#multiple-instances)** on this manner.

範例:



```shell
dotnet /opt/ASF/ArchiSteamFarm.dll --path /opt/TargetDirectory # 絕對路徑
dotnet /opt/ASF/ArchiSteamFarm.dll --path ../TargetDirectory # 或相對路徑
ASF_PATH=/opt/TargetDirectory dotnet /opt/ASF/ArchiSteamFarm.dll # 或環境變數
```




```text
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
```




---

`--process-required` - declaring this switch will disable default ASF behaviour of shutting down when no bots are running. No auto-shutdown behaviour is especially useful in combination with **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** where majority of users would expect their web service to be running regardless of the amount of bots that are enabled. If you're using IPC option or otherwise need ASF process to be running all the time until you close it yourself, this is the right option.

If you do not intend to run IPC, this option will be rather useless for you, as you can just start the process again when needed (as opposed to ASF's web server where you require it listening all the time in order to send commands).



---

`--service` - this switch is mainly used by our `systemd` service and forces `Headless` of `true`. Unless you have a particular need, you should instead configure `Headless` property directly in your config. This switch is here so our `systemd` service won't need to touch your global config in order to adapt it to its own environment. Of course, if you have a similar need then you may also make use of this switch (otherwise you're better with global config property).



---

`--system-required` - declaring this switch will cause ASF to try signalizing the OS that the process requires system to be up and running for its entire lifetime. Currently this switch has effect only on Windows machines where it'll forbid your system from going into sleep mode as long as the process is running. This can be proven especially useful when farming on your PC or laptop during night, as ASF will be able to keep your system awake while it's farming, then, once ASF is done, it'll shutdown itself like usual, making your system allowed to enter into sleep mode again, therefore saving power immediately once farming is finished.

Keep in mind that for proper auto-shutdown of ASF you need other settings - especially avoiding `--process-required` and ensuring that all your bots are following `ShutdownOnFarmingFinished`. 當然，自動關閉只是這個參數的用法之一，因為您還可以將此參數配合 `--process-required` 使用，使您的作業系統在 ASF 啟動之後無限運行下去。