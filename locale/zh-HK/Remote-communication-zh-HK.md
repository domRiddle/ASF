# Remote communication

This section elaborates on remote communication that ASF includes, including further explanation on how one can influence it. While we don't consider anything below as malicious or otherwise unwanted, and neither we're legally obliged to disclose it, we want you to better understand the program functionality especially in regards to your privacy and data being shared.

## Steam

ASF communicates with Steam network (**[CM servers](https://api.steampowered.com/ISteamDirectory/GetCMList/v1?cellid=0)**), as well as **[Steam API](api.steampowered.com)**, **[Steam store](https://store.steampowered.com)** and **[Steam community](https://steamcommunity.com)**.

It's not possible to disable any of the above communication, as it's the core foundation ASF is based on in order to provide its basic functionality. You'll need to refrain from using ASF if you're not comfortable with the above.

## Steam group

ASF communicates with our **[Steam group](https://steamcommunity.com/groups/archiasf)**. The group provides you with announcements, especially new versions, critical issues, Steam problems and other things that are important to keep community updated. It also allows you to use our technical support, by asking questions, resolving problems, reporting issues or suggesting improvements. By default, accounts used in ASF will automatically join the group upon login.

You can decide to opt-out of joining the group by disabling `SteamGroup` flag in bot's **[`RemoteCommunication`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#remotecommunication)** settings.

## GitHub

ASF communicates with **[GitHub's API](https://api.github.com)** in order to fetch **[ASF releases](https://github.com/JustArchiNET/ArchiSteamFarm/releases)** for the update functionality. This is done as part of auto-updates (if you've kept **[`UpdatePeriod`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#updateperiod)** enabled), as well as `update` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. You can influence ASF's communication with GitHub through **[`UpdateChannel`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#updatechannel)** property - setting it to `None` will result in disabling entire update functionality, including GitHub communication in this regard.

## ASF's server

ASF communicates with **[our own server](https://asf.justarchi.net)** for more advanced functionality. In particular, this includes:
- Verifying checksums of ASF builds downloaded from GitHub against our own independent database to ensure that all downloaded builds are legitimate (free of malware, MITM attacks or other tampering)
- Announcing your bot in **[our listing](https://asf.justarchi.net/STM)** if you've enabled `SteamTradeMatcher` in **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#tradingpreferences)** and meet other criteria
- Downloading currently available bots to trade from **[our listing](https://asf.justarchi.net/STM)** if you've enabled `MatchActively` in **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#tradingpreferences)** and meet other criteria

As a security measure, it's not possible to disable checksum verification for ASF builds. However, you can disable auto-updates entirely if you'd like to avoid this, as described above in the GitHub section.

You can decide to opt-out of being announced in the listing by disabling `PublicListing` flag in bot's **[`RemoteCommunication`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#remotecommunication)** settings. This might be useful if you'd like to run `SteamTradeMatcher` bot without being announced at the same time.

Downloading bots from our listing is mandatory for `MatchActively` setting, you'll need to disable that setting if you're unwilling to accept that.

---

## 公開 ASF STM 清單

我們的公開 ASF STM 清單位于**[此處](https://asf.justarchi.net/STM)**，它被用作使用` MatchActively `的ASF用戶的公共服務，以及幫助ASF和非ASF用戶進行手動匹配。

### 工作原理

登錄後，ASF 會發送一次初始數據，其中包含公開清單會使用的所有屬性。 然後，每10分鐘，ASF 發送一個非常小的心跳請求，通知我們的伺服器機械人仍在運行。 如果由於某種原因心跳訊號沒有到達，例如由於網絡問題，ASF 將每分鐘重試發送一次，直到伺服器註冊它。

這允許我們的網站記錄哪些帳戶可用於匹配，以及它們是否仍處於活動狀態。 多虧了這一點，我們的網站可以顯示在**過去15分鐘**中活躍的所有啟用 ASF 2FA 和 STM 的帳戶。

用戶按照他們的庫存（按降序排序）——首先是` MatchEverything `機械人，`Any`橫幅意味著它接受所有1：1交易，然後是符合` MatchableTypes `的遊戲計數，最後是符合` MatchableTypes `的物品計數。

請注意，如果您未符合所有要求，您將**不會**在網站上顯示 。 在這種情況下，ASF甚至不會費心與我們的服務器通信，因此，如果您沒有啟用` SteamTradeMatcher `以幫助自己匹配冗餘物品，則會完全跳過第二點。 Also public listing is compatible only with latest stable version of ASF and may refuse to display outdated bots, especially if they're missing core functionality that can be found only in newer versions.

### API

ASF STM 清單暫時只接受 ASF 機械人。 目前無法在我們的清單中列出第三方機器人（因為我們無法輕鬆查看其代碼並確保它們符合我們的整個交易邏輯）。

如果您正在尋找以編程方式訪問我們清單的簡便方法，我們有一個非常簡單的**[/Api/Bots](https://asf.justarchi.net/Api/Bots)**端點供您使用。 這也是ASF在內部用於` MatchActively `用戶的端點。

### Privacy policy

If you agree to being listed in our listing, by enabling `SteamTradeMatcher` and not refusing `PublicListing`, as specified above, we'll temporarily store some of your Steam account details on our server in order to provide the core functionality.

Public info (exposed by Steam to every interested party) includes:
- 您的Steam身份驗證器（64位形式，用於生成連結）
- 您的昵稱（用於顯示）
- 您的頭像（哈希，用於顯示）
- 您庫存里符合`MatchableTypes`的 Steam 物品總數量（用於顯示和匹配）
- 符合`MatchableTypes`的 Steam 物品所屬遊戲總數（用於顯示和匹配）

Private info (selected data required for providing the functionality) includes:
- 您的**[交易代碼](https://steamcommunity.com/my/tradeoffers/privacy)**（用於允許不在您好友名單中的用戶對您發起交易）
- 您的 `匹配類型`（用於顯示和匹配）
- Value of `MatchEverything` in your **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#tradingpreferences)** (for display purposes and matching)

ASF server will **not** collect, store or otherwise process any other data not listed above, without prior important notice in the changelog, and a very good practical reason in the first place. We do not consider anything above to be a serious matter, and we mention it to let you know what precisely ASF does apart of what you configured it to do yourself, so people can better understand the process.

Your data will be automatically hidden from general public in up to 15 minutes since the moment you stop using our listing, whether due to change of settings or not having ASF launched anymore. In addition to that, it'll be automatically deleted from our server (including all backup copies) in up to 7 days since the above happening.