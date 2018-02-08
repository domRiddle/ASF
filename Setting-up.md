# Setting up

If you arrived here for the first time, welcome! We're very happy to see yet another traveler that is interested in our project, although bear in mind that with great power comes great responsibility - there is a steep learning curve involved here. ASF is not just some toy program with friendly GUI, one click installer and fancy window, it's a very sophisticated and complex Steam tool that can do almost anything, as long as **you care enough** to learn how to configure and use it. If you do, we've got you covered with our wiki and usual wall of text. And if you don't care, just use **[Idle Master](http://www.steamidlemaster.com/)** or something, we won't do homework for you.

If you're still here then it means that you endured our text above, which is nice. Unless you skipped it, then you're going to have a **[bad time](https://www.youtube.com/watch?v=87kxspJmhs8)**... Anyway, ASF is a console app, which means that program itself doesn't have a friendly GUI that you're in general used to. This means that ASF is a standalone executable file that doesn't need installation, and actually works out of the box, but requires configuration prior to launching it.

***

## Quick video setup

For people that prefer watching over reading. Huge thanks to **[@GamingTaylor](https://www.youtube.com/channel/UCTjrsQgjZmBzYzWaAh0zI3Q)** for recording up-to-date video about ASF!

### **[YouTube link](https://www.youtube.com/watch?v=gi2UjXtGWgc)**

Please note that you should still refer to the wiki for further explanation or up-to-date setting up guide. While we consider YouTube video as a good material for actually showing how things are configured and launched, we can't easily update it when things are changed, so it should be a reference material only.

***

## OS-specific setup

In general, you should:
- Install **[.NET Core prerequisites](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)**.
- Download **[latest ASF release](https://github.com/JustArchi/ArchiSteamFarm/releases/latest)** in appropriate OS-specific variant.
- Extract the archive into new location (and `chmod +x ArchiSteamFarm` if you're on Linux/OS X).
- **[Configure ASF](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration)**.
- Launch ASF and have fun.

The above was TL;DR, most likely you'd want to read actual explanation of above steps, which is available below.

First step is ensuring that your OS can even launch ASF properly. ASF is written in C#, based on .NET Core and might require native libraries that are not available on your platform yet. Depending on whether you use Windows, Linux or OS X, you will have different requirements, although all of them are listed in **[.NET Core prerequisites](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)** that you should follow. Simply follow those instructions, there is a chance that you already have all required libraries, but you should double check. For example on Windows, all you need to do is to download & install `Microsoft Visual C++ 2015 Redistributable Update 3 RC`, which could even be already installed by some other game/software that you're using. On Linux, you have a list of libraries that can be obtained with `apt-get install` or anything you're using as your package manager. OS X doesn't have any mandatory dependencies for now, but it might change in the future.

Keep in mind that you don't need entire .NET Core SDK or even runtime, since OS-specific package includes them already, you need only .NET Core prerequisites (dependencies). Since it might be hard to extract the info you're looking for, we listed required dependencies also here, but please refer to original .NET Core source as those might get changed in the future.

---

### **[Windows](https://docs.microsoft.com/en-us/dotnet/core/windows-prerequisites?tabs=netcore2x)**:
- **[Microsoft Visual C++ 2015 Redistributable Update 3 RC](https://www.microsoft.com/en-us/download/details.aspx?id=52685)** (x64 for 64-bit Windows, x86 for 32-bit Windows)

It's possible that this package was already installed by some other software/game you're using, but please double-check by running the installer to be sure.

### **[Linux](https://docs.microsoft.com/en-us/dotnet/core/linux-prerequisites?tabs=netcore2x)**:
Package name depends on distribution, we listed most common ones. You should obtain them with package manager of your distribution (such as `apt-get` on Debian family).
- libunwind8 (libunwind)
- liblttng-ust0 (lttng-ust)
- libcurl3 (libcurl)
- libssl1.0.2 (libssl, openssl-libs, latest 1.0.X version for your distribution)
- libuuid1 (libuuid)
- libkrb5-3 (krb5-libs)
- libicu57 (libicu, latest version for your distribution, right now 57)
- zlib1g (zlib)

At least a few of those should be already natively available on your system (such as zlib1g).

### **[OS X](https://docs.microsoft.com/en-us/dotnet/core/macos-prerequisites)**:
- None for now, although you might need to **[increase the maximum open file limit](https://docs.microsoft.com/en-us/dotnet/core/macos-prerequisites?tabs=netcore2x#increase-the-maximum-open-file-limit)**

---

Next step is downloading **[latest ASF release](https://github.com/JustArchi/ArchiSteamFarm/releases/latest)**. ASF is available in many variants, but you're interested in package that matches your operating system and architecture. For example, if you're using `64`-bit `Win`dows, then you want `ASF-win-x64` package. For more information about available variants, visit **[compatibility](https://github.com/JustArchi/ArchiSteamFarm/wiki/Compatibility)** section.

Once you get your package and extract the zip file, you'll have a bunch of files, folders and everything. If you're using Linux/OS X, don't forget to `chmod +x ArchiSteamFarm`, since permissions are not automatically set in the zip file (this has to be done only once).

Be advised to put ASF in **its own directory** and not in any existing directory you're already using for something else - ASF's auto-updates feature will delete all old and unrelated files when upgrading, which might lead to you losing anything unrelated you put in ASF directory. If you have any extra scripts or files that you want to use with ASF, put them in one folder above.

```
C:\ASF (where you put your own things)
    ├── ASF shortcut.lnk
    ├── Config shortcut.lnk
    ├── Commands.txt
    ├── MyExtraScript.bat
    └── Core (dedicated to ASF only)
         ├── ArchiSteamFarm.dll
         ├── config
         └── (...)
```

This is also a structure we'd recommend, so you don't need to go through a massive number of files and folders included in ASF, since for usage you only need a shortcut to config folder and main binary.

Next step is ASF configuration. Visit **[configuration](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration)** page and follow instructions - you should at least generate one bot config file that you put in `config` directory.

All done? Great, simply launch `ArchiSteamFarm` - either by double clicking on the .exe if you're on Windows, or via navigating to ASF directory (e.g. with `cd`) command and executing `./ArchiSteamFarm` if you're on Linux/OS X. Put extra details if ASF asks for them (such as your 2FA code) and have fun. Reading more about ASF while it's doing its job is a great thing to consider.

***

## Generic setup

This setup is for advanced users that want to set up ASF to run in **[generic](https://github.com/JustArchi/ArchiSteamFarm/wiki/Compatibility#generic)** variant. It's not recommended for people that can use **[OS-specific setup](https://github.com/JustArchi/ArchiSteamFarm/wiki/Setting-up#os-specific-setup)**.

Generic setup makes main sense only for valid released .NET Core variants that we decided to not build OS-specific package for (such as `win-x86`), as well as **[work-in-progress OSes/platforms](https://github.com/dotnet/core/blob/master/roadmap.md#supported-os-versions)** that didn't have any official release yet. Of course, it can also be used in every other setup, especially if you don't want to download much bigger OS-specific package on each update. However, keep in mind that you're in charge of .NET Core runtime in this case. This means that if your .NET Core SDK is unavailable, outdated or broken, ASF won't work.

For generic package, you should:
- Install **[.NET Core prerequisites](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)**.
- Install **[.NET Core SDK](https://www.microsoft.com/net/download/core#/sdk)** (or at least runtime) appropriate for your OS. You most likely want to use an installer. Refer to **[runtime requirements](https://github.com/JustArchi/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)** if you're not sure which version to install.
- Download **[latest ASF release](https://github.com/JustArchi/ArchiSteamFarm/releases/latest)** in generic variant.
- Extract the archive into new location.
- **[Configure ASF](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration)**.
- Launch ASF by either using a helper script or by executing `dotnet /path/to/ArchiSteamFarm.dll` manually from your favourite shell.

Helper scripts (such as `ArchiSteamFarm.cmd` for Windows and `ArchiSteamFarm.sh` for Linux/OS X) are located next to `ArchiSteamFarm.dll` binary. You can use them if you don't want to execute `dotnet` command manually. Obviously they won't work if you didn't install .NET Core SDK and you don't have `dotnet` executable available in your `PATH`.

***

## Want to learn more?

Head over to the rest of **[our wiki](https://github.com/JustArchi/ArchiSteamFarm/wiki)**.