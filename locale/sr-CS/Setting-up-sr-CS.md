# Podešavanje

Ako ste ovdje prvi put, dobro došli! Srećni smo da vidimo još nekog ko je zainteresovan za naš projekat, ali zapamtite da sa velikom moći dolazi velika odgovornost - ASF ima mogućnost da kontroliše mnogo stvari na Steam-u, ali samo ako **pazite kako da ga naučite**. Strma je linija učenja, a mi očekujemo od vas da pročitate wiki-u zbog toga, koja objašnjava u detalju kako sve radi.

Ako ste još ovdje znači da ste izdržali tekst iznad, što je lijepo. Ako ste je preskočili, onda će vam uskoro biti **[loše](https://www.youtube.com/watch?v=WJgt6m6njVw)**... Anyway, ASF is a console app, which means that the program itself doesn't have a friendly GUI that you're in general used to, at least out of the box. ASF je najviše namijenjen da se koristi na serveru, pa zbog toga izgleda kao servis (daemon) a ne kao desktop aplikacija.

Ovo ipak ne znači da ga ne možete koristiti na vašem PC-u ili na nečem više komplikovanom nego obično. ASF je zaseban program koji ne zahtijeva instalaciju, i odmah radi ali zahtijeva konfiguraciju da bi vam bio od koristi. Konfiguracija kazuje ASF-u šta u stvari treba da radi nakon što ga pokrenete. Ako ga pokrenete bez konfiguracije, ASF onda neće raditi ništa, tako jednostavno.

---

## OS-specifično postavljanje

Opšte kazano, ovo ćete raditi u nekoliko sledećih minuta:
- Instalirati **[.NET Core prerequisites](#net-core-prerequisites)**.
- Preuzmite **[poslednje ASF izdanje](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** u odgovarajućoj OS-specifičnoj varijanti.
- ekstraktujte arhivu u novoj lokaciji (i `chmod +x ArchiSteamFarm` ako ste na Linux-u/OS X-u),
- **[Konfigurišete ASF](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)**.
- pokrenite ASF i vidite magiju.

Zvuči jednostavno, zar ne? Pa počnimo.

---

### .NET Core prerequisites

Prvo morate biti sigurni da ASF može biti pokrenut na vašem OS-u. ASF je napisan u C#, baziran na .NET Core i možda će zahtijevati dodatne nativne biblioteke koje još nisu prisutne na vašoj platformi. Razlika postoji u tome da li koristite Windows, Linux ili OS X, i zbog toga će imati različite zahtjeve, ali u svakom slučaju, svi su navedeni u **[.NET Core prerequisites](https://docs.microsoft.com/dotnet/core/install)** dokumentu kojeg trebate pratiti. Ovo je naš materijal za podsjećanje koji treba biti korišćen, ali zbog jednostavnosti mi smo naveli sve pakete ispod, pa ne morate čitati čitav dokument.

Normalno je da neki (ili svi) zahtjevi već postoje na vašem sistemu zbog toga što ih je neki treći softver, koji već koristite, instalirao. Ipak, budite sigurni da stvarno imate potrebne zahtjeve na vašem OS-u - bez tih zahtjeva ASF se uopšte neće pokrenuti.

Zapamtite da, osim ovog, ne morate ništa drugo raditi na vašem OS-u, posebno ne instalirati .NET Core SDK ili runtime, jer OS paket već sadrži to sve. Potrebni su vam samo .NET Core prerequisites (zahtjevi) da bi pokrenuli .NET Core runtime koje je u ASF-u.

#### **[Windows](https://docs.microsoft.com/dotnet/core/install/windows)**:
- **[Microsoft Visual C++ 2015 Redistributable Update](https://www.microsoft.com/en-us/download/details.aspx?id=53587)** (x64 za 64-bit Windows, x86 za 32-bit Windows)
- Veoma je preporučljivo da budete sigurni da su sva postojeća Windows ažurirana instalirana. Morate minimalno imati **[KB2533623](https://support.microsoft.com/en-us/help/2533623/microsoft-security-advisory-insecure-library-loading-could-allow-remot)** i **[KB2999226](https://support.microsoft.com/en-us/help/2999226/update-for-universal-c-runtime-in-windows)**, ali možda i još neko ažuriranje ako bude potrebno. Sve su već instalirane ako je Windows ažuriran do kraja. Budite sigurni da ste sve ovo uradili prije nego što instalirate Visual C++.

#### **[Linux](https://docs.microsoft.com/dotnet/core/install/linux)**:
Imena paketa zavise od Linux distribucije koju koristite, mi smo naveli najčešće varijacije. Možete ih instalirati sa nativnim paket menađerom na vašoj distribuciji (kao što je `apt` za Debian ili `yum` za CentOS).

- `libc6` (`libc`)
- `libgcc1` (`libgcc`)
- `libicu` (`icu-libs`, poslednju verziju za vašu distribuciju, npr. `libicu67`)
- `libgssapi-krb5-2` (`libkrb5-3`, `krb5-libs`)
- `libssl1.1` (`libssl`, `openssl-libs`, poslednju verziju na vašoj distribuciji, `1.1.X` or `1.0.X`)
- `libstdc++6` (`libstdc++`, u verziji `5.0` ili većoj)
- `zlib1g` (`zlib`)

Većina, ako ne i sve, bi trebalo da su već instalirane na vašem sistemi. Minimalna instalacija Debian stable zahtijeva samo `libicu63` od svih ovih.

#### **[OS X](https://docs.microsoft.com/dotnet/core/install/macos)**:
- Ništa za sada, ali trebate imati poslednju verziju OS X instaliranu, najmanje 10.13+

---

### Preuzimanje

Sada kada imate sve zahtjeve ispunjene, sledeći korak je da preuzmete **[poslednje ASF izdanje](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)**. ASF je dostupan u raznim varijantama, ali vama je potreban paket koji je isti kao vaš operativni sistem i vaša arhitektura. Npr. ako koristite `64`-bit `Win`dows, onda izaberite `ASF-win-x64` paket. Za više informacija o dostupnim varijacijama, posjetite **[kompatabilnost](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility)** sekciju. ASF je moguće pokrenuti i na OS-ovima za koje ne pravimo OS namijenjeni paket, kao što je **32-bit Windows**, za njih izaberite **[opšta podešavanja](#generic-setup)**.

![Sredstva](https://i.imgur.com/Ym2xPE5.png)

Nakon preuzimanja, ekstraktujte zip fajl u novom folderu. Mi predlažemo koršćenje **[7-zip](https://www.7-zip.org)**, ali svi standardni programi kao `unzip` na Linux/OS X-u će takođe raditi bez problema.

Ako koristite Linux/OS X, ne zaboravite da date privilegije pokretanja pomoću `chmod +x ArchiSteamFarm` u folderu gdje je program ekstraktovan, pošto privilegije nije moguće podesiti u zip fajlu. Ovo je potrebno samo jednom uraditi prije prvog pokretanja.

ASF je potrebno ekstraktovati u **svom novom direktorijumu** a ne u nekom postojećem direktorijumu koji koristite za nešto drugo - ASF će izbrisati sve fajlove koje ne prepoznaje ili mu nisu više potrebni nakon ažuriranja, što može dovesti do brisanja vaših fajlova ako ih smjestite u istom ASF folderu. Ako imate neki script ili fajl koji želite da koristite sa ASF-om, stavite ih u folder iznad.

Primjer ove strukture treba da izgleda ovako:

```text
C:\ASF (gdje stavljate vaše stvari)
    ├── ASF shortcut.lnk (opcionalno)
    ├── Config shortcut.lnk (opcionalno)
    ├── Commands.txt (opcionalno)
    ├── MyExtraScript.bat (opcionalno)
    ├── (...) (bilo koji fajl koji vi izaberete, opcionalno)
    └── Core (samo za ASF, ovdje ekstrakujete arhivu)
         ├── ArchiSteamFarm(.exe)
         ├── config
         ├── logs
         ├── plugins
         └── (...)
```

---

### Podešavanje

Sada smo na poslednjem koraku, konfiguraciji. Ovo je do sad najkomplikovaniji korak, pošto sadrži dosta novih informacija koje možda još ne poznajete, pa ćemo dati nekoliko lako-razumljivih primjera i jednostavnih objašnjenja ovdje.

Prvo i najvažnije, postoji stranica **[konfiguracija](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** koja objašnjava **sve** što je povezano sa konfiguracijom, ali sadrži veliku količinu informacija, od kojih je većinu ne morate odmah znati. Umjesto toga, naučićemo vas kako da nađete informacije koje su vam potrebne.

ASF configuration can be done in at least three ways - through our web config generator, ASF-ui or manually. Ovo je duboko objašnjena sekcija **[konfiguracije](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)**, pa se njom pozabavite ako želite detaljne informacije. We'll use web config generator as a starting point.

Pođite na našu stranicu **[web generator konfiguracije](https://justarchinet.github.io/ASF-WebConfigGenerator)** pomoću vašeg pretraživača. Ovdje morate imati JavaScript omogućen ako ste ga ručno onemogućili. Mi predlažemo Chrome ili Firefox, ali će vjerovatno raditi na svim poznatim pretraživačima.

Nakon otvaranja ove stranice, pođite na "Bot" tab. Kada ste tu, trebalo bi da vidite stranicu sličnu ovoj ispod:

![Bot tab](https://i.imgur.com/aF3k8Rg.png)

Ako je nikim slučajem vaša verzija ASF-a, koju ste preuzeli, starija od one kojom je web generator konfiguracije podešen, jednostavno izaberite vašu verziju ASF menia. Ovo se može desiti pošto generator konfiguracije može praviti konfiguracije za novije (pre-objavljene) ASF verzije koje još nisu stabilne za normalno izdanje. Vi ste preuzeli poslednju stabilnu verziju ASF-a koja je potvrđena da radi kako treba.

Počnite tako što ćete dati ime botu u predjelu označenom crvenom bojom. Ovo može biti bilo šta, kao što je nadimak, ime naloga, broj, ili bilo šta drugo. Postoji samo jedna riječ koju ne možete koristiti, a to je `ASF`, pošto je ova riječ rezervisana za fajl globalne konfiguracije. Pored toga imena botova ne mogu početi sa tačkom (ASF namjerno ignoriše ovakve fajlove). Mi takođe predlažemo da ne odvajate između riječi, umjesto odvajanja koristite `_`.

Kada ste odlučili koje ime ćete dati botu, stisnite `Omogućeno`, ovo kazuje ASF-u da li će vaš bot biti pokrenut ili ne nakon pokrenanja programa.

Sada možete odlučuti oko dvije stvari:
- Možete upisati vaše kredencijale za prijavu u `SteamLogin` polju i vašu lozinku u `SteamPassword` polju
- Ili ih možete ostaviti prazne

Ako unesete kredencijale to će omogućiti ASF-u da automatski koristi iste tokom pokretanju, i vi nećete morati da ih unosite svaki put kada želite da koristite ASF. Možete ih takođe izostaviti, a u tom slučaju oni neće biti sačuvani, i ASF će tražiti da ih unesete tokom svakog pokretanja.

ASF-u su potrebni kredencijali za prijavu zato što on posjeduje svoju inplementaciju Steam klienta i zato su mu potrebni iste stvari da bi se prijavio kao što vam i Steam klient traži kada se prijavljujete. Vaši kredencijali za prijavu su sačuvani samo na vašem PC-u u ASF `config` direktorijumu, naš web generator konfiguracije je klient-baziran što znači da je kod pokrenut lokalno u vašem pretraživaču, a detalji koje unesete ne šalju se nigdje van vašeg PC-a, pa se ne morate brinuti o curenju podataka negdje drugo. Ipak, ako zbog nekog razloga ne želite da unesete vaše podatke ovdje, mi to razumijemo, pa to možete uraditi u ručno napravljenim fajlovima, ili ih skroz maći pa ih svaki put unositi u ASF komandnoj liniji. Više o pitanjima sigurnosti možete pronaći u sekciji **[konfiguracija](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)**.

Takođe možete odlučiti da ostavite samo jedno polje prazno, kao što je `SteamPassword`, ASF će u tom slučaju automatski koristiti ime vašeg naloga, ali će tražiti vašu lozinku (slično kao što to radi Steam klient). Ako koristite roditeljska podešavanja na Steam-u da bi otključali vaš nalog, morate onda popuniti i polje `SteamParentalCode`.

Nakon vaših odluka i opcionih detalja, vaša web stranica bi trebala da izgleda slično ovoj ispod:

![Bot tab 2](https://i.imgur.com/yf54Ouc.png)

Sada možete pritisnuti "download" dugme i naš web generator konfiguracije će napraviti novi `json` fajl baziran na imenu vašeg bota. Sačuvajte taj fajl u `config` direktorijumu koji je lociran u folderu gdje ste ekstrakovali vaš zip fajl u prethodnom koraku.

Vaš `config` direktorijum bi trebao da izgleda slično ovom:

![Struktura 2](https://i.imgur.com/crWdjcp.png)

Čestitamo! Završili sve veoma jednostavnu konfiguraciju ASF bota. Ovo ćemo brzo dalje proširiti, ali za sad je to sve što vam je potrebno.

---

### Pokretanje ASF-a

Sada ste spremni da pokrenete program prvi put. Jednostavno dvokliknite `ArchiSteamFarm` fajl u ASF direktorijumu.

Nakon toga, ako ste instalirali sve potrebno u prvom koraku, ASF bi trebao da se uspješno pokrene, zapamtite da će vaš prvi bot (ako niste zaboravili da stavite napravljenu konfiguraciju u `config` direktorijumu), pokušati da se prijavi:

![ASF](https://i.imgur.com/u5hrSFz.png)

Ako ste ispunili `SteamLogin` i `SteamPassword` polje koje ASF koristi, tražiće vam se SteamGuard token (e-mail, 2FA ili ništa, u zavisnosti od vaših Steam podešavanja). Ako niste dali Steam kredencijale, onda će vam se ovdje tražiti isti.

Now would be a good time to review our **[privacy policy](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Statistics#current-privacy-policy)** section if you're concerned about stuff ASF is programmed to do, like joining a certain Steam group on launch.

Nakon što je pošao proces prijave, ako su vaši kredencijali tačni, bićete uspješno prijavljeni, i ASF će početi da idluje koristeći opšta podešavanja ako ih niste promijenili:

![ASF 2](https://i.imgur.com/Cb7DBl4.png)

Ovo pokazuje da ASF uspješno radi svoj posao na vašem nalogu, pa ga možete umanjiti i raditi nešto drugo. Nakon određenog vremena (u zavisnosti od **[performansi](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)**), polako ćete dobijati Steam kartice. Naravno, da bi se to desilo morate imati validne igrice za idlovanje, koje kažu "možete dobiti još X kartica igrajući ovu igricu" na vašoj **[bedž stranici](https://steamcommunity.com/my/badges)** - ako nema igrica za idlovanje, onda ASF neće ništa raditi kao što je kazano u našem **[FAQ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ#what-is-asf)**.

Ovo je kraj našeg jednostavnog uputstva za podešavanje. Možete odlučiti da li želite da dalje konfigurišete ASF, ili da ga ostavite da radi pomoću opštih podešavanja. Proći ćemo kroz još nekoliko osnovnih stavki, a onda ćemo vam preputsiti da otkrijete ostatak wiki-e.

---

### Šira konfiguracija

#### Idlovanje nekoliko naloga odjednom

ASF ima mogućnost da idluje više od jednog naloga odjednom, što je njegova primarna funkcija. Možete dodati više od jednog naloga u ASF pomoću generisanja više od jedne bot konfiguracije, na isti način na koji ste generisali prvu konfiguraciju prije nekoliko minuta. Morate biti sigururni o ove dvije stvari:

- Različita imena botova, ako je već ime prvog bota "GlavniNalog", ne možete napraviti drugog bota sa istim imenom.
- Pravilni detalji vašeg naloga, `SteamLogin`,<0 `SteamPassword` i `SteamParentalCode` (ako koristite Steam roditeljska podešavanja).

Drugim riječima, jednostavno ponovo idite na konfiguraciju i uradite istu stvar, za vaš drugi ili treći nalog. Zapamtite da morate koristiti različita imena za svakog bota.

---

#### Mijenjanje podešavanja

Možete promijeniti postojeća podešavanja na isti način - konfigurišući novi config fajl. Ako još niste zatvorili web generator konfiguracije, pritisnite "prikaži napredna podešavanja" da bi ste vidjeli šta je tu za vas da otkrijete. U ovod vodiču ćemo promijeniti podešavanje `CustomGamePlayedWhileFarming`, što omogućava da podesite ime koje će biti prikazano kada ASF idluje, umjesto prikazivanja stvarnog imena igrice.

Pa uradimo to, ako pokrenete ASF i počnete idlovati, na vašem Steam nalogu ćete vidjeti da ste u igrici i ime igrice će biti prikazano ovako:

![Steam](https://i.imgur.com/1VCDrGC.png)

Promijenimo to. Prikažite napredna podešavanja u vašem web generatoru konfiguracije i pronađite polje `CustomGamePlayedWhileFarming`. Kada to uradite, unesite tekst koji želite da bude prikazan, kao što je "Idlujem kartice":

![Bot tab 3](https://i.imgur.com/gHqdEqb.png)

Sada preuzmite novi konfiguracioni fajl na isti način, pa ga **zamijenite** sa vašim starim. Možete takođe izbrisati stari konfiguracioni fajl i dodati novi na njegovom mjesu.

Nakon što to uradite, pokrenite ASF ponovo, vidjećete da ASF sada prikazuje vaš uneseni tekst na prethodnom mjestu:

![Steam 2](https://i.imgur.com/vZg0G8P.png)

This confirms that you've successfully edited your config. In exactly the same way you can change global ASF properties, by switching from bot tab to "ASF" tab, downloading generated `ASF.json` config file and putting it in your `config` directory.

Editing your ASF configs can be done much easier by using our ASF-ui frontend, which will be explained further below.

---

#### Korišćenje ASF-ui

ASF je aplikacija u konsoli i ne sadrži grafički korisnički interfejs. Ipak, mi aktivno radimo na **[ASF-ui](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#asf-ui)** interfejsu za naš IPC, koji je veoma lak za korišćenje i pristup raznim ASF mogućnostima.

In order to use ASF-ui, you need to have `IPC` enabled, which is the default option starting with ASF V5.1.0.0. Once you launch ASF, you should be able to confirm that it properly started the IPC interface automatically:

![IPC](https://i.imgur.com/ZmkO8pk.png)

You can access ASF's IPC interface under **[this](http://localhost:1242)** link, as long as ASF is running, from the same machine. You can use ASF-ui for various purposes, e.g. editing the config files in-place or sending **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. Slobodno razgledajte okolo da bi pronašli razne ASF-ui funkcije.

![ASF-ui](https://raw.githubusercontent.com/JustArchiNET/ASF-ui/main/.github/previews/bots.png)

Please note that some features, such as sending commands, require a properly set `SteamOwnerID` global config property. Now that you have ASF-ui up and running, why not give it a try and set it from the frontend itself? You'll need to input unique Steam identificator in 64-bit form of your account. You can look it up in various different ways, for example through **[STEAMID I/O](https://steamid.io)** or **[SteamRep](https://steamrep.com)**. The number you're looking for should be similar to `76561198006963719`, which is my account's ID.

---

### Zaključak

Uspješno ste podesili ASF da koristi vaš Steam nalog i uspješno ste ga uredili onako kako vam odgovara. If you followed our entire guide, then you also managed to tweak ASF through our ASF-ui interface and found out that ASF actually has a GUI of some sort. Now is a good time to read our entire **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** section in order to learn what all those different settings you've seen actually do, and what ASF has to offer. If you've stumbled upon some issue or you have some generic question, read our **[FAQ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ)** instead which should cover all, or at least a vast majority of questions that you may have. Ako želite da naučite sve što postoji u ASF-u i kako vam to može pomoći, posjetite ostatak **[naše wiki-e](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Home)**. If you found out our program to be useful for you and you're feeling generous, you can also consider donating to our project. In any case, have fun!

---

## Opšta podešavanja

Ova podešavanja su za napredne korisnike koji žele da podese ASF za pokretanje na **[opštoj](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#generic)** varijanti. Ovo nije preporučeno za ljude koji mogu da koriste **[OS-specifično podešavanje](#os-specific-setup)**.

You want to use `generic` variant mainly in those situations (but of course you can use it regardless):
- Kada koristite OS za koji nema OS-specifičan paket (kao što je 32-bitni Windows)
- Kada već imate .NET Core Runtime/SDK, ili želite da ga instalirate i koristite
- Kada želite da smanjite veličinu ASF strukture tako što ćete podesite runtime zahtjeve ručno
- When you want to use a custom **[plugin](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins)** which requires a `generic` setup of ASF to run properly (due to missing native dependencies)

Ipak, zapamtite da ćete u tom slučaju vi biti odgovorni za .NET Core runtime. Ovo znači da ako je .NET Core SDK (runtime) nedostupan, zastareo ili pokvaren, ASF neće raditi. Zbog ovog ne preporučujemo ovaj metod za obične korisnike, pošto morate biti sigurni da se vaš .NET Core SDK (runtime) poklapa sa ASF zahtjevima i da može da pokrene ASF, dok u normalnoj verziji **mi** provjeravamo da li .NET Core runtime koji je upakovan u ASF može da ga pokrene.

For `generic` package, you can follow entire OS-specific guide above, with two small changes. U dodatku sa instaliranjem .NET Core prerequisites, isto trebate da instalirate .NET Core SDK, i umjesto što imate OS-specifični `ArchiSteamFarm(.exe)` eksekjucioni fajl, sada ćete imati opšti `ArchiSteamFarm.dll` binarni fajl. Sve ostalo je isto.

Sa dodatnim koracima:
- Instalirajte **[.NET Core prerequisites](https://docs.microsoft.com/dotnet/core/install/dependencies?tabs=netcore31)**.
- Instalirajte **[.NET Core SDK](https://www.microsoft.com/net/download)** (ili bar runtime) za vaš OS. Vjerovatno ćete željeti da koristite automatsku instalaciju. Pogledajte **[runtime zahtjeve](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)** ako niste sigurni koju verziju da instalirate.
- Download **[latest ASF release](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** in `generic` variant.
- Ekstraktujte arhivu u novoj lokaciji (i `chmod +x ArchiSteamFarm.sh` ako ste na Linux-u/OS X-u),.
- **[Konfigurišete ASF](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)**.
- Pokrenite ASF koristeći pomoćni skript ili ručnom ekekjucijom `dotnet /ruta/do/ArchiSteamFarm.dll` iz vaše omiljene ljuske.

Helper scripts (such as `ArchiSteamFarm.cmd` for Windows and `ArchiSteamFarm.sh` for Linux/OS X) are located next to `ArchiSteamFarm.dll` binary - those are included in `generic` variant only. Njih možete koristiti ako ne želite da ručno ekekjutujete `dotnet` komande. Pomoćni skriptovi naravno neće raditi ako niste instalirali .NET Core SDK i ako nemate `dotnet` dostupan u vašem `PATH`-u. Pomoćni skriptovi su skroz opcionalni, uvijek možete ručno koristiti `dotnet /ruta/do/ArchiSteamFarm.dll`.