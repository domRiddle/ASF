# 編譯

編譯是生成執行檔的過程。 若您想將自己的修改加入至ASF中，或出於任何原因不信任官方&#8203;**[發布頁面](https://github.com/JustArchiNET/ArchiSteamFarm/releases)**&#8203;中提供的執行檔，就需要做這件事。 如果您是一般的使用者而不是開發人員，則您很有可能需要使用已預編譯的二進制檔案，但若您希望使用自己的二進制檔案或學習新內容，請繼續閱讀。

只要您擁有所有需要的工具，就可以在任何當前支援的平台上編譯ASF。

---

## .NET SDK

不論使用何種平台，您都需要完整的.NET SDK（不只是執行環境）才能編譯ASF。 可以在&#8203;**[.NET下載頁面](https://dotnet.microsoft.com/download)**&#8203;上找到安裝說明。 您需要安裝適合您作業系統的.NET SDK版本。 成功安裝後，&#8203;`dotnet`&#8203;命令應可正常執行。 您可以使用&#8203;`dotnet --info`&#8203;來驗證。 也要確保您的.NET SDK符合ASF的&#8203;**[執行環境需求](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-zh-TW#執行環境需求)**&#8203;。

---

## 編譯

假設您已擁有適合的.NET SDK版本，只需前往ASF原始碼資料夾（Clone或下載並解壓縮後的ASF資源庫）並執行：

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net6.0" -o "out/generic"
```

若您使用Linux/macOS，則可以改用&#8203;`cc.sh`&#8203;腳本，以稍複雜的方法執行相同的操作。

若編譯成功完成，您可以在&#8203;`out/generic`&#8203;資料夾中找到您的ASF &#8203;`source`&#8203;套件。 這與ASF官方的&#8203;`Generic`&#8203;建置版本相同，但因為這是您自己建置的，所以它會強制設定&#8203;`UpdateChannel`&#8203;和&#8203;`UpdatePeriod`&#8203;為&#8203;`0`&#8203;。

### 適用於特定作業系統

若您有需要，也可以生成適用於特定作業系統的.NET套件。 一般而言，不需要這樣做，因為您剛剛編譯了&#8203;`generic`&#8203;版本，您可以使用您已安裝用於編譯的.NET執行環境來執行，但以防萬一您想要：

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net6.0" -o "out/linux-x64" -r "linux-x64"
```

當然，您需要將&#8203;`linux-x64`&#8203;替代成您所需目標的作業系統架構，例如&#8203;`win-x64`&#8203;。 這一建置版本也將停用自動更新。

### .NET Framework

在極少見的情形下，若您想要建置&#8203;`generic-netf`&#8203;套件，您可以將目標框架從&#8203;`net6.0`&#8203;更改成&#8203;`net48`&#8203;。 請注意，您需要適合的&#8203;**[.NET Framework](https://dotnet.microsoft.com/download/visual-studio-sdks)**&#8203;開發人員套件與.NET SDK才能編譯&#8203;`netf`&#8203;變體版本，所以下列只適用於Windows：

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net48" -o "out/generic-netf"
```

若無法安裝.NET Framework或甚至.NET SDK自身（例如在&#8203;`linux-x86`&#8203;上使用`mono`建置版本），您可以直接呼叫&#8203;`msbuild`&#8203;。 您還需要手動指定&#8203;`ASFNetFramework`&#8203;，因為ASF在非Windows平台上預設停用&#8203;`netf`&#8203;建置版本：

```shell
msbuild /m /r /t:Publish /p:Configuration=Release /p:TargetFramework=net48 /p:PublishDir=out/generic-netf /p:ASFNetFramework=true ArchiSteamFarm
```

### ASF-ui

雖然上述是建置完整的ASF需要的所有步驟，但您可能&#8203;*也*&#8203;有興趣了解如何建置我們的Web使用者介面：&#8203;**[ASF-ui](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-zh-TW#asf-ui)**&#8203;。 從ASF的角度來說，您需要做的是將ASF-ui建置版本輸出放到標準&#8203;`ASF-ui/dist`&#8203;位置，然後讓它與ASF一起建置（若需要）。

ASF-ui作為&#8203;**[Git Submodule](https://git-scm.com/book/en/v2/Git-Tools-Submodules)**&#8203;，是ASF Source Tree的一部份，請確保您已使用&#8203;`git clone --recursive`&#8203;來複製資源庫，否則您會缺失必要檔案。 您還必須擁有可用的NPM，&#8203;**[Node.js](https://nodejs.org)**&#8203;有自帶它。 若您使用Linux/macOS，我們建議使用我們的&#8203;`cc.sh`&#8203;腳本，它將自動建置並搭載ASF-ui（如果可能，也就是說，若您滿足我們剛剛提到的需求）。

除了&#8203;`cc.sh`&#8203;腳本，我們也在下文附上簡明建置說明，請參閱&#8203;**[ASF-ui資源庫](https://github.com/JustArchiNET/ASF-ui)**&#8203;以獲得更多說明文件。 從ASF的Source Tree位置，同上所述，執行以下命令：

```shell
rm -rf "ASF-ui/dist" # ASF-ui不會自行清除舊建置版本

npm ci --prefix ASF-ui
npm run-script deploy --prefix ASF-ui

rm -rf "out/generic/www" # 確保我們的建置輸出不會含有舊檔案
dotnet publish ArchiSteamFarm -c "Release" -f "net6.0" -o "out/generic" # 或依據上文選擇您所需的
```

您現在應該可以在&#8203;`out/generic/www`&#8203;資料夾中找到ASF-ui檔案了。 ASF能向您的瀏覽器伺服這些檔案。

或者，您也可以直接建置ASF-ui，不論是手動或是透過我們的資源庫的幫助下，再手動複製建置輸出至&#8203;`${OUT}/www`&#8203;資料夾中，其中&#8203;`${OUT}`&#8203;是您使用&#8203;`-o`&#8203;參數指定的ASF輸出資料夾。 這正是ASF在建置過程中所做的，它將&#8203;`ASF-ui/dist`&#8203;（若存在）複製到&#8203;`${OUT}/www`&#8203;，沒什麼特別的。

---

## 開發

If you'd like to edit ASF code, you can use any .NET compatible IDE for that purpose, although even that is optional, since you can as well edit with a notepad and compile with `dotnet` command described above. 不過，對於 Windows 系統，我們推薦使用​**[最新版本的 Visual Studio](https://visualstudio.microsoft.com/downloads)**（免費的社區版即可）。

If you'd like to work with ASF code on Linux/macOS instead, we recommend **[latest Visual Studio Code](https://code.visualstudio.com/download)**. 它沒有經典的 Visual Studio 那麼豐富的功能，但已足夠了。

當然，以上的所有建議都僅僅是建議，您可以使用您想用的任何工具，最後您都要使用 `dotnet build` 指令進行構建。 We use **[JetBrains Rider](https://www.jetbrains.com/rider)** for ASF development, although it's not a free solution.

---

## 標籤

`main` branch is not guaranteed to be in a state that allows successful compilation or flawless ASF execution in the first place, since it's development branch just like stated in our **[release cycle](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle)**. 如果您希望從原始碼編譯或參照 ASF，就應該為此選擇適當的​**[標籤](https://github.com/JustArchiNET/ArchiSteamFarm/tags)**，這樣能夠保證編譯成功，甚至可以正常運行（如果您選擇穩定版）。 In order to check the current "health" of the tree, you can use our CI - **[GitHub](https://github.com/JustArchiNET/ArchiSteamFarm/actions/workflows/ci.yml?query=branch%3Amain)**.

---

## 官方發布版本

Official ASF releases are compiled by **[GitHub](https://github.com/JustArchiNET/ArchiSteamFarm/actions)** on Windows, with latest .NET SDK that matches ASF **[runtime requirements](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)**. After passing tests, all packages are deployed as the release, also on GitHub. This also guarantees transparency, since GitHub always uses official public source for all builds, and you can compare checksums of GitHub artifacts with GitHub release assets. 除了私人的開發和調試過程外，ASF 開發人員不會自行編譯或發佈構建版本。

Starting from ASF V5.2.0.5, in addition to the above, ASF maintainers manually validate and publish build checksums on independent from GitHub, remote server, as additional security measure. This step is mandatory for existing ASFs to consider the release as a valid candidate for auto-update functionality.