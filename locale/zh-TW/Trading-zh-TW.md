# 交易

ASF 支援 Steam 的非互動式（離線）交易。 您可以直接接收（接受／拒絕）及發送交易，不需要特殊設定，但顯而易見這需要不受限制的 Steam 帳號（需已在商店中消費滿 $5 美元）。 受限制的帳號無法使用交易模組。

---

## 邏輯

ASF 始終會接受來自具有 `Master`（或更高）存取權限的使用者發送的所有交易提案，無論交易物品為何。 這不只可以輕鬆收集 Bot 實例掛出的 Steam 交換卡片，還能簡單管理 Bot 存放在物品庫中的 Steam 物品──包含來自其他遊戲（例如 CS:GO）的物品。

ASF 將拒絕來自交易模組黑名單的任何（非 Master）使用者的交易提案，無論交易物品為何。 黑名單儲存在 `BotName.db` 標準資料庫中，可以透過 `tb`、`tbadd` 及 `tbrm` **[指令](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-TW)**進行管理。 這應該能夠代替 Steam 提供的標準使用者封鎖，請謹慎使用。

ASF 將接受所有透過 Bot 發送類似 `loot` 的交易，除非在 `TradingPreferences` 中設定了 `DontAcceptBotTrades`。 簡而言之，`TradingPreferences` 中預設的 `None` 會使 ASF 自動接受來自具有 `Master` 存取權限 Bot 的使用者的交易（如上所述），以及 ASF 同一程序中其他 Bot 的所有贈禮交易。 若您想停用來自其他 Bot 的贈禮交易，那麼您應在 `TradingPreferences` 中設定 `DontAcceptBotTrades`。

當您在 `TradingPreferences` 中設定 `AcceptDonations` 後，ASF 還將接受任何贈禮交易：Bot 帳號不會失去任何物品的交易。 這個屬性只影響非 Bot 帳號，因為 Bot 帳號是受 `DontAcceptBotTrades` 的影響。 `AcceptDonations` 能讓您輕鬆地接受來自其他使用者，及不在同一 ASF 程序中 Bot 的贈禮。

值得一提的是，`AcceptDonations` 不需要 **[ASF 雙重驗證](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor- authentication-zh-TW)**，因為如果我們沒有失去任何物品，則無需進行交易確認。

您還可以透過修改相應的 `TradingPreferences` 來進一步自訂 ASF 的交易功能。 `TradingPreferences` 的其中一個主要功能是 `SteamTradeMatcher` 選項，它將使 ASF 使用內建邏輯來接受交易，並幫助您完成缺少的徽章，這在結合 **[SteamTradeMatcher](https://www.steamtradematcher.com)** 的公開清單使用時特別有用，但它也能單獨運作。 我們將在下面進一步說明。

---

## `SteamTradeMatcher`

當啟用 `SteamTradeMatcher` 時，ASF 將使用相當複雜的演算法，來檢查交易是否通過 STM 規則，且對我們而言是否公平。 具體的邏輯是：

- 如果我們會失去 `MatchableTypes` 之外的任何物品，則拒絕交易。
- 對於每個遊戲、物品類型及稀有度，如果我們獲得的物品數量少於失去的數量，則拒絕交易。
- 如果使用者想要交易特殊的 Steam 夏季／冬季特賣交換卡片，但有交易託管，則拒絕交易。
- 如果交易託管的時間達到全域設定屬性 `MaxTradeHoldDuration` 的值，則拒絕交易。
- 如果我們沒有設定 `MatchEverything`，且交易內容對我們不利，則拒絕交易。
- 如果未被上述任何規則拒絕，則接受交易。

值得一提的是，ASF 還支援溢價支付：只要滿足上述所有條件，在使用者向交易內容提供額外物品時，邏輯也會正常運作。

前四個拒絕條件應該是顯而易見的。 最後一個含有實際的重複邏輯，它檢查我們物品庫的當前狀態，再決定交易狀態。

- 如果交易會使您的徽章進度增加，則為**有利**。 例如：A A（交易前）-> A B（交易後）
- 如果交易不影響您的徽章進度，則為**均衡**。 例如：A B（交易前）-> A C（交易後）
- 如果交易會使您的徽章進度減少，則為**不利**。 例如：A C（交易前）-> A A（交易後）

STM 只會處理有利的交易，代表使用 STM 進行重複卡片匹配的使用者，只能發送對我們有利的交易。 然而，ASF 的機制更加自由，它也接受均衡交易，因為在這種交易中，我們並沒有實際上的損失，所以沒有理由拒絕它們。 這對好友之間的交易特別有用，因為他們可以在不使用 STM 的情形下，交換您多餘的卡片，且不影響您的徽章進度。

在預設情形下，ASF 會拒絕不利的交易──這是做為普通使用者的您所想要的。 然而，您仍可以在 `TradingPreferences` 中設定 `MatchEverything`，使 ASF 接受所有重複物品交易，包含**不利交易**。 只有當您想要在您的帳號執行 1:1 交易的 Bot 時，這個功能才有用，因為您曉得 **ASF 將不再幫您完成徽章進度，且可能會因 N 張重複卡片，而使您損失收集進度**。 除非您有意執行一個交易 Bot，且**不**打算收集徽章進度，否則您不應啟用此選項。

不論您如何設定 `TradingPreferences`，被 ASF 拒絕的交易並不代表您無法自行接受。 若您保留 `BotBehaviour` 的預設值，裡面並不包含 `RejectInvalidTrades`，ASF 將忽略這些交易，讓您自行決擇。 同樣適用於 `MatchableTypes` 及其之外的物品，這個模組只用來幫助您自動化 STM 交易，而不是用來判斷交易的利弊。 這個規則的唯一例外是，被您使用 `tbadd` 指令加入交易模組黑名單的使用者：不論 `BotBehaviour` 如何設定，來自這些使用者的交易都會立即被拒絕。

強烈建議您，在啟用這個選項時使用 **[ASF 雙重驗證](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-zh-TW)**，因為如果您還需要手動確認每筆交易，這個功能也將會失去它的特點。 即使無法確認交易，`SteamTradeMatcher` 也能正常運作，但若您沒有及時手動確認，就會積欠許多確認請求。

---

### `MatchActively`

`MatchActively` 設定是 `SteamTradeMatcher` 的主動版本，包含 Bot 發送交易給其他使用者的交互式匹配。 它可以單獨運作，亦可結合 `SteamTradeMatcher` 設定一起運作。

為了使用這個選項，您需要滿足一系列的需求。 您需要擁有加入我們的 [ASF STM 清單](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Remote-communication#公開的 ASF STM 清單)</strong>的資格，但需求略為寬鬆。 您至少應保證帳號**[不受限制](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663)**、啟用**[ASF 雙重驗證](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication#asf-2fa)**，並在 `MatchableTypes` 中設定至少一種有效類型，例如交換卡片。

若您滿足了上述需求，ASF 將會定期與我們的 [公開的 ASF STM 清單](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Remote-communication#公開的 ASF STM 清單)</strong>通訊，已主動匹配當前可用的 Bot。

- 每個匹配階段都是由「輪次」組成，單次匹配階段最多可包含 `10` 輪。
- 每輪中，ASF 都會擷取我們的物品庫，並列出的所選 Bot 的物品庫，以便找到可以匹配的 `MatchableTypes` 物品。 若找到合適的配對，ASF 將自動發送並確認交易提案。
- 每套物品（具相同 appID、類型及稀有度）在一輪中只能匹配一次。 這是為了最大限度地減少「物品不再能用」的情形，無需在發送所有交易之前，等待每個 Bot 做出回應。 這也是為什麼以輪次來匹配，而不是持續進行的主要原因。
- ASF 不會在單次交易中送出超過 `255` 個物品，且每個輪次中不會向單個使用者送出超過 `5` 個交易提案。 這是由 Steam 的限制，及我們自己的負載平衡所加上的。
- 若沒有因為匹配的套數用完而取消，ASF 有 `40` 個唯一 Bot 的硬性限制──在這種情形下，ASF 會在下一輪優先匹配尚未匹配過的機器人。
- 如果 ASF 認為應該繼續匹配，下一輪將在 `5` 分鐘後開始（增加一些冷卻時間，使所有 Bot 對我們的交易做出回應），否則匹配階段結束，並在 `8` 小時後繼續。

這個模組應是透明的。 匹配將在 ASF 啟動後約 `1` 小時開始，並且每 `8` 小時重複一次（如果需要）。 `MatchActively` 功能旨在作為一項長期的定期措施，以確保我們積極地向收集完成的方向前進，但若將此作為指令使用，就會造成短期的時間及資源壓力。 這個模組的目標使用者是主要帳號及「用於存放物品的」備用帳號，但亦可被任何未設定成 `MatchEverything` 的 Bot 使用。

ASF 會盡力將使用該選項產生的請求量及壓力降至最低，同時最大限度地提高匹配效率。 匹配 Bot 及組織整個流程的演算法是 ASF 的實作細節，並且可以依據回饋、實際情形及未來的想法而改變。

當前版本的演算法會使 ASF 優先考慮 `Any` 標籤的 Bot，特別是那些擁有更多種遊戲物品的 Bot。 當 `Any` Bot 用完後，ASF 將依據相同遊戲物品的規則平均匹配 Bot。與其他 Bot 相比，擁有過多物品的 Bot 更有可能出現物品庫相關問題，而被進一步降低優先級。 無論如何，ASF 將至少嘗試匹配一次每個可用的 Bot，以保證我們不會錯過可能的徽章套卡進度。

`MatchActively` 會考慮您使用交易**[指令](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-TW)** `tbadd` 加入黑名單的 Bot 帳號，且不會嘗試匹配它們。 這能用來告訴 ASF 它不該匹配哪些 Bot，即使它們可能有重複物品能提供我們使用。