# Конфигурация для низкого потребления ОЗУ

Это полная противоположность **[ конфигурации для высокой производительности](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/High-performance-setup-ru-RU)** и обычно вам понадобятся эти советы если вы хотите уменьшить объём занимаемой ASF памяти за счёт общего снижения производительности.

* * *

ASF is extremely lightweight on resources by definition, depending on your usage even 128 MB VPS with Linux is capable of running it, although going that low is not recommended and can lead to various issues. Несмотря на малый вес, ASF не стесняется запрашивать у ОС дополнительную память, если это необходимо для оптимизации скорости работы ASF.

ASF как приложение пытается быть настолько оптимальным и эффективным насколько это возможно, учитывая при этом ресурсы необходимые для работы. Когда дело доходит до памяти, ASF предпочитает производительность а не экономию памяти, и в результате могут появляться "пики" потребления памяти, которые можно заметить если например у аккаунта 3+ страницы со значками, поскольку в этом случае ASF запросит первую страницу, считает с неё номер страниц и затем запросит сразу все дополнительные страницы для параллельной обработки. That "extra" memory usage (compared to bare minimum for operation) can dramatically speed up execution and overall performance, for the cost of increased memory usage that is needed to do all of those things in parallel. Similar thing is happening to all other general ASF tasks that can be run in parallel, e.g. with parsing active trade offers, ASF can parse all of them at once, as they're all independent of each other. On top of that, ASF (C# runtime) will **not** return unused memory back to OS immediately afterwards, which you can quickly notice in form of ASF process only taking more and more memory, but never giving that memory back to the OS. Some people might already find it questionable, maybe even suspect a memory leak, but don't worry, all of this is to be expected.

ASF очень хорошо оптимизирована, и максимально эффективно использует доступные ресурсы. Высокое потребление памяти не означает что ASF активно **использует** эту память и **нуждается в ней**. Very often ASF will keep allocated memory as "room" for future actions, because we can drastically improve performance if we don't need to ask OS for every memory chunk that we're about to use. Среда выполнения должна автоматически освободить неиспользуемую ASF память и отдать её ОС если ОС **действительно** в этом нуждается. **[Unused memory is wasted memory](https://www.howtogeek.com/128130/htg-explains-why-its-good-that-your-computers-ram-is-full)**. You run into issues when the memory you **need** is higher than the memory that is available for you, not when ASF keeps some extra allocated with purpose of speeding up functions that will execute in a moment. Проблемы начались если ядро Linux убивает процесс ASF из-за OOM (out of memory), а не когда вы видите процесс ASF как основной потребитель памяти в `htop`.

Garbage collector being used in ASF is a very complex mechanism, smart enough to take into account not only ASF itself, but also your OS and other processes. Когда у вас много свободной памяти - ASF будет запрашивать столько, сколько позволит увеличить производительность. Это может быть вплоть до 1ГиБ (с серверным GC). When your OS memory is close to being full, ASF will automatically release some of it back to the OS to help things settle down, which can result in overall ASF memory usage as low as 50 MB. The difference between 50 MB and 1 GB is huge, but so is the difference between small 512 MB VPS and huge dedicated server with 32 GB. If ASF can guarantee that this memory will come useful, and at the same time nothing else requires it right now, it'll prefer to keep it and automatically optimize itself based on routines that were executed in the past. The GC used in ASF is self-tuning and will achieve better results the longer the process is running.

This is also why ASF process memory varies from setup to setup, as ASF will do its best to use available resources in **as efficient way as possible**, and not in a fixed way like it was done during Windows XP times. The actual (real) memory usage that ASF is using can be verified with `stats` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**, and is usually around 4 MB for just a few bots, up to 30 MB if you use stuff like **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** and other advanced features. Keep in mind that memory returned by `stats` command also includes free memory that hasn't been reclaimed by garbage collector yet. Everything else is shared runtime memory (around 40-50 MB) and room for execution (vary). This is also why the same ASF can use as little as 50 MB in low-memory VPS environment, while using even up to 1 GB on your desktop. ASF is actively adapting to your environment and will try to find optimal balance in order to neither put your OS under pressure, nor limit its own performance when you have a lot of unused memory that could be put in use.

* * *

Of course, there are a lot of ways how you can help point ASF at the right direction in terms of the memory you expect to use. In general if you don't need to do it, it's best to let garbage collector work in peace and do whatever it considers is best. But this is not always possible, for example if your Linux server is also hosting several websites, MySQL database and PHP workers, then you can't really afford ASF shrinking itself when you run close to OOM, as it's usually too late and performance degradation comes sooner. This is usually when you might be interested in further tuning, and therefore reading this page.

Below suggestions are divided into a few categories, with varied difficulty.

* * *

## Настройки ASF (лёгкий уровень)

Below tricks **do not affect performance negatively** and can be safely applied to all setups.

- Никогда не запускайте больше одной копии ASF. ASF предназначен для одновременной работы с неограниченным числом ботов, и если вы не подключаете каждую копию ASF к отдельному интерфейсу/IP адресу, у вас должен быть ровно **один** процесс ASF, с несколькими ботами (если необходимо).
- Используйте `ShutdownOnFarmingFinished`. Запущенный бот потребляет гораздо больше ресурсов, чем деактивированный. Это незначительная экономия, поскольку состояние бота всё равно придётся хранить, но вы экономите некоторые количество ресурсов, особенно связанных с сетью, таких как сокеты TCP. Вам нужен только один активным бот чтобы ASF продолжил работу, и вы всегда можете запустить других ботов при необходимости.
- Используйте небольшое количество ботов. Бот без параметра `Enabled` занимает меньше ресурсов, поскольку ASF нет нужды запускать его. Также помните что ASF создаёт бота для каждого файла конфигурации, поэтому если вы не собираетесь запускать его командой `start` и хотите сэкономить немного памяти, вы можете временно переименовать `Bot.json` например в `Bot.json.bak` чтобы избежать создания неактивного бота в процессе ASF. При этом вы не сможете запустить его командой `start` не переименовав его назад, но ASF не будет сохранять состояние этого бота в памяти, освободив эту память для других нужд (очень маленький объём, в 99.9% случаев не стоит с этим возиться, просто поставьте своим ботам значение параметра `Enabled` равным `false`).
- Настройте свои файлы конфигурации. Файл глобальной конфигурации ASF имеет особенно много переменных, которые можно изменить, например, увеличив значение `LoginLimiterDelay` вы сделаете запуск ботов более медленным, что даст возможность уже запущенным ботам параллельно запрашивать страницы значков, в противоположность быстрому запуску ботов, который потребует больше ресурсов, поскольку больше ботов будут делать основную работу (такую как разбор страниц значков) в одно и то же время. Чем меньше работы выполняется одновременно - тем меньше занимаемый объём памяти.

Those are a few things you can keep in mind when dealing with memory usage. However, those things don't have any "crucial" matter on memory usage, because memory usage comes mostly from things ASF has to deal with, and not from internal structures used for cards farming.

The most resources-heavy functions are:

- Разбор страниц со значками
- Разбор инвентаря

Which means that memory will spike the most when ASF is dealing with reading badge pages, and when it's dealing with its inventory (e.g. sending trade or working with STM). This is because ASF has to deal with really huge amount of data - the memory usage of your favourite browser launching those two pages will not be any lower than that. Sorry, that's how it works - decrease number of your badge pages, and keep number of your inventory items low, that can for sure help.

* * *

## Расширенная настройка среды исполнения

Below tricks **involve performance degradation** and should be used with caution.

Файл `ArchiSteamFarm.runtimeconfig.json` позволяет вам настроить среду выполнения ASF, в частности позволяя переключаться между серверным GC и GC для рабочих станций.

> Сборщик мусора самонастраивающийся и может работать по большому количеству сценариев. Вы можете использовать настройки в конфигурационном файле чтобы указать тип сборщика мусора основываясь на характеристиках нагрузки. CLR предоставляет следующие типы сборки мусора:
> 
> Сборка мусора для рабочей станции, предназначенная для всех клиентских рабочих станций и отдельных ПК. Это значение по умолчанию для <gcserver> элемента в схеме конфигурации среды выполнения.
> 
> Серверная сборка мусора, которая предназначена для серверных приложений, требующих высокой пропускной способности и масштабируемости. Серверная сборка мусора может быть непараллельной или фоновой.

Вы можете узнать больше, прочитав статью "**[основы сборки мусора](https://docs.microsoft.com/en-us/dotnet/standard/garbage-collection/fundamentals)**".

ASF is already using workstation GC, but you can ensure that it's truly the case by checking if `System.GC.Server` property of `ArchiSteamFarm.runtimeconfig.json` is set to `false`.

In addition to verifying that workstation GC is active, there are also interesting **[configuration knobs](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/clr-configuration-knobs.md)** that you can use - `gcTrimCommitOnLowMemory` and `GCLatencyLevel`.

### `GCLatencyLevel`

> Задаёт уровень задержек GC, который вы хотите оптимизировать.

This works exceptionally well by limiting size of GC generations and in result make GC purge them more frequently and more aggressively. Default (balanced) latency level is `1`, we'll want to use `0`, which will tune for memory usage.

### `gcTrimCommitOnLowMemory`

> Если активно - мы урезаем выделяемую память более агрессивно для недолговечных сегментов. Это используется для запуска множества процессов на сервере, где необходимо чтобы они использовали как можно меньше памяти.

This offers little improvement, but might make GC even more aggressive when system will be low on memory.

* * *

You can enable both by setting appropriate `COMPlus_` environment variables. For example, on Linux:

```shell
export COMPlus_GCLatencyLevel=0
export COMPlus_gcTrimCommitOnLowMemory=1
./ArchiSteamFarm
```

Or on Windows:

```bat
SET COMPlus_GCLatencyLevel=0
SET COMPlus_gcTrimCommitOnLowMemory=1
.\ArchiSteamFarm.exe
```

Especially `GCLatencyLevel` will come very useful as we verified that the runtime indeed optimizes code for memory and therefore drops average memory usage significantly, even with server GC. It's one of the best tricks that you can apply if you want to significantly lower ASF memory usage while not degrading performance too much with `OptimizationMode`.

* * *

## Настройки ASF (средний уровень)

Below tricks **involve serious performance degradation** and should be used with caution.

- В качестве крайней меры, вы можете настроить ASF на минимальное потребление памяти включив `MinMemoryUsage` в **[параметре глобальной конфигурации](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-ru-RU#Файл-глобальной-конфигурации)** `OptimizationMode`. Внимательно прочтите, зачем этот режим нужен, поскольку он приводит к серьёзному ухудшению производительности но незначительно улучшает ситуацию с памятью. В общем случае это **последнее что стоит делать**, уже после того как вы изменили **[настройки среды выполнения](#Расширенная-настройка-среды-исполнения)** чтобы убедиться, что вам без этого не обойтись.

* * *

## Рекомендуемые оптимизации

- Начните с простых советов по настройке ASF, возможно вы просто неправильно им пользуетесь, например запускаете несколько процессов для всех ботов, или держите их активными когда хватит только одного или двух с автозапуском.
- Если этого недостаточно, активируйте все конфигурационные регуляторы, описанные выше, установив соответствующие переменные среды `COMPlus_`. `GCLatencyLevel` даёт особенно значительное улучшение при незначительном влиянии на производительность.
- Если даже это не помогло, в качестве крайней мере включите `MinMemoryUsage` в `OptimizationMode`. Это заставит ASF выполнять почти всё синхронным образом, делая его заметно медленнее но также независящим от балансировки задач пулом потоков когда доходит до параллельной работы.

It's physically impossible to decrease memory even further, your ASF is already heavily degraded in terms of performance and you depleted all your possibilities, both code-wise and runtime-wise. Consider adding some extra memory for ASF to use, even 128 MB would make a great difference.