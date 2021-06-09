# Installation

Wenn du hier zum ersten Mal angekommen bist, herzlich willkommen! Wir freuen uns sehr, einen weiteren Reisenden zu sehen, der sich für unser Projekt interessiert, obwohl wir berücksichtigen müssen, dass mit großer Macht eine große Verantwortung einhergeht - ASF ist in der Lage, viele verschiedene Dinge rund um Steam zu tun, aber nur, solange man sich **genug kümmert, um zu lernen, wie man ihn benutzt**. Es gibt hier eine steile Lernkurve, und wir erwarten von dir, dass du das Wiki in dieser Hinsicht liest, das im Detail erklärt, wie alles funktioniert.

Wenn du immer noch hier bist, heißt das, dass du den Text oben überstanden hast, was toll ist. Außer du hast ihn übersprungen, was heißt, dass du bald **[schlechte Zeiten](https://www.youtube.com/watch?v=WJgt6m6njVw)** vor Ihrenhast... Anyway, ASF is a console app, which means that the program itself doesn't have a friendly GUI that you're in general used to, at least out of the box. ASF sollte hauptsächlich auf Servern laufen, sodass es als Dienst (Daemon) und nicht als Desktop-App fungiert.

Das bedeutet jedoch nicht, dass du es nicht auf Ihrem PC benutzen kannst oder dass die Benutzung etwas komplizierter ist als sonst, nichts dergleichen. ASF ist ein eigenständiges Programm, das keine Installation benötigt und sofort einsatzbereit ist. Allerdings wird eine Konfiguration erfordert, bevor es nützlich wird. Die Konfiguration sagt ASF, was es nach dem Start tatsächlich tun soll. Wenn du ASF ohne Konfiguration startest, dann wird es nichts tun. Ganz einfach.

---

## Betriebssystemspezifisches Setup

Folgendes werden wir in den nächsten paar Minuten machen:
- **[.NET Core Prerequisites](#net-core-prerequisites)** installieren.
- Die **[neueste ASF Version](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** in der entsprechenden betriebssystemspezifischen Variante herunterladen.
- Das Archiv in einen neuen Ordner entpacken (und mit `chmod +x ArchiSteamFarm` ausführbar machen, wenn du dich auf Linux oder OS X befindest).
- **[ASF konfigurieren](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-de-DE)**.
- ASF starten und der Magie ihren Lauf lassen.

Hört sich einfach an, richtig? Dann lassen Sie uns beginnen.

---

### .NET Core Prerequisites

Der erste Schritt ist sicherzustellen, dass dein Betriebssystem ASF überhaupt richtig ausführen kann. ASF ist in C# programmiert, basierend auf .NET Core und benötigt möglicherweise native Bibliotheken, die auf Ihrer Plattform noch nicht verfügbar sind. Abhängig davon, ob du Windows, Linux oder OS X verwendest, wirst du unterschiedliche Voraussetzungen haben, welche allerdings alle im **[.NET Core Abhängigkeiten](https://docs.microsoft.com/dotnet/core/install)**-Dokument, dem du folgen solltest, aufgelistet sind. Dieses ist Referenzmaterial, das verwendet werden sollte, allerdings haben wir im Sinne der Einfachheit auch alle benötigten Pakete unten aufgelistet, damit du nicht das gesamte Dokument lesen musst.

Es ist völlig normal, dass manche (oder sogar alle) Abhängigkeiten bereits in Ihrem System existieren, weil sie mit der Software Dritter, welche du verwendest, mitinstalliert wurden. Trotzdem solltest du sicherstellen, dass dies wirklich der Fall ist indem du das entsprechende Installationsprogramm für dein Betriebssytem ausführst - Ohne diese Abhängigkeiten wird ASF nicht einmal starten.

Denken Sie daran, dass Sie für betriebssystemspezifische ASF-Versionen nichts weiteres tun müssen. Insbesondere betrifft dies die Installation des .NET Core SDKs oder der Runtime, da die betriebssytemspezifische Versionen das alles bereits beinhalten. Sie benötigen nur die .NET Core Prerequisites (Abhängigkeiten) um die .NET Core Runtime, die bereits in ASF enthalten ist, auszuführen.

#### **[Windows](https://docs.microsoft.com/de-de/dotnet/core/install/windows)**:
- **[Microsoft Visual C++ 2015 Redistributable Update](https://www.microsoft.com/en-us/download/details.aspx?id=53587)** (x64 for 64-bit Windows, x86 for 32-bit Windows)
- Es wird dringend empfohlen sicherzustellen, dass alle Windows-Updates bereits installiert sind. Sie benötigen mindestens **[KB2533623](https://support.microsoft.com/en-us/help/2533623/microsoft-security-advisory-insecure-library-loading-could-allow-remot)** und **[KB2999226](https://support.microsoft.com/en-us/help/2999226/update-for-universal-c-runtime-in-windows)**, aber es könnten weiteren Aktualisierungen benötigt werden. Wenn dein Windows aktuell ist, sind diese bereits alle installiert. Versichere dich, dass du diese Voraussetzungen erfüllst, bevor du das Visual C++ Paket installierst.

#### **[Linux](https://docs.microsoft.com/de-de/dotnet/core/install/linux)**:
Paketnamen hängen von der Linux-Distribution, die du verwendest, ab. Wir listen nur die Gebräuchlichsten auf. Sie können alle über den nativen Paketmanager für dein Betriebssystem (wie zum Beispiel `apt` unter Debian oder `yum` unter CentOS) installieren.

- `libc6` (`libc`)
- `libgcc1` (`libgcc`)
- `libicu` (`icu-libs`, die neueste Version für Ihre Distribution, zum Beispiel `libicu67`)
- `libgssapi-krb5-2` (`libkrb5-3`, `krb5-libs`)
- `libssl1.1` (`libssl`, `openssl-libs`, die neueste Version für Ihre Distribution, `1.1.X` or `1.0.X`)
- `libstdc++6` (`libstdc++`, Version `5.0` oder höher)
- `zlib1g` (`zlib`)

Zumindest ein Großteil davon sollte bereits nativ auf Ihrem System verfügbar sein. Die minimale Installation von Debian Stable erforderte nur `libicu63`.

#### **[OS X](https://docs.microsoft.com/de-de/dotnet/core/install/macos)**:
- Keine vorerst, aber du solltest die neueste Version von OS X installiert haben, mindestens 10.13+.

---

### Herunterladen

Da wir nun alle benötigten Abhängigkeiten installiert haben, ist der nächste Schritt das Herunterladen der **[neuesten ASF-Version](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)**. ASF ist in mehreren Varianten verfügbar, aber du bist nur am Paket interessiert, das Ihrem Betriebssystem und der Architektur Ihres PCs entspricht. Zum Beispiel, wenn du `64`-Bit `Win`dows verwendest, dann willst du die `ASF-win-x64`-Variante. Für mehr Information über die verfügbaren Varianten, besuche bitte den Abschnitt **[ Kompatibilität](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-de-DE)**. ASF kann auch unter Betriebssystemen ausgeführt werden, für die wir kein betriebssystemspezifisches Paket zur Verfügung stellen. Ein Beispiel hierfür ist **32-bit Windows**. Dafür gehe bitte zur **[generischen Installation](#generic-setup)**.

![Assets](https://i.imgur.com/Ym2xPE5.png)

Beginne nach dem Download damit, die Zip-Datei in einen eigenen Ordner zu entpacken. Wir empfehlen die Verwendung von **[7-zip](https://www.7-zip.org)**, aber alle Standard-Dienstprogramme wie `unzip` unter Linux/OS X sollten ebenfalls ohne Probleme funktionieren.

Wenn Sie Linux oder OS X verwenden, vergessen Sie nicht, `chmod +x ArchiSteamFarm` in dem extrahierten Ordner anzugeben, da die Berechtigungen in der Zip-Datei nicht automatisch gesetzt werden. Dies muss nur nach dem initialen Entpacken gemacht werden.

Stelle bitte sicher, dass du ASF in **einen eigenen Ordner** entpackst und nicht in einen bereits existenten, der für etwas anderes verwendet wird - ASFs automatische Aktualisierungen werden alle alten Dateien in diesem Ordner löschen, was möglicherweise dazu führen könnte, dass du Dateien verlierst, die nichts mit ASF zu tun haben aber im selben Ordner sind. Solltest du zusätzliche Skripte oder Dateien haben, die du mit ASF verwenden willst, solltest du sie in den Ordner darüber tun.

Eine Beispiel-Struktur würde wie folgt aussehen:

```text
C:\ASF (wo Sie Ihre eigenen Dateien speichern)
    ├── ASF shortcut.lnk (optional)
    ├── Config shortcut.lnk (optional)
    ├── Commands.txt (optional)
    ├── MyExtraScript.bat (optional)
    ├── (...) (alle anderen Dateien Ihrer Wahl, optional)
    └── Core (nur ASF gewidmet, wo Sie das Archiv extrahieren)
         ├── ArchiSteamFarm(.exe)
         ├── config
         ├── logs
         ├── plugins
         └── (...)
```

---

### Konfiguration

Wir sind nun bereit, den allerletzten Schritt, die Konfiguration, durchzuführen. Dies ist bei weitem der komplizierteste Schritt, da es sich um eine Menge neuer Informationen handelt, die du noch nicht kennst, also werden wir versuchen, hier einige leicht verständliche Beispiele und vereinfachte Erklärungen zu geben.

In erster Linie gibt es die Seite **[Installation](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-de-DE)**, die **alles** erklärt, was sich auf die Konfiguration bezieht, aber es ist eine riesige Menge an neuen Informationen, von denen wir im Moment nicht alle benötigen werden. Stattdessen zeigen wir dir, wie du die Informationen bekommst, die du tatsächlich suchst.

ASF configuration can be done in at least three ways - through our web config generator, ASF-ui or manually. Dies wird ausführlich auf der Seite **[Installation](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-de-DE)** erläutert, also beziehe dich darauf, wenn du detailliertere Informationen benötigst. We'll use web config generator as a starting point.

Besuche unsere **[Web-Konfigurationsgenerator](https://justarchinet.github.io/ASF-WebConfigGenerator)** Seite mit deinem Lieblingsbrowser (du musst JavaScript aktiviert haben, falls du es manuell deaktiviert hast). Wir empfehlen Chrome oder Firefox, aber es sollte mit allen gängigen Browsern funktionieren.

Nach dem Öffnen der Seite klicke auf "Bot". Du solltest nun eine ähnliche Seite wie die unten sehen:

![Bot tab](https://i.imgur.com/aF3k8Rg.png)

Wenn die Version von ASF, die du gerade heruntergeladen hast, älter ist als der Konfigurationsgenerator, der standardmäßig verwendet wird, wähle einfach Ihre ASF-Version aus dem Dropdown-Menü. Dies kann passieren, da der Konfigurationsgenerator zum Erzeugen von Konfigurationen für neuere (Vorabversionen) ASF-Versionen verwendet werden kann, die noch nicht als stabil markiert wurden. Du hast die neueste stabile Version von ASF heruntergeladen, die auf ihre zuverlässige Funktion überprüft wurde.

Beginne damit, den Namen Ihres Bot in das rot markierte Feld zu schreiben. Dies kann jeder beliebige Name sein, den du verwenden möchtest, wie z. B. dein Spitzname, Kontoname, eine Nummer oder etwas anderes. Es gibt nur ein Wort, das du nicht verwenden kannst, nämlich `ASF`, da dieses Schlüsselwort für die globale Konfigurationsdatei reserviert ist. Außerdem kann dein Bot-Name nicht mit einem Punkt beginnen (ASF ignoriert diese Dateien absichtlich). Wir empfehlen Ihrenauch keine Leerzeichen zu verwenden. Du solltest bei Bedarf `_` als Trennzeichen verwenden.

Nachdem du dich für einen Namen entschieden hast, aktiviere den `Enabled` Schalter. Hiermit wird festgelegt, ob dein Bot von ASF automatisch nach dem Start (des Programms) gestartet werden soll.

Jetzt kannst du dich für zwei Dinge entscheiden:
- Sie können Ihren Login in das Feld `SteamLogin` und dein Passwort in das `SteamPassword` Feld eintragen
- Oder du kannst sie leer lassen

Wenn Sie das erste nutzen, kann ASF die Konto-Anmeldeinformationen während des Startvorgangs automatisch verwenden, sodass diese nicht jedes Mal manuell eingegeben werden müssen, wenn ASF sie benötigt. Sie können dies jedoch auch weglassen. In diesem Fall werden sie nicht gespeichert, sodass ASF nicht ohne Hilfe automatisch starten kann und Sie diese während der Laufzeit eingeben müssen.

ASF benötigt Ihre Anmeldeinformationen, da es seine eigene Implementierung des Steam-Clients beinhaltet und die gleichen Details benötigt, die du selbst benutzt um dich anzumelden. Die Anmeldeinformationen werden nirgends außer auf dem PC im `config` ASF-Verzeichnis gespeichert. Unser Web-Konfigurationsgenerator ist client-basiert, was bedeutet, dass der Programmcode lokal im Browser ausgeführt wird, um gültige ASF-Konfigurationen zu generieren, ohne dass Sie Details eingeben, die Ihren PC überhaupt erst verlassen, sodass Sie sich keine Sorgen über einen möglichen Verlust vertraulicher Daten machen müssen. Dennoch, wenn du aus irgenIhrem Grund Ihre Zugangsdaten dort nicht eingeben möchtest, verstehen wir das, und du kannst sie später manuell in generierte Dateien einfügen, oder sie ganz weglassen und sie nur in die ASF-Befehlszeile eingeben. Mehr zu Sicherheitsfragen findest du im Abschnitt **[Konfiguration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-de-DE)**.

Sie können auch nur ein Feld leer lassen, wie z. B. `SteamPassword`. ASF kann dann dein Login automatisch verwenden, fragt aber trotzdem nach dem Passwort (ähnlich wie beim Steam Client). Wenn du die Steam-Familienansicht benutzt um das Konto freizuschalten musst du es in das Feld `SteamParentalCode` eingeben.

Nach der Entscheidung und den optionalen Details wird Ihre Webseite nun ähnlich wie die untenstehende aussehen:

![Bot tab 2](https://i.imgur.com/yf54Ouc.png)

Sie können jetzt auf den "Download"-Button klicken und unser Web-Konfigurationsgenerator erzeugt eine neue `json` Datei basierend auf Ihrem gewählten Namen. Speichern Sie diese Datei im `config` Verzeichnis, welches sich in dem Ordner befindet, in dem Sie unsere Zip-Datei aus dem vorherigen Schritt extrahiert haben.

Dein `config` Verzeichnis sieht nun wie folgt aus:

![Structure 2](https://i.imgur.com/crWdjcp.png)

Glückwunsch! Du hast gerade die sehr einfache ASF-Bot-Konfiguration abgeschlossen. Wir werden dies in Kürze erweitern, denn jetzt ist dies alles, was du brauchst.

---

### Ausführen von ASF

Du bist nun bereit, das Programm zum ersten Mal zu starten. Führen Sie einfach einen Doppelklick auf die `ArchiSteamFarm` Binärdatei im ASF-Verzeichnis aus.

Nach diesem Schritt, vorausgesetzt, du hast alle erforderlichen Abhängigkeiten im ersten Schritt installiert, sollte ASF richtig starten, Ihren ersten Bot bemerken (wenn du nicht vergessen hast, die generierte Konfiguration in das Verzeichnis `config` zu legen) und versuchen, dich anzumelden:

![ASF](https://i.imgur.com/u5hrSFz.png)

Wenn du `SteamLogin` und `SteamPassword` für ASF angegeben hast, wirst du nur nach Ihrem SteamGuard-Code gefragt (E-Mail, 2FA oder keine, abhängig von Ihren Steam-Einstellungen). Wenn du es nicht getan hast, wirst du auch nach Ihrem Steam-Login und Passwort gefragt.

Jetzt ist ein guter Zeitpunkt, um unseren Abschnitt zur **[Datenschutzerklärung](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Statistics-de-DE#aktuelle-datenschutzerkl%C3%A4rung)** zu lesen, wenn du dir Sorgen darüber machst, was als nächstes passieren wird, wie von ASF selbst angegeben.

Nachdem du das anfängliche Anmeldeportal passiert hast, (davon ausgegangen, dass Ihre Daten korrekt sind) wirst du erfolgreich angemeldet und ASF beginnt mit den Standardeinstellungen, die du bisher nicht geändert hast, zu sammeln:

![ASF 2](https://i.imgur.com/Cb7DBl4.png)

Dies beweist, dass ASF nun erfolgreich seine Arbeit auf dem Konto erledigt, sodass Sie nun das Programm minimieren und etwas anderes erldeigen können. Nach einiger Zeit (je nach **[Performance](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance-de-DE)**) wirst du sehen, wie Steam-Karten langsam gesammelt werden. Natürlich musst du dafür zulässige Spiele zum Sammeln haben, die auf Ihrer **[Abzeichen-Seite](https://steamcommunity.com/my/badges)** folgendes zeigen: "Sie können noch X weitere Kartendrops vom Spielen dieses Spiels bekommen". Wenn es keine Spiele zum Sammeln gibt, dann wird ASF feststellen, dass es nichts zu tun gibt, wie in unserem **[FAQ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ#what-is-asf)** beschrieben.

Dies schließt unseren sehr einfachen Einrichtungsleitfaden ab. Sie können nun entscheiden, ob du ASF weiter konfigurieren möchtest oder ob du es in den Standardeinstellungen seine Arbeit tun lassen möchtest. Wir werden noch ein paar grundlegende Details besprechen und Ihrendann das gesamte Wiki zur Verfügung stellen.

---

### Erweiterte Konfiguration

#### Sammeln auf mehreren Konten gleichzeitig

ASF unterstützt das Karten-Sammeln von mehr als einem Konto auf einmal, was seine Hauptfunktion ist. Sie können weitere Konten zu ASF hinzufügen, indem du mehr Bot-Konfigurationsdateien erzeugst, genau so wie du Ihre erste vor wenigen Minuten generiert hast. Du musst nur zwei Dinge sicherstellen:

- Eindeutiger Bot-Name, wenn du bereits Ihren ersten Bot "HauptKonto" genannt hast, kannst du keinen weiteren so nennen.
- Gültige Anmeldedaten, wie z. B. `SteamLogin`, `SteamPassword` und `SteamParentalCode` (bei Verwendung der Steam Parental Einstellungen)

Mit anderen Worten, gehe einfach erneut zur Konfiguration und mache genau das Gleiche, nur für dein zweites oder drittes Konto. Vergiss nicht für alle Ihre Bots eindeutige Namen zu verwenden.

---

#### Einstellungen ändern

Sie können bestehende Einstellungen auf die gleiche Weise ändern, indem du eine neue Konfigurationsdatei erzeugst. Wenn du unseren Web-Konfigurationsgenerator noch nicht geschlossen hast, klicke auf "Zu den erweiterten Einstellungen umschalten" und sieh Ihrenan, was du entdecken kannst. Für diese Anleitung werden wir die Einstellung `CustomGamePlayedWhileFarming` ändern, mit der du einstellen kannst, dass der benutzerdefinierte Name angezeigt wird, wenn ASF am Sammeln ist, anstatt das aktuelle Spiel anzuzeigen.

Also lass uns das tun, wenn du ASF ausführst und das Sammeln beginnst, wirst du in den Standardeinstellungen sehen, dass dein Steam-Konto jetzt im Spiel ist:

![Steam](https://i.imgur.com/1VCDrGC.png)

Lass uns das jetzt ändern. Aktiviere die erweiterten Einstellungen im Webkonfigurationsgenerator und suche `CustomGamePlayedWhileFarming`. Sobald du das getan hast, füge dort Ihren eigenen benutzerdefinierten Text ein, den du anzeigen möchtest, wie zum Beispiel "Idling cards":

![Bot tab 3](https://i.imgur.com/gHqdEqb.png)

Nun kannst du die neue Konfigurationsdatei auf genau die gleiche Weise herunterladen, dann **überschreibe** deine alte Konfigurationsdatei mit der neuen. Sie können natürlich auch Ihre alte Konfigurationsdatei löschen und an ihre Stelle die neue einfügen.

Sobald du das getan hast und ASF erneut startest, wirst du feststellen, dass ASF nun Ihren benutzerdefinierten Text an der vorherigen Stelle anzeigt:

![Steam 2](https://i.imgur.com/vZg0G8P.png)

This confirms that you've successfully edited your config. In exactly the same way you can change global ASF properties, by switching from bot tab to "ASF" tab, downloading generated `ASF.json` config file and putting it in your `config` directory.

Editing your ASF configs can be done much easier by using our ASF-ui frontend, which will be explained further below.

---

#### Verwendung von ASF-ui

ASF ist eine Konsolenanwendung und beinhaltet keine grafische Benutzeroberfläche. Wir arbeiten jedoch aktiv an unserem IPC-Schnittstellen Frontend **[ASF-ui](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#asf-ui)**, was eine sehr gute und benutzerfreundliche Möglichkeit sein kann, auf verschiedene ASF-Funktionen zuzugreifen.

In order to use ASF-ui, you need to have `IPC` enabled, which is the default option starting with ASF V5.1.0.0. Once you launch ASF, you should be able to confirm that it properly started the IPC interface automatically:

![IPC](https://i.imgur.com/ZmkO8pk.png)

You can access ASF's IPC interface under **[this](http://localhost:1242)** link, as long as ASF is running, from the same machine. You can use ASF-ui for various purposes, e.g. editing the config files in-place or sending **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. Wirf einfach einen Blick darauf, um alle Funktionalitäten von ASF-ui kennenzulernen.

![ASF-ui](https://raw.githubusercontent.com/JustArchiNET/ASF-ui/main/.github/previews/bots.png)

Please note that some features, such as sending commands, require a properly set `SteamOwnerID` global config property. Now that you have ASF-ui up and running, why not give it a try and set it from the frontend itself? You'll need to input unique Steam identificator in 64-bit form of your account. You can look it up in various different ways, for example through **[STEAMID I/O](https://steamid.io)** or **[SteamRep](https://steamrep.com)**. The number you're looking for should be similar to `76561198006963719`, which is my account's ID.

---

### Zusammenfassung

Du hast ASF bereits erfolgreich so eingestellt, dass es deine Steam-Konten nutzt, und du hast es bereits ein wenig nach deinen Wünschen angepasst. If you followed our entire guide, then you also managed to tweak ASF through our ASF-ui interface and found out that ASF actually has a GUI of some sort. Now is a good time to read our entire **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** section in order to learn what all those different settings you've seen actually do, and what ASF has to offer. If you've stumbled upon some issue or you have some generic question, read our **[FAQ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ)** instead which should cover all, or at least a vast majority of questions that you may have. Wenn du alles über ASF erfahren möchtest und wie es dein Leben einfacher machen kann, dann schaue dir den Rest von **[unserem Wiki](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Home-de-DE)** an. If you found out our program to be useful for you and you're feeling generous, you can also consider donating to our project. In any case, have fun!

---

## Generisches Setup

Dieses Setup ist für fortgeschrittene Benutzer, die ASF so einrichten möchten, dass es in der Variante **[generic](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#generic)** läuft. Es wird nicht für Personen empfohlen, die das **[betriebssystemspezifische Setup](#betriebssystemspezifisches-setup)** verwenden können.

You want to use `generic` variant mainly in those situations (but of course you can use it regardless):
- Wenn du ein Betriebssystem verwendest, für das wir kein betriebssystemspezifisches Paket erstellen (z. B. 32-Bit Windows)
- Wenn du bereits die .NET Core Runtime/SDK hast, oder eine installieren und verwenden möchtest
- Wenn du die ASF-Strukturgröße minimieren willst, indem du die Laufzeitanforderungen selbst handhabst
- When you want to use a custom **[plugin](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins)** which requires a `generic` setup of ASF to run properly (due to missing native dependencies)

Bedenken Sie jedoch, dass Sie in diesem Fall für die .NET Core Runtime verantwortlich sind. Das bedeutet, dass, wenn Sie Ihre .NET Core SDK (Runtime) nicht verfügbar, veraltet oder defekt ist, ASF nicht funktioniert. Aus diesem Grund empfehlen wir dieses Setup nicht für Gelegenheitsnutzer, da Sie nun sicherstellen müssen, dass Ihr .NET Core SDK (Runtime) den ASF-Anforderungen entspricht und ASF ausführen kann, anstatt dass **wir** sicherstellen, dass unsere mit ASF gebündelte .NET Core Runtime dies ermöglicht.

For `generic` package, you can follow entire OS-specific guide above, with two small changes. Zusätzlich zur Installation der .NET Core Prerequisites möchten Sie auch das .NET Core SDK installieren, und anstatt eine betriebssystemspezifische `ArchiSteamFarm(.exe)` ausführbare Datei zu haben, nutzen Sie nun eine generische `ArchiSteamFarm.dll` Binärdatei. Alles andere ist identisch.

Mit zusätzlichen Schritten:
- **[.NET Core Prerequisites](https://docs.microsoft.com/dotnet/core/install/dependencies?tabs=netcore31)** installieren.
- Installiere **[.NET Core SDK](https://www.microsoft.com/net/download)** (oder zumindest die Runtime), die für dein Betriebssystem geeignet ist. Du möchtest höchstwahrscheinlich ein Installationsprogramm verwenden. Ließ die **[Runtime-Anforderungen](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)**, wenn du nicht sicher bist, welche Version du installieren sollst.
- Download **[latest ASF release](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** in `generic` variant.
- Entpack das Archiv an einen neuen Ort (und `chmod +x ArchiSteamFarm.sh` wenn du unter Linux/OS X bist).
- **[Configure ASF](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)**.
- Starte ASF entweder mit einem Hilfs-Skript oder führe `dotnet /pfad/zu/ArchiSteamFarm.dll` aus Ihrer Lieblings-Shell heraus manuell aus.

Helper scripts (such as `ArchiSteamFarm.cmd` for Windows and `ArchiSteamFarm.sh` for Linux/OS X) are located next to `ArchiSteamFarm.dll` binary - those are included in `generic` variant only. Sie können diese verwenden, wenn Sie den Befehl `dotnet` nicht manuell ausführen möchten. Offensichtlich funktionieren Hilfs-Skripte nicht, wenn Sie .NET Core SDK nicht installieren und Sie keine `dotnet` ausführbare Datei in Ihrem `PATH` haben. Hilfs-Skripte sind völlig optional verwendbar, Sie können jederzeit `dotnet /pfad/zu/ArchiSteamFarm.dll` manuell eingeben.