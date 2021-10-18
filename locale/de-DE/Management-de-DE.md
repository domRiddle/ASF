# Management

This section covers subjects related to managing the ASF process in optimal way. While not strictly mandatory for usage, it includes bunch of tips, tricks and good practices that we'd like to share, especially for system administrators, people packaging the ASF for usage in third-party repositories, as well as advanced users and alike.

---

## `systemd` service for Linux

In `generic` and `linux` variants, ASF comes with `ArchiSteamFarm@.service` file, which is a configuration file of the service for **[`systemd`](https://systemd.io)**. If you'd like to run ASF as a service, for example in order to launch it automatically after startup of your machine, then a proper `systemd` service is arguably the best way to do it, therefore we highly recommend it instead of managing it on your own through `nohup`, `screen` or alike.

Firstly, create the account for the user you want to run ASF under, assuming it doesn't exist yet. We'll use `asf` user for this example, if you decided to use a different one, you'll need to substitute `asf` user in all of our examples below with your selected one. Our service does not allow you to run ASF as `root`, since it's considered a **[bad practice](#never-run-asf-as-administrator)**.

```sh
su # or sudo -i
adduser asf
```

Next, unpack ASF to `/home/asf/ArchiSteamFarm` folder. The folder structure is important for our service unit, it should be `ArchiSteamFarm` folder in your `$HOME`, so `/home/<user>`. If you did everything correctly, there will be `/home/asf/ArchiSteamFarm/ArchiSteamFarm@.service` file existing.

We'll do all below actions as `root`, so get to its shell with `su` or `sudo -i`.

Firstly it's a good idea to ensure that our folder still belongs to our `asf` user, `chown -hR asf:asf /home/asf/ArchiSteamFarm` executed once will do it. The permissions could be wrong e.g. if you've downloaded and/or unpacked the zip file as `root`.

Next, `cd /etc/systemd/system` and execute `ln -s /home/asf/ArchiSteamFarm/ArchiSteamFarm\@.service .`, this will create a symbolic link to our service declaration and register it in `systemd`.

Afterwards, ensure that `systemd` recognizes our service:

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

If `systemd` states `active (running)`, it means everything went well, and you can verify that ASF process should be up and running, for example with `tail -f -n 100 /var/log/syslog`, as ASF by default also reports its console output to syslog. If you're satisfied with the setup you have right now, you can tell `systemd` to automatically start your service during boot, by executing `systemctl enable ArchiSteamFarm@asf` command. That's all.

If by any chance you'd like to stop the process, simply execute `systemctl stop ArchiSteamFarm@asf`. Likewise, if you want to disable ASF from being started automatically on boot, `systemctl disable ArchiSteamFarm@asf` will do that for you, it's very simple.

Please note that, as there is no standard input enabled for our `systemd` service, you won't be able to input your details through the console in usual way. Running through `systemd` is equivalent to specifying **[`Headless: true`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#headless)** setting and comes with all its implications. Fortunately for you, it's very easy to manage your ASF through **[ASF-ui](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#asf-ui)**, which we recommend in case you need to supply additional details during login or otherwise manage your ASF process further.

### Environment variables

It's possible to supply additional environment variables to our `systemd` service, which you'll be interested in doing in case you want to for example use a custom `--cryptkey` **[command-line argument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments#arguments)**, therefore specifying `ASF_CRYPTKEY` environment variable.

In order to provide custom environment variables, create `/etc/asf` folder (in case it doesn't exist), `mkdir -p /etc/asf`, then write to a `/etc/asf/<user>` file, where `<user>` is the user you're running the service under (`asf` in our example above, so `/etc/asf/asf`).

The file should contain all environment variables that you'd like to provide to the process:

```sh
# Declare only those that you actually need
ASF_CRYPTKEY="my_super_important_secret_cryptkey"
ASF_NETWORK_GROUP="my_network_group"

# And any other ones you're interested in
```

---

## Never run ASF as administrator!

ASF includes its own validation whether the process is being run as administrator (`root`) or not. Running as root is **not** required for any kind of operation done by the ASF process, assuming properly configured environment it's operating in, and therefore should be regarded as a **bad practice**. This means that on Windows, ASF should never be executed with "run as administrator" setting, and on Unix ASF should have a dedicated user account for itself, or re-use your own in case of a desktop system.

For further elaboration on *why* we discourage running ASF as root, refer to **[superuser](https://superuser.com/questions/218379/why-is-it-bad-to-run-as-root)** and other resources. If you're still not convinced, ask yourself what would happen to your machine if ASF process executed `rm -rf --no-preserve-root /` command right after its launch.

### I run as `root` because ASF can't write to its files

This means that you have wrongly configured permissions of the files ASF is trying to access. You should login as `root` account (either with `su` or `sudo -i`) and then **correct** the permissions by issuing `chown -hR asf:asf /path/to/ASF` command, substituting `asf:asf` with the user that you'll run ASF under, and `/path/to/ASF` accordingly. If by any chance you're using custom `--path` telling ASF user to use the different directory, you should execute the same command again for that path as well.

After doing that, you should no longer get any kind of issue related to ASF not being able to write over its own files, as you've just changed the owner of everything ASF is interested in to the user the ASF process will actually run under.

### I run as `root` because I don't know how to do it otherwise

```sh
su # or sudo -i
adduser asf
chown -hR asf:asf /path/to/ASF
su asf -c /path/to/ASF/ArchiSteamFarm # or sudo -u asf /path/to/ASF/ArchiSteamFarm
```

That would be doing it manually, it's much easier to use our **[`systemd` service](#systemd-service-for-linux)** explained above.

---

## Mehrere Instanzen

ASF unterstützt das Ausführen mehrerer Instanzen des Prozesses auf derselben Maschine. Die Instanzen können komplett eigenständig oder von demselben binären Speicherort abgeleitet werden (in diesem Fall sollten Sie sie mit verschiedenen `--path` **[Kommandozeilenargumenten](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments-de-DE)**) ausführen.

Wenn Sie mehrere Instanzen von derselben Binärdatei ausführen, ist zu beachten, dass das normalerweise automatische Updates deaktiviert werden sollte, da es keine Synchronisation zwischen diese im Bezug auf Auto-Updates gibt. Wenn Sie automatische Updates aktivieren möchten, empfehlen wir Standalone-Instanzen aber Sie können trotzdem dafür sorgen, dass Updates funktionieren, solange Sie sicherstellen können, dass alle anderen ASF-Instanzen geschlossen sind.

ASF wird sein Bestes tun, um eine minimale Menge an OS-übergreifender Kommunikation mit anderen ASF-Instanzen zu pflegen. Dazu gehört die Überprüfung des Konfigurationsverzeichnisses mit denen anderer Instanzen, sowie die Freigabe prozessweiter Begrenzer mit `*LimiterDelay` **[globaler Konfiguration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-de-DE#globale-konfiguration)**, um sicherzustellen, dass das Ausführen mehrerer ASF-Instanzen kein Erreichen des Anfragelimits verursacht. In Bezug auf technische Aspekte verwenden alle Plattformen unseren dedizierten Mechanismus von benutzerdefinierten ASF-Dateisperren, die im temporären Verzeichnis erstellt wurden, `C:\Users\<YourUser>\AppData\Local\Temp\ASF` unter Windows, und `/tmp/ASF` unter Unix.

Es ist nicht erforderlich die gleiche `*LimiterDelay` Eigenschaften zu teilen um ASF Instanzen auszuführen, sie können verschiedene Werte verwenden, da jeder ASF nach dem Erwerb der Sperre seine eigene konfigurierte Verzögerung zur Release-Zeit hinzufügt. Wenn `*LimiterDelay` auf `0` gesetzt ist, wird die ASF-Instanz das Warten auf die Sperre der angegebenen Ressource, die mit anderen Instanzen geteilt wird, überspringen (was möglicherweise immer noch eine gemeinsame Sperre untereinander aufrecht erhalten könnte). Wenn auf einen beliebigen anderen Wert gesetzt, wird ASF sich korrekt mit anderen ASF-Instanzen synchronisieren, und wartet bis es an der Reihe ist, bevor es dann nach konfigurierter Verzögerung die Sperre aufhebt, wodurch andere Instanzen fortfahren können.

ASF berücksichtigt die `WebProxy` Einstellung bei der Entscheidung über den gemeinsamen Anwendungsbereich, was bedeutet, dass zwei ASF-Instanzen, die verschiedene `WebProxy` Konfigurationen verwenden, ihre Begrenzungen nicht untereinander teilen werden. Dies wird implementiert, um `WebProxy` zu ermöglichen, ohne übermäßige Verzögerungen zu arbeiten, wie von verschiedenen Netzwerkschnittstellen erwartet. Dies sollte für die Mehrzahl der Anwendungsfälle gut genug sein. Wenn Sie jedoch eine spezifische Benutzerkonfiguration haben, in der Sie z. B. Anfragen über eine bestimmte Netzwerkgruppe weiterleiten, können Sie das **[Kommandozeilenargument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments-de-DE)**`--network-group` angeben, welches Ihnen erlaubt, eine ASF-Gruppe zu deklarieren, die mit dieser Instanz synchronisiert wird. Beachten Sie, dass ausschließlich benutzerdefinierte Netzwerkgruppen verwendet werden. Das bedeutet, dass ASF `WebProxy` nicht mehr zur Bestimmung der richtigen Gruppe verwenden wird, da Sie für die Gruppierung in diesem Fall verantwortlich sind.