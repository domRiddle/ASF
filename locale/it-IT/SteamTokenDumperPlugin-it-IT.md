# SteamTokenDumperPlugin

`SteamTokenDumperPlugin` è il **[plugin](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins)** ufficiale di ASF disponibile dalla V4.2.2.2 di ASF, sviluppata da noi, che ti consente di contribuire al progetto **[SteamDB](https://steamdb.info)** condividendo i token del pacchetto, i token dell'app e le chiavi di depot a cui il tuo profilo di Steam ha accesso. Le informazioni estese sui dati raccolti e perché SteamDB li necessita si possono trovare sulla pagina di SteamDB **[Token Dumper](https://steamdb.info/tokendumper)**. I dati inoltrati non includono alcuna informazione potenzialmente sensibile e non possiedono rischi di sicurezza/privacy, come dichiarato nella descrizione sopra.

---

## Abilitare il plugin

ASF viene rilasciato con il plugin `SteamTokenDumper` incluso nel rilascio, ma tale plugin è disabilitato per impostazione predefinita. È possibile abilitare il plugin impostando la proprietà `SteamTokenDumperPluginEnabled` nella configurazione globale di ASF a `true`, in sintassi JSON:

```json
{
  "SteamTokenDumperPluginEnabled": true
}
```

All'avvio dell'applicazione ASF, il plugin comunicherà se è stato abilitato con successo attraverso il meccanismo di logging standard. È inoltre possibile abilitare il plugin attraverso il generatore di configurazione web.

---

## Dettagli tecnici

All'abilitazione, il plugin userà i bot che esegui in ASF per la raccolta dei dati in forma di token del pacchetto, token dell'app e chiavi di depot cui i tuoi bot hanno accesso. Il modulo di raccolta dei dati include le routine passive e attive che dovrebbero minimizzare il sovraccarico aggiuntivo causato dalla raccolta di dati.

Per soddisfare il caso d'uso pianificato, oltre alla routine di raccolta dati sopra menzionata, la routine di invio è inizializzata come responsabile per la determinazione di quali dati vanno inviati a SteamDB su base periodica. Questa routine sarà eseguita fino a `1` ora dal tuo avvio di ASF, e si ripeterà ogni `24` ore. Il plugin farà del suo meglio per ridurre la quantità di dati che devono esser inviati, dunque è possibile che alcuni dati che il plugin raccoglie saranno determinati come inutili da inviare, e dunque saltati (per esempio l'aggiornamento dell'app che non cambia il token d'accesso).

Il plugin usa un database della cache persistente salvato nella posizione `config/SteamTokenDumper.cache`, che serve come scopo simile a `config/ASF.db` per ASF. Il file è usato per registrare i dati raccolti e inviati e minimizza la quantità di lavoro da eseguire per le diverse esecuzioni di ASF. Rimuovere il file causa il riavvio da zero del processo, cosa da evitare se possibile.

---

## Dati

ASF include il contributore `steamID` nella richiesta, determinato come `SteamOwnerID`, che imposti in ASF, o nel caso non lo avessi fatto, l'ID di Steam del bot che possiede gran parte delle licenze. Il contributore annunciato potrebbe ricevere vantaggi aggiuntivi da SteamDB per aiuto continuo (es. rango di donatore sul sito web), ma è totalmente a tua discrezione.

In ogni caso, lo staff di SteamDB vorrebbe ringraziarti in anticipo per il tuo aiuto. I dati inoltrati consentono a SteamDB di operare, in particolare per monitorare le informazioni sui pacchetti, le app e i depot, cosa che non sarà più possibile senza il tuo aiuto.