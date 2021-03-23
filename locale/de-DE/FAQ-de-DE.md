# FAQ

Unser FAQ umfasst Standardfragen und Antworten, die Sie vielleicht haben. Für weniger häufig gestellte Fragen besuchen Sie bitte stattdessen unser **[erweitertes FAQ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Extended-FAQ-de-DE)**.

# Inhaltsverzeichnis

- [Allgemein](#allgemein)
- [Vergleich mit ähnlichen Programmen](#vergleich-mit-ähnlichen-programmen)
- [Sicherheit / Datenschutz / VAC / Sperren / Nutzungsbedingungen](#sicherheit--datenschutz--vac--sperren--nutzungsbedingungen)
- [Sonstiges](#sonstiges)
- [Probleme](#probleme)

* * *

## Allgemein

### Was ist ASF?

### Warum behauptet das Programm, dass es auf meinem Konto nichts zum Farmen gibt?

### Warum ist mein Konto begrenzt?

Bevor Sie versuchen herauszufinden, was ASF ist, sollten Sie sicherstellen, dass Sie verstehen, was Steam Sammelkarten sind und wie man sie erhält, was in der offiziellen FAQ **[hier](https://steamcommunity.com/tradingcards/faq)** gut beschrieben ist.

Kurz gesagt, Steam-Sammelkarten sind sammelbare Gegenstände, welche Sie sammeln können, wenn Sie ein bestimmtes Spiel besitzen, und können für die Herstellung von Abzeichen, den Verkauf auf dem Steam-Markt oder für jeden anderen Zweck Ihrer Wahl verwendet werden.

Hier werden noch einmal die Kernpunkte genannt, weil Leute im Allgemeinen nicht mit ihnen einverstanden sind und so tun als ob diese nicht existieren würden:

- **Sie müssen das Spiel auf dem Steam-Account besitzen, um für die dazu gehörigen Kartenfunde berechtigt zu sein. Spiele die über die Steam-Familienbibliothek geteilt werden zählen nicht.**
- **Sie können nicht unendlich lange sammeln; jedes Spiel hat eine feste Anzahl an Kartenfunde. Sobald Sie alle Karten gesammelt haben (ungefähr die Hälfte eines vollständigen Satzes), ist das Spiel kein Idling-Kandidat mehr. Es ist unwichtig, ob Sie die Karten verkauft, gehandelt, Abzeichen hergestellt oder lediglich vergessen haben, was damit geschehen ist; sobald die maximale Anzahl erreicht wurde, wird für dieses Spiel nicht weiter gesammelt.**
- **Sie können keine Karten von F2P-Spielen erhalten, ohne dabei Geld auszugeben. Dies beinhaltet dauerhafte F2P Spiele so wie Team Fortress 2 oder Dota 2. Der Besitz von F2P-Spielen allein garantiert keinen Kartenfund.**
- **Sie erhalten keine Karten auf [beschränkten Accounts](https://support.steampowered.com/kb_article.php?ref=3330-iagk-7663&l=german), ungeachtet der registrierten Spiele. Es war in der Vergangenheit möglich, aber dies ist nicht mehr der Fall.**
- **Bezahlte Spiele, die Sie während einer Promotion kostenlos erhalten haben, können nicht für Kartendrops eingetauscht werden, unabhängig davon, was auf der Shop-Seite angezeigt wird. Es war in der Vergangenheit möglich, aber dies ist nicht mehr der Fall.**

Steam-Sammelkarten sind folglich eine Belohnung dafür, dass Sie gekaufte Spiele spielen oder Geld in F2P-Spiele investieren. Sobald Sie ein Spiel lange genug spielen, werden alle Karten für dieses Spiel irgendwann im Inventar freigeschaltet. Damit können Sie beispielsweise ein Abzeichen vervollständigen (nach dem Erhalt der restlichen erforderlichen Karten) oder handeln.

Nun, da wir die Grundlagen von Steam erklärt haben, können wir ASF erklären. Das Programm selbst ist sehr komplex und dementsprechend relativ schwer zu verstehen. Anstatt hier alle technischen Details anzugeben, geben wir weiter unten lieber einen Überblick mit einfachen Erklärungen.

ASF meldet sich über unseren integrierten Mini-Steam-Client mit den von Ihnen angegebenen Anmeldeinformationen beim Steam-Konto an. Nach dem erfolgreichen Login durchsucht es Ihre **[Abzeichen](https://steamcommunity.com/my/badges)** , um herauszufinden welche Spiele noch gesammelt werden können (`X` Kartenfunde verbleibend). Nachdem alle Seiten analysiert und eine endgültige Liste der verfügbaren Spiele erstellt wurde, wählt ASF den effizientesten Sammel-Algorithmus aus und startet den Prozess. Der Prozess hängt von dem gewählten **[Karten-Sammel-Algorithmus](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance-de-DE#Leistung)** ab, aber normalerweise besteht er aus dem Spielen des geeigneten Spiels und der periodischen (plus bei jedem Kartendrop) Überprüfung, ob das Spiel bereits vollständig gesammelt wurde - wenn ja, kann ASF mit dem nächsten Spiel fortfahren, mit dem gleichen Verfahren, bis alle Spiele vollständig im gesammelt wurden.

Beachten Sie, dass die obige Erklärung vereinfacht ist und nicht Dutzende von zusätzlichen Features und Funktionen beschreibt die ASF anbietet. Mehr Informationen und Details finden Sie in **[unserem Wiki](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Home-de-DE)**. Ich habe versucht es einfach zu halten, damit es für alle verständlich ist, ohne technische Details einzubringen - fortgeschrittene Benutzer werden ermutigt, tiefer zu graben.

Als Programm bietet ASF einige Vorteile. Erstens, ASF muss keine Spieldateien herunterladen; es kann sofort Ihre Spiele sammeln. Zweitens ist es völlig unabhängig von Ihrem normalen Steam-Client. Sie müssen Steam-Client nicht laufen lassen oder gar installiert haben. Drittens ist es eine automatisierte Lösung - was bedeutet, dass ASF automatisch alles ohne weiteres Zutun erledigt, ohne dass Sie ihm sagen müssen, was er tun soll. Dies erspart Ihnen Ärger und Zeit. Letztendlich muss es das Steam-Netzwerk nicht durch Prozessemulation (die z.B. Idle Master verwendet) austricksen, da es direkt mit ihm kommunizieren kann. Es ist auch superschnell und leicht und ist eine erstaunliche Lösung für alle, die Karten ohne großen Aufwand bekommen wollen - es ist besonders nützlich, wenn man es im Hintergrund laufen lässt, während man etwas anderes macht oder sogar im Offline-Modus spielt.

All das oben Gesagte ist ganz nett, aber ASF hat auch einige technische Einschränkungen, die von Steam erzwungen werden. Es können weder Karten für Spiele gesammelt werden, die Sie nicht besitzen; geschweige denn über das Kartenlimit hinaus. Das Sammeln ist auch eingeschränkt, während Sie selbst ein Spiel spielen. All das sollte "logisch" sein, wenn man bedenkt wie ASF funktioniert. Aber man sollte beachten, dass ASF keine Superkräfte hat und nichts tun wird, was physisch unmöglich ist. Denken Sie auch daran, dass dies im Grunde genommen dasselbe ist, als ob Sie jemandem gesagt hätten, dass er sich von einem anderen PC aus in Ihr Konto einloggen und diese Spiele für Sie sammeln soll.

Zusammenfassend lässt sich sagen: ASF ist ein Programm, das Ihnen hilft, die Karten, für die Sie berechtigt sind, ohne großen Aufwand sammeln zu lassen. Es bietet auch einige andere Funktionen, aber wir bleiben vorerst bei dieser.

* * *

### Muss ich meine Zugangsdaten angeben?

**Ja**. ASF benötigt die Konto-Anmeldeinformationen auf die gleiche Weise wie der offizielle Steam-Client, da er die gleiche Methode für die Steam-Netzwerkinteraktion verwendet. Dies bedeutet jedoch nicht, dass Sie die Zugangsdaten in ASF-Konfigurationen eingeben müssen. Stattdessen können Sie ASF weiterhin mit `null`/leerem `SteamLogin` und/oder `SteamPassword` verwenden und bei Bedarf diese Daten für jeden ASF-Ausführung eingeben (sowie mehrere andere Zugangsdaten, siehe Konfiguration). Auf diese Weise werden die Daten nirgendwo gespeichert, aber natürlich kann ASF ohne Ihre Hilfe nicht automatisch starten. ASF bietet auch viele andere Möglichkeiten die **[Sicherheit](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security-de-DE#Sicherheit)** zu erhöhen. Wenn Sie ein fortgeschrittener Benutzer sind, sollten Sie diesen Teil des Wikis lesen. Falls dem jedoch nicht so ist, und Sie die Konto-Anmeldeinformationen nicht in die ASF-Konfigurationen einfügen möchten, dann belassen Sie es einfach dabei und geben Sie diese stattdessen erst bei Bedarf ein, z. B. wenn ASF danach fragt.

Bedenken Sie, dass ASF für den persönlichen Gebrauch bestimmt ist und die Zugangsdaten werden Ihren Computer niemals verlassen werden. Sie teilen diese auch mit niemandem. Dies erfüllt die **[Steam-Nutzungsbedingungen (AGB/ToS)](https://store.steampowered.com/subscriber_agreement/german)** - eine sehr wichtige Sache, die viele Leute vergessen. Sie schicken Ihre Daten nicht an unsere Server oder einen Dritten, sondern nur direkt an Steam-Server, die von Valve betrieben werden. Wir kennen Ihre Zugangsdaten nicht und können sie auch nicht für Sie wiederherstellen, unabhängig davon, ob Sie diese in Ihren Konfigurationen eingefügt haben oder nicht.

* * *

### Wie lange muss ich warten bis ich Karten erhalte?

**So lange wie es eben dauert** - ernsthaft. Jedes Spiel hat unterschiedliche Sammel-Schwierigkeiten, die vom Entwickler/Herausgeber festgelegt werden, und es liegt ganz an ihnen, wie schnell die Karten gesammelt werden. Die Mehrheit der Spiele folgt 1 Karte pro 30 Minuten Spielzeit, aber es gibt auch Spiele, welche sogar mehrere Stunden zu Spielzeit verlangen, bevor Sie eine Karte bekommen. Darüber hinaus kann es sein, dass das Konto daran gehindert wird, Karten von Spielen zu erhalten, die Sie noch nicht lange genug gespielt haben, wie im Abschnitt **[Leistung](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance-de-DE)** beschrieben. Versuchen Sie nicht zu erraten, wie lange ASF den gegebenen Titel sammeln soll - es liegt weder an Ihnen, noch an ASF dies zu entscheiden. Es gibt nichts was Sie tun können um es zu beschleunigen und es gibt keinen "Bug", der damit zusammenhängt, dass Karten nicht pünktlich gesammelt werden. Sie kontrollieren den Karten-Sammel-Prozess nicht, ebenso wenig wie ASF. Im besten Fall erhalten Sie durchschnittlich 1 Karte pro 30 Minuten. Im schlimmsten Fall erhalten Sie innerhalb der ersten 4 Stunden nach dem Start von ASF keine einzige Karte. Beide dieser Situationen sind normal und werden in unserem Abschnitt **[Leistung](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance-de-DE#Leistung)** beschrieben.

* * *

### Das Sammeln dauert so lange - Kann ich es irgendwie beschleunigen?

Das Einzige, was die Geschwindigkeit des Sammelns stark beeinflusst ist der gewählte **[Sammel-Algorithmus](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance-de-DE)** für die jeweilige Bot-Instanz. Alles andere hat einen unwesentlichen Effekt und beschleunigt das Sammeln nicht, wobei manche Dinge, wie zum Beispiel das mehrfache Starten von ASF es sogar **schlechter** machen. Wenn Sie wirklich den Drang haben, jede einzelne Sekunde des Sammel-Prozesses zu nutzen, dann erlaubt ASF Ihnen einige grundlegende Sammel-Variablen wie `FarmingDelay` zu optimieren - alle von ihnen sind in der **[Konfiguration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-de-DE#Konfiguration)** erklärt. Nichtsdestotrotz ist dieser Effekt unwesentlich und den entsprechenden Sammel-Algorithmus für das jeweilige Konto zu wählen ist die einzige ausschlaggebende Wahl, die die Geschwindigkeit schwer beeinflussen kann, während alles andere rein kosmetisch ist. Anstatt sich um Geschwindigkeit zu sorgen, sollten Sie einfach ASF starten und es seinen Job machen lassen - Ich kann versichern, dass es das auf die effektivste Art macht, die wir uns ausdenken konnten. Je weniger Sie sich sorgen, desto zufriedener werden Sie sein.

* * *

### Aber ASF sagte, dass das Sammeln etwa X Zeit in Anspruch nehmen wird!

ASF gibt Ihnen eine grobe Schätzung basierend auf der Anzahl der Karten, die Sie sammeln müssen, und dem gewählten Algorithmus - dies ist bei weitem nicht annähernd die tatsächliche Zeit, die für das Sammeln benötigt wird, die in der Regel länger ist als diese, da ASF nur den besten Fall annimmt, und ignoriert alle Steam-Netzwerk-Eigenarten, Internet-Trennungen, Überlastung der Steam-Server und ähnliches. Es ist nur als allgemeiner Indikator zu verstehen, wie lange man mit dem Sammeln von ASF rechnen kann, im besten Fall sehr oft, da die tatsächliche Zeit variiert, in einigen Fällen sogar deutlich. Wie oben erwähnt, versuchen Sie nicht zu erraten wie lange das Spiel gesammelt wird. Es liegt weder an Ihnen, noch an ASF dies zu entscheiden.

* * *

### Kann ASF auf meinem Android/Smartphone funktionieren?

ASF ist ein C#-Programm, das eine funktionierende Implementierung von .NET Core erfordert. Derzeit gibt es keinen nativen .NET Core Build für Android selbst, aber es gibt ordentliche und funktionierende Builds für Linux auf ARM-Architektur, sodass es durchaus möglich ist, so etwas wie **[Linux Deploy](https://play.google.com/store/apps/details?id=ru.meefik.linuxdeploy)** für die Installation von Linux zu verwenden, dann ASF in einem solchen Linux Chroot wie gewohnt zu verwenden.

Es ist sehr wahrscheinlich, dass wir in Zukunft mit einem funktionierenden .NET Core für Android rechnen können.

* * *

### Kann ASF Gegenstände aus Steam-Spielen wie CS:GO oder Unturned sammeln?

**Nein**, dies verstößt gegen die **[Steam-Nutzungsbedingungen](https://store.steampowered.com/subscriber_agreement/german)** (AGB) und Valve stellte sich mit der letzten Welle von Community-Sperren entschieden gegen das Sammeln von TF2-Gegenständen. ASF ist ein Steam-Karten-Sammelprogramm, kein Spiele-Gegenstände-Sammelprogramm - es hat keine Möglichkeit Spielegegenstände zu sammeln und es ist nicht geplant ein solches Feature in der Zukunft hinzuzufügen, hauptsächlich wegen der Verletzung der Steam-Nutzungsbedingungen. Bitte stellen Sie keine Fragen dazu. Das Allerbeste was Sie davon haben ist ein Report von einem genervten Benutzer Ihnen Probleme beschert. Das Gleiche gilt für alle anderen Arten des Sammelns, wie z.B. Kisten von CS:GO Übertragungen. ASF konzentriert sich ausschließlich auf Steam-Handelskarten.

* * *

### Kann ich mir aussuchen welche Spiele gesammelt werden sollen?

**Ja**, auf verschiedene Arten. Wenn Sie die Standardreihenfolge der Sammel-Warteschlange ändern möchten, dann können Sie `FarmingOrders` **[Bot Konfigurations-Eigenschaft](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-de-DE#bot-konfiguration)** dafür verwenden können. Wenn Sie bestimmte Spiele manuell auf die schwarze Liste setzen möchten, damit sie nicht automatisch gesammelt werden, können Sie die schwarze Liste benutzen, die mit `ib` **[Befehl](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE)** verfügbar ist. Wenn Sie alles sammeln möchten, aber einigen Spielen Vorrang vor allem anderen geben möchten, dann ist das das, wofür die Sammel-Prioritäts-Warteschlange, die mit `iq` **[Befehl](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE#Befehl)** verfügbar ist, verwendet werden kann. Und schließlich, wenn Sie nur bestimmte Spiele Ihrer Wahl sammeln möchten, dann können Sie `IdlePriorityQueueOnly` **[Bot Konfigurations-Eigenschaft](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-de-DE#bot-konfiguration)** verwenden, um dies zu erreichen, zusammen mit dem Hinzufügen ausgewählter Spiele zur Sammel-Prioritäts-Warteschlange.

Zusätzlich zur Verwaltung des oben beschriebenen Moduls für das automatische Karten-Sammeln können Sie ASF auch mit dem `play` **[-Befehl](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE#Befehl)** in den manuellen Sammel-Modus umschalten oder einige andere externe Einstellungen wie `GamesPlayedWhileIdle` **[Bot Konfigurations-Eigenschaft](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-de-DE#bot-konfiguration)** verwenden.

* * *

### Ich bin nicht an Kartenfunde interessiert. Kann ich stattdessen die Spielzeit erhöhen?

Ja, ASF erlaubt es Ihnen, dies auf mehrere Arten zu tun.

Der beste Weg um dies zu erreichen ist die Nutzung der **[`GamesPlayedWhileIdle`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#gamesplayedwhileidle)** -Konfigurationseigenschaft, welche ASF dazu veranlasst, (zuvor von Ihnen definierte) appIDs zu "spielen", sobald keine Karten mehr sammelbar sind. Falls Sie stetig Ihre Spiele "sammeln", dann können Sie dies entweder mit **[`IdlePriorityQueueOnly`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#idlepriorityqueueonly)** kombinieren, selbst wenn noch weitere Kartenfunde aus anderen Spielen verfügbar sind, wodurch wird ASF lediglich die Spiele mit verfügbaren Kartenfunde sammeln, die Sie explizit vorher bestimmt haben, oder mit **[`Paused`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-de-DE#paused)** das Sammel-Modul pausiert, bis Sie es wieder starten.

Alternativ können Sie den Befehl **[`play`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands#Befehle-1)** anwenden, damit ASF Ihre ausgewählten Spiele spielt. Beachten Sie jedoch, dass `play` nur für Spiele verwendet werden sollte, die Sie vorübergehend sammeln möchten, da es kein persistenter Status ist, was ASF in den Standardzustand (beispielsweise bei der Trennung vom Steam-Netzwerk) versetzt. Daher empfehlen wir Ihnen, `GamesPlayedWhileIdle`zu verwenden, es sei denn, Sie möchten Ihre ausgewählten Spiele tatsächlich nur für einen kurzen Zeitraum starten und dann zum allgemeinen Vorgehen zurückkehren.

* * *

### Ich bin ein Linux / OS X Benutzer. Kann ASF Spiele sammeln, die für mein Betriebssystem nicht verfügbar sind? Wird ASF 64-Bit-Spiele sammeln, wenn ich es auf einem 32-Bit-Betriebssystem ausführe?

Ja, ASF kümmert sich nicht einmal um das Herunterladen aktueller Spieldateien, sodass es mit allen Lizenzen funktioniert, die an Ihr Steam-Konto gebunden sind, unabhängig von Plattform oder technischen Anforderungen. Obwohl wir das nicht garantieren (es hat bei unserem letzten Versuch funktioniert), sollte es auch für regional gebundene (in bestimmten Regionen gesperrte) Spiele funktionieren, auch wenn Sie nicht in der passenden Region sind.

* * *

## Vergleich mit ähnlichen Programmen

* * *

### Ist ASF ähnlich wie Idle Master?

Die einzige Ähnlichkeit ist der allgemeine Zweck beider Programme, nämlich das Spielen von Steam-Spielen, um Karten zu erhalten. Alles andere, einschließlich der eigentlichen Sammel-Methode, der verwendeten Algorithmen, der Programmstruktur, der Funktionalität, der Kompatibilität, die mit dem Quellcode selbst endet, ist völlig unterschiedlich und diese beiden Programme haben nichts gemeinsam, auch nicht die Kerngrundlage (IM läuft auf .NET Framework, ASF auf .NET Core). ASF wurde geschaffen, um IM-Probleme zu lösen, die mit einer einfachen Programmcode-Editierung nicht zu lösen waren - deshalb wurde ASF von Grund auf neu geschrieben, ohne eine einzige Programmzeile oder gar eine allgemeine Idee des IM zu verwenden, weil dieser Code und diese Ideen von Anfang an völlig fehlerhaft waren. IM und ASF sind wie Windows und Linux - beide sind Betriebssysteme und beide können auf deinem PC installiert werden, aber sie haben fast nichts miteinander zu tun, abgesehen davon, dass sie einem ähnlichen Zweck dienen.

Aus diesem Grund sollten Sie ASF nicht mit IM vergleichen, basierend auf den Erwartungen an IM. Sie sollten ASF und IM als völlig unabhängige Programme mit eigenen exklusiven Funktionalitäten betrachten. Einige von ihnen überschneiden sich tatsächlich und man kann in beiden ein bestimmtes Feature finden, aber sehr selten, da ASF seinen Zweck mit einem völlig anderen Ansatz als IM erfüllt.

* * *

### Lohnt es sich ASF zu verwenden, wenn ich gerade Idle Master verwende und es für mich gut funktioniert?

**Ja**. ASF ist viel zuverlässiger und beinhaltet viele eingebaute Funktionen, die **entscheidend** sind, unabhängig davon, wie Sie sammeln, den IM einfach nicht anbietet.

ASF hat die richtige Logik für **unveröffentlichte Spiele** - IM wird versuchen, Spiele zu sammeln, die bereits Karten hinzugefügt haben, auch wenn sie noch nicht veröffentlicht wurden. Natürlich ist es nicht möglich, diese Spiele bis zum Veröffentlichungsdatum zu sammeln, sodass der Sammel-Prozess stecken bleibt. Dazu müssen Sie es entweder zur Blacklist hinzufügen, auf die Freigabe warten oder manuell überspringen. Keine dieser Lösungen ist gut, und alle erfordern Ihre Aufmerksamkeit. ASF überspringt (vorrübergehend) automatisch das Sammeln unveröffentlichter Spiele und kehrt später zu ihnen zurück, sobald sie es sind, um das Problem vollständig zu vermeiden und effizient damit umzugehen.

ASF hat auch die angemessene Logik für **series** (Videos). Es gibt viele Videos auf Steam, die Karten haben, aber mit falscher `appID` auf der Abzeichen-Seite angegeben sind, wie z.B. **[Double Fine Adventure](https://store.steampowered.com/app/402590)** - IM wird die falsche `appID` sammeln, was keine Karten ergibt und der Prozess stecken bleibt. Wieder einmal müssen Sie es entweder auf die schwarze Liste setzen oder manuell überspringen, wobei beide Ihre Aufmerksamkeit erfordern. ASF entdeckt automatisch die richtige `appID` für das Sammeln von Karten.

Darüber hinaus ist ASF **viel stabiler und zuverlässiger**, wenn es um Netzwerkprobleme und Steam-Macken geht - es funktioniert meistens und erfordert Ihre Aufmerksamkeit überhaupt nicht, sobald es einmal konfiguriert ist, während IM oft für viele Leute abbricht, zusätzliche Korrekturen erfordert oder trotzdem einfach nicht funktioniert. Es ist auch völlig abhängig vom Steam-Client, was bedeutet, dass es nicht funktionieren kann, wenn Ihr Steam-Client ernste Probleme hat. ASF funktioniert ordnungsgemäß, solange es mit dem Steam-Netzwerk verbunden werden kann, und erfordert nicht, dass der Steam-Client ausgeführt oder sogar installiert wird.

Dies sind drei **sehr wichtige** Punkte, warum Sie ASF verwenden solltest, da sie jeden der Steam-Karten sammelt direkt betreffen und es keine Möglichkeit gibt zu sagen "das betrifft mich nicht", da Steam-Wartungen und -Pannen jedem passieren. Es gibt Dutzende von zusätzlichen, unwichtigeren und wichtigeren Gründen, die Sie im Rest des FAQs erfahren können. Also kurz gesagt, **ja**, Sie sollten ASF verwenden, auch wenn Sie keine zusätzliche ASF-Funktion benötigen, die im Vergleich zu IM verfügbar ist.

Darüber hinaus wird IM offiziell nicht weiterentwickelt und könnte in Zukunft komplett kaputt gehen, ohne dass sich jemand die Mühe macht, es zu reparieren, wenn man bedenkt, dass es viel leistungsfähigere Lösungen gibt (abgesehen von ASF). IM funktioniert bereits für viele Leute nicht mehr, und diese Zahl nimmt nur zu, nicht ab. Du solltest es vermeiden, veraltete Software zu verwenden, nicht nur IM, sondern auch alle anderen veralteten Programme. Kein aktiver Verwalter bedeutet, dass sich niemand darum kümmert, ob es funktioniert oder nicht, niemand verifiziert, ob es funktioniert, und **niemand ist für seine Funktionalität** verantwortlich, was für die Sicherheit entscheidend ist. Es reicht aus, dass ein kritischer Fehler auftritt, der tatsächlich Probleme mit der Steam-Infrastruktur verursacht - ohne dass jemand sie behebt. Steam kann eine weitere Sperrwelle auslösen, in der Sie betroffen werden, ohne dass Ihnen bewusst ist, dass dies ein Problem ist wie es bereits anderen Nutzern (mit veralteten ASF-Versionen) vor Ihnen erging.

* * *

### Welche interessanten Features bietet ASF, die Idle Master nicht bietet?

Es hängt davon ab, was für Sie "interessant" ist. Wenn Sie vorhaben, mehr als ein Konto zu nutzen, dann ist die Antwort bereits offensichtlich, da ASF es Ihnen erlaubt, auf Allen zu Sammeln mit einer übergeordneten Gesamtlösung, Ressourcen zu sparen, Probleme mit Ärger und Kompatibilität zu lösen. Wenn Sie allerdings diese Frage stellen, dann haben Sie höchstwahrscheinlich nicht diesen speziellen Bedarf, also lass uns andere Vorteile bewerten, die für ein einzelnes Konto gelten, das in ASF verwendet wird.

In erster Linie haben Sie einige eingebaute Funktionen, wie **[oben](#lohnt-es-sich-asf-zu-verwenden-wenn-ich-gerade-idle-master-verwende-und-es-für-mich-gut-funktioniert)** erwähnt, die der Kern für das Sammeln sind, unabhängig vom Endziel, und sehr oft reicht das allein schon aus, um ASF zu verwenden. Aber das wissen Sie bereits, also lassen Sie uns zu einigen weiteren interessanten Features kommen:

- **Sie können offline sammeln** (`OnlineStatus` von `Offline` Feature). Das Offline-Sammeln ermöglicht es Ihnen, den Steam-In-Game-Status komplett zu überspringen, was es ermöglicht, mit ASF zu sammeln, während Sie gleichzeitig "Online" auf Steam sind, ohne dass Ihre Freunde überhaupt merken, dass ASF in Ihrem Namen ein Spiel spielt. Dies ist eine überlegene Funktion, da es Ihnen erlaubt, online im Steam-Client zu bleiben, ohne Ihre Freunde mit ständigen Spieländerungen zu belästigen oder sie in die Irre zu führen, dass Sie ein Spiel spielen, während Sie es in Wirklichkeit nicht sind. Allein dieser Punkt macht es sinnvoll, ASF zu verwenden, wenn man seine eigenen Freunde respektiert, aber es ist nur der Anfang. Es ist auch gut zu wissen, dass diese Funktion nichts mit den Privatsphäre-Einstellungen von Steam zu tun hat - wenn Sie das Spiel selbst starten, dann werden Sie den Freunden richtig als In-Game angezeigt, sodass nur der ASF-Teil unsichtbar wird und das Konto überhaupt nicht beeinträchtigt werden.

- **Sie konnen erstattungsfähige Spiele überspringen** (`IdleRefundableGames` Feature). ASF hat eine eingebaute Logik für rückerstattungsfähige Spiele und Sie können ASF so konfigurieren, dass es keine rückerstattungsfähige Spiele automatisch sammelt. Auf diese Weise können Sie selbst beurteilen, ob ein neu gekauftes Spiel aus dem Steam-Shop Ihr Geld wert war, ohne dass ASF versucht, so schnell wie möglich Karten zu sammeln. Wenn Sie es für 2+ Stunden spielen, oder 2 Wochen seit Ihrem Kauf vergangen sind, dann wird ASF mit diesem Spiel fortfahren, da es nicht mehr rückerstattungsfähig ist. Bis dahin haben Sie die volle Kontrolle, ob Sie es mögen oder nicht, und können es bei Bedarf leicht zurückerstatten, ohne dass Sie dieses Spiel manuell auf die schwarze Liste setzen oder ASF für die gesamte Dauer nicht verwenden müssen.

- **Sie können neue Gegenstände automatisch als erhalten markieren** (`BotBehaviour` von `DismissInventoryNotifications` Feature). Das Sammeln mit ASF führt dazu, dass Ihr Konto neue Karten erhält. Sie wissen bereits, dass dies geschehen wird, also lassen ASF diese unnötige Benachrichtigung für Sie lesen und sicherstellen, dass nur wichtige Dinge Ihre Aufmerksamkeit gewinnen. Natürlich nur wenn Sie dies wünschen.

- **Sie können automatisch Karten von Steam Events** erhalten (`AutoSteamSaleEvent` Feature). ASF ermöglicht es Ihnen, die Entdeckungsliste während des Steam-Sales zu automatisieren, aber natürlich nur, wenn Sie das auch möchten. Dies spart jeden Tag enorm viel Zeit, während der Steam-Sale stattfindet, und stellt sicher, dass Sie die täglichen Karten nie wieder verpassen werden.

- **Sie können die bevorzugte Sammel-Reihenfolge mit mehr verfügbaren Optionen anpassen** (`FarmingOrders` Feature). Vielleicht möchten Sie zuerst Ihre neu gekauften Spiele sammeln? Oder die ältesten? Nach der Anzahl der Karten? Abzeichen-Level, die Sie bereits hergestellt haben? Gespielte Stunden? Alphabetisch? Nach den AppIDs? Oder komplett zufällig? Das liegt ganz bei Ihnen.

- **ASF kann Ihnen helfen, Ihre Sets zu vervollständigen** (`TradingPreferences` mit `SteamTradeMatcher` Feature). Mit etwas fortgeschrittenerem Tüfteln können Sie Ihrer ASF in einen vollwertigen Benutzer-Bot umwandeln, der automatisch **[STM](https://www.steamtradematcher.com)** Handelsangebote akzeptiert und Ihnen jeden Tag hilft, Ihre Sets ohne jegliche Benutzerinteraktion zu ergänzen. ASF enthält sogar ein eigenes ASF-2FA-Modul, mit dem Sie einen mobilen Steam-Authentifikator importieren und den gesamten Prozess vollständig automatisieren können, auch bei der Annahme von Bestätigungen. Oder möchten Sie vielleicht manuell akzeptieren und ASF nur diese Handelsangebote für Sie vorbereiten lassen? Auch das liegt wieder einmal ganz bei Ihnen.

- **ASF kann Produktschlüssel im Hintergrund für Sie einlösen** (**[Hintergrundproduktschlüsselaktivierer](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer-de-DE)** Feature). Vielleicht haben Sie hundert Produktschlüssel aus verschiedenen Bundles wo Sie zu faul sind alle einzulösen, da Sie bei jedem durch ein paar Fenster gehen und den Steam-Bedingungen immer wieder zustimmen müssen. Warum nicht einfach die Liste der Produktschlüssel zu ASF kopieren und es seine Arbeit machen lassen? ASF löst automatisch alle diese Produktschlüssel im Hintergrund ein und stellt Ihnen entsprechende Ausgaben zur Verfügung, damit Sie wissen, wie sich jeder Einlösungsversuch ausgewirkt hat. Außerdem, wenn Sie hunderte von Produktschlüssel haben, werden Sie garantiert früher oder später von Steam rate-limited. ASF weiß auch davon; wird geduldig warten, bis dieses Anfragelimit verschwindet, und dort weitermachen, wo es aufgehört hat.

Wir könnten nun mit dem gesamten **[ASF-Wiki](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Home-de-DE)** fortfahren und auf jedes einzelne Feature des Programms hinweisen, aber wir müssen irgendwo eine Grenze ziehen. Das war's- dies ist eine Liste von Features, die Sie als ASF-Benutzer genießen können, wo nur eine davon leicht als ein Hauptgrund angesehen werden könnte, nie zurückzuschauen, und wir haben tatsächlich **eine Menge** von ihnen aufgelistet, mit noch mehr, über die Sie in unserem restlichen Wiki mehr erfahren können. Wir sind nicht einmal ins Detail gegangen, mit Dingen wie ASF's API, die es Ihnen ermöglicht, einen eigenen Befehle zu skripten, oder glorreiches Bots-Management, da wir es einfach halten wollten.

* * *

### Ist ASF schneller als Idle Master?

**Ja**, obwohl die Erklärung ziemlich kompliziert ist.

Bei jedem neuen Prozess, der in Ihrem System gestartet und beendet wird, sendet der Steam-Client automatisch eine Anfrage mit allen Spielen, die Sie gerade spielen - so kann das Steam-Netzwerk Stunden berechnen und Karten ausgeben. Das Steam-Netzwerk zählt jedoch die Spielzeit in Intervallen von 1 Sekunde, und das Senden einer neuen Anfrage setzt den aktuellen Status zurück. Mit anderen Worten, wenn Sie alle 0,5 Sekunden einen neuen Prozess starten/abbrechen würden, würden Sie nie eine Karte sammeln, weil jeder 0,5-Sekunden-Steam-Client eine neue Anfrage senden würde und das Steam-Netzwerk nie auch nur 1 Sekunde Spielzeit zählen würde. Außerdem ist es aufgrund der Funktionsweise des Betriebssystems durchaus üblich, neue Prozesse zu erkennen, die ohne dass Sie überhaupt etwas tun (also selbst wenn Sie nichts auf einem PC unternehmen). Es gibt viele Prozesse, die immer noch im Hintergrund arbeiten und andere Prozesse die ganze Zeit über auslösen/beenden. Idle Master basiert auf dem Steam Client, sodass dieser Mechanismus Sie beeinflusst, wenn Sie ihn benutzen.

ASF basiert nicht auf dem Steam-Client, sondern hat eine eigene Steam-Client-Implementierung. Dank dessen ist das, was ASF tut, nicht das Auslösen eines Prozesses, sondern das Senden einer echten Anfrage an das Steam-Netzwerk, dass wir mit dem Spielen eines Spiels begonnen haben. Dies ist die gleiche Anfrage, die vom Steam-Client gesendet wird, aber da wir die tatsächliche Kontrolle über den ASF-Steam-Client haben, brauchen wir keine neuen Prozesse zu starten, und wir imitieren den Steam-Client nicht bezüglich der Sendeanforderung bei jeder Prozessänderung, sodass der oben beschriebene Mechanismus uns nicht betrifft. Dank dessen unterbrechen wir nie das 1-Sekunden-Intervall auf Steam, was unsere Sammel-Geschwindigkeit erhöht.

* * *

### Aber ist der Unterschied wirklich spürbar?

Nein. Die Unterbrechungen, die mit einem normalen Steam-Client und dem Idle Master auftreten, haben nur einen geringen Einfluss auf die Kartendrops, sodass es kein spürbarer Unterschied ist, der ASF überlegen machen würde.

Jedoch **gibt es** einen Unterschied, und Sie können deutlich feststellen, dass, je nachdem, wie beschäftigt Ihr Betriebssystem ist, Karten **werden** schneller gesammelt, von ein paar Sekunden auf sogar ein paar Minuten, wenn Sie extrem wenig Glück haben. Obwohl ich nicht in Betracht ziehen würde, ASF nur zu verwenden, weil es Karten schneller sammelt, da sowohl ASF als auch Idle Master von der Funktionsweise des Steam-Netzes beeinflusst werden, interagiert ASF einfach effektiver mit dem Steam-Netz, während Idle Master nicht kontrollieren kann, was der Steam-Client tatsächlich tut (also ist es nicht die Schuld von Idle Master, sondern die des Steam-Clients selbst).

* * *

### Kann ASF mehrere Spiele auf einmal sammeln?

**Ja**, obwohl ASF besser weiß, wann dieses Feature zu verwenden ist, basierend auf dem ausgewählten **[Karten-Sammel-Algorithmus](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance-de-DE)**. Die Kartendrops-Rate beim Sammeln mehrerer Spiele ist nahe Null, deshalb verwendet ASF mehrere Spiele, die ausschließlich für Stunden sammeln, um `HoursUntilCardDrops` schneller zu überwinden, für bis zu `32` Spiele auf einmal. Deshalb solltest Sie sich auch auf den Teil der Konfiguration von ASF konzentrieren und den Algorithmus entscheiden lassen, was der optimale Weg ist, um das Ziel zu erreichen - was Sie für richtig halten, ist in Wirklichkeit nicht unbedingt richtig, denn das Sammeln mehrerer Spiele auf einmal wird Ihnen keine Kartendrops bescheren.

* * *

### Kann ASF schnell durch die Spiele springen?

**Nein**, ASF unterstützt nicht und fördert auch nicht die Verwendung von **[Steam-Fehlern](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance-de-DE#steam-pannen)**.

* * *

### Kann ASF jedes Spiel automatisch für X Stunden spielen, bevor Karten gesammelt werden?

**Nein**, der Sinn der Systemumstellung von Steam-Karten war es, mit falschen Statistiken und Geisterspielern zu kämpfen. ASF wird nicht mehr als nötig dazu beitragen, denn das Hinzufügen eines solchen Features ist nicht geplant und wird nicht geschehen. Wenn Ihr Spiel auf übliche Weise Kartendrops erhält, wird ASF diese so schnell wie möglich sammeln.

* * *

### Kann ich ein Spiel spielen während ASF am Sammeln ist?

**Nein**. ASF hat im Gegensatz zu IM einen unabhängigen Steam-Client integriert, und das Steam-Netzwerk erlaubt es nur **einem Steam-Client auf einmal**, ein Spiel zu spielen. Sie können die ASF-Verbindung jedoch jederzeit trennen, indem Sie ein Spiel starten (und auf "OK" klicken, wenn Sie gefragt werden, ob das Steam-Netzwerk einen anderen Client trennen soll) - ASF wird dann geduldig warten, bis Sie fertig sind, um den Prozess danach fortsetzen. Alternativ können Sie immer noch (wann immer Sie wollen) im Offline-Modus spielen, wenn das für Sie befriedigend ist.

Beachten Sie, dass die Drop-Rate der Karten, wenn Sie mehrere Spiele spielen, ohnehin nahe bei 0 liegt, daher gibt es keine direkten Vorteile, wenn Sie das mit IM machen können, während es starke Vorteile gibt, wenn Sie sich nicht in andere Spiele einmischen, die mit ASF gestartet wurden, was z.B. für VAC entscheidend ist.

* * *

## Sicherheit / Datenschutz / VAC / Sperren / Nutzungsbedingungen

* * *

### Kann ich eine VAC-Sperre bekommen, wenn ich ASF nutze?

Nein, das ist nicht möglich, da ASF (im Gegensatz zu Idle Master oder SAM) in keiner Weise den Steam-Client oder seine Prozesse stört. Es ist physisch unmöglich, eine VAC-Sperre für die Verwendung von ASF zu erhalten, selbst wenn man auf gesicherten Servern spielt, während ASF läuft - das liegt daran, dass **ASF nicht einmal verlangt, dass der Steam-Client überhaupt installiert ist**, um ordnungsgemäß zu funktionieren. ASF ist das einzige Sammel-Programm, das derzeit die VAC-Freiheit garantieren kann.

* * *

### Kann die Verwendung von ASF verhindern, dass ich auf VAC-geschützten Servern spiele kann, wie **[hier](https://support.steampowered.com/kb_article.php?ref=2117-ilzv-2837&l=german)** angegeben?

ASF benötigt keinen Steam-Client, der ausgeführt oder gar installiert wird. Nach diesem Konzept sollte es **keine** VAC-bezogenen Probleme verursachen, da ASF garantiert, dass der Steam-Client und alle seine Prozesse nicht gestört werden - das ist der wichtigste Punkt, wenn es um die VAC-freie Garantie geht, die ASF bietet.

Nach Angaben von Benutzern und nach bestem Wissen und Gewissen ist dies im Moment der Fall, da niemand irgendwelche Probleme wie im obigen Link während der Verwendung von ASF gemeldet hat. Wir konnten das obige Problem nicht mit ASF reproduzieren, während wir es mit Idle Master klar reproduzieren konnten.

Bedenken Sie jedoch, dass Valve irgendwann immer noch ASF zur schwarzen Liste hinzufügen könnte, aber es ist ein kompletter Unsinn, denn selbst wenn sie das tun, könnten Sie dennoch VAC-gesicherte Spiele von Ihrem PC aus spielen und ASF gleichzeitig verwenden, z.B. auf einem Server, also bin ich ziemlich sicher, dass sie sehr gut wissen, dass ASF kein verdächtiger VAC-technischer Verdächtiger sein sollte, und sie werden uns das Leben nicht schwerer machen, indem ASF grundlos auf die schwarze Liste gesetzt wird. Im schlimmsten Fall werden Sie jedoch nicht spielen können, wie oben erwähnt, weil die VAC-freie Garantie von ASF immer noch da ist, unabhängig davon, ob Steam die ASF-Binärdatei auf die schwarze Liste setzt oder nicht (und Sie können ASF immer noch auf jeder anderen Maschine starten, ohne dass ein Steam-Client überhaupt installiert ist). Im Moment gibt es keinen Grund, irgendetwas davon zu tun, und wir hoffen, dass es so bleibt.

* * *

### Ist es sicher?

Wenn du fragst, ob ASF als Software sicher ist, was bedeutet, dass es deinem Computer keinen Schaden zufügt, deine privaten Daten nicht stiehlt, Viren oder ähnliches installiert - ist es sicher. ASF ist frei von Malware, Datendiebstahl, Kryptowährungsminern und jedem (und allen) anderen zweifelhaften Verhalten, das vom Benutzer als bösartig oder unerwünscht angesehen werden kann. Darüber hinaus haben wir einen speziellen Abschnitt **[Statistik](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Statistics-de-DE)**, der unsere Datenschutzerklärung und das ASF-Verhalten behandelt, das über das hinausgeht, was Sie mit dem Programm selbst konfiguriert haben.

Unser Quelltext ist Open-Source, und verteilte Binärdateien werden immer aus **[öffentlich verfügbaren Quellen](https://de.wikipedia.org/wiki/Open-source)** von **[automatisierten und vertrauenswürdigen kontinuierlichen Integrationssystemen](https://en.wikipedia.org/wiki/Build_automation)** und nicht einmal von Entwicklern selbst erstellt. Jeder Build ist reproduzierbar, indem er unserem Build-Skript folgt, und wird genau dasselbe ergeben, **[deterministisch](https://en.wikipedia.org/wiki/Deterministic_system)** IL (binärer) Code. Wenn Sie aus irgendeinem Grund unseren Builds nicht vertrauen, können Sie ASF jederzeit aus dem Quelltext kompilieren und verwenden, einschließlich aller Bibliotheken, die ASF verwendet (wie SteamKit2), die ebenfalls Open-Source sind.

Am Ende ist es jedoch immer eine Frage des Vertrauens zu den Entwicklern einer Anwendung, also sollten Sie selbst entscheiden, ob Sie ASF für sicher halten oder nicht und Ihre Entscheidung möglicherweise mit den oben genannten technischen Argumenten unterstützen. Glauben Sie nicht blind meinen Worten - überprüfen Sie es selbst, denn das ist der einzige Weg, um sicher zu gehen.

* * *

### Kann ich dafür gesperrt werden?

Um diese Frage zu beantworten, sollten wir einen genaueren Blick auf die **[Steam-Nutzungsbedingungen (AGB)](https://store.steampowered.com/subscriber_agreement/german)** werfen. Steam verbietet nicht die Verwendung mehrerer Konten, vielmehr **[erlauben sie es](https://support.steampowered.com/kb_article.php?ref=8625-WRAH-9030#share)**, da Sie denselben mobilen Authentifikator für mehr als ein Konto verwenden können. Was es jedoch nicht erlaubt, ist die gemeinsame Nutzung von Konten mit anderen Personen, aber das tun wir hier nicht.

Der einzige wirkliche Punkt, der ASF betrifft, ist der folgende:

> *Sie dürfen keine Cheats, Automationssoftware (Bots), Modifikationen, Hacks oder andere nicht autorisierte Software von Drittanbietern verwenden, um den Prozess des Abonnements Marketplace zu ändern oder zu automatisieren. *

Die Frage ist, was ist eigentlich der "Subscription Marketplace-Prozess". Wie wir lesen können:

> *Ein Beispiel eines Abonnement-Marktplatzes ist der Steam Community Market</p> </blockquote> 
> 
> Wir ändern oder automatisieren den Prozess des Abonnenten-Marktplatzes nicht, wenn wir unter Abonnenten-Marktplatz den Markt der Steam-Community oder den Steam-Shop verstehen. Jedoch...
> 
> > *Valve ist berechtigt, Ihr Benutzerkonto oder ein bestimmtes Abonnement/bestimmte Abonnements in den folgenden Fällen jederzeit zu löschen: (a) Valve stellt generell die Bereitstellung von Abonnements für Abonnenten in einer vergleichbaren Situation ein, oder (b) Sie verstoßen gegen Bedingungen der vorliegenden Vereinbarung (einschließlich etwaiger Abonnementbedingungen oder Nutzungsrichtlinien).</p> </blockquote> 
> > 
> > Daher ist ASF, wie bei jeder Steam-Software, nicht von Valve autorisiert und ich kann nicht garantieren, dass Sie nicht gesperrt werden, wenn Valve plötzlich entscheidet, dass sie Konten mit ASF jetzt sperren. Dies ist angesichts der Tatsache, dass ASF auf mehr als einer halben Million Steam-Konten verwendet wird, äußerst unwahrscheinlich, aber dennoch eine Möglichkeit, unabhängig von der tatsächlichen Wahrscheinlichkeit.
> > 
> > Besonders weil:
> > 
> > > Hinsichtlich jeglicher Abonnements sowie vertragsgegenständlichen Inhalte und Leistungen, die nicht der Urheberschaft von Valve entstammen, nimmt Valve keine Überwachung derartiger Inhalte von Drittanbietern, die auf der Steam-Plattform oder über sonstige Quellen verfügbar sind, vor. *Valve übernimmt in Bezug auf derartige Inhalte von Drittanbietern keinerlei Verantwortung oder Haftung. *Bestimmte Anwendungssoftware von Drittanbietern kann von Unternehmen für geschäftliche Zwecke verwendet werden – Sie dürfen derartige Software über die Steam-Plattform jedoch ausschließlich für Ihren privaten Gebrauch erwerben.</p> </blockquote> 
> > > 
> > > Allerdings erkennt Valve klar an, dass es "Steam-Sammler" gibt, wie **[hier](https://support.steampowered.com/kb_article.php?ref=2117-ilzv-2837&l=german)** angegeben, also wenn Sie mich fragen, bin ich mir ziemlich sicher, dass sie, wenn sie nicht einverstanden mit ihnen wären, bereits etwas tun würden, anstatt darauf hinzuweisen, dass sie VAC-technisch Probleme verursachen könnten. Das Schlüsselwort hier ist **Steam** Sammler, zum Beispiel ASF, und nicht **Spiel** Sammler.
> > > 
> > > Bitte beachten Sie, dass das oben genannte lediglich unsere Interpretation der **[Steam ToS](https://store.steampowered.com/subscriber_agreement/german)**, sowie diverser anderer Punkte ist. ASF ist durch die Apache 2.0 Lizenz lizenziert, die welche eindeutig vorschreibt:
> > > 
> > > > *Unless required by applicable law or agreed to in writing, ASF is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.*
> > > 
> > > Sie verwenden diese Software auf eigene Gefahr. Es ist sehr unwahrscheinlich, dass man dafür gesperrt wird, aber dies geschehen, kann man nur sich selbst die Schuld dafür geben.
> > > 
> > > * * *
> > > 
> > > ### Wurde jemand dafür gesperrt?
> > > 
> > > **Ja**, wir hatten bereits ein paar Vorfälle, die zu einer Art Steam-Sperre führten. Ob ASF selbst die Ursache war oder nicht, ist eine ganz andere Geschichte, die wir wahrscheinlich nie erfahren werden.
> > > 
> > > Ein Fall war von jemandem mit über 1000+ Bots, der vom Handel ausgeschlossen wurde (zusammen mit allen Bots), höchstwahrscheinlich aufgrund der übermäßigen Verwendung von `loot ASF`, das auf allen Bots gleichzeitig ausgeführt wurde, oder einer anderen verdächtigen einseitigen Menge von Handelsangeboten in sehr kurzer Zeit.
> > > 
> > > > *Hallo XXX, Vielen Dank für die Kontaktaufnahme mit dem Steam Support. *Es scheint, als ob dieses Konto verwendet wurde, um ein Netzwerk von Bot-Accounts zu verwalten. *Die Verwendung von Bot-Accounts ist eine Verletzung der Steam-Abonnementvereinbarung.*</p> </blockquote> 
> > > > 
> > > > Verwenden Sie bitte etwas gesunden Menschenverstand und gehen Sie nicht davon aus, dass solche verrückten Dinge nur tun können, weil ASF Ihnen dies erlaubt. Das Ausführen von `loot ASF` auf über 1k Bots kann leicht als **[DDoS](https://en.wikipedia.org/wiki/DDoS)** Angriff angesehen werden, und ich persönlich bin nicht schockiert, dass jemand für so eine Sache gesperrt wurde. Nutzen Sie einen gesunden Menschenverstand und ein Minimum an fairem Gebrauch im Bezug auf den Steam Service, und *höchstwahrscheinlich* wird Ihnen nichts geschehen.
> > > > 
> > > > Ein weiterer Fall war Jemand mit 170+ Bots, der während dem Steam Winter Sale gesperrt wurde.
> > > > 
> > > > > *Your account was blocked for violation of the agreement of the subscriber Steam. *Judging by the exchanges and other factors, this account was used to illegally collect collectible cards on Steam, as well as related and not only commercial activities. *The account has been permanently blocked and Steam Support can not provide additional support on this issue.*</p> </blockquote> 
> > > > > 
> > > > > Dieser Fall ist wieder einmal sehr schwer zu analysieren, da die Reaktion des Steam-Supports, der kaum Details bietet, vage ist. Basierend auf meinen persönlichen Ansichten hat dieser Benutzer wahrscheinlich Steam-Karten gegen irgendeine Art von Geld eingetauscht (Level-up-Bot?) oder auf andere Weise versucht, bei Steam Geld auszuzahlen. Im Falle, dass Sie sich dessen nicht bewusst sind es ist ebenfalls in den **[Steam-Nutzungsbedingungen" untersagt](https://store.steampowered.com/subscriber_agreement/german)**.
> > > > > 
> > > > > Letzter Fall, bei dem ein Benutzer mit 120+ Bots wegen Verletzung von **[Steam-Online-Verhalten](https://store.steampowered.com/online_conduct?l=german)** gesperrt wurde.
> > > > > 
> > > > > > *Hallo XXX, Vielen Dank für die Kontaktaufnahme mit dem Steam Support. *Dieser und andere Accounts wurden für flooding unserer Netzwerk-Infrastruktur verwendet, was gegen das Steam-Online-Verhalten verstößt. *Der Account wurde dauerhaft gesperrt und der Steam-Support kann in diesem Fall keinen zusätzlichen Support diesbezüglich anbieten.</p> </blockquote> 
> > > > > > 
> > > > > > Dieser Fall ist Aufgrund der zusätzlichen Angaben des Benutzers etwas einfacher zu analysieren. Anscheinend verwendete der Benutzer **eine sehr veraltete ASF-Version**, die einen Fehler enthielt, der dazu führte, dass ASF eine übermäßige Anzahl von Anfragen an Steam-Server sendete. Der Fehler selbst existierte zunächst nicht, wurde aber wegen einer Änderung von Steam ausgelöst, die in einer späteren Version behoben wurde. **ASF wird nur in der *[aktuellen stabilen Version](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)</strong> unterstützt, die auf GitHub* veröffentlicht wurde. Software wird von Menschen geschrieben, und Menschen neigen dazu, Fehler zu begehen. Wenn der Fehler einen globalen Umfang hat, wird er schnell behoben und als Bugfix für alle Benutzer veröffentlicht. Valve wird nicht plötzlich eine halbe Million ASF-Benutzer Aufgrund meines Fehlers sperren, aus offensichtlichen Gründen. Wenn Sie jedoch absichtlich auf die Verwendung einer aktuellen Version von ASF verzichten, dann sind Sie per Definition in einer sehr kleinen Minderheit von Benutzern, die **Vorfällen wie diesen** wegen **mangelnder technischer Unterstützung** ausgesetzt sind, da niemand auf eine veraltete Version von ASF aufpasst, niemand sie repariert und sicherstellt, dass Sie nicht einfach durch den Start von ASF völlig gesperrt werden. **Bitte verwenden Sie stets die aktuellste Version**, nicht nur ASF, sondern auch alle anderen Anwendungen.</p> 
> > > > > > 
> > > > > > * * *
> > > > > > 
> > > > > > Alle oben genannten Vorfälle haben eines gemeinsam - ASF ist nur ein Programm und es ist **Ihre** Entscheidung, wie Sie es nutzen möchten. Sie bekommen keine Sperre für die direkte Verwendung von ASF, aber es kommt darauf an **wie** Sie ASF verwenden. Es kann ein Hilfsprogramm sein, das nur ein einziges Konto sammelt, oder ein riesiges Sammel-Netzwerk aus Tausenden von Bots. In jedem dieser Fälle biete ich keine rechtliche Beratung an, und Sie sollten zunächst selbst über die ASF-Nutzung entscheiden. Ich verheimliche keine Informationen, die Ihnen helfen könnten; z.B. die Tatsache, dass ASF einige Leute gesperrt hat, da ich keinen Grund dazu habe. Es ist allein Ihre Entscheidung, was Sie mit diesen Informationen anstellen. Wenn Sie mich fragen - benutzen Sie Ihren gesunden Menschenverstand und vermeiden Sie es, ein massives Sammel-Netzwerk von Bots zu betreiben oder gar hunderte Handelsangebote gleichzeitig zu senden und verwenden Sie immer die aktuellste ASF-Version und alles *sollte* gut sein. Jeder einzelne Vorfall dieser Art ist aus **irgendeinem Grund** immer Leuten passiert, die unsere Empfehlung missachtet und entschieden haben, dass sie es besser wissen, wie viele Bots sie betreiben können. Ob es nur ein Zufall oder ein tatsächlicher Faktor ist, das müssen Sie selbst entscheiden. Ich biete keine rechtliche Beratung an, sondern gebe Ihnen nur meine Gedanken, welche Sie beachten, oder gänzlich ignoriere können, während Sie nur mit den oben genannten Fakten arbeiten.
> > > > > > 
> > > > > > * * *
> > > > > > 
> > > > > > ### Welche Datenschutzinformationen gibt ASF weiter?
> > > > > > 
> > > > > > Eine detaillierte Erklärung ist im Abschnitt **[Statistiken](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Statistics-de-DE)** ersichtlich. Sie sollten es lesen, wenn Sie sich um Ihre Privatsphäre sorgen, z.B. wenn Sie sich fragen, warum Konten, die in ASF verwendet werden, unserer Steam-Gruppe beitreten. ASF sammelt keine sensiblen Informationen und gibt sie nicht an Dritte weiter.
> > > > > > 
> > > > > > * * *
> > > > > > 
> > > > > > ## Sonstiges
> > > > > > 
> > > > > > * * *
> > > > > > 
> > > > > > ### Ich verwende ein nicht unterstütztes Betriebssystem z.B. 32-Bit-Windows, kann ich trotzdem die neueste ASF Version verwenden?
> > > > > > 
> > > > > > Ja, und diese Version wird auch unterstützt, nur nicht offiziell erstellt. Sehen Sie sich hierfür die generische Varianten im Abschnitt **[Kompatibilität](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-de-DE)** an. ASF hat keine ausgeprägte Abhängigkeit vom Betriebssystem und es kann überall dort funktionieren, wo Sie eine funktionierende .NET Core Runtime, die auch eine ausführbare 32-Bit Windows enthält, auch wenn es kein OS-spezifisches Paket für `win-x86` von uns gibt.
> > > > > > 
> > > > > > * * *
> > > > > > 
> > > > > > ### ASF ist großartig! Kann ich spenden?
> > > > > > 
> > > > > > Ja, und wir freuen uns sehr zu hören, dass Ihnen unser Projekt gefällt! Unter jeder **[Veröffentlichung](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** und auch **[auf der Hauptseite](https://github.com/JustArchiNET/ArchiSteamFarm)** finden Sie verschiedene Möglichkeiten eine Spende zu tätigen. Es ist gut zu wissen, dass wir neben allgemeinen Geldspenden auch Steam-Gegenstände akzeptieren, sodass Sie nichts davon abhält, Skins, Keys oder einen kleinen Teil der Karten, die mit ASF gesammelt wurden, zu spenden. Natürlich nur wenn Sie möchten. Vielen Dank im Voraus für Ihre Großzügigkeit!
> > > > > > 
> > > > > > * * *
> > > > > > 
> > > > > > ### Ich verwende den Steam Parental Code um mein Konto zu schützen, muss ich diesen irgendwo eingeben?
> > > > > > 
> > > > > > Ja, Sie müssen ihn in der `SteamParentalCode` Bot-Konfigurationseigenschaft eingeben. Dies liegt hauptsächlich daran, dass ASF auf viele geschützte Teile Ihres Steam-Kontos zugreift und es für ASF unmöglich ist ohne ihn zu arbeiten.
> > > > > > 
> > > > > > * * *
> > > > > > 
> > > > > > ### Ich möchte nicht, dass ASF standardmäßig irgendwelche Spiele sammelt, aber ich möchte zusätzliche ASF-Funktionen verwenden. Ist das möglich?
> > > > > > 
> > > > > > Ja, wenn Sie ASF nur mit dem pausierten Karten-Farming-Modul starten wollen, können Sie die Bot-Konfigurationseigenschaft `Paused` auf `true` setzen, um dies zu erreichen. Dies ermöglicht es Ihnen `resume` (fortsetzen) zu verwenden, während der Ausführung.
> > > > > > 
> > > > > > Wenn Sie das Karten-Farming-Modul vollständig deaktivieren und sicherstellen möchten, dass es nie ausgeführt wird, ohne dass Sie es explizit anders angeben, dann empfehlen wir, `IdlePriorityQueueOnly` auf `true` zu setzen, was den Leerlauf komplett deaktiviert, anstatt es nur anzuhalten, bis Sie die Spiele selbst in die Warteschlange für ungenutzte Prioritäten aufnehmen.
> > > > > > 
> > > > > > Wenn das Karten-Farming-Modul angehalten/deaktiviert ist, können Sie zusätzliche ASF-Funktionen nutzen, wie z. B. `GamesPlayedWhileIdle`.
> > > > > > 
> > > > > > * * *
> > > > > > 
> > > > > > ### Kann ich ASF in die Taskleiste minimieren?
> > > > > > 
> > > > > > ASF ist eine Konsolenanwendung, es gibt kein zu minimiertes Fenster, da das Fenster von Ihrem Betriebssystem für Sie erstellt wird. Sie können jedoch ein beliebiges Drittanbieterprogramm verwenden wie z.B. **[RBTray](https://github.com/benbuck/rbtray)** für Windows oder **[screen](https://linux.die.net/man/1/screen)** für Linux/OS X. Das sind nur Beispiele, es gibt viele andere Programme mit ähnlicher Funktionalität.
> > > > > > 
> > > > > > * * *
> > > > > > 
> > > > > > ### Stellt die Verwendung von ASF die Berechtigung zum Erhalt von Booster Packs sicher?
> > > > > > 
> > > > > > **Ja**. ASF verwendet die gleiche Methode wie der offizielle Client, um sich im Steam-Netzwerk anzumelden, daher enthält es auch die Möglichkeit, Booster Packs für die verwendeten Konten zu erhalten. Mehr noch: Um diese Fähigkeit zu bewahren, muss man sich nicht einmal in die Steam-Community einloggen. Damit können Sie, falls Sie es wünschen, den `OnlineStatus` `Offline` sicher verwenden.
> > > > > > 
> > > > > > * * *
> > > > > > 
> > > > > > ### Gibt es eine Möglichkeit mit ASF zu kommunizieren?
> > > > > > 
> > > > > > Ja, auf verschiedene Arten. Weitere Informationen erhalten Sie im Abschnitt **[Befehle](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE)**.
> > > > > > 
> > > > > > * * *
> > > > > > 
> > > > > > ### Ich möchte bei der Übersetzung von ASF helfen, was muss ich tun?
> > > > > > 
> > > > > > Vielen Dank für Ihr Intresse! Alle Details hierzu könnenn Sie in unserem Abschnitt **[Lokalisierung](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Localization-de-DE)** finden.
> > > > > > 
> > > > > > * * *
> > > > > > 
> > > > > > ### Ich habe nur ein (Haupt-)Konto zu ASF hinzugefügt, kann ich trotzdem Befehle per Steam-Chat senden?
> > > > > > 
> > > > > > **Ja**, es wird im Abschnitt **<a href="https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE#notes[Befehle](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE#Anmerkungen)** erklärt. Sie können dies über den Steam-Gruppen-Chat erledigen, obwohl die Verwendung von **[ASF-ui](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-de-DE#asf-ui)** für Sie einfacher sein könnte.
> > > > > > 
> > > > > > * * *
> > > > > > 
> > > > > > ### ASF scheint zu funktionieren, aber ich bekomme keine Karten!
> > > > > > 
> > > > > > Die Sammelrate der Karten ist von Spiel zu Spiel unterschiedlich, wie Sie in **[Leistung](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance-de-DE)** nachlesen können. Es dauert eine Weile, normalerweise **mehrere Stunden pro Spiel** und Sie sollten nicht erwarten, dass Karten bereits wenige Minuten nach dem Start eines Programmes gesammelt werden. Wenn Sie sehen können, dass ASF den Kartenstatus aktiv überprüft und das Spiel wechselt, nachdem das aktuelle vollständig gesammelt wurde, dann funktioniert das Ganze einwandfrei. Es ist möglich, dass Sie eine Option wie `DismissInventoryNotifications` von `BotBehaviour` aktiviert haben, die Inventarbenachrichtigungen automatisch ausblendet. Weitere Details erfahren Sie unter **[Konfiguration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-de-DE)**.
> > > > > > 
> > > > > > * * *
> > > > > > 
> > > > > > ### Wie kann ich den ASF-Prozess für mein Konto vollständig stoppen?
> > > > > > 
> > > > > > Beenden Sie einfach den ASF-Prozess, z.B. durch Anklicken von [X] unter Windows. Wenn Sie stattdessen einen bestimmten Bot Ihrer Wahl stoppen, aber andere am Laufen halten möchten, dann schauen Sie die `Enabled` **[Bot-Konfigurationseigenschaft](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-de-DE#bot-konfiguration)** an, oder den `stop` **[Befehl](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE)**. Wenn Sie stattdessen den automatischen Sammel-Prozess stoppen möchten, aber ASF für Ihr Konto am Laufen halten möchten, dann ist das das, wofür die **[Bot-Konfigurationseigenschaft](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-de-DE#bot-konfiguration)** `Paused` und der `pause` **[Befehl](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE)** ist.
> > > > > > 
> > > > > > * * *
> > > > > > 
> > > > > > ### Wie viele Bots kann ich mit ASF verwenden?
> > > > > > 
> > > > > > ASF als Programm hat keine unmittelbare Obergrenze für Bot-Instanzen, sodass Sie so viele Bot-Instanzen ausführen können, wie Sie Speicher auf einem Computer haben, aber Sie sind immer noch durch das Steam-Netzwerk und andere Steam-Dienste limitiert. Derzeit können Sie bis zu 100-200 Bots mit einer einzigen IP und einer einzelnen ASF-Instanz laufen lassen. Es ist möglich, mehr Bots mit mehr IPs und mehr ASF-Instanzen auszuführen, indem man die IP-Beschränkungen umgeht. Bedenken Sie, dass, sobald Sie diese große Anzahl von Bots verwenden, deren Anzahl Sie selbst kontrollieren sollten, z. B. sicherstellen, dass sich alle tatsächlich anmelden und zur gleichen Zeit arbeiten. ASF wurde nicht für diese große Anzahl von Bots optimiert und die allgemeine Regel gilt, dass **je mehr Bots Sie haben, desto mehr Probleme werden Sie erfahren**. Außerdem ist zu beachten, dass das obige Limit im Allgemeinen von vielen internen Faktoren abhängt - es ist eher eine Annäherung als ein strenges Limit - Sie werden höchstwahrscheinlich in der Lage sein, mehr/weniger Bots als oben beschrieben auszuführen.
> > > > > > 
> > > > > > Das ASF-Team empfiehlt, bis zu **10 Bots insgesamt** laufen zu lassen (und zu **besitzen**), alles darüber wird nicht unterstützt und auf eigenes Risiko durchgeführt, gegen unseren Vorschlag, der hier gemacht wurde. Diese Empfehlung basiert auf internen Valve-Richtlinien sowie unseren eigenen Empfehlungen. Ob Sie diese Regel einhalten oder nicht, ist einzig Ihre Entscheidung, ASF als Werkzeug wird nicht gegen Ihren eigenen Willen handeln, auch wenn es dazu führen sollte, dass Ihre Steam-Konten dafür gesperrt werden. Daher wird ASF Ihnen eine Warnung anzeigen, wenn Sie über das hinausgehen, was wir empfehlen. Dennoch können Sie - auf eigenes Risiko und ohne unseren Support - ASF darüber hinaus ausführen.
> > > > > > 
> > > > > > * * *
> > > > > > 
> > > > > > ### Kann ich mehrere ASF-Instanzen ausführen?
> > > > > > 
> > > > > > Sie können so viele ASF-Instanzen auf einem Computer ausführen, wie Sie möchten, vorausgesetzt, jede Instanz hat ihr eigenes Verzeichnis und ihre eigenen Konfigurationen, und das in einer Instanz verwendete Konto wird in Anderen nicht verwendet. Allerdings sollten Sie sich fragen, warum Sie das tun wollen. ASF ist dazu optimiert, mehrere hundert Accounts gleichzeitig zu verwalten. Das Starten vieler Bots in ihren eigenen ASF-Instazen senkt die (Gesamt-)Leistung, verbraucht mehr Betriebssystem-Ressourcen (beispielsweise CPU, Arbeitsspeicher), und verursacht potentiell Probleme beim synchronisieren zwischen den einzelnen ASF-Instanzen, da ASF gezwungen ist, seine Limitierung (Limiters) mit den anderen Instanzen zu teilen.
> > > > > > 
> > > > > > Deshalb ist es mein **Ratschlag**, immer maximal eine ASF-Instanz pro IP/Schnittstelle auszuführen. Wenn Sie mehr IPs/Schnittstelle haben, können Sie mit allen Mitteln mehr ASF-Instanzen ausführen, mit jeder Instanz über eine eigene IP/Schnittstelle oder eine eindeutige `WebProxy`-Einstellung. Wenn Sie dies nicht machen, ist es völlig sinnlos, mehr ASF-Instanzen zu starten, da Sie sonst jede einzelne IP/Schnittstelle auf 1 Instanz begrenzt ist. Steam wird es Ihnen auf magische Weise nicht erlauben, mehr Bots auszuführen, nur weil Sie sie in einer anderen ASF-Instanz gestartet haben, und ASF beschränkt Sie von Anfang an nicht.
> > > > > > 
> > > > > > Natürlich gibt es immer noch sinnvolle Anwendungsfälle für mehrere ASF-Instanzen auf derselben Netzwerkschnittstelle, wie z. B. das Hosten von ASF für Ihre Freunde, bei denen jeder Freund eine eigene ASF Instanz besitzt, um die Isolation zwischen Bots und sogar den ASF Prozessen selbst zu gewährleisten. Dies hat aber nicht den Zweck, geschweige denn die Möglichkeit, um Steambeschränkungen zu umgehen.
> > > > > > 
> > > > > > * * *
> > > > > > 
> > > > > > ### Was bedeutet Status beim Einlösen eines Produktschlüssels?
> > > > > > 
> > > > > > Der Status zeigt an, wie sich der gegebene Einlösungsversuch ausgewirkt hat. Es gibt viele verschiedene Stati, darunter die gängigsten:
> > > > > > 
> > > > > > | Status                  | Beschreibung                                                                                                                                                                                                                                                         |
> > > > > > | ----------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
> > > > > > | NoDetail                | Der Status "OK" zeigt Erfolg an - der Produktschlüssel wurde erfolgreich eingelöst.                                                                                                                                                                                  |
> > > > > > | Timeout                 | Das Steam-Netzwerk hat nicht in einer bestimmten Zeit reagiert, wir wissen nicht, ob der Produktschlüssel eingelöst wurde oder nicht (höchstwahrscheinlich wurde er das, aber Sie könen es noch einmal versuchen).                                                   |
> > > > > > | BadActivationCode       | Der angegebene Produktschlüssel ist ungültig (nicht als gültiger Produktschlüssel im Steam-Netzwerk erkannt).                                                                                                                                                        |
> > > > > > | DuplicateActivationCode | Der angegebene Produktschlüssel wurde bereits von einem anderen Konto eingelöst oder vom Entwickler/Herrausgeber widerrufen.                                                                                                                                         |
> > > > > > | AlreadyPurchased        | Ihr Konto besitzt bereits `packageID`, das mit diesem Produktschlüssel verbunden ist. Bedenken Sie, dass dies nicht anzeigt, ob der Produktschlüssel `DuplicateActivationCode` ist oder nicht - nur dass er gültig ist und bei diesem Versuch nicht verwendet wurde. |
> > > > > > | RestrictedCountry       | Dies ist ein Produktschlüssel mit Regionssperre und das Konto befindet sich in einer ungültigen Region, in der Sie ihn nicht einlösen dürfen.                                                                                                                        |
> > > > > > | DoesNotOwnRequiredApp   | Sie können diesen Produktschlüssel nicht einlösen, da Ihnen ein anderes Spiel fehlt - hauptsächlich das Basis-Spiel, wenn Sie versuchen, das DLC-Paket einzulösen.                                                                                                   |
> > > > > > | RateLimited             | Sie haben zu viele Einlösungsversuche durchgeführt und das Konto wurde vorübergehend gesperrt. Versuchen Sie es in einer Stunde erneut.                                                                                                                              |
> > > > > > 
> > > > > > * * *
> > > > > > 
> > > > > > ### Gehörst du zu einem Karten-Sammel-Dienst?
> > > > > > 
> > > > > > **Nein**. ASF ist mit keinem Dienst verknüpft und alle diese Behauptungen sind falsch. Dein Steam-Konto ist dein Eigentum und du kannst dein Konto auf jede erdenkliche Weise nutzen, aber Valve hat es klar in den **[offiziellen Nutzungsbedingungen](https://store.steampowered.com/subscriber_agreement/german)** erklärt:
> > > > > > 
> > > > > > > *Sie sind für die Sicherheit Ihres Computersystems und die Geheimhaltung Ihrer Anmeldedaten und Ihres Kennworts verantwortlich. *Valve ist in keiner Weise für die Verwendung Ihres Kennworts und die Nutzung Ihres Benutzerkontos sowie auch nicht für jegliche Kommunikation und Aktivität auf Steam verantwortlich, die durch die Verwendung Ihres Kennworts durch Sie oder durch jegliche andere Personen, denen Sie absichtlich oder unabsichtlich Ihr Kennwort durch Bruch dieser Vertraulichkeitsklausel mitgeteilt haben, zustande kommt.*</p> </blockquote> 
> > > > > > > 
> > > > > > > ASF ist unter der freien Apache 2.0 Lizenz lizenziert, die es anderen Entwicklern ermöglicht, ASF weiter in ihre eigenen Projekte und Dienste zu integrieren. Es wird jedoch nicht garantiert, dass solche Drittprojekte, die ASF verwenden, sicher, überprüft, angemessen oder legal gemäß **[Steam-Nutzungsbedingungen](https://store.steampowered.com/subscriber_agreement/german)** (ToS/ AGB) sind. Wenn Sie unsere Meinung hören möchten, **wir empfehlen Ihnen dringend, KEINE Kontoinformationen mit Drittanbietern zu teilen**. Wenn sich ein solcher Dienst als **typischer Betrug** herausstellt, werden Sie mit dem Problem allein gelassen, höchstwahrscheinlich ohne Ihr Steam-Konto und ASF übernimmt keine Verantwortung für Dienstleistungen von Drittanbietern, die behaupten, sicher und geschützt zu sein, weil das ASF-Team weder genehmigt noch eine davon überprüft hat. Mit anderen Worten, **Sie nutzten sie auf eigene Gefahr, entgegen unseres oben genannten Vorschlags**.
> > > > > > > 
> > > > > > > Zusätzlich beschreibt die offizielle **[Steam-AGB](https://store.steampowered.com/subscriber_agreement/german)** eindeutig:
> > > > > > > 
> > > > > > > > *Es ist Ihnen untersagt, Ihr Kennwort oder Benutzerkonto Dritten offenzulegen bzw. zugänglich zu machen oder Dritte sonst Ihr Kennwort oder Benutzerkonto nutzen zu lassen, es sei denn, Valve hat dem ausdrücklich zugestimmt.*
> > > > > > > 
> > > > > > > Es ist allein Ihr Konto und Ihre Entscheidung. Behaupten Sie nur nicht, dass Sie niemand gewarnt hat. ASF als Programm erfüllt alle oben genannten Bedingungen, da Sie die Kontodaten an niemanden weitergeben und das Programm für den eigenen persönlichen Gebrauch verwenden. Jeder andere "Karten-Sammel-Dienst" verlangt von Ihnen die Zugangsdaten, sodass es auch gegen die oben genannte Regel verstößt (eigentlich mehrere davon). Wie bei der **[Steam-Nutzungsbedingungen](https://store.steampowered.com/subscriber_agreement/german)** (AGB)-Auswertung, bieten wir keine Rechtsberatung an, und Sie sollten sich selbst entscheiden, ob Sie diese Dienste nutzen möchten oder nicht. Unserer Auffassung nach **verstößt es direkt gegen die [Steam-AGB](https://store.steampowered.com/subscriber_agreement/german)** und kann zu einer Suspendierung führen, wenn Valve dies herausfindet. Wie bereits erwähnt, **wir empfehlen dringend, KEINEN dieser Dienste zu nutzen**.
> > > > > > > 
> > > > > > > * * *
> > > > > > > 
> > > > > > > ## Probleme
> > > > > > > 
> > > > > > > * * *
> > > > > > > 
> > > > > > > ### Eines meiner Spiele wird nun seit mehr als 10 Stunden gefarmt, aber ich habe noch keine Karten bekommen!
> > > > > > > 
> > > > > > > Der Grund dafür könnte mit dem bekannten Steam Problem zusammenhängen, das auftritt, wenn Sie zwei Lizenzen für ein und dasselbe Spiel haben, von denen eine eine limiterte Kartenrate besitzt. Dies passiert üblicherweise, wenn Sie das Spiel kostenlos während eines Massengiveaway auf Steam aktiviert haben und anschließend einen Schlüssel für das gleiche Spiel (aber ohne Limitierungen) aktivieren. Wenn solch eine Situation eintritt, meldet Steam auf der Abzeichenseite, dass das Spiel noch Karten zum sammeln hat, aber egal, wie viel Sie spielen - die Karten werden aufgrund der kostenlosen Lizenz in Ihrem Konto nie gutgeschrieben. Da dies kein Fehler seitens ASF, sondern von Steam ist, können wir diesen auf ASF-Seite nicht umgehen und Sie müssen das Problem selbst lösen.
> > > > > > > 
> > > > > > > Es gibt zwei Möglichkeiten, das Problem zu lösen. Zum Einen können Sie das Spiel in ASF über den **[Befehl](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE)** `ibadd`, oder mit der entsprechenden `Blacklist` **[Konfigurationseigenschaft](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-de-DE)** vom automatischen Sammeln ausschließen. Dies verhindert zwar, dass ASF versucht Karten von dem Spiel zu sammeln, löst aber nicht das eigentliche Problem, welches verhindert, dass Sie Karten von dem betroffenen Spiel erhalten. Zum Zweiten können Sie das Steam-Support Self-Service Tool verwenden um die kostenlose Lizenz von Ihrem Konto zu entfernen, sodass nur noch die volle Lizenz, welche Karten beinhaltet, übrig bleibt. Um dies zu erreichen, besuchen Sie zuerst Ihre **["Lizenzen und Produktschlüssel-Aktivierungen"](https://store.steampowered.com/account/licenses)**-Seite und finden Sie sowohl die kostenlose als auch kostenpflichtige Lizenz für das betroffene Spiel. Normalerweise ist es ziemlich einfach. Beide haben einen ähnlichen Namen, aber die kostenlose Lizenz enthält "Limited free promotional package " oder andere "promo" im Lizenznamen, sowie "complimentary" im Bereich "Akquisitionsmethode". Manchmal könnte es schwieriger sein, zum Beispiel, wenn das freie Paket in einem Bundle war und einen anderen Namen hat. Falls Sie einige dieser Lizenzen gesehen haben - dann ist es tatsächlich das Problem, das hier beschrieben wurde und Sie können die kostenlose Lizenz sicher entfernen, ohne das Spiel zu verlieren.
> > > > > > > 
> > > > > > > Um die kostenlose Lizenz von Ihrem Konto zu entfernen, gehen Sie auf **[Steam-Support-Seite](https://help.steampowered.com/wizard/HelpWithGame)** und legen Sie den betroffenen Spielnamen in das Suchfeld das Spiel sollte im Bereich "Produkte" verfügbar sein; klicken Sie darauf. Alternativ können Sie einfach den `https://help.steampowered.com/wizard/HelpWithGame?appid=<appID>` Link verwenden und `<appID>` durch appID des Spiels ersetzen, das Probleme verursacht. Danach klicken Sie auf "Ich möchte dieses Spiel dauerhaft von meinem Konto entfernen" und wählen die fehlerhafte kostenlose Lizenz, die Sie oben gefunden haben- in der Regel die mit "limited free promotional package " im Namen (oder ähnlichem). Nach dem Entfernen der freien Lizenz sollte ASF in der Lage sein, Karten aus dem betroffenen Spiel ohne Probleme zu sammeln. Sie sollten den Leerlaufvorgang nach der Entfernung neu starten, nur um sicherzustellen, dass Steam diesmal die richtige Lizenz abholt.
> > > > > > > 
> > > > > > > * * *
> > > > > > > 
> > > > > > > ### ASF erkennt Spiel `X` nicht als zum Sammeln verfügbar, aber ich weiß, dass es Steam-Karten enthält!
> > > > > > > 
> > > > > > > Hierfür gibt es zwei Hauptgründe. Der erste und offensichtlichste Grund ist die Tatsache, dass Sie sich auf den **Steam-Shop** beziehen, wo ein gegebenes Spiel mit Karten angekündigt wird. Diese Annahme ist **falsch**, da es lediglich besagt, dass das Spiel Karten **enthält**, aber nicht unbedingt ob diese Funktion für dieses Spiel auch sofort **aktiviert** ist. Mehr dazu können Sie in den **[offiziellen Ankündigungen](https://steamcommunity.com/games/593110/announcements/detail/1954971077935370845)** erfahren.
> > > > > > > 
> > > > > > > Kurz gesagt, das Karten-Symbol im Steam-Shop bedeutet nichts. Überprüfen Sie Ihre **[Abzeichen-Seiten](https://steamcommunity.com/my/badges)** zur Bestätigung, ob ein Spiel Karten aktiviert wurde oder nicht - das ist auch das, was ASF tut. Wenn ein Spiel nicht auf der Liste als ein Spiel mit Karten erscheint, dann ist es **nicht** möglich diese Spiel zu sammeln, unabhängig vom Grund.
> > > > > > > 
> > > > > > > Das zweite Problem ist weniger offensichtlich. Nämlich sobald Sie sehen, dass ein Spiel tatsächlich Karten auf der Abzeichen-Seite verfügbar sind, aber es dennoch nicht sofort von ASF gesammelt wird. Es sei denn, Sie stoßen auf einen anderen Fehler, wie z. B. ASF, das nicht in der Lage ist, Abzeichen-Seiten zu überprüfen (siehe unten), es ist lediglich die Kombination aus einem Cache-Effekt und veraltete Einträge auf den Steam-Abzeichenseiten, welche Steam immernoch auf der ASF-Seite berichtet. Dieses Problem sollte sich früher oder später lösen, wenn der Cache entwertet wird. Es gibt auch keine Möglichkeit, dies unsererseits zu beheben.
> > > > > > > 
> > > > > > > Natürlich setzt das alles vorraus, dass Sie ASF mit standardmäßig unberührten Einstellungen verwenden, da Sie dieses Spiel auch zur schwarzen Liste des Sammel-Moduls hinzufügen könnten. Verwenden Sie `IdlePriorityQueueOnly` von `true`, `IdleRefundableGames` von `false` und so weiter...
> > > > > > > 
> > > > > > > * * *
> > > > > > > 
> > > > > > > ### Warum erhöht sich die Spielzeit von Spielen, die über ASF gespielt werden, nicht?
> > > > > > > 
> > > > > > > Das tut es, aber **nicht in Echtzeit**. Steam zeichnet die Spielzeit in festen Intervallen auf und aktualisiert die Zeitpläne dafür, aber es ist nicht garantiert, dass sie sofort nach Beendigung der Sitzung aktualisiert wird, geschweige denn während dieser Zeit. Wenn es möglich wäre, die Spielzeit beim Sammeln zu überspringen, dann können Sie sicher sein, dass wir es schon vor langer Zeit in ASF implementiert hätten und es tatsächlich in den Standardeinstellungen verwenden würden. Aber das tun wir momentan nicht, weil es nicht möglich ist - nur weil die Spielzeit nicht in Echtzeit aktualisiert wird, bedeutet das nicht, dass sie nicht aufgenommen wird.
> > > > > > > 
> > > > > > > * * *
> > > > > > > 
> > > > > > > ### Worin besteht der Unterschied zwischen einer Warnung und einem Fehler im Protokoll?
> > > > > > > 
> > > > > > > ASF schreibt eine Reihe von Informationen über verschiedene Logging-Ebenen in sein Protokoll. Unser Ziel ist es, **präzise** zu erklären, was ASF unternimmt, einschließlich welcher Steam-Probleme es zu bewältigen hat, oder anderer zu überwindender Probleme. Meistens ist nicht alles relevant, deshalb haben wir in ASF zwei große Stufen, die in Bezug auf Probleme verwendet werden - eine Warnstufe und eine Fehlerstufe.
> > > > > > > 
> > > > > > > Die allgemeine ASF-Regel ist, dass Warnungen **keine** Fehler sind, daher sollten sie **nicht** gemeldet werden. Eine Warnung ist ein Indikator für Sie, dass etwas möglicherweise Unerwünschtes passiert. Egal ob es Steam nicht reagierte, die API-Fehler geworfen hat oder die Netzwerkverbindung Probleme hatte - es ist eine Warnung und das bedeutet, dass wir erwarten das dies passiert, also belästigen Sie die ASF-Entwicklung bitte nicht damit. Natürlich steht es Ihnen frei, nach ihnen zu fragen oder Hilfe zu erhalten, indem Sie unsere technische Unterstützung in Anspruch nehmen, aber Sie sollten nicht davon ausgehen, dass es sich um meldenswerte ASF-Fehler handelt (sofern wir nichts anderes bestätigen).
> > > > > > > 
> > > > > > > Fehler hingegen deuten auf eine Situation hin, die nicht eintreten sollte, daher sind sie es wert, gemeldet zu werden, solange Sie sich vergewissert haben, dass Sie es nicht selbst sind, der sie verursacht. Wenn es sich um eine allgemeine Situation handelt, die wir erwarten, dann wird sie stattdessen in eine Warnung umgewandelt. Möglicherweise handelt es sich jedoch um einen Fehler, der korrigiert werden und nicht stillschweigend ignoriert werden sollte, vorausgesetzt, er ist nicht das Ergebnis Ihres eigenen technischen Problems. Wenn Sie zum Beispiel ungültigen Inhalt in die Datei `ASF.json` eintragen, wird ein Fehler auftreten, da ASF ihn nicht analysieren kann, aber Sie haben ihn dort platziert, also solltest Sie diesen Fehler nicht an uns melden (es sei denn, Sie haben bestätigt, dass ASF falsch ist und die Struktur tatsächlich absolut korrekt ist).
> > > > > > > 
> > > > > > > Zusammengefasst: Fehler melden und Warnungen nicht melden. Sie können nach wie vor zu Warnungen Fragen stellen und in unseren Supportbereichen Hilfe erhalten.
> > > > > > > 
> > > > > > > * * *
> > > > > > > 
> > > > > > > ### ASF startet nicht, das Programmfenster schließt sich sofort!
> > > > > > > 
> > > > > > > Unter normalen Bedingungen erzeugt jeder ASF-Crash oder -Schließung eine `log.txt` im Verzeichnis des Programms, die Sie sich ansehen können, um die Ursache dafür zu finden. Zusätzlich werden einige kürzliche Logdateien auch im `Logs`-Verzeichnis archiviert, da die Haupt-`log.txt`-Datei wird mit jedem ASF-Lauf überschrieben.
> > > > > > > 
> > > > > > > Wenn jedoch selbst die .NET Core Runtime nicht in der Lage ist, auf Ihrer Maschine zu booten, dann wird `log.txt` nicht erstellt. Wenn Ihnen das passiert, haben Sie höchstwahrscheinlich vergessen, die Voraussetzungen für die Installation von.NET Core zu erfüllen, wie in der Anleitung **[Einrichtung](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up-de-DE#betriebssystemspezifisches-setup)** beschrieben. Andere häufige Probleme können der Versuch sein, eine falsche ASF-Variante für Ihr Betriebssystem zu starten, oder auf andere Weise fehlende native .NET Core Laufzeitabhängigkeiten. Wenn sich das Konsolenfenster zu früh schließt, als dass Sie die Nachricht lesen könnten, dann öffnen Sie die unabhängige Konsole und starten von dort aus die ASF-Binärdatei. Öffnen Sie beispielsweise unter Windows das ASF-Verzeichnis, halten Sie `Shift` gedrückt, klicken Sie mit der rechten Maustaste in den Ordner und wählen Sie *"Befehlsfenster hier öffnen"* (oder *Powershell*), geben Sie dann in die Konsole `.\ArchiSteamFarm.exe` ein und bestätigen Sie mit Enter. Auf diese Weise erhalten Sie eine genaue Meldung, warum ASF nicht richtig startet.
> > > > > > > 
> > > > > > > * * *
> > > > > > > 
> > > > > > > ### ASF wirft mich aus meiner Steam-Client-Sitzung während ich spiele! / *Dieser Account wird an einem anderen PC verwendet*
> > > > > > > 
> > > > > > > Dies wird als Meldung im Steam-Overlay angezeigt, sobald das Konto während des Spielens woanders verwendet wird. Dieses Problem kann zwei verschiedene Ursachen haben.
> > > > > > > 
> > > > > > > Ein Grund dafür sind defekte Pakete (games), die nicht speziell eine Spielsperre richtig haben, aber trotzdem erwarten, dass diese vom Client besessen wird. Ein Beispiel für ein solches Paket wäre *Skyrim SE*. Ihr Steam-Client startet das Spiel richtig, aber dieses Spiel registriert sich selbst nicht als in Benutzung. Aus diesem Grund sieht ASF, dass der Prozess fortgesetzt werden kann, was es tut, und das wirft Sie aus dem Steam-Netzwerk, da Steam plötzlich erkennt, dass das Konto an einem anderen Ort verwendet wird.
> > > > > > > 
> > > > > > > Ein zweiter Grund kann auftreten, wenn Sie auf einem PC spielen, während ASF wartet (besonders auf einem anderen Computer) und Sie die Netzwerkverbindung verlieren. In diesem Fall markiert Sie das Steam-Netzwerk als offline und gibt die Spielsperre (wie oben) frei, was ASF (z.B. auf einer anderen Maschine) dazu veranlasst, das Sammeln wieder aufzunehmen. Wenn der PC wieder online ist, kann Steam keine Spielsperre mehr erfassen (die jetzt von ASF gehalten wird, ebenfalls ähnlich wie oben) und zeigt die gleiche Meldung an.
> > > > > > > 
> > > > > > > Die beiden Ursachen auf der ASF-Seite sind eigentlich sehr schwer zu beheben, da ASF das Sammeln einfach wieder aufnimmt, sobald das Steam-Netzwerk es darüber informiert, dass das Konto wieder beiebig verwendet werden kann. Das ist es, was normalerweise passiert, wenn Sie das Spiel schließen, aber bei defekten Paketen kann dies sofort passieren, auch wenn das Spiel noch läuft. ASF hat keine Möglichkeit zu wissen, ob Sie die Verbindung getrennt haben, aufgehörten ein Spiel zu spielen oder ob Sie immernoch ein Spiel spielen, das die Sperre nicht ordnungsgemäß einrichtet.
> > > > > > > 
> > > > > > > Die einzig angemessene Lösung für dieses Problem ist, einen Bot mit `pause` manuell zu pausieren und ihn mit `resume` wieder zu starten, sobald Sie fertig sind. Alternativ können Sie das Problem einfach ignorieren und sich genauso verhalten, als ob Sie mit dem Offline-Steam-Client spielen würden.
> > > > > > > 
> > > > > > > * * *
> > > > > > > 
> > > > > > > ### `Von Steam getrennt!` - Ich kann keine Verbindung zu den Steam-Servern herstellen.
> > > > > > > 
> > > > > > > ASF kann nur **versuchen** eine Verbindung mit den Steam-Servern herzustellen, und es kann aus vielen Gründen fehlschlagen, einschließlich fehlender Internetverbindung, Steam-Ausfällen, eine Verbindungs-Blockierung durch Ihre Firewall, Programme von Drittanbietern, falsch konfigurierte Routen oder temporäre Ausfälle. Sie können den `Debug`-Modus aktivieren, um ein ausführlicheres Protokoll mit genauen Fehlerursachen zu erhalten, obwohl es normalerweise einfach durch Ihre eigenen Aktionen verursacht wird, wie z.B. die Verwendung von *"CS:GO MM Server Picker"*, das viele Steam-IPs auf eine schwarze Liste setzt, was es für Sie sehr schwierig macht, tatsächlich das Steam-Netzwerk zu erreichen.
> > > > > > > 
> > > > > > > ASF wird sein Bestes tun, um eine Verbindung herzustellen, was nicht nur die Abfrage nach der aktualisierten Liste der Server beinhaltet, sondern auch den Versuch einer anderen IP, wenn die letzte fehlschlägt. Wenn es also wirklich ein temporäres Problem mit einem bestimmten Server oder einer bestimmten Route ist, wird ASF früher oder später eine Verbindung herstellen. Wenn Sie sich jedoch hinter der Firewall befinden oder auf andere Weise nicht in der Lage sind, die Steam-Server zu erreichen, dann müssen Sie es natürlich mit Hilfe des `Debug`-Modus selbst reparieren.
> > > > > > > 
> > > > > > > Es ist auch möglich, dass Ihre Maschine nicht in der Lage ist, eine Verbindung mit den Steam-Servern über das Standardprotokoll in ASF herzustellen. Sie können Protokolle, die ASF verwenden darf, ändern, indem Sie die globale Konfigurationseigenschaft `SteamProtocols` ändern. Wenn Sie zum Beispiel Probleme haben, Steam mit dem `UDP` Protokoll zu erreichen (z.B. Aufgrund der Firewall), dann können Sie `TCP` oder `WebSocket` ausprobieren.
> > > > > > > 
> > > > > > > In einer sehr unwahrscheinlichen Situation, in der falsche Server zwischengespeichert werden, z.B. weil ASF `config` Ordner von einer Maschine auf eine andere Maschine in einem völlig anderen Land verschoben wurde, könnte das Löschen von `ASF.db` helfen, um die Steam-Server beim nächsten Start zu aktualisieren. Meistens ist dies nicht notwendig und ist auch nicht notwendig, da diese Liste beim ersten Start und beim Verbindungsaufbau automatisch aktualisiert wird. Wir erwähnen es nur als eine Möglichkeit, um alles zu bereinigen, was mit der Liste der Steam-Server zusammenhängt, die von ASF im Zwischenspeicher gehalten werden.
> > > > > > > 
> > > > > > > * * *
> > > > > > > 
> > > > > > > ### `Konnte Abzeicheninformation nicht abfragen, wir versuchen es später erneut!`
> > > > > > > 
> > > > > > > Normalerweise bedeutet das, dass Sie die Steam-Familienansicht-PIN für den Zugriff auf Ihr Konto verwenden, aber Sie haben vergessen, sie in die ASF-Konfiguration einzugeben. Sie müssen eine gültige PIN in die Bot-Konfigurationseigenschaft `SteamParentalCode` eingeben, sonst kann ASF nicht auf die meisten Webinhalte zugreifen und kann daher nicht richtig funktionieren. Schauen Sie unter **[Konfiguration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-de-DE)** vorbei, um mehr über `SteamParentalCode` zu erfahren.
> > > > > > > 
> > > > > > > Andere Gründe können temporäre Steam-Probleme, Netzwerkprobleme oder ähnliches sein. Wenn sich das Problem nach mehreren Stunden nicht beheben lässt und Sie sicher sind, dass Sie ASF richtig konfiguriert haben, können Sie uns gerne darüber informieren.
> > > > > > > 
> > > > > > > * * *
> > > > > > > 
> > > > > > > ### ASF schlägt mit dem Fehler `Request failed after 5 tries` fehl!
> > > > > > > 
> > > > > > > Normalerweise bedeutet das, dass Sie die Steam-Familienansicht-PIN für den Zugriff auf dein Konto verwenden, aber Sie haben vergessen, diese in die ASF-Konfiguration einzugeben. Sie müssen eine gültige PIN in die Bot-Konfigurationseigenschaft `SteamParentalCode` eingeben, sonst kann ASF nicht auf die meisten Webinhalte zugreifen und somit nicht richtig funktionieren. Mehr Informationen über den `SteamParentalCode` erhalten Sie unter **[Konfiguration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-de-De)**.
> > > > > > > 
> > > > > > > Wenn die Steam-Familienansicht-PIN nicht der Grund dafür ist, dann ist dies ein häufiger Fehler, und Sie sollten sich daran gewöhnen. Das bedeutet einfach, dass ASF eine Anfrage an das Steam-Netzwerk gesendet, aber fünfmal in Folge keine gültige Antwort erhalten hat. Für gewöhnlich bedeutet es, dass Steam entweder außer Betrieb ist oder einige Schwierigkeiten oder Wartungsarbeiten hat - ASF ist sich solcher Probleme bewusst und Sie sollten sich keine Sorgen darum machen, es sei denn, sie passieren ständig für mehr als mehrere Stunden, und andere Benutzer haben keine derartigen Probleme.
> > > > > > > 
> > > > > > > Wie kann man überprüfen, ob Steam außer Betrieb ist? **[Steam Status](https://steamstat.us)** ist eine ausgezeichnete Quelle, um zu überprüfen, **ob** Steam in Betrieb sein sollte. Wenn Sie Fehler (insbesondere im Zusammenhang mit der Community oder der Web-API) bemerken, dann hat Steam Schwierigkeiten. Sie können entweder ASF in Ruhe lassen, sodass es die Aufgaben nach einer kurzen Auszeit erledigt, oder Sie warten einfach selbst.
> > > > > > > 
> > > > > > > Das ist jedoch nicht immer der Fall, da in einigen Situationen Steam-Probleme möglicherweise nicht durch Steam Status erkannt werden, z.B. passierte ein solcher Fall, als Valve die HTTPS-Unterstützung für Steam Community am 7. Juni 2016 unterbrach - der Zugriff auf **[SteamCommunity](https://steamcommunity.com)** durch HTTPS führte zu einem Fehler. Verlasse Sie sich daher auch nicht blind auf den Steam Status. Überprüfen Sie am besten selbst, ob alles so funktioniert, wie es sollte.
> > > > > > > 
> > > > > > > Darüber hinaus beinhaltet Steam verschiedene Maßnahmen zur Vermeidung eines Anfragelimits, die Ihre IP vorübergehend sperren, wenn Sie übermäßig viele Anfragen auf einmal stellen. ASF ist sich dessen bewusst und bietet Ihnen in der Konfiguration mehrere verschiedene Begrenzungen an, die Sie nutzen sollten. Die Standardeinstellungen wurden auf Basis von **einer gesunden** Anzahl der Bots angepasst. Wenn Sie soviele verwenden, dass sogar Steam Ihnen sagt, Sie sollten weggehen, dann optimieren Sie sie entweder, bis sie es Ihnen nicht mehr sagen, oder Sie tun, was man Ihnen sagt. Ich nehme an, dass der zweite Weg keine Option für Sie ist, also lesen Sie weiter zu diesem Thema und achten besonders auf `WebLimiterDelay`, der ein allgemeiner Begrenzer ist, der für alle Web-Anfragen gilt.
> > > > > > > 
> > > > > > > Es gibt keine "goldene Regel", die für jeden funktioniert, denn Sperren werden stark von Faktoren Dritter beeinflusst, deshalb muss man selbst experimentieren und einen Wert finden, der für einen funktioniert. Sie könnenmnn das auch ignorieren und einen Wert wie `10000` verwenden, was garantiert korrekt funktioniert, aber dann beschwere Sie sich nicht, dass ASF auf alles innerhalb von 10 Sekunden reagiert und wie das Parsen von Abzeichen 5 Minuten dauert. Darüber hinaus ist es durchaus möglich, dass kein Begrenzer etwas bewirkt, weil Sie so viele Bots haben, sodass Sie die **[Obergrenze](#wie-viele-bots-kann-ich-mit-asf-verwenden)** ereicht haben, wie oben erwähnt. Es ist sogar möglich, dass Sie sich ohne Probleme ins Steam-Netzwerk (Client) anmelden können, aber Steam Web (Webseite) sich schlicht weigert Ihnen zuzuhören, sollten Sie mehr als 100 Sitzungen gleichzeitig etabliert haben. ASF verlangt, dass sowohl das Steam-Netzwerk als auch das Steam-Web kooperieren. Wenn nur eines von Beiden nicht funktioniert, kann es zu Problemen kommen, von denen Sie sich nicht erholen werden.
> > > > > > > 
> > > > > > > Wenn nichts hilft und Sie nicht wissen, was kaputt ist, können Sie jederzeit der `Debug`-Modus aktivieren und sich im ASF-Log selbst ansehen, warum genau die Anfragen fehlschlagen. Zum Beispiel:
> > > > > > > 
> > > > > > > ```text
InternalRequest() HEAD https://steamcommunity.com/my/edit/settings
InternalRequest() Forbidden <- HEAD https://steamcommunity.com/my/edit/settings
```
> > > > > 
> > > > > Sehen Sie das `Forbidden`? Das bedeutet, dass Sie vorübergehend für übermäßig viele Anfragen gesperrt wurden, weil `WebLimiterDelay` noch nicht richtig angepasst wurde (vorausgesetzt, Sie bekommen den gleichen Fehlercode auch für alle anderen Anfragen). Es kann noch weitere Gründe geben, wie z.B. `InternalServerError`, `ServiceUnavailable` und Timeouts, die auf Steam Wartungsarbeiten und Probleme hinweisen. Sie können immer versuchen, den von ASF erwähnten Link selbst zu besuchen und zu überprüfen, ob er funktioniert - wenn nicht, dann wissen Sie, warum ASF auch nicht darauf zugreifen kann. Wenn dies der Fall ist und der gleiche Fehler nach ein oder zwei Tagen nicht verschwindet, könnte es sich lohnen, ihn zu untersuchen und zu melden.
> > > > > 
> > > > > Bevor Sie das tun, **sollten Sie sich vergewissern, dass der Fehler überhaupt einen Bericht wert ist**. Wenn es in diesem FAQ erwähnt wird, wie z.B. handelsbedingte Probleme, dann ist das nicht der Fall. Wenn es sich um ein temporäres Problem handelt, das ein- oder zweimal auftrat, insbesondere wenn Ihr Netzwerk instabil war oder Steam ausgefallen ist - dann ist das nicht der Fall. Wenn Sie jedoch in der Lage waren, Ihr Problem mehrmals hintereinander, über 2 Tage, zu reproduzieren, ASF sowie Ihre Maschine im Prozess neu zu starten und sicher zu stellen, dass es hier keinen FAQ-Eintrag gibt, der Ihnen hilft, es zu lösen, dann könnte es sich lohnen, nach ihm zu fragen.
> > > > > 
> > > > > * * *
> > > > > 
> > > > > ### ASF scheint einzufrieren und gibt nichts auf der Konsole aus, bis ich eine Taste drücke!
> > > > > 
> > > > > Sie verwenden höchstwahrscheinlich Windows und die Konsole hat den QuickEdit-Modus aktiviert. Eine technische Erklärung finden Sie auf **[diese](https://stackoverflow.com/questions/30418886/how-and-why-does-quickedit-mode-in-command-prompt-freeze-applications)** Frage im StackOverflow. Sie sollten den QuickEdit-Modus deaktivieren, indem Sie mit der rechten Maustaste auf das ASF-Konsolenfenster klicken, die Eigenschaften öffnen und das entsprechende Kontrollkästchen deaktivieren.
> > > > > 
> > > > > * * *
> > > > > 
> > > > > ### ASF kann keine Handelsangebote akzeptieren oder versenden!
> > > > > 
> > > > > Offensichtliche Sache zuerst - neue Konten starten als begrenzt. Bis Sie das Konto freischalten, indem Sie dessen Guthaben aufladen oder 5€ im Shop ausgeben, kann ASF weder Handelsangebote akzeptieren noch über dieses Konto versenden. In diesem Fall gibt ASF an, dass das Inventar leer ist, da jede Karte, die sich darin befindet, nicht handelbar ist. Es wird auch nicht möglich sein, ein Handelsangebot zu erhalten, da dieser Teil erfordert, dass ASF in der Lage ist, den API-Schlüssel zu holen, während die API-Schlüsselfunktionalität jedoch für begrenzte Konten deaktiviert ist. Kurz gesagt - der Handel ist für alle begrenzten Konten deaktiviert, keine Ausnahmen.
> > > > > 
> > > > > Als nächstes, wenn Sie **[ASF-2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-de-DE)** nicht verwenden, ist es möglich, dass ASF tatsächlich das Handelsangebot akzeptiert/sendet, aber Sie müssen es per E-Mail bestätigen. Ebenso müssen Sie, wenn Sie die klassische 2FA verwenden, das Handelsangebot über den Authentifikator bestätigen. Bestätigungen sind jetzt **obligatorisch**. Erwägen Sie den Authentifikator in ASF-2FA zu importieren, falls Sie diese nicht ständig manuell bestätigen möchten.
> > > > > 
> > > > > Beachten Sie auch, dass Sie nur mit Ihren Freunden und Personen mit bekanntem Handelslink handeln können. Sollten Sie versuchen, ein Handelsangebot *Bot->Master* zu veranlassen (z. B. mit `loot`), dann müssen Sie entweder den Bot auf Ihrer Freundesliste haben oder Ihr `SteamTradeToken` in der Bot-Konfiguration angegeben. Stellen Sie sicher, dass der Code gültig ist - sonst ist das Senden von Handelsangebote nicht möglich.
> > > > > 
> > > > > Abschließend sollten Sie daran denken, dass neue Geräte eine 7-Tage-Handels-Sperre haben. Falls Sie also gerade ein Konto zu ASF hinzugefügt haben, warten Sie mindestens 7 Tage. Alles nach diesem Zeitraum sollte funktionieren. Diese Einschränkung beinhaltet **sowohl** die Annahme, als **auch** das Versenden von Handelsangeboten. Es wird nicht immer ausgelöst, und es gibt Leute, die sofort Handelsangebote senden und annehmen können. Die Mehrheit der Personen ist jedoch betroffen, und die Sperre **wird** passieren, selbst wenn Sie Handelsangebote über Ihren Steam-Client auf der gleichen Maschine senden und annehmen können. Warte n Sie einfach geduldig, es gibt nichts, was Sie dagegen unternehmen können, um es zu beschleunigen. Ebenso kann es sein, dass Sie eine ähnliche Sperre für das Entfernen/Ändern verschiedener sicherheitsrelevanter Einstellungen von Steam erhalten, wie z. B. 2FA, SteamGuard, Passwort, E-Mail und ähnliches. Im Allgemeinen sollten Sie überprüfen, ob Sie ein Handelsangebot von diesem Konto aus selbst versenden können. Wenn ja, dann ist es sehr wahrscheinlich, dass es sich um eine klassische 7-Tage-Sperre von einem neuen Gerät handelt.
> > > > > 
> > > > > Bedenken Sie schließlich, dass ein Konto nur 5 ausstehende Handelsangebote zu einem anderen haben kann, sodass ASF keine Handelsangebote senden kann, wenn Sie bereits 5 (oder mehr) ausstehende Handelsangebote von diesem einen Bot vorhanden sind. Dies ist selten ein Problem, aber es ist auch erwähnenswert, besonders wenn Sie ASF auf automatisches Senden von Handelsangeboten einstellen, aber Sie benutzten ASF-2FA nicht und hast vergessen, sie tatsächlich zu bestätigen.
> > > > > 
> > > > > Wenn nichts geholfen hat, können Sie jederzeit den `Debug`-Modus aktivieren und selbst überprüfen, warum Anfragen fehlschlagen. Beachten Sie, dass Steam die meiste Zeit Unsinn redet, und vorausgesetzt, dass der Grund keinen logischen Sinn ergibt oder sogar völlig falsch ist. Wenn Sie sich entscheiden, diesen Grund zu interpretieren, stellen Sie sicher, dass Sie ein angemessenes Wissen über Steam und seine Besonderheiten besitzen. Es ist auch durchaus üblich, dieses Problem ohne logischen Grund zu sehen, und die einzige vorgeschlagene Lösung in diesem Fall ist, das Konto erneut zu ASF hinzuzufügen (und wieder 7 Tage zu warten). Manchmal behebt sich dieses Problem auf *magische* Weise, genauso wie es auch kaputt geht. Allerdings ist es in der Regel entweder nur eine siebentägige Handelssperre, temporäres Steam-Problem oder beides. Es ist am besten, ihm ein paar Tage vor der manuellen Überprüfung zu geben, was falsch ist, es sei denn, Sie haben den Drang, die wahre Ursache zu finden (und normalerweise werden Sie gezwungen sein, trotzdem zu warten, weil Fehlermeldungen keinen Sinn ergeben und Ihnen auch nicht im Geringsten helfen).
> > > > > 
> > > > > In jedem Fall kann ASF nur **versuchen**, eine ordnungsgemäße Anfrage an Steam zu senden, um das Handelsangebot anzunehmen/senden. Ob Steam diese Anfrage annimmt oder nicht, liegt außerhalb des Anwendungsbereichs von ASF, und ASF wird es nicht auf magische Weise beheben. Es gibt keinen Fehler im Zusammenhang mit dieser Funktion, und es gibt auch nichts zu verbessern, da die Logik außerhalb von ASF stattfindet. Bitten Sie deshalb nicht um die Behebung von Dingen, die nicht defekt sind, und fragen Sie auch nicht, warum ASF keine Handelsangebote annehmen oder senden kann. **Weder ich, noch ASF wissen es**. Leben Sie damit, oder beheben Sie es selbst, wenn Sie es besser wissen.
> > > > > 
> > > > > * * *
> > > > > 
> > > > > ### Warum muss ich bei der Anmeldung jedesmal den 2FA/SteamGuard-Code eingeben? / *Abgelaufener Anmelde-Schlüssel entfernt*
> > > > > 
> > > > > Sofern Sie `UseLoginKeys` aktivieren, verwendet ASF Anmelde-Schlüssel, um die Anmeldeinformationen gültig zu halten- den gleichen Mechanismus, den Steam verwendet - (2FA/SteamGuard-Code ist nur einmal erforderlich). Aufgrund von Steam-Netzwerkproblemen und -Eigenarten ist es jedoch durchaus möglich, dass der Anmelde-Schlüssel nicht im Netzwerk gespeichert ist. Ich habe solche Probleme bereits nicht nur mit ASF, sondern auch mit dem normalen Steam-Client gesehen (eine Notwendigkeit, bei jedem Durchlauf Login + Passwort einzugeben, unabhängig von der Option "Remember me").
> > > > > 
> > > > > Sie könnten (falls vorhanden) `BotName.db` (+ `BotName.bin`) des betroffenen Kontos entfernen und versuchen, ASF wieder mit Ihrem Konto zu verbinden, aber das muss nicht zum Erfolg führen. Die eigentliche ASF-basierte Lösung besteht darin, Ihren Authentifikator als **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-de-DE)** zu importieren. So kann ASF Codes automatisch generieren, wenn diese benötigt werden, und Sie müssen sie nicht manuell eingeben. In der Regel löst sich das Problem nach einiger Zeit von selbst, sodass man es einfach abwarten kann. Natürlich können Sie auch Valve nach einer Lösung fragen, denn ich kann das Steam-Netzwerk nicht zwingen, unsere Login-Schlüssel zu akzeptieren.
> > > > > 
> > > > > Als Randbemerkung können Sie auch die Anmelde-Schlüssel mit der Konfigurationseigenschaft `UseLoginKeys` mit `false` deaktivieren, aber das wird das Problem nicht lösen, sondern lediglich den Fehler des ersten Anmelde-Schlüssels überspringen. ASF ist sich des hier beschriebenen Problems bereits bewusst und wird versuchen, keine Anmelde-Schlüssel zu verwenden, wenn es sich selbst alle Anmeldeinformationen zuschicken kann, sodass es nicht notwendig ist, `UseLoginKeys` manuell zu optimieren, wenn Sie alle Zugangsdaten zusammen mit ASF 2FA angeben können.
> > > > > 
> > > > > * * *
> > > > > 
> > > > > ### Ich erhalte den Fehler: `Die Anmeldung bei Steam ist nicht möglich: InvalidPassword or RateLimitExceeded`
> > > > > 
> > > > > Dieser Fehler kann vieles bedeuten, unter anderem auch:
> > > > > 
> > > > > - Ungültige Login/Passwort-Kombination (offensichtlich)
> > > > > - Abgelaufener Anmelde-Schlüssel, der von ASF für die Anmeldung verwendet wird
> > > > > - Zu viele fehlgeschlagene Anmeldeversuche in kurzer Zeit (Anti-Bruteforce)
> > > > > - Zu viele Anmeldeversuche in kurzer Zeit (Rate-Limiting/ Anfragelimit)
> > > > > - Erfordernis eines Captcha zum Anmelden (sehr wahrscheinlich durch die beiden obigen Gründe verursacht)
> > > > > - Alle anderen Gründe, die das Steam-Netzwerk daran hindern könnten, sich anzumelden.
> > > > > 
> > > > > Im Falle von Anti-Bruteforce und Rate-Limiting wird das Problem nach einiger Zeit verschwinden Warten Sie einfach und versuche Sie sich in der Zwischenzeit nicht anzumelden. Sollten Sie dieses Problem häufiger haben, ist es vielleicht ratsam, `LoginLimiterDelay` Konfigurationseigenschaft von ASF zu erhöhen. Übermäßige Programmneustarts und andere absichtliche/nicht absichtliche Anmeldeanforderungen werden bei diesem Problem definitiv nicht helfen, also gilt es diese nach Möglichkeit zu vermeiden.
> > > > > 
> > > > > Im Falle eines abgelaufenen Anmelde-Schlüssels wird ASF den alten entfernen und beim nächsten Anmelden nach einem neuen fragen (was erfordert, dass Sie einen 2FA-Code einstellen, falls Ihr Konto 2FA-geschützt ist). Wenn das Konto ASF-2FA verwendet, wird ein Code generiert und automatisch verwendet. Dies kann natürlich mit der Zeit geschehen, aber wenn dieses Problem bei jedem Login auftritt, ist es möglich, dass Steam aus irgendeinem Grund beschlossen hat, unsere Login-Schlüssel zu ignorieren, wie im Punkt **[oben](#warum-muss-ich-bei-der-anmeldung-jedesmal-den-2fa)** erwähnt. Sie können natürlich `UseLoginKeys` komplett deaktivieren, aber das löst das Problem nicht, sondern vermeidet nur, dass jedes mal abgelaufene Login Schlüssel entfernt werden müssen. Die wirkliche Lösung besteht darin, wie im Punkt oben erwähnt, ASF-2FA zu verwenden.
> > > > > 
> > > > > Und schlussendlich, falls Sie eine falsche Kombination aus Login und Passwort verwenden, müssen Sie dies natürlich korrigieren oder den Bot deaktivieren, der versucht, sich mit diesen Zugangsdaten zu verbinden. ASF kann nicht selbstständig erraten, ob `InvalidPassword` ungültige Anmeldeinformationen oder einen der oben genannten Gründe bedeutet. Daher wird es ASF weiter versuchen, bis der Login erfolgreich ist.
> > > > > 
> > > > > Bedenken Sie, dass ASF über ein eigenes eingebautes System verfügt, um entsprechend auf Steam-Eigenarten zu reagieren, irgendwann wird es sich verbinden und seine Arbeit wieder aufnehmen. Daher ist es nicht erforderlich, etwas zu tun, solange es sich um ein temporäres Problem handelt. Ein Neustart von ASF, wird Probleme nicht auf magische Weise beheben, sondern die Situation nur verschlimmern (da der neue ASF den vorherigen ASF-Zustand nicht kennt, dass er sich nicht anmelden kann, und versucht, eine Verbindung herzustellen, anstatt zu warten). Folglich sollten Sie es unterlassen, es sei denn, Sie wissen genau was Sie tun.
> > > > > 
> > > > > Abschließend kann ASF (wie bei jeder Steam-Anfrage) nur **versuchen**, sich mit den von Ihnen angegebenen Zugangsdaten anmelden. Ob diese Anforderung erfolgreich ist oder nicht, liegt außerhalb des Umfangs und der Logik von ASF - es gibt keinen Fehler, und in dieser Hinsicht kann nichts behoben oder verbessert werden.
> > > > > 
> > > > > * * *
> > > > > 
> > > > > ### `System.IO.IOException: Input/output error`
> > > > > 
> > > > > Wenn dieser Fehler während der ASF-Eingabe aufgetreten ist (z.B. wenn Sie `Console.ReadLine()` im Stacktrace sehen können), dann wird er durch Ihre Umgebung verursacht, die es ASF verbietet, die Standardeingabe der Konsole zu lesen. Das kann aus vielen Gründen passieren, aber der häufigste ist, dass Sie ASF in der falschen Umgebung verwenden (z.B. in `nohup` oder `&` Hintergrund anstelle von `screen` unter Linux). Wenn ASF nicht auf die Standardeingabe zugreifen kann, wird dieser Fehler protokolliert und ASF kann Ihre Daten während der Ausführung nicht verwenden.
> > > > > 
> > > > > Falls Sie dieses Verhalten **erwarten**, Sie also ASF in einer eingabefreien Umgebung ausführen **möchten**, dann sollten Sie ASF ausdrücklich mitteilen, dass dies der Fall ist, indem Sie den **[`Headless`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-de-DE#headless)** Modus entsprechend einstellen. Um für Sie ein sicheres Ausführen in eingabefreien Umgebungen zu gewährleisten, wird ASF damit angehalten, nach weiteren Benutzereingaben fragen.
> > > > > 
> > > > > * * *
> > > > > 
> > > > > ### `System.Net.Http.WinHttpException: A security error occurred`
> > > > > 
> > > > > Dieser Fehler tritt auf, wenn ASF keine sichere Verbindung mit dem angegebenen Server herstellen kann, fast ausschließlich wegen des Misstrauens gegen ein SSL-Zertifikat.
> > > > > 
> > > > > In fast allen Fällen wird dieser Fehler durch ein **falsches Datum/Uhrzeit auf deiner Maschine** verursacht. Jedes SSL-Zertifikat hat ein Ausstellungsdatum und ein Verfallsdatum. Wenn dein Datum ungültig ist und außerhalb dieser beiden Grenzen liegt, kann dem Zertifikat nicht vertraut werden, da es als potentieller MITM-Angriff ausgenutzt werden kann und ASF weigert sich, eine Verbindung herzustellen.
> > > > > 
> > > > > Eine offensichtliche Lösung ist das Datum auf deiner Maschine entsprechend einzustellen. Es wird dringend empfohlen, eine automatische Datumssynchronisation zu verwenden, wie z.B. die unter Windows verfügbare native Synchronisation oder `ntpd` unter Linux.
> > > > > 
> > > > > Wenn du sichergestellt hast, dass das Datum auf deiner Maschine korrekt ist und der Fehler nicht verschwinden will, dann, angenommen, es ist kein temporäres Problem das bald verschwindet, könnten SSL-Zertifikate, denen dein System vertraut, veraltet oder ungültig sein. In diesem Fall solltest du sicherstellen, dass deine Maschine sichere Verbindungen herstellen kann, z.B. indem du über einen beliebigen Browser oder mit einem CLI-Tool wie `curl` überprüfst, ob du auf `https://github.com` zugreifen kannst. Wenn du bestätigt hast, dass dies ordnungsgemäß funktioniert, kannst du das Problem gerne in unserer Steam-Gruppe melden.
> > > > > 
> > > > > * * *
> > > > > 
> > > > > ### `System.Threading.Tasks.TaskCanceledException: A task was canceled.`
> > > > > 
> > > > > Diese Warnung bedeutet, dass Steam nicht innerhalb einer bestimmten Zeit auf die ASF-Anfrage geantwortet hat. Normalerweise wird dies durch Steam-Netzwerkproblemen verursacht und hat keine Auswirkung auf ASF. In manchen Fällen ist es das gleiche wie wenn die Anfrage nach 5 Versuchen fehlschlägt. Die Meldung dieses Problems macht meistens keinen Sinn, da wir Steam nicht zwingen können, auf unsere Anfragen zu reagieren.
> > > > > 
> > > > > * * *
> > > > > 
> > > > > ### `The type initializer for 'System.Security.Cryptography.CngKeyLite' threw an exception`
> > > > > 
> > > > > Dieses Problem wird fast ausschließlich durch einem deaktivierten/gestoppten `CNG Key Isolation`-*Windows*-Dienst, die ASF Kernkryptographie-Funktionalität bietet, ohne dessen das Programm nicht ausführbar ist. Sie können dieses Problem beheben, indem Sie `services.msc` und sicherstellen, dass *Windows* den`CNG Key Isolation`-Dienst nicht deaktiviert hat und derzeit läuft.
> > > > > 
> > > > > * * *
> > > > > 
> > > > > ### ASF wird von meinem AntiVirus als Schadsoftware erkannt! Was ist hier los?
> > > > > 
> > > > > **Stellen Sie sicher, dass ASF von einer vertrauenswürdigen Quelle heruntergeladen wurde**. Die einzige offizielle und vertrauenswürdige Quelle ist **[ASF-Veröffentlichungen](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** Seite auf GitHub (und das ist auch die Quelle für automatische ASF-Aktualisierungen) - **jede andere Quelle ist per Definition nicht vertrauenswürdig und könnte Schadsoftware enthalten, die von anderen Personen hinzugefügt wurde** - Du solltest keiner anderen Quelle per Definition vertrauen und sicherstellen, dass dein ASF immer von uns kommt.
> > > > > 
> > > > > Wenn du überprüft hast, dass ASF von einer vertrauenswürdigen Quelle heruntergeladen wurde, dann ist es höchstwahrscheinlich nur ein Fehlalarm. Dies **passierte in der Vergangenheit**, **passiert gerade jetzt**, und **wird in der Zukunft passieren**. Wenn du dir um die tatsächliche Sicherheit bei der Verwendung von ASF Sorgen machst, dann schlage ich vor, ASF mit vielen verschiedenen AVs nach der tatsächlichen Erkennungsrate zu scannen, zum Beispiel durch **[VirusTotal](https://www.virustotal.com)** (oder einem anderen Webdienst deiner Wahl).
> > > > > 
> > > > > Wenn die von dir verwendete AV-Software ASF fälschlicherweise als Schadsoftware erkennt, dann **ist es eine gute Idee, dieses Dateibeispiel an die Entwickler deiner AV-Software zu senden, damit sie es analysieren und ihre Erkennungsmaschine verbessern können**, da sie offensichtlich nicht so gut funktioniert, wie du denkst. Es gibt kein Sicherheitsproblem im ASF-Code, und es gibt auch nichts zu beheben, da wir keine Schadsoftware verteilen, daher macht es auch keinen Sinn uns diese Fehlalarme zu melden. Wir empfehlen dringend, ASF-Beispiele für weitere Analysen wie oben beschrieben zu versenden, aber wenn du dich nicht darum kümmern möchtest, dann kannst du ASF immer zu den AV-Ausnahmen hinzufügen, deine AV-Software deaktivieren oder einfach ein andere verwenden. Leider sind wir es gewohnt, dass AV-Softwares dumm sind, da ab und zu einige AV-Softwares ASF als Virus erkennen, was normalerweise sehr kurz andauert und schnell von den Entwicklern ausgebessert wird, aber wie wir oben erwähnt haben - **es passierte**, **passiert** und **wird immer passieren**. ASF enthält keinen schädlichen Code, du kannst ASF-Code überprüfen und sogar selbst den Quellcode kompilieren. Wir sind keine Hacker die den ASF-Code verschleiern, um sich vor AV-Heuristiken und Fehlalarmen zu verstecken, also erwarte nicht von uns, dass wir reparieren, was nicht kaputt ist - es gibt keinen "Virus", den wir beseitigen können.