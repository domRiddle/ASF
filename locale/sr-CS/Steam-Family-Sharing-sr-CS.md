# Steam porodično dijeljenje

ASF supports Steam Family Sharing - in order to understand how ASF works with that, you should firstly read how **[Steam Family Sharing works](https://store.steampowered.com/promotion/familysharing)**, which is available on Steam store.

---

## ASF

Podrška za Steam porodično dijeljenje je transparentna u ASF-u, što znači da se ne prave novi botovi/procesi u konfiguraciju - radi odmah kao dodatna funkcionalnost.

ASF prepoznaje ako neko igra i neće ga izbaciti iz igrice ako pokrenete igricu. ASF će biti kao primarni profil koji ima ključ, pa ako je taj ključ u vašem posjedovanju, ili u posjedovanju nekog sa kim je podijeljen, ASF neće farmati, odnosno, čekaće da se ključ oslobodi.

U dodatku sa ovim iznad, ASF će pristupiti vašim **[podešavanjima dijeljenja](https://store.steampowered.com/account/managedevices)**, iz kojih će moći da uzme do 5 `steamID`-ova dozvoljenih da koriste kolekciju igrica. Ovim korisnicima koji imaju dozvoli da koriste `FamilySharing` biće dostupne **[komande](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**, posebno komanda koja im omogućava `pause~` naredbu bot profilu koji dijeli igrice sa njima, što im omogućava da pauziraju automatsko farmanje kartica da bi mogli da pokrenu igricu koja se dijeli sa njima.

Povezivanje obje funkcionalnosti dozvoljava vašim prijateljima da `pause~` vaše farmanje kartica, pokrenu igricu, igraju koliko oće, pa kad završe proces će se automatski nastaviti u ASF. Naravno, slanje `pause~` nije potrebno ako ASF ne farma nešto trenutno, zato što vaši prijatelji mogu pokrenuti igricu odmah, i oni neće biti izbačeni iz igrice.

---

## Limitacije

Steam network često šalje netačne poruke za status korisnika ASF-u, što može prouzrokovati da ASF povjeruje da je moguće da nastavi sa radom, a za rezultat toga da vaš prijatelj bude izbačen. Ovo je isti problem kao onaj već pomenut u **[ovoj FAQ odrednici](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ#asf-is-kicking-my-steam-client-session-while-im-playing--this-account-is-logged-on-another-pc)**. Mi predlažemo istu rješenje, tj. davanje vašem prijatelju `Operator` dozvolu (ili veću) i obavijestiti ga da koristi `pause` i `resume` komande.