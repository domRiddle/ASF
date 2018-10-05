# Produktschlüsselaktivierung im Hintergrund

Der Hintergrundproduktschlüsselaktivierer ist eine besondere, in ASF integrierte Funktion, welche es dir erlaubt, eine bestimmte Menge an Steam-Produktschlüsseln (zusammen mit deren Namen) im Hintergrund aktivieren zu lassen. Das ist besonders nützlich, wenn du eine große Menge an Produktschlüsseln aktivieren möchtest und du sicherlich den `RateLimited` **[Status](https://github.com/JustArchi/ArchiSteamFarm/wiki/FAQ-de-de#what-is-the-meaning-of-status-when-redeeming-a-key)** erreichst, bevor du mit allen fertig bist.

Der Hintergrundproduktschlüsselaktivierer ist dafür gedacht, eine einzelne Bot-Umgebung zu nutzen, das bedeutet, dass dieser nicht die `RedeemingPreferences` verwendet. Diese Funktion kann bei Bedarf entweder zusammen mit oder anstelle des `redeem` **[Befehls](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands-de-de)** benutzt werden.

* * *

## Import

Der Importprozess kann über zwei Wege durchgeführt werden, entweder durch Verwendung einer Datei oder der IPC.

### Datei

ASF erkennt in seinem `Konfigurations`-Verzeichnis eine Datei mit dem Namen `BotName.keys`, wobei `BotName` der Name deines Bots ist. Diese Datei hat eine erwartete und feste Struktur, bestehend aus Spielname und Produktschlüssel, getrennt durch ein Tab-Zeichen und endend mit einem Zeilenumbruch. Wenn mehrere Tabulatoren verwendet werden, wird der erste Eintrag als Spielname und der letzte Eintrag als Produktschlüssel erachtet und alles dazwischen wird ignoriert. Zum Beispiel:

    POSTAL 2    ABCDE-EFGHJ-IJKLM
    Domino Craft VR 12345-67890-ZXCVB
    A Week of Circus Terror POIUY-KJHGD-QWERT
    Terraria    DasWirdIgnoriert   DasWirdAuchIgnoriert    ZXCVB-ASDFG-QWERT
    

ASF importiert solch eine Datei, entweder beim Bot-Start oder später während der Ausführung. Nach erfolgreichem Parsen deiner Datei und dem eventuellem Auslassen von ungültigen Einträgen werden alle korrekt erkannten Spiele der Hintergrundwarteschlange hinzugefügt, und die Datei `BotName.keys` wird dann aus dem Dateiordner `config` gelöscht.

### IPC

Zusätzlich zur Verwendung der oben genannten Produktschlüsseldateien legt ASF den `GamesToRedeemInBackground` **[API-Endpunkt](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC-de-de#post-apigamestoredeeminbackgroundbotname)** offen, welcher von jedem IPC-Tool, einschließlich unserer IPC-GUI, verwendet werden kann. Die Verwendung der IPC kann leistungsfähiger sein, da Sie selbst ein geeignetes Parsing durchführen können, wie zum Beispiel die Verwendung eines benutzerdefinierten Trennzeichens anstatt des erforderlichen Tabulatorzeichens.

* * *

## Warteschlange

Sobald die Spiele erfolgreich importiert wurden, werden sie der Warteschlange hinzugefügt. ASF arbeitet sich automatisch durch seine Hintergrundwarteschlange, solange der Bot mit dem Steam-Netzwerk verbunden ist und die Warteschlange nicht leer ist. Ein bei dem Aktivierungsvorgang verwendeter Produktschlüssel, welcher nicht zu einem `RateLimited` führte, wird aus der Warteschlange entfernt und mit seinem Ergebnis ordnungsgemäß in eine Datei im `config` Ordner geschrieben - entweder in `BotName.keys.used`, wenn der Produktschlüssel im Prozess verwendet wurde (z.B. `NoDetail`, `BadActivationCode`, `DuplicateActivationCode`) oder andernfalls `BotName.keys.unused`. ASF verwendet absichtlich den Namen Ihres Spiels, da der Produktschlüssel nicht zwingend einen aussagekräftigen Namen vom Steam-Netzwerk erhält - auf diese Weise können Sie Ihre Schlüssel sogar mit benutzerdefinierten Namen versehen, falls erforderlich/gewünscht.

Wenn während dieses Prozesses unser Konto den Status `RateLimited` erhält, wird die Warteschlange vorübergehend für eine volle Stunde unterbrochen, um die Abklingzeit abzuwarten. Danach wird der Prozess dort fortgesetzt, wo er unterbrochen wurde, und endet, wenn die gesamte Warteschlange leer ist.

* * *

## Beispiel

Nehmen wir an, du hast eine Liste mit 100 Produktschlüsseln. Zuerst solltest du eine neue `BotName.keys.new` Datei im ASF Dateiordner `config` erstellen. Wir fügen die Erweiterung `.new` hinzu, um ASF wissen zu lassen, dass es diese Datei nicht sofort beim Erstellen einlesen soll (da es sich um eine neue leere Datei handelt, die noch nicht für den Import bereit ist).

Nun kannst du unsere neue Datei öffnen und dort unsere Liste der 100 Produktschlüssel entsprechend der oben beschriebenen Struktur einfügen und bei Bedarf das Format korrigieren. Nach gegebenenfalls nötigen Korrekturen umfasst unsere Datei `BotName.keys.new` genau 100 Zeilen (oder 101, wenn man die letzte Zeile hinzuzählt), wobei jede Zeile als `Spielname\tProduktschlüssel\n` strukturiert ist, wobei `\t` ein Tabulatorzeichen und `\n` ein Zeilenumbruch darstellt.

Du bist nun bereit, diese Datei von `BotName.keys.new` in `BotName.keys` umzubenennen, um ASF mitzuteilen, dass sie bereit ist, abgeholt zu werden. In diesem Augenblick importiert ASF die Datei automatisch (ohne Neustart) und löscht sie anschließend, um zu bestätigen, dass alle unsere Spiele geparst und der Warteschlange hinzugefügt wurden.

Anstatt die Datei `BotName.keys` zu verwenden, kannst du auch den IPC API-Endpunkt verwenden, oder, wenn du es möchtest, sogar beides kombinieren.

Nach einiger Zeit werden die Dateien `BotName.keys.used` und `BotName.keys.unused` gegebenenfalls erzeugt. Diese Dateien enthalten die Ergebnisse unseres Aktivierungsprozesses. Zum Beispiel kannst du die Datei `BotName.keys.unused` in `BotName2.keys` umbenennen und somit unsere unbenutzten Schlüssel an einen anderen Bot weiterreichen, da der vorherige Bot diese Schlüssel nicht selbst verwendet hat. Oder du kannst einfach unbenutzte Schlüssel in eine andere Datei kopieren und für eine spätere Verwendung aufbewahren. Beachte bitte, dass ASF beim Abarbeiten der Warteschlange neue Einträge in die Dateien `used` und `unused` hinzufügt, wodurch es empfehlenswert ist zu warten, bis die Warteschlange vollständig geleert ist, bevor du diese Dateien verwendest. Falls du doch mal unbedingt auf diese Dateien zugreifen musst, bevor die Warteschlange vollständig geleert ist, solltest du zunächst die Ausgabedatei, auf welche du zugreifen möchtest, in ein anderen Dateiordner **verschieben**, und **dann** parsen. Dies liegt daran, dass ASF einige neue Einträge anhängen kann, während du darauf zugreifst - das kann möglicherweise zum Verlust einiger Produktschlüssel führen, wenn Sie eine Datei lesen, die z.B. 3 Produktschlüssel enthält und diese dann löschen, die Tatsache ignorierend, dass ASF in der Zwischenzeit weitere 4 Produktschlüssel zu deiner anschließend entfernten Datei hinzugefügt hat. Wenn du auf diese Dateien zugreifen möchtest, stelle bitte sicher, dass du sie aus dem ASF Dateiordner `config` entfernst, bevor du sie liest, zum Beispiel durch Umbenennen.

Es ist auch möglich, zusätzliche Spiele zu importieren, während sich einige Spiele bereits in der Warteschlange befinden, indem du alle oben genannten Schritte wiederholst. ASF wird zusätzliche Einträge ordnungsgemäß in die bereits laufende Warteschlange aufnehmen und sich um sie kümmern.

* * *

## Anmerkungen

Der Hintergrundproduktschlüsselaktivierer nutzt ein `OrderedDictionary` unter der Motorhaube, wodurch deine Produktschlüssel in der von dir in der Datei (oder per IPC API-Aufruf) spezifizierten Reihenfolge bearbeitet werden. Das bedeutet, dass du eine Liste zur Verfügung stellen kannst (und solltest), in der bestimmte Produktschlüssel nur direkte Abhängigkeiten zu den über sie aufgeführten Produktschlüsseln haben können, aber nicht zu Nachfolgenden. Beispiel: Wenn ein DLC `D` das Spiel `G` als aktiviert voraussetzt, muss das Spiel `G` **zwingend** vor dem DLC `D` enthalten sein. Dies gilt ebenfalls wenn das DLC `D` abhängig ist von `A`, `B` und `C`, dann sollten alle 3 vorher enthalten sein (in beliebiger Reihenfolge, es sei denn, es gäbe wiederum Abhängigkeiten untereinander).

Wenn du dem obigen Schema nicht folgst, wird dein DLC nicht mit `DoesNotOwnRequiredApp` aktiviert, selbst wenn dein Konto nach dem Durchlaufen der gesamten Warteschlange für die Aktivierung berechtigt wäre. Um dies zu vermeiden musst du sicherstellen, dass dein DLC immer nach dem Basisspiel in deiner Warteschlange steht.