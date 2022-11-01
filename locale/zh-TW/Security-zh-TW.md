# 安全性

## 加密

ASF目前支援以下加密方式，作為&#8203;`ECryptoMethod`&#8203;的定義：

| 值 | 名稱                          |
| - | --------------------------- |
| 0 | PlainText（純文字）              |
| 1 | AES（進階加密標準）                 |
| 2 | ProtectedDataForCurrentUser |
| 3 | EnvironmentVariable（環境變數）   |
| 4 | File（文字檔）                   |

以下提供了它們的詳細說明及比較。

要生成加密的密碼，例如在&#8203;`SteamPassword`&#8203;中使用，您可以執行&#8203;`encrypt`&#8203;**[指令](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-TW)**&#8203;，並加上您所選的適當的加密方式及您密碼的原始純文字。 然後，將您獲得的加密字串輸入&#8203;`SteamPassword`&#8203;Bot設定屬性，並修改&#8203;`PasswordFormat`&#8203;對應至您所選的加密方法。 某些格式不需要&#8203;`encrypt`指令，例如&#8203;`EnvironmentVariable`&#8203;或&#8203;`File`&#8203;，只需給予適合的路徑。

---

### `PlainText（純文字）`

這是最簡單也最不安全的密碼儲存方式，指定&#8203;`ECryptoMethod`&#8203;為&#8203;`0`&#8203;。 ASF字串應為純文字──即原始形式的密碼。 它是最容易使用的，且與所有設定100%相容，因此它是儲存私密資料的預設方式，但對於安全儲存來說毫無安全可言。

---

### `AES（進階加密標準）`

依照現今的標準，可以將&#8203;**[AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)**&#8203;儲存密碼的方式視為安全的，指定&#8203;`ECryptoMethod`&#8203;為&#8203;`1`&#8203;。 ASF字串應為AES加密的位元組陣列轉換成&#8203;**[Base64編碼](https://zh.wikipedia.org/zh-tw/Base64)**&#8203;的字元序列，需含有&#8203;**[初始向量](https://en.wikipedia.org/wiki/Initialization_vector)**&#8203;及ASF加密鍵來解密。

上述方法保證了安全性，只要攻擊者不知道用於加密及解密的ASF加密鍵。 ASF使您能夠透過&#8203;`--cryptkey`&#8203;**[命令列引數](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-Line-Arguments-zh-TW)**&#8203;來獲得最大的安全性。 若您決定省略它，ASF將會使用自己&#8203;**已知**&#8203;硬編碼至應用程式中的金鑰，這代表任何人都可以逆轉ASF的加密，並獲得解密的密碼。 雖然這需要一些時間且不容易做到，但它還是有可能的。這就是為什麼您總應一起使用&#8203;`AES`&#8203;及您自己的&#8203;`--cryptkey`&#8203;來加密。 ASF使用的AES方法提供了令人滿意的安全性，它在簡單的&#8203;`PlainText`&#8203;及複雜的&#8203;`ProtectedDataForCurrentUser`&#8203;間取得了平衡，並強烈建議您與自訂的&#8203;`--cryptkey`&#8203;一起使用。 若使用得當，就能保證安全儲存的良好安全性。

---

### `ProtectedDataForCurrentUser`

Currently the most secure way of encrypting the password that ASF offers, and much safer than `AES` method explained above, is defined as `ECryptoMethod` of `2`. The major advantage of this method is at the same time the major disadvantage - instead of using encryption key (like in `AES`), data is encrypted using login credentials of currently logged in user, which means that it's possible to decrypt the data **only** on the machine it was encrypted on, and in addition to that, **only** by the user who issued the encryption. This ensures that even if you send your entire `Bot.json` with encrypted `SteamPassword` using this method to somebody else, he will not be able to decrypt the password without direct access to your PC. This is excellent security measure, but at the same time has a major disadvantage of being least compatible, as the password encrypted using this method will be incompatible with any other user as well as machine - including **your own** if you decide to e.g. reinstall your operating system. Still, it's one of the best methods of storing passwords, and if you're worried about security of `PlainText`, and don't want to put password each time, then this is your best bet as long as you don't have to access your configs from any other machine than your own.

**請注意，這個選項目前只適用於執行Windows作業系統的設備。**

---

### `EnvironmentVariable（環境變數）`

Memory-based storage defined as `ECryptoMethod` of `3`. ASF will read the password from the environment variable with given name specified in the password field (e.g. `SteamPassword`). For example, setting `SteamPassword` to `ASF_PASSWORD_MYACCOUNT` and `PasswordFormat` to `3` will cause ASF to evaluate `${ASF_PASSWORD_MYACCOUNT}` environment variable and use whatever is assigned to it as the account password.

---

### `File（文字檔）`

File-based storage (possibly outside of the ASF config directory) defined as `ECryptoMethod` of `4`. ASF will read the password from the file path specified in the password field (e.g. `SteamPassword`). The specified path can be either absolute, or relative to ASF's "home" location (the folder with `config` directory inside, taking into account `--path` **[command-line argument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments#arguments)**). This method can be used for example with **[Docker secrets](https://docs.docker.com/engine/swarm/secrets)**, which create such files for usage, but can also be used outside of Docker if you create appropriate file yourself. For example, setting `SteamPassword` to `/etc/secrets/MyAccount.pass` and `PasswordFormat` to `4` will cause ASF to read `/etc/secrets/MyAccount.pass` and use whatever is written to that file as the account password.

Remember to ensure that file containing the password is not readable by unauthorized users, as that defeats the whole purpose of using this method.

---

## 建議

若相容性對您而言並不是個問題，且您可以接受&#8203;`ProtectedDataForCurrentUser`&#8203;方法的運作方式，那麼&#8203;**建議**&#8203;使用此選項來在ASF中儲存密碼，因為它擁有最好的安全性。 `AES` method is a good choice for people who still want to make use of their configs on any machine they want, while `PlainText` is the most simple way of storing the password, if you don't mind that anybody can look into JSON configuration file for it.

Please keep in mind that all of those 3 methods are considered **insecure** if attacker has access to your PC. ASF must be able to decrypt the encrypted passwords, and if the program running on your machine is capable of doing that, then any other program running on the same machine will be capable of doing so, too. `ProtectedDataForCurrentUser` is the most secure variant as **even other user using the same PC will not be able to decrypt it**, but it's still possible to decrypt the data if somebody is able to steal your login credentials and machine info in addition to ASF config file.

For advanced setups, you can utilize `EnvironmentVariable` and `File`. They have limited usability, the `EnvironmentVariable` will be a good idea if you'd prefer to obtain password through some kind of custom solution and store it in memory exclusively, while `File` is good for example with **[Docker secrets](https://docs.docker.com/engine/swarm/secrets)**. Both of them are unencrypted however, so you basically move the risk from ASF config file to whatever you pick from those two.

In addition to encryption methods specified above, it's possible to also avoid specifying passwords entirely, for example as `SteamPassword` by using an empty string or `null` value. ASF will ask you for your password when it's required, and won't save it anywhere but keep in memory of currently running process, until you close it. While being the most secure method of dealing with passwords (they're not saved anywhere), it's also the most troublesome as you need to enter your password manually on each ASF run (when it's required). If that's not a problem for you, this is your best bet security-wise.

---

## 解密

ASF doesn't support any way of decrypting already encrypted passwords, as decryption methods are used only internally for accessing the data inside the process. If you want to revert encryption procedure e.g. for moving ASF to other machine when using `ProtectedDataForCurrentUser`, then simply repeat the procedure from beginning in the new environment.

---

## 雜湊

ASF currently supports the following hashing methods as a definition of `EHashingMethod`:

| 值 | 名稱             |
| - | -------------- |
| 0 | PlainText（純文字） |
| 1 | SCrypt         |
| 2 | Pbkdf2         |

以下提供了它們的詳細描述和比較。

In order to generate a hash, e.g. for `IPCPassword` usage, you should execute `hash` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** with the appropriate hashing method that you chose and your original plain-text password. Afterwards, put the hashed string that you've got as `IPCPassword` ASF config property, and finally change `IPCPasswordFormat` to the one that matches your chosen hashing method.

---

### `PlainText（純文字）`

This is the most simple and insecure way of hashing a password, defined as `EHashingMethod` of `0`. ASF will generate hash matching the original input. It's the easiest one to use, and 100% compatible with all the setups, therefore it's a default way of storing secrets, totally insecure for safe storage.

---

### `SCrypt`

Considered secure by today standards, **[SCrypt](https://en.wikipedia.org/wiki/Scrypt)** way of hashing the password is defined as `EHashingMethod` of `1`. ASF will use the `SCrypt` implementation using `8` blocks, `8192` iterations, `32` hash length and encryption key as a salt to generate the array of bytes. The resulting bytes will then be encoded as **[base64](https://en.wikipedia.org/wiki/Base64)** string.

ASF allows you to specify salt for this method via `--cryptkey` **[command-line argument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-Line-Arguments)**, which you should use for maximum security. If you decide to omit it, ASF will use its own key which is **known** and hardcoded into the application, meaning hashing will be less secure. If used properly, guarantees decent security for safe storage.

---

### `Pbkdf2`

Considered weak by today standards, **[Pbkdf2](https://en.wikipedia.org/wiki/PBKDF2)** way of hashing the password is defined as `EHashingMethod` of `2`. ASF will use the `Pbkdf2` implementation using `10000` iterations, `32` hash length and encryption key as a salt, with `SHA-256` as a hmac algorithm to generate the array of bytes. The resulting bytes will then be encoded as **[base64](https://en.wikipedia.org/wiki/Base64)** string.

ASF allows you to specify salt for this method via `--cryptkey` **[command-line argument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-Line-Arguments)**, which you should use for maximum security. If you decide to omit it, ASF will use its own key which is **known** and hardcoded into the application, meaning hashing will be less secure.

---

## 建議

If you'd like to use a hashing method for storing some secrets, such as `IPCPassword`, we recommend to use `SCrypt` with custom salt, as it provides a very decent security against brute-forcing attempts. `Pbkdf2` is offered only for compatibility reasons, mainly because we already have a working (and needed) implementation of it for other use cases across Steam platform (e.g. parental pins). It's still considered secure, but weak compared to alternatives (e.g. `SCrypt`).