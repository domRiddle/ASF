# Dvofaktorska autentikacija

Pre nekog vremena Valve je uveo sistem pod nazivom "Escrow" koji zahtijeva autentikaciju za razne nalogom povezane postupke. O ovome možete pročitati više **[ovdje](https://support.steampowered.com/kb_article.php?ref=1284-WTKB-4729)** i **[odvje](https://support.steampowered.com/kb_article.php?ref=8078-TPHC-6195)**. Važno je da prvo razumijete sistem 2FA[dvofaktorske autentikacije] prije nego što razumijete logiku iza ASF-ove 2FA.

Možete primijetiti da su sve razmjene zadržane na 15 dana, što nije veliki problem za ASF, ali je ipak nezgodno, pogotovo za one koji žele punu automatizaciju. Srećom, ASF sadrži rešenje za ovaj problem, pod nazivom ASF 2FA.

* * *

# Logika ASF-a

Bez obzira da li koristite ASF 2FA (objašnjenog dolje) ili ne, ASF sadrži sopstvenu logiku i svjestan je da li koristite 2FA zaštitu naloga ili ne. Pitaće vas za potrebne informacije kada su potrebne (npr. kada se prijavljujete). Ako koristite ASF 2FA, program će biti u mogućnosti da preskoči te zahtjeve i automatski napravi potrebne tokene, štedeći vas gnavaže i omogućavajući eksta funkcijonalnosti (opisane dolje).

* * *

# ASF 2FA

ASF 2FA je ugrađena modula odgovorna za pružanje 2FA mogućnosti ASF-u, kao što je generisanje token i prihvatanje potvrda. Ono duplira vašu postojeću autentikaciju, tako da možete koristiti vašu trenutnu autentikaciju i ASF 2FA u isto vrijeme.

Možete provjeriti da li vaš bot nalog već koristi ASF 2FA izvršavanjem `2fa`**[komandi](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. Osim ako ste već unijeli autenkaciju u ASF 2FA, sve `2fa` komande su neoperativne, što znači da vaš nalog ne koristi ASF 2FA, i zbog toga nema mogućnost izvršavanja naprednih ASF mogućnosti koje zahtijevaju ovaj modul da bi bile operativne.

* * *

## Unos

Da bi koristili ASF 2FA, morate već imati postojeći i operacioni autentikator koji je podržan od strane ASF-a. ASF trenutno podržava nekoliko različitih oficijalnih i neoficijalnih izvora 2FA - Android, iOS, SteamDesktopAuthenticator i WinAuth. Ako već nemate autentikator, morate prvo izabrati i podesiti jedan od ovih. Ako ne znate koji da izaberete, predlažmo vam WinAuth, ali bilo koji od ovih će raditi dobro ako pratite instrukcije.

Sva naredna uputstva zahtijevaju da već imate **radni i operativni** autentikator koji možete koristi sa aplikacijom. ASF 2FA neće raditi propisno ako unesete nepravilne podatke, pa zbog toga budite sigurni da vaš autentikator radi prije nego što pokušate da ga unesete. To podrazumijeva testiranje i verifikaciju pravilnog rada sledećih funkcija:

- možete generisati tokene koje prihvata Steam network,
- možete primati potvrde, i one će stizati na vašem mobilnom autentikatoru,
- možete potvrditi te potvrde, i one će biti pravilno prepoznate od strane Steam-a kao potvrđene/nepotvrđene.

Potvrdite da vaš autentikator radi provjeravajući da li postupci gore navedeni rade - ako ne rade, onda neće raditi ni u ASF-u takođe, samo ćete trošiti vrijeme i zadavati sebi dodatnu brigu.

* * *

### Android telefoni

Generalno, za ubacanje autentikatora sa vašeg Android telefona morate imati **[root](https://en.wikipedia.org/wiki/Rooting_(Android_OS))** privilegije. Root je različit od telefona do telefona, pa vam ne mogu reći kako da rootujete vaš uređaj. Posjetite **[XDA forum](https://www.xda-developers.com/root)** radi odličnih uputstava o načinu kako to da uradite, kao i o uobičajenim informacijama o rootovanju. Ako ne možete da pronađete vaš uređaj ili uputstvo koje vam treba, pokušajte da tražite na Google-u.

Oficijalno, nije moguće da pristupite zaštićenim Steam fajlovima bez root-a. Jedini oficijalni bez-rootni način da dođete to Steam fajlova jeste da kreirate nezaštićenu `/data` kopiju na neki način i da ručno tražite potrebne fajlove u njoj na PC-u, ali pošto ta funkcija zavisi o vašeg proizvođaća uređaja i **nije** standardno na Android-u, nećemo o tome govoriti. Ako se zadesi da imate tu funkcijonalnost, možete je koristiti, ali najčešće korisnici to nemaju.

Neoficijalno, moguće je izvući te fajlove bez root privilegija, ako instalirate Steam aplikaciju verzije 2.1(ili ranuju), postavite mobilnu autentikaciju i napravite kopiju vaše aplikacije (zajedno sa `/data` fajlovima koji su nam potrebni) pomoću `adb backup`. Ali pošto je ove ozbiljan sigurnosni problem i skroz nepodržan način da se izvuku fajlovi, ne možemo govoriti dalje o tome, Valve je onemogućijo ovo s razlogom, a mi ga pominjemo jedino kao mogućnosti.

Pretpostavljajući da ste već uspješno root-ovali vaš uređaj, trebate nakon toga preuzeti root pretraživač koji je dosupan Play prodavnici, kao što je **[ovaj](https://play.google.com/store/apps/details?id=com.jrummy.root.browserfree)** (ili bilo koji drugi u zavisnosti od vašeg izbora). Takođe možete doći to zaštićenih fajlova pomoću ADB (Android Debug Bridge) ili drugog vama dostupnog metoda, mi ćemo objasniti pomoću pretraživača, pošto je to najdostupniji način za koristike.

Kad otvorite root pretraživač, idite u folder `/data/data`. Još jednom napominjemo da je direktorijum `/data/data` zaštićen i da mu ne možete pristupiti bez root privilegija. Kada ste u ovom direktorijumu, pronađite folder `com.valvesoftware.android.steam.community` i kopirajte ga na vašu `/sdcard`, koja je na vašoj internoj memoriji. Nakon toga, imate mogućnost da konektujete uređaj na vaš PC i kopirate folder za vaše interne memorije kao i obično. Ako je nekim slučajom folder nevidljiv i pored toga što ste ga kopirali na pravo mjesto, pokušajte restartovati uređaj prvo.

Sada možete odlučiti da li želite da ubacite vaš autentikator prvo u WinAuth, pa u ASF, ili direktno u ASF. Prvo opcija je lakša i omogućava vam da duplirate autentikator na vašem PC, što vam omogućava da prihvatate potvrde i generišete tokene sa tri različita mjesta - vašeg telefona, PC-a i ASF-a. Ako želite da to uradite, otvorite WinAuth, dodajte novi Steam autentikator i izaberite opciju "importing from Android(unesite sa Android-a)", zatim pratite instrukcije i izaberite fajlove koje ste preuzeli ranije. Kada završite, možete da dodate ovaj autentikator iz WinAuth-a u ASF, a ovo je objašnjeno ispod u WinAuth odjeljku.

Ako ne želite ili nemate potrebu da ovo radite pomoću WinAuth-a, ona možete kopirati fajl `files/Steamguard-SteamID` iz zaštićenog direktorijuma, gdje je 64-bitni `SteamID` Steam identifikator naloga kojeg želite da dodate (ako je samo jedan, to je zbog toga što imate samo jedan profil, i to je fajl koji vam je potreban). Treba da premjestite ovaj fajl in ASF-ov `config` direktorijum. Kada to uradite, promijenite ime fajla sa `BotName.maFile`, gdje je `BotName` isto kao ime kojim ste nazvali bota tokom konfiguracije i kojem dodajete ASF 2FA. Nakon ovog koraka, pokrenite ASF - on će zapaziti `.maFile` i iskoristiti ga.

```text
[*] INFO: PreuzimanjeAutentikatora() <1> Mijenjanje .maFile-a u ASF format...
[*] INFO: PreuzimanjeAutentikatora() <1> Uspješno završeno preuzimanje mobilnog autentikatora!
```

To je sve, pretpostavljajući da se izabrali tačan važeći fajl, sve bi trebalo da bude u redu, a to možete provjeriti koristeći `2fa` komande. Ako nešto pogriješite, uvijek možete izbrisati `Bot.db` i početi ponovo ako je potrebno.

* * *

### iOS

Na iOS-u možete koristiti **[ios-steamguard-extractor](https://github.com/CaitSith2/ios-steamguard-extractor)**. Ovo je moguće zahvaljujući činjenici da možete praviti nešifrovanu rezervnu kopiju, prebaciti je na PC i koristiti alaktu da bi ekstraktovali Steam podatke koje bi inače bilo nemoguće dobiti (ako vaš uređaj nije jailbreak-ovan, zato što je iOS enkriptovan).

Otiđite na **[poslednje izdanje](https://github.com/CaitSith2/ios-steamguard-extractor/releases/latest)** da bi preuzeli program. Once you extract the data you can put it e.g. in WinAuth, then from WinAuth to ASF (although you can also simply copy generated json starting from `{` ending on `}` into `BotName.maFile` and proceed like usual). If you ask me, I strongly recommend to import to WinAuth first, then making sure that both generating tokens as well as accepting confirmations work properly, so you can be sure that everything is alright. If your credentials are invalid, ASF 2FA will not work properly, so it's much better to make ASF import step your last one.

For questions/issues, please visit **[issues](https://github.com/CaitSith2/ios-steamguard-extractor/issues)**.

*Keep in mind that above tool is unofficial, you're using it at your own risk. We do not offer technical support if it doesn't work properly - we got a few signals that it's exporting invalid 2FA credentials - verify that confirmations work in authenticator like WinAuth prior to importing that data to ASF!*

* * *

### SteamDesktopAuthenticator

If you have your authenticator running in SDA already, you should notice that there is `steamID.maFile` file available in `maFiles` folder. Copy that file to `config` directory of ASF. Make sure that `.maFile` is in unencrypted form, as ASF can't decrypt SDA files - unencrypted file content should start with `{` character.

You should now rename `steamID.maFile` to `BotName.maFile` in ASF config directory, where `BotName` is the name of your bot you're adding ASF 2FA to. Alternatively you can leave it as it is, ASF will then pick it automatically after logging in. Helping ASF makes it possible to use ASF 2FA before logging in, if you won't help ASF, then the file can be picked only after ASF successfully logs in (as ASF doesn't know `steamID` of your account before in fact logging in).

If you did everything correctly, launch ASF, and you should notice:

```text
[*] INFO: ImportAuthenticator() <1> Converting .maFile into ASF format...
[*] INFO: ImportAuthenticator() <1> Successfully finished importing mobile authenticator!
```

From now on, your ASF 2FA should be operational for this account.

* * *

### WinAuth

Firstly create new empty `BotName.maFile` in ASF config directory, where `BotName` is the name of your bot you're adding ASF 2FA to. Remember that it should be `BotName.maFile` and NOT `BotName.maFile.txt`, Windows likes to hide known extensions by default. If you provide incorrect name, it won't be picked by ASF.

Now launch WinAuth as usual. Right click on Steam icon and select "Show SteamGuard and Recovery Code". Then check "Allow copy". You should notice familiar to you JSON structure on the bottom of the window, starting with `{`. Copy whole text into a `BotName.maFile` file created by you in previous step.

If you did everything correctly, launch ASF, and you should notice:

```text
[*] INFO: ImportAuthenticator() <1> Converting .maFile into ASF format...
[*] INFO: ImportAuthenticator() <1> Successfully finished importing mobile authenticator!
```

From now on, your ASF 2FA should be operational for this account.

* * *

## Završeno

From this moment, all `2fa` commands will work as they'd be called on your classic 2FA device. You can use both ASF 2FA and your authenticator of choice (Android, iOS, SDA or WinAuth) to generate tokens and accept confirmations.

If you have authenticator on your phone, you can optionally remove SteamDesktopAuthenticator and/or WinAuth, as we won't need it anymore. However, I suggest to keep it just in case, not to mention that it's more handy than normal steam authenticator. Just keep in mind that ASF 2FA is **NOT** a general purpose authenticator and it should **never** be the only one you use, since it doesn't even include all data that authenticator should have. It's not possible to convert ASF 2FA back to original authenticator, therefore always make sure that you have general-purpose authenticator in other place, such as in WinAuth/SDA, or on your phone.

* * *

## Najčešće postavljana pitanja

### How is ASF making use of 2FA module?

If ASF 2FA is available, ASF will use it for automatic confirmation of trades that are being sent/accepted by ASF. It will also be capable of automatically generating 2FA tokens on as-needed basis, for example in order to log in. In addition to that, having ASF 2FA also enables `2fa` commands for you to use. That should be all for now, if I didn't forget about anything - basically ASF uses 2FA module on as-needed basis.

* * *

### Šta ako mi je potreban 2FA token?

You will need 2FA token to access 2FA-protected account, that includes every account with ASF 2FA as well. You should generate tokens in authenticator that you used for import, but you can also generate temporary tokens through `2fa` command sent via the chat to given bot. You can also use `2fa <BotNames>` command to generate temporary token for given bot instances. This should be enough for you to access bot accounts through e.g. browser, but as noted above - you should use your friendly authenticator (Android, iOS, SDA or WinAuth) instead.

* * *

### Da li mogu koristiti moj originalni autentikator nakog što je premjestim u ASF 2FA?

Da, originalni autentikator ostaje funkcionalan i vi ga možete koristiti zajedno sa ASF 2FA. To je cijela svrha ovog procesa - mi kopiramo vaše autentikatorske kredencijale u ASF, da bi ASF mogao da ih koristi i da prihvata određene potvrde u vaše ime.

* * *

### Gdje je ASF mobilna autentikacija sačuvana?

ASF mobilni autentikator je sačuvan u `BotName.db` fajlu u config direktorijumu, zajedno sa ostalim značajnim podacima povezanim sa vašim nalogom. Ako želite da uklonite ASF 2FA, pročitajte ovo ispod.

* * *

### Kako da uklonite ASF 2FA?

Jednostavno isključite ASF i uklonite `BotName.db` vašeg bota koji posjeduje ASF 2FA a kome želite da ga uklonite. Ova opcija će ukloniti asocirani 2FA od ASF-a, ali NEĆE razdvojiti vaš autentikator. If you instead want to delink your authenticator, apart from removing it from ASF (firstly), you should delink it in authenticator of your choice (Android, iOS, SDA or WinAuth), or - if you can't for some reason, use revocation code that you received during linking that authenticator, on the Steam website. It's not possible to unlink your authenticator through ASF, this is what general-purpose authenticator that you already have should be used for.

* * *

### I linked authenticator in SDA/WinAuth, then imported to ASF. Can I now unlink it and link it again on my phone?

**No**. ASF **imports** your authenticator data in order to use it. If you delink your authenticator then you'll also cause ASF 2FA to stop functioning, regardless if you remove it firstly like stated in above question or not. If you want to use your authenticator on both your phone and ASF (plus optionally in SDA/WinAuth), then you'll need to **import** your authenticator from your phone, and not create new one in SDA/WinAuth. You can have only **one** linked authenticator, that's why ASF **imports** that authenticator and its data in order to use it as ASF 2FA - it's **the same** authenticator, just existing in two places. If you decide to delink your mobile authenticator credentials - regardless in which way, ASF 2FA will stop working, as previously copied mobile authenticator credentials will no longer be valid. In order to use ASF 2FA together with authenticator on your phone, you must import it from Android/iOS, which is described above.

* * *

### Is using ASF 2FA better than WinAuth/SDA/Other authenticator set to accept all confirmations?

**Yes**, in several ways. First and most important one - using ASF 2FA **significantly** increases your security, as ASF 2FA module ensures that ASF will only accept automatically its own confirmations, so even if attacker does request a trade that is harmful, ASF 2FA will **not** accept such trade, as it was not generated by ASF. In addition to security part, using ASF 2FA also brings performance/optimization benefits, as ASF 2FA fetches and accepts confirmations immediately after they're generated, and only then, as opposed to inefficient polling for confirmations each X minutes done e.g. by SDA or WinAuth. In short, there is no reason to use third-party authenticator over ASF 2FA, if you plan on automating confirmations generated by ASF - that's exactly what ASF 2FA is for, and using it does not conflict with you confirming everything else in authenticator of your choice. We strongly recommend to use ASF 2FA for entire ASF activity - this is much more secure than any other solution.

* * *

## Napredno

If you're advanced user, you can also generate maFile manually. This can be used in case you'd want to import authenticator from other sources than the ones we've described above. It should have a **[valid JSON structure](https://jsonlint.com)** of:

```json
{
  "shared_secret": "STRING",
  "identity_secret": "STRING"
}
```

Standard authenticator data has more fields - they're entirely ignored by ASF during import, as they're not needed. You don't have to remove them - ASF only requires valid JSON with 2 mandatory fields described above, and will ignore additional fields (if any). Of course, you need to replace `STRING` placeholder in the example above with valid values for your account.