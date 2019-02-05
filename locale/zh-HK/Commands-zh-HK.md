# 命令

ASF支援各種命令, 這些命令可用於控制進程和機器人實例的行為。

您可以通過以下三種不同的方式發送命令：

- 通過Steam私人聊天
- 通過Steam群組聊天
- 通過 **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)**

請注意，與 ASF 交互需要您擁有執行相關指令的許可權。 查看 `SteamUserPermissions` 和 `SteamOwnerID` 配置屬性瞭解更多。

以下的所有命令都受 `CommandPrefix`**[ 全域配置屬性​影響](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#commandprefix)**，該屬性的預設值為`!`。 這意味著，當您要執行 `status` 命令時，實際應該發送 `!status`（或者使用您自訂的 `CommandPrefix`）。

* * *

### Steam 私人交談

與 ASF 交互的最簡單方式——向正在運行於 ASF 進程中的機器人發送命令。 顯然，如果您只在 ASF 中運行您自己的帳戶，就無法使用這個方法。

![截圖](https://i.imgur.com/PPxx7qV.png)

* * *

### Steam 群組交談

與上述方法非常相似，但這次需要向指定 Steam 群組的聊天室發送消息。 請注意，此選項需要您正確設置 `SteamMasterClanID` 屬性，使機器人同樣監聽（並加入）指定的群組聊天。 因為這種方法不需要額外的帳戶，所以可以用來“與自己交談”。 如果您有多個機器人，可能就不希望使用此方法。

* * *

### IPC

這是最先進、靈活的執行命令方式，非常適合使用者集成（ASF-ui）或者第三方工具腳本（ASF API）。這種方式需要 ASF 運行在 `IPC` 模式下，並且客戶端需要通過 **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** 介面來執行命令。

![截圖](https://i.imgur.com/pzKE4EJ.png)

* * *

## 指令：

| 指令：                                                                        | 權限              | 描述                                                                                                                      |
| -------------------------------------------------------------------------- | --------------- | ----------------------------------------------------------------------------------------------------------------------- |
| `2fa <Bots>`                                                         | `Master`        | 為指定機器人生成臨時​**[兩步驗證](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)**​代碼。                |
| `2fano <Bots>`                                                       | `Master`        | 為指定機器人拒絕所有待處理的​**[​兩步驗證](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)**​交易確認。         |
| `2faok <Bots>`                                                       | `Master`        | 為指定機器人接受所有待處理的​**[兩步驗證](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)**​交易確認。          |
| `addlicense <Bots> <GameIDs>`                                  | `Operator`      | 為指定機器人激活給定的 `appIDs`（Steam Network）或 `subIDs`（Steam 商店），僅適用於免費游戲。                                                       |
| `balance <Bots>`                                                     | `Master`        | 顯示指定機器人的 Steam 錢包餘額。                                                                                                    |
| `bl <Bots>`                                                          | `Master`        | 列出指定機器人的交易用戶黑名單。                                                                                                        |
| `bladd <Bots> <SteamIDs64>`                                    | `Master`        | 將特定的 `steamIDs` 加入指定機器人的交易用戶黑名單。                                                                                        |
| `blrm <Bots> <SteamIDs64>`                                     | `Master`        | 將特定的 `steamIDs `移出指定機器人的交易用戶黑名單。                                                                                        |
| `退出`                                                                       | `Owner`         | 完全終止ASF進程。                                                                                                              |
| `farm <Bots>`                                                        | `Master`        | 重啟指定機器人的掛卡模塊。                                                                                                           |
| `help`                                                                     | `FamilySharing` | 顯示幫助（指向此頁面的連結）。                                                                                                         |
| `輸入 <Bots> <Type> <Value>`                               | `Master`        | 為指定機器人輸入特定欄位的值，僅在 `Headless` 模式中可用——詳見​**[下文](#input-command)**的解釋。                                                     |
| `ib <Bots>`                                                          | `Master`        | 列出指定機器人的自動掛卡遊戲黑名單。                                                                                                      |
| `ibadd <Bots> <AppIDs>`                                        | `Master`        | 將特定的 `appIDs` 加入指定機器人的自動掛卡遊戲黑名單。                                                                                        |
| `ibrm <Bots> <AppIDs>`                                         | `Master`        | 將特定的 `appIDs` 移出指定機器人的自動掛卡遊戲黑名單。                                                                                        |
| `iq <Bots>`                                                          | `Master`        | 列出指定機器人的優先掛卡佇列。                                                                                                         |
| `iqadd <Bots> <AppIDs>`                                        | `Master`        | 將特定的 `appIDs` 加入指定機器人的優先掛卡佇列。                                                                                           |
| `iqrm <Bots> <AppIDs>`                                         | `Master`        | 將特定的 `appIDs` 移出指定機器人的優先掛卡佇列。                                                                                           |
| `level <Bots>`                                                       | `Master`        | 顯示指定機器人的 Steam 等級。                                                                                                      |
| `loot <Bots>`                                                        | `Master`        | 將指定機器人的所有 `LootableTypes` 社區物品發送給其 `SteamUserPermissions` 屬性中設置的 `Master` 用戶（如有多個則取 steamID 最小的）。                       |
| `loot@ <Bots> <RealAppIDs>`                                    | `Master`        | 將指定機器人的所有符合特定 `RealAppIDs` 的 `LootableTypes` 社區物品發送給其 `SteamUserPermissions` 屬性中設置的 `Master` 用戶（如果有多個則取 steamID 最小的）。   |
| `loot^ <Bots> <AppID> <ContextID>`                       | `Master`        | 將指定機器人的` ContextID` 庫存分類中符合特定 `AppID` 的物品發送給其 `SteamUserPermissions` 屬性中設置的 `Master` 用戶（如果有多個則取 steamID 最小的）。           |
| `nickname <Bots> <Nickname>`                                   | `Master`        | 將指定機器人的 Steam `nickname`更改為自訂昵稱。                                                                                        |
| `owns <Bots> <AppIDsOrGameNames>`                              | `Operator`      | 檢查指定機器人是否已擁有某遊戲，支持查詢欄位： `appIDs` 和/或 `gameNames`（可以是遊戲名稱的一部分）。 也可以使用 `*` 顯示所有已擁有的遊戲。                                    |
| `password <Bots>`                                                    | `Master`        | 顯示指定機器人加密後的密碼（配合 `PasswordFormat` 使用）。                                                                                  |
| `pause <Bots>`                                                       | `Operator`      | 停止指定機器人的自動掛卡模組。 ASF 在本次會話中將不會再嘗試對此帳戶進行掛卡，除非您手動 `resume` 或者重啟 ASF。                                                       |
| `pause~ <Bots>`                                                      | `FamilySharing` | 暫停指定機器人的自動掛卡模組。 掛卡進程將會在下次遊戲事件被觸發時或機器人斷開連接時自動恢復。 您可以` resume` 以恢復掛卡。                                                     |
| `pause& <Bots> <Seconds>`                                  | `Operator`      | 暂停指定机器人的自动挂卡模块 `seconds` 秒。 之後，掛卡將自動恢復。                                                                                 |
| `play <Bots> <AppIDs,GameName>`                                | `Master`        | 切換至手動掛卡模式——使指定機器人運行特定的 `AppIDs`，並且可選自訂 `GameName` 為當前遊戲名稱。 使用 `resume` 以返回自動掛卡模式。                                       |
| `privacy <Bots> <Settings>`                                    | `Master`        | 更改指定機器人的 **[Steam 隱私設置](https://steamcommunity.com/my/edit/settings)**，可用選項將於**[​下文](#privacy-settings)**詳述。            |
| `redeem <Bots> <Keys>`                                         | `Operator`      | Redeems given cd-keys or wallet codes on given bot instances.                                                           |
| `redeem^ <Bots> <Modes> <Keys>`                          | `Operator`      | Redeems given cd-keys or wallet codes on given bot instances, using given `modes` explained **[below](#redeem-modes)**. |
| `rejoinchat <Bots>`                                                  | `Operator`      | 強制指定機器人重新加入 `SteamMasterClanID` 中設置的群組交談。                                                                               |
| `重啟`                                                                       | `Owner`         | 重啟 ASF 進程。                                                                                                              |
| `resume <Bots>`                                                      | `FamilySharing` | 恢復指定機器人的自動掛卡進程。 另請參閱 `pause`, `play`。                                                                                   |
| `start <Bots>`                                                       | `Master`        | 啟動指定機器人。                                                                                                                |
| `stats`                                                                    | `Owner`         | 顯示進程統計信息，例如託管記憶體用量。                                                                                                     |
| `status <Bots>`                                                      | `FamilySharing` | 顯示指定機器人的狀態。                                                                                                             |
| `stop <Bots>`                                                        | `Master`        | 停止指定機器人的進程。                                                                                                             |
| `transfer <Bots> <TargetBot>`                                  | `Master`        | 將指定機器人的所有 `TransferableTypes` 社區物品交發送至目標機器人。                                                                            |
| `transfer@ <Bots> <RealAppIDs> <TargetBot>`              | `Master`        | 將指定機器人的所有符合特定 `RealAppIDs` 的 `TransferableTypes` 社區物品發送至目標機器人。                                                          |
| `transfer^ <Bots> <AppID> <ContextID> <TargetBot>` | `Master`        | 將指定機器人的 `ContextID` 庫存分類中符合特定 `AppID` 的物品發送至目標機器人。                                                                      |
| `unpack <Bots>`                                                      | `Master`        | 打開指定機器人物品庫中的所有擴充包。                                                                                                      |
| `update`                                                                   | `Owner`         | 檢查 GitHub 上的 ASF 更新（每 `UpdatePeriod` 自動執行一次）。                                                                           |
| `version`                                                                  | `FamilySharing` | 顯示 ASF 的版本號。                                                                                                            |

* * *

### 筆記

所有的指令都不區分大小寫，但它們的參數（例如機器人名稱）通常是區分大小寫的。

`<Bots>` 參數在所有指令中都是可選的。 當指定該參數時，指令會在指定的機器人上執行。 但省略時，指令會在當前接收指令的機器人上執行。 換句話說，向機器人 `B` 發送 `status A`指令等於向機器人 `A` 發送 `status` 指令。

命令的**Access**定義了需要執行此命令所需的**最低**許可權，即 `SteamUserPermissions `中定義的 `EPermission`，例外情況是 `Owner` 指全局配置檔案中的 `SteamOwnerID` 用戶（擁有最高許可權）。

複數參數，例如 `<Bots>`，`<Keys>`或`<AppIDs>`意味著該參數支援多個由逗號分隔的同類型值。 例如，`status<Bots>` 命令支援 `status MyBot,MyOtherBot,Primary `形式的用法。 這樣，該命令會在**所有目標機器人**上執行，效果等同分別向所有機器人單獨發送 `status`命令。 需要注意的是`,`後面不能有空格。

ASF 使用所有空白字元作為命令的分隔符號，例如空格和換行符。 這意味著您不僅可以使用空格來分隔參數, 還可以使用任何其他空白字元 (如選項卡或換行符號)。

ASF 會將命令末尾超出規定範圍的多餘參數“聯接”到符合語法規定的最後一個參數上。 這意味著，對於 `redeem <Bots><Keys>`命令，`redeem bot key1 key2 key3` 與 `redeem bot key1,key2,key3` 的工作原理完全相同。 當您同時使用換行作為命令分隔符號, 就可編寫 < 0>redeem bot< 0 >, 然後粘貼由任何可接受的分隔符號 (如分行符號) 或標準 `，` ASF分隔符號分隔的序號清單。 請記住, 此技巧只能用於使用最多參數的命令變數 (因此, 在這種情況下必須指定 `<Bots>`)。

如上所述，空白字元被用作命令的分隔符號, 因此它不能在參數中使用。 但是, 如上所述, ASF可以聯接超出範圍的參數, 這意味著您實際上能夠在參數中使用空白字元, 該字元被定義為給定命令的最後一個字元。 例如，`nickname bob Great Bob` 將正確地將機器人 `bob` 的昵稱設置為“Great Bob”。 與此類似, 您可以檢查`owns` 命令中含有空格的名稱。

請注意，向群組交談發送命令類似於一個中繼器——如果您向一個含有 3 個機器人的群組交談發送 `redeem X` ，其效果等同於分別在私人交談中向每個機器人發送 `redeem X`。 在大多數情況下，**這不是您想要的效果**，您應該像之前與**單個機器人交談**時一樣，向`特定機器人`發送命令。 ASF 支持群組交談，是因為在多數情況下它是一種有效的通信方式，但如果您的群組中有多個 ASF 機器人，就最好不要在這裡執行命令，除非您完全理解 ASF 的相關行為，並且您確實想要讓所有的機器人執行相同的命令。

*即使在這種情況下, 您也應該使用 `<Bots>` 私人交談向機器人發送命令。*

* * *

一些指令有較短的別名可用，以便節省鍵入耗時。

| 指令：          | 別名   |
| ------------ | ---- |
| `owns ASF`   | `oa` |
| `status ASF` | `sa` |
| `redeem`     | `r`  |
| `redeem^`    | `r^` |

* * *

通過 Steam 聊天發送命令不需要任何額外的帳戶——您可以創建一個群組，將 `SteamMasterClanID` 屬性設置為這個新群組，然後通過 `SteamOwnerID` 屬性或者機器人的 `SteamUserPermissions` 屬性為您自己授予足夠的許可權。 這樣，ASF 機器人（即您自己的帳戶）將會加入這個群組和群組聊天室，並且開始監聽您發送的命令。 您可以加入同一個群組聊天室，以便向自己發送命令（因為在您向聊天室發送命令時，同樣在聊天室內的 ASF 實例將會收到命令，即使界面上顯示只有您自己在聊天室內）。 除此之外，您也可以使用 **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)**，但聊天室的方法更簡單，並且如果您有多個帳戶，這種方法就更簡單了。

* * *

### `<Bots>`參数

`<Bots>`是複數參數的一個特殊變體，除了接受多個值以外，它還有額外的功能。

首要的是，您可以使用特殊的關鍵字 `ASF` 來表示“所有機器人”，因此 `status ASF` 命令等同與`status all,your,bots,listed,here` 。 這也可用於輕鬆識別您有權訪問的機器人，因為儘管 `ASF` 關鍵字的目標是所有機器人，但只有您能夠實際發送指令的機器人才會作出響應。

`<Bots>` 參數支援特殊的 "範圍" 語法, 它允許您更輕鬆地選擇一系列機器人。 `<Bots>`情況下的一般語法為 `firstBot..lastBot`。 例如，如果您有名為 `A, B, C, D, E, F`的機器人，在這種情況下，执行 `status B..E`的效果等於执行 `status B,C,D,E` 。 使用此語法時, ASF將使用字母排序, 以决定哪些機器人在指定的範圍內。 `firstBot` 和 `lastBot` 都必須是可被ASF識別的有效機器人名稱, 否則進程將完全跳過範圍語法。

除了上述的範圍語法， `<Bots>`參數還支援​**[正則運算式](https://en.wikipedia.org/wiki/Regular_expression)**​匹配。 您可以使用 `r!<pattern>` 作為機器人名稱來激活正則運算式模式, 其中 `r!` 是用於正則運算式匹配的ASF啟動命令，而 `<pattern>` 是您的正則運算式。 一個使用正則運算式的例子為 `status r!\d{3}` 命令，它會向所有名稱由 3 個數位組成的機器人（例如 `123` 和 `981`）發送` status` 命令。 您可以隨時閱讀這份**[​文檔](https://docs.microsoft.com/en-us/dotnet/standard/base-types/regular-expression-language-quick-reference)**，以進一步瞭解更多可用正則運算式的解釋和示例。

* * *

## `privacy` 設置

`<Settings>`參數擁有**多至 7 個**不同的選項，使用逗號分隔。 這些選項，按順序分別是：

| 參數 | 名稱          | 從屬於        |
| -- | ----------- | ---------- |
| 1  | Profile     |            |
| 2  | OwnedGames  | Profile    |
| 3  | Playtime    | OwnedGames |
| 4  | FriendsList | Profile    |
| 5  | Inventory   | Profile    |
| 6  | 物品庫禮品       | Inventory  |
| 7  | 評論          | Profile    |

有關上述欄位的說明, 請訪問 **Steam 隱私設置 </0 >。</p> 

每個選項的有效值可以是：

| 值 | 名稱            |
| - | ------------- |
| 1 | `Private`     |
| 2 | `FriendsOnly` |
| 3 | `Public`      |

您可以使用它們的名稱（不區分大小寫）或者數值。 省略的參數將會被設置為預設值 `Private`。 請謹記上述參數的從屬關係非常重要，因為子選項無法擁有比父選項更高的許可權。 例如，如果您將個人資料設置為 `Private`，就**無法**再將遊戲詳情設置為 `Public`。

### 範例

如果您希望將機器人 `Main` 的**所有**隱私設置都設置為 `Private`，可以使用以下任一命令：

    privacy Main 1
    privacy Main Private
    

這是因為 ASF 會預設所有未賦值選項為 `Private`，所以您不需要全部寫出它們。 另一方面，如果您希望設置所有選項為 `Public`，可以使用以下任一指令：

    privacy Main 3,3,3,3,3,3,3
    privacy Main Public,Public,Public,Public,Public,Public,Public
    

也可以為每個選項設置不同的值：

    privacy Main Public,FriendsOnly,Private,Public,Public,Private,Public
    

上述命令將會設置個人資料為公開、遊戲詳情為僅限好友、遊戲時間為私密、好友列表為公開、物品庫為公開、物品庫禮物為私密、留言為公開。 若有需要，您也可以使用數字值來實現相同效果。

請記住子選項的許可權無法高於父選項。 有關可用選項, 請參閱參數關係。

* * *

## `redeem^`模式

`redeem^` 命令允許您微調用於單個兌換場景的模式。 此命令會臨時覆蓋 `RedeemingPreferences`**[ 機器人配置屬性](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#bot-config)**。

`<Modes>` 參數接受多個模式值, 通常用逗號分隔。 可用的模式值如下所示:

| 值    | 名稱                    | 描述                             |
| ---- | --------------------- | ------------------------------ |
| FD   | ForceDistributing     | 強制啟用 `Distributing` 激活偏好設置     |
| FF   | ForceForwarding       | 強制啟用 `Forwarding` 激活偏好設置       |
| FKMG | ForceKeepMissingGames | 強制啟用 `KeepMissingGames`激活偏好設置  |
| SD   | SkipDistributing      | 強制禁用 `Distributing` 激活偏好設置     |
| SF   | SkipForwarding        | 強制禁用 `Forwarding` 激活偏好設置       |
| SI   | SkipInitial           | 跳過初始機器人的金鑰兌換過程                 |
| SKMG | SkipKeepMissingGames  | 強制禁用 `KeepMissingGames` 激活偏好設置 |
| V    | Validate              | 檢查金鑰格式是否正確，並自動跳過無效金鑰           |

例如，我們打算為尚未擁有遊戲的機器人兌換 3 個金鑰，但不包括 `primary` 機器人。 為此我們需要執行命令：

`redeem^ primary FF,SI key1,key2,key3`

需要注意的是，進階激活模式只會覆蓋您**在命令中使用**的`RedeemingPreferences`選項。 例如, 如果您在 `RedeemingPreferences` 中啟用了 `Distributing`, 則無論是否使用 `FD` 模式, 都不會有任何區別。 這就是為什麼每個可強制啟用的重寫也有一個可強制禁用的選項, 若有需要，您可以決定在啟用的情況下強制覆蓋, 反之亦然。

* * *

## `input` 命令

輸入命令只能在 `Headless` 模式下使用, 用於在 ASF 運行而不支援使用者交互的情況下, 通過 **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** 或 Steam 聊天輸入給定的資料。

通用語法是 `input <Bots> <Type> <Value>`。

`<Type>` 不區分大小寫, 並定義由ASF識別的輸入類型。 目前, ASF可識別以下類型:

| 類型                      | 描述                                        |
| ----------------------- | ----------------------------------------- |
| DeviceID                | 2FA 設備驗證器, 在 `.maFile` 中缺失這個值時使用。         |
| Login                   | `SteamLogin`機器人配置屬性，在設定檔缺失這個值時使用。         |
| Password                | `SteamPassword` 機器人配置屬性，在設定檔缺失這個值時使用。     |
| SteamGuard              | 如果您未啟用2FA，驗證代碼將以電子郵件的方式發送。                |
| SteamParentalCode       | `SteamParentalCode` 機器人配置屬性，在設定檔缺失這個值時使用。 |
| TwoFactorAuthentication | 如果您使用的是2FA, 但未使用 ASF 2FA, 則從您的手機生成2FA代碼 。 |

`<Value>` 是為給定類型設置的值。 目前, 所有值都是字串。

### 範例

假設我們有一個未啟用2FA，僅由 Steam 代碼保護的機器人。 我們希望在 `Headless` 模式下啟動這個機器人。

為此，我們需要執行以下指令：

`start MySteamGuardBot` -> 機器人會嘗試登入，但缺少驗證碼會導致登入失敗，在`Headless` 模式下，機器人會停止運行。 我們需要這樣, 以便使 steam 網路通過電子郵件向我們發送驗證代碼--如果不需要這樣做, 我們將完全跳過這一步。

`input MySteamGuardBot SteamGuard ABCDE` -> 我們將 `MySteamGuardBot` 機器人的 `SteamGuard` 輸入設置為 `ABCDE`。 當然, 在這種情況下, `ABCDE` 是我們在電子郵件中獲得的驗證代碼。

`start MySteamGuardBot` ->我們重新啟動（已停止的）機器人，這一次會自動使用我們在上一步中設置的身份驗證代碼, 正確登錄, 然後清除它。

同樣, 我們可以訪問受2FA 保護的機器人 (如果它們不使用 ASF 2FA), 只需在運行時設置其他必需的屬性。