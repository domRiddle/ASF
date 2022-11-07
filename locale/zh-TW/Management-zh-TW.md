# 管理

本章節涵蓋的相關主題是以最佳方式管理ASF程序。 雖然並非嚴格強制使用，但它包含了我們想分享的大量提示、技巧和良好實作，特別是對於系統管理員、打包ASF以便於在第三方儲存庫中使用的人，及進階使用者等。

---

## Linux 的 `systemd` 服務

在&#8203;`generic`&#8203;及`linux`&#8203;變體版本中，ASF自帶&#8203;`ArchiSteamFarm@.service`&#8203;檔案，這是&#8203;**[`systemd`](https://systemd.io)**&#8203;的服務設定檔。 若您想以服務來執行ASF，例如能在您的設備啟動時自動執行，那麼正確的&#8203;`systemd`&#8203;服務無疑是最好的方法，因此我們強烈推薦透過服務，而不是使用&#8203;`nohup`&#8203;、&#8203;`screen`&#8203;或其他方法來管理。

首先，若您還尚未建立用來執行ASF使用者，請先建立它。 我們在此以&#8203;`asf`&#8203;使用者作為範例，若您想使用另外一個，您就必須將下列範例中的&#8203;`asf`&#8203;使用者取代成您想使用的使用者名稱。 我們的服務不允許您使用&#8203;`root`&#8203;來執行ASF，因為這被認為是&#8203;**[不好的方式](#永遠不要以系統管理員身分執行-asf)**&#8203;。

```sh
su # 或是 sudo -i
useradd -m asf
```

下一步，將ASF解壓縮至&#8203;`/home/asf/ArchiSteamFarm`&#8203;資料夾。 資料夾的結構對我們的服務單元來說非常重要，它應為您的&#8203;`$HOME`&#8203;，也就是說&#8203;`ArchiSteamFarm`&#8203;資料夾需要放在&#8203;`/home/<user>`&#8203;中。 若您的操作完全正確，則現在應存在&#8203;`/home/asf/ArchiSteamFarm/ArchiSteamFarm@.service`&#8203;檔案。 若您使用&#8203;`linux`變體版本，且檔案不在Linux中解壓縮，而是例如從Windows傳輸，那麼您也需執行&#8203;`chmod +x /home/asf/ArchiSteamFarm/ArchiSteamFarm`&#8203;。

我們將使用&#8203;`root`&#8203;的身分執行操作，所以需使用&#8203;`su`&#8203;或&#8203;`sudo -i`&#8203;。

我們最好先確認資料夾仍屬於&#8203;`asf`&#8203;使用者，執行一次&#8203;`chown -hR asf:asf /home/asf/ArchiSteamFarm`&#8203;即可。 因為例如您用&#8203;`root`&#8203;來下載且／或解壓縮.zip檔，權限可能會不正確。

接下來，&#8203;`cd /etc/systemd/system`&#8203;，執行&#8203;`ln -s /home/asf/ArchiSteamFarm/ArchiSteamFarm\@.service .`&#8203;，這將建立一個指向我們服務聲明的符號連結，並於&#8203;`systemd`&#8203;中註冊。 符號連結可以使ASF在更新時自動更新您的&#8203;`systemd`&#8203;單元──依據您的情形，您可能想要使用該方法，或使用&#8203;`cp`&#8203;複製檔案並自行管理。

然後，確保&#8203;`systemd`&#8203;能辨識我們的服務：

```
systemctl status ArchiSteamFarm@asf

○ ArchiSteamFarm@asf.service - ArchiSteamFarm Service (on asf)
     Loaded: loaded (/etc/systemd/system/ArchiSteamFarm@.service; disabled; vendor preset: enabled)
     Active: inactive (dead)
       Docs: https://github.com/JustArchiNET/ArchiSteamFarm/wiki
```

請特別注意我們在&#8203;`@`&#8203;後宣告的使用者，在範例中是&#8203;`asf`&#8203;。 我們的systemd服務單元要求您宣告使用者，因為這會影響&#8203;`/home/<user>/ArchiSteamFarm`&#8203;二進制檔案的實際位置，及systemd生成程序的實際使用者。

若systemd回傳輸出與上述相似，則一切正常，我們就快完成了。 現在，剩下的步驟就是以我們所選的使用者實際啟動服務：&#8203;`systemctl start ArchiSteamFarm@asf`&#8203;。 等待一下，您就可以再次檢查狀態：

```
systemctl status ArchiSteamFarm@asf

● ArchiSteamFarm@asf.service - ArchiSteamFarm Service (on asf)
     Loaded: loaded (/etc/systemd/system/ArchiSteamFarm@.service; disabled; vendor preset: enabled)
     Active: active (running) since (...)
       Docs: https://github.com/JustArchiNET/ArchiSteamFarm/wiki
   Main PID: (...)
(...)
```

若&#8203;`systemd`&#8203;的狀態為&#8203;`active (running)`&#8203;，代表一切正常，您可以透過例如&#8203;`journalctl -r`&#8203;來驗證ASF程序已啟動並執行中，因為ASF預設輸出至控制台，會被&#8203;`systemd`&#8203;記錄。 若您滿意現在的設定，就能執行&#8203;`systemctl enable ArchiSteamFarm@asf`&#8203;指令，告訴&#8203;`systemd`&#8203;在啟動期間自動啟動您的服務。 這樣就完成了。

若您想停止程序，只需執行&#8203;`systemctl stop ArchiSteamFarm@asf`&#8203;。 同樣地，若您想要停用ASF自啟動，就執行&#8203;`systemctl disable ArchiSteamFarm@asf`&#8203;，非常簡單。

請注意，由於我們的&#8203;`systemd`&#8203;服務沒有啟用標準輸入，故您無法使用平常的方式來透過控制台輸入資訊。 透過&#8203;`systemd`執行，等同於指定&#8203;**[`Headless: true`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-TW#headless)**&#8203;設定。 幸運的是，若您需要在登入期間提供額外資訊，或更好地管理您的ASF程序，我們建議您使用&#8203;**[ASF-ui](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#asf-ui)**&#8203;，能夠容易地管理您的ASF。

### 環境變數

可以為我們的&#8203;`systemd`&#8203;服務提供額外的環境變數，例如您想要使用自訂的&#8203;`--cryptkey`&#8203;**[命令列引數](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments-zh-TW#引數)**&#8203;，就要指定&#8203;`ASF_CRYPTKEY`&#8203;環境變數。

若要提供自訂環境變數，使用&#8203;`mkdir -p /etc/asf`&#8203;來建立&#8203;`/etc/asf`資料夾（若其不存在）。 我們建議使用&#8203;`chmod 700 /etc/asf`&#8203;，來確保只有&#8203;`root`&#8203;使用者能夠存取這些檔案，因為它們可能含有機密屬性，例如&#8203;`ASF_CRYPTKEY`&#8203;。 然後，編輯&#8203;`/etc/asf/<user>`&#8203;檔案，其中&#8203;`<user>`&#8203;代表您要執行服務的使用者（在上述範例中是&#8203;`asf`&#8203;，因此是&#8203;`/etc/asf/asf`&#8203;）。

此檔案應該包含所有您要提供給程序的環境變數：

```sh
# 只宣告您實際所需的變數
ASF_CRYPTKEY="my_super_important_secret_cryptkey"
ASF_NETWORK_GROUP="my_network_group"

# 及其他任何您想要使用的變數
```

---

## 永遠不要以系統管理員身分執行 ASF！

ASF includes its own validation whether the process is being run as administrator (`root`) or not. Running as root is **not** required for any kind of operation done by the ASF process, assuming properly configured environment it's operating in, and therefore should be regarded as a **bad practice**. This means that on Windows, ASF should never be executed with "run as administrator" setting, and on Unix ASF should have a dedicated user account for itself, or re-use your own in case of a desktop system.

For further elaboration on *why* we discourage running ASF as root, refer to **[superuser](https://superuser.com/questions/218379/why-is-it-bad-to-run-as-root)** and other resources. If you're still not convinced, ask yourself what would happen to your machine if ASF process executed `rm -rf /*` command right after its launch.

### 我使用 `root` 執行，因為 ASF 無法寫入它自己的檔案

This means that you have wrongly configured permissions of the files ASF is trying to access. You should login as `root` account (either with `su` or `sudo -i`) and then **correct** the permissions by issuing `chown -hR asf:asf /path/to/ASF` command, substituting `asf:asf` with the user that you'll run ASF under, and `/path/to/ASF` accordingly. If by any chance you're using custom `--path` telling ASF user to use the different directory, you should execute the same command again for that path as well.

After doing that, you should no longer get any kind of issue related to ASF not being able to write over its own files, as you've just changed the owner of everything ASF is interested in to the user the ASF process will actually run under.

### 我使用 `root` 執行，因為我不知道該怎麼做

```sh
su # 或是 sudo -i
useradd -m asf
chown -hR asf:asf /path/to/ASF
su asf -c /path/to/ASF/ArchiSteamFarm # 或是 sudo -u asf /path/to/ASF/ArchiSteamFarm
```

That would be doing it manually, it's much easier to use our **[`systemd` service](#systemd-service-for-linux)** explained above.

### 我十分清楚，且依然想要使用 `root` 執行

As of V5.2.0.10, ASF no longer stops you from doing so, only displays a warning with a short notice. Just don't be shocked if one day due to a bug in the program it'll blow up your whole OS with complete data loss - you've been warned.

---

## 多個實例

ASF is compatible with running multiple instances of the process on the same machine. The instances can be completely standalone or derived from the same binary location (in which case, you want to run them with different `--path` **[command-line argument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments)**).

When running multiple instances from the same binary, keep in mind that you should typically disable auto-updates in all of their configs, as there is no synchronization between them in regards to auto-updates. If you'd like to keep having auto-updates enabled, we recommend standalone instances, but you can still make updates work, as long as you can ensure that all other ASF instances are closed.

ASF will do its best to maintain a minimum amount of OS-wide, cross-process communication with other ASF instances. This includes ASF checking its configuration directory against other instances, as well as sharing core process-wide limiters configured with `*LimiterDelay` **[global config properties](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#global-config)**, ensuring that running multiple ASF instances will not cause a possibility to run into a rate-limiting issue. In regards to technical aspects, all platforms use our dedicated mechanism of custom ASF file-based locks created in temporary directory, which is `C:\Users\<YourUser>\AppData\Local\Temp\ASF` on Windows, and `/tmp/ASF` on Unix.

It's not required for running ASF instances to share the same `*LimiterDelay` properties, they can use different values, as each ASF will add its own configured delay to the release time after acquiring the lock. If the configured `*LimiterDelay` is set to `0`, ASF instance will entirely skip waiting for the lock of given resource that is shared with other instances (that could potentially still maintain a shared lock with each other). When set to any other value, ASF will properly synchronize with other ASF instances and wait for its turn, then release the lock after configured delay, allowing other instances to continue.

ASF takes into account `WebProxy` setting when deciding about shared scope, which means that two ASF instances using different `WebProxy` configurations will not share their limiters with each other. This is implemented in order to allow `WebProxy` setups to operate without excessive delays, as expected from different network interfaces. This should be good enough for majority of use cases, however, if you have a specific custom setup in which you're e.g. routing requests yourself in a different way, you can specify network group yourself through `--network-group` **[command-line argument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments)**, which will allow you to declare ASF group that will be synchronized with this instance. Keep in mind that custom network groups are used exclusively, which means that ASF will no longer use `WebProxy` for determining the right group, as you're in charge of grouping in this case.

如果您想使用我們以上針對多個 ASF 實例說明中的 **[`systemd` 服務](#systemd-service-for-linux)**，這非常簡單，只需在我們的 `ArchiSteamFarm@` 服務聲明中使用另一個用戶，然後執行其餘步驟即可。 This will be equivalent of running multiple ASF instances with distinct binaries, so they can also auto-update and operate independently of each other.