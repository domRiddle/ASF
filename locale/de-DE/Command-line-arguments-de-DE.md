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

Linux/macOS:

```shell
./ArchiSteamFarm --argument --anderesArgument
```

Die Befehlszeilenargumente werden auch in allgemeinen Hilfsskripten wie zum Beispiel `ArchiSteamFarm.cmd` oder `ArchiSteamFarm.sh` unterstützt. Darüber hinaus können Sie auch die Umgebungsvariable `ASF_ARGS` verwenden, wie es in den Abschnitten **[Management](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Management#environment-variables)** und **[Docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker#command-line-arguments)** beschrieben ist.

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

`<key>` erfordert keine Voraussetzungen bezüglich der Länge oder Zeichen, jedoch empfehlen wir das Passwort etwa unter Verwendung von zufälligen Zeichen zu generieren (beispielsweise mit dem `tr -dc A-Za-z0-9 < /dev/urandom | head -c 32; echo` Kommando unter Linux).

Darüber hinaus gibt es auch zwei weitere Möglichkeiten, um diese Angaben bereitzustellen: `--cryptkey-file` und `--input-cryptkey`.

Aufgrund der Natur dieser Eigenschaft ist es auch möglich, einen cryptkey zu setzen, indem man die Umgebungsvariable `ASF_CRYPTKEY` deklariert, was für Personen, die sensible Details in den Prozessargumenten vermeiden wollen, besser geeignet sein kann.

---

`--cryptkey-file <path>` oder `--cryptkey-file=<path>` - ASF startet mit dem benutzerdefinierten kryptographischen Schlüssel, der aus der Datei `<path>` gelesen wird. Dies dient dem gleichen Zweck wie `--cryptkey <key>`, wie oben erklärt, mit dem Unterschied, dass mit dieser Eigenschaft der `<key>` aus dem angegebenen `<path>` gelesen wird.

Durch die Natur dieser Option ist, es möglich die Umgebungsvariable `ASF_CRYPTKEY_FILE` für den cryptkey zu bestimmen; welcher für jene angemessen ist, die sensible Details in den Verarbeitungsargumenten zu vermeiden.

---

`--ignore-unsupported-environment` - wird ASF dazu führen die Erkennung von nicht unterstützten Umgebungen zu ignorieren, die normalerweise mit einem Fehler signalisiert und das Beenden erzwingen. Nicht unterstützte Umgebung beinhalten zum Beispiel das Ausführen von .NET Framework Build auf Plattformen, die stattdessen .NET (Core) Build ausführen könnten. Während diese Option es ASF erlaubt, in solchen Szenarien zu laufen, sollten Sie beachten, dass wir dies offiziell nicht unterstützen und Sie dies vollständig **auf eigene Gefahr** tun. Ab heute können **alle** der nicht unterstützten Umgebungsszenarien korrigiert werden, wie zum Beispiel das Ausführen von `generic` Build statt `generic-netf`. Wir empfehlen Ihnen dringend, die offenen Probleme zu beheben, anstatt dieses Argument anzugeben.

---

`--input-cryptkey` - lässt ASF während des Starts nach `--cryptkey` fragen. Diese Option könnte für Sie nützlich sein; sofern es Ihr Wunsch ist, cryptkey nicht zu speichern (in Umgebungsvariablen oder in einer Datei), sondern stattdessen bei jedem ASF-Lauf manuell einzugeben.

---

`--minimized` - minimiert das ASF-Konsolenfenster direkt nach dem Start minimiert. Hauptsächlich in Autostart-Szenarien nützlich, aber kann auch sonst genutzt werden. Dieser Schalter funktioniert derzeit ausschließlich auf Windows-Geräten.

---

`--network-group <group>` oder `--network-group=<group>` - führt dazu, dass ASF seine Begrenzer mit einer benutzerdefinierten Netzwerkgruppe mit einem Wert `<group>` initialisiert. Diese Option wirkt sich auf die Ausführung von ASF in **[mehreren Instanzen](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Management#multiple-instances)** aus, indem signalisiert wird, dass diese Instanz nur von Instanzen abhängig ist, die dieselbe Netzwerkgruppe teilen und unabhängig vom Rest. In der Regel sollte diese Eigenschaft nur verwendet werden, wenn Sie ASF-Anfragen über einen benutzerdefinierten Mechanismus (z.b. verschiedene IP-Adressen) und eine Netzwerkgruppen selbst einstellen möchten, ohne sich darauf zu verlassen, dass ASF dies automatisch macht (dies berücksichtigt derzeit nur `WebProxy`). Beachten Sie, dass es sich bei der Verwendung einer benutzerdefinierten Netzwerkgruppe um einen eindeutigen Bezeichner innerhalb des lokalen Rechners handelt und ASF keine weiteren Details berücksichtigt, wie z. B. den Wert vom `WebProxy`, wodurch Sie z. B. zwei Instanzen mit unterschiedlichen `WebProxy` Werten starten können, die noch voneinander abhängig sind.

Aufgrund der Natur dieser Eigenschaft ist es auch möglich, den Wert zu setzen, indem man die Umgebungsvariable `ASF_NETWORK_GROUP` deklariert, was für Personen, die sensible Details in den Prozessargumenten vermeiden wollen, besser geeignet sein kann.

---

`--no-config-migrate` - ASF migriert standardmäßig Ihre Konfigurationsdateien automatisch in die neueste Syntax. Diese Option verhindert das. Migration beinhaltet die Umwandlung veralteter Eigenschaften in Neue, das Entfernen von Eigenschaften mit Standardwerten (da diese keine Wirkung haben), sowie die Bereinigung der Datei im Allgemeinen (Korrektur der Einrückung und ähnliches). Dies ist fast immer eine gute Idee, aber Sie könnten eine bestimmte Situation haben, in der Sie es vorziehen würden, wenn ASF die Konfigurationsdateien niemals automatisch überschreibt. Zum Beispiel: Vielleicht möchten Sie `chmod 400`auf Ihre Konfigurationsdateien anwenden (nur für den Eigentümer lesbar) oder `chattr +i` anwenden, was dazu führt, dass für jeden Schreibzugriff verweigert wird, z.B. als Sicherheitsmaßnahme. Wir empfehlen die Konfigurationsdatei-Migration aktiviert zu lassen, es sei denn Sie haben einen guten Grund diese zu deaktivieren, und wünschen, dass ASF dies nicht tut.

---

`--no-config-watch` - legt ASF standardmäßig einen `FileSystemWatcher` für Ihre `Konfiguration` fest, um auf Ereignisse im Zusammenhang mit Änderungen der Datei zu hören, sodass diese sich interaktiv anpasst. Dies beinhaltet beispielsweise das Stoppen von Bots beim Löschen der Konfiguration, den Neustart des Bots bei der Änderung der Konfiguration, oder laden von Schlüsseln in **[BGR](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer-de-DE#)** sobald Sie sie in das `Verzeichnis` eingegeben werden. Dieser Schalter das Verhalten zu deaktivieren, welches ASF dazu bringt, sämtliche Änderungen im`config`-Ordner komplett zu ignorieren. Folglich müssen Sie fortan diese Aktionen manuell durchführen, sollte dies erforderlich sein (meistens das Neustarten des Prozesses). Wir empfehlen die Konfigurationsereignisse aktiviert zu lassen; haben Sie jedoch einen guten Grund, um dies für ASF zu deaktivieren, so erreichen Sie dies mit diesem Schalter.

---

`--no-restart` - Diese Option wird hauptsächlich in unseren **[Docker](https://github.com/JustArchi/ArchiSteamFarm/wiki/Docker-de-DE)**-Containern genutzt und setzt `AutoRestart` auf `false`. Solange du keinen bestimmten Grund besitzt, solltest du stattdessen die `AutoRestart` Option in deiner Konfiguration verwenden. Dieser Schalter dient dazu, sodass unser Docker-Skript nicht Ihre globale Konfiguration berühren muss, um es an dessen eigene Umgebung anzupassen. Selbstverständlich können Sie diese Option auch verwenden, wenn Sie ASF in einem Skript ausführen; ansonsten ist die globalen Konfigurationseigenschaft besser geeignet.

---

`--no-damam-parental-generation` - standardmäßig wird ASF automatisch versuchen, die Steam-PINs der Eltern zu generieren, wie in **[`beschrieben. SteamParentalCode`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-de-de#steamparentalcode)** konfiguriert. Da dies jedoch eine übermäßige Menge an Betriebssystem-Ressourcen erfordern kann, erlaubt Ihnen dieser Schalter dieses Verhalten zu deaktivieren, was dazu führt, dass ASF die automatische Generierung überspringt, und stattdessen den Benutzer direkt nach PIN fragt. Dies würde sonst nur passieren, wenn die Auto-Generation versagt hat. Wir empfehlen die Generation aktiviert zu lassen, es sei denn Sie haben einen guten Grund diese zu deaktivieren, und wünschen, dass ASF dies nicht tut.

---

`--path <path>` oder `--path=<path>` - ASF wechselt beim Start immer in sein eigenes Verzeichnis. Wird dieser Parameter angegeben, so wird ASF nach der Initialisierung zu dem gegebenem Programmverzeichnis navigieren. Dies macht es Ihnen möglich andere Verzeichnisse wie z. B. `config`, `plugins` oder `www` (sowie auch die `NLog.config` Datei) für unterschiedliche Teile der Applikation zu nutzen. Das Kopieren der Binärdateien an diese Stellen ist dadurch nicht mehr nötig. Es kann besonders nützlich sein, wenn Sie die Binärdatei von der eigentlichen Konfiguration trennen möchten, wie es in einer Linux-ähnlichen Paketen geschieht. So können Sie eine (aktuelle) Binärdatei mit mehreren verschiedenen Konfigurationen verwenden. Der Pfad kann entweder relativ zum aktuellen Ort der ASF-Binärdatei oder absolut sein. Bedenke auch, dass dieser Befehl auf einen neuen "ASF-Homeordner" zeigt - ein Verzeichnis, welches die gleiche Struktur wie der ursprüngliche ASF-Ordner hat, mit einem Verzeichnis config darin, siehe unteres Beispiel für eine Erklärung.

Aufgrund der Natur dieser Eigenschaft ist es auch möglich, den erwarteten Pfad zu setzen, indem man die Umgebungsvariable `ASF_PATH` deklariert wird, was für Personen, die sensible Details in den Prozessargumenten vermeiden wollen, besser geeignet sein kann.

Sollten Sie erwägen, dieses Kommandozeilenargument für die Ausführung mehrerer ASF-Instanzen zu verwenden, empfehlen wir Ihnen, sich mit dem Thema **[Verwaltung](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Managemen-de-DE#multiple-instances)** zu befassen.

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

`--service`- Dieser Schalter wird hauptsächlich für unseren `systemd` service und erzwingt den Wert von `Headless` auf `true`. Wenn Sie keinen besonderen Grund haben, sollten Sie stattdessen die `headless` Eigenschaft direkt in Ihrer Konfiguration konfigurieren. Dieser Schalter existiert, damit unser `systemd` Dienst Ihre globale Konfiguration nicht berühren muss, um sich an seine eigene Umgebung anzupassen. Selbstverständlich können Sie diese Option auch verwenden, wenn Sie einen ähnlichen Anwendungsfall benötigen; ansonsten ist die globale Konfigurationseigenschaft besser geeignet.

---

`--system-required` - Die Verwendung dieser Option veranlasst ASF, dem Betriebssystem zu signalisieren, dass das System für die gesamte Lebensdauer des Prozesses betriebsbereit sein muss. Derzeit hat diese Option nur auf Windows-Rechnern Auswirkungen, wo sie verhindert, dass dein System in den Ruhezustand wechselt, solange der Prozess läuft. Dies kann besonders nützlich sein, wenn Sie nachts auf Ihrem PC oder Laptop am Sammeln sind. ASF ist damit in der Lage, das System während des Sammelns wach zu halten, und sich nach Beendigung des Sammelns wie gewohnt abzuschalten. So kann Ihr System wieder in den Ruhemodus wechseln und somit sofort nach Beendigung des Sammelns Strom sparen.

Bedenke, dass du für ein ordnungsgemäßes automatisches Herunterfahren von ASF weitere Einstellungen benötigst - vor allem `--process-required` sollte vermieden werden und alle deine Bots sollten `ShutdownOnFarmingFinished` aktiv haben. Natürlich ist das automatische Herunterfahren nur eine Möglichkeit für dieses Feature und keine Anforderung, da du dieses Attribut auch zusammen mit z.B. `--process-required` verwenden kannst, was dein System nach dem Start von ASF unbegrenzt wach hält.