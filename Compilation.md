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

## Compilation

Assuming you have .NET Core SDK operative and in appropriate version, simply navigate to ASF directory and execute:

```
dotnet restore
dotnet build -c "Release" -o "out"
```

If you're using Linux/OS X, you can instead use `cc.sh` script which will do the same, in a bit more complex manner.

If compilation ended successfully, you can find your ASF in `generic` flavour in `ArchiSteamFarm/out` directory. This is the same as official ASF build, but it has forced `AutoUpdates` of `false` and `UpdateChannel` of `0`.

You can also generate OS-specific package if you have a specific need. In general you shouldn't do that if you have .NET Core SDK already installed, since you've just compiled `generic` flavour, but in case you want to:

```
dotnet publish -c "Release" -r "TARGET_RUNTIME" -o "out2"
```

Of course, replace `TARGET_RUNTIME` with OS-architecture you want to target, such as `win-x64`. This build will also have updates disabled.

---

## Official releases

Official ASF releases are compiled by **[AppVeyor](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)** on Windows, with .NET Core SDK included in latest Visual Studio Preview. After passing tests, they're deployed on GitHub.