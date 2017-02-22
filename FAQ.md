# FAQ

***

# Table of contents

* [General](#general)
* [Idle Master comparison](#idle-master-comparison)
* [Security / Privacy / VAC / Bans / ToS](#security--privacy--vac--bans--tos)
* [Misc](#misc)
* [Issues](#issues)

***

## General

**Q:** So how it exactly works?

**A:** Before trying to understand what ASF is, you should make sure that you understand what Steam Cards are, and how to obtain them, which is nicely described in official FAQ **[here](http://steamcommunity.com/tradingcards/faq)**.

Core points are repeated once again here, because people are either too blind to see them, or don't want to see them:

- **Yes, you need to own the game in order to be eligible for any card drops from it. Family sharing doesn't count.**
- **No, you can't farm the game infinitely, every game has fixed number of card drops. Once you run out of cards to drop in given game, it's not a candidate for farming anymore.**
- **No, you can't drop cards from F2P games without spending any money in them.**
- **No, you can't drop cards on limited accounts anymore (those that never spent 5$ in steam store). It was possible in the past, but it's no longer the case.**

So as you can see, Steam cards are awarded to you for playing a game that you bought, or F2P game that you put money into. In other words, if you play a game long enough, all cards for that game will drop to your inventory, making it possible for you to complete a badge, sell them, or do whatever you want.

ASF as a program is quite complex to understand fully, so I'll skip technical details that should be available in **[Documentation](https://github.com/JustArchi/ArchiSteamFarm/wiki/Documentation)**, and offer a very simplified explanation.

ASF logs into your Steam account through built-in mini Steam Client using your provided credentials. After successfully logging in, it parses your **[badges](http://steamcommunity.com/my/badges)** in order to find games that are available for idling (You can get X more cards from playing this game). After parsing all pages and constructing final list of games that are available, ASF chooses most efficient farming algorithm and starts the process. The process depends upon chosen **[cards farming algorithm](https://github.com/JustArchi/ArchiSteamFarm/wiki/Performance)** but usually it consists of playing eligible game and periodically (plus on each item drop) checking if game is fully idled already - if yes, ASF can proceed with the next title, using the same procedure, until all games are fully farmed.

Keep in mind that explanation above is simplified and doesn't describe dozen of extra features and functions that ASF offers. Visit **[Configuration](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration)** and **[Documentation](https://github.com/JustArchi/ArchiSteamFarm/wiki/Documentation)** if you want to know every ASF detail. I tried to make it simple enough to understand for everybody, without bringing in technical details - advanced users are encouraged to dig deeper.

Now as a program - ASF offers some magic. Firsty, it doesn't have to download any of your game files, it can play games right away. Secondly, it's entirely independent of your normal Steam client - you don't need to have Steam client running or even installed at all. Thirdly, it's automated solution - which means that ASF automatically does everything behind your back, without a need of telling it what to do - which saves you hassle and time. Lastly, it doesn't have to trick Steam network by process emulation (which e.g. Idle Master is using), as it can communicate with it directly. It's also super fast and lightweight, being an amazing solution for everybody who wants to get cards easily without much hassle - it comes especially useful by leaving it running in the background while doing something else, or even playing in offline mode.

All of the above is nice, but ASF also has some technical limitations that are enforced by Steam - we can't idle games that you don't own, we can't idle games that are fully idled already, and we can't idle games while you're playing. All of that should be "logical", considering the way how ASF works, but it's nice to note that ASF doesn't have super-powers and won't do something that is physically impossible, so keep that in mind - it's basically the same as if you told someone to log in on your account from another PC and idle those games for you.

So to sum up - ASF is a program that helps you drop those cards you're eligible for, without much hassle. It also offers several other functions, but let's stick to this one for now.

***

**Q:** How long do I have to wait for cards to drop?

**A:** **As long as it takes** - seriously. Every game has different farming difficulty set by developer/publisher, and it's totally up to them how fast cards are being dropped. Majority of the games follow 1 drop per 30 minutes of playing, but there are also games requiring from you to play even several hours before dropping a card. Do not attempt to make guesses how long ASF should farm given title - it's not up to you, neither ASF to decide. There is nothing you can do to make it faster, and there is no "bug" related to cards not being dropped in timely fashion - you do not control cards dropping process, neither does ASF.

***

**Q:** Do I have to put my account credentials?

**A:** **Yes**. ASF requires your account credentials in the same way as official Steam client does, as it's using the same method for Steam network interaction. This however doesn't mean that you have to put your account credentials in ASF configs, you can keep using ASF with ```null```/empty ```SteamLogin``` and/or ```SteamPassword```, and input that data on each ASF run, when required. This way your credentials are not saved anywhere, but of course ASF can't autostart without your help. ASF also offers several other ways of increasing your **[Security](https://github.com/JustArchi/ArchiSteamFarm/wiki/Security)**, so feel free to read that part of the wiki if you're advanced user.

***

**Q:** Farming takes too long, can I somehow speed it up?

**A:** The only thing which heavily affects speed of farming is selected **[cards farming algorithm](https://github.com/JustArchi/ArchiSteamFarm/wiki/Performance)** for your bot instance. Everything else has negligible effect and will not make farming faster, while some actions such as launching ASF process several times will even **make it worse**. If you really have an urge of making every damn second from farming process, then ASF allows you to fine-tune some core farming variables such as ```FarmingDelay``` - all of them are explained in **[configuration](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration)**. However, as I said, the effect is negligible, and choosing proper cards farming algorithm for given account is one and the only crucial choice that can heavily affect speed of farming, everything else is pure cosmetic. Instead of worrying about farming speed, just launch ASF and let it do its job - I can assure you that it's doing it in the most effective way I could come up with. The less you care, the more you will be satisfied.

***

**Q:** But ASF said that farming will take about X time!

**A:** ASF gives you rough approximation based on number of cards you need to drop, and your chosen algorithm - this is nowhere close to the actual time that you will spend on farming, which is usually longer than this, as ASF assumes best case only, and ignores all Steam Network quirks, internet disconnections, overload of Steam servers and likewise. It should be seen only as a general indicator how long you can expect ASF to be farming, very often in best case, as actual time will differ, even significantly in some cases. Like pointed out above, do not try to guess how long given game will be farmed, it's not up to you, neither ASF to decide.

***

**Q:** Can ASF work on my android/smartphone?

**A:** ASF is a C# program and requires working implementation of .NET framework - either official one (Microsoft), or unofficial one (Mono). There is no Mono available for Android (natively), so ASF won't work on Android. It might be possible to port ASF to Android using **[Xamarin](https://www.xamarin.com/)**, but that's not on ASF roadmap and not being actively developed. Perhaps in future with progress of **[.NET Core](https://dotnet.github.io/)** it will be possible, but there are no plans for doing this at the moment.

However, keep in mind that Android is simply-put Unix on ARM platform. Mono supports ARM platform, so it **is** possible to run ASF on Android, by rooting your device, installing any Linux OS next to your Android for example with **[this](https://play.google.com/store/apps/details?id=ru.meefik.linuxdeploy)** application, then installing and running Mono as described in the wiki. It's not easy task, but if you have courage to try it, there is nothing really stopping you. ASF supports Mono on ARM, and it should work properly (even though it's not carefully tested).

***

**Q:** Can ASF idle items from Steam games, such as CS:GO or Unturned?

**A:** **No**, this is against Steam ToS and Valve clearly stated that with last wave of community bans for farming TF2 items. ASF is a Steam cards farming program, not game items farmer - it doesn't have any capability of farming game items, and it's not planned to add such feature in the future, ever, mainly because of violating Steam terms of use. Please do not ask about this - the best you can get is a report from some salty user and you having problems.

***

**Q:** I'm Linux / OS X user, will ASF idle games that are not available for my OS?

**A:** Yes, ASF is not even bothering with downloading actual game files, so it will work with all your licenses tied to your Steam account, regardless of any platform or technical requirements. It should also work for games tied to specific region (region-locked games) even when you're not in the matching region, although we didn't test this.

***

**Q:** I'm Linux / OS X user, do I need to compile ASF to use it?

**A:** No, binaries that are provided in official **[releases](https://github.com/JustArchi/ArchiSteamFarm/releases/latest)** are OS-independent, and work with both .NET framework (Windows), as well as Mono (Linux, OS X and other OSes). Of course, you **can** **[compile and use ASF from source](https://github.com/JustArchi/ArchiSteamFarm/wiki/Compilation)** if you have a reason to do so, but it's absolutely not required, neither recommended if you're typical user that simply wants to launch ASF.

***

## Idle Master comparison

***

**Q:** Is ASF faster than Idle Master?

**A:** **Yes**, although the explanation is rather complicated.

On each new process spawned and terminated on your system, steam client automatically sends a request containing all of your games that you're currently playing - this way steam network can calculate hours and make cards drop. However, steam network counts your time played in 1-second intervals, and sending new request resets the current status. In other words, if you did spawn/kill new process every 0.5 second, you'd never drop any card because every 0.5 second steam client would send a new request and steam network would never count even 1 second of playthrough. Moreover, because of how operating system works, it's actually quite common to see new processes being spawned/terminated without you even doing anything, so even if you're doing nothing on your PC - there are many processes still working in the background, spawning/terminating other processes all the time. Idle master is based on steam client, so this mechanism affects you if you're using it.

ASF is not based on steam client, it has its own steam client implementation. Thanks to that, what ASF is doing, is not spawning any process, but actually sending one, real request to steam network that we started playing a game. This is the same request that would be sent by steam client, but because we have actual control over the ASF steam client, we don't need to spawn new processes, and we're not mimicing steam client regarding send request on every process change, so the mechanism explained above doesn't affect us. Thanks to that, we never interrupt that 1 second interval on steam web side, and that enhances our farming speed.

***

**Q:** But is the difference really noticable?

**A:** No. The interrupts that are happening with normal steam client and idle master have negligible effect on the card drops, so it's not any noticable difference that would make ASF superior.

However, there **is** a difference, and you can clearly notice that, as depending on how busy your OS is, cards **will** drop faster, from a few seconds to even a few minutes, if you're extremely unlucky. Although I wouldn't consider using ASF only because it drops cards faster, as both ASF and Idle Master are affected by how steam web works, ASF just interacts with steam web more effectively, while Idle Master can't control what steam client is actually doing (so it's not Idle Master's fault, but steam client's itself).

***

**Q:** Can ASF idle multiple games at once?

**A:** **Yes**, although ASF knows better when to use that feature, based on selected **[cards farming algorithm](https://github.com/JustArchi/ArchiSteamFarm/wiki/Performance)**. You do not have direct choice on cards farming algorithm, but you can suggest ASF one, via setting config properties properly. You should focus on configuration part of the ASF, and let algorithms decide what is the most optimal way to achieve the goal.

***

**Q:** Can I play a game while ASF is farming?

**A:** **No**. ASF unlike IM has independent Steam client included, and Steam network allows only **one Steam client at a time** to play a game. You can however disconnect ASF any time you like by starting a game (and clicking "OK" when asked if Steam network should disconnect other client) - ASF will then patiently wait till you're done playing, and resume the process afterwards. Alternatively, you can still play in offline mode anytime you like, if that is satisfying for you.

Keep in mind that cards drop rate when playing multiple games is close to 0 anyway, therefore there are no direct benefits from being able to do that with IM, while there are strong benefits of no interfering with other games launched with ASF, which is crucial e.g. VAC-wise.

***

## Security / Privacy / VAC / Bans / ToS

***

**Q:** Can I get VAC ban for using this?

**A:** No, it's not possible because ASF (unlike Idle Master or SAM) does not interfere in any way with steam client nor its processes. It's physically impossible to get VAC ban for using ASF, even during playing on secured servers while ASF is running - this is because **ASF doesn't even require Steam Client being installed at all** in order to work properly. ASF is the only farming program that can currently guarantee being VAC-free.

***

**Q:** Is it safe?

**A:** If you ask if ASF is safe as a software, which means that it won't cause any damage to your computer, won't steal your private data, install viruses or any other stuff like that - it is safe. Code is open-source, and distributed binaries are always compiled from publicly available sources. If you for whatever reason don't trust them, you can always compile and use ASF from source, including all libraries that ASF is using (such as SteamKit2), which are open-source too. In the end however, it's always a matter of trust to the developer(s) of your application, so you should decide yourself if you consider ASF safe or not, potentially supporting your decision with technical arguments. Do not blindly believe something only because I said so - check yourself, as that's the only way to make sure.

***

**Q:** Can I get banned for this?

**A:** We should take a closer look at **[Steam ToS](http://store.steampowered.com/subscriber_agreement)**. Steam doesn't prohibit using of multiple accounts, in fact, **[it allows it](https://support.steampowered.com/kb_article.php?ref=8625-WRAH-9030#share)** implying that you can use same mobile authenticator on more than one account. What it however doesn't allow is sharing accounts with other people, but we're not doing that here.

The only real point that considers ASF is the following:
> You may not use Cheats, automation software (bots), mods, hacks, or any other unauthorized third-party software, to modify or automate any Subscription Marketplace process.

The question is what in fact is Subscription Marketplace process. As we can read:

> An example of a Subscription Marketplace is the Steam Community Market

We're not modifying or automating subscription marketplace process, if by subscription marketplace we understand steam community market or steam store. However...

> Valve may cancel your Account or any particular Subscription(s) at any time in the event that (a) Valve ceases providing such Subscriptions to similarly situated Subscribers generally, or (b) you breach any terms of this Agreement (including any Subscription Terms or Rules of Use).

Therefore, as with every Steam software. ASF is not authorized by Valve and I cannot guarantee that you won't be suspended if Valve suddenly decides that they're banning accounts using ASF now.

Especially:
> In regard to all Subscriptions, Contents and Services, that are not authored by Valve, Valve does not screen such third party content available on Steam or through other sources. Valve does not assume any responsibility or liability for such third party content. Some third party application software is capable of being used by businesses for business purposes - however, you may only acquire such software via Steam for private personal use.

ASF is licensed under Apache 2.0 License, which clearly states:

> Unless required by applicable law or agreed to in writing, ASF is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.

**TL;DR** - You're using this software at your own risk. It's very unlikely that you can get banned for that, but if you do, you can blame only yourself.

***

**Q:** Did somebody get banned for it?

**A:** Up to today, we had a single accident of guy with over 1000 bots getting trade banned (together with all bots), most likely due to excessive usage of ```!loot ASF``` executed on all bots at once, or other suspicious one-side amount of trades in very short time.

> Hello XXX,
> Thank you for contacting Steam Support.
> It looks like this account was used to manage a network of bot accounts.
> Botting is a violation of the Steam Subscriber Agreement.

Please, use some common sense and don't assume that you can do such crazy things only because ASF allows you to do that. Doing ```!loot ASF``` on over 1k of bots can be easily considered a **[DDoS](https://en.wikipedia.org/wiki/DDoS)** attack, and personally I'm not shocked that somebody got banned for such a thing. Please keep in mind some bare minimum of fair use in regards to Steam service, and _most likely_ you'll be fine.

_Most likely_, because ASF is just a tool and it's **your** decision how to use it. It can be a helper tool idling just one single account, or a massive farming network made from thousands of bots. In any of those cases, I'm not offering legal advice, and you should decide yourself how you want to use it. I'm not hiding any information that could help you, e.g. the fact that ASF got somebody trade-banned, as I have no reason to - it's your choice what you want to do with that information. If you ask me - use some common sense, and I doubt you'll get in trouble.

***

**Q:** What privacy information ASF discloses?

**A:** You can find detailed explanation in **[Statistics](https://github.com/JustArchi/ArchiSteamFarm/wiki/Statistics)** section. You should review it if you care about your privacy, e.g. if you're wondering why accounts being used in ASF are joining our Steam group.

***

## Misc

***

**Q:** I'm using Steam parental PIN to protect my account, do I need to input it somewhere?

**A:** Yes, you must set it in ```SteamParentalPIN``` bot config property. This is mainly because ASF does access many protected parts of your Steam account and it's impossible for ASF to operate without it.

***

**Q:** Can ASF farm steam cards obtained via Steam discovery during summer/winter sale?
 
**A:** **No**, that's out of the scope of the program and not planned to be added anytime soon.

***

**Q:** Why ASF-ConfigGenerator states version mismatch between ASF binary and CG binary?
 
**A:** Because ASF-ConfigGenerator intentionally doesn't include auto-update function, as auto-update is a feature of ASF, not CG. CG is not required for ASF to run, therefore it's not part of ASF auto-updates. If you attempt to use outdated CG with ASF, then it'll refuse to start as config structure might not match given ASF version, leading to broken/invalid configs being generated. It's not planned to add auto-update feature to CG, as CG functionality should be included together with ASF functionality in upcoming ASF-GUI binary, once it's finished. Until then, you need to update CG manually if you wish to use it.

***

**Q:** Can ASF minimize to tray?
 
**A:** ASF is a console app, there is no window to be minimized, because window is created for you by your OS. You can however use any third-party tool capable of doing so, such as **[RBTray](http://rbtray.sourceforge.net/)** for Windows, or **[screen](http://linux.die.net/man/1/screen)** for Linux/OS X. Those are only examples, there are many other apps with similar functionality.

***

**Q:** Does using ASF preserves eligibility for receiving booster packs?

**A:** **Yes**. ASF is using the same method to log in to Steam network as the official client, therefore it also preserves ability to receive booster packs for accounts that are being used. However, it's not confirmed if logging in to Steam community is actually required or not, therefore in order to be sure I suggest to keep ```FarmOffline``` on ```false```, at least until somebody confirms that using Steam network alone is enough. It should be, but I'm not sure.

***

**Q:** Is there any way to communicate with ASF?

**A:** Yes, through steam chat, or by using **[WCF](https://github.com/JustArchi/ArchiSteamFarm/wiki/WCF)**.

***

**Q:** I have only one (main) account added to ASF, can I still issue commands through steam chat?

**A:** **Yes**, it's explained in **[Commands](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands#notes)** section.

***

**Q:** ASF seems to be working, but I'm not receiving any card drops!

**A:** Cards farming rate differs from game to game, as you can read in **[Performance](https://github.com/JustArchi/ArchiSteamFarm/wiki/Performance)**. It takes a while, usually **several hours per game**, and you shouldn't expect cards to drop in a few minutes since launching a program. If you can see that ASF actively checks cards status, and switches the game after current one is fully idled, then everything works fine - you're probably referring to inventory notifications, which are automatically dismissed by ASF through ```DismissInventoryNotifications``` bot config property. Check out **[Configuration](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration)** for details.

***

**Q:** How many bots can I run with ASF?

**A:** ASF as a program doesn't have any upper limit of bot instances, however you're still being limited by Steam Network. Currently you can run up to 100-110 bots with single IP and single ASF instance. It is possible to run more bots with more IPs and more ASF instances. Keep in mind that if you're using that big amount of bots, you should control their number yourself (such as making sure that all of them in fact are logging in and working at the same time). Also notice that the limit above in general depends on many internal factors - it's approximation rather than strict limit - you will most likely be able to run more/less bots than specified above.

***

**Q:** Can I run more ASF instances then?

**A:** You can run as many ASF instances on one machine as you like, assuming every instance has its own directory and its own configs, and account used in one instance is not used in another one. However, ask yourself why you want to do that. ASF is optimized to handle a dozen, even a hundred of accounts at the same time, and launching those dozen of bots in their own ASF instances degrades performance, takes more OS resources, and causes lack of synchronization between bots - so for example you're more likely to hit ```InvalidPassword/RateLimitExceeded``` issue described below, as logging in requests are not being synchronized between ASF instances. Therefore, my **strong suggestion** is, always run maximum of one ASF instance per one IP/interface. If you have more IPs/interfaces, by all means you can run more ASF instances, every instance using its own IP/interface. If you don't, launching more ASF instances is totally pointless, and does not only degrade performance and takes more OS resources (such as memory), but also causes lack of synchronization and increased likehood of causing issues. You won't gain anything from launching more than 1 instance per a single IP/interface.

***

**Q:** What is the meaning of status when redeeming a key?

**A:** Status indicates how given redeem attempt turned out. There are many different statuses possible, most common ones include:

Status | Description
--- | ---
NoDetail | "OK" status indicating success - the key was successfully redemeed.
Timeout | Steam network didn't respond in given time, we don't know if the key was redeemed, or not (most likely not, try again).
BadActivationCode | The provided key is invalid (not recognized as any valid key by Steam network).
DuplicateActivationCode | The provided key was already redeemed by some other account.
AlreadyPurchased | Your account already owns ```packageID``` that is connected with this key. This does not indicate whether key is ```DuplicateActivationCode``` or not - only that it's valid.
RestrictedCountry | This is region-locked key and your account is not in the valid region that is permitted to redeem it.
DoesNotOwnRequiredApp | You can't redeem that key as you're missing some other app - mainly base game when you're attempting to redeem DLC package.
RateLimited | You made too many failed (anything but ```NoDetail```) attempts and your account was temporarily blocked. Try again in 30-60 minutes.

***

**Q:** Are you affiliated with getsteam.cards or any other cards farming service?

**A:** **No**. ASF is not affiliated with any service and all such claims are false. Your Steam account is your property and you can use your account in whatever way you wish, but Valve clearly stated in **[official ToS](http://store.steampowered.com/subscriber_agreement/english/)** that:

> Any use of your Account with your login and/or password is deemed made by you and you are responsible for it and for the security of your computer system

ASF is licensed on liberal Apache 2.0 License, which allows other developers to further integrate ASF with their own projects and services legally. However, such third-party projects utilizing ASF are not guaranteed to be secure, reviewed, appropriate or legal according to **[Steam ToS](http://store.steampowered.com/subscriber_agreement/english/)**. If you want to know our opinion, **we strongly encourage you to NOT share ANY of your account details with third-party services**. If such service turns out to be a **typical scam**, you'll be left alone with the problem, most likely without your Steam account and ASF won't take any responsibility for third-party services claiming to be safe and secure, because ASF team did not authorize neither reviewed any of those. In other words, **you're using them at your own risk, against our suggestion made above**.

In addition to that, official Steam ToS clearly states that:

> You may not reveal, share or otherwise allow others to use your password or Account except as otherwise specifically authorized by Valve

It's your account and your choice. Just don't say that nobody warned you. ASF as a program meets all rules mentioned above, as you're not sharing your account details with anyone, and you're using the program for your own personal use, but any other "cards farming service" does require from you your account credentials, so it also violates the rule above (actually several of them).

***

## Issues

***

**Q:** ```Unhandled Exception: System.MissingMethodException: Method not found```

**A:** This error indicates that your .NET framework doesn't support latest 4.6.1 instructions, as in - it's outdated. You should make sure that you're using latest .NET framework available for your platform, or latest Mono if you're using it instead. Head over to **[setting up](https://github.com/JustArchi/ArchiSteamFarm/wiki/Setting-up)** for more information.

***

**Q:** ```No bots are running, exiting```

**A:** Either you didn't **[configure](https://github.com/JustArchi/ArchiSteamFarm/wiki/Setting-up)** ASF, or you didn't enable any configured bot instance. When all bots exit, ASF will shutdown as well, as it has nothing to do (unless it's being run in ```--server``` **[mode](https://github.com/JustArchi/ArchiSteamFarm/wiki/Command-Line-Arguments)**).

***

**Q:** ```IsAnythingToFarm() Could not get badges information, will try again later!```

**A:** Usually it means that you're using Steam parental PIN to access your acount, yet you forgot to put it in ASF config. You must put valid PIN in ```SteamParentalPIN``` bot config property, otherwise ASF will not be able to access most of web content, therefore will not be able to work properly. Head over to **[Configuration](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration)** in order to learn more about ```SteamParentalPIN```.

Other reasons might include temporary Steam problem, network issue or likewise. If issue won't solve itself after several hours and you're sure that you configured ASF appropriately, feel free to let us know about that.

***

**Q:** ASF is failing with ```Request failed despite of 5 tries``` errors!

**A:** Usually it means that you're using Steam parental PIN to access your acount, yet you forgot to put it in ASF config. You must put valid PIN in ```SteamParentalPIN``` bot config property, otherwise ASF will not be able to access most of web content, therefore will not be able to work properly. Head over to **[Configuration](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration)** in order to learn more about ```SteamParentalPIN```.

If parental PIN is not the reason, then this is a most common error, and you should get used to that - it simply means that ASF sent a request to Steam Network, and didn't get a valid response, in addition to that - in 4 retries. Usually it means that Steam is either down or is having some difficulties or maintenance - ASF is aware of such issues and you should not worry about them, unless they're happening constantly for longer than several hours, and other users do not have such problems.

How to check if Steam is being down? **[Steam Status](https://steamstat.us/)** is an excellent source of checking if Steam **should be** up, if you notice errors, especially related to Community or Web API, then Steam is having difficulties, either leave ASF alone and let it do it's job after a short while, or wait yourself.

That's however not always the case, as in some situations Steam issues might not be detected by Steam Status, for example such case happened when Valve broke HTTPS support for Steam Community 7th June 2016 - accessing **[SteamCommunity](https://steamcommunity.com/)** through HTTPS was throwing an error. Therefore, do not blindly trust Steam Status either, it's best to check yourself if everything works as supposed to.

Lastly, if nothing helps you can always enable ```Debug``` mode and see yourself in ASF log why exactly requests are failing. For example, above HTTPS issue caused:

```
[!!] ERROR: UrlRequest() <patchy> Request: https://steamcommunity.com/my/inventory/json/753/6 failed!
[!!] ERROR: UrlRequest() <patchy> Status code: ServiceUnavailable
[!!] ERROR: UrlRequest() <patchy> Content:
<HTML><HEAD><TITLE>Error</TITLE></HEAD><BODY>
An error occurred while processing your request.<p>
```

Which is clearly Steam issue and nothing to fix in ASF. You can always try to visit the link mentioned by ASF yourself and check if it works - if it doesn't, then you know why ASF can't access that either. If it does, and error doesn't go away after an hour or two, it might be worth investigating and reporting.

***

**Q:** ASF seems to freeze and doesn't print anything on the console!

**A:** You're most likely using Windows and your console has QuickEdit mode enabled. Refer to **[this](http://stackoverflow.com/questions/30418886/how-and-why-does-quickedit-mode-in-command-prompt-freeze-applications)** question on StackOverflow for technical explanation. You should disable QuickEdit mode by right clicking your ASF console window, opening properties, and unchecking appropriate checkbox.

***

**Q:** ASF can't accept or send trades!

**A:** Obvious thing first - new accounts start as limited. Until you unlock account by loading it's wallet or spending 5$ in the store, account itself can't accept neither send trades. In this case, ASF will state that inventory seems empty, because every card that is in it is non-tradable. It also won't be possible to receive any trade, as that part requires ASF to be able to fetch API key, and API key functionality is disabled for limited accounts. In short - trading is off for all limited accounts, no exceptions.

Next, if you do not use **[ASF 2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)**, it's possible that ASF in fact accepted/sent trade, but you need to confirm it via your e-mail. Likewise, if you use classic 2FA, you need to confirm the trade via your authenticator. Confirmations are **mandatory** now, so if you don't want to accept them by yourself, consider either adding or importing your authenticator into ASF 2FA.

Also notice that you can trade only with your friends, and people with known trade link. If you're trying to initiate Bot->Master trade, such as ```!loot```, then you need to either have ```SteamMasterID``` on Bot's friendlist, or ```SteamTradeToken``` of ```SteamMasterID``` set in Bot's config. Make sure that the token is valid - otherwise, you won't be able to send a trade.

Lastly, remember that new devices have 7-days trade lock, so if you've just added your account to ASF, wait at least 7 days - everything should work after that period. That limitation includes **both** accepting **and** sending trades. It does not always trigger, and there are people who can send and accept trades instantly. Majority of the people are affected though, and the lock **will** happen, even if you can send and accept trades through your steam client on the same machine. Just wait patiently, there's nothing you can do to make it faster.

And finally, keep in mind that one account can have only 5 pending trades to another one, so ASF will fail to send trades if you have 5 (or more) pending ones from that one bot to accept already. This is rarily a problem, but it's also worth mentioning.

If nothing helped, you can always enable ```Debug``` mode and check yourself why requests are failing. Please note that Steam talks crap most of the time, and provided reason might not make any sense, or can be even entirely incorrect - if you decide to interpret that reason, make sure you have decent knowledge about Steam and it's quirks. It's also quite common to see that issue with no logical reason, and the only suggested solution in this case is to re-add account to ASF (and wait 7 days again). Sometimes this issue also fixes itself *magically*, the same way it breaks. However, usually it's just either 7-days trade lock, temporary steam problem, or both. It's best to give it a few days before manually checking what is wrong, unless you have some urge to debug the real cause (and usually you'll be forced to wait anyway, because error message won't make any sense, neither help you in the slightlest).

In any case, ASF can only **try** to send a proper request to Steam in order to accept/send trade. Whether Steam accepts that request, or not, is out of the scope of ASF, and ASF will not magically make it work. There's no bug related to that feature, and there is also nothing to improve, because logic is happening outside of ASF. Therefore, do not ask for fixing stuff that is not broken, and also do not ask why ASF can't accept or send trades - **I don't know, and ASF doesn't know either**. Either deal with it, or fix yourself 💀.

***

**Q:** Why do I have to put 2FA/SteamGuard code on each login? / ```Removed expired login key```

**A:** ASF uses login keys for keeping credentials valid, the same mechanism that Steam uses - 2FA/SteamGuard token is required only once. However, due to Steam fuckups and Steam network quirks, it's entirely possible that login key is not saved in the network, I've already seen such issues not only with ASF, but with regular steam client as well (a need to input login + password on each run, regardless of "remember me" option).

You could remove bot.db (+ bot.bin, if exists) of affected account and try to link ASF to your account once again, but that doesn't have to succeed. The real ASF-based solution is to import your authenticator as **[ASF 2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)** - this way ASF can generate tokens automatically when they're needed, and you don't have to input them manually. Usually the issue magically solves itself after some time, so you can simply wait for that to happen. Of course you can also ask GabeN for solution, because I can't force Steam network to accept our login keys.

***

**Q:** I'm getting error: ```Unable to login to Steam: InvalidPassword or RateLimitExceeded```

**A:** This error can mean a lot of things, some of them include:

- Invalid Login/Password combination (obviously)
- Expired login key used by ASF for logging in
- Too many failed login attempts in short period of time (anti-bruteforce)
- Too many login attempts in short period of time (rate-limiting)
- Requirement of captcha to log in (very likely to be caused by two reasons above)
- Any other reason Steam Network might have preventing you from logging in.

In case of anti-bruteforce and rate-limiting, problem will disappear after some time, so just wait and don't attempt to log in in the meantime. If you hit that issue frequently, perhaps it's wise to increase ```LoginLimiterDelay``` config property of ASF. Excessive program restarts and other intentional/non-intentional login requests definitely won't help with that issue, so try to avoid it if possible.

In case of expired login key - ASF will remove old one and ask for new one on next login (which will require from you putting 2FA token if your account is 2FA-protected. If your account is using ASF 2FA, token will be generated and used automatically). If you get this issue often, it's possible that Steam for some reason decided to ignore our login key save requests, as mentioned in issue above.

And lastly, if you used wrong login + password combination, obviously you need to correct this, or disable bot that is attempting to connect using those credentials. ASF can't guess on its own whether ```InvalidPassword``` means invalid credentials, or any of the reasons listed above, therefore it'll keep trying until it succeeds.

Keep in mind that ASF has its own built-in system to react accordingly to steam quirks, eventually it will connect and resume it's job, therefore it's not required to do anything if the issue is temporary. Restarting ASF in order to magically fix problems will only make things worse (as new ASF won't know previous ASF state of not being able to log in, and try to connect instead of waiting), so avoid doing that unless you know what you're doing.

Finally, as with every Steam request - ASF can only **try** to log in, using your provided credentials. Whether that request will succeed or not is out of the scope and logic of ASF, and nothing can be fixed neither improved in this regard.

***

**Q:** ASF is being detected by my AntiVirus (for example as: Win32/Fethar.B!cl or Win32/Zulushal.C!cl)

**A:** It's a false positive. ASF uses exe repacking method for bundling it together with all required libraries, which can trigger some heuristics mechanics in worse AVs. If you're worried about accuracy of previous statement, or you don't believe me, I suggest scanning ASF binary with many different AVs for actual detection ratio, for example through **[VirusTotal](https://virustotal.com/)** (or any other web service of your choice like this). A sample analysis result is available **[here](https://virustotal.com/file/87187de44baa063e6ac86c056be997d60dced1a22da07a58b91aaf650b80de18/analysis)**. If that happens to you, it's a good idea to send this file sample back to developers of your AV, so they can analyze it and improve their detection engine, as clearly it's not working as good as you think it does. If you want to solve the issue, either use another AV, add ASF to some kind of exceptions, or report ASF as false positive like stated above. Sadly, I'm used to AVs being stupid, as every once in a while some AV detects ASF as a virus, which usually lasts very short and is being patched up quickly by the devs, but it happened, happens and will happen all the time. I won't attempt to fight with this issue as it's pure nonsense - ASF doesn't include any malicious code, and if you don't trust my statement, I suggest to review ASF code yourself and compile from source, as only that guarantees you real safety.

***

**Q:** ```Bot database could not be loaded, refusing to start this bot instance!```

**A:** The above means that ```Bot.db``` of given bot instance is corrupted. Corruption can happen due to various reasons, including power outages, OS malfunctions, HDD problems and more. ASF can't magically fix your corrupted database, therefore you're required to fix it manually.

You can do that by either restoring your previously backuped ```Bot.db```, or removing corrupted ```Bot.db```, so ASF will recreate it from scratch. ASF doesn't recreate database from scratch automatically because it doesn't know if it's appropriate or not, which is especially true if you had ```ASF 2FA``` enabled for this account, as you most likely want to import it once again.

Until you fix the problem, given bot instance will be disabled and won't run, but it won't affect other configured bots functioning normally.