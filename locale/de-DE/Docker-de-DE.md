# Docker

ASF ist als **[Docker Container](https://www.docker.com/what-container)** verfügbar. Unsere Docker-Pakete sind derzeit auf **[ghcr.io](https://github.com/orgs/JustArchiNET/packages/container/archisteamfarm/versions)** sowie **[Docker-Hub](https://hub.docker.com/r/justarchi/archisteamfarm)** verfügbar.

Es ist wichtig zu beachten, dass ASF im Docker Container als **erweitertes Setup** angesehen wird, was für die überwiegende Mehrheit der Benutzer **nicht benötigt wird** und gibt in der Regel **keine Vorteile** gegenüber einer containerlosen Einrichtung. Wenn Sie Docker als eine Lösung für den Betrieb von ASF als Dienst betrachten, zum Beispiel indem Sie ihn automatisch mit Ihrem Betriebssystem starten, dann sollten Sie möglicherweise stattdessen den Abschnitt **[Management](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Management#systemd-service-for-linux)** lesen, und einen korrekten `systemd` Dienst einrichten, der **fast immer** eine bessere Lösung ist, als ASF in einem Docker Container auszuführen.

Die Ausführung von ASF im Docker-Container beinhaltet in der Regel **mehrere neue Probleme und Probleme**, die Sie selbst bewältigen und lösen müssen. Aus diesem Grund empfehlen wir Ihnen **sehr** dies zu vermeiden; es sei denn, Sie haben bereits Docker-Kenntnisse und benötigen keine Hilfe beim Verständnis seiner internen Daten, über die wir hier im ASF Wiki nicht eingehen. Dieser Abschnitt ist hauptsächlich für gültige Anwendungsfälle sehr komplexer Setups. Zum Beispiel in Bezug auf erweiterte Vernetzung oder Sicherheit über Standard-Sandboxing hinaus, die ASF im `systemd` Service beinhaltet (der bereits eine überlegene Prozessisolierung durch sehr fortgeschrittene Sicherheitsmechanismen sicherstellt). Für diese wenigen Nutzer erklären wir hier bessere ASF-Konzepte in bezüglich der Docker-Kompatibilität und nur das. Voraussetzung ist, dass Sie selbst über adäquate Docker Kenntnisse verfügen, wenn Sie sich dafür entscheiden, es zusammen mit ASF zu verwenden.

---

## Tags

ASF ist über vier Haupttypen von ***[Tags](https://hub.docker.com/r/justarchi/archisteamfarm/tags)*** verfügbar:


### `main`

Dieser *Tag* verweist immer auf den ASF, der vom letzten Commit im `Hauptbereich` erstellt wurde; was genauso funktioniert, wie das Erfassen neuester Artefakte direkt aus unserer **[CI](https://github.com/JustArchiNET/ArchiSteamFarm/actions/workflows/publish.yml?query=branch%3Amain)** Pipeline. Normalerweise sollten Sie dieses *Tag* vermeiden, da es die höchste Stufe von fehlerhafter Software ist, die für Entwickler und fortgeschrittene Benutzer zu Entwicklungszwecken bestimmt ist. Das Image wird mit jedem Commit im `main`-GitHub-Zweig aktualisiert, daher können Sie hier sehr oft mit Updates (und Problemen) rechnen. Es liegt an uns, den aktuellen Stand des ASF-Projekts zu markieren, der nicht unbedingt garantiert stabil oder getestet ist, wie in unserem Veröffentlichungszyklus dargelegt. Dieses *Tag* sollte nicht in einer Produktionsumgebung verwendet werden.


### `released`

Sehr ähnlich wie oben, zeigt dieser *Tag* immer auf die neueste **[veröffentlichte](https://github.com/JustArchiNET/ArchiSteamFarm/releases)** ASF-Version (einschließlich Vorabversionen). Im Vergleich zum `main`-*Tag* wird dieses Image jedes Mal aktualisiert, wenn ein neuer GitHub-*Tag* gepusht (verfügbar) wird. Geeignet für Fortgeschrittene und Power-User, die es lieben, am Rande dessen zu leben, was als stabil und zugleich frisch angesehen werden kann. Dies würden wir empfehlen, wenn Sie nicht den `latest` *Tag* verwenden möchten. In der Praxis funktioniert es genauso wie ein fortlaufender *Tag* mit dem Hinweis auf die neueste `A.B.C.D` Version zum Zeitpunkt des Downloads (`pull`). Bitte bedenken Sie, dass die Verwendung dieses *Tags* gleichbedeutend mit der Verwendung unserer **[Vorveröffentlichungen](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle-de-DE)** ist.


### `latest`

Unter den verfügbaren *Tags* ist dieses das Einzige, welches automatische Updates enthält und auf die **[stable](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)**-ASF-Version verweist. Der Zweck dieses *Tags* ist es, einen funktionierenden (Standard)-Docker-Container zur Verfügung zu stellen, der selbstständig in der Lage ist, eine aktualisierte, OS-spezifische ASF-Instanz auszuführen. Dadurch muss das Image nicht so oft auf den neuesten Stand gebracht werden, da die enthaltene ASF-Version immer in der Lage ist, sich bei Bedarf selbst zu aktualisieren. Natürlich kann `UpdatePeriod` problemlos ausgeschaltet werden (eingestellt auf `0`), aber in diesem Fall sollten Sie wahrscheinlich stattdessen die eingefrorene Version `A.B.C.D` verwenden. Ebenso können Sie den Standard `UpdateChannel` ändern, um stattdessen ein automatisches Aktualisieren des `released` *Tags* durchzuführen.

Aufgrund der Tatsache, dass die Version `latest` automatisch aktualisiert werden kann, enthält es ein spärliches Betriebssystem inklusive einer OS-spezifischen `Linux` ASF Version; im Gegensatz zu allen anderen Tags, die OS mit .NET Laufzeit und `generische` ASF Version. Dies liegt daran, dass eine neuere (aktualisierte) ASF-Version möglicherweise auch eine neuere Laufzeitumgebung erfordert als die, mit der das Build kompiliert werden könnte. Ansonsten würde dies einen Neuaufbau des Images von Grund auf erfordern, wodurch der geplante Anwendungsfall hinfällig wäre.

### `A.B.C.D`

Im Vergleich zu den oben genannten *Tags* ist dieser *Tag* vollständig eingefroren, was bedeutet, dass das Image nach der Veröffentlichung nicht mehr aktualisiert wird. Dies funktioniert ähnlich wie bei unseren GitHub-Versionen, die nach der ersten Version nie wieder berührt werden, was Ihnen eine stabile und gefrorene Umgebung garantiert. Normalerweise sollten Sie dieses Tag verwenden, wenn Sie eine bestimmte ASF-Version verwenden und auf automatische Aktualisierungen verzichten möchten (die beispielsweise im *Tag* `latest` angeboten werden).

---

## Welcher *Tag* ist für mich der beste?

Das hängt davon ab, wonach Sie suchen. Für die Mehrheit der Benutzer sollte das `latest` *Tag* das beste sein, da es genau das bietet, was Desktop-ASF bietet, nur in einem speziellen Docker-Container als Dienst. Nutzer, die ihre Images oft neu erstellen und stattdessen lieber die volle Kontrolle über die ASF-Version haben, die an diese Veröffentlichung gebunden ist, können das `released` *Tag* verwenden. Wenn Sie stattdessen eine bestimmte eingefrorene ASF-Version verwenden möchten, die sich ohne Ihre eindeutige Absicht nie ändern wird, stehen Ihnen `A.B.C.D` Versionen als feste ASF-Meilensteine zur Verfügung, auf die Sie immer zurückgreifen können.

Wir raten generell davon ab, `main` Builds auszuprobieren, da diese für uns hier sind, um den aktuellen Stand des ASF-Projekts zu kennzeichnen. Nichts garantiert, dass ein solcher Zustand ordnungsgemäß funktioniert, aber natürlich können Sie ihn gerne ausprobieren, wenn Sie an der ASF-Entwicklung interessiert sind.

---

## Architekturen

ASF Docker Image ist derzeit auf `Linux` Plattform für 3 Architekturen verfügba - `x64`, `arm` und `arm64`. Sie können im Abschnitt **[Kompatibilität](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-de-DE)** mehr darüber lesen.

Our tags are using multi-platform manifest, which means that Docker installed on your machine will automatically select the proper image for your platform when pulling the image. If by any chance you'd like to pull a specific platform image which doesn't match the one you're currently running, you can do that through `--platform` switch in appropriate docker commands, such as `docker run`. Mehr Informationen hierzu finden Sie in der Docker Dokumentation auf **[image manifest](https://docs.docker.com/registry/spec/manifest-v2-2)**.

---

## Nutzung

Für eine vollständige Referenz solltenSie die **[offizielle Docker-Dokumentation](https://docs.docker.com/engine/reference/commandline/docker)** verwenden. Wir werden in diesem Leitfaden nur die grundlegende Verwendung behandeln. Sie sind herzlich dazu eingeladen, noch tiefer zu graben.

### Hallo ASF!

Zuallererst sollten wir überprüfen, ob unser Docker momentan überhaupt funktioniert. Das wird als unser ASF "Hallo Welt" dienen:

```shell
docker run -it --name asf --pull always --rm justarchi/archisteamfarm
```

`docker run` erstellt einen neuen ASF Docker-Container für dich und lässt ihn im Vordergrund laufen (`-it`). `--pull always` ensures that up-to-date image will be pulled first, and `--rm` ensures that our container will be purged once stopped, since we're just testing if everything works fine for now.

Wenn alles erfolgreich geendet hat, nachdem Sie alle Schichten und den Start-Container geholt haben, solltenSie feststellen, dass ASF richtig gestartet wurde und uns mitgeteilt haben, dass es keine definierten Bots gibt, was gut ist - wir haben verifiziert, dass ASF im Docker richtig funktioniert. Drücke `CTRL+P` dann `STRG+Q` um den Vordergrund-Docker-Container zu beenden, dann stoppe den ASF-Container mit `Docker stoppen asf`.

Wenn Sie Ihnen den Befehl genauer ansiehst, werden Sie feststellen, dass wir kein *Tag* deklariert haben, da es automatisch auf `latest` voreingestellt ist. Sofern Sie ein anderes Schlagwort als `latest`verwenden möchten, zum Beispiel `released`, dann sollten Sie es explizit erklären:

```shell
docker run -it --name asf --pull always --rm justarchi/archisteamfarm:released
```

---

## Verwendung eines Volumes

Wenn Sie ASF im Docker-Container verwenden, dann müssen Sie natürlich das Programm selbst konfigurieren. Sie können es auf verschiedene Weise tun, aber die empfohlene wäre, das ASF `config` Verzeichnis auf der lokalen Maschine zu erstellen und es dann als gemeinsames Volume im ASF-Docker-Container zu mounten.

Zum Beispiel gehen wir davon aus, dass sich Ihr ASF-Konfigurationsordner im Verzeichnis `/home/archi/ASF/config` befindet. Dieses Verzeichnis enthält den Kern `ASF.json` sowie Bots, die wir ausführen wollen. Jetzt müssen wir nur noch dieses Verzeichnis als Shared Volume in unserem Docker-Container anhängen, wo ASF sein Konfigurationsverzeichnis erwartet (`/app/config`).

```shell
docker run -it -v /home/archi/ASF/config:/app/config --name asf --pull always justarchi/archisteamfarm
```

Und das war's, jetzt verwendet Ihr ASF-Docker-Container das freigegebene Verzeichnis mit Ihrer lokalen Maschine im Lese-/Schreibmodus, was alles ist, was Sie für die Konfiguration von ASF brauchen. In similar way you can mount other volumes that you'd like to share with ASF, such as `/app/logs` or `/app/plugins/MyCustomPluginDirectory`.

Natürlich ist dies nur ein konkreter Weg, um das zu erreichen, was wir wollen, nichts hält dich davon ab, z. B. eine eigene `Dockerfile` zu erstellen, die Ihre Konfigurationsdateien in das Verzeichnis `/app/config` im ASF Docker-Container kopiert. Wir behandeln in diesem Leitfaden nur die grundlegende Verwendung.

### Zugriffsrechte für Volumes

ASF container by default is initialized with default `root` user, which allows it to handle the internal permissions stuff and then eventually switch to `asf` (UID `1000`) user for the remaining part of the main process. While this should be satisfying for the vast majority of users, it does affect the shared volume as newly-generated files will be normally owned by `asf` user, which may not be desired situation if you'd like some other user for your shared volume.

Docker erlaubt es Ihnen, `--user` **[Flag](https://docs.docker.com/engine/reference/run/#user)** an `docker run` Befehl zu übergeben, der den Standardbenutzer definiert, unter dem ASF laufen wird. Sie können Ihre `uid` und `gid` zum Beispiel mit dem Befehl `id` überprüfen und ihn dann an den Rest des Befehls übergeben. Zum Beispiel, wenn Ihr Ziel-Benutzer die `uid` und `gid` von 1001 hat:

```shell
docker run -it -u 1001:1001 -v /home/archi/ASF/config:/app/config --name asf --pull always justarchi/archisteamfarm
```

Remember that by default `/app` directory used by ASF is still owned by `asf`. Wenn Sie ASF mit einem benutzerdefinierten Benutzer ausführen, hat Ihr ASF-Prozess keinen Schreibzugriff auf seine eigenen Dateien. Dieser Zugriff ist für den Betrieb nicht zwingend erforderlich, aber er ist z. B. für die Funktion Automatische-Aktualisierung entscheidend. In order to fix this, it's enough to change ownership of all ASF files from default `asf` to your new custom user.

```shell
docker exec -u root asf chown -hR 1001:1001 /app
```

Dies muss nur einmal durchgeführt werden, nachdem Sie Ihren Container mit `docker run` erstellt haben, und nur dann, wenn Sie dich dazu entschieden haben, einen benutzerdefinierten Benutzer für den ASF-Prozess zu verwenden. Vergiss auch nicht, das Argument `1001:1001` in beiden obigen Befehlen auf die `uid` und `gid` zu ändern, unter dem Sie ASF ausführen möchten.

### Volume with SELinux

If you're using SELinux in enforced state on your OS, which is the default for example on RHEL-based distros, then you should mount the volume appending `:Z` option, which will set correct SELinux context for it.

```
docker run -it -v /home/archi/ASF/config:/app/config:Z --name asf --pull always justarchi/archisteamfarm
```

This will allow ASF to create files targetting the volume while inside docker container.

---

## Synchronisation mehrerer Instanzen

ASF includes support for multiple instances synchronization, as stated in **[management](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Management#multiple-instances)** section. Beim Ausführen von ASF im Docker-Container können Sie optional "opt-in" in den Prozess einschalten für den Fall, dass Sie mehrere Container mit ASF (synchronisiert) verwenden möchten.

Standardmäßig ist jeder ASF in einem eigenständigen Docker-Container und das bedeutet, dass keine Synchronisation stattfindet. Um die Synchronisation zwischen Ihnen zu aktivieren, müssen Sie den Pfad `/tmp/ASF` in jedem ASF Container, den Sie synchronisieren möchten, zu einem gemeinsamen Pfad (im Lese-Schreib-Modus) auf dem Docker-Host einbinden. Dies wird genauso wie bei der Bindung eines Volumens erreicht, das oben beschrieben wurde, nur mit verschiedenen Pfaden:

```shell
mkdir -p /tmp/ASF-g1
docker run -v /tmp/ASF-g1:/tmp/ASF -v /home/archi/ASF/config:/app/config --name asf1 --pull always justarchi/archisteamfarm
docker run -v /tmp/ASF-g1:/tmp/ASF -v /home/john/ASF/config:/app/config --name asf2 --pull always justarchi/archisteamfarm
# And so on, all ASF containers are now synchronized with each other
```

Wir empfehlen das ASF-Verzeichnis `/tmp/ASF` auch an ein temporäres `/tmp` Verzeichnis auf ihrem Rechner zu binden, aber Sie können natürlich jedes andere wählen, die ihre Nutzung befriedigt. Jeder ASF-Container, der synchronisiert werden soll, sollte sein `/tmp/ASF` Verzeichnis mit anderen Containern teilen, die am gleichen Synchronisierungsprozess teilnehmen.

Wie Sie vermutlich am Beispiel oben erraten haben, ist es auch möglich, zwei oder mehr "Synchronisierungsgruppen" zu erstellen indem Sie verschiedene Docker Host-Pfade in ASFs `/tmp/ASF` einbinden.

Das einbinden von `/tmp/ASF` ist komplett optional und aktuell nicht empfehlenswert, es sei denn, Sie möchten explizit zwei oder mehr ASF-Container synchronisieren. Wir raten davon ab, `/tmp/ASF` für den Einsatz mit einem Container einzubinden, da es absolut keine Vorteile bringt, wenn Sie nur einen ASF-Container laufen lassen wollen, und es könnte tatsächlich zu Problemen führen, die sonst vermieden werden könnten.

---

## Befehlszeilenargumente

ASF erlaubt es Ihnen **[Kommandozeilenargumente](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments-de-DE)** in Docker-Containern durch Umgebungsvariablen zu geben. Sie sollten spezifische Umgebungsvariablen für die unterstützten Schalter benutzen und für den Rest `ASF_ARGS`. Dies kann mit dem Schalter `-e` erreicht werden, der zum `Docker Run`hinzugefügt wurde; zum Beispiel:

```shell
docker run -it -e "ASF_CRYPTKEY=MyPassword" -e "ASF_ARGS=--no-config-migrate" --name asf --pull always justarchi/archisteamfarm
```

Dies wird sowohl das Argument `--cryptkey` korrekt an den ASF-Prozess übergeben, der im Docker-Container ausgeführt wird, als auch andere Argumente. Falls Sie ein fortgeschrittener Benutzer sind, können Sie natürlich auch den `ENTRYPOINT` bearbeiten oder eine `CMD` hinzufügen und ihre Argumente selbst übergeben.

Unless you want to provide custom encryption key or other advanced options, usually you don't need to include any special environment variables, as our docker containers are already configured to run with a sane expected default options of `--no-restart` `--process-required` `--system-required`, so those flags do not need to be specified explicitly in `ASF_ARGS`.

---

## IPC

Assuming you didn't change the default value for `IPC` **[global configuration property](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#global-config)**, it's already enabled. However, you must do two additional things for IPC to work in Docker container. Firstly, you must use `IPCPassword` or modify default `KnownNetworks` in custom `IPC.config` to allow you to connect from the outside without using one. Unless you really know what you're doing, just use `IPCPassword`. Secondly, you have to modify default listening address of `localhost`, as docker can't route outside traffic to loopback interface. Ein Beispiel für eine Einstellung, die auf allen Schnittstellen lauscht, wäre `http://*:1242`. Natürlich können Sie auch restriktivere Bindungen verwenden, wie z. B. nur lokales LAN oder VPN-Netzwerk, aber es muss eine von außen zugängliche Route sein - `localhost` reicht nicht aus, da die Route vollständig innerhalb des Gastcomputers liegt.

Um das oben Gesagte zu tun, solltenSie eine **[benutzerspezifische IPC-Konfiguration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-de-DE#benutzerdefinierte-konfiguration)** verwenden, wie die untenstehende:

```json
{
  "Kestrel": {
    "Endpoints": {
      "HTTP": {
        "Url": "http://*:1242"
      }
    }
  }
}
```

Sobald wir IPC auf einer Nicht-Loopback-Schnittstelle eingerichtet haben, ist es notwendig Docker mitzuteilen, dass er den ASF-Port `1242/tcp` entweder mit dem Schalter `-P` oder `-p` zuordnen soll.

Dieser Befehl würde die ASF-IPC-Schnittstelle beispielsweise (nur) zum Host-Computer freigeben:

```shell
docker run -it -p 127.0.0.1:1242:1242 -p [::1]:1242:1242 --name asf --pull always justarchi/archisteamfarm
```

Wenn Sie alles richtig eingestellt haben, wird der oben gezeigte `docker run` Befehl das **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-de-DE)** Interface von Ihrem Host-Computer aus auf der Standardroute `localhost:1242` laufen lassen, die nun korrekt auf Ihre Gast-Maschine umgeleitet wird. Es ist auch gut zu wissen, dass wir diese Route nicht weiter offenlegen, sodass die Verbindung nur innerhalb des Docker-Hosts erfolgen kann und somit sicher bleibt. Natürlich können Sie den Weg weiter ausblenden, wenn Sie wissen, was Sie tun, und für entsprechende Sicherheitsmaßnahmen sorgen.

---

### Vollständiges Beispiel

Wenn man das gesamte obige Wissen kombiniert, würde ein Beispiel für ein komplettes Setup so aussehen:

```shell
docker run -p 127.0.0.1:1242:1242 -p [::1]:1242:1242 -v /home/archi/ASF/config:/app/config --name asf --pull always justarchi/archisteamfarm
```

This assumes that you'll use a single ASF container, with all ASF config files in `/home/archi/ASF/config`. Sie sollten den Konfigurationspfad zu dem ändern, der zu ihrem Rechner passt. Dieses Setup ist auch für die optionale IPC-Nutzung bereit, wenn Sie dich entschieden haben, `IPC.config` in Ihr Konfigurationsverzeichnis mit einem Inhalt wie unten beschrieben aufzunehmen:

```json
{
  "Kestrel": {
    "Endpoints": {
      "HTTP": {
        "Url": "http://*:1242"
      }
    }
  }
}
```

---

## Profi-Tipps

Wenn Sie Ihren ASF-Docker-Container bereits startklar haben, müssen Sie nicht jedes Mal `docker run` verwenden. Sie können den ASF-Docker-Container einfach stoppen/starten mit `docker stop asf` und `docker start asf`. Keep in mind that if you're not using `latest` tag then using up-to-date ASF will still require from you to `docker stop`, `docker rm` and `docker run` again. Dies liegt daran, dass Sie Ihren Container jedes Mal aus einem frischen ASF-Docker-Image neu erstellen müssen, wenn Sie die in diesem Image enthaltene ASF-Version verwenden möchten. In `latest` tag, ASF has included capability to auto-update itself, so rebuilding the image is not necessary for using up-to-date ASF (but it's still a good idea to do it from time to time in order to use fresh .NET runtime dependencies and the underlying OS, which might be needed when jumping across major ASF version updates).

Wie oben angedeutet, wird sich ASF in einem anderen *Tag* als `latest` nicht automatisch aktualisieren, was bedeutet, dass **du** dafür verantwortlich sind, aktuelles `justarchi/archisteamfarm` Repository zu verwenden. Dies hat viele Vorteile, da die Anwendung typischerweise nicht ihren eigenen Quellcode berühren sollte, wenn sie ausgeführt wird, aber wir verstehen auch Komfort, der dadurch entsteht, dass Sie dich nicht um die ASF-Version in Ihrem Docker-Container kümmern müssen. Wenn Sie dich um gute Praktiken und die korrekte Verwendung von Dockern kümmerst, ist `released` *Tag* das, was wir vorschlagen würden, anstatt `latest`, aber wenn Sie dich nicht darum kümmern können und Sie ASF nur dazu bringen willst, sowohl zu funktionieren als auch sich selbst automatisch zu aktualisieren, dann reicht `latest`.

Sie solltenASF typischerweise im Docker-Container mit der globalen Konfiguration `Headless: true` ausführen. Dies wird ASF klar signalisieren, dass Sie nicht hier sind, um fehlende Details zu liefern und es sollte nicht nach diesen fragen. Natürlich solltenSie für die Erstkonfiguration in Betracht ziehen, diese Option bei `false` zu belassen, damit Sie Dinge einfach einrichten können, aber auf lange Sicht sind Sie typischerweise nicht an die ASF-Konsole gebunden, deshalb wäre es sinnvoll, ASF darüber zu informieren und `input` **[Befehl](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE#input-befehl)** zu verwenden, wenn Bedarf entsteht. Auf diese Weise muss ASF nicht unendlich auf Benutzereingaben warten die nicht erfolgen (und dabei Ressourcen verschwenden). Dies ermöglicht es, dass ASF im nicht-interaktiven Modus innerhalb eines Containers ausgeführt werden kann, was entscheidend ist, z. B. in Bezug auf die Weiterleitungssignale, die es ASF ermöglichen, auf Anfrage von `docker stop asf` angemessen zu schließen.