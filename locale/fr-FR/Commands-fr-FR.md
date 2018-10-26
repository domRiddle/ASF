# Commandes

ASF prend en charge diverses commandes pouvant être utilisées pour contrôler le comportement du processus et des instances de bot.

Les commandes ci-dessous peuvent être envoyées au bot de trois manières différentes:

- Par le chat privé steam
- Par le chat groupe steam
- Par l’intermédiaire de l' **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)**

N'oubliez pas que l'interaction ASF nécessite que vous soyez éligible pour la commande conformément aux autorisations ASF. Consultez les propriétés de configuration `SteamUserPermissions` et `SteamOwnerID` pour plus de détails.

Toutes les commandes ci-dessous sont affectées par ` CommandPrefix` **[ propriété de configuration globale](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#global-config)**, qui est `!` par défaut. Cela signifie que pour exécuter par exemple `status`, vous devez écrire `!Status` (ou un ` préfixe de commande personnalisé` de votre choix que vous avez défini à la place).

* * *

### Chat privé steam

La méthode la plus simple pour interagir avec ASF - Il suffit d'exécuter la commande sur le bot ASF en cours d'exécution dans le processus ASF. Évidemment, vous ne pouvez pas faire cela si vous utilisez ASF avec un seul compte bot qui vous est propre.

![Capture d"écran](https://i.imgur.com/PPxx7qV.png)

* * *

### Chat groupe steam

Très similaire à ci-dessus, mais cette fois sur le chat d'un groupe Steam donné. N'oubliez pas que cette option nécessite de définir correctement la propriété `SteamMasterClanID`, auquel cas le bot écoutera les commandes également sur le chat du groupe (et le rejoindra si nécessaire). Cela peut également être utilisé pour "parler à vous-même" puisqu'il ne nécessite pas de compte bot dédié. Vous ne voudrez probablement pas utiliser cette méthode pour plus de bots que 1.

* * *

### IPC

La manière la plus avancée et la plus souple d’exécution de commandes, idéale pour les interactions utilisateur (ASF-ui) ainsi que pour les outils tiers ou les scripts (API ASF), requiert que ASF soit exécuté en mode ` IPC `. une commande d'exécution client via l'interface **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)**.

![Capture d"écran](https://i.imgur.com/pzKE4EJ.png)

* * *

## Commandes

| Commande                                                                   | Accès             | Description                                                                                                                                                                                           |
| -------------------------------------------------------------------------- | ----------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `2fa <Bots>`                                                         | `Maître`          | Génère un jeton temporaire **[2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** pour des instances de bot donnés.                                                 |
| `2fano <Bots>`                                                       | `Maître`          | Refuse toutes les confirmations en attente **[2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** pour des instances de bot donnés.                                 |
| `2faok <Bots>`                                                       | `Maître`          | Accepte toutes les confirmations en attente **[2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** pour des instances de bot donnés.                                |
| `addlicense <Bots> <GameIDs>`                                  | `Opérateur`       | Active les `appID` (réseaux Steam) ou les `subIDs` (Steam Store) donnés sur des instances de bot données (jeux gratuits uniquement).                                                                  |
| `bl <Bots>`                                                          | `Maître`          | Liste les utilisateurs en liste noire du module de négociation d'instances de bot données.                                                                                                            |
| `bladd <Bots> <SteamIDs64>`                                    | `Maître`          | La liste noire de `steamIDs` à partir du module de négociation d'instances de bot données.                                                                                                            |
| `blrm <Bots> <SteamIDs64>`                                     | `Maître`          | Supprimer de la liste noire des` steamIDs ` donnés au module de négociation des instances de bot données.                                                                                             |
| `exit`                                                                     | `Propriétaire`    | Arrête tout le processus ASF.                                                                                                                                                                         |
| `farm <Bots>`                                                        | `Maître`          | Redémarre le module farm de cartes pour des instances de bot données.                                                                                                                                 |
| `help`                                                                     | `PartageFamilial` | Affiche l'aide (lien vers cette page).                                                                                                                                                                |
| `input <Bots> <Type> <Value>`                            | `Maître`          | Définit le type d'entrée donné sur la valeur donnée pour des instances de bot données, fonctionne uniquement en mode `Headless` - pour plus d'explications **[ci-dessous](#input-command)**.          |
| `ib <Bots>`                                                          | `Maître`          | Répertorie les applications sur la liste noire à partir (automatic idling) d'instances de bot données.                                                                                                |
| `ibadd <Bots> <AppIDs>`                                        | `Maître`          | Ajoute `appID` donné aux applications de la liste noire à partir de (automatic idling) de certaines instances de bot.                                                                                 |
| `ibrm <Bots> <AppIDs>`                                         | `Maître`          | Supprime les `appID` donnés des applications de la liste noire des applications inactives des instances de bot données.                                                                               |
| `iq <Bots>`                                                          | `Maître`          | Répertorie la file d'attente idling prioritaire d'instances de bot données.                                                                                                                           |
| `iqadd <Bots> <AppIDs>`                                        | `Maître`          | Ajoute `appIDs` donné à la priority idling d'instances de bot données.                                                                                                                                |
| `iqrm <Bots> <AppIDs>`                                         | `Maître`          | Supprime les `appIDs` donnés de la file d'attente priority idling des instances de bot données.                                                                                                       |
| `loot <Bots>`                                                        | `Maître`          | Sends all `LootableTypes` Steam community items of given bot instances to `Master` user defined in their `SteamUserPermissions` (with lowest steamID if more than one).                               |
| `loot@ <Bots> <RealAppIDs>`                                    | `Maître`          | Sends all `LootableTypes` Steam community items matching given `RealAppIDs` of given bot instances to `Master` user defined in their `SteamUserPermissions` (with lowest steamID if more than one).   |
| `loot^ <Bots> <AppID> <ContextID>`                       | `Maître`          | Sends all Steam items from given `AppID` in `ContextID` of given bot instances to `Master` user defined in their `SteamUserPermissions` (with lowest steamID if more than one).                       |
| `loot& <Bots>`                                                   | `Maître`          | Switches looting of given bot instances between enabled/disabled state.                                                                                                                               |
| `nickname <Bots> <Nickname>`                                   | `Maître`          | Changes Steam nickname of given bot instances to given `nickname`.                                                                                                                                    |
| `owns <Bots> <AppIDsOrGameNames>`                              | `Opérateur`       | Checks if given bot instances already own given `appIDs` and/or `gameNames` (can be part of the game's name). It can also be `*` to show all games available.                                         |
| `password <Bots>`                                                    | `Maître`          | Prints encrypted password of given bot instances (in use with `PasswordFormat`).                                                                                                                      |
| `pause <Bots>`                                                       | `Opérateur`       | Permanently pauses automatic cards farming module of given bot instances. ASF will not attempt to farm current account in this session, unless you manually `resume` it, or restart the process.      |
| `pause~ <Bots>`                                                      | `PartageFamilial` | Temporarily pauses automatic cards farming module of given bot instances. Farming will be automatically resumed on the next playing event, or bot disconnect. You can `resume` farming to unpause it. |
| `pause& <Bots> <Seconds>`                                  | `Opérateur`       | Temporarily pauses automatic cards farming module of given bot instances for given amount of `seconds`. After delay, cards farming module is automatically resumed.                                   |
| `play <Bots> <AppIDs,GameName>`                                | `Maître`          | Switches to manual farming - launches given `AppIDs` on given bot instances, optionally also with custom `GameName`. Use `resume` for returning to automatic farming.                                 |
| `privacy <Bots> <Settings>`                                    | `Maître`          | Changes **[Steam privacy settings](https://steamcommunity.com/my/edit/settings)** of given bot instances, to appropriately selected options explained **[below](#privacy-settings)**.                 |
| `redeem <Bots> <Keys>`                                         | `Opérateur`       | Redeems given `cd-keys` on given bot instances.                                                                                                                                                       |
| `redeem^ <Bots> <Modes> <Keys>`                          | `Opérateur`       | Redeems given `cd-keys` on given bot instances, using given `modes` explained **[below](#redeem-modes)**.                                                                                             |
| `rejoinchat <Bots>`                                                  | `Opérateur`       | Force les instances de bot spécifiées à rejoindre leur discussion de groupe `SteamMasterClanID`.                                                                                                      |
| `restart`                                                                  | `Propriétaire`    | Redémarre le processus ASF.                                                                                                                                                                           |
| `resume <Bots>`                                                      | `PartageFamilial` | Reprend le farm automatique d'instances de bot données. Voir aussi ` pause`, ` play`.                                                                                                                 |
| `start <Bots>`                                                       | `Maître`          | Démarre les instances de bot données.                                                                                                                                                                 |
| `stats`                                                                    | `Propriétaire`    | Prints process statistics, such as managed memory usage.                                                                                                                                              |
| `status <Bots>`                                                      | `PartageFamilial` | Imprime le statut d'instances de bot données.                                                                                                                                                         |
| `stop <Bots>`                                                        | `Maître`          | Arrête les instances de bot données.                                                                                                                                                                  |
| `transfer <Bots> <TargetBot>`                                  | `Maître`          | Sends all `TransferableTypes` Steam community items from given bot instances to target bot.                                                                                                           |
| `transfer@ <Bots> <RealAppIDs> <TargetBot>`              | `Maître`          | Sends all `TransferableTypes` Steam community items matching given `RealAppIDs` from given bot instances to target bot.                                                                               |
| `transfer^ <Bots> <AppID> <ContextID> <TargetBot>` | `Maître`          | Envoie tous les objets Steam de `AppID` donné dans `ContextID` d'occurrences de bot données au bot cible.                                                                                             |
| `unpack <Bots>`                                                      | `Maître`          | Déballé tous les boosters packs stockés dans l'inventaire d'instances de bot données.                                                                                                                 |
| `update`                                                                   | `Propriétaire`    | Vérifie les mises à jour ASF de GitHub (cette opération est effectuée automatiquement toutes les 24 heures si ` Mise à jour automatique` ).                                                           |
| `version`                                                                  | `PartageFamilial` | Imprimer la version d'ASF.                                                                                                                                                                            |

* * *

### Remarques

Toutes les commandes sont sensibles à la case, mais leurs arguments (tels que les noms de bot) sont généralement sensibles à la case.

`<Bots>` argument is optional in all commands. When specified, command is executed on given bots. When omitted, command is executed on current bot that receives the command. In other words, `status A` sent to bot `B` is the same as sending `status` to bot `A`.

**Access** of the command defines **minimum** `EPermission` of `SteamUserPermissions` that is required to use the command, with an exception of `Owner` which is `SteamOwnerID` defined in global configuration file (and highest permission available).

Plural arguments, such as `<Bots>`, `<Keys>` or `<AppIDs>` mean that command supports multiple arguments of given type, separated by a comma. For example, `status <Bots>` can be used as `status MyBot,MyOtherBot,Primary`. This will cause given command to be executed on **all target bots** in the same way as you'd send `status` to each bot in a separate chat window. Please note that there is no space after `,`.

ASF uses all whitespace characters as possible delimiters for a command, such as space and newline characters. This means that you don't have to use space for delimiting your arguments, you can as well use any other whitespace character (such as tab or new line).

ASF will "join" extra out-of-range arguments to plural type of the last in-range argument. This means that `redeem bot key1 key2 key3` for `redeem <Bots> <Keys>` will work exactly the same as `redeem bot key1,key2,key3`. Together with accepting newline as command delimiter, this makes it possible for you to write `redeem bot` then paste a list of keys separated by any acceptable delimiter character (such as newline), or standard `,` ASF delimiter. Keep in mind that this trick can be used only for command variant that uses the most amount of arguments (so specifying `<Bots>` is mandatory in this case).

As you've read above, a space character is being used as a delimiter for a command, therefore it can't be used in arguments. However, also as stated above, ASF can join out-of-range arguments, which means that you're actually able to use a space character in argument that is defined as a last one for given command. For example, `nickname bob Great Bob` will properly set nickname of `bob` bot to "Great Bob". In the similar way you can check names containing spaces in `owns` command.

Please note that sending a command to the group chat acts like a relay - if you're saying `redeem X` to 3 of your bots sitting together with you on the group chat, it'll result in the same as you'd say `redeem X` to every single one of them privately. In most cases **this is not what you want**, and instead you should use `given bot` command that is being sent to **a single bot in private window**. ASF supports group chat, as in many cases it can be useful source for communication with it, but you should almost never execute any command on the group chat if there are 2 or more ASF bots sitting there, unless you fully understand ASF behaviour written here and you in fact want to relay the same command to every single bot that is listening to you.

*And even in this case you should use private chat with `<Bots>` syntax instead.*

* * *

Some commands are also available with their aliases, to save you on typing:

| Commande     | Alias |
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

| Argument | Nom            | Child of   |
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

| Valeur  | Nom           |
| ------- | ------------- |
| 1       | `Private`     |
| 2       | `FriendsOnly` |
| 3       | `Public`      |

You can use either a case-insensitive name, or a numeric value. Arguments that were omitted will default to being set to `Private`. It's important to note relation between child and parent of arguments specified above, as child can never have more open permission than its parent. For example, you **can't** have `Public` games owned while having `Private` profile.

### Exemple

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

| Value | Nom                   | Description                                                           |
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

## `input` command

Input command can be used only in `Headless` mode, for inputting given data via **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** or Steam chat when ASF is running without support for user interaction.

General syntax is `input <Bots> <Type> <Value>`.

`<Type>` is case-insensitive and defines input type recognized by ASF. Currently ASF recognizes following types:

| Type                    | Description                                                                |
| ----------------------- | -------------------------------------------------------------------------- |
| DeviceID                | 2FA device identificator, if missing from `.maFile`.                       |
| Login                   | `SteamLogin` bot config property, if missing from config.                  |
| Mot de passe            | `SteamPassword` bot config property, if missing from config.               |
| SteamGuard              | Auth code sent on your e-mail if you're not using 2FA.                     |
| SteamParentalCode       | `SteamParentalCode` bot config property, if missing from config.           |
| TwoFactorAuthentication | 2FA token generated from your mobile, if you're using 2FA but not ASF 2FA. |

`<Value>` is value set for given type. Currently all values are strings.

### Exemple

Disons que nous avons un bot protégé par SteamGuard en mode non-2FA. Nous voulons lancer ce bot avec `Headless` défini sur true.

Pour ce faire, nous devons exécuter les commandes suivantes:

` start MySteamGuardBot` -> Le bot tentera de se connecter, échouera car un code d'authentification est requis, puis s'arrêtera en raison d'une exécution en mode `headless`. Nous en avons besoin pour que le réseau Steam nous envoie le code d'autorisation sur notre adresse électronique. Si cela n'était pas nécessaire, nous ignorerions cette étape.

`input MySteamGuardBot SteamGuard ABCDE` -> We set `SteamGuard` input of `MySteamGuardBot` bot to `ABCDE`. Of course, `ABCDE` in this case is auth code that we got on our e-mail.

`start MySteamGuardBot` -> We start our (stopped) bot again, this time it automatically uses auth code that we set in previous command, properly logging in, then clearing it.

In the same way we can access 2FA-protected bots (if they're not using ASF 2FA), as well as setting other required properties during runtime.