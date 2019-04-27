# Activador de juegos en segundo plano

El activador de juegos en segundo plano es una función especial de ASF que permite importar una lista de Steam keys(junto con sus nombres) para ser activados en segundo plano. Esto es especialmente útil si tienes una gran cantidad de keys para activar y te garantiza llegar a tu `RateLimited` **[status](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ#what-is-the-meaning-of-status-when-redeeming-a-key)** antes de que termines con tu lote entero.

El activador de juegos en segundo plano esta hecho para tener un solo bot de único propósito, por lo que este no hace uso de las `RedeemingPreferences`. Esta función puede usarse junto con (o en vez de) `redeem` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** de ser necesario.

* * *

## Importar

El proceso de importación puede hacerse de dos maneras - ya sea a través de un archivo o por IPC.

### Archivo

ASF reconocerá en su directorio de `config` un archivo llamado `BotName.keys` donde `BotName` es el nombre de tu bot. That file has expected and fixed structure of name of the game with cd-key, separated from each other by a tab character and ending with a newline to indicate the next entry. Si varios tabs son usados, entonces el primer espacio se considera el nombre del juego y el último espacio es la key y todo lo que esté en medio sera ignorado. Por ejemplo:

    POSTAL 2    ABCDE-EFGHJ-IJKLM
    Domino Craft VR 12345-67890-ZXCVB
    A Week of Circus Terror POIUY-KJHGD-QWERT
    Terraria    EstoSeIgnora   EstoSeIgnoraTambién    ZXCVB-ASDFG-QWERT
    

Alternatively, you're also able to use keys only format (still with a newline between each entry). ASF in this case will use Steam's response (if possible) to fill the right name. For any kind of keys tagging, we recommend that you name your keys yourself, as packages being redeemed on Steam do not have to follow logic of games that they're activating, so depending on what the developer has put, you may see correct game names, custom package names (e.g. Humble Indie Bundle 18) or outright wrong and potentially even malicious ones (e.g. Half-Life 4).

    ABCDE-EFGHJ-IJKLM
    12345-67890-ZXCVB
    POIUY-KJHGD-QWERT
    ZXCVB-ASDFG-QWERT
    

Regardless which format you've decided to stick with, ASF will import your `keys` file, either on bot startup, or later during execution. Después de revisar el archivo y eventualmente omitir las entradas inválidas, todos los juegos detectados serán añadidos a una cola en segundo plano, y el archivo `BotName.keys` será removido del directorio de `config`.

### IPC

Además de poder usar el archivo antes mencionado, ASF también posee `GamesToRedeemInBackground` **[ASF API endpoint](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#asf-api)** que se puede ejecutar desde cualquier herramienta IPC, incluyendo nuestro ASF-ui. Using IPC could be more powerful, as you can do appropriate parsing yourself, such as using a custom delimiter instead of being forced to a tab character, or even introducing your entirely own customized keys structure.

* * *

## Cola

Una vez los juegos sean importados exitosamente, seran añadidos a la cola. ASF recorrerá automáticamente dicha cola en segundo plano mientras que el bot este conectado a la red de Steam y la cola no este vacía. Cuando una key se intente activar y no resulte en `RateLimited`, esta será removida de la cola, con el resultado de dicho intento escrito en un archivo en el directorio `config` - ya sea `BotName.keys.used` si la key fue usada en el proceso (por ejemplo `NoDetail`, `BadActivationCode`, `DuplicateActivationCode`) o `BotName.keys.unused` en caso contrario. ASF usará el nombre que hayas proveido, esto debido a que no es posible garantizar que la red de Steam retorne un nombre significativo a la hora de activar una key - de esta manera puedes etiquetar tus keys usando nombres personalizados en caso de ser necesario o si el usuario así lo desea.

Si durante el proceso la cuenta llega a su `RateLimited`, la cola sera temporalmente suspendida por una hora para esperar que el limite de activación desaparezca. Cuando este limite desaparezca, el proceso continuará, hasta que la cola quede completamente vacía.

* * *

## Ejemplo

Supongamos que tienes una lista de 100 keys. Primeramente debes crear un nuevo archivo `BotName.keys.new` en el directorio `config` de ASF. Le agregamos la extensión `.new` para que ASF no intente usar este archivo de inmediato en el momento que es creado (ya que el mismo está en blanco y no esta listo para ser importado aún).

Ahora puedes abrir el archivo y pegar las 100 keys dentro del mismo, arreglando el formato de la lista si es necesario. Después de arreglar nuestro archivo `BotName.keys.new` tendremos exactamente 100 (o 101, incluyendo el último salto de linea) lineas, cada uno con un estructura de `GameName\tcd-key\n` donde `\t` es un tab y `\n` es un salto de linea.

Ahora puedes cambiarle el nombre al archivo de `BotName.keys.new` a `BotName.keys` para que ASF sepa que está listo para ser leído. En el momento en que hagas esto, ASF automáticamente importará el archivo (sin necesidad de reiniciar) y posteriormente lo borrará, confirmado que todos los juegos fueron leidos y agregados a la cola.

En vez de usar el archivo `BotName.keys`, también puedes usar la función del IPC, o incluso combinar ambas si así lo desea.

After some time, `BotName.keys.used` and `BotName.keys.unused` files will be generated. Esos archivos contienen el resultado del proceso de activación de las keys. Por ejemplo, puedes cambiar el nombre del archivo `BotName.keys.unused` a `BotName2.keys` y pasarle las keys que no fueron usadas a otro bot, ya que el anterior no las utilizó. O simplemente puedes copiar y pegar las keys sin usar a algún otro archivo y guardarlas para después, como gustes. Recuerda que ASF recorre la lista y aquellas keys que se agreguen durante el proceso serán añadidas a las archivos `used` y `unused`, aún asi se recomienda esperar a que la lista quede completamente vacía antes de querer agregar más. If you absolutely must access those files before queue is fully emptied, you should firstly **move** output file you want to access to some other directory, **then** parse it. This is because ASF can append some new results while you're doing your thing, and that could potentially lead to loss of some keys if you read a file having e.g. 3 keys inside, then delete it, totally missing the fact that ASF added 4 other keys to your removed file in the meantime. If you want to access those files, ensure to move them away from ASF `config` directory before reading them, for example by rename.

It's also possible to add extra games to import while having some games already in our queue, by repeating all above steps. ASF will properly add our extra entries to already-ongoing queue and deal with it eventually.

* * *

## Remarks

Background keys redeemer uses `OrderedDictionary` under the hood, which means that your cd-keys will have preserved order as they were specified in the file (or IPC API call). This means that you can (and should) provide a list where given cd-key can only have direct dependencies on cd-keys listed above, but not below. For example, this means that if you have DLC `D` that requires game `G` to be activated firstly, then cd-key for game `G` should **always** be included before cd-key for DLC `D`. Likewise, if DLC `D` would have dependencies on `A`, `B` and `C`, then all 3 should be included before (in any order, unless they have dependencies on their own).

Not following the scheme above will result in your DLC not being activated with `DoesNotOwnRequiredApp`, even if your account would be eligible for activating it after going through its entire queue. If you want to avoid that then you must make sure that your DLC is always included after the base game in your queue.