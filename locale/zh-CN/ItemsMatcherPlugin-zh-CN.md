# ItemsMatcherPlugin

`ItemsMatcherPlugin` is official ASF **[plugin](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins)** that extends ASF with ASF STM listing features. 此插件主要包括 **[`RemoteCommunication`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-CN#remotecommunication)** 中的 `PublicListing` 和 **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-CN#tradingpreferences)** 中的 `MatchActively`。 ASF comes with `ItemsMatcherPlugin` bundled together with the release, therefore it's ready for usage right away.

---

## `PublicListing（公共列表）`

Public listing, as the name implies, is listing of currently available ASF STM bots. It's located on **[our website](https://asf.justarchi.net/STM)**, managed automatically and used as a public service for both ASF users that make use of `MatchActively`, as well as ASF and non-ASF users for manual matching.

While `PublicListing` is enabled by default, please note that you will **not** be displayed on the website if you do not meet all of the requirements, especially `SteamTradeMatcher`, which isn't enabled by default. For people that do not meet the criteria, even if they kept `PublicListing` enabled, ASF doesn't communicate with the server in any way. Public listing is also compatible only with latest stable version of ASF and may refuse to display outdated bots, especially if they're missing core functionality that can be found only in newer versions.

### 运作方式

ASF 会在登录后发送一次初始数据，其中包含公共列表使用的所有属性。 然后，每隔 10 分钟，ASF 会发送一个非常小的“心跳”请求，通知我们的服务器机器人仍然在线并运行。 如果由于某种原因心跳包未能送达，例如网络问题，ASF 将每分钟重试发送一次，直到服务器确认收到它。 This way our server knows precisely which bots are still running and ready to accept trade offers. ASF will also send initial announcement on as-needed basis, for example if it detects that our inventory has changed since the previous one.

We display all ASF 2FA+STM accounts that were active in the **last 15 minutes**. Users are sorted according to their relative usefulness - `MatchEverything` bots which are shown with `Any` banner that accept all 1:1 trades, then unique games count, and finally items count.

### API

ASF STM 列表暂时只接受 ASF 机器人。 There is no way to list third-party bots on our listing for now, as we can't review their code easily and ensure they meet our entire trading logic. Participation in the listing therefore requires latest stable ASF version, although it can run with custom **[plugins](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins)**.

For consumers of the listing, we have a very simple **[`/Api/Listing/Bots`](https://asf.justarchi.net/Api/Listing/Bots)** endpoint that you can use. It includes all the data we have, apart from inventories of users which are part of `MatchActively` feature exclusively.

### 隐私政策

If you agree to being listed in our listing, by enabling `SteamTradeMatcher` and not refusing `PublicListing`, as specified above, we'll temporarily store some of your Steam account details on our server in order to provide the expected functionality.

公开信息（由 Steam 暴露给任何感兴趣的人 ）包括：
- 您的 Steam ID（64 位形式，用于生成链接）
- 您的昵称（用于显示目的）
- 您的头像（经过哈希，用于显示目的）

Semi-public info (exposed by Steam to every interested party if you meet listing requirements) includes:
- 您的库存（使其他用户能够对您的物品使用 `MatchActively`）

私密信息（提供功能所需的特定数据）包括：
- 您的[**交易令牌**](https://steamcommunity.com/my/tradeoffers/privacy)（使不是您好友的人可以向您发送交易报价）
- Your `MatchableTypes` setting (for display purposes and matching)
- 您的 `MatchEverything` 设置（用于显示和匹配目的）
- Your `MaxTradeHoldDuration` setting (so other people know whether you're willing to accept their trades)

---

## `MatchActively（主动匹配）`

`MatchActively` setting is active version of **[`SteamTradeMatcher`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading#steamtradematcher)** including interactive matching in which the bot will send trades to other people. 它可以单独运行，也可以与 `SteamTradeMatcher` 设置一起运行。 This feature requires `LicenseID` to be set, as it uses third-party server and paid resources to operate.

为了使用该选项，您需要满足一系列需求。 您至少应该保证帐户[**不受限**](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663)、**[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-zh-CN#asf-两步验证)** 启用，并且在 `MatchableTypes` 中设置了至少一种有效的类型，例如集换式卡牌。

If you meet all of the requirements above, ASF will periodically communicate with our **[public ASF STM listing](#publiclisting)** in order to actively match bots that are currently available.

- In each round ASF will fetch our inventory and inventory of all available bots listed in order to find `MatchableTypes` items that can be matched. Thanks to communicating directly with our server, this process requires a single request and we have immediate information whether any available bot offers something interesting for us - if match is found, ASF will send and confirm trade offer automatically.
- 每套物品（相同 appID、物品类型和稀有程度的组合为一套）只能在一轮中匹配一次。 这是为了尽可能减少“物品当前不能用于交易”的情况，并且无需在发出所有交易报价之前等待每个机器人作出回应。 这也是匹配由多个轮次组成而不是持续进行的主要原因。
- ASF 不会在单个交易报价中发送超过 `255` 个物品，每轮中不会向同一个用户发送超过 `5` 个交易报价。 这个强制限制来自于 Steam 和我们的负载平衡机制。

这一模块应该是透明的。 匹配过程会在 ASF 启动后大约 `1` 小时后开始，并且每 `6` 小时重复一次（如果有需要）。 `MatchActively` 功能旨在作为一种长期的周期性措施，确保我们向集齐卡牌套组的方向前进，但如果我们将其作为命令提供，就会造成突发的时间与资源压力。 此模块的目标用户是主帐户和用于存储的子帐户，但也可以用于任何没有设置 `MatchEverything` 的机器人。

ASF 会尽力减少由此选项带来的请求和压力，同时将匹配的效率提升至极限。 用于匹配机器人以及组织整个流程的算法是 ASF 的实现细节，可能会根据用户反馈、具体情况和未来的想法进行更改。

当前版本的算法使 ASF 优先匹配有 `Any` 标记的机器人，特别是物品所属游戏数更多的机器人。 When running out of `Any` bots, ASF will move on to the `Fair` ones upon same diversity rule. ASF will try to match every available bot at least once, to ensure that we're not missing on a possible set progress.

`MatchActively` 支持交易黑名单，您可以通过 `tbadd` [**命令**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-CN)向其中添加机器人的帐户，您的机器人将不会尝试与黑名单中的机器人主动匹配。 这告诉 ASF 永远不匹配这些机器人，即使这些机器人有我们可能需要的卡牌。

---

### Why do I need a `LicenseID` to use `MatchActively`? Wasn't it free before?

ASF is, and remains, free and open-source, as it was established at the start of the project back in October 2015. Our program is also entirely non-commercial, we do not earn anything from contributions to it, building or publishing. Over those past 7+ years ASF has received tremendous amount of development, and it's still being improved and enhanced with every monthly stable release mostly by a single person, **[JustArchi](https://github.com/JustArchi)** - with no strings attached. The only funding we receive is from non-obligatory donations that come from our users.

For a very long time, until October 2022, `MatchActively` feature was part of ASF core and available for everyone to use. In October 2022, Valve, the company behind Steam, has put very severe rate limits that rendered previous functionality entirely broken, with no solution available. The feature therefore had to be removed from ASF core in version 5.4.1.0.

`MatchActively` was resurrected as part of official `ItemsMatcher` plugin that further enhances ASF with active cards matching functionality. Resurrecting `MatchActively` feature required from us **extraordinary amount of work** to create ASF backend, entirely new service hosted on a server, with more than a thousand of proxies attached for resolving inventories, all exclusively to allow ASF clients to make use of `MatchActively` like before. Due to the amount of work involved, as well as resources that are not free and require to be paid on monthly basis by us (domain, server, proxies), we've decided to offer this functionality to our sponsors, that is, people that already support ASF project on monthly basis. Our goal isn't to profit from it, but rather, cover the **monthly costs** that are exclusively linked with offering this option - that's why we offer it basically for nothing, but we do have to charge a little for it as we can't pay hundreds of dollars from our own pockets just to make it available for you. We hope that you understand.

---

### 如何获取访问权限？

`ItemsMatcher` is offered as part of $5+ sponsor tier on **[JustArchi's GitHub](https://github.com/sponsors/JustArchi)**. Simply become a sponsor of $5 tier (or higher), then read **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#licenseid)** section to obtain and fill `LicenseID`.

The license allows you to send limited amount of requests to the server. $5 tier allows you to use `MatchActively` for one account, which should be suitable for majority of people. $10 tier allows you to use it on three accounts. If you require more resources, **[let us know](mailto:ASF@JustArchi.net)**.