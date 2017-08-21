# Low-memory setup

***

ASF is extremely lightweight on resources by definition, depending on your usage even 128 MB VPS with Linux is capable of running it, although going that low is not recommended and can lead to issues. While being light, ASF is not afraid of asking OS for more memory, if such memory is needed for ASF to operate with optimal speed.

ASF as an application tries to be as much optimized and efficient as possible, which also takes in mind resources being used during execution. When it comes to memory, ASF prefers performance over memory consumption, which can result in temporary memory "spikes", that can be noticed e.g. with accounts having 3+ badge pages, as ASF will fetch and parse first page, read from it total number of pages, then launch fetch task for every extra page, which results in concurrent fetching and parsing of remaining pages. This speeds up execution, for cost of increased memory usage. Similar thing is happening e.g. with parsing active trade offers, ASF is also parsing them all concurrently. On top of all of that, ASF (C# runtime) doesn't return unused memory back to OS immediately. Huh? What's going on?

ASF is extremely well optimized, and makes use of available resources as much as possible. High memory usage of ASF doesn't mean that ASF actively **uses** that memory and **needs it**. Very often ASF will keep some memory allocated for some "room" for future actions, as by not asking OS for every memory chunk we're improving performance. The runtime should automatically release unused ASF memory back to OS when OS will **truly** need it. Remember - **[unused memory is wasted memory](http://www.howtogeek.com/128130/htg-explains-why-its-good-that-your-computers-ram-is-full/)**. You run into issues when the memory you **need** is higher than the memory that is available for you, not when ASF keeps some extra for having free space for functions that will execute in a moment. You run into problems when your Linux kernel is killing ASF process due to OOM (out of memory), not when you see ASF process as top memory consumer in `htop`.

Server GC being used in ASF by default is smart enough to take into account not only ASF itself, but also your OS. When you have a lot of free memory, ASF will ask for whatever is needed to improve the performance. This can be even as much as 1 GB. When your OS memory is close to being full, ASF will automatically release some of it back to the OS to help things settle down, which can result in ASF memory as low as 50 MB. This is why ASF process memory varies from setup to setup, as ASF will do its best to use available resources in **as efficient way as possible**, and not in a fixed way like it was done during Windows XP times. The actual (real) memory usage that ASF is using can be verified with `!stats` **[command](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)**, and is usually around 4 MB for just a few bots. Everything else is shared runtime memory (around 40-50 MB) and room for execution (vary). This is also why the same ASF can use as little as 50 MB in low-memory VPS environment, while using even up to 1 GB on your desktop.

***

Of course, there are a lot of ways how you can help point ASF at the right direction in terms of the memory you expect to use. In general if you don't need to do it, it's best to let server GC work in peace and do whatever it considers is best. But this is not always possible, for example if your Linux server is also hosting several websites, MySQL database and PHP workers, then you can't really afford ASF shrinking itself when you run close to OOM, as it's usually too late and performance degradation comes sooner. This is usually when you might be interested in further tuning, and therefore reading this page.

Below suggestions are divided into a few categories, with varied difficulty.

***

## ASF setup (easy)

Below tricks **do not affect performance negatively** and can be safely applied to all setups.

- Never run more than one ASF instance. ASF is meant to handle unlimited number of bots all at once, and unless you're binding every ASF instance to different interface/IP address, you should have exactly **one** ASF process, with multiple bots (if needed).
- Make use of `ShutdownOnFarmingFinished`. Active bot takes more resources than deactivated one. It's not a significant save, as the state of bot still needs to be kept, but you're saving some amount of resources, especially all resources related to networking, such as TCP sockets. You need only one active bot to keep ASF instance running, and you can always bring up other bots if needed.
- Keep your bots number low. Not `Enabled` bot instance takes less resources, as ASF doesn't bother starting it. Also keep in mind that ASF has to create a bot for each of your configs, therefore if you don't need to `!start` given bot and you want to save some extra memory, you can temporarily rename `Bot.json` to e.g. `Bot.json.bak` in order to avoid creating state for your disabled bot instance in ASF. This way you won't be able to `!start` it without renaming it back, but ASF also won't bother keeping state of this bot in memory, leaving room for other things (very small save, in 99.9% cases you shouldn't bother with it, just keep your bots with `Enabled` of `false`).
- Fine-tune your configs. Especially global ASF config has many variables to adjust, for example by increasing `LoginLimiterDelay` you'll bring up your bots slower, which will allow already started instance to fetch badges in the meantime, as opposed to bringing up your bots faster, which will take more resources as more bots will do major work (such as parsing badges) at the same time. The less work that has to be done at the same time - the less memory used.

Those are a few things you can keep in mind when dealing with memory usage. However, those things don't have any "crucial" matter on memory usage, because memory usage comes mostly from things ASF has to deal with, and not from internal structures used for cards farming.

The most resources-heavy functions are:
- Badge page parsing
- Inventory parsing

Which means that memory will spike the most when ASF is dealing with reading badge pages, and when it's dealing with its inventory (e.g. sending trade or working with STM). This is because ASF has to deal with really huge amount of data - the memory usage of your favourite browser launching those two pages will not be any lower than that. Sorry, that's how it works - decrease number of your badge pages, and keep number of your inventory items low, that can for sure help.

***

## ASF tuning (intermediate)

Below tricks **involve performance degradation** and should be used with caution.

- Enable `BackgroundGCPeriod` **[global config property](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#global-config)** by setting it to `1` or `2`. This can help to keep memory low while sacrificing only a bit of constant CPU power for doing so. If you don't need to go that aggressive, a more sane `10` value is recommended.
- As a last resort, you can tune ASF for `MinMemoryUsage` through `OptimizationMode` **[global config property](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#global-config)**. Read carefully its purpose, as this is serious performance degradation for nearly no memory benefits. This is typically **the last thing you want to do**, long after you go through **[runtime tuning](https://github.com/JustArchi/ArchiSteamFarm/wiki/Low-memory-setup#runtime-tuning-advanced)** to ensure that you're forced to do this.

***

## Runtime tuning (advanced)

Below tricks **involve serious performance degradation** and should be used with caution.

`ArchiSteamFarm.runtimeconfig.json` allows you to tune ASF runtime, especially allowing you to switch between server GC and workstation GC.

> The garbage collector is self-tuning and can work in a wide variety of scenarios. You can use a configuration file setting to set the type of garbage collection based on the characteristics of the workload. The CLR provides the following types of garbage collection:
>
> Workstation garbage collection, which is for all client workstations and stand-alone PCs. This is the default setting for the <gcServer> element in the runtime configuration schema.
>
> Server garbage collection, which is intended for server applications that need high throughput and scalability. Server garbage collection can be non-concurrent or background.

More can be read at **[fundamentals of garbage collection](https://docs.microsoft.com/en-us/dotnet/standard/garbage-collection/fundamentals)**.

ASF is using server garbage collection by default. This is dictated mainly by the fact that today we have a lot of CPU cores that ASF can greatly benefit from, by having a dedicated GC thread per each CPU core that is available. This can greatly improve the performance during heavy ASF tasks such as parsing badge pages or the inventory. Server GC is recommended for machines with 3 cores and more, workstation GC is automatically forced if your machine has just 1 core, and if you have exactly 2 then you can consider trying both.

Server GC itself does not result in a very huge memory increase by just being active, but it is far more lazy when it comes to giving memory back to OS, that's why usually just setting `BackgroundGCPeriod` to `1` or `2` should be enough in order to still keep awesome performance that comes from server GC, while forcing it to give back more unused memory to OS in fixed intervals. However, if your situation requires it, you can disable server GC entirely by changing `System.GC.Server` property of `ArchiSteamFarm.runtimeconfig.json` from `true` to `false`. This will force usage of traditional workstation GC. If you decided to do this, you should probably turn off `BackgroundGCPeriod` as well, because it won't bring a big improvement anymore - workstation GC is pretty conservative as it is.

While this was an option worth mentioning, you shouldn't disable server GC unless you have a strong reason for doing so. It can result in major ASF performance drop, basically limiting ASF processing power to just 2 cores at a time. If possible, try to keep server GC enabled. Properly tuned `BackgroundGCPeriod` should be good enough for keeping server GC under control.

In addition to changing between server and workstation GC, there is also an interesting **[configuration knob](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/clr-configuration-knobs.md)** that you can use - `gcTrimCommitOnLowMemory`.

> When set we trim the committed space more aggressively for the ephemeral seg. This is used for running many instances of server processes where they want to keep as little memory committed as possible

You can enable it by setting `COMPlus_gcTrimCommitOnLowMemory` environment variable to `1`. For example on Linux with:

```
export COMPlus_gcTrimCommitOnLowMemory=1
./ArchiSteamFarm
```

To the best of my knowledge I'm not even sure if this option works properly. It definitely won't hurt to try though.

***

## Recommended optimization

- Start from simple ASF setup tricks, perhaps you're just using your ASF in a wrong way such as starting the process several times for all of your bots, or keeping all of them active if you need just one or two to autostart.
- If simple ASF setup tricks didn't help, experiment with `BackgroundGCPeriod`, this brings "the best of both worlds", by not affecting performance nearly at all, while shrinking memory usage in fixed intervals. A value such as `10` is sane enough to recommend it, although if you have more strict memory environment then you can go as low as `1` or `2`.
- If it's still not enough, enable `gcTrimCommitOnLowMemory` configuration knob by setting `COMPlus_gcTrimCommitOnLowMemory` environment variable to `1`.
- In 99.9% cases you don't want to go further, even if you have strict memory environment. At this point we're bringing serious performance degradation by disabling server GC and enabling workstation GC. Disable previously enabled `BackgroundGCPeriod` and set `System.GC.Server` to `false`.
- If despite of that memory usage spikes still above your expectations, re-enable `BackgroundGCPeriod` despite of already using workstation GC.
- If even that didn't help, as a last resort enable `MinMemoryUsage` `OptimizationMode`. It's physically impossible to decrease memory even further, your ASF is already heavily degraded in terms of performance and you depleted all your possibilities, both code-wise and runtime-wise. Next step is rewriting ASF into C++ 😆.