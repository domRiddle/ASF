# Commando's

ASF ondersteunt diverse commando's om het programma en de bots te besturen.

De commando's kunnen op drie verschillende manieren naar een bot worden verzonden:

- Via een Steam privéchat
- Via een Steam groepsgesprek
- Via **[IPC](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC#post-apicommandcommand)**

Hou er rekening mee dat je voor het communiceren met ASF de rechten nodig hebt in je ASF-instellingen om een commando te kunnen gebruiken. Check de `SteamUserPermissions` en de `SteamOwnerID` config-instellingen voor meer details.

Alle onderstaande commando's moeten beginnen met de `CommandPrefix` **[globale configuratie instelling](https://github. com/JustArchi/ArchiSteamFarm/wiki/Configuration-nl-BE#globale-configuratie)**. Standaard is dat `!`. Met andere woorden: om de opdracht `status` uit te voeren, moet je `!Status` (of de door jou ingestelde ` CommandPrefix`) invoeren.

* * *

### Steam privéchat

Dit is de makkelijkste manier om met ASF te communiceren. Stuur de commando naar een ASF-bot die actief is. Uiteraard kun je dat niet doen als je ASF uitvoert met slechts één bot-account.

![Screenshot](https://i.imgur.com/PPxx7qV.png)

* * *

### Steam groepsgesprek

Vergelijkbaar met de bovenstaande optie, maar dan via het groepsgesprek van jouw Steam-groep. Hou er rekening mee dat voor deze optie `SteamMasterClanID` correct moet worden ingesteld, of dat je de bot handmatig uitnodigt voor de chat. Dit kan ook worden gebruikt voor 'praten met jezelf' en vereist geen ander bot-account.

* * *

### IPC

Waarschijnlijk de meest "complexe" methode om met ASF te communiceren. IPC is perfect voor programma's en scripts van derden. Het vereist echter dat ASF wordt uitgevoerd in de servermodus en een client om commando's naar ASF te verzenden via de **[IPC](https://github. com/JustArchi/ArchiSteamFarm/wiki/IPC-nl-BE)** interface.

![Screenshot](https://i.imgur.com/TsAHcM0.png)

* * *

## Commando's

| Commando                                             | Toegang         | Beschrijving                                                                                                                                                                                          |
| ---------------------------------------------------- | --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `2fa <Bots>`                                   | `Master`        | Genereert tijdelijke **[2FA](https://github. com/JustArchi/ArchiSteamFarm/wiki/Two-factor-authentication-nl-BE)** codes voor de betrokken bot(s).                                                     |
| `2fano <Bots>`                                 | `Master`        | Weigert alle **[2FА](https://github. com/JustArchi/ArchiSteamFarm/wiki/Two-factor-authentication-nl-BE)** bevestigingen die in afwachting zijn van de betrokken bot(s).                               |
| `2faok <Bots>`                                 | `Master`        | Accepteert alle **[2FА](https://github. com/JustArchi/ArchiSteamFarm/wiki/Two-factor-authentication-nl-BE)** bevestigingen die in afwachting zijn van de betrokken bot(s).                            |
| `addlicense <Bots> <GameIDs>`            | `Operator`      | Activates given `appIDs` (Steam Network) or `subIDs` (Steam Store) on given bot instances (free games only).                                                                                          |
| `bl <Bots>`                                    | `Master`        | Lists blacklisted users from trading module of given bot instances.                                                                                                                                   |
| `bladd <Bots> <SteamIDs64>`              | `Master`        | Blacklists given `steamIDs` from trading module of given bot instances.                                                                                                                               |
| `blrm <Bots> <SteamIDs64>`               | `Master`        | Removes blacklist of given `steamIDs` from trading module of given bot instances.                                                                                                                     |
| `exit`                                               | `Owner`         | Stopt het ASF-programma.                                                                                                                                                                              |
| `farm <Bots>`                                  | `Master`        | Restarts cards farming module for given bot instances.                                                                                                                                                |
| `help`                                               | `FamilySharing` | Shows help (link to this page).                                                                                                                                                                       |
| `input <Bots> <Type> <Value>`      | `Master`        | Sets given input type to given value for given bot instances, works only in `Headless` mode - further explained **[below](#input-command)**.                                                          |
| `ib <Bots>`                                    | `Master`        | Lists apps blacklisted from automatic idling of given bot instances.                                                                                                                                  |
| `ibadd <Bots> <AppIDs>`                  | `Master`        | Adds given `appIDs` to apps blacklisted from automatic idling of given bot instances.                                                                                                                 |
| `ibrm <Bots> <AppIDs>`                   | `Master`        | Removes given `appIDs` from apps blacklisted from automatic idling of given bot instances.                                                                                                            |
| `iq <Bots>`                                    | `Master`        | Lists priority idling queue of given bot instances.                                                                                                                                                   |
| `iqadd <Bots> <AppIDs>`                  | `Master`        | Adds given `appIDs` to priority idling queue of given bot instances.                                                                                                                                  |
| `iqrm <Bots> <AppIDs>`                   | `Master`        | Removes given `appIDs` from priority idling queue of given bot instances.                                                                                                                             |
| `leave <Bots>`                                 | `Master`        | Makes given bot instances leave the group chat. For obvious reasons, this command works only in group chats.                                                                                          |
| `loot <Bots>`                                  | `Master`        | Sends all `LootableTypes` items of given bot instances to `Master` user defined in their `SteamUserPermissions` (with lowest steamID if more than one).                                               |
| `loot^ <Bots> <AppID> <ContextID>` | `Master`        | Sends all items from given `AppID` of `ContextID` of given bot instances to `Master` user defined in their `SteamUserPermissions` (with lowest steamID if more than one).                             |
| `loot& <Bots>`                             | `Master`        | Switches looting of given bot instances between enabled/disabled mode.                                                                                                                                |
| `nickname <Bots> <Nickname>`             | `Master`        | Changes Steam nickname of given bot instances to given `nickname`.                                                                                                                                    |
| `owns <Bots> <AppIDsOrGameNames>`        | `Operator`      | Checks if given bot instances already own given `appIDs` and/or `gameNames` (can be part of the game's name). It can also be `*` to show all games available.                                         |
| `password <Bots>`                              | `Master`        | Prints encrypted password of given bot instances (in use with `PasswordFormat`).                                                                                                                      |
| `pause <Bots>`                                 | `Operator`      | Permanently pauses automatic cards farming module of given bot instances. ASF will not attempt to farm current account in this session, unless you manually `resume` it, or restart the process.      |
| `pause~ <Bots>`                                | `FamilySharing` | Temporarily pauses automatic cards farming module of given bot instances. Farming will be automatically resumed on the next playing event, or bot disconnect. You can `resume` farming to unpause it. |
| `pause& <Bots> <Seconds>`            | `Operator`      | Temporarily pauses automatic cards farming module of given bot instances for given amount of `seconds`. After delay, cards farming module is automatically resumed.                                   |
| `play <Bots> <AppIDs,GameName>`          | `Master`        | Switches to manual farming - launches given `AppIDs` on given bot instances, optionally also with custom `GameName`. Use `resume` for returning to automatic farming.                                 |
| `privacy <Bots> <Settings>`              | `Master`        | Changes **[Steam privacy settings](https://steamcommunity.com/my/edit/settings)** of given bot instances, to appropriately selected options explained **[below](#privacy-settings)**.                 |
| `redeem <Bots> <Keys>`                   | `Operator`      | Redeems given `cd-keys` on given bot instances.                                                                                                                                                       |
| `redeem^ <Bots> <Modes> <Keys>`    | `Operator`      | Redeems given `cd-keys` on given bot instances, using given `modes` explained **[below](#redeem-modes)**.                                                                                             |
| `rejoinchat <Bots>`                            | `Operator`      | Forces given bot instances to rejoin their `SteamMasterClanID` group chat.                                                                                                                            |
| `restart`                                            | `Owner`         | Herstart het ASF-programma.                                                                                                                                                                           |
| `resume <Bots>`                                | `FamilySharing` | Resumes automatic farming of given bot instances. Also see `pause`, `play`.                                                                                                                           |
| `start <Bots>`                                 | `Master`        | Start de betrokken bot(s).                                                                                                                                                                            |
| `stats`                                              | `Owner`         | Prints process statistics, such as managed memory usage.                                                                                                                                              |
| `status <Bots>`                                | `FamilySharing` | Toont de status van de betrokken bot(s).                                                                                                                                                              |
| `stop <Bots>`                                  | `Master`        | Stopt de betrokken bot(s).                                                                                                                                                                            |
| `transfer <Bots> <Modes> <Bot>`    | `Master`        | Sends from given bot instances to given `Bot` instance, all inventory items that are matching given `modes`, explained **[below](#transfer-modes)**.                                                  |
| `unpack <Bots>`                                | `Master`        | Pakt alle booster packs uit die zich in de inventaris bevinden van de betrokken bot(s).                                                                                                               |
| `update`                                             | `Owner`         | Checks GitHub for ASF updates (this is done automatically every 24 hours if `AutoUpdates`).                                                                                                           |
| `version`                                            | `FamilySharing` | Toont het versienummer van ASF.                                                                                                                                                                       |

* * *

### Opmerkingen

Alle commando's zijn niet hoofdlettergevoelig, maar de parameters (zoals botnamen) zijn meestal wel hoofdlettergevoelig.

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

| Commando     | Alias |
| ------------ | ----- |
| `owns ASF`   | `oa`  |
| `status ASF` | `sa`  |
| `redeem`     | `r`   |
| `redeem^`    | `r^`  |

* * *

It's not required to have any extra account for executing commands though Steam chat - you can create a group, set `SteamMasterClanID` properly to that newly created group, then give yourself access either through `SteamOwnerID` or `SteamUserPermissions` of your own bot. This way ASF bot (you) will join group and chat of your selected group, and listen to commands from your own account. You can join the same group chatroom in order to issue commands to yourself (as you'll be sending command to chatroom, and ASF instance sitting on the same chatroom will receive them, even if it shows only as your account being there). Apart from that, you can also use **[IPC](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC)**, but chatroom way is much easier, and if you have access to some alt account, then using that instead is even easier.

* * *

When using **IPC**, keep in mind that:

- Commands don't have to be prefixed by `CommandPrefix`, ASF prefixes them for you automatically if needed
- When using commands that are based on `current bot instance`, ASF will choose **any** of currently enabled bots, therefore it's highly recommended to use `given bot instances` commands instead.

* * *

## Privacyinstellingen (`privacy`)

`<Settings>` argument accepts **up to 6** different options, separated as usual with standard comma ASF delimiter. Those are, in order:

| Argument | Naam           | Child of   |
| -------- | -------------- | ---------- |
| 1        | Profile        |            |
| 2        | OwnedGames     | Profile    |
| 3        | Playtime       | OwnedGames |
| 4        | Inventory      | Profile    |
| 5        | InventoryGifts | Inventory  |
| 6        | Comments       | Profile    |

For description of above fields, please visit **[Steam privacy settings](https://steamcommunity.com/my/edit/settings)**.

While valid values for all of them are:

| Waarde | Naam          |
| ------ | ------------- |
| 1      | `Private`     |
| 2      | `FriendsOnly` |
| 3      | `Public`      |

You can use either a case-insensitive name, or a numeric value. Arguments that were omitted will default to being set to `Private`. It's important to note relation between child and parent of arguments specified above, as child can never have more open permission than its parent. For example, you **can't** have `Public` games owned while having `Private` profile.

### Voorbeeld

If you want to set **all** privacy settings of your bot named `Main` to `Private`, you can use either of below:

    privacy Main 0
    privacy Main Private
    

This is because ASF will automatically assume all other settings to be `Private`, so there is no need to input them. On the other hand, if you'd like to set all privacy settings to `Public`, then you should use any of below:

    privacy Main 3,3,3,3,3,3
    privacy Main Public,Public,Public,Public,Public,Public
    

This way you can also set independent options however you like:

    privacy Main Public,FriendsOnly,Private,Public,Private,Public
    

The above will set profile to public, owned games to friends only, playtime to private, inventory to public, inventory gifts to private and profile comments to public. You can achieve the same with numeric values if you want to.

Remember that child can never have more open permission than its parent. Refer to arguments relationship for available options.

* * *

## `redeem^` modes

`redeem^` command allows you to fine-tune modes that will be used for one single redeem scenario. This works as temporary override of `RedeemingPreferences` **[bot config property](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#bot-config)**.

`<Modes>` argument accepts multiple mode values, separated as usual by a comma. Available mode values are specified below:

| Waarde | Naam                  | Beschrijving                                                          |
| ------ | --------------------- | --------------------------------------------------------------------- |
| FD     | ForceDistributing     | Forces `Distributing` redeeming preference to be enabled              |
| FF     | ForceForwarding       | Forces `Forwarding` redeeming preference to be enabled                |
| FKMG   | ForceKeepMissingGames | Forces `KeepMissingGames` redeeming preference to be enabled          |
| SD     | SkipDistributing      | Forces `Distributing` redeeming preference to be disabled             |
| SF     | SkipForwarding        | Forces `Forwarding` redeeming preference to be disabled               |
| SI     | SkipInitial           | Skips key redemption on initial bot                                   |
| SKMG   | SkipKeepMissingGames  | Forces `KeepMissingGames` redeeming preference to be disabled         |
| V      | Validate              | Validates keys for proper format and automatically skips invalid ones |

For example, we'd like to redeem 3 keys on any of our bots that don't own games yet, but not our `primary` bot. For achieving that we can use:

`redeem^ primary FF,SI key1,key2,key3`

* * *

## `transfer` modes

`<Modes>` argument accepts multiple mode values, separated as usual by a comma. Available mode values are specified below:

| Waarde     | Alias | Beschrijving                                                  |
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

| Type                    | Beschrijving                                                               |
| ----------------------- | -------------------------------------------------------------------------- |
| DeviceID                | 2FA device identificator, if missing from `.maFile`.                       |
| Login                   | `SteamLogin` bot config property, if missing from config.                  |
| Password                | `SteamPassword` bot config property, if missing from config.               |
| SteamGuard              | Auth code sent on your e-mail if you're not using 2FA.                     |
| SteamParentalPIN        | `SteamParentalPIN` bot config property, if missing from config.            |
| TwoFactorAuthentication | 2FA token generated from your mobile, if you're using 2FA but not ASF 2FA. |

`<Value>` is value set for given type. Currently all values are strings.

### Voorbeeld

Let's say that we have a bot that is protected by SteamGuard in non-2FA mode. We want to launch that bot with `Headless` set to true.

In order to do that, we need to execute following commands:

`start MySteamGuardBot` -> Bot will attempt to log in, fail due to AuthCode needed, then stop due to running in `Headless` mode. We need this in order to make Steam network send us auth code on our e-mail.

`input MySteamGuardBot SteamGuard ABCDE` -> We set `SteamGuard` input of `MySteamGuardBot` bot to `ABCDE`. Of course, `ABCDE` in this case is auth code that we got on our e-mail.

`start MySteamGuardBot` -> We start our (stopped) bot again, this time it automatically uses auth code that we set in previous command, properly logging in, then clearing it.

In the same way we can access 2FA-protected bots (if they're not using ASF 2FA), as well as setting other required properties during runtime.