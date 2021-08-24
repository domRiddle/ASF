# Statistiken

Die ASF-Entwicklung wird durch 3 wichtige Dinge unterstützt: Spenden, Benutzer-Rückmeldungen und Statistiken. Spenden beeinflussen direkt unsere Bereitschaft an dem Projekt zu arbeiten, die Rückmeldungen der Benutzer sind immer schön zu lesen (besonders positive), und Statistiken liefern uns die Informationen, wie und von wie vielen Personen unsere Software verwendet wird - auf diese Weise können wir erfahren, was wir verbessern, was wir beheben und worauf wir uns konzentrieren können.

ASF hat in den Standardeinstellungen `Statistics` globale Konfigurationseigenschaft aktiviert. Wenn du neue Versionen, Fehlerbehebungen und die Implementierung neuer Funktionen sehen möchtest, solltest du in Betracht ziehen, diese Einstellung beizubehalten, damit wir diese Daten nutzen können, um Ihreneine bessere Software (aber nicht nur) zu bieten. Dies ist besonders wichtig da ASF Ihren**kostenlos** zur Verfügung gestellt wird, und das ist das Mindeste, was du tun kannst um Danke zu sagen - **teil uns mit, dass du ASF benutzt**, da dies das ist, was unsere aktuellen Statistiken hauptsächlich tun. ASF sammelt oder verwendet keine Daten, die als privat angesehen und/oder gegen dich verwendet werden könnten. Wir reduzieren die Verwendung von Statistiken auf ein Minimum und jede einzelne gesammelte Information erfordert von uns, dass wir sie hier genau angeben, zusammen mit einer praxisnahen Erklärung. So erhältst du einen vollständigen Überblick darüber, was wir sammeln, zu welchem Zweck und wie es helfen soll. Alle diese Informationen findest du weiter unten.

---

## Aktuelle Datenschutzerklärung

Wenn `Statistics` aktiv ist passiert Folgendes:

1. Das in ASF verwendete Konto wird unserer **[Steam-Gruppe](https://steamcommunity.com/gid/103582791440160998)** beitreten. Dies geschieht aus drei Gründen:

* Es bietet **dir** Gruppenankündigungen, insbesondere bei neuen Versionen, kritischen Problemen, Steam-Problemen und andere Dingen, die wichtig sind, um die Community auf dem Laufenden zu halten (kein Spam, der nichts mit ASF zu tun hat)
* Es ermöglicht **dir**, unsere technische Unterstützung in Anspruch zu nehmen, indem du Fragen stellst, Probleme löst, Probleme meldest oder Verbesserungsvorschläge machst
* Es ermöglicht uns zu sehen wie viele Steam-Konten tatsächliche von ASF verwendet werden

Wir betrachten die Steam-Gruppe als einen entscheidenden Teil der ASF-Community. Dies ist unser wichtigster Kommunikationskanal, den wir für alle wichtigen Angelegenheiten im Zusammenhang mit ASF nutzen, insbesondere um Sie über die Entwicklung, mögliche Probleme, eventuelle Warnungen und alle anderen Angelegenheiten auf dem Laufenden zu halten, auf die Sie als Benutzer Zugriff haben sollten. Wir profitieren in keiner Weise von der Pflege dieser Gruppe, es ist der Ort, der ASF-Anwendern gewidmet ist, und wir betrachten Sie als Teil unserer Community. Da die Mitgliedschaft in der Gruppe Sie in keiner Weise als ASF-Benutzer identifiziert, betrachten wir dies nicht als ein Problem in Bezug auf die Privatsphäre.

2. If your account is **[unrestricted](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663)**, using **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication#asf-2fa)**, has **[public inventory](https://steamcommunity.com/my/edit/settings)** with at least 100 `MatchableTypes` items in it and you intentionally enabled `SteamTradeMatcher` in your `TradingPreferences`, then ASF will periodically communicate with our **[server](https://asf.justarchi.net)** in order to fulfill the enabled functionality. Tatsächliche Daten bestehen aus einer eindeutigen ASF-ID (die von ASF generiert wird) und folgenden kontobezogenen Informationen:

* Ihr Steam Identifikator (in 64-Bit-Format, um Links zu generieren, öffentliche Informationen)
* Ihr Nickname (für Anzeigezwecke, öffentliche Informationen)
* Ihr Profilbild (für Anzeigezwecke, öffentliche Informationen)
* Your **[trading token](https://steamcommunity.com/my/tradeoffers/privacy)** (so people outside of your friendlist can send you trades)
* Deine `MatchableTypes` (zum Anzeigen und Abgleichen)
* Wert von `MatchEverything` in Ihren `TradingPreferences` (für Anzeigezwecke und Zusammenführung)
* Gesamtzahl der `MatchableTypes` Steam-Gegenständen in Ihrem Inventar (für Anzeigezwecke und Zusammenführung)
* Gesamtzahl der einzigartigen Spiele, die über `MatchableTypes` Steam Gegenstände bestehen (für Anzeigezwecke und Zusammenführung)

ASF will **not** collect any other non-listed-above data without prior important notice in the changelog, and a very good practical reason in the first place. Wir erachten das Obige nicht als eine ernste Angelegenheit, und wir erwähnen es, um dich wissen zu lassen, was genau ASF neben dem, was du selbst konfiguriert hast, macht, damit die Leute unseren Standpunkt besser verstehen können.

---

# Nutzung der Daten

All values specified in second point are being used for our **Public ASF STM listing**, which is explained below. Wir verwenden keine weiteren Daten für andere Zwecke.

---

## Öffentliches ASF-STM Inserat

Our public ASF STM listing is located on **[our website](https://asf.justarchi.net/STM)** and used as a public service for both ASF users that make use of `MatchActively`, as well as ASF and non-ASF users for manual matching.

### Wie es genau funktioniert

ASF sendet nach der Anmeldung einmalig erste Daten, die alle Eigenschaften enthalten, die von der öffentlichen Auflistung verwendet werden. Dann sendet ASF alle 10 Minuten eine sehr kleine "Heartbeat"-Anfrage, die unseren Server darüber informiert, dass der Bot noch aktiv ist. Wenn der "Heartbeat" aus irgenIhrem Grund nicht ankam, z. B. aufgrund von Netzwerkproblemen, wird ASF versuchen, ihn jede Minute erneut zu senden bis der Server ihn registriert.

Auf diese Weise kann unsere Webseite erkennen, welche Konten für den Abgleich verwendet werden können und ob sie noch aktiv sind. Thanks to that, our website can show all ASF 2FA+STM accounts that were active in **last 15 minutes**.

Die Benutzer werden nach ihren Beständen (in absteigender Reihenfolge) sortiert - `MatchEverything` Bots mit `Any` Banner, die alle 1:1 Trades akzeptieren, dann `MatchableTypes` unique games count, und schließlich `MatchableTypes` items count.

Please note that you will **not** be displayed on the website if you do not meet all of the requirements. ASF wird sich in diesem Fall nicht einmal die Mühe machen, mit unserem Server zu kommunizieren, sodass der zweite Punkt für Sie komplett übersprungen wird, wenn Sie `SteamTradeMatcher` nicht absichtlich aktiviert haben, um sich selbst zu helfen, Duplikate zu finden. Auch die öffentliche Auflistung ist nur mit der neuesten stabilen Version von ASF kompatibel und kann sich weigern, veraltete Bots anzuzeigen, insbesondere wenn ihnen Kernfunktionen fehlen, die nur in neueren Versionen zu finden sind.

### API

ASF-STM Inserate akzeptieren vorerst nur ASF-Bots. Es gibt vorerst keine Möglichkeit Drittanbieter-Bots in unsere Auflistung einzutragen (da wir Ihren Quellcode nicht einfach überprüfen können und sicherstellen müssen, dass sie unserer gesamten Handelslogik entsprechen).

Wenn du nach einem einfachen Weg suchst, um programmgesteuert auf unser Angebot zuzugreifen, haben wir einen sehr einfachen **[/Api/Bots](https://asf.justarchi.net/Api/Bots)** Endpunkt, den du verwenden kannst. Dies ist auch der Endpunkt, den ASF intern für `MatchActively` Benutzer verwendet.

### Bemerkung

*Das gesamte Konzept, zusammen mit der Webseitenintegration und dem ASF-Reporting befindet sich noch in der Beta-Phase - es kann im Laufe der Zeit verbessert/geändert werden - auch dann, wenn wir das Gefühl haben, dass es nicht genügend Interesse an diesem Feature gibt.*

---

## Zustimmung verweigern

Die Teilnahme an Statistiken ist **nicht obligatorisch**, obwohl sie für die Zukunft des Programms sehr empfohlen wird. Wir verurteilen dich nicht und wenn du den inneren Drang verspürst, die Tatsache zu verbergen, dass du ASF-Benutzer bist, dann kannst du die Statistik **vollständig** deaktivieren, indem du die globale Konfigurationseigenschaft `Statistics` auf `false` stellst. Deaktivierte Statistiken machen das gesamte Modul unbrauchbar und führen keine der in unserer Datenschutzerklärung oben genannten Aktionen aus.

Das Deaktivieren von Statistiken könnte **unseren technischen Support, ASF-Funktionalität und andere Dinge beeinflussen, die Ihnen kostenlos angeboten werden**. Zum Beispiel können Sie `MatchActively` nicht verwenden, ohne `Statistik` aktiviert zu haben (da Sie sich weigern, mit unserem Server zu kommunizieren, damit die Funktionalität funktioniert).