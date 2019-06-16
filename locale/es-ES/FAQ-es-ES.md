# Preguntas frecuentes

Nuestras preguntas frecuentes básicas cubren preguntas estándar y sus posibles respuestas. Para asuntos menos comunes, por favor visita nuestras **[preguntas frecuentes extendidas](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Extended-FAQ)**.

# Tabla de contenido

- [General](#general)
- [Comparación con herramientas similares](#comparison-with-similar-tools)
- [Seguridad / Privacidad / VAC / Bans / ToS](#security--privacy--vac--bans--tos)
- [Extras](#misc)
- [Problemas](#issues)

* * *

## General

### ¿Cómo funciona exactamente?

Antes de intentar entender lo que es ASF, debes asegurarte de que entiendes lo que son los cromos de Steam, y cómo obtenerlos, lo que se describe bastante bien en el FAQ oficial **[aquí](https://steamcommunity.com/tradingcards/faq)**.

En breve, los cromos de Steam son artículos coleccionables para los que eres elegible cuando tienes un juego en particular, y pueden ser usados para fabricar insignias, venderlos en el mercado de Steam o cualquier otro propósito de tu elección.

Los puntos principales se repiten aquí, porque la gente en general no quiere verlos y actúan como si no existieran:

- **Sí, necesitas tener el juego para poder ser elegible para obtener cromos de él. El préstamo familiar no cuenta.**
- **No, no puedes recolectar el juego infinitamente, cada juego tiene un número fijo de cromos a obtener. Una vez que se agoten los cromos obtenibles en un juego dado (la mitad de un set completo), ya no es candidato para recolección. No importa que tengas el juego, se acabó.**
- **No, no puedes obtener cromos de juegos F2P sin gastar dinero en ellos. Esto incluye juegos permanentemente F2P como Team Fortress 2 o Dota 2.**
- **No, no puedes obtener cromos en cuentas limitadas (aquellas que nunca gastaron $5 en la tienda de Steam), independientemente de los juegos que tenga. Antes era posible, para ya no es el caso.**

Como puedes ver, los cromos de Steam te son otorgados por jugar un juego que compraste, o un juego F2P en el que has invertido dinero. En otras palabras, si juegos a un juego lo suficiente, todos los cromos de ese juego aparecerán en tu inventario, haciendo posible que completes una insignia, los vendas, o hagas lo que quieras.

ASF como programa es bastante complejo para entender completamente, en lugar de explicar todos los detalles técnicos, a continuación te ofreceremos una explicación muy simplificada.

ASF inicia sesión en tu cuenta de Steam a través de nuestro cliente de Steam integrado usando tus credenciales proporcionadas. Después de iniciar sesión con éxito, analiza tus **[insignias](https://steamcommunity.com/my/badges)** para encontrar juegos que estén disponibles para recolección (Puedes obtener X más cromos jugando a este juego). Después de analizar todas las páginas y construir una lista final de todos los juegos disponibles, ASF elige el algoritmo de recolección más eficiente y empieza el proceso. El proceso depende del **[algoritmo de recolección de cromos](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)** seleccionado pero normalmente consiste en jugar un juego elegible y comprobar periódicamente (además de en cada obtención de artículos) si el juego ya fue completamente recolectado - si es así, ASF puede proceder con el siguiente título, usando el mismo procedimiento, hasta que todos los juegos hayan sido recolectados.

Ten en cuenta que la explicación anterior está simplificada y no describe una docena de características adicionales y funciones que ASF ofrece. Visita el resto de **[nuestra wiki](https://github.com/JustArchiNET/ArchiSteamFarm/wiki)** si quieres conocer cada detalle de ASF. Intenté hacerla lo más sencillo posible de entender para todos, sin incluir detalles técnicos - animamos a los usuarios avanzados a profundizar.

Ahora como programa - ASF ofrece algo de magia. Primero, no tiene que descargar ninguno de los archivos de tus juegos, puede jugarlos inmediatamente. Segundo, es totalmente independiente de tu cliente de Steam normal - no necesita ejecutar el cliente de Steam o siquiera instalarlo. Tercero, es una solución automatizada - lo que significa que ASF automáticamente hace todo a tus espaldas, sin necesidad de decirle qué hacer - lo que te ahorra problemas y tiempo. Por último, no tiene que engañar a la red de Steam con un proceso de emulación (como el que usa Idle Master) ya que puede comunicarse directamente con ella. Además es superrápido y ligero, siendo una increíble solución para todos los que quiera obtener cromos fácilmente sin mucha molestia - es especialmente útil al dejar corriendo en segundo plano mientras hacemos algo más, o incluso jugar en modo desconectado.

Todo lo anterior es bueno, pero ASF también tiene limitaciones técnicas que son impuestas por Steam - no podemos recolectar juegos que no poseas, no podemos recolectar juegos por siempre para obtener cromos adicionales alcanzado el límite impuesto, y no podemos recolectar juegos mientras estás jugando. Todo eso debe ser "lógico", considerando la forma en que funciona ASF, pero es bueno observar que ASF no tiene superpoderes y no hará algo que es físicamente imposible, así que ten eso en cuenta - es básicamente lo mismo que si le dijeras a alguien más que inicie sesión en tu cuenta desde otra PC y recolecte juegos por ti.

Para resumir - ASF es un programa que te ayuda a obtener aquellos cromos para los que eres elegible, sin mucho problema. También ofrece varias funciones más, pero vamos a seguir con esta por ahora.

* * *

### ¿Tengo de introducir las credenciales de mi cuenta?

**Yes**. ASF requiere las credenciales de tu cuenta del mismo modo que el cliente oficial de Steam, ya que usa el mismo método para interactuar con la red de Steam. Sin embargo, estoy no significa que tengas que introducir las credenciales de tu cuenta en las configuraciones de ASF, puedes seguir usando ASF con `SteamLogin` y/o `SteamPassword` en `null`/vacío, e introducir esos datos cada vez que ejecutes ASF, cuando sea requerido (así como varias credenciales de inicio de sesión, consulta configuración). De esta manera tus datos no se guardan en ningún lugar, pero obviamente ASF no puede iniciar automáticamente sin tu ayuda. ASF también ofrece otras formas de aumentar tu **[seguridad](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security)**, así que siéntete libre de leer esa parte de la wiki si eres un usuario avanzado. Si no lo eres, y no quieres poner las credenciales de tu cuenta en la configuración de ASF, entonces simplemente no lo hagas, en cambio introdúcelas cuando ASF lo solicite.

Ten en cuenta que la herramienta ASF es para tu uso personal y las credenciales nunca salen de tu computadora. Tampoco las estás compartiendo con nadie, lo que cumple con los Términos de Servicio de Steam - algo muy importante de lo que muchos se olvidan. No estás enviado tus datos a nuestros servidores o a terceros, solo directamente a los servidores de Steam operados por Valve. No conocemos tus credenciales y tampoco podemos recuperarlas por ti, independientemente de si las pones en tus configuraciones o no.

* * *

### ¿Cuánto tengo que esperar para recibir cromos?

**El tiempo que sea necesario** - en serio. Cada juego tiene una dificultad de recolección diferente establecida por el desarrollador/editor, y depende completamente de ellos qué tan rápido se obtienen los cromos. La mayoría de los juegos sueltan 1 cromo por cada 30 minutos de juego, pero también hay juegos que requieren que juegues incluso varias horas antes de soltar un cromo. Además, tu cuenta podría estar restringida de recibir cromos de juegos que aún no has jugado el tiempo suficiente, como se indica en la sección de **[rendimiento](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)**. No intentes adivinar por cuánto tiempo ASF debe recolectar un título dado - no depende de ti, ni de ASF decidirlo. No puedes hacer nada para volverlo más rápido, y no hay ningún "bug" relacionado con que los cromos no caigan de manera oportuna - no controlas el proceso de obtención de cromos, ni tampoco ASF. En el mejor de los casos, recibirás un promedio de 1 cromo por cada 30 minutos. En el peor caso, no recibirás ningún cromo incluso por 4 horas desde que inicie ASF. Ambas situaciones son normales y están cubiertas en nuestra sección de **[rendimiento](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)**.

* * *

### La recolección tarda demasiado, ¿puedo acelerarla de alguna manera?

Lo único que afecta fuertemente la velocidad de recolección es el **[algoritmo de recolección de cromos](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)** seleccionado para tu instancia de bot. Todo lo demás tiene un efecto insignificante y no hará la recolección más rápida, mientras que algunas acciones tal como iniciar el proceso de ASF varias veces incluso **lo hará peor**. Si realmente necesitas aprovechar cada segundo del proceso de recolección, entonces ASF te permite ajustar algunas variables de recolección tal como `FarmingDelay` - todas se explican en **[configuración](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)**. Sin embargo, como dije, el efecto es insignificante, y elegir el algoritmo de recolección de cromos adecuado para una cuenta dada es la única decisión crucial que puede afectar fuertemente la velocidad de recolección, todo lo demás es pura cosmética. En lugar de preocuparte por la velocidad de recolección, solo ejecuta ASF y déjalo hacer su trabajo - puedo asegurarte que lo que está haciendo de la manera más eficaz posible. Cuanto menos te preocupes, más satisfecho estarás.

* * *

### ¡Pero ASF dijo que la recolección tomaría aproximadamente X tiempo!

ASF te da una aproximación basado en el número de cromos que necesitas obtener, y tu algoritmo seleccionado - esto no está nada cerca del tiempo real que pasarás recolectando, que usualmente es más que esto, ya que ASF solo asume el mejor de los casos, e ignora todos los caprichos de la Red de Steam, desconexiones de Internet, sobrecarga de los servidores de Steam y demás. Debe ser visto solo como un indicador general de cuánto puedes esperar que ASF esté recolectando, a menudo en el mejor casos, ya que el tiempo real difiere, de manera significativa en algunos casos. Como se dijo anteriormente, no intentes adivinar por cuánto tiempo un juego dado será recolectado, no depende de ti, ni de ASF decidirlo.

* * *

### ¿ASF puede funcionar en mi android/smartphone?

ASF es un programa C# que requiere la implementación de .NET Core. Actualmente no hay una compilación nativa de .NET Core para Android, hay compilaciones funcionales para linux en arquitectura ARM, así que es totalmente posible usar algo como **[Linux Deploy](https://play.google.com/store/apps/details?id=ru.meefik.linuxdeploy)** para instalar Linux, y luego usar ASF en dicha chroot Linux.

Es muy probable que en el futuro veamos funcionar .NET Core en Android.

* * *

### ¿ASF puede recolectar artículos de juegos de Steam, como CS:GO o Unturned?

**No**, esto está en contra de los Términos de Servicio de Steam y Valve lo manifestó claramente en la última ola de baneos por recolectar artículos de TF2. ASF es un programa de recolección de cromos, no un recolector de artículos de juegos - no tiene ninguna capacidad para recolectar artículos de juegos, y no se planea añadir tal característica en el futuro, nunca, principalmente porque viola los términos de servicio de Steam. Por favor, no preguntes al respecto - lo mejor que obtendrás es un reporte de algún usuario quisquilloso y tú teniendo problemas. Lo mismo sucede para todos lo demás tipos de recolección, como la recolección de las transmisiones de CS:GO. ASF se enfoca exclusivamente en los cromos de Steam.

* * *

### ¿Puedo elegir qué juegos deben ser recolectados?

**Sí**, a través de diferentes formas. Si quieres modificar el orden por defecto de la cola de recolección, para eso se puede usar la **[propiedad de configuración del bot](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#bot-config)** `FarmingOrders`. Si quieres bloquear manualmente ciertos juegos de ser recolectados automáticamente, puedes usar la lista negra de recolección que está disponible con el **[comando](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** `ib`. Si quieres recolectar todo pero darle prioridad a ciertas apps, para eso se puede usar la cola de prioridad de recolección, disponible con el **[comando](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** `iq`. Y finalmente, si solo quieres recolectar juegos de tu elección, para lograrlo puedes usar la **[propiedad de configuración del bot](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#bot-config)** `IdlePriorityQueueOnly`, junto con añadir tus apps seleccionadas a la cola de prioridad de recolección.

Además de administrar el módulo automático de recolección de cromos, que fue descrito arriba, también puedes cambiar ASF al modo de recolección manual con el **[comando](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** `play`, o usar algún otro ajuste externo como la **[propiedad de configuración del bot](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#bot-config)** `GamesPlayedWhileIdle`.

* * *

### Soy usuario de Linux / OS X, ¿ASF recolectará juegos que no está disponibles para sistema operativo? ¿ASF recolectará juegos de 64 bits cuanto lo estoy ejecutando en un SO de 32 bits?

Sí, ASF ni siquiera se molesta en descargar archivos de los juegos, así que funcionará con todas las licencias vinculadas a tu cuenta de Steam, independientemente de la plataforma o los requerimientos técnicos. También debería funcionar con juegos atados a una región específica (bloqueo regional) incluso si no estás en la región correspondiente, aunque no comprobamos esto.

* * *

## Comparación con herramientas similares

* * *

### ¿ASF es similar a Idle Master?

La única similitud es el propósito general de ambos programas, el cual es reposar juegos de Steam para obtener cromos. Todo lo demás, incluyendo el método de recolección, los algoritmos usados, la estructura del programa, funcionalidad, compatibilidad, terminando con el código fuente en sí mismo, es totalmente diferente y esos dos programas no tienen nada en común entre sí, incluso la base fundamental (IM se ejecuta en .NET Framework, ASF en .NET Core). ASF fue creado para resolver problemas de IM que no era posible solucionar con una simple edición de código - por eso ASF fue escrito desde cero, sin usar una sola línea de código o incluso la idea general de IM, porque ese código y esas ideas eran totalmente erróneas para empezar. IM y ASF son como Windows y Linux - ambos son sistemas operativos y ambos pueden ser instalados en tu PC, pero no comparten casi nada entre sí, además de tener un propósito similar.

Por eso no deberías comparar ASF con IM basado en las expectativas de IM. Debes tratar ASF e IM como programas totalmente independientes con su propio conjunto de características. Algunos de ellos se solapan y puedes encontrar una característica particular en ambos, pero muy raramente, puesto que ASF cumple su propósito con un enfoque totalmente diferente en comparación con IM.

* * *

### ¿Vale la pena usar ASF, si actualmente estoy usando Idle Master y funciona bien para mí?

**Sí**. ASF es mucho más confiable e incluye funciones integradas que son **cruciales** independientemente de la forma en que recolectes, que IM simplemente no ofrece.

ASF tiene la lógica adecuada para **juegos no lanzados** - IM intentará recolectar juegos que tienen cromos, incluso si no han sido lanzados. Por supuesto, no es posible recolectar esos juegos hasta la fecha de salida, así que tu proceso de recolección estará atascado. Esto requerirá que lo añadas a la lista negra, esperes su salida, o lo omitas manualmente. Ninguna de esas soluciones es buena, y todas ellas requieren tu atención - ASF omite automáticamente la recolección de juegos no han salido (temporalmente), y regresa a ellos posteriormente cuando salen, evitando completamente el problema y tratando con ello de manera eficiente.

ASF también tiene la lógica adecuada para **series** de video. Hay muchos videos en Steam que tienen cromos, pero se anuncian con una `appID` errónea en la página de insignias, tal como **[Double Fine Adventure](https://store.steampowered.com/app/402590)** - IM falsamente recolectará la `appID` equivocada, lo que no soltará ningún cromo y el proceso estará atascado. Una vez más, necesitarás añadirlo a la lista negra u omitirlo manualmente, y ambas requieren tu atención. ASF descubre automáticamente la `appID` correcta, lo que sí resulta en obtención de cromos.

Además de eso, ASF **es mucho más estable y confiable** cuando se trata de problemas de red y los caprichos de Steam - funciona la mayoría del tiempo y no requiere tu atención una vez configurado, mientras IM se rompe para muchas personas, requiere soluciones adicionales o simplemente no funciona. También depende completamente de tu cliente de Steam, lo que significa que no puede funcionar si tu cliente de Steam está experimentando problemas serios. ASF funciona correctamente mientras se pueda conectar a la red de Steam, y no quiere que el cliente de Steam esté en ejecución o siquiera que esté instalado.

Esos son 3 puntos **muy importantes** por los que deberías considerar usar ASF, ya que afectan a todos los que recolecte cromos de Steam y no forma de que digan "esto no aplica para mí", ya que los mantenimientos de Steam y sus caprichos son cosas que les pasan a todos. Hay una docena de razones menos y más importantes de las que puedes aprender en el resto de preguntas frecuentes. Hablando en breve, **sí**, deberías usar ASF incluso si no necesitas ninguna característica adicional que esté disponible en comparación con IM.

Además, IM está oficialmente descontinuado y se puede romper por completo en el futuro, sin nadie que se preocupe por arreglarlo, considerando la existencia soluciones mucho más potentes (no solo ASF). IM ya no funciona para muchas personas, y eso número solo está subiendo, no bajando. Debes evitar el uso de software obsoleto en primer lugar, no solo IM sino también todos los programas descontinuados. Sin alguien que dé mantenimiento activo significa que a nadie le importa si funciona o no, nadie verifica si lo hace y **nadie es responsable de su funcionalidad**, lo que es un asunto crucial en términos de seguridad.

* * *

### ¿Qué características interesantes ofrece ASF que Idle Master no tenga?

Depende de lo que consideres "interesante" para ti. Si planeas recolectar más de una cuenta entonces la respuesta ya es obvia, ya que ASF te permite recolectar todas ellas con una solución superior, ahorrando recursos, molestias, y problemas de compatibilidad. Sin embargo, si estás haciendo esa pregunta lo más probable es que no tengas esta necesidad particular, así que vamos a evaluar otros beneficios que aplican a una sola cuenta usada en ASF.

Primero y más importante, tienes algunas características integradas mencionadas **[arriba](#is-it-worth-it-to-use-asf-if-im-currently-using-idle-master-and-it-works-fine-for-me)** que son el núcleo de la recolección independientemente de tu meta final, y muy a menudo solo eso ya es suficiente para que consideres usar ASF. Pero ya sabes eso, así que vamos a pasar a algunas características más interesantes:

- **Puedes recolectar desconectado** (función `OnlineStatus` de `Desconectado`). Recolectar desconectado hace posible omitir completamente el estatus de juego, lo que permite recolectar con ASF mientras se muestra "En Línea" en Steam al mismo tiempo, sin que tus amigos noten siquiera que ASF está jugando por ti. Esta es una característica superior, ya que te permite permanecer en línea en tu cliente de Steam, sin molestar a tus amigos con cambios constantes de juego, o confundirlos haciéndoles creer que estás jugando cuando en realidad no es así. Este punto por sí solo hace que valga la pena usar ASF si respetas a tus amigos, pero solo es el principio. También es agradable notar que esta característica no tiene nada que ver con la configuración de privacidad de Steam - si ejecutas el juego tú mismo, entonces te mostrarás correctamente como jugando para tus amigos, haciendo invisible la parte de ASF y sin afectar tu cuenta en absoluto.

- **Puedes omitir juegos reembolsables** (función `IdleRefundableGames`). ASF tiene una lógica integrada para juegos reembolsables y puedes configurar ASF para no recolectarlos automáticamente. Esto te permite evaluar por ti mismo si tu juego recién comprado en la tienda de Steam valía tu dinero, sin que ASF trate de recolectar cromos de él lo antes posible. Si juegas por más de dos horas, o pasan 2 semanas desde la compra, entonces ASF procederá con ese juego dado que ya no es reembolsable. Hasta entonces tienes control total si lo disfrutas o no y lo puedes reembolsar fácilmente si es necesario, sin tener que añadirlo manualmente a la lista negra o no usar ASF por toda la duración.

- **Puedes marcar automáticamente nuevos artículos como recibidos** (función `BotBehaviour` de `DismissInventoryNotifications`). Recolectar con ASF tendrá como resultado que recibas nuevos cromos. Ya sabes que esto va a suceder, así que deja que ASF descarte esa notificación inútil por ti, asegurando que solo cosas importantes tomen tu atención. Por supuesto, solo si quieres.

- **Pues obtener cromos automáticamente de los eventos de Steam** (función `AutoSteamSaleEvent`). ASF te permite automatizar completar la lista de descubrimiento y votar en los Premios Steam durante las ofertas de Steam, por supuesto solo si deseas hacer eso de eso. Esto ahorra mucho tiempo cada día mientras dura la oferta de Steam, y asegura que no volverás a perder tus cromos diarios.

- **Puedes personalizar el orden de recolección con más opciones disponibles** (función `FarmingOrders`). ¿Tal vez quieres recolectar primero tus juegos recién comprados? ¿O los más antiguos? ¿De acuerdo al número de cromos obtenibles? ¿Nivel de insignia que ya fabricaste? ¿Horas jugadas? ¿Alfabéticamente? ¿De acuerdo a las AppIDs? ¿O tal vez completamente aleatorio? Eso depende completamente de ti decidirlo.

- **ASF puede ayudarte a completar tus sets** (función `TradingPreferences` con `SteamTradeMatcher`). Con unos retoques más avanzados, puedes convertir ASF en un bot que automáticamente aceptará ofertas de **[STM](https://www.steamtradematcher.com)**, ayudándote cada día a emparejar tus sets sin ninguna interacción de usuario. ASF también incluye su propio módulo ASF 2FA que te permite importar tu autenticador móvil de Steam y te deja automatizar todo el procesos de aceptar confirmaciones. O, ¿tal vez quieras aceptar manualmente y dejar que ASF solo prepare esos intercambios por ti? Eso una vez más, depende completamente de ti.

- **ASF puede activar claves en segundo plano** (función **[activador de juegos en segundo plano](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer)**). Quizá tengas cientos de claves de varios bundles que no tienes ganas de activar tú mismo, pasar por un montón de ventanas y aceptar los términos y condiciones de Steam una y otra vez. ¿Por qué no copiar y pegar tu lista de claves en ASF y dejar que haga su trabajo? ASF automáticamente activará todas esas claves en segundo plano, proporcionando la salida adecuada para hacerte saber en qué resultó cada intento de activación. Además, si tienes cientos de claves, es garantizado que tarde o temprano obtendrás el error de cuota excedida, y ASF también sabe acerca de eso, esperará pacientemente que desaparezca el error, y reanudará donde se quedó.

Podríamos seguir y seguir con toda la **[wiki de ASF](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Home)**, señalando cada característica del programa, pero tenemos que marcar una línea en alguna parte. Eso es todo, esta es una lista de características que puedes disfrutar como usuario de ASF, donde solo una de ellas fácilmente podría ser una gran razón para nunca mirar atrás, y mencionamos **muchas** de ellas, con incluso más, de las que puedes aprender en el resto de nuestra wiki. Ah sí, y ni siquiera entramos en detalles con cosas como la API de ASF que te permite escribir tus propios comandos, o una gloriosa administración de bots, pero lo queríamos mantener simple.

* * *

### ¿ASF es más rápido de Idle Master?

**Sí**, aunque la explicación es bastante complicada.

En cada nuevo proceso creado y terminado en tu sistema, el cliente de Steam automáticamente envía un solicitud que contiene todos los juegos que estás jugando actualmente - así la red de Steam puede calcular las horas y soltar cromos. Sin embargo, la red de Steam cuenta tu tiempo de juego en intervalos de 1 segundo, y enviar una nueva solicitud restablece el estado actual. En otras palabras, si creas/terminas un nuevo procesos cada 0.5 segundos, nunca obtendrás cromos porque cada 0.5 segundos el cliente de Steam enviaría una nueva solicitud y la red de Steam nunca contaría ni siquiera 1 segundo de tiempo jugado. Además, por cómo funciona el sistema operativo, es bastante común ver nuevos procesos creados/terminados sin que hagas nada, así que incluso si no estás haciendo nada en tu PC - hay muchos procesos funcionando en segundo plano, creando/terminando otros procesos todo el tiempo. Idle Master está basado en el cliente de Steam, por lo que este mecanismo te afecta si lo usas.

ASF no está basado en el cliente de Steam, tiene su propia implementación del cliente de Steam. Gracias a eso, lo que ASF hace, no es crear un proceso, sino enviar una solicitud real a la red de Steam de que empezamos a jugar un juego. Esta es la misma solicitud que sería enviada por el cliente de Steam, pero como tenemos el control sobre el cliente de Steam de ASF, no necesitamos crear nuevos procesos, y no estamos imitando al cliente de Steam en lo que se refiere al envío de solicitudes en cada cambio de proceso, por lo que el mecanismo explicado arriba no nos afecta. Gracias a eso, nunca interrumpimos ese intervalo de 1 segundo en el lado de la red de Steam, y eso mejora nuestra velocidad de recolección.

* * *

### ¿Pero la diferencia es realmente notable?

No. Las interrupciones que ocurren con el cliente normal de Steam e Idle Master tienen un efecto mínimo en la obtención de cromos, por lo que no es ninguna diferencia notable que haga superior a ASF.

Sin embargo, **sí hay** una diferencia, y puedes notar claramente que, dependiendo de qué tan ocupado esté tu sistema operativo, los cromo **caerán** más rápido, desde unos segundos hasta incluso unos minutos, si eres extremadamente desafortunado. Aunque yo no consideraría usar ASF solo porque se obtiene cromos más rápido, ya que tanto ASF como Idle Master son afectados por cómo funciona la web de Steam, ASF solo interactúa con la Steam web más eficazmente, mientras Idle Master no puede controlar lo que hace el cliente de Steam (así que no es culpa de Idle Master, sino del cliente de Steam en sí).

* * *

### ¿ASF puede recolectar múltiples juegos a la vez?

**Sí**, aunque ASF sabe mejor cuándo usar esa función, basado en el **[algoritmo de recolección de cromos](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)** seleccionado. La tasa de obtención de cromos cuando se recolectan múltiples juegos es cercana a cero, por esto ASF recolecta múltiples juegos por horas para superar `HoursUntilCardDrops` más rápido, con hasta `32` juegos a la vez. Esta es otra razón por la que debes enfocarte en la configuración de ASF, y dejar que los algoritmos decidan cuál es la forma más óptima de lograr el objetivo - lo que creas que es correcto, no lo es necesariamente en la realidad, recolectar múltiples juegos a la vez no dará ningún cromo.

* * *

### ¿ASF puede saltar entre juegos rápidamente?

**No**, ASF no soporta, ni promueve el uso de los **[glitches de Steam](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance#steam-glitches)**.

* * *

### ¿ASF puede recolectar cada juego por X horas antes de que se añadan los cromos?

**No**, el punto del cambio al sistema de cromos de Steam fue combatir las falsas estadísticas y jugadores fantasma. ASF no contribuirá a eso más de lo necesario, añadir esta característica no está planeado y no sucederá. Si tu juego recibe cromos de la forma usual, ASF lo recolectará lo antes posible.

* * *

### ¿Puedo jugar mientras ASF está recolectando?

**No**. ASF a diferencia de IM tiene incluido un cliente de Steam independiente, y la red de Steam solo permite jugar en **un cliente de Steam a la vez**. Sin embargo, puedes desconectar ASF siempre que quieras iniciando un juego (y haciendo clic en "Aceptar" cuando se te pregunta si la red de Steam debe desconectar el otro cliente) - ASF esperará pacientemente hasta que termines de jugar, y reanudará el proceso. Alternativamente, puedes jugar en modo desconectado siempre que quieras, si con eso estás satisfecho.

Ten en cuenta que la tasa de obtención de cromos al jugar múltiples juegos es cercana a 0, por lo tanto no hay beneficios directos de ser capaces de hacer eso con IM, mientras hay grandes beneficios de no interferir con otros juegos ejecutados con ASF, lo que es crucial, por ejemplo, en relación a VAC.

* * *

## Seguridad / Privacidad / VAC / Bans / ToS

* * *

### ¿Puedo obtener ban VAC por usar esto?

No, no es posible porque ASF (a diferencia de Idle Master o SAM) no interfiere de ninguna manera con el cliente de Steam ni sus procesos. Es físicamente imposible obtener ban VAC por usar ASF, incluso si juegas en servidores seguros mientras ASF se está ejecutando - esto es porque **ASF ni siquiera requiere que el Cliente de Steam esté instalado** para funcionar correctamente. ASF es el único programa de recolección que actualmente garantiza ser libre de VAC.

* * *

### ¿Usar ASF puede evitar que juegue en servidores asegurados por VAC, como se indica **[aquí](https://support.steampowered.com/kb_article.php?ref=2117-ilzv-2837)**?

ASF does not require Steam client being running or even installed at all. According to this concept, it should **not** cause any VAC-related issues, because ASF guarantees lack of interfering with Steam client and all its processes - this is the main point when talking about VAC-free guarantee that ASF offers.

According to users and best of my knowledge, this is the case right now, as nobody reported any issues like stated in the link above while using ASF. We couldn't reproduce the issue above with ASF as well, while clearly reproducing it with Idle Master.

However, keep in mind that Valve could still add ASF to the blacklist at some point, but it's a complete nonsense as even if they do that, you could still play VAC-secured games from your PC, and use ASF at the same time e.g. on your server, so I'm pretty sure that they know very well that ASF should not be a suspect VAC-wise, and they won't make our lifes harder by blacklisting ASF for no reason. Still, in the worst case you'll be unable to play, like stated above, because VAC-free guarantee of ASF is still here regardless if Steam blacklists ASF binary, or not (and you can still launch ASF on any other machine without Steam client being installed at all). Right now there is no need to do any of that, and let's hope it stays like this.

* * *

### ¿Es seguro?

Si preguntas si ASF es seguro como software, lo que significa que no causará ningún daño a tu computadora, no robará tus datos privados, instalará virus o cosas como esas - sí es seguro. ASF está libre de malware, robo de datos, minería de criptomonedas y cualquier (y todos) otro comportamiento dudoso que pueda ser considerado malicioso o no deseado por el usuario. Además de eso tenemos una sección dedicada de **[estadísticas](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Statistics)** que cubre nuestra política de privacidad y el comportamiento de ASF que va más allá de aquello para lo que configuraste el programa.

Nuestro código es abierto, y los binarios distribuidos siempre son compilados desde **[fuentes disponibles públicamente](https://en.wikipedia.org/wiki/Open-source_software)** por **[sistemas automatizados y confiables de integración continua](https://en.wikipedia.org/wiki/Build_automation)**, y ni siquiera por los desarrolladores. Cada compilación es reproducible siguiente nuestro script y resultará en exactamente el mismo, código IL (binario) **[determinista](https://en.wikipedia.org/wiki/Deterministic_system)**. Si por cualquier razón con confías en nuestra compilaciones, siempre puedes compilar y usar ASF desde la fuente, incluyendo todas las librerías que usa ASF (como SteamKit2), que también son de código abierto.

Al final, sin embargo, siempre es una cuestión de confianza en el desarrollador de tu aplicación, tú mismo debes decidir si consideras seguro a ASF, potencialmente apoyando tu decisión con argumentos técnicos especificados arriba. No creas ciegamente en algo solo porque yo lo dije - compruébalo tú mismo, ya que esa es la única forma de estar seguro.

* * *

### ¿Puedo ser baneado por esto?

Para responder esta pregunta, debemos ver de cerca los **[Términos de Servicio de Steam](https://store.steampowered.com/subscriber_agreement)**. Steam no prohíbe el uso de múltiples cuentas, de hecho, **[lo permite](https://support.steampowered.com/kb_article.php?ref=8625-WRAH-9030#share)** implicando que puedes usar el mismo autenticador móvil en más de una cuenta. Lo que no permite es compartir cuentas con otras personas, pero no estamos haciendo eso aquí.

El único punto real que considera a ASF es el siguiente:

> No está permitido utilizar trucos, software automatizado (bots), mods, aplicaciones de trampas (hacks) ni cualquier otro software de terceros no autorizado para modificar o automatizar cualquier proceso del bazar de suscripciones.

La preguntas qué es en realidad un proceso del bazar de suscripciones. Como podemos leer:

> Un ejemplo de bazar de suscripciones es el Mercado de la comunidad Steam

No estamos modificando o automatizando un proceso del bazar de suscripciones, si por bazar de suscripciones entendemos el mercado de la comunidad o la tienda de Steam. Sin embargo...

> Valve puede cancelar su cuenta o cualquier suscripción en cualquier momento en caso de que (a) Valve deje de proporcionar ese tipo de suscripción en general a suscriptores en situaciones similares o (b) usted incumpla cualquiera de los términos del presente acuerdo (incluidos los términos de suscripción y las normas de uso).

Por lo tanto, como con cada software de Steam, ASF no está autorizado por Valve y no puedo garantizar que no serás suspendido si Valve de repente decide banear las cuentas que usen ASF. Esto es excepcionalmente improbable considerando el hecho de que ASF es usado en más de medio millón de cuentas, pero sigue siendo una posibilidad, a pesar de la probabilidad real.

Especialmente porque:

> En cuanto a las suscripciones y los contenidos y servicios ajenos a Valve, Valve no filtra ese contenido de terceros disponible en Steam o a través de otras fuentes. Valve no acepta responsabilidad ni obligación alguna por el contenido de terceros. Algunas aplicaciones de software de terceros pueden utilizarse con fines comerciales; no obstante, si usted adquiere ese software a través de Steam, solo puede utilizarlo con fines privados.

Sin embargo, Valve claramente admite la existencia de los Steam idlers, como se indica **[aquí](https://support.steampowered.com/kb_article.php?ref=2117-ilzv-2837)**, si me preguntas, estoy bastante seguro de que si no estuvieran de acuerdo con ellos, ya hubieran hecho algo en lugar de señalar que podrían causar problemas relacionados con VAC. La palabra clave aquí es **Steam** "idlers", por ejemplo ASF, y no "idlers" de **juegos**.

Ten cuenta que lo anterior solo es nuestra interpretación de los Términos de Servicio de Steam así como de varios puntos - ASF está licenciado bajo Apache 2.0 License, que claramente indica:

> A menos que sea requerido por una ley aplicable o aceptado por escrito, ASF se distribuye "TAL CUAL", SIN GARANTÍAS O CONDICIONES DE NINGÚN TIPO, ya sea expresa o implícitamente.

Usas este software bajo tu propio riesgo. Es muy improbable que puedas ser baneado por eso, por si lo eres, solo puedes culparte a ti mismo.

* * *

### ¿Alguien ha sido baneado por esto?

**Sí**, hemos tenido al menos unos incidentes hasta ahora que resultaron en algún tipo de suspensión por parte de Steam. Ya sea que el propio ASF haya sido la causa principal o no, eso es otra historia que probablemente nunca llegaremos a saber.

Un caso fue el de un individuo com más de 1000 bots que obtuvo bloqueo de intercambio (junto con todos los bots), muy probablemente debido al excesivo uso de `loot ASF` ejecutado en todos los bots al mismo tiempo, u otra cantidad sospechosa de intercambios en un corto periodo de tiempo.

> Hola XXX, Gracias por contactar con el Soporte de Steam. Parece que esta cuenta fue usada para administrar una red de bots. Usar bots es una violación al Acuerdo de Suscriptor a Steam.

Por favor, usa algo de sentido común y no asumas que puedes hacer tales locuras solo porque ASF te permite hacerlo. Ejecutar `loot ASF` en más de 1000 bots fácilmente puede ser considerado como ataque **[DDoS](https://en.wikipedia.org/wiki/DDoS)**, y personalmente no me sorprende que alguien haya sido baneado por ello. Ten en cuenta el sentido común y un uso justo en lo que respecta al servicio de Steam, y *lo más probable* es que estés bien.

Otro caso fue el de un sujeto con más de 170 bots que fue baneado durante las Rebajas de Invierno de Steam 2017.

> Tu cuenta fue bloqueada por violar el Acuerdo de Suscriptor a Steam. Juzgando por los intercambios y otros factores, esta cuenta fue usada para coleccionar ilegalmente cromos en Steam, así como para actividades relacionadas y no solo comerciales. Esta cuenta ha sido bloqueada permanentemente y el Soporte de Steam no puede proporcionar soporte adicional con este problema.

Este caso es, una vez más, muy difícil de analizar, por la respuesta vaga del soporte de Steam que apenas ofrece detalles. Basado en mi opinión personal, este usuario probablemente intercambió cromos de Steam por algún tipo de dinero (¿bot para subir de nivel?) o de alguna otra forma intentó sacar dinero de Steam. En caso de que no lo sepas, esto también es ilegal de acuerdo a los Términos de Servicio de Steam.

El último caso implica a un usuario con más de 120 bots que fue baneado por incumplimiento de las **[normas de conducta online de Steam](https://store.steampowered.com/online_conduct?l=english)**.

> Hola XXX, Gracias por contactar con el Soporte de Steam. Esta y otras cuentas fueron usadas para hacer flooding en nuestra infraestructura de red, lo cual es una violación de las normas de conducta online de Steam. Esta cuenta ha sido bloqueada permanentemente y el Soporte de Steam no puede proporcionar soporte adicional con este problema.

Este caso es un poco más fácil de analizar por los detalles adicionales proporcionados por el usuario. Aparentemente el usuario estaba usando **una versión de ASF muy desactualizada** la cual incluía un bug causando que ASF envíe un número excesivo de solicitudes a los servidores de Steam. El bug en sí no existía en principio pero fue activado por un cambio en Steam que se corrigió en futuras versiones. **ASF solo tiene soporte para la **[última versión estable](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** liberada en GitHub**. El software es escrito por humanos, y los humanos tienden a cometer errores. Si el error tiene un alcance global, es solucionado rápidamente y liberado a todos los usuarios como un bugfix. Valve no baneará repentinamente a medio millón de usuarios de ASF debido a mi error, por obvias razones. Sin embargo, si intencionadamente te rehusas a utilizar un ASF actualizado, entonces por definición estás en una muy pequeña minoría de usuarios que están **expuestos a incidentes como estos** debido a **no tener soporte**, ya que nadie está vigilando tu versión desactualizada de ASF, nadie arreglándola y nadie asegurando que no serás baneado solo por ejecutarlo. **Por favor, usa software actualizado**, no solo ASF, sino también todas las demás aplicaciones.

* * *

Todos lo incidentes anteriores tienen algo en común - ASF solo es una herramienta y es **tu** decisión cómo haces uso de él. No serás baneado directamente por usar ASF, sino por **cómo** lo uses. Puede ser una herramienta útil para recolectar una sola cuenta, o una red masiva de recolección formada por miles de bots. En cualquier de esos casos, no ofrezco asesoría legal, y tú deberías decidir cómo usar ASF en primer lugar. No estoy ocultando ninguna información que podría ayudarte, por ejemplo, el hecho de que ASF hizo que baneen a algunas personas, no tengo razón para hacerlo - es tu decisión lo que quieras hacer con esa información. Si me preguntas - usa algo de sentido común, evita crear una red masiva de bots, no envíes cientos de intercambios al mismo tiempo, siempre usa una versión actualizada de ASF y *deberías* estar bien. Cada incidente de esta naturaleza por **alguna razón** siempre le ha pasado a personas ejecutando cientos de cuentas en primer lugar. Ya sea solo una coincidencia o un factor real, no depende de ti decidirlo. No estoy ofreciendo ningún consejo legal, solo manifiesto una opinión que te puede ser útil, o la puedes ignorar completamente y básate solo en los hechos relacionados anteriormente.

* * *

### ¿Qué información de privacidad divulga ASF?

Puedes encontrar una explicación detallada en la sección **[estadísticas](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Statistics)**. Deberías revisarla si te preocupa tu privacidad, por ejemplo, si te preguntas por qué las cuentas usadas en ASF se unen a nuestro grupo de Steam. ASF no recopila ninguna información sensible, y no la comparte con terceros.

* * *

## Extras

* * *

### Estoy usando un sistema operativo no soportado como Windows de 32 bits, ¿puedo usar la última versión de ASF?

Sí, y esa versión no cuenta son soporte de ningún tipo. Revisa la sección **[compatibilidad](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility)** para la variante genérica. ASF no tiene ninguna dependencia del sistema operativo, y puede funcionar en donde quiera que puedas hacer funcionar .NET Core runtime, lo que incluye Windows de 32 bits, incluso si no hay un paquete `win-x86` de sistema opera específico de nuestra parte.

* * *

### ¡ASF es genial! ¿Puedo hacer una donación?

¡Sí, y estamos muy contentos de escuchar que disfrutas nuestro proyecto! Puedes encontrar varias posibilidades de donación en cada **[lanzamiento](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** y también **[en la página principal](https://github.com/JustArchiNET/ArchiSteamFarm)**. Es bueno notar que además de donaciones de dinero genéricas también aceptamos artículos de Steam, así que nada te impide donar skins, llaves o una pequeña parte de los cromos que has recolectado con ASF si gustas. ¡Gracias de antemano por tu generosidad!

* * *

### Estoy usando un PIN del modo familiar de Steam para proteger mi cuenta, ¿necesito introducirlo en algún lugar?

Sí, debes configurarlo en la propiedad de configuración del bot `SteamParentalCode`. Esto se debe principalmente a que ASF accede a muchas partes protegidas de tu cuenta de Steam y le sería imposible funcionar sin ellas.

* * *

### No quiero que ASF recolecte ningún juego por defecto, pero quiero usar las características adicionales de ASF. ¿Es posible?

Sí, si solo quieres iniciar ASF con el módulo de recolección de cromos pausado, puedes establecer la propiedad de configuración del bot `Paused` a `true` para lograrlo. Esto te permitirá resumirlo `resume` durante el tiempo de ejecución.

Si quieres desactivar completamente el módulo de recolección de cromos y asegurarte que nunca se ejecute sin que explícitamente le digas lo contrario, entonces recomendamos establecer `IdlePriorityQueueOnly` a `true`, lo que en lugar de pausarlo, desactivará la recolección completamente hasta que añadas los juegos a la cola de prioridad de recolección.

Con el módulo de recolección de cromos pausado/deshabilitado, puedes utilizar las características adicionales de ASF, como `GamesPlayedWhileIdle`.

* * *

### ¿Se puede minimizar ASF a la bandeja del sistema?

ASF es una aplicación de consola, no hay ventana para ser minimizada, porque la ventana es creada para ti por tu sistema operativo. Sin embargo puedes usar cualquier herramienta de terceros capaz de hacer tal cosa, como **[RBTray](http://rbtray.sourceforge.net)** para Windows, o **[screen](https://linux.die.net/man/1/screen)** para Linux/OS. Estos son solo ejemplos, hay muchas otras aplicaciones con una funcionalidad similar.

* * *

### ¿Usar ASF mantiene la elegibilidad para recibir packs de refuerzo?

**Sí**. ASF usa el mismo método para iniciar sesión en la red de Steam que el cliente oficial, por lo tanto también mantiene la habilidad de recibir packs de refuerzo para las cuentas usadas. Sin embargo, no está confirmado si se requiere iniciar sesión en la comunidad de Steam, así que para estar seguros sugiero mantener `OnlineStatus` en `En Línea`, con eso basta al menos hasta que alguien confirme que usar solo la red de Steam es suficiente. Debería serlo, pero no estoy seguro.

* * *

### ¿Hay alguna forma de comunicarse con ASF?

Sí, a través del chat de Steam, o usando **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)**. Revisa la sección de **[comandos](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** para más información.

* * *

### Me gustaría ayudar con la traducción de ASF, ¿qué necesito hacer?

¡Gracias por tu interés! Puedes encontrar todos los detalles en nuestra sección de **[localización](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Localization)**.

* * *

### Solo tengo una (principal) cuenta añadida a ASF, ¿puedo enviar comandos a través del chat de Steam?

**Sí**, se explica en la sección de **[comandos](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands#notes)**. Puedes hacerlo a través de un chat de grupo de Steam, aunque usar **[ASF-ui](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#asf-ui)** podría ser más fácil para ti.

* * *

### ASF parece estar funcionando, ¡pero no estoy recibiendo ningún cromo!

Cards farming rate differs from game to game, as you can read in **[performance](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)**. It takes a while, usually **several hours per game**, and you shouldn't expect cards to drop in a few minutes since launching a program. If you can see that ASF actively checks cards status, and switches the game after current one is fully idled, then everything works fine. It's possible that you've enabled an option such as `DismissInventoryNotifications` of `BotBehaviour` which automatically dismisses inventory notifications. Check out **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** for details.

* * *

### ¿Cómo puedo detener completamente el proceso de ASF para mi cuenta?

Simplemente cierra el proceso de ASF, por ejemplo, haciendo clic en [X] en Windows. Si en cambio quieres detener un bot en particular pero mantener los demás en ejecución, entonces echa un vistazo a la **[propiedad de configuración del bot](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#bot-config)** `Enabled`, o al **[comando](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** `stop`. Si en cambio quieres detener el proceso de recolección automática, pero mantener ASF en ejecución para tu cuenta, entonces para eso es la **[propiedad de configuración del bot](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#bot-config)** `Paused` y el **[comando](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** `pause`.

* * *

### ¿Cuántos bots puedo correr con ASF?

ASF como programa no tiene un limite de instancias de bot, así que puedes ejecutar tantas como lo permita la memoria de tu máquina, sin embargo, estás limitado por la red de Steam y otros servicios de Steam. Actualmente puedes ejecutar hasta 100-200 bots con una sola IP y una sola instancia de ASF. Es posible ejecutar más bots con más IPs y más instancia de ASF, sorteando las limitaciones de IP. Ten en cuenta que si usas esa gran cantidad de bots, debes controlar sus números, asegurándote que todos ellos están, de hecho, iniciando sesión y funcionando al mismo tiempo. ASF no fue hecho para ese gran número de bots, y aplica la regla general de que **mientras más bots tengas, más problemas encontrarás**. También ten en cuenta que el límite anterior en general depende de muchos factores internos, es más una aproximación que un límite estricto - es probable que puedas ejecutar más/menos bots que los especificados arriba.

El equipo de ASF sugiere ejecutar (y **poseer**) hasta **10 bots en total**, cualquier cosa arriba de eso no tiene soporte y lo haces bajo tu propio riesgo, en contra de nuestro consejo. Esta recomendación está basada en directrices internas de Valve, así como en nuestras propias sugerencias. Ya sea que cumplas con esta regla o no, es tu decisión, ASF como herramienta no irá contra tu voluntad, incluso si resulta en la suspensión de tus cuentas de Steam. Por lo tanto, ASF mostrará una advertencia si vas por encima de lo que recomendamos, pero aún te permitirá ejecutar lo que quieras bajo tu propio riesgo y con falta de soporte.

* * *

### ¿Entonces puedo ejecutar más instancias de ASF?

You can run as many ASF instances on one machine as you like, assuming every instance has its own directory and its own configs, and account used in one instance is not used in another one. However, ask yourself why you want to do that. ASF is optimized to handle a dozen, even a hundred of accounts at the same time, and launching those dozen of bots in their own ASF instances degrades performance, takes more OS resources, and causes lack of synchronization between bots - so for example you're more likely to hit `InvalidPassword/RateLimitExceeded` issue described below, as logging in requests are not being synchronized between ASF instances.

Therefore, my **strong suggestion** is, always run maximum of one ASF instance per one IP/interface. If you have more IPs/interfaces, by all means you can run more ASF instances, every instance using its own IP/interface. If you don't, launching more ASF instances is totally pointless, and does not only degrade performance and takes more OS resources (such as memory), but also causes lack of synchronization and increased likehood of causing issues.

You won't gain anything from launching more than 1 instance per a single IP/interface. Steam will not magically allow you to run more bots just because you've launched them in another ASF instance, and ASF doesn't limit you to begin with.

* * *

### ¿Cuál es el significado de estatus al activar una clave?

Status indicates how given redeem attempt turned out. There are many different statuses possible, most common ones include:

| Estado                  | Descripción                                                                                                                                                                                                      |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| NoDetail                | El estado "OK" indica éxito - la clave fue activada exitósamente.                                                                                                                                                |
| Timeout                 | La red de Steam no respondió en un tiempo dado, no sabemos si la clave fue activada, o no (lo más probable es que sí, pero lo puedes intentar de nuevo).                                                         |
| BadActivationCode       | La clave proporcionada no es válida (no se reconoce como una clave válida por la red de Steam).                                                                                                                  |
| DuplicateActivationCode | La clave proporcionada ya fue activada por otra cuenta, o revocada por el desarrollador/editor.                                                                                                                  |
| AlreadyPurchased        | Tu cuenta ya tiene el `packageID` asociado a esta clave. Ten en cuenta que no esto no indica si la clave tiene el estado `DuplicateActivationCode` o no - solo que es válida y que no fue usada en este intento. |
| RestrictedCountry       | Esta es una clave con bloque regional y tu cuenta no está en la región válida permitida para activarla.                                                                                                          |
| DoesNotOwnRequiredApp   | No puedes activar esa clave porque te falta alguna otra aplicación - principalmente el juego base cuando estás intentando activar un paquete DLC.                                                                |
| RateLimited             | Has hecho demasiados intentos de activación y tu cuenta fue bloqueada temporalmente. Intenta de nuevo en una hora.                                                                                               |

* * *

### ¿Están afiliados con algún servicio de recolección de cromos?

**No**. ASF no está con ningún servicio y cualquier afirmación al respecto es falsa. Tu cuenta de Steam es de tu propiedad y la puedes usar del modo que quieras, pero Valve claramente indica en los **[Términos de Servicio oficiales](https://store.steampowered.com/subscriber_agreement/english)** que:

> El usuario es el responsable de la confidencialidad de su información de inicio de sesión y contraseña y de la seguridad de su equipo de cómputo. Valve no es responsable del uso que le dé a su contraseña y cuenta, así como de todas las comunicaciones y actividades realizadas en Steam que resulten del uso que le dé a su nombre de inicio de sesión y contraseña ni las que resulten del uso que les dé otra persona a la que se le hayan revelado de forma deliberada o por negligencia infringiendo esta disposición sobre la confidencialidad.

ASF is licensed on liberal Apache 2.0 License, which allows other developers to further integrate ASF with their own projects and services legally. However, such third-party projects utilizing ASF are not guaranteed to be secure, reviewed, appropriate or legal according to **[Steam ToS](https://store.steampowered.com/subscriber_agreement/english)**. If you want to know our opinion, **we strongly encourage you to NOT share ANY of your account details with third-party services**. If such service turns out to be a **typical scam**, you'll be left alone with the problem, most likely without your Steam account and ASF won't take any responsibility for third-party services claiming to be safe and secure, because ASF team did not authorize neither reviewed any of those. In other words, **you're using them at your own risk, against our suggestion made above**.

Además, los Términos de Servicio oficiales de Steam claramente señalan que:

> No debe revelar, compartir ni permitir de ningún otro modo que otros usuarios utilicen su contraseña o cuenta, excepto si Valve lo autoriza expresamente.

It's your account and your choice. Just don't say that nobody warned you. ASF as a program meets all rules mentioned above, as you're not sharing your account details with anyone, and you're using the program for your own personal use, but any other "cards farming service" does require from you your account credentials, so it also violates the rule above (actually several of them). Like with Steam ToS evaluation, we're not offering any legal advice, and you should decide yourself if you want to use those services, or not - according to us **it directly violates Steam ToS** and may result in suspension if Valve finds out. Like pointed out above, **we strongly recommend to NOT use any of such services**.

* * *

## Problemas

* * *

### ASF no detecta `X` juego como disponible para recolectar, ¡pero sé que incluye cromos de Steam!

There are two main reasons here. First and most obvious reason is the fact that you're referring to **Steam store** where given game is announced as card drops enabled game. This is **wrong** assumption, as it simply states that the game **has** card drops included, but not necessarily this function for that game is **enabled** right away. You can read more about this in **[official announcement](https://steamcommunity.com/games/593110/announcements/detail/1954971077935370845)**.

In short, card drops icon in Steam store doesn't mean anything, check your **[badge pages](https://steamcommunity.com/my/badges)** for confirmation whether a game has card drops enabled or not - this is also what ASF is doing. If your game doesn't appear on the list as a game with cards possible to drop, then this game is **not** possible to idle, regardless of reason.

Second issue is less obvious, and it's the situation when you can see that your game indeed is available with card drops on your badge page, yet it's not being idled by ASF right away. Unless you're hitting some other bug, such as ASF being unable to check badge pages (described below), it's simply a cache effect and on ASF side Steam is still reporting outdated badges page. This issue should solve itself sooner or later, when cache gets invalidated. There is also no way to fix this on our side.

Of course, all of that assumes that you're running ASF with default untouched settings, since you could also add this game to idling blacklist, use `IdlePriorityQueueOnly` of `true`, use `IdleRefundableGames` of `false` and so on.

* * *

### ¿Por qué el tiempo de juego de los juegos recolectados con ASF no aumenta?

It does, but **not in real-time**. Steam records your playtime in fixed intervals and schedules update for it, but you're not guaranteed to have it updated immediately the moment you quit the session, let alone during such. If it was possible to skip playtime while idling cards then you can be sure that we'd have it implemented in ASF long time ago, and actually use it in default settings. But we don't, and we don't exactly because it's not possible - just because the playtime isn't updated in real-time doesn't mean that it's not recorded.

* * *

### ¿Cuál es la diferencia entre una advertencia y un error en el registro?

ASF writes to its log a bunch of information on various logging levels. Our objective is to explain **precisely** what ASF is doing, including what Steam issues it has to deal with, or other problems to overcome. Most of the time not everything is relevant, this is why we have two major levels being used in ASF in terms of problems - a warning level, and error level.

General ASF rule is that warnings are **not** errors, therefore they should **not** be reported. A warning is an indicator to you that something potentially unwanted happen. Whether it was Steam not reacting, API throwing errors or your network connection being down - it's a warning, and it means we expected it to happen, so don't bother ASF development with it. Of course you're free to ask about them or get help by using our support, but you shouldn't assume that those are ASF errors worth reporting (unless we confirm otherwise).

Errors on the other hand indicate a situation that should not happen, therefore they're worth reporting as long as you made sure that it's not you who is causing them. If it's a common situation that we expect to happen, then it'll be converted to a warning instead. Otherwise, it's possibly a bug that should be corrected, not silently ignored, assuming it's not a result of your own technical issue. For example, putting invalid content in `ASF.json` file will throw an error, as ASF won't be able to parse it, but it was you who put it there, so you should not report that error to us (unless you confirmed that ASF is wrong and your structure is in fact absolutely correct).

In one TL;DR sentence - report errors, don't report warnings. You can still ask about warnings and receive help in our support sections.

* * *

### ASF no inicia, ¡la ventana del programa se cierra inmediatamente!

In normal conditions, any ASF crash or exit will generate a `log.txt` in the program's directory for you to view, which can be used for finding the cause of that.

Sin embargo, si ni siquiera el tiempo de ejecución .NET Core es capaz de arrancar en su máquina, entonces `log.txt` no se generará. If that happens to you then you most likely forgot to install .NET Core prerequisites, as stated in **[setting up](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up#os-specific-setup)** guide. Other common problems include trying to launch wrong ASF variant for your OS, or in other way missing native .NET Core runtime dependencies. If the console window closes too soon for you to read the message, then open independent console and launch ASF binary from there. For example on Windows, open ASF directory, hold `Shift`, right click inside the folder and choose "open command window here" (or powershell), then type into the console `.\ArchiSteamFarm.exe` and confirm with enter. This way you'll get precise message why ASF is not starting properly.

* * *

### ¡ASF está expulsando mi sesión en el Cliente de Steam mientras estoy jugando! / `Esta cuenta tiene iniciada una sesión en otro equipo`

This shows up as a message in Steam overlay that the account is being used somewhere else while you're playing. This issue can have two different reasons.

One reason is caused by broken packages (games) that specifically don't hold a playing lock properly, yet expect that lock to be possesed by the client. An example of such package would be Skyrim SE. Your Steam client launches the game properly, but that game doesn't register itself as being used. Because of that, ASF sees that it's free to resume the process, which it does, and that kicks you out of Steam network, as Steam suddenly detects that the account is being used in another place.

Second reason could come up if you're playing on your PC while ASF is waiting (especially on another machine) and you lose your network connection. In this case, Steam network marks you as offline and releases playing lock (like above), which triggers ASF (e.g. on another machine) into resuming farming. When your PC comes back online, Steam can't acquire playing lock anymore (that is now held by ASF, also similar to above) and shows the same message.

Both causes on the ASF side are actually very hard to workaround, as ASF simply resumes farming once Steam network informs it that account is free to be used again. This is what is happening normally when you close the game, but with broken packages this can happen immediately, even if your game is still running. ASF has no way to know whether you got disconnected, stopped playing a game or that you're still playing a game that doesn't hold playing lock appropriately.

The only proper solution to this problem is manually pausing your bot with `pause` before you start playing, and resuming it with `resume` once you're done. Alternatively you can just ignore the problem and act the same as if you played with offline Steam client.

* * *

### `¡Desconectado de Steam!` - No puedo conectarme a los servidores de Steam.

ASF can only **try** to establish connection with Steam servers, and it can fail due to many reasons, including lack of internet connection, Steam being down, your firewall blocking connection, third-party tools, incorrectly configured routes or temporary failures. You can enable `Debug` mode to check out more verbose log stating exact failure reasons, although usually it's simply caused by your own actions, such as using "CS:GO MM Server Picker" that blacklists a lot of Steam IPs, making it very hard for you to actually reach Steam network.

ASF will do its best to establish connection, which includes not only asking about updated list of servers but also trying another IP when last one fails, so if it's truly a temporary problem with some specific server or route, ASF will connect sooner or later. However, if you're behind firewall or in some other way unable to reach Steam servers, then obviously you need to fix it yourself, with potential help of `Debug` mode.

It's also possible that your machine is not able to establish connection with Steam servers using default protocol in ASF. You can alter protocols that ASF is permitted to use by modifying `SteamProtocols` global configuration property. For example, if you have problems reaching Steam with `UDP` protocol (e.g. due to firewalls), perhaps you'll have more luck with `TCP` or `WebSocket`.

In a very unlikely situation of having incorrect servers being cached, for example because of moving ASF `config` folder from one machine to another machine located in entirely different country, deleting `ASF.db` in order to refresh Steam servers on the next launch may help. Very often it's not needed and doesn't have to be done, as that list is automatically refreshed on first launch, as well as when the connection is established - we're just mentioning it as a way to purge anything related to list of Steam servers cached by ASF.

* * *

### `¡No se pudo obtener información de las insignias, volveremos a intentarlo más tarde!`

Usually it means that you're using Steam parental PIN to access your account, yet you forgot to put it in ASF config. You must put valid PIN in `SteamParentalCode` bot config property, otherwise ASF will not be able to access most of web content, therefore will not be able to work properly. Head over to **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** in order to learn more about `SteamParentalCode`.

Other reasons include temporary Steam problem, network issue or likewise. If issue won't solve itself after several hours and you're sure that you configured ASF appropriately, feel free to let us know about that.

* * *

### ¡ASF está fallando con el error `La solicitud falló después de 5 intentos`!

Usually it means that you're using Steam parental PIN to access your account, yet you forgot to put it in ASF config. You must put valid PIN in `SteamParentalCode` bot config property, otherwise ASF will not be able to access most of web content, therefore will not be able to work properly. Head over to **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** in order to learn more about `SteamParentalCode`.

If parental PIN is not the reason, then this is a most common error, and you should get used to that, it simply means that ASF sent a request to Steam Network, and didn't get a valid response, 5 times in a row. Usually it means that Steam is either down or is having some difficulties or maintenance - ASF is aware of such issues and you should not worry about them, unless they're happening constantly for longer than several hours, and other users do not have such problems.

How to check if Steam is being down? **[Steam Status](https://steamstat.us)** is an excellent source of checking if Steam **should be** up, if you notice errors, especially related to Community or Web API, then Steam is having difficulties, either leave ASF alone and let it do its job after a short while, or wait yourself.

That's however not always the case, as in some situations Steam issues may not be detected by Steam Status, for example such case happened when Valve broke HTTPS support for Steam Community 7th June 2016 - accessing **[SteamCommunity](https://steamcommunity.com)** through HTTPS was throwing an error. Therefore, do not blindly trust Steam Status either, it's best to check yourself if everything works as supposed to.

In addition to that, Steam includes various rate-limiting measures which will temporarily ban your IP if you make excessive number of requests at once. ASF is aware of that and offers you several different limiters in the config, which you should make use of. Default settings were tweaked based on **sane** amount of bots, if you're using so huge amount that even Steam is telling you to go away, then you either tweak them until it no longer tells you to, or you do as you're told. I assume second way is not an option to you, so go read on that topic and pay special attention to `WebLimiterDelay` which is a general limiter that applies to all web requests.

There is no "golden rule" that works for everybody, because blocks are heavily influenced by third-party factors, that's why you have to experiment yourself and find a value that works for you. You can also ignore what I say and use something like `10000` which is guaranteed to work correctly, but then don't complain how your ASF reacts to everything in 10 seconds and how badge parsing takes 5 minutes. In addition to that, it's entirely possible that no limiter will do anything because you have so huge amount of bots that you're hitting **[hard limit](#how-many-bots-can-i-run-with-asf)** that was mentioned above. Yes, it's entirely possible that you'll be able to log in without issues into Steam network, but Steam web will refuse to listen to you if you have 100 sessions established at once. ASF requires both Steam network and Steam web to be cooperative, it takes just one down to make you issues you won't recover from.

If nothing helps and you have no clue what is broken, you can always enable `Debug` mode and see yourself in ASF log why exactly requests are failing. Por ejemplo:

    InternalRequest() HEAD https://steamcommunity.com/my/edit/settings
    InternalRequest() Forbidden <- HEAD https://steamcommunity.com/my/edit/settings
    

See that `Forbidden` code? This means that you got temporarily banned for excessive amount of requests, because you didn't tweak `WebLimiterDelay` properly yet (assuming you get the same error code for all other requests as well). There could be other reasons listed there, such as `InternalServerError`, `ServiceUnavailable` and timeouts that indicate Steam maintenance/issues. You can always try to visit the link mentioned by ASF yourself and check if it works - if it doesn't, then you know why ASF can't access that either. If it does, and the same error doesn't go away after a day or two, it may be worth investigating and reporting.

Before doing that you should **make sure that the error is worth reporting in the first place**. If it's mentioned in this FAQ, such as trading-related issue, then that's out. If it's temporary issue that happened once or twice, especially when your network was unstable or Steam was down - that's out. However, if you were able to reproduce your issue several times in a row, across 2 days, restarted ASF as well as your machine in the process and made sure that there is no FAQ entry here to help resolve it, then this may be worth asking about.

* * *

### ¡ASF parece congelarse y no muestra nada en la consola hasta que presiono una tecla!

You're most likely using Windows and your console has QuickEdit mode enabled. Refer to **[this](https://stackoverflow.com/questions/30418886/how-and-why-does-quickedit-mode-in-command-prompt-freeze-applications)** question on StackOverflow for technical explanation. You should disable QuickEdit mode by right clicking your ASF console window, opening properties, and unchecking appropriate checkbox.

* * *

### ¡ASF no puede aceptar o enviar intercambios!

Primero lo obvio - las cuentas nuevas empiezan como limitadas. Hasta que desbloquees la cuenta cargando la cartera o gastando $5 en la tienda, ASF no puede aceptar ni enviar intercambios usando esta cuenta. En este caso, ASF declarará que el inventario está vacío, porque cada cromo en él es no intercambiable. Tampoco será posible recibir ningún intercambio, ya que eso requiere que ASF pueda obtener la clave API, y la funcionalidad de clave API está deshabilitada para las cuentas limitadas. En resumen - los intercambios están fuera para todas las cuentas limitadas, sin excepciones.

Siguiente, si no usas **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)**, es posible que ASF de hecho acepte/envíe intercambios, pero necesitas confirmarlos a través de tu correo electrónico. Del mismo modo, si usas la 2FA clásica, necesitas confirmar el intercambio a través de tu autenticador. Las confirmación son **obligatorias** ahora, si no las quieres aceptar por ti mismo, considera importar tu autenticador a ASF 2FA.

También ten en cuenta que solo puedes intercambiar con tus amigos, y personas con un enlace de intercambio conocido. Si estás intentando iniciar un intercambio Bot->Master, como `loot`, necesitas tener tu bot en tu lista de amigos, o tu `SteamTradeToken` declarado en la configuración del bot. Asegúrate de que el token sea válido - de lo contrario, no podrás enviar un intercambio.

Por último, recuerda que los nuevos dispositivos tienen una restricción de intercambio de 7 días, si recién añadiste tu cuenta a ASF, espera al menos 7 días - todo debería funcionar después de ese período. Esta limitación incluye **ambos**, aceptar **y** enviar intercambios. No siempre se activa, y hay personas que pueden enviar y aceptar intercambios instantáneamente. La mayoría de las personas son afectadas, y la restricción **sucederá**, incluso si puedes enviar y aceptar intercambios a través de tu cliente de Steam en la misma máquina. Solo espera pacientemente, no hay nada que puedas hacer para apresurarlo. Asimismo, puedes obtener una restricción similar al eliminar/cambiar varios ajustes de Steam relacionados con la seguridad, como 2FA, SteamGuard, contraseña, correo electrónico y demás. En general, comprueba si puedes enviar un intercambio desde esa cuenta tú mismo, si es así, es muy probable sea la clásica restricción de 7 días por nuevo dispositivo.

Y finalmente, ten en cuenta que una cuenta solo puede tener 5 intercambios pendientes de otra cuenta, así que ASF no enviará intercambios si ya tienes 5 (o más) pendientes para aceptar de ese bot. Esto rara vez es un problema, pero también vale la pena mencionarlo, especialmente si configuras ASF para enviar intercambios automáticamente, pero no estás usando ASF 2FA y olvidaste confirmarlos.

Si nada funcionó, siempre puedes activar el modo `Debug` y comprobar por ti mismo por qué las solicitudes están fallando. Por favor, ten en cuenta que Steam dice tonterías la mayor parte del tiempo, y la razón dada puede no tener ningún sentido lógico, o puede incluso ser incorrecto - si decides interpretar esa razón, asegúrate de tener bastante conocimiento acerca de Steam y sus caprichos. También es bastante común ver ese problema sin una razón lógica, y la única solución sugerida en este caso es volver a añadir la cuenta a ASF (y esperar 7 días de nuevo). A veces este problema también se soluciona *mágicamente*, de la misma forma que se rompe. Sin embargo, normalmente solo es la restricción de intercambio de 7 días, un problema temporal de Steam, o ambos. Es mejor darle algunos días antes de comprobar manualmente qué está mal, a menos que tengas necesidad de depurar la causa real (y normalmente serás forzado a esperar de todos modos, porque el mensaje de error no tendrá sentido, ni te ayudará en lo más mínimo).

En cualquier caso, ASF solo puede **intentar** enviar una solicitud a Steam para aceptar/enviar el intercambio. Si Steam acepta o no esa solicitud, está fuera del alcance de ASF, y ASF no lo hará funcionar mágicamente. No hay ningún bug relacionado con esa característica, y tampoco hay nada que probar, porque la lógica ocurre fuera de ASF. Por lo tanto, no pidas que se solucionen cosas que no están rotas, y tampoco preguntes por qué ASF no puede aceptar o enviar intercambios - **no lo sé, y tampoco lo sabe ASF**. Lidia con ello, o soluciónalo tú mismo.

* * *

### ¿Por qué tengo que introducir el código 2FA/SteamGuard en cada inicio de sesión? / `Eliminada la contraseña caducada`

ASF utiliza claves de acceso (si dejaste habilitado `UseLoginKeys`) para mantener credenciales válidas, el mismo mecanismo que utiliza Steam - el código 2FA/SteamGuard solo es requerido una vez. Sin embargo, debido a problemas en la red de Steam, es posible que la clave de acceso no se guarde en la red, ya he visto tales problemas no solo con ASF, sino también con el cliente regular de Steam (la necesidad de ingresar nombre de usuario + contraseña en cada ejecución, a pesar de la opción "recordarme").

Podrías eliminar `BotName.db` (+ `BotName.bin`, si existe) de la cuenta afectada e intentar vincular ASF a tu cuenta una vez más, pero eso no tiene que funcionar. La solución real basada en ASF es importar tu autenticador como **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** - de esta manera ASF puede generar códigos automáticamente cuando sea necesario, y no tienes que ingresarlos manualmente. Usualmente el problema se soluciona mágicamente después de algún tiempo, así que simplemente puedes esperar a que eso suceda. Por supuesto, también puedes pedirle una solución a GabeN, porque no puedo forzar a la red de Steam a aceptar nuestras claves de acceso.

Como nota aparte, también puedes desactivar las claves de acceso con la propiedad de configuración `UseLoginKeys` establecida a `false`, pero esto no resolverá el problema, solo omitirá el fallo inicial. ASF ya es consciente del problema descrito aquí y hará lo mejor posible para no usar claves de acceso si puede garantizar todas las credenciales de inicio de sesión, así que no hay necesidad de modificar manualmente `UseLoginKeys` si puedes proporcionar todos los detalles de inicio de sesión al usar ASF 2FA.

* * *

### Estoy teniendo el error: `No se puede iniciar sesión en Steam: InvalidPassword o RateLimitExceeded`

Esto error puede significar muchas cosas, algunas de ellas incluyen:

- Invalid Login/Password combination (obviously)
- Expired login key used by ASF for logging in
- Too many failed login attempts in short period of time (anti-bruteforce)
- Too many login attempts in short period of time (rate-limiting)
- Requirement of captcha to log in (very likely to be caused by two reasons above)
- Any other reason Steam Network may have preventing you from logging in.

In case of anti-bruteforce and rate-limiting, problem will disappear after some time, so just wait and don't attempt to log in in the meantime. If you hit that issue frequently, perhaps it's wise to increase `LoginLimiterDelay` config property of ASF. Excessive program restarts and other intentional/non-intentional login requests definitely won't help with that issue, so try to avoid it if possible.

In case of expired login key - ASF will remove old one and ask for new one on next login (which will require from you putting 2FA token if your account is 2FA-protected. If your account is using ASF 2FA, token will be generated and used automatically). If you get this issue often, it's possible that Steam for some reason decided to ignore our login key save requests, as mentioned in the issue above. You can avoid this issue by not using login keys at all with `UseLoginKeys` config property, but we do not recommend going that way.

And lastly, if you used wrong login + password combination, obviously you need to correct this, or disable bot that is attempting to connect using those credentials. ASF can't guess on its own whether `InvalidPassword` means invalid credentials, or any of the reasons listed above, therefore it'll keep trying until it succeeds.

Keep in mind that ASF has its own built-in system to react accordingly to steam quirks, eventually it will connect and resume its job, therefore it's not required to do anything if the issue is temporary. Restarting ASF in order to magically fix problems will only make things worse (as new ASF won't know previous ASF state of not being able to log in, and try to connect instead of waiting), so avoid doing that unless you know what you're doing.

Finally, as with every Steam request - ASF can only **try** to log in, using your provided credentials. Whether that request will succeed or not is out of the scope and logic of ASF - there is no bug, and nothing can be fixed neither improved in this regard.

* * *

### `System.IO.IOException: Input/output error`

If this error happened during ASF input (e.g. you can see `Console.ReadLine()` in the stacktrace) then it's caused by your environment which prohibits ASF from reading standard input of your console. That can occur due to a lot of reasons, but the most common one is you running ASF in the wrong environment (e.g. in `nohup` or `&` background instead of `screen` on Linux). If ASF can't access its standard input, then you'll see this error logged and ASF's inability to use your details during runtime.

If you **expect** this to happen, so you **intend** to run ASF in input-less environment, then you should explicitly tell ASF that it's the case, by setting **[`Headless`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#headless)** mode appropriately.

* * *

### `System.Net.Http.WinHttpException: A security error occurred`

This error happens when ASF can't establish secure connection with given server, almost exclusively because of SSL certificate mistrust.

In almost all cases this error is caused by **wrong date/time on your machine**. Every SSL certificate has issued date and expiry date. If your date is invalid and out of those two bounds then the certificate can't be trusted as potential MITM attack and ASF refuses to make a connection.

Obvious solution is to set the date on your machine appropriately. It's highly recommended to use automatic date synchronization, such as native synchronization available on Windows, or `ntpd` on Linux.

If you made sure that the date on your machine is appropriate and the error doesn't want to go away, then assuming it's not a temporary issue that should go away soon, SSL certificates that your system trusts could be out-of-date or invalid. In this case you should ensure that your machine can establish secure connections, for example by checking if you can access `https://github.com` with any browser of your choice, or CLI tool such as `curl`. If you confirmed that this works properly, feel free to post issue on our Steam group.

* * *

### `System.Threading.Tasks.TaskCanceledException: A task was canceled.`

This warning means that Steam did not answer to ASF request in given time. Usually it's caused by Steam networking hiccups and does not affect ASF in any way. In other cases it's the same as request failing after 5 tries. Reporting this issue makes no sense most of the time, as we can't force Steam to respond to our requests.

* * *

### ¡ASF está siendo detectado como malware por mi AntiVirus! ¿Qué está pasando?

**Asegúrate de que descargaste ASF de una fuente confiable**. La único fuente oficial y de confianza es la página de **[ASF releases](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** en GitHub (esta también es la fuente para las actualizaciones automáticas de ASF) - **cualquier otra fuente no es de confianza por definición y puede contener malware agregado por otras personas** - no debes confiar en ninguna otra ubicación de descarga por definición, y asegúrate de que tu ASF siempre viene de nosotros.

Si confirmas que ASF fue descargado de una fuente confiable, entonces muy probablemente solo se trate de un falso positivo. Esto **ha pasado antes**, **ocurre actualmente**, **y seguirá pasando en el futuro**. If you're worried about actual safety when using ASF, then I suggest scanning ASF with many different AVs for actual detection ratio, for example through **[VirusTotal](https://www.virustotal.com)** (or any other web service of your choice like this).

If the AV that you're using falsely detects ASF as a malware, then **it's a good idea to send this file sample back to developers of your AV, so they can analyze it and improve their detection engine**, as clearly it's not working as good as you think it does. There is no issue in ASF code, and there is also nothing to fix for us, since we're not distributing malware in the first place, therefore it doesn't make any sense to report those false-positives to us. We highly recommend to send ASF sample for further analysis like stated above, but if you don't want to bother with it, then you can always add ASF to some kind of AV exceptions, disable your AV or simply use another one. Sadly, we're used to AVs being stupid, as every once in a while some AV detects ASF as a virus, which usually lasts very short and is being patched up quickly by the devs, but like we pointed out above - **it happened**, **happens** and **will happen** all the time. ASF doesn't include any malicious code, you can review ASF code and even compile from source yourself. We're not hackers to obfuscate ASF code in order to hide from AV heuristics and false positives, so do not expect from us to fix what is not broken - there is no "virus" for us to fix.