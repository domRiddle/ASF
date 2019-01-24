# Kompilierung

Kompilierung ist der Prozess zur Erstellung von ausführbaren Dateien. Dies solltest du tun, wenn du deine eigenen &Auml;nderungen zu ASF hinzufügen willst oder wenn du aus irgendeinem Grund den ausf&uuml;hrbaren Dateien der offiziell bereitgestellten **[Versionen](https://github.com/JustArchiNET/ArchiSteamFarm/releases)** nicht vertraust. Wenn du ein Benutzer und kein Entwickler bist, möchtest du höchstwahrscheinlich bereits vorkompilierte Binärdateien verwenden. Wenn du aber deine eigenen verwenden oder etwas Neues lernen möchtest, lies weiter.

ASF kann auf allen momentan unterstützten Plattformen kompiliert werden so lange du alle benötigten Programme hierzu hast.

* * *

## .NET Core SDK

Unabhängig von der Plattform benötigst du die vollständige .NET Core SDK (nicht nur Runtime) um ASF zu kompilieren. Eine Installationsanleitung findest du auf der **[.NET Core Installationsseite](https://dotnet.microsoft.com/download)**. Du musst die passende .NET Core SDK-Version für dein Betriebssystem installieren. Nach erfolgreicher Installation sollte der Befehl `dotnet` funktionieren und betriebsbereit sein. Du kannst mit `dotnet --info` überprüfen ob es funktioniert. Achte auch darauf, dass dein .NET Core SDK mit den ASF **[Runtime-Anforderungen](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-de-DE#runtime-anforderungen)** übereinstimmt.

* * *

## Kompilierung

Wenn dein .NET Core SDK funktioniert und in der entsprechenden Version installiert ist, wechsle einfach in das ASF-Verzeichnis und führe folgendes aus:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "netcoreapp2.2" -o "out/generic" "/p:LinkDuringPublish=false"
```

Wenn du Linux/OS X verwendest, kannst du stattdessen das Skript `cc.sh` verwenden, das dasselbe in etwas komplexerer Weise tut.

Wenn die Kompilierung erfolgreich beendet wurde, findest du dein ASF in der `source` Version im `ArchiSteamFarm/out/generic` Verzeichnis. Dies ist dasselbe wie der offizielle `generische` ASF-Build, aber es hat `UpdateChannel` und `UpdatePeriod` von `0` erzwungen.

### Betriebssystemspezifisch

Du kannst auch das betriebssystemspezifische .NET Core Paket generieren, wenn du einen bestimmten Grund dazu hast. Im Allgemeinen solltest du das nicht tun, weil du gerade `generisch` kompiliert hast, das du mit deiner bereits installierten .NET Core Runtime laufen lassen kannst, die du für die Kompilierung verwendet hast, aber nur für den Fall, dass du es möchtest:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "netcoreapp2.2" -o "out/linux-x64" -r "linux-x64" "/p:CrossGenDuringPublish=false"
```

Natürlich solltest du `linux-x64` durch eine Betriebssystemarchitektur ersetzen, die du anpeilen möchtest, wie beispielsweise `win-x64`. Auch in diesem Build werden Aktualisierungen deaktiviert sein.

### .NET Framework

Im sehr seltenen Fall, dass du das `generic-netf` Paket erstellen möchtest, kannst du das Zielframework von `netcoreapp2.2` auf `net472` ändern. Beachte, dass du neben der .NET Core SDK auch ein entsprechendes **[.NET Framework](https://dotnet.microsoft.com/download/visual-studio-sdks)** Entwicklerpaket für die Kompilierung der `netf` Variante benötigst, so dass das untenstehende nur unter Windows funktioniert:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net472" -o "out/generic-netf"
```

Falls du das .NET Framework oder sogar die .NET Core SDK selbst nicht installieren kannst (z.B. weil du auf `linux-x86` mit `mono` kompiliert hast), kannst du `msbuild` direkt aufrufen. Du musst auch `ASFNetFramework` manuell angeben, da ASF standardmäßig netf build auf Nicht-Windows-Plattformen deaktiviert:

```shell
msbuild /m /p:Configuration=Release /p:PublishDir=out/generic-netf /p:TargetFramework=net472 /p:ASFNetFramework=true /r /t:Publish ArchiSteamFarm
```

* * *

## Entwicklung

Wenn du ASF-Quelltext bearbeiten möchtest, kannst du zu diesem Zweck jede .NET Core kompatible IDE verwenden, obwohl selbst das optional ist. Du kannst auch mit einem Notepad arbeiten und mit dem oben beschriebenen Befehl `dotnet` kompilieren. Dennoch empfehlen wir für Windows ein **[aktuelles Visual Studio](https://visualstudio.microsoft.com/downloads)** (kostenlose Community-Version reicht vollkommen). Wir empfehlen auch, Visual Studio zusammen mit **[ReSharper](https://www.jetbrains.com/resharper)** zu verwenden (optional), auch wenn dies kein kostenloses Produkt ist.

Wenn du stattdessen den ASF-Quelltext unter Linux/OS X bearbeiten möchtest, empfehlen wir eine **[aktuelle Visual Studio Code Version](https://code.visualstudio.com/download)**. Diese Version ist nicht so umfangreich wie das klassische Visual Studio, aber reicht vollkommen aus.

Natürlich sind alle obigen Vorschläge nur Empfehlungen, du kannst verwenden was immer du willst. Am Ende wird sowieso immer `dotnet build` ausgeführt. Wir verwenden Visual Studio + ReSharper für die ASF-Entwicklung, mit einem kleinen Teil der Drittanbieter `Programme`, die du im Repository finden kannst.

* * *

## Tags

`master` Zweig ist nicht garantiert in einem Zustand, der eine erfolgreiche Kompilierung oder eine fehlerfreie ASF-Ausführung überhaupt erst ermöglicht, da es sich um einen Entwicklungszweig handelt, wie in unserem **[Veröffentlichungszyklus](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle-de-DE)** beschrieben. Wenn du ASF aus dem Quelltext kompilieren möchtest, dann solltest du dafür einen geeigneten **[Tag](https://github.com/JustArchiNET/ArchiSteamFarm/tags)** verwenden, der zumindest eine erfolgreiche Kompilierung garantiert und sehr wahrscheinlich auch fehlerfreie ausgeführt werden kann (wenn der Build als stabile Version markiert wurde). Um den aktuellen "Gesundheitszustand" des Baumes zu überprüfen, kannst du unsere CIs verwenden - **[AppVeyor](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)** oder **[Travis](https://travis-ci.com/JustArchiNET/ArchiSteamFarm)**.

* * *

## Offizielle Veröffentlichungen

Offizielle ASF-Veröffentlichungen werden von **[AppVeyor](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)** unter Windows kompiliert, mit der neuesten .NET Core SDK, welche den ASF **[Runtime-Anforderungen](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-de-DE#runtime-anforderungen)** entspricht. Nach bestandenen Tests werden alle Pakete auf GitHub bereitgestellt. Dies garantiert auch Transparenz, da AppVeyor für alle Builds immer offizielle öffentliche Quellen verwendet und man Prüfsummen von AppVeyor-Artefakten mit GitHub Assets vergleichen kann. Die ASF-Entwickler kompilieren oder veröffentlichen Builds nicht selbst, außer für den privaten Entwicklungsprozess und Debugging.