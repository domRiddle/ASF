# 相容性

ASF 是一個在 .NET Core 平台上執行的 C# 應用程式。 這意味著 ASF 並非直接被編譯為可供 CPU 執行的​**[機器碼](https://en.wikipedia.org/wiki/Machine_code)**，而是被編譯為 **[通用中間語言](https://en.wikipedia.org/wiki/Common_Intermediate_Language)**，一種需要相應的執行階段才能執行的語言。

這種方法能夠帶來巨大的方便。由於 CIL 是跨平台的，這使得 ASF 能夠執行在許多作業系統上，特別是 Windows、Linux 和 OS X 這三個系統。ASF 不僅不需要透過模擬執行，同時所有對於系統及其相關硬體的優化也對其有效。 因此，ASF可以實現卓越的效能和優化，同時仍然提供完美的相容性和可靠性。

這也意味著執行 ASF **沒有特定的作業系統要求**，因為它需要的只是執行於作業系統上的**執行階段**而非作業系統本身。 這也意味著執行 ASF 沒有特定的作業系統要求，因為它需要的只是執行於作業系統上的執行環境而非作業系統本身。 只要執行環境能夠正確地執行 ASF 的代碼，底層系統是 Windows、Linux、OS X 還是 BSD，硬體是 Sony Playstation 4、Nintendo Wii甚至是您的烤麵包機都無所謂。只要有**[.NET Core ](https://github.com/dotnet/core-setup#daily-builds)**，就能用 **[ASF](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)**。

但是，無論您想要在哪個平台上執行 ASF，您必須確保該平台安裝了 **[.NET Core 必要條件](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)**。 這些都是確保執行階段功能正常的底層庫，也是確保 ASF 能夠第一時間工作的絕對核心。 很有可能你已經安裝了其中的一些（甚至全部）。

* * *

## ASF 封裝

ASF comes in 2 main flavours - generic package and OS-specific. 從功能上來講，這兩種套件是完全一樣的，都能夠自動進行更新。 唯一的區別就是 **Generic**（通用）套件中不包含能使 ASF 執行**特定作業系統**的執行階段。

* * *

### 通用

通用（Generic）套件與平台無關，所以它不包含任何特定於電腦的代碼。 所以使用這個包需要您的作業系統中已經安裝有**合適版本**的 .NET Core 執行階段。 我們都知道保證依賴项始終是最新是十分麻烦的，所以這個套件主要面向那些**已經在使用** .NET Core，不想仅仅為了 ASF 對已有執行階段做單獨備份的人。 通用（Generic）套件還允許您將 ASF 執行在**任何拥有正常 .NET Core 執行階段的裝置上**，不需要擔心是否存在特定作業系統的 ASF 組建。

如果您只是想要執行 ASF 而不想要深入瞭解 . NET Core 的技術細節，我们并不推薦一般甚至是進階使用者使用通用（Generic）套件。 也就是說，如果你瞭解通用套件，那你可以使用它，不然特定作業系統套件才是更合適的。

#### .NET Framework 套件

除了上面提到的通用（Generic）套件，我們也提供 `Generic-netf` 套件，它基於 .NET Framework（而非 .NET Core）。 該包是一種舊式套件，它補全了從 ASF V2 時代即已知的相容性缺失，並且可以使用 **[Mono](https://www.mono-project.com)** 執行，當前來自 .NET Core 的 `Generic`（通用）套件無法用於Mono。

通常，您應該**儘量避免使用此程式套件**，因為大多數作業系統都完全（並且更好地）支援上面提到的 `Generic`（通用）套件。 事實上，這個軟體套件只適用於缺失 .NET Core 執行階段，但能夠執行 Mono 的平台。 Examples of such platforms include `linux-x86` (32-bit i386/i686 linux), as well as `linux-armel` (ARMv6 boards found e.g. in Raspberry Pi 0 & 1), all of which do not have official working .NET Core runtime as of today.

隨著時間的推移，.NET Core 會支援更多平台，而 .NET Framework 和 .NET Core 之間會更加不相容，`Generic-netf` 套件將會在未來完全被 `Generic`（通用）套件取代。 如果您可以使用任何 .NET Core 軟體套件，就不要使用 Framework 套件，因為 `Generic-netf` 與 .NET Core 版本相比缺少許多功能和相容性，並且隨著時間的推移，它的功能只會變少。 We offer support for this package **only** on machines that can't use `generic` variant above (e.g. `linux-x86`), and only with up-to-date runtime (e.g. latest Mono).

* * *

### 特定作業系統（OS-specific）

除了通用（Generic）套件中包含的託管代碼之外，特定作業系統套件還包括指定平台的本機代碼。 換句話說，特定作業系統套件內部**已經包含了可用的 .NET Core 執行階段**，使您可以跳過麻煩的安裝過程，直接啟動 ASF。 特定作業系統套件，顧名思義，是針對不同作業系統的，每種作業系統都需要其特定的版本——例如 Windows 需要 PE32+ `ArchiSteamFarm.exe`二進位檔案，而 Linux 則需要 Unix ELF `ArchiSteamFarm`二進位檔案。 As you may know, those two types are not compatible with each other.

ASF 目前可用於以下作業系統 ：

- `win-x64` 支援 64 位 Windows 作業系統。 包括 Windows 7（SP1+）、8.1、10、Server 2008 R2（SP1+）、2012、2012 R2、2016，以及未來的版本。
- `linux-arm` works on 32/64-bit ARM-based (ARMv7+) GNU/Linux OSes. This includes platforms such as Raspberry Pi 2 (and newer) with all GNU/Linux OSes available for them (such as Raspbian), in current and future versions. This variant will not work with older ARM architectures, such as ARMv6 found in Raspberry Pi 0 & 1, it will also not work with OSes that do not implement required GNU/Linux environment, such as Android.
- `linux-x64` 支援 64 位 GNU/Linux 作業系統。 包括 Alpine、CentOS/Fedora/RHEL、Debian/Ubuntu/Linux Mint、OpenSUSE/SLES 等作業系統以及它們的衍生版的當前和未來版本。
- `osx-x64` 支援64 位 OS X 作業系統。 包括 10.13 及更新版本。

當然，即使沒有適合您作業系統及架構的特定作業系統套件，您也可以手動安裝適當的 .NET Core 執行階段並執行通用（Generic）ASF 套件，這也是這個套件存在的主要原因。 通用（Generic）ASF 包與平台無關，可在任何具有可用 .NET Core 執行階段的平台上執行。 需要注意——ASF 需要的是 .NET Core 執行階段，而不是特定的作業系統或架構。 例如，如果您使用的是 32 位 Windows，但 ASF 沒有 `win-x86` 版本，您仍然可以安裝 `win-x86` 版本的 .NET Core SDK，然後執行通用（Generic）版本的 ASF。 我們無法為所有作業系統和架構組合都產生一份可執行档案，所以我們為此畫下一道分隔線。 x86 就是這條線的範例之一，因為這種架構自 2004 年開始就過時了。

您可以訪問​**[發行說明​](https://github.com/dotnet/core/blob/master/release-notes/3.0/3.0-supported-os.md)**查看完整的 .NET Core 3.0 支援的平台與作業系統列表。

* * *

## 執行階段必要條件

如果您正在使用特定作業系統套件，那麼您不必擔心執行環境必要條件，因為 ASF 總會將所需的最新執行環境封裝在一起，只要您已安裝並更新 **[.NET Core 依賴項](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)**，就能夠正常執行。 換句話說，**您不需要安裝 .NET Core 執行階段或 SDK**，因為特定作業系統版本只需要本機已安裝對應作業系統的依賴項，而不需要其他項目。

但如果您使用 **Generic**（通用）套件，則必須保證已安裝 ASF 所需的對應平台的 .NET Core 執行階段。

ASF as a program is targeting **.NET Core 3.0** (`netcoreapp3.0`) right now, but it may target newer platform in the future. `netcoreapp3.0` is supported since 3.0.100 SDK (3.0.0 runtime), although ASF is configured to target **latest runtime at the moment of compilation**, so you should ensure that you have **[latest SDK](https://dotnet.microsoft.com/download)** (or at least runtime) available for your machine. Generic ASF variant may refuse to launch if your runtime is older than the minimum (target) one known during compilation.

如有疑問，您可以訪問我們用於編譯並在 GitHub 上部署新版本的 **[CI](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)**。 您可以在每個構建的頂端找到 `dotnet --info` 的輸出。

* * *

## 問題

### Various lock-related issues running ASF on Linux VPS with OpenVZ virtualization

OpenVZ kernel is usually based on a very old Linux kernel version (2.6) which seems incompatible with latest .NET Core runtime. If you're trying to run ASF in such environment, you may encounter various lock-related issues, usually in form of process freeze. See related CoreCLR issue: https://github.com/dotnet/coreclr/issues/26873

Our recommendation is to ditch OpenVZ in favour of much better virtualization solutions, such as KVM. The issue described here is indeed a .NET Core runtime bug that is supposed to be **[fixed](https://github.com/dotnet/coreclr/pull/26912)** in the next .NET Core runtime upgrade (3.1, probably also 3.0), but there is no actual timeframe for it yet. If you're unable to move to better virtualization solution, you can consider running `generic-netf` ASF variant with `mono`, at least until new runtime version is released.

Of course you can also run previous version of ASF (V4.0), but running outdated ASF versions is not supported by us and might cause you entirely different issues, possibly more severe.