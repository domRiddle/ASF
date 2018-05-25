# Befehle

ASF unterstützt eine Vielzahl von Befehlen, die einem die Kontrolle über das Verhalten des Prozesses und der Botinstanzen geben.

Die unten angeführten Befehle können über drei verschiedene Wege an einen Bot gesendet werden:

- Über den privaten Steam-Chat
- Über einen Steam-Gruppen-Chat
- Über **[IPC](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC#post-apicommandcommand)**

Bitte vergiss nicht, dass jegliche ASF-Interaktion verlangt, dass du gemäß deinen ASF-Einstellungen über die Berechtigungen für den Befehl verfügen musst. Für mehr Informationen lies bitte die Einträge zu den Config-Feldern `SteamUserPermissions` und `SteamOwnerID` durch.

Alle unten angeführten Befehle werden durch das **[globale Konfigurationsfeld](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#global-config)** `CommandPrefix` beeinflusst, welches standardmäsig `!` ist. Das heißt, dass du um zum Beispiel den `status`-Befehl auszuführen `!status` (oder dein entsprechendes `CommandPrefix`) schreiben solltest.

* * *

### privater Steam Chat

... ist definitiv die einfachste Methode mit ASF zu interagieren - Sende dazu einfach den Befehl an einen ASF-Bot der zur Zeit im ASF-Prozess läuft. Logischerweise kannst du das nicht machen, wenn du ASF mit nur einem einzigen Bot-Account laufen lässt.

![Screenshot](https://i.imgur.com/PPxx7qV.png)

* * *

### Steam Gruppenchat

Sehr ähnlich zur oben genannten Möglichkeit, allerdings diesmal über den Gruppenchat einer vorgegebenen Steam-Gruppe. Vergiss nicht, dass diese Möglichkeit voraussetzt, dass du entweder die `SteamMasterClanID` richtig eingestellt hast, oder du deinen Bot manuell zum Chat einladen musst. Dies kann außerdem verwendet werden, um "mit dir selbst zu reden" und benötigt keinen dedizierten Bot-Account.

* * *

### IPC

Wahrscheinlich die "komplexeste" Methode mit ASF zu kommunizieren. IPC ist perfekt für Programme von Drittanbietern und Skripte. Es verlangt allerdings, dass ASF im Server-Modus läuft und ein Client die Befehle über das **[IPC](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC)** Interface an ASF sendet.

![Screenshot](https://i.imgur.com/TsAHcM0.png)

* * *

## Befehle

| Command | Access | Description | | \---\---- | \---\--- |\---\---\---\----| `2fa <Bots>` | `Master` | Generates temporary **[2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)** token for given bot instances. `2fano <Bots>` | `Master` | Denies all pending **[2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)** confirmations for given bot instances. `2faok <Bots>` | `Master` | Accepts all pending **[2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)** confirmations for given bot instances. `addlicense <Bots> <GameIDs>` | `Operator` | Activates given `appIDs` (Steam Network) or `subIDs` (Steam Store) on given bot instances (free games only). `bl <Bots>` | `Master` | Lists blacklisted users from trading module of given bot instances. `bladd <Bots> <SteamIDs64>` | `Master` | Blacklists given `steamIDs` from trading module of given bot instances. `blrm <Bots> <SteamIDs64>` | `Master` | Removes blacklist of given `steamIDs` from trading module of given bot instances. `exit` | `Owner` | Stops whole ASF process. `farm <Bots>` | `Master` | Restarts cards farming module for given bot instances. `help` | `FamilySharing` | Shows help (link to this page). `input <Bots> <Type> <Value>` | `Master` | Sets given input type to given value for given bot instances, works only in `Headless` mode - further explained **[below](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands#input-command)**. `ib <Bots>` | `Master` | Lists apps blacklisted from automatic idling of given bot instances. `ibadd <Bots> <AppIDs>` | `Master` | Adds given `appIDs` to apps blacklisted from automatic idling of given bot instances. `ibrm <Bots> <AppIDs>` | `Master` | Removes given `appIDs` from apps blacklisted from automatic idling of given bot instances. `iq <Bots>` | `Master` | Lists priority idling queue of given bot instances. `iqadd <Bots> <AppIDs>` | `Master` | Adds given `appIDs` to priority idling queue of given bot instances. `iqrm <Bots> <AppIDs>` | `Master` | Removes given `appIDs` from priority idling queue of given bot instances. `leave <Bots>` | `Master` | Makes given bot instances leave the group chat. For obvious reasons, this command works only in group chats. `loot <Bots>` | `Master` | Sends all `MatchableTypes` items of given bot instances to `Master` user defined in their `SteamUserPermissions` (with lowest steamID if more than one). `loot^ <Bots> <AppID> <ContextID>` | `Master` | Sends all items from given `AppID` of `ContextID` of given bot instances to `Master` user defined in their `SteamUserPermissions` (with lowest steamID if more than one). `loot& <Bots>` | `Master` | Switches looting of given bot instances between enabled/disabled mode. `nickname <Bots> <Nickname>` | `Master` | Changes Steam nickname of given bot instances to given `nickname`. `owns <Bots> <AppIDsOrGameNames>` | `Operator` | Checks if given bot instances already own given `appIDs` and/or `gameNames` (can be part of the game's name). It can also be `*` to show all games available. `password <Bots>` | `Master` | Prints encrypted password of given bot instances (in use with `PasswordFormat`). `pause <Bots>` | `Operator` | Permanently pauses automatic cards farming module of given bot instances. ASF will not attempt to farm current account in this session, unless you manually `resume` it, or restart the process. Also called sticky pause. `pause~ <Bots>` | `FamilySharing` | Temporarily pauses automatic cards farming module of given bot instances. Farming will be automatically resumed on the next playing event, or bot disconnect. You can `resume` farming to unpause it. `pause& <Bots> <Seconds>` | `Operator` | Temporarily pauses automatic cards farming module of given bot instances for given amount of `seconds`. After delay, cards farming module is automatically resumed. `play <Bots> <AppIDs,GameName>` | `Master` | Switches to manual farming - launches given `AppIDs` on given bot instances, optionally also with custom `GameName`. Use `resume` for returning to automatic farming. `redeem <Bots> <Keys>` | `Operator` | Redeems given `cd-keys` on given bot instances. | `Operator` | Redeems given `cd-keys` on given bot instances. `redeem^ <Bots> <Modes> <Keys>` | `Operator` | Redeems given `cd-keys` on given bot instances, using given `modes` explained **[below](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands#redeem-modes)**. `rejoinchat <Bots>` | `Operator` | Forces given bot instances to rejoin their `SteamMasterClanID` group chat. `restart` | `Owner` | Restarts ASF process. `resume <Bots>` | `FamilySharing` | Resumes automatic farming of given bot instances. Also see `pause`, `play`. `start <Bots>` | `Master` | Starts given bot instances. `stats` | `Owner` | Prints process statistics, such as managed memory usage. `status <Bots>` | `FamilySharing` | Prints status of given bot instances. `stop <Bots>` | `Master` | Stops given bot instances. `transfer <Bots> <Modes> <Bot>` | `Master` | Sends from given bot instances to given `Bot` instance, all inventory items that are matching given `modes`, explained **[below](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands#transfer-modes)**. `unpack <Bots>` | `Master` | Unpacks all booster packs stored in the inventory of given bot instances. `update` | `Owner` | Checks GitHub for ASF updates (this is done automatically every 24 hours if `AutoUpdates`). `version` | `FamilySharing` | Prints version of ASF.

* * *

### Notizen

All commands are case-insensitive, but their arguments (such as bot names) are usually case-sensitive.

`<Bots>` argument is optional in all commands. When specified, command is executed on given bots. When omitted, command is executed on current bot that receives the command. In other words, `status A` sent to bot `B` is the same as sending `status` to bot `A`.

**Access** of the command defines **minimum** `EPermission` of `SteamUserPermissions` that is required to use the command, with an exception of `Owner` which is `SteamOwnerID` defined in global configuration file (and highest permission available).

Plural arguments, such as `<Bots>`, `<Keys>` or `<AppIDs>` mean that command supports multiple arguments of given type, separated by a comma. For example, `status <Bots>` can be used as `status MyBot,MyOtherBot,Primary`. This will cause given command to be executed on **all target bots** in the same way as you'd send `status` to each bot in a separate chat window. Please note that there is no space after `,`.

ASF uses all whitespace characters as possible delimiters for a command, such as space and newline characters. This means that you don't have to use space for delimiting your arguments, you can as well use any other whitespace character (such as tab or new line).

ASF will "join" extra out-of-range arguments to plural type of the last in-range argument. This means that `redeem bot key1 key2 key3` for `redeem <Bots> <Keys>` will work exactly the same as `redeem bot key1,key2,key3`. Together with accepting newline as command delimiter, this makes it possible for you to write `redeem bot` then paste a list of keys separated by any acceptable delimiter character (such as newline), or standard `,` ASF delimiter. Keep in mind that this trick can be used only for command variant that uses the most amount of arguments.

As you've read above, a space character is being used as a delimiter for a command, therefore it can't be used in arguments. However, also as stated above, ASF can join out-of-range arguments, which means that you're actually able to use a space character in argument that is defined as a last one for given command. For example, `nickname bob Great Bob` will properly set nickname of `bob` bot to "Great Bob". In the similar way you can check names containing spaces in `owns` command.

For `<Bots>` argument, there is a special `ASF` keyword which acts as "all bots in the process", so `status ASF` is equal to `status all,your,bots,listed,here`.

It's nice to note that `<Bots>` also supports special "range" syntax, which allows you to choose a range of bots more easily. The general syntax for `<Bots>` in this case is `firstBot..lastBot`. For example, if you have bots named `A, B, C, D, E, F`, you can execute `status B..E`, which is equal to `status B,C,D,E` in this case. When using this syntax, ASF will use alphabetical sorting in order to determine which bots are in your specified range. Both `firstBot` and `lastBot` must be valid bot names recognized by ASF, otherwise range syntax is entirely skipped.

* * *

Please note that sending a command to the group chat acts like a relay - if you're saying `redeem X` to 3 of your bots sitting together with you on the group chat, it'll result in the same as you'd say `redeem X` to every single one of them privately. In most cases **this is not what you want**, and instead you should use `given bot` command that is being sent to **a single bot in private window**. ASF supports group chat, as in many cases it can be useful source for communication with it, but you should almost never execute any command on the group chat if there are 2 or more ASF bots sitting there, unless you fully understand ASF behaviour written here and you in fact want to relay the same command to every single bot that is listening to you.

*And even in this case you should use private chat with `<Bots>` syntax instead.*

* * *

Some commands are also available with their aliases, to save you on typing:

| Command | Alias | |\---\---\---\----|\---\----| `owns ASF` | `oa` | `status ASF` | `sa` | `redeem` | `r` | `redeem^` | `r^` |

* * *

It's not required to have any extra account for executing commands though Steam chat - you can create a group, set `SteamMasterClanID` properly to that newly created group, then give yourself access either through `SteamOwnerID` or `SteamUserPermissions` of your own bot. This way ASF bot (you) will join group and chat of your selected group, and listen to commands from your own account. You can join the same group chatroom in order to issue commands to yourself (as you'll be sending command to chatroom, and ASF instance sitting on the same chatroom will receive them, even if it shows only as your account being there). Apart from that, you can also use **[IPC](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC)**, but chatroom way is much easier, and if you have access to some alt account, then using that instead is even easier.

* * *

When using **IPC**, keep in mind that:

- Befehle müssen nicht mit deinem `CommandPrefix` beginnen. ASF fügt dieses automatisch hinzu, wenn es benötigt wird
- Bei der Verwendung von Befehlen, die auf einer `Bot-Instanz` beruhen, wird ASF **irgendeinen** der zur Zeit aktivierten Bots wählen, weshalb wir dir wärmstens empfehlen eine Variante des Befehls zu verwenden, bei der du eine Bot-Instanz mitgeben kannst.

* * *

## `redeem^` modes

`redeem^` command allows you to fine-tune modes that will be used for one single redeem scenario. This works as temporary override of `RedeemingPreferences` **[bot config property](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#bot-config)**.

`<Modes>` argument accepts multiple mode values, separated as usual by a comma. Available mode values are specified below:

| Value | Name                  | Description                                                           |
| ----- | --------------------- | --------------------------------------------------------------------- |
| FD    | ForceDistributing     | Forces `Distributing` redeeming preference to be enabled              |
| FF    | ForceForwarding       | Forces `Forwarding` redeeming preference to be enabled                |
| FKMG  | ForceKeepMissingGames | Forces `KeepMissingGames` redeeming preference to be enabled          |
| SD    | SkipDistributing      | Forces `Distributing` redeeming preference to be disabled             |
| SF    | SkipForwarding        | Forces `Forwarding` redeeming preference to be disabled               |
| SI    | SkipInitial           | Skips key redemption on initial bot                                   |
| SKMG  | SkipKeepMissingGames  | Forces `KeepMissingGames` redeeming preference to be disabled         |
| V     | Validate              | Validates keys for proper format and automatically skips invalid ones |

For example, we'd like to redeem 3 keys on any of our bots that don't own games yet, but not our `primary` bot. For achieving that we can use:

`redeem^ primary FF,SI key1,key2,key3`

* * *

## `transfer` modes

`<Modes>` argument accepts multiple mode values, separated as usual by a comma. Available mode values are specified below:

| Value      | Alias | Description                                                   |
| ---------- | ----- | ------------------------------------------------------------- |
| All        | A     | Same as enabling all item types below                         |
| Background | BG    | Profile background to use on your Steam profile               |
| Booster    | BO    | Booster pack                                                  |
| Card       | C     | Steam trading card, being used for crafting badges (non-foil) |
| Emoticon   | E     | Emoticon to use in Steam Chat                                 |
| Foil       | F     | Foil variant of `Card`                                        |
| Gems       | G     | Steam gems being used for crafting boosters, sacks included   |
| Unknown    | U     | Every type that doesn't fit in any of the above               |

For example, in order to send trading cards and foils from `MyBot` to `MyMain`, you'd execute:

`transfer MyBot C,F MyMain`

* * *

## `input` command

Input command can be used only in `Headless` mode, for inputting given data via **[IPC](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC)** or Steam chat when ASF is running without support for user interaction.

General syntax is `input <Bots> <Type> <Value>`.

`<Type>` is case-insensitive and defines input type recognized by ASF. Currently ASF recognizes following types:

| Type                    | Description                                                                |
| ----------------------- | -------------------------------------------------------------------------- |
| DeviceID                | 2FA device identificator, if missing from `.maFile`.                       |
| Login                   | `SteamLogin` bot config property, if missing from config.                  |
| Password                | `SteamPassword` bot config property, if missing from config.               |
| SteamGuard              | Auth code sent on your e-mail if you're not using 2FA.                     |
| SteamParentalPIN        | `SteamParentalPIN` bot config property, if missing from config.            |
| TwoFactorAuthentication | 2FA token generated from your mobile, if you're using 2FA but not ASF 2FA. |

`<Value>` is value set for given type. Currently all values are strings.

### Beispiel

Let's say that we have a bot that is protected by SteamGuard in non-2FA mode. We want to launch that bot with `Headless` set to true.

In order to do that, we need to execute following commands:

`start MySteamGuardBot` -> Bot will attempt to log in, fail due to AuthCode needed, then stop due to running in `Headless` mode. We need this in order to make Steam network send us auth code on our e-mail.

`input MySteamGuardBot SteamGuard ABCDE` -> We set `SteamGuard` input of `MySteamGuardBot` bot to `ABCDE`. Of course, `ABCDE` in this case is auth code that we got on our e-mail.

`start MySteamGuardBot` -> We start our (stopped) bot again, this time it automatically uses auth code that we set in previous command, properly logging in, then clearing it.

In the same way we can access 2FA-protected bots (if they're not using ASF 2FA), as well as setting other required properties during runtime.