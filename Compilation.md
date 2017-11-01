# Compilation

Compilation is the process of creating executable file. This is what you want to do if you want to add your own changes to ASF, or if you for whatever reason don't trust executable files provided in official **[releases](https://github.com/JustArchi/ArchiSteamFarm/releases)**. If you're user and not a developer, most likely you want to use already precompiled binaries, but if you'd like to use your own ones, or learn something new, continue reading.

ASF can be compiled on any currently supported platform, as long as you have all needed tools to do so.

---

## .NET Core SDK

Regardless of platform, you need full .NET Core SDK in order to compile ASF. Installation instructions can be found on **[.NET Core installation page](https://www.microsoft.com/net/learn/get-started)**. You need to install appropriate .NET Core SDK version for your OS. After successful installation, `dotnet` command should be working and operative. You can verify if it works with `dotnet --info`. Also ensure that your .NET Core SDK matches ASF **[runtime requirements](https://github.com/JustArchi/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)**.

Example of `dotnet --info` on Windows:

```
C:\Users\Archi>dotnet --info
Product Information:
 Version:            2.0.0
 Commit SHA-1 hash:  cdcd1928c9

Runtime Environment:
 OS Name:     Windows
 OS Version:  10.0.15063
 OS Platform: Windows
 RID:         win10-x64
 Base Path:   C:\Program Files\dotnet\sdk\2.0.0\

Microsoft .NET Core Shared Framework Host

  Version  : 2.0.0
  Build    : e8b8861ac7faf042c87a5c2f9f2d04c98b69f28d
```

---

## Compilation

Assuming you have .NET Core SDK operative and in appropriate version, simply navigate to ASF directory and execute:

```
dotnet build -c "Release" -o "out"
```

If you're using Linux/OS X, you can instead use `cc.sh` script which will do the same, in a bit more complex manner.

If compilation ended successfully, you can find your ASF in `generic` flavour in `ArchiSteamFarm/out` directory. This is the same as official ASF build, but it has forced `AutoUpdates` of `false` and `UpdateChannel` of `0`.

You can also generate OS-specific package if you have a specific need. In general you shouldn't do that if you have .NET Core SDK already installed, since you've just compiled `generic` flavour, but in case you want to:

```
dotnet publish -c "Release" -r "linux-x64" -o "out2"
```

Of course, replace `linux-x64` with OS-architecture you want to target, such as `win-x64`. This build will also have updates disabled.

---

## Official releases

Official ASF releases are compiled by **[AppVeyor](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)** on Windows, with latest .NET Core SDK that matches ASF **[runtime requirements](https://github.com/JustArchi/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)**. After passing tests, generic and OS-specific packages are deployed on GitHub.