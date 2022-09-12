# 新手上路

如果您是第一次來到這裡，歡迎！ 我們很高興又看到一位對我們的專案感興趣的旅客，但請記住：能力越強，責任越大──只要您**足夠認真學習如何使用它**，ASF 就能完成相當多的 Steam 相關事務。 學習的過程將充滿艱辛與坎坷，我們希望您閱讀與之相關的 Wiki，並詳細了解這一切是如何運作的。

如果您還在這裡，那就代表您堅持看完了上面的文字，做得不錯。 除非您跳過了它，那樣的話，您很快就會經歷一段**[不好的時光](https://www.youtube.com/watch?v=WJgt6m6njVw)**…… 總的來說，ASF 是一個控制台應用程式，這代表程式本身沒有您習慣的圖型介面。 ASF 主要是設計執行於伺服器中，所以它更偏向為一個服務（常駐程式），而非桌面應用程式。

然而，這並不代表您無法在電腦上使用 ASF，或是使用起來會比一般的程式更為複雜。 ASF 是一個獨立的免安裝應用程式，可以直接使用，但在此之前，需要先進行組態設定。 設定檔會告訴 ASF 啟動之後應該做什麼。 若您在沒有設定檔的情況下啟動它，那麼，ASF 將不會做任何事情，就是如此簡單。

---

## 適用於您的作業系統的設定

一般來說，這是我們在接下來的幾分鐘內要做的事情：
- 安裝必要的 **[.NET 套件](#.NET 套件需求)**。
- 在**[ASF 發布頁面](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)**下載適用於您的作業系統的版本變體。
- 將壓縮檔解壓縮至一個新資料夾中。
- **[設定 ASF](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-TW)**。
- 執行 ASF，並見證奇蹟！

聽起來十分簡單，對吧？ 那就讓我們繼續吧。

---

### .NET 套件需求

首要步驟，是確保您的作業系統可以正確地啟動 ASF。 ASF 是用 C# 語言編寫的，基於 .NET 平台，並可能需要您平台上尚未擁有的原生函式庫。 取決您使用的是 Windows、Linux 還是 macOS，您將有不同的系統需求。所有要求都列在您應遵循的 **[.NET 套件需求](#.NET 套件需求)**文件中。 這是我們應當使用的參考資料，但為了簡單起見，我們在下面額外列出需要的所有軟體套件。因此您無需閱讀完整的文件。

若您已安裝並使用其他第三方軟體，一些（甚至全部）相依性套件已存在於您的作業系統上，是很正常的。 不過，您仍應在作業系統上執行合適的安裝程式，來確保這些軟體確實已被安裝──缺少這些相依套件，ASF 將完全無法啟動。

請注意，您不需要特地為作業系統的建置版本做其他任何事情，特別是 .NET SDK 的安裝或執行環境，因為它們已包含於作業系統套件中。 您只需要安裝必要的 .NET 相依套件，即可執行 ASF 裡面自帶的 .NET 執行環境。

#### **[Windows](https://docs.microsoft.com/dotnet/core/install/windows)**：
- **[Microsoft Visual C++ 可轉散發套件更新](https://docs.microsoft.com/cpp/windows/latest-supported-vc-redist?view=msvc-170#visual-studio-2015-2017-2019-and-2022)**（**[x64](https://aka.ms/vs/16/release/vc_redist.x64.exe)** 適用於 64 位元 Windows；**[x86](https://aka.ms/vs/16/release/vc_redist.x86.exe)** 適用於 32 位元 Windows）
- 強烈建議您，務必確保已安裝所有的 Windows 更新。 您最少需要安裝 **[KB2533623](https://support.microsoft.com/en-us/help/2533623/microsoft-security-advisory-insecure-library-loading-could-allow-remot)**、**[KB2999226](https://support.microsoft.com/en-us/help/2999226/update-for-universal-c-runtime-in-windows)**，但仍可能需要更多的更新。 如果您的 Windows 已更新至最新版，則上述更新補丁都已安裝。 在安裝 Visual C++ 套件前，請確保滿足上述需求。
- 若您使用 Windows 7，您必須使用 **[generic](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up#generic-setup)** 版本變體，因為套件不再支援您的作業系統。 我們強烈建議您盡快進行更新您的作業系統，因為 ASF 的未來版本可能會完全無法在上面使用，而且說實話，Windows 7 在 2020 年就已經結束了生命週期。

#### **[Linux](https://docs.microsoft.com/dotnet/core/install/linux)**：
套件名稱取決於您使用的 Linux 發行版本，我們列出了最常見的套件名稱。 您可以使用本機中的套件管理系統，為您的作業系統取得全部套件（例如適用於 Debian 的 `apt` 或適用於 CentOS 的 `yum`）。

- `ca-certificates`（用於建立 HTTPS 連線的標準可信賴的 SSL 憑證）
- `libc6`（`libc`）
- `libgcc1`（`libgcc`）
- `libicu`（`icu-libs`，您的發行版本的最新版，例如 `libicu67`）
- `libgssapi-krb5-2`（`libkrb5-3`、`krb5-libs`）
- `libssl1.1`（`libssl`、`openssl-libs`，您的發行版本的最新版，且至少為 `1.1.X` 版本，因為 `1.0.X` 版本可能無法運作）
- `libstdc++6`（`libstdc++`，`5.0` 版本或更高）
- `zlib1g`（`zlib`）

上述大多數套件應早已安裝於您的系統中。 Debian 穩定版的最小安裝也只需再安裝 `libicu67` 即可。

#### **[macOS](https://docs.microsoft.com/dotnet/core/install/macos)**：
- 暫時沒有需求，但您仍應安裝最新版本的 macOS，至少為 10.15+ 版本

---

### 下載

既然我們有了所有的相依性套件，那麼接下來就是下載 **[ASF 最新版本](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)**。 ASF 有許多變體版本可供使用，但您應使用符合您作業系統及其架構的版本套件。 舉例來說，假設您使用 `64` 位元 `Win`dows，那麼您需使用 `ASF-win-x64` 版本的套件。 欲取得關於可用變體的更多資訊，請參閱**[相容性](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-zh-TW)**章節。 ASF 也可以執行於沒有提供建置版本的作業系統上，例如 **32 位元 Windows**，請前往**[安裝通用版本套件](#安裝通用版本套件)**繼續閱讀。

![資產](https://i.imgur.com/Ym2xPE5.png)

下載完成後，請先解壓縮 .zip 檔至一個資料夾中。 我們建議使用 **[7-zip](https://www.7-zip.org)**，但所有標準工具（例如 Linux/macOS 中的 `unzip` 工具）應該也可以正常運作。

並建議將 ASF 解壓縮至**獨立資料夾**中，而不是包含其他檔案的資料夾中：ASF 的自動更新功能，會在升級時刪除所有舊的或無關的檔案，並可能導致您遺失儲存於 ASF 資料夾中的其餘檔案。 若您有任何額外用於 ASF 的腳本或檔案，請將它們存放於 ASF 的上層資料夾。

一個檔案結構範例，看起來會像是這樣：

```text
C:\ASF（可存放您自己的東西）
    ├── ASF shortcut.lnk（選擇性）
    ├── Config shortcut.lnk（選擇性）
    ├── Commands.txt（選擇性）
    ├── MyExtraScript.bat（選擇性）
    ├── (...)（其餘您想存放的檔案，選擇性）
    └── Core（ASF 專用資料夾，也就是您解壓縮壓縮檔的地方）
         ├── ArchiSteamFarm(.exe)
         ├── config
         ├── logs
         ├── plugins
         └── (...)
```

---

### 組態設定

現在只剩最後一步：組態設定。 這是目前為止最複雜的步驟，因為它包含了許多您還不熟悉的新資訊，所以我們會嘗試在此提供一些易於理解的範例和簡單的解釋。

首先最重要的，**[設定檔](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-TW)**頁面解釋了跟設定檔有關的**一切事物**，但它包含了數量龐大的新資訊，其中有很大一部分是我們不需要立即瞭解的。 作為代替，我們會教您如何取得您真正應該查找的資訊。

ASF configuration can be done in at least three ways - through our web config generator, ASF-ui or manually. 這在**[設定檔](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)**部分有深入地解釋，所以如果您想要取得更多詳細資訊，請參閱此部分。 We'll use web config generator as a starting point.

使用您最愛的瀏覽器造訪我們的**[網頁設定檔產生器](https://justarchinet.github.io/ASF-WebConfigGenerator)**頁面，在您手動停用了 JavaScript 的情況下您將需要啟用它。 我們建議使用 Chrome 瀏覽器或 Firefox 瀏覽器，不過產生器應該能在大多數熱門瀏覽器上作業。

開啟頁面後，切換到「BOT」分頁。 您應該會看見跟下面圖片相似的頁面：

![Bot tab](https://i.imgur.com/aF3k8Rg.png)

如果您剛剛下載的 ASF 版本低於設定檔產生器的預設值的話，只需要在下拉式選單中選擇您的 ASF 版本號。 這會在設定檔產生器可用於尚未標記為穩定版的更新版（預覽版）ASF時發生。 您已經下載了已證實能可靠工作的最新穩定版 ASF。

從把 BOT 名稱輸入紅色醒目提示的欄位開始。 您可以使用任何名稱，例如您的暱稱、帳戶名稱、一串數字或是任何其他文字。 只有一個不能使用的名稱，`ASF`，因為這個關鍵字是為全域設定檔保留的。 除了 BOT 名稱不能以一個點開始（ASF 會故意略過那些檔案）以外。 我們還建議您避免使用空格，如果需要請使用「`_`」作為分隔符號。

決定好 BOT 名稱之後，把「Enabled」開啟，這個選項決定了您的 BOT 是否應該在 ASF 開啟後自動啟用。

現在您可以在兩件事上做決定：
- 您可以將帳戶名稱填入 `SteamLogin` 欄位，密碼填入 `SteamPassword` 欄位
- 或者您可以留空

選擇做第一件事會允許 ASF 開啟時自動使用您的帳戶認證資訊，這樣您就不需要每次 ASF 需要這些資訊時都手動輸入。 不過您也可以選擇忽略它們，這樣它們就不會被儲存，所以缺少您的幫助 ASF 將無法自動啟動，您需要在執行階段輸入。

ASF 需要您的登入認證，因為它透過內建 Steam 用戶端自行實現，需要跟您自己使用的用戶端相同的資訊來登入。 您的登入認證只會儲存在您的電腦上的 ASF `Config` 目錄，不會儲存在其他地方，我們的網頁設定檔產生器是基於用戶端的，也就是說代碼是在您的瀏覽器本地執行來產生 ASF 設定檔的，您輸入的資訊永遠不會離開您的電腦，所以沒有必要擔心任何可能的敏感資料洩漏。 然而，無論是什麼原因讓您不想輸入登入認證，我們能夠理解，您可以稍後手動輸入到產生的檔案裡，或者徹底省略並只在 ASF 命令提示字元輸入。 更多安全性問題可以在**[設定檔](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-TW)**部分找到。

您也可以選擇只省略一個欄位，例如 `SteamPassword`（Steam 密碼），這樣將能使 ASF 自動登入，不過仍然會詢問密碼（類似 Steam 用戶端）。 如果您在使用 Steam 家庭監護，要解鎖您的帳戶，您需要將 PIN 碼填入 `SteamParentalCode` 欄位。

在一連串的決定和選擇過後，您的網頁現在看起來會像是下圖：

![Bot tab 2](https://i.imgur.com/yf54Ouc.png)

您現在可以按下您現在可以按下「下載」按鈕了，網頁設定檔產生器將會產生一個名為您剛才輸入的名稱的 `json` 檔。 Save that file into `config` directory which is located in the folder where you've extracted our zip file in the previous step.

您的 `config` 目錄現在看來會像是這樣：

![架構 2](https://i.imgur.com/crWdjcp.png)

恭喜！ 您剛剛完成了最基本的 ASF BOT 設定。 我們之後將會擴充設定檔，現在您暫時只需要這些。

---

### 執行 ASF

現在您已經準備好第一次啟動程式了。 Simply double-click `ArchiSteamFarm` binary in ASF directory. You can also start it from the console.

按下以後，如果您在第一步正確地安裝了所有相依性，ASF 應該會正確地啟動，注意您的第一個 BOT（如果您沒忘記把產生的設定檔置放在 `config` 目錄裡），然後嘗試登入：

![ASF](https://i.imgur.com/u5hrSFz.png)

如果您在設定檔提供了 `SteamLogin` 和 `SteamPassword` 給 ASF 使用，ASF 將只會詢問您的 Steam Guard 權杖（電子郵件、兩步驟驗證或無，取決於您的 Steam 設定）。 如果您沒有提供這兩項，ASF 也會同時詢問您的 Steam 帳戶名稱和密碼。

Now would be a good time to review our **[remote communication](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Remote-communication)** section if you're concerned about stuff ASF is programmed to do, including actions it'll take in your name.

After passing through initial login gate, assuming your details are correct, you'll successfully log in, and ASF will start farming using default settings that you didn't change as of now:

![ASF 2](https://i.imgur.com/Cb7DBl4.png)

這表明 ASF 現在成功在您的帳戶上工作了，所以您現在可以最小化程式然後幹其他事。 經過足夠的時間後（取決於**[效能](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance-zh-TW)**），您將會看到 Steam 交換卡片慢慢掉落。 Of course, for that to happen you must have valid games to farm, showing as "you can get X more card drops from playing this game" on your **[badges page](https://steamcommunity.com/my/badges)** - if there are no games to farm, then ASF will state that there is nothing to do, as stated in our **[FAQ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ#what-is-asf)**.

我們最基本的新手上路指南到此結束。 您現在可以決定要進一步設定 ASF，還是讓 ASF 以預設設定工作。 我們將會介紹更多基本細節，然後您可以自己探索整個 Wiki。

---

### 擴充設定

#### 多個帳戶同時掛卡

ASF 支援一個帳戶以上同時掛卡，也是它的主要功能。 您可以透過產生更多 BOT 設定檔來添加更多帳戶，方法跟您幾分鐘前產生的第一個設定檔完全相同。 您只需要確保兩件事：

- BOT 名稱唯一，例如假設您的第一個 BOT 名為「主帳戶」，您就不能擁有另一個跟它名稱一樣的 BOT。
- 登入資訊有效，例如 `SteamLogin`、`SteamPassword` 和 `SteamParentalCode`（如果正在使用 Steam 家庭監護）

換句話說，只要再次跳轉到設定檔部分，然後做完全一樣的事，只不過這次要填入您第二或是第三個帳戶的資訊。 記住為您所有的 BOT 使用唯一的名稱。

---

#### 變更設定

您可以透過完全相同的方式來變更現有的設定——產生一個新的設定檔。 如果您還沒有關閉網頁設定檔產生器，按一下「開啟進階設定」然後看看有什麼是您發現您所需要的選項。 For this tutorial we'll change `CustomGamePlayedWhileFarming` setting, which allows you to set custom name being displayed when ASF is farming, instead of showing actual game.

So let's do that, if you run ASF and start farming, in default settings you'll see that your Steam account is in-game now:

![Steam](https://i.imgur.com/1VCDrGC.png)

那就讓我們一起來完善吧。 在網頁設定檔產生器中按一下「開啟進階設定」然後找到 `CustomGamePlayedWhileFarming`。 找到後，輸入您想要顯示的的自訂文字，例如「掛卡中（Idling cards）」：

![Bot tab 3](https://i.imgur.com/gHqdEqb.png)

現在跟之前一樣下載新的設定檔，然後用新的設定檔**取代**舊的。 當然，您也可以先刪除舊的設定檔，然後再放置新的。

一旦完成並重新啟動 ASF，您將注意到現在 ASF 會在之前的位置顯示您的自訂文字：

![Steam 2](https://i.imgur.com/vZg0G8P.png)

This confirms that you've successfully edited your config. In exactly the same way you can change global ASF properties, by switching from bot tab to "ASF" tab, downloading generated `ASF.json` config file and putting it in your `config` directory.

Editing your ASF configs can be done much easier by using our ASF-ui frontend, which will be explained further below.

---

#### 使用 ASF-ui

ASF 是一個沒有圖形使用者介面的主控台應用程式。 然而，我們正在積極開發 IPC 介面的 **[ASF-ui](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-zh-TW#asf-ui)** 前端，一個存取各種 ASF 功能非常方便的使用者友好型方式。

In order to use ASF-ui, you need to have `IPC` enabled, which is the default option starting with ASF V5.1.0.0. Once you launch ASF, you should be able to confirm that it properly started the IPC interface automatically:

![IPC](https://i.imgur.com/ZmkO8pk.png)

You can access ASF's IPC interface under **[this](http://localhost:1242)** link, as long as ASF is running, from the same machine. You can use ASF-ui for various purposes, e.g. editing the config files in-place or sending **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. 請隨意看看來發現 ASF-ui 的全部功能。

![ASF-ui](https://raw.githubusercontent.com/JustArchiNET/ASF-ui/main/.github/previews/bots.png)

---

### 概要

您已經成功設定好讓 ASF 使用您的 Steam 帳戶，並且根據您的喜好進行了客製化。 If you followed our entire guide, then you also managed to tweak ASF through our ASF-ui interface and found out that ASF actually has a GUI of some sort. Now is a good time to read our entire **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** section in order to learn what all those different settings you've seen actually do, and what ASF has to offer. If you've stumbled upon some issue or you have some generic question, read our **[FAQ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ)** instead which should cover all, or at least a vast majority of questions that you may have. 如果您想瞭解關於 ASF 的一切以及它如何讓您掛卡事半功倍，請閱讀 **[ASF Wiki](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Home-zh-TW)** 的剩餘部分。 If you found out our program to be useful for you and you're feeling generous, you can also consider donating to our project. In any case, have fun!

---

## 安裝通用版本套件

這部分是為想要安裝 ASF **[通用（Generic）](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-zh-TW#通用Generic)** 套件的進階使用者準備的。 如果您可以**[安裝特定作業系統（OS-specific）套件](#安裝特定作業系統OS-specific套件)**，我們不推薦安裝您安装通用套件。

You want to use `generic` variant mainly in those situations (but of course you can use it regardless):
- 當您正在使用沒有建置特定作業系統套件的系統（例如 32 位元 Windows）
- When you already have .NET Runtime/SDK, or want to install and use one
- 當您想透過自行管理執行階段必要條件來最小化 ASF 結構大小
- When you want to use a custom **[plugin](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins)** which requires a `generic` setup of ASF to run properly (due to missing native dependencies)

However, keep in mind that you're in charge of .NET runtime in this case. This means that if your .NET SDK (runtime) is unavailable, outdated or broken, ASF won't work. This is why we don't recommend this setup for casual users, since you now need to ensure that your .NET SDK (runtime) matches ASF requirements and can run ASF, as opposed to **us** ensuring that our .NET runtime bundled with ASF can do so.

For `generic` package, you can follow entire OS-specific guide above, with two small changes. In addition to installing .NET prerequisites, you also want to install .NET SDK, and instead of having OS-specific `ArchiSteamFarm(.exe)` executable file, you now have a generic `ArchiSteamFarm.dll` binary only. 其他步驟完全一樣。

額外步驟：
- 安裝 **[.NET 的必要條件](#net-prerequisites)**。
- Install **[.NET SDK](https://www.microsoft.com/net/download)** (or at least ASP.NET Core runtime) appropriate for your OS. 大部分情況下您會需要一個安裝程式。 如果您不知道要安裝 .NET Core 的哪一個版本，請參閱**[執行階段必要條件](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-zh-TW#執行階段必要條件)**。
- Download **[latest ASF release](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** in `generic` variant.
- 將壓縮檔解壓到一個新資料夾。
- **[設定 ASF](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-TW)**
- 透過輔助腳本或是手動在 Shell 中執行 `dotnet /path/to/ArchiSteamFarm.dll` 指令來啟動 ASF。

Helper scripts (such as `ArchiSteamFarm.cmd` for Windows and `ArchiSteamFarm.sh` for Linux/macOS) are located next to `ArchiSteamFarm.dll` binary - those are included in `generic` variant only. 如果您不想手動執行 `dotnet` 指令，請使用輔助腳本。 Obviously helper scripts won't work if you didn't install .NET SDK and you don't have `dotnet` executable available in your `PATH`. 輔助腳本完全是非必要的，您永遠可以透過手動執行 `dotnet /path/to/ArchiSteamFarm.dll` 指令來啟動 ASF。