# 거래

ASF는 Steam 비-대화형(오프라인) 거래를 지원합니다. 거래를 받는것(수락/거절)과 보내는 것이 즉시 가능하고, 특별한 설정이 필요하지 않지만 분명히 제한되지 않은 Steam 계정이 필요합니다.(상점에서 $5를 이미 사용한 계정) 거래 모듈은 제한된 계정에서는 사용할 수 없습니다.

* * *

## 논리구조

ASF는 `주인(Master)` 혹은 그 이상의 봇 접근권한을 가진 사용자가 보낸 모든 거래를 항목에 상관없이 수락합니다. 이렇게 해서 봇 인스턴스에서 농사지은 Steam 카드를 쉽게 가져올 수 있고 봇이 보관함에 가지고 있는 Steam 항목을 쉽게 관리할 수 있습니다.

ASF는 거래 모듈의 블랙리스트에 오른 주인이 아닌 모든 사용자로부터의 거래 제안을 내용과 상관없이 거절합니다. 블랙리스트는 표준 `봇이름.db` 데이터베이스에 저장되며 `bl`, `bladd`, `blrm` **[명령어](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-ko-KR)**로 관리할 수 있습니다. 이는 Steam이 제공하는 표준 사용자 차단의 대안으로써 작동하므로 사용에 주의하시기 바랍니다.

ASF는 `TradingPreferences`에 `봇거래수락안함(DontAcceptBotTrades)`이 명시되어있지 않는 한 봇 간의 모든 `loot` 같은 거래를 수락합니다. 즉, `TradingPreferences`의 기본값인 `없음(None)`은 위에서 설명한대로 봇의 `주인(Master)` 권한을 가진 사용자로부터의 거래를 자동으로 수락하도록 합니다. 또한 ASF 프로세스로 일어나는 다른 봇의 기부 거래도 자동으로 수락합니다. 다른 봇으로부터의 기부 거래를 비활성화 하려면 `TradingPreferences`의 `봇거래수락안함(DontAcceptBotTrades)`을 사용하십시오.

`TradingPreferences`에 `기부수락(AcceptDonations)`을 활성화하면 봇 계정이 어떤 항목도 잃지 않는 기부 거래를 수락할 것입니다. 이 속성값은 봇이 아닌 계정에만 영향을 주고, 봇 계정은 `봇거래수락안함(DontAcceptBotTrades)`의 영향을 받습니다. `기부수락(AcceptDonations)`은 다른 사람이나 ASF 프로세스에 참여하지 않은 봇으로부터의 기부를 쉽게 수락하게 해줍니다.

어떤 항목도 잃지 않는 경우 확인사항이 없으므로 `기부수락(AcceptDonations)`은 **[ASF 2단계 인증](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-ko-KR)** 을 필요로 하지 않음을 알아두십시오.

`TradingPreferences`를 적절하게 수정하여 ASF의 거래 기능을 더 자세하게 지정할 수 있습니다. `TradingPreferences`의 주요 기능 중 하나는 `SteamTradeMatcher` 옵션으로, **[SteamTradeMatcher](https://www.steamtradematcher.com)** 의 공개 리스트와 협업할때 특히 유용한데, 배지를 완성할 수 있도록 거래를 받아들이는 ASF의 내장 논리구조를 사용하도록 합니다. 물론 SteamTradeMatcher 없이도 가능합니다. 아래에서 더 자세하게 설명하겠습니다.

* * *

## `SteamTradeMatcher`

When `SteamTradeMatcher` is active, ASF will use quite complex algorithm of checking if trade passes STM rules and is at least neutral towards us. The actual logic is following:

- Reject the trade if we're losing anything but item types specified in our `MatchableTypes`.
- Reject the trade if we're not receiving at least the same number of items on per-game and per-type basis.
- Reject the trade if user asks for special Steam summer/winter sale cards, and has a trade hold.
- Reject the trade if trade hold duration exceeds `MaxTradeHoldDuration` global config property.
- Reject the trade if we don't have `MatchEverything` set, and it's worse than neutral for us.
- Accept the trade if we didn't reject it through any of the points above.

It's nice to note that ASF also supports overpaying - the logic will work properly when user is adding something extra to the trade, as long as all above conditions are met.

First 4 reject predicates should be obvious for everyone. The final one includes actual dupes logic which checks current state of our inventory and decides what is the status of the trade.

- Trade is **good** if our progress towards set completion advances. A A (before) <-> A B (after)
- Trade is **neutral** if our progress towards set completion stays in-tact. A B (before) <-> A C (after)
- Trade is **bad** if our progress towards set completion declines. A C (before) <-> A A (after)

STM operates only on good trades, which means that user using STM for dupes matching should always suggest only good trades for us. However, ASF is liberal, and it also accepts neutral trades, because in those trades we're not actually losing anything, so there is no real reason to decline them. This is especially useful for your friends, since they can swap your excessive cards without using STM at all, as long as you're not losing any set progress.

By default ASF will reject bad trades - this is almost always what you want as an user. However, you can optionally enable `MatchEverything` in your `TradingPreferences` in order to make ASF accept all dupe trades, including **bad ones**. This is useful only if you want to run a 1:1 trade bot under your account, as you understand that **ASF will no longer help you progress towards badge completion, and make you prone to losing entire finished set for N dupes of the same card**. Unless you intentionally want to run a trade bot that is **never** supposed to finish any set, you don't want to enable this option.

Regardless of your chosen `TradingPreferences`, a trade being rejected by ASF doesn't mean that you can't accept it yourself. If you kept default value of `BotBehaviour`, which doesn't include `RejectInvalidTrades`, ASF will just ignore those trades - allowing you to decide yourself if you're interested in them or not. Same goes for trades with items outside of `MatchableTypes`, as well as everything else - the module is supposed to help you automate STM trades, not decide what is a good trade and what is not. The only exception from this rule is when talking about users you blacklisted from trading module using `bladd` command - trades from those users are immediately rejected regardless of `BotBehaviour` settings.

It's highly recommended to use **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** when you enable this option, as this function loses its whole potential if you decide to manually confirm every trade. `SteamTradeMatcher` will work properly even without ability to confirm trades, but it might generate backlog of confirmations if you're not accepting them in time.

* * *

### `능동적 매칭(MatchActively)`

`MatchActively` setting is extended version of `SteamTradeMatcher` which in addition to passive matching offered by that option, also includes active matching in which the bot will send trades to other people.

In order to make use of that option, you have a set of requirements to meet. Firstly, you need to enable `SteamTradeMatcher` (as this feature is extension of that), and ensure that you have `MatchEverything` disabled (as trading bots never match actively). Afterwards, you have to be eligible for our **[ASF STM listing](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Statistics#current-privacy-policy)**, without a requirement of 100 items. This means that, at the minimum you must have `Statistics` enabled, **[unrestricted](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663)** account, **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication#asf-2fa)** active, **[public inventory](https://steamcommunity.com/my/edit/settings)** and at least one valid type in `MatchableTypes`, such as trading cards.

If you meet all of the requirements above, ASF will periodically communicate with our **[public ASF STM listing](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Statistics#public-asf-stm-listing)** in order to actively match `Any` (`MatchEverything`) bots currently available.

- Each matching is composed of "rounds", with up to `10` being maximum at once.
- In each round ASF will fetch our inventory and inventory of selected bots that are listed in order to find `MatchableTypes` items that can be matched. If match is found, ASF will send and confirm trade offer automatically.
- Each set (composition of item type and appID it's from) can be matched in a single round only once. This is implemented in order to minimize "items no longer available" and avoid a need to wait for each bot to react before sending all the trades.
- ASF will send no more than `255` items in a single trade, and no more than `5` trades to a single user in a single round. This is imposed by Steam limits, as well as our own load-balancing.
- Matching round ends the moment we try to match a total of `40` bots, or when we hit no items to match with consecutive `20` different bots.
- If last round resulted in at least a single trade being sent, next round starts within `5` minutes since the last one (to add some cooldown and allow all bots to react to our trades), otherwise matching ends and repeats itself in `8` hours.

This module is supposed to be transparent. Matching will start in approximately `1` hour since ASF start, and will repeat each `8` hours (if needed). `MatchActively` feature is aimed to be used as a long-run, periodical measure to ensure that we're actively heading towards sets completion, but without a short-term time and resources pressure that would happen if this was offered as a command. The target users of this module are primary accounts and "stash" alt accounts, although it can be used on any bot that is not set to `MatchEverything`.

ASF will do its best to minimize the amount of requests and pressure generated by using this option, while at the same time maximizing efficiency of matching to the upper limit. Exact algorithm of choosing bots to match is ASF's implementation detail, but right now ASF will tend to favor bots with better diversity of games that their items are from.

`MatchActively` takes into accounts bots that you blacklisted from trading through `bladd` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** and will not attempt to actively match them. This can be used for telling ASF which bots it should never match, even if they'd have potential dupes for us to use.