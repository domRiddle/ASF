# Plugins

Ab ASF V4 bietet das Programm Unterstützung für benutzerdefinierte Plugins, die während der Laufzeit geladen werden können. Plugins ermöglichen es dir, das ASF-Verhalten anzupassen, z.B. durch Hinzufügen von benutzerdefinierten Befehlen, einer benutzerdefinierten Handelslogik oder der vollständigen Integration mit Diensten und APIs von Drittanbietern.

* * *

## Für Benutzer

ASF lädt Plugins aus dem Verzeichnis `plugins`, das sich in deinem ASF-Ordner befindet. Es wird empfohlen, für jedes Plugin das du verwenden möchtest ein eigenes Verzeichnis zu erstellen, das auf seinem Namen basieren kann, wie z.B. `MeinPlugin`. Dies führt zur finalen Baumstruktur von `plugins/MeinPlugin`. Schließlich sollten alle Binärdateien des Plugins in diesem speziellen Ordner abgelegt werden. ASF wird dein Plugin nach dem Neustart ordnungsgemäß erkennen und verwenden.

Normalerweise veröffentlichen Plugin-Entwickler ihre Plugins in Form einer `zip` Datei mit bereits vorbereiteter Struktur für dich, so dass es genügt, das Zip-Archiv in das Verzeichnis `plugins` zu entpacken, das automatisch den entsprechenden Ordner erstellt.

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
    <TargetFramework>netcoreapp2.2</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="System.Composition.AttributedModel" Version="*" />
  </ItemGroup>

  <ItemGroup>
    <Reference Include="ArchiSteamFarm">
      <HintPath>C:\\Pfad\Zu\Heruntergeladenen\ArchiSteamFarm.dll</HintPath>
    </Reference>

    <!-- Wenn du als Teil des ASF-Quellbaums erstellst, verwende dies anstelle von <Reference> oben -->
    <!-- <ProjectReference Include="C:\\Pfad\Zu\ArchiSteamFarm\ArchiSteamFarm.csproj" /> -->
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

Dies ist jedoch nur das Basisszenario um dir den Einstieg zu erleichtern. Wir haben ein **[`ExamplePlugin`](https://github.com/JustArchiNET/ArchiSteamFarm/tree/master/ArchiSteamFarm.CustomPlugins.ExamplePlugin)** Projekt, das dir exemplarische Schnittstellen und Aktionen zeigt, die du innerhalb deines eigenen Plugins durchführen kannst, einschließlich hilfreicher Kommentare. Zögere nicht einen Blick darauf zu werfen, wenn du von einem funktionierenden Quellcode lernen möchtest, oder entdecke den `ArchiSteamFarm.Plugins` Namespace selbst und schaue in die mitgelieferte Dokumentation für alle verfügbaren Optionen.

* * *

### API Verfügbarkeit

ASF stellt dir neben dem, worauf du in den Schnittstellen selbst Zugriff hast, viele seiner internen APIs zur Verfügung die du nutzen kannst um die Funktionalität zu erweitern. Wenn du zum Beispiel eine neue Art einer Steam-Web-Anfrage senden möchtest, dann musst du nicht alles von Grund auf neu implementieren, insbesondere nicht all den Fragen mit denen wir uns vor dir beschäftigt haben. Nutze einfach unseren `Bot.ArchiWebHandler`, der bereits eine Menge `UrlWithSession()`-Methoden für dich freilegt und alle untergeordneten Dinge wie Authentifizierung, Sitzungsaktualisierung oder Weblimitierung für dich handhabt.

Wir haben eine sehr offene Richtlinie in Bezug auf unsere API-Verfügbarkeit, wenn du also etwas nutzen möchtest das der ASF-Code bereits enthält, öffne einfach **[ein Issue](https://github.com/JustArchiNET/ArchiSteamFarm/issues)** und erkläre darin deinen geplanten Anwendungsfall der internen ASF-API. Wir werden höchstwahrscheinlich nichts dagegen haben solange dein Anwendungsfall Sinn macht. Es ist einfach nicht möglich alles zu öffnen was jemand nutzen möchte, also haben wir das geöffnet, was für uns am sinnvollsten ist und warten auf deine Wünsche, falls du Zugang zu etwas haben möchtest das noch nicht `public` ist. Dazu gehören auch alle Vorschläge zu neuen `IPlugin`Schnittstellen die sinnvoll sein könnten, um die bestehende Funktionalität zu erweitern.

Tatsächlich ist die interne ASF-API die einzige wirkliche Einschränkung in Bezug auf das, was dein Plugin tun kann. Nichts hält dich davon ab, z.B. die `Discord.Net` Bibliothek in deine Anwendung aufzunehmen und eine Brücke zwischen deinem Discord-Bot und ASF-Befehlen zu schlagen, da dein Plugin auch eigenständige Abhängigkeiten haben kann. Die Möglichkeiten sind endlos und wir haben unser Bestes getan, um dir so viel Freiheit und Flexibilität wie möglich innerhalb deines Plugins zu geben, also gibt es keine künstlichen Grenzen für irgendetwas, nur dass wir nicht ganz sicher sind welche ASF-Teile für deine Plugin-Entwicklung entscheidend sind (was du lösen kannst indem du uns das mitteilst).

* * *

### API Kompatibilität

Es ist wichtig zu betonen, dass ASF eine Verbraucher-Anwendung ist und nicht eine typische Bibliothek mit fester API-Oberfläche, auf die du dich bedingungslos verlassen kannst. Das bedeutet, dass du nicht davon ausgehen kannst, dass dein einmal kompiliertes Plugin mit allen zukünftigen ASF-Versionen weiter funktioniert. Es ist einfach unmöglich, wenn du das Programm weiter entwickeln willst und nicht in der Lage bist dich an ständig laufende Steam-Änderungen anzupassen, um der Rückwärtskompatibilität willen, ist einfach nicht angemessen für unseren Fall. Das sollte für dich logisch sein, aber es ist wichtig diese Tatsache hervorzuheben.

Wir werden unser Bestes tun um die öffentlichen Teile von ASF funktionsfähig und stabil zu halten, aber wir werden keine Angst davor haben die Kompatibilität zu brechen, wenn gute Gründe dafür vorliegen, da wir unserer **[Veraltete Funktionen](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Deprecation-de-DE)** Politik folgen. Dies ist besonders wichtig für interne ASF-Strukturen, die dir als Teil der ASF-Infrastruktur zur Verfügung stehen, wie oben erläutert (z.B. `ArchiWebHandler`), die im Rahmen von ASF-Erweiterungen in einer der zukünftigen Versionen verbessert (und damit neu geschrieben) werden können. Wir werden unser Bestes tun, um dich in den Änderungsprotokollen angemessen zu informieren und während der Laufzeit entsprechende Warnungen über veraltete Funktionen zu geben. Wir haben nicht die Absicht alles neu zu schreiben, um es neu zu schreiben, also kannst du ziemlich sicher sein, dass die nächste kleinere ASF-Version dein Plugin nicht einfach nur deshalb komplett zerstört, weil es eine höhere Versionsnummer hat. Aber es ist eine gute Idee ein Auge auf Änderungsprotokolle zu werden und gelegentlich zu überprüfen ob alles einwandfrei funktioniert.

* * *

### Plugin Abhängigkeiten

Dein Plugin enthält standardmäßig mindestens zwei Abhängigkeiten, die `ArchiSteamFarm` Referenz für die interne API und `PackageReference` von `System.Composition.AttributedModel`, die erforderlich ist, um als ASF-Plugin erkannt zu werden. Darüber hinaus könnte es mehr Abhängigkeiten in Bezug auf das, was du in deinem Plugin getan hast, enthalten (z.B. `Discord.Net` Bibliothek, wenn du dich für die Integration mit Discord entschieden hast).

Die Ausgabe deines Builds beinhaltet deine Kernbibliothek `DeinPluginName.dll` und alle Abhängigkeiten, auf die du verwiesen hast, mindestens jedoch `ArchiSteamFarm.dll` und `System.Composition.AttributedModel.dll`.

Da du ein Plugin für ein bereits funktionierendes Programm entwickelst, brauchst du nicht, ja sogar **solltest du nicht** alle Abhängigkeiten einbinden, die für dich während des Builds generiert wurden. Denn ASF beinhaltet bereits die meisten davon, zum Beispiel `ArchiSteamFarm`, `SteamKit2` oder `Newtonsoft.Json`. Das Entfernen von mit ASF geteilten Abhängigkeiten in deinem Build ist nicht die absolute Voraussetzung für die Funktionsfähigkeit deines Plugins, aber dies wird den Speicherbedarf und die Größe deines Plugins drastisch reduzieren und gleichzeitig die Leistung erhöhen, da ASF seine eigenen Abhängigkeiten mit dir teilt und nur die Bibliotheken lädt, die es nicht über sich selbst kennt.

Daher wird empfohlen, nur Bibliotheken einzubinden, die ASF entweder nicht enthält oder in der falschen/inkompatiblen Version enthält. Beispiele dafür wären natürlich `DeinPluginName.dll`, aber zum Beispiel auch `Discord.Net.dll`, wenn du dich entschieden haben solltest, darauf zurückzugreifen. Das Bündeln von Bibliotheken die gemeinsam mit ASF genutzt werden, kann immer noch sinnvoll sein, wenn du die API-Kompatibilität sicherstellen willst (z.B. sicherstellen, dass `Newtonsoft.Json`, von dem du in deinem Plugin abhängig bist, immer in der Version `X` und nicht in der Version ist, in der ASF sie bereitstellt), aber offensichtlich wird dies zu einem Preis von mehr Speicher/Größe und schlechterer Leistung angeboten.

Wenn du über die obige Erklärung verwirrt bist und du es nicht besser weißt, überprüfe, welche `dll` Bibliotheken im Paket `ASF-generic.zip` enthalten sind und stelle sicher, dass dein Plugin nur die enthält, die noch nicht Teil davon sind. Für die einfachsten Plugins wird dies nur `DeinPluginName.dll` sein. Wenn du während der Laufzeit Probleme in Bezug auf einige Bibliotheken bekommst, füge auch die betroffenen Bibliotheken hinzu. Wenn alles andere fehlschlägt, kannst du dich jederzeit entscheiden, alles zu bündeln.

* * *

### Native Abhängigkeiten

Native Abhängigkeiten werden als Teil von betriebssystemspezifischen Builds generiert, da auf dem Host keine .NET Core Runtime verfügbar ist und ASF seine eigene .NET Core Runtime nutzt, die als Teil des betriebssystemspezifischen Builds gebündelt wird. Um die Größe des Builds zu minimieren, reduziert ASF seine nativen Abhängigkeiten so, dass nur der Programmcode berücksichtigt wird, der innerhalb des Programms erreichbar ist, was die ungenutzten Teile der Laufzeit effektiv reduziert. Dies kann ein potenzielles Problem für dich in Bezug auf dein Plugin darstellen, wenn du dich plötzlich in einer Situation wiederfindest, in der dein Plugin von einer Funktion abhängt, die nicht in ASF verwendet wird, und daher können OS-spezifische Builds es nicht richtig ausführen.

Dies ist bei generischen Builds nie ein Problem, da es sich hierbei nie um native Abhängigkeiten handelt (da sie die gesamte lauffähige Runtime auf dem Host haben und ASF ausführen). Es ist auch automatisch eine Lösung des Problems, indem **du dein Plugin ausschließlich in generischen Builds verwendest**, aber offensichtlich hat das seinen eigenen Nachteil, wenn es darum geht, dein Plugin von Benutzern zu trennen, die OS-spezifische Builds von ASF ausführen. Wenn du dich fragst ob dein Problem im Zusammenhang mit nativen Abhängigkeiten steht, kannst du diese Methode auch zur Überprüfung verwenden. Lade dein Plugin in der generischen ASF Variante und sieh ob es funktioniert. Wenn dies der Fall ist sind die Plugin-Abhängigkeiten abgedeckt, so dass nur native Abhängigkeiten bearbeitet werden können.

Glücklicherweise ist die eigentliche Lösung des Problems, sehr ähnlich den allgemeinen Plugin-Abhängigkeiten, wieder einmal die Bündelung deines Plugins mit den Abhängigkeiten, die ASF entweder nicht selbst hat oder in einer falschen/inkompatiblen (z.B. durch Reduzierung) Version hat. Im Vergleich zu den Plugin-Abhängigkeiten kannst du dir nicht sicher sein, ob die gekürzte Version der nativen ASF-Abhängigkeit alles hat, was du brauchst, also kannst du dich entweder entscheiden den einfachen Weg zu gehen und einfach alles zu bündeln oder dich anstrengen und manuell überprüfen welche Teile in ASF fehlen und nur diese Teile einfügen.

Dies bedeutet auch, dass **du eventuell einen separaten Build deines Plugins für jede ASF-Variante** haben musst, da in jedem betriebssystemspezifische ASF-Build unterschiedliche Features fehlen können die du dann selbst bereitstellen musst und dein generischer Plugin-Build nicht alle von ihnen alleine bereitstellen kann. Dies hängt stark davon ab, was dein Plugin tatsächlich tut und wovon es abhängt, da sehr einfache Plugins, die rein auf ASF-Funktionen basieren, garantiert in allen Konfigurationen gut funktionieren, da sie keine eigenen Abhängigkeiten mitbringen und alle nativen Abhängigkeiten per Definition abgedeckt sind. Kompliziertere Plugins (insbesondere solche, die selbst Abhängigkeiten haben) müssen möglicherweise zusätzliche Maßnahmen ergreifen, um sicherzustellen, dass sie tatsächlich alle erforderlichen Codeteile bereitstellen, nicht nur hochrangige Plugin-Abhängigkeiten (siehe Abschnitt oben), sondern auch niedrigrangige native Abhängigkeiten. Wenn alles andere fehlschlägt, wie oben beschrieben, kannst du dein Plugin immer für die gleiche betriebssystemspezifische Variante kompilieren, die du verwenden möchtest und einfach alle erzeugten Abhängigkeiten bündeln.