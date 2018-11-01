# Befehle

ASF unterstützt eine Vielzahl von Befehlen, mit denen das Verhalten der Prozess- und Bot-Instanzen gesteuert werden kann.

Die unten angeführten Befehle können über drei verschiedene Wege an einen Bot gesendet werden:

- Über den privaten Steam-Chat
- Über den Steam-Gruppen-Chat
- Über **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-de-DE)**

Bedenke, dass die ASF-Interaktion von dir verlangt, dass du für den Befehl gemäß den ASF-Berechtigungen berechtigt bist. Weitere Informationen findest du in den Konfigurationseigenschaften `SteamUserPermissions` und `SteamOwnerID`.

All commands below are affected by `CommandPrefix` **[global configuration property](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#commandprefix)**, which is `!` by default. Das bedeutet, dass du für die Ausführung von z.B. dem Befehl `status` eher den Befehl `!status` (oder einen benutzerdefinierten `CommandPrefix` deiner Wahl) schreiben solltest.

* * *

### Privater Steam-Chat

Definitiv die einfachste Methode um mit ASF zu interagieren - man muss nur den Befehl an den ASF-Bot senden, der gerade im ASF-Prozess läuft. Logischerweise kannst du das nicht machen, wenn du ASF mit nur einem einzigen Bot laufen lässt.

![Screenshot](https://i.imgur.com/PPxx7qV.png)

* * *

### Steam-Gruppen-Chat

Sehr ähnlich wie die oben genannte Möglichkeit, aber diesmal im Gruppen-Chat der gegebenen Steam-Gruppe. Beachte, dass diese Option die korrekt eingestellte Eigenschaft `SteamMasterClanID` erfordert, in diesem Fall wird der Bot auch im Gruppen-Chat auf Befehle warten (und sich bei Bedarf diesem anschließen). Dies kann auch für "Selbstgespräche" genutzt werden, da es kein dediziertes Bot-Konto erfordert. Du willst diese Methode höchstwahrscheinlich nicht für mehr als 1 Bot verwenden.

* * *

### IPC

Die fortschrittlichste und flexibelste Art der Befehlsausführung, perfekt für die Benutzerinteraktion (ASF-ui) sowie für Drittanbieter-Programme oder Skripte (ASF-API), erfordert, dass ASF im `IPC` Modus ausgeführt wird und einen Client der Befehle über die **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-de-DE)** Schnittstelle ausführt.

![Screenshot](https://i.imgur.com/pzKE4EJ.png)

* * *

## Befehle

| Befehl                                                                     | Zugriff         | Beschreibung                                                                                                                                                                                                                                                          |
| -------------------------------------------------------------------------- | --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `2fa <Bots>`                                                         | `Master`        | Generiert temporäre **[2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)**-Codes für gegebene Bot-Instanzen.                                                                                                                        |
| `2fano <Bots>`                                                       | `Master`        | Lehnt alle ausstehenden **[2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)**-Bestätigungen für gegebene Bot-Instanzen ab.                                                                                                         |
| `2faok <Bots>`                                                       | `Master`        | Akzeptiert alle ausstehenden **[2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)**-Bestätigungen für gegebene Bot-Instanzen.                                                                                                       |
| `addlicense <Bots> <GameIDs>`                                  | `Operator`      | Aktiviert gegebene `appIDs` (Steam-Netzwerk) oder `subIDs` (Steam-Shop) auf gegebenen Bot-Instanzen (nur kostenlose Spiele).                                                                                                                                          |
| `bl <Bots>`                                                          | `Master`        | Listet Benutzer aus dem Handelsmodul der gegebenen Bot-Instanzen auf, die auf der schwarzen Liste stehen.                                                                                                                                                             |
| `bladd <Bots> <SteamIDs64>`                                    | `Master`        | Setzt gegebene `steamIDs` auf die Schwarze Liste des Handelsmodul der gegebenen Bot-Instanzen.                                                                                                                                                                        |
| `blrm <Bots> <SteamIDs64>`                                     | `Master`        | Entfernt gegebene `steamIDs` von der schwarzen Liste des Handelsmoduls der gegebenen Bot-Instanzen.                                                                                                                                                                   |
| `exit`                                                                     | `Owner`         | Stoppt den kompletten ASF-Prozess.                                                                                                                                                                                                                                    |
| `farm <Bots>`                                                        | `Master`        | Startet das Karten-Sammel-Modul für gegebene Bot-Instanzen neu.                                                                                                                                                                                                       |
| `help`                                                                     | `FamilySharing` | Zeigt Hilfe an (Link zu dieser Seite).                                                                                                                                                                                                                                |
| `input <Bots> <Type> <Value>`                            | `Master`        | Setzt den gegebenen Eingabetyp auf den gegebenen Wert für gegebene Bot-Instanzen. Funktioniert nur im `Headless`-Modus, genauer erklärt **[unten](#input-befehl)**.                                                                                                   |
| `ib <Bots>`                                                          | `Master`        | Listet Anwendungen auf, die vom automatischen Sammeln von gegebenen Bot-Instanzen ausgeschlossen sind.                                                                                                                                                                |
| `ibadd <Bots> <AppIDs>`                                        | `Master`        | Fügt gegebene `appIDs` zur schwarzen Liste der gegebenen Bot-Instanzen hinzu, die vom automatischen Sammeln ausgeschlossen sind.                                                                                                                                      |
| `ibrm <Bots> <AppIDs>`                                         | `Master`        | Entfernt gegebene `appIDs` von der schwarzen Liste der gegebenen Bot-Instanzen, die vom automatischen Sammeln ausgeschlossen sind.                                                                                                                                    |
| `iq <Bots>`                                                          | `Master`        | Listet die Priorität der Sammel-Warteschlange der gegebenen Bot-Instanzen auf.                                                                                                                                                                                        |
| `iqadd <Bots> <AppIDs>`                                        | `Master`        | Fügt gegebene `appIDs` zur Priorität Sammel-Warteschlange der gegebenen Bot-Instanzen hinzu.                                                                                                                                                                          |
| `iqrm <Bots> <AppIDs>`                                         | `Master`        | Entfernt gegebene `appIDs` aus der Priorität Sammel-Warteschlange der gegebenen Bot-Instanzen.                                                                                                                                                                        |
| `loot <Bots>`                                                        | `Master`        | Sendet alle `LootableTypes` Steam-Community-Gegenstände gegebener Bot-Instanzen an `Master` welcher in den `SteamUserPermissions` (mit niedrigster steamID, wenn mehr als eine) definiert ist.                                                                        |
| `loot@ <Bots> <RealAppIDs>`                                    | `Master`        | Sendet alle `LootableTypes` Steam-Community-Gegenstände, die mit den gegebenen `RealAppIDs` der gegebenen Bot-Instanzen übereinstimmen, an `Master` welcher in den `SteamUserPermissions` (mit der niedrigsten SteamID, wenn mehr als eine) definiert ist.            |
| `loot^ <Bots> <AppID> <ContextID>`                       | `Master`        | Sendet alle Steam-Gegenstände von gegebenem `AppID` in `ContextID` der gegebenen Bot-Instanzen an `Master` welcher in den `SteamUserPermissions` (mit niedrigster SteamID wenn mehr als eine) definiert ist.                                                          |
| `loot& <Bots>`                                                   | `Master`        | Schaltet das Plündern gegebener Bot-Instanzen zwischen aktiviertem und deaktiviertem Zustand um.                                                                                                                                                                      |
| `nickname <Bots> <Nickname>`                                   | `Master`        | Ändert den Steam-Namen gegebener Bot-Instanzen auf gegeben `nickname`.                                                                                                                                                                                                |
| `owns <Bots> <AppIDsOrGameNames>`                              | `Operator`      | Prüft, ob gegebene Bot-Instanzen bereits gegebene `appIDs` und/oder `gameNames` besitzen (kann Teil des Spielnamens sein). Es kann auch `*` sein, um alle verfügbaren Spiele anzuzeigen.                                                                              |
| `password <Bots>`                                                    | `Master`        | Gibt das verschlüsselte Passwort der gegebenen Bot-Instanzen an (in Verwendung mit `PasswordFormat`).                                                                                                                                                                 |
| `pause <Bots>`                                                       | `Operator`      | Pausiert permanent das automatische Karten-Sammel-Modul der gegebenen Bot-Instanzen. ASF wird nicht versuchen, das aktuelle Konto in dieser Sitzung zu verwalten, es sei denn, du setzt `resume` manuell ein oder startest den Prozess neu.                           |
| `pause~ <Bots>`                                                      | `FamilySharing` | Pausiert vorübergehend das automatische Karten-Sammel-Modul der gegebenen Bot-Instanzen. Das Sammeln wird beim nächsten Wiedergabe-Ereignis oder beim Trennen der Bot-Verbindung automatisch wieder fortgesetzt. Du kannst `resume` verwenden, um es zu deaktivieren. |
| `pause& <Bots> <Seconds>`                                  | `Operator`      | Pausiert vorübergehend das automatische Karten-Sammel-Modul gegebener Bot-Instanzen für eine bestimmte Anzahl von `seconds`. Nach einer Verzögerung wird das Karten-Sammel-Modul automatisch wieder aktiviert.                                                        |
| `play <Bots> <AppIDs,GameName>`                                | `Master`        | Wechselt zu manuellem Sammeln - startet gegebene `AppIDs` auf gegebenen Bot-Instanzen, optional auch mit benutzerdefiniertem `GameName`. Benutze `resume`, um zum automatischen Sammeln zurückzukehren.                                                               |
| `privacy <Bots> <Settings>`                                    | `Master`        | Ändert die **[Steam Privatsphäre-Einstellungen](https://steamcommunity.com/my/edit/settings)** der gegebenen Bot-Instanzen, zu entsprechend ausgewählten Optionen erklärt **[unten](#privacy-einstellungen)**.                                                        |
| `redeem <Bots> <Keys>`                                         | `Operator`      | Löst gegebene `cd-keys` auf gegebenen Bot-Instanzen ein.                                                                                                                                                                                                              |
| `redeem^ <Bots> <Modes> <Keys>`                          | `Operator`      | Löst gegebene `cd-keys` auf gegebenen Bot-Instanzen ein, indem er gegebene `Modi` verwendet, die **[unten](#redeem-modi)** erklärt werden.                                                                                                                            |
| `rejoinchat <Bots>`                                                  | `Operator`      | Zwingt gegebene Bot-Instanzen, sich wieder ihrem `SteamMasterClanID` Gruppen-Chat anzuschließen.                                                                                                                                                                      |
| `restart`                                                                  | `Owner`         | Startet den ASF-Prozess neu.                                                                                                                                                                                                                                          |
| `resume <Bots>`                                                      | `FamilySharing` | Setzt das automatische Sammeln der gegebenen Bot-Instanzen fort. Siehe auch `pause`, `play`.                                                                                                                                                                          |
| `start <Bots>`                                                       | `Master`        | Startet gegebene Bot-Instanzen.                                                                                                                                                                                                                                       |
| `stats`                                                                    | `Owner`         | Gibt Prozessstatistiken an, wie z.B. die Nutzung des verwalteten Speichers.                                                                                                                                                                                           |
| `status <Bots>`                                                      | `FamilySharing` | Gibt den Status der gegebenen Bot-Instanzen an.                                                                                                                                                                                                                       |
| `stop <Bots>`                                                        | `Master`        | Stoppt gegebene Bot-Instanzen.                                                                                                                                                                                                                                        |
| `transfer <Bots> <TargetBot>`                                  | `Master`        | Sendet alle `TransferableTypes` Steam Community-Gegenstände von gegebenen Bot-Instanzen an den Ziel-Bot.                                                                                                                                                              |
| `transfer@ <Bots> <RealAppIDs> <TargetBot>`              | `Master`        | Sendet alle `TransferableTypes` Steam-Community-Gegenstände, die mit den gegebenen `RealAppIDs` übereinstimmen, von gegebenen Bot-Instanzen an den Ziel-Bot.                                                                                                          |
| `transfer^ <Bots> <AppID> <ContextID> <TargetBot>` | `Master`        | Sendet alle Steam-Gegenstände von gegebenem `AppID` in `ContextID` der gegebenen Bot-Instanzen an den Ziel-Bot.                                                                                                                                                       |
| `unpack <Bots>`                                                      | `Master`        | Entpackt alle Booster Packs, die im Inventar der gegebenen Bot-Instanzen gespeichert sind.                                                                                                                                                                            |
| `update`                                                                   | `Owner`         | Checks GitHub for ASF updates (this is done automatically every `UpdatePeriod`).                                                                                                                                                                                      |
| `version`                                                                  | `FamilySharing` | Gibt die ASF-Version an.                                                                                                                                                                                                                                              |

* * *

### Anmerkungen

Bei Befehlen selbst ist die Groß- und Kleinschreibung egal, aber bei den Argumenten dieser (wie zum Beispiel Botnamen) ist sich an entsprechende Groß- und Kleinschreibung zu halten.

Das Argument `<Bots>` ist in allen Befehlen optional. Wenn spezifiziert wird der Befehl auf den angegebenen Bots ausgeführt. Ohne diese Angabe wird der Befehl auf dem aktuellen Bot ausgeführt der den Befehl erhält. Mit anderen Worten, `status A` der an Bot `B` gesendet wird ist dasselbe wie `status` an Bot `A` zu senden.

**Zugriff** des Befehls definiert **minimale** `EPermission` von `SteamUserPermissions` die für die Verwendung des Befehls erforderlich ist - mit Ausnahme von `Owner` das als `SteamOwnerID` in der globalen Konfigurationsdatei (und höchste verfügbare Berechtigung) definiert ist.

Plural Argumente, wie `<Bots>`, `<Keys>` oder `<AppIDs>` bedeuten, dass der Befehl mehrere Argumente eines bestimmten Typs unterstützt, getrennt durch ein Komma. Zum Beispiel kann `status <Bots>` als `status MeinBot,MeinAndererBot,MeinKonto` verwendet werden. Dies führt dazu, dass der angegebene Befehl auf **alle Ziel-Bots** auf die gleiche Weise ausgeführt wird, wie wenn du `status` an jeden Bot in einem separaten Chat-Fenster senden würdest. Bitte beachte, dass nach `,` kein Leerzeichen steht.

ASF verwendet alle Leerzeichen als mögliche Trennzeichen für einen Befehl, wie Leerzeichen und Zeilenumbrüche. Das bedeutet, dass du zur Begrenzungen deiner Argumente keine Leerzeichen verwenden musst, du kannst auch jedes andere Whitespace-Zeichen (z.B. Tabulator oder Zeilenumbruch) verwenden.

ASF wird zusätzliche "Out-of-Range"-Argumente mit dem Plural-Typ des letzten "In-Range"-Arguments "verbinden". Das bedeutet, dass `redeem bot key1 key2 key3` für `redeem <Bots> <Keys>` genau so funktioniert wie `redeem bot key1,key2,key3`. Zusammen mit der Annahme eines Zeilenumbruchs als Befehlsbegrenzer ermöglicht dir dies, `redeem bot` zu schreiben und dann eine Liste von Produktschlüsseln einzufügen, die durch ein beliebiges zulässiges Trennzeichen (z.B. Zeilenumbruch) oder Standard `,` ASF-Trennzeichen getrennt sind. Bedenke, dass dieser Trick nur für eine Befehlsvariante verwendet werden kann, die die meiste Anzahl von Argumenten verwendet (daher ist die Angabe von `<Bots>` in diesem Fall obligatorisch).

Wie du oben gelesen hast, wird ein Leerzeichen als Trennzeichen für einen Befehl verwendet, daher kann es nicht in Argumenten verwendet werden. Allerdings kann ASF auch, wie oben erwähnt, "Out-of-Range"-Argumente verbinden, was bedeutet, dass du tatsächlich ein Leerzeichen in Argumenten verwenden kannst, das als letztes für einen gegebenen Befehl definiert ist. Zum Beispiel, `nickname bob Great Bob` wird den Benutzernamen des Bots `bob` richtig auf "Great Bob" setzen. Auf die gleiche Weise kannst du Namen, die Leerzeichen enthalten, im Befehl `owns` überprüfen.

Bitte bedenke, dass das Senden eines Befehls an den Gruppen-Chat wie ein Relais funktioniert - wenn du sagst, dass `redeem X` an 3 deiner Bots, die zusammen mit dir im Gruppen-Chat sitzen, gesendet wird, wird es das gleiche Ergebnis haben, wie wenn du `redeem X` an jeden einzelnen von ihnen privat senden würdest. In den meisten Fällen ist **dies nicht das, was du willst**, und stattdessen solltest du den Befehl `given bot` verwenden, der an **einen einzelnen Bot im privaten Fenster** gesendet wird. ASF unterstützt den Gruppen-Chat, da es in vielen Fällen eine nützliche Kommunikationsquelle sein kann, aber du solltest fast nie einen Befehl im Gruppen-Chat ausführen, wenn dort 2 oder mehr ASF-Bots sitzen, es sei denn, du verstehst das hier geschriebene ASF-Verhalten vollständig und du willst tatsächlich den gleichen Befehl an jeden einzelnen Bot weitergeben, der dir zuhört.

*Und selbst in diesem Fall solltest du stattdessen den privaten Chat mit der Syntax `<Bots>` verwenden.*

* * *

Einige Befehle sind auch als Aliase verfügbar, um Zeit beim tippen zu sparen:

| Befehl       | Alias |
| ------------ | ----- |
| `owns ASF`   | `oa`  |
| `status ASF` | `sa`  |
| `redeem`     | `r`   |
| `redeem^`    | `r^`  |

* * *

Es ist nicht erforderlich, ein zusätzliches Konto für die Ausführung von Befehlen über den Steam-Chat zu haben - Du kannst eine Gruppe erstellen, `SteamMasterClanID` richtig auf diese neu erstellte Gruppe einstellen und dir dann entweder über `SteamOwnerID` oder `SteamUserPermissions` deines eigenen Bots Zugriff geben kannst. Auf diese Weise tritt der ASF-Bot (Du) der Gruppe bei und chattet mit der ausgewählten Gruppe und hört sich die Befehle von deinem eigenen Konto an. Du kannst dem gleichen Gruppen-Chatroom beitreten, um dir selbst Befehle zu erteilen (wie du den Befehl an den Chatroom senden wirst, und die ASF-Instanz, die im gleichen Chatroom sitzt, wird sie empfangen, auch wenn sie nur anzeigt, dass dein Konto dort ist). Abgesehen davon kannst du auch **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-de-DE)** verwenden, aber der Lösung mittels Chatroom ist viel einfacher, und wenn du Zugang zu einem Bot-Konto hast, dann ist die Verwendung dieses Kontos stattdessen noch einfacher.

* * *

### `<Bots>` Argument

`<Bots>` Argument ist eine spezielle Variante des Plural Arguments, da es neben der Annahme mehrerer Werte auch zusätzliche Funktionalität bietet.

Es gibt zunächst ein spezielles Schlüsselwort `ASF`, das als "alle Bots im Prozess" fungiert, so dass `status ASF` Befehl gleich `status all,your,bots,listed,here` ist. Dies kann auch verwendet werden, um die Bots zu identifizieren, auf die du Zugriff hast, da das `ASF` Schlüsselwort, trotz der Ausrichtung auf alle Bots, nur aus den Bots eine Antwort ergibt, an die du den Befehl tatsächlich senden kannst.

Das Argument `<Bots>` unterstützt eine spezielle " Bereichs"-Syntax, die es dir ermöglicht, einen Bereich von Bots einfacher auszuwählen. Die allgemeine Syntax für `<Bots>` lautet in diesem Fall `ersterBot..letzterBot`. Wenn du zum Beispiel Bots mit den Namen `A, B, C, D, E, F` hast, kannst du `status B..E` ausführen, was in diesem Fall gleich `status B,C,D,E` ist. Bei Verwendung dieser Syntax verwendet ASF die alphabetische Sortierung, um festzustellen, welche Bots sich in dem von dir angegebenen Bereich befinden. Sowohl `ersterBot` als auch `letzterBot` müssen gültige Bot-Namen sein, die von ASF erkannt werden, da sonst die Bereichs-Syntax vollständig übersprungen wird.

Zusätzlich zur obigen Bereichs-Syntax unterstützt das Argument `<Bots>` auch den **[Regex](https://en.wikipedia.org/wiki/Regular_expression)** Abgleich. Du kannst Regex Muster aktivieren, indem du `r!<pattern>` als Bot-Namen verwendest, wobei `r!` ASF-Aktivator für den Regex Abgleich ist, und `<pattern>` dein Regex-Muster ist. Ein Beispiel für einen regex-basierten Bot-Befehl wäre `status r!\d{3}`, der den Befehl `status` an Bots sendet, die einen aus 3 Ziffern bestehenden Namen haben (z.B. `123` und `981`). Zögere nicht einen Blick auf die **[Dokumentation](https://docs.microsoft.com/en-us/dotnet/standard/base-types/regular-expression-language-quick-reference)** zu werfen, um weitere Erklärungen und Beispiele für verfügbare Regex-Muster zu erhalten.

* * *

## `privacy` Einstellungen

`<Settings>` Argument akzeptiert **bis zu 7** verschiedene Optionen, getrennt wie üblich durch das standard ASF-Trennzeichen, das Komma. Diese sind, in der Reihenfolge:

| Argument | Name           | Kind von   |
| -------- | -------------- | ---------- |
| 1        | Profile        |            |
| 2        | OwnedGames     | Profile    |
| 3        | Playtime       | OwnedGames |
| 4        | FriendsList    | Profile    |
| 5        | Inventory      | Profile    |
| 6        | InventoryGifts | Inventory  |
| 7        | Comments       | Profile    |

Für eine Beschreibung der oben genannten Felder besuche bitte die **[Steam Privatsphäre-Einstellungen](https://steamcommunity.com/my/edit/settings)**.

Während gültige Werte für alle von ihnen sind:

| Wert | Name          |
| ---- | ------------- |
| 1    | `Private`     |
| 2    | `FriendsOnly` |
| 3    | `Public`      |

Du kannst entweder einen case-insensitiven Namen oder einen numerischen Wert verwenden. Argumente, die weggelassen wurden, werden standardmäßig auf `Private` gesetzt. Es ist wichtig, die Beziehung zwischen Kind und Eltern von oben genannten Argumenten zu beachten, da das Kind nie mehr offene Berechtigung haben kann als sein Elternteil. Zum Beispiel kannst du **nicht** Spiele im Besitz auf `Public` haben, während dein Profil auf `Private` hast.

### Beispiel

Wenn du **alle** Privatsphäre-Einstellungen deines Bots namens `Main` auf `Private` setzen möchtest, kannst du jede der folgenden Optionen verwenden:

    privacy Main 1
    privacy Main Private
    

Dies liegt daran, dass ASF automatisch alle anderen Einstellungen als `Private` ansieht, so dass es nicht notwendig ist, sie anzugeben. Wenn du dagegen alle Privatsphäre-Einstellungen auf `Public` setzen möchtest, dann solltest du eine der folgenden Optionen verwenden:

    privacy Main 3,3,3,3,3,3,3
    privacy Main Public,Public,Public,Public,Public,Public,Public
    

Auf diese Weise kannst du auch unabhängige Einstellungen vornehmen, wie du willst:

    privacy Main Public,FriendsOnly,Private,Public,Public,Private,Public
    

Das oben genannte wird das Profil auf öffentlich setzen, eigene Spiele nur für Freunde, Spielzeit auf privat, Freundesliste auf öffentlich, Inventar auf öffentlich, Inventar Geschenke auf privat und Profil-Kommentare auf öffentlich. Das Gleiche kann man mit numerischen Werten erreichen, wenn man will.

Bedenke, dass ein Kind nie mehr offene Berechtigungen haben kann als sein Elternteil. Siehe hierzu die Argumentationsbeziehung für die verfügbaren Optionen.

* * *

## `redeem^` Modi

Der Befehl `redeem^` ermöglicht es dir, die Modi zu optimieren, die für ein einziges Redem-Szenario verwendet werden. Dies funktioniert als temporäre Überschreibung der `RedeemingPreferences` **[Bot-Konfigurationseigenschaft](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-de-DE#redeemingpreferences)**.

`<Modes>` Argument akzeptiert mehrere Modus-Werte, getrennt wie üblich durch ein Komma. Die verfügbaren Modus-Werte sind im Folgenden aufgeführt:

| Wert | Name                  | Beschreibung                                                                                                  |
| ---- | --------------------- | ------------------------------------------------------------------------------------------------------------- |
| FD   | ForceDistributing     | Erzwingt die Aktivierung der `Distributing` Einlöse-Präferenz                                                 |
| FF   | ForceForwarding       | Erzwingt die Aktivierung der `Forwarding` Einlöse-Präferenz                                                   |
| FKMG | ForceKeepMissingGames | Erzwingt die Aktivierung der `KeepMissingGames` Einlöse-Präferenz                                             |
| SD   | SkipDistributing      | Erzwingt die Deaktivierung der `Distributing` Einlöse-Präferenz                                               |
| SF   | SkipForwarding        | Erzwingt die Deaktivierung der `Forwarding` Einlöse-Präferenz                                                 |
| SI   | SkipInitial           | Überspringt die Produktschlüssel-Aktivierung beim ersten Bot                                                  |
| SKMG | SkipKeepMissingGames  | Erzwingt die Deaktivierung der `KeepMissingGames` Einlöse-Präferenz                                           |
| V    | Validate              | Validiert die Produktschlüssel auf das richtige Format und überspringt automatisch ungültige Produktschlüssel |

Zum Beispiel möchten wir 3 Produktschlüssel auf einem unserer Bots einlösen, die noch keine Spiele besitzen, aber nicht auf unserem `primary` Bot. Um das zu erreichen, können wir Folgendes nutzen:

`redeem^ primary FF,SI key1,key2,key3`

* * *

## `input` Befehl

Der Input-Befehl kann nur im `Headless`-Modus verwendet werden, um gegebene Daten über **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-de-DE)** oder Steam-Chat einzugeben, wenn ASF ohne Unterstützung für Benutzerinteraktion läuft.

Die allgemeine Syntax ist `input <Bots> <Type> <Value>`.

`<Type>` ist Groß-/Kleinschreibung unabhängig und definiert den von ASF erkannten Eingangstyp. Derzeit erkennt ASF folgende Typen:

| Typ                     | Beschreibung                                                                              |
| ----------------------- | ----------------------------------------------------------------------------------------- |
| DeviceID                | 2FA Geräte-Identifikation, wenn sie in `.maFile` fehlt.                                   |
| Login                   | `SteamLogin` Bot-Konfigurationseigenschaft, wenn sie in der Konfiguration fehlt.          |
| Password                | `SteamPassword` Bot-Konfigurationseigenschaft, wenn sie in der Konfiguration fehlt.       |
| SteamGuard              | Auth-Code, der an deine E-Mail gesendet wird, wenn du 2FA nicht benutzt.                  |
| SteamParentalCode       | ` SteamParentalCode ` Bot-Konfigurationseigenschaft, wenn sie in der Konfiguration fehlt. |
| TwoFactorAuthentication | 2FA-Code, der von deinem Handy generiert wurde, wenn du 2FA benutzt, aber nicht ASF 2FA.  |

`<Value>` ist der Wert, der für einen gegebenen Typ festgelegt ist. Derzeit sind alle Werte Zeichenketten.

### Beispiel

Lass uns annehmen, dass wir einen Bot haben, der durch SteamGuard (nicht im Zwei-Faktor-Modus) geschützt wird. Wir wollen diesen Bot starten während das Konfigurationsfeld `Headless` auf wahr gesetzt ist.

Um das zu tun müssen wir folgende Befehle ausführen:

`start MeinSteamGuardBot` -> Der Bot wird versuchen zu starten, was allerdings fehlschlagen wird, weil ein Authentifizierungscode benötigt wird. Dann wird er sich selbst stoppen, weil ASF im `Headless`-Modus läuft. Wir brauchen dies, damit das Steam-Netzwerk uns den Authentisierungscode an unsere E-Mail sendet - wenn es keinen Bedarf dafür gibt, würden wir diesen Schritt komplett überspringen.

`input MeinSteamGuardBot SteamGuard ABCDE` -> Wir setzen den `SteamGuard`-Input von `MeinSteamGuardBot` auf `ABCDE`. Natürlich sollte `ABCDE` der Code sein, den du in deiner E-Mail erhalten hast.

`start MeinSteamGuardBot` -> wir starten unseren (gestoppten) Bot wieder. Diesmal wird der Code benutzt, den wir im vorherigen Befehl gesetzt haben und der Bot loggt sich ein und löscht den Code aus seinem internen Speicher.

Auf dem selben Weg können wir auf Bots, die durch Zwei-Faktor-Authentifizierung geschützt sind (und nicht die 2FA von ASF verwenden), zugreifen und andere Dinge während der Laufzeit tun.