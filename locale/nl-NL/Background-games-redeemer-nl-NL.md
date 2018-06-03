# Productcodes-activator op de achtergrond

Productcodes-activator op de achtergrond is een speciaal ingebouwde ASF-functie waarmee je een serie Steam-productcodes (samen met hun namen) kunt importeren die op de achtergrond wordt geactiveerd. Dit is vooral handig als je een heleboel codes moet activeren, waarvan het zeker is dat je de `Aanvraaglimiet` **[status](https://github.com/JustArchi/ArchiSteamFarm/wiki/FAQ#what-is-the-meaning-of-status-when-redeeming-a-key)** zal krijgen voordat je klaar bent met het activeren.

Productcodes-activator op de achtergrond is gemaakt dat het werkt voor één bot. Dit houdt in dat het geen gebruik maakt van de `RedeemingPreferences`. Deze functie kan worden gebruikt samen met (of in plaats van) de `redeem` **[commando](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)**, indien nodig.

* * *

## Importeren

Het importeren kan worden gedaan op twee manieren - door gebruik te maken van een bestand of via IPC.

### Bestand

ASF is in staat in de `config` map een file met de naam `BotName.keys` te herkennen waarvan `BotNaam` de naam van je bot is. De data op het bestand moet bestaan uit een vaste structuur, bestaande uit de naam van het spel en de productcode, gescheiden door een tabteken en eindigend met een nieuwe regel. Als meerdere tabs worden gebruikt, wordt de eerste invoer beschouwd als de naam van het spel en de laatste invoer als de productcode. Alles daar tussenin wordt genegeerd. Bijvoorbeeld:

    POSTAL 2    ABCDE-EFGHJ-IJKLM
    Domino Craft VR 12345-67890-ZXCVB
    A Week of Circus Terror POIUY-KJHGD-QWERT
    Terraria    DitWordtGenegeerd  DitWordtOokGenegeerd    ZXCVB-ASDFG-QWERT
    

ASF zal het bestand importeren tijdens het opstarten van de bot of later tijdens de uitvoering. Na het succesvol verwerken van je bestand en eventuele weglating van ongeldige vermeldingen, worden alle correct gedetecteerde spellen toegevoegd aan de achtergrondwachtrij en wordt het `BotNaam.keys` bestand verwijderd uit de `config` map.

### IPC

Naast het gebruik van het hierboven besproken bestand, biedt ASF ook een `GamesToRedeemInBackground` **[API-eindpunt](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC#post-apigamestoredeeminbackgroundbotname)** die kan worden uitgevoerd door een IPC-tool, inclusief onze IPC GUI. Het gebruik van de IPC kan efficiënter zijn, omdat u zelf de juiste verwerking kunt uitvoeren, zoals het gebruik van een aangepast scheidingsteken in plaats van het vereiste tabteken.

* * *

## Wachtrij

Zodra de spellen geïmporteerd zijn, zijn ze toegevoegd aan de wachtrij. ASF doorloopt automatisch de achtergrondwachtrij zolang de bot is verbonden met het Steam-netwerk en de wachtrij niet leeg is. Een productcode, waarbij is geprobeerd die te activeren en niet resulteerde in `RateLimited`, wordt verwijderd uit de wachtrij. De status wordt dan correct weggeschreven naar een bestand in de `config` map. Als een code werd gebruikt in het proces, wordt die opgeslagen in een `BotNaam.keys.used` bestand (bijv. `NoDetail`, `BadActivationCode`, `DuplicateActivationCode`). Als een code niet werd gebruikt, wordt die opgeslagen in een `BotNaam.keys.unused` bestand. ASF gebruikt opzettelijk de door jou opgegeven naam van het spel, omdat de productsleutel niet altijd een betekenisvolle naam krijgt van het Steam-netwerk - dus op die manier kun je de productsleutel zelfs je eigen naam geven als dat nodig/gewenst is.

Als tijdens het proces het account de `RateLimited` status krijgt, wordt de wachtrij tijdelijk voor een uur onderbroken om de cooldown af te wachten. Daarna gaat het proces verder waar het gebleven is, totdat de wachtrij leeg is.

* * *

## Voorbeeld

Stel je hebt een lijst met 100 codes. Eerst moet je een nieuw `BotNaam.keys.new` bestand maken in de ASF `config` map. We hebben een `.new` extensie toegevoegd om ASF te laten weten dat het dit bestand niet onmiddellijk moet lezen op het moment dat het gemaakt werd (aangezien het een nieuw leeg bestand is, wat nog niet gereed is om te importeren).

Je kunt nu het nieuwe bestand openen en de lijst met de 100 codes kopiëren en plakken - en indien nodig de opmaak corrigeren. Na de benodigde correcties heeft het `BotNaam.keys.new` bestand precies 100 regels (of 101, met de laatste nieuwe regel). Elke regel heeft een structuur met `GameNaam\tproductsleutel\n`, waarbij`\t` het tabteken is en`\n` de regeleinde aanduidt.

Je bent nu klaar om dit bestand te hernoemen van `BotNaam.keys.new` naar `BotNaam.keys` om ASF te laten weten dat het gereed is om te worden verwerkt. Wanneer je dit doet, zal ASF automatisch het bestand importeren (een herstart is niet nodig) en daarna verwijderen, waarbij wordt bevestigd dat al je spellen worden verwerkt en aan de wachtrij worden toegevoegd.

In plaats van het `BotName.keys` bestand te gebruiken, kun je ook het IPC API-eindpunt gebruiken of zelfs beide combineren als je dat wilt.

After some time, `BotName.keys.used` and `BotName.keys.unused` files might get generated. Those files contain results of our redeeming process. For example, you could rename `BotName.keys.unused` into `BotName2.keys` file and therefore submit our unused keys for some other bot, since previous bot didn't make use of those keys himself. Or you could simply copy-paste unused keys to some other file and keep it for later, your call. Keep in mind that as ASF goes through the queue, new entries will be added to our output `used` and `unused` files, therefore it's recommended to wait for the queue to be fully emptied before making use of them. If you absolutely must access those files before queue is fully emptied, you should firstly **move** output file you want to access to some other directory, **then** parse it. This is because ASF can append some new results while you're doing your thing, and that could potentially lead to loss of some keys if you read a file having e.g. 3 keys inside, then delete it, totally missing the fact that ASF added 4 other keys to your removed file in the meantime. If you want to access those files, ensure to move them away from ASF `config` directory before reading them, for example by rename.

It's also possible to add extra games to import while having some games already in our queue, by repeating all above steps. ASF will properly add our extra entries to already-ongoing queue and deal with it eventually.

* * *

## Opmerkingen

Background keys redeemer uses `OrderedDictionary` under the hood, which means that your cd-keys will have preserved order as they were specified in the file (or IPC API call). This means that you can (and should) provide a list where given cd-key can only have direct dependencies on cd-keys listed above, but not below. For example, this means that if you have DLC `D` that requires game `G` to be activated firstly, then cd-key for game `G` should **always** be included before cd-key for DLC `D`. Likewise, if DLC `D` would have dependencies on `A`, `B` and `C`, then all 3 should be included before (in any order, unless they have dependencies on their own).

Not following the scheme above will result in your DLC not being activated with `DoesNotOwnRequiredApp`, even if your account would be eligible for activating it after going through its entire queue. If you want to avoid that then you must make sure that your DLC is always included after the base game in your queue.