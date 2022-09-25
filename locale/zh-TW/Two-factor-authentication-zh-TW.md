# 雙重驗證

不久前，Valve引入了一種稱為「託管」的系統，該系統需要額外的驗證器才能進行各種與帳號相關的操作。 您可以在&#8203;**[這裡](https://support.steampowered.com/kb_article.php?ref=1284-WTKB-4729)**&#8203;以及&#8203;**[這裡](https://support.steampowered.com/kb_article.php?ref=8078-TPHC-6195)**&#8203;閱讀，以了解更多。 在試圖了解ASF雙重驗證的邏輯之前，首先要了解雙重驗證系統。

如您所見，所有交易都會被託管最多15天，雖然對於ASF來說並不是個問題，但對於那些想要完全自動化的人來說，仍特別煩人。 幸運的是，ASF擁有解決該問題的辦法，叫做ASF雙重驗證。

---

# ASF 邏輯

不論您是否使用以下所述的ASF雙重驗證，ASF都包含正確的邏輯，且完全了解受標準雙重驗證保護的帳號。 它會在需要時，向您請求所需的詳細資訊（例如在登入期間）。 若您使用ASF雙重驗證，程式能夠跳過這些請求並自動生成所需的權杖，為您免去麻煩的同時並啟用額外功能（如下所述）。

---

# ASF 雙重驗證

ASF雙重驗證是一個內建模組，負責為ASF程序提供雙重驗證功能，例如生成權杖及接受交易確認。 它會複製您現有的驗證器，所以您可以同時使用原有的驗證器與ASF雙重驗證。

您可以執行&#8203;`2fa`&#8203;**[指令](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-TW)**&#8203;來驗證您的Bot帳號是否已啟用ASF雙重驗證。 除非您已將驗證器導入成ASF雙重驗證，否則所有&#8203;`2fa`&#8203;指令都沒有作用，這代表您的帳號未啟用ASF雙重驗證，因此對於需要此模組的進階功能也無法運作。

---

## 匯入

要使用ASF雙重驗證，您應該已經擁有並綁定ASF支援可用的驗證器。 除了您能自行手動提供所需的憑證外，ASF目前還支援幾種不同的官方或非官方雙重驗證來源：Android、iOS、SteamDesktopAuthenticator與WinAuth。 若您還沒有任何驗證器，則需要先選擇一個可用的應用程式，並進行設定。 如果您不知道該選擇哪個，我們建議您使用WinAuth，但只要您依說明操作，上述任何一個都能正常運作。

以下所有指南都需要您已擁有上述工具／應用程式中**可使用的**驗證器。 若您導入無效資料，ASF雙重驗證會無法正常運作，因此在嘗試導入前，請確認您的身份驗證器運作正常。 這包含測試並驗證下列驗證器功能是否正常運作：
- 您能生成權杖，且Steam網路接受這些權杖
- 您能獲得交易確認，且您的行動裝置驗證器能收到它們
- 您能接受這些交易確認，且Steam網路能夠正確辨識它們為已確認／已拒絕

檢查上述操作是否有效，來保證您的驗證器正常運作──若它們不正常，那麼它們也不會在ASF中運作，您只會浪費時間並給自己帶來額外麻煩。

---

### Android 手機

**以下說明適用於&#8203;`2.X`&#8203;版本的Steam應用程式，目前還未有從&#8203;`3.0`&#8203;及以上版本擷取所需資源的方法。 一旦找到普遍可用的方法，我們將更新本章節。 至今為止，暫時的解決方法是故意安裝舊版的Steam應用程式，註冊雙重驗證，提取所需資訊，最後再將應用程式更新到最新版本──原有的驗證器將繼續運作。**

在一般情形下，若要從您的Android手機導入驗證器，您需要具有&#8203;**[Root](https://zh.wikipedia.org/zh-tw/Root_(Android)) **&#8203;權限。 不同的設備具有不同的Root方法，所以無法告訴您如何Root您的設備。 可以造訪&#8203;**[XDA](https://www.xda-developers.com/root)**&#8203;查詢相關指南，及與Root相關的一般資訊。 若您找不到適用於您設備的指南，請在Google中搜尋。

理論上來說，若沒有Root權限則無法讀取受保護的Steam檔案。 目前唯一正式的不需要Root擷取Steam檔案的方法是，以某種方式來製作未加密的&#8203;`/data`&#8203;備份，然後在PC上手動提取所需的檔案，但此種方法高度依賴您的手機製造商，且&#8203;**不屬於**&#8203;Android標準，因此我們不會在此討論。 若您幸運擁有這種功能，您可以使用它，但大多數使用者並沒有這樣的機會。

也有無需Root權限的非正式方法，透過安裝或降級您的Steam應用程式到&#8203;`2.1`&#8203;（或更早）版本，設定行動驗證器，然後透過&#8203;`adb backup`&#8203;建立應用程式快照（與我們所需的&#8203;`data`&#8203;檔案一起），來擷取所需的檔案。 但由於這是一個嚴重的安全漏洞，而且擷取檔案的方法完全不受支援，因此我們不會在此詳細說明。Valve在新版本中停用了這個安全漏洞是有原因的，我們只是提到這個方法是種可能。 儘管如此，仍然可以乾淨安裝這個舊版本，綁定新的驗證器，擷取所需檔案，然後升級應用程式，這理論上能夠使用，但需要您自行嘗試。

假設您已成功Root您的手機，接下來您應下載應用程式商店上可用的任何Root檔案總管，例如&#8203;**[這個](https://play.google.com/store /apps/details?id=com.jrummy.root.browserfree)**&#8203;（或任何您偏好的）。 您還可以透過ADB（Android Debug Bridge）或其他任何可行的方式存取受保護的檔案。我們使用檔案總管，因為它對使用者來說，絕對是最友好的方式。

打開Root檔案總管後，前往&#8203;`/data/data`&#8203;資料夾。 請注意，&#8203;`/data/data`&#8203;資料夾受到保護，若您沒有Root權限，則無法存取。 然後找到&#8203;`com.valvesoftware.android.steam.community`&#8203;資料夾，把它複製到您的&#8203;`/sdcard`&#8203;中，也就是您的內部儲存空間。 之後，您應該能夠將手機插入至PC上，並像平常一樣從內部儲存空間中複製資料夾。 若您確定已將資料夾複製到正確的位置，但仍無法看到該資料夾，請先嘗試重新啟動手機。

現在，您可以選擇是先將驗證器匯入WinAuth，然後再匯入ASF，或是直接匯入ASF。 第一個選項更加友善，使您在PC上也複製一份驗證器，可以讓您從3個不同的地方（手機、PC與ASF）進行交易確認及生成權杖。 若您想這樣做，只需打開WinAuth，加入新的Steam驗證器，選擇從Android匯入，然後依照說明存取您在上述過程中取得的檔案。 完成後，您可以從WinAuth將此驗證器匯入ASF，這將在下面WinAuth章節有專門的說明。

若您不想或不需要透過WinAuth，那麼只需從受保護的資料夾中複製&#8203;`files/Steamguard-<SteamID>`&#8203;檔案，其中&#8203;`SteamID`&#8203;是您要加入的帳號的64位元Steam ID（可能有多個。若您只有一個帳號，那麼只有一個檔案）。 您需要將該檔案放在ASF的&#8203;`config`&#8203;資料夾中。 完成後，將檔案重新命名成&#8203;`BotName.maFile`&#8203;，其中&#8203;`BotName`&#8203;是您要加入ASF雙重驗證的Bot名稱。 最後，開啟ASF──它將會偵測到&#8203;`.maFile`&#8203;並將其匯入。

```text
[*] INFO: ImportAuthenticator() <1> 正在將 .maFile 轉換成 ASF 格式…
[*] INFO: ImportAuthenticator() <1> 成功匯入行動驗證器！
```

就是這樣，假設您已匯入具有效密鑰的正確檔案，一切都應該正常運作，您可以使用&#8203;`2fa`&#8203;指令來驗證。 若您做錯了，隨時可以刪除&#8203;`Bot.db`&#8203;來重新開始。

---

### iOS

對於iOS而言，您可以使用&#8203;**[ios-steamguard-extractor](https://github.com/CaitSith2/ios-steamguard-extractor)**&#8203;。 這是能夠做到是因為您可以解密並備份，放到您的PC上，並使用該工具來擷取原本無法獲得的Steam資料（由於iOS的加密機制，至少在沒有越獄的情形下無法做到）。

前往&#8203;**[發布頁面](https://github.com/CaitSith2/ios-steamguard-extractor/releases/latest)**&#8203;下載這個程式的最新版本。 擷取資料後，您可以將其放進WinAuth中，然後從WinAuth複製到ASF（但您也可以將生成的.json裡面，從&#8203;`{`&#8203;起始到&#8203;`}`&#8203;結尾的部分複製到&#8203;`BotName.maFile`&#8203;再繼續）。 若您詢問，我會強烈建議先匯入到WinAuth，然後確保生成權杖與接受確認都正常運作，這樣您就能確定到目前為止沒有出問題。 If your credentials are invalid, ASF 2FA will not work properly, so it's much better to make ASF import step your last one.

For questions/issues, please visit **[issues](https://github.com/CaitSith2/ios-steamguard-extractor/issues)**.

*Keep in mind that above tool is unofficial, you're using it at your own risk. We do not offer technical support if it doesn't work properly - we got a few signals that it's exporting invalid 2FA credentials - verify that confirmations work in authenticator like WinAuth prior to importing that data to ASF!*

---

### SteamDesktopAuthenticator

If you have your authenticator running in SDA already, you should notice that there is `steamID.maFile` file available in `maFiles` folder. Copy that file to `config` directory of ASF. Make sure that `.maFile` is in unencrypted form, as ASF can't decrypt SDA files - unencrypted file content should start with `{` character.

You should now rename `steamID.maFile` to `BotName.maFile` in ASF config directory, where `BotName` is the name of your bot you're adding ASF 2FA to. Alternatively you can leave it as it is, ASF will then pick it automatically after logging in. Helping ASF makes it possible to use ASF 2FA before logging in, if you won't help ASF, then the file can be picked only after ASF successfully logs in (as ASF doesn't know `steamID` of your account before in fact logging in).

If you did everything correctly, launch ASF, and you should notice:

```text
[*] INFO: ImportAuthenticator() <1> Converting .maFile into ASF format...
[*] INFO: ImportAuthenticator() <1> Successfully finished importing mobile authenticator!
```

From now on, your ASF 2FA should be operational for this account.

---

### WinAuth

Firstly create new empty `BotName.maFile` in ASF config directory, where `BotName` is the name of your bot you're adding ASF 2FA to. Remember that it should be `BotName.maFile` and NOT `BotName.maFile.txt`, Windows likes to hide known extensions by default. If you provide incorrect name, it won't be picked by ASF.

Now launch WinAuth as usual. Right click on Steam icon and select "Show SteamGuard and Recovery Code". Then check "Allow copy". You should notice familiar to you JSON structure on the bottom of the window, starting with `{`. Copy whole text into a `BotName.maFile` file created by you in previous step.

If you did everything correctly, launch ASF, and you should notice:

```text
[*] INFO: ImportAuthenticator() <1> Converting .maFile into ASF format...
[*] INFO: ImportAuthenticator() <1> Successfully finished importing mobile authenticator!
```

From now on, your ASF 2FA should be operational for this account.

---

## 完成

From this moment, all `2fa` commands will work as they'd be called on your classic 2FA device. You can use both ASF 2FA and your authenticator of choice (Android, iOS, SDA or WinAuth) to generate tokens and accept confirmations.

If you have authenticator on your phone, you can optionally remove SteamDesktopAuthenticator and/or WinAuth, as we won't need it anymore. However, I suggest to keep it just in case, not to mention that it's more handy than normal steam authenticator. Just keep in mind that ASF 2FA is **NOT** a general purpose authenticator and it should **never** be the only one you use, since it doesn't even include all data that authenticator should have. It's not possible to convert ASF 2FA back to original authenticator, therefore always make sure that you have general-purpose authenticator in other place, such as in WinAuth/SDA, or on your phone.

---

## 常見問題

### ASF 如何使用雙重驗證模組？

如果 ASF 兩步驟驗證可用，ASF 將用它自動確認由 ASF 傳送/接受的交易。 It will also be capable of automatically generating 2FA tokens on as-needed basis, for example in order to log in. In addition to that, having ASF 2FA also enables `2fa` commands for you to use. That should be all for now, if I didn't forget about anything - basically ASF uses 2FA module on as-needed basis.

---

### 假如我需要雙重驗證權杖？

You will need 2FA token to access 2FA-protected account, that includes every account with ASF 2FA as well. You should generate tokens in authenticator that you used for import, but you can also generate temporary tokens through `2fa` command sent via the chat to given bot. You can also use `2fa <BotNames>` command to generate temporary token for given bot instances. This should be enough for you to access bot accounts through e.g. browser, but as noted above - you should use your friendly authenticator (Android, iOS, SDA or WinAuth) instead.

---

### 匯入 ASF 雙重驗證後，我可以使用原本的驗證器嗎？

Yes, your original authenticator remains functional and you can use it together with using ASF 2FA. That's the whole point of the process - we're importing your authenticator credentials into ASF, so ASF can make use of them and accept selected confirmations on your behalf.

---

### ASF 行動裝置驗證器儲存在哪裡？

ASF mobile authenticator is saved in `BotName.db` file in your config directory, along with some other crucial data related to given account. If you want to remove ASF 2FA, read how below.

---

### 如何移除 ASF 雙重驗證？

Simply stop ASF and remove associated `BotName.db` of the bot with linked ASF 2FA you want to remove. This option will remove associated imported 2FA with ASF, but will NOT delink your authenticator. If you instead want to delink your authenticator, apart from removing it from ASF (firstly), you should delink it in authenticator of your choice (Android, iOS, SDA or WinAuth), or - if you can't for some reason, use revocation code that you received during linking that authenticator, on the Steam website. It's not possible to unlink your authenticator through ASF, this is what general-purpose authenticator that you already have should be used for.

---

### 我將驗證器綁定到 SDA/WinAuth，然後匯入 ASF。 我可以解除綁定，並重新綁定到我的手機上嗎？

**不**。 ASF **imports** your authenticator data in order to use it. If you delink your authenticator then you'll also cause ASF 2FA to stop functioning, regardless if you remove it firstly like stated in above question or not. If you want to use your authenticator on both your phone and ASF (plus optionally in SDA/WinAuth), then you'll need to **import** your authenticator from your phone, and not create new one in SDA/WinAuth. You can have only **one** linked authenticator, that's why ASF **imports** that authenticator and its data in order to use it as ASF 2FA - it's **the same** authenticator, just existing in two places. If you decide to delink your mobile authenticator credentials - regardless in which way, ASF 2FA will stop working, as previously copied mobile authenticator credentials will no longer be valid. In order to use ASF 2FA together with authenticator on your phone, you must import it from Android/iOS, which is described above.

---

### 使用 ASF 雙重驗證是否比 WinAuth/SDA/Other 驗證器更適合接受所有確認？

**Yes**, in several ways. First and most important one - using ASF 2FA **significantly** increases your security, as ASF 2FA module ensures that ASF will only accept automatically its own confirmations, so even if attacker does request a trade that is harmful, ASF 2FA will **not** accept such trade, as it was not generated by ASF. In addition to security part, using ASF 2FA also brings performance/optimization benefits, as ASF 2FA fetches and accepts confirmations immediately after they're generated, and only then, as opposed to inefficient polling for confirmations each X minutes done e.g. by SDA or WinAuth. In short, there is no reason to use third-party authenticator over ASF 2FA, if you plan on automating confirmations generated by ASF - that's exactly what ASF 2FA is for, and using it does not conflict with you confirming everything else in authenticator of your choice. We strongly recommend to use ASF 2FA for entire ASF activity - this is much more secure than any other solution.

---

## 進階

If you're advanced user, you can also generate maFile manually. This can be used in case you'd want to import authenticator from other sources than the ones we've described above. It should have a **[valid JSON structure](https://jsonlint.com)** of:

```json
{
  "shared_secret": "STRING",
  "identity_secret": "STRING"
}
```

Standard authenticator data has more fields - they're entirely ignored by ASF during import, as they're not needed. You don't have to remove them - ASF only requires valid JSON with 2 mandatory fields described above, and will ignore additional fields (if any). Of course, you need to replace `STRING` placeholder in the example above with valid values for your account. Each `STRING` should be base64-encoded representation of bytes the appropriate private key is made of.