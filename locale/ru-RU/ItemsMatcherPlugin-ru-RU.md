# Note

This page is work-in-progress. For translators: you may want to wait a bit (until stable release), as the page is still being written and corrected.

---

# Plugin

`ItemsMatcherPlugin` is official ASF plugin that extends ASF with ASF STM listing features. In particular, this includes `PublicListing` in **[`RemoteCommunication`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#remotecommunication)** and `MatchActively` in **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#tradingpreferences)**.

---

## `PublicListing`

Наш публичный каталог ASF STM расположен на **[нашем сайте](https://asf-backend.justarchi.net/STM)** и используется как публичная служба как для пользователей ASF, использующих `MatchActively`, так и для всех пользователей, независимо от использования ASF, для ручного сопоставления.

Пожалуйста, обратите внимание, что вы **не** будете показаны на сайте если не соответствуете всем критериям. ASF won't even bother communicating with our server in this case, so this section is entirely skipped for you if you didn't intentionally enable `SteamTradeMatcher` in order to help yourself match dupes. Также, публичный каталог совместим только с последней стабильной версией ASF, и может отказаться показывать ботов с устаревшей программой, особенно если у них отсутствует какая-нибудь базовая функциональность, доступная только в более новых версиях.

### Как именно это работает

ASF отправляет начальные данные один раз после входа, они содержат все свойства которые используются в публичном списке. После этого, каждые 10 минут ASF отправляет один очень маленький "heartbeat" запрос, который сообщает нашему серверу что бот всё ещё запущен. Если по какой-то причине "heartbeat" не был получен, например из-за проблем с сетью, ASF будет пытаться снова каждую минуту, пока сервер его не зарегистрирует.

Это позволяет нашему сайту отслеживать, какие аккаунты могут быть использованы для поиска дубликатов, и активны ли они. Благодаря этому, наш веб-сайт может отображать все аккаунты с ASF, 2ФА и STM, которые были активны **в течение последних 15 минут**.

Пользователи отсортированы согласно их инвентарю (по убыванию) - сначала боты с включенным `MatchEverything`, которые принимают любые обмены 1:1, со значком `Any`, затем по количеству уникальных игр из которых получены предметы, соответствующие `MatchableTypes`, и наконец по количеству предметов, соответствующих `MatchableTypes`.

### API

Каталог ASF STM на данный момент отображает только ботов на основе ASF. У нас на данный момент нет возможности отображать сторонних ботов в нашем каталоге (поскольку мы не можем легко проверить их код и убедиться что их логика обменов соответствует нашей).

If you're looking for easy way to access our listing in programmatic way, we have a very simple **[`/Api/Listing/Bots`](https://asf-backend.justarchi.net/Api/Listing/Bots)** endpoint that you can use. Именно эту конечную точку ASF использует для пользователей с включенным режимом `MatchActively`.

### Политика конфиденциальности

If you agree to being listed in our listing, by enabling `SteamTradeMatcher` and not refusing `PublicListing`, as specified above, we'll temporarily store some of your Steam account details on our server in order to provide the core functionality.

Public info (exposed by Steam to every interested party) includes:
- Ваш идентификатор Steam (в 64-разрядном формате, для создания ссылок)
- Ваш никнейм (для отображения)
- Ваш аватар (только хеш, для отображения)

Private info (selected data required for providing the functionality) includes:
- Your **[inventory](https://steamcommunity.com/my/inventory/#753_6)** limited to item types that you've picked in `MatchableTypes` (so people can use `MatchActively` against your items).
- Ваш **[токен для обменов](https://steamcommunity.com/my/tradeoffers/privacy)** (чтобы люди, не состоящие в вашем списке друзей могли отправить вам обмен)
- Your `MaxTradeHoldDuration` (so other people know whether you're willing to accept their trades)
- Значение параметра `MatchableTypes` (для отображения и сопоставления)
- Total number of Steam items in your inventory (for display purposes and matching)

---

## `MatchActively`

`MatchActively` setting is active version of **[`SteamTradeMatcher`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading#steamtradematcher)** which includes interactive matching in which the bot will send trades to other people. Он может работать автономно или вместе с настройкой `SteamTradeMatcher`. This feature requires `LicenseID` to be set, as it uses third-party server.

Чтобы использовать данный режим, вам необходимо соответствовать ряду требований. At the minimum you must have **[unrestricted](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663)** account, **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication#asf-2fa)** active and at least one valid type in `MatchableTypes`, such as trading cards.

If you meet all of the requirements above, ASF will periodically communicate with our **[public ASF STM listing](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Remote-communication#public-asf-stm-listing)** in order to actively match bots that are currently available.

- Каждый раунд ASF будет проверять наш инвентарь и инвентарь выбранных ботов, которые находятся в списке, чтобы найти предметы `MatchableTypes` которые можно сопоставить. Если совпадение найдено, ASF отправит и подтвердит предложение обмена автоматически.
- Каждый набор (сочетание appID, типа предмета и его редкости) может быть сопоставлен в одном раунде только один раз. Это реализовано чтобы минимизировать ситуацию "Предметы более недоступны" и избежать необходимости ждать реакции каждого бота, прежде чем отправить все обмены. Именно по этой причине сеанс сопоставления состоит из раундов, а не из одного непрерывного процесса.
- ASF не будет отправлять более `255` предметов в одном предложении обмена, и не будет отправлять более `5` предложений обмена одному пользователю в одном раунде. Это связано с ограничениями Steam, а также с нашей собственной балансировкой нагрузки.

Работа этого модуля должна быть полностью прозрачной. Сопоставление начинается примерно через `1` час с момента запуска ASF, и будет повторяться каждые `6`часов (при необходимости). Функция `MatchActively` предназначена для длительного циклического использования с целью завершения наборов карточек, с целью избежать повышенной нагрузки, которая могла бы наблюдаться если бы этот функционал был доступен как команда. Целевые пользователи данного модуля это основные аккаунты и дополнительные аккаунты-"склады", однако его можно использовать для любого бота, у которого не включен режим `MatchEverything`.

ASF будет делать все возможное чтобы сократить количество запросов и иную нагрузку, возникающие из-за использования этого функционала, в тоже время максимально увеличивая эффективность сопоставления. Точный алгоритм, по которому выбираются боты для сопоставления и прочая организация процесса - это детали реализации ASF и могут меняться в соответствии с полученной обратной связью, ситуацией и возможными будущими идеями.

Согласно текущей версии алгоритма, ASF сначала обрабатывает ботов с параметром `Any`, в первую очередь тех, которые предоставляют большее разнообразие игр, из которых у них есть карточки. Когда боты с параметром `Any` заканчиваются, ASF переходит к ботам, поддерживающим только честные обмены с учётом того же правила в отношении разнообразия игр, при этом боты с большим количеством предметов имеют пониженный приоритет из-за вероятных проблем с обработкой инвентаря таких ботов в сравнении с другими ботами. Независимо от этого, ASF попытается сопоставить карточки с каждым ботом хотя бы один раз, чтобы обеспечить что мы не пропустим возможность продвинутся в собирании набора.

`MatchActively` учитывает ботов, которых вы внесли в черный список для торговли через `tbadd` **[ команду](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** и не будет пытаться активно сопоставлять их. Это можно использовать чтобы указать ASF ботов, с которыми никогда не следует сопоставлять наборов, даже если они имеют потенциальные дубликаты которые можно использовать.

---

### Why do I need a `LicenseID` to use the plugin? Wasn't `MatchActively` free before?

ASF is, and remains, free and open-source, as it was established at the start of the project back in October 2015. Our program is also entirely non-commercial, we do not earn anything from contributions to it, building or publishing. Over those past 7+ years ASF has received tremendous amount of development, and it's still being improved and enhanced with every monthly stable release mostly by a single person, **[JustArchi](https://github.com/JustArchi)** - with no strings attached. The only funding we receive is from non-obligatory donations that come from our users.

For a very long time, until October 2022, `MatchActively` feature was part of ASF core and available for everyone to use. In October 2022, Valve, the company behind Steam, has put very severe rate limits that rendered previous functionality entirely broken, with no solution available. The feature therefore has been removed from ASF core in version 5.4.1.0.

`MatchActively` was resurrected as part of official `ItemsMatcher` plugin that further enhances ASF with active cards matching functionality. Resurrecting `MatchActively` feature required from us extraordinary amount of work to create ASF backend, entirely new service hosted on a server, with more than a thousand of proxies attached for resolving inventories, all exclusively to allow ASF clients to make use of `MatchActively` like before. Due to the amount of work involved, as well as resources that are not free and require to be paid on monthly basis by us (domain, server, proxies), we've decided to offer this plugin to our sponsors, that is, people that already support ASF project on monthly basis. Our goal isn't to profit from it, but rather, cover the **monthly costs** that are exclusively linked with offering this functionality - that's why we offer it basically for nothing, but we do have to charge a little for it as we can't pay hundreds of dollars from our own pockets just to make it available for you. We hope that you understand.

---

### How can I get an access?

`ItemsMatcher` is offered as part of $5+ sponsor tier on **[JustArchi's GitHub](https://github.com/sponsors/JustArchi)**. Simply become a sponsor of $5 tier (or higher), then click **[here](https://asf-backend.justarchi.net/user/status)** to obtain your `LicenseID`. You'll need to sign in with GitHub for confirming your identity.

The license allows you to send limited amount of requests to the server. $5 tier allows you to use `MatchActively` for one account, which should be suitable for majority of people. $10 tier allows you to use it on three accounts. If you require more resources, **[let us know](mailto:ASF@JustArchi.net)**.

`LicenseID` is made out of 32 hexadecimal characters, such as `f6a0529813f74d119982eb4fe43a9a24`. Simply put it in `LicenseID` property of ASF global config.