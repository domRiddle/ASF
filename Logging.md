# Logging

Starting from version V2.1.2.0, ASF now allows you to configure your own custom logging format that will be used during runtime. You can do so by putting special file named ```NLog.config``` in application’s directory. You can read entire documentation of NLog **[here](https://github.com/NLog/NLog/wiki/Configuration-file)**, but in addition to that you'll find some useful examples here as well.

---

## Default logging

Using custom NLog config automatically disables default ASF one, which includes ```ColoredConsole```, ```File``` (if ```LogToFile``` is ```true```) and ```EventLog``` (if ASF is started as a service). In other words, your config overrides **completely** default ASF logging, which means that if you e.g. want to keep ```ColoredConsole``` target, you must define it yourself. This allows you to not only add **extra** logging targets, but also disable or modify **default** ones.

---

## Examples

Let's start from something easy. Let's use classic non-colored console target only. Our ```NLog.config``` will look like this:

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