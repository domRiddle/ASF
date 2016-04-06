# Configuration

This page is dedicated for ASF configuration. It includes both file structure used by ASF, as well as fine-tuning ASF to your needs.

---

## Introduction

ASF configuration is divided into two major ports - global (process) configuration, and configuration of every bot. Bot is a single steam account that is taking part in ASF process. In order to work, ASF needs at least one **enabled** bot instance. There is no process-enforced limit of supported bot instances, so you can use as many steam accounts (bots) as you want.

Configuration can be done either manually - by creating proper JSON configs, or by using graphical config generator - **ASF-GUI.exe**, which should be much easier and convenient. Unless you're advanced user, I suggest using the config generator.

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

ASF is using **[JSON](https://en.wikipedia.org/wiki/JSON)** format for storing it's config files. It's human-friendly, readable and very universal format in which you can configure global and bot configs for ASF. You can edit ASF configs using any text editor you want, I suggest **[Notepad++](https://notepad-plus-plus.org)**.

---

## Types

Every config property has it's type. Type of the property defines values that are valid for it. You can only use values that are valid for given type.

Types used by ASF are native C# types, which are specified below:

```bool``` - Boolean type accepting only ```true``` and ```false``` values.

```byte``` - Byte type, accepting only numbers from ```0``` to ```255``` (inclusive)

```ushort``` - Unsigned short type, accepting only numbers from ```0``` to ```65535``` (inclusive)

```uint``` - Unsigned integer type, accepting only numbers from ```0``` to ```4294967295``` (inclusive)

```ulong``` - Unsigned long integer type, accepting only numbers from ```0``` to ```18446744073709551615``` (inclusive)

```string``` - String type, accepting any sequence of characters, such as ```"password"``` or ```"pablo32"``` and ```null```. **Notice:** Remember that strings should be contained in quotes ```""```, unless you're using ```null``` value. Also keep in mind that you need to escape some special characters if your string contains them - use ```\"``` instead of ```"``` and ```\\``` instead of ```\```.

```HashSet<uint>``` - Collection (set) of unique unsigned integers, accepting any number of them.

---

## Global config

We start configuring ASF by opening global config - ```ASF.json```. After opening the file, you should notice following structure:

```
{
  "Debug": false,
  "Headless": false,
  "AutoUpdates": true,
  "UpdateChannel": 1,
  "SteamProtocol": 6,
  "SteamOwnerID": 0,
  "MaxFarmingTime": 10,
  "IdleFarmingPeriod": 3,
  "FarmingDelay": 5,
  "AccountPlayingDelay": 5,
  "LoginLimiterDelay": 7,
  "InventoryLimiterDelay": 3,
  "ForceHttp": false,
  "HttpTimeout": 60,
  "WCFHostname": "localhost",
  "WCFPort": 1242,
  "LogToFile": true,
  "Statistics": true,
  "HackIgnoreMachineID": false,
  "Blacklist": [
    267420,
    303700,
    335590,
    368020,
    425280
  ]
}
```

**Tip:** Unless you want to change any of those options, you're good to go with leaving everything at default values, therefore you can close ```ASF.json``` and proceed to bot config.

---

All options are explained below:

```Debug``` - ```bool``` type with default value of ```false```. This property defines if process should run in debug mode. When in debug mode, ASF creates a special ```debug``` directory in the place of the executable, which keeps track of whole communication between ASF and Steam servers. Debug information can help spotting nasty issues related to networking. **You should not run ASF in Debug mode, unless asked by developer**. Running ASF in debug mode decreases performance and should not be used, unless you've been asked by developer to record a debug log. **Notice:** debug log consists of **sensitive** information such as the password you're using for logging in to steam. You shouldn't post your debug log in any public location, ASF developer should always remind you of sending it to his e-mail.

```Headless``` - ```bool``` type with default value of ```false```. This property defines if process should run in headless mode. When in headless mode, ASF assumes that it's running on a server, therefore it will not attempt to read crucial account credentials such as 2FA code, SteamGuard code, password or any other variable required for ASF to operate. This mode is useful for users running ASF on their servers, as instead of asking e.g. for 2FA code, ASF will silently abort the operation by stopping an account. Unless you you're running ASF on a server, and you previously confirmed that ASF is able to operate in non-headless mode, you should keep this property enabled. Any user interaction will be denied when in headless mode, and your accounts will not run if they require your "helpful hand" during starting.

```AutoUpdates``` - ```bool``` type with default value of ```true```. This property defines if ASF should automatically update itself when new version is available. Updates are crucial not only to receive new features, but also to receive bugfixes, performance enhancements, stability improvements and more. When enabled, ASF will automatically download, replace, and restart itself when new update is available. In addition to initial version check on startup, ASF will also check every 24 hours if new update is available. Update process of ASF always includes only replacement of core executable file (ASF.exe) - it never touches any configs or other database files. Unless you have **strong** reason to disable this feature, you should keep it enabled.

```UpdateChannel``` - ```byte``` type with default value of ```1```. This property defines update channel which is being used if ```AutoUpdates``` config property defined above is ```true```. Currently ASF supports two update channels - ```1```, which is called ```Stable``` and ```2```, which is called ```Experimental```. ```Stable``` channel is the default release channel, which should be used by majority of users. ```Experimental``` channel in addition to ```Stable``` releases, also includes **pre-releases** dedicated for advanced users and other developers in order to test new features, confirm bugfixes or give feedback about planned features. **Experimental versions often contain unpatched bugs, work-in-progress features or rewritten implementations**. If you don't consider yourself advanced user, please stay with default ```1``` (Stable) update channel. ```Experimental``` channel is dedicated to users who know how to report bugs, deal with issues and give feedback. No technical support will be given. You can also set ```UpdateChannel``` to ```0``` (Unknown), if you want to completely remove all version checks, although this is not recommended, unless for some reason you don't want to even receive notifications about new versions.

```SteamProtocol``` - ```byte``` type with default value of ```6```. This property defines network protocol that will be used for built-in steam client being used by ASF. Currently only 2 values are supported - ```6``` which specifies ```TCP``` protocol, and ```17``` which specifies ```UDP``` protocol. Using any other value will result in using default value of ```6```. Switching from ```TCP``` to ```UDP``` might be useful if you're trying to work around some kind of firewall, or you're trying to set up a proxy. UDP steam protocol is currently **EXPERIMENTAL**, use it at your own risk. Unless you have a **strong** reason to edit this property, you should keep it at default.

```SteamOwnerID``` - ```ulong``` type with default value of ```0```. This property is similar to ```SteamMasterID``` property of given bot instance, but instead - it specifies steamID of the owner of the ASF process, which is - **you**. ```SteamMasterID``` has full control over his bot instance, but global commands such as ```!exit```, ```!restart``` or ```!update``` are reserved for ```SteamOwnerID``` only. This is convenient, as you might want to run bots for your friends, while not allowing them to control ASF process, such as exiting it via ```!exit``` command. Default value of ```0``` specifies that there is no owner of ASF process, which means that nobody will be able to issue global ASF commands. Keep in mind that **[WCF](https://github.com/JustArchi/ArchiSteamFarm/wiki/WCF)** works with ```SteamOwnerID```, so if you want to use it you must provide a valid value here, which will be also the same on client machine.

```MaxFarmingTime``` - ```byte``` type with default value of ```10```. As you should know, Steam is not always working properly, sometimes weird situations can happen such as steam not being recording our playtime, despite of in fact playing a game. ASF will allow farming a single game in solo mode for maximum of ```MaxFarmingTime``` hours, and consider it fully farmed after that period. This is required to not freeze farming process in case of weird situations happening, but also if for some reason Steam released a new badge that would stop ASF from progressing further (See: ```Blacklist```). Default value of ```10``` hours should be enough for dropping all steam cards from one game. Setting this property too low can result in valid games being skipped (and yes, there are valid games taking even up to 9 hours to farm), while setting it too high can result in farming process being frozen. Unless you have a **strong** reason to edit this property, you should keep it at default.

```IdleFarmingPeriod``` - ```byte``` type with default value of ```3```. When ASF has nothing to farm, it will periodically check every ```IdleFarmingPeriod``` hours if perhaps account got some new games to farm. Value of 0 disables this feature. Also check: ```ShutdownOnFarmingFinished```.

```FarmingDelay``` - ```byte``` type with default value of ```5```. In order for ASF to work, it will check currently farmed game every ```FarmingDelay``` minutes, if it perhaps dropped all cards already. Setting this property too low can result in excessive amount of steam requests being sent, while setting it too high can result in ASF still "farming" given title for up to ```FarmingDelay``` minutes after it's fully farmed. Default value should be excellent for most users, but if you have many bots running, you might consider increasing it to something like ```15``` minutes in order to limit steam requests being sent. Unless you have a reason to edit this property, you should keep it at default.

```AccountPlayingDelay``` - ```byte``` type with default value of ```5```. When you decide to disconnect farming ASF by starting a game, ASF will try to resume farming, after waiting ```AccountPlayingDelay``` minutes. Remember that Steam limits number of allowed logins per hour, so you should not go too low unless you have a **very strong** reason. Default value of ```5``` minutes is very short and should satisfy most people, you may however want to increase the delay to e.g. ```15``` or ```30``` minutes, if you feel that ```5``` minutes checks are too intrusive. In addition to that, you can use a value of ```0``` which will cause ASF to shutdown bot instance immediately after noticing that account is being used elsewhere. Unless you have a reason to edit this property, you should keep it at default.

```LoginLimiterDelay``` - ```byte``` type with default value of ```7```. As noted above, Steam has some login requests limitations, that also includes too many logins in short time. ASF will ensure that there will be at least ```LoginLimiterDelay``` seconds in between of two consecutive logins (with an exception of 2FA-logins, as they need to be handled ASAP). Default value of ```7``` was set based on connecting over 100 bot instances, and should satisfy most (if not all) of the users. You may however want to decrease it, or even change to ```0``` if you have very low amount of bots, so ASF will ignore the delay and connect to Steam much faster. Be warned though, as setting it too low will result in Steam temporarily banning your IP, which will result in all logins failing with ```InvalidPassword``` error. Unless you have a **strong** reason to edit this property, you should keep it at default.

```InventoryLimiterDelay``` - ```byte``` type with default value of ```3```. Similar to above, fetching steam inventory is also rate-limited and some delay should be put between each inventory request. ASF will ensure that there will be at least ```InventoryLimiterDelay``` seconds in between of two consecutive inventory requests. Default value of ```3``` was set based on looting over 100 bot instances, and should satisfy most (if not all) of the users. You may however want to decrease it, or even change to ```0``` if you have very low amount of bots, so ASF will ignore the delay and loot steam inventories much faster. Be warned though, as setting it too low will result in Steam temporarily banning your IP, which will result in all inventory requests failing. Unless you have a **strong** reason to edit this property, you should keep it at default.

```ForceHttp``` - ```bool``` type with default value of ```false```. By default ASF is trying to make use of secure ```https``` protocol whenever possible, which should be the preferred way of using it. However, in some rare situations, you may want to switch from ```https``` back to ```http```, which has lower overhead and is more compatible. You can do that by switching ```ForceHttp``` to ```true```. Using this option does not guarantee all requests being sent by ASF to be ```http```, as some services ASF is using (such as GitHub api) are supporting ```https``` only. In such cases, where there is no way to use ```http``` for such services, ASF will simply refuse to send a ```https``` request, which will result in some requests failing. If you're not debugging your network traffic, it is highly recommended to keep using secure and encrypted ```https```, instead of insecure and unencrypted ```http```. Unless you have a **strong** reason to edit this property, you should keep it at default.

```HttpTimeout``` - ```byte``` type with default value of ```60```. This property defines timeout for HTTP(S) requests sent by ASF, in seconds. Default value of ```60``` should be fine for majority of people, however, if you have rather slow network connection or PC, you might want to increase this number to something higher (like ```90```). Unless you have a **strong** reason to edit this property, you should keep it at default.

```WCFHostname``` - ```string``` type with default value of ```"localhost"```. This is a hostname, also known as "bind address", used by **[WCF](https://github.com/JustArchi/ArchiSteamFarm/wiki/WCF)**. This property makes sense only when WCF is enabled. ASF by default listens only on ```"localhost"``` address to ensure that no other machine but your own can access it. This is a security measure, as accessing WCF interface can lead to attacker taking over your ASF process, which can have dramatic effects. However, if you know what you're doing, e.g. you will restrict access to WCF yourself, using something like ```iptables```, you may change this property (at your own risk) to something less restrictive, such as ```"0.0.0.0"``` which enables WCF on all network interfaces. Remember that this property should be properly configured for both ```server``` and ```client``` machines (if they're not the same). In addition to that, you can use a value of ```null```, which will cause ASF to ask you about that property on each startup (which might be useful security measure if you don't want to expose IP of your server). Unless you have a **strong** reason to edit this property, you should keep it at default.

```WCFPort``` - ```ushort``` type with default value of ```1242```. This is the port on which **[WCF](https://github.com/JustArchi/ArchiSteamFarm/wiki/WCF)** is running by default. You may want to change it to any port you want, suggested ports are above ```1024```, as ports ```0-1024``` typically require ```root``` priviledges on Unix-like operating systems. Remember that this property should be the same on both ```server``` and ```client``` machines (if they're not the same). Unless you have a reason to edit this property, you should keep it at default.

```LogToFile``` - ```bool``` type with default value of ```true```. This property defines if ASF should maintain ```log.txt``` with the log of it's latest run. As explained above, log file is useful for you as it allows you to analyze what ASF is doing, and it's also crucial if you decide to report some ASF bug. On the other hand, if you have many bot instances, it's possible that log file will grow big pretty fast, and instead of letting ASF to log "everything" to it's log, you might want to redirect standard output (```stdout```) somewhere else, e.g. to the ```grep``` filters of your choice. Unless you have a **strong** reason to edit this property, you should keep it at default.

```Statistics``` - ```bool``` type with default value of ```true```. This property defines if ASF should have statistics enabled. Statistics help ASF developers by providing them with information crucial to development cycle. If you want to see new versions coming up, bugs being fixed, and new features getting implemented, consider leaving it at default value of ```true```. Currently statistics include only very minimalistic information - accounts being used by ASF will automatically join our **[steam group](http://steamcommunity.com/groups/ascfarm)** and the group chat. Statistics are available for everyone, so you can check yourself how our steam group looks like, how many members it has, and how many bots are currently running (aka "on chat"). We're not gathering any other information, especially sensitive ones such as passwords, steam logins or even operating system you're using, and by keeping them enabled you help ASF developers - this way we can see how many users are using our program :+1:. Unless you have a reason to edit this property, you should keep it at default.

```HackIgnoreMachineID``` - ```bool``` type with default value of ```false```. This property acts as a workaround for broken GenerateMachineID() function in SK2. If ASF is "stuck" right after ```Connected to Steam!``` and ```Logging in...```, then you may need to enable this option. This option is a hack, and **will be removed as soon as GenerateMachineID() bug is fixed**. Refer to https://github.com/JustArchi/ArchiSteamFarm/issues/154 and https://github.com/SteamRE/SteamKit/issues/254 for more info. Unless you have a **strong** reason to edit this property, you should keep it at default.

```Blacklist``` - ```HashSet<uint>``` type with default value of ```267420, 303700, 335590, 368020, 425280``` appIDs. Unfortunately Steam loves to flag summer/winter sale badges as "available for cards drop", which confuses ASF process by making it believe that it's a valid game that should be farmed. If there was no blacklist, ASF would eventually "hang" at farming a game which is in fact not a game, and wait infinitely for cards drop that will not happen. ```Blacklist``` serves a purpose of marking those badges as not available for farming, so ASF can silently ignore them when deciding what to farm. ASF includes two blacklists by default - ```GlobalBlacklist```, which is hardcoded into the ASF process and not possible to edit, and normal ```Blacklist```, which is defined here. The only purpose of this property is to allow you blacklisting new, not-known at the time of ASF release appIDs, which should not be farmed. Hardcoded ```GlobalBlacklist``` is being updated as fast as possible, therefore you're not required to update your own ```Blacklist``` if you're using latest ASF version, but without ```Blacklist``` you'd be forced to update in order for ASF to "keep running" when Valve releases new sale badge, therefore this property is here to allow you "fix" ASF yourself if you for some reason don't want to update to new hardcoded ```GlobalBlacklist``` in new ASF release, yet you want your old ASF to keep running. Unless you have a **strong** reason to edit this property, you should keep it at default.

---

## Bot config

As you should know already, every bot should have it's own config. Example bot config is included in ```example.json``` file, which should be used for bot configuration. Simply **copy paste** ```example.json``` to a new file, and remember to name it appropriately, as it will be your bot instance. You should start from configuring your **primary** account, so some good suggestions for filename is ```primary.json```, ```1.json``` or ```YourNickname.json```. After deciding how you want to name your bot, open it's file, and start with configuration. You should notice following structure:

```
{
  "Enabled": false,
  "StartOnLaunch": true,
  "SteamLogin": null,
  "SteamPassword": null,
  "SteamParentalPIN": "0",
  "SteamApiKey": null,
  "SteamMasterID": 0,
  "SteamMasterClanID": 0,
  "CardDropsRestricted": false,
  "DismissInventoryNotifications": true,
  "FarmOffline": false,
  "HandleOfflineMessages": false,
  "AcceptGifts": false,
  "ForwardKeysToOtherBots": false,
  "DistributeKeys": false,
  "UseAsfAsMobileAuthenticator": false,
  "ShutdownOnFarmingFinished": false,
  "SendOnFarmingFinished": false,
  "SteamTradeToken": null,
  "SendTradePeriod": 0,
  "AcceptConfirmationsPeriod": 0,
  "CustomGamePlayedWhileIdle": null,
  "GamesPlayedWhileIdle": [
    0
  ]
}
```

**Tip:** In order for bot to work properly, you should edit at least ```Enabled```, ```SteamLogin``` and ```SteamPassword``` properties. I also suggest to take a look at some fine-tuning such as ```CardDropsRestricted```, but all of that is optional. ASF configs are quite advanced to allow you tune your bots and ASF however you want, if you don't "require" such advanced setup, you don't really have to go deep into each config property. It's up to you how simple or how complex ASF should be.

---

All options are explained below:

```Enabled``` - ```bool``` type with default value of ```false```. This property defines if bot is enabled. Being enabled does not mean that bot has to start and run, being enabled means that ASF should "notice" it being a valid defined and configured bot instance, which has a potential to ```Start()``` and ```Stop()```. This property allows you to easily switch bots on and off, without a need to remove config files. By default every bot is disabled, and if you want to use it in ASF, and that's probably what you want, you should switch this property to ```true```.

```StartOnLaunch``` - ```bool``` type with default value of ```true```. This property defines if bot should automatically start when it's instance is created by ASF. Usually you want to keep it at ```true```, but you may also want to keep bot ```Enabled```, yet ```Stopped```. If you decide to change this property to ```false```, you'll need to ```!start``` given bot instance manually on every ASF startup. Unless you have a reason to edit this property, you should keep it at default.

```SteamLogin``` - ```string``` type with default value of ```null```. This property defines your steam login - the one you use for logging in to steam. In addition to defining steam login here, you may also keep default value of ```null``` if you want to enter your steam login on each ASF startup instead of putting it in the config. This may be useful for you if you don't want to save sensitive data in config file.

```SteamPassword``` - ```string``` type with default value of ```null```. This property defines your steam password - the one you use for logging in to steam. In addition to defining steam password here, you may also keep default value of ```null``` if you want to enter your steam password on each ASF startup instead of putting it in the config. This may be useful for you if you don't want to save sensitive data in config file.

```SteamParentalPIN``` - ```string``` type with default value of ```"0"```. This property defines your steam parental PIN. ASF requires accessing to resources protected by steam parental, therefore if you use that feature, you need to provide ASF with parental unlock PIN, so it can operate normally. Default value of ```"0"``` means that there is no steam parental PIN required to unlock this account, and this is probably what you want if you don't use steam parental functionality. In addition to defining steam parental PIN here, you may also use value of ```null``` if you want to enter your steam parental PIN on each ASF startup instead of putting it in the config. This may be useful for you if you don't want to save sensitive data in config file.

```SteamApiKey``` - ```string``` type with default value of ```null```. This property defines your Steam web API key. ASF requires API key for receiving and declining trade offers, therefore you need to put your Steam web API key if you want to use that functionality. You can generate your API key by clicking **[here](https://steamcommunity.com/dev/apikey)**, logging in (if required), and then clicking appropriate button. Domain is not important for ASF, you can put any domain you want. Remember that steam API key is valid only for the account it was generated for, therefore you can't use API key of Bot1 on Bot2 and vice versa - every bot should have it's own API key. Keep in mind that this field is not mandatory, and ASF will work properly with default value of ```null``` API key, however in this case it won't be possible to use some of ASF functions that require the key, such as receiving and declining trade offers. If your account is restricted, then you won't be able to generate API key, but this is fine, as restricted accounts can't trade, and API key is used only for that part only.

```SteamMasterID``` - ```ulong``` type with default value of ```0```. This property defines steamID in 64-bit form of bot master, which is (usually) you. Bot master has access to executing ASF commands that relates to the bot. If you're configuring ```primary``` account, and you're not going to execute any commands, then you don't need to put anything here and you can leave it with default value of ```0```. When you're configuring an ```alt``` account, or you're going to execute commands that relate to this bot instance, this property defines steamID which is allowed to control the bot. ASF will also accept friend requests, chat invites and all trades sent by ```SteamMasterID```. This is also the steamID to which ASF is sending items through ```!loot``` **[command](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)**. You can check your steamID in 64-bit form e.g. **[here](https://steamrep.com/)**, by logging in and navigating to your profile. The example steamID we're looking for looks like this: ```76561198006963719```.

```SteamMasterClanID``` - ```ulong``` type with default value of ```0```. This property is similar to above, but it defines the steamID of the steam group that bot should automatically join, including group chat. You can check steamID of your group by navigating to it's **[page](http://steamcommunity.com/groups/ascfarm)**, then adding ```/memberslistxml/?xml=1``` to the end of the link, so the link will look like **[this](http://steamcommunity.com/groups/ascfarm/memberslistxml/?xml=1)**. Then you can get steamID of your group from the result, it's in ```<groupID64>``` tag. In above example it would be ```103582791440160998```. If you don't have any "farm group" for your bots, you should keep it at default.

```CardDropsRestricted``` - ```bool``` type with default value of ```false```. This property defines if account has card drops restricted. Restricted card drops means that account is not receiving any card drops from given game until the game is played for at least 2 hours. Unfortunately there is no magical way to detect that, so ASF relies on you. This property affects **[cards farming algorithm](https://github.com/JustArchi/ArchiSteamFarm/wiki/Performance)** that will be used. Setting this property properly will maximize profits and minimize time required for cards to be farmed. Remember that there is **no obvious answer** whether you should set this to ```true``` or ```false```. If you're not sure if your account is restricted or not, I suggest to keep default value of ```false```, then if you notice that no cards are dropping until game reaches 2 hours of playtime, consider switching it to ```true``` in order to speedup farming. It seems that older accounts which never asked for refund have **unrestricted card drops**, while new accounts and those who did ask for refund have **restricted card drops**. This is however only theory, and should not be taken as a rule.

```DismissInventoryNotifications``` - ```bool``` type with default value of ```true```. Every card drop triggers inventory notification - steam notification telling you that you received new items. This can get annoying pretty fast, and serves little to no purpose, therefore ASF by default automatically dismisses those notifications. If you for some reason would like to still receive and manually mark those notifications as read, consider switching this option to ```false```.

```FarmOffline``` - ```bool``` type with default value of ```false```. Offline farming is extremely useful for primary accounts. As you should know, farming a game actually shows your steam status as "Playing game: XXX", which can be misleading to your friends, confusing them that you're playing a game while actually you're only farming it. Offline farming solves that issue - your account will never be shown as "in-game" when you're farming steam cards with ASF. This is possible thanks to the fact that ASF does not have to "sign in" to steamcommunity in order to work properly, so we're in fact playing those games, but in "semi-offline" mode. Keep in mind that played games using offline farming will still count towards your playtime, and show as "recently played" on your profile. Also, bots with ```FarmOffline``` feature enabled can't react to commands (directly), which is important if you decide to use that feature with alt accounts. See: ```HandleOfflineMessages```

```HandleOfflineMessages``` - ```bool``` type with default value of ```false```. When ```FarmOffline``` feature is enabled, bot is not able to receive commands from ```SteamMasterID``` in usual way, as it's not logged into steamcommunity. However, it can still receive offline messages, and send responses back, even without being logged in. If you use ```FarmOffline``` on your alt accounts, consider switching this property to ```true``` in order to still be able to send commands to your offline bots. Keep in mind that this feature is based on offline steam messages, and receiving them automatically marks them as read, therefore this option is NOT recommended for primary accounts, as ASF will be forced to read and mark all offline messages as received in order to listen for offline commands - this affects also offline messages from your friends. If you're unsure whether you want this feature enabled or not, keep it with default value of ```false```.

```AcceptGifts``` - ```bool``` type with default value of ```false```. When enabled, ASF will automatically accept and redeem all steam gifts received by the bot. This includes also gifts from users different than ```SteamMasterID```. If bot already owns the game, he'll instead accept gift and add it to his inventory. This option is recommended only for alt accounts, as it's very likely that you don't want to automatically redeem all gifts sent to your primary account. Keep in mind that gifts sent to e-mail address are not directly forwarded to the client, so ASF won't accept those gifts (without your help), therefore you should be sending steam gifts to your bots directly. If you're unsure whether you want this feature enabled or not, keep it with default value of ```false```.

```ForwardKeysToOtherBots``` - ```bool``` type with default value of ```false```. Forwarding key is useful if you have at least 2 or more bots enabled in ASF. This option serves the purpose of "I have key X, I want to farm cards off it, and it doesn't matter for me on which bot the key will be redeemed". When you switch this property to ```true```, bot will firstly try to redeem key on it's own account, and if it fails for any reason (such as bot already owning the game), bot will forward the key to other bots, so they can redeem it instead. This option is useful for lazy people, who don't keep a track which bot owns what game, although it's a very fast way to trigger ```OnCooldown``` status, as Steam temporarily blocks accounts from key redeeming if it reaches given number of failures in short period of time. Therefore, this option can be a nice addition if you don't want to check if all of your bots own given game, but it should not be overused. If you're not sure how to set this property, leave it with default value of ```false```.

```DistributeKeys``` - ```bool``` type with default value of ```false```. When ```ForwardKeysToOtherBots``` is enabled, a bot receiving 3 keys will firstly try to redeem key on it's own account, then forward that key to other bots, until it gets redeemed, then proceed with second key in the same manner. However, you might want to alter this behaviour, so when you send 3 keys to given bot, it will use one on itself, second one on bot2, and third one on bot3. This can be useful if you have many keys for the same game and you want to "distribute" all of those keys to your bots. This can be faster and more successful way in this case, as bots have lower chance of getting ```OnCooldown```, as every bot is trying to redeem only one key (instead of all 3 of them). If you're not sure how to set this property, leave it with default value of ```false```.

```UseAsfAsMobileAuthenticator``` - ```bool``` type with default value of ```false```. This property defines if ASF should enable mechanism known as "ASF 2FA" for this account. As this feature is rather complicated, you can read more about it **[here](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)**. **This feature can potentially lock you out of your account, therefore it should NOT be used unless you fully understand what is the purpose of ASF 2FA**. Please read **[Escrow](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)** wiki page carefully before deciding to switch this to ```true```.

```ShutdownOnFarmingFinished``` - ```bool``` type with default value of ```false```. ASF is "occupying" an account for the whole time of process being active. When given account is done with farming, ASF periodically checks it (every ```IdleFarmingPeriod``` hours), if perhaps some new games with steam cards were added in the meantime, so it can resume farming of that account without a need to restart the process. This is useful for majority of people, as ASF can automatically resume farming when needed. However, you may actually want to stop the process when given account is fully farmed, you can achieve that by setting this property to ```true```. When enabled, ASF will proceed with logging off when account is fully farmed, which means that it won't be periodically checked or occupied anymore. When all ASF bots are stopped, ASF process will also exit as it has nothing to do. You should decide yourself if you prefer ASF to work on given bot instance for the whole time, or if perhaps ASF should stop it when farming process is done. If you're not sure how to set this property, leave it with default value of ```false```.

```SendOnFarmingFinished``` - ```bool``` type with default value of ```false```. When ASF is done with farming given account, it can automatically send steam trade containing everything farmed up to this point to ```SteamMasterID```, which is very convenient if you don't want to bother with trades yourself. This option works the same as ```!loot``` command, therefore keep in mind that it requires ```SteamMasterID``` properly set. You may also need to set ```SteamTradeToken```, which is described below. Account should also be eligible for trading, and you will need to manually confirm trade on your e-mail, unless you're using **[ASF 2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)**, which does that automatically for you.

```SteamTradeToken``` - ```string``` type with default value of ```null```. When you (```SteamMasterID```) have your bot on your friend list, then bot can send trade right away without worrying about trade token, therefore you can leave this property at default value of ```null```. If you however decide to NOT have your bot on your friend list, then you will need to generate and input trade token of ```SteamMasterID``` here. As logged in ```SteamMasterID```, navigate **[here](http://steamcommunity.com/id/me/tradeoffers/privacy)** and take a look at your trade URL. The token we're looking for is made out of 8 characters after ```&token=``` part in your trade URL. You should copy and put those 8 characters here, as ```SteamTradeToken```. Do not put whole trading URL, only token.

```SendTradePeriod``` - ```byte``` type with default value of ```0```. This property works very similar to ```SendOnFarmingFinished``` explained above, with one difference - instead of sending trade when farming is done, we can also send it every ```SendTradePeriod``` hours, regardless of how much we have to farm left. This is useful if you want to ```!loot``` your alt accounts on usual basis instead of waiting for it to finish farming. Default value of ```0``` disables this feature, if you want your bot to send you trade e.g. every day, you should put ```24``` here. If you're not sure how to set this property, leave it with default value of ```0```.

```AcceptConfirmationsPeriod``` - ```byte``` type with default value of ```0```. This property makes sense only if you have **[ASF 2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)** enabled for this account. Recently Valve introduced additional restrictions, and now every item listed on the market requires additional 2FA confirmation. This option works the same as ```!2faok``` command, ASF will automatically accept all pending confirmations every ```AcceptConfirmationsPeriod``` minutes. Default value of ```0``` disables this feature. In general it's not recommended to enable this option, but if you want to keep it enabled, you should use rather long period, such as ```30``` minutes. If you're not sure how to set this property, leave it with default value of ```0```.

```CustomGamePlayedWhileIdle``` - ```string``` type with default value of ```null```. When ASF is idle, which means that it has nothing to do (as account is fully farmed), it can display itself as "Playing non-steam game: ```CustomGamePlayedWhileIdle```". Default value of ```null``` disables this feature.

```GamesPlayedWhileIdle``` - ```HashSet<uint``` type with default value of ```0```. Similar to above, if ASF has nothing to farm it can play your specified steam games instead. Playing games in such manner increases your "hours played" of those games, but nothing else apart of it. Default value of ```0``` disables this feature.

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

But what if you want to add ```CardDropsRestricted``` to that config? It's simple, just copy that part from ```example.json```, so it looks like this:

```
{
  "Enabled": true,
  "SteamLogin": "pablo32",
  "SteamPassword": "pass123",
  "CardDropsRestricted": true
}
```

Remember to keep proper JSON structure - strings should be contained in ```""```, and there should be ```,``` at the end of each config property, but not the last one.

---

## Compatibility

It's top priority for ASF to remain compatible with older configs. As you should already know, missing config properties are treated the same as they would be defined with their **default values**. Therefore, if new config property gets introduced in new version of ASF, all your configs will remain **compatible** with new version, and ASF will treat that new config property as it'd be defined with it's **default value**. You can always add, remove or edit config properties according to your needs, ```example.json``` will always include all currently supported config properties for you to use.