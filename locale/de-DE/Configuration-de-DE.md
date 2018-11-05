# Konfiguration

Diese Seite widmet sich der Konfiguration von ASF. Es dient als eine lückenlose Dokumentation des `config` Verzeichnisses, so dass du ASF auf deine Bedürfnisse abstimmen kannst.

- **[Einleitung](#einleitung)**
- **[Web-basierter ConfigGenerator](#web-basierter-configgenerator)**
- **[Manuelle Konfiguration](#manuelle-konfiguration)**
- **[Globale Konfiguration](#globale-konfiguration)**
- **[Bot Konfiguration](#bot-konfiguration)**
- **[Dateistruktur](#dateistruktur)**
- **[JSON-Mapping](#json-mapping)**
- **[Kompatibilitätsmapping](#kompatibilitätsmapping)**
- **[Konfigurations-Kompatibilität](#konfigurations-kompatibilität)**
- **[Automatisches Nachladen](#automatisches-nachladen)**

* * *

## Einleitung

Die ASF-Konfiguration gliedert sich in zwei Hauptteile - die globale (Prozess-)Konfiguration und die Konfiguration jedes einzelnen Bots. Jeder Bot hat seine eigene Bot-Konfigurationsdatei namens `BotName.json` (wobei `BotName` der Name des Bots ist), während die globale ASF (Prozess-)Konfiguration eine einzige Datei namens `ASF.json` ist.

Ein Bot ist ein einzelnes Steam-Konto welches am ASF-Prozess teilnimmt. Um ordnungsgemäß zu funktionieren benötigt ASF mindestens **eine** definierte Bot-Instanz. Es gibt keine prozessbedingte Begrenzung der Bot-Instanzen, so dass du so viele Bots (Steam-Konten) verwenden kannst wie du willst.

ASF verwendet das **[JSON](https://en.wikipedia.org/wiki/JSON)** Format zum Speichern seiner Konfigurationsdateien. Es ist ein benutzerfreundliches, lesbares und sehr universelles Format in dem du das Programm konfigurieren kannst. Keine Sorge, du musst JSON nicht kennen um ASF zu konfigurieren. Ich habe es nur erwähnt, falls du bereits ASF-Konfigurationen mit einer Art Bash-Skript massenhaft erstellen möchtest.

Die Konfiguration kann entweder manuell, durch Erstellung korrekter JSON-Konfigurationen, erfolgen oder durch Verwendung unseres **[Web-basierten ConfigGenerators](https://justarchinet.github.io/ASF-WebConfigGenerator)**, was viel einfacher und komfortabler sein sollte. Wenn du kein fortgeschrittener Benutzer bist, schlage ich vor, den Konfigurationsgenerator zu verwenden, der im Folgenden beschrieben wird.

**[Zum Seitenanfang](#konfiguration)**

* * *

## Web-basierter ConfigGenerator

Der Zweck des web-basierten ConfigGenerator ist es, dir ein benutzerfreundliches Frontend zur Verfügung zu stellen das zum Erzeugen von ASF-Konfigurationsdateien verwendet werden kann. Der web-basierte ConfigGenerator ist zu 100% Client-basiert, was bedeutet, dass die von dir eingegebenen Daten nicht versendet, sondern nur lokal verarbeitet werden. Dies garantiert Sicherheit und Zuverlässigkeit, da es sogar **[offline](https://github.com/JustArchiNET/ASF-WebConfigGenerator/tree/master/docs)** funktionieren kann, wenn du alle Dateien herunterladen und `index.html` in deinem Lieblingsbrowser ausführen möchtest.

Der web-basierte ConfigGenerator ist so konzipiert, dass er unter Chrome und Firefox ordnungsgemäß läuft, sollte aber in allen gängigen JavaScript-fähigen Browsern ordnungsgemäß funktionieren.

Die Verwendung ist denkbar einfach - man wähle, ob man die Konfiguration `ASF` oder `Bot` durch Umschalten auf die entsprechende Registerkarte generieren möchte, stelle sicher, dass die gewählte Version der Konfigurationsdatei zu deiner ASF-Version passt, gebe dann alle Details ein und klicke auf die Schaltfläche "Herunterladen". Bewege diese Datei in das ASF `config` Verzeichnis und überschreibe bei Bedarf vorhandene Dateien. Wiederhole dies für alle eventuellen weiteren Änderungen und lies den Rest dieses Abschnitts, um alle verfügbaren Konfigurationsoptionen zu erlernen.

**[Zum Seitenanfang](#konfiguration)**

* * *

## Manuelle Konfiguration

Ich empfehle dringend, den webbasierten Konfigurationsgenerator zu verwenden, aber wenn du aus irgendeinem Grund nicht willst, dann kannst du auch selbst richtige Konfigurationen erstellen. Überprüfe `example.json` für einen guten Einstieg in die richtige Struktur, du kannst diese Datei kopieren und als Basis für deinen neu konfigurierten Bot verwenden. Da du nicht unser Frontend verwendest, stelle sicher, dass deine Konfiguration **[gültig](https://jsonlint.com)** ist, da ASF das Einlesen ablehnt, wenn es nicht analysiert werden kann. Für eine korrekte JSON-Struktur aller verfügbaren Felder sieh dir den **[JSON-Mapping](#json-mapping)** Abschnitt und die Dokumentation unten an.

**[Zum Seitenanfang](#konfiguration)**

* * *

## Globale Konfiguration

Die globale Konfigurationsdatei befindet sich in `ASF.json` und hat folgende Struktur:

```json
{
    "AutoRestart": true,
    "Blacklist": [],
    "CommandPrefix": "!",
    "ConfirmationsLimiterDelay": 10,
    "ConnectionTimeout": 60,
    "CurrentCulture": null,
    "Debug": false,
    "FarmingDelay": 15,
    "GiftsLimiterDelay": 1,
    "Headless": false,
    "IdleFarmingPeriod": 8,
    "InventoryLimiterDelay": 3,
    "IPC": false,
    "IPCPassword": null,
    "LoginLimiterDelay": 10,
    "MaxFarmingTime": 10,
    "MaxTradeHoldDuration": 15,
    "OptimizationMode": 0,
    "Statistics": true,
    "SteamMessagePrefix": "/me ",
    "SteamOwnerID": 0,
    "SteamProtocols": 7,
    "UpdateChannel": 1,
    "UpdatePeriod": 24,
    "WebLimiterDelay": 200,
    "WebProxy": null,
    "WebProxyPassword": null,
    "WebProxyUsername": null
}
```

**Tipp:** Sofern du keine dieser Optionen ändern möchtest, solltest du alles auf Standardwerte belassen, deshalb kannst du `ASF.json` schließen und zur Bot-Konfiguration übergehen.

* * *

Alle Optionen werden im Folgenden erläutert:

### `AutoRestart`

`bool` Typ mit Standardwert von `true`. Diese Eigenschaft legt fest, ob ASF bei Bedarf einen Selbst-Neustart durchführen darf. Es gibt ein paar Fälle, die von ASF einen Neustart erfordern, wie z.B. ASF-Aktualisierung (durchgeführt mit `UpdatePeriod` oder dem Befehl `update`), sowie das Verändern der `ASF.json` Konfiguration, `restart` Befehl und ähnlich. Normalerweise beinhaltet der Neustart zwei Teile - das Erstellen eines neuen Prozesses und das Beenden des aktuellen Prozesses. Die meisten Benutzer sollten damit einverstanden sein und diese Eigenschaft auf dem Standardwert `true` behalten - wenn du ASF durch dein eigenes Skript und/oder mit `dotnet` ausführst, solltest du vielleicht die volle Kontrolle über den Start des Prozesses haben und eine Situation vermeiden, in der ein neuer (neu gestarteter) ASF-Prozess irgendwo im Hintergrund und nicht im Vordergrund des Skripts läuft, der zusammen mit dem alten ASF-Prozess beendet wurde. Dies ist besonders wichtig, wenn man bedenkt, dass der neue Prozess nicht mehr dein direktes Kind ist, was dich z.B. nicht in die Lage versetzen würde, die Standardkonsolen-Eingabe dafür zu verwenden.

Wenn das der Fall ist, ist diese Eigenschaft speziell für dich und du kannst sie auf `false` setzen. Bedenke jedoch, dass in diesem Fall **du** für den Neustart des Prozesses verantwortlich bist. Dies ist irgendwie wichtig, da ASF nur beendet wird, anstatt einen neuen Prozess zu starten (z.B. nach der Aktualisierung), so dass, wenn keine Logik von dir hinzugefügt wird, es einfach aufhört zu funktionieren, bis du es wieder startest. ASF beendet sich immer mit einem korrekten Fehlercode, der Erfolg (Null) oder Misserfolg (ungleich Null) anzeigt, auf diese Weise kannst du in deinem Skript eine korrekte Logik hinzufügen, die einen automatischen Neustart von ASF im Fehlerfall vermeiden sollte, oder zumindest eine lokale Kopie von `log.txt` für weitere Analysen erstellen. Bedenke auch, dass der Befehl `restart` ASF immer neu startet, unabhängig davon, wie diese Eigenschaft eingestellt ist, da diese Eigenschaft das Standardverhalten definiert, während der Befehl `restart` den Prozess immer neu startet. Wenn du keinen Grund hast, diese Funktion zu deaktivieren, solltest du sie aktiviert lassen.

* * *

### `Blacklist`

`ImmutableHashSet<uint>` Typ mit einem leeren Standardwert. Wie der Name schon sagt, definiert diese globale Konfigurationseigenschaft AppIDs (Spiele), die vom automatischen ASF-Sammel-Prozess vollständig ignoriert werden. Unfortunately Steam loves to flag summer/winter sale badges as "available for cards drop", which confuses ASF process by making it believe that it's a valid game that should be farmed. If there was no any kind of blacklist, ASF would eventually "hang" at farming a game which is in fact not a game, and wait infinitely for cards drop that will not happen. ASF blacklist serves a purpose of marking those badges as not available for farming, so we can silently ignore them when deciding what to farm, and not fall into the trap.

ASF includes two blacklists by default - `GlobalBlacklist`, which is hardcoded into the ASF code and not possible to edit, and normal `Blacklist`, which is defined here. `GlobalBlacklist` is updated together with ASF version and typically includes all "bad" appIDs at the time of release, so if you're using up-to-date ASF then you do not need to maintain your own `Blacklist` defined here. The main purpose of this property is to allow you blacklisting new, not-known at the time of ASF release appIDs, which should not be farmed. Hardcoded `GlobalBlacklist` is being updated as fast as possible, therefore you're not required to update your own `Blacklist` if you're using latest ASF version, but without `Blacklist` you'd be forced to update ASF in order to "keep running" when Valve releases new sale badge - I don't want to force you to use latest ASF code, therefore this property is here to allow you "fixing" ASF yourself if you for some reason don't want to, or can't, update to new hardcoded `GlobalBlacklist` in new ASF release, yet you want to keep your old ASF running. Unless you have a **strong** reason to edit this property, you should keep it at default.

If you're looking for bot-based blacklist instead, take a look at `ib`, `ibadd` and `ibrm` **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**.

* * *

### `CommandPrefix`

`string` Typ mit Standardwert von `!`. This property specifies **case-sensitive** prefix used for ASF **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. In other words, this is what you need to prepend to each ASF command in order to make ASF listen to you. Es ist möglich, diesen Wert auf `null` oder leer zu setzen, damit ASF kein Befehlspräfix verwendet, in diesem Fall gibst du Befehle mit ihren einfachen Bezeichnern ein. However, doing so will potentially decrease ASF's performance as ASF is optimized to not parse message further if it doesn't start with `CommandPrefix` - if you intentionally decide to not use it, ASF will be forced to read all messages and respond to them, even if they're not ASF commands. Therefore it's recommended to keep using some `CommandPrefix`, such as `/` if you don't like default value of `!`. For consistency, `CommandPrefix` affects the entire ASF process. Unless you have a reason to edit this property, you should keep it at default.

* * *

### `ConfirmationsLimiterDelay`

`byte` Typ mit Standardwert von `10`. ASF will ensure that there will be at least `ConfirmationsLimiterDelay` seconds in between of two consecutive 2FA confirmations fetching requests to avoid triggering rate-limit - those are being used by **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** during e.g. `2faok` command, as well as on as-needed basis when performing various trading-related operations. Default value was set based on our tests and should not be lowered if you don't want to run into issues. Unless you have a **strong** reason to edit this property, you should keep it at default.

* * *

### `ConnectionTimeout`

`byte` Typ mit Standardwert von `60`. This property defines timeouts for various network actions done by ASF, in seconds. In particular, `ConnectionTimeout` defines timeout in seconds for HTTP and IPC requests, `ConnectionTimeout / 10` defines maximum number of failed heartbeats, while `ConnectionTimeout / 30` defines number of minutes we allow for initial Steam network connection request. Default value of `60` should be fine for majority of people, however, if you have rather slow network connection or PC, you might want to increase this number to something higher (like `90`). Keep in mind that bigger values will not magically fix slow or even inaccessible Steam servers, so we shouldn't infinitely wait for something that won't happen and simply try again later. Setting this value too high will result in excessive delay in catching network issues, as well as in decrease of overall performance. Setting this value too low will decrease overall stability and performance as well, as ASF will abort valid request still being parsed. Therefore setting this value lower than default has no advantage in general, as Steam servers tend to be slow from time to time, and might require more time for parsing ASF requests. Default value is a balance between believing that our network connection is stable, and doubting in Steam network to handle our request in given timeout. If you want to detect issues sooner and make ASF reconnect/respond faster, default value should do (or very slightly below, making ASF less patient). If you instead notice that ASF is running into network issues, such as failing requests, heartbeats being lost or connection to Steam interrupted, it might make sense to increase this value if you're sure that it's **not** caused by your network, but by Steam itself, as increasing timeouts makes ASF more "patient" and not deciding to reconnect right away. It might also make sense to increase this value if you have rather slow internet that requires more time to process the data being transmitted. In short, default value should be decent for most cases, but you might want to increase it if needed. Still, going far above the default value doesn't make much sense either, since bigger timeouts won't magically fix inaccessible Steam servers. Unless you have a reason to edit this property, you should keep it at default.

* * *

### `CurrentCulture`

`string` Typ mit Standardwert von `null`. Standardmäßig versucht ASF, die Sprache deines Betriebssystems zu verwenden, und wird es vorziehen, übersetzte Zeichenketten in dieser Sprache zu verwenden, falls verfügbar. This is possible thanks to our community that tries to **[localize](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Localization)** ASF in all most popular languages. If for some reason you don't want to use your OS native language, you can use this config property to pick any valid language you'd want to use instead. For a list of all available cultures, please visit **[MSDN](https://msdn.microsoft.com/en-us/library/cc233982.aspx)** and look for `Language tag`. It's nice to note that ASF accepts both specific cultures, such as `en-GB`, but also neutral ones, such as `en`. Specifying current culture might also affect other culture-specific behaviour, such as currency/date format and alike. Please note that you might need additional font/language packs for displaying language-specific characters properly, if you picked non-native culture that makes use of them. Typically you want to make use of this config property if you prefer ASF in English instead of your native language.

* * *

### `Debug`

`bool` Typ mit Standardwert von `false`. This property defines if process should run in debug mode. When in debug mode, ASF creates a special `debug` directory next to the `config`, which keeps track of whole communication between ASF and Steam servers. Debug information can help spotting nasty issues related to networking and general ASF workflow. In addition to that, some program routines will be far more verbose, such as `WebBrowser` stating exact reason why some requests are failing - those entries are written to normal ASF log. **You should not run ASF in Debug mode, unless asked by developer**. Running ASF in debug mode **decreases performance**, **affects stability negatively** and is **far more verbose in various places**, so it should be used **only** intentionally, in short-run, for debugging particular issue, reproducing the problem or getting more info about a failing request, and alike, but **not** for normal program execution. You will see **a lot** of new errors, issues, and exceptions - make sure that you have a decent knowledge about ASF, Steam and its quirks if you decide to analyze all of that yourself, as not everything is relevant.

**WARNING:** enabling this mode includes logging of **potentially sensitive** information such as logins and passwords that you're using for logging in to Steam (due to network logging). That data is written to both `debug` directory, as well as standard `log.txt` (that is now intentionally much more verbose to log this info). You should not post debug content generated by ASF in any public location, ASF developer should always remind you of sending it to his e-mail, or other secure location. We're not storing, neither making use of those sensitive details, they're written as part of debug routines since their presence might be relevant to the issue that is affecting you. We'd prefer if you didn't alter ASF logging in any way, but if you're worried, you're free to redact those sensitive details appropriately.

> Redacting involves replacing sensitive details, for example with stars. You should refrain from removing sensitive lines entirely, as their pure existence might be relevant and should be preserved.

* * *

### `FarmingDelay`

`byte` Typ mit Standardwert von `15`. In order for ASF to work, it will check currently farmed game every `FarmingDelay` minutes, if it perhaps dropped all cards already. Setting this property too low can result in excessive amount of steam requests being sent, while setting it too high can result in ASF still "farming" given title for up to `FarmingDelay` minutes after it's fully farmed. Default value should be excellent for most users, but if you have many bots running, you might consider increasing it to something like `30` minutes in order to limit steam requests being sent. It's nice to note that ASF uses event-based mechanism and checks game badge page on each Steam item dropped, so in general **we don't even need to check it in fixed time intervals**, but as we don't fully trust Steam network - we check game badge page anyway, if we didn't check it through card being dropped event in last `FarmingDelay` minutes (in case Steam network didn't inform us about item dropped or stuff like that). Assuming that Steam network is working properly, decreasing this value **will not improve farming efficiency in any way**, while **increasing network overhead significantly** - it's recommended only to increase it (if needed) from default of `15` minutes. Unless you have a **strong** reason to edit this property, you should keep it at default.

* * *

### `GiftsLimiterDelay`

`byte` Typ mit Standardwert von `1`. ASF will ensure that there will be at least `GiftsLimiterDelay` seconds in between of two consecutive gift/key/license handling (redeeming) requests to avoid triggering rate-limit. In addition to that it'll also be used as global limiter for game list requests, such as the one issued by `owns` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. Unless you have a **strong** reason to edit this property, you should keep it at default.

* * *

### `Headless`

`bool` Typ mit Standardwert von `false`. This property defines if process should run in headless mode. When in headless mode, ASF assumes that it's running on a server or in other non-interactive environment, therefore it will not attempt to read crucial account credentials such as 2FA code, SteamGuard code, password or any other variable required for ASF to operate. This is equal to making ASF console read-only. `Headless` mode is useful mainly for users running ASF on their servers, as instead of asking e.g. for 2FA code, ASF will silently abort the operation by stopping an account. Unless you're running ASF on a server, and you previously confirmed that ASF is able to operate in non-headless mode, you should keep this property disabled. Any user interaction will be denied when in headless mode, and your accounts will not run if they require **any** console input during starting. This is useful for servers, as ASF can abort trying to log onto the account when asked for credentials, instead of waiting (infinitely) for user to provide those. Enabling this mode will also allow you to use `input` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** which acts as a replacement for standard console input. If you're not sure how to set this property, leave it with default value of `false`.

If you're running ASF on the server, you might want to use this option together with `--process-required` **[command-line argument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments)**.

* * *

### `IdleFarmingPeriod`

`byte` Typ mit Standardwert von `8`. When ASF has nothing to farm, it will periodically check every `IdleFarmingPeriod` hours if perhaps account got some new games to farm. This feature is not needed when talking about new games we're getting, as ASF is smart enough to automatically check badge pages in this case. `IdleFarmingPeriod` is mainly for situations such as old games we already have having trading cards added. In this case there is no event, so ASF has to periodically check badge pages if we want to have this covered. Value of `0` disables this feature. Also check: `ShutdownOnFarmingFinished`.

* * *

### `InventoryLimiterDelay`

`byte` Typ mit Standardwert von `3`. ASF will ensure that there will be at least `InventoryLimiterDelay` seconds in between of two consecutive inventory requests to avoid triggering rate-limit - those are being used for fetching your own inventory (and only for that). Default value of `3` was set based on looting over 100 bot instances, and should satisfy most (if not all) of the users. You may however want to decrease it, or even change to `0` if you have very low amount of bots, so ASF will ignore the delay and loot steam inventories much faster. Be warned though, as setting it too low **will** result in Steam temporarily banning your IP, and that will prevent you from fetching your inventory at all. You also might need to increase this value if you're running a lot of bots with a lot of inventory requests, although in this case you should probably try to limit number of those requests instead. Unless you have a **strong** reason to edit this property, you should keep it at default.

* * *

### `IPC`

`bool` Typ mit Standardwert von `false`. Diese Eigenschaft definiert, ob der ASF-**[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-de-DE)**-Server zusammen mit dem Prozess gestartet werden soll. IPC ermöglicht die prozessübergreifende Kommunikation durch Starten eines lokalen HTTP-Servers. Wenn du den IPC-Server von ASF nicht nutzen willst, dann gibt es keinen Grund für dich, diese Option zu aktivieren.

* * *

### `IPCPassword`

`string` Typ mit Standardwert von `null`. This property defines mandatory password for every call done via IPC and serves as an extra security measure. When set to non-empty value, all IPC requests will require extra `password` property set to the password specified here. Default value of `null` will skip a need of the password, making ASF respect all queries. In addition to that, enabling this option also enables built-in IPC anti-bruteforce mechanism which will temporarily ban given `IPAddress` after sending too many unauthorized requests in a very short time. Unless you have a reason to edit this property, you should keep it at default.

* * *

### `LoginLimiterDelay`

`byte` Typ mit Standardwert von `10`. ASF will ensure that there will be at least `LoginLimiterDelay` seconds in between of two consecutive connection attempts to avoid triggering rate-limit. Default value of `10` was set based on connecting over 100 bot instances, and should satisfy most (if not all) of the users. You may however want to increase/decrease it, or even change to `0` if you have very low amount of bots, so ASF will ignore the delay and connect to Steam much faster. Be warned though, as setting it too low while having too many bots **will** result in Steam temporarily banning your IP, and that will prevent you from logging in **at all**, with `InvalidPassword/RateLimitExceeded` error - and that also includes your normal Steam client, not only ASF. Likewise, if you're running excessive number of bots, especially together with other Steam clients/tools using the same IP address, most likely you'll need to increase this value in order to spread logins across longer period of time.

As a side note, this value is also used as load-balancing buffer in all ASF-scheduled actions, such as trades in `SendTradePeriod`. Unless you have a **strong** reason to edit this property, you should keep it at default.

* * *

### `MaxFarmingTime`

`byte` Typ mit Standardwert von `10`. As you should know, Steam is not always working properly, sometimes weird situations can happen such as steam not being recording our playtime, despite of in fact playing a game. ASF will allow farming a single game in solo mode for maximum of `MaxFarmingTime` hours, and consider it fully farmed after that period. This is required to not freeze farming process in case of weird situations happening, but also if for some reason Steam released a new badge that would stop ASF from progressing further (see: `Blacklist`). Default value of `10` hours should be enough for dropping all steam cards from one game. Setting this property too low can result in valid games being skipped (and yes, there are valid games taking even up to 9 hours to farm), while setting it too high can result in farming process being frozen. Please note that this property affects only a single game in a single farming session (so after going through entire queue ASF will return to that title), also it's not based on total playtime but internal ASF farming time. Unless you have a **strong** reason to edit this property, you should keep it at default.

* * *

### `MaxTradeHoldDuration`

`byte` Typ mit Standardwert von `15`. This property defines maximum duration of trade hold in days that we're willing to accept - ASF will reject trades that are being held for more than `MaxTradeHoldDuration` days. This option makes sense only for bots with `TradingPreferences` of `SteamTradeMatcher`, as it doesn't affect `Master`/`SteamOwnerID` trades, neither donations. Trade holds are annoying for everyone, and nobody really wants to deal with them. ASF is supposed to work on liberal rules and help everyone, regardless if on trade hold or not - that's why this option is set to `15` by default. However, if you'd instead prefer to reject all trades affected by trade holds, you can specify `0` here. Please consider the fact that cards with short lifespan are not affected by this option and automatically rejected for people with trade holds, as described in **[trading](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading)** section, so there is no need to globally reject everybody only because of that. Unless you have a reason to edit this property, you should keep it at default.

* * *

### `OptimizationMode`

`byte` Typ mit Standardwert von `0`. This property defines optimization mode which ASF will prefer during runtime. Currently ASF supports two modes - `0` which is called `MaxPerformance`, and `1` which is called `MinMemoryUsage`. By default ASF prefers to run as many things in parallel (concurrently) as possible, which enhances performance by load-balancing work across all CPU cores, multiple CPU threads, multiple sockets and multiple threadpool tasks. For example, ASF will ask for your first badge page when checking for games to idle, and then once request arrived, ASF will read from it how many badge pages you actually have, then request each other one concurrently. This is what you should want **almost always**, as the overhead in most cases is minimal and benefits from asynchronous ASF code can be seen even on the oldest hardware with a single CPU core and heavily limited power. However, with many tasks being processed in parallel, ASF runtime is responsible for their maintenance, e.g. keeping sockets open, threads alive and tasks being processed, which can result in increased memory usage from time to time, and if you're extremely constrained by available memory, you might want to switch this property to `1` (`MinMemoryUsage`) in order to force ASF into using as little tasks as possible, and typically running possible-to-parallel asynchronous code in a synchronous manner. You should consider switching this property only if you previously read **[low-memory setup](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup)** and you intentionally want to sacrifice gigantic performance boost, for a very small memory overhead decrease. Usually this option is **much worse** than what you can achieve with other possible ways, such as by limiting your ASF usage or tuning runtime's garbage collector, as explained in **[low-memory setup](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup)**. Therefore, you should use `MinMemoryUsage` as a **last resort**, right before runtime recompilation, if you couldn't achieve satisfying results with other (much better) options. Unless you have a **strong** reason to edit this property, you should keep it at default.

* * *

### `Statistics`

`bool` Typ mit Standardwert von `true`. Diese Eigenschaft legt fest, ob ASF die Statistik aktiviert haben soll. Eine detaillierte Erklärung, was genau diese Option bewirkt, findest du im Abschnitt **[Statistiken](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Statistics-de-DE)**. Wenn du keinen Grund hast, diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

* * *

### `SteamMessagePrefix`

`string` Typ mit Standardwert von `"/me "`. This property defines a prefix that will be prepended to all Steam messages being sent by ASF. By default ASF uses `"/me "` prefix in order to distinguish bot messages more easily by showing them in different color on Steam chat. Another worthy mention is `"/pre "` prefix which achieves similar result, but uses different formatting. You can also set this property to empty string or `null` in order to disable using prefix entirely and output all ASF messages in a traditional way. It's nice to note that this property affects Steam messages only - responses returned through other channels (such as IPC) are not affected. Unless you want to customize standard ASF behaviour, it's a good idea to leave it at default.

* * *

### `SteamOwnerID`

`ulong` Typ mit Standardwert von `0`. This property defines Steam ID in 64-bit form of ASF process owner, and is very similar to `Master` permission of given bot instance (but global instead). You almost always want to set this property to ID of your own main Steam account. `Master` permission includes full control over his bot instance, but global commands such as `exit`, `restart` or `update` are reserved for `SteamOwnerID` only. This is convenient, as you might want to run bots for your friends, while not allowing them to control ASF process, such as exiting it via `exit` command. Default value of `0` specifies that there is no owner of ASF process, which means that nobody will be able to issue global ASF commands. Keep in mind that **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** works with `SteamOwnerID`, so if you want to use it you must provide a valid value here.

* * *

### `SteamProtocols`

`byte flags` Typ mit Standardwert von `7`. This property defines Steam protocols that ASF will use when connecting to Steam servers, which are defined as below:

| Wert | Name      | Beschreibung                                                                                     |
| ---- | --------- | ------------------------------------------------------------------------------------------------ |
| 0    | None      | Kein Protokoll                                                                                   |
| 1    | TCP       | **[Transmission Control Protocol](https://en.wikipedia.org/wiki/Transmission_Control_Protocol)** |
| 2    | UDP       | **[User Datagram Protocol](https://en.wikipedia.org/wiki/User_Datagram_Protocol)**               |
| 4    | WebSocket | **[WebSocket](https://en.wikipedia.org/wiki/WebSocket)**                                         |

Please notice that this property is `flags` field, therefore it's possible to choose any combination of available values. Check out **[flags mapping](#json-mapping)** if you'd like to learn more. Not enabling any of flags results in `None` option, and that option is invalid by itself.

By default ASF should use all available Steam protocols as a measure for fighting with downtimes and other similar Steam issues. Typically you want to change this property if you want to limit ASF into using only one or two specific protocols instead of all available ones. Such measure could be needed if you're e.g. enabling only TCP traffic on your firewall and you do not want ASF to try connecting via UDP. However, unless you're debugging particular problem or issue, you almost always want to ensure that ASF is free to use any protocol that is currently supported and not just one or two. Unless you have a **strong** reason to edit this property, you should keep it at default.

* * *

### `UpdateChannel`

`byte` Typ mit Standardwert von `1`. This property defines update channel which is being used, either for auto-updates (if `UpdatePeriod` is greater than `0`), or update notifications (otherwise). Currently ASF supports three update channels - `0` which is called `None`, `1`, which is called `Stable`, and `2`, which is called `Experimental`. `Stable` channel is the default release channel, which should be used by majority of users. `Experimental` channel in addition to `Stable` releases, also includes **pre-releases** dedicated for advanced users and other developers in order to test new features, confirm bugfixes or give feedback about planned enhancements. **Experimental versions often contain unpatched bugs, work-in-progress features or rewritten implementations**. If you don't consider yourself advanced user, please stay with default `1` (Stable) update channel. `Experimental` channel is dedicated to users who know how to report bugs, deal with issues and give feedback - no technical support will be given. Check out ASF **[release cycle](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle)** if you'd like to learn more. You can also set `UpdateChannel` to `0` (`None`), if you want to completely remove all version checks. Setting `UpdateChannel` to `0` will entirely disable entire functionality related to updates, including `update` command. Using `None` channel is **strongly discouraged** due to exposing yourself to all sort of problems (mentioned in `UpdatePeriod` description below).

**Unless you know what you're doing**, we **strongly** recommend to keep it at default.

* * *

### `UpdatePeriod`

`byte` Typ mit Standardwert von `24`. This property defines how often ASF should check for auto-updates. Updates are crucial not only to receive new features and critical security patches, but also to receive bugfixes, performance enhancements, stability improvements and more. When a value greater than `0` is set, ASF will automatically download, replace, and restart itself (if `AutoRestart` permits) when new update is available. In order to achieve this, ASF will check every `UpdatePeriod` hours if new update is available on our GitHub repo. A value of `0` disables auto-updates, but still allows you to execute `update` command manually. You might also be interested in setting appropriate `UpdateChannel` that `UpdatePeriod` should follow.

Update process of ASF involves update of entire folder structure that ASF is using, but without touching your own configs or databases located in `config` directory - this means that any extra files unrelated to ASF in its directory might be lost during update. Default value of `24` is a good balance between unnecessary checks, and ASF that is fresh enough.

Unless you have a **strong** reason to disable this feature, you should keep auto-updates enabled within reasonable `UpdatePeriod` **for your own good**. This is not only because we don't support anything but latest stable ASF release, but also because **we give our security guarantee only for latest version**. If you're using outdated ASF version then you're intentionally exposing yourself to all kind of issues, from small bugs, through broken functionality, ending with **[permanent Steam account suspensions](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ#did-somebody-get-banned-for-it)**, so we **strongly recommend**, for your own good, to always ensure that your ASF version is up to date. Auto-updates allow us to react quickly to all kind of issues by disabling or patching problematic code before it can escalate - if you opt out of it, you lose all of our security guarantees and risk consequences from running code that could be potentially harmful, not only to Steam network, but also (by definition) to your own Steam account.

* * *

### `WebLimiterDelay`

`ushort` Typ mit Standardwert von `200`. Diese Eigenschaft definiert in Millisekunden die minimale Verzögerung zwischen dem Senden von zwei aufeinanderfolgenden Anfragen an denselben Steam-Webservice. Eine solche Verzögerung ist erforderlich, da der Dienst **[AkamaiGhost](https://www.akamai.com)**, den Steam intern nutzt, eine Geschwindigkeitsbegrenzung basierend auf der globalen Anzahl der über einen bestimmten Zeitraum gesendeten Anfragen beinhaltet. Unter normalen Umständen ist akamai Block ziemlich schwer zu erreichen, aber unter sehr hohen Arbeitslasten mit einer riesigen laufenden Warteschlange von Anfragen ist es möglich, ihn auszulösen, wenn wir immer wieder zu viele Anfragen über einen zu kurzen Zeitraum senden.

Der Standardwert wurde unter der Annahme festgelegt, dass ASF das einzige Programm ist, das auf die Steam-Webdienste zugreift, insbesondere `steamcommunity.com`, `api.steampowered.com` und `store.steampowered.com`. Wenn du andere Programme verwendest, die Anfragen an dieselben Webdienste senden, dann solltest du sicherstellen, dass dein Programm ähnliche Funktionen wie `WebLimiterDelay` enthält und beide auf das Doppelte des Standardwerts setzt, was `400` wäre. Dies garantiert, dass du unter keinen Umständen mehr als 1 Anfrage pro 200 ms senden wirst.

In general, lowering `WebLimiterDelay` under default value is **strongly discouraged** as it might lead to various IP-related blocks, some of which are possible to be permanent. Default value is good enough for running a single ASF instance on the server, as well as using ASF in normal scenario along with original Steam client. It should be correct for majority of usages, and you should only increase it (never lower it), if - apart from using ASF, you're also using another tool that might send excessive number of requests to the same web-services that ASF is making use of. In short, global number of all requests sent from a single IP to a single Steam domain should never exceed more than 1 request per 200 ms.

Wenn du keinen Grund hast, diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

* * *

### `WebProxy`

`string` Typ mit Standardwert von `null`. This property defines a web proxy address that will be used for all internal http and https requests sent by ASF's `HttpClient`, especially to services such as `github.com`, `steamcommunity.com` and `store.steampowered.com`. Proxying ASF requests in general has no advantages, but it's exceptionally useful for bypassing various kinds of firewalls, especially the great firewall in China.

This property is defined as uri string:

> A URI string is composed of a scheme (http or https), a host, and an optional port. An example of a complete uri string is `"http://contoso.com:8080"`.

If your proxy requires user authentication, you will also need to set up `WebProxyUsername` and/or `WebProxyPassword`. If there is no such need, setting up this property alone is sufficient.

Right now ASF uses web proxy only for `http` and `https` requests, which **do not** include internal Steam network communication done within ASF's internal Steam client. There are currently no plans for supporting that, mainly due to missing **[SK2](https://github.com/SteamRE/SteamKit)** functionality. If you need/want it to happen, I'd suggest starting from there.

Wenn du keinen Grund hast, diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

* * *

### `WebProxyPassword`

`string` Typ mit Standardwert von `null`. This property defines password field used in basic, digest, NTLM, and Kerberos authentication that is supported by a target `WebProxy` machine providing proxy functionality. If your proxy doesn't require user credentials, there is no need for you to input anything here. Using this option makes sense only if `WebProxy` is used as well, as it has no effect otherwise.

Wenn du keinen Grund hast, diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

* * *

### `WebProxyUsername`

`string` Typ mit Standardwert von `null`. This property defines username field used in basic, digest, NTLM, and Kerberos authentication that is supported by a target `WebProxy` machine providing proxy functionality. If your proxy doesn't require user credentials, there is no need for you to input anything here. Using this option makes sense only if `WebProxy` is used as well, as it has no effect otherwise.

Wenn du keinen Grund hast, diese Eigenschaft zu bearbeiten, solltest du sie auf dem Standard belassen.

* * *

**[Zum Seitenanfang](#konfiguration)**

* * *

## Bot Konfiguration

Wie du bereits wissen solltest, sollte jeder Bot seine eigene Konfiguration haben. Die Beispiel-Bot-Konfiguration ist in der Datei `example.json` enthalten, die für die Bot-Konfiguration verwendet werden sollte. Simply **copy paste** `example.json` to a new file, and remember to name it appropriately, as it will be your bot instance. You should start from configuring your **primary** account, so some good suggestions for filename is `primary.json`, `1.json` or `YourNickname.json`.

**Notice:** There are several names which are reserved and can't be used for bot configs. Those are: **ASF**, **example** and **minimal**. ASF will ignore such configuration files. ASF will also ignore configuration files starting with a dot.

Nachdem du entschieden hast, wie du deinen Bot nennen möchtest, öffne seine Datei und beginne mit der Konfiguration. Du solltest die folgende Struktur beachten:

```json
{
    "AcceptGifts": false,
    "AutoSteamSaleEvent": false,
    "BotBehaviour": 0,
    "CustomGamePlayedWhileFarming": null,
    "CustomGamePlayedWhileIdle": null,
    "Enabled": false,
    "FarmingOrders": [],
    "GamesPlayedWhileIdle": [],
    "HoursUntilCardDrops": 3,
    "IdlePriorityQueueOnly": false,
    "IdleRefundableGames": true,
    "LootableTypes": [1, 3, 5],
    "MatchableTypes": [5],
    "OnlineStatus": 1,
    "PasswordFormat": 0,
    "Paused": false,
    "RedeemingPreferences": 0,
    "SendOnFarmingFinished": false,
    "SendTradePeriod": 0,
    "ShutdownOnFarmingFinished": false,
    "SteamLogin": null,
    "SteamMasterClanID": 0,
    "SteamParentalCode": null,
    "SteamPassword": null,
    "SteamTradeToken": null,
    "SteamUserPermissions": {},
    "TradingPreferences": 0,
    "TransferableTypes": [1, 3, 5],
    "UseLoginKeys": true
}
```

**Tipp:** Damit der Bot richtig funktioniert, solltest du mindestens die Eigenschaften `Enabled`, `SteamLogin` und `SteamPassword` bearbeiten. Ich schlage auch vor, einen Blick auf einige Feinabstimmungen wie `HoursUntilCardDrops` zu werfen, aber all das ist optional. ASF-Konfigurationen sind ziemlich fortschrittlich, um es dir zu ermöglichen, deine Bots und ASF so einzustellen, wie du willst. Wenn du ein solches fortgeschrittenes Setup nicht "benötigst", musst du nicht wirklich detailliert in jede Konfigurationseigenschaft schauen. Es liegt an dir, wie einfach oder wie komplex ASF sein sollte.

* * *

Alle Optionen werden im Folgenden erläutert:

### `AcceptGifts`

`bool` Typ mit Standardwert von `false`. Wenn aktiviert, akzeptiert und löst ASF automatisch alle Steam-Geschenke (einschließlich Guthaben-Geschenkgutscheine), die an den Bot geschickt werden. Dazu gehören auch Geschenke, die von anderen Benutzern als denjenigen gesendet werden, die in `SteamUserPermissions` definiert sind. Bedenke, dass Geschenke, die an die E-Mail-Adresse geschickt werden, nicht direkt an den Client weitergeleitet werden, so dass ASF diese ohne deine Hilfe nicht annehmen wird.

Diese Option wird nur für Alternativkonten empfohlen, da es sehr wahrscheinlich ist, dass du nicht automatisch alle Geschenke einlösen möchtest, die an dein Hauptkonto gesendet werden. Wenn du dir nicht sicher bist, ob du diese Funktion aktivieren möchtest oder nicht, behalte sie mit dem Standardwert `false`.

* * *

### `AutoSteamSaleEvent`

`bool` Typ mit Standardwert von `false`. During Steam summer/winter sale events Steam is known for providing you extra cards for browsing discovery queue each day, as well as voting in the Steam awards. When this option is enabled, ASF will automatically check Steam discovery queue and Steam awards each 6 hours, and clear them if needed. This option is not recommended if you want to do those actions yourself, and typically it should make sense only on bot accounts. Moreover, you need to ensure that your account is at least of level `8` if you expect to receive those cards in the first place. Wenn du dir nicht sicher bist, ob du diese Funktion aktivieren möchtest oder nicht, behalte sie mit dem Standardwert `false`.

Please note that due to constant Valve issues, changes and problems, **we give no guarantee whether this function will work properly**, therefore it's entirely possible that this option **will not work at all**. We do not accept **any** bug reports, neither support requests for this option. It's offered with absolutely no guarantees, you're using it at your own risk.

* * *

### `BotBehaviour`

`byte flags` Typ mit Standardwert von `0`. This property defines ASF bot-like behaviour during various events, and is defined as below:

| Wert | Name                          | Beschreibung                                                                                |
| ---- | ----------------------------- | ------------------------------------------------------------------------------------------- |
| 0    | None                          | Kein spezielles Bot-Verhalten, der am wenigsten invasive Modus, Standard                    |
| 1    | RejectInvalidFriendInvites    | Führt dazu, dass ASF ungültige Freundschaftseinladungen ablehnt (anstatt sie zu ignorieren) |
| 2    | RejectInvalidTrades           | Wird ASF dazu veranlassen, ungültige Handelsangebote abzulehnen (anstatt sie zu ignorieren) |
| 4    | RejectInvalidGroupInvites     | Führt dazu, dass ASF ungültige Gruppeneinladungen ablehnt (anstatt sie zu ignorieren)       |
| 8    | DismissInventoryNotifications | Veranlasst ASF dazu, alle Inventar-Benachrichtigungen automatisch zu entfernen              |
| 16   | MarkReceivedMessagesAsRead    | Führt dazu, dass ASF automatisch alle empfangenen Nachrichten als gelesen markiert          |

Bitte bedenke, dass diese Eigenschaft das Feld `flags` ist, daher ist es möglich, eine beliebige Kombination von verfügbaren Werten auszuwählen. Check out **[flags mapping](#json-mapping)** if you'd like to learn more. Not enabling any of flags results in `None` option.

In general you want to modify this property if you expect from ASF to do certain amount of automation related to its activity, as it'd be expected from a bot account, but not a primary account used in ASF. Therefore, changing this property makes sense mainly for alt accounts, although you're free to use selected options for main accounts as well.

Normal (`None`) ASF behaviour is to only automate things that user wants (e.g. cards farming or `SteamTradeMatcher` offers, if set in `TradingPreferences`). This is the least invasive mode, and it's beneficial to majority of users since you remain in full control over your account and you can decide yourself whether to allow certain out-of-scope interactions, or not.

Invalid friend invite is an invite that doesn't come from user with `FamilySharing` permission (defined in `SteamUserPermissions`) or above. ASF in normal mode ignores those invites, as you'd expect, giving you free choice whether to accept them, or not. `RejectInvalidFriendInvites` will cause those invites to be automatically rejected, which will practically disable option for other people to add you to their friend list (as ASF will deny all such requests, apart from people defined in `SteamUserPermissions`). Unless you want to outright deny all friend invites, you shouldn't enable this option.

Invalid trade offer is an offer that isn't accepted through built-in ASF module. More on this matter can be found in **[trading](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading)** section which explicitly defines what types of trade ASF is willing to accept automatically. Valid trades are also defined by other settings, especially `TradingPreferences`. `RejectInvalidTrades` will cause all invalid trade offers to be rejected, instead of being ignored. Unless you want to outright deny all trade offers that aren't automatically accepted by ASF, you shouldn't enable this option.

Invalid group invite is an invite that doesn't come from `SteamMasterClanID` group. ASF in normal mode ignores those group invites, as you'd expect, allowing you to decide yourself if you want to join particular Steam group or not. `RejectInvalidGroupInvites` will cause all those group invites to be automatically rejected, effectively making it impossible to invite you to any other group than `SteamMasterClanID`. Unless you want to outright deny all group invites, you shouldn't enable this option.

Wenn du dir nicht sicher bist, wie du diese Option konfigurieren sollst, ist es am besten, sie auf dem Standardwert zu belassen.

* * *

### `CustomGamePlayedWhileFarming`

`string` Typ mit Standardwert von `null`. When ASF is farming, it can display itself as "Playing non-steam game: `CustomGamePlayedWhileFarming`" instead of currently farmed game. This can be useful if you would like to let your friends know that you're farming, yet you don't want to change default `OnlineStatus`. Please note that ASF cannot guarantee the actual display order of Steam network, therefore this is only a suggestion that may, or may not, display properly. Default value of `null` disables this feature.

* * *

### `CustomGamePlayedWhileIdle`

`string` Typ mit Standardwert von `null`. Similar to `CustomGamePlayedWhileFarming`, but for the situation when ASF has nothing to do (as account is fully farmed). Default value of `null` disables this feature.

* * *

### `Enabled`

`bool` Typ mit Standardwert von `false`. This property defines if bot is enabled. Enabled bot instance (`true`) will automatically start on ASF run, while disabled bot instance (`false`) will need to be started manually. By default every bot is disabled, so you probably want to switch this property to `true` for all of your bots that should be started automatically.

* * *

### `FarmingOrders`

`ImmutableHashSet<byte>` Typ mit einem leeren Standardwert. This property defines the **preferred** farming order used by ASF for given bot account. Currently there are following farming orders available:

| Wert | Name                      | Beschreibung                                                                     |
| ---- | ------------------------- | -------------------------------------------------------------------------------- |
| 0    | Unordered                 | Keine Sortierung, leichte Verbesserung der CPU-Leistung                          |
| 1    | AppIDsAscending           | Versuche Spiele mit den niedrigsten `appID`s zuerst zu sammeln                   |
| 2    | AppIDsDescending          | Versuche Spiele mit den höchsten `appID`s zuerst zu sammeln                      |
| 3    | CardDropsAscending        | Try to farm games with lowest number of card drops remaining first               |
| 4    | CardDropsDescending       | Try to farm games with highest number of card drops remaining first              |
| 5    | HoursAscending            | Try to farm games with lowest number of hours played first                       |
| 6    | HoursDescending           | Try to farm games with highest number of hours played first                      |
| 7    | NamesAscending            | Try to farm games in alphabetical order, starting from A                         |
| 8    | NamesDescending           | Try to farm games in reverse alphabetical order, starting from Z                 |
| 9    | Random                    | Try to farm games in totally random order (different on each run of the program) |
| 10   | BadgeLevelsAscending      | Try to farm games with lowest badge levels first                                 |
| 11   | BadgeLevelsDescending     | Try to farm games with highest badge levels first                                |
| 12   | RedeemDateTimesAscending  | Try to farm oldest games on our account first                                    |
| 13   | RedeemDateTimesDescending | Try to farm newest games on our account first                                    |
| 14   | MarketableAscending       | Try to farm games with unmarketable card drops first                             |
| 15   | MarketableDescending      | Try to farm games with marketable card drops first                               |

Since this property is an array, it allows you to use several different settings in your fixed order. For example, you can include values of `15`, `11` and `7` in order to sort by marketable games first, then by those with highest badge level, and finally alphabetically. As you can guess, the order actually matters, as reverse one (`7`, `11` and `15`) achieves something entirely different. Majority of people will probably use just one order out of all of them, but in case you want to, you can also sort further by extra parameters.

Also notice the word "try" in all above descriptions - the actual ASF order is heavily affected by selected **[cards farming algorithm](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)** and sorting will affect only results that ASF considers same performance-wise. For example, in `Simple` algorithm, selected `FarmingOrders` should be entirely respected in current farming session (as every game has the same performance value), while in `Complex` algorithm actual order is affected by hours first, and then sorted according to chosen `FarmingOrders`. This will lead to different results, as games with existing playtime will have a priority over others, so effectively ASF will prefer games with highest playtime first, and only then sorting everything further by your chosen `FarmingOrders`. Therefore, this config property is only a **suggestion** that ASF will try to respect, as long as it doesn't affect performance negatively (in this case, ASF will always prefer performance over `FarmingOrders`).

There is also idling priority queue that is accessible through `iq` **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. If it's used, actual idling order is sorted firstly by performance, then by idling queue, and finally by your `FarmingOrders`.

* * *

### `GamesPlayedWhileIdle`

`ImmutableHashSet<uint>` Typ mit einem leeren Standardwert. If ASF has nothing to farm it can play your specified steam games (`appID`s) instead. Playing games in such manner increases your "hours played" of those games, but nothing else apart of it. This feature can be enabled at the same time with `CustomGamePlayedWhileIdle` in order to play your selected games while showing custom status in Steam network, but in this case, like in `CustomGamePlayedWhileFarming` case, the actual display order is not guaranteed. Please note that Steam allows ASF to play only up to `32` `appID`s, therefore if you put more games than that, only first `32` will be respected (and extra ones being ignored).

* * *

### `HoursUntilCardDrops`

`byte` Typ mit Standardwert von `3`. This property defines if account has card drops restricted, and if yes, for how many initial hours. Restricted card drops means that account is not receiving any card drops from given game until the game is played for at least `HoursUntilCardDrops` hours. Unfortunately there is no magical way to detect that, so ASF relies on you. This property affects **[cards farming algorithm](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)** that will be used. Setting this property properly will maximize profits and minimize time required for cards to be farmed. Remember that there is no obvious answer whether you should use one or another value, since it fully depends on your account. It seems that older accounts which never asked for refund have unrestricted card drops, so they should use a value of `0`, while new accounts and those who did ask for refund have restricted card drops with a value of `3`. Dies ist jedoch nur eine Theorie und sollte nicht als Tatsache betrachtet werden. The default value for this property was set based on "lesser evil" and majority of use cases.

* * *

### `IdlePriorityQueueOnly`

`bool` Typ mit Standardwert von `false`. This property defines if ASF should consider for automatic idling only apps that you added yourself to priority idling queue available with `iq` **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. When this option is enabled, ASF will skip all `appIDs` that are missing on the list, effectively allowing you to cherry-pick games for automatic ASF idling. Keep in mind that if you didn't add any games to the queue then ASF will act as if there is nothing to idle on your account. Wenn du dir nicht sicher bist, ob du diese Funktion aktivieren möchtest oder nicht, behalte sie mit dem Standardwert `false`.

* * *

### `IdleRefundableGames`

`bool` Typ mit Standardwert von `true`. This property defines if ASF is permitted to idle games that are still refundable. A refundable game is a game that we bought in last 2 weeks through Steam Store and we didn't play it for longer than 2 hours yet, as stated **[here](https://store.steampowered.com/steam_refunds)**. By default when this option is set to `true`, ASF ignores Steam refunds entirely and idles everything, as most people expect. However, you can change this option to `false` if you want to ensure that ASF won't idle any of your refundable games too soon, allowing you to evaluate those games yourself and refund if needed without worrying about ASF affecting playtime negatively. Please note that if you disable this option then games you purchased from Steam Store won't be idled by ASF for up to 14 days since redeem date. If you're unsure whether you want this feature enabled or not, keep it with default value of `true`.

* * *

### `LootableTypes`

`ImmutableHashSet<byte>` type with default value of `1, 3, 5` steam item types. This property defines ASF behaviour when looting - both manual and automatic. ASF will ensure that only items from `LootableTypes` will be included in a trade offer, therefore this property allows you to choose what you want to receive in a trade offer that is being sent to you.

| Wert | Name              | Beschreibung                                                  |
| ---- | ----------------- | ------------------------------------------------------------- |
| 0    | Unknown           | Every type that doesn't fit in any of the below               |
| 1    | BoosterPack       | Unpacked booster pack                                         |
| 2    | Emoticon          | Emoticon to use in Steam Chat                                 |
| 3    | FoilTradingCard   | Foil variant of `TradingCard`                                 |
| 4    | ProfileBackground | Profile background to use on your Steam profile               |
| 5    | TradingCard       | Steam trading card, being used for crafting badges (non-foil) |
| 6    | SteamGems         | Steam gems being used for crafting boosters, sacks included   |

Please note that regardless of the settings above, ASF will only ask for Steam (`appID` of 753) community (`contextID` of 6) items, so all game items, gifts and likewise, are excluded from the trade offer by definition.

Default ASF setting is based on most common usage of the bot, with looting only booster packs, and trading cards (including foils). The property defined here allows you to alter that behaviour in whatever way that satisfies you. Please keep in mind that all types not defined above will show as `Unknown` type, which is especially important when Valve releases some new Steam item, that will be marked as `Unknown` by ASF as well, until it's added here (in the future release). That's why in general it's not recommended to include `Unknown` type in your `LootableTypes`, unless you know what you're doing, and you also understand that ASF will send your entire inventory in a trade offer if Steam Network gets broken again and reports all your items as `Unknown`. My strong suggestion is to not include `Unknown` type in the `LootableTypes`, even if you expect to loot everything.

* * *

### `MatchableTypes`

`ImmutableHashSet<byte>` type with default value of `5` Steam item types. This property defines which Steam item types are permitted to be matched when `SteamTradeMatcher` option in `TradingPreferences` is enabled. Types are defined as below:

| Wert | Name              | Beschreibung                                                  |
| ---- | ----------------- | ------------------------------------------------------------- |
| 0    | Unknown           | Every type that doesn't fit in any of the below               |
| 1    | BoosterPack       | Unpacked booster pack                                         |
| 2    | Emoticon          | Emoticon to use in Steam Chat                                 |
| 3    | FoilTradingCard   | Foil variant of `TradingCard`                                 |
| 4    | ProfileBackground | Profile background to use on your Steam profile               |
| 5    | TradingCard       | Steam trading card, being used for crafting badges (non-foil) |
| 6    | SteamGems         | Steam gems being used for crafting boosters, sacks included   |

Of course, types that you should use for this property typically include only `2`, `3`, `4` and `5`, as only those types are supported by STM. Please note that **ASF is not a trading bot** and **will NOT care about price or rarity**, which means that if you use it e.g. with `Emoticon` type, then ASF will be happy to trade your 2x rare emoticon for 1x rare 1x common, as that makes progress towards badge (in this case emoticons) completion. Please evaluate twice if you're fine with that. Unless you know what you're doing, you should keep it with default value of `5`.

* * *

### `OnlineStatus`

`byte` Typ mit Standardwert von `1`. This property specifies Steam community status that the bot will be announced with after logging in to Steam network. Currently you can choose one of below statuses:

| Wert | Name           |
| ---- | -------------- |
| 0    | Offline        |
| 1    | Online         |
| 2    | Busy           |
| 3    | Away           |
| 4    | Snooze         |
| 5    | LookingToTrade |
| 6    | LookingToPlay  |
| 7    | Invisible      |

`Offline` status is extremely useful for primary accounts. As you should know, farming a game actually shows your steam status as "Playing game: XXX", which can be misleading to your friends, confusing them that you're playing a game while actually you're only farming it. Using `Offline` status solves that issue - your account will never be shown as "in-game" when you're farming steam cards with ASF. This is possible thanks to the fact that ASF does not have to sign in into Steam Community in order to work properly, so we're in fact playing those games, connected to Steam network, but without announcing our online presence at all. Keep in mind that played games using offline status will still count towards your playtime, and show as "recently played" on your profile.

In addition to that, this feature is also important if you want to receive notifications and unread messages when ASF is running, while not keeping Steam client open at the same time. This is because ASF acts as a Steam client itself, and whether ASF would like it or not, Steam broadcasts all those messages and other events to it. This is not a problem if you have both ASF and your own Steam client running, as both clients receive exactly the same events. However, if just ASF is running, Steam network could mark certain events and messages as "delivered", despite of your traditional Steam client not receiving it due to not being present. Offline status also solves this problem, as ASF is never considered for any community events in this case, so all unread messages and other events will be properly marked as unread when you come back.

It's important to note that ASF running on `Offline` mode will **not** be able to receive commands in usual Steam chat way, as the chat, as well as entire community presence is in fact, entirely offline. A solution to this issue is using `Invisible` mode instead which works in a similar way (not exposing status), but keeps the ability to receive and respond to messages (so also a potential to dismiss notifications and unread messages as stated above). `Invisible` mode makes the most sense on alt accounts that you don't want to expose (status-wise), but still be able to send commands to.

However, there is one catch with `Invisible` mode - it doesn't go well with primary accounts. This is because **any** Steam session that is currently online **exposes** the status, even if ASF itself does not. This is caused by the current limitation/bug of the Steam network that isn't possible to be fixed on ASF side, so if you want to use `Invisible` mode you will also need to ensure that **all** other sessions to the same account use `Invisible` mode as well. This will be the case on alt accounts where ASF is hopefully the only active session, but on primary accounts you'll almost always prefer to show as `Online` to your friends, hiding only ASF activity, and in this case `Invisible` mode will be entirely useless for you (we recommend to use `Offline` mode instead). Hopefully this limitation/bug will be eventually solved in the future by Valve, but I wouldn't expect that to happen anytime soon...

If you're unsure how to set up this property, it's recommended to use a value of `0` (`Offline`) for primary accounts, and default `1` (`Online`) otherwise.

* * *

### `PasswordFormat`

`byte` Typ mit Standardwert von `0`. This property defines the format of `SteamPassword` property, and currently supports - `0` for `PlainText`, `1` for `AES` and `2` for `ProtectedDataForCurrentUser`. Please refer to **[Security](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security)** section if you want to learn more, as you'll need to ensure that `SteamPassword` property indeed includes password in matching `PasswordFormat`. In other words, when you change `PasswordFormat` then your `SteamPassword` should be **already** in that format, not just aiming to be. Unless you know what you're doing, you should keep it with default value of `0`.

* * *

### `Paused`

`bool` Typ mit Standardwert von `false`. This property defines initial state of `CardsFarmer` module. With default value of `false`, bot will automatically start farming when it's started, either because of `Enabled` or `start` command. Switching this property to `true` should be done only if you want to manually `resume` automatic farming process, for example because you want to use `play` all the time and never use automatic `CardsFarmer` module - this works exactly the same as `pause` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. Wenn du dir nicht sicher bist, ob du diese Funktion aktivieren möchtest oder nicht, behalte sie mit dem Standardwert `false`.

* * *

### `RedeemingPreferences`

`byte flags` Typ mit Standardwert von `0`. This property defines ASF behaviour when redeeming cd-keys, and is defined as below:

| Wert | Name             | Beschreibung                                                                   |
| ---- | ---------------- | ------------------------------------------------------------------------------ |
| 0    | None             | No redeeming preferences, typical                                              |
| 1    | Forwarding       | Forward keys unavailable to redeem to other bots                               |
| 2    | Distributing     | Distribute all keys among itself and other bots                                |
| 4    | KeepMissingGames | Keep keys for (potentially) missing games when forwarding, leaving them unused |

Please notice that this property is `flags` field, therefore it's possible to choose any combination of available values. Check out **[flags mapping](#json-mapping)** if you'd like to learn more. Not enabling any of flags results in `None` option.

`Forwarding` will cause bot to forward a key that is not possible to redeem, to another connected and logged on bot that is missing that particular game (if possible to check). The most common situation is forwarding `AlreadyPurchased` game to another bot that is missing that particular game, but this option also covers other scenarios, such as `DoesNotOwnRequiredApp`, `RateLimited` or `RestrictedCountry`.

`Distributing` will cause bot to distribute all received keys among itself and other bots. This means that every bot will get a single key from the batch. Typically this is used only when you're redeeming many keys for the same game, and you want to evenly distribute them among your bots, as opposed to redeeming keys for various different games. This feature makes no sense if you're redeeming only one key in a single `redeem` action (as there are no extra keys to be distributed).

`KeepMissingGames` will cause bot to skip `Forwarding` when we can't be sure if key being redeemed is in fact owned by our bot, or not. This basically means that `Forwarding` will apply **only** to `AlreadyPurchased` keys, instead of covering also other cases such as `DoesNotOwnRequiredApp`, `RateLimited` or `RestrictedCountry`. Typically you might want to use this option on primary account, to ensure that keys being redeemed on it won't be forwarded further if your bot for example becomes temporarily `RateLimited`. As you can guess from the description, this field has absolutely no effect if `Forwarding` is not enabled.

Enabling both `Forwarding` and `Distributing` will add distributing feature on top of forwarding one, which makes ASF trying to redeem one key on all bots firstly (forwarding) before moving to the next one (distributing). Typically you want to use this option only when you want `Forwarding`, but with altered behaviour of switching the bot on key being used, instead of always going in-order with every key (which would be `Forwarding` alone). This behaviour can be beneficial if you know that majority or even all of your keys are tied to the same game, because in this situation `Forwarding` alone would try to redeem everything on one bot firstly (which makes sense if your keys are for unique games), and `Forwarding` + `Distributing` will switch the bot on the next key, "distributing" the task of redeeming new key onto another bot than the initial one (which makes sense if keys are for the same game, skipping one pointless attempt per key).

The actual bots order for all of the redeeming scenarios is alphabetical, excluding bots that are unavailable (not connected, stopped or likewise). Please keep in mind that there is per-IP and per-account hourly limit of redeeming tries, and every redeem try that didn't end with `OK` contributes to failed tries. ASF will do its best to minimize number of `AlreadyPurchased` failures, e.g. by trying to avoid forwarding a key to another bot that already owns that particular game, but it's not always guaranteed to work due to how Steam is handling licenses. Using redeeming flags such as `Forwarding` or `Distributing` will always increase your likelyhood to hit `RateLimited`.

Also keep in mind that you can't forward or distribute keys to bots that you do not have access to. This should be obvious, but ensure that you're at least `Operator` of all the bots you want to include in your redeeming process, for example with `status ASF` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**.

* * *

### `SendOnFarmingFinished`

`bool` Typ mit Standardwert von `false`. When ASF is done with farming given account, it can automatically send steam trade containing everything farmed up to this point to user with `Master` permission, which is very convenient if you don't want to bother with trades yourself. This option works the same as `loot` command, therefore keep in mind that it requires user with `Master` permission set, you might also require valid `SteamTradeToken`, including using an account that is actually eligible for trading. In addition to initiating `loot` after finishing farming, ASF will also initiate `loot` on each new items notification (when not farming), and after completing each trade that results in new items (always) when this option is active. This is especially useful for "forwarding" items received from other people to our account. It's strongly recommended to use this feature together with **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** being set, as there is no point in sending automatic trades if you need to manually confirm them. If you're not sure how to set this property, leave it with default value of `false`.

* * *

### `SendTradePeriod`

`byte` Typ mit Standardwert von `0`. Diese Eigenschaft funktioniert sehr ähnlich wie `SendOnFarmingFinished` Eigenschaft, mit einem Unterschied - anstatt Handelsangebote zu senden, wenn das Sammeln abgeschlossen ist, können wir sie auch alle `SendTradePeriod` Stunden senden, unabhängig davon, wie viel wir noch zu sammeln haben. This is useful if you want to `loot` your alt accounts on usual basis instead of waiting for it to finish farming. Default value of `0` disables this feature, if you want your bot to send you trade e.g. every day, you should put `24` here. It's strongly recommended to use this feature together with **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** being set, as there is no point in sending automatic trades if you need to manually confirm them. If you're not sure how to set this property, leave it with default value of `0`.

* * *

### `ShutdownOnFarmingFinished`

`bool` Typ mit Standardwert von `false`. ASF is "occupying" an account for the whole time of process being active. When given account is done with farming, ASF periodically checks it (every `IdleFarmingPeriod` hours), if perhaps some new games with steam cards were added in the meantime, so it can resume farming of that account without a need to restart the process. This is useful for majority of people, as ASF can automatically resume farming when needed. However, you may actually want to stop the process when given account is fully farmed, you can achieve that by setting this property to `true`. When enabled, ASF will proceed with logging off when account is fully farmed, which means that it won't be periodically checked or occupied anymore. You should decide yourself if you prefer ASF to work on given bot instance for the whole time, or if perhaps ASF should stop it when farming process is done. When all accounts are stopped and process is not running in `--process-required` **[mode](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-Line-Arguments)**, ASF will shutdown as well. If you're not sure how to set this property, leave it with default value of `false`.

* * *

### `SteamLogin`

`string` Typ mit Standardwert von `null`. This property defines your steam login - the one you use for logging in to steam. In addition to defining steam login here, you may also keep default value of `null` if you want to enter your steam login on each ASF startup instead of putting it in the config. This may be useful for you if you don't want to save sensitive data in config file.

* * *

### `SteamMasterClanID`

`ulong` Typ mit Standardwert von `0`. This property defines the steamID of the steam group that bot should automatically join, including its group chat. You can check steamID of your group by navigating to its **[page](https://steamcommunity.com/groups/archiasf)**, then adding `/memberslistxml?xml=1` to the end of the link, so the link will look like **[this](https://steamcommunity.com/groups/archiasf/memberslistxml?xml=1)**. Then you can get steamID of your group from the result, it's in `<groupID64>` tag. In above example it would be `103582791440160998`. In addition to trying to join given group at startup, the bot will also automatically accept group invites to this group, which makes it possible for you to invite your bot manually if your group has private membership. If you don't have any group dedicated for your bots, you should keep this property with default value of `0`.

* * *

### `SteamParentalCode`

`string` Typ mit Standardwert von `null`. This property defines your steam parental PIN. ASF requires an access to resources protected by steam parental, therefore if you use that feature, you need to provide ASF with parental unlock PIN, so it can operate normally. Default value of `null` means that there is no steam parental PIN required to unlock this account, and this is probably what you want if you don't use steam parental functionality. In addition to defining steam parental PIN here, you may also use value of `0` if you want to enter your steam parental PIN on each ASF startup instead of putting it in the config. This may be useful for you if you don't want to save sensitive data in config file.

* * *

### `SteamPassword`

`string` Typ mit Standardwert von `null`. Diese Eigenschaft definiert dein Steam-Passwort - das, mit dem du dich bei Steam anmeldest. In addition to defining steam password here, you may also keep default value of `null` if you want to enter your steam password on each ASF startup instead of putting it in the config. This may be useful for you if you don't want to save sensitive data in config file.

* * *

### `SteamTradeToken`

`string` Typ mit Standardwert von `null`. Wenn du deinen Bot auf deiner Freundesliste hast, dann kann der Bot dir sofort ein Handelsangebot schicken, ohne sich um den Handels-Code zu kümmern, deshalb kannst du diese Eigenschaft auf dem Standardwert von `null` belassen. Wenn du dich jedoch dazu entscheidest, deinen Bot NICHT auf deiner Freundesliste zu haben, dann musst du einen Handels-Code dessen Benutzers generieren und eintragen an den dieser Bot Handelsangebote senden möchte. In other words, this property should be filled with trade token of the account that is defined with `Master` permission in `SteamUserPermissions` of **this** bot instance.

In order to find your token, as logged in user with `Master` permission, navigate **[here](https://steamcommunity.com/my/tradeoffers/privacy)** and take a look at your trade URL. The token we're looking for is made out of 8 characters after `&token=` part in your trade URL. You should copy and put those 8 characters here, as `SteamTradeToken`. Do not include whole trading URL, neither `&token=` part, only token itself.

* * *

### `SteamUserPermissions`

` ImmutableDictionary<ulong, byte>` Typ mit einem leeren Standardwert. This property is a dictionary property which maps given Steam user identified by his 64-bit steam ID, to `byte` number that specifies his permission in ASF instance. Currently available bot permissions in ASF are defined as:

| Wert | Name          | Beschreibung                                                                                                                                                                                       |
| ---- | ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0    | None          | No permission, this is mainly a reference value that is assigned to steam IDs missing in this dictionary - there is no need to define anybody with this permission                                 |
| 1    | FamilySharing | Provides minimum access for family sharing users. Once again, this is mainly a reference value since ASF is capable of automatically discovering steam IDs that we permitted for using our library |
| 2    | Operator      | Provides basic access to given bot instances, mainly adding licenses and redeeming keys                                                                                                            |
| 3    | Master        | Provides full access to given bot instance                                                                                                                                                         |

In short, this property allows you to handle permissions for given users. Permissions are important mainly for access to ASF **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**, but also for enabling many ASF features, such as accepting trades. For example you might want to set your own account as `Master`, and give `Operator` access to 2-3 of your friends so they can easily redeem keys for your bot with ASF, while **not** being eligible e.g. for stopping it. Thanks to that you can easily assign permissions to given users and let them use your bot to some specified by you degree.

We recommend to set exactly one user as `Master`, and any amount you wish as `Operator`s and below. While it's technically possible to set multiple `Master`s and ASF will work correctly with them, for example by accepting all of their trades sent to the bot, ASF will use only one of them (with lowest steam ID) for every action that requires a single target, for example a `loot` request, so also properties like `SendOnFarmingFinished` or `SendTradePeriod`. If you perfectly understand those limitations, especially the fact that `loot` request will always send items to the `Master` with lowest steam ID, regardless of the `Master` that actually executed the command, then you can define multiple users with `Master` permission here, but we still recommend a single master scheme - multiple masters scheme is discouraged setup that is not supported.

It's nice to note that there is one more extra `Owner` permission, which is declared as `SteamOwnerID` global config property. You can't assign `Owner` permission to anybody here, as `SteamUserPermissions` property defines only permissions that are related to the bot instance, and not ASF as a process.

* * *

### `TradingPreferences`

`byte flags` Typ mit Standardwert von `0`. Diese Eigenschaft definiert das ASF-Verhalten beim Handeln und ist wie folgt definiert:

| Wert | Name                | Beschreibung                                                                                                                                                                    |
| ---- | ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0    | None                | No trading preferences - accepts only `Master` trades                                                                                                                           |
| 1    | AcceptDonations     | Accepts trades in which we're not losing anything                                                                                                                               |
| 2    | SteamTradeMatcher   | Accepts dupes-matching **[STM](https://www.steamtradematcher.com)**-like trades. Visit **[trading](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading)** for more info |
| 4    | MatchEverything     | Requires `SteamTradeMatcher` to be set, and in combination with it - also accepts bad trades in addition to good and neutral ones                                               |
| 8    | DontAcceptBotTrades | Doesn't automatically accept `loot` trades from other bot instances                                                                                                             |

Bitte bedenke, dass diese Eigenschaft das Feld `flags` ist, daher ist es möglich, eine beliebige Kombination von verfügbaren Werten auszuwählen. Schau dir **[JSON-Mapping](#json-mapping)** an, wenn du mehr darüber erfahren möchtest. Wenn keines der Flags aktiviert wird, wird die Option `None` verwendet.

Für weitere Erläuterungen zur ASF-Handelslogik und zur Beschreibung jedes verfügbaren Flags besuche bitte den Abschnitt **[Handeln](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading-de-DE)**.

* * *

### `TransferableTypes`

`ImmutableHashSet<byte>` Typ mit Standardwert von `1, 3, 5` Steam-Gegenstands-Typen. Diese Eigenschaft definiert, welche Steam-Gegenstands-Typen für die Übertragung zwischen Bots während `transfer` **[Befehl](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE)** berücksichtigt werden. ASF wird sicherstellen, dass nur Gegenstände von `TransferableTypes` in ein Handelsangebot aufgenommen werden, daher kannst du mit dieser Eigenschaft wählen, was du in einem Handelsangebot erhalten möchtest, das an einen deiner Bots gesendet wird.

| Wert | Name              | Beschreibung                                                  |
| ---- | ----------------- | ------------------------------------------------------------- |
| 0    | Unknown           | Jeder Typ, der nicht in eine der folgenden Kategorien passt   |
| 1    | BoosterPack       | Unpacked booster pack                                         |
| 2    | Emoticon          | Emoticon to use in Steam Chat                                 |
| 3    | FoilTradingCard   | Foil variant of `TradingCard`                                 |
| 4    | ProfileBackground | Profile background to use on your Steam profile               |
| 5    | TradingCard       | Steam trading card, being used for crafting badges (non-foil) |
| 6    | SteamGems         | Steam gems being used for crafting boosters, sacks included   |

Bitte bedenke, dass ASF unabhängig von den obigen Einstellungen nur nach Steam (`appID` von 753) Community (`contextID` von 6) Gegenständen fragt, so dass alle Spiel-Gegenstände und Geschenke und dergleichen per Definition aus dem Handelsangebot ausgeschlossen sind.

Die Standard-ASF-Einstellung basiert auf der am häufigsten verwendeten Nutzung des Bots, wobei nur Booster Packs und Handels-Karten (einschließlich Folien) übertragen werden. Die hier definierte Eigenschaft ermöglicht es dir, dieses Verhalten so zu verändern, dass es dich zufrieden stellt. Bitte bedenke, dass alle nicht oben definierten Typen als `Unknown` Typ angezeigt werden, was besonders wichtig ist, wenn Valve einen neuen Steam-Gegenstand veröffentlicht, der ebenfalls von ASF als `Unknown` markiert wird, bis er hier (in der zukünftigen Version) hinzugefügt wird. Deshalb ist es im Allgemeinen nicht empfehlenswert, `Unknown` in deinen `TransferableTypes` einzugeben, es sei denn, du weißt, was du tust, und du verstehst auch, dass ASF dein gesamtes Inventar in einem Handelsangebot versenden wird, wenn das Steam-Netzwerk wieder unterbrochen wird und alle deine Gegenstände als `Unknown` meldet. Mein nachdrücklicher Vorschlag ist, `Unknown` nicht in das Feld `TransferableTypes` einzutragen, selbst wenn du alles übertragen möchtest.

* * *

### `UseLoginKeys`

`bool` Typ mit Standardwert von `true`. Diese Eigenschaft legt fest, ob ASF den Anmelde-Schlüssel-Mechanismus für dieses Steam-Konto verwenden soll. Der Anmelde-Schlüssel-Mechanismus funktioniert sehr ähnlich wie die "Remember me"-Option des offiziellen Steam-Clients, die es ASF ermöglicht, einen temporären, einmaligen Anmelde-Schlüssel für den nächsten Anmeldeversuch zu speichern und zu verwenden, wodurch die Angabe von Passwort, Steam Guard oder 2FA-Code übergangen wird, solange unser Anmelde-Schlüssel gültig ist. Der Anmelde-Schlüssel wird in der Datei `BotName.db` gespeichert und automatisch aktualisiert. Aus diesem Grund musst du kein Passwort/SteamGuard/2FA-Code angeben, nachdem du dich nur einmal erfolgreich mit ASF angemeldet hast.

Die Anmelde-Schlüssel werden standardmäßig für deine Bequemlichkeit verwendet, so dass du nicht bei jedem Anmeldevorgang `SteamPassword`, SteamGuard oder 2FA-Code (wenn du nicht **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-de-DE)** verwendest) eingeben musst. Es ist auch eine überlegene Alternative, da der Anmelde-Schlüssel nur einmalig verwendet werden kann und dein ursprüngliches Passwort in keiner Weise offenbart. Genau die gleiche Methode wird von deinem ursprünglichen Steam-Client verwendet, der deinen Konto-Namen und Anmelde-Schlüssel für deinen nächsten Anmeldeversuch speichert, was praktisch die gleiche ist wie die Verwendung von `SteamLogin` mit `UseLoginKeys` und leerem `SteamPassword` in ASF.

Einige Leute sind jedoch vielleicht sogar über dieses kleine Detail besorgt, deshalb steht dir diese Option hier zur Verfügung, wenn du sicherstellen möchtest, dass ASF keine Art von Daten speichert, die es ermöglichen würden, die vorherige Sitzung nach dem Schließen wieder aufzunehmen, was dazu führt, dass eine vollständige Authentifizierung bei jedem Anmeldeversuch obligatorisch ist. Das Deaktivieren dieser Option funktioniert genau so, wie wenn du "Remember me" im offiziellen Steam-Client nicht aktivierst. Wenn du nicht weißt, was du tust, solltest du es bei dem Standardwert `true` belassen.

**[Zum Seitenanfang](#konfiguration)**

* * *

## Dateistruktur

ASF benutzt eine einfache Dateistruktur.

    ├── config
    │     ├── ASF.json
    │     ├── ASF.db
    │     ├── Bot1.json
    │     ├── Bot1.db
    │     ├── Bot1.bin
    │     ├── Bot2.json
    │     ├── Bot2.db
    │     ├── Bot2.bin
    │     └── ...
    ├── ArchiSteamFarm.dll
    ├── log.txt
    └── ...
    

Um ASF an einen neuen Ort zu verschieben, z.B. auf einen anderen PC, genügt es, das Verzeichnis `config` allein zu verschieben/kopieren, und das ist die empfohlene Methode, um jede Form von "ASF-Backups" durchzuführen.

Die `log.txt` Datei enthält das Protokoll, das durch deinen letzten ASF-Lauf erzeugt wurde. Diese Datei enthält keine sensiblen Informationen und ist äußerst nützlich, wenn es um Probleme, Abstürze oder einfach nur als Information an dich geht, was im letzten ASF-Lauf passiert ist. Wir werden sehr oft nach einer Log-Datei fragen, wenn du auf Probleme oder Fehler stößt. ASF verwaltet diese Datei automatisch für dich, aber du kannst die ASF **[Protokollierung](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Logging-de-DE)**s-Modul weiter optimieren, wenn du ein fortgeschrittener Benutzer bist.

`config` Verzeichnis ist der Ort, der die Konfiguration für ASF enthält, einschließlich aller seiner Bots.

`ASF.json` ist eine globale ASF-Konfigurationsdatei. Diese Konfiguration wird verwendet, um anzugeben, wie sich ASF als Prozess verhält, was alle Bots und das Programm selbst betrifft. Dort findest du globale Eigenschaften wie ASF-Prozesseigentümer, automatische Aktualisierungen oder Debugging.

`BotName.json` ist eine Konfiguration der gegebenen Bot-Instanz. Diese Konfiguration wird verwendet, um das Verhalten einer gegebenen Bot-Instanz festzulegen, daher sind diese Einstellungen nur für diesen Bot spezifisch und nicht für andere bestimmt. Dies ermöglicht es dir, Bots mit verschiedenen Einstellungen zu konfigurieren, und nicht unbedingt alle funktionieren auf die gleiche Weise.

Neben den Konfigurationsdateien verwendet ASF auch das Verzeichnis `config` zum Speichern von Datenbanken.

`ASF.db` ist eine globale ASF-Datenbankdatei. Es fungiert als globaler dauerhafter Speicher und wird zum Speichern verschiedener Informationen im Zusammenhang mit dem ASF-Prozess verwendet, wie z.B. IPs von lokalen Steam-Servern. **Du solltest diese Datei nicht bearbeiten**.

`BotName.db` ist eine Datenbank der jeweiligen Bot-Instanz. Diese Datei wird verwendet, um wichtige Daten der jeweiligen Bot-Instanz im dauerhaften Speicher zu speichern, wie z.B. Anmelde-Schlüssel oder ASF 2FA. **Du solltest diese Datei nicht bearbeiten**.

`BotName.bin` ist eine spezielle Datei der jeweiligen Bot-Instanz, die Informationen über den Steam-Sentry-Hash enthält. Der Sentry-Hash wird für die Authentifizierung mit dem `SteamGuard` Mechanismus verwendet, sehr ähnlich Steam-Datei `ssfn`. **Du solltest diese Datei nicht bearbeiten**.

`BotName.keys` ist eine spezielle Datei, die zum Importieren von Produkt-Schlüsseln in den **[Hintergrundproduktschlüsselaktivierer ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer-de-DE)** verwendet werden kann. Es ist nicht zwingend erforderlich und wird nicht generiert, aber von ASF anerkannt. Diese Datei wird nach dem erfolgreichen Import der Produkt-Schlüssel automatisch gelöscht.

`BotName.maFile` ist eine spezielle Datei, die für den Import von **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-de-DE)** verwendet werden kann. Es ist nicht zwingend erforderlich und wird nicht generiert, aber von ASF erkannt, wenn dein `BotName` ASF 2FA noch nicht verwendet. Diese Datei wird nach erfolgreichem Import von ASF 2FA automatisch gelöscht.

**[Zum Seitenanfang](#konfiguration)**

* * *

## JSON-Mapping

Jede Konfigurationseigenschaft hat ihren Typ. Der Typ der Eigenschaft definiert Werte, die für sie gültig sind. Du kannst nur Werte verwenden, die für einen bestimmten Typ gültig sind - wenn du einen ungültigen Wert verwendest, dann kann ASF deine Konfiguration nicht verarbeiten.

**Wir empfehlen dringend, den ConfigGenerator für die Generierung von Konfigurationen zu verwenden** - er handhabt den größten Teil des Low-Level-Sachen (wie z.B. die Typ-Validierung) für dich, so dass du nur die richtigen Werte eingeben musst, und du musst auch die unten angegebenen Variablentypen nicht verstehen. Dieser Abschnitt ist hauptsächlich für Personen gedacht, die Konfigurationen manuell erstellen/bearbeiten, damit sie wissen, welche Werte sie verwenden können.

Von ASF verwendete Typen sind native C#-Typen, die im Folgenden aufgeführt werden:

* * *

`bool` - Boolescher Typ, der nur `true` und `false` Werte akzeptiert.

Beispiel: `"Enabled": true`

* * *

`byte` - Unsignierter Byte-Typ, der nur ganze Zahlen von `0` bis `255` (einschließlich) akzeptiert.

Beispiel: `"ConnectionTimeout": 60`

* * *

`uint` - Unsignierter Ganzzahl-Typ, der nur ganze Zahlen von `0` bis `4294967295` (einschließlich) akzeptiert.

* * *

`ulong` - Unsignierter long integer Typ, der nur ganze Zahlen von `0` bis `18446744073709551615` (einschließlich) akzeptiert.

Beispiel: `"SteamMasterClanID": 103582791440160998`

* * *

`string` - Zeichenketten-Typ, der jede beliebige Zeichenfolge akzeptiert, einschließlich der leeren Folge `""` und `null`. Sowohl die leere Sequenz als auch der Wert `null` werden von ASF gleich behandelt, so dass es deiner Präferenz überlassen bleibt, welche davon du verwenden möchtest.

Beispiele: `"SteamLogin": null`, `"SteamLogin": ""`, `"SteamLogin": "MeinLogin"`

* * *

`ImmutableHashSet<valueType>` - Unveränderliche Sammlung (Satz) von eindeutigen Werten in gegebenen `valueType`. In JSON ist es definiert als Array von Elementen in gegebenem `valueType`.

Beispiel für `ImmutableHashSet<uint>`: `"Blacklist": [267420, 303700, 335590]`

* * *

`ImmutableDictionary<keyType, valueType>` - Unveränderliches Wörterbuch (Map), das einen in seinem `keyType` angegebenen Schlüssel auf den in seinem `valueType` angegebenen Wert abbildet. In JSON wird es als Objekt mit Schlüssel-Wert-Paaren definiert. Bedenke, dass `keyType` in diesem Fall immer angegeben wird, auch wenn es sich um einen Wert-Typ wie `ulong` handelt.

Beispiel für `ImmutableDictionary<ulong, byte>`: `"SteamUserPermissions": { "76561198174813138": 3, "76561198174813137": 1 }`

* * *

`flags` - Das Attribut Flags kombiniert mehrere verschiedene Eigenschaften zu einem Endwert, indem es bitweise Operationen anwendet. Auf diese Weise kannst du jede mögliche Kombination aus verschiedenen zulässigen Werten gleichzeitig wählen. Der Endwert wird als Summe aller Werte der aktivierten Optionen berechnet.

Zum Beispiel, wenn folgende Werte angegeben werden:

| Wert | Name |
| ---- | ---- |
| 0    | None |
| 1    | A    |
| 2    | B    |
| 4    | C    |

Die Verwendung von `B + C` würde zu einem Wert von `6` führen, die Verwendung von `A + C` würde zu einem Wert von `5` führen, die Verwendung von `C` würde zu einem Wert von `4`führen und so weiter. Dies ermöglicht es dir, jede mögliche Kombination von aktivierten Werten zu erstellen - wenn du dich dazu entschieden hast, alle zu aktivieren, indem du `None + A + B + C` wählst, bekommst du den Wert von `7`. Außerdem ist zu beachten, dass das Flag mit dem Wert `0` per Definition in allen anderen verfügbaren Kombinationen aktiviert ist, daher ist es sehr oft ein Flag, das nichts Bestimmtes aktiviert (wie `None`).

Wie du sehen kannst, haben wir im obigen Beispiel 3 verfügbare Flags zum Ein-/Ausschalten (`A`, `B`, `C`) und 8 mögliche Werte insgesamt (`None -> 0`, `A -> 1`, `B -> 2`, `A+B -> 3`, `C -> 4`, `A+C -> 5`, `B+C -> 6`, `A+B+C -> 7`).

**[Zum Seitenanfang](#konfiguration)**

* * *

## Kompatibilitätsmapping

Aufgrund von JavaScript-Beschränkungen, da einfache `ulong` Felder in JSON bei Verwendung des webbasierten ConfigGenerators nicht ordnungsgemäß serialisiert werden können, werden `ulong` Felder als Zeichenketten mit `s_` Präfix in der resultierenden Konfiguration dargestellt. Dazu gehört z.B. `"SteamOwnerID": 76561198006963719`, der von unserem ConfigGenerator als `"s_SteamOwnerID": "76561198006963719"` geschrieben wird. ASF enthält die richtige Logik für die automatische Verarbeitung dieses Zeichenketten-Mappings, so dass `s_` Einträge in deine Konfigurationen tatsächlich gültig und korrekt generiert sind. Wenn du selbst Konfigurationen generierst, empfehlen wir, nach Möglichkeit an den ursprünglichen `ulong` Feldern festzuhalten, aber wenn du dazu nicht in der Lage bist, kannst du auch diesem Schema folgen und sie als Zeichenketten mit `s_` Präfix an ihren Namen kodieren. Wir hoffen, diese JavaScript-Einschränkung irgendwann aufheben zu können.

**[Zum Seitenanfang](#konfiguration)**

* * *

## Konfigurations-Kompatibilität

Es ist für ASF oberste Priorität, mit älteren Konfigurationen kompatibel zu bleiben. Wie du bereits weißt, werden fehlende Konfigurationseigenschaften genauso behandelt, wie wenn sie mit ihren **Standardwerten** angegeben würden. Wenn also eine neue Konfigurationseigenschaft in einer neuen Version von ASF eingeführt wird, bleiben all deine Konfigurationen **kompatibel** mit der neuen Version, und ASF wird diese neue Konfigurationseigenschaft so behandeln, wie sie mit ihrem **Standardwert** definiert wäre. Du kannst jederzeit Konfigurationseigenschaften nach deinen Wünschen hinzufügen, entfernen oder bearbeiten. Wir empfehlen, definierte Konfigurationseigenschaften nur auf die zu ändernden Eigenschaften zu beschränken, da du auf diese Weise automatisch Standardwerte für alle anderen vererbst, nicht nur deine Konfiguration sauber hältst, sondern auch die Kompatibilität erhöhst, falls wir beschließen, einen Standardwert für Eigenschaften zu ändern, die du nicht explizit selbst festlegen möchtest. Zögere nicht, die Beispiel-Konfigurationsdatei `minimal.json` zu lesen, die diesem Konzept folgt.

**[Zum Seitenanfang](#konfiguration)**

* * *

## Automatisches Nachladen

Ab ASF V2.1.6.2+ ist es möglich, dass Konfigurationen "on-the-fly" geändert werden - dadurch wird ASF automatisch:

- Eine neue Bot-Instanz erstellen (und startet sie bei Bedarf), wenn du die Konfiguration erstellst
- Stoppt (falls erforderlich) und entfernt alte Bot-Instanz, wenn du ihre Konfiguration löschst
- Stoppt (und startet bei Bedarf) eine beliebige Bot-Instanz, wenn du ihre Konfiguration bearbeitest
- Startet den Bot (falls erforderlich) unter neuem Namen neu, wenn du seine Konfiguration umbenennst

All dies ist transparent und wird automatisch durchgeführt, ohne dass das Programm neu gestartet oder andere (nicht betroffene) Bot-Instanzen beendet werden müssen.

Darüber hinaus wird sich ASF auch selbst neu starten (wenn `AutoRestart` es erlaubt), wenn du die ASF-Kern-Konfiguration `ASF.json` änderst. Ebenso wird das Programm beendet, wenn du es löschst oder umbenennst.

**[Zum Seitenanfang](#konfiguration)**