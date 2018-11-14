# Hochleistungs-Setup

Dies ist genau das Gegenteil von **[Speichereffizientes Setup](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup-de-DE)** und normalerweise möchtest du diesen Tipps folgen, wenn du die ASF-Leistung (in Bezug auf die CPU-Geschwindigkeit) weiter erhöhen willst, für potenzielle Kosten einer erhöhten Speichernutzung.

* * *

ASF versucht bereits die Performance zu bevorzugen, wenn es um die allgemeine ausgewogene Abstimmung geht. Daher gibt es nicht viel was man tun kann um die Performance weiter zu steigern, obwohl man auch nicht völlig ohne Möglichkeiten da steht. Beachte jedoch, dass diese Optionen standardmäßig nicht aktiviert sind, was bedeutet, dass sie nicht gut genug sind, um sie für die Mehrheit der Anwendungen als ausgewogen zu betrachten. Deshalb solltest du selbst entscheiden, ob diese Optionen zur Speichererweiterung für dich akzeptabel sind.

* * *

## Laufzeitoptimierung (Erweitert)

Die folgenden Tricks **beanspruchen eine ernsthafte Speicherzunahme** und sollten mit Vorsicht verwendet werden.

`ArchiSteamFarm.runtimeconfig.json` ermöglicht es dir, die ASF-Runtime einzustellen, insbesondere zwischen Server GC und Workstation GC zu wechseln.

> Der Garbage Collector ist selbstoptimierend und kann in einer Vielzahl von Szenarien eingesetzt werden. Du kannst mit Hilfe einer Einstellung in der Konfigurationsdatei die Art der Garbage Collection basierend auf den Eigenschaften der Systemlast festlegen. Die CLR bietet die folgenden Arten von Garbage Collection:
> 
> Workstation Garbage Collection, die für alle Client-Arbeitsplätze und Einzel-PCs gilt. Dies ist die Standardeinstellung für das <gcserver> Element im Runtime-Konfigurationsschema.
> 
> Server Garbage Collection, die für Serveranwendungen bestimmt ist, die einen hohen Datendurchsatz und Skalierbarkeit erfordern. Die Server Garbage Collection kann sowohl nicht zeitgleich als auch im Hintergrund erfolgen.

Weitere Informationen findest du unter **[Grundlagen der Garbage Collection](https://docs.microsoft.com/en-us/dotnet/standard/garbage-collection/fundamentals)**.

ASF verwendet standardmäßig die Garbage Collection der Workstation. Dies liegt vor allem an einem ausgewogenen Verhältnis zwischen Speichernutzung und Performance, das für wenige Bots mehr als ausreichend ist, da normalerweise ein einzelner gleichzeitiger Hintergrund-GC-Thread schnell genug ist um den gesamten von ASF zugewiesenen Speicher zu bewältigen.

Heutzutage haben wir jedoch eine Menge an CPU-Kernen, von denen ASF sehr profitieren kann, indem wir für jede verfügbare CPU vCore einen eigenen GC-Thread bereitstellen. Dies kann die Leistung bei komplexen ASF-Aufgaben wie dem Parsen von Abzeichen-Seiten oder dem Inventar erheblich verbessern, da jede CPU vCore helfen kann, im Gegensatz zu nur 2 (Haupt und GC). Server GC wird für Maschinen mit 3 CPU vCores und mehr empfohlen, Workstation GC wird automatisch erzwungen, wenn deine Maschine nur 1 CPU vCore hat, und wenn du genau 2 hast, dann kannst du beide ausprobieren (die Ergebnisse können variieren).

Du kannst den Server GC aktivieren, indem du in der `ArchiSteamFarm.runtimeconfig.json` Datei die `System.GC.Server` Eigenschaft von `false` auf `true` umstellst. Bedenke, dass du es möglicherweise mehr als einmal tun musst, da ASF nach dem automatischen Update standardmäßig immer noch `false` verwendet.

Server GC selbst führt nicht zu einer sehr großen Speicherzunahme, wenn er einfach nur aktiv ist, aber er hat viel größere Generationsgrößen und ist daher viel fauler, wenn es darum geht, dem Betriebssystem Speicher zurückzugeben. Du befindest dich möglicherweise an einem Sweet Spot, an dem Server GC die Leistung signifikant erhöht und du sie weiterhin nutzen möchtest, aber gleichzeitig kannst du dir nicht leisten, dass der enorme Speicherzuwachs, der durch die Verwendung entsteht, zunimmt. Zum Glück für dich gibt es eine "Das Beste aus beiden Welten"-Einstellung, indem du den Server GC mit **[GC latency level](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup-de-DE#gclatencylevel)** auf `0` gesetzt hast, was immer noch Server GC ermöglicht, aber die Größe der Generierung einschränkt und mehr auf den Speicher setzt.

Wenn der Speicher jedoch kein Problem für dich ist (da GC immer noch deinen verfügbaren Speicher berücksichtigt und sich selbst optimiert), ist es viel besser, `GCLatencyLevel` überhaupt nicht zu ändern, da man so eine höhere Leistung als Ergebnis erlangt.

* * *

## Empfohlene Optimierung

- Vergewissere dich, dass du den Standardwert `MaxPerformance` für `OptimizationMode` verwendest. Dies ist bei weitem die wichtigste Einstellung, da die Verwendung des Wertes `MinMemoryUsage` dramatische Auswirkungen auf die Performance hat.
- Aktiviere den Server GC, indem du in der `ArchiSteamFarm.runtimeconfig.json` Datei die `System.GC.Server` Eigenschaft von `false` auf `true` umstellst. Dadurch wird der Server-GC aktiviert, der durch eine Speichererhöhung im Vergleich zum Arbeitsplatz-GC sofort als aktiv angesehen werden kann.
- Falls du dir eine solche Speicherzunahme nicht leisten kannst, solltest du `GCLatencyLevel` mit einem Wert von `0` verwenden um "das Beste aus beiden Welten" zu erreichen. Wenn dein Speicher es sich jedoch leisten kann, dann ist es besser, es bei der Standardeinstellung zu belassen - Server GC optimiert sich bereits während der Laufzeit und ist intelligent genug um weniger Speicher zu verbrauchen, wenn dein Betriebssystem es wirklich benötigt.

Wenn du den Server GC aktiviert hast und `GCLatencyLevel` auf Standardeinstellung belassen hast, dann hast du eine überragende ASF-Performance, die auch bei Hunderten oder Tausenden von aktivierten Bots blitzschnell sein sollte. Die CPU sollte kein Engpass mehr sein, da ASF in der Lage ist, bei Bedarf die gesamte CPU-Leistung zu nutzen, was die benötigte Zeit auf ein Minimum reduziert.