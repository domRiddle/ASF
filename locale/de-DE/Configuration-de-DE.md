# Konfiguration

Diese Seite widmet sich der Konfiguration von ASF. Es dient als eine lückenlose Dokumentation des `config` Verzeichnisses, sodass Sie ASF auf Ihre Bedürfnisse abstimmen können.

* **[Einleitung](#introduction)**
* **[Web-basierter ConfigGenerator](#web-based-configgenerator)**
* **[ASF-UI-Einstellungen](#asf-ui-configuration)**
* **[Manuelle Konfiguration](#manual-configuration)**
* **[Globale Konfiguration](#global-config)**
* **[Bot-Konfiguration](#bot-config)**
* **[Datei-Struktur](#file-structure)**
* **[JSON-Strukturierung](#json-mapping)**
* **[Kompatibilitätsstrukturierung](#compatibility-mapping)**
* **[Konfigurationskompatibilität](#configs-compatibility)**
* **[Automatisches Nachladen](#auto-reload)**

---

## Einleitung

Die ASF-Konfiguration gliedert sich in zwei Hauptteile - die globale (Prozess-)Konfiguration und die Konfiguration jedes einzelnen Bots. Jeder Bot hat seine eigene Bot-Konfigurationsdatei namens `BotName.json` (wobei `BotName` der Name des Bots ist), während die globale ASF (Prozess-)Konfiguration eine einzige Datei namens `ASF.json` ist.

Ein Bot ist ein einzelnes Steam-Konto welches am ASF-Prozess teilnimmt. Um ordnungsgemäß zu funktionieren benötigt ASF mindestens **eine** definierte Bot-Instanz. Es gibt keine prozessbedingte Begrenzung der Bot-Instanzen, sodass Sie theoretisch unendlich viele Bots (Steam-Konten) verwenden können.

ASF verwendet das **[JSON](https://en.wikipedia.org/wiki/JSON)** Format zum Speichern seiner Konfigurationsdateien. Es ist ein benutzerfreundliches, lesbares und sehr universelles Format in dem man das Programm konfigurieren kann. Keine Sorge, Sie müssen sich nicht mit JSON auskennen um ASF zu konfigurieren. Wir erwähnen es nur, falls Sie bereits daran denken ASF-Konfigurationen mit einer Art Bash-Skript massenhaft zu erstellen.

Die Konfiguration kann auf verschiedene Art und Weise erfolgen. Sie können unseren **[webbasierten ConfigGenerator](https://justarchinet.github.io/ASF-WebConfigGenerator)**verwenden, der eine von ASF unabhängige lokale App ist. Sie können unsere **[ASF-ui](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-de-DE#asf-ui)** IPC-Benutzeroberfläche für die Konfiguration direkt in ASF verwenden. Schließlich können Sie die Konfigurationsdateien immer manuell erzeugen, da sie der unten angegebenen festen JSON-Struktur folgen. Wir werden in aller Kürze die verfügbaren Optionen erklären.

---

## Web-basierter ConfigGenerator

Der Zweck des **[web-basierten Config-Generators](https://justarchinet.github.io/ASF-WebConfigGenerator)** ist es, Ihnen ein benutzerfreundliches Frontend zur Verfügung zu stellen, das zum Erzeugen von ASF-Konfigurationsdateien verwendet wird. Der web-basierte ConfigGenerator ist zu 100% Client-basiert, was bedeutet, dass die von Ihnen eingegebenen Daten nirgendwo hin gesendet, sondern nur lokal verarbeitet werden. Dies garantiert Sicherheit und Zuverlässigkeit, da es sogar **[offline](https://github.com/JustArchiNET/ASF-WebConfigGenerator/tree/main/docs)** funktioniert, wenn zuvor alle Dateien heruntergeladen wurden. Sie müssen dann nur noch `index.html` im Browser öffnen.

Der web-basierte ConfigGenerator wurde unter Chrome und Firefox getestet, sollte aber in allen gängigen JavaScript-fähigen Browsern ordnungsgemäß funktionieren.

Die Verwendung ist denkbar einfach - man wählt über die entsprechenden Registerkarten, ob man eine Konfiguration für `ASF` oder `Bot` erstellen möchte, überprüft ob die gewählte Version der Konfigurationsdatei zur verwendeten ASF-Version passt, füllt dann die entsprechenden Felder aus und klickt abschließend auf die Schaltfläche "Herunterladen". Verschieben Sie diese Datei in das ASF `config` Verzeichnis und überschreiben Sie bei Bedarf vorhandene Dateien. Bei allen zukünftigen Anpassungen können diese Schritte jederzeit wiederholt werden. Der Rest dieses Abschnitts erklärt alle zur Verfügung stehenden Konfigurationsmöglichkeiten.

---

## ASF-UI-Einstellungen

Unsere **[ASF-ui](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-de-DE#asf-ui)** IPC-Schnittstelle erlaubt es Ihnen auch ASF zu konfigurieren und ist eine bessere Lösung für die Neukonfiguration von ASF nach der Generierung der ersten Konfigurationen aufgrund der Tatsache, dass es die Konfigurationen im Ort bearbeiten kann im Gegensatz zum webbasierten Config-Generator, welcher sie statisch erzeugt.

In order to use ASF-ui, you must have our **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** interface enabled itself. `IPC` is enabled by default starting with ASF V5.1.0.0, therefore you can access it right away, as long as you didn't disable it yourself.

After launching the program, simply navigate to ASF's **[IPC address](http://localhost:1242)**. If everything worked properly, you can change ASF configuration from there as well.

---

## Manuelle Konfiguration

In general we strongly recommend using either our ConfigGenerator or ASF-ui, as it's much easier and ensures you won't make a mistake in the JSON structure, but if for some reason you don't want to, then you can also create proper configs manually. Check JSON examples below for a good start in proper structure, you can copy the content into a file and use it as a base for your config. Since you're not using any of our frontends, ensure that your config is **[valid](https://jsonlint.com)**, as ASF will refuse to load it if it can't be parsed. Even if it's a valid JSON, you also have to ensure that all the properties have the proper type, as required by ASF. For proper JSON structure of all available fields, refer to **[JSON mapping](#json-mapping)** section and our documentation below.

---

## Globale Konfiguration

Die globale Konfiguration befindet sich in der Datei `ASF.json` und hat folgende Struktur:

```json
{
    "AutoRestart": true,
    "Blacklist": [],
    "CommandPrefix": "!",
    "ConfirmationsLimiterDelay": 10,
    "ConnectionTimeout": 90,
    "CurrentCulture": null,
    "Debug": false,
    "FarmingDelay": 15,
    "FilterBadBots": true,
    "GiftsLimiterDelay": 1,
    "Headless": false,
    "IdleFarmingPeriod": 8,
    "InventoryLimiterDelay": 4,
    "IPC": true,
    "IPCPassword": null,
    "IPCPasswordFormat": 0,
    "LicenseID": null,
    "LoginLimiterDelay": 10,
    "MaxFarmingTime": 10,
    "MaxTradeHoldDuration": 15,
    "MinFarmingDelayAfterBlock": 60,
    "OptimizationMode": 0,
    "SteamMessagePrefix": "/me ",
    "SteamOwnerID": 0,
    "SteamProtocols": 7,
    "UpdateChannel": 1,
    "UpdatePeriod": 24,
    "WebLimiterDelay": 300,
    "WebProxy": null,
    "WebProxyPassword": null,
    "WebProxyUsername": null
}
```

---

Alle Optionen werden nachfolgend erklärt:

### `AutoRestart`

`bool` Typ mit Standardwert von `true`. Diese Eigenschaft legt fest, ob ASF bei Bedarf einen Selbst-Neustart durchführen darf. Es gibt ein paar Fälle, die von ASF einen Neustart erfordern, wie z.B. die ASF-Aktualisierung (durchgeführt mit `UpdatePeriod` oder dem Befehl `update`), sowie das Verändern der `ASF.json` Konfiguration, dem `restart` Befehl und ähnlich. Normalerweise beinhaltet der Neustart zwei Teile - das Erstellen eines neuen Prozesses und das Beenden des aktuellen Prozesses. Die meisten Benutzer sollten damit einverstanden sein und diese Eigenschaft auf dem Standardwert `true` behalten - wenn du ASF durch dein eigenes Skript und/oder mit `dotnet` ausführst, solltest du vielleicht die volle Kontrolle über den Start des Prozesses haben und eine Situation vermeiden, in der ein neuer (neu gestarteter) ASF-Prozess irgendwo im Hintergrund und nicht im Vordergrund des Skripts läuft, der zusammen mit dem alten ASF-Prozess beendet wurde. Dies ist besonders wichtig, wenn man bedenkt, dass der neue Prozess nicht mehr dein direktes "Kind" ist, was dich z.B. nicht in die Lage versetzen würde, die Standardkonsolen-Eingabe dafür zu verwenden.

Wenn das der Fall ist, ist diese Eigenschaft speziell für dich und du kannst sie auf `false` setzen. Bedenke jedoch, dass **du** in diesem Fall für den Neustart des Prozesses verantwortlich bist. Dies ist einigermaßen wichtig, da ASF sich nur beendet, anstatt einen neuen Prozess zu starten (z.B. nach der Aktualisierung), so dass, wenn du keine Logik hinzugefügt hast, ASF einfach aufhört zu laufen, bis du es wieder startest. ASF beendet sich immer mit einem korrekten Fehlercode, der Erfolg (Null) oder Misserfolg (ungleich Null) anzeigt. Auf diese Weise kannst du in deinem Skript eine entsprechende Logik hinzufügen, die einen automatischen Neustart von ASF im Fehlerfall vermeiden sollte oder zumindest zur weiteren Analyse eine lokale Kopie von `log.txt` erstellt. Bedenke auch, dass der Befehl `restart` ASF immer neu startet, unabhängig davon, wie diese Eigenschaft eingestellt ist, da diese Eigenschaft das Standardverhalten definiert, während der Befehl `restart` den Prozess immer neu startet. Wenn du keinen Grund hast, diese Funktion zu deaktivieren, solltest du sie aktiviert lassen.

---

### `Blacklist`

`ImmutableHashSet<uint>` Typ mit einem leeren Standardwert. As the name suggests, this global config property defines appIDs (games) that will be entirely ignored by automatic ASF farming process. Leider liebt Steam es Sommer/Winter-Sale-Abzeichen als "verfügbar für Kartenabgabe" zu kennzeichnen, was den ASF-Prozess verwirrt, da es den Eindruck erweckt, dass es sich um ein gültiges Spiel handelt das gesammelt werden sollte. Wenn es keine schwarze Liste gäbe, würde ASF irgendwann beim Sammeln eines Spieles (das eigentlich kein Spiel ist) "hängen" bleiben und unendlich warten bis die Karten gesammelt wurden, was aber nie passieren wird. Die schwarze Liste von ASF dient dazu, diese Abzeichen als nicht für das Sammeln verfügbar zu kennzeichnen, so dass wir sie bei der Entscheidung, was zu Sammeln ist, stillschweigend ignorieren und nicht in die Falle tappen können.

ASF includes two blacklists by default - `SalesBlacklist`, which is hardcoded into the ASF code and not possible to edit, and normal `Blacklist`, which is defined here. `SalesBlacklist` is updated together with ASF version and typically includes all "bad" appIDs at the time of release, so if you're using up-to-date ASF then you do not need to maintain your own `Blacklist` defined here. Der Hauptzweck dieser Eigenschaft ist es, dir die Möglichkeit zu geben, neue, zum Zeitpunkt der ASF-Veröffentlichung unbekannte AppIDs (die nicht gesammelt werden sollten) auf die schwarze Liste zu setzen. Hardcoded `SalesBlacklist` is being updated as fast as possible, therefore you're not required to update your own `Blacklist` if you're using latest ASF version, but without `Blacklist` you'd be forced to update ASF in order to "keep running" when Valve releases new sale badge - I don't want to force you to use latest ASF code, therefore this property is here to allow you "fixing" ASF yourself if you for some reason don't want to, or can't, update to new hardcoded `SalesBlacklist` in new ASF release, yet you want to keep your old ASF running. Wenn Sie keinen **triftigen** Grund haben diese Eigenschaft zu bearbeiten, sollte der Standardwert belassen werden.

If you're looking for bot-based blacklist instead, take a look at `fb`, `fbadd` and `fbrm` **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**.

---

### `CommandPrefix`

`string` Typ mit einem Standardwert von `!`. Diese Eigenschaft spezifiziert das **größensensitive** Präfix, das für **[Befehle](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE)** in ASF verwendet wird. Mit anderen Worten: Du solltest es jedem ASF-Befehl voranstellen, damit ASF dir zuhört. Es ist möglich, diesen Wert auf `null` oder leer zu setzen, damit ASF kein Befehlspräfix verwendet, in diesem Fall gibst du Befehle mit ihren einfachen Bezeichnern ein. Dies kann jedoch die Leistung von ASF beeinträchtigen, da ASF optimiert ist, um Nachrichten nicht weiter zu analysieren, wenn sie nicht mit `CommandPrefix` beginnen - wenn du dich absichtlich dazu entscheidest, sie nicht zu verwenden, wird ASF gezwungen sein, alle Nachrichten zu lesen und darauf zu reagieren, selbst wenn es sich nicht um ASF-Befehle handelt. Daher wird empfohlen, weiterhin irgendein `CommandPrefix` zu verwenden, wie z.B. `/`, wenn du den Standardwert von `!` nicht magst. Aus Konsistenzgründen betrifft `CommandPrefix` den gesamten ASF-Prozess. Wenn Sie keinen Grund haben diese Eigenschaft zu bearbeiten, solltest der Standardwert belassen werden.

---

### `ConfirmationsLimiterDelay`

`byte` Typ mit einem Standardwert von `10`. ASF wird sicherstellen, dass mindestens `ConfirmationsLimiterDelay` Sekunden zwischen zwei aufeinanderfolgenden 2FA-Bestätigungen liegen, die Anfragen abrufen, um eine Auslösung des Ratenlimits zu vermeiden - diese werden von **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-de-DE)** während z.B. des Befehls `2faok` sowie bei verschiedenen handelsbezogenen Operationen nach Bedarf verwendet. Der Standardwert wurde auf Basis unserer Tests festgelegt und sollte nicht verringert werden, wenn du auf keine Probleme stoßen möchtest. Wenn Sie keinen **triftigen** Grund haben diese Eigenschaft zu bearbeiten, sollte der Standardwert belassen werden.

---

### `ConnectionTimeout`

`byte` Typ mit einem Standardwert von `90`. Diese Eigenschaft definiert die Auszeiten für verschiedene Netzwerkaktionen, die von ASF ausgeführt werden, in Sekunden. Im Einzelnen definiert `ConnectionTimeout` die Auszeit in Sekunden für HTTP- und IPC-Anfragen, `ConnectionTimeout / 10` definiert die maximale Anzahl der fehlgeschlagenen Heartbeats, während `ConnectionTimeout / 30` die Anzahl der Minuten definiert, die wir für die initiale Steam-Netzwerkverbindungsanforderung berücksichtigen. Der Standardwert von `90` sollte für die Mehrheit der Benutzer in Ordnung sein, aber wenn du eine eher langsame Netzwerkverbindung oder einen PC hast, solltest du diese Zahl vielleicht um etwas Höheres erhöhen (wie `120`). Bedenke, dass höhere Werte langsame oder sogar unzugängliche Steam-Server nicht auf magische Weise reparieren, also sollten wir nicht unendlich auf etwas warten, das nicht passieren wird, und es einfach später noch einmal versuchen. Eine zu hohe Einstellung dieses Wertes führt zu einer übermäßigen Verzögerung bei der Erfassung von Netzwerkproblemen und zu einer Verringerung der Gesamtleistung. Wenn du diesen Wert zu niedrig einstellst, verringert sich auch die Gesamtstabilität und -leistung, da ASF eine gültige Anfrage abbricht, die noch verarbeitet wird. Daher hat das Setzen dieses Wertes unter dem Standardwert im Allgemeinen keinen Vorteil, da Steam-Server von Zeit zu Zeit sehr langsam sind und mehr Zeit für das Verarbeiten von ASF-Anfragen benötigen könnten. Der Standardwert ist eine ausgewogene Balance zwischen dem Vertrauen, dass unsere Netzwerkverbindung stabil ist, und dem Misstrauen des Steam-Netzwerks, unsere Anfrage innerhalb einer bestimmten Auszeit zu bearbeiten. Wenn du Probleme früher erkennen und die ASF-Wiederverbindung bzw. -Antwort beschleunigen möchtest, sollte der Standardwert dies tun (oder ganz knapp darunter, wie z.B. `60`, wodurch ASF weniger geduldig wird). Wenn du stattdessen bemerkst, dass ASF auf Netzwerkprobleme stößt, wie z.B. fehlgeschlagene Anfragen, Heartbeats, die verloren gehen oder die Verbindung zu Steam unterbrochen wird, könnte es sinnvoll sein, diesen Wert zu erhöhen, wenn du sicher bist, dass es sich um **nicht** um Fehler handelt, die durch dein Netzwerk, sondern durch Steam selbst verursacht werden, da zunehmende Auszeiten ASF mehr "geduldig" machen und sich nicht entscheiden, die Verbindung sofort wieder herzustellen.

Eine Beispielsituation, die eine Erhöhung dieser Eigenschaft erfordern könnte, ist die Möglichkeit, ASF mit einem sehr großen Handelsangebot zu beauftragen, das gut 2+ Minuten dauern kann, um von Steam vollständig akzeptiert und bearbeitet zu werden. Durch die Erhöhung des Standard-Timeout wird ASF geduldiger und wartet länger, bevor es entscheidet, dass der Handel nicht durchläuft und die ursprüngliche Anfrage aufgibt.

Eine weitere Situation könnte durch eine sehr langsame Maschine oder Internetverbindung verursacht werden, die mehr Zeit benötigt, um die zu übertragenden Daten zu verarbeiten. Dies ist eine ziemlich seltene Bedingung, da die CPU/Netzwerkbandbreite fast nie ein Engpass ist, aber dennoch eine erwähnenswerte Möglichkeit.

Kurz gesagt, der Standardwert sollte in den meisten Fällen angemessen sein, aber du kannst ihn bei Bedarf erhöhen. Trotzdem macht es keinen großen Sinn, weit über den Standardwert hinauszugehen, da größere Auszeiten unzugängliche Steam-Server nicht magisch beheben. Wenn Sie keinen Grund haben diese Eigenschaft zu bearbeiten, solltest der Standardwert belassen werden.

---

### `CurrentCulture`

`string` Typ mit einem Standardwert von `null`. Standardmäßig versucht ASF, die Sprache deines Betriebssystems zu verwenden, und wird es vorziehen, übersetzte Zeichenketten in dieser Sprache zu verwenden, falls verfügbar. Dies ist möglich dank unserer Community, die versucht, ASF in allen gängigen Sprachen zu **[übersetzen](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Localization-de-DE)**. Wenn du aus irgendeinem Grund deine Betriebssystem-Muttersprache nicht verwenden möchtest, kannst du diese Konfigurationseigenschaft verwenden, um eine gültige Sprache auszuwählen, die du stattdessen verwenden möchtest. Für eine Liste aller verfügbaren Sprachen besuche bitte **[MSDN](https://msdn.microsoft.com/en-us/library/cc233982.aspx)** und suche nach `Language tag`. It's nice to note that ASF accepts both specific cultures, such as `"en-GB"`, but also neutral ones, such as `"en"`. Die Angabe der aktuellen Sprache kann auch andere sprachspezifische Aspekte beeinflussen, wie z.B. das Währungs-/Datumsformat und ähnliches. Bitte bedenke, dass du möglicherweise zusätzliche Schrift-/Sprachpakete benötigst, um sprachspezifische Zeichen richtig darzustellen, wenn du eine nicht-native Sprache gewählt hast. Normalerweise willst du diese Konfigurationseigenschaft nutzen, wenn du ASF auf Englisch anstelle deiner Muttersprache bevorzugst.

---

### `Debug`

`bool` Typ mit einem Standardwert von `false`. Diese Eigenschaft legt fest, ob der Prozess im Debug-Modus laufen soll. Im Debug-Modus erstellt ASF ein spezielles Verzeichnis `debug` neben dem `config` Verzeichnis, das die gesamte Kommunikation zwischen ASF und den Steam-Servern verfolgt. Debug-Informationen können helfen, lästige Probleme im Zusammenhang mit der Vernetzung und dem allgemeinen ASF-Arbeitsablauf zu erkennen. Darüber hinaus werden einige Programmroutinen viel ausführlicher sein, wie z.B. `WebBrowser`, die den genauen Grund für das Scheitern einiger Anfragen angeben - diese Einträge werden in das normale ASF-Protokoll geschrieben. **Du solltest ASF nicht im Debug-Modus ausführen, es sei denn, du wirst von einem Entwickler dazu aufgefordert**. Das Ausführen von ASF im Debug-Modus **verringert die Leistung**, **beeinträchtigt die Stabilität negativ** und ist **weitaus ausführlicher an verschiedenen Stellen**, daher sollte es **nur** gewollt und kurzfristig, für das Debuggen bestimmter Probleme, die Reproduktion des Problems oder das Erhalten von mehr Informationen über eine fehlgeschlagene Anfrage und ähnliches verwendet werden, aber **nicht** für die normale Programmausführung. Du wirst **viele** neue Fehler, Probleme und Ausnahmen sehen - stelle sicher, dass du ein gutes Wissen über ASF, Steam und seine Besonderheiten hast, wenn du dich entscheidest, das alles selbst zu analysieren, da nicht alles relevant ist.

**WARNUNG:** Das Aktivieren dieses Moduses beinhaltet das Protokollieren von **potenziell sensiblen** Informationen wie Anmeldeinformationen und Passwörter, die du für die Anmeldung bei Steam verwendest (aufgrund von Netzwerkprotokollierung). Diese Daten werden sowohl in das Verzeichnis `debug` als auch in die normale `log.txt` Datei geschrieben (diese ist nun absichtlich viel umfangreicher, um diese Information zu protokollieren). Du solltest keine Debug-Inhalte, die von ASF generiert wurden, an einem öffentlichen Ort posten, der ASF-Entwickler sollte dich immer daran erinnern, sie an seine E-Mail oder einen anderen sicheren Ort zu senden. Wir speichern diese sensiblen Details nicht und nutzen sie auch nicht, sie werden als Teil von Debug-Routinen geschrieben, da ihre Anwesenheit für das Problem, das dich betrifft, relevant sein könnte. Wir würden es vorziehen, wenn du das ASF-Logging in keiner Weise ändern würdest, aber wenn du besorgt bist, kannst du diese sensiblen Details entsprechend bearbeiten.

> Beim Editieren werden sensible Details, z. B. durch Sterne, ersetzt. Sie sollten darauf verzichten, sensible Zeilen ganz zu entfernen, da ihre reine Existenz relevant sein könnte und erhalten werden sollte.

---

### `FarmingDelay`

`byte` Typ mit einem Standardwert von `15`. Damit ASF funktioniert, wird es alle `FarmingDelay` Minuten das aktuell gesammelte Spiel überprüfen, ob es vielleicht schon alle Karten erhalten hat. Wenn du diese Eigenschaft zu niedrig einstellst, kann dies dazu führen, dass eine übermäßige Anzahl von Steam-Anfragen gesendet wird, während eine zu hohe Einstellung dazu führen kann, dass ASF immer noch den vorgegebenen Titel für bis zu `FarmingDelay` Minuten, nachdem es vollständig gesammelt wurde, "sammelt". Der Standardwert sollte für die meisten Benutzer hervorragend sein, aber wenn du viele Bots im Einsatz hast, kannst du ihn auf etwa `30` Minuten erhöhen, um das Senden von Steam-Anfragen zu begrenzen. Es ist gut zu wissen, dass ASF einen ereignisbasierten Mechanismus verwendet und die Spiel-Abzeichen-Seite für jeden gesammelten Steam-Gegenstand überprüft, so dass wir im Allgemeinen **nicht einmal in festen Zeitabständen überprüfen müssen**, aber da wir dem Steam-Netzwerk nicht voll vertrauen - überprüfen wir die Spiel-Abzeichen-Seite trotzdem, wenn wir es nicht überprüft haben, indem die Karte im letzten `FarmingDelay` Minuten gesammelt wurde (falls das Steam-Netzwerk uns nicht über den gesammelten Gegenstand oder dergleichen informiert hat). Angenommen, das Steam-Netzwerk funktioniert ordnungsgemäß, wird die Herabsetzung dieses Wertes **in keiner Weise die Effizienz des Sammelns verbessern**, während **die Erhöhung des Netzwerkaufwandes dies signifikant tut** - es wird empfohlen, es nur (falls erforderlich) vom Standardwert aus auf `15` Minuten zu erhöhen. Wenn du keinen **triftigen** Grund hast diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

---

### `FilterBadBots`

`bool` Typ mit Standardwert von `true`. This property defines whether ASF will automatically decline trade offers that are received from known and marked bad actors. In order to do that, ASF will communicate with our server on as-needed basis to fetch a list of blacklisted Steam identificators. The bots listed are operated by people that are classified as harmful towards ASF initiative by us, such as those that violate our **[code of conduct](https://github.com/JustArchiNET/ArchiSteamFarm/blob/main/.github/CODE_OF_CONDUCT.md)**, use provided functionality and resources by us such as **[`PublicListing`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/ItemsMatcherPlugin#publiclisting)** in order to abuse and exploit other people, or are doing outright criminal activity such as launching DDoS attacks on the server. Since ASF has strong stance on overall fairness, honesty and cooperation between its users in order to make the whole community thrive, this property is enabled by default, and therefore ASF filters bots that we've classified as harmful from services offered. Unless you have a **strong** reason to edit this property, such as disagreeing with our statement and intentionally allowing those bots to operate (including exploiting your accounts), you should keep it at default.

---

### `GiftsLimiterDelay`

`byte` Typ mit einem Standardwert von `1`. ASF wird sicherstellen, dass mindestens `GiftsLimiterDelay` Sekunden zwischen zwei aufeinanderfolgenden Anfragen zur Handhabung (Einlösen) von Geschenken/Produktschlüsseln/Lizenzen liegen, um zu vermeiden, dass ein Anfragen-Limit ausgelöst wird. Darüber hinaus wird es auch als globaler Begrenzer für Spiele-Listen-Anfragen verwendet, wie z.B. die vom `owns` **[Befehl](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE)**. Wenn Sie keinen **triftigen** Grund haben diese Eigenschaft zu bearbeiten, sollte der Standardwert belassen werden.

---

### `Headless`

`bool` Typ mit einem Standardwert von `false`. Diese Eigenschaft legt fest, ob der Prozess im Headless-Modus laufen soll. Im Headless-Modus geht ASF davon aus, dass es auf einem Server oder in einer anderen nicht interaktiven Umgebung läuft, daher wird es nicht versuchen Informationen über die Konsolen-Eingabe zu lesen. Dazu gehören On-Demand-Details (Konto-Anmeldeinformationen wie 2FA-Code, SteamGuard-Code, Passwort oder jede andere Variable, die für den Betrieb von ASF erforderlich ist) sowie alle anderen Konsolen-Eingaben (z. B. interaktive Befehlskonsole). Mit anderen Worten, der `Headless` Modus ist gleichbedeutend mit der schreibgeschützten ASF-Konsole. Diese Einstellung ist vor allem für Benutzer nützlich die ASF auf ihren Servern ausführen, da ASF, anstatt z. B. nach 2FA-Code zu fragen, den Vorgang stillschweigend abbricht, indem es ein Konto stoppt. Wenn du ASF nicht auf einem Server ausführst und vorher bestätigt hast, dass ASF im Nicht-Headless-Modus funktionieren kann, solltest du diese Eigenschaft deaktiviert lassen. Jegliche Benutzerinteraktion wird im Headless-Modus verweigert, und deine Konten werden nicht ausgeführt, wenn sie beim Start **irgendwelche** Konsolen-Eingaben erfordern. Dies ist für Server nützlich, da ASF den Versuch, sich am Konto anzumelden, wenn nach Anmeldeinformationen gefragt wird, abbrechen kann, anstatt (unendlich) darauf zu warten, dass der Benutzer diese bereitstellt. Wenn du diesen Modus aktivierst, kannst du auch den `input` **[Befehl](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE)** verwenden, der als Ersatz für die standardmäßige Konsolen-Eingabe dient. Wenn du dir nicht sicher bist wie du diese Eigenschaft einstellen sollst, belasse sie bei dem Standardwert `false`.

Wenn du ASF auf dem Server verwendest, solltest du diese Option zusammen mit dem `--process-required` **[command-line argument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments-de-DE)** verwenden.

---

### `IdleFarmingPeriod`

`byte` Typ mit einem Standardwert von `8`. Wenn ASF nichts zu sammeln hat, wird es regelmäßig alle `IdleFarmingPeriod` Stunden überprüfen, ob das Konto vielleicht einige neue Spiele zum Sammeln hat. Dieses Feature ist nicht erforderlich, wenn es sich um neue Spiele handelt, die wir erhalten, da ASF intelligent genug ist, um in diesem Fall automatisch die Abzeichen-Seiten zu überprüfen. `IdleFarmingPeriod` ist hauptsächlich für Situationen wie alte Spiele gedacht, bei denen wir bereits Karten hinzugefügt haben. In diesem Fall gibt es kein Ereignis, weshalb ASF regelmäßig die Abzeichen-Seiten überprüfen muss, wenn wir dies berücksichtigen wollen. Der Wert von `0` deaktiviert dieses Feature. Siehe auch: `ShutdownOnFarmingFinished`.

---

### `InventoryLimiterDelay`

`byte` Typ mit einem Standardwert von `4`. ASF will ensure that there will be at least `InventoryLimiterDelay` seconds in between of two consecutive inventory requests to avoid triggering rate-limit - those are being used for fetching Steam inventories, especially during your own commands such as `loot` or `transfer`. Der Standardwert von `4` wurde basierend auf dem Abrufen von Inventaren von über 100 aufeinanderfolgenden Bot-Instanzen festgelegt und sollte die meisten (wenn nicht alle) Benutzer zufrieden stellen. Du kannst es aber auch verringern oder sogar zu `0` wechseln, wenn du eine sehr geringe Anzahl von Bots hast, so dass ASF die Verzögerung ignoriert und die Steam-Inventare viel schneller plündert. Sei jedoch gewarnt, dass das Setzen eines zu niedrigen Wertes **dazu führt**, dass Steam deine IP vorübergehend blockiert, und das wird dich daran hindern, dein Inventar überhaupt abzurufen. Sie müssen diesen Wert möglicherweise auch erhöhen, wenn Sie viele Bots mit vielen Inventar-Anfragen ausführen, obwohl Sie in diesem Fall wahrscheinlich versuchen sollten, die Anzahl dieser Anfragen zu begrenzen. Wenn Sie keinen **triftigen** Grund haben diese Eigenschaft zu bearbeiten, sollte der Standardwert belassen werden.

---

### `IPC`

`bool` Typ mit Standardwert von `true`. Diese Eigenschaft definiert, ob der ASF-**[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-de-DE)**-Server zusammen mit dem Prozess gestartet werden soll. IPC allows for inter-process communication, including usage of **[ASF-ui](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#asf-ui)**, by booting a local HTTP server. If you do not intend to use any third-party IPC integration with ASF, including our ASF-ui, you can safely disable this option. Otherwise, it's a good idea to keep it enabled (default option).

---

### `IPCPassword`

`string` Typ mit einem Standardwert von `null`. Diese Eigenschaft definiert das obligatorische Passwort für jeden API Aufruf über die IPC-Schnittstelle und dient als zusätzliche Sicherheitsmaßnahme. Wenn auf einen nicht-leeren Wert gesetzt, benötigen alle IPC-Anfragen eine zusätzliche `password`-Eigenschaft, die auf das hier angegebene Passwort gesetzt ist. Der Standardwert von `null` überspringt die Notwendigkeit des Passwortes, so dass ASF alle Anfragen akzeptiert. Darüber hinaus ermöglicht die Aktivierung dieser Option auch den eingebauten IPC-Anti-Bruteforce-Mechanismus, der die gegebene `IPAdress` vorübergehend blockiert, nachdem zu viele nicht autorisierte Anfragen in sehr kurzer Zeit gesendet wurden. Wenn du keinen Grund hast diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

---

### `IPCPasswordFormat`

`byte` Typ mit einem Standardwert von `0`. Diese Eigenschaft legt das Format der Eigenschaft `IPCPassword` fest und nutzt `EHashingMethod` als zugrunde liegenden Typ. Bitte lesen Sie den Abschnitt **[Sicherheit](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security-de-DE#sicherheit)**, für mehr Informationen, um sicherzustellen, dass die Eigenschaft `IPCPassword` tatsächlich ein Passwort im passenden `IPCPasswordFormat` enthält. Mit anderen Worten, wenn Sie `IPCPasswordFormat` ändern, dann sollte das `IPCPassword` **bereits** im richtigen Format sein und nicht nur darauf abzielen, lediglich vorhanden zu sein. Wenn du nicht weißt was du tust, solltest du es bei dem Standardwert `0` belassen.

---

### `LicenseID`

`Guid?` type with default value of `null`. This property allows our **[sponsors](https://github.com/sponsors/JustArchi)** to enhance ASF with optional features that require paid resources to work. For now, this allows you to make use of **[`MatchActively`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/ItemsMatcherPlugin#matchactively)** feature in `ItemsMatcher` plugin.

While we recommend you to utilize GitHub since it offers monthly and one-time tiers, as well as allows full automation and gives you immediate access, we **also** support all other currently-available **[donation options](https://github.com/JustArchiNET/ArchiSteamFarm#archisteamfarm)**. See **[this post](https://github.com/JustArchiNET/ArchiSteamFarm/discussions/2780#discussioncomment-4486091)** for instructions on how to donate using other methods in order to get a manual license valid for given period.

Regardless of the method used, if you're ASF sponsor, you can obtain your license **[here](https://asf.justarchi.net/User/Status)**. You'll need to sign in with GitHub for confirming your identity, we ask only for read-only public information, which is your username. `LicenseID` is made out of 32 hexadecimal characters, such as `f6a0529813f74d119982eb4fe43a9a24`.

**Ensure that you do not share your `LicenseID` with other people**. Since it's issued on personal basis, it might get revoked if it's leaked. If by any chance this happened to you accidentally, you can generate a new one from the same place.

Unless you want to enable extra ASF functionalities, there is no need for you to provide the license.

---

### `LoginLimiterDelay`

`byte` Typ mit einem Standardwert von `10`. ASF stellt sicher, dass zwischen zwei aufeinanderfolgenden Verbindungsversuchen mindestens `LoginLimiterDelay` Sekunden liegen, um eine Auslösung des Anfrage-Limits zu vermeiden. Der Standardwert von `10` wurde basierend auf der Verbindung von über 100 Bot-Instanzen festgelegt und sollte die meisten (wenn nicht alle) Benutzer zufrieden stellen. Sie können es aber auch verringern oder sogar zu `0` wechseln, wenn eine sehr geringe Anzahl von Bots eingerichtet sind, sodass ASF die Verzögerung ignoriert und sich viel schneller mit Steam verbindet. Aber seien Sie gewarnt, da das Setzen einer zu niedrigen Einstellung **garantiert** dazu führt, dass Steam Ihre IP vorübergehend sperrt, während zu viele Bots gleichzeitig benutzt werden. Das wird in einem `InvalidPassword/RateLimitExceeded`-Fehler resultieren und Sie **komplett** daran hindern, sich anzumelden. Das betrifft dann auch den normalen Steam-Client, nicht nur ASF. Gleichermaßen gilt, wenn Sie eine übermäßige Anzahl von Bots verwenden, insbesondere zusammen mit anderen Steam-Clients/Programmen, die die gleiche IP-Adresse verwenden, muss dieser Wert höchstwahrscheinlich erhöht werden, um die Anmeldungen über einen längeren Zeitraum verteilen zu können.

Nebenbei bemerkt, wird dieser Wert auch als Load-Balancing-Puffer in allen ASF-geplanten Aktionen verwendet, wie z. B. Handelsangebote in `SendTradePeriod`. Wenn Sie keinen **triftigen** Grund haben diese Eigenschaft zu bearbeiten, sollte der Standardwert belassen werden.

---

### `MaxFarmingTime`

`byte` Typ mit einem Standardwert von `10`. As you should know, Steam is not always working properly, sometimes weird situations can happen such as our playtime not being recorded, despite of, in fact, playing a game. ASF erlaubt es, ein einzelnes Spiel im Einzelmodus für maximal `MaxFarmingTime` Stunden zu betreiben und betrachtet es nach diesem Zeitraum als vollständig gesammelt. Dies ist erforderlich, um den Sammel-Prozess nicht einzufrieren, wenn es zu seltsamen Situationen kommt, aber auch, wenn Steam aus irgenIhrem Grund ein neues Abzeichen veröffentlicht hat, das ASF daran hindern würde, weiter voranzukommen (siehe: `Blacklist`). Der Standardwert von `10` Stunden sollte ausreichen, um alle Steam-Karten aus einem Spiel zu sammeln. Wenn Sie diese Eigenschaft zu niedrig einstellen, kann dies dazu führen, dass gültige Spiele übersprungen werden (und ja, es gibt gültige Spiele, die sogar bis zu 9 Stunden zum Sammeln benötigen), während eine zu hohe Einstellung dazu führen kann, dass der Sammel-Prozess eingefroren wird. Bitte beachten Sie, dass diese Eigenschaft nur ein einzelnes Spiel in einer einzigen Sammel-Sitzung betrifft (sodass ASF nach dem Durchlaufen der gesamten Warteschlange zu diesem Titel zurückkehrt), auch basiert sie nicht auf der Gesamtspielzeit, sondern auf der internen ASF-Sammel-Zeit, sodass ASF auch nach einem Neustart zu diesem Titel zurückkehrt. Wenn Sie keinen **triftigen** Grund haben diese Eigenschaft zu bearbeiten, sollte der Standardwert belassen werden.

---

### `MaxTradeHoldDuration`

`byte` Typ mit einem Standardwert von `15`. Diese Eigenschaft definiert die maximale Dauer der Handelssperre in Tagen, die wir bereit sind zu akzeptieren - ASF lehnt Handelsangebote ab, die länger als `MaxTradeHoldDuration` Tage gehalten werden, wie im **[Handels](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading-de-DE#Handel)**-Abschnitt definiert. Diese Option ist nur für Bots mit `TradingPreferences` von `SteamTradeMatcher` sinnvoll, da sie `Master` / `SteamOwnerID` Handelsangebote und keine Spenden betrifft. Handelssperren sind für alle ärgerlich, und niemand will sich wirklich mit ihnen befassen. ASF soll nach liberalen Regeln arbeiten und jedem helfen, egal ob er sich in einer Handelssperre befindet oder nicht. Deshalb ist diese Option standardmäßig auf `15` gesetzt. Wenn Sie stattdessen lieber alle von Handelssperren betroffene Handelsangebote ablehnen möchten, können Sie hier `0` angeben. Bitte bedenken Sie, dass Karten mit kurzer Lebensdauer von dieser Option nicht betroffen sind und für Personen mit Handelssperren automatisch abgelehnt werden, wie im Abschnitt **[Handel](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading-de-DE#Handel)** beschrieben, sodass es nicht notwendig ist, alle nur deshalb global abzulehnen. Wenn Sie keinen Grund haben diese Eigenschaft zu bearbeiten, solltest der Standardwert belassen werden.


---

### `MinFarmingDelayAfterBlock`

`byte` Typ mit Standardwert von `60`. This property defines minimum amount of time, in seconds, which ASF will wait before resuming cards farming if it got previously disconnected with `LoggedInElsewhere`, which happens when you decide to forcefully disconnect currently-farming ASF by launching a game. This delay exists mainly for convenience and overhead reasons, for example it allows you to restart the game without having to fight with ASF occupying your account again only because playing lock was available for a split second. Due to the fact that reclaiming the session causes `LoggedInElsewhere` disconnect, ASF has to go through whole reconnect procedure, which puts additional pressure on the machine and Steam network, therefore avoiding additional disconnects, if possible, is preferable. By default, this is configured at `60` seconds, which should be enough to allow you restart the game without much hassle. However, there are scenarios when you could be interested in increasing it, for example if your network disconnects often and ASF is taking over too soon, which causes being forced to go through the reconnect process yourself. We allow a maximum value of `255` for this property, which should be enough for all common scenarios. In addition to the above, it's also possible to decrease the delay, or even remove it entirely with a value of `0`, although that is usually not recommended due to reasons stated above. Wenn Sie keinen Grund haben diese Eigenschaft zu bearbeiten, solltest der Standardwert belassen werden.

---

### `OptimizationMode`

`byte` Typ mit einem Standardwert von `0`. Diese Eigenschaft definiert den Optimierungsmodus, den ASF während der Laufzeit bevorzugt. Derzeit unterstützt ASF zwei Modi - `0`, der so genannte `MaxPerformance`, und `1`, der so genannte `MinMemoryUsage`. Standardmäßig bevorzugt ASF es, so viele Dinge wie möglich parallel (gleichzeitig) auszuführen, was die Leistung durch Load-Balancing-Arbeiten über alle CPU-Kerne, mehrere CPU-Threads, mehrere Sockel und mehrere Thread-Pool-Aufgaben hinweg erhöht. For example, ASF will ask for your first badge page when checking for games to farm, and then once request arrived, ASF will read from it how many badge pages you actually have, then request each other one concurrently. Das ist es, was Sie eigentlich **fast immer** möchten, da der Aufwand in den meisten Fällen minimal ist und die Vorteile des asynchronen ASF-Codes auch auf der ältesten Hardware mit einem einzigen CPU-Kern und stark eingeschränkter Leistung sichtbar sind. Da jedoch viele Aufgaben parallel verarbeitet werden, ist die ASF-Laufzeit für ihre Wartung verantwortlich, z. B. Sockets offen zu halten, Threads am Leben zu erhalten und Aufgaben zu bearbeiten, was von Zeit zu Zeit zu einer erhöhten Speicherauslastung führen kann, und wenn Sie durch den verfügbaren Speicher extrem eingeschränkt sind, sollten Sie diese Eigenschaft auf `1` (`MinMemoryUsage`) umschalten, um ASF zu zwingen, so wenig Aufgaben wie möglich zu verwenden und typischerweise possible-to-parallel asynchronen Code synchron zu verwenden. Sie sollten erwägen, diese Eigenschaft nur zu ändern, wenn Sie vorher **[Speichereffizientes Setup](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup-de-DE#speichereffizientes-setup)** gelesen haben und absichtlich gigantische Leistungssteigerung opfern möchten, für eine sehr kleine Verringerung des Speicheraufwands. Normalerweise ist diese Option **viel schlechter** als das, was mit anderen möglichen Methoden erreichen ist, z. B. indem Sie eine ASF-Nutzung einschränken oder den Garbage Collector der Laufzeit abstimmen, wie in **[Speichereffizientes Setup](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup-de-DE#speichereffizientes-setup)** erklärt. Daher sollte `MinMemoryUsage` als **letzter Schritt** verwendet werden, kurz vor der Neukompilierung der Laufzeit, wenn mit anderweitigen (viel besseren) Optionen keine zufriedenstellende Ergebnisse erzielt werden können. Wenn Sie keinen **triftigen** Grund haben diese Eigenschaft zu bearbeiten, sollte der Standardwert belassen werden.

---

### `SteamMessagePrefix`

`string` Typ mit einem Standardwert von `"/me "`. Diese Eigenschaft definiert ein Präfix, das allen Steam-Nachrichten, die von ASF gesendet werden, vorangestellt wird. Standardmäßig verwendet ASF das Präfix `"/me "`, um Bot-Nachrichten leichter zu unterscheiden, da sie im Steam-Chat in verschiedenen Farben angezeigt werden. Eine weitere erwähnenswerte Eigenschaft ist das Präfix `"/pre "`, das ein ähnliches Ergebnis erzielt, aber eine andere Formatierung verwendet. Sie können diese Eigenschaft auch auf eine leere Zeichenkette oder `null` setzen, um die Verwendung des Präfixes vollständig zu deaktivieren und alle ASF-Nachrichten auf traditionelle Weise auszugeben. Es ist gut zu wissen, dass diese Eigenschaft nur Steam-Nachrichten betrifft - Antworten, die über andere Kanäle (z. B. IPC) zurückgegeben werden, sind nicht betroffen. Wenn Sie das standardmäßige ASF-Verhalten nicht anpassen möchten, ist es eine gute Idee, es bei den Standardeinstellungen zu belassen.

---

### `SteamOwnerID`

`ulong` Typ mit einem Standardwert von `0`. Diese Eigenschaft definiert die Steam-ID in 64-Bit-Form des ASF-Prozessinhabers und ist sehr ähnlich der `Master` Berechtigung der gegebenen Bot-Instanz (aber stattdessen global). Sie möchten diese Eigenschaft fast immer auf die ID des eigenen Steam-Haupt-Kontos setzen. Die `Master`-Berechtigung beinhaltet die volle Kontrolle über seine Bot-Instanz, aber globale Befehle wie `exit`, `restart` oder `update` sind nur für `SteamOwnerID` verfügbar. Dies ist praktisch, da Sie vielleicht Bots für Freunde ausführen möchten, ohne ihnen zu erlauben, den ASF-Prozess zu steuern, wie z. B. das Beenden über den Befehl `exit`. Der Standardwert von `0` gibt an, dass es keinen Besitzer des ASF-Prozesses gibt, was bedeutet, dass niemand in der Lage sein wird, globale ASF-Befehle auszuführen. Keep in mind that this property applies to Steam chat exclusively, **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)**, as well as interactive console, will still allow you to execute `Owner` commands even if this property is not set.

---

### `SteamProtocols`

`byte flags` Typ mit einem Standardwert von `7`. Diese Eigenschaft definiert Steam-Protokolle welche ASF beim Verbinden mit Steam-Servern verwendet. Sie sind wie folgt definiert:

| Wert | Name      | Beschreibung                                                                                     |
| ---- | --------- | ------------------------------------------------------------------------------------------------ |
| 0    | None      | Kein Protokoll                                                                                   |
| 1    | TCP       | **[Transmission Control Protocol](https://en.wikipedia.org/wiki/Transmission_Control_Protocol)** |
| 2    | UDP       | **[User Datagram Protocol](https://en.wikipedia.org/wiki/User_Datagram_Protocol)**               |
| 4    | WebSocket | **[WebSocket](https://en.wikipedia.org/wiki/WebSocket)**                                         |

Bitte bedenken Sie, dass diese Eigenschaft das Feld `flags` ist, daher ist es möglich, eine beliebige Kombination von verfügbaren Werten auszuwählen. Mehr Informationen finden Sie unter **[JSON-Mapping](#json-mapping)**. Das Nicht-Aktivieren eines der Flags führt zur Option `None`, und diese Option ist an sich schon ungültig.

Standardmäßig verwendet ASF alle verfügbaren Steam-Protokolle als Maßnahme für den Kampf gegen Ausfallzeiten und ähnliche Steam-Probleme. Normalerweise möchten Sie diese Eigenschaft ändern, wenn Sie ASF darauf beschränken möchten, nur ein oder zwei bestimmte Protokolle anstelle aller verfügbaren zu verwenden. Eine solche Maßnahme könnte erforderlich sein, wenn Sie z. B. nur TCP-Datenverkehr auf einer Firewall aktivieren, und verhindern möchten, dass ASF versucht, eine Verbindung über UDP herzustellen. Wenn Sie jedoch kein bestimmtes Problem debuggen, ist es aber immer in Ihrem Sinne, dass ASF jedes Protokoll verwenden kann, das derzeit unterstützt wird, anstelle von nur ein oder zwei. Wenn Sie keinen **triftigen** Grund haben diese Eigenschaft zu bearbeiten, sollte der Standardwert belassen werden.

---

### `UpdateChannel`

`byte` Typ mit einem Standardwert von `1`. Diese Eigenschaft definiert den Aktualisierungskanal, der entweder für automatische Aktualisierungen verwendet wird (wenn `UpdatePeriod` größer als `0` ist) oder für Aktualisierungsbenachrichtigungen (anderweitig). Derzeit unterstützt ASF drei Aktualisierungskanäle - `0`, welcher `None` genannt wird, `1`, welcher `Stable` genannt wird, und `2`, welcher `Experimental` genannt wird. Der Kanal `Stable` ist der standardmäßige Veröffentlichungskanal, der von der Mehrheit der Benutzer verwendet werden sollte. Der Kanal `experimental` beinhaltet zusätzlich zu `Stable` Veröffentlichungen, auch **Vorveröffentlichungen** für fortgeschrittene Benutzer und andere Entwickler, um neue Funktionen zu testen, fehlerbehebungen zu bestätigen oder Rückmeldungen über geplante Verbesserungen abzugeben. **Experimentelle Versionen enthalten oft unbehobene Programmfehler, Work-in-Progress-Funktionen oder neu geschriebene Implementierungen**. Wenn Sie sich nicht als fortgeschrittener Benutzer betrachten, bleiben Sie bitte beim Standard-Aktualisierungskanal `1` (Stable). Der Kanal `experimental` ist für Benutzer gedacht, die wissen, wie man Fehler meldet, Probleme löst und Rückmeldung gibt - es wird keine technische Unterstützung geboten. Sehen Sie sich den **[Veröffentlichungszyklus](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle-de-DE#veröffentlichungszyklus)** von ASF an, um mehr darüber zu erfahren. Sie können auch den`UpdateChannel` auf `0` (`None`) setzen, wenn alle Versionsüberprüfungen vollständig deaktivieren werden sollen. Falls `UpdateChannel` auf `0` gesetzt ist, wird die gesamte Funktionalität im Zusammenhang mit Aktualisierungen vollständig deaktiviert, einschließlich des Befehls `update`. Es wird **ausdrücklich** von der Verwendung des `None`-Kanals **abgeraten**, weil Sie sich dadurch allen möglichen Problemen aussetzt (erwähnt in [`UpdatePeriod`](#UpdatePeriod) Beschreibung unten).

**Wenn Sie nicht wissen, was Sie tun**, empfehlen wir **ausdrücklich** es bei den Standardeinstellungen zu belassen.

---

### `UpdatePeriod`

`byte` Typ mit einem Standardwert von `24`. Diese Eigenschaft legt fest, wie oft ASF nach automatischen Aktualisierungen suchen soll. Aktualisierungen sind nicht nur für den Erhalt neuer Funktionen und kritischer Sicherheitspatches entscheidend, sondern auch für den Erhalt von Fehlerbehebungen, Leistungssteigerungen, Stabilitätsverbesserungen und mehr. Wenn ein Wert größer als `0` eingestellt ist, lädt ASF sich automatisch herunter, ersetzt und startet sich neu (wenn `AutoRestart` es erlaubt), sobald eine neue Aktualisierung verfügbar ist. Um dies zu erreichen, wird ASF alle `UpdatePeriod` Stunden überprüfen, ob eine neue Aktualisierung in unserem GitHub Repository verfügbar ist. Ein Wert von `0` deaktiviert automatische Aktualisierungen, ermöglicht es aber dennoch, den Befehl `update` manuell auszuführen. Sie könnten auch daran interessiert sein, den entsprechenden `UpdateChannel` einzustellen, dem `UpdatePeriod` folgen sollte.

Der Aktualisierungsprozess von ASF beinhaltet die Aktualisierung der gesamten Ordnerstruktur, die ASF verwendet, mit Aunsnahme der eigenen Konfigurationen oder Datenbanken im Verzeichnis `config` - das bedeutet, dass alle zusätzlichen Dateien, die nicht mit ASF in seinem Verzeichnis zusammenhängen, während des Aktualisierungsvorgangs verloren gehen können. Der Standardwert von `24` ist ein gutes Verhältnis zwischen unnötigen Prüfungen und ASF, das aktuell genug ist.

Wenn du keinen **triftigen** Grund hast, diese Funktion zu deaktivieren, solltest du die automatischen Aktualisierungen innerhalb einer angemessenen Zeitspanne von `UpdatePeriod` **für dein eigenes Wohl aktivieren**. Dies liegt nicht nur daran, dass wir ausschließlich die neueste stabile ASF-Version unterstützen, sondern auch, weil **wir unsere Sicherheitsgarantie nur für die neueste Version** geben. Wenn Sie eine veraltete ASF-Version verwenden, dann setzten Sie sich absichtlich allen möglichen Problemen aus, von kleinen Fehlern über defekte Funktionen bis hin zu **[permanenten Steam-Konto Sperren](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ-de-DE#wurde-jemand-daf%C3%BCr-gesperrt)**, also empfehlen wir **dringend**, um zu Ihrem eigenen Nutzen immer sicherzustellen, dass die ASF-Version auf dem neuesten Stand ist. Automatische Aktualisierungen ermöglichen es uns, schnell auf alle Arten von Problemen zu reagieren, indem wir problematischen Quelltext deaktivieren oder beheben, bevor er eskalieren kann - wenn Sie sich dagegen entscheiden, verlieren Sie sämtliche Sicherheitsgarantien und riskieren Risikofolgen durch das Ausführen von Code, der möglicherweise schädlich sein könnte (nicht nur für das Steam-Netzwerk, sondern auch -per Definition- für das eigene Steam-Konto).

---

### `WebLimiterDelay`

`ushort` Typ mit einem Standardwert von `300`. Diese Eigenschaft definiert in Millisekunden die minimale Verzögerung zwischen dem Senden von zwei aufeinanderfolgenden Anfragen an denselben Steam-Webservice. Eine solche Verzögerung ist erforderlich, da der Dienst **[AkamaiGhost](https://www.akamai.com/de/de)**, den Steam intern nutzt, eine Geschwindigkeitsbegrenzung basierend auf der globalen Anzahl der über einen bestimmten Zeitraum gesendeten Anfragen beinhaltet. Unter normalen Umständen ist eine Blockierung des Akamai-Services ziemlich schwer zu erreichen, aber unter sehr hohen Arbeitslasten mit einer riesigen laufenden Warteschlange von Anfragen ist es möglich, ihn auszulösen, wenn wir immer wieder zu viele Anfragen über einen zu kurzen Zeitraum senden.

Der Standardwert wurde unter der Annahme festgelegt, dass ASF das einzige Programm ist, das auf die Steam-Webdienste zugreift, insbesondere `steamcommunity.com`, `api.steampowered.com` und `store.steampowered.com`. Wenn Sie andere Programme verwendest, die Anfragen an dieselben Webservices senden, dann solltest du sicherstellen, dass das Programm ähnliche Funktionen wie `WebLimiterDelay` enthält und beide auf das Doppelte des Standardwerts setzen, was `600` wäre. Dies garantiert, dass unter keinen Umständen mehr als 1 Anfrage pro `300` ms gesendet wird.

Im Allgemeinen wird das Herabsetzen von `WebLimiterDelay` unter den Standardwert **stark abgeraten**, da es zu verschiedenen IP-bezogenen Sperren führen kann, von denen einige dauerhaft sein können. Der Standardwert ist gut genug, um eine einzelne ASF-Instanz auf dem Server auszuführen und ASF im Normalfall zusammen mit dem ursprünglichen Steam-Client zu verwenden. Es sollte für die meisten Anwendungen zutreffend sein, und Sie sollten es nur erhöhen (nie senken), wenn Sie- abgesehen von der Verwendung von ASF - auch ein anderes Programm verwenden, das eine übermäßige Anzahl von Anfragen an dieselben Webdienste senden könnte, die ASF nutzt. Kurz gesagt, die globale Anzahl aller Anfragen, die von einer einzelnen IP an eine einzelne Steam-Domäne gesendet werden, sollte nie 1 Anfrage pro `300` ms überschreiten.

Wenn Sie keinen Grund haben diese Eigenschaft zu bearbeiten, sollten Sie den Standardwert belassen.

---

### `WebProxy`

`string` Typ mit einem Standardwert von `null`. Diese Eigenschaft definiert eine Web-Proxy-Adresse, die für alle internen http- und https-Anfragen verwendet wird, die von ASFs `HttpClient` gesendet werden, insbesondere für Dienste wie `github.com`, `steamcommunity.com` und `store.steampowered.com`. Das Proxying von ASF-Anfragen im Allgemeinen hat keine Vorteile, aber es ist äußerst nützlich, um verschiedene Arten von Firewalls zu umgehen, insbesondere die große Firewall von China.

Diese Eigenschaft ist als uri-Zeichenfolge definiert:

> A URI string is composed of a scheme (supported: http/https/socks4/socks4a/socks5), a host, and an optional port. Ein Beispiel für eine komplette uri-Zeichenkette wäre `"http://contoso.com:8080"`.

Wenn ein Proxy eine Benutzer-Authentifizierung erfordert, musst auch `WebProxyUsername` und/oder `WebProxyPassword` eingerichtet sein. Wenn es keinen solchen Bedarf gibt, genügt die Einrichtung dieser Eigenschaft allein.

Im Moment verwendet ASF den Web-Proxy nur für `http` und `https` Anfragen, was **nicht** die interne Steam-Netzwerk-Kommunikation innerhalb des internen Steam-Clients von ASF beinhaltet. Es gibt derzeit keine Pläne dies zu unterstützen, hauptsächlich wegen der fehlenden **[SK2](https://github.com/SteamRE/SteamKit/issues/587#issuecomment-413271550)**-Funktionalität. Wenn Sie dies benötigen/wollen, schlagen wir vor, da anzufangen.

Wenn Sie keinen Grund haben diese Eigenschaft zu bearbeiten, solltest der Standardwert belassen werden.

---

### `WebProxyPassword`

`string` Typ mit einem Standardwert von `null`. Diese Eigenschaft definiert das Feld für das Passwort, das bei der Basis-, Digest-, NTLM- und Kerberos-Authentifizierung verwendet wird und von einem Ziel `WebProxy`-Rechner mit Proxy-Funktionalität unterstützt wird. Wenn der Proxy keine Benutzer-Anmeldeinformationen benötigt, ist es nicht notwendig, hier etwas einzutragen. Die Verwendung dieser Option ist nur sinnvoll, wenn auch `WebProxy` verwendet wird, da sie sonst keine Wirkung hat.

Wenn Sie keinen Grund haben diese Eigenschaft zu bearbeiten, solltest der Standardwert belassen werden.

---

### `WebProxyUsername`

`string` Typ mit einem Standardwert von `null`. Diese Eigenschaft definiert das Feld für den Benutzernamen, das bei der Basis-, Digest-, NTLM- und Kerberos-Authentifizierung verwendet wird und von einem `WebProxy`-Rechner (Ziel) mit Proxy-Funktionalität unterstützt wird. Wenn der Proxy keine Benutzer-Anmeldeinformationen benötigt, ist es nicht notwendig, hier etwas einzutragen. Die Verwendung dieser Option ist nur sinnvoll, wenn auch `WebProxy` verwendet wird, da sie sonst keine Wirkung hat.

Wenn Sie keinen Grund haben diese Eigenschaft zu bearbeiten, solltest der Standardwert belassen werden.

---

## Bot Konfiguration

Wie du bereits weißt, sollte jeder Bot eine eigene Konfiguration haben, die auf der folgenden beispielhaften JSON-Struktur basiert. Beginne damit, zu entscheiden, wie du deinen Bot benennen möchtest (z.B. `1.json`, `main.json`, `haupt.json` oder `IrgendwasAnderes.json`) und gehe zur Konfiguration über.

**Hinweis:** Der Bot kann den Namen `ASF` nicht haben (da dieses Schlüsselwort für die globale Konfiguration reserviert ist), ASF ignoriert auch alle Konfigurationsdateien, die mit einem Punkt beginnen.

Die Bot-Konfiguration hat folgende Struktur:

```json
{
    "AcceptGifts": false,
    "AutoSteamSaleEvent": false,
    "BotBehaviour": 0,
    "CompleteTypesToSend": [],
    "CustomGamePlayedWhileFarming": null,
    "CustomGamePlayedWhileIdle": null,
    "Enabled": false,
    "FarmingOrders": [],
    "FarmPriorityQueueOnly": false,
    "GamesPlayedWhileIdle": [],
    "HoursUntilCardDrops": 3,
    "LootableTypes": [1, 3, 5],
    "MatchableTypes": [5],
    "OnlineFlags": 0,
    "OnlineStatus": 1,
    "PasswordFormat": 0,
    "Paused": false,
    "RedeemingPreferences": 0,
    "RemoteCommunication": 3,
    "SendOnFarmingFinished": false,
    "SendTradePeriod": 0,
    "ShutdownOnFarmingFinished": false,
    "SkipRefundableGames": false,
    "SteamLogin": null,
    "SteamMasterClanID": 0,
    "SteamParentalCode": null,
    "SteamPassword": null,
    "SteamTradeToken": null,
    "SteamUserPermissions": {},
    "TradeCheckPeriod": 60,
    "TradingPreferences": 0,
    "TransferableTypes": [1, 3, 5],
    "UseLoginKeys": true,
    "UserInterfaceMode": 0
}
```

---

Alle Optionen werden nachfolgend erklärt:

### `AcceptGifts`

`bool` Typ mit einem Standardwert von `false`. Wenn aktiviert, akzeptiert und löst ASF automatisch alle Steam-Geschenke (einschließlich Guthaben-Geschenkgutscheine), die an den Bot geschickt werden. Dazu gehören auch Geschenke, die von anderen Benutzern als denjenigen gesendet werden, die in `SteamUserPermissions` definiert sind. Bedenke, dass Geschenke, die an die E-Mail-Adresse geschickt werden, nicht direkt an den Client weitergeleitet werden, so dass ASF diese ohne deine Hilfe nicht annehmen wird.

Diese Option wird nur für Alternativkonten empfohlen, da es sehr wahrscheinlich ist, dass du nicht automatisch alle Geschenke einlösen möchtest, die an dein Hauptkonto gesendet werden. Wenn du dir nicht sicher bist ob du diese Funktion aktivieren möchtest oder nicht, behalte sie mit dem Standardwert `false`.

---

### `AutoSteamSaleEvent`

`bool` Typ mit einem Standardwert von `false`. Während der Steam Sommer/Winter-Verkaufsveranstaltungen ist Steam dafür bekannt, dass es Nutzern zusätzliche Karten zur Verfügung stellt, um jeden Tag die Entdeckungsliste zu durchsuchen, sowie durch andere ereignisspezifische Aktivitäten. Wenn diese Option aktiviert ist, überprüft ASF automatisch die Steam Entdeckungsliste alle `8` Stunden (beginnend bei einer Stunde seit Programmstart) und durchläuft sie bei Bedarf. Diese Option wird nicht empfohlen, wenn Sie diese Aktion selbst durchführen möchten, und normalerweise ist dies nur bei Bot-Konten sinnvoll. Außerdem muss das Konto den Steam-Anforderung entsprechend mindestens Level `8` haben, um diese Karten überhaupt zu erhalten. Wenn du dir nicht sicher bist ob du diese Funktion aktivieren möchtest oder nicht, behalte sie mit dem Standardwert `false`.

Bitte bedenke, dass wir aufgrund von ständigen Steam-Problemen, Änderungen und Problemen **keine Garantie geben, ob diese Funktion ordnungsgemäß funktioniert**, daher ist es durchaus möglich, dass diese Option **überhaupt nicht funktioniert**. Wir akzeptieren **keine** Fehlermeldungen, auch keine Unterstützungsanfragen für diese Option. Es wird ohne jegliche Garantie angeboten, die Nutzung erfolgt auf eigene Gefahr.

---

### `BotBehaviour`

`byte flags` Typ mit Standardwert von `0`. Diese Eigenschaft definiert das ASF-Bot-ähnliche Verhalten bei verschiedenen Ereignissen und ist wie folgt definiert:

| Wert | Name                          | Beschreibung                                                                                                          |
| ---- | ----------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| 0    | None                          | Kein spezielles Bot-Verhalten, der am wenigsten invasive Modus, (Standard)                                            |
| 1    | RejectInvalidFriendInvites    | Führt dazu, dass ASF ungültige Freundschaftseinladungen ablehnt (anstatt sie zu ignorieren)                           |
| 2    | RejectInvalidTrades           | Wird ASF dazu veranlassen, ungültige Handelsangebote abzulehnen (anstatt sie zu ignorieren)                           |
| 4    | RejectInvalidGroupInvites     | Führt dazu, dass ASF ungültige Gruppeneinladungen ablehnt (anstatt sie zu ignorieren)                                 |
| 8    | DismissInventoryNotifications | Veranlasst ASF dazu, alle Inventar-Benachrichtigungen automatisch zu entfernen                                        |
| 16   | MarkReceivedMessagesAsRead    | Führt dazu, dass ASF automatisch alle empfangenen Nachrichten als gelesen markiert                                    |
| 32   | MarkBotMessagesAsRead         | Bewirkt, dass ASF Nachrichten von anderen ASF-Bots automatisch als gelesen markiert (ausgeführt in derselben Instanz) |

Bitte bedenken Sie, dass diese Eigenschaft das Feld `flags` ist, daher ist es möglich, eine beliebige Kombination von verfügbaren Werten auszuwählen. Mehr Informationen finden Sie unter **[JSON-Mapping](#json-mapping)**. Wenn keines der Flags aktiviert wird, wird die Option `None` verwendet.

Im Allgemeinen möchtest du diese Eigenschaft ändern, wenn du von ASF erwartest, dass er einen bestimmten Grad an Automatisierung im Zusammenhang mit seiner Aktivität durchführt, wie es von einem Bot-Konto erwartet wird, aber nicht von einem primären Konto, das in ASF verwendet wird. Daher ist die Änderung dieser Eigenschaft vor allem für Alternativ-Konten sinnvoll, obwohl du ausgewählte Optionen auch für Hauptkonten verwenden kannst.

Normales (`None`) ASF-Verhalten ist es, nur Dinge zu automatisieren, die der Benutzer wünscht (z.B. Karten sammeln oder `SteamTradeMatcher` Angebote, wenn in `TradingPreferences` eingestellt). Dies ist der am wenigsten eingreifende Modus, und es ist für die Mehrheit der Benutzer von Vorteil, da du die volle Kontrolle über dein Konto hast und du selbst entscheiden kannst, ob du bestimmte Interaktionen außerhalb des Anwendungsbereichs zulässt oder nicht.

Eine ungültige Freundschaftseinladung ist eine Einladung, die nicht vom Benutzer mit `FamilySharing` Berechtigung (definiert in `SteamUserPermissions`) oder höher kommt. ASF ignoriert diese Einladungen im normalen Modus, wie du es erwarten würdest, und gibt dir die freie Wahl, ob du sie annehmen möchtest oder nicht. `RejectInvalidFriendInvites` führt dazu, dass diese Einladungen automatisch abgelehnt werden, was die Option für andere Personen, dich zu ihrer Freundesliste hinzuzufügen, praktisch deaktiviert (da ASF alle diese Anfragen ablehnt, mit Ausnahme der in `SteamUserPermissions` definierten Personen). Wenn du nicht alle Freundschaftseinladungen direkt ablehnen willst, solltest du diese Option nicht aktivieren.

Ein ungültiges Handelsangebot ist ein Angebot, das nicht durch das eingebaute ASF-Modul angenommen wird. Mehr zu diesem Thema findest du im Abschnitt **[Handel](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading-de-DE)**, der explizit definiert, welche Arten von Handelsangeboten ASF bereit ist, automatisch zu akzeptieren. Gültige Handelsangebote werden auch durch andere Einstellungen definiert, insbesondere `TradingPreferences`. `RejectInvalidTrades` bewirkt, dass alle ungültigen Handelsangebote abgelehnt, anstatt ignoriert zu werden. Wenn du nicht alle Handelsangebote, die nicht automatisch von ASF angenommen werden, endgültig ablehnen willst, solltest du diese Option nicht aktivieren.

Eine ungültige Gruppeneinladung ist eine Einladung, die nicht aus der Gruppe `SteamMasterClanID` stammt. ASF ignoriert im normalen Modus diese Gruppeneinladungen, wie du es erwarten würdest, und erlaubt es dir, selbst zu entscheiden, ob du einer bestimmten Steam-Gruppe beitreten möchtest oder nicht. `RejectInvalidGroupInvites` führt dazu, dass alle diese Gruppeneinladungen automatisch abgelehnt werden, was es praktisch unmöglich macht, dich in irgendeine andere Gruppe als `SteamMasterClanID` einzuladen. Wenn du nicht alle Gruppeneinladungen direkt ablehnen möchtest, solltest du diese Option nicht aktivieren.

`DismissInventoryNotifications` ist äußerst nützlich, wenn Sie sich an den Steam-Benachrichtigung über den Erhalt neuer Gegenstände stören. ASF kann die Benachrichtigung ans sich nicht selbst löschen, da diese im Steam-Client integriert ist, aber es ist in der Lage, die Benachrichtigung nach Erhalt automatisch zu löschen, was dazu führt, dass keine Benachrichtigung über "neue Gegenstände im Inventar" mehr auftritt. If you expect to evaluate yourself all received items (especially cards farmed with ASF), then naturally you shouldn't enable this option. Beachten Sie, falls Sie sich nicht sicher sind, ob Sie diese Einstellung aktivieren sollen, dass diese Einstellung optional ist.

`MarkReceivedMessagesAsRead` markiert automatisch **alle** Nachrichten, die von dem Konto empfangen werden (egal ob von Gruppen oder Privat), auf dem ASF läuft. Dies sollte typischerweise nur von alternativen Konten verwendet werden, um Benachrichtigungen über "neue Nachrichten" zu löschen, die z. B. von Ihnen während der Ausführung von ASF-Befehlen erscheinen. Wir empfehlen diese Option nicht für Hauptkonten, es sei denn, Sie möchten sich von jeder Art von Benachrichtigungen über neue Nachrichten befreien, **einschließlich** derjenigen, die während des Offline-Betriebs erschienen sind, vorausgesetzt, dass ASF immer noch offen gelassen wurde, als es sie ablehnte.

`MarkBotMessagesAsRead` funktioniert in ähnlicher Weise, nur werden hier lediglich Bot-Nachrichten als gelesen markiert. Beachten Sie, dass die Steam-Implementierung ** sowohl beim verwenden dieser Option** Gruppen-Chats mit den Bots und anderen Leuten bestätigt, **als auch alle vorherigen Nachrichten**. Wenn Sie also aus irgenIhrem Grund keine unzusammenhängende Nachricht verpassen wollen, sollten Sie diese Funktion in der Regel vermeiden. Natürlich ist es ebenfalls riskant, wenn mehrere Primärkonten (z. B. von unterschiedlichen Nutzern) in der Selben ASF-Instanz laufen, besonders weil Sie die normalen out-of-ASF Nachrichten (Steam-Benachrichtigungen) verpassen könnten.

Wenn Sie sich nicht sicher sind, wie Sie diese Option konfigurieren, können Sie den Standardwert belassen.

---

### `CompleteTypesToSend`

`ImmutableHashSet<byte>` Typ mit einem leeren Standardwert. Wenn ASF mit dem vervollständigen eines (hier zuvor) bestimmten Sets von Gegenstandsarten fertig ist, kann dies automatisch mit allen fertigen Sets an den Benutzer mit `Master`-Berechtigung senden. Das ist sehr praktisch, wenn Sie das angegebene Bot-Konto beispielsweise für einen STM-Abgleich verwenden möchten, während fertige Sets auf ein anderes Konto verschoben werden. Diese Option funktioniert genauso wie der `loot`-Befehl. Beachten Sie deshalb, dass Sie einen Benutzer mit der Berechtigung `Master` benötigen; vielleicht benötigen Sie außerdem einen gültigen `SteamTradeToken`, sowie ein Konto, das überhaupt zum Handel zugelassen ist.

Momentan werden folgende Artikelgegenstände in dieser Einstellung unterstützt:

| Wert | Name            | Beschreibung                                                                             |
| ---- | --------------- | ---------------------------------------------------------------------------------------- |
| 3    | FoilTradingCard | Glanz-Variante von `TradingCard`                                                         |
| 5    | TradingCard     | Steam-Sammel-Karte, die für die Herstellung von Abzeichen (Nicht-Glanz) verwendet werden |

Bitte bedenke, dass ASF unabhängig von den obigen Einstellungen nur nach Steam (`appID` von 753) Community (`contextID` von 6) Gegenständen fragt, so dass alle Spiel-Gegenstände und Geschenke und dergleichen per Definition aus dem Handelsangebot ausgeschlossen sind.

Aufgrund der zusätzlichen Mehrbelastung durch Verwendung dieser Option wird es empfohlen, diese Einstellung nur auf Bot-Konten zu verwenden, die eine realistische Chance haben, Sets selbst zu beenden. Es ergibt beispielsweise keinen Sinn, diese Eigenschaft zu aktivieren, wenn bereits `SendOnFarmingFinished`, `SendTradePeriod` oder `loot` auf üblicher Basis verwendet wird.

Wenn Sie sich nicht sicher sind, wie Sie diese Option konfigurieren, können Sie den Standardwert belassen.

---

### `CustomGamePlayedWhileFarming`

`string` Typ mit einem Standardwert von `null`. Wenn ASF sammelt, kann es sich als "Spielt ein Steam fremdes Spiel: `CustomGamePlayedWhileFarming`" anzeigen, anstatt des aktuellen Spiels was gesammelt wird. Dies kann nützlich sein, wenn Sie ihre Freunde darüber informieren möchten, dass Sie am Sammeln sind, aber den `OnlineStatus` nicht auf `Offline` ändern möchten. Bitte bedenke, dass ASF die tatsächliche Anzeige-Reihenfolge des Steam-Netzwerks nicht garantieren kann, daher ist dies nur ein Vorschlag, der richtig angezeigt werden kann oder auch nicht. In particular, custom name will not display in `Complex` farming algorithm if ASF fills all `32` slots with games requiring hours to be bumped. Der Standardwert von `null` deaktiviert dieses Feature.

ASF bietet einige spezielle Variablen, die Sie optional in Ihrem Text verwenden können. `{0}` wird durch die `AppID` des derzeit gefarmtem Spiele(s) von ASF ersetzt, während `{1}` stattdessen den `GameName` verwendet.

---

### `CustomGamePlayedWhileIdle`

`string` Typ mit einem Standardwert von `null`. Ähnlich wie `CustomGamePlayedWhileFarming`, aber für die Situation, in der ASF nichts zu tun hat (da das Konto vollständig gesammelt wurde). Bitte bedenke, dass ASF die tatsächliche Anzeige-Reihenfolge des Steam-Netzwerks nicht garantieren kann, daher ist dies nur ein Vorschlag, der richtig angezeigt werden kann oder auch nicht. If you're using `GamesPlayedWhileIdle` together with this option, then keep in mind that `GamesPlayedWhileIdle` get priority, therefore you can't declare more than `31` of them, as otherwise `CustomGamePlayedWhileIdle` will not be able to occupy the slot for custom name. Der Standardwert von `null` deaktiviert dieses Feature.

---

### `Enabled`

`bool` Typ mit einem Standardwert von `false`. Diese Eigenschaft definiert, ob der Bot aktiviert ist. Eine aktivierte Bot-Instanz (`true`) wird automatisch beim Starten von ASF gestartet, während eine deaktivierte Bot-Instanz (`false`) manuell gestartet werden muss. Standardmäßig ist jeder Bot deaktiviert, sodass Sie diese Eigenschaft wahrscheinlich auf `true` für alle Bots umschalten möchten, die automatisch gestartet werden sollen.

---

### `FarmingOrders`

`ImmutableList<byte>` Typ mit einem leeren Standardwert. Diese Eigenschaft definiert die **bevorzugte** Sammel-Reihenfolge für ein gegebenes Bot-Konto, welche ASF verwendet. Derzeit sind folgende Sammel-Reihenfolgen verfügbar:

| Wert | Name                      | Beschreibung                                                                                                            |
| ---- | ------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| 0    | Unordered                 | Keine Sortierung, leichte Verbesserung der CPU-Leistung                                                                 |
| 1    | AppIDsAscending           | Versuche Spiele mit den niedrigsten `appIDs` zuerst zu sammeln                                                          |
| 2    | AppIDsDescending          | Versuche Spiele mit den höchsten `appIDs` zuerst zu sammeln                                                             |
| 3    | CardDropsAscending        | Versuche Spiele mit der niedrigsten Anzahl an verbleibenden Karten zuerst zu sammeln                                    |
| 4    | CardDropsDescending       | Versuche Spiele mit der höchsten Anzahl an verbleibenden Karten zuerst zu sammeln                                       |
| 5    | HoursAscending            | Versuche Spiele mit der niedrigsten Anzahl an gespielten Stunden zuerst zu sammeln                                      |
| 6    | HoursDescending           | Versuche Spiele mit der höchsten Anzahl an gespielten Stunden zuerst zu sammeln                                         |
| 7    | NamesAscending            | Versuche Spiele in alphabetischer Reihenfolge zu sammeln, beginnend mit A                                               |
| 8    | NamesDescending           | Versuche Spiele in umgekehrter alphabetischer Reihenfolge zu sammeln, beginnend mit Z                                   |
| 9    | Random                    | Versuche Spiele in einer komplett zufälligen Reihenfolge zu sammeln (unterschiedlich bei jedem ausführen des Programms) |
| 10   | BadgeLevelsAscending      | Versuche Spiele mit den niedrigsten Abzeichen-Level zuerst zu sammeln                                                   |
| 11   | BadgeLevelsDescending     | Versuche Spiele mit den höchsten Abzeichen-Level zuerst zu sammeln                                                      |
| 12   | RedeemDateTimesAscending  | Versuche die ältesten Spiele auf unserem Konto zuerst zu sammeln                                                        |
| 13   | RedeemDateTimesDescending | Versuche die neuesten Spiele auf unserem Konto zuerst zu sammeln                                                        |
| 14   | MarketableAscending       | Versuche Spiele mit nicht marktfähigen Karten zuerst zu sammeln                                                         |
| 15   | MarketableDescending      | Versuche Spiele mit marktfähigen Karten zuerst zu sammeln                                                               |

Da es sich bei dieser Eigenschaft um ein Array handelt, können mehrere verschiedene Einstellungen in einer festen Reihenfolge kombiniert werden. Beispielsweise, indem Sie Werte von `15`, `11` und `7` einbeziehen, um zuerst nach marktfähigen Spielen, dann nach denen mit dem höchsten Abzeichen-Level und schließlich alphabetisch zu sortieren. As you can guess, the order actually matters, as reverse one (`7`, `11` and `15`) achieves something entirely different (sorts games alphabetically first, and due to game names being unique, the other two are effectively useless). Die Mehrheit der Benutzer wird wahrscheinlich nur eine Reihenfolge von allen verwenden, aber falls du gewünscht, können Sie auch weiter nach zusätzlichen Parametern sortiert werden.

Achte auch darauf, dass das Wort "versuchen" in allen obigen Beschreibungen vorkommt - die tatsächliche ASF-Reihenfolge wird stark von ausgewählten **[Karten-Sammel-Algorithmus](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance-de-DE)** beeinflusst und die Sortierung wirkt sich nur auf Ergebnisse aus, die ASF als gleich leistungsmäßig betrachtet. So sollte beispielsweise in `Simple` der ausgewählte `FarmingOrders` in der aktuellen Sammel-Sitzung vollständig berücksichtigen, da jedes Spiel den gleichen Leistungswert hat, während in `Complex` der Algorithmus die tatsächliche Reihenfolge zuerst nach Stunden beeinflusst und dann nach der gewählten `FarmingOrders` sortiert wird. Dies führt zu unterschiedlichen Ergebnissen, da Spiele mit vorhandener Spielzeit eine Priorität gegenüber anderen haben, sodass ASF effektiv Spiele bevorzugt, die bereits die erforderlichen `HoursUntilCardDrops` durchlaufen haben. Erst dann werden diese Spiele nach der gewählten `FarmingOrders` weiter sortiert. Genauso, sobald ASF keine bereits angestoßenen Spiele mehr hat, wird die verbleibende Warteschlange zuerst nach Stunden sortiert (da dies die Zeit, die für das Anstoßen eines der verbleibenden Titel benötigt wird, auf `HoursUntilCardDrops` verringert). Therefore, this config property is only a **suggestion** that ASF will try to respect, as long as it doesn't affect performance negatively (in this case, ASF will always prefer farming performance over `FarmingOrders`).

There is also farming priority queue that is accessible through `fq` **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. If it's used, actual farming order is sorted firstly by performance, then by farming queue, and finally by your `FarmingOrders`.

---

### `FarmPriorityQueueOnly`

`bool` Typ mit einem Standardwert von `false`. This property defines if ASF should consider for automatic farming only apps that you added yourself to priority farming queue available with `fq` **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. When this option is enabled, ASF will skip all `appIDs` that are missing on the list, effectively allowing you to cherry-pick games for automatic ASF farming. Keep in mind that if you didn't add any games to the queue then ASF will act as if there is nothing to farm on your account. Wenn du dir nicht sicher bist ob du diese Funktion aktivieren möchtest oder nicht, behalte sie mit dem Standardwert `false`.

---

### `GamesPlayedWhileIdle`

`ImmutableHashSet<uint>` Typ mit einem leeren Standardwert. Wenn ASF nichts zu sammeln hat, kann es stattdessen Ihre angegebenen Steam-Spiele (`appIDs`) spielen. Das Spielen von Spielen auf diese Weise erhöht lediglich die "gesamte Spielzeit" dieser Spiele. Damit diese Funktion richtig funktioniert, **muss** das Steam-Konto eine gültige Lizenz für alle `AppIDs` besitzen, die Sie angegeben haben; dies schließt auch F2P-Spiele mit ein. Dieses Feature kann gleichzeitig mit `CustomGamePlayedWhileIdle` aktiviert werden, um die ausgewählten Spiele zu spielen, während der benutzerdefinierte Status im Steam-Netzwerk angezeigt wird. In diesem Fall, wie in `CustomGamePlayedWhileFarming`, ist die tatsächliche Anzeige-Reihenfolge jedoch nicht garantiert. Bitte beachten Sie, dass Steam insgesamt ASF nur bis zu `32` `appIDs` spielen lässt, daher können nur so viele Spiele in diese Eigenschaft eingetragen werden.

---

### `HoursUntilCardDrops`

`byte` Typ mit einem Standardwert von `3`. Diese Eigenschaft legt fest, ob (und falls dies zutrifft, für wie viele Anfangsstunden) Kartendrops auf dem Konto eingeschränkt sind. Eingeschränkte Kartendrops bedeuten, dass das Konto keine Karten von einem bestimmten Spiel erhält, bis das Spiel mindestens `HoursUntilCardDrops` Stunden lang gespielt wurde. Leider gibt es keinen magischen Weg, das zu erkennen, also verlässt sich ASF auf Sie. Diese Eigenschaft wirkt sich auf den **[Karten-Sammel-Algorithmus](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance-de-DE)** aus, der verwendet wird. Wenn diese Eigenschaft richtig eingestellt ist, wird der Gewinn maximiert und die benötigte Zeit für die Bearbeitung der Karten minimiert. Bedenke, dass es keine offensichtliche Antwort gibt, welchen Wert Sie verwenden sollten, da es völlig vom entsprechenden Konto abhängt. Es scheint, dass ältere Konten, die nie um Rückerstattung gebeten haben, unbeschränkte Kartendrops haben, also sollten Sie einen Wert von `0` verwenden, während neue Konten und diejenigen, die um Rückerstattung gebeten haben, beschränkte Kartendrops mit einem Wert von `3` haben. Dies ist jedoch nur eine Theorie und sollte nicht als Tatsache betrachtet werden. Der Standardwert für diese Eigenschaft wurde basierend auf das "kleinere Übel" und der Mehrheit der Anwendungsfälle festgelegt.

---

### `LootableTypes`

`ImmutableHashSet<byte>` Typ mit Standardwert von `1, 3, 5` Steam-Gegenstands-Typen. Diese Eigenschaft definiert das ASF-Verhalten beim Plündern - sowohl manuell, durch Verwendung eines **[Befehls](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE#Befehle)**, als auch automatisch über eine oder mehrere Konfigurationseigenschaften. ASF wird sicherstellen, dass nur Gegenstände von `LootableTypes` in ein Handelsangebot aufgenommen werden, daher können Sie mit dieser Eigenschaft wählen, was Sie in einem Handelsangebot erhalten möchten, das an Sie gesendet wird.

| Wert | Name                  | Beschreibung                                                                                  |
| ---- | --------------------- | --------------------------------------------------------------------------------------------- |
| 0    | Unknown               | Jeder Typ, der nicht in eine der folgenden Kategorien passt                                   |
| 1    | BoosterPack           | Booster Pack mit 3 zufälligen Karten aus einem Spiel                                          |
| 2    | Emoticon              | Emoticon zur Verwendung im Steam-Chat                                                         |
| 3    | FoilTradingCard       | Folienvariante von `TradingCard`                                                              |
| 4    | ProfileBackground     | Profilhintergrund zur Verwendung im Steam-Profil                                              |
| 5    | TradingCard           | Steam-Karte, die für die Herstellung von Abzeichen (Nicht-Folie) verwendet werden             |
| 6    | SteamGems             | Steam-Edelsteine, die für die Herstellung von Booster Packs verwendet werden, inklusive Säcke |
| 7    | SaleItem              | Spezialgegenstände, die während des Steam-Sales vergeben werden                               |
| 8    | Consumable            | Spezielle Verbrauchsgegenstände, die nach dem Gebrauch wieder verschwinden                    |
| 9    | ProfileModifier       | Spezialgegenstände, welche das Aussehen des Steam-Profils verändern können                    |
| 10   | Sticker               | Spezialgegenstände, welche im Steam-Chat verwendet werden können                              |
| 11   | ChatEffect            | Spezialgegenstände, welche im Steam-Chat verwendet werden können                              |
| 12   | MiniProfilhintergrund | Besonderer Hintergrund für Steam-Profil                                                       |
| 13   | AvatarProfileFrame    | Besonderer Avatarrahmen für das Steam-Profil                                                  |
| 14   | AnimatedAvatar        | Besonders animierter Avatar für das Steam-Profil                                              |
| 15   | KeyboardSkin          | Special keyboard skin for Steam deck                                                          |
| 16   | StartupVideo          | Special startup video for Steam deck                                                          |

Bitte bedenke, dass ASF unabhängig von den obigen Einstellungen nur nach Steam (`appID` von 753) Community (`contextID` von 6) Gegenständen fragt, so dass alle Spiel-Gegenstände und Geschenke und dergleichen per Definition aus dem Handelsangebot ausgeschlossen sind.

Die Standard-ASF-Einstellung basiert auf der gebräuchlichsten Verwendung des Bots, wobei nur Boosterpacks und handelbare Sammelkarten (einschließlich Folienkarten) geöffnet werden. Die hier definierte Eigenschaft ermöglicht es Ihnen, dieses Verhalten so zu verändern, dass es Sie zufrieden stellt. Bitte bedenken Sie, dass alle nicht oben definierten Typen als `Unknown` Typ angezeigt werden, was besonders wichtig ist, wenn Valve einen neuen Steam-Gegenstand veröffentlicht, der ebenfalls von ASF als `Unknown` markiert wird, bis er hier (in einer zukünftigen Version) hinzugefügt wird. Deshalb ist es im Allgemeinen nicht empfehlenswert, `Unknown` in Ihren `LootableTypes` einzugeben, es sei denn, Sie wissen genau, was Sie tun und verstehen auch, dass ASF das gesamtes Inventar in einem Handelsangebot versenden wird, wenn das Steam-Netzwerk wieder unterbrochen wird und alle Ihre Gegenstände als `Unknown` meldet. Mein nachdrücklicher Vorschlag ist, `Unknown` nicht in das Feld `LootableTypes` einzutragen, selbst wenn Sie alles übertragen möchten.

---

### `MatchableTypes`

`ImmutableHashSet<byte>` Typ mit Standardwert von `5` Steam-Gegenstands-Typen. Diese Eigenschaft definiert, welche Steam Gegenstands-Arten angepasst werden dürfen, wenn die Option `SteamTradeMatcher` in `TradingPreferences` aktiviert ist. Die Arten sind wie folgt definiert:

| Wert | Name                  | Beschreibung                                                                                           |
| ---- | --------------------- | ------------------------------------------------------------------------------------------------------ |
| 0    | Unknown               | Jeder Typ, der nicht in eine der folgenden Kategorien passt                                            |
| 1    | BoosterPack           | Booster Pack mit 3 zufälligen Karten aus einem Spiel                                                   |
| 2    | Emoticon              | Emoticon zur Verwendung im Steam-Chat                                                                  |
| 3    | FoilTradingCard       | Glanz-Variante von `TradingCard`                                                                       |
| 4    | ProfileBackground     | Profilhintergrund zur Verwendung in deinem Steam-Profil                                                |
| 5    | TradingCard           | Steam-Sammel-Karte, die für die Herstellung von Abzeichen (Nicht-Glanz) verwendet werden               |
| 6    | SteamGems             | Steam-Edelsteine, die für die Herstellung von Booster Packs verwendet werden, inklusive Edelsteinsäcke |
| 7    | SaleItem              | Spezialgegenstände, die während des Steam-Sales vergeben werden                                        |
| 8    | Consumable            | Spezielle Verbrauchsgegenstände, die nach dem Gebrauch wieder verschwinden                             |
| 9    | ProfileModifier       | Spezialgegenstände, welche das Aussehen des Steam-Profils verändern können                             |
| 10   | Aufkleber             | Spezialgegenstände, welche im Steam-Chat verwendet werden können                                       |
| 11   | ChatEffect            | Spezialgegenstände, welche im Steam-Chat verwendet werden können                                       |
| 12   | MiniProfilhintergrund | Besonderer Hintergrund für Steam Profile                                                               |
| 13   | AvatarProfileFrame    | Besonderer Avatarrahmen für das Steam-Profil                                                           |
| 14   | AnimatedAvatar        | Besonders animierter Avatar für das Steam-Profil                                                       |
| 15   | KeyboardSkin          | Special keyboard skin for Steam deck                                                                   |
| 16   | StartupVideo          | Special startup video for Steam deck                                                                   |

Natürlich beinhalten die Typen, die Sie für diese Eigenschaft verwenden sollten, typischerweise nur `2`, `3`, `4` und `5`, da nur diese Typen von STM unterstützt werden. ASF beinhaltet die passende Logik, um die Seltenheit der Gegenstände zu ermitteln, daher ist es auch sicher, Emoticons oder Hintergründe zu vergleichen, da ASF nur die Gegenstände aus dem gleichen Spiel und Typ, die auch die gleiche Seltenheit aufweisen, als fair erachten wird.

Bitte bedenke, dass **ASF kein Handelsbot** ist und **sich NICHT um den Marktpreis schert.**. Wenn Sie Gegenstände der gleichen Seltenheit aus dem gleichen Set nicht als preislich gleichwertig betrachten, dann ist diese Option NICHT in ihrem Sinne. Bitte überlegen Sie sich zweimal, ob Sie diese Erklärung verstehen und damit einverstanden sind, bevor Sie diese Einstellung ändern.

Wenn du nicht weißt, was du tust, solltest du es bei dem Standardwert `5` belassen.

---

### `OnlineFlags`

`ushort flags` type with default value of `0`. This property works as supplement to **[`OnlineStatus`](#onlinestatus)** and specifies additional online presence features announced to Steam network. Requires **[`OnlineStatus`](#onlinestatus)** other than `Offline`, and is defined as below:

| Wert | Name              | Beschreibung                              |
| ---- | ----------------- | ----------------------------------------- |
| 0    | None              | No special online presence flags, default |
| 256  | ClientTypeWeb     | Client is using web interface             |
| 512  | ClientTypeMobile  | Client is using mobile app                |
| 1024 | ClientTypeTenfoot | Client is using big picture               |
| 2048 | ClientTypeVR      | Client is using VR headset                |

Bitte bedenken Sie, dass diese Eigenschaft das Feld `flags` ist, daher ist es möglich, eine beliebige Kombination von verfügbaren Werten auszuwählen. Mehr Informationen finden Sie unter **[JSON-Mapping](#json-mapping)**. Wenn keines der Flags aktiviert wird, wird die Option `None` verwendet.

The underlying `EPersonaStateFlag` type that this property is based on includes more available flags, however, to the best of our knowledge they have absolutely no effect as of today, therefore they were cut for visibility.

Wenn du dir nicht sicher bist, wie du diese Eigenschaft einstellen sollst, belasse sie bei dem Standardwert `0`.

---

### `OnlineStatus`

`byte` Typ mit einem Standardwert von `1`. Diese Eigenschaft gibt den Status der Steam-Community an, mit dem der Bot nach der Anmeldung im Steam-Netzwerk angekündigt wird. Derzeit können Sie einen der folgenden Stati wählen:

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

`Offline` Status ist extrem nützlich für primäre Konten. Wie Sie wissen sollten, zeigt das Sammeln eines Spiels tatsächlich den Steam-Status als "Spielt: XXX" an, was für Freunde irreführend sein kann und sie verwirrt, dass Sie ein Spiel spielen, während eigentlich nur gesammelt wird. Die Verwendung des `Offline`-Status löst dieses Problem. Das Konto wird nie als "im Spiel" angezeigt, wenn Sie Steam-Karten mit ASF sammeln. Dies ist dank der Tatsache möglich, dass sich ASF nicht in Steam Community anmelden muss, um richtig zu funktionieren. Also spielen wir tatsächlich diese Spiele, sind mit dem Steam-Netzwerk verbunden, aber ohne unsere Online-Präsenz überhaupt bekannt zu geben. Bedenken Sie, dass gespielte Spiele im Offline-Status immer noch auf die gesamte Spielzeit zählen und in dem Profil als "kürzlich gespielt" angezeigt werden.

Darüber hinaus ist diese Funktion auch wichtig, wenn Sie Benachrichtigungen und ungelesene Nachrichten erhalten möchten, während ASF läuft, ohne den Steam-Client gleichzeitig offen zu halten. Dies liegt daran, dass ASF selbst als Steam-Client fungiert, und ob ASF es möchte oder nicht, Steam sendet all diese Nachrichten (und andere Ereignisse) an ihn. Dies ist kein Problem, wenn Sie sowohl ASF als auch den eigenen Steam-Client ausführen, da beide Clients genau die gleichen Ereignisse empfangen. Wenn jedoch nur ASF läuft, könnte das Steam-Netzwerk bestimmte Ereignisse und Nachrichten als "zugestellt" markieren, obwohl der herkömmlicher Steam-Client diese nicht erhält, weil er nicht anwesend ist. Der Offline-Status löst auch dieses Problem, da ASF in diesem Fall nie für Community-Events in Betracht gezogen wird, sodass alle ungelesenen Nachrichten und andere Ereignisse bei Ihrer Rückkehr ordnungsgemäß als ungelesen markiert werden.

Es ist wichtig zu beachten, dass ASF, das im `Offline` Modus läuft, **nicht** in der Lage sein wird, Befehle auf herkömmliche Weise im Steam-Chat zu empfangen, da der Chat sowie die gesamte Community-Präsenz in der Tat völlig offline ist. Eine Lösung für dieses Problem ist die Verwendung des Modus `Invisible`, der auf ähnliche Weise funktioniert (nicht den Status offenbart), aber die Fähigkeit behält, Nachrichten zu empfangen und zu beantworten (also auch das Potenzial, Benachrichtigungen und ungelesene Nachrichten wie oben beschrieben zu verwerfen). Der Modus `Invisible` ist am sinnvollsten bei Alternativ-Konten, die statusmäßig unsichtbar bleiben sollen, aber trotzdem in der Lage sind, Befehle zu senden.

Es gibt jedoch einen Haken beim `Invisible` Modus - er funktioniert nicht gut mit primären Konten. Dies liegt daran, dass **jede** Steam-Sitzung, die derzeit online ist, den Status **anzeigt**, auch wenn ASF selbst es nicht tut. Dies wird durch Begrenzung/Fehler des Steam-Netzwerks verursacht, die auf der ASF-Seite nicht behoben werden können, sodass Sie, wenn Sie den `Invisible`-Modus verwenden möchten, auch sicherstellen müssen, dass **alle** andere Sitzungen zum gleichen Konto auch den `Invisible`-Modus verwenden. Dies trifft auf Alternativ-Konten zu, bei denen ASF hoffentlich die einzige aktive Sitzung ist, aber bei primären Konten werden Sie es fast immer vorziehen, sich Ihren Freunden als `Online` anzuzeigen, indem nur die Aktivität von ASF versteckt wird, und in diesem Fall wird der `Invisible`-Modus für Sie völlig nutzlos sein (wir empfehlen, stattdessen den `Offline`-Modus zu verwenden). Hoffentlich wird diese Beschränkung/Fehler in Zukunft von Valve behoben, aber ich würde nicht erwarten, dass das irgendwann bald passieren wird...

Wenn Sie sich nicht sicher sind, wie Sie diese Eigenschaft einrichten sollen, wird empfohlen, einen Wert von `0` (`Offline`) für primäre Konten und den Standard `1` (`Online`) anderweitig zu verwenden.

---

### `PasswordFormat`

`byte` type with default value of `0` (`PlainText`). This property defines the format of `SteamPassword` property, and currently supports values specified in the **[security](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security)** section. You should follow the instructions specified there, as you'll need to ensure that `SteamPassword` property indeed includes password in matching `PasswordFormat`. Mit anderen Worten, wenn Sie `PasswordFormat` ändern, dann sollte das `SteamPassword` **bereits** in diesem Format sein und nicht nur darauf abzielen, zu existieren. Wenn du nicht weißt was du tust, solltest du es bei dem Standardwert `0` belassen.

---

### `Paused`

`bool` Typ mit einem Standardwert von `false`. Diese Eigenschaft definiert den Anfangszustand des `CardsFarmer` Moduls. Mit dem Standardwert `false` startet der Bot automatisch das Sammeln, sobald er (entweder wegen `Enabled` oder wegen dem `start` Befehl) gestartet wird. Das Umschalten dieser Eigenschaft auf `true` sollte nur dann erfolgen, wenn du manuell den `resume` Befehl verwenden willst um den automatischen Sammel-Prozess wieder zu starten, z.B. weil du `play` ständig verwenden willst und niemals automatisch `CardsFarmer` Modul verwendest - das funktioniert genau wie der `pause` **[Befehl](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE)**. Wenn du dir nicht sicher bist ob du diese Funktion aktivieren möchtest oder nicht, behalte sie mit dem Standardwert `false`.

---

### `RedeemingPreferences`

`byte flags` Typ mit Standardwert von `0`. Diese Eigenschaft definiert das ASF-Verhalten beim aktivieren von Produktschlüsseln und ist wie folgt definiert:

| Wert | Name                               | Beschreibung                                                                                                                                         |
| ---- | ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0    | None                               | Keine speziellen Einlöse-Einstellungen, Standard                                                                                                     |
| 1    | Forwarding                         | Leite Produktschlüssel, die nicht zum Einlösen verfügbar sind, zu anderen Bots weiter                                                                |
| 2    | Distributing                       | Verteile alle Produktschlüssel auf sich und andere Bots                                                                                              |
| 4    | KeepMissingGames                   | Bewahre Produktschlüssel für (potenziell) fehlende Spiele beim Weiterleiten auf und lasse sie ungenutzt                                              |
| 8    | AssumeWalletKeyOnBadActivationCode | Versucht Keys als Guthaben-Codes einzulösen, da angenommen wird, dass `BadActivationCode`-Schlüssel mit `CannotRedeemCodeFromClient` identisch sind. |

Bitte bedenke, dass diese Eigenschaft das Feld `flags` ist, daher ist es möglich, eine beliebige Kombination von verfügbaren Werten auszuwählen. Mehr Informationen finden Sie unter **[JSON-Mapping](#json-mapping)**. Wenn keines der Flags aktiviert wird, wird die Option `None` verwendet.

`Forwarding` veranlasst den Bot, einen Produktschlüssel, der nicht eingelöst werden kann, an einen anderen verbundenen und angemeldeten Bot weiterzuleiten, dem dieses bestimmte Spiel fehlt (wenn es möglich ist, dies zu überprüfen). Die häufigste Situation ist die Weiterleitung von `AlreadyPurchased` Spiel an einen anderen Bot, dem dieses Spiel fehlt, aber diese Option deckt auch andere Szenarien ab, wie `DoesNotOwnRequiredApp`, `RateLimited` oder `RestrictedCountry`.

`Distributing` veranlasst den Bot, alle empfangenen Produktschlüssel unter sich und anderen Bots zu verteilen. Das bedeutet, dass jeder Bot einen einzigen Produktschlüssel aus der Charge erhält. Normalerweise wird dies nur verwendet, wenn Sie viele Produktschlüssel für das gleiche Spiel einlösen und diese gleichmäßig auf Ihre Bots verteilen möchten; im Gegensatz zum Einlösen von Produktschlüsseln für verschiedene Spiele. Diese Funktion ist nicht sinnvoll, wenn Sie nur einen Produktschlüssel in einer einzigen `redeem` Aktion einlösen (da es keine zusätzlichen Produktschlüssel gibt, die verteilt werden müssen).

`KeepMissingGames` veranlasst den Bot, `Forwarding` zu überspringen, wenn wir nicht sicher sein können, ob der Produktschlüssel, der eingelöst wird, tatsächlich unserem Bot gehört oder nicht. Dies bedeutet im Grunde, dass `Forwarding` **nur** für `AlreadyPurchased` Produktschlüssel gilt, anstatt auch andere Fälle wie `DoesNotOwnRequiredApp`, `RateLimited` oder `RestrictedCountry` zu behandeln. Normalerweise sollten Sie diese Option für das primäre Konto verwenden, um sicherzustellen, dass Produktschlüssel, die auf dem primären Konto eingelöst werden, nicht weitergereicht werden, wenn ein Bot beispielsweise vorübergehend `RateLimited` ist. Wie Sie der Beschreibung entnehmen können, hat dieses Feld absolut keine Wirkung, wenn `Forwarding` nicht aktiviert ist.

`AssumeWalletKeyOnBadActivationCode` wird `BadActivationCode` Schlüssel als `CannotRedeemCodeFromClient`behandeln, und führt somit dazu, dass ASF versucht, diese als Guthaben-Schlüssel einzulösen. Dies ist notwendig, da Steam möglicherweise Guthaben-Codes als `BadActivationCode` ankündigen könnte (und nicht `CannotRedeemCodeFromClient` , wie es zuvor war). Dies führt dazu, dass ASF nie versucht, sie einzulösen. Allerdings raten wir von **gegen** dieser Präferenz ab, da es dazu führen wird, dass ASF versucht, jeden ungültigen Schlüssel als Guthaben-Code einzulösen, was widerum zu einer übermäßigen Anzahl (potenziell ungültiger) Anfragen an den Steam-Service (mit allen möglichen Folgen). Stattdessen empfehlen wir den Modus `ForceAssumeWalletKey` **[`redeem^`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE#befehle-1)**, während bewusst Wallet-Schlüssel eingelöst werden, die die benötigte Problemumgehung nur dann aktiviert, wenn sie benötigt wird, auf der Basis des Bedarfs.

Wenn Sie sowohl `Forwarding` als auch `Distributing` aktivieren, wird zusätzlich zur Weiterleitung eine Verteilungsfunktion hinzugefügt, wodurch ASF versucht, einen Produktschlüssel auf allen Bots zuerst einzulösen (Weiterleitung), bevor es zum nächsten geht (Verteilung). Normalerweise möchten Sie diese Option nur verwenden, wenn Sie `Forwarding` möchten, aber mit geändertem Verhalten beim Einschalten des verwendeten Bots für Produktschlüssel, anstatt immer mit jedem Produktschlüssel in Reihenfolge zu gehen (das wäre `Forwarding` allein). Dieses Verhalten kann von Vorteil sein, wenn Sie wissen, dass die Mehrheit oder sogar alle Produktschlüssel an das gleiche Spiel gebunden sind. In dieser Situation würde `Forwarding` allein versuchen, alles auf einem Bot einzulösen (was Sinn macht, wenn die Produktschlüssel für einzigartige Spiele sind), und `Forwarding` + `Distributing` schaltet den Bot auf den nächsten Produktschlüssel um, "verteilt" die Aufgabe, einen neuen Produktschlüssel auf einen anderen als den ursprünglichen einzulösen (was sinnvoll ist, wenn die Produktschlüssel für das gleiche Spiel sind, überspringt einen sinnlosen Versuch pro Produktschlüssel).

Die tatsächliche Reihenfolge der Bots für alle Einlöseszenarien ist alphabetisch, mit Ausnahme von Bots, die nicht verfügbar sind (nicht verbunden, gestoppt oder ähnliches). Bitte bedenken Sie, dass es ein stündliches Limit für Einlösungsversuche pro IP und pro Konto gibt, und jeder Einlösungsversuch, der nicht mit `OK` endete, trägt zu fehlgeschlagenen Versuchen bei. ASF wird sein Bestes tun, um die Anzahl der `AlreadyPurchased` Fehler zu minimieren, z. B. indem es versucht zu vermeiden, einen Produktschlüssel an einen anderen Bot weiterzuleiten, der bereits dieses bestimmte Spiel besitzt, aber es ist nicht immer garantiert, dass es funktioniert, aufgrund dessen, wie Steam mit Lizenzen umgeht. Die Verwendung von Einlöse-Flags wie `Forwarding` oder `Distributing` erhöht immer die Wahrscheinlichkeit, dass Sie ein `RateLimited` (Anfragelimit) erreichen.

Außerdem ist zu beachten, dass Sie keine Produktschlüssel an Bots weiterleiten oder verteilen können, auf die Sie keinen Zugriff haben. This should be obvious, but ensure that you're at least `Operator` of all the bots you want to include in your redeeming process, for example with `status ASF` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**.

---

### `RemoteCommunication`

`byte flags` type with default value of `3`. This property defines per-bot ASF behaviour when it comes to communication with remote, third-party services, and is defined as below:

| Wert | Name          | Beschreibung                                                                                                                                                                                                                                                      |
| ---- | ------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0    | None          | No allowed third-party communication, rendering selected ASF features unusable                                                                                                                                                                                    |
| 1    | SteamGroup    | Allows communication with **[ASF's Steam group](https://steamcommunity.com/groups/archiasf)**                                                                                                                                                                     |
| 2    | PublicListing | Allows communication with **[ASF's STM listing](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/ItemsMatcherPlugin#publiclisting)** in order to being listed, if user has also enabled `SteamTradeMatcher` in **[`TradingPreferences`](#tradingpreferences)** |

Bitte bedenken Sie, dass diese Eigenschaft das Feld `flags` ist, daher ist es möglich, eine beliebige Kombination von verfügbaren Werten auszuwählen. Check out **[flags mapping](#json-mapping)** if you'd like to learn more. Wenn keines der Flags aktiviert wird, wird die Option `None` verwendet.

This option doesn't include every third-party communication offered by ASF, only those that are not implied by other settings. For example, if you've enabled ASF's auto-updates, ASF will communicate with both GitHub (for downloads) and our server (for checksum verification), as per your configuration. Likewise, enabling `MatchActively` in **[`TradingPreferences`](#tradingpreferences)** implies communication with our server to fetch listed bots, which is required for that functionality.

Further explanation on this subject is available in **[remote communication](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Remote-communication)** section. Wenn Sie keinen Grund haben diese Eigenschaft zu bearbeiten, solltest der Standardwert belassen werden.

---

### `SendOnFarmingFinished`

`bool` Typ mit einem Standardwert von `false`. Wenn ASF mit dem Sammeln des angegebenen Kontos fertig ist, kann es automatisch ein Steam-Handelsangebot mit allem, was bis zu diesem Punkt gesammelt wurde, an den Benutzer mit der Berechtigung `Master` senden, was sehr praktisch ist, falls Sie sich nicht selbst mit den Handelsangeboten beschäftigen möchten. Diese Option funktioniert genauso wie der `loot`-Befehl. Beachten Sie deshalb, dass Sie einen Benutzer mit der Berechtigung `Master` benötigen; vielleicht benötigen Sie außerdem einen gültigen `SteamTradeToken`, sowie ein Konto, das überhaupt zum Handel zugelassen ist. Zusätzlich zum Einleiten von `loot` nach Beendigung des Sammelns initiiert ASF auch `loot` bei jeder Benachrichtigung über neue Gegenstände (wenn nicht am Sammeln) und nach Abschluss jedes Handelsangebotes, der zu neuen Gegenständen führt (immer), wenn diese Option aktiv ist. Dies ist besonders nützlich, um von anderen Personen erhaltene Gegenstände auf unser Konto "weiterzuleiten".

Normalerweise solltest du **[ASF-2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-de-DE)** zusammen mit dieser Funktion verwenden, obwohl es keine Voraussetzung ist, wenn du beabsichtigst, manuell und rechtzeitig zu bestätigen. Wenn du dir nicht sicher bist, wie du diese Eigenschaft einstellen sollst, belasse sie bei dem Standardwert `false`.

---

### `SendTradePeriod`

`byte` Typ mit einem Standardwert von `0`. Diese Eigenschaft funktioniert sehr ähnlich wie `SendOnFarmingFinished` Eigenschaft, mit einem Unterschied - anstatt Handelsangebote zu senden, wenn das Sammeln abgeschlossen ist, können wir sie auch alle `SendTradePeriod` Stunden senden, unabhängig davon, wie viel wir noch zu sammeln haben. Dies ist nützlich, wenn du den normalen `loot` Befehl auf deinen Alternativ-Konten ausführen willst, anstatt darauf zu warten, dass sie das Sammeln beenden. Der Standardwert von `0` deaktiviert diese Funktion, wenn du möchtest, dass dein Bot dir z.B. jeden Tag Handelsangebote sendet, solltest du `24` hier eintragen.

Normalerweise solltest du **[ASF-2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-de-DE)** zusammen mit dieser Funktion verwenden, obwohl es keine Voraussetzung ist, wenn du beabsichtigst, manuell und rechtzeitig zu bestätigen. Wenn du dir nicht sicher bist, wie du diese Eigenschaft einstellen sollst, belasse sie bei dem Standardwert `0`.

---

### `ShutdownOnFarmingFinished`

`bool` Typ mit einem Standardwert von `false`. ASF "belegt" ein Konto für die gesamte Zeit des aktiven Prozesses. Wenn ein Konto mit dem Sammeln fertig ist, überprüft ASF regelmäßig (jede `IdleFarmingPeriode` Stunden), ob in der Zwischenzeit vielleicht einige neue Spiele mit Steam-Karten hinzugefügt wurden, damit es das Sammeln dieses Kontos fortsetzen kann, ohne den Prozess neu starten zu müssen. Dies ist für die Mehrheit der Menschen nützlich, da ASF bei Bedarf automatisch das Sammeln wieder aufnehmen kann. Jedoch kannst du den Prozess tatsächlich stoppen wollen, wenn das angegebene Konto vollständig gesammelt ist, du kannst das erreichen, indem du diese Eigenschaft auf `true` setzt. Wenn aktiviert, fährt ASF mit der Abmeldung fort, wenn das Konto vollständig gesammelt ist, was bedeutet, dass es nicht mehr regelmäßig überprüft oder belegt wird. Du solltest selbst entscheiden, ob du es vorziehst, dass ASF die ganze Zeit an einer bestimmten Bot-Instanz arbeitet, oder ob ASF sie vielleicht stoppen sollte, wenn das Sammeln abgeschlossen ist. Wenn alle Konten gestoppt sind und der Prozess nicht in `--process-required` **[Modus](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-Line-Arguments-de-DE)** läuft, wird ASF auch heruntergefahren, wodurch Ihre Maschine in den Ruhemodus geht und Sie andere Aktionen planen können (etwa ein Standbye-Modus oder Herunterfahren), sobald die letzte Karte gesammelt wurde.

Wenn du dir nicht sicher bist, wie du diese Eigenschaft einstellen sollst, belasse sie bei dem Standardwert `false`.

---

### `SkipRefundableGames`

`bool` Typ mit einem Standardwert von `false`. This property defines if ASF is permitted to farm games that are still refundable. Ein rückerstattungsfähiges Spiel ist ein Spiel, das Sie gemäß den **[Steam-Rückerstattungs-Richtlinien](https://store.steampowered.com/steam_refunds)** in den letzten 2 Wochen über den Steam-Shop gekauft und (welches Sie/ASF) noch nicht länger als 2 Stunden gespielt haben. By default when this option is set to `false`, ASF ignores Steam refunds policy entirely and farms everything, as most people would expect. However, you can change this option to `true` if you want to ensure that ASF won't farm any of your refundable games too soon, allowing you to evaluate those games yourself and refund if needed without worrying about ASF affecting playtime negatively. Please note that if you enable this option then games you purchased from Steam Store won't be farmed by ASF for up to 14 days since redeem date, which will show as nothing to farm if your account doesn't own anything else. Wenn du dir nicht sicher bist ob du diese Funktion aktivieren möchtest oder nicht, behalte sie mit dem Standardwert `false`.

---

### `SteamLogin`

`string` Typ mit einem Standardwert von `null`. Diese Eigenschaft definiert dein Steam-Login - das, mit dem du dich bei Steam anmeldest. In addition to defining steam login here, you may also keep default value of `null` if you want to enter your steam login on each ASF startup instead of putting it in the config. This may be useful for you if you don't want to save sensitive data in config file.

---

### `SteamMasterClanID`

`ulong` Typ mit einem Standardwert von `0`. Diese Eigenschaft definiert die steamID der Steam-Gruppe, der der Bot automatisch beitreten soll, einschließlich des Gruppen-Chats. Du kannst die steamID deiner Gruppe überprüfen, indem du zu ihrer **[Seite](https://steamcommunity.com/groups/archiasf)** navigierst und dann `/memberslistxml?xml=1` am Ende des Links hinzufügst, so dass der Link wie **[dieser](https://steamcommunity.com/groups/archiasf/memberslistxml?xml=1)** aussehen wird. Dann kannst du die steamID deiner Gruppe aus dem Ergebnis erhalten, sie befindet sich im `<groupID64>` Tag. Im obigen Beispiel wäre es `103582791440160998`. Zusätzlich zu dem Versuch, einer bestimmten Gruppe beim Start beizutreten, akzeptiert der Bot auch automatisch Gruppeneinladungen zu dieser Gruppe, so dass du deinen Bot manuell einladen kannst, wenn deine Gruppe eine private Mitgliedschaft hat. Wenn du keine Gruppe für deine Bots hast, solltest du diese Eigenschaft mit dem Standardwert `0` behalten.

---

### `SteamParentalCode`

`string` Typ mit einem Standardwert von `null`. Diese Eigenschaft definiert deinen Steam-Familienansicht-Code. ASF erfordert Zugriff auf Familienansicht-Ressourcen. Wenn Sie diese Funktion verwenden, sollten Sie ASF für eine problemlose Ausführung die PIN zum entsperren verraten. Der Standardwert von `null` bedeutet, dass für die Freischaltung dieses Kontos kein Steam-Familienansicht-Code erforderlich ist, und das ist wahrscheinlich das, was du willst, wenn du nicht die Steam-Elternfunktionalität verwendest.

Unter bestimmten Umständen kann ASF auch selbst einen gültigen Steam-Elterncode generieren, obwohl dies eine übermäßige Menge an Betriebssystem-Ressourcen und zusätzliche Zeit zum Fertigstellen erfordert, ganz zu schweigen davon, dass es garantiert erfolgreich ist. Daher empfehlen wir, sich nicht auf diese Funktion zu verlassen und stattdessen einen gültigen `SteamParentalCode` in der Konfiguration für ASF einzutragen. If ASF determines that PIN is required, and it'll be unable to generate one on its own, it'll ask you for input.

---

### `SteamPassword`

`string` Typ mit einem Standardwert von `null`. Diese Eigenschaft definiert dein Steam-Passwort - das, mit dem du dich bei Steam anmeldest. In addition to defining steam password here, you may also keep default value of `null` if you want to enter your steam password on each ASF startup instead of putting it in the config. This may be useful for you if you don't want to save sensitive data in config file.

---

### `SteamTradeToken`

`string` Typ mit einem Standardwert von `null`. Wenn du deinen Bot auf deiner Freundesliste hast, dann kann der Bot dir sofort ein Handelsangebot schicken, ohne sich um den Handels-Code zu kümmern, deshalb kannst du diese Eigenschaft auf dem Standardwert von `null` belassen. Wenn du dich jedoch dazu entscheidest, deinen Bot NICHT auf deiner Freundesliste zu haben, dann musst du einen Handels-Code dessen Benutzers generieren und eintragen an den dieser Bot Handelsangebote senden möchte. Mit anderen Worten, diese Eigenschaft sollte mit dem Handelscode des Kontos gefüllt werden, das mit `Master` Berechtigung in `SteamUserPermissions` von **dieser** Bot-Instanz definiert ist.

Um deinen Code zu finden, navigiere als angemeldeter Benutzer mit der Berechtigung `Master` **[hier](https://steamcommunity.com/my/tradeoffers/privacy)** und schau dir deine Handels URL an. Der Code, nach dem wir suchen, besteht aus 8 Zeichen nach `&token=` Teil in deiner Handels-URL. Du solltest diese 8 Zeichen kopieren und hier einfügen, als `SteamTradeToken`. Ungültig sind hingegen die Handels-URL als gesammtes, sowie der `&token=` Teil. Nur der Code selbst (8 Zeichen) ist hier erforderlich.

---

### `SteamUserPermissions`

` ImmutableDictionary<ulong, byte>` Typ mit einem leeren Standardwert. Diese Eigenschaft ist eine Dictionary-Eigenschaft, die den Steam-Benutzer, der durch seine 64-Bit-Steam-ID identifiziert wurde, auf die Nummer `byte` umbildet, die seine Berechtigung in ASF-Instanz angibt. Die derzeit verfügbaren Bot-Berechtigungen in ASF sind definiert als:

| Wert | Name          | Beschreibung                                                                                                                                                                                                                              |
| ---- | ------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0    | None          | Keine spezielle Berechtigung, dies ist hauptsächlich ein Referenzwert, der den in diesem Verzeichnis fehlenden Steam-IDs zugeordnet wird - es ist nicht notwendig, irgendjemanden mit dieser Berechtigung zu definieren                   |
| 1    | FamilySharing | Bietet einen minimalen Zugriff für Familienmitglieder. Auch hier handelt es sich vor allem um einen Referenzwert, da ASF in der Lage ist, automatisch Steam-IDs zu entdecken, die wir für die Nutzung unserer Bibliothek zugelassen haben |
| 2    | Operator      | Bietet Basiszugriff auf bestimmte Bot-Instanzen, hauptsächlich das Hinzufügen von Lizenzen und das Einlösen von Schlüsseln                                                                                                                |
| 3    | Master        | Gewährt vollen Zugriff auf die angegebene Bot-Instanz                                                                                                                                                                                     |

Kurz gesagt, diese Eigenschaft ermöglicht es dir, Berechtigungen für bestimmte Benutzer zu verwalten. Berechtigungen sind vor allem für den Zugriff auf ASF **[Befehle](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE)** wichtig, aber auch für die Aktivierung vieler ASF-Funktionen, wie z.B. die Annahme von Handelsangeboten. Zum Beispiel könnten Sie ihr eigenes Konto als `Master` einrichten und `Operator`-Zugang zu 2-3 Freunden geben, damit diese leicht Produktschlüssel für Ihren Bot mit ASF einlösen können, während Freude **nicht** berechtigt sind, z. B. den Bot zu stoppen. Dank dessen kannst du bestimmten Benutzern einfach Berechtigungen zuweisen und sie deinen Bot verwenden lassen.

Wir empfehlen, genau einen Benutzer als `Master` und eine beliebige Anzahl von `Operators` und darunter einzutragen. Während es technisch möglich ist, mehrere `Masters` einzustellen und ASF wird korrekt mit ihnen arbeiten, z.B. indem es alle Ihre an den Bot gesendeten Handelsangebote akzeptiert, wird ASF nur einen von ihnen (mit der niedrigsten Steam-ID) für jede Aktion verwenden, die ein einzelnes Ziel erfordert, z.B. eine `loot` Anfrage, also auch Eigenschaften wie `SendOnFarmingFinished` oder `SendTradePeriod`. If you perfectly understand those limitations, especially the fact that `loot` request will always send items to the `Master` with lowest steam ID, regardless of the `Master` that actually executed the command, then you can define multiple users with `Master` permission here, but we still recommend a single master scheme.

Es ist schön zu beachten, dass es noch eine weitere zusätzliche `Owner` Berechtigung gibt, die als `SteamOwnerID` globale Konfigurationseigenschaft deklariert ist. Du kannst hier niemandem die Berechtigung `Owner` zuweisen, da die Eigenschaft `SteamUserPermissions` nur Berechtigungen definiert, die sich auf die Bot-Instanz beziehen, und nicht ASF als Prozess. Für Bot-bezogene Aufgaben wird die `SteamOwnerID` genauso behandelt wie `Master`, sodass die Angabe einer `SteamOwnerID` hier nicht erforderlich ist.

---

### `TradeCheckPeriod`

`byte` Typ mit Standardwert von `60`. Normally ASF handles incoming trade offers right after receiving notification about one, but sometimes because of Steam glitches it can't do it at that time, and such trade offers remain ignored until next trade notification or bot restart occurs, which may lead to trades being cancelled or items not available at that later time. If this parameter is set to a non-zero value, ASF will additionally check for such outstanding trades every `TradeCheckPeriod` minutes. Default value is selected with balance between additional requests to steam servers and losing incoming trades in mind. However, if you are just using ASF to farm cards, and don't plan to automatically process any incoming trades, you may set it to `0` to disable this feature completely. On the other hand, if your bot participates in public [ASF's STM listing](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/ItemsMatcherPlugin#publiclisting) or provides other automated services as a trade bot, you may want to decrease this parameter to `15` minutes or so, to process all trades in a timely manner.

---

### `TradingPreferences`

`byte flags` Typ mit Standardwert von `0`. Diese Eigenschaft definiert das ASF-Verhalten beim Handeln und ist wie folgt definiert:

| Wert | Name                | Beschreibung                                                                                                                                                                                                                     |
| ---- | ------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0    | None                | Keine besonderen Handelspräferenzen, Standard                                                                                                                                                                                    |
| 1    | AcceptDonations     | Akzeptiert Handelsangebote, bei denen wir nichts verlieren                                                                                                                                                                       |
| 2    | SteamTradeMatcher   | Nimmt passiv an **[STM](https://www.steamtradematcher.com)**-artigen Handelsangeboten teil. Besuche **[Handeln](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading-de-DE#steamtradematcher)** für weitere Informationen |
| 4    | MatchEverything     | Erfordert das Setzen von `SteamTradeMatcher` und akzeptiert dabei neben guten und neutralen auch schlechte Handelsangebote                                                                                                       |
| 8    | DontAcceptBotTrades | Akzeptiert keine automatischen `loot` Handelsangebote von anderen Bot-Instanzen                                                                                                                                                  |
| 16   | MatchActively       | Nimmt aktiv an **[STM](https://www.steamtradematcher.com)**-artigen Handelsangeboten teil. Visit **[ItemsMatcherPlugin](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/ItemsMatcherPlugin#matchactively)** for more info    |

Bitte bedenken Sie, dass diese Eigenschaft das Feld `flags` ist, daher ist es möglich, eine beliebige Kombination von verfügbaren Werten auszuwählen. Check out **[flags mapping](#json-mapping)** if you'd like to learn more. Wenn keines der Flags aktiviert wird, wird die Option `None` verwendet.

For further explanation of ASF trading logic, and description of every available flag, please visit **[trading](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading)** section.

---

### `TransferableTypes`

`ImmutableHashSet<byte>` Typ mit Standardwert von `1, 3, 5` Steam-Gegenstands-Typen. This property defines which Steam item types will be considered for transfering between bots, during `transfer` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. ASF wird sicherstellen, dass nur Gegenstände von `TransferableTypes` in ein Handelsangebot aufgenommen werden, daher kannst du mit dieser Eigenschaft wählen, was du in einem Handelsangebot erhalten möchtest, das an einen deiner Bots gesendet wird.

| Wert | Name                  | Beschreibung                                                                                           |
| ---- | --------------------- | ------------------------------------------------------------------------------------------------------ |
| 0    | Unknown               | Jeder Typ, der nicht in eine der folgenden Kategorien passt                                            |
| 1    | BoosterPack           | Booster Pack mit 3 zufälligen Karten aus einem Spiel                                                   |
| 2    | Emoticon              | Emoticon zur Verwendung im Steam-Chat                                                                  |
| 3    | FoilTradingCard       | Glanz-Variante von `TradingCard`                                                                       |
| 4    | ProfileBackground     | Profilhintergrund zur Verwendung in deinem Steam-Profil                                                |
| 5    | TradingCard           | Steam-Sammel-Karte, die für die Herstellung von Abzeichen (Nicht-Glanz) verwendet werden               |
| 6    | SteamGems             | Steam-Edelsteine, die für die Herstellung von Booster Packs verwendet werden, inklusive Edelsteinsäcke |
| 7    | SaleItem              | Spezialgegenstände, die während des Steam-Sales vergeben werden                                        |
| 8    | Consumable            | Spezielle Verbrauchsgegenstände, die nach dem Gebrauch wieder verschwinden                             |
| 9    | ProfileModifier       | Spezialgegenstände, welche das Aussehen des Steam-Profils verändern können                             |
| 10   | Aufkleber             | Spezialgegenstände, welche im Steam-Chat verwendet werden können                                       |
| 11   | ChatEffect            | Spezialgegenstände, welche im Steam-Chat verwendet werden können                                       |
| 12   | MiniProfilhintergrund | Besonderer Hintergrund für Steam Profile                                                               |
| 13   | AvatarProfileFrame    | Besonderer Avatarrahmen für das Steam-Profil                                                           |
| 14   | AnimatedAvatar        | Besonders animierter Avatar für das Steam-Profil                                                       |
| 15   | KeyboardSkin          | Special keyboard skin for Steam deck                                                                   |
| 16   | StartupVideo          | Special startup video for Steam deck                                                                   |

Bitte bedenke, dass ASF unabhängig von den obigen Einstellungen nur nach Steam (`appID` von 753) Community (`contextID` von 6) Gegenständen fragt, so dass alle Spiel-Gegenstände und Geschenke und dergleichen per Definition aus dem Handelsangebot ausgeschlossen sind.

Die Standard-ASF-Einstellung basiert auf der gebräuchlichsten Verwendung des Bot, wobei nur Booster Packs und Trading Cards (einschließlich Folienkarten) gehandelt werden. Die hier definierte Eigenschaft ermöglicht es Ihnen, dieses Verhalten so zu verändern, dass es Sie zufrieden stellt. Bitte bedenken Sie, dass alle nicht oben definierten Typen als `Unknown` Typ angezeigt werden, was besonders wichtig ist, wenn Valve einen neuen Steam-Gegenstand veröffentlicht, der ebenfalls von ASF als `Unknown` markiert wird, bis er hier (in einer zukünftigen Version) hinzugefügt wird. Deshalb ist es im Allgemeinen nicht empfehlenswert, `Unknown` in deinen `TransferableTypes` einzugeben, es sei denn, du weißt, was du tust, und du verstehst auch, dass ASF dein gesamtes Inventar in einem Handelsangebot versenden wird, wenn das Steam-Netzwerk wieder unterbrochen wird und alle deine Gegenstände als `Unknown` meldet. Mein nachdrücklicher Vorschlag ist, `Unknown` nicht in das Feld `TransferableTypes` einzutragen, selbst wenn du alles übertragen möchtest.

---

### `UseLoginKeys`

`bool` Typ mit Standardwert von `true`. Diese Eigenschaft legt fest, ob ASF den Anmelde-Schlüssel-Mechanismus für dieses Steam-Konto verwenden soll. Der Anmelde-Schlüssel-Mechanismus funktioniert sehr ähnlich wie die "Remember me"-Option des offiziellen Steam-Clients, die es ASF ermöglicht, einen temporären, einmaligen Anmelde-Schlüssel für den nächsten Anmeldeversuch zu speichern und zu verwenden, wodurch die Angabe von Passwort, Steam Guard oder 2FA-Code übergangen wird, solange unser Anmelde-Schlüssel gültig ist. Der Anmelde-Schlüssel wird in der Datei `BotName.db` gespeichert und automatisch aktualisiert. Aus diesem Grund musst du kein Passwort/SteamGuard/2FA-Code angeben, nachdem du dich nur einmal erfolgreich mit ASF angemeldet hast.

Die Anmelde-Schlüssel werden standardmäßig für deine Bequemlichkeit verwendet, so dass du nicht bei jedem Anmeldevorgang `SteamPassword`, SteamGuard oder 2FA-Code (wenn du nicht **[ASF-2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-de-DE)** verwendest) eingeben musst. Es ist auch eine überlegene Alternative, da der Anmelde-Schlüssel nur einmalig verwendet werden kann und dein ursprüngliches Passwort in keiner Weise offenbart. Genau die gleiche Methode wird von deinem ursprünglichen Steam-Client verwendet, der deinen Konto-Namen und Anmelde-Schlüssel für deinen nächsten Anmeldeversuch speichert, was praktisch die gleiche ist wie die Verwendung von `SteamLogin` mit `UseLoginKeys` und leerem `SteamPassword` in ASF.

Einige Leute sind jedoch vielleicht sogar über dieses kleine Detail besorgt, deshalb steht diese Option hier zur Verfügung, wenn Sie sicherstellen möchten, dass ASF keine Art von Daten speichert, die es ermöglichen würden, die vorherige Sitzung nach dem Schließen wieder aufzunehmen, was dazu führt, dass eine vollständige Authentifizierung bei jedem Anmeldeversuch obligatorisch ist. Das Deaktivieren dieser Option funktioniert genau so, wie wenn du "Remember me" im offiziellen Steam-Client nicht aktivierst. Wenn du nicht weißt, was du tust, solltest du es bei dem Standardwert `true` belassen.

---

### `UserInterfaceMode`

`byte` Typ mit einem Standardwert von `0`. This property specifies user interface mode that the bot will be announced with after logging in to Steam network. Currently you can choose one of below modes:

| Wert | Name       |
| ---- | ---------- |
| `0`  | Default    |
| `1`  | BigPicture |
| `2`  | Mobile     |

Wenn du dir nicht sicher bist, wie du diese Eigenschaft einstellen sollst, belasse sie bei dem Standardwert `0`.

---

## Dateistruktur

ASF benutzt eine einfache Dateistruktur.

```text
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
```

Um ASF an einen neuen Ort zu verschieben, z. B. auf einen anderen PC, genügt es, das Verzeichnis `config` allein zu verschieben/kopieren, und das ist die empfohlene Art und Weise, jede Form von "ASF-Backups" durchzuführen, da man immer den verbleibenden (Programm-)Teil von GitHub herunterladen kann, ohne zu riskieren, interne ASF-Dateien zu beschädigen, z. B. durch ein fehlerhaftes Backup.

Die `log.txt` Datei enthält das Protokoll, das durch deinen letzten ASF-Lauf erzeugt wurde. Diese Datei enthält keine sensiblen Informationen und ist äußerst nützlich, wenn es um Probleme, Abstürze oder einfach nur als Information an dich geht, was im letzten ASF-Lauf passiert ist. Wir werden sehr oft nach dieser Datei fragen, wenn Sie auf Probleme oder Fehler stoßen. ASF verwaltet diese Datei automatisch für dich, aber du kannst die ASF **[Protokollierung](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Logging-de-DE)**s-Modul weiter optimieren, wenn du ein fortgeschrittener Benutzer bist.

`config` Verzeichnis ist der Ort, der die Konfiguration für ASF enthält, einschließlich aller seiner Bots.

`ASF.json` ist eine globale ASF-Konfigurationsdatei. Diese Konfiguration wird verwendet, um anzugeben, wie sich ASF als Prozess verhält, was alle Bots und das Programm selbst betrifft. Dort findest du globale Eigenschaften wie ASF-Prozesseigentümer, automatische Aktualisierungen oder Debugging.

`BotName.json` ist eine Konfiguration der gegebenen Bot-Instanz. Diese Konfiguration wird verwendet, um das Verhalten einer gegebenen Bot-Instanz festzulegen, daher sind diese Einstellungen nur für diesen Bot spezifisch und nicht für andere bestimmt. Dies ermöglicht es dir, Bots mit verschiedenen Einstellungen zu konfigurieren, und nicht unbedingt alle funktionieren auf die gleiche Weise. Jeder Bot wird mit einer eindeutigen Kennzeichnung benannt, die Sie anstelle von `BotName` auswählen.

Neben den Konfigurationsdateien verwendet ASF auch das Verzeichnis `config` zum Speichern von Datenbanken.

`ASF.db` ist eine globale ASF-Datenbankdatei. Es fungiert als globaler dauerhafter Speicher und wird zum Speichern verschiedener Informationen im Zusammenhang mit dem ASF-Prozess verwendet, wie z.B. IPs von lokalen Steam-Servern. **Du solltest diese Datei nicht bearbeiten**.

`BotName.db` ist eine Datenbank der jeweiligen Bot-Instanz. Diese Datei wird verwendet, um wichtige Daten der jeweiligen Bot-Instanz im dauerhaften Speicher zu speichern, wie z.B. Anmelde-Schlüssel oder ASF 2FA. **Du solltest diese Datei nicht bearbeiten**.

`BotName.bin` ist eine spezielle Datei der jeweiligen Bot-Instanz, die Informationen über den Steam-Sentry-Hash enthält. Der Sentry-Hash wird für die Authentifizierung mit dem `SteamGuard` Mechanismus verwendet, sehr ähnlich Steam-Datei `ssfn`. **Du solltest diese Datei nicht bearbeiten**.

`BotName.keys` ist eine spezielle Datei, die zum Importieren von Produkt-Schlüsseln in den **[Hintergrundproduktschlüsselaktivierer ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer-de-DE)** verwendet werden kann. Es ist nicht zwingend erforderlich und wird nicht generiert, aber von ASF anerkannt. Diese Datei wird nach dem erfolgreichen Import der Produkt-Schlüssel automatisch gelöscht.

`BotName.maFile` ist eine spezielle Datei, die für den Import von **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-de-DE)** verwendet werden kann. Es ist nicht zwingend erforderlich und wird nicht generiert, aber von ASF erkannt, wenn dein `BotName` ASF 2FA noch nicht verwendet. Diese Datei wird nach erfolgreichem Import von ASF 2FA automatisch gelöscht.

---

## JSON-Mapping

Jede Konfigurationseigenschaft hat ihren Typ. Der Typ der Eigenschaft definiert Werte, die für sie gültig sind. Du kannst nur Werte verwenden, die für einen bestimmten Typ gültig sind - wenn du einen ungültigen Wert verwendest, dann kann ASF deine Konfiguration nicht verarbeiten.

**Wir empfehlen dringend, den ConfigGenerator für die Generierung von Konfigurationen zu verwenden** - er handhabt den größten Teil des Low-Level-Sachen (wie z.B. die Typ-Validierung) für dich, so dass du nur die richtigen Werte eingeben musst, und du musst auch die unten angegebenen Variablentypen nicht verstehen. Dieser Abschnitt ist hauptsächlich für Personen gedacht, die Konfigurationen manuell erstellen/bearbeiten, damit sie wissen, welche Werte sie verwenden können.

Die von ASF verwendeten Typen sind native C#-Typen, die im Folgenden aufgeführt werden:

---

`bool` - Boolean-Typ der nur die Werte `true` und `false` akzeptiert.

Beispiel: `"Enabled": true`

---

`byte` - Unsignierter Byte-Typ der nur ganze Zahlen von `0` bis `255` (einschließlich) akzeptiert.

Beispiel: `"ConnectionTimeout": 90`

---

`ushort` - Unsignierter Short-Typ der nur ganze Zahlen von `0` bis `65535` (einschließlich) akzeptiert.

Beispiel: `"WebLimiterDelay": 300`

---

`uint` - Unsignierter Ganzzahl-Typ, der nur ganze Zahlen von `0` bis `4294967295` (einschließlich) akzeptiert.

---

`ulong` - Unsignierter long integer Typ, der nur ganze Zahlen von `0` bis `18446744073709551615` (einschließlich) akzeptiert.

Beispiel: `"SteamMasterClanID": 103582791440160998`

---

`string` - Zeichenketten-Typ, der jede beliebige Zeichenfolge akzeptiert, einschließlich der leeren Folge `""` und `null`. Leere Sequenz und `null` Wert werden von ASF gleich behandelt, sodass Sie es sich aussuchen können, welche Sie verwenden möchten (wir bleiben bei `null`).

Beispiele: `"SteamLogin": null`, `"SteamLogin": ""`, `"SteamLogin": "MeinLogin"`

---

`Guid?` - Nullable UUID type, in JSON encoded as string. UUID is made out of 32 hexadecimal characters, in range from `0` to `9` and `a` to `f`. ASF accepts variety of valid formats - lowercase, uppercase, with and without dashes. In addition to valid UUID, since this property is nullable, a special value of `null` is accepted to indicate lack of UUID to provide.

Examples: `"LicenseID": null`, `"LicenseID": "f6a0529813f74d119982eb4fe43a9a24"`

---

`ImmutableList<valueType>` - Unveränderliche Sammlung (set) von eindeutigen Werten im gegebenen `valueType`. In JSON ist es definiert als Array von Elementen in gegebenem `valueType`. ASF verwendet `list` um anzugeben, dass die angegebene Eigenschaft mehrere Werte unterstützt und deren Reihenfolge relevant sein kann.

Beispiel für `ImmutableList<byte>`: `"FarmingOrders": [15, 11, 7]`

---

`ImmutableHashSet<valueType>` - Unveränderliche Sammlung (Satz) von eindeutigen Werten in gegebenen `valueType`. In JSON ist es definiert als Array von Elementen in gegebenem `valueType`. ASF verwendet `HashSet`, um anzugeben, dass eine angegebene Eigenschaft nur für eindeutige Werte Sinn macht (und deren Reihenfolge unwichtig ist). Daher ignoriert es bewusst alle potenziellen Duplikate während des Parsen (falls sie trotzdem angegeben wurden).

Beispiel für `ImmutableHashSet<uint>`: `"Blacklist": [267420, 303700, 335590]`

---

`ImmutableDictionary<keyType, valueType>` - Unveränderliches Wörterbuch (Map) das einen in seinem `keyType` angegebenen einzigartigen Schlüssel auf den in seinem `valueType` angegebenen Wert abbildet. In JSON wird es als Objekt mit Schlüssel-Wert-Paaren definiert. Bedenke, dass `keyType` in diesem Fall immer angegeben wird, auch wenn es sich um einen Wert-Typ wie `ulong` handelt. Es gibt auch eine strenge Anforderung, dass der Schlüssel auf der gesamten map eindeutig ist, diesmal ebenfalls von JSON durchgesetzt.

Beispiel für `ImmutableDictionary<ulong, byte>`: `"SteamUserPermissions": { "76561198174813138": 3, "76561198174813137": 1 }`

---

`flags` - Das Attribut Flags kombiniert mehrere verschiedene Eigenschaften zu einem Endwert, indem es bitweise Operationen anwendet. Auf diese Weise kannst du jede mögliche Kombination aus verschiedenen zulässigen Werten gleichzeitig wählen. Der Endwert wird als Summe aller Werte der aktivierten Optionen berechnet.

Zum Beispiel, wenn folgende Werte angegeben werden:

| Wert | Name |
| ---- | ---- |
| 0    | None |
| 1    | A    |
| 2    | B    |
| 4    | C    |

Die Verwendung von `B + C` würde zu einem Wert von `6` führen, die Verwendung von `A + C` würde zu einem Wert von `5` führen, die Verwendung von `C` würde zu einem Wert von `4`führen und so weiter. Dies ermöglicht es dir, jede mögliche Kombination von aktivierten Werten zu erstellen - wenn du dich dazu entschieden hast, alle zu aktivieren, indem du `None + A + B + C` wählst, bekommst du den Wert von `7`. Außerdem ist zu beachten, dass das Flag mit dem Wert `0` per Definition in allen anderen verfügbaren Kombinationen aktiviert ist, daher ist es sehr oft ein Flag, das nichts Bestimmtes aktiviert (wie `None`).

Wie Sie sehen können, haben wir im obigen Beispiel 3 verfügbare Flags zum Ein- und Ausschalten (`A`, `B`, `C`), und `8` mögliche Werte insgesamt:
- `None -> 0`
- `A -> 1`
- `B -> 2`
- `A + B -> 3`
- `C -> 4`
- `A + C -> 5`
- `B + C -> 6`
- `A + B + C -> 7`

Zum Beispiel: `"SteamProtocols": 7`

---

## Kompatibilitätsmapping

Due to JavaScript limitations of being unable to properly serialize simple `ulong` fields in JSON when using web-based ConfigGenerator, `ulong` fields will be rendered as strings with `s_` prefix in the resulting config. This includes for example `"SteamOwnerID": 76561198006963719` that will be written by our ConfigGenerator as `"s_SteamOwnerID": "76561198006963719"`. ASF enthält die richtige Logik für die automatische Verarbeitung dieses Zeichenketten-Mappings, so dass `s_` Einträge in deine Konfigurationen tatsächlich gültig und korrekt generiert sind. Wenn du selbst Konfigurationen generierst, empfehlen wir, nach Möglichkeit an den ursprünglichen `ulong` Feldern festzuhalten, aber wenn du dazu nicht in der Lage bist, kannst du auch diesem Schema folgen und sie als Zeichenketten mit `s_` Präfix an ihren Namen kodieren. Wir hoffen, diese JavaScript-Einschränkung irgendwann aufheben zu können.

---

## Konfigurationskompatibilität

It's top priority for ASF to remain compatible with older configs. As you should already know, missing config properties are treated the same as they would be defined with their **default values**. Wenn also eine neue Konfigurationseigenschaft in einer neuen Version von ASF eingeführt wird, bleiben all deine Konfigurationen **kompatibel** mit der neuen Version, und ASF wird diese neue Konfigurationseigenschaft so behandeln, wie sie mit ihrem **Standardwert** definiert wäre. Du kannst jederzeit Konfigurationseigenschaften nach deinen Wünschen hinzufügen, entfernen oder bearbeiten.

Wir empfehlen, definierte Konfigurationseigenschaften nur auf die zu ändernden Eigenschaften zu beschränken, da Sie auf diese Weise automatisch Standardwerte für alle anderen vererben, nicht nur Ihre Konfiguration sauber halten, sondern auch die Kompatibilität erhöhen, falls wir beschließen, einen Standardwert für Eigenschaften zu ändern, die Sie nicht explizit selbst festlegen möchten (z. B. `WebLimiterDelay`).

Due to above, ASF will automatically migrate/optimize your configs by reformatting them and removing fields that hold default value. You can disable this behaviour with `--no-config-migrate` **[command-line argument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments#arguments)** if you have a specific reason, for example you're providing read-only config files and you don't want ASF to modify them.

---

## Automatisches Nachladen

Ab ASF V2.1.6.2+ ist es möglich, dass Konfigurationen "on-the-fly" geändert werden - dadurch wird ASF automatisch:
- Eine neue Bot-Instanz erstellen (und startet sie bei Bedarf), wenn Sie die Konfiguration erstellen
- Stoppt (falls erforderlich) und entfernt alte Bot-Instanz, wenn Sie Ihre Konfiguration löschen
- Stoppt (und startet bei Bedarf) eine beliebige Bot-Instanz, wenn Sie Ihre Konfiguration bearbeiteen
- Startet den Bot (falls erforderlich) unter neuem Namen neu, wenn Sie seine Konfiguration umbenennen

All dies ist transparent und wird automatisch durchgeführt, ohne dass das Programm neu gestartet oder andere (nicht betroffene) Bot-Instanzen beendet werden müssen.

In addition to that, ASF will also restart itself (if `AutoRestart` permits) if you modify core ASF `ASF.json` config. Likewise, program will quit if you delete or rename it.

You can disable this behaviour with `--no-config-watch` **[command-line argument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments#arguments)** if you have a specific reason, for example you don't want from ASF to react to file changes in `config` folder.