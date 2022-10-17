# 高效能設定

這與&#8203;**[低記憶體設定](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup-zh-TW)**&#8203;完全相反，若您想進一步提高ASF的效能（就CPU速度方面），請遵照下列指示。這可能會增加記憶體使用量。

---

ASF已經嘗試在一般的平衡性中考慮效能優先，因此您沒有很多提高效能的餘地，但您也不是完全沒有其他選擇。 但請注意，這些選項預設是未啟用，這代表它們不能在大多數情形下保證平衡性，因此，您應該自行決定是否能夠接受它們所造成的記憶體增加。

---

## 執行環境調整（進階）

以下技巧&#8203;**涉及嚴重的記憶體及啟動時間的增加**&#8203;，因此應謹慎使用。

套用這些設定的推薦方法，是設定&#8203;`DOTNET_`&#8203;環境屬性。 當然，您也可以使用其他方法，例如&#8203;`runtimeconfig.json`&#8203;，但有些設定無法如此設定。除此之外，ASF會在每次更新時，將您的自訂&#8203;`runtimeconfig.json`&#8203;取代成自己的檔案，因此，我們建議使用環境屬性，這樣您在啟動程序前就可以輕鬆設定。

.NET執行環境允許您以多種方法&#8203;**[調整垃圾回收](https://docs.microsoft.com/dotnet/core/run-time-config/garbage-collector)**&#8203;，依據您的需求高效微調垃圾回收（GC）程序。 我們記錄了下列我們認為特別重要的屬性。

### [`gcServer`](https://docs.microsoft.com/zh-tw/dotnet/core/run-time-config/garbage-collector#flavors-of-garbage-collection)

> 設定應用程式是使用工作站垃圾回收還是伺服器垃圾回收。

您可以在&#8203;**[記憶體回收的基本概念](https://learn.microsoft.com/zh-tw/dotnet/standard/garbage-collection/fundamentals)**&#8203;中閱讀伺服器GC的詳細資訊。

ASF預設使用工作站垃圾回收。 這主要是因為記憶體的使用與效能間的平衡良好，這對於執行少數Bot來說綽綽有餘，因為通常單個並行的背景GC執行緒，足以快速處理所有由ASF分配的記憶體。

However, today we have a lot of CPU cores that ASF can greatly benefit from, by having a dedicated GC thread per each CPU vCore that is available. This can greatly improve the performance during heavy ASF tasks such as parsing badge pages or the inventory, since every CPU vCore can help, as opposed to just 2 (main and GC). Server GC is recommended for machines with 3 CPU vCores and more, workstation GC is automatically forced if your machine has just 1 CPU vCore, and if you have exactly 2 then you can consider trying both (results may vary).

Server GC itself does not result in a very huge memory increase by just being active, but it has much bigger generation sizes, and therefore is far more lazy when it comes to giving memory back to OS. You may find yourself in a sweet spot where server GC increases performance significantly and you'd like to keep using it, but at the same time you can't afford that huge memory increase that comes out of using it. Luckily for you, there is a "best of both worlds" setting, by using server GC with **[`GCLatencyLevel`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup#gclatencylevel)** configuration property set to `0`, which will still enable server GC, but limit generation sizes and focus more on memory. Alternatively, you might also experiment with another property, **[`GCHeapHardLimitPercent`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup#gcheaphardlimitpercent)**, or even both of them at the same time.

However, if memory is not a problem for you (as GC still takes into account your available memory and tweaks itself), it's a much better idea to not change those properties at all, achieving superior performance in result.

### **[`DOTNET_TieredPGO`](https://docs.microsoft.com/zh-tw/dotnet/core/run-time-config/compilation#profile-guided-optimization)**

> This setting enables dynamic or tiered profile-guided optimization (PGO) in .NET 6 and later versions.

Disabled by default. In a nutshell, this will cause JIT to spend more time analyzing ASF's code and its patterns in order to generate superior code optimized for your typical usage. If you want to learn more about this setting, visit **[performance improvements in .NET 6](https://devblogs.microsoft.com/dotnet/performance-improvements-in-net-6)**.

### **[`DOTNET_ReadyToRun`](https://docs.microsoft.com/zh-tw/dotnet/core/run-time-config/compilation#readytorun)**

> Configures whether the .NET Core runtime uses pre-compiled code for images with available ReadyToRun data. Disabling this option forces the runtime to JIT-compile framework code.

預設啟用。 Disabling this in combination with enabling `DOTNET_TieredPGO` allows you to extend tiered profile-guided optimization to the whole .NET platform, and not just ASF code.

### **[`DOTNET_TC_QuickJitForLoops`](https://docs.microsoft.com/zh-tw/dotnet/core/run-time-config/compilation#quick-jit-for-loops)**

> Configures whether the JIT compiler uses quick JIT on methods that contain loops. Enabling quick JIT for loops may improve startup performance. However, long-running loops can get stuck in less-optimized code for long periods.

預設停用。 While the description doesn't make it obvious, enabling this will allow methods with loops to go through additional compilation tier, which will allow `DOTNET_TieredPGO` to do a better job by analyzing its usage data.

---

You can enable selected properties by setting appropriate environment variables. For example, on Linux (shell):

```shell
export DOTNET_gcServer=1

export DOTNET_TieredPGO=1
export DOTNET_ReadyToRun=0
export DOTNET_TC_QuickJitForLoops=1

./ArchiSteamFarm # 適用於您的作業系統的建置版本
```

或在 Windows 上（PowerShell）：

```powershell
$Env:DOTNET_gcServer=1

$Env:DOTNET_TieredPGO=1
$Env:DOTNET_ReadyToRun=0
$Env:DOTNET_TC_QuickJitForLoops=1

.\ArchiSteamFarm.exe # 適用於您的作業系統的建置版本
```

---

## 優化建議

- Ensure that you're using default value of `OptimizationMode` which is `MaxPerformance`. This is by far the most important setting, as using `MinMemoryUsage` value has dramatic effects on performance.
- 啟用伺服器 GC。 Server GC can be immediately seen as being active by significant memory increase compared to workstation GC. This will spawn a GC thread for every CPU thread your machine has in order to perform GC operations in parallel with maximum speed.
- If you can't afford memory increase due to server GC, consider tweaking **[`GCLatencyLevel`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup#gclatencylevel)** and/or **[`GCHeapHardLimitPercent`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup#gcheaphardlimitpercent)** to achieve "the best of both worlds". However, if your memory can afford it, then it's better to keep it at default - server GC already tweaks itself during runtime and is smart enough to use less memory when your OS will truly need it.
- You can also consider increased optimization for longer startup time with additional tweaking through other `DOTNET_` properties explained above.

Applying recommendations above allows you to have superior ASF performance that should be blazing fast even with hundreds or thousands of enabled bots. CPU should not be a bottleneck anymore, as ASF is able to use your entire CPU power when needed, cutting required time to bare minimum. The next step would be CPU and RAM upgrades.