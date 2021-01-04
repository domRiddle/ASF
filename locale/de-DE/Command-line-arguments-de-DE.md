# Befehlszeilenargumente

ASF unterstützt mehrere Befehlszeilenargumente, die die Programm-Runtime beeinflussen können. Diese können von fortgeschrittenen Benutzern verwendet werden, um festzulegen, wie das Programm arbeiten soll. Im Vergleich zum gängigen Standardweg über die Konfigurationsdatei `ASF.json` werden Befehlszeilenargumente für die Kerninitialisierung (z.B. `--path`), plattformspezifische Einstellungen (z.B. `--system-required`) oder sensible Daten (z.B. `--cryptkey`) verwendet.

* * *

## Nutzung

Die Nutzung ist abhängig vom verwendeten Betriebssystem und der ASF Variante.

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

Die Befehlszeilenargumente werden auch in allgemeinen Hilfsskripten wie zum Beispiel `ArchiSteamFarm.cmd` oder `ArchiSteamFarm.sh` unterstützt. Darüber hinaus kannst du bei Verwendung eines Hilfsskripts auch die Umgebungsvariable `ASF_ARGS` verwenden, wie es im Abschnitt **[Docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker-de-DE#befehlszeilenargumente)** beschrieben ist.

Wenn dein Argument Leerzeichen enthält, vergiss nicht, es in Anführungszeichen zu setzen. Diese zwei sind falsch:

```shell
./ArchiSteamFarm --path /home/archi/Meine Downloads/ASF # Falsch!
./ArchiSteamFarm --path=/home/archi/Meine Downloads/ASF # Falsch!
```

Aber diese beiden sind völlig in Ordnung:

```shell
./ArchiSteamFarm --path "/home/archi/Meine Downloads/ASF" # OK
./ArchiSteamFarm "--path=/home/archi/Meine Downloads/ASF" # OK
```

## Argumente

`--cryptkey <key>` oder `--cryptkey=<key>` - ASF startet mit dem benutzerdefinierten kryptographischen Schlüssel `<key>`. Diese Option wirkt sich auf die **[Sicherheit](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security-de-DE)** aus und veranlasst ASF, den von dir bereitgestellten `<key>` Schlüssel anstelle des standardmäßig fest in die ausführbare Datei einprogrammierten Schlüssels zu verwenden. Since this property affects default encryption key (for encrypting purposes) as well as salt (for hashing purposes), keep in mind that everything encrypted/hashed with this key will require it to be passed on each ASF run.

Aufgrund der Natur dieser Eigenschaft ist es auch möglich, einen cryptkey zu setzen, indem man die Umgebungsvariable `ASF_CRYPTKEY` deklariert, was für Personen, die sensible Details in den Prozessargumenten vermeiden wollen, besser geeignet sein kann.

* * *

`--ignore-unsupported-environment` - will cause ASF to ignore detection of unsupported environment, which normally is signalized with an error and forced exit. As of now, unsupported environment is classifed as running .NET Framework build on platform that could be running .NET Core build instead. Since we support `generic-netf` builds only in very limited scenarios (with **[Mono](https://www.mono-project.com)**), using it for other cases (e.g. for launching on `win-x64` platform) is not supported. Mehr Informationen finden Sie unter **[Kompatibilität](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility)**.

* * *

`--network-group <group>` oder `--network-group=<group>` - führt dazu, dass ASF seine Begrenzer mit einer benutzerdefinierten Netzwerkgruppe mit einem Wert `<group>` initialisiert. Diese Option wirkt sich auf die Ausführung von ASF in **[mehreren Instanzen](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#multiple-instances)** aus, indem signalisiert wird, dass diese Instanz nur von Instanzen abhängig ist, die dieselbe Netzwerkgruppe teilen und unabhängig vom Rest. In der Regel sollte diese Eigenschaft nur verwendet werden, wenn Sie ASF-Anfragen über einen benutzerdefinierten Mechanismus (z.b. verschiedene IP-Adressen) und wenn sie Netzwerkgruppen selbst einstellen möchten, ohne sich darauf zu verlassen, dass ASF dies automatisch macht (was derzeit nur `WebProxy` berücksichtigt). Beachten Sie, dass es sich bei der Verwendung einer benutzerdefinierten Netzwerkgruppe um einen eindeutigen Bezeichner innerhalb des lokalen Rechners handelt und ASF keine weiteren Details berücksichtigt, wie z.B. den Wert vom `WebProxy`, wodurch Sie z.B. zwei Instanzen mit unterschiedlichen `WebProxy` Werten starten können, die noch voneinander abhängig sind.

Aufgrund der Natur dieser Eigenschaft ist es auch möglich, den Wert zu setzen, indem man die Umgebungsvariable `ASF_NETWORK_GROUP` deklariert, was für Personen, die sensible Details in den Prozessargumenten vermeiden wollen, besser geeignet sein kann.

* * *

`--no-config-watch` - by default ASF sets up a `FileSystemWatcher` over your `config` directory in order to listen for events related to file changes, so it can interactively adapt to them. For example, this includes stopping bots on config deletion, restarting bot on config being changed, or loading keys into **[BGR](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer)** once you drop them into the `config` directory. This switch allows you to disable such behaviour, which will cause ASF to completely ignore all the changes in `config` directory, requiring from you to do such actions manually, if deemed appropriate. Usually we recommend to keep the config events enabled, but if you have a particular reason for disabling them and would instead prefer ASF to not do that, you can use this switch for achieving that purpose.

* * *

`--no-restart` - Diese Option wird hauptsächlich in unseren **[Docker](https://github.com/JustArchi/ArchiSteamFarm/wiki/Docker-de-DE)**-Containern genutzt und setzt `AutoRestart` auf `false`. Wenn du keinen besonderen Bedarf hast, solltest du stattdessen die Eigenschaft `AutoRestart` direkt in deiner Konfigurationsdatei setzen. Diese Option existiert nur damit unser Docker-Skript deine globale Konfiguration nicht modifizieren muss, um sie an die eigene Umgebung anzupassen. Natürlich, wenn Sie ASF in einem Skript ausführen, können Sie auch diesen Schalter verwenden (ansonsten sind Sie mit der globalen Konfigurationseigenschaft besser dran).

* * *

`--path <path>` oder `--path=<path>` - ASF wechselt beim Start immer in sein eigenes Verzeichnis. Wird dieser Parameter angegeben so wird ASF nach der Initialisierung zu dem gegebenem Programmverzeichnis navigieren, dies macht es Ihnen möglich andere Verzeichnisse wie z.B. `config`, `plugins` oder `www` (sowie auch die `NLog.config` Datei) für unterschiedliche Teile der Applikation zu nutzen. Das Kopieren der Binärdateien an diese Stellen ist dadurch nicht mehr nötig. Es kann besonders nützlich sein, wenn Sie die Binärdatei von der eigentlichen Konfiguration trennen möchten, wie es in einer Linux-ähnlichen Verpackung geschieht - so können Sie eine (aktuelle) Binärdatei mit mehreren verschiedenen Konfigurationen verwenden. Der Pfad kann entweder relativ zum aktuellen Ort der ASF-Binärdatei oder absolut sein. Bedenke auch, dass dieser Befehl auf einen neuen "ASF Homeordner" zeigt - ein Verzeichnis, welches die gleiche Struktur wie der ursprüngliche ASF Ordner hat, mit einem Verzeichnis config darin, siehe unteres Beispiel für eine Erklärung.

Aufgrund der Natur dieser Eigenschaft ist es auch möglich, den erwarteten Pfad zu setzen, indem man die Umgebungsvariable `ASF_PATH` deklariert, was für Personen, die sensible Details in den Prozessargumenten vermeiden wollen, besser geeignet sein kann.

Wenn Sie erwägen, dieses Kommandozeilenargument für die Ausführung mehrerer ASF-Instanzen zu verwenden, empfehlen wir Ihnen, unsere **[Kompatibilitätsseite](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#multiple-instances)** zu diesem Thema durchzulesen.

Beispiele:

```shell
dotnet /opt/ASF/ArchiSteamFarm.dll --path /opt/TargetDirectory # Absolute path
dotnet /opt/ASF/ArchiSteamFarm.dll --path ../TargetDirectory # Relative path works as well
ASF_PATH=/opt/TargetDirectory dotnet /opt/ASF/ArchiSteamFarm.dll # Same as env variable
```

```text
├── /opt
│     ├── ASF
│     │     ├── ArchiSteamFarm.dll
│     │     └── ...
│     └── Ziel-Verzeichnis
│           ├── config
│           ├── logs (generiert)
│           ├── plugins (optional)
│           ├── www (optional)
│           ├── log.txt (generiert)
│           └── NLog.config (optional)
└── ...
```

* * *

`--process-required` - Durch die Verwendung dieser Option wird das standardmäßige Herunterfahren von ASF deaktiviert, wenn keine Bots laufen. Dies ist besonders in Kombination mit der **[IPC](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC-de-DE)**-API nützlich, da die Mehrheit der Benutzer erwarten würden, dass ihr Webservice unabhängig von der Anzahl der aktivierten Bots ausgeführt wird. Wenn du die Option IPC verwendest oder anderweitig einen ASF-Prozess benötigst, welcher die ganze Zeit läuft, bis du ihn selbst beendest, ist dies die richtige Option.

Wenn du nicht vorhast IPC auszuführen, wird diese Option für dich ziemlich nutzlos sein, da du den Prozess bei Bedarf einfach wieder starten kannst (im Gegensatz zum Webserver von ASF, wo du ihn die ganze Zeit lauschen lassen musst um Befehle zu senden).

* * *

`--system-required` - Die Verwendung dieser Option veranlasst ASF, dem Betriebssystem zu signalisieren, dass das System für die gesamte Lebensdauer des Prozesses betriebsbereit sein muss. Derzeit hat diese Option nur auf Windows-Rechnern Auswirkungen, wo sie verhindert, dass dein System in den Ruhezustand wechselt, solange der Prozess läuft. Dies kann besonders nützlich sein, wenn du nachts auf deinem PC oder Laptop am Sammeln bist. ASF ist damit in der Lage dein System während des Sammelns wach zu halten und sich nach Beendigung des Sammelns wie gewohnt abzuschalten. So kann dein System wieder in den Ruhemodus wechseln und somit sofort nach Beendigung des Sammelns Strom sparen.

Bedenke, dass du für ein ordnungsgemäßes automatisches Herunterfahren von ASF weitere Einstellungen benötigst - vor allem `--process-required` sollte vermieden werden und alle deine Bots sollten `ShutdownOnFarmingFinished` aktiv haben. Natürlich ist das automatische Herunterfahren nur eine Möglichkeit für dieses Feature und keine Anforderung, da du dieses Attribut auch zusammen mit z.B. `--process-required` verwenden kannst, was dein System nach dem Start von ASF unbegrenzt wach hält.