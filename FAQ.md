# FAQ

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

**A:** Up to today, I didn't hear of any single case where ASF would be directly responsible for any kind of steam suspension. Therefore, we can assume that Valve either doesn't care, or like I pointed above - ASF doesn't violate any terms of service.

***

**Q:** I have a question regarding ASF 2FA...

**A:** Then check **[ASF 2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)** wiki page.

***

**Q:** Is there any way to communicate with ASF?

**A:** Yes, through steam chat, or by using **[WCF](https://github.com/JustArchi/ArchiSteamFarm/wiki/WCF)**

***

**Q:** ASF doesn't automatically accept trades!

**A:** Firstly, make sure that ```SteamApiKey``` config property is set, and is valid. If it is, you should notice that after sending the trade to the bot, it tries to accept it:
```
[*] INFO: ParseTrade() <1> Accepting trade: XXXX
```

If you can see this, then your API key is proper.

Next, if you do not use **[ASF 2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)**, then make sure that you have **[trade confirmations turned off](http://steamcommunity.com/my/edit/settings)**, as ASF can't automatically accept trade confirmations sent to your e-mail or mobile phone. Keep in mind that turning off confirmations it NOT required by ASF, it's required only if you want to accept trades automatically and you do not use ASF 2FA. If you have confirmations turned on, it's possible that ASF in fact accepted trade, but you need to confirm it via your e-mail.

Lastly, remember that new devices have 7-days trade lock, so if you've just added your account to ASF, wait at least 7 days. After that period, everything should work.

***

**Q:** What are the .key-Files used for?

**A:** Alternative Password that is used to login to Steam when 2FA is enabled. It´s the same when you´re updating your Steam-Client and don´t have to insert a 2FA-Token again, except this key is used for launching instead of updating.