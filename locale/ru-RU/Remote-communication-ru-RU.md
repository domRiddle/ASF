# Remote communication

This section elaborates on remote communication that ASF includes, including further explanation on how one can influence it. Хотя мы не считаем что-либо из приведенного ниже вредоносным или иным образом нежелательным, и мы не обязаны по закону раскрывать это, мы хотим, чтобы вы лучше понимали функциональные возможности программы, особенно в отношении вашей конфиденциальности и обмена данными.

## Steam

ASF communicates with Steam network (**[CM servers](https://api.steampowered.com/ISteamDirectory/GetCMList/v1?cellid=0)**), as well as **[Steam API](https://steamcommunity.com/dev)**, **[Steam store](https://store.steampowered.com)** and **[Steam community](https://steamcommunity.com)**.

It's not possible to disable any of the above communication, as it's the core foundation ASF is based on in order to provide its basic functionality. You'll need to refrain from using ASF if you're not comfortable with the above.

## Группа Steam

ASF связывается с нашей **[группой Steam](https://steamcommunity.com/groups/archiasf)**. Группа предоставляет вам объявления, в частности о новых версиях, критических проблемах, проблемах Steam и других вещах, которые важны участникам сообщества. Это также позволяет вам использовать нашу техническую поддержку, задавая вопросы, решая проблемы, сообщая о проблемах или предлагая улучшения. По умолчанию учетные записи, используемые в ASF, автоматически присоединяются к группе при входе в систему.

You can decide to opt-out of joining the group by disabling `SteamGroup` flag in bot's **[`RemoteCommunication`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#remotecommunication)** settings.

## GitHub

ASF communicates with **[GitHub's API](https://api.github.com)** in order to fetch **[ASF releases](https://github.com/JustArchiNET/ArchiSteamFarm/releases)** for the update functionality. This is done as part of auto-updates (if you've kept **[`UpdatePeriod`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#updateperiod)** enabled), as well as `update` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. You can influence ASF's communication with GitHub through **[`UpdateChannel`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#updatechannel)** property - setting it to `None` will result in disabling entire update functionality, including GitHub communication in this regard.

## ASF сервер

ASF communicates with **[our own server](https://asf.justarchi.net)** for more advanced functionality. In particular, this includes:
- Verifying checksums of ASF builds downloaded from GitHub against our own independent database to ensure that all downloaded builds are legitimate (free of malware, MITM attacks or other tampering)
- Announcing your bot in **[our listing](https://asf.justarchi.net/STM)** if you've enabled `SteamTradeMatcher` in **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#tradingpreferences)** and meet other criteria
- Downloading currently available bots to trade from **[our listing](https://asf.justarchi.net/STM)** if you've enabled `MatchActively` in **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#tradingpreferences)** and meet other criteria

As a security measure, it's not possible to disable checksum verification for ASF builds. However, you can disable auto-updates entirely if you'd like to avoid this, as described above in the GitHub section.

You can decide to opt-out of being announced in the listing by disabling `PublicListing` flag in bot's **[`RemoteCommunication`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#remotecommunication)** settings. This might be useful if you'd like to run `SteamTradeMatcher` bot without being announced at the same time.

Downloading bots from our listing is mandatory for `MatchActively` setting, you'll need to disable that setting if you're unwilling to accept that.

---

## Публичный список ASF STM

Наш публичный каталог ASF STM расположен на **[нашем сайте](https://asf.justarchi.net/STM)** и используется как публичная служба как для пользователей ASF, использующих `MatchActively`, так и для всех пользователей, независимо от использования ASF, для ручного сопоставления.

### Как именно это работает

ASF отправляет начальные данные один раз после входа, они содержат все свойства которые используются в публичном списке. После этого, каждые 10 минут ASF отправляет один очень маленький "heartbeat" запрос, который сообщает нашему серверу что бот всё ещё запущен. Если по какой-то причине "heartbeat" не был получен, например из-за проблем с сетью, ASF будет пытаться снова каждую минуту, пока сервер его не зарегистрирует.

Это позволяет нашему сайту отслеживать, какие аккаунты могут быть использованы для поиска дубликатов, и активны ли они. Благодаря этому, наш веб-сайт может отображать все аккаунты с ASF, 2ФА и STM, которые были активны **в течение последних 15 минут**.

Пользователи отсортированы согласно их инвентарю (по убыванию) - сначала боты с включенным `MatchEverything`, которые принимают любые обмены 1:1, со значком `Any`, затем по количеству уникальных игр из которых получены предметы, соответствующие `MatchableTypes`, и наконец по количеству предметов, соответствующих `MatchableTypes`.

Пожалуйста, обратите внимание, что вы **не** будете показаны на сайте если не соответствуете всем критериям. ASF даже не будет обмениваться с нашем сервером в этом случае, поэтому второй пункт будет неприменим к вам если вы намеренно не включили `SteamTradeMatcher` чтобы обменять свои дубликаты карточек. Также, публичный каталог совместим только с последней стабильной версией ASF, и может отказаться показывать ботов с устаревшей программой, особенно если у них отсутствует какая-нибудь базовая функциональность, доступная только в более новых версиях.

### API

Каталог ASF STM на данный момент отображает только ботов на основе ASF. У нас на данный момент нет возможности отображать сторонних ботов в нашем каталоге (поскольку мы не можем легко проверить их код и убедиться что их логика обменов соответствует нашей).

Если вы ищете удобный способ получить доступ к нашему каталогу из программы, у нас есть очень простая конечная точка **[/Api/Bots](https://asf.justarchi.net/Api/Bots)** как раз для этого случая. Именно эту конечную точку ASF использует для пользователей с включенным режимом `MatchActively`.

### Политика конфиденциальности

If you agree to being listed in our listing, by enabling `SteamTradeMatcher` and not refusing `PublicListing`, as specified above, we'll temporarily store some of your Steam account details on our server in order to provide the core functionality.

Public info (exposed by Steam to every interested party) includes:
- Ваш идентификатор Steam (в 64-разрядном формате, для создания ссылок)
- Ваш никнейм (для отображения)
- Ваш аватар (только хеш, для отображения)

Private info (selected data required for providing the functionality) includes:
- Ваш **[токен для обменов](https://steamcommunity.com/my/tradeoffers/privacy)** (чтобы люди, не состоящие в вашем списке друзей могли отправить вам обмен)
- Значение параметра `MatchableTypes` (для отображения и сопоставления)
- Общее число предметов Steam, соответствующих `MatchableTypes` типов в вашем инвентаре (для отображения и сопоставления)
- Общее число уникальных игр из которых указанные выше предметы Steam соответствующих `MatchableTypes` типов были получена (для отображения и сопоставления)
- Value of `MatchEverything` in your **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#tradingpreferences)** (for display purposes and matching)

ASF server will **not** collect, store or otherwise process any other data not listed above, without prior important notice in the changelog, and a very good practical reason in the first place. We do not consider anything above to be a serious matter, and we mention it to let you know what precisely ASF does apart of what you configured it to do yourself, so people can better understand the process.

Your data will be automatically hidden from general public in up to 15 minutes since the moment you stop using our listing, whether due to change of settings or not having ASF launched anymore. In addition to that, it'll be automatically deleted from our server (including all backup copies) in up to 7 days since the above happening.