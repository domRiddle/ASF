# Kompatibilität

ASF ist eine C#-Anwendung, welche mit der .NET-Plattform ausgeführt wird. Das bedeutet, dass ASF nicht direkt in **[Maschinencode](https://en.wikipedia.org/wiki/Machine_code)** kompiliert wird, der auf der CPU läuft, sondern in **[CIL](https://de.wikipedia.org/wiki/Common_Intermediate_Language)**, welche eine CIL-kompatible Runtime für dessen Ausführung benötigt.

Dieser Ansatz hat enorme Vorteile, da CIL plattformunabhängig ist. Aus diesem Grund kann ASF nativ auf vielen verfügbaren Betriebssystemen, insbesondere Windows, Linux und macOS, ausgeführt werden. Es wird nicht nur keine Emulation benötigt, sondern auch Unterstützung für alle plattformbezogenen und hardwarebezogenen Optimierungen, wie z. B. CPU-SSE-Anweisungen. Dank dessen kann ASF eine überlegene Leistung und Optimierung erreichen, während es gleichzeitig eine perfekte Kompatibilität und Zuverlässigkeit bietet.

Das bedeutet auch, dass ASF **keine spezifische Betriebssystem-Anforderung** hat, weil es die funktionierende **Runtime** auf diesem Betriebssystem benötigt und nicht das Betriebssystem selbst. Solange die Laufzeitumgebung den ASF-Code korrekt ausführt, spielt es keine Rolle, ob das Betriebssystem Windows, Linux ist macOS, BSD, Sony Playstation 4, Nintendo Wii oder ein Toaster ist - solange es **[.NET](https://dotnet.microsoft.com/download/dotnet)** dafür gibt, existiert **[ASF](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** dafür (als generische Variante).

Unabhängig davon, wo Sie ASF ausführen, müssen Sie jedoch sicherstellen, dass auf ihrer Zielplattform **[.NET-Voraussetzungen](https://github.com/dotnet/core/blob/main/Documentation/prereqs.md)** installiert sind. Diese sind Low-Level-Bibliotheken, welche für eine einwandfreie Laufzeitfunktionalität benötigt werden und unerlässlich für die bloße Funktion von ASF sind. Sehr wahrscheinlich haben Sie bereits einige (oder sogar alle) davon installiert.

---

## ASF-Pakete

ASF gibt es in 2 Hauptformen - generisches Paket und betriebssystemspezifisch. Aus funktionaler Sicht sind beide Pakete genau gleich. Beide sind auch in der Lage, sich selbst automatisch zu aktualisieren. Der einzige Unterschied zwischen ihnen ist, ob zusätzlich zum **generischen** Paket von ASF auch eine **betriebssystemspezifische** Runtime enthalten ist oder nicht.

---

### Generisch

Das generische Paket ist ein plattformunabhängiger Build, der keinen maschinenspezifischen Code enthält. Dieses Setup erfordert, dass Sie die .NET Core Runtime bereits **in der entsprechenden Version** auf dem Betriebssystem installiert haben. Wir alle wissen, wie lästig es ist, Abhängigkeiten auf dem neuesten Stand zu halten, deshalb ist dieses Paket hauptsächlich für Leute gedacht, die .NET **bereits** verwenden und diese Runtime nicht explizit für ASF zusätzlich pflegen wollen; wenn sie das nutzen können, was sie bereits installiert haben. Das generische Paket ermöglicht es ihnen auch, ASF ** überall dort auszuführen, solange eine funktionierende Implementierung der .NET Runtime zur Verfügung steht**, unabhängig davon, ob es einen betriebssystemspezifischen ASF Build dafür gibt oder nicht.

Es wird nicht empfohlen, den generischen Build zu verwenden, wenn Sie ein Gelegenheits- oder sogar fortgeschrittener Benutzer sind, der nur ASF zum Laufen bringen will und sich nicht über technische Details (von .NET) befassen möchten. Mit anderen Worten - wenn Sie wissen, worauf Sie sich damit einlassen, dann können Sie es benutzen; sonst ist es viel besser, ein betriebssystemspezifisches Paket zu verwenden, das unten erklärt wird.

#### .NET Framework Paket

Zusätzlich zu dem oben genannten generischen Paket gibt es auch `generic-netf` Paket, das grundlegend auf .NET Framework und nicht .NET (Core). Bei diesem Paket handelt es sich um eine veraltete Variante, die eine, bereits aus ASF V2 bekannte, fehlende Kompatibilität bietet und z.B. mit **[Mono](https://www.mono-project.com)** ausgeführt werden kann, während das mit dem `generic` .NET-Paket derzeit nicht möglich ist.

Im Allgemeinen sollten Sie **dieses Paket so weit wie möglich vermeiden**, da die meisten Betriebssysteme und Setups perfekt (und viel besser) mit dem oben erwähnten ` generic ` Paket unterstützt werden. Tatsächlich ist es sinnvoll, dieses Paket nur auf Plattformen zu verwenden, auf denen es keine funktionierende .NET Runtime gibt, aber zuindest eine Mono-Implementierung bieten. Beispiele für solche Plattformen sind `linux-x86` (32-bit i386/i686 linux), sowie `linux-armel` (ARMv6-Boards wie z.b. der Raspberry Pi 0 & 1), die alle bislang kein offiziell funktionierendes .NET Runtime haben.

Im Laufe der Zeit werden mehr Plattformen von .NET unterstützt und die Kompatibilität zwischen .NET Framework und .NET wird sich verringern. In der Zukunft wird das `generic-netf` Paket vollständig durch das `generic` Paket ersetzt werden. Bitte verzichten Sie auf die Verwendung dieser Variante, wenn Sie stattdessen ein .NET Paket verwenden können; da `generic-netf` im Vergleich zu .NET Versionen viel Funktionalität und Kompatibilität einbüßt und es in Zukunft immer weniger funktional sein wird. Wir bieten **nur** Unterstützung für dieses Paket auf Rechnern, die die obrige `generic` Variante nicht verwenden können (z. B. `linux-x86`), und nur mit aktueller Runtime (z. B. neuestes Mono).

---

### Betriebssystemspezifisch

Das betriebssystemspezifische Paket beinhaltet neben dem verwalteten Code, der im generischen Paket enthalten ist, auch nativen Code für die jeweilige Plattform. Mit anderen Worten, das betriebssystemspezifische Paket **beinhaltet bereits die richtige .NET Runtime**, was es ihnen ermöglicht, den gesamten Installationsprozess komplett zu überspringen und ASF einfach direkt zu starten. Das betriebssystemspezifische Paket, wie man dem Namen entnehmen kann, ist betriebssystemspezifisch und jedes Betriebssystem benötigt eine eigene Version - zum Beispiel Windows benötigt PE32+ `ArchiSteamFarm.exe` Binärdatei während Linux mit Unix ELF `ArchiSteamFarm` Binärdatei arbeitet. Wie Sie vielleicht wissen, sind diese beiden Typen nicht miteinander kompatibel.

ASF ist derzeit in folgenden betriebsystemspezifischen Varianten verfügbar:

- `linux-arm` arbeitet unter 32-bit ARM-basierten (ARMv7+) GNU/Linux-Betriebssystemen mit glibc 2.27 und neuer. Diese Variante deckt Plattformen wie Raspberry Pi 2 (und neuer) ab, sie funktioniert **nicht** mit älteren ARM-Architekturen (wie ARMv6 in Raspberry Pi 0 & 1 vorhanden); es funktioniert auch nicht mit Betriebssystemen, die nicht die erforderliche GNU/Linux-Umgebung implementieren (etwa Android).
- `linux-arm64` arbeitet unter 64-Bit-ARM-basierten (ARMv8+) GNU/Linux-Betriebssystemen mit glibc 2.23/musl 1.2.2 und neuer. Diese Variante umfasst Plattformen wie Raspberry Pi 3 (und neuer), es wird **nicht** mit 32-Bit Betriebssystemen arbeiten, die nicht über erforderliche 64-Bit Bibliotheken verfügen (z. B. 32-Bit Raspberry Pi OS), es funktioniert auch nicht mit Betriebssystemen, die nicht die erforderliche GNU/Linux-Umgebung implementieren (wie Android).
- `linux-x64` arbeitet unter 64-Bit-GNU/Linux-Betriebssystemen mit glibc 2.17/musl 1.2.2 und neuer.
- `osx-arm64` arbeitet unter 64-Bit-ARM-basierten (Apple silicon) macOS Betriebssystemen in Version 11 und neuer.
- `osx-x64` funktioniert auf 64-Bit-MacOS-Betriebssystemen in Version 10.15 und neuer.
- `win-arm64` funktioniert unter 64-Bit-ARM-basierten (ARMv8+) Windows-Betriebssystemen in Version 10, 11 und neuer.
- `win-x64` funktioniert unter 64-Bit-Windows-Betriebssystemen in Version 10, 11, Server 2012+ und neuer.

Selbst wenn Sie kein betriebssystemspezifisches Paket für Ihre Betriebssystem-Architektur-Kombination zur Auswahl haben, können Sie natürlich jederzeit selbst die entsprechende .NET Runtime installieren und die generische ASF-Version ausführen, was auch der Hauptgrund dafür ist, dass diese überhaupt existiert. Generic ASF build is platform-agnostic and will run on any platform that has a working .NET runtime. This is important to note - ASF requires .NET runtime, not some specific OS or architecture. For example, if you're running 32-bit Windows then despite of no dedicated `win-x86` ASF version, you can still install .NET SDK in `win-x86` version and run generic ASF just fine. Wir können nicht jede Kombination aus Betriebssystem und Architektur ansprechen, die existiert und von jemandem verwendet wird, also müssen wir irgendwo eine Grenze ziehen. Ein gutes Beispiel für diese Grenze ist x86, da es sich um eine veraltete Architektur seit mindestens 2004 handelt.

For a complete list of all supported platforms and OSes by .NET 7.0, visit **[release notes](https://github.com/dotnet/core/blob/main/release-notes/7.0/supported-os.md)**.

---

## Runtime-Anforderungen

If you're using OS-specific package then you don't need to worry about runtime requirements, because ASF always ships with required and up-to-date runtime that will work properly as long as you have **[.NET prerequisites](https://github.com/dotnet/core/blob/main/Documentation/prereqs.md)** installed and up-to-date. In other words, **you don't need to install .NET runtime or SDK**, as OS-specific builds require only native OS dependencies (prerequisites) and nothing else.

However, if you're trying to run **generic** ASF package then you must ensure that your .NET runtime supports platform required by ASF.

ASF as a program is targeting **.NET 7.0** (`net7.0`) right now, but it may target newer platform in the future. `net7.0` is supported since 7.0.100 SDK (7.0.0 runtime), although ASF might prefer **latest runtime at the moment of compilation**, so you should ensure that you have **[latest SDK](https://dotnet.microsoft.com/download)** (or at least runtime) available for your machine. Generic ASF variant may refuse to launch if your runtime is older than the specified minimum supported one during compilation.

Im Zweifelsfall solltest du überprüfen, was unsere **[kontinuierliche Integration](https://github.com/JustArchiNET/ArchiSteamFarm/actions/workflows/publish.yml?query=branch%3Amain)** für die Kompilierung und Bereitstellung von ASF-Versionen auf GitHub verwendet. You can find `dotnet --info` output in every build as part of .NET verification step.