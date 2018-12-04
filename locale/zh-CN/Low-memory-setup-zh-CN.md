# 低内存方案

这篇文档与&#8203;**[高性能方案](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/High-performance-setup-zh-CN)**&#8203;完全相反，如果您愿意牺牲一些性能换取较小的内存用量，请阅读以下内容。

* * *

ASF 在资源上是非常轻量级的，取决于您的使用情况，即使 128 MB 的 Linux VPS 也应该足以运行它，但我们并不建议使用这么差的设备，低配可能会导致各种问题。 在轻量的同时，如果 ASF 需要更多内存以保证运行速度，它并不吝啬于向操作系统申请这些内存。

ASF as an application tries to be as much optimized and efficient as possible, which also takes in mind resources being used during execution. When it comes to memory, ASF prefers performance over memory consumption, which can result in temporary memory "spikes", that can be noticed e.g. with accounts having 3+ badge pages, as ASF will fetch and parse first page, read from it total number of pages, then launch fetch task for every extra page, which results in concurrent fetching and parsing of remaining pages. That "extra" memory usage (compared to bare minimum for operation) can dramatically speed up execution and overall performance, for the cost of increased memory usage that is needed to do all of those things in parallel. Similar thing is happening to all other general ASF tasks that can be run in parallel, e.g. with parsing active trade offers, ASF can parse all of them at once, as they're all independent of each other. On top of that, ASF (C# runtime) will **not** return unused memory back to OS immediately afterwards, which you can quickly notice in form of ASF process only taking more and more memory, but never giving that memory back to the OS. Some people might already find it questionable, maybe even suspect a memory leak, but don't worry, all of this is to be expected.

ASF is extremely well optimized, and makes use of available resources as much as possible. High memory usage of ASF doesn't mean that ASF actively **uses** that memory and **needs it**. Very often ASF will keep allocated memory as "room" for future actions, because we can drastically improve performance if we don't need to ask OS for every memory chunk that we're about to use. The runtime should automatically release unused ASF memory back to OS when OS will **truly** need it. **[空闲内存是被浪费的资源](https://www.howtogeek.com/128130/htg-explains-why-its-good-that-your-computers-ram-is-full)**。 You run into issues when the memory you **need** is higher than the memory that is available for you, not when ASF keeps some extra allocated with purpose of speeding up functions that will execute in a moment. You run into problems when your Linux kernel is killing ASF process due to OOM (out of memory), not when you see ASF process as top memory consumer in `htop`.

Garbage collector being used in ASF is a very complex mechanism, smart enough to take into account not only ASF itself, but also your OS and other processes. When you have a lot of free memory, ASF will ask for whatever is needed to improve the performance. This can be even as much as 1 GB (with server GC). When your OS memory is close to being full, ASF will automatically release some of it back to the OS to help things settle down, which can result in overall ASF memory usage as low as 50 MB. The difference between 50 MB and 1 GB is huge, but so is the difference between small 512 MB VPS and huge dedicated server with 32 GB. If ASF can guarantee that this memory will come useful, and at the same time nothing else requires it right now, it'll prefer to keep it and automatically optimize itself based on routines that were executed in the past. The GC used in ASF is self-tuning and will achieve better results the longer the process is running.

This is also why ASF process memory varies from setup to setup, as ASF will do its best to use available resources in **as efficient way as possible**, and not in a fixed way like it was done during Windows XP times. ASF 实际的内存用量可以通过 `stats` **[命令](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-CN)**&#8203;查看。如果您机器人的数量很少，通常它只会占用大约 4 MB 内存，但如果启用了 **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-zh-CN)** 和其他额外功能，ASF 将会占用多达 30 MB 内存。 Keep in mind that memory returned by `stats` command also includes free memory that hasn't been reclaimed by garbage collector yet. Everything else is shared runtime memory (around 40-50 MB) and room for execution (vary). This is also why the same ASF can use as little as 50 MB in low-memory VPS environment, while using even up to 1 GB on your desktop. ASF is actively adapting to your environment and will try to find optimal balance in order to neither put your OS under pressure, nor limit its own performance when you have a lot of unused memory that could be put in use.

* * *

Of course, there are a lot of ways how you can help point ASF at the right direction in terms of the memory you expect to use. In general if you don't need to do it, it's best to let garbage collector work in peace and do whatever it considers is best. But this is not always possible, for example if your Linux server is also hosting several websites, MySQL database and PHP workers, then you can't really afford ASF shrinking itself when you run close to OOM, as it's usually too late and performance degradation comes sooner. This is usually when you might be interested in further tuning, and therefore reading this page.

下面这些建议被分为了不同的类别，其难度各不相同。

* * *

## ASF 设置（简单）

以下技巧**不会对性能造成负面影响**，可以在所有情况下安全选用。

- 永远不要运行多个 ASF 实例。 ASF 可以同时处理无限个机器人，除非您需要将每个 ASF 实例绑定到不同的网络接口或 IP 地址，否则您只需要**一个**有多个机器人的 ASF 进程。
- 善用 `ShutdownOnFarmingFinished`。 启用的机器人比未启用的消耗更多资源。 尽管效果不明显，因为仍然需要保留机器人的状态，但这仍然可以节约一些资源，尤其是 TCP 套接字等网络相关的资源。 要保持 ASF 实例的运行，只需要启用一个机器人，并且您可以随时在需要时激活其他机器人。
- 不要有太多机器人。 未 `Enabled`（启用）的机器人实例消耗较少的资源，因为 ASF 不需要启动它。 还需要注意，ASF 会为每份配置文件创建一个机器人，因此如果您不需要 `start`（运行）指定的机器人，并且希望节省一些内存，您可以临时将 `Bot.json` 重命名为 `Bot.json.bak` 等，以防止 ASF 创建被禁用的机器人。 如果您不将其重命名为原名，就无法 `start`（运行）这个机器人，但 ASF 也不会在内存中为这个机器人保存状态，为其他数据留出空间（非常小的空间，在 99.9% 的情况下您不需要这么做，将机器人的 `Enabled` 设置为 `false` 已经足够）。
- 妥善优化配置文件。特别是全局 ASF 配置中有很多变量可以调整，例如增加 `LoginLimiterDelay` 会减慢机器人启动的速度，留出时间给已启用的实例抓取徽章页面，如果减少这个值，就会让机器人尽快启动，当机器人很多时，它们就会同时进行解析徽章等消耗资源的任务。 同时进行的任务越少——使用的内存就越少。

这些都是您在处理内存占用问题时可以考虑的一些事情。 然而，这些事情不是影响内存的关键问题，因为内存占用主要来自于 ASF 必须处理的事情，而不是来自于挂卡使用的内部结构。

最消耗资源的功能是：

- 徽章页面解析
- 库存解析

这意味着，当 ASF 读取徽章页面以及处理库存时（例如发送交易报价或者进行 STM 相关的操作），内存用量将会最大。 这是因为此时 ASF 必须处理大量的数据——您直接用浏览器访问这两个页面消耗的内存也不会比 ASF 更低。 很抱歉，但这就是它的工作原理——减少您的徽章页面数，并且只在库存内保留少量物品，都会对此有帮助。

* * *

## 运行时环境调优（高级）

以下技巧**会造成性能下降**，应谨慎使用。

`ArchiSteamFarm.runtimeconfig.json` 允许您调整 ASF 运行时环境，尤其是允许您在服务器 GC 和工作站 GC 之间切换。

> 垃圾回收器可自行优化并且适用于多种方案。 您可使用配置文件来基于工作负荷的特征设置垃圾回收的类型。 CLR 提供了以下类型的垃圾回收： - 工作站垃圾回收，用于所有客户端工作站和独立 PC。 这是运行时配置架构中 `<gcServer>` 元素的默认设置。 - 服务器垃圾回收，用于需要高吞吐量和可伸缩性的服务器应用程序。 服务器垃圾回收可以是非并发或者是后台的。

您可以在&#8203;**[垃圾回收基础](https://docs.microsoft.com/en-us/dotnet/standard/garbage-collection/fundamentals)**&#8203;阅读更多。

ASF 已经使用工作站 GC，您可以检查 `ArchiSteamFarm.runtimeconfig.json` 中的 `System.GC.Server` 属性是否被设置为 `false` 来确认这一点。

为了进一步确认已启用工作站 GC，您可以使用两个有趣的&#8203;**[配置选项](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/clr-configuration-knobs.md)**——`gcTrimCommitOnLowMemory` 和 `GCLatencyLevel`。

### `GCLatencyLevel`

> 指定您要优化的 GC 延迟级别。

限制 GC 代数的大小非常有效，使 GC 发生得更频繁。 默认的（平衡）延迟级别为 `1`，而我们希望它为 `0`，这将会调整内存用量。

### `gcTrimCommitOnLowMemory`

> 设置此选项时，我们会更积极地为暂时段减少提交的空间。 这可用于运行多个服务器进程的实例，这些实例需要尽可能地保持较小的内存提交。

这个选项带来的改进很小，但是当系统内存不足时，它可能会使 GC 更加激进。

* * *

您可以通过 `COMPlus_` 环境变量设置它们。 例如，在 Linux 上：

```shell
export COMPlus_GCLatencyLevel=0
export COMPlus_gcTrimCommitOnLowMemory=1
./ArchiSteamFarm
```

或者在 Windows 上：

```bat
SET COMPlus_GCLatencyLevel=0
SET COMPlus_gcTrimCommitOnLowMemory=1
.\ArchiSteamFarm.exe
```

其中 `GCLatencyLevel` 将非常有用，因为我们可以验证运行时环境确实为内存优化了代码，因此即使采用服务器 GC 也会显著降低平均内存使用量。 如果您希望显著降低 ASF 的内存用量，但不希望 `OptimizationMode` 造成严重的性能下降，那么这是您可以选择的最佳技巧之一。

* * *

## ASF 调优（中级）

以下技巧**会造成严重的性能下降**，应谨慎使用。

- 作为最后的手段，您可以通过修改 `OptimizationMode` **[全局配置属性](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-CN#全局配置)**&#8203;调整 `MinMemoryUsage`。 请仔细阅读这个选项的作用，因为它会严重损失性能并且几乎不会减少内存的消耗。 通常，只有在您按照&#8203;**[运行时环境调优](#运行时环境调优高级)**&#8203;作出的调整仍然不能满足需求的情况下，这才是**您应该最后尝试的方式**。

* * *

## 建议的优化

- 从简单的 ASF 配置开始，也许您只是以错误的方式使用了 ASF，例如为所有的机器人启动多个进程，或者在只需要自动启动一两个机器人的情况下启动了所有机器人。
- 如果仍然不理想，通过设置合适的 `COMPlus_` 环境变量启用所有上述的配置选项。 特别是 `GCLatencyLevel` 能够在轻微影响性能的情况下显著减少内存用量。
- 如果这样仍然没有效果，作为最后的手段，您可以启用 `OptimizationMode` 的 `MinMemoryUsage` 选项。 这会强制 ASF 同步执行几乎所有操作，使其运行速度明显变慢，但在并行执行时也不再依赖于线程池来平衡。

进一步减少内存用量是不可能的，此时您的 ASF 的性能已经严重降低，并且已经耗尽了代码方面与运行时方面所有的可能性。 您应该考虑为 ASF 增加一些内存，即使只增加 128 MB 也有明显的差别。