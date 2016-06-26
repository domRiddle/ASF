# FAQ

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

**A:** **Yes**, although ASF knows better when to use that feature, based on selected **[cards farming algorithm](https://github.com/JustArchi/ArchiSteamFarm/wiki/Performance)**. You do not have direct choice on cards farming algorithm, but you can suggest ASF one, via setting config properties properly.

***

## VAC / Bans / ToS

***

**Q:** Can I get VAC ban for using this?

**A:** No, it's not possible because ASF (unlike Idle Master or SAM) does not interfere in any way with steam client nor it's processes. It's physically impossible to get VAC ban for using ASF, even during playing on secured servers while ASF is running. ASF is the only farming program that can currently guarantee that.

***

**Q:** Is it safe?

**A:** If you ask if ASF is safe as a software, which means that it won't cause any damage to your computer, won't steal your private data, install viruses or any other stuff like that - it is safe. Code is open-source, and distributed binaries are always compiled from publicly available sources. If you for whatever reason don't trust them, you can always compile and use ASF from source, including all libraries that ASF is using (such as SteamKit2), which are open-source too.

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

**A:** Up to today, I didn't hear of any single case where ASF would be directly responsible for any kind of steam suspension. Therefore, we can assume that Valve either doesn't care, or like I pointed above - ASF doesn't violate any terms of service. Valve didn't state whether they agree with what ASF (or IdleMaster) is doing, or not. Therefore, you should consider both programs as grey zone, and decide yourself if you're fine with using ASF or not. If you ask me, you should be perfectly fine, but nobody can guarantee that, as written above.

***

## Misc

***

**Q:** I have a question regarding ASF 2FA...

**A:** Then check **[ASF 2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)** wiki page.

***

**Q:** Is there any way to communicate with ASF?

**A:** Yes, through steam chat, or by using **[WCF](https://github.com/JustArchi/ArchiSteamFarm/wiki/WCF)**

***

**Q:** Is there any way to communicate with ASF?

**A:** Yes, through steam chat, or by using **[WCF](https://github.com/JustArchi/ArchiSteamFarm/wiki/WCF)**

***

**Q:** ASF seems to be working, but I'm not receiving any card drops!

**A:** Cards farming rate differs from game to game, as you can read in **[Performance](https://github.com/JustArchi/ArchiSteamFarm/wiki/Performance)**. It takes a while and you shouldn't expect cards to drop in a few minutes since launching a program. If you can see that ASF switches the game in the console, then everything works fine - you're probably referring to inventory notifications, which are automatically dismissed by ASF through ```DismissInventoryNotifications``` bot config property. Check out **[Configuration](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration)** for details.

***

**Q:** How many bots can I run with ASF?

**A:** ASF as a program doesn't have any upper limit of bot instances, however - steam includes some restrictions. Currently you can run up to 100-110 bots with one IP and 1 ASF instance. It is possible to run more bots with more IPs and more ASF instances. Keep in mind that if you're using that big amount of bots, you should control their number yourself (such as making sure that all of them in fact are logging in and working at the same time).

***

**Q:** Can I run more ASF instances then?

**A:** You can run as many ASF instances on one machine as you like, assuming every instance has it's own directory and it's own configs, and account used in one instance is not used in another one. However, ask yourself why you want to do that. ASF is optimized to handle a dozen, even a hundred of accounts at the same time, and launching those dozen of bots in their own ASF instances degrades performance, takes more OS resources, and causes lack of synchronization between bots - so for example you're more likely to hit ```InvalidPassword``` issue described below, as logging in requests are not being synchronized between ASF instances. Therefore, my **strong suggestion** is, always run maximum of one ASF instance per one IP/interface. If you have more IPs/interfaces, by all means you can run more ASF instances, every instance using it's own IP/interface. If you don't, launching more ASF instances is totally pointless, and does not only degrade performance and takes more OS resources (such as memory), but also causes lack of synchronization and increased likehood of causing issues.

***

**Q:** Farming takes too long, can I somehow speed it up?

**A:** The only thing which heavily affects speed of farming is selected **[cards farming algorithm](https://github.com/JustArchi/ArchiSteamFarm/wiki/Performance)** for your bot instance. Everything else has negligible effect and will not make farming faster, while some actions such as launching ASF process several times will even **make it worse**. If you really have an urge of making every damn second from farming process, then ASF allows you to fine-tune some core farming variables such as ```FarmingDelay``` - all of them are explained in **[configuration](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration)**. However, as I said, the effect is negligible, and choosing proper cards farming algorithm for given account is one and the only crucial choice that can heavily affect speed of farming, everything else is pure cosmetic. Instead of worrying about farming speed, just launch ASF and let it do it's job - I can assure you that it's doing it in the most effective way I could come up with :+1:.

***

## Issues

***

**Q:** ASF is failing with ```Request failed even after 5 tries, WTF?``` errors!

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

**Q:** ASF seems to be stuck after ```Connected to Steam!``` and ```Logging in...```. Nothing is happening!

**A:** This is an issue of SteamKit2 library that ASF is using. It was already reported and is pending to fix. Until it gets fixed, I suggest switching ```HackIgnoreMachineID``` global config property to ```true```, which works around this issue. You can read more about the issue **[here](https://github.com/JustArchi/ArchiSteamFarm/issues/154)**.

*** 

**Q:** ASF can't accept or send trades!

**A:** For accepting trades, firstly, make sure that ```SteamApiKey``` config property is set, and is valid. If it is, you should notice that after sending the trade to the bot, it tries to accept it:
```
[*] INFO: ParseTrade() <1> Accepting trade: XXXX
```

If you can see this, then your API key is proper. It's nice to note that you don't need to have ```SteamApiKey``` set if you'll only initiate Bot->Master trades, such as ```!loot```, as API key is required only for receiving trades, not sending them.

Next, if you do not use **[ASF 2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)**, it's possible that ASF in fact accepted/sent trade, but you need to confirm it via your e-mail. Likewise, if you use classic 2FA, you need to confirm the trade via your authenticator. Confirmations are **mandatory** now, so if you don't want to accept them by yourself, consider either adding or importing your authenticator into ASF 2FA.

Keep in mind that new accounts start as limited. Until you unlock account by loading it's wallet or spending 5$ in the store, all dropped cards are **non-tradable**, and account itself can't accept neither send trades as well.

Lastly, remember that new devices have 7-days trade lock, so if you've just added your account to ASF, wait at least 7 days - everything should work after that period. That limitation includes **both** accepting **and** sending trades. It does not always trigger, and there are people who can send and accept trades instantly. Majority of the people are affected though, and the lock **will** happen, even if you can send and accept trades through your steam client on the same machine. Just wait patiently, there's nothing you can do to make it faster.

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

**Q:** ASF is being detected by my AV as Trojan: Win32/Fethar.B!cl!

**A:** False positive. If you're worried about accuracy of previous statement, I suggest scanning ASF binary with many different AVs, for example through **[VirusTotal](https://virustotal.com/)**. A sample analysis result is available **[here](https://virustotal.com/file/0b48ba917be45cab793c343f133b3dbeaff262953d917512cb8fc92e4e9374f9/analysis/)**. Only Microsoft AVs, including Security Essentials and Windows Defender falsely detect ASF as a trojan - I already filled a report and sent back to Microsoft but they don't seem to care, so I don't care either. If you want to solve the issue, either use another AV, add ASF to some kind of exceptions, or tell Microsoft to fix their detection engine yourself, because they don't seem to listen to me. Or, just don't use the program if you don't trust it. For more info about the problem, see **[issue](https://github.com/gluck/il-repack/issues/152)**.

***

**Q:** ```Bot database could not be loaded, refusing to start this bot instance!```

**A:** The above means that ```Bot.db``` of given bot instance is corrupted. Corruption can happen due to various reasons, including power outages, OS malfunctions, HDD problems and more. ASF can't magically fix your corrupted database, therefore you're required to fix it manually.

You can do that by either restoring your previously backuped ```Bot.db```, or removing corrupted ```Bot.db```, so ASF will recreate it from scratch. ASF doesn't recreate database from scratch automatically because it doesn't know if it's appropriate or not, which is especially true if you had ```ASF 2FA``` enabled for this account, as you most likely want to import it once again.

Until you fix the problem, given bot instance will be disabled and won't run, but it won't affect other configured bots functioning normally.