# Befehle

ASF unterstützt eine Vielzahl von Befehlen, mit denen das Verhalten der Prozess- und Bot-Instanzen gesteuert werden kann.

Die unten angeführten Befehle können über drei verschiedene Wege an einen Bot gesendet werden:

- Über den privaten Steam-Chat
- Über den Steam-Gruppen-Chat
- Über **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-de-DE)**

Bedenke, dass die ASF-Interaktion von dir verlangt, dass du für den Befehl gemäß den ASF-Berechtigungen berechtigt bist. Weitere Informationen findest du in den Konfigurationseigenschaften `SteamUserPermissions` und `SteamOwnerID`.

Alle unten aufgeführten Befehle werden durch das **[globale Konfigurationsfeld](https://github. com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-de-DE#commandprefix)** `CommandPrefix` beeinflusst, welches standardmäsig `!` ist. Das bedeutet, dass du beispielsweise für die Ausführung von dem Befehl `status` tatsächlich `!status` (oder einen benutzerdefinierten `CommandPrefix` deiner Wahl) verwenden musst.

* * *

### Privater Steam-Chat

Definitiv die einfachste Methode um mit ASF zu interagieren - man muss nur den Befehl an einen ASF-Bot senden, der gerade im ASF-Prozess läuft. Logischerweise kannst du das nicht machen, wenn du ASF mit nur einem einzigen Bot laufen lässt.

![Screenshot](https://i.imgur.com/PPxx7qV.png)

* * *

### Steam-Gruppen-Chat

Sehr ähnlich wie die oben genannte Möglichkeit, aber diesmal im Gruppen-Chat der gegebenen Steam-Gruppe. Beachte, dass diese Option die korrekt eingestellte Eigenschaft `SteamMasterClanID` erfordert. In diesem Fall wird der Bot auch im Gruppen-Chat auf Befehle warten (und sich bei Bedarf diesem anschließen). Dies kann auch für "Selbstgespräche" genutzt werden, da diese Variante kein dediziertes Bot-Konto erfordert. Du willst diese Methode höchstwahrscheinlich nicht für mehr als einen Bot verwenden.

* * *

### IPC

Die fortschrittlichste und flexibelste Art der Befehlsausführung. Perfekt geeignet für die Benutzerinteraktion (ASF-ui), für Drittanbieter-Programme oder Skripte (ASF-API). Diese Variante erfordert, dass ASF im `IPC` Modus ausgeführt wird und einen Client der Befehle über die **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-de-DE)** Schnittstelle ausführt.

![Screenshot](https://i.imgur.com/pzKE4EJ.png)

* * *

## Befehle

| Befehl                                                                     | Zugriff         | Beschreibung                                                                                                                                                                                                                                                                   |
| -------------------------------------------------------------------------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `2fa <Bots>`                                                         | `Master`        | Generiert temporäre **[2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-de-DE)**-Codes der angegebenen Bot-Instanzen.                                                                                                                        |
| `2fano <Bots>`                                                       | `Master`        | Lehnt alle ausstehenden **[2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-de-DE)**-Bestätigungen der angegebenen Bot-Instanzen ab.                                                                                                         |
| `2faok <Bots>`                                                       | `Master`        | Akzeptiert alle ausstehenden **[2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-de-DE)**-Bestätigungen der angegebenen Bot-Instanzen.                                                                                                       |
| `addlicense <Bots> <GameIDs>`                                  | `Operator`      | Aktiviert die angegebenen `appIDs` (Steam-Netzwerk) oder `subIDs` (Steam-Shop) auf den angegebenen Bot-Instanzen (nur kostenlose Spiele).                                                                                                                                      |
| `balance <Bots>`                                                     | `Master`        | Zeigt das Guthaben der angegebenen Bot-Instanzen an.                                                                                                                                                                                                                           |
| `bl <Bots>`                                                          | `Master`        | Zeigt Benutzer aus dem Handelsmodul der angegebenen Bot-Instanzen an, die auf der schwarzen Liste stehen.                                                                                                                                                                      |
| `bladd <Bots> <SteamIDs64>`                                    | `Master`        | Setzt gegebene `steamIDs` auf die schwarze Liste des Handelsmodul der gegebenen Bot-Instanzen.                                                                                                                                                                                 |
| `blrm <Bots> <SteamIDs64>`                                     | `Master`        | Entfernt die angegebenen `steamIDs` von der schwarzen Liste des Handelsmoduls der angegebenen Bot-Instanzen.                                                                                                                                                                   |
| `exit`                                                                     | `Owner`         | Stoppt den kompletten ASF-Prozess.                                                                                                                                                                                                                                             |
| `farm <Bots>`                                                        | `Master`        | Startet das Karten-Sammel-Modul der angegebenen Bot-Instanzen neu.                                                                                                                                                                                                             |
| `help`                                                                     | `FamilySharing` | Zeigt die Hilfe an (Link zu dieser Seite).                                                                                                                                                                                                                                     |
| `input <Bots> <Type> <Value>`                            | `Master`        | Setzt den angegebenen Eingabetyp auf den angegebenen Wert für die angegebenen Bot-Instanzen. Funktioniert nur im `Headless`-Modus, genauer erklärt **[unten](#input-befehl)**.                                                                                                 |
| `ib <Bots>`                                                          | `Master`        | Listet Spiele der angegebenen Bot-Instanzen auf, die auf der schwarzen Liste des Sammelmoduls stehen.                                                                                                                                                                          |
| `ibadd <Bots> <AppIDs>`                                        | `Master`        | Setzt die angegebenen `appIDs` auf die schwarze Liste des Sammelmoduls der angegebenen Bot-Instanzen.                                                                                                                                                                          |
| `ibrm <Bots> <AppIDs>`                                         | `Master`        | Entfernt die angegebenen `appIDs` von der schwarzen Liste des Sammelmoduls der angegebenen Bot-Instanzen.                                                                                                                                                                      |
| `iq <Bots>`                                                          | `Master`        | Zeigt die priorisierte Sammel-Warteschlange der angegebenen Bot-Instanzen an.                                                                                                                                                                                                  |
| `iqadd <Bots> <AppIDs>`                                        | `Master`        | Fügt die angegebenen `appIDs` zur priorisierten Sammel-Warteschlange der angegebenen Bot-Instanzen hinzu.                                                                                                                                                                      |
| `iqrm <Bots> <AppIDs>`                                         | `Master`        | Entfernt die angegebenen `appIDs` aus der priorisierten Sammel-Warteschlange der angegebenen Bot-Instanzen.                                                                                                                                                                    |
| `level <Bots>`                                                       | `Master`        | Gibt das Steam-Level der angegebenen Bot-Instanzen an.                                                                                                                                                                                                                         |
| `loot <Bots>`                                                        | `Master`        | Sendet alle `LootableTypes` Steam-Community-Gegenstände gegebener Bot-Instanzen an `Master` welcher in den `SteamUserPermissions` (mit niedrigster steamID, wenn mehr als eine) definiert ist.                                                                                 |
| `loot@ <Bots> <RealAppIDs>`                                    | `Master`        | Sendet alle `LootableTypes` Steam-Community-Gegenstände, die mit den angegebenen `RealAppIDs` der angegebenen Bot-Instanzen übereinstimmen, an `Master` welcher in den `SteamUserPermissions` (mit der niedrigsten SteamID, wenn mehr als eine eingetragen ist) definiert ist. |
| `loot^ <Bots> <AppID> <ContextID>`                       | `Master`        | Sendet alle Steam-Gegenstände von gegebenem `AppID` in `ContextID` der gegebenen Bot-Instanzen an `Master` welcher in den `SteamUserPermissions` (mit niedrigster SteamID wenn mehr als eine) definiert ist.                                                                   |
| `nickname <Bots> <Nickname>`                                   | `Master`        | Ändert den Steam-Namen der angegebenen Bot-Instanzen auf den angegeben `nickname`.                                                                                                                                                                                             |
| `owns <Bots> <AppIDsOrGameNames>`                              | `Operator`      | Prüft, ob die angegebenen Bot-Instanzen bereits die angegebenen `appIDs` und/oder `gameNames` besitzen (kann auch nur ein Teil des Spielnamens sein). Es kann auch `*` eingegeben werden, um alle verfügbaren Spiele anzuzeigen.                                               |
| `password <Bots>`                                                    | `Master`        | Zeigt das verschlüsselte Passwort der angegebenen Bot-Instanzen an (in Verwendung mit `PasswordFormat`).                                                                                                                                                                       |
| `pause <Bots>`                                                       | `Operator`      | Pausiert permanent das automatische Karten-Sammel-Modul der angegebenen Bot-Instanzen. ASF wird in der aktuellen Sitzung nicht versuchen auf den angegebenen Bots zu Sammeln, es sei denn du verwendest selbst `resume` oder startest den Prozess neu.                         |
| `pause~ <Bots>`                                                      | `FamilySharing` | Pausiert vorübergehend das automatische Karten-Sammel-Modul der angegebenen Bot-Instanzen. Das Sammeln wird automatisch beim nächsten Spielen-Ereignis oder beim Trennen der Bot-Verbindung fortgesetzt. Du kannst `resume` verwenden um die Pause zu beenden.                 |
| `pause& <Bots> <Seconds>`                                  | `Operator`      | Pausiert vorübergehend das automatische Karten-Sammel-Modul der angegebenen Bot-Instanzen für eine bestimmte Anzahl von `seconds`. Nach der angegebenen Zeit wird das Karten-Sammel-Modul automatisch wieder aktiviert.                                                        |
| `play <Bots> <AppIDs,GameName>`                                | `Master`        | Wechselt zum manuellem Sammeln - startet die angegebenen `AppIDs` auf den angegebenen Bot-Instanzen, optional auch mit benutzerdefiniertem `GameName`. Benutze `resume`, um zum automatischen Sammeln zurückzukehren.                                                          |
| `privacy <Bots> <Settings>`                                    | `Master`        | Ändert die **[Steam Privatsphäre-Einstellungen](https://steamcommunity.com/my/edit/settings)** der angegebenen Bot-Instanzen, zu den entsprechend ausgewählten Optionen. Diese sind weiter **[unten](#privacy-einstellungen)** erklärt.                                        |
| `redeem <Bots> <Keys>`                                         | `Operator`      | Löst die angegebenen `cd-keys` auf den angegebenen Bot-Instanzen ein.                                                                                                                                                                                                          |
| `redeem^ <Bots> <Modes> <Keys>`                          | `Operator`      | Löst die angegebenen `cd-keys` auf den angegebenen Bot-Instanzen ein, indem die angegebenen `Modi` verwendet werden. Diese werden weiter **[unten](#redeem-modi)** erklärt.                                                                                                    |
| `rejoinchat <Bots>`                                                  | `Operator`      | Zwingt die angegebene Bot-Instanzen sich wieder ihrem `SteamMasterClanID` Gruppen-Chat anzuschließen.                                                                                                                                                                          |
| `restart`                                                                  | `Besitzer`      | Startet den ASF-Prozess neu.                                                                                                                                                                                                                                                   |
| `resume <Bots>`                                                      | `FamilySharing` | Setzt das automatische Sammeln der angegebenen Bot-Instanzen fort. Siehe auch `pause`, `play`.                                                                                                                                                                                 |
| `start <Bots>`                                                       | `Master`        | Startet die angegebene Bot-Instanzen.                                                                                                                                                                                                                                          |
| `stats`                                                                    | `Owner`         | Zeigt Prozessstatistiken, wie z.B. die Nutzung des verwalteten Speichers an.                                                                                                                                                                                                   |
| `status <Bots>`                                                      | `FamilySharing` | Zeigt den Status der angegebenen Bot-Instanzen an.                                                                                                                                                                                                                             |
| `stop <Bots>`                                                        | `Master`        | Stoppt die angegebenen Bot-Instanzen.                                                                                                                                                                                                                                          |
| `transfer <Bots> <TargetBot>`                                  | `Master`        | Sendet alle `TransferableTypes` Steam Community-Gegenstände von den angegebenen Bot-Instanzen an den Ziel-Bot.                                                                                                                                                                 |
| `transfer@ <Bots> <RealAppIDs> <TargetBot>`              | `Master`        | Sendet alle `TransferableTypes` Steam-Community-Gegenstände, die mit den angegebenen `RealAppIDs` übereinstimmen, von den angegebenen Bot-Instanzen an den Ziel-Bot.                                                                                                           |
| `transfer^ <Bots> <AppID> <ContextID> <TargetBot>` | `Master`        | Sendet alle Steam-Gegenstände von angegebener `AppID` in `ContextID` der angegebenen Bot-Instanzen an den Ziel-Bot.                                                                                                                                                            |
| `unpack <Bots>`                                                      | `Master`        | Entpackt alle Booster Packs, die im Inventar der angegebenen Bot-Instanzen vorhanden sind.                                                                                                                                                                                     |
| `update`                                                                   | `Owner`         | Überprüft GitHub auf ASF-Aktualisierungen (dies geschieht automatisch alle `UpdatePeriod`).                                                                                                                                                                                    |
| `version`                                                                  | `FamilySharing` | Zeigt die ASF-Version an.                                                                                                                                                                                                                                                      |

* * *

### Anmerkungen

Bei Befehlen selbst ist die Groß- und Kleinschreibung egal, aber bei deren Argumenten (wie zum Beispiel Bot-Namen) muss die Groß- und Kleinschreibung beachtet werden.

Das Argument `<Bots>` ist in allen Befehlen optional. Wenn verwendet, wird der Befehl auf den angegebenen Bots ausgeführt. Ohne diese Angabe wird der Befehl auf dem Bot ausgeführt, der den Befehl erhält. Mit anderen Worten: Wenn `status A` an Bot `B` gesendet wird ist es dasselbe wie wenn `status` an Bot `A` gesendet wird.

**Zugriff** eines Befehls definiert die **minimale** `EPermission` von `SteamUserPermissions`, die für die Verwendung des Befehls erforderlich ist - mit Ausnahme von `Owner` der als `SteamOwnerID` in der globalen Konfigurationsdatei (und damit höchste verfügbare Berechtigung) definiert ist.

Plural Argumente, wie `<Bots>`, `<Keys>` oder `<AppIDs>` bedeuten, dass der Befehl mehrere Argumente eines bestimmten Typs unterstützt, getrennt durch ein Komma. Zum Beispiel kann `status <Bots>` als `status MeinBot,MeinAndererBot,MeinKonto` verwendet werden. Dies führt dazu, dass der angegebene Befehl auf **alle Ziel-Bots** auf die gleiche Weise ausgeführt wird, wie wenn du `status` an jeden Bot in einem separaten Chat-Fenster senden würdest. Bitte beachte, dass nach `,` kein Leerzeichen steht.

ASF verwendet alle Whitespace-Zeichen als mögliche Trennzeichen für einen Befehl, wie Leerzeichen und Zeilenumbrüche. Das bedeutet, dass du zur Begrenzungen deiner Argumente keine Leerzeichen verwenden musst, du kannst auch jedes andere Whitespace-Zeichen (z.B. Tabulator oder Zeilenumbruch) verwenden.

ASF wird zusätzliche "Out-of-Range"-Argumente mit dem Plural-Typ des letzten "In-Range"-Arguments "verbinden". Das bedeutet, dass `redeem bot key1 key2 key3` für `redeem <Bots> <Keys>` genau so funktioniert wie `redeem bot key1,key2,key3`. Da ein Zeilenumbruch als Trennzeichen verwendet werden kann ermöglicht dir dies, `redeem bot` zu schreiben und dann eine Liste von Produktschlüsseln einzufügen, die durch ein beliebiges zulässiges Trennzeichen (z.B. Zeilenumbruch) oder dem Standard `,` ASF-Trennzeichen getrennt sind. Bedenke, dass dieser Trick nur für eine Befehlsvariante verwendet werden kann, die die meiste Anzahl von Argumenten verwendet (daher ist die Angabe von `<Bots>` in diesem Fall notwendig).

Wie du oben gelesen hast, wird ein Leerzeichen als Trennzeichen für einen Befehl verwendet, daher kann es nicht in Argumenten verwendet werden. Allerdings kann ASF auch, wie oben erwähnt, "Out-of-Range"-Argumente verbinden, was bedeutet, dass du tatsächlich ein Leerzeichen in Argumenten verwenden kannst, das als letztes für den angegebenen Befehl definiert ist. Zum Beispiel, `nickname bob Great Bob` wird den Benutzernamen des Bots `bob` richtig auf "Great Bob" setzen. Auf die gleiche Weise kannst du Namen, die Leerzeichen enthalten, im Befehl `owns` überprüfen.

Bitte bedenke, dass das Senden eines Befehls an den Gruppen-Chat wie ein Relais funktioniert - wenn du `redeem X` zu drei deiner Bots sendest, die zusammen mit dir im Gruppen-Chat sind, wird es das gleiche Ergebnis haben, wie wenn du `redeem X` an jeden einzelnen von ihnen privat senden würdest. In den meisten Fällen ist **dies nicht das, was du willst**, und stattdessen solltest du den Befehl `given bot` verwenden, der an **einen einzelnen Bot im privaten Fenster** gesendet wird. ASF unterstützt den Gruppen-Chat, da es in vielen Fällen eine nützliche Kommunikationsquelle sein kann, aber du solltest fast nie einen Befehl im Gruppen-Chat ausführen, wenn dort zwei oder mehr ASF-Bots aktiv sind, es sei denn, du verstehst das hier beschriebene ASF-Verhalten vollständig und du willst tatsächlich den gleichen Befehl an jeden einzelnen Bot weitergeben, der auf dich hört.

*Und selbst in diesem Fall solltest du stattdessen den privaten Chat mit der Syntax `<Bots>` verwenden.*

* * *

Für einige Befehle sind auch Aliase verfügbar, um dir Zeit beim tippen zu sparen:

| Befehl       | Alias |
| ------------ | ----- |
| `owns ASF`   | `oa`  |
| `status ASF` | `sa`  |
| `redeem`     | `r`   |
| `redeem^`    | `r^`  |

* * *

Es ist nicht erforderlich, ein zusätzliches Konto für die Ausführung von Befehlen über den Steam-Chat zu haben - Du kannst eine Gruppe erstellen, `SteamMasterClanID` richtig auf diese neu erstellte Gruppe einstellen und dir dann entweder über `SteamOwnerID` oder `SteamUserPermissions` deines eigenen Bots Zugriff geben. Auf diese Weise tritt der ASF-Bot (Du) der Gruppe bei und chattet mit der ausgewählten Gruppe und hört auf die Befehle von deinem eigenen Konto. Du kannst dem gleichen Gruppen-Chatroom beitreten, um dir selbst Befehle zu erteilen (Achtung: Wenn eine ASF-Instanz diesem Chatroom beigetreten ist wird diese ebenfalls sämtliche Befehle empfangen, die an diesen Chat gesendet werden, selbst wenn dieser anzeigt, dass nur dein Konto im Chat ist). Abgesehen davon kannst du auch **[IPC](https://github. com/JustArchiNET/ArchiSteamFarm/wiki/IPC-de-DE)** verwenden, aber die Lösung mittels Chatroom ist viel einfacher. Wenn du Zugang zu einem anderen Konto hast, dann ist die Verwendung dieses Kontos stattdessen noch einfacher.

* * *

### `<Bots>` Argument

`<Bots>` Argument ist eine spezielle Variante des Plural Arguments, da es neben der Annahme mehrerer Werte auch zusätzliche Funktionalität bietet.

Es gibt zunächst ein spezielles Schlüsselwort `ASF`, das als "alle Bots im Prozess" fungiert, so dass der Befehl `status ASF` gleich `status all,your,bots,listed,here` ist. Dies kann auch verwendet werden, um die Bots zu identifizieren, auf die du Zugriff hast, da das `ASF` Schlüsselwort, trotz der Ausrichtung auf alle Bots, nur von den Bots eine Antwort generiert, an die du den Befehl tatsächlich senden kannst.

Das Argument `<Bots>` unterstützt eine spezielle "Bereichs"-Syntax, die es dir ermöglicht, einen Bereich von Bots einfacher auszuwählen. Die allgemeine Syntax für `<Bots>` lautet in diesem Fall `ersterBot..letzterBot`. Wenn du zum Beispiel Bots mit den Namen `A, B, C, D, E, F` hast, kannst du `status B..E` ausführen, was in diesem Fall gleich `status B,C,D,E` ist. Bei Verwendung dieser Syntax verwendet ASF die alphabetische Sortierung, um festzustellen, welche Bots sich in dem von dir angegebenen Bereich befinden. Sowohl `ersterBot` als auch `letzterBot` müssen gültige Bot-Namen sein, die von ASF erkannt werden, da sonst die Bereichs-Syntax vollständig übersprungen wird.

Zusätzlich zur obigen Bereichs-Syntax unterstützt das Argument `<Bots>` auch den **[Regex](https://en.wikipedia.org/wiki/Regular_expression)** Abgleich. Du kannst Regex Muster aktivieren, indem du `r!<pattern>` als Bot-Namen verwendest, wobei `r!` der ASF-Aktivator für den Regex Abgleich ist, und `<pattern>` dein Regex-Muster ist. Ein Beispiel für einen regex-basierten Bot-Befehl wäre `status r!\d{3}`, der den Befehl `status` an Bots sendet, die einen aus 3 Ziffern bestehenden Namen haben (z.B. `123` und `981`). Zögere nicht einen Blick auf die **[Dokumentation](https://docs.microsoft.com/en-us/dotnet/standard/base-types/regular-expression-language-quick-reference)** zu werfen, um weitere Erklärungen und Beispiele für verfügbare Regex-Muster zu erhalten.

* * *

## `privacy` Einstellungen

`<Settings>` Argument akzeptiert **bis zu 7** verschiedene Optionen, getrennt wie üblich durch das standard ASF-Trennzeichen, das Komma. Diese sind, in der Reihenfolge:

| Argument | Name           | Child von  |
| -------- | -------------- | ---------- |
| 1        | Profile        |            |
| 2        | OwnedGames     | Profile    |
| 3        | Playtime       | OwnedGames |
| 4        | FriendsList    | Profile    |
| 5        | Inventory      | Profile    |
| 6        | InventoryGifts | Inventory  |
| 7        | Comments       | Profile    |

Für eine Beschreibung der oben genannten Felder besuche bitte die **[Steam Privatsphäre-Einstellungen](https://steamcommunity.com/my/edit/settings)**.

Für alle Argumente sind folgende Werte gültig:

| Wert | Name          |
| ---- | ------------- |
| 1    | `Private`     |
| 2    | `FriendsOnly` |
| 3    | `Public`      |

Du kannst entweder den case-insensitiven Namen oder den numerischen Wert verwenden. Argumente, die weggelassen wurden, werden standardmäßig auf `Private` gesetzt. Es ist wichtig, die Beziehung zwischen Kind und Eltern von oben genannten Argumenten zu beachten, da das Kind nie mehr offene Berechtigung haben kann als sein Elternteil. Zum Beispiel kannst du Spiele im Besitz **nicht** auf `Public` haben, während dein Profil auf `Private` steht.

### Beispiel

Wenn du **alle** Privatsphäre-Einstellungen deines Bots namens `Main` auf `Private` setzen möchtest, kannst du jede der folgenden Optionen verwenden:

    privacy Main 1
    privacy Main Private
    

Dies liegt daran, dass ASF automatisch alle anderen Einstellungen als `Private` ansieht, so dass es nicht notwendig ist, sie anzugeben. Wenn du dagegen alle Privatsphäre-Einstellungen auf `Public` setzen möchtest, dann musst du eine der folgenden Optionen verwenden:

    privacy Main 3,3,3,3,3,3,3
    privacy Main Public,Public,Public,Public,Public,Public,Public
    

Auf diese Weise kannst du auch einzelne Einstellungen so vornehmen, wie du willst:

    privacy Main Public,FriendsOnly,Private,Public,Public,Private,Public
    

Das oben genannte wird das Profil auf öffentlich setzen, eigene Spiele auf nur für Freunde, Spielzeit auf privat, Freundesliste auf öffentlich, Inventar auf öffentlich, Inventar Geschenke auf privat und Profil-Kommentare auf öffentlich. Das Gleiche kann man mit numerischen Werten erreichen, wenn man dies möchte.

Bedenke, dass ein Kind nie mehr offene Berechtigungen haben kann als sein Elternteil. Siehe hierzu die Argumentationsbeziehung für die verfügbaren Optionen.

* * *

## `redeem^` Modi

Der Befehl `redeem^` ermöglicht es dir, die Modi zu optimieren, die für ein einzelnes Einlöse-Szenario verwendet werden. Dazu wird temporär die `RedeemingPreferences` **[Bot-Konfigurationseigenschaft](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-de-DE#redeemingpreferences)** überschrieben.

`<Modes>` Argument akzeptiert mehrere Modus-Werte, die, wie üblich, durch ein Komma getrennt werden. Die verfügbaren Modus-Werte sind im Folgenden aufgeführt:

| Wert | Name                  | Beschreibung                                                                                                  |
| ---- | --------------------- | ------------------------------------------------------------------------------------------------------------- |
| FD   | ForceDistributing     | Erzwingt die Aktivierung der `Distributing` Einlöse-Präferenz                                                 |
| FF   | ForceForwarding       | Erzwingt die Aktivierung der `Forwarding` Einlöse-Präferenz                                                   |
| FKMG | ForceKeepMissingGames | Erzwingt die Aktivierung der `KeepMissingGames` Einlöse-Präferenz                                             |
| SD   | SkipDistributing      | Erzwingt die Deaktivierung der `Distributing` Einlöse-Präferenz                                               |
| SF   | SkipForwarding        | Erzwingt die Deaktivierung der `Forwarding` Einlöse-Präferenz                                                 |
| SI   | SkipInitial           | Überspringt die Produktschlüssel-Aktivierung beim ersten Bot                                                  |
| SKMG | SkipKeepMissingGames  | Erzwingt die Deaktivierung der `KeepMissingGames` Einlöse-Präferenz                                           |
| V    | Validate              | Überprüft die Produktschlüssel auf das richtige Format und überspringt automatisch ungültige Produktschlüssel |

Zum Beispiel möchten wir drei Produktschlüssel auf einem unserer Bots einlösen, der noch keine Spiele besitzt, aber nicht auf unserem `primary` Bot. Um das zu erreichen, können wir Folgendes nutzen:

`redeem^ primary FF,SI key1,key2,key3`

Es ist wichtig zu beachten, dass verbessertes Einlösen nur solche `RedeemingPreferences` überschreibt die sie **im Befehl angeben**. Beispielsweise wenn du angeschaltet hast `Distributing` in deinen `RedeemingPreferences` dann gibt es keinen Unterschied egal ob du `FD`Modus verwendest oder nicht, weil die Verteilung bereits aktiv ist ungeachtet, durch die`RedeemingPreferences` die du benutzt. Deshalb jede erzwungen aktivierte Überschreibung auch eine erzwungen Deaktivierte hat, kannst du selbst entscheiden ob du bevorzugst zu überschreiben deaktiviert mit aktiviert, oder umgekehrt.

* * *

## `input` Befehl

Der Input-Befehl kann nur im `Headless`-Modus verwendet werden, um die angegebenen Daten über **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-de-DE)** oder Steam-Chat einzugeben, wenn ASF ohne Unterstützung für Benutzerinteraktion läuft.

Die allgemeine Syntax ist `input <Bots> <Type> <Value>`.

`<Type>` ist Groß-/Kleinschreibung unabhängig und definiert einen von ASF unterstützten Eingabetyp. Derzeit unterstützt ASF folgende Typen:

| Typ                     | Beschreibung                                                                              |
| ----------------------- | ----------------------------------------------------------------------------------------- |
| DeviceID                | 2FA Geräte-Identifikation, wenn sie in `.maFile` fehlt.                                   |
| Login                   | `SteamLogin` Bot-Konfigurationseigenschaft, wenn sie in der Konfiguration fehlt.          |
| Password                | `SteamPassword` Bot-Konfigurationseigenschaft, wenn sie in der Konfiguration fehlt.       |
| SteamGuard              | Auth-Code, der an deine E-Mail gesendet wird, wenn du 2FA nicht benutzt.                  |
| SteamParentalCode       | ` SteamParentalCode ` Bot-Konfigurationseigenschaft, wenn sie in der Konfiguration fehlt. |
| TwoFactorAuthentication | 2FA-Code, der von deinem Handy generiert wurde, wenn du 2FA benutzt, aber nicht ASF 2FA.  |

`<Value>` ist der Wert, der für einen angegebenen Typ gesetzt werden soll. Derzeit sind alle Werte Zeichenketten.

### Beispiel

Lass uns annehmen, dass wir einen Bot haben, der durch SteamGuard (nicht im Zwei-Faktor-Modus) geschützt wird. Wir wollen diesen Bot starten während das Konfigurationsfeld `Headless` auf wahr gesetzt ist.

Um das zu tun müssen wir folgende Befehle ausführen:

`start MeinSteamGuardBot` -> Der Bot wird versuchen zu starten, was allerdings fehlschlagen wird, weil ein Authentifizierungscode benötigt wird. Dann wird er sich selbst stoppen, weil ASF im `Headless`-Modus läuft. Wir brauchen dies, damit das Steam-Netzwerk uns den Authentisierungscode an unsere E-Mail sendet - wenn es keinen Bedarf dafür gibt, würden wir diesen Schritt komplett überspringen.

`input MeinSteamGuardBot SteamGuard ABCDE` -> Wir setzen den `SteamGuard`-Input von `MeinSteamGuardBot` auf `ABCDE`. Natürlich sollte `ABCDE` der Code sein, den du in deiner E-Mail erhalten hast.

`start MeinSteamGuardBot` -> wir starten unseren (gestoppten) Bot wieder. Diesmal wird der Code benutzt, den wir im vorherigen Befehl gesetzt haben und der Bot loggt sich ein und löscht den Code aus seinem internen Speicher.

Auf dem selben Weg können wir auf Bots, die durch Zwei-Faktor-Authentifizierung geschützt sind (und nicht die 2FA von ASF verwenden), zugreifen und andere Dinge während der Laufzeit tun.