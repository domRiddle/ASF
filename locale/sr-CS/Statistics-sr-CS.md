# Statistika

ASF developovanje je podržano od 3 značajne stvari: donacija, povratna mišljenja korisnika i statistike. Donacije direktno utiču na našu volju da radimo na određenom projektu, povratna mišljenja korisnika su lijepa za čitanje (posebno pozitivna), dok nam statistike pružaju uvid u informacije o tome kako je naš softver iskorišćen, i koliko ga ljudi koristi - zbog toga znamo šta da poboljšamo, popravimo, i na čemu da se fokusiramo.

ASF u opštim podešavanjima ima omogućen `Statistics` u globalnoj konfiguraciji. Ako želite da vidite šta će biti u novim verzijama, koji su bugovi popravljeni, i koje nove mogućnosti su dodate, onda omogućiti statistike, da bi smo mogli koristiti te podatke da bi omogućili bolji softver (ali ne samo to). Ovo je veoma važno jer je ASF **besplatan**, i ovo može biti način da kažete hvala - **kažete nam kako koristite ASF**, jer je to što trenutna statistika radi. ASF ne prikuplja i ne koristi podatke koji bi mogli biti privatni i/ili korišćeni protiv vas. Korišćenje statistika je minimalno, i svaka informacija koju prikupimo ima tačno objašnjenje zbog čega je potrebna, i ima praktično objašnjenje. Ovo vam pruža kompletan pregled tog što prikupljamo, za koju potrebu, i kako nam pomaže. Sve ove informacije možete pronaći ispod.

* * *

## Trenutna politika privatnosti

Kada su `Statistics` aktivirane, ove stvari će se desiti:

1. Nalog koji koristite pridružiće se našoj ASF **[Steam grupi](https://steamcommunity.com/gid/103582791440160998)** nakon prijavljivanja. Ovo radimo radi tri razloga:

* Ovo **vam** pruža obavlještenja iz grupe, posebno nove verzije, velike probleme, steam probleme i ostale stvari koje su potrebne da održe zajednicu informisanom (ne šaljemo spam i nevažne stvari za ASF)
* Omogućava **vam** da koristite našu tehničku podršku, postavljajući pitanja, rešavajući probleme, prijavljivajući probleme ili predlažući načine da se nešto popravi
* Omogućava nam da vidimo koliko steam naloga koristi ASF

Mi smatramo da je steam grupa važan dio ASF zajednice. Ovo je naš glavni način komunikacije koji koristimo za sve što je važno za ASF, posebno radi redovnog informisanja sa napretkom, potencijalnim problemima, ponekim upozorenjem i svi ostalim stvarima kojima korisnim treba da ima pristup. Mi nemamo nikakav benefit od održavanja ove grupe, to je mjesto posvećeno ASF korisnicima i mi smatramo vam dijelom te zajednice. Pošto članstvo u ovoj grupi ne identifiše vas kao ASF korisnika, mi ne smatramo da je ovo problem vaše privatnosti.

2. Ako vaš nalog nije **[limitiran](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663)**, imate **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication#asf-2fa)**, **[javni inventar](https://steamcommunity.com/my/edit/settings)** sa minimum 100 `MatchableTypes` itema i ako ste omogućili `SteamTradeMatcher` u vašim `TradingPreferences`, onda će ASF periodično komunicirati sa našim **[serverom](https://asf.justarchi.net)** da bi ostvario tu funkcionalnost. Ti podaci sastoje se od unikatnog ASF ID-a (generisanog od strane ASF-a), i sledećih srodnih informacija:

* Vašeg Steam identifikatora (u 64-bitnoj formi, za generisanje linkova)
* Vašeg nadimka (za svrhe prikazivanja)
* Vašeg avatara (heš, za svrhe prikazivanja)
* Vašeg **[tokena za razmjenu](https://steamcommunity.com/my/tradeoffers/privacy)** (da bi mogli da vršite razmjene sa ljudima koji nisu vaši prijatelji)
* Vašeg `MatchableTypes` (za svrhe prikazivanja i pronalaženja)
* Vrijednost `MatchEverything` iz vaših `TradingPreferences` (za svrhe prikazivanja i pronalaženja)
* Ukupan broj `MatchableTypes` Steam iteam u vašem inventaru (za svrhe prikazivanja i pronalaženja)
* Ukupan broj različitih igrica kojima gornji `MatchableTypes` Steam itemi pripadaju (za svrhe prikazivanja i pronalaženja)

ASF **neće** prikupljati bilo koji podatak koji nije naveden iznad prije nego što to ne navede u važnim obavještenjima u listi promjena, i bez praktičnog razloga za to. Mi ne smatramo da je nešto od ovog iznad ozbiljna stvar, i to kažemo da bi vi znali šta tačno ASF radi pored onoga što sve vi sami podesili, da bi ljudi bolje razumjeli naš pogled na to.

* * *

# Korišćenje podataka

Sve vrijednosti u drugoj tački se koriste za **Javnu ASF STM listu**, koja je objašnjena ispod. Ne koristimo te podatke za bilo koju drugu svrhu.

* * *

## Javna lista ASF STM botova

Naša javna ASF STM lista se nalazi na **[našem websajtu](https://asf.justarchi.net/STM)** i ima svrhu javnog upotrebe od strane nas i ASF korisnika koji koriste funkciju `MatchActively`, kao i ASF korisnika (i onih koji to nisu) radi ručnog poklapanja.

### Kako to tačno radi

ASF šalje podatke jednom kada se prijavi, a to sadrži sva podešavanja koja javna lista može da iskoristi. Onda, svakih 10 minuta ASF šalje jedan, veoma mali zahtjev koji obavještava server da je bot još aktivan i radi. Ako zbog nekog razloga taj zahtjev ne stige, npr. ako image probleme sa mrežom, onda će ASF ponavljati taj zahtjev svakog minuta, sve dok on ne stigne.

Ovo nam omogućava da imamo informacije o tome koga možemo spojiti, kao i da li su još aktivni. Zahvaljujući tome, naš websajt može da prikaže sve ASF 2FA+STM naloge koji su još aktivni **zadnjih 15 minuta**.

Nalozi su sortirani u zavisnosti od njihovog inventara (u nizlaznom redu) - `MatchEverything` botovi sa `Any` banerom koji prihvataju 1:1 razmjene, onda `MatchableTypes` unikatne igrice, i na kraju `MatchableTypes` itemi.

Zapamtite da **nećete** biti prikazani na websajtu ako ne dostižete sve zahtjeve. ASF neće u tom slučaju ni pokušavati da komunicira sa našim serverom, pa je druga tačka skroz preskočena ako niste namjerno omogućili `SteamTradeMatcher` radi pomoći oko razmjene duplikata. Takođe, javna lista je kompatabilna samo sa poslednjom stabilnom verzijom ASF-a i ne prikazuje neažurirane botove, posebno ako ne posjeduju glavne funkcionalnosti koje mogu biti u novijim verzijama.

### API

ASF STM lista trenutno sam prihvata ASF botove. Za sada ne postoji način da navedete botove trećih strana (pošto ne možemo lako da pregledamo njihov kod i zaključimo da se poklapaju sa našom logikom razmjene).

Ako tražite jednostavan način za pristupanje našim listama u programerskom načinu, radi toga imamo veoma jednostavnu **[/Api/Bots](https://asf.justarchi.net/Api/Bots)** tačku koju možete korstiti. Ovo je takođe tačka koju ASF upotrebljava radi korišćenja `MatchActively`.

### Beleške

*Cijeli koncept, zajedno sa integracijom na sajtu i ASF-ovim izbještavanjem je još u beti - može biti usavršem/promijenjen tokom vremena - ili uklonjen ako se ispostavi da nema dovoljno interesovanja za ovu mogućnost.*

* * *

## Izlaženje iz statistika

Učestvovanje u statistikama **nije obavezno**, a je veoma poželjno radi budućnosti ovog programa. Mi vas ne osuđujemo, i ako imate potrebu da sakrijete da sete ASF korisnik onda možete **onemogućiti** statistike mijenanjem vrijednosti `Statistics` u globalnoj konfiguraciji u vrijednost `false`. Onemogućene statistike prave cije modul ne-operativnim, i sve akcije navedene u politici privatnosti biće onemogućene.

Onemogućavanje statistika možda će uticati na **našu tehničku podršku, funkcionalnost ASF-a i ostalih stvari koje su vam ponuđene besplatno**. Na primjer, nećete moći da koristite `MatchActively` be omogućenih `Statistics` (pošto onda odbijate da komunicirate sa našim serverima gdje je ta funkcionalnost ponuđena).