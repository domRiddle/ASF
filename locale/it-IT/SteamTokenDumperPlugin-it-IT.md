# Plugin SteamTokenDumper

`SteamTokenDumperPlugin` is official ASF **[plugin](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins)** available since ASF V4.2.2.2, developed by us, which allows you to contribute to **[SteamDB](https://steamdb.info)** project by sharing package tokens, app tokens and depot keys that your Steam account has access to. The extended info on collected data and why SteamDB needs it can be found on SteamDB's **[Token Dumper](https://steamdb.info/tokendumper)** page. The submitted data doesn't include any potentially-sensitive information, and posseses no security/privacy risk, as stated in above description.

---

## Abilitare il plugin

ASF viene rilasciato con il plugin `SteamTokenDumper` incluso nel rilascio, ma tale plugin è disabilitato per impostazione predefinita. È possibile abilitare il plugin impostando la proprietà `SteamTokenDumperPluginEnabled` nella configurazione globale di ASF a `true`, in sintassi JSON:

```json
{
  "SteamTokenDumperPluginEnabled": true
}
```

All'avvio dell'applicazione ASF, il plugin comunicherà se è stato abilitato con successo attraverso il meccanismo di logging standard. È inoltre possibile abilitare il plugin attraverso il generatore di configurazione web.

---

## Dettagli tecnici

Upon enabling, the plugin will use the bots that you're running in ASF for data gathering in form of package tokens, app tokens and depot keys that your bots have access to. Data gathering module includes passive and active routines that are supposed to minimize the additional overhead caused by collecting data.

In order to fulfill the planned use case, in addition to data gathering routine explained above, submission routine is initialized as being responsible for determining what data needs to be submitted to SteamDB on periodic basis. This routine will fire in up to `1` hour since your ASF start, and will repeat itself every `24` hours. The plugin will do its best to minimize the amount of data that needs to be sent, therefore it's possible that some data which the plugin will collect will be determined as useless to submit, and therefore skipped (for example app update which doesn't change the access token).

The plugin uses a persistent cache database saved in `config/SteamTokenDumper.cache` location, which serves a similar purpose to `config/ASF.db` for ASF. The file is used in order to record the gathered and submitted data and minimize the amount of work that has to be done across different ASF runs. Removing the file causes the process to be restarted from scratch, which should be avoided if possible.

---

## Data

ASF includes the contributor `steamID` in the request, which is determined as `SteamOwnerID` that you set in ASF, or in case you didn't, the Steam ID of the bot which owns the most licenses. The announced contributor might receive some additional perks from SteamDB for continuous help (e.g. donator rank on the website), but that is entirely up to SteamDB's discretion.

In any case, SteamDB staff would like to thank you in advance for your help. The submitted data allows SteamDB to operate, in particular to track info about packages, apps and depots, which would no longer be possible without your help.