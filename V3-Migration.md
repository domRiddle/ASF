# ASF V3 Migration (WORK IN PROGRESS)

## Everything you should know about next major ASF version

![:3](http://1.bp.blogspot.com/-w1LcChYTikU/UMDTX33sKYI/AAAAAAAABcI/wgc3YGotuD4/s1600/ohgodhow.jpg)

---

## What is ASF V3 about?

ASF V3 is next major iteration of ASF that will come as a direct replacement of ASF V2.

In a single TL;DR sentence it's about entire ASF code rewrite from **[.NET Framework](https://en.wikipedia.org/wiki/.NET_Framework)** 4.6.1 into **[.NET Core](https://www.microsoft.com/net/core/platform)** 2.0.

---

## But why?

The rewrite was justified with far more than "enough" reasons. The first and primary one was **[.NET Core itself](https://blogs.msdn.microsoft.com/dotnet/2014/12/04/introducing-net-core/)** with further **[extra](https://blogs.msdn.microsoft.com/dotnet/2016/09/26/introducing-net-standard/)** that Microsoft nicely documented, so go check that if you're interested in "technical/development" reasons. There is no reason for me to explain what .NET Core is if Microsoft folks did that much better than me.

However, apart from the above there is also another huge file of ASF-specific reasons. Here are some of them.

---

### Finally we're getting PROPER cross-OS support!

If you're using ASF on Windows then it might be shocking to you but you're a "second class" citizen. I tend to remind people from time to time that ASF was never coded for Windows - it was targetting running on Linux from the very start, and Windows support was always offered as "an extra". Thanks to official implementation of .NET Framework on Windows, as long as you were running ASF with supported .NET Framework then you couldn't run into runtime issues, you were all fine.

Situation didn't look that well on Linux where ASF was supposed to be run in the first place. If you ever tried to run ASF on anything else than Windows then you know perfectly well what I'm talking about - Mono. Don't get me wrong, Mono folks did nothing less than awesome with porting a huge amount of .NET Framework onto Linux, thanks to them ASF was **even possible in the first place**. However, due to being unofficial and very hard to develop, it wasn't really rock solid most of the time, and it still isn't today. Sure, you can run ASF V2 just fine on Mono most of the time, but things like **[#529](https://github.com/JustArchi/ArchiSteamFarm/issues/529)** are to be expected. I don't like how I have to require from everybody to always run latest Mono version and it can still not work correctly, be it because of platform, installation, repos being unavailable or likewise.

.NET Core solves that problem, and at the same time that is **the most important reason** why we're switching in the first place - because finally there is a C# platform that is **officially supported AND tested** on Linux, and while Windows users shouldn't care that much about this reason, all Linux/OS X users will greatly appreciate ASF that finally works properly on their OS natively.

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

Regardless how you look at it, Microsoft dedicated a lot of money, time, people and support into their new platform. After all they've decided to rewrite platform that they've been working on **for the last 15 years**. This is not some side-project done in a few weeks, it's probably one of the biggest coding efforts that happened in the last decade, if not since Windows 95 times. It has **A LOT** of more potential than .NET Framework which Microsoft is slowly dropping in favour of .NET Core. The amount of people, support, help, development and entire positivity is really huge and it's clear as crystal that .NET Core is the most important thing when it comes to ASF future as a project.

I want the best for ASF, and .NET Core is probably the best thing that happened for ASF since its first 0.1 version until this day. It's like major upgrade of that old server that was handling thousands of requests hourly - it can get only better, not worse.

---

## Enough, what changed? Do I need to start from scratch again?

Luckily for you - **not at all!**

In terms of differences, this change is a very huge step forward, but in terms of practical differences ASF V3 mainly changed **low-level things**, those that are often implementation details and not necessary for you to touch in the first place.

Of course, there are some changes that are important to mention, here is the list of changes I consider important, as well as what to do with them.

---

### Packaging changed

ASF V2 had very complex packaging that resulted in a very nice structure I liked a lot - a single EXE file to "rule them all".

Sadly, existing packaging tools are incompatible with .NET Core and it's unknown if we ever get to the point of being able to produce a single binary. Because of that, ASF folder is now a huge "mess" of executables, libraries, localization folders, runtime configurations and more. Of course it's as organized as possible, but it's no longer a single binary - it's entire folder with a lot of dependencies that were nicely packed previously.

ASF also comes in 2 main flavours now - generic package and OS-specific.

Generic package works similar to how ASF V2 worked for Linux - you had `ASF.exe` file that you were supposed to run with `mono`. In ASF V3 generic flavour you get `ArchiSteamFarm.dll` library that you're supposed to run with `dotnet`. This setup requires from you to have .NET Core already installed on your OS **in appropriate version**. We all know how troublesome it was to keep .NET Framework/Mono up-to-date, therefore this package is here mainly for people that **already use** .NET Core and don't want to duplicate their runtime solely for ASF if they can make use of what they have installed already. I do not recommend using generic flavour if you're casual or even advanced user that just want to make ASF work and not dig into .NET Core technical details. In other words - if you know what this is, use it, otherwise just use OS-specific package explained below.

OS-specific package works similar to how ASF V2 worked for Windows - you get a single binary that serves as a "starting point" and should be started. In comparison with generic package above, OS-specific package **already includes proper .NET Core runtime inside**, which allows you to entirely skip the whole installation mess and just launch ASF directly, yes, **also on Linux**, where binary is typical Unix ELF executable. OS-specific package, as you can guess from the name, is OS-specific and every OS requires its own version. ASF V3.0.0.0 is planned to be released in 3 OS-specific flavours: `win-x64`, `linux-x64` and `osx-x64`, with more possibly to come in the future.

Of course, even if you don't have OS-specific package available for your OS-architecture combination, you can always install appropriate .NET Core runtime yourself and run generic ASF flavour, which is also the main reason why it exists in the first place.

---

### Auto-update was rewritten

This is not a change that considers you as an user specifically, but entire auto-update code was rewritten and might contain new bugs or issues that are yet to be discovered and corrected. In practice, ASF update works exactly the same as before, but now it updates whole structure (see packaging changes above), which is much more problematic than a single exe only.

Of course, your config directory, like previously, is never touched by update process (apart from `example.json` and `minimal.json` files that are now appropriately updated too).

---

### Config-Generator was rewritten

As funny as it can sound, cross-OS GUI (graphical user interface) is not available in .NET Core in any form yet. This means that old ConfigGenerator can't be ported to .NET Core, so I either could force you to **still** use old .NET Framework/Mono one, or rewrite entire thing in some other multi-platform technology, as CG always was independent of ASF and optional.

Of course I decided to go with second option and together with my friend **[@Aareksio](https://github.com/Aareksio)** we coded for you brand new browser-based ConfigGenerator, mainly in javascript. It can now be used for generating valid ASF V3 configs easily from your favourite browser, simplifying things a lot and allowing for future improvements.

// TODO: JS-based CG is still work-in-progress, it'll be here once its available

---

### IPC was rewritten / WCF is dead

Server-based WCF is not available in .NET Core, and it doesn't look like being considered anytime soon. Luckily for you, this doesn't mean that ASF `--server` option is going anywhere. WCF was **implementation detail** of how ASF handled **inter-process communication**. Because of being implementation detail, it was simply rewritten from being `WCF-based` into being `Http-based`. `WCFHostname` and `WCFPort` were changed into `IPCHostname` and `IPCPort`, while `WCFBinding` was removed.

When you start ASF V3 in `--server` mode, ASF now creates special http listener listening on `http://127.0.0.1:1242/IPC` address (or whatever else you put as `IPCHostname` and `IPCPort`). It's even easier to use it now, since executing command is as easy as launching `http://127.0.0.1:1242/IPC?command=version` in your favourite browser. Because of that, `--client` was entirely removed as being now-useless since you can access the link in your favourite way now, be it via browser or `curl`.

Please note that while new IPC works fine, it's rather low-level and not really suitable for building a full ASF API over it. That's why basically you can only call commands with it. It's very likely that it'll be rewritten once again at some point, when there will be enough interest and a good idea how we can improve it further. For now it "just works".

As a side-note, this change will also break compatibility with anything that depended on WCF previously, such as **[ASFui](https://www.steamgifts.com/discussion/eT97I/asfui-archisteamfarm-user-interface-asf-gui)**. Of course all developers that made use of ASF WCF in their apps previously can very easily adapt their apps to use http-based IPC now, but it requires some changes nonetheless.

---

### ASF-Service is dead

Windows services are not supported in .NET Core and it doesn't look like they're coming anytime soon. While I like the fact that ASF could be installed as a service on Windows, it was never the primary objective of ASF and was added in **[#268](https://github.com/JustArchi/ArchiSteamFarm/pull/268)** by one of the interested developers.

Sadly it's no longer available and it's the only ASF V3 change that has no real workaround, as this feature is simply not available. If it ever comes back, for sure it'll be implemented, but for now I suggest to use ASF as usual, many services-based things such as auto-start or auto-restart can be easily enabled without ASF-Service in the first place. I know that it's more troublesome, but sadly I don't have a good solution for those few folks that actually made use of it 🙁.

---

### `ProtectedDataForCurrentUser` is available only on Windows

If you previously used `ProtectedDataForCurrentUser` `PasswordFormat` on Mono, then sadly you must "downgrade" yourself, for example to `AES`. This feature is available only on Windows OS for now, and you'll get a `PlatformNotSupportedException` if you try to use it on Linux or OS X.

It's unknown for now whether this feature will come to Linux or OS X in the future, but very likely **[it will happen](https://github.com/NuGet/Home/issues/1851)** sooner or later. For now, just stick with `AES`.

---

## When ASF V3 will be available?

Pre-releases should start becoming available around mid-July, with stable release depending on feedback and progress, sometime in August (mid-August planned, maybe earlier).