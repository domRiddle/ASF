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
- Announcing your bot in **[our listing](https://asf.justarchi.net/STM)** if you've enabled `SteamTradeMatcher` in **[`TradingPreferences`](#tradingpreferences)** and meet other criteria
- Downloading currently available bots to trade from **[our listing](https://asf.justarchi.net/STM)** if you've enabled `MatchActively` in **[`TradingPreferences`](#tradingpreferences)** and meet other criteria

As a security measure, it's not possible to disable checksum verification for ASF builds. However, you can disable auto-updates entirely if you'd like to avoid this, as described above in the GitHub section.

You can decide to opt-out of being announced in the listing by disabling `PublicListing` flag in bot's **[`RemoteCommunication`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#remotecommunication)** settings. This might be useful if you'd like to run `SteamTradeMatcher` bot without being announced at the same time.

Downloading bots from our listing is mandatory for `MatchActively` setting, you'll need to disable that setting if you're unwilling to accept that.

---

## Lista pública ASF STM

Nuestra lista pública ASF STM está ubicada en **[nuestro sitio web](https://asf.justarchi.net/STM)** y es usada como un servicio público tanto para usuarios de ASF que usan `MatchActively`, así como para usuarios y no usuarios de ASF para emparejamiento manual.

### Cómo funciona exactamente

ASF envía información inicial después de iniciar sesión, que contiene todas las propiedades de las que hace uso la lista pública. Luego, cada 10 minutos ASF envía una pequeña solicitud "latido" que notifica a nuestro servidor que el bot todavía está funcionando. Si por alguna razón el latido no llega, por ejemplo debido a problemas de red, entonces ASF intentará enviarlo cada minuto, hasta que el servidor lo registre.

Esto permite a nuestro sitio web registrar qué cuentas pueden ser usadas para emparejamiento, así como para registrar si todavía están activas. Gracias a eso, nuestro sitio web puede mostrar todas las cuentas ASF 2FA+STM que estaban activas en los **últimos 15 minutos**.

Los usuarios se ordenan de acuerdo a sus inventarios (en orden descendente) - bots con `MatchEverything` con la etiqueta `Any` que aceptan todos los intercambios 1:1, luego por cantidad de juegos únicos `MatchableTypes`, y finalmente por cantidad de artículos `MatchableTypes`.

Ten en cuenta que **no** serás mostrado en el sitio web si no cumples con todos los requisitos. En este caso ASF ni siquiera se tomará la molestia de comunicarse con nuestro servidor, así que el segundo punto se omite completamente si intencionalmente no habilitaste `SteamTradeMatcher` para ayudarte a emparejar duplicados. Además la lista pública es compatible solo con la última versión estable de ASF y puede negarse a mostrar bots desactualizados, especialmente si les falta alguna funcionalidad crucial que solo se puede encontrar en las versiones más recientes.

### API

La lista ASF STM solo acepta bots de ASF por el momento. No hay forma de listar bots de terceros por ahora (ya que no podemos revisar fácilmente su código y asegurarnos de que cumplen con nuestra lógica de intercambio).

Si buscas una forma sencilla de acceder a nuestro listado de manera programática, tenemos un endpoint **[/Api/Bots](https://asf.justarchi.net/Api/Bots)** muy simple que puedes usar. Este es también el endpoint que ASF usa internamente para los usuarios de `MatchActively`.

### Privacy policy

If you agree to being listed in our listing, by enabling `SteamTradeMatcher` and not refusing `PublicListing`, as specified above, we'll temporarily store some of your Steam account details on our server in order to provide the core functionality.

Public info (exposed by Steam to every interested party) includes:
- Tu identificador de Steam (en forma de 64 bits, para generar enlaces)
- Tu nombre de usuario (para efectos de visualización)
- Tu avatar (para efectos de visualización)
- El número total de artículos `MatchableTypes` en tu inventario (para efectos de visualización y emparejamiento)
- El número total de juegos de los que provienen los artículos `MatchableTypes` mencionados antes (para efectos de visualización y emparejamiento)

Private info (selected data required for providing the functionality) includes:
- Tu **[token de intercambio](https://steamcommunity.com/my/tradeoffers/privacy)** (para que las personas fuera de tu lista de amigos puedan enviarte intercambios)
- Tus `MatchableTypes` (para efectos de visualización y emparejamiento)
- Value of `MatchEverything` in your **[`TradingPreferences`](#tradingpreferences)** (for display purposes and matching)

ASF server will **not** collect, store or otherwise process any other data not listed above, without prior important notice in the changelog, and a very good practical reason in the first place. We do not consider anything above to be a serious matter, and we mention it to let you know what precisely ASF does apart of what you configured it to do yourself, so people can better understand the process.

Your data will be automatically hidden from general public in up to 15 minutes since the moment you stop using our listing, whether due to change of settings or not having ASF launched anymore. In addition to that, it'll be automatically deleted from our server (including all backup copies) in up to 7 days since the above happening.