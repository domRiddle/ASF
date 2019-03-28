# 設置指南

如果您是首次來訪，歡迎！ 我們很高興又見到一位對我們的項目感興趣的訪客，但請記住能力越強責任越大──只要你**足夠認真去學習如何使用它**，ASF 有能力完成非常多的 Steam 相關事務。 書山有路勤為徑，學海無涯苦作舟，在開始這段陡峭的學習旅途之前，我們期待您閱讀這方面的wiki，詳細了解這一切是如何運作的。

如果您還在這裏，那就意味著您認同我們上面的文字，不錯。 除非你跳過它，否則您很快就會經歷**[一段艱難的時間](https://www.youtube.com/watch?v=WJgt6m6njVw)**⋯⋯ 無論如何，ASF是一個控制台應用程序，這意味著應用程式不會提供您習慣的友好圖型介面。 ASF 主要設計以在伺服器上執行，所以它僅作為一個服務（守護進程）運行，而非桌面應用程式。

然而這並不代表您不能在 PC 上使用它，或它在某種程度上比常見的程式更複雜，並非如此。 ASF是一個獨立的程式，不需要安裝並且可以立即使用，但這之前需要進行設置。 設置 ASF 以定義它啟動之後應該做什麼。 如果你在沒有設置的情況下啟動它，那麼 ASF 將不會做任何事情，就是這麼簡單。

* * *

## 特定操作系統設置

一般來說，這是我們在接下來的幾分鐘內要做的事情：

- 安裝 **[.NET 核心套件](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)**。
- 下載適合您操作系統的**[最新版 ASF ](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)**
- 將安裝包解壓縮到您指定的位置 ( 若你使用Linux/OS X 系統，則需執行 `chmod +x ArchiSteamFarm`命令)。
- **[配置ASF](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)**。
- 啟動 ASF，見證奇蹟的時刻！

輕而易舉，對吧？ 所以讓我們繼續吧。

* * *

### .NET 核心套件

第一步是確保您的操作系統可以正確地啟動 ASF。 ASF 以 C# 編寫的，基於. net core，可能需要您的平台上尚不可用的本機庫。 根據您是否使用 Windows、Linux 或 OS X, 您將有不同的要求, 它們都列在您應該遵循的 **[.NET 核心套件](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)** 文檔中。 這是我們應該使用的參考資料，但為了簡單起見，我們還在下面詳細介紹了所有需要的套裝軟件。因此您無需閱讀完整的文檔。

在通常情況下，您正在使用的其他軟件已安裝了其依賴項，因此您的操作系統上可能已經存在某些（甚至全部）依賴項。 然而，您還是應運行相應安裝程式以確保您的操作系統上確實已經存在 ASF 的依賴項──否則 ASF 無法啟動。

請謹記，您不需要為特定于操作系統的 ASF 安裝包執行任何其他操作，尤其是 .NET Core SDK 甚至是運行時環境，因為此包自帶這些內容。 您只需要 .NET Core 核心套件（依賴項）即可運行 ASF 中包含的 .NET Core 運行時。

#### **[Windows](https://docs.microsoft.com/en-us/dotnet/core/windows-prerequisites?tabs=netcore2x)**:

- **[Microsoft Visual C++ 2015 Redistributable Update 3 RC](https://www.microsoft.com/en-us/download/details.aspx?id=52685)** (x64 for 64-bit Windows, x86 for 32-bit Windows)
- 強烈建議您確保已安裝所有 Windows 更新。 至少需要 **[KB2533623](https://support.microsoft.com/en-us/help/2533623/microsoft-security-advisory-insecure-library-loading-could-allow-remot)** 和 **[KB2999226](https://support.microsoft.com/en-us/help/2999226/update-for-universal-c-runtime-in-windows)**，並可能需要更多更新檔。 如果您的 Windows 是最新版本，則上述所有更新都已安裝。 在安裝 Visual C ++ 套件之前，請確保滿足這些要求。

#### **[Linux](https://docs.microsoft.com/en-us/dotnet/core/linux-prerequisites?tabs=netcore2x)**:

套裝名稱因您使用的 Linux 版本而異，我們列出了最常見的套裝軟件名稱。 您可以獲得所有這些適用於您操作系統的本機套件管理器（如適用於 Debian 的`apt`或適用於 CentOS 的 `yum`）。

- libcurl3 (libcurl)
- libicu（適用於您操作系統的最新版本，例如用於 Debian 9 的`libicu57`）
- libkrb5-3 (krb5-libs)
- liblttng-ust0 (lttng-ust)
- libssl1.0.2 (libssl, openssl-libs, compat-openssl10, 適用於您操作系統的最新版 1.0.X)
- zlib1g (zlib)

At least a few of those should be already natively available on your system (such as zlib1g that is required in almost every Linux distro nowadays).

#### **[OS X](https://docs.microsoft.com/en-us/dotnet/core/macos-prerequisites?tabs=netcore2x)**:

- None for now, but you should have latest version of OS X installed, at least 10.12+

* * *

### 下載

由於我們已經有了所有必需的依賴項，下一步是下載 **[最新發佈的ASF](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)**。 ASF有多種型號可供選擇，您盡可選擇與您的操作系統和架構相匹配的軟件包。 例如，如果您使用` 64 `位` Windows `，那麼您需要` ASF-win-x64 `包。 有關可用變體的更多信息，請訪問** [兼容性](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility) **部分。 ASF也可以運行在我們沒有為其構建特定於操作系統的軟件包的操作系統上，例如** 32位Windows **，詳情請見** <a href =“＃generic-setup” >通用設置</a> **。

![資產](https://i.imgur.com/Ym2xPE5.png)

After download, start from extracting the zip file into its own folder. We recommend using **[7-zip](https://www.7-zip.org)**, standard utilities like `unzip` from Linux/OS X should work without problems as well. Afterwards, you'll have a huge mess of folders and files. 別擔心，我們會迅速清理它。

如果您使用的是Linux / OS X，請不要忘記執行` chmod + x ArchiSteamFarm `命令，因為權限不會自動設置在zip檔案中。 這在解壓縮後只需進行一次。

建議將ASF解壓縮到**專用目錄**，而不是解壓縮到已經用於其他目的的任何現有目錄——ASF的自動更新功能將在升級時刪除所有舊的和不相關的檔案，這可能會導致 你丟失任何與ASF目錄無關的東西。 如果您有任何要與ASF一起使用的額外腳本或档案，請將它們放在之前提到的資料夾中。

示例結構如下所示：

    C:\ASF (where you put your own things)
        ├── ASF shortcut.lnk (optional)
        ├── Config shortcut.lnk (optional)
        ├── Commands.txt (optional)
        ├── MyExtraScript.bat (optional)
        ├── ... (any other files of your choice, optional)
        └── Core (dedicated to ASF only, where you extract the archive)
             ├── ArchiSteamFarm.dll
             ├── config
             └── (...)
    

這也是我們推薦的結構，因此您不需要查看ASF中包含的大量資料夾和檔案，因為您只需要一個配置資料夾和主二進制檔的快捷方式用於使用。

好的，我們現在準備好了可供使用的ASF資料夾。 如果您希望，現在可以跳到下一步，因為清理 ASF 結構不是必需的，但它會讓您更輕鬆一些。

打開ASF資料夾並找到核心可執行檔，這將是Windows上的` ArchiSteamFarm.exe `，或Linux / OS X上的` ArchiSteamFarm `。右鍵單擊它並選擇“複製”。 現在導航到您實際想要放置 ASF 快捷方式的地方（如桌面）， 按右鍵並選擇「粘貼快捷方式」。 如果需要，可以重命名快捷方式，例如為其命名為「ASF」。 現在，您可以在與二進位ASF檔案相同的位置找到 `config` 目錄。

經過一次迅速清理後，您現在將擁有一個簡便的結構，類似這樣：

![架構](https://i.imgur.com/k85csaZ.png)

這將允許您輕鬆訪問二進位ASF檔案和設定檔。 於我而言，我決定使用上面提到的結構，所以我將 ASF 檔直接放置在 「Core」 目錄中。 您可以根據自己的喜好調整此結構，例如在桌面和 ASF 目錄中使用 ASF + 配置快捷方式，例如在 `C:\ASF` 中，這取決於您自己。

Linux/OS X users are advised to do the same, you can use excellent symbolic links mechanism available through `ln -s`.

* * *

### 配置

我們現在已經準備好做最後一步，配置。 這是迄今為止最複雜的一步，因為它涉及許多您還不熟悉的新信息，因此我們將嘗試在此提供一些易於理解的示例和簡化說明。

首先，** [配置](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration) **頁面解釋了**一切**與配置有關的事情，但它包含大量的新信息，其中的很多信息我們並不需要立即理解。 相反，我們將教您如何獲取您實際要找的資訊。

ASF configuration can be done in two ways - either by using our web config generator, or manually. 這將在 **[配置](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** 部分中進行深入解釋，因此如果您想要更詳細的資訊，請參考。 我們將使用便於操作的網頁設定檔產生器。

使用您喜歡的瀏覽器導航到我們的** [網頁設定檔產生器](https://justarchinet.github.io/ASF-WebConfigGenerator) **頁面，若您之前手動禁用了 Javascript，您需要啓用之。 我們推薦 Chrome 或 Firefox，但它應該適用于當前所有流行的瀏覽器。

打開頁面後，切換到「機械人」選項卡。 現在，您應該會看到類似于下面的頁面：

![Bot tab](https://i.imgur.com/aF3k8Rg.png)

如果您剛剛下載的 ASF 版本比網頁設定檔產生器的預設版本更舊，從下拉菜單中選擇您的 ASF 版本即可。 因為配置生成器可用於生成配置到尚未標記為穩定的較新（預發布）ASF 版本，所以這种情況有可能發生。 您下載的是經過驗證可靠運行的 ASF 最新穩定版本。

從在突出顯示為紅色的字段填入機械人的名稱開始。 這可以是您想要使用的任何名稱，例如您的昵稱、帳戶名、號碼或任何其他名稱。 只有一個詞不能使用，`ASF`，因為該關鍵字是為全域設定檔保留的。 除此之外，您的機械人名稱不能以點開頭（ASF會忽略這些檔）。 我們還建議您避免使用空格，如果需要，可以使用 `_` 作為單詞分隔符號。

在確定名稱後，開啓` Enabled `開關，這將定義 ASF 是否應該在啟動（程序之後）自動啟動您的機械人 。

現在，您可以決定兩件事：

- 您可以在` SteamLogin `字段中填入您的登錄帳號，並在` SteamPassword `字段中填入您的登錄密碼
- 或者您可以讓它們空著

前者將允許 ASF 在啟動期間自動使用您的帳戶憑據，使您不必在每次 ASF 需要時手動輸入它們。 但是，您可以決定省略它們，在這種情況下它們不會被保存，因此 ASF 將無法在沒有您幫助的情況下自動啟動，您需要在運行時輸入它們。

ASF 需要您的登錄憑據，因為它使用內置的Steam用戶端實現，並且需要與您自己使用的用戶端相同的詳細資訊登錄。 Your login credentials are not saved anywhere but on your PC in ASF `config` directory only, our web config generator is client-based which means that the code is run locally in your browser to generate valid ASF configs, without details you're inputting ever leaving your PC in the first place, so there is no need to worry about any possible sensitive data leak. Still, if you for whatever reason don't want to put your credentials there, we understand that, and you can put them manually later in generated files, or omit them entirely and put them only in ASF command prompt. More on security matter can be found in **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** section.

You can also decide to leave just one field empty, such as `SteamPassword`, ASF will then be able to use your login automatically, but will still ask for password (similar to Steam Client). If you're using Steam parental to unlock the account, you'll need to put it into `SteamParentalCode` field.

After the decision and optional details, your web page will now look similar to the one below:

![Bot tab 2](https://i.imgur.com/yf54Ouc.png)

You can now hit "download" button and our web config generator will generate new `json` file based on your chosen name. Save that file into `config` directory of ASF. You can use previously-created `config` shortcut, or find `config` directory manually, directly in ASF file structure.

Your `config` directory will now look like this:

![架構 2](https://i.imgur.com/crWdjcp.png)

恭喜！ 您剛剛完成了 ASF 機械人的基礎配置。 我們很快就會對此進行擴展説明，現在這就是您需要的一切。

* * *

### 運行 ASF

您現在已準備好首次啟動該程序。 Simply double-click ASF shortcut, or `ArchiSteamFarm(.exe)` binary in ASF directory.

After doing so, assuming you installed all required dependencies in the first step, ASF should launch properly, notice your first bot (if you didn't forget to put generated config in `config` directory), and attempt to log in:

![ASF](https://i.imgur.com/u5hrSFz.png)

If you supplied `SteamLogin` and `SteamPassword` for ASF to use, you'll be asked for your SteamGuard token only (e-mail, 2FA or none, depending on your Steam settings). If you didn't, you'll also be asked for your Steam login and password.

Now is a good time to review our **[privacy policy](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Statistics#current-privacy-policy)** section if you're concerned about what will happen next, as stated by ASF itself.

After passing through initial login gate, assuming your details are correct, you'll successfully log in, and ASF will start idling using default settings that you didn't change as of now:

![ASF 2](https://i.imgur.com/Cb7DBl4.png)

This proves that ASF is now successfully doing its job on your account, so you can now minimize the program and do something else. After enough of time (depending on **[performance](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)**), you'll see Steam trading cards slowly being dropped. Of course, for that to happen you must have valid games to idle, showing as "you can get X more card drops from playing this game" on your **[badges page](https://steamcommunity.com/my/badges)** - if there are no games to idle, then ASF will state that there is nothing to do, as stated in our **[FAQ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ#so-how-it-exactly-works)**.

This concludes our very basic setting up guide. You can now decide whether you want to configure ASF further, or let it do its job in default settings. We'll cover a few more basic details, then leave you entire wiki for discovery.

* * *

### 擴展配置

#### 同時讓多個帳戶掛卡

ASF supports idling more than one account at a time, which is its primary function. You can add more accounts to ASF by generating more bot config files, in exactly the same way as you've generated your first one just a few minutes ago. You need to ensure only two things:

- Unique bot name, if you already have your first bot named "MainAccount", you can't have another one with the same name.
- Valid login details, such as `SteamLogin`, `SteamPassword` and `SteamParentalCode` (if using Steam parental settings)

In other words, simply jump to configuration again and do exactly the same, just for your second or third account. Remember to use unique names for all of your bots.

* * *

#### 變更設定

You change existing settings in exactly the same way - by generating a new config file. If you didn't close our web config generator yet, click on "toggle advanced settings" and see what is there for you to discover. For this tutorial we'll change `CustomGamePlayedWhileFarming` setting, which allows you to set custom name being displayed when ASF is idling, instead of showing actual game.

So let's do that, if you run ASF and start idling, in default settings you'll see that your Steam account is in-game now:

![Steam](https://i.imgur.com/1VCDrGC.png)

Let's change that then. Toggle advanced settings in web config generator and find `CustomGamePlayedWhileFarming`. Once you do that, put your own custom text there that you want to display, such as "Idling cards":

![Bot tab 3](https://i.imgur.com/gHqdEqb.png)

Now download the new config file in exactly the same way, then **overwrite** your old config file with new one. You can also delete your old config file and put new one in its place of course.

Once you do that and start ASF again, you'll notice that ASF now displays your custom text in previous place:

![Steam 2](https://i.imgur.com/vZg0G8P.png)

This confirms that you've successfully edited your config. In exactly the same way you can change global ASF properties, by switching from bot tab to "ASF" tab, downloading generated `ASF.json` config file and putting it in your `config` directory.

* * *

#### 使用 ASF-ui

ASF is a console app and doesn't include a graphical user interface. 但是，我們正積極致力於**[ ASF-ui ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#asf-ui)**我們的 IPC 前端接口，它將以合適且對用戶友好的方式來訪問各種 ASF 功能。

In order to use ASF-ui, you should ensure that you set up `IPC` and `SteamOwnerID` global configuration properties (ASF tab).

For `SteamOwnerID`, you need to input unique Steam identificator in 64-bit form of your account. You can look it up in various different ways, we'll use **[SteamRep](https://steamrep.com)** for that purpose. Open the website, locate sign in through Steam button in top right corner, then log in. Afterwards, in the same place, click on your avatar, and look up `steamID64` field on your profile.

![SteamRep](https://i.imgur.com/RUuJ63i.png)

For my account, this is `76561198006963719` number. You'll have a similar one, also starting from `7656`. Copy it.

Now navigate once again to our web config generator and input that number as SteamOwnerID.

![SteamOwnerID](https://i.imgur.com/V6jslfQ.png)

You need to do only one more thing, toggle advanced settings, find `IPC` option, and enable it.

![IPC](https://i.imgur.com/NhujZCN.png)

Now you can download your ASF config and put it in your `config` directory, as usual. Afterwards, launch ASF again, and you should be able to confirm that it properly started IPC interface:

![IPC 2](https://i.imgur.com/ZmkO8pk.png)

If you did everything properly, you'll now be able to access ASF's IPC interface under **[this](http://127.0.0.1:1242)** link, as long as ASF is running. You can use ASF-ui for various purposes, e.g. sending **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. Feel free to take a look around in order to find out all ASF-ui functionalities.

![ASF-ui](https://raw.githubusercontent.com/JustArchiNET/ASF-ui/master/.github/previews/bots.png)

Please note that ASF-ui is currently in preview state and not everything is available/working yet, but it's more than enough for simple ASF usage.

* * *

### 概要

You've successfully set up ASF to use your Steam accounts and you've already customized it to your liking a little. If you followed our entire guide, then you even managed to send a simple command through our ASF-ui interface. Now is a good time to read our entire **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** section in order to learn what all those different settings you've seen in advanced tab actually do, and what ASF can offer. If you've stumbled upon some issue or you have some generic question, read **[FAQ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ)** instead which should cover all, or at least majority of questions that you might have. If you want to learn everything about ASF and how it can make your life easier, head over to the rest of **[our wiki](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Home)**. Have fun!

* * *

## 通用設定

This setup is for advanced users that want to set up ASF to run in **[generic](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#generic)** variant. It's not recommended for people that can use **[OS-specific setup](#os-specific-setup)**.

You want to use generic variant mainly in three situations (but of course you can use it regardless):

- When you're using OS that we don't build OS-specific package for (such as 32-bit Windows)
- When you already have .NET Core Runtime/SDK, or want to install and use one
- When you want to minimize ASF structure size by handling runtime requirements yourself

However, keep in mind that you're in charge of .NET Core runtime in this case. This means that if your .NET Core SDK (runtime) is unavailable, outdated or broken, ASF won't work. This is why we don't recommend this setup for casual users, since you now need to ensure that your .NET Core SDK (runtime) matches ASF requirements and can run ASF, as opposed to **us** ensuring that our .NET Core runtime bundled with ASF can do so.

For generic package, you can follow entire OS-specific guide above, with two small changes. In addition to installing .NET Core prerequisites, you also want to install .NET Core SDK, and instead of having OS-specific `ArchiSteamFarm(.exe)` executable file, you now have a generic `ArchiSteamFarm.dll` binary only. Everything else is exactly the same.

額外步驟：

- 安裝 **[.NET 核心套件](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)**。
- Install **[.NET Core SDK](https://www.microsoft.com/net/download)** (or at least runtime) appropriate for your OS. You most likely want to use an installer. Refer to **[runtime requirements](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)** if you're not sure which version to install.
- Download **[latest ASF release](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** in generic variant.
- Extract the archive into new location (and `chmod +x ArchiSteamFarm.sh` if you're on Linux/OS X).
- **[配置ASF](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)**。
- Launch ASF by either using a helper script or executing `dotnet /path/to/ArchiSteamFarm.dll` manually from your favourite shell.

Helper scripts (such as `ArchiSteamFarm.cmd` for Windows and `ArchiSteamFarm.sh` for Linux/OS X) are located next to `ArchiSteamFarm.dll` binary - those are included in generic variant only. You can use them if you don't want to execute `dotnet` command manually. You can also make a shortcut to those scripts like showed above, since they're supposed to provide binary replacement in a script way. Obviously helper scripts won't work if you didn't install .NET Core SDK and you don't have `dotnet` executable available in your `PATH`. Helper scripts are entirely optional to use, you can always `dotnet /path/to/ArchiSteamFarm.dll` manually.