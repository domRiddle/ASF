# 編譯

編譯是創建可執行檔的過程。 如果您想將自己的更改添加到ASF，或者出於任何原因不信任官方 **[releases](https://github.com/JustArchiNET/ArchiSteamFarm/releases)** 中提供的可執行檔，則需要執行此操作。 如果您是普通用戶而不是開發人員，則很可能需要使用已預編譯的二進位檔案，但如果您希望使用自己的二進位檔案，或學習新內容，請繼續閱讀。

只要您擁有所有需要的工具， 即可以在當前支援的任何平台上編譯ASF。

---

## .NET SDK

Regardless of platform, you need full .NET SDK (not just runtime) in order to compile ASF. Installation instructions can be found on **[.NET download page](https://dotnet.microsoft.com/download)**. You need to install appropriate .NET SDK version for your OS. 成功安裝後，`dotnet` 命令應可正常運行。 您可以驗證它是否適用于 < 0>dotnet-info</0 >。 Also ensure that your .NET SDK matches ASF **[runtime requirements](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)**.

---

## 編譯

Assuming you have .NET SDK operative and in appropriate version, simply navigate to source ASF directory (cloned or downloaded and unpacked ASF repo) and execute:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net5.0" -o "out/generic"
```

如果您在使用 Linux/OS X，您也可以使用 `cc.sh` 腳本實現同樣的效果，此種編譯方法方式稍複雜。

If compilation ended successfully, you can find your ASF in `source` flavour in `out/generic` directory. This is the same as official `generic` ASF build, but it has forced `UpdateChannel` and `UpdatePeriod` of `0`, which is appropriate for self-builds.

### OS-specific

You can also generate OS-specific .NET package if you have a specific need. In general you shouldn't do that because you've just compiled `generic` flavour that you can run with your already-installed .NET runtime that you've used for the compilation in the first place, but just in case you want to:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net5.0" -o "out/linux-x64" -r "linux-x64"
```

當然，您需要將 `linux-x64` 替換成您需要的目標操作系統架構，例如 `win-x64`。 這一構建也將禁用自動更新。

### .NET 框架

In a very rare case when you'd want to build `generic-netf` package, you can change target framework from `net5.0` to `net48`. Keep in mind that you'll need appropriate **[.NET Framework](https://dotnet.microsoft.com/download/visual-studio-sdks)** developer pack for compiling `netf` variant, in addition to .NET SDK, so the below will work only on Windows:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net48" -o "out/generic-netf"
```

In case of being unable to install .NET Framework or even .NET SDK itself (e.g. because of building on `linux-x86` with `mono`), you can call `msbuild` directly. You'll also need to specify `ASFNetFramework` manually, as ASF by default disables `netf` build on non-Windows platforms:

```shell
msbuild /m /r /t:Publish /p:Configuration=Release /p:TargetFramework=net48 /p:PublishDir=out/generic-netf /p:ASFNetFramework=true ArchiSteamFarm
```

---

## 開發

If you'd like to edit ASF code, you can use any .NET compatible IDE for that purpose, although even that is optional, since you can as well edit with a notepad and compile with `dotnet` command described above. 不過，對於 Windows 系統，我們推薦使用​**[最新版本的 Visual Studio](https://visualstudio.microsoft.com/downloads)**（免費的社區版即可）。

如果您要在 Linux/OS X 上開發 ASF 代碼，我們推薦使用​**[最新版的 Visual Studio Code](https://code.visualstudio.com/download)**。 它沒有經典的 Visual Studio 那麼豐富的功能，但已足夠了。

當然，以上的所有建議都僅僅是建議，您可以使用您想用的任何工具，最後您都要使用 `dotnet build` 命令進行構建。 We use **[JetBrains Rider](https://www.jetbrains.com/rider)** for ASF development, although it's not a free solution.

---

## 標籤

`main` branch is not guaranteed to be in a state that allows successful compilation or flawless ASF execution in the first place, since it's development branch just like stated in our **[release cycle](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle)**. If you want to compile or reference ASF from source, then you should use appropriate **[tag](https://github.com/JustArchiNET/ArchiSteamFarm/tags)** for that purpose, which guarantees at least successful compilation, and very likely also flawless execution (if build was marked as stable release). In order to check the current "health" of the tree, you can use our continuous integrations - **[GitHub](https://github.com/JustArchiNET/ArchiSteamFarm/actions)** or **[AppVeyor](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)**.

---

## 官方發佈版本

Official ASF releases are compiled by **[GitHub](https://github.com/JustArchiNET/ArchiSteamFarm/actions)** on Windows, with latest .NET SDK that matches ASF **[runtime requirements](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)**. After passing tests, all packages are deployed as the release, also on GitHub. This also guarantees transparency, since GitHub always uses official public source for all builds, and you can compare checksums of GitHub artifacts with GitHub release assets. 除了私人的開發和調試過程外，ASF 開發人員不會自行編譯或發佈構建版本。