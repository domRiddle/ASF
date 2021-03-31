# COMPATIBILITY

ASF IZ C# APPLICASHUN DAT IZ RUNNIN ON .NET CORE PLATFORM. DIS MEANZ DAT ASF IZ NOT COMPILD DIRECTLY INTO **[MACHINE CODE](https://en.wikipedia.org/wiki/Machine_code)** DAT IZ RUNNIN ON UR CPU, BUT INTO **[CIL](https://en.wikipedia.org/wiki/Common_Intermediate_Language)** DAT REQUIREZ CIL-COMPATIBLE RUNTIME 4 EXECUTIN IT.

DIS APPROACH HAS GIGANTIC AMOUNT OV ADVANTAGEZ, AS CIL IZ PLATFORM-INDEPENDENT, WHICH IZ Y ASF CAN RUN NATIVELY ON LOTZ DA AVAILABLE OSEZ, ESPECIALLY WINDOWS, LINUX AN OS X. THAR IZ NOT ONLY NO EMULASHUN NEEDD, BUT ALSO SUPPORT 4 ALL PLATFORM-RELATD AN HARDWARE-RELATD OPTIMIZASHUNS, SUCH AS CPU SSE INSTRUCSHUNS. THX 2 DAT, ASF CAN ACHIEVE SUPERIOR PERFORMANCE AN OPTIMIZASHUN, WHILE STILL OFFERIN PERFIK COMPATIBILITY AN RELIABILITY.

DIS ALSO MEANZ DAT ASF HAS **NO SPECIFIC OS REQUIREMENT**, CUZ IT REQUIREZ WERKIN **RUNTIME** ON DAT OS AN NOT OS ITSELF. AS LONG AS DAT RUNTIME IZ EXECUTIN ASF CODE PROPERLY, IT DOEZ NOT MATTR WHETHR UNDERLYIN OS IZ WINDOWS, LINUX, OS X, BSD, SONY PLAYSTASHUN 4, NINTENDO WII OR UR TOASTR - AS LONG AS THAR IZ **[.NET CORE 4 IT](https://github.com/dotnet/core-setup#daily-builds)**, THAR IZ **[ASF](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** 4 IT.

HOWEVR, REGARDLES OV WER U RUN ASF, U MUST ENSURE DAT UR TARGET PLATFORM HAS **[.NET CORE PREREQUIZIETS](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)** INSTALLD. DOSE R LOW-LEVEL LIBRARIEZ REQUIRD 4 PROPR RUNTIME FUNCSHUNALITY AN ABSOLUTELY CORE 4 ASF 2 WERK IN DA FURST PLACE. VRY LIKELY U CAN HAS SUM OV THEM (OR EVEN ALL) ALREADY INSTALLD.

* * *

## MULTIPLE INSTANCEZ

ASF IZ COMPATIBLE WIF RUNNIN MULTIPLE INSTANCEZ OV TEH PROCES ON TEH SAME MACHINE. The instances can be completely standalone or derived from the same binary location (in which case, you want to run them with different `--path` **[command-line argument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments)**).

WHEN RUNNIN MULTIPLE INSTANCEZ FRUM TEH SAME BINARY, KEEP IN MIND DAT U SHUD TYPICALLY DISABLE AUTO-UPDATEZ IN ALL OV THEIR CONFIGS, AS THAR IZ NO SYNCHRONIZASHUN TWEEN THEM IN REGARDZ 2 AUTO-UPDATEZ. IF UD LIEK 2 KEEP HAVIN AUTO-UPDATEZ ENABLD, WE RECOMMEND STANDALONE INSTANCEZ, BUT U CAN STILL MAK UPDATEZ WERK, AS LONG AS U CAN ENSURE DAT ALL OTHR ASF INSTANCEZ R CLOSD.

ASF WILL DO ITZ BEST 2 MAINTAIN MINIMUM AMOUNT OV OS-WIDE, CROS-PROCES COMMUNICASHUN WIF OTHR ASF INSTANCEZ. This includes ASF checking its configuration directory against other instances, as well as sharing core process-wide limiters configured with `*LimiterDelay` **[global config properties](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#global-config)**, ensuring that running multiple ASF instances will not cause a possibility to run into a rate-limiting issue. In regards to technical aspects, all platforms use our dedicated mechanism of custom ASF file-based locks created in temporary directory, which is `C:\Users\<YourUser>\AppData\Local\Temp\ASF` on Windows, and `/tmp/ASF` on Unix.

It's not required for running ASF instances to share the same `*LimiterDelay` properties, they can use different values, as each ASF will add its own configured delay to the release time after acquiring the lock. If the configured `*LimiterDelay` is set to `0`, ASF instance will entirely skip waiting for the lock of given resource that is shared with other instances (that could potentially still maintain a shared lock with each other). When set to any other value, ASF will properly synchronize with other ASF instances and wait for its turn, then release the lock after configured delay, allowing other instances to continue.

ASF takes into account `WebProxy` setting when deciding about shared scope, which means that two ASF instances using different `WebProxy` configurations will not share their limiters with each other. This is implemented in order to allow `WebProxy` setups to operate without excessive delays, as expected from different network interfaces. This should be good enough for majority of use cases, however, if you have a specific custom setup in which you're e.g. routing requests yourself in a different way, you can specify network group yourself through `--network-group` **[command-line argument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments)**, which will allow you to declare ASF group that will be synchronized with this instance. Keep in mind that custom network groups are used exclusively, which means that ASF will no longer use `WebProxy` for determining the right group, as you're in charge of grouping in this case.

* * *

## ASF packaging

ASF comes in 2 main flavours - generic package and OS-specific. Functionality-wise both packages are exactly the same, they're both also capable of automatically updating themselves. The only difference between them is whether or not ASF **generic** package also comes with **OS-specific** runtime to power it.

* * *

### Generic

Generic package is platform-agnostic build that doesn't include any machine-specific code. This setup requires from you to have .NET Core runtime already installed on your OS **in appropriate version**. We all know how troublesome it is to keep dependencies up-to-date, therefore this package is here mainly for people that **already use** .NET Core and don't want to duplicate their runtime solely for ASF if they can make use of what they have installed already. Generic package also allows you to run ASF **anywhere where you can obtain working implementation of .NET Core runtime**, regardless if there exists OS-specific ASF build for it, or not.

It's not recommended to use generic flavour if you're casual or even advanced user that just wants to make ASF work and not dig into .NET Core technical details. In other words - if you know what this is, you can use it, otherwise it's much better to use OS-specific package explained below.

#### .NET Framework package

In addition to generic package mentioned above, there is also `generic-netf` package which is built on top of .NET Framework (and not .NET Core). This package is a legacy variant that provides missing compatibility known from ASF V2 times, and can be run e.g. with **[Mono](https://www.mono-project.com)**, while .NET Core `generic` package can't as of today.

In general you should **avoid this package as much as possible**, as majority of operating systems and setups are perfectly (and much better) supported with `generic` package mentioned above. In fact, this package makes sense to be used only on platforms that lack working .NET Core runtime, while having working Mono implementation. Examples of such platforms include `linux-x86` (32-bit i386/i686 linux), as well as `linux-armel` (ARMv6 boards found e.g. in Raspberry Pi 0 & 1), all of which do not have official working .NET Core runtime as of today.

As the time goes on with more platforms being supported by .NET Core and less compatibility between .NET Framework and .NET Core, `generic-netf` package will be entirely replaced with `generic` one in the future. Please refrain from using it if you can use any .NET Core package instead, as `generic-netf` is missing a lot of functionality and compatibility compared to .NET Core versions, and it'll be only less functional as the time goes on. We offer support for this package **only** on machines that can't use `generic` variant above (e.g. `linux-x86`), and only with up-to-date runtime (e.g. latest Mono).

* * *

### OS-specific

OS-specific package, apart from managed code included in generic package, also includes native code for given platform. In other words, OS-specific package **already includes proper .NET Core runtime inside**, which allows you to entirely skip the whole installation mess and just launch ASF directly. OS-specific package, as you can guess from the name, is OS-specific and every OS requires its own version - for example Windows requires PE32+ `ArchiSteamFarm.exe` binary while Linux works with Unix ELF `ArchiSteamFarm` binary. As you may know, those two types are not compatible with each other.

ASF is currently available in following OS-specific variants:

- `win-x64` works on 64-bit Windows OSes. This includes Windows 7 (SP1+), 8.1, 10, Server 2012 R2, 2016, as well as future versions.
- `linux-arm` works on 32-bit ARM-based (ARMv7+) GNU/Linux OSes. This includes platforms such as Raspberry Pi 2 (and newer) with all GNU/Linux OSes available for them (such as Raspbian), in current and future versions. This variant will not work with older ARM architectures, such as ARMv6 found in Raspberry Pi 0 & 1, it will also not work with OSes that do not implement required GNU/Linux environment (such as Android).
- `linux-arm64` works on 64-bit ARM-based (ARMv8+) GNU/Linux OSes. This includes platforms such as Raspberry Pi 3 (and newer) with all AArch64 GNU/Linux OSes available for them (such as Debian), in current and future versions. This variant will not work with 32-bit OSes that do not have required 64-bit libraries available (such as Raspbian), it will also not work with OSes that do not implement required GNU/Linux environment (such as Android).
- `linux-x64` works on 64-bit GNU/Linux OSes. This includes Alpine, CentOS/Fedora/RHEL, Debian/Ubuntu/Linux Mint, OpenSUSE/SLES and many other ones, including their derivatives, in current and future versions.
- `osx-x64` works on 64-bit OS X OSes. This includes 10.13, as well as future versions.

Of course, even if you don't have OS-specific package available for your OS-architecture combination, you can always install appropriate .NET Core runtime yourself and run generic ASF flavour, which is also the main reason why it exists in the first place. Generic ASF build is platform-agnostic and will run on any platform that has a working .NET Core runtime. This is important to note - ASF requires .NET Core runtime, not some specific OS or architecture. For example, if you're running 32-bit Windows then despite of no dedicated `win-x86` ASF version, you can still install .NET Core SDK in `win-x86` version and run generic ASF just fine. We simply can't target every OS-architecture combination that exists and is used by somebody, so we have to draw a line somewhere. x86 is a good example of that line, as it's obsolete architecture since at least 2004.

For a complete list of all supported platforms and OSes by .NET Core 5.0, visit **[release notes](https://github.com/dotnet/core/blob/master/release-notes/5.0/5.0-supported-os.md)**.

* * *

## Runtime requirements

If you're using OS-specific package then you don't need to worry about runtime requirements, because ASF always ships with required and up-to-date runtime that will work properly as long as you have **[.NET Core prerequisites](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)** installed and up-to-date. In other words, **you don't need to install .NET Core runtime or SDK**, as OS-specific builds require only native OS dependencies (prerequisites) and nothing else.

However, if you're trying to run **generic** ASF package then you must ensure that your .NET Core runtime supports platform required by ASF.

ASF as a program is targeting **.NET 5.0** (`net5.0`) right now, but it may target newer platform in the future. `net5.0` is supported since 5.0.100 SDK (5.0.0 runtime), although ASF is configured to target **latest runtime at the moment of compilation**, so you should ensure that you have **[latest SDK](https://dotnet.microsoft.com/download)** (or at least runtime) available for your machine. Generic ASF variant may refuse to launch if your runtime is older than the minimum (target) one known during compilation.

If in doubt, check what our **[continuous integration uses](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)** for compiling and deploying ASF releases on GitHub. You can find `dotnet --info` output on top of each build.