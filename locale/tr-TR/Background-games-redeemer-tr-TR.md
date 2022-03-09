# Arka plan oyun etkinleştirici

Arkaplan oyunları etkinleştiricisi, arka planda kullanılmak üzere verilen Steam cd-anahtarlarının (isimleri ile birlikte) içe aktarılabilmesini sağlayan özel bir ASF özelliğidir. Bu özellikle, çok sayıda anahtarınız varsa ve tüm toplu işleminiz tamamlanmadan önce `RateLimited` **[durumunu](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ#what-is-the-meaning-of-status-when-redeeming-a-key)** ulaşmayı garantilediyseniz kullanışlıdır.

Arkaplan oyunları kurtarıcısı, tek bir bot alanına sahip olacak şekilde yapılır, bu da `RedeemingPreferences`'dan yararlanmadığı anlamına gelir. Bu özellik, gerekirse, `redeem` **[komutu](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** ile birlikte (veya bunun yerine) kullanılabilir.

---

## İçe Aktarma

İçe aktarma işlemi iki yolla yapılabilir - bir dosya kullanarak veya IPC kullanarak.

### Dosya

ASF, kendi `yapılandırma` dizininde `BotName.keys` adlı bir dosyayı, `BotName` botunuzun adı olduğunu görecektir. Bu dosya, girişi belirtmek için birbirinden bir sekme (tab) karakteriyle ayrılmış şekilde oyun adı ve cd-key içeren, sonu yeni bir satırla (\n) biten satırlar içerir. Birden çok sekme kullanılıyorsa, o halde ilk giriş oyunun adı olarak kabul edilir, son giriş bir cd-key olarak kabul edilir ve aradaki her şey göz ardı edilir. Örneğin:

```text
POSTAL 2    ABCDE-EFGHJ-IJKLM
Domino Craft VR 12345-67890-ZXCVB
A Week of Circus Terror POIUY-KJHGD-QWERT
Terraria    ThisIsIgnored   ThisIsIgnoredToo    ZXCVB-ASDFG-QWERT
```

Alternatif olarak, yalnızca cd-key de kullanabilirsiniz (yine her giriş arasında yeni bir satırla). Bu durumda ASF, doğru ismi doldurmak için Steam'in yanıtını (mümkünse) kullanacaktır. Her türlü anahtar etiketleme için, Steam'de kullanılan paketlerin etkinleştirdikleri oyunların mantığını takip etmesi gerekmediğinden, geliştiricinin ne koyduğuna bağlı olarak, doğru oyunu görebilirsiniz. adlar, özel paket adları (örn. Humble Indie Bundle 18) veya tamamen yanlış ve potansiyel olarak kötü niyetli olanlar (örn. Half-Life 4).

```text
ABCDE-EFGHJ-IJKLM
12345-67890-ZXCVB
POIUY-KJHGD-QWERT
ZXCVB-ASDFG-QWERT
```

Hangi biçime bağlı kalmaya karar vermiş olursanız olun, ASF, bot başlangıcında veya daha sonra yürütme sırasında `keys` dosyanızı içe aktarır. Dosyanızın başarılı bir şekilde ayrıştırılmasından ve geçersiz girişlerin atlanmasından sonra, düzgün bir şekilde algılanan tüm oyunlar arka plan sırasına eklenecek ve `BotName.keys` dosyasının kendisi `yapılandırma` dizininden kaldırılacaktır.

### IPC

ASF, yukarıda bahsi geçen anahtarların kullanılmasına ek olarak, herhangi bir IPC aracı tarafından yürütülebilir, `GamesToRedeemInBackground` **[ASF API uç noktasını](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#asf-api)** ASF-ui'da dahil etmek üzere. Using IPC could be more powerful, as you can do appropriate parsing yourself, such as using a custom delimiter instead of being forced to a tab character, or even introducing your entirely own customized keys structure.

---

## Kuyruk

Oyunlar başarıyla içe aktarıldığında, sıraya eklenirler. Bot, Steam ağına bağlı olduğu sürece ve kuyruk boş değilse ASF otomatik olarak arka plan kuyruğundan geçer. A key that was attempted to be redeemed and did not result in `RateLimited` is removed from the queue, with its status properly written to a file in `config` directory - either `BotName.keys.used` if the key was used in the process (e.g. `NoDetail`, `BadActivationCode`, `DuplicateActivationCode`), or `BotName.keys.unused` otherwise. ASF intentionally uses your provided game's name since key is not guaranteed to have a meaningful name returned by Steam network - this way you can tag your keys using even custom names if needed/wanted.

If during the process our account hits `RateLimited` status, the queue is temporarily suspended for a full hour in order to wait for cooldown to disappear. Afterwards, the process continues where it left, until the entire queue is empty.

---

## Örnek

100 anahtarlık bir listeniz olduğunu varsayalım. Öncelikle ASF `config` dizininde yeni bir `BotName.keys.new` dosyası oluşturmalısınız. ASF'ye bu dosyayı oluşturulduğu anda almaması gerektiğini bildirmek için `.new` uzantısını ekledik (çünkü yeni boş dosya olduğundan, içe aktarmaya henüz hazır değil).

Artık yeni dosyamızı açabilir ve 100 anahtarımızın kopyala-yapıştır listesini orada açabilir, gerekirse formatı düzeltebilirsiniz. Düzeltmelerden sonra `BotName.keys.new` dosyamız tam olarak 100 (veya son satırsonu ile birlikte 101) satıra sahip olacak ve her satır `OyunAdı\tcd-key\n` yapısına sahip olacak. Burada `\t` sekme karakteri ve `\n` yeni satırdır.

ASF'nin dosyanın alınmaya hazır olduğunu bilmesini sağlamak için artık bu dosyayı `BotName.keys.new` yerine `BotName.keys` olarak yeniden adlandırmaya hazırsınız. Bunu yaptığınız anda, ASF dosyayı otomatik olarak içe aktarır (yeniden başlatmaya gerek kalmadan), tüm oyunlarımızın ayrıştırıldığını ve sıraya eklendiğini onayladıktan sonra sonra siler.

`BotName.keys` dosyasını kullanmak yerine, IPC API uç noktasını da kullanabilir, hatta isterseniz ikisini birleştirebilirsiniz.

After some time, `BotName.keys.used` and `BotName.keys.unused` files will be generated. Those files contain results of our redeeming process. For example, you could rename `BotName.keys.unused` into `BotName2.keys` file and therefore submit our unused keys for some other bot, since previous bot didn't make use of those keys himself. Or you could simply copy-paste unused keys to some other file and keep it for later, your call. Keep in mind that as ASF goes through the queue, new entries will be added to our output `used` and `unused` files, therefore it's recommended to wait for the queue to be fully emptied before making use of them. If you absolutely must access those files before queue is fully emptied, you should firstly **move** output file you want to access to some other directory, **then** parse it. This is because ASF can append some new results while you're doing your thing, and that could potentially lead to loss of some keys if you read a file having e.g. 3 keys inside, then delete it, totally missing the fact that ASF added 4 other keys to your removed file in the meantime. If you want to access those files, ensure to move them away from ASF `config` directory before reading them, for example by rename.

It's also possible to add extra games to import while having some games already in our queue, by repeating all above steps. ASF will properly add our extra entries to already-ongoing queue and deal with it eventually.

---

## Notlar

Background keys redeemer uses `OrderedDictionary` under the hood, which means that your cd-keys will have preserved order as they were specified in the file (or IPC API call). This means that you can (and should) provide a list where given cd-key can only have direct dependencies on cd-keys listed above, but not below. For example, this means that if you have DLC `D` that requires game `G` to be activated firstly, then cd-key for game `G` should **always** be included before cd-key for DLC `D`. Likewise, if DLC `D` would have dependencies on `A`, `B` and `C`, then all 3 should be included before (in any order, unless they have dependencies on their own).

Not following the scheme above will result in your DLC not being activated with `DoesNotOwnRequiredApp`, even if your account would be eligible for activating it after going through its entire queue. If you want to avoid that then you must make sure that your DLC is always included after the base game in your queue.