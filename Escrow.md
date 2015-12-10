# Escrow

Recently Valve introduced a system known as "Escrow". You can read more about it **[here](https://support.steampowered.com/kb_article.php?ref=1284-WTKB-4729)** and **[here](https://support.steampowered.com/kb_article.php?ref=8078-TPHC-6195)**. It's crucial to understand Escrow firstly before trying to understand logic behind ASF 2FA. I suggest testing it, reading more about it, whatever you consider appropriate.

Okay, from now on I assume you already know what Escrow is. Now as you can see all trades are being hold for up to 3 days, which is not a major problem when it comes to our ASF, but can still be annoying, especially for those who want full automation. Luckily, I found a way how to overcome that problem, the solution is called ASF 2FA.

# ASF 2FA

The idea is simple. We already imitiate steam client, imitiate launching and playing a game, so why not imitate mobile device? ASF 2FA is exactly what you think it is, it's just a module responsible for generating 2FA tokens as valid recognized mobile device, which allows us to skip 3-days trade lock, and automatically confirm all trades. Sounds awesome, but is complicated as hell, and should be used only by advanced users.

To use ASF 2FA, you need to have:
- A phone number **capable of receiving SMSes**
- An account with **turned off 2FA**, which you want to enable in ASF 2FA
- The option for accepting trades via email must be activated
- A brain

Firstly make sure that you have all of the above, then you can start the fun through switching ```UseAsfAsMobileAuthenticator``` config property to ```true```. After starting ASF, and successfully logging in, you'll see ASF 2FA module becoming active. If you have classic SteamGuard active, you'll need to enter the auth code.

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

**Q:** Why do you need a phone number, if 2FA is supposed to be ASF module?

**A:** Becuase Valve requires confirmed phone number to enable 2FA, regardless if you like it or not.

***

**Q:** Why account must not have 2FA enabled at the time of linking?

**A:** You can have only one mobile authenticator linked to one account at given time. You can use either traditional mobile authenticator on your mobile, or ASF 2FA, not both.

***

**Q:** Why isn't it suggested to use ASF 2FA for primary accounts?

**A:** Primary account is being used by you. ASF 2FA has been created in order to automate trade confirmations and skip requirement of 3-days trade holds. It's one giant hack which defeats the whole purpose of 2FA. Because it's possible to have only one mobile authenticator for given account (see above), I suggest to use ASF 2FA ONLY for accounts that are made purely for farming (alts).

***

**Q:** What if I need a 2FA token?

**A:** You will need 2FA token to access 2FA-protected account, that includes every account with ASF 2FA as well. You can generate temporary tokens through ```!2fa``` command sent via the chat to given bot. You can also use ```!2fa <BOT>``` command to generate temporary token for given bot instance. This should be enough for you to access bot accounts through e.g. browser.

***

**Q:** How to turn ASF 2FA off?

**A:** If ASF 2FA is currently active, you can disable it completely through ```!2faoff``` and ```!2faoff <BOT>``` commands, again, sent to the bot through chat. This command will unlink ASF as mobile authenticator, which will result also in switching your account security from 2FA to either SteamGuard or None, depending what you had before. Keep in mind that this option will NOT remove your linked phone number, you can remove it manually **[here](https://store.steampowered.com/phone/manage)**.

***

**Q:** Where is ASF mobile authenticator saved?

**A:** ASF mobile authenticator is saved as ```BotName.auth``` file in your config directory. Remember that this file is basically your mobile authenticator, so removing it will result the same as you'd lose your mobile phone with 2FA turned on. Therefore DO NOT REMOVE IT, if you want to turn off ASF 2FA, it is explained how above. Removing the file will do nothing apart from leaving you without mobile authenticator. BTW, it may be wise to backup that file as well, it does contain revocation code inside as well.

***

**Q:** HELP! I LOST MY ASF MOBILE AUTHENTICATOR!

**A:** This can happen if you e.g. removed file mentioned above by accident. There's no need to panic, as long as you have **revocation code** ASF gave to you the moment it was linking itself as mobile authenticator. Simply enter **[here](https://store.steampowered.com/twofactor/manage)** from logged in account, and use your revocation code to disable 2FA from your account. You can then link ASF again (if you like), by following the same procedure.

***

**Q:** BUT I'M NOT LOGGED IN!

**A:** No problem, you can use backup SteamGuard codes I suggested you to do in order to login, then you can disable 2FA through revocation code mentioned above.

***

**Q:** I AM LAZY BASTARD AND I DIDN'T GENERATE STEAMGUARD BACKUP CODES!

**A:** Sigh, luckily for you, lazy bastard, you can restore access to your account also through SMS/e-mail. Go **[here](https://help.steampowered.com/#HelpWithLoginInfo?nav=authenticator)**

***

**Q:** I REMOVED ASF 2A FILE, I DIDN'T WRITE DOWN REVOCATION CODE, I DIDN'T GENERATE STEAMGUARD BACKUP CODES, I REMOVED ALL LINKED PHONE NUMBERS, AND SOMEBODY HACKED MY E-MAIL ACCOUNTS!

**A:** A brain was listed in the ASF 2FA requirements. Try steam support, and good luck, you'll need it.