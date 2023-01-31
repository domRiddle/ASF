# 組態設定

本頁面專門用於說明ASF的設定。 這是一份關於&#8203;`config`&#8203;資料夾的完整文件，使您能夠依照您的需求調整ASF。

* **[簡介](#簡介)**
* **[設定檔生成器網頁工具](#設定檔生成器網頁工具)**
* **[ASF-ui 設定](#asf-ui-設定)**
* **[手動設定](#手動設定)**
* **[全域設定檔](#全域設定檔)**
* **[Bot 設定檔](#bot-設定檔)**
* **[檔案結構](#檔案結構)**
* **[JSON 映射](#json-映射)**
* **[相容性映射](#相容性映射)**
* **[設定檔相容性](#設定相容性)**
* **[自動重新載入](#自動重新載入)**

---

## 簡介

ASF的設定分為兩個主要部分⸺全域（程序）設定與每個Bot的設定。 每個Bot都有自己一個名為&#8203;`BotName.json`&#8203;的Bot設定檔（&#8203;`BotName`&#8203;為Bot的名稱），而全域ASF（程序）設定是一個名為&#8203;`ASF.json`&#8203;的檔案。

每個Bot都是一個在ASF程序中執行的獨立Steam帳號。 為了能夠正常運作，ASF需要定義&#8203;**至少一個**&#8203;Bot實例。 程序不會強制限制Bot實例的數量，所以您可以依據您的需求使用任意數量的Bot（Steam帳號）。

ASF使用&#8203;**[JSON](https://zh.wikipedia.org/zh-tw/JSON)**&#8203;格式來儲存自身的設定檔。 這是個人性化、可讀性高且非常通用的格式，您可以在裡面設定程式。 不過不用擔心，您不需要為了設定ASF去專門了解JSON。 我提到它只是考慮到您可能會想要使用一些Bash腳本批次建立大量的ASF設定檔。

您可以經由幾種方式來完成設定。 您可以使用我們的&#8203;**[設定檔生成器網頁工具](https://justarchinet.github.io/ASF-WebConfigGenerator)**&#8203;，這是一個獨立於ASF的本機應用程式。 您也可以使用我們的IPC前端&#8203;**[ASF-ui](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-zh-TW#asf-ui)**&#8203;來直接設定ASF。 最後，您也隨時能夠依照下文指定的固定JSON結構，手動生成設定檔。 我們將簡明地解釋這些可用方式。

---

## 設定檔生成器網頁工具

我們的&#8203;**[設定檔生成器網頁工具](https://justarchinet.github.io/ASF-WebConfigGenerator)**&#8203;的目標是提供您一個用於生成ASF設定檔的友善前端。 設定檔生成器也是基於用戶端的網頁工具，也就是說，您輸入的任何資訊都不會被上傳，而只會在本機中進行處理。 這保證了安全性與可靠性，因為如果您願意下載所有相關檔案，然後在您偏好的瀏覽器中打開&#8203;`index.html`&#8203;，它甚至可以&#8203;**[離線](https://github.com/JustArchiNET/ASF-WebConfigGenerator/tree/main/docs)**&#8203;執行。

設定檔生成器網頁工具在Chrome與Firefox上經過驗證能夠正常執行，且它應該也能在所有支援JavaScript的主流瀏覽器中正常執行。

它的用法非常簡單：切換到適當的分頁來選擇要生成&#8203;`ASF`&#8203;設定檔還是&#8203;`Bot`&#8203;設定檔，確保所選設定檔的版本與您的ASF版本相符，然後輸入所有詳細資訊，並點擊「下載」按鈕。 最後將這個檔案移動到ASF的&#8203;`config`&#8203;資料夾中，如果需要的話，覆蓋掉已經存在的檔案。 若要繼續設定，則重複以上操作，並參考本章節的其他部分來了解所有可設定的選項。

---

## ASF-ui 設定

我們的&#8203;**[ASF-ui](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-zh-TW#asf-ui)**&#8203; IPC介面也允許您設定ASF，且這是在生成初始設定檔後修改設定的最佳方式，因為與設定檔生成器網頁工具總是生成新的檔案不同，這可以在原地直接編輯設定檔。

為了使用ASF-ui，您必須先啟用&#8203;**[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-zh-TW)**&#8203;介面自身。 從ASF V5.1.0.0版本開始，&#8203;`IPC`&#8203;預設為啟用，因此只要您沒有把它停用，就可以直接存取它。

在啟動程式後，直接前往ASF的&#8203;**[IPC位址](http://localhost:1242)**&#8203;。 若一切運作正常，您可以在這裡修改ASF設定。

---

## 手動設定

在一般情形下，我們強烈建議使用我們的設定檔生成器或ASF-ui，因為這樣更簡單，且能確保您不會不小心造成JSON結構錯誤。但如果您出於某些原因不想使用它們，那麼您也可以自行建立正確的設定檔。 參考以下的JSON範例，有個好的開始來了解正確的結構。您可以將內容複製到檔案中，並把它當作設定檔的基礎內容。 由於您沒有使用任何我們的前端，請確保您的設定檔&#8203;**[是有效的](https://jsonlint.com)**&#8203;，因為如果無法剖析，ASF會拒絕載入。 即使它屬於有效的JSON，您也必須確保所有屬性都有正確的型別，就像ASF所要求的。 關於所有可使用欄位正確的JSON結構，請參閱&#8203;**[JSON映射](#json-映射)**&#8203;及我們下方的文件。

---

## 全域設定檔

全域設定位於&#8203;`ASF.json`&#8203;檔案中，具有下列結構：

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
    "FilterBadBots": true,
    "GiftsLimiterDelay": 1,
    "Headless": false,
    "IdleFarmingPeriod": 8,
    "InventoryLimiterDelay": 4,
    "IPC": true,
    "IPCPassword": null,
    "IPCPasswordFormat": 0,
    "LicenseID": null,
    "LoginLimiterDelay": 10,
    "MaxFarmingTime": 10,
    "MaxTradeHoldDuration": 15,
    "MinFarmingDelayAfterBlock": 60,
    "OptimizationMode": 0,
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

---

所有選項的解釋如下：

### `AutoRestart（自動重新啟動）`

`bool`&#8203;型別，預設值為&#8203;`true`&#8203;。 本屬性定義了是否允許ASF在需要時自行重新啟動。 有些事件會要求ASF自行重新啟動，例如ASF更新（經由&#8203;`UpdatePeriod`&#8203;或&#8203;`update`&#8203;指令來達成），編輯&#8203;`ASF.json`&#8203;設定檔、&#8203;`restart`&#8203;指令或類似事件。 通常重新啟動包含兩個部分⸺建立新的程序及結束當前程序。 大多數使用者對此應該沒有問題，且應維持本屬性為預設值&#8203;`true`&#8203;，但是⸺若您正在透過自己的腳本並／或經由&#8203;`dotnet`&#8203;執行ASF，您可能會想完全控制程序並避免某些情形，例如有新（重新啟動）的ASF程序在背景沉靜執行，而不是在腳本前景與舊的ASF程序一起退出。 考慮到新的程序將不再是您原有程序的直接子程序，這一點特別重要，因為這可能會使您無法為其使用標準控制台輸入。

如果有這樣的情形，那麼本屬性就是為您所準備的，您可以把它設定成&#8203;`false`&#8203;。 但是，請注意，在這種情形下&#8203;**您**&#8203;需要負責自行重新啟動程序。 這在某種程度上很重要，因為ASF將只會退出，而不會生成新程序（例如在更新後）。因此，若您沒有加入任何邏輯，它將會停止運作直到您再次啟動它。 ASF總是會以正確的錯誤碼退出，指示出成功（零）或不成功（非零）。這樣您就能在腳本中加入正確的邏輯，以避免在故障時自動重新啟動，或至少製作一份本機副本&#8203;`log.txt`&#8203;來提供進一步分析。 也請注意，不論如何設定本屬性，&#8203;`restart`&#8203;指令總是會重新啟動ASF，因為此屬性只用於定義預設行為，而&#8203;`restart`&#8203;指令總能重新啟動程序。 除非您有停用此功能的理由，否則您應維持啟用它。

---

### `Blacklist（黑名單）`

`ImmutableHashSet<uint>`&#8203;型別，預設值為空。 顧名思義，本全域設定屬性定義了ASF自動掛卡程序要完全忽略的AppID（遊戲）。 但不幸的是，Steam喜歡將夏季／冬季特賣徽章標記成「可掉落的交換卡片」，這誤導了ASF程序，相信這是一個能掛卡的有效遊戲。 若沒有任何形式的黑名單，ASF最終會「掛」在掛卡一個實際上並非遊戲的遊戲上，並無限等待不存在的卡片掉落。 ASF黑名單的目的是將這些徽章標記成無法掛卡，因此我們可以在掛卡時直接忽略它們，而不是落入Bug陷阱。

ASF包含兩個預設的黑名單：&#8203;`SalesBlacklist`&#8203;硬編碼於ASF的程式碼中，且無法編輯；一般的&#8203;`Blacklist`&#8203;則是在本處定義。 `SalesBlacklist`&#8203;與ASF版本一同更新，且通常會包含在發布時已知的所有「不良」AppID，因此如果您使用最新版的ASF，就不需要自行管理這裡定義的&#8203;`Blacklist`&#8203;。 本屬性的主要目的是允許您將ASF發布時未知的新AppID加入黑名單，它們不應被掛卡。 硬編碼的&#8203;`SalesBlacklist`&#8203;都會在第一時間更新，因此如果您使用最新版的ASF，就不需要自行更新您的&#8203;`Blacklist`&#8203;；但如果沒有&#8203;`Blacklist`&#8203;，您就被迫強制更新ASF以在Valve釋出新的特賣徽章時能「正常執行」⸺我並不想強制您使用最新版的ASF程式碼，因此假如您因為某些原因，或無法在新的ASF中更新硬編碼的&#8203;`SalesBlacklist`&#8203;，但您又想要讓舊的ASF維持運作，本屬性允許您自行「修復」ASF。 除非您有&#8203;**充分的**&#8203;理由編輯此屬性，否則您應維持它為預設值。

若您正尋找基於Bot的黑名單，請查看&#8203;`fb`&#8203;、&#8203;`fbadd`&#8203;及&#8203;`fbrm`&#8203;**[指令](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-TW)**&#8203;。

---

### `CommandPrefix（指令前綴）`

`string`&#8203;型別，預設值為&#8203;`!`&#8203;。 本屬性指定用於ASF&#8203;**[指令](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-TW)**&#8203;**區分大小寫**&#8203;的前綴。 也就是說，這是您要在每個ASF指令前加上這個，ASF才會聽從您的指令。 您也可以將此值設定成&#8203;`null`&#8203;或留空，來讓ASF不使用前綴，在這種情形下，您可以直接輸入帶有一般識別碼的指令。 但是，這樣做會降低ASF的效能，因為ASF經過最佳化，如果訊息不是以&#8203;`CommandPrefix`&#8203;開頭，就不會進一步剖析它⸺若您決定不使用前綴，ASF將會被迫讀取並回應所有訊息，即使它們並不是ASF指令。 因此，若您不喜歡預設值&#8203;`!`&#8203;，仍建議繼續使用&#8203;`CommandPrefix`&#8203;，如&#8203;`/`&#8203;。 為了保持一致，&#8203;`CommandPrefix`&#8203;會影響整個ASF程序。 除非您有理由編輯此屬性，否則您應維持它為預設值。

---

### `ConfirmationsLimiterDelay（確認請求延時）`

`byte`&#8203;型別，預設值為&#8203;`10`&#8203;。 ASF會確保在兩次連續的雙重驗證確認提取請求間至少間隔&#8203;`ConfirmationsLimiterDelay`&#8203;秒，以避免觸發速率限制⸺這被使用在&#8203;**[ASF雙重驗證](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-zh-TW)**&#8203;上，例如&#8203;`2faok`&#8203;指令，以及在一些與交易相關的操作上面。 預設值是依據我們的測試結果所訂定，如果您不想遇到問題，請勿減少此值。 除非您有&#8203;**充分的**&#8203;理由編輯此屬性，否則您應維持它為預設值。

---

### `ConnectionTimeout（連線逾時時間）`

`byte`&#8203;型別，預設值為&#8203;`90`&#8203;。 本屬性定義了ASF執行各種網路操作的逾時時間，以秒為單位。 特別是，&#8203;`ConnectionTimeout`&#8203;定義了HTTP及IPC請求的逾時秒數，&#8203;`ConnectionTimeout / 10`&#8203;定義了心跳機制的最大失敗次數，而&#8203;`ConnectionTimeout / 30`&#8203;定義了我們允許初始Steam網路連線請求的分鐘數。 對於大多數人來說，預設值&#8203;`90`&#8203;應該是適合的，但是如果您的網路連線或PC速度較慢，您可能會希望增加此值（像是&#8203;`120`&#8203;）。 請注意，即使是更大的值也無法神奇地修復緩慢或甚至是無法存取的Steam伺服器，所以我們不應該無限等待不會發生的事情，而是稍後再試。 將此值設定過高會導致在抓取網路問題時出現過多延遲，並降低整體效能。 將此值設定過低會降低整體的穩定性及效能，因為ASF會在尚在剖析時中止有效的請求。 因此，在一般情形下，將此值設定成比預設還要低並沒有任何優點，因為Steam伺服器常常會變得異常緩慢，且可能會需要更多時間來剖析ASF請求。 預設值是相信我們的網路連線穩定，及懷疑Steam網路能在逾時前處理我們的請求之間的平衡。 若您想盡快發現問題，並使ASF更快重新連線／做出回應，預設值就足夠（或稍微低於，像是&#8203;`60`&#8203;，使ASF不用等那麼久）。 與之相反，若您發現ASF遇到了網路問題，例如請求失敗、遺失心跳包或與Steam連線中斷，如果您確定這&#8203;**並非**&#8203;由您的網路所引起的，而是Steam自身，則增加此值就具意義，因為增加逾時時間可以讓ASF有更多「耐心」，來不會決定要立刻重新連線。

可能需要增加此屬性的一個範例情形是讓ASF處理極大量的交易提案，Steam可能需要2分鐘以上才能完全接受並處理這些提案。 透過增加預設的逾時時間，ASF會更有耐心並等待更長時間，然後才會決定不完成交易並放棄初始請求。

另一種情形可能是設備或網路連線非常緩慢，需要更多的時間來處理正在傳輸的資料。 這是個非常罕見的情形，因為CPU／網路頻寬基本上不會是個瓶頸，但仍是個值得一提的可能性。

簡而言之，在大多數情形下預設值是適合的，但如果需要，您可能會想要增加它。 不過遠高於預設值也沒有任何意義，因為更多的逾時時間也無法神奇地修復無法存取的Steam伺服器。 除非您有理由編輯此屬性，否則您應維持它為預設值。

---

### `CurrentCulture（所在語言地區）`

`string`&#8203;型別，預設值為&#8203;`null`&#8203;。 預設情形下，ASF會嘗試使用您的作業系統語言，且如果可以，會優先使用該語言的翻譯字串。 這要感謝我們的社群，他們致力於將ASF&#8203;**[在地化](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Localization-zh-TW)**&#8203;成各類主流語言。 若出於某種原因，您不想使用您的作業系統原生語言，您可以使用此設定屬性來選擇任意一種您想使用的有效語言。 如需所有可以使用的語言，請造訪&#8203;**[MSDN](https://msdn.microsoft.com/en-us/library/cc233982.aspx)**&#8203;來查詢&#8203;`語言編碼`&#8203;。 值得一提的是，ASF接受包含特定國家或地區的編碼，例如&#8203;`"en-GB"`&#8203;，也接受通用編碼，例如&#8203;`"en"`&#8203;。 指定當前語系還可能會影響與地區相關的行為，例如貨幣／日期格式等。 請注意，若您選擇非本機原生語言，您可能會需要額外的字型／語言套件才能正確顯示特定語言的字元。 通常在您偏好以英文而非您的母語使用ASF時，會需要使用這個設定屬性。

---

### `Debug（除錯模式）`

`bool`&#8203;型別，預設值為&#8203;`false`&#8203;。 本屬性定義了程序是否以除錯模式執行。 在除錯模式中，ASF會在&#8203;`config`&#8203;旁建立一個特殊的&#8203;`debug`&#8203;資料夾，用於追蹤ASF與Steam伺服器間的所有通訊。 除錯資訊有助於發現與網路及一般ASF工作流程相關的棘手問題。 除此之外，某些程式常式會更加詳細，例如&#8203;`WebBrowser`&#8203;會說明某些請求失敗的確切原因⸺這些條目會被寫入至一般的ASF紀錄日誌中。 **除非開發人員要求，否則您不應在除錯模式下執行ASF**&#8203;。 以除錯模式執行ASF會&#8203;**降低效能**&#8203;、&#8203;**減少穩定性**&#8203;，且會&#8203;**生成過量除錯訊息**&#8203;，因此&#8203;**只應**&#8203;在需要時短暫使用，用於除錯特定問題、重現錯誤或獲得關於失敗請求的更多資訊，但&#8203;**不應**&#8203;用於一般的程式執行階段。 您將會看到&#8203;**非常大量的**&#8203;新錯誤、問題及異常狀況⸺若您決定自行分析這些資訊，請確保您對ASF、Steam及其特點有著充分的了解，因為並不是所有資訊皆與問題相關。

**警告：**&#8203;啟用本模式會在紀錄日誌中記錄&#8203;**可能敏感**&#8203;的資訊，例如您登入至Steam的帳號及密碼（因網路日誌所記錄）。 這些資料會同時寫入至&#8203;`debug`&#8203;資料夾及標準的&#8203;`log.txt`&#8203;（現在是故意詳細記錄本資訊）中。 您不應在任何公開位置張貼ASF生成的除錯內容，且ASF開發人員總是會提醒您應經由電子郵件或其他安全位置傳送它。 我們不會儲存或利用這些敏感資訊，它們只是做為除錯常式的一部份而寫入，因為它們或許會與您遇到的問題有關。 我們希望您不以任何方式修改ASF紀錄日誌，但如果您擔心，您還是依然能適當編輯這些敏感資訊。

> 您可以使用取代的方式替換掉敏感資訊，例如使用星號。 但您需要避免完全刪除包含敏感資訊的資訊行，因為它們可能與問題有關，應予以保留。

---

### `FarmingDelay（掛卡延時）`

`byte`&#8203;型別，預設值為&#8203;`15`&#8203;。 ASF會在正常運作時每隔&#8203;`FarmingDelay`&#8203;分鐘檢查一次當前掛卡的遊戲是否已掉落所有卡片。 將此屬性設定過低可能會傳送過多的Steam請求，而設定過高可能會使ASF在掛卡完成後仍「掛著」指定遊戲，直到最多滿&#8203;`FarmingDelay`&#8203;分鐘。 預設值應該對於大多數使用者來說是適合的，但如果您有很多個Bot在執行，則可以考慮增加至例如&#8203;`30`&#8203;分鐘，以限制傳送的Steam請求。 值得一提的是，ASF使用基於事件的機制，並在每個Steam物品掉落時檢查遊戲的徽章頁面，所以在一般情形下，&#8203;**我們甚至能不必每隔固定時間去檢查**&#8203;，但由於我們並不完全信任Steam網路⸺如果我們無法在&#8203;`FarmingDelay`&#8203;分鐘內未能檢查卡片是否掉落，我們仍會去檢查遊戲的徽章頁面。 假設Steam網路運作正常，降低此值&#8203;**不會以任何方式提高掛卡效率**&#8203;，而且&#8203;**還會顯著增加網路負擔**&#8203;⸺建議維持預設值&#8203;`15`&#8203;分鐘，並只在需要時才增加它。 除非您有&#8203;**充分的**&#8203;理由編輯此屬性，否則您應維持它為預設值。

---

### `FilterBadBots（過濾惡意Bot）`

`bool`&#8203;型別，預設值為&#8203;`true`&#8203;。 本屬性定義了ASF是否會自動拒絕已知被標示成惡劣行為者的交易提案。 為此，ASF會依據需要與我們的伺服器通訊，以提取Steam ID黑名單清單。 列出的那些Bot，是我們歸類為對ASF有害的人所操作的，例如那些違反我們的&#8203;**[行為準則](https://github.com/JustArchiNET/ArchiSteamFarm/blob/main/.github/CODE_OF_CONDUCT.md)**&#8203;、使用我們提供的功能及資源例如&#8203;**[`PublicListing`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/ItemsMatcherPlugin-zh-TW#publiclisting)**&#8203;來濫用或利用他人，或是從事直接犯罪行為例如對伺服器發動DDoS攻擊的人。 因為ASF對於使用者之間的整體公平、誠信及合作方面有堅定的立場，以使得整體社群蓬勃發展，因此預設情形下此屬性為啟用，ASF會篩選掉歸類為對我們提供的服務有害的Bot。 除非您有&#8203;**充分的**&#8203;理由編輯此屬性，例如不同意我們的聲明，並故意允許這些Bot進行操作（包含利用您的帳號），否則您應維持它為預設值。

---

### `GiftsLimiterDelay（請求限制延時）`

`byte`&#8203;型別，預設值為&#8203;`1`&#8203;。 在兩個連續的贈禮／序號／授權處理（兌換）請求間，ASF會確保至少間隔&#8203;`GiftsLimiterDelay`&#8203;秒，以避免觸發速率限制。 除此之外，這也用於遊戲清單請求的全域限制器，例如由&#8203;`owns`&#8203;**[指令](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-TW)**&#8203;所發出的請求。 除非您有&#8203;**充分的**&#8203;理由編輯此屬性，否則您應維持它為預設值。

---

### `Headless（無頭模式）`

`bool`&#8203;型別，預設值為&#8203;`false`&#8203;。 本屬性定義了程序是否以無頭模式執行。 在無頭模式中，ASF會假定它在伺服器或其他非互動式環境下執行，因此它不會嘗試在控制台輸入中讀取任何資訊。 這包含了隨選詳細資料（帳號憑證，例如雙重驗證代碼、Steam Guard代碼、密碼，或ASF操作所需的其他任何變數），及其他所有控制台輸入（例如互動式指令控制台）。 也就是說，&#8203;`Headless`&#8203;模式等同於將ASF控制台變成唯讀。 本設定主要給在伺服器上執行ASF的使用者使用，在需要使用者輸入（例如雙重驗證代碼）時，ASF將會直接停止使用該帳號以中止輸入操作。 除非您在伺服器上執行ASF，且您先前已確認ASF能夠在非無頭模式中執行，否則您應維持停用它。 在無頭模式下，任何使用者互動行為皆會被拒絕，如果您的帳號在啟動過程中需要&#8203;**任何**&#8203;控制台輸入，則帳號不會啟動。 這對伺服器來說非常有用，因為ASF可以在要求提供憑證時嘗試中止帳號登入，而不是（無限）等待使用者提供這些憑證。 啟用此模式也會允許您能夠使用&#8203;`input`&#8203;**[指令](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-TW)**&#8203;，來作為標準控制台輸入的替代方式。 若您不確定如何設定本屬性，請保留預設值&#8203;`false`&#8203;。

若您在伺服器上執行ASF，您可能會需要將本選項與&#8203;`--process-required`&#8203;**[命令列引數](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments-zh-TW)**&#8203;一起配合使用。

---

### `IdleFarmingPeriod（掛卡檢查週期）`

`byte`&#8203;型別，預設值為&#8203;`8`&#8203;。 當ASF沒有東西掛卡時，它會每隔&#8203;`IdleFarmingPeriod`&#8203;小時檢查帳號內是否擁有新遊戲可供掛卡。 在我們獲得新遊戲時，並不需要此功能，因為ASF足夠智慧，可以在這種情形中自動檢查徽章頁面。 `IdleFarmingPeriod`&#8203;主要是針對我們在帳號中已有的遊戲新加入了交換卡片功能的情形。 在這種情形下不會產生事件，因此如果我們要知道這件事，ASF就必須定期檢查徽章頁面。 將值設定為&#8203;`0`&#8203;時會停用本功能。 也請注意：&#8203;`ShutdownOnFarmingFinished`&#8203;。

---

### `InventoryLimiterDelay（物品庫限制延時）`

`byte`&#8203;型別，預設值為&#8203;`4`&#8203;。 ASF會確保在兩次連續的物品庫請求間至少間隔&#8203;`InventoryLimiterDelay`&#8203;秒，以避免觸發速率限制⸺這被使用在提取Steam物品庫時，特別是在您自己的指令中，例如&#8203;`loot`&#8203;或&#8203;`transfer`&#8203;。 預設值&#8203;`4`&#8203;是依據連續提取超過100個Bot實例的物品庫所設定的，應該能滿足大部分（可能不是全部）的使用者需求。 但是，如果您的Bot數量很少，您可能會想要減少它，或甚至把它更改成&#8203;`0`&#8203;，使ASF忽略延時並加快獲得Steam物品庫。 但請注意，設定過低的值&#8203;**將會**&#8203;導致Steam暫時封鎖您的IP，並完全阻止您提取您的物品庫。 若您執行了很多需要大量物品庫請求的Bot，您可能還會需要增加此值，但在這種情形下，您可能更應該去限制這些請求的數量。 除非您有&#8203;**充分的**&#8203;理由編輯此屬性，否則您應維持它為預設值。

---

### `IPC（行程間通訊）`

`bool`&#8203;型別，預設值為&#8203;`true`&#8203;。 本屬性定義了ASF的&#8203;**[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-zh-TW)**&#8203;伺服器是否與主程序一同啟動。 IPC允許行程間通訊，包含透過啟動本機HTTP伺服器使用&#8203;**[ASF-ui](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-zh-TW#asf-ui)**&#8203;。 若您不打算使用任何ASF的第三方IPC整合工具，包含我們的ASF-ui的話，您就可以安全地停用這個選項。 否則，最好保持啟用狀態（預設狀態）。

---

### `IPCPassword（IPC 密碼）`

`string`&#8203;型別，預設值為&#8203;`null`&#8203;。 本屬性定義了每個經由IPC執行的API呼叫的強制性密碼，作為一項額外的安全措施。 當設定成非空值時，所有的IPC請求都會需要額外的&#8203;`password`&#8203;屬性，為此處指定的密碼。 預設值&#8203;`null`&#8203;將會跳過要求密碼，並使ASF接受所有請求。 除此之外，啟用此選項也會啟用內建的IPC反暴力破解機制，一個&#8203;`IPAddress`&#8203;在非常短的時間內傳送過多未授權的請求時，將會被暫時封鎖。 除非您有理由編輯此屬性，否則您應維持它為預設值。

---

### `IPCPasswordFormat（IPC 密碼格式）`

`byte`&#8203;型別，預設值為&#8203;`0`&#8203;。 本屬性定義了&#8203;`IPCPassword`&#8203;屬性的格式，使用&#8203;`EHashingMethod`&#8203;作為基本類型。 若需了解更多，請參閱&#8203;**[安全性](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security-zh-TW)**&#8203;章節，因為您需要確保&#8203;`IPCPassword`&#8203;屬性確實包含匹配`IPCPasswordFormat`&#8203;的密碼。 也就是說，在您更改&#8203;`IPCPasswordFormat`&#8203;的時候，您的&#8203;`IPCPassword`&#8203;就必須&#8203;**已經**&#8203;是您所選的格式了，而不是在更改完成後才是。 除非您知道您在做什麼，否則請保留預設值&#8203;`0`&#8203;。

---

### `LicenseID（授權 ID）`

`Guid?`&#8203;型別，預設值為&#8203;`null`&#8203;。 本屬性允許我們的&#8203;**[贊助者](https://github.com/sponsors/JustArchi)**&#8203;使用付費的選擇性功能來增強ASF的運作能力。 在目前，這允許您使用&#8203;`ItemsMatcher`&#8203;外掛程式中的&#8203;**[`MatchActively`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/ItemsMatcherPlugin-zh-TW#matchactively)**&#8203;功能。

若您是ASF贊助者，您可以在&#8203;**[這裡](https://asf.justarchi.net/User/Status)**&#8203;獲得您的授權碼。 您會需要以GitHub登入來確認您的身分，我們只會請求您的公開唯讀資訊，也就是您的使用者名稱。 `LicenseID`&#8203;由32個十六進制字元組成，例如&#8203;`f6a0529813f74d119982eb4fe43a9a24`&#8203;。

**請確保您沒有與其他人共用您的&#8203;`LicenseID`**&#8203;。 因為它是給個人使用的，如果洩漏出去就有可能被撤銷。 如果不幸您意外洩漏了授權碼，您可以在相同地方生成一個新的。

除非您想要啟用ASF的額外功能，否則您沒有必要在此提供授權碼。

---

### `LoginLimiterDelay（登入限制延時）`

`byte`&#8203;型別，預設值為&#8203;`10`&#8203;。 在兩個連續的連線嘗試間，ASF會確保至少間隔&#8203;`LoginLimiterDelay`&#8203;秒，以避免觸發速率限制。 預設值&#8203;`10`&#8203;是依據超過100個Bot實例的連線所設定的，應該能滿足大部分（可能不是全部）的使用者需求。 但是，如果您的Bot數量很少，您可能會想要增加／減少它，或甚至把更改成&#8203;`0`&#8203;，使ASF忽略延時並加快連線至Steam。 但請注意，在有很多Bot的情形下&#8203;**將會**&#8203;導致Steam暫時封鎖您的IP，觸發&#8203;`InvalidPassword/RateLimitExceeded`&#8203;錯誤，並&#8203;**完全**&#8203;阻止您登入⸺不只是在ASF，也包含您的Steam用戶端那邊。 同理，如果您執行大量Bot，特別是在同一IP位址內一起使用其他Steam用戶端／工具，您可能還會需要增加此值，以將登入請求分散至更長的時間段。

另外說明一下，此值還會用於所有ASF計畫任務的附載平衡緩衝，例如在&#8203;`SendTradePeriod`&#8203;中的交易。 除非您有&#8203;**充分的**&#8203;理由編輯此屬性，否則您應維持它為預設值。

---

### `MaxFarmingTime（掛卡最大時數）`

`byte`&#8203;型別，預設值為&#8203;`10`&#8203;。 如您所知，Steam並不總是能正常運作，有時候會發生奇怪的狀況，例如雖然我們正在遊玩，但沒有到紀錄我們的遊玩時數上。 ASF允許一個遊戲在單一遊戲模式下最多掛卡&#8203;`MaxFarmingTime`&#8203;小時，並在此之後認為該遊戲已完成掛卡。 這是為了在發生怪異情形時不會使掛卡程序卡住所必需的，也可能Steam因為某種原因釋出了一個新的徽章，而使ASF卡住掛卡進度（參見：&#8203;`Blacklist`&#8203;）。 預設值&#8203;`10`&#8203;小時應該足以使一個遊戲掉落全部的交換卡片。 將此屬性設定過低可能會使ASF跳過有效的遊戲（是的，有些遊戲甚至需要長達9小時卡片才能全部掉完），設定過高則可能會導致掛卡程序卡住過久。 請注意，本屬性只會影響單次掛卡期間的單一遊戲（在完成整個佇列後，ASF將會重新計時），此外，此屬性並非依據遊玩時數，而是ASF內部的掛卡時數，因此ASF也會在重新啟動後重新計時。 除非您有&#8203;**充分的**&#8203;理由編輯此屬性，否則您應維持它為預設值。

---

### `MaxTradeHoldDuration（交易託管最大期間）`

`byte`&#8203;型別，預設值為&#8203;`15`&#8203;。 本屬性定義了我們願意接受的交易託管最大天數⸺ASF會拒絕保留超過&#8203;`MaxTradeHoldDuration`&#8203;天的交易，如&#8203;**[交易](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading-zh-TW)**&#8203;章節所述。 此選項只會作用於在&#8203;`TradingPreferences`&#8203;中啟用了&#8203;`SteamTradeMatcher`&#8203;的Bot，而不會影響來自&#8203;`Master`&#8203;／&#8203;`SteamOwnerID`&#8203;的交易，也不影響贈禮。 對每個人來說，交易託管是非常惱人的，沒有人想被它打擾。 ASF應在自由的原則下幫助每個人使用，不論是否有交易託管⸺這就是為什麼本選項的預設值為&#8203;`15`&#8203;。 但是，如果您想要拒絕所有具交易託管的交易，您可以在此處設定成&#8203;`0`&#8203;。 請注意以下說明：有時間限制的交換卡片不受此選項影響，且ASF會自動拒絕具有交易託管的交易，如&#8203;**[交易](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading-zh-TW)**&#8203;章節所述，所以沒有必要只因為這個原因而拒絕所有人。 除非您有理由編輯此屬性，否則您應維持它為預設值。


---

### `MinFarmingDelayAfterBlock（停止後重新掛卡最小延時）`

`byte`&#8203;型別，預設值為&#8203;`60`&#8203;。 本屬性定義了以秒為單位的最小時間，如果ASF先前因為&#8203;`LoggedInElsewhere`&#8203;而失去連線，則在等待這段時間後才會再次恢復掛卡：如果您決定以執行遊戲來強制使當前掛卡的ASF斷線，就會產生這種情形。 這種延遲存在的原因主要是為了方便並節省負擔，例如它使您能夠重新啟動遊戲，不必因為那一兩秒鐘的空檔，去跟ASF爭奪帳號的使用權。 由於重新獲得連線階段會使&#8203;`LoggedInElsewhere`&#8203;失去連線，ASF就必須執行整個重新連線的流程，這會使設備及Steam網路增加額外的壓力，因此能夠的話，避免掉所有不必要的斷線。 預設情形下，本屬性設定成&#8203;`60`&#8203;秒，這應該足以讓您輕鬆重新啟動遊戲了。 但是，在某些情境中您可能會希望增加此值，例如您的網路經常失去連線，且ASF會太快接管，這會導致您被迫自行重新連線。 此屬性的最大允許值為&#8203;`255`&#8203;，這應該足以應付所有常見情境。 除了上述情形外，您也可以減少甚至設定成&#8203;`0`&#8203;來完全取消本延遲，但因上述原因，我們通常不建議這樣做。 除非您有理由編輯此屬性，否則您應維持它為預設值。

---

### `OptimizationMode（最佳化模式）`

`byte`&#8203;型別，預設值為&#8203;`0`&#8203;。 本屬性定義了ASF在執行期間偏好的最佳化模式。 目前，ASF支援兩種模式：&#8203;`0`&#8203;為&#8203;`MaxPerformance`&#8203;；&#8203;`1`&#8203;為&#8203;`MinMemoryUsage`&#8203;。 預設情形下，ASF會盡可能地平行（並行）執行越多的工作，以跨CPU核心、多個CPU執行緒、多個網路插座及多個執行緒集區工作來增強效能。 舉例來說，ASF會在檢查需要掛卡的時查詢您的第一頁徽章頁面，在請求完成後，ASF會從中讀取您實際的徽章頁數，然後同時向所有剩餘頁面傳送請求。 這&#8203;**基本上**&#8203;就是您所想要的，因為在大多數情形下負擔是最小的，即使在單個CPU核心和非常有限功率的老舊硬體上，也能看到ASF異步程式碼的好處。 但是，由於許多工作是平行處理的，因此ASF需要在執行期間負責維護它們，例如維持網路插座開啟、執行緒處於活動狀態及工作有被處理，這可能會導致記憶體使用量不時增加。若您的可用記憶體嚴重受限，您可能會想要切換本屬性為&#8203;`1`&#8203;（&#8203;`MinMemoryUsage`&#8203;）來強制ASF處理盡可能少的工作，並盡量以同步方式執行可平行處理的異步程式碼。 只有在您已詳閱&#8203;**[低記憶體設定](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup-zh-TW)**&#8203;後，且決定想要犧牲大量效能以獲得節省極少量的記憶體負擔時，才應考慮開啟此屬性。 通常本選項比您能夠使用的其他方式達成的成效還來得&#8203;**差得多**&#8203;，例如經由限制您的ASF使用，或調整執行環境的垃圾收集器，如&#8203;**[低記憶體設定](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup-zh-TW)**&#8203;所述。 因此，如果您無法經由其他（更好的）方式獲得滿意的結果，則應將&#8203;`MinMemoryUsage`&#8203;作為重新編譯執行環境前的&#8203;**最後手段**&#8203;。 除非您有&#8203;**充分的**&#8203;理由編輯此屬性，否則您應維持它為預設值。

---

### `SteamMessagePrefix（Steam 訊息前綴）`

`string`&#8203;型別，預設值為&#8203;`"/me "`&#8203;。 本屬性定義了加入至ASF送出的所有Steam訊息最開頭的前綴。 預設情形下，ASF使用&#8203;`"/me "`&#8203;為前綴，使Steam聊天以不同顏色顯示Bot訊息，以便更容易區分它們。 另一個值得提及的前綴是&#8203;`"/pre "`&#8203;，它有相似的結果，但有著不同的格式。 您也可以將本屬性設定成空值或&#8203;`null`&#8203;來完全停用前綴，並以傳統的方式輸出所有ASF訊息。 值得一提的是，本屬性只會影響Steam訊息⸺以其他通道（例如IPC）回傳的回應不會受到影響。 除非您想要自訂標準的ASF行為，否則最好保留預設值。

---

### `SteamOwnerID（擁有者的 Steam ID）`

`ulong`&#8203;型別，預設值為&#8203;`0`&#8203;。 本屬性定義了ASF程序擁有者的64位元Steam ID，與指定Bot實例的&#8203;`Master`&#8203;權限非常相似（但作用於全域）。 基本上您會想要將本屬性設定成您自己的Steam主帳號的ID。 `Master`&#8203;權限包含對該Bot實例的完全控制，但是全域指令例如&#8203;`exit`&#8203;、&#8203;`restart`&#8203;或&#8203;`update`&#8203;則只能由&#8203;`SteamOwnerID`&#8203;使用。 這很方便，因為您可能想要為您的朋友執行Bot，但同時不允許他們控制ASF程序，例如使用&#8203;`exit`&#8203;指令退出程序。 預設值&#8203;`0`&#8203;代表ASF程序沒有擁有者，這代表沒有人能使用全域ASF指令。 請注意此屬性只適用於Steam聊天，即使沒有設定本屬性，&#8203;**[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-zh-TW)**&#8203;與互動式控制台仍會允許您執行&#8203;`Owner`&#8203;指令。

---

### `SteamProtocols（Steam 網際網路協定）`

`byte flags`&#8203;型別，預設值為&#8203;`7`&#8203;。 本屬性定義了ASF在連線Steam伺服器時使用的Steam網際網路協定，其定義如下：

| 值 | 名稱        | 描述                                                                                                             |
| - | --------- | -------------------------------------------------------------------------------------------------------------- |
| 0 | 無         | 無協定                                                                                                            |
| 1 | TCP       | **[傳輸控制協定](https://zh.wikipedia.org/zh-tw/%E4%BC%A0%E8%BE%93%E6%8E%A7%E5%88%B6%E5%8D%8F%E8%AE%AE)**            |
| 2 | UDP       | **[使用者資料報協定](https://zh.wikipedia.org/zh-tw/%E7%94%A8%E6%88%B7%E6%95%B0%E6%8D%AE%E6%8A%A5%E5%8D%8F%E8%AE%AE)** |
| 4 | WebSocket | **[WebSocket](https://zh.wikipedia.org/zh-tw/WebSocket)**                                                      |

請注意，本屬性為&#8203;`flags`&#8203;欄位，因此可以使用所有可用值任意組合。 若您想了解更多，請參閱&#8203;**[旗標映射](#json-映射)**&#8203;。 不啟用任何旗標即為&#8203;`None`&#8203;選項，且該選項本身無效。

預設情形下，ASF會使用所有可以使用的Steam協定，作為應對當機期間及其他相似的Steam問題的措施。 通常若您想限制ASF使用一個或兩個指定的協定，而不是所有可用的協定時，您會想要修改此屬性。 例如若您在防火牆中啟用只啟用了TCP的流量，且您不希望ASF嘗試透過UDP連線，就可能會需要這樣的措施。 但是，除非您在除錯特定問題，否則您基本上會希望確保ASF能夠自由使用當前支援的所有協定，而不是只有一兩個。 除非您有&#8203;**充分的**&#8203;理由編輯此屬性，否則您應維持它為預設值。

---

### `UpdateChannel（更新通道）`

`byte`&#8203;型別，預設值為&#8203;`1`&#8203;。 本屬性定義了ASF使用的更新通道，用於自動更新（如果&#8203;`UpdatePeriod`&#8203;大於&#8203;`0`&#8203;時），或更新通知（&#8203;`UpdatePeriod`&#8203;為&#8203;`0`&#8203;時）。 目前ASF支援三個更新通道：&#8203;`0`&#8203;為&#8203;`None`&#8203;；&#8203;`1`&#8203;為&#8203;`Stable`&#8203;；而&#8203;`2`&#8203;為&#8203;`Experimental`&#8203;。 `Stable`&#8203;通道是預設的發布通道，這適合大多數的使用者。 `Experimental`&#8203;除了&#8203;`Stable`&#8203;版本以外，也包含了專門提供進階使用者及其他開發人員測試新功能、確認錯誤修復或提供相關增強計畫回饋的&#8203;**預覽版本**&#8203;。 **實驗性版本通常包含了尚未修補的錯誤、未完成或重寫的功能**&#8203;。 若您任為您並非進階使用者，請保留預設值&#8203;`1`&#8203;（穩定的）更新通道。 `Experimental`&#8203;通道是專門提供給知道如何回報錯誤、處理問題並給予回饋的使用者所使用⸺我們不會提供任何技術支援。 若您想了解更多，請參閱&#8203;**[發布週期](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle-zh-TW)**&#8203;。 若您想完全停用所有版本檢查，您也可以將&#8203;`UpdateChannel`&#8203;設定成&#8203;`0`&#8203;（&#8203;`None`&#8203;）。 將&#8203;`UpdateChannel`&#8203;設定成&#8203;`0`&#8203;會完全停用與更新相關的功能，包含&#8203;`update`&#8203;指令。 **強烈反對**&#8203;您使用&#8203;`None`&#8203;通道，因為這會使您自己面臨各種問題（在下文&#8203;`UpdatePeriod`&#8203;的說明中有提到）。

**除非您知道您在做什麼**，否則我們&#8203;**強烈**&#8203;建議維持它為預設值。

---

### `UpdatePeriod（更新週期）`

`ushort`&#8203;型別，預設值為&#8203;`300`&#8203;。 本屬性定義了ASF每隔多久會檢查一次自動更新。 更新是非常重要的，不僅能獲得新功能及重要安全補丁，也能修復錯誤、獲得效能增加及提高穩定性等。 當設定的值大於&#8203;`0`&#8203;時，ASF會在有新版本可供使用的時候自動下載、取代並重新啟動自身（如果&#8203;`AutoRestart`&#8203;允許）。 為了達到上述功能，ASF會每&#8203;`UpdatePeriod`&#8203;小時檢查一次我們的GitHub儲存庫上是否有新版本可供使用。 值為&#8203;`0`&#8203;會停用自動更新，但您仍能夠手動執行&#8203;`update`&#8203;指令。 您可能還會對&#8203;`UpdatePeriod`&#8203;需要設定的正確的&#8203;`UpdateChannel`&#8203;感興趣。

ASF的更新過程涉及到更新整個ASF所使用的資料夾結構，但不會動到位於&#8203;`config`&#8203;資料夾中您自己的設定檔或資料庫⸺這代表在ASF資料夾中的所有無關檔案都會在更新過程中遺失。 預設值&#8203;`24`&#8203;能在不必要的檢查與維持ASF足夠新之間取得不錯的平衡。

除非您有&#8203;**充分的**&#8203;理由停用此屬性，否則您應維持使用合理的&#8203;`UpdatePeriod`&#8203;來啟用自動更新，&#8203;**這是為了你好**&#8203;。 這不只是因為我們只支援最新的穩定版ASF，也因為&#8203;**我們只對最新版本提供安全保證**&#8203;。 若您使用過時的ASF版本，您就是故意讓您自己暴露於各種問題下，從小型錯誤、損壞的功能，到&#8203;**[Steam帳號永久停權](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ-zh-TW#有人因為它被封鎖嗎)**&#8203;，所以我們&#8203;**強烈建議**&#8203;，為了你好，請始終確保您的ASF版本是最新的。 自動更新使我們能夠對任何問題快速反應，在造成後果之前能停用或修補有問題的程式碼⸺若您選擇停用，您會失去我們所有的安全保證，並需承擔執行可能的有害程式碼的風險後果，不只是對Steam網路，也包含您自己的Steam帳號。

---

### `WebLimiterDelay（網路限制延時）`

`ushort`&#8203;型別，預設值為&#8203;`300`&#8203;。 本屬性定義了向同一個Steam Web服務傳送兩個連續請求間的最小延遲，以毫秒為單位。 這種延遲是必需的，因為Steam內部使用的&#8203;**[AkamaiGhost](https://www.akamai.com)**&#8203;服務包含了依據指定時間段內傳送請求總數的速率限制。 在正常情形下很難處發Akamai的限制，但如果我們在極短的時間內持續傳送過多的請求，導致異常繁重的工作負載及大量的持續佇列請求，就有可能觸發它。

預設值是依據假設ASF是存取Steam Web服務的唯一工具所設定的，特別是&#8203;`steamcommunity.com`&#8203;、&#8203;`api.steampowered.com`&#8203;及&#8203;`store.steampowered.com`&#8203;。 若您使用了其他工具向同一Web服務傳送請求，那麼您應確保您的工具也包含類似&#8203;`WebLimiterDelay`&#8203;的功能，並將兩者都設為預設值的兩倍，也就是&#8203;`600`&#8203;。 這能保證在任何情形下，您都不會在每個&#8203;`300`&#8203;毫秒中傳送超過1個請求。

在一般情形下，將&#8203;`WebLimiterDelay`&#8203;減少至預設值以下是&#8203;**強烈反對這麼做的**&#8203;，因為它可能會導致各種IP相關的封鎖，有可能會是永久的。 預設值足以在伺服器上執行單一ASF實例，或在一般情境下與原版的Steam用戶端一起使用ASF。 對於大多數用法來說這應該是正確的值，且您應該只能增加它（永遠不要減少），除非⸺除了ASF之外，您還使用了另一個可能會向ASF所使用的Web服務傳送大量請求的工具。 簡而言之，由單一IP傳送至單個Steam網域的請求總量每&#8203;`300`&#8203;毫秒不應大於1個請求。

除非您有理由編輯此屬性，否則您應維持它為預設值。

---

### `WebProxy（網頁代理）`

`string`&#8203;型別，預設值為&#8203;`null`&#8203;。 本屬性定義了網頁代理位址，用於ASF的&#8203;`HttpClient`&#8203;傳送的所有HTTP及HTTPS請求，特別是例如&#8203;`github.com`&#8203;、&#8203;`steamcommunity.com`&#8203;及&#8203;`store.steampowered.com`&#8203;服務。 在一般情形下，代理ASF請求並沒有好處，但它對於繞過各式各樣的防火牆來說特別有用，特別是中國的長城防火牆。

本屬性定義為URI本文：

> URI字串由協定名稱（支援：http/https/socks4/socks4a/socks5）、主機名稱及選擇性的連接埠所組成。 一個完整URI字串的範例是&#8203;`"http://contoso.com:8080"`&#8203;。

若您的代理需要使用者身分驗證，您還需要設定&#8203;`WebProxyUsername`&#8203;與／或&#8203;`WebProxyPassword`&#8203;。 若不需要，則僅設定本屬性即可。

目前ASF只對&#8203;`HTTP`&#8203;及&#8203;`HTTPS`&#8203;請求使用網際網路代理，&#8203;**不包含**&#8203;在ASF內部Steam用戶端完成的內部Steam網路通訊。 目前並無支援該功能的計畫，其主要原因是缺少了&#8203;**[SK2](https://github.com/SteamRE/SteamKit/issues/587#issuecomment-413271550)**&#8203;功能。 若您需要／希望支援此功能，我建議可以先從此處了解。

除非您有理由編輯此屬性，否則您應維持它為預設值。

---

### `WebProxyPassword（網頁代理密碼）`

`string`&#8203;型別，預設值為&#8203;`null`&#8203;。 本屬性定義了要提供代理功能的目標&#8203;`WebProxy`&#8203;設備支援的Basic、Digest、NTLM或Kerberos身分驗證所使用的密碼欄位。 若您的代理不需要使用者憑證，您就不需要再此處輸入任何內容。 本選項只有在使用了&#8203;`WebProxy`&#8203;的情形下才有用處，否則它沒有任何效果。

除非您有理由編輯此屬性，否則您應維持它為預設值。

---

### `WebProxyUsername（網頁代理使用者名稱）`

`string`&#8203;型別，預設值為&#8203;`null`&#8203;。 本屬性定義了要提供代理功能的目標&#8203;`WebProxy`&#8203;設備支援的Basic、Digest、NTLM或Kerberos身分驗證所使用的使用者名稱欄位。 若您的代理不需要使用者憑證，您就不需要再此處輸入任何內容。 本選項只有在使用了&#8203;`WebProxy`&#8203;的情形下才有用處，否則它沒有任何效果。

除非您有理由編輯此屬性，否則您應維持它為預設值。

---

## Bot 設定檔

如您所已知的，每個Bot都有自己的設定檔，使用基於下列範例的JSON結構： 首先您需要決定您的Bot的名稱（例如&#8203;`1.json`&#8203;、&#8203;`main.json`&#8203;、&#8203;`primary.json`&#8203;或&#8203;`AnythingElse.json`&#8203;），然後再開始設定。

**注意：**&#8203;Bot無法被命名為&#8203;`ASF`&#8203;（因為該關鍵字保留為全域設定），ASF也會忽略所有以點為開頭的設定檔。

Bot設定檔具有以下結構：

```json
{
    "AcceptGifts": false,
    "AutoSteamSaleEvent": false,
    "BotBehaviour": 0,
    "CompleteTypesToSend": [],
    "CustomGamePlayedWhileFarming": null,
    "CustomGamePlayedWhileIdle": null,
    "Enabled": false,
    "FarmingOrders": [],
    "FarmPriorityQueueOnly": false,
    "GamesPlayedWhileIdle": [],
    "HoursUntilCardDrops": 3,
    "LootableTypes": [1, 3, 5],
    "MatchableTypes": [5],
    "OnlineFlags": 0,
    "OnlineStatus": 1,
    "PasswordFormat": 0,
    "Paused": false,
    "RedeemingPreferences": 0,
    "RemoteCommunication": 3,
    "SendOnFarmingFinished": false,
    "SendTradePeriod": 0,
    "ShutdownOnFarmingFinished": false,
    "SkipRefundableGames": false,
    "SteamLogin": null,
    "SteamMasterClanID": 0,
    "SteamParentalCode": null,
    "SteamPassword": null,
    "SteamTradeToken": null,
    "SteamUserPermissions": {},
    "TradingPreferences": 0,
    "TransferableTypes": [1, 3, 5],
    "UseLoginKeys": true,
    "UserInterfaceMode": 0
}
```

---

所有選項的解釋如下：

### `AcceptGifts（接受贈禮）`

`bool`&#8203;型別，預設值為&#8203;`false`&#8203;。 啟用時，ASF會自動接受並兌換所有發送給Bot的Steam禮物（包含錢包禮物卡）。 這也包含了不在&#8203;`SteamUserPermissions`&#8203;中定義的使用者所發送的禮物。 請注意，發送到電子郵件的禮物不會直接轉發給用戶端，所以ASF無法在沒有您幫助的情形下接受這些禮物。

本選項建議只在小號上使用，因為您很有可能不希望在主要帳號上自動兌換所有發送過來的禮物。 若您不確定您是否想要啟用本功能，請保留預設值&#8203;`false`&#8203;。

---

### `AutoSteamSaleEvent（自動完成 Steam 特賣活動）`

`bool`&#8203;型別，預設值為&#8203;`false`&#8203;。 眾所周知，在Steam夏季／冬季特賣活動期間，Steam透過每日瀏覽探索佇列或其他活動特定事件來提供您額外的交換卡片。 啟用本選項時，ASF會每隔&#8203;`8`&#8203;小時自動檢查Steam探索佇列（從程式啟動一小時內開始），並在需要時完成它。 若您想自己執行該行為，則不建議使用本選項，因為在一般情形下本選項只對Bot帳號有意義。 此外，若您想要收到這些交換卡片，您需要確保您的帳號等級至少&#8203;`8`&#8203;等，這是Steam本身的限制。 若您不確定您是否想要啟用本功能，請保留預設值&#8203;`false`&#8203;。

請注意，由於Valve經常產生問題或改動，&#8203;**我們無法保證本功能是否能運作正常**&#8203;，因此本選項是很有可能&#8203;**完全無效**&#8203;的。 我們不接受&#8203;**任何**&#8203;相關的錯誤報告，也不支援關於本選項的請求。 這是在沒有任何保證的情形下所提供的，您需要自行承擔風險。

---

### `BotBehaviour（Bot 行為）`

`byte flags`&#8203;型別，預設值為&#8203;`0`&#8203;。 本屬性定義了在各種事件中ASF的自動化行為，定義項如下：

| 值  | 名稱                            | 描述                                     |
| -- | ----------------------------- | -------------------------------------- |
| 0  | 無                             | 無特殊Bot行為，帳號控制最少，預設值                    |
| 1  | RejectInvalidFriendInvites    | 使ASF拒絕（而非忽略）無效的好友邀請                    |
| 2  | RejectInvalidTrades           | 使ASF拒絕（而非忽略）無效的交易提案                    |
| 4  | RejectInvalidGroupInvites     | 使ASF拒絕（而非忽略）無效的群組邀請                    |
| 8  | DismissInventoryNotifications | 使ASF自動取消所有物品庫通知                        |
| 16 | MarkReceivedMessagesAsRead    | 使ASF自動將所有收到的訊息標示成已讀                    |
| 32 | MarkBotMessagesAsRead         | 使ASF自動將所有從其他（執行於同一實例中的）ASF Bot的訊息標示成已讀 |

請注意，本屬性為&#8203;`flags`&#8203;欄位，因此可以使用所有可用值任意組合。 若您想了解更多，請參閱&#8203;**[旗標映射](#json-映射)**&#8203;。 不啟用任何旗標即為&#8203;`None`&#8203;選項。

在一般情形下，若您想要ASF有一定的自動化行為，像使用Bot帳號，而不是ASF主要帳號的那樣，就需要更改本屬性。 因此，本屬性主要用於小號，但您仍可在主要帳號上使用所選選項。

一般（&#8203;`None`&#8203;）模式的ASF行為只是將使用者所想要的自動化（例如自動掛卡或&#8203;`SteamTradeMatcher`&#8203;交易，如果在&#8203;`TradingPreferences`&#8203;中有設定）。 這是對帳號控制最少的模式，並對大多數使用者來說都不錯，因為您可以完全控制您的帳號，且能自行決定是否允許某些超出範圍的交互行為。

無效的好友邀請是指由不具&#8203;`FamilySharing`&#8203;或更高權限（定義於&#8203;`SteamUserPermissions`&#8203;）的使用者所發出的好友邀請。 如您所預期的那樣，一般模式下的ASF會忽略這些邀請，給予您自由選擇是否接受邀請的空間。 `RejectInvalidFriendInvites`&#8203;會使這些邀請被自動拒絕，實際上這會阻止其他人將您加入至他們的好友清單中（因為ASF會拒絕所有這類請求，除非他們在&#8203;`SteamUserPermissions`&#8203;中有被定義）。 除非您想要徹底拒絕所有好友邀請，否則您不應啟用本選項。

無效的交易提案是指不被ASF內建模組接受的交易。 關於本問題的更多資訊，可以在&#8203;**[交易](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading-zh-TW)**&#8203;章節中找到，其中明確定義了ASF願意自動接受哪些類型的交易。 有效的交易還會被其他設定所定義，特別是&#8203;`TradingPreferences`&#8203;。 `RejectInvalidTrades`&#8203;會使所有無效的交易提案被拒絕，而不是被忽略。 除非您想要直接拒絕所有ASF未自動接受的交易提案，否則您不應啟用本選項。

無效的群組邀請是指來自&#8203;`SteamMasterClanID`&#8203;群組以外的邀請。 如您所預期的那樣，一般模式下的ASF會忽略這些邀請，使您可以自行決定是否要加入特定的群組。 `RejectInvalidGroupInvites`&#8203;會使這些群組邀請被拒絕，使您無法加入&#8203;`SteamMasterClanID`&#8203;以外的其他任何群組。 除非您想要徹底拒絕所有群組邀請，否則您不應啟用本選項。

若您開始對收到新物品的通知感到厭煩時，&#8203;`DismissInventoryNotifications`&#8203;會是一個相當有用的功能。 ASF無法去除通知本身，因為它是您Steam用戶端內建的功能，但它能在收到通知後自動清除通知，這樣就不會留有「新物品在您的物品庫」的通知了。 若您希望自行評估所收到的物品（特別是ASF掛到的卡片），那麼很明顯您不應該啟用本選項。 但如果您快要開始被煩到抓狂了，切記這裡有個選項可以使用。

`MarkReceivedMessagesAsRead`&#8203;會自動將ASF執行的帳號所收到的&#8203;**所有**&#8203;訊息標示成已讀，包含私人及群組聊天。 這通常只應由小號所使用，以清除例如在執行ASF指令期間來自您的「新訊息」通知。 我們不建議在主要帳號上使用本選項，除非您希望能隔絕所有種類的訊息通知，&#8203;**包含**&#8203;在您離線時的通知，ASF仍會清除它們。

`MarkBotMessagesAsRead`&#8203;以類似的方式運作，但只會將Bot的訊息標示成已讀。 但請注意，在使用本選項時，若您的Bot與其他人都在群組聊天中，已知Steam在將聊天訊息標示成已讀時，&#8203;**也**&#8203;會將&#8203;**該訊息前的**&#8203;訊息標示成已讀，所以如果您不想要錯過在這期間的其他訊息，就應該避免使用本功能。 很顯然，當您在同一個ASF實例上執行多個主要帳號（例如其他使用者的帳號）時也是有風險的，因為您可能會錯過正常的無關ASF的訊息。

若您不確定如何設定本選項，最好保留預設值。

---

### `CompleteTypesToSend（傳送的完成種類）`

`ImmutableHashSet<byte>`&#8203;型別，預設值為空。 在ASF完成收集一套此處設定的物品類型時，它可以透過Steam交易傳送所有收集完成的物品套組自動傳送給具有&#8203;`Master`&#8203;權限的使用者，如果您想將指定的Bot帳號用於例如TM匹配等需求，同時將收集完成的套組移動至另一個帳號上，這將會非常方便。 本選項與&#8203;`loot`&#8203;指令作用相同，因此請注意，首先它需要您有帳號的交易合法權限，且使用者具有&#8203;`Master`&#8203;權限，而您可能也要有有效的&#8203;`SteamTradeToken`&#8203;。

目前，在本設定中支援下列物品類型：

| 值 | 名稱              | 描述                               |
| - | --------------- | -------------------------------- |
| 3 | FoilTradingCard | 閃亮版本的&#8203;`TradingCard`&#8203; |
| 5 | TradingCard     | Steam交換卡片，用於合成徽章（非閃亮卡片）          |

請注意，不論上述如何設定，ASF都只會處理Steam分類（&#8203;`appID`&#8203;為753）中的社群物品（&#8203;`contextID`&#8203;為6），因此依據定義所有遊戲物品、禮物等物品都會被排除在交易提案之外。

Due to additional overhead of using this option, it's recommended to use it only on bot accounts that have a realistic chance of finishing sets on their own - for example, it makes no sense to activate if you're already using `SendOnFarmingFinished`, `SendTradePeriod` or `loot` command on usual basis.

If you're unsure how to configure this option, it's best to leave it at default.

---

### `CustomGamePlayedWhileFarming（掛卡時顯示自訂遊戲名稱）`

`string`&#8203;型別，預設值為&#8203;`null`&#8203;。 When ASF is farming, it can display itself as "Playing non-steam game: `CustomGamePlayedWhileFarming`" instead of currently farmed game. This can be useful if you would like to let your friends know that you're farming, yet you don't want to use `OnlineStatus` of `Offline`. Please note that ASF cannot guarantee the actual display order of Steam network, therefore this is only a suggestion that may, or may not, display properly. In particular, custom name will not display in `Complex` farming algorithm if ASF fills all `32` slots with games requiring hours to be bumped. Default value of `null` disables this feature.

ASF provides a few special variables that you can optionally use in your text. `{0}` will be replaced by ASF with `AppID` of currently farmed game(s), while `{1}` will be replaced by ASF with `GameName` of currently farmed game(s).

---

### `CustomGamePlayedWhileIdle（閒置時顯示自訂遊戲名稱）`

`string`&#8203;型別，預設值為&#8203;`null`&#8203;。 Similar to `CustomGamePlayedWhileFarming`, but for the situation when ASF has nothing to do (as account is fully farmed). Please note that ASF cannot guarantee the actual display order of Steam network, therefore this is only a suggestion that may, or may not, display properly. If you're using `GamesPlayedWhileIdle` together with this option, then keep in mind that `GamesPlayedWhileIdle` get priority, therefore you can't declare more than `31` of them, as otherwise `CustomGamePlayedWhileIdle` will not be able to occupy the slot for custom name. Default value of `null` disables this feature.

---

### `Enabled`

`bool`&#8203;型別，預設值為&#8203;`false`&#8203;。 This property defines if bot is enabled. Enabled bot instance (`true`) will automatically start on ASF run, while disabled bot instance (`false`) will need to be started manually. By default every bot is disabled, so you probably want to switch this property to `true` for all of your bots that should be started automatically.

---

### `FarmingOrders`

`ImmutableList<byte>`&#8203;型別，預設值為空。 This property defines the **preferred** farming order used by ASF for given bot account. Currently there are following farming orders available:

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
| 9  | Random（隨機）                | Try to farm games in totally random order (different on each run of the program) |
| 10 | BadgeLevelsAscending      | Try to farm games with lowest badge levels first                                 |
| 11 | BadgeLevelsDescending     | Try to farm games with highest badge levels first                                |
| 12 | RedeemDateTimesAscending  | Try to farm oldest games on our account first                                    |
| 13 | RedeemDateTimesDescending | Try to farm newest games on our account first                                    |
| 14 | MarketableAscending       | Try to farm games with unmarketable card drops first                             |
| 15 | MarketableDescending      | Try to farm games with marketable card drops first                               |

Since this property is an array, it allows you to use several different settings in your fixed order. For example, you can include values of `15`, `11` and `7` in order to sort by marketable games first, then by those with highest badge level, and finally alphabetically. As you can guess, the order actually matters, as reverse one (`7`, `11` and `15`) achieves something entirely different (sorts games alphabetically first, and due to game names being unique, the other two are effectively useless). Majority of people will probably use just one order out of all of them, but in case you want to, you can also sort further by extra parameters.

Also notice the word "try" in all above descriptions - the actual ASF order is heavily affected by selected **[cards farming algorithm](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)** and sorting will affect only results that ASF considers same performance-wise. For example, in `Simple` algorithm, selected `FarmingOrders` should be entirely respected in current farming session (as every game has the same performance value), while in `Complex` algorithm actual order is affected by hours first, and then sorted according to chosen `FarmingOrders`. This will lead to different results, as games with existing playtime will have a priority over others, so effectively ASF will prefer games that already passed required `HoursUntilCardDrops` firstly, and only then sorting those games further by your chosen `FarmingOrders`. Likewise, once ASF runs out of already-bumped games, it'll sort remaining queue by hours first (as that will decrease time required for bumping any of remaining titles to `HoursUntilCardDrops`). Therefore, this config property is only a **suggestion** that ASF will try to respect, as long as it doesn't affect performance negatively (in this case, ASF will always prefer farming performance over `FarmingOrders`).

There is also farming priority queue that is accessible through `fq` **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. If it's used, actual farming order is sorted firstly by performance, then by farming queue, and finally by your `FarmingOrders`.

---

### `FarmPriorityQueueOnly`

`bool`&#8203;型別，預設值為&#8203;`false`&#8203;。 This property defines if ASF should consider for automatic farming only apps that you added yourself to priority farming queue available with `fq` **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. When this option is enabled, ASF will skip all `appIDs` that are missing on the list, effectively allowing you to cherry-pick games for automatic ASF farming. Keep in mind that if you didn't add any games to the queue then ASF will act as if there is nothing to farm on your account. If you're unsure whether you want this feature enabled or not, keep it with default value of `false`.

---

### `GamesPlayedWhileIdle`

<`ImmutableHashSet<uint>`&#8203;型別，預設值為空。 If ASF has nothing to farm it can play your specified steam games (`appIDs`) instead. Playing games in such manner increases your "hours played" of those games, but nothing else apart of it. In order for this feature to work properly, your Steam account **must** own a valid license to all the `appIDs` that you specify here, this includes F2P games as well. This feature can be enabled at the same time with `CustomGamePlayedWhileIdle` in order to play your selected games while showing custom status in Steam network, but in this case, like in `CustomGamePlayedWhileFarming` case, the actual display order is not guaranteed. Please note that Steam allows ASF to play only up to `32` `appIDs` total, therefore you can put only as many games in this property.

---

### `HoursUntilCardDrops`

`byte`&#8203;型別，預設值為&#8203;`3`&#8203;。 This property defines if account has card drops restricted, and if yes, for how many initial hours. Restricted card drops means that account is not receiving any card drops from given game until the game is played for at least `HoursUntilCardDrops` hours. Unfortunately there is no magical way to detect that, so ASF relies on you. This property affects **[cards farming algorithm](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)** that will be used. Setting this property properly will maximize profits and minimize time required for cards to be farmed. Remember that there is no obvious answer whether you should use one or another value, since it fully depends on your account. It seems that older accounts which never asked for refund have unrestricted card drops, so they should use a value of `0`, while new accounts and those who did ask for refund have restricted card drops with a value of `3`. 但是，這只是理論，不應將它視為規則。 The default value for this property was set based on "lesser evil" and majority of use cases.

---

### `LootableTypes`

`ImmutableHashSet<byte>`&#8203;型別，預設值為&#8203;`1, 3, 5`&#8203;的Steam物品類型。 This property defines ASF behaviour when looting - both manual, using a **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**, as well as automatic one, through one or more configuration properties. ASF will ensure that only items from `LootableTypes` will be included in a trade offer, therefore this property allows you to choose what you want to receive in a trade offer that is being sent to you.

| 值  | 名稱                    | 描述                                                            |
| -- | --------------------- | ------------------------------------------------------------- |
| 0  | Unknown               | Every type that doesn't fit in any of the below               |
| 1  | BoosterPack           | Booster pack containing 3 random cards from a game            |
| 2  | Emoticon              | Emoticon to use in Steam Chat                                 |
| 3  | FoilTradingCard       | Foil variant of `TradingCard`                                 |
| 4  | ProfileBackground     | Profile background to use on your Steam profile               |
| 5  | TradingCard           | Steam trading card, being used for crafting badges (non-foil) |
| 6  | SteamGems             | Steam gems being used for crafting boosters, sacks included   |
| 7  | SaleItem              | Special items awarded during Steam sales                      |
| 8  | Consumable            | Special consumable items that disappear after being used      |
| 9  | ProfileModifier       | Special items that can modify Steam profile appearance        |
| 10 | Sticker               | 可用在 Steam 聊天中的特殊物品                                            |
| 11 | ChatEffect            | 可用在 Steam 聊天中的特殊物品                                            |
| 12 | MiniProfileBackground | Special background for Steam profile                          |
| 13 | AvatarProfileFrame    | Special avatar frame for Steam profile                        |
| 14 | AnimatedAvatar        | Special animated avatar for Steam profile                     |
| 15 | KeyboardSkin          | Steam Deck 的特別鍵盤造型                                            |

Please note that regardless of the settings above, ASF will only ask for Steam (`appID` of 753) community (`contextID` of 6) items, so all game items, gifts and likewise, are excluded from the trade offer by definition.

Default ASF setting is based on the most common usage of the bot, with looting only booster packs, and trading cards (including foils). The property defined here allows you to alter that behaviour in whatever way that satisfies you. Please keep in mind that all types not defined above will show as `Unknown` type, which is especially important when Valve releases some new Steam item, that will be marked as `Unknown` by ASF as well, until it's added here (in the future release). That's why in general it's not recommended to include `Unknown` type in your `LootableTypes`, unless you know what you're doing, and you also understand that ASF will send your entire inventory in a trade offer if Steam Network gets broken again and reports all your items as `Unknown`. My strong suggestion is to not include `Unknown` type in the `LootableTypes`, even if you expect to loot everything (else).

---

### `MatchableTypes`

`ImmutableHashSet<byte>`&#8203;型別，預設值為&#8203;`5`&#8203;的Steam物品類型。 This property defines which Steam item types are permitted to be matched when `SteamTradeMatcher` option in `TradingPreferences` is enabled. Types are defined as below:

| 值  | 名稱                    | 描述                                                            |
| -- | --------------------- | ------------------------------------------------------------- |
| 0  | Unknown               | Every type that doesn't fit in any of the below               |
| 1  | BoosterPack           | Booster pack containing 3 random cards from a game            |
| 2  | Emoticon              | Emoticon to use in Steam Chat                                 |
| 3  | FoilTradingCard       | Foil variant of `TradingCard`                                 |
| 4  | ProfileBackground     | Profile background to use on your Steam profile               |
| 5  | TradingCard           | Steam trading card, being used for crafting badges (non-foil) |
| 6  | SteamGems             | Steam gems being used for crafting boosters, sacks included   |
| 7  | SaleItem              | Special items awarded during Steam sales                      |
| 8  | Consumable            | Special consumable items that disappear after being used      |
| 9  | ProfileModifier       | Special items that can modify Steam profile appearance        |
| 10 | Sticker               | 可用在 Steam 聊天中的特殊物品                                            |
| 11 | ChatEffect            | 可用在 Steam 聊天中的特殊物品                                            |
| 12 | MiniProfileBackground | Special background for Steam profile                          |
| 13 | AvatarProfileFrame    | Special avatar frame for Steam profile                        |
| 14 | AnimatedAvatar        | Special animated avatar for Steam profile                     |
| 15 | KeyboardSkin          | Steam Deck 的特別鍵盤造型                                            |

Of course, types that you should use for this property typically include only `2`, `3`, `4` and `5`, as only those types are supported by STM. ASF includes proper logic for discovering rarity of the items, therefore it's also safe to match emoticons or backgrounds, as ASF will properly consider fair only those items from the same game and type, that also share the same rarity.

Please note that **ASF is not a trading bot** and **will NOT care about the market price**. If you don't consider items of the same rarity from the same set to be the same price-wise, then this option is NOT for you. Please evaluate twice if you understand and agree with this statement before you decide to change this setting.

Unless you know what you're doing, you should keep it with default value of `5`.

---

### `OnlineFlags`

`ushort flags`&#8203;型別，預設值為&#8203;`0`&#8203;。 This property works as supplement to **[`OnlineStatus`](#onlinestatus)** and specifies additional online presence features announced to Steam network. Requires **[`OnlineStatus`](#onlinestatus)** other than `Offline`, and is defined as below:

| 值    | 名稱                | 描述                                        |
| ---- | ----------------- | ----------------------------------------- |
| 0    | 無                 | No special online presence flags, default |
| 256  | ClientTypeWeb     | Client is using web interface             |
| 512  | ClientTypeMobile  | Client is using mobile app                |
| 1024 | ClientTypeTenfoot | Client is using big picture               |
| 2048 | ClientTypeVR      | Client is using VR headset                |

Please notice that this property is `flags` field, therefore it's possible to choose any combination of available values. Check out **[flags mapping](#json-mapping)** if you'd like to learn more. Not enabling any of flags results in `None` option.

The underlying `EPersonaStateFlag` type that this property is based on includes more available flags, however, to the best of our knowledge they have absolutely no effect as of today, therefore they were cut for visibility.

If you're not sure how to set this property, leave it with default value of `0`.

---

### `OnlineStatus（線上狀態）`

`byte`&#8203;型別，預設值為&#8203;`1`&#8203;。 This property specifies Steam community status that the bot will be announced with after logging in to Steam network. 目前您可以選擇以下狀態之一：

| 值 | 名稱                     |
| - | ---------------------- |
| 0 | Offline（離線）            |
| 1 | Online（線上）             |
| 2 | Busy（忙碌）               |
| 3 | Away（離開）               |
| 4 | Snooze（打盹）             |
| 5 | LookingToTrade（尋找交易對象） |
| 6 | LookingToPlay（尋找遊戲對象）  |
| 7 | Invisible（隱藏）          |

`Offline` status is extremely useful for primary accounts. As you should know, farming a game actually shows your steam status as "Playing game: XXX", which can be misleading to your friends, confusing them that you're playing a game while actually you're only farming it. Using `Offline` status solves that issue - your account will never be shown as "in-game" when you're farming steam cards with ASF. This is possible thanks to the fact that ASF does not have to sign in into Steam Community in order to work properly, so we're in fact playing those games, connected to Steam network, but without announcing our online presence at all. Keep in mind that played games using offline status will still count towards your playtime, and show as "recently played" on your profile.

In addition to that, this feature is also important if you want to receive notifications and unread messages when ASF is running, while not keeping Steam client open at the same time. This is because ASF acts as a Steam client itself, and whether ASF would like it or not, Steam broadcasts all those messages and other events to it. This is not a problem if you have both ASF and your own Steam client running, as both clients receive exactly the same events. However, if just ASF is running, Steam network could mark certain events and messages as "delivered", despite of your traditional Steam client not receiving it due to not being present. Offline status also solves this problem, as ASF is never considered for any community events in this case, so all unread messages and other events will be properly marked as unread when you come back.

It's important to note that ASF running on `Offline` mode will **not** be able to receive commands in usual Steam chat way, as the chat, as well as entire community presence is in fact, entirely offline. A solution to this issue is using `Invisible` mode instead which works in a similar way (not exposing status), but keeps the ability to receive and respond to messages (so also a potential to dismiss notifications and unread messages as stated above). `Invisible` mode makes the most sense on alt accounts that you don't want to expose (status-wise), but still be able to send commands to.

However, there is one catch with `Invisible` mode - it doesn't go well with primary accounts. This is because **any** Steam session that is currently online **exposes** the status, even if ASF itself does not. This is caused by the current limitation/bug of the Steam network that isn't possible to be fixed on ASF side, so if you want to use `Invisible` mode you will also need to ensure that **all** other sessions to the same account use `Invisible` mode as well. This will be the case on alt accounts where ASF is hopefully the only active session, but on primary accounts you'll almost always prefer to show as `Online` to your friends, hiding only ASF activity, and in this case `Invisible` mode will be entirely useless for you (we recommend to use `Offline` mode instead). Hopefully this limitation/bug will be eventually solved in the future by Valve, but I wouldn't expect that to happen anytime soon...

If you're unsure how to set up this property, it's recommended to use a value of `0` (`Offline`) for primary accounts, and default `1` (`Online`) otherwise.

---

### `PasswordFormat（密碼格式）`

`byte`&#8203;型別，預設值為&#8203;`0`&#8203;（&#8203;`PlainText`&#8203;）。 This property defines the format of `SteamPassword` property, and currently supports values specified in the **[security](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security)** section. You should follow the instructions specified there, as you'll need to ensure that `SteamPassword` property indeed includes password in matching `PasswordFormat`. In other words, when you change `PasswordFormat` then your `SteamPassword` should be **already** in that format, not just aiming to be. Unless you know what you're doing, you should keep it with default value of `0`.

---

### `Paused（暫停）`

`bool`&#8203;型別，預設值為&#8203;`false`&#8203;。 This property defines initial state of `CardsFarmer` module. With default value of `false`, bot will automatically start farming when it's started, either because of `Enabled` or `start` command. Switching this property to `true` should be done only if you want to manually `resume` automatic farming process, for example because you want to use `play` all the time and never use automatic `CardsFarmer` module - this works exactly the same as `pause` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. If you're unsure whether you want this feature enabled or not, keep it with default value of `false`.

---

### `RedeemingPreferences`

`byte flags`&#8203;型別，預設值為&#8203;`0`&#8203;。 This property defines ASF behaviour when redeeming cd-keys, and is defined as below:

| 值 | 名稱                                 | 描述                                                                                                                              |
| - | ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| 0 | 無                                  | No special redeeming preferences, default                                                                                       |
| 1 | Forwarding                         | Forward keys unavailable to redeem to other bots                                                                                |
| 2 | Distributing                       | Distribute all keys among itself and other bots                                                                                 |
| 4 | KeepMissingGames                   | Keep keys for (potentially) missing games when forwarding, leaving them unused                                                  |
| 8 | AssumeWalletKeyOnBadActivationCode | Assume that `BadActivationCode` keys are equal to `CannotRedeemCodeFromClient`, and therefore try to redeem them as wallet keys |

Please notice that this property is `flags` field, therefore it's possible to choose any combination of available values. Check out **[flags mapping](#json-mapping)** if you'd like to learn more. Not enabling any of flags results in `None` option.

`Forwarding` will cause bot to forward a key that is not possible to redeem, to another connected and logged on bot that is missing that particular game (if possible to check). The most common situation is forwarding `AlreadyPurchased` game to another bot that is missing that particular game, but this option also covers other scenarios, such as `DoesNotOwnRequiredApp`, `RateLimited` or `RestrictedCountry`.

`Distributing` will cause bot to distribute all received keys among itself and other bots. This means that every bot will get a single key from the batch. Typically this is used only when you're redeeming many keys for the same game, and you want to evenly distribute them among your bots, as opposed to redeeming keys for various different games. This feature makes no sense if you're redeeming only one key in a single `redeem` action (as there are no extra keys to be distributed).

`KeepMissingGames` will cause bot to skip `Forwarding` when we can't be sure if key being redeemed is in fact owned by our bot, or not. This basically means that `Forwarding` will apply **only** to `AlreadyPurchased` keys, instead of covering also other cases such as `DoesNotOwnRequiredApp`, `RateLimited` or `RestrictedCountry`. Typically you want to use this option on primary account, to ensure that keys being redeemed on it won't be forwarded further if your bot for example becomes temporarily `RateLimited`. As you can guess from the description, this field has absolutely no effect if `Forwarding` is not enabled.

`AssumeWalletKeyOnBadActivationCode` will cause `BadActivationCode` keys to be treated as `CannotRedeemCodeFromClient`, and therefore result in ASF trying to redeem them as wallet keys. This is needed because Steam might announce wallet keys as `BadActivationCode` (and not `CannotRedeemCodeFromClient` as it used to), resulting in ASF never attempting to redeem them. However, we recommend **against** using this preference, as it'll result in ASF trying to redeem every invalid key as a wallet code, resulting in excessive amount of (potentially invalid) requests sent to the Steam service, with all the potential consequences. Instead, we recommend to use `ForceAssumeWalletKey` **[`redeem^`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands#redeem-modes)** mode while knowingly redeeming wallet keys, which will enable the needed workaround only when it's required, on as-needed basis.

Enabling both `Forwarding` and `Distributing` will add distributing feature on top of forwarding one, which makes ASF trying to redeem one key on all bots firstly (forwarding) before moving to the next one (distributing). Typically you want to use this option only when you want `Forwarding`, but with altered behaviour of switching the bot on key being used, instead of always going in-order with every key (which would be `Forwarding` alone). This behaviour can be beneficial if you know that majority or even all of your keys are tied to the same game, because in this situation `Forwarding` alone would try to redeem everything on one bot firstly (which makes sense if your keys are for unique games), and `Forwarding` + `Distributing` will switch the bot on the next key, "distributing" the task of redeeming new key onto another bot than the initial one (which makes sense if keys are for the same game, skipping one pointless attempt per key).

The actual bots order for all of the redeeming scenarios is alphabetical, excluding bots that are unavailable (not connected, stopped or likewise). Please keep in mind that there is per-IP and per-account hourly limit of redeeming tries, and every redeem try that didn't end with `OK` contributes to failed tries. ASF will do its best to minimize number of `AlreadyPurchased` failures, e.g. by trying to avoid forwarding a key to another bot that already owns that particular game, but it's not always guaranteed to work due to how Steam is handling licenses. Using redeeming flags such as `Forwarding` or `Distributing` will always increase your likelyhood to hit `RateLimited`.

Also keep in mind that you can't forward or distribute keys to bots that you do not have access to. This should be obvious, but ensure that you're at least `Operator` of all the bots you want to include in your redeeming process, for example with `status ASF` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**.

---

### `RemoteCommunication`

`byte flags`&#8203;型別，預設值為&#8203;`3`&#8203;。 This property defines per-bot ASF behaviour when it comes to communication with remote, third-party services, and is defined as below:

| 值 | 名稱            | 描述                                                                                                                                                                                                                                                                |
| - | ------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0 | 無             | No allowed third-party communication, rendering selected ASF features unusable                                                                                                                                                                                    |
| 1 | SteamGroup    | Allows communication with **[ASF's Steam group](https://steamcommunity.com/groups/archiasf)**                                                                                                                                                                     |
| 2 | PublicListing | Allows communication with **[ASF's STM listing](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/ItemsMatcherPlugin#publiclisting)** in order to being listed, if user has also enabled `SteamTradeMatcher` in **[`TradingPreferences`](#tradingpreferences)** |

Please notice that this property is `flags` field, therefore it's possible to choose any combination of available values. Check out **[flags mapping](#json-mapping)** if you'd like to learn more. Not enabling any of flags results in `None` option.

This option doesn't include every third-party communication offered by ASF, only those that are not implied by other settings. For example, if you've enabled ASF's auto-updates, ASF will communicate with both GitHub (for downloads) and our server (for checksum verification), as per your configuration. Likewise, enabling `MatchActively` in **[`TradingPreferences`](#tradingpreferences)** implies communication with our server to fetch listed bots, which is required for that functionality.

Further explanation on this subject is available in **[remote communication](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Remote-communication)** section. Unless you have a reason to edit this property, you should keep it at default.

---

### `SendOnFarmingFinished`

`bool`&#8203;型別，預設值為&#8203;`false`&#8203;。 When ASF is done with farming given account, it can automatically send steam trade containing everything farmed up to this point to user with `Master` permission, which is very convenient if you don't want to bother with trades yourself. This option works the same as `loot` command, therefore keep in mind that it requires user with `Master` permission set, you may also need a valid `SteamTradeToken`, as well as using an account that is eligible for trading in the first place. In addition to initiating `loot` after finishing farming, ASF will also initiate `loot` on each new items notification (when not farming), and after completing each trade that results in new items (always) when this option is active. This is especially useful for "forwarding" items received from other people to our account.

Typically you'll want to use **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** together with this feature, although it's not a requirement if you intend to confirm manually in timely fashion. If you're not sure how to set this property, leave it with default value of `false`.

---

### `SendTradePeriod`

`byte`&#8203;型別，預設值為&#8203;`0`&#8203;。 This property works very similar to `SendOnFarmingFinished` property, with one difference - instead of sending trade when farming is done, we can also send it every `SendTradePeriod` hours, regardless of how much we have to farm left. This is useful if you want to `loot` your alt accounts on usual basis instead of waiting for it to finish farming. Default value of `0` disables this feature, if you want your bot to send you trade e.g. every day, you should put `24` here.

Typically you'll want to use **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** together with this feature, although it's not a requirement if you intend to confirm manually in timely fashion. If you're not sure how to set this property, leave it with default value of `0`.

---

### `ShutdownOnFarmingFinished`

`bool`&#8203;型別，預設值為&#8203;`false`&#8203;。 ASF is "occupying" an account for the whole time of process being active. When given account is done with farming, ASF periodically checks it (every `IdleFarmingPeriod` hours), if perhaps some new games with steam cards were added in the meantime, so it can resume farming of that account without a need to restart the process. This is useful for majority of people, as ASF can automatically resume farming when needed. However, you may actually want to stop the process when given account is fully farmed, you can achieve that by setting this property to `true`. When enabled, ASF will proceed with logging off when account is fully farmed, which means that it won't be periodically checked or occupied anymore. You should decide yourself if you prefer ASF to work on given bot instance for the whole time, or if perhaps ASF should stop it when farming process is done. When all accounts are stopped and process is not running in `--process-required` **[mode](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-Line-Arguments)**, ASF will shutdown as well, putting your machine at rest and allowing you to schedule other actions, such as sleep or shutdown at the same moment of last card dropping.

If you're not sure how to set this property, leave it with default value of `false`.

---

### `SkipRefundableGames`

`bool`&#8203;型別，預設值為&#8203;`false`&#8203;。 This property defines if ASF is permitted to farm games that are still refundable. A refundable game is a game that you bought in last 2 weeks through Steam Store and didn't play for longer than 2 hours yet, as stated on **[Steam refunds](https://store.steampowered.com/steam_refunds)** page. By default when this option is set to `false`, ASF ignores Steam refunds policy entirely and farms everything, as most people would expect. However, you can change this option to `true` if you want to ensure that ASF won't farm any of your refundable games too soon, allowing you to evaluate those games yourself and refund if needed without worrying about ASF affecting playtime negatively. Please note that if you enable this option then games you purchased from Steam Store won't be farmed by ASF for up to 14 days since redeem date, which will show as nothing to farm if your account doesn't own anything else. If you're unsure whether you want this feature enabled or not, keep it with default value of `false`.

---

### `SteamLogin`

`string`&#8203;型別，預設值為&#8203;`null`&#8203;。 This property defines your steam login - the one you use for logging in to steam. In addition to defining steam login here, you may also keep default value of `null` if you want to enter your steam login on each ASF startup instead of putting it in the config. This may be useful for you if you don't want to save sensitive data in config file.

---

### `SteamMasterClanID`

`ulong`&#8203;型別，預設值為&#8203;`0`&#8203;。 This property defines the steamID of the steam group that bot should automatically join, including its group chat. You can check steamID of your group by navigating to its **[page](https://steamcommunity.com/groups/archiasf)**, then adding `/memberslistxml?xml=1` to the end of the link, so the link will look like **[this](https://steamcommunity.com/groups/archiasf/memberslistxml?xml=1)**. Then you can get steamID of your group from the result, it's in `<groupID64>` tag. In above example it would be `103582791440160998`. In addition to trying to join given group at startup, the bot will also automatically accept group invites to this group, which makes it possible for you to invite your bot manually if your group has private membership. If you don't have any group dedicated for your bots, you should keep this property with default value of `0`.

---

### `SteamParentalCode`

`string`&#8203;型別，預設值為&#8203;`null`&#8203;。 This property defines your steam parental PIN. ASF requires an access to resources protected by steam parental, therefore if you use that feature, you should provide ASF with parental unlock PIN, so it can operate normally. Default value of `null` means that there is no steam parental PIN required to unlock this account, and this is probably what you want if you don't use steam parental functionality.

In limited circumstances, ASF is also able to generate a valid Steam parental code itself, although that requires excessive amount of OS resources and additional time to complete, not to mention that it's not guaranteed to succeed, therefore we recommend to not rely on that feature and instead put valid `SteamParentalCode` in the config for ASF to use. If ASF determines that PIN is required, and it'll be unable to generate one on its own, it'll ask you for input.

---

### `SteamPassword（Steam 密碼）`

`string`&#8203;型別，預設值為&#8203;`null`&#8203;。 This property defines your steam password - the one you use for logging in to steam. In addition to defining steam password here, you may also keep default value of `null` if you want to enter your steam password on each ASF startup instead of putting it in the config. This may be useful for you if you don't want to save sensitive data in config file.

---

### `SteamTradeToken`

`string`&#8203;型別，預設值為&#8203;`null`&#8203;。 When you have your bot on your friend list, then bot can send a trade to you right away without worrying about trade token, therefore you can leave this property at default value of `null`. If you however decide to NOT have your bot on your friend list, then you will need to generate and fill a trade token as the user that this bot is expecting to send trades to. In other words, this property should be filled with trade token of the account that is defined with `Master` permission in `SteamUserPermissions` of **this** bot instance.

In order to find your token, as logged in user with `Master` permission, navigate **[here](https://steamcommunity.com/my/tradeoffers/privacy)** and take a look at your trade URL. The token we're looking for is made out of 8 characters after `&token=` part in your trade URL. You should copy and put those 8 characters here, as `SteamTradeToken`. Do not include whole trading URL, neither `&token=` part, only the token itself (8 characters).

---

### `SteamUserPermissions`

`ImmutableDictionary<ulong, byte>`&#8203;型別，預設值為空。 This property is a dictionary property which maps given Steam user identified by his 64-bit steam ID, to `byte` number that specifies his permission in ASF instance. Currently available bot permissions in ASF are defined as:

| 值 | 名稱            | 描述                                                                                                                                                                                                 |
| - | ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0 | 無             | No special permission, this is mainly a reference value that is assigned to steam IDs missing in this dictionary - there is no need to define anybody with this permission                         |
| 1 | FamilySharing | Provides minimum access for family sharing users. Once again, this is mainly a reference value since ASF is capable of automatically discovering steam IDs that we permitted for using our library |
| 2 | Operator      | Provides basic access to given bot instances, mainly adding licenses and redeeming keys                                                                                                            |
| 3 | Master        | Provides full access to given bot instance                                                                                                                                                         |

In short, this property allows you to handle permissions for given users. Permissions are important mainly for access to ASF **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**, but also for enabling many ASF features, such as accepting trades. For example you may want to set your own account as `Master`, and give `Operator` access to 2-3 of your friends so they can easily redeem keys for your bot with ASF, while **not** being eligible e.g. for stopping it. Thanks to that you can easily assign permissions to given users and let them use your bot to some specified by you degree.

We recommend to set exactly one user as `Master`, and any amount you wish as `Operators` and below. While it's technically possible to set multiple `Masters` and ASF will work correctly with them, for example by accepting all of their trades sent to the bot, ASF will use only one of them (with lowest steam ID) for every action that requires a single target, for example a `loot` request, so also properties like `SendOnFarmingFinished` or `SendTradePeriod`. If you perfectly understand those limitations, especially the fact that `loot` request will always send items to the `Master` with lowest steam ID, regardless of the `Master` that actually executed the command, then you can define multiple users with `Master` permission here, but we still recommend a single master scheme.

It's nice to note that there is one more extra `Owner` permission, which is declared as `SteamOwnerID` global config property. You can't assign `Owner` permission to anybody here, as `SteamUserPermissions` property defines only permissions that are related to the bot instance, and not ASF as a process. For bot-related tasks, `SteamOwnerID` is treated the same as `Master`, so defining your `SteamOwnerID` here is not necessary.

---

### `TradingPreferences`

`byte flags`&#8203;型別，預設值為&#8203;`0`&#8203;。 This property defines ASF behaviour when in trading, and is defined as below:

| 值  | 名稱                  | 描述                                                                                                                                                                                                                    |
| -- | ------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0  | 無                   | No special trading preferences, default                                                                                                                                                                               |
| 1  | AcceptDonations     | Accepts trades in which we're not losing anything                                                                                                                                                                     |
| 2  | SteamTradeMatcher   | Passively participates in **[STM](https://www.steamtradematcher.com)**-like trades. Visit **[trading](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading#steamtradematcher)** for more info                  |
| 4  | MatchEverything     | Requires `SteamTradeMatcher` to be set, and in combination with it - also accepts bad trades in addition to good and neutral ones                                                                                     |
| 8  | DontAcceptBotTrades | Doesn't automatically accept `loot` trades from other bot instances                                                                                                                                                   |
| 16 | MatchActively       | Actively participates in **[STM](https://www.steamtradematcher.com)**-like trades. Visit **[ItemsMatcherPlugin](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/ItemsMatcherPlugin#matchactively)** for more info |

Please notice that this property is `flags` field, therefore it's possible to choose any combination of available values. Check out **[flags mapping](#json-mapping)** if you'd like to learn more. Not enabling any of flags results in `None` option.

For further explanation of ASF trading logic, and description of every available flag, please visit **[trading](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading)** section.

---

### `TransferableTypes`

`ImmutableHashSet<byte>`&#8203;型別，預設值為&#8203;`1, 3, 5`&#8203;的Steam物品類型。 This property defines which Steam item types will be considered for transfering between bots, during `transfer` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. ASF will ensure that only items from `TransferableTypes` will be included in a trade offer, therefore this property allows you to choose what you want to receive in a trade offer that is being sent to one of your bots.

| 值  | 名稱                    | 描述                                                            |
| -- | --------------------- | ------------------------------------------------------------- |
| 0  | Unknown               | Every type that doesn't fit in any of the below               |
| 1  | BoosterPack           | Booster pack containing 3 random cards from a game            |
| 2  | Emoticon              | Emoticon to use in Steam Chat                                 |
| 3  | FoilTradingCard       | Foil variant of `TradingCard`                                 |
| 4  | ProfileBackground     | Profile background to use on your Steam profile               |
| 5  | TradingCard           | Steam trading card, being used for crafting badges (non-foil) |
| 6  | SteamGems             | Steam gems being used for crafting boosters, sacks included   |
| 7  | SaleItem              | Special items awarded during Steam sales                      |
| 8  | Consumable            | Special consumable items that disappear after being used      |
| 9  | ProfileModifier       | Special items that can modify Steam profile appearance        |
| 10 | Sticker               | 可用在 Steam 聊天中的特殊物品                                            |
| 11 | ChatEffect            | 可用在 Steam 聊天中的特殊物品                                            |
| 12 | MiniProfileBackground | Special background for Steam profile                          |
| 13 | AvatarProfileFrame    | Special avatar frame for Steam profile                        |
| 14 | AnimatedAvatar        | Special animated avatar for Steam profile                     |
| 15 | KeyboardSkin          | Steam Deck 的特別鍵盤造型                                            |

Please note that regardless of the settings above, ASF will only ask for Steam (`appID` of 753) community (`contextID` of 6) items, so all game items, gifts and likewise, are excluded from the trade offer by definition.

Default ASF setting is based on the most common usage of the bot, with transfering only booster packs, and trading cards (including foils). The property defined here allows you to alter that behaviour in whatever way that satisfies you. Please keep in mind that all types not defined above will show as `Unknown` type, which is especially important when Valve releases some new Steam item, that will be marked as `Unknown` by ASF as well, until it's added here (in the future release). That's why in general it's not recommended to include `Unknown` type in your `TransferableTypes`, unless you know what you're doing, and you also understand that ASF will send your entire inventory in a trade offer if Steam Network gets broken again and reports all your items as `Unknown`. My strong suggestion is to not include `Unknown` type in the `TransferableTypes`, even if you expect to transfer everything.

---

### `UseLoginKeys`

`bool`&#8203;型別，預設值為&#8203;`true`&#8203;。 This property defines if ASF should use login keys mechanism for this Steam account. Login keys mechanism works very similar to official Steam client's "remember me" option, which makes it possible for ASF to store and use temporary one-time use login key for next logon attempt, effectively skipping a need of providing password, Steam Guard or 2FA code as long as our login key is valid. Login key is stored in `BotName.db` file and updated automatically. This is why you don't need to provide password/SteamGuard/2FA code after logging in successfully with ASF just once.

Login keys are used by default for your convenience, so you don't need to input `SteamPassword`, SteamGuard or 2FA code (when not using **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)**) on each login. It's also superior alternative since login key can be used only for a single time and does not reveal your original password in any way. Exactly the same method is being used by your original Steam client, which saves your account name and login key for your next logon attempt, effectively being the same as using `SteamLogin` with `UseLoginKeys` and empty `SteamPassword` in ASF.

However, some people could be concerned even about this little detail, therefore this option is available here for you if you'd like to ensure that ASF won't store any kind of token that would allow resuming previous session after being closed, which will result in full authentication being mandatory on each login attempt. Disabling this option will work exactly the same as not checking "remember me" in official Steam client. Unless you know what you're doing, you should keep it with default value of `true`.

---

### `UserInterfaceMode`

`byte`&#8203;型別，預設值為&#8203;`0`&#8203;。 This property specifies user interface mode that the bot will be announced with after logging in to Steam network. Currently you can choose one of below modes:

| 值   | 名稱         |
| --- | ---------- |
| `0` | Default    |
| `1` | BigPicture |
| `2` | Mobile     |

If you're not sure how to set this property, leave it with default value of `0`.

---

## 檔案結構

ASF is using quite simple file structure.

```text
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
```

In order to move ASF to new location, for example another PC, it's enough to move/copy `config` directory alone, and that's the recommended way of doing any form of "ASF backups", since you can always download the remaining (program) part from the GitHub, while not risking corrupting internal ASF files, e.g. through a faulty backup.

`log.txt` file holds the log generated by your last ASF run. This file doesn't contain any sensitive information, and is extremely useful when it comes to issues, crashes or simply as an information to you what happened in last ASF run. We will very often ask about this file if you run into issues or bugs. ASF automatically manages this file for you, but you can further tweak ASF **[logging](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Logging)** module if you're advanced user.

`config` directory is the place that holds configuration for ASF, including all of its bots.

`ASF.json` is a global ASF configuration file. This config is used for specifying how ASF behaves as a process, which affects all of the bots and program itself. You can find global properties there, such as ASF process owner, auto-updates or debugging.

`BotName.json` is a config of given bot instance. This config is used for specifying how given bot instance behaves, therefore those settings are specific to that bot only and not shared across other ones. This allows you to configure bots with various different settings and not necessarily all of them working in exactly the same way. Every bot is named using unique identifier, chosen by you in place of `BotName`.

Apart from config files, ASF also uses `config` directory for storing databases.

`ASF.db` is a global ASF database file. It acts as a global persistent storage and is used for saving various information related to ASF process, such as IPs of local Steam servers. **You should not edit this file**.

`BotName.db` is a database of given bot instance. This file is used for storing crucial data about given bot instance in persistent storage, such as login keys or ASF 2FA. **You should not edit this file**.

`BotName.bin` is a special file of given bot instance, which holds information about Steam sentry hash. Sentry hash is used for authenticating using `SteamGuard` mechanism, very similar to Steam `ssfn` file. **You should not edit this file**.

`BotName.keys` is a special file that can be used for importing keys into **[background games redeemer](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer)**. It's not mandatory and not generated, but recognized by ASF. This file is automatically deleted after keys are successfully imported.

`BotName.maFile` is a special file that can be used for importing **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)**. It's not mandatory and not generated, but recognized by ASF if your `BotName` does not use ASF 2FA yet. This file is automatically deleted after ASF 2FA is successfully imported.

---

## JSON 映射

Every configuration property has its type. Type of the property defines values that are valid for it. You can only use values that are valid for given type - if you use invalid value, then ASF won't be able to parse your config.

**We strongly recommend to use ConfigGenerator for generating configs** - it handles most of the low-level stuff (such as types validation) for you, so you only need to input proper values, and you also don't need to understand variable types specified below. This section is mainly for people generating/editing configs manually, so they know what values they can use.

Types used by ASF are native C# types, which are specified below:

---

`bool` - Boolean type accepting only `true` and `false` values.

Example: `"Enabled": true`

---

`byte` - Unsigned byte type, accepting only integers from `0` to `255` (inclusive).

Example: `"ConnectionTimeout": 90`

---

`ushort` - Unsigned short type, accepting only integers from `0` to `65535` (inclusive).

Example: `"WebLimiterDelay": 300`

---

`uint` - Unsigned integer type, accepting only integers from `0` to `4294967295` (inclusive).

---

`ulong` - Unsigned long integer type, accepting only integers from `0` to `18446744073709551615` (inclusive).

Example: `"SteamMasterClanID": 103582791440160998`

---

`string` - String type, accepting any sequence of characters, including empty sequence `""` and `null`. Empty sequence and `null` value are treated the same by ASF, so it's up to your preference which one you want to use (we stick with `null`).

Examples: `"SteamLogin": null`, `"SteamLogin": ""`, `"SteamLogin": "MyAccountName"`

---

`Guid?` - Nullable UUID type, in JSON encoded as string. UUID is made out of 32 hexadecimal characters, in range from `0` to `9` and `a` to `f`. ASF accepts variety of valid formats - lowercase, uppercase, with and without dashes. In addition to valid UUID, since this property is nullable, a special value of `null` is accepted to indicate lack of UUID to provide.

Examples: `"LicenseID": null`, `"LicenseID": "f6a0529813f74d119982eb4fe43a9a24"`

---

`ImmutableList<valueType>` - Immutable collection (list) of values in given `valueType`. In JSON, it's defined as array of elements in given `valueType`. ASF uses `List` to indicate that given property supports multiple values and that their order might be relevant.

Example for `ImmutableList<byte>`: `"FarmingOrders": [15, 11, 7]`

---

`ImmutableHashSet<valueType>` - Immutable collection (set) of unique values in given `valueType`. In JSON, it's defined as array of elements in given `valueType`. ASF uses `HashSet` to indicate that given property makes sense only for unique values and that their order doesn't matter, therefore it'll intentionally ignore any potential duplicates during parsing (if you happened to supply them anyway).

Example for `ImmutableHashSet<uint>`: `"Blacklist": [267420, 303700, 335590]`

---

`ImmutableDictionary<keyType, valueType>` - Immutable dictionary (map) that maps a unique key specified in its `keyType`, to value specified in its `valueType`. In JSON, it's defined as an object with key-value pairs. Keep in mind that `keyType` is always quoted in this case, even if it's value type such as `ulong`. There is also a strict requirement of the key being unique across the map, this time enforced by JSON as well.

Example for `ImmutableDictionary<ulong, byte>`: `"SteamUserPermissions": { "76561198174813138": 3, "76561198174813137": 1 }`

---

`flags` - Flags attribute combines several different properties into one final value by applying bitwise operations. This allows you to choose any possible combination of various different allowed values at the same time. The final value is constructed as a sum of values of all enabled options.

For example, given following values:

| 值 | 名稱 |
| - | -- |
| 0 | 無  |
| 1 | A  |
| 2 | B  |
| 4 | C  |

Using `B + C` would result in value of `6`, using `A + C` would result in value of `5`, using `C` would result in value of `4` and so on. This allows you to create any possible combination of enabled values - if you decided to enable all of them, making `None + A + B + C`, you'd get value of `7`. Also notice that flag with value of `0` is enabled by definition in all other available combinations, therefore very often it's a flag that doesn't enable anything specifically (such as `None`).

So as you can see, in above example we have 3 available flags to switch on/off (`A`, `B`, `C`), and `8` possible values overall:
- `None -> 0`
- `A -> 1`
- `B -> 2`
- `A + B -> 3`
- `C -> 4`
- `A + C -> 5`
- `B + C -> 6`
- `A + B + C -> 7`

Example: `"SteamProtocols": 7`

---

## 相容性映射

Due to JavaScript limitations of being unable to properly serialize simple `ulong` fields in JSON when using web-based ConfigGenerator, `ulong` fields will be rendered as strings with `s_` prefix in the resulting config. This includes for example `"SteamOwnerID": 76561198006963719` that will be written by our ConfigGenerator as `"s_SteamOwnerID": "76561198006963719"`. ASF includes proper logic for handling this string mapping automatically, so `s_` entries in your configs are actually valid and correctly generated. If you're generating configs yourself, we recommend to stick with original `ulong` fields if possible, but if you're unable to do so, you can also follow this scheme and encode them as strings with `s_` prefix added to their names. We hope to resolve this JavaScript limitation eventually.

---

## 設定檔相容性

It's top priority for ASF to remain compatible with older configs. As you should already know, missing config properties are treated the same as they would be defined with their **default values**. Therefore, if new config property gets introduced in new version of ASF, all your configs will remain **compatible** with new version, and ASF will treat that new config property as it'd be defined with its **default value**. You can always add, remove or edit config properties according to your needs.

We recommend to limit defined config properties only to those that you want to change, since this way you automatically inherit default values for all other ones, not only keeping your config clean but also increasing compatibility in case we decide to change a default value for property that you don't want to explicitly set yourself (e.g. `WebLimiterDelay`).

Due to above, ASF will automatically migrate/optimize your configs by reformatting them and removing fields that hold default value. You can disable this behaviour with `--no-config-migrate` **[command-line argument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments#arguments)** if you have a specific reason, for example you're providing read-only config files and you don't want ASF to modify them.

---

## 自動重新載入

Starting with ASF V2.1.6.2+, the program is now aware of configs being modified "on-the-fly" - thanks to that, ASF will automatically:
- Create (and start, if needed) new bot instance, when you create its config
- Stop (if needed) and remove old bot instance, when you delete its config
- Stop (and start, if needed) any bot instance, when you edit its config
- Restart (if needed) the bot under new name, when you rename its config

All of the above is transparent and will be done automatically without a need of restarting the program, or killing other (unaffected) bot instances.

In addition to that, ASF will also restart itself (if `AutoRestart` permits) if you modify core ASF `ASF.json` config. Likewise, program will quit if you delete or rename it.

You can disable this behaviour with `--no-config-watch` **[command-line argument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments#arguments)** if you have a specific reason, for example you don't want from ASF to react to file changes in `config` folder.