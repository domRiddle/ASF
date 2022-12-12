# Note

This page is work-in-progress. For translators: you may want to wait a bit (until stable release), as the page is still being written and corrected.

---

# Plugin

`ItemsMatcherPlugin` is official ASF plugin that extends ASF with ASF STM listing features. In particular, this includes `PublicListing` in **[`RemoteCommunication`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#remotecommunication)** and `MatchActively` in **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#tradingpreferences)**.

---

## `PublicListing`

Our public ASF STM listing is located on **[our website](https://asf-backend.justarchi.net/STM)** and used as a public service for both ASF users that make use of `MatchActively`, as well as ASF and non-ASF users for manual matching.

Please note that you will **not** be displayed on the website if you do not meet all of the requirements. ASF won't even bother communicating with our server in this case, so this section is entirely skipped for you if you didn't intentionally enable `SteamTradeMatcher` in order to help yourself match dupes. Also public listing is compatible only with latest stable version of ASF and may refuse to display outdated bots, especially if they're missing core functionality that can be found only in newer versions.

### How it exactly works

ASF sends initial data once after logging in, that contains all properties public listing makes use of. Then, every 10 minutes ASF sends one, very tiny "heartbeat" request that notifies our server that the bot is still up and running. If for some reason the heartbeat didn't arrive, for example due to networking issues, then ASF will retry sending it each minute, until server registers it.

This allows our website to record which accounts can be used for matching, as well as if they're still active. Thanks to that, our website can show all ASF 2FA+STM accounts that were active in **last 15 minutes**.

Users are sorted according to their inventories (in descending order) - `MatchEverything` bots with `Any` banner that accept all 1:1 trades, then `MatchableTypes` unique games count, and finally `MatchableTypes` items count.

### API

ASF STM listing only accepts ASF bots for time being. There is no way to list third-party bots on our listing for now (as we can't review their code easily and ensure they meet our entire trading logic).

If you're looking for easy way to access our listing in programmatic way, we have a very simple **[`/Api/Listing/Bots`](https://asf-backend.justarchi.net/Api/Listing/Bots)** endpoint that you can use. This is also the endpoint that ASF uses internally for `MatchActively` users.

### Privacy policy

If you agree to being listed in our listing, by enabling `SteamTradeMatcher` and not refusing `PublicListing`, as specified above, we'll temporarily store some of your Steam account details on our server in order to provide the core functionality.

Public info (exposed by Steam to every interested party) includes:
- Your Steam identificator (in 64-bit form, for generating links)
- Your nickname (for display purposes)
- Your avatar (hash, for display purposes)

Private info (selected data required for providing the functionality) includes:
- Your **[inventory](https://steamcommunity.com/my/inventory/#753_6)** limited to item types that you've picked in `MatchableTypes` (so people can use `MatchActively` against your items).
- Your **[trading token](https://steamcommunity.com/my/tradeoffers/privacy)** (so people outside of your friendlist can send you trades)
- Your `MaxTradeHoldDuration` (so other people know whether you're willing to accept their trades)
- Your `MatchableTypes` (for display purposes and matching)
- Total number of Steam items in your inventory (for display purposes and matching)

---

## `MatchActively`

`MatchActively` setting is active version of **[`SteamTradeMatcher`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading#steamtradematcher)** which includes interactive matching in which the bot will send trades to other people. It can work standalone, or together with `SteamTradeMatcher` setting. This feature requires `LicenseID` to be set, as it uses third-party server.

Bu seçeneği kullanmak için karşılamanız gereken bir dizi gereksiniminiz vardır. At the minimum you must have **[unrestricted](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663)** account, **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication#asf-2fa)** active and at least one valid type in `MatchableTypes`, such as trading cards.

Yukarıdaki gereksinimlerin tümünü karşılıyorsanız, ASF periyodik olarak **[herkese açık ASF STM listesi](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Remote-communication#public-asf-stm-listing)** ile mevcut botları aktif olarak eşleştirmek için iletişim kuracaktır.

- Her turda ASF, eşleştirilebilecek `MatchableTypes` ögelerini bulmak için listelenmiş seçili botların envanterini ve senin envanterini alacaktır. Eğer eşleşme bulunursa, ASF takas teklifini otomatik olarak gönderecek ve onaylayacaktır.
- Her set (appID'nin bileşimi, ögenin türü ve nadirliği) tek bir turda yalnızca bir kez eşleştirilebilir. Bu, "artık mevcut olmayan ögeleri" en aza indirmek ve tüm takasları göndermeden önce her bir botun tepki vermesini bekleme ihtiyacından kaçınmak için uygulanır. Eşleştirmenin devam eden bir süreçten değil, turlardan oluşmasının birincil nedeni de budur.
- ASF, tek bir roundda tek bir kullanıcıya `255` öğeden ve tek bir kullanıcıya `5` takastan fazla göndermez. Bu, Steam limitlerinin yanı sıra kendi yük dengelememiz tarafından uygulanır.

Bu modülün şeffaf olması gerekiyor. Eşleştirme, ASF başladıktan yaklaşık `1` saat sonra başlayacak ve her `6` saatte bir (gerekirse) kendini tekrar edecektir. `MatchActively` özelliğinin, aktif olarak setlerin tamamlanmasına doğru ilerlediğimizden emin olmak için uzun vadeli, periyodik bir önlem olarak kullanılması hedefleniyor, ancak kısa vadeli zaman ve kaynak baskısı olmadan, bu bir komut olarak girilirse gerçekleşecek. Bu modülün hedef kullanıcıları, birincil hesaplar ve "stash" alt hesaplardır, ancak bu, `MatchEverything` olarak ayarlanmamış herhangi bir bot tarafından kullanılabilir.

ASF, bu seçeneği kullanarak oluşturulan talep ve baskı miktarını en aza indirmek için elinden gelenin en iyisini yapar ve aynı zamanda üst sınıra eşleştirme verimliliğini en üst düzeye çıkarır. Botları eşleştirmenin ve süreci diğer türlü düzenlemek için kesin algoritması, ASF'nin uygulama detayıdır ve geri bildirim, durum ve gelecekteki olası fikirlere göre değişebilir.

Algoritmanın mevcut sürümü, ASF'nin öncelikle `Herhangi bir` bota, özellikle de öğelerinin geldiği oyun çeşitliliği daha iyi olanlara öncelik vermesini sağlar. `Herhangi bir` bot tükendiğinde, ASF, aynı çeşitlilik kuralına göre adil olanlara geçecek ve aşırı sayıda öğeye sahip olanların, diğer botlara kıyasla olası envanterle ilgili sorunların olasılığı daha yüksek olduğu için öncelikleri daha da düşürülecek. Bundan bağımsız olarak, ASF olası bir ilerlemeyi kaçırmamamızı sağlamak için mevcut her botu en az bir kez eşleştirmeye çalışacaktır.

`MatchActively` takes into account bots that you blacklisted from trading through `tbadd` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** and will not attempt to actively match them. Bu, kullanmamız için potansiyel kopyaları olsa bile, ASF'ye hangi botların asla eşleşmemesi gerektiğini söylemek için kullanılabilir.

---

### Why do I need a `LicenseID` to use the plugin? Wasn't `MatchActively` free before?

ASF is, and remains, free and open-source, as it was established at the start of the project back in October 2015. Our program is also entirely non-commercial, we do not earn anything from contributions to it, building or publishing. Over those past 7+ years ASF has received tremendous amount of development, and it's still being improved and enhanced with every monthly stable release mostly by a single person, **[JustArchi](https://github.com/JustArchi)** - with no strings attached. The only funding we receive is from non-obligatory donations that come from our users.

For a very long time, until October 2022, `MatchActively` feature was part of ASF core and available for everyone to use. In October 2022, Valve, the company behind Steam, has put very severe rate limits that rendered previous functionality entirely broken, with no solution available. The feature therefore has been removed from ASF core in version 5.4.1.0.

`MatchActively` was resurrected as part of official `ItemsMatcher` plugin that further enhances ASF with active cards matching functionality. Resurrecting `MatchActively` feature required from us extraordinary amount of work to create ASF backend, entirely new service hosted on a server, with more than a thousand of proxies attached for resolving inventories, all exclusively to allow ASF clients to make use of `MatchActively` like before. Due to the amount of work involved, as well as resources that are not free and require to be paid on monthly basis by us (domain, server, proxies), we've decided to offer this plugin to our sponsors, that is, people that already support ASF project on monthly basis. Our goal isn't to profit from it, but rather, cover the **monthly costs** that are exclusively linked with offering this functionality - that's why we offer it basically for nothing, but we do have to charge a little for it as we can't pay hundreds of dollars from our own pockets just to make it available for you. We hope that you understand.

---

### How can I get an access?

`ItemsMatcher` is offered as part of $5+ sponsor tier on **[JustArchi's GitHub](https://github.com/sponsors/JustArchi)**. Simply become a sponsor of $5 tier (or higher), then click **[here](https://asf-backend.justarchi.net/user/status)** to obtain your `LicenseID`. You'll need to sign in with GitHub for confirming your identity.

The license allows you to send limited amount of requests to the server. $5 tier allows you to use `MatchActively` for one account, which should be suitable for majority of people. $10 tier allows you to use it on three accounts. If you require more resources, **[let us know](mailto:ASF@JustArchi.net)**.

`LicenseID` is made out of 32 hexadecimal characters, such as `f6a0529813f74d119982eb4fe43a9a24`. Simply put it in `LicenseID` property of ASF global config.