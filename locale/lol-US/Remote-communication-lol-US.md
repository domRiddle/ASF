# Remote communication

This section elaborates on remote communication that ASF includes, including further explanation on how one can influence it. While we don't consider anything below as malicious or otherwise unwanted, and neither we're legally obliged to disclose it, we want you to better understand the program functionality especially in regards to your privacy and data being shared.

## Steam

ASF communicates with Steam network (**[CM servers](https://api.steampowered.com/ISteamDirectory/GetCMList/v1?cellid=0)**), as well as **[Steam API](api.steampowered.com)**, **[Steam store](https://store.steampowered.com)** and **[Steam community](https://steamcommunity.com)**.

It's not possible to disable any of the above communication, as it's the core foundation ASF is based on in order to provide its basic functionality. You'll need to refrain from using ASF if you're not comfortable with the above.

## Steam group

ASF communicates with our **[Steam group](https://steamcommunity.com/groups/archiasf)**. The group provides you with announcements, especially new versions, critical issues, Steam problems and other things that are important to keep community updated. It also allows you to use our technical support, by asking questions, resolving problems, reporting issues or suggesting improvements. By default, accounts used in ASF will automatically join the group upon login.

You can decide to opt-out of joining the group by disabling `SteamGroup` flag in bot's **[`RemoteCommunication`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#remotecommunication)** settings.

## GitHub

ASF communicates with **[GitHub's API](https://api.github.com)** in order to fetch **[ASF releases](https://github.com/JustArchiNET/ArchiSteamFarm/releases)** for the update functionality. This is done as part of auto-updates (if you've kept **[`UpdatePeriod`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#updateperiod)** enabled), as well as `update` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. You can influence ASF's communication with GitHub through **[`UpdateChannel`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#updatechannel)** property - setting it to `None` will result in disabling entire update functionality, including GitHub communication in this regard.

## ASF's server

ASF communicates with **[our own server](https://asf.justarchi.net)** for more advanced functionality. In particular, this includes:
- Verifying checksums of ASF builds downloaded from GitHub against our own independent database to ensure that all downloaded builds are legitimate (free of malware, MITM attacks or other tampering)
- Announcing your bot in **[our listing](https://asf.justarchi.net/STM)** if you've enabled `SteamTradeMatcher` in **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#tradingpreferences)** and meet other criteria
- Downloading currently available bots to trade from **[our listing](https://asf.justarchi.net/STM)** if you've enabled `MatchActively` in **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#tradingpreferences)** and meet other criteria

As a security measure, it's not possible to disable checksum verification for ASF builds. However, you can disable auto-updates entirely if you'd like to avoid this, as described above in the GitHub section.

You can decide to opt-out of being announced in the listing by disabling `PublicListing` flag in bot's **[`RemoteCommunication`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#remotecommunication)** settings. This might be useful if you'd like to run `SteamTradeMatcher` bot without being announced at the same time.

Downloading bots from our listing is mandatory for `MatchActively` setting, you'll need to disable that setting if you're unwilling to accept that.

---

## PUBLIC ASF STM LISTIN

R PUBLIC ASF STM LISTIN IZ LOCATD ON **[R WEBSIET](https://asf.justarchi.net/STM)** AN USD AS PUBLIC SERVICE 4 BOTH ASF USERS DAT MAK USE OV `MatchActively`, AS WELL AS ASF AN NON-ASF USERS 4 MANUAL MATCHIN.

### HOW IT EGSAKTLY WERKZ

ASF SENDZ INITIAL DATA ONCE AFTR LOGGIN IN, DAT CONTAINS ALL PROPERTIEZ PUBLIC LISTIN MAKEZ USE OV. DEN, EVRY 10 MINUTEZ ASF SENDZ WAN, VRY TINY "HEARTBEAT" REQUEST DAT NOTIFIEZ R SERVR DAT TEH BOT IZ STILL UP AN RUNNIN. IF 4 SUM REASON TEH HEARTBEAT DIDNT ARRIV, 4 EXAMPLE DUE 2 NETWORKIN ISSUEZ, DEN ASF WILL RETRY SENDIN IT EACH MINIT, TIL SERVR REGISTERS IT.

DIS ALLOWS R WEBSIET 2 RECORD WHICH ACCOUNTS CAN BE USD 4 MATCHIN, AS WELL AS IF THEYRE STILL ACTIV. THX 2 DAT, R WEBSIET CAN SHOW ALL ASF 2FA+STM ACCOUNTS DAT WUZ ACTIV IN **LAST 15 MINUTEZ**.

USERS R SORTD ACCORDIN 2 THEIR INVENTORIEZ (IN DESCENDIN ORDR) - `MatchEverything` BOTS WIF `Any` BANNR DAT ACCEPT ALL 1:1 TRADEZ, DEN `MatchableTypes` UNIQUE GAMEZ COUNT, AN FINALLY `MatchableTypes` ITEMS COUNT.

PLZ NOWT DAT U WILL **NOT** BE DISPLAYD ON TEH WEBSIET IF U DO NOT MEET ALL OV TEH REQUIREMENTS. ASF WONT EVEN BOTHR COMMUNICATIN WIF R SERVR IN DIS CASE, SO SECOND POINT IZ ENTIRELY SKIPPD 4 U IF U DIDNT INTENSHUNALLY ENABLE `SteamTradeMatcher` IN ORDR 2 HALP YOURSELF MATCH DUPEZ. ALSO PUBLIC LISTIN IZ COMPATIBLE ONLY WIF LATEST STABLE VERSHUN OV ASF AN CUD REFUSE 2 DISPLAY OUTDATD BOTS, ESPECIALLY IF THEYRE MISIN CORE FUNCSHUNALITY DAT CAN BE FINDZ ONLY IN NEWR VERSHUNS.

### API

ASF STM LISTIN ONLY ACCEPTS ASF BOTS 4 TIEM BEAN. THAR IZ NOWAI 2 LIST THIRD-PARTY BOTS ON R LISTIN 4 NAO (AS WE CANT REVIEW THEIR CODE EASILY AN ENSURE THEY MEET R ENTIRE TRADIN LOGIC).

IF URE LOOKIN 4 EASY WAI 2 ACCES R LISTIN IN PROGRAMMATIC WAI, WE HAS VRY SIMPLE **[/API/BOTS](https://asf.justarchi.net/Api/Bots)** ENDPOINT DAT U CAN USE. DIS AR TEH ALSO TEH ENDPOINT DAT ASF USEZ INTERNALLY 4 `MatchActively` USERS.

### Privacy policy

If you agree to being listed in our listing, by enabling `SteamTradeMatcher` and not refusing `PublicListing`, as specified above, we'll temporarily store some of your Steam account details on our server in order to provide the core functionality.

Public info (exposed by Steam to every interested party) includes:
- UR STEAM IDENTIFICATOR (IN 64-BIT FORM, 4 GENERATIN LINKZ)
- UR NICKNAME (4 DISPLAY PURPOSEZ)
- UR AVATAR (HASH, 4 DISPLAY PURPOSEZ)
- TOTAL NUMBR OV `MatchableTypes` STEAM ITEMS IN UR INVENTORY (4 DISPLAY PURPOSEZ AN MATCHIN)
- TOTAL NUMBR OV UNIQUE GAMEZ DAT ABOOV `MatchableTypes` STEAM ITEMS R MADE OV (4 DISPLAY PURPOSEZ AN MATCHIN)

Private info (selected data required for providing the functionality) includes:
- UR **[TRADIN TOKEN](https://steamcommunity.com/my/tradeoffers/privacy)** (SO PEEPS OUTSIDE OV UR FRIENDLIST CAN SEND U TRADEZ)
- UR `MatchableTypes` (4 DISPLAY PURPOSEZ AN MATCHIN)
- Value of `MatchEverything` in your **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#tradingpreferences)** (for display purposes and matching)

ASF server will **not** collect, store or otherwise process any other data not listed above, without prior important notice in the changelog, and a very good practical reason in the first place. We do not consider anything above to be a serious matter, and we mention it to let you know what precisely ASF does apart of what you configured it to do yourself, so people can better understand the process.

Your data will be automatically hidden from general public in up to 15 minutes since the moment you stop using our listing, whether due to change of settings or not having ASF launched anymore. In addition to that, it'll be automatically deleted from our server (including all backup copies) in up to 7 days since the above happening.