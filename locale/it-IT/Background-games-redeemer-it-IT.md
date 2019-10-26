# Riscatto giochi in background

Il riscatto giochi in background è una caratteristica speciale incorporata in ASF che ti permette di importare un dato numero di chiavi di Steam (insieme con i loro nomi) per essere riscattate in background. Questo è particolarmente utile se hai molte chiavi da riscattare e sei sicuro di raggiungere lo **[stato](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ#what-is-the-meaning-of-status-when-redeeming-a-key)** `RateLimited` prima di aver terminato l'intero batch.

Il riscatto giochi in background è fatto per un singolo bot, questo significa che non utilizza `RedeemingPreferences`. Questa funzione può essere usata insieme a (o al posto di) `redeem` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**, se necessario.

* * *

## Importa

L'importazione può essere effettuata in due modi: usando un file o con IPC.

### File

ASF riconoscerà nella cartella `config` un file chiamato `BotName.keys` dove `BotName` è il nome del tuo bot. Questo file si aspetta una struttura ben precisa con il nome del gioco e la cd-key separati da un carattere tabulato e una nuova riga per indicare la prossima entrata. Se più tab vengono usati, allora la prima voce viene considerata il nome del gioco, l'ultima la cd-key e ciò che è nel mezzo viene ignorato. Per esempio:

    POSTAL 2    ABCDE-EFGHJ-IJKLM
    Domino Craft VR    12345-67890-ZXCVB
    A Week of Circus Terror    POIUY-KJHGD-QWERT
    Terraria    QuestoVieneIgnorato    AncheQuestoVieneIgnorato    ZXCVB-ASDFG-QWERT
    

Alternativamente, puoi anche usare chiavi di solo formato (ancora con una nuova riga tra ogni voce). ASF in questo caso userà la risposta di Steam (se possibile) per compilare il giusto nome. Per ogni tipo di tag delle chiavi, raccomandiamo che tu stesso nomini le tue chiavi, come pacchetti riscattati su Steam, senza seguire la logica dei giochi che stai attivando, quindi in base a cosa ha messo lo sviluppatore, potresti vedere nomi dei giochi corretti, nomi dei pacchetti personalizzati (es. Humble Indie Bundle 18) o totalmente errati e potenzialmente anche maligni (es. Half-Life 4).

    ABCDE-EFGHJ-IJKLM
    12345-67890-ZXCVB
    POIUY-KJHGD-QWERT
    ZXCVB-ASDFG-QWERT
    

Indipendentemente da quale formato hai deciso di mantenere, ASF importerà il tuo file `chiavi`, all'avvio del bot, o dopo durante l'esecuzione. Dopo aver analizzato con successo il tuo file ed eventualmente omissione di voci non valide, tutti i giochi propriamente rilevati saranno aggiunti alla coda in background, ed il file `BotName.keys` sarà rimosso dalla directory `config`.

### IPC

Oltre ad usare il file delle chiavi sopra menzionato, ASF espone anche **[l'endpoint API ASF](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#asf-api)** `GamesToRedeemInBackground` che può essere eseguito da qualsiasi strumento IPC, inclusa la nostra ASF-ui. L'uso di IPC potrebbe essere più potente, come puoi fare tu stesso un'analisi appropriata, come usare un delimitatore personalizzato invece di essere forzato ad un carattere di scheda, o anche introdurre la tua struttura di chiavi personalizzate interamente tua.

* * *

## Coda

Una volta che i giochi sono importati con successo, sono aggiunti alla coda. ASF passa automaticamente per la sua coda in background finché il bot è connesso alla rete Steam, e la coda non è vuota. A key that was attempted to be redeemed and did not result in `RateLimited` is removed from the queue, with its status properly written to a file in `config` directory - either `BotName.keys.used` if the key was used in the process (e.g. `NoDetail`, `BadActivationCode`, `DuplicateActivationCode`), or `BotName.keys.unused` otherwise. ASF intentionally uses your provided game's name since key is not guaranteed to have a meaningful name returned by Steam network - this way you can tag your keys using even custom names if needed/wanted.

If during the process our account hits `RateLimited` status, the queue is temporarily suspended for a full hour in order to wait for cooldown to disappear. Afterwards, the process continues where it left, until the entire queue is empty.

* * *

## Example

Let's assume that you have a list of 100 keys. Firstly you should create a new `BotName.keys.new` file in ASF `config` directory. We appended `.new` extension in order to let ASF know that it shouldn't pick up this file immediately the moment it's created (as it's new empty file, not ready for import yet).

Now you can open our new file and copy-paste list of our 100 keys there, fixing the format if needed. After fixes our `BotName.keys.new` file will have exactly 100 (or 101, with last newline) lines, each line having a structure of `GameName\tcd-key\n`, where `\t` is tab character and `\n` is newline.

You're now ready to rename this file from `BotName.keys.new` to `BotName.keys` in order to let ASF know that it's ready to be picked up. The moment you do this, ASF will automatically import the file (without a need of restart) and delete it afterwards, confirming that all our games were parsed and added to the queue.

Instead of using `BotName.keys` file, you could also use IPC API endpoint, or even combining both if you want to.

After some time, `BotName.keys.used` and `BotName.keys.unused` files will be generated. Those files contain results of our redeeming process. For example, you could rename `BotName.keys.unused` into `BotName2.keys` file and therefore submit our unused keys for some other bot, since previous bot didn't make use of those keys himself. Or you could simply copy-paste unused keys to some other file and keep it for later, your call. Keep in mind that as ASF goes through the queue, new entries will be added to our output `used` and `unused` files, therefore it's recommended to wait for the queue to be fully emptied before making use of them. If you absolutely must access those files before queue is fully emptied, you should firstly **move** output file you want to access to some other directory, **then** parse it. This is because ASF can append some new results while you're doing your thing, and that could potentially lead to loss of some keys if you read a file having e.g. 3 keys inside, then delete it, totally missing the fact that ASF added 4 other keys to your removed file in the meantime. If you want to access those files, ensure to move them away from ASF `config` directory before reading them, for example by rename.

It's also possible to add extra games to import while having some games already in our queue, by repeating all above steps. ASF will properly add our extra entries to already-ongoing queue and deal with it eventually.

* * *

## Remarks

Background keys redeemer uses `OrderedDictionary` under the hood, which means that your cd-keys will have preserved order as they were specified in the file (or IPC API call). This means that you can (and should) provide a list where given cd-key can only have direct dependencies on cd-keys listed above, but not below. For example, this means that if you have DLC `D` that requires game `G` to be activated firstly, then cd-key for game `G` should **always** be included before cd-key for DLC `D`. Likewise, if DLC `D` would have dependencies on `A`, `B` and `C`, then all 3 should be included before (in any order, unless they have dependencies on their own).

Not following the scheme above will result in your DLC not being activated with `DoesNotOwnRequiredApp`, even if your account would be eligible for activating it after going through its entire queue. If you want to avoid that then you must make sure that your DLC is always included after the base game in your queue.