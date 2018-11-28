# 配置

此页面专门用于说明 ASF 的相关配置。 它是一份关于 `config` 文件夹的完整文档，让您能够根据自己的需求调整 ASF。

- **[简介](#简介)**
- **[在线配置文件生成器](#在线配置文件生成器)**
- **[手动配置](#手动配置)**
- **[全局配置](#全局配置)**
- **[机器人配置](#机器人配置)**
- **[文件结构](#文件结构)**
- **[JSON 映射](#json-映射)**
- **[兼容性映射](#兼容性映射)**
- **[配置兼容性](#配置兼容性)**
- **[自动重载](#自动重载)**

* * *

## 简介

ASF 的配置分为两个主要的部分——全局（进程）配置和每个机器人的配置。 每个机器人都有自己的名为 `BotName.json` 的机器人配置文件（其中 `BotName` 是机器人的名称），而 ASF 的全局（进程）配置是一个单独的名叫 `ASF.json` 的文件。

每个机器人都是一个在 ASF 进程中运行的单独的 Steam 帐户。 为了能够正常工作，ASF 需要至少定义**一个**机器人实例。 进程不会为机器人实例设定强制的数量限制，所以您可以运行任意数量的机器人（Steam 帐户）。

ASF 采用 **[JSON](https://en.wikipedia.org/wiki/JSON)** 格式存储其配置文件。 这是人性化、可读性高且非常通用的格式，您可以在其中对程序进行配置。 不过不用担心，您不需要为了配置 ASF 去专门了解 JSON。 我提到它只是考虑到您可能会想要使用一些 Bash 脚本批量创建大量 ASF 配置文件。

您可以通过创建合适的 JSON 配置文件来手动完成配置，也可以通过我们的&#8203;**[在线配置文件生成器](https://justarchinet.github.io/ASF-WebConfigGenerator)**&#8203;来进行配置，那将会简单方便很多。 除非您是高级用户，否则我建议您使用配置生成器，我们将会在下方对其进行具体说明。

**[回到顶部](#配置)**

* * *

## 在线配置文件生成器

在线配置文件生成器的目标是给您提供一个用于生成 ASF 配置文件的友好前端。 在线配置文件生成器是 100% 基于客户端的，这意味着您输入的任何信息都不会被上传，而仅在本地进行处理。 这保证了安全性和可靠性，因为假设您愿意下载所有相关文件，并在您喜爱的浏览器中打开其中的 `index.html`，它甚至可以&#8203;**[离线](https://github.com/JustArchiNET/ASF-WebConfigGenerator/tree/master/docs)**&#8203;运行。

在线配置文件生成器已经在 Chrome 和 Firefox 上经过验证可以正常运行，但它也应该可以在所有流行的的支持 JavaScript 的浏览器中正常运行。

它的用法非常简单——切换到正确的选项卡来选择要生成 `ASF` 配置还是 `Bot` 配置，确保所选配置文件的版本与您的 ASF 版本相匹配，然后输入所有详细信息并点击“下载”按钮。 将此文件移动到 ASF 的 `config` 文件夹，如果需要的话，覆盖掉已经存在的文件。 如果要继续配置，则重复以上操作，并参考本页的其他部分以了解所有可配置的选项。

**[回到顶部](#配置)**

* * *

## 手动配置

我强烈建议使用在线配置文件生成器，但如果出于某种原因，您不想使用，那么您也可以自行创建正确的配置文件。 参考下面的 JSON 示例来保证结构正确，您可以将内容复制到文件中并在此基础上作修改。 由于您没有使用我们的前端，请确保您的配置&#8203;**[有效](https://jsonlint.com)**，因为如果 ASF 无法解析它，将拒绝加载。 关于各字段的正确 JSON 结构，请阅读本文中的 **[JSON 映射](#json-映射)**&#8203;一节。

**[回到顶部](#配置)**

* * *

## 全局配置

全局配置存放于 `ASF.json` 文件中，其结构如下：

```json
{
    "AutoRestart": true,
    "Blacklist": [],
    "CommandPrefix": "!",
    "ConfirmationsLimiterDelay": 10,
    "ConnectionTimeout": 60,
    "CurrentCulture": null,
    "Debug": false,
    "FarmingDelay": 15,
    "GiftsLimiterDelay": 1,
    "Headless": false,
    "IdleFarmingPeriod": 8,
    "InventoryLimiterDelay": 3,
    "IPC": false,
    "IPCPassword": null,
    "LoginLimiterDelay": 10,
    "MaxFarmingTime": 10,
    "MaxTradeHoldDuration": 15,
    "OptimizationMode": 0,
    "Statistics": true,
    "SteamMessagePrefix": "/me ",
    "SteamOwnerID": 0,
    "SteamProtocols": 7,
    "UpdateChannel": 1,
    "UpdatePeriod": 24,
    "WebLimiterDelay": 200,
    "WebProxy": null,
    "WebProxyPassword": null,
    "WebProxyUsername": null
}
```

**提示：**&#8203;除非您想要修改其中的选项，否则可以保持所有选项为默认，关闭 `ASF.json` 然后开始配置机器人选项。

* * *

下面是对所有选项的解释：

### `AutoRestart`

这是一个默认值为 `true` 的 `bool` 类型属性。 该属性定义了是否允许 ASF 程序在需要时自动重启。 有一些情况需要 ASF 自动重启，例如 ASF 更新（通过 `UpdatePeriod` 或者 `update` 命令完成）、`ASF.json` 文件有修改、执行 `restart` 命令等。 通常情况下，重启包括两个部分——创建新进程和结束当前进程。 对于大多数用户来说，这个属性应保留为默认值 `true`。除非您正在通过自己的脚本或者 `dotnet` 运行 ASF，这种情况下，您可能更希望能够完全控制进程的启动，同时避免新的（自动重启产生的）ASF 程序在后台运行而不是在脚本控制下运行，而脚本随着旧的 ASF 进程一起退出的情况。 同时考虑到新的进程不再是原先进程的直接子进程，这可能会导致您无法使用标准控制台输入。

如果是这样的情况，这个属性就是专门为您设置的，您可以将其设置为 `false`。 However, keep in mind that in such case **you** are responsible for restarting the process. This is somehow important as ASF will only exit instead of spawning new process (e.g. after update), so if there is no logic added by you, it'll simply stop working until you start it again. ASF always exits with proper error code indicating success (zero) or non-success (non-zero), this way you're able to add proper logic in your script which should avoid auto-restarting ASF in case of failure, or at least make a local copy of `log.txt` for further analysis. Also keep in mind that `restart` command will always restart ASF regardless of how this property is set, as this property defines default behaviour, while `restart` command always restarts the process. 除非您有理由禁用此功能, 否则应将其保持为启用。

* * *

### `Blacklist`

`ImmutableHashSet<uint>` type with default value of being empty. As the name suggests, this global config property defines appIDs (games) that will be entirely ignored by automatic ASF idling process. Unfortunately Steam loves to flag summer/winter sale badges as "available for cards drop", which confuses ASF process by making it believe that it's a valid game that should be farmed. If there was no any kind of blacklist, ASF would eventually "hang" at farming a game which is in fact not a game, and wait infinitely for cards drop that will not happen. ASF blacklist serves a purpose of marking those badges as not available for farming, so we can silently ignore them when deciding what to farm, and not fall into the trap.

ASF includes two blacklists by default - `GlobalBlacklist`, which is hardcoded into the ASF code and not possible to edit, and normal `Blacklist`, which is defined here. `GlobalBlacklist` is updated together with ASF version and typically includes all "bad" appIDs at the time of release, so if you're using up-to-date ASF then you do not need to maintain your own `Blacklist` defined here. The main purpose of this property is to allow you blacklisting new, not-known at the time of ASF release appIDs, which should not be farmed. Hardcoded `GlobalBlacklist` is being updated as fast as possible, therefore you're not required to update your own `Blacklist` if you're using latest ASF version, but without `Blacklist` you'd be forced to update ASF in order to "keep running" when Valve releases new sale badge - I don't want to force you to use latest ASF code, therefore this property is here to allow you "fixing" ASF yourself if you for some reason don't want to, or can't, update to new hardcoded `GlobalBlacklist` in new ASF release, yet you want to keep your old ASF running. 除非您有**充分的**理由编辑此属性, 否则应将其保留为默认值。

如果您需要基于机器人设置的黑名单，请查看 `ib`、`ibadd` 和 `ibrm` **[命令](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-CN)**。

* * *

### `CommandPrefix`

`string` type with default value of `!`. 这个属性为 ASF **[命令](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-CN)**&#8203;指定一个**大小写敏感**的命令前缀。 In other words, this is what you need to prepend to each ASF command in order to make ASF listen to you. It's possible to set this value to `null` or empty in order to make ASF use no command prefix, in which case you input commands with their plain identifiers. However, doing so will potentially decrease ASF's performance as ASF is optimized to not parse message further if it doesn't start with `CommandPrefix` - if you intentionally decide to not use it, ASF will be forced to read all messages and respond to them, even if they're not ASF commands. Therefore it's recommended to keep using some `CommandPrefix`, such as `/` if you don't like default value of `!`. For consistency, `CommandPrefix` affects the entire ASF process. 除非您有理由编辑此属性, 否则应将其保留为默认值。

* * *

### `ConfirmationsLimiterDelay`

这是一个默认值为 `10` 的 `byte` 类型属性。 ASF 保证至少间隔 `ConfirmationsLimiterDelay` 秒才会重新抓取两步验证确认列表，以避免触发频率限制——这一特性会应用在 **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-zh-CN)** 操作中，例如 `2faok` 命令。在执行一些交易相关的操作时，如有需要，也会应用这一特性。 默认值基于我们的测试结果选择，如果您不想遇到问题，请不要减小这个值。 除非您有**充分的**理由编辑此属性, 否则应将其保留为默认值。

* * *

### `ConnectionTimeout`

这是一个默认值为 `60` 的 `byte` 类型属性。 This property defines timeouts for various network actions done by ASF, in seconds. In particular, `ConnectionTimeout` defines timeout in seconds for HTTP and IPC requests, `ConnectionTimeout / 10` defines maximum number of failed heartbeats, while `ConnectionTimeout / 30` defines number of minutes we allow for initial Steam network connection request. Default value of `60` should be fine for majority of people, however, if you have rather slow network connection or PC, you might want to increase this number to something higher (like `90`). Keep in mind that bigger values will not magically fix slow or even inaccessible Steam servers, so we shouldn't infinitely wait for something that won't happen and simply try again later. Setting this value too high will result in excessive delay in catching network issues, as well as in decrease of overall performance. Setting this value too low will decrease overall stability and performance as well, as ASF will abort valid request still being parsed. Therefore setting this value lower than default has no advantage in general, as Steam servers tend to be slow from time to time, and might require more time for parsing ASF requests. Default value is a balance between believing that our network connection is stable, and doubting in Steam network to handle our request in given timeout. If you want to detect issues sooner and make ASF reconnect/respond faster, default value should do (or very slightly below, making ASF less patient). If you instead notice that ASF is running into network issues, such as failing requests, heartbeats being lost or connection to Steam interrupted, it might make sense to increase this value if you're sure that it's **not** caused by your network, but by Steam itself, as increasing timeouts makes ASF more "patient" and not deciding to reconnect right away. It might also make sense to increase this value if you have rather slow internet that requires more time to process the data being transmitted. In short, default value should be decent for most cases, but you might want to increase it if needed. Still, going far above the default value doesn't make much sense either, since bigger timeouts won't magically fix inaccessible Steam servers. 除非您有理由编辑此属性, 否则应将其保留为默认值。

* * *

### `CurrentCulture`

`string` type with default value of `null`. By default ASF attempts to use your operating system language, and will prefer to use translated strings in that language if available. 感谢我们的社区将 ASF **[本地化](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Localization-zh-CN)**&#8203;为各门主流语言。 If for some reason you don't want to use your OS native language, you can use this config property to pick any valid language you'd want to use instead. For a list of all available cultures, please visit **[MSDN](https://msdn.microsoft.com/en-us/library/cc233982.aspx)** and look for `Language tag`. It's nice to note that ASF accepts both specific cultures, such as `en-GB`, but also neutral ones, such as `en`. Specifying current culture might also affect other culture-specific behaviour, such as currency/date format and alike. Please note that you might need additional font/language packs for displaying language-specific characters properly, if you picked non-native culture that makes use of them. Typically you want to make use of this config property if you prefer ASF in English instead of your native language.

* * *

### `Debug`

这是一个默认值为 `false` 的 `bool` 类型属性。 This property defines if process should run in debug mode. When in debug mode, ASF creates a special `debug` directory next to the `config`, which keeps track of whole communication between ASF and Steam servers. Debug information can help spotting nasty issues related to networking and general ASF workflow. In addition to that, some program routines will be far more verbose, such as `WebBrowser` stating exact reason why some requests are failing - those entries are written to normal ASF log. **You should not run ASF in Debug mode, unless asked by developer**. Running ASF in debug mode **decreases performance**, **affects stability negatively** and is **far more verbose in various places**, so it should be used **only** intentionally, in short-run, for debugging particular issue, reproducing the problem or getting more info about a failing request, and alike, but **not** for normal program execution. You will see **a lot** of new errors, issues, and exceptions - make sure that you have a decent knowledge about ASF, Steam and its quirks if you decide to analyze all of that yourself, as not everything is relevant.

**WARNING:** enabling this mode includes logging of **potentially sensitive** information such as logins and passwords that you're using for logging in to Steam (due to network logging). That data is written to both `debug` directory, as well as standard `log.txt` (that is now intentionally much more verbose to log this info). You should not post debug content generated by ASF in any public location, ASF developer should always remind you of sending it to his e-mail, or other secure location. We're not storing, neither making use of those sensitive details, they're written as part of debug routines since their presence might be relevant to the issue that is affecting you. We'd prefer if you didn't alter ASF logging in any way, but if you're worried, you're free to redact those sensitive details appropriately.

> Redacting involves replacing sensitive details, for example with stars. You should refrain from removing sensitive lines entirely, as their pure existence might be relevant and should be preserved.

* * *

### `FarmingDelay`

这是一个默认值为 `15` 的 `byte` 类型属性。 In order for ASF to work, it will check currently farmed game every `FarmingDelay` minutes, if it perhaps dropped all cards already. Setting this property too low can result in excessive amount of steam requests being sent, while setting it too high can result in ASF still "farming" given title for up to `FarmingDelay` minutes after it's fully farmed. Default value should be excellent for most users, but if you have many bots running, you might consider increasing it to something like `30` minutes in order to limit steam requests being sent. It's nice to note that ASF uses event-based mechanism and checks game badge page on each Steam item dropped, so in general **we don't even need to check it in fixed time intervals**, but as we don't fully trust Steam network - we check game badge page anyway, if we didn't check it through card being dropped event in last `FarmingDelay` minutes (in case Steam network didn't inform us about item dropped or stuff like that). Assuming that Steam network is working properly, decreasing this value **will not improve farming efficiency in any way**, while **increasing network overhead significantly** - it's recommended only to increase it (if needed) from default of `15` minutes. 除非您有**充分的**理由编辑此属性, 否则应将其保留为默认值。

* * *

### `GiftsLimiterDelay`

这是一个默认值为 `1` 的 `byte` 类型属性。 ASF will ensure that there will be at least `GiftsLimiterDelay` seconds in between of two consecutive gift/key/license handling (redeeming) requests to avoid triggering rate-limit. 此外，在游戏列表请求时（例如执行 `owns` **[命令](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-CN)**&#8203;时），也会以这个值作为全局限制。 除非您有**充分的**理由编辑此属性, 否则应将其保留为默认值。

* * *

### `Headless`

这是一个默认值为 `false` 的 `bool` 类型属性。 This property defines if process should run in headless mode. When in headless mode, ASF assumes that it's running on a server or in other non-interactive environment, therefore it will not attempt to read crucial account credentials such as 2FA code, SteamGuard code, password or any other variable required for ASF to operate. This is equal to making ASF console read-only. `Headless` mode is useful mainly for users running ASF on their servers, as instead of asking e.g. for 2FA code, ASF will silently abort the operation by stopping an account. Unless you're running ASF on a server, and you previously confirmed that ASF is able to operate in non-headless mode, you should keep this property disabled. Any user interaction will be denied when in headless mode, and your accounts will not run if they require **any** console input during starting. This is useful for servers, as ASF can abort trying to log onto the account when asked for credentials, instead of waiting (infinitely) for user to provide those. 启用这一模式将允许您使用 `input` **[命令](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-CN)**&#8203;作为标准控制台输入的替代。 If you're not sure how to set this property, leave it with default value of `false`.

如果您在服务器上运行 ASF，您可能需要将这个属性与 `--process-required` **[命令行参数](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments-zh-CN)**&#8203;配合使用。

* * *

### `IdleFarmingPeriod`

这是一个默认值为 `8` 的 `byte` 类型属性。 When ASF has nothing to farm, it will periodically check every `IdleFarmingPeriod` hours if perhaps account got some new games to farm. This feature is not needed when talking about new games we're getting, as ASF is smart enough to automatically check badge pages in this case. `IdleFarmingPeriod` is mainly for situations such as old games we already have having trading cards added. In this case there is no event, so ASF has to periodically check badge pages if we want to have this covered. Value of `0` disables this feature. Also check: `ShutdownOnFarmingFinished`.

* * *

### `InventoryLimiterDelay`

这是一个默认值为 `3` 的 `byte` 类型属性。 ASF will ensure that there will be at least `InventoryLimiterDelay` seconds in between of two consecutive inventory requests to avoid triggering rate-limit - those are being used for fetching your own inventory (and only for that). Default value of `3` was set based on looting over 100 bot instances, and should satisfy most (if not all) of the users. You may however want to decrease it, or even change to `0` if you have very low amount of bots, so ASF will ignore the delay and loot steam inventories much faster. Be warned though, as setting it too low **will** result in Steam temporarily banning your IP, and that will prevent you from fetching your inventory at all. You also might need to increase this value if you're running a lot of bots with a lot of inventory requests, although in this case you should probably try to limit number of those requests instead. 除非您有**充分的**理由编辑此属性, 否则应将其保留为默认值。

* * *

### `IPC`

这是一个默认值为 `false` 的 `bool` 类型属性。 这个属性定义了 ASF 的 **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-zh-CN)** 服务器是否需要随进程一同启动。 IPC allows for inter-process communication by booting a local HTTP server. 如果您不需要 IPC 服务，则不需要启用此选项。

* * *

### `IPCPassword`

`string` type with default value of `null`. This property defines mandatory password for every call done via IPC and serves as an extra security measure. When set to non-empty value, all IPC requests will require extra `password` property set to the password specified here. Default value of `null` will skip a need of the password, making ASF respect all queries. In addition to that, enabling this option also enables built-in IPC anti-bruteforce mechanism which will temporarily ban given `IPAddress` after sending too many unauthorized requests in a very short time. 除非您有理由编辑此属性, 否则应将其保留为默认值。

* * *

### `LoginLimiterDelay`

这是一个默认值为 `10` 的 `byte` 类型属性。 ASF will ensure that there will be at least `LoginLimiterDelay` seconds in between of two consecutive connection attempts to avoid triggering rate-limit. Default value of `10` was set based on connecting over 100 bot instances, and should satisfy most (if not all) of the users. You may however want to increase/decrease it, or even change to `0` if you have very low amount of bots, so ASF will ignore the delay and connect to Steam much faster. Be warned though, as setting it too low while having too many bots **will** result in Steam temporarily banning your IP, and that will prevent you from logging in **at all**, with `InvalidPassword/RateLimitExceeded` error - and that also includes your normal Steam client, not only ASF. Likewise, if you're running excessive number of bots, especially together with other Steam clients/tools using the same IP address, most likely you'll need to increase this value in order to spread logins across longer period of time.

As a side note, this value is also used as load-balancing buffer in all ASF-scheduled actions, such as trades in `SendTradePeriod`. 除非您有**充分的**理由编辑此属性, 否则应将其保留为默认值。

* * *

### `MaxFarmingTime`

这是一个默认值为 `10` 的 `byte` 类型属性。 As you should know, Steam is not always working properly, sometimes weird situations can happen such as steam not being recording our playtime, despite of in fact playing a game. ASF will allow farming a single game in solo mode for maximum of `MaxFarmingTime` hours, and consider it fully farmed after that period. This is required to not freeze farming process in case of weird situations happening, but also if for some reason Steam released a new badge that would stop ASF from progressing further (see: `Blacklist`). Default value of `10` hours should be enough for dropping all steam cards from one game. Setting this property too low can result in valid games being skipped (and yes, there are valid games taking even up to 9 hours to farm), while setting it too high can result in farming process being frozen. Please note that this property affects only a single game in a single farming session (so after going through entire queue ASF will return to that title), also it's not based on total playtime but internal ASF farming time. 除非您有**充分的**理由编辑此属性, 否则应将其保留为默认值。

* * *

### `MaxTradeHoldDuration`

这是一个默认值为 `15` 的 `byte` 类型属性。 This property defines maximum duration of trade hold in days that we're willing to accept - ASF will reject trades that are being held for more than `MaxTradeHoldDuration` days. This option makes sense only for bots with `TradingPreferences` of `SteamTradeMatcher`, as it doesn't affect `Master`/`SteamOwnerID` trades, neither donations. Trade holds are annoying for everyone, and nobody really wants to deal with them. ASF is supposed to work on liberal rules and help everyone, regardless if on trade hold or not - that's why this option is set to `15` by default. However, if you'd instead prefer to reject all trades affected by trade holds, you can specify `0` here. 请注意，有时间限制的卡牌（例如特惠活动卡牌）不会受这一选项影响，ASF 会驳回任何有交易暂挂的交易者，如&#8203;**[交易](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading-zh-CN)**&#8203;章节所述，所以您不需要仅仅因为这种情况驳回所有人。 除非您有理由编辑此属性, 否则应将其保留为默认值。

* * *

### `OptimizationMode`

这是一个默认值为 `0` 的 `byte` 类型属性。 This property defines optimization mode which ASF will prefer during runtime. Currently ASF supports two modes - `0` which is called `MaxPerformance`, and `1` which is called `MinMemoryUsage`. By default ASF prefers to run as many things in parallel (concurrently) as possible, which enhances performance by load-balancing work across all CPU cores, multiple CPU threads, multiple sockets and multiple threadpool tasks. For example, ASF will ask for your first badge page when checking for games to idle, and then once request arrived, ASF will read from it how many badge pages you actually have, then request each other one concurrently. This is what you should want **almost always**, as the overhead in most cases is minimal and benefits from asynchronous ASF code can be seen even on the oldest hardware with a single CPU core and heavily limited power. However, with many tasks being processed in parallel, ASF runtime is responsible for their maintenance, e.g. keeping sockets open, threads alive and tasks being processed, which can result in increased memory usage from time to time, and if you're extremely constrained by available memory, you might want to switch this property to `1` (`MinMemoryUsage`) in order to force ASF into using as little tasks as possible, and typically running possible-to-parallel asynchronous code in a synchronous manner. 只有在您已阅读&#8203;**[低内存方案](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup-zh-CN)**，并且决定大量牺牲性能换取少量的内存节省的情况下，才应该考虑修改这一属性。 通常，与&#8203;**[低内存方案](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup-zh-CN)**&#8203;中所述的其他方式，例如限制 ASF 使用或者调优运行时环境的垃圾回收机制相比，这一选项的效果会**差得多**。 Therefore, you should use `MinMemoryUsage` as a **last resort**, right before runtime recompilation, if you couldn't achieve satisfying results with other (much better) options. 除非您有**充分的**理由编辑此属性, 否则应将其保留为默认值。

* * *

### `Statistics`

这是一个默认值为 `true` 的 `bool` 类型属性。 这个属性定义了 ASF 是否启用统计功能。 我们在&#8203;**[统计](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Statistics-zh-CN)**&#8203;章节中具体解释了这个选项究竟会做什么。 除非您有理由编辑此属性, 否则应将其保留为默认值。

* * *

### `SteamMessagePrefix`

`string` type with default value of `"/me "`. This property defines a prefix that will be prepended to all Steam messages being sent by ASF. By default ASF uses `"/me "` prefix in order to distinguish bot messages more easily by showing them in different color on Steam chat. Another worthy mention is `"/pre "` prefix which achieves similar result, but uses different formatting. You can also set this property to empty string or `null` in order to disable using prefix entirely and output all ASF messages in a traditional way. It's nice to note that this property affects Steam messages only - responses returned through other channels (such as IPC) are not affected. 除非您希望更改 ASF 的标准行为, 否则最好将其保留为默认值。

* * *

### `SteamOwnerID`

`ulong` type with default value of `0`. This property defines Steam ID in 64-bit form of ASF process owner, and is very similar to `Master` permission of given bot instance (but global instead). You almost always want to set this property to ID of your own main Steam account. `Master` permission includes full control over his bot instance, but global commands such as `exit`, `restart` or `update` are reserved for `SteamOwnerID` only. This is convenient, as you might want to run bots for your friends, while not allowing them to control ASF process, such as exiting it via `exit` command. Default value of `0` specifies that there is no owner of ASF process, which means that nobody will be able to issue global ASF commands. Keep in mind that **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** works with `SteamOwnerID`, so if you want to use it you must provide a valid value here.

* * *

### `SteamProtocols`

`byte flags` type with default value of `7`. This property defines Steam protocols that ASF will use when connecting to Steam servers, which are defined as below:

| 值 | 名称        | 描述                                                                                               |
| - | --------- | ------------------------------------------------------------------------------------------------ |
| 0 | None      | 无协议                                                                                              |
| 1 | TCP       | **[Transmission Control Protocol](https://en.wikipedia.org/wiki/Transmission_Control_Protocol)** |
| 2 | UDP       | **[User Datagram Protocol](https://en.wikipedia.org/wiki/User_Datagram_Protocol)**               |
| 4 | WebSocket | **[WebSocket](https://en.wikipedia.org/wiki/WebSocket)**                                         |

Please notice that this property is `flags` field, therefore it's possible to choose any combination of available values. 如果您想了解更多，请阅读 **[flags 映射](#json-映射)**。 Not enabling any of flags results in `None` option, and that option is invalid by itself.

By default ASF should use all available Steam protocols as a measure for fighting with downtimes and other similar Steam issues. Typically you want to change this property if you want to limit ASF into using only one or two specific protocols instead of all available ones. Such measure could be needed if you're e.g. enabling only TCP traffic on your firewall and you do not want ASF to try connecting via UDP. However, unless you're debugging particular problem or issue, you almost always want to ensure that ASF is free to use any protocol that is currently supported and not just one or two. 除非您有**充分的**理由编辑此属性, 否则应将其保留为默认值。

* * *

### `UpdateChannel`

这是一个默认值为 `1` 的 `byte` 类型属性。 This property defines update channel which is being used, either for auto-updates (if `UpdatePeriod` is greater than `0`), or update notifications (otherwise). Currently ASF supports three update channels - `0` which is called `None`, `1`, which is called `Stable`, and `2`, which is called `Experimental`. `Stable` channel is the default release channel, which should be used by majority of users. `Experimental` channel in addition to `Stable` releases, also includes **pre-releases** dedicated for advanced users and other developers in order to test new features, confirm bugfixes or give feedback about planned enhancements. **Experimental versions often contain unpatched bugs, work-in-progress features or rewritten implementations**. If you don't consider yourself advanced user, please stay with default `1` (Stable) update channel. `Experimental` channel is dedicated to users who know how to report bugs, deal with issues and give feedback - no technical support will be given. 如果您想了解更多，请阅读&#8203;**[发布周期](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle-zh-CN)**。 You can also set `UpdateChannel` to `0` (`None`), if you want to completely remove all version checks. Setting `UpdateChannel` to `0` will entirely disable entire functionality related to updates, including `update` command. Using `None` channel is **strongly discouraged** due to exposing yourself to all sort of problems (mentioned in `UpdatePeriod` description below).

**除非您明确知道自己在做什么**，否则我们**强烈**建议您将其保留为默认值。

* * *

### `UpdatePeriod`

这是一个默认值为 `24` 的 `byte` 类型属性。 This property defines how often ASF should check for auto-updates. Updates are crucial not only to receive new features and critical security patches, but also to receive bugfixes, performance enhancements, stability improvements and more. When a value greater than `0` is set, ASF will automatically download, replace, and restart itself (if `AutoRestart` permits) when new update is available. In order to achieve this, ASF will check every `UpdatePeriod` hours if new update is available on our GitHub repo. A value of `0` disables auto-updates, but still allows you to execute `update` command manually. You might also be interested in setting appropriate `UpdateChannel` that `UpdatePeriod` should follow.

Update process of ASF involves update of entire folder structure that ASF is using, but without touching your own configs or databases located in `config` directory - this means that any extra files unrelated to ASF in its directory might be lost during update. Default value of `24` is a good balance between unnecessary checks, and ASF that is fresh enough.

Unless you have a **strong** reason to disable this feature, you should keep auto-updates enabled within reasonable `UpdatePeriod` **for your own good**. This is not only because we don't support anything but latest stable ASF release, but also because **we give our security guarantee only for latest version**. 如果您使用过时版本的 ASF，就会将自己暴露在各种问题下，例如小漏洞、功能失效，甚至 **[Steam 帐户永久停用](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ-zh-CN#曾经有人因此被封禁吗)**，所以我们**强烈建议**，为了您自己的利益，始终确保使用 ASF 的最新版本。 Auto-updates allow us to react quickly to all kind of issues by disabling or patching problematic code before it can escalate - if you opt out of it, you lose all of our security guarantees and risk consequences from running code that could be potentially harmful, not only to Steam network, but also (by definition) to your own Steam account.

* * *

### `WebLimiterDelay`

`ushort` type with default value of `200`. This property defines, in miliseconds, the minimum amount of delay between sending two consecutive requests to the same Steam web-service. Such delay is required as **[AkamaiGhost](https://www.akamai.com)** service that Steam uses internally includes rate-limiting based on global number of requests sent across given time period. In normal circumstances akamai block is rather hard to achieve, but under very heavy workloads with a huge ongoing queue of requests, it's possible to trigger it if we keep sending too many requests across too short time period.

Default value was set based on assumption that ASF is the only tool accessing Steam web-services, in particular `steamcommunity.com`, `api.steampowered.com` and `store.steampowered.com`. If you're using other tools sending requests to the same web-services then you should make sure that your tool includes similar functionality of `WebLimiterDelay` and set both to double of default value, which would be `400`. This guarantees that under no circumstance you'll exceed sending more than 1 request per 200 ms.

In general, lowering `WebLimiterDelay` under default value is **strongly discouraged** as it might lead to various IP-related blocks, some of which are possible to be permanent. Default value is good enough for running a single ASF instance on the server, as well as using ASF in normal scenario along with original Steam client. It should be correct for majority of usages, and you should only increase it (never lower it), if - apart from using ASF, you're also using another tool that might send excessive number of requests to the same web-services that ASF is making use of. In short, global number of all requests sent from a single IP to a single Steam domain should never exceed more than 1 request per 200 ms.

除非您有理由编辑此属性, 否则应将其保留为默认值。

* * *

### `WebProxy`

`string` type with default value of `null`. This property defines a web proxy address that will be used for all internal http and https requests sent by ASF's `HttpClient`, especially to services such as `github.com`, `steamcommunity.com` and `store.steampowered.com`. Proxying ASF requests in general has no advantages, but it's exceptionally useful for bypassing various kinds of firewalls, especially the great firewall in China.

This property is defined as uri string:

> A URI string is composed of a scheme (http or https), a host, and an optional port. An example of a complete uri string is `"http://contoso.com:8080"`.

If your proxy requires user authentication, you will also need to set up `WebProxyUsername` and/or `WebProxyPassword`. If there is no such need, setting up this property alone is sufficient.

Right now ASF uses web proxy only for `http` and `https` requests, which **do not** include internal Steam network communication done within ASF's internal Steam client. There are currently no plans for supporting that, mainly due to missing **[SK2](https://github.com/SteamRE/SteamKit)** functionality. If you need/want it to happen, I'd suggest starting from there.

除非您有理由编辑此属性, 否则应将其保留为默认值。

* * *

### `WebProxyPassword`

`string` type with default value of `null`. This property defines password field used in basic, digest, NTLM, and Kerberos authentication that is supported by a target `WebProxy` machine providing proxy functionality. If your proxy doesn't require user credentials, there is no need for you to input anything here. Using this option makes sense only if `WebProxy` is used as well, as it has no effect otherwise.

除非您有理由编辑此属性, 否则应将其保留为默认值。

* * *

### `WebProxyUsername`

`string` type with default value of `null`. This property defines username field used in basic, digest, NTLM, and Kerberos authentication that is supported by a target `WebProxy` machine providing proxy functionality. If your proxy doesn't require user credentials, there is no need for you to input anything here. Using this option makes sense only if `WebProxy` is used as well, as it has no effect otherwise.

除非您有理由编辑此属性, 否则应将其保留为默认值。

* * *

**[回到顶部](#配置)**

* * *

## 机器人配置

As you should know already, every bot should have its own config based on example JSON structure below. Start from deciding how you want to name your bot (e.g. `1.json`, `main.json`, `primary.json` or `AnythingElse.json`) and head over to configuration.

**Notice:** Bot can't be named `ASF` (as that keyword is reserved for global config), ASF will also ignore all configuration files starting with a dot.

机器人配置文件有如下的结构：

```json
{
    "AcceptGifts": false,
    "AutoSteamSaleEvent": false,
    "BotBehaviour": 0,
    "CustomGamePlayedWhileFarming": null,
    "CustomGamePlayedWhileIdle": null,
    "Enabled": false,
    "FarmingOrders": [],
    "GamesPlayedWhileIdle": [],
    "HoursUntilCardDrops": 3,
    "IdlePriorityQueueOnly": false,
    "IdleRefundableGames": true,
    "LootableTypes": [1, 3, 5],
    "MatchableTypes": [5],
    "OnlineStatus": 1,
    "PasswordFormat": 0,
    "Paused": false,
    "RedeemingPreferences": 0,
    "SendOnFarmingFinished": false,
    "SendTradePeriod": 0,
    "ShutdownOnFarmingFinished": false,
    "SteamLogin": null,
    "SteamMasterClanID": 0,
    "SteamParentalCode": null,
    "SteamPassword": null,
    "SteamTradeToken": null,
    "SteamUserPermissions": {},
    "TradingPreferences": 0,
    "TransferableTypes": [1, 3, 5],
    "UseLoginKeys": true
}
```

**Tip:** In order for bot to work properly, you should edit at least `Enabled`, `SteamLogin` and `SteamPassword` properties. I also suggest to take a look at some fine-tuning such as `HoursUntilCardDrops`, but all of that is optional. ASF configs are quite advanced to allow you tune your bots and ASF however you want, if you don't "require" such advanced setup, you don't really have to go deep into each config property. It's up to you how simple or how complex ASF should be.

* * *

下面是对所有选项的解释：

### `AcceptGifts`

这是一个默认值为 `false` 的 `bool` 类型属性。 When enabled, ASF will automatically accept and redeem all steam gifts (including wallet gift cards) sent to the bot. This includes also gifts sent from users other than those defined in `SteamUserPermissions`. Keep in mind that gifts sent to e-mail address are not directly forwarded to the client, so ASF won't accept those without your help.

This option is recommended only for alt accounts, as it's very likely that you don't want to automatically redeem all gifts sent to your primary account. If you're unsure whether you want this feature enabled or not, keep it with default value of `false`.

* * *

### `AutoSteamSaleEvent`

这是一个默认值为 `false` 的 `bool` 类型属性。 During Steam summer/winter sale events Steam is known for providing you extra cards for browsing discovery queue each day, as well as voting in the Steam awards. When this option is enabled, ASF will automatically check Steam discovery queue and Steam awards each 6 hours, and clear them if needed. This option is not recommended if you want to do those actions yourself, and typically it should make sense only on bot accounts. Moreover, you need to ensure that your account is at least of level `8` if you expect to receive those cards in the first place. If you're unsure whether you want this feature enabled or not, keep it with default value of `false`.

Please note that due to constant Valve issues, changes and problems, **we give no guarantee whether this function will work properly**, therefore it's entirely possible that this option **will not work at all**. We do not accept **any** bug reports, neither support requests for this option. It's offered with absolutely no guarantees, you're using it at your own risk.

* * *

### `BotBehaviour`

`byte flags` type with default value of `0`. This property defines ASF bot-like behaviour during various events, and is defined as below:

| 值  | 名称                            | 描述                                                                    |
| -- | ----------------------------- | --------------------------------------------------------------------- |
| 0  | None                          | No special bot behaviour, the least invasive mode, default            |
| 1  | RejectInvalidFriendInvites    | Will cause ASF to reject (instead of ignoring) invalid friend invites |
| 2  | RejectInvalidTrades           | Will cause ASF to reject (instead of ignoring) invalid trade offers   |
| 4  | RejectInvalidGroupInvites     | Will cause ASF to reject (instead of ignoring) invalid group invites  |
| 8  | DismissInventoryNotifications | Will cause ASF to automatically dismiss all inventory notifications   |
| 16 | MarkReceivedMessagesAsRead    | Will cause ASF to automatically mark all received messages as read    |

Please notice that this property is `flags` field, therefore it's possible to choose any combination of available values. 如果您想了解更多，请阅读 **[flags 映射](#json-映射)**。 Not enabling any of flags results in `None` option.

In general you want to modify this property if you expect from ASF to do certain amount of automation related to its activity, as it'd be expected from a bot account, but not a primary account used in ASF. Therefore, changing this property makes sense mainly for alt accounts, although you're free to use selected options for main accounts as well.

Normal (`None`) ASF behaviour is to only automate things that user wants (e.g. cards farming or `SteamTradeMatcher` offers, if set in `TradingPreferences`). This is the least invasive mode, and it's beneficial to majority of users since you remain in full control over your account and you can decide yourself whether to allow certain out-of-scope interactions, or not.

Invalid friend invite is an invite that doesn't come from user with `FamilySharing` permission (defined in `SteamUserPermissions`) or above. ASF in normal mode ignores those invites, as you'd expect, giving you free choice whether to accept them, or not. `RejectInvalidFriendInvites` will cause those invites to be automatically rejected, which will practically disable option for other people to add you to their friend list (as ASF will deny all such requests, apart from people defined in `SteamUserPermissions`). Unless you want to outright deny all friend invites, you shouldn't enable this option.

Invalid trade offer is an offer that isn't accepted through built-in ASF module. 关于这一情况的更多信息，可以在&#8203;**[交易](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading-zh-CN)**&#8203;章节中找到，这一章节明确定义了 ASF 将会自动接受何种类型的交易报价。 Valid trades are also defined by other settings, especially `TradingPreferences`. `RejectInvalidTrades` will cause all invalid trade offers to be rejected, instead of being ignored. Unless you want to outright deny all trade offers that aren't automatically accepted by ASF, you shouldn't enable this option.

Invalid group invite is an invite that doesn't come from `SteamMasterClanID` group. ASF in normal mode ignores those group invites, as you'd expect, allowing you to decide yourself if you want to join particular Steam group or not. `RejectInvalidGroupInvites` will cause all those group invites to be automatically rejected, effectively making it impossible to invite you to any other group than `SteamMasterClanID`. Unless you want to outright deny all group invites, you shouldn't enable this option.

If you're unsure how to configure this option, it's best to leave it at default.

* * *

### `CustomGamePlayedWhileFarming`

`string` type with default value of `null`. When ASF is farming, it can display itself as "Playing non-steam game: `CustomGamePlayedWhileFarming`" instead of currently farmed game. This can be useful if you would like to let your friends know that you're farming, yet you don't want to change default `OnlineStatus`. Please note that ASF cannot guarantee the actual display order of Steam network, therefore this is only a suggestion that may, or may not, display properly. Default value of `null` disables this feature.

* * *

### `CustomGamePlayedWhileIdle`

`string` type with default value of `null`. Similar to `CustomGamePlayedWhileFarming`, but for the situation when ASF has nothing to do (as account is fully farmed). Default value of `null` disables this feature.

* * *

### `Enabled`

这是一个默认值为 `false` 的 `bool` 类型属性。 This property defines if bot is enabled. Enabled bot instance (`true`) will automatically start on ASF run, while disabled bot instance (`false`) will need to be started manually. By default every bot is disabled, so you probably want to switch this property to `true` for all of your bots that should be started automatically.

* * *

### `FarmingOrders`

`ImmutableHashSet<byte>` type with default value of being empty. This property defines the **preferred** farming order used by ASF for given bot account. Currently there are following farming orders available:

| 值  | 名称                        | 描述                                                                               |
| -- | ------------------------- | -------------------------------------------------------------------------------- |
| 0  | Unordered                 | No sorting, slightly improving CPU performance                                   |
| 1  | AppIDsAscending           | Try to farm games with lowest `appID`s first                                     |
| 2  | AppIDsDescending          | Try to farm games with highest `appID`s first                                    |
| 3  | CardDropsAscending        | Try to farm games with lowest number of card drops remaining first               |
| 4  | CardDropsDescending       | Try to farm games with highest number of card drops remaining first              |
| 5  | HoursAscending            | Try to farm games with lowest number of hours played first                       |
| 6  | HoursDescending           | Try to farm games with highest number of hours played first                      |
| 7  | NamesAscending            | Try to farm games in alphabetical order, starting from A                         |
| 8  | NamesDescending           | Try to farm games in reverse alphabetical order, starting from Z                 |
| 9  | Random                    | Try to farm games in totally random order (different on each run of the program) |
| 10 | BadgeLevelsAscending      | Try to farm games with lowest badge levels first                                 |
| 11 | BadgeLevelsDescending     | Try to farm games with highest badge levels first                                |
| 12 | RedeemDateTimesAscending  | Try to farm oldest games on our account first                                    |
| 13 | RedeemDateTimesDescending | Try to farm newest games on our account first                                    |
| 14 | MarketableAscending       | Try to farm games with unmarketable card drops first                             |
| 15 | MarketableDescending      | Try to farm games with marketable card drops first                               |

Since this property is an array, it allows you to use several different settings in your fixed order. For example, you can include values of `15`, `11` and `7` in order to sort by marketable games first, then by those with highest badge level, and finally alphabetically. As you can guess, the order actually matters, as reverse one (`7`, `11` and `15`) achieves something entirely different. Majority of people will probably use just one order out of all of them, but in case you want to, you can also sort further by extra parameters.

需要注意的是上表中所有描述都含有词汇“尝试”——ASF 实际采用的顺序非常受所选的&#8203;**[挂卡算法](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance-zh-CN)**&#8203;影响，并且 ASF 只会在不影响性能的情况下尝试进行排序。 For example, in `Simple` algorithm, selected `FarmingOrders` should be entirely respected in current farming session (as every game has the same performance value), while in `Complex` algorithm actual order is affected by hours first, and then sorted according to chosen `FarmingOrders`. This will lead to different results, as games with existing playtime will have a priority over others, so effectively ASF will prefer games with highest playtime first, and only then sorting everything further by your chosen `FarmingOrders`. Therefore, this config property is only a **suggestion** that ASF will try to respect, as long as it doesn't affect performance negatively (in this case, ASF will always prefer performance over `FarmingOrders`).

此外，您可以通过 `iq` **[命令](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-CN)**&#8203;访问“挂卡优先级队列”。 如果使用这个队列，则实际的挂卡顺序首先由性能决定，其次受挂卡优先级队列影响，最后才是 ` FarmingOrders`。

* * *

### `GamesPlayedWhileIdle`

`ImmutableHashSet<uint>` type with default value of being empty. If ASF has nothing to farm it can play your specified steam games (`appID`s) instead. Playing games in such manner increases your "hours played" of those games, but nothing else apart of it. This feature can be enabled at the same time with `CustomGamePlayedWhileIdle` in order to play your selected games while showing custom status in Steam network, but in this case, like in `CustomGamePlayedWhileFarming` case, the actual display order is not guaranteed. Please note that Steam allows ASF to play only up to `32` `appID`s, therefore if you put more games than that, only first `32` will be respected (and extra ones being ignored).

* * *

### `HoursUntilCardDrops`

这是一个默认值为 `3` 的 `byte` 类型属性。 This property defines if account has card drops restricted, and if yes, for how many initial hours. Restricted card drops means that account is not receiving any card drops from given game until the game is played for at least `HoursUntilCardDrops` hours. Unfortunately there is no magical way to detect that, so ASF relies on you. 这个属性会影响对&#8203;**[挂卡算法](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance-zh-CN)**&#8203;的选择。 Setting this property properly will maximize profits and minimize time required for cards to be farmed. Remember that there is no obvious answer whether you should use one or another value, since it fully depends on your account. It seems that older accounts which never asked for refund have unrestricted card drops, so they should use a value of `0`, while new accounts and those who did ask for refund have restricted card drops with a value of `3`. This is however only theory, and should not be taken as a rule. The default value for this property was set based on "lesser evil" and majority of use cases.

* * *

### `IdlePriorityQueueOnly`

这是一个默认值为 `false` 的 `bool` 类型属性。 这个属性定义 ASF 是否应该仅自动挂您通过 `iq` **[命令](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-CN)**&#8203;添加到挂卡优先级队列内的应用。 When this option is enabled, ASF will skip all `appIDs` that are missing on the list, effectively allowing you to cherry-pick games for automatic ASF idling. Keep in mind that if you didn't add any games to the queue then ASF will act as if there is nothing to idle on your account. If you're unsure whether you want this feature enabled or not, keep it with default value of `false`.

* * *

### `IdleRefundableGames`

这是一个默认值为 `true` 的 `bool` 类型属性。 This property defines if ASF is permitted to idle games that are still refundable. A refundable game is a game that we bought in last 2 weeks through Steam Store and we didn't play it for longer than 2 hours yet, as stated **[here](https://store.steampowered.com/steam_refunds)**. By default when this option is set to `true`, ASF ignores Steam refunds entirely and idles everything, as most people expect. However, you can change this option to `false` if you want to ensure that ASF won't idle any of your refundable games too soon, allowing you to evaluate those games yourself and refund if needed without worrying about ASF affecting playtime negatively. Please note that if you disable this option then games you purchased from Steam Store won't be idled by ASF for up to 14 days since redeem date. If you're unsure whether you want this feature enabled or not, keep it with default value of `true`.

* * *

### `LootableTypes`

`ImmutableHashSet<byte>` type with default value of `1, 3, 5` steam item types. This property defines ASF behaviour when looting - both manual and automatic. ASF will ensure that only items from `LootableTypes` will be included in a trade offer, therefore this property allows you to choose what you want to receive in a trade offer that is being sent to you.

| 值 | 名称                | 描述                                                            |
| - | ----------------- | ------------------------------------------------------------- |
| 0 | Unknown           | Every type that doesn't fit in any of the below               |
| 1 | BoosterPack       | Booster pack containing 3 random cards from a game            |
| 2 | Emoticon          | Emoticon to use in Steam Chat                                 |
| 3 | FoilTradingCard   | Foil variant of `TradingCard`                                 |
| 4 | ProfileBackground | Profile background to use on your Steam profile               |
| 5 | TradingCard       | Steam trading card, being used for crafting badges (non-foil) |
| 6 | SteamGems         | Steam gems being used for crafting boosters, sacks included   |

Please note that regardless of the settings above, ASF will only ask for Steam (`appID` of 753) community (`contextID` of 6) items, so all game items, gifts and likewise, are excluded from the trade offer by definition.

Default ASF setting is based on most common usage of the bot, with looting only booster packs, and trading cards (including foils). The property defined here allows you to alter that behaviour in whatever way that satisfies you. Please keep in mind that all types not defined above will show as `Unknown` type, which is especially important when Valve releases some new Steam item, that will be marked as `Unknown` by ASF as well, until it's added here (in the future release). That's why in general it's not recommended to include `Unknown` type in your `LootableTypes`, unless you know what you're doing, and you also understand that ASF will send your entire inventory in a trade offer if Steam Network gets broken again and reports all your items as `Unknown`. My strong suggestion is to not include `Unknown` type in the `LootableTypes`, even if you expect to loot everything.

* * *

### `MatchableTypes`

`ImmutableHashSet<byte>` type with default value of `5` Steam item types. This property defines which Steam item types are permitted to be matched when `SteamTradeMatcher` option in `TradingPreferences` is enabled. Types are defined as below:

| 值 | 名称                | 描述                                                            |
| - | ----------------- | ------------------------------------------------------------- |
| 0 | Unknown           | Every type that doesn't fit in any of the below               |
| 1 | BoosterPack       | Booster pack containing 3 random cards from a game            |
| 2 | Emoticon          | Emoticon to use in Steam Chat                                 |
| 3 | FoilTradingCard   | Foil variant of `TradingCard`                                 |
| 4 | ProfileBackground | Profile background to use on your Steam profile               |
| 5 | TradingCard       | Steam trading card, being used for crafting badges (non-foil) |
| 6 | SteamGems         | Steam gems being used for crafting boosters, sacks included   |

Of course, types that you should use for this property typically include only `2`, `3`, `4` and `5`, as only those types are supported by STM. Please note that **ASF is not a trading bot** and **will NOT care about price or rarity**, which means that if you use it e.g. with `Emoticon` type, then ASF will be happy to trade your 2x rare emoticon for 1x rare 1x common, as that makes progress towards badge (in this case emoticons) completion. Please evaluate twice if you're fine with that. Unless you know what you're doing, you should keep it with default value of `5`.

* * *

### `OnlineStatus`

这是一个默认值为 `1` 的 `byte` 类型属性。 This property specifies Steam community status that the bot will be announced with after logging in to Steam network. Currently you can choose one of below statuses:

| 值 | 名称                  |
| - | ------------------- |
| 0 | Offline（离线）         |
| 1 | Online（在线）          |
| 2 | Busy（忙碌）            |
| 3 | Away（离开）            |
| 4 | Snooze（打盹）          |
| 5 | LookingToTrade（想交易） |
| 6 | LookingToPlay（想玩游戏） |
| 7 | Invisible（隐身）       |

`Offline` status is extremely useful for primary accounts. As you should know, farming a game actually shows your steam status as "Playing game: XXX", which can be misleading to your friends, confusing them that you're playing a game while actually you're only farming it. Using `Offline` status solves that issue - your account will never be shown as "in-game" when you're farming steam cards with ASF. This is possible thanks to the fact that ASF does not have to sign in into Steam Community in order to work properly, so we're in fact playing those games, connected to Steam network, but without announcing our online presence at all. Keep in mind that played games using offline status will still count towards your playtime, and show as "recently played" on your profile.

In addition to that, this feature is also important if you want to receive notifications and unread messages when ASF is running, while not keeping Steam client open at the same time. This is because ASF acts as a Steam client itself, and whether ASF would like it or not, Steam broadcasts all those messages and other events to it. This is not a problem if you have both ASF and your own Steam client running, as both clients receive exactly the same events. However, if just ASF is running, Steam network could mark certain events and messages as "delivered", despite of your traditional Steam client not receiving it due to not being present. Offline status also solves this problem, as ASF is never considered for any community events in this case, so all unread messages and other events will be properly marked as unread when you come back.

It's important to note that ASF running on `Offline` mode will **not** be able to receive commands in usual Steam chat way, as the chat, as well as entire community presence is in fact, entirely offline. A solution to this issue is using `Invisible` mode instead which works in a similar way (not exposing status), but keeps the ability to receive and respond to messages (so also a potential to dismiss notifications and unread messages as stated above). `Invisible` mode makes the most sense on alt accounts that you don't want to expose (status-wise), but still be able to send commands to.

However, there is one catch with `Invisible` mode - it doesn't go well with primary accounts. This is because **any** Steam session that is currently online **exposes** the status, even if ASF itself does not. This is caused by the current limitation/bug of the Steam network that isn't possible to be fixed on ASF side, so if you want to use `Invisible` mode you will also need to ensure that **all** other sessions to the same account use `Invisible` mode as well. This will be the case on alt accounts where ASF is hopefully the only active session, but on primary accounts you'll almost always prefer to show as `Online` to your friends, hiding only ASF activity, and in this case `Invisible` mode will be entirely useless for you (we recommend to use `Offline` mode instead). Hopefully this limitation/bug will be eventually solved in the future by Valve, but I wouldn't expect that to happen anytime soon...

If you're unsure how to set up this property, it's recommended to use a value of `0` (`Offline`) for primary accounts, and default `1` (`Online`) otherwise.

* * *

### `PasswordFormat`

这是一个默认值为 `0` 的 `byte` 类型属性。 This property defines the format of `SteamPassword` property, and currently supports - `0` for `PlainText`, `1` for `AES` and `2` for `ProtectedDataForCurrentUser`. 如果您需要了解更多，请参考&#8203;**[安全性](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security-zh-CN)**&#8203;章节，确保 `SteamPassword` 属性的值确实符合 `PasswordFormat` 定义的格式。 In other words, when you change `PasswordFormat` then your `SteamPassword` should be **already** in that format, not just aiming to be. Unless you know what you're doing, you should keep it with default value of `0`.

* * *

### `Paused`

这是一个默认值为 `false` 的 `bool` 类型属性。 This property defines initial state of `CardsFarmer` module. With default value of `false`, bot will automatically start farming when it's started, either because of `Enabled` or `start` command. 只有您希望手动 `resume`（恢复）自动挂卡进程时，才应该将这个属性设置为 `true`。例如，您可能只使用 `play` 命令，从来都不用 `CardFarmer`（挂卡）模块——这一属性与发送 `pause` **[命令](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-CN)**&#8203;的效果完全相同。 If you're unsure whether you want this feature enabled or not, keep it with default value of `false`.

* * *

### `RedeemingPreferences`

`byte flags` type with default value of `0`. This property defines ASF behaviour when redeeming cd-keys, and is defined as below:

| 值 | 名称               | 描述                                                                             |
| - | ---------------- | ------------------------------------------------------------------------------ |
| 0 | None             | No redeeming preferences, typical                                              |
| 1 | Forwarding       | Forward keys unavailable to redeem to other bots                               |
| 2 | Distributing     | Distribute all keys among itself and other bots                                |
| 4 | KeepMissingGames | Keep keys for (potentially) missing games when forwarding, leaving them unused |

Please notice that this property is `flags` field, therefore it's possible to choose any combination of available values. 如果您想了解更多，请阅读 **[flags 映射](#json-映射)**。 Not enabling any of flags results in `None` option.

`Forwarding` will cause bot to forward a key that is not possible to redeem, to another connected and logged on bot that is missing that particular game (if possible to check). The most common situation is forwarding `AlreadyPurchased` game to another bot that is missing that particular game, but this option also covers other scenarios, such as `DoesNotOwnRequiredApp`, `RateLimited` or `RestrictedCountry`.

`Distributing` will cause bot to distribute all received keys among itself and other bots. This means that every bot will get a single key from the batch. Typically this is used only when you're redeeming many keys for the same game, and you want to evenly distribute them among your bots, as opposed to redeeming keys for various different games. This feature makes no sense if you're redeeming only one key in a single `redeem` action (as there are no extra keys to be distributed).

`KeepMissingGames` will cause bot to skip `Forwarding` when we can't be sure if key being redeemed is in fact owned by our bot, or not. This basically means that `Forwarding` will apply **only** to `AlreadyPurchased` keys, instead of covering also other cases such as `DoesNotOwnRequiredApp`, `RateLimited` or `RestrictedCountry`. Typically you might want to use this option on primary account, to ensure that keys being redeemed on it won't be forwarded further if your bot for example becomes temporarily `RateLimited`. As you can guess from the description, this field has absolutely no effect if `Forwarding` is not enabled.

Enabling both `Forwarding` and `Distributing` will add distributing feature on top of forwarding one, which makes ASF trying to redeem one key on all bots firstly (forwarding) before moving to the next one (distributing). Typically you want to use this option only when you want `Forwarding`, but with altered behaviour of switching the bot on key being used, instead of always going in-order with every key (which would be `Forwarding` alone). This behaviour can be beneficial if you know that majority or even all of your keys are tied to the same game, because in this situation `Forwarding` alone would try to redeem everything on one bot firstly (which makes sense if your keys are for unique games), and `Forwarding` + `Distributing` will switch the bot on the next key, "distributing" the task of redeeming new key onto another bot than the initial one (which makes sense if keys are for the same game, skipping one pointless attempt per key).

The actual bots order for all of the redeeming scenarios is alphabetical, excluding bots that are unavailable (not connected, stopped or likewise). Please keep in mind that there is per-IP and per-account hourly limit of redeeming tries, and every redeem try that didn't end with `OK` contributes to failed tries. ASF will do its best to minimize number of `AlreadyPurchased` failures, e.g. by trying to avoid forwarding a key to another bot that already owns that particular game, but it's not always guaranteed to work due to how Steam is handling licenses. Using redeeming flags such as `Forwarding` or `Distributing` will always increase your likelyhood to hit `RateLimited`.

Also keep in mind that you can't forward or distribute keys to bots that you do not have access to. 这应该是很显然的，但是您需要确保您至少对激活过程中的机器人拥有 `Operator` 权限，例如使用 `status ASF` **[命令](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-CN)**&#8203;检查。

* * *

### `SendOnFarmingFinished`

这是一个默认值为 `false` 的 `bool` 类型属性。 When ASF is done with farming given account, it can automatically send steam trade containing everything farmed up to this point to user with `Master` permission, which is very convenient if you don't want to bother with trades yourself. This option works the same as `loot` command, therefore keep in mind that it requires user with `Master` permission set, you might also require valid `SteamTradeToken`, including using an account that is actually eligible for trading. In addition to initiating `loot` after finishing farming, ASF will also initiate `loot` on each new items notification (when not farming), and after completing each trade that results in new items (always) when this option is active. This is especially useful for "forwarding" items received from other people to our account. 强烈建议您在启用 **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-zh-CN)** 的情况下使用此功能，因为如果需要您手动确认才能发送交易报价，自动化也就毫无意义。 If you're not sure how to set this property, leave it with default value of `false`.

* * *

### `SendTradePeriod`

这是一个默认值为 `0` 的 `byte` 类型属性。 This property works very similar to `SendOnFarmingFinished` property, with one difference - instead of sending trade when farming is done, we can also send it every `SendTradePeriod` hours, regardless of how much we have to farm left. This is useful if you want to `loot` your alt accounts on usual basis instead of waiting for it to finish farming. Default value of `0` disables this feature, if you want your bot to send you trade e.g. every day, you should put `24` here. 强烈建议您在启用 **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-zh-CN)** 的情况下使用此功能，因为如果需要您手动确认才能发送交易报价，自动化也就毫无意义。 If you're not sure how to set this property, leave it with default value of `0`.

* * *

### `ShutdownOnFarmingFinished`

这是一个默认值为 `false` 的 `bool` 类型属性。 ASF is "occupying" an account for the whole time of process being active. When given account is done with farming, ASF periodically checks it (every `IdleFarmingPeriod` hours), if perhaps some new games with steam cards were added in the meantime, so it can resume farming of that account without a need to restart the process. This is useful for majority of people, as ASF can automatically resume farming when needed. However, you may actually want to stop the process when given account is fully farmed, you can achieve that by setting this property to `true`. When enabled, ASF will proceed with logging off when account is fully farmed, which means that it won't be periodically checked or occupied anymore. You should decide yourself if you prefer ASF to work on given bot instance for the whole time, or if perhaps ASF should stop it when farming process is done. 如果所有帐户都停止运行，并且程序没有以 `--process-required` **[模式](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-Line-Arguments-zh-CN)**&#8203;运行，则 ASF 本身也会关闭。 If you're not sure how to set this property, leave it with default value of `false`.

* * *

### `SteamLogin`

`string` type with default value of `null`. This property defines your steam login - the one you use for logging in to steam. In addition to defining steam login here, you may also keep default value of `null` if you want to enter your steam login on each ASF startup instead of putting it in the config. This may be useful for you if you don't want to save sensitive data in config file.

* * *

### `SteamMasterClanID`

`ulong` type with default value of `0`. This property defines the steamID of the steam group that bot should automatically join, including its group chat. You can check steamID of your group by navigating to its **[page](https://steamcommunity.com/groups/archiasf)**, then adding `/memberslistxml?xml=1` to the end of the link, so the link will look like **[this](https://steamcommunity.com/groups/archiasf/memberslistxml?xml=1)**. Then you can get steamID of your group from the result, it's in `<groupID64>` tag. In above example it would be `103582791440160998`. In addition to trying to join given group at startup, the bot will also automatically accept group invites to this group, which makes it possible for you to invite your bot manually if your group has private membership. If you don't have any group dedicated for your bots, you should keep this property with default value of `0`.

* * *

### `SteamParentalCode`

`string` type with default value of `null`. This property defines your steam parental PIN. ASF requires an access to resources protected by steam parental, therefore if you use that feature, you need to provide ASF with parental unlock PIN, so it can operate normally. Default value of `null` means that there is no steam parental PIN required to unlock this account, and this is probably what you want if you don't use steam parental functionality. In addition to defining steam parental PIN here, you may also use value of `0` if you want to enter your steam parental PIN on each ASF startup instead of putting it in the config. This may be useful for you if you don't want to save sensitive data in config file.

* * *

### `SteamPassword`

`string` type with default value of `null`. This property defines your steam password - the one you use for logging in to steam. In addition to defining steam password here, you may also keep default value of `null` if you want to enter your steam password on each ASF startup instead of putting it in the config. This may be useful for you if you don't want to save sensitive data in config file.

* * *

### `SteamTradeToken`

`string` type with default value of `null`. When you have your bot on your friend list, then bot can send a trade to you right away without worrying about trade token, therefore you can leave this property at default value of `null`. If you however decide to NOT have your bot on your friend list, then you will need to generate and fill a trade token as the user that this bot is expecting to send trades to. In other words, this property should be filled with trade token of the account that is defined with `Master` permission in `SteamUserPermissions` of **this** bot instance.

In order to find your token, as logged in user with `Master` permission, navigate **[here](https://steamcommunity.com/my/tradeoffers/privacy)** and take a look at your trade URL. The token we're looking for is made out of 8 characters after `&token=` part in your trade URL. You should copy and put those 8 characters here, as `SteamTradeToken`. Do not include whole trading URL, neither `&token=` part, only token itself.

* * *

### `SteamUserPermissions`

`ImmutableDictionary<ulong, byte>` type with default value of being empty. This property is a dictionary property which maps given Steam user identified by his 64-bit steam ID, to `byte` number that specifies his permission in ASF instance. Currently available bot permissions in ASF are defined as:

| 值 | 名称            | 描述                                                                                                                                                                                                 |
| - | ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0 | None          | No permission, this is mainly a reference value that is assigned to steam IDs missing in this dictionary - there is no need to define anybody with this permission                                 |
| 1 | FamilySharing | Provides minimum access for family sharing users. Once again, this is mainly a reference value since ASF is capable of automatically discovering steam IDs that we permitted for using our library |
| 2 | Operator      | Provides basic access to given bot instances, mainly adding licenses and redeeming keys                                                                                                            |
| 3 | Master        | Provides full access to given bot instance                                                                                                                                                         |

In short, this property allows you to handle permissions for given users. 权限主要用于访问 ASF **[命令](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-CN)**，但也用于启用很多其他 ASF 功能，例如接受交易报价。 For example you might want to set your own account as `Master`, and give `Operator` access to 2-3 of your friends so they can easily redeem keys for your bot with ASF, while **not** being eligible e.g. for stopping it. Thanks to that you can easily assign permissions to given users and let them use your bot to some specified by you degree.

We recommend to set exactly one user as `Master`, and any amount you wish as `Operator`s and below. While it's technically possible to set multiple `Master`s and ASF will work correctly with them, for example by accepting all of their trades sent to the bot, ASF will use only one of them (with lowest steam ID) for every action that requires a single target, for example a `loot` request, so also properties like `SendOnFarmingFinished` or `SendTradePeriod`. If you perfectly understand those limitations, especially the fact that `loot` request will always send items to the `Master` with lowest steam ID, regardless of the `Master` that actually executed the command, then you can define multiple users with `Master` permission here, but we still recommend a single master scheme - multiple masters scheme is discouraged setup that is not supported.

It's nice to note that there is one more extra `Owner` permission, which is declared as `SteamOwnerID` global config property. You can't assign `Owner` permission to anybody here, as `SteamUserPermissions` property defines only permissions that are related to the bot instance, and not ASF as a process.

* * *

### `TradingPreferences`

`byte flags` type with default value of `0`. This property defines ASF behaviour when in trading, and is defined as below:

| 值 | 名称                  | 描述                                                                                                                                                                           |
| - | ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0 | None                | No trading preferences - accepts only `Master` trades                                                                                                                        |
| 1 | AcceptDonations     | Accepts trades in which we're not losing anything                                                                                                                            |
| 2 | SteamTradeMatcher   | Accepts dupes-matching **[STM](https://www.steamtradematcher.com)**-like trades. 访问&#8203;**[交易](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading)**&#8203;获得更多信息 |
| 4 | MatchEverything     | Requires `SteamTradeMatcher` to be set, and in combination with it - also accepts bad trades in addition to good and neutral ones                                            |
| 8 | DontAcceptBotTrades | Doesn't automatically accept `loot` trades from other bot instances                                                                                                          |

Please notice that this property is `flags` field, therefore it's possible to choose any combination of available values. 如果您想了解更多，请阅读 **[flags 映射](#json-映射)**。 Not enabling any of flags results in `None` option.

若要了解 ASF 的交易逻辑，以及对于每个 flag 的详细说明，请阅读&#8203;**[交易](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading-zh-CN)**&#8203;章节。

* * *

### `TransferableTypes`

`ImmutableHashSet<byte>` type with default value of `1, 3, 5` steam item types. 这个属性定义了在使用 `transfer` **[命令](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-CN)**&#8203;时，机器人之间可转移 Steam 物品的类型。 ASF will ensure that only items from `TransferableTypes` will be included in a trade offer, therefore this property allows you to choose what you want to receive in a trade offer that is being sent to one of your bots.

| 值 | 名称                | 描述                                                            |
| - | ----------------- | ------------------------------------------------------------- |
| 0 | Unknown           | Every type that doesn't fit in any of the below               |
| 1 | BoosterPack       | Booster pack containing 3 random cards from a game            |
| 2 | Emoticon          | Emoticon to use in Steam Chat                                 |
| 3 | FoilTradingCard   | Foil variant of `TradingCard`                                 |
| 4 | ProfileBackground | Profile background to use on your Steam profile               |
| 5 | TradingCard       | Steam trading card, being used for crafting badges (non-foil) |
| 6 | SteamGems         | Steam gems being used for crafting boosters, sacks included   |

Please note that regardless of the settings above, ASF will only ask for Steam (`appID` of 753) community (`contextID` of 6) items, so all game items, gifts and likewise, are excluded from the trade offer by definition.

Default ASF setting is based on most common usage of the bot, with transfering only booster packs, and trading cards (including foils). The property defined here allows you to alter that behaviour in whatever way that satisfies you. Please keep in mind that all types not defined above will show as `Unknown` type, which is especially important when Valve releases some new Steam item, that will be marked as `Unknown` by ASF as well, until it's added here (in the future release). That's why in general it's not recommended to include `Unknown` type in your `TransferableTypes`, unless you know what you're doing, and you also understand that ASF will send your entire inventory in a trade offer if Steam Network gets broken again and reports all your items as `Unknown`. My strong suggestion is to not include `Unknown` type in the `TransferableTypes`, even if you expect to transfer everything.

* * *

### `UseLoginKeys`

这是一个默认值为 `true` 的 `bool` 类型属性。 This property defines if ASF should use login keys mechanism for this Steam account. Login keys mechanism works very similar to official Steam client's "remember me" option, which makes it possible for ASF to store and use temporary one-time use login key for next logon attempt, effectively skipping a need of providing password, Steam Guard or 2FA code as long as our login key is valid. Login key is stored in `BotName.db` file and updated automatically. This is why you don't need to provide password/SteamGuard/2FA code after logging in successfully with ASF just once.

登录密钥主要用来为您提供方便，使您无需每次登录都要输入 `SteamPassword`、Steam 令牌或者两步验证代码（当没有启用 **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-zh-CN)** 时）。 It's also superior alternative since login key can be used only for a single time and does not reveal your original password in any way. Exactly the same method is being used by your original Steam client, which saves your account name and login key for your next logon attempt, effectively being the same as using `SteamLogin` with `UseLoginKeys` and empty `SteamPassword` in ASF.

However, some people might be concerned even about this little detail, therefore this option is available here for you if you'd like to ensure that ASF won't store any kind of token that would allow resuming previous session after being closed, which will result in full authentication being mandatory on each login attempt. Disabling this option will work exactly the same as not checking "remember me" in official Steam client. Unless you know what you're doing, you should keep it with default value of `true`.

**[回到顶部](#配置)**

* * *

## 文件结构

ASF 采用这种很简单的文件结构：

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
    

In order to move ASF to new location, for example another PC, it's enough to move/copy `config` directory alone, and that's the recommended way of doing any form of "ASF backups".

`log.txt` file holds the log generated by your last ASF run. This file doesn't contain any sensitive information, and is extremely useful when it comes to issues, crashes or simply as an information to you what happened in last ASF run. We will very often ask about for file if you run into issues or bugs. ASF 会自动管理这些文件，但如果您是一名高级用户，您可以进一步调整 ASF 的&#8203;**[日志](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Logging-zh-CN)**&#8203;模块。

`config` directory is the place that holds configuration for ASF, including all of its bots.

`ASF.json` is a global ASF configuration file. This config is used for specifying how ASF behaves as a process, which affects all of the bots and program itself. You can find global properties there, such as ASF process owner, auto-updates or debugging.

`BotName.json` is a config of given bot instance. This config is used for specifying how given bot instance behaves, therefore those settings are specific to that bot only and not shared across other ones. This allows you to configure bots with various different settings and not necessarily all of them working in exactly the same way.

Apart from config files, ASF also uses `config` directory for storing databases.

`ASF.db` is a global ASF database file. It acts as a global persistent storage and is used for saving various information related to ASF process, such as IPs of local Steam servers. **您不应该编辑这个文件**。

`BotName.db` is a database of given bot instance. This file is used for storing crucial data about given bot instance in persistent storage, such as login keys or ASF 2FA. **您不应该编辑这个文件**。

`BotName.bin` is a special file of given bot instance, which holds information about Steam sentry hash. Sentry hash is used for authenticating using `SteamGuard` mechanism, very similar to Steam `ssfn` file. **您不应该编辑这个文件**。

`BotName.keys` 是一个特殊文件，用于向&#8203;**[后台游戏激活器](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer-zh-CN)**&#8203;中导入序列号。 It's not mandatory and not generated, but recognized by ASF. This file is automatically deleted after keys are successfully imported.

`BotName.maFile` 是一个特殊文件，用于导入 **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-zh-CN)**。 It's not mandatory and not generated, but recognized by ASF if your `BotName` does not use ASF 2FA yet. This file is automatically deleted after ASF 2FA is successfully imported.

**[回到顶部](#配置)**

* * *

## JSON 映射

Every configuration property has its type. Type of the property defines values that are valid for it. You can only use values that are valid for given type - if you use invalid value, then ASF won't be able to parse your config.

**We strongly recommend to use ConfigGenerator for generating configs** - it handles most of the low-level stuff (such as types validation) for you, so you only need to input proper values, and you also don't need to understand variable types specified below. This section is mainly for people generating/editing configs manually, so they know what values they can use.

Types used by ASF are native C# types, which are specified below:

* * *

`bool` - Boolean type accepting only `true` and `false` values.

Example: `"Enabled": true`

* * *

`byte` - Unsigned byte type, accepting only integers from `0` to `255` (inclusive).

Example: `"ConnectionTimeout": 60`

* * *

`uint` - Unsigned integer type, accepting only integers from `0` to `4294967295` (inclusive).

* * *

`ulong` - Unsigned long integer type, accepting only integers from `0` to `18446744073709551615` (inclusive).

Example: `"SteamMasterClanID": 103582791440160998`

* * *

`string` - String type, accepting any sequence of characters, including empty sequence `""` and `null`. Both empty sequence as well as `null` value is treated the same by ASF, so it's up to your preference which one you want to use.

Examples: `"SteamLogin": null`, `"SteamLogin": ""`, `"SteamLogin": "MyAccountName"`

* * *

`ImmutableHashSet<valueType>` - Immutable collection (set) of unique values in given `valueType`. In JSON, it's defined as array of elements in given `valueType`.

Example for `ImmutableHashSet<uint>`: `"Blacklist": [267420, 303700, 335590]`

* * *

`ImmutableDictionary<keyType, valueType>` - Immutable dictionary (map) that maps a key specified in its `keyType`, to value specified in its `valueType`. In JSON, it's defined as an object with key-value pairs. Keep in mind that `keyType` is always quoted in this case, even if it's value type such as `ulong`.

Example for `ImmutableDictionary<ulong, byte>`: `"SteamUserPermissions": { "76561198174813138": 3, "76561198174813137": 1 }`

* * *

`flags` - Flags attribute combines several different properties into one final value by applying bitwise operations. This allows you to choose any possible combination of various different allowed values at the same time. The final value is constructed as a sum of values of all enabled options.

For example, given following values:

| 值 | 名称   |
| - | ---- |
| 0 | None |
| 1 | A    |
| 2 | B    |
| 4 | C    |

Using `B + C` would result in value of `6`, using `A + C` would result in value of `5`, using `C` would result in value of `4` and so on. This allows you to create any possible combination of enabled values - if you decided to enable all of them, making `None + A + B + C`, you'd get value of `7`. Also notice that flag with value of `0` is enabled by definition in all other available combinations, therefore very often it's a flag that doesn't enable anything specifically (such as `None`).

So as you can see, in above example we have 3 available flags to switch on/off (`A`, `B`, `C`), and 8 possible values overall (`None -> 0`, `A -> 1`, `B -> 2`, `A+B -> 3`, `C -> 4`, `A+C -> 5`, `B+C -> 6`, `A+B+C -> 7`).

**[回到顶部](#配置)**

* * *

## 兼容性映射

Due to JavaScript limitations of being unable to properly serialize simple `ulong` fields in JSON when using web-based ConfigGenerator, `ulong` fields will be rendered as strings with `s_` prefix in the resulting config. This includes for example `"SteamOwnerID": 76561198006963719` that will be written by our ConfigGenerator as `"s_SteamOwnerID": "76561198006963719"`. ASF includes proper logic for handling this string mapping automatically, so `s_` entries in your configs are actually valid and correctly generated. If you're generating configs yourself, we recommend to stick with original `ulong` fields if possible, but if you're unable to do so, you can also follow this scheme and encode them as strings with `s_` prefix added to their names. We hope to resolve this JavaScript limitation eventually.

**[回到顶部](#配置)**

* * *

## 配置兼容性

It's top priority for ASF to remain compatible with older configs. As you should already know, missing config properties are treated the same as they would be defined with their **default values**. Therefore, if new config property gets introduced in new version of ASF, all your configs will remain **compatible** with new version, and ASF will treat that new config property as it'd be defined with its **default value**. You can always add, remove or edit config properties according to your needs. We recommend to limit defined config properties only to those that you want to change, since this way you automatically inherit default values for all other ones, not only keeping your config clean but also increasing compatibility in case we decide to change a default value for property that you don't want to explicitly set yourself.

**[回到顶部](#配置)**

* * *

## 自动重载

Starting with ASF V2.1.6.2+, the program is now aware of configs being modified "on-the-fly" - thanks to that, ASF will automatically:

- Create (and start, if needed) new bot instance, when you create its config
- Stop (if needed) and remove old bot instance, when you delete its config
- Stop (and start, if needed) any bot instance, when you edit its config
- Restart (if needed) the bot under new name, when you rename its config

All of the above is transparent and will be done automatically without a need of restarting the program, or killing other (unaffected) bot instances.

In addition to that, ASF will also restart itself (if `AutoRestart` permits) if you modify core ASF `ASF.json` config. Likewise, program will quit if you delete or rename it.

**[回到顶部](#配置)**