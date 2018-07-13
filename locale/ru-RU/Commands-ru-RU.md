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

| Команда                                              | Доступ          | Описание                                                                                                                                                                                                                                  |
| ---------------------------------------------------- | --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `2fa <Bots>`                                   | `Master`        | Создает временный код **[2ФА](https://github.com/JustArchi/ArchiSteamFarm/wiki/Two-factor-authentication-ru-RU)**(двухфакторной аутентификации) для данных ботов.                                                                         |
| `2fano <Bots>`                                 | `Master`        | Отменяет для заданных ботов все предложения обмена и лоты на торговой площадке, ожидающие подтверждения по **[2ФА](https://github.com/JustArchi/ArchiSteamFarm/wiki/Two-factor-authentication-ru-RU)**.                                   |
| `2faok <Bots>`                                 | `Master`        | Подтверждает для заданных ботов все предложения обмена и лоты на торговой площадке, ожидающие подтверждения по **[2ФА](https://github.com/JustArchi/ArchiSteamFarm/wiki/Two-factor-authentication-ru-RU)**.                               |
| `addlicense <Bots> <GameIDs>`            | `Operator`      | Активирует указанные `appID` (сеть Steam) или `subID` (магазин Steam) на заданных ботах (только для бесплатных игр).                                                                                                                      |
| `bl <Bots>`                                    | `Master`        | Выводит список пользователей, обмены с которыми запрещены для заданных ботов.                                                                                                                                                             |
| `bladd <Bots> <SteamIDs64>`              | `Master`        | Запрещает заданным ботам обмениваться с указанными с помощью `steamID` пользователями.                                                                                                                                                    |
| `blrm <Bots> <SteamIDs64>`               | `Master`        | Снимает у заданных ботов запрет на обмен с указанными с помощью `steamID` пользователями.                                                                                                                                                 |
| `exit`                                               | `Owner`         | Полностью завершает процесс ASF.                                                                                                                                                                                                          |
| `farm <Bots>`                                  | `Master`        | Перезапускает модуль фарма для заданных ботов.                                                                                                                                                                                            |
| `help`                                               | `FamilySharing` | Помощь по командам (ссылка на эту страницу).                                                                                                                                                                                              |
| `input <Bots> <Type> <Value>`      | `Master`        | Задаёт входные данные для указанного значения для заданных ботов, работает только в режиме `Headless` - подробное описание **[ниже](#Команда-input)**.                                                                                    |
| `ib <Bots>`                                    | `Master`        | Выводит список приложений, автоматический фарм которых запрещён для заданных ботов.                                                                                                                                                       |
| `ibadd <Bots> <AppIDs>`                  | `Master`        | Запрещает автоматический фарм приложений с указанными `appID` для заданных ботов.                                                                                                                                                         |
| `ibrm <Bots> <AppIDs>`                   | `Master`        | Снимает запрет на фарм приложений с указанными `appID` для заданных ботов.                                                                                                                                                                |
| `iq <Bots>`                                    | `Master`        | Выводит список приоритетной очереди фарма для заданных ботов.                                                                                                                                                                             |
| `iqadd <Bots> <AppIDs>`                  | `Master`        | Добавляет приложения с указанными `appIDs` в список приоритетной очереди фарма для заданных ботов.                                                                                                                                        |
| `iqrm <Bots> <AppIDs>`                   | `Master`        | Удаляет приложения с указанными `appIDs` из список приоритетной очереди фарма для заданных ботов.                                                                                                                                         |
| `leave <Bots>`                                 | `Master`        | Указание заданным ботам покинуть чат группы. По очевидным причинам, эта команда работает только в групповых чатах.                                                                                                                        |
| `loot <Bots>`                                  | `Master`        | Отправляет все предметы Сообщества, соответствующие `LootableTypes`, от заданных ботов к их `Master` заданному в `SteamUserPermissions` (с самым меньшим steamID, если их больше одного).                                                 |
| `loot@ <Bots> <RealAppIDs>`              | `Master`        | Отправляет все предметы Сообщества, соответствующие `LootableTypes`, из игры с `appID` равным заданному `RealAppIDs` от заданных ботов к их `Master` заданному в `SteamUserPermissions` (с самым меньшим steamID, если их больше одного). |
| `loot^ <Bots> <AppID> <ContextID>` | `Master`        | Отправляет все предметы из инвентаря с заданными `AppID` и `ContextID` от заданных ботов их `Master` заданному в `SteamUserPermissions` (с самым меньшим steamID, если их больше одного).                                                 |
| `loot& <Bots>`                             | `Master`        | Переключает отправку предметов с заданных ботов между между режимами включено/выключено.                                                                                                                                                  |
| `nickname <Bots> <Nickname>`             | `Master`        | Меняет имя профиля в Steam на указанный `nickname` для заданных ботов.                                                                                                                                                                    |
| `owns <Bots> <AppIDsOrGameNames>`        | `Operator`      | Проверяет, есть ли у заданных ботов в библиотеке приложения с указанными `appIDs` и/или названиями `gameNames` (может быть частью названия). Также можно указать `*` чтобы отобразить все имеющиеся игры.                                 |
| `password <Bots>`                              | `Master`        | Выводит зашифрованные пароли для заданных ботов (используется с `PasswordFormat`).                                                                                                                                                        |
| `pause <Bots>`                                 | `Operator`      | Ставит на постоянную паузу модуль автоматического фарма для заданных ботов. ASF не будет пытаться фармить игры на этом аккаунте в этой сессии, если только вы вручную не снимте паузу командой `resume`, или не перезапустите процесс.    |
| `pause~ <Bots>`                                | `FamilySharing` | Ставит на временную паузу модуль автоматического фарма для заданных ботов. Фарм будет продолжен автоматически по следующему игровому событию или при отключении бота. Вы можете использовать команду `resume` чтобы снять паузу.          |
| `pause& <Bots> <Seconds>`            | `Operator`      | Ставит на временную паузу модуль автоматического фарма для заданных ботов на `Seconds` секунд. После этого времени фарм будет продолжен автоматически.                                                                                    |
| `play <Bots> <AppIDs,GameName>`          | `Master`        | Переключается на ручной режим фарма - запускает указанные `AppIDs` на заданных ботах, опционально с отображением статуса `GameName`. Используйте команду `resume` чтобы вернуться к автоматическому фарму.                                |
| `privacy <Bots> <Settings>`              | `Master`        | Изменяет **[настройки приватности Steam](https://steamcommunity.com/my/edit/settings)** для заданных ботов на специальным образом заданные, подробное описание приведено **[ниже](#Настройки-приватности-privacy)**.                      |
| `redeem <Bots> <Keys>`                   | `Operator`      | Активирует указанные ключи на заданных ботах.                                                                                                                                                                                             |
| `redeem^ <Bots> <Modes> <Keys>`    | `Operator`      | Активирует ключи на заданных ботах, с использованием режимов `modes`, подробно описанных **[ниже](#Режимы-активации-redeem)**.                                                                                                            |
| `rejoinchat <Bots>`                            | `Operator`      | Указание заданным ботам переподключиться к чату группы, указанной в `SteamMasterClanID`.                                                                                                                                                  |
| `restart`                                            | `Owner`         | Перезапускает процесс ASF.                                                                                                                                                                                                                |
| `resume <Bots>`                                | `FamilySharing` | Возобновляет автоматический фарм на заданных ботах. См. также `pause`, `play`.                                                                                                                                                            |
| `start <Bots>`                                 | `Master`        | Запускает заданных ботов.                                                                                                                                                                                                                 |
| `stats`                                              | `Owner`         | Выводит статистику процесса, такую как объём занимаемой памяти.                                                                                                                                                                           |
| `status <Bots>`                                | `FamilySharing` | Выводит статус заданных ботов.                                                                                                                                                                                                            |
| `stop <Bots>`                                  | `Master`        | Останавливает заданных ботов.                                                                                                                                                                                                             |
| `transfer <Bots> <Modes> <Bot>`    | `Master`        | Отправляет боту `Bot` от заданных ботов все предметы инвентаря, соответствующие `modes`, описанным **[ниже](#Режимы-transfer)**.                                                                                                          |
| `unpack <Bots>`                                | `Master`        | Распаковывает все наборы карточек, находящиеся в инвентаре заданных ботов.                                                                                                                                                                |
| `update`                                             | `Owner`         | Проверяет наличие на GitHub новой версии ASF (это делается автоматически каждые `UpdatePeriod` часов).                                                                                                                                    |
| `version`                                            | `FamilySharing` | Выводит версию ASF.                                                                                                                                                                                                                       |

* * *

### Примечания

Все команды регистронезависимы, но их параметры (такие как имена ботов) обычно регистрозависимые.

Параметр `<Bots>` является необязательным для всех команд. Если он указан - команда будет исполнена на указанных ботах. Если этот параметр пропущен, команда исполняется на боте, получившем команду. Другими словами, команда `status A` отправленная боту `B` это то же самое что команда `status`, отправленная боту `A`.

**Доступ** к команде означет **минимальное** `EPermission` в параметре `SteamUserPermissions`, которое необходимо для использования этой команды, за исключением `Owner`, которое соответствует `SteamOwnerID` заданному в файле глобальной конфигурации (и являющееся максимальным уровнем доступа).

Параметры во множественном числе, такие как `<Bots>`, `<Keys>` или `<AppIDs>` означают что команда поддерживает несколько аргументов данного типа, разделённых запятыми. Например, `status <Bots>` может быть использовано в виде `status MyBot,MyOtherBot,Primary`. Это приведёт к исполнению команды на **всех заданных ботах**, как есл и бы вы послали `status` каждому боту в отдельном окне чата. Обратите внимание, что после запятой (`,`) нет пробела.

ASF использует все пробельные символы как возможные разделители команд, такие как пробел или перевод строки. Это означает, что вам не обязательно использовать пробел для разделения параметров, вы можете использовать любой пробельный символ (такой как табуляция или перевод строки).

ASF "объединяет" лишние параметры в типы-множества последнего параметра правильного параметра. Это означает, что `redeem bot key1 key2 key3` для команды `redeem <Bots> <Keys>` будет работать точно так же, как `redeem bot key1,key2,key3`. В сочетании с возможностью использовать символ перевода строки как разделитель, у вас появляется возможность написать `redeem bot`, затем вставить список ключей с любым допустимым разделителем (таким как перевод строки) или стандартным разделителем (`,`). Помните, что этот трюк сработает только с вариантами команд, которые используют наибольшее количество параметров.

Как вы прочли выше, символ пробела используется как разделитель в командах, и поэтому не может использоваться в параметрах. Однако, как тоже сказано выше, ASF может объединять лишние параметры, что означает что вы можете использовать пробел в параметрах, которые определены последними для данной команды. Например, `nickname bob Great Bob` правильно установит для бота `bob` имя профиля "Great Bob". Похожим образом вы можете проверять имена, содержащие пробелы, в команде `owns`.

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

- К командам не нужно добавлять префикс `CommandPrefix`, ASF при необходимости добавит его автоматически
- При использовании комманд, адресованых `текущему боту`, ASF может выбрать **любого** из активных ботов, поэтому настоятельно рекомендуется использовать команды с указанием `имени бот(ов)`.

* * *

### `<Bots>` argument

`<Bots>` argument is a special variant of plural argument, as in addition to accepting multiple values it also offers extra functionality.

First and foremost, there is a special `ASF` keyword which acts as "all bots in the process", so `status ASF` command is equal to `status all,your,bots,listed,here`. This can also be used to easily identify the bots that you have access to, as `ASF` keyword, despite of targetting all bots, will result in response only from those bots that you can actually send the command to.

`<Bots>` argument supports special "range" syntax, which allows you to choose a range of bots more easily. The general syntax for `<Bots>` in this case is `firstBot..lastBot`. For example, if you have bots named `A, B, C, D, E, F`, you can execute `status B..E`, which is equal to `status B,C,D,E` in this case. When using this syntax, ASF will use alphabetical sorting in order to determine which bots are in your specified range. Both `firstBot` and `lastBot` must be valid bot names recognized by ASF, otherwise range syntax is entirely skipped.

In addition to range syntax above, `<Bots>` argument also supports **[regex](https://en.wikipedia.org/wiki/Regular_expression)** matching. You can activate regex pattern by using `r!<pattern>` as a bot name, where `r!` is ASF activator for regex matching (case insensitive), and `<pattern>` is your regex pattern. An example of a regex-based bot command would be `status r!\d{3}` which will send `status` command to bots that have a name made out of 3 digits (e.g. `123` and `981`). Feel free to take a look at the **[docs](https://docs.microsoft.com/en-us/dotnet/standard/base-types/regular-expression-language-quick-reference)** for further explanation and more examples of available regex patterns.

* * *

## Настройки приватности (`privacy`)

`<Settings>` argument accepts **up to 6** different options, separated as usual with standard comma ASF delimiter. Those are, in order:

| Параметр | Имя                | Наследуется от     |
| -------- | ------------------ | ------------------ |
| 1        | Профиль            |                    |
| 2        | Игровая Информация | Профиль            |
| 3        | Время в игре       | Игровая Информация |
| 4        | Инвентарь          | Профиль            |
| 5        | Инвентарь подарков | Инвентарь          |
| 6        | Комментарии        | Профиль            |

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

| Значение | Имя                   | Описание                                                                    |
| -------- | --------------------- | --------------------------------------------------------------------------- |
| FD       | ForceDistributing     | Принудительно включает режим `Distributing` для активации                   |
| FF       | ForceForwarding       | Принудительно включает режим `Forwarding` для активации                     |
| FKMG     | ForceKeepMissingGames | Принудительно включает режим `KeepMissingGames` для активации               |
| SD       | SkipDistributing      | Принудительно отключает режим `Distributing` для активации                  |
| SF       | SkipForwarding        | Принудительно отключает режим `Forwarding` для активации                    |
| SI       | SkipInitial           | Отключает активацию ключа на исходном боте                                  |
| SKMG     | SkipKeepMissingGames  | Принудительно отключает режим `KeepMissingGames` для активации              |
| V        | Validate              | Проводить проверку формата ключей и не пытаться активировать неверные ключи |

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

| Тип                     | Описание                                                                                                           |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------ |
| DeviceID                | Идентификатор устройства 2FA, на случай если он отсутствует в `.maFile`.                                           |
| Login                   | Логин, замена параметра `SteamLogin`, если он отсутствует в файле конфигурации.                                    |
| Password                | Пароль, замена параметра `SteamPassword`, если он отсутствует в файле конфигурации.                                |
| SteamGuard              | Код авторизации, присылаемый вам на e-mail в случае когда вы не пользуетесь 2ФА.                                   |
| SteamParentalPIN        | ПИН для семейного просмотра, замена параметра `SteamParentalPIN` в случае когда он не указан в файле конфигурации. |
| TwoFactorAuthentication | Код 2ФА, генерируемый на вашем мобильном устройстве, если вы пользуетесь 2ФА на не импортировали его в 2ФА ASF.    |

`<Value>` is value set for given type. Currently all values are strings.

### Пример

Let's say that we have a bot that is protected by SteamGuard in non-2FA mode. We want to launch that bot with `Headless` set to true.

In order to do that, we need to execute following commands:

`start MySteamGuardBot` -> Bot will attempt to log in, fail due to AuthCode needed, then stop due to running in `Headless` mode. We need this in order to make Steam network send us auth code on our e-mail.

`input MySteamGuardBot SteamGuard ABCDE` -> We set `SteamGuard` input of `MySteamGuardBot` bot to `ABCDE`. Of course, `ABCDE` in this case is auth code that we got on our e-mail.

`start MySteamGuardBot` -> We start our (stopped) bot again, this time it automatically uses auth code that we set in previous command, properly logging in, then clearing it.

In the same way we can access 2FA-protected bots (if they're not using ASF 2FA), as well as setting other required properties during runtime.