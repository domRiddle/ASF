# Compatibility

ASF is a C# application that is running on .NET platform. This means that ASF is not compiled directly into **[machine code](https://en.wikipedia.org/wiki/Machine_code)** that is running on your CPU, but into **[CIL](https://en.wikipedia.org/wiki/Common_Intermediate_Language)** that requires a CIL-compatible runtime for executing it. This approach has gigantic amount of advantages, as CIL is platform-independent, which is why there is a single `ASF.exe` binary that is being natively executed on all OSes supported by ASF, especially Windows, Linux and OS X. There is not only no emulation needed, but also support for all platform-related and hardware-related optimizations, such as CPU SSE instructions.

This also means that ASF has **no specific OS requirement**, because it requires working **runtime** on that OS and not OS itself. As long as that runtime is executing ASF code properly, it does not matter whether underlying OS is Windows, Linux, OS X, BSD, Sony Playstation 4 or Nintendo Wii - it might be shocking to you, but all the platforms that were mentioned **can run ASF** just fine, because there is **working .NET platform implementation for them**.

---

## Runtime requirements

ASF as a program is targetting **.NET 4.6.1 platform** right now, and should work just fine with any working implementation of that platform. Currently we have two implementations that are passing that requirement, and it's official **[.NET Framework](https://en.wikipedia.org/wiki/.NET_Framework)** by Microsoft, and unofficial .NET Framework implementation called **[Mono](https://en.wikipedia.org/wiki/Mono_(software))**.

---

### .NET Framework

Currently latest version of .NET Framework is **[4.6.2](https://www.microsoft.com/en-us/download/details.aspx?id=53345)**, and it can be installed on following Windows OSes:

- Windows 10
- Windows 8.1 (x86 and x64)
- Windows 8 (x86 and x64)
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