# Compatibility

## This page applies to ASF V3 only. If you're using ASF V2, please visit **[ASF V2](https://github.com/JustArchi/ArchiSteamFarm/wiki/_Compatibility-(ASF-V2))** page instead.

ASF is a C# application that is running on .NET Core platform. This means that ASF is not compiled directly into **[machine code](https://en.wikipedia.org/wiki/Machine_code)** that is running on your CPU, but into **[CIL](https://en.wikipedia.org/wiki/Common_Intermediate_Language)** that requires a CIL-compatible runtime for executing it. This approach has gigantic amount of advantages, as CIL is platform-independent, which is why ASF can run natively on many available OSes, especially Windows, Linux and OS X. There is not only no emulation needed, but also support for all platform-related and hardware-related optimizations, such as CPU SSE instructions.

This also means that ASF has **no specific OS requirement**, because it requires working **runtime** on that OS and not OS itself. As long as that runtime is executing ASF code properly, it does not matter whether underlying OS is Windows, Linux, OS X, BSD, Sony Playstation 4, Nintendo Wii or your toaster - as long as there is **[.NET Core for it](https://github.com/dotnet/core-setup#daily-builds)**, ASF will run just fine.

However, regardless of where you run ASF, you must ensure that your target platform has **[.NET Core prerequisites](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)** installed. Those are low-level libraries required for proper runtime functionality and absolutely core for ASF to work in the first place. Very likely you can have some of them (or even all) already installed.

---

## ASF packaging

ASF comes in 2 main flavours - generic package and OS-specific.

---

### Generic

Generic package is platform-agnostic build that doesn't include anything but managed code. This setup requires from you to have .NET Core already installed on your OS **in appropriate version**. We all know how troublesome it is to keep dependencies up-to-date, therefore this package is here mainly for people that **already use** .NET Core and don't want to duplicate their runtime solely for ASF if they can make use of what they have installed already. I do not recommend using generic flavour if you're casual or even advanced user that just want to make ASF work and not dig into .NET Core technical details. In other words - if you know what this is, use it, otherwise just use OS-specific package explained below.

---

## OS-specific

OS-specific package, apart from managed code included in generic package, also includes native code for given platform. In other words, OS-specific package **already includes proper .NET Core runtime inside**, which allows you to entirely skip the whole installation mess and just launch ASF directly, yes, **also on Linux**, where binary is typical Unix ELF executable. OS-specific package, as you can guess from the name, is OS-specific and every OS requires its own version.

ASF is currently available in following OS-specific variants:

- `win-x64` works on 64-bit Windows OSes. This includes Windows 7, 8, 8.1, 10, Server 2008 R2, 2012, 2012 R2, 2016, as well as future versions.
- `linux-arm` works on ARM-based (hard float) Linux distributions. This includes especially Raspberry Pis 2+ and all glibc-based Linux OSes available for them, in current and future versions.
- `linux-x64` works on 64-bit glibc-bacsd Linux distributions. This includes RHEL, Ubuntu, CentOS, Debian, Fedora, OpenSUSE and many other ones, including their derivatives, in current and future versions.
- `osx-x64` works on 64-bit OS X OSes. This includes 10.10, 10.11 and 10.12, as well as future versions.

For more information about RIDs, visit **[RID catalog](https://docs.microsoft.com/en-us/dotnet/core/rid-catalog)**.

Of course, even if you don't have OS-specific package available for your OS-architecture combination, you can always install appropriate .NET Core runtime yourself and run generic ASF flavour, which is also the main reason why it exists in the first place. Generic ASF build is platform-agnostic and will run on any platform that has a working .NET Core runtime. This is important to note - ASF requires .NET Core 2.0+, not some specific OS or architecture. For example, if you're running 32-bit Windows then despite of no dedicated `win-x86` ASF version, you can still install .NET Core 2.0 in `win-x86` version and run generic ASF just fine. We simply can't target every OS-architecture combination that exists and is used by somebody, so we have to draw a line somewhere.

---

## Runtime requirements

If you're using OS-specific package then you don't need to worry about runtime requirements, because ASF always ships with required and up-to-date runtime that will work properly as long as you have 

ASF as a program is targetting **.NET Core 2.0** right now, but it might target newer platform in future.

---

### .NET Framework

Currently latest version of .NET Framework is **[4.7](https://www.microsoft.com/en-us/download/details.aspx?id=55170)**, and it can be installed on following Windows OSes:

- Windows 10
- Windows 8.1 (x86 and x64)
- Windows 7 SP1 (x86 and x64)
- Windows Server 2016
- Windows Server 2012 R2 (x64)
- Windows Server 2012 (x64)
- Windows Server 2008 R2 SP1 (x64)

This means that ASF with .NET Framework **does not** support any older Windows versions that weren't listed above, especially **Windows 7 without SP1**, **Windows Vista** or **Windows XP**. If you can't use any recent version of Windows, for example due to pricing, keep in mind that you can still use ASF on Linux without any issues (and it's free too).

---

### Mono

Mono is available **[here](http://www.mono-project.com/download/)** with an installer for Windows, Linux and OS X. In general if you're using Windows OS, then you probably should stick with official .NET Framework from Microsoft, although Mono could still be interesting alternative for unsupported by official .NET Framework Windows versions, such as **Windows Vista**.

Mono installation/usage is carefully explained in our **[Mono](https://github.com/JustArchi/ArchiSteamFarm/wiki/Mono)** section, so feel free to visit that page if you're interesting in running ASF with Mono.

.NET 4.6.1 instructions were implemented firstly in **[Mono 4.4.0](http://www.mono-project.com/docs/about-mono/releases/4.4.0/#class-libraries)** and that version is absolute minimum for ASF to even launch, but due to rapid development and stability reasons, we will **always** recommend to use latest stable and not anything older. This is mainly because Mono improves its codebase with every revision, and **we do not actively test if ASF still works in new version with any other Mono version than latest stable and nightly**. This means that if you want to use latest ASF, you **must also use latest Mono** that is available at the time of release. Otherwise, **[you might run into issues](https://github.com/JustArchi/ArchiSteamFarm/issues/529)** even if the Mono version you were using worked just fine a moment ago.

Technically, Mono also supports **[many other OSes and setups](http://www.mono-project.com/docs/about-mono/supported-platforms/)** besides Linux and OS X, but we're not capable of testing all of them, so officially we support Mono only on OS X and Linux setups. You shouldn't have any problems running ASF with Mono on any other officially-supported setup, as long as Mono port for that setup is in fact working properly - we simply didn't test that, so we also can't state whether every single Mono port is capable of running ASF flawlessly - we can guarantee that Linux and OS X ports do though.

Of course, in terms of Linux and OS X it's really hard to find incompatible version so you should be able to run ASF on nearly anything - we confirmed that ASF works properly at least with:

- Arch Linux (March 2017)
- CentOS 7.3
- CentOS 6.8
- Debian 9 Stretch
- Debian 8 Jessie
- Debian 7 Wheezy
- Fedora 25
- Fedora 24
- Gentoo (March 2017)
- OS X 10.12.3
- OS X 10.11
- Raspbian Stretch
- Raspbian Jessie
- Ubuntu 17.04 Zesty Zapus
- Ubuntu 16.10 Yakkety Yak
- Ubuntu 16.04 Xenial Xerus

This list is nowhere close to being complete, and it's very likely that you can run ASF even on much older (not to mention newer) OS versions than listed above, so just give it a try!