# Compilation

## This page applies to ASF V3 only. If you're using ASF V2, please visit **[ASF V2](https://github.com/JustArchi/ArchiSteamFarm/wiki/_Compilation-(ASF-V2))** page instead.

Compilation is the process of creating executable file. This is what you want to do if you want to add your own changes to ASF, or if you for whatever reason don't trust executable files provided in official **[releases](https://github.com/JustArchi/ArchiSteamFarm/releases)**. If you're user and not a developer, most likely you want to use already precompiled binaries, but if you'd like to use your own ones, or learn something new, continue reading.

ASF can be compiled on any currently supported platform, as long as you have all needed tools to do so.

---

## .NET Core SDK

Regardless of platform, you need full .NET Core SDK in order to compile ASF. Instructions can be found on **[.NET Core installation page](https://www.microsoft.com/net/core/preview)**. In any case, you must have `dotnet` command operative after installation. You can verify if it works with `dotnet --info`.

```
C:\Users\Archi>dotnet --info
.NET Command Line Tools (2.0.0-preview2-006497)

Product Information:
 Version:            2.0.0-preview2-006497
 Commit SHA-1 hash:  06a2093335

Runtime Environment:
 OS Name:     Windows
 OS Version:  10.0.15063
 OS Platform: Windows
 RID:         win10-x64
 Base Path:   C:\Program Files\dotnet\sdk\2.0.0-preview2-006497\

Microsoft .NET Core Shared Framework Host

  Version  : 2.0.0-preview2-25407-01
  Build    : 40c565230930ead58a50719c0ec799df77bddee9
```

---

## Unofficial compilation

Assuming you have .NET Core SDK operative and in appropriate version, simply navigate to ASF directory and execute:

```
dotnet restore
dotnet build -c "Release" -o "out"
```

If you're using Linux/OS X, you can instead use `cc.sh` script which will do the same, in a bit more complex manner.

If compilation ended successfully, you can find your ASF in `source` flavour in `ArchiSteamFarm/out` directory. This is the same flavour as `generic`, but it has forced `AutoUpdates` of `false` and `UpdateChannel` of `0`.

You can also generate OS-specific package if you have a specific need. In general you shouldn't do that if you have .NET Core SDK already installed, since you've just compiled `generic` flavour, but in case you want to:

```
dotnet publish -c "Release" -r "TARGET_RUNTIME" -o "out2"
```

Of course, replace `TARGET_RUNTIME` with OS-architecture you want to target, such as `win-x64`.

---

## Official compilation

Official ASF releases are compiled by **[AppVeyor](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)**, on Windows, with .NET Core SDK included in latest Visual Studio Preview.

Our releases are working with both .NET, as well as Mono, so **there is no need to compile yourself if you simply want to use ASF with Mono**, e.g. on Linux or OS X. You can simply download **[latest release](https://github.com/JustArchi/ArchiSteamFarm/releases/latest)** and run it with Mono right away.

---

# Components

ASF uses a few additional components for compilation process.

---

### ILRepack

ASF uses **[ILRepack](https://github.com/gluck/il-repack)** tool, which merges executable file and it's required libraries into one. ILRepack is launched only in ```Release``` builds, in ```PostBuildEvents```, which are declared in ```ArchiSteamFarm.csproj```. ILRepack is free and open-source too, but for convenience, it's provided in binary form in **[tools](https://github.com/JustArchi/ArchiSteamFarm/tree/master/tools)** directory, so it can be launched automatically after build finishes. You can compile and use it from source if you wish, or disable it by removing from ```PostBuildEvents```, if you for whatever reason don't want to use it.

---

### Third-party libraries

ASF also uses a few third-party libraries, which are crucial to make the program work. You can find all of them in **[packages](https://github.com/JustArchi/ArchiSteamFarm/tree/master/packages)** directory. Every third-party library is free and open-source as well, all of them are available on **[NuGet](https://www.nuget.org/)**. Again, for convenience, they're provided in binary form. You can compile and use them from source if you wish.