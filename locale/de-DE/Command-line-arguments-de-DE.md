# Befehlszeilenargumente

ASF unterstützt mehrere Befehlszeilenargumente, die die Programmlaufzeit beeinflussen können. Diese können von fortgeschrittenen Benutzern verwendet werden, um festzulegen, wie das Programm arbeiten soll. Im Vergleich zum gängigen Standardweg über die Konfigurationsdatei `ASF.json` werden Befehlszeilenargumente für die Kerninitialisierung (z.B. `--path`), plattformspezifische Einstellungen (z.B. `--system-required`) oder sensible Daten (z.B. `--cryptkey`) verwendet.

* * *

## Nutzung

Die Nutzung ist abhängig vom verwendeten Betriebssystem und der ASF Variante.

Generisch:

```shell
dotnet ArchiSteamFarm.dll --argument --otherOne
```

Windows:

```powershell
.\ArchiSteamFarm.exe --argument --otherOne
```

Linux/OS X:

```shell
./ArchiSteamFarm --argument --otherOne
```

Die Befehlszeilenargumente werden ebenfalls in allgemeinen Hilfsskripten wie zum Beispiel `ArchiSteamFarm.cmd` oder `ArchiSteamFarm.sh` unterstützt. Darüber hinaus kannst du bei Verwendung eines Hilfsskripts auch die Umgebungsvariable `ASF_ARGS` verwenden, wie es im Abschnitt **[Docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker-de-DE#befehlszeilenargumente)** beschrieben ist.

Wenn dein Argument Leerzeichen enthält, vergiss nicht, es in Anführungszeichen zu setzen. Diese zwei sind falsch:

```shell
./ArchiSteamFarm --path /home/archi/Meine Downloads/ASF # Schlecht!
./ArchiSteamFarm --path=/home/archi/Meine Downloads/ASF # Schlecht!
```

Aber diese beiden sind völlig in Ordnung:

```shell
./ArchiSteamFarm --path "/home/archi/Meine Downloads/ASF" # OK
./ArchiSteamFarm "--path=/home/archi/Meine Downloads/ASF" # OK
```

## Argumente

`--cryptkey <key>` oder `--cryptkey=<key>` - ASF startet mit dem benutzerdefinierten kryptographischen Schlüssel `<key>`. Diese Option wirkt sich auf die **[Sicherheit](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security-de-DE)** aus und veranlasst ASF, den von dir bereitgestellten `<key>` Schlüssel anstelle des standardmäßig fest in die ausführbare Datei einprogrammierten Schlüssels zu verwenden. Beachte, dass Passwörter, welche mit diesem Schlüssel verschlüsselt sind, diesen bei jedem Start von ASF erfordern.

* * *

`--no-restart` - dieser Schalter wird hauptsächlich von unseren **[Docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker-de-DE)** Containern verwendet und setzt `AutoRestart` auf `false`. Wenn du keinen besonderen Bedarf hast, solltest du stattdessen die Eigenschaft `AutoRestart` direkt in deiner Konfiguration setzen. Dieser Schalter existiert nur damit unser Docker-Skript deine globale Konfiguration nicht modifizieren muss, um sie an die eigene Umgebung anzupassen. Wenn du ASF innerhalb eines Skripts ausführst, kannst du natürlich auch diesen Schalter verwenden (ansonsten ist es besser, wenn du die globale Konfigurationseigenschaft verwendest).

* * *

`--path <path>` oder `--path=<path>` - ASF navigiert beim Start immer in sein eigenes Verzeichnis. Durch die Angabe dieses Arguments navigiert ASF nach der Initialisierung zu dem angegebenen Verzeichnis, wodurch du den benutzerdefinierten Pfad für das Verzeichnis `config` (und optional auch `www`) verwenden kannst, ohne dass du Binärdateien an der gleichen Stelle duplizieren musst. Es könnte besonders nützlich sein, wenn du die Binärdatei von der eigentlichen Konfiguration trennst, wie es in einem Linux-ähnlichen Paket geschieht - auf diese Weise kannst du eine (aktuelle) Binärdatei mit mehreren verschiedenen Setups verwenden. Der Pfad kann entweder relativ zum aktuellen Ort der ASF-Binärdatei oder absolut sein. Wenn du mehrere Instanzen derselben Binärdatei ausführst, denke daran, dass du normalerweise automatische Updates deaktivieren solltest, da es keine Synchronisation zwischen ihnen gibt. Bedenke auch, dass dieser Befehl auf das neue "ASF Heim" zeigt - das Verzeichnis, welches die gleiche Struktur wie das ursprüngliche ASF hat, mit dem Verzeichnis `config` darin.

Beispiel:

```shell
dotnet /opt/ASF/ArchiSteamFarm.dll --path /opt/TargetDirectory # Absoluter Pfad
dotnet /opt/ASF/ArchiSteamFarm.dll --path ../TargetDirectory # Relativer Pfad funktioniere auch
```

    ├── /opt
    │     ├── ASF
    │     │     ├── ArchiSteamFarm.dll
    │     │     └── ...
    │     └── Zielverzeichnis
    │           └── config
    └── ...
    

* * *

`--process-required` - Durch die Deklaration dieses Schalters wird das standardmäßige Herunterfahren von ASF deaktiviert, wenn keine Bots laufen. Dies ist besonders in Kombination mit der **[IPC](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC-de-DE)**-API nützlich, da die Mehrheit der Benutzer erwarten würden, dass ihr Webservice unabhängig von der Anzahl der aktivierten Bots ausgeführt wird. Wenn du die Option IPC verwendest oder anderweitig einen ASF-Prozess benötigst, welcher die ganze Zeit läuft, bis du ihn selbst beendest, ist dies die richtige Option.

Wenn du nicht vorhast IPC auszuführen, wird diese Option für dich ziemlich nutzlos sein, da du den Prozess bei Bedarf einfach wieder starten kannst (im Gegensatz zum Webserver von ASF, wo du ihn die ganze Zeit lauschen lassen musst um Befehle zu senden).

* * *

`--system-required` - die Deklaration dieses Schalters veranlasst ASF, dem Betriebssystem zu signalisieren, dass das System für die gesamte Lebensdauer des Prozesses betriebsbereit sein muss. Derzeit hat dieser Schalter nur auf Windows-Rechnern Auswirkungen, wo er verhindert, dass dein System in den Ruhezustand wechselt, solange der Prozess läuft. Dies kann besonders nützlich sein, wenn du nachts auf deinem PC oder Laptop am Sammeln bist, da ASF in der Lage ist, dein System während des Sammelns wach zu halten und sich dann bei Beendigung von ASF wie gewohnt abschaltet, so dass dein System wieder in den Ruhemodus wechseln kann und somit sofort nach Beendigung des Sammelns Strom spart.

Bedenke, dass du für ein ordnungsgemäßes automatisches Herunterfahren von ASF weitere Einstellungen benötigst - insbesondere das Vermeiden von `--process-required` und das Sicherstellen, dass alle deine Bots `ShutdownOnFarmingFinished` folgen. Natürlich ist das automatische Herunterfahren nur eine Möglichkeit für dieses Feature und keine Anforderung, da du dieses Attribut auch zusammen mit z.B. `--process-required` verwenden kannst, was dein System nach dem Start von ASF unbegrenzt wach hält.