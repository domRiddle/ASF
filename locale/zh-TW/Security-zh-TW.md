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

這是目前ASF提供最安全的密碼加密方式，比上述的&#8203;`AES`&#8203;方法還安全的多，您需將&#8203;`ECryptoMethod`&#8203;設為&#8203;`2`&#8203;。 這種方法的主要優點同時也是主要缺點：並不使用加密金鑰（如同&#8203;`AES`&#8203;），資料是使用當前登入使用者的憑證來加密的，也就是說它&#8203;**只能**&#8203;在加密設備上解密，除此之外也&#8203;**只能**&#8203;被加密使用者所使用。 若您使用此方法加密&#8203;`Bot.json`&#8203;的&#8203;`SteamPassword`&#8203;，即使您將整個檔案傳送給其他人，那麼要是他沒有直接存取您PC的權限，也無法解密密碼。 這是一種非常優秀的安全措施，但同時也有一個巨大的缺點，那就是幾乎沒有相容性可言，因為使用這種方法加密的密碼會與其他使用者或設備不相容：假設您打算重新安裝作業系統，這也將包含&#8203;**您自己的**&#8203;設備。 不過這仍是儲存密碼的最好方法，若您擔心&#8203;`PlainText`&#8203;的安全性，也不想要每次都輸入密碼，只要您不會在其他設備上存取您的設定檔，那麼這就是您最好的選擇。

**請注意，這個選項目前只適用於執行Windows作業系統的設備。**

---

### `EnvironmentVariable（環境變數）`

基於記憶體的儲存方法，&#8203;`ECryptoMethod`&#8203;為&#8203;`3`&#8203;。 ASF會從環境變數中讀取密碼，且名稱需指定於密碼欄位中（例如&#8203;`SteamPassword`&#8203;）。 舉例來說，把&#8203;`SteamPassword`&#8203;設定成&#8203;`ASF_PASSWORD_MYACCOUNT`&#8203;、&#8203;`PasswordFormat`&#8203;設定成&#8203;`3`&#8203;，就能讓ASF讀取環境變數&#8203;`${ASF_PASSWORD_MYACCOUNT}`&#8203;作為帳號的密碼。

---

### `File（文字檔）`

基於檔案的儲存方法（可在ASF設定資料夾外），&#8203;`ECryptoMethod`&#8203;為&#8203;`4`&#8203;。 ASF會從密碼欄位（例如&#8203;`SteamPassword`&#8203;）指定的路徑中讀取密碼。 指定的路徑可以是絕對路徑，也可以相對於ASF的「主要」位置（就是含有&#8203;`config`資料夾的資料夾，並與&#8203;`--path`&#8203;**[命令列引數](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments-zh-TW#引數)**&#8203;相符）。 此方法可用於&#8203;**[Docker secret](https://docs.docker.com/engine/swarm/secrets)**&#8203;，它會建立這類檔案以供使用，但若您自行建立檔案，亦可在Docker外使用。 舉例來說，把&#8203;`SteamPassword`&#8203;設定成&#8203;`/etc/secrets/MyAccount.pass`&#8203;、&#8203;`PasswordFormat`&#8203;設定成&#8203;`4`&#8203;，就能讓ASF讀取&#8203;`/etc/secrets/MyAccount.pass`&#8203;檔案內容作為帳號的密碼。

記住，請確保未授權的使用者無法讀取含有密碼的檔案，因為這樣就違背了使用本方法的目的。

---

## 建議

若相容性對您而言並不是個問題，且您可以接受&#8203;`ProtectedDataForCurrentUser`&#8203;方法的運作方式，那麼&#8203;**建議**&#8203;使用此選項來在ASF中儲存密碼，因為它擁有最好的安全性。 `AES`&#8203;方法對於那些想要在其他設備上使用設定的使用者來說是個不錯的選擇，而若您不介意其他人都可以查閱JSON設定檔，那&#8203;`PlainText`是最簡單儲存密碼的方法。

請注意，若攻擊者存取您的PC，則上述3種方法都被視為&#8203;**不安全**&#8203;。 ASF必須能解密被加密的密碼，但若您設備上執行的程式能解密，那麼在相同設備上執行的其他程式亦能解密您的密碼。 `ProtectedDataForCurrentUser`&#8203;是最安全的方式，因為&#8203;**即使使用同一台PC的其他使用者，也無法解密密碼**&#8203;，但若有人能夠竊取您的登入憑證、設備資訊及ASF設定檔，則仍能解密這些資料。

對於進階的設定，您可以使用&#8203;`EnvironmentVariable`&#8203;與&#8203;`File`。 它們的用途有限，若您希望透過某種自訂方式來獲得密碼，並只儲存於記憶體中，使用&#8203;`EnvironmentVariable`&#8203;比較好；而例如使用&#8203;**[Docker secret](https://docs.docker.com/engine/swarm/secrets)**&#8203;的情形則更適合使用&#8203;`File`&#8203;。 不過，它們都並未加密，因此基本上是將風險從ASF設定檔轉移到您選擇的這兩個方法的位置上。

除了上述指定的加密方法外，也可以完全不指定密碼，例如在&#8203;`SteamPassword`&#8203;設定成空字串，或&#8203;`null`&#8203;值。 ASF會在需要時向您詢問密碼，且不會儲存於任何地方，而是儲存於當前執行程序的記憶體中，直到您關閉它。 雖然這是儲存密碼最安全的方式（密碼並未儲存於任何地方），但也是最麻煩的，因為您需要在每次執行ASF時手動輸入密碼（若需要）。 若這對您而言並不是個問題，那這就是您在安全性方面最好的選擇。

---

## 解密

ASF不支援解密已加密密碼的任何方式，因為解密方式只在內部使用，用於存取程序裡的資料。 If you want to revert encryption procedure e.g. for moving ASF to other machine when using `ProtectedDataForCurrentUser`, then simply repeat the procedure from beginning in the new environment.

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