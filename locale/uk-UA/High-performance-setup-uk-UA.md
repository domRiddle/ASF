# Налаштування високої продуктивності

This is exact opposite of **[low-memory setup](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup)** and typically you want to follow those tips if you want to further increase ASF performance (in terms of CPU speed), for potential cost of increased memory usage.

* * *

ASF already tries to prefer performance when it comes to general balanced tuning, therefore there is not a lot you can do to further increase its performance, although you're not completely out of options either. However, keep in mind that those options are not enabled by default, which means that they're not good enough to consider them balanced for majority of usages, therefore you should decide yourself if memory increase brought by them is acceptable for you.

* * *

## Runtime tuning (advanced)

Below tricks **involve serious memory increase** and should be used with caution.

`ArchiSteamFarm.runtimeconfig.json` allows you to tune ASF runtime, especially allowing you to switch between server GC and workstation GC.

> The garbage collector is self-tuning and can work in a wide variety of scenarios. You can use a configuration file setting to set the type of garbage collection based on the characteristics of the workload. The CLR provides the following types of garbage collection:
> 
> Workstation garbage collection, which is for all client workstations and stand-alone PCs. This is the default setting for the <gcserver> element in the runtime configuration schema.
> 
> Server garbage collection, which is intended for server applications that need high throughput and scalability. Server garbage collection can be non-concurrent or background.

More can be read at **[fundamentals of garbage collection](https://docs.microsoft.com/en-us/dotnet/standard/garbage-collection/fundamentals)**.

ASF is using workstation garbage collection by default. This is mainly because of a good balance between memory usage and performance, which is more than enough for just a few bots, as usually a single concurrent background GC thread is fast enough to handle entire memory allocated by ASF.

However, today we have a lot of CPU cores that ASF can greatly benefit from, by having a dedicated GC thread per each CPU vCore that is available. This can greatly improve the performance during heavy ASF tasks such as parsing badge pages or the inventory, since every CPU vCore can help, as opposed to just 2 (main and GC). Server GC is recommended for machines with 3 CPU vCores and more, workstation GC is automatically forced if your machine has just 1 CPU vCore, and if you have exactly 2 then you can consider trying both (results may vary).

You can enable server GC by switching `System.GC.Server` property of `ArchiSteamFarm.runtimeconfig.json` from `false` to `true`. Keep in mind that you might need to do it more than once, as ASF will still use `false` by default after auto-update.

Server GC itself does not result in a very huge memory increase by just being active, but it has much bigger generation sizes, and therefore is far more lazy when it comes to giving memory back to OS. You might find yourself in a sweet spot where server GC increases performance significantly and you'd like to keep using it, but at the same time you can't afford that huge memory increase that comes out of using it. Luckily for you, there is a "best of both worlds" setting, by using server GC with **[GC latency level](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup#gclatencylevel)** set to `0`, which will still enable server GC, but limit generation sizes and focus more on memory.

However, if memory is not a problem for you (as GC still takes into account your available memory and tweaks itself), it's much better to not change `GCLatencyLevel` at all, achieving superior performance in result.

* * *

## Recommended optimization

- Enable server GC by switching `System.GC.Server` property of `ArchiSteamFarm.runtimeconfig.json` from `false` to `true`. This will enable server GC which can be immediately seen as being active by memory increase compared to workstation GC.
- If you can't afford that much memory increase, consider using `GCLatencyLevel` of `0` to achieve "the best of both worlds". However, if your memory can afford it, then it's better to keep it at default - server GC already tweaks itself during runtime and is smart enough to use less memory when your OS will truly need it.

If you've enabled server GC and kept `GCLatencyLevel` at default setting, then you have superior ASF performance that should be blazing fast even with hundreds or thousands of enabled bots. CPU should not be a bottleneck anymore, as ASF is able to use your entire CPU power when needed, cutting required time to bare minimum.