# Обмены

ASF включает в себя поддержку не-интерактивных (офлайн) обменов Steam. Как получение (принятие/отклонение) так и отправка обменов работают не требуя дополнительной настройки, однако, естественно, требуют аккаунт Steam без ограничений (такой, на котором потрачено больше чем 5$ в эквиваленте). Модуль обменов недоступен для ограниченных аккаунтов.

* * *

## Логика

ASF всегда принимает все обмены, независимо от предметов в них, присланные от пользователя с правами `Master` (или выше). Это позволяет не только легко забрать карты, которые нафармил бот, но также удобно манипулировать любыми предметами Steam, хранящимися на этом боте.

ASF будет отказыватся от обменов, независимо от содержимого, которые присланы от пользователей(кроме `Master`) обмены с которыми запрещены в модуле обменов. Список пользователей, с которыми запрещены обмены, хранится в базе данных `BotName.db`, а управлять вы им можете с помощью **[команд](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-ru-RU)** `bl`, `bladd` и `blrm`. Это может использоваться как альтернатив блокировке в Steam - используйте с осторожностью.

ASF будет принимать все `loot`-подобные обмены присланные от других ботов, за исключением случая когда в параметре `TradingPreferences` указано значение `DontAcceptBotTrades`. Вкратце, параметр `TradingPreferences` со значением по умолчанию `None` приведёт к тому, что ASF будет автоматически принимать обмены от пользователей с правами `Master` (описано выше) а также все безвозмездные обмены от других ботов, задействованых в этом процессе ASF. Если вы хотите отключить принятие безвозмездных обменов от других ботов, для этого служит значение `DontAcceptBotTrades` в параметре `TradingPreferences`.

Если вы добавите значение `AcceptDonations` в параметр `TradingPreferences`, ASF также будет принимать любые безвозмездные обмены - обмены, в которых бот не отдаёт никаких предметов. Это влияет только на обмены с не-ботами, поскольку боты на обмены с ботами влияет значение `DontAcceptBotTrades`. `AcceptDonations` позволяет вам легко принимать пожертвования от других людей, а также от ботов не задействованных в данном процессе ASF.

Приятно отметить, что `AcceptDonations` не требует наличия **[2ФА ASF](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-ru-RU)**, поскольку если мы не отдаём предметов - подтверждение на мобильном не требуется.

Вы можете настроить другие возможности обменов в ASF изменяя параметр `TradingPreferences`. Одна из важных функций доступных в `TradingPreferences` - это значение `SteamTradeMatcher`, которое активирует встроенную логику принятия обменов, помогающих собирать карточки для создания значков, она особенно полезна в сочетании с публикацией своего профиля на сайте **[SteamTradeMatcher](https://www.steamtradematcher.com)**, но будет работать и без этого. Эта функция подробно описана ниже.

* * *

## `SteamTradeMatcher`

Если включено значение `SteamTradeMatcher`, ASF будет использовать довольно сложный алгоритм для проверки, удовлетворяет ли обмен правилам STM и является ли для нас как минимум нейтральным. Логика работы следующая:

- Отказ от обмена если мы отдаём что-то, что не указано в параметре `MatchableTypes`.
- Отказ от обмена если мы не получаем как минимум столько же предметов из определённой игры и определённого типа.
- Отказ от обмена если у нас запрашивают предмет летней/зимней распродажи, и при этом у пользователя действует удержание обменов.
- Отказ от обмена если удержание обмена превышает указанное в параметре глобальной конфигурации `MaxTradeHoldDuration`.
- Отказ от обмена если не установлено значение `MatchEverything` и обмен для нас хуже, чем нейтральный.
- Принятие обмена если мы не отказались от него на одном из этапов выше.

Приятно отметить, что ASF также поддерживает переплату - логика будет нормально работать если мы получаем в обмене что-то дополнительно, если все другие условия соблюдены.

Первые 4 условия отказа должны быть очевидны для всех. Последнее условие включает собственно логику проверки дубликатов, которая проверяет наш текущий инвентарь и решает, каков статус этого обмена.

- Обмен **хороший** если наш прогресс к завершению значка увеличивается. A A (было) <-> A B (стало)
- Обмен **нейтральный** если наш прогресс к завершению значка не меняется. A B (было) <-> A C (стало)
- Обмен **плохой** если наш прогресс к завершению значка уменьшается. A C (было) <-> A A (стало)

STM работает только с хорошими обменами, а значит пользователь, пользующийся STM для обмена дубликатов всегда будет предлагать нам только хорошие обмены. Однако, ASF имеет либеральную настройку, и принимает также нейтральные обмены, поскольку в этом случае мы ничего не теряем, поэтому нет реальных причин отклонять их. Это особенно полезно для ваших друзей, поскольку они смогут меняться на ваши лишние карточки вообще не используя STM, если вы при этом не теряете прогресс к завершению значка.

По умолчанию ASF будет отклонять плохие сделки - практически всегда это то, чего хочет пользователь. Однако, при желании вы можете включить значение `MatchEverything` в параметр `TradingPreferences` чтобы ASF принимал все обмены дубликатов, включая **плохие**. Это может быть полезным только если вы хотите сделать из своего аккаунта бота по обмену карт 1:1, и вы должны понимать, что в этом случае **ASF не поможет вашему прогрессу к завершению значков, и сделает возможным потерю уже собранного набора карт на N дубликатов одной и той же карты**. Если вы не хотите намеренно создать бота который **никогда** не соберёт набор карт, вам не стоит включать эту опцию.

В зависимости от выбранных значений в параметре `TradingPreferences`, то, что ASF отказал в обмене не означает что вы не сможете принять её самостоятельно. If you kept default value of `BotBehaviour`, which doesn't include `RejectInvalidTrades`, ASF will just ignore those trades - allowing you to decide yourself if you're interested in them or not. То же самое касается и обменов с предметами, не соответствующими `MatchableTypes`, и прочих случаев - модуль обменов призван помочь автоматизировать обмены по алгоритму STM, а не решать за вас, какой обмен хороший а какой нет. Единственное исключение из этого правила - это пользователи, с которыми запрещены обмены командой `bladd` - предложения обменов от таких пользователей немедленно отклоняются независимо от настроек `BotBehaviour`.

Настоятельно рекомендуем использовать **[2ФА ASF](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-ru-RU)** если вы решите использовать этот функционал, поскольку он не раскрывает полностью свой потенциал если вам приходится вручную подтверждать каждый обмен. `SteamTradeMatcher` будет работать даже без возможности подтверждать обмены, но это может создать очередь на подтверждение если вы не принимаете их своевременно.

* * *

### `MatchActively`

`MatchActively` setting is extended version of `SteamTradeMatcher` which in addition to passive matching offered by that option, also includes active matching in which the bot will send trades to other people.

In order to make use of that option, you have a set of requirements to meet. Firstly, you need to enable `SteamTradeMatcher` (as this feature is extension of that), and avoid `MatchEverything` setting (as trading bots never match actively). Afterwards, you have to be eligible for our **[ASF STM listing](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Statistics#current-privacy-policy)**, without a requirement of 100 items. This means that, at the minimum you must have `Statistics` enabled, **[unrestricted](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663)** account, **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication#asf-2fa)** active, **[public inventory](https://steamcommunity.com/my/edit/settings)** and at least one valid type in `MatchableTypes`, such as trading cards.

If you meet all of the requirements above, ASF will periodically communicate with our **[public ASF STM listing](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Statistics#public-asf-stm-listing)** in order to actively match `Any` (`MatchEverything`) bots currently available.

- Each matching is composed of "rounds", with up to `10` being maximum at once.
- In each round ASF will fetch our inventory and inventory of selected bots that are listed in order to find `MatchableTypes` items that can be matched. If match is found, ASF will send and confirm trade offer automatically.
- Each set (composition of item type and appID it's from) can be matched in a single round only once. This is implemented in order to minimize "items no longer available" and avoid a need to wait for each bot to react before sending all the trades.
- ASF will send no more than `255` items in a single trade, and no more than `5` trades to a single user in a single round. This is imposed by Steam limits, as well as our own load-balancing.
- Matching round ends the moment we try to match a total of `40` bots, or when we hit no items to match with consecutive `20` different bots.
- If last round resulted in at least a single trade being sent, next round starts within `5` minutes since the last one (to add some cooldown and allow all bots to react to our trades), otherwise matching ends and repeats itself in `8` hours.

This module is supposed to be transparent. Matching will start in approximately `1` hour since ASF start, and will repeat each `8` hours (if needed). `MatchActively` feature is aimed to be used as a long-run, periodical measure to ensure that we're actively heading towards sets completion, but without a short-term time and resources pressure that would happen if this was offered as a command. The target users of this module are primary accounts and "stash" alt accounts, although it can be used on any bot that is not set to `MatchEverything`.

ASF will do its best to minimize the amount of requests and pressure generated by using this option, while at the same time maximizing efficiency of matching to the upper limit. Exact algorithm of choosing bots to match is ASF's implementation detail, but right now ASF will tend to favor bots with better diversity of games that their items are from.