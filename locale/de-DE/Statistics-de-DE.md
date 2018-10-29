# Statistiken

Die ASF-Entwicklung wird durch 3 wichtige Dinge unterstützt: Spenden, Benutzer-Feedback und Statistiken. Spenden beeinflussen direkt unsere Bereitschaft an dem Projekt zu arbeiten, das Feedback der Benutzer ist immer schön zu lesen (besonders positives), und Statistiken liefern uns die Informationen, wie und von wie vielen Personen unsere Software verwendet wird - auf diese Weise können wir erfahren, was wir verbessern, was wir beheben und worauf wir uns konzentrieren können.

ASF hat in den Standardeinstellungen `Statistics` globale Konfigurationseigenschaft aktiviert. Wenn du neue Versionen, Fehlerbehebungen und die Implementierung neuer Funktionen sehen möchtest, solltest du in Betracht ziehen, diese Einstellung beizubehalten, damit wir diese Daten nutzen können, um dir eine bessere Software (aber nicht nur) zu bieten. Dies ist besonders wichtig da ASF dir **kostenlos** zur Verfügung gestellt wird, und das ist das Mindeste, was du tun kannst, um Danke zu sagen - **teil uns mit, dass du ASF benutzt**, da dies das ist, was unsere aktuellen Statistiken hauptsächlich tun.

Wir halten die Verwendung von Statistiken auf ein Minimum beschränkt, und jede einzelne gesammelte Information bedarf einer praktischen Erklärung - was wir sammeln, zu welchem Zweck und wie sie helfen soll. All das findest du unten.

* * *

# Aktuelle Datenschutzerklärung

Wenn `Statistics` aktiv sind, wird folgendes passieren:

a) Jeder in ASF benutzter Account wird unserer **[Steam Gruppe](https://steamcommunity.com/groups/ascfarm)** beitreten. Dies geschieht aus drei Gründen:

* Es bietet **dir** Gruppenankündigungen, insbesondere neue Versionen, kritische Probleme, Steam-Probleme und andere Dinge, die wichtig sind, um die Community auf dem Laufenden zu halten (kein Spam, der nichts mit ASF zu tun hat)
* Es ermöglicht **dir** unseren technischen Support zu nutzen, indem du Fragen stellst, Probleme löst, Probleme meldest oder Verbesserungsvorschläge machst
* Es ermöglicht uns zu sehen, wie viele Steam-Accounts tatsächliche von ASF verwendet werden

b) Wenn dein Konto **[uneingeschränkt](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663)** ist, unter Verwendung von **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication#asf-2fa)**, dein **[öffentliches Inventar](https://steamcommunity.com/my/edit/settings)** mit mindestens 100 `MatchableTypes` Elementen darin hat und `SteamTradeMatcher` in `TradingPreferences` beinhaltet, dann wird ASF regelmäßig mit unserem **[Server](https://asf.justarchi.net)** kommunizieren. Tatsächliche Daten bestehen aus einer eindeutigen ASF-ID (die von ASF generiert wird) und folgenden kontobezogenen Informationen:

* Deine Steam-ID (in 64-Bit-Form, zur Generierung von Links)
* Dein Nickname (zu Anzeigezwecken)
* Dein Avatar (hash, zu Anzeigezwecken)
* Dein **[Handel-Code](https://steamcommunity.com/my/tradeoffers/privacy)** (damit Leute außerhalb deiner Freundesliste dir Handelsangebote schicken können)
* Deine `MatchableTypes` (zu Anzeigezwecken)
* Wert von `MatchEverything` in deinen `TradingPreferences` (für Anzeigezwecke und Sortierung)
* Gesamtzahl der `MatchableTypes` Steam-Gegenständen in deinem Inventar (für Anzeigezwecke und Sortierung)
* Gesamtzahl der einzigartigen Spiele, die über `MatchableTypes` Steam Gegenstände bestehen (für Anzeigezwecke und Sortierung)

ASF wird **keine** weiteren Daten (außer die aufgezählten) ohne vorherige wichtige Mitteilung im Änderungsprotokoll und aus einem sehr guten praktischen Grund sammeln. Wir halten alles oben genannte nicht für gravierend genug, da die Steam-Gruppe in keiner Weise das Konto der Verwendung des ASF-Programms identifiziert oder nicht, während unsere öffentliche Auflistung nur aktiviert wird, wenn du absichtlich `SteamTradeMatcher` Funktion aktivierst, also wenn du tatsächlich **willst**, dass die Leute davon erfahren und dir überhaupt Handelsangebote schicken.

* * *

# Nutzung der Daten

Alle in Punkt b) angegebenen Werte werden für unser **Öffentliches ASF STM Inserat** verwendet, das nachfolgend erläutert wird, und nur für diesen Zweck.

* * *

## Öffentliches ASF STM Inserat

Unser öffentliches ASF STM-Inserat befindet sich **[hier](https://asf.justarchi.net/STM)** und dient einem sehr einfachen Zweck, nämlich allen Benutzern zu ermöglichen, ASF-Bots schnell auf Duplikate abzugleichen.

Dank unserer Auflistung kann jeder interessierte ASF- und Nicht-ASF-Benutzer leicht Bots, die derzeit aktiv sind, erkennen und ihnen ein STM-Handelsangebot schicken, was beiden Benutzern, **auch dir**, hilft, doppelte Karten loszuwerden und dich weiter auf den Weg zur Abzeichenvollständigung zu begeben. Wir wollten so etwas schon lange kreieren, denn **jeder** schätzt die sofortige Reaktion auf Handelsangebote, die ASF beinhaltet, was die Effizienz des Abgleichs drastisch verbessern kann, sowie Informationen über die Verfügbarkeit von Bots - bis jetzt war es sehr schwierig, ein solches öffentliches Inserat zu erstellen, und dank ASF ist es viel einfacher.

### Wie es genau funktioniert

ASF sendet nach der Anmeldung einmalig erste Daten, die alle Eigenschaften enthalten, die von der öffentlichen Auflistung verwendet werden. Dann sendet ASF alle 10 Minuten eine sehr kleine "Heartbeat"-Anfrage, die unseren Server darüber informiert, dass der Bot noch aktiv ist. Wenn der "Heartbeat" aus irgendeinem Grund nicht ankam, z.B. aufgrund von Netzwerkproblemen, wird ASF versuchen, ihn jede Minute erneut zu senden, bis der Server ihn registriert.

Auf diese Weise kann unsere Webseite erfassen, welche Konten für den Abgleich verwendet werden können und ob sie noch aktiv sind. Dank dieser Tatsache kann unsere Webseite alle ASF 2FA+STM Konten anzeigen, die in den **letzten 15 Minuten** aktiv waren.

Die Benutzer werden nach ihren Inventaren (in absteigender Reihenfolge) sortiert - `MatchableTypes` einzigartige Anzahl von Spielen, dann `MatchableTypes` Anzahl der Gegenstände, mit Zusatz von `MatchEverything` Bots, die oben mit `Any` Banner aufgeführt sind.

Bitte beachte, dass du **nicht** auf der Webseite angezeigt wirst, wenn du nicht alle Anforderungen erfüllst. ASF wird sich in diesem Fall nicht einmal die Mühe machen, mit unserem Server zu kommunizieren, daher wird Punkt b) für dich komplett übersprungen, wenn du `SteamTradeMatcher` nicht absichtlich aktiviert hast, um dir zu helfen, Duplikate zu finden. Auch die öffentliche Auflistung ist nur mit der neuesten stabilen Version von ASF kompatibel und kann sich weigern, veraltete Bots anzuzeigen, insbesondere wenn ihnen Kernfunktionen fehlen, die nur in neueren Versionen zu finden sind.

### API

ASF STM Inserate akzeptieren vorerst nur ASF Bots. Es gibt vorerst keine Möglichkeit, Drittanbieter-Bots auf unserer Auflistung einzutragen (da wir ihren Quellcode nicht einfach überprüfen können und sicherstellen müssen, dass sie unserer gesamten Handelslogik entsprechen).

Wenn du nach einem einfachen Weg suchst, um programmgesteuert auf unser Angebot zuzugreifen, haben wir einen sehr einfachen **[/Api/Bots](https://asf.justarchi.net/Api/Bots)** Endpunkt, den du verwenden kannst.

### Bemerkung

*Das gesamte Konzept, zusammen mit der Webseitenintegration und dem ASF-Reporting befindet sich noch in der Beta-Phase - es kann im Laufe der Zeit verbessert/geändert werden - auch dann, wenn wir das Gefühl haben, dass es nicht genügend Interesse an diesem Feature gibt.*

* * *

## Zustimmung verweigern

Die Teilnahme an Statistiken ist **nicht obligatorisch**, obwohl sie für die Zukunft des Programms sehr empfohlen wird. Wir verurteilen dich nicht, und wenn du den inneren Drang verspürst, die Tatsache zu verbergen, dass du ASF-Benutzer bist, dann kannst du die Statistik **vollständig** deaktivieren, indem du `Statistics` globale Konfigurationseigenschaft auf `false` umschaltest. Deaktivierte Statistiken machen das gesamte Modul unbrauchbar und führen keine der in unserer Datenschutzerklärung oben genannten Aktionen aus.

> Bitte bedenke, dass das Deaktivieren von Statistiken unseren technischen Support und andere Dinge, die dir (kostenlos) angeboten werden, wie z.B. ASF-Funktionalität und das Programm selbst, beeinflussen kann.