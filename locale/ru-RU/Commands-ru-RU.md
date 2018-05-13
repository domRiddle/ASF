# Команды

ASF поддерживает множество команд, которые используются для контроля поведения процесса и ботов.

Описанные ниже команды, могут быть отправлены боту с помощью 3 способов:

- С помощью приватного чата Steam
- С помощью группового чата Steam
- С помощью **[IPC](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC#post-apicommandcommand)**

Помните, что взаимодействие ASF требует от Вас права для отправки команд, в соответствии с ASF разрешениями. Ознакомьтесь с `SteamUserPermissions` и `SteamOwnerID` значениями конфига для больших подробностей.

Все команды вызываются с помощью префикса `CommandPrefix`в **[глобальной конфигурации бота](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#global-config)**, значение которого, по дефолту - `!`. Это значит,что для запуска команды `status` Вам необходимо прописать `!status`(или пользовательский `CommandPrefix`,который Вы установили вместо стандартного).

* * *

### Приватный чат Steam

Определенно, самый простой метод взаимодействия с ASF - просто введите команду боту ASF, который запущен в ASF. Очевидно, что Вы не можете сделать это если ASF запущен с Вашим личным аккаунтом.

![Скриншот](https://i.imgur.com/PPxx7qV.png)

* * *

### Групповой чат Steam

Очень похож на метод выше, но в этот раз взаимодействие происходит в чате группы Steam. Имейте ввиду, что эта опция требует уставленного значения `SteamMasterClanID` или Вы можете пригласить бота в чат собственноручно. Также, этот метод используется для отправки команд вашему личному аккаунту и не требует отдельно выделенного бот-аккаунта.

* * *

### IPC

Вероятно самый сложный метод вызова ASF, идеален для посторонних инструментов или скриптов, требует запуска ASF в режиме сервера, и клиента для ввода команд в интерфейсе **[IPC](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC)**.

* * *

## Команды

| Команда | Разрешения | Описание | | \---\---- | \---\--- |\---\---\---\----| `2fa` | `Master` | Создает временный **[2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)** токен для данного бота. `2fa <Bots>` | `Master` | Создает временный **[2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)** токен для данных ботов. `2fano` | `Master` | Отменяет все ожидающие **[2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)** подтверждения для данного бота. `2fano <Bots>` | `Master` | Отменяет все ожидающие **[2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)** подтверждения для данных ботов. `2faok` | `Master` | Принимает все ожидающие **[2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)** подтверждения для данного бота. `2faok <Bots>` | `Master` | Принимает все ожидающие **[2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)** подтверждения для данных ботов. `addlicense <GameIDs>` | `Operator` | Активирует данные `appIDs` (сеть Steam) или `subIDs` (магазин Steam) на данном боте (работает только с бесплатными играми). `addlicense <Bots> <GameIDs>` | `Operator` | Активирует данные `appIDs` (сеть Steam) или `subIDs` (магазин Steam) на данных ботах (работает только с бесплатными играми). `bl` | `Master` | Добавляет пользователей в черный список обмена для данного бота. `bl <Bots>` | `Master` | Добавляет пользователей в черный список обмена для данных ботов. `bladd <SteamIDs64>` | `Master` | Blacklists given `steamIDs` from trading module of current bot instance. `bladd <Bots> <SteamIDs64>` | `Master` | Blacklists given `steamIDs` from trading module of given bot instances. `blrm <SteamIDs64>` | `Master` | Removes blacklist of given `steamIDs` from trading module of current bot instance. `blrm <Bots> <SteamIDs64>` | `Master` | Убирает черный список обмена данных `steamIDs` для данных ботов. `exit` | `Owner` | Прекращает работу ASF. `farm` | `Master` | Возобновляет фарм карт для данного бота. `farm <Bots>` | `Master` | Возобновляет фарм для данных ботов. `help` | `FamilySharing` | Показывает помощь(ссылку на эту страницу). `input <Type> <Value>` | `Master` | Устанавливает данный input тип для данного значения данного бота, работает только с `Headless` режимом- подробнее описан **[ниже](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands#input-command)**. `input <Bots> <Type> <Value>` | `Master` | Sets given input type to given value for given bot instances, works only in `Headless` mode - further explained **[below](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands#input-command)**. `ib` | `Master` | Lists apps blacklisted from automatic idling of current bot instance. `ib <Bots>` | `Master` | Lists apps blacklisted from automatic idling of given bot instances. `ibadd <AppIDs>` | `Master` | Adds given `appIDs` to apps blacklisted from automatic idling of current bot instance. `ibadd <Bots> <AppIDs>` | `Master` | Adds given `appIDs` to apps blacklisted from automatic idling of given bot instances. `ibrm <AppIDs>` | `Master` | Removes given `appIDs` from apps blacklisted from automatic idling of current bot instance. `ibrm <Bots> <AppIDs>` | `Master` | Removes given `appIDs` from apps blacklisted from automatic idling of given bot instances. `iq` | `Master` | Lists priority idling queue of current bot instance. `iq <Bots>` | `Master` | Lists priority idling queue of given bot instances. `iqadd <AppIDs>` | `Master` | Adds given `appIDs` to priority idling queue of current bot instance. `iqadd <Bots> <AppIDs>` | `Master` | Adds given `appIDs` to priority idling queue of given bot instances. `iqrm <AppIDs>` | `Master` | Removes given `appIDs` from priority idling queue of current bot instance. `iqrm <Bots> <AppIDs>` | `Master` | Removes given `appIDs` from priority idling queue of given bot instances. `leave` | `Master` | Makes current bot instance leave the group chat. For obvious reasons, this command works only in group chats. `leave <Bots>` | `Master` | Makes given bot instances leave the group chat. For obvious reasons, this command works only in group chats. `loot` | `Master` | Sends all `MatchableTypes` items of current bot instance to `Master` user defined in its `SteamUserPermissions` (with lowest steamID if more than one). `loot <Bots>` | `Master` | Sends all `MatchableTypes` items of given bot instances to `Master` user defined in their `SteamUserPermissions` (with lowest steamID if more than one). `loot^ <AppID> <ContextID>` | `Master` | Sends all items from given `AppID` of `ContextID` of current bot instance to `Master` user defined in its `SteamUserPermissions` (with lowest steamID if more than one). `loot^ <Bots> <AppID> <ContextID>` | `Master` | Sends all items from given `AppID` of `ContextID` of given bot instances to `Master` user defined in their `SteamUserPermissions` (with lowest steamID if more than one). `loot&` | `Master` | Switches looting of current bot instance between enabled/disabled mode. `loot& <Bots>` | `Master` | Switches looting of given bot instances between enabled/disabled mode. `nickname <Nickname>` | `Master` | Changes Steam nickname of current bot instance to given `nickname`. `nickname <Bots> <Nickname>` | `Master` | Changes Steam nickname of given bot instances to given `nickname`. `owns <AppIDsOrGameNames>` | `Operator` | Checks if current bot instance already owns given `appIDs` and/or `gameNames` (can be part of the game's name). It can also be `*` to show all games available. `owns <Bots> <AppIDsOrGameNames>` | `Operator` | Checks if given bot instances already own given `appIDs` and/or `gameNames` (can be part of the game's name). It can also be `*` to show all games available. `password` | `Master` | Prints encrypted password of current bot instance (in use with `PasswordFormat`). `password <Bots>` | `Master` | Prints encrypted password of given bot instances (in use with `PasswordFormat`). `pause` | `Operator` | Permanently pauses automatic cards farming module of current bot instance. ASF will not attempt to farm current account in this session, unless you manually `resume` it, or restart the process. Also called sticky pause. `pause <Bots>` | `Operator` | Permanently pauses automatic cards farming module of given bot instances. ASF will not attempt to farm current account in this session, unless you manually `resume` it, or restart the process. Also called sticky pause. `pause~` | `FamilySharing` | Temporarily pauses automatic cards farming module of current bot instance. Farming will be automatically resumed on the next playing event, or bot disconnect. You can `resume` farming to unpause it. `pause~ <Bots>` | `FamilySharing` | Temporarily pauses automatic cards farming module of given bot instances. Farming will be automatically resumed on the next playing event, or bot disconnect. You can `resume` farming to unpause it. `pause& <Seconds>` | `Operator` | Temporarily pauses automatic cards farming module of current bot instance for given amount of `seconds`. After delay, cards farming module is automatically resumed. `pause& <Bots> <Seconds>` | `Operator` | Temporarily pauses automatic cards farming module of given bot instances for given amount of `seconds`. After delay, cards farming module is automatically resumed. `play <AppIDs,GameName>` | `Master` | Switches to manual farming - launches given `AppIDs` on current bot instance, optionally also with custom `GameName`. Use `resume` for returning to automatic farming. `play <Bots> <AppIDs,GameName>` | `Master` | Switches to manual farming - launches given `AppIDs` on given bot instances, optionally also with custom `GameName`. Use `resume` for returning to automatic farming. `redeem <Keys>` | `Operator` | Активирует введенные `cd-keys` на данном боте. `redeem <Bots> <Keys>` | `Operator` | Активирует введенные `cd-keys` на выбранном боте. `redeem^ <Modes> <Keys>` | `Operator` | Redeems given `cd-keys` on current bot instance, using given `modes` explained **[below](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands#redeem-modes)**. `redeem^ <Bots> <Modes> <Keys>` | `Operator` | Redeems given `cd-keys` on given bot instances, using given `modes` explained **[below](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands#redeem-modes)**. `rejoinchat` | `Operator` | Forces current bot instance to rejoin its `SteamMasterClanID` group chat. `rejoinchat <Bots>` | `Operator` | Forces given bot instances to rejoin their `SteamMasterClanID` group chat. `restart` | `Owner` | Перезапускает ASF. `resume` | `FamilySharing` | Возобновляет автоматический фарм на данном боте. Also see `pause`, `play`. `resume <Bots>` | `FamilySharing` | Возобновляет автоматический фарм на выбранном боте. Also see `pause`, `play`. `start <Bots>` | `Master` | Starts given bot instances. `stats` | `Owner` | Prints process statistics, such as managed memory usage. `status` | `FamilySharing` | Prints status of current bot instance. `status <Bots>` | `FamilySharing` | Prints status of given bot instances. `stop` | `Master` | Останавливает работу данного бота. `stop <Bots>` | `Master` | Останавливает работу выбранного бота. `transfer <Modes> <Bot>` | `Master` | Sends from current bot instance to given `bot` instance, all inventory items of that are matching given `modes`, explained **[below](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands#transfer-modes)**. `transfer <Bots> <Modes> <Bot>` | `Master` | Sends from given bot instances to given `bot` instance, all inventory items of that are matching given `modes`, explained **[below](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands#transfer-modes)**. `unpack` | `Master` | Распаковывает все наборы карточек, находящиеся в инвентаре у данного бота. `unpack <Bots>` | `Master` | Распаковывает все наборы карточек, находящиеся в инвентаре у выбранного бота. `update` | `Owner` | Проверяет GitHub на наличие обновлений ASF (выполняется автоматически каждые 24 часа если включен `AutoUpdates`). `version` | `FamilySharing` | Выводит текущую версию ASF.

* * *

### Примечания

All commands are case-insensitive, but their arguments (such as bot names) are usually case-sensitive.

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

- Commands don't have to be prefixed by `CommandPrefix`, ASF prefixes them for you automatically if needed
- When using commands that are based on `current bot instance`, ASF will choose **any** of currently enabled bots, therefore it's highly recommended to use `given bot instances` commands instead.

* * *

## значения `redeem^`

`redeem^` command allows you to fine-tune modes that will be used for one single redeem scenario. This works as temporary override of `RedeemingPreferences` **[bot config property](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#bot-config)**.

`<Modes>` этот аргумент позволяет использовать несколько значений, разделенных запятой. Доступные значения указаны ниже:

| Значение | Имя                   | Описание                                                              |
| -------- | --------------------- | --------------------------------------------------------------------- |
| FD       | ForceDistributing     | Forces `Distributing` redeeming preference to be enabled              |
| FF       | ForceForwarding       | Forces `Forwarding` redeeming preference to be enabled                |
| FKMG     | ForceKeepMissingGames | Forces `KeepMissingGames` redeeming preference to be enabled          |
| SD       | SkipDistributing      | Forces `Distributing` redeeming preference to be disabled             |
| SF       | SkipForwarding        | Forces `Forwarding` redeeming preference to be disabled               |
| SI       | SkipInitial           | Skips key redemption on initial bot                                   |
| SKMG     | SkipKeepMissingGames  | Forces `KeepMissingGames` redeeming preference to be disabled         |
| V        | Validate              | Validates keys for proper format and automatically skips invalid ones |

For example, we'd like to redeem 3 keys on any of our bots that don't own games yet, but not our `primary` bot. For achieving that we can use:

`redeem^ primary FF,SI key1,key2,key3`

* * *

## значения `transfer`

`<Modes>` этот аргумент позволяет использовать несколько значений, разделенных запятой. Доступные значения указаны ниже:

| Имя        | Значение | Описание                                                                        |
| ---------- | -------- | ------------------------------------------------------------------------------- |
| All        | A        | Все типы предметов, указанные ниже                                              |
| Background | BG       | Фоны, используемые в вашем профиле Steam                                        |
| Booster    | BO       | Наборы карточек                                                                 |
| Card       | C        | Коллекционные карточки Steam, используемые для создания значков (обычных)       |
| Emoticon   | E        | Эмоции, используемые в чате Steam                                               |
| Foil       | F        | Коллекционные карточки Steam, используемые для создания значков (металлических) |
| Gems       | G        | Самоцветы и мешки самоцветов, используемые для создания наборов карточек        |
| Unknown    | U        | Остальные типы предметов                                                        |

Например, чтобы отправить коллекционные металлические карточки с `MyBot` на `MyMain`, необходимо выполнить:

`transfer MyBot C,F MyMain`

* * *

## команда `input`

Input command can be used only in `Headless` mode, for inputting given data via **[IPC](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC)** or Steam chat when ASF is running without support for user interaction.

General syntax is `input <Bots> <Type> <Value>`.

`<Type>` is case-insensitive and defines input type recognized by ASF. Currently ASF recognizes following types:

| Type                    | Описание                                                                   |
| ----------------------- | -------------------------------------------------------------------------- |
| DeviceID                | 2FA device identificator, if missing from `.maFile`.                       |
| Login                   | `SteamLogin` bot config property, if missing from config.                  |
| Password                | `SteamPassword` bot config property, if missing from config.               |
| SteamGuard              | Auth code sent on your e-mail if you're not using 2FA.                     |
| SteamParentalPIN        | `SteamParentalPIN` bot config property, if missing from config.            |
| TwoFactorAuthentication | 2FA token generated from your mobile, if you're using 2FA but not ASF 2FA. |

`<Value>` is value set for given type. Currently all values are strings.

### Пример

Let's say that we have a bot that is protected by SteamGuard in non-2FA mode. We want to launch that bot with `Headless` set to true.

In order to do that, we need to execute following commands:

`start MySteamGuardBot` -> Bot will attempt to log in, fail due to AuthCode needed, then stop due to running in `Headless` mode. We need this in order to make Steam network send us auth code on our e-mail.

`input MySteamGuardBot SteamGuard ABCDE` -> We set `SteamGuard` input of `MySteamGuardBot` bot to `ABCDE`. Of course, `ABCDE` in this case is auth code that we got on our e-mail.

`start MySteamGuardBot` -> We start our (stopped) bot again, this time it automatically uses auth code that we set in previous command, properly logging in, then clearing it.

In the same way we can access 2FA-protected bots (if they're not using ASF 2FA), as well as setting other required properties during runtime.