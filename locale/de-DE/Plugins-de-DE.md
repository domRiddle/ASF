# Plugins

Ab ASF V4 bietet das Programm Unterstützung für benutzerdefinierte Plugins, die während der Laufzeit geladen werden können. Plugins ermöglichen es dir, das ASF-Verhalten anzupassen, z.B. durch Hinzufügen von benutzerdefinierten Befehlen, einer benutzerdefinierten Handelslogik oder der vollständigen Integration mit Diensten und APIs von Drittanbietern.

* * *

## Für Benutzer

ASF lädt Plugins aus dem Verzeichnis `plugins`, das sich in deinem ASF-Ordner befindet. Es wird empfohlen, für jedes Plugin das du verwenden möchtest ein eigenes Verzeichnis zu erstellen, das auf seinem Namen basieren kann, wie z.B. `MeinPlugin`. Dies führt zur finalen Baumstruktur von `plugins/MeinPlugin`. Schließlich sollten alle Binärdateien des Plugins in diesem speziellen Ordner abgelegt werden. ASF wird dein Plugin nach dem Neustart ordnungsgemäß erkennen und verwenden.

Normalerweise veröffentlichen Plugin-Entwickler ihre Plugins in Form einer `zip`-Datei mit bereits vorbereiteter Struktur für Sie, sodass es genügt, das Zip-Archiv in das Verzeichnis `plugins` zu entpacken, welches automatisch den entsprechenden Ordner erstellt.

Wenn das Plugin erfolgreich geladen wurde siehst du seinen Namen und seine Version in deinem Protokoll. Du solltest deinen Plugin-Entwickler konsultieren, wenn es um Fragen, Probleme oder die Verwendung der Plugins geht die du verwendest.

Du findest einige der vorgestellten Plugins in unserem **[Drittanbieter](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Third-party-de-DE#asf-plugins)** Abschnitt.

**Bitte beachte, dass ASF-Plugins möglicherweise gefährlich sein können**. Du solltest immer sicherstellen, dass du Plugins von Entwicklern verwendest denen du vertrauen kannst. Die ASF-Entwickler können dir die üblichen ASF-Vorteile (wie z.B. keine Schadsoftware oder VAC-Freiheit) nicht mehr garantieren, wenn du dich dazu entscheidest benutzerdefinierte Plugins zu verwenden. Wir können auch keine Konfigurationen unterstützen die benutzerdefinierte Plugins verwenden, da du dann keinen originalen ASF-Quellcode mehr verwendest.

* * *

## Für Entwickler

Plugins sind Standard .NET-Bibliotheken die die übliche `IPlugin`-Schnittstelle von ASF erben. Du kannst Plugins völlig unabhängig von der Haupt-ASF-Version entwickeln und in aktuellen und zukünftigen ASF-Versionen wiederverwenden, solange die API kompatibel bleibt. Das in ASF verwendete Plugin-System basiert auf `System.Composition`, früher bekannt als **[Managed Extensibility Framework](https://docs.microsoft.com/dotnet/framework/mef)**, das es ASF ermöglicht, deine Bibliotheken während der Laufzeit zu entdecken und zu laden.

* * *

### Erste Schritte

Dein Projekt sollte eine Standard .NET-Bibliothek sein, die auf das geeignete Framework deiner Ziel-ASF-Version abzielt, wie in der **[Kompilierung](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compilation-de-DE)** angegeben. Wir empfehlen dir .NET Core anzuzielen, aber .NET Framework-Plugins sind auch verfügbar.

Das Projekt muss auf die Haupt-Assembly `ArchiSteamFarm` verweisen, entweder auf die vorgefertigte `ArchiSteamFarm.dll`-Bibliothek, die du als Teil der Veröffentlichung heruntergeladen hast, oder auf das Quellprojekt (z.B. wenn du dich entschieden hast den ASF-Baum als Submodul hinzuzufügen). Dies ermöglicht es dir auf ASF-Strukturen, -Methoden und -Eigenschaften zuzugreifen und diese zu entdecken, insbesondere auf das Core `IPlugin`-Interface, von dem du im nächsten Schritt erben musst. Das Projekt muss auch mindestens `System.Composition.AttributedModel` referenzieren, was es dir erlaubt, dein `IPlugin` mittels `[Export]` für ASF bereitzustellen. Darüber hinaus kann/muss man auf andere gemeinsame Bibliotheken verweisen, um die Datenstrukturen zu interpretieren, die man in manchen Schnittstellen erhält, aber wenn man sie nicht explizit benötigt, reicht das fürs Erste.

Wenn du alles richtig gemacht hast wird deine `csproj` Datei ähnlich wie unten aussehen:

```csproj
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>net5.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="System.Composition.AttributedModel" IncludeAssets="compile" Version="*" />
  </ItemGroup>

  <ItemGroup>
    <Reference Include="ArchiSteamFarm" HintPath="C:\\Path\To\Downloaded\ArchiSteamFarm.dll" />

    <!-- If building as part of ASF source tree, use this instead of <Reference> above -->
    <!-- <ProjectReference Include="C:\\Path\To\ArchiSteamFarm\ArchiSteamFarm.csproj" ExcludeAssets="all" Private="false" /> -->
  </ItemGroup>
</Project>
```

Von der Quellcode-Seite muss deine Plugin-Klasse von der Schnittstelle `IPlugin` erben (entweder explizit oder implizit von einer spezialisierteren Schnittstelle, wie `IASF`) und `[Export(typeof(IPlugin))]`, um von ASF während der Laufzeit erkannt zu werden. Das einfachste Beispiel, das dies ermöglicht, ist das folgende:

```csharp
using System;
using System.Composition;
using ArchiSteamFarm;
using ArchiSteamFarm.Plugins;

namespace DeinNamespace.DeinPluginName {
    [Export(typeof(IPlugin))]
    public sealed class DeinPluginName : IPlugin {
        public string Name => nameof(DeinPluginName);
        public Version Version => typeof(DeinPluginName).Assembly.GetName().Version;

        public void OnLoaded() {
            ASF.ArchiLogger.LogGenericInfo("Miau");
        }
    }
}
```

Um dein Plugin nutzen zu können, musst du es zunächst kompilieren. Du kannst das entweder von deiner Entwicklungsumgebung aus oder aus dem Stammverzeichnis deines Projekts mittels eines Befehls tun:

```shell
# Wenn dein Projekt eigenständig ist (es ist nicht notwendig, seinen Namen zu definieren, da es das einzige ist)
dotnet publish -c "Release" -o "out"

# Wenn dein Projekt Teil des ASF-Quellbaums ist (um das Kompilieren unnötiger Teile zu vermeiden)
dotnet publish YourPluginName -c "Release" -o "out"
```

Danach ist dein Plugin einsatzbereit. Es liegt an dir, wie du dein Plugin verteilen und veröffentlichen möchtest, aber wir empfehlen, ein Zip-Archiv mit einem einzigen Ordner namens `DeinNamespace.DeinPluginName` zu erstellen, in dem du dein kompiliertes Plugin mit seinen **[Abhängigkeiten](#plugin-abhängigkeiten)** kopierst. Auf diese Weise muss der Benutzer einfach sein Zip-Archiv in sein `plugins` Verzeichnis entpacken und nichts weiter tun.

This is only the most basic scenario to get you started. We have **[`ExamplePlugin`](https://github.com/JustArchiNET/ArchiSteamFarm/tree/main/ArchiSteamFarm.CustomPlugins.ExamplePlugin)** project that shows you example interfaces and actions that you can do within your own plugin, including helpful comments. Zögere nicht einen Blick darauf zu werfen, wenn du von einem funktionierenden Quellcode lernen möchtest, oder entdecke den `ArchiSteamFarm.Plugins` Namespace selbst und schaue in die mitgelieferte Dokumentation für alle verfügbaren Optionen.

If instead of example plugins you'd want to learn from real projects, there is **[`SteamTokenDumper`](https://github.com/JustArchiNET/ArchiSteamFarm/tree/main/ArchiSteamFarm.OfficialPlugins.SteamTokenDumper)** plugin developed by us, the one that is bundled together with ASF. In addition to that, there are also plugins developed by other developers, in our **[third-party](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Third-party#asf-plugins)** section.

* * *

### API Verfügbarkeit

ASF stellt dir neben dem, worauf du in den Schnittstellen selbst Zugriff hast, viele seiner internen APIs zur Verfügung die du nutzen kannst um die Funktionalität zu erweitern. Wenn du zum Beispiel eine neue Art einer Steam-Web-Anfrage senden möchtest, dann musst du nicht alles von Grund auf neu implementieren, insbesondere nicht all den Fragen mit denen wir uns vor dir beschäftigt haben. Nutze einfach unseren `Bot.ArchiWebHandler`, der bereits eine Menge `UrlWithSession()`-Methoden für dich freilegt und alle untergeordneten Dinge wie Authentifizierung, Sitzungsaktualisierung oder Weblimitierung für dich handhabt. Likewise, for sending web requests outside of Steam platform, you could use standard .NET `HttpClient` class, but it's much better idea to use `Bot.ArchiWebHandler.WebBrowser` that is available for you, which once again offers you a helpful hand, for example in regards to retrying failed requests.

Wir haben eine sehr offene Richtlinie in Bezug auf unsere API-Verfügbarkeit, wenn du also etwas nutzen möchtest das der ASF-Code bereits enthält, öffne einfach **[ein Issue](https://github.com/JustArchiNET/ArchiSteamFarm/issues)** und erkläre darin deinen geplanten Anwendungsfall der internen ASF-API. Wir werden höchstwahrscheinlich nichts dagegen haben solange dein Anwendungsfall Sinn macht. Es ist einfach nicht möglich alles zu öffnen was jemand nutzen möchte, also haben wir das geöffnet, was für uns am sinnvollsten ist und warten auf deine Wünsche, falls du Zugang zu etwas haben möchtest das noch nicht `public` ist. Dazu gehören auch alle Vorschläge zu neuen `IPlugin`Schnittstellen die sinnvoll sein könnten, um die bestehende Funktionalität zu erweitern.

Tatsächlich ist die interne ASF-API die einzige wirkliche Einschränkung in Bezug auf das, was dein Plugin tun kann. Nichts hält dich davon ab, z.B. die `Discord.Net` Bibliothek in deine Anwendung aufzunehmen und eine Brücke zwischen deinem Discord-Bot und ASF-Befehlen zu schlagen, da dein Plugin auch eigenständige Abhängigkeiten haben kann. The possibilities are endless, and we made our best to give you as much freedom and flexibility as possible within your plugin, so there are no artificial limits on anything, just us not being completely sure which ASF parts are crucial for your plugin development (which you can solve by letting us know, and even without that you can always reimplement the functionality that you need).

* * *

### API Kompatibilität

Es ist wichtig zu betonen, dass ASF eine Verbraucher-Anwendung ist und nicht eine typische Bibliothek mit fester API-Oberfläche, auf die du dich bedingungslos verlassen kannst. Das bedeutet, dass du nicht davon ausgehen kannst, dass dein einmal kompiliertes Plugin mit allen zukünftigen ASF-Versionen weiter funktioniert. Es ist einfach unmöglich, wenn du das Programm weiter entwickeln willst und nicht in der Lage bist dich an ständig laufende Steam-Änderungen anzupassen, um der Rückwärtskompatibilität willen, ist einfach nicht angemessen für unseren Fall. Das sollte für dich logisch sein, aber es ist wichtig diese Tatsache hervorzuheben.

Wir werden unser Bestes tun um die öffentlichen Teile von ASF funktionsfähig und stabil zu halten, aber wir werden keine Angst davor haben die Kompatibilität zu brechen, wenn gute Gründe dafür vorliegen, da wir unserer **[Veraltete Funktionen](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Deprecation-de-DE)** Politik folgen. Dies ist besonders wichtig für interne ASF-Strukturen, die dir als Teil der ASF-Infrastruktur zur Verfügung stehen, wie oben erläutert (z.B. `ArchiWebHandler`), die im Rahmen von ASF-Erweiterungen in einer der zukünftigen Versionen verbessert (und damit neu geschrieben) werden können. Wir werden unser Bestes tun, um dich in den Änderungsprotokollen angemessen zu informieren und während der Laufzeit entsprechende Warnungen über veraltete Funktionen zu geben. Wir haben nicht die Absicht alles neu zu schreiben, um es neu zu schreiben, also kannst du ziemlich sicher sein, dass die nächste kleinere ASF-Version dein Plugin nicht einfach nur deshalb komplett zerstört, weil es eine höhere Versionsnummer hat. Aber es ist eine gute Idee ein Auge auf Änderungsprotokolle zu werden und gelegentlich zu überprüfen ob alles einwandfrei funktioniert.

* * *

### Plugin Abhängigkeiten

Dein Plugin enthält standardmäßig mindestens zwei Abhängigkeiten, die `ArchiSteamFarm` Referenz für die interne API und `PackageReference` von `System.Composition.AttributedModel`, die erforderlich ist, um als ASF-Plugin erkannt zu werden. Darüber hinaus könnte es mehr Abhängigkeiten in Bezug auf das, was du in deinem Plugin getan hast, enthalten (z.B. `Discord.Net` Bibliothek, wenn du dich für die Integration mit Discord entschieden hast).

The output of your build will include your core `YourPluginName.dll` library, as well as all the dependencies that you've referenced. Since you're developing a plugin to already-working program, you don't have to, and even **shouldn't** include dependencies that ASF already includes, for example `ArchiSteamFarm`, `SteamKit2` or `Newtonsoft.Json`. Das Entfernen von mit ASF geteilten Abhängigkeiten in deinem Build ist nicht die absolute Voraussetzung für die Funktionsfähigkeit deines Plugins, aber dies wird den Speicherbedarf und die Größe deines Plugins drastisch reduzieren und gleichzeitig die Leistung erhöhen, da ASF seine eigenen Abhängigkeiten mit dir teilt und nur die Bibliotheken lädt, die es nicht über sich selbst kennt.

In general, it's a recommended practice to include only those libraries that ASF either doesn't include, or includes in the wrong/incompatible version. Examples of those would be obviously `YourPluginName.dll`, but for example also `Discord.Net.dll` if you decided to depend on it, as ASF doesn't include it itself. Bundling libraries that are shared with ASF can still make sense if you want to ensure API compatibility (e.g. being sure that `Newtonsoft.Json` which you depend on in your plugin will always be in version `X` and not the one that ASF ships with), but obviously doing that comes for a price of increased memory/size and worse performance, and therefore should be carefully evaluated.

If you know that the dependency which you need is included in ASF, you can mark it with `IncludeAssets="compile"` as we showed you in the example `csproj` above. This will tell the compiler to avoid publishing referenced library itself, as ASF already includes that one. Likewise, notice that we reference the ASF project with `ExcludeAssets="all" Private="false"` which works in a very similar way - telling the compiler to not produce any ASF files (as the user already has them). This applies only when referencing ASF project, since if you reference a `dll` library, then you're not producing ASF files as part of your plugin.

Wenn du über die obige Erklärung verwirrt bist und du es nicht besser weißt, überprüfe, welche `dll` Bibliotheken im Paket `ASF-generic.zip` enthalten sind und stelle sicher, dass dein Plugin nur die enthält, die noch nicht Teil davon sind. Für die einfachsten Plugins wird dies nur `DeinPluginName.dll` sein. Wenn du während der Laufzeit Probleme in Bezug auf einige Bibliotheken bekommst, füge auch die betroffenen Bibliotheken hinzu. Wenn alles andere fehlschlägt, kannst du dich jederzeit entscheiden, alles zu bündeln.

* * *

### Native Abhängigkeiten

Native Abhängigkeiten werden als Teil von betriebssystemspezifischen Builds generiert, da auf dem Host keine .NET Core Runtime verfügbar ist und ASF seine eigene .NET Core Runtime nutzt, die als Teil des betriebssystemspezifischen Builds gebündelt wird. Um die Größe des Builds zu minimieren, reduziert ASF seine nativen Abhängigkeiten so, dass nur der Programmcode berücksichtigt wird, der innerhalb des Programms erreichbar ist, was die ungenutzten Teile der Laufzeit effektiv reduziert. This can create a potential problem for you in regards to your plugin, if suddenly you find out yourself in a situation where your plugin depends on some .NET Core feature that isn't being used in ASF, and therefore OS-specific builds can't execute it properly, usually throwing `System.MissingMethodException` or `System.Reflection.ReflectionTypeLoadException` in the process.

Dies ist bei generischen Builds nie ein Problem, da es sich hierbei nie um native Abhängigkeiten handelt (da sie die gesamte lauffähige Runtime auf dem Host haben und ASF ausführen). Es ist auch automatisch eine Lösung des Problems, indem **du dein Plugin ausschließlich in generischen Builds verwendest**, aber offensichtlich hat das seinen eigenen Nachteil, wenn es darum geht, dein Plugin von Benutzern zu trennen, die OS-spezifische Builds von ASF ausführen. Wenn du dich fragst ob dein Problem im Zusammenhang mit nativen Abhängigkeiten steht, kannst du diese Methode auch zur Überprüfung verwenden. Lade dein Plugin in der generischen ASF Variante und sieh ob es funktioniert. If it does, you have plugin dependencies covered, and it's the native dependencies causing issues.

Unfortunately, we had to make a hard choice between publishing whole runtime as part of our OS-specific builds, and deciding to cut it out of unused features, making the build over 50 MB smaller compared to the full one. We've picked the second option, and it's unfortunately impossible for you to include the missing runtime features together with your plugin. If your project requires access to runtime features that are left out, you have to include full .NET runtime that you depend on, and that means running your plugin together with `generic` ASF flavour. You can't run your plugin in OS-specific builds, as those builds are simply missing a runtime feature that you need, and .NET Core runtime as of now is unable to "merge" native dependency that you could've provided with our own. Perhaps it'll improve one day in the future, but as of now it's simply not possible.

ASF's OS-specific builds include the bare minimum of additional functionality which is required to run our official plugins. Apart of that being possible, this also slightly extends the surface to extra dependencies required for the most basic plugins. Therefore not all plugins will need to worry about native dependencies to begin with - only those that go beyond what ASF and our official plugins directly need. This is done as an extra, since if we need to include additional native dependencies ourselves for our own use cases anyway, we can as well ship them directly with ASF, making them available, and therefore easier to cover, also for you.