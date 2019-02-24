# 棄用

從 ASF V3.1.2.2 開始，我們將遵循一致的棄用策略，以使開發和使用更加一致。

* * *

## 什麼是棄用？

棄用是重大更改（增刪）的過程，這些更改使以前使用的一些選項、參數、功能或使用方式過時。 棄用通常意味著給定的內容被簡單地重寫為另一個（類似）表單, 您應該及時確保您對其進行適當的切換。 在這種情況下，它只是將給定的功能移動到更合適的位置。

ASF 版本迭代迅速，總是追求卓越。 遺憾的是，這意味著我們可能會更改或將一些現有功能移動到程式的另一個部分，以便它從新功能、兼容性或穩定性中受益。 正因如此，我們不需要堅持我們多年前做出的過時或錯誤的發展決定。 我們一直在努力提供合理的替換方案，以兼容以前可用的功能，這就是為什麼棄用大多是無害的，僅需要對以前的邏輯進行小的修復。

* * *

## 棄用階段

ASF 的棄用分為兩個階段，使過渡更容易並減少麻煩。

### 第1階段

一旦給定的功能被棄用，就會進入第1階段，並立即對此功能提供另一個解決方案（如果沒有重新引入的計畫，則不提供）。

在此階段，ASF 將在不推薦使用的函數被調用時列印恰當的警告。 只要有可能，ASF 就會嘗試模仿之前的行為，並繼續與之相容。 至少在下一個穩定版本發佈之前，ASF 將繼續處於第1階段。 在這個時刻，希望您在不破壞兼容性的情況下，可以在所有的工具和模式中進行適當的切換，以滿足新的行為。 當您不再看到棄用警告，這表示您進行了所有適當的更改。

### 第2階段

第2階段安排在上面解釋的第1階段發生後，並在穩定的版本中發佈。 此階段將完全刪除之前不推薦使用的功能，這意味著 ASF 甚至不會承認您正在嘗試使用不推薦使用的功能，更不用說考慮它了，因為它根本不存在於當前代碼中。 ASF 將不再列印任何警告，因為它不再識別您嘗試執行的操作。

* * *

## 概要

您有至少**一個月** 來切換並適應，這對於一個普通的 ASF 用戶來說應足夠了。 在這段時間之後，ASF 不再保證舊設置將產生任何效果（第2階段），在您察覺不到的情況下使某些功能完全停止運行。 如果您在一個多月離線後啟動 ASF，建議您**[從頭來過](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up)**以啟動 ASF，或者再次閱讀您錯過的所有更改，並手動調整您的使用方式以適應當前的更改。

在大多數情況下，無視棄用警告並不會使 ASF 的常規功能不可用，僅會退回到預設行為（這可能與您的個人偏好相符合，也可能不符合）。

* * *

## 範例

We moved pre-V3.1.2.2 `--server` **[command-line argument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments)** into `IPC` **[global configuration property](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#global-config)**.

### 第1階段

Stage 1 happened in version V3.1.2.2 where we added appropriate warning to usage of `--server`. Now-obsolete `--server` argument was automatically mapped into `IPC: true` global config property, effectively acting exactly the same as old `--server` switch for time being. This allowed everybody to do appropriate switch before ASF stops accepting old argument.

### 第2階段

Stage 2 happened in version V3.1.3.0, right after V3.1.2.9 stable with stage 1 explained above. Stage 2 caused ASF to stop recognizing the `--server` argument at all, treating it like every other invalid argument being passed, which no longer has any effect on the program. For people that still didn't change their usage of `--server` into `IPC: true`, it caused IPC to stop functioning altogether, as ASF no longer did appropriate mapping.