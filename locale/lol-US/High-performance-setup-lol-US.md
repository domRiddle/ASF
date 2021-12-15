# HIGH-PERFORMANCE SETUP

DIS AR TEH EGSAKT OPPOSIET OV **[LOW-MEMS SETUP](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup-lol-US)** AN TYPICALLY U WANTS 2 FOLLOW DOSE TIPS IF U WANTS 2 FURTHR INCREASE ASF PERFORMANCE (IN TERMS OV CPU SPED), 4 POTENTIAL COST OV INCREASD MEMS USAGE.

---

ASF ALREADY TRIEZ 2 PREFR PERFORMANCE WHEN IT COMEZ 2 GENERAL BALANCD TUNIN, THEREFORE THAR IZ NOT LOT U CAN DO 2 FURTHR INCREASE ITZ PERFORMANCE, ALTHOUGH URE NOT COMPLETELY OUT OV OPSHUNS EITHR. HOWEVR, KEEP IN MIND DAT DOSE OPSHUNS R NOT ENABLD BY DEFAULT, WHICH MEANZ DAT THEYRE NOT GUD ENOUGH 2 CONSIDR THEM BALANCD 4 MAJORITY OV USAGEZ, THEREFORE U SHUD DECIDE YOURSELF IF MEMS INCREASE BROUGHT BY THEM IZ ACCEPTABLE 4 U.

---

## RUNTIME TUNIN (ADVANCD)

Below tricks **involve serious memory and startup time increase** and should therefore be used with caution.

The recommended way of applying those settings is through `DOTNET_` environment properties. OV COURSE, U CUD ALSO USE OTHR METHODZ, E.G. `runtimeconfig.json`, BUT SUM SETTINGS R IMPOSIBLE 2 BE SET DIS WAI, AN ON TOP OV DAT ASF WILL REPLACE UR CUSTOM `runtimeconfig.json` WIF ITZ OWN ON TEH NEXT UPDATE, THEREFORE WE RECOMMEND ENVIRONMENT PROPERTIEZ DAT U CAN SET EASILY PRIOR 2 LAUNCHIN TEH PROCES.

.NET runtime allows you to **[tweak garbage collector](https://docs.microsoft.com/dotnet/core/run-time-config/garbage-collector)** in a lot of ways, effectively fine-tuning the GC process according to your needs. We've documented below properties that are especially important in our opinion.

### [`gcServer`](https://docs.microsoft.com/dotnet/core/run-time-config/garbage-collector#flavors-of-garbage-collection)

> CONFIGUREZ WHETHR TEH APPLICASHUN USEZ WERKSTASHUN GARBAGE COLLECSHUN OR SERVR GARBAGE COLLECSHUN.

U CAN READ TEH EGSAKT SPECIFIC OV TEH SERVR GC AT **[FUNDAMENTALS OV GARBAGE COLLECSHUN](https://docs.microsoft.com/dotnet/standard/garbage-collection/fundamentals)**.

ASF IZ USIN WERKSTASHUN GARBAGE COLLECSHUN BY DEFAULT. DIS AR TEH MAINLY CUZ OV GUD BALANCE TWEEN MEMS USAGE AN PERFORMANCE, WHICH IZ MOAR THAN ENOUGH 4 JUS FEW BOTS, AS USUALLY SINGLE CONCURRENT BAKGROUND GC THREAD IZ FAST ENOUGH 2 HANDLE ENTIRE MEMS ALLOCATD BY ASF.

HOWEVR, TODAI WE HAS LOT OV CPU COREZ DAT ASF CAN GREATLY BENEFIT FRUM, BY HAVIN DEDICATD GC THREAD PER EACH CPU VCORE DAT IZ AVAILABLE. DIS CAN GREATLY IMPROOOV TEH PERFORMANCE DURIN HEAVY ASF TASKZ SUCH AS PARSIN BADGE PAGEZ OR TEH INVENTORY, SINCE EVRY CPU VCORE CAN HALP, AS OPPOSD 2 JUS 2 (MAIN AN GC). SERVR GC IZ RECOMMENDD 4 MACHINEZ WIF 3 CPU VCOREZ AN MOAR, WERKSTASHUN GC IZ AUTOMATICALLY FORCD IF UR MACHINE HAS JUS 1 CPU VCORE, AN IF U HAS EGSAKTLY 2 DEN U CAN CONSIDR TRYIN BOTH (RESULTS CUD VARY).

SERVR GC ITSELF DOEZ NOT RESULT IN VRY HUGE MEMS INCREASE BY JUS BEAN ACTIV, BUT IT HAS MUTCH BIGGR GENERASHUN SIZEZ, AN THEREFORE IZ FAR MOAR LAZY WHEN IT COMEZ 2 GIVIN MEMS BAK 2 OS. U CUD FIND YOURSELF IN SWEET SPOT WER SERVR GC INCREASEZ PERFORMANCE SIGNIFICANTLY AN UD LIEK 2 KEEP USIN IT, BUT AT TEH SAME TIEM U CANT AFFORD DAT HUGE MEMS INCREASE DAT COMEZ OUT OV USIN IT. LUCKILY 4 U, THAR IZ "BEST OV BOTH WORLDZ" SETTIN, BY USIN SERVR GC WIF **[`GCLatencyLevel`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup-lol-US#gclatencylevel)** CONFIGURASHUN PROPERTY SET 2 `0`, WHICH WILL STILL ENABLE SERVR GC, BUT LIMIT GENERASHUN SIZEZ AN FOCUS MOAR ON MEMS. ALTERNATIVELY, U MITE ALSO EXPERIMENT WIF ANOTHR PROPERTY, **[`GCHeapHardLimitPercent`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup-lol-US#gcheafardlimitpersent)**, OR EVEN BOTH OV THEM AT TEH SAME TIEM.

HOWEVR, IF MEMS IZ NOT PROBLEM 4 U (AS GC STILL TAKEZ INTO AKOWNT UR AVAILABLE MEMS AN TWEAKZ ITSELF), IT BE MUTCH BETTR IDEA 2 NOT CHANGE DOSE PROPERTIEZ AT ALL, ACHIEVIN SUPERIOR PERFORMANCE IN RESULT.

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

You can enable selected properties by setting appropriate environment variables. 4 EXAMPLE, ON LINUX (SHELL):

```shell
export DOTNET_gcServer=1

export DOTNET_TieredPGO=1
export DOTNET_ReadyToRun=0
export DOTNET_TC_QuickJitForLoops=1

./ArchiSteamFarm # For OS-specific build
```

OR ON WINDOWS (POWERSHELL):

```powershell
$Env:DOTNET_gcServer=1

$Env:DOTNET_TieredPGO=1
$Env:DOTNET_ReadyToRun=0
$Env:DOTNET_TC_QuickJitForLoops=1

.\ArchiSteamFarm.exe # For OS-specific build
```

---

## RECOMMENDD OPTIMIZASHUN

- ENSURE DAT URE USIN DEFAULT VALUE OV `OptimizationMode` WHICH IZ `MaxPerformance`. DIS AR TEH BY FAR TEH MOST IMPORTANT SETTIN, AS USIN `MinMemoryUsage` VALUE HAS DRAMATIC EFFECTS ON PERFORMANCE.
- ENABLE SERVR GC. SERVR GC CAN BE IMMEDIATELY SEEN AS BEAN ACTIV BY SIGNIFICANT MEMS INCREASE COMPARD 2 WERKSTASHUN GC. This will spawn a GC thread for every CPU thread your machine has in order to perform GC operations in parallel with maximum speed.
- If you can't afford memory increase due to server GC, consider tweaking **[`GCLatencyLevel`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup#gclatencylevel)** and/or **[`GCHeapHardLimitPercent`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup#gcheaphardlimitpercent)** to achieve "the best of both worlds". HOWEVR, IF UR MEMS CAN AFFORD IT, DEN IZ BETTR 2 KEEP IT AT DEFAULT - SERVR GC ALREADY TWEAKZ ITSELF DURIN RUNTIME AN IZ SMART ENOUGH 2 USE LES MEMS WHEN UR OS WILL TRULY NED IT.
- You can also consider increased optimization for longer startup time with additional tweaking through other `DOTNET_` properties explained above.

Applying recommendations above allows you to have superior ASF performance that should be blazing fast even with hundreds or thousands of enabled bots. CPU SHUD NOT BE BOTTLENECK NOMORE, AS ASF IZ ABLE 2 USE UR ENTIRE CPU POWR WHEN NEEDD, CUTTIN REQUIRD TIEM 2 BARE MINIMUM. TEH NEXT STEP WUD BE CPU AN RAM UPGRADEZ.