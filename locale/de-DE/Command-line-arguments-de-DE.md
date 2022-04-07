# Konsolen-Argumente

ASF unterstützt mehrere Befehlszeilenargumente, die die Programm-Runtime beeinflussen können. Diese können von fortgeschrittenen Benutzern verwendet werden, um festzulegen, wie das Programm arbeiten soll. Im Vergleich zum gängigen Standardweg über die Konfigurationsdatei `ASF.json` werden Befehlszeilenargumente für die Kerninitialisierung (z. B. `--path`), plattformspezifische Einstellungen (z. B. `--system-required`) oder sensible Daten (z. B. `--cryptkey`) verwendet.

---

## Nutzung

Die Nutzung ist abhängig vom verwendeten Betriebssystem und der ASF-Variante.

Generisch:

```shell
dotnet ArchiSteamFarm.dll --argument --anderesArgument
```

Windows:

```powershell
.\ArchiSteamFarm.exe --argument --anderesArgument
```

Linux/OS X:

```shell
./ArchiSteamFarm --argument --anderesArgument
```

Die Befehlszeilenargumente werden auch in allgemeinen Hilfsskripten wie zum Beispiel `ArchiSteamFarm.cmd` oder `ArchiSteamFarm.sh` unterstützt. Darüber hinaus können Sie bei Verwendung eines Hilfsskripts auch die Umgebungsvariable `ASF_ARGS` verwenden, wie es im Abschnitt **[Docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker-de-DE#befehlszeilenargumente)** beschrieben ist.

Vergessen Sie nicht, Anführungszeichen zu verwenden, falls Ihr Argument Leerzeichen enthält. Diese beiden Beispiele sind falsch:

```shell
./ArchiSteamFarm --path /home/archi/Meine Downloads/ASF # Falsch!
./ArchiSteamFarm --path=/home/archi/Meine Downloads/ASF # Falsch!
```

Aber diese zwei sind völlig in Ordnung:

```shell
./ArchiSteamFarm --path "/home/archi/Meine Downloads/ASF" # OK
./ArchiSteamFarm "--path=/home/archi/Meine Downloads/ASF" # OK
```

## Argumente

`--cryptkey <key>` oder `--cryptkey=<key>` - ASF startet mit dem benutzerdefinierten kryptographischen Schlüssel `<key>`. Diese Option wirkt sich auf die **[Sicherheit](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security-de-DE#sicherheit)** aus und veranlasst ASF, den von Ihnen bereitgestellten `<key>` Schlüssel anstelle des standardmäßig fest in die ausführbare Datei einprogrammierten Schlüssels zu verwenden. Beachten Sie bitte, dass sämtliche Verschlüsselungen/Hashs bei jedem ASF-Lauf weitergegeben wird, da diese Eigenschaft den Standard-Verschlüsselungsschlüssel (für Verschlüsselungszwecke) sowie SALT (für Hashing-Zwecke) betrifft.

Aufgrund der Natur dieser Eigenschaft ist es auch möglich, einen cryptkey zu setzen, indem man die Umgebungsvariable `ASF_CRYPTKEY` deklariert, was für Personen, die sensible Details in den Prozessargumenten vermeiden wollen, besser geeignet sein kann.

---

`--ignore-unsupported-environment` - wird ASF dazu führen die Erkennung von nicht unterstützten Umgebungen zu ignorieren, die normalerweise mit einem Fehler signalisiert und das Beenden erzwingen. Nicht unterstützte Umgebung beinhalten zum Beispiel das Ausführen von .NET Framework Build auf Plattformen, die stattdessen .NET (Core) Build ausführen könnten. Während diese Option es ASF erlaubt, in solchen Szenarien zu laufen, sollten Sie beachten, dass wir dies offiziell nicht unterstützen und Sie dies vollständig **auf eigene Gefahr** tun. Ab heute können **alle** der nicht unterstützten Umgebungsszenarien korrigiert werden, wie zum Beispiel das Ausführen von `generic` Build statt `generic-netf`. Wir empfehlen Ihnen dringend, die offenen Probleme zu beheben, anstatt dieses Argument anzugeben.

---

`--network-group <group>` oder `--network-group=<group>` - führt dazu, dass ASF seine Begrenzer mit einer benutzerdefinierten Netzwerkgruppe mit einem Wert `<group>` initialisiert. Diese Option wirkt sich auf die Ausführung von ASF in **[mehreren Instanzen](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-de-DE#multiple-instances)** aus, indem signalisiert wird, dass diese Instanz nur von Instanzen abhängig ist, die dieselbe Netzwerkgruppe teilen und unabhängig vom Rest. In der Regel sollte diese Eigenschaft nur verwendet werden, wenn Sie ASF-Anfragen über einen benutzerdefinierten Mechanismus (z.b. verschiedene IP-Adressen) und eine Netzwerkgruppen selbst einstellen möchten, ohne sich darauf zu verlassen, dass ASF dies automatisch macht (dies berücksichtigt derzeit nur `WebProxy`). Beachten Sie, dass es sich bei der Verwendung einer benutzerdefinierten Netzwerkgruppe um einen eindeutigen Bezeichner innerhalb des lokalen Rechners handelt und ASF keine weiteren Details berücksichtigt, wie z. B. den Wert vom `WebProxy`, wodurch Sie z. B. zwei Instanzen mit unterschiedlichen `WebProxy` Werten starten können, die noch voneinander abhängig sind.

Aufgrund der Natur dieser Eigenschaft ist es auch möglich, den Wert zu setzen, indem man die Umgebungsvariable `ASF_NETWORK_GROUP` deklariert, was für Personen, die sensible Details in den Prozessargumenten vermeiden wollen, besser geeignet sein kann.

---

`--no-config-migrate` - ASF migriert standardmäßig Ihre Konfigurationsdateien automatisch in die neueste Syntax. Diese Option verhindert das. Migration beinhaltet die Umwandlung veralteter Eigenschaften in Neue, das Entfernen von Eigenschaften mit Standardwerten (da diese keine Wirkung haben), sowie die Bereinigung der Datei im Allgemeinen (Korrektur der Einrückung und ähnliches). Dies ist fast immer eine gute Idee, aber Sie könnten eine bestimmte Situation haben, in der Sie es vorziehen würden, wenn ASF die Konfigurationsdateien niemals automatisch überschreibt. Zum Beispiel: Vielleicht möchten Sie `chmod 400`auf Ihre Konfigurationsdateien anwenden (nur für den Eigentümer lesbar) oder `chattr +i` anwenden, was dazu führt, dass für jeden Schreibzugriff verweigert wird, z.B. als Sicherheitsmaßnahme. Wir empfehlen die Konfigurationsdatei-Migration aktiviert zu lassen, es sei denn Sie haben einen guten Grund diese zu deaktivieren, und wünschen, dass ASF dies nicht tut.

---

`--no-config-watch` - legt ASF standardmäßig einen `FileSystemWatcher` für Ihre `Konfiguration` fest, um auf Ereignisse im Zusammenhang mit Änderungen der Datei zu hören, sodass diese sich interaktiv anpasst. Dies beinhaltet beispielsweise das Stoppen von Bots beim Löschen der Konfiguration, den Neustart des Bots bei der Änderung der Konfiguration, oder laden von Schlüsseln in **[BGR](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer-de-DE#)** sobald Sie sie in das `Verzeichnis` eingegeben werden. Diese Option erlaubt es Ihnen, solch ein Verhalten zu deaktivieren, sodass ASF alle Änderungen im `config`-Verzeichnis ignoriert, wodurch alle Aktionen manuell ausgeführt werden müssen, sofern sie erforderlich sind. Wir empfehlen die Konfigurationsereignisse aktiviert zu lassen, außer Sie haben einen guten Grund diese für ASF zu deaktivieren.

---

`--no-restart` - Diese Option wird hauptsächlich in unseren **[Docker](https://github.com/JustArchi/ArchiSteamFarm/wiki/Docker-de-DE)**-Containern genutzt und setzt `AutoRestart` auf `false`. Solange du keinen bestimmten Grund besitzt, solltest du stattdessen die `AutoRestart` Option in deiner Konfiguration verwenden. This switch is here so our docker script won't need to touch your global config in order to adapt it to its own environment. Natürlich, wenn Sie ASF in einem Skript ausführen, können Sie auch diesen Schalter verwenden (ansonsten sind Sie mit der globalen Konfigurationseigenschaft besser dran).

---

`--no-steam-parental-generation` - by default ASF will automatically attempt to generate Steam parental PINs, as described in **[`SteamParentalCode`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#steamparentalcode)** configuration property. However, since that might require excessive amount of OS resources, this switch allows you to disable that behaviour, which will result in ASF skipping auto-generation and go straight to asking user for PIN instead, which is what would normally happen only if the auto-generation has failed. Usually we recommend to keep the generation enabled, but if you have a particular reason for disabling it and would instead prefer ASF to not do that, you can use this switch for achieving that purpose.

---

`--path <path>` oder `--path=<path>` - ASF wechselt beim Start immer in sein eigenes Verzeichnis. Wird dieser Parameter angegeben so wird ASF nach der Initialisierung zu dem gegebenem Programmverzeichnis navigieren, dies macht es Ihnen möglich andere Verzeichnisse wie z.B. `config`, `plugins` oder `www` (sowie auch die `NLog.config` Datei) für unterschiedliche Teile der Applikation zu nutzen. Das Kopieren der Binärdateien an diese Stellen ist dadurch nicht mehr nötig. Es kann besonders nützlich sein, wenn Sie die Binärdatei von der eigentlichen Konfiguration trennen möchten, wie es in einer Linux-ähnlichen Verpackung geschieht - so können Sie eine (aktuelle) Binärdatei mit mehreren verschiedenen Konfigurationen verwenden. Der Pfad kann entweder relativ zum aktuellen Ort der ASF-Binärdatei oder absolut sein. Bedenke auch, dass dieser Befehl auf einen neuen "ASF Homeordner" zeigt - ein Verzeichnis, welches die gleiche Struktur wie der ursprüngliche ASF Ordner hat, mit einem Verzeichnis config darin, siehe unteres Beispiel für eine Erklärung.

Aufgrund der Natur dieser Eigenschaft ist es auch möglich, den erwarteten Pfad zu setzen, indem man die Umgebungsvariable `ASF_PATH` deklariert, was für Personen, die sensible Details in den Prozessargumenten vermeiden wollen, besser geeignet sein kann.

Wenn Sie erwägen, dieses Kommandozeilenargument für die Ausführung mehrerer ASF-Instanzen zu verwenden, empfehlen wir Ihnen, unsere **[Kompatibilitätsseite](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-de-DE#multiple-instances)** zu diesem Thema durchzulesen.

Beispiele:

```shell
dotnet /opt/ASF/ArchiSteamFarm.dll --path /opt/Zielverzeichnis #Absoluter Pfad
dotnet /opt/ASF/ArchiSteamFarm.dll --path ../Zielverzeichnis #Relativer Pfad funktioniert ebenfalls
ASF_PATH=/opt/Zielverzeichnis dotnet /opt/ASF/ArchiSteamFarm.dll #Identisch mit Umgebungsvariable
```

```text
├── /opt
│     ├── ASF
│     │     ├── ArchiSteamFarm.dll
│     │     └── ...
│     └── Zielverzeichnis
│           ├── config
│           ├── logs (generiert)
│           ├── plugins (optional)
│           ├── www (optional)
│           ├── log.txt (generiert)
│           └── NLog.config (optional)
└── ...
```

---

`--process-required` - Durch die Verwendung dieser Option wird das standardmäßige Herunterfahren von ASF deaktiviert, wenn keine Bots laufen. Dies ist besonders in Kombination mit der **[IPC](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC-de-DE)**-API nützlich, da die Mehrheit der Benutzer erwarten würden, dass ihr Webservice unabhängig von der Anzahl der aktivierten Bots ausgeführt wird. Wenn du die Option IPC verwendest oder anderweitig einen ASF-Prozess benötigst, welcher die ganze Zeit läuft, bis du ihn selbst beendest, ist dies die richtige Option.

Wenn du nicht vorhast IPC auszuführen, wird diese Option für dich ziemlich nutzlos sein, da du den Prozess bei Bedarf einfach wieder starten kannst (im Gegensatz zum Webserver von ASF, wo du ihn die ganze Zeit lauschen lassen musst um Befehle zu senden).

---

`--service`- Dieser Schalter wird hauptsächlich für unseren `systemd` service und erzwingt den Wert von `Headless` auf `true`. Unless you have a particular need, you should instead configure `Headless` property directly in your config. This switch is here so our `systemd` service won't need to touch your global config in order to adapt it to its own environment. Of course, if you have a similar need then you may also make use of this switch (otherwise you're better with global config property).

---

`--system-required` - Die Verwendung dieser Option veranlasst ASF, dem Betriebssystem zu signalisieren, dass das System für die gesamte Lebensdauer des Prozesses betriebsbereit sein muss. Derzeit hat diese Option nur auf Windows-Rechnern Auswirkungen, wo sie verhindert, dass dein System in den Ruhezustand wechselt, solange der Prozess läuft. This can be proven especially useful when farming on your PC or laptop during night, as ASF will be able to keep your system awake while it's farming, then, once ASF is done, it'll shutdown itself like usual, making your system allowed to enter into sleep mode again, therefore saving power immediately once farming is finished.

Bedenke, dass du für ein ordnungsgemäßes automatisches Herunterfahren von ASF weitere Einstellungen benötigst - vor allem `--process-required` sollte vermieden werden und alle deine Bots sollten `ShutdownOnFarmingFinished` aktiv haben. Natürlich ist das automatische Herunterfahren nur eine Möglichkeit für dieses Feature und keine Anforderung, da du dieses Attribut auch zusammen mit z.B. `--process-required` verwenden kannst, was dein System nach dem Start von ASF unbegrenzt wach hält.