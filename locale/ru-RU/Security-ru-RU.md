# Безопасность

## Шифрование

ASF на данный момент поддерживает следующие методы шифрования, как значения `ECryptoMethod`:

| Значение | Имя                         |
| -------- | --------------------------- |
| 0        | PlainText                   |
| 1        | AES                         |
| 2        | ProtectedDataForCurrentUser |

Их полное описание и сравнение вы найдёте ниже.

Для создания зашифрованного пароля, например для использования в `SteamPassword`, вам нужно выполнить **[команду](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-ru-RU)** `encrypt` с соответствующим типом шифрования и вашим паролем в виде открытого текста. После этого, установите полученную зашифрованную строку в качестве значения для параметра `SteamPassword`, и, наконец, измените значение параметра `PasswordFormat` на соответствующее вашему методу шифрования.

* * *

### PlainText

Это самый простой и небезопасный способ хранения пароля, который определён как `ECryptoMethod` равный `0`. ASF ожидает что строка будет в виде открытого текста - пароль в его явном виде. Это самый простой в использовании, и 100% совместимый со всеми конфигурациями, а потому и используемый по умолчанию способ хранения секретных данных, совершенно небезопасный для хранения.

* * *

### AES

Считающийся безопасным по современным стандартам, способ хранения пароля **[AES](https://ru.wikipedia.org/wiki/Advanced_Encryption_Standard)** определён как `ECryptoMethod` равный `1`. ASF ожидает что строка будет **[base64-кодированнаой](https://ru.wikipedia.org/wiki/Base64)** последовательностью символов, которая после декодирования даст AES-зашифрованный массив байт, который можно будет расшифровать используя **[вектор инициализации](https://en.wikipedia.org/wiki/Initialization_vector)** и ключ шифрования ASF.

Описанный выше метод гарантирует безопасность при условии что атакующему неизвестен встроенный ключ AES-шифрования, используемый для для шифрования и расшифровки паролей. ASF позволяет вам задать ключ с помощью **[аргумента командной строки](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-Line-Arguments-ru-RU)** `--cryptkey`, который вам следует использовать для максимальной безопасности. Если вы решите опустить его, ASF будет использовать свой собственный ключ, который **известен** и жёстко запрограммирован в приложении, и кто угодно может расшифровать пароль зашифрованный ASF. Это всё равно требует некоторых усилий и не так просто сделать, однако это возможно, и поэтому вам всегда следует использовать шифрование `AES` со своим собственным `--cryptkey`, который вы храните в секрете. Метод шифрования AES, используемый в ASF, предоставляет безопасность, которая должна удовлетворить желание сбалансировать простоту `PlainText` и сложность `ProtectedDataForCurrentUser`, однако настоятельно рекомендуется использовать его с пользовательским `--cryptkey`. При правильном использовании гарантирует приемлемую безопасность хранения.

* * *

### ProtectedDataForCurrentUser

На данный момент самый безопасный метод хранения пароля, представленный в ASF, и гораздо более безопасный чем `AES`, описанный выше, определён как `ECryptoMethod` равный `2`. Главное достоинство этого метода это одновременно и главный его недостаток - вместо использования ключа шифрования (как в методе `AES`) данные шифруются с помощью учетных данных пользователя, вошедшего в систему, а это значит что расшифровка данных возможна **только** на той же машине где они зашифрованы, и в добавок к этому **только** тем пользователем, который выполнил шифрование. Это гарантирует, что даже если вы отправите весь файл `Bot.json` с зашифрованным этим методом `SteamPassword` кому-то ещё - он не сможет расшифровать пароль без доступа к вашему ПК. Это прекрасная мера безопасности, но одновременно имеет серьёзный недостаток, заключающийся в наименьшей совместимости, поскольку зашифрованный этим методом пароль будет несовместим с любым другим пользователем или машиной, включая **вашу собственную**, если вы решите, например, переустановить операционную систему. Однако это всё же самый лучший метод хранения паролей, и если вы беспокоитесь о безопасности `PlainText`, и не хотите вводить пароль каждый раз, то это наилучший вариант для вас, конечно, если вам не нужно иметь доступ к конфигурационным файлам с любой машины кроме собственной.

**Пожалуйста, обратите внимание, на данный момент эта опция доступна только на машинах под управлением ОС Windows.**

* * *

## Рекомендации

Если вам не важна совместимость, и вас устраивает как работает метод `ProtectedDataForCurrentUser`, то это **рекомендуемый** вариант хранения пароля в ASF, поскольку он обеспечивает наилучшую безопасность. Метод `AES` это хороший выбор для людей, которые хотят использовать свои конфигурационные файлы на любой машине, а `PlainText` это самый простой метод хранения пароля, если вы не против что любой может посмотреть его в файле JSON.

Не забывайте, что все эти 3 метода считаются **небезопасными** если у атакующего есть доступ к вашему ПК. ASF должен иметь возможность расшифровать зашифрованные пароли, а если одна программа работающая на машине может это сделать, то любая программа работающая на той же машине тоже способна это сделать. `ProtectedDataForCurrentUser` это самый безопасный вариант, поскольку **даже другой пользователь на том же ПК не сможет его расшифровать**, но даже его можно расшифровать если кто-то украдёт ваши учётные данные и информацию о машине в добавок к конфигурационным файлам ASF.

В дополнение к методам шифрования, описанным выше, также возможно избежать указания пароля вообще, например использовав пустую строку или значение `null` в `SteamPassword`. ASF при необходимости запросит ваш пароль, и не будет его нигде сохранять, только хранить в памяти запущенного процесса, пока вы его не закроете. Хотя это самый безопасный метод работы с паролями (они нигде не сохраняются), но и самый неудобный, поскольку вам нужно вручную вводить пароль при каждом запуске ASF (когда он требуется). Если для вас это не проблема, это самый лучший вариант с точки зрения безопасности.

* * *

## Расшифровка

ASF не содержит никаких средств для расшифровки уже зашифрованных паролей, поскольку методы расшифровки используются только для внутреннего доступа к данным внутри процесса. Если вы хотите отменить процедуру шифрования, например для переноса ASF на другую машину при использовании `ProtectedDataForCurrentUser`, просто повторите процедуру с самого начала в новом окружении.

* * *

## Хэширование

ASF currently supports the following hashing methods as a definition of `EHashingMethod`:

| Значение | Имя       |
| -------- | --------- |
| 0        | PlainText |
| 1        | SCrypt    |
| 2        | Pbkdf2    |

Их полное описание и сравнение вы найдёте ниже.

In order to generate a hash, e.g. for `IPCPassword` usage, you should execute `hash` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** with the appropriate hashing method that you chose and your original plain-text password. Afterwards, put the hashed string that you've got as `IPCPassword` ASF config property, and finally change `IPCPasswordFormat` to the one that matches your chosen encryption method.

* * *

### PlainText

This is the most simple and insecure way of hashing a password, defined as `EHashingMethod` of `0`. ASF will generate hash matching the original input. Это самый простой в использовании, и 100% совместимый со всеми конфигурациями, а потому и используемый по умолчанию способ хранения секретных данных, совершенно небезопасный для хранения.

* * *

### SCrypt

Considered secure by today standards, **[SCrypt](https://en.wikipedia.org/wiki/Scrypt)** way of hashing the password is defined as `EHashingMethod` of `1`. ASF will use the `SCrypt` implementation using `8` blocks, `8192` iterations, `32` hash length and encryption key as a salt to generate the array of bytes. The resulting bytes will then be encoded as **[base64](https://en.wikipedia.org/wiki/Base64)** string.

ASF allows you to specify salt for this method via `--cryptkey` **[command-line argument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-Line-Arguments)**, which you should use for maximum security. If you decide to omit it, ASF will use its own key which is **known** and hardcoded into the application, meaning hashing will be less secure. При правильном использовании гарантирует приемлемую безопасность хранения.

* * *

### Pbkdf2

Considered weak by today standards, **[Pbkdf2](https://en.wikipedia.org/wiki/PBKDF2)** way of hashing the password is defined as `EHashingMethod` of `2`. ASF will use the `Pbkdf2` implementation using `10000` iterations, `32` hash length and encryption key as a salt, with `SHA-256` as a hmac algorithm to generate the array of bytes. The resulting bytes will then be encoded as **[base64](https://en.wikipedia.org/wiki/Base64)** string.

ASF allows you to specify salt for this method via `--cryptkey` **[command-line argument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-Line-Arguments)**, which you should use for maximum security. If you decide to omit it, ASF will use its own key which is **known** and hardcoded into the application, meaning hashing will be less secure.

* * *

## Рекомендации

If you'd like to use a hashing method for storing some secrets, such as `IPCPassword`, we recommend to use `SCrypt` with custom salt, as it provides a very decent security against brute-forcing attempts. `Pbkdf2` is offered only for compatibility reasons, mainly because we already have a working (and needed) implementation of it for other use cases across Steam platform (e.g. parental pins). It's still considered secure, but weak compared to alternatives (e.g. `SCrypt`).