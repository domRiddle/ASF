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
- Announcing your bot in **[our listing](https://asf.justarchi.net/STM)** if you've enabled `SteamTradeMatcher` in **[`TradingPreferences`](#tradingpreferences)** and meet other criteria
- Downloading currently available bots to trade from **[our listing](https://asf.justarchi.net/STM)** if you've enabled `MatchActively` in **[`TradingPreferences`](#tradingpreferences)** and meet other criteria

As a security measure, it's not possible to disable checksum verification for ASF builds. However, you can disable auto-updates entirely if you'd like to avoid this, as described above in the GitHub section.

You can decide to opt-out of being announced in the listing by disabling `PublicListing` flag in bot's **[`RemoteCommunication`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#remotecommunication)** settings. This might be useful if you'd like to run `SteamTradeMatcher` bot without being announced at the same time.

Downloading bots from our listing is mandatory for `MatchActively` setting, you'll need to disable that setting if you're unwilling to accept that.

---

## 公共 ASF STM 列表

我们的公共 ASF STM 列表位于&#8203;**[我们的网站](https://asf.justarchi.net/STM)**，它是一项面向启用 `MatchActively` 的 ASF 用户的公开服务，同时也供 ASF 用户与非 ASF 用户进行手动匹配。

### 运作方式

ASF 会在登录后发送一次初始数据，其中包含公共列表使用的所有属性。 然后，每隔 10 分钟，ASF 会发送一个非常小的“心跳”请求，通知我们的服务器机器人仍然在线并运行。 如果由于某种原因心跳包未能送达，例如网络问题，ASF 将每分钟重试发送一次，直到服务器确认收到它。

这使我们的网站可以记录哪些帐户可用于匹配，以及它们是否仍然在线。 借助这一功能，我们的网站可以显示在**过去 15 分钟**内在线的所有 ASF 2FA+STM 帐户。

用户将会按照他们的库存降序排序——排在最上方的是启用 `MatchEverything` 的、接受所有 1:1 交易的、带有 `Any` 标记的机器人，然后再按 `MatchableTypes` 游戏数排序，最后按 `MatchableTypes` 物品数排序。

请注意，如果您没有满足所有要求，将**不会**被显示在网站上。 在这种情况下，ASF 甚至不会与我们的服务器通信，因此，如果您没有启用 `SteamTradeMatcher` 以帮助自己匹配交易，则上述的第二点会被完全跳过。 此外，公开列表仅与最新稳定版本的 ASF 兼容，并且可能拒绝显示过时的机器人，特别是如果它们缺少只能在较新版本中找到的核心功能。

### API

ASF STM 列表暂时只接受 ASF 机器人。 目前无法在我们的列表中列出第三方机器人（因为我们无法轻松检查它们的代码，并确保它们符合我们的交易逻辑）。

如果您正在寻找以编程方式访问我们的列表的方法，我们为您提供了一个简单的 **[/Api/Bots](https://asf.justarchi.net/Api/Bots)** 端点。 这也是 ASF 内部为 `MatchActively` 用户使用的端点。

### Privacy policy

If you agree to being listed in our listing, by enabling `SteamTradeMatcher` and not refusing `PublicListing`, as specified above, we'll temporarily store some of your Steam account details on our server in order to provide the core functionality.

Public info (exposed by Steam to every interested party) includes:
- 您的 Steam ID（64 位形式，用于生成链接）
- 您的昵称（用于显示目的）
- 您的头像（经过哈希，用于显示目的）
- 您库存中 `MatchableTypes` 物品的总数（用于显示和匹配目的）
- 提供上述 `MatchableTypes` 物品的游戏总数（用于显示和匹配目的）

Private info (selected data required for providing the functionality) includes:
- 您的&#8203;**[交易令牌](https://steamcommunity.com/my/tradeoffers/privacy)**（使不是您好友的人可以向您发送交易报价）
- 您的 `MatchableTypes`（用于显示和匹配目的）
- Value of `MatchEverything` in your **[`TradingPreferences`](#tradingpreferences)** (for display purposes and matching)

ASF server will **not** collect, store or otherwise process any other data not listed above, without prior important notice in the changelog, and a very good practical reason in the first place. We do not consider anything above to be a serious matter, and we mention it to let you know what precisely ASF does apart of what you configured it to do yourself, so people can better understand the process.

Your data will be automatically hidden from general public in up to 15 minutes since the moment you stop using our listing, whether due to change of settings or not having ASF launched anymore. In addition to that, it'll be automatically deleted from our server (including all backup copies) in up to 7 days since the above happening.