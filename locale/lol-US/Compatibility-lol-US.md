# COMPATIBILITY

ASF is a C# application that is running on .NET platform. DIS MEANZ DAT ASF IZ NOT COMPILD DIRECTLY INTO **[MACHINE CODE](https://en.wikipedia.org/wiki/Machine_code)** DAT IZ RUNNIN ON UR CPU, BUT INTO **[CIL](https://en.wikipedia.org/wiki/Common_Intermediate_Language)** DAT REQUIREZ CIL-COMPATIBLE RUNTIME 4 EXECUTIN IT.

DIS APPROACH HAS GIGANTIC AMOUNT OV ADVANTAGEZ, AS CIL IZ PLATFORM-INDEPENDENT, WHICH IZ Y ASF CAN RUN NATIVELY ON LOTZ DA AVAILABLE OSEZ, ESPECIALLY WINDOWS, LINUX AN OS X. THAR IZ NOT ONLY NO EMULASHUN NEEDD, BUT ALSO SUPPORT 4 ALL PLATFORM-RELATD AN HARDWARE-RELATD OPTIMIZASHUNS, SUCH AS CPU SSE INSTRUCSHUNS. THX 2 DAT, ASF CAN ACHIEVE SUPERIOR PERFORMANCE AN OPTIMIZASHUN, WHILE STILL OFFERIN PERFIK COMPATIBILITY AN RELIABILITY.

DIS ALSO MEANZ DAT ASF HAS **NO SPECIFIC OS REQUIREMENT**, CUZ IT REQUIREZ WERKIN **RUNTIME** ON DAT OS AN NOT OS ITSELF. As long as that runtime is executing ASF code properly, it does not matter whether underlying OS is Windows, Linux, OS X, BSD, Sony Playstation 4, Nintendo Wii or your toaster - as long as there is **[.NET for it](https://dotnet.microsoft.com/download/dotnet)**, there is **[ASF](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** for it.

However, regardless of where you run ASF, you must ensure that your target platform has **[.NET prerequisites](https://github.com/dotnet/core/blob/main/Documentation/prereqs.md)** installed. DOSE R LOW-LEVEL LIBRARIEZ REQUIRD 4 PROPR RUNTIME FUNCSHUNALITY AN ABSOLUTELY CORE 4 ASF 2 WERK IN DA FURST PLACE. VRY LIKELY U CAN HAS SUM OV THEM (OR EVEN ALL) ALREADY INSTALLD.

---

## ASF PACKAGIN

ASF COMEZ IN 2 MAIN FLAVOURS - GENERIC PACKAGE AN OS-SPECIFIC. FUNCSHUNALITY-WIZE BOTH PACKAGEZ R EGSAKTLY TEH SAME, THEYRE BOTH ALSO CAPABLE OV AUTOMATICALLY UPDATIN THEMSELVEZ. TEH ONLY DIFFERENCE TWEEN THEM IZ WHETHR OR NOT ASF **GENERIC** PACKAGE ALSO COMEZ WIF **OS-SPECIFIC** RUNTIME 2 POWR IT.

---

### GENERIC

GENERIC PACKAGE IZ PLATFORM-AGNOSTIC BUILD DAT DOESNT INCLUDE ANY MACHINE-SPECIFIC CODE. This setup requires from you to have .NET runtime already installed on your OS **in appropriate version**. We all know how troublesome it is to keep dependencies up-to-date, therefore this package is here mainly for people that **already use** .NET and don't want to duplicate their runtime solely for ASF if they can make use of what they have installed already. Generic package also allows you to run ASF **anywhere where you can obtain working implementation of .NET runtime**, regardless if there exists OS-specific ASF build for it, or not.

It's not recommended to use generic flavour if you're casual or even advanced user that just wants to make ASF work and not dig into .NET technical details. IN OTHR WERDZ - IF U KNOE WUT DIS AR TEH, U CAN USE IT, OTHERWIZE IZ MUTCH BETTR 2 USE OS-SPECIFIC PACKAGE EXPLAIND BELOW.

#### .NET FRAMEWORK PACKAGE

In addition to generic package mentioned above, there is also `generic-netf` package which is built on top of .NET Framework and not .NET (Core). This package is a legacy variant that provides missing compatibility known from ASF V2 times, and can be run e.g. with **[Mono](https://www.mono-project.com)**, while .NET `generic` package can't as of today.

IN GENERAL U SHUD **AVOID DIS PACKAGE AS MUTCH AS POSIBLE**, AS MAJORITY OV OPERATIN SISTEMS AN SETUPS R PERFECTLY (AN MUTCH BETTR) SUPPORTD WIF `generic` PACKAGE MENSHUND ABOOV. In fact, this package makes sense to be used only on platforms that lack working .NET runtime, while having working Mono implementation. Examples of such platforms include `linux-x86` (32-bit i386/i686 linux), as well as `linux-armel` (ARMv6 boards found e.g. in Raspberry Pi 0 & 1), all of which do not have official working .NET runtime as of today.

As the time goes on with more platforms being supported by .NET and less compatibility between .NET Framework and .NET, `generic-netf` package will be entirely replaced with `generic` one in the future. Please refrain from using it if you can use any .NET package instead, as `generic-netf` is missing a lot of functionality and compatibility compared to .NET versions, and it'll be only less functional as the time goes on. WE OFFR SUPPORT 4 DIS PACKAGE **ONLY** ON MACHINEZ DAT CANT USE `generic` VARIANT ABOOV (E.G. `linux-x86`), AN ONLY WIF UP-2-DATE RUNTIME (E.G. LATEST MONO).

---

### OS-SPECIFIC

OS-SPECIFIC PACKAGE, APART FRUM MANAGD CODE INCLUDD IN GENERIC PACKAGE, ALSO INCLUDEZ NATIV CODE 4 GIVEN PLATFORM. In other words, OS-specific package **already includes proper .NET runtime inside**, which allows you to entirely skip the whole installation mess and just launch ASF directly. OS-SPECIFIC PACKAGE, AS U CAN GUES FRUM TEH NAYM, IZ OS-SPECIFIC AN EVRY OS REQUIREZ ITZ OWN VERSHUN - 4 EXAMPLE WINDOWS REQUIREZ PE32+ `ArchiSteamFarm.exe` BINARY WHILE LINUX WERKZ WIF UNIX ELF `ArchiSteamFarm` BINARY. AS U CUD KNOE, DOSE 2 TYPEZ R NOT COMPATIBLE WIF EACH OTHR.

ASF IZ CURRENTLY AVAILABLE IN FOLLOWIN OS-SPECIFIC VARIANTS:

- `win-x64` WERKZ ON 64-BIT WINDOWS OSEZ. This includes Windows 7 SP1+, 8.1, 10, 11, Server 2012+ as well as future versions. Windows 7 requires **[generic](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up#generic-setup)** variant and its support is very limited, you may have issues running ASF in that environment. We strongly recommend an update soon, as future versions of ASF are likely to stop working altogether with it, not to mention that the OS reached its end of life back in 2020.
- `linux-arm` WERKZ ON 32-BIT ARM-BASD (ARMV7+) GNU/LINUX OSEZ. This includes platforms such as Raspberry Pi 2 (and newer) with all GNU/Linux OSes available for them (such as Raspberry Pi OS), in current and future versions. This variant will **not** work with older ARM architectures, such as ARMv6 found in Raspberry Pi 0 & 1, it will also not work with OSes that do not implement required GNU/Linux environment (such as Android).
- `linux-arm64` WERKZ ON 64-BIT ARM-BASD (ARMV8+) GNU/LINUX OSEZ. DIS INCLUDEZ PLATFORMS SUCH AS RASPBERRY PI 3 (AN NEWR) WIF ALL AARCH64 GNU/LINUX OSEZ AVAILABLE 4 THEM (SUCH AS DEBIAN), IN CURRENT AN FUCHUR VERSHUNS. This variant will **not** work with 32-bit OSes that do not have required 64-bit libraries available (such as Raspberry Pi OS), it will also not work with OSes that do not implement required GNU/Linux environment (such as Android).
- `linux-x64` WERKZ ON 64-BIT GNU/LINUX OSEZ. This includes Alpine, CentOS/Fedora/RHEL, Debian/Ubuntu, OpenSUSE/SLES and many other ones, including their derivatives, in current and future versions.
- `osx-arm64` works on 64-bit ARM-based (Apple silicon) OS X OSes. This includes version 11, as well as future ones.
- `osx-x64` WERKZ ON 64-BIT OS X OSEZ. This includes version 10.15, as well as future ones.

Of course, even if you don't have OS-specific package available for your OS-architecture combination, you can always install appropriate .NET runtime yourself and run generic ASF flavour, which is also the main reason why it exists in the first place. Generic ASF build is platform-agnostic and will run on any platform that has a working .NET runtime. This is important to note - ASF requires .NET runtime, not some specific OS or architecture. For example, if you're running 32-bit Windows then despite of no dedicated `win-x86` ASF version, you can still install .NET SDK in `win-x86` version and run generic ASF just fine. WE SIMPLY CANT TARGET EVRY OS-ARCHITECCHUR COMBINASHUN DAT EXISTS AN IZ USD BY SOMEBODY, SO WE HAS 2 DRAW LINE SOMEWHERE. X86 IZ GUD EXAMPLE OV DAT LINE, AS IZ OBSOLETE ARCHITECCHUR SINCE AT LEAST 2004.

For a complete list of all supported platforms and OSes by .NET 6.0, visit **[release notes](https://github.com/dotnet/core/blob/main/release-notes/6.0/supported-os.md)**.

---

## RUNTIME REQUIREMENTS

If you're using OS-specific package then you don't need to worry about runtime requirements, because ASF always ships with required and up-to-date runtime that will work properly as long as you have **[.NET prerequisites](https://github.com/dotnet/core/blob/main/Documentation/prereqs.md)** installed and up-to-date. In other words, **you don't need to install .NET runtime or SDK**, as OS-specific builds require only native OS dependencies (prerequisites) and nothing else.

However, if you're trying to run **generic** ASF package then you must ensure that your .NET runtime supports platform required by ASF.

ASF as a program is targeting **.NET 6.0** (`net6.0`) right now, but it may target newer platform in the future. `net6.0` is supported since 6.0.100 SDK (6.0.0 runtime), although ASF is configured to prefer **latest runtime at the moment of compilation**, so you should ensure that you have **[latest SDK](https://dotnet.microsoft.com/download)** (or at least runtime) available for your machine. Generic ASF variant may refuse to launch if your runtime is older than the specified minimum supported one during compilation.

IF IN DOUBT, CHECK WUT R **[CONTINUOUS INTEGRASHUN USEZ](https://github.com/JustArchiNET/ArchiSteamFarm/actions/workflows/publish.yml?query=branch%3Amain)** 4 COMPILIN AN DEPLOYIN ASF RELEASEZ ON GITHUB. U CAN FIND `dotnet --info` OUTPUT IN EVRY BUILD AS PART OV .NET VERIFICASHUN STEP.