# 低記憶體設定

這與&#8203;**[高效能設定](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/High-performance-setup-zh-TW)**&#8203;完全相反，若您想減少ASF的記憶體使用量，請遵照下列指示。這可能會降低效能。

---

依據定義，ASF在資源上屬於極輕量級。取決於您的使用情形，即使是128 MB的Linux VPS也可以執行它，儘管不建議使用那麼低的資源配置，因為這可能會導致各種問題。 在輕量化的同時，如果ASF需要更多記憶體才能以最佳速度執行，它也不會吝嗇於向作業系統請求更多的記憶體空間。

作為一個應用程式，ASF試圖盡可能最佳化與高效能，這也考慮了執行期間所使用的資源。 在記憶體方面，ASF更看重效能而不是記憶體使用量，這可能會導致記憶體臨時「尖峰」，例如，您可以注意到，當ASF從3頁或以上徽章頁面的帳號中提取並剖析第一頁，從中讀取總頁數，然後為每個額外頁面啟動提取工作，這會導致並行提取及剖析剩餘的頁面。 這種「額外」的記憶體使用量（與運算的最低限度相比）可以顯著增加執行速度與整體效能，以增加記憶體使用量為代價，平行執行所有工作。 類似的事情也發生於所有可以平行執行的一般ASF工作上，例如剖析交易提案，ASF可以一次剖析所有工作，因為它們都是相互獨立的。 最重要的是，ASF（C#執行環境）&#8203;**不會**&#8203;在之後立刻將未使用到的記憶體還給作業系統，您可以很快注意到ASF程序只會佔用越來越多的記憶體，但（幾乎）永遠不會將那些記憶體還給作業系統。 一些人可能已覺得這樣有問題，甚至可能懷疑產生了記憶體洩漏，但不用擔心，一切都在預料之中。

ASF經過非常好的最佳化，並會盡可能利用可以使用的資源。 ASF的高記憶體使用量不代表ASF主動&#8203;**使用**&#8203;這些記憶體，或&#8203;**需要它們**&#8203;。 通常ASF會保留分配的記憶體來作為未來行動的「空間」，因為如果我們在每次使用記憶體區塊時，都不需要向作業系統發出詢問，就可以大大地提高效能。 執行環境會在作業系統&#8203;**真正**&#8203;需要記憶體時，將ASF未使用的記憶體自動釋放回作業系統。 **[不使用的記憶體，就是被浪費掉的記憶體](https://www.howtogeek.com/128130/htg-explains-why-its-good-that-your-computers-ram-is-full)**&#8203;。 當您&#8203;**需要**&#8203;的記憶體大於可用的記憶體時，您可能會遇到問題，但這並不是因為ASF保留了一些額外分配的記憶體以加速稍後執行的功能。 當您的Linux核心由於OOM（記憶體不足）而結束ASF程序時才會遇到問題，而不是您在&#8203;`htop`&#8203;看到ASF程序是記憶體使用量大戶時。

ASF使用的&#8203;**[垃圾回收](https://zh.wikipedia.org/zh-tw/垃圾回收_(計算機科學))**&#8203;程序是種非常複雜的機制，它足夠智慧，不只可以考慮ASF自身，也可以考慮到作業系統及其他程序。 當您擁有大量的空閒記憶體時，ASF將會要求任何能夠提高效能的資源。 這甚至能夠達到1GB（使用伺服器GC時）。 在您作業系統的記憶體接近用滿時ASF將會自動將其部分記憶體釋放回作業系統，以助其穩定，此時ASF的記憶體使用量可以低至50MB。 50MB與1GB間的差異巨大，但在小型的512 MB VPS與32 GB的大型專用伺服器間的差異也是如此。 若ASF能保證這些記憶體能夠發揮作用，且同時沒有其他程序需要它們，ASF會更願意保留這些記憶體，並依據過去執行的常式進行自我最佳化。 ASF使用的GC是自調諧的，程序執行時間越長，效果就越好。

This is also why ASF process memory varies from setup to setup, as ASF will do its best to use available resources in **as efficient way as possible**, and not in a fixed way like it was done during Windows XP times. The actual (real) memory usage that ASF is using can be verified with `stats` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**, and is usually around 4 MB for just a few bots, up to 30 MB if you use stuff like **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** and other advanced features. Keep in mind that memory returned by `stats` command also includes free memory that hasn't been reclaimed by garbage collector yet. Everything else is shared runtime memory (around 40-50 MB) and room for execution (vary). This is also why the same ASF can use as little as 50 MB in low-memory VPS environment, while using even up to 1 GB on your desktop. ASF is actively adapting to your environment and will try to find optimal balance in order to neither put your OS under pressure, nor limit its own performance when you have a lot of unused memory that could be put in use.

---

Of course, there are a lot of ways how you can help point ASF at the right direction in terms of the memory you expect to use. In general if you don't need to do it, it's best to let garbage collector work in peace and do whatever it considers is best. But this is not always possible, for example if your Linux server is also hosting several websites, MySQL database and PHP workers, then you can't really afford ASF shrinking itself when you run close to OOM, as it's usually too late and performance degradation comes sooner. This is usually when you could be interested in further tuning, and therefore reading this page.

Below suggestions are divided into a few categories, with varied difficulty.

---

## ASF 設定（簡單）

Below tricks **do not affect performance negatively** and can be safely applied to all setups.

- Never run more than one ASF instance. ASF is meant to handle unlimited number of bots all at once, and unless you're binding every ASF instance to different interface/IP address, you should have exactly **one** ASF process, with multiple bots (if needed).
- Make use of `ShutdownOnFarmingFinished`. Active bot takes more resources than deactivated one. It's not a significant save, as the state of bot still needs to be kept, but you're saving some amount of resources, especially all resources related to networking, such as TCP sockets. You can always bring up other bots if needed.
- Keep your bots number low. Not `Enabled` bot instance takes less resources, as ASF doesn't bother starting it. Also keep in mind that ASF has to create a bot for each of your configs, therefore if you don't need to `start` given bot and you want to save some extra memory, you can temporarily rename `Bot.json` to e.g. `Bot.json.bak` in order to avoid creating state for your disabled bot instance in ASF. This way you won't be able to `start` it without renaming it back, but ASF also won't bother keeping state of this bot in memory, leaving room for other things (very small save, in 99.9% cases you shouldn't bother with it, just keep your bots with `Enabled` of `false`).
- Fine-tune your configs. Especially global ASF config has many variables to adjust, for example by increasing `LoginLimiterDelay` you'll bring up your bots slower, which will allow already started instance to fetch badges in the meantime, as opposed to bringing up your bots faster, which will take more resources as more bots will do major work (such as parsing badges) at the same time. The less work that has to be done at the same time - the less memory used.

Those are a few things you can keep in mind when dealing with memory usage. However, those things don't have any "crucial" matter on memory usage, because memory usage comes mostly from things ASF has to deal with, and not from internal structures used for cards farming.

The most resources-heavy functions are:
- Badge page parsing
- Inventory parsing

Which means that memory will spike the most when ASF is dealing with reading badge pages, and when it's dealing with its inventory (e.g. sending trade or working with STM). This is because ASF has to deal with really huge amount of data - the memory usage of your favourite browser launching those two pages will not be any lower than that. Sorry, that's how it works - decrease number of your badge pages, and keep number of your inventory items low, that can for sure help.

---

## 執行環境調整（進階）

Below tricks **involve performance degradation** and should be used with caution.

The recommended way of applying those settings is through `DOTNET_` environment properties. Of course, you could also use other methods, e.g. `runtimeconfig.json`, but some settings are impossible to be set this way, and on top of that ASF will replace your custom `runtimeconfig.json` with its own on the next update, therefore we recommend environment properties that you can set easily prior to launching the process.

.NET runtime allows you to **[tweak garbage collector](https://docs.microsoft.com/dotnet/core/run-time-config/garbage-collector)** in a lot of ways, effectively fine-tuning the GC process according to your needs. We've documented below properties that are especially important in our opinion.

### [`GCHeapHardLimitPercent`](https://docs.microsoft.com/zh-tw/dotnet/core/run-time-config/garbage-collector#heap-limit-percent)

> Specifies the allowable GC heap usage as a percentage of the total physical memory.

The "hard" memory limit for ASF process, this setting tunes GC to use only a subset of total memory and not all of it. It may become especially useful in various server-like situations where you can dedicate a fixed percentage of your server's memory for ASF, but never more than that. Be advised that limiting memory for ASF to use will not magically make all of those required memory allocations go away, therefore setting this value too low might result in running into out of memory scenarios, where ASF process will be forced to terminate.

On the other hand, setting this value high enough is a perfect way to ensure that ASF will never use more memory than you can realistically afford, giving your machine some breathing room even under heavy load, while still allowing the program to do its job as efficiently as possible.

### [`GCHighMemPercent`](https://docs.microsoft.com/zh-tw/dotnet/core/run-time-config/garbage-collector#high-memory-percent)

> Specifies the amount of memory used after which GC becomes more aggressive.

This setting configures the memory treshold of your whole OS, which once passed, causes GC to become more aggressive and attempt to help the OS lower the memory load by running more intensive GC process and in result releasing more free memory back to the OS. It's a good idea to set this property to maximum amount of memory (as percentage) which you consider "critical" for your whole OS performance. Default is 90%, and usually you want to keep it in 80-97% range, as too low value will cause unnecessary aggression from the GC and performance degradation for no reason, while too high value will put unnecessary load on your OS, considering ASF could release some of its memory to help.

### **[`GCLatencyLevel`](https://github.com/dotnet/runtime/blob/4b90e803262cb5a045205d946d800f9b55f88571/src/coreclr/gc/gcpriv.h#L375-L398)**

> Specifies the GC latency level that you want to optimize for.

This is undocumented property that turned out to work exceptionally well for ASF, by limiting size of GC generations and in result make GC purge them more frequently and more aggressively. Default (balanced) latency level is `1`, but you can use `0`, which will tune for memory usage.

### [`gcTrimCommitOnLowMemory`](https://docs.microsoft.com/zh-tw/dotnet/standard/garbage-collection/optimization-for-shared-web-hosting)

> When set we trim the committed space more aggressively for the ephemeral seg. This is used for running many instances of server processes where they want to keep as little memory committed as possible.

This offers little improvement, but may make GC even more aggressive when system will be low on memory, especially for ASF which makes use of threadpool tasks heavily.

---

You can enable selected properties by setting appropriate environment variables. For example, on Linux (shell):

```shell
# 若您打算使用它們，別忘了調整一下
export DOTNET_GCHeapHardLimitPercent=0x4B # 75% as hex
export DOTNET_GCHighMemPercent=0x50 # 80% as hex

export DOTNET_GCLatencyLevel=0
export DOTNET_gcTrimCommitOnLowMemory=1

./ArchiSteamFarm # 適用於您的作業系統的建置版本
```

或在 Windows 上（PowerShell）：

```powershell
# 若您打算使用它們，別忘了調整一下
$Env:DOTNET_GCHeapHardLimitPercent=0x4B # 75% as hex
$Env:DOTNET_GCHighMemPercent=0x50 # 80% as hex

$Env:DOTNET_GCLatencyLevel=0
$Env:DOTNET_gcTrimCommitOnLowMemory=1

.\ArchiSteamFarm.exe # 適用於您的作業系統的建置版本
```

Especially `GCLatencyLevel` will come very useful as we verified that the runtime indeed optimizes code for memory and therefore drops average memory usage significantly, even with server GC. It's one of the best tricks that you can apply if you want to significantly lower ASF memory usage while not degrading performance too much with `OptimizationMode`.

---

## ASF 調整（中等）

Below tricks **involve serious performance degradation** and should be used with caution.

- As a last resort, you can tune ASF for `MinMemoryUsage` through `OptimizationMode` **[global config property](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#global-config)**. Read carefully its purpose, as this is serious performance degradation for nearly no memory benefits. This is typically **the last thing you want to do**, long after you go through **[runtime tuning](#runtime-tuning-advanced)** to ensure that you're forced to do this. Unless absolutely critical for your setup, we discourage using `MinMemoryUsage`, even in memory-constrained environments.

---

## 最佳化建議

- Start from simple ASF setup tricks, perhaps you're just using your ASF in a wrong way such as starting the process several times for all of your bots, or keeping all of them active if you need just one or two to autostart.
- If it's still not enough, enable all configuration properties listed above by setting appropriate `DOTNET_` environment variables. Especially `GCLatencyLevel` offers significant runtime improvements for little cost on performance.
- If even that didn't help, as a last resort enable `MinMemoryUsage` `OptimizationMode`. This forces ASF to execute almost everything in synchronous matter, making it work much slower but also not relying on thread pool to balance things out when it comes to parallel execution.

It's physically impossible to decrease memory even further, your ASF is already heavily degraded in terms of performance and you depleted all your possibilities, both code-wise and runtime-wise. Consider adding some extra memory for ASF to use, even 128 MB would make a great difference.