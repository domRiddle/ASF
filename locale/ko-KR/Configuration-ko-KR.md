# 환경설정

이 페이지는 ASF 환경설정에 대한 내용입니다. 이는 `config` 디렉토리에 대한 완전한 설명이며, ASF를 당신의 입맛에 맞게 조정하게 해줍니다.

- **[소개](#소개)**
- **[웹 기반 환경설정 생성기(ConfigGenerator)](#웹-기반-환경설정-생성기)**
- **[수동 환경설정](#수동-환경설정)**
- **[일반 환경설정](#일반-환경설정)**
- **[봇 환경설정](#봇-환경설정)**
- **[파일 구조](#파일-구조)**
- **[JSON 매핑](#json-매핑)**
- **[호환성 매핑](#호환성-매핑)**
- **[환경설정 호환성](#환경설정-호환성)**
- **[자동 재시작](#자동-재시작)**

* * *

## 소개

ASF 환경설정은 두개의 주요 부분으로 나누어집니다. 일반(프로세스) 환경설정과 모든 봇의 환경설정입니다. 모든 봇은 각각 `봇이름.json`이라는 이름의 봇 환경설정 파일을 가집니다. `봇이름`은 봇의 이름입니다. ASF 일반(프로세스) 설정은 `ASF.json`이라는 단일 파일입니다.

봇은 ASF 프로세스에 참여하는 단일 스팀 계정입니다. 정상적으로 작동하기 위해서 ASF는 최소한 **하나** 이상의 정의된 봇 인스턴스가 필요합니다. 프로세스가 강제하는 봇 인스턴스의 개수제한은 없습니다. 원하는 만큼 봇(스팀 계정)을 사용할 수 있습니다.

ASF는 환경설정 파일을 저장하기 위하여 **[JSON](https://ko.wikipedia.org/wiki/JSON)** 형식을 사용합니다. 사람에게 친숙하고, 읽을 수 있으며 프로그램을 설정할 수 있는 가장 보편적인 형식입니다. 걱정하지 마십시오. ASF를 설정하기 위해 JSON을 알 필요는 없습니다. 어떤 종류의 bash 스크립트로 ASF 환경설정을 대량생성하길 원하는 경우가 있어서 언급했을 뿐입니다.

환경설정은 정확한 JSON 환경설정을 작성하여 수동으로 혹은 훨씬 쉽고 편리한 **[웹 기반 환경설정 생성기(ConfigGenerator)](https://justarchinet.github.io/ASF-WebConfigGenerator)**를 이용해서 가능합니다. 고급 사용자가 아니라면 아래에서 설명할 환경설정 생성기를 사용하는 것을 추천합니다.

**[위로 돌아가기](#환경설정)**

* * *

## 웹 기반 환경설정 생성기(ConfigGenerator)

웹 기반 환경설정 생성기(ConfigGenerator)의 목적은 ASF 환경설정 파일을 생성하는데 사용하는 친숙한 화면을 제공하기 위해서입니다. 웹 기반 환경설정 생성기는 100% 클라이언트 기반입니다. 즉 입력한 세부내용은 다른 어디로도 보내지지 않고 로컬에서만 처리된다는 뜻입니다. 이렇게 해서 보안성과 신뢰성을 보장할 수 있습니다. 모든 파일을 다운로드 받고 `index.html` 파일을 당신의 웹 브라우저에서 실행하면 **[오프라인](https://github.com/JustArchiNET/ASF-WebConfigGenerator/tree/master/docs)**으로 작동할 수도 있습니다.

웹 기반 환경설정 생성기는 크롬과 파이어폭스에서 정상작동됨을 검증하였습니다. 또한 자바스크립트가 가능한 대부분의 유명한 웹 브라우저에서도 정상적으로 작동할 것입니다.

사용법은 매우 간단합니다. 적절한 탭을 선택해서 생성을 원하는 `ASF` 또는 `봇` 환경설정을 선택합니다. ASF 버전과 환경설정 파일의 버전이 맞는지 다시한번 확인 후, 모든 세부내용을 입력하고 "다운로드" 버튼을 누릅니다. 이 파일을 ASF의 `config` 디렉토리로 옮깁니다. 필요하다면 기존의 파일에 덮어쓰기 합니다. 매 최종수정마다 이를 반복합니다. 환경설정에서 가능한 옵션에 대한 설명은 이 섹션의 나머지 부분을 참고하십시오.

**[위로 돌아가기](#환경설정)**

* * *

## 수동 환경설정

웹기반 환경설정 생성기를 사용하는 것을 매우 권장합니다만, 모종의 이유로 사용을 원하지 않는 경우에는 직접 정확한 환경설정파일을 만들 수도 있습니다. 적절한 구조에서 잘 시작하기 위해서 `example.json` 파일을 참고하십시오. 새로 설정할 봇을 위해 이를 복사해서 사용할 수도 있습니다. 우리가 만든 화면을 사용하지 않기 때문에, 환경설정 내용이 **[유효한지](https://jsonlint.com)** 다시 한번 확인해야 합니다. ASF는 구문 분석이 안되면 실행이 안됩니다. 모든 가능한 항목의 적절한 JSON 구조는 아래의 **[JSON 매핑](#json-매핑)** 항목과 설명을 참고하십시오.

**[위로 돌아가기](#환경설정)**

* * *

## 일반 환경설정

일반 환경설정은 `ASF.json` 파일에 들어있으며 다음과 같은 구조를 가집니다.

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

**팁:** 저 옵션을 바꾸길 원하지 않는 한, 모든 것을 기본값으로 놔두어도 괜찮습니다. 그러면 이제 `ASF.json` 파일을 닫고 봇 환경설정으로 넘어갈 수 있습니다.

* * *

모든 옵션은 다음과 같습니다.

### `AutoRestart`

`bool` type with default value of `true`. 이 속성값은 필요할때 ASF가 자동으로 재시작할지를 정의합니다. `UpdatePeriod` 혹은 `update` 명령으로 수행되는 ASF업데이트나, `ASF.json` 환경설정 변경, `restart` 명령 등 ASF가 재시작을 필요로 하는 몇가지 이벤트가 있습니다. 일반적으로, 재시작은 두 부분으로 이루어져 있습니다. 새로운 프로세스의 생성과 현재 프로세스의 종료입니다. Most users should be fine with it and keep this property with default value of `true`, however - if you're running ASF through your own script and/or with `dotnet`, you might want to have full control over starting the process, and avoid a situation such as having new (restarted) ASF process running somewhere silently in the background, and not in the foreground of the script, that exited together with old ASF process. This is especially important considering the fact that new process will no longer be your direct child, which would make you unable e.g. to use standard console input for it.

If that's the case, this property if specially for you and you can set it to `false`. However, keep in mind that in such case **you** are responsible for restarting the process. This is somehow important as ASF will only exit instead of spawning new process (e.g. after update), so if there is no logic added by you, it'll simply stop working until you start it again. ASF always exits with proper error code indicating success (zero) or non-success (non-zero), this way you're able to add proper logic in your script which should avoid auto-restarting ASF in case of failure, or at least make a local copy of `log.txt` for further analysis. Also keep in mind that `restart` command will always restart ASF regardless of how this property is set, as this property defines default behaviour, while `restart` command always restarts the process. Unless you have a reason to disable this feature, you should keep it enabled.

* * *

### `Blacklist`

`ImmutableHashSet<uint>` type with default value of being empty. As the name suggests, this global config property defines appIDs (games) that will be entirely ignored by automatic ASF idling process. Unfortunately Steam loves to flag summer/winter sale badges as "available for cards drop", which confuses ASF process by making it believe that it's a valid game that should be farmed. If there was no any kind of blacklist, ASF would eventually "hang" at farming a game which is in fact not a game, and wait infinitely for cards drop that will not happen. ASF blacklist serves a purpose of marking those badges as not available for farming, so we can silently ignore them when deciding what to farm, and not fall into the trap.

ASF includes two blacklists by default - `GlobalBlacklist`, which is hardcoded into the ASF code and not possible to edit, and normal `Blacklist`, which is defined here. `GlobalBlacklist` is updated together with ASF version and typically includes all "bad" appIDs at the time of release, so if you're using up-to-date ASF then you do not need to maintain your own `Blacklist` defined here. The main purpose of this property is to allow you blacklisting new, not-known at the time of ASF release appIDs, which should not be farmed. Hardcoded `GlobalBlacklist` is being updated as fast as possible, therefore you're not required to update your own `Blacklist` if you're using latest ASF version, but without `Blacklist` you'd be forced to update ASF in order to "keep running" when Valve releases new sale badge - I don't want to force you to use latest ASF code, therefore this property is here to allow you "fixing" ASF yourself if you for some reason don't want to, or can't, update to new hardcoded `GlobalBlacklist` in new ASF release, yet you want to keep your old ASF running. Unless you have a **strong** reason to edit this property, you should keep it at default.

If you're looking for bot-based blacklist instead, take a look at `ib`, `ibadd` and `ibrm` **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**.

* * *

### `CommandPrefix`

`string` type with default value of `!`. This property specifies **case-sensitive** prefix used for ASF **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. In other words, this is what you need to prepend to each ASF command in order to make ASF listen to you. It's possible to set this value to `null` or empty in order to make ASF use no command prefix, in which case you input commands with their plain identifiers. However, doing so will potentially decrease ASF's performance as ASF is optimized to not parse message further if it doesn't start with `CommandPrefix` - if you intentionally decide to not use it, ASF will be forced to read all messages and respond to them, even if they're not ASF commands. Therefore it's recommended to keep using some `CommandPrefix`, such as `/` if you don't like default value of `!`. For consistency, `CommandPrefix` affects the entire ASF process. Unless you have a reason to edit this property, you should keep it at default.

* * *

### `ConfirmationsLimiterDelay`

`byte` type with default value of `10`. ASF will ensure that there will be at least `ConfirmationsLimiterDelay` seconds in between of two consecutive 2FA confirmations fetching requests to avoid triggering rate-limit - those are being used by **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** during e.g. `2faok` command, as well as on as-needed basis when performing various trading-related operations. Default value was set based on our tests and should not be lowered if you don't want to run into issues. Unless you have a **strong** reason to edit this property, you should keep it at default.

* * *

### `ConnectionTimeout`

`byte` type with default value of `60`. This property defines timeouts for various network actions done by ASF, in seconds. In particular, `ConnectionTimeout` defines timeout in seconds for HTTP and IPC requests, `ConnectionTimeout / 10` defines maximum number of failed heartbeats, while `ConnectionTimeout / 30` defines number of minutes we allow for initial Steam network connection request. Default value of `60` should be fine for majority of people, however, if you have rather slow network connection or PC, you might want to increase this number to something higher (like `90`). Keep in mind that bigger values will not magically fix slow or even inaccessible Steam servers, so we shouldn't infinitely wait for something that won't happen and simply try again later. Setting this value too high will result in excessive delay in catching network issues, as well as in decrease of overall performance. Setting this value too low will decrease overall stability and performance as well, as ASF will abort valid request still being parsed. Therefore setting this value lower than default has no advantage in general, as Steam servers tend to be slow from time to time, and might require more time for parsing ASF requests. Default value is a balance between believing that our network connection is stable, and doubting in Steam network to handle our request in given timeout. If you want to detect issues sooner and make ASF reconnect/respond faster, default value should do (or very slightly below, making ASF less patient). If you instead notice that ASF is running into network issues, such as failing requests, heartbeats being lost or connection to Steam interrupted, it might make sense to increase this value if you're sure that it's **not** caused by your network, but by Steam itself, as increasing timeouts makes ASF more "patient" and not deciding to reconnect right away. It might also make sense to increase this value if you have rather slow internet that requires more time to process the data being transmitted. In short, default value should be decent for most cases, but you might want to increase it if needed. Still, going far above the default value doesn't make much sense either, since bigger timeouts won't magically fix inaccessible Steam servers. Unless you have a reason to edit this property, you should keep it at default.

* * *

### `CurrentCulture`

`string` type with default value of `null`. By default ASF attempts to use your operating system language, and will prefer to use translated strings in that language if available. This is possible thanks to our community that tries to **[localize](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Localization)** ASF in all most popular languages. If for some reason you don't want to use your OS native language, you can use this config property to pick any valid language you'd want to use instead. For a list of all available cultures, please visit **[MSDN](https://msdn.microsoft.com/en-us/library/cc233982.aspx)** and look for `Language tag`. It's nice to note that ASF accepts both specific cultures, such as `en-GB`, but also neutral ones, such as `en`. Specifying current culture might also affect other culture-specific behaviour, such as currency/date format and alike. Please note that you might need additional font/language packs for displaying language-specific characters properly, if you picked non-native culture that makes use of them. Typically you want to make use of this config property if you prefer ASF in English instead of your native language.

* * *

### `Debug`

`bool` type with default value of `false`. This property defines if process should run in debug mode. When in debug mode, ASF creates a special `debug` directory next to the `config`, which keeps track of whole communication between ASF and Steam servers. Debug information can help spotting nasty issues related to networking and general ASF workflow. In addition to that, some program routines will be far more verbose, such as `WebBrowser` stating exact reason why some requests are failing - those entries are written to normal ASF log. **You should not run ASF in Debug mode, unless asked by developer**. Running ASF in debug mode **decreases performance**, **affects stability negatively** and is **far more verbose in various places**, so it should be used **only** intentionally, in short-run, for debugging particular issue, reproducing the problem or getting more info about a failing request, and alike, but **not** for normal program execution. You will see **a lot** of new errors, issues, and exceptions - make sure that you have a decent knowledge about ASF, Steam and its quirks if you decide to analyze all of that yourself, as not everything is relevant.

**WARNING:** enabling this mode includes logging of **potentially sensitive** information such as logins and passwords that you're using for logging in to Steam (due to network logging). That data is written to both `debug` directory, as well as standard `log.txt` (that is now intentionally much more verbose to log this info). You should not post debug content generated by ASF in any public location, ASF developer should always remind you of sending it to his e-mail, or other secure location. We're not storing, neither making use of those sensitive details, they're written as part of debug routines since their presence might be relevant to the issue that is affecting you. We'd prefer if you didn't alter ASF logging in any way, but if you're worried, you're free to redact those sensitive details appropriately.

> Redacting involves replacing sensitive details, for example with stars. You should refrain from removing sensitive lines entirely, as their pure existence might be relevant and should be preserved.

* * *

### `FarmingDelay`

`byte` type with default value of `15`. In order for ASF to work, it will check currently farmed game every `FarmingDelay` minutes, if it perhaps dropped all cards already. Setting this property too low can result in excessive amount of steam requests being sent, while setting it too high can result in ASF still "farming" given title for up to `FarmingDelay` minutes after it's fully farmed. Default value should be excellent for most users, but if you have many bots running, you might consider increasing it to something like `30` minutes in order to limit steam requests being sent. It's nice to note that ASF uses event-based mechanism and checks game badge page on each Steam item dropped, so in general **we don't even need to check it in fixed time intervals**, but as we don't fully trust Steam network - we check game badge page anyway, if we didn't check it through card being dropped event in last `FarmingDelay` minutes (in case Steam network didn't inform us about item dropped or stuff like that). Assuming that Steam network is working properly, decreasing this value **will not improve farming efficiency in any way**, while **increasing network overhead significantly** - it's recommended only to increase it (if needed) from default of `15` minutes. Unless you have a **strong** reason to edit this property, you should keep it at default.

* * *

### `GiftsLimiterDelay`

`byte` type with default value of `1`. ASF will ensure that there will be at least `GiftsLimiterDelay` seconds in between of two consecutive gift/key/license handling (redeeming) requests to avoid triggering rate-limit. In addition to that it'll also be used as global limiter for game list requests, such as the one issued by `owns` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. Unless you have a **strong** reason to edit this property, you should keep it at default.

* * *

### `Headless`

`bool` type with default value of `false`. This property defines if process should run in headless mode. When in headless mode, ASF assumes that it's running on a server or in other non-interactive environment, therefore it will not attempt to read crucial account credentials such as 2FA code, SteamGuard code, password or any other variable required for ASF to operate. This is equal to making ASF console read-only. `Headless` mode is useful mainly for users running ASF on their servers, as instead of asking e.g. for 2FA code, ASF will silently abort the operation by stopping an account. Unless you're running ASF on a server, and you previously confirmed that ASF is able to operate in non-headless mode, you should keep this property disabled. Any user interaction will be denied when in headless mode, and your accounts will not run if they require **any** console input during starting. This is useful for servers, as ASF can abort trying to log onto the account when asked for credentials, instead of waiting (infinitely) for user to provide those. Enabling this mode will also allow you to use `input` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** which acts as a replacement for standard console input. If you're not sure how to set this property, leave it with default value of `false`.

If you're running ASF on the server, you might want to use this option together with `--process-required` **[command-line argument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments)**.

* * *

### `IdleFarmingPeriod`

`byte` type with default value of `8`. When ASF has nothing to farm, it will periodically check every `IdleFarmingPeriod` hours if perhaps account got some new games to farm. This feature is not needed when talking about new games we're getting, as ASF is smart enough to automatically check badge pages in this case. `IdleFarmingPeriod` is mainly for situations such as old games we already have having trading cards added. In this case there is no event, so ASF has to periodically check badge pages if we want to have this covered. Value of `0` disables this feature. Also check: `ShutdownOnFarmingFinished`.

* * *

### `InventoryLimiterDelay`

`byte` type with default value of `3`. ASF will ensure that there will be at least `InventoryLimiterDelay` seconds in between of two consecutive inventory requests to avoid triggering rate-limit - those are being used for fetching your own inventory (and only for that). Default value of `3` was set based on looting over 100 bot instances, and should satisfy most (if not all) of the users. You may however want to decrease it, or even change to `0` if you have very low amount of bots, so ASF will ignore the delay and loot steam inventories much faster. Be warned though, as setting it too low **will** result in Steam temporarily banning your IP, and that will prevent you from fetching your inventory at all. You also might need to increase this value if you're running a lot of bots with a lot of inventory requests, although in this case you should probably try to limit number of those requests instead. Unless you have a **strong** reason to edit this property, you should keep it at default.

* * *

### `IPC`

`bool` type with default value of `false`. This property defines if ASF's **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** server should start together with the process. IPC allows for inter-process communication by booting a local HTTP server. If you're not going to make use of ASF's IPC server, then there is no reason for you to enable this option.

* * *

### `IPCPassword`

`string` type with default value of `null`. This property defines mandatory password for every call done via IPC and serves as an extra security measure. When set to non-empty value, all IPC requests will require extra `password` property set to the password specified here. Default value of `null` will skip a need of the password, making ASF respect all queries. In addition to that, enabling this option also enables built-in IPC anti-bruteforce mechanism which will temporarily ban given `IPAddress` after sending too many unauthorized requests in a very short time. Unless you have a reason to edit this property, you should keep it at default.

* * *

### `LoginLimiterDelay`

`byte` type with default value of `10`. ASF will ensure that there will be at least `LoginLimiterDelay` seconds in between of two consecutive connection attempts to avoid triggering rate-limit. Default value of `10` was set based on connecting over 100 bot instances, and should satisfy most (if not all) of the users. You may however want to increase/decrease it, or even change to `0` if you have very low amount of bots, so ASF will ignore the delay and connect to Steam much faster. Be warned though, as setting it too low while having too many bots **will** result in Steam temporarily banning your IP, and that will prevent you from logging in **at all**, with `InvalidPassword/RateLimitExceeded` error - and that also includes your normal Steam client, not only ASF. Likewise, if you're running excessive number of bots, especially together with other Steam clients/tools using the same IP address, most likely you'll need to increase this value in order to spread logins across longer period of time.

As a side note, this value is also used as load-balancing buffer in all ASF-scheduled actions, such as trades in `SendTradePeriod`. Unless you have a **strong** reason to edit this property, you should keep it at default.

* * *

### `MaxFarmingTime`

`byte` type with default value of `10`. As you should know, Steam is not always working properly, sometimes weird situations can happen such as steam not being recording our playtime, despite of in fact playing a game. ASF will allow farming a single game in solo mode for maximum of `MaxFarmingTime` hours, and consider it fully farmed after that period. This is required to not freeze farming process in case of weird situations happening, but also if for some reason Steam released a new badge that would stop ASF from progressing further (see: `Blacklist`). Default value of `10` hours should be enough for dropping all steam cards from one game. Setting this property too low can result in valid games being skipped (and yes, there are valid games taking even up to 9 hours to farm), while setting it too high can result in farming process being frozen. Please note that this property affects only a single game in a single farming session (so after going through entire queue ASF will return to that title), also it's not based on total playtime but internal ASF farming time. Unless you have a **strong** reason to edit this property, you should keep it at default.

* * *

### `MaxTradeHoldDuration`

`byte` type with default value of `15`. This property defines maximum duration of trade hold in days that we're willing to accept - ASF will reject trades that are being held for more than `MaxTradeHoldDuration` days. This option makes sense only for bots with `TradingPreferences` of `SteamTradeMatcher`, as it doesn't affect `Master`/`SteamOwnerID` trades, neither donations. Trade holds are annoying for everyone, and nobody really wants to deal with them. ASF is supposed to work on liberal rules and help everyone, regardless if on trade hold or not - that's why this option is set to `15` by default. However, if you'd instead prefer to reject all trades affected by trade holds, you can specify `0` here. Please consider the fact that cards with short lifespan are not affected by this option and automatically rejected for people with trade holds, as described in **[trading](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading)** section, so there is no need to globally reject everybody only because of that. Unless you have a reason to edit this property, you should keep it at default.

* * *

### `OptimizationMode`

`byte` type with default value of `0`. This property defines optimization mode which ASF will prefer during runtime. Currently ASF supports two modes - `0` which is called `MaxPerformance`, and `1` which is called `MinMemoryUsage`. By default ASF prefers to run as many things in parallel (concurrently) as possible, which enhances performance by load-balancing work across all CPU cores, multiple CPU threads, multiple sockets and multiple threadpool tasks. For example, ASF will ask for your first badge page when checking for games to idle, and then once request arrived, ASF will read from it how many badge pages you actually have, then request each other one concurrently. This is what you should want **almost always**, as the overhead in most cases is minimal and benefits from asynchronous ASF code can be seen even on the oldest hardware with a single CPU core and heavily limited power. However, with many tasks being processed in parallel, ASF runtime is responsible for their maintenance, e.g. keeping sockets open, threads alive and tasks being processed, which can result in increased memory usage from time to time, and if you're extremely constrained by available memory, you might want to switch this property to `1` (`MinMemoryUsage`) in order to force ASF into using as little tasks as possible, and typically running possible-to-parallel asynchronous code in a synchronous manner. You should consider switching this property only if you previously read **[low-memory setup](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup)** and you intentionally want to sacrifice gigantic performance boost, for a very small memory overhead decrease. Usually this option is **much worse** than what you can achieve with other possible ways, such as by limiting your ASF usage or tuning runtime's garbage collector, as explained in **[low-memory setup](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup)**. Therefore, you should use `MinMemoryUsage` as a **last resort**, right before runtime recompilation, if you couldn't achieve satisfying results with other (much better) options. Unless you have a **strong** reason to edit this property, you should keep it at default.

* * *

### `통계`

`bool` type with default value of `true`. This property defines if ASF should have statistics enabled. Detailed explanation what exactly this option does is available in **[statistics](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Statistics)** section. Unless you have a reason to edit this property, you should keep it at default.

* * *

### `SteamMessagePrefix`

`string` type with default value of `"/me "`. This property defines a prefix that will be prepended to all Steam messages being sent by ASF. By default ASF uses `"/me "` prefix in order to distinguish bot messages more easily by showing them in different color on Steam chat. Another worthy mention is `"/pre "` prefix which achieves similar result, but uses different formatting. You can also set this property to empty string or `null` in order to disable using prefix entirely and output all ASF messages in a traditional way. It's nice to note that this property affects Steam messages only - responses returned through other channels (such as IPC) are not affected. Unless you want to customize standard ASF behaviour, it's a good idea to leave it at default.

* * *

### `SteamOwnerID`

`ulong` type with default value of `0`. This property defines Steam ID in 64-bit form of ASF process owner, and is very similar to `Master` permission of given bot instance (but global instead). You almost always want to set this property to ID of your own main Steam account. `Master` permission includes full control over his bot instance, but global commands such as `exit`, `restart` or `update` are reserved for `SteamOwnerID` only. This is convenient, as you might want to run bots for your friends, while not allowing them to control ASF process, such as exiting it via `exit` command. Default value of `0` specifies that there is no owner of ASF process, which means that nobody will be able to issue global ASF commands. Keep in mind that **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** works with `SteamOwnerID`, so if you want to use it you must provide a valid value here.

* * *

### `SteamProtocols`

`byte flags` type with default value of `7`. This property defines Steam protocols that ASF will use when connecting to Steam servers, which are defined as below:

| 값 | 이름        | 설명                                                                                               |
| - | --------- | ------------------------------------------------------------------------------------------------ |
| 0 | None      | No protocol                                                                                      |
| 1 | TCP       | **[Transmission Control Protocol](https://ko.wikipedia.org/wiki/Transmission_Control_Protocol)** |
| 2 | UDP       | **[User Datagram Protocol](https://ko.wikipedia.org/wiki/User_Datagram_Protocol)**               |
| 4 | WebSocket | **[WebSocket](https://ko.wikipedia.org/wiki/WebSocket)**                                         |

Please notice that this property is `flags` field, therefore it's possible to choose any combination of available values. Check out **[flags mapping](#json-mapping)** if you'd like to learn more. Not enabling any of flags results in `None` option, and that option is invalid by itself.

By default ASF should use all available Steam protocols as a measure for fighting with downtimes and other similar Steam issues. Typically you want to change this property if you want to limit ASF into using only one or two specific protocols instead of all available ones. Such measure could be needed if you're e.g. enabling only TCP traffic on your firewall and you do not want ASF to try connecting via UDP. However, unless you're debugging particular problem or issue, you almost always want to ensure that ASF is free to use any protocol that is currently supported and not just one or two. Unless you have a **strong** reason to edit this property, you should keep it at default.

* * *

### `UpdateChannel`

`byte` type with default value of `1`. This property defines update channel which is being used, either for auto-updates (if `UpdatePeriod` is greater than `0`), or update notifications (otherwise). Currently ASF supports three update channels - `0` which is called `None`, `1`, which is called `Stable`, and `2`, which is called `Experimental`. `Stable` channel is the default release channel, which should be used by majority of users. `Experimental` channel in addition to `Stable` releases, also includes **pre-releases** dedicated for advanced users and other developers in order to test new features, confirm bugfixes or give feedback about planned enhancements. **Experimental versions often contain unpatched bugs, work-in-progress features or rewritten implementations**. If you don't consider yourself advanced user, please stay with default `1` (Stable) update channel. `Experimental` channel is dedicated to users who know how to report bugs, deal with issues and give feedback - no technical support will be given. Check out ASF **[release cycle](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle)** if you'd like to learn more. You can also set `UpdateChannel` to `0` (`None`), if you want to completely remove all version checks. Setting `UpdateChannel` to `0` will entirely disable entire functionality related to updates, including `update` command. Using `None` channel is **strongly discouraged** due to exposing yourself to all sort of problems (mentioned in `UpdatePeriod` description below).

**Unless you know what you're doing**, we **strongly** recommend to keep it at default.

* * *

### `UpdatePeriod`

`byte` type with default value of `24`. This property defines how often ASF should check for auto-updates. Updates are crucial not only to receive new features and critical security patches, but also to receive bugfixes, performance enhancements, stability improvements and more. When a value greater than `0` is set, ASF will automatically download, replace, and restart itself (if `AutoRestart` permits) when new update is available. In order to achieve this, ASF will check every `UpdatePeriod` hours if new update is available on our GitHub repo. A value of `0` disables auto-updates, but still allows you to execute `update` command manually. You might also be interested in setting appropriate `UpdateChannel` that `UpdatePeriod` should follow.

Update process of ASF involves update of entire folder structure that ASF is using, but without touching your own configs or databases located in `config` directory - this means that any extra files unrelated to ASF in its directory might be lost during update. Default value of `24` is a good balance between unnecessary checks, and ASF that is fresh enough.

Unless you have a **strong** reason to disable this feature, you should keep auto-updates enabled within reasonable `UpdatePeriod` **for your own good**. This is not only because we don't support anything but latest stable ASF release, but also because **we give our security guarantee only for latest version**. If you're using outdated ASF version then you're intentionally exposing yourself to all kind of issues, from small bugs, through broken functionality, ending with **[permanent Steam account suspensions](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ#did-somebody-get-banned-for-it)**, so we **strongly recommend**, for your own good, to always ensure that your ASF version is up to date. Auto-updates allow us to react quickly to all kind of issues by disabling or patching problematic code before it can escalate - if you opt out of it, you lose all of our security guarantees and risk consequences from running code that could be potentially harmful, not only to Steam network, but also (by definition) to your own Steam account.

* * *

### `WebLimiterDelay`

`ushort` type with default value of `200`. This property defines, in miliseconds, the minimum amount of delay between sending two consecutive requests to the same Steam web-service. Such delay is required as **[AkamaiGhost](https://www.akamai.com)** service that Steam uses internally includes rate-limiting based on global number of requests sent across given time period. In normal circumstances akamai block is rather hard to achieve, but under very heavy workloads with a huge ongoing queue of requests, it's possible to trigger it if we keep sending too many requests across too short time period.

Default value was set based on assumption that ASF is the only tool accessing Steam web-services, in particular `steamcommunity.com`, `api.steampowered.com` and `store.steampowered.com`. If you're using other tools sending requests to the same web-services then you should make sure that your tool includes similar functionality of `WebLimiterDelay` and set both to double of default value, which would be `400`. This guarantees that under no circumstance you'll exceed sending more than 1 request per 200 ms.

In general, lowering `WebLimiterDelay` under default value is **strongly discouraged** as it might lead to various IP-related blocks, some of which are possible to be permanent. Default value is good enough for running a single ASF instance on the server, as well as using ASF in normal scenario along with original Steam client. It should be correct for majority of usages, and you should only increase it (never lower it), if - apart from using ASF, you're also using another tool that might send excessive number of requests to the same web-services that ASF is making use of. In short, global number of all requests sent from a single IP to a single Steam domain should never exceed more than 1 request per 200 ms.

Unless you have a reason to edit this property, you should keep it at default.

* * *

### `WebProxy`

`string` type with default value of `null`. This property defines a web proxy address that will be used for all internal http and https requests sent by ASF's `HttpClient`, especially to services such as `github.com`, `steamcommunity.com` and `store.steampowered.com`. Proxying ASF requests in general has no advantages, but it's exceptionally useful for bypassing various kinds of firewalls, especially the great firewall in China.

This property is defined as uri string:

> A URI string is composed of a scheme (http or https), a host, and an optional port. An example of a complete uri string is `"http://contoso.com:8080"`.

If your proxy requires user authentication, you will also need to set up `WebProxyUsername` and/or `WebProxyPassword`. If there is no such need, setting up this property alone is sufficient.

Right now ASF uses web proxy only for `http` and `https` requests, which **do not** include internal Steam network communication done within ASF's internal Steam client. There are currently no plans for supporting that, mainly due to missing **[SK2](https://github.com/SteamRE/SteamKit)** functionality. If you need/want it to happen, I'd suggest starting from there.

Unless you have a reason to edit this property, you should keep it at default.

* * *

### `WebProxyPassword`

`string` type with default value of `null`. This property defines password field used in basic, digest, NTLM, and Kerberos authentication that is supported by a target `WebProxy` machine providing proxy functionality. If your proxy doesn't require user credentials, there is no need for you to input anything here. Using this option makes sense only if `WebProxy` is used as well, as it has no effect otherwise.

Unless you have a reason to edit this property, you should keep it at default.

* * *

### `WebProxyUsername`

`string` type with default value of `null`. This property defines username field used in basic, digest, NTLM, and Kerberos authentication that is supported by a target `WebProxy` machine providing proxy functionality. If your proxy doesn't require user credentials, there is no need for you to input anything here. Using this option makes sense only if `WebProxy` is used as well, as it has no effect otherwise.

Unless you have a reason to edit this property, you should keep it at default.

* * *

**[위로 돌아가기](#환경설정)**

* * *

## 봇 환경설정

As you should know already, every bot should have its own config. Example bot config is included in `example.json` file, which should be used for bot configuration. Simply **copy paste** `example.json` to a new file, and remember to name it appropriately, as it will be your bot instance. You should start from configuring your **primary** account, so some good suggestions for filename is `primary.json`, `1.json` or `YourNickname.json`.

**Notice:** There are several names which are reserved and can't be used for bot configs. Those are: **ASF**, **example** and **minimal**. ASF will ignore such configuration files. ASF will also ignore configuration files starting with a dot.

After deciding how you want to name your bot, open its file, and start with configuration. You should notice following structure:

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

모든 옵션은 다음과 같습니다.

### `AcceptGifts`

`bool` type with default value of `false`. When enabled, ASF will automatically accept and redeem all steam gifts (including wallet gift cards) sent to the bot. This includes also gifts sent from users other than those defined in `SteamUserPermissions`. Keep in mind that gifts sent to e-mail address are not directly forwarded to the client, so ASF won't accept those without your help.

This option is recommended only for alt accounts, as it's very likely that you don't want to automatically redeem all gifts sent to your primary account. If you're unsure whether you want this feature enabled or not, keep it with default value of `false`.

* * *

### `AutoSteamSaleEvent`

`bool` type with default value of `false`. During Steam summer/winter sale events Steam is known for providing you extra cards for browsing discovery queue each day, as well as voting in the Steam awards. When this option is enabled, ASF will automatically check Steam discovery queue and Steam awards each 6 hours, and clear them if needed. This option is not recommended if you want to do those actions yourself, and typically it should make sense only on bot accounts. Moreover, you need to ensure that your account is at least of level `8` if you expect to receive those cards in the first place. If you're unsure whether you want this feature enabled or not, keep it with default value of `false`.

Please note that due to constant Valve issues, changes and problems, **we give no guarantee whether this function will work properly**, therefore it's entirely possible that this option **will not work at all**. We do not accept **any** bug reports, neither support requests for this option. It's offered with absolutely no guarantees, you're using it at your own risk.

* * *

### `BotBehaviour`

`byte flags` type with default value of `0`. This property defines ASF bot-like behaviour during various events, and is defined as below:

| 값  | 이름                            | 설명                                                                    |
| -- | ----------------------------- | --------------------------------------------------------------------- |
| 0  | None                          | No special bot behaviour, the least invasive mode, default            |
| 1  | RejectInvalidFriendInvites    | Will cause ASF to reject (instead of ignoring) invalid friend invites |
| 2  | RejectInvalidTrades           | Will cause ASF to reject (instead of ignoring) invalid trade offers   |
| 4  | RejectInvalidGroupInvites     | Will cause ASF to reject (instead of ignoring) invalid group invites  |
| 8  | DismissInventoryNotifications | Will cause ASF to automatically dismiss all inventory notifications   |
| 16 | MarkReceivedMessagesAsRead    | Will cause ASF to automatically mark all received messages as read    |

Please notice that this property is `flags` field, therefore it's possible to choose any combination of available values. Check out **[flags mapping](#json-mapping)** if you'd like to learn more. Not enabling any of flags results in `None` option.

In general you want to modify this property if you expect from ASF to do certain amount of automation related to its activity, as it'd be expected from a bot account, but not a primary account used in ASF. Therefore, changing this property makes sense mainly for alt accounts, although you're free to use selected options for main accounts as well.

Normal (`None`) ASF behaviour is to only automate things that user wants (e.g. cards farming or `SteamTradeMatcher` offers, if set in `TradingPreferences`). This is the least invasive mode, and it's beneficial to majority of users since you remain in full control over your account and you can decide yourself whether to allow certain out-of-scope interactions, or not.

Invalid friend invite is an invite that doesn't come from user with `FamilySharing` permission (defined in `SteamUserPermissions`) or above. ASF in normal mode ignores those invites, as you'd expect, giving you free choice whether to accept them, or not. `RejectInvalidFriendInvites` will cause those invites to be automatically rejected, which will practically disable option for other people to add you to their friend list (as ASF will deny all such requests, apart from people defined in `SteamUserPermissions`). Unless you want to outright deny all friend invites, you shouldn't enable this option.

Invalid trade offer is an offer that isn't accepted through built-in ASF module. More on this matter can be found in **[trading](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading)** section which explicitly defines what types of trade ASF is willing to accept automatically. Valid trades are also defined by other settings, especially `TradingPreferences`. `RejectInvalidTrades` will cause all invalid trade offers to be rejected, instead of being ignored. Unless you want to outright deny all trade offers that aren't automatically accepted by ASF, you shouldn't enable this option.

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

`bool` type with default value of `false`. This property defines if bot is enabled. Enabled bot instance (`true`) will automatically start on ASF run, while disabled bot instance (`false`) will need to be started manually. By default every bot is disabled, so you probably want to switch this property to `true` for all of your bots that should be started automatically.

* * *

### `FarmingOrders`

`ImmutableHashSet<byte>` type with default value of being empty. This property defines the **preferred** farming order used by ASF for given bot account. Currently there are following farming orders available:

| 값  | 이름                        | 설명                                                                               |
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

Also notice the word "try" in all above descriptions - the actual ASF order is heavily affected by selected **[cards farming algorithm](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)** and sorting will affect only results that ASF considers same performance-wise. For example, in `Simple` algorithm, selected `FarmingOrders` should be entirely respected in current farming session (as every game has the same performance value), while in `Complex` algorithm actual order is affected by hours first, and then sorted according to chosen `FarmingOrders`. This will lead to different results, as games with existing playtime will have a priority over others, so effectively ASF will prefer games with highest playtime first, and only then sorting everything further by your chosen `FarmingOrders`. Therefore, this config property is only a **suggestion** that ASF will try to respect, as long as it doesn't affect performance negatively (in this case, ASF will always prefer performance over `FarmingOrders`).

There is also idling priority queue that is accessible through `iq` **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. If it's used, actual idling order is sorted firstly by performance, then by idling queue, and finally by your `FarmingOrders`.

* * *

### `GamesPlayedWhileIdle`

`ImmutableHashSet<uint>` type with default value of being empty. If ASF has nothing to farm it can play your specified steam games (`appID`s) instead. Playing games in such manner increases your "hours played" of those games, but nothing else apart of it. This feature can be enabled at the same time with `CustomGamePlayedWhileIdle` in order to play your selected games while showing custom status in Steam network, but in this case, like in `CustomGamePlayedWhileFarming` case, the actual display order is not guaranteed. Please note that Steam allows ASF to play only up to `32` `appID`s, therefore if you put more games than that, only first `32` will be respected (and extra ones being ignored).

* * *

### `HoursUntilCardDrops`

`byte` type with default value of `3`. This property defines if account has card drops restricted, and if yes, for how many initial hours. Restricted card drops means that account is not receiving any card drops from given game until the game is played for at least `HoursUntilCardDrops` hours. Unfortunately there is no magical way to detect that, so ASF relies on you. This property affects **[cards farming algorithm](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)** that will be used. Setting this property properly will maximize profits and minimize time required for cards to be farmed. Remember that there is no obvious answer whether you should use one or another value, since it fully depends on your account. It seems that older accounts which never asked for refund have unrestricted card drops, so they should use a value of `0`, while new accounts and those who did ask for refund have restricted card drops with a value of `3`. This is however only theory, and should not be taken as a rule. The default value for this property was set based on "lesser evil" and majority of use cases.

* * *

### `IdlePriorityQueueOnly`

`bool` type with default value of `false`. This property defines if ASF should consider for automatic idling only apps that you added yourself to priority idling queue available with `iq` **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. When this option is enabled, ASF will skip all `appIDs` that are missing on the list, effectively allowing you to cherry-pick games for automatic ASF idling. Keep in mind that if you didn't add any games to the queue then ASF will act as if there is nothing to idle on your account. If you're unsure whether you want this feature enabled or not, keep it with default value of `false`.

* * *

### `IdleRefundableGames`

`bool` type with default value of `true`. This property defines if ASF is permitted to idle games that are still refundable. A refundable game is a game that we bought in last 2 weeks through Steam Store and we didn't play it for longer than 2 hours yet, as stated **[here](https://store.steampowered.com/steam_refunds)**. By default when this option is set to `true`, ASF ignores Steam refunds entirely and idles everything, as most people expect. However, you can change this option to `false` if you want to ensure that ASF won't idle any of your refundable games too soon, allowing you to evaluate those games yourself and refund if needed without worrying about ASF affecting playtime negatively. Please note that if you disable this option then games you purchased from Steam Store won't be idled by ASF for up to 14 days since redeem date. If you're unsure whether you want this feature enabled or not, keep it with default value of `true`.

* * *

### `LootableTypes`

`ImmutableHashSet<byte>` type with default value of `1, 3, 5` steam item types. This property defines ASF behaviour when looting - both manual and automatic. ASF will ensure that only items from `LootableTypes` will be included in a trade offer, therefore this property allows you to choose what you want to receive in a trade offer that is being sent to you.

| 값 | 이름                | 설명                                                            |
| - | ----------------- | ------------------------------------------------------------- |
| 0 | Unknown           | Every type that doesn't fit in any of the below               |
| 1 | BoosterPack       | Unpacked booster pack                                         |
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

| 값 | 이름                | 설명                                                            |
| - | ----------------- | ------------------------------------------------------------- |
| 0 | Unknown           | Every type that doesn't fit in any of the below               |
| 1 | BoosterPack       | Unpacked booster pack                                         |
| 2 | Emoticon          | Emoticon to use in Steam Chat                                 |
| 3 | FoilTradingCard   | Foil variant of `TradingCard`                                 |
| 4 | ProfileBackground | Profile background to use on your Steam profile               |
| 5 | TradingCard       | Steam trading card, being used for crafting badges (non-foil) |
| 6 | SteamGems         | Steam gems being used for crafting boosters, sacks included   |

Of course, types that you should use for this property typically include only `2`, `3`, `4` and `5`, as only those types are supported by STM. Please note that **ASF is not a trading bot** and **will NOT care about price or rarity**, which means that if you use it e.g. with `Emoticon` type, then ASF will be happy to trade your 2x rare emoticon for 1x rare 1x common, as that makes progress towards badge (in this case emoticons) completion. Please evaluate twice if you're fine with that. Unless you know what you're doing, you should keep it with default value of `5`.

* * *

### `OnlineStatus`

`byte` type with default value of `1`. 이 속성값은 스팀 네트워크에 로그인 후 스팀 네트워크에 알려줄 활동 상태를 지정합니다. 현재 선택할 수 있는 활동 상태는 다음과 같습니다.

| 값 | 이름             |
| - | -------------- |
| 0 | 오프라인           |
| 1 | 온라인            |
| 2 | 다른 용무 중        |
| 3 | 자리 비움          |
| 4 | 수면 중           |
| 5 | LookingToTrade |
| 6 | LookingToPlay  |
| 7 | Invisible      |

`오프라인` 상태는 주 계정에 극도로 유용합니다. 알다시피 게임을 농사지으면 스팀 상태가 "XXX를 플레이중"으로 나타나며, 실제로는 농사를 짓고 있는데 플레이를 하는것으로 혼동울 주어 당신의 친구들이 오해할 수 있습니다. `오프라인` 상태를 사용하면 ASF로 카드 농사중일때 "게임중"으로 표시하지 않아 이런 문제를 해결해줍니다. 이것은 ASF가 정상작동하기 위해서 스팀 커뮤니티에 로그인할 필요가 없기 때문에 가능합니다. 우리는 사실 스팀 네트워크에 접속해서, 게임을 플레이중이지만, 우리의 존재를 전혀 알리지 않습니다. 오프라인 상태에서 플레이했던 게임도 플레이 시간에 포함되며, 프로필의 "최근에 플레이한 게임"에 표시된 다는 것을 명심하십시오.

그 외에, 이 기능은 ASF는 실행중이지만 스팀 클라이언트는 동시에 열어놓지 않고도 알림과 읽지 않은 메시지를 수신하려는 경우에 중요합니다. 이것은 ASF가 스팀 클라이언트 그 자체인 것 처럼 행동하며, ASF가 그렇게 하던 아니던 스팀이 모든 메시지와 이벤트를 ASF로 보내주기 때문입니다. ASF와 스팀 클라이언트를 둘 다 실행하는 것은 문제가 없습니다. 두 클라이언트 모두 정확하게 동일한 이벤트를 수신합니다. 하지만 만약 ASF가 실행중이라면 스팀 클라이언트가 존재하지 않아 받을 수 없음에도 불구하고 특정 이벤트와 메시지를 "배달된" 것으로 표시할 수도 있습니다. 오프라인 상태는 이 문제도 해결합니다. ASF는 이 경우 어떤 커뮤니티 이벤트트로도 간주되지 않기때문에 모든 읽지 않은 메시지와 다른 이벤트가 당신이 돌아오면 읽지 않은 상태로 정상적으로 표시될 것입니다.

It's important to note that ASF running on `Offline` mode will **not** be able to receive commands in usual Steam chat way, as the chat, as well as entire community presence is in fact, entirely offline. A solution to this issue is using `Invisible` mode instead which works in a similar way (not exposing status), but keeps the ability to receive and respond to messages (so also a potential to dismiss notifications and unread messages as stated above). `Invisible` mode makes the most sense on alt accounts that you don't want to expose (status-wise), but still be able to send commands to.

However, there is one catch with `Invisible` mode - it doesn't go well with primary accounts. This is because **any** Steam session that is currently online **exposes** the status, even if ASF itself does not. This is caused by the current limitation/bug of the Steam network that isn't possible to be fixed on ASF side, so if you want to use `Invisible` mode you will also need to ensure that **all** other sessions to the same account use `Invisible` mode as well. This will be the case on alt accounts where ASF is hopefully the only active session, but on primary accounts you'll almost always prefer to show as `Online` to your friends, hiding only ASF activity, and in this case `Invisible` mode will be entirely useless for you (we recommend to use `Offline` mode instead). Hopefully this limitation/bug will be eventually solved in the future by Valve, but I wouldn't expect that to happen anytime soon...

이 속성값을 어떻게 설정할지 잘 모르겠다면, 주 계정은 `0` (`오프라인`)으로 놓고 다른 계정은 기본값인 `1` (`온라인`) 로 두는 것을 추천합니다.

* * *

### `PasswordFormat`

`byte` type with default value of `0`. This property defines the format of `SteamPassword` property, and currently supports - `0` for `PlainText`, `1` for `AES` and `2` for `ProtectedDataForCurrentUser`. Please refer to **[Security](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security)** section if you want to learn more, as you'll need to ensure that `SteamPassword` property indeed includes password in matching `PasswordFormat`. In other words, when you change `PasswordFormat` then your `SteamPassword` should be **already** in that format, not just aiming to be. Unless you know what you're doing, you should keep it with default value of `0`.

* * *

### `Paused`

`bool` type with default value of `false`. This property defines initial state of `CardsFarmer` module. With default value of `false`, bot will automatically start farming when it's started, either because of `Enabled` or `start` command. Switching this property to `true` should be done only if you want to manually `resume` automatic farming process, for example because you want to use `play` all the time and never use automatic `CardsFarmer` module - this works exactly the same as `pause` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. If you're unsure whether you want this feature enabled or not, keep it with default value of `false`.

* * *

### `RedeemingPreferences`

`byte flags` type with default value of `0`. This property defines ASF behaviour when redeeming cd-keys, and is defined as below:

| 값 | 이름               | 설명                                                                             |
| - | ---------------- | ------------------------------------------------------------------------------ |
| 0 | None             | No redeeming preferences, typical                                              |
| 1 | Forwarding       | Forward keys unavailable to redeem to other bots                               |
| 2 | Distributing     | Distribute all keys among itself and other bots                                |
| 4 | KeepMissingGames | Keep keys for (potentially) missing games when forwarding, leaving them unused |

Please notice that this property is `flags` field, therefore it's possible to choose any combination of available values. Check out **[flags mapping](#json-mapping)** if you'd like to learn more. Not enabling any of flags results in `None` option.

`Forwarding` will cause bot to forward a key that is not possible to redeem, to another connected and logged on bot that is missing that particular game (if possible to check). The most common situation is forwarding `AlreadyPurchased` game to another bot that is missing that particular game, but this option also covers other scenarios, such as `DoesNotOwnRequiredApp`, `RateLimited` or `RestrictedCountry`.

`Distributing` will cause bot to distribute all received keys among itself and other bots. This means that every bot will get a single key from the batch. Typically this is used only when you're redeeming many keys for the same game, and you want to evenly distribute them among your bots, as opposed to redeeming keys for various different games. This feature makes no sense if you're redeeming only one key in a single `redeem` action (as there are no extra keys to be distributed).

`KeepMissingGames` will cause bot to skip `Forwarding` when we can't be sure if key being redeemed is in fact owned by our bot, or not. This basically means that `Forwarding` will apply **only** to `AlreadyPurchased` keys, instead of covering also other cases such as `DoesNotOwnRequiredApp`, `RateLimited` or `RestrictedCountry`. Typically you might want to use this option on primary account, to ensure that keys being redeemed on it won't be forwarded further if your bot for example becomes temporarily `RateLimited`. As you can guess from the description, this field has absolutely no effect if `Forwarding` is not enabled.

Enabling both `Forwarding` and `Distributing` will add distributing feature on top of forwarding one, which makes ASF trying to redeem one key on all bots firstly (forwarding) before moving to the next one (distributing). Typically you want to use this option only when you want `Forwarding`, but with altered behaviour of switching the bot on key being used, instead of always going in-order with every key (which would be `Forwarding` alone). This behaviour can be beneficial if you know that majority or even all of your keys are tied to the same game, because in this situation `Forwarding` alone would try to redeem everything on one bot firstly (which makes sense if your keys are for unique games), and `Forwarding` + `Distributing` will switch the bot on the next key, "distributing" the task of redeeming new key onto another bot than the initial one (which makes sense if keys are for the same game, skipping one pointless attempt per key).

The actual bots order for all of the redeeming scenarios is alphabetical, excluding bots that are unavailable (not connected, stopped or likewise). Please keep in mind that there is per-IP and per-account hourly limit of redeeming tries, and every redeem try that didn't end with `OK` contributes to failed tries. ASF will do its best to minimize number of `AlreadyPurchased` failures, e.g. by trying to avoid forwarding a key to another bot that already owns that particular game, but it's not always guaranteed to work due to how Steam is handling licenses. Using redeeming flags such as `Forwarding` or `Distributing` will always increase your likelyhood to hit `RateLimited`.

Also keep in mind that you can't forward or distribute keys to bots that you do not have access to. This should be obvious, but ensure that you're at least `Operator` of all the bots you want to include in your redeeming process, for example with `status ASF` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**.

* * *

### `SendOnFarmingFinished`

`bool` type with default value of `false`. When ASF is done with farming given account, it can automatically send steam trade containing everything farmed up to this point to user with `Master` permission, which is very convenient if you don't want to bother with trades yourself. This option works the same as `loot` command, therefore keep in mind that it requires user with `Master` permission set, you might also require valid `SteamTradeToken`, including using an account that is actually eligible for trading. In addition to initiating `loot` after finishing farming, ASF will also initiate `loot` on each new items notification (when not farming), and after completing each trade that results in new items (always) when this option is active. This is especially useful for "forwarding" items received from other people to our account. It's strongly recommended to use this feature together with **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** being set, as there is no point in sending automatic trades if you need to manually confirm them. If you're not sure how to set this property, leave it with default value of `false`.

* * *

### `SendTradePeriod`

`byte` type with default value of `0`. This property works very similar to `SendOnFarmingFinished` property, with one difference - instead of sending trade when farming is done, we can also send it every `SendTradePeriod` hours, regardless of how much we have to farm left. This is useful if you want to `loot` your alt accounts on usual basis instead of waiting for it to finish farming. Default value of `0` disables this feature, if you want your bot to send you trade e.g. every day, you should put `24` here. It's strongly recommended to use this feature together with **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** being set, as there is no point in sending automatic trades if you need to manually confirm them. If you're not sure how to set this property, leave it with default value of `0`.

* * *

### `ShutdownOnFarmingFinished`

`bool` type with default value of `false`. ASF is "occupying" an account for the whole time of process being active. When given account is done with farming, ASF periodically checks it (every `IdleFarmingPeriod` hours), if perhaps some new games with steam cards were added in the meantime, so it can resume farming of that account without a need to restart the process. This is useful for majority of people, as ASF can automatically resume farming when needed. However, you may actually want to stop the process when given account is fully farmed, you can achieve that by setting this property to `true`. When enabled, ASF will proceed with logging off when account is fully farmed, which means that it won't be periodically checked or occupied anymore. You should decide yourself if you prefer ASF to work on given bot instance for the whole time, or if perhaps ASF should stop it when farming process is done. When all accounts are stopped and process is not running in `--process-required` **[mode](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-Line-Arguments)**, ASF will shutdown as well. If you're not sure how to set this property, leave it with default value of `false`.

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

| 값 | 이름            | 설명                                                                                                                                                                                                 |
| - | ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0 | None          | No permission, this is mainly a reference value that is assigned to steam IDs missing in this dictionary - there is no need to define anybody with this permission                                 |
| 1 | FamilySharing | Provides minimum access for family sharing users. Once again, this is mainly a reference value since ASF is capable of automatically discovering steam IDs that we permitted for using our library |
| 2 | Operator      | Provides basic access to given bot instances, mainly adding licenses and redeeming keys                                                                                                            |
| 3 | Master        | Provides full access to given bot instance                                                                                                                                                         |

In short, this property allows you to handle permissions for given users. Permissions are important mainly for access to ASF **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**, but also for enabling many ASF features, such as accepting trades. For example you might want to set your own account as `Master`, and give `Operator` access to 2-3 of your friends so they can easily redeem keys for your bot with ASF, while **not** being eligible e.g. for stopping it. Thanks to that you can easily assign permissions to given users and let them use your bot to some specified by you degree.

We recommend to set exactly one user as `Master`, and any amount you wish as `Operator`s and below. While it's technically possible to set multiple `Master`s and ASF will work correctly with them, for example by accepting all of their trades sent to the bot, ASF will use only one of them (with lowest steam ID) for every action that requires a single target, for example a `loot` request, so also properties like `SendOnFarmingFinished` or `SendTradePeriod`. If you perfectly understand those limitations, especially the fact that `loot` request will always send items to the `Master` with lowest steam ID, regardless of the `Master` that actually executed the command, then you can define multiple users with `Master` permission here, but we still recommend a single master scheme - multiple masters scheme is discouraged setup that is not supported.

It's nice to note that there is one more extra `Owner` permission, which is declared as `SteamOwnerID` global config property. You can't assign `Owner` permission to anybody here, as `SteamUserPermissions` property defines only permissions that are related to the bot instance, and not ASF as a process.

* * *

### `TradingPreferences`

`byte flags` type with default value of `0`. This property defines ASF behaviour when in trading, and is defined as below:

| 값 | 이름                  | 설명                                                                                                                                                                              |
| - | ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0 | None                | No trading preferences - accepts only `Master` trades                                                                                                                           |
| 1 | AcceptDonations     | Accepts trades in which we're not losing anything                                                                                                                               |
| 2 | SteamTradeMatcher   | Accepts dupes-matching **[STM](https://www.steamtradematcher.com)**-like trades. Visit **[trading](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading)** for more info |
| 4 | MatchEverything     | Requires `SteamTradeMatcher` to be set, and in combination with it - also accepts bad trades in addition to good and neutral ones                                               |
| 8 | DontAcceptBotTrades | Doesn't automatically accept `loot` trades from other bot instances                                                                                                             |

Please notice that this property is `flags` field, therefore it's possible to choose any combination of available values. Check out **[flags mapping](#json-mapping)** if you'd like to learn more. Not enabling any of flags results in `None` option.

For further explanation of ASF trading logic, and description of every available flag, please visit **[Trading](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading)** section.

* * *

### `TransferableTypes`

`ImmutableHashSet<byte>` type with default value of `1, 3, 5` steam item types. This property defines which Steam item types will be considered for transferring between bots, during `transfer` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. ASF will ensure that only items from `TransferableTypes` will be included in a trade offer, therefore this property allows you to choose what you want to receive in a trade offer that is being sent to one of your bots.

| 값 | 이름                | 설명                                                            |
| - | ----------------- | ------------------------------------------------------------- |
| 0 | Unknown           | Every type that doesn't fit in any of the below               |
| 1 | BoosterPack       | Unpacked booster pack                                         |
| 2 | Emoticon          | Emoticon to use in Steam Chat                                 |
| 3 | FoilTradingCard   | Foil variant of `TradingCard`                                 |
| 4 | ProfileBackground | Profile background to use on your Steam profile               |
| 5 | TradingCard       | Steam trading card, being used for crafting badges (non-foil) |
| 6 | SteamGems         | Steam gems being used for crafting boosters, sacks included   |

Please note that regardless of the settings above, ASF will only ask for Steam (`appID` of 753) community (`contextID` of 6) items, so all game items, gifts and likewise, are excluded from the trade offer by definition.

Default ASF setting is based on most common usage of the bot, with transfering only booster packs, and trading cards (including foils). The property defined here allows you to alter that behaviour in whatever way that satisfies you. Please keep in mind that all types not defined above will show as `Unknown` type, which is especially important when Valve releases some new Steam item, that will be marked as `Unknown` by ASF as well, until it's added here (in the future release). That's why in general it's not recommended to include `Unknown` type in your `TransferableTypes`, unless you know what you're doing, and you also understand that ASF will send your entire inventory in a trade offer if Steam Network gets broken again and reports all your items as `Unknown`. My strong suggestion is to not include `Unknown` type in the `TransferableTypes`, even if you expect to transfer everything.

* * *

### `UseLoginKeys`

`bool` type with default value of `true`. This property defines if ASF should use login keys mechanism for this Steam account. Login keys mechanism works very similar to official Steam client's "remember me" option, which makes it possible for ASF to store and use temporary one-time use login key for next logon attempt, effectively skipping a need of providing password, Steam Guard or 2FA code as long as our login key is valid. Login key is stored in `BotName.db` file and updated automatically. This is why you don't need to provide password/SteamGuard/2FA code after logging in successfully with ASF just once.

Login keys are used by default for your convenience, so you don't need to input `SteamPassword`, SteamGuard or 2FA code (when not using **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)**) on each login. It's also superior alternative since login key can be used only for a single time and does not reveal your original password in any way. Exactly the same method is being used by your original Steam client, which saves your account name and login key for your next logon attempt, effectively being the same as using `SteamLogin` with `UseLoginKeys` and empty `SteamPassword` in ASF.

However, some people might be concerned even about this little detail, therefore this option is available here for you if you'd like to ensure that ASF won't store any kind of token that would allow resuming previous session after being closed, which will result in full authentication being mandatory on each login attempt. Disabling this option will work exactly the same as not checking "remember me" in official Steam client. Unless you know what you're doing, you should keep it with default value of `true`.

**[위로 돌아가기](#환경설정)**

* * *

## 파일 구조

ASF는 꽤 간단한 파일구조를 사용합니다.

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
    

ASF를 다른 PC 등 새로운 위치로 옮기려면 `config` 디렉토리 하나만을 이동/복사하는 것으로 충분합니다. 그리고 이것이 ASF 백업으로 권장되는 방법입니다.

`log.txt` 파일은 마지막 ASF 실행으로 생성된 로그를 담고 있습니다. 이 파일은 어떠한 민감한 정보도 포함하고 있지 않으며, 이슈나 충돌, 혹은 지난번 ASF 실행에서 무슨일이 있었는지 정보로써도 굉장히 가치가 있습니다. 이슈나 버그가 발생하면 우리는 이 파일을 자주 요청하게 될 것입니다. ASF는 이 파일을 자동으로 관리하지만, 고급 사용자라면 ASF **[로그](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Logging-ko-KR)** 모듈을 더 깊이 조절할 수 있습니다.

`config` directory is the place that holds configuration for ASF, including all of its bots.

`ASF.json` is a global ASF configuration file. This config is used for specifying how ASF behaves as a process, which affects all of the bots and program itself. You can find global properties there, such as ASF process owner, auto-updates or debugging.

`BotName.json` is a config of given bot instance. This config is used for specifying how given bot instance behaves, therefore those settings are specific to that bot only and not shared across other ones. This allows you to configure bots with various different settings and not necessarily all of them working in exactly the same way.

Apart from config files, ASF also uses `config` directory for storing databases.

`ASF.db` is a global ASF database file. It acts as a global persistent storage and is used for saving various information related to ASF process, such as IPs of local Steam servers. **이 파일을 수정해서는 안됩니다**.

`BotName.db` is a database of given bot instance. This file is used for storing crucial data about given bot instance in persistent storage, such as login keys or ASF 2FA. **이 파일을 수정해서는 안됩니다**.

`BotName.bin` is a special file of given bot instance, which holds information about Steam sentry hash. Sentry hash is used for authenticating using `SteamGuard` mechanism, very similar to Steam `ssfn` file. **이 파일을 수정해서는 안됩니다**.

`BotName.keys` is a special file that can be used for importing keys into **[background games redeemer](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer)**. It's not mandatory and not generated, but recognized by ASF. This file is automatically deleted after keys are successfully imported.

`BotName.maFile` is a special file that can be used for importing **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)**. It's not mandatory and not generated, but recognized by ASF if your `BotName` does not use ASF 2FA yet. This file is automatically deleted after ASF 2FA is successfully imported.

**[위로 돌아가기](#환경설정)**

* * *

## JSON 매핑

모든 환경설정 속성값은 타입이 있습니다. 속성값의 타입은 유효한 값을 정의합니다. 주어진 타입에 유효한 값만 사용할 수 있습니다. 유효하지 않은 값을 사용하면 ASF는 환경설정을 수행할 수 없습니다.

**We strongly recommend to use ConfigGenerator for generating configs** - it handles most of the low-level stuff (such as types validation) for you, so you only need to input proper values, and you also don't need to understand variable types specified below. This section is mainly for people generating/editing configs manually, so they know what values they can use.

Types used by ASF are native C# types, which are specified below:

* * *

`bool` - `true`와 `false` 값만 받는 불린타입입니다.

예: `"Enabled": true`

* * *

`byte` - `0`부터 `255` 까지의 정수만 받는 Unsigned 바이트 타입입니다.

예: `"ConnectionTimeout": 60`

* * *

`uint` - `0`부터 `4294967295` 까지의 정수만 받는 Unsigned 정수 타입입니다.

* * *

`ulong` - `0`부터 `18446744073709551615` 까지의 정수만 받는 Unsigned long 정수 타입입니다.

예: `"SteamMasterClanID": 103582791440160998`

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

| 값 | 이름   |
| - | ---- |
| 0 | None |
| 1 | A    |
| 2 | B    |
| 4 | C    |

Using `B + C` would result in value of `6`, using `A + C` would result in value of `5`, using `C` would result in value of `4` and so on. This allows you to create any possible combination of enabled values - if you decided to enable all of them, making `None + A + B + C`, you'd get value of `7`. Also notice that flag with value of `0` is enabled by definition in all other available combinations, therefore very often it's a flag that doesn't enable anything specifically (such as `None`).

So as you can see, in above example we have 3 available flags to switch on/off (`A`, `B`, `C`), and 8 possible values overall (`None -> 0`, `A -> 1`, `B -> 2`, `A+B -> 3`, `C -> 4`, `A+C -> 5`, `B+C -> 6`, `A+B+C -> 7`).

**[위로 돌아가기](#환경설정)**

* * *

## 호환성 매핑

Due to JavaScript limitations of being unable to properly serialize simple `ulong` fields in JSON when using web-based ConfigGenerator, `ulong` fields will be rendered as strings with `s_` prefix in the resulting config. This includes for example `"SteamOwnerID": 76561198006963719` that will be written by our ConfigGenerator as `"s_SteamOwnerID": "76561198006963719"`. ASF includes proper logic for handling this string mapping automatically, so `s_` entries in your configs are actually valid and correctly generated. If you're generating configs yourself, we recommend to stick with original `ulong` fields if possible, but if you're unable to do so, you can also follow this scheme and encode them as strings with `s_` prefix added to their names. We hope to resolve this JavaScript limitation eventually.

**[위로 돌아가기](#환경설정)**

* * *

## 환경설정 호환성

It's top priority for ASF to remain compatible with older configs. As you should already know, missing config properties are treated the same as they would be defined with their **default values**. Therefore, if new config property gets introduced in new version of ASF, all your configs will remain **compatible** with new version, and ASF will treat that new config property as it'd be defined with its **default value**. You can always add, remove or edit config properties according to your needs. We recommend to limit defined config properties only to those that you want to change, since this way you automatically inherit default values for all other ones, not only keeping your config clean but also increasing compatibility in case we decide to change a default value for property that you don't want to explicitly set yourself. Feel free to check `minimal.json` example configuration file that follows this concept.

**[위로 돌아가기](#환경설정)**

* * *

## 자동 재시작

ASF V2.1.6.2 이상 버전부터 실행중간의 환경설정 수정을 감지할 수 있습니다. 이에 따라 ASF는 자동적으로 아래와 같은 행동을 합니다.

- 새로운 봇 환경설정을 만드는 경우 그 봇 인스턴스의 생성 및 시작(필요한 경우)
- 예전 봇 환경설정을 삭제하는 경우 그 봇 인스턴스의 중지(필요한 경우) 및 제거
- 봇 환경설정을 수정하는 경우 그 봇 인스턴스의 중지 및 시작(필요한 경우)
- 봇 환경설정 이름을 변경하는 경우 새 이름으로 봇 재시작(필요한 경우)

위의 모든 것은 투명하고 프로그램의 재시작이나 다른 영향이 없는 봇 인스턴스의 중지 없이 자동으로 수행됩니다.

게다가 ASF는 `AutoRestart`가 허용되어있다면 `ASF.json` 환경설정이 변경되면 ASF를 재시작합니다. 동일하게 삭제하거나 이름을 바꾸면 프로그램은 종료됩니다.

**[위로 돌아가기](#환경설정)**