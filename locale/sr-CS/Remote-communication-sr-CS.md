# Remote communication

This section elaborates on remote communication that ASF includes, including further explanation on how one can influence it. While we don't consider anything below as malicious or otherwise unwanted, and neither we're legally obliged to disclose it, we want you to better understand the program functionality especially in regards to your privacy and data being shared.

## Steam

ASF communicates with Steam network (**[CM servers](https://api.steampowered.com/ISteamDirectory/GetCMList/v1?cellid=0)**), as well as **[Steam API](api.steampowered.com)**, **[Steam store](https://store.steampowered.com)** and **[Steam community](https://steamcommunity.com)**.

It's not possible to disable any of the above communication, as it's the core foundation ASF is based on in order to provide its basic functionality. You'll need to refrain from using ASF if you're not comfortable with the above.

## Steam group

ASF communicates with our **[Steam group](https://steamcommunity.com/groups/archiasf)**. The group provides you with announcements, especially new versions, critical issues, Steam problems and other things that are important to keep community updated. It also allows you to use our technical support, by asking questions, resolving problems, reporting issues or suggesting improvements. By default, accounts used in ASF will automatically join the group upon login.

You can decide to opt-out of joining the group by disabling `SteamGroup` flag in bot's **[`RemoteCommunication`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#remotecommunication)** settings.

## GitHub

ASF communicates with **[GitHub's API](https://api.github.com)** in order to fetch **[ASF releases](https://github.com/JustArchiNET/ArchiSteamFarm/releases)** for the update functionality. This is done as part of auto-updates (if you've kept **[`UpdatePeriod`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#updateperiod)** enabled), as well as `update` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. You can influence ASF's communication with GitHub through **[`UpdateChannel`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#updatechannel)** property - setting it to `None` will result in disabling entire update functionality, including GitHub communication in this regard.

## ASF's server

ASF communicates with **[our own server](https://asf.justarchi.net)** for more advanced functionality. In particular, this includes:
- Verifying checksums of ASF builds downloaded from GitHub against our own independent database to ensure that all downloaded builds are legitimate (free of malware, MITM attacks or other tampering)
- Announcing your bot in **[our listing](https://asf.justarchi.net/STM)** if you've enabled `SteamTradeMatcher` in **[`TradingPreferences`](#tradingpreferences)** and meet other criteria
- Downloading currently available bots to trade from **[our listing](https://asf.justarchi.net/STM)** if you've enabled `MatchActively` in **[`TradingPreferences`](#tradingpreferences)** and meet other criteria

As a security measure, it's not possible to disable checksum verification for ASF builds. However, you can disable auto-updates entirely if you'd like to avoid this, as described above in the GitHub section.

You can decide to opt-out of being announced in the listing by disabling `PublicListing` flag in bot's **[`RemoteCommunication`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#remotecommunication)** settings. This might be useful if you'd like to run `SteamTradeMatcher` bot without being announced at the same time.

Downloading bots from our listing is mandatory for `MatchActively` setting, you'll need to disable that setting if you're unwilling to accept that.

---

## Javna lista ASF STM botova

Naša javna ASF STM lista se nalazi na **[našem websajtu](https://asf.justarchi.net/STM)** i ima svrhu javne upotrebe za ASF korisnike koji koriste funkciju `MatchActively`, kao i ASF i ne-ASF korisnika radi ručnog poklapanja.

### Kako to tačno radi

ASF šalje podatke jednom kada se prijavi, a to sadrži sva podešavanja koja javna lista može da iskoristi. Onda, svakih 10 minuta ASF šalje jedan, veoma mali zahtjev koji obavještava server da je bot još aktivan i radi. Ako zbog nekog razloga taj zahtjev ne stige, npr. ako image probleme sa mrežom, onda će ASF ponavljati taj zahtjev svakog minuta, sve dok on ne stigne.

Ovo nam omogućava da imamo informacije o tome koga možemo spojiti, kao i da li su još aktivni. Zahvaljujući tome, naš websajt može da prikaže sve ASF 2FA+STM naloge koji su aktivni u **zadnjih 15 minuta**.

Nalozi su sortirani u zavisnosti od njihovog inventara (u nizlaznom redu) - `MatchEverything` botovi sa `Any` banerom koji prihvataju 1:1 razmjene, onda `MatchableTypes` unikatne igrice, i na kraju `MatchableTypes` itemi.

Zapamtite da **nećete** biti prikazani na websajtu ako ne ispunjavate sve zahteve. ASF neće u tom slučaju ni pokušavati da komunicira sa našim serverom, pa je druga tačka skroz preskočena ako niste namjerno omogućili `SteamTradeMatcher` radi pomoći oko razmjene duplikata. Takođe, javna lista je kompatabilna samo sa poslednjom stabilnom verzijom ASF-a i ne prikazuje neažurirane botove, posebno ako ne posjeduju glavne funkcionalnosti koje mogu biti u novijim verzijama.

### API

ASF STM lista trenutno sam prihvata ASF botove. Za sada ne postoji način da navedete botove trećih strana (pošto ne možemo lako da pregledamo njihov kod i zaključimo da se poklapaju sa našom logikom razmjene).

Ako tražite jednostavan način za pristupanje našim listama u programerskom načinu, radi toga imamo veoma jednostavnu **[/Api/Bots](https://asf.justarchi.net/Api/Bots)** tačku koju možete korstiti. Ovo je takođe tačka koju ASF upotrebljava radi korišćenja `MatchActively`.

### Privacy policy

If you agree to being listed in our listing, by enabling `SteamTradeMatcher` and not refusing `PublicListing`, as specified above, we'll temporarily store some of your Steam account details on our server in order to provide the core functionality.

Public info (exposed by Steam to every interested party) includes:
- Vašeg Steam identifikatora (u 64-bitnoj formi, za generisanje linkova)
- Vašeg nadimka (za svrhe prikazivanja)
- Vašeg avatara (heš, za svrhe prikazivanja)
- Ukupan broj `MatchableTypes` Steam iteam u vašem inventaru (za svrhe prikazivanja i pronalaženja)
- Ukupan broj različitih igrica kojima gornji `MatchableTypes` Steam itemi pripadaju (za svrhe prikazivanja i pronalaženja)

Private info (selected data required for providing the functionality) includes:
- Vaš **[token za razmenu](https://steamcommunity.com/my/tradeoffers/privacy)** (da bi ljudi van vaše liste prijatelja mogli da vrše razmenu sa vama)
- Vašeg `MatchableTypes` (za svrhe prikazivanja i pronalaženja)
- Value of `MatchEverything` in your **[`TradingPreferences`](#tradingpreferences)** (for display purposes and matching)

ASF server will **not** collect, store or otherwise process any other data not listed above, without prior important notice in the changelog, and a very good practical reason in the first place. We do not consider anything above to be a serious matter, and we mention it to let you know what precisely ASF does apart of what you configured it to do yourself, so people can better understand the process.

Your data will be automatically hidden from general public in up to 15 minutes since the moment you stop using our listing, whether due to change of settings or not having ASF launched anymore. In addition to that, it'll be automatically deleted from our server (including all backup copies) in up to 7 days since the above happening.