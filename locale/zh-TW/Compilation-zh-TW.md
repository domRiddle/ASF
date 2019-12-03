# 編譯

編譯是建立執行檔的過程。 如果您想將自己的變更添加到ASF，或者出於任何原因不信任官方 **[releases](https://github.com/JustArchiNET/ArchiSteamFarm/releases)** 中提供的可執行檔，則需要執行此操作。 如果您是使用者而不是開發人員，則很可能需要使用已預編譯的二進位檔案，但如果您希望使用自己的二進位檔案或學習新內容，請繼續閱讀。

只要您擁有所有需要的工具， 即可以在任何當前支援的平台上編譯ASF。

* * *

## .NET Core SDK

無論使用什麼平台，您都需要完整的 .NET Core SDK（不僅僅是執行階段）才能編譯 ASF。 您可以在 **[.NET Core 安裝頁面​](https://dotnet.microsoft.com/download)**找到安裝指南。 您需要為您的作業系統安裝相應的.NET Core SDK版本。 成功安裝後，`dotnet` 指令應可正常執行。 您可以執行 `dotnet --info` 指令以驗證。 同樣需要確認您的 .NET Core SDK 符合 ASF 的**[執行階段必要條件](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-zh-TW#執行階段必要條件)**。

* * *

## 編譯

Assuming you have .NET Core SDK operative and in appropriate version, simply navigate to source ASF directory (cloned or downloaded and unpacked ASF repo) and execute:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "netcoreapp3.0" -o "out/generic" "/p:PublishTrimmed=false"
```

如果您在使用 Linux/OS X，您也可以使用 `cc.sh` 腳本實現同樣的效果，此種編譯方法方式稍複雜。

If compilation ended successfully, you can find your ASF in `source` flavour in `out/generic` directory. This is the same as official `generic` ASF build, but it has forced `UpdateChannel` and `UpdatePeriod` of `0`, which is appropriate for self-builds.

### 特定作業系統（OS-specific）

如果您需要，也可以產生特定作業系統的 .NET Core 套件。 In general you shouldn't do that because you've just compiled `generic` flavour that you can run with your already-installed .NET Core runtime that you've used for the compilation in the first place, but just in case you want to:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "netcoreapp3.0" -o "out/linux-x64" -r "linux-x64"
```

Of course, replace `linux-x64` with OS-architecture that you want to target, such as `win-x64`. 這一構建也將停用自動更新。

### .NET Framework

在非常罕見的情況下您會需要建置 `generic-netf` 套件，您可以將目標軟體框架從 `netcoreapp3.0` 變更到 `net48`。 請注意，您需要合適的 **[.NET Framework](https://dotnet.microsoft.com/download/visual-studio-sdks)** ​開發人員工具套件和 .NET Core SDK 才能編譯 `netf` 變體，所以此指令僅適用於 Windows：

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net48" -o "out/generic-netf" "/p:PublishTrimmed=false"
```

在無法安裝 .NET Framework 甚至 .NET Core SDK 本身的情況下（例如在 `linux-x86` 平台用 `mono` 構建），可以直接調用 `msbuild`。 您還需要手動指定 `ASFNetFramework`，因為 ASF 預設禁止在非 Windows 平台上構建 netf：

```shell
msbuild /m /p:Configuration=Release /p:PublishDir=out/generic-netf /p:TargetFramework=net48 /p:ASFNetFramework=true /p:PublishTrimmed=false /r /t:Publish ArchiSteamFarm
```

* * *

## 開發

如果您想要編輯 ASF 代碼，您可以使用任何相容 .NET Core 的 IDE，但這也是可選的，因為您甚至可以用記事本編輯代碼並用上述 `dotnet` 指令編譯。 不過，對於 Windows 系統，我們推薦使用​**[最新版本的 Visual Studio](https://visualstudio.microsoft.com/downloads)**（免費的社區版即可）。 我們還建議您配合使用 **[ReSharper](https://www.jetbrains.com/resharper)**（可選），但它不是一個免費軟體。

如果您要在 Linux/OS X 上開發 ASF 代碼，我們推薦使用​**[最新版的 Visual Studio Code](https://code.visualstudio.com/download)**。 它沒有經典的 Visual Studio 那麼豐富的功能，但已足夠了。

當然，以上的所有建議都僅僅是建議，您可以使用您想用的任何工具，最後您都要使用 `dotnet build` 指令進行構建。 我們使用 Visual Studio + ReSharper 進行 ASF 的開發，也使用了一部分第三方工具，您可以在repo的 `tools` 目錄中找到它們。

* * *

## 標籤

`master` 分支並不保證能夠成功編譯或者正常執行 ASF，正如我們在**[發佈周期](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle-zh-TW)**​中所述，這是一個開發分支。 If you want to compile or reference ASF from source, then you should use appropriate **[tag](https://github.com/JustArchiNET/ArchiSteamFarm/tags)** for that purpose, which guarantees at least successful compilation, and very likely also flawless execution (if build was marked as stable release). 您可以檢查我們的 CI——**[AppVeyor](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)** 或 **[Travis](https://travis-ci.com/JustArchiNET/ArchiSteamFarm)**以瞭解代碼庫的「健康狀態」。

* * *

## 官方發佈版本

官方 ASF 發佈版本由 Windows 上的帶有滿足 ASF **[執行階段必要條件](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-zh-TW#執行階段必要條件)**​的最新版 .NET Core SDK 的 **[AppVeyor](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)** 編譯。 經測試後，所有的套件都會被部署在 GitHub 上。 這也保證了透明度，因為 AppVeyor 總是為所有構建使用官方公共源，並且您可以檢查 AppVeyor 的 Artifacts 與 GitHub 資產的核對和（Checksum）。 除了私人的開發和調試過程外，ASF 開發人員不會自行編譯或發佈構建版本。