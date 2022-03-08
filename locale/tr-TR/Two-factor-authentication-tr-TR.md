# İki aşamalı kimlik doğrulaması

Valve bir süre önce "Escow" ismiyle bilinen ve hesapla ilgili çeşitli etkinlikler için ekstra kimlik doğrulayıcı gerektiren bir sistem tanıttı. **[Buradan](https://support.steampowered.com/kb_article.php?ref=1284-WTKB-4729)** ve **[buradan](https://support.steampowered.com/kb_article.php?ref=8078-TPHC-6195)** daha fazlasını okuyabilirsiniz. ASF 2FA' nın arkasındaki mantığı anlamaya çalışmadan önce 2FA sistemini anlamak çok önemlidir.

Gördüğünüz gibi tüm takas işlemlerinin 15 güne kadar bekletilmesi; söz konusu ASF olduğunda büyük bir sorun olmaktan çıkıyor, ama özellikle tam otomasyon isteyenler için hala can sıkıcı olabiliyor. Neyse ki ASF bu soruna ASF 2FA adı verilen bir çözüm içeriyor.

---

# ASF Mantığı

Aşağıda açıklanan ASF 2FA' yı kullansanız da kullanmasanız da, ASF uygun mantığı içerir ve standart 2FA ile korunan hesapların tamamen farkındadır. İhtiyaç olduğunda sizden gerekli ayrıntıları isteyecektir.(oturum açma sırasında olduğu gibi). Eğer ASF 2FA kullanıyorsanız, uygulama bu istekleri atlayabilecek ve otomatik olarak gerekli tokenları oluşturabilecek, sizi güçlükten kurtaracak ve ekstra işlevsellik sağlayacak.(aşağıda açıklanan).

---

# ASF 2FA

ASF 2AD, ASF sürecine belirteç oluşturma ve onayları kabul etme gibi 2AD özellikleri sağlamaktan sorumlu dahili bir modüldür. Mevcut kimlik doğrulayıcınızı çoğaltır, böylece geçerli kimlik doğrulayıcınızı ve ASF 2AD'yi aynı anda kullanabilirsiniz.

Bot hesabınızın ASF 2AD'yi zaten kullandığını `2ad` **[komutlarını](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** yürüterek doğrulayabilirsiniz. Kimlik doğrulayıcınızı ASF 2AD olarak almadıysanız, 2ad komutlarının tümü çalışmayacaktır, yani hesabınız ASF 2AD kullanmaz, bu nedenle modülün çalışır durumda olmasını gerektiren gelişmiş ASF özellikleri için de kullanılamaz.

---

## İçe aktarma

ASF 2AD'yi kullanmak için, ASF tarafından desteklenen bağlantı kurulmuş ve çalışan kimlik doğrulayıcıya sahip olmanız gerekir. ASF şu anda birkaç farklı resmi ve resmi olmayan 2AD Android, iOS, SteamDesktopAuthenticator ve WinAuth kaynağını desteklemektedir. Henüz bir kimlik doğrulayıcınız yoksa, bunlardan birini seçip ilk önce onu kurmanız gerekir. Hangisini seçeceğinizi iyi bilmiyorsanız, WinAuth'u öneririz, ancak talimatları uyguladığınızı varsayarsak yukarıdakilerden herhangi biri işinizi görecektir.

Aşağıdaki tüm rehberin, belirli bir araç/uygulama ile birlikte düzgün çalışan bir kimlik doğrulayıcısına ihtiyacı vardır. Geçersiz verileri içe aktarırsanız ASF 2AD düzgün çalışmaz, bu nedenle içe aktarmayı denemeden önce kimlik doğrulayıcınızın düzgün çalıştığından emin olun. Bu, aşağıdaki kimlik doğrulayıcı işlevlerinin düzgün çalışıp çalışmadığını test etmeyi ve doğrulamayı içerir:
- You can generate tokens and those tokens are accepted by Steam network
- You can fetch confirmations, and they are arriving on your mobile authenticator
- You can accept those confirmations, and they're properly recognized by Steam network as confirmed/rejected

Yukarıdaki eylemlerin işe yarayıp yaramadığını kontrol ederek kimlik doğrulayıcınızın çalıştığından emin olun - çalışmazlarsa, ASF'de de çalışmazlar, yalnızca zaman kaybedersiniz ve kendinize ek sorunlara neden olursunuz.

---

### Android telefon

Genel olarak, Android telefonunuzdan kimlik doğrulayıcıyı içe aktarmak için **[root](https://en.wikipedia.org/wiki/Rooting_(Android_OS))** erişimine ihtiyacınız olacaktır. Root işlemi cihazdan cihaza değişir, bu yüzden size cihazınızı nasıl rootlayacağınızı söylemeyeceğim. Ancak root işleminin nasıl yapılacağına dair mükemmel bir rehber sayfası olan **[XDA](https://www.xda-developers.com/root)**'yı zirayet etmenizi önereceğim. Cihazınızı veya ihtiyacınız olan rehberi buradan bulamıyorsanız, o zaman google amcadan bulmaya çalışın.

En azından resmi olarak, korumalı Steam dosyalarına root olmadan erişmek mümkün değildir. Steam dosyalarını ayıklamak için tek resmi olmayan yöntem root yöntemidir. Şu veya bu şekilde şifrelenmemiş veri(`/data`) yedeklemesi oluşturmak ve uygun dosyaları buradan PC'nize manuel olarak getirmektir, ancak böyle bir şey büyük ölçüde telefon üreticinize bağlı olduğundan ve Android standardında **olmadığı** için, burada tartışmayacağız. Böyle bir işlevselliğe zaten sahip olduğunuz için şanslıysanız, bundan yararlanabilirsiniz, ancak kullanıcıların çoğunda bu işlevsellik yoktur.

Gayri resmi olarak, Steam uygulamanızı sürüm 2.1'e (veya daha eski) yükleyerek veya indirerek, mobil kimlik doğrulayıcıyı ayarlayarak ve ardından uygulamanın bir anlık görüntüsünü oluşturarak (ihtiyacımız olan veri (`data`) dosyalarıyla birlikte) gerekli dosyaları root erişimi olmadan çıkarmak `adb backup`. yoluyla mümkündür. Ancak, ciddi bir güvenlik ihlali ve dosyaları çıkarmanın tamamen desteklenmeyen bir yolu olduğu için, bunun üzerinde daha fazla durmayacağız, Valve bu güvenlik açığını yeni sürümlerde bir nedenden dolayı devre dışı bıraktı ve bundan sadece bir olasılık olarak bahsediyoruz.

Telefonunuzu başarılı bir şekilde rootladığınızı varsayarsak, daha sonra **[bunun gibi](https://play.google.com/store/apps/details?id=com.jrummy.root.browserfree)** piyasada bulunan herhangi bir root gezginini (veya tercih ettiğiniz herhangi bir başkasını) indirmelisiniz. Ayrıca, korunan dosyalara ADB (Android Hata Ayıklama Köprüsü) veya kullanabileceğiniz diğer herhangi bir yöntemle erişebilirsiniz, kesinlikle en kullanıcı dostu yol olduğu için bunu gezgin aracılığıyla yapacağız.

Root gezgininizi açtıktan sonra `/data/data` klasörüne gidin. `/data/data` dizininin korunduğunu ve root erişimi olmadan ona erişemeyeceğinizi unutmayın. `com.valvesoftware.android.steam.community` klasörünü bulun ve yerleşik dahili depolama alanınızı gösteren `/sdcard`'ınıza kopyalayın. Daha sonra, telefonunuzu PC'nize bağlayabilmeli ve klasörü her zamanki gibi dahili depolama alanınızdan kopyalayabilmelisiniz. Doğru yere kopyaladığınızdan emin olmanıza rağmen klasör görünmüyorsa, önce telefonunuzu yeniden başlatmayı deneyin.

Şimdi, kimlik doğrulayıcınızı önce WinAuth'a, ardından ASF'ye veya hemen ASF'ye aktarmak isteyip istemediğinizi seçebilirsiniz. İlk seçenek daha kolay ve kimlik doğrulayıcınızı PC'nizde de çoğaltmanıza izin vererek, 3 farklı yerden - telefonunuz, PC'niz ve ASF - onaylar yapmanıza ve belirteçler oluşturmanıza olanak tanır. Bunu yapmak istiyorsanız, sadece WinAuth'u açın, yeni Steam kimlik doğrulayıcı ekleyin ve Android'den içe aktar seçeneğini seçin, ardından yukarıda edindiğiniz dosyalara erişerek talimatları izleyin. Bittiğinde, bu kimlik doğrulayıcıyı WinAuth'tan ASF'ye aktarabilirsiniz; bu, aşağıdaki özel WinAuth bölümünde açıklanmıştır.

WinAuth'u kullanmak istemiyorsanız veya buna gerek duymuyorsanız, `files/Steamguard-SteamID` dosyasını korumalı dizinimizden kopyalamamamız yeterlidir. Burada; eklemek istediğiniz hesabınızın `SteamID`'sini yani 64-bit'lik Steam kimliğini bulup onu eklemeniz gerekmektedir (eğer birden fazla hesabınız varsa burada farklı kimliklerde(Id) birden fazla klasör göreceksiniz. Eğer tek bir hesabınız varsa burada bir adet klasör göreceksiniz). Bu dosyayı ASF'nin yapılandırma(`config`) dizinine yerleştirmeniz gerekiyor. Bunu yaptıktan sonra dosyayı `BotName.maFile` olarak yeniden adlandırın. Burada ki `BotName`, ASF 2AD'yı eklediğiniz botunuzun adıdır. Bu adımdan sonra, ASF'yi başlatın ve ASF `.maFile` dosyasını fark etmeli ve içe aktarmalıdır.

```text
[*] BİLGİ: İçeAktarılanKimlikDoğrulayıcı() <1> .maFile ASF formatına dönüştürülüyor...
[*] BİLGİ: İçeAktarılanKimlikDoğrulayıcı() <1> Mobil kimlik doğrulayıcıyı içe aktarma başarıyla tamamlandı!
```

Hepsi bu kadar, geçerli sırlarla doğru dosyayı içe aktardığınızı varsayarsak, `2ad` komutlarını kullanarak doğrulayabileceğiniz her şey düzgün çalışmalıdır. Bir hata yaptıysanız, her zaman `Bot.db` 'yi kaldırabilir ve gerekirse baştan başlayabilirsiniz.

---

### iOS

iOS için **[ios-steamguard-extractor](https://github.com/CaitSith2/ios-steamguard-extractor)** kullanabilirsiniz. Bu, şifresi çözülmüş yedekleme yapabilmeniz, PC'nize koyabilmeniz ve aksi takdirde elde edilmesi imkansız olan Steam verilerini (en azından iOS şifrelemesi nedeniyle jailbreak olmadan) çıkarmak için aracı kullanabilmeniz sayesinde mümkündür.

Programı indirmek için **[en son sürüme](https://github.com/CaitSith2/ios-steamguard-extractor/releases/latest)** gidin. Verileri çıkardıktan sonra, ör. WinAuth'da, ardından WinAuth'tan ASF'ye (yine de, `{` ile biten `}` ile başlayıp oluşturulan json'u `BotName.maFile` içine kopyalayabilir ve her zamanki gibi ilerleyebilirsiniz). Bana sorarsanız, önce WinAuth'a aktarmanızı, ardından hem token oluşturmanın hem de onayları kabul etmenin düzgün çalıştığından emin olmanızı şiddetle tavsiye ederim, böylece her şeyin yolunda olduğundan emin olabilirsiniz. Kimlik bilgileriniz geçersizse, ASF 2AD düzgün çalışmayacaktır, bu nedenle ASF içe aktarma adımını son adımınız yapmak çok daha iyidir.

Sorularınız/sorunlarınız için lütfen **[sorunları](https://github.com/CaitSith2/ios-steamguard-extractor/issues)** ziyaret edin.

*Yukarıdaki aracın gayri resmi olduğunu, riski size ait olmak üzere kullandığınızı unutmayın. Düzgün çalışmıyorsa teknik destek sağlamıyoruz. geçersiz 2FA kimlik bilgilerini dışa aktardığına dair birkaç sinyal aldık. bu verileri ASF'ye aktarmadan önce onayların WinAuth gibi bir kimlik doğrulayıcıda çalıştığını doğrulayın!*

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

## Bitti

From this moment, all `2fa` commands will work as they'd be called on your classic 2FA device. You can use both ASF 2FA and your authenticator of choice (Android, iOS, SDA or WinAuth) to generate tokens and accept confirmations.

If you have authenticator on your phone, you can optionally remove SteamDesktopAuthenticator and/or WinAuth, as we won't need it anymore. However, I suggest to keep it just in case, not to mention that it's more handy than normal steam authenticator. Just keep in mind that ASF 2FA is **NOT** a general purpose authenticator and it should **never** be the only one you use, since it doesn't even include all data that authenticator should have. It's not possible to convert ASF 2FA back to original authenticator, therefore always make sure that you have general-purpose authenticator in other place, such as in WinAuth/SDA, or on your phone.

---

## FAQ

### How is ASF making use of 2FA module?

If ASF 2FA is available, ASF will use it for automatic confirmation of trades that are being sent/accepted by ASF. It will also be capable of automatically generating 2FA tokens on as-needed basis, for example in order to log in. In addition to that, having ASF 2FA also enables `2fa` commands for you to use. That should be all for now, if I didn't forget about anything - basically ASF uses 2FA module on as-needed basis.

---

### What if I need a 2FA token?

You will need 2FA token to access 2FA-protected account, that includes every account with ASF 2FA as well. You should generate tokens in authenticator that you used for import, but you can also generate temporary tokens through `2fa` command sent via the chat to given bot. You can also use `2fa <BotNames>` command to generate temporary token for given bot instances. This should be enough for you to access bot accounts through e.g. browser, but as noted above - you should use your friendly authenticator (Android, iOS, SDA or WinAuth) instead.

---

### Can I use my original authenticator after importing it as ASF 2FA?

Yes, your original authenticator remains functional and you can use it together with using ASF 2FA. That's the whole point of the process - we're importing your authenticator credentials into ASF, so ASF can make use of them and accept selected confirmations on your behalf.

---

### Where is ASF mobile authenticator saved?

ASF mobile authenticator is saved in `BotName.db` file in your config directory, along with some other crucial data related to given account. If you want to remove ASF 2FA, read how below.

---

### How to remove ASF 2FA?

Simply stop ASF and remove associated `BotName.db` of the bot with linked ASF 2FA you want to remove. This option will remove associated imported 2FA with ASF, but will NOT delink your authenticator. If you instead want to delink your authenticator, apart from removing it from ASF (firstly), you should delink it in authenticator of your choice (Android, iOS, SDA or WinAuth), or - if you can't for some reason, use revocation code that you received during linking that authenticator, on the Steam website. It's not possible to unlink your authenticator through ASF, this is what general-purpose authenticator that you already have should be used for.

---

### I linked authenticator in SDA/WinAuth, then imported to ASF. Can I now unlink it and link it again on my phone?

**No**. ASF **imports** your authenticator data in order to use it. If you delink your authenticator then you'll also cause ASF 2FA to stop functioning, regardless if you remove it firstly like stated in above question or not. If you want to use your authenticator on both your phone and ASF (plus optionally in SDA/WinAuth), then you'll need to **import** your authenticator from your phone, and not create new one in SDA/WinAuth. You can have only **one** linked authenticator, that's why ASF **imports** that authenticator and its data in order to use it as ASF 2FA - it's **the same** authenticator, just existing in two places. If you decide to delink your mobile authenticator credentials - regardless in which way, ASF 2FA will stop working, as previously copied mobile authenticator credentials will no longer be valid. In order to use ASF 2FA together with authenticator on your phone, you must import it from Android/iOS, which is described above.

---

### Is using ASF 2FA better than WinAuth/SDA/Other authenticator set to accept all confirmations?

**Yes**, in several ways. First and most important one - using ASF 2FA **significantly** increases your security, as ASF 2FA module ensures that ASF will only accept automatically its own confirmations, so even if attacker does request a trade that is harmful, ASF 2FA will **not** accept such trade, as it was not generated by ASF. In addition to security part, using ASF 2FA also brings performance/optimization benefits, as ASF 2FA fetches and accepts confirmations immediately after they're generated, and only then, as opposed to inefficient polling for confirmations each X minutes done e.g. by SDA or WinAuth. In short, there is no reason to use third-party authenticator over ASF 2FA, if you plan on automating confirmations generated by ASF - that's exactly what ASF 2FA is for, and using it does not conflict with you confirming everything else in authenticator of your choice. We strongly recommend to use ASF 2FA for entire ASF activity - this is much more secure than any other solution.

---

## Gelişmiş

If you're advanced user, you can also generate maFile manually. This can be used in case you'd want to import authenticator from other sources than the ones we've described above. It should have a **[valid JSON structure](https://jsonlint.com)** of:

```json
{
  "shared_secret": "STRING",
  "identity_secret": "STRING"
}
```

Standard authenticator data has more fields - they're entirely ignored by ASF during import, as they're not needed. You don't have to remove them - ASF only requires valid JSON with 2 mandatory fields described above, and will ignore additional fields (if any). Of course, you need to replace `STRING` placeholder in the example above with valid values for your account.