# Verwaltung

Dieser Abschnitt beschäftigt sich mit dem Thema, den ASF-Prozess optimal zu verwalten. Obwohl es für die Nutzung nicht zwingend vorgeschrieben ist, enthält es eine Reihe von Tipps, Tricks und bewährten Praktiken, die wir teilen möchten, speziell für Systemadministratoren, Menschen, die ASF sowohl für den Einsatz in externen Repositories, als auch für fortgeschrittene Benutzer verpacken.

---

## `systemd` Dienst für Linux

In den `generic` und `Linux` Varianten kommt ASF mit der Datei `ArchiSteamFarm@.service`, welche eine Konfigurationsdatei des Dienstes für **[`systemd`](https://systemd.io)** ist. Wenn Sie ASF als Dienst ausführen möchten, zum Beispiel um ihn nach dem Start Ihres Gerätes automatisch auszuführen, dann ist ein richtiger `systemd` Dienst wohl der beste Weg, dies zu erreichen. Daher empfehlen wir dies dringend, anstatt sie durch `nohup`, `screen` oder ähnlichem selbst zu verwalten.

Zuerst erstellen Sie das Konto für den Benutzer, unter dem Sie ASF betreiben möchten (sofern es noch nicht existiert). Wir verwenden für dieses Beispiel den Benutzer `asf`. Wenn Sie sich für einen anderen entschieden haben, müssen Sie `asf` in all unseren Beispielen durch Ihren ausgewählten Benutzer ersetzen. Unser Dienst erlaubt es Ihnen niemals, ASF mit `root` auszuführen, da es als **[schlechte Praxis](#f%C3%BChren-sie-asf-niemals-als-administrator-aus)** gilt.

```sh
su # ODER sudo -i, um in die Root-Shell zu gelangen
useradd -m asf # Erstellen Sie ein Konto unter dem Sie ASF ausführen wollen
```

Als nächstes entpacken Sie ASF in den Ordner `/home/asf/ArchiSteamFarm`. Die Ordnerstruktur ist wichtig für unsere Dienst-Einheit; es sollte der Ordner `ArchiSteamFarm` in Ihrem Startverzeichnis `$HOME` sein; also `/home/<user>`. Wenn Sie alles richtig gemacht haben, gibt es die Datei `/home/asf/ArchiSteamFarm/ArchiSteamFarm@.service`. Wenn Sie die Variante `linux` verwenden und die Datei nicht unter Linux entpackt haben (sondern zum Beispiel die Dateiübertragung von Windows nutzten), dann müssen Sie auch `chmod +x /home/asf/ArchiSteamFarm/ArchiSteamFarm` verwenden.

Wir werden alle folgenden Aktionen als `root` ausführen. Bitte wechseln Sie in der Shell mit` su` oder `sudo -i`.

Zunächst ist es eine gute Idee, sicherzustellen, dass unser Ordner noch zu unserem Benutzer `asf` gehört; dazu führen wir einmalig den Befehl `chown -hR asf:asf /home/asf/ArchiSteamFarm` aus. Die Berechtigungen könnten falsch sein, z. B. wenn Sie die Zip-Datei als `root` heruntergeladen und/oder entpackt haben.

Außerdem, falls Sie eine generische ASF-Variante verwenden, müssen Sie sicherstellen, dass der Befehl `dotnet` erkannt wird und innerhalb eines der Standardorte: `/usr/local/bin`, `/usr/bin` oder `/bin` ist. Für unseren systemd-Dienst ist der Befehl `dotnet /path/to/ArchiSteamFarm.dll` erforderlich. Überprüfen Sie, ob `dotnet --info` für Sie funktioniert; falls ja, geben Sie `command -v dotnet` ein, um herauszufinden, wo es sich befindet. Falls Sie den offiziellen Installationsassistenten verwendet haben, sollte es sich wie gewünscht entweder unter `usr/bin/dotnet` oder einem der beiden anderen Verzeichnisse vorhanden sein. Bei Verwendung eines benutzerdefinierten Ortes (wie z.B. code/usr/share/dotnet/dotnet/code) erstellen wir dafür einen <a href="https://de.wikipedia.org/wiki/Symbolische_Verkn%C3%BCpfung"Symlink</a> mit `ln -s "$(command -v dotnet)" /usr/bin/dotnet`. Nun sollte `command -v dotnet` `/usr/bin/dotnet` anzeigen, womit auch unsere systemd-Einheit funktioniert. Wenn Sie eine OS-spezifische Version verwenden, müssen Sie sich keine Gedanken darüber machen.

Als Nächstes `cd /etc/systemd/system` und führen Sie `ln -s /home/asf/ArchiSteamFarm/ArchiSteamFarm\@.service .` aus; damit ein symbolischer Link zu unserer Service-Deklaration erstellt und in `systemd` registriert wird. Der symbolische Link wird es ASF erlauben, Ihre `systemd` Einheit automatisch als Teil des ASF Updates zu aktualisieren – je nach Situation. Sie können diesen Ansatz verwenden oder einfach `cp` die Datei verwenden und sie selbst verwalten, sofern Sie möchten.

Stellen Sie anschließend sicher, dass `systemd` unseren Dienst erkennt:

```
systemctl status ArchiSteamFarm@asf

○ ArchiSteamFarm@asf.service – ArchiSteamFarm Service (on asf)
   Loaded: loaded (/etc/systemd/system/ArchiSteamFarm@.service; disabled; vendor preset: enabled)
   Active: inactive (dead)
    Docs: https://github.com/JustArchiNET/ArchiSteamFarm/wiki
```

Achten Sie besonders auf den Benutzer, den wir nach `@` deklarieren, es ist in unserem Fall `asf`. Unsere System-Service-Einheit bittet Sie, den Benutzer anzugeben. Dies wird sowohl den genauen Ort der Binärdatei `/home/<user>/ArchiSteamFarm`, als auch den eigentlichen systemd-Startprozess des Benutzers beeinflussen.

Wenn systemd die Ausgabe ähnlich wie oben zurückgibt, ist alles in Ordnung, und wir sind fast fertig. Jetzt bleibt uns nur noch, unseren Dienst mit unserem gewählten Benutzer zu starten: `systemctl start ArchiSteamFarm@asf`. Warten Sie ein oder zwei Sekunden, um den Status erneut zu überprüfen:

```
systemctl status ArchiSteamFarm@asf

● ArchiSteamFarm@asf.service – ArchiSteamFarm Service (on asf)
   Loaded: loaded (/etc/systemd/system/ArchiSteamFarm@.service; disabled; vendor preset: enabled)
   Active: active (running) since (...)
       Docs: https://github.com/JustArchiNET/ArchiSteamFarm/wiki
  Main PID: (...)
(...)
```

If `systemd` states `active (running)`, it means everything went well, and you can verify that ASF process should be up and running, for example with `journalctl -r`, as ASF by default also writes to its console output, which is recorded by `systemd`. If you're satisfied with the setup you have right now, you can tell `systemd` to automatically start your service during boot, by executing `systemctl enable ArchiSteamFarm@asf` command. Das war’s!

If by any chance you'd like to stop the process, simply execute `systemctl stop ArchiSteamFarm@asf`. Likewise, if you want to disable ASF from being started automatically on boot, `systemctl disable ArchiSteamFarm@asf` will do that for you, it's very simple.

Please note that, as there is no standard input enabled for our `systemd` service, you won't be able to input your details through the console in usual way. Running through `systemd` is equivalent to specifying **[`Headless: true`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#headless)** setting and comes with all its implications. Fortunately for you, it's very easy to manage your ASF through **[ASF-ui](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#asf-ui)**, which we recommend in case you need to supply additional details during login or otherwise manage your ASF process further.

### Umgebungsvariablen

Es ist möglich, zusätzliche Umgebungsvariablen für unseren `systemd`-Dienst bereitzustellen. Das ist für Sie interessant, wenn Sie zum Beispiel ein benutzerdefiniertes `--cryptkey` **[Befehlszeilenargument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments-de-DE#argumente)**verwenden möchten – spezifizieren Sie daher `ASF_CRYPTKEY` Umgebungsvariable.

Um benutzerdefinierte Umgebungsvariablen bereitzustellen, muss der Ordner `/etc/asf` (falls er nicht existiert), mit `mkdir -p /etc/asf` erstellt werden. Wir empfehlen `chown -hR root:root /etc/asf && chmod 700 /etc/asf` um sicherzustellen, dass nur der Benutzer `root` Zugang zum Lesen dieser Dateien hat; da diese empfindliche Eigenschaften (z. B. `ASF_CRYPTKEY`) enthalten könnten. Afterwards, write to a `/etc/asf/<user>` file, where `<user>` is the user you're running the service under (`asf` in our example above, so `/etc/asf/asf`).

Die Datei sollte alle Umgebungsvariablen enthalten, die Sie dem Prozess zur Verfügung stellen möchten. Diejenigen, die keine eigene Umgebungsvariable haben, können in `ASF_ARGS` deklariert werden:

```sh
# Definieren Sie jene die Sie tatsächlich benötigen
ASF_ARGS="--no-config-migrate --no-config-watch"
ASF_CRYPTKEY="my_super_important_secret_cryptkey"
ASF_NETWORK_GROUP="my_network_group"

# Und alle anderen in die Sie interessiert sind
```

### Überschreibung eines Teils der "Service unit" (Dienstverwaltung)

Dank der Flexibilität von `systemd` ist es möglich, einen Teil der ASF-Einheit zu überschreiben, während dabei die Originaldatei zu bewahrt wird und ASF zum Beispiel als Teil von Auto-Updates sich zu aktualisieren.

In this example, we'd like to modify default ASF `systemd` unit behaviour from restarting only on success, to restarting also on fatal crashes. In order to do so, we'll override `Restart` property under `[Service]` from default of `on-success` to `always`. Simply execute `systemctl edit ArchiSteamFarm@asf`, naturally replacing `asf` with the target user of your service. Then add your changes as indicated by `systemd` in proper section:

```sh
### Bearbeite /etc/systemd/system/ArchiSteamFarm@asf.service.d/override.conf
### Alles zwischen diesem Kommentar und unten wird zur neuen Datei

[Service]
Restart=always

### Zeilen werden verworfen

### /etc/systemd/system/ArchiSteamFarm@asf.service
# [Install]
# WantedBy=multi-user.target
# 
# [Service]
# EnvironmentFile=-/etc/asf/%i
# ExecStart=dotnet /home/%i/ArchiSteamFarm/ArchiSteamFarm.dll --no-restart --process-required --service --system-required
# Restart=on-success
# RestartSec=1s
# SyslogIdentifier=asf-%i
# User=%i
# (...)
```

And that's it, now your unit acts the same as if it had only `Restart=always` under `[Service]`.

Of course, alternative is to `cp` the file and manage it yourself, but this allows you for flexible changes even if you decided to keep original ASF unit, for example with a symlink.

---

## Führen Sie ASF niemals als Administrator aus!

ASF includes its own validation whether the process is being run as administrator (`root`) or not. Running as root is **not** required for any kind of operation done by the ASF process, assuming properly configured environment it's operating in, and therefore should be regarded as a **bad practice**. Es ist wichtig, dass ASF unter Windows niemals mit der Einstellung „als Administrator ausführen“ ausgeführt wird und unter Unix ASF sollte ein eigenes Benutzerkonto für sich selbst existieren, oder wiederverwenden Sie Ihr eigenes im Falle eines Desktop-Systems.

Für weitere Erläuterungen dazu, *warum* wir die Ausführung von ASF als root nicht empfehlen, können Sie **[Superuser](https://stackovercoder.com.de/superuser/218379/why-is-it-bad-to-run-as-root)** und andere Ressourcen nachschlagen. Falls Sie weiterhin nicht überzeugt sind, fragen Sie sich, was mit Ihrem Gerät passieren würde, wenn der ASF-Prozess direkt nach dem Start den Befehl `rm -rf /*` ausführt.

### Ich nutze `root`, weil ASF nicht in seine Dateien schreiben kann

Das bedeutet, dass Sie falsch konfigurierte Datei-Berechtigungen haben, auf die ASF zugreifen möchte. You should login as `root` account (either with `su` or `sudo -i`) and then **correct** the permissions by issuing `chown -hR asf:asf /path/to/ASF` command, substituting `asf:asf` with the user that you'll run ASF under, and `/path/to/ASF` accordingly. If by any chance you're using custom `--path` telling ASF user to use the different directory, you should execute the same command again for that path as well.

Danach sollten Sie keine Schwierigkeiten mehr haben, die damit zusammenhängen, dass ASF nicht in der Lage ist, über seine eigenen Dateien zu schreiben; weil Sie soeben den Besitzer von allem, was ASF interessiert, für den Benutzer geändert haben.

### Ich verwende `root`, weil ich nicht weiß, wie ich es sonst tun soll

```sh
su # oder sudo -i, um in die Root-Shell zu gelangen
useradd -m asf # Erstelle einen Account den Sie unter
chown -hR asf:asf /path/zu/ASF ausführen möchten # Stellen Sie sicher dass dein neuer Benutzer Zugriff auf das ASF-Verzeichnis hat
su asf -c /path/zu/ASF/ArchiSteamFarm hat # Oder sudo -u asf /path/zu/ASF/ArchiSteamFarm um das Programm tatsächlich unter Ihrem Benutzer zu starten
```

Dies würde manuell erfolgen, es ist viel einfacher, unseren **[`systemd`-Dienst](#systemd-Dienst-f%C3%BCr-linux)** (Erläuterung oben) zu nutzen.

### Ich weiß es besser und möchte trotzdem als `root` nutzen

ASF hält Sie nicht zwingend davon ab, dies zu tun, und zeigt nur kurzfristig eine Warnung an. Seien Sie einfach nicht schockiert, wenn es eines Tages aufgrund eines Fehlers im Programm Ihr gesamtes Betriebssystem mit dem vollständigen Datenverlust sprengt – Sie wurden gewarnt!

---

## Mehrere Instanzen

ASF unterstützt das Ausführen mehrerer Instanzen des Prozesses auf derselben Maschine. Die Instanzen können komplett eigenständig oder von demselben binären Speicherort abgeleitet werden. In diesem Fall sollten Sie sie mit einem anderen **[Kommandozeilenargument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments-de-DE)** `--path` ausführen.

Wenn Sie mehrere Instanzen von derselben Binärdatei ausführen, ist zu beachten, dass das normalerweise automatische Updates deaktiviert werden sollte, da es keine Synchronisation zwischen diese im Bezug auf Auto-Updates gibt. Wenn Sie automatische Updates aktivieren möchten, empfehlen wir Standalone-Instanzen aber Sie können trotzdem dafür sorgen, dass Updates funktionieren, solange Sie sicherstellen können, dass alle anderen ASF-Instanzen geschlossen sind.

ASF wird sein Bestes tun, um eine minimale Menge an OS-übergreifender Kommunikation mit anderen ASF-Instanzen zu pflegen. Dazu gehört die Überprüfung des Konfigurationsverzeichnisses mit denen anderer Instanzen, sowie die Freigabe prozessweiter Begrenzungen mit der **[globaler Konfigurationsvariable](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-de-DE#globale-konfiguration)** `*LimiterDelay`, um sicherzustellen, dass das Ausführen mehrerer ASF-Instanzen kein Anfragelimit auslöst. In Bezug auf technische Aspekte verwenden alle Plattformen unseren dedizierten Mechanismus von benutzerdefinierten ASF-Dateisperren, die im temporären Verzeichnis erstellt wurden, `C:\Users\<YourUser>\AppData\Local\Temp\ASF` unter Windows, und `/tmp/ASF` unter Unix.

Es ist nicht erforderlich die gleiche `*LimiterDelay` Eigenschaften zu teilen um ASF Instanzen auszuführen, sie können verschiedene Werte verwenden, da jeder ASF nach dem Erwerb der Sperre seine eigene konfigurierte Verzögerung zur Release-Zeit hinzufügt. Wenn `*LimiterDelay` auf `0` gesetzt ist, wird die ASF-Instanz das Warten auf die Sperre der angegebenen Ressource, die mit anderen Instanzen geteilt wird, überspringen (was möglicherweise immer noch eine gemeinsame Sperre untereinander aufrecht erhalten könnte). Wenn auf einen beliebigen anderen Wert gesetzt, wird ASF sich korrekt mit anderen ASF-Instanzen synchronisieren, und wartet bis es an der Reihe ist, bevor es dann nach konfigurierter Verzögerung die Sperre aufhebt, wodurch andere Instanzen fortfahren können.

ASF berücksichtigt die `WebProxy` Einstellung bei der Entscheidung über den gemeinsamen Anwendungsbereich, was bedeutet, dass zwei ASF-Instanzen, die verschiedene `WebProxy` Konfigurationen verwenden, Ihre Begrenzungen nicht untereinander teilen werden. Dies wird implementiert, um `WebProxy` zu ermöglichen, ohne übermäßige Verzögerungen zu arbeiten, wie von verschiedenen Netzwerkschnittstellen erwartet. Dies sollte für die Mehrzahl der Anwendungsfälle gut genug sein. Wenn Sie jedoch eine spezifische Benutzerkonfiguration haben, in der Sie z. B. Anfragen über eine bestimmte Netzwerkgruppe weiterleiten, können Sie das **[Kommandozeilenargument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments-de-DE)**`--network-group` angeben, welches Ihnen erlaubt, eine ASF-Gruppe zu deklarieren, die mit dieser Instanz synchronisiert wird. Beachten Sie, dass ausschließlich benutzerdefinierte Netzwerkgruppen verwendet werden. Das bedeutet, dass ASF `WebProxy` nicht mehr zur Bestimmung der richtigen Gruppe verwenden wird, da Sie für die Gruppierung in diesem Fall verantwortlich sind.

Wenn Sie unseren **[`systemd`-Dienst](#systemd-Dienst-f%C3%BCr-linux)** wie oben erläutert für mehrere ASF-Instanzen verwenden möchten, ist dies sehr einfach. Verwenden Sie einfach einen anderen Benutzer für unsere `ArchiSteamFarm@` Dienst-Deklaration und folgen Sie mit den verbleibenden Schritten. Dies entspricht dem Ausführen mehrerer ASF-Instanzen mit unterschiedlichen Binärdateien, sodass diese sich auch unabhängig voneinander automatisch aktualisieren und funktionieren können.