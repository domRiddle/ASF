# Items Matcher 外掛程式

`ItemsMatcherPlugin`&#8203;是ASF官方的&#8203;**[外掛程式](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins-zh-TW)**&#8203;，使用ASF STM名單來擴充ASF的功能。 特別是這包含了&#8203;**[`RemoteCommunication`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-TW#remotecommunication)**&#8203;中的&#8203;`PublicListing`&#8203;，及&#8203;**[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-TW#tradingpreferences)**&#8203;中的&#8203;`MatchActively`&#8203;。 ASF發行版本附隨了&#8203;`ItemsMatcherPlugin`&#8203;，因此能夠立即使用。

---

## `PublicListing`

顧名思義，公開名單是當前可供使用的ASF STM Bot的名單。 它&#8203;**[位於我們的網站上](https://asf.justarchi.net/STM)**&#8203;進行自動管理，並作為一項公共服務，提供使用&#8203;`MatchActively`&#8203;的ASF使用者及手動匹配的ASF與非ASF使用者使用。

為了要被列在名單中，您需要滿足一系列的需求。 您必須至少在&#8203;`RemoteCommunication`&#8203;中允許&#8203;`PublicListing`&#8203;（預設設定）、在&#8203;`TradingPreferences`&#8203;中啟用&#8203;`SteamTradeMatcher`&#8203;、設定成&#8203;**[公開物品庫](https://steamcommunity.com/my/edit/settings)**&#8203;的隱私設定、&#8203;**[不受限制的](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663)**&#8203;帳號，且啟用了&#8203;**[ASF雙重驗證](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-zh-TW#asf-雙重驗證)**&#8203;。 當然，您需要至少擁有一件符合&#8203;`MatchableTypes`&#8203;的物品，例如交換卡片。

雖然&#8203;`PublicListing`&#8203;預設為啟用，但請注意，如果您沒有滿足所有要求，您將&#8203;**不會**&#8203;被顯示在網站上，特別是要注意&#8203;`SteamTradeMatcher`&#8203;，它在預設情形下並未啟用。 對於不滿足條件的人，即使他們保持啟用&#8203;`PublicListing`&#8203;，ASF也不會以任何方式與伺服器通訊。 公開名單也只會與ASF最新的穩定版相容，並可能拒絕顯示過時的Bot，特別是如果它們缺少只能在新版本中找到的核心功能。

### 確切的運作方式

ASF會在登入後傳送一次初始資料，其中包含公開名單會使用的所有屬性。 然後每隔10分鐘，ASF會傳送一個非常微小的「心跳」請求，來通知我們的伺服器該Bot仍在執行。 如果由於某種原因該心跳未能送達，例如網路問題，那麼ASF將每分鐘重新傳送一次，直到被伺服器記錄下來。 透過這種方式，我們的伺服器可以準確知道哪些Bot仍在執行，並準備好接受交易提案。 ASF還會依據所需來傳送初始發布，例如它偵測到我們的物品庫跟上個紀錄相比發生了改變。

我們顯示在&#8203;**過去15分鐘**&#8203;內，所有啟用ASF雙重驗證及STM的活躍帳號。 使用者會依他們的相對有用性排序：首先是顯示帶有&#8203;`Any`&#8203;標題，接受所有1:1交易&#8203;`MatchEverything`&#8203;的Bot，然後依遊戲數量，最後依物品數量排序。

### API

ASF STM名單暫時只接受ASF Bot。 目前無法在我們的名單中顯示第三方Bot，因為我們無法輕易查看它們的程式碼，並確保它們符合我們的整個交易邏輯。 所以，參與名單需要最新的ASF穩定版本，雖然它仍可以使用自訂的&#8203;**[外掛程式](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins-zh-TW)**&#8203;來執行。

對於名單的使用者，我們有個非常簡單的&#8203;**[`/Api/Listing/Bots`](https://asf.justarchi.net/Api/Listing/Bots)**&#8203;端點供您使用。 它包含了我們擁有的所有資料，除了作為&#8203;`MatchActively`&#8203;功能一部分的使用者物品庫。

### 隱私權政策

若您同意顯示於我們的名單中，即如上所述，啟用&#8203;`SteamTradeMatcher`&#8203;且不拒絕&#8203;`PublicListing`&#8203;，我們將在我們的伺服器上臨時儲存一些您的Steam帳號詳細資料，以提供必要功能。

公開資訊（Steam向所有相關者公開的）包括：
- 您的Steam ID（64位元形式，用於生成連結）
- 您的暱稱（用於顯示）
- 您的個人檔案圖示（雜湊值，用於顯示）

條件性的公開資訊（Steam向所有符合名單需求相關者公開的）包括：
- 您的&#8203;**[物品庫](https://steamcommunity.com/my/inventory/#753_6)**&#8203;（使其他人可以對您的物品使用&#8203;`MatchActively`&#8203;）。

私人資訊（提供功能所需的特定資料）包括：
- 您的&#8203;**[交易權杖](https://steamcommunity.com/my/tradeoffers/privacy)**&#8203;（使非您好友的帳號能向您發送交易提案）
- 您的&#8203;`MatchableTypes`&#8203;設定（用於顯示及匹配）
- 您的&#8203;`MatchEverything`&#8203;設定（用於顯示及匹配）
- 您的&#8203;`MaxTradeHoldDuration`&#8203;設定（使其他人知道您是否願意接受他們的交易）

從您停止使用（停止顯示於）我們的名單後，您的資料最多儲存兩周，且在該段時間後自動刪除。

---

## `MatchActively`

`MatchActively`&#8203;設定是&#8203;**[`SteamTradeMatcher`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading-zh-TW#steamtradematcher)**&#8203;的主動版本，包含Bot發送交易給其他使用者的交互式匹配。 它可以單獨運作，亦可結合&#8203;`SteamTradeMatcher`&#8203;設定一起運作。 本功能需要設定&#8203;`LicenseID`&#8203;，因為它使用了第三方伺服器及付費資源來維持運作。

為了使用這個選項，您需要滿足一系列的需求。 您至少應保證帳號&#8203;**[不受限制](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663)**&#8203;、啟用&#8203;**[ASF雙重驗證](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-zh-TW#asf-雙重驗證)**&#8203;，並在&#8203;`MatchableTypes`&#8203;中設定至少一種有效類型，例如交換卡片。

若您滿足了上述需求，ASF將會定期與我們的&#8203;**[ASF STM公開名單](#publiclisting)**&#8203;通訊，以主動匹配當前可用的Bot。

在匹配期間，ASF Bot將會提取自己的物品庫，然後與我們的伺服器通訊，來從其他當前可用的Bot中找到所有可能的&#8203;`MatchableTypes`&#8203;匹配。 由於直接與我們的伺服器通訊，這個過程只需一個請求，我們就可以立即了解是否有任何可用的Bot能夠提供我們感興趣的東西⸺如果找到匹配物品，ASF將自動發送並確認交易提案。

本模組是透明的。 匹配將在ASF啟動後約&#8203;`1`&#8203;個小時後開始，並且每&#8203;`8`&#8203;個小時重複一次（如果需要）。 `MatchActively`&#8203;功能旨在作為一種長期的、定期的措施，用來確保我們積極朝著收集完整套的目標邁進。不過不是24小時都在執行ASF的使用者，也可以考慮使用&#8203;`match`&#8203;**[指令](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-TW)**&#8203;。 這個模組的目標使用者是主要帳號及「用於存放物品的」備用帳號，但亦可被任何未設定成&#8203;`MatchEverything`&#8203;的Bot使用。

ASF會盡力將使用該選項產生的請求量及壓力降至最低，同時盡可能提高匹配效率。 匹配Bot及組織整個流程的演算法是ASF的實作細節，並且可以依據回饋、實際情形及未來的想法而改變。

當前版本的演算法會使ASF優先考慮&#8203;`Any`&#8203;標籤的Bot，特別是那些擁有更多種遊戲物品的Bot。 當用完&#8203;`Any`&#8203; Bot時，ASF將會依據相同的多樣性規則改為考慮&#8203;`Fair`&#8203; Bot。 ASF將至少嘗試匹配一次每個可用的Bot，以保證我們不會錯過可以獲得的徽章套卡進度。

`MatchActively`&#8203;會考慮您使用&#8203;`tbadd`&#8203;**[指令](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-TW)**&#8203;加入交易黑名單的Bot帳號，且不會嘗試去匹配它們。 這能用來告訴ASF它不該匹配哪些Bot，即使它們可能有重複物品能提供我們使用。

ASF也會盡力使交易提案順利完成。 在下一輪匹配中（通常在6小時內），ASF將會取消任何尚未被接受的交易提案，降低對方的Steam ID優先級，並在之後會優先選擇更活躍的Bot。 儘管如此，如果低優先級的Bot是具有我們所需的匹配項目的最後一個Bot，我們仍會（再次）嘗試匹配他們。

---

### 為什麼我需要 `LicenseID` 才能使用 `MatchActively`？ 這之前不是免費的嗎？

ASF是也仍是免費且開源的，它是在2015年10月專案開始時建立的。 `ItemsMatcher`&#8203;外掛程式的原始碼與&#8203;`MatchActively`&#8203;在我們的儲存庫中提供，且ASF完全屬於非商業性程式，我們不會從對它的貢獻、建置或發布中獲取任何利益。 在過去7年多的時間裡，ASF得到了巨大的發展，且在每個月的穩定版本中仍不斷地改進與增強，這主要由一個人⸺**[JustArchi](https://github.com/JustArchi)**&#8203;⸺來完成的，無償付出。 我們唯一的資金來源來自於使用者的非強制性捐贈。

在很長的一段時間內，直到2022年10月前，&#8203;`MatchActively`&#8203;功能一直是ASF核心的一部分，來提供所有人使用。 在2022年10月時，Steam背後的公司Valve對提取其他Bot的物品庫設定了非常嚴格的速率限制，使得原先的功能完全崩潰，且沒有任何可行的解決方案。 因此，由於該功能已完全失效且無法修復，作為過時功能，已在5.4.1.0版本從ASF核心中移除。

`MatchActively`&#8203;作為官方&#8203;`ItemsMatcher`&#8203;外掛程式的一部分涅槃重生，該外掛程式進一步增強了ASF的交換卡片主動匹配功能。 恢復&#8203;`MatchActively`&#8203;功能需要我們&#8203;**大量的工作**&#8203;來建立ASF後端，這是代管在伺服器上的全新服務，加入了超過一百個用於解析物品庫的代理節點，而所有的這些都是為了讓ASF用戶端能像以前一樣使用&#8203;`MatchActively`&#8203;。 因為涉及的工作量過大，且所用資源並不是免費的，需要我們每月付款（網域、伺服器、網路代理），我們決定只向我們的贊助者提供此功能，也就是說，那些已在每個月支援ASF專案的人。感謝他們，我們才能夠使用並提供這些付費資源。

我們的目標並不是從中獲利，而是支付本功能相關的&#8203;**月費**&#8203;⸺這就是為什麼我們基本上算是無償提供它，但仍需為此收取一點費用的原因。我們無法每個月都自掏腰包支付數百美元，只是為了讓您可以使用它。 我們並不是真的在當商品討論售價，而是因為Valve把這些成本強加於我們之上，而我們的另一種選擇是根本完全不提供這項功能。如果您出於任何原因，無法認為我們插件的使用條款是合理的，您也可以選擇完全不使用本功能。

不論如何，我們了解不是每個人都支持&#8203;`MatchActively`&#8203;，且我們希望您也能了解為什麼我們無法免費提供它。

---

### 我要如何獲得使用權限？

`ItemsMatcher`&#8203;在JustArchi的GitHub上作為每月$5美元或以上贊助階級的一部分提供。 您也可以成為一次性贊助者，但在這種情形下，授權的有效期限只有一個月（可以使用相同的方式延期）。 只要成為$5美元（或更高）階級的贊助者，閱讀&#8203;**[組態設定](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-TW#licenseid)**&#8203;章節，以獲得並填寫&#8203;`LicenseID`&#8203;。 之後，您只需在您所選Bot的&#8203;`TradingPreferences`&#8203;中啟用&#8203;`MatchActively`&#8203;即可。

授權碼允許您向伺服器傳送有限數量的請求。 $5美元的階級允許您為一個Bot帳號使用&#8203;`MatchActively`&#8203;（每天4個請求），且每增加$5美元就能增加兩個Bot帳號（每天8個請求）。 舉例來說，若您想要在三個帳號上使用它，就必需使用$10美元或以上的階級。