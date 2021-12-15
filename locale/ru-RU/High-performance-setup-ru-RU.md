# Конфигурация для высокой производительности

Это полная противоположность **[конфигурации для низкого потребления ОЗУ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup-ru-RU)** и обычно вы захотите следовать этим советам если хотите дополнительно увеличить производительность ASF (в плане скорости ЦПУ), ценой увеличения потребляемой памяти.

---

ASF и так пытается предпочитать производительность когда дело доходит до баланса, поэтому вы не многое можете сделать чтобы улучшить производительность ещё больше, хотя некоторые возможности есть. Однако помните, что эти настройки не включены по умолчанию, а значит они недостаточно хороши чтобы считать их сбалансированными для большинства пользователей, поэтому вам надо решить, допустимо ли для вас дальнейшее увеличение потребляемой памяти в качестве платы за производительность.

---

## Расширенная настройка среды исполнения

Below tricks **involve serious memory and startup time increase** and should therefore be used with caution.

Рекомендуется применять эти настройки через переменные среды с префиксом `DOTNET_`. Разумеется, вы можете использовать и другие методы, например файл `runtimeconfig.json`, но некоторые настройки таким образом поменять не удастся, а кроме того ASF заменит ваш `runtimeconfig.json` на стандартный при следующем обновлении, поэтому мы рекомендуем переменные среды, которые вы легко можете установить перед запуском процесса.

Среда выполнения .NET позволяет вам **[настраивать сборщик мусора](https://docs.microsoft.com/dotnet/core/run-time-config/garbage-collector)** несколькими способами, эффективно настраивая процесс сборки мусора в соответствии с вашими потребностями. We've documented below properties that are especially important in our opinion.

### [`gcServer`](https://docs.microsoft.com/dotnet/core/run-time-config/garbage-collector#flavors-of-garbage-collection)

> Эта настройка определяет, будет ли приложение использовать сборщик мусора для рабочих станций или серверный сборщик мусора.

Вы можете прочесть полное описание серверной сборки мусора в статье "**[основы сборки мусора](https://docs.microsoft.com/dotnet/standard/garbage-collection/fundamentals)**".

ASF по умолчанию использует сборку мусора для базовых станций. В основном потому, что этот вариант предоставляет хороший баланс между потреблением памяти и производительности, этого более чем достаточно для небольшого числа ботов, поскольку только один параллельный фоновый поток GC достаточно быстр чтобы справиться со всеми процессами выделения памяти в ASF.

Однако, в наши дни у нас есть ЦПУ c большим количеством ядер, и ASF может получить из этого выгоду, имея по одному выделенному потоку GC на каждое доступное виртуальное ядро ЦПУ. Это может значительно улучшить производительность во время тяжёлых задач в ASF, таких как обработка страниц значков или инвентаря, поскольку каждое виртуальное ядро ЦПУ сможет помочь, в противовес только 2 (основной и GC). Серверный GC рекомендуется для машин с 3 и более виртуальных ядер ЦПУ, GC для рабочих станций принудительно включается или машина имеет только одно виртуальное ядро ЦПУ, а если у вас их ровно 2 вы можете попробовать оба способа (результаты могут быть разные).

Серверный GC сам по себе не даёт большого увеличения потребляемой памяти просто будучи включенным, однако у него гораздо больше объёмы генерации, и поэтому он более ленивый когда дело доходит до возвращения памяти ОС. Вы можете обнаружить что вам нравится как сильно серверный GC увеличивает производительность и вы хотите этим пользоваться, но в то же время вы не можете позволить себе значительного увеличения потребляемой памяти, которое из этого следует. К счастью для вас, есть вариант настроек для получения "лучшего из обоих миров", это использование серверного сборщика мусора совместно с настройкой **[`GCLatencyLevel`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup-ru-RU#gclatencylevel)** установленной в `0`, что позволит использовать серверный сборщик мусора, но ограничить размеры поколений объектов и больше сфокусироваться на памяти. В качестве альтернативы вы также можете поэкспериментировать с другой настройкой, **[`GCHeapHardLimitPercent`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup-ru-RU#gcheaphardlimitpercent)**, или даже одновременно с ними обеими.

Однако, если память для вас не проблема (поскольку сборщик мусора всё равно учитывает объём доступной у вас памяти и подстраивается), гораздо лучшей идеей будет вообще не менять эти настройки, получив в результате превосходную производительность.

### **[`DOTNET_TieredPGO`](https://docs.microsoft.com/dotnet/core/run-time-config/compilation#profile-guided-optimization)**

> This setting enables dynamic or tiered profile-guided optimization (PGO) in .NET 6 and later versions.

Disabled by default. In a nutshell, this will cause JIT to spend more time analyzing ASF's code and its patterns in order to generate superior code optimized for your typical usage. If you want to learn more about this setting, visit **[performance improvements in .NET 6](https://devblogs.microsoft.com/dotnet/performance-improvements-in-net-6)**.

### **[`DOTNET_ReadyToRun`](https://docs.microsoft.com/dotnet/core/run-time-config/compilation#readytorun)**

> Configures whether the .NET Core runtime uses pre-compiled code for images with available ReadyToRun data. Disabling this option forces the runtime to JIT-compile framework code.

Enabled by default. Disabling this in combination with enabling `DOTNET_TieredPGO` allows you to extend tiered profile-guided optimization to the whole .NET platform, and not just ASF code.

### **[`DOTNET_TC_QuickJitForLoops`](https://docs.microsoft.com/dotnet/core/run-time-config/compilation#quick-jit-for-loops)**

> Configures whether the JIT compiler uses quick JIT on methods that contain loops. Enabling quick JIT for loops may improve startup performance. However, long-running loops can get stuck in less-optimized code for long periods.

Disabled by default. While the description doesn't make it obvious, enabling this will allow methods with loops to go through additional compilation tier, which will allow `DOTNET_TieredPGO` to do a better job by analyzing its usage data.

---

Вы можете включить выбранные настройки, установив соответствующие переменные среды. Например, для Linux (shell):

```shell
export DOTNET_gcServer=1

export DOTNET_TieredPGO=1
export DOTNET_ReadyToRun=0
export DOTNET_TC_QuickJitForLoops=1

./ArchiSteamFarm # For OS-specific build
```

Или для Windows (powershell):

```powershell
$Env:DOTNET_gcServer=1

$Env:DOTNET_TieredPGO=1
$Env:DOTNET_ReadyToRun=0
$Env:DOTNET_TC_QuickJitForLoops=1

.\ArchiSteamFarm.exe # For OS-specific build
```

---

## Рекомендуемые оптимизации

- Убедитесь что используете значение по умолчанию в параметре `OptimizationMode`, равное `MaxPerformance`. На данный момент это самая важная настройка, поскольку использование значения `MinMemoryUsage` имеет драматический эффект на производительность.
- Включите серверный сборщик мусора. Вы сразу заметите что серверный сборщик мусора активен по значительному увеличению потребляемой памяти в сравнении со сборщиком мусора для рабочих станций. This will spawn a GC thread for every CPU thread your machine has in order to perform GC operations in parallel with maximum speed.
- If you can't afford memory increase due to server GC, consider tweaking **[`GCLatencyLevel`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup#gclatencylevel)** and/or **[`GCHeapHardLimitPercent`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup#gcheaphardlimitpercent)** to achieve "the best of both worlds". Однако, если память позволяет, лучше оставить этот параметр по умолчанию - серверный сборщик мусора подстраивает себя во время работы и достаточно умён чтобы использовать меньше памяти когда вашей ОС это действительно необходимо.
- You can also consider increased optimization for longer startup time with additional tweaking through other `DOTNET_` properties explained above.

Applying recommendations above allows you to have superior ASF performance that should be blazing fast even with hundreds or thousands of enabled bots. ЦПУ больше не будет узким местом, поскольку ASF сможет использовать при необходимости всю доступную производительность ЦПУ, снижая требуемое время до необходимого минимума. Следующим шагом будет апгрейд процессора и памяти.