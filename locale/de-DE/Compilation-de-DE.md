# Kompilierung

Kompilierung ist der Prozess zur Erstellung von ausführbaren Dateien. Dies solltest du tun, wenn du deine eigenen &Auml;nderungen zu ASF hinzufügen willst oder wenn du aus irgendeinem Grund den ausf&uuml;hrbaren Dateien der offiziell bereitgestellten **[Versionen](https://github.com/JustArchiNET/ArchiSteamFarm/releases)** nicht vertraust. Wenn du ein Benutzer und kein Entwickler bist, möchtest du höchstwahrscheinlich bereits vorkompilierte Binärdateien verwenden. Wenn du aber deine eigenen verwenden oder etwas Neues lernen möchtest, lies weiter.

ASF kann auf allen momentan unterstützten Plattformen kompiliert werden so lange du alle benötigten Programme hierzu hast.

* * *

## .NET Core SDK

Unabhängig von der Plattform benötigst du die vollständige .NET Core SDK (nicht nur Runtime) um ASF zu kompilieren. Eine Installationsanleitung findest du auf der **[.NET Core Installationsseite](https://www.microsoft.com/net/download)**. Du musst die passende .NET Core SDK-Version für dein Betriebssystem installieren. Nach erfolgreicher Installation sollte der Befehl `dotnet` funktionieren und betriebsbereit sein. Du kannst mit `dotnet --info` überprüfen ob es funktioniert. Achte auch darauf, dass dein .NET Core SDK mit den ASF **[Runtime-Anforderungen](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-de-DE#runtime-anforderungen)** übereinstimmt.

Beispiel von `dotnet --info` unter Windows:

    .NET Core SDK (reflecting any global.json):
     Version:   2.1.300
     Commit:    adab45bf0c
    
    Runtime Environment:
     OS Name:     Windows
     OS Version:  10.0.17134
     OS Platform: Windows
     RID:         win10-x64
     Base Path:   C:\Program Files\dotnet\sdk\2.1.300\
    
    Host (useful for support):
      Version: 2.1.0
      Commit:  caa7b7e2ba
    
    .NET Core SDKs installed:
      2.1.300 [C:\Program Files\dotnet\sdk]
    
    .NET Core runtimes installed:
      Microsoft.AspNetCore.All 2.1.0 [C:\Program Files\dotnet\shared\Microsoft.AspNetCore.All]
      Microsoft.AspNetCore.App 2.1.0 [C:\Program Files\dotnet\shared\Microsoft.AspNetCore.App]
      Microsoft.NETCore.App 2.1.0 [C:\Program Files\dotnet\shared\Microsoft.NETCore.App]
    

* * *

## Kompilierung

Wenn dein .NET Core SDK funktioniert und in der entsprechenden Version installiert ist, wechsle einfach in das ASF-Verzeichnis und führe folgendes aus:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "netcoreapp2.1" -o "out/generic" "/p:LinkDuringPublish=false"
```

Wenn du Linux/OS X verwendest, kannst du stattdessen das Skript `cc.sh` verwenden, das dasselbe in etwas komplexerer Weise tut.

Wenn die Kompilierung erfolgreich beendet wurde, findest du dein ASF in der `source` Version im `ArchiSteamFarm/out/generic` Verzeichnis. Dies ist dasselbe wie der offizielle `generische` ASF-Build, aber es hat `UpdateChannel` und `UpdatePeriod` von `0` erzwungen.

### Betriebssystemspezifisch

Du kannst auch das betriebssystemspezifische .NET Core Paket generieren, wenn du einen bestimmten Grund dazu hast. Im Allgemeinen solltest du das nicht tun, weil du gerade `generisch` kompiliert hast, das du mit deiner bereits installierten .NET Core Runtime laufen lassen kannst, die du für die Kompilierung verwendet hast, aber nur für den Fall, dass du es möchtest:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "netcoreapp2.1" -o "out/linux-x64" -r "linux-x64" "/p:CrossGenDuringPublish=false"
```

Natürlich solltest du `linux-x64` durch eine Betriebssystemarchitektur ersetzen, die du anpeilen möchtest, wie beispielsweise `win-x64`. Auch in diesem Build werden Aktualisierungen deaktiviert sein.

### .NET Framework

Im sehr seltenen Fall, dass du das `generic-netf` Paket erstellen möchtest, kannst du das Zielframework von `netcoreapp2.1` auf `net472` ändern. Denke daran, dass du zusätzlich zum .NET Core SDK ein entsprechendes **[.NET Framework](https://www.microsoft.com/net/download/visual-studio-sdks)** Developer Pack zum Kompilieren der `netf` Variante benötigst.

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net472" -o "out/generic-netf"
```

In noch selteneren Fällen, wenn du das .NET Framework oder sogar das .NET Core SDK selbst nicht installieren kannst (z.B. weil du unter `linux-x86` mit `mono` kompilieren möchtest), kannst du `msbuild` direkt aufrufen:

```shell
msbuild /m /p:Configuration=Release /p:PublishDir=out/generic-netf /p:TargetFramework=net472 /r /t:Publish ArchiSteamFarm
```

* * *

## Entwicklung

Wenn du ASF-Quelltext bearbeiten möchtest, kannst du zu diesem Zweck jede .NET Core kompatible IDE verwenden, obwohl selbst das optional ist. Du kannst auch mit einem Notepad arbeiten und mit dem oben beschriebenen Befehl `dotnet` kompilieren. Dennoch empfehlen wir für Windows ein **[aktuelles Visual Studio](https://www.visualstudio.com/downloads)** (kostenlose Community-Version reicht vollkommen). Wir empfehlen auch, Visual Studio zusammen mit **[ReSharper](https://www.jetbrains.com/resharper)** zu verwenden, auch wenn dies kein kostenloses Produkt ist.

Wenn du stattdessen den ASF-Quelltext unter Linux/OS X bearbeiten möchtest, empfehlen wir eine **[aktuelle Visual Studio Code Version](https://code.visualstudio.com/download)**. Diese Version ist nicht so umfangreich wie das klassische Visual Studio, aber reicht vollkommen aus.

Natürlich sind alle obigen Vorschläge nur Empfehlungen, du kannst verwenden was immer du willst. Am Ende wird sowieso immer `dotnet build` ausgeführt. Wir verwenden Visual Studio + ReSharper für die ASF-Entwicklung, mit einem kleinen Teil der Drittanbieter `Programme`, die du im Repository finden kannst.

* * *

## Tags

`master` Zweig ist nicht garantiert in einem Zustand, der eine erfolgreiche Kompilierung oder eine fehlerfreie ASF-Ausführung überhaupt erst ermöglicht, da es sich um einen Entwicklungszweig handelt, wie in unserem **[Veröffentlichungszyklus](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle-de-DE)** beschrieben. Wenn du ASF aus dem Quelltext kompilieren möchtest, dann solltest du dafür einen geeigneten **[Tag](https://github.com/JustArchiNET/ArchiSteamFarm/tags)** verwenden, der zumindest eine erfolgreiche Kompilierung garantiert und sehr wahrscheinlich auch fehlerfreie ausgeführt werden kann (wenn der Build als stabile Version markiert wurde). Um den aktuellen "Gesundheitszustand" des Baumes zu überprüfen, kannst du unsere CIs verwenden - **[AppVeyor](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)** oder **[Travis](https://travis-ci.com/JustArchiNET/ArchiSteamFarm)**.

* * *

## Offizielle Veröffentlichungen

Offizielle ASF-Veröffentlichungen werden von **[AppVeyor](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)** unter Windows kompiliert, mit der neuesten .NET Core SDK, welche den ASF **[Runtime-Anforderungen](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-de-DE#runtime-anforderungen)** entspricht. Nach bestandenen Tests werden alle Pakete auf GitHub bereitgestellt. Dies garantiert auch Transparenz, da AppVeyor für alle Builds immer offizielle öffentliche Quellen verwendet und man Prüfsummen von AppVeyor-Artefakten mit GitHub Assets vergleichen kann. ASF-Entwickler kompilieren oder veröffentlichen Builds nicht manuell, außer für den privaten Entwicklungsprozess, einschließlich Debugging.