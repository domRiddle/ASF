# Compilation

Compilation is the process of creating executable file. This is what you want to do if you want to add your own changes to ASF, or if you for whatever reason don't trust executable files provided in official **[releases](https://github.com/JustArchi/ArchiSteamFarm/releases)**. If you're user and not a developer, most likely you want to use already precompiled binaries, but if you'd like to use your own ones, or learn something new, continue reading.

ASF can be compiled on any currently supported platform, as long as you have all needed tools to do so.

---

## Windows

On Windows I suggest to use **[latest Visual Studio](https://www.visualstudio.com/products/visual-studio-community-vs.aspx)**, which is now free. After installing all components, you should launch **ArchiSteamFarm.sln** file in VS, switch to ```Release``` target, then hit ```Build``` -> ```Build ArchiSteamFarm```.

---

## Linux / Mono

Compilation on Mono-powered OSes is even easier in my opinion. Firstly you should make sure that you have **[latest Mono installed](https://github.com/JustArchi/ArchiSteamFarm/wiki/Mono)**. If you do, all you need to do is navigating to the directory where ASF source is located, then executing:

```xbuild /p:Configuration=Release```

---

If everything ended successfully, you can find your compiled binaries in ```bin``` directory.