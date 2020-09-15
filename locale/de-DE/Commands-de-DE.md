# Befehle

ASF unterstützt eine Vielzahl von Befehlen, mit denen das Verhalten der Prozess- und Bot-Instanzen gesteuert werden kann.

Die folgenden Befehle können auf verschiedene Weise an den Bot gesendet werden:

- Durch die interaktive ASF-Konsole
- Durch den privaten Steam-Chat / Gruppen-Chat
- Durch unsere **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-de-DE)** Schnittstelle

Bedenke, dass die ASF-Interaktion von dir verlangt, dass du für den Befehl gemäß den ASF-Berechtigungen berechtigt bist. Weitere Informationen findest du in den Konfigurationseigenschaften `SteamUserPermissions` und `SteamOwnerID`.

Befehle die über den Steam-Chat ausgeführt werden, sind von der **[globalen Konfigurationseigenschaft](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-de-DE#commandprefix)** `CommandPrefix` betroffen, welche standardmäßig `!` ist. Das bedeutet, dass du beispielsweise für die Ausführung von dem Befehl `status` tatsächlich `!status` (oder einen benutzerdefinierten `CommandPrefix` deiner Wahl) verwenden musst. `CommandPrefix` ist bei Verwendung von IPC oder der Konsole nicht zwingend erforderlich und kann weggelassen werden.

* * *

### Interaktive Konsole

Beginnend mit V4.0.0.0.9 unterstützt ASF eine interaktive Konsole, die durch die Einrichtung der [**`SteamOwnerID`**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-de-DE#steamownerid) Eigenschaft aktiviert werden kann. Anschließend drückst du einfach die `c` Taste um den Befehlsmodus zu aktivieren, gibst deinen Befehl ein und bestätigst mit der Eingabetaste.

![Screenshot](https://i.imgur.com/bH5Gtjq.png)

Die interaktive Konsole ist nicht im [**`Headless`**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-de-DE#headless) Modus verfügbar.

* * *

### Steam-Chat

Du kannst den Befehl zum angegebenen ASF-Bot auch über den Steam-Chat ausführen. Natürlich kannst du nicht direkt mit dir selbst schreiben, deshalb brauchst du mindestens ein weiteres Bot-Konto, wenn du Befehle ausführen willst, die auf dein Haupt-Konto abzielen.

![Screenshot](https://i.imgur.com/IvFRJ5S.png)

In ähnlicher Weise kannst du auch den Gruppen-Chat einer bestimmten Steam-Gruppe verwenden. Beachte, dass diese Option die korrekt eingestellte Eigenschaft `SteamMasterClanID` erfordert. In diesem Fall wird der Bot auch im Gruppen-Chat auf Befehle warten (und sich bei Bedarf diesem anschließen). Dies kann auch für "Selbstgespräche" genutzt werden, da diese Variante kein dediziertes Bot-Konto erfordert, im Gegensatz zum privaten Chat. Du kannst einfach die `SteamMasterClanID`-Eigenschaft auf deine neu erstellte Gruppe setzen und dir dann entweder über `SteamOwnerID` oder `SteamUserPermissions` Zugriff auf deinen eigenen Bot gewähren. Auf diese Weise tritt der ASF-Bot (Du) der Gruppe bei und chattet mit der ausgewählten Gruppe und hört auf die Befehle von deinem eigenen Konto. Du kannst dem gleichen Gruppen-Chatroom beitreten, um dir selbst Befehle zu erteilen (Achtung: Wenn eine ASF-Instanz diesem Chatroom beigetreten ist wird diese ebenfalls sämtliche Befehle empfangen, die an diesen Chat gesendet werden, selbst wenn dieser anzeigt, dass nur dein Konto im Chat ist).

Bitte bedenke, dass das Senden eines Befehls an den Gruppen-Chat wie ein Relais funktioniert. Wenn du `redeem X` zu drei deiner Bots sendest, die zusammen mit dir im Gruppen-Chat sind, wird es das gleiche Ergebnis haben, wie wenn du `redeem X` an jeden einzelnen von ihnen privat senden würdest. In den meisten Fällen ist **dies nicht das, was du willst**, und stattdessen solltest du den Befehl `given bot` verwenden, der an **einen einzelnen Bot im privaten Fenster** gesendet wird. ASF unterstützt den Gruppen-Chat, da es in vielen Fällen eine nützliche Quelle für die Kommunikation mit deinem einzigen Bot sein kann, aber du solltest fast nie einen Befehl im Gruppen-Chat ausführen, wenn dort zwei oder mehr ASF-Bots sitzen, es sei denn, du verstehst das hier beschriebene ASF-Verhalten vollständig und du willst tatsächlich den gleichen Befehl an jeden einzelnen Bot weitergeben, der auf dich hört.

*Und selbst in diesem Fall solltest du stattdessen den privaten Chat mit dem Syntax `[Bots]` benutzen.*

* * *

### IPC

Die fortschrittlichste und flexibelste Art der Befehlsausführung. Perfekt geeignet für die Benutzerinteraktion (ASF-ui), für Drittanbieter-Programme oder Skripte (ASF-API). Diese Variante erfordert, dass ASF im `IPC` Modus ausgeführt wird und einen Client der Befehle über die **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-de-DE)** Schnittstelle ausführt.

![Screenshot](https://raw.githubusercontent.com/JustArchiNET/ASF-ui/master/.github/previews/commands.png)

* * *

## Befehle

Verschlüsselt die Zeichenkette mit dem bereitgestellten kryptographischen Mechanismus - ausführlicher weiter **unten erklärt<0>.</td> </tr> 

</tbody> </table> 

* * *

### Anmerkungen

Bei Befehlen selbst ist die Groß- und Kleinschreibung egal, aber bei deren Argumenten (wie zum Beispiel Bot-Namen) muss die Groß- und Kleinschreibung beachtet werden.

Das `[Bots]` Argument ist in allen Befehlen optional. Wenn verwendet, wird der Befehl auf den angegebenen Bots ausgeführt. Ohne diese Angabe wird der Befehl auf dem Bot ausgeführt, der den Befehl erhält. Mit anderen Worten: Wenn `status A` an Bot `B` gesendet wird ist es dasselbe wie wenn `status` an Bot `A` gesendet wird. Bot `B` dient in diesem Fall nur als Proxy. Dies kann auch zum Senden von Befehlen an Bots verwendet werden, die andernfalls nicht verfügbar sind. Zum Beispiel das Starten angehaltener Bots oder die Ausführung von Aktionen auf Ihrem Hauptkonto (welches Sie für die Ausführung der Befehle verwenden).

**Zugriff** eines Befehls definiert die **minimale** `EPermission` von `SteamUserPermissions`, die für die Verwendung des Befehls erforderlich ist - mit Ausnahme von `Owner` der als `SteamOwnerID` in der globalen Konfigurationsdatei (und damit höchste verfügbare Berechtigung) definiert ist.

Mehrere Argumente, wie `[Bots]`, `<Keys>` oder `<AppIDs>` bedeuten, dass der Befehl mehrere, mit Komma getrennte, Argumente eines gegebenen Typs unterstützt. `status [Bots]` kann zum Beispiel als `status MyBot,MyOtherBot,Primär` verwendet werden. Dies führt dazu, dass der angegebene Befehl auf **alle Ziel-Bots** auf die gleiche Weise ausgeführt wird, wie wenn du `status` an jeden Bot in einem separaten Chat-Fenster senden würdest. Bitte beachte, dass nach `,` kein Leerzeichen steht.

ASF verwendet alle Whitespace-Zeichen als mögliche Trennzeichen für einen Befehl, wie Leerzeichen und Zeilenumbrüche. Das bedeutet, dass du zur Begrenzungen deiner Argumente keine Leerzeichen verwenden musst, du kannst auch jedes andere Whitespace-Zeichen (z.B. Tabulator oder Zeilenumbruch) verwenden.

ASF wird zusätzliche "Out-of-Range"-Argumente mit dem Plural-Typ des letzten "In-Range"-Arguments "verbinden". Dies bedeutet, dass `redeem bot key1 key2 key3` für `redeem [Bots] <Keys>` genauso funktionieren wird, wie `redeem bot key1,key2,key3`. Da ein Zeilenumbruch als Trennzeichen verwendet werden kann ermöglicht dir dies, `redeem bot` zu schreiben und dann eine Liste von Produktschlüsseln einzufügen, die durch ein beliebiges zulässiges Trennzeichen (z.B. Zeilenumbruch) oder dem Standard `,` ASF-Trennzeichen getrennt sind. Bedenke, dass dieser Trick nur für eine Befehlsvariante verwendet werden kann, die die meiste Anzahl von Argumenten verwendet (daher ist die Angabe von `[Bots]` in diesem Fall notwendig).

Wie du oben gelesen hast, wird ein Leerzeichen als Trennzeichen für einen Befehl verwendet, daher kann es nicht in Argumenten verwendet werden. Allerdings kann ASF auch, wie oben erwähnt, "Out-of-Range"-Argumente verbinden, was bedeutet, dass du tatsächlich ein Leerzeichen in Argumenten verwenden kannst, das als letztes für den angegebenen Befehl definiert ist. Zum Beispiel, `nickname bob Great Bob` wird den Benutzernamen des Bots `bob` richtig auf "Great Bob" setzen. Auf die gleiche Weise kannst du Namen, die Leerzeichen enthalten, im Befehl `owns` überprüfen.

* * *

Für einige Befehle sind auch Aliase verfügbar, um dir Zeit beim tippen zu sparen:

| Befehl       | Alias |
| ------------ | ----- |
| `owns ASF`   | `oa`  |
| `status ASF` | `sa`  |
| `redeem`     | `r`   |
| `redeem^`    | `r^`  |

* * *

### `[Bots]` Argument

`[Bots]` Argument ist eine spezielle Variante des Plural Arguments, da es neben der Annahme mehrerer Werte auch zusätzliche Funktionalität bietet.

Es gibt zunächst ein spezielles Schlüsselwort `ASF`, das als "alle Bots im Prozess" fungiert, so dass der Befehl `status ASF` gleich `status all,your,bots,listed,here` ist. Dies kann auch verwendet werden, um die Bots zu identifizieren, auf die du Zugriff hast, da das `ASF` Schlüsselwort, trotz der Ausrichtung auf alle Bots, nur von den Bots eine Antwort generiert, an die du den Befehl tatsächlich senden kannst.

Das Argument `[Bots]` unterstützt eine speziellen "range"-Syntax, der es dir ermöglicht, einen Reihe von Bots einfacher auszuwählen. Der allgemeine Syntax für `[Bots]` ist in diesem Fall `ersterBot..letzterBot`. Wenn du zum Beispiel Bots mit den Namen `A, B, C, D, E, F` hast, kannst du `status B..E` ausführen, was in diesem Fall gleich `status B,C,D,E` ist. Bei Verwendung dieser Syntax verwendet ASF die alphabetische Sortierung, um festzustellen, welche Bots sich in dem von dir angegebenen Bereich befinden. Sowohl `ersterBot` als auch `letzterBot` müssen gültige Bot-Namen sein, die von ASF erkannt werden, da sonst die Bereichs-Syntax vollständig übersprungen wird.

Zusätzlich zum obigen range-syntax unterstützt das `[Bots]` Argument auch **[regex](https://en.wikipedia.org/wiki/Regular_expression)** Übereinstimmung. Du kannst Regex Muster aktivieren, indem du `r!<pattern>` als Bot-Namen verwendest, wobei `r!` der ASF-Aktivator für den Regex Abgleich ist, und `<pattern>` dein Regex-Muster ist. Ein Beispiel für einen regex-basierten Bot-Befehl wäre `status r!\d{3}`, der den Befehl `status` an Bots sendet, die einen aus 3 Ziffern bestehenden Namen haben (z.B. `123` und `981`). Zögere nicht einen Blick auf die **[Dokumentation](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference)** zu werfen, um weitere Erklärungen und Beispiele für verfügbare Regex-Muster zu erhalten.

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

```text
privacy Main 1
privacy Main Private
```

Dies liegt daran, dass ASF automatisch alle anderen Einstellungen als `Private` ansieht, so dass es nicht notwendig ist, sie anzugeben. Wenn du dagegen alle Privatsphäre-Einstellungen auf `Public` setzen möchtest, dann musst du eine der folgenden Optionen verwenden:

```text
privacy Main 3,3,3,3,3,3,3
privacy Main Public,Public,Public,Public,Public,Public,Public
```

Auf diese Weise kannst du auch einzelne Einstellungen so vornehmen, wie du willst:

```text
privacy Main Public,FriendsOnly,Private,Public,Public,Private,Public
```

Das oben genannte wird das Profil auf öffentlich setzen, eigene Spiele auf nur für Freunde, Spielzeit auf privat, Freundesliste auf öffentlich, Inventar auf öffentlich, Inventar Geschenke auf privat und Profil-Kommentare auf öffentlich. Das Gleiche kann man mit numerischen Werten erreichen, wenn man dies möchte.

Bedenke, dass ein Kind nie mehr offene Berechtigungen haben kann als sein Elternteil. Siehe hierzu die Argumentationsbeziehung für die verfügbaren Optionen.

* * *

## `addlicense` Lizenzen

`addlicense` Befehl unterstützt zwei verschiedene Lizenztypen, diese sind:

| Typ   | Alias | Beispiel     | Beschreibung                                                                      |
| ----- | ----- | ------------ | --------------------------------------------------------------------------------- |
| `app` | `a`   | `app/292030` | Spiel bestimmt durch seine einzigartige `appID`.                                  |
| `sub` | `s`   | `sub/47807`  | Paket mit einem oder mehreren Spielen, bestimmt durch seine einzigartige `subID`. |

Die Differenzierung ist wichtig, da ASF die Steam Netzwerk Aktivierung für Apps und die Steam Shop Aktivierung für Pakete verwenden wird. Diese beiden sind nicht miteinander kompatibel, normalerweise werden Apps für Spiele mit kostenlosen Wochenenden und dauerhaft kostenlose Spiele verwendet, und Packages für alles andere.

Wir empfehlen, die Art jedes Eintrags explizit zu definieren, um zweideutige Ergebnisse zu vermeiden, aber für die Abwärtskompatibilität, wird ASF, wenn Sie einen ungültigen Typ angeben oder ihn ganz weglassen, davon ausgehen, dass Sie `sub` anfordern. Sie können auch eine oder mehrere Lizenzen gleichzeitig abfragen, indem Sie die Standard ASF `,` Trennzeichen verwenden.

Beispiel für einen vollständigen Befehl:

```text
addlicense ASF app/292030,sub/47807
```

* * *

## `owns` Spiele

Der `owns` Befehl unterstützt verschiedene Spielarten die für das `<games>` Argument genutzt werden können, diese sind:

| Typ     | Alias | Beispiel         | Beschreibung                                                                                                                                                                                                                                                                                                                   |
| ------- | ----- | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `app`   | `a`   | `app/292030`     | Spiel bestimmt durch seine einzigartige `appID`.                                                                                                                                                                                                                                                                               |
| `sub`   | `s`   | `sub/47807`      | Paket mit einem oder mehreren Spielen, bestimmt durch seine einzigartige `subID`.                                                                                                                                                                                                                                              |
| `regex` | `r`   | `regex/^\d{4}:` | **[Regex](https://en.wikipedia.org/wiki/Regular_expression)** der sich auf den Namen des Spiels bezieht, Groß- und Kleinschreibung wird berücksichtigt. Siehe **[docs](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference)** für den vollständige Syntax und weitere Beispiele. |
| `name`  | `n`   | `name/Witcher`   | Teil des Namens des Spiels, Groß- und Kleinschreibung wird nicht berücksichtigt.                                                                                                                                                                                                                                               |

Wir empfehlen, die Art jedes Eintrags explizit zu definieren, um zweideutige Ergebnisse zu vermeiden, aber für die Abwärtskompatibilität, wird ASF wenn Sie einen ungültigen Typ angeben oder ihn komplett weglassen davon ausgehen, dass Sie `app` verlangen, wenn Ihre Eingabe eine Nummer ist, und `name` falls nicht. Sie können auch eines oder mehrere Spiele gleichzeitig abfragen, indem sie die Standard ASF `,` Trennzeichen verwenden.

Beispiel für einen vollständigen Befehl:

```text
owns ASF app/292030,name/Witcher
```

* * *

## `redeem^` Modi

Der Befehl `redeem^` ermöglicht es dir, die Modi zu optimieren, die für ein einzelnes Einlöse-Szenario verwendet werden. Dazu wird temporär die `RedeemingPreferences` **[Bot-Konfigurationseigenschaft](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-de-DE#redeemingpreferences)** überschrieben.

`<Modes>` Argument akzeptiert mehrere Modus-Werte, die, wie üblich, durch ein Komma getrennt werden. Die verfügbaren Modus-Werte sind im Folgenden aufgeführt:

| Wert | Name                  | Beschreibung                                                                                                  |
| ---- | --------------------- | ------------------------------------------------------------------------------------------------------------- |
| FAWK | ForceAssumeWalletKey  | Erzwingt `AssumeWalletKeyOnBadActivationCode` Einlöseeinstellungen zu aktivieren                              |
| FD   | ForceDistributing     | Erzwingt die Aktivierung der `Distributing` Einlöse-Präferenz                                                 |
| FF   | ForceForwarding       | Erzwingt die Aktivierung der `Forwarding` Einlöse-Präferenz                                                   |
| FKMG | ForceKeepMissingGames | Erzwingt die Aktivierung der `KeepMissingGames` Einlöse-Präferenz                                             |
| SAWK | SkipAssumeWalletKey   | Erzwingt `AssumeWalletKeyOnBadActivationCode` Einlöseeinstellungen zu deaktivieren                            |
| SD   | SkipDistributing      | Erzwingt die Deaktivierung der `Distributing` Einlöse-Präferenz                                               |
| SF   | SkipForwarding        | Erzwingt die Deaktivierung der `Forwarding` Einlöse-Präferenz                                                 |
| SI   | SkipInitial           | Überspringt die Produktschlüssel-Aktivierung beim ersten Bot                                                  |
| SKMG | SkipKeepMissingGames  | Erzwingt die Deaktivierung der `KeepMissingGames` Einlöse-Präferenz                                           |
| V    | Validate              | Überprüft die Produktschlüssel auf das richtige Format und überspringt automatisch ungültige Produktschlüssel |

Zum Beispiel möchten wir drei Produktschlüssel auf einem unserer Bots einlösen, der noch keine Spiele besitzt, aber nicht auf unserem `primary` Bot. Um das zu erreichen, können wir Folgendes nutzen:

`redeem^ primary FF,SI key1,key2,key3`

Es ist wichtig zu beachten, dass fortgeschrittenes Einlösen nur die `RedeemingPreferences` überschreibt, die du **im Befehl angeben hast**. Wenn du zum Beispiel `Distributing` in deinen `RedeemingPreferences` aktiviert hast, dann macht es keinen Unterschied, ob du den `FD` Modus verwendest oder nicht, weil die Verteilung trotzdem bereits aktiv ist, aufgrund der `RedeemingPreferences` die du verwendest. Deshalb hat jede zwangsweise aktivierte Überschreibung auch eine zwangsweise deaktivierte. Du kannst selbst entscheiden, ob du es vorziehst deaktivierte mit aktiviert zu überschreiben oder umgekehrt.

* * *

## `encrypt` Befehl

Mit dem Encrypt-Befehl kannst du beliebige Zeichenketten mit den ASF-Verschlüsselungsmechanismen verschlüsseln. `<cryptoMethod>` muss einer der folgenden Werte sein:

| Wert | Name                          |
| ---- | ----------------------------- |
| 0    | `PlainText`                   |
| 1    | `AES`                         |
| 2    | `ProtectedDataForCurrentUser` |

Du kannst entweder einen Namen ohne Berücksichtigung der Groß-/Kleinschreibung oder einen numerischen Wert verwenden. Die Verschlüsselungsmechanismen werden im Abschnitt **[Sicherheit](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security)** erläutert. Dieser Befehl ist nützlich, wenn du im Voraus verschlüsselte Details erzeugen möchtest, z. B. um zu vermeiden, dass du zuerst dein `PlainText`-Passwort in die Konfiguration eingibst und dann den Befehl `Passwort` verwendest. Wir empfehlen, diesen Befehl über sichere Kanäle zu verwenden (ASF-Konsole oder IPC-Schnittstelle, für die es auch einen dedizierten API-Endpunkt gibt), da sonst sensible Details von verschiedenen Dritten protokolliert werden könnten (z. B. Chat-Nachrichten, die von Steam-Servern protokolliert werden).

* * *

## `input` Befehl

Der Input-Befehl kann nur im `Headless`-Modus verwendet werden, um die angegebenen Daten über **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-de-DE)** oder Steam-Chat einzugeben, wenn ASF ohne Unterstützung für Benutzerinteraktion läuft.

Der Allgemeine Syntax ist `Input [Bots] <Type> <Value>`.

`<Type>` ist Groß-/Kleinschreibung unabhängig und definiert einen von ASF unterstützten Eingabetyp. Derzeit unterstützt ASF folgende Typen:

| Typ                     | Beschreibung                                                                              |
| ----------------------- | ----------------------------------------------------------------------------------------- |
| Login                   | `SteamLogin` Bot-Konfigurationseigenschaft, wenn sie in der Konfiguration fehlt.          |
| Passwort                | `SteamPassword` Bot-Konfigurationseigenschaft, wenn sie in der Konfiguration fehlt.       |
| SteamGuard              | Auth-Code, der an deine E-Mail gesendet wird, wenn du 2FA nicht benutzt.                  |
| SteamParentalCode       | ` SteamParentalCode ` Bot-Konfigurationseigenschaft, wenn sie in der Konfiguration fehlt. |
| TwoFactorAuthentication | 2FA-Code, der von deinem Handy generiert wurde, wenn du 2FA benutzt, aber nicht ASF 2FA.  |

`<Value>` ist der Wert, der für einen angegebenen Typ gesetzt werden soll. Derzeit sind alle Werte Zeichenketten.

### Beispiel

Lass uns annehmen, dass wir einen Bot haben, der durch SteamGuard (nicht im Zwei-Faktor-Modus) geschützt wird. Wir wollen diesen Bot starten während das Konfigurationsfeld `Headless` auf wahr gesetzt ist.

Um das zu tun, müssen wir folgende Befehle ausführen:

`start MeinSteamGuardBot` -> Der Bot wird versuchen zu starten, was allerdings fehlschlagen wird, weil ein Authentifizierungscode benötigt wird. Dann wird er sich selbst stoppen, weil ASF im `Headless`-Modus läuft. Wir brauchen dies, damit das Steam-Netzwerk uns den Authentisierungscode an unsere E-Mail sendet - wenn es keinen Bedarf dafür gibt, würden wir diesen Schritt komplett überspringen.

`input MeinSteamGuardBot SteamGuard ABCDE` -> Wir setzen den `SteamGuard`-Input von `MeinSteamGuardBot` auf `ABCDE`. Natürlich sollte `ABCDE` der Code sein, den du in deiner E-Mail erhalten hast.

`start MeinSteamGuardBot` -> wir starten unseren (gestoppten) Bot wieder. Diesmal wird der Code benutzt, den wir im vorherigen Befehl gesetzt haben und der Bot loggt sich ein und löscht den Code aus seinem internen Speicher.

Auf dem selben Weg können wir auf Bots, die durch Zwei-Faktor-Authentifizierung geschützt sind (und nicht die 2FA von ASF verwenden), zugreifen und andere Dinge während der Laufzeit tun.