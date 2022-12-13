# 注意

本頁面尚未完成。 For translators: you may want to wait a bit (until stable release), as the page is still being written and corrected.

---

# 外掛程式

`ItemsMatcherPlugin` is official ASF plugin that extends ASF with ASF STM listing features. In particular, this includes `PublicListing` in **[`RemoteCommunication`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#remotecommunication)** and `MatchActively` in **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#tradingpreferences)**.

---

## `PublicListing`

我們的公開ASF STM清單位於&#8203;**[我們的網站](https://asf-backend.justarchi.net/STM)**&#8203;上，為使用&#8203;`MatchActively`&#8203;的ASF使用者及手動匹配的ASF與非ASF使用者提供公共服務。

請注意，若您未滿足所有需求，您將&#8203;**不被**&#8203;顯示於網站。 在這種情形下，ASF甚至不會與我們的伺服器通訊，因此，若您沒有蓄意啟用&#8203;`SteamTradeMatcher`&#8203;來幫助自己匹配交易，則可以完全跳過本章節。 此外，公開清單只會與ASF最新的穩定版相容，並可能拒絕顯示過時的Bot，特別是如果它們缺少只能在新版本中找到的核心功能。

### 確切的運作方式

ASF會在登入後發送一次初始資料，其中包含公開清單使用的所有屬性。 然後每隔10分鐘，ASF會傳送一個非常微小的「心跳」請求，來通知我們的伺服器該Bot仍在執行。 如果由於某種原因該心跳未能送達，例如網路問題，那麼ASF將每分鐘重新傳送一次，直到被伺服器記錄。

這使我們的網站得以記錄哪些帳號可用於匹配，以及它們是否仍然處於活動狀態。 多虧了這個，我們的網站可以顯示在&#8203;**過去15分鐘**&#8203;內，所有啟用ASF雙重驗證及STM的活躍帳號。

使用者會依他們的物品庫排序（按降序排列）：首先是帶有&#8203;`Any`&#8203;標題，接受所有1:1交易的&#8203;`MatchEverything`&#8203; Bot，然後依&#8203;`MatchableTypes`&#8203;遊戲數量，最後依&#8203;`MatchableTypes`&#8203;物品數量排序。

### API

ASF STM清單暫時只接受ASF Bot。 目前無法在我們的清單中顯示第三方Bot（因為我們無法輕易地查看它們的程式碼，並保證它們符合我們的整個交易邏輯）。

If you're looking for easy way to access our listing in programmatic way, we have a very simple **[`/Api/Listing/Bots`](https://asf-backend.justarchi.net/Api/Listing/Bots)** endpoint that you can use. 這也是ASF在內部為&#8203;`MatchActively`&#8203;使用者使用的端點。

### 隱私權政策

若您同意顯示於我們的清單中，即如上所述，啟用&#8203;`SteamTradeMatcher`&#8203;且不拒絕&#8203;`PublicListing`&#8203;，我們將在我們的伺服器上臨時儲存一些您的Steam帳號詳細資料，用以提供核心功能。

公開資訊（Steam向所有相關者公開的）包括：
- 您的Steam ID（64位元形式，用於生成連結）
- 您的暱稱（用於顯示）
- 您的頭像（雜湊值，用於顯示）

私人資訊（提供功能所需的特定資料）包括：
- Your **[inventory](https://steamcommunity.com/my/inventory/#753_6)** limited to item types that you've picked in `MatchableTypes` (so people can use `MatchActively` against your items).
- 您的&#8203;**[交易權杖](https://steamcommunity.com/my/tradeoffers/privacy)**&#8203;（使非您好友的帳號能向您發起交易提案）
- 您的&#8203;`MaxTradeHoldDuration`&#8203;（使其他人知道您是否願意接受他們的交易）
- 您的&#8203;`MatchableTypes`&#8203;（用於顯示及匹配）
- Total number of Steam items in your inventory (for display purposes and matching)

---

## `MatchActively`

`MatchActively` setting is active version of **[`SteamTradeMatcher`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading#steamtradematcher)** which includes interactive matching in which the bot will send trades to other people. 它可以單獨運作，亦可結合&#8203;`SteamTradeMatcher`&#8203;設定一起運作。 This feature requires `LicenseID` to be set, as it uses third-party server.

為了使用這個選項，您需要滿足一系列的需求。 您至少應保證帳號&#8203;**[不受限制](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663)**&#8203;、啟用&#8203;**[ASF雙重驗證](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-zh-TW#asf-雙重驗證)**&#8203;，並在&#8203;`MatchableTypes`&#8203;中設定至少一種有效類型，例如交換卡片。

若您滿足了上述需求，ASF將會定期與我們的&#8203;**[公開的ASF STM清單](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Remote-communication-zh-TW#公開的-asf-stm-清單)**&#8203;通訊，以主動匹配當前可用的Bot。

- 每輪中，ASF 都會擷取我們的物品庫，並列出的所選Bot的物品庫，以便找到可以匹配的&#8203;`MatchableTypes`&#8203;物品。 若找到合適的配對，ASF將自動發送並確認交易提案。
- 每套物品（具相同appID、類型及稀有度）在一輪中只能匹配一次。 這是為了盡可能減少「物品不再能用」的情形，無需在發送所有交易之前，等待每個Bot做出回應。 這也是為什麼以輪次來匹配，而不是持續進行的主要原因。
- ASF不會在單次交易中送出超過&#8203;`255`&#8203;個物品，且每個輪次中不會向單個使用者送出超過&#8203;`5`&#8203;個交易提案。 這是由Steam的限制，及我們自己的負載平衡所加上的。

這個模組應是透明的。 匹配將在ASF啟動後約&#6203;`1`&#8203;個小時後開始，並且每&#8203;`8`&#8203;個小時重複一次（如果需要）。 `MatchActively`&#8203;功能旨在作為一項長期的定期措施，以確保我們積極地向收集完成的方向前進，但若將此作為指令使用，就會造成短期的時間及資源壓力。 這個模組的目標使用者是主要帳號及「用於存放物品的」備用帳號，但亦可被任何未設定成&#8203;`MatchEverything`&#8203;的Bot使用。

ASF會盡力將使用該選項產生的請求量及壓力降至最低，同時盡可能提高匹配效率。 匹配Bot及組織整個流程的演算法是ASF的實作細節，並且可以依據回饋、實際情形及未來的想法而改變。

當前版本的演算法會使ASF優先考慮&#8203;`Any`&#8203;標籤的Bot，特別是那些擁有更多種遊戲物品的Bot。 當&#8203;`Any`&#8203;的Bot用完後，ASF將依據相同遊戲物品的規則平均匹配Bot。與其他Bot相比，擁有過多物品的Bot更有可能出現物品庫相關問題，而被進一步降低優先級。 無論如何，ASF將至少嘗試匹配一次每個可用的Bot，以保證我們不會錯過可能的徽章套卡進度。

`MatchActively`&#8203;會考慮您使用交易&#8203;**[指令](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-TW)**&#8203;`tbadd`&#8203;加入黑名單的Bot帳號，且不會嘗試匹配它們。 這能用來告訴ASF它不該匹配哪些Bot，即使它們可能有重複物品能提供我們使用。

---

### Why do I need a `LicenseID` to use the plugin? Wasn't `MatchActively` free before?

ASF is, and remains, free and open-source, as it was established at the start of the project back in October 2015. Our program is also entirely non-commercial, we do not earn anything from contributions to it, building or publishing. Over those past 7+ years ASF has received tremendous amount of development, and it's still being improved and enhanced with every monthly stable release mostly by a single person, **[JustArchi](https://github.com/JustArchi)** - with no strings attached. The only funding we receive is from non-obligatory donations that come from our users.

For a very long time, until October 2022, `MatchActively` feature was part of ASF core and available for everyone to use. In October 2022, Valve, the company behind Steam, has put very severe rate limits that rendered previous functionality entirely broken, with no solution available. The feature therefore has been removed from ASF core in version 5.4.1.0.

`MatchActively` was resurrected as part of official `ItemsMatcher` plugin that further enhances ASF with active cards matching functionality. Resurrecting `MatchActively` feature required from us extraordinary amount of work to create ASF backend, entirely new service hosted on a server, with more than a thousand of proxies attached for resolving inventories, all exclusively to allow ASF clients to make use of `MatchActively` like before. Due to the amount of work involved, as well as resources that are not free and require to be paid on monthly basis by us (domain, server, proxies), we've decided to offer this plugin to our sponsors, that is, people that already support ASF project on monthly basis. Our goal isn't to profit from it, but rather, cover the **monthly costs** that are exclusively linked with offering this functionality - that's why we offer it basically for nothing, but we do have to charge a little for it as we can't pay hundreds of dollars from our own pockets just to make it available for you. We hope that you understand.

---

### How can I get an access?

`ItemsMatcher` is offered as part of $5+ sponsor tier on **[JustArchi's GitHub](https://github.com/sponsors/JustArchi)**. Simply become a sponsor of $5 tier (or higher), then click **[here](https://asf-backend.justarchi.net/user/status)** to obtain your `LicenseID`. You'll need to sign in with GitHub for confirming your identity.

The license allows you to send limited amount of requests to the server. $5 tier allows you to use `MatchActively` for one account, which should be suitable for majority of people. $10 tier allows you to use it on three accounts. If you require more resources, **[let us know](mailto:ASF@JustArchi.net)**.

`LicenseID` is made out of 32 hexadecimal characters, such as `f6a0529813f74d119982eb4fe43a9a24`. Simply put it in `LicenseID` property of ASF global config.