# Befehle

ASF unterstützt eine Vielzahl von Befehlen, die einem die Kontrolle über das Verhalten des Prozesses und der Botinstanzen geben.

Die unten angeführten Befehle können über drei verschiedene Wege an einen Bot gesendet werden:

- Über den privaten Steam-Chat
- Über einen Steam-Gruppen-Chat
- Über **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)**

Bitte vergiss nicht, dass jegliche ASF-Interaktion verlangt, dass du gemäß deinen ASF-Einstellungen über die Berechtigungen für den Befehl verfügen musst. Für mehr Informationen lies bitte die Einträge zu den Config-Feldern `SteamUserPermissions` und `SteamOwnerID` durch.

Alle unten angeführten Befehle werden durch das **[globale Konfigurationsfeld](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#global-config)** `CommandPrefix` beeinflusst, welches standardmäsig `!` ist. Das heißt, dass du um zum Beispiel den `status`-Befehl auszuführen `!status` (oder dein entsprechendes `CommandPrefix`) schreiben solltest.

* * *

### Privater Steam Chat

... ist definitiv die einfachste Methode mit ASF zu interagieren - Sende dazu einfach den Befehl an einen ASF-Bot der zur Zeit im ASF-Prozess läuft. Logischerweise kannst du das nicht machen, wenn du ASF mit nur einem einzigen Bot-Account laufen lässt.

![Screenshot](https://i.imgur.com/PPxx7qV.png)

* * *

### Steam Gruppenchat

Sehr ähnlich zur oben genannten Möglichkeit, allerdings diesmal über den Gruppenchat einer vorgegebenen Steam-Gruppe. Beachte, dass diese Option die korrekt eingestellte Eigenschaft `SteamMasterClanID` erfordert, in diesem Fall wird der Bot auch im Gruppen-Chat auf Befehle warten (und sich bei Bedarf diesem anschließen). Dies kann auch für "Selbstgespräche" genutzt werden, da es kein dediziertes Bot-Konto erfordert. Du willst diese Methode höchstwahrscheinlich nicht für mehr Bots als 1 verwenden.

* * *

### IPC

Die fortschrittlichste und flexibelste Art der Befehlsausführung, perfekt für die Benutzerinteraktion (ASF-ui) sowie für Drittanbieter-Programme oder Skripte (ASF-API), erfordert, dass ASF im `IPC` Modus ausgeführt wird und einen Client der Befehle über die **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** Schnittstelle ausführt.

![Screenshot](https://i.imgur.com/pzKE4EJ.png)

* * *

## Befehle

| Befehl                                                                     | Zugriff         | Beschreibung                                                                                                                                                                                          |
| -------------------------------------------------------------------------- | --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `2fa <Bots>`                                                         | `Master`        | Generiert temporäre **[2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)**-Codes für gegebene Bot-Instanzen.                                                        |
| `2fano <Bots>`                                                       | `Master`        | Lehnt alle ausstehenden **[2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)**-Bestätigungen für gegebene Bot-Instanzen ab.                                         |
| `2faok <Bots>`                                                       | `Master`        | Akzeptiert alle ausstehenden **[2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)**-Bestätigungen für gegebene Bot-Instanzen.                                       |
| `addlicense <Bots> <GameIDs>`                                  | `Operator`      | Aktiviert gegebene `appIDs` (Steam-Netzwerk) oder `subIDs` (Steam-Shop) auf gegebenen Bot-Instanzen (nur kostenlose Spiele).                                                                          |
| `bl <Bots>`                                                          | `Master`        | Listet Benutzer aus dem Handelsmodul der gegebenen Bot-Instanzen auf, die auf der schwarzen Liste stehen.                                                                                             |
| `bladd <Bots> <SteamIDs64>`                                    | `Master`        | Setzt gegebene `steamIDs` auf die Schwarze Liste des Handelsmodul der gegebenen Bot-Instanzen.                                                                                                        |
| `blrm <Bots> <SteamIDs64>`                                     | `Master`        | Entfernt gegebene `steamIDs` von der schwarzen Liste des Handelsmoduls der gegebenen Bot-Instanzen.                                                                                                   |
| `exit`                                                                     | `Owner`         | Stoppt den kompletten ASF-Prozess.                                                                                                                                                                    |
| `farm <Bots>`                                                        | `Master`        | Startet das Karten-Sammelmodul für gegebene Bot-Instanzen neu.                                                                                                                                        |
| `help`                                                                     | `FamilySharing` | Zeigt Hilfe an (Link zu dieser Seite).                                                                                                                                                                |
| `input <Bots> <Type> <Value>`                            | `Master`        | Setzt den gegebenen Eingabetyp auf den gegebenen Wert für gegebene Bot-Instanzen. Funktioniert nur im `Headless`-Modus, genauer erklärt **[unten](#input-command)**.                                  |
| `ib <Bots>`                                                          | `Master`        | Listet Anwendungen auf, die vom automatischen Sammeln von gegebenen Bot-Instanzen ausgeschlossen sind.                                                                                                |
| `ibadd <Bots> <AppIDs>`                                        | `Master`        | Adds given `appIDs` to apps blacklisted from automatic idling of given bot instances.                                                                                                                 |
| `ibrm <Bots> <AppIDs>`                                         | `Master`        | Removes given `appIDs` from apps blacklisted from automatic idling of given bot instances.                                                                                                            |
| `iq <Bots>`                                                          | `Master`        | Lists priority idling queue of given bot instances.                                                                                                                                                   |
| `iqadd <Bots> <AppIDs>`                                        | `Master`        | Adds given `appIDs` to priority idling queue of given bot instances.                                                                                                                                  |
| `iqrm <Bots> <AppIDs>`                                         | `Master`        | Removes given `appIDs` from priority idling queue of given bot instances.                                                                                                                             |
| `loot <Bots>`                                                        | `Master`        | Sends all `LootableTypes` Steam community items of given bot instances to `Master` user defined in their `SteamUserPermissions` (with lowest steamID if more than one).                               |
| `loot@ <Bots> <RealAppIDs>`                                    | `Master`        | Sends all `LootableTypes` Steam community items matching given `RealAppIDs` of given bot instances to `Master` user defined in their `SteamUserPermissions` (with lowest steamID if more than one).   |
| `loot^ <Bots> <AppID> <ContextID>`                       | `Master`        | Sends all Steam items from given `AppID` in `ContextID` of given bot instances to `Master` user defined in their `SteamUserPermissions` (with lowest steamID if more than one).                       |
| `loot& <Bots>`                                                   | `Master`        | Switches looting of given bot instances between enabled/disabled state.                                                                                                                               |
| `nickname <Bots> <Nickname>`                                   | `Master`        | Changes Steam nickname of given bot instances to given `nickname`.                                                                                                                                    |
| `owns <Bots> <AppIDsOrGameNames>`                              | `Operator`      | Checks if given bot instances already own given `appIDs` and/or `gameNames` (can be part of the game's name). It can also be `*` to show all games available.                                         |
| `password <Bots>`                                                    | `Master`        | Prints encrypted password of given bot instances (in use with `PasswordFormat`).                                                                                                                      |
| `pause <Bots>`                                                       | `Operator`      | Permanently pauses automatic cards farming module of given bot instances. ASF will not attempt to farm current account in this session, unless you manually `resume` it, or restart the process.      |
| `pause~ <Bots>`                                                      | `FamilySharing` | Temporarily pauses automatic cards farming module of given bot instances. Farming will be automatically resumed on the next playing event, or bot disconnect. You can `resume` farming to unpause it. |
| `pause& <Bots> <Seconds>`                                  | `Operator`      | Temporarily pauses automatic cards farming module of given bot instances for given amount of `seconds`. After delay, cards farming module is automatically resumed.                                   |
| `play <Bots> <AppIDs,GameName>`                                | `Master`        | Switches to manual farming - launches given `AppIDs` on given bot instances, optionally also with custom `GameName`. Use `resume` for returning to automatic farming.                                 |
| `privacy <Bots> <Settings>`                                    | `Master`        | Changes **[Steam privacy settings](https://steamcommunity.com/my/edit/settings)** of given bot instances, to appropriately selected options explained **[below](#privacy-settings)**.                 |
| `redeem <Bots> <Keys>`                                         | `Operator`      | Redeems given `cd-keys` on given bot instances.                                                                                                                                                       |
| `redeem^ <Bots> <Modes> <Keys>`                          | `Operator`      | Redeems given `cd-keys` on given bot instances, using given `modes` explained **[below](#redeem-modes)**.                                                                                             |
| `rejoinchat <Bots>`                                                  | `Operator`      | Forces given bot instances to rejoin their `SteamMasterClanID` group chat.                                                                                                                            |
| `restart`                                                                  | `Besitzer`      | Startet den ASF-Prozess neu.                                                                                                                                                                          |
| `resume <Bots>`                                                      | `FamilySharing` | Resumes automatic farming of given bot instances. Also see `pause`, `play`.                                                                                                                           |
| `start <Bots>`                                                       | `Master`        | Starts given bot instances.                                                                                                                                                                           |
| `stats`                                                                    | `Besitzer`      | Prints process statistics, such as managed memory usage.                                                                                                                                              |
| `status <Bots>`                                                      | `FamilySharing` | Prints status of given bot instances.                                                                                                                                                                 |
| `stop <Bots>`                                                        | `Master`        | Stops given bot instances.                                                                                                                                                                            |
| `transfer <Bots> <TargetBot>`                                  | `Master`        | Sends all `TransferableTypes` Steam community items from given bot instances to target bot.                                                                                                           |
| `transfer@ <Bots> <RealAppIDs> <TargetBot>`              | `Master`        | Sends all `TransferableTypes` Steam community items matching given `RealAppIDs` from given bot instances to target bot.                                                                               |
| `transfer^ <Bots> <AppID> <ContextID> <TargetBot>` | `Master`        | Sends all Steam items from given `AppID` in `ContextID` of given bot instances to target bot.                                                                                                         |
| `unpack <Bots>`                                                      | `Master`        | Unpacks all booster packs stored in the inventory of given bot instances.                                                                                                                             |
| `update`                                                                   | `Besitzer`      | Checks GitHub for ASF updates (this is done automatically every 24 hours if `AutoUpdates`).                                                                                                           |
| `version`                                                                  | `FamilySharing` | Gibt die ASF-Version an.                                                                                                                                                                              |

* * *

### Notizen

Bei Befehlen selbst ist die Groß- und Kleinschreibung egal, aber bei den Argumenten dieser (wie zum Beispiel Botnamen) ist sich an entsprechende Groß- und Kleinschreibung zu halten.

Das Argument `<Bots>` ist in allen Befehlen optional. Wenn spezifiziert wird der Befehl auf den angegebenen Bots ausgeführt. Ohne diese Angabe wird der Befehl auf dem aktuellen Bot ausgeführt der den Befehl erhält. Mit anderen Worten, `status A` der an Bot `B` gesendet wird ist dasselbe wie `status` an Bot `A` zu senden.

**Zugriff** des Befehls definiert **minimale** `EPermission` von `SteamUserPermissions` die für die Verwendung des Befehls erforderlich ist - mit Ausnahme von `Owner` das als `SteamOwnerID` in der globalen Konfigurationsdatei (und höchste verfügbare Berechtigung) definiert ist.

Plural arguments, such as `<Bots>`, `<Keys>` or `<AppIDs>` mean that command supports multiple arguments of given type, separated by a comma. For example, `status <Bots>` can be used as `status MyBot,MyOtherBot,Primary`. This will cause given command to be executed on **all target bots** in the same way as you'd send `status` to each bot in a separate chat window. Please note that there is no space after `,`.

ASF uses all whitespace characters as possible delimiters for a command, such as space and newline characters. This means that you don't have to use space for delimiting your arguments, you can as well use any other whitespace character (such as tab or new line).

ASF will "join" extra out-of-range arguments to plural type of the last in-range argument. This means that `redeem bot key1 key2 key3` for `redeem <Bots> <Keys>` will work exactly the same as `redeem bot key1,key2,key3`. Together with accepting newline as command delimiter, this makes it possible for you to write `redeem bot` then paste a list of keys separated by any acceptable delimiter character (such as newline), or standard `,` ASF delimiter. Keep in mind that this trick can be used only for command variant that uses the most amount of arguments (so specifying `<Bots>` is mandatory in this case).

As you've read above, a space character is being used as a delimiter for a command, therefore it can't be used in arguments. However, also as stated above, ASF can join out-of-range arguments, which means that you're actually able to use a space character in argument that is defined as a last one for given command. For example, `nickname bob Great Bob` will properly set nickname of `bob` bot to "Great Bob". In the similar way you can check names containing spaces in `owns` command.

Please note that sending a command to the group chat acts like a relay - if you're saying `redeem X` to 3 of your bots sitting together with you on the group chat, it'll result in the same as you'd say `redeem X` to every single one of them privately. In most cases **this is not what you want**, and instead you should use `given bot` command that is being sent to **a single bot in private window**. ASF supports group chat, as in many cases it can be useful source for communication with it, but you should almost never execute any command on the group chat if there are 2 or more ASF bots sitting there, unless you fully understand ASF behaviour written here and you in fact want to relay the same command to every single bot that is listening to you.

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

It's not required to have any extra account for executing commands though Steam chat - you can create a group, set `SteamMasterClanID` properly to that newly created group, then give yourself access either through `SteamOwnerID` or `SteamUserPermissions` of your own bot. This way ASF bot (you) will join group and chat of your selected group, and listen to commands from your own account. You can join the same group chatroom in order to issue commands to yourself (as you'll be sending command to chatroom, and ASF instance sitting on the same chatroom will receive them, even if it shows only as your account being there). Apart from that, you can also use **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)**, but chatroom way is much easier, and if you have access to some alt account, then using that instead is even easier.

* * *

### `<Bots>` argument

`<Bots>` argument is a special variant of plural argument, as in addition to accepting multiple values it also offers extra functionality.

First and foremost, there is a special `ASF` keyword which acts as "all bots in the process", so `status ASF` command is equal to `status all,your,bots,listed,here`. This can also be used to easily identify the bots that you have access to, as `ASF` keyword, despite of targeting all bots, will result in response only from those bots that you can actually send the command to.

`<Bots>` argument supports special "range" syntax, which allows you to choose a range of bots more easily. The general syntax for `<Bots>` in this case is `firstBot..lastBot`. For example, if you have bots named `A, B, C, D, E, F`, you can execute `status B..E`, which is equal to `status B,C,D,E` in this case. When using this syntax, ASF will use alphabetical sorting in order to determine which bots are in your specified range. Both `firstBot` and `lastBot` must be valid bot names recognized by ASF, otherwise range syntax is entirely skipped.

In addition to range syntax above, `<Bots>` argument also supports **[regex](https://en.wikipedia.org/wiki/Regular_expression)** matching. You can activate regex pattern by using `r!<pattern>` as a bot name, where `r!` is ASF activator for regex matching, and `<pattern>` is your regex pattern. An example of a regex-based bot command would be `status r!\d{3}` which will send `status` command to bots that have a name made out of 3 digits (e.g. `123` and `981`). Feel free to take a look at the **[docs](https://docs.microsoft.com/en-us/dotnet/standard/base-types/regular-expression-language-quick-reference)** for further explanation and more examples of available regex patterns.

* * *

## `privacy` settings

`<Settings>` argument accepts **up to 7** different options, separated as usual with standard comma ASF delimiter. Those are, in order:

| Argument | Name           | Child of   |
| -------- | -------------- | ---------- |
| 1        | Profile        |            |
| 2        | OwnedGames     | Profile    |
| 3        | Playtime       | OwnedGames |
| 4        | FriendsList    | Profile    |
| 5        | Inventory      | Profile    |
| 6        | InventoryGifts | Inventory  |
| 7        | Comments       | Profile    |

For description of above fields, please visit **[Steam privacy settings](https://steamcommunity.com/my/edit/settings)**.

While valid values for all of them are:

| Wert | Name          |
| ---- | ------------- |
| 1    | `Privat`      |
| 2    | `Nur Freunde` |
| 3    | `Öffentlich`  |

You can use either a case-insensitive name, or a numeric value. Arguments that were omitted will default to being set to `Private`. It's important to note relation between child and parent of arguments specified above, as child can never have more open permission than its parent. For example, you **can't** have `Public` games owned while having `Private` profile.

### Beispiel

If you want to set **all** privacy settings of your bot named `Main` to `Private`, you can use either of below:

    privacy Main 1
    privacy Main Private
    

This is because ASF will automatically assume all other settings to be `Private`, so there is no need to input them. On the other hand, if you'd like to set all privacy settings to `Public`, then you should use any of below:

    privacy Main 3,3,3,3,3,3,3
    privacy Main Public,Public,Public,Public,Public,Public,Public
    

This way you can also set independent options however you like:

    privacy Main Public,FriendsOnly,Private,Public,Public,Private,Public
    

The above will set profile to public, owned games to friends only, playtime to private, friends list to public, inventory to public, inventory gifts to private and profile comments to public. You can achieve the same with numeric values if you want to.

Remember that child can never have more open permission than its parent. Refer to arguments relationship for available options.

* * *

## `redeem^` modes

`redeem^` command allows you to fine-tune modes that will be used for one single redeem scenario. This works as temporary override of `RedeemingPreferences` **[bot config property](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#bot-config)**.

`<Modes>` argument accepts multiple mode values, separated as usual by a comma. Available mode values are specified below:

| Wert | Name                  | Beschreibung                                                          |
| ---- | --------------------- | --------------------------------------------------------------------- |
| FD   | ForceDistributing     | Forces `Distributing` redeeming preference to be enabled              |
| FF   | ForceForwarding       | Forces `Forwarding` redeeming preference to be enabled                |
| FKMG | ForceKeepMissingGames | Forces `KeepMissingGames` redeeming preference to be enabled          |
| SD   | SkipDistributing      | Forces `Distributing` redeeming preference to be disabled             |
| SF   | SkipForwarding        | Forces `Forwarding` redeeming preference to be disabled               |
| SI   | SkipInitial           | Skips key redemption on initial bot                                   |
| SKMG | SkipKeepMissingGames  | Forces `KeepMissingGames` redeeming preference to be disabled         |
| V    | Validate              | Validates keys for proper format and automatically skips invalid ones |

For example, we'd like to redeem 3 keys on any of our bots that don't own games yet, but not our `primary` bot. For achieving that we can use:

`redeem^ primary FF,SI key1,key2,key3`

* * *

## `input` Befehl

Input command can be used only in `Headless` mode, for inputting given data via **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** or Steam chat when ASF is running without support for user interaction.

General syntax is `input <Bots> <Type> <Value>`.

`<Type>` is case-insensitive and defines input type recognized by ASF. Currently ASF recognizes following types:

| Typ                     | Beschreibung                                                               |
| ----------------------- | -------------------------------------------------------------------------- |
| DeviceID                | 2FA device identificator, if missing from `.maFile`.                       |
| Login                   | `SteamLogin` bot config property, if missing from config.                  |
| Password                | `SteamPassword` bot config property, if missing from config.               |
| SteamGuard              | Auth code sent on your e-mail if you're not using 2FA.                     |
| SteamParentalCode       | `SteamParentalCode` bot config property, if missing from config.           |
| TwoFactorAuthentication | 2FA token generated from your mobile, if you're using 2FA but not ASF 2FA. |

`<Value>` ist der Wert, der für einen gegebenen Typ festgelegt ist. Derzeit sind alle Werte Zeichenketten.

### Beispiel

Lass uns annehmen, dass wir einen Bot haben, der durch SteamGuard (nicht im Zwei-Faktor-Modus) geschützt wird. Wir wollen diesen Bot starten während das Konfigurationsfeld `Headless` auf wahr gesetzt ist.

Um das zu tun müssen wir folgende Befehle ausführen:

`start MeinSteamGuardBot` -> Der Bot wird versuchen zu starten, was allerdings fehlschlagen wird, weil ein Authentifizierungscode benötigt wird. Dann wird er sich selbst stoppen, weil ASF im `Headless`-Modus läuft. Wir brauchen dies, damit das Steam-Netzwerk uns den Authentisierungscode an unsere E-Mail sendet - wenn es keinen Bedarf dafür gibt, würden wir diesen Schritt komplett überspringen.

`input MeinSteamGuardBot SteamGuard ABCDE` -> Wir setzen den `SteamGuard`-Input von `MeinSteamGuardBot` auf `ABCDE`. Natürlich sollte `ABCDE` der Code sein, den du in deiner E-Mail erhalten hast.

`start MeinSteamGuardBot` -> wir starten unseren (gestoppten) Bot wieder. Diesmal wird der Code benutzt, den wir im vorherigen Befehl gesetzt haben und der Bot loggt sich ein und löscht den Code aus seinem internen Speicher.

Auf dem selben Weg können wir auf Bots, die durch Zwei-Faktor-Authentifizierung geschützt sind (und nicht die 2FA von ASF verwenden), zugreifen und andere Dinge während der Laufzeit tun.