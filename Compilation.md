# Compilation

Compilation is the process of creating executable file. This is what you want to do if you want to add your own changes to ASF, or if you for whatever reason don't trust executable files provided in official **[releases](https://github.com/JustArchi/ArchiSteamFarm/releases)**. If you're user and not a developer, most likely you want to use already precompiled binaries, but if you'd like to use your own ones, or learn something new, continue reading.

ASF can be compiled on any currently supported platform, as long as you have all needed tools to do so.

---

## .NET Core SDK

Regardless of platform, you need full .NET Core SDK in order to compile ASF. Installation instructions can be found on **[.NET Core installation page](https://www.microsoft.com/net/download)**. You need to install appropriate .NET Core SDK version for your OS. After successful installation, `dotnet` command should be working and operative. You can verify if it works with `dotnet --info`. Also ensure that your .NET Core SDK matches ASF **[runtime requirements](https://github.com/JustArchi/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)**.

Example of `dotnet --info` on Windows:

```powershell
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

```shell
dotnet publish ArchiSteamFarm -c "Release" -o "out"
```

If you're using Linux/OS X, you can instead use `cc.sh` script which will do the same, in a bit more complex manner.

If compilation ended successfully, you can find your ASF in `source` flavour in `ArchiSteamFarm/out` directory. This is the same as official `generic` ASF build, but it has forced `UpdateChannel` and `UpdatePeriod` of `0`.

You can also generate OS-specific package if you have a specific need. In general you shouldn't do that because you've just compiled `generic` flavour that you can run with your already-installed .NET Core runtime that you used for the compilation in the first place, but just in case you want to:

```shell
dotnet publish ArchiSteamFarm -c "Release" -o "out2" -r "linux-x64"
```

Of course, replace `linux-x64` with OS-architecture you want to target, such as `win-x64`. This build will also have updates disabled.

---

## Development

If you'd like to edit ASF code, you can use any .NET Core compatible IDE for that purpose, although even that is optional, since you can as well edit with a notepad and compile with `dotnet` command described above. Still, for Windows we recommend **[latest Visual Studio](https://www.visualstudio.com/downloads)** (free community version is more than enough). We also heavily recommend to load it with **[ReSharper](https://www.jetbrains.com/resharper)**, although it's not a free product.

If you'd like to work with ASF code on Linux/OS X instead, we recommend **[latest Visual Studio Code](https://code.visualstudio.com/download)**. It's not as rich as classic Visual Studio, but it's good enough.

Of course all suggestions above are only recommendations, you can use whatever you want to, it comes down to `dotnet build` command anyway. Originally we use Visual Studio + ReSharper for ASF development, with a small part of third-party `tools` that you can find in the repo.

---

## Tags

`master` branch is not guaranteed to be in a state that allows successful compilation or flawless ASF execution in the first place, since it's development branch just like stated in our **[release cycle](https://github.com/JustArchi/ArchiSteamFarm/wiki/Release-cycle)**. If you want to compile ASF from source, then you should use appropriate **[tag](https://github.com/JustArchi/ArchiSteamFarm/tags)** for that purpose, which guarantees at least successful compilation, and very likely also flawless execution (if build was marked as stable release).

---

## Official releases

Official ASF releases are compiled by **[AppVeyor](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)** on Windows, with latest .NET Core SDK that matches ASF **[runtime requirements](https://github.com/JustArchi/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)**. After passing tests, generic and OS-specific packages are deployed on GitHub. This also guarantees transparency, since AppVeyor always uses official public source for all builds, and you can compare checksums of AppVeyor artifacts with GitHub assets. ASF developers do not compile or publish builds manually, except for private development process, including debugging.