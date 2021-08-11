# Kompabilität

ASF ist eine, auf .NET Core basierende, C#-Anwendung. Das bedeutet, dass ASF nicht direkt in **[Maschinencode](https://en.wikipedia.org/wiki/Machine_code)** kompiliert wird, der auf einer CPU läuft, sondern in **[CIL](https://en.wikipedia.org/wiki/Common_Intermediate_Language)**, der eine CIL-kompatible Runtime für seine Ausführung benötigt.

Dieser Lösungsansatz hat enorme Vorteile, da CIL plattformunabhängig ist, weshalb ASF nativ auf vielen verfügbaren Betriebssystemen, insbesondere Windows, Linux und OS X, laufen kann. Es ist nicht nur keine Emulation erforderlich, sondern es werden auch alle plattform- und hardwarebezogenen Optimierungen, wie z. B. CPU SSE-Anweisungen unterstützt. Dank dessen kann ASF eine überlegene Leistung und Optimierung erreichen, während es gleichzeitig eine perfekte Kompatibilität und Zuverlässigkeit bietet.

Das bedeutet auch, dass ASF **keine spezifische Betriebssystem-Anforderung** hat, weil es die funktionierende **Runtime** auf diesem Betriebssystem benötigt und nicht das Betriebssystem selbst. Solange diese Runtime ASF-Code korrekt ausführt, spielt es keine Rolle ob das zugrunde liegende Betriebssystem Windows, Linux, OS X, BSD, Sony Playstation 4, Nintendo Wii oder ein Toaster ist - solange es **[.NET Core dafür gibt](https://github.com/dotnet/core-setup#daily-builds)**, gibt es **[ASF](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** für dieses System.

Aber je nachdem auf welcher Plattform Sie ASF ausführen, müssen Sie sicherstellen, dass auf der Zielplattform die **[.NET Core Prerequisites](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)** installiert sind. Das sind Low-Level-Bibliotheken, die für eine einwandfreie Laufzeitfunktionalität erforderlich sind und absolut notwendig für die Funktionsfähigkeit von ASF. Sehr wahrscheinlich hast du einige von ihnen (oder sogar alle) bereits installiert.

---

## Mehrere Instanzen

ASF unterstützt das Ausführen mehrerer Instanzen des Prozesses auf derselben Maschine. Die Instanzen können komplett eigenständig oder von demselben binären Speicherort abgeleitet werden (in diesem Fall sollten Sie sie mit verschiedenen `--path` **[Kommandozeilenargumenten](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments-de-DE)**) ausführen.

Wenn Sie mehrere Instanzen von derselben Binärdatei ausführen, ist zu beachten, dass das normalerweise automatische Updates deaktiviert werden sollte, da es keine Synchronisation zwischen diese im Bezug auf Auto-Updates gibt. Wenn Sie automatische Updates aktivieren möchten, empfehlen wir Standalone-Instanzen aber Sie können trotzdem dafür sorgen, dass Updates funktionieren, solange Sie sicherstellen können, dass alle anderen ASF-Instanzen geschlossen sind.

ASF wird sein Bestes tun, um eine minimale Menge an OS-übergreifender Kommunikation mit anderen ASF-Instanzen zu pflegen. Dazu gehört die Überprüfung des Konfigurationsverzeichnisses mit denen anderer Instanzen, sowie die Freigabe prozessweiter Begrenzer mit `*LimiterDelay` **[globaler Konfiguration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-de-DE#globale-konfiguration)**, um sicherzustellen, dass das Ausführen mehrerer ASF-Instanzen kein Erreichen des Anfragelimits verursacht. In Bezug auf technische Aspekte verwenden alle Plattformen unseren dedizierten Mechanismus von benutzerdefinierten ASF-Dateisperren, die im temporären Verzeichnis erstellt wurden, `C:\Users\<YourUser>\AppData\Local\Temp\ASF` unter Windows, und `/tmp/ASF` unter Unix.

Es ist nicht erforderlich die gleiche `*LimiterDelay` Eigenschaften zu teilen um ASF Instanzen auszuführen, sie können verschiedene Werte verwenden, da jeder ASF nach dem Erwerb der Sperre seine eigene konfigurierte Verzögerung zur Release-Zeit hinzufügt. Wenn `*LimiterDelay` auf `0` gesetzt ist, wird die ASF-Instanz das Warten auf die Sperre der angegebenen Ressource, die mit anderen Instanzen geteilt wird, überspringen (was möglicherweise immer noch eine gemeinsame Sperre untereinander aufrecht erhalten könnte). Wenn auf einen beliebigen anderen Wert gesetzt, wird ASF sich korrekt mit anderen ASF-Instanzen synchronisieren, und wartet bis es an der Reihe ist, bevor es dann nach konfigurierter Verzögerung die Sperre aufhebt, wodurch andere Instanzen fortfahren können.

ASF berücksichtigt die `WebProxy` Einstellung bei der Entscheidung über den gemeinsamen Anwendungsbereich, was bedeutet, dass zwei ASF-Instanzen, die verschiedene `WebProxy` Konfigurationen verwenden, ihre Begrenzungen nicht untereinander teilen werden. Dies wird implementiert, um `WebProxy` zu ermöglichen, ohne übermäßige Verzögerungen zu arbeiten, wie von verschiedenen Netzwerkschnittstellen erwartet. Dies sollte für die Mehrzahl der Anwendungsfälle gut genug sein. Wenn Sie jedoch eine spezifische Benutzerkonfiguration haben, in der Sie z. B. Anfragen über eine bestimmte Netzwerkgruppe weiterleiten, können Sie das **[Kommandozeilenargument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments-de-DE)**`--network-group` angeben, welches Ihnen erlaubt, eine ASF-Gruppe zu deklarieren, die mit dieser Instanz synchronisiert wird. Beachten Sie, dass ausschließlich benutzerdefinierte Netzwerkgruppen verwendet werden. Das bedeutet, dass ASF `WebProxy` nicht mehr zur Bestimmung der richtigen Gruppe verwenden wird, da Sie für die Gruppierung in diesem Fall verantwortlich sind.

---

## ASF-Pakete

ASF gibt es in 2 Hauptformen - als generisches Paket und betriebssystemspezifisch. Aus funktionaler Sicht sind beide Pakete identisch. Beide sind auch in der Lage, sich selbst automatisch zu aktualisieren. Der einzige Unterschied zwischen ihnen ist, ob zusätzlich zum **generischen** Paket von ASF auch eine **betriebssystemspezifische** Runtime enthalten ist oder nicht.

---

### Generisch

Das generische Paket ist ein plattformunabhängiger Build, der keinen maschinenspezifischen Code enthält. Dieses Setup erfordert von dir, dass du die .NET Core Runtime bereits auf deinem Betriebssystem ** in der entsprechenden Version** installiert hast. Wir alle wissen, wie lästig es ist, Abhängigkeiten auf dem neuesten Stand zu halten, deshalb ist dieses Paket hauptsächlich für Leute gedacht, die .NET Core **bereits** verwenden und diese Runtime nicht explizit für ASF zusätzlich pflegen wollen, wenn sie das nutzen können, was sie bereits installiert haben. Das generische Paket ermöglicht es dir auch, ASF ** überall dort auszuführen, wo du eine funktionierende Implementierung der .NET Core Runtime zur Verfügung hast**, unabhängig davon, ob es einen betriebssystemspezifischen ASF Build dafür gibt oder nicht.

Es wird nicht empfohlen, den generischen Build zu verwenden, wenn Sie ein Gelegenheits- oder sogar fortgeschrittener Benutzer sind, der nur ASF zum Laufen bringen will und nicht in technische Details von .NET Core einsteigen möchte. Mit anderen Worten - wenn Sie wissen, was das ist, können Sie es benutzen, sonst ist es viel besser, ein betriebssystemspezifisches Paket zu verwenden. Mehr Informationen dazu gibt es in den folgenden beiden Abschnitten.

#### .NET Framework Paket

Zusätzlich zu dem oben genannten generischen Paket gibt es auch das Paket `generic-netf`, das auf dem .NET Framework (und nicht dem .NET Core) basiert. Bei diesem Paket handelt es sich um eine veraltete Variante, die eine, bereits aus ASF V2 bekannte, fehlende Kompatibilität bietet und z.B. mit **[Mono](https://www.mono-project.com)** ausgeführt werden kann, während das mit dem `generic` .NET Core Paket derzeit nicht möglich ist.

Im Allgemeinen solltest du **dieses Paket so weit wie möglich vermeiden**, da die meisten Betriebssysteme und Setups perfekt (und viel besser) mit dem oben erwähnten ` generic ` Paket unterstützt werden. Tatsächlich ist es sinnvoll, dieses Paket nur auf Plattformen zu verwenden, auf denen es keine funktionierende .NET Core Runtime gibt, aber eine Mono-Implementierung bieten. Beispiele für solche Plattformen sind `linux-x86` (32-bit i386/i686 linux), sowie `linux-armel` (ARMv6-Boards wie z.b. der Raspberry Pi 0 & 1), die alle bislang kein offiziell funktionierendes .NET Core Runtime haben.

Im Laufe der Zeit werden mehr Plattformen von .NET Core unterstützt und die Kompatibilität zwischen .NET Framework und .NET Core wird sich verringern. In der Zukunft wird das `generic-netf` Paket vollständig durch das `generic` Paket ersetzt werden. Bitte verzichten Sie darauf es zu verwenden, wenn Sie stattdessen ein anderes .NET Core Paket verwenden können, da `generic-netf` im Vergleich zu .NET Core Versionen viel Funktionalität und Kompatibilität fehlt und es in Zukunft immer weniger funktional sein wird. Wir bieten **nur** Unterstützung für dieses Paket auf Rechnern, die die obrige `generic` Variante nicht verwenden können (z. B. `linux-x86`), und nur mit aktueller Runtime (z. B. neuestes Mono).

---

### Betriebssystemspezifisch

Das betriebssystemspezifische Paket beinhaltet neben dem verwalteten Code, der im generischen Paket enthalten ist, auch nativen Code für die jeweilige Plattform. Mit anderen Worten, das betriebssystemspezifische Paket **beinhaltet bereits die richtige .NET Core Runtime**, was es dir ermöglicht, den gesamten Installationsprozess komplett zu überspringen und ASF einfach direkt zu starten. Das betriebssystemspezifische Paket, wie man dem Namen entnehmen kann, ist betriebssystemspezifisch und jedes Betriebssystem benötigt eine eigene Version - zum Beispiel Windows benötigt PE32+ `ArchiSteamFarm.exe` Binärdatei während Linux mit Unix ELF `ArchiSteamFarm` Binärdatei arbeitet. Diese beiden Typen sind nicht miteinander kompatibel.

ASF ist derzeit in folgenden betriebsystemspezifischen Varianten verfügbar:

- `win-x64` ist kompatibel mit 64-Bit-Windows-Betriebssystemen. Dies beinhaltet sowohl Windows 7 (SP1+), 8.1, 10, Server 2012 R2/ Server 2016, als auch zukünftige Versionen.
- `linux-arm` ist kompatibel mit 32-Bit-ARM-basierten (ARMv7+) GNU/Linux-Betriebssystemen. Dies beinhaltet sämtliche Plattformen, wie etwa Raspberry Pi 2 (oder Neuer) mit allen für diese verfügbaren GNU/Linux OS (z. B. Raspbian), in der aktuellen bzw. zukünftigen Version. Diese Variante funktioniert nicht mit älteren ARM-Architekturen, wie z. B. ARMv6 in Raspberry Pi 0 & 1. Sie funktioniert auch nicht mit Betriebssystemen, die nicht die erforderliche GNU/Linux-Umgebung implementieren, wie z. B. Android).
- `linux-arm64` ist kompatibel mit 64-Bit-ARM-basierten (ARMv8+) GNU/Linux-Betriebssystemen. Dies beinhaltet sämtliche Plattformen, wie etwa Raspberry Pi 3 (oder Neuer) mit allen für diese verfügbaren AArch64 GNU/Linux OS (z. B. Raspbian), in der aktuellen bzw. zukünftigen Version. Diese Variante funktioniert nicht mit 32-Bit-Betriebssystemen, die nicht über erforderliche 64-Bit-Bibliotheken verfügen (wie Raspbian), es funktioniert auch nicht mit Betriebssystemen, die nicht die erforderliche GNU/Linux-Umgebung implementieren (wie Android).
- `linux-x64` ist kompatibel mit 64-Bit GNU/Linux-Betriebssystemen. Dazu gehören Alpine, CentOS/Fedora/RHEL, Debian/Ubuntu/Linux Mint, OpenSUSE/SLES und viele andere, einschließlich ihrer Derivate, in aktuellen und zukünftigen Versionen.
- `osx-x64` ist kompatibel mit 64-Bit OS X Betriebssystemen. Dazu gehört die Version ab 10.13, sowie zukünftige Versionen.

Selbst wenn Sie kein betriebssystemspezifisches Paket für Ihre Betriebssystem-Architektur-Kombination zur Auswahl haben, können Sie natürlich jederzeit selbst die entsprechende .NET Core Runtime installieren und die generische ASF-Version ausführen, was auch der Hauptgrund dafür ist, dass diese überhaupt existiert. Der generische ASF-Build ist plattformunabhängig und läuft auf jeder Plattform, die eine funktionierende .NET Core Runtime hat. Wichtig ist: ASF benötigt die .NET Core Runtime und nicht ein bestimmtes Betriebssystem oder eine bestimmte Architektur. Wenn Sie zum Beispiel ein 32-Bit-Windows benutzen, dann können Sie trotz der fehlenden dedizierten `win-x86` ASF-Version immer noch das .NET Core SDK in der `win-x86`-Version installieren und die generische ASF-Version problemlos ausführen. Wir können nicht jede Kombination aus Betriebssystem und Architektur ansprechen, die existiert und von jemandem verwendet wird, also müssen wir irgendwo eine Grenze ziehen. Ein gutes Beispiel für diese Grenze ist x86, da es sich um eine veraltete Architektur seit mindestens 2004 handelt.

Für eine vollständige Liste aller unterstützten Plattformen und Betriebssystemen von .NET Core 5.0 besuchen Sie bitte die **[Versionshinweise](https://github.com/dotnet/core/blob/master/release-notes/5.0/5.0-supported-os.md)**.

---

## Runtime-Anforderungen

Wenn du ein betriebssystemspezifisches Paket verwendest, musst du dir keine Sorgen um die Runtime-Anforderungen machen, denn ASF wird immer mit der erforderlichen und aktuellen Runtime ausgeliefert, die einwandfrei funktioniert, solange du die **[.NET Core Prerequisites](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)** installiert und auf dem neuesten Stand hast. Mit anderen Worten, **du musst die .NET Core Runtime oder SDK** nicht installieren, da betriebssystemspezifische Builds nur native Betriebssystemabhängigkeiten (Prerequisites) und nichts anderes erfordern.

Wenn du jedoch versuchst, das **generische** ASF-Paket auszuführen, dann musst du sicherstellen, dass deine .NET Core Runtime die von ASF benötigte Plattform unterstützt.

ASF als Programm ist derzeit auf **.NET Core 5.0** (`.NET 5.0`) ausgerichtet, könnte aber in Zukunft eine neuere Plattform erfordern. `net5.0` wird seit 5.0.100 SDK (5.0.0 Runtime) unterstützt, wobei ASF darauf konfiguriert ist, ** die letzte Runtime zum Zeitpunkt der Kompilierung** zu verwenden. Also sollten Sie sicherstellen, dass Ihnen**[die neueste SDK](https://dotnet.microsoft.com/download)** (oder zumindest die Runtime) für Ihre Maschine zur Verfügung steht. Die generische ASF-Variante kann den Start verweigern, wenn die installierte Runtime älter ist als die minimale (Ziel-) Runtime, die während der Kompilierung bekannt ist.

If in doubt, check what our **[continuous integration uses](https://github.com/JustArchiNET/ArchiSteamFarm/actions/workflows/publish.yml?query=branch%3Amain)** for compiling and deploying ASF releases on GitHub. You can find `dotnet --info` output in every build as part of .NET verification step.