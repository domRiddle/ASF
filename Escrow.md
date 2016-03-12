# Escrow

Recently Valve introduced a system known as "Escrow". You can read more about it **[here](https://support.steampowered.com/kb_article.php?ref=1284-WTKB-4729)** and **[here](https://support.steampowered.com/kb_article.php?ref=8078-TPHC-6195)**. It's crucial to understand Escrow firstly before trying to understand logic behind ASF 2FA. I suggest testing it, reading more about it, whatever you consider appropriate.

Okay, from now on I assume you already know what Escrow is. Now as you can see all trades are being hold for up to 3 days, which is not a major problem when it comes to our ASF, but can still be annoying, especially for those who want full automation. Luckily, I found a way how to overcome that problem, the solution is called ASF 2FA.

# ASF 2FA

The idea is simple. We already imitiate steam client, imitiate launching and playing a game, so why not imitate mobile device? ASF 2FA is exactly what you think it is, it's just a module responsible for generating 2FA tokens as valid recognized mobile device, which allows us to skip 3-days trade lock, and automatically confirm all trades. Sounds awesome, but is complicated as hell, and should be used only by advanced users.

To enable ASF 2FA, you need to have:
- A phone number **capable of receiving SMSes**
- An account with **turned off 2FA**, which you want to enable in ASF 2FA

To enable 2FA + ASF 2FA, you need to have **either**:
- Working steam authenticator in your Android phone
- or working steam authenticator in **[SteamDesktopAuthenticator](https://github.com/Jessecar96/SteamDesktopAuthenticator)**
- or working steam authenticator in **[WinAuth](https://winauth.com/)**

Also a brain is needed for all of those tasks :+1: 

***

## ASF 2FA (only)

This part is for you if you want to use ASF 2FA only (no classic 2FA for account)

Firstly make sure that you pass all the requirements above, then you can start the fun through switching ```UseAsfAsMobileAuthenticator``` config property to ```true```. After starting ASF, and successfully logging in, you'll see ASF 2FA module becoming active. If you have classic SteamGuard active, you'll need to enter the auth code.

```
[*] NOTICE: LinkMobileAuthenticator() <1> Linking new ASF MobileAuthenticator...
<1> Please enter the auth code sent to your email:
```

If you don't have any phone number linked to your account yet, you'll be prompted to enter it. Please notice that number should be in international format starting from +CC (country code).
```
<1> Please enter your full phone number (e.g. +1234567890):
```

To link your phone number successfully, you'll be prompted to confirm it. Confirmation code should arrive instantly in SMS message.

```
<1> Please enter SMS code sent on your mobile:
```

After above step, assuming everything ended successfully, you should see:
```
[*] INFO: LinkMobileAuthenticator() <1> Successfully linked ASF as new mobile authenticator for this account!
<1> PLEASE WRITE DOWN YOUR REVOCATION CODE: RXXXXX
<1> THIS IS THE ONLY WAY TO NOT GET LOCKED OUT OF YOUR ACCOUNT!
<1> Hit enter once ready...
```

You should save your revocation code in secure place (NOT in ASF folder). This is the only way to recover your account if something goes wrong, for example if you accidentally delete whole ASF folder, including your linked ASF mobile authenticator. Don't skip this step, or you'll deeply regret it later.

From now onwards, ASF is able to use newly linked ASF mobile authenticator to generate 2FA tokens on as-needed basis.

I also suggest making a backup of SteamGuard codes, which can be done **[here](https://store.steampowered.com/twofactor/manage)**

***

### Limitations

Currently ASF 2FA can't be enabled on accounts restricted with Steam Parental PIN, you'll need to disable that feature at the time of linking, you can re-enable it after linking is done.

***

## 2FA + ASF 2FA

This part is for you if you want to use ASF 2FA AND 2FA for the same account.

This is much more complicated than using ASF 2FA only, but still possible. You can have only one authenticator enabled for given account, so instead of linking ASF 2FA as **new** authenticator, we'll instead import your current 2FA authenticator on your phone and use it in ASF 2FA. ASF supports three different sources of 2FA - Your Android phone, SteamDesktopAuthenticator and WinAuth.

***

### Android phone

In order to import from your Android phone, you should download **[SteamDesktopAuthenticator](https://github.com/Jessecar96/SteamDesktopAuthenticator/blob/master/README.md)** and **[import your account from android phone](https://github.com/Jessecar96/SteamDesktopAuthenticator/wiki/Importing-account-from-an-Android-phone)**. Visit those two links in order to learn how to do that in a right way. After successfully importing, follow the rest of instructions for SDA below. You can also use WinAuth instead of SDA for this step, likewise, after you finish import, you should follow the rest of instructions. If you're not sure which program to use, use SDA, as it's faster to do import to ASF later :+1:.

***

### SteamDesktopAuthenticator

If you have your authenticator running in SDA already, you should notice that there is ```steamID.maFile``` file available in ```maFiles``` folder. Copy that file to ```config``` directory of ASF. Make sure that ```.maFile``` is in unencrypted form, as ASF can't decrypt SDA files. It should have already familiar to you JSON structure :+1:.

You should now rename ```steamID.maFile``` to ```Bot.maFile``` where ```Bot``` is the name of your bot instance (```Bot.json```). Alternatively you can leave it as it is, ASF will then pick it automatically after logging in. Helping ASF makes it possible to use ASF 2FA before logging in, if you won't help ASF, then the file can be picked only after ASF successfully logs in (as ASF doesn't know ```steamID``` of your account before in fact logging in).

If you did everything correctly, launch ASF, and you should notice:

```
[*] INFO: ImportAuthenticator() <1> Converting SDA .maFile into ASF format...
[*] INFO: ImportAuthenticator() <1> Success!
[*] INFO: ImportAuthenticator() <1> Successfully finished importing mobile authenticator!
```

***

### WinAuth (since ASF V2.0.0.8+)

WinAuth import is a bit more tricky than SDA, but still possible with a little more effort.

Firstly create new empty ```Bot.maFile``` file in ASF ```config``` directory. Remember that it should be ```Bot.maFile``` and NOT ```Bot.maFile.txt```, Windows likes to hide known extensions by default.

Now launch WinAuth as usual. Right click on Steam icon and select "Show SteamGuard and Recovery Code". Then check "Allow copy". You should notice familiar to you JSON structure on the bottom of the window, starting with ```{"shared_secret```. Copy whole text into a ```Bot.maFile``` file created by you in previous step.

If you did everything correctly, launch ASF, and you should notice:

```
[*] INFO: ImportAuthenticator() <1> Converting SDA .maFile into ASF format...
[*] INFO: ImportAuthenticator() <1> Success!
[*] INFO: ImportAuthenticator() <1> ASF requires a few more steps to complete authenticator import...
```

This is when tricky part comes in. WinAuth is missing some required by ASF data, so you'll need to do some extra steps. Firstly you'll be prompted to enter your 2FA code, this should be done from your WinAuth.

```
<1> Please enter your 2 factor auth code from your authenticator app:
```

Then you'll be asked to put your android device ID:

```
<1> Please enter your Device ID (including "android:"):
```

Go back to WinAuth's "Show SteamGuard and Recovery Code" and you should notice "Device ID" property above the JSON code you were copying not that long ago. Copy whole android device ID, including ```android:``` part into ASF. BTW, you can right click on console bar, select properties and enable "fast edit mode" to enable copy paste feature of the console. Your mouse right click can work as paste button in this case :+1: 

If you've done that properly as well, you're now done!

```
[*] INFO: ImportAuthenticator() <1> Successfully finished importing mobile authenticator!
```

Please confirm that accepting confirmations in fact works. If you made a mistake while entering your ```DeviceID``` then you'll have half-broken authenticator - tokens will work, but accepting confirmations will not. You can always remove ```Bot.db``` and start over if needed.

***

From that moment, all ```!2fa``` commands will work as they'd be called on your classic 2FA device. You can use both ASF 2FA and your authenticator of choice (android phone, SDA or WinAuth) to generate tokens and accept confirmations.

You can remove SteamDesktopAuthenticator and/or WinAuth after you're done, as we won't need it anymore, if you want to of course.

***

## FAQ

**Q:** Why do you need a phone number, if 2FA is supposed to be ASF module?

**A:** Becuase Valve requires confirmed phone number to enable 2FA, regardless if you like it or not. It's used only once, during linking.

***

**Q:** Why account must not have 2FA enabled at the time of linking?

**A:** You can have only one mobile authenticator linked to one account at given time. If you want to use ASF 2FA only, then you must not have any other authenticators enabled (obviously). If you want to use classic 2FA and ASF 2FA at the same time, then you should firstly set up classic authenticator on your phone, then import it to SteamDesktopAuthenticator, and then import it to ASF, as explained above.

***

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