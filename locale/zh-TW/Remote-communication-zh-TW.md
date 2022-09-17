# 遠端通訊

本章節詳細說明 ASF 包含的遠端通訊，包括進一步解釋您如何改變它。 雖然我們不認為以下任何內容是惡意或無用的，我們也沒有公開它的法律義務，但我們希望您能更好理解本程式功能，特別是在您的隱私及共享資料的方面。

## Steam

ASF 與 Steam 網路（**[CM 伺服器](https://api.steampowered.com/ISteamDirectory/GetCMList/v1?cellid=0)**）、 **[Steam API](https://steamcommunity.com/dev)**、**[Steam 商店](https://store.steampowered.com)**及 **[Steam 社群](https://steamcommunity.com)**通訊。

停用上述通訊是不可能的，因為它是 ASF 提供基本功能的核心基礎。 如果您對上述內容不滿意，請避免使用 ASF。

## Steam 群組

ASF 與我們的**[Steam 群組](https://steamcommunity.com/groups/archiasf)**通訊。 群組能為您發布公告，特別是新版本、緊急狀況、Steam 問題，及其他對於保持社群更新重要的事情。 它還允許您經由提出問題、解決問題、報告問題或提出改進建議，來獲得我們的技術支援。 預設情形下，ASF 使用的帳號會在登入時自動加入群組。

您可以透過在 Bot 的 **[`RemoteCommunication`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-TW#遠端通訊)** 設定中停用 `SteamGroup` 旗標，來決定退出群組。

## GitHub

ASF 與 **[GitHub 的 API](https://api.github.com)** 通訊，獲取 **[ASF Release](https://github.com/JustArchiNET/ArchiSteamFarm/releases)** 以用於更新功能。 這是自動更新（若您保持啟用 **[`UpdatePeriod`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#updateperiod)**），及 `update` **[指令](https://github. com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-TW)**的一部分。 您可以透過設定 **[`UpdateChannel`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#updatechannel)** 屬性，來影響 ASF 與 GitHub 的通訊：設定成 `None` 將停用整個更新功能，包含此方面的 GitHub 通訊。

## ASF 伺服器

ASF 與**[我們自己的伺服器](https://asf.justarchi.net)**通訊，以提供進階功能。 特別包含了：
- 依據我們自己的獨立資料庫，驗證從 GitHub 下載的 ASF 建置的核對和，以確保所有下載的建置檔案都是正規的（不含惡意程式、MITM 攻擊或其他篡改破壞）
- 如果您在 **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-TW#tradingpreferences)** 中啟用 `SteamTradeMatcher` 並滿足其他準則，則會在**[我們的清單](https://asf.justarchi.net/STM)**中顯示您的 Bot
- 如果您在 **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-TW#tradingpreferences)** 中啟用 `MatchActively` 並滿足其他準則，則會從**[我們的清單](https://asf.justarchi.net/STM)**中下載當前可交易的 Bot 來進行交易

作為一項安全措施，您無法停用 ASF 建置檔案核對和的驗證。 但如果您不想發生這種情況，如上文的 GitHub 章節中所述，您可以完全停用自動更新。

您可以透過在 Bot 的 **[`RemoteCommunication`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-TW#遠端通訊)** 設定中停用 `PublicListing` 旗標，來決定不顯示在清單中。 如果您想執行 `SteamTradeMatcher` Bot 且不被顯示，這可能會有幫助。

`MatchActively` 設定必定會從我們的清單中下載 Bot，如果您不願下載，請停用該設定。

---

## 公開的 ASF STM 清單

我們的公開 ASF STM 清單位於**[我們的網站](https://asf.justarchi.net/STM)**上，為使用 `MatchActively` 的 ASF 使用者及手動匹配的 ASF 與非 ASF 使用者提供公共服務。

請注意，若您未滿足所有需求，您將**不被**顯示於網站。 在這種情形下，ASF 甚至不會與我們的伺服器通訊，因此，若您沒有蓄意啟用 `SteamTradeMatcher` 來幫助自己匹配交易，則可以完全跳過本章節。 此外，公開清單只會與 ASF 最新的穩定版相容，並可能拒絕顯示過時的 Bot，特別是如果它們缺少只能在新版本中找到的核心功能。

### 確切的運作方式

ASF 會在登入後發送一次初始資料，其中包含公開清單使用的所有屬性。 然後每隔 10 分鐘 ASF 會傳送一個非常微小的「心跳」請求，來通知我們的伺服器該 Bot 仍在執行。 如果由於某種原因該心跳未能送達，例如網路問題，那麼 ASF 將每分鐘重新傳送一次，直到登記至伺服器中。

這使我們的網站得以記錄哪些帳號可用於匹配，以及它們是否仍然處於活動狀態。 多虧了這個，我們的網站可以顯示在**過去 15 分鐘**內，所有啟用 ASF 雙重驗證及 STM 的活躍帳號。

使用者會依他們的物品庫排序（按降序排列）：首先是帶有 `Any` 標題，接受所有 1:1 交易的 `MatchEverything` Bot，然後依 `MatchableTypes` 遊戲數量，最後依 `MatchableTypes` 物品數量排序。

### API

ASF STM 清單暫時只接受 ASF Bot。 目前無法在我們的清單中顯示第三方 Bot（因為我們無法輕易地查看它們的程式碼，並保證它們符合我們的整個交易邏輯）。

若您正尋找以程式設計的方式存取我們清單的簡易方法，我們有個非常簡單的 **[/Api/Bots](https://asf.justarchi.net/Api/Bots)** 端點供您使用。 這也是 ASF 在內部為 `MatchActively` 使用者使用的端點。

### 隱私權政策

若您同意顯示於我們的清單中，即如上所述，啟用 `SteamTradeMatcher` 且不拒絕 `PublicListing`，我們將在我們的伺服器上臨時儲存一些您的 Steam 帳號詳細資料，用以提供核心功能。

公開資訊（Steam 向所有相關者公開的）包含：
- 您的 Steam ID（64 位元形式，用於生成連結）
- 您的暱稱（用於顯示）
- 您的頭像（雜湊值，用於顯示）

私人資訊（提供功能所需的特定資料）包含：
- 您的**[交易權杖](https://steamcommunity.com/my/tradeoffers/privacy)**（使非您好友的帳號能向您發起交易提案）
- 您的 `MaxTradeHoldDuration`（使其他人知道您是否願意接受他們的交易）
- 您的 `MatchableTypes`（用於顯示及匹配）
- 您物品庫中的 `MatchableTypes` Steam 物品總數（用於顯示及匹配）
- 構成以上 `MatchableTypes` 個 Steam 物品的遊戲總數（用於顯示及匹配）
- 您的 **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-
TW#交易偏好)** 中 `MatchEverything` 的價值（用於顯示及匹配）

ASF 伺服器**不會**在沒有事先於更新日誌中通知變更及其原因的情形下，收集、儲存或以其他方式處理任何上述未列出的資料。 我們認為上述一切都不是嚴重的事情，我們提到這些是為了讓您知道，ASF 除了您自己設定的功能之外究竟還做了什麼，以便使您更好地了解其流程。

您的資料會在您停止使用我們的清單後的最多 15 分鐘內，自動從公開變成隱藏，不論是因為您更改設定或是關閉 ASF。 除此之外，在上述情形發生後的最多 7 天內，自動從我們的伺服器（包含所有副本備份）上刪除。