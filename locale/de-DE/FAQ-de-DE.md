# FAQ

Unser FAQ umfasst Standardfragen und Antworten, die du vielleicht hast. Für weniger häufig gestellte Fragen besuche bitte stattdessen unser **[erweitertes FAQ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Extended-FAQ-de-DE)**.

# Inhaltsverzeichnis

- [Allgemein](#allgemein)
- [Vergleich mit ähnlichen Programmen](#vergleich-mit-ähnlichen-programmen)
- [Sicherheit / Datenschutz / VAC / Sperren / Nutzungsbedingungen](#sicherheit-datenschutz-vac-sperren-nutzungsbedingungen)
- [Sonstiges](#sonstiges)
- [Probleme](#probleme)

* * *

## Allgemein

### Also wie funktioniert das genau?

Bevor Sie versuchen zu verstehen, was ASF ist, sollten Sie sicherstellen, dass Sie verstehen, was Steam Sammelkarten sind und wie man sie erhält, was in der offiziellen FAQ **[hier](https://steamcommunity.com/tradingcards/faq)** gut beschrieben ist.

Kurz gesagt, Steam-Sammelkarten sind sammelbare Gegenstände, für die Sie berechtigt sind, wenn Sie ein bestimmtes Spiel besitzen, und können für die Herstellung von Abzeichen, den Verkauf auf dem Steam-Markt oder für jeden anderen Zweck Ihrer Wahl verwendet werden.

Hier werden noch einmal Kernpunkte genannt, weil Leute ihnen im Allgemeinen nicht zustimmen wollen:

- **Ja, Sie müssen das Spiel besitzen, um für Kartendrops berechtigt zu sein. Spiele die über die Steam-Familienbibliothek geteilt werden zählen nicht.**
- **Nein, du kannst das Spiel nicht unendlich lange farmen - jedes Spiel hat eine feste Anzahl an Kartendrops. Sobald du alle Karten für das jeweilige Spiel bekommen hast wird es nicht mehr gefarmt.**
- **Nein, du kannst keine Karten von F2P Spiele bekommen ohne Geld in ihnen ausgegeben zu haben. Dies beinhaltet dauerhafte F2P Spiele so wie Team Fortress 2 oder Dota 2.**
- **Nein, du kannst keine Kartendrops auf einem eingeschränkten Account (diejenigen die nie 5€ im Steam Store ausgegeben haben) erhalten, unabhängig der Anzahl der Spiele im Besitz. Es war in der Vergangenheit möglich, aber dies ist nicht mehr der Fall.**

Wie du sehen kannst, werden dir Steam-Karten verliehen, weil du ein Spiel spielst, das du gekauft hast oder ein F2P-Spiel in das du Geld gesteckt hast. Mit anderen Worten, wenn du ein Spiel lange genug spielst fallen alle Karten für dieses Spiel in dein Inventar, so dass du ein Abzeichen erstellen, die Karten verkaufen oder tun kannst, was du willst.

ASF als Programm ist recht komplex zu verstehen, so dass wir, anstatt alle technischen Details zu erklären, im Folgenden eine sehr vereinfachte Erklärung anbieten werden.

ASF meldet sich über den integrierten Mini Steam-Client mit den von dir angegebenen Anmeldeinformationen bei deinem Steam-Konto an. Nach erfolgreicher Anmeldung analysiert es deine **[Abzeichen](https://steamcommunity.com/my/badges)** um Spiele zu finden, die für das Sammeln verfügbar sind (Du kannst X weitere Karten erhalten, wenn du dieses Spiel spielst). Nachdem alle Seiten analysiert und eine endgültige Liste der verfügbaren Spiele erstellt wurde, wählt ASF den effizientesten Sammel-Algorithmus aus und startet den Prozess. Der Prozess hängt von dem gewählten **[Karten-Sammel-Algorithmus](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)** ab, aber normalerweise besteht er aus dem Spielen des geeigneten Spiels und der periodischen (plus bei jedem Karten-Drop) Überprüfung, ob das Spiel bereits vollständig gesammelt wurde - wenn ja, kann ASF mit dem nächsten Spiel fortfahren, mit dem gleichen Verfahren, bis alle Spiele vollständig im gesammelt wurden.

Beachte, dass die obige Erklärung vereinfacht ist und nicht Dutzende von zusätzlichen Features und Funktionen beschreibt die ASF anbietet. Besuche den Rest von **[unserem Wiki](https://github.com/JustArchiNET/ArchiSteamFarm/wiki)**, wenn du jedes Detail von ASF wissen willst. Ich habe versucht es einfach zu halten, damit es für alle verständlich ist, ohne technische Details einzubringen - fortgeschrittene Benutzer werden ermutigt, tiefer zu graben.

Nun als Programm bietet ASF etwas Magie. Erstens, es muss keine deiner Spieldateien herunterladen, es kann sofort Spiele spielen. Zweitens ist es völlig unabhängig von deinem normalen Steam-Client - du musst den Steam-Client nicht laufen lassen oder gar installiert haben. Drittens ist es eine automatisierte Lösung - was bedeutet, dass ASF automatisch alles hinter deinem Rücken erledigt, ohne dass du ihm sagen musst, was er tun soll - was dir Ärger und Zeit erspart. Letztendlich muss es das Steam-Netzwerk nicht durch Prozessemulation (die z.B. Idle Master verwendet) austricksen, da es direkt mit ihm kommunizieren kann. Es ist auch superschnell und leicht und ist eine erstaunliche Lösung für alle, die Karten ohne großen Aufwand bekommen wollen - es ist besonders nützlich, wenn man es im Hintergrund laufen lässt, während man etwas anderes macht oder sogar im Offline-Modus spielt.

All das oben Gesagte ist schön, aber ASF hat auch einige technische Einschränkungen, die von Steam erzwungen werden - wir können keine Spiele sammeln, die du nicht besitzt, wir können keine Spiele für immer sammeln, um zusätzliche Drops über das erzwungene Limit hinaus zu bekommen, und wir können keine Spiele sammeln, während du spielst. All das sollte "logisch" sein, wenn man bedenkt, wie ASF funktioniert, aber es ist schön zu beachten, dass ASF keine Superkräfte hat und nichts tun wird, was physisch unmöglich ist, also denk daran - es ist im Grunde genommen dasselbe, als ob du jemandem gesagt hättest, dass er sich von einem anderen PC aus in dein Konto einloggen und diese Spiele für dich sammeln soll.

Zusammenfassend lässt sich sagen: ASF ist ein Programm, das dir hilft, die Karten, für die du berechtigt bist, ohne großen Aufwand sammeln zu lassen. Es bietet auch einige andere Funktionen, aber lasst uns vorerst bei dieser bleiben.

* * *

### Muss ich meine Zugangsdaten angeben?

**Ja**. ASF benötigt deine Kontoanmeldeinformationen auf die gleiche Weise wie der offizielle Steam-Client, da er die gleiche Methode für die Steam-Netzwerkinteraktion verwendet. Dies bedeutet jedoch nicht, dass du deine Zugangsdaten in ASF-Konfigurationen eingeben musst, du kannst ASF weiterhin mit `null`/leerem `SteamLogin` und/oder `SteamPassword` verwenden und bei Bedarf diese Daten für jeden ASF-Lauf eingeben (sowie mehrere andere Zugangsdaten, siehe Konfiguration). Auf diese Weise werden deine Daten nirgendwo gespeichert, aber natürlich kann ASF ohne deine Hilfe nicht automatisch starten. ASF bietet auch mehrere andere Möglichkeiten, deine **[Sicherheit](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security-de-DE)** zu erhöhen, also zögere nicht, diesen Teil des Wikis zu lesen, wenn du ein fortgeschrittener Benutzer bist. Wenn du das nicht bist und deine Konto-Anmeldeinformationen nicht in ASF-Konfigurationen einfügen möchtest, dann tue das einfach nicht und gebe es stattdessen bei Bedarf ein, wenn ASF nach ihnen fragt.

Bedenke, dass ASF für deinen persönlichen Gebrauch bestimmt ist und deine Zugangsdaten werden deinen Computer niemals verlassen. Du teilst sie auch mit niemandem, was Steam ToS erfüllt - eine sehr wichtige Sache, die viele Leute vergessen. Du schickst deine Daten nicht an unsere Server oder einen Dritten, sondern nur direkt an Steam-Server, die von Valve betrieben werden.

* * *

### Wie lange muss ich warten bis Karten droppen?

**So lange, wie es dauert** - ernsthaft. Jedes Spiel hat unterschiedliche Sammel-Schwierigkeiten, die vom Entwickler/Herausgeber festgelegt werden, und es liegt ganz an ihnen, wie schnell die Karten gesammelt werden. Die Mehrheit der Spiele folgt 1 Drop pro 30 Minuten Spielzeit, aber es gibt auch Spiele, die von dir verlangen, sogar mehrere Stunden zu spielen, bevor du eine Karte bekommst. Darüber hinaus kann es sein, dass dein Konto daran gehindert wird, Karten von Spielen zu erhalten, die du noch nicht lange genug gespielt hast, wie im Abschnitt **[Performance](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance-de-DE)** beschrieben. Versuche nicht zu erraten, wie lange ASF den gegebenen Titel sammeln soll - es liegt weder an dir, noch an ASF dies zu entscheiden. Es gibt nichts, was du tun kannst, um es schneller zu machen, und es gibt keinen "Bug", der damit zusammenhängt, dass Karten nicht rechtzeitig gesammelt werden - du kontrollierst den Karten-Sammel-Prozess nicht, ebenso wenig wie ASF. Im besten Fall erhältst du durchschnittlich 1 Karte pro 30 Minuten. Im schlimmsten Fall erhältst du auch 4 Stunden nach dem Start von ASF keine Karte. Beide dieser Situationen sind normal und werden in unserem Abschnitt **[Performance](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance-de-DE)** beschrieben.

* * *

### Das Sammeln dauert so lange - Kann ich es irgendwie beschleunigen?

Das Einzige, was die Geschwindigkeit des Sammelns stark beeinflusst ist der gewählte **[Sammel-Algorithmus](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance-de-DE)** für die jeweilige Bot-Instanz. Alles andere hat einen unwesentlichen Effekt und beschleunigt das Sammeln nicht, wobei manche Dinge, wie zum Beispiel das mehrfache Starten von ASF, es sogar **schlechter** machen. Wenn du wirklich den Drang hast, jede einzelne Sekunde des Sammel-Prozesses zu machen, dann erlaubt ASF dir einige grundlegende Sammel-Variablen wie `FarmingDelay` zu optimieren - alle von ihnen sind in der **[Konfiguration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-de-DE)** erklärt. Nichtsdestotrotz ist dieser Effekt unwesentlich und den entsprechenden Sammelalgorithmus für den jeweiligen Account zu wählen ist die einzige ausschlaggebende Wahl, die die Geschwindigkeit schwer beeinflussen kann, während alles andere rein kosmetisch ist. Anstatt dich wegen der Geschwindigkeit zu sorgen, solltest du einfach ASF starten und es seinen Job machen lassen - Ich kann dir versichern, dass es das auf die effektivste Art macht, die wir uns ausdenken konnten. Je weniger du dich sorgst, desto zufriedener wirst du sein.

* * *

### Aber ASF sagte, dass das Sammeln etwa X Zeit in Anspruch nehmen wird!

ASF gibt dir eine grobe Schätzung basierend auf der Anzahl der Karten, die du sammeln musst, und dem gewählten Algorithmus - dies ist bei weitem nicht annähernd die tatsächliche Zeit, die du für das Sammeln aufwenden wirst, die in der Regel länger ist als diese, da ASF nur den besten Fall annimmt, und ignoriert alle Steam-Netzwerk-Eigenarten, Internet-Trennungen, Überlastung der Steam-Server und ähnliches. Es ist nur als allgemeiner Indikator zu verstehen, wie lange man mit dem Sammeln von ASF rechnen kann, im besten Fall sehr oft, da die tatsächliche Zeit variiert, in einigen Fällen sogar deutlich. Wie oben erwähnt, versuche nicht zu erraten, wie lange das Spiel gesammelt wird, es liegt nicht weder an dir, noch an ASF dies zu entscheiden.

* * *

### Kann ASF auf meinem Android/Smartphone funktionieren?

ASF ist ein C#-Programm, das eine funktionierende Implementierung von .NET Core erfordert. Derzeit gibt es keinen nativen .NET Core Build für Android selbst, aber es gibt ordentliche und funktionierende Builds für Linux auf ARM-Architektur, so dass es durchaus möglich ist, so etwas wie **[Linux Deploy](https://play.google.com/store/apps/details?id=ru.meefik.linuxdeploy)** für die Installation von Linux zu verwenden, dann ASF in einem solchen Linux Chroot wie gewohnt zu verwenden.

Es ist sehr wahrscheinlich, dass wir in Zukunft mit einem funktionierenden .NET Core für Android rechnen können.

* * *

### Kann ASF Gegenstände aus Steam-Spielen wie CS:GO oder Unturned sammeln?

**Nein**, dies verstößt gegen Steam-Nutzungsbedingungen und Valve hat dies mit der letzten Welle von Community-Sperren für das Sammeln von TF2-Gegenständen deutlich gemacht. ASF ist ein Steam-Karten-Sammel-Programm, kein Spiele-Gegenstände-Sammel-Programm - es hat keine Möglichkeit, Spiele-Gegenstände zu sammeln, und es ist nicht geplant, ein solches Feature in der Zukunft hinzuzufügen, niemals, hauptsächlich wegen der Verletzung der Steam-Nutzungsbedingungen. Bitte stell keine Fragen dazu - das Allerbeste, was du bekommen kannst, ist ein Report von einem salzigen Benutzer und du hast Probleme. Das Gleiche gilt für alle anderen Arten des Sammelns, wie z.B. Drops von CS:GO Übertragungen. ASF konzentriert sich ausschließlich auf Steam-Handelskarten.

* * *

### Kann ich mir aussuchen welche Spiele gesammelt werden sollen?

**Ja**, auf verschiedene Arten. Wenn du die Standardreihenfolge der Sammel-Warteschlange ändern möchtest, dann ist das das, wofür `FarmingOrders` **[Bot Konfigurations-Eigenschaft](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-de-DE#bot-konfiguration)** verwendet werden kann. Wenn du bestimmte Spiele manuell auf die schwarze Liste setzen möchtest, damit sie nicht automatisch gesammelt werden, kannst du die schwarze Liste verwenden, die mit `ib` **[Befehl](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE)** verfügbar ist. Wenn du alles sammeln möchtest, aber einigen Spielen Vorrang vor allem anderen geben möchtest, dann ist das das, wofür die Sammel-Prioritäts-Warteschlange, die mit `iq` **[Befehl](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE)** verfügbar ist, verwendet werden kann. Und schließlich, wenn du nur bestimmte Spiele deiner Wahl sammeln möchtest, dann kannst du `IdlePriorityQueueOnly` **[Bot Konfigurations-Eigenschaft](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-de-DE#bot-konfiguration)** verwenden, um dies zu erreichen, zusammen mit dem Hinzufügen deiner ausgewählten Spiele zur Sammel-Prioritäts-Warteschlange.

Zusätzlich zur Verwaltung des oben beschriebenen Moduls für das automatische Karten-Sammeln kannst du ASF auch mit dem `play` **[Befehl](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE)** in den manuellen Sammel-Modus umschalten oder einige andere externe Einstellungen wie `GamesPlayedWhileIdle` **[Bot Konfigurations-Eigenschaft](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-de-DE#bot-konfiguration)** verwenden.

* * *

### Ich bin ein Linux / OS X Benutzer, kann ASF Spiele sammeln, die für mein Betriebssystem nicht verfügbar sind? Wird ASF 64-Bit-Spiele sammeln, wenn ich es auf einem 32-Bit-Betriebssystem ausführe?

Ja, ASF kümmert sich nicht einmal um das Herunterladen aktueller Spieldateien, so dass es mit all deinen Lizenzen funktioniert, die an dein Steam-Konto gebunden sind, unabhängig von Plattform oder technischen Anforderungen. Es sollte auch für Spiele funktionieren, die an eine bestimmte Region gebunden sind (region-locked Spiele), auch wenn du nicht in der passenden Region bist, obwohl wir dies nicht getestet haben.

* * *

## Vergleich mit ähnlichen Programmen

* * *

### Ist ASF ähnlich wie Idle Master?

Die einzige Ähnlichkeit ist der allgemeine Zweck beider Programme, nämlich das Sammeln von Steam-Spielen, um Karten zu erhalten. Alles andere, einschließlich der eigentlichen Sammel-Methode, der verwendeten Algorithmen, der Programmstruktur, der Funktionalität, der Kompatibilität, die mit dem Quellcode selbst endet, ist völlig unterschiedlich und diese beiden Programme haben nichts gemeinsam, auch nicht die Kerngrundlage (IM läuft auf .NET Framework, ASF auf .NET Core). ASF wurde geschaffen, um IM-Probleme zu lösen, die mit einer einfachen Programmcode-Editierung nicht zu lösen waren - deshalb wurde ASF von Grund auf neu geschrieben, ohne eine einzige Programmzeile oder gar eine allgemeine Idee des IM zu verwenden, weil dieser Code und diese Ideen von Anfang an völlig fehlerhaft waren. IM und ASF sind wie Windows und Linux - beide sind Betriebssysteme und beide können auf deinem PC installiert werden, aber sie haben fast nichts miteinander zu tun, abgesehen davon, dass sie einem ähnlichen Zweck dienen.

Aus diesem Grund solltest du ASF nicht mit IM vergleichen, basierend auf den Erwartungen von IM. Du solltest ASF und IM als völlig unabhängige Programme mit eigenen exklusiven Funktionalitäten betrachten. Einige von ihnen überschneiden sich tatsächlich und man kann in beiden ein bestimmtes Feature finden, aber sehr selten, da ASF seinen Zweck mit einem völlig anderen Ansatz als IM erfüllt.

* * *

### Lohnt es sich, ASF zu verwenden, wenn ich gerade Idle Master verwende und es für mich gut funktioniert?

**Ja**. ASF ist viel zuverlässiger und beinhaltet viele eingebaute Funktionen, die **entscheidend** sind, unabhängig davon, wie du sammelst, den IM einfach nicht anbietet.

ASF hat die richtige Logik für **unveröffentlichte Spiele** - IM wird versuchen, Spiele zu sammeln, die bereits Karten hinzugefügt haben, auch wenn sie noch nicht veröffentlicht wurden. Natürlich ist es nicht möglich, diese Spiele bis zum Veröffentlichungsdatum zu sammeln, so dass dein Sammel-Prozess stecken bleibt. Dazu musst du es entweder zur Blacklist hinzufügen, auf die Freigabe warten oder manuell überspringen. Keine dieser Lösungen ist gut, und alle erfordern deine Aufmerksamkeit - ASF überspringt automatisch das Sammeln unveröffentlichter Spiele (vorübergehend) und kehrt später zu ihnen zurück, wenn sie es sind, um das Problem vollständig zu vermeiden und effizient damit umzugehen.

ASF hat auch die richtige Logik für **Serien** Videos. Es gibt viele Videos auf Steam, die Karten haben, aber mit falscher `appID` auf der Abzeichen-Seite angegeben sind, wie z.B. **[Double Fine Adventure](https://store.steampowered.com/app/402590)** - IM wird die falsche `appID` sammeln, was keine Karten ergibt und der Prozess stecken bleibt. Wieder einmal musst du es entweder auf die schwarze Liste setzen oder manuell überspringen, wobei beide deine Aufmerksamkeit erfordern. ASF entdeckt automatisch die richtige `appID` für das Sammeln von Karten.

Darüber hinaus ist ASF **viel stabiler und zuverlässiger**, wenn es um Netzwerkprobleme und Steam-Macken geht - es funktioniert meistens und erfordert deine Aufmerksamkeit überhaupt nicht, wenn es einmal konfiguriert ist, während IM oft für viele Leute abbricht, zusätzliche Korrekturen erfordert oder trotzdem einfach nicht funktioniert. Es ist auch völlig abhängig von deinem Steam-Client, was bedeutet, dass es nicht funktionieren kann, wenn dein Steam-Client ernste Probleme hat. ASF funktioniert ordnungsgemäß, solange es mit dem Steam-Netzwerk verbunden werden kann, und erfordert nicht, dass der Steam-Client ausgeführt oder sogar installiert wird.

Dies sind 3 **sehr wichtige** Punkte, warum du ASF verwenden solltest, da sie jeden der Steam-Karten sammelt direkt betreffen und es keine Möglichkeit gibt zu sagen "das betrifft mich nicht", da Steam-Wartungen und -Pannen jedem passieren. Es gibt Dutzende von zusätzlichen, unwichtigeren und wichtigeren Gründen, die du im Rest des FAQs erfahren kannst. Also kurz gesagt, **ja**, du solltest ASF verwenden, auch wenn du keine zusätzliche ASF-Funktion brauchst, die im Vergleich zu IM verfügbar ist.

Darüber hinaus wird IM offiziell aufgegeben und könnte in Zukunft komplett kaputt gehen, ohne dass sich jemand die Mühe macht, es zu reparieren, wenn man bedenkt, dass es viel leistungsfähigere Lösungen gibt (nicht nur ASF). IM funktioniert bereits für viele Leute nicht mehr, und diese Zahl nimmt nur zu, nicht ab. Du solltest es vermeiden, veraltete Software zu verwenden, nicht nur IM, sondern auch alle anderen veralteten Programme. Kein aktiver Verwalter bedeutet, dass sich niemand darum kümmert, ob es funktioniert oder nicht, niemand verifiziert, ob es funktioniert, und **niemand ist für seine Funktionalität** verantwortlich, was für die Sicherheit entscheidend ist.

* * *

### Welche interessanten Features bietet ASF, die Idle Master nicht bietet?

Es hängt davon ab, was für dich "interessant" ist. Wenn du vorhast, mehr als ein Konto zu nutzen, dann ist die Antwort bereits offensichtlich, da ASF es dir erlaubt, auf allen zu Sammeln mit einer übergeordneten Gesamtlösung, Ressourcen zu sparen, Probleme mit Ärger und Kompatibilität zu lösen. Wenn du allerdings diese Frage stellst, dann hast du höchstwahrscheinlich nicht diesen speziellen Bedarf, also lass uns andere Vorteile bewerten, die für ein einzelnes Konto gelten, das in ASF verwendet wird.

In erster Linie hast du einige eingebaute Funktionen, die **[oben](#lohnt-es-sich-asf-zu-verwenden-wenn-ich-gerade-idle-master-verwende-und-es-für-mich-gut-funktioniert)** erwähnt werden, die der Kern für das Sammeln sind, unabhängig von deinem Endziel, und sehr oft reicht das allein schon aus, um ASF zu verwenden. Aber das weißt du bereits, also laßt uns zu einigen weiteren interessanten Features kommen:

- **Du kannst offline sammeln** (`OnlineStatus` von `Offline` Feature). Das Offline-Sammeln ermöglicht es dir, deinen Steam-In-Game-Status komplett zu überspringen, was es dir ermöglicht, mit ASF zu sammeln, während du gleichzeitig "Online" auf Steam bist, ohne dass deine Freunde überhaupt merken, dass ASF in deinem Namen ein Spiel spielt. Dies ist eine überlegene Funktion, da es dir erlaubt, online in deinem Steam-Client zu bleiben, ohne deine Freunde mit ständigen Spieländerungen zu belästigen oder sie in die Irre zu führen, dass du ein Spiel spielst, während du es in Wirklichkeit nicht bist. Allein dieser Punkt macht es sinnvoll, ASF zu verwenden, wenn man seine eigenen Freunde respektiert, aber es ist nur der Anfang. Es ist auch schön zu beachten, dass diese Funktion nichts mit den Privatsphäre-Einstellungen von Steam zu tun hat - wenn du das Spiel selbst startest, dann wirst du deinen Freunden richtig als In-Game angezeigt, so dass nur der ASF-Teil unsichtbar wird und dein Konto überhaupt nicht beeinträchtigt wird.

- **Du kannst erstattungsfähige Spiele überspringen** (`IdleRefundableGames` Feature). ASF hat eine eingebaute Logik für rückerstattungsfähige Spiele und du kannst ASF so konfigurieren, dass es keine rückerstattungsfähige Spiele automatisch sammelt. Auf diese Weise kannst du selbst beurteilen, ob dein neu gekauftes Spiel aus dem Steam-Shop dein Geld wert war, ohne dass ASF versucht, so schnell wie möglich Karten zu sammeln. Wenn du es für 2+ Stunden spielst, oder 2 Wochen seit deinem Kauf vergangen sind, dann wird ASF mit diesem Spiel fortfahren, da es nicht mehr rückerstattungsfähig ist. Bis dahin hast du die volle Kontrolle, ob du es magst oder nicht, und du kannst es bei Bedarf leicht zurückerstatten, ohne dass du dieses Spiel manuell auf die schwarze Liste setzen oder ASF für die gesamte Dauer nicht verwenden musst.

- **Du kannst neue Gegenstände automatisch als erhalten markieren** (`BotBehaviour` von `DismissInventoryNotifications` Feature). Das Sammeln mit ASF führt dazu, dass dein Konto neue Karten erhält. Du weißt bereits, dass dies geschehen wird, also lass ASF diese unnötige Benachrichtigung für dich lesen und sicherstellen, dass nur wichtige Dinge deine Aufmerksamkeit gewinnen. Natürlich nur wenn du willst.

- **Du kannst automatisch Karten von Steam Events** erhalten (`AutoSteamSaleEvent` Feature). ASF ermöglicht es dir, das Durchlaufen der Entdeckungsliste und das Abstimmen bei Steam Awards während des Steam-Sales zu automatisieren, natürlich nur, wenn du das nutzen möchtest. Dies spart jeden Tag enorm viel Zeit, während der Steam-Sale stattfindet, und stellt sicher, dass du deine täglichen Karten nie wieder verpassen wirst.

- **Du kannst die bevorzugte Sammel-Reihenfolge mit mehr verfügbaren Optionen anpassen** (`FarmingOrders` Feature). Vielleicht möchtest du zuerst deine neu gekauften Spiele sammeln? Oder deine ältesten? Nach der Anzahl der Karten? Abzeichen-Level, die du bereits hergestellt hast? Gespielte Stunden? Alphabetisch? Nach den AppIDs? Oder komplett zufällig? Das liegt ganz bei dir.

- **ASF kann dir helfen, deine Sets zu vervollständigen** (`TradingPreferences` mit `SteamTradeMatcher` Feature). Mit etwas fortgeschrittenerem Tüfteln kannst du deinen ASF in einen vollwertigen Benutzer-Bot umwandeln, der automatisch **[STM](https://www.steamtradematcher.com)** Handelsangebote akzeptiert und dir jeden Tag hilft, deine Sets ohne jegliche Benutzerinteraktion zu ergänzen. ASF enthält sogar ein eigenes ASF 2FA-Modul, mit dem du deinen mobilen Steam-Authentifikator importieren und den gesamten Prozess vollständig automatisieren kannst, auch bei der Annahme von Bestätigungen. Oder möchtest du vielleicht manuell akzeptieren und ASF nur diese Handelsangebote für dich vorbereiten lassen? Das liegt wieder einmal ganz bei dir.

- **ASF kann Produktschlüssel im Hintergrund für dich einlösen** (**[Hintergrundproduktschlüsselaktivierer](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer-de-DE)** Feature). Vielleicht hast du hundert Produktschlüssel aus verschiedenen Bundles wo du zu faul bist alle einzulösen, da du bei jedem durch ein paar Fenster gehen und den Steam-Bedingungen immer wieder zustimmen musst. Warum nicht einfach die Liste der Produktschlüssel zu ASF kopieren und es seine Arbeit machen lassen? ASF löst automatisch alle diese Produktschlüssel im Hintergrund ein und stellt dir entsprechende Ausgaben zur Verfügung, damit du weißt, wie sich jeder Einlösungsversuch ausgewirkt hat. Außerdem, wenn du hunderte von Produktschlüssel hast, wirst du garantiert früher oder später von Steam rate-limited, und ASF weiß auch davon, es wird geduldig warten, bis das Rate-Limit weggeht, und dort weitermachen, wo es aufgehört hat.

Wir könnten nun mit dem gesamten **[ASF-Wiki](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Home-de-DE)** fortfahren und auf jedes einzelne Feature des Programms hinweisen, aber wir müssen irgendwo eine Grenze ziehen. Das ist es, dies ist eine Liste von Features, die du als ASF-Benutzer genießen kannst, wo nur eine davon leicht als ein Hauptgrund angesehen werden könnte, nie zurückzuschauen, und wir haben tatsächlich **eine Menge** von ihnen aufgelistet, mit noch mehr, über die du auf dem Rest unseres Wikis mehr erfahren kannst. Ah ja, und wir sind nicht einmal ins Detail gegangen, mit Dingen wie ASFs API, die es dir ermöglicht, deine eigenen Befehle zu skripten, oder glorreiches Bots-Management, da wir es einfach halten wollten.

* * *

### Ist ASF schneller als Idle Master?

**Ja**, obwohl die Erklärung ziemlich kompliziert ist.

Bei jedem neuen Prozess, der in deinem System gestartet und beendet wird, sendet der Steam-Client automatisch eine Anfrage mit allen deinen Spielen, die du gerade spielst - so kann das Steam-Netzwerk Stunden berechnen und Karten ausgeben. Das Steam-Netzwerk zählt jedoch deine Spielzeit in Intervallen von 1 Sekunde, und das Senden einer neuen Anfrage setzt den aktuellen Status zurück. Mit anderen Worten, wenn du alle 0,5 Sekunden einen neuen Prozess starten/abbrechen würdest, würdest du nie eine Karte sammeln, weil jeder 0,5-Sekunden-Steam-Client eine neue Anfrage senden würde und das Steam-Netzwerk nie auch nur 1 Sekunde Spielzeit zählen würde. Außerdem ist es aufgrund der Funktionsweise des Betriebssystems durchaus üblich, neue Prozesse zu erkennen, die ohne dass du überhaupt etwas tust, also selbst wenn du nichts auf deinem PC tust - es gibt viele Prozesse, die immer noch im Hintergrund arbeiten und andere Prozesse die ganze Zeit über treiben/beenden. Idle Master basiert auf dem Steam Client, so dass dieser Mechanismus dich beeinflusst, wenn du ihn benutzt.

ASF basiert nicht auf dem Steam-Client, sondern hat eine eigene Steam-Client-Implementierung. Dank dessen ist das, was ASF tut, nicht das Auslösen eines Prozesses, sondern das Senden einer echten Anfrage an das Steam-Netzwerk, dass wir mit dem Spielen eines Spiels begonnen haben. Dies ist die gleiche Anfrage, die vom Steam-Client gesendet wird, aber da wir die tatsächliche Kontrolle über den ASF-Steam-Client haben, brauchen wir keine neuen Prozesse zu starten, und wir imitieren den Steam-Client nicht bezüglich der Sendeanforderung bei jeder Prozessänderung, so dass der oben beschriebene Mechanismus uns nicht betrifft. Dank dessen unterbrechen wir nie das 1-Sekunden-Intervall auf Steam, was unsere Sammel-Geschwindigkeit erhöht.

* * *

### Aber ist der Unterschied wirklich spürbar?

Nein. Die Unterbrechungen, die mit einem normalen Steam-Client und dem Idle Master auftreten, haben nur einen geringen Einfluss auf die Kartendrops, so dass es kein spürbarer Unterschied ist, der ASF überlegen machen würde.

Jedoch **gibt es** einen Unterschied, und du kannst deutlich feststellen, dass, je nachdem, wie beschäftigt dein Betriebssystem ist, Karten **werden** schneller gesammelt, von ein paar Sekunden auf sogar ein paar Minuten, wenn du extrem unglücklich bist. Obwohl ich nicht in Betracht ziehen würde, ASF nur zu verwenden, weil es Karten schneller sammelt, da sowohl ASF als auch Idle Master von der Funktionsweise des Steam-Netzes beeinflusst werden, interagiert ASF einfach effektiver mit dem Steam-Netz, während Idle Master nicht kontrollieren kann, was der Steam-Client tatsächlich tut (also ist es nicht die Schuld von Idle Master, sondern die des Steam-Clients selbst).

* * *

### Kann ASF mehrere Spiele auf einmal sammeln?

**Ja**, obwohl ASF besser weiß, wann dieses Feature zu verwenden ist, basierend auf dem ausgewählten **[Karten-Sammel-Algorithmus](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance-de-DE)**. Du hast keine direkte Wahl beim Karten-Sammel-Algorithmus, aber du kannst ASF eins vorschlagen, indem du die Konfigurationseigenschaften richtig einstellst. Du solltest dich auf den Konfigurationsteil von ASF konzentrieren und Algorithmen entscheiden lassen, was der optimale Weg ist, um das Ziel zu erreichen.

* * *

### Kann ASF schnell durch die Spiele springen?

**Nein**, ASF unterstützt nicht und fördert auch nicht die Verwendung von **[Steam-Pannen](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance#steam-glitches)**.

* * *

### Kann ASF jedes Spiel automatisch für X Stunden spielen, bevor Karten gesammelt werden?

**Nein**, der Sinn der Systemumstellung von Steam-Karten war es, mit falschen Statistiken und Geisterspielern zu kämpfen. ASF wird nicht mehr als nötig dazu beitragen, denn das Hinzufügen eines solchen Features ist nicht geplant und wird nicht geschehen.

* * *

### Kann ich ein Spiel spielen während ASF am Sammeln ist?

**Nein**. ASF hat im Gegensatz zu IM einen unabhängigen Steam-Client integriert, und das Steam-Netzwerk erlaubt es nur **einem Steam-Client auf einmal**, ein Spiel zu spielen. Du kannst die ASF-Verbindung jedoch jederzeit trennen, indem du ein Spiel startest (und auf "OK" klickst, wenn du gefragt wirst, ob das Steam-Netzwerk einen anderen Client trennen soll) - ASF wird dann geduldig warten, bis du fertig bist, und den Prozess danach fortsetzen. Alternativ kannst du immer noch im Offline-Modus spielen, wann immer du willst, wenn das für dich befriedigend ist.

Bedenke, dass die Drop-Rate der Karten, wenn du mehrere Spiele spielst, ohnehin nahe bei 0 liegt, daher gibt es keine direkten Vorteile, wenn du das mit IM machen kannst, während es starke Vorteile gibt, wenn du dich nicht in andere Spiele einmischst, die mit ASF gestartet wurden, was z.B. für VAC entscheidend ist.

* * *

## Sicherheit / Datenschutz / VAC / Sperren / Nutzungsbedingungen

* * *

### Kann ich eine VAC-Sperre bekommen, wenn ich ASF nutze?

Nein, das ist nicht möglich, da ASF (im Gegensatz zu Idle Master oder SAM) in keiner Weise den Steam-Client oder seine Prozesse stört. Es ist physisch unmöglich, eine VAC-Sperre für die Verwendung von ASF zu erhalten, selbst wenn man auf gesicherten Servern spielt, während ASF läuft - das liegt daran, dass **ASF nicht einmal verlangt, dass der Steam-Client überhaupt installiert ist**, um ordnungsgemäß zu funktionieren. ASF ist das einzige Sammel-Programm, das derzeit die VAC-Freiheit garantieren kann.

* * *

### Kann die Verwendung von ASF verhindern, dass ich auf VAC-geschützten Servern spiele kann, wie **[hier](https://support.steampowered.com/kb_article.php?ref=2117-ilzv-2837)** angegeben?

ASF benötigt keinen Steam-Client, der ausgeführt oder gar installiert wird. Nach diesem Konzept sollte es **keine** VAC-bezogenen Probleme verursachen, da ASF garantiert, dass der Steam-Client und alle seine Prozesse nicht gestört werden - das ist der wichtigste Punkt, wenn es um die VAC-freie Garantie geht, die ASF bietet.

Nach Angaben von Benutzern und nach bestem Wissen und Gewissen ist dies im Moment der Fall, da niemand irgendwelche Probleme wie im obigen Link während der Verwendung von ASF gemeldet hat. Wir konnten das obige Problem nicht mit ASF reproduzieren, während wir es mit Idle Master klar reproduzieren konnten.

Bedenke jedoch, dass Valve irgendwann immer noch ASF zur schwarzen Liste hinzufügen könnte, aber es ist ein kompletter Unsinn, denn selbst wenn sie das tun, könntest du immer noch VAC-gesicherte Spiele von deinem PC aus spielen und ASF gleichzeitig verwenden, z.B. auf deinem Server, also bin ich ziemlich sicher, dass sie sehr gut wissen, dass ASF kein verdächtiger VAC-technischer Verdächtiger sein sollte, und sie werden uns das Leben nicht schwerer machen, indem sie ASF ohne Grund auf die schwarze Liste setzen. Im schlimmsten Fall wirst du jedoch nicht spielen können, wie oben erwähnt, weil die VAC-freie Garantie von ASF immer noch da ist, unabhängig davon, ob Steam die ASF-Binärdatei auf die schwarze Liste setzt oder nicht (und du kannst ASF immer noch auf jeder anderen Maschine starten, ohne dass ein Steam-Client überhaupt installiert ist). Im Moment gibt es keinen Grund, irgendetwas davon zu tun, und wir hoffen, dass es so bleibt.

* * *

### Ist es sicher?

Wenn du fragst, ob ASF als Software sicher ist, was bedeutet, dass es deinem Computer keinen Schaden zufügt, deine privaten Daten nicht stiehlt, Viren oder ähnliches installiert - ist es sicher. Der Quelltext ist Open-Source, und verteilte Binärdateien werden immer aus **[öffentlich verfügbaren Quellen](https://en.wikipedia.org/wiki/Open-source_software)** von **[automatisierten und vertrauenswürdigen kontinuierlichen Integrationssystemen](https://en.wikipedia.org/wiki/Build_automation)** und nicht einmal von Entwicklern selbst erstellt. Jeder Build ist reproduzierbar, indem er unserem Build-Skript folgt, und wird genau dasselbe ergeben, **[deterministisch](https://en.wikipedia.org/wiki/Deterministic_system)** IL (binärer) Code.

Wenn du aus irgendeinem Grund unseren Builds nicht vertraust, kannst du ASF jederzeit aus dem Quelltext kompilieren und verwenden, einschließlich aller Bibliotheken, die ASF verwendet (wie SteamKit2), die ebenfalls Open-Source sind. Am Ende ist es jedoch immer eine Frage des Vertrauens zu den Entwicklern deiner Anwendung, also solltest du selbst entscheiden, ob du ASF für sicher hältst oder nicht und deine Entscheidung möglicherweise mit technischen Argumenten unterstützen. Glaube nicht blind an etwas, nur weil ich es gesagt habe - überprüfe es selbst, denn das ist der einzige Weg, um sicher zu gehen.

* * *

### Kann ich dafür gesperrt werden?

Um diese Frage zu beantworten, sollten wir einen genaueren Blick auf die **[Steam-Nutzungsbedingungen](https://store.steampowered.com/subscriber_agreement)** werfen. Steam verbietet nicht die Verwendung mehrerer Konten, vielmehr **[erlauben sie es](https://support.steampowered.com/kb_article.php?ref=8625-WRAH-9030#share)**, da du denselben mobilen Authentifikator für mehr als ein Konto verwenden kannst. Was es jedoch nicht erlaubt, ist die gemeinsame Nutzung von Konten mit anderen Personen, aber das tun wir hier nicht.

Der einzige wirkliche Punkt, der ASF betrifft, ist der folgende:

> You may not use Cheats, automation software (bots), mods, hacks, or any other unauthorized third-party software, to modify or automate any Subscription Marketplace process.

Die Frage ist, was ist eigentlich der Subscription Marketplace-Prozess. Wie wir lesen können:

> An example of a Subscription Marketplace is the Steam Community Market

Wir ändern oder automatisieren den Prozess des Abonnenten-Marktplatzes nicht, wenn wir unter Abonnenten-Marktplatz den Markt der Steam-Community oder den Steam-Shop verstehen. Allerdings...

> Valve may cancel your Account or any particular Subscription(s) at any time in the event that (a) Valve ceases providing such Subscriptions to similarly situated Subscribers generally, or (b) you breach any terms of this Agreement (including any Subscription Terms or Rules of Use).

Daher ist ASF, wie bei jeder Steam-Software, nicht von Valve autorisiert und ich kann nicht garantieren, dass du nicht gesperrt wirst, wenn Valve plötzlich entscheidet, dass sie Konten mit ASF jetzt sperren. Dies ist angesichts der Tatsache, dass ASF auf mehr als einer halben Million Steam-Konten verwendet wird, äußerst unwahrscheinlich, aber dennoch eine Möglichkeit, unabhängig von der tatsächlichen Wahrscheinlichkeit.

Besonders weil:

> In regard to all Subscriptions, Contents and Services that are not authored by Valve, Valve does not screen such third party content available on Steam or through other sources. Valve assumes no responsibility or liability for such third party content. Some third party application software is capable of being used by businesses for business purposes - however, you may only acquire such software via Steam for private personal use.

Allerdings erkennt Valve klar an, dass es "Steam-Sammler" gibt, wie **[hier](https://support.steampowered.com/kb_article.php?ref=2117-ilzv-2837)** angegeben, also wenn du mich fragst, bin ich mir ziemlich sicher, dass sie, wenn sie nicht einverstanden mit ihnen wären, bereits etwas tun würden, anstatt darauf hinzuweisen, dass sie VAC-technisch Probleme verursachen könnten. Das Schlüsselwort hier ist **Steam** Sammler, zum Beispiel ASF, und nicht **Spiel** Sammler.

Bitte bedenke, dass oben nur unsere Interpretation der Steam Nutzungsbedingungen und verschiedenen Punkten steht - ASF ist unter der Apache 2.0 Lizenz lizenziert, was klar und deutlich sagt:

> Unless required by applicable law or agreed to in writing, ASF is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.

Du benutzt diese Software auf eigene Gefahr. Es ist sehr unwahrscheinlich, dass man dafür gesperrt wird, aber wenn man es wird, kann man nur sich selbst die Schuld dafür geben.

* * *

### Wurde jemand dafür gesperrt?

**Ja**, wir hatten bereits ein paar Vorfälle, die zu einer Art Steam-Sperre führten. Ob ASF selbst die Ursache war oder nicht, ist eine ganz andere Geschichte, die wir wahrscheinlich nie erfahren werden.

Ein Fall war von einem Typen mit über 1000+ Bots, der vom Handel ausgeschlossen wurde (zusammen mit allen Bots), höchstwahrscheinlich aufgrund der übermäßigen Verwendung von `loot ASF`, das auf allen Bots gleichzeitig ausgeführt wurde, oder einer anderen verdächtigen einseitigen Menge von Handelsangeboten in sehr kurzer Zeit.

> Hello XXX, Thank you for contacting Steam Support. It looks like this account was used to manage a network of bot accounts. Botting is a violation of the Steam Subscriber Agreement.

Bitte, benutze etwas gesunden Menschenverstand und gehe nicht davon aus, dass du solche verrückten Dinge nur tun kannst, weil ASF dir das erlaubt. Das Ausführen von `loot ASF` auf über 1k Bots kann leicht als **[DDoS](https://en.wikipedia.org/wiki/DDoS)** Angriff angesehen werden, und ich persönlich bin nicht schockiert, dass jemand für so eine Sache gesperrt wurde. Bitte bedenke und benutze etwas gesunden Menschenverstand und ein Minimum an fairem Gebrauch in Bezug auf den Steam Service, und * höchstwahrscheinlich* wird es dir gut gehen.

Ein weiterer Fall war ein Typ mit 170+ Bots, der während dem Steam Winter Sale gesperrt wurde.

> Your account was blocked for violation of the agreement of the subscriber Steam. Judging by the exchanges and other factors, this account was used to illegally collect collectible cards on Steam, as well as related and not only commercial activities. The account has been permanently blocked and Steam Support can not provide additional support on this issue.

Dieser Fall ist wieder einmal sehr schwer zu analysieren, da die Reaktion des Steam-Supports, der kaum Details bietet, vage ist. Basierend auf meinen persönlichen Ansichten hat dieser Benutzer wahrscheinlich Steam-Karten gegen irgendeine Art von Geld eingetauscht (Level-up-Bot?) oder auf andere Weise versucht, bei Steam Geld auszuzahlen. Falls du es nicht wusstest, ist dies nach Steam-Nutzungsbedingungen auch verboten.

Letzter Fall, bei dem ein Benutzer mit 120+ Bots wegen Verletzung von **[Steam-Online-Verhalten](https://store.steampowered.com/online_conduct?l=english)** gesperrt wurde.

> Hello XXX, Thank you for contacting Steam Support. This and other accounts were used for flooding our network infrastructure, which is a violation of Steam online conduct. The account has been permanently blocked and Steam Support can not provide additional support on this issue.

Dieser Fall ist aufgrund der zusätzlichen Angaben des Benutzers etwas einfacher zu analysieren. Anscheinend verwendete der Benutzer **eine sehr veraltete ASF-Version**, die einen Fehler enthielt, der dazu führte, dass ASF eine übermäßige Anzahl von Anfragen an Steam-Server sendete. Der Fehler selbst existierte zunächst nicht, wurde aber aufgrund einer Änderung von Steam aktiviert, die in einer späteren Version behoben wurde. **ASF wird nur in der **[aktuellen stabilen Version](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** unterstützt, die auf GitHub** veröffentlicht wurde. Software wird von Menschen geschrieben, und Menschen neigen dazu, Fehler zu machen. Wenn der Fehler einen globalen Umfang hat, wird er schnell behoben und als Bugfix für alle Benutzer veröffentlicht. Valve wird nicht plötzlich eine halbe Million ASF-Benutzer aufgrund meines Fehlers sperren, aus offensichtlichen Gründen. Wenn du jedoch absichtlich auf die Verwendung einer aktuellen Version von ASF verzichtest, dann bist du per Definition in einer sehr kleinen Minderheit von Benutzern, die **Vorfällen wie diesen** aufgrund von **keiner technischen Unterstützung** ausgesetzt sind, da niemand auf deine veraltete Version von ASF aufpasst, niemand sie repariert und niemand sicherstellt, dass du nicht einfach durch den Start von ASF völlig gesperrt wirst. **Bitte verwende die aktuellste Version**, nicht nur ASF, sondern auch alle anderen Anwendungen.

* * *

Alle oben genannten Vorfälle haben eines gemeinsam - ASF ist nur ein Programm und es ist **deine** Entscheidung, wie du es nutzen willst. Du bekommst keine Sperre für die direkte Verwendung von ASF, aber es kommt darauf an **wie** du ASF benutzt. Es kann ein Hilfsprogramm sein, das nur ein einziges Konto sammelt, oder ein riesiges Sammel-Netzwerk aus Tausenden von Bots. In jedem dieser Fälle biete ich keine rechtliche Beratung an, und du solltest dich überhaupt erst selbst über deine ASF-Nutzung entscheiden. Ich verheimliche keine Informationen, die dir helfen könnten, z.B. die Tatsache, dass ASF einige Leute gesperrt hat, da ich keinen Grund dazu habe - es ist deine Entscheidung, was du mit diesen Informationen machen willst. Wenn du mich fragst - benutze deinen gesunden Menschenverstand, vermeide es, ein massives Sammel-Netzwerk von Bots zu erstellen, versende nicht Hunderte von Handelsangebote gleichzeitig, benutze immer die aktuelle ASF-Version und alles *sollte* gut sein. Jeder einzelne Vorfall dieser Art geschah **zufälligerweise** immer den Leuten, die Hunderte von Konten betrieben. Ob es nur ein Zufall oder ein tatsächlicher Faktor ist, das liegt bei dir zu entscheiden. Ich biete keine rechtliche Beratung an, sondern gebe dir nur meine Gedanken, die du nützlich finden kannst, oder ignoriere sie ganz und gar und arbeite nur mit den oben genannten Fakten.

* * *

### Welche Datenschutzinformationen gibt ASF weiter?

Eine detaillierte Erklärung findest du im Abschnitt **[Statistiken](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Statistics-de-DE)**. Du solltest es lesen, wenn du dich um deine Privatsphäre sorgst, z.B. wenn du dich fragst, warum Konten, die in ASF verwendet werden, unserer Steam-Gruppe beitreten. ASF sammelt keine sensiblen Informationen und gibt sie nicht an Dritte weiter.

* * *

## Sonstiges

* * *

### Ich verwende ein nicht unterstütztes Betriebssystem z.B. 32-Bit-Windows, kann ich trotzdem ASF V3 verwenden?

Ja, und diese Version wird in keiner Weise nicht unterstützt, nur nicht offiziell erstellt. Schaue dir den Abschnitt **[Kompatibilität](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-de-DE)** für generische Varianten an.

* * *

### ASF ist großartig! Kann ich spenden?

Ja, und wir freuen uns sehr zu hören, dass dir unser Projekt gefällt! Unter jeder **[Veröffentlichung](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** und auch **[auf der Hauptseite](https://github.com/JustArchiNET/ArchiSteamFarm)** findest du verschiedene Möglichkeiten eine Spende zu tätigen. Es ist gut zu wissen, dass wir neben allgemeinen Geldspenden auch Steam-Gegenstände akzeptieren, so dass dich nichts davon abhält, Skins, Keys oder einen kleinen Teil der Karten zu spenden, die du mit ASF gesammelt hast, wenn du möchtest. Vielen Dank im Voraus für deine Großzügigkeit!

* * *

### Ich verwende die Steam Parental PIN, um mein Konto zu schützen, muss ich sie irgendwo eingeben?

Ja, du musst es in der `SteamParentalCode` Bot-Konfigurationseigenschaft setzen. Dies liegt hauptsächlich daran, dass ASF auf viele geschützte Teile deines Steam-Kontos zugreift und es für ASF unmöglich ist, ohne sie zu arbeiten.

* * *

### Ich möchte nicht, dass ASF standardmäßig irgendwelche Spiele sammelt, aber ich möchte zusätzliche ASF-Funktionen verwenden. Ist das möglich?

Ja, du kannst `Paused` Bot-Konfigurationseigenschaft auf `true` setzen, um ASF mit angehaltenem Karten-Sammel-Modul zu starten, dann kannst du zusätzliche ASF-Funktionen nutzen, wie `GamesPlayedWhileIdle`.

* * *

### Kann ASF in das Benachrichtigungsfeld minimieren?

ASF ist eine Konsolenanwendung, es gibt kein zu minimierendes Fenster, da das Fenster von deinem Betriebssystem für dich erstellt wird. Du kannst jedoch auch ein beliebiges Drittanbieterprogramm verwenden, wie z.B. **[RBTray](http://rbtray.sourceforge.net)** für Windows, oder **[screen](https://linux.die.net/man/1/screen)** für Linux/OS X. Das sind nur Beispiele, es gibt viele andere Programme mit ähnlicher Funktionalität.

* * *

### Stellt die Verwendung von ASF die Berechtigung zum Erhalt von Booster Packs sicher?

**Ja**. ASF verwendet die gleiche Methode, um sich im Steam-Netzwerk wie der offizielle Client anzumelden, daher behält es auch die Möglichkeit, Booster Packs für die verwendeten Konten zu erhalten. Es ist jedoch nicht bestätigt, ob das Anmelden bei der Steam-Community tatsächlich erforderlich ist oder nicht, deshalb schlage ich vor, `OnlineStatus` auf `Online` zu belassen, zumindest bis jemand bestätigt, dass die Verwendung des Steam-Netzwerkes allein ausreichend ist. Es sollte so sein, aber ich bin mir nicht sicher.

* * *

### Gibt es eine Möglichkeit, mit ASF zu kommunizieren?

Ja, durch den Steam-Chat or mittels **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)**. Weitere Informationen findest du im Abschnitt **[Befehle](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**.

* * *

### Ich möchte bei der Übersetzung von ASF helfen, was muss ich tun?

Vielen Dank für dein Intresse! Alle Details hierzu kannst du in unserem Abschnitt **[Lokalisierung](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Localization)** finden.

* * *

### Ich habe nur ein (Haupt-)Konto zu ASF hinzugefügt, kann ich trotzdem Befehle per Steam-Chat senden?

**Ja**, es wird im Abschnitt **[Befehle](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands#notes)** erklärt. Du kannst dies über den Steam-Gruppen-Chat tun.

* * *

### ASF scheint zu funktionieren, aber ich bekomme keine Karten!

Die Sammelrate der Karten ist von Spiel zu Spiel unterschiedlich, wie du in **[Performance](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance-de-DE)** nachlesen kannst. Es dauert eine Weile, normalerweise **mehrere Stunden pro Spiel** und du solltest nicht erwarten, dass Karten in ein paar Minuten nach dem Start eines Programmes gesammelt werden. Wenn du sehen kannst, dass ASF aktiv den Kartenstatus überprüft und das Spiel wechselt, nachdem das aktuelle vollständig ungenutzt ist, dann funktioniert alles gut - du beziehst dich wahrscheinlich auf Inventarbenachrichtigungen, die von ASF automatisch durch `BotBehaviour` von `DismissInventoryNotifications` Bot Konfigurationseigenschaften abgewiesen werden. Siehe **[Konfiguration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-de-DE)** für Details.

* * *

### Wie kann ich den ASF-Prozess für mein Konto vollständig stoppen?

Beende einfach den ASF-Prozess, z.B. durch Anklicken von [X] unter Windows. Wenn du stattdessen einen bestimmten Bot deiner Wahl stoppen, aber andere am Laufen halten möchtest, dann schaue dir die `Enabled` **[Bot-Konfigurationseigenschaft](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-de-DE#bot-konfiguration)** an, oder den `stop` **[Befehl](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE)**. Wenn du stattdessen den automatischen Sammel-Prozess stoppen willst, aber ASF für dein Konto am Laufen halten möchtest, dann ist das das, wofür `Paused` **[Bot-Konfigurationseigenschaft](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-de-DE#bot-konfiguration)** und der `pause` **[Befehl](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE)** ist.

* * *

### Wie viele Bots kann ich mit ASF verwenden?

ASF als Programm hat keine Obergrenze für Bot-Instanzen, aber du bist immer noch durch das Steam-Netzwerk eingeschränkt. Derzeit kannst du bis zu 100-110 Bots mit einer einzigen IP und einer einzelnen ASF-Instanz laufen lassen. Es ist möglich, mehr Bots mit mehr IPs und mehr ASF-Instanzen zu betreiben. Bedenke, dass du, wenn du diese große Anzahl von Bots verwendest, deren Anzahl du selbst kontrollieren solltest (z.B. sicherstellen, dass sich alle tatsächlich anmelden und zur gleichen Zeit arbeiten). Außerdem ist zu beachten, dass das obige Limit im Allgemeinen von vielen internen Faktoren abhängt - es ist eher eine Annäherung als ein strenges Limit - Du wirst höchstwahrscheinlich in der Lage sein, mehr/weniger Bots als oben beschrieben auszuführen.

* * *

### Kann ich dann mehr ASF-Instanzen ausführen?

Du kannst so viele ASF-Instanzen auf einem Computer ausführen, wie du willst, vorausgesetzt, jede Instanz hat ihr eigenes Verzeichnis und ihre eigenen Konfigurationen, und das in einer Instanz verwendete Konto wird in einer anderen nicht verwendet. Allerdings solltest du dich fragen, warum du das tun willst. ASF ist optimiert, um ein dutzend, sogar hundert Konten gleichzeitig zu verwalten und diese dutzende Bots in ihren eigenen ASF-Instanzen zu starten, beeinträchtigt die Leistung, nimmt mehr Betriebssystem-Ressourcen in Anspruch und verursacht eine mangelnde Synchronisation zwischen den Bots - so ist es beispielsweise wahrscheinlicher, dass du das unten beschriebene Problem `InvalidPassword/RateLimitExceeded` erreichst, da Anmeldeanfragen nicht zwischen ASF-Instanzen synchronisiert werden. Deshalb ist es mein **Ratschlag**, immer maximal eine ASF-Instanz pro IP/Schnittstelle auszuführen. Wenn du mehr IPs/Interfaces hast, kannst du auf jeden Fall mehr ASF-Instanzen betreiben, wobei jede Instanz ihre eigene IP/Schnittstelle verwendet. Wenn du es nicht tust, ist es völlig sinnlos, mehr ASF-Instanzen zu starten, was nicht nur die Leistung beeinträchtigt und mehr Betriebssystem-Ressourcen (z.B. Speicher) beansprucht, sondern auch zu mangelnder Synchronisation und erhöhter Wahrscheinlichkeit führt, dass Probleme auftreten. Du wirst keine Vorteile haben, wenn du mehr als eine Instanz pro IP/Schnittstelle startest.

* * *

### Was bedeutet Status beim Einlösen eines Produktschlüssels?

Der Status zeigt an, wie sich der gegebene Einlösungsversuch ausgewirkt hat. Es gibt viele verschiedene Stati, darunter die gängigsten:

| Status                  | Beschreibung                                                                                                                                                                                                                                                     |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| NoDetail                | Der Status "OK" zeigt Erfolg an - der Produktschlüssel wurde erfolgreich eingelöst.                                                                                                                                                                              |
| Timeout                 | Das Steam-Netzwerk hat nicht in einer bestimmten Zeit reagiert, wir wissen nicht, ob der Produktschlüssel eingelöst wurde oder nicht (höchstwahrscheinlich wurde er das, aber du kannst es noch einmal versuchen).                                               |
| BadActivationCode       | Der angegebene Produktschlüssel ist ungültig (nicht als gültiger Produktschlüssel im Steam-Netzwerk erkannt).                                                                                                                                                    |
| DuplicateActivationCode | Der angegebene Produktschlüssel wurde bereits von einem anderen Konto eingelöst oder vom Entwickler/Verleger widerrufen.                                                                                                                                         |
| AlreadyPurchased        | Dein Konto besitzt bereits `packageID`, das mit diesem Produktschlüssel verbunden ist. Bedenke, dass dies nicht anzeigt, ob der Produktschlüssel `DuplicateActivationCode` ist oder nicht - nur dass er gültig ist und bei diesem Versuch nicht verwendet wurde. |
| RestrictedCountry       | Dies ist ein Produktschlüssel mit Regionssperre und dein Konto befindet sich nicht in der gültigen Region, in der du ihn einlösen darfst.                                                                                                                        |
| DoesNotOwnRequiredApp   | Du kannst diesen Produktschlüssel nicht einlösen, da dir ein anderes Spiel fehlt - hauptsächlich das Basis-Spiel, wenn du versuchst, das DLC-Paket einzulösen.                                                                                                   |
| RateLimited             | Du hast zu viele Einlösungsversuche durchgeführt und dein Konto wurde vorübergehend gesperrt. Versuche es in einer Stunde erneut.                                                                                                                                |

* * *

### Gehörst du zu einem Karten-Sammel-Dienst?

**Nein**. ASF ist mit keinem Dienst verknüpft und alle diese Behauptungen sind falsch. Dein Steam-Konto ist dein Eigentum und du kannst dein Konto auf jede erdenkliche Weise nutzen, aber Valve hat es klar in den **[offiziellen Nutzungsbedingungen](https://store.steampowered.com/subscriber_agreement/english)** erklärt:

> You are responsible for the confidentiality of your login and password and for the security of your computer system. Valve is not responsible for the use of your password and Account or for all of the communication and activity on Steam that results from use of your login name and password by you, by any person to whom you may have intentionally or by negligence disclosed your login and/or password in violation of this confidentiality provision.

ASF ist unter der freien Apache 2.0 Lizenz lizenziert, die es anderen Entwicklern ermöglicht, ASF weiter in ihre eigenen Projekte und Dienste zu integrieren. However, such third-party projects utilizing ASF are not guaranteed to be secure, reviewed, appropriate or legal according to **[Steam ToS](https://store.steampowered.com/subscriber_agreement/english)**. If you want to know our opinion, **we strongly encourage you to NOT share ANY of your account details with third-party services**. If such service turns out to be a **typical scam**, you'll be left alone with the problem, most likely without your Steam account and ASF won't take any responsibility for third-party services claiming to be safe and secure, because ASF team did not authorize neither reviewed any of those. In other words, **you're using them at your own risk, against our suggestion made above**.

In addition to that, official Steam ToS clearly states that:

> You may not reveal, share or otherwise allow others to use your password or Account except as otherwise specifically authorized by Valve.

It's your account and your choice. Just don't say that nobody warned you. ASF as a program meets all rules mentioned above, as you're not sharing your account details with anyone, and you're using the program for your own personal use, but any other "cards farming service" does require from you your account credentials, so it also violates the rule above (actually several of them). Like with Steam ToS evaluation, we're not offering any legal advice, and you should decide yourself if you want to use those services, or not - according to us **it directly violates Steam ToS** and might result in suspension if Valve finds out. Like pointed out above, **we strongly recommend to NOT use any of such services**.

* * *

## Probleme

* * *

### ASF doesn't detect game `X` as available for farming, yet I know it includes Steam trading cards!

There are two main reasons here. First and most obvious reason is the fact that you're referring to **Steam store** where given game is announced as card drops enabled game. This is **wrong** assumption, as it simply states that the game **has** card drops included, but not necessarily this function for that game is **enabled** right away. You can read more about this in **[official announcement](https://steamcommunity.com/games/593110/announcements/detail/1954971077935370845)**.

In short, card drops icon in Steam store doesn't mean anything, check your **[badge pages](https://steamcommunity.com/my/badges)** for confirmation whether a game has card drops enabled or not - this is also what ASF is doing. If your game doesn't appear on the list as a game with cards possible to drop, then this game is **not** possible to idle, regardless of reason.

Second issue is less obvious, and it's the situation when you can see that your game indeed is available with card drops on your badge page, yet it's not being idled by ASF right away. Unless you're hitting some other bug, such as ASF being unable to check badge pages (described below), it's simply a cache effect and on ASF side Steam is still reporting outdated badges page. This issue should solve itself sooner or later, when cache gets invalidated. There is also no way to fix this on our side.

Of course, all of that assumes that you're running ASF with default untouched settings, since you could also add this game to idling blacklist, use `IdleRefundableGames` of `false` and so on.

* * *

### Why playtime of games idled through ASF doesn't increase?

It does, but **not in real-time**. Steam records your playtime in fixed intervals and schedules update for it, but you're not guaranteed to have it updated immediately the moment you quit the session, let alone during such. If it was possible to skip playtime while idling cards then you can be sure that we'd have it implemented in ASF long time ago, and actually use it in default settings. But we don't, and we don't exactly because it's not possible - just because the playtime isn't updated in real-time doesn't mean that it's not recorded.

* * *

### Worin besteht der Unterschied zwischen einer Warnung und einem Fehler im Protokoll?

ASF schreibt eine Reihe von Informationen über verschiedene Logging-Ebenen in sein Protokoll. Our objective is to explain **precisely** what ASF is doing, including what Steam issues it has to deal with, or other problems to overcome. Most of the time not everything is relevant, this is why we have two major levels being used in ASF in terms of problems - a warning level, and error level.

General ASF rule is that warnings are **not** errors, therefore they should **not** be reported. A warning is an indicator to you that something potentially unwanted happen. Whether it was Steam not reacting, API throwing errors or your network connection being down - it's a warning, and it means we expected it to happen, so don't bother ASF development with it. Of course you're free to ask about them or get help by using our support, but you shouldn't assume that those are ASF errors worth reporting (unless we confirm otherwise).

Errors on the other hand indicate a situation that should not happen, therefore they're worth reporting as long as you made sure that it's not you who is causing them. If it's a common situation that we expect to happen, then it'll be converted to a warning instead. Otherwise, it's possibly a bug that should be corrected, not silently ignored, assuming it's not a result of your own technical issue. For example, removing core `ASF.json` file will throw an error, as ASF can't operate without that file, but it was you who removed it, so you should not report that error to us (unless you confirmed that ASF is wrong and the file is there).

In one TL;DR sentence - report errors, don't report warnings. You can still ask about warnings and receive help in our support sections.

* * *

### ASF can't start, the program window immediately closes after being launched!

If even `log.txt` is not being generated then you most likely forgot to install .NET Core prerequisites, as stated in **[setting up](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up#os-specific-setup)** guide. Other common problems might include trying to launch wrong ASF variant for your OS, or in other way missing native .NET Core runtime dependencies. If the console window closes too soon for you to read the message, then open independent console and launch ASF binary from there. For example on Windows, open ASF directory, hold `Shift`, right click inside the folder and choose "open command window here" (or powershell), then type into the console `.\ArchiSteamFarm.exe` and hit enter. This way you'll get precise message why ASF is not starting properly.

* * *

### ASF is kicking my Steam Client session while I'm playing! / `This account is logged on another PC`

This shows up as a message in Steam overlay that the account is being used somewhere else while you're playing. This issue can have two different reasons.

One reason is caused by broken packages (games) that specifically don't hold a playing lock properly, yet expect that lock to be possesed by the client. An example of such package would be Skyrim SE. Your Steam client launches the game properly, but that game doesn't register itself as being used. Because of that, ASF sees that it's free to resume the process, which it does, and that kicks you out of Steam network, as Steam suddenly detects that the account is being used in another place.

Second reason might come up if you're playing on your PC while ASF is waiting (especially on another machine) and you lose your network connection. In this case, Steam network marks you as offline and releases playing lock (like above), which triggers ASF (e.g. on another machine) into resuming farming. When your PC comes back online, Steam can't acquire playing lock anymore (that is now held by ASF, also similar to above) and shows the same message.

Both causes on the ASF side are actually very hard to workaround, as ASF simply resumes farming once Steam network informs it that account is free to be used again. This is what is happening normally when you close the game, but with broken packages this can happen immediately, even if your game is still running. ASF has no way to know whether you got disconnected, stopped playing a game or that you're still playing a game that doesn't hold playing lock appropriately.

The only proper solution to this problem is manually pausing your bot with `pause` before you start playing, and resuming it with `resume` once you're done. Alternatively you can just ignore the problem and act the same as if you played with offline Steam client.

* * *

### `Disconnected from Steam!` - I can't establish connection with Steam servers.

ASF can only **try** to establish connection with Steam servers, and it can fail due to many reasons, including lack of internet connection, Steam being down, your firewall blocking connection, third-party tools, incorrectly configured routes or temporary failures. You can enable `Debug` mode to check out more verbose log stating exact failure reasons, although usually it's simply caused by your own actions, such as using "CS:GO MM Server Picker" that blacklists a lot of Steam IPs, making it very hard for you to actually reach Steam network.

ASF will do its best to establish connection, which includes not only asking about updated list of servers but also trying another IP when last one fails, so if it's truly a temporary problem with some specific server or route, ASF will connect sooner or later. However, if you're behind firewall or in some other way unable to reach Steam servers, then obviously you need to fix it yourself, with potential help of `Debug` mode.

It's also possible that your machine is not able to establish connection with Steam servers using default protocol in ASF. You can alter protocols that ASF is permitted to use by modifying `SteamProtocols` global configuration property. For example, if you have problems reaching Steam with `TCP` protocol, then you can try `UDP` or `WebSocket`.

In a very unlikely situation of having incorrect servers being cached, for example because of moving ASF `config` folder from one machine to machine located in another country, deleting `ASF.db` in order to refresh Steam servers on next launch might help. Very often it's not needed and doesn't have to be done, as that list is automatically refreshed on first launch, as well as when the connection is established.

* * *

### `Konnte Abzeicheninformation nicht abfragen, wir versuchen es später erneut!`

Usually it means that you're using Steam parental PIN to access your account, yet you forgot to put it in ASF config. You must put valid PIN in `SteamParentalCode` bot config property, otherwise ASF will not be able to access most of web content, therefore will not be able to work properly. Head over to **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** in order to learn more about `SteamParentalCode`.

Other reasons might include temporary Steam problem, network issue or likewise. If issue won't solve itself after several hours and you're sure that you configured ASF appropriately, feel free to let us know about that.

* * *

### ASF is failing with `Request failed despite of 5 tries` errors!

Usually it means that you're using Steam parental PIN to access your account, yet you forgot to put it in ASF config. You must put valid PIN in `SteamParentalCode` bot config property, otherwise ASF will not be able to access most of web content, therefore will not be able to work properly. Head over to **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** in order to learn more about `SteamParentalCode`.

If parental PIN is not the reason, then this is a most common error, and you should get used to that - it simply means that ASF sent a request to Steam Network, and didn't get a valid response, in addition to that - in 4 retries. Usually it means that Steam is either down or is having some difficulties or maintenance - ASF is aware of such issues and you should not worry about them, unless they're happening constantly for longer than several hours, and other users do not have such problems.

How to check if Steam is being down? **[Steam Status](https://steamstat.us)** is an excellent source of checking if Steam **should be** up, if you notice errors, especially related to Community or Web API, then Steam is having difficulties, either leave ASF alone and let it do its job after a short while, or wait yourself.

That's however not always the case, as in some situations Steam issues might not be detected by Steam Status, for example such case happened when Valve broke HTTPS support for Steam Community 7th June 2016 - accessing **[SteamCommunity](https://steamcommunity.com)** through HTTPS was throwing an error. Therefore, do not blindly trust Steam Status either, it's best to check yourself if everything works as supposed to.

Lastly, if nothing helps you can always enable `Debug` mode and see yourself in ASF log why exactly requests are failing. For example, above HTTPS issue caused:

    <HTML><HEAD><TITLE>Error</TITLE></HEAD><BODY>
    An error occurred while processing your request.<p>
    

Which is clearly Steam issue and nothing to fix in ASF. You can always try to visit the link mentioned by ASF yourself and check if it works - if it doesn't, then you know why ASF can't access that either. If it does, and error doesn't go away after a day or two, it might be worth investigating and reporting.

Before doing that you should **make sure that the error is worth reporting in the first place**. If it's mentioned in this FAQ, such as trading-related issue, then that's out. If it's temporary issue that happened once or twice, especially when your network was unstable or Steam was down - that's out. However, if you were able to reproduce your issue several times in a row, across 2 days, restarted ASF as well as your machine in the process and made sure that there is no FAQ entry here to help resolve it, then this might be worth asking about.

* * *

### ASF seems to freeze and doesn't print anything on the console until I press a key!

You're most likely using Windows and your console has QuickEdit mode enabled. Refer to **[this](https://stackoverflow.com/questions/30418886/how-and-why-does-quickedit-mode-in-command-prompt-freeze-applications)** question on StackOverflow for technical explanation. You should disable QuickEdit mode by right clicking your ASF console window, opening properties, and unchecking appropriate checkbox.

* * *

### ASF kann keine Handelsangebote akzeptieren oder versenden!

Obvious thing first - new accounts start as limited. Until you unlock account by loading its wallet or spending 5$ in the store, ASF can't accept neither send trades using this account. In this case, ASF will state that inventory seems empty, because every card that is in it is non-tradable. It also won't be possible to receive any trade, as that part requires ASF to be able to fetch API key, and API key functionality is disabled for limited accounts. In short - trading is off for all limited accounts, no exceptions.

Next, if you do not use **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)**, it's possible that ASF in fact accepted/sent trade, but you need to confirm it via your e-mail. Likewise, if you use classic 2FA, you need to confirm the trade via your authenticator. Confirmations are **mandatory** now, so if you don't want to accept them by yourself, consider either adding or importing your authenticator into ASF 2FA.

Also notice that you can trade only with your friends, and people with known trade link. If you're trying to initiate Bot->Master trade, such as `loot`, then you need to either have your bot on your friendlist, or your `SteamTradeToken` declared in Bot's config. Make sure that the token is valid - otherwise, you won't be able to send a trade.

Lastly, remember that new devices have 7-days trade lock, so if you've just added your account to ASF, wait at least 7 days - everything should work after that period. That limitation includes **both** accepting **and** sending trades. It does not always trigger, and there are people who can send and accept trades instantly. Majority of the people are affected though, and the lock **will** happen, even if you can send and accept trades through your steam client on the same machine. Just wait patiently, there's nothing you can do to make it faster.

And finally, keep in mind that one account can have only 5 pending trades to another one, so ASF will fail to send trades if you have 5 (or more) pending ones from that one bot to accept already. This is rarely a problem, but it's also worth mentioning, especially if you set ASF to auto-send trades, yet you're not using ASF 2FA and forgot to actually confirm them.

Wenn nichts geholfen hat, kannst du jederzeit den `Debug` Modus aktivieren und selbst überprüfen, warum Anfragen fehlschlagen. Bitte bedenke, dass Steam die meiste Zeit Unsinn redet, und vorausgesetzt, dass der Grund keinen logischen Sinn ergibt oder sogar völlig falsch ist - wenn du dich entscheidest, diesen Grund zu interpretieren, stelle sicher, dass du ein angemessenes Wissen über Steam und seine Besonderheiten hast. Es ist auch durchaus üblich, dieses Problem ohne logischen Grund zu sehen, und die einzige vorgeschlagene Lösung in diesem Fall ist, das Konto erneut zu ASF hinzuzufügen (und wieder 7 Tage zu warten). Manchmal behebt sich dieses Problem auf *magische* Weise, genauso wie es auch kaputt geht. Allerdings ist es in der Regel nur entweder 7-Tage Handels-Sperre, temporäres Steam-Problem oder beides. Es ist am besten, ihm ein paar Tage vor der manuellen Überprüfung zu geben, was falsch ist, es sei denn, du hast den Drang, die wahre Ursache zu finden (und normalerweise wirst du gezwungen sein, trotzdem zu warten, weil Fehlermeldungen keinen Sinn ergeben und dir auch nicht im Geringsten helfen).

In jedem Fall kann ASF nur **versuchen**, eine ordnungsgemäße Anfrage an Steam zu senden, um das Handelsangebot anzunehmen/senden. Ob Steam diese Anfrage annimmt oder nicht, liegt außerhalb des Anwendungsbereichs von ASF, und ASF wird es nicht auf magische Weise zum Funktionieren bringen. Es gibt keinen Fehler im Zusammenhang mit dieser Funktion, und es gibt auch nichts zu verbessern, da die Logik außerhalb von ASF stattfindet. Deshalb, bitte nicht um die Behebung von Dingen, die nicht kaputt sind, und frage auch nicht, warum ASF keine Handelsangebote annehmen oder senden kann - **Ich weiß es nicht, und ASF weiß es auch nicht**. Entweder findest du dich damit ab oder du reparierst es selbst.

* * *

### Warum muss ich bei der Anmeldung jedesmal den 2FA/SteamGuard-Code eingeben? / `Abgelaufenen Anmelde-Schlüssel entfernt`

ASF verwendet Anmelde-Schlüssel (wenn du `UseLoginKeys` aktiviert hast), um die Anmeldeinformationen gültig zu halten, den gleichen Mechanismus, den Steam verwendet - 2FA/SteamGuard-Code ist nur einmal erforderlich. Aufgrund von Steam-Netzwerkproblemen und -schwierigkeiten ist es jedoch durchaus möglich, dass der Anmelde-Schlüssel nicht im Netzwerk gespeichert ist. Ich habe solche Probleme bereits nicht nur mit ASF, sondern auch mit dem normalen Steam-Client gesehen (eine Notwendigkeit, bei jedem Durchlauf Login + Passwort einzugeben, unabhängig von der Option "Remember me").

Du könntest `BotName.db` (+ `BotName.bin`, falls vorhanden) des betroffenen Kontos entfernen und versuchen, ASF wieder mit deinem Konto zu verbinden, aber das muss nicht zum Erfolg führen. Die eigentliche ASF-basierte Lösung besteht darin, deinen Authentifikator als **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-de-DE)** zu importieren - so kann ASF Codes automatisch generieren, wenn sie benötigt werden, und du musst sie nicht manuell eingeben. In der Regel löst sich das Problem nach einiger Zeit von selbst, so dass man es einfach abwarten kann. Natürlich kannst du GabeN auch nach einer Lösung fragen, denn ich kann das Steam-Netzwerk nicht zwingen, unsere Anmelde-Schlüssel zu akzeptieren.

Als Randbemerkung kannst du auch die Anmelde-Schlüssel mit `UseLoginKeys` Konfigurationseigenschaft auf `false` deaktivieren, aber du solltest das nur tun, wenn ASF eine vollständig automatisierte Möglichkeit hat, die erste Anmeldung durchzuführen. Dies ist derzeit nur mit gültigem `SteamPassword` und **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-de-DE)** möglich, da wir uns in diesem Fall überhaupt nicht auf Anmelde-Schlüssel verlassen müssen, da wir bei jeder Anmeldung die Anmeldeinformationen (Passwort und 2FA-Code) benötigt haben.

* * *

### Ich erhalte den Fehler: `Die Anmeldung bei Steam ist nicht möglich: InvalidPassword or RateLimitExceeded`

Dieser Fehler kann vieles bedeuten, unter anderem auch:

- Ungültige Login/Passwort-Kombination (offensichtlich)
- Abgelaufener Anmelde-Schlüssel, der von ASF für die Anmeldung verwendet wird
- Zu viele fehlgeschlagene Anmeldeversuche in kurzer Zeit (Anti-Bruteforce)
- Zu viele Anmeldeversuche in kurzer Zeit (Rate-Limiting)
- Erfordernis eines Captcha zum Anmelden (sehr wahrscheinlich durch die beiden obigen Gründe verursacht)
- Alle anderen Gründe, die das Steam-Netzwerk daran hindern könnten, sich anzumelden.

Im Falle von Anti-Bruteforce und Rate-Limiting wird das Problem nach einiger Zeit verschwinden, also warte einfach und versuche dich in der Zwischenzeit nicht anzumelden. Wenn du dieses Problem häufiger hast, ist es vielleicht ratsam, `LoginLimiterDelay` Konfigurationseigenschaft von ASF zu erhöhen. Übermäßige Programmneustarts und andere absichtliche/nicht absichtliche Anmeldeanforderungen werden bei diesem Problem definitiv nicht helfen, also vermeide es, wenn möglich.

Im Falle eines abgelaufenen Anmelde-Schlüssels wird ASF den alten entfernen und beim nächsten Anmelden nach einem neuen fragen (was erfordert, dass du einen 2FA-Code setzt, wenn dein Konto 2FA-geschützt ist). Wenn dein Konto ASF 2FA verwendet, wird ein Code generiert und automatisch verwendet. Wenn du dieses Problem öfters hast, ist es möglich, dass Steam aus irgendeinem Grund beschlossen hat, unsere Speicheranforderungen für den Anmelde-Schlüssel zu ignorieren, wie in der obigen Problematik erwähnt. Du könntest dieses Problem vermeiden, indem du die Anmelde-Schlüssel mit der Konfigurationseigenschaft `UseLoginKeys` überhaupt nicht verwendest, aber wir empfehlen nicht, diesen Weg zu gehen.

Und schlussendlich, wenn du eine falsche Kombination aus Login und Passwort verwendet hast, musst du dies natürlich korrigieren oder den Bot deaktivieren, der versucht, sich mit diesen Zugangsdaten zu verbinden. ASF kann nicht von selbst erraten, ob `InvalidPassword` ungültige Anmeldeinformationen bedeutet, oder einen der oben genannten Gründe, daher wird es weiter versuchen, bis es erfolgreich ist.

Bedenke, dass ASF über ein eigenes eingebautes System verfügt, um entsprechend auf Steam-Schrullen zu reagieren, irgendwann wird es sich verbinden und seine Arbeit wieder aufnehmen, daher ist es nicht erforderlich, etwas zu tun, wenn das Problem vorübergehend ist. Ein Neustart von ASF, um Probleme magisch zu beheben, wird die Situation nur verschlimmern (da der neue ASF den vorherigen ASF-Zustand nicht kennt, dass er sich nicht anmelden kann, und versucht, eine Verbindung herzustellen, anstatt zu warten), also solltest du das nicht tun, es sei denn, du weißt, was du tust.

Abschließend kann sich ASF, wie bei jeder Steam-Anfrage, nur **versuchen** mit den von dir angegebenen Zugangsdaten anmelden. Ob diese Anforderung erfolgreich ist oder nicht, liegt außerhalb des Umfangs und der Logik von ASF - es gibt keinen Fehler, und in dieser Hinsicht kann nichts behoben oder verbessert werden.

* * *

### `System.Threading.Tasks.TaskCanceledException: A task was canceled.`

Diese Warnung bedeutet, dass Steam nicht innerhalb einer bestimmten Zeit auf die ASF-Anfrage geantwortet hat. Normalerweise wird dies durch Steam-Netzwerkproblemen verursacht und hat keine Auswirkung auf ASF. In manchen Fällen ist es das gleiche wie wenn die Anfrage trotz 5 Versuchen fehlschlägt. Die Meldung dieses Problems macht meistens keinen Sinn, da wir Steam nicht zwingen können, auf unsere Anfragen zu reagieren.

* * *

### `System.Net.Http.WinHttpException: A security error occurred`

Dieser Fehler tritt auf, wenn ASF keine sichere Verbindung mit dem angegebenen Server herstellen kann, fast ausschließlich wegen des Misstrauens gegen ein SSL-Zertifikat.

In fast allen Fällen wird dieser Fehler durch ein **falsches Datum/Uhrzeit auf deiner Maschine** verursacht. Jedes SSL-Zertifikat hat ein Ausstellungsdatum und ein Verfallsdatum. Wenn dein Datum ungültig ist und außerhalb dieser beiden Grenzen liegt, kann dem Zertifikat nicht vertraut werden, da es als potentieller MITM-Angriff ausgenutzt werden kann und ASF weigert sich, eine Verbindung herzustellen.

Eine offensichtliche Lösung ist das Datum auf deiner Maschine entsprechend einzustellen. Es wird dringend empfohlen, eine automatische Datumssynchronisation zu verwenden, wie z.B. die unter Windows verfügbare native Synchronisation oder `ntpd` unter Linux.

Wenn du sichergestellt hast, dass das Datum auf deiner Maschine korrekt ist und der Fehler nicht verschwinden will, dann, angenommen, es ist kein temporäres Problem das bald verschwindet, könnten SSL-Zertifikate, denen dein System vertraut, veraltet oder ungültig sein. In diesem Fall solltest du sicherstellen, dass deine Maschine sichere Verbindungen herstellen kann, z.B. indem du über einen beliebigen Browser oder mit einem CLI-Tool wie `curl` überprüfst, ob du auf `https://github.com` zugreifen kannst. Wenn du bestätigt hast, dass dies ordnungsgemäß funktioniert, kannst du das Problem gerne in unserer Steam-Gruppe melden.

* * *

### ASF wird von meinem AntiVirus als Schadsoftware erkannt! Was ist hier los?

**Stelle sicher, dass du ASF von einer vertrauenswürdigen Quelle heruntergeladen hast**. Die einzige offizielle und vertrauenswürdige Quelle ist **[ASF-Veröffentlichungen](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** Seite auf GitHub (und das ist auch die Quelle für automatische ASF-Aktualisierungen) - **jede andere Quelle ist per Definition nicht vertrauenswürdig und könnte Schadsoftware enthalten, die von anderen Personen hinzugefügt wurde** - Du solltest keiner anderen Quelle per Definition vertrauen und sicherstellen, dass dein ASF immer von uns kommt.

Wenn du überprüft hast, dass ASF von einer vertrauenswürdigen Quelle heruntergeladen wurde, dann ist es höchstwahrscheinlich nur ein Fehlalarm. Dies **passierte in der Vergangenheit**, **passiert gerade jetzt**, und **wird in der Zukunft passieren**. Wenn du dir um die tatsächliche Sicherheit bei der Verwendung von ASF Sorgen machst, dann schlage ich vor, ASF mit vielen verschiedenen AVs nach der tatsächlichen Erkennungsrate zu scannen, zum Beispiel durch **[VirusTotal](https://www.virustotal.com)** (oder einem anderen Webdienst deiner Wahl).

Wenn die von dir verwendete AV-Software ASF fälschlicherweise als Schadsoftware erkennt, dann **ist es eine gute Idee, dieses Dateibeispiel an die Entwickler deiner AV-Software zu senden, damit sie es analysieren und ihre Erkennungsmaschine verbessern können**, da sie offensichtlich nicht so gut funktioniert, wie du denkst. Es gibt kein Sicherheitsproblem im ASF-Code, und es gibt auch nichts zu beheben, da wir keine Schadsoftware verteilen, daher macht es auch keinen Sinn uns diese Fehlalarme zu melden. Wir empfehlen dringend, ASF-Beispiele für weitere Analysen wie oben beschrieben zu versenden, aber wenn du dich nicht darum kümmern möchtest, dann kannst du ASF immer zu den AV-Ausnahmen hinzufügen, deine AV-Software deaktivieren oder einfach ein andere verwenden. Leider sind wir es gewohnt, dass AV-Softwares dumm sind, da ab und zu einige AV-Softwares ASF als Virus erkennen, was normalerweise sehr kurz andauert und schnell von den Entwicklern ausgebessert wird, aber wie wir oben erwähnt haben - **es passierte**, **passiert** und **wird immer passieren**. ASF enthält keinen schädlichen Code, du kannst ASF-Code überprüfen und sogar selbst den Quellcode kompilieren. Wir sind keine Hacker die den ASF-Code verschleiern, um sich vor AV-Heuristiken und Fehlalarmen zu verstecken, also erwarte nicht von uns, dass wir reparieren, was nicht kaputt ist - es gibt keinen "Virus", den wir beseitigen können.