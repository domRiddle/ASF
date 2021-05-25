# Statistika

ASF developovanje je podržano od 3 značajne stvari: donacija, povratna mišljenja korisnika i statistike. Donacije direktno utiču na našu volju da radimo na određenom projektu, povratna mišljenja korisnika su lijepa za čitanje (posebno pozitivna), dok nam statistike pružaju uvid u informacije o tome kako je naš softver iskorišćen, i koliko ga ljudi koristi - zbog toga znamo šta da poboljšamo, popravimo, i na čemu da se fokusiramo.

ASF u opštim podešavanjima ima omogućen `Statistics` u globalnoj konfiguraciji. Ako želite da vidite šta će biti u novim verzijama, koji su bugovi popravljeni, i koje nove mogućnosti su dodate, onda omogućiti statistike, da bi smo mogli koristiti te podatke da bi omogućili bolji softver (ali ne samo to). Ovo je veoma važno jer je ASF **besplatan**, i ovo može biti način da kažete hvala - **kažete nam kako koristite ASF**, jer je to što trenutna statistika radi. ASF ne prikuplja i ne koristi podatke koji bi mogli biti privatni i/ili korišćeni protiv vas. Korišćenje statistika je minimalno, i svaka informacija koju prikupimo ima tačno objašnjenje zbog čega je potrebna, i ima praktično objašnjenje. Ovo vam pruža kompletan pregled tog što prikupljamo, za koju potrebu, i kako nam pomaže. Sve ove informacije možete pronaći ispod.

---

## Trenutna politika privatnosti

Kada su `Statistics` aktivirane, ove stvari će se desiti:

1. Nalog koji koristite pridružiće se našoj ASF **[Steam grupi](https://steamcommunity.com/gid/103582791440160998)** nakon prijavljivanja. Ovo radimo radi tri razloga:

* Ovo **vam** pruža obavlještenja iz grupe, posebno nove verzije, velike probleme, steam probleme i ostale stvari koje su potrebne da održe zajednicu informisanom (ne šaljemo spam i nevažne stvari za ASF)
* Omogućava **vam** da koristite našu tehničku podršku, postavljajući pitanja, rešavajući probleme, prijavljivajući probleme ili predlažući načine da se nešto popravi
* Omogućava nam da vidimo koliko steam naloga koristi ASF

Mi smatramo da je steam grupa važan dio ASF zajednice. Ovo je naš glavni način komunikacije koji koristimo za sve što je važno za ASF, posebno radi redovnog informisanja sa napretkom, potencijalnim problemima, ponekim upozorenjem i svi ostalim stvarima kojima korisnim treba da ima pristup. Mi nemamo nikakav benefit od održavanja ove grupe, to je mjesto posvećeno ASF korisnicima i mi smatramo vam dijelom te zajednice. Pošto članstvo u ovoj grupi ne identifiše vas kao ASF korisnika, mi ne smatramo da je ovo problem vaše privatnosti.

2. If your account is **[unrestricted](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663)**, using **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication#asf-2fa)**, has **[public inventory](https://steamcommunity.com/my/edit/settings)** with at least 100 `MatchableTypes` items in it and you intentionally enabled `SteamTradeMatcher` in your `TradingPreferences`, then ASF will periodically communicate with our **[server](https://asf.justarchi.net)** in order to fulfill the enabled functionality. Ti podaci sastoje se od unikatnog ASF ID-a (generisanog od strane ASF-a), i sledećih srodnih informacija:

* Your Steam identificator (in 64-bit form, for generating links, public info)
* Your nickname (for display purposes, public info)
* Your avatar (hash, for display purposes, public info)
* Your **[trading token](https://steamcommunity.com/my/tradeoffers/privacy)** (so people outside of your friendlist can send you trades)
* Vašeg `MatchableTypes` (za svrhe prikazivanja i pronalaženja)
* Vrijednost `MatchEverything` iz vaših `TradingPreferences` (za svrhe prikazivanja i pronalaženja)
* Ukupan broj `MatchableTypes` Steam iteam u vašem inventaru (za svrhe prikazivanja i pronalaženja)
* Ukupan broj različitih igrica kojima gornji `MatchableTypes` Steam itemi pripadaju (za svrhe prikazivanja i pronalaženja)

ASF will **not** collect any other non-listed-above data without prior important notice in the changelog, and a very good practical reason in the first place. Mi ne smatramo da je nešto od ovog iznad ozbiljna stvar, i to kažemo da bi vi znali šta tačno ASF radi pored onoga što sve vi sami podesili, da bi ljudi bolje razumjeli naš pogled na to.

---

# Korišćenje podataka

All values specified in second point are being used for our **Public ASF STM listing**, which is explained below. Ne koristimo te podatke za bilo koju drugu svrhu.

---

## Javna lista ASF STM botova

Our public ASF STM listing is located on **[our website](https://asf.justarchi.net/STM)** and used as a public service for both ASF users that make use of `MatchActively`, as well as ASF and non-ASF users for manual matching.

### Kako to tačno radi

ASF šalje podatke jednom kada se prijavi, a to sadrži sva podešavanja koja javna lista može da iskoristi. Onda, svakih 10 minuta ASF šalje jedan, veoma mali zahtjev koji obavještava server da je bot još aktivan i radi. Ako zbog nekog razloga taj zahtjev ne stige, npr. ako image probleme sa mrežom, onda će ASF ponavljati taj zahtjev svakog minuta, sve dok on ne stigne.

Ovo nam omogućava da imamo informacije o tome koga možemo spojiti, kao i da li su još aktivni. Thanks to that, our website can show all ASF 2FA+STM accounts that were active in **last 15 minutes**.

Nalozi su sortirani u zavisnosti od njihovog inventara (u nizlaznom redu) - `MatchEverything` botovi sa `Any` banerom koji prihvataju 1:1 razmjene, onda `MatchableTypes` unikatne igrice, i na kraju `MatchableTypes` itemi.

Please note that you will **not** be displayed on the website if you do not meet all of the requirements. ASF neće u tom slučaju ni pokušavati da komunicira sa našim serverom, pa je druga tačka skroz preskočena ako niste namjerno omogućili `SteamTradeMatcher` radi pomoći oko razmjene duplikata. Takođe, javna lista je kompatabilna samo sa poslednjom stabilnom verzijom ASF-a i ne prikazuje neažurirane botove, posebno ako ne posjeduju glavne funkcionalnosti koje mogu biti u novijim verzijama.

### API

ASF STM lista trenutno sam prihvata ASF botove. Za sada ne postoji način da navedete botove trećih strana (pošto ne možemo lako da pregledamo njihov kod i zaključimo da se poklapaju sa našom logikom razmjene).

Ako tražite jednostavan način za pristupanje našim listama u programerskom načinu, radi toga imamo veoma jednostavnu **[/Api/Bots](https://asf.justarchi.net/Api/Bots)** tačku koju možete korstiti. Ovo je takođe tačka koju ASF upotrebljava radi korišćenja `MatchActively`.

### Beleške

*Cijeli koncept, zajedno sa integracijom na sajtu i ASF-ovim izbještavanjem je još u beti - može biti usavršem/promijenjen tokom vremena - ili uklonjen ako se ispostavi da nema dovoljno interesovanja za ovu mogućnost.*

---

## Izlaženje iz statistika

Učestvovanje u statistikama **nije obavezno**, a je veoma poželjno radi budućnosti ovog programa. Mi vas ne osuđujemo, i ako imate potrebu da sakrijete da sete ASF korisnik onda možete **onemogućiti** statistike mijenanjem vrijednosti `Statistics` u globalnoj konfiguraciji u vrijednost `false`. Onemogućene statistike prave cije modul ne-operativnim, i sve akcije navedene u politici privatnosti biće onemogućene.

Onemogućavanje statistika možda će uticati na **našu tehničku podršku, funkcionalnost ASF-a i ostalih stvari koje su vam ponuđene besplatno**. Na primjer, nećete moći da koristite `MatchActively` be omogućenih `Statistics` (pošto onda odbijate da komunicirate sa našim serverima gdje je ta funkcionalnost ponuđena).