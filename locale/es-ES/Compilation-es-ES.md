# Compilación

La compilación es el proceso de crear un archivo ejecutable. Esto es lo que quieres hacer si quieres añadir tus propios cambios a ASF, o si por cualquier razón no confías en los archivos ejecutables proporcionados en las **[versiones](https://github.com/JustArchiNET/ArchiSteamFarm/releases)** oficiales. Si eres usuario y no desarrollador, lo más probable es que quieras usar ejecutables ya compilados, pero si quieres usar los tuyos propios, a aprender algo nuevo, continua leyendo.

ASF puede ser compilado en cualquier plataforma actualmente soportada, siempre que tengas todas las herramientas para hacerlo.

* * *

## .NET Core SDK

Independientemente de la plataforma, necesita .NET Core SDK completo (no solo runtime) para compilar ASF. Las instrucciones de instalación se pueden encontrar en la **[página de instalación de .NET Core](https://dotnet.microsoft.com/download)**. Necesitas instalar la versión apropiada de .NET Core SDK para tu sistema operativo. Después de una instalación exitosa, el comando `dotnet` debería estar funcionando y operativo. Puedes verificar si funciona con `dotnet --info`. También asegúrate de que tu .NET Core SDK coincida con los **[requisitos de runtime](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-es-es#requisitos-de-runtime)** de ASF.

* * *

## Compilación

Assuming you have .NET Core SDK operative and in appropriate version, simply navigate to source ASF directory (cloned or downloaded and unpacked ASF repo) and execute:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "netcoreapp2.2" -o "out/generic" "/p:LinkDuringPublish=false"
```

If you're using Linux/OS X, you can instead use `cc.sh` script which will do the same, in a bit more complex manner.

If compilation ended successfully, you can find your ASF in `source` flavour in `ArchiSteamFarm/out/generic` directory. This is the same as official `generic` ASF build, but it has forced `UpdateChannel` and `UpdatePeriod` of `0`.

### Sistema operativo específico

You can also generate OS-specific .NET Core package if you have a specific need. In general you shouldn't do that because you've just compiled `generic` flavour that you can run with your already-installed .NET Core runtime that you used for the compilation in the first place, but just in case you want to:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "netcoreapp2.2" -o "out/linux-x64" -r "linux-x64" "/p:CrossGenDuringPublish=false"
```

Of course, replace `linux-x64` with OS-architecture that you want to target, such as `win-x64`. This build will also have updates disabled.

### .NET Framework

In a very rare case when you'd want to build `generic-netf` package, you can change target framework from `netcoreapp2.2` to `net472`. Keep in mind that you'll need appropriate **[.NET Framework](https://dotnet.microsoft.com/download/visual-studio-sdks)** developer pack for compiling `netf` variant, in addition to .NET Core SDK, so the below will work only on Windows:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net472" -o "out/generic-netf"
```

In case of being unable to install .NET Framework or even .NET Core SDK itself (e.g. because of building on `linux-x86` with `mono`), you can call `msbuild` directly. You'll also need to specify `ASFNetFramework` manually, as ASF by default disables netf build on non-Windows platforms:

```shell
msbuild /m /p:Configuration=Release /p:PublishDir=out/generic-netf /p:TargetFramework=net472 /p:ASFNetFramework=true /r /t:Publish ArchiSteamFarm
```

* * *

## Desarrollo

If you'd like to edit ASF code, you can use any .NET Core compatible IDE for that purpose, although even that is optional, since you can as well edit with a notepad and compile with `dotnet` command described above. Still, for Windows we recommend **[latest Visual Studio](https://visualstudio.microsoft.com/downloads)** (free community version is more than enough). We also suggest to use it together with **[ReSharper](https://www.jetbrains.com/resharper)** (optionally), although it's not a free product.

If you'd like to work with ASF code on Linux/OS X instead, we recommend **[latest Visual Studio Code](https://code.visualstudio.com/download)**. It's not as rich as classic Visual Studio, but it's good enough.

Of course all suggestions above are only recommendations, you can use whatever you want to, it comes down to `dotnet build` command anyway. We use Visual Studio + ReSharper for ASF development, with a small part of third-party `tools` that you can find in the repo.

* * *

## Etiquetas

`master` branch is not guaranteed to be in a state that allows successful compilation or flawless ASF execution in the first place, since it's development branch just like stated in our **[release cycle](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle)**. If you want to compile ASF from source, then you should use appropriate **[tag](https://github.com/JustArchiNET/ArchiSteamFarm/tags)** for that purpose, which guarantees at least successful compilation, and very likely also flawless execution (if build was marked as stable release). In order to check the current "health" of the tree, you can use our CIs - **[AppVeyor](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)** or **[Travis](https://travis-ci.com/JustArchiNET/ArchiSteamFarm)**.

* * *

## Versiones oficiales

Official ASF releases are compiled by **[AppVeyor](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)** on Windows, with latest .NET Core SDK that matches ASF **[runtime requirements](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)**. After passing tests, all packages are deployed on GitHub. This also guarantees transparency, since AppVeyor always uses official public source for all builds, and you can compare checksums of AppVeyor artifacts with GitHub assets. ASF developers do not compile or publish builds themselves, except for private development process and debugging.