# Comunicación remota

Esta sección explica la comunicación remota que ASF incorpora, incluyendo más información sobre cómo uno puede influenciarla. Si bien no consideramos nada de lo siguiente como malicioso o no deseado, ni estamos legalmente obligados a revelarlo, queremos que entiendas mejor la funcionalidad del programa, especialmente en lo que respecta a tu privacidad y los datos que se comparten.

## Steam

ASF se comunica con la red de Steam (**[servidores CM](https://api.steampowered.com/ISteamDirectory/GetCMList/v1?cellid=0)**), así como **[Steam API](https://steamcommunity.com/dev)**, **[la tienda de Steam](https://store.steampowered.com)** y **[la comunidad de Steam](https://steamcommunity.com)**.

No es posible desactivar ninguna de las comunicaciones mencionadas, ya que es lo principal en que ASF se basa para proporcionar su funcionalidad básica. Deberás abstenerte de usar ASF si no estás de acuerdo con lo anterior.

## Grupo de Steam

ASF se comunica con nuestro **[grupo de Steam](https://steamcommunity.com/groups/archiasf)**. El grupo te proporciona anuncios, especialmente de versiones nuevas, problemas críticas, problemas de Steam y otras cosas que son importantes para mantener actualizada a la comunidad. También te permite usar nuestro soporte técnico, haciendo preguntas, resolviendo problemas, reportando problemas o sugiriendo mejoras. Por defecto, las cuentas usadas en ASF se unirán automáticamente al grupo al iniciar sesión.

Puedes optar por no unirte al grupo desactivando la bandera `SteamGroup` en las opciones **[`RemoteCommunication`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-es-ES#remotecommunication)** del bot.

## GitHub

ASF se comunica con la **[API de GitHub](https://api.github.com)** para obtener las **[versiones de ASF](https://github.com/JustArchiNET/ArchiSteamFarm/releases)** para la funcionalidad de actualización. Esto se hace como parte de las actualizaciones automáticas (si dejaste **[`UpdatePeriod`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-es-ES#updateperiod)** habilitado), así como con el **[comando](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-es-ES)** `update`. Puedes influir la comunicación de ASF con GitHub a través de la propiedad **[`UpdateChannel`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-es-ES#updatechannel)** - estableciéndola a `None` resultará en deshabilitar por completo la funcionalidad de actualización, incluyendo la comunicación con GitHub en este sentido.

## Servidor de ASF

ASF se comunica con **[nuestro servidor](https://asf.justarchi.net)** para funcionalidades avanzadas. En particular, esto incluye:
- Verificar las sumas de comprobación de las compilaciones de ASF descargadas de GitHub contra nuestra base de datos independiente para asegurar que todas las compilaciones descargadas sean legítimas (libres de malware, ataques de intermediario u otro tipo de manipulación)
- Anunciar tu bot en **[nuestra lista](https://asf.justarchi.net/STM)** si habilitaste `SteamTradeMatcher` en **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-es-ES#tradingpreferences)** y cumples con otros criterios.
- Descargar los bots disponibles para intercambiar de **[nuestra lista](https://asf.justarchi.net/STM)** si habilitaste `MatchActively` en **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-es-ES#tradingpreferences)** y cumples con otros criterios.

Como medida de seguridad, no es posible desactivar la suma de verificación para las compilaciones de ASF. Sin embargo, puedes desactivar las actualizaciones automáticas si deseas evitar esto, como se describió anteriormente en la sección GitHub.

Puedes optar por no ser anunciado en la lista desactivando la bandera `PublicListing` en las opciones **[`RemoteCommunication`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-es-ES#remotecommunication)** del bot. Esto puede ser útil si quieres ejecutar un bot con `SteamTradeMatcher` sin ser anunciado al mismo tiempo.

Descargar los bots de nuestro listado es obligatorio para la opción `MatchActively`, necesitarás deshabilitar dicha opción si no deseas aceptar eso.

---

## Lista pública ASF STM

Nuestra lista pública ASF STM está ubicada en **[nuestro sitio web](https://asf.justarchi.net/STM)** y es usada como un servicio público tanto para usuarios de ASF que usan `MatchActively`, así como para usuarios y no usuarios de ASF para emparejamiento manual.

Ten en cuenta que **no** serás mostrado en el sitio web si no cumples con todos los requisitos. En este caso ASF ni siquiera se tomará la molestia de comunicarse con nuestro servidor, por lo que esta sección se omite completamente si intencionalmente no habilitaste `SteamTradeMatcher` para ayudarte a emparejar duplicados. Además, la lista pública es compatible solo con la última versión estable de ASF y puede negarse a mostrar bots desactualizados, especialmente si les falta alguna funcionalidad crucial que solo se puede encontrar en las versiones más recientes.

### Cómo funciona exactamente

ASF envía información inicial después de iniciar sesión, que contiene todas las propiedades de las que hace uso la lista pública. Luego, cada 10 minutos ASF envía una pequeña solicitud "latido" que notifica a nuestro servidor que el bot todavía está funcionando. Si por alguna razón el latido no llega, por ejemplo debido a problemas de red, entonces ASF intentará enviarlo cada minuto, hasta que el servidor lo registre.

Esto permite a nuestro sitio web registrar qué cuentas pueden ser usadas para emparejamiento, así como para registrar si todavía están activas. Gracias a eso, nuestro sitio web puede mostrar todas las cuentas ASF 2FA+STM que estaban activas en los **últimos 15 minutos**.

Los usuarios se ordenan de acuerdo a sus inventarios (en orden descendente) - bots con `MatchEverything` con la etiqueta `Any` que aceptan todos los intercambios 1:1, luego por cantidad de juegos únicos `MatchableTypes`, y finalmente por cantidad de artículos `MatchableTypes`.

### API

La lista ASF STM solo acepta bots de ASF por el momento. No hay forma de listar bots de terceros por ahora (ya que no podemos revisar fácilmente su código y asegurarnos de que cumplen con nuestra lógica de intercambio).

Si buscas una forma sencilla de acceder a nuestro listado de manera programática, tenemos un endpoint **[/Api/Bots](https://asf.justarchi.net/Api/Bots)** muy simple que puedes usar. Este es también el endpoint que ASF usa internamente para los usuarios de `MatchActively`.

### Política de privacidad

Si aceptas aparecer en nuestro listado, al habilitar `SteamTradeMatcher` y no rechazando `PublicListing`, como se especificó arriba, temporalmente almacenaremos algunos detalles de tu cuenta de Steam en nuestro servidor para proporcionar la funcionalidad principal.

La información pública (expuesta por Stea a todas las partes interesadas) incluye:
- Tu identificador de Steam (en forma de 64 bits, para generar enlaces)
- Tu nombre de usuario (para efectos de visualización)
- Tu avatar (para efectos de visualización)

La información privada (datos seleccionados necesarios para proporcionar la funcionalidad) incluye:
- Tu **[token de intercambio](https://steamcommunity.com/my/tradeoffers/privacy)** (para que las personas fuera de tu lista de amigos puedan enviarte intercambios)
- Tu `MaxTradeHoldDuration` (para que otras personas sepan si estás dispuesto a aceptar sus intercambios)
- Tus `MatchableTypes` (para efectos de visualización y emparejamiento)
- El número total de artículos `MatchableTypes` en tu inventario (para efectos de visualización y emparejamiento)
- El número total de juegos de los que provienen los artículos `MatchableTypes` mencionados antes (para efectos de visualización y emparejamiento)
- El valor de `MatchEverything` en **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-es-ES#tradingpreferences)** (para efectos de visualización y emparejamiento)

El servidor de ASF **no** recolectará, almacenará o procesará cualquier otra información no mencionada arriba, sin previo aviso en el registro de cambios, y sin una buena razón práctica en primer lugar. No consideramos nada de lo anterior como un asunto grave, y lo mencionamos para que sepas exactamente lo que hace ASF además de aquello para lo que lo configuraste, para que la gente pueda entender mejor el proceso.

Tu información se ocultará automáticamente del público general en hasta 15 minutos desde el momento en que dejes de usar nuestro listado, ya sea por un cambio en la configuración o por ya no tener ASF en ejecución. Además, será borrada automáticamente de nuestro servidor (incluyendo todas las copias de respaldo) en hasta 7 días desde que ocurra lo mencionado anteriormente.