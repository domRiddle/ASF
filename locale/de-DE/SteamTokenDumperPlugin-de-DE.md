# SteamTokenDumperPlugin

`SteamTokenDumperPlugin` ist ein offizielles ASF **[plugin](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins)** seit ASF V4.2.2., wird durch uns entwickelt und erlaubt dir zum **[SteamDB](https://steamdb.info)** Projekt beizutragen, indem du Paket- und App-Tokens sowie Depot-Schlüssel, auf die dein Steam Konto Zugriff hat, teilst. Informationen zu den gesammelten Daten und warum SteamDB diese benötigt finden Sie auf der Seite SteamDB's **[Token Dumper](https://steamdb.info/tokendumper)**. Die übermittelten Daten enthalten demnach keine potentiell sensiblen Informationen und bergen kein Sicherheits-/Datenschutzrisiko.

---

## Aktivierung des Plugins

Das `SteamTokenDumperPlugin</0 > ist in der Releaseversion von ASF enthalten, jedoch ist das Plugin standardmäßig deaktiviert. Sie können das Plugin aktivieren, indem Sie die globale ASF Konfigurationseigenschaft <code>SteamTokenDumperPluginEnabled` auf `true` setzen, im JSON-Syntax:

```json
{
  "SteamTokenDumperPluginAktiviert": true
}
```

Beim Start von ASF wird das Plugin Sie mittels der Standard-Protokollierungsmethode darüber informieren, ob es erfolgreich aktiviert wurde oder nicht. Sie können das Plugin auch über unseren web-basierten Konfigurationsgenerator aktivieren.

---

## Technische Details

Upon enabling, the plugin will use the bots that you're running in ASF for data gathering in form of package tokens, app tokens and depot keys that your bots have access to. Data gathering module includes passive and active routines that are supposed to minimize the additional overhead caused by collecting data.

In order to fulfill the planned use case, in addition to data gathering routine explained above, submission routine is initialized as being responsible for determining what data needs to be submitted to SteamDB on periodic basis. This routine will fire in up to `1` hour since your ASF start, and will repeat itself every `24` hours. The plugin will do its best to minimize the amount of data that needs to be sent, therefore it's possible that some data which the plugin will collect will be determined as useless to submit, and therefore skipped (for example app update which doesn't change the access token).

The plugin uses a persistent cache database saved in `config/SteamTokenDumper.cache` location, which serves a similar purpose to `config/ASF.db` for ASF. The file is used in order to record the gathered and submitted data and minimize the amount of work that has to be done across different ASF runs. Removing the file causes the process to be restarted from scratch, which should be avoided if possible.

---

## Daten

ASF includes the contributor `steamID` in the request, which is determined as `SteamOwnerID` that you set in ASF, or in case you didn't, the Steam ID of the bot which owns the most licenses. The announced contributor might receive some additional perks from SteamDB for continuous help (e.g. donator rank on the website), but that is entirely up to SteamDB's discretion.

In any case, SteamDB staff would like to thank you in advance for your help. The submitted data allows SteamDB to operate, in particular to track info about packages, apps and depots, which would no longer be possible without your help.