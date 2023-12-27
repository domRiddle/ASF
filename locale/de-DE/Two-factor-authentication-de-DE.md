# Zwei-Faktor-Authentifizierung (2FA)

Steam beinhaltet das Zwei-Faktor-Authentifizierungssystem, bekannt als „Escrow“, das zusätzliche Details für verschiedene kontenbezogene Aktivitäten erfordert. Sie können **[hier](https://help.steampowered.com/faqs/view/2E6E-A02C-5581-8904)** und **[hier](https://help.steampowered.com/faqs/view/34A1-EA3F-83ED-54AB)** mehr darüber lesen. Diese Seite befasst sich mit dem 2FA-System sowie unserer Lösung, die mit ihm integriert wird (ASF-2FA).

---

# ASF-Logik

Unabhängig davon, ob Sie ASF-2FA verwenden oder nicht, enthält ASF die passende Logik und ist sich bewusst, dass die Konten durch das Standard 2FA geschützt sind. Es fragt Sie nach den erforderlichen Details, sobald diese benötigt werden (z. B. beim Anmelden). Es ist jedoch möglich, diese Anfragen durch ASF-2FA zu automatisieren, wodurch automatisch Token generiert, Ihnen lästige Aufgaben abgenommen und zusätzliche Funktionen hinzugefügt werden (Erläuterungen siehe unten).

---

# ASF-2FA

ASF-2FA ist ein eingebautes Modul, das für die Bereitstellung von 2FA-Funktionen für den ASF-Prozess verantwortlich ist, wie das Erzeugen von Codes und das Annehmen von Bestätigungen. Es funktioniert durch das Duplizieren Ihres bestehenden Authentifikators, sodass Sie Ihren aktuellen Steam Guard und ASF-2FA gleichzeitig verwenden können.

Sie können überprüfen, ob Ihr Bot-Konto bereits ASF-2FA verwendet, indem Sie den `2fa` **[Befehl](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE)** ausführen. Wenn Sie Ihren Authentifikator nicht bereits als ASF-2FA importiert haben, sind alle standardmäßigen `2fa`-Befehle nicht funktionsfähig, was bedeutet, dass Ihr Konto ASF-2FA nicht verwendet und es deshalb nicht für erweiterte ASF-Funktionen verfügbar ist, die den Betrieb des Moduls erfordern.

---

# Empfehlungen

Wir bieten Ihnen eine Vielzahl an Möglichkeiten, ASF 2FA operativ zu behandeln. Unsere Empfehlungen basieren auf Ihrer aktuellen Situation:

- Falls Sie bereits SteamDesktopAuthenticator, WinAuth oder eine andere Drittanbieter-App verwenden, mit der Sie 2FA-Details problemlos extrahieren können, nur **[importieren Sie](#import)** diese zu ASF.
- Wenn Sie die offizielle App verwenden und Sie nichts dagegen haben, Ihre 2FA-Anmeldeinformationen zurückzusetzen; dann ist es der beste Weg, 2FA zu deaktivieren, dann **[](#erstellung)** neue 2FA-Anmeldedaten mit **[gemeinsamer Authentifikator](#gemeinsamer-authentifikator)**, mit der Sie die offizielle App und ASF 2FA verwenden können. Diese Methode erfordert kein root oder fortgeschrittenes Wissen, kaum den Anweisungen.
- Wenn Sie die offizielle App verwenden und Ihre 2FA-Anmeldeinformationen nicht neu erstellen möchten, sind Ihre Optionen sehr begrenzt, typischerweise benötigen Sie root und zusätzliches Experimentieren, um **[zu importieren](#importieren)** diese Details, und sogar mit diesem könnte es unmöglich sein.
- Wenn Sie noch keine 2FA verwenden und es Ihnen egal ist, kommen als ASF-2FA die Nutzung des **[eigenständigen Authentifikators](#eigenst%C3%A4ndiger-authentifikator)**, die **[Duplizierung](#importieren)** der Drittanbieter-App nach ASF (Empfehlung #1), oder der **[gemeinsamer Authentifikator](#gemeinsamer-authentifikator)** mit offizieller App (Empfehlung #2) für Sie infrage.

Im Folgenden diskutieren wir alle möglichen Optionen und bekannten Methoden.

---

## Erstellung

Im Allgemeinen empfehlen wir dringend **[Ihren bestehenden Authentifikator zu duplizieren](#import)**, da dies der Hauptzweck der ASF-2FA ist. ASF verfügt jedoch über eine offizielle `MobileAuthenticator`-**[Erweiterung](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins)**, welche ASF-2FA dahin gehend ergänzt, sodass Sie auch einen komplett neuen Authentifikator verknüpfen können. Dies kann hilfreich sein, wenn Sie nicht bereit oder in der Lage sind, andere Tools zu verwenden und es keine Einwände gibt, dass ASF-2FA Ihr (möglicherweise einziger) Hauptauthentifikator sein wird.

Es gibt zwei mögliche Szenarien für das Hinzufügen eines Zwei-Faktor-Authentifikators mit dem `MobileAuthenticator`-**[Erweiterung](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins)**: Alleinstehend oder mit der offiziellen Steam Mobile App verbunden. Im zweiten Szenario werden Sie identischen Authentifikator sowohl für mit ASF als auch in der mobilen App teilen; beide generieren dieselben Codes, und beide werden imstande sein, Handelsangebote, Steam „Communitymarkt“-Transaktionen usw. zu bestätigen.

### Gemeinsame Schritte für beide Szenarien

Egal, ob Sie vorhaben ASF als alleinstehenden Authentifikator verwenden oder die gleiche 2FA in der offiziellen Steam App zu nutzen, – Sie müssen diese Initialisierungsschritte machen:

1. Erstellen Sie einen ASF-Bot für das Zielkonto, starten Sie ihn und melden Sie sich an (was Sie wahrscheinlich bereits getan haben).
2. Optional weisen Sie dem Konto **[hier](https://store.steampowered.com/phone/manage)** eine funktionierende und operative Telefonnummer zu, die vom Bot verwendet werden soll. Dies erlaubt Ihnen den Empfang von SMS-Code, sowie bei Bedarf die Wiederherstellung, aber es ist erforderlich.
3. Stellen Sie sicher, dass Sie 2FA bisher nicht für Ihr Konto verwenden; deaktivieren Sie es zuerst, falls doch.
4. Führen Sie den Befehl `2fainit [Bot]` aus, indem Sie `[Bot]` durch den Namen Ihres Bots ersetzen.

Angenommenen, Sie haben eine erfolgreiche Antwort erhalten; dann sind die folgenden zwei Dinge passiert:

- Eine neue Datei `<Bot>.maFile.PENDING` wurde von ASF in Ihrem `Konfigurations`-Verzeichnis generiert.
- SMS wurde von Steam an die Telefonnummer gesendet, die Sie für das oben genannte Konto zugewiesen haben. Wenn Sie keine Telefonnummer angegeben haben, wurde stattdessen eine E-Mail an die E-Mail-Adresse des Kontos gesendet.

Die Authentifizierungsdetails sind noch nicht funktionsfähig, aber Sie können die generierte Datei überprüfen, wenn Sie es wünschen. Für den Fall, dass Sie doppelte Sicherheit wünschen, können Sie den Widerrufscode bereits notieren. Die nächsten Schritte hängen von Ihrem gewählten Szenario ab.

### Eigenständiger Authentifikator

Wenn Sie ASF als (möglicherweise einzigen) Hauptauthentifikator verwenden möchten, müssen Sie jetzt den Finalisierungsschritt machen:

5. Führt den Befehl `2fafinalize [Bot] <ActivationCode>` aus; ersetzt `[Bot]` mit dem Namen Ihres Bots und `<ActivationCode>` mit dem Code, den Sie im vorherigen Schritt per SMS oder E-Mail erhalten haben.

### Gemeinsamer Authentifikator

Wenn Sie den identischen Authentifikator sowohl in der ASF, als auch in der offiziellen Steam Mobile App haben möchten, dann sind jetzt die nächenen Schritte erforderlich:

5. Ignoriere die SMS oder den E-Mail-Code, den Sie nach dem vorherigen Schritt erhalten haben.
6. Installieren Sie die mobilen Steam App, falls sie bisher nicht installiert ist, und öffnen Sie diese. Navigieren Sie zur Registerkarte „Steam Guard“ und fügen Sie einen neuen Authentifikator hinzu, indem Sie den Anweisungen der App folgen.
7. Nachdem Ihr Authentifikator in der mobilen App hinzugefügt wurde und funktioniert, kehren Sie zu ASF zurück. Sie müssen jetzt ASF mitteilen, dass die Fertigstellung mithilfe eines der folgenden Befehle erfolgt:
 - Warten Sie, bis der nächste 2FA-Code in der Steam Mobile App angezeigt wird, und verwenden Sie den Befehl `2fafinalized [Bot] <2fa_code_from_app>` (`[Bot]` mit dem Namen Ihres Bots ersetzen) und `<2fa_code_from_app>` mit dem Code, den Sie derzeit in der Steam Mobile App sehen. Falls der von ASF generierte Code und der von Ihnen angegebene Code derselbe sind, ASF geht davon aus, dass ein Authentifikator korrekt hinzugefügt wurde und setzt den Import Ihres neu erstellten Authentifikators fort.
 - Wir empfehlen dringend, dies zu tun, um sicherzustellen, dass Ihre Anmeldedaten gültig sind. Wenn Sie jedoch nicht überprüfen möchten (oder können), ob Codes gleich sind und Sie wissen, was Sie tun; dann können Sie stattdessen den Befehl `2fafinalizedforce [Bot]` verwenden (`[Bot]` mit dem Namen Ihres Bots ersetzen). ASF geht davon aus, dass der Authentifikator korrekt eingefügt und Sie nun mit dem Import Ihres neuen Authentifikators beginnen werden.

### Nach Abschluss

Angenommen, alles funktionierte richtig, wurde die zuvor generierte Datei `<Bot>.maFile.PENDING` in `<Bot>.maFile.NEW` umbenannt. Dies belegt, dass Ihre 2FA-Anmeldedaten nun gültig und aktiv sind. Wir empfehlen Ihnen, eine Kopie dieser Datei zu erstellen und sie in **an einem geschützten und sicheren Ort** aufzubewahren. Darüber hinaus empfehlen wir Ihnen, die Datei in Ihrem bevorzugten Editor zu öffnen und den `revokation_code` aufzuschreiben. Das erlaubt es Ihnen erlauben, den Authentifikator zu widerrufen, falls Sie (den Zugriff auf) ihn verlieren.

Im Hinblick auf technische Details umfasst die generierte `maFile` sämtliche Informationen, die wir vom Steam-Server erhalten haben, sowie das Feld `device_id`, welches für andere Authenticatoren von Bedeutung ist. Die Datei ist vollständig kompatibel mit **[SDA](#steamdesktopauthenticator)** für den Import.

ASF importiert automatisch Ihren Authentifikator. Sobald die Prozedur abgeschlossen ist, sollten `2fa` und andere verwandte Befehle nun für das Bot-Konto funktionieren, mit dem Sie den Authentifikator verbunden haben.

---

## Importieren

Der Import erfordert bereits einen verknüpften und funktionsfähigen Authentifikator, der von ASF unterstüzt wird. ASF unterstützt derzeit ein paar verschiedene offizielle und inoffiziell 2FA-Quellen – Android, iOS, SteamDesktopAuthenticator und WinAuth, zusätzlich zur manuellen Methode, die es Ihnen erlaubt, die benötigten Anmeldedaten selbst zur Verfügung zu stellen. Sofern Sie noch keinen Authentifikator haben, müssen Sie einen davon auswählen und ihn zuerst einrichten. Wenn Sie nicht genau wissen welchen Sie auswählen sollten, empfehlen wir WinAuth, aber alle der oben genannten Möglichkeiten funktionieren gut, wenn Sie die Anweisungen befolgen.

Alle folgenden Anleitungen erfordern von Ihnen, dass Sie bereits einen **funktionierenden und betriebsbereiten** Authentifikator haben, der mit dem gegebenen Programm/Anwendung verwendet wird. ASF-2FA funktioniert nicht ordnungsgemäß, wenn Sie ungültige Daten importieren. Stelle daher sicher, dass Ihr Authentifikator ordnungsgemäß funktioniert bevor Sie versuchen ihn zu importieren. Dies beinhaltet das Testen und Überprüfen, ob die folgenden Authentifikator-Funktionen ordnungsgemäß funktionieren:
- Sie können Codes generieren und diese Codes werden vom Steam-Netzwerk akzeptiert
- Sie können Bestätigungen abrufen und sie kommen auf Ihrem mobilen Authentifikator an
- Sie können diese Bestätigungen akzeptieren und sie werden vom Steam-Netzwerk ordnungsgemäß als bestätigt/abgelehnt erkannt

Vergewissere Sie sich, dass Ihr Authentifikator funktioniert, indem Sie überprüfst, ob die obigen Aktionen funktionieren – wenn nicht, dann werden sie auch nicht in ASF funktionieren und Sie verschwenden nur Zeit und erzeugen Ihren selbst zusätzliche Probleme.

---

### Android-Handy

**Die folgenden Anweisungen gelten für die Steam-App in Version `2.X`, es gibt derzeit begrenzte **[Ressourcen](https://github.com/JustArchiNET/ArchiSteamFarm/discussions/2786)** beim Extrahieren der erforderlichen Details aus Version `3.0` und darüber. Wir werden diesen Abschnitt aktualisieren, sobald eine allgemein verfügbare Methode gefunden wurde. Derzeit besteht die Übergangslösung darin, vorsätzlich die ältere Version der Steam App zu installieren, 2FA zu registrieren und die erforderlichen Details zuerst zu extrahieren. Danach ist es möglich, die Anwendung auf die neueste Version zu aktualisieren – der existierender Authentifikator wird weiterhin funktionieren.**

Im Allgemeinen benötigen Sie für den Import des Authentifikators von Ihrem Android-Handy **[root](https://de.wikipedia.org/wiki/Rooting_(Android_OS))**-Zugriff. Das Rooting variiert von Gerät zu Gerät, daher werde ich Ihnen nicht sagen wie Sie Ihr Gerät rooten können. Auf der Webseite **[XDA](https://www.xda-developers.com/root)** finden Sie exzellente Anleitungen, wie Sie das machen können, sowie allgemeine Informationen zum Rooten im Allgemeinen. Wenn Sie Ihr Gerät oder die Anleitung dazu nicht finden können, versuche es bei Google zu finden.

Zumindest ist es offiziell nicht möglich, auf geschützte Steam-Dateien ohne Root zuzugreifen. Die einzige offizielle nicht-root Methode zum Extrahieren von Steam-Dateien ist das Erstellen von unverschlüsselten `/data` Backups, auf die eine oder andere Weise und das manuelle Abrufen entsprechender Dateien auf dem PC. Aber da so etwas stark von Ihrem Handyhersteller abhängt und **nicht** in Android-Standard ist, werden wir es hier nicht weiter erörtern. Wenn Sie das Glück haben eine solche Funktionalität zu haben, können Sie sie nutzen, aber die Mehrheit der Benutzer hat so etwas nicht.

Inoffiziell ist es möglich, die benötigten Dateien ohne Root-Zugriff zu extrahieren, indem man die Steam-App mit der Version `2.1` (oder früher) installiert oder heruntergestuft, einen mobilen Authentifikator installieren und dann einen Snapshot der App (zusammen mit den von uns benötigten Dateien `data`) über `adb backup` erstellt. Da es sich jedoch um eine schwerwiegende Sicherheitsverletzung und eine gänzlich nicht unterstützte Methode zum Extrahieren der Dateien handelt, werden wir nicht weiter darauf eingehen. Valve hat diese Sicherheitslücke in neueren Versionen aus einem bestimmten Grund deaktiviert und wir erwähnen sie nur als Möglichkeit. Trotzdem ist es möglich, eine saubere Installation dieser Version durchzuführen, einen neuen Authentifikator zu verknüpfen, die erforderlichen Dateien zu extrahieren und dann aktualisieren Sie die App. Dies sollte ausreichen, aber Sie sind mit dieser Methode auf sich allein gestellt.

Angenommen, Sie haben Ihr Handy erfolgreich gerootet, sollten Sie danach einen beliebigen auf dem Markt erhältlichen Root-Explorer herunterladen, z. B. **[dieser ](https://play.google.com/store/apps/details?id=com.jrummy.root.browserfree)** (oder einen anderen Ihrer Wahl). Sie können auch über ADB (Android Debug Bridge) oder jede andere Ihren zur Verfügung stehende Methode auf die geschützten Dateien zugreifen, wir machen es über den Explorer, da es definitiv der benutzerfreundlichste Weg ist.

Nachdem Sie Ihren Root-Explorer geöffnet haben, navigiere zum Ordner `/data/data`. Beachte, dass das Verzeichnis `/data/data` geschützt ist und Sie ohne Root-Zugriff nicht darauf zugreifen können. Dort angekommen, finden Sie den Ordner `com.valvesoftware.android.steam.community` und kopieren ihn auf Ihre `/sdcard`, die auf Ihren internen Speicher verweist. Danach sollten Sie Ihr Handy an Ihren PC anschließen und den Ordner wie gewohnt von Ihrem internen Speicher kopieren können. Wenn der Ordner zufällig nicht sichtbar ist, obwohl Sie Ihren sicher sind, dass Sie ihn an die richtige Stelle kopiert haben, versuche zuerst Ihr Handy neu zu starten.

Nun können Sie wählen, ob Sie Ihren Authentifikator zuerst in WinAuth und dann in ASF oder sofort in ASF importieren möchten. Die erste Option ist benutzerfreundlicher und ermöglicht es Ihnen Ihren Authentifikator auch auf Ihrem PC zu duplizieren, sodass Sie Bestätigungen vornehmen und Codes an 3 verschiedenen Stellen generieren können – Ihrem Handy, Ihrem PC und ASF. Wenn Sie das tun möchten, öffne einfach WinAuth, füge einen neuen Steam-Authentifikator hinzu und wähle die Option „Import aus Android“. Anschließend folge den Anweisungen, indem Sie auf die Dateien zugreifst, die Sie oben erhalten haben. Danach können Sie diesen Authentifikator von WinAuth nach ASF importieren, was im speziellen WinAuth-Abschnitt weiter unten erläutert wird.

Wenn Sie sich nicht mit WinAuth befassen möchten, dann kopieren Sie einfach die Datei `files/Steamguard-<SteamID>` aus unserem geschützten Verzeichnis; wobei `SteamID` Ihre 64-Bit Steam-Identifikation des Kontos ist, das Sie hinzufügen möchten (falls mehr als eine, denn wenn Sie nur ein Konto haben, dann ist dies die einzige Datei). Sie musst diese Datei in das ASF-Verzeichnis `config` kopieren. Sobald Sie das getan haben, benenne die Datei um in `BotName.maFile`, wobei `BotName` der Name Ihres Bot ist, dem Sie ASF-2FA hinzufügen. Nach diesem Schritt starte ASF – es sollte die `.maFile` erkennen und importieren.

```text
[*] INFO: ImportAuthenticator() <1> Konvertiere .maFile in ASF-Format...
[*] INFO: ImportAuthenticator() <1> Import vom mobilen Authentifikator erfolgreich abgeschlossen!
```

Vorrausgesezt Sie haben die richtige Datei mit gültigen Geheimnissen importiert, sollte alles, was Sie mit den `2fa`-Befehlen überprüfen können, einwandfrei funktionieren. Falls Sie einen Fehler gemacht haben, können Sie `Bot.db` jederzeit entfernen und bei Bedarf neu beginnen.

---

### iOS

Für iOS können Sie **[ios-steamguard-extractor](https://github.com/CaitSith2/ios-steamguard-extractor)** verwenden. Dies ist möglich, da Sie entschlüsselte Backups erstellen, auf Ihrem PC installieren und das Programm verwenden können, um Steam-Daten zu extrahieren, die sonst unmöglich zu bekommen sind (zumindest ohne Jailbreak, aufgrund der iOS-Verschlüsselung).

Gehe zu der **[neuesten Version](https://github.com/CaitSith2/ios-steamguard-extractor/releases/laten)** um das Programm herunterzuladen. Sobald Sie die Daten extrahiert haben, können Sie sie z. B. in WinAuth, dann von WinAuth nach ASF kopieren (obwohl Sie auch einfach generiertes json kopieren können, beginnend mit `{` endend mit `}` in `BotName.maFile` und wie üblich vorgehen). Wenn Sie mich fragen, empfehle ich Ihnen dringend zuerst nach WinAuth zu importieren und dann sicherzustellen, dass sowohl das Erzeugen von Codes, als auch das Akzeptieren von Bestätigungen richtig funktioniert, damit Sie sicher sein können, dass alles in Ordnung ist. Wenn Ihre Anmeldeinformationen ungültig sind wird ASF-2FA nicht ordnungsgemäß funktionieren, daher ist es viel besser den ASF-Importschritt als letzten Schritt durchzuführen.

Für Fragen/Probleme besuche bitte **[issues](https://github.com/CaitSith2/ios-steamguard-extractor/issues)**.

*Beachte, dass das obige Programm inoffiziell ist – Sie benutzt es auf eigene Gefahr. Wir bieten keine technische Unterstützung wenn es nicht ordnungsgemäß funktioniert – wir haben ein paar Hinweise erhalten, dass es sich um den Export ungültiger 2FA-Anmeldeinformationen handelt – überprüfe, ob Bestätigungen in einem Authentifikator wie WinAuth funktionieren, bevor Sie diese Daten in ASF importieren!*

---

### SteamDesktopAuthenticator (SDA)

Wenn Sie Ihren Authentifikator bereits in SDA laufen haben, sollten Sie beachten, dass es eine `steamID.maFile` Datei im Ordner `maFiles` gibt. Vergewissern Sie sich, dass `maFile` in unverschlüsselter Form vorliegt, da ASF diese SDA-Dateien nicht entschlüsseln kann – unverschlüsselte Dateiinhalte sollten mit `{` beginnen und mit dem Zeichen `}` enden. Falls notwendig, können Sie die Verschlüsselung zuerst aus den SDA-Einstellungen entfernen und sie wieder aktivieren, sobald Sie fertig sind. Sobald die Datei in unverschlüsselter Form vorliegt, kopieren Sie sie in das ASF-Verzeichnis `config`.

Sie sollten nun `steamID.maFile` in `BotName.maFile` im ASF-Konfigurationsverzeichnis umbenennen, wobei `BotName` der Name Ihres Bots ist, dem Sie ASF-2FA hinzufügen. Alternativ können Sie es so lassen wie es ist, ASF wird es dann nach dem Einloggen automatisch auswählen. Die Datei umzubenennen, erlaubt ASF die Verwendung der ASF-2FA vor dem Einloggen. Falls Sie dies unterlassen, dann kann die Datei erst ausgewählt werden, sobald ASF sich erfolgreich einloggt hat (da ASF `steamID` Ihres Kontos nicht kennt, bevor Sie sich tatsächlich anmelden).

Wenn Sie alles richtig gemacht haben, starte ASF und Sie sollten es merken:

```text
[*] INFO: ImportAuthenticator() <1> Konvertiere .maFile in ASF-Format...
[*] INFO: ImportAuthenticator() <1> Import vom mobilen Authentifikator erfolgreich abgeschlossen!
```

Von nun an sollte Ihr ASF-2FA für dieses Konto einsatzbereit sein.

---

### WinAuth

Erstelle zunächst eine neue leere `BotName.maFile` Datei im ASF-Konfigurationsverzeichnis, wobei `BotName` der Name Ihres Bot ist, dem Sie ASF-2FA hinzufügen. Bedenke, dass es `BotName.maFile` und NICHT `BotName.maFile.txt` sein sollte, Windows mag es bekannte Erweiterungen standardmäßig zu verstecken. Wenn Sie einen falschen Namen angiben wird er nicht von ASF ausgewählt.

Nun starte WinAuth wie gewohnt. Nach einem Rechtsklick auf das Steam-Symbol wähle „Show SteamGuard and Recovery Code“. Dann setze einen Haken bei „Allow copy“. Sie sollten bemerken, dass Ihnen die JSON-Struktur am unteren Rand des Fensters vertraut ist, beginnend mit `{`. Kopiere den gesamten Text in die `BotName.maFile` Datei, die Sie im vorherigen Schritt erstellt haben.

Wenn Sie alles richtig gemacht haben, starte ASF und Sie sollten es merken:

```text
[*] INFO: ImportAuthenticator() <1> Konvertiere .maFile in ASF-Format...
[*] INFO: ImportAuthenticator() <1> Import vom mobilen Authentifikator erfolgreich abgeschlossen!
```

Von nun an sollte Ihr ASF-2FA für dieses Konto einsatzbereit sein.

---

## Fertig

Von diesem Moment an funktionieren alle `2fa` Befehle so, wie sie auf Ihrem herkömmlichen 2FA-Gerät ausgeführt werden. Sie können sowohl ASF-2FA, als auch den Authentifikator Ihrer Wahl (Android, iOS, SDA oder WinAuth) verwenden, um Codes zu generieren und Bestätigungen zu akzeptieren.

Wenn Sie einen Authentifikator auf Ihrem Handy haben, können Sie optional SteamDesktopAuthenticator und/oder WinAuth entfernen, da wir ihn nicht mehr benötigen. Allerdings schlage ich vor es für alle Fälle aufzubewahren, ganz zu schweigen davon, dass es praktischer ist als ein normaler Steam-Authentifikator. Bitte beachten Sie, dass ASF-2FA **KEINEN** einen allgemeinen Zweck-Authentifikationsmechanismus besitzt. Es enthält nicht alle Daten, die der Authentifikator haben sollte, sondern einen kleinen Teil der ursprünglichen `maFile`. Es ist nicht möglich ASF-2FA wieder in den Original-Authentifikator umzuwandeln, deshalb ist es wichtig, dass Sie den Universal-Authentifikator oder `maFile` an anderer Stelle (z. B. in WinAuth/SDA oder auf dem Handy haben).

---

## FAQ (oft gestellte Fragen)

### Wie nutzt ASF das 2FA-Modul?

Wenn ASF-2FA verfügbar ist wird ASF es für die automatische Bestätigung von Handelsangeboten verwenden, die von ASF gesendet bzw. akzeptiert werden. Es wird auch in der Lage sein bei Bedarf automatisch 2FA-Codes zu generieren, z. B. um sich anzumelden. Darüber hinaus ermöglicht ASF-2FA auch die Verwendung von `2fa`-Befehlen. Das sollte vorerst alles sein, falls ich nichts vergessen habe – im Grunde genommen verwendet ASF das 2FA-Modul bei Bedarf.

---

### Was ist wenn ich einen 2FA-Code benötige?

Sie benötigen einen 2FA-Code, um auf das 2FA-geschützte Konto zuzugreifen, das auch jedes Konto mit ASF-2FA einbezieht. Sie sollten Codes im Authentifikator generieren, den Sie für den Import benutzt haben, aber Sie können auch temporäre Codes über den Befehl `2fa` erzeugen, der über den Chat an den angegebenen Bot gesendet wird. Sie können auch den Befehl `2fa <BotNames>` verwenden, um temporäre Codes für bestimmte Bot-Instanzen zu erzeugen. Das sollte ausreichen, damit Sie z. B. über den Browser auf Bot-Konten zugreifen können, aber wie oben erwähnt, sollten Sie stattdessen Ihren benutzerfreundlichen Authentifikator (Android, iOS, SDA oder WinAuth) verwenden.

---

### Kann ich nach dem Import zu ASF-2FA meinen Original-Authentifikator verwenden?

Ja, Ihr Original-Authentifikator bleibt funktionsfähig und Sie können ihn zusammen mit ASF-2FA verwenden. Das ist der springende Punkt des Verfahrens – wir importieren Ihre Authentifikationsinformationen in ASF damit ASF sie nutzen und ausgewählte Bestätigungen in Ihrem Namen akzeptieren kann.

---

### Wo wird der ASF Mobile Authenticator gespeichert?

ASF Mobile Authenticator wird in der Datei `BotName.db` in Ihrem Konfigurationsverzeichnis gespeichert, zusammen mit einigen anderen wichtigen Daten, die sich auf das angegebene Konto beziehen. Wenn Sie ASF-2FA entfernen möchten, lies das untenstehende.

---

### Wie entferne ich ASF-2FA?

Stoppe einfach ASF und entferne die zugehörige `BotName.db` Datei des Bots mit der verlinkten ASF-2FA, die Sie entfernen möchten. Diese Option entfernt die zugehörige importierte 2FA mit ASF, entfernt aber NICHT Ihren Authentifikator. Wenn Sie stattdessen Ihren Authentifikator entfernen möchten, sollten Sie ihn nicht nur von ASF entfernen (zuerst), sondern auch in einem Authentifikator Ihrer Wahl (Android, iOS, SDA oder WinAuth) entfernen, oder – wenn Sie aus irgendeinem Grund nicht können, einen Widerrufscode verwenden, den Sie während der Verknüpfung dieses Authentifikators auf der Steam-Webseite erhalten haben. Es ist nicht möglich, Ihren Authentifikator über ASF zu entfernen, dafür sollte der bereits vorhandene Standard-Authentifikator verwendet werden.

---

### Ich habe den Authentifikator in SDA/WinAuth verlinkt und dann in ASF importiert. Kann ich ihn jetzt entfernen und auf meinem Handy wieder verlinken?

**Nein**. ASF **importiert** Ihre Authentifikatordaten um sie zu verwenden. Wenn Sie Ihren Authentifikator entfernst, dann werden Sie damit auch bewirken, dass ASF-2FA nicht mehr funktioniert, egal ob Sie ihn zuerst entfernst, wie in der obigen Frage angegeben ist oder nicht. Wenn Sie Ihren Authentifikator sowohl auf Ihrem Handy, als auch auf ASF (plus optional in SDA/WinAuth) verwenden möchten, dann musst Sie Ihren Authentifikator von Ihrem Handy **importieren** und keinen neuen in SDA/WinAuth erstellen. Sie können nur **einen** verknüpften Authentifikator haben, deshalb **importiert** ASF den Authentifikator und seine Daten, um ihn als ASF-2FA zu verwenden – es ist **derselbe** Authentifikator der nur an zwei Stellen existiert. Wenn Sie sich dazu entscheiden Ihre mobilen Authentifizierungsdaten zu entfernen – unabhängig davon, in welcher Weise, wird ASF-2FA die Funktionalität einstellen, da die zuvor kopierten mobilen Authentifizierungsdaten nicht mehr gültig sind. Um ASF-2FA zusammen mit dem Authentifikator auf Ihrem Handy verwenden zu können musst Sie es aus Android/iOS importieren, was oben erläutert ist.

---

### Ist die Verwendung von ASF-2FA besser als die Verwendung von WinAuth/SDA/Anderer Authentifikator um alle Bestätigungen zu akzeptieren?

**Ja**, auf unterschiedlicher Weise. Die erste und wichtigste – die Verwendung von ASF-2FA erhöht **erheblich** Ihre Sicherheit, da das ASF-2FA-Modul sicherstellt, dass ASF nur automatisch seine eigenen Bestätigungen akzeptiert, sodass selbst wenn der Angreifer ein schädliches Handelsangebot sendet, ASF-2FA dieses Handelsangebot **nicht** akzeptiert, da er nicht von ASF generiert wurde. Zusätzlich zum Sicherheitsteil bringt die Verwendung von ASF-2FA auch Performance-/Optimierungsvorteile, da ASF-2FA Bestätigungen unmittelbar nach der Generierung abruft und akzeptiert und nur dann, im Gegensatz zur ineffizienten Abfrage der Bestätigungen alle X Minuten, die z. B. von SDA oder WinAuth durchgeführt wird. Kurz gesagt, es gibt keinen Grund den Authentifikator eines Drittanbieters dem von ASF-2FA vorzuziehen, wenn Sie vorhaben von ASF generierte Bestätigungen zu automatisieren – genau dafür ist ASF-2FA gedacht und die Verwendung steht nicht im Konflikt mit der Bestätigung von allem anderen in einem Authentifikator Ihrer Wahl. Wir empfehlen nachdrücklich ASF-2FA für die gesamte ASF-Aktivität zu verwenden – dies ist viel sicherer als jede andere Lösung.

---

## Erweiterte Einstellungen

Wenn Sie ein fortgeschrittener Benutzer sind, können Sie die maFile-Datei auch manuell generieren. Dies kann in dem Fall verwendet werden, wenn Sie den Authentifikator aus anderen, als den oben erläuterten Quellen, importieren möchten. Es sollte eine **[gültige JSON-Struktur](https://jsonlint.com)** aufweisen:

```json
{
 "shared_secret": "STRING",
 "identity_secret": "STRING"
}
```

Standard-Authentifikatordaten haben mehr Felder – sie werden von ASF beim Import völlig ignoriert, da sie nicht benötigt werden. Sie musst sie nicht entfernen – ASF benötigt nur gültiges JSON mit den 2 oben erläuterten Pflichtfeldern und wird zusätzliche Felder (falls vorhanden) ignorieren. Natürlich musst Sie im obigen Beispiel `STRING` Platzhalter durch gültige Werte für Ihr Konto ersetzen. Jede `STRING` sollte base64 kodierte Darstellung von Bytes sein, aus denen der entsprechende private Schlüssel besteht.
