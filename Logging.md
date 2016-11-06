# Logging

Starting from version V2.1.2.0, ASF allows you to configure your own custom logging module that will be used during runtime. You can do so by putting special file named ```NLog.config``` in application’s directory. You can read entire documentation of NLog **[here](https://github.com/NLog/NLog/wiki/Configuration-file)**, but in addition to that you'll find some useful examples here as well.

---

## ASF integration

ASF includes some nice code tricks that enhance it's integration with NLog, allowing you to catch specific messages more easily.

NLog-specific ```${logger}``` variable will always distinguish the source of the message - it will be either ```BotName``` of one of your bots, or ```ASF``` if message comes from ASF process directly. This way you can easily catch messages considering specific bot(s), or ASF process (only), instead of all of them, based on the name of the logger.

ASF tries to mark messages appropriately based on NLog-provided warning levels, which makes it possible for you to catch only specific messages from specific log levels instead of all of them.

ASF logs extra info, such as user/chat messages on ```Trace``` logging level. Default ASF logging logs only ```Debug``` level and above, which hides that extra information, as it's not needed for majority of users, plus clutters output containing potentially more important messages. You can however make use of that information by re-enabling ```Trace``` logging level, especially in combination with logging only one specific bot of your choice.

In general, ASF tries to make it as easy and convenient for you as possible, to log only messages you want instead of forcing you to manually filter it through third-party tools such as ```grep``` and alike. Simply configure NLog properly as written below, and you should be able to specify even very complex logging rules with custom targets such as entire databases.

---

## Default logging

Using custom NLog config automatically disables default ASF one, which includes ```ColoredConsole```, ```EventLog``` (if ASF is started as a service) and ```File``` (otherwise). In other words, your config overrides **completely** default ASF logging, which means that if you e.g. want to keep ```ColoredConsole``` target, you must define it yourself. This allows you to not only add **extra** logging targets, but also disable or modify **default** ones.

Default layout used in ASF for ```ColoredConsole``` and ```File``` is:

```
${date:format=yyyy-MM-dd HH\:mm\:ss}|${processname}-${processid}|${level:uppercase=true}|${logger}|${message}${onexception:inner= ${exception:format=toString,Data}}
```

```EventLog``` layout is a bit simplified, as this target already includes core information we add to other ones:

```
${logger}|${message}${onexception:inner= ${exception:format=toString,Data}}
```

If you want to use default ASF logging without any modifications, you don't need to do anything - you also don't need to define it in custom ```NLog.config```. For reference though, equivalent of hardcoded ASF default logging would be:

```
<?xml version="1.0" encoding="utf-8" ?>
<nlog xmlns="http://www.nlog-project.org/schemas/NLog.xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <targets>
    <target xsi:type="ColoredConsole" name="ColoredConsole" detectConsoleAvailable="false" layout="${date:format=yyyy-MM-dd HH\:mm\:ss}|${processname}-${processid}|${level:uppercase=true}|${logger}|${message}${onexception:inner= ${exception:format=toString,Data}}" />
    <target xsi:type="EventLog" name="EventLog" layout="${logger}|${message}${onexception:inner= ${exception:format=toString,Data}}" log="ArchiSteamFarm" source="Logger" />
    <target xsi:type="File" name="File" deleteOldFileOnStartup="true" fileName="log.txt" layout="${date:format=yyyy-MM-dd HH\:mm\:ss}|${processname}-${processid}|${level:uppercase=true}|${logger}|${message}${onexception:inner= ${exception:format=toString,Data}}" />
  </targets>

  <rules>
    <logger name="*" minlevel="Debug" writeTo="ColoredConsole" />
    <logger name="*" minlevel="Debug" writeTo="EventLog" />
    <logger name="*" minlevel="Debug" writeTo="File" />
  </rules>
</nlog>
```

**Notice:** Remember that ASF uses either ```EventLog```, or ```File```, depending of whether it was started as Windows service (```EventLog```), or not (```File```), but never both at the same time. In custom NLog above, both targets were included for completion.

---

## Examples

Let's start from something easy. We will use **[ColoredConsole](https://github.com/nlog/nlog/wiki/ColoredConsole-target)** target only. Our initial ```NLog.config``` will look like this:

```
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

The explanation of above config is rather simple - we define one **logging target**, which is ```ColoredConsole```, then we redirect **all loggers** of level ```Debug``` and higher to ```ColoredConsole``` target we defined earlier. That's it.

If you start ASF with above ```NLog.config``` now, only ```ColoredConsole``` target will be active, and ASF won't write to ```File```, neither to ```EventLog```, regardless of hardcoded ASF NLog configuration.

Now let's say that we don't like default format of ```${longdate}|${level:uppercase=true}|${logger}|${message}``` and we want to log message only. We can do so by modifying **[Layout](https://github.com/nlog/nlog/wiki/Layouts)** of our target.

```
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

If you launch ASF now, you'll notice that date, level and logger name disappeared - leaving you only with ASF messages in format of ```Function() Message```.

We can also modify the config to log to more than one target. Let's log to ```ColoredConsole``` and **[File](https://github.com/nlog/nlog/wiki/File-target)** at the same time.

```
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

And done, we'll now log everything to ```ColoredConsole``` and ```File```. Did you notice that you can also specify custom ```fileName``` and extra options?

Finally, ASF uses various log levels, to make it easier for you to understand what is going on. We can use that information for modifying severity logging. Let's say that we want to log everything (```Trace```) to ```File```, but only ```Warning``` and above **[log level](https://github.com/nlog/nlog/wiki/Log-levels)** to the ```ColoredConsole```. We can achieve that by modifying our ```rules```:

```
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

That's it, now our ```ColoredConsole``` will show only warnings and above, while still logging everything to ```File```. You can further tweak it to log e.g. only ```Info``` and below, and so on.

---

## Advanced

The examples above are rather simple and made to show you how easy it is to define your own logging rules that can be used with ASF. You can use NLog for various different things, including complex targets (such as keeping logs in ```Database```), logs rotation (such as removing old ```File``` logs), using custom ```Layout```s and much more. I encourage you to read through entire **[NLog documentation](https://github.com/nlog/nlog/wiki/Configuration-file)** to learn about every option that is available to you, allowing you to fine-tune ASF logging module in the way you want. It's a really powerful tool and customizing ASF logging was never easier.

---

## Limitations

ASF will temporarily disable **all** rules that include ```ColoredConsole``` or ```Console``` targets when expecting user input. Therefore, if you want to keep logging for other targets even when ASF expects user input, you should define those targets with their own rules, as shown in examples above, instead of putting many targets in ```writeTo``` of the same rule (unless this is your wanted behaviour). Temporary disable of console targets is done in order to keep console clear when waiting for user input.