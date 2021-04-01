# COMMANDZ

ASF SUPPORTS VARIETY OV COMMANDZ, WHICH CAN BE USD 2 CONTROL BEHAVIOUR OV TEH PROCES AN BOT INSTANCEZ.

BELOW COMMANDZ CAN BE SENT 2 TEH BOT THRU VARIOUS DIFFERENT WAYS:

- THRU INTERACTIV ASF CONSOLE
- THRU STEAM PRIVATE/GROUP CHAT
- Through our **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** interface

KEEP IN MIND DAT ASF INTERACSHUN REQUIREZ FRUM U 2 BE ELIGIBLE 4 DA COMMAND ACCORDIN 2 ASF PERMISHUNS. CHECK OUT `STEAMUSERPERMISHUNS` AN `STEAMOWNERID` CONFIG PROPERTIEZ 4 MOAR DETAILS.

Commands executed through Steam chat are affected by `CommandPrefix` **[global configuration property](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#commandprefix)**, which is `!` by default. DIS MEANZ DAT 4 EXECUTIN E.G. `STATUS` COMMAND, U SHUD AKSHULLY RITE `!STATUS` (OR CUSTOM `COMMANDPREFIX` OV UR CHOICE DAT U SET INSTEAD). `COMMANDPREFIX` IZ NOT MANDATORY WHEN USIN CONSOLE OR IPC AN CAN BE OMITTD.

* * *

### INTERACTIV CONSOLE

Starting with V4.0.0.9, ASF has support for interactive console that can be enabled by setting up [**`SteamOwnerID`**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#steamownerid) property. AFTERWARDZ, SIMPLY PRES `C` BUTN IN ORDR 2 ENABLE COMMAND MODE, TYPE UR COMMAND AN CONFIRM WIF ENTR.

![Screenshot](https://i.imgur.com/bH5Gtjq.png)

Interactive console is not available in [**`Headless`**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#headless) mode.

* * *

### STEAM CHAT

U CAN EXECUTE COMMAND 2 GIVEN ASF BOT ALSO THRU STEAM CHAT. OBVIOUSLY U CANT TALK 2 YOURSELF DIRECTLY, THEREFORE ULL NED AT LEAST WAN ANOTHR BOT AKOWNT IF U WANTS 2 EXECUTE COMMANDZ TARGETTIN UR MAIN.

![Screenshot](https://i.imgur.com/IvFRJ5S.png)

IN SIMILAR WAI U CAN ALSO USE GROUP CHAT OV GIVEN STEAM GROUP. KEEP IN MIND DAT DIS OPSHUN REQUIREZ PROPERLY SET `STEAMMASTERCLANID` PROPERTY, IN WHICH CASE BOT WILL LISTEN 4 COMMANDZ ALSO ON GROUPS CHAT (AN JOIN IT IF NEEDD). DIS CAN ALSO BE USD 4 "TALKIN 2 YOURSELF" SINCE IT DOESNT REQUIRE DEDICATD BOT AKOWNT, AS OPPOSD 2 PRIVATE CHAT. U CAN SIMPLY SET `STEAMMASTERCLANID` PROPERTY 2 UR NEWLY-CREATD GROUP, DEN GIV YOURSELF ACCES EITHR THRU `STEAMOWNERID` OR `STEAMUSERPERMISHUNS` OV UR OWN BOT. DIS WAI ASF BOT (U) WILL JOIN GROUP AN CHAT OV UR SELECTD GROUP, AN LISTEN 2 COMMANDZ FRUM UR OWN AKOWNT. U CAN JOIN TEH SAME GROUP CHATROOM IN ORDR 2 ISSUE COMMANDZ 2 YOURSELF (AS ULL BE SENDIN COMMAND 2 CHATROOM, AN ASF INSTANCE SITTIN ON TEH SAME CHATROOM WILL RECEIV THEM, EVEN IF IT SHOWS ONLY AS UR AKOWNT BEAN THAR).

PLZ NOWT DAT SENDIN COMMAND 2 TEH GROUP CHAT ACTS LIEK RELAY. IF URE SAYIN `REDEEM X` 2 3 OV UR BOTS SITTIN TOGETHR WIF U ON TEH GROUP CHAT, ITLL RESULT IN DA SAME AS UD SAY `REDEEM X` 2 EVRY SINGLE WAN OV THEM PRIVATELY. IN MOST CASEZ **DIS AR TEH NOT WUT U WANTS**, AN INSTEAD U SHUD USE `GIVEN BOT` COMMAND DAT IZ BEAN SENT 2 **A SINGLE BOT IN PRIVATE WINDOW**. ASF SUPPORTS GROUP CHAT, AS IN LOTZ DA CASEZ IT CAN BE USEFUL SOURCE 4 COMMUNICASHUN WIF UR ONLY BOT, BUT U SHUD ALMOST NEVR EXECUTE ANY COMMAND ON TEH GROUP CHAT IF THAR R 2 OR MOAR ASF BOTS SITTIN THAR, UNLES U FULLY UNDERSTAND ASF BEHAVIOUR WRITTEN HER AN U IN FACT WANTS 2 RELAY TEH SAME COMMAND 2 EVRY SINGLE BOT DAT IZ LISTENIN 2 U.

*AN EVEN IN DIS CASE U SHUD USE PRIVATE CHAT WIF `[BOTS]` SYNTAX INSTEAD.*

* * *

### IPC

The most advanced and flexible way of executing commands, perfect for user interaction (ASF-ui) as well as third-party tools or scripting (ASF API), requires ASF to be run in `IPC` mode, and a client executing command through **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** interface.

![Screenshot](https://raw.githubusercontent.com/JustArchiNET/ASF-ui/main/.github/previews/commands.png)

* * *

## COMMANDZ

| COMMAND                                                              | ACCES           | DESCRIPSHUN                                                                                                                                                                                                                                                                                                                         |
| -------------------------------------------------------------------- | --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `2fa [Bots]`                                                         | `Master`        | Generates temporary **[2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** token for given bot instances.                                                                                                                                                                                         |
| `2fano [Bots]`                                                       | `Master`        | Denies all pending **[2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** confirmations for given bot instances.                                                                                                                                                                                  |
| `2faok [Bots]`                                                       | `Master`        | Accepts all pending **[2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** confirmations for given bot instances.                                                                                                                                                                                 |
| `addlicense [Bots] <Licenses>`                                 | `Operator`      | Activates given `licenses`, explained **[below](#addlicense-licenses)**, on given bot instances (free games only).                                                                                                                                                                                                                  |
| `balance [Bots]`                                                     | `Master`        | SHOWS WALLET BALANCE OV GIVEN BOT INSTANCEZ.                                                                                                                                                                                                                                                                                        |
| `bgr [Bots]`                                                         | `Master`        | Prints information about **[BGR](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer)** queue of given bot instances.                                                                                                                                                                                     |
| `bl [Bots]`                                                          | `Master`        | LISTS BLACKLISTD USERS FRUM TRADIN MODULE OV GIVEN BOT INSTANCEZ.                                                                                                                                                                                                                                                                   |
| `bladd [Bots] <SteamIDs64>`                                    | `Master`        | BLACKLISTS GIVEN `STEAMIDZ` FRUM TRADIN MODULE OV GIVEN BOT INSTANCEZ.                                                                                                                                                                                                                                                              |
| `blrm [Bots] <SteamIDs64>`                                     | `Master`        | REMOVEZ BLACKLIST OV GIVEN `STEAMIDZ` FRUM TRADIN MODULE OV GIVEN BOT INSTANCEZ.                                                                                                                                                                                                                                                    |
| `encrypt <encryptionMethod> <stringToEncrypt>`           | `Owner`         | Encrypts the string using provided cryptographic method - further explained **[below](#encrypt-command)**.                                                                                                                                                                                                                          |
| `exit`                                                               | `Owner`         | Stops whole ASF process.                                                                                                                                                                                                                                                                                                            |
| `farm [Bots]`                                                        | `Master`        | Restarts cards farming module for given bot instances.                                                                                                                                                                                                                                                                              |
| `hash <hashingMethod> <stringToHash>`                    | `Owner`         | Generated a hash of the string using provided cryptographic method - further explained **[below](#hash-command)**.                                                                                                                                                                                                                  |
| `help`                                                               | `FamilySharing` | Shows help (link to this page).                                                                                                                                                                                                                                                                                                     |
| `input [Bots] <Type> <Value>`                            | `Master`        | Sets given input type to given value for given bot instances, works only in `Headless` mode - further explained **[below](#input-command)**.                                                                                                                                                                                        |
| `ib [Bots]`                                                          | `Master`        | Lists apps blacklisted from automatic idling of given bot instances.                                                                                                                                                                                                                                                                |
| `ibadd [Bots] <AppIDs>`                                        | `Master`        | Adds given `appIDs` to apps blacklisted from automatic idling of given bot instances.                                                                                                                                                                                                                                               |
| `ibrm [Bots] <AppIDs>`                                         | `Master`        | Removes given `appIDs` from apps blacklisted from automatic idling of given bot instances.                                                                                                                                                                                                                                          |
| `iq [Bots]`                                                          | `Master`        | Lists priority idling queue of given bot instances.                                                                                                                                                                                                                                                                                 |
| `iqadd [Bots] <AppIDs>`                                        | `Master`        | Adds given `appIDs` to priority idling queue of given bot instances.                                                                                                                                                                                                                                                                |
| `iqrm [Bots] <AppIDs>`                                         | `Master`        | Removes given `appIDs` from priority idling queue of given bot instances.                                                                                                                                                                                                                                                           |
| `level [Bots]`                                                       | `Master`        | Shows Steam account level of given bot instances.                                                                                                                                                                                                                                                                                   |
| `loot [Bots]`                                                        | `Master`        | Sends all `LootableTypes` Steam community items of given bot instances to `Master` user defined in their `SteamUserPermissions` (with lowest steamID if more than one).                                                                                                                                                             |
| `loot@ [Bots] <AppIDs>`                                        | `Master`        | Sends all `LootableTypes` Steam community items matching given `AppIDs` of given bot instances to `Master` user defined in their `SteamUserPermissions` (with lowest steamID if more than one). This is the opposite of `loot%`.                                                                                                    |
| `loot% [Bots] <AppIDs>`                                        | `Master`        | Sends all `LootableTypes` Steam community items apart from given `AppIDs` of given bot instances to `Master` user defined in their `SteamUserPermissions` (with lowest steamID if more than one). This is the opposite of `loot@`.                                                                                                  |
| `loot^ [Bots] <AppID> <ContextID>`                       | `Master`        | Sends all Steam items from given `AppID` in `ContextID` of given bot instances to `Master` user defined in their `SteamUserPermissions` (with lowest steamID if more than one).                                                                                                                                                     |
| `mab [Bots]`                                                         | `Master`        | Lists apps blacklisted from automatic trading in **[`MatchActively`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading#matchactively)**.                                                                                                                                                                                  |
| `mabadd [Bots] <AppIDs>`                                       | `Master`        | Adds given `appIDs` to apps blacklisted from automatic trading in **[`MatchActively`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading#matchactively)**.                                                                                                                                                                 |
| `mabrm [Bots] <AppIDs>`                                        | `Master`        | Removes given `appIDs` from apps blacklisted from automatic trading in **[`MatchActively`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading#matchactively)**.                                                                                                                                                            |
| `nickname [Bots] <Nickname>`                                   | `Master`        | Changes Steam nickname of given bot instances to given `nickname`.                                                                                                                                                                                                                                                                  |
| `owns [Bots] <Games>`                                          | `Operator`      | Checks if given bot instances already own given `games`, explained **[below](#owns-games)**.                                                                                                                                                                                                                                        |
| `password [Bots]`                                                    | `Master`        | Prints encrypted password of given bot instances (in use with `PasswordFormat`).                                                                                                                                                                                                                                                    |
| `pause [Bots]`                                                       | `Operator`      | Permanently pauses automatic cards farming module of given bot instances. ASF will not attempt to farm current account in this session, unless you manually `resume` it, or restart the process.                                                                                                                                    |
| `pause~ [Bots]`                                                      | `FamilySharing` | Temporarily pauses automatic cards farming module of given bot instances. Farming will be automatically resumed on the next playing event, or bot disconnect. You can `resume` farming to unpause it.                                                                                                                               |
| `pause& [Bots] <Seconds>`                                  | `Operator`      | Temporarily pauses automatic cards farming module of given bot instances for given amount of `seconds`. After delay, cards farming module is automatically resumed.                                                                                                                                                                 |
| `play [Bots] <AppIDs,GameName>`                                | `Master`        | Switches to manual farming - launches given `AppIDs` on given bot instances, optionally also with custom `GameName`. In order for this feature to work properly, your Steam account **must** own a valid license to all the `AppIDs` that you specify here, this includes F2P games as well. Use `reset` or `resume` for returning. |
| `privacy [Bots] <Settings>`                                    | `Master`        | Changes **[Steam privacy settings](https://steamcommunity.com/my/edit/settings)** of given bot instances, to appropriately selected options explained **[below](#privacy-settings)**.                                                                                                                                               |
| `redeem [Bots] <Keys>`                                         | `Operator`      | Redeems given cd-keys or wallet codes on given bot instances.                                                                                                                                                                                                                                                                       |
| `redeem^ [Bots] <Modes> <Keys>`                          | `Operator`      | Redeems given cd-keys or wallet codes on given bot instances, using given `modes` explained **[below](#redeem-modes)**.                                                                                                                                                                                                             |
| `reset [Bots]`                                                       | `Master`        | Resets the playing status back to normal, used during manual farming with `play` command.                                                                                                                                                                                                                                           |
| `restart`                                                            | `Owner`         | Restarts ASF process.                                                                                                                                                                                                                                                                                                               |
| `resume [Bots]`                                                      | `FamilySharing` | Resumes automatic farming of given bot instances. Also see `pause`, `play`.                                                                                                                                                                                                                                                         |
| `start [Bots]`                                                       | `Master`        | Starts given bot instances.                                                                                                                                                                                                                                                                                                         |
| `stats`                                                              | `Owner`         | Prints process statistics, such as managed memory usage.                                                                                                                                                                                                                                                                            |
| `status [Bots]`                                                      | `FamilySharing` | Prints status of given bot instances.                                                                                                                                                                                                                                                                                               |
| `stop [Bots]`                                                        | `Master`        | Stops given bot instances.                                                                                                                                                                                                                                                                                                          |
| `transfer [Bots] <TargetBot>`                                  | `Master`        | Sends all `TransferableTypes` Steam community items from given bot instances to target bot instance.                                                                                                                                                                                                                                |
| `transfer@ [Bots] <AppIDs> <TargetBot>`                  | `Master`        | Sends all `TransferableTypes` Steam community items matching given `AppIDs` from given bot instances to target bot instance. This is the opposite of `transfer%`.                                                                                                                                                                   |
| `transfer% [Bots] <AppIDs> <TargetBot>`                  | `Master`        | Sends all `TransferableTypes` Steam community items apart from given `AppIDs` from given bot instances to target bot instance. This is the opposite of `transfer@`.                                                                                                                                                                 |
| `transfer^ [Bots] <AppID> <ContextID> <TargetBot>` | `Master`        | Sends all Steam items from given `AppID` in `ContextID` of given bot instances to target bot instance.                                                                                                                                                                                                                              |
| `unpack [Bots]`                                                      | `Master`        | Unpacks all booster packs stored in the inventory of given bot instances.                                                                                                                                                                                                                                                           |
| `update`                                                             | `Owner`         | Checks GitHub for ASF updates (this is done automatically every `UpdatePeriod`).                                                                                                                                                                                                                                                    |
| `version`                                                            | `FamilySharing` | Prints version of ASF.                                                                                                                                                                                                                                                                                                              |

* * *

### NOTEZ

ALL COMMANDZ R CASE-INSENSITIV, BUT THEIR ARGUMENTS (SUCH AS BOT NAMEZ) R USUALLY CASE-SENSITIV.

`[BOTS]` ARGUMENT IZ OPSHUNAL IN ALL COMMANDZ. WHEN SPECIFID, COMMAND IZ EXECUTD ON GIVEN BOTS. WHEN OMITTD, COMMAND IZ EXECUTD ON CURRENT BOT DAT RECEIVEZ TEH COMMAND. IN OTHR WERDZ, `STATUS A` SENT 2 BOT `B` IZ TEH SAME AS SENDIN `STATUS` 2 BOT `A`, BOT `B` IN DIS CASE ACTS ONLY AS PROXY. DIS CAN ALSO BE USD 4 SENDIN COMMANDZ 2 BOTS DAT R UNAVAILABLE OTHERWIZE, 4 EXAMPLE STARTIN STOPPD BOTS, OR EXECUTIN ACSHUNS ON UR MAIN AKOWNT (DAT URE USIN 4 EXECUTIN TEH COMMANDZ).

**ACCES** OV TEH COMMAND DEFINEZ **MINIMUM** `EPERMISHUN` OV `STEAMUSERPERMISHUNS` DAT IZ REQUIRD 2 USE TEH COMMAND, WIF AN EXCEPSHUN OV `OWNR` WHICH IZ `STEAMOWNERID` DEFIND IN GLOBAL CONFIGURASHUN FILE (AN HIGHEST PERMISHUN AVAILABLE).

Plural arguments, such as `[Bots]`, `<Keys>` or `<AppIDs>` mean that command supports multiple arguments of given type, separated by a comma. 4 EXAMPLE, `STATUS [BOTS]` CAN BE USD AS `STATUS MYBOT,MYOTHERBOT,PRIMARY`. DIS WILL CAUSE GIVEN COMMAND 2 BE EXECUTD ON **ALL TARGET BOTS** IN DA SAME WAI AS UD SEND `STATUS` 2 EACH BOT IN SEPARATE CHAT WINDOW. PLZ NOWT DAT THAR IZ NO SPACE AFTR `,`.

ASF USEZ ALL WHITESPACE CHARACTERS AS POSIBLE DELIMITERS 4 COMMAND, SUCH AS SPACE AN NEWLINE CHARACTERS. DIS MEANZ DAT U DOAN HAS 2 USE SPACE 4 DELIMITIN UR ARGUMENTS, U CAN AS WELL USE ANY OTHR WHITESPACE CHARACTR (SUCH AS TAB OR NEW LINE).

ASF WILL "JOIN" EXTRA OUT-OV-RANGE ARGUMENTS 2 PLURAL TYPE OV TEH LAST IN-RANGE ARGUMENT. This means that `redeem bot key1 key2 key3` for `redeem [Bots] <Keys>` will work exactly the same as `redeem bot key1,key2,key3`. TOGETHR WIF ACCEPTIN NEWLINE AS COMMAND DELIMITR, DIS MAKEZ IT POSIBLE 4 U 2 RITE `REDEEM BOT` DEN PASTE LIST OV KEYS SEPARATD BY ANY ACCEPTABLE DELIMITR CHARACTR (SUCH AS NEWLINE), OR STANDARD `,` ASF DELIMITR. KEEP IN MIND DAT DIS TRICK CAN BE USD ONLY 4 COMMAND VARIANT DAT USEZ TEH MOST AMOUNT OV ARGUMENTS (SO SPECIFYIN `[BOTS]` IZ MANDATORY IN DIS CASE).

AS UVE READ ABOOV, SPACE CHARACTR IZ BEAN USD AS DELIMITR 4 COMMAND, THEREFORE IT CANT BE USD IN ARGUMENTS. HOWEVR, ALSO AS STATD ABOOV, ASF CAN JOIN OUT-OV-RANGE ARGUMENTS, WHICH MEANZ DAT URE AKSHULLY ABLE 2 USE SPACE CHARACTR IN ARGUMENT DAT IZ DEFIND AS LAST WAN 4 GIVEN COMMAND. 4 EXAMPLE, `NICKNAME BOB GREAT BOB` WILL PROPERLY SET NICKNAME OV `BOB` BOT 2 "GREAT BOB". IN DA SIMILAR WAI U CAN CHECK NAMEZ CONTAININ SPACEZ IN `OWNS` COMMAND.

* * *

SUM COMMANDZ R ALSO AVAILABLE WIF THEIR ALIASEZ, 2 SAVE U ON TYPIN:

| COMMAND      | ALIAS |
| ------------ | ----- |
| `owns ASF`   | `oa`  |
| `status ASF` | `sa`  |
| `redeem`     | `r`   |
| `redeem^`    | `r^`  |

* * *

### `[BOTS]` ARGUMENT

`[BOTS]` ARGUMENT IZ SPESHUL VARIANT OV PLURAL ARGUMENT, AS IN ADDISHUN 2 ACCEPTIN MULTIPLE VALUEZ IT ALSO OFFERS EXTRA FUNCSHUNALITY.

FURST AN FOREMOST, THAR IZ SPESHUL `ASF` KEYWORD WHICH ACTS AS "ALL BOTS IN DA PROCES", SO `STATUS ASF` COMMAND IZ EQUAL 2 `STATUS ALL,UR,BOTS,LISTD,HER`. DIS CAN ALSO BE USD 2 EASILY IDENTIFY TEH BOTS DAT U HAS ACCES 2, AS `ASF` KEYWORD, DESPITE OV TARGETIN ALL BOTS, WILL RESULT IN RESPONSE ONLY FRUM DOSE BOTS DAT U CAN AKSHULLY SEND TEH COMMAND 2.

`[BOTS]` ARGUMENT SUPPORTS SPESHUL "RANGE" SYNTAX, WHICH ALLOWS U 2 CHOOSE RANGE OV BOTS MOAR EASILY. TEH GENERAL SYNTAX 4 `[BOTS]` IN DIS CASE IZ `FIRSTBOT..LASTBOT`. 4 EXAMPLE, IF U HAS BOTS NAMD `A, B, C, D, E, F`, U CAN EXECUTE `STATUS B..E`, WHICH IZ EQUAL 2 `STATUS B,C,D,E` IN DIS CASE. WHEN USIN DIS SYNTAX, ASF WILL USE ALFABETICAL SORTIN IN ORDR 2 DETERMINE WHICH BOTS R IN UR SPECIFID RANGE. BOTH `FIRSTBOT` AN `LASTBOT` MUST BE VALID BOT NAMEZ RECOGNIZD BY ASF, OTHERWIZE RANGE SYNTAX IZ ENTIRELY SKIPPD.

In addition to range syntax above, `[Bots]` argument also supports **[regex](https://en.wikipedia.org/wiki/Regular_expression)** matching. You can activate regex pattern by using `r!<pattern>` as a bot name, where `r!` is ASF activator for regex matching, and `<pattern>` is your regex pattern. AN EXAMPLE OV REGEX-BASD BOT COMMAND WUD BE `STATUS R!\D{3}` WHICH WILL SEND `STATUS` COMMAND 2 BOTS DAT HAS NAYM MADE OUT OV 3 DIGITS (E.G. `123` AN `981`). Feel free to take a look at the **[docs](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference)** for further explanation and more examples of available regex patterns.

* * *

## `PRIVACY` SETTINGS

`<Settings>` argument accepts **up to 7** different options, separated as usual with standard comma ASF delimiter. Those are, in order:

| ARGUMENT | NAYM           | CHILD OV   |
| -------- | -------------- | ---------- |
| 1        | Profile        |            |
| 2        | OwnedGames     | Profile    |
| 3        | Playtime       | OwnedGames |
| 4        | FriendsList    | Profile    |
| 5        | Inventory      | Profile    |
| 6        | InventoryGifts | Inventory  |
| 7        | Comments       | Profile    |

For description of above fields, please visit **[Steam privacy settings](https://steamcommunity.com/my/edit/settings)**.

WHILE VALID VALUEZ 4 ALL OV THEM R:

| VALUE | NAYM          |
| ----- | ------------- |
| 1     | `Private`     |
| 2     | `FriendsOnly` |
| 3     | `Public`      |

U CAN USE EITHR CASE-INSENSITIV NAYM, OR NUMERIC VALUE. ARGUMENTS DAT WUZ OMITTD WILL DEFAULT 2 BEAN SET 2 `PRIVATE`. IZ IMPORTANT 2 NOWT RELASHUN TWEEN CHILD AN PARENT OV ARGUMENTS SPECIFID ABOOV, AS CHILD CAN NEVR HAS MOAR OPEN PERMISHUN THAN ITZ PARENT. 4 EXAMPLE, U **CANT** HAS `PUBLIC` GAMEZ OWND WHILE HAVIN `PRIVATE` PROFILE.

### EXAMPLE

IF U WANTS 2 SET **ALL** PRIVACY SETTINGS OV UR BOT NAMD `MAIN` 2 `PRIVATE`, U CAN USE EITHR OV BELOW:

```text
PRIVACY MAIN 1
PRIVACY MAIN PRIVATE
```

DIS AR TEH CUZ ASF WILL AUTOMATICALLY ASSUME ALL OTHR SETTINGS 2 BE `PRIVATE`, SO THAR IZ NO NED 2 INPUT THEM. ON TEH OTHR HAND, IF UD LIEK 2 SET ALL PRIVACY SETTINGS 2 `PUBLIC`, DEN U SHUD USE ANY OV BELOW:

```text
PRIVACY MAIN 3,3,3,3,3,3,3
PRIVACY MAIN PUBLIC,PUBLIC,PUBLIC,PUBLIC,PUBLIC,PUBLIC,PUBLIC
```

DIS WAI U CAN ALSO SET INDEPENDENT OPSHUNS HOWEVR U LIEK:

```text
PRIVACY MAIN PUBLIC,FRIENDSONLY,PRIVATE,PUBLIC,PUBLIC,PRIVATE,PUBLIC
```

TEH ABOOV WILL SET PROFILE 2 PUBLIC, OWND GAMEZ 2 FRENZ ONLY, PLAYTIME 2 PRIVATE, FRENZ LIST 2 PUBLIC, INVENTORY 2 PUBLIC, INVENTORY GIFTS 2 PRIVATE AN PROFILE COMMENTS 2 PUBLIC. U CAN ACHIEVE TEH SAME WIF NUMERIC VALUEZ IF U WANTS 2.

REMEMBR DAT CHILD CAN NEVR HAS MOAR OPEN PERMISHUN THAN ITZ PARENT. REFR 2 ARGUMENTS RELASHUNSHIP 4 AVAILABLE OPSHUNS.

* * *

## `ADDLICENSE` LICENSEZ

`ADDLICENSE` COMMAND SUPPORTS 2 DIFFERENT LICENSE TYPEZ, DOSE R:

| TYPE  | ALIAS | EXAMPLE      | DESCRIPSHUN                                                           |
| ----- | ----- | ------------ | --------------------------------------------------------------------- |
| `app` | `a`   | `app/292030` | GAME DETERMIND BY ITZ UNIQUE `APPID`.                                 |
| `sub` | `s`   | `sub/47807`  | PACKAGE CONTAININ WAN OR MOAR GAMEZ, DETERMIND BY ITZ UNIQUE `SUBID`. |

TEH DISTINCSHUN IZ IMPORTANT, AS ASF WILL USE STEAM NETWORK ACTIVASHUN 4 APPS, AN STEAM STORE ACTIVASHUN 4 PACKAGEZ. DOSE 2 R NOT COMPATIBLE WIF EACH OTHR, TYPICALLY ULL USE APPS 4 FREE WEEKENDZ AN PERMANENTLY F2P GAMEZ, AN PACKAGEZ OTHERWIZE.

WE RECOMMEND 2 EXPLICITLY DEFINE TEH TYPE OV EACH ENTRY IN ORDR 2 AVOID AMBIGUOUS RESULTS, BUT 4 DA BAKWARDZ COMPATIBILITY, IF U SUPPLY INVALID TYPE OR OMIT IT ENTIRELY, ASF WILL ASSUME DAT U ASK 4 `SUB` IN DIS CASE. U CAN ALSO QUERY WAN OR MOAR OV TEH LICENSEZ AT TEH SAME TIEM, USIN STANDARD ASF `,` DELIMITR.

COMPLETE COMMAND EXAMPLE:

```text
ADDLICENSE ASF APP/292030,SUB/47807
```

* * *

## `owns` games

`owns` command supports several different game types for `<games>` argument that can be used, those are:

| TYPE    | ALIAS | EXAMPLE          | DESCRIPSHUN                                                                                                                                                                                                                                                             |
| ------- | ----- | ---------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `app`   | `a`   | `app/292030`     | GAME DETERMIND BY ITZ UNIQUE `APPID`.                                                                                                                                                                                                                                   |
| `sub`   | `s`   | `sub/47807`      | PACKAGE CONTAININ WAN OR MOAR GAMEZ, DETERMIND BY ITZ UNIQUE `SUBID`.                                                                                                                                                                                                   |
| `regex` | `r`   | `regex/^\d{4}:` | **[Regex](https://en.wikipedia.org/wiki/Regular_expression)** applying to the game's name, case-sensitive. See the **[docs](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference)** for complete syntax and more examples. |
| `name`  | `n`   | `name/Witcher`   | PART OV TEH GAMEZ NAYM, CASE-INSENSITIV.                                                                                                                                                                                                                                |

WE RECOMMEND 2 EXPLICITLY DEFINE TEH TYPE OV EACH ENTRY IN ORDR 2 AVOID AMBIGUOUS RESULTS, BUT 4 DA BAKWARDZ COMPATIBILITY, IF U SUPPLY INVALID TYPE OR OMIT IT ENTIRELY, ASF WILL ASSUME DAT U ASK 4 `APP` IF UR INPUT IZ NUMBR, AN `NAYM` OTHERWIZE. U CAN ALSO QUERY WAN OR MOAR OV TEH GAMEZ AT TEH SAME TIEM, USIN STANDARD ASF `,` DELIMITR.

COMPLETE COMMAND EXAMPLE:

```text
OWNS ASF APP/292030,NAYM/WITCHR
```

* * *

## `REDEEM^` MODEZ

`REDEEM^` COMMAND ALLOWS U 2 FINE-TUNE MODEZ DAT WILL BE USD 4 WAN SINGLE REDEEM SCENARIO. This works as temporary override of `RedeemingPreferences` **[bot config property](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#bot-config)**.

`<Modes>` argument accepts multiple mode values, separated as usual by a comma. AVAILABLE MODE VALUEZ R SPECIFID BELOW:

| VALUE | NAYM                  | DESCRIPSHUN                                                                     |
| ----- | --------------------- | ------------------------------------------------------------------------------- |
| FAWK  | ForceAssumeWalletKey  | Forces `AssumeWalletKeyOnBadActivationCode` redeeming preference to be enabled  |
| FD    | ForceDistributing     | Forces `Distributing` redeeming preference to be enabled                        |
| FF    | ForceForwarding       | Forces `Forwarding` redeeming preference to be enabled                          |
| FKMG  | ForceKeepMissingGames | Forces `KeepMissingGames` redeeming preference to be enabled                    |
| SAWK  | SkipAssumeWalletKey   | Forces `AssumeWalletKeyOnBadActivationCode` redeeming preference to be disabled |
| SD    | SkipDistributing      | Forces `Distributing` redeeming preference to be disabled                       |
| SF    | SkipForwarding        | Forces `Forwarding` redeeming preference to be disabled                         |
| SI    | SkipInitial           | Skips key redemption on initial bot                                             |
| SKMG  | SkipKeepMissingGames  | Forces `KeepMissingGames` redeeming preference to be disabled                   |
| V     | Validate              | Validates keys for proper format and automatically skips invalid ones           |

4 EXAMPLE, WED LIEK 2 REDEEM 3 KEYS ON ANY OV R BOTS DAT DOAN OWN GAMEZ YET, BUT NOT R `PRIMARY` BOT. 4 ACHIEVIN DAT WE CAN USE:

`REDEEM^ PRIMARY FF,SI KEY1,KEY2,KEY3`

IZ IMPORTANT 2 NOWT DAT ADVANCD REDEEM OVERRIDEZ ONLY DOSE `REDEEMINGPREFERENCEZ` DAT U **SPECIFY IN DA COMMAND**. 4 EXAMPLE, IF UVE ENABLD `DISTRIBUTIN` IN UR `REDEEMINGPREFERENCEZ` DEN THAR WILL BE NO DIFFERENCE WHETHR U USE `FD` MODE OR NOT, CUZ DISTRIBUTIN WILL BE ALREADY ACTIV REGARDLES, DUE 2 `REDEEMINGPREFERENCEZ` DAT U USE. DIS AR TEH Y EACH FORCIBLY ENABLD OVERRIDE ALSO HAS FORCIBLY DISABLD WAN, U CAN DECIDE YOURSELF IF U PREFR 2 OVERRIDE DISABLD WIF ENABLD, OR VICE VERSA.

* * *

## `ENCRYPT` COMMAND

ENCRYPT COMMAND ALLOWS U 2 ENCRYPT ARBITRARY STRINGS USIN ASFS ENCRYPSHUN METHODZ. `<encryptionMethod>` must be one of the encryption methods specified and explained in **[security](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security)** section. DIS COMMAND IZ USEFUL IN CASE UD WANTS 2 GENERATE ENCRYPTD DETAILS IN ADVANCE, E.G. IN ORDR 2 AVOID PUTTIN UR `PLAINTEXT` PASWORD IN DA CONFIG FURST AN DEN USIN `PASWORD` COMMAND. WE RECOMMEND 2 USE DIS COMMAND THRU SECURE CHANNELS (ASF CONSOLE OR IPC INTERFACE, WHICH ALSO HAS DEDICATD API ENDPOINT 4 IT), AS OTHERWIZE SENSITIV DETAILS MITE GIT LOGGD BY VARIOUS THIRD-PARTIEZ (SUCH AS CHAT MESAGEZ BEAN LOGGD BY STEAM SERVERS).

* * *

## `HASH` COMMAND

HASH COMMAND ALLOWS U 2 GENERATD HASHEZ OV ARBITRARY STRINGS USIN ASFS HASHIN METHODZ. `<hashingMethod>` must be one of the hashing methods specified and explained in **[security](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security)** section. WE RECOMMEND 2 USE DIS COMMAND THRU SECURE CHANNELS (ASF CONSOLE OR IPC INTERFACE, WHICH ALSO HAS DEDICATD API ENDPOINT 4 IT), AS OTHERWIZE SENSITIV DETAILS MITE GIT LOGGD BY VARIOUS THIRD-PARTIEZ (SUCH AS CHAT MESAGEZ BEAN LOGGD BY STEAM SERVERS).

* * *

## `input` command

Input command can be used only in `Headless` mode, for inputting given data via **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** or Steam chat when ASF is running without support for user interaction.

General syntax is `input [Bots] <Type> <Value>`.

`<Type>` is case-insensitive and defines input type recognized by ASF. CURRENTLY ASF RECOGNIZEZ FOLLOWIN TYPEZ:

| TYPE                    | DESCRIPSHUN                                                         |
| ----------------------- | ------------------------------------------------------------------- |
| Login                   | `STEAMLOGIN` BOT CONFIG PROPERTY, IF MISIN FRUM CONFIG.             |
| PASWORD                 | `STEAMPASWORD` BOT CONFIG PROPERTY, IF MISIN FRUM CONFIG.           |
| SteamGuard              | AUTH CODE SENT ON UR E-MAIL IF URE NOT USIN 2FA.                    |
| SteamParentalCode       | `STEAMPARENTALCODE` BOT CONFIG PROPERTY, IF MISIN FRUM CONFIG.      |
| TwoFactorAuthentication | 2FA TOKEN GENERATD FRUM UR MOBILE, IF URE USIN 2FA BUT NOT ASF 2FA. |

`<Value>` is value set for given type. Currently all values are strings.

### EXAMPLE

LETS SAY DAT WE HAS BOT DAT IZ PROTECTD BY STEAMGUARD IN NON-2FA MODE. WE WANTS 2 LAUNCH DAT BOT WIF `HEADLES` SET 2 TRUE.

IN ORDR 2 DO DAT, WE NED 2 EXECUTE FOLLOWIN COMMANDZ:

`START MYSTEAMGUARDBOT` -> BOT WILL ATTEMPT 2 LOG IN, FAIL DUE 2 AUTHCODE NEEDD, DEN STOP DUE 2 RUNNIN IN `HEADLES` MODE. WE NED DIS IN ORDR 2 MAK STEAM NETWORK SEND US AUTH CODE ON R E-MAIL - IF THAR WUZ NO NED 4 DAT, WED SKIP DIS STEP ENTIRELY.

`INPUT MYSTEAMGUARDBOT STEAMGUARD ABCDE` -> WE SET `STEAMGUARD` INPUT OV `MYSTEAMGUARDBOT` BOT 2 `ABCDE`. OV COURSE, `ABCDE` IN DIS CASE IZ AUTH CODE DAT WE GOT ON R E-MAIL.

`START MYSTEAMGUARDBOT` -> WE START R (STOPPD) BOT AGAIN, DIS TIEM IT AUTOMATICALLY USEZ AUTH CODE DAT WE SET IN PREVIOUS COMMAND, PROPERLY LOGGIN IN, DEN CLEARIN IT.

IN DA SAME WAI WE CAN ACCES 2FA-PROTECTD BOTS (IF THEYRE NOT USIN ASF 2FA), AS WELL AS SETTIN OTHR REQUIRD PROPERTIEZ DURIN RUNTIME.