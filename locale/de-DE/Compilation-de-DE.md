# Kompilierung

Kompilierung ist der Prozess zur Erstellung von ausführbaren Dateien. Dies ist ratsam, wenn Sie Ihre eigenen Änderungen zu ASF hinzufügen wollen, oder wenn Sie aus irgendeinem Grund den ausführbaren Dateien der offiziell bereitgestellten **[Versionen](https://github.com/JustArchiNET/ArchiSteamFarm/releases)** nicht vertrauen. Wenn Sie ein einfacher Benutzer und kein Entwickler sind, möchten Sie höchstwahrscheinlich bereits vorkompilierte Binärdateien verwenden. Wenn Sie aber eigenen Dateien verwenden oder etwas Neues lernen möchten, lesen Sie bitte hier weiter.

ASF kann auf allen momentan unterstützten Plattformen kompiliert werden, solange Sie Zugriff auf die benötigten Programme haben.

* * *

## .NET Core SDK

Unabhängig von der Plattform benötigen Sie die vollständige .NET Core SDK (nicht nur Runtime), um ASF zu kompilieren. Eine Installationsanleitung finden Sie auf der **[.NET Core Installationsseite](https://dotnet.microsoft.com/download)**. Es muss die passende .NET Core SDK-Version für Ihr Betriebssystem installiert sein. Nach erfolgreicher Installation sollte der Befehl `dotnet` funktionieren und betriebsbereit sein. Sie können mit `dotnet --info` überprüfen ob es funktioniert. Stellen Sie sicher, dass Ihr .NET Core SDK mit den ASF **[Runtime-Anforderungen](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-de-DE#runtime-anforderungen)** übereinstimmt.

* * *

## Kompilierung

Sofern Ihr .NET Core SDK funktionsfähig und in der entsprechenden Version ist, navigieren Sie einfach zum Quell-ASF-Verzeichnis (geklont oder heruntergeladen und entpacktes ASF-Repository) und führen Sie folgendes aus:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net5.0" -o "out/generic"
```

Für Linux/OS X als Zielplattform können Sie stattdessen das Skript `cc.sh` verwenden, was dasselbe in etwas komplexerer Weise tut.

Wenn die Kompilierung erfolgreich beendet wurde, finden Sie Ihr ASF in der `source` Version im `ArchiSteamFarm/out/generic` Verzeichnis. Dies gleicht sich mit dem offiziellen `generic` ASF-Build, nur ist hier der `UpdateChannel` und `UpdatePeriod` auf `0` gesetzt, womit ein überschreiben des Selbst-Builds bis zur nächsten Kompilierung vermieden wird.

### Betriebssystemspezifisch

Sie können auch das betriebssystemspezifische .NET Core Paket erstellen, falls es einen bestimmten Grund dazu gibt. Im Allgemeinen sollten Sie das nicht tun, weil Sie gerade die `generic` Version kompiliert haben, welche Sie mit Ihrer bereits installierten .NET Core Laufzeit ausführen können. Aber nur für den Fall, dass Sie es möchten:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net5.0" -o "out/linux-x64" -r "linux-x64"
```

Natürlich sollten Sie `linux-x64` durch eine Betriebssystemarchitektur ersetzen die Sie anpeilen möchten, wie beispielsweise `win-x64`. Auch in diesem Build werden Aktualisierungen deaktiviert sein.

### .NET Framework

Im sehr seltenen Fall, dass Sie das `generic-netf`-Paket erstellen möchten, können Sie das Zielframework von `net5.0` auf `net48` ändern. Dadurch benötigen Sie neben der .NET Core SDK auch ein entsprechendes **[.NET Framework](https://dotnet.microsoft.com/download/visual-studio-sdks)** Entwicklerpaket für die Kompilierung der `netf` Variante, sodass das untenstehende nur unter Windows funktioniert:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net48" -o "out/generic-netf"
```

Falls Sie das .NET Framework oder sogar die .NET Core SDK nicht selbst installieren können (z.B. weil es auf `linux-x86` mit `mono` kompiliert wurde), können Sie `msbuild` direkt aufrufen. You'll also need to specify `ASFNetFramework` manually, as ASF by default disables `netf` build on non-Windows platforms:

```shell
msbuild /m /r /t:Publish /p:Configuration=Release /p:TargetFramework=net48 /p:PublishDir=out/generic-netf /p:ASFNetFramework=true ArchiSteamFarm
```

* * *

## Entwicklung

Wenn Sie ASF-Quelltext bearbeiten möchten, können Sie zu diesem Zweck jede .NET Core kompatible IDE verwenden, obwohl selbst das optional ist. Sie können auch mit einem Notepad arbeiten und mit dem oben beschriebenen `dotnet` Befehl kompilieren. Dennoch empfehlen wir für Windows ein **[aktuelles Visual Studio](https://visualstudio.microsoft.com/downloads)** (kostenlose Community-Version reicht vollkommen).

Wenn Sie stattdessen den ASF-Quelltext unter Linux/OS X bearbeiten möchten, empfehlen wir eine **[aktuelle Visual Studio Code Version](https://code.visualstudio.com/download)**. Diese Version ist nicht so umfangreich wie das klassische Visual Studio, aber reicht vollkommen aus.

Natürlich sind alle obigen Vorschläge nur Empfehlungen, Sie können verwenden was immer Sie möchten. Am Ende wird sowieso immer `dotnet build` ausgeführt. Wir verwenden **[JetBrains Rider](https://www.jetbrains.com/rider)** für die ASF-Entwicklung, mit einem kleinen Teil von Drittanbieter-`Werkzeugen`, die Sie im Repo finden können.

* * *

## Tags

Der `master` Zweig ist nicht unbedingt in einem Zustand, der eine erfolgreiche Kompilierung oder eine fehlerfreie ASF-Ausführung überhaupt erst ermöglicht, da es sich um einen Entwicklungszweig handelt, wie in unserem **[Veröffentlichungszyklus](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle-de-DE)** beschrieben. Für die Kompilierung oder Referenz von ASF aus dem Quelltext, die wird ein angemessenes **[Tag](https://github.com/JustArchiNET/ArchiSteamFarm/tags)** benötigt, welches zumindest eine erfolgreiche kompilation, und problemlose Ausführung (falls das Build als stable release markiert wurde), zu garantieren. In order to check the current "health" of the tree, you can use our continuous integrations - **[GitHub](https://github.com/JustArchiNET/ArchiSteamFarm/actions)**, **[AppVeyor](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)** or **[Travis](https://travis-ci.com/JustArchiNET/ArchiSteamFarm)**.

* * *

## Offizielle Veröffentlichungen

Offizielle ASF-Versionen werden von **[GitHub](https://github.com/JustArchiNET/ArchiSteamFarm/actions)** unter Windows mit der neuesten .NET Core SDK kompiliert, welche mit den ASF **[Runtime-Anforderungen](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-de-DE#runtime-anforderungen)** übereinstimmt. Nach dem Bestehen von Tests werden alle Pakete auf GitHub als Release bereitgestellt. Dies garantiert auch Transparenz, da GitHub immer offizielle öffentliche Quellen für alle Builds verwendet und Sie können die Prüfsummen der GitHub Artefakte mit GitHub Release-Assets abgleichen. Die ASF-Entwickler kompilieren oder veröffentlichen selbst keine Builds, außer für den privaten Entwicklungsprozess und Debugging.