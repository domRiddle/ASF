# Compilation

Compilation is the process of creating executable file. This is what you want to do if you want to add your own changes to ASF, or if you for whatever reason don't trust executable files provided in official **[releases](https://github.com/JustArchi/ArchiSteamFarm/releases)**. If you're user and not a developer, most likely you want to use already precompiled binaries, but if you'd like to use your own ones, or learn something new, continue reading.

ASF can be compiled on any currently supported platform, as long as you have all needed tools to do so.

---

## .NET Core SDK

Regardless of platform, you need full .NET Core SDK (not just runtime) in order to compile ASF. Installation instructions can be found on **[.NET Core installation page](https://www.microsoft.com/net/download)**. You need to install appropriate .NET Core SDK version for your OS. After successful installation, `dotnet` command should be working and operative. You can verify if it works with `dotnet --info`. Also ensure that your .NET Core SDK matches ASF **[runtime requirements](https://github.com/JustArchi/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)**.

Example of `dotnet --info` on Windows:

```
.NET Core SDK (reflecting any global.json):
 Version:   2.1.300
 Commit:    adab45bf0c

Runtime Environment:
 OS Name:     Windows
 OS Version:  10.0.17134
 OS Platform: Windows
 RID:         win10-x64
 Base Path:   C:\Program Files\dotnet\sdk\2.1.300\

Host (useful for support):
  Version: 2.1.0
  Commit:  caa7b7e2ba

.NET Core SDKs installed:
  2.1.300 [C:\Program Files\dotnet\sdk]

.NET Core runtimes installed:
  Microsoft.AspNetCore.All 2.1.0 [C:\Program Files\dotnet\shared\Microsoft.AspNetCore.All]
  Microsoft.AspNetCore.App 2.1.0 [C:\Program Files\dotnet\shared\Microsoft.AspNetCore.App]
  Microsoft.NETCore.App 2.1.0 [C:\Program Files\dotnet\shared\Microsoft.NETCore.App]
```

---

## Compilation

Assuming you have .NET Core SDK operative and in appropriate version, simply navigate to ASF directory and execute:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "netcoreapp2.1" -o "out-generic"
```

If you're using Linux/OS X, you can instead use `cc.sh` script which will do the same, in a bit more complex manner.

If compilation ended successfully, you can find your ASF in `source` flavour in `ArchiSteamFarm/out-generic` directory. This is the same as official `generic` ASF build, but it has forced `UpdateChannel` and `UpdatePeriod` of `0`.

### OS-specific

You can also generate OS-specific .NET Core package if you have a specific need. In general you shouldn't do that because you've just compiled `generic` flavour that you can run with your already-installed .NET Core runtime that you used for the compilation in the first place, but just in case you want to:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "netcoreapp2.1" -o "out-linux-x64" -r "linux-x64"
```

Of course, replace `linux-x64` with OS-architecture you want to target, such as `win-x64`. This build will also have updates disabled.

### .NET Framework

In a very rare case when you'd want to build `generic-netf` package, you can change target framework from `netcoreapp2.1` to `net472`. Keep in mind that you'll need appropriate **[.NET Framework](https://www.microsoft.com/net/download/visual-studio-sdks)** developer pack for compiling `netf` variant, in addition to .NET Core SDK.

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net472" -o "out-generic-netf"
```

In even more rare case, if you can't install .NET Framework or even .NET Core SDK itself (e.g. because of building on `linux-x86` with `mono`), you can call `msbuild` directly:

```shell
msbuild /m /p:Configuration=Release /p:PublishDir=out-generic-netf /p:TargetFramework=net472 /r /t:Publish ArchiSteamFarm
```

---

## Development

If you'd like to edit ASF code, you can use any .NET Core compatible IDE for that purpose, although even that is optional, since you can as well edit with a notepad and compile with `dotnet` command described above. Still, for Windows we recommend **[latest Visual Studio](https://www.visualstudio.com/downloads)** (free community version is more than enough). We also suggest to use it together with **[ReSharper](https://www.jetbrains.com/resharper)**, although it's not a free product.

If you'd like to work with ASF code on Linux/OS X instead, we recommend **[latest Visual Studio Code](https://code.visualstudio.com/download)**. It's not as rich as classic Visual Studio, but it's good enough.

Of course all suggestions above are only recommendations, you can use whatever you want to, it comes down to `dotnet build` command anyway. We use Visual Studio + ReSharper for ASF development, with a small part of third-party `tools` that you can find in the repo.

---

## Tags

`master` branch is not guaranteed to be in a state that allows successful compilation or flawless ASF execution in the first place, since it's development branch just like stated in our **[release cycle](https://github.com/JustArchi/ArchiSteamFarm/wiki/Release-cycle)**. If you want to compile ASF from source, then you should use appropriate **[tag](https://github.com/JustArchi/ArchiSteamFarm/tags)** for that purpose, which guarantees at least successful compilation, and very likely also flawless execution (if build was marked as stable release).

---

## Official releases

Official ASF releases are compiled by **[AppVeyor](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)** on Windows, with latest .NET Core SDK that matches ASF **[runtime requirements](https://github.com/JustArchi/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)**. After passing tests, generic and OS-specific packages are deployed on GitHub. This also guarantees transparency, since AppVeyor always uses official public source for all builds, and you can compare checksums of AppVeyor artifacts with GitHub assets. ASF developers do not compile or publish builds manually, except for private development process, including debugging.