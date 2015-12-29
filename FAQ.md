# FAQ

***

**Q:** ASF doesn't automatically accept trades!

**A:** Firstly, make sure that ```SteamApiKey``` config property is set, and is valid. If it is, you should notice that after sending the trade to the bot, it tries to accept it:
```
[*] INFO: ParseTrade() <1> Accepting trade: XXXX
```

If you can see this, then your API key is proper.

Next, if you do not use **[ASF 2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)**, then make sure that you have **[trade confirmations turned off](http://steamcommunity.com/my/edit/settings)**, as ASF can't automatically accept trade confirmations sent to your e-mail or mobile phone. Keep in mind that turning off confirmations it NOT required by ASF, it's required only if you want to accept trades automatically and you do not use ASF 2FA.

Lastly, remember that new devices have 7-days trade lock, so if you've just added your account to ASF, wait at least 7 days. After that period, everything should work.

***

**Q:** Can I get VAC ban for using this?

**A:** No, it's not possible because ASF (unlike Idle Master or SAM) does not interfere in any way with steam client nor it's processes. It's physically impossible to get VAC ban for using ASF, even during playing on secured servers while ASF is running.

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

**A:** Up to today, there was only **one** report from user who got 1 month steam market suspension on all of his alts. This is however unconfirmed and most likely not caused by ASF as he was still able to trade those cards (just not sell them on market from alts), and if I were the one suspending him, I'd make sure he won't get any profits from those cards instead of putting temporary steam market ban. Therefore, if you ask me, I don't think that suspension is ASF-related, but I'm committed to tell you that such situation did happen.

***

**Q:** I have a question regarding ASF 2FA...

**A:** Then check **[ASF 2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)** wiki page.