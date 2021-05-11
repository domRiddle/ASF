# Comandos

ASF soporta una variedad de comandos, que pueden ser utilizados para controlar el comportamiento del proceso y las instancias de bot.

Los comandos pueden ser enviados al bot de diferentes formas:

- A través de la consola interactiva de ASF
- A través de Steam por chat privado/grupal
- A través de la interfaz **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-es-es)**

Ten en cuenta que la interacción con ASF requiere que seas elegible para el comando de acuerdo a los permisos de ASF. Para más detalles, revisa las propiedades de configuración `SteamUserPermissions` y `SteamOwnerID`.

Los comandos ejecutados por medio del chat de Steam son afectados por la **[propiedad de configuración global](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-es-es#commandprefix)** `CommandPrefix`, que por defecto es `!`. Esto significa que para ejecutar, por ejemplo, el comando `status`, debes escribir `!status` (o en su caso, el `CommandPrefix` de tu elección que hayas establecido). `CommandPrefix` no es obligatorio cuando se usa la consola interactiva o IPC y puede ser omitido en esos casos.

* * *

### Consola interactiva

A partir de V4.0.0.9, ASF tiene soporte para una consola interactiva que puede ser activada configurando la propiedad [**`SteamOwnerID`**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-es-es#steamownerid). Después, simplemente pulsa la tecla `c` para activar el modo de comandos, escribe tu comando y confirma con Enter.

![Captura de pantalla](https://i.imgur.com/bH5Gtjq.png)

La consola interactiva no está disponible en el modo [**`Headless`**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-es-es#headless).

* * *

### Chat de Steam

También puedes ejecutar comandos en un bot determinado a través del chat de Steam. Obviamente no puedes hablar contigo mismo directamente, por lo tanto necesitarás al menos otra cuenta bot para ejecutar comandos dirigidos a tu cuenta principal.

![Captura de pantalla](https://i.imgur.com/IvFRJ5S.png)

De la misma manera, también puedes usar el chat grupal de un grupo de Steam determinado. Ten en cuenta que esta opción requiere establecer correctamente la propiedad `SteamMasterClanID`, en cuyo caso el bot también escuchará comandos en el chat grupal (y se unirá si es necesario). Esto también puede usarse para "hablar contigo mismo" ya que no requiere una cuenta bot dedicada, contrario al chat privado. Puedes simplemente establecer la propiedad `SteamMasterClanID` de tu grupo recién creado, luego darte acceso a ti mismo ya sea con `SteamOwnerID` o `SteamUserPermissions` de tu bot. De esta manera, el bot (tú) se unirá al grupo seleccionado y a su chat, y escuchará comandos de tu propia cuenta. Puedes unirte al mismo chat de grupo para enviarte comandos a ti mismo (ya que estarás enviando comandos a un chat de grupo, y la instancia de ASF en ese mismo chat los recibirá, incluso si se muestra como si solo tu cuenta estuviese ahí).

Por favor, ten en cuenta que enviar un comando al chat de grupo funciona como un relé. Si enváis `redeem X` a 3 de tus bots que están junto contigo en el chat de grupo, el resultado será igual que si enviaras `redeem X` a cada uno de ellos en privado. En la mayoría de los casos **esto no es lo que quieres**, y en cambio deberías usar un comando a un `given bot` bot determinado que sea enviado a **un solo bot en una ventana privada**. ASF soporta el chat de grupo, ya que en muchos casos puede ser útil para la comunicación con tu único bot, pero casi nunca deberías ejecutar ningún comando en el chat de grupo si hay 2 o más bots unidos, a menos que entiendas completamente el comportamiento de ASF descrito aquí y que de hecho quieres enviar el mismo comando a cada bot que esté escuchando.

*E incluso en este caso deberías usar el chat privado con la sintaxis `[Bots]`.*

* * *

### IPC

La forma más avanzada y flexible de ejecutar comandos, perfecta para la interacción con el usuario (ASF-ui) así como con herramientas de terceros o con scripting (ASF API), requiere que ASF se ejecute en modo `IPC`, y un cliente que ejecute comandos a través de la interfaz **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-es-es)**.

![Captura de pantalla](https://raw.githubusercontent.com/JustArchiNET/ASF-ui/main/.github/previews/commands.png)

* * *

## Comandos

| Comando                                                              | Acceso          | Descripción                                                                                                                                                                                                                                                                                                                                                                                        |
| -------------------------------------------------------------------- | --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `2fa [Bots]`                                                         | `Master`        | Genera un código **[2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-es-es)** temporal para determinadas instancias de bot.                                                                                                                                                                                                                                      |
| `2fano [Bots]`                                                       | `Master`        | Rechaza todas las confirmaciones **[2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-es-es)** pendientes para determinadas instancias de bot.                                                                                                                                                                                                                    |
| `2faok [Bots]`                                                       | `Master`        | Acepta todas las confirmaciones **[2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-es-es)** pendientes para determinadas instancias de bot.                                                                                                                                                                                                                     |
| `addlicense [Bots] <Licenses>`                                 | `Operator`      | Activa las `licenses` licencias especificadas, explicado **[abajo](#addlicense-añadir-licencias)**, en instancias de bot determinadas (solo juegos gratuitos).                                                                                                                                                                                                                                     |
| `balance [Bots]`                                                     | `Master`        | Muestra el saldo de la cartera de instancias de bot determinadas.                                                                                                                                                                                                                                                                                                                                  |
| `bgr [Bots]`                                                         | `Master`        | Muestra información sobre la cola del **[activador de juegos en segundo plano](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer-es-es)** de instancias de bot determinadas.                                                                                                                                                                                           |
| `bl [Bots]`                                                          | `Master`        | Enlista los usuarios bloqueados del módulo de intercambio de instancias de bot determinadas.                                                                                                                                                                                                                                                                                                       |
| `bladd [Bots] <SteamIDs64>`                                    | `Master`        | Bloquea ciertas `steamIDs` del módulo de intercambio de instancias de bot determinadas.                                                                                                                                                                                                                                                                                                            |
| `blrm [Bots] <SteamIDs64>`                                     | `Master`        | Desbloquea determinadas `steamIDs` del módulo de intercambio de instancias de bot determinadas.                                                                                                                                                                                                                                                                                                    |
| `encrypt <encryptionMethod> <stringToEncrypt>`           | `Owner`         | Cifra la cadena de caracteres usando el método criptográfico proporcionado - se explica con detalle **[debajo](#comando-encrypt)**.                                                                                                                                                                                                                                                                |
| `exit`                                                               | `Owner`         | Detiene todo el proceso de ASF.                                                                                                                                                                                                                                                                                                                                                                    |
| `farm [Bots]`                                                        | `Master`        | Reinicia el módulo de recolección de cromos para instancias de bot determinadas.                                                                                                                                                                                                                                                                                                                   |
| `hash <hashingMethod> <stringToHash>`                    | `Owner`         | Genera un hash de la cadena de caracteres usando el método criptográfico proporcionado - explicado con detalle **[debajo](#comando-hash)**.                                                                                                                                                                                                                                                        |
| `help`                                                               | `FamilySharing` | Muestra la ayuda (enlace a esta página).                                                                                                                                                                                                                                                                                                                                                           |
| `input [Bots] <Type> <Value>`                            | `Master`        | Establece un determinado tipo de entrada a un valor dado para instancias de bot determinadas, solo funciona en modo `Headless` - se explica con detalle **[debajo](#comando-input)**.                                                                                                                                                                                                              |
| `ib [Bots]`                                                          | `Master`        | Enlista las aplicaciones bloqueadas para recolección automática de instancias de bot determinadas.                                                                                                                                                                                                                                                                                                 |
| `ibadd [Bots] <AppIDs>`                                        | `Master`        | Añade ciertas `appIDs` a las aplicaciones bloqueadas para recolección automática de instancias de bot determinadas.                                                                                                                                                                                                                                                                                |
| `ibrm [Bots] <AppIDs>`                                         | `Master`        | Remueve ciertas `appIDs` de la lista de aplicaciones bloqueadas para la recolección automática de instancias de bot determinadas.                                                                                                                                                                                                                                                                  |
| `iq [Bots]`                                                          | `Master`        | Muestra la cola de prioridad de recolección de instancias de bot determinadas.                                                                                                                                                                                                                                                                                                                     |
| `iqadd [Bots] <AppIDs>`                                        | `Master`        | Añade ciertas `appIDs` a la cola de prioridad de recolección de instancias de bot determinadas.                                                                                                                                                                                                                                                                                                    |
| `iqrm [Bots] <AppIDs>`                                         | `Master`        | Remueve ciertas `appIDs` de la cola de prioridad de recolección de instancias de bot determinadas.                                                                                                                                                                                                                                                                                                 |
| `level [Bots]`                                                       | `Master`        | Muestra el nivel de la cuenta de Steam de instancias de bot determinadas.                                                                                                                                                                                                                                                                                                                          |
| `loot [Bots]`                                                        | `Master`        | Envía todos los artículos de la comunidad de Steam establecidos en `LootableTypes` de instancias de bot determinadas al usuario definido como `Master` en su `SteamUserPermissions` (el que tenga el steamID más bajo si hay más de uno).                                                                                                                                                          |
| `loot@ [Bots] <AppIDs>`                                        | `Master`        | Envía todos los artículos de la comunidad de Steam establecidos en `LootableTypes` que coincidan con ciertas `AppIDs` de instancias de bot determinadas al usuario definido como `Master` en su `SteamUserPermissions` (el que tenga el steamID más bajo si hay más de uno). Esto es lo contrario de `loot%`.                                                                                      |
| `loot% [Bots] <AppIDs>`                                        | `Master`        | Envía todos los artículos de la comunidad de Steam establecidos en `LootableTypes` excepto los de las `AppIDs` proporcionadas, de instancias de bot determinadas al usuario definido como `Master` en su `SteamUserPermissions` (el que tenga el steamID más bajo si hay más de uno). Esto es lo contrario de `loot@`.                                                                             |
| `loot^ [Bots] <AppID> <ContextID>`                       | `Master`        | Envía todos los artículos de Steam de cierto `AppID` y `ContextID` de instancias de bot determinadas al usuario definido como `Master` en su `SteamUserPermissions` (el que tenga el steamID más bajo si hay más de uno).                                                                                                                                                                          |
| `mab [Bots]`                                                         | `Master`        | Muestra las aplicaciones bloqueadas del intercambio automático en **[`MatchActively`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading-es-es#matchactively)**.                                                                                                                                                                                                                          |
| `mabadd [Bots] <AppIDs>`                                       | `Master`        | Añade ciertas `appIDs` a la lista de aplicaciones bloqueadas del intercambio automático en **[`MatchActively`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading-es-es#matchactively)**.                                                                                                                                                                                                 |
| `mabrm [Bots] <AppIDs>`                                        | `Master`        | Remueve ciertas `appIDs` de la lista de aplicaciones bloqueadas del intercambio automático en **[`MatchActively`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading-es-es#matchactively)**.                                                                                                                                                                                              |
| `nickname [Bots] <Nickname>`                                   | `Master`        | Cambia el nombre de perfil de Steam de instancias de bot determinadas al `nickname` nombre de perfil especificado.                                                                                                                                                                                                                                                                                 |
| `owns [Bots] <Games>`                                          | `Operator`      | Comprueba si las instancias de bot determinadas ya poseen ciertos `games` juegos, explicado **[abajo](#owns-juegos)**.                                                                                                                                                                                                                                                                             |
| `password [Bots]`                                                    | `Master`        | Muestra la contraseña cifrada de instancias de bot determinadas (en uso junto con `PasswordFormat`).                                                                                                                                                                                                                                                                                               |
| `pause [Bots]`                                                       | `Operator`      | Pausa permanentemente el módulo de recolección automática de cromos de instancias de bot determinadas. ASF no intentará recolectar la cuenta actual en esta sesión, a menos que la reanudes manualmente a través del comando `resume`, o reinicies el proceso.                                                                                                                                     |
| `pause~ [Bots]`                                                      | `FamilySharing` | Pausa temporalmente el módulo de recolección automática de cromos de instancias de bot determinadas. La recolección se reanudará automáticamente en el siguiente evento de juego, o cuando se desconecte el bot. Puedes usar el comando `resume` para reanudar la recolección.                                                                                                                     |
| `pause& [Bots] <Seconds>`                                  | `Operator`      | Pausa temporalmente el módulo de recolección automática de cromos de instancias de bot determinadas por la cantidad de `seconds` segundos especificada. Después de ese tiempo, el módulo de recolección de cromos se reanuda automáticamente.                                                                                                                                                      |
| `play [Bots] <AppIDs,GameName>`                                | `Master`        | Cambia a recolección manual - ejecuta ciertas `AppIDs` en instancias de bot determinadas, opcionalmente también con un nombre de juego personalizado `GameName`. Para que esta característica funcione correctamente, tu cuenta de Steam **debe** poseer una licencia válida para todas las `AppIDs` que especifiques aquí, esto también incluye juegos F2P. Usa `reset` o `resume` para reanudar. |
| `privacy [Bots] <Settings>`                                    | `Master`        | Cambia la **[configuración de privacidad de Steam](https://steamcommunity.com/my/edit/settings)** de instancias de bot determinadas, a las opciones seleccionadas que se explican **[debajo](#ajustes-privacy)**.                                                                                                                                                                                  |
| `redeem [Bots] <Keys>`                                         | `Operator`      | Activa ciertas claves de producto o códigos de la cartera en instancias de bot determinadas.                                                                                                                                                                                                                                                                                                       |
| `redeem^ [Bots] <Modes> <Keys>`                          | `Operator`      | Activa ciertas claves de producto o códigos de la cartera en instancias de bot determinadas, usando ciertos modos `modes` explicados **[debajo](#modos-redeem)**.                                                                                                                                                                                                                                  |
| `reset [Bots]`                                                       | `Master`        | Restablece el estado de juego a normal, usado durante la recolección manual con el comando `play`.                                                                                                                                                                                                                                                                                                 |
| `restart`                                                            | `Owner`         | Reinicia el proceso de ASF.                                                                                                                                                                                                                                                                                                                                                                        |
| `resume [Bots]`                                                      | `FamilySharing` | Reanuda la recolección automática de instancias de bot determinadas. Revisa también `pause`, `play`.                                                                                                                                                                                                                                                                                               |
| `start [Bots]`                                                       | `Master`        | Inicia instancias de bot determinadas.                                                                                                                                                                                                                                                                                                                                                             |
| `stats`                                                              | `Owner`         | Muestra las estadísticas del proceso, tal como el uso de memoria.                                                                                                                                                                                                                                                                                                                                  |
| `status [Bots]`                                                      | `FamilySharing` | Muestra el estatus de instancias de bot determinadas.                                                                                                                                                                                                                                                                                                                                              |
| `stop [Bots]`                                                        | `Master`        | Detiene instancias de bot determinadas.                                                                                                                                                                                                                                                                                                                                                            |
| `transfer [Bots] <TargetBot>`                                  | `Master`        | Envía todos los artículos de la comunidad de Steam establecidos en `TransferableTypes` de instancias de bot determinadas a la instancia de bot destino.                                                                                                                                                                                                                                            |
| `transfer@ [Bots] <AppIDs> <TargetBot>`                  | `Master`        | Envía todos los artículos de la comunidad de Steam establecidos en `TransferableTypes` que coincidan con las `AppIDs` especificadas, de instancias de bot determinadas a la instancia de bot destino. Esto es lo contrario de `transfer%`.                                                                                                                                                         |
| `transfer% [Bots] <AppIDs> <TargetBot>`                  | `Master`        | Envía todos los artículos de la comunidad de Steam establecidos en `TransferableTypes` excepto los de las `AppIDs` especificadas, de instancias de bot determinadas a la instancia de bot destino. Esto es lo contrario de `transfer@`.                                                                                                                                                            |
| `transfer^ [Bots] <AppID> <ContextID> <TargetBot>` | `Master`        | Envía todos los artículos de Steam de `AppID` y `ContextID` especificados de instancias de bot determinadas a la instancia de bot destino.                                                                                                                                                                                                                                                         |
| `unpack [Bots]`                                                      | `Master`        | Abre todos los packs de refuerzo almacenados en el inventario de instancias de bot determinadas.                                                                                                                                                                                                                                                                                                   |
| `update`                                                             | `Owner`         | Comprueba en GitHub si hay actualizaciones para ASF (esto se hace automáticamente cada `UpdatePeriod`).                                                                                                                                                                                                                                                                                            |
| `version`                                                            | `FamilySharing` | Muestra la versión de ASF.                                                                                                                                                                                                                                                                                                                                                                         |

* * *

### Notas

Todos los comandos son insensibles a mayúsculas y minúsculas, pero sus argumentos (como nombres de bots) sí suelen distinguir mayúsculas y minúsculas.

El argumento `[Bots]` es opcional en todos los comandos. Cuando se especifica, el comando se ejecuta en los bots indicados. Cuando se omite, el comando se ejecuta en el bot actual que reciba el comando. En otras palabras, `status A` enviado al bot `B` es lo mismo que enviar `status` al bot `A`, pero el bot `B` en este caso actúa solo como un proxy. Esto también se puede usar para enviar comandos a bots que de otro modo no están disponibles, por ejemplo, iniciar bots detenidos, o ejecutar acciones en tu cuenta principal (que estás usando para ejecutar comandos).

El **acceso** del comando define el `EPermission` permiso **mínimo** de `SteamUserPermissions` que se requiere para usar el comando, con la excepción de `Owner` propietario que se define en `SteamOwnerID` en el archivo de configuración global (y que es el permiso más alto disponible).

Los argumentos plurales, como `[Bots]`, `<Keys>` o `<AppIDs>` significan que el comando soporta múltiples argumentos de un tipo dado, separados por una coma. Por ejemplo, `status [Bots]` puede ser usado como `status MyBot,MyOtherBot,Primary`. Esto causará que el comando se ejecute en **todos los bots objetivo** de la misma forma que si enviaras `status` a cada bot en una ventana de chat separada. Por favor, ten en cuenta que no hay espacio después de la coma `,`.

ASF usa todos los caracteres en blanco como posibles delimitadores para un comando, tal como espacio y caracteres de nueva línea. Esto significa que no tienes que usar espacio para delimitar tus argumentos, puedes usar cualquier otro carácter en blanco (como tabulador o nueva línea).

ASF "unirá" argumentos adicionales fuera de rango a tipo plural del último argumento en rango. Esto significa que `redeem bot key1 key2 key3` para `redeem [Bots] <Keys>` funcionará exactamente igual que `redeem bot key1,key2,key3`. Junto con el hecho de aceptar una nueva línea como delimitador de comandos, esto hace posible que escribas `redeem bot` y luego pegues una lista de claves separadas por cualquier delimitador aceptable (tal como nueva línea), o el delimitador estándar `,` de ASF. Ten en cuenta que este truco solo puede ser usado para una variante del comando que use la mayor cantidad de argumentos (así que especificar `[Bots]` es obligatorio en este caso).

Como leíste arriba, se usa un carácter de espacio como delimitador para un comando, por lo tanto no puede ser usado en los argumentos. Sin embargo, como se ha dicho anteriormente, ASF puede unir argumentos fuera de rango, esto significa que realmente puedes usar un carácter de espacio en el último argumento definido para un comando especificado. Por ejemplo, `nickname bob Great Bob` establecerá correctamente el nombre de perfil del bot `bob` a "Great Bob". De la misma manera puedes comprobar nombres que contengan espacios en el comando `owns`.

* * *

Algunos comandos también están disponibles con sus alias, para ahorrarte la escritura:

| Comando      | Alias |
| ------------ | ----- |
| `owns ASF`   | `oa`  |
| `status ASF` | `sa`  |
| `redeem`     | `r`   |
| `redeem^`    | `r^`  |

* * *

### Argumento `[Bots]`

El argumento `[Bots]` es una variante especial de los argumentos plurales, ya que además de aceptar múltiples valores también ofrece funcionalidad adicional.

En primer lugar, hay una palabra clave `ASF` que actúa como "todos los bots en el proceso", así que el comando `status ASF` es igual a `status todos,tus,bots,enlistados,aquí`. Esto también puede utilizarse para identificar fácilmente los bots a los que tienes acceso, ya que la palabra clave `ASF`, a pesar de dirigirse a todos los bots, solo tendrá respuesta de aquellos bots a los que realmente puedes enviar el comando.

El argumento `[Bots]` soporta una sintaxis especial de "rango", que te permite elegir un rango de bots más fácilmente. La sintaxis general para `[Bots]` en este caso es `primerBot..últimoBot`. Por ejemplo, si tienes bots llamados `A, B, C, D, E, F`, puedes ejecutar `status B..E`, lo que es igual a `status B,C,D,E` en este caso. Al usar esta sintaxis, ASF usará el orden alfabético para determinar qué bots están en tu rango especificado. Tanto `primerBot` como `últimoBot` deben ser nombres válidos de bots reconocidos por ASF, de lo contrario la sintaxis de rango se omite completamente.

Además de la sintaxis de rango, el argumento `[Bots]` también soporta la coincidencia de **[expresión regular](https://es.wikipedia.org/wiki/Expresi%C3%B3n_regular)**. Puedes activar el patrón de expresión regular usando `r!<pattern>` como nombre de bot, donde `r!` es el activador de ASF para la coincidencia de expresión regular, y `<pattern>` es tu patrón de expresión regular. Un ejemplo de comando basado en expresión regular sería `status r!\d{3}` que enviará el comando `status` a los bots que tengan un nombre formado por 3 dígitos (por ejemplo `123` y `981`). No dudes en echar un vistazo a la **[documentación](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference)** para mayor información y más ejemplos de patrones de expresión regular disponibles.

* * *

## Ajustes `privacy`

Los argumentos `<Settings>` aceptan **hasta 7** opciones diferentes, separadas como es habitual por una coma, que es el delimitador estándar de ASF. Esas son, en orden:

| Argumento | Nombre              | Hijo de        |
| --------- | ------------------- | -------------- |
| 1         | Perfil              |                |
| 2         | JuegosPoseídos      | Perfil         |
| 3         | TiempoDeJuego       | JuegosPoseídos |
| 4         | ListaDeAmigos       | Perfil         |
| 5         | Inventario          | Perfil         |
| 6         | InventarioDeRegalos | Inventario     |
| 7         | Comentarios         | Perfil         |

Para la descripción de los campos anteriores, por favor, visita la **[configuración de privacidad de Steam](https://steamcommunity.com/my/edit/settings)**.

Mientras que los valores válidos para todos ellos son:

| Valor | Nombre        |
| ----- | ------------- |
| 1     | `Private`     |
| 2     | `FriendsOnly` |
| 3     | `Public`      |

Puedes usar un nombre, que no distingue mayúsculas y minúsculas, o un valor numérico. Los argumentos omitidos por defecto se establecerán a `Private` privado. Es importante tener en cuenta la relación entre padre e hijo de los argumentos especificados anteriormente, ya que el hijo nunca puede tener permisos más abiertos que el padre. Por ejemplo, **no** puedes tener juegos poseídos en `Public` público teniendo el perfil en `Private` privado.

### Ejemplo

Si quieres establecer **todos** los ajustes de privacidad de tu bot llamado `Main` a `Private` privado, puedes usar cualquiera de los siguientes:

```text
privacy Main 1
privacy Main Private
```

Esto es porque ASF asumirá automáticamente que todos los demás ajustes son `Private` privados, así que no hay necesidad de especificarlos. Por otra parte, si quieres establecer todos los ajustes de privacidad a `Public` público, entonces deberías usar cualquiera de los siguientes:

```text
privacy Main 3,3,3,3,3,3,3
privacy Main Public,Public,Public,Public,Public,Public,Public
```

De esta manera también puedes establecer opciones independientes de la forma que quieras:

```text
privacy Main Public,FriendsOnly,Private,Public,Public,Private,Public
```

Lo anterior establecerá el perfil a público, los juegos poseídos a solo amigos, el tiempo de juego a privado, la lista de amigos a público, el inventario a público, el inventario de regalos a privado y los comentarios en el perfil a público. Puedes lograr lo mismo con valores numéricos si así lo quieres.

Recuerda que el hijo nunca puede tener permisos más abiertos que su padre. Consulta la relación de los argumentos para las opciones disponibles.

* * *

## `addlicense` añadir licencias

El comando `addlicense` soporta dos tipos de licencia diferentes, estos son:

| Tipo  | Alias | Ejemplo      | Descripción                                                              |
| ----- | ----- | ------------ | ------------------------------------------------------------------------ |
| `app` | `a`   | `app/292030` | Juego determinado por su `appID` única.                                  |
| `sub` | `s`   | `sub/47807`  | Paquete que contiene uno o más juegos, determinado por su `subID` único. |

La distinción es importante, ya que ASF usará la activación de la red de Steam para las aplicaciones, y la activación de la tienda de Steam para los paquetes. Estos dos no son compatibles entre sí, normalmente usarías aplicaciones (app) para fines de semana gratis y juegos permanentemente F2P, y paquetes (sub) en los demás casos.

Recomendamos definir explícitamente el tipo de cada entrada para evitar resultados ambiguos, pero por la retrocompatibilidad, si proporcionas un tipo no válido o lo omites por completo, ASF asumirá que estás solicitando `sub` en este caso. También puedes consultar una o más de las licencias al mismo tiempo, usando la coma `,` que es el el delimitador estándar de ASF.

Ejemplo de comando completo:

```text
addlicense ASF app/292030,sub/47807
```

* * *

## `owns` juegos

El comando `owns` soporta varios tipos de juegos diferentes para el argumento `<games>`, estos son:

| Tipo    | Alias | Ejemplo          | Descripción                                                                                                                                                                                                                                                                                                        |
| ------- | ----- | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `app`   | `a`   | `app/292030`     | Juego determinado por su `appID` única.                                                                                                                                                                                                                                                                            |
| `sub`   | `s`   | `sub/47807`      | Paquete que contiene uno o más juegos, determinado por su `subID` único.                                                                                                                                                                                                                                           |
| `regex` | `r`   | `regex/^\d{4}:` | **[Expresión regular](https://es.wikipedia.org/wiki/Expresi%C3%B3n_regular)** aplicada al nombre del juego, distingue mayúsculas y minúsculas. Ve la **[documentación](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference)** para sintaxis completa y más ejemplos. |
| `name`  | `n`   | `name/Witcher`   | Parte del nombre del juego, distingue mayúsculas y minúsculas.                                                                                                                                                                                                                                                     |

Recomendamos definir explícitamente el tipo de cada entrada para evitar resultados ambiguos, pero para la retrocompatibilidad, si proporcionas un tipo no válido o lo omites por completo, ASF asumirá que estás solicitando `app` si ingresas un número, y `name` de lo contrario. También puedes consultar uno o más de los juegos al mismo tiempo, usando la coma `,` que es el el delimitador estándar de ASF.

Ejemplo de comando completo:

```text
owns ASF app/292030,name/Witcher
```

* * *

## Modos `redeem^`

El comando `redeem^` te permite ajustar los modos que serán utilizados para un escenario de activación. Esto funciona como una anulación temporal de la **[propiedad de configuración del bot](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-es-es#configuraci%C3%B3n-de-bot)** `RedeemingPreferences`.

El argumento `<Modes>` acepta múltiples valores de modo, separados como es usual por una coma. Los valores de modo disponibles se especifican a continuación:

| Valor | Nombre                | Descripción                                                                                     |
| ----- | --------------------- | ----------------------------------------------------------------------------------------------- |
| FAWK  | ForceAssumeWalletKey  | Fuerza que la preferencia de activación `AssumeWalletKeyOnBadActivationCode` esté habilitada    |
| FD    | ForceDistributing     | Fuerza que la preferencia de activación `Distributing` esté habilitada                          |
| FF    | ForceForwarding       | Fuerza que la preferencia de activación `Forwarding` esté habilitada                            |
| FKMG  | ForceKeepMissingGames | Fuerza que la preferencia de activación `KeepMissingGames` esté habilitada                      |
| SAWK  | SkipAssumeWalletKey   | Fuerza que la preferencia de activación `AssumeWalletKeyOnBadActivationCode` esté deshabilitada |
| SD    | SkipDistributing      | Fuerza que la preferencia de activación `Distributing` esté deshabilitada                       |
| SF    | SkipForwarding        | Fuerza que la preferencia de activación `Forwarding` esté deshabilitada                         |
| SI    | SkipInitial           | Omite la activación de claves en el bot inicial                                                 |
| SKMG  | SkipKeepMissingGames  | Fuerza que la preferencia de activación `KeepMissingGames` esté deshabilitada                   |
| V     | Validate              | Valida que las claves tengan el formato correcto y automáticamente omite las inválidas          |

Por ejemplo, si quisieramos activar 3 claves en cualquiera de nuestros bots que aún no poseen juegos, pero no en nuestro bot `primary`. Para lograrlo podemos utilizar:

`redeem^ primary FF,SI clave1,clave2,clave3`

Es importante notar que la activación avanzada solo anula las `RedeemingPreferences` preferencias de activación que **especifiques en el comando**. Por ejemplo, si tienes habilitado `Distributing` en tus `RedeemingPreferences` entonces no habrá diferencia si usas o no el modo `FD`, porque la distribución ya estará activa, debido a las `RedeemingPreferences` que usas. Por eso cada anulación para habilitar a la fuerza también tiene una para deshabilitar a la fuerza, puedes decidir si prefieres anular deshabilitada con habilitada, o viceversa.

* * *

## Comando `encrypt`

El comando `encrypt` te permite cifrar cadenas de caracteres usando los métodos de cifrado de ASF. `<encryptionMethod>` debe ser uno de los métodos de cifrado especificados y explicados en la sección **[seguridad](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security-es-es)**. Este comando es útil en caso de que desees generar detalles cifrados de antemano, por ejemplo, para evitar introducir primero tu contraseña en `PlainText` en la configuración y después usar el comando `password`. Recomendamos usar este comando a través de canales seguros (consola de ASF o la interfaz IPC, que también tienen un API endpoint dedicado para ello), ya que de lo contrario los detalles confidenciales podrían ser registrados por terceros (tal como mensajes de chat registrados por los servidores de Steam).

* * *

## Comando `hash`

El comando `hash` te permite generar hashes de cadenas arbitrarias usando los métodos de hash de ASF. `<hashingMethod>` debe ser uno de los métodos de hash especificados y explicados en la sección **[seguridad](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security-es-es)**. Recomendamos usar este comando a través de canales seguros (consola de ASF o la interfaz IPC, que también tienen un API endpoint dedicado para ello), ya que de lo contrario los detalles confidenciales podrían ser registrados por terceros (tal como mensajes de chat registrados por los servidores de Steam).

* * *

## Comando `input`

El comando `input` solo puede usarse en modo `Headless`, para introducir determinados datos a través de **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-es-es)** o del chat de Steam cuando ASF se está ejecutando sin soporte para interacción del usuario.

La sintaxis general es `input [Bots] <Type> <Value>`.

`<Type>` no distingue mayúsculas y minúsculas y define el tipo de entrada reconocido por ASF. Actualmente ASF reconoce los siguientes tipos:

| Tipo                    | Descripción                                                                                  |
| ----------------------- | -------------------------------------------------------------------------------------------- |
| Login                   | Propiedad de configuración del bot `SteamLogin`, si está ausente en la configuración.        |
| Password                | Propiedad de configuración del bot `SteamPassword`, si está ausente en la configuración.     |
| SteamGuard              | Código de autenticación enviado a tu correo electrónico si no usas 2FA.                      |
| SteamParentalCode       | Propiedad de configuración del bot `SteamParentalCode`, si está ausente en la configuración. |
| TwoFactorAuthentication | Código 2FA generado desde tu móvil, si usas 2FA pero no ASF 2FA.                             |

`<Value>` es el valor establecido para cada tipo. Actualmente todos los valores son cadenas de caracteres.

### Ejemplo

Digamos que tenemos un bot protegido por SteamGuard en modo no-2FA. Queremos ejecutar ese bot con `Headless` establecido a `true`.

Para ello, necesitamos ejecutar los siguientes comandos:

`start MySteamGuardBot` -> El bot intentará iniciar sesión, fallará debido a que se requiere el código de autenticación, luego se detendrá debido a que se ejecuta en modo `Headless`. Necesitamos esto para hacer que la red de Steam nos envíe un código de autenticación a nuestro correo electrónico - si no hubiera necesidad de eso, omitiríamos por completo este paso.

`input MySteamGuardBot SteamGuard ABCDE` -> Establecemos la entrada `SteamGuard` del bot `MySteamGuardBot` a `ABCDE`. Por supuesto, `ABCDE` en este caso es el código de autenticación que recibimos en nuestro correo electrónico.

`start MySteamGuardBot` -> Iniciamos nuevamente nuestro bot (detenido), esta vez automáticamente usa el código de autenticación que establecimos en el comando anterior, iniciando sesión correctamente, y luego lo borra.

De la misma manera podemos acceder a bots protegidos por 2FA (si no están usando ASF 2FA), así como establecer otras propiedades requeridas durante el tiempo de ejecución.