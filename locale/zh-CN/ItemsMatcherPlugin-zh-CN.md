# 物品匹配插件

`ItemsMatcherPlugin` 是 ASF 官方[**插件**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins-zh-CN)，为 ASF 添加 ASF STM 列表功能。 此插件主要包括 **[`RemoteCommunication`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-CN#remotecommunication)** 中的 `PublicListing` 和 **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-CN#tradingpreferences)** 中的 `MatchActively`。 ASF 在发布时会一并打包 `ItemsMatcherPlugin`，因此相关功能开箱即用。

---

## `PublicListing（公共列表）`

公共列表，顾名思义，是列出当前可用 ASF STM 机器人的列表。 它位于[**我们的网站**](https://asf.justarchi.net/STM)，它是一项面向启用 `MatchActively` 的 ASF 用户的全自动公开服务，同时也供 ASF 用户与非 ASF 用户进行手动匹配。

In order to be listed, you have a set of requirements to meet. At the minimum you must have allowed `PublicListing` in `RemoteCommunication` (default setting), `SteamTradeMatcher` enabled in `TradingPreferences`, **[public inventory](https://steamcommunity.com/my/edit/settings)** privacy settings, **[unrestricted](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663)** account, **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication#asf-2fa)** active and at least `100` items from specified `MatchableTypes`, such as trading cards.

尽管 `PublicListing` 默认启用，但请注意，如果您没有满足所有要求，特别是默认禁用的 `SteamTradeMatcher`，您将**不会**被显示在网站上。 对于未满足要求的用户，即使他们保持 `PublicListing` 启用，ASF 也不会以任何方式与服务器通信。 公开列表也仅与最新稳定版本的 ASF 兼容，并且可能拒绝显示过时的机器人，特别是如果它们缺少只能在较新版本中找到的核心功能。

### 运作方式

ASF 会在登录后发送一次初始数据，其中包含公共列表使用的所有属性。 然后，每隔 10 分钟，ASF 会发送一个非常小的“心跳”请求，通知我们的服务器机器人仍然在线并运行。 如果由于某种原因心跳包未能送达，例如网络问题，ASF 将每分钟重试发送一次，直到服务器确认收到它。 这样，我们的服务器能准确地知道哪些机器人仍在运行并准备好接受交易报价。 ASF 还将按需发出初始通报，例如，当检测到我们的库存与上次相比发生变化时。

我们的网站会显示在**过去 15 分钟**内在线的所有 ASF 2FA+STM 帐户。 用户将会按照他们相关性排序——排在最前面的是启用 `MatchEverything` 的机器人，它们会显示 `Any` 标记表示接受所有 1:1 交易，然后再按游戏数排序，最后按物品数排序。

### API

ASF STM 列表暂时只接受 ASF 机器人。 目前无法在我们的列表中列出第三方机器人，因为我们无法轻松检查它们的代码，并确保它们符合我们的交易逻辑。 因此，参与此列表需要最新的 ASF 稳定版，尽管使用自定义[**插件**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins-zh-CN)也可以。

对于列表的消费者，我们为您提供了一个简单的 **[`/Api/Listing/Bots`](https://asf.justarchi.net/Api/Listing/Bots)** 端点。 它包括所有我们已有的数据，但不包括用户的库存，因为这是 `MatchActively` 功能专用的数据。

### 隐私政策

如果您同意在我们的列表中展示，即如上所述，启用 `SteamTradeMatcher`，并且不拒绝 `PublicListing`，我们会在服务器中临时存储一些您的 Steam 帐户数据，以便提供预期功能。

公开信息（由 Steam 暴露给任何感兴趣的人 ）包括：
- 您的 Steam ID（64 位形式，用于生成链接）
- 您的昵称（用于显示目的）
- 您的头像（经过哈希，用于显示目的）

半公开信息（由 Steam 暴露给任何感兴趣并满足条件的人 ）包括：
- 您的库存（使其他用户能够对您的物品使用 `MatchActively`）

私密信息（提供功能所需的特定数据）包括：
- 您的[**交易令牌**](https://steamcommunity.com/my/tradeoffers/privacy)（使不是您好友的人可以向您发送交易报价）
- 您的 `MatchableTypes` 设置（用于显示和匹配目的）
- 您的 `MatchEverything` 设置（用于显示和匹配目的）
- 您的 `MaxTradeHoldDuration` 设置（使其他人了解您是否愿意接受他们的交易）

---

## `MatchActively（主动匹配）`

`MatchActively`（主动匹配）是 **[`SteamTradeMatcher`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading-zh-CN#steamtradematcher)** 的主动版本，包括互动式匹配，机器人同时会向其他人发送交易报价。 它可以单独运行，也可以与 `SteamTradeMatcher` 设置一起运行。 此功能需要设置 `LicenseID`，因为它需要使用第三方服务器和付费资源才能工作。

为了使用该选项，您需要满足一系列需求。 您至少应该保证帐户[**不受限**](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663)、**[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-zh-CN#asf-两步验证)** 启用，并且在 `MatchableTypes` 中设置了至少一种有效的类型，例如集换式卡牌。

如果您满足上述所有要求，ASF 将会定期与我们的[**公共 ASF STM 列表**](#publiclisting公共列表)通信，以主动匹配当前在线的机器人。

匹配过程会在 ASF 启动后大约 `1` 小时后开始，并且每 `6` 小时重复一次（如果有需要）。 During matching, ASF bot will fetch its own inventory, then communicate with our server with it to find all possible `MatchableTypes` matches from other, currently available bots. 由于是直接与我们的服务器通信，此过程只需要一次请求，就可以立即知道是否有可用机器人提供了我们感兴趣的物品——如果找到匹配，ASF 将会自动发送和确认交易报价。

这一模块应该是透明的。 匹配过程会在 ASF 启动后大约 `1` 小时后开始，并且每 `6` 小时重复一次（如果有需要）。 `MatchActively` feature is aimed to be used as a long-run, periodical measure to ensure that we're actively heading towards sets completion, but without a short-term time and resources pressure that would happen if this was offered only as a command. 此模块的目标用户是主帐户和用于存储的子帐户，但也可以用于任何没有设置 `MatchEverything` 的机器人。

ASF 会尽力减少由此选项带来的请求和压力，同时将匹配的效率提升至极限。 用于匹配机器人以及组织整个流程的算法是 ASF 的实现细节，可能会根据用户反馈、具体情况和未来的想法进行更改。

当前版本的算法使 ASF 优先匹配有 `Any` 标记的机器人，特别是物品所属游戏数更多的机器人。 当 `Any` 机器人耗尽时，ASF 会以相同差异规则与 `Fair` 机器人匹配。 ASF 将尝试匹配每个可用的机器人至少一次，以确保我们不会错过可能的物品套组进度。

`MatchActively` 支持交易黑名单，您可以通过 `tbadd` [**命令**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-CN)向其中添加机器人的帐户，您的机器人将不会尝试与黑名单中的机器人主动匹配。 这告诉 ASF 永远不匹配这些机器人，即使这些机器人有我们可能需要的卡牌。

---

### 为什么我需要 `LicenseID` 才能使用 `MatchActively`？ 它之前不是免费的吗？

ASF 是并且现在仍然是免费、开放源代码的，一如在 2015 年 10 月项目创立时所确定的那样。 我们的程序也是完全非商业化的，我们不会从对它的贡献、构建和发布中赚取任何东西。 在过去 7 年多的时间里，ASF 获得了巨大的发展，而且仍然每月发布稳定版对程序进行改进和增强，这一切主要由一个人完成，**[JustArchi](https://github.com/JustArchi)**——没有提出任何附加条件。 我们唯一的资金来源是来自用户的非强制性捐款。

在 2022 年 10 月以前的很长一段时间里，`MatchActively` 功能都是 ASF 核心的一部分，供所有人使用。 In October 2022, Valve, the company behind Steam, has put very severe rate limits on fetching inventories of other bots - which rendered previous functionality entirely broken, with no solution available. 因此，自 5.4.1.0 版本起，此功能不得不从 ASF 核心功能中删除。

`MatchActively` 以官方 `ItemsMatcher` 插件的形式复活，使 ASF 仍然有能力进行主动卡片匹配。 复活的 `MatchActively` 功能需要我们在 ASF 后端进行**巨量的工作**，包括全新的托管服务器和上千个用于解析库存的代理，一切都专门用来让 ASF 客户端能像以前一样使用 `MatchActively`。 由于涉及的工作量非常大，并且相关资源（域名、服务器、代理）并非免费，而是需要我们每月付费，我们决定仅将此功能提供给我们的赞助者，也就是已经每月支持 ASF 项目的人。 我们的目标不是从中获益，而是为了能负担起专门提供这一选项所带来的**月度费用**——因此我们基本上无条件提供功能，但确实必须从中收取一些费用，因为我们不可能仅仅为了此功能可以正常运行就每月自掏腰包支付数百美元。 我们希望您能理解。

---

### 如何获取访问权限？

`ItemsMatcher` 在 **[JustArchi 的 GitHub](https://github.com/sponsors/JustArchi)** 作为 $5+ 赞助者等级的奖励提供。 您只需要成为 $5 或更高等级的赞助者，然后阅读[**配置**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-CN#licenseid)章节来获取和填写 `LicenseID`。

此许可证允许您向服务器发送有限数量的请求。 $5 等级允许您为一个帐户启用 `MatchActively`，这应该能满足大多数人的需求。 $10 等级允许您为三个帐户启用。  如果您需要更多资源，**[请告知我们](mailto:ASF@JustArchi.net)**。