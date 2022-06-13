# 远程通信

本章节描述 ASF 所包含的远程通信，以及进一步解释您如何改变其行为。 虽然我们不认为下文所述是恶意或者不需要的，我们在法律上也没有义务予以披露，但我们希望您能更好地理解程序功能，特别是在您的隐私和数据共享方面。

## Steam

ASF 会与 Steam 网络（**[CM 服务器](https://api.steampowered.com/ISteamDirectory/GetCMList/v1?cellid=0)**）、**[Steam API](https://steamcommunity.com/dev)**、[**Steam 商店**](https://store.steampowered.com)和 [**Steam 社区**](https://steamcommunity.com)通信。

禁用上述任何通信是不可能的，因为它是 ASF 提供一切基本功能的基础。 如果您不喜欢上述通信，则应该选择不使用 ASF。

## Steam 组

ASF 与我们的 [**Steam 组**](https://steamcommunity.com/groups/archiasf)通信。 群组可以为您发布公告，特别是新版本、紧急状况、Steam 问题以及其他对于保持社区更新非常重要的事项。 它还使您可以使用我们的技术支持，即提出问题、解决问题、报告问题或建议改进。 默认情况下，使用 ASF 的帐户会在登录时自动加入到群组。

您可以选择不加入群组，只需要在机器人的 **[`RemoteCommunication`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-CN#remotecommunication)** 设置中关闭 `SteamGroup` flag。

## GitHub

ASF 与 **[GitHub 的 API](https://api.github.com)** 通信来获取 **[ASF Release](https://github.com/JustArchiNET/ArchiSteamFarm/releases)** 用于更新功能。 需要此通信的包括自动更新（如果您启用了 **[`UpdatePeriod`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-CN#updateperiod)**），也包括 `update` **[命令](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-CN)**。 您可以设置 **[`UpdateChannel`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-CN#updatechannel)** 属性影响 ASF 与 GitHub 的通信——设置为 `None` 会完全禁用更新功能，因此也就禁用了与 GitHub 的通信。

## ASF 服务器

ASF 与[**我们的服务器**](https://asf.justarchi.net)通信提供进阶功能。 特别包括：
- 验证从 GitHub 下载的 ASF 构建符合我们独立数据库中的校验和，以确保所有下载的构建是安全的（不含恶意软件、中间人攻击或其他篡改）
- 如果您在 **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-CN#tradingpreferences)** 中启用了 `SteamTradeMatcher`，并满足我们的要求，则会在[**我们的列表**](https://asf.justarchi.net/STM)中展示您的机器人
- 如果您在 **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-CN#tradingpreferences)** 中启用了 `MatchActively`，并满足我们的其他要求，则会从[**我们的列表**](https://asf.justarchi.net/STM)中下载当前可以交易的机器人

作为安全措施，您无法禁用 ASF 构建的校验和验证。  但是，如上文 GitHub 小节所述，如果您想避免此类通信，可以完全禁用自动更新。

您可以选择不在列表中展示，只需要在机器人的 **[`RemoteCommunication`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-CN#remotecommunication)** 设置中关闭 `PublicListing` flag。 如果您想要运行 `SteamTradeMatcher` 机器人但不展示，就可以这样设置。

从我们的列表中下载机器人对于 `MatchActively` 是强制要求，您如果不愿意接受，就必须禁用其设置。

---

## 公共 ASF STM 列表

我们的公共 ASF STM 列表位于&#8203;**[我们的网站](https://asf.justarchi.net/STM)**，它是一项面向启用 `MatchActively` 的 ASF 用户的公开服务，同时也供 ASF 用户与非 ASF 用户进行手动匹配。

请注意，如果您没有满足所有要求，将**不会**被显示在网站上。 在这种情况下，ASF 甚至不会与我们的服务器通信，因此，如果您没有启用 `SteamTradeMatcher` 以帮助自己匹配交易，则这一小节会被完全跳过。 此外，公开列表仅与最新稳定版本的 ASF 兼容，并且可能拒绝显示过时的机器人，特别是如果它们缺少只能在较新版本中找到的核心功能。

### 运作方式

ASF 会在登录后发送一次初始数据，其中包含公共列表使用的所有属性。 然后，每隔 10 分钟，ASF 会发送一个非常小的“心跳”请求，通知我们的服务器机器人仍然在线并运行。 如果由于某种原因心跳包未能送达，例如网络问题，ASF 将每分钟重试发送一次，直到服务器确认收到它。

这使我们的网站可以记录哪些帐户可用于匹配，以及它们是否仍然在线。 借助这一功能，我们的网站可以显示在**过去 15 分钟**内在线的所有 ASF 2FA+STM 帐户。

用户将会按照他们的库存降序排序——排在最上方的是启用 `MatchEverything` 的、接受所有 1:1 交易的、带有 `Any` 标记的机器人，然后再按 `MatchableTypes` 游戏数排序，最后按 `MatchableTypes` 物品数排序。

### API

ASF STM 列表暂时只接受 ASF 机器人。 目前无法在我们的列表中列出第三方机器人（因为我们无法轻松检查它们的代码，并确保它们符合我们的交易逻辑）。

如果您正在寻找以编程方式访问我们的列表的方法，我们为您提供了一个简单的 **[/Api/Bots](https://asf.justarchi.net/Api/Bots)** 端点。 这也是 ASF 内部为 `MatchActively` 用户使用的端点。

### 隐私政策

如果您同意在我们的列表中展示，即如上所述，启用 `SteamTradeMatcher`，并且不拒绝 `PublicListing`，我们会在服务器中临时存储一些您的 Steam 帐户数据，以便提供核心功能。

公开信息（由 Steam 暴露给任何感兴趣的人 ）包括：
- 您的 Steam ID（64 位形式，用于生成链接）
- 您的昵称（用于显示目的）
- 您的头像（经过哈希，用于显示目的）

私密信息（提供功能所需的特定数据）包括：
- 您的&#8203;**[交易令牌](https://steamcommunity.com/my/tradeoffers/privacy)**（使不是您好友的人可以向您发送交易报价）
- 您的 `MaxTradeHoldDuration`（使其他人了解您是否愿意接受他们的交易）
- 您的 `MatchableTypes`（用于显示和匹配目的）
- 您库存中 `MatchableTypes` 物品的总数（用于显示和匹配目的）
- 提供上述 `MatchableTypes` 物品的游戏总数（用于显示和匹配目的）
- 您在 **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-CN#tradingpreferences)** 中为 `MatchEverything` 设置的值（用于显示和匹配目的）

ASF 服务器**不会**在事先没有通知条款变更及其切实原因的情况下收集、存储或处理任何其他上面未列出的数据。 我们认为上述的一切都不是严重问题，我们提到这些是为了让您了解 ASF 在除了您自己配置的功能之外究竟还做了什么，使用户更好地了解其流程。

您的数据会在停止使用公共列表后的最多 15 分钟内自动从公开转为隐藏，无论是因为您更改了设置还是关闭了 ASF。     此外，自上述情况发生起最多 7 天内，它将被自动从我们的服务器上删除（包括所有备份）。