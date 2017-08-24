# High-performance setup

This is exact opposite of **[low-memory setup](https://github.com/JustArchi/ArchiSteamFarm/wiki/Low-memory-setup)** and typically you want to follow those tips if ASF is not performing fast enough (in terms of CPU usage).

***

ASF already tries to prefer performance when it comes to general balanced tuning, therefore there is not a lot you can do to further increase its performance, although you're not completely out of options either.

***

## Runtime tuning (advanced)

Below tricks **involve serious memory increase** and should be used with caution.

`ArchiSteamFarm.runtimeconfig.json` allows you to tune ASF runtime, especially allowing you to switch between server GC and workstation GC.

> The garbage collector is self-tuning and can work in a wide variety of scenarios. You can use a configuration file setting to set the type of garbage collection based on the characteristics of the workload. The CLR provides the following types of garbage collection:
>
> Workstation garbage collection, which is for all client workstations and stand-alone PCs. This is the default setting for the <gcServer> element in the runtime configuration schema.
>
> Server garbage collection, which is intended for server applications that need high throughput and scalability. Server garbage collection can be non-concurrent or background.

More can be read at **[fundamentals of garbage collection](https://docs.microsoft.com/en-us/dotnet/standard/garbage-collection/fundamentals)**.

ASF is using workstation garbage collection by default. This is mainly because of a good balance between memory usage and performance, which is good enough for just a few bots, as usually a single concurrent background GC thread is enough to handle memory allocated by ASF.

However, today we have a lot of CPU cores that ASF can greatly benefit from, by having a dedicated GC thread per each CPU core that is available. This can greatly improve the performance during heavy ASF tasks such as parsing badge pages or the inventory. Server GC is recommended for machines with 3 cores and more, workstation GC is automatically forced if your machine has just 1 core, and if you have exactly 2 then you can consider trying both.

You can enable server GC by switching `System.GC.Server` property of `ArchiSteamFarm.runtimeconfig.json` from `false` to `true`.

Server GC itself does not result in a very huge memory increase by just being active, but it is far more lazy when it comes to giving memory back to OS, that's why usually just setting `BackgroundGCPeriod` to `1` or `2` should be enough in order to still keep awesome performance that comes from server GC, while forcing it to give back more unused memory to OS in fixed intervals. This is "the best of both worlds" if you want to benefit from server GC performance, but at the same time can't afford that huge memory increase. If memory is not a problem for you (as GC still takes into account available memory and tweaks itself), it's much better to not enable `BackgroundGCPeriod` at all.

***

## Recommended optimization

- Enable server GC by switching `System.GC.Server` property of `ArchiSteamFarm.runtimeconfig.json` from `false` to `true`. This will enable server GC which can be immediately seen as being active by memory increase compared to workstation GC.
- If you can't afford that much memory increase, considering using `BackgroundGCPeriod` to use "the best of both worlds", although it's much better if you don't need to do that. Server GC is smart enough to use less memory when your OS will truly need it.