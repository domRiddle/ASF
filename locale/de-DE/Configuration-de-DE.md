# Konfiguration

Diese Seite widmet sich der Konfiguration von ASF. Es dient als eine lückenlose Dokumentation des `config` Verzeichnisses, so dass du ASF auf deine Bedürfnisse abstimmen kannst.

- **[Einleitung](#einleitung)**
- **[Web-basierter ConfigGenerator](#web-basierter-configgenerator)**
- **[Manuelle Konfiguration](#manuelle-konfiguration)**
- **[Globale Konfiguration](#globale-konfiguration)**
- **[Bot Konfiguration](#bot-konfiguration)**
- **[Dateistruktur](#dateistruktur)**
- **[JSON-Mapping](#json-mapping)**
- **[Kompatibilitätsmapping](#kompatibilitätsmapping)**
- **[Konfigurations-Kompatibilität](#konfigurations-kompatibilität)**
- **[Automatisches Nachladen](#automatisches-nachladen)**

* * *

## Einleitung

Die ASF-Konfiguration gliedert sich in zwei Hauptteile - die globale (Prozess-)Konfiguration und die Konfiguration jedes einzelnen Bots. Jeder Bot hat seine eigene Bot-Konfigurationsdatei namens `BotName.json` (wobei `BotName` der Name des Bots ist), während die globale ASF (Prozess-)Konfiguration eine einzige Datei namens `ASF.json` ist.

Ein Bot ist ein einzelnes Steam-Konto welches am ASF-Prozess teilnimmt. Um ordnungsgemäß zu funktionieren benötigt ASF mindestens **eine** definierte Bot-Instanz. Es gibt keine prozessbedingte Begrenzung der Bot-Instanzen, so dass du so viele Bots (Steam-Konten) verwenden kannst wie du willst.

ASF verwendet das **[JSON](https://en.wikipedia.org/wiki/JSON)** Format zum Speichern seiner Konfigurationsdateien. Es ist ein benutzerfreundliches, lesbares und sehr universelles Format in dem du das Programm konfigurieren kannst. Keine Sorge, du musst kein JSON können um ASF zu konfigurieren. Ich habe es nur erwähnt, falls du bereits daran denkst ASF-Konfigurationen mit einer Art Bash-Skript massenhaft zu erstellen.

Die Konfiguration kann entweder manuell, durch Erstellung korrekter JSON-Konfigurationen, erfolgen oder durch die Verwendung unseres **[Web-basierten ConfigGenerators](https://justarchinet.github.io/ASF-WebConfigGenerator)**, was viel einfacher und komfortabler sein sollte. Wenn du kein fortgeschrittener Benutzer bist, schlage ich vor, den Konfigurationsgenerator zu verwenden, der im Folgenden beschrieben wird.

**[Zum Seitenanfang](#konfiguration)**

* * *

## Web-basierter ConfigGenerator

Der Zweck des web-basierten ConfigGenerator ist es, dir ein benutzerfreundliches Frontend zur Verfügung zu stellen das zum Erzeugen von ASF-Konfigurationsdateien verwendet werden kann. Der web-basierte ConfigGenerator ist zu 100% Client-basiert, was bedeutet, dass die von dir eingegebenen Daten nirgendwo hin gesendet, sondern nur lokal verarbeitet werden. Dies garantiert Sicherheit und Zuverlässigkeit, da es sogar **[offline](https://github.com/JustArchiNET/ASF-WebConfigGenerator/tree/master/docs)** funktioniert, wenn du alle Dateien herunterlädst und `index.html` in deinem Lieblingsbrowser öffnest.

Der web-basierte ConfigGenerator wurde unter Chrome und Firefox getestet, sollte aber in allen gängigen JavaScript-fähigen Browsern ordnungsgemäß funktionieren.

Die Verwendung ist denkbar einfach - man wählt über die entsprechenden Registerkarten, ob man eine Konfiguration für `ASF` oder `Bot` erstellen möchte, überprüft ob die gewählte Version der Konfigurationsdatei zur verwendeten ASF-Version passt, füllt dann die entsprechenden Felder aus und klickt abschließend auf die Schaltfläche "Herunterladen". Verschiebe diese Datei in das ASF `config` Verzeichnis und überschreibe bei Bedarf vorhandene Dateien. Wiederhole diese Schritte bei allen zukünftigen Anpassungen. Der Rest dieses Abschnitts erklärt alle zur Verfügung stehenden Konfigurationsmöglichkeiten.

**[Zum Seitenanfang](#konfiguration)**

* * *

## Manuelle Konfiguration

Ich empfehle dir unbedingt den web-basierten ConfigGenerator zu verwenden. Solltest du diesen aus irgendeinem Grund nicht verwenden wollen kannst du natürlich auch selbst gültige Konfigurationen erstellen. Sieh dir die folgenden JSON-Beispiele an, um einen guten Start in die richtige Struktur zu erhalten. Du kannst den Inhalt in eine Datei kopieren und als Basis für deine Konfiguration verwenden. Da du nicht unser Frontend verwendest, stelle sicher, dass deine Konfiguration **[gültig](https://jsonlint.com)** ist, da ASF sich weigert, sie zu laden, wenn sie nicht verarbeitet werden kann. Für eine korrekte JSON-Struktur aller verfügbaren Felder sieh dir den Abschnitt **[JSON-Mapping](#json-mapping)** und die Dokumentation unten an.

**[Zum Seitenanfang](#konfiguration)**

* * *

## Globale Konfiguration

Die globale Konfiguration befindet sich in der Datei `ASF.json` und hat folgende Struktur:

```json
{
    "AutoRestart": true,
    "Blacklist": [],
    "CommandPrefix": "!",
    "ConfirmationsLimiterDelay": 10,
    "ConnectionTimeout": 60,
    "CurrentCulture": null,
    "Debug": false,
    "FarmingDelay": 15,
    "GiftsLimiterDelay": 1,
    "Headless": false,
    "IdleFarmingPeriod": 8,
    "InventoryLimiterDelay": 3,
    "IPC": false,
    "IPCPassword": null,
    "LoginLimiterDelay": 10,
    "MaxFarmingTime": 10,
    "MaxTradeHoldDuration": 15,
    "OptimizationMode": 0,
    "Statistics": true,
    "SteamMessagePrefix": "/me ",
    "SteamOwnerID": 0,
    "SteamProtocols": 7,
    "UpdateChannel": 1,
    "UpdatePeriod": 24,
    "WebLimiterDelay": 200,
    "WebProxy": null,
    "WebProxyPassword": null,
    "WebProxyUsername": null
}
```

**Tipp:** Sofern du keine dieser Optionen ändern möchtest solltest du alles auf den Standardwerten belassen. Deshalb kannst du `ASF.json` schließen und zur Bot-Konfiguration übergehen.

* * *

Alle Optionen werden nachfolgend erklärt:

### `AutoRestart`

`bool` Typ mit Standardwert von `true`. Diese Eigenschaft legt fest, ob ASF bei Bedarf einen Selbst-Neustart durchführen darf. Es gibt ein paar Fälle, die von ASF einen Neustart erfordern, wie z.B. ASF-Aktualisierung (durchgeführt mit `UpdatePeriod` oder dem Befehl `update`), sowie das Verändern der `ASF.json` Konfiguration, `restart` Befehl und ähnlich. Normalerweise beinhaltet der Neustart zwei Teile - das Erstellen eines neuen Prozesses und das Beenden des aktuellen Prozesses. Die meisten Benutzer sollten damit einverstanden sein und diese Eigenschaft auf dem Standardwert `true` behalten - wenn du ASF durch dein eigenes Skript und/oder mit `dotnet` ausführst, solltest du vielleicht die volle Kontrolle über den Start des Prozesses haben und eine Situation vermeiden, in der ein neuer (neu gestarteter) ASF-Prozess irgendwo im Hintergrund und nicht im Vordergrund des Skripts läuft, der zusammen mit dem alten ASF-Prozess beendet wurde. Dies ist besonders wichtig, wenn man bedenkt, dass der neue Prozess nicht mehr dein direktes Kind ist, was dich z.B. nicht in die Lage versetzen würde, die Standardkonsolen-Eingabe dafür zu verwenden.

Wenn das der Fall ist, ist diese Eigenschaft speziell für dich und du kannst sie auf `false` setzen. Bedenke jedoch, dass in diesem Fall **du** für den Neustart des Prozesses verantwortlich bist. Dies ist irgendwie wichtig, da ASF nur beendet wird, anstatt einen neuen Prozess zu starten (z.B. nach der Aktualisierung), so dass, wenn keine Logik von dir hinzugefügt wird, es einfach aufhört zu funktionieren, bis du es wieder startest. ASF beendet sich immer mit einem korrekten Fehlercode, der Erfolg (Null) oder Misserfolg (ungleich Null) anzeigt, auf diese Weise kannst du in deinem Skript eine korrekte Logik hinzufügen, die einen automatischen Neustart von ASF im Fehlerfall vermeiden sollte, oder zumindest eine lokale Kopie von `log.txt` für weitere Analysen erstellen. Bedenke auch, dass der Befehl `restart` ASF immer neu startet, unabhängig davon, wie diese Eigenschaft eingestellt ist, da diese Eigenschaft das Standardverhalten definiert, während der Befehl `restart` den Prozess immer neu startet. Wenn du keinen Grund hast, diese Funktion zu deaktivieren, solltest du sie aktiviert lassen.

* * *

### `Blacklist`

`ImmutableHashSet<uint>` Typ mit einem leeren Standardwert. Wie der Name schon sagt, definiert diese globale Konfigurationseigenschaft AppIDs (Spiele), die vom automatischen ASF-Sammel-Prozess vollständig ignoriert werden. Leider liebt es Steam, Sommer/Winter-Sale-Abzeichen als "verfügbar für Kartenabgabe" zu kennzeichnen, was den ASF-Prozess verwirrt, indem es den Eindruck erweckt, dass es sich um ein gültiges Spiel handelt, das gesammelt werden sollte. Wenn es keine schwarze Liste gäbe, würde ASF irgendwann beim Sammeln eines Spiels "hängen" bleiben, das eigentlich kein Spiel ist, und unendlich warten, bis die Karten gesammelt wurden, was nicht passieren wird. Die schwarze Liste von ASF dient dazu, diese Abzeichen als nicht für das Sammeln verfügbar zu kennzeichnen, so dass wir sie bei der Entscheidung, was zu Sammeln ist, stillschweigend ignorieren und nicht in die Falle tappen können.

ASF enthält standardmäßig zwei schwarze Listen - `GlobalBlacklist`, die fest in den ASF-Code kodiert und nicht editierbar ist, und normale `Blacklist`, die hier definiert ist. `GlobalBlacklist` wird zusammen mit der ASF-Version aktualisiert und enthält typischerweise alle "schlechten" AppIDs zum Zeitpunkt der Veröffentlichung, so dass du, wenn die aktuelle Version von ASF verwendet wird, nicht deine eigene `Blacklist` pflegen musst, die hier definiert ist. Der Hauptzweck dieser Eigenschaft ist es, dir die Möglichkeit zu geben, neue, zum Zeitpunkt der ASF-Veröffentlichung unbekannte AppIDs auf die schwarze Liste zu setzen, die nicht gesammelt werden sollten. Die fest programmierte `GlobalBlacklist` wird so schnell wie möglich aktualisiert, daher ist es nicht erforderlich, dass du deine eigene `Blacklist` aktualisierst, wenn du die neueste ASF-Version verwendest, aber ohne `Blacklist` wärst du gezwungen, ASF zu aktualisieren, um "weiterzumachen", wenn Valve ein neues Sale-Abzeichen veröffentlicht - ich möchte dich nicht zwingen, den neuesten ASF-Code zu verwenden, daher ist diese Eigenschaft hier, um es dir zu ermöglichen, ASF selbst zu "reparieren", wenn du aus irgendeinem Grund nicht auf neue fest programmierte `GlobalBlacklist` in der neuen ASF-Version aktualisieren willst oder kannst, aber du willst dein altes ASF am Laufen halten. Wenn du keinen **triftigen** Grund hast, diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

Wenn du stattdessen nach einer Bot-basierten schwarzen Liste suchst, schaue dir `ib`, `ibadd` und `ibrm` **[Befehle](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE)** an.

* * *

### `CommandPrefix`

`string` Typ mit Standardwert von `!`. This property specifies **case-sensitive** prefix used for ASF **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. In other words, this is what you need to prepend to each ASF command in order to make ASF listen to you. Es ist möglich, diesen Wert auf `null` oder leer zu setzen, damit ASF kein Befehlspräfix verwendet, in diesem Fall gibst du Befehle mit ihren einfachen Bezeichnern ein. Dies kann jedoch die Leistung von ASF beeinträchtigen, da ASF optimiert ist, um Nachrichten nicht weiter zu analysieren, wenn sie nicht mit `CommandPrefix` beginnen - wenn du dich absichtlich dazu entscheidest, sie nicht zu verwenden, wird ASF gezwungen sein, alle Nachrichten zu lesen und darauf zu reagieren, selbst wenn es sich nicht um ASF-Befehle handelt. Daher wird empfohlen, weiterhin irgendein `CommandPrefix` zu verwenden, wie z.B. `/`, wenn du den Standardwert von `!` nicht magst. Aus Konsistenzgründen betrifft `CommandPrefix` den gesamten ASF-Prozess. Wenn du keinen Grund hast, diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

* * *

### `ConfirmationsLimiterDelay`

`byte` Typ mit Standardwert von `10`. ASF wird sicherstellen, dass mindestens `ConfirmationsLimiterDelay` Sekunden zwischen zwei aufeinanderfolgenden 2FA-Bestätigungen liegen, die Anfragen abrufen, um eine Auslösung des Ratenlimits zu vermeiden - diese werden von **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-de-DE)** während z.B. des Befehls `2faok` sowie bei verschiedenen handelsbezogenen Operationen nach Bedarf verwendet. Der Standardwert wurde auf Basis unserer Tests festgelegt und sollte nicht verringert werden, wenn du auf keine Probleme stoßen möchtest. Wenn du keinen **triftigen** Grund hast, diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

* * *

### `ConnectionTimeout`

`byte` Typ mit Standardwert von `60`. Diese Eigenschaft definiert die Auszeiten für verschiedene Netzwerkaktionen, die von ASF ausgeführt werden, in Sekunden. Im Einzelnen definiert `ConnectionTimeout` die Auszeit in Sekunden für HTTP- und IPC-Anfragen, `ConnectionTimeout / 10` definiert die maximale Anzahl der fehlgeschlagenen Heartbeats, während `ConnectionTimeout / 30` die Anzahl der Minuten definiert, die wir für die initiale Steam-Netzwerkverbindungsanforderung berücksichtigen. Der Standardwert von `60` sollte für die Mehrheit der Benutzer in Ordnung sein, aber wenn du eine eher langsame Netzwerkverbindung oder einen PC hast, solltest du diese Zahl vielleicht um etwas Höheres erhöhen (wie `90`). Bedenke, dass höhere Werte langsame oder sogar unzugängliche Steam-Server nicht auf magische Weise reparieren, also sollten wir nicht unendlich auf etwas warten, das nicht passieren wird, und es einfach später noch einmal versuchen. Eine zu hohe Einstellung dieses Wertes führt zu einer übermäßigen Verzögerung bei der Erfassung von Netzwerkproblemen und zu einer Verringerung der Gesamtleistung. Wenn du diesen Wert zu niedrig einstellst, verringert sich auch die Gesamtstabilität und -leistung, da ASF eine gültige Anfrage abbricht, die noch verarbeitet wird. Daher hat das Setzen dieses Wertes unter dem Standardwert im Allgemeinen keinen Vorteil, da Steam-Server von Zeit zu Zeit langsam sind und mehr Zeit für das Verarbeiten von ASF-Anfragen benötigen könnten. Der Standardwert ist eine ausgewogene Balance zwischen dem Vertrauen, dass unsere Netzwerkverbindung stabil ist, und dem Misstrauen des Steam-Netzwerks, unsere Anfrage innerhalb einer bestimmten Auszeit zu bearbeiten. Wenn du Probleme früher erkennen und die ASF-Wiederverbindung bzw. -Antwort beschleunigen möchtest, sollte der Standardwert dies tun (oder ganz knapp darunter, wodurch ASF weniger geduldig wird). Wenn du stattdessen bemerkst, dass ASF auf Netzwerkprobleme stößt, wie z.B. fehlgeschlagene Anfragen, Heartbeats, die verloren gehen oder die Verbindung zu Steam unterbrochen wird, könnte es sinnvoll sein, diesen Wert zu erhöhen, wenn du sicher bist, dass es sich um **nicht** um Fehler handelt, die durch dein Netzwerk, sondern durch Steam selbst verursacht werden, da zunehmende Auszeiten ASF mehr "geduldig" machen und sich nicht entscheiden, die Verbindung sofort wieder herzustellen. Es kann auch sinnvoll sein diesen Wert zu erhöhen, wenn du ein eher langsames Internet hast, das mehr Zeit für die Verarbeitung der übertragenen Daten benötigt. Kurz gesagt, der Standardwert sollte in den meisten Fällen angemessen sein, aber du kannst ihn bei Bedarf erhöhen. Trotzdem macht es keinen großen Sinn, weit über den Standardwert hinauszugehen, da größere Auszeiten unzugängliche Steam-Server nicht magisch beheben. Wenn du keinen Grund hast, diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

* * *

### `CurrentCulture`

`string` Typ mit Standardwert von `null`. Standardmäßig versucht ASF, die Sprache deines Betriebssystems zu verwenden, und wird es vorziehen, übersetzte Zeichenketten in dieser Sprache zu verwenden, falls verfügbar. Dies ist möglich dank unserer Community, die versucht, ASF in allen gängigen Sprachen zu **[übersetzen](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Localization)**. Wenn du aus irgendeinem Grund deine Betriebssystem-Muttersprache nicht verwenden möchtest, kannst du diese Konfigurationseigenschaft verwenden, um eine gültige Sprache auszuwählen, die du stattdessen verwenden möchtest. Für eine Liste aller verfügbaren Sprachen besuche bitte **[MSDN](https://msdn.microsoft.com/en-us/library/cc233982.aspx)** und suche nach `Language tag`. Es ist gut zu erwähnen, dass ASF sowohl spezifische Sprachen wie `en-GB` als auch neutrale Sprachen wie `en` akzeptiert. Die Angabe der aktuellen Sprache kann auch andere sprachspezifische Aspekte beeinflussen, wie z.B. das Währungs-/Datumsformat und ähnliches. Bitte bedenke, dass du möglicherweise zusätzliche Schrift-/Sprachpakete benötigst, um sprachspezifische Zeichen richtig darzustellen, wenn du eine nicht-native Sprache gewählt hast. Normalerweise willst du diese Konfigurationseigenschaft nutzen, wenn du ASF auf Englisch anstelle deiner Muttersprache bevorzugst.

* * *

### `Debug`

`bool` Typ mit Standardwert von `false`. Diese Eigenschaft legt fest, ob der Prozess im Debug-Modus laufen soll. Im Debug-Modus erstellt ASF ein spezielles Verzeichnis `debug` neben dem `config` Verzeichnis, das die gesamte Kommunikation zwischen ASF und den Steam-Servern verfolgt. Debug-Informationen können helfen, lästige Probleme im Zusammenhang mit der Vernetzung und dem allgemeinen ASF-Arbeitsablauf zu erkennen. Darüber hinaus werden einige Programmroutinen viel ausführlicher sein, wie z.B. `WebBrowser`, die den genauen Grund für das Scheitern einiger Anfragen angeben - diese Einträge werden in das normale ASF-Protokoll geschrieben. **Du solltest ASF nicht im Debug-Modus ausführen, es sei denn, du wirst von einem Entwickler dazu aufgefordert**. Das Ausführen von ASF im Debug-Modus **verringert die Leistung**, **beeinträchtigt die Stabilität negativ** und ist **weitaus ausführlicher an verschiedenen Stellen**, daher sollte es **nur** gewollt und kurzfristig, für das Debuggen bestimmter Probleme, die Reproduktion des Problems oder das Erhalten von mehr Informationen über eine fehlgeschlagene Anfrage und ähnliches verwendet werden, aber **nicht** für die normale Programmausführung. Du wirst **viele** neue Fehler, Probleme und Ausnahmen sehen - stelle sicher, dass du ein gutes Wissen über ASF, Steam und seine Besonderheiten hast, wenn du dich entscheidest, das alles selbst zu analysieren, da nicht alles relevant ist.

**WARNUNG:** Das Aktivieren dieses Moduses beinhaltet das Protokollieren von **potenziell sensiblen** Informationen wie Anmeldeinformationen und Passwörter, die du für die Anmeldung bei Steam verwendest (aufgrund von Netzwerkprotokollierung). Diese Daten werden sowohl in das Verzeichnis `debug` als auch in die normale `log.txt` Datei geschrieben (diese ist nun absichtlich viel umfangreicher, um diese Information zu protokollieren). Du solltest keine Debug-Inhalte, die von ASF generiert wurden, an einem öffentlichen Ort posten, der ASF-Entwickler sollte dich immer daran erinnern, sie an seine E-Mail oder einen anderen sicheren Ort zu senden. Wir speichern diese sensiblen Details nicht und nutzen sie auch nicht, sie werden als Teil von Debug-Routinen geschrieben, da ihre Anwesenheit für das Problem, das dich betrifft, relevant sein könnte. Wir würden es vorziehen, wenn du das ASF-Logging in keiner Weise ändern würdest, aber wenn du besorgt bist, kannst du diese sensiblen Details entsprechend bearbeiten.

> Beim Editieren werden sensible Details, z.B. durch Sterne, ersetzt. Du solltest darauf verzichten, sensible Zeilen ganz zu entfernen, da ihre reine Existenz relevant sein könnte und erhalten werden sollte.

* * *

### `FarmingDelay`

`byte` Typ mit Standardwert von `15`. Damit ASF funktioniert, wird es alle `FarmingDelay` Minuten das aktuell gesammelte Spiel überprüfen, ob es vielleicht schon alle Karten erhalten hat. Wenn du diese Eigenschaft zu niedrig einstellst, kann dies dazu führen, dass eine übermäßige Anzahl von Steam-Anfragen gesendet wird, während eine zu hohe Einstellung dazu führen kann, dass ASF immer noch den vorgegebenen Titel für bis zu `FarmingDelay` Minuten, nachdem es vollständig gesammelt wurde, "sammelt". Der Standardwert sollte für die meisten Benutzer hervorragend sein, aber wenn du viele Bots im Einsatz hast, kannst du ihn auf etwa `30` Minuten erhöhen, um das Senden von Steam-Anfragen zu begrenzen. It's nice to note that ASF uses event-based mechanism and checks game badge page on each Steam item dropped, so in general **we don't even need to check it in fixed time intervals**, but as we don't fully trust Steam network - we check game badge page anyway, if we didn't check it through card being dropped event in last `FarmingDelay` minutes (in case Steam network didn't inform us about item dropped or stuff like that). Angenommen, das Steam-Netzwerk funktioniert ordnungsgemäß, wird die Herabsetzung dieses Wertes **in keiner Weise die Effizienz des Sammelns verbessern**, während **die Erhöhung des Netzwerkaufwandes dies signifikant tut** - es wird empfohlen, es nur (falls erforderlich) vom Standardwert aus auf `15` Minuten zu erhöhen. Wenn du keinen **triftigen** Grund hast, diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

* * *

### `GiftsLimiterDelay`

`byte` Typ mit Standardwert von `1`. ASF wird sicherstellen, dass mindestens `GiftsLimiterDelay` Sekunden zwischen zwei aufeinanderfolgenden Anfragen zur Handhabung (Einlösen) von Geschenken/Produktschlüsseln/Lizenzen liegen, um zu vermeiden, dass ein Anfragen-Limit ausgelöst wird. Darüber hinaus wird es auch als globaler Begrenzer für Spiele-Listen-Anfragen verwendet, wie z.B. die vom `owns` **[Befehl](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE)**. Wenn du keinen **triftigen** Grund hast, diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

* * *

### `Headless`

`bool` Typ mit Standardwert von `false`. Diese Eigenschaft legt fest, ob der Prozess im Headless-Modus laufen soll. Im Headless-Modus geht ASF davon aus, dass es auf einem Server oder in einer anderen nicht interaktiven Umgebung läuft, daher wird es nicht versuchen, wichtige Konto-Anmeldeinformationen wie 2FA-Code, SteamGuard-Code, Passwort oder eine andere Variable zu lesen, die für den Betrieb von ASF erforderlich ist. Dies ist vergleichbar mit der schreibgeschützten ASF-Konsole. Der `Headless`-Modus ist vor allem für Benutzer nützlich, die ASF auf ihren Servern ausführen, da ASF, anstatt z.B. nach 2FA-Code zu fragen, den Vorgang stillschweigend abbricht, indem es ein Konto stoppt. Wenn du ASF nicht auf einem Server ausführst und vorher bestätigt hast, dass ASF im Nicht-Headless-Modus funktionieren kann, solltest du diese Eigenschaft deaktiviert lassen. Jegliche Benutzerinteraktion wird im Headless-Modus verweigert, und deine Konten werden nicht ausgeführt, wenn sie beim Start **irgendwelche** Konsolen-Eingaben erfordern. Dies ist für Server nützlich, da ASF den Versuch, sich am Konto anzumelden, wenn nach Anmeldeinformationen gefragt wird, abbrechen kann, anstatt (unendlich) darauf zu warten, dass der Benutzer diese bereitstellt. Wenn du diesen Modus aktivierst, kannst du auch den `input` **[Befehl](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE)** verwenden, der als Ersatz für die standardmäßige Konsolen-Eingabe dient. Wenn du dir nicht sicher bist, wie du diese Eigenschaft einstellen sollst, belasse sie bei dem Standardwert `false`.

Wenn du ASF auf dem Server verwendest, solltest du diese Option zusammen mit dem **[Befehlszeilenargument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments)** `--process-required` verwenden.

* * *

### `IdleFarmingPeriod`

`byte` Typ mit Standardwert von `8`. Wenn ASF nichts zu sammeln hat, wird es regelmäßig alle `IdleFarmingPeriod` Stunden überprüfen, ob das Konto vielleicht einige neue Spiele zum Sammeln hat. Dieses Feature ist nicht erforderlich, wenn es sich um neue Spiele handelt, die wir erhalten, da ASF intelligent genug ist, um in diesem Fall automatisch die Abzeichen-Seiten zu überprüfen. `IdleFarmingPeriod` ist hauptsächlich für Situationen wie alte Spiele gedacht, bei denen wir bereits Karten hinzugefügt haben. In diesem Fall gibt es kein Ereignis, weshalb ASF regelmäßig die Abzeichen-Seiten überprüfen muss, wenn wir dies berücksichtigen wollen. Der Wert von `0` deaktiviert dieses Feature. Siehe auch: `ShutdownOnFarmingFinished`.

* * *

### `InventoryLimiterDelay`

`byte` Typ mit Standardwert von `3`. ASF wird sicherstellen, dass sich mindestens `InventoryLimiterDelay` Sekunden zwischen zwei aufeinanderfolgenden Inventar-Anfragen befinden, um zu vermeiden, dass ein Anfragen-Limit ausgelöst wird - diese werden für das Abrufen deines eigenen Inventars verwendet (und nur für dieses). Der Standardwert von `3` wurde basierend auf dem Plündern von über 100 Bot-Instanzen festgelegt und sollte die meisten (wenn nicht alle) Benutzer zufrieden stellen. Du kannst es aber auch verringern oder sogar zu `0` wechseln, wenn du eine sehr geringe Anzahl von Bots hast, so dass ASF die Verzögerung ignoriert und die Steam-Inventare viel schneller plündert. Sei jedoch gewarnt, dass das Setzen eines zu niedrigen Wertes **dazu führt**, dass Steam deine IP vorübergehend blockiert, und das wird dich daran hindern, dein Inventar überhaupt abzurufen. Du musst diesen Wert möglicherweise auch erhöhen, wenn du viele Bots mit vielen Inventar-Anfragen ausführst, obwohl du in diesem Fall wahrscheinlich versuchen solltest, die Anzahl dieser Anfragen zu begrenzen. Wenn du keinen **triftigen** Grund hast, diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

* * *

### `IPC`

`bool` Typ mit Standardwert von `false`. Diese Eigenschaft definiert, ob der ASF-**[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-de-DE)**-Server zusammen mit dem Prozess gestartet werden soll. IPC ermöglicht die prozessübergreifende Kommunikation durch Starten eines lokalen HTTP-Servers. Wenn du den IPC-Server von ASF nicht nutzen willst, dann gibt es keinen Grund für dich, diese Option zu aktivieren.

* * *

### `IPCPassword`

`string` Typ mit Standardwert von `null`. Diese Eigenschaft definiert das obligatorische Passwort für jeden Anruf über die IPC-Schnittstelle und dient als zusätzliche Sicherheitsmaßnahme. When set to non-empty value, all IPC requests will require extra `password` property set to the password specified here. Der Standardwert von `null` überspringt die Notwendigkeit des Passwortes, so dass ASF alle Anfragen akzeptiert. Darüber hinaus ermöglicht die Aktivierung dieser Option auch den eingebauten IPC-Anti-Bruteforce-Mechanismus, der die gegebene `IPAdress` vorübergehend blockiert, nachdem zu viele nicht autorisierte Anfragen in sehr kurzer Zeit gesendet wurden. Wenn du keinen Grund hast, diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

* * *

### `LoginLimiterDelay`

`byte` Typ mit Standardwert von `10`. ASF stellt sicher, dass zwischen zwei aufeinanderfolgenden Verbindungsversuchen mindestens `LoginLimiterDelay` Sekunden liegen, um eine Auslösung des Anfrage-Limits zu vermeiden. Der Standardwert von `10` wurde basierend auf der Verbindung von über 100 Bot-Instanzen festgelegt und sollte die meisten (wenn nicht alle) Benutzer zufrieden stellen. Du kannst es jedoch erhöhen/verringern, oder sogar zu `0` wechseln, wenn du eine sehr geringe Anzahl von Bots hast, so dass ASF die Verzögerung ignoriert und sich viel schneller mit Steam verbindet. Be warned though, as setting it too low while having too many bots **will** result in Steam temporarily banning your IP, and that will prevent you from logging in **at all**, with `InvalidPassword/RateLimitExceeded` error - and that also includes your normal Steam client, not only ASF. Gleichermaßen, wenn du eine übermäßige Anzahl von Bots verwendest, insbesondere zusammen mit anderen Steam-Clients/Programmen, die die gleiche IP-Adresse verwenden, musst du diesen Wert höchstwahrscheinlich erhöhen, um die Anmeldungen über einen längeren Zeitraum verteilen zu können.

As a side note, this value is also used as load-balancing buffer in all ASF-scheduled actions, such as trades in `SendTradePeriod`. Wenn du keinen **triftigen** Grund hast, diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

* * *

### `MaxFarmingTime`

`byte` Typ mit Standardwert von `10`. Wie du wissen solltest, funktioniert Steam nicht immer richtig, manchmal können seltsame Situationen auftreten, wie z.B. dass Steam unsere Spielzeit nicht aufzeichnet, obwohl tatsächlich ein Spiel gespielt wird. ASF erlaubt es, ein einzelnes Spiel im Einzelmodus für maximal `MaxFarmingTime` Stunden zu betreiben und betrachtet es nach diesem Zeitraum als vollständig gesammelt. Dies ist erforderlich, um den Sammel-Prozess nicht einzufrieren, wenn es zu seltsamen Situationen kommt, aber auch, wenn Steam aus irgendeinem Grund ein neues Abzeichen veröffentlicht hat, das ASF daran hindern würde, weiter voranzukommen (siehe: `Blacklist`). Der Standardwert von `10` Stunden sollte ausreichen, um alle Steam-Karten aus einem Spiel zu sammeln. Wenn du diese Eigenschaft zu niedrig einstellst, kann dies dazu führen, dass gültige Spiele übersprungen werden (und ja, es gibt gültige Spiele, die sogar bis zu 9 Stunden zum Sammeln benötigen), während eine zu hohe Einstellung dazu führen kann, dass der Sammel-Prozess eingefroren wird. Bitte bedenke, dass diese Eigenschaft nur ein einzelnes Spiel in einer einzigen Sammel-Sitzung betrifft (so dass ASF nach dem Durchlaufen der gesamten Warteschlange zu diesem Titel zurückkehrt), auch basiert sie nicht auf der Gesamtspielzeit, sondern auf der internen ASF-Sammel-Zeit. Wenn du keinen **triftigen** Grund hast, diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

* * *

### `MaxTradeHoldDuration`

`byte` Typ mit Standardwert von `15`. This property defines maximum duration of trade hold in days that we're willing to accept - ASF will reject trades that are being held for more than `MaxTradeHoldDuration` days. This option makes sense only for bots with `TradingPreferences` of `SteamTradeMatcher`, as it doesn't affect `Master`/`SteamOwnerID` trades, neither donations. Trade holds are annoying for everyone, and nobody really wants to deal with them. ASF is supposed to work on liberal rules and help everyone, regardless if on trade hold or not - that's why this option is set to `15` by default. However, if you'd instead prefer to reject all trades affected by trade holds, you can specify `0` here. Please consider the fact that cards with short lifespan are not affected by this option and automatically rejected for people with trade holds, as described in **[trading](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading)** section, so there is no need to globally reject everybody only because of that. Wenn du keinen Grund hast, diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

* * *

### `OptimizationMode`

`byte` Typ mit Standardwert von `0`. Diese Eigenschaft definiert den Optimierungsmodus, den ASF während der Laufzeit bevorzugt. Derzeit unterstützt ASF zwei Modi - `0`, der so genannte `MaxPerformance`, und `1`, der so genannte `MinMemoryUsage`. Standardmäßig bevorzugt ASF es, so viele Dinge wie möglich parallel (gleichzeitig) auszuführen, was die Leistung durch Load-Balancing-Arbeiten über alle CPU-Kerne, mehrere CPU-Threads, mehrere Sockel und mehrere Thread-Pool-Aufgaben hinweg erhöht. Zum Beispiel fragt ASF nach deiner ersten Abzeichen-Seite, wenn du nach Spielen zum Sammeln suchst, und sobald die Anfrage eingetroffen ist, liest ASF daraus, wie viele Abzeichen-Seiten du tatsächlich hast, und fordert dann alle gleichzeitig an. Das ist es, was du dir eigentlich **fast immer** wünschst, da der Aufwand in den meisten Fällen minimal ist und die Vorteile des asynchronen ASF-Codes auch auf der ältesten Hardware mit einem einzigen CPU-Kern und stark eingeschränkter Leistung sichtbar sind. Da jedoch viele Aufgaben parallel verarbeitet werden, ist die ASF-Laufzeit für ihre Wartung verantwortlich, z.B. Sockets offen zu halten, Threads am Leben zu erhalten und Aufgaben zu bearbeiten, was von Zeit zu Zeit zu einer erhöhten Speicherauslastung führen kann, und wenn du durch den verfügbaren Speicher extrem eingeschränkt bist, solltest du diese Eigenschaft auf `1` (`MinMemoryUsage`) umschalten, um ASF zu zwingen, so wenig Aufgaben wie möglich zu verwenden und typischerweise possible-to-parallel asynchronen Code synchron zu verwenden. You should consider switching this property only if you previously read **[low-memory setup](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup)** and you intentionally want to sacrifice gigantic performance boost, for a very small memory overhead decrease. Usually this option is **much worse** than what you can achieve with other possible ways, such as by limiting your ASF usage or tuning runtime's garbage collector, as explained in **[low-memory setup](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup)**. Therefore, you should use `MinMemoryUsage` as a **last resort**, right before runtime recompilation, if you couldn't achieve satisfying results with other (much better) options. Wenn du keinen **triftigen** Grund hast, diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

* * *

### `Statistics`

`bool` Typ mit Standardwert von `true`. Diese Eigenschaft legt fest, ob ASF die Statistik aktiviert haben soll. Eine detaillierte Erklärung, was genau diese Option bewirkt, findest du im Abschnitt **[Statistiken](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Statistics-de-DE)**. Wenn du keinen Grund hast, diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

* * *

### `SteamMessagePrefix`

`string` Typ mit Standardwert von `"/me "`. This property defines a prefix that will be prepended to all Steam messages being sent by ASF. By default ASF uses `"/me "` prefix in order to distinguish bot messages more easily by showing them in different color on Steam chat. Another worthy mention is `"/pre "` prefix which achieves similar result, but uses different formatting. You can also set this property to empty string or `null` in order to disable using prefix entirely and output all ASF messages in a traditional way. It's nice to note that this property affects Steam messages only - responses returned through other channels (such as IPC) are not affected. Wenn du das standardmäßige ASF-Verhalten nicht anpassen möchtest, ist es eine gute Idee, es bei den Standardeinstellungen zu belassen.

* * *

### `SteamOwnerID`

`ulong` Typ mit Standardwert von `0`. This property defines Steam ID in 64-bit form of ASF process owner, and is very similar to `Master` permission of given bot instance (but global instead). You almost always want to set this property to ID of your own main Steam account. `Master` permission includes full control over his bot instance, but global commands such as `exit`, `restart` or `update` are reserved for `SteamOwnerID` only. This is convenient, as you might want to run bots for your friends, while not allowing them to control ASF process, such as exiting it via `exit` command. Default value of `0` specifies that there is no owner of ASF process, which means that nobody will be able to issue global ASF commands. Keep in mind that **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** works with `SteamOwnerID`, so if you want to use it you must provide a valid value here.

* * *

### `SteamProtocols`

`byte flags` Typ mit Standardwert von `7`. Diese Eigenschaft definiert Steam-Protokolle, die ASF beim Verbinden mit Steam-Servern verwendet, die wie folgt definiert sind:

| Wert | Name      | Beschreibung                                                                                     |
| ---- | --------- | ------------------------------------------------------------------------------------------------ |
| 0    | None      | Kein Protokoll                                                                                   |
| 1    | TCP       | **[Transmission Control Protocol](https://en.wikipedia.org/wiki/Transmission_Control_Protocol)** |
| 2    | UDP       | **[User Datagram Protocol](https://en.wikipedia.org/wiki/User_Datagram_Protocol)**               |
| 4    | WebSocket | **[WebSocket](https://en.wikipedia.org/wiki/WebSocket)**                                         |

Bitte bedenke, dass diese Eigenschaft das Feld `flags` ist, daher ist es möglich, eine beliebige Kombination von verfügbaren Werten auszuwählen. Schau dir **[JSON-Mapping](#json-mapping)** an, wenn du mehr darüber erfahren möchtest. Not enabling any of flags results in `None` option, and that option is invalid by itself.

By default ASF should use all available Steam protocols as a measure for fighting with downtimes and other similar Steam issues. Typically you want to change this property if you want to limit ASF into using only one or two specific protocols instead of all available ones. Such measure could be needed if you're e.g. enabling only TCP traffic on your firewall and you do not want ASF to try connecting via UDP. However, unless you're debugging particular problem or issue, you almost always want to ensure that ASF is free to use any protocol that is currently supported and not just one or two. Wenn du keinen **triftigen** Grund hast, diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

* * *

### `UpdateChannel`

`byte` Typ mit Standardwert von `1`. This property defines update channel which is being used, either for auto-updates (if `UpdatePeriod` is greater than `0`), or update notifications (otherwise). Currently ASF supports three update channels - `0` which is called `None`, `1`, which is called `Stable`, and `2`, which is called `Experimental`. `Stable` channel is the default release channel, which should be used by majority of users. `Experimental` channel in addition to `Stable` releases, also includes **pre-releases** dedicated for advanced users and other developers in order to test new features, confirm bugfixes or give feedback about planned enhancements. **Experimental versions often contain unpatched bugs, work-in-progress features or rewritten implementations**. If you don't consider yourself advanced user, please stay with default `1` (Stable) update channel. `Experimental` channel is dedicated to users who know how to report bugs, deal with issues and give feedback - no technical support will be given. Check out ASF **[release cycle](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle)** if you'd like to learn more. You can also set `UpdateChannel` to `0` (`None`), if you want to completely remove all version checks. Setting `UpdateChannel` to `0` will entirely disable entire functionality related to updates, including `update` command. Using `None` channel is **strongly discouraged** due to exposing yourself to all sort of problems (mentioned in `UpdatePeriod` description below).

**Unless you know what you're doing**, we **strongly** recommend to keep it at default.

* * *

### `UpdatePeriod`

`byte` Typ mit Standardwert von `24`. This property defines how often ASF should check for auto-updates. Updates are crucial not only to receive new features and critical security patches, but also to receive bugfixes, performance enhancements, stability improvements and more. When a value greater than `0` is set, ASF will automatically download, replace, and restart itself (if `AutoRestart` permits) when new update is available. In order to achieve this, ASF will check every `UpdatePeriod` hours if new update is available on our GitHub repo. A value of `0` disables auto-updates, but still allows you to execute `update` command manually. You might also be interested in setting appropriate `UpdateChannel` that `UpdatePeriod` should follow.

Update process of ASF involves update of entire folder structure that ASF is using, but without touching your own configs or databases located in `config` directory - this means that any extra files unrelated to ASF in its directory might be lost during update. Default value of `24` is a good balance between unnecessary checks, and ASF that is fresh enough.

Unless you have a **strong** reason to disable this feature, you should keep auto-updates enabled within reasonable `UpdatePeriod` **for your own good**. This is not only because we don't support anything but latest stable ASF release, but also because **we give our security guarantee only for latest version**. If you're using outdated ASF version then you're intentionally exposing yourself to all kind of issues, from small bugs, through broken functionality, ending with **[permanent Steam account suspensions](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ#did-somebody-get-banned-for-it)**, so we **strongly recommend**, for your own good, to always ensure that your ASF version is up to date. Auto-updates allow us to react quickly to all kind of issues by disabling or patching problematic code before it can escalate - if you opt out of it, you lose all of our security guarantees and risk consequences from running code that could be potentially harmful, not only to Steam network, but also (by definition) to your own Steam account.

* * *

### `WebLimiterDelay`

`ushort` Typ mit Standardwert von `200`. Diese Eigenschaft definiert in Millisekunden die minimale Verzögerung zwischen dem Senden von zwei aufeinanderfolgenden Anfragen an denselben Steam-Webservice. Eine solche Verzögerung ist erforderlich, da der Dienst **[AkamaiGhost](https://www.akamai.com)**, den Steam intern nutzt, eine Geschwindigkeitsbegrenzung basierend auf der globalen Anzahl der über einen bestimmten Zeitraum gesendeten Anfragen beinhaltet. Unter normalen Umständen ist akamai Block ziemlich schwer zu erreichen, aber unter sehr hohen Arbeitslasten mit einer riesigen laufenden Warteschlange von Anfragen ist es möglich, ihn auszulösen, wenn wir immer wieder zu viele Anfragen über einen zu kurzen Zeitraum senden.

Der Standardwert wurde unter der Annahme festgelegt, dass ASF das einzige Programm ist, das auf die Steam-Webdienste zugreift, insbesondere `steamcommunity.com`, `api.steampowered.com` und `store.steampowered.com`. Wenn du andere Programme verwendest, die Anfragen an dieselben Webdienste senden, dann solltest du sicherstellen, dass dein Programm ähnliche Funktionen wie `WebLimiterDelay` enthält und beide auf das Doppelte des Standardwerts setzt, was `400` wäre. Dies garantiert, dass du unter keinen Umständen mehr als 1 Anfrage pro 200 ms senden wirst.

Im Allgemeinen wird das Herabsetzen von `WebLimiterDelay` unter den Standardwert **stark abgeraten**, da es zu verschiedenen IP-bezogenen Sperren führen kann, von denen einige dauerhaft sein können. Der Standardwert ist gut genug, um eine einzelne ASF-Instanz auf dem Server auszuführen und ASF im Normalfall zusammen mit dem ursprünglichen Steam-Client zu verwenden. Es sollte für die meisten Anwendungen zutreffend sein, und du solltest es nur erhöhen (nie senken), wenn du - abgesehen von der Verwendung von ASF - auch ein anderes Programm verwendest, das eine übermäßige Anzahl von Anfragen an dieselben Webdienste senden könnte, die ASF nutzt. Kurz gesagt, die globale Anzahl aller Anfragen, die von einer einzelnen IP an eine einzelne Steam-Domäne gesendet werden, sollte nie mehr als 1 Anfrage pro 200 ms überschreiten.

Wenn du keinen Grund hast, diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

* * *

### `WebProxy`

`string` Typ mit Standardwert von `null`. Diese Eigenschaft definiert eine Web-Proxy-Adresse, die für alle internen http- und https-Anfragen verwendet wird, die von ASFs `HttpClient` gesendet werden, insbesondere für Dienste wie `github.com`, `steamcommunity.com` und `store.steampowered.com`. Das Proxying von ASF-Anfragen im Allgemeinen hat keine Vorteile, aber es ist äußerst nützlich, um verschiedene Arten von Firewalls zu umgehen, insbesondere die große Firewall von China.

Diese Eigenschaft ist als uri Zeichenfolge definiert:

> A URI string is composed of a scheme (http or https), a host, and an optional port. An example of a complete uri string is `"http://contoso.com:8080"`.

Wenn dein Proxy eine Benutzer-Authentifizierung erfordert, musst du auch `WebProxyUsername` und/oder `WebProxyPassword` einrichten. Wenn es keinen solchen Bedarf gibt, genügt die Errichtung dieser Eigenschaft allein.

Right now ASF uses web proxy only for `http` and `https` requests, which **do not** include internal Steam network communication done within ASF's internal Steam client. There are currently no plans for supporting that, mainly due to missing **[SK2](https://github.com/SteamRE/SteamKit)** functionality. If you need/want it to happen, I'd suggest starting from there.

Wenn du keinen Grund hast, diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

* * *

### `WebProxyPassword`

`string` Typ mit Standardwert von `null`. Diese Eigenschaft definiert das Feld für das Passwort, das bei der Basis-, Digest-, NTLM- und Kerberos-Authentifizierung verwendet wird und von einem Ziel `WebProxy`-Rechner mit Proxy-Funktionalität unterstützt wird. Wenn dein Proxy keine Benutzer-Anmeldeinformationen benötigt, ist es nicht notwendig, dass du hier etwas einträgst. Die Verwendung dieser Option ist nur sinnvoll, wenn auch `WebProxy` verwendet wird, da sie sonst keine Wirkung hat.

Wenn du keinen Grund hast, diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

* * *

### `WebProxyUsername`

`string` Typ mit Standardwert von `null`. Diese Eigenschaft definiert das Feld für den Benutzernamen, das bei der Basis-, Digest-, NTLM- und Kerberos-Authentifizierung verwendet wird und von einem Ziel `WebProxy`-Rechner mit Proxy-Funktionalität unterstützt wird. Wenn dein Proxy keine Benutzer-Anmeldeinformationen benötigt, ist es nicht notwendig, dass du hier etwas einträgst. Die Verwendung dieser Option ist nur sinnvoll, wenn auch `WebProxy` verwendet wird, da sie sonst keine Wirkung hat.

Wenn du keinen Grund hast, diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

* * *

**[Zum Seitenanfang](#konfiguration)**

* * *

## Bot Konfiguration

Wie du bereits weißt, sollte jeder Bot eine eigene Konfiguration haben, die auf der folgenden beispielhaften JSON-Struktur basiert. Beginne damit, zu entscheiden, wie du deinen Bot benennen möchtest (z.B. `1.json`, `main.json`, `haupt.json` oder `IrgendwasAnderes.json`) und gehe zur Konfiguration über.

**Hinweis:** Der Bot kann den Namen `ASF` nicht haben (da dieses Schlüsselwort für die globale Konfiguration reserviert ist), ASF ignoriert auch alle Konfigurationsdateien, die mit einem Punkt beginnen.

Die Bot-Konfiguration hat folgende Struktur:

```json
{
    "AcceptGifts": false,
    "AutoSteamSaleEvent": false,
    "BotBehaviour": 0,
    "CustomGamePlayedWhileFarming": null,
    "CustomGamePlayedWhileIdle": null,
    "Enabled": false,
    "FarmingOrders": [],
    "GamesPlayedWhileIdle": [],
    "HoursUntilCardDrops": 3,
    "IdlePriorityQueueOnly": false,
    "IdleRefundableGames": true,
    "LootableTypes": [1, 3, 5],
    "MatchableTypes": [5],
    "OnlineStatus": 1,
    "PasswordFormat": 0,
    "Paused": false,
    "RedeemingPreferences": 0,
    "SendOnFarmingFinished": false,
    "SendTradePeriod": 0,
    "ShutdownOnFarmingFinished": false,
    "SteamLogin": null,
    "SteamMasterClanID": 0,
    "SteamParentalCode": null,
    "SteamPassword": null,
    "SteamTradeToken": null,
    "SteamUserPermissions": {},
    "TradingPreferences": 0,
    "TransferableTypes": [1, 3, 5],
    "UseLoginKeys": true
}
```

**Tipp:** Damit der Bot richtig funktioniert, solltest du mindestens die Eigenschaften `Enabled`, `SteamLogin` und `SteamPassword` bearbeiten. Ich schlage auch vor, einen Blick auf einige Feinabstimmungen wie `HoursUntilCardDrops` zu werfen, aber all das ist optional. ASF-Konfigurationen sind ziemlich fortschrittlich, um es dir zu ermöglichen, deine Bots und ASF so einzustellen, wie du willst. Wenn du ein solches fortgeschrittenes Setup nicht "benötigst", musst du nicht wirklich detailliert in jede Konfigurationseigenschaft schauen. Es liegt an dir, wie einfach oder wie komplex ASF sein sollte.

* * *

Alle Optionen werden nachfolgend erklärt:

### `AcceptGifts`

`bool` Typ mit Standardwert von `false`. Wenn aktiviert, akzeptiert und löst ASF automatisch alle Steam-Geschenke (einschließlich Guthaben-Geschenkgutscheine), die an den Bot geschickt werden. Dazu gehören auch Geschenke, die von anderen Benutzern als denjenigen gesendet werden, die in `SteamUserPermissions` definiert sind. Bedenke, dass Geschenke, die an die E-Mail-Adresse geschickt werden, nicht direkt an den Client weitergeleitet werden, so dass ASF diese ohne deine Hilfe nicht annehmen wird.

Diese Option wird nur für Alternativkonten empfohlen, da es sehr wahrscheinlich ist, dass du nicht automatisch alle Geschenke einlösen möchtest, die an dein Hauptkonto gesendet werden. Wenn du dir nicht sicher bist, ob du diese Funktion aktivieren möchtest oder nicht, behalte sie mit dem Standardwert `false`.

* * *

### `AutoSteamSaleEvent`

`bool` Typ mit Standardwert von `false`. Während der Steam Summer/Winter-Sale Events ist Steam dafür bekannt, dir zusätzliche Karten zur Verfügung zu stellen, die du für das Durchlaufen der Entdeckungsliste erhalten kannst oder für das Teilnehmen an den Steam Awards. Wenn diese Option aktiviert ist, überprüft ASF automatisch alle 6 Stunden die Steam Entdeckungsliste und die Steam Awards und durchläuft sie bei Bedarf. Diese Option wird nicht empfohlen, wenn du diese Aktionen selbst durchführen möchtest, und normalerweise sollte sie nur bei Bot-Konten sinnvoll sein. Außerdem musst du sicherstellen, dass dein Konto mindestens Steam Level `8` ist, damit du diese Karten überhaupt erhalten kannst. Wenn du dir nicht sicher bist, ob du diese Funktion aktivieren möchtest oder nicht, behalte sie mit dem Standardwert `false`.

Bitte bedenke, dass wir aufgrund von ständigen Steam-Problemen, Änderungen und Problemen **keine Garantie geben, ob diese Funktion ordnungsgemäß funktioniert**, daher ist es durchaus möglich, dass diese Option **überhaupt nicht funktioniert**. Wir akzeptieren **keine** Fehlermeldungen, auch keine Unterstützungsanfragen für diese Option. Es wird ohne jegliche Garantie angeboten, die Nutzung erfolgt auf eigene Gefahr.

* * *

### `BotBehaviour`

`byte flags` Typ mit Standardwert von `0`. Diese Eigenschaft definiert das ASF-Bot-ähnliche Verhalten bei verschiedenen Ereignissen und ist wie folgt definiert:

| Wert | Name                          | Beschreibung                                                                                |
| ---- | ----------------------------- | ------------------------------------------------------------------------------------------- |
| 0    | None                          | Kein spezielles Bot-Verhalten, der am wenigsten invasive Modus, Standard                    |
| 1    | RejectInvalidFriendInvites    | Führt dazu, dass ASF ungültige Freundschaftseinladungen ablehnt (anstatt sie zu ignorieren) |
| 2    | RejectInvalidTrades           | Wird ASF dazu veranlassen, ungültige Handelsangebote abzulehnen (anstatt sie zu ignorieren) |
| 4    | RejectInvalidGroupInvites     | Führt dazu, dass ASF ungültige Gruppeneinladungen ablehnt (anstatt sie zu ignorieren)       |
| 8    | DismissInventoryNotifications | Veranlasst ASF dazu, alle Inventar-Benachrichtigungen automatisch zu entfernen              |
| 16   | MarkReceivedMessagesAsRead    | Führt dazu, dass ASF automatisch alle empfangenen Nachrichten als gelesen markiert          |

Bitte bedenke, dass diese Eigenschaft das Feld `flags` ist, daher ist es möglich, eine beliebige Kombination von verfügbaren Werten auszuwählen. Schau dir **[JSON-Mapping](#json-mapping)** an, wenn du mehr darüber erfahren möchtest. Wenn keines der Flags aktiviert wird, wird die Option `None` verwendet.

In general you want to modify this property if you expect from ASF to do certain amount of automation related to its activity, as it'd be expected from a bot account, but not a primary account used in ASF. Therefore, changing this property makes sense mainly for alt accounts, although you're free to use selected options for main accounts as well.

Normal (`None`) ASF behaviour is to only automate things that user wants (e.g. cards farming or `SteamTradeMatcher` offers, if set in `TradingPreferences`). This is the least invasive mode, and it's beneficial to majority of users since you remain in full control over your account and you can decide yourself whether to allow certain out-of-scope interactions, or not.

Invalid friend invite is an invite that doesn't come from user with `FamilySharing` permission (defined in `SteamUserPermissions`) or above. ASF in normal mode ignores those invites, as you'd expect, giving you free choice whether to accept them, or not. `RejectInvalidFriendInvites` will cause those invites to be automatically rejected, which will practically disable option for other people to add you to their friend list (as ASF will deny all such requests, apart from people defined in `SteamUserPermissions`). Unless you want to outright deny all friend invites, you shouldn't enable this option.

Invalid trade offer is an offer that isn't accepted through built-in ASF module. More on this matter can be found in **[trading](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading)** section which explicitly defines what types of trade ASF is willing to accept automatically. Valid trades are also defined by other settings, especially `TradingPreferences`. `RejectInvalidTrades` will cause all invalid trade offers to be rejected, instead of being ignored. Unless you want to outright deny all trade offers that aren't automatically accepted by ASF, you shouldn't enable this option.

Invalid group invite is an invite that doesn't come from `SteamMasterClanID` group. ASF in normal mode ignores those group invites, as you'd expect, allowing you to decide yourself if you want to join particular Steam group or not. `RejectInvalidGroupInvites` will cause all those group invites to be automatically rejected, effectively making it impossible to invite you to any other group than `SteamMasterClanID`. Unless you want to outright deny all group invites, you shouldn't enable this option.

Wenn du dir nicht sicher bist, wie du diese Option konfigurieren sollst, ist es am besten, sie auf dem Standardwert zu belassen.

* * *

### `CustomGamePlayedWhileFarming`

`string` Typ mit Standardwert von `null`. When ASF is farming, it can display itself as "Playing non-steam game: `CustomGamePlayedWhileFarming`" instead of currently farmed game. This can be useful if you would like to let your friends know that you're farming, yet you don't want to change default `OnlineStatus`. Please note that ASF cannot guarantee the actual display order of Steam network, therefore this is only a suggestion that may, or may not, display properly. Default value of `null` disables this feature.

* * *

### `CustomGamePlayedWhileIdle`

`string` Typ mit Standardwert von `null`. Similar to `CustomGamePlayedWhileFarming`, but for the situation when ASF has nothing to do (as account is fully farmed). Default value of `null` disables this feature.

* * *

### `Enabled`

`bool` Typ mit Standardwert von `false`. This property defines if bot is enabled. Enabled bot instance (`true`) will automatically start on ASF run, while disabled bot instance (`false`) will need to be started manually. By default every bot is disabled, so you probably want to switch this property to `true` for all of your bots that should be started automatically.

* * *

### `FarmingOrders`

`ImmutableHashSet<byte>` Typ mit einem leeren Standardwert. This property defines the **preferred** farming order used by ASF for given bot account. Currently there are following farming orders available:

| Wert | Name                      | Beschreibung                                                                     |
| ---- | ------------------------- | -------------------------------------------------------------------------------- |
| 0    | Unordered                 | Keine Sortierung, leichte Verbesserung der CPU-Leistung                          |
| 1    | AppIDsAscending           | Versuche Spiele mit den niedrigsten `appID`s zuerst zu sammeln                   |
| 2    | AppIDsDescending          | Versuche Spiele mit den höchsten `appID`s zuerst zu sammeln                      |
| 3    | CardDropsAscending        | Try to farm games with lowest number of card drops remaining first               |
| 4    | CardDropsDescending       | Try to farm games with highest number of card drops remaining first              |
| 5    | HoursAscending            | Try to farm games with lowest number of hours played first                       |
| 6    | HoursDescending           | Try to farm games with highest number of hours played first                      |
| 7    | NamesAscending            | Try to farm games in alphabetical order, starting from A                         |
| 8    | NamesDescending           | Try to farm games in reverse alphabetical order, starting from Z                 |
| 9    | Random                    | Try to farm games in totally random order (different on each run of the program) |
| 10   | BadgeLevelsAscending      | Try to farm games with lowest badge levels first                                 |
| 11   | BadgeLevelsDescending     | Try to farm games with highest badge levels first                                |
| 12   | RedeemDateTimesAscending  | Try to farm oldest games on our account first                                    |
| 13   | RedeemDateTimesDescending | Try to farm newest games on our account first                                    |
| 14   | MarketableAscending       | Try to farm games with unmarketable card drops first                             |
| 15   | MarketableDescending      | Try to farm games with marketable card drops first                               |

Since this property is an array, it allows you to use several different settings in your fixed order. For example, you can include values of `15`, `11` and `7` in order to sort by marketable games first, then by those with highest badge level, and finally alphabetically. As you can guess, the order actually matters, as reverse one (`7`, `11` and `15`) achieves something entirely different. Majority of people will probably use just one order out of all of them, but in case you want to, you can also sort further by extra parameters.

Also notice the word "try" in all above descriptions - the actual ASF order is heavily affected by selected **[cards farming algorithm](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)** and sorting will affect only results that ASF considers same performance-wise. For example, in `Simple` algorithm, selected `FarmingOrders` should be entirely respected in current farming session (as every game has the same performance value), while in `Complex` algorithm actual order is affected by hours first, and then sorted according to chosen `FarmingOrders`. This will lead to different results, as games with existing playtime will have a priority over others, so effectively ASF will prefer games with highest playtime first, and only then sorting everything further by your chosen `FarmingOrders`. Therefore, this config property is only a **suggestion** that ASF will try to respect, as long as it doesn't affect performance negatively (in this case, ASF will always prefer performance over `FarmingOrders`).

There is also idling priority queue that is accessible through `iq` **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. If it's used, actual idling order is sorted firstly by performance, then by idling queue, and finally by your `FarmingOrders`.

* * *

### `GamesPlayedWhileIdle`

`ImmutableHashSet<uint>` Typ mit einem leeren Standardwert. If ASF has nothing to farm it can play your specified steam games (`appID`s) instead. Playing games in such manner increases your "hours played" of those games, but nothing else apart of it. This feature can be enabled at the same time with `CustomGamePlayedWhileIdle` in order to play your selected games while showing custom status in Steam network, but in this case, like in `CustomGamePlayedWhileFarming` case, the actual display order is not guaranteed. Please note that Steam allows ASF to play only up to `32` `appID`s, therefore if you put more games than that, only first `32` will be respected (and extra ones being ignored).

* * *

### `HoursUntilCardDrops`

`byte` Typ mit Standardwert von `3`. This property defines if account has card drops restricted, and if yes, for how many initial hours. Restricted card drops means that account is not receiving any card drops from given game until the game is played for at least `HoursUntilCardDrops` hours. Unfortunately there is no magical way to detect that, so ASF relies on you. This property affects **[cards farming algorithm](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)** that will be used. Setting this property properly will maximize profits and minimize time required for cards to be farmed. Remember that there is no obvious answer whether you should use one or another value, since it fully depends on your account. It seems that older accounts which never asked for refund have unrestricted card drops, so they should use a value of `0`, while new accounts and those who did ask for refund have restricted card drops with a value of `3`. Dies ist jedoch nur eine Theorie und sollte nicht als Tatsache betrachtet werden. The default value for this property was set based on "lesser evil" and majority of use cases.

* * *

### `IdlePriorityQueueOnly`

`bool` Typ mit Standardwert von `false`. This property defines if ASF should consider for automatic idling only apps that you added yourself to priority idling queue available with `iq` **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. When this option is enabled, ASF will skip all `appIDs` that are missing on the list, effectively allowing you to cherry-pick games for automatic ASF idling. Keep in mind that if you didn't add any games to the queue then ASF will act as if there is nothing to idle on your account. Wenn du dir nicht sicher bist, ob du diese Funktion aktivieren möchtest oder nicht, behalte sie mit dem Standardwert `false`.

* * *

### `IdleRefundableGames`

`bool` Typ mit Standardwert von `true`. This property defines if ASF is permitted to idle games that are still refundable. A refundable game is a game that we bought in last 2 weeks through Steam Store and we didn't play it for longer than 2 hours yet, as stated **[here](https://store.steampowered.com/steam_refunds)**. By default when this option is set to `true`, ASF ignores Steam refunds entirely and idles everything, as most people expect. However, you can change this option to `false` if you want to ensure that ASF won't idle any of your refundable games too soon, allowing you to evaluate those games yourself and refund if needed without worrying about ASF affecting playtime negatively. Please note that if you disable this option then games you purchased from Steam Store won't be idled by ASF for up to 14 days since redeem date. If you're unsure whether you want this feature enabled or not, keep it with default value of `true`.

* * *

### `LootableTypes`

`ImmutableHashSet<byte>` Typ mit Standardwert von `1, 3, 5` Steam-Gegenstands-Typen. This property defines ASF behaviour when looting - both manual and automatic. ASF will ensure that only items from `LootableTypes` will be included in a trade offer, therefore this property allows you to choose what you want to receive in a trade offer that is being sent to you.

| Wert | Name              | Beschreibung                                                                                  |
| ---- | ----------------- | --------------------------------------------------------------------------------------------- |
| 0    | Unknown           | Jeder Typ, der nicht in eine der folgenden Kategorien passt                                   |
| 1    | BoosterPack       | Booster Pack mit 3 zufälligen Karten aus einem Spiel                                          |
| 2    | Emoticon          | Emoticon zur Verwendung im Steam-Chat                                                         |
| 3    | FoilTradingCard   | Folienvariante von `TradingCard`                                                              |
| 4    | ProfileBackground | Profilhintergrund zur Verwendung in deinem Steam-Profil                                       |
| 5    | TradingCard       | Steam-Karte, die für die Herstellung von Abzeichen (Nicht-Folie) verwendet werden             |
| 6    | SteamGems         | Steam-Edelsteine, die für die Herstellung von Booster Packs verwendet werden, inklusive Säcke |

Bitte bedenke, dass ASF unabhängig von den obigen Einstellungen nur nach Steam (`appID` von 753) Community (`contextID` von 6) Gegenständen fragt, so dass alle Spiel-Gegenstände und Geschenke und dergleichen per Definition aus dem Handelsangebot ausgeschlossen sind.

Default ASF setting is based on most common usage of the bot, with looting only booster packs, and trading cards (including foils). Die hier definierte Eigenschaft ermöglicht es dir, dieses Verhalten so zu verändern, dass es dich zufrieden stellt. Bitte bedenke, dass alle nicht oben definierten Typen als `Unknown` Typ angezeigt werden, was besonders wichtig ist, wenn Valve einen neuen Steam-Gegenstand veröffentlicht, der ebenfalls von ASF als `Unknown` markiert wird, bis er hier (in der zukünftigen Version) hinzugefügt wird. That's why in general it's not recommended to include `Unknown` type in your `LootableTypes`, unless you know what you're doing, and you also understand that ASF will send your entire inventory in a trade offer if Steam Network gets broken again and reports all your items as `Unknown`. My strong suggestion is to not include `Unknown` type in the `LootableTypes`, even if you expect to loot everything.

* * *

### `MatchableTypes`

`ImmutableHashSet<byte>` type with default value of `5` Steam item types. This property defines which Steam item types are permitted to be matched when `SteamTradeMatcher` option in `TradingPreferences` is enabled. Types are defined as below:

| Wert | Name              | Beschreibung                                                                                  |
| ---- | ----------------- | --------------------------------------------------------------------------------------------- |
| 0    | Unknown           | Jeder Typ, der nicht in eine der folgenden Kategorien passt                                   |
| 1    | BoosterPack       | Booster Pack mit 3 zufälligen Karten aus einem Spiel                                          |
| 2    | Emoticon          | Emoticon zur Verwendung im Steam-Chat                                                         |
| 3    | FoilTradingCard   | Folienvariante von `TradingCard`                                                              |
| 4    | ProfileBackground | Profilhintergrund zur Verwendung in deinem Steam-Profil                                       |
| 5    | TradingCard       | Steam-Karte, die für die Herstellung von Abzeichen (Nicht-Folie) verwendet werden             |
| 6    | SteamGems         | Steam-Edelsteine, die für die Herstellung von Booster Packs verwendet werden, inklusive Säcke |

Of course, types that you should use for this property typically include only `2`, `3`, `4` and `5`, as only those types are supported by STM. Please note that **ASF is not a trading bot** and **will NOT care about price or rarity**, which means that if you use it e.g. with `Emoticon` type, then ASF will be happy to trade your 2x rare emoticon for 1x rare 1x common, as that makes progress towards badge (in this case emoticons) completion. Please evaluate twice if you're fine with that. Unless you know what you're doing, you should keep it with default value of `5`.

* * *

### `OnlineStatus`

`byte` Typ mit Standardwert von `1`. This property specifies Steam community status that the bot will be announced with after logging in to Steam network. Currently you can choose one of below statuses:

| Wert | Name           |
| ---- | -------------- |
| 0    | Offline        |
| 1    | Online         |
| 2    | Busy           |
| 3    | Away           |
| 4    | Snooze         |
| 5    | LookingToTrade |
| 6    | LookingToPlay  |
| 7    | Invisible      |

`Offline` status is extremely useful for primary accounts. As you should know, farming a game actually shows your steam status as "Playing game: XXX", which can be misleading to your friends, confusing them that you're playing a game while actually you're only farming it. Using `Offline` status solves that issue - your account will never be shown as "in-game" when you're farming steam cards with ASF. This is possible thanks to the fact that ASF does not have to sign in into Steam Community in order to work properly, so we're in fact playing those games, connected to Steam network, but without announcing our online presence at all. Keep in mind that played games using offline status will still count towards your playtime, and show as "recently played" on your profile.

In addition to that, this feature is also important if you want to receive notifications and unread messages when ASF is running, while not keeping Steam client open at the same time. This is because ASF acts as a Steam client itself, and whether ASF would like it or not, Steam broadcasts all those messages and other events to it. This is not a problem if you have both ASF and your own Steam client running, as both clients receive exactly the same events. However, if just ASF is running, Steam network could mark certain events and messages as "delivered", despite of your traditional Steam client not receiving it due to not being present. Offline status also solves this problem, as ASF is never considered for any community events in this case, so all unread messages and other events will be properly marked as unread when you come back.

It's important to note that ASF running on `Offline` mode will **not** be able to receive commands in usual Steam chat way, as the chat, as well as entire community presence is in fact, entirely offline. A solution to this issue is using `Invisible` mode instead which works in a similar way (not exposing status), but keeps the ability to receive and respond to messages (so also a potential to dismiss notifications and unread messages as stated above). `Invisible` mode makes the most sense on alt accounts that you don't want to expose (status-wise), but still be able to send commands to.

However, there is one catch with `Invisible` mode - it doesn't go well with primary accounts. This is because **any** Steam session that is currently online **exposes** the status, even if ASF itself does not. This is caused by the current limitation/bug of the Steam network that isn't possible to be fixed on ASF side, so if you want to use `Invisible` mode you will also need to ensure that **all** other sessions to the same account use `Invisible` mode as well. This will be the case on alt accounts where ASF is hopefully the only active session, but on primary accounts you'll almost always prefer to show as `Online` to your friends, hiding only ASF activity, and in this case `Invisible` mode will be entirely useless for you (we recommend to use `Offline` mode instead). Hopefully this limitation/bug will be eventually solved in the future by Valve, but I wouldn't expect that to happen anytime soon...

If you're unsure how to set up this property, it's recommended to use a value of `0` (`Offline`) for primary accounts, and default `1` (`Online`) otherwise.

* * *

### `PasswordFormat`

`byte` Typ mit Standardwert von `0`. This property defines the format of `SteamPassword` property, and currently supports - `0` for `PlainText`, `1` for `AES` and `2` for `ProtectedDataForCurrentUser`. Please refer to **[Security](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security)** section if you want to learn more, as you'll need to ensure that `SteamPassword` property indeed includes password in matching `PasswordFormat`. In other words, when you change `PasswordFormat` then your `SteamPassword` should be **already** in that format, not just aiming to be. Unless you know what you're doing, you should keep it with default value of `0`.

* * *

### `Paused`

`bool` Typ mit Standardwert von `false`. This property defines initial state of `CardsFarmer` module. With default value of `false`, bot will automatically start farming when it's started, either because of `Enabled` or `start` command. Switching this property to `true` should be done only if you want to manually `resume` automatic farming process, for example because you want to use `play` all the time and never use automatic `CardsFarmer` module - this works exactly the same as `pause` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. Wenn du dir nicht sicher bist, ob du diese Funktion aktivieren möchtest oder nicht, behalte sie mit dem Standardwert `false`.

* * *

### `RedeemingPreferences`

`byte flags` Typ mit Standardwert von `0`. This property defines ASF behaviour when redeeming cd-keys, and is defined as below:

| Wert | Name             | Beschreibung                                                                   |
| ---- | ---------------- | ------------------------------------------------------------------------------ |
| 0    | None             | No redeeming preferences, typical                                              |
| 1    | Forwarding       | Forward keys unavailable to redeem to other bots                               |
| 2    | Distributing     | Distribute all keys among itself and other bots                                |
| 4    | KeepMissingGames | Keep keys for (potentially) missing games when forwarding, leaving them unused |

Bitte bedenke, dass diese Eigenschaft das Feld `flags` ist, daher ist es möglich, eine beliebige Kombination von verfügbaren Werten auszuwählen. Schau dir **[JSON-Mapping](#json-mapping)** an, wenn du mehr darüber erfahren möchtest. Wenn keines der Flags aktiviert wird, wird die Option `None` verwendet.

`Forwarding` will cause bot to forward a key that is not possible to redeem, to another connected and logged on bot that is missing that particular game (if possible to check). The most common situation is forwarding `AlreadyPurchased` game to another bot that is missing that particular game, but this option also covers other scenarios, such as `DoesNotOwnRequiredApp`, `RateLimited` or `RestrictedCountry`.

`Distributing` will cause bot to distribute all received keys among itself and other bots. This means that every bot will get a single key from the batch. Typically this is used only when you're redeeming many keys for the same game, and you want to evenly distribute them among your bots, as opposed to redeeming keys for various different games. This feature makes no sense if you're redeeming only one key in a single `redeem` action (as there are no extra keys to be distributed).

`KeepMissingGames` will cause bot to skip `Forwarding` when we can't be sure if key being redeemed is in fact owned by our bot, or not. This basically means that `Forwarding` will apply **only** to `AlreadyPurchased` keys, instead of covering also other cases such as `DoesNotOwnRequiredApp`, `RateLimited` or `RestrictedCountry`. Typically you might want to use this option on primary account, to ensure that keys being redeemed on it won't be forwarded further if your bot for example becomes temporarily `RateLimited`. As you can guess from the description, this field has absolutely no effect if `Forwarding` is not enabled.

Enabling both `Forwarding` and `Distributing` will add distributing feature on top of forwarding one, which makes ASF trying to redeem one key on all bots firstly (forwarding) before moving to the next one (distributing). Typically you want to use this option only when you want `Forwarding`, but with altered behaviour of switching the bot on key being used, instead of always going in-order with every key (which would be `Forwarding` alone). This behaviour can be beneficial if you know that majority or even all of your keys are tied to the same game, because in this situation `Forwarding` alone would try to redeem everything on one bot firstly (which makes sense if your keys are for unique games), and `Forwarding` + `Distributing` will switch the bot on the next key, "distributing" the task of redeeming new key onto another bot than the initial one (which makes sense if keys are for the same game, skipping one pointless attempt per key).

The actual bots order for all of the redeeming scenarios is alphabetical, excluding bots that are unavailable (not connected, stopped or likewise). Please keep in mind that there is per-IP and per-account hourly limit of redeeming tries, and every redeem try that didn't end with `OK` contributes to failed tries. ASF will do its best to minimize number of `AlreadyPurchased` failures, e.g. by trying to avoid forwarding a key to another bot that already owns that particular game, but it's not always guaranteed to work due to how Steam is handling licenses. Using redeeming flags such as `Forwarding` or `Distributing` will always increase your likelyhood to hit `RateLimited`.

Also keep in mind that you can't forward or distribute keys to bots that you do not have access to. This should be obvious, but ensure that you're at least `Operator` of all the bots you want to include in your redeeming process, for example with `status ASF` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**.

* * *

### `SendOnFarmingFinished`

`bool` Typ mit Standardwert von `false`. When ASF is done with farming given account, it can automatically send steam trade containing everything farmed up to this point to user with `Master` permission, which is very convenient if you don't want to bother with trades yourself. This option works the same as `loot` command, therefore keep in mind that it requires user with `Master` permission set, you might also require valid `SteamTradeToken`, including using an account that is actually eligible for trading. In addition to initiating `loot` after finishing farming, ASF will also initiate `loot` on each new items notification (when not farming), and after completing each trade that results in new items (always) when this option is active. This is especially useful for "forwarding" items received from other people to our account. It's strongly recommended to use this feature together with **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** being set, as there is no point in sending automatic trades if you need to manually confirm them. Wenn du dir nicht sicher bist, wie du diese Eigenschaft einstellen sollst, belasse sie bei dem Standardwert `false`.

* * *

### `SendTradePeriod`

`byte` Typ mit Standardwert von `0`. Diese Eigenschaft funktioniert sehr ähnlich wie `SendOnFarmingFinished` Eigenschaft, mit einem Unterschied - anstatt Handelsangebote zu senden, wenn das Sammeln abgeschlossen ist, können wir sie auch alle `SendTradePeriod` Stunden senden, unabhängig davon, wie viel wir noch zu sammeln haben. This is useful if you want to `loot` your alt accounts on usual basis instead of waiting for it to finish farming. Default value of `0` disables this feature, if you want your bot to send you trade e.g. every day, you should put `24` here. It's strongly recommended to use this feature together with **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** being set, as there is no point in sending automatic trades if you need to manually confirm them. If you're not sure how to set this property, leave it with default value of `0`.

* * *

### `ShutdownOnFarmingFinished`

`bool` Typ mit Standardwert von `false`. ASF is "occupying" an account for the whole time of process being active. When given account is done with farming, ASF periodically checks it (every `IdleFarmingPeriod` hours), if perhaps some new games with steam cards were added in the meantime, so it can resume farming of that account without a need to restart the process. This is useful for majority of people, as ASF can automatically resume farming when needed. However, you may actually want to stop the process when given account is fully farmed, you can achieve that by setting this property to `true`. When enabled, ASF will proceed with logging off when account is fully farmed, which means that it won't be periodically checked or occupied anymore. You should decide yourself if you prefer ASF to work on given bot instance for the whole time, or if perhaps ASF should stop it when farming process is done. When all accounts are stopped and process is not running in `--process-required` **[mode](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-Line-Arguments)**, ASF will shutdown as well. Wenn du dir nicht sicher bist, wie du diese Eigenschaft einstellen sollst, belasse sie bei dem Standardwert `false`.

* * *

### `SteamLogin`

`string` Typ mit Standardwert von `null`. Diese Eigenschaft definiert dein Steam-Login - das, mit dem du dich bei Steam anmeldest. Zusätzlich zur Definition des Steam-Login kannst du hier auch den Standardwert `null` beibehalten, wenn du dein Steam-Login bei jedem ASF-Start eingeben möchtest, anstatt es in die Konfiguration einzutragen. Dies kann für dich nützlich sein, wenn du sensible Daten nicht in der Konfigurationsdatei speichern möchtest.

* * *

### `SteamMasterClanID`

`ulong` Typ mit Standardwert von `0`. This property defines the steamID of the steam group that bot should automatically join, including its group chat. You can check steamID of your group by navigating to its **[page](https://steamcommunity.com/groups/archiasf)**, then adding `/memberslistxml?xml=1` to the end of the link, so the link will look like **[this](https://steamcommunity.com/groups/archiasf/memberslistxml?xml=1)**. Then you can get steamID of your group from the result, it's in `<groupID64>` tag. In above example it would be `103582791440160998`. In addition to trying to join given group at startup, the bot will also automatically accept group invites to this group, which makes it possible for you to invite your bot manually if your group has private membership. If you don't have any group dedicated for your bots, you should keep this property with default value of `0`.

* * *

### `SteamParentalCode`

`string` Typ mit Standardwert von `null`. This property defines your steam parental PIN. ASF requires an access to resources protected by steam parental, therefore if you use that feature, you need to provide ASF with parental unlock PIN, so it can operate normally. Default value of `null` means that there is no steam parental PIN required to unlock this account, and this is probably what you want if you don't use steam parental functionality. In addition to defining steam parental PIN here, you may also use value of `0` if you want to enter your steam parental PIN on each ASF startup instead of putting it in the config. This may be useful for you if you don't want to save sensitive data in config file.

* * *

### `SteamPassword`

`string` Typ mit Standardwert von `null`. Diese Eigenschaft definiert dein Steam-Passwort - das, mit dem du dich bei Steam anmeldest. Zusätzlich zur Definition des Steam-Passworts kannst du hier auch den Standardwert `null` beibehalten, wenn du dein Steam-Passwort bei jedem ASF-Start eingeben möchtest, anstatt es in die Konfiguration einzutragen. Dies kann für dich nützlich sein, wenn du sensible Daten nicht in der Konfigurationsdatei speichern möchtest.

* * *

### `SteamTradeToken`

`string` Typ mit Standardwert von `null`. Wenn du deinen Bot auf deiner Freundesliste hast, dann kann der Bot dir sofort ein Handelsangebot schicken, ohne sich um den Handels-Code zu kümmern, deshalb kannst du diese Eigenschaft auf dem Standardwert von `null` belassen. Wenn du dich jedoch dazu entscheidest, deinen Bot NICHT auf deiner Freundesliste zu haben, dann musst du einen Handels-Code dessen Benutzers generieren und eintragen an den dieser Bot Handelsangebote senden möchte. In other words, this property should be filled with trade token of the account that is defined with `Master` permission in `SteamUserPermissions` of **this** bot instance.

In order to find your token, as logged in user with `Master` permission, navigate **[here](https://steamcommunity.com/my/tradeoffers/privacy)** and take a look at your trade URL. The token we're looking for is made out of 8 characters after `&token=` part in your trade URL. You should copy and put those 8 characters here, as `SteamTradeToken`. Do not include whole trading URL, neither `&token=` part, only token itself.

* * *

### `SteamUserPermissions`

` ImmutableDictionary<ulong, byte>` Typ mit einem leeren Standardwert. This property is a dictionary property which maps given Steam user identified by his 64-bit steam ID, to `byte` number that specifies his permission in ASF instance. Currently available bot permissions in ASF are defined as:

| Wert | Name          | Beschreibung                                                                                                                                                                                       |
| ---- | ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0    | None          | No permission, this is mainly a reference value that is assigned to steam IDs missing in this dictionary - there is no need to define anybody with this permission                                 |
| 1    | FamilySharing | Provides minimum access for family sharing users. Once again, this is mainly a reference value since ASF is capable of automatically discovering steam IDs that we permitted for using our library |
| 2    | Operator      | Provides basic access to given bot instances, mainly adding licenses and redeeming keys                                                                                                            |
| 3    | Master        | Provides full access to given bot instance                                                                                                                                                         |

In short, this property allows you to handle permissions for given users. Permissions are important mainly for access to ASF **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**, but also for enabling many ASF features, such as accepting trades. For example you might want to set your own account as `Master`, and give `Operator` access to 2-3 of your friends so they can easily redeem keys for your bot with ASF, while **not** being eligible e.g. for stopping it. Thanks to that you can easily assign permissions to given users and let them use your bot to some specified by you degree.

We recommend to set exactly one user as `Master`, and any amount you wish as `Operator`s and below. While it's technically possible to set multiple `Master`s and ASF will work correctly with them, for example by accepting all of their trades sent to the bot, ASF will use only one of them (with lowest steam ID) for every action that requires a single target, for example a `loot` request, so also properties like `SendOnFarmingFinished` or `SendTradePeriod`. If you perfectly understand those limitations, especially the fact that `loot` request will always send items to the `Master` with lowest steam ID, regardless of the `Master` that actually executed the command, then you can define multiple users with `Master` permission here, but we still recommend a single master scheme - multiple masters scheme is discouraged setup that is not supported.

It's nice to note that there is one more extra `Owner` permission, which is declared as `SteamOwnerID` global config property. You can't assign `Owner` permission to anybody here, as `SteamUserPermissions` property defines only permissions that are related to the bot instance, and not ASF as a process.

* * *

### `TradingPreferences`

`byte flags` Typ mit Standardwert von `0`. Diese Eigenschaft definiert das ASF-Verhalten beim Handeln und ist wie folgt definiert:

| Wert | Name                | Beschreibung                                                                                                                                                                                                                |
| ---- | ------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0    | None                | Keine Handelspräferenzen - akzeptiert nur `Master` Handelsangebote                                                                                                                                                          |
| 1    | AcceptDonations     | Akzeptiert Handelsangebote, bei denen wir nichts verlieren                                                                                                                                                                  |
| 2    | SteamTradeMatcher   | Akzeptiert Duplikat-Matching **[STM](https://www.steamtradematcher.com)**-ähnliche Handelsangebote. Weitere Informationen findest du unter **[Handeln](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading-de-DE)** |
| 4    | MatchEverything     | Erfordert das Setzen von `SteamTradeMatcher` und akzeptiert dabei neben guten und neutralen auch schlechte Handelsangebote                                                                                                  |
| 8    | DontAcceptBotTrades | Akzeptiert keine automatischen `loot` Handelsangebote von anderen Bot-Instanzen                                                                                                                                             |

Bitte bedenke, dass diese Eigenschaft das Feld `flags` ist, daher ist es möglich, eine beliebige Kombination von verfügbaren Werten auszuwählen. Schau dir **[JSON-Mapping](#json-mapping)** an, wenn du mehr darüber erfahren möchtest. Wenn keines der Flags aktiviert wird, wird die Option `None` verwendet.

Für weitere Erläuterungen zur ASF-Handelslogik und zur Beschreibung jedes verfügbaren Flags besuche bitte den Abschnitt **[Handeln](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading-de-DE)**.

* * *

### `TransferableTypes`

`ImmutableHashSet<byte>` Typ mit Standardwert von `1, 3, 5` Steam-Gegenstands-Typen. Diese Eigenschaft definiert, welche Steam-Gegenstands-Typen für die Übertragung zwischen Bots während `transfer` **[Befehl](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE)** berücksichtigt werden. ASF wird sicherstellen, dass nur Gegenstände von `TransferableTypes` in ein Handelsangebot aufgenommen werden, daher kannst du mit dieser Eigenschaft wählen, was du in einem Handelsangebot erhalten möchtest, das an einen deiner Bots gesendet wird.

| Wert | Name              | Beschreibung                                                                                  |
| ---- | ----------------- | --------------------------------------------------------------------------------------------- |
| 0    | Unknown           | Jeder Typ, der nicht in eine der folgenden Kategorien passt                                   |
| 1    | BoosterPack       | Booster Pack mit 3 zufälligen Karten aus einem Spiel                                          |
| 2    | Emoticon          | Emoticon zur Verwendung im Steam-Chat                                                         |
| 3    | FoilTradingCard   | Folienvariante von `TradingCard`                                                              |
| 4    | ProfileBackground | Profilhintergrund zur Verwendung in deinem Steam-Profil                                       |
| 5    | TradingCard       | Steam-Karte, die für die Herstellung von Abzeichen (Nicht-Folie) verwendet werden             |
| 6    | SteamGems         | Steam-Edelsteine, die für die Herstellung von Booster Packs verwendet werden, inklusive Säcke |

Bitte bedenke, dass ASF unabhängig von den obigen Einstellungen nur nach Steam (`appID` von 753) Community (`contextID` von 6) Gegenständen fragt, so dass alle Spiel-Gegenstände und Geschenke und dergleichen per Definition aus dem Handelsangebot ausgeschlossen sind.

Die Standard-ASF-Einstellung basiert auf der am häufigsten verwendeten Nutzung des Bots, wobei nur Booster Packs und Handels-Karten (einschließlich Folien) übertragen werden. Die hier definierte Eigenschaft ermöglicht es dir, dieses Verhalten so zu verändern, dass es dich zufrieden stellt. Bitte bedenke, dass alle nicht oben definierten Typen als `Unknown` Typ angezeigt werden, was besonders wichtig ist, wenn Valve einen neuen Steam-Gegenstand veröffentlicht, der ebenfalls von ASF als `Unknown` markiert wird, bis er hier (in der zukünftigen Version) hinzugefügt wird. Deshalb ist es im Allgemeinen nicht empfehlenswert, `Unknown` in deinen `TransferableTypes` einzugeben, es sei denn, du weißt, was du tust, und du verstehst auch, dass ASF dein gesamtes Inventar in einem Handelsangebot versenden wird, wenn das Steam-Netzwerk wieder unterbrochen wird und alle deine Gegenstände als `Unknown` meldet. Mein nachdrücklicher Vorschlag ist, `Unknown` nicht in das Feld `TransferableTypes` einzutragen, selbst wenn du alles übertragen möchtest.

* * *

### `UseLoginKeys`

`bool` Typ mit Standardwert von `true`. Diese Eigenschaft legt fest, ob ASF den Anmelde-Schlüssel-Mechanismus für dieses Steam-Konto verwenden soll. Der Anmelde-Schlüssel-Mechanismus funktioniert sehr ähnlich wie die "Remember me"-Option des offiziellen Steam-Clients, die es ASF ermöglicht, einen temporären, einmaligen Anmelde-Schlüssel für den nächsten Anmeldeversuch zu speichern und zu verwenden, wodurch die Angabe von Passwort, Steam Guard oder 2FA-Code übergangen wird, solange unser Anmelde-Schlüssel gültig ist. Der Anmelde-Schlüssel wird in der Datei `BotName.db` gespeichert und automatisch aktualisiert. Aus diesem Grund musst du kein Passwort/SteamGuard/2FA-Code angeben, nachdem du dich nur einmal erfolgreich mit ASF angemeldet hast.

Die Anmelde-Schlüssel werden standardmäßig für deine Bequemlichkeit verwendet, so dass du nicht bei jedem Anmeldevorgang `SteamPassword`, SteamGuard oder 2FA-Code (wenn du nicht **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-de-DE)** verwendest) eingeben musst. Es ist auch eine überlegene Alternative, da der Anmelde-Schlüssel nur einmalig verwendet werden kann und dein ursprüngliches Passwort in keiner Weise offenbart. Genau die gleiche Methode wird von deinem ursprünglichen Steam-Client verwendet, der deinen Konto-Namen und Anmelde-Schlüssel für deinen nächsten Anmeldeversuch speichert, was praktisch die gleiche ist wie die Verwendung von `SteamLogin` mit `UseLoginKeys` und leerem `SteamPassword` in ASF.

Einige Leute sind jedoch vielleicht sogar über dieses kleine Detail besorgt, deshalb steht dir diese Option hier zur Verfügung, wenn du sicherstellen möchtest, dass ASF keine Art von Daten speichert, die es ermöglichen würden, die vorherige Sitzung nach dem Schließen wieder aufzunehmen, was dazu führt, dass eine vollständige Authentifizierung bei jedem Anmeldeversuch obligatorisch ist. Das Deaktivieren dieser Option funktioniert genau so, wie wenn du "Remember me" im offiziellen Steam-Client nicht aktivierst. Wenn du nicht weißt, was du tust, solltest du es bei dem Standardwert `true` belassen.

**[Zum Seitenanfang](#konfiguration)**

* * *

## Dateistruktur

ASF benutzt eine einfache Dateistruktur.

    ├── config
    │     ├── ASF.json
    │     ├── ASF.db
    │     ├── Bot1.json
    │     ├── Bot1.db
    │     ├── Bot1.bin
    │     ├── Bot2.json
    │     ├── Bot2.db
    │     ├── Bot2.bin
    │     └── ...
    ├── ArchiSteamFarm.dll
    ├── log.txt
    └── ...
    

Um ASF an einen neuen Ort zu verschieben, z.B. auf einen anderen PC, genügt es, das Verzeichnis `config` allein zu verschieben/kopieren, und das ist die empfohlene Methode, um jede Form von "ASF-Backups" durchzuführen.

Die `log.txt` Datei enthält das Protokoll, das durch deinen letzten ASF-Lauf erzeugt wurde. Diese Datei enthält keine sensiblen Informationen und ist äußerst nützlich, wenn es um Probleme, Abstürze oder einfach nur als Information an dich geht, was im letzten ASF-Lauf passiert ist. Wir werden sehr oft nach einer Log-Datei fragen, wenn du auf Probleme oder Fehler stößt. ASF verwaltet diese Datei automatisch für dich, aber du kannst die ASF **[Protokollierung](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Logging-de-DE)**s-Modul weiter optimieren, wenn du ein fortgeschrittener Benutzer bist.

`config` Verzeichnis ist der Ort, der die Konfiguration für ASF enthält, einschließlich aller seiner Bots.

`ASF.json` ist eine globale ASF-Konfigurationsdatei. Diese Konfiguration wird verwendet, um anzugeben, wie sich ASF als Prozess verhält, was alle Bots und das Programm selbst betrifft. Dort findest du globale Eigenschaften wie ASF-Prozesseigentümer, automatische Aktualisierungen oder Debugging.

`BotName.json` ist eine Konfiguration der gegebenen Bot-Instanz. Diese Konfiguration wird verwendet, um das Verhalten einer gegebenen Bot-Instanz festzulegen, daher sind diese Einstellungen nur für diesen Bot spezifisch und nicht für andere bestimmt. Dies ermöglicht es dir, Bots mit verschiedenen Einstellungen zu konfigurieren, und nicht unbedingt alle funktionieren auf die gleiche Weise.

Neben den Konfigurationsdateien verwendet ASF auch das Verzeichnis `config` zum Speichern von Datenbanken.

`ASF.db` ist eine globale ASF-Datenbankdatei. Es fungiert als globaler dauerhafter Speicher und wird zum Speichern verschiedener Informationen im Zusammenhang mit dem ASF-Prozess verwendet, wie z.B. IPs von lokalen Steam-Servern. **Du solltest diese Datei nicht bearbeiten**.

`BotName.db` ist eine Datenbank der jeweiligen Bot-Instanz. Diese Datei wird verwendet, um wichtige Daten der jeweiligen Bot-Instanz im dauerhaften Speicher zu speichern, wie z.B. Anmelde-Schlüssel oder ASF 2FA. **Du solltest diese Datei nicht bearbeiten**.

`BotName.bin` ist eine spezielle Datei der jeweiligen Bot-Instanz, die Informationen über den Steam-Sentry-Hash enthält. Der Sentry-Hash wird für die Authentifizierung mit dem `SteamGuard` Mechanismus verwendet, sehr ähnlich Steam-Datei `ssfn`. **Du solltest diese Datei nicht bearbeiten**.

`BotName.keys` ist eine spezielle Datei, die zum Importieren von Produkt-Schlüsseln in den **[Hintergrundproduktschlüsselaktivierer ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer-de-DE)** verwendet werden kann. Es ist nicht zwingend erforderlich und wird nicht generiert, aber von ASF anerkannt. Diese Datei wird nach dem erfolgreichen Import der Produkt-Schlüssel automatisch gelöscht.

`BotName.maFile` ist eine spezielle Datei, die für den Import von **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-de-DE)** verwendet werden kann. Es ist nicht zwingend erforderlich und wird nicht generiert, aber von ASF erkannt, wenn dein `BotName` ASF 2FA noch nicht verwendet. Diese Datei wird nach erfolgreichem Import von ASF 2FA automatisch gelöscht.

**[Zum Seitenanfang](#konfiguration)**

* * *

## JSON-Mapping

Jede Konfigurationseigenschaft hat ihren Typ. Der Typ der Eigenschaft definiert Werte, die für sie gültig sind. Du kannst nur Werte verwenden, die für einen bestimmten Typ gültig sind - wenn du einen ungültigen Wert verwendest, dann kann ASF deine Konfiguration nicht verarbeiten.

**Wir empfehlen dringend, den ConfigGenerator für die Generierung von Konfigurationen zu verwenden** - er handhabt den größten Teil des Low-Level-Sachen (wie z.B. die Typ-Validierung) für dich, so dass du nur die richtigen Werte eingeben musst, und du musst auch die unten angegebenen Variablentypen nicht verstehen. Dieser Abschnitt ist hauptsächlich für Personen gedacht, die Konfigurationen manuell erstellen/bearbeiten, damit sie wissen, welche Werte sie verwenden können.

Von ASF verwendete Typen sind native C#-Typen, die im Folgenden aufgeführt werden:

* * *

`bool` - Boolescher Typ, der nur `true` und `false` Werte akzeptiert.

Beispiel: `"Enabled": true`

* * *

`byte` - Unsignierter Byte-Typ, der nur ganze Zahlen von `0` bis `255` (einschließlich) akzeptiert.

Beispiel: `"ConnectionTimeout": 60`

* * *

`uint` - Unsignierter Ganzzahl-Typ, der nur ganze Zahlen von `0` bis `4294967295` (einschließlich) akzeptiert.

* * *

`ulong` - Unsignierter long integer Typ, der nur ganze Zahlen von `0` bis `18446744073709551615` (einschließlich) akzeptiert.

Beispiel: `"SteamMasterClanID": 103582791440160998`

* * *

`string` - Zeichenketten-Typ, der jede beliebige Zeichenfolge akzeptiert, einschließlich der leeren Folge `""` und `null`. Sowohl die leere Sequenz als auch der Wert `null` werden von ASF gleich behandelt, so dass es deiner Präferenz überlassen bleibt, welche davon du verwenden möchtest.

Beispiele: `"SteamLogin": null`, `"SteamLogin": ""`, `"SteamLogin": "MeinLogin"`

* * *

`ImmutableHashSet<valueType>` - Unveränderliche Sammlung (Satz) von eindeutigen Werten in gegebenen `valueType`. In JSON ist es definiert als Array von Elementen in gegebenem `valueType`.

Beispiel für `ImmutableHashSet<uint>`: `"Blacklist": [267420, 303700, 335590]`

* * *

`ImmutableDictionary<keyType, valueType>` - Unveränderliches Wörterbuch (Map), das einen in seinem `keyType` angegebenen Schlüssel auf den in seinem `valueType` angegebenen Wert abbildet. In JSON wird es als Objekt mit Schlüssel-Wert-Paaren definiert. Bedenke, dass `keyType` in diesem Fall immer angegeben wird, auch wenn es sich um einen Wert-Typ wie `ulong` handelt.

Beispiel für `ImmutableDictionary<ulong, byte>`: `"SteamUserPermissions": { "76561198174813138": 3, "76561198174813137": 1 }`

* * *

`flags` - Das Attribut Flags kombiniert mehrere verschiedene Eigenschaften zu einem Endwert, indem es bitweise Operationen anwendet. Auf diese Weise kannst du jede mögliche Kombination aus verschiedenen zulässigen Werten gleichzeitig wählen. Der Endwert wird als Summe aller Werte der aktivierten Optionen berechnet.

Zum Beispiel, wenn folgende Werte angegeben werden:

| Wert | Name |
| ---- | ---- |
| 0    | None |
| 1    | A    |
| 2    | B    |
| 4    | C    |

Die Verwendung von `B + C` würde zu einem Wert von `6` führen, die Verwendung von `A + C` würde zu einem Wert von `5` führen, die Verwendung von `C` würde zu einem Wert von `4`führen und so weiter. Dies ermöglicht es dir, jede mögliche Kombination von aktivierten Werten zu erstellen - wenn du dich dazu entschieden hast, alle zu aktivieren, indem du `None + A + B + C` wählst, bekommst du den Wert von `7`. Außerdem ist zu beachten, dass das Flag mit dem Wert `0` per Definition in allen anderen verfügbaren Kombinationen aktiviert ist, daher ist es sehr oft ein Flag, das nichts Bestimmtes aktiviert (wie `None`).

Wie du sehen kannst, haben wir im obigen Beispiel 3 verfügbare Flags zum Ein-/Ausschalten (`A`, `B`, `C`) und 8 mögliche Werte insgesamt (`None -> 0`, `A -> 1`, `B -> 2`, `A+B -> 3`, `C -> 4`, `A+C -> 5`, `B+C -> 6`, `A+B+C -> 7`).

**[Zum Seitenanfang](#konfiguration)**

* * *

## Kompatibilitätsmapping

Aufgrund von JavaScript-Beschränkungen, da einfache `ulong` Felder in JSON bei Verwendung des webbasierten ConfigGenerators nicht ordnungsgemäß serialisiert werden können, werden `ulong` Felder als Zeichenketten mit `s_` Präfix in der resultierenden Konfiguration dargestellt. Dazu gehört z.B. `"SteamOwnerID": 76561198006963719`, der von unserem ConfigGenerator als `"s_SteamOwnerID": "76561198006963719"` geschrieben wird. ASF enthält die richtige Logik für die automatische Verarbeitung dieses Zeichenketten-Mappings, so dass `s_` Einträge in deine Konfigurationen tatsächlich gültig und korrekt generiert sind. Wenn du selbst Konfigurationen generierst, empfehlen wir, nach Möglichkeit an den ursprünglichen `ulong` Feldern festzuhalten, aber wenn du dazu nicht in der Lage bist, kannst du auch diesem Schema folgen und sie als Zeichenketten mit `s_` Präfix an ihren Namen kodieren. Wir hoffen, diese JavaScript-Einschränkung irgendwann aufheben zu können.

**[Zum Seitenanfang](#konfiguration)**

* * *

## Konfigurations-Kompatibilität

Es ist für ASF oberste Priorität, mit älteren Konfigurationen kompatibel zu bleiben. Wie du bereits weißt, werden fehlende Konfigurationseigenschaften genauso behandelt, wie wenn sie mit ihren **Standardwerten** angegeben würden. Wenn also eine neue Konfigurationseigenschaft in einer neuen Version von ASF eingeführt wird, bleiben all deine Konfigurationen **kompatibel** mit der neuen Version, und ASF wird diese neue Konfigurationseigenschaft so behandeln, wie sie mit ihrem **Standardwert** definiert wäre. Du kannst jederzeit Konfigurationseigenschaften nach deinen Wünschen hinzufügen, entfernen oder bearbeiten. Wir empfehlen, definierte Konfigurationseigenschaften nur auf die zu ändernden Eigenschaften zu beschränken, da du auf diese Weise automatisch Standardwerte für alle anderen vererbst, nicht nur deine Konfiguration sauber hältst, sondern auch die Kompatibilität erhöhst, falls wir beschließen, einen Standardwert für Eigenschaften zu ändern, die du nicht explizit selbst festlegen möchtest.

**[Zum Seitenanfang](#konfiguration)**

* * *

## Automatisches Nachladen

Ab ASF V2.1.6.2+ ist es möglich, dass Konfigurationen "on-the-fly" geändert werden - dadurch wird ASF automatisch:

- Eine neue Bot-Instanz erstellen (und startet sie bei Bedarf), wenn du die Konfiguration erstellst
- Stoppt (falls erforderlich) und entfernt alte Bot-Instanz, wenn du ihre Konfiguration löschst
- Stoppt (und startet bei Bedarf) eine beliebige Bot-Instanz, wenn du ihre Konfiguration bearbeitest
- Startet den Bot (falls erforderlich) unter neuem Namen neu, wenn du seine Konfiguration umbenennst

All dies ist transparent und wird automatisch durchgeführt, ohne dass das Programm neu gestartet oder andere (nicht betroffene) Bot-Instanzen beendet werden müssen.

Darüber hinaus wird sich ASF auch selbst neu starten (wenn `AutoRestart` es erlaubt), wenn du die ASF-Kern-Konfiguration `ASF.json` änderst. Ebenso wird das Programm beendet, wenn du es löschst oder umbenennst.

**[Zum Seitenanfang](#konfiguration)**