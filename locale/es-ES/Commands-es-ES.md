# Comandos

ASF soporta una variedad de comandos, que pueden ser utilizados para controlar el comportamiento del proceso y las instancias de bot.

Los comandos pueden ser enviados al bot de diferentes formas:

- A través de la consola interactiva de ASF
- A través de Steam por chat privado/grupal
- A través de la interfaz **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)**

Ten en cuenta que la interacción con ASF requiere que seas elegible para el comando de acuerdo a los permisos de ASF. Para más detalles, revisa las propiedades de configuración `SteamUserPermissions` y `SteamOwnerID`.

Los comandos ejecutados por medio del chat de Steam son afectados por la **[propiedad de configuración global](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#commandprefix)** `CommandPrefix`, que por defecto es `!`. Esto significa que para ejecutar, por ejemplo, el comando `status`, debes escribir `!status` (o en su caso, el `CommandPrefix` de tu elección que hayas establecido). `CommandPrefix` no es obligatorio cuando se usa la consola o IPC y puede ser omitido.

* * *

### Consola interactiva

A partir de V4.0.0.9, ASF tiene soporte para una consola interactiva que puede ser activada configurando la propiedad [**`SteamOwnerID`**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#steamownerid). Después, simplemente pulsa la tecla `c` para activar el modo comando, escribe tu comando y confirma con Enter.

![Screenshot](https://i.imgur.com/bH5Gtjq.png)

La consola interactiva no está disponible en el modo [**`Headless`**](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#headless).

* * *

### Chat de Steam

También puedes ejecutar comandos a un bot dado a través del chat de Steam. Obviamente no puedes hablar contigo mismo directamente, por lo tanto necesitarás al menos una cuenta bot más para ejecutar comandos dirigidos a tu cuenta principal.

![Screenshot](https://i.imgur.com/IvFRJ5S.png)

De la misma manera, también puedes usar el chat grupal de un grupo de Steam. Ten en cuenta que esta opción requiere establecer correctamente la propiedad `SteamMasterClanID`, en cuyo caso el bot también escuchará comandos en el chat grupal (y se unirá si es necesario). Esto también puede usarse para "hablar contigo mismo" ya que no quiere una cuenta bot dedicada, contrario al chat privado. Puedes simplemente establecer la propiedad `SteamMasterClanID` a tu recién creado grupo, luego darte acceso a ti mismo ya sea con `SteamOwnerID` o `SteamUserPermissions` de tu bot. De esta manera, el bot (tú) se unirá al grupo y a su chat, y escuchará comandos de tu propia cuenta. Puedes unirte al mismo chat de grupo para enviarte comandos a ti mismo (ya que estarás enviando comandos a un chat de grupo, y la instancia de ASF en ese mismo chat los recibirá, incluso si se muestra como si solo tu cuenta estuviese ahí).

Por favor, ten en cuenta que enviar un comando al chat de grupo funciona como un relé. Si le dices `redeem X` a 3 de tus bots esperando junto contigo en el chat de grupo, el resultado será igual que si le dijeras `redeem X` a cada uno de ellos en privado. En la mayoría de los casos **esto no es lo que quieres**, y en cambio deberías usar un comando a un `bot dado` que sea enviado a **un solo bot en una ventana privada**. ASF soporta el chat de grupo, ya que en muchos casos puede ser útil para la comunicación con un único bot, pero casi nunca deberías ejecutar ningún comando en el chat de grupo si hay 2 o más bots unidos, a menos que entiendas completamente el comportamiento de ASF descrito aquí y que de hecho quieres enviar el mismo comando a cada bot que esté escuchando.

*E incluso en este caso deberías usar un chat privado con la sintaxis `<Bots>`.*

* * *

### IPC

La forma más avanzada y flexible de ejecutar comandos, perfecta para la interacción con el usuario (ASF-ui) así como con herramientas de terceros o con scripting (ASF API), requiere que ASF se ejecute en modo `IPC`, y un cliente que ejecute comandos a través de la interfaz **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)**.

![Screenshot](https://raw.githubusercontent.com/JustArchiNET/ASF-ui/master/.github/previews/commands.png)

* * *

## Comandos

| Comando                                                                    | Acceso             | Descripción                                                                                                                                                                                                                                                                      |
| -------------------------------------------------------------------------- | ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `2fa <Bots>`                                                         | `Master`           | Genera un código **[2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** temporal para determinadas instancias de bot.                                                                                                                          |
| `2fano <Bots>`                                                       | `Master`           | Rechaza todas las confirmaciones **[2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** pendientes para determinadas instancias de bot.                                                                                                        |
| `2faok <Bots>`                                                       | `Master`           | Acepta todas las confirmaciones **[2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** pendientes para determinadas instancias de bot.                                                                                                         |
| `addlicense <Bots> <GameIDs>`                                  | `Operador`         | Activa ciertas `appIDs` (Red de Steam) o `subIDs` (Tienda de Steam) en determinadas instancias de bot (solo juegos gratis).                                                                                                                                                      |
| `balance <Bots>`                                                     | `Master`           | Muestra el saldo de la cartera de instancias de bot determinadas.                                                                                                                                                                                                                |
| `bgr <Bots>`                                                         | `Master`           | Muestra información sobre la cola **[BGR](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer)** de instancias de bot determinadas.                                                                                                                    |
| `bl <Bots>`                                                          | `Master`           | Enlista los usuarios bloqueados del módulo de intercambio de instancias de bot determinadas.                                                                                                                                                                                     |
| `bladd <Bots> <SteamIDs64>`                                    | `Master`           | Bloquea ciertas `steamIDs` del módulo de intercambio de instancias de bot determinadas.                                                                                                                                                                                          |
| `blrm <Bots> <SteamIDs64>`                                     | `Master`           | Desbloquea determinadas `steamIDs` del módulo de intercambio de instancias de bot determinadas.                                                                                                                                                                                  |
| `exit`                                                                     | `Propietario`      | Detiene todo el proceso de ASF.                                                                                                                                                                                                                                                  |
| `farm <Bots>`                                                        | `Master`           | Reinicia el módulo de recolección de cromos para determinadas instancias de bot.                                                                                                                                                                                                 |
| `help`                                                                     | `PréstamoFamiliar` | Muestra la ayuda (enlace a esta página).                                                                                                                                                                                                                                         |
| `input <Bots> <Type> <Value>`                            | `Master`           | Establece un determinado tipo de entrada a un valor dado para instancias de bot determinadas, solo funciona en modo `Headless` - se explica con detalle **[debajo](#input-command)**.                                                                                            |
| `ib <Bots>`                                                          | `Master`           | Enlista las aplicaciones bloqueadas para recolección automática de instancias de bot determinadas.                                                                                                                                                                               |
| `ibadd <Bots> <AppIDs>`                                        | `Master`           | Añade ciertas `appIDs` a las aplicaciones bloqueadas para recolección automática de instancias de bot determinadas.                                                                                                                                                              |
| `ibrm <Bots> <AppIDs>`                                         | `Master`           | Remueve ciertas `appIDs` de las aplicaciones bloqueadas para recolección automática de instancias de bot determinadas.                                                                                                                                                           |
| `iq <Bots>`                                                          | `Master`           | Enlista la cola de prioridad de recolección de instancias de bot determinadas.                                                                                                                                                                                                   |
| `iqadd <Bots> <AppIDs>`                                        | `Master`           | Añade ciertas `appIDs` a la cola de prioridad de recolección de instancias de bot determinadas.                                                                                                                                                                                  |
| `iqrm <Bots> <AppIDs>`                                         | `Master`           | Remueve ciertas `appIDs` de la cola de prioridad de recolección de instancias de bot determinadas.                                                                                                                                                                               |
| `level <Bots>`                                                       | `Master`           | Muestra el nivel de la cuenta de Steam de instancias de bot determinadas.                                                                                                                                                                                                        |
| `loot <Bots>`                                                        | `Master`           | Envía todos los artículos de la comunidad de Steam establecidos en `LootableTypes` de instancias de bot determinadas al usuario definido como `Master` en su `SteamUserPermissions` (el que tenga el steamID más bajo si hay más de uno).                                        |
| `loot@ <Bots> <RealAppIDs>`                                    | `Master`           | Envía todos los artículos de la comunidad de Steam establecidos en `LootableTypes` que coincidan con ciertas `RealAppIDs` de instancias de bot determinadas al usuario definido como `Master` en su `SteamUserPermissions` (el que tenga el steamID más bajo si hay más de uno). |
| `loot^ <Bots> <AppID> <ContextID>`                       | `Master`           | Envía todos los artículos de Steam de cierta `AppID` en `ContextID` de instancias de bot determinadas al usuario definido como `Master` en su `SteamUserPermissions` (el que tenga el steamID más bajo si hay más de uno).                                                       |
| `nickname <Bots> <Nickname>`                                   | `Master`           | Cambia el nickname de Steam de instancias de bot determinadas a un `nickname` dado.                                                                                                                                                                                              |
| `owns <Bots> <AppIDsOrGameNames>`                              | `Operador`         | Comprueba si las instancias de bot dadas ya poseen ciertas `appIDs` y/o `gameNames` (puede ser parte del nombre del juego). También puede ser `*` para mostrar todos los juegos disponibles.                                                                                     |
| `password <Bots>`                                                    | `Master`           | Muestra la contraseña encriptada de instancias de bot determinadas (en uso junto con `PasswordFormat`).                                                                                                                                                                          |
| `pause <Bots>`                                                       | `Operador`         | Pausa permanentemente el módulo de recolección automática de cromos de instancias de bot determinadas. ASF no intentará recolectar la cuenta actual en esta sesión, a menos que la reanudes (`resume`) manualmente, o reinicies el proceso.                                      |
| `pause~ <Bots>`                                                      | `PréstamoFamiliar` | Pausa temporalmente el módulo de recolección automática de cromos de instancias de bot determinadas. La recolección se reanudará automáticamente en el siguiente evento de juego, o cuando se desconecte el bot. Puedes reanudar (`resume`) la recolección para despausar.       |
| `pause& <Bots> <Seconds>`                                  | `Operador`         | Pausa temporalmente el módulo de recolección automática de cromos de instancias de bot determinadas por una cantidad de segundos (`seconds`) dada. Después de ese tiempo, el módulo de recolección de cromos se reanuda automáticamente.                                         |
| `play <Bots> <AppIDs,GameName>`                                | `Master`           | Cambia a recolección manual - ejecuta ciertas `AppIDs` en instancias de bot determinadas, opcionalmente también con nombre de juego (`GameName`) personalizado. Usa `resume` para regresar a la recolección automática.                                                          |
| `privacy <Bots> <Settings>`                                    | `Master`           | Cambia la **[configuración de privacidad de Steam](https://steamcommunity.com/my/edit/settings)** de instancias de bot determinadas, a las opciones seleccionadas, explicadas **[debajo](#privacy-settings)**.                                                                   |
| `redeem <Bots> <Keys>`                                         | `Operador`         | Activa ciertas claves de producto o códigos de la cartera en instancias de bot determinadas.                                                                                                                                                                                     |
| `redeem^ <Bots> <Modes> <Keys>`                          | `Operador`         | Activa ciertas claves de producto o códigos de la cartera en instancias de bot determinadas, usando ciertos modos (`modes`) explicados **[debajo](#redeem-modes)**.                                                                                                              |
| `rejoinchat <Bots>`                                                  | `Operador`         | Fuerza a instancias de bot determinadas a unirse al chat de grupo de su `SteamMasterClanID`.                                                                                                                                                                                     |
| `restart`                                                                  | `Propietario`      | Reinicia el proceso de ASF.                                                                                                                                                                                                                                                      |
| `resume <Bots>`                                                      | `PréstamoFamiliar` | Reanuda la recolección automática de instancias de bot determinadas. También ve `pause`, `play`.                                                                                                                                                                                 |
| `start <Bots>`                                                       | `Master`           | Inicia instancias de bot determinadas.                                                                                                                                                                                                                                           |
| `stats`                                                                    | `Propietario`      | Muestra las estadísticas del proceso, como el uso de memoria.                                                                                                                                                                                                                    |
| `status <Bots>`                                                      | `PréstamoFamiliar` | Muestra el estatus de instancias de bot determinadas.                                                                                                                                                                                                                            |
| `stop <Bots>`                                                        | `Master`           | Detiene instancias de bot determinadas.                                                                                                                                                                                                                                          |
| `transfer <Bots> <TargetBot>`                                  | `Master`           | Envía todos los artículos de la comunidad de Steam establecidos en `TransferableTypes` de instancias de bot determinadas al bot destino.                                                                                                                                         |
| `transfer@ <Bots> <RealAppIDs> <TargetBot>`              | `Master`           | Envía todos los artículos de la comunidad de Steam establecidos en `TransferableTypes` que coincidan con ciertas `RealAppIDs` de instancias de bot determinadas al bot destino.                                                                                                  |
| `transfer^ <Bots> <AppID> <ContextID> <TargetBot>` | `Master`           | Envía todos los artículos de Steam de cierta `AppID` en `ContextID` de instancias de bot determinadas al bot destino.                                                                                                                                                            |
| `unpack <Bots>`                                                      | `Master`           | Abre todos los packs de refuerzo almacenados en el inventario de instancias de bot determinadas.                                                                                                                                                                                 |
| `update`                                                                   | `Propietario`      | Comprueba GitHub para actualizaciones de ASF (esto se hace automáticamente cada `UpdatePeriod`).                                                                                                                                                                                 |
| `version`                                                                  | `PréstamoFamiliar` | Muestra la versión de ASF.                                                                                                                                                                                                                                                       |

* * *

### Notas

Todas los comandos son insensibles a mayúsculas y minúsculas, pero sus argumentos (como nombres de bots) suelen ser sensibles a mayúsculas y minúsculas.

El argumento `<Bots>` es opcional en todos los comandos. Cuando se especifica, el comando se ejecuta en determinados bots. Cuando se omite, el comando se ejecuta en el bot actual que reciba el comando. En otras palabras, `status A` enviado al bot `B` es lo mismo que enviar `status` al bot `A`, pero el bot `B` en este caso actúa solo como un proxy.

El **acceso** del comando define el `EPermission` **mínimo** de `SteamUserPermissions` que se requiere para usar el comando, con la excepción de `Propietario` que se define en `SteamOwnerID` en el archivo de configuración global (y el permiso más alto disponible).

Los argumentos plurales, como `<Bots>`, `<Keys>` o `<AppIDs>` significan que el comando soporta múltiples argumentos de un tipo dado, separados por una coma. Por ejemplo, `status <Bots>` puede ser usado como `status MiBot,MiOtroBot,Principal`. Esta causará que el comando se ejecute en **todos los bots objetivo** de la misma forma que si enviaras `status` a cada bot en una ventana de chat separada. Por favor, ten en cuenta que no hay espacio después de `,`.

ASF usa todos los caracteres en blanco como posibles delimitadores para un comando, como espacio y caracteres de línea nueva. Esto significa que no tienes que usar espacio para delimitar tus argumentos, puedes usar cualquier otro caracter en blanco (como tabulador o línea nueva).

ASF "unirá" argumentos adicionales fuera de rango a tipo plural del último argumento en rango. Esto significa que `redeem bot key1 key2 key3` para `redeem <Bots><Keys>` funcionará exactamente igual que `redeem bot key1,key2,key3`. Together with accepting newline as command delimiter, this makes it possible for you to write `redeem bot` then paste a list of keys separated by any acceptable delimiter character (such as newline), or standard `,` ASF delimiter. Ten en cuenta que este truco solo puede ser usado para variantes de comandos que usan la mayor cantidad de argumentos (así que especificar `<Bots>` es obligatorio en este caso).

As you've read above, a space character is being used as a delimiter for a command, therefore it can't be used in arguments. However, also as stated above, ASF can join out-of-range arguments, which means that you're actually able to use a space character in argument that is defined as a last one for given command. For example, `nickname bob Great Bob` will properly set nickname of `bob` bot to "Great Bob". In the similar way you can check names containing spaces in `owns` command.

* * *

Some commands are also available with their aliases, to save you on typing:

| Comando      | Alias |
| ------------ | ----- |
| `owns ASF`   | `oa`  |
| `status ASF` | `sa`  |
| `redeem`     | `r`   |
| `redeem^`    | `r^`  |

* * *

### Argumento `<Bots>`

El argumento `<Bots>` es una variante especial de los argumento plurales, ya que además de aceptar múltiples valores también ofrece funcionalidad extra.

En primer lugar, hay una palabra clave `ASF` que actúa como "todos los bots en el proceso", así que el comando `status ASF` es igual a `status todos,tus,bots,enlistados,aquí`. Esto también puede utilizarse para identificar fácilmente los bots a los que tienes acceso, ya que la palabra clave `ASF`, a pesar de apuntar a todos los bots, solo tendrá respuesta de aquellos bots a los que realmente puedes enviar el comando.

El argumento `<Bots>` soporta una sintaxis especial de rango, que te permite elegir un rango de bots más fácilmente. La sintaxis general para `<Bots>` en este caso es `primerBot..últimoBot`. Por ejemplo, si tienes bots llamados `A, B, C, D, E, F`, puedes ejecutar `status B..E`, lo que es igual a `status B,C,D,E` en este caso. Al usar esta sintaxis, ASF utilizará la ordenación alfabética para determinar qué bots están en tu rango especificado. Tanto `primerBot` como `últimoBot` debes ser nombres de bots reconocidos por ASF, de lo contrario la sintaxis de rango se omite completamente.

Además de la sintaxis de rango, el argumento `<Bots>` también soporta emparejamiento **[regex](https://en.wikipedia.org/wiki/Regular_expression)**. Puede activar el patrón regex usando `r!<pattern>` como nombre de bot, donde `r!` es el activador para el emparejamiento regex, y `<pattern>` es tu patrón regex. Un ejemplo de comando basado en regex sería `status r!\d{3}` que enviará el comando `status` a los bots que tengan un nombre formado por 3 dígitos (por ejemplo `123` y `981`). No dudes en echar un vistazo a la **[documentación](https://docs.microsoft.com/en-us/dotnet/standard/base-types/regular-expression-language-quick-reference)** para mayor información y más ejemplos de patrones regex disponibles.

* * *

## Ajustes `privacy`

`<Settings>` argument accepts **up to 7** different options, separated as usual with standard comma ASF delimiter. Those are, in order:

| Argumento | Nombre         | Hijo de    |
| --------- | -------------- | ---------- |
| 1         | Profile        |            |
| 2         | OwnedGames     | Profile    |
| 3         | Playtime       | OwnedGames |
| 4         | FriendsList    | Profile    |
| 5         | Inventory      | Profile    |
| 6         | InventoryGifts | Inventory  |
| 7         | Comments       | Profile    |

For description of above fields, please visit **[Steam privacy settings](https://steamcommunity.com/my/edit/settings)**.

While valid values for all of them are:

| Valor | Nombre        |
| ----- | ------------- |
| 1     | `Private`     |
| 2     | `FriendsOnly` |
| 3     | `Public`      |

You can use either a case-insensitive name, or a numeric value. Arguments that were omitted will default to being set to `Private`. It's important to note relation between child and parent of arguments specified above, as child can never have more open permission than its parent. For example, you **can't** have `Public` games owned while having `Private` profile.

### Ejemplo

If you want to set **all** privacy settings of your bot named `Main` to `Private`, you can use either of below:

    privacy Main 1
    privacy Main Private
    

This is because ASF will automatically assume all other settings to be `Private`, so there is no need to input them. On the other hand, if you'd like to set all privacy settings to `Public`, then you should use any of below:

    privacy Main 3,3,3,3,3,3,3
    privacy Main Public,Public,Public,Public,Public,Public,Public
    

This way you can also set independent options however you like:

    privacy Main Public,FriendsOnly,Private,Public,Public,Private,Public
    

The above will set profile to public, owned games to friends only, playtime to private, friends list to public, inventory to public, inventory gifts to private and profile comments to public. You can achieve the same with numeric values if you want to.

Remember that child can never have more open permission than its parent. Refer to arguments relationship for available options.

* * *

## Modos `redeem^`

`redeem^` command allows you to fine-tune modes that will be used for one single redeem scenario. This works as temporary override of `RedeemingPreferences` **[bot config property](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#bot-config)**.

`<Modes>` argument accepts multiple mode values, separated as usual by a comma. Available mode values are specified below:

| Valor | Nombre                | Descripción                                                           |
| ----- | --------------------- | --------------------------------------------------------------------- |
| FD    | ForceDistributing     | Forces `Distributing` redeeming preference to be enabled              |
| FF    | ForceForwarding       | Forces `Forwarding` redeeming preference to be enabled                |
| FKMG  | ForceKeepMissingGames | Forces `KeepMissingGames` redeeming preference to be enabled          |
| SD    | SkipDistributing      | Forces `Distributing` redeeming preference to be disabled             |
| SF    | SkipForwarding        | Forces `Forwarding` redeeming preference to be disabled               |
| SI    | SkipInitial           | Skips key redemption on initial bot                                   |
| SKMG  | SkipKeepMissingGames  | Forces `KeepMissingGames` redeeming preference to be disabled         |
| V     | Validate              | Validates keys for proper format and automatically skips invalid ones |

For example, we'd like to redeem 3 keys on any of our bots that don't own games yet, but not our `primary` bot. For achieving that we can use:

`redeem^ primary FF,SI key1,key2,key3`

It's important to note that advanced redeem overrides only those `RedeemingPreferences` that you **specify in the command**. For example, if you've enabled `Distributing` in your `RedeemingPreferences` then there will be no difference whether you use `FD` mode or not, because distributing will be already active regardless, due to `RedeemingPreferences` that you use. This is why each forcibly enabled override also has a forcibly disabled one, you can decide yourself if you prefer to override disabled with enabled, or vice versa.

* * *

## Comando `input`

Input command can be used only in `Headless` mode, for inputting given data via **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** or Steam chat when ASF is running without support for user interaction.

General syntax is `input <Bots> <Type> <Value>`.

`<Type>` is case-insensitive and defines input type recognized by ASF. Currently ASF recognizes following types:

| Tipo                    | Descripción                                                                |
| ----------------------- | -------------------------------------------------------------------------- |
| DeviceID                | 2FA device identificator, if missing from `.maFile`.                       |
| Login                   | `SteamLogin` bot config property, if missing from config.                  |
| Contraseña              | `SteamPassword` bot config property, if missing from config.               |
| SteamGuard              | Auth code sent on your e-mail if you're not using 2FA.                     |
| SteamParentalCode       | `SteamParentalCode` bot config property, if missing from config.           |
| TwoFactorAuthentication | 2FA token generated from your mobile, if you're using 2FA but not ASF 2FA. |

`<Value>` is value set for given type. Currently all values are strings.

### Ejemplo

Let's say that we have a bot that is protected by SteamGuard in non-2FA mode. We want to launch that bot with `Headless` set to true.

In order to do that, we need to execute following commands:

`start MySteamGuardBot` -> Bot will attempt to log in, fail due to AuthCode needed, then stop due to running in `Headless` mode. We need this in order to make Steam network send us auth code on our e-mail - if there was no need for that, we'd skip this step entirely.

`input MySteamGuardBot SteamGuard ABCDE` -> We set `SteamGuard` input of `MySteamGuardBot` bot to `ABCDE`. Of course, `ABCDE` in this case is auth code that we got on our e-mail.

`start MySteamGuardBot` -> We start our (stopped) bot again, this time it automatically uses auth code that we set in previous command, properly logging in, then clearing it.

In the same way we can access 2FA-protected bots (if they're not using ASF 2FA), as well as setting other required properties during runtime.