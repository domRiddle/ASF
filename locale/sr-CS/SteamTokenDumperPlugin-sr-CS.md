# SteamTokenDumperPlugin

`SteamTokenDumperPlugin` je oficijalni ASF **[dodatak](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins)** koji je dostupan u ASF-u od V4.2.2.2, napravljen od strane naših developera, a koji vam omogućava da doprinesete **[SteamDB](https://steamdb.info)** projektu tako što ćete dijeliti tokene, tokene aplikacija i depot ključeve kojima vaš Steam nalog ima pristup. Više informacija o prikupljenim podacima i o tome zašto SteamDB ima potrebu za njim možete pronaći na SteamDB **[TokenDumper](https://steamdb.info/tokendumper)** stranici. Poslati podaci ne sadrže potencijalno osjetljive informacije, i nema sigurnosnih i rizika privatnosti, kao što je navedeno u opisu iznad.

---

## Omogućavanje dodatka

ASF ima ugrađen `SteamTokenDumperPlugin` sa izdanjima, ali je dodatak onemogućen sve dok ga vi ne uključite. Možete ga uključiti podešavanjem `SteamTokenDumperPluginEnabled` ASF kofiguracije na `true`, u JSON sintaksi:

```json
{
  "SteamTokenDumperPluginEnabled": true
}
```

Tokom pokretanja vašeg ASF programa, dodatak će vam kazati da li je pravilno omogućen tokom standardnig ASF izvještaja. Takođe možete omogućiti ovaj dodatak preko web-baziranog generatora.

---

## Tehnički detalji

Nakon omogućavanja, dodatak će koristiti botove koje su pokrenuti u ASF-u za prikupljanje podataka u formi paket tokena, aplikacionih tokena i depot ključeva kojima vaš bot ima pristup. Modul za prikupljanje podataka sadrži pasivnu i aktivnu rutinu koja bi trebala da smanji dodatne smetnje izazvane prikupljanjem podataka.

Da bi se ispunila planirana funkcionalnost, u sklopu sa prikupljanjem podataka objašnjenih iznad, rutina slanja određuje koji podaci nedostaju na SteamDB-u i šalje ih periodično. Ova rutina će se upaliti najkasnije `1` sat nakoš što upalite ASF, i ponavljaće se svakih `24` sata. Ovaj dodatak radi što najbolje može da bi odredio količinu podataka koju treba poslati, pa je zbog toga moguće da neke prikupljene podatke, koje dodatak prikupi, označi nepotrebnim, i zbog toga ih ne pošalje (kao što je ažuriranje aplikacija koje ne mijenja pristupni token).

Dodatak koristi trajnu keš bazu podataka koja se nalazi u `config/SteamTokenDumper.cache` lokaciji, koja služi sličnu funkciju kao `config/ASF.db` ASF-u. Fajl se koristi radi bilježenja prikupljenih podataka i poslatih podataka da bi smanjila količinu posla koji treba da uradi tokom raznim ASF pokretanja. Brisanje ovog fajla daje posledicu da se od ponovo napravi u sledećem pokretanju, a to treba izbjegavati, ako je moguće.

---

## Podaci

ASF uključuje `steamID` doprinosioca tokom slanja, koji je određen kao `SteamOwnerID` koji ste postavili u ASF, a ako niste koristiće se Steam ID bota koji sadrži najviše licenci. Navedeni dopinosioc će možda dobiti neke dodatne koristi od strane SteamDB-a za kontinuiranu pomoć (npr. donator rank na njihovom sajtu), ali to zavisi od strane SteamDB kompletno.

U svakom slučaju, SteamDB osoblje vam se unaprijed zahvaljuje na vašoj pomoći. Poslati podaci omogućavaju SteamDB-u da radi, posebno da prati promjene oko paketa, aplikacija i depotova, što nebi milo moguće bez vaše pomoći.