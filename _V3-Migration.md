# ASF V3 Migration

## Everything you should know about next major ASF version

![:3](http://1.bp.blogspot.com/-w1LcChYTikU/UMDTX33sKYI/AAAAAAAABcI/wgc3YGotuD4/s1600/ohgodhow.jpg)

---

## What is ASF V3 about?

ASF V3 is next major iteration of ASF that will come as a direct replacement of ASF V2.

In a single TL;DR sentence it's about entire ASF code rewrite from **[.NET Framework](https://en.wikipedia.org/wiki/.NET_Framework)** 4.6.1 into **[.NET Core](https://www.microsoft.com/net/core/platform)** 2.0.

---

## But why?

The rewrite was justified with a far more than "enough" reasons. The first and primary one was **[.NET Core itself](https://blogs.msdn.microsoft.com/dotnet/2014/12/04/introducing-net-core/)** with further **[extra](https://blogs.msdn.microsoft.com/dotnet/2016/09/26/introducing-net-standard/)** that Microsoft nicely documented, so go check that if you're interested in "technical/development" reasons. There is no need for me to explain what .NET Core is if Microsoft folks did that much better.

However, apart from the above there is also another huge file of ASF-specific reasons that I decided to document myself. Here are some of them.

---

### Finally we're getting PROPER cross-OS support!

If you're using ASF on Windows then it might be shocking to you but you're a "second class" citizen. I tend to remind people from time to time that ASF was never coded for Windows - it was targetting running on Linux from the very start, and Windows support was always offered as "an extra" to Linux build. However, thanks to official implementation of .NET Framework on Windows, as long as you were running ASF with supported .NET Framework then you couldn't run into any runtime issues - you had flawless experience.

Situation didn't look that well on Linux where ASF was supposed to be run in the first place. If you ever tried to run ASF on anything else than Windows then you know perfectly well what I'm talking about - Mono. Don't get me wrong, Mono folks did nothing less than awesome with porting a huge amount of .NET Framework onto Linux, thanks to them ASF was **even possible in the first place**. However, due to being unofficial and very hard to develop, it wasn't really rock solid most of the time, and it still isn't today. Sure, you can run ASF V2 just fine on Mono most of the time, but things like **[#529](https://github.com/JustArchi/ArchiSteamFarm/issues/529)** are to be expected. I don't like how I have to require from everybody to always run latest Mono version and it can still not work correctly, be it because of platform, installation, repos being unavailable or likewise.

.NET Core solves that problem, and at the same time that is **the most important reason** why we're switching in the first place - because finally there is a C# platform that is **officially supported AND tested** on Linux, and while Windows users shouldn't care that much about this reason, all Linux/OS X users will greatly appreciate ASF that finally works properly on their OS natively - you can remove Mono entirely, ASF V3 no longer depends on it.

This reason has also many development implications that make my life much easier - finally I don't have to worry if given thing I'm implementing is working on Mono in the first place. Finally I won't need to play cat-and-mouse game of catching Mono bugs and working around them. Finally I can code as I should, and I don't need to constantly verify if perhaps Mono on OS X doesn't support feature X that works properly on Windows and Mono on Linux.

---

### .NET Core is open-source

Once again something people don't care about in general, but this has long-term side-effects that are crucial for future of ASF. Let's say it loudly - everything is slowly moving to .NET Standard and .NET Core. From small libraries to core ASF modules such as SteamKit2. Because of proper support, many developers are ditching .NET Framework and half-official support for Mono in favour of .NET Core. This means that the longer we stay with .NET Framework, the harder it will be to develop ASF. Case in point - **[SteamKit2 developers will probably drop support for Mono entirely](https://github.com/SteamRE/SteamKit/pull/385#issuecomment-294330501)** after SK2 in version 2.0.0 gets released, which means that I won't only have to deal with Steam itself, but fix extra libraries on my own, spending even more time on ASF than I should. As they say, you either go with the flow, or you stay behind. I want the best for my project, so this decision is crucial for the future of the program.

Another more "idealistic" view on the entire situation is obvious - I love open-source, this is why ASF is open-source, all its libraries are open-source and you can use it **for free**. We had open-source Mono for a while now, but we were still stuck with proprietary .NET Framework on Windows and many of its parts being unavailable/half-baked on Mono.

.NET Core once again solves this problem entirely - it's fully open-source and possible to build on any platform, which compared to .NET Framework, will probably result in major community gathering around it, with me being the prime example.

---

### .NET Core is optimized

Try to guess how much memory ASF is using for running 3 bots in the process. Yes, the memory that ASF allocates for all connections, structures, keeping up with networking and everything else. Answer is: **4 MB**. No, there is no typo, 4 megabytes.

Of course, that is **managed** memory, aka what ASF truly allocates/uses. Entire process memory is made out of many other things such as runtime itself that is running ASF in the first place. It's not a lot, but end result together with some "room" for ASF actions is around 40-100 MB of process memory usage.

In comparison with Mono, ASF process can now take **up to 2x less memory than previously**, like in my case when ASF V2 used **7.9%** of memory, while ASF V3 for the same config uses as low as **4.4%**. I know that I'm probably some dinosaur that I even care about optimization of my program in 2017 (I'm looking at you, Unity), but it really feels awesome how tiny and fast ASF in fact is. People running ASF on low-memory VPSes with limited power should definitely appreciate more room for other services, while everybody else will appreciate the fact that ASF is even more lightweight, even faster and even more reliable than before. Proprietary .NET Framework was fast, Mono was only a bit slower, .NET Core is faster than both. It could as well be the fastest and most optimized platform that ever existed, but I don't really have time neither willings to compare, so the fact that it's **faster than anything we had before** while **far more lightweight** is more than enough for me.

---

### .NET Core is the future

Regardless how you look at it, Microsoft dedicated a lot of money, time, people and support into their new platform. After all they've decided to rewrite platform that they've been working on **for the last 15 years**. This is not some side-project done in a few weeks, it's probably one of the biggest coding efforts that happened in the last decade, if not since Windows 95 times. It has **A LOT** of more potential than .NET Framework which Microsoft is slowly dropping in favour of .NET Core. The amount of people, support, help, development and entire positivity is really huge and it's clear as crystal that .NET Core is the most important thing when it comes to ASF future as a project. It's not a common thing for me to report a bug, add reference to it in ASF and in a few hours later learn that it's already fixed **[in my own ASF issue](https://github.com/JustArchi/ArchiSteamFarm/issues/586#issuecomment-313132484)** that was blocked by it 💓.

I want the best for ASF, and .NET Core is probably the best thing that happened for ASF since its first 0.1 version until this day. It's like major upgrade of that old server that was handling thousands of requests hourly - it can get only better, not worse.

---

## Enough, what changed? Do I need to start from scratch again?

Luckily for you - **not at all!**

In terms of technical differences, this change is a very huge step forward, but in terms of practical differences ASF V3 mainly changed **low-level things**, those that are often implementation details and not necessary for you to touch in the first place.

Of course, there are some changes that are important to mention, here is the list of changes I consider important, as well as what to do with them.

---

### Packaging changed

ASF V2 had very complex packaging that resulted in a very nice structure I liked a lot - a single EXE file to "rule them all".

Sadly, existing packaging tools are incompatible with .NET Core and it's unknown if we ever get to the point of being able to produce a single binary. Because of that, ASF folder is now a huge "mess" of executables, libraries, localization folders, runtime configurations and more. Of course it's as organized as possible, but it's no longer a single binary - it's entire folder with a lot of dependencies that were nicely packed previously.

ASF also comes in 2 main flavours now - generic package and OS-specific.

Generic package works similar to how ASF V2 worked for Linux - you had `ASF.exe` file that you were supposed to run with `mono`. In ASF V3 generic flavour you get `ArchiSteamFarm.dll` library that you're supposed to run with `dotnet`. This setup requires from you to have .NET Core already installed on your OS **in appropriate version**. We all know how troublesome it was to keep .NET Framework/Mono up-to-date, therefore this package is here mainly for people that **already use** .NET Core and don't want to duplicate their runtime solely for ASF if they can make use of what they have installed already. I do not recommend using generic flavour if you're casual or even advanced user that just want to make ASF work and not dig into .NET Core technical details. In other words - if you know what this is, use it, otherwise just use OS-specific package explained below.

OS-specific package works similar to how ASF V2 worked for Windows - you get a single binary that serves as a "starting point" and should be started. In comparison with generic package above, OS-specific package **already includes proper .NET Core runtime inside**, which allows you to entirely skip the whole installation mess and just launch ASF directly, yes, **also on Linux**, where binary is typical Unix ELF executable. OS-specific package, as you can guess from the name, is OS-specific and every OS requires its own version. ASF V3 is planned to be released in 4 OS-specific flavours: `win-x64`, `linux-x64`, `linux-arm` (e.g. Raspberry Pi 2+) and `osx-x64`, with more possibly to come in the future.

Of course, even if you don't have OS-specific package available for your OS-architecture combination, you can always install appropriate .NET Core runtime yourself and run generic ASF flavour, which is also the main reason why it exists in the first place. Generic ASF build is platform-agnostic and will run on any platform that has a working .NET Core runtime. This is important to note - ASF requires .NET Core 2.0+, not some specific OS or architecture. For example, if you're running 32-bit Windows then despite of no dedicated `win-x86` ASF version, you can still install .NET Core 2.0 in `win-x86` version and run generic ASF just fine. If you want to read more, head over to **[compatibility](https://github.com/JustArchi/ArchiSteamFarm/wiki/Compatibility)** section.

---

### Auto-update was rewritten

This is not a change that considers you as an user specifically, but entire auto-update code was rewritten and might contain new bugs or issues that are yet to be discovered and corrected. In practice, ASF update works exactly the same as before, but now it updates whole structure (see packaging changes above), which is much more problematic than a single exe only.

Of course, your config directory, like previously, is never touched by update process (apart from `example.json` and `minimal.json` files that are now appropriately updated too).

---

### Config-Generator was rewritten

As funny as it can sound, cross-OS GUI (graphical user interface) is not available in .NET Core in any form yet. This means that old ConfigGenerator can't be ported to .NET Core, so I either could force you to **still** use old .NET Framework/Mono one, or rewrite entire thing in some other multi-platform technology, as CG always was independent of ASF and optional.

Of course I decided to go with second option and together with my friend **[@Aareksio](https://github.com/Aareksio)** we coded for you brand new browser-based ConfigGenerator, mainly in javascript. It can now be used for generating valid ASF V3 configs easily from your favourite browser, simplifying things a lot and allowing for future improvements.

In order to use the new ConfigGenerator, simply navigate to its **[page](https://justarchi.github.io/ArchiSteamFarm/)**. From there you can easily generate config files compatible with ASF V3.

Apart from much more user-friendly structure, there is now proper support for versioning so CG is never fixed to one specific version - you can use it for generating proper configs for any ASF version from V3 upwards.

CG source is available **[here](https://github.com/JustArchi/ArchiSteamFarm/tree/gh-pages)** and any improvements are welcome. We'll also try to improve it a bit as the time goes.

---

### IPC was rewritten / WCF is dead

Server-based WCF is not available in .NET Core, and it doesn't look like being considered anytime soon. Luckily for you, this doesn't mean that ASF `--server` option is going anywhere. WCF was **implementation detail** of how ASF handled **inter-process communication**. Because of being implementation detail, it was simply rewritten from being `WCF-based` into being `Http-based`. `WCFHostname` and `WCFPort` were changed into `IPCHostname` and `IPCPort`, while `WCFBinding` was removed.

When you start ASF V3 in `--server` mode, ASF now creates special http listener listening on `http://127.0.0.1:1242/IPC` address (or whatever else you put as `IPCHostname` and `IPCPort`). It's even easier to use it now, since executing command is as easy as launching `http://127.0.0.1:1242/IPC?command=version` in your favourite browser. Because of that, `--client` was entirely removed as being now-useless since you can access the link in your favourite way now, be it via browser or `curl`.

Please note that while new IPC works fine, it's rather low-level and not really suitable for building a full ASF API over it. That's why basically you can only call commands with it. It's very likely that it'll be rewritten once again at some point, when there will be enough interest and a good idea how we can improve it further. For now it "just works".

As a side-note, this change will also break compatibility with anything that depended on WCF previously, such as **[ASFui](https://www.steamgifts.com/discussion/eT97I/asfui-archisteamfarm-user-interface-asf-gui)**. Of course all developers that made use of ASF WCF in their apps previously can very easily adapt their apps to use http-based IPC now, but it requires some changes nonetheless.

---

### ASF-Service is dead

Windows services are not supported in .NET Core and it doesn't look like they're coming anytime soon. While I like the fact that ASF could be installed as a service on Windows, it was never the primary objective of ASF and was added in **[#268](https://github.com/JustArchi/ArchiSteamFarm/pull/268)** by one of the interested developers.

Sadly it's no longer available and it's the only ASF V3 change that has no real workaround, as this feature is simply non-existent. If it ever comes back, for sure it'll be implemented, but for now I suggest to use ASF as usual, many services-based things such as auto-start or auto-restart can be easily enabled without ASF-Service in the first place. I know that it's more troublesome, but sadly I don't have a good solution for those few folks that actually made use of it 🙁.

---

### `ProtectedDataForCurrentUser` is available only on Windows

If you previously used `ProtectedDataForCurrentUser` `PasswordFormat` on Mono, then sadly you must "downgrade" yourself, for example to `AES`. This feature is available only on Windows OS for now, and you'll get a `PlatformNotSupportedException` if you try to use it on Linux or OS X.

It's unknown for now whether this feature will come to Linux or OS X in the future, but very likely **[it will happen](https://github.com/NuGet/Home/issues/1851)** sooner or later. For now, just stick with `AES`.

---

## Complete migration guide

ASF V2 **could** in theory upgrade itself to ASF V3 automatically, as there are no breaking changes in terms of configs or general usage, but intentionally this **won't happen**, as V3 is a major replacement and I want everybody to carefully proceed with a manual upgrade in order to ensure that everything works fine for them.

Luckily for you, like pointed above, installation/migration guide is very easy. Just follow points below:

- Install **[.NET Core prerequisites](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)** for your machine. For Windows this is just Visual C++ Redistributable, for other OSes this is a set of standard libraries. Very likely you might have majority or even all requirements already installed.
- Download OS-specific ASF package for your machine, such as `win-x64` for 64-bit Windows.
- Unpack downloaded archive into new location (and `chmod +x ArchiSteamFarm` if you're on Linux/OS X).
- Copy entire `config` directory from old ASF V2 location into new ASF V3 location, overwriting existing files.
- Run `ArchiSteamFarm(.exe)`, confirm that everything works, and delete your old ASF V2 from its location.

If you need/want to regenerate your configs, feel free to use our new **[ConfigGenerator](https://github.com/JustArchi/ArchiSteamFarm/wiki/V3-Migration#config-generator-was-rewritten)**. And if you require more detailed explanation, visit **[setting up](https://github.com/JustArchi/ArchiSteamFarm/wiki/Setting-up)**.

---

## Can I still use ASF V2?

Of course you can, same as you can still use ASF V1 or even ASF V0. Releases on GitHub are frozen and will never disappear, so it's always possible to fallback to any ever released ASF version, both binary-wise and source-wise.

However, keep in mind that ASF V2, like previously V1 and V0 **will no longer receive updates or support**. Even if it still works right now, nothing guarantees you that Valve won't bring another Steam breaking change soon that will affect one or more of ASF functionalities. **We do not recommend using obsolete ASF versions**, the sooner you switch, the smoother the process will be with less issues, hassle and stuff to learn. There is also no guarantee that e.g. old ASF V2 configs will still work in one of the future ASF V3 versions, so if you want our advice - it's best to upgrade right away to new stable and don't look back. The upgrade process involves almost no effort, and even could be done automatically if we didn't want you to read this article.

---

## Video

For people that prefer watching over reading. Huge thanks to **[@GamingTaylor](https://www.youtube.com/channel/UCTjrsQgjZmBzYzWaAh0zI3Q)** for recording up-to-date video about ASF!

### **[YouTube](https://www.youtube.com/watch?v=gi2UjXtGWgc)**