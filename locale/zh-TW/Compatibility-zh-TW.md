# 相容性

ASF 是一個在 .NET Core 平台上執行的 C# 應用程式。 這意味著 ASF 並非被編譯為可供 CPU 直接執行的​**[機器碼](https://en.wikipedia.org/wiki/Machine_code)**，而是被編譯為  **[CIL（通用中間語言）](https://en.wikipedia.org/wiki/Common_Intermediate_Language)**碼，一種需要相容的執行階段才能執行的語言。

這種方法能夠帶來巨大的方便。由於 CIL 是跨平台的，這使得 ASF 能夠在許多作業系統上執行，特別是 Windows、Linux 和 OS X 這三個系統。ASF 不但無須透過模擬執行，同時所有對於系統及其相關硬體的最佳化也對其有效。 因此，ASF可以實現卓越的效能和最佳化，同時仍然提供完美的相容性和可靠性。

這也意味著執行 ASF **沒有特定的作業系統需求**，因為它需要的只是執行於作業系統上的**執行階段**而非作業系統本身。 As long as that runtime is executing ASF code properly, it does not matter whether underlying OS is Windows, Linux, OS X, BSD, Sony Playstation 4, Nintendo Wii or your toaster - as long as there is **[.NET for it](https://dotnet.microsoft.com/download/dotnet)**, there is **[ASF](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** for it.

然而，無論您想要在哪個平台上執行 ASF，您必須確保該平台安裝了 **[ .NET Core 的必要條件 ](https://github.com/dotnet/core/blob/main/Documentation/prereqs.md)**。 這些都是確保執行階段功能正常的底層庫，也是確保 ASF 能夠第一時間工作的絕對核心。 很有可能你已經安裝了其中的一些（甚至全部）。

---

## ASF 套件

ASF 主要有兩種封裝方式——通用（Generic）套件和特定作業系統（OS-specific）套件。 從功能上來講，這兩種套件是完全一樣的，都能夠自動進行更新。 唯一的區別就是**通用（Generic）**套件中不包含能使 ASF 直接執行的**特定作業系統**執行階段。

---

### 通用（Generic）

通用（Generic）套件與平台無關，所以它不包含任何給特定機器的程式碼。 This setup requires from you to have .NET runtime already installed on your OS **in appropriate version**. We all know how troublesome it is to keep dependencies up-to-date, therefore this package is here mainly for people that **already use** .NET and don't want to duplicate their runtime solely for ASF if they can make use of what they have installed already. Generic package also allows you to run ASF **anywhere where you can obtain working implementation of .NET runtime**, regardless if there exists OS-specific ASF build for it, or not.

It's not recommended to use generic flavour if you're casual or even advanced user that just wants to make ASF work and not dig into .NET technical details. 也就是說，如果你瞭解通用套件，那你可以使用它，不然特定作業系統套件才是更合適的。

#### .NET Framework 套件

In addition to generic package mentioned above, there is also `generic-netf` package which is built on top of .NET Framework and not .NET (Core). This package is a legacy variant that provides missing compatibility known from ASF V2 times, and can be run e.g. with **[Mono](https://www.mono-project.com)**, while .NET `generic` package can't as of today.

通常，您應該**儘量避免使用此程式套件**，因為大多數作業系統都完全（並且更好地）支援上面提到的 `Generic`（通用）套件。 In fact, this package makes sense to be used only on platforms that lack working .NET runtime, while having working Mono implementation. Examples of such platforms include `linux-x86` (32-bit i386/i686 linux), as well as `linux-armel` (ARMv6 boards found e.g. in Raspberry Pi 0 & 1), all of which do not have official working .NET runtime as of today.

As the time goes on with more platforms being supported by .NET and less compatibility between .NET Framework and .NET, `generic-netf` package will be entirely replaced with `generic` one in the future. Please refrain from using it if you can use any .NET package instead, as `generic-netf` is missing a lot of functionality and compatibility compared to .NET versions, and it'll be only less functional as the time goes on. 我們**僅**對無法使用`通用`套件的平台提供此版本的支援（例如 `linux-x86`），並且也僅支援基於最新版本的執行階段（例如最新版 Mono）。

---

### 特定作業系統（OS-specific）

除了通用（Generic）套件中包含的託管程式碼之外，特定作業系統套件還包括指定平台的本地程式碼。 In other words, OS-specific package **already includes proper .NET runtime inside**, which allows you to entirely skip the whole installation mess and just launch ASF directly. 特定作業系統套件，顧名思義，是針對不同作業系統的，每種作業系統都需要其特定的版本——例如 Windows 需要 PE32+ `ArchiSteamFarm.exe`二進位檔案，而 Linux 則需要 Unix ELF `ArchiSteamFarm`二進位檔案。 您可能已經知道，這兩種類型之間是完全不相容的。

ASF 目前可用於以下作業系統：

- `linux-arm`支援基於 ARM（ARMv7+）的 32 位元 GNU／Linux 作業系統。 This includes platforms such as Raspberry Pi 2 (and newer) with all GNU/Linux OSes available for them (such as Raspberry Pi OS), in current and future versions. This variant will **not** work with older ARM architectures, such as ARMv6 found in Raspberry Pi 0 & 1, it will also not work with OSes that do not implement required GNU/Linux environment (such as Android).
- `linux-arm64` works on 64-bit ARM-based (ARMv8+) GNU/Linux OSes. This includes platforms such as Raspberry Pi 3 (and newer) with all AArch64 GNU/Linux OSes available for them (such as Debian), in current and future versions. This variant will **not** work with 32-bit OSes that do not have required 64-bit libraries available (such as Raspberry Pi OS), it will also not work with OSes that do not implement required GNU/Linux environment (such as Android).
- `linux-x64` 支援 64 位元的 GNU／Linux 作業系統。 This includes Alpine, CentOS/Fedora/RHEL, Debian/Ubuntu, OpenSUSE/SLES and many other ones, including their derivatives, in current and future versions.
- `osx-arm64` works on 64-bit ARM-based (Apple silicon) OS X OSes. This includes version 11, as well as future ones.
- `osx-x64` 支援 64 位元的 OS X 作業系統。 This includes version 10.15, as well as future ones.
- `win-x64` 支援 64 位元的 Windows 作業系統。 This includes Windows 8.1, 10, 11, Server 2012+ as well as future versions. Windows 7 requires **[generic](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up#generic-setup)** variant and its support is very limited, you may have issues running ASF in that environment. We strongly recommend an update soon, as future versions of ASF are likely to stop working altogether with it, not to mention that the OS reached its end of life back in 2020.

Of course, even if you don't have OS-specific package available for your OS-architecture combination, you can always install appropriate .NET runtime yourself and run generic ASF flavour, which is also the main reason why it exists in the first place. Generic ASF build is platform-agnostic and will run on any platform that has a working .NET runtime. This is important to note - ASF requires .NET runtime, not some specific OS or architecture. For example, if you're running 32-bit Windows then despite of no dedicated `win-x86` ASF version, you can still install .NET SDK in `win-x86` version and run generic ASF just fine. 我們無法為所有作業系統和架構組合都產生一份可執行档案，所以我們為此畫下一道分隔線。 x86 就是這條線的範例之一，因為這種架構自 2004 年開始就過時了。

For a complete list of all supported platforms and OSes by .NET 6.0, visit **[release notes](https://github.com/dotnet/core/blob/main/release-notes/6.0/supported-os.md)**.

---

## 執行階段必要條件

If you're using OS-specific package then you don't need to worry about runtime requirements, because ASF always ships with required and up-to-date runtime that will work properly as long as you have **[.NET prerequisites](https://github.com/dotnet/core/blob/main/Documentation/prereqs.md)** installed and up-to-date. In other words, **you don't need to install .NET runtime or SDK**, as OS-specific builds require only native OS dependencies (prerequisites) and nothing else.

However, if you're trying to run **generic** ASF package then you must ensure that your .NET runtime supports platform required by ASF.

ASF as a program is targeting **.NET 6.0** (`net6.0`) right now, but it may target newer platform in the future. `net6.0` is supported since 6.0.100 SDK (6.0.0 runtime), although ASF is configured to prefer **latest runtime at the moment of compilation**, so you should ensure that you have **[latest SDK](https://dotnet.microsoft.com/download)** (or at least runtime) available for your machine. Generic ASF variant may refuse to launch if your runtime is older than the specified minimum supported one during compilation.

如有疑問，您可以訪問我們用於編譯並在 GitHub 上部署新版本的 **[CI](https://github.com/JustArchiNET/ArchiSteamFarm/actions/workflows/publish.yml?query=branch%3Amain)**。 You can find `dotnet --info` output in every build as part of .NET verification step.