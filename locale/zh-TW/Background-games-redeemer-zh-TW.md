# 背景啟動序號

背景啟動序號是 ASF 內建的特殊功能，可以讓你輸入一組 Steam 序號並在背景啟用，然後告訴你遊戲的名稱。 如果你想批次啟用多組序號，請確保 `RateLimited` **[status](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ#what-is-the-meaning-of-status-when-redeeming-a-key)** 才能完成整個過程。

背景啟用序號是單一 bot 執行的範圍，這表示它不使用 `RedeemingPreferences`。 如果需要，此功能可以與 `redeem` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** 一起使用。

* * *

## 匯入

導入可以通過兩種方式進行，使用檔案或 IPC。

### 檔案

在 ASF 的 `config` 資料夾中建立一個名為 `BotName.keys` 的檔案，`BotName` 為你的 bot名稱。 該檔案必須具有固定的格式，由遊戲名稱和序號組成，由 Tab 鍵分隔並以換行作為結束。 若有多個 Tab 已被使用，則第一項應考慮遊戲的名稱，最後一項是序號，中間的所有內容都被忽略。 範例：

    POSTAL 2    ABCDE-EFGHJ-IJKLM
    Domino Craft VR 12345-67890-ZXCVB
    A Week of Circus Terror POIUY-KJHGD-QWERT
    Terraria    ThisIsIgnored   ThisIsIgnoredToo    ZXCVB-ASDFG-QWERT
    

ASF 將在啟動時或稍後匯入指定的文件。 成功讀取檔案並跳過錯誤的項目後，`BotName.keys` 將自動從 `config` 刪除。

### IPC

除了使用 keys 檔案，ASF 還提供 IPC `GamesToRedeemInBackground` **[API endpoint](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#post-apigamestoredeeminbackgroundbotname)**，包括 IPC 圖形控制台。 使用 IPC 的方法可以更加實用，你可以自訂參數，例如使用分隔符而不是表格鍵。

* * *

## 佇列

當遊戲成功導入時，會被添加到佇列中。 ASF 會自動在背景處理佇列，而 bot 必須與 Steam 保持連線。 A key that was attempted to be redeemed and did not result in `RateLimited` is removed from the queue, with its status properly written to a file in `config` directory - either `BotName.keys.used` if the key was used in the process (e.g. `NoDetail`, `BadActivationCode`, `DuplicateActivationCode`), or `BotName.keys.unused` otherwise. ASF intentionally uses your provided game's name since key is not guaranteed to have a meaningful name returned by Steam network - this way you can tag your keys using even custom names if needed/wanted.

If during the process our account hits `RateLimited` status, the queue is temporarily suspended for a full hour in order to wait for cooldown to disappear. Afterwards, the process continues where it left, until the entire queue is empty.

* * *

## 範例

假設你的列表有一個 100 組序號。 首先必須在 ASF `config` 目錄中建立一個 `BotName.keys.new` 檔案。 我們添加了 `.new`副檔名，以便讓 ASF 知道不應該馬上處理此文件 (因為它是剛建立好的，還沒準備好導入)。

現在可以打開剛建立好的的檔案並在裡面貼上你那 100 組序號列表，如果需要的話就修改格式。 After fixes our `BotName.keys.new` file will have exactly 100 (or 101, with last newline) lines, each line having a structure of `GameName\tcd-key\n`, where `\t` is tab character and `\n` is newline.

You're now ready to rename this file from `BotName.keys.new` to `BotName.keys` in order to let ASF know that it's ready to be picked up. The moment you do this, ASF will automatically import the file (without a need of restart) and delete it afterwards, confirming that all our games were parsed and added to the queue.

Instead of using `BotName.keys` file, you could also use IPC API endpoint, or even combining both if you want to.

After some time, `BotName.keys.used` and `BotName.keys.unused` files might get generated. Those files contain results of our redeeming process. For example, you could rename `BotName.keys.unused` into `BotName2.keys` file and therefore submit our unused keys for some other bot, since previous bot didn't make use of those keys himself. Or you could simply copy-paste unused keys to some other file and keep it for later, your call. Keep in mind that as ASF goes through the queue, new entries will be added to our output `used` and `unused` files, therefore it's recommended to wait for the queue to be fully emptied before making use of them. If you absolutely must access those files before queue is fully emptied, you should firstly **move** output file you want to access to some other directory, **then** parse it. This is because ASF can append some new results while you're doing your thing, and that could potentially lead to loss of some keys if you read a file having e.g. 3 keys inside, then delete it, totally missing the fact that ASF added 4 other keys to your removed file in the meantime. If you want to access those files, ensure to move them away from ASF `config` directory before reading them, for example by rename.

It's also possible to add extra games to import while having some games already in our queue, by repeating all above steps. ASF will properly add our extra entries to already-ongoing queue and deal with it eventually.

* * *

## Remarks

Background keys redeemer uses `OrderedDictionary` under the hood, which means that your cd-keys will have preserved order as they were specified in the file (or IPC API call). This means that you can (and should) provide a list where given cd-key can only have direct dependencies on cd-keys listed above, but not below. For example, this means that if you have DLC `D` that requires game `G` to be activated firstly, then cd-key for game `G` should **always** be included before cd-key for DLC `D`. Likewise, if DLC `D` would have dependencies on `A`, `B` and `C`, then all 3 should be included before (in any order, unless they have dependencies on their own).

Not following the scheme above will result in your DLC not being activated with `DoesNotOwnRequiredApp`, even if your account would be eligible for activating it after going through its entire queue. If you want to avoid that then you must make sure that your DLC is always included after the base game in your queue.