# SteamTokenDumperPlugin

`SteamTokenDumperPlugin` is official ASF **[plugin](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins)** developed by us which allows you to contribute to **[SteamDB](https://steamdb.info)** project by sharing package tokens, app tokens and depot keys that your Steam account has access to. The extended info on collected data and why SteamDB needs it can be found on SteamDB's **[Token Dumper](https://steamdb.info/tokendumper)** page.

---

## Enabling the plugin

ASF comes with `SteamTokenDumperPlugin` bundled together with the release, but the plugin itself is disabled by default. You can enable the plugin by setting `SteamTokenDumperPluginEnabled` ASF global config property to `true`, in JSON syntax:

```json
{
  "SteamTokenDumperPluginEnabled": true
}
```

On the launch of the ASF program, the plugin will let you know whether it was enabled successfully through standard ASF logging mechanism.

---

## Technical details

Upon enabling, the plugin will use the bots that you're running in ASF for data gathering in form of package tokens, app tokens and depot keys that your bots have access to. Data gathering module includes passive and active routines that are supposed to minimize the additional overhead caused by collecting data.

In order to fulfill the planned use case, in addition to data gathering routine explained above, submission routine is initialized as being responsible for determining what data needs to be submitted to SteamDB on periodic basis. This routine will fire in approximately `30` minutes since your ASF start, and will repeat itself every `24` hours. The plugin will minimize the amount of data that needs to be sent in form of including only data that needs to be updated.

The plugin uses a persistent cache database saved in `config/SteamDB.cache` location, which serves a similar purpose to `config/ASF.db` for ASF. The file is used in order to record the gathered and submitted data and minimize the amount of work that has to be done across different ASF runs. Removing the file causes the process to be restarted from scratch, which should be avoided if possible.

---

## Data

ASF includes the contributor `steamID` in the request, which is determined as `SteamOwnerID` that you set in ASF, or in case you didn't, the Steam ID of the bot which owns the most licenses. The announced contributor might receive some additional perks from SteamDB for continuous help (e.g. donator rank on the website), but that is entirely up to SteamDB's discretion.

In any case, SteamDB staff would like to thank you in advance for your help. The submitted data allows SteamDB to operate, in particular to track info about packages, apps and depots, which would no longer be possible without your help.