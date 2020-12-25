# Configurare de înaltă performanță

Acest lucru este exact opusul **[setării cu memorie mică](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup)** și de obicei doriți să urmați aceste sfaturi dacă doriți să creșteți în continuare performanța ASF (în termeni de viteză CPU), cu potențialul cost de utilizare crescută a memoriei.

* * *

ASF încearcă deja să prefere performanța în ceea ce privește reglarea generală echilibrată; prin urmare nu puteţi face multe pentru a-i creşte performanţa, deși aveţi anumite opţiuni care pot fi configurate. Cu toate acestea, aveți în vedere faptul că aceste opțiuni nu sunt activate în mod implicit, ceea ce înseamnă că nu sunt suficient de bune pentru a le considera echilibrate pentru majoritatea utilizărilor; de aceea trebuie să vă decideţi dacă creşterea memoriei indusă de acestea este acceptabilă pentru dumneavoastră.

* * *

## Reglaj Runtime (avansat)

Trucurile de mai jos **implică o creștere gravă a memoriei** și trebuie folosite cu precauție.

.NET Core runtime vă permite **[să modificati colectorul de gunoi](https://docs.microsoft.com/dotnet/core/run-time-config/garbage-collector)** in multe feluri, prin ajustarea eficientă a procesului GC în funcție de nevoile dumneavoastră.

Modul recomandat de aplicare al acestor setări este prin modificarea proprietății de mediu `COMPlus_`. Bineînțeles, ați putea folosi și alte metode, de ex. `runtimeconfig.json`, dar unele setări sunt imposibil de stabilit în acest fel, și pe deasupra ASF va înlocui fișierul personalizat `runtimeconfig.json` cu cel propriu la următoarea actualizare, de aceea recomandăm proprietățile de mediu pe care le puteți seta cu ușurință înainte de a lansa procesul.

Consultați documentația pentru toate proprietățile pe care le puteți folosi, le vom menționa pe cele mai importante (în opinia noastră) mai jos:

### [`gcServer`](https://docs.microsoft.com/dotnet/core/run-time-config/garbage-collector#flavors-of-garbage-collection)

> Configurează dacă aplicaţia se foloseşte de colectarea gunoiului de la staţia de lucru sau de colectarea gunoiului de pe server.

Puteti citi specificul serverului GC la **[fundamentale pentru colectarea de gunoi](https://docs.microsoft.com/dotnet/standard/garbage-collection/fundamentals)**.

ASF is using workstation garbage collection by default. This is mainly because of a good balance between memory usage and performance, which is more than enough for just a few bots, as usually a single concurrent background GC thread is fast enough to handle entire memory allocated by ASF.

However, today we have a lot of CPU cores that ASF can greatly benefit from, by having a dedicated GC thread per each CPU vCore that is available. This can greatly improve the performance during heavy ASF tasks such as parsing badge pages or the inventory, since every CPU vCore can help, as opposed to just 2 (main and GC). Server GC is recommended for machines with 3 CPU vCores and more, workstation GC is automatically forced if your machine has just 1 CPU vCore, and if you have exactly 2 then you can consider trying both (results may vary).

Server GC itself does not result in a very huge memory increase by just being active, but it has much bigger generation sizes, and therefore is far more lazy when it comes to giving memory back to OS. You may find yourself in a sweet spot where server GC increases performance significantly and you'd like to keep using it, but at the same time you can't afford that huge memory increase that comes out of using it. Luckily for you, there is a "best of both worlds" setting, by using server GC with **[`GCLatencyLevel`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup#gclatencylevel)** configuration property set to `0`, which will still enable server GC, but limit generation sizes and focus more on memory. Alternatively, you might also experiment with another property, **[`GCHeapHardLimitPercent`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup#gcheaphardlimitpercent)**, or even both of them at the same time.

However, if memory is not a problem for you (as GC still takes into account your available memory and tweaks itself), it's a much better idea to not change those properties at all, achieving superior performance in result.

* * *

You can enable all GC properties by setting appropriate `COMPlus_` environment variables. For example, on Linux (shell):

```shell
export COMPlus_gcServer=1

./ArchiSteamFarm # For OS-specific build
```

Or on Windows (powershell):

```powershell
$Env:COMPlus_gcServer=1

.\ArchiSteamFarm.exe # For OS-specific build
```

* * *

## Recommended optimization

- Ensure that you're using default value of `OptimizationMode` which is `MaxPerformance`. This is by far the most important setting, as using `MinMemoryUsage` value has dramatic effects on performance.
- Enable server GC. Server GC can be immediately seen as being active by significant memory increase compared to workstation GC.
- If you can't afford that much memory increase, considering tweaking **[`GCLatencyLevel`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup#gclatencylevel)** and/or **[`GCHeapHardLimitPercent`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup#gcheaphardlimitpercent)** to achieve "the best of both worlds". However, if your memory can afford it, then it's better to keep it at default - server GC already tweaks itself during runtime and is smart enough to use less memory when your OS will truly need it.

If you've enabled server GC and kept other configuration properties at their default values, then you have superior ASF performance that should be blazing fast even with hundreds or thousands of enabled bots. CPU should not be a bottleneck anymore, as ASF is able to use your entire CPU power when needed, cutting required time to bare minimum. The next step would be CPU and RAM upgrades.