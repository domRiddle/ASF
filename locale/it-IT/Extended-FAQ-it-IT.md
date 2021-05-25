# FAQ Estesa

La nostra FAQ estesa copre le domande un po' meno comuni e le risposte che potresti avere. Per questioni più comuni, sei invece pregato di visitare la nostra **[FAQ di base](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ)**.

---

### Chi ha creato ASF?

ASF è stato creato da **[Archi](https://github.com/JustArchi)** a Ottobre 2015. Nel caso te lo stessi chiedendo, sono come te un **[utente di Steam](https://steamcommunity.com/profiles/76561198006963719)**. Oltre a giocare a giochi, amo anche mettere in uso le mie abilità e determinazione, che puoi esplorare ora. Nessuna grande azienda è coinvolta qui, nessun team di sviluppatori e nessun budget di $1M per coprire tutto questo; solo io, a correggere cose che non sono rotte.

Tuttavia, ASF è un progetto open-source e non so come dire che non sono dietro tutto ciò che vedete qui. Abbiamo alcuni **[altri](https://github.com/JustArchiNET?q=ASF-)** progetti di ASF in via di sviluppo quasi esclusivamente da altri sviluppatori. Anche il progetto principale di ASF ha molti **[collaboratori](https://github.com/JustArchiNET/ArchiSteamFarm/graphs/contributors)** che mi hanno aiutato a far succedere tutto questo. Oltre ciò, ci sono diversi servizi di terze parti a supporto dello sviluppo di ASF, specialmente **[GitHub](https://github.com)**, **[JetBrains](https://www.jetbrains.com)**, **[AppVeyor](https://www.appveyor.com)** e **[Crowdin](https://crowdin.com)**. Non si possono anche dimenticare tutte i meravigliosi strumenti e le librerie che hanno reso possibile ASF, come **[Rider](https://www.jetbrains.com/rider)** che usiamo come IDE (adoriamo le aggiunte di **[ReSharper](https://www.jetbrains.com/resharper)**) e specialmente **[SteamKit2](https://github.com/SteamRE/SteamKit)** senza cui ASF non esisterebbe in primo luogo. ASF non sarebbe qui oggi senza i miei **[sponsor](https://github.com/sponsors/JustArchi)**, **[patron](https://www.patreon.com/JustArchi)** e vari donatori, che mi supportano in tutto ciò che sto facendo qui.

Grazie a tutti per l'aiuto nello sviluppo di ASF! Siete fantastici.

---

### Perché ASF è stato creato in primo luogo?

ASF è stato creato con l'obiettivo principale di essere uno strumento di inattività completamente automatizzato di Steam per Linux, senza il bisogno di alcuna dipendenza esterna (come il client di Steam). Difatti, rimane ancora il suo obiettivo e punto focale principale, perché il mio concetto di ASF non è cambiato da allora e lo uso ancora allo stesso modo di come lo usavo nel 2015. Di certo, ci sono stati davvero **molti** cambiamenti da allora, e sono felice di vedere quanto ASF sia progredito, principalmente grazie ai suoi utenti, perché non programmerei mai nemmeno metà delle funzionalità se fossi totalmente solo.

È bello notare che ASF non è mai stato pensato per competere con altri idler, specialmente **[Idle Master](https://www.steamidlemaster.com)**, poiché ASF non era progettato come app desktop/utente e ancora oggi non lo è. Se analizzi lo scopo principale di ASF sopra descritto, vedrai come Idle Master è **l'esatto opposto** di tutto questo. Mentre puoi sicuramente trovare idler simili ad ASF oggi, allora nulla era abbastanza per me (e ancora non lo è oggi), quindi ho creato il mio idler, come lo volevo. Nel tempo gli utenti sono passati ad ASF principalmente per la sua robustezza, stabilità e sicurezza, ma anche per tutte le funzionalità che ho sviluppato in tutti questi anni. Oggi, ASF è meglio che mai.

---

### OK, dov'è il trucco? Cosa guadagni dalla condivisione di ASF?

Nessun trucco, ho creato ASF **per me stesso** e l'ho condiviso con il resto della community nella speranza che diventasse utile. Esattamente la stessa cosa è successa nel 1991, quando Linus Torvalds **[condivise il suo primo kernel di Linux](https://groups.google.com/forum/#!msg/comp.os.Minix/dlNtH7RRrGA/SwRavCzVE7gJ)** con il resto del mondo. Nessun malware nascosto, mining di dati, mining di criptovalute o ogni altra attività che genererebbe alcun beneficio finanziario per me. Il progetto di ASF è supportato interamente da donazioni facoltative inviate da utenti felici come te. Puoi usare ASF esattamente al mio stesso modo, e se ti piace, puoi sempre offrirmi un caffè, mostrandomi la tua gratitudine per ciò che sto facendo.

Uso ASF anche come un perfetto esempio di un moderno progetto in C# che colpisce sempre alla perfezione e ha le migliori pratiche, che sia con la tecnologia, la gestione del progetto o il codice stesso. È la mia definizione di "cose fatte bene", quindi se per caso riesci a imparare qualcosa di utile dal mio progetto, allora questo mi renderà solo più felice.

---

### Ho lanciato il programma il 1 di Aprile e la lingua di ASF è cambiata in qualcosa di strano, che succede?

CONGRATULASHUNS ON DISCOVERIN R APRIL FOOLS EASTR EGG! Se non hai impostato l'opzione personalizzata `CurrentCulture`, allora ASF lanciato il primo d'Aprile userà in realtà la lingua **[LOLcat](https://en.wikipedia.org/wiki/Lolcat)** invece della tua lingua di sistema. Se per qualche motivo vorresti disabilitare questo comportamento, puoi semplicemente impostare `CurrentCulture` al locale che vorresti piuttosto usare. Bello da notare anche che puoi abilitare incondizionatamente il nostro easter egg, impostando il tuo valore `CurrentCulture` a `qps-Ploc`.