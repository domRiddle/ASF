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

但是如至今日我們通常擁有很多CPU核心，使ASF也因此受益，因為在每個CPU vCore中都能有一個專用的GC執行緒。 這可以大大地提高ASF執行繁重工作時的效能，例如剖析徽章頁面或物品庫，因為每個CPU vCore都可以提供協助，而不只是2個（主執行緒及GC）。 對於具有3個或更多CPU vCore的設備，建議使用伺服器GC；若您的設備只有1個CPU vCore，則會自動強制使用工作站GC；若您正好有2個，您可以考慮嘗試使用兩者（結果可能會不同）。

伺服器GC本身不會因為只處於活動狀態而導致巨量的記憶體消耗，但它具有更大的生成大小，因此更不會把記憶體還給作業系統。 您可能會發現自己處於一個尷尬情形，伺服器GC能顯著提高效能，使您希望繼續使用它，但同時您無法承受使用它帶來的巨量記憶體消耗。 幸運的是，將&#8203;**[`GCLatencyLevel`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup-zh-TW#gclatencylevel)**&#8203;組態設定設定成&#8203;`0`&#8203;，是個「兩全其美」的方法，它仍會啟用伺服器GC，但會限制生成大小並更關照記憶體的消耗。 或者您也許可以嘗試另外一個屬性，&#8203;**[`GCHeapHardLimitPercent`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup-zh-TW#gcheaphardlimitpercent)**&#8203;，或同時啟用這兩個選項。

但是，如果記憶體對您來說不是問題（因為GC仍會依據您可用的記憶體來自行調整），那麼最好不要更改這些屬性，以達到最高效能。

### **[`DOTNET_TieredPGO`](https://docs.microsoft.com/zh-tw/dotnet/core/run-time-config/compilation#profile-guided-optimization)**

> 這個設定在.NET 6及更高版本中啟用動態或分層特性指引最佳化（PGO）。

預設為停用。 簡而言之，這會使JIT使用更多時間來分析ASF的代碼及其模式，以便為您的典型使用方式來生成最佳化的上級程式碼。 若您想了解更多關於此設定的資訊，請造訪&#8203;**[Performance Improvements in .NET 6](https://devblogs.microsoft.com/dotnet/performance-improvements-in-net-6)**&#8203;。

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

## 最佳化建議

- Ensure that you're using default value of `OptimizationMode` which is `MaxPerformance`. This is by far the most important setting, as using `MinMemoryUsage` value has dramatic effects on performance.
- 啟用伺服器 GC。 Server GC can be immediately seen as being active by significant memory increase compared to workstation GC. This will spawn a GC thread for every CPU thread your machine has in order to perform GC operations in parallel with maximum speed.
- If you can't afford memory increase due to server GC, consider tweaking **[`GCLatencyLevel`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup#gclatencylevel)** and/or **[`GCHeapHardLimitPercent`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup#gcheaphardlimitpercent)** to achieve "the best of both worlds". However, if your memory can afford it, then it's better to keep it at default - server GC already tweaks itself during runtime and is smart enough to use less memory when your OS will truly need it.
- You can also consider increased optimization for longer startup time with additional tweaking through other `DOTNET_` properties explained above.

Applying recommendations above allows you to have superior ASF performance that should be blazing fast even with hundreds or thousands of enabled bots. CPU should not be a bottleneck anymore, as ASF is able to use your entire CPU power when needed, cutting required time to bare minimum. The next step would be CPU and RAM upgrades.