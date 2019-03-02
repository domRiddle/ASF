# Configuración

Esta página está dedicada a la configuración de ASF. Sirve como documentación completa del directorio `config`, permitiéndote ajustar ASF a tus necesidades.

- **[Introducción](#introduction)**
- **[ConfigGenerator basado en la Web](#web-based-configgenerator)**
- **[Configuración manual](#manual-configuration)**
- **[Configuración global](#global-config)**
- **[Configuración de bot](#bot-config)**
- **[Estructura de archivos](#file-structure)**
- **[Mapeo JSON](#json-mapping)**
- **[Compatibilidad de mapeo](#compatibility-mapping)**
- **[Compatibilidad de configuraciones](#configs-compatibility)**
- **[Autorrecarga](#auto-reload)**

* * *

## Introducción

La configuración de ASF está dividida en dos partes principales - configuración global (proceso), y la configuración de cada bot. Cada bot tiene su propio archivo de configuración nombrado `NombreDeBot.json` (donde `NombreDeBot` es el nombre del bot), mientras que la configuración global de ASF (proceso) es un archivo nombrado `ASF.json`.

Un bot es una cuenta de Steam que forma parte en el proceso de ASF. Para funcionar correctamente, ASF necesita por lo menos **una** instancia de bot definida. No hay un límite impuesto de instancias de bot, así que puedes usar tantos bots (cuentas de Steam) como desees.

ASF usa el formato **[JSON](https://en.wikipedia.org/wiki/JSON)** para almacenar sus archivos de configuración. Es un formato amigable, legible y universal en el cual puedes configurar el programa. Pero no te preocupes, no necesitas saber JSON para poder configurar ASF. Solo lo mencioné en caso de que ya quieras crear en masa configuraciones de ASF con algún tipo de script bash.

La configuración puede ser hecha manualmente - creando configuraciones JSON, o usando nuestro **[ConfigGenerator basado en la web](https://justarchinet.github.io/ASF-WebConfigGenerator)**, lo cual debería ser mucho más fácil y conveniente. A menos que seas un usuario avanzado, sugiero usar el generador de configuraciones, que será descrito a continuación.

* * *

## ConfigGenerator basado en la web

El propósito del **[ConfigGenerator basado en la web](https://justarchinet.github.io/ASF-WebConfigGenerator)** es proporcionar una interfaz amigable usada para generar archivos de configuración de ASF. El ConfigGenerator basado en la web está 100% basado en el cliente, lo que significa que los detalles que introduces no son enviados a ningún lado, sino que solo son procesados localmente. Esto garantiza seguridad y confiabilidad, que incluso puede funcionar **[offline](https://github.com/JustArchiNET/ASF-WebConfigGenerator/tree/master/docs)** si quieres descargar todos los archivos y ejecutar `index.html` en tu navegador favorito.

Está verificado que el ConfigGenerator basado en la web funciona correctamente en Chrome y Firefox, pero también debería funcionar correctamente en todos los navegadores más populares que usen javascript.

El uso es bastante simple - elige si quieres generar una configuración de `ASF` o de `Bot` cambiando a la pestaña adecuada, asegúrate que la versión elegida del archivo de configuración coincida con la de ASF, luego introduce todos los detalles y haz clic en el botón "Download". Mueve este archivo al directorio `config` de ASF, sobreescribiendo los archivos existentes si es necesario. Repite para todas las posteriores modificaciones y dirígete al resto de esta sección para una explicación de todas las opciones disponibles para configurar.

* * *

## Configuración manual

Recomiendo fuertemente usar el ConfigGenerator basado en la web, pero por si alguna razón no quieres, entonces también puedes crear archivos de configuración tú mismo. Revisa los ejemplos de JSON debajo para un buen comienzo en la estructura apropiada, puedes copiar el contenido en un archivo y usarlo como base para tu configuración. Ya que no estás usando nuestra interfaz, asegúrate de que tu configuración es **[válida](https://jsonlint.com)**, ya que ASF se negará a cargarla si no puede ser analizada. Para una apropiada estructura JSON de todos los campos disponibles, dirígete a la sección **[mapeo JSON](#json-mapping)** y la documentación debajo.

* * *

## Configuración global

La configuración global se encuentra en el archivo `ASF.json` y tiene la siguiente estructura:

```json
{
    "AutoRestart": true,
    "Blacklist": [],
    "CommandPrefix": "!",
    "ConfirmationsLimiterDelay": 10,
    "ConnectionTimeout": 90,
    "CurrentCulture": null,
    "Debug": false,
    "FarmingDelay": 15,
    "GiftsLimiterDelay": 1,
    "Headless": false,
    "IdleFarmingPeriod": 8,
    "InventoryLimiterDelay": 3,
    "IPC": false,
    "IPCPassword": null,
    "LoginLimiterDelay": 10,
    "MaxFarmingTime": 10,
    "MaxTradeHoldDuration": 15,
    "OptimizationMode": 0,
    "Statistics": true,
    "SteamMessagePrefix": "/me ",
    "SteamOwnerID": 0,
    "SteamProtocols": 7,
    "UpdateChannel": 1,
    "UpdatePeriod": 24,
    "WebLimiterDelay": 300,
    "WebProxy": null,
    "WebProxyPassword": null,
    "WebProxyUsername": null
}
```

**Consejo:** A menos que quieras cambiar alguna de esas opciones, estás listo para seguir dejando todo en sus valores predeterminados, por lo tanto puedes cerrar `ASF.json` y proceder a la configuración del bot.

* * *

Todas las opciones se explican a continuación:

### `AutoRestart`

Tipo `bool` con valor predeterminado de `true`. Esta propiedad define si ASF tiene permitido realizar un autorreinicio cuando sea necesario. Hay algunos eventos que requerirán un autorreinicio de ASF, tal como una actualización de ASF (hecho con `UpdatePeriod` o el comando `update`), así como al editar el archivo de configuración `ASF.json`, con el comando `restart` y así por el estilo. Por lo general, el reinicio incluye dos partes - crear un nuevo proceso, y terminar el actual. La mayoría de los usuarios deberían estar bien así y dejar esta propiedad con el valor predeterminado de `true`, sin embargo - si estás ejecutando ASF a través de tu propio script y/o con `dotnet`, tal vez quieras tener control total sobre el proceso de inicio, y evitar una situación tal como tener un nuevo (reiniciado) proceso de ASF ejecutándose en silencio en segundo plano, y no en el primer plano del script, que terminó junto con el antiguo proceso de ASF. Esto es especialmente importante considerando el hecho de que el nuevo proceso ya no será tu "hijo" directo, lo que hará que no puedas, por ejemplo, usar la entrada de consola estándar para ello.

Si ese es el caso, esta propiedad es especialmente para ti y puedes la puedes establecer en `false`. Sin embargo, ten en cuenta que en tal caso **tú** eres responsable de reiniciar el proceso. Esto es importante de alguna manera ya que ASF solo saldrá en lugar de generar un nuevo proceso (por ejemplo, después de actualizar), así que si no hay ninguna lógica introducida por ti, simplemente dejará de funcionar hasta que lo inicies de nuevo. ASF siempre termina con el código de error apropiado indicando éxito (cero) o no éxito (no cero), de esta manera serás capaz de añadir la lógica correcta en tu script que debe evitar que ASF se autorreinicie en caso de fallo, o por lo menos hacer una copia local de `log.txt` para mayor análisis. También ten en cuenta que el comando `restart` siempre reiniciará ASF sin importar cómo esté establecida esta propiedad, puesto que esta propiedad define el comportamiento predeterminado, mientras que el comando `restart` siempre reinicia el proceso. A menos que tengas una razón para deshabilitar esta característica, debes dejarla habilitada.

* * *

### `Blacklist`

Tipo `ImmutableHashSet<uint>` con valor predeterminado estando vacío. Como su nombre lo indica, esta propiedad de configuración global define las appIDs (juegos) que serán completamente ignorados por el proceso automático de "idleo" de ASF. Desafortunadamente a Steam le encanta etiquetar las insignias de las ofertas de verano/invierno como "con cromos obtenibles", lo que confunde al proceso de ASF haciéndole creer que es un juego válido que debería ser "farmeado". Si no hubiera ningún tipo de lista negra, ASF eventualmente se "colgaría" "farmeando" un juego que en realidad no es un juego, y esperaría infinitamente para obtener cromos, lo que no va a suceder. La lista negra de ASF sirve al propósito de marcar esas insignias como no disponibles para "farmear", para que podamos ignorarlas silenciosamente al momento de decidir qué "farmear", y no caer en la trampa.

ASF incluye dos listas negras por defecto - la `GlobalBlacklist`, que está codificada en el código de ASF y no es posible editarla, y la `Blacklist` normal, que se define aquí. La `GlobalBlacklist` se actualiza junto con ASF y generalmente incluye todas las appIDs "malas" al momento de la liberación, así que si usas la versión más reciente de ASF no necesitas mantener tu propia `lista negra` definida aquí. El objetivo principal de esta propiedad es permitirte poner en la lista negra appIDs nuevas, no conocidas al momento de la liberación de ASF, que no deben ser "farmeadas". La `GlobalBlacklist` se actualiza tan rápido como es posible, por lo tanto no es necesario que actualices tu propia `Blacklist` si estás usando la última versión de ASF, pero sin la `Blacklist` te verías obligado a actualizar ASF para que pueda "seguir funcionando" cuando Valve libere una nueva insignia de ofertas - no quiero obligarte a usar el último código de ASF, por lo tanto esta propiedad está aquí para permitirte "arreglar" ASF por ti mismo si por alguna razón no quieres, o no puedes, actualizar la `GlobalBlacklist` a través de la nueva versión de ASF, pero quieres mantener funcionando tu viejo ASF. A menos que tengas una **buena** razón para editar esta propiedad, deberías dejarla en su valor predeterminado.

Si en cambio lo que buscas es una lista negra basada en los bots, echa un vistazo a los **[comandos](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** `ib`, `ibadd` e `ibrm`.

* * *

### `CommandPrefix`

Tipo `string` con valor predeterminado de `!`. Esta propiedad especifica el prefijo (que **distingue entre mayúsculas y minúsculas**) usado para los **[comandos](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** de ASF. En otras palabras, esto es lo que necesitas anteponer a cada comando de ASF para poder hacer que ASF te escuche. Es posible establecer este valor a `null` o vacío para hacer que ASF no use un comando de prefijos, en cuyo caso ingresas los comandos solo con sus identificadores. Sin embargo, haciéndolo así se disminuye potencialmente el rendimiento de ASF ya que ASF está optimizado para no analizar los mensajes si no empiezan con `CommandPrefix` - si intencionalmente decides no usarlo, ASF se verá forzado a leer todos los mensajes y responderlos, incluso si no son comandos de ASF. Por lo tanto se recomienda seguir usando algún `CommandPrefix`. tal como `/` si no te gusta el valor predeterminado de `!`. Por consistencia, `CommandPrefix` afecta a todo el proceso de ASF. A menos que tengas una razón para editar esta propiedad, deberías dejarla en su valor predeterminado.

* * *

### `ConfirmationsLimiterDelay`

Tipo `byte`con valor predeterminado de `10`. ASF se asegurará de que haya al menos `ConfirmationsLimiterDelay` segundos entre dos solicitudes de confirmación de 2FA consecutivas para evitar activar el límite de tarifa - estos son usados por **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** durante, por ejemplo, el comando `2faok`, así como cuando sea necesario al realizar varias operaciones relacionadas con intercambios. El valor predeterminado se estableció basado en nuestras pruebas y no debe ser reducido si no quieres tener problemas. A menos que tengas una **buena** razón para editar esta propiedad, deberías dejarla en su valor predeterminado.

* * *

### `ConnectionTimeout`

Tipo `byte` con valor predeterminado de `90`. Esta propiedad define el tiempo de espera para varias acciones de red realizadas por ASF, en segundos. En particular, `ConnectionTimeout` define el tiempo de espera en segundos para las solicitudes HTTP e IPC, `ConnectionTimeout / 10` define el máximo número de latidos fallidos, mientras que `ConnectionTimeout / 30` define el número de minutos que permitimos para la solicitud inicial de conexión a la red de Steam. El Valor predeterminado de `90` debería estar bien para la mayoría de las personas, sin embargo, si tienes una PC o conexión a la red lentas, tal vez quieras incrementar este número (como `120`). Ten en cuenta que valores mayores no solucionarán mágicamente unos servidores de Steam lentos o incluso inaccesibles, así que no deberíamos esperar infinitamente por algo que no sucederá y simplemente intentarlo más tarde. Establecer este valor demasiado alto dará lugar a un retraso excesivo en capturar problemas de red, así como una disminución en el rendimiento general. Establecer este valor demasiado bajo disminuirá la estabilidad y rendimiento general, ya que ASF abortará solicitudes válidas mientras todavía están siendo analizadas. Por lo tanto, establecer este valor inferior al predeterminado no tiene ninguna ventaja en general, ya que los servidores de Steam tienden a ser superlentos de vez en cuando, y pueden requerir más tiempo para analizar las solicitudes de ASF. El valor predeterminado es un balance entre creer que nuestra conexión a la red es estable, y dudar de la red de Steam para manejar nuestra solicitud en determinado tiempo de espera. Si quieres detectar problemas antes y hacer que ASF se reconecte/responda más rápido, el valor predeterminado debería bastar (o ligeramente menos, como `60`, haciendo a ASF menos paciente). Si en cambio notas que ASF está teniendo problemas de red, como solicitudes fallidas, latidos perdidos o conexión interrumpida a Steam, podría tener sentido aumentar este valor si estás seguro de que **no** está siendo causado por tu red, sino por Steam mismo, ya que aumentar los tiempos de espera hace a ASF más "paciente" y no decide reconectarse de inmediato.

Una situación de ejemplo que podría requerir un aumento de esta propiedad es permitir a ASF lidiar con un intercambio muy grande que puede tomar más de dos minutos para ser completamente aceptado y manejado por Steam. Al incrementar el tiempo de espera predeterminado, ASF será más paciente y esperará más antes de decidir que el intercambio funcionará y abandonar la solicitud inicial.

Otra situación podría ser causada por una máquina muy lenta o una conexión a internet que requiera más tiempo para manejar la información que se transmite. Esta es una situación bastante rara, ya que la CPU/ancho de banda casi nunca tiene cuello de botella, pero aún así es una posibilidad que vale la pena mencionar.

En breve, el valor predeterminado debería ser adecuado para la mayoría de los casos, pero tal vez quieras incrementarlo si es necesario. Aún así, ponerlo por encima del valor predeterminado no tiene mucho sentido, ya que tiempos de espera mayores no solucionarán mágicamente servidores de Steam inaccesibles. A menos que tengas una razón para editar esta propiedad, deberías dejarla en su valor predeterminado.

* * *

### `CurrentCulture`

Tipo `string` con valor predeterminado de `null`. Por defecto ASF intenta utilizar el idioma de tu sistema operativo, y preferirá usar las traducciones en ese idioma si están disponibles. Esto es posible gracias a nuestra comunidad que intenta **[localizar](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Localization)** ASF en casi todos los idiomas populares. Si por alguna razón no quieres usar el idioma nativo de tu SO, puedes usar esta propiedad para elegir cualquier idioma válido que quieras usar en cambio. Para una lista de todas las culturas disponibles, por favor visita **[MSDN](https://msdn.microsoft.com/en-us/library/cc233982.aspx)** y busca `Language tag`. Es bueno notar que ASF acepta tanto culturas específicas, tal como `en-GB`, pero también las neutrales, tal como `en`. Especificar una cultura también puede afectar otros comportamientos específicos de la cultura, tal como moneda/formato de fecha y así por el estilo. Por favor ten en cuenta que tal vez requieras fuentes/paquetes de idiomas adicionales para mostrar correctamente caracteres específicos de algún idioma, si elegiste alguna cultura no nativa que haga uso de ellos. Generalmente querrás hacer uso de esta propiedad si prefieres ASF en inglés en vez de tu idioma nativo.

* * *

### `Debug`

Tipo `bool` con valor predeterminado de `false`. Esta propiedad define si el proceso debe ejecutarse en modo de depuración. Estando en modo de depuración, ASF crea un directorio especial `debug` junto al `config`, que mantiene un seguimiento de toda la comunicación entre ASF y los servidores de Steam. La información de depuración puede ayudar a detectar errores relacionados con la red y el flujo de trabajo general de ASF. Demás de eso, algunas rutinas del programa serán mucho más detalladas, como `WebBrowser` especificando la razón exacta por la que algunas solicitudes fallan - esas entradas son escritas en el registro normal de ASF. **No debes ejecutar ASF in modo de depuración, a menos que te sea solicitado por el desarrollador**. Ejecutar ASF en modo de depuración **reduce el rendimiento**, **afecta negativamente la estabilidad** y es **mucho más detallado en algunas partes**, así que debe ser usado **solamente** de forma intencional, a corto plazo, para depurar un problema particular, reproducir el problema u obtener más información acerca de una solicitud fallida, y así por el estilo, pero **no** para la ejecución normal del programa. Verás **muchos** errores nuevos, problemas, y excepciones - asegúrate de tener un conocimiento decente sobre ASF, Steam y sus peculiaridades si decides analizar todo eso tú mismo, ya que no todo es relevante.

**ADVERTENCIA:** habilitar este modo incluye el registro de información **potencialmente sensible** tal como los nombres de usuario y contraseñas que utilizas para iniciar sesión en Steam (debido al registro de red). Esa información es escrita tanto en el directorio `debug`, como en el estándar `log.txt` (que ahora es mucho más detallado para registrar esta información). No debes publicar el contenido de depuración generado por ASF en ningún sitio público, el desarrollador de ASF siempre debe recordarte enviarlo a su correo, u otro sitio seguro. No almacenamos, ni hacemos uso de esos detalles sensibles, son escritos como parte de la rutina de depuración ya que su presencia puede ser relevante para el problema que te esté afectando. Preferimos que no alteres el registro de ASF en ningún modo, pero si te preocupes, eres libre de redactar apropiadamente los detalles sensibles.

> Redactar consiste en reemplazar los detalles sensibles, por ejemplo con asteriscos. Debes abstenerte de remover líneas completas, ya que su mera existencia puede ser relevante y deben ser preservadas.

* * *

### `FarmingDelay`

Tipo `byte` con valor predeterminado de `15`. A fin de que ASF funciones, comprobará el juego que actualmente se esté "farmeando" cada `FarmingDelay` minutos, por si acaso ya se han obtenido todos los cromos. Establecer esta propiedad demasiado baja puede resultar en una cantidad excesiva de solicitudes enviadas, mientras que establecerla muy alta puede resultar en que ASF siga "farmeando" determinado juego por hasta `FarmingDelay` minutos después de que haya sido "farmeado" por completo. El valor por defecto debería ser excelente para la mayoría de los usuarios, pero si tienes muchos bots ejecutándose, tal vez debas considerar incrementarlo a algo como `30` minutos para limitar las solicitudes de Steam que son enviadas. Es bueno señalar que ASF usa un mecanismo basado en eventos y comprueba la página de insignia del juego cada vez que se obtiene un artículo de Steam, para que en general **no tengamos que comprobarlo en intervalos de tiempo fijos**, pero ya que no confiamos completamente en la red de Steam - comprobamos la página de insignia del juego de todos modos, por si no lo comprobamos al obtener un cromo en los últimos `FarmingDelay` minutos (en caso de que la red de Steam no nos haya informado acerca del artículo obtenido o algo por el estilo). Suponiendo que la red de Steam está funcionando correctamente, reducir este valor **no mejorará de ningún modo la eficiencia del "farmeo"**, mientras se **incrementa significativamente la carga general de la red** - solo se recomienda (si es necesario) incrementar su valor por defecto de `15` minutos. A menos que tengas una **buena** razón para editar esta propiedad, deberías dejarla en su valor predeterminado.

* * *

### `GiftsLimiterDelay`

Tipo `byte` con valor predeterminado de `1`. ASF se asegurará de que haya por lo menos `GiftsLimiterDelay` segundos entre dos solicitudes de manejo de regalo/clave/licencia (activación) para evitar que se active el límite de tarifa. Además, también será usado como limitador global para las solicitudes de listas de juegos, como la enviada por el **[comando](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** `owns`. A menos que tengas una **buena** razón para editar esta propiedad, deberías dejarla en su valor predeterminado.

* * *

### `Headless`

Tipo `bool` con valor predeterminado de `false`. Esta propiedad define si el proceso debe ejecutarse en modo "headless". When in headless mode, ASF assumes that it's running on a server or in other non-interactive environment, therefore it will not attempt to read any information through console input. This includes on-demand details (account credentials such as 2FA code, SteamGuard code, password or any other variable required for ASF to operate) as well as all other console inputs (such as interactive command console). In other words, `Headless` mode is equal to making ASF console read-only. This setting is useful mainly for users running ASF on their servers, as instead of asking e.g. for 2FA code, ASF will silently abort the operation by stopping an account. A menos que estés ejecutando ASF en un servidor, y si previamente confirmaste que ASF es capaz de operar en modo no headless, debes dejar esta propiedad deshabilitada. Cualquier interacción del usuario será rechazada estando en modo headless, y tus cuentas no se ejecutarán si requieren **cualquier** entrada de consola durante el inicio. Esto es útil para servidores, ya que ASF puede abortar al intentar iniciar sesión cuando se le soliciten credenciales, en lugar de esperar (infinitamente) a que el usuario las proporcione. Habilitar este modo también te permitirá usar el **[comando ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** ` <0>input` que funciona como un reemplazo para la entrada de consola estándar. Si no estás seguro de cómo establecer esta propiedad, déjala con su valor predeterminado de `false`.

Si estás ejecutando ASF en un servidor, tal vez quieras usar esta opción junto con el **[argumento de línea de comandos](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments)** `--process-required`.

* * *

### `IdleFarmingPeriod`

Tipo `byte` con valor predeterminado de `8`. Cuando ASF no tiene nada para "farmear", periódicamente comprobará cada `IdleFarmingPeriod` horas si la cuenta tiene nuevos juegos para "farmear". Esta característica no es necesaria cuando hablamos de nuevos juegos que obtenemos, ya que ASF es lo suficientemente inteligente para comprobar las páginas de insignias en este caso. `IdleFarmingPeriod` es principalmente para situaciones como cuando se le añaden cromos a juegos viejos que ya tenemos. En este caso no hay ningún evento, por lo que ASF tiene que comprobar periódicamente las páginas de insignias si queremos tener esto cubierto. El valor `0` desactiva esta función. También revisa: `ShutdownOnFarmingFinished`.

* * *

### `InventoryLimiterDelay`

Tipo `byte` con valor predeterminado de `3`. ASF se asegurará de que haya por lo menos `InventoryLimiterDelay` segundos entre dos solicitudes consecutivas de inventario para evitar que se active el límite de tarifa - estos son usados para obtener el inventario de Steam, especialmente durante tus propios comandos tal como `transfer`, y también en funciones como `MatchActively`. El valor predeterminado de `3` fue establecido basado en obtener el inventario de más de 100 instancias de bot consecutivas, y debería satisfacer a la mayoría (si no a todos) los usuarios. Sin embargo tal vez quieras reducirlo, o incluso cambiarlo a `0` si tienes una poca cantidad de bots, así ASF ignorará el retraso y "looteará" los inventarios de Steam mucho más rápido. Se advierte, sin embargo, que establecerlo muy bajo **resultará** en que Steam prohíba temporalmente tu IP, y eso impedirá obtener tu inventario por completo. También podría ser necesario que incrementes este valor si estás ejecutando un montón de bots con muchas solicitudes de inventario, aunque en este caso tal vez debas intentar limitar el número de esas solicitudes. A menos que tengas una **buena** razón para editar esta propiedad, deberías dejarla en su valor predeterminado.

* * *

### `IPC`

Tipo `bool` con valor predeterminado de `false`. Esta propiedad define si el servidor **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** de ASF debe empezar junto con el proceso. El IPC permite la comunicación entre procesos al iniciar un servidor HTTP local. Si no vas a hacer uso del servidor IPC de ASF, entonces no hay razón para que habilites esta opción.

* * *

### `IPCPassword`

Tipo `string` con valor predeterminado de `null`. Esta propiedad define la contraseña obligatoria para cada llamada API hecha vía IPC y sirve como una medida de seguridad adicional. Cuando se establece un valor no vacío, todas las solicitudes IPC requerirán adicionalmente la propiedad `password` establecida a la contraseña especificada aquí. El valor predeterminado de `null` omitirá la necesidad de una contraseña, haciendo que ASF respete todas las consultas. Además de eso, activar esta opción también habilita un mecanismo integrado anti fuerza bruta que restringirá temporalmente determinada `Dirección IP` al enviar muchas solicitudes no autorizadas en un corto período de tiempo. A menos que tengas una razón para editar esta propiedad, deberías dejarla en su valor predeterminado.

* * *

### `LoginLimiterDelay`

Tipo `byte`con valor predeterminado de `10`. ASF se asegurará de que haya por lo menos `LoginLimiterDelay` segundos entre dos intentos de conexión consecutivos para evitar que se active el límite de tarifa. El valor predeterminado de `10` fue establecido basado en conectar más de 100 instancias de bot, y debería satisfacer a la mayoría (si no a todos) de usuarios. Sin embargo, tal vez o quieras aumentar/disminuir, o incluso cambiarlo a `0` si tienes una poca cantidad de bots, así ASF ignorará el retraso y se conectará a Steam mucho más rápido. Se advierte, sin embargo, que establecerla muy bajo teniendo demasiados bots **resultará** en que Steam restrinja temporalmente tu IP, y eso impedirá que inicies sesión **por completo**, con el error `InvalidPassword/RateLimitExceeded` - y eso también incluye tu cliente de Steam, no solamente ASF. Igualmente, si estás ejecutando un número excesivo de bots, especialmente junto con otros clientes/herramientas de Steam usando la misma dirección IP, probablemente necesites aumentar este valor para distribuir los inicios de sesión entre períodos de tiempo más largos.

Como nota, esto valor también es usado como un búfer balance de carga in todas las acciones de programadas de ASF, tal como intercambios en `SendTradePeriod`. A menos que tengas una **buena** razón para editar esta propiedad, deberías dejarla en su valor predeterminado.

* * *

### `MaxFarmingTime`

Tipo `byte`con valor predeterminado de `10`. Como debes saber, Steam no siempre funciona correctamente, algunas veces pueden suceder situaciones extrañas como que Steam no registre tu tiempo de juego, a pesar de en efecto jugar un juego. ASF permitirá "farmear" un juego en modo individual por un máximo de `MaxFarmingTime` horas, y lo considerará como completamente "farmeado" después de ese período. Esto es necesario para no congelar el proceso de "farmeo" in caso de que suceda alguna situación extraña, pero también si por alguna razón Steam liberara una nueva insignia que impediría que ASF siga funcionando (véase: `Blacklist`). El valor predeterminado de `10` horas debería ser suficiente para obtener todos los cromos de un juego. Establecer esta propiedad muy bajo puede resultar en que juegos válidos sean omitidos (y sí, hay juegos válidos que pueden tomar hasta 9 horas para "farmear"), mientras que establecerlo muy alto puede resultar en que se congele el proceso de "farmeo". Ten en cuenta que esta propiedad solo afecta a un juego en una sesión de "farmeo" (por lo que después de completar toda a lista ASF regresará a ese título), además no está basado en el tiempo de juego total sino en el tiempo interno de ASF, por lo que ASF también regresará a ese título después de un reinicio. A menos que tengas una **buena** razón para editar esta propiedad, deberías dejarla en su valor predeterminado.

* * *

### `MaxTradeHoldDuration`

Tipo `byte` con valor predeterminado de `15`. Esta propiedad define la duración máxima en días de la retención de intercambio que estés dispuesto a aceptar - ASF rechazará intercambios que sean retenidos por más de `MaxTradeHoldDuration` días, como se explica en la sección **[intercambios](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading)**. Esta opción tiene sentido solo para bots con la propiedad `TradingPreferences` establecida en `SteamTradeMatcher`, ya que no afecta los intercambios de `Master`/`SteamOwnerID`, ni las donaciones. Las retenciones de intercambio son molestas para todos, y realmente nadie quiere lidiar con ellas. Se supone que ASF funcione con reglas liberales y ayude a todos, sin importar si tiene retención de intercambio o no - es por eso que esta opción está establecida en `15` por defecto. Sin embargo, si en cambio prefieres rechazar todos los intercambios afectados por retenciones de intercambio, puedes especificar `0` aquí. Por favor, ten en cuenta el hecho de que los cromos con cortos períodos de vida no son afectados por esta opción y son rechazados automáticamente para las personas con retención de intercambio, como se describe en la sección **[intercambios](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading)**, así que no hay necesidad de rechazar globalmente a todos solo por eso. A menos que tengas una razón para editar esta propiedad, deberías dejarla en su valor predeterminado.

* * *

### `OptimizationMode`

Tipo `byte` con valor predeterminado de `0`. Esta propiedad define el modo de optimización que ASF preferirá durante el tiempo que esté siendo ejecutado. Actualmente ASF soporta dos modos - `0` que se llama `MaxPerformance` y `1` que se llama `MinMemoryUsage`. Por defecto ASF prefiere ejecutar tantas cosas en paralelo (concurrentemente) como sea posible, lo que mejora el rendimiento al distribuir la carga de trabajo entre todos los núcleos de la CPU, múltiples hilos de CPU, múltiples sockets y múltiples tareas de subprocesos. Por ejemplo, ASF solicitará tu primera página de insignias al comprobar si hay juegos para "idlear", y una vez que llegue la solicitud, ASF leerá cuántas páginas de insignias tienes realmente, y entonces solicitará cada una de forma concurrente. Esto es lo que deberías querer **casi siempre**, ya que la carga de trabajo en la mayoría de los casos es mínima y los beneficios del código asincrónico de ASF puede ser notado incluso en el hardware más viejo con un solo núcleo de CPU y poder altamente limitado. Sin embargo, con muchas tareas siendo procesadas en paralelo, el "runtime" de ASF es responsable de su mantenimiento, por ejemplo, mantener sockets abiertos, hilos vivos y tareas siendo procesadas, lo que puede resultar en un uso incrementado de memoria de vez en cuando, y si estás muy limitado por la memoria disponible, tal vez quieras cambiar esta propiedad a `1` (`MinMemoryUsage`) para forzar a ASF a usar tan pocas tareas como sea posible, y generalmente ejecutando código asincrónico cercano a lo paralelo de forma sincrónica. Debes considerar cambiar esta propiedad solo si previamente hasta leído la **[configuración de bajo uso de memoria](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup)** e intencionalmente quieres sacrificar una gran mejora de rendimiento, por una pequeña disminución de uso de memoria. Esta opción suele ser **mucho peor** que lo que puedes lograr con otras posibilidades, tal como limitar tu uso de ASF o ajustar el recolector de basura de runtime, como s explica en **[configuración de bajo uso de memoria](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup)**. Por lo tanto, debes usar `MinMemoryUsage` como **último recurso**, justo antes de la recompilación de runtime, si no pudiste lograr resultados satisfactorios con otras (mucho mejores) opciones. A menos que tengas una **buena** razón para editar esta propiedad, deberías dejarla en su valor predeterminado.

* * *

### `Statistics`

Tipo `bool` con valor predeterminado de `true`. Esta propiedad define si ASF debe tener las estadísticas habilitadas. Una explicación detallada de lo que esta opción hace exactamente está disponible en la sección **[estadísticas](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Statistics)**. A menos que tengas una razón para editar esta propiedad, deberías dejarla en su valor predeterminado.

* * *

### `SteamMessagePrefix`

Tipo `string` con valor predeterminado de `"/me "`. Esta propiedad define un prefijo que se antepondrá a todos los mensajes de Steam que envíe ASF. Por defecto ASF utiliza el prefijo `"/me "` para distinguir más fácilmente los mensajes de bots al mostrarlos con un color diferente en el chat de Steam. Otra cosa digna de mención es que el prefijo `"/pre "` logra resultados similares, pero usa un formato diferente. También puedes establecer esta propiedad en vacío o `null` para desactivar por completo el uso de prefijos y mostrar todos los mensajes de ASF de forma tradicional. Es bueno notar que esta propiedad solo afecta a los mensajes de Steam - las respuestas a través de otros canales (como IPC) no son afectadas. A menos que quieras personalizar el comportamiento estándar de ASF, es buena idea dejarlo en su valor predeterminado.

* * *

### `SteamOwnerID`

`ulong` type with default value of `0`. This property defines Steam ID in 64-bit form of ASF process owner, and is very similar to `Master` permission of given bot instance (but global instead). You almost always want to set this property to ID of your own main Steam account. `Master` permission includes full control over his bot instance, but global commands such as `exit`, `restart` or `update` are reserved for `SteamOwnerID` only. This is convenient, as you might want to run bots for your friends, while not allowing them to control ASF process, such as exiting it via `exit` command. Default value of `0` specifies that there is no owner of ASF process, which means that nobody will be able to issue global ASF commands. Keep in mind that **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** commands work with `SteamOwnerID`, so if you want to use them, then you must provide a valid value here.

* * *

### `SteamProtocols`

`byte flags` type with default value of `7`. This property defines Steam protocols that ASF will use when connecting to Steam servers, which are defined as below:

| Valor | Nombre    | Descripción                                                                                      |
| ----- | --------- | ------------------------------------------------------------------------------------------------ |
| 0     | Ninguno   | No protocol                                                                                      |
| 1     | TCP       | **[Transmission Control Protocol](https://en.wikipedia.org/wiki/Transmission_Control_Protocol)** |
| 2     | UDP       | **[User Datagram Protocol](https://en.wikipedia.org/wiki/User_Datagram_Protocol)**               |
| 4     | WebSocket | **[WebSocket](https://en.wikipedia.org/wiki/WebSocket)**                                         |

Please notice that this property is `flags` field, therefore it's possible to choose any combination of available values. Check out **[flags mapping](#json-mapping)** if you'd like to learn more. Not enabling any of flags results in `None` option, and that option is invalid by itself.

By default ASF should use all available Steam protocols as a measure for fighting with downtimes and other similar Steam issues. Typically you want to change this property if you want to limit ASF into using only one or two specific protocols instead of all available ones. Such measure could be needed if you're e.g. enabling only TCP traffic on your firewall and you do not want ASF to try connecting via UDP. However, unless you're debugging particular problem or issue, you almost always want to ensure that ASF is free to use any protocol that is currently supported and not just one or two. A menos que tengas una **buena** razón para editar esta propiedad, deberías dejarla en su valor predeterminado.

* * *

### `UpdateChannel`

Tipo `byte` con valor predeterminado de `1`. This property defines update channel which is being used, either for auto-updates (if `UpdatePeriod` is greater than `0`), or update notifications (otherwise). Currently ASF supports three update channels - `0` which is called `None`, `1`, which is called `Stable`, and `2`, which is called `Experimental`. `Stable` channel is the default release channel, which should be used by majority of users. `Experimental` channel in addition to `Stable` releases, also includes **pre-releases** dedicated for advanced users and other developers in order to test new features, confirm bugfixes or give feedback about planned enhancements. **Experimental versions often contain unpatched bugs, work-in-progress features or rewritten implementations**. If you don't consider yourself advanced user, please stay with default `1` (Stable) update channel. `Experimental` channel is dedicated to users who know how to report bugs, deal with issues and give feedback - no technical support will be given. Check out ASF **[release cycle](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle)** if you'd like to learn more. You can also set `UpdateChannel` to `0` (`None`), if you want to completely remove all version checks. Setting `UpdateChannel` to `0` will entirely disable entire functionality related to updates, including `update` command. Using `None` channel is **strongly discouraged** due to exposing yourself to all sort of problems (mentioned in `UpdatePeriod` description below).

**Unless you know what you're doing**, we **strongly** recommend to keep it at default.

* * *

### `UpdatePeriod`

`byte` type with default value of `24`. This property defines how often ASF should check for auto-updates. Updates are crucial not only to receive new features and critical security patches, but also to receive bugfixes, performance enhancements, stability improvements and more. When a value greater than `0` is set, ASF will automatically download, replace, and restart itself (if `AutoRestart` permits) when new update is available. In order to achieve this, ASF will check every `UpdatePeriod` hours if new update is available on our GitHub repo. A value of `0` disables auto-updates, but still allows you to execute `update` command manually. You might also be interested in setting appropriate `UpdateChannel` that `UpdatePeriod` should follow.

Update process of ASF involves update of entire folder structure that ASF is using, but without touching your own configs or databases located in `config` directory - this means that any extra files unrelated to ASF in its directory might be lost during update. Default value of `24` is a good balance between unnecessary checks, and ASF that is fresh enough.

Unless you have a **strong** reason to disable this feature, you should keep auto-updates enabled within reasonable `UpdatePeriod` **for your own good**. This is not only because we don't support anything but latest stable ASF release, but also because **we give our security guarantee only for latest version**. If you're using outdated ASF version then you're intentionally exposing yourself to all kind of issues, from small bugs, through broken functionality, ending with **[permanent Steam account suspensions](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ#did-somebody-get-banned-for-it)**, so we **strongly recommend**, for your own good, to always ensure that your ASF version is up to date. Auto-updates allow us to react quickly to all kind of issues by disabling or patching problematic code before it can escalate - if you opt out of it, you lose all of our security guarantees and risk consequences from running code that could be potentially harmful, not only to Steam network, but also (by definition) to your own Steam account.

* * *

### `WebLimiterDelay`

`ushort` type with default value of `300`. This property defines, in miliseconds, the minimum amount of delay between sending two consecutive requests to the same Steam web-service. Such delay is required as **[AkamaiGhost](https://www.akamai.com)** service that Steam uses internally includes rate-limiting based on global number of requests sent across given time period. In normal circumstances akamai block is rather hard to achieve, but under very heavy workloads with a huge ongoing queue of requests, it's possible to trigger it if we keep sending too many requests across too short time period.

Default value was set based on assumption that ASF is the only tool accessing Steam web-services, in particular `steamcommunity.com`, `api.steampowered.com` and `store.steampowered.com`. If you're using other tools sending requests to the same web-services then you should make sure that your tool includes similar functionality of `WebLimiterDelay` and set both to double of default value, which would be `600`. This guarantees that under no circumstance you'll exceed sending more than 1 request per `300` ms.

In general, lowering `WebLimiterDelay` under default value is **strongly discouraged** as it might lead to various IP-related blocks, some of which are possible to be permanent. Default value is good enough for running a single ASF instance on the server, as well as using ASF in normal scenario along with original Steam client. It should be correct for majority of usages, and you should only increase it (never lower it), if - apart from using ASF, you're also using another tool that might send excessive number of requests to the same web-services that ASF is making use of. In short, global number of all requests sent from a single IP to a single Steam domain should never exceed more than 1 request per `300` ms.

A menos que tengas una razón para editar esta propiedad, deberías dejarla en su valor predeterminado.

* * *

### `WebProxy`

Tipo `string` con valor predeterminado de `null`. This property defines a web proxy address that will be used for all internal http and https requests sent by ASF's `HttpClient`, especially to services such as `github.com`, `steamcommunity.com` and `store.steampowered.com`. Proxying ASF requests in general has no advantages, but it's exceptionally useful for bypassing various kinds of firewalls, especially the great firewall in China.

This property is defined as uri string:

> A URI string is composed of a scheme (http or https), a host, and an optional port. An example of a complete uri string is `"http://contoso.com:8080"`.

If your proxy requires user authentication, you will also need to set up `WebProxyUsername` and/or `WebProxyPassword`. If there is no such need, setting up this property alone is sufficient.

Right now ASF uses web proxy only for `http` and `https` requests, which **do not** include internal Steam network communication done within ASF's internal Steam client. There are currently no plans for supporting that, mainly due to missing **[SK2](https://github.com/SteamRE/SteamKit)** functionality. If you need/want it to happen, I'd suggest starting from there.

A menos que tengas una razón para editar esta propiedad, deberías dejarla en su valor predeterminado.

* * *

### `WebProxyPassword`

Tipo `string` con valor predeterminado de `null`. This property defines password field used in basic, digest, NTLM, and Kerberos authentication that is supported by a target `WebProxy` machine providing proxy functionality. If your proxy doesn't require user credentials, there is no need for you to input anything here. Using this option makes sense only if `WebProxy` is used as well, as it has no effect otherwise.

A menos que tengas una razón para editar esta propiedad, deberías dejarla en su valor predeterminado.

* * *

### `WebProxyUsername`

Tipo `string` con valor predeterminado de `null`. This property defines username field used in basic, digest, NTLM, and Kerberos authentication that is supported by a target `WebProxy` machine providing proxy functionality. If your proxy doesn't require user credentials, there is no need for you to input anything here. Using this option makes sense only if `WebProxy` is used as well, as it has no effect otherwise.

A menos que tengas una razón para editar esta propiedad, deberías dejarla en su valor predeterminado.

* * *

## Configuración de bot

As you should know already, every bot should have its own config based on example JSON structure below. Start from deciding how you want to name your bot (e.g. `1.json`, `main.json`, `primary.json` or `AnythingElse.json`) and head over to configuration.

**Notice:** Bot can't be named `ASF` (as that keyword is reserved for global config), ASF will also ignore all configuration files starting with a dot.

The bot config has following structure:

```json
{
    "AcceptGifts": false,
    "AutoSteamSaleEvent": false,
    "BotBehaviour": 0,
    "CustomGamePlayedWhileFarming": null,
    "CustomGamePlayedWhileIdle": null,
    "Enabled": false,
    "FarmingOrders": [],
    "GamesPlayedWhileIdle": [],
    "HoursUntilCardDrops": 3,
    "IdlePriorityQueueOnly": false,
    "IdleRefundableGames": true,
    "LootableTypes": [1, 3, 5],
    "MatchableTypes": [5],
    "OnlineStatus": 1,
    "PasswordFormat": 0,
    "Paused": false,
    "RedeemingPreferences": 0,
    "SendOnFarmingFinished": false,
    "SendTradePeriod": 0,
    "ShutdownOnFarmingFinished": false,
    "SteamLogin": null,
    "SteamMasterClanID": 0,
    "SteamParentalCode": null,
    "SteamPassword": null,
    "SteamTradeToken": null,
    "SteamUserPermissions": {},
    "TradingPreferences": 0,
    "TransferableTypes": [1, 3, 5],
    "UseLoginKeys": true
}
```

**Tip:** In order for bot to work properly, you should edit at least `Enabled`, `SteamLogin` and `SteamPassword` properties. I also suggest to take a look at some fine-tuning such as `HoursUntilCardDrops`, but all of that is optional. ASF configs are quite advanced to allow you tune your bots and ASF however you want, if you don't "require" such advanced setup, you don't really have to go deep into each config property. It's up to you how simple or how complex ASF should be.

* * *

Todas las opciones se explican a continuación:

### `AcceptGifts`

Tipo `bool` con valor predeterminado de `false`. When enabled, ASF will automatically accept and redeem all steam gifts (including wallet gift cards) sent to the bot. This includes also gifts sent from users other than those defined in `SteamUserPermissions`. Keep in mind that gifts sent to e-mail address are not directly forwarded to the client, so ASF won't accept those without your help.

This option is recommended only for alt accounts, as it's very likely that you don't want to automatically redeem all gifts sent to your primary account. If you're unsure whether you want this feature enabled or not, keep it with default value of `false`.

* * *

### `AutoSteamSaleEvent`

Tipo `bool` con valor predeterminado de `false`. During Steam summer/winter sale events Steam is known for providing you extra cards for browsing discovery queue each day, as well as through other event-specific activities. When this option is enabled, ASF will automatically check Steam discovery queue each `8` hours (starting in one hour since program start), and clear it if needed. This option is not recommended if you want to do that action yourself, and typically it should make sense only on bot accounts. Moreover, you need to ensure that your account is at least of level `8` if you expect to receive those cards in the first place, which comes directly as Steam requirement. If you're unsure whether you want this feature enabled or not, keep it with default value of `false`.

Please note that due to constant Valve issues, changes and problems, **we give no guarantee whether this function will work properly**, therefore it's entirely possible that this option **will not work at all**. We do not accept **any** bug reports, neither support requests for this option. It's offered with absolutely no guarantees, you're using it at your own risk.

* * *

### `BotBehaviour`

`byte flags` type with default value of `0`. This property defines ASF bot-like behaviour during various events, and is defined as below:

| Valor | Nombre                        | Descripción                                                           |
| ----- | ----------------------------- | --------------------------------------------------------------------- |
| 0     | Ninguno                       | No special bot behaviour, the least invasive mode, default            |
| 1     | RejectInvalidFriendInvites    | Will cause ASF to reject (instead of ignoring) invalid friend invites |
| 2     | RejectInvalidTrades           | Will cause ASF to reject (instead of ignoring) invalid trade offers   |
| 4     | RejectInvalidGroupInvites     | Will cause ASF to reject (instead of ignoring) invalid group invites  |
| 8     | DismissInventoryNotifications | Will cause ASF to automatically dismiss all inventory notifications   |
| 16    | MarkReceivedMessagesAsRead    | Will cause ASF to automatically mark all received messages as read    |

Please notice that this property is `flags` field, therefore it's possible to choose any combination of available values. Check out **[flags mapping](#json-mapping)** if you'd like to learn more. Not enabling any of flags results in `None` option.

In general you want to modify this property if you expect from ASF to do certain amount of automation related to its activity, as it'd be expected from a bot account, but not a primary account used in ASF. Therefore, changing this property makes sense mainly for alt accounts, although you're free to use selected options for main accounts as well.

Normal (`None`) ASF behaviour is to only automate things that user wants (e.g. cards farming or `SteamTradeMatcher` offers, if set in `TradingPreferences`). This is the least invasive mode, and it's beneficial to majority of users since you remain in full control over your account and you can decide yourself whether to allow certain out-of-scope interactions, or not.

Invalid friend invite is an invite that doesn't come from user with `FamilySharing` permission (defined in `SteamUserPermissions`) or above. ASF in normal mode ignores those invites, as you'd expect, giving you free choice whether to accept them, or not. `RejectInvalidFriendInvites` will cause those invites to be automatically rejected, which will practically disable option for other people to add you to their friend list (as ASF will deny all such requests, apart from people defined in `SteamUserPermissions`). Unless you want to outright deny all friend invites, you shouldn't enable this option.

Invalid trade offer is an offer that isn't accepted through built-in ASF module. More on this matter can be found in **[trading](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading)** section which explicitly defines what types of trade ASF is willing to accept automatically. Valid trades are also defined by other settings, especially `TradingPreferences`. `RejectInvalidTrades` will cause all invalid trade offers to be rejected, instead of being ignored. Unless you want to outright deny all trade offers that aren't automatically accepted by ASF, you shouldn't enable this option.

Invalid group invite is an invite that doesn't come from `SteamMasterClanID` group. ASF in normal mode ignores those group invites, as you'd expect, allowing you to decide yourself if you want to join particular Steam group or not. `RejectInvalidGroupInvites` will cause all those group invites to be automatically rejected, effectively making it impossible to invite you to any other group than `SteamMasterClanID`. Unless you want to outright deny all group invites, you shouldn't enable this option.

`DismissInventoryNotifications` is extremely useful when you start getting annoyed by contact Steam notification about receiving new items. ASF can't get rid of the notification itself, as that's built-in into your Steam client, but it's able to automatically clear the notification after receiving it, which will no longer leave "new items in inventory" notification hanging around. If you expect to evaluate yourself all received items (especially cards idled with ASF), then naturally you shouldn't enable this option. When you start going crazy, remember this is offered as an option.

`MarkReceivedMessagesAsRead` will automatically mark **all** messages being received by the account on which ASF is running. This typically should be used by alt accounts only in order to clear "new message" notification coming e.g. from you during executing ASF commands. We do not recommend this option for primary accounts, unless you want to cut yourself from any kind of new messages notifications, **including** those that happened while you were offline, assuming that ASF was still left open dismissing it.

If you're unsure how to configure this option, it's best to leave it at default.

* * *

### `CustomGamePlayedWhileFarming`

Tipo `string` con valor predeterminado de `null`. When ASF is farming, it can display itself as "Playing non-steam game: `CustomGamePlayedWhileFarming`" instead of currently farmed game. This can be useful if you would like to let your friends know that you're farming, yet you don't want to use `OnlineStatus` of `Offline`. Please note that ASF cannot guarantee the actual display order of Steam network, therefore this is only a suggestion that may, or may not, display properly. Default value of `null` disables this feature.

* * *

### `CustomGamePlayedWhileIdle`

Tipo `string` con valor predeterminado de `null`. Similar to `CustomGamePlayedWhileFarming`, but for the situation when ASF has nothing to do (as account is fully farmed). Default value of `null` disables this feature.

* * *

### `Enabled`

Tipo `bool` con valor predeterminado de `false`. This property defines if bot is enabled. Enabled bot instance (`true`) will automatically start on ASF run, while disabled bot instance (`false`) will need to be started manually. By default every bot is disabled, so you probably want to switch this property to `true` for all of your bots that should be started automatically.

* * *

### `FarmingOrders`

Tipo `ImmutableHashSet<byte>` con valor predeterminado estando vacío. This property defines the **preferred** farming order used by ASF for given bot account. Currently there are following farming orders available:

| Valor | Nombre                    | Descripción                                                                      |
| ----- | ------------------------- | -------------------------------------------------------------------------------- |
| 0     | Unordered                 | No sorting, slightly improving CPU performance                                   |
| 1     | AppIDsAscending           | Try to farm games with lowest `appIDs` first                                     |
| 2     | AppIDsDescending          | Try to farm games with highest `appIDs` first                                    |
| 3     | CardDropsAscending        | Try to farm games with lowest number of card drops remaining first               |
| 4     | CardDropsDescending       | Try to farm games with highest number of card drops remaining first              |
| 5     | HoursAscending            | Try to farm games with lowest number of hours played first                       |
| 6     | HoursDescending           | Try to farm games with highest number of hours played first                      |
| 7     | NamesAscending            | Try to farm games in alphabetical order, starting from A                         |
| 8     | NamesDescending           | Try to farm games in reverse alphabetical order, starting from Z                 |
| 9     | Random                    | Try to farm games in totally random order (different on each run of the program) |
| 10    | BadgeLevelsAscending      | Try to farm games with lowest badge levels first                                 |
| 11    | BadgeLevelsDescending     | Try to farm games with highest badge levels first                                |
| 12    | RedeemDateTimesAscending  | Try to farm oldest games on our account first                                    |
| 13    | RedeemDateTimesDescending | Try to farm newest games on our account first                                    |
| 14    | MarketableAscending       | Try to farm games with unmarketable card drops first                             |
| 15    | MarketableDescending      | Try to farm games with marketable card drops first                               |

Since this property is an array, it allows you to use several different settings in your fixed order. For example, you can include values of `15`, `11` and `7` in order to sort by marketable games first, then by those with highest badge level, and finally alphabetically. As you can guess, the order actually matters, as reverse one (`7`, `11` and `15`) achieves something entirely different. Majority of people will probably use just one order out of all of them, but in case you want to, you can also sort further by extra parameters.

Also notice the word "try" in all above descriptions - the actual ASF order is heavily affected by selected **[cards farming algorithm](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)** and sorting will affect only results that ASF considers same performance-wise. For example, in `Simple` algorithm, selected `FarmingOrders` should be entirely respected in current farming session (as every game has the same performance value), while in `Complex` algorithm actual order is affected by hours first, and then sorted according to chosen `FarmingOrders`. This will lead to different results, as games with existing playtime will have a priority over others, so effectively ASF will prefer games that already passed required `HoursUntilCardDrops` firstly, and only then sorting those games further by your chosen `FarmingOrders`. Likewise, once ASF runs out of already-bumped games, it'll sort remaining queue by hours first (as that will decrease time required for bumping any of remaining titles to `HoursUntilCardDrops`). Therefore, this config property is only a **suggestion** that ASF will try to respect, as long as it doesn't affect performance negatively (in this case, ASF will always prefer idling performance over `FarmingOrders`).

There is also idling priority queue that is accessible through `iq` **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. If it's used, actual idling order is sorted firstly by performance, then by idling queue, and finally by your `FarmingOrders`.

* * *

### `GamesPlayedWhileIdle`

Tipo `ImmutableHashSet<uint>` con valor predeterminado estando vacío. If ASF has nothing to farm it can play your specified steam games (`appIDs`) instead. Playing games in such manner increases your "hours played" of those games, but nothing else apart of it. This feature can be enabled at the same time with `CustomGamePlayedWhileIdle` in order to play your selected games while showing custom status in Steam network, but in this case, like in `CustomGamePlayedWhileFarming` case, the actual display order is not guaranteed. Please note that Steam allows ASF to play only up to `32` `appIDs` total, therefore you can put only as many games in this property.

* * *

### `HoursUntilCardDrops`

Tipo `byte` con valor predeterminado de `3`. This property defines if account has card drops restricted, and if yes, for how many initial hours. Restricted card drops means that account is not receiving any card drops from given game until the game is played for at least `HoursUntilCardDrops` hours. Unfortunately there is no magical way to detect that, so ASF relies on you. This property affects **[cards farming algorithm](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)** that will be used. Setting this property properly will maximize profits and minimize time required for cards to be farmed. Remember that there is no obvious answer whether you should use one or another value, since it fully depends on your account. It seems that older accounts which never asked for refund have unrestricted card drops, so they should use a value of `0`, while new accounts and those who did ask for refund have restricted card drops with a value of `3`. This is however only theory, and should not be taken as a rule. The default value for this property was set based on "lesser evil" and majority of use cases.

* * *

### `IdlePriorityQueueOnly`

Tipo `bool` con valor predeterminado de `false`. This property defines if ASF should consider for automatic idling only apps that you added yourself to priority idling queue available with `iq` **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. When this option is enabled, ASF will skip all `appIDs` that are missing on the list, effectively allowing you to cherry-pick games for automatic ASF idling. Keep in mind that if you didn't add any games to the queue then ASF will act as if there is nothing to idle on your account. If you're unsure whether you want this feature enabled or not, keep it with default value of `false`.

* * *

### `IdleRefundableGames`

Tipo `bool` con valor predeterminado de `true`. This property defines if ASF is permitted to idle games that are still refundable. A refundable game is a game that you bought in last 2 weeks through Steam Store and didn't play for longer than 2 hours yet, as stated on **[Steam refunds](https://store.steampowered.com/steam_refunds)** page. By default when this option is set to `true`, ASF ignores Steam refunds policy entirely and idles everything, as most people would expect. However, you can change this option to `false` if you want to ensure that ASF won't idle any of your refundable games too soon, allowing you to evaluate those games yourself and refund if needed without worrying about ASF affecting playtime negatively. Please note that if you disable this option then games you purchased from Steam Store won't be idled by ASF for up to 14 days since redeem date, which will show as nothing to idle if your account doesn't own anything else. If you're unsure whether you want this feature enabled or not, keep it with default value of `true`.

* * *

### `LootableTypes`

`ImmutableHashSet<byte>` type with default value of `1, 3, 5` steam item types. This property defines ASF behaviour when looting - both manual and automatic. ASF will ensure that only items from `LootableTypes` will be included in a trade offer, therefore this property allows you to choose what you want to receive in a trade offer that is being sent to you.

| Valor | Nombre            | Descripción                                                   |
| ----- | ----------------- | ------------------------------------------------------------- |
| 0     | Unknown           | Every type that doesn't fit in any of the below               |
| 1     | BoosterPack       | Booster pack containing 3 random cards from a game            |
| 2     | Emoticon          | Emoticon to use in Steam Chat                                 |
| 3     | FoilTradingCard   | Foil variant of `TradingCard`                                 |
| 4     | ProfileBackground | Profile background to use on your Steam profile               |
| 5     | TradingCard       | Steam trading card, being used for crafting badges (non-foil) |
| 6     | SteamGems         | Steam gems being used for crafting boosters, sacks included   |
| 7     | SaleItem          | Special items awarded during Steam sales                      |
| 8     | Consumable        | Special consumable items that disappear after being used      |
| 9     | ProfileModifier   | Special items that can modify Steam profile appearance        |

Please note that regardless of the settings above, ASF will only ask for Steam (`appID` of 753) community (`contextID` of 6) items, so all game items, gifts and likewise, are excluded from the trade offer by definition.

Default ASF setting is based on most common usage of the bot, with looting only booster packs, and trading cards (including foils). The property defined here allows you to alter that behaviour in whatever way that satisfies you. Please keep in mind that all types not defined above will show as `Unknown` type, which is especially important when Valve releases some new Steam item, that will be marked as `Unknown` by ASF as well, until it's added here (in the future release). That's why in general it's not recommended to include `Unknown` type in your `LootableTypes`, unless you know what you're doing, and you also understand that ASF will send your entire inventory in a trade offer if Steam Network gets broken again and reports all your items as `Unknown`. My strong suggestion is to not include `Unknown` type in the `LootableTypes`, even if you expect to loot everything.

* * *

### `MatchableTypes`

`ImmutableHashSet<byte>` type with default value of `5` Steam item types. This property defines which Steam item types are permitted to be matched when `SteamTradeMatcher` option in `TradingPreferences` is enabled. Types are defined as below:

| Valor | Nombre            | Descripción                                                   |
| ----- | ----------------- | ------------------------------------------------------------- |
| 0     | Unknown           | Every type that doesn't fit in any of the below               |
| 1     | BoosterPack       | Booster pack containing 3 random cards from a game            |
| 2     | Emoticon          | Emoticon to use in Steam Chat                                 |
| 3     | FoilTradingCard   | Foil variant of `TradingCard`                                 |
| 4     | ProfileBackground | Profile background to use on your Steam profile               |
| 5     | TradingCard       | Steam trading card, being used for crafting badges (non-foil) |
| 6     | SteamGems         | Steam gems being used for crafting boosters, sacks included   |
| 7     | SaleItem          | Special items awarded during Steam sales                      |
| 8     | Consumable        | Special consumable items that disappear after being used      |
| 9     | ProfileModifier   | Special items that can modify Steam profile appearance        |

Of course, types that you should use for this property typically include only `2`, `3`, `4` and `5`, as only those types are supported by STM. ASF includes proper logic for discovering rarity of the items, therefore it's also safe to match emoticons or backgrounds, as ASF will properly consider fair only those items from the same game and type, that also share the same rarity.

Please note that **ASF is not a trading bot** and **will NOT care about the market price**. If you don't consider items of the same rarity from the same set to be the same price-wise, then this option is NOT for you. Please evaluate twice if you understand and agree with this statement before you decide to change this setting.

Unless you know what you're doing, you should keep it with default value of `5`.

* * *

### `OnlineStatus`

Tipo `byte` con valor predeterminado de `1`. This property specifies Steam community status that the bot will be announced with after logging in to Steam network. Currently you can choose one of below statuses:

| Valor | Nombre         |
| ----- | -------------- |
| 0     | Desconectado   |
| 1     | En línea       |
| 2     | Busy           |
| 3     | Away           |
| 4     | Snooze         |
| 5     | LookingToTrade |
| 6     | LookingToPlay  |
| 7     | Invisible      |

`Offline` status is extremely useful for primary accounts. As you should know, farming a game actually shows your steam status as "Playing game: XXX", which can be misleading to your friends, confusing them that you're playing a game while actually you're only farming it. Using `Offline` status solves that issue - your account will never be shown as "in-game" when you're farming steam cards with ASF. This is possible thanks to the fact that ASF does not have to sign in into Steam Community in order to work properly, so we're in fact playing those games, connected to Steam network, but without announcing our online presence at all. Keep in mind that played games using offline status will still count towards your playtime, and show as "recently played" on your profile.

In addition to that, this feature is also important if you want to receive notifications and unread messages when ASF is running, while not keeping Steam client open at the same time. This is because ASF acts as a Steam client itself, and whether ASF would like it or not, Steam broadcasts all those messages and other events to it. This is not a problem if you have both ASF and your own Steam client running, as both clients receive exactly the same events. However, if just ASF is running, Steam network could mark certain events and messages as "delivered", despite of your traditional Steam client not receiving it due to not being present. Offline status also solves this problem, as ASF is never considered for any community events in this case, so all unread messages and other events will be properly marked as unread when you come back.

It's important to note that ASF running on `Offline` mode will **not** be able to receive commands in usual Steam chat way, as the chat, as well as entire community presence is in fact, entirely offline. A solution to this issue is using `Invisible` mode instead which works in a similar way (not exposing status), but keeps the ability to receive and respond to messages (so also a potential to dismiss notifications and unread messages as stated above). `Invisible` mode makes the most sense on alt accounts that you don't want to expose (status-wise), but still be able to send commands to.

However, there is one catch with `Invisible` mode - it doesn't go well with primary accounts. This is because **any** Steam session that is currently online **exposes** the status, even if ASF itself does not. This is caused by the current limitation/bug of the Steam network that isn't possible to be fixed on ASF side, so if you want to use `Invisible` mode you will also need to ensure that **all** other sessions to the same account use `Invisible` mode as well. This will be the case on alt accounts where ASF is hopefully the only active session, but on primary accounts you'll almost always prefer to show as `Online` to your friends, hiding only ASF activity, and in this case `Invisible` mode will be entirely useless for you (we recommend to use `Offline` mode instead). Hopefully this limitation/bug will be eventually solved in the future by Valve, but I wouldn't expect that to happen anytime soon...

If you're unsure how to set up this property, it's recommended to use a value of `0` (`Offline`) for primary accounts, and default `1` (`Online`) otherwise.

* * *

### `PasswordFormat`

Tipo `byte` con valor predeterminado de `0`. This property defines the format of `SteamPassword` property, and currently supports - `0` for `PlainText`, `1` for `AES` and `2` for `ProtectedDataForCurrentUser`. Please refer to **[Security](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security)** section if you want to learn more, as you'll need to ensure that `SteamPassword` property indeed includes password in matching `PasswordFormat`. In other words, when you change `PasswordFormat` then your `SteamPassword` should be **already** in that format, not just aiming to be. Unless you know what you're doing, you should keep it with default value of `0`.

* * *

### `Paused`

Tipo `bool` con valor predeterminado de `false`. This property defines initial state of `CardsFarmer` module. With default value of `false`, bot will automatically start farming when it's started, either because of `Enabled` or `start` command. Switching this property to `true` should be done only if you want to manually `resume` automatic farming process, for example because you want to use `play` all the time and never use automatic `CardsFarmer` module - this works exactly the same as `pause` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. If you're unsure whether you want this feature enabled or not, keep it with default value of `false`.

* * *

### `RedeemingPreferences`

`byte flags` type with default value of `0`. This property defines ASF behaviour when redeeming cd-keys, and is defined as below:

| Valor | Nombre           | Descripción                                                                    |
| ----- | ---------------- | ------------------------------------------------------------------------------ |
| 0     | Ninguno          | No redeeming preferences, typical                                              |
| 1     | Forwarding       | Forward keys unavailable to redeem to other bots                               |
| 2     | Distributing     | Distribute all keys among itself and other bots                                |
| 4     | KeepMissingGames | Keep keys for (potentially) missing games when forwarding, leaving them unused |

Please notice that this property is `flags` field, therefore it's possible to choose any combination of available values. Check out **[flags mapping](#json-mapping)** if you'd like to learn more. Not enabling any of flags results in `None` option.

`Forwarding` will cause bot to forward a key that is not possible to redeem, to another connected and logged on bot that is missing that particular game (if possible to check). The most common situation is forwarding `AlreadyPurchased` game to another bot that is missing that particular game, but this option also covers other scenarios, such as `DoesNotOwnRequiredApp`, `RateLimited` or `RestrictedCountry`.

`Distributing` will cause bot to distribute all received keys among itself and other bots. This means that every bot will get a single key from the batch. Typically this is used only when you're redeeming many keys for the same game, and you want to evenly distribute them among your bots, as opposed to redeeming keys for various different games. This feature makes no sense if you're redeeming only one key in a single `redeem` action (as there are no extra keys to be distributed).

`KeepMissingGames` will cause bot to skip `Forwarding` when we can't be sure if key being redeemed is in fact owned by our bot, or not. This basically means that `Forwarding` will apply **only** to `AlreadyPurchased` keys, instead of covering also other cases such as `DoesNotOwnRequiredApp`, `RateLimited` or `RestrictedCountry`. Typically you might want to use this option on primary account, to ensure that keys being redeemed on it won't be forwarded further if your bot for example becomes temporarily `RateLimited`. As you can guess from the description, this field has absolutely no effect if `Forwarding` is not enabled.

Enabling both `Forwarding` and `Distributing` will add distributing feature on top of forwarding one, which makes ASF trying to redeem one key on all bots firstly (forwarding) before moving to the next one (distributing). Typically you want to use this option only when you want `Forwarding`, but with altered behaviour of switching the bot on key being used, instead of always going in-order with every key (which would be `Forwarding` alone). This behaviour can be beneficial if you know that majority or even all of your keys are tied to the same game, because in this situation `Forwarding` alone would try to redeem everything on one bot firstly (which makes sense if your keys are for unique games), and `Forwarding` + `Distributing` will switch the bot on the next key, "distributing" the task of redeeming new key onto another bot than the initial one (which makes sense if keys are for the same game, skipping one pointless attempt per key).

The actual bots order for all of the redeeming scenarios is alphabetical, excluding bots that are unavailable (not connected, stopped or likewise). Please keep in mind that there is per-IP and per-account hourly limit of redeeming tries, and every redeem try that didn't end with `OK` contributes to failed tries. ASF will do its best to minimize number of `AlreadyPurchased` failures, e.g. by trying to avoid forwarding a key to another bot that already owns that particular game, but it's not always guaranteed to work due to how Steam is handling licenses. Using redeeming flags such as `Forwarding` or `Distributing` will always increase your likelyhood to hit `RateLimited`.

Also keep in mind that you can't forward or distribute keys to bots that you do not have access to. This should be obvious, but ensure that you're at least `Operator` of all the bots you want to include in your redeeming process, for example with `status ASF` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**.

* * *

### `SendOnFarmingFinished`

Tipo `bool` con valor predeterminado de `false`. When ASF is done with farming given account, it can automatically send steam trade containing everything farmed up to this point to user with `Master` permission, which is very convenient if you don't want to bother with trades yourself. This option works the same as `loot` command, therefore keep in mind that it requires user with `Master` permission set, you might also require valid `SteamTradeToken`, including using an account that is actually eligible for trading. In addition to initiating `loot` after finishing farming, ASF will also initiate `loot` on each new items notification (when not farming), and after completing each trade that results in new items (always) when this option is active. This is especially useful for "forwarding" items received from other people to our account.

Typically you'll want to use **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** together with this feature, although it's not a requirement if you intend to confirm manually in timely fashion. Si no estás seguro de cómo establecer esta propiedad, déjala con su valor predeterminado de `false`.

* * *

### `SendTradePeriod`

Tipo `byte` con valor predeterminado de `0`. This property works very similar to `SendOnFarmingFinished` property, with one difference - instead of sending trade when farming is done, we can also send it every `SendTradePeriod` hours, regardless of how much we have to farm left. This is useful if you want to `loot` your alt accounts on usual basis instead of waiting for it to finish farming. Default value of `0` disables this feature, if you want your bot to send you trade e.g. every day, you should put `24` here.

Typically you'll want to use **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** together with this feature, although it's not a requirement if you intend to confirm manually in timely fashion. If you're not sure how to set this property, leave it with default value of `0`.

* * *

### `ShutdownOnFarmingFinished`

Tipo `bool` con valor predeterminado de `false`. ASF is "occupying" an account for the whole time of process being active. When given account is done with farming, ASF periodically checks it (every `IdleFarmingPeriod` hours), if perhaps some new games with steam cards were added in the meantime, so it can resume farming of that account without a need to restart the process. This is useful for majority of people, as ASF can automatically resume farming when needed. However, you may actually want to stop the process when given account is fully farmed, you can achieve that by setting this property to `true`. When enabled, ASF will proceed with logging off when account is fully farmed, which means that it won't be periodically checked or occupied anymore. You should decide yourself if you prefer ASF to work on given bot instance for the whole time, or if perhaps ASF should stop it when farming process is done. When all accounts are stopped and process is not running in `--process-required` **[mode](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-Line-Arguments)**, ASF will shutdown as well, putting your machine at rest and allowing you to schedule other actions, such as sleep or shutdown at the same moment of last card dropping.

Si no estás seguro de cómo establecer esta propiedad, déjala con su valor predeterminado de `false`.

* * *

### `SteamLogin`

Tipo `string` con valor predeterminado de `null`. This property defines your steam login - the one you use for logging in to steam. In addition to defining steam login here, you may also keep default value of `null` if you want to enter your steam login on each ASF startup instead of putting it in the config. This may be useful for you if you don't want to save sensitive data in config file.

* * *

### `SteamMasterClanID`

`ulong` type with default value of `0`. This property defines the steamID of the steam group that bot should automatically join, including its group chat. You can check steamID of your group by navigating to its **[page](https://steamcommunity.com/groups/archiasf)**, then adding `/memberslistxml?xml=1` to the end of the link, so the link will look like **[this](https://steamcommunity.com/groups/archiasf/memberslistxml?xml=1)**. Then you can get steamID of your group from the result, it's in `<groupID64>` tag. In above example it would be `103582791440160998`. In addition to trying to join given group at startup, the bot will also automatically accept group invites to this group, which makes it possible for you to invite your bot manually if your group has private membership. If you don't have any group dedicated for your bots, you should keep this property with default value of `0`.

* * *

### `SteamParentalCode`

Tipo `string` con valor predeterminado de `null`. This property defines your steam parental PIN. ASF requires an access to resources protected by steam parental, therefore if you use that feature, you need to provide ASF with parental unlock PIN, so it can operate normally. Default value of `null` means that there is no steam parental PIN required to unlock this account, and this is probably what you want if you don't use steam parental functionality. In addition to defining steam parental PIN here, you may also use value of `0` if you want to enter your steam parental PIN on each ASF startup instead of putting it in the config. This may be useful for you if you don't want to save sensitive data in config file.

* * *

### `SteamPassword`

Tipo `string` con valor predeterminado de `null`. This property defines your steam password - the one you use for logging in to steam. In addition to defining steam password here, you may also keep default value of `null` if you want to enter your steam password on each ASF startup instead of putting it in the config. This may be useful for you if you don't want to save sensitive data in config file.

* * *

### `SteamTradeToken`

Tipo `string` con valor predeterminado de `null`. When you have your bot on your friend list, then bot can send a trade to you right away without worrying about trade token, therefore you can leave this property at default value of `null`. If you however decide to NOT have your bot on your friend list, then you will need to generate and fill a trade token as the user that this bot is expecting to send trades to. In other words, this property should be filled with trade token of the account that is defined with `Master` permission in `SteamUserPermissions` of **this** bot instance.

In order to find your token, as logged in user with `Master` permission, navigate **[here](https://steamcommunity.com/my/tradeoffers/privacy)** and take a look at your trade URL. The token we're looking for is made out of 8 characters after `&token=` part in your trade URL. You should copy and put those 8 characters here, as `SteamTradeToken`. Do not include whole trading URL, neither `&token=` part, only the token itself (8 characters).

* * *

### `SteamUserPermissions`

`ImmutableDictionary<ulong, byte>` type with default value of being empty. This property is a dictionary property which maps given Steam user identified by his 64-bit steam ID, to `byte` number that specifies his permission in ASF instance. Currently available bot permissions in ASF are defined as:

| Valor | Nombre        | Descripción                                                                                                                                                                                        |
| ----- | ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0     | Ninguno       | No permission, this is mainly a reference value that is assigned to steam IDs missing in this dictionary - there is no need to define anybody with this permission                                 |
| 1     | FamilySharing | Provides minimum access for family sharing users. Once again, this is mainly a reference value since ASF is capable of automatically discovering steam IDs that we permitted for using our library |
| 2     | Operator      | Provides basic access to given bot instances, mainly adding licenses and redeeming keys                                                                                                            |
| 3     | Master        | Provides full access to given bot instance                                                                                                                                                         |

In short, this property allows you to handle permissions for given users. Permissions are important mainly for access to ASF **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**, but also for enabling many ASF features, such as accepting trades. For example you might want to set your own account as `Master`, and give `Operator` access to 2-3 of your friends so they can easily redeem keys for your bot with ASF, while **not** being eligible e.g. for stopping it. Thanks to that you can easily assign permissions to given users and let them use your bot to some specified by you degree.

We recommend to set exactly one user as `Master`, and any amount you wish as `Operators` and below. While it's technically possible to set multiple `Masters` and ASF will work correctly with them, for example by accepting all of their trades sent to the bot, ASF will use only one of them (with lowest steam ID) for every action that requires a single target, for example a `loot` request, so also properties like `SendOnFarmingFinished` or `SendTradePeriod`. If you perfectly understand those limitations, especially the fact that `loot` request will always send items to the `Master` with lowest steam ID, regardless of the `Master` that actually executed the command, then you can define multiple users with `Master` permission here, but we still recommend a single master scheme - multiple masters scheme is discouraged setup that is not supported.

It's nice to note that there is one more extra `Owner` permission, which is declared as `SteamOwnerID` global config property. You can't assign `Owner` permission to anybody here, as `SteamUserPermissions` property defines only permissions that are related to the bot instance, and not ASF as a process. For bot-related tasks, `SteamOwnerID` is treated the same as `Master`, so defining your `SteamOwnerID` here is not necessary.

* * *

### `TradingPreferences`

`byte flags` type with default value of `0`. This property defines ASF behaviour when in trading, and is defined as below:

| Valor | Nombre              | Descripción                                                                                                                                                                                          |
| ----- | ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0     | Ninguno             | No trading preferences - accepts only `Master` trades                                                                                                                                                |
| 1     | AcceptDonations     | Accepts trades in which we're not losing anything                                                                                                                                                    |
| 2     | SteamTradeMatcher   | Passively participates in **[STM](https://www.steamtradematcher.com)**-like trades. Visit **[trading](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading#steamtradematcher)** for more info |
| 4     | MatchEverything     | Requires `SteamTradeMatcher` to be set, and in combination with it - also accepts bad trades in addition to good and neutral ones                                                                    |
| 8     | DontAcceptBotTrades | Doesn't automatically accept `loot` trades from other bot instances                                                                                                                                  |
| 16    | MatchActively       | Actively participates in **[STM](https://www.steamtradematcher.com)**-like trades. Visit **[trading](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading#matchactively)** for more info      |

Please notice that this property is `flags` field, therefore it's possible to choose any combination of available values. Check out **[flags mapping](#json-mapping)** if you'd like to learn more. Not enabling any of flags results in `None` option.

For further explanation of ASF trading logic, and description of every available flag, please visit **[trading](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading)** section.

* * *

### `TransferableTypes`

`ImmutableHashSet<byte>` type with default value of `1, 3, 5` steam item types. This property defines which Steam item types will be considered for transferring between bots, during `transfer` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. ASF will ensure that only items from `TransferableTypes` will be included in a trade offer, therefore this property allows you to choose what you want to receive in a trade offer that is being sent to one of your bots.

| Valor | Nombre            | Descripción                                                   |
| ----- | ----------------- | ------------------------------------------------------------- |
| 0     | Unknown           | Every type that doesn't fit in any of the below               |
| 1     | BoosterPack       | Booster pack containing 3 random cards from a game            |
| 2     | Emoticon          | Emoticon to use in Steam Chat                                 |
| 3     | FoilTradingCard   | Foil variant of `TradingCard`                                 |
| 4     | ProfileBackground | Profile background to use on your Steam profile               |
| 5     | TradingCard       | Steam trading card, being used for crafting badges (non-foil) |
| 6     | SteamGems         | Steam gems being used for crafting boosters, sacks included   |
| 7     | SaleItem          | Special items awarded during Steam sales                      |
| 8     | Consumable        | Special consumable items that disappear after being used      |
| 9     | ProfileModifier   | Special items that can modify Steam profile appearance        |

Please note that regardless of the settings above, ASF will only ask for Steam (`appID` of 753) community (`contextID` of 6) items, so all game items, gifts and likewise, are excluded from the trade offer by definition.

Default ASF setting is based on most common usage of the bot, with transfering only booster packs, and trading cards (including foils). The property defined here allows you to alter that behaviour in whatever way that satisfies you. Please keep in mind that all types not defined above will show as `Unknown` type, which is especially important when Valve releases some new Steam item, that will be marked as `Unknown` by ASF as well, until it's added here (in the future release). That's why in general it's not recommended to include `Unknown` type in your `TransferableTypes`, unless you know what you're doing, and you also understand that ASF will send your entire inventory in a trade offer if Steam Network gets broken again and reports all your items as `Unknown`. My strong suggestion is to not include `Unknown` type in the `TransferableTypes`, even if you expect to transfer everything.

* * *

### `UseLoginKeys`

Tipo `bool` con valor predeterminado de `true`. This property defines if ASF should use login keys mechanism for this Steam account. Login keys mechanism works very similar to official Steam client's "remember me" option, which makes it possible for ASF to store and use temporary one-time use login key for next logon attempt, effectively skipping a need of providing password, Steam Guard or 2FA code as long as our login key is valid. Login key is stored in `BotName.db` file and updated automatically. This is why you don't need to provide password/SteamGuard/2FA code after logging in successfully with ASF just once.

Login keys are used by default for your convenience, so you don't need to input `SteamPassword`, SteamGuard or 2FA code (when not using **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)**) on each login. It's also superior alternative since login key can be used only for a single time and does not reveal your original password in any way. Exactly the same method is being used by your original Steam client, which saves your account name and login key for your next logon attempt, effectively being the same as using `SteamLogin` with `UseLoginKeys` and empty `SteamPassword` in ASF.

However, some people might be concerned even about this little detail, therefore this option is available here for you if you'd like to ensure that ASF won't store any kind of token that would allow resuming previous session after being closed, which will result in full authentication being mandatory on each login attempt. Disabling this option will work exactly the same as not checking "remember me" in official Steam client. Unless you know what you're doing, you should keep it with default value of `true`.

* * *

## Estructura de archivos

ASF is using quite simple file structure.

    ├── config
    │     ├── ASF.json
    │     ├── ASF.db
    │     ├── Bot1.json
    │     ├── Bot1.db
    │     ├── Bot1.bin
    │     ├── Bot2.json
    │     ├── Bot2.db
    │     ├── Bot2.bin
    │     └── ...
    ├── ArchiSteamFarm.dll
    ├── log.txt
    └── ...
    

In order to move ASF to new location, for example another PC, it's enough to move/copy `config` directory alone, and that's the recommended way of doing any form of "ASF backups", since you can always download the remaining (program) part from the GitHub, while not risking corrupting internal ASF files, e.g. through a faulty backup.

`log.txt` file holds the log generated by your last ASF run. This file doesn't contain any sensitive information, and is extremely useful when it comes to issues, crashes or simply as an information to you what happened in last ASF run. We will very often ask about this file if you run into issues or bugs. ASF automatically manages this file for you, but you can further tweak ASF **[logging](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Logging)** module if you're advanced user.

`config` directory is the place that holds configuration for ASF, including all of its bots.

`ASF.json` is a global ASF configuration file. This config is used for specifying how ASF behaves as a process, which affects all of the bots and program itself. You can find global properties there, such as ASF process owner, auto-updates or debugging.

`BotName.json` is a config of given bot instance. This config is used for specifying how given bot instance behaves, therefore those settings are specific to that bot only and not shared across other ones. This allows you to configure bots with various different settings and not necessarily all of them working in exactly the same way. Every bot is named using unique identifier, chosen by you in place of `BotName`.

Apart from config files, ASF also uses `config` directory for storing databases.

`ASF.db` is a global ASF database file. It acts as a global persistent storage and is used for saving various information related to ASF process, such as IPs of local Steam servers. **You should not edit this file**.

`BotName.db` is a database of given bot instance. This file is used for storing crucial data about given bot instance in persistent storage, such as login keys or ASF 2FA. **You should not edit this file**.

`BotName.bin` is a special file of given bot instance, which holds information about Steam sentry hash. Sentry hash is used for authenticating using `SteamGuard` mechanism, very similar to Steam `ssfn` file. **You should not edit this file**.

`BotName.keys` is a special file that can be used for importing keys into **[background games redeemer](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer)**. It's not mandatory and not generated, but recognized by ASF. This file is automatically deleted after keys are successfully imported.

`BotName.maFile` is a special file that can be used for importing **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)**. It's not mandatory and not generated, but recognized by ASF if your `BotName` does not use ASF 2FA yet. This file is automatically deleted after ASF 2FA is successfully imported.

* * *

## Mapeo JSON

Every configuration property has its type. Type of the property defines values that are valid for it. You can only use values that are valid for given type - if you use invalid value, then ASF won't be able to parse your config.

**We strongly recommend to use ConfigGenerator for generating configs** - it handles most of the low-level stuff (such as types validation) for you, so you only need to input proper values, and you also don't need to understand variable types specified below. This section is mainly for people generating/editing configs manually, so they know what values they can use.

Types used by ASF are native C# types, which are specified below:

* * *

`bool` - Boolean type accepting only `true` and `false` values.

Example: `"Enabled": true`

* * *

`byte` - Unsigned byte type, accepting only integers from `0` to `255` (inclusive).

Example: `"ConnectionTimeout": 60`

* * *

`ushort` - Unsigned short type, accepting only integers from `0` to `65535` (inclusive).

Example: `"WebLimiterDelay": 300`

* * *

`uint` - Unsigned integer type, accepting only integers from `0` to `4294967295` (inclusive).

* * *

`ulong` - Unsigned long integer type, accepting only integers from `0` to `18446744073709551615` (inclusive).

Example: `"SteamMasterClanID": 103582791440160998`

* * *

`string` - String type, accepting any sequence of characters, including empty sequence `""` and `null`. Both empty sequence as well as `null` value is treated the same by ASF, so it's up to your preference which one you want to use.

Examples: `"SteamLogin": null`, `"SteamLogin": ""`, `"SteamLogin": "MyAccountName"`

* * *

`ImmutableHashSet<valueType>` - Immutable collection (set) of unique values in given `valueType`. In JSON, it's defined as array of elements in given `valueType`. ASF uses `HashSet` to indicate that given property makes sense only for unique values, therefore it'll intentionally ignore any potential duplicates during parsing (if you happened to supply them anyway).

Example for `ImmutableHashSet<uint>`: `"Blacklist": [267420, 303700, 335590]`

* * *

`ImmutableDictionary<keyType, valueType>` - Immutable dictionary (map) that maps a unique key specified in its `keyType`, to value specified in its `valueType`. In JSON, it's defined as an object with key-value pairs. Keep in mind that `keyType` is always quoted in this case, even if it's value type such as `ulong`. There is also a strict requirement of the key being unique across the map, this time enforced by JSON itself.

Example for `ImmutableDictionary<ulong, byte>`: `"SteamUserPermissions": { "76561198174813138": 3, "76561198174813137": 1 }`

* * *

`flags` - Flags attribute combines several different properties into one final value by applying bitwise operations. This allows you to choose any possible combination of various different allowed values at the same time. The final value is constructed as a sum of values of all enabled options.

For example, given following values:

| Valor | Nombre  |
| ----- | ------- |
| 0     | Ninguno |
| 1     | A       |
| 2     | B       |
| 4     | C       |

Using `B + C` would result in value of `6`, using `A + C` would result in value of `5`, using `C` would result in value of `4` and so on. This allows you to create any possible combination of enabled values - if you decided to enable all of them, making `None + A + B + C`, you'd get value of `7`. Also notice that flag with value of `0` is enabled by definition in all other available combinations, therefore very often it's a flag that doesn't enable anything specifically (such as `None`).

So as you can see, in above example we have 3 available flags to switch on/off (`A`, `B`, `C`), and `8` possible values overall:

- `None -> 0`
- `A -> 1`
- `B -> 2`
- `A+B -> 3`
- `C -> 4`
- `A+C -> 5`
- `B+C -> 6`
- `A+B+C -> 7`

* * *

## Compatibilidad de mapeo

Due to JavaScript limitations of being unable to properly serialize simple `ulong` fields in JSON when using web-based ConfigGenerator, `ulong` fields will be rendered as strings with `s_` prefix in the resulting config. This includes for example `"SteamOwnerID": 76561198006963719` that will be written by our ConfigGenerator as `"s_SteamOwnerID": "76561198006963719"`. ASF includes proper logic for handling this string mapping automatically, so `s_` entries in your configs are actually valid and correctly generated. If you're generating configs yourself, we recommend to stick with original `ulong` fields if possible, but if you're unable to do so, you can also follow this scheme and encode them as strings with `s_` prefix added to their names. We hope to resolve this JavaScript limitation eventually.

* * *

## Compatibilidad de configuraciones

It's top priority for ASF to remain compatible with older configs. As you should already know, missing config properties are treated the same as they would be defined with their **default values**. Therefore, if new config property gets introduced in new version of ASF, all your configs will remain **compatible** with new version, and ASF will treat that new config property as it'd be defined with its **default value**. You can always add, remove or edit config properties according to your needs. We recommend to limit defined config properties only to those that you want to change, since this way you automatically inherit default values for all other ones, not only keeping your config clean but also increasing compatibility in case we decide to change a default value for property that you don't want to explicitly set yourself (e.g. `WebLimiterDelay`).

* * *

## Autorrecarga

Starting with ASF V2.1.6.2+, the program is now aware of configs being modified "on-the-fly" - thanks to that, ASF will automatically:

- Create (and start, if needed) new bot instance, when you create its config
- Stop (if needed) and remove old bot instance, when you delete its config
- Stop (and start, if needed) any bot instance, when you edit its config
- Restart (if needed) the bot under new name, when you rename its config

All of the above is transparent and will be done automatically without a need of restarting the program, or killing other (unaffected) bot instances.

In addition to that, ASF will also restart itself (if `AutoRestart` permits) if you modify core ASF `ASF.json` config. Likewise, program will quit if you delete or rename it.