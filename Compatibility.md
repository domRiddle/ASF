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

If you're using OS-specific package then you don't need to worry about runtime requirements, because ASF always ships with required and up-to-date runtime that will work properly as long as you have **[.NET Core prerequisites](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)** installed and up-to-date.

However, if you're running generic package then you must ensure that your .NET Core runtime supports platform required by ASF.

ASF as a program is targetting **.NET Core 2.0** (`netcoreapp2.0`) right now, but it might target newer platform in the future. We like preview builds, so very likely you'll need to have latest .NET Core preview runtime available if you want to use latest ASF code.