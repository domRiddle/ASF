# Docker

Seit Version 3.0.3.2 steht ASF nun auch als **[Docker-Container](https://www.docker.com/what-container)** zur Verfügung. ASF im Docker-Container laufen zu lassen hat in der Regel keine Vorteile für gelegentliche Nutzer. Es könnte aber eine hervorragende Möglichkeit zur Nutzung von ASF auf Servern sein, um sicherzustellen dass ASF in einer Sandbox-Umgebung getrennt von allen anderen Anwendungen ausgeführt wird. Unsere Docker-Repository findest du **[hier](https://hub.docker.com/r/justarchi/archisteamfarm)**.

* * *

## Tags

ASF ist durch 4 Haupttypen von **[Tags](https://hub.docker.com/r/justarchi/archisteamfarm/tags)** verfügbar:

### `master`

Dieses Tag zeigt immer auf den ASF-Build, der aus dem neuesten Commit im Master-Zweig erstellt wurde, der genauso funktioniert wie der experimentelle AppVeyor-Build, der in unserem **[Veröffentlichungszyklus](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle-de-DE)** beschrieben ist. Normalerweise solltest du dieses Tag vermeiden, da es die höchste Stufe von fehlerhafter Software ist, die für Entwickler und fortgeschrittene Benutzer zu Entwicklungszwecken bestimmt ist. Das Image wird mit jedem Commit im Master GitHub Zweig aktualisiert, daher kann man sehr oft mit Updates (und Problemen) rechnen, genau wie in unserem AppVeyor Build. Es liegt an uns, den aktuellen Stand des ASF-Projekts zu markieren, der nicht unbedingt garantiert stabil oder getestet ist, wie in unserem Veröffentlichungszyklus dargelegt. Dieses Tag sollte nicht in einer Produktionsumgebung verwendet werden.

### `released`

Sehr ähnlich wie oben, zeigt dieses Tag immer auf die neueste **[veröffentlichte](https://github.com/JustArchiNET/ArchiSteamFarm/releases)** ASF-Version, einschließlich Vorabversionen. Im Vergleich zu `master` Tag wird dieses Bild jedes Mal aktualisiert, wenn ein neuer GitHub-Tag gepusht wird. Geeignet für Fortgeschrittene und Power-User, die es lieben, am Rande dessen zu leben, was als stabil und zugleich frisch angesehen werden kann. Dies ist es, was wir empfehlen würden, wenn du nicht den `latest` Tag verwenden möchtest. Bitte bedenke, dass die Verwendung dieses Tags gleichbedeutend ist mit der Verwendung unserer **[Vorveröffentlichungen](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle-de-DE)**.

### `latest`

Dieses Tag zeigt im Vergleich zu den beiden vorherigen, da das erste die ASF-Funktion für automatische Aktualisierungen beinhaltet, typischerweise auf diejenige der stabilen Versionen, aber nicht unbedingt auf die neueste. Das Ziel dieses Tags ist es, einen gesunden Standard-Docker-Container bereitzustellen, der in der Lage ist, selbst-aktualisierendes ASF auszuführen. Dadurch muss das Image nicht so oft auf den neuesten Stand gebracht werden, da die enthaltene ASF-Version immer in der Lage ist, sich bei Bedarf selbst zu aktualisieren. Natürlich kann `UpdatePeriod` ausgeschaltet werden (eingestellt auf `0`), aber in diesem Fall solltest du stattdessen wahrscheinlich die eingefrorene Version `A.B.C.D` verwenden. Ebenso kannst du den Standard `UpdateChannel` ändern, um stattdessen ein automatisches Aktualisieren des `released` Tags durchzuführen.

### `A.B.C.D`

Im Vergleich zu den oben genannten Tags ist dieser Tag vollständig eingefroren, was bedeutet, dass das Image nach der Veröffentlichung nicht mehr aktualisiert wird. Dies funktioniert ähnlich wie bei unseren GitHub-Versionen, die nach der ersten Version nie wieder berührt werden, was dir eine stabile und gefrorene Umgebung garantiert. Typically you should use this tag when you want to use some specific ASF release (older than `latest`) and you don't want to use any kind of auto-updates (e.g. those offered in `latest` tag).

* * *

## Welcher Tag ist für mich der beste?

Das hängt davon ab, wonach du suchst. Für die Mehrheit der Benutzer sollte das `latest` Tag das beste sein, da es genau das bietet, was Desktop-ASF bietet, nur in einem speziellen Docker-Container als Dienst. Leute, die ihre Images ziemlich oft neu erstellen und stattdessen lieber die ASF-Version an eine bestimmte Version gebunden haben möchten, können gerne `released` Tag verwenden. Wenn du stattdessen eine bestimmte eingefrorene ASF-Version verwenden möchtest, die sich ohne deine eindeutige Absicht nie ändern wird, stehen dir `A.B.C.D` Versionen als feste ASF-Meilensteine zur Verfügung, auf die du immer zurückgreifen kannst.

Wir raten generell davon ab, `master` Builds auszuprobieren, genau wie automatisierte AppVeyor Builds - diese Builds sind für uns da um den aktuellen Stand des ASF-Projekts zu bestimmen. Nichts garantiert, dass ein solcher Zustand ordnungsgemäß funktioniert, aber natürlich kannst du ihn gerne ausprobieren, wenn du an der ASF-Entwicklung interessiert bist.

* * *

## Architekturen

Ein ASF-Docker-Image steht derzeit für zwei Architekturen zur Verfügung - `X64` und `arm`. Du kannst im Abschnitt **[Kompatibilität](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-de-DE)** mehr darüber lesen.

Da Multi-Arch Docker-Tags noch in Arbeit sind, sind Builds für andere Architekturen als den Standard `x64` derzeit verfügbar, wobei `-{ARCH}` an den Tag-Namen angehängt ist. Mit anderen Worten, wenn du `latest` Tag für `arm` Architektur verwenden willst, verwende einfach `latest-arm`.

* * *

## Nutzung

Für eine vollständige Referenz solltest du die **[offizielle Docker-Dokumentation](https://docs.docker.com/engine/reference/commandline/docker)** verwenden. Wir werden in diesem Leitfaden nur die grundlegende Verwendung behandeln. Du bist herzlich dazu eingeladen, noch tiefer zu graben.

### Hallo ASF!

Zuallererst sollten wir überprüfen, ob unser Docker momentan überhaupt funktioniert. Das wird als unser ASF "Hallo Welt" dienen:

```shell
docker pull justarchi/archisteamfarm
docker run -it --name asf justarchi/archisteamfarm
```

Der `docker pull`-Befehl sorgt dafür, dass du ein aktuelles `Justarchi/Archisteamfarm`-Abbild verwendst - für den Fall, dass du eine veraltete lokale Kopie im Cache hattest. `docker run` erstellt einen neuen ASF Docker-Container für dich und lässt ihn im Vordergrund laufen (`-it`).

Wenn alles erfolgreich geendet hat, nachdem du alle Schichten und den Start-Container geholt hast, solltest du feststellen, dass ASF richtig gestartet wurde und uns mitgeteilt hast, dass es keine definierten Bots gibt, was gut ist - wir haben verifiziert, dass ASF im Docker richtig funktioniert. Drück' `STRG+P` dann `STRG+Q`, um den Vordergrund-Docker-Container zu verlassen, dann stopp' den ASF-Container mit `docker stop asf` und entferne ihn mit `docker rm asf`.

Wenn du dir den Befehl genauer ansiehst, wirst du feststellen, dass wir kein Tag deklariert haben, da es automatisch auf `latest` voreingestellt ist. Wenn du ein anderes Tag als `latest` verwenden möchtest, z.B. `latest-arm`, dann solltest du es explizit deklarieren:

```shell
docker pull justarchi/archisteamfarm:latest-arm
docker run -it --name asf justarchi/archisteamfarm:latest-arm
```

* * *

## Verwendung eines Volumes

Wenn du ASF im Docker-Container verwendest, dann musst du natürlich das Programm selbst konfigurieren. Du kannst es auf verschiedene Weise tun, aber die empfohlene wäre, das ASF `config` Verzeichnis auf der lokalen Maschine zu erstellen und es dann als gemeinsames Volume im ASF-Docker-Container zu mounten.

Zum Beispiel gehen wir davon aus, dass sich dein ASF-Konfigurationsordner im Verzeichnis `/home/archi/ASF/config` befindet. Dieses Verzeichnis enthält den Kern `ASF.json` sowie Bots, die wir ausführen wollen. Jetzt müssen wir nur noch dieses Verzeichnis als Shared Volume in unserem Docker-Container anhängen, wo ASF sein Konfigurationsverzeichnis erwartet (`/app/config`).

```shell
docker pull justarchi/archisteamfarm
docker run -it -v /home/archi/ASF/config:/app/config --name asf justarchi/archisteamfarm
```

Und das war's, jetzt verwendet dein ASF-Docker-Container das freigegebene Verzeichnis mit deiner lokalen Maschine im Lese-/Schreibmodus, was alles ist, was du für die Konfiguration von ASF brauchst.

Natürlich ist dies nur ein konkreter Weg, um das zu erreichen, was wir wollen, nichts hält dich davon ab, z.B. eine eigene `Dockerfile` zu erstellen, die deine Konfigurationsdateien in das Verzeichnis `/app/config` im ASF Docker-Container kopiert. Wir behandeln in diesem Leitfaden nur die grundlegende Verwendung.

### Zugriffsrechte für Volumes

ASF wird standardmäßig mit dem Standard `root` Benutzer innerhalb eines Containers ausgeführt. Dies ist sicherheitstechnisch kein Problem, da wir uns bereits im Docker-Container befinden, aber es wirkt sich auf das freigegebene Volume aus, da neu generierte Dateien normalerweise `root` gehören, was bei der Verwendung eines freigegebenen Volumes möglicherweise nicht gewünscht ist.

Docker erlaubt es dir, `--user` **[Flag](https://docs.docker.com/engine/reference/run/#user)** an `docker run` Befehl zu übergeben, der den Standardbenutzer definiert, unter dem ASF laufen wird. Du kannst deine `uid` und `gid` zum Beispiel mit dem Befehl `id` überprüfen und ihn dann an den Rest des Befehls übergeben. Zum Beispiel, wenn dein Ziel-Benutzer die `uid` und `gid` von 1000 hat:

```shell
docker pull justarchi/archisteamfarm
docker run -it -u 1000:1000 -v /home/archi/ASF/config:/app/config --name asf justarchi/archisteamfarm
```

Bedenke, dass das von ASF verwendete Verzeichnis `/app` standardmäßig immer noch im Besitz von `root` ist. Wenn du ASF mit einem benutzerdefinierten Benutzer ausführst, hat dein ASF-Prozess keinen Schreibzugriff auf seine eigenen Dateien. Dieser Zugriff ist für den Betrieb nicht zwingend erforderlich, aber er ist z.B. für die Funktion Automatische-Aktualisierung entscheidend. Um dies zu beheben, genügt es, den Besitzer aller ASF-Dateien von Standard `root` auf deinen neuen benutzerdefinierten Benutzer zu ändern.

```shell
docker exec -u root asf chown -hR 1000:1000 /app
```

Dies muss nur einmal durchgeführt werden, nachdem du deinen Container mit `docker run` erstellt hast, und nur dann, wenn du dich dazu entschieden hast, einen benutzerdefinierten Benutzer für den ASF-Prozess zu verwenden. Vergiss auch nicht, das Argument `1000:1000` in beiden obigen Befehlen auf die `uid` und `gid` zu ändern, unter dem du ASF ausführen möchtest.

* * *

## Befehlszeilenargumente

ASF erlaubt es dir im Docker-Container **[Befehlszeilenargumente](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments-de-DE)** zu übergeben, indem du die `ASF_ARGS` Umgebungsvariable verwendest. Dies kann zusätzlich zu `docker run` mit dem `-e` Schalter hinzugefügt werden. Zum Beispiel:

```shell
docker pull justarchi/archisteamfarm
docker run -it -e "ASF_ARGS=--cryptkey MeinPasswort" --name asf justarchi/archisteamfarm
```

Dies wird dein Argument `--cryptkey` korrekt an den ASF-Prozess übergeben, der im Docker-Container ausgeführt wird. Natürlich, wenn du ein fortgeschrittener Benutzer bist, dann kannst du auch den `ENTRYPOINT` ändern und deine eigenen Argumente übergeben.

Sofern du keinen benutzerdefinierten Verschlüsselungsschlüssel oder andere erweiterte Optionen bereitstellen möchtest, musst du normalerweise keine speziellen `ASF_ARGS` einfügen, da unsere Docker-Container bereits so konfiguriert sind, dass sie mit einer sinnvollen Anzahl von Standardeinstellungen von `--no-restart` `--process-required` `--system-quired` laufen.

* * *

## IPC

Für die Verwendung von IPC solltest du zunächst `IPC` **[globale Konfigurationseigenschaft](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-de-DE#ipc)** auf `true` umschalten. Darüber hinaus **musst** du die Standard-Abhöradresse von `localhost` ändern, da Docker keinen externen Traffic an die Loopback-Schnittstelle umleiten kann. Ein Beispiel für eine Einstellung, die auf allen Schnittstellen lauscht, wäre `http://*:1242`. Natürlich kannst du auch restriktivere Bindungen verwenden, wie z.B. nur lokales LAN oder VPN-Netzwerk, aber es muss eine von außen zugängliche Route sein - `localhost` reicht nicht aus, da die Route vollständig innerhalb des Gastcomputers liegt.

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
docker pull justarchi/archisteamfarm
docker run -it -p 127.0.0.1:1242:1242 -p [::1]:1242:1242 --name asf justarchi/archisteamfarm
```

Wenn du alles richtig eingestellt hast, wird der oben gezeigte `docker run` Befehl das **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-de-DE)** Interface von deinem Host-Computer aus auf der Standardroute `localhost:1242` laufen lassen, die nun korrekt auf deine Gast-Maschine umgeleitet wird. Es ist auch gut zu wissen, dass wir diese Route nicht weiter offenlegen, so dass die Verbindung nur innerhalb des Docker-Hosts erfolgen kann und somit sicher bleibt.

* * *

### Vollständiges Beispiel

Wenn man das gesamte obige Wissen kombiniert, würde ein Beispiel für ein komplettes Setup so aussehen:

```shell
docker pull justarchi/archisteamfarm
docker run -it -p 127.0.0.1:1242:1242 -p [::1]:1242:1242 -v /home/archi/asf:/app/config --name asf justarchi/archisteamfarm
```

Dies setzt voraus, dass du alle ASF-Konfigurationsdateien in `/home/archi/asf` hast, wenn nicht, solltest du den Pfad zu demjenigen ändern, der passt. Dieses Setup ist auch für die optionale IPC-Nutzung bereit, wenn du dich entschieden hast, `IPC.config` in dein Konfigurationsverzeichnis mit einem Inhalt wie unten beschrieben aufzunehmen:

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

* * *

## Profi-Tipps

Wenn du deinen ASF-Docker-Container bereits startklar hast, musst du nicht jedes Mal `docker run` verwenden. Du kannst den ASF-Docker-Container einfach stoppen/starten mit `docker stop asf` und `docker start asf`. Bedenke, dass wenn du nicht `latest` Tag benutzt, dann wird die Aktualisierung von ASF immer noch von dir verlangt, um `docker stop`, `docker rm`, `docker pull` und `docker run` wieder zu aktualisieren. Dies liegt daran, dass du deinen Container jedes Mal aus einem frischen ASF-Docker-Image neu erstellen musst, wenn du die in diesem Image enthaltene ASF-Version verwenden möchtest. In `latest` Tag hat ASF die Möglichkeit, sich selbst automatisch zu aktualisieren, so dass ein Neuaufbau des Images nicht notwendig ist um aktuelles ASF zu verwenden (aber es könnte trotzdem eine gute Idee sein, dies von Zeit zu Zeit zu tun, um frische .NET Core Runtime und das zugrunde liegende Betriebssystem zu verwenden).

Wie oben angedeutet, wird sich ASF in einem anderen Tag als `latest` nicht automatisch aktualisieren, was bedeutet, dass **du** dafür verantwortlich bist, aktuelles `justarchi/archisteamfarm` Repository zu verwenden. Dies hat viele Vorteile, da die Anwendung typischerweise nicht ihren eigenen Quellcode berühren sollte, wenn sie ausgeführt wird, aber wir verstehen auch Komfort, der dadurch entsteht, dass du dich nicht um die ASF-Version in deinem Docker-Container kümmern musst. Wenn du dich um gute Praktiken und die korrekte Verwendung von Dockern kümmerst, ist `released` Tag das, was wir vorschlagen würden, anstatt `latest`, aber wenn du dich nicht darum kümmern kannst und du ASF nur dazu bringen willst, sowohl zu funktionieren als auch sich selbst automatisch zu aktualisieren, dann reicht `latest`.

Du solltest ASF typischerweise im Docker-Container mit der globalen Konfiguration `Headless: true` ausführen. Dies wird ASF klar signalisieren, dass du nicht hier bist, um fehlende Details zu liefern und es sollte nicht nach diesen fragen. Natürlich solltest du für die Erstkonfiguration in Betracht ziehen, diese Option bei `false` zu belassen, damit du Dinge einfach einrichten kannst, aber auf lange Sicht bist du typischerweise nicht an die ASF-Konsole gebunden, deshalb wäre es sinnvoll, ASF darüber zu informieren und `input` **[Befehl](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE#input-befehl)** zu verwenden, wenn Bedarf entsteht. Auf diese Weise muss ASF nicht unendlich auf Benutzereingaben warten die nicht erfolgen (und dabei Ressourcen verschwenden).