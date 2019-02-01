# 指令

ASF supports variety of commands, which can be used to control behaviour of the process and bot instances.

Below commands can be sent to the bot through three different ways:

- Through steam private chat
- Through steam group chat
- Through **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)**

Keep in mind that ASF interaction requires from you to be eligible for the command according to ASF permissions. Check out `SteamUserPermissions` and `SteamOwnerID` config properties for more details.

All commands below are affected by `CommandPrefix` **[global configuration property](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#commandprefix)**, which is `!` by default. This means that for executing e.g. `status` command, you should actually write `!status` (or custom `CommandPrefix` of your choice that you set instead).

* * *

### Steam 私人交談

Definitely the easiest method to interact with ASF - simply execute command to ASF bot that is currently running in ASF process. Obviously, you can't do that if you're running ASF with a single bot account that is your own.

![Screenshot](https://i.imgur.com/PPxx7qV.png)

* * *

### Steam 群組交談

Very similar to above, but this time on group chat of given Steam group. Keep in mind that this option requires properly set `SteamMasterClanID` property, in which case bot will listen for commands also on group's chat (and join it if needed). This can also be used for "talking to yourself" since it doesn't require a dedicated bot account. You most likely don't want to use this method for more bots than 1.

* * *

### IPC

The most advanced and flexible way of executing commands, perfect for user interaction (ASF-ui) as well as third-party tools or scripting (ASF API), requires ASF to be run in `IPC` mode, and a client executing command through **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** interface.

![Screenshot](https://i.imgur.com/pzKE4EJ.png)

* * *

## 指令

| 命令                                                                         | 權限              | 描述                                                                                                                    |
| -------------------------------------------------------------------------- | --------------- | --------------------------------------------------------------------------------------------------------------------- |
| `2fa <Bots>`                                                         | `Master`        | 為指定機器人生成臨時​**[兩步驗證](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)**​令牌。              |
| `2fano <Bots>`                                                       | `Master`        | 為指定機器人拒絕所有待處理的​**[​兩步驗證](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)**​交易確認。       |
| `2faok <Bots>`                                                       | `Master`        | 為指定機器人接受所有待處理的​**[兩步驗證](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)**​交易確認。        |
| `addlicense <Bots> <GameIDs>`                                  | `Operator`      | 為指定機器人激活給定的 `appIDs`（Steam Network）或 `subIDs`（Steam 商店），僅適用於免費游戲。                                                     |
| `balance <Bots>`                                                     | `Master`        | 顯示指定機器人的 Steam 錢包餘額。                                                                                                  |
| `bl <Bots>`                                                          | `Master`        | 列出指定機器人的交易用戶黑名單。                                                                                                      |
| `bladd <Bots> <SteamIDs64>`                                    | `Master`        | 將特定的 `steamIDs` 加入指定機器人的交易用戶黑名單。                                                                                      |
| `blrm <Bots> <SteamIDs64>`                                     | `Master`        | 將特定的 `steamIDs `移出指定機器人的交易用戶黑名單。                                                                                      |
| `exit`                                                                     | `Owner`         | 完全終止ASF進程。                                                                                                            |
| `farm <Bots>`                                                        | `Master`        | 重啟指定機器人的掛卡模塊。                                                                                                         |
| `help`                                                                     | `FamilySharing` | 顯示幫助（指向此頁面的連結）。                                                                                                       |
| `input <Bots> <Type> <Value>`                            | `Master`        | 為指定機器人輸入特定欄位的值，僅在 `Headless` 模式中可用——詳見​**[下文](#input-command)**的解釋。                                                   |
| `ib <Bots>`                                                          | `Master`        | 列出指定機器人的自動掛卡游戲黑名單。                                                                                                    |
| `ibadd <Bots> <AppIDs>`                                        | `Master`        | 將特定的 `appIDs` 加入指定機器人的自動掛卡游戲黑名單。                                                                                      |
| `ibrm <Bots> <AppIDs>`                                         | `Master`        | 將特定的 `appIDs` 移出指定機器人的自動掛卡游戲黑名單。                                                                                      |
| `iq <Bots>`                                                          | `Master`        | 列出指定機器人的優先掛卡隊列。                                                                                                       |
| `iqadd <Bots> <AppIDs>`                                        | `Master`        | 將特定的 `appIDs` 加入指定機器人的優先掛卡隊列。                                                                                         |
| `iqrm <Bots> <AppIDs>`                                         | `Master`        | 將特定的 `appIDs` 移出指定機器人的優先掛卡隊列。                                                                                         |
| `level <Bots>`                                                       | `Master`        | 顯示指定機器人的 Steam 等級。                                                                                                    |
| `loot <Bots>`                                                        | `Master`        | 將指定機器人的所有 `LootableTypes` 社區物品交易給其 `SteamUserPermissions` 屬性中設置的 `Master` 用戶（如有多個則取 steamID 最小的）。                     |
| `loot@ <Bots> <RealAppIDs>`                                    | `Master`        | 將指定機器人的所有符合特定 `RealAppIDs` 的 `LootableTypes` 社區物品交易給其 `SteamUserPermissions` 屬性中設置的 `Master` 用戶（如果有多個則取 steamID 最小的）。 |
| `loot^ <Bots> <AppID> <ContextID>`                       | `Master`        | 將指定機器人的` ContextID` 庫存分類中符合特定 `AppID` 的物品交易給其 `SteamUserPermissions` 屬性中設置的 `Master` 用戶（如果有多個則取 steamID 最小的）。         |
| `nickname <Bots> <Nickname>`                                   | `Master`        | 將指定機器人的 Steam `nickname`更改為自定義暱稱。                                                                                     |
| `owns <Bots> <AppIDsOrGameNames>`                              | `Operator`      | 檢查指定機器人是否已擁有某游戲，支持查詢欄位： `appIDs` 和/或 `gameNames`（游戲名稱的一部分）。 也可以使用 `*` 顯示所有已擁有的游戲。                                     |
| `password <Bots>`                                                    | `Master`        | 顯示指定機器人加密後的密碼（配合 `PasswordFormat` 使用）。                                                                                |
| `pause <Bots>`                                                       | `Operator`      | 停止指定機器人的自動掛卡模塊。 ASF 在本次會話中將不會再嘗試對此帳戶進行掛卡，除非您手動 `resume` 或者重啟 ASF。                                                     |
| `pause~ <Bots>`                                                      | `FamilySharing` | 暫停指定機器人的自動掛卡模塊。 掛卡進程將會在下次游戲事件被觸發時或機器人斷開連接時自動恢復。 您可以` resume` 以恢復掛卡。                                                   |
| `pause& <Bots> <Seconds>`                                  | `Operator`      | 暂停指定机器人的自动挂卡模块 `seconds` 秒。 之後，掛卡將自動恢復。                                                                               |
| `play <Bots> <AppIDs,GameName>`                                | `Master`        | 切換至手動掛卡模式——使指定機器人運行特定的 `AppIDs`，並且可選自定義 `GameName` 為當前游戲名稱。 使用 `resume` 以返回自動掛卡模式。                                    |
| `privacy <Bots> <Settings>`                                    | `Master`        | 更改指定機器人的 **[Steam 隱私設置](https://steamcommunity.com/my/edit/settings)**，可用選項將於**[​下文](#privacy-settings)**詳述。          |
| `redeem <Bots> <Keys>`                                         | `Operator`      | 為指定機器人激活特定的游戲序列號 `Cd-Keys`。                                                                                           |
| `redeem^ <Bots> <Modes> <Keys>`                          | `Operator`      | 以 `Modes` 模式為指定機器人激活特定的游戲序列號 `Cd-Keys`，模式將於**[​下文](#redeem-modes)**詳述。                                                |
| `rejoinchat <Bots>`                                                  | `Operator`      | 命令指定機器人重新加入 `SteamMasterClanID` 中設置的群組交談。                                                                             |
| `restart`                                                                  | `Owner`         | 重啟 ASF 進程。                                                                                                            |
| `resume <Bots>`                                                      | `FamilySharing` | 恢復指定機器人的自動掛卡進程。 參見 `pause` 和 `play`。                                                                                  |
| `start <Bots>`                                                       | `Master`        | 啟動指定機器人。                                                                                                              |
| `stats`                                                                    | `Owner`         | 顯示進程統計信息，例如託管記憶體用量。                                                                                                   |
| `status <Bots>`                                                      | `FamilySharing` | 顯示指定機器人的狀態。                                                                                                           |
| `stop <Bots>`                                                        | `Master`        | 停止指定機器人的進程。                                                                                                           |
| `transfer <Bots> <TargetBot>`                                  | `Master`        | 將指定機器人的所有 `TransferableTypes` 社區物品交易至目標機器人。                                                                           |
| `transfer@ <Bots> <RealAppIDs> <TargetBot>`              | `Master`        | 將指定機器人的所有符合特定 `RealAppIDs` 的 `TransferableTypes` 社區物品交易至目標機器人。                                                        |
| `transfer^ <Bots> <AppID> <ContextID> <TargetBot>` | `Master`        | 將指定機器人的 `ContextID` 庫存分類中符合特定 `AppID` 的物品交易至目標機器人。                                                                    |
| `unpack <Bots>`                                                      | `Master`        | 拆開指定機器人庫存中的所有補充包。                                                                                                     |
| `update`                                                                   | `Owner`         | 檢查 GitHub 上的 ASF 更新（每 `UpdatePeriod` 自動執行一次）。                                                                         |
| `version`                                                                  | `FamilySharing` | 顯示 ASF 的版本號。                                                                                                          |

* * *

### 備註

All commands are case-insensitive, but their arguments (such as bot names) are usually case-sensitive.

`<Bots>` argument is optional in all commands. When specified, command is executed on given bots. When omitted, command is executed on current bot that receives the command. In other words, `status A` sent to bot `B` is the same as sending `status` to bot `A`.

**Access** of the command defines **minimum** `EPermission` of `SteamUserPermissions` that is required to use the command, with an exception of `Owner` which is `SteamOwnerID` defined in global configuration file (and highest permission available).

Plural arguments, such as `<Bots>`, `<Keys>` or `<AppIDs>` mean that command supports multiple arguments of given type, separated by a comma. For example, `status <Bots>` can be used as `status MyBot,MyOtherBot,Primary`. This will cause given command to be executed on **all target bots** in the same way as you'd send `status` to each bot in a separate chat window. Please note that there is no space after `,`.

ASF uses all whitespace characters as possible delimiters for a command, such as space and newline characters. This means that you don't have to use space for delimiting your arguments, you can as well use any other whitespace character (such as tab or new line).

ASF will "join" extra out-of-range arguments to plural type of the last in-range argument. This means that `redeem bot key1 key2 key3` for `redeem <Bots> <Keys>` will work exactly the same as `redeem bot key1,key2,key3`. Together with accepting newline as command delimiter, this makes it possible for you to write `redeem bot` then paste a list of keys separated by any acceptable delimiter character (such as newline), or standard `,` ASF delimiter. Keep in mind that this trick can be used only for command variant that uses the most amount of arguments (so specifying `<Bots>` is mandatory in this case).

As you've read above, a space character is being used as a delimiter for a command, therefore it can't be used in arguments. However, also as stated above, ASF can join out-of-range arguments, which means that you're actually able to use a space character in argument that is defined as a last one for given command. For example, `nickname bob Great Bob` will properly set nickname of `bob` bot to "Great Bob". In the similar way you can check names containing spaces in `owns` command.

Please note that sending a command to the group chat acts like a relay - if you're saying `redeem X` to 3 of your bots sitting together with you on the group chat, it'll result in the same as you'd say `redeem X` to every single one of them privately. In most cases **this is not what you want**, and instead you should use `given bot` command that is being sent to **a single bot in private window**. ASF supports group chat, as in many cases it can be useful source for communication with it, but you should almost never execute any command on the group chat if there are 2 or more ASF bots sitting there, unless you fully understand ASF behaviour written here and you in fact want to relay the same command to every single bot that is listening to you.

*And even in this case you should use private chat with `<Bots>` syntax instead.*

* * *

一些命令有較短的別名可用，便於輸入。

| 命令           | 別名   |
| ------------ | ---- |
| `owns ASF`   | `oa` |
| `status ASF` | `sa` |
| `redeem`     | `r`  |
| `redeem^`    | `r^` |

* * *

It's not required to have any extra account for executing commands though Steam chat - you can create a group, set `SteamMasterClanID` properly to that newly created group, then give yourself access either through `SteamOwnerID` or `SteamUserPermissions` of your own bot. This way ASF bot (you) will join group and chat of your selected group, and listen to commands from your own account. You can join the same group chatroom in order to issue commands to yourself (as you'll be sending command to chatroom, and ASF instance sitting on the same chatroom will receive them, even if it shows only as your account being there). Apart from that, you can also use **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)**, but chatroom way is much easier, and if you have access to some alt account, then using that instead is even easier.

* * *

### `<Bots>`参数

`<Bots>` argument is a special variant of plural argument, as in addition to accepting multiple values it also offers extra functionality.

First and foremost, there is a special `ASF` keyword which acts as "all bots in the process", so `status ASF` command is equal to `status all,your,bots,listed,here`. This can also be used to easily identify the bots that you have access to, as `ASF` keyword, despite of targeting all bots, will result in response only from those bots that you can actually send the command to.

`<Bots>` argument supports special "range" syntax, which allows you to choose a range of bots more easily. The general syntax for `<Bots>` in this case is `firstBot..lastBot`. For example, if you have bots named `A, B, C, D, E, F`, you can execute `status B..E`, which is equal to `status B,C,D,E` in this case. When using this syntax, ASF will use alphabetical sorting in order to determine which bots are in your specified range. Both `firstBot` and `lastBot` must be valid bot names recognized by ASF, otherwise range syntax is entirely skipped.

In addition to range syntax above, `<Bots>` argument also supports **[regex](https://en.wikipedia.org/wiki/Regular_expression)** matching. You can activate regex pattern by using `r!<pattern>` as a bot name, where `r!` is ASF activator for regex matching, and `<pattern>` is your regex pattern. An example of a regex-based bot command would be `status r!\d{3}` which will send `status` command to bots that have a name made out of 3 digits (e.g. `123` and `981`). Feel free to take a look at the **[docs](https://docs.microsoft.com/en-us/dotnet/standard/base-types/regular-expression-language-quick-reference)** for further explanation and more examples of available regex patterns.

* * *

## `privacy` 設置

`<Settings>`參數擁有**多至 7 個**不同的選項，使用 ASF 標準的逗號分隔格式。 這些選項分別是：

| 參數 | 名稱             | 從屬於        |
| -- | -------------- | ---------- |
| 1  | Profile        |            |
| 2  | OwnedGames     | Profile    |
| 3  | Playtime       | OwnedGames |
| 4  | FriendsList    | Profile    |
| 5  | Inventory      | Profile    |
| 6  | InventoryGifts | Inventory  |
| 7  | Comments       | Profile    |

關於上述選項的說明，請訪問 **[Steam 隱私設置](https://steamcommunity.com/my/edit/settings)**。

每個選項的有效值可以是：

| 值 | 名稱            |
| - | ------------- |
| 1 | `Private`     |
| 2 | `FriendsOnly` |
| 3 | `Public`      |

您可以使用它們的名稱（不區分大小寫）或者數值。 未賦值的參數將會被設置為預設值 `Private`。 It's important to note relation between child and parent of arguments specified above, as child can never have more open permission than its parent. For example, you **can't** have `Public` games owned while having `Private` profile.

### 範例

如果您希望將機器人 `Main` 的**所有**隱私設置都設置為 `Private`，可以使用以下任一命令：

    privacy Main 1
    privacy Main Private
    

這是因為 ASF 會預設所有未賦值選項為 `Private`，所以您不需要全部寫出它們。 另一方面，如果您希望設置所有選項為 `Public`，可以使用以下任一命令：

    privacy Main 3,3,3,3,3,3,3
    privacy Main Public,Public,Public,Public,Public,Public,Public
    

也可以為每個選項設置不同的值：

    privacy Main Public,FriendsOnly,Private,Public,Public,Private,Public
    

The above will set profile to public, owned games to friends only, playtime to private, friends list to public, inventory to public, inventory gifts to private and profile comments to public. You can achieve the same with numeric values if you want to.

Remember that child can never have more open permission than its parent. Refer to arguments relationship for available options.

* * *

## `redeem^`模式

`redeem^` command allows you to fine-tune modes that will be used for one single redeem scenario. This works as temporary override of `RedeemingPreferences` **[bot config property](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#bot-config)**.

`<Modes>` argument accepts multiple mode values, separated as usual by a comma. Available mode values are specified below:

| 值    | 名稱                    | 描述                                                                    |
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

It's important to note that advanced redeem overrides only those `RedeemingPreferences` that you **specify in the command**. For example, if you've enabled `Distributing` in your `RedeemingPreferences` then there will be no difference whether you use `FD` mode or not, because distributing will be already active regardless, due to `RedeemingPreferences` that you use. This is why each forcibly enabled override also has a forcibly disabled one, you can decide yourself if you prefer to override disabled with enabled, or vice versa.

* * *

## `input` 命令

Input command can be used only in `Headless` mode, for inputting given data via **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** or Steam chat when ASF is running without support for user interaction.

General syntax is `input <Bots> <Type> <Value>`.

`<Type>` is case-insensitive and defines input type recognized by ASF. Currently ASF recognizes following types:

| 類型                      | 描述                                                                         |
| ----------------------- | -------------------------------------------------------------------------- |
| DeviceID                | 2FA device identificator, if missing from `.maFile`.                       |
| Login                   | `SteamLogin` bot config property, if missing from config.                  |
| Password                | `SteamPassword` bot config property, if missing from config.               |
| SteamGuard              | 如果您未啟用2FA，驗證碼將以電子郵件的方式發送。                                                  |
| SteamParentalCode       | `SteamParentalCode` bot config property, if missing from config.           |
| TwoFactorAuthentication | 2FA token generated from your mobile, if you're using 2FA but not ASF 2FA. |

`<Value>`是要為指定類型設置的值。 目前所有的值都是字元串。

### 範例

Let's say that we have a bot that is protected by SteamGuard in non-2FA mode. We want to launch that bot with `Headless` set to true.

In order to do that, we need to execute following commands:

`start MySteamGuardBot` -> Bot will attempt to log in, fail due to AuthCode needed, then stop due to running in `Headless` mode. We need this in order to make Steam network send us auth code on our e-mail - if there was no need for that, we'd skip this step entirely.

`input MySteamGuardBot SteamGuard ABCDE` -> We set `SteamGuard` input of `MySteamGuardBot` bot to `ABCDE`. Of course, `ABCDE` in this case is auth code that we got on our e-mail.

`start MySteamGuardBot` -> We start our (stopped) bot again, this time it automatically uses auth code that we set in previous command, properly logging in, then clearing it.

In the same way we can access 2FA-protected bots (if they're not using ASF 2FA), as well as setting other required properties during runtime.