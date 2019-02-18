# 相容性

ASF 是一個在.NET Core 平臺上運行的 C# 應用程式。 這意味著 ASF 並非直接被編譯為可供 CPU 執行的​**[機器碼](https://en.wikipedia.org/wiki/Machine_code)**，而是被編譯為 **[通用中間語言](https://en.wikipedia.org/wiki/Common_Intermediate_Language)**，一種需要相應的運行環境才能執行的語言。

這種方法能夠帶來巨大的方便。由於 CIL 是跨平臺的，這使得 ASF 能夠運行在許多作業系統上，特別是 Windows、Linux 和 OS X 這三個系統。ASF 不僅不需要通過模擬運行，同時所有對於系統及其相關硬體的優化也對其有效。 因此，ASF可以實現卓越的性能和優化，同時仍然提供完美的相容性和可靠性。

這也意味著運行 ASF **沒有特定的作業系統要求**，因為它需要的只是運行於作業系統上的**運行環境**而非作業系統本身。 這也意味著運行 ASF 沒有特定的作業系統要求，因為它需要的只是運行於作業系統上的運行環境而非作業系統本身。 只要運行環境能夠正確地執行 ASF 的代碼，底層系統是 Windows、Linux、OS X 還是 BSD，硬體是 Sony Playstation 4、Nintendo Wii甚至是您的烤麵包機都無所謂。只要有**[.NET Core ](https://github.com/dotnet/core-setup#daily-builds)**，就能用 **[ASF](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)**。

但是，無論您想要在哪個平臺上運行 ASF，您必須確保該平臺安裝了**[.NET Core 的依賴項](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)**。 這些都是確保運行環境功能正常的底層庫，也是確保 ASF 能夠第一時間工作的絕對核心。 很有可能你已經安裝了其中的一些 (甚至全部)。

* * *

## ASF 包

ASF comes in 2 main flavours - generic package and OS-specific. 從功能上來講，這兩種包是完全一樣的，都能夠自動進行更新。 唯一的區別就是 **Generic包**中不包含** OS-specific **包內附帶的能使 ASF 運行的環境。

* * *

### Generic

Generic 包與平臺無關，所以它不包含任何特定於電腦的代碼。 所以使用這個包需要您的作業系統中已經安裝有**合適版本**的 .NET Core 運行時環境。 我們都知道保證依賴项始終是最新是十分麻烦的，所以這個包主要面向那些**已經在使用** .NET Core，不想仅仅為了 ASF 對已有環境做單獨備份的人。 Generic 包還允許您將 ASF 運行在**任何拥有正常 .NET Core 環境的機器上**，不需要擔心是否存在相應的 OS-specific 包。

我们并不推薦一般用戶甚至是高級用戶使用 Generic 包，如果您只是想要運行 ASF 而不想要深入瞭解 . NET core 的技術細節。 也就是說，如果你瞭解Generic包，那你可以使用它，不然 OS-specific 包才是更合適的。

#### .NET 框架包

除了上面提到的 Generic 包，我們也提供 `Generic-netf` 包，它基於 .NET 框架（而非 .NET Core）。 該包是一種舊式包，它補全了從 ASF V2 時代即已知的相容性缺失，並且可以使用 **[Mono](https://www.mono-project.com)** 運行，當前來自 .NET Core 的 `Generic` 包無法用於Mono。

通常，您應該**儘量避免使用此程式包**，因為大多數作業系統都完全（並且更好地）支援上面提到的` Generic `包。 事實上，這個軟體包只適用於缺失 .NET Core 運行時環境，但能夠運行 Mono 的平臺。 此類平臺的例子之一是至今仍沒有獲得 .NET Core 運行時環境支援的 `linux-x86` 平臺。

隨著時間的推移，.NET Core 會支援更多平臺，而 .NET Framework 和 .NET Core 之間會更加不相容，`Generic-netf` 包將會在未來完全被 `Generic` 包取代。 如果您可以使用任何 .NET Core 軟體包，就不要使用框架包，因為 `Generic-netf` 與 .NET Core 版本相比缺少許多功能和相容性，並且隨著時間的推移，它的功能只會變少。 我們僅對無法使用 `Generic` 包的平臺提供此版本的支援（例如 `linux-x86`），並且也僅支援基於最新版本的運行時環境（例如最新版 Mono）。

* * *

### OS-specific

除了 Generic 包中包含的託管代碼之外，OS-specific 包還包括指定平臺的本機代碼。 換句話說，OS-specific 包內部**已經包含了可用的 .NET Core 運行時環境**，使您可以跳過麻煩的安裝過程，直接啟動 ASF。 OS-specific 包，顧名思義，是針對不同作業系統的，每種作業系統都需要其特定的版本——例如 Windows 需要 PE32+ `ArchiSteamFarm.exe`二進位檔案，而 Linux 則需要 Unix ELF `ArchiSteamFarm`二進位檔案。 您可能已經知道，這兩種類型之間是完全不相容的。

ASF目前可用於以下作業系統 ：

- `win-x64` 支援 64 位 Windows 作業系統。 包括 Windows 7（SP1+）、8.1、10、Server 2008 R2（SP1+）、2012、2012 R2、2016，以及未來的版本。
- `linux-arm`支援 32 位基於 ARM（ARMv7+）的 GNU/Linux 作業系統。 特別是包括所有 Raspberry Pi 2& 3 可用的 GNU/Linux 作業系統（例如 Raspbian）的當前和未來版本。 此包不支援更早的 ARM 架構，例如 Raspberry Pi 0 & 1 使用的 ARMv6，也不支援未實現 GNU/Linux 特性的作業系統，例如 Android。
- `linux-x64` 支援 64 位 GNU/Linux 作業系統。 包括 Alpine、CentOS/Fedora/RHEL、Debian/Ubuntu/Linux Mint、OpenSUSE/SLES 等作業系統以及它們的衍生版的當前和未來版本。
- `osx-x64` 支援64 位 OS X 作業系統。 包括 10.12 及更新版本。

當然，即使沒有適合您作業系統及架構的 OS-specific 包，您也可以手動安裝適當的 .NET Core 運行時環境並運行 Generic ASF 包，這也是這個包存在的主要原因。 Generic ASF 包與平臺無關，可在任何具有可用 .NET Core 運行時環境的平臺上運行。 需要注意——ASF 需要的是 .NET Core 運行時環境，而不是特定的作業系統或架構。 例如，如果您使用的是 32 位 Windows，但 ASF 沒有 `win-x86` 版本，您仍然可以安裝 `win-x86` 版本的 .NET Core SDK，然後運行 Generic 版本的 ASF。 我們無法為所有作業系統和架構組合都生成一份可執行档案，所以我們為此畫下一道分隔線。 x86 就是這條線的範例之一，因為這種架構自 2004 年開始就過時了。

您可以訪問​**[發行說明​](https://github.com/dotnet/core/blob/master/release-notes/2.2/2.2-supported-os.md)**查看完整的 .NET Core 2.2 支持的平臺與作業系統列表。

* * *

## 運行時環境需求

如果您正在使用 OS-specific 包，那麼您不必擔心運行時需求，因為 ASF 總會將所需的最新運行時環境打包在一起，只要您已安裝並更新 **[.NET Core 依賴項](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)**，就能夠正常運行。 換句話說，**您不需要安裝 .NET Core 運行時環境或 SDK**，因為 OS-specific 版本只需要本機已安裝對應作業系統的依賴項，而不需要其他項目。

但如果您使用 **Generic** 包，則必須保證已安裝 ASF 所需的對應平臺的 .NET Core 運行時環境。

ASF as a program is targeting **.NET Core 2.2** (`netcoreapp2.2`) right now, but it might target newer platform in the future. `netcoreapp2.2` is supported since 2.2.100 SDK (2.2.0 runtime), although ASF is configured to target **latest runtime at the moment of compilation**, so you should ensure that you have **[latest SDK](https://www.microsoft.com/net/download)** available for your machine. Generic ASF variant might refuse to launch if your runtime is older than the minimum (target) one known during compilation.

如有疑問，您可以訪問我們用於編譯併在 GitHub 上部署新版本的 **[CI](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)**。 您可以在每個構建的頂端找到 `dotnet --info` 的輸出。

* * *

## 問題與解決方案

### Debian Jessie 更新

如果您從 Debian 8 Jessie（或更舊版本）升級到 Debian 9 Stretch，請確保您**沒有** `libssl1.0.0` 包，清除方法之一是執行` apt-get purge libssl1.0.0`。 否則，您可能會遇到段錯誤。 This package is obsolete and doesn't exist by definition, neither is possible to install on clean Debian 9 setups, the only way to run into this issue is upgrading from Debian 8 or older - **[dotnet/corefx #8951](https://github.com/dotnet/corefx/issues/8951#issuecomment-314455190)**. If you have some other packages depending on that outdated libssl version then you should either upgrade them, or get rid of them - not only because of this issue, but also because they're based on obsolete library in the first place.