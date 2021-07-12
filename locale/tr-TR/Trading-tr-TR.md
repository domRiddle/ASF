# Takas

ASF, Steam'in etkileşimli olmayan (çevrimdışı) takaslarına destek sağlar. Hem alım (kabul/reddetme) hem de takas gönderme hemen yapılabilir ve özel yapılandırma gerektirmez, ancak şüphesiz bir şekilde sınırlandırılmamış (mağazada 5$ harcamış olan) Steam hesabı gerektirir. Sınırlandırılmış hesaplarda takas modülüne erişilemez.

---

## Mantık

ASF, bota `Ana Kullanıcı` (veya daha yüksek) erişimi olan kullanıcıdan, ögelerden bağımsız olarak gönderilen tüm takasları her zaman kabul edecektir. Bu, yalnızca bot istemcisi tarafından farmlanan Steam kartlarının kolayca lootlanmasına izin vermekle kalmaz, aynı zamanda, envanterde saklanan Steam ögelerini -(CS:GO gibi olan) diğer oyunlar da dahil olmak üzere- kolayca yönetebilmeyi sağlar.

ASF, takas modülünden kara listeye alınmış herhangi bir (ana kullanıcı olmayan) kullanıcıdan gelen alım satım teklifini içerikten bağımsız olarak reddedecektir. Kara liste, standart `BotName.db` veritabanında saklanır ve `bl`, `bladd` ve `blrm` **[komutlarıyla](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** yönetilebilir. Bu, Steam tarafından sunulan standart kullanıcı bloğuna bir alternatif olarak çalışmalıdır - dikkatli kullanın.

ASF, `TradingPreferences` içinde `DontAcceptBotTrades` belirtilmedikçe, botlar arasında gönderilen tüm `loot` gibi işlemleri kabul eder. Kısaca, `TradingPreferences`'in varsayılan değeri olan `None`, ASF'nin bota `Ana Kullanıcı` erişimi olan (yukarıda açıklanmıştır) kullanıcıdan gelen işlemleri (ASF içerisinde yer alan diğer botlardan yapılan tüm bağış işlemleri gibi) otomatik olarak kabul etmesine neden olur. Diğer botlardan yapılan bağış takaslarını devre dışı bırakmak istiyorsanız, `TradingPreferences`'ın `DontAcceptBotTrades` ayarı bunun için bulunmakta.

`TradingPreferences`'ın `Donations` ayarını etkinleştirdiğinizde, ASF, bot hesaplarından herhangi birinin öge kaybetmediği herhangi bir takası kabul eder. Bot hesapları `DontAcceptBotTrades` tarafından etkilendiğinden, bu özellik yalnızca bot olmayan hesapları etkiler. `AcceptDonations`, diğer insanlardan ve ayrıca ASF içerisinde yer almayan botlardan kolayca bağış kabul etmenizi sağlar.

`AcceptDonations`'un **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** gerektirmediğini belirtmek güzel, çünkü herhangi bir öge kaybetmezsek onay gerekmez.

Ayrıca, `TradingPreferences`'ı buna göre değiştirerek ASF takas modülünü daha da özelleştirebilirsiniz. Ana `TradingPreferences` özelliklerinden biri olan `SteamTradeMatcher` seçeneği, ASF'nin eksik rozetleri tamamlamanıza yardımcı olan alım satımları kabul etmek için yerleşik mantığı kullanmasına neden olur. Bu özellik, **[SteamTradeMatcher](https://www.steamtradematcher.com)**'ın herkese açık listelemesiyle işbirliği için özellikle yararlıdır ancak onsuz da çalışabilir. Aşağıda ayrıca açıklanmıştır.

---

## `SteamTradeMatcher`

`SteamTradeMatcher` etkin olduğunda, ASF, takasın STM kurallarını geçip geçmediğini ve en azından bize karşı tarafsız olup olmadığını kontrol etmek için oldukça karmaşık bir algoritma kullanır. Asıl mantık aşağıdaki gibidir:

- `MatchableTypes`'da belirtilen öge türleri dışında bir şey kaybedersek takası reddet.
- Oyun başına, tür başına ve nadirlik bazında en az aynı sayıda öge almıyorsak takası reddet.
- Kullanıcı özel Steam yaz/kış indirim kartları isterse ve takas beklemeye alınırsa takası reddet.
- Beklemede tutma süresi `MaxTradeHoldDuration` global yapılandırma özelliğini aşarsa takası reddet.
- Eğer `MatchEverything` ayarlı değilse, takası reddet. Bu bizim için nötrden daha kötü.
- Yukarıdakilerden herhangi biri sebebiyle reddetmediysek, takası kabul et.

ASF'nin fazla ödemeyi de desteklediğini belirtmek güzel - yukarıdaki tüm koşullar karşılandığı sürece, kullanıcı ticarete fazladan bir şey eklediğinde mantık düzgün çalışacaktır.

İlk 4 red beyanı herkes için anlaşılır olmalı. Sonuncusu, envanterinizin mevcut durumunu kontrol eden ve ticaretin durumunun ne olduğuna karar veren gerçek kopyalar mantığını içerir.

- Belirlenen tamamlama yönünde ilerlememiz varsa, ticaret **iyidir**. Example: A A (before) -> A B (after)
- Set tamamlama yolundaki ilerlemeniz değişmeden kalırsa, ticaret **nötr** olur. Example: A B (before) -> A C (after)
- Set tamamlama yönünde ilerlemeniz azalırsa, ticaret **kötü** olur. Example: A C (before) -> A A (after)

STM yalnızca iyi takaslarda çalışır, bu da kopyaların eşleştirmesi için STM kullanan kullanıcının bizim için her zaman yalnızca iyi takaslar önermesi gerektiği anlamına gelir. Ancak, ASF liberaldir ve aynı zamanda nötr takasları da kabul eder, çünkü bu takaslarda aslında hiçbir şey kaybetmiyoruz, dolayısıyla onları reddetmek için geçerli bir sebep yok. Bu, özellikle arkadaşlarınız için yararlıdır, çünkü herhangi bir ilerleme kaydetmediğiniz sürece, STM kullanmadan fazla kartlarınızı takaslayabilirler.

Varsayılan olarak ASF kötü takasları reddeder - bu neredeyse her zaman bir kullanıcı olarak istediğiniz şeydir. Ancak, ASF'nin **kötü** olanlar da dahil olmak üzere tüm kopya takaslarını kabul etmesini sağlamak için `TradingPreferences`'ı `MatchEverything` olarak ayarlayabilirsiniz. Bu yalnızca, hesabınız altında 1:1 ticaret botu çalıştırmak istiyorsanız ve eğer **ASF'nin artık rozet tamamlama yolunda ilerlemenize yardımcı olmayacağını ve aynı kartın N kopyası için bitmiş setin tamamını kaybetme eğiliminde olmanıza neden olacağını** anladığınızda kullanışlıdır. Eğer herhangi bir seti **Asla** bitirmesi beklenmeyen bir ticaret botunu kasıtlı olarak çalıştırmak istemiyorsanız, bu seçeneği etkinleştirmek istemezsiniz.

Seçtiğiniz `TradingPreferences` ne olursa olsun, bir işlemin ASF tarafından reddedilmiş olması, onu kendiniz kabul edemeyeceğiniz anlamına gelmez. `BotBehaviour`'un varsayılan değerini koruduysanız ki bu değer `RejectInvalidTrades`'e eşit değil, ASF bu işlemleri yok sayar - bu işlemlerle ilgilenip ilgilenmediğinize kendiniz karar vermenize olanak tanır. Aynı şey, `MatchableTypes` dışındaki ögelerle yapılan takaslar ve diğer her şey için de geçerlidir - modülün, neyin iyi olup neyin olmadığına karar vermek yerine STM işlemlerini otomatikleştirmenize yardımcı olması beklenir. Bu kuralın tek istisnası, `bladd` komutunu kullanarak işlem modülünden kara listeye aldığınız kullanıcılardır. Bu kullanıcılardan yapılan işlemler, `BotBehaviour` ayarlarından bağımsız olarak hemen reddedilir.

Bu seçeneği etkinleştirdiğinizde, **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)**'nın kullanılması önemle tavsiye edilir. Eğer her işlemi manuel olarak onaylamaya karar verirseniz, bu işlev tüm potansiyelini kaybeder. `SteamTradeMatcher`, alım satımları onaylama yeteneği olmadan bile düzgün şekilde çalışacaktır, ancak bunları zamanında kabul etmezseniz, birikmiş onaylar oluşturabilir.

---

### `MatchActively`

`MatchActively` ayarı, `SteamTradeMatcher`'ın genişletilmiş versiyonudur ve bu seçeneğin sunduğu pasif eşleştirmeye ek olarak, botun diğer kişilere işlem göndereceği aktif eşleştirmeyi de içerir.

Bu seçeneği kullanmak için karşılamanız gereken bir dizi gereksiniminiz vardır. İlk olarak, `SteamTradeMatcher`'ı etkinleştirmeniz gerekir (çünkü bu özellik bunun bir uzantısıdır) ve `MatchEverything`'in **devre dışı** olduğundan emin olun. (ticaret botları hiçbir zaman aktif olarak eşleşmez). Ardından, gereksinimleri biraz gevşeterek **[ASF STM listemiz](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Statistics#current-privacy-policy)** için uygun olmanız gerekir. En azından `Statistics`'i etkinleştirmiş olmalısınız, **[Sınırsız](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663)** bir Steam hesabı, **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication#asf-2fa)** etkin ve `MatchableTypes`'da ticaret kartları gibi en az bir geçerli tür bulunmalı.

Yukarıdaki gereksinimlerin tümünü karşılıyorsanız, ASF periyodik olarak **[herkese açık ASF STM listesi](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Statistics#public-asf-stm-listing)** ile mevcut botları aktif olarak eşleştirmek için iletişim kuracaktır.

- Her eşleştirme oturumu, tek bir eşleştirme oturumunda maksimum `10` tane olmak üzere "turlardan" oluşur.
- Her turda ASF, eşleştirilebilecek `MatchableTypes` ögelerini bulmak için listelenmiş seçili botların envanterini ve senin envanterini alacaktır. Eğer eşleşme bulunursa, ASF takas teklifini otomatik olarak gönderecek ve onaylayacaktır.
- Her set (appID'nin bileşimi, ögenin türü ve nadirliği) tek bir turda yalnızca bir kez eşleştirilebilir. Bu, "artık mevcut olmayan ögeleri" en aza indirmek ve tüm takasları göndermeden önce her bir botun tepki vermesini bekleme ihtiyacından kaçınmak için uygulanır. Eşleştirmenin devam eden bir süreçten değil, turlardan oluşmasının birincil nedeni de budur.
- ASF, tek bir roundda tek bir kullanıcıya `255` öğeden ve tek bir kullanıcıya `5` takastan fazla göndermez. Bu, Steam limitlerinin yanı sıra kendi yük dengelememiz tarafından uygulanır.
- Eğer eşleşmek için setlerin bitmesi nedeniyle daha önce iptal edilmezse, ASF'nin tek bir turda eşleştirilebilecek `40` benzersiz botluk sabit bir sınırı vardır. Eğer iptal edilirse, bir sonraki turda ASF, öncelikle henüz eşleşmeyen botları eşleştirmeye çalışacaktır.
- ASF eşleşmenin devam etmesi gerektiğine karar verirse, bir sonraki tur, son turdan itibaren `5` dakika içinde başlar (biraz bekleme süresi eklemek ve tüm botların takaslara tepki vermesini sağlamak için), aksi takdirde eşleştirme oturumu biter ve `8` saat içinde kendini tekrar eder.

Bu modülün şeffaf olması gerekiyor. Eşleştirme, ASF başladıktan yaklaşık `1` saat sonra başlayacak ve her `8` saatte bir (gerekirse) kendini tekrar edecektir. `MatchActively` özelliğinin, aktif olarak setlerin tamamlanmasına doğru ilerlediğimizden emin olmak için uzun vadeli, periyodik bir önlem olarak kullanılması hedefleniyor, ancak kısa vadeli zaman ve kaynak baskısı olmadan, bu bir komut olarak girilirse gerçekleşecek. Bu modülün hedef kullanıcıları, birincil hesaplar ve "stash" alt hesaplardır, ancak bu, `MatchEverything` olarak ayarlanmamış herhangi bir bot tarafından kullanılabilir.

ASF, bu seçeneği kullanarak oluşturulan talep ve baskı miktarını en aza indirmek için elinden gelenin en iyisini yapar ve aynı zamanda üst sınıra eşleştirme verimliliğini en üst düzeye çıkarır. Botları eşleştirmenin ve süreci diğer türlü düzenlemek için kesin algoritması, ASF'nin uygulama detayıdır ve geri bildirim, durum ve gelecekteki olası fikirlere göre değişebilir.

Algoritmanın mevcut sürümü, ASF'nin öncelikle `Herhangi bir` bota, özellikle de öğelerinin geldiği oyun çeşitliliği daha iyi olanlara öncelik vermesini sağlar. `Herhangi bir` bot tükendiğinde, ASF, aynı çeşitlilik kuralına göre adil olanlara geçecek ve aşırı sayıda öğeye sahip olanların, diğer botlara kıyasla olası envanterle ilgili sorunların olasılığı daha yüksek olduğu için öncelikleri daha da düşürülecek. Bundan bağımsız olarak, ASF olası bir ilerlemeyi kaçırmamamızı sağlamak için mevcut her botu en az bir kez eşleştirmeye çalışacaktır.

`MatchActively`, `bladd` **[komutuyla](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** ticaretten kara listeye aldığınız botları hesaba katar ve onları aktif olarak eşleştirmeye çalışmaz. Bu, kullanmamız için potansiyel kopyaları olsa bile, ASF'ye hangi botların asla eşleşmemesi gerektiğini söylemek için kullanılabilir.