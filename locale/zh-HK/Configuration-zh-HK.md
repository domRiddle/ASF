# 配置

此頁面專門用於 ASF 配置。 提供關於 `config` 目錄的完整文件，允許您依照您的需求調整 ASF。

- **[介紹](#introduction)**
- **[網頁設定檔產生器](#web-based-configgenerator)**
- **[手動配置](#manual-configuration)**
- **[全域配置](#global-config)**
- **[機器人配置](#bot-config)**
- **[檔案結構](#file-structure)**
- **[JSON 映射](#json-mapping)**
- **[相容性映射](#compatibility-mapping)**
- **[配置相容性](#configs-compatibility)**
- **[自動重載](#auto-reload)**

* * *

## 介紹

ASF配置分為兩個主要部分-全域 (流程) 配置和每個機器人的配置。 每個機器人都有自己的機器人設定檔, 名為 `BotName.json` (其中 `BotName` 是機器人的名稱), 而全域 ASF (進程) 配置是一個名為 `ASF.json` 的檔。

每個機器人都是一個在 ASF 進程中運行的獨立的 Steam 帳戶。 為了能夠正常工作，ASF 需要定義**至少一個**機器人實例。 機器人實例沒有過程強制限制, 因此您可以根據需要使用任意數量的機器人 (Steam帳戶)。

ASF使用 **[JSON](https://en.wikipedia.org/wiki/JSON)** 格式來存儲其設定檔。 它是人性化的, 可讀的, 廣泛適用的格式, 您可以在其中對程式進行配置。 不過不用擔心，您不需要為了配置 ASF 去專門瞭解 JSON。 我提到它只是考慮到您可能會想要使用某種 Bash 腳本批量創建大量 ASF 配置檔案。

您可以通過創建合適的 JSON 配置檔案來手動完成配置，也可以通過我們的**[​網頁設定檔產生器​](https://justarchinet.github.io/ASF-WebConfigGenerator)**來進行配置，那將會更簡單方便。 除非您是高級用戶，否則我建議您使用基于Web的配置產生器，我們將會在下方對其詳細說明。

**[返回頂部](#configuration)**

* * *

## 基於Web的配置產生器

**[基於Web的配置產生器](https://justarchinet.github.io/ASF-WebConfigGenerator)**的目的是為您提供一個用於生成 ASF 配置檔的友好前端。 基於Web的配置產生器是100% 基於用戶端的, 這意味著您輸入的詳細資訊都不會被上傳，而只在本地處理。 這保證了安全性和可靠性，因為如果您願意下載所有相關檔案，併在您喜愛的瀏覽器中打開其中的 `index.html`，它甚至可以​**[離線](https://github.com/JustArchiNET/ASF-WebConfigGenerator/tree/master/docs)**​工作。

基於Web的配置產生器已經在 Chrome 和 Firefox 上經過驗證可以正常運行，但它也應該可以在所有流行的支援 JavaScript 的瀏覽器中正常工作。

它的用法非常簡單——切換到正確的標簽來選擇要生成 `ASF` 設定檔還是 `Bot（機器人）`設定檔，確保所選設定檔的版本與您的 ASF 版本相匹配，然後輸入所有詳細信息並點擊“下載”按鈕。 將此檔移動到 ASF `config` 目錄, 如果需要, 將覆蓋現有檔。 如果要繼續配置，則重覆以上操作，並參考本頁的其他部分以瞭解所有可用的選項。

**[返回頂部](#configuration)**

* * *

## 手動配置

我強烈建議使用基於 Web 的配置產生器, 但如果由於某種原因, 您不想使用, 那麼您也可以手動創建正確的配置。 檢查下面的 JSON 示例, 以便在結構中有一個良好的開端, 您可以將內容複寫到檔中, 並將其用作配置的基礎。由於您沒有使用我們的前端, 請確保您的配置為 **[valid](https://jsonlint.com)**, 因為如果無法對其進行分析, ASF將拒絕載入它。 有關所有可用欄位的正確 JSON 結構, 請參閱下面的 **JSON 映射 </0 > 部分和文檔。</p> 

**[返回頂部](#configuration)**

* * *

## 全域配置

全域配置位於 `ASF.json` 檔中, 具有以下結構:

```json
{
    "AutoRestart": true,
    "Blacklist": [],
    "CommandPrefix": "!",
    "ConfirmationsLimiterDelay": 10,
    "ConnectionTimeout": 90,
    "CurrentCulture": null,
    "Debug": false,
    "FarmingDelay": 15,
    "GiftsLimiterDelay": 1,
    "Headless": false,
    "IdleFarmingPeriod": 8,
    "InventoryLimiterDelay": 3,
    "IPC": false,
    "IPCPassword": null,
    "LoginLimiterDelay": 10,
    "MaxFarmingTime": 10,
    "MaxTradeHoldDuration": 15,
    "OptimizationMode": 0,
    "Statistics": true,
    "SteamMessagePrefix": "/me ",
    "SteamOwnerID": 0,
    "SteamProtocols": 7,
    "UpdateChannel": 1,
    "UpdatePeriod": 24,
    "WebLimiterDelay": 300,
    "WebProxy": null,
    "WebProxyPassword": null,
    "WebProxyUsername": null
}
```

**提示**：​除非您想要改變其中的選項，否則應保持所有選項為預設值，關閉` ASF.json` 然後繼續進行機器人配置。

* * *

以下是對所有選項的解釋：

### `AutoRestart`

預設值為 `true` 的 `bool` 類型。 此屬性定義是否允許 ASF 在需要時自行重啟。 有一些事件需要 ASF 自行重啟, 例如 ASF 更新 (通過 `UpdatePeriod` 或 `update` 命令完成), 以及 `ASF.json` 配置的編輯、`restart` 命令等。 通常情況下, 重啟包括兩個部分--創建新流程和完成當前流程。 但是, 大多數使用者應該可以使用它並保留屬性預設值為 `true` -如果您正在通過自己的腳本運行 ASF, 或者使用 `dotnet` 運行 ASF, 則可能希望完全控制進程的啟動, 並避免以下情況: 使新的 (重啟) ASF進程在後臺靜默運行, 而不是在腳本的前景中運行, 這些進程與舊的ASF進程一起退出。 考慮到新的流程將不再是您原有流程的直接分支, 這一點尤其重要, 這將使您無法使用標準的主控台輸入。

在此屬性僅為您服務的情況下，您可以將其設置為 `false`。 但是, 請記住, 在這種情況下 **you** 負責重啟進程。 這在某種程度上很重要, 因為ASF將僅退出, 而非生成新的進程 (例如更新後), 因此, 如果您沒有添加邏輯, 它將停止工作, 直到您再次啟動它。 ASF總是使用指示成功 (零) 或非成功 (非零) 的錯誤代碼退出, 這樣您就可以在腳本中添加合適的邏輯, 從而避免在出現故障時自動重啟ASF，或者至少製作 `log.txt` 的本機複本以獲得進一步的應用分析。 還要記住, 無論此屬性設置的方式如何, `restart` 命令都將始終重啟 ASF, 因為此屬性定義預設行為, 而 `restart` 命令始終會重新啟動進程。 除非您有理由禁用此功能, 否則應保持啟用它。

* * *

### `Blacklist`

預設值為空的 `ImmutableHashSet<uint>` 類型。 顧名思義, 此全域配置屬性定義了ASF自動掛卡過程將完全忽略的 appIDs (遊戲)。 不幸的是, Steam喜歡將夏季/冬季銷售徽章標記為 "有卡掉落", 這欺騙了ASF過程, 使其相信這是一個可以掛卡的遊戲。 如果沒有任何黑名單, ASF最終會 "掛" 一個并不存在的遊戲, 並無限等待𣎴存在的卡牌掉落。 ASF黑名單的目的是將這些徽章標記為不可掛卡，因此, 我們在掛卡時可以忽視這些徽章, 而避免落入陷阱。

預設情況下, ASF包括兩個黑名單-硬編碼到ASF代碼中、無法編輯的`GlobalBlacklist`, 以及可自訂的`Blacklist`, 在這裡定義。 `GlobalBlacklist` 與ASF版本一起更新, 通常包括發佈時的所有 "壞" appIDs, 因此, 如果您使用的是最新的ASF，則無需維護此處的自訂`Blacklist`。 此屬性的主要目的是允許您將在ASF發佈時未知的新appIDs設置為黑名單, 不予掛卡。 我們會盡可能快地更新硬編碼的 `GlobalBlacklist`, 因此, 如果您使用的是最新的 ASF 版本, 則不需要更新自己的 `Blacklist`。 但如果沒有 `Blacklist`， 您將被要求更新ASF，以便在Valve發佈新徽章的情況下 "繼續運行"。我不想強迫您使用最新的 ASF 代碼, 此屬性是為了讓您手動 "修復" ASF。如果您出於某種原因不想, 或不能更新的ASF版本，您可以更新硬編碼 `GlobalBlacklist`以保持你的舊 ASF 運行。 除非您有**強烈**的修改意願，否則應保持它為预設值。

如果您正在尋找基於機器人的黑名單, 請查看 `ib`、`ibadd` 和 `ibrm` **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**。

* * *

### `CommandPrefix`

預設值為 `！` 的 `string` 類型。 此屬性指定用於 asf **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** 的**case-sensitive** 首碼。 換句話說, 這是您需要在每個 ASF 命令之前加上的內容, 以使ASF偵聽命令。 可以將此值設置為 `null` 或空, 以讓 ASF 不使用命令首碼, 在這種情況下, 您可以使用其普通識別碼輸入命令。 但是, 這樣做可能會降低ASF的性能, 因為ASF 經過優化, 如果不從 `CommandPrefix` 開始, 就不會進一步分析消息-如果您有意決定不使用它, ASF將被迫讀取所有消息並對其做出回應, 即使它們不是ASF 命令。 因此, 如果您不想使用預設值 `! `, 建議您繼續使用某些 `CommandPrefix`, 如 `/`。 為了保持一致, `CommandPrefix` 會影響整個 ASF 過程。 除非您有充分的修改理由，否則應保持它為預設值。

* * *

### `ConfirmationsLimiterDelay`

這是一個預設值為`10` 的 `byte flags` 類型屬性。 ASF will ensure that there will be at least `ConfirmationsLimiterDelay` seconds in between of two consecutive 2FA confirmations fetching requests to avoid triggering rate-limit - those are being used by **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** during e.g. `2faok` command, as well as on as-needed basis when performing various trading-related operations. 預設值基於我們的測試設定, 如果您不想遇到問題, 則不應降低預設值。 除非您有**強烈**的修改意願，否則應保持它為预設值。

* * *

### `連接超時`

這是一個預設值為`7` 的 `byte flags` 類型屬性。 此屬性定義 ASF執行的各種網路操作的超時 (以秒為單位)。 In particular, `ConnectionTimeout` defines timeout in seconds for HTTP and IPC requests, `ConnectionTimeout / 10` defines maximum number of failed heartbeats, while `ConnectionTimeout / 30` defines number of minutes we allow for initial Steam network connection request. ` 90 ` 的預設值對大多數人來說應該是可以的, 但是, 如果您的網路連接或PC速度相當慢, 您可能希望將此數位增加到更高的數量 (如 ` 120 `)。 請記住, 即使是更大的值亦無法神奇地修復緩慢甚至無法訪問的 Steam 伺服器, 因此我們不應該無限等待不會發生的事情, 只需稍後再試。 如果將此值設置得過高, 將導致捕獲網路問題的過度延遲, 並降低整體性能。 將此值設置得過低也會降低整體穩定性和性能, 因為ASF將中止仍在分析的有效請求。 因此, 將此值設置為低於預設值通常沒有優勢, 因為 Steam伺服器往往有時會非常慢, 並且可能需要更多的時間來分析ASF請求。 預設值是在相信我們的網路連接是穩定的和懷疑蒸汽網路在給定的超時下處理我們的請求之間的平衡。 如果您想更早地發現問題並使ASF的重新連接/回應速度更快, 預設值應該設為 (或略低於, 如 ` 60 `, 從而減少ASF 的等待時間)。 如果您注意到 ASF 遇到了網路問題, 例如失敗的請求、失去心跳或與 Steam 的連接中斷, 那麼如果您確定此問題是由您的網路, 而**非**Steam本身造成，則增加此值可能是有意義的, 因為增加超時使ASF 更 "有耐心", 而不是決定立即重新連接。

一個可能需要增加此屬性的示例情況是讓ASF處理一個非常巨大的交易提案, 可能需要大于2分鐘的時間才能被Steam完全接受並處理。 通過增加超時的預設值, 在決定放棄交易之前，ASF 將更有耐心, 等待更長的時間。

另一種情況可能是由於機器或互聯網連接非常慢, 需要更多的時間來處理正在傳輸的資料。 這是非常罕見的情況, 因為CPU\ 網路頻寬幾乎從來都不是瓶頸, 但這仍然是一個值得提及的可能性。

簡而言之, 預設值在大多數情況下應該是合適的, 但若需要，您可能要增加預設值。 不過, 遠遠高於預設值也沒有多大意義, 因為更大的超時亦無修復無法訪問的Steam 伺服器的魔法。 除非您有充分的修改理由，否則應保持它為默認值。

* * *

### `CurrentCulture`

預設值為 `null` 的 `string` 類型。 預設情況下, ASF將嘗試使用您的作業系統語言, 並且更願意使用該語言中的翻譯字串 (如果可用)。 這應感謝我們的社區成員，他們致力於推動ASF在各種主流語言中的**[本土化](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Localization)**進程。 如果由於某種原因您不想使用作業系統本機語言, 則可以使用此配置屬性選擇要使用的任何有效語言。 有關所有可用語系的清單, 請訪問 **[MSDN](https://msdn.microsoft.com/en-us/library/cc233982.aspx)** 並查找 < 1>語言標籤 </1 >。 It's nice to note that ASF accepts both specific cultures, such as `en-GB`, but also neutral ones, such as `en`. 指定目前範圍性還可能會影響其他特定于區域性的行為, 如貨幣/日期格式等。 請注意, 如果您選擇了使用特定于語言的非本機區域性, 則可能需要額外的字體/語言包來正確顯示特定于語言的字元。 通常, 如果您更喜歡讓ASF使用英語而不是您的母語, 則需使用此配置屬性。

* * *

### `Debug`

預設值為 `false` 的 `bool` 類型。 此屬性定義進程是否應在偵錯模式下運行。 在偵錯模式下, ASF會在 `config` 旁邊創建一個特殊的 `debug` 目錄, 用於跟蹤ASF和 Steam 伺服器之間的整體通信。 調試資訊有助於發現與網路和一般ASF工作流相關的棘手問題。 除此之外, 一些程式常式將更加詳細, 例如 `WebBrowser` 說明某些請求失敗的確切原因-這些條目被寫入正常的ASF日誌中。 **除非開發人員提出要求, 否則您不應在偵錯模式下運行 asf。** Running ASF in debug mode **decreases performance**, **affects stability negatively** and is **far more verbose in various places**, so it should be used **only** intentionally, in short-run, for debugging particular issue, reproducing the problem or getting more info about a failing request, and alike, but **not** for normal program execution. 您將看到**一堆**新的錯誤、問題和異常--如果您決定自己分析所有這些內容, 請確保您對ASF及Steam有充分的瞭解, 因為並非所有內容都是相關的。

**WARNING:** 啟用此模式將記錄 **潜在的敏感</0 > 資訊, 如登錄名和用於登錄到Steam的密碼 (由於網路日誌記錄)。 這些資料既寫入 `debug` 目錄, 也寫入標準 `log.txt` (此資訊將被詳細地記錄)。 您不應該在任何公共位置發佈 ASF生成的調試內容, ASF 開發人員始終提醒您應將其發送到他的電子郵件或其他安全位置。 We're not storing, neither making use of those sensitive details, they're written as part of debug routines since their presence might be relevant to the issue that is affecting you. We'd prefer if you didn't alter ASF logging in any way, but if you're worried, you're free to redact those sensitive details appropriately.</p> 

> Redacting involves replacing sensitive details, for example with stars. 你應該避免完全刪除敏感的數據, 因為它們的存在可能與問題相關，應該保留。

* * *

### `FarmingDelay`

這是一個預設值為`15` 的 `byte flags` 類型屬性。 In order for ASF to work, it will check currently farmed game every `FarmingDelay` minutes, if it perhaps dropped all cards already. Setting this property too low can result in excessive amount of steam requests being sent, while setting it too high can result in ASF still "farming" given title for up to `FarmingDelay` minutes after it's fully farmed. Default value should be excellent for most users, but if you have many bots running, you might consider increasing it to something like `30` minutes in order to limit steam requests being sent. It's nice to note that ASF uses event-based mechanism and checks game badge page on each Steam item dropped, so in general **we don't even need to check it in fixed time intervals**, but as we don't fully trust Steam network - we check game badge page anyway, if we didn't check it through card being dropped event in last `FarmingDelay` minutes (in case Steam network didn't inform us about item dropped or stuff like that). Assuming that Steam network is working properly, decreasing this value **will not improve farming efficiency in any way**, while **increasing network overhead significantly** - it's recommended only to increase it (if needed) from default of `15` minutes. 除非您有**強烈**的修改意願，否則應保持它為默認值。

* * *

### `GiftsLimiterDelay`

這是一個預設值為`1` 的 `byte flags` 類型屬性。 ASF will ensure that there will be at least `GiftsLimiterDelay` seconds in between of two consecutive gift/key/license handling (redeeming) requests to avoid triggering rate-limit. In addition to that it'll also be used as global limiter for game list requests, such as the one issued by `owns` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. 除非您有**強烈**的修改意願，否則應保持它為默認值。

* * *

### `無頭`

預設值為 `false` 的 `bool` 類型。 此屬性定義進程是否應在偵錯模式下運行。 When in headless mode, ASF assumes that it's running on a server or in other non-interactive environment, therefore it will not attempt to read crucial account credentials such as 2FA code, SteamGuard code, password or any other variable required for ASF to operate. This is equal to making ASF console read-only. `Headless` mode is useful mainly for users running ASF on their servers, as instead of asking e.g. for 2FA code, ASF will silently abort the operation by stopping an account. Unless you're running ASF on a server, and you previously confirmed that ASF is able to operate in non-headless mode, you should keep this property disabled. Any user interaction will be denied when in headless mode, and your accounts will not run if they require **any** console input during starting. This is useful for servers, as ASF can abort trying to log onto the account when asked for credentials, instead of waiting (infinitely) for user to provide those. Enabling this mode will also allow you to use `input` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** which acts as a replacement for standard console input. If you're not sure how to set this property, leave it with default value of `false`.

If you're running ASF on the server, you might want to use this option together with `--process-required` **[command-line argument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments)**.

* * *

### `IdleFarmingPeriod`

`byte` type with default value of `8`. When ASF has nothing to farm, it will periodically check every `IdleFarmingPeriod` hours if perhaps account got some new games to farm. This feature is not needed when talking about new games we're getting, as ASF is smart enough to automatically check badge pages in this case. `IdleFarmingPeriod` is mainly for situations such as old games we already have having trading cards added. In this case there is no event, so ASF has to periodically check badge pages if we want to have this covered. Value of `0` disables this feature. Also check: `ShutdownOnFarmingFinished`.

* * *

### `InventoryLimiterDelay`

`byte` type with default value of `3`. ASF will ensure that there will be at least `InventoryLimiterDelay` seconds in between of two consecutive inventory requests to avoid triggering rate-limit - those are being used for fetching Steam inventories, especially during your own commands such as `transfer`, as well as in features like `MatchActively`. Default value of `3` was set based on fetching inventories of over 100 consecutive bot instances, and should satisfy most (if not all) of the users. You may however want to decrease it, or even change to `0` if you have very low amount of bots, so ASF will ignore the delay and loot steam inventories much faster. Be warned though, as setting it too low **will** result in Steam temporarily banning your IP, and that will prevent you from fetching your inventory at all. You also might need to increase this value if you're running a lot of bots with a lot of inventory requests, although in this case you should probably try to limit number of those requests instead. 除非您有**強烈**的修改意願，否則應保持它為预設值。

* * *

### `IPC`

預設值為 `false` 的 `bool` 類型。 This property defines if ASF's **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** server should start together with the process. IPC allows for inter-process communication by booting a local HTTP server. If you're not going to make use of ASF's IPC server, then there is no reason for you to enable this option.

* * *

### `IPCPassword`

預設值為 `null` 的 `string` 類型。 This property defines mandatory password for every API call done via IPC and serves as an extra security measure. When set to non-empty value, all IPC requests will require extra `password` property set to the password specified here. Default value of `null` will skip a need of the password, making ASF respect all queries. In addition to that, enabling this option also enables built-in IPC anti-bruteforce mechanism which will temporarily ban given `IPAddress` after sending too many unauthorized requests in a very short time. 除非您有充分的修改理由，否則應保持它為默認值。

* * *

### `LoginLimiterDelay`

這是一個預設值為`10` 的 `byte flags` 類型屬性。 ASF will ensure that there will be at least `LoginLimiterDelay` seconds in between of two consecutive connection attempts to avoid triggering rate-limit. Default value of `10` was set based on connecting over 100 bot instances, and should satisfy most (if not all) of the users. You may however want to increase/decrease it, or even change to `0` if you have very low amount of bots, so ASF will ignore the delay and connect to Steam much faster. Be warned though, as setting it too low while having too many bots **will** result in Steam temporarily banning your IP, and that will prevent you from logging in **at all**, with `InvalidPassword/RateLimitExceeded` error - and that also includes your normal Steam client, not only ASF. Likewise, if you're running excessive number of bots, especially together with other Steam clients/tools using the same IP address, most likely you'll need to increase this value in order to spread logins across longer period of time.

As a side note, this value is also used as load-balancing buffer in all ASF-scheduled actions, such as trades in `SendTradePeriod`. 除非您有**強烈**的修改意願，否則應保持它為默認值。

* * *

### `MaxFarmingTime`

這是一個預設值為`10` 的 `byte flags` 類型屬性。 As you should know, Steam is not always working properly, sometimes weird situations can happen such as steam not being recording our playtime, despite of in fact playing a game. ASF will allow farming a single game in solo mode for maximum of `MaxFarmingTime` hours, and consider it fully farmed after that period. This is required to not freeze farming process in case of weird situations happening, but also if for some reason Steam released a new badge that would stop ASF from progressing further (see: `Blacklist`). Default value of `10` hours should be enough for dropping all steam cards from one game. Setting this property too low can result in valid games being skipped (and yes, there are valid games taking even up to 9 hours to farm), while setting it too high can result in farming process being frozen. Please note that this property affects only a single game in a single farming session (so after going through entire queue ASF will return to that title), also it's not based on total playtime but internal ASF farming time, so ASF will also return to that title after a restart. 除非您有**強烈**的修改意願，否則應保持它為默認值。

* * *

### `MaxTradeHoldDuration`

這是一個預設值為`15` 的 `byte flags` 類型屬性。 This property defines maximum duration of trade hold in days that we're willing to accept - ASF will reject trades that are being held for more than `MaxTradeHoldDuration` days, as defined in **[trading](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading)** section. This option makes sense only for bots with `TradingPreferences` of `SteamTradeMatcher`, as it doesn't affect `Master`/`SteamOwnerID` trades, neither donations. 交易暫掛是很煩人的，沒人會想被它困擾。 ASF is supposed to work on liberal rules and help everyone, regardless if on trade hold or not - that's why this option is set to `15` by default. However, if you'd instead prefer to reject all trades affected by trade holds, you can specify `0` here. Please consider the fact that cards with short lifespan are not affected by this option and automatically rejected for people with trade holds, as described in **[trading](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading)** section, so there is no need to globally reject everybody only because of that. 除非您有充分的修改理由，否則應保持它為預設值。

* * *

### `OptimizationMode`

這是一個預設值為`0` 的 `byte flags` 類型。 This property defines optimization mode which ASF will prefer during runtime. Currently ASF supports two modes - `0` which is called `MaxPerformance`, and `1` which is called `MinMemoryUsage`. 預設情況下, ASF希望盡可能多地並行 (同時) 運行, 這通過跨所有 CPU 內核、多個 CPU 執行緒、多個通訊端和多個執行緒池任務的負載平衡工作來提高性能。 例如, ASF在檢查可掛卡遊戲時將請求您的第一個徽章頁面, 然後在請求到達後, ASF 將從中讀取您實際擁有多少徽章頁面, 然後同時向每個徽章頁發送請求。 This is what you should want **almost always**, as the overhead in most cases is minimal and benefits from asynchronous ASF code can be seen even on the oldest hardware with a single CPU core and heavily limited power. However, with many tasks being processed in parallel, ASF runtime is responsible for their maintenance, e.g. keeping sockets open, threads alive and tasks being processed, which can result in increased memory usage from time to time, and if you're extremely constrained by available memory, you might want to switch this property to `1` (`MinMemoryUsage`) in order to force ASF into using as little tasks as possible, and typically running possible-to-parallel asynchronous code in a synchronous manner. You should consider switching this property only if you previously read **[low-memory setup](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup)** and you intentionally want to sacrifice gigantic performance boost, for a very small memory overhead decrease. Usually this option is **much worse** than what you can achieve with other possible ways, such as by limiting your ASF usage or tuning runtime's garbage collector, as explained in **[low-memory setup](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup)**. Therefore, you should use `MinMemoryUsage` as a **last resort**, right before runtime recompilation, if you couldn't achieve satisfying results with other (much better) options. 除非您有**強烈**的修改意願，否則應保持它為预設值。

* * *

### `統計`

預設值為 `true` 的 `bool` 類型。 此屬性定義ASF是否啟用統計資訊。 **[statistics](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Statistics)** 部分詳細說明了此選項的確切作用。 除非您有充分的修改理由，否則應保持它為預設值。

* * *

### `SteamMessagePrefix`

預設值為 `"/me "` 的 `string` 類型。 此屬性定義了一個首碼, 該首碼將作為ASF發送的所有 Steam 消息的首碼。 預設情況下, ASF使用`"/me "` 首碼, 以便通過在 Steam 聊天中以不同的顏色顯示機器人消息來更輕鬆地區分機器人消息。 另一個值得提及的地方是 `"/pre "` 首碼, 它實現了類似的結果, 但使用了不同的格式。 您還可以將此屬性設置為空字串或 `null`, 以便完全禁用首碼, 並以傳統方式輸出所有ASF消息。 好消息是, 此屬性僅影響 Steam 消息-通過其他通道 (如 IPC) 返回的回應不受影響。 除非您要自訂標準 ASF 行為, 否則最好將其保留為預設值。

* * *

### `SteamOwnerID`

預設值 為`0` 的 `ulong` 類型。 This property defines Steam ID in 64-bit form of ASF process owner, and is very similar to `Master` permission of given bot instance (but global instead). You almost always want to set this property to ID of your own main Steam account. `Master` permission includes full control over his bot instance, but global commands such as `exit`, `restart` or `update` are reserved for `SteamOwnerID` only. This is convenient, as you might want to run bots for your friends, while not allowing them to control ASF process, such as exiting it via `exit` command. Default value of `0` specifies that there is no owner of ASF process, which means that nobody will be able to issue global ASF commands. Keep in mind that **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** commands work with `SteamOwnerID`, so if you want to use them, then you must provide a valid value here.

* * *

### `SteamProtocols`

這是一個預設值為`7` 的 `byte flags` 類型屬性。 此屬性定義了 ASF 在連接 Steam 伺服器時使用的網路協議，其定義如下：

| 值 | 名稱         | 描述                                                                        |
| - | ---------- | ------------------------------------------------------------------------- |
| 0 | None       | 無協議                                                                       |
| 1 | TCP        | **[傳輸控制協議](https://en.wikipedia.org/wiki/Transmission_Control_Protocol)** |
| 2 | UDP        | **[用戶數據報協議](https://en.wikipedia.org/wiki/User_Datagram_Protocol)**       |
| 4 | WebSockets | **[WebSockets](https://en.wikipedia.org/wiki/WebSocket)**                 |

Please notice that this property is `flags` field, therefore it's possible to choose any combination of available values. Check out **[flags mapping](#json-mapping)** if you'd like to learn more. Not enabling any of flags results in `None` option, and that option is invalid by itself.

By default ASF should use all available Steam protocols as a measure for fighting with downtimes and other similar Steam issues. Typically you want to change this property if you want to limit ASF into using only one or two specific protocols instead of all available ones. Such measure could be needed if you're e.g. enabling only TCP traffic on your firewall and you do not want ASF to try connecting via UDP. However, unless you're debugging particular problem or issue, you almost always want to ensure that ASF is free to use any protocol that is currently supported and not just one or two. 除非您有**強烈**的修改意願，否則應保持它為预設值。

* * *

### `UpdateChannel`

這是一個預設值為`7` 的 `byte flags` 類型屬性。 This property defines update channel which is being used, either for auto-updates (if `UpdatePeriod` is greater than `0`), or update notifications (otherwise). Currently ASF supports three update channels - `0` which is called `None`, `1`, which is called `Stable`, and `2`, which is called `Experimental`. `Stable` channel is the default release channel, which should be used by majority of users. `Experimental` channel in addition to `Stable` releases, also includes **pre-releases** dedicated for advanced users and other developers in order to test new features, confirm bugfixes or give feedback about planned enhancements. **Experimental versions often contain unpatched bugs, work-in-progress features or rewritten implementations**. 如果您不認為自己是高級使用者, 請保留預設 ` 1 ` (穩定) 更新通道。 `Experimental` 通道專門針對知道如何報告錯誤、處理問題和提供回饋的使用者--不會提供任何技術支援。 如果您想瞭解更多資訊, 請查看ASF**[發布周期](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle)**。 You can also set `UpdateChannel` to `0` (`None`), if you want to completely remove all version checks. 將 `UpdateChannel` 設置為 ` 0 ` 將完全禁用與更新相關的整個功能, 包括 `update` 命令。 Using `None` channel is **strongly discouraged** due to exposing yourself to all sort of problems (mentioned in `UpdatePeriod` description below).

**Unless you know what you're doing**, we **strongly** recommend to keep it at default.

* * *

### `UpdatePeriod`

這是一個預設值為`24` 的 `byte flags` 屬性。 此屬性定義 ASF 檢查自動更新的頻率。 更新不僅對於接收新功能和關鍵安全修補程式至關重要, 而且對於接收錯誤修復、性能增強、穩定性改進等也至關重要。 When a value greater than `0` is set, ASF will automatically download, replace, and restart itself (if `AutoRestart` permits) when new update is available. 為了實現這一目標, ASF將每 `UpdatePeriod` 小時檢查一次我們的GitHub 存儲庫上是否有新的更新。 A value of `0` disables auto-updates, but still allows you to execute `update` command manually. 您可能還有興趣了解設置 `UpdatePeriod` 應遵循的適當 `UpdateChannel`。

Update process of ASF involves update of entire folder structure that ASF is using, but without touching your own configs or databases located in `config` directory - this means that any extra files unrelated to ASF in its directory might be lost during update. Default value of `24` is a good balance between unnecessary checks, and ASF that is fresh enough.

Unless you have a **strong** reason to disable this feature, you should keep auto-updates enabled within reasonable `UpdatePeriod` **for your own good**. This is not only because we don't support anything but latest stable ASF release, but also because **we give our security guarantee only for latest version**. If you're using outdated ASF version then you're intentionally exposing yourself to all kind of issues, from small bugs, through broken functionality, ending with **[permanent Steam account suspensions](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ#did-somebody-get-banned-for-it)**, so we **strongly recommend**, for your own good, to always ensure that your ASF version is up to date. Auto-updates allow us to react quickly to all kind of issues by disabling or patching problematic code before it can escalate - if you opt out of it, you lose all of our security guarantees and risk consequences from running code that could be potentially harmful, not only to Steam network, but also (by definition) to your own Steam account.

* * *

### `WebLimiterDelay`

預設值為 `300` 的 `ushort` 類型。 This property defines, in miliseconds, the minimum amount of delay between sending two consecutive requests to the same Steam web-service. Such delay is required as **[AkamaiGhost](https://www.akamai.com)** service that Steam uses internally includes rate-limiting based on global number of requests sent across given time period. In normal circumstances akamai block is rather hard to achieve, but under very heavy workloads with a huge ongoing queue of requests, it's possible to trigger it if we keep sending too many requests across too short time period.

Default value was set based on assumption that ASF is the only tool accessing Steam web-services, in particular `steamcommunity.com`, `api.steampowered.com` and `store.steampowered.com`. If you're using other tools sending requests to the same web-services then you should make sure that your tool includes similar functionality of `WebLimiterDelay` and set both to double of default value, which would be `600`. This guarantees that under no circumstance you'll exceed sending more than 1 request per `300` ms.

In general, lowering `WebLimiterDelay` under default value is **strongly discouraged** as it might lead to various IP-related blocks, some of which are possible to be permanent. Default value is good enough for running a single ASF instance on the server, as well as using ASF in normal scenario along with original Steam client. It should be correct for majority of usages, and you should only increase it (never lower it), if - apart from using ASF, you're also using another tool that might send excessive number of requests to the same web-services that ASF is making use of. In short, global number of all requests sent from a single IP to a single Steam domain should never exceed more than 1 request per `300` ms.

除非您有充分的修改理由，否則應保持它為預設值。

* * *

### `WebProxy`

預設值為 `null` 的 `string` 類型。 This property defines a web proxy address that will be used for all internal http and https requests sent by ASF's `HttpClient`, especially to services such as `github.com`, `steamcommunity.com` and `store.steampowered.com`. Proxying ASF requests in general has no advantages, but it's exceptionally useful for bypassing various kinds of firewalls, especially the great firewall in China.

This property is defined as uri string:

> A URI string is composed of a scheme (http or https), a host, and an optional port. An example of a complete uri string is `"http://contoso.com:8080"`.

If your proxy requires user authentication, you will also need to set up `WebProxyUsername` and/or `WebProxyPassword`. If there is no such need, setting up this property alone is sufficient.

Right now ASF uses web proxy only for `http` and `https` requests, which **do not** include internal Steam network communication done within ASF's internal Steam client. There are currently no plans for supporting that, mainly due to missing **[SK2](https://github.com/SteamRE/SteamKit)** functionality. If you need/want it to happen, I'd suggest starting from there.

除非您有充分的修改理由，否則應保持它為預設值。

* * *

### `WebProxyPassword`

預設值為 `null` 的 `string` 類型。 This property defines password field used in basic, digest, NTLM, and Kerberos authentication that is supported by a target `WebProxy` machine providing proxy functionality. If your proxy doesn't require user credentials, there is no need for you to input anything here. Using this option makes sense only if `WebProxy` is used as well, as it has no effect otherwise.

除非您有充分的修改理由，否則應保持它為預設值。

* * *

### `WebProxyUsername`

預設值為 `null` 的 `string` 類型。 This property defines username field used in basic, digest, NTLM, and Kerberos authentication that is supported by a target `WebProxy` machine providing proxy functionality. If your proxy doesn't require user credentials, there is no need for you to input anything here. Using this option makes sense only if `WebProxy` is used as well, as it has no effect otherwise.

除非您有充分的修改理由，否則應保持它為預設值。

* * *

**[回到頂部](#configuration)**

* * *

## 機器人配置

As you should know already, every bot should have its own config based on example JSON structure below. Start from deciding how you want to name your bot (e.g. `1.json`, `main.json`, `primary.json` or `AnythingElse.json`) and head over to configuration.

**Notice:** Bot can't be named `ASF` (as that keyword is reserved for global config), ASF will also ignore all configuration files starting with a dot.

The bot config has following structure:

```json
{
    "AcceptGifts": false,
    "AutoSteamSaleEvent": false,
    "BotBehaviour": 0,
    "CustomGamePlayedWhileFarming": null,
    "CustomGamePlayedWhileIdle": null,
    "Enabled": false,
    "FarmingOrders": [],
    "GamesPlayedWhileIdle": [],
    "HoursUntilCardDrops": 3,
    "IdlePriorityQueueOnly": false,
    "IdleRefundableGames": true,
    "LootableTypes": [1, 3, 5],
    "MatchableTypes": [5],
    "OnlineStatus": 1,
    "PasswordFormat": 0,
    "Paused": false,
    "RedeemingPreferences": 0,
    "SendOnFarmingFinished": false,
    "SendTradePeriod": 0,
    "ShutdownOnFarmingFinished": false,
    "SteamLogin": null,
    "SteamMasterClanID": 0,
    "SteamParentalCode": null,
    "SteamPassword": null,
    "SteamTradeToken": null,
    "SteamUserPermissions": {},
    "TradingPreferences": 0,
    "TransferableTypes": [1, 3, 5],
    "UseLoginKeys": true
}
```

**Tip:** In order for bot to work properly, you should edit at least `Enabled`, `SteamLogin` and `SteamPassword` properties. I also suggest to take a look at some fine-tuning such as `HoursUntilCardDrops`, but all of that is optional. ASF configs are quite advanced to allow you tune your bots and ASF however you want, if you don't "require" such advanced setup, you don't really have to go deep into each config property. 簡單或複雜，ASF都取決於你。

* * *

以下是對所有選項的解釋：

### `AcceptGifts`

預設值為 `false` 的 `bool` 類型。 When enabled, ASF will automatically accept and redeem all steam gifts (including wallet gift cards) sent to the bot. This includes also gifts sent from users other than those defined in `SteamUserPermissions`. Keep in mind that gifts sent to e-mail address are not directly forwarded to the client, so ASF won't accept those without your help.

This option is recommended only for alt accounts, as it's very likely that you don't want to automatically redeem all gifts sent to your primary account. If you're unsure whether you want this feature enabled or not, keep it with default value of `false`.

* * *

### `AutoSteamSaleEvent`

預設值為 `false` 的 `bool` 類型。 During Steam summer/winter sale events Steam is known for providing you extra cards for browsing discovery queue each day, as well as through other event-specific activities. 啟用此選項後, ASF 將每 ` 8` 小時 (從程式開始後1小時內開始)自動檢查Steam發現佇列，並在需要時進行清除。 This option is not recommended if you want to do that action yourself, and typically it should make sense only on bot accounts. Moreover, you need to ensure that your account is at least of level `8` if you expect to receive those cards in the first place, which comes directly as Steam requirement. If you're unsure whether you want this feature enabled or not, keep it with default value of `false`.

Please note that due to constant Valve issues, changes and problems, **we give no guarantee whether this function will work properly**, therefore it's entirely possible that this option **will not work at all**. We do not accept **any** bug reports, neither support requests for this option. 它是在絕對沒有保證的情況下提供的, 一切風險將由您自行承擔。

* * *

### `BotBehaviour`

這是一個預設值為`0` 的 `byte flags` 屬性。 This property defines ASF bot-like behaviour during various events, and is defined as below:

| 值  | 名稱                            | 描述                                                                    |
| -- | ----------------------------- | --------------------------------------------------------------------- |
| 0  | None                          | No special bot behaviour, the least invasive mode, default            |
| 1  | RejectInvalidFriendInvites    | Will cause ASF to reject (instead of ignoring) invalid friend invites |
| 2  | RejectInvalidTrades           | Will cause ASF to reject (instead of ignoring) invalid trade offers   |
| 4  | RejectInvalidGroupInvites     | Will cause ASF to reject (instead of ignoring) invalid group invites  |
| 8  | DismissInventoryNotifications | Will cause ASF to automatically dismiss all inventory notifications   |
| 16 | MarkReceivedMessagesAsRead    | Will cause ASF to automatically mark all received messages as read    |

Please notice that this property is `flags` field, therefore it's possible to choose any combination of available values. Check out **[flags mapping](#json-mapping)** if you'd like to learn more. Not enabling any of flags results in `None` option.

In general you want to modify this property if you expect from ASF to do certain amount of automation related to its activity, as it'd be expected from a bot account, but not a primary account used in ASF. Therefore, changing this property makes sense mainly for alt accounts, although you're free to use selected options for main accounts as well.

Normal (`None`) ASF behaviour is to only automate things that user wants (e.g. cards farming or `SteamTradeMatcher` offers, if set in `TradingPreferences`). This is the least invasive mode, and it's beneficial to majority of users since you remain in full control over your account and you can decide yourself whether to allow certain out-of-scope interactions, or not.

Invalid friend invite is an invite that doesn't come from user with `FamilySharing` permission (defined in `SteamUserPermissions`) or above. ASF in normal mode ignores those invites, as you'd expect, giving you free choice whether to accept them, or not. `RejectInvalidFriendInvites` will cause those invites to be automatically rejected, which will practically disable option for other people to add you to their friend list (as ASF will deny all such requests, apart from people defined in `SteamUserPermissions`). Unless you want to outright deny all friend invites, you shouldn't enable this option.

Invalid trade offer is an offer that isn't accepted through built-in ASF module. More on this matter can be found in **[trading](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading)** section which explicitly defines what types of trade ASF is willing to accept automatically. Valid trades are also defined by other settings, especially `TradingPreferences`. `RejectInvalidTrades` will cause all invalid trade offers to be rejected, instead of being ignored. Unless you want to outright deny all trade offers that aren't automatically accepted by ASF, you shouldn't enable this option.

Invalid group invite is an invite that doesn't come from `SteamMasterClanID` group. ASF in normal mode ignores those group invites, as you'd expect, allowing you to decide yourself if you want to join particular Steam group or not. `RejectInvalidGroupInvites` will cause all those group invites to be automatically rejected, effectively making it impossible to invite you to any other group than `SteamMasterClanID`. Unless you want to outright deny all group invites, you shouldn't enable this option.

`DismissInventoryNotifications` is extremely useful when you start getting annoyed by contact Steam notification about receiving new items. ASF can't get rid of the notification itself, as that's built-in into your Steam client, but it's able to automatically clear the notification after receiving it, which will no longer leave "new items in inventory" notification hanging around. If you expect to evaluate yourself all received items (especially cards idled with ASF), then naturally you shouldn't enable this option. When you start going crazy, remember this is offered as an option.

`MarkReceivedMessagesAsRead` will automatically mark **all** messages being received by the account on which ASF is running. This typically should be used by alt accounts only in order to clear "new message" notification coming e.g. from you during executing ASF commands. We do not recommend this option for primary accounts, unless you want to cut yourself from any kind of new messages notifications, **including** those that happened while you were offline, assuming that ASF was still left open dismissing it.

If you're unsure how to configure this option, it's best to leave it at default.

* * *

### `CustomGamePlayedWhileFarming`

預設值為 `null` 的 `string` 類型。 When ASF is farming, it can display itself as "Playing non-steam game: `CustomGamePlayedWhileFarming`" instead of currently farmed game. This can be useful if you would like to let your friends know that you're farming, yet you don't want to use `OnlineStatus` of `Offline`. Please note that ASF cannot guarantee the actual display order of Steam network, therefore this is only a suggestion that may, or may not, display properly. Default value of `null` disables this feature.

* * *

### `CustomGamePlayedWhileIdle`

預設值為 `null` 的 `string` 類型。 Similar to `CustomGamePlayedWhileFarming`, but for the situation when ASF has nothing to do (as account is fully farmed). Default value of `null` disables this feature.

* * *

### `啟用`

預設值為 `false` 的 `bool` 類型。 This property defines if bot is enabled. Enabled bot instance (`true`) will automatically start on ASF run, while disabled bot instance (`false`) will need to be started manually. By default every bot is disabled, so you probably want to switch this property to `true` for all of your bots that should be started automatically.

* * *

### `FarmingOrders`

`ImmutableHashSet<byte>` type with default value of being empty. This property defines the **preferred** farming order used by ASF for given bot account. 目前可選的掛卡佇列如下：

| 值  | 名稱                        | 描述                                                                               |
| -- | ------------------------- | -------------------------------------------------------------------------------- |
| 0  | Unordered                 | No sorting, slightly improving CPU performance                                   |
| 1  | AppIDsAscending           | Try to farm games with lowest `appIDs` first                                     |
| 2  | AppIDsDescending          | Try to farm games with highest `appIDs` first                                    |
| 3  | CardDropsAscending        | Try to farm games with lowest number of card drops remaining first               |
| 4  | CardDropsDescending       | Try to farm games with highest number of card drops remaining first              |
| 5  | HoursAscending            | Try to farm games with lowest number of hours played first                       |
| 6  | HoursDescending           | Try to farm games with highest number of hours played first                      |
| 7  | NamesAscending            | Try to farm games in alphabetical order, starting from A                         |
| 8  | NamesDescending           | Try to farm games in reverse alphabetical order, starting from Z                 |
| 9  | Random                    | Try to farm games in totally random order (different on each run of the program) |
| 10 | BadgeLevelsAscending      | Try to farm games with lowest badge levels first                                 |
| 11 | BadgeLevelsDescending     | Try to farm games with highest badge levels first                                |
| 12 | RedeemDateTimesAscending  | Try to farm oldest games on our account first                                    |
| 13 | RedeemDateTimesDescending | Try to farm newest games on our account first                                    |
| 14 | MarketableAscending       | Try to farm games with unmarketable card drops first                             |
| 15 | MarketableDescending      | Try to farm games with marketable card drops first                               |

Since this property is an array, it allows you to use several different settings in your fixed order. For example, you can include values of `15`, `11` and `7` in order to sort by marketable games first, then by those with highest badge level, and finally alphabetically. As you can guess, the order actually matters, as reverse one (`7`, `11` and `15`) achieves something entirely different. Majority of people will probably use just one order out of all of them, but in case you want to, you can also sort further by extra parameters.

Also notice the word "try" in all above descriptions - the actual ASF order is heavily affected by selected **[cards farming algorithm](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)** and sorting will affect only results that ASF considers same performance-wise. For example, in `Simple` algorithm, selected `FarmingOrders` should be entirely respected in current farming session (as every game has the same performance value), while in `Complex` algorithm actual order is affected by hours first, and then sorted according to chosen `FarmingOrders`. This will lead to different results, as games with existing playtime will have a priority over others, so effectively ASF will prefer games that already passed required `HoursUntilCardDrops` firstly, and only then sorting those games further by your chosen `FarmingOrders`. Likewise, once ASF runs out of already-bumped games, it'll sort remaining queue by hours first (as that will decrease time required for bumping any of remaining titles to `HoursUntilCardDrops`). Therefore, this config property is only a **suggestion** that ASF will try to respect, as long as it doesn't affect performance negatively (in this case, ASF will always prefer idling performance over `FarmingOrders`).

There is also idling priority queue that is accessible through `iq` **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. If it's used, actual idling order is sorted firstly by performance, then by idling queue, and finally by your `FarmingOrders`.

* * *

### `GamesPlayedWhileIdle`

預設值為空的 `ImmutableHashSet<uint>` 類型。 If ASF has nothing to farm it can play your specified steam games (`appIDs`) instead. Playing games in such manner increases your "hours played" of those games, but nothing else apart of it. This feature can be enabled at the same time with `CustomGamePlayedWhileIdle` in order to play your selected games while showing custom status in Steam network, but in this case, like in `CustomGamePlayedWhileFarming` case, the actual display order is not guaranteed. Please note that Steam allows ASF to play only up to `32` `appIDs` total, therefore you can put only as many games in this property.

* * *

### `HoursUntilCardDrops`

`byte` type with default value of `3`. This property defines if account has card drops restricted, and if yes, for how many initial hours. Restricted card drops means that account is not receiving any card drops from given game until the game is played for at least `HoursUntilCardDrops` hours. Unfortunately there is no magical way to detect that, so ASF relies on you. This property affects **[cards farming algorithm](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)** that will be used. Setting this property properly will maximize profits and minimize time required for cards to be farmed. Remember that there is no obvious answer whether you should use one or another value, since it fully depends on your account. It seems that older accounts which never asked for refund have unrestricted card drops, so they should use a value of `0`, while new accounts and those who did ask for refund have restricted card drops with a value of `3`. This is however only theory, and should not be taken as a rule. The default value for this property was set based on "lesser evil" and majority of use cases.

* * *

### `IdlePriorityQueueOnly`

預設值為 `false` 的 `bool` 類型。 This property defines if ASF should consider for automatic idling only apps that you added yourself to priority idling queue available with `iq` **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. When this option is enabled, ASF will skip all `appIDs` that are missing on the list, effectively allowing you to cherry-pick games for automatic ASF idling. Keep in mind that if you didn't add any games to the queue then ASF will act as if there is nothing to idle on your account. If you're unsure whether you want this feature enabled or not, keep it with default value of `false`.

* * *

### `IdleRefundableGames`

預設值為 `true` 的 `bool` 類型。 This property defines if ASF is permitted to idle games that are still refundable. A refundable game is a game that you bought in last 2 weeks through Steam Store and didn't play for longer than 2 hours yet, as stated on **[Steam refunds](https://store.steampowered.com/steam_refunds)** page. By default when this option is set to `true`, ASF ignores Steam refunds policy entirely and idles everything, as most people would expect. However, you can change this option to `false` if you want to ensure that ASF won't idle any of your refundable games too soon, allowing you to evaluate those games yourself and refund if needed without worrying about ASF affecting playtime negatively. Please note that if you disable this option then games you purchased from Steam Store won't be idled by ASF for up to 14 days since redeem date, which will show as nothing to idle if your account doesn't own anything else. If you're unsure whether you want this feature enabled or not, keep it with default value of `true`.

* * *

### `LootableTypes`

`ImmutableHashSet<byte>` type with default value of `1, 3, 5` steam item types. This property defines ASF behaviour when looting - both manual and automatic. ASF will ensure that only items from `LootableTypes` will be included in a trade offer, therefore this property allows you to choose what you want to receive in a trade offer that is being sent to you.

| 值 | 名稱                | 描述                                                          |
| - | ----------------- | ----------------------------------------------------------- |
| 0 | Unknown           | 不屬於以下任何類型的類型                                                |
| 1 | BoosterPack       | Booster pack containing 3 random cards from a game          |
| 2 | Emoticon          | 在Steam聊天中使用的表情符號                                            |
| 3 | FoilTradingCard   | 閃亮類型的`TradingCard`                                          |
| 4 | ProfileBackground | 可在您的Steam個人資料頁中使用的背景                                        |
| 5 | TradingCard       | Steam交易卡片，可用於合成徽章 (非閃卡）                                     |
| 6 | SteamGems         | Steam gems being used for crafting boosters, sacks included |
| 7 | SaleItem          | Steam特賣期間獲得的特殊物品                                            |
| 8 | Consumable        | Special consumable items that disappear after being used    |

Please note that regardless of the settings above, ASF will only ask for Steam (`appID` of 753) community (`contextID` of 6) items, so all game items, gifts and likewise, are excluded from the trade offer by definition.

Default ASF setting is based on most common usage of the bot, with looting only booster packs, and trading cards (including foils). 這裡定義的屬性允許你以任何令你滿意的方式改變這種行為。 Please keep in mind that all types not defined above will show as `Unknown` type, which is especially important when Valve releases some new Steam item, that will be marked as `Unknown` by ASF as well, until it's added here (in the future release). That's why in general it's not recommended to include `Unknown` type in your `LootableTypes`, unless you know what you're doing, and you also understand that ASF will send your entire inventory in a trade offer if Steam Network gets broken again and reports all your items as `Unknown`. My strong suggestion is to not include `Unknown` type in the `LootableTypes`, even if you expect to loot everything.

* * *

### `MatchableTypes`

`ImmutableHashSet<byte>` type with default value of `5` Steam item types. This property defines which Steam item types are permitted to be matched when `SteamTradeMatcher` option in `TradingPreferences` is enabled. 類型的定義如下：

| 值 | 名稱                | 描述                                                          |
| - | ----------------- | ----------------------------------------------------------- |
| 0 | Unknown           | 不屬於以下任何類型的類型                                                |
| 1 | BoosterPack       | Booster pack containing 3 random cards from a game          |
| 2 | Emoticon          | 在Steam聊天中使用的表情符號                                            |
| 3 | FoilTradingCard   | 閃亮類型的`TradingCard`                                          |
| 4 | ProfileBackground | 可在您的Steam個人資料頁中使用的背景                                        |
| 5 | TradingCard       | Steam交易卡片，可用於合成徽章 (非閃卡）                                     |
| 6 | SteamGems         | Steam gems being used for crafting boosters, sacks included |
| 7 | SaleItem          | Steam特賣期間獲得的特殊物品                                            |
| 8 | Consumable        | Special consumable items that disappear after being used    |

Of course, types that you should use for this property typically include only `2`, `3`, `4` and `5`, as only those types are supported by STM. ASF includes proper logic for discovering rarity of the items, therefore it's also safe to match emoticons or backgrounds, as ASF will properly consider fair only those items from the same game and type, that also share the same rarity.

Please note that **ASF is not a trading bot** and **will NOT care about the market price**. If you don't consider items of the same rarity from the same set to be the same price-wise, then this option is NOT for you. Please evaluate twice if you understand and agree with this statement before you decide to change this setting.

Unless you know what you're doing, you should keep it with default value of `5`.

* * *

### `OnlineStatus`

這是一個預設值為`1` 的 `byte flags` 類型屬性。 This property specifies Steam community status that the bot will be announced with after logging in to Steam network. Currently you can choose one of below statuses:

| 值 | 名稱    |
| - | ----- |
| 0 | 離線    |
| 1 | 線上    |
| 2 | 忙碌    |
| 3 | 離開    |
| 4 | 打瞌睡   |
| 5 | 期待交易  |
| 6 | 期待玩游戲 |
| 7 | 隐身    |

`Offline` status is extremely useful for primary accounts. As you should know, farming a game actually shows your steam status as "Playing game: XXX", which can be misleading to your friends, confusing them that you're playing a game while actually you're only farming it. Using `Offline` status solves that issue - your account will never be shown as "in-game" when you're farming steam cards with ASF. This is possible thanks to the fact that ASF does not have to sign in into Steam Community in order to work properly, so we're in fact playing those games, connected to Steam network, but without announcing our online presence at all. Keep in mind that played games using offline status will still count towards your playtime, and show as "recently played" on your profile.

In addition to that, this feature is also important if you want to receive notifications and unread messages when ASF is running, while not keeping Steam client open at the same time. This is because ASF acts as a Steam client itself, and whether ASF would like it or not, Steam broadcasts all those messages and other events to it. This is not a problem if you have both ASF and your own Steam client running, as both clients receive exactly the same events. However, if just ASF is running, Steam network could mark certain events and messages as "delivered", despite of your traditional Steam client not receiving it due to not being present. Offline status also solves this problem, as ASF is never considered for any community events in this case, so all unread messages and other events will be properly marked as unread when you come back.

It's important to note that ASF running on `Offline` mode will **not** be able to receive commands in usual Steam chat way, as the chat, as well as entire community presence is in fact, entirely offline. A solution to this issue is using `Invisible` mode instead which works in a similar way (not exposing status), but keeps the ability to receive and respond to messages (so also a potential to dismiss notifications and unread messages as stated above). `Invisible` mode makes the most sense on alt accounts that you don't want to expose (status-wise), but still be able to send commands to.

However, there is one catch with `Invisible` mode - it doesn't go well with primary accounts. This is because **any** Steam session that is currently online **exposes** the status, even if ASF itself does not. This is caused by the current limitation/bug of the Steam network that isn't possible to be fixed on ASF side, so if you want to use `Invisible` mode you will also need to ensure that **all** other sessions to the same account use `Invisible` mode as well. This will be the case on alt accounts where ASF is hopefully the only active session, but on primary accounts you'll almost always prefer to show as `Online` to your friends, hiding only ASF activity, and in this case `Invisible` mode will be entirely useless for you (we recommend to use `Offline` mode instead). Hopefully this limitation/bug will be eventually solved in the future by Valve, but I wouldn't expect that to happen anytime soon...

If you're unsure how to set up this property, it's recommended to use a value of `0` (`Offline`) for primary accounts, and default `1` (`Online`) otherwise.

* * *

### `PasswordFormat`

這是一個預設值為`0` 的 `byte flags` 類型。 This property defines the format of `SteamPassword` property, and currently supports - `0` for `PlainText`, `1` for `AES` and `2` for `ProtectedDataForCurrentUser`. Please refer to **[Security](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security)** section if you want to learn more, as you'll need to ensure that `SteamPassword` property indeed includes password in matching `PasswordFormat`. In other words, when you change `PasswordFormat` then your `SteamPassword` should be **already** in that format, not just aiming to be. Unless you know what you're doing, you should keep it with default value of `0`.

* * *

### `Paused`

預設值為 `false` 的 `bool` 類型。 This property defines initial state of `CardsFarmer` module. With default value of `false`, bot will automatically start farming when it's started, either because of `Enabled` or `start` command. Switching this property to `true` should be done only if you want to manually `resume` automatic farming process, for example because you want to use `play` all the time and never use automatic `CardsFarmer` module - this works exactly the same as `pause` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. If you're unsure whether you want this feature enabled or not, keep it with default value of `false`.

* * *

### `RedeemingPreferences`

這是一個預設值為`0` 的 `byte flags` 屬性。 This property defines ASF behaviour when redeeming cd-keys, and is defined as below:

| 值 | 名稱               | 描述                                                                             |
| - | ---------------- | ------------------------------------------------------------------------------ |
| 0 | None             | No redeeming preferences, typical                                              |
| 1 | Forwarding       | 將無法兌換的金鑰转發給其他機器人                                                               |
| 2 | Distributing     | Distribute all keys among itself and other bots                                |
| 4 | KeepMissingGames | Keep keys for (potentially) missing games when forwarding, leaving them unused |

Please notice that this property is `flags` field, therefore it's possible to choose any combination of available values. Check out **[flags mapping](#json-mapping)** if you'd like to learn more. Not enabling any of flags results in `None` option.

`Forwarding` will cause bot to forward a key that is not possible to redeem, to another connected and logged on bot that is missing that particular game (if possible to check). The most common situation is forwarding `AlreadyPurchased` game to another bot that is missing that particular game, but this option also covers other scenarios, such as `DoesNotOwnRequiredApp`, `RateLimited` or `RestrictedCountry`.

`Distributing` will cause bot to distribute all received keys among itself and other bots. This means that every bot will get a single key from the batch. Typically this is used only when you're redeeming many keys for the same game, and you want to evenly distribute them among your bots, as opposed to redeeming keys for various different games. This feature makes no sense if you're redeeming only one key in a single `redeem` action (as there are no extra keys to be distributed).

`KeepMissingGames` will cause bot to skip `Forwarding` when we can't be sure if key being redeemed is in fact owned by our bot, or not. This basically means that `Forwarding` will apply **only** to `AlreadyPurchased` keys, instead of covering also other cases such as `DoesNotOwnRequiredApp`, `RateLimited` or `RestrictedCountry`. Typically you might want to use this option on primary account, to ensure that keys being redeemed on it won't be forwarded further if your bot for example becomes temporarily `RateLimited`. As you can guess from the description, this field has absolutely no effect if `Forwarding` is not enabled.

Enabling both `Forwarding` and `Distributing` will add distributing feature on top of forwarding one, which makes ASF trying to redeem one key on all bots firstly (forwarding) before moving to the next one (distributing). Typically you want to use this option only when you want `Forwarding`, but with altered behaviour of switching the bot on key being used, instead of always going in-order with every key (which would be `Forwarding` alone). This behaviour can be beneficial if you know that majority or even all of your keys are tied to the same game, because in this situation `Forwarding` alone would try to redeem everything on one bot firstly (which makes sense if your keys are for unique games), and `Forwarding` + `Distributing` will switch the bot on the next key, "distributing" the task of redeeming new key onto another bot than the initial one (which makes sense if keys are for the same game, skipping one pointless attempt per key).

The actual bots order for all of the redeeming scenarios is alphabetical, excluding bots that are unavailable (not connected, stopped or likewise). Please keep in mind that there is per-IP and per-account hourly limit of redeeming tries, and every redeem try that didn't end with `OK` contributes to failed tries. ASF will do its best to minimize number of `AlreadyPurchased` failures, e.g. by trying to avoid forwarding a key to another bot that already owns that particular game, but it's not always guaranteed to work due to how Steam is handling licenses. Using redeeming flags such as `Forwarding` or `Distributing` will always increase your likelyhood to hit `RateLimited`.

Also keep in mind that you can't forward or distribute keys to bots that you do not have access to. This should be obvious, but ensure that you're at least `Operator` of all the bots you want to include in your redeeming process, for example with `status ASF` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**.

* * *

### `SendOnFarmingFinished`

預設值為 `false` 的 `bool` 類型。 When ASF is done with farming given account, it can automatically send steam trade containing everything farmed up to this point to user with `Master` permission, which is very convenient if you don't want to bother with trades yourself. This option works the same as `loot` command, therefore keep in mind that it requires user with `Master` permission set, you might also require valid `SteamTradeToken`, including using an account that is actually eligible for trading. In addition to initiating `loot` after finishing farming, ASF will also initiate `loot` on each new items notification (when not farming), and after completing each trade that results in new items (always) when this option is active. This is especially useful for "forwarding" items received from other people to our account.

Typically you'll want to use **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** together with this feature, although it's not a requirement if you intend to confirm manually in timely fashion. If you're not sure how to set this property, leave it with default value of `false`.

* * *

### `SendTradePeriod`

這是一個預設值為`0` 的 `byte flags` 類型。 This property works very similar to `SendOnFarmingFinished` property, with one difference - instead of sending trade when farming is done, we can also send it every `SendTradePeriod` hours, regardless of how much we have to farm left. This is useful if you want to `loot` your alt accounts on usual basis instead of waiting for it to finish farming. Default value of `0` disables this feature, if you want your bot to send you trade e.g. every day, you should put `24` here.

Typically you'll want to use **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** together with this feature, although it's not a requirement if you intend to confirm manually in timely fashion. If you're not sure how to set this property, leave it with default value of `0`.

* * *

### `ShutdownOnFarmingFinished`

預設值為 `false` 的 `bool` 類型。 ASF is "occupying" an account for the whole time of process being active. When given account is done with farming, ASF periodically checks it (every `IdleFarmingPeriod` hours), if perhaps some new games with steam cards were added in the meantime, so it can resume farming of that account without a need to restart the process. This is useful for majority of people, as ASF can automatically resume farming when needed. However, you may actually want to stop the process when given account is fully farmed, you can achieve that by setting this property to `true`. When enabled, ASF will proceed with logging off when account is fully farmed, which means that it won't be periodically checked or occupied anymore. You should decide yourself if you prefer ASF to work on given bot instance for the whole time, or if perhaps ASF should stop it when farming process is done. When all accounts are stopped and process is not running in `--process-required` **[mode](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-Line-Arguments)**, ASF will shutdown as well, putting your machine at rest and allowing you to schedule other actions, such as sleep or shutdown at the same moment of last card dropping.

If you're not sure how to set this property, leave it with default value of `false`.

* * *

### `SteamLogin`

預設值為 `null` 的 `string` 類型。 This property defines your steam login - the one you use for logging in to steam. In addition to defining steam login here, you may also keep default value of `null` if you want to enter your steam login on each ASF startup instead of putting it in the config. This may be useful for you if you don't want to save sensitive data in config file.

* * *

### `SteamMasterClanID`

預設值 為`0` 的 `ulong` 類型。 This property defines the steamID of the steam group that bot should automatically join, including its group chat. You can check steamID of your group by navigating to its **[page](https://steamcommunity.com/groups/archiasf)**, then adding `/memberslistxml?xml=1` to the end of the link, so the link will look like **[this](https://steamcommunity.com/groups/archiasf/memberslistxml?xml=1)**. Then you can get steamID of your group from the result, it's in `<groupID64>` tag. In above example it would be `103582791440160998`. In addition to trying to join given group at startup, the bot will also automatically accept group invites to this group, which makes it possible for you to invite your bot manually if your group has private membership. If you don't have any group dedicated for your bots, you should keep this property with default value of `0`.

* * *

### `SteamParentalCode`

預設值為 `null` 的 `string` 類型。 This property defines your steam parental PIN. ASF requires an access to resources protected by steam parental, therefore if you use that feature, you need to provide ASF with parental unlock PIN, so it can operate normally. Default value of `null` means that there is no steam parental PIN required to unlock this account, and this is probably what you want if you don't use steam parental functionality. In addition to defining steam parental PIN here, you may also use value of `0` if you want to enter your steam parental PIN on each ASF startup instead of putting it in the config. This may be useful for you if you don't want to save sensitive data in config file.

* * *

### `SteamPassword`

預設值為 `null` 的 `string` 類型。 This property defines your steam password - the one you use for logging in to steam. In addition to defining steam password here, you may also keep default value of `null` if you want to enter your steam password on each ASF startup instead of putting it in the config. This may be useful for you if you don't want to save sensitive data in config file.

* * *

### `SteamTradeToken`

預設值為 `null` 的 `string` 類型。 When you have your bot on your friend list, then bot can send a trade to you right away without worrying about trade token, therefore you can leave this property at default value of `null`. If you however decide to NOT have your bot on your friend list, then you will need to generate and fill a trade token as the user that this bot is expecting to send trades to. In other words, this property should be filled with trade token of the account that is defined with `Master` permission in `SteamUserPermissions` of **this** bot instance.

In order to find your token, as logged in user with `Master` permission, navigate **[here](https://steamcommunity.com/my/tradeoffers/privacy)** and take a look at your trade URL. The token we're looking for is made out of 8 characters after `&token=` part in your trade URL. You should copy and put those 8 characters here, as `SteamTradeToken`. Do not include whole trading URL, neither `&token=` part, only the token itself (8 characters).

* * *

### `SteamUserPermissions`

`ImmutableDictionary<ulong, byte>` type with default value of being empty. This property is a dictionary property which maps given Steam user identified by his 64-bit steam ID, to `byte` number that specifies his permission in ASF instance. Currently available bot permissions in ASF are defined as:

| 值 | 名稱            | 描述                                                                                                                                                                                                 |
| - | ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0 | None          | No permission, this is mainly a reference value that is assigned to steam IDs missing in this dictionary - there is no need to define anybody with this permission                                 |
| 1 | FamilySharing | Provides minimum access for family sharing users. Once again, this is mainly a reference value since ASF is capable of automatically discovering steam IDs that we permitted for using our library |
| 2 | Operator      | Provides basic access to given bot instances, mainly adding licenses and redeeming keys                                                                                                            |
| 3 | Master        | Provides full access to given bot instance                                                                                                                                                         |

In short, this property allows you to handle permissions for given users. Permissions are important mainly for access to ASF **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**, but also for enabling many ASF features, such as accepting trades. For example you might want to set your own account as `Master`, and give `Operator` access to 2-3 of your friends so they can easily redeem keys for your bot with ASF, while **not** being eligible e.g. for stopping it. Thanks to that you can easily assign permissions to given users and let them use your bot to some specified by you degree.

We recommend to set exactly one user as `Master`, and any amount you wish as `Operators` and below. While it's technically possible to set multiple `Masters` and ASF will work correctly with them, for example by accepting all of their trades sent to the bot, ASF will use only one of them (with lowest steam ID) for every action that requires a single target, for example a `loot` request, so also properties like `SendOnFarmingFinished` or `SendTradePeriod`. If you perfectly understand those limitations, especially the fact that `loot` request will always send items to the `Master` with lowest steam ID, regardless of the `Master` that actually executed the command, then you can define multiple users with `Master` permission here, but we still recommend a single master scheme - multiple masters scheme is discouraged setup that is not supported.

It's nice to note that there is one more extra `Owner` permission, which is declared as `SteamOwnerID` global config property. You can't assign `Owner` permission to anybody here, as `SteamUserPermissions` property defines only permissions that are related to the bot instance, and not ASF as a process. For bot-related tasks, `SteamOwnerID` is treated the same as `Master`, so defining your `SteamOwnerID` here is not necessary.

* * *

### `TradingPreferences`

這是一個預設值為`0` 的 `byte flags` 屬性。 此屬性定義ASF在交易中的行為，定義如下：

| 值  | 名稱                  | 描述                                                                                                                                                                                              |
| -- | ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0  | None                | 無交易偏好 - 只接受來自`Master` 的交易                                                                                                                                                                       |
| 1  | AcceptDonations     | 接受對我們無任何損失的交易                                                                                                                                                                                   |
| 2  | SteamTradeMatcher   | 被動參與 **[STM](https://www.steamtradematcher.com)**交易。 訪問 **[交易](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading#steamtradematcher)** 瞭解更多資訊                                          |
| 4  | 匹配所有物品              | 需要設置 `SteamTradeMatcher`, 並與其同時使用--除了對我們有利的和無損的交易外, 還接受不利交易                                                                                                                                     |
| 8  | DontAcceptBotTrades | 不自動接受來自其他機器人實例的 `loot` 交易                                                                                                                                                                       |
| 16 | MatchActively       | Actively participates in **[STM](https://www.steamtradematcher.com)**-like trades. Visit **[trading](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading#matchactively)** for more info |

Please notice that this property is `flags` field, therefore it's possible to choose any combination of available values. Check out **[flags mapping](#json-mapping)** if you'd like to learn more. Not enabling any of flags results in `None` option.

For further explanation of ASF trading logic, and description of every available flag, please visit **[trading](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading)** section.

* * *

### `TransferableTypes`

`ImmutableHashSet<byte>` type with default value of `1, 3, 5` steam item types. This property defines which Steam item types will be considered for transferring between bots, during `transfer` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. ASF will ensure that only items from `TransferableTypes` will be included in a trade offer, therefore this property allows you to choose what you want to receive in a trade offer that is being sent to one of your bots.

| 值 | 名稱                | 描述                                                          |
| - | ----------------- | ----------------------------------------------------------- |
| 0 | Unknown           | 不屬於以下任何類型的類型                                                |
| 1 | BoosterPack       | Booster pack containing 3 random cards from a game          |
| 2 | Emoticon          | 在Steam聊天中使用的表情符號                                            |
| 3 | FoilTradingCard   | 閃亮類型的`TradingCard`                                          |
| 4 | ProfileBackground | 可在您的Steam個人資料頁中使用的背景                                        |
| 5 | TradingCard       | Steam交易卡片，可用於合成徽章 (非閃卡）                                     |
| 6 | SteamGems         | Steam gems being used for crafting boosters, sacks included |
| 7 | SaleItem          | Steam特賣期間獲得的特殊物品                                            |
| 8 | Consumable        | Special consumable items that disappear after being used    |

Please note that regardless of the settings above, ASF will only ask for Steam (`appID` of 753) community (`contextID` of 6) items, so all game items, gifts and likewise, are excluded from the trade offer by definition.

Default ASF setting is based on most common usage of the bot, with transfering only booster packs, and trading cards (including foils). 這裡定義的屬性允許你以任何令你滿意的方式改變這種行為。 Please keep in mind that all types not defined above will show as `Unknown` type, which is especially important when Valve releases some new Steam item, that will be marked as `Unknown` by ASF as well, until it's added here (in the future release). That's why in general it's not recommended to include `Unknown` type in your `TransferableTypes`, unless you know what you're doing, and you also understand that ASF will send your entire inventory in a trade offer if Steam Network gets broken again and reports all your items as `Unknown`. My strong suggestion is to not include `Unknown` type in the `TransferableTypes`, even if you expect to transfer everything.

* * *

### `UseLoginKeys`

預設值為 `true` 的 `bool` 類型。 This property defines if ASF should use login keys mechanism for this Steam account. Login keys mechanism works very similar to official Steam client's "remember me" option, which makes it possible for ASF to store and use temporary one-time use login key for next logon attempt, effectively skipping a need of providing password, Steam Guard or 2FA code as long as our login key is valid. Login key is stored in `BotName.db` file and updated automatically. This is why you don't need to provide password/SteamGuard/2FA code after logging in successfully with ASF just once.

Login keys are used by default for your convenience, so you don't need to input `SteamPassword`, SteamGuard or 2FA code (when not using **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)**) on each login. It's also superior alternative since login key can be used only for a single time and does not reveal your original password in any way. Exactly the same method is being used by your original Steam client, which saves your account name and login key for your next logon attempt, effectively being the same as using `SteamLogin` with `UseLoginKeys` and empty `SteamPassword` in ASF.

However, some people might be concerned even about this little detail, therefore this option is available here for you if you'd like to ensure that ASF won't store any kind of token that would allow resuming previous session after being closed, which will result in full authentication being mandatory on each login attempt. Disabling this option will work exactly the same as not checking "remember me" in official Steam client. Unless you know what you're doing, you should keep it with default value of `true`.

**[返回頂部](#configuration)**

* * *

## 檔案結構

ASF is using quite simple file structure.

    ├── config
    │     ├── ASF.json
    │     ├── ASF.db
    │     ├── Bot1.json
    │     ├── Bot1.db
    │     ├── Bot1.bin
    │     ├── Bot2.json
    │     ├── Bot2.db
    │     ├── Bot2.bin
    │     └── ...
    ├── ArchiSteamFarm.dll
    ├── log.txt
    └── ...
    

In order to move ASF to new location, for example another PC, it's enough to move/copy `config` directory alone, and that's the recommended way of doing any form of "ASF backups", since you can always download the remaining (program) part from the GitHub, while not risking corrupting internal ASF files, e.g. through a faulty backup.

`log.txt` file holds the log generated by your last ASF run. This file doesn't contain any sensitive information, and is extremely useful when it comes to issues, crashes or simply as an information to you what happened in last ASF run. We will very often ask about this file if you run into issues or bugs. ASF automatically manages this file for you, but you can further tweak ASF **[logging](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Logging)** module if you're advanced user.

`config` directory is the place that holds configuration for ASF, including all of its bots.

`ASF.json` is a global ASF configuration file. This config is used for specifying how ASF behaves as a process, which affects all of the bots and program itself. You can find global properties there, such as ASF process owner, auto-updates or debugging.

`BotName.json` is a config of given bot instance. This config is used for specifying how given bot instance behaves, therefore those settings are specific to that bot only and not shared across other ones. This allows you to configure bots with various different settings and not necessarily all of them working in exactly the same way. Every bot is named using unique identifier, chosen by you in place of `BotName`.

Apart from config files, ASF also uses `config` directory for storing databases.

`ASF.db` is a global ASF database file. It acts as a global persistent storage and is used for saving various information related to ASF process, such as IPs of local Steam servers. **You should not edit this file**.

`BotName.db` is a database of given bot instance. This file is used for storing crucial data about given bot instance in persistent storage, such as login keys or ASF 2FA. **您不應對此檔進行任何改變**。

`BotName.bin` is a special file of given bot instance, which holds information about Steam sentry hash. Sentry hash is used for authenticating using `SteamGuard` mechanism, very similar to Steam `ssfn` file. **您不應對此檔進行任何改變**。

`BotName.keys` is a special file that can be used for importing keys into **[background games redeemer](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer)**. It's not mandatory and not generated, but recognized by ASF. This file is automatically deleted after keys are successfully imported.

`BotName.maFile` is a special file that can be used for importing **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)**. It's not mandatory and not generated, but recognized by ASF if your `BotName` does not use ASF 2FA yet. This file is automatically deleted after ASF 2FA is successfully imported.

**[返回頂部](#configuration)**

* * *

## JSON 映射

Every configuration property has its type. Type of the property defines values that are valid for it. You can only use values that are valid for given type - if you use invalid value, then ASF won't be able to parse your config.

**We strongly recommend to use ConfigGenerator for generating configs** - it handles most of the low-level stuff (such as types validation) for you, so you only need to input proper values, and you also don't need to understand variable types specified below. This section is mainly for people generating/editing configs manually, so they know what values they can use.

Types used by ASF are native C# types, which are specified below:

* * *

`bool` - Boolean type accepting only `true` and `false` values.

Example: `"Enabled": true`

* * *

`byte` - Unsigned byte type, accepting only integers from `0` to `255` (inclusive).

Example: `"ConnectionTimeout": 60`

* * *

`ushort` - Unsigned short type, accepting only integers from `0` to `65535` (inclusive).

Example: `"WebLimiterDelay": 300`

* * *

`uint` - Unsigned integer type, accepting only integers from `0` to `4294967295` (inclusive).

* * *

`ulong` - Unsigned long integer type, accepting only integers from `0` to `18446744073709551615` (inclusive).

Example: `"SteamMasterClanID": 103582791440160998`

* * *

`string` - String type, accepting any sequence of characters, including empty sequence `""` and `null`. Both empty sequence as well as `null` value is treated the same by ASF, so it's up to your preference which one you want to use.

Examples: `"SteamLogin": null`, `"SteamLogin": ""`, `"SteamLogin": "MyAccountName"`

* * *

`ImmutableHashSet<valueType>` - Immutable collection (set) of unique values in given `valueType`. In JSON, it's defined as array of elements in given `valueType`. ASF uses `HashSet` to indicate that given property makes sense only for unique values, therefore it'll intentionally ignore any potential duplicates during parsing (if you happened to supply them anyway).

Example for `ImmutableHashSet<uint>`: `"Blacklist": [267420, 303700, 335590]`

* * *

`ImmutableDictionary<keyType, valueType>` - Immutable dictionary (map) that maps a unique key specified in its `keyType`, to value specified in its `valueType`. In JSON, it's defined as an object with key-value pairs. Keep in mind that `keyType` is always quoted in this case, even if it's value type such as `ulong`. There is also a strict requirement of the key being unique across the map, this time enforced by JSON itself.

Example for `ImmutableDictionary<ulong, byte>`: `"SteamUserPermissions": { "76561198174813138": 3, "76561198174813137": 1 }`

* * *

`flags` - Flags attribute combines several different properties into one final value by applying bitwise operations. This allows you to choose any possible combination of various different allowed values at the same time. The final value is constructed as a sum of values of all enabled options.

舉例來說，給出以下的值：

| 值 | 名稱   |
| - | ---- |
| 0 | None |
| 1 | A    |
| 2 | B    |
| 4 | C    |

Using `B + C` would result in value of `6`, using `A + C` would result in value of `5`, using `C` would result in value of `4` and so on. This allows you to create any possible combination of enabled values - if you decided to enable all of them, making `None + A + B + C`, you'd get value of `7`. Also notice that flag with value of `0` is enabled by definition in all other available combinations, therefore very often it's a flag that doesn't enable anything specifically (such as `None`).

So as you can see, in above example we have 3 available flags to switch on/off (`A`, `B`, `C`), and `8` possible values overall:

- `None -> 0`
- `A -> 1`
- `B -> 2`
- `A+B -> 3`
- `C -> 4`
- `A+C -> 5`
- `B+C -> 6`
- `A+B+C -> 7`

**[返回頂部](#configuration)**

* * *

## 相容性映射

Due to JavaScript limitations of being unable to properly serialize simple `ulong` fields in JSON when using web-based ConfigGenerator, `ulong` fields will be rendered as strings with `s_` prefix in the resulting config. This includes for example `"SteamOwnerID": 76561198006963719` that will be written by our ConfigGenerator as `"s_SteamOwnerID": "76561198006963719"`. ASF includes proper logic for handling this string mapping automatically, so `s_` entries in your configs are actually valid and correctly generated. If you're generating configs yourself, we recommend to stick with original `ulong` fields if possible, but if you're unable to do so, you can also follow this scheme and encode them as strings with `s_` prefix added to their names. We hope to resolve this JavaScript limitation eventually.

**[返回頂部](#configuration)**

* * *

## 配置相容性

It's top priority for ASF to remain compatible with older configs. As you should already know, missing config properties are treated the same as they would be defined with their **default values**. Therefore, if new config property gets introduced in new version of ASF, all your configs will remain **compatible** with new version, and ASF will treat that new config property as it'd be defined with its **default value**. You can always add, remove or edit config properties according to your needs. We recommend to limit defined config properties only to those that you want to change, since this way you automatically inherit default values for all other ones, not only keeping your config clean but also increasing compatibility in case we decide to change a default value for property that you don't want to explicitly set yourself (e.g. `WebLimiterDelay`).

**[返回頂部](#configuration)**

* * *

## 自動重載

Starting with ASF V2.1.6.2+, the program is now aware of configs being modified "on-the-fly" - thanks to that, ASF will automatically:

- Create (and start, if needed) new bot instance, when you create its config
- Stop (if needed) and remove old bot instance, when you delete its config
- Stop (and start, if needed) any bot instance, when you edit its config
- Restart (if needed) the bot under new name, when you rename its config

All of the above is transparent and will be done automatically without a need of restarting the program, or killing other (unaffected) bot instances.

In addition to that, ASF will also restart itself (if `AutoRestart` permits) if you modify core ASF `ASF.json` config. Likewise, program will quit if you delete or rename it.

**[返回頂部](#configuration)**