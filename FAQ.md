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

**Q:** I have a question regarding ASF 2FA...

**A:** Then check **[ASF 2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)** wiki page.