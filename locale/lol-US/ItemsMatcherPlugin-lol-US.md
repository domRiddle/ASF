# ItemsMatcherPlugin

`ItemsMatcherPlugin` is official ASF **[plugin](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins)** that extends ASF with ASF STM listing features. In particular, this includes `PublicListing` in **[`RemoteCommunication`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#remotecommunication)** and `MatchActively` in **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#tradingpreferences)**. ASF comes with `ItemsMatcherPlugin` bundled together with the release, therefore it's ready for usage right away.

---

## `PublicListing`

Public listing, as the name implies, is listing of currently available ASF STM bots. It's located on **[our website](https://asf.justarchi.net/STM)**, managed automatically and used as a public service for both ASF users that make use of `MatchActively`, as well as ASF and non-ASF users for manual matching.

While `PublicListing` is enabled by default, please note that you will **not** be displayed on the website if you do not meet all of the requirements, especially `SteamTradeMatcher`, which isn't enabled by default. For people that do not meet the criteria, even if they kept `PublicListing` enabled, ASF doesn't communicate with the server in any way. Public listing is also compatible only with latest stable version of ASF and may refuse to display outdated bots, especially if they're missing core functionality that can be found only in newer versions.

### HOW IT EGSAKTLY WERKZ

ASF SENDZ INITIAL DATA ONCE AFTR LOGGIN IN, DAT CONTAINS ALL PROPERTIEZ PUBLIC LISTIN MAKEZ USE OV. DEN, EVRY 10 MINUTEZ ASF SENDZ WAN, VRY TINY "HEARTBEAT" REQUEST DAT NOTIFIEZ R SERVR DAT TEH BOT IZ STILL UP AN RUNNIN. IF 4 SUM REASON TEH HEARTBEAT DIDNT ARRIV, 4 EXAMPLE DUE 2 NETWORKIN ISSUEZ, DEN ASF WILL RETRY SENDIN IT EACH MINIT, TIL SERVR REGISTERS IT. This way our server knows precisely which bots are still running and ready to accept trade offers. ASF will also send initial announcement on as-needed basis, for example if it detects that our inventory has changed since the previous one.

We display all ASF 2FA+STM accounts that were active in the **last 15 minutes**. Users are sorted according to their relative usefulness - `MatchEverything` bots which are shown with `Any` banner that accept all 1:1 trades, then unique games count, and finally items count.

### API

ASF STM LISTIN ONLY ACCEPTS ASF BOTS 4 TIEM BEAN. There is no way to list third-party bots on our listing for now, as we can't review their code easily and ensure they meet our entire trading logic. Participation in the listing therefore requires latest stable ASF version, although it can run with custom **[plugins](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins)**.

For consumers of the listing, we have a very simple **[`/Api/Listing/Bots`](https://asf.justarchi.net/Api/Listing/Bots)** endpoint that you can use. It includes all the data we have, apart from inventories of users which are part of `MatchActively` feature exclusively.

### PRIVACY POLICY

If you agree to being listed in our listing, by enabling `SteamTradeMatcher` and not refusing `PublicListing`, as specified above, we'll temporarily store some of your Steam account details on our server in order to provide the expected functionality.

PUBLIC INFO (EXPOSD BY STEAM 2 EVRY INTERESTD PARTY) INCLUDEZ:
- UR STEAM IDENTIFICATOR (IN 64-BIT FORM, 4 GENERATIN LINKZ)
- UR NICKNAME (4 DISPLAY PURPOSEZ)
- UR AVATAR (HASH, 4 DISPLAY PURPOSEZ)

Semi-public info (exposed by Steam to every interested party if you meet listing requirements) includes:
- Your **[inventory](https://steamcommunity.com/my/inventory/#753_6)** (so people can use `MatchActively` against your items).

PRIVATE INFO (SELECTD DATA REQUIRD 4 PROVIDIN TEH FUNCSHUNALITY) INCLUDEZ:
- UR **[TRADIN TOKEN](https://steamcommunity.com/my/tradeoffers/privacy)** (SO PEEPS OUTSIDE OV UR FRIENDLIST CAN SEND U TRADEZ)
- Your `MatchableTypes` setting (for display purposes and matching)
- Your `MatchEverything` setting (for display purposes and matching)
- Your `MaxTradeHoldDuration` setting (so other people know whether you're willing to accept their trades)

---

## `MatchActively`

`MatchActively` setting is active version of **[`SteamTradeMatcher`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading#steamtradematcher)** including interactive matching in which the bot will send trades to other people. IT CAN WERK STANDALONE, OR TOGETHR WIF `SteamTradeMatcher` SETTIN. This feature requires `LicenseID` to be set, as it uses third-party server and paid resources to operate.

IN ORDR 2 MAK USE OV DAT OPSHUN, U HAS SET OV REQUIREMENTS 2 MEET. AT TEH MINIMUM U MUST HAS **[UNRESTRICTD](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663)** AKOWNT, **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-lol-US#asf-2fa)** ACTIV AN AT LEAST WAN VALID TYPE IN `MatchableTypes`, SUCH AS TRADIN CARDZ.

If you meet all of the requirements above, ASF will periodically communicate with our **[public ASF STM listing](#publiclisting)** in order to actively match bots that are currently available.

- In each round ASF will fetch our inventory and inventory of all available bots listed in order to find `MatchableTypes` items that can be matched. Thanks to communicating directly with our server, this process requires a single request and we have immediate information whether any available bot offers something interesting for us - if match is found, ASF will send and confirm trade offer automatically.
- EACH SET (COMPOSISHUN OV APPID, TYPE AN RARITY OV TEH ITEM) CAN BE MATCHD IN SINGLE ROUND ONLY ONCE. DIS AR TEH IMPLEMENTD IN ORDR 2 MINIMIZE "ITEMS NO LONGR AVAILABLE" AN AVOID NED 2 WAIT 4 EACH BOT 2 REACT BEFORE SENDIN ALL TEH TRADEZ. IZ ALSO TEH PRIMARY REASON Y MATCHIN IZ COMPOSD OV ROUNDZ AN NOT WAN ONGOIN PROCES.
- ASF WILL SEND NO MOAR THAN `255` ITEMS IN SINGLE TRADE, AN NO MOAR THAN `5` TRADEZ 2 SINGLE USR IN SINGLE ROUND. DIS AR TEH IMPOSD BY STEAM LIMITS, AS WELL AS R OWN LOAD-BALANCIN.

DIS MODULE IZ SUPPOSD 2 BE TRANZPARENT. MATCHIN WILL START IN APPROXIMATELY `1` HOUR SINCE ASF START, AN WILL REPEAT ITSELF EACH `6` HOURS (IF NEEDD). `MatchActively` FEACHUR IZ AIMD 2 BE USD AS LONG-RUN, PERIODICAL MEASURE 2 ENSURE DAT WERE ACTIVELY HEADIN TOWARDZ SETS COMPLESHUN, BUT WITHOUT SHORT-TURM TIEM AN RESOURCEZ PRESURE DAT WUD HAPPEN IF DIS WUZ OFFERD AS COMMAND. TEH TARGET USERS OV DIS MODULE R PRIMARY ACCOUNTS AN "STASH" ALT ACCOUNTS, ALTHOUGH IT CAN BE USD BY ANY BOT DAT IZ NOT SET 2 `MatchEverything`.

ASF DOEZ ITZ BEST 2 MINIMIZE TEH AMOUNT OV REQUESTS AN PRESURE GENERATD BY USIN DIS OPSHUN, WHILE AT TEH SAME TIEM MAXIMIZIN EFFICIENCY OV MATCHIN 2 TEH UPPR LIMIT. TEH EGSAKT ALGORITHM OV CHOOSIN TEH BOTS 2 MATCH AN OTHERWIZE ORGANIZE TEH WHOLE PROCES, IZ ASFS IMPLEMENTASHUN DETAIL AN CAN CHANGE IN REGARDZ 2 FEEDBACK, SITUASHUN AN POSIBLE FUCHUR IDEAS.

TEH CURRENT VERSHUN OV TEH ALGORITHM MAKEZ ASF PRIORITIZE `Any` BOTS FURST, ESPECIALLY DOSE WIF BETTR DIVERSITY OV GAMEZ DAT THEIR ITEMS R FRUM. When running out of `Any` bots, ASF will move on to the `Fair` ones upon same diversity rule. ASF will try to match every available bot at least once, to ensure that we're not missing on a possible set progress.

`MatchActively` TAKEZ INTO AKOWNT BOTS DAT U BLACKLISTD FRUM TRADIN THRU `tbadd` **[COMMAND](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-lol-US)** AN WILL NOT ATTEMPT 2 ACTIVELY MATCH THEM. DIS CAN BE USD 4 TELLIN ASF WHICH BOTS IT SHUD NEVR MATCH, EVEN IF THEYD HAS POTENTIAL DUPEZ 4 US 2 USE.

---

### Why do I need a `LicenseID` to use `MatchActively`? Wasn't it free before?

ASF is, and remains, free and open-source, as it was established at the start of the project back in October 2015. Our program is also entirely non-commercial, we do not earn anything from contributions to it, building or publishing. Over those past 7+ years ASF has received tremendous amount of development, and it's still being improved and enhanced with every monthly stable release mostly by a single person, **[JustArchi](https://github.com/JustArchi)** - with no strings attached. The only funding we receive is from non-obligatory donations that come from our users.

For a very long time, until October 2022, `MatchActively` feature was part of ASF core and available for everyone to use. In October 2022, Valve, the company behind Steam, has put very severe rate limits that rendered previous functionality entirely broken, with no solution available. The feature therefore had to be removed from ASF core in version 5.4.1.0.

`MatchActively` was resurrected as part of official `ItemsMatcher` plugin that further enhances ASF with active cards matching functionality. Resurrecting `MatchActively` feature required from us **extraordinary amount of work** to create ASF backend, entirely new service hosted on a server, with more than a thousand of proxies attached for resolving inventories, all exclusively to allow ASF clients to make use of `MatchActively` like before. Due to the amount of work involved, as well as resources that are not free and require to be paid on monthly basis by us (domain, server, proxies), we've decided to offer this functionality to our sponsors, that is, people that already support ASF project on monthly basis. Our goal isn't to profit from it, but rather, cover the **monthly costs** that are exclusively linked with offering this option - that's why we offer it basically for nothing, but we do have to charge a little for it as we can't pay hundreds of dollars from our own pockets just to make it available for you. We hope that you understand.

---

### How can I get an access?

`ItemsMatcher` is offered as part of $5+ sponsor tier on **[JustArchi's GitHub](https://github.com/sponsors/JustArchi)**. Simply become a sponsor of $5 tier (or higher), then read **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#licenseid)** section to obtain and fill `LicenseID`.

The license allows you to send limited amount of requests to the server. $5 tier allows you to use `MatchActively` for one account, which should be suitable for majority of people. $10 tier allows you to use it on three accounts. If you require more resources, **[let us know](mailto:ASF@JustArchi.net)**.