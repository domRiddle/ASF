# 高性能設置

這與**[低記憶體設置](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup)**截然相反，如果你想進一步提高 ASF 性能（就CPU速度而言），可能會增加記憶體使用的潛在成本，請遵循這些提示。

* * *

當涉及到常規優化時，ASF 始終以性能為先，因此在進一步提高性能方面，您可做的事情並不多，儘管您並非別無選擇。 但是，請記住，預設情況下不會啟用這些選項，這意味著它們不足以在大多數用法中保證它們的平衡，因此您應該自己決定是否可以接受啟用它們帶來的記憶體增加。

* * *

## 運行時優化（進階）

以下技巧**涉及嚴重的記憶體增加**，應謹慎使用。

`ArchiSteamFarm.runtimeconfig.json` 允許您調整 ASF 運行時，特別是在伺服器 GC 和工作站 GC 之間切換。

> 垃圾收集器是自調整的，可以在各種情況下工作。 您可以使用設定檔根據工作負荷的特徵設置垃圾回收的類型。 CLR 提供以下類型的垃圾回收： ——工作站垃圾回收，適用于所有用戶端工作站和獨立 PC。 這是運行時配置架構中 `<gcServer>` 元素的默認設置。 ——伺服器垃圾回收，適用于需要高吞吐量和可伸縮性的伺服器應用程式。 伺服器垃圾回收可以是非並發方式的，也可以在後台運行。

可在**[垃圾收集概要](https://docs.microsoft.com/en-us/dotnet/standard/garbage-collection/fundamentals)**中了解更多。

ASF is using workstation garbage collection by default. This is mainly because of a good balance between memory usage and performance, which is more than enough for just a few bots, as usually a single concurrent background GC thread is fast enough to handle entire memory allocated by ASF.

However, today we have a lot of CPU cores that ASF can greatly benefit from, by having a dedicated GC thread per each CPU vCore that is available. This can greatly improve the performance during heavy ASF tasks such as parsing badge pages or the inventory, since every CPU vCore can help, as opposed to just 2 (main and GC). Server GC is recommended for machines with 3 CPU vCores and more, workstation GC is automatically forced if your machine has just 1 CPU vCore, and if you have exactly 2 then you can consider trying both (results may vary).

You can enable server GC by switching `System.GC.Server` property of `ArchiSteamFarm.runtimeconfig.json` from `false` to `true`. Keep in mind that you might need to do it more than once, as ASF will still use `false` by default after auto-update.

Server GC itself does not result in a very huge memory increase by just being active, but it has much bigger generation sizes, and therefore is far more lazy when it comes to giving memory back to OS. You might find yourself in a sweet spot where server GC increases performance significantly and you'd like to keep using it, but at the same time you can't afford that huge memory increase that comes out of using it. Luckily for you, there is a "best of both worlds" setting, by using server GC with **[GC latency level](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup#gclatencylevel)** set to `0`, which will still enable server GC, but limit generation sizes and focus more on memory.

However, if memory is not a problem for you (as GC still takes into account your available memory and tweaks itself), it's much better to not change `GCLatencyLevel` at all, achieving superior performance in result.

* * *

## Recommended optimization

- Ensure that you're using default value of `OptimizationMode` which is `MaxPerformance`. This is by far the most important setting, as using `MinMemoryUsage` value has dramatic effects on performance.
- Enable server GC by switching `System.GC.Server` property of `ArchiSteamFarm.runtimeconfig.json` from `false` to `true`. This will enable server GC which can be immediately seen as being active by memory increase compared to workstation GC.
- If you can't afford that much memory increase, consider using `GCLatencyLevel` of `0` to achieve "the best of both worlds". However, if your memory can afford it, then it's better to keep it at default - server GC already tweaks itself during runtime and is smart enough to use less memory when your OS will truly need it.

If you've enabled server GC and kept `GCLatencyLevel` at default setting, then you have superior ASF performance that should be blazing fast even with hundreds or thousands of enabled bots. CPU should not be a bottleneck anymore, as ASF is able to use your entire CPU power when needed, cutting required time to bare minimum. The next step would be CPU and RAM upgrades.