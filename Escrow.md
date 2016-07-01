# Escrow

Recently Valve introduced a system known as "Escrow". You can read more about it **[here](https://support.steampowered.com/kb_article.php?ref=1284-WTKB-4729)** and **[here](https://support.steampowered.com/kb_article.php?ref=8078-TPHC-6195)**. It's crucial to understand Escrow firstly before trying to understand logic behind ASF 2FA. I suggest testing it, reading more about it, whatever you consider appropriate.

Okay, from now on I assume you already know what Escrow is. Now as you can see all trades are being hold for up to 15 days, which is not a major problem when it comes to our ASF, but can still be annoying, especially for those who want full automation. Luckily, I coded a way how to overcome that problem, the solution is called ASF 2FA.

# ASF 2FA

The idea is simple. We already imitiate steam client, imitiate launching and playing a game, so why not imitate mobile device? ASF 2FA is exactly what you think it is, it's just a module responsible for generating 2FA tokens as valid recognized mobile device, which allows us to skip trade holds, and automatically confirm all trades. Sounds awesome, but requires some effort, and should be used only by advanced users.

To enable ASF 2FA, you need to have:
- Working steam authenticator in your Android phone
- or working steam authenticator in **[SteamDesktopAuthenticator](https://github.com/Jessecar96/SteamDesktopAuthenticator)**
- or working steam authenticator in **[WinAuth](https://winauth.com/)**

Also a brain is needed for all of those tasks :+1: 

***

## Import

From version V2.1 onwards, ASF no longer allows you to use ASF 2FA "solo" mode - it means that you should have already linked and operational authenticator that is supported by ASF. ASF currently supports three different sources of 2FA - Your Android phone, SteamDesktopAuthenticator and WinAuth. If you don't have any authenticator yet, and you're about to link for the first time, I strongly encourage to use WinAuth, which can be then imported to ASF (and used by you).

***

### Android phone

In order to import from your Android phone, you should download **[SteamDesktopAuthenticator](https://github.com/Jessecar96/SteamDesktopAuthenticator/blob/master/README.md)** and **[import your account from android phone](https://github.com/Jessecar96/SteamDesktopAuthenticator/wiki/Importing-account-from-an-Android-phone)**. Visit those two links in order to learn how to do that in a right way. After successfully importing, follow the rest of instructions for SDA below. You can also use WinAuth instead of SDA for this step, likewise, after you finish import, you should follow the rest of instructions.

If you're not sure which program to use, use SDA if you don't have a rooted phone, and WinAuth if you have root access. SDA in general seems to cause trouble, and non-root method doesn't work for majority of the people, so most likely **root will be required to import authenticator from your phone**. You can still try SDA non-root method, but don't be shocked if it doesn't work.

In any case, if you successfully imported your authenticator either into SDA (root/non-root), or WinAuth (root), proceed below.

***

### SteamDesktopAuthenticator

If you have your authenticator running in SDA already, you should notice that there is ```steamID.maFile``` file available in ```maFiles``` folder. Copy that file to ```config``` directory of ASF. Make sure that ```.maFile``` is in unencrypted form, as ASF can't decrypt SDA files - it should start with ```{``` character.

You should now rename ```steamID.maFile``` to ```Bot.maFile``` where ```Bot``` is the name of your bot instance (```Bot.json```). Alternatively you can leave it as it is, ASF will then pick it automatically after logging in. Helping ASF makes it possible to use ASF 2FA before logging in, if you won't help ASF, then the file can be picked only after ASF successfully logs in (as ASF doesn't know ```steamID``` of your account before in fact logging in).

If you did everything correctly, launch ASF, and you should notice:

```
[*] INFO: ImportAuthenticator() <1> Converting .maFile into ASF format...
[*] INFO: ImportAuthenticator() <1> Successfully finished importing mobile authenticator!
```

From now on, your ASF 2FA should be operational for this account.

***

### WinAuth

**Make sure that you're using WinAuth in version 3.4 or higher, as older versions do not support Steam authenticator properly.**

Firstly create new empty ```Bot.maFile``` file in ASF ```config``` directory. Remember that it should be ```Bot.maFile``` and NOT ```Bot.maFile.txt```, Windows likes to hide known extensions by default. If you provide incorrect name, it won't be picked by ASF.

Now launch WinAuth as usual. Right click on Steam icon and select "Show SteamGuard and Recovery Code". Then check "Allow copy". You should notice familiar to you JSON structure on the bottom of the window, starting with ```{```. Copy whole text into a ```Bot.maFile``` file created by you in previous step.

If you did everything correctly, launch ASF, and you should notice:

```
[*] INFO: ImportAuthenticator() <1> Converting .maFile into ASF format...
<1> Please enter your Device ID (including "android:"):
```

This is when tricky part comes in. WinAuth is missing deviceID property that is required by ASF, so you'll need to do one more thing.

Go back to WinAuth's "Show SteamGuard and Recovery Code" and you should notice "Device ID" property above the JSON code you were copying not that long ago. Copy whole android device ID, including ```android:``` part into ASF.

If you've done that properly as well, you're now done!

```
[*] INFO: ImportAuthenticator() <1> Successfully finished importing mobile authenticator!
```

Please confirm that accepting confirmations in fact works. If you made a mistake while entering your ```DeviceID``` then you'll have half-broken authenticator - tokens will work, but accepting confirmations will not. You can always remove ```Bot.db``` and start over if needed.

***

From that moment, all ```!2fa``` commands will work as they'd be called on your classic 2FA device. You can use both ASF 2FA and your authenticator of choice (android phone, SDA, WinAuth, or all three) to generate tokens and accept confirmations.

If you have authenticator on your phone, you can optionally remove SteamDesktopAuthenticator and/or WinAuth, as we won't need it anymore. However, I suggest to keep it just in case, not to mention that it's more handy than normal steam authenticator. Do as you please.

***

## FAQ

**Q:** What if I need a 2FA token?

**A:** You will need 2FA token to access 2FA-protected account, that includes every account with ASF 2FA as well. You should generate tokens in authenticator that you used for import, but you can also generate temporary tokens through ```!2fa``` command sent via the chat to given bot. You can also use ```!2fa <BOT>``` command to generate temporary token for given bot instance. This should be enough for you to access bot accounts through e.g. browser.

***

**Q:** Where is ASF mobile authenticator saved?

**A:** ASF mobile authenticator is saved in ```BotName.db``` file in your config directory, along with some other crucial data related to given account. If you want to remove ASF 2FA, read how below.

***

**Q:** How to remove ASF 2FA?

**A:** Simply stop ASF and remove associated ```BotName.db``` of the bot with linked ASF 2FA you want to remove. This option will remove associated imported 2FA with ASF, but will NOT delink your authenticator. If you instead want to delink your authenticator, apart from removing it from ASF (firstly), you should delink it in authenticator of your choice (Android, SDA or WinAuth), or - if you can't for some reason, use revocation code that you received during linkling for that via Steam website.

***

**Q:** HELP! I LOST MY ASF MOBILE AUTHENTICATOR!

**A:** This can happen if you e.g. removed file mentioned above by accident. There's no need to panic, as long as you have **revocation code** that you received during linking. Simply enter **[here](https://store.steampowered.com/twofactor/manage)** from logged in account, and use your revocation code to disable 2FA from your account. You can then link ASF again (if you like), by following the same procedure. Of course, if you still have your original authenticator, it's probably easier to delink it from there.

***

**Q:** BUT I'M NOT LOGGED IN!

**A:** No problem, you can use backup SteamGuard codes (if you have them). Alternatively, you can go **[here](https://help.steampowered.com/#HelpWithLoginInfo?nav=authenticator)** and disable 2FA through either SMS, or revocation code, without a need of logging in.

***

**Q:** I REMOVED ASF 2FA FILE, I DIDN'T WRITE DOWN REVOCATION CODE, I DIDN'T GENERATE STEAMGUARD BACKUP CODES, I REMOVED ALL LINKED PHONE NUMBERS, AND SOMEBODY HACKED MY E-MAIL ACCOUNTS!

**A:** A brain was listed in the ASF 2FA requirements. Try steam support, and good luck, you'll need it.