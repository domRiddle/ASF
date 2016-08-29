# Compilation

Compilation is the process of creating executable file. This is what you want to do if you want to add your own changes to ASF, or if you for whatever reason don't trust executable files provided in official **[releases](https://github.com/JustArchi/ArchiSteamFarm/releases)**. If you're user and not a developer, most likely you want to use already precompiled binaries, but if you'd like to use your own ones, or learn something new, continue reading.

ASF can be compiled on any currently supported platform, as long as you have all needed tools to do so.

---

## Windows

On Windows I suggest to use **[latest Visual Studio](https://www.visualstudio.com/products/visual-studio-community-vs.aspx)**, which is now free. After installing all components, you should launch **ArchiSteamFarm.sln** file in VS, switch to ```Release``` target, then hit ```Build``` -> ```Build Solution```.

Alternatively, you can do the same from the console, by navigating to ASF directory and executing:

```
msbuild /p:Configuration=Release ArchiSteamFarm.sln
```

Of course, you need ```msbuild``` in your ```PATH```.

---

## Linux / Mono

Compilation on Mono-powered OSes is even easier in my opinion. Firstly you should make sure that you have **[latest Mono installed](https://github.com/JustArchi/ArchiSteamFarm/wiki/Mono)**. If you do, all you need to do is navigating to the directory where ASF source is located, then executing:

```
xbuild /p:Configuration=Release ArchiSteamFarm.sln
```

You can also use ```cc.sh``` script, which simplifies things even further.

```
./cc.sh
```

---

If everything ended successfully, you can find your compiled binaries in ```bin``` directory of each project, and in addition to that you can find repacked ready-to-go binaries with appropriate structure in ```out``` directory. You can use either ```ArchiSteamFarm.exe``` with all required DLL libraries, or repacked ```ASF.exe``` which already contains all of them inside. If you're not shipping your binaries and you just want to use ASF from source tree, I highly recommend launching ```ArchiSteamFarm.exe``` instead of ```ASF.exe```, simply because it's smaller, more optimized, can load DLLs on as-needed basis and doesn't automatically update to the version found on GitHub.

ASF by default is compiled on Windows - with latest Visual Studio, as Mono-powered builds may not work correctly on non-Mono platforms, so if you decide to compile with Mono, make sure that you're running output binary with Mono as well.

---

# Components

ASF uses a few additional components for compilation process.

---

### ILRepack

ASF uses **[ILRepack](https://github.com/gluck/il-repack)** tool, which merges executable file and it's required libraries into one. ILRepack is launched only in ```Release``` builds, in ```PostBuildEvents```, which are declared in ```ArchiSteamFarm.csproj```. ILRepack is free and open-source too, but for convenience, it's provided in binary form in **[tools](https://github.com/JustArchi/ArchiSteamFarm/tree/master/tools)** directory, so it can be launched automatically after build finishes. You can compile and use it from source if you wish, or disable it by removing from ```PostBuildEvents```, if you for whatever reason don't want to use it.

---

### Third-party libraries

ASF also uses a few third-party libraries, which are crucial to make the program work. You can find all of them in **[packages](https://github.com/JustArchi/ArchiSteamFarm/tree/master/packages)** directory. Every third-party library is free and open-source as well, all of them are available on **[NuGet](https://www.nuget.org/)**. Again, for convenience, they're provided in binary form. You can compile and use them from source if you wish.