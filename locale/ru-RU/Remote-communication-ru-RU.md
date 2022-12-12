# Удаленная связь

This section elaborates on remote communication that ASF includes, including further explanation on how one can influence it. Хотя мы не считаем что-либо из приведенного ниже вредоносным или иным образом нежелательным, и мы не обязаны по закону раскрывать это, мы хотим, чтобы вы лучше понимали функциональные возможности программы, особенно в отношении вашей конфиденциальности и обмена данными.

## Steam

ASF communicates with Steam network (**[CM servers](https://api.steampowered.com/ISteamDirectory/GetCMList/v1?cellid=0)**), as well as **[Steam API](https://steamcommunity.com/dev)**, **[Steam store](https://store.steampowered.com)** and **[Steam community](https://steamcommunity.com)**.

It's not possible to disable any of the above communication, as it's the core foundation ASF is based on in order to provide its basic functionality. You'll need to refrain from using ASF if you're not comfortable with the above.

## Группа Steam

ASF связывается с нашей **[группой Steam](https://steamcommunity.com/groups/archiasf)**. Группа предоставляет вам объявления, в частности о новых версиях, критических проблемах, проблемах Steam и других вещах, которые важны участникам сообщества. Это также позволяет вам использовать нашу техническую поддержку, задавая вопросы, решая проблемы, сообщая о проблемах или предлагая улучшения. По умолчанию учетные записи, используемые в ASF, автоматически присоединяются к группе при входе в систему.

You can decide to opt-out of joining the group by disabling `SteamGroup` flag in bot's **[`RemoteCommunication`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#remotecommunication)** settings.

## GitHub

ASF communicates with **[GitHub's API](https://api.github.com)** in order to fetch **[ASF releases](https://github.com/JustArchiNET/ArchiSteamFarm/releases)** for the update functionality. This is done as part of auto-updates (if you've kept **[`UpdatePeriod`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#updateperiod)** enabled), as well as `update` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. You can influence ASF's communication with GitHub through **[`UpdateChannel`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#updatechannel)** property - setting it to `None` will result in disabling entire update functionality, including GitHub communication in this regard.

## ASF сервер

ASF communicates with **[our own server](https://asf.justarchi.net)** for more advanced functionality. In particular, this includes:
- Verifying checksums of ASF builds downloaded from GitHub against our own independent database to ensure that all downloaded builds are legitimate (free of malware, MITM attacks or other tampering)
- Announcing your bot in **[our listing](https://asf.justarchi.net/STM)** if you've enabled `SteamTradeMatcher` in **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#tradingpreferences)** and meet other criteria
- Downloading currently available bots to trade from **[our listing](https://asf.justarchi.net/STM)** if you've enabled `MatchActively` in **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#tradingpreferences)** and meet other criteria

As a security measure, it's not possible to disable checksum verification for ASF builds. However, you can disable auto-updates entirely if you'd like to avoid this, as described above in the GitHub section.

You can decide to opt-out of being announced in the listing by disabling `PublicListing` flag in bot's **[`RemoteCommunication`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#remotecommunication)** settings. This might be useful if you'd like to run `SteamTradeMatcher` bot without being announced at the same time.

Downloading bots from our listing is mandatory for `MatchActively` setting, you'll need to disable that setting if you're unwilling to accept that.