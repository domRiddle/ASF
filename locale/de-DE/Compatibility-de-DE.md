# Kompatibilität

ASF ist eine auf der .NET-Plattform laufende C#-Anwendung. Das bedeutet, dass ASF nicht direkt in **[Maschinencode](https://en.wikipedia.org/wiki/Machine_code)** kompiliert wird, der auf deiner CPU läuft, sondern in **[CIL](https://en.wikipedia.org/wiki/Common_Intermediate_Language)**, welche eine CIL-kompatible Runtime für ihre Ausführung benötigt.

Dieser Ansatz hat enorme Vorteile, da CIL plattformunabhängig ist. Aus diesem Grund kann ASF nativ auf vielen verfügbaren Betriebssystemen, insbesondere Windows, Linux und macOS, ausgeführt werden. Es wird nicht nur keine Emulation benötigt, sondern auch Unterstützung für alle plattformbezogenen und hardwarebezogenen Optimierungen, wie z. B. CPU-SSE-Anweisungen. Dank dessen kann ASF eine überlegene Leistung und Optimierung erreichen, während es gleichzeitig eine perfekte Kompatibilität und Zuverlässigkeit bietet.

Das bedeutet auch, dass ASF **keine spezifische Betriebssystem-Anforderung** hat, weil es die funktionierende **Runtime** auf diesem Betriebssystem benötigt und nicht das Betriebssystem selbst. Solange diese Runtime ASF-Code korrekt ausführt, spielt es keine Rolle ob das zugrunde liegende Betriebssystem Windows, Linux, macOS, BSD, Sony Playstation 4, Nintendo Wii oder ein Toaster ist - solange es **[.NET Core dafür gibt](https://docs.microsoft.com/de-de/dotnet/)**, gibt es **[ASF](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** für dieses System.

Unabhängig davon, wo du ASF nutzt, musst du jedoch sicherstellen, dass auf deiner Zielplattform **[.NET-Voraussetzungen](https://github.com/dotnet/core/blob/main/Documentation/prereqs.md)** installiert sind. Diese sind Low-Level-Bibliotheken, welche für eine einwandfreie Laufzeitfunktionalität benötigt werden und unerlässlich für die bloße Funktion von ASF sind. Sehr wahrscheinlich hast du einige von ihnen (oder sogar alle) bereits installiert.

---

## ASF-Pakete

ASF gibt es in 2 Hauptformen - generisches Paket und betriebssystemspezifisch. Aus funktionaler Sicht sind beide Pakete genau gleich. Beide sind auch in der Lage, sich selbst automatisch zu aktualisieren. Der einzige Unterschied zwischen ihnen ist, ob zusätzlich zum **generischen** Paket von ASF auch eine **betriebssystemspezifische** Runtime enthalten ist oder nicht.

---

### Generisch

Das generische Paket ist ein plattformunabhängiger Build, der keinen maschinenspezifischen Code enthält. Dieses Setup erfordert, dass du die .NET Core Runtime bereits **in der entsprechenden Version** auf deinem Betriebssystem installiert hast. We all know how troublesome it is to keep dependencies up-to-date, therefore this package is here mainly for people that **already use** .NET and don't want to duplicate their runtime solely for ASF if they can make use of what they have installed already. Generic package also allows you to run ASF **anywhere, as long as you can obtain working implementation of .NET runtime**, regardless if there exists OS-specific ASF build for it, or not.

It's not recommended to use generic flavour if you're casual or even advanced user that just wants to make ASF work and not dig into .NET technical details. Mit anderen Worten - wenn du weißt, was das ist, kannst du es benutzen, sonst ist es viel besser, ein betriebssystemspezifisches Paket zu verwenden, das unten erklärt wird.

#### .NET Framework Paket

In addition to generic package mentioned above, there is also `generic-netf` package which is built on top of .NET Framework and not .NET (Core). This package is a legacy variant that provides missing compatibility known from ASF V2 times, and can be run e.g. with **[Mono](https://www.mono-project.com)**, while .NET `generic` package can't as of today.

Im Allgemeinen solltest du **dieses Paket so weit wie möglich vermeiden**, da die meisten Betriebssysteme und Setups perfekt (und viel besser) mit dem oben erwähnten ` generic ` Paket unterstützt werden. In fact, this package makes sense to be used only on platforms that lack working .NET runtime, while having working Mono implementation. Examples of such platforms include `linux-x86` (32-bit i386/i686 linux), as well as `linux-armel` (ARMv6 boards found e.g. in Raspberry Pi 0 & 1), all of which do not have official working .NET runtime as of today.

As the time goes on with more platforms being supported by .NET and less compatibility between .NET Framework and .NET, `generic-netf` package will be entirely replaced with `generic` one in the future. Please refrain from using it if you can use any .NET package instead, as `generic-netf` is missing a lot of functionality and compatibility compared to .NET versions, and it'll be only less functional as the time goes on. Wir bieten **nur** Unterstützung für dieses Paket auf Rechnern, die die obrige `generic` Variante nicht verwenden können (z. B. `linux-x86`), und nur mit aktueller Runtime (z. B. neuestes Mono).

---

### Betriebssystemspezifisch

Das betriebssystemspezifische Paket beinhaltet neben dem verwalteten Code, der im generischen Paket enthalten ist, auch nativen Code für die jeweilige Plattform. In other words, OS-specific package **already includes proper .NET runtime inside**, which allows you to entirely skip the whole installation mess and just launch ASF directly. Das betriebssystemspezifische Paket, wie man dem Namen entnehmen kann, ist betriebssystemspezifisch und jedes Betriebssystem benötigt eine eigene Version - zum Beispiel Windows benötigt PE32+ `ArchiSteamFarm.exe` Binärdatei während Linux mit Unix ELF `ArchiSteamFarm` Binärdatei arbeitet. Wie du vielleicht weißt, sind diese beiden Typen nicht miteinander kompatibel.

ASF ist derzeit in folgenden betriebsystemspezifischen Varianten verfügbar:

- `linux-arm` ist kompatibel mit 32-Bit-ARM-basierten (ARMv7+) GNU/Linux-Betriebssystemen. This includes platforms such as Raspberry Pi 2 (and newer) with all GNU/Linux OSes available for them (such as Raspberry Pi OS), in current and future versions. This variant will **not** work with older ARM architectures, such as ARMv6 found in Raspberry Pi 0 & 1, it will also not work with OSes that do not implement required GNU/Linux environment (such as Android).
- `linux-arm64` ist kompatibel mit 64-Bit-ARM-basierten (ARMv8+) GNU/Linux-Betriebssystemen. Dies beinhaltet sämtliche Plattformen, wie etwa Raspberry Pi 3 (oder Neuer) mit allen für diese verfügbaren AArch64 GNU/Linux OS (z. B. Raspbian), in der aktuellen bzw. zukünftigen Version. This variant will **not** work with 32-bit OSes that do not have required 64-bit libraries available (such as Raspberry Pi OS), it will also not work with OSes that do not implement required GNU/Linux environment (such as Android).
- `linux-x64` ist kompatibel mit 64-Bit GNU/Linux-Betriebssystemen. This includes Alpine, CentOS/Fedora/RHEL, Debian/Ubuntu, OpenSUSE/SLES and many other ones, including their derivatives, in current and future versions.
- `osx-arm64` funktioniert unter 64-Bit ARM-basierten (Apple silicon) macOS Betriebssystemen. This includes version 11, as well as future ones.
- `osx-x64` ist kompatibel mit 64-Bit macOS Betriebssystemen. This includes version 10.15, as well as future ones.
- `win-x64` ist kompatibel mit 64-Bit-Windows-Betriebssystemen. This includes 10, 11, Server 2012+ as well as future versions.

Of course, even if you don't have OS-specific package available for your OS-architecture combination, you can always install appropriate .NET runtime yourself and run generic ASF flavour, which is also the main reason why it exists in the first place. Generic ASF build is platform-agnostic and will run on any platform that has a working .NET runtime. This is important to note - ASF requires .NET runtime, not some specific OS or architecture. For example, if you're running 32-bit Windows then despite of no dedicated `win-x86` ASF version, you can still install .NET SDK in `win-x86` version and run generic ASF just fine. Wir können nicht jede Kombination aus Betriebssystem und Architektur ansprechen, die existiert und von jemandem verwendet wird, also müssen wir irgendwo eine Grenze ziehen. Ein gutes Beispiel für diese Grenze ist x86, da es sich um eine veraltete Architektur seit mindestens 2004 handelt.

For a complete list of all supported platforms and OSes by .NET 7.0, visit **[release notes](https://github.com/dotnet/core/blob/main/release-notes/7.0/supported-os.md)**.

---

## Runtime-Anforderungen

If you're using OS-specific package then you don't need to worry about runtime requirements, because ASF always ships with required and up-to-date runtime that will work properly as long as you have **[.NET prerequisites](https://github.com/dotnet/core/blob/main/Documentation/prereqs.md)** installed and up-to-date. In other words, **you don't need to install .NET runtime or SDK**, as OS-specific builds require only native OS dependencies (prerequisites) and nothing else.

However, if you're trying to run **generic** ASF package then you must ensure that your .NET runtime supports platform required by ASF.

ASF as a program is targeting **.NET 7.0** (`net7.0`) right now, but it may target newer platform in the future. `net7.0` is supported since 7.0.100 SDK (7.0.0 runtime), although ASF is configured to prefer **latest runtime at the moment of compilation**, so you should ensure that you have **[latest SDK](https://dotnet.microsoft.com/download)** (or at least runtime) available for your machine. Generic ASF variant may refuse to launch if your runtime is older than the specified minimum supported one during compilation.

Im Zweifelsfall solltest du überprüfen, was unsere **[kontinuierliche Integration](https://github.com/JustArchiNET/ArchiSteamFarm/actions/workflows/publish.yml?query=branch%3Amain)** für die Kompilierung und Bereitstellung von ASF-Versionen auf GitHub verwendet. You can find `dotnet --info` output in every build as part of .NET verification step.