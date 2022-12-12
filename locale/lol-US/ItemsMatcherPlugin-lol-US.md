# Note

This page is work-in-progress. For translators: you may want to wait a bit (until stable release), as the page is still being written and corrected.

---

# Plugin

`ItemsMatcherPlugin` is official ASF plugin that extends ASF with ASF STM listing features. In particular, this includes `PublicListing` in **[`RemoteCommunication`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#remotecommunication)** and `MatchActively` in **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#tradingpreferences)**.

---

## `PublicListing`

R PUBLIC ASF STM LISTIN IZ LOCATD ON **[R WEBSIET](https://asf-backend.justarchi.net/STM)** AN USD AS PUBLIC SERVICE 4 BOTH ASF USERS DAT MAK USE OV `MatchActively`, AS WELL AS ASF AN NON-ASF USERS 4 MANUAL MATCHIN.

PLZ NOWT DAT U WILL **NOT** BE DISPLAYD ON TEH WEBSIET IF U DO NOT MEET ALL OV TEH REQUIREMENTS. ASF WONT EVEN BOTHR COMMUNICATIN WIF R SERVR IN DIS CASE, SO DIS SECSHUN IZ ENTIRELY SKIPPD 4 U IF U DIDNT INTENSHUNALLY ENABLE `SteamTradeMatcher` IN ORDR 2 HALP YOURSELF MATCH DUPEZ. ALSO PUBLIC LISTIN IZ COMPATIBLE ONLY WIF LATEST STABLE VERSHUN OV ASF AN CUD REFUSE 2 DISPLAY OUTDATD BOTS, ESPECIALLY IF THEYRE MISIN CORE FUNCSHUNALITY DAT CAN BE FINDZ ONLY IN NEWR VERSHUNS.

### HOW IT EGSAKTLY WERKZ

ASF SENDZ INITIAL DATA ONCE AFTR LOGGIN IN, DAT CONTAINS ALL PROPERTIEZ PUBLIC LISTIN MAKEZ USE OV. DEN, EVRY 10 MINUTEZ ASF SENDZ WAN, VRY TINY "HEARTBEAT" REQUEST DAT NOTIFIEZ R SERVR DAT TEH BOT IZ STILL UP AN RUNNIN. IF 4 SUM REASON TEH HEARTBEAT DIDNT ARRIV, 4 EXAMPLE DUE 2 NETWORKIN ISSUEZ, DEN ASF WILL RETRY SENDIN IT EACH MINIT, TIL SERVR REGISTERS IT.

DIS ALLOWS R WEBSIET 2 RECORD WHICH ACCOUNTS CAN BE USD 4 MATCHIN, AS WELL AS IF THEYRE STILL ACTIV. THX 2 DAT, R WEBSIET CAN SHOW ALL ASF 2FA+STM ACCOUNTS DAT WUZ ACTIV IN **LAST 15 MINUTEZ**.

USERS R SORTD ACCORDIN 2 THEIR INVENTORIEZ (IN DESCENDIN ORDR) - `MatchEverything` BOTS WIF `Any` BANNR DAT ACCEPT ALL 1:1 TRADEZ, DEN `MatchableTypes` UNIQUE GAMEZ COUNT, AN FINALLY `MatchableTypes` ITEMS COUNT.

### API

ASF STM LISTIN ONLY ACCEPTS ASF BOTS 4 TIEM BEAN. THAR IZ NOWAI 2 LIST THIRD-PARTY BOTS ON R LISTIN 4 NAO (AS WE CANT REVIEW THEIR CODE EASILY AN ENSURE THEY MEET R ENTIRE TRADIN LOGIC).

If you're looking for easy way to access our listing in programmatic way, we have a very simple **[`/Api/Listing/Bots`](https://asf-backend.justarchi.net/Api/Listing/Bots)** endpoint that you can use. DIS AR TEH ALSO TEH ENDPOINT DAT ASF USEZ INTERNALLY 4 `MatchActively` USERS.

### PRIVACY POLICY

IF U AGREE 2 BEAN LISTD IN R LISTIN, BY ENABLIN `SteamTradeMatcher` AN NOT REFUSIN `PublicListing`, AS SPECIFID ABOOV, WELL TEMPORARILY STORE SUM OV UR STEAM AKOWNT DETAILS ON R SERVR IN ORDR 2 PROVIDE TEH CORE FUNCSHUNALITY.

PUBLIC INFO (EXPOSD BY STEAM 2 EVRY INTERESTD PARTY) INCLUDEZ:
- UR STEAM IDENTIFICATOR (IN 64-BIT FORM, 4 GENERATIN LINKZ)
- UR NICKNAME (4 DISPLAY PURPOSEZ)
- UR AVATAR (HASH, 4 DISPLAY PURPOSEZ)

PRIVATE INFO (SELECTD DATA REQUIRD 4 PROVIDIN TEH FUNCSHUNALITY) INCLUDEZ:
- Your **[inventory](https://steamcommunity.com/my/inventory/#753_6)** limited to item types that you've picked in `MatchableTypes` (so people can use `MatchActively` against your items).
- UR **[TRADIN TOKEN](https://steamcommunity.com/my/tradeoffers/privacy)** (SO PEEPS OUTSIDE OV UR FRIENDLIST CAN SEND U TRADEZ)
- UR `MaxTradeHoldDuration` (SO OTHR PEEPS KNOE WHETHR URE WILLIN 2 ACCEPT THEIR TRADEZ)
- UR `MatchableTypes` (4 DISPLAY PURPOSEZ AN MATCHIN)
- Total number of Steam items in your inventory (for display purposes and matching)

---

## `MatchActively`

`MatchActively` setting is active version of **[`SteamTradeMatcher`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading#steamtradematcher)** which includes interactive matching in which the bot will send trades to other people. IT CAN WERK STANDALONE, OR TOGETHR WIF `SteamTradeMatcher` SETTIN. This feature requires `LicenseID` to be set, as it uses third-party server.

IN ORDR 2 MAK USE OV DAT OPSHUN, U HAS SET OV REQUIREMENTS 2 MEET. AT TEH MINIMUM U MUST HAS **[UNRESTRICTD](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663)** AKOWNT, **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-lol-US#asf-2fa)** ACTIV AN AT LEAST WAN VALID TYPE IN `MatchableTypes`, SUCH AS TRADIN CARDZ.

IF U MEET ALL OV TEH REQUIREMENTS ABOOV, ASF WILL PERIODICALLY SPEEK WIF R **[PUBLIC ASF STM LISTIN](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Remote-communication-lol-US#public-asf-stm-listin)** IN ORDR 2 ACTIVELY MATCH BOTS DAT R CURRENTLY AVAILABLE.

- IN EACH ROUND ASF WILL FETCH R INVENTORY AN INVENTORY OV SELECTD BOTS DAT R LISTD IN ORDR 2 FIND `MatchableTypes` ITEMS DAT CAN BE MATCHD. IF MATCH IZ FINDZ, ASF WILL SEND AN CONFIRM TRADE OFFR AUTOMATICALLY.
- EACH SET (COMPOSISHUN OV APPID, TYPE AN RARITY OV TEH ITEM) CAN BE MATCHD IN SINGLE ROUND ONLY ONCE. DIS AR TEH IMPLEMENTD IN ORDR 2 MINIMIZE "ITEMS NO LONGR AVAILABLE" AN AVOID NED 2 WAIT 4 EACH BOT 2 REACT BEFORE SENDIN ALL TEH TRADEZ. IZ ALSO TEH PRIMARY REASON Y MATCHIN IZ COMPOSD OV ROUNDZ AN NOT WAN ONGOIN PROCES.
- ASF WILL SEND NO MOAR THAN `255` ITEMS IN SINGLE TRADE, AN NO MOAR THAN `5` TRADEZ 2 SINGLE USR IN SINGLE ROUND. DIS AR TEH IMPOSD BY STEAM LIMITS, AS WELL AS R OWN LOAD-BALANCIN.

DIS MODULE IZ SUPPOSD 2 BE TRANZPARENT. MATCHIN WILL START IN APPROXIMATELY `1` HOUR SINCE ASF START, AN WILL REPEAT ITSELF EACH `6` HOURS (IF NEEDD). `MatchActively` FEACHUR IZ AIMD 2 BE USD AS LONG-RUN, PERIODICAL MEASURE 2 ENSURE DAT WERE ACTIVELY HEADIN TOWARDZ SETS COMPLESHUN, BUT WITHOUT SHORT-TURM TIEM AN RESOURCEZ PRESURE DAT WUD HAPPEN IF DIS WUZ OFFERD AS COMMAND. TEH TARGET USERS OV DIS MODULE R PRIMARY ACCOUNTS AN "STASH" ALT ACCOUNTS, ALTHOUGH IT CAN BE USD BY ANY BOT DAT IZ NOT SET 2 `MatchEverything`.

ASF DOEZ ITZ BEST 2 MINIMIZE TEH AMOUNT OV REQUESTS AN PRESURE GENERATD BY USIN DIS OPSHUN, WHILE AT TEH SAME TIEM MAXIMIZIN EFFICIENCY OV MATCHIN 2 TEH UPPR LIMIT. TEH EGSAKT ALGORITHM OV CHOOSIN TEH BOTS 2 MATCH AN OTHERWIZE ORGANIZE TEH WHOLE PROCES, IZ ASFS IMPLEMENTASHUN DETAIL AN CAN CHANGE IN REGARDZ 2 FEEDBACK, SITUASHUN AN POSIBLE FUCHUR IDEAS.

TEH CURRENT VERSHUN OV TEH ALGORITHM MAKEZ ASF PRIORITIZE `Any` BOTS FURST, ESPECIALLY DOSE WIF BETTR DIVERSITY OV GAMEZ DAT THEIR ITEMS R FRUM. WHEN RUNNIN OUT OV `Any` BOTS, ASF WILL MOOV ON 2 TEH FAIR ONEZ UPON SAME DIVERSITY RULE, WIF DOSE OWNIN EXCESIV NUMBR OV ITEMS FURTHR DEPRIORITIZD DUE 2 HIGHR CHANCE OV POSIBLE INVENTORY-RELATD PROBLEMS COMPARD 2 OTHR BOTS. REGARDLES OV DAT, ASF WILL TRY 2 MATCH EVRY AVAILABLE BOT AT LEAST ONCE, 2 ENSURE DAT WERE NOT MISIN ON POSIBLE SET PROGRES.

`MatchActively` TAKEZ INTO AKOWNT BOTS DAT U BLACKLISTD FRUM TRADIN THRU `tbadd` **[COMMAND](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-lol-US)** AN WILL NOT ATTEMPT 2 ACTIVELY MATCH THEM. DIS CAN BE USD 4 TELLIN ASF WHICH BOTS IT SHUD NEVR MATCH, EVEN IF THEYD HAS POTENTIAL DUPEZ 4 US 2 USE.

---

### Why do I need a `LicenseID` to use the plugin? Wasn't `MatchActively` free before?

ASF is, and remains, free and open-source, as it was established at the start of the project back in October 2015. Our program is also entirely non-commercial, we do not earn anything from contributions to it, building or publishing. Over those past 7+ years ASF has received tremendous amount of development, and it's still being improved and enhanced with every monthly stable release mostly by a single person, **[JustArchi](https://github.com/JustArchi)** - with no strings attached. The only funding we receive is from non-obligatory donations that come from our users.

For a very long time, until October 2022, `MatchActively` feature was part of ASF core and available for everyone to use. In October 2022, Valve, the company behind Steam, has put very severe rate limits that rendered previous functionality entirely broken, with no solution available. The feature therefore has been removed from ASF core in version 5.4.1.0.

`MatchActively` was resurrected as part of official `ItemsMatcher` plugin that further enhances ASF with active cards matching functionality. Resurrecting `MatchActively` feature required from us extraordinary amount of work to create ASF backend, entirely new service hosted on a server, with more than a thousand of proxies attached for resolving inventories, all exclusively to allow ASF clients to make use of `MatchActively` like before. Due to the amount of work involved, as well as resources that are not free and require to be paid on monthly basis by us (domain, server, proxies), we've decided to offer this plugin to our sponsors, that is, people that already support ASF project on monthly basis. Our goal isn't to profit from it, but rather, cover the **monthly costs** that are exclusively linked with offering this functionality - that's why we offer it basically for nothing, but we do have to charge a little for it as we can't pay hundreds of dollars from our own pockets just to make it available for you. We hope that you understand.

---

### How can I get an access?

`ItemsMatcher` is offered as part of $5+ sponsor tier on **[JustArchi's GitHub](https://github.com/sponsors/JustArchi)**. Simply become a sponsor of $5 tier (or higher), then click **[here](https://asf-backend.justarchi.net/user/status)** to obtain your `LicenseID`. You'll need to sign in with GitHub for confirming your identity.

The license allows you to send limited amount of requests to the server. $5 tier allows you to use `MatchActively` for one account, which should be suitable for majority of people. $10 tier allows you to use it on three accounts. If you require more resources, **[let us know](mailto:ASF@JustArchi.net)**.

`LicenseID` is made out of 32 hexadecimal characters, such as `f6a0529813f74d119982eb4fe43a9a24`. Simply put it in `LicenseID` property of ASF global config.