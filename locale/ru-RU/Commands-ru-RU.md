# Команды

ASF поддерживает множество команд, которые используются для управления поведением процесса и отдельных ботов.

Описанные ниже команды, могут быть отправлены боту одним из трёх способов:

- С помощью личного чата Steam
- С помощью группового чата Steam
- С помощью **[IPC](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC-ru-RU#post-apicommandcommand)**

Помните, что для взаимодействия с ASF вам требуется, чтобы у вас были права на выполнение команд, в соответствии с разрешениями ASF. Для дополнительной информации ознакомьтесь с конфигурационными параметрами `SteamUserPermissions` и`SteamOwnerID`.

Все команды должны начинаться с префикса, задаваемого в **[параметре глобальной конфигурации](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration-ru-RU#Файл-глобальной-конфигурации)** `CommandPrefix`, значение которого по-умолчанию `!`. Это значит, что для запуска команды `status` на самом деле вам надо написать `!status` (или использовать другой `CommandPrefix`, который вы задали).

* * *

### Личный чат Steam

Определенно, самый простой способ взаимодействия с ASF - просто введите команду боту, который запущен в ASF. Естественно вы не можете этого сделать если ASF запущен с одним лишь вашим основным аккаунтом.

![Скриншот](https://i.imgur.com/PPxx7qV.png)

* * *

### Групповой чат Steam

Очень похоже на способ выше, но в этот раз взаимодействие происходит в чате группы Steam. Помните, что для этой опции требуется либо задать параметр `SteamMasterClanID`, либо пригласить бота в чат группы вручную. Также, этот способ можно использовать для отправки команд вашему основному аккаунту и не требует дополнительного бот-аккаунта.

* * *

### IPC

Наверное, самый "сложный" метод связи с ASF, идеален для инструментов или скриптов от сторонних разработчиков, требует запуска ASF в режиме сервера, а клиент запускает команды через интерфейс **[IPC](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC-ru-RU)**.

![Скриншот](https://i.imgur.com/TsAHcM0.png)

* * *

## Команды

| Команда                                              | Доступ          | Описание                                                                                                                                                                                                                               |
| ---------------------------------------------------- | --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `2fa <Bots>`                                   | `Master`        | Generates temporary **[2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Two-factor-authentication)** token for given bot instances.                                                                                               |
| `2fano <Bots>`                                 | `Master`        | Denies all pending **[2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Two-factor-authentication)** confirmations for given bot instances.                                                                                        |
| `2faok <Bots>`                                 | `Master`        | Accepts all pending **[2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Two-factor-authentication)** confirmations for given bot instances.                                                                                       |
| `addlicense <Bots> <GameIDs>`            | `Operator`      | Активирует указанные `appID` (сеть Steam) или `subID` (магазин Steam) на заданных ботах (только для бесплатных игр).                                                                                                                   |
| `bl <Bots>`                                    | `Master`        | Выводит список пользователей, обмены с которыми запрещены для заданных ботов.                                                                                                                                                          |
| `bladd <Bots> <SteamIDs64>`              | `Master`        | Запрещает заданным ботам обмениваться с указанными с помощью `steamID` пользователями.                                                                                                                                                 |
| `blrm <Bots> <SteamIDs64>`               | `Master`        | Снимает у заданных ботов запрет на обмен с указанными с помощью `steamID` пользователями.                                                                                                                                              |
| `exit`                                               | `Owner`         | Полностью завершает процесс ASF.                                                                                                                                                                                                       |
| `farm <Bots>`                                  | `Master`        | Перезапускает модуль фарма для заданных ботов.                                                                                                                                                                                         |
| `help`                                               | `FamilySharing` | Помощь по командам (ссылка на эту страницу).                                                                                                                                                                                           |
| `input <Bots> <Type> <Value>`      | `Master`        | Задаёт входные данные для указанного значения для заданных ботов, работает только в режиме `Headless` - подробное описание **[ниже](#Команда-input)**.                                                                                 |
| `ib <Bots>`                                    | `Master`        | Выводит список приложений, автоматический фарм которых запрещён для заданных ботов.                                                                                                                                                    |
| `ibadd <Bots> <AppIDs>`                  | `Master`        | Запрещает автоматический фарм приложений с указанными `appID` для заданных ботов.                                                                                                                                                      |
| `ibrm <Bots> <AppIDs>`                   | `Master`        | Снимает запрет на фарм приложений с указанными `appID` для заданных ботов.                                                                                                                                                             |
| `iq <Bots>`                                    | `Master`        | Выводит список приоритетной очереди фарма для заданных ботов.                                                                                                                                                                          |
| `iqadd <Bots> <AppIDs>`                  | `Master`        | Добавляет приложения с указанными `appIDs` в список приоритетной очереди фарма для заданных ботов.                                                                                                                                     |
| `iqrm <Bots> <AppIDs>`                   | `Master`        | Удаляет приложения с указанными `appIDs` из список приоритетной очереди фарма для заданных ботов.                                                                                                                                      |
| `leave <Bots>`                                 | `Master`        | Указание заданным ботам покинуть чат группы. По очевидным причинам, эта команда работает только в групповых чатах.                                                                                                                     |
| `loot <Bots>`                                  | `Master`        | Отправляет все предметы, соответствующие `MatchableTypes`, от заданных ботов к их `Master` заданному в `SteamUserPermissions` (с самым меньшим steamID, если их больше одного).                                                        |
| `loot^ <Bots> <AppID> <ContextID>` | `Master`        | Отправляет все предметы из инвентаря с заданными `AppID` и `ContextID` от заданных ботов их `Master` заданному в `SteamUserPermissions` (с самым меньшим steamID, если их больше одного).                                              |
| `loot& <Bots>`                             | `Master`        | Переключает отправку предметов с заданных ботов между между состояниями включено/выключено.                                                                                                                                            |
| `nickname <Bots> <Nickname>`             | `Master`        | Меняет имя профиля в Steam на указанный `nickname` для заданных ботов.                                                                                                                                                                 |
| `owns <Bots> <AppIDsOrGameNames>`        | `Operator`      | Проверяет, есть ли у заданных ботов в библиотеке приложения с указанными `appIDs` и/или названиями `gameNames` (может быть частью названия). Также можно указать `*` чтобы отобразить все имеющиеся игры.                              |
| `password <Bots>`                              | `Master`        | Выводит зашифрованные пароли для заданных ботов (используется с `PasswordFormat`).                                                                                                                                                     |
| `pause <Bots>`                                 | `Operator`      | Ставит на постоянную паузу модуль автоматического фарма для заданных ботов. ASF не будет пытаться фармить игры на этом аккаунте в этой сессии, если только вы вручную не снимте паузу командой `resume`, или не перезапустите процесс. |
| `pause~ <Bots>`                                | `FamilySharing` | Ставит на временную паузу модуль автоматического фарма для заданных ботов. Фарм будет продолжен автоматически по следующему игровому событию или при отключении бота. Вы можете использовать команду `resume` чтобы снять паузу.       |
| `pause& <Bots> <Seconds>`            | `Operator`      | Ставит на временную паузу модуль автоматического фарма для заданных ботов на `Seconds` секунд. После этого времени фарм будет продолжен автоматически.                                                                                 |
| `play <Bots> <AppIDs,GameName>`          | `Master`        | Переключается на ручной режим фарма - запускает указанные `AppIDs` на заданных ботах, опционально с отображением статуса `GameName`. Используйте команду `resume` чтобы вернуться к автоматическому фарму.                             |
| `privacy <Bots> <Settings>`              | `Master`        | Изменяет **[настройки приватности Steam](https://steamcommunity.com/my/edit/settings)** для заданных ботов на специальным образом заданные, подробное описание приведено **[ниже](#Настройки-приватности-privacy)**.                   |
| `redeem <Bots> <Keys>`                   | `Operator`      | Активирует указанные ключи на заданных ботах.                                                                                                                                                                                          |
| `redeem^ <Bots> <Modes> <Keys>`    | `Operator`      | Активирует ключи на заданных ботах, с использованием режимов `modes`, подробно описанных **[ниже](#Режимы-активации-redeem)**.                                                                                                         |
| `rejoinchat <Bots>`                            | `Operator`      | Указание заданным ботам переподключиться к чату группы, указанной в `SteamMasterClanID`.                                                                                                                                               |
| `restart`                                            | `Owner`         | Перезапускает процесс ASF.                                                                                                                                                                                                             |
| `resume <Bots>`                                | `FamilySharing` | Возобновляет автоматический фарм на заданных ботах. См. также `pause`, `play`.                                                                                                                                                         |
| `start <Bots>`                                 | `Master`        | Запускает заданных ботов.                                                                                                                                                                                                              |
| `stats`                                              | `Owner`         | Выводит статистику процесса, такую как объём занимаемой памяти.                                                                                                                                                                        |
| `status <Bots>`                                | `FamilySharing` | Выводит статус заданных ботов.                                                                                                                                                                                                         |
| `stop <Bots>`                                  | `Master`        | Останавливает заданных ботов.                                                                                                                                                                                                          |
| `transfer <Bots> <Modes> <Bot>`    | `Master`        | Отправляет боту `Bot` от заданных ботов все предметы инвентаря, соответствующие `modes`, описанным **[ниже](#Режимы-transfer)**.                                                                                                       |
| `unpack <Bots>`                                | `Master`        | Распаковывает все наборы карточек, находящиеся в инвентаре заданных ботов.                                                                                                                                                             |
| `update`                                             | `Owner`         | Проверяет наличие на GitHub новой версии ASF (это делается автоматически каждые `UpdatePeriod` часов).                                                                                                                                 |
| `version`                                            | `FamilySharing` | Выводит версию ASF.                                                                                                                                                                                                                    |

* * *

### Примечания

Все команды регистронезависимы, но их параметры (такие как имена ботов) обычно регистрозависимые.

Параметр `<Bots>` является необязательным для всех команд. Если он указан - команда будет исполнена на указанных ботах. When omitted, command is executed on current bot that receives the command. In other words, `status A` sent to bot `B` is the same as sending `status` to bot `A`.

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

| Команда      | Сокращение |
| ------------ | ---------- |
| `owns ASF`   | `oa`       |
| `status ASF` | `sa`       |
| `redeem`     | `r`        |
| `redeem^`    | `r^`       |

* * *

It's not required to have any extra account for executing commands though Steam chat - you can create a group, set `SteamMasterClanID` properly to that newly created group, then give yourself access either through `SteamOwnerID` or `SteamUserPermissions` of your own bot. This way ASF bot (you) will join group and chat of your selected group, and listen to commands from your own account. You can join the same group chatroom in order to issue commands to yourself (as you'll be sending command to chatroom, and ASF instance sitting on the same chatroom will receive them, even if it shows only as your account being there). Apart from that, you can also use **[IPC](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC)**, but chatroom way is much easier, and if you have access to some alt account, then using that instead is even easier.

* * *

When using **IPC**, keep in mind that:

- Commands don't have to be prefixed by `CommandPrefix`, ASF prefixes them for you automatically if needed
- When using commands that are based on `current bot instance`, ASF will choose **any** of currently enabled bots, therefore it's highly recommended to use `given bot instances` commands instead.

* * *

## Настройки приватности (`privacy`)

`<Settings>` argument accepts **up to 6** different options, separated as usual with standard comma ASF delimiter. Those are, in order:

| Argument | Имя            | Child of   |
| -------- | -------------- | ---------- |
| 1        | Profile        |            |
| 2        | OwnedGames     | Profile    |
| 3        | Playtime       | OwnedGames |
| 4        | Inventory      | Profile    |
| 5        | InventoryGifts | Inventory  |
| 6        | Comments       | Profile    |

For description of above fields, please visit **[Steam privacy settings](https://steamcommunity.com/my/edit/settings)**.

While valid values for all of them are:

| Значение | Имя           |
| -------- | ------------- |
| 1        | `Private`     |
| 2        | `FriendsOnly` |
| 3        | `Public`      |

You can use either a case-insensitive name, or a numeric value. Arguments that were omitted will default to being set to `Private`. It's important to note relation between child and parent of arguments specified above, as child can never have more open permission than its parent. For example, you **can't** have `Public` games owned while having `Private` profile.

### Пример

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

## Режимы активации `redeem^`

`redeem^` command allows you to fine-tune modes that will be used for one single redeem scenario. This works as temporary override of `RedeemingPreferences` **[bot config property](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#bot-config)**.

`<Modes>` argument accepts multiple mode values, separated as usual by a comma. Available mode values are specified below:

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

## Режимы `transfer`

`<Modes>` argument accepts multiple mode values, separated as usual by a comma. Available mode values are specified below:

| Значение   | Сокращение | Описание                                                                  |
| ---------- | ---------- | ------------------------------------------------------------------------- |
| All        | A          | Все типы предметов, указанные ниже                                        |
| Background | BG         | Фоны, используемые в вашем профиле Steam                                  |
| Booster    | BO         | Наборы карточек                                                           |
| Card       | C          | Коллекционные карточки Steam, используемые для создания значков (обычных) |
| Emoticon   | E          | Смайлики, используемые в чате Steam                                       |
| Foil       | F          | Аналог `Card`, но для металлических карточек                              |
| Gems       | G          | Самоцветы и мешки самоцветов, используемые для создания наборов карточек  |
| Unknown    | U          | Типы предметов, которые не попадают ни в одну из категорий выше           |

For example, in order to send trading cards and foils from `MyBot` to `MyMain`, you'd execute:

`transfer MyBot C,F MyMain`

* * *

## Команда `input`

Input command can be used only in `Headless` mode, for inputting given data via **[IPC](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC)** or Steam chat when ASF is running without support for user interaction.

General syntax is `input <Bots> <Type> <Value>`.

`<Type>` is case-insensitive and defines input type recognized by ASF. Currently ASF recognizes following types:

| Тип                     | Описание                                                                   |
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