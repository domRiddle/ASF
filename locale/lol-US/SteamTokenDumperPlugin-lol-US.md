# STEAMTOKENDUMPERPLUGIN

`SteamTokenDumperPlugin` IZ OFFISHUL ASF **[PLUGIN](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins-lol-US)** AVAILABLE SINCE ASF V4.2.2.2, DEVELOPD BY US, WHICH ALLOWS U 2 CONTRIBUTE 2 **[STEAMDB](https://steamdb.info)** PROJECT BY SHARIN PACKAGE TOKENS, APP TOKENS AN DEPOT KEYS DAT UR STEAM AKOWNT HAS ACCES 2. TEH EXTENDD INFO ON COLLECTD DATA AN Y STEAMDB NEEDZ IT CAN BE FINDZ ON STEAMDBS **[TOKEN DUMPR](https://steamdb.info/tokendumper)** PAEG. TEH SUBMITTD DATA DOESNT INCLUDE ANY POTENTIALLY-SENSITIV INFORMASHUN, AN POSESEZ NO SECURITY/PRIVACY RISK, AS STATD IN ABOOV DESCRIPSHUN.

---

## ENABLIN TEH PLUGIN

ASF COMEZ WIF `SteamTokenDumperPlugin` BUNDLD TOGETHR WIF TEH RELEASE, BUT TEH PLUGIN ITSELF IZ DISABLD BY DEFAULT. U CAN ENABLE TEH PLUGIN BY SETTIN `SteamTokenDumperPluginEnabled` ASF GLOBAL CONFIG PROPERTY 2 `true`, IN JSON SYNTAX:

```json
{
  "SteamTokenDumperPluginEnabled": true
}
```

ON TEH LAUNCH OV TEH ASF PROGRAM, TEH PLUGIN WILL LET U KNOE WHETHR IT WUZ ENABLD SUCCESFULLY THRU STANDARD ASF LOGGIN MECHANISM. U CAN ALSO ENABLE TEH PLUGIN THRU R WEB-BASD CONFIG GENERATOR.

---

## TECHNICAL DETAILS

UPON ENABLIN, TEH PLUGIN WILL USE TEH BOTS DAT URE RUNNIN IN ASF 4 DATA GATHERIN IN FORM OV PACKAGE TOKENS, APP TOKENS AN DEPOT KEYS DAT UR BOTS HAS ACCES 2. DATA GATHERIN MODULE INCLUDEZ PASIV AN ACTIV ROUTINEZ DAT R SUPPOSD 2 MINIMIZE TEH ADDISHUNAL OVERHEAD CAUSD BY COLLECTIN DATA.

IN ORDR 2 FULFILL TEH PLANND USE CASE, IN ADDISHUN 2 DATA GATHERIN ROUTINE EXPLAIND ABOOV, SUBMISHUN ROUTINE IZ INITIALIZD AS BEAN RESPONSIBLE 4 DETERMININ WUT DATA NEEDZ 2 BE SUBMITTD 2 STEAMDB ON PERIODIC BASIS. DIS ROUTINE WILL FIRE IN UP 2 `1` HOUR SINCE UR ASF START, AN WILL REPEAT ITSELF EVRY `24` HOURS. TEH PLUGIN WILL DO ITZ BEST 2 MINIMIZE TEH AMOUNT OV DATA DAT NEEDZ 2 BE SENT, THEREFORE IZ POSIBLE DAT SUM DATA WHICH TEH PLUGIN WILL COLLECT WILL BE DETERMIND AS USELES 2 SUBMIT, AN THEREFORE SKIPPD (4 EXAMPLE APP UPDATE WHICH DOESNT CHANGE TEH ACCES TOKEN).

TEH PLUGIN USEZ PERSISTENT CACHE DATABASE SAVD IN `config/SteamTokenDumper.cache` LOCASHUN, WHICH SERVEZ SIMILAR PURPOSE 2 `config/ASF.db` 4 ASF. TEH FILE IZ USD IN ORDR 2 RECORD TEH GATHERD AN SUBMITTD DATA AN MINIMIZE TEH AMOUNT OV WERK DAT HAS 2 BE DUN ACROS DIFFERENT ASF RUNS. REMOVIN TEH FILE CAUSEZ TEH PROCES 2 BE RESTARTD FRUM SCRATCH, WHICH SHUD BE AVOIDD IF POSIBLE.

---

## DATA

ASF INCLUDEZ TEH CONTRIBUTOR `steamID` IN DA REQUEST, WHICH IZ DETERMIND AS `SteamOwnerID` DAT U SET IN ASF, OR IN CASE U DIDNT, TEH STEAM ID OV TEH BOT WHICH OWNS TEH MOST LICENSEZ. TEH ANNOUNCD CONTRIBUTOR MITE RECEIV SUM ADDISHUNAL PERKZ FRUM STEAMDB 4 CONTINUOUS HALP (E.G. DONATOR RANK ON TEH WEBSIET), BUT DAT IZ ENTIRELY UP 2 STEAMDBS DISCRESHUN.

IN ANY CASE, STEAMDB STAFF WUD LIEK 2 THANK U IN ADVANCE 4 UR HALP. TEH SUBMITTD DATA ALLOWS STEAMDB 2 OPERATE, IN PARTICULAR 2 TRACK INFO BOUT PACKAGEZ, APPS AN DEPOTS, WHICH WUD NO LONGR BE POSIBLE WITHOUT UR HALP.

---

## Advanced config

Starting with ASF V5.0.7.0, our plugin supports advanced config which might come useful for people that would like to tweak the internals to their preference.

The advanced config has the following structure located within `ASF.json`:

```json
{
  "SteamTokenDumperPlugin": {
    "Enabled": false,
    "SecretAppIDs": [],
    "SecretDepotIDs": [],
    "SecretPackageIDs": [],
    "SkipAutoGrantPackages": false
  }
}
```

ALL OPSHUNS R EXPLAIND BELOW:

### `Enabled`

`bool` TYPE WIF DEFAULT VALUE OV `false`. This property acts the same as `SteamTokenDumperPluginEnabled` root-level property explained above, and can be used instead, dedicated to people that would prefer to have entire plugin-related config in its own structure (so most likely those already using other advanced properties explained below).

---

### `SecretAppIDs`

`ImmutableHashSet<uint>` TYPE WIF DEFAULT VALUE OV BEAN EMPTY. This property specifies `appIDs` that the plugin won't resolve, and if they're already resolved, won't submit the token for. This property can be useful for people with access to potentially-sensitive information about unpublished items, especially the developers, publishers or closed beta testers.

---

### `SecretDepotIDs`

`ImmutableHashSet<uint>` TYPE WIF DEFAULT VALUE OV BEAN EMPTY. This property specifies `depotIDs` that the plugin won't resolve, and if they're already resolved, won't submit the key for. This property can be useful for people with access to potentially-sensitive information about unpublished items, especially the developers, publishers or closed beta testers.

---

### `SecretPackageIDs`

`ImmutableHashSet<uint>` TYPE WIF DEFAULT VALUE OV BEAN EMPTY. This property specifies `packageIDs` (also known as `subIDs`) that the plugin won't resolve, and if they're already resolved, won't submit the token for. This property can be useful for people with access to potentially-sensitive information about unpublished items, especially the developers, publishers or closed beta testers.

---

### `SkipAutoGrantPackages`

`bool` TYPE WIF DEFAULT VALUE OV `false`. This property acts very similar to `SecretPackageIDs` and when enabled, will cause packages with `EPaymentMethod` of `AutoGrant` to be skipped during resolve routine explained below. `AutoGrant` payment method is used by **[Steamworks](https://partner.steamgames.com)** to automatically grant packages on developer accounts. While this is not as explicit as other `Secret` options, and therefore doesn't guarantee anything (since you might have other packages than `AutoGrant` that you still don't want to submit), it should be good enough for skipping majority, if not all, of the secret packages.

---

## Further explanation

At the root level, every Steam account owns a set of packages (licenses, subscriptions), which are classified by their `packageID` (also known as `subID`). Every package may contain several apps classified by their `appID`. Every app may then include several depots classified by their `depotID`.

```text
├── sub/124923
│     ├── app/292030
│     │     ├── depot/292031
│     │     ├── depot/378648
│     │     └── ...
│     ├── app/378649
│     └── ...
└── ...
```

Our plugin includes two routines which take into account skipped items - the resolve routine and submission routine.

The resolve routine is responsible for resolving the tree you can see above. By blacklisting the packages/apps/depots in advance, you'll effectively cut the tree in the place of blacklisted branch/leaf without additional need of specifying the remaining parts of it. In our example above, if `124923` package was ignored, whether by `SecretPackageIDs` or `SkipAutoGrantPackages`, and it was the only package you own which linked to the `292030` appID, then appID `292030` wouldn't get resolved either, and by definition, if there were no other resolved apps which linked to the `292031` and `378648` depots, then they wouldn't get resolved either. However, keep in mind that if the plugin has already resolved the tree, then effectively this will only stop given item from being updated (e.g. new apps added), it will not make the plugin "forget" about the existing items that were already resolved (e.g. apps found in that package before you decided to blacklist it).

The submission routine is responsible for submitting package tokens, app tokens and depot keys of already resolved items (by the resolve routine above). Here your blacklist has immediate effect, as even if the plugin has already resolved the info, the submission routine will not actually submit it over to SteamDB if you have it blacklisted, regardless if it has been resolved or not. Keep in mind however that we're not talking about the tree anymore at this point, the submission routine does not know whether the information about the app comes from this or that package, so it only skips information about particular, blacklisted items, regardless of the relation they're in with other.

For majority of the developers and publishers, it should be enough to enable `SkipAutoGrantPackages`, potentially empowered with `SecretPackageIDs` only, as it effectively cuts the tree at the beginning branch and guarantees that the apps and depots included further will not get submitted as long as there is no other package linking to the same app. If you want to be double sure, in addition to that you can also use `SecretAppIDs`, which will skip the resolve of the app even if there are some other licenses you didn't blacklist linking to it. Using `SecretDepotIDs` should not be needed, unless you have a particular, specific need (such as skipping only a particular depot while still submitting info about packages and apps), or if you want to add yet another layer to be triple safe.