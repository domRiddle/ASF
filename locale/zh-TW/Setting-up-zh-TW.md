# 新手上路

如果您是第一次來到這裡，歡迎！ 我們很高興又看到一位對我們的專案感興趣的旅客，但請記住：能力越強，責任越大—— ASF 能夠完成許多不同的 Steam 相關事項，但您必須**足夠小心地學習使用方法**。 入門 ASF 的過程將會是一條陡峭的學習曲線，我們希望您在這方面閱讀 Wiki，它會詳細解釋一切如何運作。

如果您還在這裡，那就表示您堅持看完了上面的文字，非常好。 除非您跳過了上面的文字，那樣您很快就會經歷一段**[糟糕的時間](https://www.youtube.com/watch?v=WJgt6m6njVw)**…… 總之，ASF 是一個控制台應用程序，這意味著應用程式本身沒有你習慣的友好的圖型介面。 ASF 主要設計應該在伺服器上執行，所以它只是一個服務（daemon）而非桌面應用程式。

然而這並不代表，您不能在電腦上使用 ASF 或是使用它會比一般的程式更複雜，並非如此。 ASF 是一個無需安裝的獨立程式，並且可以立即使用，但在這之前需要進行設定。 設定檔會告訴 ASF 啟動之後應該做什麼。 如果你在沒有設定檔的情況下啟動它，那麼 ASF 將不會做任何事情，就是這麼簡單。

* * *

## 特定作業系統設定

一般來說，這是我們在接下來的幾分鐘內要做的事情：

- 安裝 **[.NET Core 必要條件](#net-core-prerequisites)**。
- 下載適合您作業系統的**[最新 ASF 版本](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)**變體。
- 解壓縮檔案到新位置（若使用Linux/OS X 系統，請執行指令 `chmod +x ArchiSteamFarm`）。
- **[設定 ASF](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-TW)**
- 執行 ASF 並觀賞神奇的一刻

聽起來有夠簡單，是吧？ 那就一起繼續吧。

* * *

### .NET Core 必要條件

第一步是確保您的作業系統可以正確地啟動 ASF。 ASF 是用 C# 語言編寫的，基於 .NET Core，並且可能需要你的平台上尚不可用的原生程式庫。 根據您是否使用 Windows、Linux 或 OS X，您將有不同的要求，它們都列在您應該遵循的 **[.NET 核心套件](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)** 檔案中。 這是我們應該使用的參考資料，但為了簡單起見，我們還額外詳細說明了下面所有需要的軟體。因此您無需閱讀完整的文件。

由於您正在使用的第三方軟體，一些（甚至全部）相依性已經存在於您的作業系統是完全正常的。 但您仍應透過在作業系統上執行合適的安裝器來確保：這些軟體確實已經安裝——缺少這些相依性，ASF 將完全無法啟動。

請注意您不需要為特定作業系統的組建做其他事，特別是安裝 .NET Core SDK 或者甚至是執行階段，因為已經全部包含在特定作業系統套件中。 您只需要 .NET Core 必要條件（相依性）以執行包括在 ASF 內的 .NET Core 執行階段。

#### **[Windows](https://docs.microsoft.com/en-us/dotnet/core/windows-prerequisites?tabs=netcore30)**：

- **[Microsoft Visual C++ 2015 可轉散發套件 Update](https://www.microsoft.com/en-us/download/details.aspx?id=53587)**（64 位元版本 Windows 請下載 x64，32 位元版本 Windows 請下載 x86）
- 強烈建議確保已安裝所有Windows更新。 至少需要 **[KB2533623](https://support.microsoft.com/en-us/help/2533623/microsoft-security-advisory-insecure-library-loading-could-allow-remot)** 和 **[KB2999226](https://support.microsoft.com/en-us/help/2999226/update-for-universal-c-runtime-in-windows)** 版本，並可能需要更新的版本。 如果您的 Windows 更新到最新版，則上述所有都已安裝。 在安裝 Visual C++ 套件之前，請確保滿足這些要求。

#### **[Linux](https://docs.microsoft.com/en-us/dotnet/core/linux-prerequisites?tabs=netcore30)**：

套件名稱取決於您正在使用的 Linux 發行版，我們已經列出了最常見的套件。 您可以使用本地套件管理系統，為您的作業系統取得全部套件（例如 Debian 的 `apt` 或 CentOS 的 `yum`）。

- `libcurl`（`libcurl4`、`libcurl3`）
- `libicu`（您的 Linux 發行版的最新版本，例如 `libicu60`）
- `libkrb5-3`（`krb5-libs`）
- `liblttng-ust0`（`lttng-ust`）
- `libssl`（`libssl1.1`、`openssl-libs`、您的 Linux 發行版最新的 1.1.X 版本）
- `zlib1g`（`zlib`）

其中至少應該有幾個套件已經可用於您的系統本地了（例如現今幾乎每一個 Linux 發行版都需要 `zlib1g`）。

#### **[OS X](https://docs.microsoft.com/en-us/dotnet/core/macos-prerequisites?tabs=netcore30)**：

- 暫時沒有必要條件，但你應該安裝最新的 OS X 版本，至少 10.13 以上版本。

* * *

### 下載

既然我們已經擁有所有相依性程式，下一步就是下載**[最新的 ASF 版本](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)**。 ASF 有許多變體可用，但您應該關注符合您作業系統和系統架構的套件。 例如，如果您正在使用 `64` 位元 `Win`dows，那麼您將需要 `ASF-win-x64` 套件。 要取得更多關於可用變體的資訊，請參閱**[相容性](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-zh-TW)**部分。 ASF 也可以執行在我們沒有建置特定作業系統套件的系統上，例如 **32 位元 Windows** 請前往**[安裝通用（Generic）套件](#generic-setup)**。

![資產](https://i.imgur.com/Ym2xPE5.png)

下載完成之後，從解壓縮 ZIP 檔至一個資料夾中開始。 我們建議使用 **[7-zip](https://www.7-zip.org)**，Linux/OS X 的標準工具程式（例如 `unzip`）應該也沒有問題。 解壓縮之後，您將會擁有一大堆資料夾和檔案。 別擔心，我們馬上就來整理這些資料。

如果您正在使用 Linux/OS X，別忘記使用 `chmod +x ArchiSteamFarm` 指令，因為權限不會自動設定在 ZIP 檔中。 此操作只需在第一次解壓縮後執行一次。

建議將 ASF 解壓縮到**獨立的目錄**中而不是任何已有其他用途的目錄—— ASF 的自動更新功能將會在升級時刪除所有舊的和無關檔案，可能會使您丟失您儲存於 ASF 目錄的任何檔案。 如果您有任何想用於 ASF 的額外腳本或檔案，請將他們置放在 ASF 資料夾的上一層。

一個範例結構看起來會像是這樣：

    C:\ASF（存放您個人檔案的位置）
        ├── ASF 捷徑.lnk（可省略）
        ├── 設定檔捷徑.lnk（即下面「核心」裡的「config」資料夾，可省略）
        ├── 指令.txt（可省略）
        ├── 額外腳本.bat（可省略）
        ├── ... （您選擇的任何其他檔案，可省略）
        └── 核心（專用於 ASF，您解壓縮檔案的位置）
             ├── ArchiSteamFarm.dll
             ├── config
             └──（...）
    

這也是我們建議的結構，這樣您就不需要仔細查看 ASF 包含的大量檔案和資料夾，因為 ASF 的使用只需要設定檔（config）資料夾和主程式的捷徑。

好了，現在就來準備 ASF 資料夾。 如果您不想，現在就可以跳到下一個步驟，因為釐清 ASF 結構並不是必需的，但能使您更方便使用。

開啟 ASF 資料夾，然後找到核心執行檔：在 Windows 上是 `ArchiSteamFarm.exe`，在 Linux/OS X 上是 `ArchiSteamFarm`。右鍵按一下它然後選取「複製」。 現在瀏覽到您想要置放 ASF 捷徑的位置（例如您的桌面），按右鍵然後選擇「貼上捷徑」。 若您想要，您可以重新命名捷徑，例如重新命名為「ASF」。 現在對設定檔目錄「`config`」資料夾做同樣的事情，您可在與 ASF 主程式同樣的位置找到它。

經過簡單整理，您現在將擁有與下方圖片類似的非常方便的結構：

![架構](https://i.imgur.com/k85csaZ.png)

這會使您更方便存取 ASF 主程式和設定檔。 我自己是決定使用上面提到的結構，所以我的 ASF 檔案直接放在「核心」目錄裡。 您可以根據自己的喜好改造這個結構，例如在桌面建立 ASF 和設定檔捷徑然後將 ASF 目錄建立在` C:\ASF`，一切都由您決定。

Linux/OS X 使用者也建議做相同的事情，您可以透過 `ln -s` 指令來使用出色的符號連結機制。

* * *

### 設定檔

現在只剩最後一步：設定檔。 這是目前為止最複雜的步驟，因為它包含了許多您還不熟悉的新資訊，所以我們會嘗試在此提供一些易於理解的範例和簡單的解釋。

首先最重要的，**[設定檔](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-TW)**頁面解釋了跟設定檔有關的**一切事物**，但它包含了數量龐大的新資訊，其中有很大一部分是我們不需要立即瞭解的。 作為代替，我們會教您如何取得您真正應該查找的資訊。

ASF 設定檔可以用兩種方法完成——要不使用我們的網頁設定檔產生器，要不就是手動設定。 這在**[設定檔](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)**部分有深入地解釋，所以如果您想要取得更多詳細資訊，請參閱此部分。 我們將會使用網頁設定檔產生器的方法，因為這會更簡單。

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

您現在可以按下您現在可以按下「下載」按鈕了，網頁設定檔產生器將會產生一個名為您剛才輸入的名稱的 `json` 檔。 將檔案儲存到 ASF 的 `config` 目錄裡。 您可以使用您稍早建立的`設定檔`捷徑，或者直接在 ASF 檔案結構裡手動找到 `config` 目錄。

您的 `config` 目錄現在看來會像是這樣：

![架構 2](https://i.imgur.com/crWdjcp.png)

恭喜！ 您剛剛完成了最基本的 ASF BOT 設定。 我們之後將會擴充設定檔，現在您暫時只需要這些。

* * *

### 執行 ASF

現在您已經準備好第一次啟動程式了。 只需要按兩下 ASF 捷徑或 ASF 目錄裡的 `ArchiSteamFarm(.exe)` 執行檔。

按下以後，如果您在第一步正確地安裝了所有相依性，ASF 應該會正確地啟動，注意您的第一個 BOT（如果您沒忘記把產生的設定檔置放在 `config` 目錄裡），然後嘗試登入：

![ASF](https://i.imgur.com/u5hrSFz.png)

如果您在設定檔提供了 `SteamLogin` 和 `SteamPassword` 給 ASF 使用，ASF 將只會詢問您的 Steam Guard 權杖（電子郵件、兩步驟驗證或無，取決於您的 Steam 設定）。 如果您沒有提供這兩項，ASF 也會同時詢問您的 Steam 帳戶名稱和密碼。

如果您擔心 ASF 陳述的接下來會發生的事情，現在是仔細檢查我們的**[隱私權政策](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Statistics-zh-TW#當前的隱私權政策)**的好時機。

初次登入通過後，假設您的資訊填寫無誤，您的帳戶將會成功登入，ASF 也將會使用您未曾變更的預設設定開始掛卡：

![ASF 2](https://i.imgur.com/Cb7DBl4.png)

這表明 ASF 現在成功在您的帳戶上工作了，所以您現在可以最小化程式然後幹其他事。 經過足夠的時間後（取決於**[效能](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance-zh-TW)**），您將會看到 Steam 交換卡片慢慢掉落。 當然，要使那發生，您必須擁有可以掛卡的遊戲，可以在 **[Steam 徽章](https://steamcommunity.com/my/badges)**頁面看到「還有 N 張卡片會掉落」的藍色提示——如果沒有遊戲可以掛卡，那麼 ASF 將會提示「這個帳戶目前沒有需要掛卡的遊戲！」，跟我們的**[常見問題](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ-zh-TW#什麼是-ASF)**裡描述的一樣。

我們最基本的新手上路指南到此結束。 您現在可以決定要進一步設定 ASF，還是讓 ASF 以預設設定工作。 我們將會介紹更多基本細節，然後您可以自己探索整個 Wiki。

* * *

### 擴充設定

#### 多個帳戶同時掛卡

ASF 支援不止一個帳戶同時掛卡，也是它的主要功能。 您可以透過產生更多 BOT 設定檔來添加更多帳戶，方法跟您幾分鐘前產生的第一個設定檔完全相同。 您只需要確保兩件事：

- BOT 名稱唯一，例如假設您的第一個 BOT 名為「主帳戶」，您就不能擁有另一個跟它名稱一樣的 BOT。
- 登入資訊有效，例如 `SteamLogin`、`SteamPassword` 和 `SteamParentalCode`（如果正在使用 Steam 家庭監護）

換句話說，只要再次跳轉到設定檔部分，然後做完全一樣的事，只不過這次要填入您第二或是第三個帳戶的資訊。 記住為您所有的 BOT 使用唯一的名稱。

* * *

#### 變更設定

您可以透過完全相同的方式來變更現有的設定——產生一個新的設定檔。 如果您還沒有關閉網頁設定檔產生器，按一下「開啟進階設定」然後看看有什麼是您發現您所需要的選項。 在這個教學課程裡，我們要來變更 `CustomGamePlayedWhileFarming` 設定，這個選項使您可以設定 ASF 掛卡時顯示自訂遊戲名稱，而不是真正的遊戲名稱。

那就開始吧，如果您用預設設定執行 ASF 並開始掛卡，您將會看見您的 Steam 帳戶正在遊戲中：

![Steam](https://i.imgur.com/1VCDrGC.png)

那就讓我們一起來完善吧。 在網頁設定檔產生器中按一下「開啟進階設定」然後找到 `CustomGamePlayedWhileFarming`。 找到後，輸入您想要顯示的的自訂文字，例如「掛卡中（Idling cards）」：

![Bot tab 3](https://i.imgur.com/gHqdEqb.png)

現在跟之前一樣下載新的設定檔，然後用新的設定檔**取代**舊的。 當然，您也可以先刪除舊的設定檔，然後再放置新的。

一旦完成並重新啟動 ASF，您將注意到現在 ASF 會在之前的位置顯示您的自訂文字：

![Steam 2](https://i.imgur.com/vZg0G8P.png)

這表示您成功地編輯了設定檔。您也可以用完全相同的方法來變更全域 ASF 屬性，切換到「分頁」，下載產生的 `ASF.json` 設定檔，然後放置在 `config` 目錄裡。

* * *

#### 使用 ASF-ui

ASF 是一個沒有圖形使用者介面的主控台應用程式。 然而，我們正在積極開發 IPC 介面的 **[ASF-ui](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-zh-TW#asf-ui)** 前端，一個存取各種 ASF 功能非常方便的使用者友好型方式。

要使用 ASF-ui，您應該確保（在 ASF 分頁）設定了全域設定檔屬性 `IPC` 和 `SteamOwnerID`。

您需要將您帐户獨一無二的 Steam 64 位 ID 填入 `SteamOwnerID`。 您可以透過各種方法查看 ID，我們將會使用 **[SteamRep](https://steamrep.com)** 來查看。 開啟網站，在右上角找到「Sign in through Steam」按鈕，然後登入。 然後在相同的位置，按一下您的個人頭像，然後在您的個人資料中查看 `steamID64` 欄位。

![SteamRep](https://i.imgur.com/RUuJ63i.png)

例如我的帳戶的 Steam 64位 ID 就是這串數字 `76561198006963719`。 您的 ID 也與其相似並同樣以 `7656` 開始。 複製它。

現在重新回到網頁設定檔產生器，然後把剛剛複製的數字貼上 SteamOwnerID。

![SteamOwnerID](https://i.imgur.com/V6jslfQ.png)

您只需要再做一件事：開啟進階設定，找到並啟用 `IPC` 選項。

![IPC](https://i.imgur.com/NhujZCN.png)

現在您可以下載 ASF 設定檔並放置在 `config` 目錄，跟往常一樣。 然後重新啟動 ASF，您應該可以確認 IPC 介面正確啟動了：

![IPC 2](https://i.imgur.com/ZmkO8pk.png)

如果您所做的一切無誤，只要 ASF 正在執行，您現在將可以透過**[此連結](http://localhost:1242)**來存取 ASF 的 IPC 介面。 您可以使用 ASF-ui 用於各種目的，例如發送**[指令](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-TW)**。 請隨意看看來發現 ASF-ui 的全部功能。

![ASF-ui](https://raw.githubusercontent.com/JustArchiNET/ASF-ui/master/.github/previews/bots.png)

請注意，ASF-ui 當前處於預覽狀態，暫時無法保證所有功能都可用/正常工作，但要應對簡單的 ASF 使用應該綽綽有餘。

* * *

### 概要

您已經成功設定好讓 ASF 使用您的 Steam 帳戶，並且根據您的喜好進行了客製化。 If you followed our entire guide, then you even managed to send a simple command through our ASF-ui interface. Now is a good time to read our entire **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** section in order to learn what all those different settings you've seen in advanced tab actually do, and what ASF can offer. If you've stumbled upon some issue or you have some generic question, read **[FAQ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ)** instead which should cover all, or at least majority of questions that you may have. If you want to learn everything about ASF and how it can make your life easier, head over to the rest of **[our wiki](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Home)**. 祝您愉快！

* * *

## 安裝通用（Generic）套件

This setup is for advanced users that want to set up ASF to run in **[generic](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#generic)** variant. It's not recommended for people that can use **[OS-specific setup](#os-specific-setup)**.

You want to use generic variant mainly in three situations (but of course you can use it regardless):

- When you're using OS that we don't build OS-specific package for (such as 32-bit Windows)
- When you already have .NET Core Runtime/SDK, or want to install and use one
- When you want to minimize ASF structure size by handling runtime requirements yourself

However, keep in mind that you're in charge of .NET Core runtime in this case. This means that if your .NET Core SDK (runtime) is unavailable, outdated or broken, ASF won't work. This is why we don't recommend this setup for casual users, since you now need to ensure that your .NET Core SDK (runtime) matches ASF requirements and can run ASF, as opposed to **us** ensuring that our .NET Core runtime bundled with ASF can do so.

For generic package, you can follow entire OS-specific guide above, with two small changes. In addition to installing .NET Core prerequisites, you also want to install .NET Core SDK, and instead of having OS-specific `ArchiSteamFarm(.exe)` executable file, you now have a generic `ArchiSteamFarm.dll` binary only. Everything else is exactly the same.

額外步驟：

- 安裝 **[.NET 核心套件](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)**。
- 安裝適用於您作業系統的 **[.NET Core SDK](https://www.microsoft.com/net/download)**（或者至少安裝執行階段）。 You most likely want to use an installer. Refer to **[runtime requirements](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)** if you're not sure which version to install.
- Download **[latest ASF release](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** in generic variant.
- Extract the archive into new location (and `chmod +x ArchiSteamFarm.sh` if you're on Linux/OS X).
- **[設定 ASF](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-TW)**
- Launch ASF by either using a helper script or executing `dotnet /path/to/ArchiSteamFarm.dll` manually from your favourite shell.

Helper scripts (such as `ArchiSteamFarm.cmd` for Windows and `ArchiSteamFarm.sh` for Linux/OS X) are located next to `ArchiSteamFarm.dll` binary - those are included in generic variant only. You can use them if you don't want to execute `dotnet` command manually. You can also make a shortcut to those scripts like showed above, since they're supposed to provide binary replacement in a script way. Obviously helper scripts won't work if you didn't install .NET Core SDK and you don't have `dotnet` executable available in your `PATH`. Helper scripts are entirely optional to use, you can always `dotnet /path/to/ArchiSteamFarm.dll` manually.