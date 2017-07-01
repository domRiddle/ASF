# ASF V3 Migration (WORK IN PROGRESS)

## Everything you should know about next major ASF version

---

## What is ASF V3 about?

ASF V3 is next major iteration of ASF that will come as a direct replacement of ASF V2. In a single TL;DR sentence it's about entire ASF code rewrite from **[.NET Framework](https://en.wikipedia.org/wiki/.NET_Framework)** 4.6.1 into **[.NET Core](https://www.microsoft.com/net/core/platform)** 2.0.

---

## But why?

The rewrite was justified with far more than "enough" reasons. The first and primary one was **[.NET Core itself](https://blogs.msdn.microsoft.com/dotnet/2016/09/26/introducing-net-standard/)** which Microsoft nicely documented, so go check that if you're interested in "technical/development" reasons.

However, apart from the above there is also another huge file of ASF-specific reasons. Here are some of them:

---

### Finally we're getting PROPER cross-OS support!

If you're using ASF on Windows then it might be shocking to you but you're a "second class" citizen. I tend to remind people from time to time that ASF was never coded for Windows - it was targetting running on Linux from the very start, and Windows support was always offered as "an extra". Thanks to official implementation of .NET Framework on Windows, as long as you were running ASF with supported .NET Framework then you couldn't run into runtime issues, you were all fine.

Situation didn't look that well on Linux where ASF was supposed to be run in the first place. If you ever tried to run ASF on anything else than Windows then you perfectly well what I'm talking about - Mono. Don't get me wrong, Mono folks did nothing less than awesome with porting a huge amount of .NET Framework onto Linux, thanks to them ASF was **even possible in the first place**. However, due to being unofficial and very hard to develop, it wasn't really rock solid most of the time, and it still isn't today. Sure, you can run ASF V2 just fine on Mono most of the time, but things like #529 are to be expected. I don't like how I have to require from everybody to always run latest Mono version and it can still not work correctly, be it because of platform, installation, repos being unavailable or likewise.

.NET Core solves that problem, and is at the same time **the most important reason** why we're switching in the first place - because finally there is a C# platform that is **officially supported AND tested** on Linux, and while Windows users shouldn't care that much about this reason, all Linux/OS X users will greatly appreciate ASF that finally works properly on their OS natively.

This reason has also many development implications that make my life much easier - finally I don't have to worry if given thing I'm implementing is working on Mono in the first place. Finally I won't need to play cat-and-mouse game of catching Mono bugs and working around them. Finally I can code as I should, and I don't need to constantly verify if perhaps Mono on OS X doesn't support feature X that works properly on Windows and Mono on Linux.

---

### .NET Core is open-source

Once again something people don't care about in general, but this has long-term side-effects that are crucial for future of ASF. Let's say it loudly - everything is slowly moving to .NET Standard and .NET Core. From small libraries to core ASF modules such as SteamKit2. Because of proper support, many developers are ditching .NET Framework and half-official support for Mono in favour of .NET Core. This means that the longer we stay with .NET Framework, the harder it will be to develop ASF. Case in point - SteamKit2 developers will probably drop support for Mono **entirely** after SK2 in version 2.0.0 gets released, which means that I won't only have to deal with Steam itself, but fix extra libraries on my own, spending even more time on ASF than I should. As they say, you either go with the flow, or you stay behind. I want the best for my project, so this decision is crucial for the future of the program.

---

### .NET Core is optimized

Try to guess how much memory ASF is using for running 3 bots in the process. Yes, the memory that ASF allocates for all connections, structures, keeping up with networking and everything else. Answer is: **4 MB**. No, there is no typo, 4 megabytes.

Of course, that is **managed** memory, aka what ASF truly allocates/uses. Entire process memory is made out of many other things such as runtime itself that is running ASF in the first place.

In comparison with Mono, ASF process can now take **up to 2x less memory than previously**, like in my case when ASF V2 used **7.9%** of memory, while ASF V3 for the same config uses as low as **4.4%**. I know that I'm probably some dinosaur that I even care about optimization of my program in 2017 (I'm looking at you, Unity), but it really feels awesome how tiny and fast ASF in fact is. People running ASF on low-memory VPSes with limited power should definitely appreciate more room for other services.

---

### When ASF V3 will be available?

Pre-releases should start becoming available around mid-July, with stable release depending on feedback and progress, sometime in August (mid-August planned, maybe earlier).