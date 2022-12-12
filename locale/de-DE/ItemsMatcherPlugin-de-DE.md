# Note

This page is work-in-progress. For translators: you may want to wait a bit (until stable release), as the page is still being written and corrected.

---

# Plugin

`ItemsMatcherPlugin` is official ASF plugin that extends ASF with ASF STM listing features. In particular, this includes `PublicListing` in **[`RemoteCommunication`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#remotecommunication)** and `MatchActively` in **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#tradingpreferences)**.

---

## `PublicListing`

Unsere öffentliche ASF STM-Liste befindet sich auf **[unserer Website](https://asf-backend.justarchi.net/STM)** und wird als öffentlicher Dienst sowohl für ASF-Benutzer, die `MatchActively` nutzen, als auch für ASF- und Nicht-ASF-Benutzer für manuelles Matching verwendet.

Bitte beachte, dass du **nicht** auf der Webseite angezeigt wirst, wenn du nicht alle Anforderungen erfüllst. ASF won't even bother communicating with our server in this case, so this section is entirely skipped for you if you didn't intentionally enable `SteamTradeMatcher` in order to help yourself match dupes. Auch die öffentliche Auflistung ist nur mit der neuesten stabilen Version von ASF kompatibel und kann sich weigern, veraltete Bots anzuzeigen, insbesondere wenn ihnen Kernfunktionen fehlen, die nur in neueren Versionen zu finden sind.

### Wie es genau funktioniert

ASF sendet nach der Anmeldung einmalig erste Daten, die alle Eigenschaften enthalten, die von der öffentlichen Auflistung verwendet werden. Dann sendet ASF alle 10 Minuten eine sehr kleine "Heartbeat"-Anfrage, die unseren Server darüber informiert, dass der Bot noch aktiv ist. Wenn der "Heartbeat" aus irgendeinem Grund nicht ankam, z.B. aufgrund von Netzwerkproblemen, wird ASF versuchen, ihn jede Minute erneut zu senden bis der Server ihn registriert.

Auf diese Weise kann unsere Webseite erkennen, welche Konten für den Abgleich verwendet werden können und ob sie noch aktiv sind. Dank dieser Tatsache kann unsere Webseite alle ASF-2FA+STM Konten anzeigen die in den **letzten 15 Minuten** aktiv waren.

Die Benutzer werden nach ihren Beständen (in absteigender Reihenfolge) sortiert - `MatchEverything` Bots mit `Any` Banner, die alle 1:1 Trades akzeptieren, dann `MatchableTypes` unique games count, und schließlich `MatchableTypes` items count.

### API

ASF STM Inserate akzeptieren vorerst nur ASF-Bots. Es gibt vorerst keine Möglichkeit Drittanbieter-Bots in unsere Auflistung einzutragen (da wir ihren Quellcode nicht einfach überprüfen können und sicherstellen müssen, dass sie unserer gesamten Handelslogik entsprechen).

If you're looking for easy way to access our listing in programmatic way, we have a very simple **[`/Api/Listing/Bots`](https://asf-backend.justarchi.net/Api/Listing/Bots)** endpoint that you can use. Dies ist auch der Endpunkt, den ASF intern für `MatchActively` Benutzer verwendet.

### Privacy policy

If you agree to being listed in our listing, by enabling `SteamTradeMatcher` and not refusing `PublicListing`, as specified above, we'll temporarily store some of your Steam account details on our server in order to provide the core functionality.

Public info (exposed by Steam to every interested party) includes:
- Deine Steam-ID (in 64-Bit-Form, zur Generierung von Links)
- Dein Nickname (zu Anzeigezwecken)
- Dein Avatar (hash, zu Anzeigezwecken)

Private info (selected data required for providing the functionality) includes:
- Your **[inventory](https://steamcommunity.com/my/inventory/#753_6)** limited to item types that you've picked in `MatchableTypes` (so people can use `MatchActively` against your items).
- Dein **[Handels-Code](https://steamcommunity.com/my/tradeoffers/privacy)** (damit Leute außerhalb Ihrer Freundesliste IhrenHandelsangebote schicken können)
- Your `MaxTradeHoldDuration` (so other people know whether you're willing to accept their trades)
- Deine `MatchableTypes` (zum Anzeigen und Abgleichen)
- Total number of Steam items in your inventory (for display purposes and matching)

---

## `MatchActively`

`MatchActively` setting is active version of **[`SteamTradeMatcher`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading#steamtradematcher)** which includes interactive matching in which the bot will send trades to other people. It can work standalone, or together with `SteamTradeMatcher` setting. This feature requires `LicenseID` to be set, as it uses third-party server.

Um von dieser Option Gebrauch zu machen musst du eine Reihe von Anforderungen erfüllen. At the minimum you must have **[unrestricted](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663)** account, **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication#asf-2fa)** active and at least one valid type in `MatchableTypes`, such as trading cards.

Wenn du alle oben genannten Anforderungen erfüllst, wird ASF regelmäßig mit unserer **[öffentlichen ASF STM Liste](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Remote-communication#public-asf-stm-listing)** kommunizieren um aktiv mit aktuell verfügbaren Bots abzugleichen.

- In jeder Runde holt ASF unser Inventar und das Inventar der ausgewählten Bots, die aufgelistet sind, um `MatchableTypes` Gegenstände zu finden, die zugeordnet werden können. Wenn die passende Übereinstimmung gefunden wird, sendet und bestätigt ASF das Handelsangebot automatisch.
- Jedes Set (bestehend aus appID, Typ und Seltenheit eines Gegenstands) kann pro Runde nur ein einziges mal gepaart werden. Dies ist implementiert, um "nicht mehr verfügbare Artikel" zu minimieren und zu vermeiden, dass man warten muss, bis jeder Bot reagiert bevor man alle Handelsangebote versendet. Das ist auch der primäre Grund, wieso der Vorgang in Runden und nicht als ein durchgehender Prozess ausgeführt wird.
- ASF sendet nicht mehr als `255` Gegenstände in einem einzigen Handelsangebot, und nicht mehr als `5` Handelsangebote an einen einzelnen Benutzer in einer einzigen Runde. Dies wird durch Begrenzungen von Steam sowie durch unsere eigene Load-Balancing-Funktion vorgegeben.

Dieses Modul soll transparent sein. Matching will start in approximately `1` hour since ASF start, and will repeat itself each `6` hours (if needed). Die Funktion `MatchActively` ist als langfristige, periodische Maßnahme gedacht, um sicherzustellen, dass wir aktiv auf dem Weg zur Fertigstellung von Sets sind, aber ohne einen kurzfristigen Zeit- und Ressourcendruck, der auftreten würde, wenn dies als Befehl angeboten werden würde. The target users of this module are primary accounts and "stash" alt accounts, although it can be used by any bot that is not set to `MatchEverything`.

ASF does its best to minimize the amount of requests and pressure generated by using this option, while at the same time maximizing efficiency of matching to the upper limit. The exact algorithm of choosing the bots to match and otherwise organize the whole process, is ASF's implementation detail and can change in regards to feedback, situation and possible future ideas.

The current version of the algorithm makes ASF prioritize `Any` bots first, especially those with better diversity of games that their items are from. When running out of `Any` bots, ASF will move on to the fair ones upon same diversity rule, with those owning excessive number of items further deprioritized due to higher chance of possible inventory-related problems compared to other bots. Regardless of that, ASF will try to match every available bot at least once, to ensure that we're not missing on a possible set progress.

`MatchActively` takes into account bots that you blacklisted from trading through `tbadd` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** and will not attempt to actively match them. Dies kann verwendet werden, um ASF mitzuteilen, welche Bots nie verglichen werden sollen, auch wenn sie potenzielle Duplikate haben die wir verwenden können.

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