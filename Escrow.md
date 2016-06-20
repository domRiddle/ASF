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

If you have your authenticator running in SDA already, you should notice that there is ```steamID.maFile``` file available in ```maFiles``` folder. Copy that file to ```config``` directory of ASF. Make sure that ```.maFile``` is in unencrypted form, as ASF can't decrypt SDA files. It should have already familiar to you JSON structure :+1:.

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

**A:** You will need 2FA token to access 2FA-protected account, that includes every account with ASF 2FA as well. You can generate temporary tokens through ```!2fa``` command sent via the chat to given bot. You can also use ```!2fa <BOT>``` command to generate temporary token for given bot instance. This should be enough for you to access bot accounts through e.g. browser. Of course, if you use 2FA + ASF 2FA combo, then you can generate tokens either by your phone, or by ASF.

***

**Q:** How to turn ASF 2FA off?

**A:** If ASF 2FA is currently active, you can disable it completely through ```!2faoff``` and ```!2faoff <BOT>``` commands, again, sent to the bot through chat. This command will unlink ASF as mobile authenticator, which will result also in switching your account security from 2FA to either SteamGuard or None, depending what you had before. Keep in mind that this option will NOT remove your linked phone number, you can remove it manually **[here](https://store.steampowered.com/phone/manage)**. Remember that if you use 2FA + ASF 2FA combo, then this command **WILL** delink classic 2FA as well.

***

**Q:** Where is ASF mobile authenticator saved?

**A:** ASF mobile authenticator is saved in ```BotName.db``` file in your config directory. Remember that this file is basically your mobile authenticator, so removing it will result the same as you'd lose your mobile phone with 2FA turned on. Therefore DO NOT REMOVE IT, if you want to turn off ASF 2FA, it is explained how above. Removing the file will do nothing apart from leaving you without mobile authenticator.

***

**Q:** HELP! I LOST MY ASF MOBILE AUTHENTICATOR!

**A:** This can happen if you e.g. removed file mentioned above by accident. There's no need to panic, as long as you have **revocation code** ASF gave to you the moment it was linking itself as mobile authenticator. Simply enter **[here](https://store.steampowered.com/twofactor/manage)** from logged in account, and use your revocation code to disable 2FA from your account. You can then link ASF again (if you like), by following the same procedure.

***

**Q:** BUT I'M NOT LOGGED IN!

**A:** No problem, you can use backup SteamGuard codes I suggested you to do in order to login, then you can disable 2FA through revocation code mentioned above. Alternatively, you can go **[here](https://help.steampowered.com/#HelpWithLoginInfo?nav=authenticator)** and disable 2FA through either SMS, or revocation code, without a need of logging in.

***

**Q:** I REMOVED ASF 2FA FILE, I DIDN'T WRITE DOWN REVOCATION CODE, I DIDN'T GENERATE STEAMGUARD BACKUP CODES, I REMOVED ALL LINKED PHONE NUMBERS, AND SOMEBODY HACKED MY E-MAIL ACCOUNTS!

**A:** A brain was listed in the ASF 2FA requirements. Try steam support, and good luck, you'll need it.