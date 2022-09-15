# Steam 親友同享

ASF 自 2.1.5.5+ 版本開始，支援 Steam 親友同享。 想要了解 ASF 是如何支援 Steam 親友同享的，您應先閱讀 Steam 商店中提供的**[親友同享](https://store.steampowered.com/promotion/familysharing)**介紹。

---

## ASF

ASF 對 Steam 親友同享功能的支援是透明的，這代表它不會引入任何新的 bot／程序設定屬性：它是個額外的內置功能，開箱即用。

ASF 含有合適的邏輯，在偵測到收藏庫被親友同享的使用者鎖定時，不會因為啟動遊戲，而將他們「踢出」遊戲連線。 ASF 的行為與擁有鎖的主帳號完全相同，因此，若您的 Steam 用戶端或您的親友同享使用者持有該鎖，ASF 將不會嘗試進行掛卡，而是等待帳號解除鎖定。

除了上述內容以外，在登入之後，ASF 還會存取您的**

遊戲分享設定</0 >，它將從中提取最多 5 個允許使用您收藏庫使用者的 `steamID`。 這些使用者將被授予**[指令](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-TW)**中的 `FamilySharing` 權限，尤其是允許他們對與他們共用遊戲的 Bot 帳號使用 `pause~` 指令，暫停 Bot 的自動掛卡模組，使他們能夠啟動親友同享遊戲。</p> 

綜合上述功能，可以讓您的朋友執行 `pause~` 來暫停您的掛卡程序，開啟遊戲並玩到天昏地暗。在他們退出遊戲後，ASF 將自動恢復掛卡程序。 當然，如果 ASF 當下沒有在掛卡，則不需要傳送 `pause~`，因為您的朋友可以立即開始遊戲，且上述的邏輯會確保他們不會被踢出連線。



---



## 限制

Steam 網路經常廣播不實的狀態更新來誤導 ASF，這可能使 ASF 認為能夠恢復掛卡，並導致您的朋友被踢出遊戲連線。 這與我們已在**[這項常見問題](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ#asf-is-kicking-my-steam-client-session-while-im-playing--this-account-is-logged-on-another-pc)**中解釋的是相同的問題。 我們建議的解決方案也完全相同，主要是讓您的朋友獲得 `Operator` （或以上的）權限，並告訴他們使用 `pause` 與 `resume` 指令。