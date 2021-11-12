# Совместимость

ASF is a C# application that is running on .NET platform. Это значит, что ASF не компилируется напрямую в **[машинный код](https://ru.wikipedia.org/wiki/%D0%9C%D0%B0%D1%88%D0%B8%D0%BD%D0%BD%D1%8B%D0%B9_%D0%BA%D0%BE%D0%B4)**, который исполняется на вашем ЦП, а компилируется в код **[CIL](https://ru.wikipedia.org/wiki/Common_Intermediate_Language)**, которому для работы требуется CIL-совместимая среда выполнения.

Этот подход даёт гигантское количество преимуществ, поскольку CIL платформо-независим, и поэтому ASF может запускаться на множестве доступных ОС, в частности Windows, Linux и OS X. Для этого не только не требуется эмуляция, но даже поддерживаются оптимизации под конкретную аппаратуру, такие как наборы инструкций SSE. Благодаря этому, ASF может достигать превосходной производительности и оптимальности, и при этом оставаться прекрасно совместимым и надёжным.

Это также означает что ASF не имеет **особых требований к ОС**, поскольку всё что он требует это работающую **среду выполнения** на этой ОС, а не саму ОС. As long as that runtime is executing ASF code properly, it does not matter whether underlying OS is Windows, Linux, OS X, BSD, Sony Playstation 4, Nintendo Wii or your toaster - as long as there is **[.NET for it](https://dotnet.microsoft.com/download/dotnet)**, there is **[ASF](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** for it.

However, regardless of where you run ASF, you must ensure that your target platform has **[.NET prerequisites](https://github.com/dotnet/core/blob/main/Documentation/prereqs.md)** installed. Это низкоуровневые библиотеки, необходимые для функционирования среды выполнения и ядра ASF. Вполне возможно, что некоторые (или даже все) из них у вас уже установлены.

---

## Пакеты ASF

ASF поставляется в 2 основных вариациях - универсальный пакет и пакеты под конкретные ОС. С точки зрения функциональности они идентичны, и способны автоматически обновляться. Единственная разница в том, содержится ли там только **универсальный (generic)** пакет, или с ним поставляется среда выполнения **под конкретную ОС**.

---

### Универсальный пакет

Универсальный (`generic`) пакет это платформо-независимая сборка, не включающая себя никакого машинно-ориентированного кода. This setup requires from you to have .NET runtime already installed on your OS **in appropriate version**. We all know how troublesome it is to keep dependencies up-to-date, therefore this package is here mainly for people that **already use** .NET and don't want to duplicate their runtime solely for ASF if they can make use of what they have installed already. Generic package also allows you to run ASF **anywhere where you can obtain working implementation of .NET runtime**, regardless if there exists OS-specific ASF build for it, or not.

It's not recommended to use generic flavour if you're casual or even advanced user that just wants to make ASF work and not dig into .NET technical details. Другими словами - если вы знаете что это, можете пользоваться, иначе вам лучше взять пакет под конкретную ОС, как описано ниже.

#### Пакет для .NET Framework

In addition to generic package mentioned above, there is also `generic-netf` package which is built on top of .NET Framework and not .NET (Core). This package is a legacy variant that provides missing compatibility known from ASF V2 times, and can be run e.g. with **[Mono](https://www.mono-project.com)**, while .NET `generic` package can't as of today.

В основном вам следует **по возможности избегать использования этого пакета**, поскольку большинство операционных систем прекрасно (и гораздо лучше) работают с пакетом `generic`, упомянутым выше. In fact, this package makes sense to be used only on platforms that lack working .NET runtime, while having working Mono implementation. Examples of such platforms include `linux-x86` (32-bit i386/i686 linux), as well as `linux-armel` (ARMv6 boards found e.g. in Raspberry Pi 0 & 1), all of which do not have official working .NET runtime as of today.

As the time goes on with more platforms being supported by .NET and less compatibility between .NET Framework and .NET, `generic-netf` package will be entirely replaced with `generic` one in the future. Please refrain from using it if you can use any .NET package instead, as `generic-netf` is missing a lot of functionality and compatibility compared to .NET versions, and it'll be only less functional as the time goes on. Мы оказываем поддержку этого пакета **только** на машинах, на которых невозможно использовать вариант `generic`, описанный выше (например, `linux-x86`), и только с актуальной средой выполнения (например, последней версией Mono).

---

### Особенности OC

Пакеты под конкретную ОС, помимо управляемого кода, присутствующего в универсальном пакете, также включают в себя машинный код для заданной платформы. In other words, OS-specific package **already includes proper .NET runtime inside**, which allows you to entirely skip the whole installation mess and just launch ASF directly. Пакет под конкретную ОС, как понятно из названия, подходит только для одной ОС, и для каждой ОС нужна своя версия - например для Windows требуется исполняемый файл `ArchiSteamFarm.exe` формата PE32+, а для Linux исполняемый файл `ArchiSteamFarm` формата Unix ELF. Как вы, возможно, знаете, эти два типа несовместимы между собой.

ASF на данный момент доступно для следующих вариантов ОС:

- `win-x64` работает на 64-разрядных ОС Windows. This includes Windows 7 (SP1+), 10, 11, Server 2012+ as well as future versions.
- `linux-arm` работает на 32-разрядных ОС GNU/Linux для процессоров на базе ARM (ARMv7+). This includes platforms such as Raspberry Pi 2 (and newer) with all GNU/Linux OSes available for them (such as Raspberry Pi OS), in current and future versions. This variant will **not** work with older ARM architectures, such as ARMv6 found in Raspberry Pi 0 & 1, it will also not work with OSes that do not implement required GNU/Linux environment (such as Android).
- `linux-arm64` работает на 64-разрядных ОС GNU/Linux для процессоров на базе ARM (ARMv8+). Это включает в себя такие платформы как Raspberry Pi 3 (и более новые) и все доступные для них ОС GNU/Linux для архитектуры AArch64 (такие как Debian), как текущие так и будущие версии. This variant will **not** work with 32-bit OSes that do not have required 64-bit libraries available (such as Raspberry Pi OS), it will also not work with OSes that do not implement required GNU/Linux environment (such as Android).
- `linux-x64` работает на 64-разрядных ОС GNU/Linux. This includes Alpine, CentOS/Fedora/RHEL, Debian/Ubuntu, OpenSUSE/SLES and many other ones, including their derivatives, in current and future versions.
- `osx-arm64` works on 64-bit ARM-based (Apple silicon) OS X OSes. This includes version 11, as well as future ones.
- `win-x64` работает на 64-разрядных ОС OS X. This includes version 10.14, as well as future ones.

Of course, even if you don't have OS-specific package available for your OS-architecture combination, you can always install appropriate .NET runtime yourself and run generic ASF flavour, which is also the main reason why it exists in the first place. Generic ASF build is platform-agnostic and will run on any platform that has a working .NET runtime. This is important to note - ASF requires .NET runtime, not some specific OS or architecture. For example, if you're running 32-bit Windows then despite of no dedicated `win-x86` ASF version, you can still install .NET SDK in `win-x86` version and run generic ASF just fine. Мы просто не можем создать пакеты под все сочетания ОС-архитектура которые могут использоваться, поэтому приходится чем-то ограничиться. x86 не попадает в это ограничение, поскольку это устаревшая архитектура как минимум с 2004 года.

For a complete list of all supported platforms and OSes by .NET 6.0, visit **[release notes](https://github.com/dotnet/core/blob/main/release-notes/6.0/supported-os.md)**.

---

## Требования среды выполнения

If you're using OS-specific package then you don't need to worry about runtime requirements, because ASF always ships with required and up-to-date runtime that will work properly as long as you have **[.NET prerequisites](https://github.com/dotnet/core/blob/main/Documentation/prereqs.md)** installed and up-to-date. In other words, **you don't need to install .NET runtime or SDK**, as OS-specific builds require only native OS dependencies (prerequisites) and nothing else.

However, if you're trying to run **generic** ASF package then you must ensure that your .NET runtime supports platform required by ASF.

ASF as a program is targeting **.NET 6.0** (`net6.0`) right now, but it may target newer platform in the future. `net6.0` is supported since 6.0.100 SDK (6.0.0 runtime), although ASF is configured to prefer **latest runtime at the moment of compilation**, so you should ensure that you have **[latest SDK](https://dotnet.microsoft.com/download)** (or at least runtime) available for your machine. Generic ASF variant may refuse to launch if your runtime is older than the specified minimum supported one during compilation.

Если сомневаетесь - проверьте что использует наша **[система непрерывной интеграции](https://github.com/JustArchiNET/ArchiSteamFarm/actions/workflows/publish.yml?query=branch%3Amain)** для компиляции и развертывания сборок ASF на GitHub. Вы можете найти вывод `dotnet --info` в каждой сборке как часть этапа проверки .NET.