# Kompatibilität

ASF ist eine, auf .NET Core basierende, C#-Anwendung. Das bedeutet, dass ASF nicht direkt in **[Maschinencode](https://en.wikipedia.org/wiki/Machine_code)** kompiliert wird, der auf deiner CPU läuft, sondern in **[CIL](https://en.wikipedia.org/wiki/Common_Intermediate_Language)**, der eine CIL-kompatible Runtime für seine Ausführung benötigt.

Dieser Lösungsansatz hat enorme Vorteile, da CIL plattformunabhängig ist, weshalb ASF nativ auf vielen verfügbaren Betriebssystemen, insbesondere Windows, Linux und OS X, laufen kann. Es ist nicht nur keine Emulation erforderlich, sondern es werden auch alle plattform- und hardwarebezogenen Optimierungen, wie z.B. CPU SSE-Anweisungen unterstützt. Dank dessen kann ASF eine überlegene Leistung und Optimierung erreichen, während es gleichzeitig eine perfekte Kompatibilität und Zuverlässigkeit bietet.

Das bedeutet auch, dass ASF **keine spezifische Betriebssystem-Anforderung** hat, weil es die funktionierende **Runtime** auf diesem Betriebssystem benötigt und nicht das Betriebssystem selbst. Solange diese Runtime ASF-Code korrekt ausführt, spielt es keine Rolle ob das zugrunde liegende Betriebssystem Windows, Linux, OS X, BSD, Sony Playstation 4, Nintendo Wii oder dein Toaster ist - solange es **[.NET Core dafür gibt](https://github.com/dotnet/core-setup#daily-builds)**, gibt es **[ASF](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** für dieses System.

Aber je nachdem auf welcher Plattform du ASF ausführst, musst du sicherstellen, dass auf deiner Zielplattform die **[.NET Core Prerequisites](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)** installiert sind. Das sind Low-Level-Bibliotheken, die für eine einwandfreie Laufzeitfunktionalität erforderlich sind und absolut notwendig für die Funktionsfähigkeit von ASF. Sehr wahrscheinlich hast du einige von ihnen (oder sogar alle) bereits installiert.

* * *

## ASF-Pakete

ASF gibt es in 2 Hauptformen - generisches Paket und betriebssystemspezifisch. Aus funktionaler Sicht sind beide Pakete genau gleich. Beide sind auch in der Lage, sich selbst automatisch zu aktualisieren. Der einzige Unterschied zwischen ihnen ist, ob zusätzlich zum **generischen** Paket von ASF auch eine **betriebssystemspezifische** Runtime enthalten ist oder nicht.

* * *

### Generisch

Das generische Paket ist ein plattformunabhängiger Build, der keinen maschinenspezifischen Code enthält. Dieses Setup erfordert von dir, dass du die .NET Core Runtime bereits auf deinem Betriebssystem ** in der entsprechenden Version** installiert hast. Wir alle wissen, wie lästig es ist, Abhängigkeiten auf dem neuesten Stand zu halten, deshalb ist dieses Paket hauptsächlich für Leute gedacht, die .NET Core **bereits** verwenden und diese Runtime nicht explizit für ASF zusätzlich pflegen wollen, wenn sie das nutzen können, was sie bereits installiert haben. Das generische Paket ermöglicht es dir auch, ASF ** überall dort auszuführen, wo du eine funktionierende Implementierung der .NET Core Runtime zur Verfügung hast**, unabhängig davon, ob es einen betriebssystemspezifischen ASF Build dafür gibt oder nicht.

Es wird nicht empfohlen, den generischen Build zu verwenden, wenn du ein Gelegenheits- oder sogar fortgeschrittener Benutzer bist, der nur ASF zum Laufen bringen will und nicht in technische Details von .NET Core einsteigen möchte. Mit anderen Worten - wenn du weißt, was das ist, kannst du es benutzen, sonst ist es viel besser, ein betriebssystemspezifisches Paket zu verwenden, das unten erklärt wird.

#### .NET Framework Paket

Zusätzlich zu dem oben genannten generischen Paket gibt es auch das Paket `generic-netf`, das auf dem .NET Framework (und nicht dem .NET Core) basiert. Bei diesem Paket handelt es sich um eine veraltete Variante, die eine, bereits aus ASF V2 bekannte, fehlende Kompatibilität bietet und z.B. mit **[Mono](https://www.mono-project.com)** ausgeführt werden kann, während das mit dem `generic` .NET Core Paket derzeit nicht möglich ist.

Im Allgemeinen solltest du **dieses Paket so weit wie möglich vermeiden**, da die meisten Betriebssysteme und Setups perfekt (und viel besser) mit dem oben erwähnten ` generic ` Paket unterstützt werden. Tatsächlich ist es sinnvoll, dieses Paket nur auf Plattformen zu verwenden, auf denen es keine funktionierende .NET Core Runtime gibt, aber eine Mono-Implementierung bieten. Ein Beispiel für eine solche Plattform wäre `linux-x86`, die bis heute keine funktionierende .NET Core Runtime erhalten hat.

Im Laufe der Zeit werden mehr Plattformen von .NET Core unterstützt und die Kompatibilität zwischen .NET Framework und .NET Core wird sich verringern. In der Zukunft wird das `generic-netf` Paket vollständig durch das `generic` Paket ersetzt werden. Bitte verzichte darauf es zu verwenden, wenn du stattdessen ein anderes .NET Core Paket verwenden kannst, da `generic-netf` im Vergleich zu .NET Core Versionen viel Funktionalität und Kompatibilität fehlt und es in Zukunft immer weniger funktional sein wird. Wir bieten Unterstützung für dieses Paket nur auf Rechnern an, die die obige Variante `generic` nicht verwenden können (z.B. `linux-x86`), und nur mit aktueller Runtime (z.B. neueste Mono).

* * *

### Betriebssystemspezifisch

Das betriebssystemspezifische Paket beinhaltet neben dem verwalteten Code, der im generischen Paket enthalten ist, auch nativen Code für die jeweilige Plattform. Mit anderen Worten, das betriebssystemspezifische Paket **beinhaltet bereits die richtige .NET Core Runtime**, was es dir ermöglicht, den gesamten Installationsprozess komplett zu überspringen und ASF einfach direkt zu starten. Das betriebssystemspezifische Paket, wie man dem Namen entnehmen kann, ist betriebssystemspezifisch und jedes Betriebssystem benötigt eine eigene Version - zum Beispiel Windows benötigt PE32+ `ArchiSteamFarm.exe` Binärdatei während Linux mit Unix ELF `ArchiSteamFarm` Binärdatei arbeitet. Wie du vielleicht weißt, sind diese beiden Typen nicht miteinander kompatibel.

ASF ist derzeit in folgenden betriebsystemspezifischen Varianten verfügbar:

- `win-x64` ist kompatibel mit 64-Bit-Windows-Betriebssystemen. Dazu gehören Windows 7 (SP1+), 8.1, 10, Server 2008 R2 (SP1+), 2012, 2012 R2, 2016 sowie zukünftige Versionen.
- `linux-arm` ist kompatibel mit 32-Bit-ARM-basierten (ARMv7+) GNU/Linux-Betriebssystemen. Dazu gehören insbesondere Raspberry Pi 2 & 3 mit allen für diese erhältlichen GNU/Linux-Betriebssystemen (wie zum Beispiel Raspbian), in aktuellen und zukünftigen Versionen. Diese Variante funktioniert nicht mit älteren ARM-Architekturen, wie z.B. ARMv6 in Raspberry Pi 0 & 1, sie funktioniert auch nicht mit Betriebssystemen, die nicht die erforderlichen GNU/Linux-Funktionen implementieren, wie z.B. Android.
- `linux-x64` ist kompatibel mit 64-Bit GNU/Linux-Betriebssystemen. Dazu gehören Alpine, CentOS/Fedora/RHEL, Debian/Ubuntu/Linux Mint, OpenSUSE/SLES und viele andere, einschließlich ihrer Derivate, in aktuellen und zukünftigen Versionen.
- `osx-x64` ist kompatibel mit 64-Bit OS X Betriebssystemen. Dazu gehört die Version 10.12 sowie zukünftige Versionen.

Selbst wenn du kein betriebssystemspezifisches Paket für deine Betriebssystem-Architektur-Kombination zur Auswahl hast, kannst du natürlich jederzeit selbst die entsprechende .NET Core Runtime installieren und die generische ASF-Version ausführen, was auch der Hauptgrund dafür ist, dass diese überhaupt existiert. Der generische ASF-Build ist plattformunabhängig und läuft auf jeder Plattform, die eine funktionierende .NET Core Runtime hat. Wichtig ist: ASF benötigt die .NET Core Runtime und nicht ein bestimmtes Betriebssystem oder eine bestimmte Architektur. Wenn du zum Beispiel ein 32-Bit-Windows benutzt, dann kannst du trotz der fehlenden dedizierten `win-x86` ASF-Version immer noch das .NET Core SDK in der `win-x86`-Version installieren und die generische ASF-Version problemlos ausführen. Wir können nicht jede Kombination aus Betriebssystem und Architektur ansprechen, die existiert und von jemandem verwendet wird, also müssen wir irgendwo eine Grenze ziehen. Ein gutes Beispiel für diese Grenze ist x86, da es sich um eine veraltete Architektur seit mindestens 2004 handelt.

Für eine vollständige Liste aller unterstützten Plattformen und Betriebssysteme von .NET Core 2.2 besuche die **[Versionshinweise](https://github.com/dotnet/core/blob/master/release-notes/2.2/2.2-supported-os.md)**.

* * *

## Runtime-Anforderungen

Wenn du ein betriebssystemspezifisches Paket verwendest, musst du dir keine Sorgen um die Runtime-Anforderungen machen, denn ASF wird immer mit der erforderlichen und aktuellen Runtime ausgeliefert, die einwandfrei funktioniert, solange du die **[.NET Core Prerequisites](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)** installiert und auf dem neuesten Stand hast. Mit anderen Worten, **du musst die .NET Core Runtime oder SDK** nicht installieren, da betriebssystemspezifische Builds nur native Betriebssystemabhängigkeiten (Prerequisites) und nichts anderes erfordern.

Wenn du jedoch versuchst, das **generische** ASF-Paket auszuführen, dann musst du sicherstellen, dass deine .NET Core Runtime die von ASF benötigte Plattform unterstützt.

ASF als Programm richtet sich derzeit an **.NET Core 2.2** (`netcoreapp2.2`), könnte aber in Zukunft auch auf neuere Plattformen ausgerichtet sein. `netcoreapp2.2` wird seit 2.2.100 SDK (2.2.0 Runtime) unterstützt, obwohl ASF konfiguriert ist, um ** die letzte Runtime zum Zeitpunkt der Kompilierung** zu verwenden, also solltest du sicherstellen, dass du **[die neueste SDK](https://www.microsoft.com/net/download)** für deine Maschine zur Verfügung hast. Die generische ASF-Variante kann den Start verweigern, wenn deine Runtime älter ist als die minimale (Ziel-) Runtime, die während der Kompilierung bekannt ist.

Im Zweifelsfall solltest du überprüfen, was unsere **[kontinuierliche Integration](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)** für die Kompilierung und Bereitstellung von ASF-Versionen auf GitHub verwendet. Dort findest du die `dotnet --info` Ausgabe oben in jedem Build.

* * *

## Probleme und Lösungen

### Debian Jessie Aktualisierung

Wenn du von Debian 8 Jessie (oder älter) auf Debian 9 Stretch aktualisiert hast, stelle sicher, dass du **nicht** das `libssl1.0.0` Paket hast, zum Beispiel mit `apt-get purge libssl1.0.0`. Andernfalls besteht die Gefahr, dass du auf einen Segmentfehler stößt. Dieses Paket ist veraltet und existiert per Definition nicht, es ist auch nicht möglich, es auf sauberen Debian 9-Setups zu installieren. Der einzige Weg auf dieses Problem zu sto&szlig;en ist ein Upgrade von Debian 8 oder älter - **[dotnet/corefx #8951](https://github.com/dotnet/corefx/issues/8951#issuecomment-314455190)**. Wenn du einige andere Pakete hast, die von dieser veralteten libssl-Version abhängig sind, dann solltest du sie entweder aktualisieren oder loswerden - nicht nur wegen dieses Problems, sondern auch, weil sie auf einer veralteten Bibliothek basieren.