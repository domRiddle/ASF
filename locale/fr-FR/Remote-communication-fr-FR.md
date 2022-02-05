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

## Liste publique ASF STM

Our public ASF STM listing is located on **[our website](https://asf.justarchi.net/STM)** and used as a public service for both ASF users that make use of `MatchActively`, as well as ASF and non-ASF users for manual matching.

### Comment ça marche exactement

ASF envoie les données initiales une fois après la connexion, contenant toutes les propriétés que la liste publique utilise. Ensuite, toutes les 10 minutes, ASF envoie une très petite requête de type "pulsation" qui informe notre serveur que le bot est toujours opérationnel. Si, pour une raison quelconque, la pulsation n’arrivait pas, par exemple à cause de problèmes de réseau, ASF tentera de l’envoyer à chaque minute, jusqu’à ce que le serveur l’enregistre.

Cela permet à notre site Web d'enregistrer les comptes pouvant être utilisés pour le matching, ainsi que s'ils sont toujours actifs. Grâce à cela, notre site Web peut afficher tous les comptes ASF 2FA + STM actifs au cours des ** 15 dernières minutes </ 0>.</p>

Users are sorted according to their inventories (in descending order) - `MatchEverything` bots with `Any` banner that accept all 1:1 trades, then `MatchableTypes` unique games count, and finally `MatchableTypes` items count.

Veuillez noter que vous ne serez **pas** affiché sur le site si vous ne remplissez pas toutes les exigences. ASF won't even bother communicating with our server in this case, so second point is entirely skipped for you if you didn't intentionally enable `SteamTradeMatcher` in order to help yourself match dupes. Also public listing is compatible only with latest stable version of ASF and may refuse to display outdated bots, especially if they're missing core functionality that can be found only in newer versions.

### API

ASF STM accepte seulement les bots ASF pour l’instant. Il n’y a aucun moyen pour répertorier les bots de tiers sur notre liste pour l’instant (nous ne pouvons pas revoir leur code facilement et s’assurer qu’ils répondent à notre toute logique commerciale).

Si vous cherchez un moyen facile de notre liste d’accès de manière programmatique, nous avons un point de **

/Api/Bots</0 très simple que vous pouvez utiliser. This is also the endpoint that ASF uses internally for `MatchActively` users.</p> 



### Privacy policy

If you agree to being listed in our listing, by enabling `SteamTradeMatcher` and not refusing `PublicListing`, as specified above, we'll temporarily store some of your Steam account details on our server in order to provide the core functionality.

Public info (exposed by Steam to every interested party) includes:

- Votre identificateur Steam (sous forme 64 bits, pour générer des liens)
- Votre pseudo (pour l'affichage)
- Votre avatar (hash, pour l'affichage)
- Total number of `MatchableTypes` Steam items in your inventory (for display purposes and matching)
- Total number of unique games that above `MatchableTypes` Steam items are made of (for display purposes and matching)

Private info (selected data required for providing the functionality) includes:

- Votre ** jeton de d'échange </ 0> (afin que des personnes ne faisant pas partie de votre liste d'amis puissent vous envoyer des échanges)</li> 
  
  - Your `MatchableTypes` (for display purposes and matching)
- Value of `MatchEverything` in your **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#tradingpreferences)** (for display purposes and matching)</ul> 

ASF server will **not** collect, store or otherwise process any other data not listed above, without prior important notice in the changelog, and a very good practical reason in the first place. We do not consider anything above to be a serious matter, and we mention it to let you know what precisely ASF does apart of what you configured it to do yourself, so people can better understand the process.

Your data will be automatically hidden from general public in up to 15 minutes since the moment you stop using our listing, whether due to change of settings or not having ASF launched anymore. In addition to that, it'll be automatically deleted from our server (including all backup copies) in up to 7 days since the above happening.