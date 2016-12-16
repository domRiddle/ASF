# Configuration

This page is dedicated for ASF configuration. It includes both file structure used by ASF, as well as fine-tuning ASF to your needs.

---

## Introduction

ASF configuration is divided into two major ports - global (process) configuration, and configuration of every bot. Bot is a single steam account that is taking part in ASF process. In order to work, ASF needs at least one **enabled** bot instance. There is no process-enforced limit of supported bot instances, so you can use as many steam accounts (bots) as you want.

Configuration can be done either manually - by creating proper JSON configs, or by using graphical config generator - **ASF-ConfigGenerator.exe**, which should be much easier and convenient. Unless you're advanced user, I suggest using the config generator. Simply double-click **ASF-ConfigGenerator.exe** to launch it, then follow tutorial that ASF will offer to you. 

Alternatively, you can create proper configs yourself in config directory if you decided to go with manual way (check example.json for a good start). This is recommended only for advanced users though.

[**Global config**](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#global-config)

[**Bot config**](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#bot-config)

---

## File structure

ASF is using quite simple file structure.

```
├── config
│   ├── ASF.json
│   ├── ASF.db
│   ├── Bot1.json
│   ├── Bot1.db
│   ├── Bot1.bin
│   ├── Bot2.json
│   ├── Bot2.db
│   ├── Bot2.bin
│   └── ...
├── ASF.exe
└── log.txt
```

**Mandatory** tag used below means that given file is absolutely crucial to launch ASF. **Generated** tag means that file does not exist by default, and may be generated in ASF process, on as-needed basis.

In order to move ASF to new location, or another PC, it's enough to move whole file structure mentioned above. No further action is required.

``ASF.exe (mandatory)`` is the core executable (binary) file, which is starting the program.

```log.txt (generated)``` is the log file of ASF process. Log file holds only last ASF run, and is being automatically cleared on every launch. The purpose of the log file is to log the information about potential bug or crash, that could help ASF developers to find and fix the culprit. Log does not contain any sensitive information, and is mostly used for debugging and informational purposes.

```config (mandatory)``` is the directory which holds configuration for ASF process, and all the bots.

```ASF.json (mandatory)``` is global ASF configuration file. This config is used for specifying how ASF process behaves, which affects program as a whole. You can (and should) edit global config according to your needs. It is well explained below.

```ASF.db (generated)``` is global ASF database file. It acts as ASF global persistent storage and is used for saving some important information. **You should not edit this file**.


Now we move onto bot configs. Every bot has it's own config and related files.

```Bot.json (mandatory)``` is config of given bot instance. This config is used for specifying how given bot instance behaves, including all potentially needed details for it to run properly. Config properties defined in this file affect only given bot instance, so you can have many bots operating in different ways (as opposed to global ASF config which affects the whole process and every bot)

```Bot.db (generated)``` is database of given bot instance. This file is used for storing crucial data about given bot instance in persistent storage. **You should not edit this file**.

```Bot.bin (generated)``` is special file of given bot instance, which holds information of Steam sentry hash. Sentry hash is used for authenticating using ```SteamGuard``` mechanism. **You should not edit this file**.

---

## Configs

ASF is using **[JSON](https://en.wikipedia.org/wiki/JSON)** format for storing it's config files. It's human-friendly, readable and very universal format in which you can configure global and bot configs for ASF. You can edit ASF configs using any text editor you want, I suggest **[Notepad++](https://notepad-plus-plus.org)**. Alternatively, you can use ASF graphical config generator to make that a little bit easier.

---

## Types

Every config property has it's type. Type of the property defines values that are valid for it. You can only use values that are valid for given type.

Types used by ASF are native C# types, which are specified below:

```bool``` - Boolean type accepting only ```true``` and ```false``` values.

```byte``` - Byte type, accepting only numbers from ```0``` to ```255``` (inclusive)

```ushort``` - Unsigned short type, accepting only numbers from ```0``` to ```65535``` (inclusive)

```uint``` - Unsigned integer type, accepting only numbers from ```0``` to ```4294967295``` (inclusive)

```ulong``` - Unsigned long integer type, accepting only numbers from ```0``` to ```18446744073709551615``` (inclusive)

```string``` - String type, accepting any sequence of characters, including empty sequence and ```null```. **Notice:** Remember that strings should be contained in quotes ```""```, unless you're using ```null``` value. Also keep in mind that you need to escape some special characters if your string contains them - use ```\"``` instead of ```"``` and ```\\``` instead of ```\```. That applies to manual way of editing configs, if you're using graphical config generator then program automatically does everything for you, just input your strings.

```HashSet<uint>``` - Collection (set) of unique unsigned integers, separated by a comma.

```flags``` - Flags attribute combines several different properties into one final value by applying bitwise operations. This allows you to choose any possible combination of various different allowed values at the same time. The final value is constructed as a sum of values of all enabled options.

For example, given following values:

Value | Name
--- | ---
0 | None
1 | A
2 | B
4 | C

Using ```B + C``` would result in value of ```6```, using ```A + C``` would result in value of ```5```, using ```C``` would result in value of ```4``` and so on. This allows you to create any possible combination of enabled values - if you decided to enable all of them, making ```None + A + B + C```, you'd get value of ```7```. Also notice that flag with value of ```0``` is enabled by definition in all other available combinations, therefore very often it's a flag that doesn't enable anything specifically (such as ```None```).

*Pro tip: To avoid all that mess and calculating final value yourself, just use ConfigGenerator and pick appropriate checkboxes, it's much easier!*

---

## Global config

Global config is located in ```ASF.json``` file and has following structure:

```
{
  "Debug": false,
  "Headless": false,
  "AutoUpdates": true,
  "AutoRestart": true,
  "UpdateChannel": 1,
  "SteamProtocol": 6,
  "SteamOwnerID": 0,
  "MaxFarmingTime": 10,
  "IdleFarmingPeriod": 3,
  "FarmingDelay": 15,
  "LoginLimiterDelay": 10,
  "InventoryLimiterDelay": 3,
  "GiftsLimiterDelay": 1,
  "MaxTradeHoldDuration": 15,
  "ForceHttp": false,
  "HttpTimeout": 60,
  "WCFHost": "127.0.0.1",
  "WCFPort": 1242,
  "Statistics": true,
  "Blacklist": [
    267420,
    303700,
    335590,
    368020,
    425280,
    480730,
    566020
  ]
}
```

**Tip:** Unless you want to change any of those options, you're good to go with leaving everything at default values, therefore you can close ```ASF.json``` and proceed to bot config.

---

All options are explained below:

```Debug``` - ```bool``` type with default value of ```false```. This property defines if process should run in debug mode. When in debug mode, ASF creates a special ```debug``` directory in the place of the executable, which keeps track of whole communication between ASF and Steam servers. Debug information can help spotting nasty issues related to networking. In addition to that, some program routines will be more verbose, such as ```WebBrowser``` stating exact reason why some requests are failing - those entries are written to normal ASF log. **You should not run ASF in Debug mode, unless asked by developer**. Running ASF in debug mode decreases performance and should not be used, unless you've been asked by developer to record a debug log. **Notice:** ```debug``` directory consists of **sensitive** information such as the password you're using for logging in to steam. You should not post content of your ```debug``` directory in any public location, ASF developer should always remind you of sending it to his e-mail.

```Headless``` - ```bool``` type with default value of ```false```. This property defines if process should run in headless mode. When in headless mode, ASF assumes that it's running on a server, therefore it will not attempt to read crucial account credentials such as 2FA code, SteamGuard code, password or any other variable required for ASF to operate. This mode is useful for users running ASF on their servers, as instead of asking e.g. for 2FA code, ASF will silently abort the operation by stopping an account. Unless you're running ASF on a server, and you previously confirmed that ASF is able to operate in non-headless mode, you should keep this property disabled. Any user interaction will be denied when in headless mode, and your accounts will not run if they require your "helpful hand" during starting. This is useful for servers, as ASF can abort trying to log into the account when asked for credentials, instead of waiting (infinitely) for user to provide those. If you're not sure how to set this property, leave it with default value of ```false```.

```AutoUpdates``` - ```bool``` type with default value of ```true```. This property defines if ASF should automatically update itself when new version is available. Updates are crucial not only to receive new features, but also to receive bugfixes, performance enhancements, stability improvements and more. When enabled, ASF will automatically download, replace, and restart itself when new update is available. In addition to initial version check on startup, ASF will also check every 24 hours if new update is available. Update process of ASF always includes only replacement of core executable file (ASF.exe) - it never touches any configs or other database files. Unless you have **strong** reason to disable this feature, you should keep it enabled.

```AutoRestart``` - ```bool``` type with default value of ```true```. This property defines if ASF is allowed to perform a self-restart when needed. For now the only event which might cause ASF a need of restart is auto-update defined above, but that may change in future. Typically, restart includes two parts - creating new process, and exiting current one. Most users should be fine with it and keep this property with default value of ```true```, however - if you're running ASF through your own script and/or with Mono, you might want to have full control over starting the process, and avoid a situation such as having new (restarted) ASF process running somewhere silently in the background, and not in the foreground of script. If that's the case, this property if specially for you and you can set it to ```false```. However, keep in mind that in such case **you** are responsible for restarting the process. This is somehow important as ASF will only exit instead of spawning new process (e.g. after update), so if there is no logic added by you, it'll simply stop working until you start it again. ASF always exits with proper error code indicating success (zero) or non-success (non-zero), this way you're able to add proper logic in your script which should avoid auto-restarting ASF in case of failure, or at least make a local copy of ```log.txt``` for further analysis. Example script is provided in **[Mono](https://github.com/JustArchi/ArchiSteamFarm/wiki/Mono#usage)** section. Also keep in mind that ```!restart``` command will always restart ASF regardless of how this property is set, as this property defines default behaviour, while ```!exit``` and ```!restart``` commands always work the same way. Unless you have a reason to disable this feature, you should keep it enabled.

```UpdateChannel``` - ```byte``` type with default value of ```1```. This property defines update channel which is being used, either for auto-updates (if ```AutoUpdates``` is ```true```), or update notifications (otherwise). Currently ASF supports three update channels - ```0``` which is called ```None```, ```1```, which is called ```Stable```, and ```2```, which is called ```Experimental```. ```Stable``` channel is the default release channel, which should be used by majority of users. ```Experimental``` channel in addition to ```Stable``` releases, also includes **pre-releases** dedicated for advanced users and other developers in order to test new features, confirm bugfixes or give feedback about planned enhancements. **Experimental versions often contain unpatched bugs, work-in-progress features or rewritten implementations**. If you don't consider yourself advanced user, please stay with default ```1``` (Stable) update channel. ```Experimental``` channel is dedicated to users who know how to report bugs, deal with issues and give feedback - no technical support will be given. Check out ASF **[release cycle](https://github.com/JustArchi/ArchiSteamFarm/wiki/Release-cycle)** if you'd like to learn more. You can also set ```UpdateChannel``` to ```0``` (None), if you want to completely remove all version checks, although this is not recommended, unless for some reason you don't want to even receive notifications about new versions.

```SteamProtocol``` - ```byte``` type with default value of ```6```. This property defines network protocol that will be used for built-in steam client being used by ASF. Currently only 2 values are supported - ```6``` which specifies ```TCP``` protocol, and ```17``` which specifies ```UDP``` protocol. Using any other value will result in using default value of ```6```. Switching from ```TCP``` to ```UDP``` might be useful if you're trying to work around some kind of firewall, or you're trying to set up a proxy. UDP steam protocol is currently **EXPERIMENTAL**, and **[contains bugs](https://github.com/JustArchi/ArchiSteamFarm/issues/186)**, so use it at your own risk. Unless you have a **strong** reason to edit this property, you should keep it at default.

```SteamOwnerID``` - ```ulong``` type with default value of ```0```. This property is similar to ```SteamMasterID``` property of given bot instance, but instead - it specifies steamID of the owner of the ASF process, which is - **you**. ```SteamMasterID``` has full control over his bot instance, but global commands such as ```!exit```, ```!restart``` or ```!update``` are reserved for ```SteamOwnerID``` only. This is convenient, as you might want to run bots for your friends, while not allowing them to control ASF process, such as exiting it via ```!exit``` command. Default value of ```0``` specifies that there is no owner of ASF process, which means that nobody will be able to issue global ASF commands. Keep in mind that **[WCF](https://github.com/JustArchi/ArchiSteamFarm/wiki/WCF)** works with ```SteamOwnerID```, so if you want to use it you must provide a valid value here, which will be also the same on client machine.

```MaxFarmingTime``` - ```byte``` type with default value of ```10```. As you should know, Steam is not always working properly, sometimes weird situations can happen such as steam not being recording our playtime, despite of in fact playing a game. ASF will allow farming a single game in solo mode for maximum of ```MaxFarmingTime``` hours, and consider it fully farmed after that period. This is required to not freeze farming process in case of weird situations happening, but also if for some reason Steam released a new badge that would stop ASF from progressing further (See: ```Blacklist```). Default value of ```10``` hours should be enough for dropping all steam cards from one game. Setting this property too low can result in valid games being skipped (and yes, there are valid games taking even up to 9 hours to farm), while setting it too high can result in farming process being frozen. Unless you have a **strong** reason to edit this property, you should keep it at default.

```IdleFarmingPeriod``` - ```byte``` type with default value of ```3```. When ASF has nothing to farm, it will periodically check every ```IdleFarmingPeriod``` hours if perhaps account got some new games to farm. Value of 0 disables this feature. Also check: ```ShutdownOnFarmingFinished```.

```FarmingDelay``` - ```byte``` type with default value of ```15```. In order for ASF to work, it will check currently farmed game every ```FarmingDelay``` minutes, if it perhaps dropped all cards already. Setting this property too low can result in excessive amount of steam requests being sent, while setting it too high can result in ASF still "farming" given title for up to ```FarmingDelay``` minutes after it's fully farmed. Default value should be excellent for most users, but if you have many bots running, you might consider increasing it to something like ```30``` minutes in order to limit steam requests being sent. It's nice to note that ASF uses event-based mechanism and checks game badge page on each Steam item dropped, so in general **we don't even need to check it in fixed time intervals**, but as we don't fully trust Steam network - we check game badge page anyway, if we didn't check it through card being dropped event in last ```FarmingDelay``` minutes (in case Steam network didn't inform us about item dropped or stuff like that). Assuming that Steam network is working properly, decreasing this value **will not improve farming efficiency in any way**, while **increasing network overhead significantly** - it's recommended only to increase it (if needed) from default of ```15``` minutes. Unless you have a **strong** reason to edit this property, you should keep it at default.

```LoginLimiterDelay``` - ```byte``` type with default value of ```10```. Steam network in general includes some login requests limitations, that also mean too many (valid) logins in a short time. ASF will ensure that there will be at least ```LoginLimiterDelay``` seconds in between of two consecutive connection attempts (with an exception of 2FA-logins, as they need to be handled ASAP). Default value of ```10``` was set based on connecting over 100 bot instances, and should satisfy most (if not all) of the users. You may however want to decrease it, or even change to ```0``` if you have very low amount of bots, so ASF will ignore the delay and connect to Steam much faster. Be warned though, as setting it too low while having too many bots **will** result in Steam temporarily banning your IP, and that will prevent you from logging in **at all**, with ```InvalidPassword``` error - and that also includes your normal Steam client, not only ASF. Unless you have a **strong** reason to edit this property, you should keep it at default.

```InventoryLimiterDelay``` - ```byte``` type with default value of ```3```. Similar to above, fetching steam inventory is also rate-limited and some delay should be put between each inventory request. ASF will ensure that there will be at least ```InventoryLimiterDelay``` seconds in between of two consecutive inventory requests. Default value of ```3``` was set based on looting over 100 bot instances, and should satisfy most (if not all) of the users. You may however want to decrease it, or even change to ```0``` if you have very low amount of bots, so ASF will ignore the delay and loot steam inventories much faster. Be warned though, as setting it too low **will** result in Steam temporarily banning your IP, and that will prevent you from fetching your inventory at all. Unless you have a **strong** reason to edit this property, you should keep it at default.

```GiftsLimiterDelay``` - ```byte``` type with default value of ```1```. Similar to ```LoginLimiterDelay``` and ```InventoryLimiterDelay```, this specifies minimum amount of seconds between two consecutive gift handling requests. Unless you have a **strong** reason to edit this property, you should keep it at default.

```MaxTradeHoldDuration``` - ```byte``` type with default value of ```15```. This property defines maximum duration of trade hold in days that we're willing to accept - ASF will reject trades that are being hold for more than ```MaxTradeHoldDuration``` days. This option makes sense only for bots with ```TradingPreferences``` of ```SteamTradeMatcher```, as it doesn't affect ```SteamMasterID```/```SteamOwnerID``` trades, neither donations. Trade holds are annoying for everyone, and nobody really wants to deal with them. ASF is supposed to work on liberal rules and help everyone, regardless if on trade hold or not - that's why this option is set to ```15``` by default. However, if you'd instead prefer to reject all trades affected by trade holds, you can specify ```0``` here. Please consider the fact that cards with short lifespan are not affected by this option and automatically rejected for people with trade holds, as described in **[Trading](https://github.com/JustArchi/ArchiSteamFarm/wiki/Trading)** section, so there is no need to globally reject everybody only because of that. Unless you have a reason to edit this property, you should keep it at default.

```ForceHttp``` - ```bool``` type with default value of ```false```. By default ASF is trying to make use of secure ```https``` protocol whenever possible, which should be the preferred way of using it. However, in some rare situations, you may want to switch from ```https``` back to ```http```, which has lower overhead and is more compatible. You can do that by switching ```ForceHttp``` to ```true```. Using this option does not guarantee all requests being sent by ASF to be ```http```, as some services ASF is using (such as GitHub api) are supporting ```https``` only. In such cases, where there is no way to use ```http``` for such services, ASF will simply refuse to send a ```https``` request, which will result in some requests failing. If you're not debugging your network traffic, it is highly recommended to keep using secure and encrypted ```https```, instead of insecure and unencrypted ```http```. Unless you have a **strong** reason to edit this property, you should keep it at default. **We do plan removing this option entirely in the future**.

```HttpTimeout``` - ```byte``` type with default value of ```60```. This property defines timeout for HTTP(S) requests sent by ASF, in seconds. Default value of ```60``` should be fine for majority of people, however, if you have rather slow network connection or PC, you might want to increase this number to something higher (like ```90```). Keep in mind that bigger values will not magically fix slow or even inacessible steam servers, there is a moment when we must simply accept the fact that steam server is not responding and try again later. Setting this value too high will result in waiting for something that will not happen, and decrease overall performance instead of accepting the fact of request timing out and retrying. Setting this value lower has no advantage in general, and can even decrease overall stability, as Steam servers tend to be slow from time to time, and might require more time for parsing ASF requests. Unless you have a **strong** reason to edit this property, you should keep it at default.

```WCFHost``` - ```string``` type with default value of ```"127.0.0.1"```. This is a host, also known as "bind address", used by **[WCF](https://github.com/JustArchi/ArchiSteamFarm/wiki/WCF)**. This property makes sense only when WCF is enabled. ASF by default listens only on ```127.0.0.1``` address to ensure that no other machine but your own can access it. This is a security measure, as accessing WCF interface can lead to attacker taking over your ASF process, which can have dramatic effects. However, if you know what you're doing, e.g. you will restrict access to WCF yourself, using something like ```iptables``` or another form of firewall, you may change this property (at your own risk) to something less restrictive, such as ```0.0.0.0``` which enables WCF on all network interfaces. Remember that this property should be properly configured for both ```server``` and ```client``` machines (if they're not the same). In addition to that, you can use a value of ```null```, which will cause ASF to ask you about that property on each startup (which might be useful security measure if you don't want to expose IP of your server). Unless you have a **strong** reason to edit this property, you should keep it at default.

```WCFPort``` - ```ushort``` type with default value of ```1242```. This is the port on which **[WCF](https://github.com/JustArchi/ArchiSteamFarm/wiki/WCF)** is running by default. You may want to change it to any port you want, suggested ports are above ```1024```, as ports ```0-1024``` typically require ```root``` priviledges on Unix-like operating systems. Remember that this property should be the same on both ```server``` and ```client``` machines (if they're not the same). Unless you have a reason to edit this property, you should keep it at default.

```Statistics``` - ```bool``` type with default value of ```true```. This property defines if ASF should have statistics enabled. Detailed explanation what exactly this option does is available in **[Statistics](https://github.com/JustArchi/ArchiSteamFarm/wiki/Statistics)** section. Unless you have a reason to edit this property, you should keep it at default.

```Blacklist``` - ```HashSet<uint>``` type with default value of ```267420, 303700, 335590, 368020, 425280, 480730, 566020``` appIDs. Unfortunately Steam loves to flag summer/winter sale badges as "available for cards drop", which confuses ASF process by making it believe that it's a valid game that should be farmed. If there was no any kind of blacklist, ASF would eventually "hang" at farming a game which is in fact not a game, and wait infinitely for cards drop that will not happen. ASF blacklist serves a purpose of marking those badges as not available for farming, so we can silently ignore them when deciding what to farm, and not fall into the trap. ASF includes two blacklists by default - ```GlobalBlacklist```, which is hardcoded into the ASF code and not possible to edit, and normal ```Blacklist```, which is defined here. ```GlobalBlacklist``` is updated together with ASF version, therefore it's nice to note that if you're using up-to-date ASF then you do not need to maintain your own ```Blacklist``` which is defined here. The only purpose of this property is to allow you blacklisting new, not-known at the time of ASF release appIDs, which should not be farmed. Hardcoded ```GlobalBlacklist``` is being updated as fast as possible, therefore you're not required to update your own ```Blacklist``` if you're using latest ASF version, but without ```Blacklist``` you'd be forced to update ASF in order to "keep running" when Valve releases new sale badge - I don't want to force you to use latest ASF code, therefore this property is here to allow you "fix" ASF yourself if you for some reason don't want to update to new hardcoded ```GlobalBlacklist``` in new ASF release, yet you want to keep your old ASF running. Unless you have a **strong** reason to edit this property, you should keep it at default.

---

## Bot config

As you should know already, every bot should have it's own config. Example bot config is included in ```example.json``` file, which should be used for bot configuration. Simply **copy paste** ```example.json``` to a new file, and remember to name it appropriately, as it will be your bot instance. You should start from configuring your **primary** account, so some good suggestions for filename is ```primary.json```, ```1.json``` or ```YourNickname.json```.

**Notice:** There are several names which are reserved and can't be used for bot configs. Those are: **ASF**, **example** and **minimal**. ASF will ignore such configuration files.

 After deciding how you want to name your bot, open it's file, and start with configuration. You should notice following structure:

```
{
  "Enabled": false,
  "Paused": false,
  "SteamLogin": null,
  "SteamPassword": null,
  "PasswordFormat": 0,
  "SteamParentalPIN": "0",
  "SteamApiKey": null,
  "SteamMasterID": 0,
  "SteamMasterClanID": 0,
  "CardDropsRestricted": true,
  "DismissInventoryNotifications": true,
  "FarmingOrder": 0,
  "FarmOffline": false,
  "HandleOfflineMessages": false,
  "AcceptGifts": false,
  "IsBotAccount": false,
  "ForwardKeysToOtherBots": false,
  "DistributeKeys": false,
  "ShutdownOnFarmingFinished": false,
  "SendOnFarmingFinished": false,
  "SteamTradeToken": null,
  "SendTradePeriod": 0,
  "TradingPreferences": 1,
  "AcceptConfirmationsPeriod": 0,
  "CustomGamePlayedWhileFarming": null,
  "CustomGamePlayedWhileIdle": null,
  "GamesPlayedWhileIdle": [ ]
}
```

**Tip:** In order for bot to work properly, you should edit at least ```Enabled```, ```SteamLogin``` and ```SteamPassword``` properties. I also suggest to take a look at some fine-tuning such as ```CardDropsRestricted```, but all of that is optional. ASF configs are quite advanced to allow you tune your bots and ASF however you want, if you don't "require" such advanced setup, you don't really have to go deep into each config property. It's up to you how simple or how complex ASF should be.

---

All options are explained below:

```Enabled``` - ```bool``` type with default value of ```false```. This property defines if bot is enabled. Enabled bot instance (```true```) will automatically start on ASF run, while disabled bot instance (```false```) will need to be ```!start```ed manually. By default every bot is disabled, so you probably want to switch this property to ```true``` for all of your bots that should be started automatically.

```Paused``` - ```bool``` type with default value of ```false```. This property defines initial state of ```CardsFarmer``` module. With default value of ```false```, bot will automatically start farming when it's started, either because of ```Enabled``` or ```!start``` command. Switching this property to ```true``` should be done only if you want to manually ```!resume``` automatic farming process, for example because you want to use ```!play``` all the time and never use automatic ```CardsFarmer``` module - this works exactly the same as ```!pause^``` **[command](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)**. If you're unsure whether you want this feature enabled or not, keep it with default value of ```false```.

```SteamLogin``` - ```string``` type with default value of ```null```. This property defines your steam login - the one you use for logging in to steam. In addition to defining steam login here, you may also keep default value of ```null``` if you want to enter your steam login on each ASF startup instead of putting it in the config. This may be useful for you if you don't want to save sensitive data in config file.

```SteamPassword``` - ```string``` type with default value of ```null```. This property defines your steam password - the one you use for logging in to steam. In addition to defining steam password here, you may also keep default value of ```null``` if you want to enter your steam password on each ASF startup instead of putting it in the config. This may be useful for you if you don't want to save sensitive data in config file.

```PasswordFormat``` - ```byte``` type with default value of ```0```. This property defines format of ```SteamPassword``` property above, and currently supports - ```0``` for ```PlainText```, ```1``` for ```AES``` and ```2``` for ```ProtectedDataForCurrentUser```. Please refer to **[Security](https://github.com/JustArchi/ArchiSteamFarm/wiki/Security)** section if you want to learn more, as you'll need to ensure that ```SteamPassword``` property defined above indeed includes password in matching ```PasswordFormat```. Unless you know what you're doing, you should keep it with default value of ```0```.

```SteamParentalPIN``` - ```string``` type with default value of ```"0"```. This property defines your steam parental PIN. ASF requires accessing to resources protected by steam parental, therefore if you use that feature, you need to provide ASF with parental unlock PIN, so it can operate normally. Default value of ```"0"``` means that there is no steam parental PIN required to unlock this account, and this is probably what you want if you don't use steam parental functionality. In addition to defining steam parental PIN here, you may also use value of ```null``` if you want to enter your steam parental PIN on each ASF startup instead of putting it in the config. This may be useful for you if you don't want to save sensitive data in config file.

```SteamApiKey``` - ```string``` type with default value of ```null```. This property defines your Steam web API key. ASF makes use of API key in various ways, for example in receiving and declining trade offers, therefore you need to put your Steam web API key if you want to use that functionality. You can generate your API key by clicking **[here](https://steamcommunity.com/dev/apikey)**, logging in (if needed), and then clicking appropriate button. Domain is not important for ASF, you can put any domain you want. Remember that steam API key is valid only for the account it was generated for, therefore you can't use API key of Bot1 on Bot2 and vice versa - every bot should have it's own API key. API key is not mandatory, and ASF will work properly with default value of ```null``` API key, however in this case it won't be possible to use some of ASF functions that require the key, such as receiving and declining trade offers. If you can generate and use API key for your account, then it's highly recommended to use it, as the key is not only being used for trades, but also for enabling other features and enhancing existing ones, such as using more optimized way of ```!owns``` command that can work with private profiles instead of relying on Steam community only.

```SteamMasterID``` - ```ulong``` type with default value of ```0```. This property defines steamID in 64-bit form of bot master, which is (usually) you. Bot master has access to executing ASF commands that relates to the bot. If you're configuring ```primary``` account, and you're not going to execute any commands, then you don't need to put anything here and you can leave it with default value of ```0```. When you're configuring an ```alt``` account, or you're going to execute commands that relate to this bot instance, this property defines steamID which is allowed to control the bot. ASF will also accept friend requests, chat invites and all trades sent by ```SteamMasterID```. This is also the steamID to which ASF is sending items through ```!loot``` **[command](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)**. You can check your steamID in 64-bit form e.g. **[here](https://steamrep.com/)**, by logging in and navigating to your profile. The example steamID we're looking for looks like this: ```76561198006963719```.

```SteamMasterClanID``` - ```ulong``` type with default value of ```0```. This property is similar to above, but it defines the steamID of the steam group that bot should automatically join, including group chat. You can check steamID of your group by navigating to it's **[page](http://steamcommunity.com/groups/ascfarm)**, then adding ```/memberslistxml/?xml=1``` to the end of the link, so the link will look like **[this](http://steamcommunity.com/groups/ascfarm/memberslistxml/?xml=1)**. Then you can get steamID of your group from the result, it's in ```<groupID64>``` tag. In above example it would be ```103582791440160998```. If you don't have any "farm group" for your bots, you should keep it at default.

```CardDropsRestricted``` - ```bool``` type with default value of ```true```. This property defines if account has card drops restricted. Restricted card drops means that account is not receiving any card drops from given game until the game is played for at least 2 hours. Unfortunately there is no magical way to detect that, so ASF relies on you. This property affects **[cards farming algorithm](https://github.com/JustArchi/ArchiSteamFarm/wiki/Performance)** that will be used. Setting this property properly will maximize profits and minimize time required for cards to be farmed. Remember that there is **no obvious answer** whether you should set this to ```true``` or ```false```. If you're not sure if your account is restricted or not, I suggest to set it to the value of ```false```, then if you notice that no cards are dropping until game reaches 2 hours of playtime, consider switching it to ```true``` in order to speedup farming. It seems that older accounts which never asked for refund have **unrestricted card drops**, while new accounts and those who did ask for refund have **restricted card drops**. This is however only theory, and should not be taken as a rule.

```DismissInventoryNotifications``` - ```bool``` type with default value of ```true```. Every card drop triggers inventory notification - steam notification telling you that you received new items. This can get annoying pretty fast, and serves little to no purpose, therefore ASF by default automatically dismisses those notifications. If you for some reason would like to still receive and manually mark those notifications as read, consider switching this option to ```false```. It's nice to note that this option affects all item drops - including items you obtained through trading, and not only card drops.

```FarmingOrder``` - ```byte``` type with default value of ```0```. This property defines the **preferred** farming order of ASF. There are currently 9 farming orders available:

Value | Name  | Description
--- | --- | ---
0 | Unordered | No sorting, slightly improving performance
1 | AppIDsAscending | Try to farm games with lowest ```appID```s first
2 | AppIDsDescending | Try to farm games with highest ```appID```s first
3 | CardDropsAscending | Try to farm games with lowest number of card drops remaining first
4 | CardDropsDescending | Try to farm games with highest number of card drops remaining first
5 | HoursAscending | Try to farm games with lowest number of hours played first
6 | HoursDescending | Try to farm games with highest number of hours played first
7 | NamesAscending | Try to farm games in alphabetical order, starting from A
8 | NamesDescending | Try to farm games in reverse alphabetical order, starting from Z

Notice the word "try" in all above descriptions - the actual order is heavily affected by selected **[cards farming algorithm](https://github.com/JustArchi/ArchiSteamFarm/wiki/Performance)** and sorting will affect only results that ASF considers same performance-wise. For example, in ```Simple``` algorithm, selected ```FarmingOrder``` should be entirely respected in current farming session (as every game is treated the same), while in ```Complex``` algorithm actual order is affected by hours and then sorted according to chosen ```FarmingOrder```. This will lead to different results, as post-2h games have higher priority over pre-2h ones. Therefore, this config property is only a **suggestion** that ASF will try to respect, as long as it doesn't affect performance negatively (in this case, ASF will prefer performance over ```FarmingOrder```).

```FarmOffline``` - ```bool``` type with default value of ```false```. Offline farming is extremely useful for primary accounts. As you should know, farming a game actually shows your steam status as "Playing game: XXX", which can be misleading to your friends, confusing them that you're playing a game while actually you're only farming it. Offline farming solves that issue - your account will never be shown as "in-game" when you're farming steam cards with ASF. This is possible thanks to the fact that ASF does not have to "sign in" to steamcommunity in order to work properly, so we're in fact playing those games, but in "semi-offline" mode. Keep in mind that played games using offline farming will still count towards your playtime, and show as "recently played" on your profile. Also, bots with ```FarmOffline``` feature enabled can't react to commands (directly), which is important if you decide to use that feature with alt accounts. See: ```HandleOfflineMessages```

```HandleOfflineMessages``` - ```bool``` type with default value of ```false```. When ```FarmOffline``` feature is enabled, bot is not able to receive commands from ```SteamMasterID``` in usual way, as it's not logged into steamcommunity. However, it can still receive offline messages, and send responses back, even without being logged in. If you use ```FarmOffline``` on your alt accounts, consider switching this property to ```true``` in order to still be able to send commands to your offline bots. Keep in mind that this feature is based on offline steam messages, and receiving them automatically marks them as read, therefore this option is NOT recommended for primary accounts, as ASF will be forced to read and mark all offline messages as received in order to listen for offline commands - this affects also offline messages from your friends. If you're unsure whether you want this feature enabled or not, keep it with default value of ```false```.

```AcceptGifts``` - ```bool``` type with default value of ```false```. When enabled, ASF will automatically accept and redeem all steam gifts received by the bot. This includes also gifts from users different than ```SteamMasterID```. This option is recommended only for alt accounts, as it's very likely that you don't want to automatically redeem all gifts sent to your primary account. Keep in mind that gifts sent to e-mail address are not directly forwarded to the client, so ASF won't accept those gifts (without your help), therefore you should be sending steam gifts to your bots directly. ASF will accept (and redeem) only gifts that can be directly associated with the account. If you're unsure whether you want this feature enabled or not, keep it with default value of ```false```.

```IsBotAccount``` - ```bool``` type with default value of ```false```. This property defines if account used for this bot instance should be considered a primary one (```false```), or bot/alt one (```true```). ASF tries to be as much compatible with both types as possible, therefore switching this option to ```true``` for alts is not technically required for ASF to work, but doing so will allow ASF to tune the logic better for alt accounts. At the moment, it affects following things:

```Event``` | ```IsBotAccount: false``` | ```IsBotAccount: true```
--- | --- | ---
Invalid trades | Ignored | Rejected
Invalid friend/clan invites | Ignored | Rejected
```!loot``` | Cards (without foils) + Boosters | Cards (with foils) + Boosters

For example, invalid trades will be ignored on primary accounts, which allows you to decide yourself if you want to accept/decline them or not. On bot accounts, those trades will be immediately rejected, as there is nobody taking care of them.

Invalid friend invite is the one that doesn't come from ```SteamMasterID```. Likewise - invalid clan invite is the one that doesn't come from ```SteamMasterClanID```.

The logic might get extended in future releases if needed. If you're not sure how to set this property, leave it with default value of ```false```.

```ForwardKeysToOtherBots``` - ```bool``` type with default value of ```false```. Forwarding key is useful if you have at least 2 or more bots enabled in ASF. This option serves the purpose of "I have key X, I want to farm cards off it, and it doesn't matter for me on which bot the key will be redeemed". When you switch this property to ```true```, bot will firstly try to redeem key on it's own account, and if it fails for any reason (such as bot already owning the game), bot will forward the key to other bots, so they can redeem it instead. ASF will do it's best to ensure that key will be forwarded to bot that doesn't own it yet, but it's still a very fast way to reach ```OnCooldown``` status, therefore this option can be a nice addition if you don't want to check if all of your bots own given game, but it should not be overused. The actual redeeming order is alphabetical according to names of the bots, excluding bots that are not connected. ASF will do it's best to avoid pointless redeem tries, therefore it will not forward/distribute keys to bots that own the game already (first try is always needed in order to find out what is the game behind the key). If you're not sure how to set this property, leave it with default value of ```false```.

```DistributeKeys``` - ```bool``` type with default value of ```false```. When ```ForwardKeysToOtherBots``` is enabled, a bot receiving 3 keys will firstly try to redeem key on it's own account, then forward that key to other bots, until it gets redeemed, then proceed with second key in the same manner. However, you might want to alter this behaviour, so when you send 3 keys to given bot, it will use one on itself, second one on bot2, and third one on bot3. This can be useful if you have many keys for the same game and you want to "distribute" all of those keys to your bots. It can be faster and more successful way in this case, as bots have lower chance of getting ```OnCooldown```, as every bot is trying to redeem only one key (instead of all 3 of them). Keep in mind that ```ForwardKeysToOtherBots``` also affects this property, and most likely you want to use **only one** of those two, not both at the same time. If you however use both, bots in addition to receiving that one distributed key will also try to forward it further if they have the game already, which is basically only useful if you're distributing less keys than your bots count. The actual redeeming order is alphabetical according to names of the bots, excluding bots that are not connected. ASF will do it's best to avoid pointless redeem tries, therefore it will not forward/distribute keys to bots that own the game already (first try is always needed in order to find out what is the game behind the key). If you're not sure how to set this property, leave it with default value of ```false```.

```ShutdownOnFarmingFinished``` - ```bool``` type with default value of ```false```. ASF is "occupying" an account for the whole time of process being active. When given account is done with farming, ASF periodically checks it (every ```IdleFarmingPeriod``` hours), if perhaps some new games with steam cards were added in the meantime, so it can resume farming of that account without a need to restart the process. This is useful for majority of people, as ASF can automatically resume farming when needed. However, you may actually want to stop the process when given account is fully farmed, you can achieve that by setting this property to ```true```. When enabled, ASF will proceed with logging off when account is fully farmed, which means that it won't be periodically checked or occupied anymore. You should decide yourself if you prefer ASF to work on given bot instance for the whole time, or if perhaps ASF should stop it when farming process is done. If you're not sure how to set this property, leave it with default value of ```false```.

```SendOnFarmingFinished``` - ```bool``` type with default value of ```false```. When ASF is done with farming given account, it can automatically send steam trade containing everything farmed up to this point to ```SteamMasterID```, which is very convenient if you don't want to bother with trades yourself. This option works the same as ```!loot``` command, therefore keep in mind that it requires ```SteamMasterID``` properly set. You may also need to set ```SteamTradeToken```, which is described below. Account should also be eligible for trading, and you will need to manually confirm trade on your authenticator/e-mail, unless you're using **[ASF 2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)**, which does that automatically for you. In addition to initiating ```!loot``` after finishing farming, ASF will also initiate ```!loot``` on each new items notification (when not farming), and after completing each trade that results in new items (always) when this option is active. This is especially useful for "forwarding" items received from other people to our account. If you're not sure how to set this property, leave it with default value of ```false```.

```SteamTradeToken``` - ```string``` type with default value of ```null```. When you (```SteamMasterID```) have your bot on your friend list, then bot can send trade right away without worrying about trade token, therefore you can leave this property at default value of ```null```. If you however decide to NOT have your bot on your friend list, then you will need to generate and input trade token of ```SteamMasterID``` here. As logged in ```SteamMasterID```, navigate **[here](http://steamcommunity.com/id/me/tradeoffers/privacy)** and take a look at your trade URL. The token we're looking for is made out of 8 characters after ```&token=``` part in your trade URL. You should copy and put those 8 characters here, as ```SteamTradeToken```. Do not put whole trading URL, only token.

```SendTradePeriod``` - ```byte``` type with default value of ```0```. This property works very similar to ```SendOnFarmingFinished``` explained above, with one difference - instead of sending trade when farming is done, we can also send it every ```SendTradePeriod``` hours, regardless of how much we have to farm left. This is useful if you want to ```!loot``` your alt accounts on usual basis instead of waiting for it to finish farming. Default value of ```0``` disables this feature, if you want your bot to send you trade e.g. every day, you should put ```24``` here. If you're not sure how to set this property, leave it with default value of ```0```.

```TradingPreferences``` - ```byte flags``` type with default value of ```1```. This property defines ASF behaviour when in trading, and is defined as below:

Value | Name  | Description
--- | --- | ---
0 | None | No trading preferences
1 | AcceptDonations | Accepts trades in which we're not losing anything
2 | SteamTradeMatcher | Accepts dupes-matching **[STM](http://www.steamtradematcher.com/)**-like trades. Visit **[Trading](https://github.com/JustArchi/ArchiSteamFarm/wiki/Trading)** for more info
4 | MatchEverything | Requires ```SteamTradeMatcher``` to be set, and in combination with it - also accepts bad trades in addition to good and neutral ones

Please notice that this property is ```flags``` field, therefore it's possible to choose any combination of available values. Check out **[flags explanation](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#types)** if you'd like to learn more. Not enabling any of flags results in ```None``` option.

```AcceptConfirmationsPeriod``` - ```byte``` type with default value of ```0```. This property makes sense only if you have **[ASF 2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)** enabled for this account. Recently Valve introduced additional restrictions, and now every item listed on the market requires additional 2FA confirmation. This option works the same as ```!2faok``` command, ASF will automatically accept all pending confirmations every ```AcceptConfirmationsPeriod``` minutes. Default value of ```0``` disables this feature. In general it's not recommended to enable this option, but if you want to keep it enabled, you should use rather long period, such as ```30``` minutes. If you're not sure how to set this property, leave it with default value of ```0```.

```CustomGamePlayedWhileFarming``` - ```string``` type with default value of ```null```. When ASF is farming, it can display itself as "Playing non-steam game: ```CustomGamePlayedWhileFarming```" instead of currently farmed game. This can be useful if you would like to let your friends know that you're farming, yet you don't want to use ```FarmOffline``` feature. Please note that ASF cannot guarantee the actual display order of Steam network, therefore this is only a suggestion that may, or may not, display properly. Default value of ```null``` disables this feature.

```CustomGamePlayedWhileIdle``` - ```string``` type with default value of ```null```. Similar to above, but for the situation when ASF has nothing to do (as account is fully farmed). Default value of ```null``` disables this feature.

```GamesPlayedWhileIdle``` - ```HashSet<uint>``` type with default value of being empty. If ASF has nothing to farm it can play your specified steam games (```appID```s) instead. Playing games in such manner increases your "hours played" of those games, but nothing else apart of it. This feature can be enabled at the same time with ```CustomGamePlayedWhileIdle``` in order to play your selected games while showing custom status in Steam network, but in this case, like in ```CustomGamePlayedWhileFarming``` case, the actual display order is not guaranteed. Please note that Steam allows ASF to play only up to ```32``` ```appID```s, therefore if you put more games than that, only first ```32``` will be respected (and extra ones being ignored).

---

## Bot config (Minimalistic)

In addition to full bot config contained in ```example.json```, ASF also offers you ```minimal.json``` config, which can be used instead. Simply **copy paste** ```minimal.json``` to your filename of choice, and open it.

You should notice following structure:

```
{
  "Enabled": false,
  "SteamLogin": null,
  "SteamPassword": null
}
```

Now, as you can see - this config is pretty damn short compared to ```example.json```. This is because minimalistic config includes only properties that should be configured in order for bot to run. When given config property is not defined, such as ```CardDropsRestricted``` in above ```minimal.json```, it's the same as you'd define it with **it's default value**. This is useful for you if you want to keep your configs short and simple, as you don't need to include every property that ASF offers, but only redefine those which you want to change.

For example, if you have no intention of changing anything, then you're good to go only with such very short and simple config:

```
{
  "Enabled": true,
  "SteamLogin": "pablo32",
  "SteamPassword": "pass123"
}
```

But what if you want to change ```CardDropsRestricted``` from default value of ```true``` to ```false```? It's simple, just add that part from ```example.json``` and edit accordingly, so it looks like this:

```
{
  "Enabled": true,
  "SteamLogin": "pablo32",
  "SteamPassword": "pass123",
  "CardDropsRestricted": false
}
```

Remember to keep proper JSON structure - strings should be contained in ```""```, and there should be ```,``` at the end of each config property, but not the last one. If you're not sure if your config is proper, you can always **[validate it](http://jsonlint.com/)**.

---

## Compatibility

It's top priority for ASF to remain compatible with older configs. As you should already know, missing config properties are treated the same as they would be defined with their **default values**. Therefore, if new config property gets introduced in new version of ASF, all your configs will remain **compatible** with new version, and ASF will treat that new config property as it'd be defined with it's **default value**. You can always add, remove or edit config properties according to your needs, ```ASF.json``` will always include all currently supported global config properties, while ```example.json``` will always include all currently supported bot config properties for you to use. There's no need to "regenerate" configs when new property gets added, unless you want to switch it from it's default value to something else. Especially advanced users are encouraged to keep minimalistic file structure and define only those properties, in both global and bot configs, which they **require** to change, instead of copying entire ```example.json``` and changing only 3 variables. You can always add missing properties later, ```example.json``` is always available for you.

---

## Auto-reload

Starting with ASF V2.1.6.2+, the program is now aware of configs being modified "on-the-fly" - thanks to that, ASF will automatically:
- Create (and start, if needed) new bot instance, when you create it's config
- Stop (if needed) and remove old bot instance, when you delete it's config
- Stop (and start, if needed) any bot instance, when you edit it's config

All of the above is transparent and will be done automatically without a need of restarting the program, or killing other (unaffected) bot instances.

In addition to that, ASF will also restart itself (if ```AutoRestart``` permits) if you modify core ASF ```ASF.json``` config. Likewise, program will quit if you delete or rename it.