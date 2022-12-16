# 注意

本页面尚未完成。 翻译者：建议您暂时等待（直到稳定版发布），因为此页面仍在编写和修改。

---

# 插件

`ItemsMatcherPlugin` 是 ASF 官方插件，为 ASF 带来 ASF STM 列表功能。 此插件主要包括 **[`RemoteCommunication`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-CN#remotecommunication)** 中的 `PublicListing` 和 **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-CN#tradingpreferences)** 中的 `MatchActively`。

---

## `PublicListing（公共列表）`

我们的公共 ASF STM 列表位于[**我们的网站**](https://asf-backend.justarchi.net/STM)，它是一项面向启用 `MatchActively` 的 ASF 用户的公开服务，同时也供 ASF 用户与非 ASF 用户进行手动匹配。

请注意，如果您没有满足所有要求，将**不会**被显示在网站上。 在这种情况下，ASF 甚至不会与我们的服务器通信，因此，如果您没有启用 `SteamTradeMatcher` 以帮助自己匹配交易，则这一小节会被完全跳过。 此外，公开列表仅与最新稳定版本的 ASF 兼容，并且可能拒绝显示过时的机器人，特别是如果它们缺少只能在较新版本中找到的核心功能。

### 运作方式

ASF 会在登录后发送一次初始数据，其中包含公共列表使用的所有属性。 然后，每隔 10 分钟，ASF 会发送一个非常小的“心跳”请求，通知我们的服务器机器人仍然在线并运行。 如果由于某种原因心跳包未能送达，例如网络问题，ASF 将每分钟重试发送一次，直到服务器确认收到它。

这使我们的网站可以记录哪些帐户可用于匹配，以及它们是否仍然在线。 借助这一功能，我们的网站可以显示在**过去 15 分钟**内在线的所有 ASF 2FA+STM 帐户。

用户将会按照他们的库存降序排序——排在最上方的是启用 `MatchEverything` 的、接受所有 1:1 交易的、带有 `Any` 标记的机器人，然后再按 `MatchableTypes` 游戏数排序，最后按 `MatchableTypes` 物品数排序。

### API

ASF STM 列表暂时只接受 ASF 机器人。 目前无法在我们的列表中列出第三方机器人（因为我们无法轻松检查它们的代码，并确保它们符合我们的交易逻辑）。

如果您正在寻找以编程方式访问我们的列表的方法，我们为您提供了一个简单的 **[`/Api/Listing/Bots`](https://asf-backend.justarchi.net/Api/Listing/Bots)** 端点。 这也是 ASF 内部为 `MatchActively` 用户使用的端点。

### 隐私政策

如果您同意在我们的列表中展示，即如上所述，启用 `SteamTradeMatcher`，并且不拒绝 `PublicListing`，我们会在服务器中临时存储一些您的 Steam 帐户数据，以便提供核心功能。

公开信息（由 Steam 暴露给任何感兴趣的人 ）包括：
- 您的 Steam ID（64 位形式，用于生成链接）
- 您的昵称（用于显示目的）
- 您的头像（经过哈希，用于显示目的）

私密信息（提供功能所需的特定数据）包括：
- 符合您设置的 `MatchableTypes` 类型的库存物品（使其他用户能够对您的物品使用 `MatchActively`）。
- 您的[**交易令牌**](https://steamcommunity.com/my/tradeoffers/privacy)（使不是您好友的人可以向您发送交易报价）
- 您的 `MaxTradeHoldDuration`（使其他人了解您是否愿意接受他们的交易）
- 您的 `MatchableTypes`（用于显示和匹配目的）
- 您库存中 Steam 物品的总数（用于显示和匹配目的）

---

## `MatchActively（主动匹配）`

`MatchActively`（主动匹配）是 **[`SteamTradeMatcher`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading-zh-CN#steamtradematcher)** 的主动版本，包括互动式匹配，机器人同时会向其他人发送交易报价。 它可以单独运行，也可以与 `SteamTradeMatcher` 设置一起运行。 此功能需要设置 `LicenseID`，因为它会使用第三方服务器。

为了使用该选项，您需要满足一系列需求。 您至少应该保证帐户[**不受限**](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663)、**[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-zh-CN#asf-两步验证)** 启用，并且在 `MatchableTypes` 中设置了至少一种有效的类型，例如集换式卡牌。

如果您满足上述所有要求，ASF 将会定期与我们的[**公共 ASF STM 列表**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Remote-communication#公共-asf-stm-列表)通信，以主动匹配当前在线的机器人。

- 每一轮，ASF 将会获取我们的库存与列表中选定机器人的库存，以寻找可匹配的 `MatchableTypes` 物品。 如果找到合适的匹配，ASF 将会自动发送并确认交易报价。
- 每套物品（相同 appID、物品类型和稀有程度的组合为一套）只能在一轮中匹配一次。 这是为了尽可能减少“物品当前不能用于交易”的情况，并且无需在发出所有交易报价之前等待每个机器人作出回应。 这也是匹配由多个轮次组成而不是持续进行的主要原因。
- ASF 不会在单个交易报价中发送超过 `255` 个物品，每轮中不会向同一个用户发送超过 `5` 个交易报价。 这个强制限制来自于 Steam 和我们的负载平衡机制。

这一模块应该是透明的。 匹配过程会在 ASF 启动后大约 `1` 小时后开始，并且每 `6` 小时重复一次（如果有需要）。 `MatchActively` 功能旨在作为一种长期的周期性措施，确保我们向集齐卡牌套组的方向前进，但如果我们将其作为命令提供，就会造成突发的时间与资源压力。 此模块的目标用户是主帐户和用于存储的子帐户，但也可以用于任何没有设置 `MatchEverything` 的机器人。

ASF 会尽力减少由此选项带来的请求和压力，同时将匹配的效率提升至极限。 用于匹配机器人以及组织整个流程的算法是 ASF 的实现细节，可能会根据用户反馈、具体情况和未来的想法进行更改。

当前版本的算法使 ASF 优先匹配有 `Any` 标记的机器人，特别是物品所属游戏数更多的机器人。 在耗尽 `Any` 机器人后，ASF 会按照相同的游戏数规则开始匹配平衡机器人，由于拥有过多物品的机器人更有可能出现库存问题，这些机器人会被进一步降低优先级。 无论如何，ASF 将尝试匹配每个可用的机器人至少一次，以确保我们不会错过可能的物品套组进度。

`MatchActively` 支持交易黑名单，您可以通过 `tbadd` [**命令**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-CN)向其中添加机器人的帐户，您的机器人将不会尝试与黑名单中的机器人主动匹配。 这告诉 ASF 永远不匹配这些机器人，即使这些机器人有我们可能需要的卡牌。

---

### 为什么我需要 `LicenseID` 才能使用此插件？ `MatchActively` 之前不是免费的吗？

ASF is, and remains, free and open-source, as it was established at the start of the project back in October 2015. Our program is also entirely non-commercial, we do not earn anything from contributions to it, building or publishing. Over those past 7+ years ASF has received tremendous amount of development, and it's still being improved and enhanced with every monthly stable release mostly by a single person, **[JustArchi](https://github.com/JustArchi)** - with no strings attached. The only funding we receive is from non-obligatory donations that come from our users.

For a very long time, until October 2022, `MatchActively` feature was part of ASF core and available for everyone to use. In October 2022, Valve, the company behind Steam, has put very severe rate limits that rendered previous functionality entirely broken, with no solution available. The feature therefore has been removed from ASF core in version 5.4.1.0.

`MatchActively` was resurrected as part of official `ItemsMatcher` plugin that further enhances ASF with active cards matching functionality. Resurrecting `MatchActively` feature required from us extraordinary amount of work to create ASF backend, entirely new service hosted on a server, with more than a thousand of proxies attached for resolving inventories, all exclusively to allow ASF clients to make use of `MatchActively` like before. Due to the amount of work involved, as well as resources that are not free and require to be paid on monthly basis by us (domain, server, proxies), we've decided to offer this plugin to our sponsors, that is, people that already support ASF project on monthly basis. Our goal isn't to profit from it, but rather, cover the **monthly costs** that are exclusively linked with offering this functionality - that's why we offer it basically for nothing, but we do have to charge a little for it as we can't pay hundreds of dollars from our own pockets just to make it available for you. We hope that you understand.

---

### 如何获取访问权限？

`ItemsMatcher` is offered as part of $5+ sponsor tier on **[JustArchi's GitHub](https://github.com/sponsors/JustArchi)**. Simply become a sponsor of $5 tier (or higher), then click **[here](https://asf-backend.justarchi.net/user/status)** to obtain your `LicenseID`. You'll need to sign in with GitHub for confirming your identity.

The license allows you to send limited amount of requests to the server. $5 tier allows you to use `MatchActively` for one account, which should be suitable for majority of people. $10 tier allows you to use it on three accounts. If you require more resources, **[let us know](mailto:ASF@JustArchi.net)**.

`LicenseID` is made out of 32 hexadecimal characters, such as `f6a0529813f74d119982eb4fe43a9a24`. Simply put it in `LicenseID` property of ASF global config.