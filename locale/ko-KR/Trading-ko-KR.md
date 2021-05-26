# 거래

ASF는 Steam 비-대화형(오프라인) 거래를 지원합니다. 거래를 받는것(수락/거절)과 보내는 것이 즉시 가능하고, 특별한 설정이 필요하지 않지만 분명히 제한되지 않은 Steam 계정이 필요합니다.(상점에서 $5를 이미 사용한 계정) 거래 모듈은 제한된 계정에서는 사용할 수 없습니다.

---

## 논리구조

ASF는 `주인(Master)` 혹은 그 이상의 봇 접근권한을 가진 사용자가 보낸 모든 거래를 항목에 상관없이 수락합니다. This allows not only easily looting steam cards farmed by the bot instance, but also allows to easily manage Steam items that bot stashes in the inventory - including those from other games (such as CS:GO).

ASF는 거래 모듈의 블랙리스트에 오른 주인이 아닌 모든 사용자로부터의 거래 제안을 내용과 상관없이 거절합니다. 블랙리스트는 표준 `봇이름.db` 데이터베이스에 저장되며 `bl`, `bladd`, `blrm` **[명령어](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-ko-KR)**로 관리할 수 있습니다. 이는 Steam이 제공하는 표준 사용자 차단의 대안으로써 작동하므로 사용에 주의하시기 바랍니다.

ASF는 `TradingPreferences`에 `봇거래수락안함(DontAcceptBotTrades)`이 명시되어있지 않는 한 봇 간의 모든 `loot` 같은 거래를 수락합니다. 즉, `TradingPreferences`의 기본값인 `없음(None)`은 위에서 설명한대로 봇의 `주인(Master)` 권한을 가진 사용자로부터의 거래를 자동으로 수락하도록 합니다. 또한 ASF 프로세스로 일어나는 다른 봇의 기부 거래도 자동으로 수락합니다. 다른 봇으로부터의 기부 거래를 비활성화 하려면 `TradingPreferences`의 `봇거래수락안함(DontAcceptBotTrades)`을 사용하십시오.

`TradingPreferences`에 `기부수락(AcceptDonations)`을 활성화하면 봇 계정이 어떤 항목도 잃지 않는 기부 거래를 수락할 것입니다. 이 속성값은 봇이 아닌 계정에만 영향을 주고, 봇 계정은 `봇거래수락안함(DontAcceptBotTrades)`의 영향을 받습니다. `기부수락(AcceptDonations)`은 다른 사람이나 ASF 프로세스에 참여하지 않은 봇으로부터의 기부를 쉽게 수락하게 해줍니다.

어떤 항목도 잃지 않는 경우 확인사항이 없으므로 `기부수락(AcceptDonations)`은 **[ASF 2단계 인증](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-ko-KR)** 을 필요로 하지 않음을 알아두십시오.

`TradingPreferences`를 적절하게 수정하여 ASF의 거래 기능을 더 자세하게 지정할 수 있습니다. `TradingPreferences`의 주요 기능 중 하나는 `SteamTradeMatcher` 옵션으로, **[SteamTradeMatcher](https://www.steamtradematcher.com)** 의 공개 리스트와 협업할때 특히 유용한데, 배지를 완성할 수 있도록 거래를 받아들이는 ASF의 내장 논리구조를 사용하도록 합니다. 물론 SteamTradeMatcher 없이도 가능합니다. 아래에서 더 자세하게 설명하겠습니다.

---

## `SteamTradeMatcher`

`SteamTradeMatcher`가 활성화 되면, ASF는 거래가 STM의 규칙을 통과하고 우리에게 적어도 중립인지를 확인하는 꽤 복잡한 알고리즘을 사용합니다. 실제 논리구조는 다음과 같습니다.

- `MatchableTypes`에 특정된 항목타입이 아닌 것을 잃게 되면 거래를 거절합니다.
- Reject the trade if we're not receiving at least the same number of items on per-game, per-type and per-rarity basis.
- 사용자가 특별한 Steam 여름/겨울 세일 카드를 요청하고, 거래 지연의 영향을 받는다면 거래를 거절합니다.
- 거래 지연 기간이 일반 환경설정의 `MaxTradeHoldDuration` 속성값을 초과하는 경우 거래를 거절합니다.
- `MatchEverything` 설정이 아니라면 거래를 거절합니다. 이는 우리에게 중립보다 더 나쁩니다.
- 위의 내용으로 거절되지 않았다면 거래를 수락합니다.

ASF가 과지급을 지원함을 알아두십시오. 이 논리구조는 위의 모든 조건을 만족하면서 사용자가 뭔가를 추가로 거래에 추가할때 정확하게 동작합니다.

처음 4개의 거절 조건은 모두에게 명백해야 합니다. 마지막 거절 조건은 우리 보관함의 현재 상태를 확인하고 거래 상태를 결정하는 실제 중복 논리구조를 포함합니다.

- 세트 완성 진행도가 증가했다면 이 거래는 **좋음(good)** 입니다. 예시: A A (거래 전) <-> A B (거래 후)
- 세트 완성 진행도가 현상태 그대로라면 이 거래는 **중립(neutral)** 입니다. 예시: A B (거래 전) <-> A C (거래 후)
- 세트 완성 진행도가 감소했다면 이 거래는 **나쁨(bad)** 입니다. 예시: A C (거래 전) <-> A A (거래 후)

STM은 좋음 거래만 수행합니다. 즉, 중복 매칭을 위해 STM을 사용하는 사용자는 우리에게는 항상 좋음 거래만 제안할 것입니다. 하지만 ASF는 자유민주주의라서 중립 거래도 수락합니다. 중립 거래는 실제로 우리가 잃는것이 없기 때문에, 거절할 이유가 없습니다. 이는 당신의 친구들에게 특히 유용합니다. 그들은 STM을 전혀 사용하지 않고도 당신의 과도한 카드를 교환할 수 있습니다. 당신의 세트 완성 진행도도 떨어지지 않습니다.

기본적으로 ASF는 나쁨 거래를 거절합니다. 이는 사용자라면 거의 항상 원하는 것입니다. 하지만, ASF가 **나쁨 거래**를 포함한 모든 중복 거래를 받아들일 수 있도록 `TradingPreferences`의 `MatchEverything`를 활성화할 수도 있습니다. 이는 당신의 계정에서 1:1 거래 봇을 실행하고 싶은 경우 유용합니다. 물론 **ASF는 더이상 당신의 배지완성 진행도를 도와주지 않을것이고, 완성된 세트를 중복 카드 N장으로 바꿔버리기 쉽게 함을** 알고 계십시오. 어떤 세트도 **절대** 완성하지 못하는 거래 봇을 일부러 실행하려는 것이 아니라면, 이 옵션을 활성화하지 마십시오.

Regardless of your chosen `TradingPreferences`, a trade being rejected by ASF doesn't mean that you can't accept it yourself. If you kept default value of `BotBehaviour`, which doesn't include `RejectInvalidTrades`, ASF will just ignore those trades - allowing you to decide yourself if you're interested in them or not. Same goes for trades with items outside of `MatchableTypes`, as well as everything else - the module is supposed to help you automate STM trades, not decide what is a good trade and what is not. The only exception from this rule is when talking about users that you blacklisted from trading module using `bladd` command - trades from those users are immediately rejected regardless of `BotBehaviour` settings.

It's highly recommended to use **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** when you enable this option, as this function loses its whole potential if you decide to manually confirm every trade. `SteamTradeMatcher` will work properly even without ability to confirm trades, but it can generate backlog of confirmations if you're not accepting them in time.

---

### `능동적 매칭(MatchActively)`

`MatchActively` setting is extended version of `SteamTradeMatcher` which in addition to passive matching offered by that option, also includes active matching in which the bot will send trades to other people.

In order to make use of that option, you have a set of requirements to meet. Firstly, you need to enable `SteamTradeMatcher` (as this feature is extension of that), and ensure that you have `MatchEverything` **disabled** (as trading bots never match actively). Afterwards, you have to be eligible for our **[ASF STM listing](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Statistics#current-privacy-policy)**, with a bit relaxed requirements. At the minimum you must have `Statistics` enabled, **[unrestricted](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663)** account, **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication#asf-2fa)** active and at least one valid type in `MatchableTypes`, such as trading cards.

If you meet all of the requirements above, ASF will periodically communicate with our **[public ASF STM listing](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Statistics#public-asf-stm-listing)** in order to actively match bots that are currently available.

- Each matching session is composed of "rounds", with `10` being maximum in a single matching session.
- In each round ASF will fetch our inventory and inventory of selected bots that are listed in order to find `MatchableTypes` items that can be matched. If match is found, ASF will send and confirm trade offer automatically.
- Each set (composition of appID, type and rarity of the item) can be matched in a single round only once. This is implemented in order to minimize "items no longer available" and avoid a need to wait for each bot to react before sending all the trades. It's also the primary reason why matching is composed of rounds and not one ongoing process.
- ASF will send no more than `255` items in a single trade, and no more than `5` trades to a single user in a single round. This is imposed by Steam limits, as well as our own load-balancing.
- ASF has a hard limit of `40` unique bots that can be matched in a single round, if not cancelled before due to running out of sets to match - in this case, during the next round ASF will try to match bots that weren't matched yet firstly.
- If ASF determines that the matching should continue, next round starts within `5` minutes since the last one (to add some cooldown and allow all bots to react to our trades), otherwise matching session ends and repeats itself in `8` hours.

This module is supposed to be transparent. Matching will start in approximately `1` hour since ASF start, and will repeat itself each `8` hours (if needed). `MatchActively` feature is aimed to be used as a long-run, periodical measure to ensure that we're actively heading towards sets completion, but without a short-term time and resources pressure that would happen if this was offered as a command. The target users of this module are primary accounts and "stash" alt accounts, although it can be used by any bot that is not set to `MatchEverything`.

ASF does its best to minimize the amount of requests and pressure generated by using this option, while at the same time maximizing efficiency of matching to the upper limit. The exact algorithm of choosing the bots to match and otherwise organize the whole process, is ASF's implementation detail and can change in regards to feedback, situation and possible future ideas.

The current version of the algorithm makes ASF prioritize `Any` bots first, especially those with better diversity of games that their items are from. When running out of `Any` bots, ASF will move on to the fair ones upon same diversity rule, with those owning excessive number of items further deprioritized due to higher chance of possible inventory-related problems compared to other bots. Regardless of that, ASF will try to match every available bot at least once, to ensure that we're not missing on a possible set progress.

`MatchActively` takes into account bots that you blacklisted from trading through `bladd` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** and will not attempt to actively match them. This can be used for telling ASF which bots it should never match, even if they'd have potential dupes for us to use.