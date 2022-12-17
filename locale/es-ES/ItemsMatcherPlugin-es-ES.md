# ItemsMatcherPlugin

`ItemsMatcherPlugin` is official ASF **[plugin](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins)** that extends ASF with ASF STM listing features. In particular, this includes `PublicListing` in **[`RemoteCommunication`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#remotecommunication)** and `MatchActively` in **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#tradingpreferences)**. ASF comes with `ItemsMatcherPlugin` bundled together with the release, therefore it's ready for usage right away.

---

## `PublicListing`

Public listing, as the name implies, is listing of currently available ASF STM bots. It's located on **[our website](https://asf.justarchi.net/STM)**, managed automatically and used as a public service for both ASF users that make use of `MatchActively`, as well as ASF and non-ASF users for manual matching.

While `PublicListing` is enabled by default, please note that you will **not** be displayed on the website if you do not meet all of the requirements, especially `SteamTradeMatcher`, which isn't enabled by default. For people that do not meet the criteria, even if they kept `PublicListing` enabled, ASF doesn't communicate with the server in any way. Public listing is also compatible only with latest stable version of ASF and may refuse to display outdated bots, especially if they're missing core functionality that can be found only in newer versions.

### Cómo funciona exactamente

ASF envía información inicial después de iniciar sesión, que contiene todas las propiedades de las que hace uso la lista pública. Luego, cada 10 minutos ASF envía una pequeña solicitud "latido" que notifica a nuestro servidor que el bot todavía está funcionando. Si por alguna razón el latido no llega, por ejemplo debido a problemas de red, entonces ASF intentará enviarlo cada minuto, hasta que el servidor lo registre. This way our server knows precisely which bots are still running and ready to accept trade offers. ASF will also send initial announcement on as-needed basis, for example if it detects that our inventory has changed since the previous one.

We display all ASF 2FA+STM accounts that were active in the **last 15 minutes**. Users are sorted according to their relative usefulness - `MatchEverything` bots which are shown with `Any` banner that accept all 1:1 trades, then unique games count, and finally items count.

### API

La lista ASF STM solo acepta bots de ASF por el momento. There is no way to list third-party bots on our listing for now, as we can't review their code easily and ensure they meet our entire trading logic. Participation in the listing therefore requires latest stable ASF version, although it can run with custom **[plugins](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins)**.

For consumers of the listing, we have a very simple **[`/Api/Listing/Bots`](https://asf.justarchi.net/Api/Listing/Bots)** endpoint that you can use. It includes all the data we have, apart from inventories of users which are part of `MatchActively` feature exclusively.

### Política de privacidad

If you agree to being listed in our listing, by enabling `SteamTradeMatcher` and not refusing `PublicListing`, as specified above, we'll temporarily store some of your Steam account details on our server in order to provide the expected functionality.

La información pública (expuesta por Stea a todas las partes interesadas) incluye:
- Tu identificador de Steam (en forma de 64 bits, para generar enlaces)
- Tu nombre de usuario (para efectos de visualización)
- Tu avatar (para efectos de visualización)

Semi-public info (exposed by Steam to every interested party if you meet listing requirements) includes:
- Your **[inventory](https://steamcommunity.com/my/inventory/#753_6)** (so people can use `MatchActively` against your items).

La información privada (datos seleccionados necesarios para proporcionar la funcionalidad) incluye:
- Tu **[token de intercambio](https://steamcommunity.com/my/tradeoffers/privacy)** (para que las personas fuera de tu lista de amigos puedan enviarte intercambios)
- Your `MatchableTypes` setting (for display purposes and matching)
- Your `MatchEverything` setting (for display purposes and matching)
- Your `MaxTradeHoldDuration` setting (so other people know whether you're willing to accept their trades)

---

## `MatchActively`

`MatchActively` setting is active version of **[`SteamTradeMatcher`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading#steamtradematcher)** including interactive matching in which the bot will send trades to other people. Puede funcionar solo, o junto con el ajuste `SteamTradeMatcher`. This feature requires `LicenseID` to be set, as it uses third-party server and paid resources to operate.

Para usar esa opción, tienes que cumplir ciertos requisitos. Como mínimo debes tener una cuenta **[deslimitada](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663)**, **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-es-es#asf-2fa)** activo y por lo menos un tipo válido en `MatchableTypes`, tal como los cromos.

If you meet all of the requirements above, ASF will periodically communicate with our **[public ASF STM listing](#publiclisting)** in order to actively match bots that are currently available.

- In each round ASF will fetch our inventory and inventory of all available bots listed in order to find `MatchableTypes` items that can be matched. Thanks to communicating directly with our server, this process requires a single request and we have immediate information whether any available bot offers something interesting for us - if match is found, ASF will send and confirm trade offer automatically.
- Cada set (compuesto de appID, tipo y rareza del artículo) puede ser emparejado solo una vez en cada ronda. Esto se implementa para minimizar el error que indica que "los artículos ya no están disponibles para intercambiar" y evitar la necesidad de esperar a que cada bot reaccione antes de enviar todos los intercambios. También es la razón principal por la que el emparejamiento se compone de rondas y no de un proceso continuo.
- ASF no enviará más de `255` artículos en un solo intercambio, y no más de `5` intercambios al mismo usuario en una sola ronda. Esto es impuesto por los límites de Steam, así como por nuestro propio equilibrio de carga.

Este módulo debe ser transparente. El emparejamiento comenzará en aproximadamente `1` hora desde el inicio de ASF, y se repetirá cada `6` horas (si es necesario). La función `MatchActively` está diseñada para ser usada como una medida periódica a largo plazo, para asegurar que avanzamos activamente hacia completar sets, pero sin la presión a corto plazo de tiempo y recursos que surgiría si esto se ofreciera como un comando. Los usuarios objetivo de este módulo son cuentas principales y cuentas alternas usadas para "almacenar", aunque puede ser usado por cualquier bot que no esté configurado a `MatchEverything`.

ASF hace todo lo posible para minimizar la cantidad de solicitudes y presión generada por usar esta opción, al mismo tiempo que maximiza la eficiencia del emparejamiento. El algoritmo exacto para elegir los bots a emparejar y organizar todo el proceso, es un detalle de implementación de ASF y puede cambiar por la retroalimentación, la situación y posibles futuras ideas.

La versión actual del algoritmo hace que ASF dé prioridad a bots `Any`, especialmente aquellos con mejor diversidad de juegos de los que provienen sus artículos. When running out of `Any` bots, ASF will move on to the `Fair` ones upon same diversity rule. ASF will try to match every available bot at least once, to ensure that we're not missing on a possible set progress.

`MatchActively` toma en cuenta los bots que bloqueaste del intercambio a través del **[comando](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-es-es)** `tbadd` y no intentará emparejar activamente con ellos. Esto puede ser usado para decirle a ASF con qué bots nunca debería emparejar, incluso si tienen posibles duplicados que nos pudieran servir.

---

### Why do I need a `LicenseID` to use `MatchActively`? Wasn't it free before?

ASF is, and remains, free and open-source, as it was established at the start of the project back in October 2015. Our program is also entirely non-commercial, we do not earn anything from contributions to it, building or publishing. Over those past 7+ years ASF has received tremendous amount of development, and it's still being improved and enhanced with every monthly stable release mostly by a single person, **[JustArchi](https://github.com/JustArchi)** - with no strings attached. The only funding we receive is from non-obligatory donations that come from our users.

For a very long time, until October 2022, `MatchActively` feature was part of ASF core and available for everyone to use. In October 2022, Valve, the company behind Steam, has put very severe rate limits that rendered previous functionality entirely broken, with no solution available. The feature therefore had to be removed from ASF core in version 5.4.1.0.

`MatchActively` was resurrected as part of official `ItemsMatcher` plugin that further enhances ASF with active cards matching functionality. Resurrecting `MatchActively` feature required from us **extraordinary amount of work** to create ASF backend, entirely new service hosted on a server, with more than a thousand of proxies attached for resolving inventories, all exclusively to allow ASF clients to make use of `MatchActively` like before. Due to the amount of work involved, as well as resources that are not free and require to be paid on monthly basis by us (domain, server, proxies), we've decided to offer this functionality to our sponsors, that is, people that already support ASF project on monthly basis. Our goal isn't to profit from it, but rather, cover the **monthly costs** that are exclusively linked with offering this option - that's why we offer it basically for nothing, but we do have to charge a little for it as we can't pay hundreds of dollars from our own pockets just to make it available for you. We hope that you understand.

---

### How can I get an access?

`ItemsMatcher` is offered as part of $5+ sponsor tier on **[JustArchi's GitHub](https://github.com/sponsors/JustArchi)**. Simply become a sponsor of $5 tier (or higher), then read **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#licenseid)** section to obtain and fill `LicenseID`.

The license allows you to send limited amount of requests to the server. $5 tier allows you to use `MatchActively` for one account, which should be suitable for majority of people. $10 tier allows you to use it on three accounts. If you require more resources, **[let us know](mailto:ASF@JustArchi.net)**.