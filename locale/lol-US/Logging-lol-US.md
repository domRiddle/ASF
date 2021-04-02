# LOGGIN

ASF ALLOWS U 2 CONFIGURE UR OWN CUSTOM LOGGIN MODULE DAT WILL BE USD DURIN RUNTIME. U CAN DO SO BY PUTTIN SPESHUL FILE NAMD `NLOG.CONFIG` IN APPLICASHUN’S DIRECTORY. You can read entire documentation of NLog on **[NLog wiki](https://github.com/NLog/NLog/wiki/Configuration-file)**, but in addition to that you'll find some useful examples here as well.

* * *

## DEFAULT LOGGIN

BY DEFAULT, ASF IZ LOGGIN 2 `COLOREDCONSOLE` (STANDARD OUTPUT) AN `FILE`. `FILE` LOGGIN INCLUDEZ `LOG.TXT` FILE IN PROGRAMS DIRECTORY, AN `LOGS` DIRECTORY 4 ARCHIVAL PURPOSEZ.

USIN CUSTOM NLOG CONFIG AUTOMATICALLY DISABLEZ DEFAULT ASF CONFIG, UR CONFIG OVERRIDEZ **COMPLETELY** DEFAULT ASF LOGGIN, WHICH MEANZ DAT IF U WANTS 2 KEEP E.G. R **COLOREDCONSOLE** TARGET, DEN U MUST DEFINE IT **YOURSELF**. DIS ALLOWS U 2 NOT ONLY ADD **EXTRA** LOGGIN TARGETS, BUT ALSO DISABLE OR MODIFY **DEFAULT** ONEZ.

IF U WANTS 2 USE DEFAULT ASF LOGGIN WITHOUT ANY MODIFICASHUNS, U DOAN NED 2 DO ANYTHIN - U ALSO DOAN NED 2 DEFINE IT IN CUSTOM `NLOG.CONFIG`. DOAN USE CUSTOM `NLOG.CONFIG` IF U DOAN WANTS 2 MODIFY DEFAULT ASF LOGGIN. 4 REFERENCE THOUGH, EQUIVALENT OV HARDCODD ASF DEFAULT LOGGIN WUD BE:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<nlog xmlns="https://nlog-project.org/schemas/NLog.xsd" xsi:schemaLocation="NLog NLog.xsd" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <targets>
    <target xsi:type="ColoredConsole" name="ColoredConsole" layout="${date:format=yyyy-MM-dd HH\:mm\:ss}|${processname}-${processid}|${level:uppercase=true}|${logger}|${message}${onexception:inner= ${exception:format=toString,Data}}" />
    <target xsi:type="File" name="File" archiveFileName="${currentdir}/logs/log.{#}.txt" archiveNumbering="Rolling" archiveOldFileOnStartup="true" cleanupFileName="false" concurrentWrites="false" deleteOldFileOnStartup="true" fileName="${currentdir}/log.txt" layout="${date:format=yyyy-MM-dd HH\:mm\:ss}|${processname}-${processid}|${level:uppercase=true}|${logger}|${message}${onexception:inner= ${exception:format=toString,Data}}" maxArchiveFiles="10" />
    <!-- BELOW BECOMEZ ACTIV WHEN ASFS IPC INTERFACE IZ STARTD -->
    <!-- <target type="History" name="History" layout="${date:format=yyyy-MM-dd HH\:mm\:ss}|${processname}-${processid}|${level:uppercase=true}|${logger}|${message}${onexception:inner= ${exception:format=toString,Data}}" maxCount="20" /> -->
  </targets>

  <rules>
    <logger name="*" minlevel="Debug" writeTo="ColoredConsole" />
    <logger name="*" minlevel="Debug" writeTo="File" />
    <!-- BELOW BECOMEZ ACTIV WHEN ASFS IPC INTERFACE IZ STARTD -->
    <!-- <logger name="*" minlevel="Debug" writeTo="History" /> -->
  </rules>
</nlog>
```

* * *

## ASF INTEGRASHUN

ASF INCLUDEZ SUM NICE CODE TRICKZ DAT ENHANCE ITZ INTEGRASHUN WIF NLOG, ALLOWIN U 2 KATCH SPECIFIC MESAGEZ MOAR EASILY.

NLOG-SPECIFIC `${LOGGR}` VARIABLE WILL ALWAYS DISTINGUISH TEH SOURCE OV TEH MESAGE - IT WILL BE EITHR `BOTNAME` OV WAN OV UR BOTS, OR `ASF` IF MESAGE COMEZ FRUM ASF PROCES DIRECTLY. DIS WAI U CAN EASILY KATCH MESAGEZ CONSIDERIN SPECIFIC BOT(S), OR ASF PROCES (ONLY), INSTEAD OV ALL OV THEM, BASD ON TEH NAYM OV TEH LOGGR.

ASF TRIEZ 2 MARK MESAGEZ APPROPRIATELY BASD ON NLOG-PROVIDD WARNIN LEVELS, WHICH MAKEZ IT POSIBLE 4 U 2 KATCH ONLY SPECIFIC MESAGEZ FRUM SPECIFIC LOG LEVELS INSTEAD OV ALL OV THEM. OV COURSE, LOGGIN LEVEL 4 SPECIFIC MESAGE CANT BE CUSTOMIZD, AS IZ ASF HARDCODD DECISHUN HOW SERIOUS GIVEN MESAGE IZ, BUT U DEFINITELY CAN MAK ASF LES/MOAR SILENT, AS U C FIT.

ASF LOGS EXTRA INFO, SUCH AS USR/CHAT MESAGEZ ON `TRACE` LOGGIN LEVEL. DEFAULT ASF LOGGIN LOGS ONLY `DEBUG` LEVEL AN ABOOV, WHICH HIDEZ DAT EXTRA INFORMASHUN, AS IZ NOT NEEDD 4 MAJORITY OV USERS, PLUS CLUTTERS OUTPUT CONTAININ POTENTIALLY MOAR IMPORTANT MESAGEZ. U CAN HOWEVR MAK USE OV DAT INFORMASHUN BY RE-ENABLIN `TRACE` LOGGIN LEVEL, ESPECIALLY IN COMBINASHUN WIF LOGGIN ONLY WAN SPECIFIC BOT OV UR CHOICE, WIF PARTICULAR EVENT URE INTERESTD IN.

IN GENERAL, ASF TRIEZ 2 MAK IT AS EASY AN CONVENIENT 4 U AS POSIBLE, 2 LOG ONLY MESAGEZ U WANTS INSTEAD OV FORCIN U 2 MANUALLY FILTR IT THRU THIRD-PARTY TOOLS SUCH AS `GREP` AN ALIKE. SIMPLY CONFIGURE NLOG PROPERLY AS WRITTEN BELOW, AN U SHUD BE ABLE 2 SPECIFY EVEN VRY COMPLEX LOGGIN RULEZ WIF CUSTOM TARGETS SUCH AS ENTIRE DATABASEZ.

Regarding versioning - ASF tries to always ship with most up-to-date version of NLog that is available on **[NuGet](https://www.nuget.org/packages/NLog)** at the time of ASF release. IZ VRY OFTEN VERSHUN DAT IZ NEWR THAN LATEST STABLE, THEREFORE IT SHUD NOT BE PROBLEM 2 USE ANY FEACHUR U CAN FIND ON NLOG WIKI IN ASF, EVEN FEATUREZ DAT R IN VRY ACTIV DEVELOPMENT AN WIP STATE - JUS MAK SURE URE ALSO USIN UP-2-DATE ASF.

AS PART OV ASF INTEGRASHUN, ASF ALSO INCLUDEZ SUPPORT 4 ADDISHUNAL ASF NLOG LOGGIN TARGETS, WHICH WILL BE EXPLAIND BELOW.

* * *

## EXAMPLEZ

LETS START FRUM SOMETHIN EASY. We will use **[ColoredConsole](https://github.com/nlog/nlog/wiki/ColoredConsole-target)** target only. Our initial `NLog.config` will look like this:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<nlog xmlns="https://nlog-project.org/schemas/NLog.xsd" xsi:schemaLocation="NLog NLog.xsd" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <targets>
    <target xsi:type="ColoredConsole" name="ColoredConsole" />
  </targets>

  <rules>
    <logger name="*" minlevel="Debug" writeTo="ColoredConsole" />
  </rules>
</nlog>
```

The explanation of above config is rather simple - we define one **logging target**, which is `ColoredConsole`, then we redirect **all loggers** (`*`) of level `Debug` and higher to `ColoredConsole` target we defined earlier. That's it.

IF U START ASF WIF ABOOV `NLog.config` NAO, ONLY `ColoredConsole` TARGET WILL BE ACTIV, AN ASF WONT RITE 2 `File`, REGARDLES OV HARDCODD ASF NLOG CONFIGURASHUN.

Now let's say that we don't like default format of `${longdate}|${level:uppercase=true}|${logger}|${message}` and we want to log message only. We can do so by modifying **[Layout](https://github.com/nlog/nlog/wiki/Layouts)** of our target.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<nlog xmlns="https://nlog-project.org/schemas/NLog.xsd" xsi:schemaLocation="NLog NLog.xsd" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <targets>
    <target xsi:type="ColoredConsole" name="ColoredConsole" layout="${message}" />
  </targets>

  <rules>
    <logger name="*" minlevel="Debug" writeTo="ColoredConsole" />
  </rules>
</nlog>
```

IF U LAUNCH ASF NAO, ULL NOTICE DAT DATE, LEVEL AN LOGGR NAYM DISAPPEARD - LEAVIN U ONLY WIF ASF MESAGEZ IN FORMAT OV `Function() Message`.

WE CAN ALSO MODIFY TEH CONFIG 2 LOG 2 MOAR THAN WAN TARGET. Let's log to `ColoredConsole` and **[File](https://github.com/nlog/nlog/wiki/File-target)** at the same time.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<nlog xmlns="https://nlog-project.org/schemas/NLog.xsd" xsi:schemaLocation="NLog NLog.xsd" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <targets>
    <target xsi:type="ColoredConsole" name="ColoredConsole" />
    <target xsi:type="File" name="File" fileName="${currentdir}/NLog.txt" deleteOldFileOnStartup="true" />
  </targets>

  <rules>
    <logger name="*" minlevel="Debug" writeTo="ColoredConsole" />
    <logger name="*" minlevel="Debug" writeTo="File" />
  </rules>
</nlog>
```

AN DUN, WELL NAO LOG EVRYTHIN 2 `ColoredConsole` AN `File`. DID U NOTICE DAT U CAN ALSO SPECIFY CUSTOM `fileName` AN EXTRA OPSHUNS?

FINALLY, ASF USEZ VARIOUS LOG LEVELS, 2 MAK IT EASIR 4 U 2 UNDERSTAND WUT IZ GOIN ON. WE CAN USE DAT INFORMASHUN 4 MODIFYIN SEVERITY LOGGIN. Let's say that we want to log everything (`Trace`) to `File`, but only `Warning` and above **[log level](https://github.com/NLog/NLog/wiki/Configuration-file#log-levels)** to the `ColoredConsole`. WE CAN ACHIEVE DAT BY MODIFYIN R `rules`:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<nlog xmlns="https://nlog-project.org/schemas/NLog.xsd" xsi:schemaLocation="NLog NLog.xsd" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <targets>
    <target xsi:type="ColoredConsole" name="ColoredConsole" />
    <target xsi:type="File" name="File" fileName="${currentdir}/NLog.txt" deleteOldFileOnStartup="true" />
  </targets>

  <rules>
    <logger name="*" minlevel="Warn" writeTo="ColoredConsole" />
    <logger name="*" minlevel="Trace" writeTo="File" />
  </rules>
</nlog>
```

That's it, now our `ColoredConsole` will show only warnings and above, while still logging everything to `File`. U CAN FURTHR TWEAK IT 2 LOG E.G. ONLY `Info` AN BELOW, AN SO ON.

LASTLY, LETS DO SOMETHIN BIT MOAR ADVANCD AN LOG ALL MESAGEZ 2 FILE, BUT ONLY FRUM BOT NAMD `LogBot`.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<nlog xmlns="https://nlog-project.org/schemas/NLog.xsd" xsi:schemaLocation="NLog NLog.xsd" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <targets>
    <target xsi:type="ColoredConsole" name="ColoredConsole" />
    <target xsi:type="File" name="LogBotFile" fileName="${currentdir}/LogBot.txt" deleteOldFileOnStartup="true" />
  </targets>

  <rules>
    <logger name="*" minlevel="Debug" writeTo="ColoredConsole" />
    <logger name="LogBot" minlevel="Trace" writeTo="LogBotFile" />
  </rules>
</nlog>
```

U CAN C HOW WE USD ASF INTEGRASHUN ABOOV AN EASILY DISTINGUISHD SOURCE OV TEH MESAGE BASD ON `${logger}` PROPERTY.

* * *

## ADVANCD USAGE

TEH EXAMPLEZ ABOOV R RATHR SIMPLE AN MADE 2 SHOW U HOW EASY IT 2 DEFINE UR OWN LOGGIN RULEZ DAT CAN BE USD WIF ASF. U CAN USE NLOG 4 VARIOUS DIFFERENT THINGS, INCLUDIN COMPLEX TARGETS (SUCH AS KEEPIN LOGS IN `Database`), LOGS ROTASHUN (SUCH AS REMOVIN OLD `File` LOGS), USIN CUSTOM `Layout`S, DECLARIN UR OWN `<when>` LOGGIN FILTERS AN MUTCH MOAR. I encourage you to read through entire **[NLog documentation](https://github.com/nlog/nlog/wiki/Configuration-file)** to learn about every option that is available to you, allowing you to fine-tune ASF logging module in the way you want. IT BE RLY POWERFUL TOOL AN CUSTOMIZIN ASF LOGGIN WUZ NEVR EASIR.

* * *

## LIMITASHUNS

ASF will temporarily disable **all** rules that include `ColoredConsole` or `Console` targets when expecting user input. THEREFORE, IF U WANTS 2 KEEP LOGGIN 4 OTHR TARGETS EVEN WHEN ASF EXPEX USR INPUT, U SHUD DEFINE DOSE TARGETS WIF THEIR OWN RULEZ, AS SHOWN IN EXAMPLEZ ABOOV, INSTEAD OV PUTTIN LOTZ DA TARGETS IN `writeTo` OV TEH SAME RULE (UNLES DIS AR TEH UR WANTD BEHAVIOUR). TEMPORARY DISABLE OV CONSOLE TARGETS IZ DUN IN ORDR 2 KEEP CONSOLE CLEAN WHEN WAITIN 4 USR INPUT.

* * *

## CHAT LOGGIN

ASF includes extended support for chat logging by not only recording all received/sent messages on `Trace` logging level, but also exposing extra info related to them in **[event properties](https://github.com/NLog/NLog/wiki/EventProperties-Layout-Renderer)**. DIS AR TEH CUZ WE NED 2 HANDLE CHAT MESAGEZ AS COMMANDZ ANYWAY, SO IT DOESNT COST US ANYTHIN 2 LOG DOSE EVENTS IN ORDR 2 MAK IT POSIBLE 4 U 2 ADD EXTRA LOGIC (SUCH AS MAKIN ASF UR PERSONAL STEAM CHATTIN ARCHIV).

### EVENT PROPERTIEZ

| NAYM        | DESCRIPSHUN                                                                                                                                                                                                |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Echo        | `bool` type. This is set to `true` when message is being sent from us to the recipient, and `false` otherwise.                                                                                             |
| Message     | `string` type. This is the actual sent/received message.                                                                                                                                                   |
| ChatGroupID | `ulong` type. This is the ID of the group chat for sent/received messages. Will be `0` when no group chat is used for transmitting this message.                                                           |
| ChatID      | `ulong` type. This is the ID of the `ChatGroupID` channel for sent/received messages. Will be `0` when no group chat is used for transmitting this message.                                                |
| SteamID     | `ulong` type. DIS AR TEH TEH ID OV TEH STEAM USR 4 SENT/RECEIVD MESAGEZ. Can be `0` when no particular user is involved in the message transmission (e.g. when it's us sending a message to a group chat). |

### EXAMPLE

DIS EXAMPLE IZ BASD ON R `ColoredConsole` BASIC EXAMPLE ABOOV. Before trying to understand it, I strongly recommend to take a look **[above](#examples)** in order to learn about basics of NLog logging firstly.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<nlog xmlns="https://nlog-project.org/schemas/NLog.xsd" xsi:schemaLocation="NLog NLog.xsd" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <targets>
    <target xsi:type="ColoredConsole" name="ColoredConsole" />
    <target xsi:type="File" name="ChatLogFile" fileName="${currentdir}/${event-properties:item=ChatGroupID}-${event-properties:item=ChatID}${when:when='${event-properties:item=ChatGroupID}' == 0:inner=-${event-properties:item=SteamID}}.txt" layout="${date:format=yyyy-MM-dd HH\:mm\:ss} ${event-properties:item=Message} ${when:when='${event-properties:item=Echo}' == true:inner=-&gt;:else=&lt;-} ${event-properties:item=SteamID}" />
  </targets>

  <rules>
    <logger name="*" minlevel="Debug" writeTo="ColoredConsole" />
    <logger name="MainAccount" level="Trace" writeTo="ChatLogFile">
      <filters>
        <when condition="not starts-with('${message}','OnIncoming') and not starts-with('${message}','SendMessage')" action="Ignore" />
      </filters>
    </logger>
  </rules>
</nlog>
```

We've started from our basic `ColoredConsole` example and extended it further. FURST AN FOREMOST, WEVE PREPARD PERMANENT CHAT LOG FILE PER EACH GROUP CHANNEL AN STEAM USR - DIS AR TEH POSIBLE THX 2 EXTRA PROPERTIEZ DAT ASF EXPOSEZ 2 US IN FANCY WAI. WEVE ALSO DECIDD 2 GO WIF CUSTOM LAYOUT DAT WRITEZ ONLY CURRENT DATE, TEH MESAGE, SENT/RECEIVD INFO AN STEAM USR ITSELF. Lastly, we've enabled our chat logging rule only for `Trace` level, only for our `MainAccount` bot and only for functions related to chat logging (`OnIncoming*` which is used for receiving messages and echos, and `SendMessage*` for ASF messages sending).

The example above will generate `0-0-76561198069026042.txt` file when talking with **[ArchiBot](https://steamcommunity.com/profiles/76561198069026042)**:

```text
2018-07-26 01:38:38 how are you doing? -> 76561198069026042
2018-07-26 01:38:38 I'm doing great, how about you? <- 76561198069026042
```

OV COURSE DIS AR TEH JUS WERKIN EXAMPLE WIF FEW NICE LAYOUT TRICKZ SHOWD IN PRACTICAL MANNR. U CAN FURTHR EXPAND DIS IDEA 2 UR OWN NEEDZ, SUCH AS EXTRA FILTERIN, CUSTOM ORDR, PERSONAL LAYOUT, RECORDIN ONLY RECEIVD MESAGEZ AN SO ON.

* * *

## ASF TARGETS

IN ADDISHUN 2 STANDARD NLOG LOGGIN TARGETS (SUCH AS `ColoredConsole` AN `File` EXPLAIND ABOOV), U CAN ALSO USE CUSTOM ASF LOGGIN TARGETS.

4 MAXIMUM COMPLETENES, DEFINISHUN OV ASF TARGETS WILL FOLLOW NLOG DOCUMENTASHUN CONVENSHUN.

* * *

### STEAMTARGET

AS U CAN GUES, DIS TARGET USEZ STEAM CHAT MESAGEZ 4 LOGGIN ASF MESAGEZ. U CAN CONFIGURE IT 2 USE EITHR GROUP CHAT, OR PRIVATE CHAT. IN ADDISHUN 2 SPECIFYIN STEAM TARGET 4 UR MESAGEZ, U CAN ALSO SPECIFY `botName` OV TEH BOT DAT IZ SUPPOSD 2 SEND DOSE.

SUPPORTD IN ALL ENVIRONMENTS USD BY ASF.

* * *

#### CONFIGURASHUN SYNTAX

```xml
<targets>
  <target type="Steam"
          name="String"
          layout="Layout"
          chatGroupID="Ulong"
          steamID="Ulong"
          botName="Layout" />
</targets>
```

Read more about using the [Configuration File](https://github.com/NLog/NLog/wiki/Configuration-file).

* * *

#### PARAMETERS

##### GENERAL OPSHUNS

*name* - NAYM OV TEH TARGET.

* * *

##### LAYOUT OPSHUNS

*layout* - Text to be rendered. [Layout](https://github.com/NLog/NLog/wiki/Layouts) Required. Default: `${level:uppercase=true}|${logger}|${message}`

* * *

##### SteamTarget Options

*chatGroupID* - ID of the group chat declared as 64-bit long unsigned integer. Not required. DEFAULTS 2 `0` WHICH WILL DISABLE GROUP CHAT FUNCSHUNALITY AN USE PRIVATE CHAT INSTEAD. When enabled (set to non-zero value), `steamID` property below acts as `chatID` and specifies ID of the channel in this `chatGroupID` that the bot should send messages to.

*steamID* - SteamID declared as 64-bit long unsigned integer of target Steam user (like `SteamOwnerID`), or target `chatID` (when `chatGroupID` is set). Required. Defaults to 0 which disables logging target entirely.

*botName* - Name of the bot (as it's recognized by ASF, case-sensitive) of target bot that will be sending messages to `steamID` declared above. NOT REQUIRD. Defaults to `null` which will automatically select **any** currently connected bot. It's recommended to set this value appropriately, as `SteamTarget` does not take into account many Steam limitations, such as the fact that you must have `steamID` of the target on your friendlist. This variable is defined as [layout](https://github.com/NLog/NLog/wiki/Layouts) type, therefore you can use special syntax in it, such as `${logger}` in order to use the bot that generated the message.

* * *

#### SteamTarget Examples

In order to write all messages of `Debug` level and above, from bot named `MyBot` to steamID of `76561198006963719`, you should use `NLog.config` similar to below:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<nlog xmlns="https://nlog-project.org/schemas/NLog.xsd" xsi:schemaLocation="NLog NLog.xsd" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <targets>
    <target type="Steam" name="Steam" steamID="76561198006963719" botName="MyBot" />
  </targets>

  <rules>
    <logger name="*" minlevel="Debug" writeTo="Steam" />
  </rules>
</nlog>
```

**Notice:** Our `SteamTarget` is custom target, so you should make sure that you're declaring it as `type="Steam"`, NOT `xsi:type="Steam"`, as xsi is reserved for official targets supported by NLog.

When you launch ASF with `NLog.config` similar to above, `MyBot` will start messaging `76561198006963719` Steam user with all usual ASF log messages. KEEP IN MIND DAT `MyBot` MUST BE CONNECTD IN ORDR 2 SEND MESAGEZ, SO ALL INITIAL ASF MESAGEZ DAT HAPPEND BEFORE R BOT CUD CONNECT 2 STEAM NETWORK, WONT BE FORWARDD.

OV COURSE, `SteamTarget` HAS ALL TYPICAL FUNCSHUNS DAT U CUD EXPECT FRUM GENERIC `TargetWithLayout`, SO U CAN USE IT IN CONJUNCSHUN WIF E.G. CUSTOM LAYOUTS, NAMEZ OR ADVANCD LOGGIN RULEZ. TEH EXAMPLE ABOOV IZ ONLY TEH MOST BASIC WAN.

* * *

#### SCREENSHOTS

![Screenshot](https://i.imgur.com/5juKHMt.png)

* * *

### HistoryTarget

This target is used internally by ASF for providing fixed-size logging history in `/Api/NLog` endpoint of **[ASF API](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#asf-api)** that can be afterwards consumed by ASF-ui and other tools. IN GENERAL U SHUD DEFINE DIS TARGET ONLY IF URE ALREADY USIN CUSTOM NLOG CONFIG 4 OTHR CUSTOMIZASHUNS AN U ALSO WANTS TEH LOG 2 BE EXPOSD IN ASF API, E.G. 4 ASF-UI. IT CAN ALSO BE DECLARD WHEN UD WANTS 2 MODIFY DEFAULT LAYOUT OR `maxCount` OV SAVD MESAGEZ.

SUPPORTD IN ALL ENVIRONMENTS USD BY ASF.

* * *

#### CONFIGURASHUN SYNTAX

```xml
<targets>
  <target type="History"
          name="String"
          layout="Layout"
          maxCount="Byte" />
</targets>
```

Read more about using the [Configuration File](https://github.com/NLog/NLog/wiki/Configuration-file).

* * *

#### PARAMETERS

##### GENERAL OPSHUNS

*name* - NAYM OV TEH TARGET.

* * *

##### LAYOUT OPSHUNS

*layout* - Text to be rendered. [Layout](https://github.com/NLog/NLog/wiki/Layouts) Required. Default: `${date:format=yyyy-MM-dd HH\:mm\:ss}|${processname}-${processid}|${level:uppercase=true}|${logger}|${message}${onexception:inner= ${exception:format=toString,Data}}`

* * *

##### HistoryTarget Options

*maxCount* - Maximum amount of stored logs for on-demand history. Not required. Defaults to `20` which is a good balance for providing initial history, while still keeping in mind memory usage that comes out of storage requirements. Must be greater than `0`.

* * *

## CAVEATS

Be careful when you decide to combine `Debug` logging level or below in your `SteamTarget` with `steamID` that is taking part in the ASF process. DIS CAN LEAD 2 POTENTIAL `StackOverflowException` CUZ ULL CREATE AN INFINITE LOOP OV ASF RECEIVIN GIVEN MESAGE, DEN LOGGIN IT THRU STEAM, RESULTIN IN ANOTHR MESAGE DAT NEEDZ 2 BE LOGGD. Currently the only possibility for it to happen is to log `Trace` level (where ASF records its own chat messages), or `Debug` level while also running ASF in `Debug` mode (where ASF records all Steam packets).

In short, if your `steamID` is taking part in the same ASF process, then the `minlevel` logging level of your `SteamTarget` should be `Info` (or `Debug` if you're also not running ASF in `Debug` mode) and above. ALTERNATIVELY U CAN DEFINE UR OWN `<when>` LOGGIN FILTERS IN ORDR 2 AVOID INFINITE LOGGIN LOOP, IF MODIFYIN LEVEL IZ NOT APPROPRIATE 4 UR CASE. DIS CAVEAT ALSO APPLIEZ 2 GROUP CHATS.