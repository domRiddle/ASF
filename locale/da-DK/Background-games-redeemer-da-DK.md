# Produktaktivering i baggrunden

Produktaktivering i baggrunden er en specialindbygget ASF funktion som tillader dig at importere et givent sæt Steam cd-keys (med deres navne) som aktiveres i baggrunden. Dette er især nyttigt hvis du har mange nøgler der skal indløses og hvor du er sikker på at ramme `RateLimited` **[status](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ#what-is-the-meaning-of-status-when-redeeming-a-key)** før du er færdig med hele partiet.

Produktaktivering i baggrunden er lavet til et enkelt bot-område, hvilket betyder at den ikke bruger `RedeemingPreferences`. Denne funktion kan bruges med (eller i stedet for) `reedem` **[kommando](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**, om nødvendigt.

* * *

## Import

Importeringsprocessen kan gøres på to måder - enten ved brug af en fil eller via IPC.

### Fil

ASF vil i dens `config`-mappe genkende en fil med navnet `BotName.keys` hvor `BotName` er din bots navn. Denne fil har forventet og fast struktur af navnet på spillet med cd-nøglen, adskilt et tabulatortegn og slutning med en ny linje for at angive starten på den næste indtastning. Hvis der bruges flere tabulatortegn, ses den første indtastning som spillets navn, sidste indtastning en cd-nøgle og alt imellem disse ignoreres. For eksempel:

    POSTAL 2    ABCDE-EFGHJ-IJKLM
    Domino Craft VR 12345-67890-ZXCVB
    A Week of Circus Terror POIUY-KJHGD-QWERT
    Terraria    DetteErIgnoreret   DetteErOgsåIgnoreret    ZXCVB-ASDFG-QWERT
    

Som et alternativ kan du også bruge "keys only format" (stadig med en ny linje mellem hver indtastning). ASF vil i dette tilfælde bruge Steam's svar (hvis muligt) til at udfylde det rigtige navn. For alle typer af nøgletagging anbefaler vi at du navngiver nøglerne selv, fordi pakker der indløses på Steam ikke skal følge den samme logik som det spil de aktiverer, så afhængigt af hvad udvikleren har indtastet kan du risikere at se det rigtige spilnavn, "Bundle"-navn (f.eks. Humble Indie Bundle 18) eller direkte forkert og potentielt skadelige navne (f.eks. Half-Life 4).

    ABCDE-EFGHJ-IJKLM
    12345-67890-ZXCVB
    POIUY-KJHGD-QWERT
    ZXCVB-ASDFG-QWERT
    

Uanset hvilket format du vælger at bruge vil ASF importere din `nøgle` fil, enten ved botstart eller senere ved udførsel. Efter en vellykket analyse af din fil og eventuel fjernelse af ugyldige indtastninger, vil alle opdagede spil blive tilføjet til baggrundskøen og selve `BotName.keys`-filen vil blive fjernet fra `config`-mappen.

### IPC

Udover brugen af nøglefiler som nævnt ovenfor, kan ASF også bruge `GamesToRedeemInBackground` **[API endpoint](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#asf-api)** som kan køres af alle IPC værktøjer, inklusiv vores ASF-ui. Brug af IPC kan være lettere da du selv kan analysere det nødvendige, som brugen af en brugerdefineret delimiter i stedet for at blive tvunget til at bruge tab-karakter eller endda at introducere din helt egen brugerdefinerede nøglestruktur.

* * *

## Kø

Når spil er succesfuldt importeret, er de tilføjet til køen. ASF automatically goes through its background queue as long as bot is connected to Steam network, and the queue is not empty. A key that was attempted to be redeemed and did not result in `RateLimited` is removed from the queue, with its status properly written to a file in `config` directory - either `BotName.keys.used` if the key was used in the process (e.g. `NoDetail`, `BadActivationCode`, `DuplicateActivationCode`), or `BotName.keys.unused` otherwise. ASF intentionally uses your provided game's name since key is not guaranteed to have a meaningful name returned by Steam network - this way you can tag your keys using even custom names if needed/wanted.

If during the process our account hits `RateLimited` status, the queue is temporarily suspended for a full hour in order to wait for cooldown to disappear. Afterwards, the process continues where it left, until the entire queue is empty.

* * *

## Eksempel

Lad os antage, at du har en liste med 100 nøgler. Firstly you should create a new `BotName.keys.new` file in ASF `config` directory. We appended `.new` extension in order to let ASF know that it shouldn't pick up this file immediately the moment it's created (as it's new empty file, not ready for import yet).

Now you can open our new file and copy-paste list of our 100 keys there, fixing the format if needed. After fixes our `BotName.keys.new` file will have exactly 100 (or 101, with last newline) lines, each line having a structure of `GameName\tcd-key\n`, where `\t` is tab character and `\n` is newline.

You're now ready to rename this file from `BotName.keys.new` to `BotName.keys` in order to let ASF know that it's ready to be picked up. The moment you do this, ASF will automatically import the file (without a need of restart) and delete it afterwards, confirming that all our games were parsed and added to the queue.

Instead of using `BotName.keys` file, you could also use IPC API endpoint, or even combining both if you want to.

After some time, `BotName.keys.used` and `BotName.keys.unused` files will be generated. Those files contain results of our redeeming process. For example, you could rename `BotName.keys.unused` into `BotName2.keys` file and therefore submit our unused keys for some other bot, since previous bot didn't make use of those keys himself. Or you could simply copy-paste unused keys to some other file and keep it for later, your call. Keep in mind that as ASF goes through the queue, new entries will be added to our output `used` and `unused` files, therefore it's recommended to wait for the queue to be fully emptied before making use of them. If you absolutely must access those files before queue is fully emptied, you should firstly **move** output file you want to access to some other directory, **then** parse it. This is because ASF can append some new results while you're doing your thing, and that could potentially lead to loss of some keys if you read a file having e.g. 3 keys inside, then delete it, totally missing the fact that ASF added 4 other keys to your removed file in the meantime. If you want to access those files, ensure to move them away from ASF `config` directory before reading them, for example by rename.

It's also possible to add extra games to import while having some games already in our queue, by repeating all above steps. ASF will properly add our extra entries to already-ongoing queue and deal with it eventually.

* * *

## Bemærkninger

Background keys redeemer uses `OrderedDictionary` under the hood, which means that your cd-keys will have preserved order as they were specified in the file (or IPC API call). This means that you can (and should) provide a list where given cd-key can only have direct dependencies on cd-keys listed above, but not below. For example, this means that if you have DLC `D` that requires game `G` to be activated firstly, then cd-key for game `G` should **always** be included before cd-key for DLC `D`. Likewise, if DLC `D` would have dependencies on `A`, `B` and `C`, then all 3 should be included before (in any order, unless they have dependencies on their own).

Not following the scheme above will result in your DLC not being activated with `DoesNotOwnRequiredApp`, even if your account would be eligible for activating it after going through its entire queue. If you want to avoid that then you must make sure that your DLC is always included after the base game in your queue.