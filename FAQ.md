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

**A:** Before trying to understand what ASF is, you should make sure that you understand what Steam Cards are, which is nicely described in official FAQ **[here](http://steamcommunity.com/tradingcards/faq)**.

ASF as a program is quite complex and full technical explanation should be available in **[Documentation](https://github.com/JustArchi/ArchiSteamFarm/wiki/Documentation)**. Much simplified explanation is easier to understand: ASF logs into your Steam account through built-in mini Steam Client using your provided credentials. After successfully logging in, it parses your **[badges](http://steamcommunity.com/my/badges)** in order to find games that are available for farming (You can get X more cards from playing this game). After parsing all pages and constructing final list of games that are available for farming, ASF chooses most efficient farming algorithm and starts the process. The process depends upon chosen **[cards farming algorithm](https://github.com/JustArchi/ArchiSteamFarm/wiki/Performance)** but usually it contains of playing eligible game and periodically (plus on each item drop) checking if game is fully farmed already - if yes, ASF can proceed with the next title, until all games are fully farmed.

Keep in mind that explanation above is simplified and doesn't describe dozen of extra features and functions that ASF offers. Visit **[Configuration](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration)** and **[Documentation](https://github.com/JustArchi/ArchiSteamFarm/wiki/Documentation)** if you want to know every ASF detail.

ASF still fully depends on Steam Network - **it can't farm games that account doesn't own, it can't farm games that are fully farmed already, and it can't farm games while you're playing**. However ASF also includes some magic - it doesn't require Steam Client running or even being installed, it doesn't require downloading games that should be farmed, and it doesn't have to trick Steam Client by process emulation (which e.g. Idle Master is using), as it can communicate with Steam Network directly. On top of all of that, it's automated solution, which means that it doesn't need any human interaction at all, apart from initial setup (configs) and providing crucial data during runtime (SteamGuard/2FA). Therefore, it's excellent tool to leave running in the background while doing something else, or even playing in offline mode.

***

**Q:** How long do I have to wait for cards to drop?

**A:** **As long as it takes** - seriously. Every game has different farming difficulty set by developer/publisher, and it's totally up to them how fast cards are being dropped. Majority of the games follow 1 drop per 30 minutes of playing, but there are also games requiring from you to play even several hours before dropping a card. Do not attempt to make guesses how long ASF should farm given title - it's not up to you, neither ASF to decide.

***

**Q:** Farming takes too long, can I somehow speed it up?

**A:** The only thing which heavily affects speed of farming is selected **[cards farming algorithm](https://github.com/JustArchi/ArchiSteamFarm/wiki/Performance)** for your bot instance. Everything else has negligible effect and will not make farming faster, while some actions such as launching ASF process several times will even **make it worse**. If you really have an urge of making every damn second from farming process, then ASF allows you to fine-tune some core farming variables such as ```FarmingDelay``` - all of them are explained in **[configuration](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration)**. However, as I said, the effect is negligible, and choosing proper cards farming algorithm for given account is one and the only crucial choice that can heavily affect speed of farming, everything else is pure cosmetic. Instead of worrying about farming speed, just launch ASF and let it do it's job - I can assure you that it's doing it in the most effective way I could come up with :+1:.

***

**Q:** Can ASF work on my android?

**A:** ASF is a C# program and requires working implementation of .NET framework - either official one (Microsoft), or unofficial one (Mono). There is no Mono available for Android (natively), so ASF won't work on Android. It might be possible to port ASF to Android using **[Xamarin](https://www.xamarin.com/)**, but that's not on ASF roadmap and not being actively developed. Perhaps in future with progress of **[.NET Core](https://dotnet.github.io/)** it will be possible, but there are no plans for doing this at the moment.

However, keep in mind that Android is simply-put Unix on ARM platform. Mono supports ARM platform, so it **is** possible to run ASF on Android, by rooting your device, installing any Linux OS next to your Android for example with **[this](https://play.google.com/store/apps/details?id=ru.meefik.linuxdeploy)** application, then installing and running Mono as described in the wiki. It's not easy task, but if you have courage to try it, there is nothing really stopping you. ASF supports Mono on ARM, and it should work properly (even though it's not carefully tested).

***

## Idle Master comparison

***

**Q:** Is ASF faster than Idle Master?

**A:** **Yes**, although the explanation is rather complicated.

On each new process spawned and terminated on your system, steam client automatically sends a request containing all of your games that you're currently playing - this way steam network can calculate hours and make cards drop. However, steam network counts your time played in 1-second intervals, and sending new request resets the current status. In other words, if you did spawn/kill new process every 0.5 second, you'd never drop any card because every 0.5 second steam client would send a new request and steam network would never count even 1 second of playthrough. Moreover, because of how operating system works, it's actually quite common to see new processes being spawned/terminated without you even doing anything, so even if you're doing nothing on your PC - there are many processes still working in the background, spawning/terminating other processes all the time. Idle master is based on steam client, so this mechanism affects you if you're using it.

ASF is not based on steam client, it has it's own steam client implementation. Thanks to that, what ASF is doing, is not spawning any process, but actually sending one, real request to steam network that we started playing a game. This is the same request that would be sent by steam client, but because we have actual control over the ASF steam client, we don't need to spawn new processes, and we're not mimicing steam client regarding send request on every process change, so the mechanism explained above doesn't affect us. Thanks to that, we never interrupt that 1 second interval on steam web side, and that enhances our farming speed.

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

**A:** No, it's not possible because ASF (unlike Idle Master or SAM) does not interfere in any way with steam client nor it's processes. It's physically impossible to get VAC ban for using ASF, even during playing on secured servers while ASF is running - this is because **ASF doesn't even require Steam Client being installed at all** in order to work properly. ASF is the only farming program that can currently guarantee being VAC-free.

***

**Q:** Is it safe?

**A:** If you ask if ASF is safe as a software, which means that it won't cause any damage to your computer, won't steal your private data, install viruses or any other stuff like that - it is safe. Code is open-source, and distributed binaries are always compiled from publicly available sources. If you for whatever reason don't trust them, you can always compile and use ASF from source, including all libraries that ASF is using (such as SteamKit2), which are open-source too. In the end however, it's always a matter of trust to the developer(s) of your application, so you should decide yourself if you consider ASF safe or not, potentially supporting your decision with technical arguments.

***

**Q:** Can I get banned for this?

**A:** We must take a closer look at **[Steam ToS](http://store.steampowered.com/subscriber_agreement)**. Steam doesn't prohibit using of multiple accounts, in fact, **[it allows it](https://support.steampowered.com/kb_article.php?ref=8625-WRAH-9030#share)** implying that you can use same mobile authenticator on more than one account. What it however doesn't allow is sharing accounts with other people, but we're not doing that here.

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

**A:** Up to today, I didn't hear of any single case where ASF would be directly responsible for any kind of steam suspension. Therefore, we can assume that Valve either doesn't care, or like I pointed above - ASF doesn't violate any terms of service. Valve didn't state whether they agree with what ASF (or IdleMaster) is doing, or not. Therefore, you should consider both programs as grey zone, and decide yourself if you're fine with using ASF or not. If you ask me, you should be perfectly fine, but nobody can guarantee that, as written above. Therefore, in the end, it's your decision, and nobody's else.

***

**Q:** What privacy information ASF discloses?

**A:** ASF in default settings has ```Statistics``` option enabled. Statistics help ASF developers by providing them with information crucial to development cycle. If you want to see new versions coming up, bugs being fixed, and new features getting implemented, you should consider keeping them on. Currently statistics include only very minimalistic information - accounts being used by ASF will automatically join our **[steam group](http://steamcommunity.com/groups/ascfarm)**. This is done for three reasons - first one, to provide **you** with group announcements, especially new versions, critical issues, steam problems and other things that are important to keep community updated. Secondly, it allows **you** to use our technical support, by asking questions, resolving problems, reporting issues or suggesting improvements. Lastly - it allows ASF developers to get some minimal data about usage of the application, especially number of actual users. Statistics are available for everyone, so you can check yourself how our steam group looks like or how many members it has. We're not gathering any other information, especially sensitive ones such as passwords, steam logins or even operating system you're using, and by keeping ```Statistics``` enabled you help ASF developers - which is **bare minimum** what you can (and should) do as user. It's still possible to disable even this minimalistic information, by switching ```Statistics``` to off in ASF configuration, as we respect your decision, but consider twice if you don't want to even tell us that you're ASF user - this decision might affect not only actual work being done on improving the program, but also receiving updates or support from ASF team. We leave you with a choice, and disclosing whole ASF activity, unlike many other programs that silently behind your back are selling your sensitive information to third-parties, so we hope that you also respect that we need bare minimal information to provide you with a better application. In the end - decision is upon you.

***

## Misc

***

**Q:** Can ASF minimize to tray?
 
**A:** ASF is a console app, there is no window to be minimized, because window is created for you by your OS. You can however use any third-party tool capable of doing so, such as **[RBTray](http://rbtray.sourceforge.net/)** for Windows, or **[screen](http://linux.die.net/man/1/screen)** for Linux/OS X. Those are only examples, there are many other apps with similar functionality.

***

**Q:** I have a question regarding ASF 2FA...

**A:** Then check **[ASF 2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)** wiki page.

***

**Q:** Is there any way to communicate with ASF?

**A:** Yes, through steam chat, or by using **[WCF](https://github.com/JustArchi/ArchiSteamFarm/wiki/WCF)**.

***

**Q:** ASF seems to be working, but I'm not receiving any card drops!

**A:** Cards farming rate differs from game to game, as you can read in **[Performance](https://github.com/JustArchi/ArchiSteamFarm/wiki/Performance)**. It takes a while and you shouldn't expect cards to drop in a few minutes since launching a program. If you can see that ASF switches the game in the console, then everything works fine - you're probably referring to inventory notifications, which are automatically dismissed by ASF through ```DismissInventoryNotifications``` bot config property. Check out **[Configuration](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration)** for details.

***

**Q:** How many bots can I run with ASF?

**A:** ASF as a program doesn't have any upper limit of bot instances, however you're still being limited by Steam Network. Currently you can run up to 100-110 bots with single IP and single ASF instance. It is possible to run more bots with more IPs and more ASF instances. Keep in mind that if you're using that big amount of bots, you should control their number yourself (such as making sure that all of them in fact are logging in and working at the same time). Also notice that the limit above in general depends on many internal factors - it's approximation rather than strict limit - you will most likely be able to run more/less bots than specified above.

***

**Q:** Can I run more ASF instances then?

**A:** You can run as many ASF instances on one machine as you like, assuming every instance has it's own directory and it's own configs, and account used in one instance is not used in another one. However, ask yourself why you want to do that. ASF is optimized to handle a dozen, even a hundred of accounts at the same time, and launching those dozen of bots in their own ASF instances degrades performance, takes more OS resources, and causes lack of synchronization between bots - so for example you're more likely to hit ```InvalidPassword``` issue described below, as logging in requests are not being synchronized between ASF instances. Therefore, my **strong suggestion** is, always run maximum of one ASF instance per one IP/interface. If you have more IPs/interfaces, by all means you can run more ASF instances, every instance using it's own IP/interface. If you don't, launching more ASF instances is totally pointless, and does not only degrade performance and takes more OS resources (such as memory), but also causes lack of synchronization and increased likehood of causing issues.

***

**Q:** Are you affiliated with getsteam.cards or any other cards farming service?

**A:** **No**. ASF is not affiliated with any service and all such claims are false. Your Steam account is your property and you can use your account in whatever way you wish, but Valve clearly stated in **[official ToS](http://store.steampowered.com/subscriber_agreement/english/)** that:

> Any use of your Account with your login and/or password is deemed made by you and you are responsible for it and for the security of your computer system

ASF is licensed on liberal Apache 2.0 License, which allows other developers to further integrate ASF with their own projects and services legally. However, such third-party projects utilizing ASF are not guaranteed to be secure, reviewed, appropriate or legal according to **[Steam ToS](http://store.steampowered.com/subscriber_agreement/english/)**. If you want to know our opinion, **we strongly encourage you to NOT share ANY of your account details with third-party services**. If such service turns out to be a **typical scam**, you'll be left alone with the problem, most likely without your Steam account and ASF won't take any responsibility for third-party services claiming to be safe and secure, because ASF team did not authorize neither reviewed any of those. In other words, **you're using them at your own risk, against our suggestion made above**.

> You may not reveal, share or otherwise allow others to use your password or Account except as otherwise specifically authorized by Valve

Your account, your choice. Just don't say that nobody warned you.

***

## Issues

***

**Q:** ```Unhandled Exception: System.MissingMethodException```

**A:** This error indicates that your .NET framework doesn't support latest 4.6.1 instructions, as in - it's outdated. You should make sure that you're using latest .NET framework available for your platform, or latest Mono if you're using it instead. Head over to **[setting up](https://github.com/JustArchi/ArchiSteamFarm/wiki/Setting-up)** for more information.

***

**Q:** ```No bots are running, exiting```

**A:** Either you didn't **[configure](https://github.com/JustArchi/ArchiSteamFarm/wiki/Setting-up)** ASF, or you didn't enable any configured bot instance. When all bots exit, ASF will shutdown as well, as it has nothing to do (unless it's being run in ```--server``` **[mode](https://github.com/JustArchi/ArchiSteamFarm/wiki/Command-Line-Arguments)**).

***

**Q:** ASF is failing with ```Request failed even after 5 tries``` errors!

**A:** This is most common error, and you should get used to that - it simply means that ASF sent a request to Steam Network, and didn't get a valid response, in addition to that - in 4 retries. Usually it means that Steam is either down or is having some difficulties or maintenance - ASF is aware of such issues and you should not worry about them, unless they're happening constantly for longer than several hours, and other users do not have such problems.

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

**A:** For accepting trades, firstly, make sure that ```SteamApiKey``` config property is set, and is valid. If it is, you should notice that after sending the trade to the bot, it tries to accept it:
```
[*] INFO: ParseTrade() <1> Accepting trade: XXXX
```

If you can see this, then your API key is proper. It's nice to note that you don't need to have ```SteamApiKey``` set if you'll only initiate Bot->Master trades, such as ```!loot```, as API key is required only for receiving trades, not sending them.

Next, if you do not use **[ASF 2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)**, it's possible that ASF in fact accepted/sent trade, but you need to confirm it via your e-mail. Likewise, if you use classic 2FA, you need to confirm the trade via your authenticator. Confirmations are **mandatory** now, so if you don't want to accept them by yourself, consider either adding or importing your authenticator into ASF 2FA.

Keep in mind that new accounts start as limited. Until you unlock account by loading it's wallet or spending 5$ in the store, all dropped cards are **non-tradable**, and account itself can't accept neither send trades as well. In this case, ASF will state that inventory seems empty, because every card that is in it is non-tradable.

Also notice that you can trade only with your friends, and people with known trade link. If you're trying to initiate Bot->Master trade, such as ```!loot```, then you need to either have ```SteamMasterID``` on Bot's friendlist, or ```SteamTradeToken``` of ```SteamMasterID``` set in Bot's config. Otherwise, you won't be able to send such trade.

Lastly, remember that new devices have 7-days trade lock, so if you've just added your account to ASF, wait at least 7 days - everything should work after that period. That limitation includes **both** accepting **and** sending trades. It does not always trigger, and there are people who can send and accept trades instantly. Majority of the people are affected though, and the lock **will** happen, even if you can send and accept trades through your steam client on the same machine. Just wait patiently, there's nothing you can do to make it faster.

And finally, keep in mind that one account can have only 5 pending trades to another one, so ASF will fail to send trades if you have 5 (or more) pending ones from that one bot to accept already. This is rarily a problem, but it's also worth mentioning.

If nothing helped, you can always enable ```Debug``` mode and check yourself why requests are failing. Sometimes there is no reason or provided reason doesn't make any sense, it's quite common issue and the only suggested solution is to re-add account to ASF (and wait 7 days again). Sometimes it also fixes itself *magically*, the same way it breaks. However, usually it's just either 7-days trade lock, temporary steam problem, or both. It's best to give it a few days before manually checking what is wrong, unless you have some urge to debug the real cause.

In any case, ASF can only **try** to send a proper request to Steam in order to accept/send trade. Whether Steam accepts that request, or not, is out of the scope of ASF, and ASF will not magically make it work. There's no bug related to that feature, and there is also nothing to improve, because logic is happening outside of ASF. Therefore, do not ask why ASF can't accept or send trades - **I don't know**, and ASF doesn't know either.

***

**Q:** Why do I have to put 2FA code on each login?

**A:** ASF uses login keys for keeping 2FA active, the same mechanism that Steam uses - 2FA token is required only once. However, due to Steam fuckups and Steam network quirks, it's entirely possible that login key is not saved in the network, I've already seen such issues not only with ASF, but with regular steam client as well (a need to input login + password on each run, regardless of "remember me" option).

You could remove bot.db of affected account and try to link ASF to your account once again, but that doesn't have to succeed. The real ASF-based solution is to import your authenticator as **[ASF 2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)** - this way ASF can generate tokens automatically when they're needed, and you don't have to input them manually. Sometimes the issue also magically solves itself after some time, ask GabeN.

***

**Q:** I'm getting error: ```Unable to login to Steam: InvalidPassword```

**A:** ```InvalidPassword``` can mean a lot of things, some of them include:

- Invalid Login/Password combination (obviously)
- Expired login key used by ASF for logging in
- Too many failed login attempts in short period of time (anti-bruteforce)
- Too many login attempts in short period of time (rate-limiting)
- Requirement of captcha to log in (very likely to be caused by two reasons above)

In case of anti-bruteforce and rate-limiting, problem will disappear after some time, so just wait and don't attempt to log in in the meantime. If you hit that issue frequently, perhaps it's wise to increase ```LoginLimiterDelay``` config property of ASF.

In case of expired login key - ASF will remove old one and ask for new one on next login (which will require from you putting 2FA token if your account is 2FA-protected. If your account is using ASF 2FA, token will be generated and used automatically). If you get this issue often, it's possible that Steam for some reason decided to ignore our login key save requests:

> You could remove bot.db of affected account and try to link ASF to your account once again, but that doesn't have to succeed. The real ASF-based solution is to import your authenticator as **[ASF 2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)** - this way ASF can generate tokens automatically when they're needed, and you don't have to input them manually. Sometimes the issue also magically solves itself after some time, ask GabeN.

And lastly, if you used wrong login + password combination, obviously you need to correct this, or disable bot that is attempting to connect using those credentials. ASF can't guess on it's own whether ```InvalidPassword``` means invalid credentials, or any of the reasons listed above, therefore it'll keep trying until it succeeds.

Keep in mind that ASF has it's own built-in system to react accordingly to steam quirks, eventually it will connect and resume it's job, therefore it's not required to do anything if the issue is temporary. Restarting ASF in order to magically fix problems will only make things worse (as new ASF won't know previous ASF state of ```InvalidPassword```, and try to connect instead of waiting), so avoid doing that unless you know what you're doing.

***

**Q:** ASF is being detected by my AV as Trojan: Win32/Fethar.B!cl / Win32/Zulushal.C!cl!

**A:** False positive. If you're worried about accuracy of previous statement, I suggest scanning ASF binary with many different AVs, for example through **[VirusTotal](https://virustotal.com/)**. A sample analysis result is available **[here](https://virustotal.com/file/d40cc117c485046990e9e5c4640b62571abb5c4a4b82125f2b31f43fa5499e6c/analysis/1467438651/)**. Only Microsoft AVs, including Security Essentials and Windows Defender falsely detect ASF as a trojan - I already filled a report and sent back to Microsoft but they don't seem to care, so I don't care either. If you want to solve the issue, either use another AV, add ASF to some kind of exceptions, or tell Microsoft to fix their detection engine yourself, because they don't seem to listen to me. Or, just don't use the program if you don't trust it. For more info about the problem, see **[issue](https://github.com/gluck/il-repack/issues/152)**.

***

**Q:** ```Bot database could not be loaded, refusing to start this bot instance!```

**A:** The above means that ```Bot.db``` of given bot instance is corrupted. Corruption can happen due to various reasons, including power outages, OS malfunctions, HDD problems and more. ASF can't magically fix your corrupted database, therefore you're required to fix it manually.

You can do that by either restoring your previously backuped ```Bot.db```, or removing corrupted ```Bot.db```, so ASF will recreate it from scratch. ASF doesn't recreate database from scratch automatically because it doesn't know if it's appropriate or not, which is especially true if you had ```ASF 2FA``` enabled for this account, as you most likely want to import it once again.

Until you fix the problem, given bot instance will be disabled and won't run, but it won't affect other configured bots functioning normally.