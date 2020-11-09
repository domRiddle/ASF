# Segurança

## Encryption

ASF currently supports the following encryption methods as a definition of `ECryptoMethod`:

| Valor | Nome                             |
| ----- | -------------------------------- |
| 0     | PlainText (Texto sem formatação) |
| 1     | AES                              |
| 2     | ProtectedDataForCurrentUser      |

The exact description and comparison of them is available below.

In order to generate encrypted password, e.g. for `SteamPassword` usage, you should execute `encrypt` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** with the appropriate encryption that you chose and your original plain-text password. Afterwards, put the encrypted string that you've got as `SteamPassword` bot config property, and finally change `PasswordFormat` to the one that matches your chosen encryption method.

* * *

### PlainText (Texto sem formatação)

This is the most simple and insecure way of storing a password, defined as `ECryptoMethod` of `0`. ASF expects the string to be a plain text - a password in its direct form. It's the easiest one to use, and 100% compatible with all the setups, therefore it's a default way of storing secrets, totally insecure for safe storage.

* * *

### AES

Considered secure by today standards, **[AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)** way of storing the password is defined as `ECryptoMethod` of `1`. ASF expects the string to be a **[base64-encoded](https://en.wikipedia.org/wiki/Base64)** sequence of characters resulting in AES-encrypted byte array after translation, which then should be decrypted using included **[initialization vector](https://en.wikipedia.org/wiki/Initialization_vector)** and ASF encryption key.

O método acima garante segurança enquanto o agressor não saiba a chave de criptografia embutida do ASF que está sendo usada para descriptografia e criptografia de senhas. O ASF permite que você especifique a chave através do **[argumento de linha de comando](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-Line-Arguments-pt-BR)** `--cryptkey`, que você deve usar para segurança máxima. Se você decidir para omiti-lo, o ASF usará sua própria chave, que é **conhecida** e codificada no aplicativo, ou seja, qualquer um pode reverter a criptografia ASF e obter a senha descriptografada. Isso requer esforço e não é fácil de fazer, mas é possível e é por isso que você deve sempre que possível usar a encriptação `AES` com sua própria `--cryptkey` mantida em segredo. O método AES utilizado pelo ASF fornece segurança suficiente e é um equilíbrio entre a simplicidade do `PlainText` e a complexidade do `ProtectedDataForCurrentUser`, mas é altamente recomendado usá-lo com uma `--cryptkey` personalizada. If used properly, guarantees decent security for safe storage.

* * *

### ProtectedDataForCurrentUser

Currently the most secure way of encrypting the password that ASF offers, and much safer than `AES` method explained above, is defined as `ECryptoMethod` of `2`. A maior vantagem deste método é ao mesmo tempo a maior desvantagem - ao invés de usar uma chave de criptografia (como no `AES`), os dados são criptografados usando credenciais de login do usuário conectado no momento, o que significa que **só** é possível descriptografar os dados na máquina em que eles foram criptografados e, além disso, **somente** pelo usuário que emitiu a criptografia. This ensures that even if you send your entire `Bot.json` with encrypted `SteamPassword` using this method to somebody else, he will not be able to decrypt the password without direct access to your PC. This is excellent security measure, but at the same time has a major disadvantage of being least compatible, as the password encrypted using this method will be incompatible with any other user as well as machine - including **your own** if you decide to e.g. reinstall your operating system. Still, it's one of the best methods of storing passwords, and if you're worried about security of `PlainText`, and don't want to put password each time, then this is your best bet as long as you don't have to access your configs from any other machine than your own.

**Please note that this option is available only for machines running Windows OS as of now.**

* * *

## Recomendação

Se compatibilidade não é um problema para você, e você se sente tranquilo com a forma que o método `ProtectedDataForCurrentUser` funciona, é esse o método **recomendado** para salvar suas senhas no ASF, já que ele fornece a melhor segurança. O método `AES` é uma boa escolha para as pessoas que querem usar suas configurações em mais de um computador, enquanto `PlainText` é a forma mais simples de salvar a senha, se você não se importar que qualquer um pode pegá-la no arquivo JSON.

Tenha em mente que todos esses 3 métodos são considerados **inseguros** se um atacante tiver acesso ao seu PC. ASF must be able to decrypt the encrypted passwords, and if the program running on your machine is capable of doing that, then any other program running on the same machine will be capable of doing so, too. `ProtectedDataForCurrentUser` é a variante mais segura já que **mesmo outro usuário usando o mesmo PC não será capaz de descriptografá-lo**, mas ainda é possível descriptografar os dados se alguém for capas de roubar suas credenciais de login e informações do seu computador, juntamente com o arquivo de configuração do ASF.

In addition to encryption methods specified above, it's possible to also avoid specifying passwords entirely, for example as `SteamPassword` by using an empty string or `null` value. ASF will ask you for your password when it's required, and won't save it anywhere but keep in memory of currently running process, until you close it. While being the most secure method of dealing with passwords (they're not saved anywhere), it's also the most troublesome as you need to enter your password manually on each ASF run (when it's required). Se isso não for um problema para você, então é sua melhor aposta em termos de segurança.

* * *

## Descriptografar

O ASF não suporta nenhuma forma de descriptografar senha já criptografadas, já que os métodos de descriptografia são usados internamente para acessar os dados dentro do processo. If you want to revert encryption procedure e.g. for moving ASF to other machine when using `ProtectedDataForCurrentUser`, then simply repeat the procedure from beginning in the new environment.

* * *

## Hashing

ASF currently supports the following hashing methods as a definition of `EHashingMethod`:

| Valor | Nome                             |
| ----- | -------------------------------- |
| 0     | PlainText (Texto sem formatação) |
| 1     | SCrypt                           |
| 2     | Pbkdf2                           |

The exact description and comparison of them is available below.

In order to generate a hash, e.g. for `IPCPassword` usage, you should execute `hash` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** with the appropriate hashing method that you chose and your original plain-text password. Afterwards, put the hashed string that you've got as `IPCPassword` ASF config property, and finally change `IPCPasswordFormat` to the one that matches your chosen encryption method.

* * *

### PlainText (Texto sem formatação)

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

## Recomendação

If you'd like to use a hashing method for storing some secrets, such as `IPCPassword`, we recommend to use `SCrypt` with custom salt, as it provides a very decent security against brute-forcing attempts. `Pbkdf2` is offered only for compatibility reasons, mainly because we already have a working (and needed) implementation of it for other use cases across Steam platform (e.g. parental pins). It's still considered secure, but weak compared to alternatives (e.g. `SCrypt`).