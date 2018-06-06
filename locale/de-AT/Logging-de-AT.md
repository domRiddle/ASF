# Protokollierung

Beginnend mit Version V2.1.2.0 erlaubt dir ASF, benutzerdefinierte Protokollierungsmodule zu konfigurieren, die während der Laufzeit verwendet werden. Dies funktioniert, indem du eine spezielle Datei mit dem Namen `NLog.config` in dem Programmverzeichnis ablegst. Du kannst die gesamte Dokumentation über NLog auf der **[NLog wiki](https://github.com/NLog/NLog/wiki/Configuration-file)** nachlesen, zusätzlich wirst du auch hier nützliche Beispiele dazu finden.

* * *

## Standard Protokollierung

Eine benutzerdefinierte NLog Konfiguration deaktiviert automatisch die Standard ASF Konfiguration, welche `ColoredConsole` und `File` beinhaltet. In anderen Worten überschreibt deine Konfiguration **komplett** die standard ASF Protokollierung, was bedeutet, dass wenn du z.B. `ColoredConsole` als Ziel behalten möchtest, du es selber definieren musst. Dies erlaubt dir nicht nur **extra** Protokollierungsziele zu erstellen, sondern auch **Standardziele** zu verändern oder deaktivieren.

Wenn du die Standard ASF Protokollierung ohne irgendwelche Veränderung verwenden möchtest, musst du nichts tun - auch brauchst du dies nicht in der `NLog.config` definieren. Verwende die `NLog.config` nicht, wenn du die standard ASF Protokollierung nicht verändern möchtest. Als Referenz dagegen, die entsprechende hardgecodete Variante der ASF Protokollierung wäre:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<nlog xmlns="http://www.nlog-project.org/schemas/NLog.xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <targets>
    <target xsi:type="ColoredConsole" name="ColoredConsole" layout="${date:format=yyyy-MM-dd HH\:mm\:ss}|${processname}-${processid}|${level:uppercase=true}|${logger}|${message}${onexception:inner= ${exception:format=toString,Data}}" />
    <target xsi:type="File" name="File" deleteOldFileOnStartup="true" fileName="log.txt" layout="${date:format=yyyy-MM-dd HH\:mm\:ss}|${processname}-${processid}|${level:uppercase=true}|${logger}|${message}${onexception:inner= ${exception:format=toString,Data}}" />
    <!-- Folgende Zeile wird aktiv wenn ASF's IPC Schnittstelle gestartet ist -->
    <!-- <target type="History" name="History" layout="${date:format=yyyy-MM-dd HH\:mm\:ss}|${processname}-${processid}|${level:uppercase=true}|${logger}|${message}${onexception:inner= ${exception:format=toString,Data}}" maxCount="20" /> -->
  </targets>

  <rules>
    <logger name="*" minlevel="Debug" writeTo="ColoredConsole" />
    <logger name="*" minlevel="Debug" writeTo="File" />
    <!-- Folgende Zeile wird aktiv wenn ASF's IPC Schnittstelle gestartet ist -->
    <!-- <logger name="*" minlevel="Debug" writeTo="History" /> -->
  </rules>
</nlog>
```

* * *

## ASF Integration

ASF includes some nice code tricks that enhance its integration with NLog, allowing you to catch specific messages more easily.

NLog-specific `${logger}` variable will always distinguish the source of the message - it will be either `BotName` of one of your bots, or `ASF` if message comes from ASF process directly. This way you can easily catch messages considering specific bot(s), or ASF process (only), instead of all of them, based on the name of the logger.

ASF tries to mark messages appropriately based on NLog-provided warning levels, which makes it possible for you to catch only specific messages from specific log levels instead of all of them. Of course, logging level for specific message can't be customized, as it's ASF hardcoded decision how serious given message is, but you definitely can make ASF less/more silent, as you see fit.

ASF logs extra info, such as user/chat messages on `Trace` logging level. Default ASF logging logs only `Debug` level and above, which hides that extra information, as it's not needed for majority of users, plus clutters output containing potentially more important messages. You can however make use of that information by re-enabling `Trace` logging level, especially in combination with logging only one specific bot of your choice, with particular event you're interested in.

In general, ASF tries to make it as easy and convenient for you as possible, to log only messages you want instead of forcing you to manually filter it through third-party tools such as `grep` and alike. Simply configure NLog properly as written below, and you should be able to specify even very complex logging rules with custom targets such as entire databases.

Regarding versioning - ASF tries to always ship with most up-to-date version of NLog that is available on **[NuGet](https://www.nuget.org/packages/NLog)** at the time of ASF release. It's very often a version that is newer than latest stable, therefore it should not be a problem to use any feature you can find on NLog wiki in ASF, even features that are in very active development and WIP state - just make sure you're also using up-to-date ASF.

As part of ASF integration, ASF also includes support for additional ASF NLog logging targets, which will be explained below.

* * *

## Examples

Let's start from something easy. We will use **[ColoredConsole](https://github.com/nlog/nlog/wiki/ColoredConsole-target)** target only. Our initial `NLog.config` will look like this:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<nlog xmlns="http://www.nlog-project.org/schemas/NLog.xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <targets>
    <target xsi:type="ColoredConsole" name="ColoredConsole" />
  </targets>

  <rules>
    <logger name="*" minlevel="Debug" writeTo="ColoredConsole" />
  </rules>
</nlog>
```

The explanation of above config is rather simple - we define one **logging target**, which is `ColoredConsole`, then we redirect **all loggers** of level `Debug` and higher to `ColoredConsole` target we defined earlier. That's it.

If you start ASF with above `NLog.config` now, only `ColoredConsole` target will be active, and ASF won't write to `File`, regardless of hardcoded ASF NLog configuration.

Now let's say that we don't like default format of `${longdate}|${level:uppercase=true}|${logger}|${message}` and we want to log message only. We can do so by modifying **[Layout](https://github.com/nlog/nlog/wiki/Layouts)** of our target.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<nlog xmlns="http://www.nlog-project.org/schemas/NLog.xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <targets>
    <target xsi:type="ColoredConsole" name="ColoredConsole" layout="${message}" />
  </targets>

  <rules>
    <logger name="*" minlevel="Debug" writeTo="ColoredConsole" />
  </rules>
</nlog>
```

If you launch ASF now, you'll notice that date, level and logger name disappeared - leaving you only with ASF messages in format of `Function() Message`.

We can also modify the config to log to more than one target. Let's log to `ColoredConsole` and **[File](https://github.com/nlog/nlog/wiki/File-target)** at the same time.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<nlog xmlns="http://www.nlog-project.org/schemas/NLog.xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <targets>
    <target xsi:type="ColoredConsole" name="ColoredConsole" />
    <target xsi:type="File" name="File" fileName="NLog.txt" deleteOldFileOnStartup="true" />
  </targets>

  <rules>
    <logger name="*" minlevel="Debug" writeTo="ColoredConsole" />
    <logger name="*" minlevel="Debug" writeTo="File" />
  </rules>
</nlog>
```

And done, we'll now log everything to `ColoredConsole` and `File`. Did you notice that you can also specify custom `fileName` and extra options?

Finally, ASF uses various log levels, to make it easier for you to understand what is going on. We can use that information for modifying severity logging. Let's say that we want to log everything (`Trace`) to `File`, but only `Warning` and above **[log level](https://github.com/NLog/NLog/wiki/Configuration-file#log-levels)** to the `ColoredConsole`. We can achieve that by modifying our `rules`:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<nlog xmlns="http://www.nlog-project.org/schemas/NLog.xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <targets>
    <target xsi:type="ColoredConsole" name="ColoredConsole" />
    <target xsi:type="File" name="File" fileName="NLog.txt" deleteOldFileOnStartup="true" />
  </targets>

  <rules>
    <logger name="*" minlevel="Warn" writeTo="ColoredConsole" />
    <logger name="*" minlevel="Trace" writeTo="File" />
  </rules>
</nlog>
```

That's it, now our `ColoredConsole` will show only warnings and above, while still logging everything to `File`. You can further tweak it to log e.g. only `Info` and below, and so on.

Lastly, let's do something a bit more advanced and log all messages to file, but only from bot named `LogBot`.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<nlog xmlns="http://www.nlog-project.org/schemas/NLog.xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <targets>
    <target xsi:type="ColoredConsole" name="ColoredConsole" />
    <target xsi:type="File" name="LogBotFile" fileName="LogBot.txt" deleteOldFileOnStartup="true" />
  </targets>

  <rules>
    <logger name="*" minlevel="Debug" writeTo="ColoredConsole" />
    <logger name="LogBot" minlevel="Trace" writeTo="LogBotFile" />
  </rules>
</nlog>
```

You can see how we used ASF integration above and easily distinguished source of the message based on `${logger}` property.

* * *

## Erweiterte Nutzung

The examples above are rather simple and made to show you how easy it is to define your own logging rules that can be used with ASF. You can use NLog for various different things, including complex targets (such as keeping logs in `Database`), logs rotation (such as removing old `File` logs), using custom `Layout`s, declaring your own `<when>` logging filters and much more. I encourage you to read through entire **[NLog documentation](https://github.com/nlog/nlog/wiki/Configuration-file)** to learn about every option that is available to you, allowing you to fine-tune ASF logging module in the way you want. It's a really powerful tool and customizing ASF logging was never easier.

* * *

## Einschränkungen

ASF will temporarily disable **all** rules that include `ColoredConsole` or `Console` targets when expecting user input. Therefore, if you want to keep logging for other targets even when ASF expects user input, you should define those targets with their own rules, as shown in examples above, instead of putting many targets in `writeTo` of the same rule (unless this is your wanted behaviour). Temporary disable of console targets is done in order to keep console clean when waiting for user input.

* * *

## ASF targets

Starting with version 2.2.1.7, in addition to standard NLog logging targets (such as `ColoredConsole` and `File` explained above), you can also use custom ASF logging targets.

For maximum completeness, definition of ASF targets will follow NLog documentation convention.

* * *

### SteamTarget

As you can guess, this target uses Steam chat messages for logging ASF messages. It writes log messages to specific `steamID`, from specific `botName`.

Supported in all environments used by ASF.

* * *

#### Configuration Syntax

```xml
<targets>
  <target type="Steam"
          name="String"
          layout="Layout"
          steamID="Ulong"
          botName="String" />
</targets>
```

Read more about using the [Configuration File](https://github.com/NLog/NLog/wiki/Configuration-file).

* * *

#### Parameter

##### General Options

*name* - Name of the target.

* * *

##### Layout Options

*layout* - Text to be rendered. [Layout](https://github.com/NLog/NLog/wiki/Layouts) Required. Standard: `${level:uppercase=true}|${logger}|${message}`

* * *

##### SteamTarget Options

*steamID* - SteamID declared as 64-bit long unsigned integer of target Steam user (like `SteamOwnerID`), or target group chat (like `SteamMasterClanID`) where messages will be sent. Required. Defaults to 0 which disables logging target entirely.

*botName* - Name of the bot (as it's recognized by ASF, case-sensitive) of target bot that will be sending messages to `steamID` declared above. Not required. Defaults to `null` which will automatically select **any** currently connected bot. It's recommended to set this value appropriately, as `SteamTarget` does not take in account many Steam limitations, such as the fact that you must have `steamID` of the target on your friendlist.

* * *

#### SteamTarget Examples

In order to write all messages of `Debug` level and above, from bot named `MyBot` to steamID of `76561198006963719`, you should use `NLog.config` similar to below:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<nlog xmlns="http://www.nlog-project.org/schemas/NLog.xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <targets>
    <target type="Steam" name="Steam" steamID="76561198006963719" botName="MyBot" />
  </targets>

  <rules>
    <logger name="*" minlevel="Debug" writeTo="Steam" />
  </rules>
</nlog>
```

**Notice:** Our `SteamTarget` is custom target, so you should make sure that you're declaring it as `type="Steam"`, NOT `xsi:type="Steam"`, as xsi is reserved for official targets supported by NLog.

When you launch ASF with `NLog.config` similar to above, `MyBot` will start messaging `76561198006963719` Steam user with all usual ASF log messages. Keep in mind that `MyBot` must be connected in order to send messages, so all initial ASF messages that happened before our bot could connect to Steam network, won't be forwarded.

Of course, `SteamTarget` has all typical functions that you could expect from generic `TargetWithLayout`, so you can use it in conjuction with e.g. custom layouts, names or advanced logging rules. The example above is only the most basic one.

* * *

#### Screenshots

![Screenshot](https://i.imgur.com/5juKHMt.png)

* * *

### HistoryTarget

This target is used internally by ASF for providing fixed-size logging history for IPC GUI usage. In general you should define this target only if you're using custom NLog config for other customizations and you also want logging history in IPC GUI. It can also be declared when you'd want to modify default value of `maxCount`.

Supported in all environments used by ASF.

* * *

#### Configuration Syntax

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

#### Parameter

##### General Options

*name* - Name of the target.

* * *

##### Layout Options

*layout* - Text to be rendered. [Layout](https://github.com/NLog/NLog/wiki/Layouts) Required. Standard: `${date:format=yyyy-MM-dd HH\:mm\:ss}|${processname}-${processid}|${level:uppercase=true}|${logger}|${message}${onexception:inner= ${exception:format=toString,Data}}`

* * *

##### HistoryTarget Options

*maxCount* - Maximum amount of stored logs for on-demand history. Not required. Defaults to `20` which is a good balance for providing initial history, while still keeping in mind memory usage that comes out of storage requirements. Must be greater than `0`.

* * *

## Fallstricke

Be careful when you decide to combine `Debug` logging level or below in your `SteamTarget` with `steamID` that is taking part in the ASF process. This can lead to potential `StackOverflowException` because you'll create an infinite loop of ASF receiving given message, then logging it through Steam, resulting in another message that needs to be logged. Currently the only possibility for it to happen is to log `Trace` level (where ASF records its own chat messages), or `Debug` level while also running ASF in `Debug` mode (where ASF records all Steam packets).

In short, if your `steamID` is taking part in the same ASF process, then the `minlevel` logging level of your `SteamTarget` should `Info` (or `Debug` if you're also not running ASF in `Debug` mode) and above. Alternatively you can define your own `<when>` logging filters in order to avoid infinite logging loop, if modifying level is not appropriate for your case.