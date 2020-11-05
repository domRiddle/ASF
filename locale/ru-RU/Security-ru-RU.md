# Безопасность

## Encryption

ASF currently supports the following encryption mechanisms:

| Значение | Имя                         |
| -------- | --------------------------- |
| 0        | PlainText                   |
| 1        | AES                         |
| 2        | ProtectedDataForCurrentUser |

The exact description and comparison of them is available below.

In order to generate encrypted password, e.g. for `SteamPassword` usage, you should execute `encrypt` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** with the appropriate encryption that you chose and your original plain-text password. Afterwards, put the encrypted string that you've got as `SteamPassword` bot config property, and finally change `PasswordFormat` to the one that matches your chosen encryption method.

* * *

### PlainText

This is the most simple and insecure way of storing a password, defined as `ECryptoMethod` of `0`. ASF expects the string to be a plain text - a password in its direct form. It's the easiest one to use, and 100% compatible with all the setups, therefore it's a default way of storing secrets, totally insecure for safe storage.

* * *

### AES

Considered secure by today standards, **[AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)** way of storing the password is defined as `ECryptoMethod` of `1`. ASF expects the string to be a **[base64-encoded](https://en.wikipedia.org/wiki/Base64)** sequence of characters resulting in AES-encrypted byte array after translation, which then should be decrypted using included **[initialization vector](https://en.wikipedia.org/wiki/Initialization_vector)** and ASF encryption key.

Описанный выше метод гарантирует безопасность при условии что атакующему неизвестен встроенный ключ AES-шифрования, используемый для для шифрования и расшифровки паролей. ASF позволяет вам задать ключ с помощью **[аргумента командной строки](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-Line-Arguments-ru-RU)** `--cryptkey`, который вам следует использовать для максимальной безопасности. Если вы решите опустить его, ASF будет использовать свой собственный ключ, который **известен** и жёстко запрограммирован в приложении, и кто угодно может расшифровать пароль зашифрованный ASF. Это всё равно требует некоторых усилий и не так просто сделать, однако это возможно, и поэтому вам всегда следует использовать шифрование `AES` со своим собственным `--cryptkey`, который вы храните в секрете. Метод шифрования AES, используемый в ASF, предоставляет безопасность, которая должна удовлетворить желание сбалансировать простоту `PlainText` и сложность `ProtectedDataForCurrentUser`, однако настоятельно рекомендуется использовать его с пользовательским `--cryptkey`. If used properly, guarantees decent security for safe storage.

* * *

### ProtectedDataForCurrentUser

Currently the most secure way of encrypting the password that ASF offers, and much safer than `AES` method explained above, is defined as `ECryptoMethod` of `2`. Главное достоинство этого метода это одновременно и главный его недостаток - вместо использования ключа шифрования (как в методе `AES`) данные шифруются с помощью учетных данных пользователя, вошедшего в систему, а это значит что расшифровка данных возможна **только** на той же машине где они зашифрованы, и в добавок к этому **только** тем пользователем, который выполнил шифрование. This ensures that even if you send your entire `Bot.json` with encrypted `SteamPassword` using this method to somebody else, he will not be able to decrypt the password without direct access to your PC. This is excellent security measure, but at the same time has a major disadvantage of being least compatible, as the password encrypted using this method will be incompatible with any other user as well as machine - including **your own** if you decide to e.g. reinstall your operating system. Still, it's one of the best methods of storing passwords, and if you're worried about security of `PlainText`, and don't want to put password each time, then this is your best bet as long as you don't have to access your configs from any other machine than your own.

**Please note that this option is available only for machines running Windows OS as of now.**

* * *

## Рекомендации

Если вам не важна совместимость, и вас устраивает как работает метод `ProtectedDataForCurrentUser`, то это **рекомендуемый** вариант хранения пароля в ASF, поскольку он обеспечивает наилучшую безопасность. Метод `AES` это хороший выбор для людей, которые хотят использовать свои конфигурационные файлы на любой машине, а `PlainText` это самый простой метод хранения пароля, если вы не против что любой может посмотреть его в файле JSON.

Не забывайте, что все эти 3 метода считаются **небезопасными** если у атакующего есть доступ к вашему ПК. ASF must be able to decrypt the encrypted passwords, and if the program running on your machine is capable of doing that, then any other program running on the same machine will be capable of doing so, too. `ProtectedDataForCurrentUser` это самый безопасный вариант, поскольку **даже другой пользователь на том же ПК не сможет его расшифровать**, но даже его можно расшифровать если кто-то украдёт ваши учётные данные и информацию о машине в добавок к конфигурационным файлам ASF.

In addition to encryption methods specified above, it's possible to also avoid specifying passwords entirely, for example as `SteamPassword` by using an empty string or `null` value. ASF will ask you for your password when it's required, and won't save it anywhere but keep in memory of currently running process, until you close it. While being the most secure method of dealing with passwords (they're not saved anywhere), it's also the most troublesome as you need to enter your password manually on each ASF run (when it's required). Если для вас это не проблема, это самый лучший вариант с точки зрения безопасности.

* * *

# Расшифровка

ASF не содержит никаких средств для расшифровки уже зашифрованных паролей, поскольку методы расшифровки используются только для внутреннего доступа к данным внутри процесса. If you want to revert encryption procedure e.g. for moving ASF to other machine when using `ProtectedDataForCurrentUser`, then simply repeat the procedure from beginning in the new environment.