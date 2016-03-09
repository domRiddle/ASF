# Configuration

This page is dedicated for ASF configuration. It includes both file structure used by ASF, as well as fine-tuning ASF to your needs.

---

## Introduction

ASF configuration is divided into two major ports - global (process) configuration, and configuration of every bot. Bot is a single steam account that is taking part in ASF process. In order to work, ASF needs at least one **enabled** bot instance. There is no process-enforced limit of supported bot instances, so you can use as many steam accounts (bots) as you want.

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

**Example: ** ASF has ```AutoUpdates``` switch defined as ```bool``` type, which can be only ```true (on)``` or ```false (off)```. It wouldn't make any sense if you put a value like "password" there, right?

Types used by ASF are native C# types, which are specified below:

```bool``` - Boolean type accepting only ```true``` and ```false``` values.

```byte``` - Byte type, accepting only numbers from ```0``` to ```255``` (inclusive)

```ushort``` - Unsigned short type, accepting only numbers from ```0``` to ```65535``` (inclusive)

```string``` - String type, accepting any sequence of characters, such as ```"password"``` or ```"pablo32"```

---

## Global config

```
{
  "Debug": false,
  "AutoUpdates": true,
  "UpdateChannel": 1,
  "HttpTimeout": 30,
  "AccountPlayingDelay": 5,
  "RequestLimiterDelay": 7,
  "WCFHostname": "localhost",
  "WCFPort": 1242,
  "Statistics": true,
  "Blacklist": [
    267420,
    303700,
    335590,
    368020,
    425280
  ]
}
```

```Debug``` - ```bool``` type with default value of ```false```. This property defines if process should run in debug mode. When in debug mode, ASF creates a special ```debug``` directory in the place of the executable, which keeps track of whole communication between ASF and Steam servers. Debug information can help spotting nasty issues related to networking. **You should not run ASF in Debug mode, unless asked by developer**. Running ASF in debug mode decreases performance and should not be used, unless you've been asked by developer to record a debug log. **Notice:** debug log consists of **sensitive** information such as the password you're using for logging in to steam. You shouldn't post your debug log in any public location, ASF developer should always remind you of sending it to his e-mail.

```AutoUpdates``` - ```bool``` type with default value of ```true```. This property defines if ASF should automatically update itself when new version is available. Updates are crucial not only to receive new features, but also to receive bugfixes, performance enhancements, stability improvements and more. When enabled, ASF will automatically download, replace, and restart itself when new update is available. In addition to initial version check on startup, ASF will also check every 24 hours if new update is available. Update process of ASF always includes only replacement of core executable file (ASF.exe) - it never touches any configs or other database files. Unless you have strong reason to disable this feature, you should keep it enabled.

```UpdateChannel``` - ```byte``` type with default value of ```1```. This property defines update channel which is being used if ```AutoUpdates``` config property defined above is ```true```. Currently ASF supports two update channels - ```1```, which is called ```Stable``` and ```2```, which is called ```Experimental```. ```Stable``` channel is the default release channel, which should be used by majority of users. ```Experimental``` channel in addition to ```Stable``` releases, also includes **pre-releases** dedicated for advanced users and other developers in order to test new features, confirm bugfixes or give feedback about planned features. **Experimental versions often contain unpatched bugs, work-in-progress features or rewritten implementations**. If you don't consider yourself advanced user, please stay with default ```1``` (Stable) update channel. ```Experimental``` channel is dedicated to users who know how to report bugs, deal with issues and give feedback. No technical support will be given.

```HttpTimeout``` - ```byte``` type with default value of ```30```. This property defines timeout for HTTP(S) requests sent by ASF, in seconds. Default value of ```30``` should be fine for majority of people, however, if you have rather slow network connection or PC, you might want to increase this number to something higher. Unless you have a strong reason to edit this property, you should keep it at default.

```AccountPlayingDelay``` - ```byte``` type with default value of ```5```. When you decide to disconnect farming ASF by starting a game, ASF will try to resume farming, after waiting ```AccountPlayingDelay``` minutes. Remember that Steam limits number of allowed logins per hour, so you should not go too low unless you have a very strong reason. Default value of ```5``` minutes is very short and should satisfy most people, you may however want to increase the delay to e.g. ```15``` or ```30``` minutes, if you feel that ```5``` minutes checks are too intrusive. Unless you have a reason to edit this property, you should keep it at default.

```RequestLimiterDelay``` - ```byte``` type with default value of ```7```. As noted above, Steam has some login requests limitations, that also includes too many logins in short time. ASF will ensure that there will be at least ```RequestLimiterDelay``` seconds in between of two consecutive logins (with an exception of 2FA-logins, as they need to be handled ASAP). Default value of ```7``` was set based on connecting over 100 bot instances, and should satisfy most (if not all) of the users. You may however want to decrease it, or even change to ```0``` if you have very low amount of bots, so ASF will ignore the delay and connect to Steam much faster. Be warned though, as setting it too low will result in Steam temporarily banning your IP, which will result in all logins failing with ```InvalidPassword``` error. Unless you have a strong reason to edit this property, you should keep it at default.

```WCFHostname``` - ```string``` type with default value of ```"localhost"```. This is a hostname, also known as "bind address", used by **[WCF](https://github.com/JustArchi/ArchiSteamFarm/wiki/WCF)**. This property makes sense only when WCF is enabled. ASF by default listens only on ```"localhost"``` address to ensure that no other machine but your own can access it. This is a security measure, as accessing WCF interface can lead to attacker taking over your ASF process, which can have dramatic effects. However, if you know what you're doing, e.g. you will restrict access to WCF yourself, using e.g. ```iptables```, you may change this property (at your own risk) to something less restrictive, such as ```"0.0.0.0"``` which enables WCF on all network interfaces. Remember that this property should be properly configured for both ```server``` and ```client``` machines (if they're not the same). Unless you have a strong reason to edit this property, you should keep it at default.

```WCFPort``` - ```ushort``` type with default value of ```1242```. This is the port on which **[WCF](https://github.com/JustArchi/ArchiSteamFarm/wiki/WCF)** is running by default. You may want to change it to any port you want, suggested ports are above ```1024```, as ports ```0-1024``` typically require ```root``` priviledges on Unix-like operating systems. Remember that this property should be the same on both ```server``` and ```client``` machines (if they're not the same). Unless you have a reason to edit this property, you should keep it at default.

```Statistics``` - ```bool``` type with default value of ```true```. This property defines if ASF should have statistics enabled. Statistics help ASF developers by providing them with information crucial to development cycle. If you want to see new versions coming up, bugs being fixed, and new features getting implemented, consider leaving it at default value of ```true```. Currently statistics include only very minimalistic information - accounts being used by ASF will automatically join our **[steam group](http://steamcommunity.com/groups/ascfarm)** and the group chat. Statistics are available for everyone, so you can check yourself how our steam group looks like, how many members it has, and how many bots are currently running (aka "on chat"). We're not gathering any other information, especially sensitive ones such as passwords, steam logins or even operating system you're using, and by keeping them enabled you help ASF developers - this way we can see how many users are using our program :+1:. Unless you have a reason to edit this property, you should keep it at default.

```Blacklist``` - ```List<uint>``` type with default value of ```267420, 303700, 335590, 368020, 425280``` appIDs. Unfortunately Steam loves to flag summer/winter sale badges as "available for cards drop", which confuses ASF process by making it believe that it's a valid game that should be farmed. If there was no blacklist, ASF would eventually "hang" at farming a game which is in fact not a game, and wait infinitely for cards drop that will not happen. ```Blacklist``` serves a purpose of marking those badges as not available for farming, so ASF can silently ignore them when deciding what to farm. ASF includes two blacklists by default - ```GlobalBlacklist```, which is hardcoded into the ASF process and not possible to edit, and normal ```Blacklist```, which is defined here. The only purpose of this property is to allow you blacklisting new, not-known at the time of ASF release appIDs, which should not be farmed. Hardcoded ```GlobalBlacklist``` is being updated as fast as possible, therefore you're not required to update your own ```Blacklist``` if you're using latest ASF version, but without ```Blacklist``` you'd be forced to update in order for ASF to "keep running" when Valve releases new sale badge, therefore this property is here to allow you "fix" ASF yourself if you for some reason don't want to update to new hardcoded ```GlobalBlacklist``` in new ASF release, yet you want your old ASF to keep running. Unless you have a strong reason to edit this property, you should keep it at default.