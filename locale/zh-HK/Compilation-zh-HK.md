# 編譯

編譯是創建可執行檔的過程。 如果您想將自己的更改添加到ASF，或者出於任何原因不信任官方 **[releases](https://github.com/JustArchiNET/ArchiSteamFarm/releases)** 中提供的可執行檔，則需要執行此操作。 如果您是普通用戶而不是開發人員，則很可能需要使用已預編譯的二進位檔案，但如果您希望使用自己的二進位檔案，或學習新內容，請繼續閱讀。

只要您擁有所有需要的工具， 即可以在當前支援的任何平台上編譯ASF。

* * *

## .NET Core SDK

無論使用什麼平台，您都需要完整的 .NET Core SDK（不僅僅是運行時環境）才能編譯 ASF。 您可以在 **[.NET Core 安裝頁面​](https://dotnet.microsoft.com/download)**找到安裝指南。 您需要為您的操作系統安裝相應的.NET Core SDK版本。 成功安裝後，`dotnet` 命令應可正常運行。 您可以驗證它是否適用于 < 0>dotnet-info</0 >。 同樣需要確認您的 .NET Core SDK 匹配 ASF 的​**[運行時環境需求](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)**。

* * *

## 編譯

如果您有合適版本的 .NET Core SDK，只需導航到源 ASF 目錄（克隆或下載並解壓縮 ASF 存儲庫），然後執行：

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net5.0" -o "out/generic"
```

如果您在使用 Linux/OS X，您也可以使用 `cc.sh` 腳本實現同樣的效果，此種編譯方法方式稍複雜。

If compilation ended successfully, you can find your ASF in `source` flavour in `out/generic` directory. This is the same as official `generic` ASF build, but it has forced `UpdateChannel` and `UpdatePeriod` of `0`, which is appropriate for self-builds.

### OS-specific

如果您需要，也可以生成特定操作系統的 .NET Core 包。 In general you shouldn't do that because you've just compiled `generic` flavour that you can run with your already-installed .NET Core runtime that you've used for the compilation in the first place, but just in case you want to:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net5.0" -o "out/linux-x64" -r "linux-x64"
```

當然，您需要將 `linux-x64` 替換成您需要的目標操作系統架構，例如 `win-x64`。 這一構建也將禁用自動更新。

### .NET 框架

In a very rare case when you'd want to build `generic-netf` package, you can change target framework from `net5.0` to `net48`. 請注意，您需要合適的 **[.NET 框架](https://dotnet.microsoft.com/download/visual-studio-sdks)**​開發者工具包和 .NET Core SDK 才能編譯 `netf` 包，所以此命令僅適用於 Windows：

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net48" -o "out/generic-netf"
```

在無法安裝 .NET 框架甚至 .NET Core SDK 本身的情況下（例如在 `linux-x86` 平台用 `mono` 構建），可以直接調用 `msbuild`。 You'll also need to specify `ASFNetFramework` manually, as ASF by default disables `netf` build on non-Windows platforms:

```shell
msbuild /m /r /t:Publish /p:Configuration=Release /p:TargetFramework=net48 /p:PublishDir=out/generic-netf /p:ASFNetFramework=true ArchiSteamFarm
```

* * *

## 開發

如果您想要編輯 ASF 代碼，您可以使用任何相容 .NET Core 的 IDE，但這也是可選的，因為您甚至可以用記事本編輯代碼並用上述 `dotnet` 指令編譯。 不過，對於 Windows 系統，我們推薦使用​**[最新版本的 Visual Studio](https://visualstudio.microsoft.com/downloads)**（免費的社區版即可）。

如果您要在 Linux/OS X 上開發 ASF 代碼，我們推薦使用​**[最新版的 Visual Studio Code](https://code.visualstudio.com/download)**。 它沒有經典的 Visual Studio 那麼豐富的功能，但已足夠了。

當然，以上的所有建議都僅僅是建議，您可以使用您想用的任何工具，最後您都要使用 `dotnet build` 命令進行構建。 We use **[JetBrains Rider](https://www.jetbrains.com/rider)** for ASF development, with a small part of third-party `tools` that you can find in the repo.

* * *

## 標籤

`master` 分支並不保證能夠成功編譯或者正常運行 ASF，正如我們在​**[發佈周期](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle)**​中所述，這是一個開發分支。 If you want to compile or reference ASF from source, then you should use appropriate **[tag](https://github.com/JustArchiNET/ArchiSteamFarm/tags)** for that purpose, which guarantees at least successful compilation, and very likely also flawless execution (if build was marked as stable release). In order to check the current "health" of the tree, you can use our continuous integrations - **[GitHub](https://github.com/JustArchiNET/ArchiSteamFarm/actions)**, **[AppVeyor](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)** or **[Travis](https://travis-ci.com/JustArchiNET/ArchiSteamFarm)**.

* * *

## 官方發佈版本

Official ASF releases are compiled by **[GitHub](https://github.com/JustArchiNET/ArchiSteamFarm/actions)** on Windows, with latest .NET Core SDK that matches ASF **[runtime requirements](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)**. After passing tests, all packages are deployed as the release, also on GitHub. This also guarantees transparency, since GitHub always uses official public source for all builds, and you can compare checksums of GitHub artifacts with GitHub release assets. 除了私人的開發和調試過程外，ASF 開發人員不會自行編譯或發佈構建版本。