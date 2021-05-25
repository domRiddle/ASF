# 统计

ASF 开发由 3 个主要来源支持：捐赠、用户反馈和统计数据。 捐赠直接影响我们为此项目工作的意愿，用户反馈总是值得一读（特别是积极的），而统计数据为我们提供了软件被如何使用、有多少人在用的信息——这样我们就可以知道改进什么、修复什么、关注什么。

ASF 默认启用了 `Statistics` 全局配置属性。 如果您希望看到发布新版本、修复错误、实现新功能，您应该考虑保留它，以便我们可以利用这些数据，为您提供更好的软件（但不仅如此）。 这一点特别重要，因为 ASF **免费**供您使用，这是您可以表达的最基本的感谢——**告诉我们您正在使用 ASF**，也就是统计最主要的功能。 ASF 不会收集或利用任何可能被视为您的私人数据和/或可能对您不利的数据。 我们保持在最低限度使用统计，每一条收集的信息都需要我们在此明确声明，并准确解释。 这使您可以充分了解我们收集什么、收集的目的，以及这些数据有什么帮助。 您可以在下文阅读上述所有信息。

---

## 当前隐私政策

当 `Statistics` 被启用时，会发生下面的事：

1. ASF 使用的帐户都会在登录时加入我们的 **[Steam 群组](https://steamcommunity.com/gid/103582791440160998)**。 这样做有三个原因：

* 为**您**提供群组公告，特别是新版本、紧急状况、Steam 问题以及其他对于保持社区更新非常重要的事项（保证没有 ASF 以外的垃圾消息）。
* 使**您**可以使用我们的技术支持，即提出问题、解决问题、报告问题或建议改进。
* 让我们了解使用 ASF 的实际 Steam 用户数量。

我们认为，对于 ASF 社区来说，Steam 群组是一个关键部分。 这是我们的主要交流渠道，我们利用这个渠道来处理关于 ASF 的所有重要事项，特别是使您随时了解最新的开发进程、潜在问题、最终警告以及所有您作为用户应该了解的其他事项。 我们不以任何方式从维护此群组中获利，这是致力于 ASF 用户的地方，并且我们认为您是我们社区的一部分。 因为属于该群组无法代表您是一名 ASF 用户，我们认为这不是一个隐私方面的问题。

2. If your account is **[unrestricted](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663)**, using **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication#asf-2fa)**, has **[public inventory](https://steamcommunity.com/my/edit/settings)** with at least 100 `MatchableTypes` items in it and you intentionally enabled `SteamTradeMatcher` in your `TradingPreferences`, then ASF will periodically communicate with our **[server](https://asf.justarchi.net)** in order to fulfill the enabled functionality. 实际数据由唯一的 ASF ID（由 ASF 生成）和以下与帐户相关的信息组成：

* 您的 Steam ID（64 位形式，用于生成链接，属于公开信息）
* 您的昵称（用于显示目的，属于公开信息）
* 您的头像（经过哈希，用于显示目的，属于公开信息）
* Your **[trading token](https://steamcommunity.com/my/tradeoffers/privacy)** (so people outside of your friendlist can send you trades)
* 您的 `MatchableTypes`（用于显示和匹配目的）
* 您在 `TradingPreferences` 中为 `MatchEverything` 设置的值（用于显示和匹配目的）
* 您库存中 `MatchableTypes` 物品的总数（用于显示和匹配目的）
* 提供上述 `MatchableTypes` 物品的游戏总数（用于显示和匹配目的）

ASF will **not** collect any other non-listed-above data without prior important notice in the changelog, and a very good practical reason in the first place. 我们认为上述的一切都不是严重问题，我们提到这些是为了让您了解 ASF 在除了您自己配置的功能之外究竟还做了什么，使用户更好地了解我们的观点。

---

# 数据用途

All values specified in second point are being used for our **Public ASF STM listing**, which is explained below. 我们不会以任何其他目的使用任何其他数据。

---

## 公共 ASF STM 列表

Our public ASF STM listing is located on **[our website](https://asf.justarchi.net/STM)** and used as a public service for both ASF users that make use of `MatchActively`, as well as ASF and non-ASF users for manual matching.

### 运作方式

ASF 会在登录后发送一次初始数据，其中包含公共列表使用的所有属性。 然后，每隔 10 分钟，ASF 会发送一个非常小的“心跳”请求，通知我们的服务器机器人仍然在线并运行。 如果由于某种原因心跳包未能送达，例如网络问题，ASF 将每分钟重试发送一次，直到服务器确认收到它。

这使我们的网站可以记录哪些帐户可用于匹配，以及它们是否仍然在线。 Thanks to that, our website can show all ASF 2FA+STM accounts that were active in **last 15 minutes**.

用户将会按照他们的库存降序排序——排在最上方的是启用 `MatchEverything` 的、接受所有 1:1 交易的、带有 `Any` 标记的机器人，然后再按 `MatchableTypes` 游戏数排序，最后按 `MatchableTypes` 物品数排序。

Please note that you will **not** be displayed on the website if you do not meet all of the requirements. 在这种情况下，ASF 甚至不会与我们的服务器通信，因此，如果您没有启用 `SteamTradeMatcher` 以帮助自己匹配交易，则上述的第二点会被完全跳过。 此外，公开列表仅与最新稳定版本的 ASF 兼容，并且可能拒绝显示过时的机器人，特别是如果它们缺少只能在较新版本中找到的核心功能。

### API

ASF STM 列表暂时只接受 ASF 机器人。 目前无法在我们的列表中列出第三方机器人（因为我们无法轻松检查它们的代码，并确保它们符合我们的交易逻辑）。

如果您正在寻找以编程方式访问我们的列表的方法，我们为您提供了一个简单的 **[/Api/Bots](https://asf.justarchi.net/Api/Bots)** 端点。 这也是 ASF 内部为 `MatchActively` 用户使用的端点。

### 注意

*所有的概念，包括网站集成和 ASF 报告仍在测试中——它可能随着时间的推移而更新，或者在我们对其不再感兴趣的情况下将其删除。*

---

## 选择退出

参与统计**不是强制性的**，但我们建议您为了 ASF 的发展加入统计。 我们不会评判您的行为，如果您内心强烈希望隐藏您是 ASF 用户这件事，可以将 `Statistics` 设置为 `false`，**完全**关闭统计功能。 禁用统计会使整个模块停止运行，并且不会再执行上述隐私政策中指定的任何操作。

禁用统计可能会影响**我们的技术支持、ASF 的功能以及其他我们免费为您提供的东西**。 例如，如果您不启用 Statistics 选项，就无法使用 `MatchActively` 功能（因为您拒绝向启用此功能所需的服务器发送信息）。