# Speichereffizientes Setup

Dies ist genau das Gegenteil von **[Hochleistungs-Setup](https://github. com/JustArchiNET/ArchiSteamFarm/wiki/High-performance-setup-de-DE)** und normalerweise möchtest du diesen Tipps folgen, wenn du den Speicherverbrauch von ASF verringern willst, um die Gesamtbelastung zu senken.

* * *

ASF ist per Definition extrem ressourcenschonend und je nach Nutzung ist sogar ein 128 MB VPS mit Linux in der Lage, es auszuführen, obwohl es nicht empfohlen wird so niedrig zu gehen da dies zu verschiedenen Problemen führen kann. Obwohl ASF leicht ist, hat es keine Angst davor, das Betriebssystem nach mehr Speicher zu fragen, wenn Speicher benötigt wird, damit ASF mit optimaler Geschwindigkeit arbeiten kann.

ASF als Anwendung versucht, so optimiert und effizient wie möglich zu sein, wobei auch die während der Ausführung verwendeten Ressourcen berücksichtigt werden. Wenn es um Speicher geht, zieht ASF die Performance dem Speicherverbrauch vor, was zu temporären Speicherspitzen führen kann, was z.B. bei Konten mit mehr als 3 Abzeichen-Seiten auffällt, da ASF die erste Seite abruft und analysiert, daraus die Gesamtzahl der Seiten liest und dann für jede zusätzliche Seite die Abrufaufgabe startet, was zum gleichzeitigen Abrufen und Parsen der verbleibenden Seiten führt. Dieser "zusätzliche" Speicherverbrauch (im Vergleich zu einem absoluten Minimum für den Betrieb) kann die Ausführung und die Gesamtleistung drastisch beschleunigen, und zwar auf Kosten des erhöhten Speicherverbrauchs, der erforderlich ist um all diese Dinge parallel auszuführen. Ähnliches geschieht mit allen anderen allgemeinen ASF-Aufgaben, die parallel ausgeführt werden können, z.B. mit dem Parsen aktiver Handelsangebote, ASF kann sie alle auf einmal analysieren, da sie alle unabhängig voneinander agieren. Darüber hinaus wird ASF (C# Runtime) ungenutzten Speicher **nicht** sofort danach an das Betriebssystem zurückgeben, was man in Form eines ASF-Prozesses schnell bemerken kann, der nur mehr und mehr Speicher benötigt, aber diesen Speicher nie an das Betriebssystem zurückgibt. Einige Leute mögen es bereits fragwürdig finden, vielleicht sogar ein Memory-Leak vermuten, aber keine Sorge, all das ist zu erwarten.

ASF ist extrem gut optimiert und nutzt die vorhandenen Ressourcen so gut wie möglich. Eine hohe Speicherauslastung von ASF bedeutet nicht, dass ASF aktiv diesen Speicher **verwendet** und ** ihn benötigt**. Sehr oft behält ASF zugewiesenen Speicher als "Raum" für zukünftige Aktionen, da wir die Leistung drastisch verbessern können, wenn wir nicht für jeden Speicherabschnitt, den wir verwenden wollen, das Betriebssystem fragen müssen. Die Runtime sollte automatisch ungenutzten ASF-Speicher an das Betriebssystem zurückgeben, wenn das Betriebssystem ihn **wirklich** benötigt. **[Ungenutzter Speicher ist verschwendeter Speicher](https://www.howtogeek.com/128130/htg-explains-why-its-good-that-your-computers-ram-is-full)**. Du stößt auf Probleme, wenn der Speicher, den du **benötigst** höher ist als der Speicher, der für dich verfügbar ist, nicht wenn ASF einige zusätzliche zugewiesene Ressourcen behält, mit dem Ziel, Funktionen zu beschleunigen, die in einem Moment ausgeführt werden. Du stößt auf Probleme, wenn dein Linux-Kernel den ASF-Prozess aufgrund von OOM (out of memory) beendet, nicht wenn du den ASF-Prozess als Top-Speicherverbraucher in `htop` siehst.

Garbage collector being used in ASF is a very complex mechanism, smart enough to take into account not only ASF itself, but also your OS and other processes. When you have a lot of free memory, ASF will ask for whatever is needed to improve the performance. This can be even as much as 1 GB (with server GC). When your OS memory is close to being full, ASF will automatically release some of it back to the OS to help things settle down, which can result in overall ASF memory usage as low as 50 MB. The difference between 50 MB and 1 GB is huge, but so is the difference between small 512 MB VPS and huge dedicated server with 32 GB. If ASF can guarantee that this memory will come useful, and at the same time nothing else requires it right now, it'll prefer to keep it and automatically optimize itself based on routines that were executed in the past. The GC used in ASF is self-tuning and will achieve better results the longer the process is running.

This is also why ASF process memory varies from setup to setup, as ASF will do its best to use available resources in **as efficient way as possible**, and not in a fixed way like it was done during Windows XP times. The actual (real) memory usage that ASF is using can be verified with `stats` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**, and is usually around 4 MB for just a few bots, up to 30 MB if you use stuff like **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** and other advanced features. Keep in mind that memory returned by `stats` command also includes free memory that hasn't been reclaimed by garbage collector yet. Everything else is shared runtime memory (around 40-50 MB) and room for execution (vary). This is also why the same ASF can use as little as 50 MB in low-memory VPS environment, while using even up to 1 GB on your desktop. ASF is actively adapting to your environment and will try to find optimal balance in order to neither put your OS under pressure, nor limit its own performance when you have a lot of unused memory that could be put in use.

* * *

Natürlich gibt es viele Möglichkeiten, wie du ASF helfen kannst die richtige Richtung in Bezug auf den Speicher den du verwenden möchtest zu lenken. Im Allgemeinen ist es am besten, den Garbage-Collector in Ruhe arbeiten zu lassen und das zu tun, was er für das Beste hält. But this is not always possible, for example if your Linux server is also hosting several websites, MySQL database and PHP workers, then you can't really afford ASF shrinking itself when you run close to OOM, as it's usually too late and performance degradation comes sooner. This is usually when you might be interested in further tuning, and therefore reading this page.

Die folgenden Vorschläge sind in einige Kategorien unterteilt, mit unterschiedlichem Schwierigkeitsgrad.

* * *

## ASF Einrichtung (einfach)

Below tricks **do not affect performance negatively** and can be safely applied to all setups.

- Never run more than one ASF instance. ASF is meant to handle unlimited number of bots all at once, and unless you're binding every ASF instance to different interface/IP address, you should have exactly **one** ASF process, with multiple bots (if needed).
- Make use of `ShutdownOnFarmingFinished`. Active bot takes more resources than deactivated one. It's not a significant save, as the state of bot still needs to be kept, but you're saving some amount of resources, especially all resources related to networking, such as TCP sockets. You need only one active bot to keep ASF instance running, and you can always bring up other bots if needed.
- Keep your bots number low. Not `Enabled` bot instance takes less resources, as ASF doesn't bother starting it. Also keep in mind that ASF has to create a bot for each of your configs, therefore if you don't need to `start` given bot and you want to save some extra memory, you can temporarily rename `Bot.json` to e.g. `Bot.json.bak` in order to avoid creating state for your disabled bot instance in ASF. This way you won't be able to `start` it without renaming it back, but ASF also won't bother keeping state of this bot in memory, leaving room for other things (very small save, in 99.9% cases you shouldn't bother with it, just keep your bots with `Enabled` of `false`).
- Fine-tune your configs. Especially global ASF config has many variables to adjust, for example by increasing `LoginLimiterDelay` you'll bring up your bots slower, which will allow already started instance to fetch badges in the meantime, as opposed to bringing up your bots faster, which will take more resources as more bots will do major work (such as parsing badges) at the same time. The less work that has to be done at the same time - the less memory used.

Those are a few things you can keep in mind when dealing with memory usage. However, those things don't have any "crucial" matter on memory usage, because memory usage comes mostly from things ASF has to deal with, and not from internal structures used for cards farming.

Die ressourcenintensivsten Funktionen sind:

- Das Parsen der Abzeichen-Seite
- Das Parsen des Inventars

Which means that memory will spike the most when ASF is dealing with reading badge pages, and when it's dealing with its inventory (e.g. sending trade or working with STM). This is because ASF has to deal with really huge amount of data - the memory usage of your favourite browser launching those two pages will not be any lower than that. Sorry, that's how it works - decrease number of your badge pages, and keep number of your inventory items low, that can for sure help.

* * *

## Laufzeitoptimierung (Fortgeschritten)

Die folgenden Tricks **beeinträchtigen die Performance-Minderung** und sollten mit Vorsicht angewendet werden.

`ArchiSteamFarm.runtimeconfig.json` ermöglicht es dir, die ASF-Runtime einzustellen, insbesondere zwischen Server GC und Workstation GC zu wechseln.

> Der Garbage Collector ist selbstoptimierend und kann in einer Vielzahl von Szenarien eingesetzt werden. Du kannst mit Hilfe einer Einstellung in der Konfigurationsdatei die Art der Garbage Collection basierend auf den Eigenschaften der Systemlast festlegen. Die CLR bietet die folgenden Arten von Garbage Collection:
> 
> Workstation Garbage Collection, die für alle Client-Arbeitsplätze und Einzel-PCs gilt. Dies ist die Standardeinstellung für das <gcserver> Element im Runtime-Konfigurationsschema.
> 
> Server Garbage Collection, die für Serveranwendungen bestimmt ist, die einen hohen Datendurchsatz und Skalierbarkeit erfordern. Die Server Garbage Collection kann sowohl nicht zeitgleich als auch im Hintergrund erfolgen.

Weitere Informationen findest du unter **[Grundlagen der Garbage Collection](https://docs.microsoft.com/en-us/dotnet/standard/garbage-collection/fundamentals)**.

ASF is already using workstation GC, but you can ensure that it's truly the case by checking if `System.GC.Server` property of `ArchiSteamFarm.runtimeconfig.json` is set to `false`.

In addition to verifying that workstation GC is active, there are also interesting **[configuration knobs](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/clr-configuration-knobs.md)** that you can use - `gcTrimCommitOnLowMemory` and `GCLatencyLevel`.

### `GCLatencyLevel`

> Specifies the GC latency level that you want to optimize for.

This works exceptionally well by limiting size of GC generations and in result make GC purge them more frequently and more aggressively. Default (balanced) latency level is `1`, we'll want to use `0`, which will tune for memory usage.

### `gcTrimCommitOnLowMemory`

> When set we trim the committed space more aggressively for the ephemeral seg. This is used for running many instances of server processes where they want to keep as little memory committed as possible.

This offers little improvement, but might make GC even more aggressive when system will be low on memory.

* * *

You can enable both by setting appropriate `COMPlus_` environment variables. Als Beispiel unter Linux:

```shell
export COMPlus_GCLatencyLevel=0
export COMPlus_gcTrimCommitOnLowMemory=1
./ArchiSteamFarm
```

Oder unter Windows:

```bat
SET COMPlus_GCLatencyLevel=0
SET COMPlus_gcTrimCommitOnLowMemory=1
.\ArchiSteamFarm.exe
```

Especially `GCLatencyLevel` will come very useful as we verified that the runtime indeed optimizes code for memory and therefore drops average memory usage significantly, even with server GC. It's one of the best tricks that you can apply if you want to significantly lower ASF memory usage while not degrading performance too much with `OptimizationMode`.

* * *

## ASF tuning (intermediate)

Below tricks **involve serious performance degradation** and should be used with caution.

- As a last resort, you can tune ASF for `MinMemoryUsage` through `OptimizationMode` **[global config property](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#global-config)**. Read carefully its purpose, as this is serious performance degradation for nearly no memory benefits. This is typically **the last thing you want to do**, long after you go through **[runtime tuning](#runtime-tuning-advanced)** to ensure that you're forced to do this.

* * *

## Empfohlene Optimierung

- Start from simple ASF setup tricks, perhaps you're just using your ASF in a wrong way such as starting the process several times for all of your bots, or keeping all of them active if you need just one or two to autostart.
- If it's still not enough, enable all configuration knobs listed above by setting appropriate `COMPlus_` environment variables. Especially `GCLatencyLevel` offers significant runtime improvements for little cost on performance.
- If even that didn't help, as a last resort enable `MinMemoryUsage` `OptimizationMode`. This forces ASF to execute almost everything in synchronous matter, making it work much slower but also not relying on thread pool to balance things out when it comes to parallel execution.

It's physically impossible to decrease memory even further, your ASF is already heavily degraded in terms of performance and you depleted all your possibilities, both code-wise and runtime-wise. Consider adding some extra memory for ASF to use, even 128 MB would make a great difference.