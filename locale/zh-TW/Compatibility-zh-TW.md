# 相容性

ASF是一個在.NET Core平台上執行的C#應用程式。 這代表ASF並不是被編譯成可供CPU直接執行的&#8203;**[機器語言](https://zh.wikipedia.org/wiki/%E6%9C%BA%E5%99%A8%E8%AF%AD%E8%A8%80)**&#8203;，而是被編譯成&#8203;**[通用中間語言](https://en.wikipedia.org/wiki/Common_Intermediate_Language)**&#8203;（CIL），一種需要對應的執行環境才能執行的語言。

這種方法具有巨大的優勢，因為CIL獨立於平台，這就是為什麼ASF能夠天然地在許多作業系統上執行的原因，特別是Windows、Linux與macOS。 不僅不需要模擬，同時也支援所有平台相關及硬體相關的最佳化，例如CPU SSE指令。 因此，ASF在表現卓越的效能及最佳化時，同時仍能提供完美的相容性與可靠性。

這也代表執行ASF&#8203;**沒有特定的作業系統需求**&#8203;，因為它需要的只是執行於作業系統上的&#8203;**執行環境**&#8203;，而非作業系統本身。 只要在執行期間正確執行ASF程式碼，底層的作業系統無論是Windows、Linux、macOS、BSD、Sony Playstation 4、Nintendo Wii，或是您的烤麵包機上，都無所謂。只要有相應的&#8203;**[.NET](https://dotnet.microsoft.com/download/dotnet)**&#8203;，就能執行&#8203;**[ASF](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)**&#8203;。

然而，不論您想要在哪個平台上執行ASF，您都需要確保該平台安裝了&#8203;**[.NET需求套件](https://github.com/dotnet/core/blob/main/Documentation/prereqs.md)**&#8203;。 這些都是確保執行環境功能正常所需的底層函式庫，也是ASF第一時間運作的絕對核心。 很有可能您已安裝了其中的一些（甚至全部）。

---

## ASF 封裝套件

ASF有兩種封裝方式：通用（Generic）套件與特定作業系統（OS-specific）套件。 從功能上來說，這兩種套件是完全相同的，都能夠自動進行更新。 唯一的區別就是&#8203;**Generic**&#8203;套件中，不包含能使ASF直接執行**適用於您的作業系統的**執行環境。

---

### Generic

Generic套件與平台無關，所以它不包含任何給特定設備的程式碼。 這個版本需要您在作業系統中已安裝&#8203;**適合版本**&#8203;的.NET執行環境。 我們都知道讓相依套件一直保持在最新是多麼麻煩的一件事，因此這個套件主要是為那些&#8203;**已在使用**&#8203;.NET，知道如何使用已安裝的程式，且不想為了ASF單獨複製執行環境的人提供的。 Generic套件還可以使您在&#8203;**任何可以獲得.NET執行環境的的地方**&#8203;執行ASF，無論是否存在適用於您的作業系統的ASF建置版本。

對於一般或甚至是進階的使用者，如果只想執行ASF而不想鑽研.NET的技術細節，則不建議使用Generic版本。 也就是說，若您瞭解Generic套件，那您就能使用它，否則最好使用下面介紹的適用於您的作業系統的套件。

#### .NET Framework 套件

除了上面所提到的Generic套件外，我們也有&#8203;`Generic-netf`&#8203;套件，它是建立於.NET Framework而不是.NET（Core）之上。 這個套件是個舊的變體版本，它提供了從ASF V2時代開始缺少的相容性，且可以執行於例如&#8203;**[Mono](https://www.mono-project.com)**&#8203;之上，而.NET的&#8203;`Generic`&#8203;套件則無法使用。

在一般情形下，您應該要&#8203;**盡量避免使用此套件**&#8203;，因為大多數作業系統都完全（並且更好地）支援上面所提到的&#8203;`Generic`&#8203;套件。 事實上，這個套件只適用於缺少.NET執行環境，但同時具有Mono實作的平台上。 此類平台的範例包含&#8203;`linux-x86`&#8203;（32位元i386/i686 Linux）與&#8203;`linux-armel`&#8203;（例如在Raspberry Pi 0 & 1開發版中使用的ARMv6），到目前為止，這些平台都沒有官方的.NET執行環境。

隨著時間的推移，.NET支援的平台會越來越多，.NET Framework與.NET間的相容性會越來越差，而&#8203;`Generic-netf`&#8203;套件將會在未來完全被&#8203;`Generic`&#8203;套件所取代。 如果您可以使用任何.NET套件，請不要使用它，因為與.NET版本相比，&#8203;`Generic-netf`&#8203;缺少很多功能及相容性，而且它的功能會隨時間減少。 我們&#8203;**只會**&#8203;對無法使用&#8203;`Generic`&#8203;套件的平台提供此版本的支援（例如&#8203;`linux-x86`&#8203;），並且也只會支援基於最新版本的執行環境（例如最新版Mono）。

---

### 適用於特定作業系統

除了Generic套件中包含的受控代碼外，適用於特定作業系統的套件還包含指定平台的本機碼。 也就是說，適用於特定作業系統的套件&#8203;**已經在裡面包含了正確的.NET 執行環境**&#8203;，它可以使您完全跳過整個麻煩的安裝過程，直接啟動ASF。 適用於特定作業系統的套件，顧名思義，是針對不同作業系統的，每種作業系統都需要它自己特定的版本：例如Windows需要PE32+ &#8203;`ArchiSteamFarm.exe`&#8203;二進制檔案，而Linux則需要Unix ELF &#8203;`ArchiSteamFarm`&#8203;二進制檔案。 如您所知，這兩種類型彼此不相容。

ASF目前擁有以下特定作業系統的變體版本：

- `linux-arm`&#8203;支援32位元基於ARM（ARMv7+）的GNU/Linux作業系統。 這包含例如Raspberry Pi 2（或更新版本）等的平台，以及所有在當前和未來版本中可用的GNU/Linux作業系統（例如Raspberry Pi OS）。 這個變體版本&#8203;**並不**&#8203;支援較舊的ARM架構，例如Raspberry Pi 0中的ARMv6 & 1，它也不適用於未實作所需GNU/Linux環境的作業系統（例如Android）。
- `linux-arm64`&#8203;支援64 位元基於ARM（ARMv8+）的GNU/Linux作業系統。 這包含例如Raspberry Pi 3（或更新版本）等的平台，以及所有在當前和未來版本中可用的AArch64 GNU/Linux作業系統（例如Debian）。 這個變體版本&#8203;**並不**&#8203;支援沒有所需64位元函式庫的32位元作業系統（例如Raspberry Pi OS），它也不適用於未實作所需GNU/Linux環境的作業系統（例如Android）。
- `linux-x64`&#8203;支援64位元的GNU/Linux作業系統。 這包含Alpine、CentOS/Fedora/RHEL、Debian/Ubuntu、OpenSUSE/SLES等很多作業系統，包含它們在當前和未來的版本中的衍生版本。
- `osx-x64`&#8203;支援64位元基於ARM（Apple silicon）的macOS作業系統。 包含版本11及其更新版本。
- `osx-x64`&#8203;支援64位元的macOS作業系統。 包含版本10.15及其更新版本。
- `win-x64`&#8203;支援64位元的Windows作業系統。 This includes 10, 11, Server 2012+ as well as future versions.

當然，即使沒有適合您作業系統及架構的特定作業系統套件，您也可以手動安裝適當的.NET Core執行環境並執行Generic ASF套件，這也是這個套件存在的主要原因。 Generic ASF套件與平台無關，可以在任何具有可用.NET Core執行環境的平台上執行。 需要注意：ASF需要的是.NET Core執行環境，而不是特定的作業系統或架構。 例如，如果您使用的是32位元Windows，但ASF沒有&#8203;`win-x86`&#8203;版本，您仍然可以安裝&#8203;`win-x86`&#8203;版本的.NET Core SDK，然後執行Generic版本的ASF。 我們無法為所有作業系統及架構組合都產生一份執行檔，所以我們為此畫下一道分隔線。 x86就是這條線的其中之一，因為這種架構從2004年開始就過時了。

您可以造訪​&#8203;**[發行說明​](https://github.com/dotnet/core/blob/main/release-notes/7.0/supported-os.md)**&#8203;來查看.NET Core 7.0支援的平台與作業系統的完整清單。

---

## 執行環境需求

若您使用適用於特定作業系統的套件，那麼您不必擔心執行環境的需求，因為ASF總會將所需的最新執行環境封裝在一起，只要您已安裝並更新&#8203;**[.NET Core需求套件](https://github.com/dotnet/core/blob/main/Documentation/prereqs.md)**&#8203;，就能夠正常執行。 也就是說，&#8203;**您不需要安裝.NET Core執行環境或SDK**&#8203;，因為特定作業系統的版本只需要本機安裝對應作業系統的相依套件（需求套件），而不需要其他項目。

但是，如果您嘗試執行&#8203;**Generic**&#8203; ASF套件，則必須確保您的.NET執行環境支援ASF所需的平台。

ASF as a program is targeting **.NET 7.0** (`net7.0`) right now, but it may target newer platform in the future. `net7.0` is supported since 7.0.100 SDK (7.0.0 runtime), although ASF is configured to prefer **latest runtime at the moment of compilation**, so you should ensure that you have **[latest SDK](https://dotnet.microsoft.com/download)** (or at least runtime) available for your machine. 若您的執行環境低於編譯期間指定的最低支援版本，Generic ASF變體版本可能會拒絕啟動。

如有疑問，您可以訪問我們用於編譯並在GitHub上部署新版本的&#8203;**[持續整合程序](https://github.com/JustArchiNET/ArchiSteamFarm/actions/workflows/publish.yml?query=branch%3Amain)**&#8203;。 作為.NET驗證步驟的一部分，您可以在每個建置版本中找到&#8203;`dotnet --info`&#8203;輸出。