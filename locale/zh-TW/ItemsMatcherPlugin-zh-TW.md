# Items Matcher 外掛程式

`ItemsMatcherPlugin`&#8203;是ASF官方&#8203;**[外掛程式](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins-zh-TW)**&#8203;，使用ASF STM清單來擴充ASF功能。 這特別包含了&#8203;**[`RemoteCommunication`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-TW#remotecommunication)**&#8203;中的&#8203;`PublicListing`&#8203;，及&#8203;**[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-TW#tradingpreferences)**&#8203;中的&#8203;`MatchActively`&#8203;。 ASF發行版本附隨了&#8203;`ItemsMatcherPlugin`&#8203;，因此能夠立即使用。

---

## `PublicListing`

顧名思義，公開清單是當前可供使用的ASF STM Bot的清單。 它位於&#8203;**[我們的網站](https://asf.justarchi.net/STM)**&#8203;上，進行自動管理並作為一項公共服務，提供使用&#8203;`MatchActively`&#8203;的ASF使用者及手動匹配的ASF與非ASF使用者使用。

雖然&#8203;`PublicListing`&#8203;預設為啟用，但請注意，如果您不滿足所有要求，您將&#8203;**不會**&#8203;被顯示在網站上，特別是&#8203;`SteamTradeMatcher`&#8203;，它在預設情形下並未啟用。 對於不滿足條件的人，即使他們保持啟用&#8203;`PublicListing`&#8203;，ASF也不會以任何方式與伺服器通訊。 公開清單也只會與ASF最新的穩定版相容，並可能拒絕顯示過時的Bot，特別是如果它們缺少只能在新版本中找到的核心功能。

### 確切的運作方式

ASF會在登入後傳送一次初始資料，其中包含公開清單使用的所有屬性。 然後每隔10分鐘，ASF會傳送一個非常微小的「心跳」請求，來通知我們的伺服器該Bot仍在執行。 如果由於某種原因該心跳未能送達，例如網路問題，那麼ASF將每分鐘重新傳送一次，直到被伺服器記錄。 透過這種方式，我們的伺服器可以準確知道哪些Bot仍在執行，並準備好接受交易提案。 ASF還會依據需要傳送初始發布，例如它偵測到我們的物品庫自上個紀錄以來發生了變化時。

我們顯示在&#8203;**過去15分鐘**&#8203;內，所有啟用ASF雙重驗證及STM的活躍帳號。 使用者會依他們的相對有用性排序：首先是顯示帶有&#8203;`Any`&#8203;標題，接受所有1:1交易的&#8203;`MatchEverything`&#8203; Bot，然後依遊戲數量，最後依物品數量排序。

### API

ASF STM清單暫時只接受ASF Bot。 目前無法在我們的清單中顯示第三方Bot，因為我們無法輕易查看它們的程式碼，並確保它們符合我們的整個交易邏輯。 所以，參與清單需要最新的ASF穩定版本，雖然它仍可以使用自訂&#8203;**[外掛程式](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins-zh-TW)**&#8203;執行。

對於清單的消費者，我們有個非常簡單的&#8203;**[`/Api/Listing/Bots`](https://asf.justarchi.net/Api/Listing/Bots)**&#8203;端點供您使用。 它包含了我們擁有的所有資料，但不包含作為&#8203;`MatchActively`&#8203;功能一部分的使用者物品庫。

### 隱私權政策

若您同意顯示於我們的清單中，即如上所述，啟用&#8203;`SteamTradeMatcher`&#8203;且不拒絕&#8203;`PublicListing`&#8203;，我們將在我們的伺服器上臨時儲存一些您的Steam帳號詳細資料，用以提供所需功能。

公開資訊（Steam向所有相關者公開的）包括：
- 您的Steam ID（64位元形式，用於生成連結）
- 您的暱稱（用於顯示）
- 您的頭像（雜湊值，用於顯示）

半公開資訊（Steam向所有符合清單需求相關者公開的）包括：
- 您的&#8203;**[物品庫](https://steamcommunity.com/my/inventory/#753_6)**&#8203;（使其他人可以對您的物品使用&#8203;`MatchActively`&#8203;）。

私人資訊（提供功能所需的特定資料）包括：
- 您的&#8203;**[交易權杖](https://steamcommunity.com/my/tradeoffers/privacy)**&#8203;（使非您好友的帳號能向您發送交易提案）
- 您的&#8203;`MatchableTypes`&#8203;設定（用於顯示及匹配）
- 您的&#8203;`MatchEverything`&#8203;設定（用於顯示及匹配）
- 您的&#8203;`MaxTradeHoldDuration`&#8203;設定（使其他人知道您是否願意接受他們的交易）

---

## `MatchActively`

`MatchActively`&#8203;設定是&#8203;**[`SteamTradeMatcher`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading-zh-TW#steamtradematcher)**&#8203;的主動版本，包含Bot發送交易給其他使用者的交互式匹配。 它可以單獨運作，亦可結合&#8203;`SteamTradeMatcher`&#8203;設定一起運作。 這個功能需要設定&#8203;`LicenseID`&#8203;，因為它使用了第三方伺服器及付費資源來運作。

為了使用這個選項，您需要滿足一系列的需求。 您至少應保證帳號&#8203;**[不受限制](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663)**&#8203;、啟用&#8203;**[ASF雙重驗證](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-zh-TW#asf-雙重驗證)**&#8203;，並在&#8203;`MatchableTypes`&#8203;中設定至少一種有效類型，例如交換卡片。

若您滿足了上述需求，ASF將會定期與我們的&#8203;**[公開的ASF STM清單](#publiclisting)**&#8203;通訊，以主動匹配當前可用的Bot。

- 每輪中，ASF 都會擷取我們的物品庫，並列出所有可用Bot的物品庫，以便找到可以匹配的&#8203;`MatchableTypes`&#8203;物品。 由於直接與我們的伺服器通訊，這個過程只需一個請求，我們就可以立即了解是否有任何可用的Bot能夠提供我們感興趣的東西⸺如果找到匹配物品，ASF將自動發送並確認交易提案。
- 每套物品（具相同appID、類型及稀有度）在一輪中只能匹配一次。 這是為了盡可能減少「物品不再能用」的情形，無需在發送所有交易之前，等待每個Bot做出回應。 這也是為什麼以輪次來匹配，而不是持續進行的主要原因。
- ASF不會在單次交易中送出超過&#8203;`255`&#8203;個物品，且每個輪次中不會向單個使用者送出超過&#8203;`5`&#8203;個交易提案。 這是由Steam的限制，及我們自己的負載平衡所加上的。

這個模組應是透明的。 匹配將在ASF啟動後約&#8203;`1`&#8203;個小時後開始，並且每&#8203;`8`&#8203;個小時重複一次（如果需要）。 `MatchActively`&#8203;功能旨在作為一項長期的定期措施，以確保我們積極地向收集完成的方向前進，但若將此作為指令使用，就會造成短期的時間及資源壓力。 這個模組的目標使用者是主要帳號及「用於存放物品的」備用帳號，但亦可被任何未設定成&#8203;`MatchEverything`&#8203;的Bot使用。

ASF會盡力將使用該選項產生的請求量及壓力降至最低，同時盡可能提高匹配效率。 匹配Bot及組織整個流程的演算法是ASF的實作細節，並且可以依據回饋、實際情形及未來的想法而改變。

當前版本的演算法會使ASF優先考慮&#8203;`Any`&#8203;標籤的Bot，特別是那些擁有更多種遊戲物品的Bot。 當用完&#8203;`Any`&#8203; Bot時，ASF將會依據相同的多樣性規則改為考慮&#8203;`Fair`&#8203; Bot。 ASF將至少嘗試匹配一次每個可用的Bot，以保證我們不會錯過可能的徽章套卡進度。

`MatchActively`&#8203;會考慮您使用交易&#8203;**[指令](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-TW)**&#8203;`tbadd`&#8203;加入黑名單的Bot帳號，且不會嘗試匹配它們。 這能用來告訴ASF它不該匹配哪些Bot，即使它們可能有重複物品能提供我們使用。

---

### 為什麼我需要 `LicenseID` 才能使用 `MatchActively`？ 這之前不是免費的嗎？

ASF是也仍是免費且開源的，它是在2015年10月專案開始時建立的。 我們的程式也完全是非商業性的，我們不會從對其的貢獻、建置或發布的檔案中獲取任何收益。 在過去7年多的時間裡，ASF得到了巨大的發展，且在每個月的穩定版本中仍不斷地改進與增強，這主要由一個人⸺**[JustArchi](https://github.com/JustArchi)**&#8203;⸺來完成的，無償付出。 我們唯一的資金來源來自於使用者的非強制性捐贈。

在很長的一段時間內，直到2022年10月，&#8203;`MatchActively`&#8203;功能一直是ASF核心的一部分，來提供所有人使用。 在2022年10月時，Steam背後的公司Valve設定了非常嚴格的速率限制，使得原先的功能完全崩潰，且沒有任何可行的解決方案。 因此，該功能必須從5.4.1.0版本的ASF核心中刪除。

`MatchActively`&#8203;作為官方&#8203;`ItemsMatcher`&#8203;外掛程式的一部分涅槃重生，該外掛程式進一步增強了ASF的交換卡片主動匹配功能。 恢復&#8203;`MatchActively`&#8203;功能需要我們&#8203;**大量的工作**&#8203;來建立ASF後端，這是代管在伺服器上的全新服務，加入了超過一千個用於解析物品庫的代理節點，而所有的這些都是為了讓ASF用戶端能像以前一樣使用&#8203;`MatchActively`&#8203;。 因為涉及的工作量過大，且所用資源並不是免費的，需要我們每月付款（網域、伺服器、網路代理），我們決定只向我們的贊助者提供此功能，也就是說，那些已在每個月支援ASF專案的人。 我們的目標並不是從中獲利，而是支付本功能相關的&#8203;**月費**&#8203;⸺這就是為什麼我們基本上免費提供它，但我們仍需為此收取一點費用的原因，因為我們無法自掏腰包支付數百美元，只是為了讓您可以使用它。 我們希望您能夠理解。

---

### 我要如何獲得使用權限？

`ItemsMatcher`&#8203;在JustArchi的GitHub上作為$5美元或以上贊助階級的一部分提供。 只需成為$5美元（或更高）階級的贊助者，然後閱讀&#8203;**[組態設定](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-TW#licenseid)**&#8203;章節，以獲得並填寫&#8203;`LicenseID`&#8203;。

授權碼允許您向伺服器傳送有限數量的請求。 $5美元的階級允許您為一個帳號使用&#8203;`MatchActively`&#8203;，這應該適合大多數人。 $10美元的階級允許您在三個帳號上使用它。 若您需要更多資源，請&#8203;**[聯絡我們](mailto:ASF@JustArchi.net)**&#8203;。