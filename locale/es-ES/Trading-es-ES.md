# Intercambios

ASF incluye soporte para intercambios no interactivos (offline) de Steam. Tanto la recepción (aceptar/rechazar) como el envío de intercambios está disponible inmediatamente y no requiere una configuración especial, pero obviamente se requiere una cuenta de Steam sin restricciones (aquella que ya ha gastado 5$ en la tienda). El módulo de intercambios no está disponible para cuentas limitadas.

---

## Lógica

ASF siempre aceptará todos los intercambios, independientemente de los artículos, enviados por el usuario con acceso `Master` (o superior) al bot. Esto permite no solo recoger (loot) fácilmente los cromos recolectados por el bot, sino también administrar fácilmente los artículos que el bot almacena en el inventario - incluyendo los de otros juegos (como CS:GO).

ASF rechazará las ofertas de intercambio, independientemente del contenido, de cualquier usuario (no master) que esté bloqueado del módulo de intercambios. La lista negra se almacena en la base de datos estándar `BotName.db`, y puede ser administrada por medio de los **[comandos](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-es-es)** `bl`, `bladd` y `blrm`. Esto debe servir como una alternativa al bloqueo de usuarios que ofrece Steam - usar con precaución.

ASF aceptará todos los intercambios de `loot` enviados entre bots, a menos que `DontAcceptBotTrades` se especifique en `TradingPreferences`. En resumen, `TradingPreferences` por defecto en `None` hará que ASF acepte automáticamente intercambios del usuario con acceso `Master` al bot (explicado arriba), así como todos los intercambios de donación de otros bots que formen parte del proceso de ASF. Si quieres deshabilitar los intercambios de donación de otros bots, entonces para eso es `DontAcceptBotTrades` en la propiedad `TradingPreferences`.

Cuando habilitas `AcceptDonations` en `TradingPreferences`, ASF también aceptará cualquier intercambio de donación - un intercambio en el que la cuenta bot no pierde ningún artículo. Esta propiedad solo afecta cuentas no bot, ya que las cuentas bot son afectadas por `DontAcceptBotTrades`. `AcceptDonations` te permite aceptar fácilmente donaciones de otras personas, y también de bots que no formen parte del proceso de ASF.

Es bueno notar que `AcceptDonations` no requiere **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-es-es)**, ya que no se necesita una confirmación si no estamos perdiendo ningún artículo.

También puedes personalizar aún más las capacidades de intercambio de ASF modificando `TradingPreferences` según lo deseado. Una de las características principales de `TradingPreferences` es la opción `SteamTradeMatcher` que hará que ASF use su lógica integrada para aceptar intercambios que te ayuden a completar insignias faltantes, lo que es especialmente útil en cooperación con la lista pública de **[SteamTradeMatcher](https://www.steamtradematcher.com)**, pero también puede funcionar sin ella. Se describe con detalle abajo.

---

## `SteamTradeMatcher`

Cuando `SteamTradeMatcher` está activo, ASF usará un algoritmo bastante complejo para comprobar si el intercambio pasa las reglas de STM y que al menos sea neutral para nosotros. La lógica es la siguiente:

- Rechaza el intercambio si estamos perdiendo algún tipo de artículo que no esté especificado en la propiedad `MatchableTypes`.
- Rechaza el intercambio si no estamos recibiendo al menos el mismo número de artículos por juego, por tipo y por rareza.
- Rechaza el intercambio si el usuario solicita cromos especiales de las ofertas de verano/invierno, y tiene una retención de intercambio.
- Rechaza el intercambio si la duración de la retención de intercambio excede lo establecido en la propiedad de configuración global `MaxTradeHoldDuration`.
- Rechaza el intercambio si no tenemos establecido `MatchEverything`, y es peor que neutral para nosotros.
- Acepta el intercambio si no lo rechazamos por alguno de los puntos anteriores.

Es agradable notar que ASF también soporta pagos en exceso - la lógica funcionará correctamente cuando el usuario añada algo adicional al intercambio, siempre y cuando se cumplan todas las condiciones anteriores.

Las primeras 4 condiciones de rechazo deberían ser obvias para todos. La última incluye una lógica para cromos duplicados que comprueba el estado actual de nuestro inventario y decide cuál es el estatus del intercambio.

- Un intercambio es **bueno** si nuestro progreso para completar el set avanza. Ejemplo: A A (antes) -> A B (después)
- Un intercambio es **neutral** si nuestro progreso para completar el set se mantiene intacto. Ejemplo: A B (antes) -> A C (después)
- Un intercambio es **malo** si nuestro progreso para completar el set retrocede. Ejemplo: A C (antes) -> A A (después)

STM solo opera con intercambios buenos, lo que significa que un usuario utilizando STM para emparejar duplicados siempre debería sugerirnos intercambios buenos. Sin embargo, ASF es liberal, y también acepta intercambios neutrales, porque en esos intercambios realmente no estamos perdiendo nada, así que no hay ninguna razón para rechazarlos. Esto es especialmente útil para tus amigos, ya que pueden cambiar tus cromos adicionales sin usar STM en absoluto, siempre y cuando no pierdas el progreso del set.

Por defecto ASF rechazará intercambios malos - como usuario, esto es lo que querrás casi siempre. Sin embargo, opcionalmente puedes habilitar `MatchEverything` en `TradingPreferences` para hacer que ASF acepte todos los intercambios de duplicados, incluyendo los **malos**. Esto es útil si deseas ejecutar un bot de intercambio 1:1 en tu cuenta, y por lo tanto debes entender que **ASF ya no te ayudará en el progreso para completar las insignias, y que podrías perder sets completos por N cantidad de duplicados del mismo cromo**. A menos que intencionalmente quieras ejecutar un bot de intercambio que **nunca** se supone que complete ningún set, no quieres habilitar esta opción.

Independientemente de las `TradingPreferences` que hayas seleccionado, que un intercambio sea rechazado por ASF no significa que tú no puedas aceptarlo. Si dejaste el valor predeterminado de `BotBehaviour`, el cual no incluye `RejectInvalidTrades`, ASF simplemente ignorará esos intercambios - permitiéndote decidir si estás interesado en ellos o no. Lo mismo ocurre con los intercambios con artículos fuera de los incluidos en `MatchableTypes`, así como con todo lo demás - se supone que el módulo te ayude a automatizar los intercambios de STM, no decidir cuál es un intercambio bueno y cuál no. La única excepción a esta regla es cuando se trata de usuarios que pusiste en la lista negra del módulo de intercambios usando el comando `bladd` - los intercambios de esos usuarios son rechazados inmediatamente, independientemente de la configuración de `BotBehaviour`.

Es altamente recomendable usar **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-es-es)** cuando habilitas esta opción, ya que esta función pierde todo su potencial si decides confirmar manualmente cada intercambio. `SteamTradeMatcher` funcionará correctamente incluso sin la capacidad de confirmar intercambios, pero puede generar un atraso en las confirmaciones si no las aceptas a tiempo.

---

### `MatchActively`

El ajuste `MatchActively` es una versión extendida de `SteamTradeMatcher` que además del emparejamiento pasivo ofrecido por esta opción, también incluye emparejamiento activo en el que el bot enviará intercambios a otras personas.

Para usar esa opción, tienes que cumplir ciertos requisitos. Primero, necesitas habilitar `SteamTradeMatcher` (puesto que esta característica es una extensión de aquella), y asegurarte de que tienes `MatchEverything` **deshabilitado** (ya que los bots de intercambio nunca emparejan activamente). Además, tienes que ser elegible para nuestra **[lista ASF STM](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Statistics-es-es#pol%C3%ADtica-de-privacidad-actual)**, con requisitos más flexibles. Como mínimo debes tener `Statistics` habilitado, una cuenta **[ilimitada](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663)**, **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-es-es#asf-2fa)** activo y por lo menos un tipo válido en `MatchableTypes`, tal como cromos.

Si cumples todos los requisitos anteriores, ASF se comunicará periódicamente con nuestra **[lista pública de ASF STM](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Statistics-es-es#lista-p%C3%BAblica-asf-stm)** para emparejar activamente con los bots que estén disponibles.

- Cada sesión de emparejamiento se compone de "rondas", con hasta `10` siendo el máximo en una sesión de emparejamiento.
- En cada ronda ASF analizará nuestro inventario y el inventario de los bots seleccionados que estén listados para encontrar artículos `MatchableTypes` que puedan ser emparejados. Si se encuentra una coincidencia, ASF enviará y confirmará automáticamente la oferta de intercambio.
- Cada set (compuesto de appID, tipo y rareza del artículo) puede ser emparejado solo una vez en cada ronda. Esto se implementa para minimizar el error que indica que "los artículos ya no están disponibles para intercambiar" y evitar la necesidad de esperar a que cada bot reaccione antes de enviar todos los intercambios. También es la razón principal por la que el emparejamiento se compone de rondas y no de un proceso continuo.
- ASF no enviará más de `255` artículos en un solo intercambio, y no más de `5` intercambios al mismo usuario en una sola ronda. Esto es impuesto por los límites de Steam, así como por nuestro propio equilibrio de carga.
- ASF tiene un límite de `40` bots únicos que pueden emparejarse en una ronda, a menos que se cancele antes por haberse quedado sin sets para emparejar - en este caso, durante la siguiente ronda ASF tratará de emparejar primero los bots que no fueron emparejados previamente.
- Si ASF determina que el emparejamiento debe continuar, la siguiente ronda comienza dentro de `5` minutos desde la anterior (para añadir un tiempo de espera y permitir que todos los bots reaccionen a nuestros intercambios), de lo contrario la sesión de emparejamiento termina y se repite en `8` horas.

Este módulo debe ser transparente. El emparejamiento comenzará en aproximadamente `1` hora desde el inicio de ASF, y se repetirá cada `8` horas (si es necesario). La función `MatchActively` está diseñada para ser usada como una medida periódica a largo plazo, para asegurar que avanzamos activamente hacia completar sets, pero sin la presión a corto plazo de tiempo y recursos que surgiría si esto se ofreciera como un comando. Los usuarios objetivo de este módulo son cuentas principales y cuentas alternas usadas para "almacenar", aunque puede ser usado por cualquier bot que no esté configurado a `MatchEverything`.

ASF hace todo lo posible para minimizar la cantidad de solicitudes y presión generada por usar esta opción, al mismo tiempo que maximiza la eficiencia del emparejamiento. El algoritmo exacto para elegir los bots a emparejar y organizar todo el proceso, es un detalle de implementación de ASF y puede cambiar por la retroalimentación, la situación y posibles futuras ideas.

La versión actual del algoritmo hace que ASF dé prioridad a bots `Any`, especialmente aquellos con mejor diversidad de juegos de los que provienen sus artículos. Si se acaban los bots `Any`, ASF pasará a los de intercambio justo con la misma regla de diversidad, con aquellos que tengan un número excesivo de artículos con menos prioridad debido a que tienen una mayor probabilidad de presentar problemas relacionados con el inventario en comparación con otros bots. Independientemente de eso, ASF intentará emparejar cada bot disponible al menos una vez, para asegurar que no perdemos el posible progreso de un set.

`MatchActively` toma en cuenta los bots que bloqueaste del intercambio a través del **[comando](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-es-es)** `bladd` y no intentará emparejar activamente con ellos. Esto puede ser usado para decirle a ASF con qué bots nunca debería emparejar, incluso si tienen posibles duplicados que nos pudieran servir.