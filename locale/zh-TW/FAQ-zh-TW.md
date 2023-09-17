# 常見問題

常見問題包含了您可能會經常遇到的問題及它們的解答。 如果您的問題較不常見，請參閱&#8203;**[其他常見問題](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Extended-FAQ-zh-TW)**&#8203;。

# 目錄

* [基本問題](#基本問題)
* [與相似的工具比較](#與相似的工具比較)
* [安全／隱私／VAC／封鎖／服務條款](#安全隱私vac封鎖服務條款)
* [其他](#其他)
* [執行問題](#執行問題)

---

## 基本問題

### 什麼是 ASF？
### 為什麼程式提示我的帳號沒有卡片可以掛？
### 為什麼 ASF 沒有偵測到我全部的遊戲？
### 為什麼我的帳號受到限制？

在嘗試了解什麼是ASF之前，你應該先了解什麼是Steam交換卡片以及它的獲得方式，這在官方的&#8203;**[常見問題](https://steamcommunity.com/tradingcards/faq)**&#8203;中有詳細的說明。

簡而言之，Steam交換卡片是種可收集物品，在您擁有特定的遊戲後即可獲得它們。交換卡片可用於合成徽章、於Steam社群市集中販售，或其他您想對它們做的事。

這裡再次強調核心重點，因為人們通常不想同意，並無視這些事實：

- **您必須在您的Steam帳號上擁有相應的遊戲，才有資格從該遊戲掉落卡片。 親友同享的遊戲不包含在內。**
- **您無法無限制地掛卡，每款遊戲的交換卡片皆有固定掉落的數量。 只要您掛完這些卡片（約等同於整套卡片的一半數量），這款遊戲將不再能掛出新卡片。 這無關您出售、交易、合成或其他您對已獲得卡牌的行為，只要可掉落的數量用完，這款遊戲就是掛完了。**
- **如果您沒有在免費遊戲中消費，這類遊戲將不會掉落卡片。 這是指永久免費遊玩的遊戲，例如《絕地要塞2》或《Dota 2》。 擁有免費遊戲無法讓您掉落出卡片。**
- **不論是否擁有遊戲，&#8203;[受限使用者帳戶](https://support.steampowered.com/kb_article.php?ref=3330-iagk-7663&l=traditional%20chinese)都無法掉落交換卡片。 （以前曾可以掉落，但現在已經沒辦法了。）**
- **不論在商店頁面顯示什麼，您在促銷期間以免費方式獲得的付費遊戲，將無法掉落交換卡片。 （以前曾可以掉落，但現在已經沒辦法了。）**

如您所見，Steam交換卡片是您遊玩付費遊戲，或在免費遊戲中消費的獎勵。 若您遊玩一款遊戲足夠長的時間，該遊戲的所有卡片最終都會掉落至您的物品庫中，這使您能夠（在獲得另外半套後）有機會完成一個徽章、賣掉它們，或其他任何您想處置的方式。

現在我們已經解釋了Steam的基礎知識，接下來要開始說明ASF。 程式本身非常複雜且難以完全理解，所以我們接下來只會簡單介紹，並不會深入解釋完整的技術細節。

ASF使用您提供的憑證，透過我們內建的自訂Steam用戶端來登入您的Steam帳號。 在登入成功後，它會剖析您的&#8203;**[徽章頁面](https://steamcommunity.com/my/badges)**&#8203;，來搜尋可掛卡的遊戲（還有&#8203;`X`&#8203;張卡片會掉落）。 在剖析完所有頁面，並建構出最終的掛卡遊戲清單後，ASF會選擇最有效的掛卡演算法並開始掛卡。 掛卡過程取決於您所選擇的&#8203;**[掛卡演算法](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance-zh-TW)**&#8203;，這通常包含執行所選的遊戲，及定期（與有物品掉落時）檢查該遊戲是否已完成掛卡⸺若已掛完，ASF將會換下一款遊戲繼續掛卡，重複這個過程，直到所有遊戲都完成掛卡。

請注意，上述說明是簡化過的，且也沒有描述ASF所提供的諸多額外功能。 若您想了解ASF的所有細節，請造訪我們&#8203;**[Wiki](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Home-zh-TW)**&#8203;的其餘頁面。 我盡量在此處以淺顯易懂的方式說明，在不需要知道技術細節的情形下，讓每個人都能理解⸺並鼓勵進階使用者深入研究。

作為一個程式，ASF提供了一些魔法。 首先，它不需要下載任何遊戲檔案，就能夠立刻遊玩遊戲。 其次，它完全獨立於您的Steam用戶端⸺您不需要執行Steam用戶端，甚至也不需要安裝。 再者，這是個自動化的解決方案⸺這代表ASF會自動為您處理事情，而不需要您告訴它該如何⸺這為您解決了麻煩並節省了時間。 最後，它毋須使用程序模擬來欺騙Steam網路（而Idle Master正是這樣做），因為ASF能直接與Steam網路通訊。 它很快速、且為輕量級，是個完美的解決方案，讓每個人都能輕鬆獲得交換卡片⸺您可以讓它在後台運作，而去做其他的事情，甚至在離線模式下遊玩。

除了上述優點，ASF也有些因Steam造成的技術限制⸺我們無法掛您並未擁有的遊戲，無法掛出超過限制的額外卡片，亦無法在您遊玩時同時掛卡。 考慮到ASF的運作原理，這些限制應該十分合理。並值得一提的是，ASF並沒有超能力，也無法超越物理限制，所以請注意⸺它就像是您讓別人從另一台PC登入您的帳號，並進行掛卡是一樣的。

綜上所述⸺ASF是個能為您省去麻煩，並幫助您掉落可獲得的卡片的程式。 它還提供了一些額外的功能，但讓我們先只專注於掛卡功能中。

---

### 我必須輸入我的帳號憑證嗎？

**是的**&#8203;。 ASF與Steam官方用戶端相同，需要您的帳號憑證，因為它使用了相同的方法來與Steam網路交互。 但這並不代表您就必須將帳號憑證放在ASF設定檔中，可以在&#8203;`SteamLogin`&#8203;及／或&#8203;`SteamPassword`&#8203;屬性保留&#8203;`null`&#8203;或空值，並在每次執行ASF時輸入所需的資訊（及其他相關的登入憑證，請參閱組態設定章節）。 這樣，您的資訊就不會儲存在任何地方，但在這種情形下，如果沒有您的協助，ASF將會無法自動啟動。 ASF也提供了幾種方式來增加您的&#8203;**[安全性](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security-zh-TW)**&#8203;，因此若您是進階使用者，可以閱讀該部分的Wiki。 若您只是一般使用者，且您不想要在ASF設定檔中留下您的帳號憑證，則不需要這樣做，並在ASF要求這些資訊時輸入它們。

請注意，ASF作為一個個人使用的工具，您的憑證永遠不會離開您的電腦。 您也不應主動將憑證分享給任何人，因為這會違反&#8203;**[Steam服務條款](https://store.steampowered.com/subscriber_agreement)**&#8203;⸺一份沒人記得但卻非常重要的規定。 您的詳細資料不會被傳送至我們的伺服器或第三方中，只會直接傳送至Valve營運的Steam伺服器中。 不論您是否將憑證放在設定檔中，我們都無從得知您的憑證，亦無法幫您恢復。

---

### 我需要等多久，卡片才會掉落？

認真地說，&#8203;**「不論多久，您都必須等待」**&#8203;。 每款遊戲的掛卡難度是由該遊戲的開發者／發行者所訂，只有他們才能決定交換卡片掉落的速率。 大多數的遊戲會每隔約30分鐘掉落一張交換卡片，但也有遊戲會需要您遊玩數個小時後才會開始掉落。 除此之外，若您未具有足夠的遊玩時數，您的帳號可能受到掉卡限制，如&#8203;**[效能](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance-zh-TW)**&#8203;章節中所述。 請不要嘗試去猜測ASF掛卡所需的時間⸺這無法由您或ASF來決定。 您無法加速掛卡進度，也不存在卡片能無法及時掉落的「錯誤」⸺您無法控制交換卡片掉過的過程，ASF也無法。 在最理想的情形下，您將每隔30分鐘獲得一張交換卡片。 而在最差的情形下，可能在您啟動ASF四個小時內都沒有掉落任何卡片。 這些情形都是正常的，且在我們的&#8203;**[效能](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance-zh-TW)**&#8203;章節中有介紹。

---

### 掛卡耗費時間太長，我該怎麼讓它加快速率？

能嚴重影響掛卡速率的唯一因素是您Bot實例所選擇的&#8203;**[掛卡演算法](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance-zh-TW)**&#8203;。 其他的因素只能帶來微不足道的影響，無法加速掛卡進度，而部分行為例如多次重啟ASF程序，甚至會&#8203;**使進度更為緩慢**&#8203;。 若您真的想要充分地利用掛卡過程的每一秒鐘，ASF允許您微調一些與掛卡相關的變數，例如&#8203;`FarmingDelay`&#8203;⸺這些參數都在&#8203;**[組態設定](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-TW)**&#8203;中有進一步的說明。 但正如我所說的，這些的影響可說是微不足道，為您的帳號選擇正確的掛卡演算法才是唯一嚴重影響掛卡速率的關鍵因素，其他都只能作為錦上添花之效。 不要過分擔心掛卡速率，只要啟動ASF並讓它自主運作⸺我可以向您保證，這是我能想得到的最有效的掛卡方式。 認真就輸了，隨興就好。

---

### 但 ASF 說掛卡會耗費大約 X 個小時！

ASF會依據您可掉落的交換卡片數量及您選擇的演算法，來預估所需要的時間⸺這與您實際掛卡要花費的時間無關，通常會需要更久的時間⸺因為ASF只會假設最理想的情形，並忽略所有Steam網路異常、網際網路斷線、Steam伺服器超載等問題。 它只應被當成您在理想情形下，來粗估ASF掛卡時間的指標，因為實際情形會有所不同，甚至差異巨大。 如上所述，請不要嘗試去猜測遊戲所需的掛卡時間，因為您無法決定它，ASF也無法。

---

### ASF 可以在我的 Android／智慧型手機上執行嗎？

ASF是個C#程式，需要.NET環境來執行。 從.NET 6.0開始，Android就已成為一個受支援的平台，但因為目前仍&#8203;**[缺少可供使用的ASP.NET執行環境](https://github.com/dotnet/aspnetcore/issues/35077)**&#8203;，在Android執行ASF仍受阻礙。 儘管尚未有原生的選擇，但目前在ARM架構上仍有可運作於GNU/Linux的組建版本，所以完全能夠使用像&#8203;**[Linux Deploy](https://play.google.com/store/apps/details?id=ru.meefik.linuxdeploy)**&#8203;等方式安裝Linux，然後如平常那樣在Linux chroot中執行ASF。

在所有ASF的需求都被滿足時，我們就會考慮發布官方版的Android組建版本。

---

### ASF 可以掛 Steam 遊戲物品嗎，例如 CS:GO 或 Unturned？

**不能**&#8203;，這違反&#8203;**[Steam服務條款](https://store.steampowered.com/subscriber_agreement)**&#8203;，且Valve在上一次對掛機獲得TF2物品的帳號實施大規模社群封鎖時，就明確表示過了。 ASF是一個Steam交換卡片的掛卡程式，而非遊戲物品掛機工具⸺它沒有任何能掛遊戲物品的能力，也無計畫在未來加入這類功能，主要是因為這違反Steam服務條款。 請勿詢問這類問題⸺您能獲得的最佳答案便是來自鄉民的檢舉，使您陷入更大的麻煩。 這同樣適用於其他所有種類的掛機，例如在CS:GO直播中掛機獲取掉落物品。 ASF只會專注於Steam交換卡片上。

---

### 我可以選擇要掛哪些遊戲嗎？

**可以**，這有幾種不同的方法。 若您想調整預設的掛卡順序，可以修改&#8203;`FarmingOrders`&#8203;**[Bot設定屬性](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-TW#bot-設定檔)**&#8203;。 若您想手動將某些遊戲列入自動掛卡黑名單，可以使用&#8203;`fb`&#8203;**[指令](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-TW)**&#8203;來設定黑名單。 若您想掛所有的卡，且想要先掛某一部份，可以使用&#8203;`fq`&#8203;**[指令](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-TW)**&#8203;來管理優先掛卡佇列。 最後，若您只想掛您想要的遊戲，那您可以使用&#8203;`FarmPriorityQueueOnly`&#8203;**[Bot設定屬性](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-TW#bot-設定檔)**&#8203;來達成，並將您所選的應用程式加入至優先掛卡佇列中。

除了管理上述的自動掛卡模組以外，您也可以使用手動掛卡模式&#8203;`play`&#8203;**[指令](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-TW)**&#8203;，或使用一些雜項的外部設定，例如&#8203;`GamesPlayedWhileIdle`&#8203;**[Bot設定屬性](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-TW#bot-設定檔)**&#8203;。

---

### 我對掉落的卡片不感興趣，我想改為掛遊戲時數，這可以做到嗎？

可以，ASF允許您以幾種方式做到。

最好的方法是設定&#8203;**[`GamesPlayedWhileIdle`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-TW#gamesplayedwhileidle閒置時掛卡的遊戲)**&#8203;設定屬性，ASF會在無卡可掛時掛您所選的AppID。 若您希望在其他遊戲仍未掛完卡時，也始終掛您所指定的遊戲，那麼可以接著設定&#8203;**[`FarmPriorityQueueOnly`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-TW#farmpriorityqueueonly只掛卡優先佇列)**&#8203;，這樣ASF就只會掛那些您明確設定出的遊戲；或設定&#8203;**[`Paused`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-TW#paused暫停)**&#8203;暫停掛卡模組，直到您取消暫停。

或者，您也可以使用&#8203;**[`play`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-TW##指令-1)**&#8203;指令，來使ASF開始掛您所選的遊戲。 但請注意，&#8203;`play`&#8203;只應在您需要臨時掛遊戲時使用，因為它並不是個持續指令，若ASF遇到例如像Steam網路斷線等情形，就會恢復成預設狀態。 因此，我們建議您使用&#8203;`GamesPlayedWhileIdle`&#8203;，除非您確實想在短時間內掛您選擇的遊戲，然後再恢復成一般的掛卡流程。

---

### 我是 Linux/macOS 的使用者，ASF 可以掛我的作業系統不支援的遊戲嗎？ 在我執行 32 位元作業系統時，ASF 可以掛 64 位元的遊戲嗎？

可以，ASF甚至不需要下載實際的遊戲檔案，因此不論遊戲的平台或技術的要求為何，它都能正確運作於您Steam帳號下的授權遊戲。 即使您不在限制區域內，儘管我們無法保證，但它仍能執行區域限制（鎖區）的遊戲。（在我們上次測試時，可以這樣運作）

---

## 與相似的工具比較

---

### ASF 與 Idle Master 相似嗎？

兩個程式唯一的相似之處只有它們的開發目的，就是掛機Steam遊戲來獲得掉落的交換卡片。 其他任何方面，包含實際的掛卡方式、使用的演算法、程式結構、功能性、相容性，最後連原始碼本身，都是完全不一樣的，兩者並沒有任何共同之處，甚至連核心介面都是不同的⸺IM執行於.NET Framework，而ASF是.NET (Core)。 建立ASF是為了解決IM的問題，這些問題無法透過簡單地編輯程式碼來解決⸺這也是為什麼ASF從零開始編寫，而未使用任何一行IM的程式碼，甚至是它的開發想法，因為它的原始碼及邏輯一開始就存在缺陷。 IM與ASF的關係就如同Windows及Linux⸺兩者都是能安裝於您PC上的作業系統，但除了服務這個目的相似外，其餘地方毫無相關性。

這也是為什麼您不應拿對於IM的期望，來將它與ASF做比較。 您應該將ASF與IM視為擁有各自獨家功能的兩個獨立程式。 您可以在兩者間找到重疊的功能，但能找到的非常少，因為ASF以完全不同於IM的方式來達成目的。

---

### 如果我目前已在使用 Idle Master，且它用起來還可以，我有必要換成 ASF 嗎？

**是的**&#8203;。 ASF更加可靠，且含有許多內建功能，不論您以何種方式掛卡，這些功能都是非常&#8203;**關鍵**&#8203;的，而IM卻並未提供這些。

ASF擁有正確的邏輯來處理&#8203;**未發行的遊戲**&#8203;；而IM會嘗試去掛這些已有交換卡片的遊戲，即使它們尚未發行。 當然，在遊戲發行前是不可能掛卡的，所以您的掛卡程序就會被卡住。 此時您就只能將它加入至黑名單、等待遊戲發行，或手動跳過這款遊戲了。 這幾種解決方式都並不友善，都需要您的操作⸺而ASF會自動（暫時）跳過未發行遊戲的掛卡，並在它們發行後恢復掛卡，完美地避免了上述問題。

ASF也擁有正確的邏輯來處理&#8203;**影集**&#8203;。 Steam上有許多影片也擁有交換卡片，但在徽章頁面上它們被標記了錯誤的&#8203;`appID`&#8203;，例如&#8203;**[Double Fine Adventure](https://store.steampowered.com/app/402590)**&#8203;⸺IM會以錯誤的&#8203;`appID`&#8203;掛卡，使程序卡住，導致無法獲得任何卡片。 與上述情形相同，您得將它加入至黑名單或手動跳過，這都需要您的操作。 而ASF會自動找到正確的&#8203;`appID`&#8203;來掛卡，使您能夠獲得交換卡片。

除此之外，ASF在遇到網路問題或Steam問題時&#8203;**更穩定且更可靠**&#8203;⸺ASF在絕大多數時間都能正常運作，只要正確設定後，就不用您再次操作；而大多數人使用IM時都會遇到各種問題，需要額外的修復操作，甚至完全無法使用。 它甚至完全相依於您的Steam用戶端，這代表在您Steam用戶端遇到問題時，它也無法正常運作。 ASF只要能連線至Steam網路即可正常運作，甚至毋須執行或安裝Steam用戶端。

上述三點皆為您該考慮使用ASF的&#8203;**重要**&#8203;原因，因為它們會直接影響到每個使用者的掛卡過程，沒有人能肯定這「不會影響到我」，因為Steam的維護及問題會影響到所有人。 您可以在本章節中的其餘部分，了解到更多重要及不那麼重要的原因。 因此長話短說，&#8203;**是的**&#8203;，所以即使您不需要與IM相比的ASF額外功能，也應該考慮使用ASF。

除此之外，IM的專案已正式停止維護，在未來有可能完全損壞，因為現在有更強大的解決方案（不只是ASF），所以沒有人願意繼續修復它了。 已有很多使用者無法再使用IM，而這個數字只會越來越多不會減少。 您應始終避免使用過時的軟體，不只是IM，其他所有廢止的程式也該如此。 無人維護代表沒有人在乎它是否還能運作，沒有人驗證，&#8203;**也沒有人對它的功能負責**&#8203;，這是一個非常嚴重的安全性問題。 只要出現一個造成Steam基礎架構嚴重問題的致命錯誤就夠了⸺如果無人修復，Steam可能會有新一波的封鎖潮，您有可能在不知情的情形下受到影響。猜猜哪些人經歷過這樣的事情呢？就是那些使用過時版本ASF的使用者。

---

### ASF 提供了哪些 Idle Master 沒有的有趣功能？

這取決於您所定義的「有趣」。 若您計畫為不只一個帳號掛卡，那這個答案顯而易見，因為ASF使您能夠以一套優秀的解決方案來為所有帳號掛卡，節省資源、省去麻煩，並避開相容性問題。 但要是您會問這種問題，那您很可能還沒有這種特殊需求，因此，先來讓我們說明使用ASF掛卡單一帳號的好處在哪。

首先，您擁有&#8203;**[上述](#如果我目前已在使用-idle-master且它用起來還可以我有必要換成-asf-嗎)**&#8203;的內建功能，這是掛卡功能的核心，不論您的掛卡目標為何，這都足以讓您考慮使用ASF。 您在上面已經了解這個部份了，所以我們將介紹一些更有趣的功能：

- **您可以使用離線模式掛卡**&#8203;（&#8203;`OnlineStatus`&#8203;中的&#8203;`Offline`&#8203;功能）。 離線模式掛卡使您能夠隱藏您Steam「正在遊戲中」的狀態，這樣您可以在顯示「正在線上」的同時使用ASF繼續掛卡，您的朋友們就不會發現ASF正在代替您玩遊戲。 這是一個絕佳的功能，它使您能夠在Steam用戶端維持在線狀態，但又不會因頻繁的遊戲切換而打擾到您的朋友們，或讓他們誤認為您正在玩遊戲。 若您尊重朋友，只憑這一點就應使用ASF，但這仍只是個開始。 值得一提的是，這個功能與您的Steam隱私設定無關⸺若您自行啟動遊戲，您仍會向朋友們顯示正確的遊戲狀態；只有ASF啟動的遊戲會被隱藏，且這並不會影響到您的帳號。

- **您可以跳過仍可退款的遊戲**&#8203;（&#8203;`SkipRefundableGames`&#8203;的功能）。 ASF擁有正確的內建邏輯來處理仍可退款的遊戲，您可以設定ASF不要自動掛它們。 這樣您就能夠自行評估那些從Steam商店購買的遊戲是否值這個價格，而不是讓ASF盡快掛完遊戲的卡片。 若您已遊玩超過2小時，或購買超過2周，那麼ASF將會繼續掛這款遊戲，因為它不再能退款。 在此之前，您都有該遊戲的完整控制權，不論您是否滿意，都可以直接退款該遊戲，整個流程您都毋須手動將它加入黑名單中，也不需要在這段期間停用ASF。

- **您可以自動將新物品的通知自動標示成已讀**&#8203;（&#8203;`BotBehaviour`&#8203;中的&#8203;`DismissInventoryNotifications`&#8203;功能）。 使用ASF掛卡會使您的帳號收到新的交換卡片。 這是您已經知道會發生的事情，所以就讓ASF幫您清理這些無用的通知，來確保您只會注意到那些重要通知。 當然，這取決於您。

- **您可以在Steam特賣期間自動獲得活動的交換卡片**&#8203;（&#8203;`AutoSteamSaleEvent`&#8203;的功能）。 若您想要，ASF可以在Steam特賣期間自動探索佇列。 這能在Steam特賣期間為您節省大量時間，並能確保您再也不會錯過每日獲得卡片的機會。

- **您可以使用更多選項來自定掛卡順序**&#8203;（&#8203;`FarmingOrders`的功能）。 或許您會想要先掛新買的遊戲？ 或最早購買的？ 依據能掉卡的數量去做排序？ 您已合成的徽章等級排序？ 遊玩時數？ 字母順序？ 依據AppID？ 或完全隨機？ 這完全取決於您。

- **能夠協助您收集完成徽章進度**&#8203;（&#8203;`TradingPreferences`&#8203;中的&#8203;`SteamTradeMatcher`&#8203;功能）。 透過更進階的調整，您可以將ASF轉換成全功能的使用者機器人，能自動接受&#8203;**[STM](https://www.steamtradematcher.com)**&#8203;交易提案，幫助您每天匹配交易，而不需要您的操作。 ASF甚至包含了獨有的ASF雙重驗證模組，使您能夠匯入自己的Steam行動驗證器，讓您能完全自動化整個交易流程。 或許您想要ASF為您只準備交易提案，但仍需自行手動確認？ 再次強調，這完全取決於您。

- **ASF可以在背景幫您兌換產品序號**&#8203;（&#8203;**[背景序號啟動器](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer-zh-TW)**&#8203;的功能）。 也許您擁有來自各種組合包、數以百計的產品序號，但您懶得一遍又一遍地打開視窗，同意Steam條款，並輸入產品序號。 這樣的話，何不將您的序號清單複製貼上給ASF，並讓它幫您完成這項工作呢？ ASF會在背景自動兌換這些產品序號，並為您提供對應的輸出訊息，讓您能得知每次兌換後的結果。 另外一點，若您有上百個序號，您將很快會觸發Steam的頻率限制，ASF也知道這一點，所以它會耐心等候頻率限制失效，並從中斷處繼續兌換。

雖然我們可以繼續在此列出&#8203;**[ASF Wiki](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Home-zh-TW)**&#8203;中的所有功能，但我們需要在這裡停下。 而這些就是您作為ASF使用者可以享受到的功能清單，每一項應該都能使您再也離不開ASF。實際上，我們也列出了&#8203;**更多的功能**&#8203;，您可以深入閱讀剩餘的Wiki來了解它們。 啊，是啊，為了使說明不要過於長篇大論，我們甚至都還沒能詳細介紹ASF的API，它使您能夠自行編寫腳本命令，或用於很讚的機器人管理。

---

### ASF 的效率比 Idle Master 好嗎？

**是的**&#8203;，但解釋起來稍顯複雜。

每當您的系統產生或終止一個新程序，Steam用戶端都會自動傳送一個含有您所有當前正在遊玩的遊戲的請求⸺這樣Steam網路就能計算您的遊玩時數，並依此掉落卡片。 但是，Steam網路是以1秒為單位計算您的遊玩時數，且傳送新的請求會重置當前的狀態。 也就是說，若您每隔0.5秒就產生／終止一個新程序，您將永遠無法掉落任何卡片，因為Steam用戶端每0.5秒傳送一個新請求，但因不足1秒，所以Steam網路無法計算這些遊玩時數。 此外，因為作業系統的運作方式，即使您什麼也不做，實際上依然也會有許多新程序在產生／終止⸺有許多程序仍在背景持續運作，並隨時產生／終止其他的程序。 Idle Master基於Steam用戶端，若您使用它，那麼您也會受此機制的影響。

ASF並非基於Steam用戶端，而是使用自身的Steam用戶端來實作。 正因如此，ASF所做的並不是產生一個程序，而是向Steam網路傳送一個真正的請求，來表示我們已開始玩遊戲。 這與Steam用戶端所傳送的請求相同，但因我們能實際操控ASF所自帶的Steam用戶端，我們毋須產生新程序，也不需要在每次程序變更時模仿Steam用戶端傳送請求，因此，上述機制對我們毫無影響。 由於這項原因，我們永遠不會在Steam網路上有著1秒間隔的中斷事件，這也加快了掛卡速率。

---

### 但是這個差異有這麼明顯嗎？

並沒有。 一般的Steam用戶端與Idle Master所發生的中斷事件對掉卡速率的影響可以忽略不計，因此並沒有非常明顯的差別來讓ASF顯得更優異。

但，這仍&#8203;**是個**&#8203;差別，若您的作業系統極端繁忙，您可能會注意到卡片&#8203;**會**&#8203;加速掉落幾秒，甚至幾分鐘。 雖然我不會只因ASF的掉卡速率更快而選擇它，因為ASF與Idle Master都受到Steam網路的影響，但ASF能以更高效的方式與Steam網路互動，而Idle Master卻沒有能力控制Steam用戶端的行為（這不是Idle Master的問題，而是Steam用戶端本身的）。

---

### ASF 可以同時掛多個遊戲嗎？

**可以**&#8203;，ASF會依據所選的&#8203;**[掛卡演算法](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance-zh-TW)**&#8203;得知使用本功能的時機。 在同時掛多款遊戲時，卡片的掉落率會趨近於零，這就是為什麼ASF只會在批量掛遊玩時數到&#8203;`HoursUntilCardDrops`&#8203;時，才會一次掛最多&#8203;`32`&#8203;款遊戲。 這也是為何您應該只專注於ASF的設定的原因，讓演算法去幫您決定達成目的的最佳解⸺您所認為的方式，實際上不一定是最好的方式，一次掛多款遊戲會讓您掉不了卡。

---

### ASF 可以快速切換遊戲嗎？

**不能**&#8203;，ASF既不支援，與不鼓勵使用&#8203;**[Steam故障](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance-zh-TW#steam-故障)**&#8203;。

---

### ASF 可以自動為每個尚未加入交換卡片的遊戲掛 X 個小時嗎？

**不能**&#8203;，Steam卡片系統的改動主要是用於對付錯誤的統計資訊及非人類玩家。 ASF不會對這類行為作出貢獻，不計劃加入這些功能，它們也不會發生於此。 若遊戲的交換卡片能正常掉落，ASF仍會盡快掛完它們。

---

### 我可以在 ASF 掛卡時玩遊戲嗎？

**不能**&#8203;。 ASF與IM不同，含有獨立的Steam用戶端，且Steam網路&#8203;**一次只會允許一個Steam用戶端**&#8203;玩遊戲。 但您可以透過像啟動遊戲等行為來中斷ASF的連線（若Steam詢問您是否要中斷其他連線階段，請點擊「繼續」）⸺ASF將會耐心等待您玩完，並在此之後繼續掛卡。 或您願意的話，也可以在離線模式下遊玩。

請注意，同時遊玩多個遊戲，會使掉卡速率掉至0，因此IM在這個功能上沒有辦法提供任何優點，但若您使用ASF，就不會干擾您其他的遊戲，這對於VAC來說非常重要。

---

## 安全／隱私／VAC／封鎖／服務條款

---

### 我會因為使用這個被 VAC 封鎖嗎？

不會，這絕無可能，因為ASF（不同於Idle Master或SAM）不會以任何方式干擾Steam用戶端或其程序。 使用ASF完全不可能得到VAC封鎖，即使您在執行ASF的時候進入安全伺服器也不會⸺這是因為&#8203;**ASF完全不需要安裝Steam用戶端**&#8203;也能夠正常運作。 ASF是目前唯一能讓您保證不被VAC的掛卡程式。

---

### ASF 會像&#8203;**[此處](https://support.steampowered.com/kb_article.php?ref=2117-ilzv-2837)**&#8203;所提到的一樣，使我無法加入 VAC 防護伺服器嗎？

使用ASF不需要同時執行Steam用戶端，甚至根本不需要安裝它。 依據這個概念，它應該&#8203;**無法**&#8203;造成任何與VAC相關的問題，因為ASF能保證不干擾Steam用戶端及其所有程序⸺這是ASF能保證不被VAC的主要原因。

依據使用者們以及我自己所了解到的，目前的情況就是如此，因為沒有人報告在使用ASF時遇到上述連結中所述的任何問題。 我們亦無法使用ASF重現上述問題，但Idle Master卻能明顯地重現出來。

但請注意，Valve仍可能在未來把ASF加進黑名單中，但這樣做完全沒有意義，因為即使他們把ASF加入黑名單，您仍然可以在您的PC上遊玩受到VAC保護的遊戲，並同時在伺服器上使用ASF。所以我很確定他們也很清楚ASF不該成為被VAC的嫌疑者，也不會無緣無故將ASF列入黑名單中，而後帶給我們麻煩。 儘管如此，在最糟糕的情況下，您將如同上面所述一樣無法遊玩遊戲，因為不論Steam是否將ASF的二進制檔案列入黑名單，ASF的不受VAC的保證仍然有效（您仍然能在其他未安裝Steam用戶端的設備上執行ASF）。 但現在沒有必要去做這些事情，我們都希望維持現狀。

---

### 這個安全嗎？

如果您是要詢問ASF是否為一個安全的軟體，換句話說，想要知道它是否會對您的電腦造成損害、是否會竊取您的個人資料、是否會安裝病毒或任何惡意程式，這個問題的答案是⸺它很安全。 ASF不存有惡意程式、資料竊取、加密貨幣挖礦程式，或任何（及所有）其餘可能被使用者視為有惡意或不想要的可疑行為。 除此之外，我們有一個專門的&#8203;**[遠端通訊](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Remote-communication-zh-TW)**&#8203;章節，它涵蓋了我們的隱私權政策，及超出您能夠設定程式的範圍的ASF行為。

我們的原始碼是開源的，且轉發的二進制檔案總是會由&#8203;**[開放原始碼](https://zh.wikipedia.org/zh-tw/%E5%BC%80%E6%BA%90%E8%BD%AF%E4%BB%B6)**&#8203;的&#8203;**[自動化且可信的持續整合系統](https://zh.wikipedia.org/zh-tw/%E7%B5%84%E5%BB%BA%E8%87%AA%E5%8B%95%E5%8C%96)**&#8203;編譯，而不是由開發者。 每個組建都可以由我們的組建腳本再現，並會產生完全相同、&#8203;**[確定](https://zh.wikipedia.org/zh-tw/%E7%A1%AE%E5%AE%9A%E6%80%A7%E6%A8%A1%E5%9E%8B)**&#8203;的IL（二進制）程式碼。 若您出於各種原因無法信任我們的組建，您隨時能由原始碼及ASF使用的所有函式庫（例如SteamKit2）來編譯ASF，而這些函式庫也是開源的。

但最終，是否要信任應用程式的開發人員始終是個問題，因此您應該自行決定是否將ASF視為安全的，並透過上述的技術來驗證您的決策。 不要盲從⸺自行去檢驗，因為這是唯一能確定事實的方法。

---

### 我會因為這個被封鎖嗎？

要回答這個問題，我們首先需要詳閱&#8203;**[Steam服務條款](https://store.steampowered.com/subscriber_agreement)**&#8203;。 Steam並不禁止玩家開小號，且實際上，還&#8203;**[允許](https://support.steampowered.com/kb_article.php?ref=8625-WRAH-9030#share)**&#8203;您的多個帳號使用同一個行動驗證器。 但是，它不允許共用帳號，而我們也並未這樣做。

ASF真正需要考慮的重點如下：
> You may not use Cheats, automation software (bots), mods, hacks, or any other unauthorized third-party software, to modify or automate any Subscription Marketplace process.<br>（參考翻譯：您不得使用作弊軟體、自動化軟體（機器人）、模組、破解檔案，或其餘任何未經授權的第三方軟體，來修改或自動執行訂閱市集的任何交易過程。）

問題是，我們需要知道Subscription Marketplace（訂閱市集）確切代表什麼。 我們可以得知：

> An example of a Subscription Marketplace is the Steam Community Market<br>（參考翻譯：訂閱市集的其中一個範例是 Steam 社群市集）

如果訂閱市集指的就是Steam社群市集或Steam商店，那麼我們不會修改或自動化訂閱市集的交易過程。 然而……

> Valve may cancel your Account or any particular Subscription(s) at any time in the event that (a) Valve ceases providing such Subscriptions to similarly situated Subscribers generally, or (b) you breach any terms of this Agreement (including any Subscription Terms or Rules of Use).<br>（參考翻譯：Valve 隨時能夠在下列情形發生時，停用您的帳號或取消特定訂閱：(a) Valve 不再向此類訂閱者提供訂閱服務，或 (b) 您違反了本協議的任一條款（包含任意訂閱條款，及使用規章））

因此，與每款Steam軟體相同，ASF未經Valve授權。如果Valve突然決定要封鎖使用ASF的帳號，我無法像您保證不被終止使用合約。 考慮到目前已有超百萬個Steam帳號使用過ASF，這種情形幾乎不會發生，但不論機率有多低，仍不能排除發生的可能性。

特別是因為：
> In regard to all Subscriptions, Content and Services that are not authored by Valve, Valve does not screen such third-party content available on Steam or through other sources. Valve assumes no responsibility or liability for such third party content. Some third-party application software is capable of being used by businesses for business purposes - however, you may only acquire such software via Steam for private personal use.<br>（參考翻譯：對於不是由Valve創作的訂閱、內容及服務，Valve 不會篩選由 Steam 或其他來源提供的此類第三方內容。Valve 對此類第三方內容不承擔任何責任。某些第三方應用程式可供企業應用於商業行為⸺但是，您透過 Steam 獲得的這類軟體，僅供個人使用。）

但如&#8203;**[此處](https://help.steampowered.com/zh-tw/faqs/view/22C0-03D0-AE4B-04E8)**&#8203;所述，Valve早已知曉「Steam掛機程式」的存在，因此，假設您問我，我很確定他們如果認為這些行為不妥，早就會採取相應措施，而不是只說出這種行為會造成有關VAC的問題了。 這裡的關鍵字是&#8203;**Steam**&#8203;掛機程式，例如ASF，而不是&#8203;**遊戲**&#8203;掛機程式。

請注意，上述說明只是我們對&#8203;**[Steam服務條款](https://store.steampowered.com/subscriber_agreement)**&#8203;及其部分條文的解釋⸺ASF是依據Apache 2.0授權條款所授權，其中明確規定了：

> Unless required by applicable law or agreed to in writing, ASF is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.<br>（參考翻譯：除非適用法律要求或書面同意，否則 ASF 均按「原始形式」轉發，不附帶任何明示或暗示的擔保或條件。）

您使用本軟體的風險是由您自己所承擔。 您不太可能會因此受到封鎖，但萬一您還是被封鎖了，那麼您也只能怪您自己。

---

### 有人因為它被封鎖嗎？

**有的**&#8203;，到目前為止，我們發生過幾起導致某種Steam封鎖的事件。 但這幾起事件的根本原因是否為ASF，我們現在已經無從得知。

第一起案例是一名擁有超過1000個Bot的使用者遭受交易封鎖（包含他所有的Bot），原因很可能是一次性對所有的Bot大量執行了&#8203;`loot ASF`&#8203;，或是其他能在短時間內進行大量單向交易的操作。

> XXX 您好， 感謝您聯絡 Steam 支援團隊。 看起來這個帳號被用來管理一個機器人帳號網路。 使用機器人是違反 Steam 訂戶協議的。

請帶上您的腦子好好想想，不要只因為ASF能讓您這樣做，就做出這類瘋狂的事情。 對超過1000個Bot使用&#8203;`loot ASF`&#8203;很容易被視為一次&#8203;**[DDoS](https://zh.wikipedia.org/zh-tw/%E9%98%BB%E6%96%B7%E6%9C%8D%E5%8B%99%E6%94%BB%E6%93%8A)**&#8203;攻擊，對我來說，我並不驚訝有人因為這類事情而被封鎖。 請記住關於Steam服務的一些基本常識，只要不濫用，就&#8203;**基本不可能**&#8203;遇到問題。

第二起案例是一名擁有超過170個Bot的使用者，在Steam 2017年冬季特賣期間被封鎖。

> 您的帳號因違反 Steam 訂戶協議而被封鎖。 由交易及其他的因素來判斷，此帳號被用於非法收集 Steam 交換卡片，並與商業行為有關，或直接進行了商業行為。 該帳號已被永久封鎖，Steam 支援團隊無法在這個問題上為您提供進一步的協助。

此案例同樣難以分析，因為Steam支援團隊的回應含糊其辭，幾乎沒有提到任何細節。 但依據我個人的想法，該名使用者可能使用了Steam交換卡片來換取金錢（等級機器人？），或以其他方式來嘗試在Steam上進行變現。 或許您並不清楚，但這種行為同樣違反了&#8203;**[Steam服務條款](https://store.steampowered.com/subscriber_agreement)**&#8203;。

第三起案例是一名擁有超過120個Bot的使用者，因違反&#8203;**[Steam線上行為守則](https://store.steampowered.com/online_conduct?l=english)**&#8203;而被封鎖。

> XXX 您好， 感謝您聯絡 Steam 支援團隊。 此帳號及其他帳號被用於攻擊我們的網路架構，這違反了 Steam 線上行為守則。 該帳號已被永久封鎖，Steam 支援團隊無法在這個問題上為您提供進一步的協助。

此案例比較容易分析，因為這名使用者提供了額外的細節。 很明顯，該名使用者使用了&#8203;**過時已久的ASF版本**&#8203;，而該版本包含了一個使ASF向Steam伺服器傳送過多請求的錯誤。 這個錯誤最初並未存在，而是因為Steam的重大改動導致了這個問題，而後在稍後的ASF版本中被修正。 **&#8203;ASF只支援在GitHub上發布的&#8203;[最新穩定版本](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)**&#8203;。 軟體是由人寫的，而人會犯錯。 如果錯誤會影響到所有人，那麼它會被盡快修正，並做為版本更新發布給所有使用者。 顯然Valve不會因為我犯的錯，而突然封鎖掉超過百萬名的ASF使用者。 但如果您故意不使用最新版本的ASF，那您就會成為少數使用者群，您將會因為&#8203;**不受支援**&#8203;，而將自己&#8203;**暴露於類似的情形下**&#8203;，因為沒有人會注意到您過時的ASF版本，沒有人會修正它的錯誤，也沒有人能確保您不會因啟動它而遭受封鎖。 **請使用最新版本的軟體**&#8203;，不只是ASF，這也適用於其他所有應用程式。

最新的一起案例是發生於2021年6月左右，據此使用者所述：

> 我一直在使用您的程式，在過去 3 年間，為 28 個帳號合成擴充包，並在近 6 個月的時候增加到 128 個帳號。 在合成擴充包時，我最多有 15 個帳號同時在線，並在合成完後傳送至主要帳號上。 在上個月，我將同時在線的帳號數量提高到 20 個，經過一週後，我所有的帳號都被封鎖了。 這封信不是來向您興師問罪的，相反，我自始至終都知道這樣做的後果。 我希望您能藉此得知何種行為會導致帳號被永久封鎖。

很難說提高帳號同時在線的數量，是否為獲得封鎖的直接原因，至少我並不覺得是，相反地，我相信大量的帳號才是罪魁禍首，而提高同時在線的數量可能只是使這名使用者違反規定的情形被發現，因為他的Bot數量遠遠比我們的建議數量還多。

---

上述所有事件都有一個共通點⸺ASF只是一個工具，如何使用取決於&#8203;**您**&#8203;。 您不會只是因為使用ASF而被封鎖，但有可能會因您的&#8203;**使用方式**&#8203;而被封鎖。 ASF可以只做為一個單一帳號的掛卡協助工具，但也能用於管理成千上萬個機器人的掛卡網路。 在任何情形下，我都不會提供法律諮詢，您應該一開始就決定好自己要如何使用ASF。 我並沒有隱瞞任何對您有幫助的資訊，例如因ASF導致使用者被封鎖的事件等，因為我沒有理由去隱瞞它⸺您可以自行選擇如何處理這些資訊。 若您詢問我有沒有什麼建議⸺那麼您應該要先了解一些常識，避免持有超過我們建議數量的Bot、不要同時發送上百個交易，並始終使用最新版本的ASF，這樣，您&#8203;_應該_&#8203;就不會遇到任何問題。 出於&#8203;**某些原因**&#8203;，這些類似事件總會發生在無視我們建議的人身上，他們會自認為比我們更清楚能夠執行多少個Bot。 不論這算巧合還是有實際上的原因，這都是由您來決定的。 我不會提供任何法律諮詢，而只會向您提供我的想法，您可能會覺得有用，或想要完全忽視它們，並只依據上述事實來操作。

---

### ASF 會公開哪些隱私訊息？

您可以在&#8203;**[遠端通訊](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Remote-communication-zh-TW)**&#8203;章節中，有著更深入的了解。 若您關心自己的隱私，例如您想知道為什麼使用ASF的帳號需要加入我們的Steam群組，請前去閱讀它。 ASF不會收集任何敏感性資訊，也不會將訊息分享給第三方。

---

## 其他

---

### 我使用不被支援的作業系統，例如 32 位元 Windows，我還能使用最新版本的 ASF 嗎？

可以，實際上該版本並非不受支援，只是沒有官方的組建版本。 請前往&#8203;**[相容性](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-zh-TW)**&#8203;章節，來深入了解Generic版本。 ASF並沒有任何地方高相依於作業系統，您可以在任何有.NET執行環境的設備執行，其中就包含了32位元的Windows，即使我們並不提供針對&#8203;`win-x86`&#8203;的軟體套件。

---

### ASF 太棒了！ 我可以贊助嗎？

可以，我們非常高興能聽到您喜歡我們的專案！ 您可以在每個&#8203;**[版本](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)**&#8203;下方及&#8203;**[主頁面上](https://github.com/JustArchiNET/ArchiSteamFarm)**&#8203;找到各種贊助管道。 值得一提的是，除了一般的現金贊助，我們也接受Steam物品，只要您願意，也可以向我們贊助外觀造型、產品序號，或您的部分掛卡所得。 在此先感謝您的慷慨贊助！

---

### 我使用 Steam 家庭監護 PIN 碼來保護我的帳號，我是否需要在某處輸入它？

要的，您需要將它填入&#8203;`SteamParentalCode`&#8203; Bot設定屬性中。 這主要是因為ASF需要存取您Steam帳號內許多受到保護的部分，如果沒有PIN碼，ASF將無法正常運作。

---

### 我希望 ASF 預設不要掛任何遊戲，我只想使用 ASF 的附加功能。 這是可以做到的嗎？

可以，若您想要ASF在啟動的時候就暫停掛卡模組，您可以將&#8203;`Paused`&#8203; Bot設定屬性設定成&#8203;`true`&#8203;來達成。 您可以在執行期間使用&#8203;`resume`&#8203;來恢復掛卡。

如果您想完全停用掛卡模組，且確保它不會在沒有明確要求時執行，那麼我們建議您將&#8203;`FarmPriorityQueueOnly`&#8203;設定成&#8203;`true`&#8203;，而不是只將它暫停，這項操作會完全停用掛卡，直到您自己將遊戲加進優先掛卡佇列中。

暫停／停用掛卡模組後，您可以使用ASF的額外功能，例如&#8203;`GamesPlayedWhileIdle`&#8203;。

---

### ASF 能最小化到工作列中嗎？

ASF是個控制台應用程式，沒有可被最小化的圖形化視窗，而控制台視窗是由您的作業系統建立的。 但您可以透過第三方工具來達成，例如Windows的&#8203;**

RBTray</strong></strong>&#8203;，或Linux/macOS的&#8203;**[screen](https://linux.die.net/man/1/screen)**&#8203;。 這只是範例，也有許多相似功能的應用程式。</p> 



---



### 使用 ASF 是否能為我保有獲得擴充包的資格？

**可以**&#8203;。 ASF使用與官方用戶端相同的方法登入至Steam網路，因此它也有能力為使用ASF的帳號保有獲得擴充包的資格。 此外，保有這份資格甚至不需要登入至Steam社群，所以如果您想要，也能將&#8203;`OnlineStatus`&#8203;設定成&#8203;`Offline`&#8203;。



---



### 有任何方式與 ASF 通訊嗎？

有的，這有幾種不同的方法。 請前往&#8203;**[指令](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-TW)**&#8203;章節，來深入了解更多資訊。



---



### 我想要幫助 ASF 翻譯，我需要做什麼？

感謝您的協助！ 您可以在&#8203;**[在地化翻譯](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Localization-zh-TW)**&#8203;章節中，了解更多詳細資訊。



---



### 我在 ASF 中只有一個（主要）帳號，我仍然能透過 Steam 聊天來傳送指令嗎？

**可以**&#8203;，在&#8203;**[指令](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-TW#備註)**&#8203;章節中有詳細解釋。 您可以使用Steam群組聊天，但使用&#8203;**[ASF-ui](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-zh-TW#asf-ui)**&#8203;可能是個更簡單的方法。



---



### ASF 似乎已在執行，但我沒有獲得任何掉落卡片！

每款遊戲的掉卡速率有所不同，您可以閱讀&#8203;**[效能](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance-zh-TW)**&#8203;章節來了解。 這需要一定的時間，通常&#8203;**每款遊戲需要數個小時**&#8203;，您不應期望程式才剛執行幾分鐘，就能掉出卡片。 若您發現ASF主動在檢查卡片狀態，並在一款遊戲掛完後切換至另一款，那代表一切正常。 也有可能您將&#8203;`BotBehaviour`&#8203;設定了&#8203;`DismissInventoryNotifications`&#8203;，這會使您收不到物品庫有新物品的通知。 請前往&#8203;**[組態設定](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-TW)**&#8203;章節，來了解更多資訊。



---



### 如何完全終止我的帳號的 ASF 程序？

直接關閉ASF程序即可，例如在Windows上，點擊[X]即可關閉。 如果您只是想要關閉某個Bot，但要讓其他Bot繼續執行，請查閱&#8203;`Enabled`&#8203; &#8203;**[Bot設定屬性](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-TW#bot-設定檔)**&#8203;，或&#8203;`stop`&#8203;**[指令](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-TW)**&#8203;。 如果您只是想要關閉自動掛卡程序，但要讓ASF繼續掛著您的帳號，請使用&#8203;`Paused`&#8203; &#8203;**[Bot設定屬性](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-TW#bot-設定檔)**&#8203;及&#8203;`pause`&#8203;**[指令](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-TW)**&#8203;。



---



### 我可以在 ASF 上執行多少個 Bot？

ASF程式並沒有Bot實例數量的硬性上限，所以只要您的設備具有足夠的記憶體，就能夠執行無限多的Bot，但是，您仍然會受到Steam網路及其他Steam的限制。 目前，您可以在單一IP中以單一ASF實例執行最多100-200個Bot。 透過解決IP限制，您可以使用更多IP及更多ASF實例來執行更多的Bot。 請注意，若您使用過多的Bot，則應該要自行控制它們的數量，來確保它們能夠同時正常登入及運作。 ASF並未對大數量的Bot做出相應調整，並適用一般法則：&#8203;**您擁有的Bot越多，就越容易遇到問題**&#8203;。 並請注意，上述限制通常還會取決於許多內部因素，因此這只是一個參考值，而非準確的數量限制⸺您可能可以執行比上述更多／更少的Bot。

ASF團隊建議您執行（包含&#8203;**擁有**&#8203;）&#8203;**最多10個Bot**&#8203;，如果您擁有超過這個數量，那就需要自行承擔風險，且我們並不為此支援。 這項建議依據Valve的內部指南，以及我們自己的建議。 您有權決定是否要遵守這個規定，即使您的行為會導致Steam帳號被封鎖，ASF也不會阻止您，因為它只是一個工具。 因此，如果您忽視我們的建議，ASF將會對您顯示警告，但您仍可以自由使用並自行承擔風險，我們對此不會提供支援。



---



### 那我可以同時執行更多個 ASF 實例嗎？

您可以在一台設備上執行無限數量的ASF實例，但前提是每個實例都具有獨立的資料夾及設定檔，且已被某個實例使用的帳號，並未在另一個實例中使用。 但是，您該問問自己為什麼要這樣做。 ASF已最佳化至能夠一次處理上百個帳號了，將這些Bot以獨立的ASF實例執行，會降低效能、使用更多系統資源（例如CPU及記憶體），並可能導致不同ASF實例間的同步問題，因為ASF會被強制與其他實例共用限制。

因此，我&#8203;**強烈建議**&#8203;在每個IP／介面上最多只執行一個ASF實例。 若您擁有多個IP／介面，這就代表您能夠執行多個ASF實例，且每個實例都使用各自獨立的IP／介面，或唯一的&#8203;`WebProxy`&#8203;設定。 否則，執行多個ASF實例就沒有意義，因為在單一IP／介面執行多個實例並不會帶來好處。 ASF並沒有對此進行任何限制，但Steam不會只因您使用了多個ASF實例，就神奇地允許您執行更多的Bot。

當然，仍然有使用同一個網路介面，並合理使用多個ASF實例的範例，例如分別為您的朋友代管ASF服務，並為您的朋友們都使用獨立的ASF實例，以保證每個Bot甚至ASF程序本身都是隔離的，但是，您無法使用這種方式來繞過任何的Steam限制，這是完全不同的目的。



---



### 啟用序號時的狀態是什麼意思？

狀態代表序號嘗試兌換後的結果。 可能有很種多不同的狀態結果，而其中最常見的包括：

| 狀態                      | 描述                                                                                                                             |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| NoDetail                | 「OK」狀態，代表成功⸺產品序號已被成功兌換。                                                                                                        |
| Timeout                 | Steam網路未在規定時間內回應，我們無法得知產品序號是否被啟用（很可能被使用了，但您可以再試一次）。                                                                            |
| BadActivationCode       | 提供的產品序號無效（無法被Steam網路辨識成任何有效的序號）。                                                                                               |
| DuplicateActivationCode | 所提供的產品序號已被其他帳號兌換過，或已被開發人員／發行者撤銷。                                                                                               |
| AlreadyPurchased        | 您的帳號已經擁有此產品序號所關聯的&#8203;`packageID`&#8203;。 請注意，本狀態無法代表序號是否為&#8203;`DuplicateActivationCode`&#8203;⸺它只代表序號有效，但在本次的兌換嘗試中沒有被使用掉。 |
| RestrictedCountry       | 此產品序號擁有區域限制，且您的帳號並不在有效的兌換區域內。                                                                                                  |
| DoesNotOwnRequiredApp   | 您缺少了啟用此產品序號所需的其他應用程式⸺通常是您在兌換DLC時，缺少遊戲本體。                                                                                       |
| RateLimited             | 您在短時間內進行了過多的兌換嘗試，您的帳號已被暫時限制。 請於一小時後再試一次。                                                                                       |




---



### 你是否附屬於任何掛卡／掛機服務？

**不**&#8203;。 ASF並不屬於任何服務，任何類似的聲明都是假的。 您的Steam帳號是您的個人財產，您可以透過任何方式來使用您的帳號，但Valve在&#8203;**[官方服務條款](https://store.steampowered.com/subscriber_agreement)**&#8203;中明確說明了：



> You are responsible for the confidentiality of your login and password and for the security of your computer system. Valve is not responsible for the use of your password and Account or for all of the communication and activity on Steam that results from use of your login name and password by you, or by any person to whom you may have intentionally or by negligence disclosed your login and/or password in violation of this confidentiality provision.<br>（參考翻譯：您有責任保護您的帳號、密碼，與您電腦系統的安全。Valve 不對您帳號密碼的使用負責，也不對因您、或您有意或無意間違反此保密條款，向他人洩漏您的帳號密碼所導致的Steam上的所有通訊及行為負責。）

ASF是依據自由的Apache 2.0授權條款所授權，允許其他開發人員合法地將ASF整合進他們自己的專案或服務中。 但是，這些使用ASF的第三方專案無法被保證為安全的、經過審查的、適合的，或合乎&#8203;**[Steam服務條款](https://store.steampowered.com/subscriber_agreement)**&#8203;的。 如果您想知道我們的看法，&#8203;**我們強烈建議您不要與任何第三方服務共用任何您帳號的詳細資料**&#8203;。 若該服務&#8203;**是個經典的詐騙**&#8203;，您很可能會遇到各種問題，通常會是Steam帳號被盜，而ASF並不對任何第三方服務的安全聲明負責，因為ASF團隊從未授權或審查這些服務。 也就是說，&#8203;**若您忽視我們上面的建議，您必須在使用這些服務時自行承擔風險**&#8203;。

除此之外，官方的&#8203;**[Steam服務條款](https://store.steampowered.com/subscriber_agreement)**&#8203;中明確說明了：



> You may not reveal, share or otherwise allow others to use your password or Account except as otherwise specifically authorized by Valve.<br>（參考翻譯：除 Valve 另有授權，否則您不得透漏、共用，或以其他方式允許他人使用您的密碼或帳號。）

這是您的帳號，也是您的選擇。 不要說沒人警告過您。 ASF是個遵守上述所有規章的程式，因為您並未將您的帳號詳細資料共用給任何人，且您以個人用途使用本程式；但其他的「掛卡服務」都會需要您提供您的帳號憑證，因此它們將違反上述規章（實際上違反了數條）。 如同上述的&#8203;**[Steam服務條款](https://store.steampowered.com/subscriber_agreement)**&#8203;解釋相同，我們不提供任何法律諮詢，您應自行決定是否使用這些服務⸺而我們會說&#8203;**它們違反了&#8203;[Steam服務條款](https://store.steampowered.com/subscriber_agreement)**&#8203;，如果Valve發現，您的帳號可能遭到封鎖。 如上文所述，&#8203;**我們強烈建議不要使用這類服務**&#8203;。



---



## 執行問題



---



### 我有一款遊戲現在已經掛超過 10 個小時了，但我仍然沒有獲得任何卡片！

這個問題的原因可能與Steam的一個已知問題相關，當您同時擁有同一款遊戲的兩份授權，但其中一份具有掉卡限制時，就有可能會發生。 造成這個問題的原因，通常是因為您在Steam上先領取了限時免費遊戲，而後又為同一款遊戲啟用了（不具限制的）產品序號，例如經由付費的組合包啟用。 若發生了這種情形，Steam會在徽章頁面上顯示該遊戲仍可掉落卡片，但由於您的帳號擁有免費版授權，不論您玩了多久，卡片都無法掉落。 因為這是Steam的問題而非ASF的，我們無法在ASF中繞過這項問題，您必須自行解決。

有兩種方式能夠解決這項問題。 第一種，您將這款遊戲加入至ASF黑名單中，可使用&#8203;`fbadd`&#8203;**[指令](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-TW)**&#8203;或&#8203;`Blacklist`&#8203;**[設定屬性](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-TW)**&#8203;。 這將阻止ASF嘗試為此款遊戲掛卡，但這種方式治標不治本，並不能幫您解決無法從該遊戲中獲得卡片的問題。 第二種，您可以使用Steam客服中心的自助服務工具來移除免費版授權，並只保留能夠掉卡的完整版授權。 如要使用此方法，請造訪&#8203;**[授權與產品序號啟用](https://store.steampowered.com/account/licenses)**&#8203;頁面，查看受影響遊戲的免費版及付費版授權。 這通常很簡單⸺兩者會有相似的名稱，但免費版會包含「Limited Free Promotional Package」或類似「Promo」的名稱，且「取得方式」的欄位應會顯示為「贈品」。 但也可能稍加麻煩，例如免費版授權被包含在組合包中，或是擁有完全不同的名稱。 若您找到了上述的兩份授權⸺那麼問題的根源確實在此，您可以安全移除免費版授權，並仍保有遊戲。

若要從帳號中移除免費版授權，請造訪&#8203;**[Steam客服頁面](https://help.steampowered.com/wizard/HelpWithGame)**&#8203;，並在搜尋欄中輸入受影響的遊戲，該遊戲應該會出現在「產品」類別的下方，然後點擊此遊戲。 或者，您也可以直接前往&#8203;`https://help.steampowered.com/wizard/HelpWithGame?appid=<appID>`&#8203;連結，將其中的&#8203;`<appID>`&#8203;取代成受影響遊戲的AppID。 然後，點擊「我想從帳戶中移除這款遊戲」，選擇您在剛才所找到的免費版授權，就是通常在名稱中含有「Limited Free Promotional Package」（或類似文字）的那個。 在移除免費版授權後，ASF應該就能從受影響的遊戲中正常掉卡，您需要在移除授權後重新啟動掛卡操作，以確保Steam這次提供了正確的授權。



---



### 我知道有&#8203;`某款`&#8203;遊戲能夠掉落 Steam 交換卡片，但 ASF 並未偵測到！

這有兩個主要的原因。 第一個也是最明顯的原因，您所指的是，這款遊戲的&#8203;**Steam商店**&#8203;頁面中，說明了該款遊戲含有交換卡片。 這是&#8203;**錯誤的**&#8203;假設，因為它只能代表該遊戲&#8203;**支援**&#8203;交換卡片，但不代表該遊戲現在&#8203;**已有**&#8203;卡片功能。 您能閱讀這份&#8203;**[官方公告](https://steamcommunity.com/games/593110/announcements/detail/1954971077935370845)**&#8203;來深入了解更多相關資訊。

簡而言之，Steam商店中的交換卡片圖示並不能代表什麼，請去檢查您的&#8203;**[徽章頁面](https://steamcommunity.com/my/badges)**&#8203;，才能確認該遊戲是否啟用了卡片掉落⸺這也是ASF所採用的方式。 若您的遊戲沒有出現在可掉卡的清單中，那不論何種原因，這款遊戲都&#8203;**無法**&#8203;掛卡。

第二個原因並不那麼顯而易見，有時，您的徽章頁面確實顯示了該遊戲可供掉卡，但ASF並未馬上為它掛卡。 除非您是遇到其他錯誤，例如ASF無法檢查徽頁面（詳見下述），否則這只是個快取效應，就是Steam讓ASF取得了舊的徽章頁面。 等快取過期後，這個問題很快就會自行解決。 在我們這邊沒有任何方式能解決。

當然，上述都假設了您使用預設設定執行ASF，因為您也可以透過&#8203;`FarmPriorityQueueOnly`&#8203;、&#8203;`SkipRefundableGames`&#8203;等方式，將遊戲加入掛卡黑名單中。



---



### 為什麼透過 ASF 掛的遊玩時數並未增加？

遊玩時數增加了，但&#8203;**非實時更新**&#8203;。 Steam以固定的時間間隔來記錄您的遊玩時數，並排程更新，這無法保證您在退出遊戲時能立即更新，更不用說在遊玩時了。 遊玩時數非實時更新不代表沒有被記錄，通常它每隔30分鐘左右會更新一次。



---



### 紀錄中的 WARN（警告）與 ERROR（錯誤）的區別在哪？

ASF writes to its log a bunch of information on various logging levels. Our objective is to explain **precisely** what ASF is doing, including what Steam issues it has to deal with, or other problems to overcome. Most of the time not everything is relevant, this is why we have two major levels being used in ASF in terms of problems - a warning level, and error level.

General ASF rule is that warnings are **not** errors, therefore they should **not** be reported. A warning is an indicator to you that something potentially unwanted happen. Whether it was Steam not reacting, API throwing errors or your network connection being down - it's a warning, and it means we expected it to happen, so don't bother ASF development with it. Of course you're free to ask about them or get help by using our support, but you shouldn't assume that those are ASF errors worth reporting (unless we confirm otherwise).

Errors on the other hand indicate a situation that should not happen, therefore they're worth reporting as long as you made sure that it's not you who is causing them. If it's a common situation that we expect to happen, then it'll be converted to a warning instead. Otherwise, it's possibly a bug that should be corrected, not silently ignored, assuming it's not a result of your own technical issue. For example, putting invalid content in `ASF.json` file will throw an error, as ASF won't be able to parse it, but it was you who put it there, so you should not report that error to us (unless you confirmed that ASF is wrong and your structure is in fact absolutely correct).

In one TL;DR sentence - report errors, don't report warnings. You can still ask about warnings and receive help in our support sections.



---



### 無法開啟 ASF，程式視窗會立刻關閉！

In normal conditions, any ASF crash or exit will generate a `log.txt` in the program's directory for you to view, which can be used for finding the cause of that. In addition to that, a few last log files are also archived in `logs` directory, since the main `log.txt` file is overwritten with each ASF run.

However, if even .NET runtime isn't able to boot on your machine, then `log.txt` will not be generated. If that happens to you then you most likely forgot to install .NET prerequisites, as stated in **[setting up](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up#os-specific-setup)** guide. Other common problems include trying to launch wrong ASF variant for your OS, or in other way missing native .NET runtime dependencies. If the console window closes too soon for you to read the message, then open independent console and launch ASF binary from there. For example on Windows, open ASF directory, hold `Shift`, right click inside the folder and choose "*open command window here*" (or *powershell*), then type into the console `.\ArchiSteamFarm.exe` and confirm with enter. This way you'll get precise message why ASF is not starting properly.



---



### 在我遊玩的時候，ASF 會將我的 Steam 用戶端踢下線！ ／&#8203;*此帳號已於另一台電腦中登入*&#8203;

This shows up as a message in Steam overlay that the account is being used somewhere else while you're playing. This issue can have two different reasons.

One reason is caused by broken packages (games) that specifically don't hold a playing lock properly, yet expect that lock to be possesed by the client. An example of such package would be Skyrim SE. Your Steam client launches the game properly, but that game doesn't register itself as being used. Because of that, ASF sees that it's free to resume the process, which it does, and that kicks you out of Steam network, as Steam suddenly detects that the account is being used in another place.

Second reason could come up if you're playing on your PC while ASF is waiting (especially on another machine) and you lose your network connection. In this case, Steam network marks you as offline and releases playing lock (like above), which triggers ASF (e.g. on another machine) into resuming farming. When your PC comes back online, Steam can't acquire playing lock anymore (that is now held by ASF, also similar to above) and shows the same message.

Both causes on the ASF side are actually very hard to workaround, as ASF simply resumes farming once Steam network informs it that account is free to be used again. This is what is happening normally when you close the game, but with broken packages this can happen immediately, even if your game is still running. ASF has no way to know whether you got disconnected, stopped playing a game or that you're still playing a game that doesn't hold playing lock appropriately.

The only proper solution to this problem is manually pausing your bot with `pause` before you start playing, and resuming it with `resume` once you're done. Alternatively you can just ignore the problem and act the same as if you played with offline Steam client.



---



### `已與 Steam 中斷連線！`&#8203;：我無法與 Steam 伺服器建立連線。

ASF can only **try** to establish connection with Steam servers, and it can fail due to many reasons, including lack of internet connection, Steam being down, your firewall blocking connection, third-party tools, incorrectly configured routes or temporary failures. You can enable `Debug` mode to check out more verbose log stating exact failure reasons, although usually it's simply caused by your own actions, such as using "CS:GO MM Server Picker" that blacklists a lot of Steam IPs, making it very hard for you to actually reach Steam network.

ASF will do its best to establish connection, which includes not only asking about updated list of servers but also trying another IP when last one fails, so if it's truly a temporary problem with some specific server or route, ASF will connect sooner or later. However, if you're behind firewall or in some other way unable to reach Steam servers, then obviously you need to fix it yourself, with potential help of `Debug` mode.

It's also possible that your machine is not able to establish connection with Steam servers using default protocol in ASF. You can alter protocols that ASF is permitted to use by modifying `SteamProtocols` global configuration property. For example, if you have problems reaching Steam with `UDP` protocol (e.g. due to firewalls), perhaps you'll have more luck with `TCP` or `WebSocket`.

In a very unlikely situation of having incorrect servers being cached, for example because of moving ASF `config` folder from one machine to another machine located in entirely different country, deleting `ASF.db` in order to refresh Steam servers on the next launch may help. Very often it's not needed and doesn't have to be done, as that list is automatically refreshed on first launch, as well as when the connection is established - we're just mentioning it as a way to purge anything related to list of Steam servers cached by ASF.



---



### `無法取得徽章頁資訊，我們將稍後再試 ！`

Usually it means that you're using Steam parental PIN to access your account, yet you forgot to put it in ASF config. You must put valid PIN in `SteamParentalCode` bot config property, otherwise ASF will not be able to access most of web content, therefore will not be able to work properly. Head over to **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** in order to learn more about `SteamParentalCode`.

Other reasons include temporary Steam problem, network issue or likewise. If issue won't solve itself after several hours and you're sure that you configured ASF appropriately, feel free to let us know about that.



---



### ASF 發生&#8203;`在嘗試 5 次請求後失敗`&#8203;的錯誤！

Usually it means that you're using Steam parental PIN to access your account, yet you forgot to put it in ASF config. You must put valid PIN in `SteamParentalCode` bot config property, otherwise ASF will not be able to access most of web content, therefore will not be able to work properly. Head over to **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** in order to learn more about `SteamParentalCode`.

If parental PIN is not the reason, then this is a most common error, and you should get used to that, it simply means that ASF sent a request to Steam Network, and didn't get a valid response, 5 times in a row. Usually it means that Steam is either down or is having some difficulties or maintenance - ASF is aware of such issues and you should not worry about them, unless they're happening constantly for longer than several hours, and other users do not have such problems.

How to check if Steam is being down? **[Steam Status](https://steamstat.us)** is an excellent source of checking if Steam **should be** up, if you notice errors, especially related to Community or Web API, then Steam is having difficulties. You may want to leave ASF alone and let it do its job after a short while of downtime, or quit it and wait yourself.

That's however not always the case, as in some situations Steam issues may not be detected by Steam Status, for example such case happened when Valve broke HTTPS support for Steam Community 7th June 2016 - accessing **[SteamCommunity](https://steamcommunity.com)** through HTTPS was throwing an error. Therefore, do not blindly trust Steam Status either, it's best to check yourself if everything works as supposed to.

In addition to that, Steam includes various rate-limiting measures which will temporarily ban your IP if you make excessive number of requests at once. ASF is aware of that and offers you several different limiters in the config, which you should make use of. Default settings were tweaked based on **sane** amount of bots, if you're using so huge amount that even Steam is telling you to go away, then you either tweak them until it no longer tells you to, or you do as you're told. I assume second way is not an option to you, so go read on that topic and pay special attention to `WebLimiterDelay` which is a general limiter that applies to all web requests.

There is no "golden rule" that works for everybody, because blocks are heavily influenced by third-party factors, that's why you have to experiment yourself and find a value that works for you. You can also ignore what I say and use something like `10000` which is guaranteed to work correctly, but then don't complain how your ASF reacts to everything in 10 seconds and how badge parsing takes 5 minutes. In addition to that, it's entirely possible that no limiter will do anything because you have so huge amount of bots that you're hitting **[hard limit](#how-many-bots-can-i-run-with-asf)** that was mentioned above. Yes, it's entirely possible that you'll be able to log in without issues into Steam network (client), but Steam web (website) will refuse to listen to you if you have 100 sessions established at once. ASF requires both Steam network and Steam web to be cooperative, it takes just one down to make you issues you won't recover from.

If nothing helps and you have no clue what is broken, you can always enable `Debug` mode and see yourself in ASF log why exactly requests are failing. 範例：



```text
InternalRequest() HEAD https://steamcommunity.com/my/edit/settings
InternalRequest() Forbidden <- HEAD https://steamcommunity.com/my/edit/settings
```


See that `Forbidden` code? This means that you got temporarily banned for excessive amount of requests, because you didn't tweak `WebLimiterDelay` properly yet (assuming you get the same error code for all other requests as well). There could be other reasons listed there, such as `InternalServerError`, `ServiceUnavailable` and timeouts that indicate Steam maintenance/issues. You can always try to visit the link mentioned by ASF yourself and check if it works - if it doesn't, then you know why ASF can't access that either. If it does, and the same error doesn't go away after a day or two, it may be worth investigating and reporting.

Before doing that you should **make sure that the error is worth reporting in the first place**. If it's mentioned in this FAQ, such as trading-related issue, then that's out. If it's temporary issue that happened once or twice, especially when your network was unstable or Steam was down - that's out. However, if you were able to reproduce your issue several times in a row, across 2 days, restarted ASF as well as your machine in the process and made sure that there is no FAQ entry here to help resolve it, then this may be worth asking about.



---



### ASF 似乎卡住了，如果我不按下任意鍵，控制台就不會輸出任何東西！

You're most likely using Windows and your console has QuickEdit mode enabled. Refer to **[this](https://stackoverflow.com/questions/30418886/how-and-why-does-quickedit-mode-in-command-prompt-freeze-applications)** question on StackOverflow for technical explanation. You should disable QuickEdit mode by right clicking your ASF console window, opening properties, and unchecking appropriate checkbox.



---



### ASF 無法接受或提出任何交易請求！

Obvious thing first - new accounts start as limited. Until you unlock account by loading its wallet or spending $5 in the store, ASF can't accept neither send trades using this account. In this case, ASF will state that inventory seems empty, because every card that is in it is non-tradable. It also won't be possible to receive any trade, as that part requires ASF to be able to fetch API key, and API key functionality is disabled for limited accounts. In short - trading is off for all limited accounts, no exceptions.

Next, if you do not use **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)**, it's possible that ASF in fact accepted/sent trade, but you need to confirm it via your e-mail. Likewise, if you use classic 2FA, you need to confirm the trade via your authenticator. Confirmations are **mandatory** now, so if you don't want to accept them by yourself, consider importing your authenticator into ASF 2FA.

Also notice that you can trade only with your friends, and people with known trade link. If you're trying to initiate *Bot -> Master* trade, such as `loot`, then you need to either have your bot on your friendlist, or your `SteamTradeToken` declared in Bot's config. Make sure that the token is valid - otherwise, you won't be able to send a trade.

Lastly, remember that new devices have 7-days trade lock, so if you've just added your account to ASF, wait at least 7 days - everything should work after that period. That limitation includes **both** accepting **and** sending trades. It does not always trigger, and there are people who can send and accept trades instantly. Majority of the people are affected though, and the lock **will** happen, even if you can send and accept trades through your steam client on the same machine. Just wait patiently, there's nothing you can do to make it faster. Likewise, you may get similar lock for removing/changing various Steam security-related settings, such as 2FA, SteamGuard, password, e-mail and likewise. In general, check if you can send a trade from that account yourself, if yes, very likely it's classic 7-days lock from new device.

And finally, keep in mind that one account can have only 5 pending trades to another one, so ASF will fail to send trades if you have 5 (or more) pending ones from that one bot to accept already. This is rarely a problem, but it's also worth mentioning, especially if you set ASF to auto-send trades, yet you're not using ASF 2FA and forgot to actually confirm them.

If nothing helped, you can always enable `Debug` mode and check yourself why requests are failing. Please note that Steam talks nonsense most of the time, and provided reason may not make any logical sense, or can be even entirely incorrect - if you decide to interpret that reason, make sure you have decent knowledge about Steam and its quirks. It's also quite common to see that issue with no logical reason, and the only suggested solution in this case is to re-add account to ASF (and wait 7 days again). Sometimes this issue also fixes itself *magically*, the same way it breaks. However, usually it's just either 7-days trade lock, temporary steam problem, or both. It's best to give it a few days before manually checking what is wrong, unless you have some urge to debug the real cause (and usually you'll be forced to wait anyway, because error message won't make any sense, neither help you in the slightest).

In any case, ASF can only **try** to send a proper request to Steam in order to accept/send trade. Whether Steam accepts that request, or not, is out of the scope of ASF, and ASF will not magically make it work. There's no bug related to that feature, and there is also nothing to improve, because logic is happening outside of ASF. Therefore, do not ask for fixing stuff that is not broken, and also do not ask why ASF can't accept or send trades - **I don't know, and ASF doesn't know either**. Either deal with it, or fix yourself, if you know better.



---



### 為什麼我每次登入時，都需要輸入雙重驗證／Steam Guard 代碼？ ／&#8203;*移除過期的登入金鑰*&#8203;

ASF uses login keys (if you kept `UseLoginKeys` enabled) for keeping credentials valid, the same mechanism that Steam uses - 2FA/SteamGuard token is required only once. However, due to Steam network issues and quirks, it's entirely possible that login key is not saved in the network, we've already seen such issues not only with ASF, but with regular steam client as well (a need to input login + password on each run, regardless of "remember me" option).

You could remove `BotName.db` and `BotName.bin` (if available) of affected account and try to link ASF to your account once again, but that likely won't do anything. Some users have reported that **[deauthorizing all devices](https://store.steampowered.com/twofactor/manage)** on Steam side should help, changing password will do the same. However, those are only workarounds that are not even guaranteed to work, the real ASF-based solution is to import your authenticator as **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** - this way ASF can generate tokens automatically when they're needed, and you don't have to input them manually. Usually the issue magically solves itself after some time, so you can simply wait for that to happen. Of course you can also ask Valve for solution, because I can't force Steam network to accept our login keys.

As a side note, you can also turn off login keys with `UseLoginKeys` config property set to `false`, but this will not solve the problem, only skip the initial login key failure. ASF is already aware of the issue explained here and will try its best to not use login keys if it can guarantee itself all login credentials, so there is no need to tweak `UseLoginKeys` manually if you can provide all login details together with using ASF 2FA.



---



### 我遇到了錯誤：&#8203;*無法登入至 Steam：&#8203;`InvalidPassword`&#8203;或&#8203;`RateLimitExceeded`*

This error can mean a lot of things, some of them include:

- Invalid Login/Password combination (obviously)
- Expired login key used by ASF for logging in
- Too many failed login attempts in short period of time (anti-bruteforce)
- Too many login attempts in short period of time (rate-limiting)
- Requirement of captcha to log in (very likely to be caused by two reasons above)
- Any other reason Steam Network may have preventing you from logging in.

In case of anti-bruteforce and rate-limiting, problem will disappear after some time, so just wait and don't attempt to log in in the meantime. If you hit that issue frequently, perhaps it's wise to increase `LoginLimiterDelay` config property of ASF. Excessive program restarts and other intentional/non-intentional login requests definitely won't help with that issue, so try to avoid it if possible.

In case of expired login key - ASF will remove old one and ask for new one on next login (which will require from you putting 2FA token if your account is 2FA-protected. If your account is using ASF 2FA, token will be generated and used automatically). This can naturally happen over time, but if you get this issue on each login, it's possible that Steam for some reason decided to ignore our login key save requests, as mentioned in the issue **[above](#why-do-i-have-to-put-2fasteamguard-code-on-each-login--removed-expired-login-key)**. You can of course disable `UseLoginKeys` entirely, but that won't solve the issue, only avoid a need of removing expired login keys each time. The real solution, as per the issue above, is to use ASF 2FA.

And lastly, if you used wrong login + password combination, obviously you need to correct this, or disable bot that is attempting to connect using those credentials. ASF can't guess on its own whether `InvalidPassword` means invalid credentials, or any of the reasons listed above, therefore it'll keep trying until it succeeds.

Keep in mind that ASF has its own built-in system to react accordingly to steam quirks, eventually it will connect and resume its job, therefore it's not required to do anything if the issue is temporary. Restarting ASF in order to magically fix problems will only make things worse (as new ASF won't know previous ASF state of not being able to log in, and try to connect instead of waiting), so avoid doing that unless you know what you're doing.

Finally, as with every Steam request - ASF can only **try** to log in, using your provided credentials. Whether that request will succeed or not is out of the scope and logic of ASF - there is no bug, and nothing can be fixed neither improved in this regard.



---



### `System.IO.IOException: Input/output error`

If this error happened during ASF input (e.g. you can see `Console.ReadLine()` in the stacktrace) then it's caused by your environment which prohibits ASF from reading standard input of your console. That can occur due to a lot of reasons, but the most common one is you running ASF in the wrong environment (e.g. in `nohup` or `&` background instead of `screen` on Linux). If ASF can't access its standard input, then you'll see this error logged and ASF's inability to use your details during runtime.

If you **expect** this to happen, so you **intend** to run ASF in input-less environment, then you should explicitly tell ASF that it's the case, by setting **[`Headless`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#headless)** mode appropriately. This will tell ASF to never ask for user input under any circumstance, allowing you to run ASF in input-less environments safely.



---



### `System.Net.Http.WinHttpException: A security error occurred`

This error happens when ASF can't establish secure connection with given server, almost exclusively because of SSL certificate mistrust.

In almost all cases this error is caused by **wrong date/time on your machine**. Every SSL certificate has issued date and expiry date. If your date is invalid and out of those two bounds then the certificate can't be trusted due to a potential **[MITM](https://en.wikipedia.org/wiki/Man-in-the-middle_attack)** attack and ASF refuses to make a connection.

Obvious solution is to set the date on your machine appropriately. It's highly recommended to use automatic date synchronization, such as native synchronization available on Windows, or `ntpd` on Linux.

If you made sure that the date on your machine is appropriate and the error doesn't want to go away, SSL certificates that your system trusts could be out-of-date or invalid. In this case you should ensure that your machine can establish secure connections, for example by checking if you can access `https://github.com` with any browser of your choice, or CLI tool such as `curl`. If you confirmed that this works properly, feel free to post issue on our Steam group.



---



### `System.Threading.Tasks.TaskCanceledException: A task was canceled`

This warning means that Steam did not answer to ASF request in given time. Usually it's caused by Steam networking hiccups and does not affect ASF in any way. In other cases it's the same as request failing after 5 tries. Reporting this issue makes no sense most of the time, as we can't force Steam to respond to our requests.



---



### `The type initializer for 'System.Security.Cryptography.CngKeyLite' threw an exception`

This problem is almost exclusively caused by disabled/stopped `CNG Key Isolation` Windows service, which provides core cryptography functionality for ASF, without which the program isn't able to run. You can fix this issue by launching `services.msc` and ensuring that `CNG Key Isolation` Windows service doesn't have disabled startup and is currently running.



---



### ASF 被我的防毒軟體偵測成惡意程式！ 這是怎麼回事？

**Ensure that you downloaded ASF from trusted source**. The only official and trusted source is **[ASF releases](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** page on GitHub (and this is also the source for ASF auto-updates) - **any other source is untrusted by definition and can contain malware added by other people** - you should not trust any other download location by definition, and ensure that your ASF always comes from us.

If you confirmed that ASF is downloaded from trusted source, then very likely it's simply a false positive. This **happened in the past**, **is happening right now**, and **will happen in the future**. If you're worried about actual safety when using ASF, then I suggest scanning ASF with many different AVs for actual detection ratio, for example through **[VirusTotal](https://www.virustotal.com)** (or any other web service of your choice like this).

If the AV that you're using falsely detects ASF as a malware, then **it's a good idea to send this file sample back to developers of your AV, so they can analyze it and improve their detection engine**, as clearly it's not working as good as you think it does. There is no issue in ASF code, and there is also nothing to fix for us, since we're not distributing malware in the first place, therefore it doesn't make any sense to report those false-positives to us. We highly recommend to send ASF sample for further analysis like stated above, but if you don't want to bother with it, then you can always add ASF to some kind of AV exceptions, disable your AV or simply use another one. Sadly, we're used to AVs being stupid, as every once in a while some AV detects ASF as a virus, which usually lasts very short and is being patched up quickly by the devs, but like we pointed out above - **it happened**, **happens** and **will happen** all the time. ASF doesn't include any malicious code, you can review ASF code and even compile from source yourself. We're not hackers to obfuscate ASF code in order to hide from AV heuristics and false positives, so do not expect from us to fix what is not broken - there is no "virus" for us to fix.