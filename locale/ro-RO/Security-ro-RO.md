# Securitate

## Criptare

ASF acceptă în prezent următoarele metode de criptare ca definiție a `ECryptoMethod`:

| Valoare | Nume                        |
| ------- | --------------------------- |
| 0       | PlainText                   |
| 1       | AES                         |
| 2       | ProtectedDataForCurrentUser |

Descrierea exactă și compararea acestora sunt disponibile mai jos.

Pentru a genera parola criptată, de ex. pentru utilizarea `SteamPassword`, ar trebui să executați **[comanda](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** `encrypt` cu criptarea corespunzătoare pe care ați ales-o și parola originală. După aceea, puneți textul criptat pe care l-ati obținut ca proprietate de configurare bot `SteamPassword` și în cele din urmă schimbați `PasswordFormat` cu cea care se potrivește cu metoda de criptare aleasă.

---

### PlainText

Acesta este cel mai simplu și nesigur mod de a stoca o parolă, definită ca `ECryptoMethod` de `0`. ASF se așteaptă ca șirul să fie un text simplu - o parolă în formă directă. Este cel mai ușor de utilizat, 100% compatibil cu toate configurațiile, deci este un mod implicit de a stoca secrete, complet nesigur pentru stocare în siguranță.

---

### AES

Considerat securizat de standardele de azi, modul **[AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)** de stocare a parolei este definit ca `ECryptoMethod` de `1`. ASF se așteaptă ca șirul să fie **[codat base64-](https://en.wikipedia.org/wiki/Base64)** secvență de caractere rezultând în array byte criptat AES după traducere, care apoi trebuie decriptate folosind **[vectorul de inițializare](https://en.wikipedia.org/wiki/Initialization_vector)** și cheia de criptare ASF.

The method above guarantees security as long as attacker doesn't know built-in ASF encryption key which is being used for decryption as well as encryption of passwords. ASF allows you to specify key via `--cryptkey` **[command-line argument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-Line-Arguments)**, which you should use for maximum security. If you decide to omit it, ASF will use its own key which is **known** and hardcoded into the application, meaning anybody can reverse the ASF encryption and get decrypted password. It still requires some effort and is not that easy to do, but possible, that's why you should almost always use `AES` encryption with your own `--cryptkey` which is kept in secret. AES method used in ASF provides security that should be satisfying and it's a balance between simplicity of `PlainText` and complexity of `ProtectedDataForCurrentUser`, but it's highly recommended to use it with custom `--cryptkey`. If used properly, guarantees decent security for safe storage.

---

### ProtectedDataForCurrentUser

Currently the most secure way of encrypting the password that ASF offers, and much safer than `AES` method explained above, is defined as `ECryptoMethod` of `2`. The major advantage of this method is at the same time the major disadvantage - instead of using encryption key (like in `AES`), data is encrypted using login credentials of currently logged in user, which means that it's possible to decrypt the data **only** on the machine it was encrypted on, and in addition to that, **only** by the user who issued the encryption. This ensures that even if you send your entire `Bot.json` with encrypted `SteamPassword` using this method to somebody else, he will not be able to decrypt the password without direct access to your PC. This is excellent security measure, but at the same time has a major disadvantage of being least compatible, as the password encrypted using this method will be incompatible with any other user as well as machine - including **your own** if you decide to e.g. reinstall your operating system. Still, it's one of the best methods of storing passwords, and if you're worried about security of `PlainText`, and don't want to put password each time, then this is your best bet as long as you don't have to access your configs from any other machine than your own.

**Please note that this option is available only for machines running Windows OS as of now.**

---

## Recommendation

If compatibility is not an issue for you, and you're fine with the way how `ProtectedDataForCurrentUser` method works, it is the **recommended** option of storing the password in ASF, as it provides the best security. `AES` method is a good choice for people who still want to make use of their configs on any machine they want, while `PlainText` is the most simple way of storing the password, if you don't mind that anybody can look into JSON configuration file for it.

Please keep in mind that all of those 3 methods are considered **insecure** if attacker has access to your PC. ASF must be able to decrypt the encrypted passwords, and if the program running on your machine is capable of doing that, then any other program running on the same machine will be capable of doing so, too. `ProtectedDataForCurrentUser` is the most secure variant as **even other user using the same PC will not be able to decrypt it**, but it's still possible to decrypt the data if somebody is able to steal your login credentials and machine info in addition to ASF config file.

In addition to encryption methods specified above, it's possible to also avoid specifying passwords entirely, for example as `SteamPassword` by using an empty string or `null` value. ASF will ask you for your password when it's required, and won't save it anywhere but keep in memory of currently running process, until you close it. While being the most secure method of dealing with passwords (they're not saved anywhere), it's also the most troublesome as you need to enter your password manually on each ASF run (when it's required). If that's not a problem for you, this is your best bet security-wise.

---

## Decryption

ASF doesn't support any way of decrypting already encrypted passwords, as decryption methods are used only internally for accessing the data inside the process. If you want to revert encryption procedure e.g. for moving ASF to other machine when using `ProtectedDataForCurrentUser`, then simply repeat the procedure from beginning in the new environment.

---

## Hashing

ASF currently supports the following hashing methods as a definition of `EHashingMethod`:

| Valoare | Nume      |
| ------- | --------- |
| 0       | PlainText |
| 1       | SCrypt    |
| 2       | Pbkdf2    |

Descrierea exactă și compararea acestora sunt disponibile mai jos.

In order to generate a hash, e.g. for `IPCPassword` usage, you should execute `hash` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** with the appropriate hashing method that you chose and your original plain-text password. Afterwards, put the hashed string that you've got as `IPCPassword` ASF config property, and finally change `IPCPasswordFormat` to the one that matches your chosen hashing method.

---

### PlainText

This is the most simple and insecure way of hashing a password, defined as `EHashingMethod` of `0`. ASF will generate hash matching the original input. Este cel mai ușor de utilizat, 100% compatibil cu toate configurațiile, deci este un mod implicit de a stoca secrete, complet nesigur pentru stocare în siguranță.

---

### SCrypt

Considered secure by today standards, **[SCrypt](https://en.wikipedia.org/wiki/Scrypt)** way of hashing the password is defined as `EHashingMethod` of `1`. ASF will use the `SCrypt` implementation using `8` blocks, `8192` iterations, `32` hash length and encryption key as a salt to generate the array of bytes. The resulting bytes will then be encoded as **[base64](https://en.wikipedia.org/wiki/Base64)** string.

ASF allows you to specify salt for this method via `--cryptkey` **[command-line argument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-Line-Arguments)**, which you should use for maximum security. If you decide to omit it, ASF will use its own key which is **known** and hardcoded into the application, meaning hashing will be less secure. If used properly, guarantees decent security for safe storage.

---

### Pbkdf2

Considered weak by today standards, **[Pbkdf2](https://en.wikipedia.org/wiki/PBKDF2)** way of hashing the password is defined as `EHashingMethod` of `2`. ASF will use the `Pbkdf2` implementation using `10000` iterations, `32` hash length and encryption key as a salt, with `SHA-256` as a hmac algorithm to generate the array of bytes. The resulting bytes will then be encoded as **[base64](https://en.wikipedia.org/wiki/Base64)** string.

ASF allows you to specify salt for this method via `--cryptkey` **[command-line argument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-Line-Arguments)**, which you should use for maximum security. If you decide to omit it, ASF will use its own key which is **known** and hardcoded into the application, meaning hashing will be less secure.

---

## Recommendation

If you'd like to use a hashing method for storing some secrets, such as `IPCPassword`, we recommend to use `SCrypt` with custom salt, as it provides a very decent security against brute-forcing attempts. `Pbkdf2` is offered only for compatibility reasons, mainly because we already have a working (and needed) implementation of it for other use cases across Steam platform (e.g. parental pins). It's still considered secure, but weak compared to alternatives (e.g. `SCrypt`).