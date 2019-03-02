# Statistiken

Die ASF-Entwicklung wird durch 3 wichtige Dinge unterstützt: Spenden, Benutzer-Rückmeldungen und Statistiken. Spenden beeinflussen direkt unsere Bereitschaft an dem Projekt zu arbeiten, die Rückmeldungen der Benutzer sind immer schön zu lesen (besonders positive), und Statistiken liefern uns die Informationen, wie und von wie vielen Personen unsere Software verwendet wird - auf diese Weise können wir erfahren, was wir verbessern, was wir beheben und worauf wir uns konzentrieren können.

ASF hat in den Standardeinstellungen `Statistics` globale Konfigurationseigenschaft aktiviert. Wenn du neue Versionen, Fehlerbehebungen und die Implementierung neuer Funktionen sehen möchtest, solltest du in Betracht ziehen, diese Einstellung beizubehalten, damit wir diese Daten nutzen können, um dir eine bessere Software (aber nicht nur) zu bieten. Dies ist besonders wichtig da ASF dir **kostenlos** zur Verfügung gestellt wird, und das ist das Mindeste, was du tun kannst um Danke zu sagen - **teil uns mit, dass du ASF benutzt**, da dies das ist, was unsere aktuellen Statistiken hauptsächlich tun. ASF does not collect or make use of any data that could be considered private and/or used against you. We keep the usage of statistics to bare minimum, and every single information being collected requires from us to precisely state it here, together with practical explanation. This gives you full insight into what we collect, for what purpose, and how it's supposed to help. All of that info can be found further below.

* * *

## Aktuelle Datenschutzerklärung

Wenn `Statistics` aktiv ist passiert Folgendes:

1. Account being used in ASF will join our **[Steam group](https://steamcommunity.com/gid/103582791440160998)** upon login. Dies geschieht aus drei Gründen:

* Es bietet **dir** Gruppenankündigungen, insbesondere bei neuen Versionen, kritischen Problemen, Steam-Problemen und andere Dingen, die wichtig sind, um die Community auf dem Laufenden zu halten (kein Spam, der nichts mit ASF zu tun hat)
* Es ermöglicht **dir**, unsere technische Unterstützung in Anspruch zu nehmen, indem du Fragen stellst, Probleme löst, Probleme meldest oder Verbesserungsvorschläge machst
* Es ermöglicht uns zu sehen wie viele Steam-Konten tatsächliche von ASF verwendet werden

We consider Steam group as a crucial part in regards to ASF community. This is our main communication channel which we use for all important matters in regards to ASF, especially keeping you up with the development, potential issues, eventual warnings and all other matters that you should have access to as an user. We do not benefit from maintenance of that group in any way, it's the place dedicated to ASF users and we consider you part of our community. Since membership of the group in no way identifies you as ASF user, we do not consider this to be a problem in terms of privacy.

2. If your account is **[unrestricted](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663)**, using **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication#asf-2fa)**, has **[public inventory](https://steamcommunity.com/my/edit/settings)** with at least 100 `MatchableTypes` items in it and you intentionally enabled `SteamTradeMatcher` in your `TradingPreferences`, then ASF will periodically communicate with our **[server](https://asf.justarchi.net)** in order to fulfill the enabled functionality. Tatsächliche Daten bestehen aus einer eindeutigen ASF-ID (die von ASF generiert wird) und folgenden kontobezogenen Informationen:

* Deine Steam-ID (in 64-Bit-Form, zur Generierung von Links)
* Dein Nickname (zu Anzeigezwecken)
* Dein Avatar (hash, zu Anzeigezwecken)
* Dein **[Handels-Code](https://steamcommunity.com/my/tradeoffers/privacy)** (damit Leute außerhalb deiner Freundesliste dir Handelsangebote schicken können)
* Deine `MatchableTypes` (zum Anzeigen und Abgleichen)
* Value of `MatchEverything` in your `TradingPreferences` (for display purposes and matching)
* Total number of `MatchableTypes` Steam items in your inventory (for display purposes and matching)
* Total number of unique games that above `MatchableTypes` Steam items are made of (for display purposes and matching)

ASF wird **keine** weiteren Daten (außer die aufgezählten) ohne vorherige wichtige Mitteilung im Änderungsprotokoll und aus einem sehr guten praktischen Grund sammeln. We do not consider anything above to be a serious matter, and we mention it to let you know what precisely ASF does apart of what you configured it to do yourself, so people can better understand our point of view.

* * *

# Nutzung der Daten

All values specified in second point are being used for our **Public ASF STM listing**, which is explained below. We do not use any other data for any other purpose.

* * *

## Öffentliches ASF STM Inserat

Our public ASF STM listing is located on **[our website](https://asf.justarchi.net/STM)** and used as a public service for both ASF users that make use of `MatchActively`, as well as ASF and non-ASF users for manual matching.

### Wie es genau funktioniert

ASF sendet nach der Anmeldung einmalig erste Daten, die alle Eigenschaften enthalten, die von der öffentlichen Auflistung verwendet werden. Dann sendet ASF alle 10 Minuten eine sehr kleine "Heartbeat"-Anfrage, die unseren Server darüber informiert, dass der Bot noch aktiv ist. Wenn der "Heartbeat" aus irgendeinem Grund nicht ankam, z.B. aufgrund von Netzwerkproblemen, wird ASF versuchen, ihn jede Minute erneut zu senden bis der Server ihn registriert.

Auf diese Weise kann unsere Webseite erkennen, welche Konten für den Abgleich verwendet werden können und ob sie noch aktiv sind. Dank dieser Tatsache kann unsere Webseite alle ASF 2FA+STM Konten anzeigen die in den **letzten 15 Minuten** aktiv waren.

Users are sorted according to their inventories (in descending order) - `MatchEverything` bots with `Any` banner that accept all 1:1 trades, then `MatchableTypes` unique games count, and finally `MatchableTypes` items count.

Bitte beachte, dass du **nicht** auf der Webseite angezeigt wirst, wenn du nicht alle Anforderungen erfüllst. ASF won't even bother communicating with our server in this case, so second point is entirely skipped for you if you didn't intentionally enable `SteamTradeMatcher` in order to help yourself match dupes. Auch die öffentliche Auflistung ist nur mit der neuesten stabilen Version von ASF kompatibel und kann sich weigern, veraltete Bots anzuzeigen, insbesondere wenn ihnen Kernfunktionen fehlen, die nur in neueren Versionen zu finden sind.

### API

ASF STM Inserate akzeptieren vorerst nur ASF-Bots. Es gibt vorerst keine Möglichkeit Drittanbieter-Bots in unsere Auflistung einzutragen (da wir ihren Quellcode nicht einfach überprüfen können und sicherstellen müssen, dass sie unserer gesamten Handelslogik entsprechen).

Wenn du nach einem einfachen Weg suchst, um programmgesteuert auf unser Angebot zuzugreifen, haben wir einen sehr einfachen **[/Api/Bots](https://asf.justarchi.net/Api/Bots)** Endpunkt, den du verwenden kannst. Dies ist auch der Endpunkt, den ASF intern für `MatchActively` Benutzer verwendet.

### Bemerkung

*Das gesamte Konzept, zusammen mit der Webseitenintegration und dem ASF-Reporting befindet sich noch in der Beta-Phase - es kann im Laufe der Zeit verbessert/geändert werden - auch dann, wenn wir das Gefühl haben, dass es nicht genügend Interesse an diesem Feature gibt.*

* * *

## Zustimmung verweigern

Die Teilnahme an Statistiken ist **nicht obligatorisch**, obwohl sie für die Zukunft des Programms sehr empfohlen wird. Wir verurteilen dich nicht und wenn du den inneren Drang verspürst, die Tatsache zu verbergen, dass du ASF-Benutzer bist, dann kannst du die Statistik **vollständig** deaktivieren, indem du die globale Konfigurationseigenschaft `Statistics` auf `false` stellst. Deaktivierte Statistiken machen das gesamte Modul unbrauchbar und führen keine der in unserer Datenschutzerklärung oben genannten Aktionen aus.

Disabling statistics might influence **our technical support, ASF functionality and other things that are offered to you for free**. For example, you can't make use of `SteamTradeMatcher` or `MatchActively` without having `Statistics` enabled.