# Befehlszeilenargumente

ASF unterstützt mehrere Befehlszeilenargumente, die die Programmlaufzeit beeinflussen können. Diese können von fortgeschrittenen Benutzern verwendet werden, um festzulegen, wie das Programm arbeiten soll. Im Vergleich zum gängigen Standardweg über die Konfigurationsdatei `ASF.json` werden Befehlszeilenargumente für die Kerninitialisierung (z.B. `--path`), plattformspezifische Einstellungen (z.B. `--system-required`) oder sensible Daten (z.B. `--cryptkey`) verwendet.

* * *

## Gebrauchsweise

Die Verwendung ist abhängig vom verwendeten Betriebssystem und der ASF Variante.

Allgemein:

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

Die Befehlszeilenargumente werden ebenfalls in allgemeinen Hilfsskripten wie zum Beispiel `ArchiSteamFarm.cmd` oder `ArchiSteamFarm.sh` unterstützt. Darüber hinaus kannst du bei Verwendung eines Hilfsskripts auch die Umgebungsvariable `ASF_ARGS` verwenden, wie es im Abschnitt **[Docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker#command-line-arguments)** beschrieben ist.

Wenn Ihr Argument Leerzeichen enthalten sollte, so ist es in Anführungszeichen zu setzen. Diese zwei sind falsch:

```shell
./ArchiSteamFarm --path /home/archi/Meine Downloads/ASF # Schlecht!
./ArchiSteamFarm --path=/home/archi/My Downloads/ASF # Schlecht!
```

Aber diese beiden sind völlig in Ordnung:

```shell
./ArchiSteamFarm --path "/home/archi/My Downloads/ASF" # OK
./ArchiSteamFarm "--path=/home/archi/My Downloads/ASF" # OK
```

## Argumente

`--cryptkey <key>` oder `--cryptkey=<key>` - ASF startet mit dem benutzerdefinierten kryptographischen Schlüssel `<key>`. Diese Option betrifft die **[Sicherheit](https://github.com/JustArchi/ArchiSteamFarm/wiki/Security-de-DE)** und führt zur Verwendung des angegebenen Passwords `<key>` anstatt eines standardmäßig in die ausführbare Datei einprogrammierten. Beachte, dass Passwörter, welche mit diesem Schlüssel verschlüsselt sind, diesen bei jedem ASF Start erfordern.

* * *

`--no-restart` - Diese Option wird hauptsächlich in unseren **[Docker](https://github.com/JustArchi/ArchiSteamFarm/wiki/Docker-de-DE)**-Containern genutzt und setzt `AutoRestart` auf `false`. Wenn du keinen besonderen Bedarf hast, solltest du stattdessen die Eigenschaft `AutoRestart` direkt in deiner Konfiguration setzen. Dieser Schalter existiert nur damit unser Docker-Skript deine globale Konfiguration nicht modifizieren muss, um sie an die eigene Umgebung anzupassen. Wenn du ASF innerhalb eines Skripts ausführst, kannst du natürlich auch diesen Schalter verwenden (ansonsten ist es besser, wenn du die globale Konfigurationseigenschaft verwendest).

* * *

`--path <path>` oder `--path=<path>` - ASF navigiert beim Start immer in den angegebenen Dateiordner. By specifying this argument, ASF will navigate to given directory after initialization, which allows you to use custom path for `config` (and optionally also `www`) directory without a need of duplicating binary in the same place. Es könnte besonders nützlich sein, wenn du die Binärdatei von der eigentlichen Konfiguration trennst, wie es in einem Linux-ähnlichen Paket geschieht - auf diese Weise kannst du eine (aktuelle) Binärdatei mit mehreren verschiedenen Setups verwenden. Der Pfad kann entweder relativ zum aktuellen Ort der ASF-Binärdatei oder absolut sein. Wenn du mehrere Instanzen derselben Binärdatei ausführst, denke daran, dass du normalerweise automatische Updates deaktivieren solltest, da es keine Synchronisation zwischen ihnen gibt. Beachte bitte auch, dass dieser Befehl auf das neue "ASF home" zeigt - der Dateiordner, welcher die gleiche Struktur wie das ursprüngliche ASF hat, mit dem enthaltenen Dateiordner `config`.

Beispiel:

```shell
dotnet /opt/ASF/ArchiSteamFarm.dll --path /opt/TargetDirectory # Absoluter Pfad
dotnet /opt/ASF/ArchiSteamFarm.dll --path ../TargetDirectory # Relative Pfade funktioniere auch
```

    ├── /opt
    │     ├── ASF
    │     │     ├── ArchiSteamFarm.dll
    │     │     └── ...
    │     └── Zieldateiordner
    │           └── config
    └── ...
    

* * *

`--process-required` - Durch die Deklaration dieses Schalters wird das standardmäßige Herunterfahren von ASF deaktiviert, wenn keine Bots laufen. Das Nicht-Beenden ist besonders nützlich in Kombination mit der **[IPC](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC-de-DE)**-API, von welcher die Mehrheit der Benutzer erwarten würde, dass ihr Server unabhängig von der Anzahl der aktivierten Bots läuft. Wenn du die Option IPC verwendest oder anderweitig einen ASF-Prozess benötigst, welcher die ganze Zeit läuft, bis du ihn selbst beendest, ist dies die richtige Option.

Wenn du nicht vorhast, die IPC auszuführen, ist diese Option für dich ziemlich nutzlos, da du den Prozess bei Bedarf einfach neu starten kannst (im Gegensatz zu ASF's Webserver, wo Sie ihn ständig hören müssen, um Befehle zu senden).

* * *

`--System-benötigt` - Deklarieren des Schalters wird ASF veranlassen dem Betriebssystem zu signalisieren, der Prozess benötige ein betriebsbereites System während der ganzen Lebenszeit. Dieser Schalter zeigt derzeit eine Wirkung nur auf Windows-Rechnern, auf welchen er es deinem System verbietet, in den Ruhezustand zu wechseln, solange der Prozess läuft. Dies kann besonders nützlich sein, wenn du nachts auf deinem PC oder Laptop am Sammeln bist, da ASF in der Lage ist, dein System während des Sammelns wach zu halten, und sich dann bei Beendigung von ASF wie gewohnt abschaltet, so dass dein System wieder in den Ruhemodus wechseln kann und somit sofort nach Beendigung des Sammelns Strom spart.

Beachte bitte, dass für das ordnungsmäßige automatische Beenden von ASF weitere Einstellungen korrekt vorgenommen sein müssen - vor allem das `--process-required` vermieden wird und das alle deine Bots `ShutdownOnFarmingFinished` befolgen. Natürlich ist das automatische Beenden nur eine Verwendungsmöglichkeit für diese Funktion, keine Notwendigkeit, denn du kannst diesen Schalter auch z.B. zusammen mit `--process-required` nutzen, wodurch dein System faktisch für immer nach dem Starten von ASF wach bleibt.