# 安全性

## Encryption

ASF currently supports the following encryption mechanisms:

| 值 | 名称                          |
| - | --------------------------- |
| 0 | PlainText                   |
| 1 | AES                         |
| 2 | ProtectedDataForCurrentUser |

The exact description and comparison of them is available below.

In order to generate encrypted password, e.g. for `SteamPassword` usage, you should execute `encrypt` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** with the appropriate encryption that you chose and your original plain-text password. Afterwards, put the encrypted string that you've got as `SteamPassword` bot config property, and finally change `PasswordFormat` to the one that matches your chosen encryption method.

* * *

### PlainText

This is the most simple and insecure way of storing a password, defined as `ECryptoMethod` of `0`. ASF expects the string to be a plain text - a password in its direct form. It's the easiest one to use, and 100% compatible with all the setups, therefore it's a default way of storing secrets, totally insecure for safe storage.

* * *

### AES

Considered secure by today standards, **[AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)** way of storing the password is defined as `ECryptoMethod` of `1`. ASF expects the string to be a **[base64-encoded](https://en.wikipedia.org/wiki/Base64)** sequence of characters resulting in AES-encrypted byte array after translation, which then should be decrypted using included **[initialization vector](https://en.wikipedia.org/wiki/Initialization_vector)** and ASF encryption key.

此方法保证了安全性，只要攻击者不知道用于加密和解密的 ASF 内置密钥。 ASF 允许您通过 `--cryptkey` **[命令行参数](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-Line-Arguments-zh-CN)**&#8203;指定自定义密钥增强 ASF 的安全性。 如果您决定省略它，ASF 将使用自己提供的密钥，这个密钥是**已知**的并已硬编码到应用程序中，这意味着任何人都可以撤消 ASF 的加密并获取解密后的密码。 虽然这种攻击仍然需要一定时间而且并不容易，但是这是可以做到的。所以您总应该同时使用 `AES` 加密并用 `--cryptkey` 指定自定义密钥。 ASF 使用的 AES 方法提供了相对令人满意的安全性，并且它在 `PlainText` 的简单和 `ProtectedDataForCurrentUser` 的复杂之间取得了平衡，但强烈建议您将它与自定义密钥 `--cryptkey` 一起使用。 If used properly, guarantees decent security for safe storage.

* * *

### ProtectedDataForCurrentUser

Currently the most secure way of encrypting the password that ASF offers, and much safer than `AES` method explained above, is defined as `ECryptoMethod` of `2`. 这种方法的主要优点同时也是它主要的缺点——它并不使用加密密钥（像 `AES` 那样），数据会使用当前计算机登录的用户凭据加密，这意味着数据**仅**能在这台机器上进行解密，事实上，**仅仅**触发加密的计算机用户才能解密。 This ensures that even if you send your entire `Bot.json` with encrypted `SteamPassword` using this method to somebody else, he will not be able to decrypt the password without direct access to your PC. This is excellent security measure, but at the same time has a major disadvantage of being least compatible, as the password encrypted using this method will be incompatible with any other user as well as machine - including **your own** if you decide to e.g. reinstall your operating system. Still, it's one of the best methods of storing passwords, and if you're worried about security of `PlainText`, and don't want to put password each time, then this is your best bet as long as you don't have to access your configs from any other machine than your own.

**Please note that this option is available only for machines running Windows OS as of now.**

* * *

## 建议

如果兼容性对您来说不是问题，并且您可以接受 `ProtectedDataForCurrentUser` 方式，我们**推荐**您以这种方式存储密码，因为它有着最好的安全性。 对于还需要在其他计算机上使用配置文件的用户来说，`AES` 方法也是一个不错的选择。而 `PlainText` 是存储密码最简单的方法，只要您不介意其他人可能会查看您的 JSON 配置文件。

请注意，如果入侵者能够访问您的计算机，上述 3 种方法都**不安全**。 ASF must be able to decrypt the encrypted passwords, and if the program running on your machine is capable of doing that, then any other program running on the same machine will be capable of doing so, too. `ProtectedDataForCurrentUser` 是其中最安全的方法，**即使使用同一台计算机的其他用户也无法解密**，但如果有人窃取了您的登录凭据、计算机信息和 ASF 配置文件，他仍然有可能解密出您的密码。

In addition to encryption methods specified above, it's possible to also avoid specifying passwords entirely, for example as `SteamPassword` by using an empty string or `null` value. ASF will ask you for your password when it's required, and won't save it anywhere but keep in memory of currently running process, until you close it. While being the most secure method of dealing with passwords (they're not saved anywhere), it's also the most troublesome as you need to enter your password manually on each ASF run (when it's required). 如果这对您来说不是问题，这就是您在安全方面的最佳选择。

* * *

# 解密

ASF 不支持任何解密已加密密码的方法，因为解密方法仅在内部使用，用于访问进程内的数据。 If you want to revert encryption procedure e.g. for moving ASF to other machine when using `ProtectedDataForCurrentUser`, then simply repeat the procedure from beginning in the new environment.