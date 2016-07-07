# Logging

Starting from version V2.1.2.0, ASF now allows you to configure your own custom logging format that will be used during runtime. You can do so by putting special file named ```NLog.config``` in application’s directory. You can read entire documentation of NLog **[here](https://github.com/NLog/NLog/wiki/Configuration-file)**, but in addition to that you'll find some useful examples here as well.

---

## Default logging

Using custom NLog config automatically disables default ASF one, which includes ```ColoredConsole```, ```File``` (if ```LogToFile``` is ```true```) and ```EventLog``` (if ASF is started as a service). In other words, your config overrides **completely** default ASF logging, which means that if you e.g. want to keep ```ColoredConsole``` target, you must define it yourself. This allows you to not only add **extra** logging targets, but also disable or modify **default** ones.

---

## Examples

Let's start from something easy. We will use colored console target only. Our initial ```NLog.config``` will look like this:

```
<?xml version="1.0" encoding="utf-8" ?>
<nlog xmlns="http://www.nlog-project.org/schemas/NLog.xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <targets>
    <target xsi:type="ColoredConsole" name="ColoredConsole" />
  </targets>

  <rules>
    <logger name="*" minlevel="Trace" writeTo="ColoredConsole" />
  </rules>
</nlog>
```

If you start ASF with above ```NLog.config``` now, only ```ColoredConsole``` target will be active, and ASF won't write to ```File```, neither to ```EventLog```, regardless of ```ASF.json``` configuration.

Now let's say that we don't like default format of ```${longdate}|${level:uppercase=true}|${logger}|${message}``` and we want to log message only. We can do so by modifying ```Layout``` of our target.

```
<?xml version="1.0" encoding="utf-8" ?>
<nlog xmlns="http://www.nlog-project.org/schemas/NLog.xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <targets>
    <target xsi:type="ColoredConsole" name="ColoredConsole" layout="${message}" />
  </targets>

  <rules>
    <logger name="*" minlevel="Trace" writeTo="ColoredConsole" />
  </rules>
</nlog>
```

If you launch ASF now, you'll notice that date, level and logger name disappeared - leaving you only with ASF messages in format of ```BotName|Function() Message```.

We can also modify the config to log to more than one target. Let's log to console and file at the same time.

```
<?xml version="1.0" encoding="utf-8" ?>
<nlog xmlns="http://www.nlog-project.org/schemas/NLog.xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <targets>
    <target xsi:type="ColoredConsole" name="ColoredConsole" />
    <target xsi:type="File" name="File" fileName="NLog.txt" deleteOldFileOnStartup="true" />
  </targets>

  <rules>
    <logger name="*" minlevel="Trace" writeTo="ColoredConsole,File" />
  </rules>
</nlog>
```

And done, we'll now log everything to ```ColoredConsole``` and ```File```. Did you notice that you can also specify custom ```fileName``` and extra options?

Finally, ASF uses various log levels, to make it easier for you to understand what is going on. We can use that information for modifying severity logging. Let's say that we want to log everything to the file, but only ```Warning``` and above **[log leve](https://github.com/nlog/nlog/wiki/Log-levels)** to the ```ColoredConsole```. We can achieve that by modifying our ```rules```:

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

That's it, now our ```ColoredConsole``` will show only warnings and above, while logging everything to the file. You can further tweak it to log e.g. only ```Info``` and below, and so on.