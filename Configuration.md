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

*bool* - Boolean type accepting only ```true``` and ```false``` values.
*byte* - Byte type, accepting only numbers from ```0``` to ```255``` (inclusive)
*string* - String type, accepting any sequence of characters, such as ```"password"``` or ```"pablo32"```

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

```AutoUpdates``` - ```bool``` type with default value of ```true```. This property defines if ASF should automatically update itself when new version is available. Updates are crucial not only to receive new features, but also to receive bugfixes, performance enhancements, stability improvements and more. When enabled, ASF will automatically download, replace, and restart itself when new update is available. In addition to initial version check on startup, ASF will also check every 24 hours if new update is available. Update process of ASF always includes only replacement of core executable file (ASF.exe), and never touches any configs or other database files.