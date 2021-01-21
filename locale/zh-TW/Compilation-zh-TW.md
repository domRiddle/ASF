# 編譯

編譯是建立執行檔的過程。 如果您想將自己的變更添加到ASF，或者出於任何原因不信任官方 **[releases](https://github.com/JustArchiNET/ArchiSteamFarm/releases)** 中提供的可執行檔，則需要執行此操作。 如果您是使用者而不是開發人員，則很可能需要使用已預編譯的二進位檔案，但如果您希望使用自己的二進位檔案或學習新內容，請繼續閱讀。

只要您擁有所有需要的工具，即可以在任何當前支援的平台上編譯ASF。

* * *

## .NET Core SDK

無論使用什麼平台，您都需要完整的 .NET Core SDK（不僅僅是執行階段）才能編譯 ASF。 您可以在 **[.NET Core 安裝頁面​](https://dotnet.microsoft.com/download)**找到安裝指南。 您需要為您的作業系統安裝相應的.NET Core SDK版本。 成功安裝後，`dotnet` 指令應可正常執行。 您可以執行 `dotnet --info` 指令以驗證。 同樣需要確認您的 .NET Core SDK 符合 ASF 的**[執行階段必要條件](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-zh-TW#執行階段必要條件)**。

* * *

## 編譯

如果您有合適版本的 .NET Core SDK，只需瀏覽 ASF 原始目錄（複製或下載並解壓縮 ASF 儲存庫），然後執行：

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net5.0" -o "out/generic"
```

如果您在使用 Linux/OS X，您也可以使用 `cc.sh` 腳本實現同樣的效果，此種編譯方法方式稍複雜。

如果編譯成功完成，您可以在 `out/generic` 目錄中找到您的 ASF `source` 套件。 這與 ASF 官方的 `通用（Generic）` 建置相同，但因為這是您自己建置的，所以它強制設定 `UpdateChannel` 和 `UpdatePeriod` 為 `0`。

### 特定作業系統（OS-specific）

如果您需要，也可以為特定作業系統產生 .NET Core 套件。 一般您不需要這樣做，因為您剛剛編譯了`通用`套件，您可以直接使用已安裝用於編譯的 .NET Core 執行階段來執行此套件。如果您還是想要使用特定作業系統套件，請執行：

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net5.0" -o "out/linux-x64" -r "linux-x64"
```

當然，您需要將 `linux-x64` 替換成您需要的目標作業系統架構，例如 `win-x64`。 這一構建也將停用自動更新。

### .NET Framework

In a very rare case when you'd want to build `generic-netf` package, you can change target framework from `net5.0` to `net48`. 請注意，您需要合適的 **[.NET Framework](https://dotnet.microsoft.com/download/visual-studio-sdks)** ​開發人員工具套件和 .NET Core SDK 才能編譯 `netf` 變體，所以此指令僅適用於 Windows：

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net48" -o "out/generic-netf"
```

在無法安裝 .NET Framework 甚至 .NET Core SDK 本身的情況下（例如在 `linux-x86` 平台用 `mono` 構建），可以直接調用 `msbuild`。 You'll also need to specify `ASFNetFramework` manually, as ASF by default disables `netf` build on non-Windows platforms:

```shell
msbuild /m /r /t:Publish /p:Configuration=Release /p:TargetFramework=net48 /p:PublishDir=out/generic-netf /p:ASFNetFramework=true ArchiSteamFarm
```

* * *

## 開發

如果您想要編輯 ASF 程式碼，您可以使用任何與 .NET Core 相容的 IDE，但這也是非必要的，因為您甚至可以用記事本編輯程式碼並用上述 `dotnet` 指令編譯。 不過，對於 Windows 系統，我們推薦使用​**[最新版本的 Visual Studio](https://visualstudio.microsoft.com/downloads)**（免費的社區版即可）。

如果您要在 Linux/OS X 上開發 ASF 程式碼，我們推薦使用​**[最新版的 Visual Studio Code](https://code.visualstudio.com/download)**。 它沒有經典的 Visual Studio 那麼豐富的功能，但已足夠了。

當然，以上的所有建議都僅僅是建議，您可以使用您想用的任何工具，最後您都要使用 `dotnet build` 指令進行構建。 We use **[JetBrains Rider](https://www.jetbrains.com/rider)** for ASF development, with a small part of third-party `tools` that you can find in the repo.

* * *

## 標籤

`main` branch is not guaranteed to be in a state that allows successful compilation or flawless ASF execution in the first place, since it's development branch just like stated in our **[release cycle](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle)**. 如果您希望從原始碼編譯或參照 ASF，就應該為此選擇適當的​**[標籤](https://github.com/JustArchiNET/ArchiSteamFarm/tags)**，這樣能夠保證編譯成功，甚至可以正常運行（如果您選擇穩定版）。 In order to check the current "health" of the tree, you can use our continuous integrations - **[GitHub](https://github.com/JustArchiNET/ArchiSteamFarm/actions)**, **[AppVeyor](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)** or **[Travis](https://travis-ci.com/JustArchiNET/ArchiSteamFarm)**.

* * *

## 官方發佈版本

Official ASF releases are compiled by **[GitHub](https://github.com/JustArchiNET/ArchiSteamFarm/actions)** on Windows, with latest .NET Core SDK that matches ASF **[runtime requirements](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)**. After passing tests, all packages are deployed as the release, also on GitHub. This also guarantees transparency, since GitHub always uses official public source for all builds, and you can compare checksums of GitHub artifacts with GitHub release assets. 除了私人的開發和調試過程外，ASF 開發人員不會自行編譯或發佈構建版本。