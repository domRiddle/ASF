# Kompatibilität

ASF ist eine, auf .NET Core basierende, C#-Anwendung. Das bedeutet, dass ASF nicht direkt in **[Maschinencode](https://en.wikipedia.org/wiki/Machine_code)** kompiliert wird, der auf deiner CPU läuft, sondern in **[CIL](https://en.wikipedia.org/wiki/Common_Intermediate_Language)**, der eine CIL-kompatible Runtime für seine Ausführung benötigt.

Dieser Lösungsansatz hat enorme Vorteile, da CIL plattformunabhängig ist, weshalb ASF nativ auf vielen verfügbaren Betriebssystemen, insbesondere Windows, Linux und OS X, laufen kann. Es ist nicht nur keine Emulation erforderlich, sondern auch die Unterstützung aller plattform- und hardwarebezogenen Optimierungen, wie z.B. CPU SSE-Anweisungen. Dank dessen kann ASF eine überlegene Leistung und Optimierung erreichen, während es gleichzeitig eine perfekte Kompatibilität und Zuverlässigkeit bietet.

Das bedeutet auch, dass ASF **keine spezifische OS-Anforderung** hat, weil es die funktionierende **Runtime** auf diesem Betriebssystem benötigt und nicht das Betriebssystem selbst. Solange diese Runtime ASF-Code korrekt ausführt, spielt es keine Rolle ob das zugrunde liegende Betriebssystem Windows, Linux, OS X, BSD, Sony Playstation 4, Nintendo Wii oder dein Toaster ist - solange es **[.NET Core dafür gibt](https://github.com/dotnet/core-setup#daily-builds)**, gibt es **[ASF](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** für ihn.

Unabhängig davon von wo aus du ASF ausführst, musst du jedoch sicherstellen, dass auf deiner Zielplattform **[.NET Core Prerequisites](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)** installiert sind. Das sind Low-Level-Bibliotheken, die für eine einwandfreie Laufzeitfunktionalität erforderlich sind und der absolute Kern für die Funktionsfähigkeit von ASF. Sehr wahrscheinlich hast du einige von ihnen (oder sogar alle) bereits installiert.

* * *

## ASF-Verpackung

ASF gibt es in 2 Hauptformen - generisches Paket und betriebssystemspezifisch. Aus funktionaler Sicht sind beide Pakete genau gleich, sie sind beide auch in der Lage, sich selbst automatisch zu aktualisieren. Der einzige Unterschied zwischen ihnen ist, dass das **generische** Paket von ASF auch mit einer **betriebssystemspezifische** Runtime ausgestattet ist, um es zu betreiben.

* * *

### Generisch

Das generische Paket ist ein plattformunabhängiger Build, der keinen maschinenspezifischen Code enthält. Dieses Setup erfordert von dir, dass du die .NET Core Runtime bereits auf deinem Betriebssystem ** in der entsprechenden Version** installiert hast. Wir alle wissen, wie lästig es ist, Abhängigkeiten auf dem neuesten Stand zu halten, deshalb ist dieses Paket hauptsächlich für Leute gedacht, die .NET Core **bereits** verwenden und ihre Runtime nicht nur für ASF verdoppeln wollen, wenn sie das nutzen können, was sie bereits installiert haben. Das generische Paket ermöglicht es dir auch, ASF ** überall dort auszuführen, wo du eine funktionierende Implementierung der .NET Core Runtime** erreichen kannst, unabhängig davon, ob es einen betriebssystemspezifischen ASF Build dafür gibt oder nicht.

Es wird nicht empfohlen, den generischen Build zu verwenden, wenn du ein Gelegenheits- oder sogar fortgeschrittener Benutzer bist, der nur ASF zum Laufen bringen will und nicht in technische Details von .NET Core einsteigen möchte. Mit anderen Worten - wenn du weißt, was das ist, kannst du es benutzen, sonst ist es viel besser, ein betriebssystemspezifisches Paket zu verwenden, das unten erklärt wird.

#### .NET Framework Paket

Zusätzlich zu dem oben genannten generischen Paket gibt es auch das Paket `generic-netf`, das auf dem .NET Framework (und nicht dem .NET Core) basiert. Bei diesem Paket handelt es sich um eine ältere Variante, die eine aus ASF V2 bekannte fehlende Kompatibilität bietet und z.B. mit **[Mono](https://www.mono-project.com)** ausgeführt werden kann, während das ` generic ` .NET Core Paket dies derzeitig nicht kann.

Im Allgemeinen solltest du **dieses Paket so weit wie möglich vermeiden**, da die meisten Betriebssysteme und Setups perfekt (und viel besser) mit dem oben erwähnten ` generic ` Paket unterstützt werden. Tatsächlich ist es sinnvoll, dieses Paket nur auf Plattformen zu verwenden, auf denen die funktionierende .NET Core Runtime fehlt, während die Mono-Implementierung funktioniert. Ein Beispiel für eine solche Plattform wäre `linux-x86`, die bis heute keine funktionierende .NET Core Runtime erhalten hat.

Im Laufe der Zeit werden mehr Plattformen von .NET Core unterstützt und die Kompatibilität zwischen .NET Framework und .NET Core verringert, `generic-netf` Paket wird vollständig durch ` generic ` Paket ersetzt. Bitte verzichte darauf es zu verwenden, wenn du stattdessen ein anderes .NET Core Paket verwenden kannst, da `generic-netf` im Vergleich zu .NET Core Versionen viel Funktionalität und Kompatibilität fehlt und es im Laufe der Zeit nur weniger funktional sein wird. Wir bieten Unterstützung für dieses Paket nur auf Rechnern an, die die obige Variante `generic` nicht verwenden können (z.B. `linux-x86`), und nur mit aktueller Runtime (z.B. neueste Mono).

* * *

### Betriebssystemspezifisch

Das betriebssystemspezifische Paket beinhaltet neben dem verwalteten Code, der im generischen Paket enthalten ist, auch den nativen Code für die jeweilige Plattform. Mit anderen Worten, das betriebssystemspezifische Paket **beinhaltet bereits die richtige .NET Core Runtime**, was es dir ermöglicht, den gesamten Installationsprozess komplett zu überspringen und ASF einfach direkt zu starten. Das betriebssystemspezifische Paket, wie man dem Namen entnehmen kann, ist betriebssystemspezifisch und jedes Betriebssystem benötigt eine eigene Version - zum Beispiel Windows benötigt PE32+ `ArchiSteamFarm.exe` Binärdatei während Linux mit Unix ELF `ArchiSteamFarm` Binärdatei arbeitet. Wie du vielleicht weißt, sind diese beiden Typen nicht miteinander kompatibel.

ASF ist derzeit in folgenden Betriebsystem spezifischen Varianten verfügbar:

- `win-x64` ist kompatibel mit 64-Bit-Windows-Betriebssystemen. Dazu gehören Windows 7 (SP1+), 8.1, 10, Server 2008 R2 (SP1+), 2012, 2012 R2, 2016 sowie zukünftige Versionen.
- `linux-arm` ist kompatibel mit 32-Bit-ARM-basierten (ARMv7+) Linux-Betriebssystemen. Dazu gehören insbesondere Raspberry Pi 2 & 3 mit allen für diese erhältlichen glibc-basierten Linux-Betriebssystemen in aktuellen und zukünftigen Versionen. Diese Variante ist nicht kompatibel mit älteren ARM-Architekturen, wie beispielsweise ARMv6 in Raspberry Pi 0 & 1.
- `linux-x64` ist kompatibel mit 64-Bit glibc-basierten Linux-Betriebssystemen. Dazu gehören Alpine, CentOS/Fedora/RHEL, Debian/Ubuntu/Linux Mint, OpenSUSE/SLES und viele andere, einschließlich ihrer Derivate, in aktuellen und zukünftigen Versionen.
- `osx-x64` ist kompatibel mit 64-Bit OS X Betriebssystemen. Dazu gehört die Version 10.12 sowie zukünftige Versionen.

Selbst wenn du kein betriebssystemspezifisches Paket für deine Betriebssystem-Architektur-Kombination zur Auswahl hast, kannst du natürlich jederzeit selbst die entsprechende .NET Core Runtime installieren und die generische ASF-Version ausführen, was auch der Hauptgrund dafür ist, dass sie überhaupt existiert. Der generische ASF-Build ist plattformunabhängig und läuft auf jeder Plattform, die eine funktionierende .NET Core Runtime hat. Das ist wichtig zu beachten - ASF benötigt .NET Core Runtime, nicht ein bestimmtes Betriebssystem oder eine bestimmte Architektur. Wenn du zum Beispiel ein 32-Bit-Windows benutzt, dann kannst du trotz der fehlenden dedizierten `win-x86` ASF-Version immer noch das .NET Core SDK in der `win-x86`-Version installieren und die generische ASF-Version problemlos ausführen. Wir können nicht jede Kombination aus Betriebssystem und Architektur ansprechen, die existiert und von jemandem verwendet wird, also müssen wir irgendwo eine Linie ziehen. x86 ist ein gutes Beispiel für diese Linie, da es sich um eine veraltete Architektur seit mindestens 2004 handelt.

Für eine vollständige Liste aller unterstützten Plattformen und Betriebssysteme von .NET Core 2.1 besuche die **[Versionshinweise](https://github.com/dotnet/core/blob/master/release-notes/2.1/2.1-supported-os.md)**.

* * *

## Runtime-Anforderungen

Wenn du ein betriebssystemspezifisches Paket verwendest, musst du dir keine Sorgen um die Runtime-Anforderungen machen, denn ASF wird immer mit der erforderlichen und aktuellen Runtime ausgeliefert, die einwandfrei funktioniert, solange du die **[.NET Core Prerequisites](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)** installiert und auf dem neuesten Stand hast. Mit anderen Worten, **du musst die .NET Core Runtime oder SDK** nicht installieren, da betriebssystemspezifische Builds nur native Betriebssystemabhängigkeiten (Prerequisites) und nichts anderes erfordern.

Wenn du jedoch versuchst, das **generic** ASF-Paket auszuführen, dann musst du sicherstellen, dass deine .NET Core Runtime die von ASF benötigte Plattform unterstützt.

ASF als Programm richtet sich derzeit an **.NET Core 2.1** (`netcoreapp2.1`), könnte aber in Zukunft auch auf neuere Plattformen ausgerichtet sein. `netcoreapp2.1` wird seit 2.1.300 SDK (2.1.0 Runtime) unterstützt, obwohl wir empfehlen, **[das neueste SDK](https://www.microsoft.com/net/download)** zu verwenden, das für deinen Computer verfügbar ist.

Im Zweifelsfall solltest du überprüfen, was unsere **[kontinuierliche Integration](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)** für die Kompilierung und Bereitstellung von ASF-Versionen auf GitHub verwendet. Du findest die `dotnet --info` Ausgabe oben in jedem Build.

* * *

## Probleme und Lösungen

### Debian Jessie Aktualisierung

Wenn du von Debian 8 Jessie (oder älter) auf Debian 9 Stretch aktualisiert hast, stelle sicher, dass du **nicht** das `libssl1.0.0` Paket hast, zum Beispiel mit `apt-get purge libssl1.0.0`. Andernfalls besteht die Gefahr, dass du auf einen Segmentfehler stößt. Dieses Paket ist veraltet und existiert per Definition nicht, es ist auch nicht möglich, auf sauberen Debian 9-Setups zu installieren, der einzige Weg, dieses Problem zu lösen, ist ein Upgrade von Debian 8 oder älter - **[dotnet/corefx #8951](https://github.com/dotnet/corefx/issues/8951#issuecomment-314455190)**. Wenn du einige andere Pakete hast, die von dieser veralteten libssl-Version abhängig sind, dann solltest du sie entweder aktualisieren oder loswerden - nicht nur wegen dieses Problems, sondern auch, weil sie überhaupt auf einer veralteten Bibliothek basieren.

### Ungültige Maschinenzertifikate mit ASF V3.2+

Möglicherweise tritt unter Linux ein spezielles Problem im Zusammenhang mit SSL-Zertifikaten auf, die von deinem Computer bereitgestellt werden. Dies wird als Exception unten angezeigt:

```csharp
System.Net.Http.HttpRequestException: The SSL connection could not be established, see inner exception. ---> Interop+Crypto+OpenSslCryptographicException: error:2006D002:BIO routines:BIO_new_file:system lib
   at Interop.Crypto.CheckValidOpenSslHandle(SafeHandle handle)
```

Dieses Problem wird höchstwahrscheinlich dadurch verursacht, dass einige der Zertifikate nicht gelesen werden können. Dies kann an unzureichenden Berechtigungen liegen (als Test kannst du überprüfen, ob ASF unter `root` funktioniert) oder an anderen Problemen beim Einlesen ungültiger Zertifikatsdateien.

Der aktuelle Workaround hängt vom Hauptproblem ab. Bei sauberen Setups (z.B. Debian) sollte dies niemals passieren, da es keine ungültigen/sensitiven Zertifikate im Speicher geben sollte, so dass das Problem nur Personen berücksichtigt, die höchstwahrscheinlich andere Zertifikate für andere Zwecke haben (z.B. VPN oder Websites), auf die ASF keinen Zugriff hat. Du kannst in Betracht ziehen, entweder dem ASF-Benutzer die entsprechenden Berechtigungen zu erteilen (z.B. sicherzustellen, dass alle SSL-Zertifikate in `/etc/pki` und `/etc/ssl` mindestens `644` globale Leserechte haben), ASF unter `root` auszuführen oder sensible Zertifikate an einen anderen Ort zu verschieben, damit ASF nicht versucht, sie während der Initialisierung zu lesen.

Dieses Problem soll mit der nächsten .NET Core 2.1.1.1 Wartungsversion behoben werden, daher sollten die oben beschriebenen Workarounds in naher Zukunft nicht mehr erforderlich sein.

Referenz: **[dotnet/corefx #29942](https://github.com/dotnet/corefx/issues/29942)**.

### .NET Core Runtime wählt falsche `libcurl.so` Bibliothek

Wenn du sowohl `libcurl.so.3` als auch `libcurl.so.4` auf deinem System hast, dann könnte .NET Core sich entscheiden, die zweite zu wählen, was zum ASF-Absturz führt, sobald es versucht, seinen http-Client zu initialisieren.

```csharp
OnUnhandledException() System.TypeInitializationException: The type initializer for 'System.Net.Http.CurlHandler' threw an exception. ---> System.TypeInitializationException: The type initializer for 'Http' threw an exception. ---> System.TypeInitializationException: The type initializer for 'HttpInitializer' threw an exception. ---> System.DllNotFoundException: Unable to load DLL 'System.Net.Http.Native': The specified module or one of its dependencies could not be found.
```

Wenn du auf das Problem oben stolperst, dann musst du möglicherweise manuell .NET Core Runtime anweisen, die richtige Bibliothek in diesem Fall auszuwählen. Lokalisiere `libcurl.so.3` auf deinem System und füge es zu `LD_PRELOAD` hinzu, bevor du ASF startest:

```shell
LD_PRELOAD=/usr/lib/libcurl.so.3 ./ArchiSteamFarm
```

Dies sollte hoffentlich das Problem lösen, vorausgesetzt, dass deine `libcurl.so.3` ordnungsgemäß funktioniert.