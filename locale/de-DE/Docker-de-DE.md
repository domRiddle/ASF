# Docker

Seit Version 3.0.3.2 steht ASF nun auch als **[Docker-Container](https://www.docker.com/what-container)** zur Verfügung. ASF im Docker-Container laufen zu lassen hat in der Regel keine Vorteile für gelegentliche Nutzer. Es könnte aber eine hervorragende Möglichkeit zur Nutzung von ASF auf Servern sein, um sicherzustellen dass ASF in einer Sandbox-Umgebung getrennt von allen anderen Anwendungen ausgeführt wird. Unsere Docker-Pakete sind derzeit auf **[ghcr.io](https://github.com/orgs/JustArchiNET/packages/container/archisteamfarm/versions)** sowie **[Docker-Hub](https://hub.docker.com/r/justarchi/archisteamfarm)** verfügbar.

---

## Tags

ASF ist durch 4 Haupttypen von **[Tags](https://hub.docker.com/r/justarchi/archisteamfarm/tags)** verfügbar:


### `main`

Dieses Tag zeigt immer auf den ASF-Build, der aus dem neuesten Commit im `main`-Zweig erstellt wurde, der genauso funktioniert wie der experimentelle AppVeyor-Build, der in unserem **[Veröffentlichungszyklus](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/release cycle-de-DE)** beschrieben ist. Normalerweise sollten Sie dieses Tag vermeiden, da es die höchste Stufe von fehlerhafter Software ist, die für Entwickler und fortgeschrittene Benutzer zu Entwicklungszwecken bestimmt ist. Das Image wird mit jedem Commit im `main`-GitHub-Zweig aktualisiert, daher kann man sehr oft mit Updates (und Problemen) rechnen, genau wie in unserem AppVeyor Build. Es liegt an uns, den aktuellen Stand des ASF-Projekts zu markieren, der nicht unbedingt garantiert stabil oder getestet ist, wie in unserem Veröffentlichungszyklus dargelegt. Dieses Tag sollte nicht in einer Produktionsumgebung verwendet werden.


### `released`

Sehr ähnlich wie oben, zeigt dieses Tag immer auf die neueste **[veröffentlichte](https://github.com/JustArchiNET/ArchiSteamFarm/releases)** ASF-Version, einschließlich Vorabversionen. Im Vergleich zum `main`-Tag wird dieses Image jedes Mal aktualisiert, wenn ein neuer GitHub-Tag gepusht wird. Geeignet für Fortgeschrittene und Power-User, die es lieben, am Rande dessen zu leben, was als stabil und zugleich frisch angesehen werden kann. Dies ist es, was wir empfehlen würden, wenn Sie nicht den `latest` Tag verwenden möchten. Bitte bedenken Sie, dass die Verwendung dieses Tags gleichbedeutend ist mit der Verwendung unserer **[Vorveröffentlichungen](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle-de-DE)**.


### `latest`

Unter den verfügbaren Tags ist dieses das Einzige, welches automatische Updates enthält und auf die **[stable](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)**-ASF-Version verweist. Der Zweck dieses Tags ist es, einen funktionierenden (Standard)-Docker-Container zur Verfügung zu stellen, der selbstständig in der Lage ist, eine aktualisierte, OS-spezifische ASF-Instanz auszuführen. Dadurch muss das Image nicht so oft auf den neuesten Stand gebracht werden, da die enthaltene ASF-Version immer in der Lage ist, sich bei Bedarf selbst zu aktualisieren. Natürlich kann `UpdatePeriod` ausgeschaltet werden (eingestellt auf `0`), aber in diesem Fall sollten Sie stattdessen wahrscheinlich die eingefrorene Version `A.B.C.D` verwenden. Ebenso kannst können Sie den Standard `UpdateChannel` ändern, um stattdessen ein automatisches Aktualisieren des `released` Tags durchzuführen.

Aufgrund der Tatsache, dass das `latest` Build automatisch aktualisiert werden kann, enthält es ein nacktes Betriebssystem mit der OS-spezifischen ASF-Version, im Gegensatz zu allen anderen Tags, etwa OS mit .NET Core Runtime und `generic` ASF-Version. Dies liegt daran, dass eine neuere (aktualisierte) ASF-Version möglicherweise auch eine neuere Laufzeit erfordert als die, mit der das Build möglicherweise kompiliert werden könnte. Ansonsten würde dies einen Neuaufbau des Images von Grund auf erfordern, wodurch der geplante Anwendungsfall hinfällig wäre.

### `A.B.C.D`

Im Vergleich zu den oben genannten Tags ist dieser Tag vollständig eingefroren, was bedeutet, dass das Image nach der Veröffentlichung nicht mehr aktualisiert wird. Dies funktioniert ähnlich wie bei unseren GitHub-Versionen, die nach der ersten Version nie wieder berührt werden, was Ihnen eine stabile und gefrorene Umgebung garantiert. Normalerweise sollten Sie dieses Tag verwenden, wenn Sie eine bestimmte ASF-Version verwenden (älter als `latest`) und auf Aktualisierungen verzichten möchten (zum Beispiel die, die im Tag `latest` angeboten werden).

---

## Welcher Tag ist für mich der beste?

Das hängt davon ab, wonach Sie suchen. Für die Mehrheit der Benutzer sollte das `latest` Tag das beste sein, da es genau das bietet, was Desktop-ASF bietet, nur in einem speziellen Docker-Container als Dienst. Leute, die ihre Images ziemlich oft neu erstellen und stattdessen lieber die ASF-Version an eine bestimmte Version gebunden haben möchten, können gerne das `released` Tag verwenden. Wenn Sie stattdessen eine bestimmte eingefrorene ASF-Version verwenden möchten, die sich ohne Ihre eindeutige Absicht nie ändern wird, stehen Ihnen `A.B.C.D`-Versionen als feste ASF-Meilensteine zur Verfügung, auf die Sie immer zurückgreifen können.

Wir raten generell davon ab, `main` Builds auszuprobieren, genau wie automatisierte AppVeyor Builds; diese Builds sind für uns da, um den aktuellen Stand des ASF-Projekts zu bestimmen. Nichts garantiert, dass ein solcher Zustand ordnungsgemäß funktioniert, aber natürlich kannst Sie ihn gerne ausprobieren, wenn Sie an der ASF-Entwicklung interessiert sind.

---

## Architekturen

Das ASF Docker Image ist derzeit auf `Linux` Plattform mit 3 Architekturen verfügbar- `x64`, `Arm` und `arm64`. Du kannst im Abschnitt **[Kompatibilität](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-de-DE)** mehr darüber lesen.

Seit ASF-Version V5.0.2.2 verwenden unsere Tags das Multi-Plattform-Manifest, was bedeutet, dass Docker auf Ihrem Rechner automatisch das passende Image für Ihre Plattform auswählt, wenn Sie dieses herunterladen. If by any chance you'd like to pull a specific platform image which doesn't match the one you're currently running, you can do that through `--platform` switch in appropriate docker commands, such as `docker run`. Mehr Informationen hierzu finden Sie in der Docker Dokumentation auf **[image manifest](https://docs.docker.com/registry/spec/manifest-v2-2)**.

---

## Usage

Für eine vollständige Referenz solltest du die **[offizielle Docker-Dokumentation](https://docs.docker.com/engine/reference/commandline/docker)** verwenden. Wir werden in diesem Leitfaden nur die grundlegende Verwendung behandeln. Du bist herzlich dazu eingeladen, noch tiefer zu graben.

### Hallo ASF!

Zuallererst sollten wir überprüfen, ob unser Docker momentan überhaupt funktioniert. Das wird als unser ASF "Hallo Welt" dienen:

```shell
docker run -it --name asf --pull always --rm justarchi/archisteamfarm
```

`docker run` erstellt einen neuen ASF Docker-Container für Sie und lässt ihn im Vordergrund laufen (`-it`). `--pull always` ensures that up-to-date image will be pulled first, and `--rm` ensures that our container will be purged once stopped, since we're just testing if everything works fine for now.

Wenn alles erfolgreich geendet hat, nachdem Sie alle Schichten und den Start-Container geholt haben, sollten Sie feststellen, dass ASF richtig gestartet wurde und es keine definierten Bots gibt, was gut ist - wir haben verifiziert, dass ASF im Docker richtig funktioniert. Drücken Sie `CTRL+P` dann `STRG+Q` um den Vordergrund-Docker-Container zu beenden, dann stoppen Sie den ASF-Container mit `Docker stoppen asf`.

Wenn Sie sich den Befehl genauer ansehen, werden Sie feststellen, dass wir kein Tag deklariert haben, da es automatisch auf `latest` voreingestellt ist. Sofern Sie ein anderes Schlagwort als `latest`verwenden möchten, zum Beispiel `released`, dann sollten Sie es explizit erklären:

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

Und das war's, jetzt verwendet der ASF-Docker-Container das freigegebene Verzeichnis mit Ihrer lokalen Maschine im Lese-/Schreibmodus, was alles ist, was Sie für die Konfiguration von ASF brauchen. Auf ähnliche Weise können Sie andere Volumes einhängen, die Sie mit ASF teilen möchten, wie `/app/logs` oder `/app/plugins`.

Natürlich ist dies nur ein konkreter Weg, um das zu erreichen, was wir wollen, nichts hält Sie davon ab, z. B. eine eigene `Dockerfile` zu erstellen, die Ihre Konfigurationsdateien in das Verzeichnis `/app/config` im ASF Docker-Container kopiert. Wir behandeln in diesem Leitfaden nur die grundlegende Verwendung.

### Zugriffsrechte für Volumes

ASF wird standardmäßig mit dem Standard `root` Benutzer innerhalb eines Containers ausgeführt. Dies ist sicherheitstechnisch kein Problem, da wir uns bereits im Docker-Container befinden, aber es wirkt sich auf das freigegebene Volume aus, da neu generierte Dateien normalerweise `root` gehören, was bei der Verwendung eines freigegebenen Volumes möglicherweise nicht gewünscht ist.

Docker erlaubt es dir, `--user` **[Flag](https://docs.docker.com/engine/reference/run/#user)** an `docker run` Befehl zu übergeben, der den Standardbenutzer definiert, unter dem ASF laufen wird. Sie können Ihre `uid` und `gid` zum Beispiel mit dem Befehl `id` überprüfen und ihn dann an den Rest des Befehls übergeben. Zum Beispiel, wenn Ihr Ziel-Benutzer die `uid` und `gid` von 1000 hat:

```shell
docker run -it -u 1000:1000 -v /home/archi/ASF/config:/app/config --name asf --pull always justarchi/archisteamfarm
```

Bedenken Sie, dass das von ASF verwendete Verzeichnis `/app` standardmäßig immer noch im Besitz von `root` ist. Wenn Sie ASF mit einem benutzerdefinierten Benutzer ausführen, hat der ASF-Prozess keinen Schreibzugriff auf seine eigenen Dateien. Dieser Zugriff ist für den Betrieb nicht zwingend erforderlich, aber er ist z. B. für die Funktion Automatische-Aktualisierung entscheidend. Um dies zu beheben, genügt es, den Besitzer aller ASF-Dateien von Standard `root` auf Ihren neuen benutzerdefinierten Benutzer zu ändern.

```shell
docker exec -u root asf chown -hR 1000:1000 /app
```

Dies muss nur einmal durchgeführt werden, nachdem Sie einen Container mit `docker run` erstellt haben, und nur dann, wenn Sie sich dazu entschieden haben, einen benutzerdefinierten Benutzer für den ASF-Prozess zu verwenden. Vergessen Sie bitte auch nicht, das Argument `1000:1000` in beiden obigen Befehlen auf die `uid` und `gid` zu ändern, unter dem Sie ASF ausführen möchten.

---

## Synchronisation mehrerer Instanzen

ASF unterstützt die Synchronisation mehrerer Instanzen, wie im Abschnitt **[Kompatibilität](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-de-DE#mehrere-instanzen)**. Beim Ausführen von ASF im Docker-Container können Sie optional "opt-in" in den Prozess einschalten für den Fall, dass Sie mehrere Container mit ASF (synchronisiert) verwenden möchten.

Standardmäßig ist jeder ASF in einem eigenständigen Docker-Container und das bedeutet, dass keine Synchronisation stattfindet. Um die Synchronisation zwischen ihnen zu aktivieren, müssen Sie den Pfad `/tmp/ASF` in jedem ASF Container, den Sie synchronisieren möchten, zu einem gemeinsamen Pfad (im Lese-Schreib-Modus) auf dem Docker-Host einbinden. Dies wird genauso wie bei der Bindung eines Volumens erreicht, das oben beschrieben wurde, nur mit verschiedenen Pfaden:

```shell
mkdir -p /tmp/ASF-g1
docker run -v /tmp/ASF-g1:/tmp/ASF -v /home/archi/ASF/config:/app/config --name asf1 --pull always justarchi/archisteamfarm
docker run -v /tmp/ASF-g1:/tmp/ASF -v /home/john/ASF/config:/app/config --name asf2 --pull always justarchi/archisteamfarm
# And so on, all ASF containers are now synchronized with each other
```

Wir empfehlen das ASF-Verzeichnis `/tmp/ASF` auch an ein temporäres `/tmp` Verzeichnis auf Ihrem Rechner zu binden, aber Sie können natürlich jedes andere wählen, die Ihre Nutzung befriedigt. Jeder ASF-Container, der synchronisiert werden soll, sollte sein `/tmp/ASF` Verzeichnis mit anderen Containern teilen, die am gleichen Synchronisierungsprozess teilnehmen.

Wie Sie vermutlich am Beispiel oben erraten haben, ist es auch möglich, zwei oder mehr "Synchronisierungsgruppen" zu erstellen indem Sie verschiedene Docker Host-Pfade in ASFs `/tmp/ASF` einbinden.

Das einbinden von `/tmp/ASF` ist komplett optional und aktuell nicht empfehlenswert, es sei denn, Sie möchten explizit zwei oder mehr ASF-Container synchronisieren. Wir raten davon ab, `/tmp/ASF` für den Einsatz mit einem Container einzubinden, da es absolut keine Vorteile bringt, wenn Sie nur einen ASF-Container laufen lassen wollen, und es könnte tatsächlich zu Problemen führen, die sonst vermieden werden könnten.

---

## Command-line arguments

ASF erlaubt es Ihnen **[Kommandozeilenargumente](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments)** in Docker-Containern durch Umgebungsvariablen zu geben. Sie sollten spezifische Umgebungsvariablen für die unterstützten Schalter benutzen und für den Rest `ASF_ARGS`. Dies kann mit dem Schalter `-e` erreicht werden, der zum `Docker Run`hinzugefügt wurde; zum Beispiel:

```shell
docker run -it -e "ASF_CRYPTKEY=MyPassword" -e "ASF_ARGS=--process-required" --name asf --pull always justarchi/archisteamfarm
```

Dies wird sowohl das Argument `--cryptkey` korrekt an den ASF-Prozess übergeben, der im Docker-Container ausgeführt wird, als auch andere Argumente. Falls Sie ein fortgeschrittener Benutzer sind, können Sie natürlich auch den `ENTRYPOINT` bearbeiten oder eine `CMD` hinzufügen und Ihre Argumente selbst übergeben.

Sofern Sie keinen benutzerdefinierten Verschlüsselungsschlüssel oder andere erweiterte Optionen bereitstellen möchten, müssen Sie in der Regel keine speziellen Umgebungsvariablen einbinden, da unsere Docker-Container bereits so konfiguriert sind, dass sie mit zu erwarteten Standardoptionen von `--no-restart` `--process-required` `--system-required` funktionieren. Wie Sie sehen können, ist `ASF_ARGS` in diesem Fall redundant und nur `ASF_CRYPTKEY` relevant.

---

## IPC

Assuming you didn't change the default value for `IPC` **[global configuration property](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#global-config)**, it's already enabled, but you have to modify default listening address of `localhost`, as docker can't route outside traffic to loopback interface. Ein Beispiel für eine Einstellung, die auf allen Schnittstellen lauscht, wäre `http://*:1242`. Natürlich können Sie auch restriktivere Bindungen verwenden, wie z. B. nur lokales LAN oder VPN-Netzwerk, aber es muss eine von außen zugängliche Route sein - `localhost` reicht nicht aus, da die Route vollständig innerhalb des Gastcomputers liegt.

Um das oben Gesagte zu tun, solltest du eine **[benutzerspezifische IPC-Konfiguration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-de-DE#benutzerdefinierte-konfiguration)** verwenden, wie die untenstehende:

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

Wenn du alles richtig eingestellt hast, wird der oben gezeigte `docker run` Befehl das **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-de-DE)** Interface von deinem Host-Computer aus auf der Standardroute `localhost:1242` laufen lassen, die nun korrekt auf deine Gast-Maschine umgeleitet wird. Es ist auch gut zu wissen, dass wir diese Route nicht weiter offenlegen, sodass die Verbindung nur innerhalb des Docker-Hosts erfolgen kann und somit sicher bleibt. Natürlich können Sie den Weg weiter ausblenden, wenn Sie wissen, was Sie tun, und für entsprechende Sicherheitsmaßnahmen sorgen.

---

### Vollständiges Beispiel

Wenn man das gesamte obige Wissen kombiniert, würde ein Beispiel für ein komplettes Setup so aussehen:

```shell
docker run -it -p 127.0.0.1:1242:1242 -p [::1]:1242:1242 -v /home/archi/asf:/app/config --name asf --pull always justarchi/archisteamfarm
```

Dies setzt voraus, dass Sie einen einzigen ASF-Container mit allen ASF-Konfigurationsdateien in `/home/archi/asf` verwenden. Sie sollten den Konfigurationspfad zu dem ändern, der zu Ihrem Rechner passt. Dieses Setup ist auch für die optionale IPC-Nutzung bereit, wenn Sie sich entschieden haben, `IPC.config` in ein Konfigurationsverzeichnis mit einem Inhalt wie unten beschrieben mit einzubeziehen:

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

Wenn der ASF-Docker-Container bereits startklar ist, müssen Sie nicht jedes Mal `docker run` verwenden. Sie können den ASF-Docker-Container einfach mit `docker stop asf`/ `docker start asf` stoppen/ starten. Keep in mind that if you're not using `latest` tag then using up-to-date ASF will still require from you to `docker stop`, `docker rm` and `docker run` again. Dies liegt daran, dass Sie einen Container jedes Mal aus einem frischen ASF-Docker-Image neu erstellen müssen, wenn Sie die in diesem Image enthaltene ASF-Version verwenden möchten. In `latest` Tag hat ASF die Möglichkeit, sich selbst automatisch zu aktualisieren, sodass ein Neuaufbau des Images nicht notwendig ist um aktuelles ASF zu verwenden (aber es könnte trotzdem eine gute Idee sein, dies von Zeit zu Zeit zu tun, um frische .NET Core Runtime und das zugrunde liegende Betriebssystem zu verwenden).

Wie oben angedeutet, wird sich ASF in einem anderen Tag als `latest` nicht automatisch aktualisieren, was bedeutet, dass **du** dafür verantwortlich bist, aktuelles `justarchi/archisteamfarm` Repository zu verwenden. Dies hat viele Vorteile, da die Anwendung typischerweise nicht ihren eigenen Quellcode berühren sollte, wenn sie ausgeführt wird, aber wir verstehen auch Komfort, der dadurch entsteht, dass Sie sich nicht um die ASF-Version in Ihrem Docker-Container kümmern müssen. Wenn Sie sich um gute Praktiken und die korrekte Verwendung von Dockern kümmern, ist `released` Tag das, was wir vorschlagen würden, anstatt `latest`, aber wenn Sie sich nicht darum kümmern können und ASF nur dazu bringen möchten, sowohl zu funktionieren, als auch sich selbst automatisch zu aktualisieren, dann reicht `latest`.

Sie sollten ASF typischerweise im Docker-Container mit der globalen Konfiguration `Headless: true` ausführen. Dies wird ASF klar signalisieren, dass Sie nicht hier sind, um fehlende Details zu liefern und es sollte nicht nach diesen fragen. Natürlich solltest du für die Erstkonfiguration in Betracht ziehen, diese Option bei `false` zu belassen, damit du Dinge einfach einrichten kannst, aber auf lange Sicht bist du typischerweise nicht an die ASF-Konsole gebunden, deshalb wäre es sinnvoll, ASF darüber zu informieren und `input` **[Befehl](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE#input-befehl)** zu verwenden, wenn Bedarf entsteht. Auf diese Weise muss ASF nicht unendlich auf Benutzereingaben warten, die nicht erfolgen (und dabei Ressourcen verschwenden). Dies ermöglicht es, dass ASF im nicht-interaktiven Modus innerhalb eines Containers ausgeführt werden kann, was entscheidend ist, z. B. in Bezug auf die Weiterleitungssignale, die es ASF ermöglichen, auf Anfrage von `docker stop asf` angemessen zu schließen.