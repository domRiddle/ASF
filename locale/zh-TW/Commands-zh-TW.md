# 指令

ASF 支援各種指令，以此控制程序及 Bot 實例的行為。

您可以透過這些方式來發送指令：
- 透過互動式 ASF 控制台
- 透過 Steam 私人／群組聊天
- 透過 **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-zh-TW)** 介面

請注意，與 ASF 互動需要您擁有執行相關指令的權限。 查看 `SteamUserPermissions` 和 `SteamOwnerID` 的設定屬性以了解更多。

透過 Steam 聊天執行的指令都受**[全域設定屬性](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-TW#commandprefix)**的 `CommandPrefix` 影響，而該屬性的預設值為 `!`。 這意味著，當您要執行 `status` 指令時，實際應該要傳送 `!status`（或使用您自訂的 `CommandPrefix`）。 而當您使用控制台或 IPC 時，可以省略 `CommandPrefix`，這項屬性在這裡不是強制性的。

---

### 互動式控制台

從 V4.0.0.9 版本開始，ASF 支援互動式控制台，只要您沒有讓它在 [**`Headless`**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-TW#headless) 模式下執行。 只要按下 `C` 鍵，就可以啟用指令模式。輸入您的指令並按下確認鍵確認。

![擷圖](https://i.imgur.com/bH5Gtjq.png)

---

### Steam 聊天

您也可以透過 Steam 聊天，使指定的 ASF Bot 執行指令。 很顯然，您無法直接跟自己聊天。因此若您想在自己的主帳號執行指令，您至少需要另一個 Bot 帳號。

![擷圖](https://i.imgur.com/IvFRJ5S.png)

同樣，您也可以使用指定的 Steam 群組聊天。 請注意，此選項需要您正確設定 `SteamMasterClanID` 屬性，使 Bot 也會同時監聽（並加入）指定群組聊天中的指令。 這與私人聊天不同，因為這個方式不需要專用的 Bot 帳號，所以可以用於「和自己交談」。 您只需將 `SteamMasterClanID` 屬性設為您新建立的群組，然後透過設定 Bot 的 `SteamOwnerID` 或 `SteamUserPermissions` 給您自己存取權限。 這樣，ASF Bot（即您自己的帳號）將會加入這個群組及它的群組聊天室，並開始監聽您發送的指令。 您可以加入同一個群組聊天室，以便向自己發送指令（因為在您向聊天室發送指令時，同樣在聊天室內的 ASF 實例將會收到指令，即使界面上顯示只有您自己在聊天室中）。

請注意，傳送指令至群組聊天就像是一個中繼。 如果您向一個含有 3 個 Bot 的群組聊天發送 `redeem X` 指令，其效果跟分別向每個 Bot 私人聊天發送 `redeem X` 指令一樣。 在大多數情況下，**這並非您想要的效果**，您應該像之前**與單個 Bot 交談**時一樣，使用`特定 Bot` 名稱的指令形式。 ASF 支援群組聊天，是因為在多數情況下，它是一種與您唯一的 Bot 通訊的有效方式。但若您的群組中有多個 ASF Bot，就最好不要在這裡執行指令，除非您完全理解 ASF 的相關行為，並且您確實想讓所有 Bot 執行相同的指令。

*但即使在這種情況下，您也應該使用帶有 `[Bots]` 語法的私人聊天，來向 Bot 發送指令。*

---

### IPC

這是最先進且最靈活執行指令的方式，非常適合使用者交互（ASF-ui）及第三方工具或腳本（ASF API）。這種方式需要 ASF 執行於 `IPC` 模式，且用戶端需要透過 **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** 介面來執行指令。

![擷圖](https://raw.githubusercontent.com/JustArchiNET/ASF-ui/main/.github/previews/commands.png)

---

## 指令

| 指令                                                                   | 存取權限            | 描述                                                                                                                                                             |
| -------------------------------------------------------------------- | --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `2fa [Bots]`                                                         | `Master`        | 使指定的 Bot 實例，生成臨時的**[雙重驗證](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-zh-TW)**​權杖。                                           |
| `2fano [Bots]`                                                       | `Master`        | 使指定的 Bot 實例，拒絕所有待處理的**[雙重驗證](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-zh-TW)**​交易確認。                                      |
| `2faok [Bots]`                                                       | `Master`        | 使指定的 Bot 實例，接受所有待處理的**[雙重驗證](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-zh-TW)**​交易確認。                                      |
| `addlicense [Bots] <Licenses>`                                 | `Operator`      | 在指定的 Bot 實例上啟用給定的 `licenses`，請參閱**[下文](#addlicense-licenses)**的說明（僅適用於免費遊戲）。                                                                                   |
| `balance [Bots]`                                                     | `Master`        | 顯示指定 Bot 實例的 Steam 錢包餘額。                                                                                                                                       |
| `bgr [Bots]`                                                         | `Master`        | 顯示關於指定 Bot 實例的**[背景序號啟動器](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer-zh-TW)**佇列的資訊。                                         |
| `encrypt <encryptionMethod> <stringToEncrypt>`           | `Owner`         | 以指定的加密方式加密字串──請參閱​**[下文](#encrypt-command)**的說明。                                                                                                               |
| `exit`                                                               | `Owner`         | 完全終止 ASF 程序。                                                                                                                                                   |
| `farm [Bots]`                                                        | `Master`        | 重新啟動指定 Bot 實例的掛卡模組。                                                                                                                                            |
| `fb [Bots]`                                                          | `Master`        | 列出指定 Bot 實例的應用程式自動掛卡黑名單。                                                                                                                                       |
| `fbadd [Bots] <AppIDs>`                                        | `Master`        | 將給定的 `AppIDs` 新增至指定 Bot 實例的應用程式自動掛卡黑名單中。                                                                                                                       |
| `fbrm [Bots] <AppIDs>`                                         | `Master`        | 將給定的 `AppIDs` 從指定 Bot 實例的應用程式自動掛卡黑名單中移除。                                                                                                                       |
| `fq [Bots]`                                                          | `Master`        | 列出指定 Bot 實例的優先掛卡佇列。                                                                                                                                            |
| `fqadd [Bots] <AppIDs>`                                        | `Master`        | 將給定的 `AppIDs` 新增至指定 Bot 實例的優先掛卡佇列中。                                                                                                                            |
| `fqrm [Bots] <AppIDs>`                                         | `Master`        | 將給定的 `AppIDs` 從指定 Bot 實例的優先掛卡佇列中移除。                                                                                                                            |
| `hash <hashingMethod> <stringToHash>`                    | `Owner`         | 以指定的加密方式產生給定字串的雜湊值──請參閱​**[下文](#hash-command)**的說明。                                                                                                            |
| `help`                                                               | `FamilySharing` | 顯示幫助（指向此頁面的連結）。                                                                                                                                                |
| `input [Bots] <Type> <Value>`                            | `Master`        | 為指定 Bot 實例輸入指定類型的輸入值，僅在 `Headless` 模式下工作──請參閱**[下文](#input-指令)**的說明。                                                                                           |
| `level [Bots]`                                                       | `Master`        | 顯示指定 Bot 實例的 Steam 帳號等級。                                                                                                                                       |
| `loot [Bots]`                                                        | `Master`        | 將指定 Bot 實例中所有 `LootableTypes` Steam 社群物品，交易給其 `SteamUserPermissions` 屬性中設定的 `Master` 使用者（如有多個，則取 steamID 最小者）。                                                 |
| `loot@ [Bots] <AppIDs>`                                        | `Master`        | 將指定 Bot 實例中，所有符合 `AppIDs` 的 `LootableTypes` Steam 社群物品，交易給其 `SteamUserPermissions` 屬性中設定的 `Master` 使用者（如有多個，則取 steamID 最小者）。 這是與 `loot%` 相反的指令。                |
| `loot% [Bots] <AppIDs>`                                        | `Master`        | 將指定 Bot 實例中，除符合 `AppIDs` 以外的所有 `LootableTypes` Steam 社群物品，交易給其 `SteamUserPermissions` 屬性中設定的 `Master` 使用者（如有多個，則取 steamID 最小者）。 這是與 `loot@` 相反的指令。             |
| `loot^ [Bots] <AppID> <ContextID>`                       | `Master`        | 將指定 Bot 實例的 `ContextID` 物品庫分類中，所有符合給定 `AppID` 的 `LootableTypes` Steam 社群物品，交易給其 `SteamUserPermissions` 屬性中設定的 <0>Master</0> 使用者（如有多個，則取 steamID 最小者）。          |
| `mab [Bots]`                                                         | `Master`        | 列出 **[`MatchActively`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading#matchactively)** 中的應用程式自動交易黑名單。                                             |
| `mabadd [Bots] <AppIDs>`                                       | `Master`        | 將給定的 `AppIDs` 加入至 **[`MatchActively`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading#matchactively)** 的應用程式自動交易黑名單中。                              |
| `mabrm [Bots] <AppIDs>`                                        | `Master`        | 將給定的 `AppIDs` 從 **[`MatchActively`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading#matchactively)** 的應用程式自動交易黑名單中移除。                              |
| `nickname [Bots] <Nickname>`                                   | `Master`        | 將指定 Bot 實例的 Steam 暱稱，更改為給定的 `Nickname`。                                                                                                                        |
| `owns [Bots] <Games>`                                          | `Operator`      | 檢查指定的 Bot 實例是否已擁有給定的 `Games`，請參閱**[下文](#owns-games)**的說明。                                                                                                      |
| `pause [Bots]`                                                       | `Operator`      | 停止指定 Bot 實例的自動掛卡模組。 ASF 在本次階段中，將不會再次嘗試對此帳號進行掛卡，除非您手動 `resume`，或者重新啟動 ASF 程序。                                                                                   |
| `pause~ [Bots]`                                                      | `FamilySharing` | 暫停指定 Bot 實例的自動掛卡模組。 掛卡程序將會在下次遊戲事件被觸發時，或在 Bot 斷線時自動恢復。 您可以使用 `resume` 以恢復掛卡。                                                                                    |
| `pause& [Bots] <Seconds>`                                  | `Operator`      | 暫停指定 Bot 實例的自動掛卡模組 `Seconds` 秒。 在此之後，掛卡模組將會自動恢復。                                                                                                               |
| `play [Bots] <AppIDs,GameName>`                                | `Master`        | 切換至手動掛卡模式──使指定的 Bot 實例為給定的 `AppIDs` 執行掛卡，自訂的 `GameName` 為可選的遊戲名稱。 若要使此功能正常運作，您的 Steam 帳號**必須**擁有您指定的所有 `AppIDs` 的有效許可，包括免費遊戲。 使用 `reset` 或 `resume` 指令來恢復自動掛卡。 |
| `points [Bots]`                                                      | `Master`        | 顯示指定 Bot 實例的 **[Steam 商店](https://store.steampowered.com/points/shop)**​點數餘額。                                                                                  |
| `privacy [Bots] <Settings>`                                    | `Master`        | 更改指定 Bot 實例的 **[Steam 隱私設定](https://steamcommunity.com/my/edit/settings)**，可用的選項請參閱**[​下文](#privacy-settings)**的說明。                                            |
| `redeem [Bots] <Keys>`                                         | `Operator`      | 使指定的 Bot 實例啟用給定的產品序號或錢包儲值碼。                                                                                                                                    |
| `redeem^ [Bots] <Modes> <Keys>`                          | `Operator`      | 使指定的 Bot 實例啟用給定的產品序號或錢包儲值碼，並以**[​下文](#redeem-modes)**說明的 `Modes` 模式使用。                                                                                         |
| `reset [Bots]`                                                       | `Master`        | 將遊戲狀態重設為初始（先前的）狀態。本指令用於配合 `play` 指令的手動掛卡模式。                                                                                                                    |
| `restart`                                                            | `Owner`         | 重新啟動 ASF 程序。                                                                                                                                                   |
| `resume [Bots]`                                                      | `FamilySharing` | 恢復指定 Bot 實例的自動掛卡程序。                                                                                                                                            |
| `start [Bots]`                                                       | `Master`        | 啟動指定的 Bot 實例。                                                                                                                                                  |
| `stats`                                                              | `Owner`         | 顯示程序的統計資料，例如託管記憶體的使用量。                                                                                                                                         |
| `status [Bots]`                                                      | `FamilySharing` | 顯示指定 Bot 實例的狀態。                                                                                                                                                |
| `std`                                                                | `Owner`         | 用於控制 **[`SteamTokenDumperPlugin`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/SteamTokenDumperPlugin-zh-TW)** 的特殊指令，用於立即提交資料。                          |
| `stop [Bots]`                                                        | `Master`        | 停止指定的 Bot 實例。                                                                                                                                                  |
| `tb [Bots]`                                                          | `Master`        | 列出指定 Bot 實例交易模組黑名單中的使用者。                                                                                                                                       |
| `tbadd [Bots] <SteamIDs64>`                                    | `Master`        | 將給定的 `SteamIDs64` 加入至指定 Bot 的實例交易模組黑名單中。                                                                                                                       |
| `tbrm [Bots] <SteamIDs64>`                                     | `Master`        | 將給定的 `SteamIDs64` 從指定 Bot 的實例交易模組黑名單中移除。                                                                                                                       |
| `transfer [Bots] <TargetBot>`                                  | `Master`        | 將指定 Bot 實例所有 `TransferableTypes` 的 Steam 社群物品交易至目標 Bot 實例。                                                                                                     |
| `transfer@ [Bots] <AppIDs> <TargetBot>`                  | `Master`        | 將指定 Bot 實例中，所有符合 `AppIDs` 的 `TransferableTypes` 的 Steam 社群物品，交易至目標 Bot 實例。 這是與 `transfer%` 相反的指令。                                                              |
| `transfer% [Bots] <AppIDs> <TargetBot>`                  | `Master`        | 將指定 Bot 實例中，除符合 `AppIDs` 以外的 `TransferableTypes` 的所有 Steam 社群物品，交易至目標 Bot 實例。 這是與 `transfer@` 相反的指令。                                                           |
| `transfer^ [Bots] <AppID> <ContextID> <TargetBot>` | `Master`        | 將指定 Bot 實例的 `ContextID` 中，符合 `AppID` 的 Steam 物品交易至目標 Bot 實例。                                                                                                   |
| `unpack [Bots]`                                                      | `Master`        | 拆開指定 Bot 實例物品庫中的所有擴充包。                                                                                                                                         |
| `update`                                                             | `Owner`         | 檢查 GitHub 上的 ASF 更新（每 `UpdatePeriod` 會自動執行一次）。                                                                                                                 |
| `version`                                                            | `FamilySharing` | 顯示 ASF 的版本號碼。                                                                                                                                                  |

---

### 備註

所有的指令都不區分大小寫，但它們的引數（例如 Bot 的名稱）通常需要區分大小寫。

`[Bots]` 引數在所有指令中，都是可省略的。 當指定該引數時，指令會在指定的 Bot 上執行。 但省略時，指令會在當前接收到指令的 Bot 上執行。 換句話說，向 Bot `B` 發送 `status A`，等於直接向 Bot `A` 發送 `status` 指令。在這種情形下， Bot `B` 只被作為一個代理 Bot。 這也可用於向無法使用的 Bot 傳送指令，例如啟動已終止的 Bot，或者在（您用於執行指令的）主要帳號上執行動作。

指令的**存取權限**定義了執行此指令所需的**最低**權限，即 `SteamUserPermissions` 中定義的 `EPermission`。但有個例外，`Owner` 是指全域設定檔中定義的 `SteamOwnerID` 使用者（擁有最高權限）。

複數引數，例如 `[Bots]`、`<Keys>`或`<AppIDs>`，意味著該引數支援多個由逗號分隔的相同類型數值。 舉例來說，`status [Bots]` 可以輸入成 `status MyBot,MyOtherBot,Primary`。 這樣，該指令會在**所有的目標 Bot** 上執行，效果等同於向所有的 Bot 分別發送 `status` 指令。 需要注意的是，「`,`」後面不能有空格。

ASF 使用所有的空白字元作為指令的定界符，例如空格和換行字元。 這代表您不僅可以使用空格來分隔引數，還可以使用任何其他空白字元（例如 Tab 及換行）。

ASF 會將超出指令範圍的多餘引數「連接」至符合語法的最後一個複數引數上。 這代表對於 `redeem [Bots]<Keys>` 指令而言，`redeem bot key1 key2 key3` 跟 `redeem bot key1,key2,key3` 的作用完全相同。 再加上您可以使用換行作為指令定界符，這樣您就可以先輸入 `redeem bot`，然後再貼上產品序號清單，其中的每一項可以由任意空白字元（例如換行字元）或者 ASF 的標準 `,` 作為定界。 請注意，若要使用這一技巧，您必須採用該指令引數數量最多的形式（故在這種情形下，您必須指定 `[Bots]` 引數）。

如上所述，空白字元被用於指令引數的定界符，所以引數內部無法再使用空白字元。 但同樣如上所述，ASF 可以連接超出範圍的引數。這代表您可以在指令的最後一個引數中，使用空白字元。 舉例來說，`nickname bob Great Bob` 指令能夠將 Bot `bob` 的暱稱正確設定成「Great Bob」。 與之相似，您也可以使用 `owns` 指令來檢查帶有空格的名稱。

---

部分指令亦有較短的指令別名可供使用，以便於輸入：

| 指令           | 指令別名         |
| ------------ | ------------ |
| `addlicense` | `addlicence` |
| `owns ASF`   | `oa`         |
| `status ASF` | `sa`         |
| `redeem`     | `r`          |
| `redeem^`    | `r^`         |

---

### `[Bots]` 引數

`[Bots]` 是複數引數的一個特殊變體，除了能夠接收多個值以外，它還具有額外的功能。

首先，您可以使用特殊關鍵字 `ASF` 來表示「程序中的所有 Bot」。因此 `status ASF` 指令等同於 `status 您,在此,列出的,所有,Bot`。 這也可以方便地辨識您有權操作哪些 Bot，因為儘管關鍵字 `ASF` 的目標是所有 Bot，但只有您實際能夠傳送指令的 Bot 才會作出回應。

`[Bots]` 引數支援特殊的「範圍」語法，這可以讓您更容易選擇特定範圍的 Bot。 在這種情況下，`[Bots]` 的一般語法是 `firstBot..lastBot`。 舉例來說，假設您有名為 `A, B, C, D, E, F` 的 Bot，若您執行 `status B..E`，等同於執行 `status B,C,D,E`。 在使用此語法時，ASF 將會以字母排序來決定哪些 Bot 在指定範圍內。 `firstBot` 及 `lastBot` 必須皆是 ASF 能夠辨識的有效 Bot 名稱，否則範圍語法將不會生效。

除了上述的範圍語法，`[Bots]` 引數還支援**[正規表示式](https://en.wikipedia.org/wiki/Regular_expression)**的檢索符合規則。 您可以使用 `r!<pattern>` 作為 Bot 的名稱，來使用正規表示式。其中 `r!` 是 ASF 用於進入正規表示式檢索行為的啟動指令，而 `<pattern>` 是您的正規表示式。 一個正規表示式的 Bot 指令範例：`status r!^\d{3}`，它會向所有名稱為 3 位數字的 Bot（例如 `123` 及 `981`）傳送 `status` 指令。 您可以閱讀這份**[檔案說明](https://docs.microsoft.com/zh-tw/dotnet/standard/base-types/regular-expression-language-quick-reference)**，來進一步了解更多說明及範例。

---

## `privacy` 指令中的 Setting 引數

`<Settings>` 引數接受**最多 7 個**不同的選項，它使用了 ASF 的標準逗號定界格式。 這些選項分別是：

| 引數 | 名稱             | 為…的子選項     |
| -- | -------------- | ---------- |
| 1  | Profile        |            |
| 2  | OwnedGames     | Profile    |
| 3  | Playtime       | OwnedGames |
| 4  | FriendsList    | Profile    |
| 5  | Inventory      | Profile    |
| 6  | InventoryGifts | Inventory  |
| 7  | Comments       | Profile    |

關於上述選項的說明，請造訪 **[Steam 隱私設定](https://steamcommunity.com/my/edit/settings)**。

每個選項的有效值可以是：

| 值 | 名稱                  |
| - | ------------------- |
| 1 | `Private（私人）`       |
| 2 | `FriendsOnly（僅限好友）` |
| 3 | `Public（公開）`        |

您可以使用它們的名稱（不區分大小寫）或數值。 未賦值的引數將會被設為預設值 `Private`。 特別注意，上述引數的從屬關係非常重要，因為子選項無法擁有比父選項還高的權限。 舉例來說，若您將個人檔案設為 `Private`，就**無法**再將遊戲資料設定成 `Public`。

### 範例

如果希望將名為 `Main` 的 Bot 的**所有**隱私設定都設為 `Private`，您可以使用以下任一指令：

```text
privacy Main 1
privacy Main Private
```

這是因為 ASF 會將所有未賦值的選項設為 `Private`，所以您無需將它們全部列出來。 與上述完全相反的情況，若您希望將所有選項設為 `Public`，則可以使用以下任一指令：

```text
privacy Main 3,3,3,3,3,3,3
privacy Main Public,Public,Public,Public,Public,Public,Public
```

用這種方式，您也可以為每個選項設定不同的值：

```text
privacy Main Public,FriendsOnly,Private,Public,Public,Private,Public
```

上述指令會將個人檔案設為公開、遊戲資料設為僅限好友、總遊玩時數為私人、好友名單為公開、物品庫為公開、物品庫禮物為私密、個人檔案留言為私人。 若有需要，您也可以使用數值來實現相同效果。

請記住，子選項的權限無法高於父選項。 您可以參閱上述引數選項的子選項關係。

---

## `addlicense` 指令中的 Licenses 引數

`addlicense` 指令支援兩種不同的授權類型：

| 類型    | 別名  | 範例           | 描述                                |
| ----- | --- | ------------ | --------------------------------- |
| `app` | `a` | `app/292030` | 透過遊戲的唯一 `appID` 授權。               |
| `sub` | `s` | `sub/47807`  | 透過遊戲組合包的唯一 `subID` 授權，包含一款或以上的遊戲。 |

需要注意這兩者的區別，因為以 ASF 來說，對於應用程式將會使用 Steam 網路來啟動，而對遊戲組合包則使用 Steam 商店來啟動。 這兩者互不相容。一般而言，您會對免費週末及永久免費遊玩的遊戲使用 app 類型，而對遊戲組合包使用 sub。

我們建議您明確定義每一個項目的類型，以避免歧義。但為了反向相容性，在您提供了無效或省略的類型的情況下，ASF 將會預設您想要使用 `sub` 類型。 您也可以使用 ASF 的標準定界符「`,`」來同時啟用多個許可。

完整的指令範例：

```text
addlicense ASF app/292030,sub/47807
```

---

## `owns` 指令中的 Games 引數

`owns` 指令支援數種不同的 `<games>` 引數類型：

使用**[正規表示式](https://en.wikipedia.org/wiki/Regular_expression)**來表示遊戲名稱，區分大小寫。 請參閱**[檔案說明](https://docs.microsoft.com/zh-tw/dotnet/standard/base-types/regular-expression-language-quick-reference)</strong>，來進一步了解完整語法及範例。</td> </tr> 

</tbody> </table> 

我們建議您明確定義每一個項目的類型，以避免歧義。但為了反向相容性，在您提供了無效或省略的類型的情況下，若您填入了數字，ASF 將會預設您想要使用 `app` 類型，若為其他值則為 `name` 類型。 您也可以使用 ASF 的標準定界符「`,`」來同時查詢多個遊戲。

完整的指令範例：



```text
owns ASF app/292030,name/Witcher
```




---



## `redeem^` 指令中的 Modes 引數

`redeem^` 指令允許您微調使用於兌換單個產品序號情境的模式。 此指令會臨時覆蓋 `RedeemingPreferences` **[Bot 設定屬性](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-TW#Bot-設定檔)**。

`<Modes>` 引數接受多個模式值，通常使用逗號分隔。 可用的模式值如下：

| 值    | 名稱                    | 描述                                                |
| ---- | --------------------- | ------------------------------------------------- |
| FAWK | ForceAssumeWalletKey  | 強制啟用 `AssumeWalletKeyOnBadActivationCode` 兌換偏好設定。 |
| FD   | ForceDistributing     | 強制啟用 `Distributing` 兌換偏好設定。                       |
| FF   | ForceForwarding       | 強制啟用 `Forwarding` 兌換偏好設定。                         |
| FKMG | ForceKeepMissingGames | 強制啟用 `KeepMissingGames` 兌換偏好設定。                   |
| SAWK | SkipAssumeWalletKey   | 強制停用 `AssumeWalletKeyOnBadActivationCode` 兌換偏好設定。 |
| SD   | SkipDistributing      | 強制停用 `Distributing` 兌換偏好設定。                       |
| SF   | SkipForwarding        | 強制停用 `Forwarding` 兌換偏好設定。                         |
| SI   | SkipInitial           | 產品序號兌換過程跳過第一個 Bot。                                |
| SKMG | SkipKeepMissingGames  | 強制停用 `KeepMissingGames` 兌換偏好設定。                   |
| V    | Validate              | 檢查序號格式，自動跳過無效的產品序號。                               |


舉例來說，我們打算為任何尚未擁有遊戲的 Bot 兌換 3 個產品序號，但不包含 `primary` Bot。 為此，我們需要執行指令：

`redeem^ primary FF,SI key1,key2,key3`

需要注意的是，進階兌換模式只會替換您**在指令中指定**的 `RedeemingPreferences` 選項。 舉例來說，若您在`RedeemingPreferences` 中啟用了 `Distributing`，那麼無論是否使用 `FD` 模式，都不會有任何區別，因為您已經在 `RedeemingPreferences` 中啟用了它。 這就是為什麼每個可強制啟用的模式，也會有一個可強制停用的選項。您可以將被停用的選項強制覆蓋成啟用，相反的狀況亦可。



---



## `encrypt` 指令

`encrypt` 指令使您能夠使用 ASF 的加密方式加密任意字串。 加密方式 `<encryptionMethod>` 必須是​**[安全性](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security)**​章節所述方式之一。 我們建議透過安全的通道（ASF 控制台、ASF-ui 或 IPC 提供的專用 API 端點）使用此指令，否則敏感資訊可能會被第三方記錄（例如聊天訊息會被 Steam 伺服器記錄）。



---



## `hash` 指令

`hash` 指令使您能夠使用 ASF 的雜湊方式，產生任意字串的雜湊值。 雜湊方式 `<hashingMethod>` 必須是​**[安全性](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security)**​章節所述方式之一。 我們建議透過安全的通道（ASF 控制台、ASF-ui 或 IPC 提供的專用 API 端點）使用此指令，否則敏感資訊可能會被第三方記錄（例如聊天訊息會被 Steam 伺服器記錄）。



---



## `input` 指令

`input` 指令僅可在 `Headless` 模式下使用。在 ASF 無法接受使用者輸入的情況下，透過 **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** 或 Steam 聊天來輸入資料。

基本語法為 `input [Bots] <Type> <Value>`。

`<Type>` 不區分大小寫，定義 ASF 可識別的輸入類型。 目前 ASF 可識別以下類型：

| 類型                      | 描述                                       |
| ----------------------- | ---------------------------------------- |
| Login                   | `SteamLogin` Bot 設定屬性，設定檔缺失時使用此值。        |
| Password                | `SteamPassword` Bot 設定屬性，設定檔缺失時使用此值。     |
| Steam Guard             | 如果您未啟用雙重驗證，驗證碼將以電子郵件的方式發送。               |
| SteamParentalCode       | `SteamParentalCode` Bot 設定屬性，設定檔缺失時使用此值。 |
| TwoFactorAuthentication | 如果您啟用雙重驗證但並非 ASF 雙重驗證，雙重驗證權杖將由您的手機生成。    |


`<Value>` 是要為指定類型設定的值。 目前所有的值都屬於字串。



### 範例

假設我們有一個未啟用雙重驗證，僅由 Steam Guard 保護的 Bot。 我們希望在 `Headless` 為 `true` 的情況下啟動這個 Bot。

為此，我們需要執行以下指令：

`start MySteamGuardBot` -> Bot 將嘗試登入，但會因為缺少驗證碼而登入失敗，然後因為 ASF 處於 `Headless` 模式，Bot 會停止執行。 我們做這一步的目的，是讓 Steam 網路透過電子郵件向我們發送驗證碼──若不需要，我們可以直接跳過此步驟。

`input MySteamGuardBot SteamGuard ABCDE` -> 我們將 `MySteamGuardBot` Bot 的 `SteamGuard` 輸入設定為 `ABCDE`。 當然在這種情形下，`ABCDE` 就是我們從電子郵件中獲得的驗證碼。

`start MySteamGuardBot` -> 我們重新啟動已停止的 Bot。這一次，會自動使用我們在上個步驟中設定的驗證碼，登入將會成功，且之前輸入的驗證碼會被清除。

以同樣的方式，我們可以存取受雙重驗證保護的 Bot（如果它們不使用 ASF 雙重驗證），只需在執行時設定其他必須屬性即可。