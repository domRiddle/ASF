# Kompilierung

Kompilierung ist der Prozess zur Erstellung von ausführbaren Dateien. Dies ist ratsam, wenn Sie Ihre eigenen Änderungen zu ASF hinzufügen wollen, oder wenn Sie aus irgenIhrem Grund den ausführbaren Dateien der offiziell bereitgestellten **[Versionen](https://github.com/JustArchiNET/ArchiSteamFarm/releases)** nicht vertrauen. Wenn Sie ein einfacher Benutzer und kein Entwickler sind, möchten Sie höchstwahrscheinlich bereits vorkompilierte Binärdateien verwenden. Wenn Sie aber eigenen Dateien verwenden oder etwas Neues lernen möchten, lesen Sie bitte hier weiter.

ASF kann auf allen momentan unterstützten Plattformen kompiliert werden, solange Sie Zugriff auf die benötigten Programme haben.

---

## .NET SDK

Regardless of platform, you need full .NET SDK (not just runtime) in order to compile ASF. Installation instructions can be found on **[.NET download page](https://dotnet.microsoft.com/download)**. You need to install appropriate .NET SDK version for your OS. Nach erfolgreicher Installation sollte der Befehl `dotnet` funktionieren und betriebsbereit sein. Sie können mit `dotnet --info` überprüfen ob es funktioniert. Also ensure that your .NET SDK matches ASF **[runtime requirements](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)**.

---

## Kompilierung

Assuming you have .NET SDK operative and in appropriate version, simply navigate to source ASF directory (cloned or downloaded and unpacked ASF repo) and execute:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net5.0" -o "out/generic"
```

Für Linux/OS X als Zielplattform können Sie stattdessen das Skript `cc.sh` verwenden, was dasselbe in etwas komplexerer Weise tut.

Wenn die Kompilierung erfolgreich beendet wurde, finden Sie Ihr ASF in der `source` Version im `ArchiSteamFarm/out/generic` Verzeichnis. Dies gleicht sich mit dem offiziellen `generic` ASF-Build, nur ist hier der `UpdateChannel` und `UpdatePeriod` auf `0` gesetzt, womit ein überschreiben des Selbst-Builds bis zur nächsten Kompilierung vermieden wird.

### Betriebssystemspezifisch

You can also generate OS-specific .NET package if you have a specific need. In general you shouldn't do that because you've just compiled `generic` flavour that you can run with your already-installed .NET runtime that you've used for the compilation in the first place, but just in case you want to:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net5.0" -o "out/linux-x64" -r "linux-x64"
```

Natürlich sollten Sie `linux-x64` durch eine Betriebssystemarchitektur ersetzen die Sie anpeilen möchten, wie beispielsweise `win-x64`. Auch in diesem Build werden Aktualisierungen deaktiviert sein.

### .NET Framework

Im sehr seltenen Fall, dass Sie das `generic-netf`-Paket erstellen möchten, können Sie das Zielframework von `net5.0` auf `net48` ändern. Keep in mind that you'll need appropriate **[.NET Framework](https://dotnet.microsoft.com/download/visual-studio-sdks)** developer pack for compiling `netf` variant, in addition to .NET SDK, so the below will work only on Windows:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net48" -o "out/generic-netf"
```

In case of being unable to install .NET Framework or even .NET SDK itself (e.g. because of building on `linux-x86` with `mono`), you can call `msbuild` directly. Sie müssen auch `ASFNetFramework` manuell angeben, da ASF standardmäßig `netf` build auf Nicht-Windows-Plattformen deaktiviert:

```shell
msbuild /m /r /t:Publish /p:Configuration=Release /p:TargetFramework=net48 /p:PublishDir=out/generic-netf /p:ASFNetFramework=true ArchiSteamFarm
```

---

## Entwicklung

If you'd like to edit ASF code, you can use any .NET compatible IDE for that purpose, although even that is optional, since you can as well edit with a notepad and compile with `dotnet` command described above. Dennoch empfehlen wir für Windows ein **[aktuelles Visual Studio](https://visualstudio.microsoft.com/downloads)** (kostenlose Community-Version reicht vollkommen).

Wenn du stattdessen den ASF-Quelltext unter Linux/OS X bearbeiten möchtest, empfehlen wir eine **[aktuelle Visual Studio Code Version](https://code.visualstudio.com/download)**. Diese Version ist nicht so umfangreich wie das klassische Visual Studio, aber reicht vollkommen aus.

Natürlich sind alle obigen Vorschläge nur Empfehlungen, Sie können verwenden was immer Sie möchten. Am Ende wird sowieso immer `dotnet build` ausgeführt. Wir verwenden **[JetBrains Rider](https://www.jetbrains.com/rider)** für die ASF-Entwicklung, obwohl es keine freie Lösung ist.

---

## Tags

Der `main` Zweig ist nicht unbedingt in einem Zustand, der eine erfolgreiche Kompilierung oder eine fehlerfreie ASF-Ausführung überhaupt erst ermöglicht, da es sich um einen Entwicklungszweig handelt, wie in unserem **[Veröffentlichungszyklus](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle-de-DE)** beschrieben. Für die Kompilierung oder Referenz von ASF aus dem Quelltext, die wird ein angemessenes **[Tag](https://github.com/JustArchiNET/ArchiSteamFarm/tags)** benötigt, welches zumindest eine erfolgreiche kompilation, und problemlose Ausführung (falls das Build als stable release markiert wurde), zu garantieren. In order to check the current "health" of the tree, you can use our CI - **[GitHub](https://github.com/JustArchiNET/ArchiSteamFarm/actions/workflows/ci.yml?query=branch%3Amain)**.

---

## Offizielle Veröffentlichungen

Official ASF releases are compiled by **[GitHub](https://github.com/JustArchiNET/ArchiSteamFarm/actions)** on Windows, with latest .NET SDK that matches ASF **[runtime requirements](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)**. Nach dem Bestehen von Tests werden alle Pakete auf GitHub als Release bereitgestellt. Dies garantiert auch Transparenz, da GitHub immer offizielle öffentliche Quellen für alle Builds verwendet und Sie können die Prüfsummen der GitHub Artefakte mit GitHub Release-Assets abgleichen. Die ASF-Entwickler kompilieren oder veröffentlichen selbst keine Builds, außer für den privaten Entwicklungsprozess und Debugging.