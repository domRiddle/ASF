# Running as Service

ASF supports being run as Service - this mode can be especially useful on Windows. Running as service on Mono were not tested and most likely won't work due to many current Mono limitations, it's suggested to use classic Unix integration in this case, such as systemd.

---

## Installation

At the moment normal ASF binary is not capable to be used in service environment, this limitation might be solved in future (see: **[issue](https://github.com/Fody/Costura/issues/164)**). Until then, there is special ```ASF-Service.exe``` binary provided in releases that can be used for that purpose, and it should be used instead of ```ASF.exe```. It has exactly the same functionality, including auto-updates as normal ASF.exe binary, while using different repacking method suitable for being used as a service.

In order to install ```ASF-Service.exe```, you should use **[InstallUtil.exe](https://msdn.microsoft.com/library/50614e95(v=vs.110).aspx)**. Typically this binary can be found in C:\Windows\Microsoft.NET\Framework(64)\VERSION where VERSION should be latest available .NET runtime version, such as ```v4.0.30319``` right now. ```InstallUtil.exe``` must be executed with admin rights, so if you want to execute it through ```cmd```, make sure to start ```cmd``` as administrator.

Complete command should look like this:

```
C:\Windows\Microsoft.NET\Framework64\v4.0.30319\InstallUtil.exe C:\Path\To\ASF-Service.exe
```

If installation went successfully, you can now find your ASF service in **[Services](https://msdn.microsoft.com/library/d56de412(v=vs.110).aspx)** and configure it further, such as if it should start Manually or Automatically etc.

**Notice:** It is recommended to switch ```AutoRestart``` to ```false``` and ```Headless``` to ```true``` when running ASF as a service.

---

## Functionality

Running ASF as a service should result in the same functionality as running ASF in usual way, with some extra integration features that make it easier for you to manage ASF. Such extras include e.g. tweaked **[Logging](https://github.com/JustArchi/ArchiSteamFarm/wiki/Logging)** module that will write logs to ```EventLog``` instead of ```File``` by default when ASF is running as a service. You can modify default behaviour in Logging settings linked above if you for some reason would like to customize it.

---

## Limitations

Service mode offered in ASF comes as an extra, and is not being used in our testing environment. It should work properly, but in the end various limitations may apply. One of those is the fact that ASF doesn't seem to be able to restart itself when being run as a service, that's why it's recommended to set ```AutoRestart``` to false. Apart from that, there are no more known limitations, but because of not being carefully tested, it's possible that there might be more unknown ones.