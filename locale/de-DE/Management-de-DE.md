# Verwaltung

Dieser Abschnitt beschäftigt sich mit dem Thema, den ASF-Prozess optimal zu verwalten. Obwohl es für die Nutzung nicht zwingend vorgeschrieben ist, enthält es eine Reihe von Tipps, Tricks und bewährten Praktiken, die wir teilen möchten, speziell für Systemadministratoren, Menschen, die ASF sowohl für den Einsatz in externen Repositories, als auch für fortgeschrittene Benutzer verpacken.

---

## `systemd` Dienst für Linux

In den `generic` und `Linux` Varianten kommt ASF mit der Datei `ArchiSteamFarm@.service`, welche eine Konfigurationsdatei des Dienstes für **[`systemd`](https://systemd.io)** ist. Wenn Sie ASF als Dienst ausführen möchten, zum Beispiel um ihn nach dem Start Ihres Rechners automatisch zu starten, dann ist ein richtiger `systemd` Dienst wohl der beste Weg, dies zu erreichen. Daher empfehlen wir dies dringend, anstatt sie durch `nohup`, `screen` oder ähnlichem selbst zu verwalten.

Zuerst erstellen Sie das Konto für den Benutzer, unter dem Sie ASF betreiben möchten, sofern es noch nicht existiert. Wir verwenden für dieses Beispiel den Benutzer `asf`, wenn Sie sich für einen anderen entschieden haben Sie müssen `asf` in all unseren Beispielen durch Ihren ausgewählten Benutzer ersetzen. Unser Service erlaubt es Ihnen niemals, ASF mit `root` auszuführen, da es als **[schlechte Praxis](#f%C3%BChren-sie-asf-niemals-als-administrator-aus)** gilt.

```sh
su # ODER sudo -i, um in die Root-Shell zu gelangen
useradd -m asf # Erstellen Sie ein Konto unter dem Sie ASF ausführen wollen
```

Als nächstes entpacken Sie ASF in den Ordner `/home/asf/ArchiSteamFarm`. Die Ordnerstruktur ist wichtig für unsere Service-Einheit, es sollte in Ihrem Ordner `ArchiSteamFarm` Ordner `$HOME` sein; also `/home/<user>`. Wenn Sie alles richtig gemacht haben, gibt es `/home/asf/ArchiSteamFarm/ArchiSteamFarm@.service` Datei. Wenn Sie `linux` Variante verwenden und die Datei nicht unter Linux entpackt haben, sondern zum Beispiel die verwendete Dateiübertragung von Windows, dann müssen Sie auch `chmod +x /home/asf/ArchiSteamFarm/ArchiSteamFarm` verwenden.

Wir werden alle folgenden Aktionen als `root` ausführen, also gehen Sie zur Shell mit `su` oder `sudo -i`.

Zunächst ist es eine gute Idee, sicherzustellen, dass unser Ordner noch zu unserem `asf` Benutzer gehört, `chown -hR asf:asf /home/asf/ArchiSteamFarm` ausgeführt, wird es tun. Die Berechtigungen könnten falsch sein, z.B. wenn Sie die Zip-Datei als `root` heruntergeladen und/oder entpackt haben.

Zweitens, wenn Sie eine generische ASF-Variante verwenden, müssen Sie sicherstellen, dass `dotnet` Befehl erkannt wird und innerhalb eines der Standardorte: `/usr/local/bin`, `/usr/bin` oder `/bin` ist. Dies ist erforderlich für unseren systemd-Service, der den Befehl `dotnet /path/zu/ArchiSteamFarm.dll` ausführt. Überprüfen Sie, ob `dotnet --info` für Sie funktioniert; falls ja, geben Sie `command -v dotnet` ein, um herauszufinden, wo es sich befindet. Wenn Sie den offiziellen Installer verwendet haben, sollte er in `/usr/bin/dotnet` oder einem der beiden anderen Verzeichnisse liegen, was in Ordnung ist. Wenn es in benutzerdefinierter Position wie `/usr/share/dotnet/dotnet`ist, dann erstellen Sie für die Verwendung einen Symlink dafür mit `ln -s "$(command -v dotnet)" /usr/bin/dotnet`. Now `command -v dotnet` should report `/usr/bin/dotnet`, which will also make our systemd unit work. If you're using OS-specific variant, you don't need to do anything in this regard.

Next, `cd /etc/systemd/system` and execute `ln -s /home/asf/ArchiSteamFarm/ArchiSteamFarm\@.service .`, this will create a symbolic link to our service declaration and register it in `systemd`. Symbolic link will allow ASF to update your `systemd` unit automatically as part of ASF update - depending on your situation, you may want to use that approach, or simply `cp` the file and manage it yourself however you'd like.

Stellen Sie anschließend sicher, dass `systemd` unseren Dienst erkennt:

```
systemctl status ArchiSteamFarm@asf

○ ArchiSteamFarm@asf.service - ArchiSteamFarm Service (on asf)
   Loaded: loaded (/etc/systemd/system/ArchiSteamFarm@.service; disabled; vendor preset: enabled)
   Active: inactive (dead)
    Docs: https://github.com/JustArchiNET/ArchiSteamFarm/wiki
```

Pay special attention to the user we declare after `@`, it's `asf` in our case. Our systemd service unit expects from you to declare the user, as it influences the exact place of the binary `/home/<user>/ArchiSteamFarm`, as well as the actual user systemd will spawn the process as.

If systemd returned output similar to above, everything is in order, and we're almost done. Now all that is left is actually starting our service as our chosen user: `systemctl start ArchiSteamFarm@asf`. Wait a second or two, and you can check the status again:

```
systemctl status ArchiSteamFarm@asf

● ArchiSteamFarm@asf.service - ArchiSteamFarm Service (on asf)
   Loaded: loaded (/etc/systemd/system/ArchiSteamFarm@.service; disabled; vendor preset: enabled)
   Active: active (running) since (...)
       Docs: https://github.com/JustArchiNET/ArchiSteamFarm/wiki
  Main PID: (...)
(...)
```

If `systemd` states `active (running)`, it means everything went well, and you can verify that ASF process should be up and running, for example with `journalctl -r`, as ASF by default also writes to its console output, which is recorded by `systemd`. If you're satisfied with the setup you have right now, you can tell `systemd` to automatically start your service during boot, by executing `systemctl enable ArchiSteamFarm@asf` command. Das war's!

If by any chance you'd like to stop the process, simply execute `systemctl stop ArchiSteamFarm@asf`. Likewise, if you want to disable ASF from being started automatically on boot, `systemctl disable ArchiSteamFarm@asf` will do that for you, it's very simple.

Please note that, as there is no standard input enabled for our `systemd` service, you won't be able to input your details through the console in usual way. Running through `systemd` is equivalent to specifying **[`Headless: true`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#headless)** setting and comes with all its implications. Fortunately for you, it's very easy to manage your ASF through **[ASF-ui](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#asf-ui)**, which we recommend in case you need to supply additional details during login or otherwise manage your ASF process further.

### Umgebungsvariablen

Es ist möglich, zusätzliche Umgebungsvariablen für unseren `systemd`-Dienst bereitzustellen. Das ist für Sie interessant, wenn Sie zum Beispiel ein benutzerdefiniertes `--cryptkey` **[Befehlszeilenargument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments-de-DE#argumente)**verwenden möchten - spezifizieren Sie daher `ASF_CRYPTKEY` Umgebungsvariable.

Um benutzerdefinierte Umgebungsvariablen bereitzustellen, muss der Ordner `/etc/asf` (falls er nicht existiert), mit `mkdir -p /etc/asf` erstellt werden. Wir empfehlen `chown -hR root:root /etc/asf && chmod 700 /etc/asf` um sicherzustellen, dass nur der Benutzer `root` Zugang zum Lesen dieser Dateien hat; da diese empfindliche Eigenschaften (z. B. `ASF_CRYPTKEY`) enthalten könnten. Afterwards, write to a `/etc/asf/<user>` file, where `<user>` is the user you're running the service under (`asf` in our example above, so `/etc/asf/asf`).

The file should contain all environment variables that you'd like to provide to the process. Those that do not have a dedicated environment variable, can be declared in `ASF_ARGS`:

```sh
# Definieren Sie jene die Sie tatsächlich benötigen
ASF_ARGS="--no-config-migrate --no-config-watch"
ASF_CRYPTKEY="my_super_important_secret_cryptkey"
ASF_NETWORK_GROUP="my_network_group"

# Und alle anderen in die Sie interessiert sind
```

### Überschreibung eines Teils der "Service unit" (Dienstverwaltung)

Thanks to the flexibility of `systemd`, it's possible to overwrite part of ASF unit while still preserving the original unit file and allowing ASF to update it for example as part of auto-updates.

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

ASF includes its own validation whether the process is being run as administrator (`root`) or not. Running as root is **not** required for any kind of operation done by the ASF process, assuming properly configured environment it's operating in, and therefore should be regarded as a **bad practice**. This means that on Windows, ASF should never be executed with "run as administrator" setting, and on Unix ASF should have a dedicated user account for itself, or re-use your own in case of a desktop system.

For further elaboration on *why* we discourage running ASF as root, refer to **[superuser](https://superuser.com/questions/218379/why-is-it-bad-to-run-as-root)** and other resources. If you're still not convinced, ask yourself what would happen to your machine if ASF process executed `rm -rf /*` command right after its launch.

### Ich nutze `root`, weil ASF nicht in seine Dateien schreiben kann

Das bedeutet, dass Sie falsch konfigurierte Datei-Berechtigungen haben, auf die ASF zugreifen möchte. You should login as `root` account (either with `su` or `sudo -i`) and then **correct** the permissions by issuing `chown -hR asf:asf /path/to/ASF` command, substituting `asf:asf` with the user that you'll run ASF under, and `/path/to/ASF` accordingly. If by any chance you're using custom `--path` telling ASF user to use the different directory, you should execute the same command again for that path as well.

After doing that, you should no longer get any kind of issue related to ASF not being able to write over its own files, as you've just changed the owner of everything ASF is interested in to the user the ASF process will actually run under.

### Ich verwende `root`, weil ich nicht weiß, wie ich es sonst tun soll

```sh
su # oder sudo -i, um in die Root-Shell zu gelangen
useradd -m asf # Erstelle einen Account den Sie unter
chown -hR asf:asf /path/zu/ASF ausführen möchten # Stellen Sie sicher dass dein neuer Benutzer Zugriff auf das ASF-Verzeichnis hat
su asf -c /path/zu/ASF/ArchiSteamFarm hat # Oder sudo -u asf /path/zu/ASF/ArchiSteamFarm um das Programm tatsächlich unter Ihrem Benutzer zu starten
```

That would be doing it manually, it's much easier to use our **[`systemd` service](#systemd-service-for-linux)** explained above.

### Ich weiß es besser und möchte trotzdem als `root` nutzen

ASF doesn't forcefully stop you from doing so, only displays a warning with a short notice. Just don't be shocked if one day due to a bug in the program it'll blow up your whole OS with complete data loss - you've been warned.

---

## Mehrere Instanzen

ASF unterstützt das Ausführen mehrerer Instanzen des Prozesses auf derselben Maschine. Die Instanzen können komplett eigenständig oder von demselben binären Speicherort abgeleitet werden. In diesem Fall sollten Sie sie mit einem anderen **[Kommandozeilenargument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments-de-DE)** `--path` ausführen.

Wenn Sie mehrere Instanzen von derselben Binärdatei ausführen, ist zu beachten, dass das normalerweise automatische Updates deaktiviert werden sollte, da es keine Synchronisation zwischen diese im Bezug auf Auto-Updates gibt. Wenn Sie automatische Updates aktivieren möchten, empfehlen wir Standalone-Instanzen aber Sie können trotzdem dafür sorgen, dass Updates funktionieren, solange Sie sicherstellen können, dass alle anderen ASF-Instanzen geschlossen sind.

ASF wird sein Bestes tun, um eine minimale Menge an OS-übergreifender Kommunikation mit anderen ASF-Instanzen zu pflegen. Dazu gehört die Überprüfung des Konfigurationsverzeichnisses mit denen anderer Instanzen, sowie die Freigabe prozessweiter Begrenzungen mit der **[globaler Konfigurationsvariable](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-de-DE#globale-konfiguration)** `*LimiterDelay`, um sicherzustellen, dass das Ausführen mehrerer ASF-Instanzen kein Anfragelimit auslöst. In Bezug auf technische Aspekte verwenden alle Plattformen unseren dedizierten Mechanismus von benutzerdefinierten ASF-Dateisperren, die im temporären Verzeichnis erstellt wurden, `C:\Users\<YourUser>\AppData\Local\Temp\ASF` unter Windows, und `/tmp/ASF` unter Unix.

Es ist nicht erforderlich die gleiche `*LimiterDelay` Eigenschaften zu teilen um ASF Instanzen auszuführen, sie können verschiedene Werte verwenden, da jeder ASF nach dem Erwerb der Sperre seine eigene konfigurierte Verzögerung zur Release-Zeit hinzufügt. Wenn `*LimiterDelay` auf `0` gesetzt ist, wird die ASF-Instanz das Warten auf die Sperre der angegebenen Ressource, die mit anderen Instanzen geteilt wird, überspringen (was möglicherweise immer noch eine gemeinsame Sperre untereinander aufrecht erhalten könnte). Wenn auf einen beliebigen anderen Wert gesetzt, wird ASF sich korrekt mit anderen ASF-Instanzen synchronisieren, und wartet bis es an der Reihe ist, bevor es dann nach konfigurierter Verzögerung die Sperre aufhebt, wodurch andere Instanzen fortfahren können.

ASF berücksichtigt die `WebProxy` Einstellung bei der Entscheidung über den gemeinsamen Anwendungsbereich, was bedeutet, dass zwei ASF-Instanzen, die verschiedene `WebProxy` Konfigurationen verwenden, Ihre Begrenzungen nicht untereinander teilen werden. Dies wird implementiert, um `WebProxy` zu ermöglichen, ohne übermäßige Verzögerungen zu arbeiten, wie von verschiedenen Netzwerkschnittstellen erwartet. Dies sollte für die Mehrzahl der Anwendungsfälle gut genug sein. Wenn Sie jedoch eine spezifische Benutzerkonfiguration haben, in der Sie z. B. Anfragen über eine bestimmte Netzwerkgruppe weiterleiten, können Sie das **[Kommandozeilenargument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments-de-DE)**`--network-group` angeben, welches Ihnen erlaubt, eine ASF-Gruppe zu deklarieren, die mit dieser Instanz synchronisiert wird. Beachten Sie, dass ausschließlich benutzerdefinierte Netzwerkgruppen verwendet werden. Das bedeutet, dass ASF `WebProxy` nicht mehr zur Bestimmung der richtigen Gruppe verwenden wird, da Sie für die Gruppierung in diesem Fall verantwortlich sind.

Wenn Sie unseren **[`systemd`-Dienst](#systemd-service-f%C3%BCr-linux)** wie oben erläutert für mehrere ASF-Instanzen verwenden möchten, ist dies sehr einfach. Verwenden Sie einfach einen anderen Benutzer für unsere `ArchiSteamFarm@` Service-Deklaration und folgen Sie mit den verbleibenden Schritten. Dies entspricht dem Ausführen mehrerer ASF-Instanzen mit unterschiedlichen Binärdateien, sodass diese sich auch unabhängig voneinander automatisch aktualisieren und funktionieren können.