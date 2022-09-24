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

理論上來說，若沒有Root權限則無法讀取受保護的Steam檔案。 The only official non-root method for extracting Steam files is creating unencrypted `/data` backup in one way or another and manually fetching appropriate files from it on your PC, however because such thing highly depends on your phone manufacturer and **is not** in Android standard, we won't discuss it here. If you're lucky to have such functionality, you can make use of it, but majority of users don't have anything like that.

Unofficially, it is possible to extract the needed files without root access, by installing or downgrading your Steam app to version `2.1` (or earlier), setting up mobile authenticator and then creating a snapshot of the app (together with the `data` files that we need) through `adb backup`. However, since it's a serious security breach and entirely unsupported way to extract the files, we won't elaborate further on this, Valve disabled this security hole in newer versions for a reason, and we only mention it as a possibility. Still, it might be possible to do a clean install of that version, link new authenticator, extract the required files, and then upgrade the app, which should be just enough, but you're on your own with this method anyway.

Assuming that you've successfully rooted your phone, you should afterwards download any root explorer available on the market, such as **[this one](https://play.google.com/store/apps/details?id=com.jrummy.root.browserfree)** (or any other one of your preference). You can also access the protected files through ADB (Android Debug Bridge) or any other available to you method, we'll do it through the explorer since it's definitely the most user-friendly way.

Once you opened your root explorer, navigate to `/data/data` folder. Keep in mind that `/data/data` directory is protected and you won't be able to access it without root access. Once there, find `com.valvesoftware.android.steam.community` folder and copy it to your `/sdcard`, which points to your built-in internal storage. Afterwards, you should be able to plug your phone to your PC and copy the folder from your internal storage like usual. If by any chance the folder won't be visible despite you being sure that you copied it to the right place, try restarting your phone first.

Now, you can choose if you want to import your authenticator to WinAuth first, then to ASF, or to ASF right away. First option is more friendly and allows you to duplicate your authenticator also on your PC, allowing you to make confirmations and generate tokens from 3 different places - your phone, your PC and ASF. If you want to do that, simply open WinAuth, add new Steam authenticator and choose importing from Android option, then follow instructions by accessing the files that you've obtained above. When done, you can then import this authenticator from WinAuth to ASF, which is explained in dedicated WinAuth section below.

If you don't want to or don't need to go through WinAuth, then simply copy `files/Steamguard-<SteamID>` file from our protected directory, where `SteamID` is your 64-bit Steam identificator of the account that you want to add (if more than one, because if you have only one account then this will be the only file). You need to place that file in ASF's `config` directory. Once you do that, rename the file to `BotName.maFile`, where `BotName` is the name of your bot you're adding ASF 2FA to. After this step, launch ASF - it should notice the `.maFile` and import it.

```text
[*] INFO: ImportAuthenticator() <1> Converting .maFile into ASF format...
[*] INFO: ImportAuthenticator() <1> Successfully finished importing mobile authenticator!
```

That's all, assuming that you've imported the correct file with valid secrets, everything should work properly, which you can verify by using `2fa` commands. If you made a mistake, you can always remove `Bot.db` and start over if needed.

---

### iOS

For iOS you can use **[ios-steamguard-extractor](https://github.com/CaitSith2/ios-steamguard-extractor)**. This is possible thanks to the fact that you can make decrypted backup, put in on your PC and use the tool in order to extract Steam data that is otherwise impossible to get (at least without jailbreak, due to iOS encryption).

Head over to **[latest release](https://github.com/CaitSith2/ios-steamguard-extractor/releases/latest)** in order to download the program. Once you extract the data you can put it e.g. in WinAuth, then from WinAuth to ASF (although you can also simply copy generated json starting from `{` ending on `}` into `BotName.maFile` and proceed like usual). If you ask me, I strongly recommend to import to WinAuth first, then making sure that both generating tokens as well as accepting confirmations work properly, so you can be sure that everything is alright. If your credentials are invalid, ASF 2FA will not work properly, so it's much better to make ASF import step your last one.

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