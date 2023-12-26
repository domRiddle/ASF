# Erweiterungen (Plugins)

ASF bietet Unterstützung für benutzerdefinierte Plugins, die während der Laufzeit geladen werden können. Erweiterungen ermöglichen es dir, das ASF-Verhalten anzupassen, z. B. durch Hinzufügen von benutzerdefinierten Befehlen, einer benutzerdefinierten Handelslogik oder der vollständigen Integration mit Diensten und APIs von Drittanbietern.

---

## Für Benutzer

ASF lädt Erweiterungen aus dem Verzeichnis `plugins`, das sich in Ihrem ASF-Ordner befindet. Es wird empfohlen, für jedes Plugin das Sie verwenden möchten ein eigenes Verzeichnis zu erstellen, das auf seinem Namen basieren kann, z. B. `MeinPlugin`. Dies führt zur finalen Baumstruktur von `plugins/MeinPlugin`. Schließlich sollten alle Binärdateien der Erweiterungen in diesem speziellen Ordner abgelegt werden. ASF wird Ihr Plugin nach dem Neustart ordnungsgemäß erkennen und verwenden.

Normalerweise veröffentlichen Plugin-Entwickler Ihre Erweiterungen in Form einer `zip`-Datei mit bereits vorbereiteter Struktur für Sie, sodass es genügt, das Zip-Archiv in das Verzeichnis `plugins` zu entpacken, welches automatisch den entsprechenden Ordner erstellt.

Wenn die Erweiterung erfolgreich geladen wurde en sehen Sie seinen Namen und seine Version in Ihrem Protokoll. Sie sollten Ihren Plugin-Entwickler konsultieren, wenn es um Fragen, Probleme oder die Verwendung der Plugins geht die Sie verwenden.

Sie finden einige der vorgestellten Erweiterungen in unserem **[Drittanbieter](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Third-party-de-DE#asf-plugins)** Abschnitt.

**Bitte beachte, dass ASF-Erweiterungen möglicherweise gefährlich sein können**. Sie sollten immer sicherstellen, dass Sie Erweiterungen von Entwicklern verwenden denen Sie vertrauen können. Die ASF-Entwickler können Ihren die üblichen ASF-Vorteile (z. B. keine Schadsoftware oder VAC-Freiheit) nicht mehr garantieren, wenn Sie sich dazu entscheiden benutzerdefinierte Erweiterungen zu verwenden. Sie müssen verstehen, dass Erweiterungen die volle Kontrolle über den ASF-Prozess haben, sobald sie geladen wurden. Wir sind nicht mehr in der Lage, Installationen mit benutzerdefinierten Plugins zu unterstützen, da Sie keinen Vanilla ASF Code mehr verwenden.

---

## Für Entwickler

Erweiterungen sind Standard .NET-Bibliotheken die die übliche `IPlugin`-Schnittstelle von ASF erben. Sie können Erweiterungen völlig unabhängig von der Haupt-ASF-Version entwickeln und in aktuellen und zukünftigen ASF-Versionen wiederverwenden, solange die API kompatibel bleibt. Das in ASF verwendete Plugin-System basiert auf `System.Composition`, früher bekannt als **[Managed Extensibility Framework](https://docs.microsoft.com/dotnet/framework/mef)**, das es ASF ermöglicht, Ihre Bibliotheken während der Laufzeit zu entdecken und zu laden.

---

### Erste Schritte

Wir haben eine **[ASF-Erweiterungsvorlage](https://github.com/JustArchiNET/ASF-PluginTemplate)</a>** für Sie vorbereitet, welche als Basis für Ihr Plugin-Projekt verwendet werden kann. Die Verwendung der Vorlage ist nicht erforderlich (wenn Sie alles von Grund auf erledigen können), aber wir empfehlen sie dringend, da es Ihre Entwicklung beschleunigen und die Zeit verkürzen kann, die benötigt wird, um alles richtigzumachen. Die **[README](https://github.com/JustArchiNET/ASF-PluginTemplate/blob/main/README.md)** der Vorlage an wird Ihnen weiterhelfen. Unabhängig davon decken wir die Grundlagen unten ab, falls Sie bei null anfangen – oder die Konzepte in der Plugin-Vorlage besser verstehen möchten.

Ihr Projekt sollte eine Standard .NET-Bibliothek sein, die auf das geeignete Framework Ihrer Ziel-ASF-Version abzielt, wie in der **[Kompilierung](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compilation-de-DE)** angegeben.

Das Projekt muss auf die Haupt-Assembly `ArchiSteamFarm` verweisen, entweder auf die vorgefertigte `ArchiSteamFarm.dll`-Bibliothek, die Sie als Teil der Veröffentlichung heruntergeladen haben, oder auf das Quellprojekt (z. B. wenn Sie sich entschieden haben den ASF-Baum als Submodul hinzuzufügen). Dies ermöglicht es Ihrenauf ASF-Strukturen, -Methoden und -Eigenschaften zuzugreifen und diese zu entdecken, insbesondere auf das Core `IPlugin`-Interface, von dem Sie im nächsten Schritt erben musst. Das Projekt muss auch mindestens `System.Composition.AttributedModel` referenzieren, was es Ihren erlaubt, Ihr `IPlugin` mittels `[Export]` für ASF bereitzustellen. Darüber hinaus kann/muss man auf andere gemeinsame Bibliotheken verweisen, um die Datenstrukturen zu interpretieren, die man in manchen Schnittstellen erhält, aber wenn man sie nicht explizit benötigt, reicht das fürs Erste.

Wenn Sie alles richtig gemacht haben wird Ihre `csproj` Datei ähnlich wie unten aussehen:

```csproj
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>net8.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="System.Composition.AttributedModel" IncludeAssets="compile" Version="8.0.0" />
  </ItemGroup>

  <ItemGroup>
    <ProjectReference Include="C:\\Path\To\ArchiSteamFarm\ArchiSteamFarm.csproj" ExcludeAssets="all" Private="false" />

    <!-- If building with downloaded DLL binary, use this instead of <ProjectReference> above -->
    <!-- <Reference Include="ArchiSteamFarm" HintPath="C:\\Path\To\Downloaded\ArchiSteamFarm.dll" /> -->
  </ItemGroup>
</Project>
```

Von der Quellcode-Seite muss Ihre Plugin-Klasse von der Schnittstelle `IPlugin` erben (entweder explizit oder implizit von einer spezialisierteren Schnittstelle, wie `IASF`) und `[Export(typeof(IPlugin))]`, um von ASF während der Laufzeit erkannt zu werden. Das einfachste Beispiel, das dies ermöglicht, ist das folgende:

```csharp
using System;
using System.Composition;
using System.Threading.Tasks;
using ArchiSteamFarm;
using ArchiSteamFarm.Plugins;

namespace IhrNamespace.IhrPluginName;

[Export(typeof(IPlugin))]
public sealed class IhrPluginName : IPlugin {
    public string Name => nameof(IhrPluginName);
    public Version Version => typeof(IhrPluginName).Assembly.GetName().Version;

    public Task OnLoaded() {
        ASF.ArchiLogger.LogGenericInfo("Meow");

        return Task.CompletedTask;
    }
}
```

Um Ihr Plugin nutzen zu können, musst Sie es zunächst kompilieren. Sie können das entweder von Ihrer Entwicklungsumgebung aus oder aus dem Stammverzeichnis Ihres Projekts mittels eines Befehls tun:

```shell
# Wenn Ihr Projekt eigenständig ist (es ist nicht notwendig, seinen Namen zu definieren, da es das einzige ist)
dotnet publish -c "Release" -o "out"

# Wenn Ihr Projekt Teil des ASF-Quellbaums ist (um das Kompilieren unnötiger Teile zu vermeiden)
dotnet publish YourPluginName -c "Release" -o "out"
```

Danach ist Ihr Plugin einsatzbereit. Es liegt an dir, wie Sie Ihr Plugin verteilen und veröffentlichen möchten, aber wir empfehlen, ein Zip-Archiv mit einem einzigen Ordner namens `IhrNamespace.IhrPluginName` zu erstellen, in dem Sie Ihr kompiliertes Plugin mit seinen **[Abhängigkeiten](#plugin-abhängigkeiten)** kopieren. Auf diese Weise muss der Benutzer einfach sein Zip-Archiv in sein `plugins` Verzeichnis entpacken und nichts weiter tun.

Dies ist nur das grundlegendste Szenario, um zu beginnen. We have **[`ExamplePlugin`](https://github.com/JustArchiNET/ArchiSteamFarm/tree/main/ArchiSteamFarm.CustomPlugins.ExamplePlugin)** project that shows you example interfaces and actions that you can do within your own plugin, including helpful comments. Zögere nicht einen Blick darauf zu werfen, wenn Sie von einem funktionierenden Quellcode lernen möchten, oder entdecke den `ArchiSteamFarm.Plugins` Namespace selbst und schaue in die mitgelieferte Dokumentation für alle verfügbaren Optionen.

Falls Sie anstatt der Beispiel-Erweiterung von richtigen Projekten erfahren möchten, gibt es die **[`SteamTokenDumper`](https://github.com/JustArchiNET/ArchiSteamFarm/tree/main/ArchiSteamFarm.OfficialPlugins.SteamTokenDumper)**-Erweiterung, von uns entwickelt und mit ASF gebündelt wird. Weitere Erweiterungen von anderen Entwicklern werden in unserem **[Drittanbieter](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Third-party#asf-plugins-de-DE)**-Abschnitt vorgestellt.

---

### API Verfügbarkeit

ASF stellt Ihnen neben dem, worauf Sie in den Schnittstellen selbst Zugriff haben, viele seiner internen APIs zur Verfügung die Sie nutzen können um die Funktionalität zu erweitern. Wenn Sie zum Beispiel eine neue Art einer Steam-Web-Anfrage senden möchten, dann musst Sie nicht alles von Grund auf neu implementieren, insbesondere nicht all den Fragen mit denen wir uns vor Ihnen beschäftigt haben. Nutze einfach unseren `Bot.ArchiWebHandler`, der bereits eine Menge `UrlWithSession()`-Methoden für dich freilegt und alle untergeordneten Dinge wie Authentifizierung, Sitzungsaktualisierung oder Weblimitierung für dich handhabt. Likewise, for sending web requests outside of Steam platform, you could use standard .NET `HttpClient` class, but it's much better idea to use `Bot.ArchiWebHandler.WebBrowser` that is available for you, which once again offers you a helpful hand, for example in regards to retrying failed requests.

Wir haben eine sehr offene Richtlinie in Bezug auf unsere API-Verfügbarkeit, wenn Sie also etwas nutzen möchten das der ASF-Code bereits enthält, öffne einfach **[ein Issue](https://github.com/JustArchiNET/ArchiSteamFarm/issues)** und erkläre darin Ihren geplanten Anwendungsfall der internen ASF-API. Wir werden höchstwahrscheinlich nichts dagegen haben solange Ihr Anwendungsfall Sinn macht. Es ist einfach nicht möglich alles zu öffnen was jemand nutzen möchte, also haben wir das geöffnet, was für uns am sinnvollsten ist und warten auf Ihre Wünsche, falls Sie Zugang zu etwas haben möchten das bislang nicht `public` ist. Dazu gehören auch alle Vorschläge zu neuen `IPlugin`Schnittstellen die sinnvoll sein könnten, um die bestehende Funktionalität zu erweitern.

Tatsächlich ist die interne ASF-API die einzige wirkliche Einschränkung in Bezug auf das, was Ihr Plugin tun kann. Nichts hält dich davon ab, z. B. die `Discord.Net` Bibliothek in Ihre Anwendung aufzunehmen und eine Brücke zwischen Ihrem Discord-Bot und ASF-Befehlen zu schlagen, da Ihr Plugin auch eigenständige Abhängigkeiten haben kann. Die Möglichkeiten sind endlos und wir haben unser Bestes getan, um Ihnen so viel Freiheit und Flexibilität wie möglich innerhalb Ihrer Erweiterung zu geben; also gibt es keinerlei künstlichen Grenzen – nur sind wir uns nicht ganz sicher, welche ASF-Teile für Ihre Plugin-Entwicklung entscheidend sind (was Sie lösen können, indem Sie uns das Mitteilen), und Sie können auch ohne dem die Funktionalität, die Sie benötigen, jederzeit erneut implementieren.

---

### API Kompatibilität

Es ist wichtig zu betonen, dass ASF eine Verbraucher-Anwendung ist und nicht eine typische Bibliothek mit fester API-Oberfläche, auf die Sie sich bedingungslos verlassen können. Es ist somit unmöglich zu erwarten, dass die von Ihnen erstellte Erweiterung auch in den kommenden ASF-Versionen weiterhin funktioniert. Wenn wir das Programm weiterentwickeln wollen, um sich an ständig auftretende Steam-Änderungen anzupassen, dann sind Abstriche bei der Rückwärtskompatibilität früher oder später erforderlich. Es für Ihren Fall schlichtweg nicht angemessen, ausschließlich auf die Rückwärtskompatibilität zu achten. Das sollte für dich logisch sein, aber es ist wichtig diese Tatsache hervorzuheben.

Wir werden unser Bestes tun um die öffentlichen Teile von ASF funktionsfähig und stabil zu halten, aber wir werden keine Angst davor haben die Kompatibilität zu brechen, wenn gute Gründe dafür vorliegen, da wir unserer **[Veraltete Funktionen](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Deprecation-de-DE)** Politik folgen. Dies ist besonders wichtig für interne ASF-Strukturen, die Ihnen als Teil der ASF-Infrastruktur zur Verfügung stehen, wie oben erläutert (z. B. `ArchiWebHandler`), die im Rahmen von ASF-Erweiterungen in einer der zukünftigen Versionen verbessert (und damit neu geschrieben) werden können. Wir werden unser Bestes tun, um dich in den Änderungsprotokollen angemessen zu informieren und während der Laufzeit entsprechende Warnungen über veraltete Funktionen zu geben. Wir haben nicht die Absicht alles neu zu schreiben, um es neu zu schreiben, also können Sie ziemlich sicher sein, dass die nächste kleinere ASF-Version Ihr Plugin nicht einfach nur deshalb komplett zerstört, weil es eine höhere Versionsnummer hat. Aber es ist eine gute Idee ein Auge auf Änderungsprotokolle zu werden und gelegentlich zu überprüfen ob alles einwandfrei funktioniert.

---

### Plugin Abhängigkeiten

Ihr Plugin enthält standardmäßig mindestens zwei Abhängigkeiten, die `ArchiSteamFarm` Referenz für die interne API und `PackageReference` von `System.Composition.AttributedModel`, die erforderlich ist, um als ASF-Plugin erkannt zu werden. Darüber hinaus könnte es mehr Abhängigkeiten in Bezug auf das, was Sie in Ihrem Plugin getan haben, enthalten (z. B. `Discord.Net` Bibliothek, wenn Sie sich für die Integration mit Discord entschieden haben).

The output of your build will include your core `YourPluginName.dll` library, as well as all the dependencies that you've referenced. Since you're developing a plugin to already-working program, you don't have to, and even **shouldn't** include dependencies that ASF already includes, for example `ArchiSteamFarm`, `SteamKit2` or `Newtonsoft.Json`. Das Entfernen von mit ASF geteilten Abhängigkeiten in Ihrem Build ist nicht die absolute Voraussetzung für die Funktionsfähigkeit eines Plugins, aber dies wird den Speicherbedarf und die Größe des Plugins drastisch reduzieren und gleichzeitig die Leistung erhöhen, da ASF seine eigenen Abhängigkeiten mit Ihnen teilt und nur die Bibliotheken lädt, die es nicht über sich selbst kennt.

Generell wird empfohlen, nur Bibliotheken einzubinden, die ASF entweder gar nicht oder nur in der falschen/inkompatiblen Version enthält. Beispiele dafür wären natürlich `DeinPluginName.dll`, aber zum Beispiel auch `Discord.Net.dll`, falls Sie sich entschieden haben sollten, darauf zurückzugreifen, da ASF diese nicht selbst mitbringt. Das Bündeln von Bibliotheken, die gemeinsam mit ASF genutzt werden, kann immer noch sinnvoll sein, wenn Sie die API-Kompatibilität sicherstellen möchten (z. B. sicherstellen, dass `Newtonsoft.Json`, von dem Sie in Ihrer Erweiterung abhängig sind, immer in der Version `X` und nicht in der Version ist, in der ASF sie bereitstellt); allerdings kommt offensichtlich dies auf Kosten von mehr Speicher/Größe und schlechterer Leistung.

If you know that the dependency which you need is included in ASF, you can mark it with `IncludeAssets="compile"` as we showed you in the example `csproj` above. Dies wird dem Compiler mitteilen, um die Veröffentlichung referenzierter Bibliothek selbst zu vermeiden, da ASF diese bereits beinhaltet. Likewise, notice that we reference the ASF project with `ExcludeAssets="all" Private="false"` which works in a very similar way - telling the compiler to not produce any ASF files (as the user already has them). Dies gilt nur, wenn das ASF-Projekt referenziert wird, da Sie eine `dll` Bibliothek referenzieren, dann produzieren Sie keine ASF-Dateien als Teil Ihrer Erweiterung.

Wenn Sie über die obige Erklärung verwirrt sind und Sie es nicht besser weißt, überprüfe, welche `dll` Bibliotheken im Paket `ASF-generic.zip` enthalten sind und stelle sicher, dass Ihr Plugin nur die enthält, die bislang nicht Teil davon sind. Für die einfachsten Erweiterungen wird dies nur `IhrPluginName.dll` sein. Wenn Sie während der Laufzeit Probleme in Bezug auf einige Bibliotheken bekommst, füge auch die betroffenen Bibliotheken hinzu. Wenn alles andere fehlschlägt, können Sie sich jederzeit entscheiden, alles zu bündeln.

---

### Native Abhängigkeiten

Native Abhängigkeiten werden als Teil von betriebssystemspezifischen Builds generiert, da auf dem Host keine .NET Runtime verfügbar ist und ASF seine eigene .NET Runtime nutzt, die als Teil des betriebssystemspezifischen Builds gebündelt wird. Um die Größe des Builds zu minimieren, reduziert ASF seine nativen Abhängigkeiten so, dass nur der Programmcode berücksichtigt wird, der innerhalb des Programms erreichbar ist, was die ungenutzten Teile der Laufzeit effektiv reduziert. Es könnte sich um ein potenzielles Hindernis für Ihre Erweiterung handeln, falls Sie unerwartet eine Situation erleben, in der das Plugin eine .NET-Funktion aufweist, die nicht in ASF integriert ist. Deshalb können OS-spezifische Builds nicht richtig funktionieren und resultiert bei dem Versuch für gewöhnlich in die Fehler `System.MissingMethodException` oder `System.Reflection.ReflectionTypeLoadException`.

Dies ist bei generischen Builds nie ein Problem, da es sich hierbei nie um native Abhängigkeiten handelt (da sie die gesamte lauffähige Runtime auf dem Host haben und ASF ausführen). Es ist auch automatisch eine Lösung des Problems, indem **Sie Ihr Plugin ausschließlich in generischen Builds verwenden**, aber offensichtlich hat das seinen eigenen Nachteil, wenn es darum geht, Ihr Plugin von Benutzern zu trennen, die OS-spezifische Builds von ASF ausführen. Wenn Sie sich fragen ob Ihr Problem im Zusammenhang mit nativen Abhängigkeiten steht, können Sie diese Methode auch zur Überprüfung verwenden. Lade Ihr Plugin in der generischen ASF Variante und schauen Sie ob es funktioniert. Wenn dies der Fall ist, sind die Plugin-Abhängigkeiten abgedeckt, sodass native Abhängigkeiten Probleme verursachen.

Unfortunately, we had to make a hard choice between publishing whole runtime as part of our OS-specific builds, and deciding to cut it out of unused features, making the build over 50 MB smaller compared to the full one. Wir haben die zweite Option ausgewählt, und es ist leider unmöglich für Sie, die fehlenden Laufzeitfunktionen zusammen mit Ihrer Erweiterung hinzuzufügen. If your project requires access to runtime features that are left out, you have to include full .NET runtime that you depend on, and that means running your plugin together with `generic` ASF flavour. You can't run your plugin in OS-specific builds, as those builds are simply missing a runtime feature that you need, and .NET runtime as of now is unable to "merge" native dependency that you could've provided with our own. Vielleicht wird es sich eines Tages bessern, aber zum jetzigen Zeitpunkt ist es einfach nicht möglich.

OS-spezifische -Builds von ASF enthalten das Minimum an zusätzlicher Funktionalität, das für unsere offiziellen Erweiterungen erforderlich ist. Abgesehen davon, dass dies möglich ist, erweitert dies auch die Oberfläche leicht auf zusätzliche Abhängigkeiten für die grundlegendsten Erweiterungen. Daher müssen sich nicht alle Erweiterungen um native Abhängigkeiten kümmern – nur solche, die über das hinausgehen, was ASF und unsere offiziellen Plugins direkt benötigen. This is done as an extra, since if we need to include additional native dependencies ourselves for our own use cases anyway, we can as well ship them directly with ASF, making them available, and therefore easier to cover, also for you. Leider reicht das nicht immer aus, und da Ihr Plugin größer und komplexer wird, erhöht sich die Wahrscheinlichkeit, dass Sie in eingeschränkte Funktionalität laufen können. Daher empfehlen wir Ihnen normalerweise Ihre benutzerdefinierten Erweiterungen ausschließlich mit der `generic` ASF-Variante zu betreiben. Sie können immer noch manuell überprüfen, ob OS-spezifische ASF-Versionen alles haben, was die Erweiterung für seine Funktionalität benötigt – aber da sich dies sowohl bei Ihren, als auch bei unserem Update ändert, könnte es schwierig sein dieses zu pflegen.