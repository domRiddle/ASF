# Seguridad

## Encryption

ASF currently supports the following encryption mechanisms:

| Valor | Nombre                      |
| ----- | --------------------------- |
| 0     | PlainText                   |
| 1     | AES                         |
| 2     | ProtectedDataForCurrentUser |

The exact description and comparison of them is available below.

In order to generate encrypted password, e.g. for `SteamPassword` usage, you should execute `encrypt` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** with the appropriate encryption that you chose and your original plain-text password. Afterwards, put the encrypted string that you've got as `SteamPassword` bot config property, and finally change `PasswordFormat` to the one that matches your chosen encryption method.

* * *

### PlainText

This is the most simple and insecure way of storing a password, defined as `ECryptoMethod` of `0`. ASF expects the string to be a plain text - a password in its direct form. It's the easiest one to use, and 100% compatible with all the setups, therefore it's a default way of storing secrets, totally insecure for safe storage.

* * *

### AES

Considered secure by today standards, **[AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)** way of storing the password is defined as `ECryptoMethod` of `1`. ASF expects the string to be a **[base64-encoded](https://en.wikipedia.org/wiki/Base64)** sequence of characters resulting in AES-encrypted byte array after translation, which then should be decrypted using included **[initialization vector](https://en.wikipedia.org/wiki/Initialization_vector)** and ASF encryption key.

El método anterior garantiza la seguridad siempre que el atacante no conozca la clave de encriptación integrada en ASF, la cual es usada tanto para la desencriptación como para la encriptación de contraseñas. ASF te permite especificar la clave a través del **[argumento de la línea de comandos](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-Line-Arguments-es-es)** `--cryptkey`, el cual deberías usar para máxima seguridad. Si decides omitirlo, ASF usará su propia clave que es **conocida** y está en el código de la aplicación, lo que significa que cualquiera puede revertir la encriptación de ASF y obtener la contraseña desencriptada. Requiere algo de esfuerzo y no es tan fácil de hacer, pero es posible, por eso casi siempre debes usar la encriptación `AES` con tu propia `--cryptkey` que es mantenida en secreto. El método AES usado en ASF proporciona seguridad que debería ser satisfactoria y tiene un balance entre la simplicidad de `PlainText` y la complejidad de `ProtectedDataForCurrentUser`, pero se recomienda mucho usarlo con una `--cryptkey` personalizada. If used properly, guarantees decent security for safe storage.

* * *

### ProtectedDataForCurrentUser

Currently the most secure way of encrypting the password that ASF offers, and much safer than `AES` method explained above, is defined as `ECryptoMethod` of `2`. La ventaja principal de este método es al mismo tiempo la mayor desventaja - en lugar de usar una clave de encriptación (como en `AES`), la información es encriptada usando las credenciales de inicio de sesión del usuario que actualmente tiene la sesión iniciada, lo que significa que es posible desencriptar la información **solo** en la máquina en la que fue encriptada, y además **solo** por el usuario que hizo la encriptación. This ensures that even if you send your entire `Bot.json` with encrypted `SteamPassword` using this method to somebody else, he will not be able to decrypt the password without direct access to your PC. This is excellent security measure, but at the same time has a major disadvantage of being least compatible, as the password encrypted using this method will be incompatible with any other user as well as machine - including **your own** if you decide to e.g. reinstall your operating system. Still, it's one of the best methods of storing passwords, and if you're worried about security of `PlainText`, and don't want to put password each time, then this is your best bet as long as you don't have to access your configs from any other machine than your own.

**Please note that this option is available only for machines running Windows OS as of now.**

* * *

## Recomendación

Si la compatibilidad no es un problema para ti, y estás bien con cómo funciona el método `ProtectedDataForCurrentUser`, es la opción **recomendada** para almacenar la contraseña en ASF, ya que proporciona la mejor seguridad. El método `AES` es una buena opción para las personas que quieren usar sus configuraciones en cualquiera máquina que quieran, mientras que `PlainText` es la forma más simple de almacenar la contraseña, si no te importa que cualquiera pueda buscarla en la configuración JSON.

Por favor, ten en cuenta que los 3 se consideran **insecure** si el atacante tiene acceso a tu PC. ASF must be able to decrypt the encrypted passwords, and if the program running on your machine is capable of doing that, then any other program running on the same machine will be capable of doing so, too. `ProtectedDataForCurrentUser` es la variante más segura ya que **incluso otro usuario usando la misma PC no podrá desencriptarla**, pero aún es posible desencriptar la información si alguien es capaz de robar tus credenciales de acceso e información de tu máquina además del archivo de configuración de ASF.

In addition to encryption methods specified above, it's possible to also avoid specifying passwords entirely, for example as `SteamPassword` by using an empty string or `null` value. ASF will ask you for your password when it's required, and won't save it anywhere but keep in memory of currently running process, until you close it. While being the most secure method of dealing with passwords (they're not saved anywhere), it's also the most troublesome as you need to enter your password manually on each ASF run (when it's required). Si eso no es un problema para ti, esta es tu mejor apuesta en lo que se refiere a seguridad.

* * *

# Desencriptación

ASF no soporta ninguna forma de desencriptar contraseñas ya encriptadas, ya que los métodos de desencriptación solo son usados internamente para acceder a la información dentro del proceso. If you want to revert encryption procedure e.g. for moving ASF to other machine when using `ProtectedDataForCurrentUser`, then simply repeat the procedure from beginning in the new environment.