# Sicherheit

## Encryption

ASF currently supports the following encryption methods as a definition of `ECryptoMethod`:

| Wert | Name                        |
| ---- | --------------------------- |
| 0    | PlainText                   |
| 1    | AES                         |
| 2    | ProtectedDataForCurrentUser |

The exact description and comparison of them is available below.

In order to generate encrypted password, e.g. for `SteamPassword` usage, you should execute `encrypt` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** with the appropriate encryption that you chose and your original plain-text password. Afterwards, put the encrypted string that you've got as `SteamPassword` bot config property, and finally change `PasswordFormat` to the one that matches your chosen encryption method.

* * *

### PlainText

This is the most simple and insecure way of storing a password, defined as `ECryptoMethod` of `0`. ASF expects the string to be a plain text - a password in its direct form. It's the easiest one to use, and 100% compatible with all the setups, therefore it's a default way of storing secrets, totally insecure for safe storage.

* * *

### AES

Considered secure by today standards, **[AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)** way of storing the password is defined as `ECryptoMethod` of `1`. ASF expects the string to be a **[base64-encoded](https://en.wikipedia.org/wiki/Base64)** sequence of characters resulting in AES-encrypted byte array after translation, which then should be decrypted using included **[initialization vector](https://en.wikipedia.org/wiki/Initialization_vector)** and ASF encryption key.

Die obige Methode garantiert Sicherheit, solange der Angreifer den eingebauten ASF-Verschlüsselungsschlüssel nicht kennt, der sowohl zur Entschlüsselung als auch zur Verschlüsselung von Passwörtern verwendet wird. ASF erlaubt es dir, den Schlüssel mit dem **[Befehlszeilenargument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-Line-Arguments-de-DE)** `--cryptkey` anzugeben, den du für maximale Sicherheit verwenden solltest. Wenn du dich dazu entscheidest es wegzulassen, verwendet ASF seinen eigenen Schlüssel, der **bekannt** und fest in der Anwendung programmiert ist, was bedeutet, dass jeder die ASF-Verschlüsselung umkehren und ein entschlüsseltes Passwort erhalten kann. Es erfordert immer noch etwas Aufwand und ist nicht so einfach zu bewerkstelligen, aber möglich, deshalb solltest du fast immer `AES` Verschlüsselung mit deinem eigenen `--cryptkey` verwenden, den du geheim halten solltest. Die in ASF verwendete AES-Methode bietet eine zufriedenstellende Sicherheit und es ist ein Kompromiss zwischen der Einfachheit von `PlainText` und der Komplexität von `ProtectedDataForCurrentUser`, aber es wird dringend empfohlen, sie mit benutzerdefinierten `--cryptkey` zu verwenden. If used properly, guarantees decent security for safe storage.

* * *

### ProtectedDataForCurrentUser

Currently the most secure way of encrypting the password that ASF offers, and much safer than `AES` method explained above, is defined as `ECryptoMethod` of `2`. Der Hauptvorteil dieser Methode ist gleichzeitig der größte Nachteil - anstelle der Verwendung eines Verschlüsselungsschlüssels (wie in `AES`) werden die Daten mit den Anmeldeinformationen des aktuell angemeldeten Benutzers verschlüsselt, was bedeutet, dass es möglich ist die Daten **nur** auf dem Computer zu entschlüsseln, auf dem sie verschlüsselt wurden, und darüber hinaus **nur** durch den Benutzer der die Verschlüsselung ausgestellt hat. This ensures that even if you send your entire `Bot.json` with encrypted `SteamPassword` using this method to somebody else, he will not be able to decrypt the password without direct access to your PC. This is excellent security measure, but at the same time has a major disadvantage of being least compatible, as the password encrypted using this method will be incompatible with any other user as well as machine - including **your own** if you decide to e.g. reinstall your operating system. Still, it's one of the best methods of storing passwords, and if you're worried about security of `PlainText`, and don't want to put password each time, then this is your best bet as long as you don't have to access your configs from any other machine than your own.

**Please note that this option is available only for machines running Windows OS as of now.**

* * *

## Empfehlung

Wenn die Kompatibilität für dich kein Problem ist und du mit der Funktionsweise der `ProtectedDataForCurrentUser` Methode einverstanden bist, ist diese Option **empfohlen** um das Passwort in ASF zu speichern, da sie die beste Sicherheit bietet. Die Methode `AES` ist eine gute Wahl für Leute, die ihre Konfigurationen immer noch auf jedem beliebigen Computer verwenden wollen, während `PlainText` die einfachste Art ist das Passwort zu speichern, wenn es dir nichts ausmacht, dass jeder danach in der JSON-Konfigurationsdatei suchen kann.

Bitte bedenke, dass diese 3 Methoden als **unsicher** betrachtet werden, wenn ein Angreifer Zugriff auf deinen PC hat. ASF must be able to decrypt the encrypted passwords, and if the program running on your machine is capable of doing that, then any other program running on the same machine will be capable of doing so, too. `ProtectedDataForCurrentUser` ist die sicherste Variante, da **auch andere Benutzer, die den gleichen PC verwenden, ihn nicht entschlüsseln können**, aber es ist trotzdem möglich die Daten zu entschlüsseln, wenn jemand in der Lage ist deine Login-Informationen und Maschineninformationen zusätzlich zur ASF-Konfigurationsdatei zu stehlen.

In addition to encryption methods specified above, it's possible to also avoid specifying passwords entirely, for example as `SteamPassword` by using an empty string or `null` value. ASF will ask you for your password when it's required, and won't save it anywhere but keep in memory of currently running process, until you close it. While being the most secure method of dealing with passwords (they're not saved anywhere), it's also the most troublesome as you need to enter your password manually on each ASF run (when it's required). Wenn das kein Problem für dich ist ist dies die beste Wahl in Bezug auf Sicherheit.

* * *

## Entschlüsselung

ASF unterstützt keine Möglichkeit, bereits verschlüsselte Passwörter zu entschlüsseln, da Entschlüsselungsmethoden nur intern für den Zugriff auf die Daten innerhalb des Prozesses verwendet werden. If you want to revert encryption procedure e.g. for moving ASF to other machine when using `ProtectedDataForCurrentUser`, then simply repeat the procedure from beginning in the new environment.

* * *

## Hashing

ASF currently supports the following hashing methods as a definition of `EHashingMethod`:

| Wert | Name      |
| ---- | --------- |
| 0    | PlainText |
| 1    | SCrypt    |
| 2    | Pbkdf2    |

The exact description and comparison of them is available below.

In order to generate a hash, e.g. for `IPCPassword` usage, you should execute `hash` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** with the appropriate hashing method that you chose and your original plain-text password. Afterwards, put the hashed string that you've got as `IPCPassword` ASF config property, and finally change `IPCPasswordFormat` to the one that matches your chosen hashing method.

* * *

### PlainText

This is the most simple and insecure way of hashing a password, defined as `EHashingMethod` of `0`. ASF will generate hash matching the original input. It's the easiest one to use, and 100% compatible with all the setups, therefore it's a default way of storing secrets, totally insecure for safe storage.

* * *

### SCrypt

Considered secure by today standards, **[SCrypt](https://en.wikipedia.org/wiki/Scrypt)** way of hashing the password is defined as `EHashingMethod` of `1`. ASF will use the `SCrypt` implementation using `8` blocks, `8192` iterations, `32` hash length and encryption key as a salt to generate the array of bytes. The resulting bytes will then be encoded as **[base64](https://en.wikipedia.org/wiki/Base64)** string.

ASF allows you to specify salt for this method via `--cryptkey` **[command-line argument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-Line-Arguments)**, which you should use for maximum security. If you decide to omit it, ASF will use its own key which is **known** and hardcoded into the application, meaning hashing will be less secure. If used properly, guarantees decent security for safe storage.

* * *

### Pbkdf2

Considered weak by today standards, **[Pbkdf2](https://en.wikipedia.org/wiki/PBKDF2)** way of hashing the password is defined as `EHashingMethod` of `2`. ASF will use the `Pbkdf2` implementation using `10000` iterations, `32` hash length and encryption key as a salt, with `SHA-256` as a hmac algorithm to generate the array of bytes. The resulting bytes will then be encoded as **[base64](https://en.wikipedia.org/wiki/Base64)** string.

ASF allows you to specify salt for this method via `--cryptkey` **[command-line argument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-Line-Arguments)**, which you should use for maximum security. If you decide to omit it, ASF will use its own key which is **known** and hardcoded into the application, meaning hashing will be less secure.

* * *

## Empfehlung

If you'd like to use a hashing method for storing some secrets, such as `IPCPassword`, we recommend to use `SCrypt` with custom salt, as it provides a very decent security against brute-forcing attempts. `Pbkdf2` is offered only for compatibility reasons, mainly because we already have a working (and needed) implementation of it for other use cases across Steam platform (e.g. parental pins). It's still considered secure, but weak compared to alternatives (e.g. `SCrypt`).