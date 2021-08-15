# FAQ Estesa

La nostra FAQ estesa copre le domande un po' meno comuni e le risposte che potresti avere. Per questioni più comuni, sei invece pregato di visitare la nostra **[FAQ di base](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ)**.

---

### Chi ha creato ASF?

ASF è stato creato da **[Archi](https://github.com/JustArchi)** a Ottobre 2015. Nel caso te lo stessi chiedendo, sono come te un **[utente di Steam](https://steamcommunity.com/profiles/76561198006963719)**. Oltre a giocare a giochi, amo anche mettere in uso le mie abilità e determinazione, che puoi esplorare ora. Nessuna grande azienda è coinvolta qui, nessun team di sviluppatori e nessun budget di $1M per coprire tutto questo; solo io, a correggere cose che non sono rotte.

Tuttavia, ASF è un progetto open-source e non so come dire che non sono dietro tutto ciò che vedete qui. Abbiamo alcuni **[altri](https://github.com/JustArchiNET?q=ASF-)** progetti di ASF in via di sviluppo quasi esclusivamente da altri sviluppatori. Anche il progetto principale di ASF ha molti **[collaboratori](https://github.com/JustArchiNET/ArchiSteamFarm/graphs/contributors)** che mi hanno aiutato a far succedere tutto questo. On top of that, there are several third-party services supporting ASF development, especially **[GitHub](https://github.com)**, **[JetBrains](https://www.jetbrains.com)** and **[Crowdin](https://crowdin.com)**. Non si possono anche dimenticare tutte i meravigliosi strumenti e le librerie che hanno reso possibile ASF, come **[Rider](https://www.jetbrains.com/rider)** che usiamo come IDE (adoriamo le aggiunte di **[ReSharper](https://www.jetbrains.com/resharper)**) e specialmente **[SteamKit2](https://github.com/SteamRE/SteamKit)** senza cui ASF non esisterebbe in primo luogo. ASF non sarebbe qui oggi senza i miei **[sponsor](https://github.com/sponsors/JustArchi)**, **[patron](https://www.patreon.com/JustArchi)** e vari donatori, che mi supportano in tutto ciò che sto facendo qui.

Grazie a tutti per l'aiuto nello sviluppo di ASF! Siete fantastici.

---

### Perché ASF è stato creato in primo luogo?

ASF was created with primary purpose of being fully automated Steam farming tool for Linux, without a need of any external dependencies (such as Steam client). Difatti, rimane ancora il suo obiettivo e punto focale principale, perché il mio concetto di ASF non è cambiato da allora e lo uso ancora allo stesso modo di come lo usavo nel 2015. Di certo, ci sono stati davvero **molti** cambiamenti da allora, e sono felice di vedere quanto ASF sia progredito, principalmente grazie ai suoi utenti, perché non programmerei mai nemmeno metà delle funzionalità se fossi totalmente solo.

It's nice to note that ASF was never made to compete with other, similar programs, especially **[Idle Master](https://www.steamidlemaster.com)**, because ASF was never designed to be a desktop/user app, and it still isn't today. Se analizzi lo scopo principale di ASF sopra descritto, vedrai come Idle Master è **l'esatto opposto** di tutto questo. While you can most definitely find similar to ASF programs today, nothing was good enough for me back then (and still isn't today), so I created my own software, the way I wanted it. Nel tempo gli utenti sono passati ad ASF principalmente per la sua robustezza, stabilità e sicurezza, ma anche per tutte le funzionalità che ho sviluppato in tutti questi anni. Oggi, ASF è meglio che mai.

---

### OK, dov'è il trucco? Cosa guadagni dalla condivisione di ASF?

Nessun trucco, ho creato ASF **per me stesso** e l'ho condiviso con il resto della community nella speranza che diventasse utile. Esattamente la stessa cosa è successa nel 1991, quando Linus Torvalds **[condivise il suo primo kernel di Linux](https://groups.google.com/forum/#!msg/comp.os.Minix/dlNtH7RRrGA/SwRavCzVE7gJ)** con il resto del mondo. Nessun malware nascosto, mining di dati, mining di criptovalute o ogni altra attività che genererebbe alcun beneficio finanziario per me. Il progetto di ASF è supportato interamente da donazioni facoltative inviate da utenti felici come te. Puoi usare ASF esattamente al mio stesso modo, e se ti piace, puoi sempre offrirmi un caffè, mostrandomi la tua gratitudine per ciò che sto facendo.

Uso ASF anche come un perfetto esempio di un moderno progetto in C# che colpisce sempre alla perfezione e ha le migliori pratiche, che sia con la tecnologia, la gestione del progetto o il codice stesso. È la mia definizione di "cose fatte bene", quindi se per caso riesci a imparare qualcosa di utile dal mio progetto, allora questo mi renderà solo più felice.

---

### How do I verify that the downloaded files are genuine?

As part of our releases on GitHub, we utilize a very similar verification process as the one used by **[Debian](https://www.debian.org/CD/verify)**. In every official release starting with ASF V5.1.3.3, in addition to `zip` files you can find `SHA512SUMS` and `SHA512SUMS.sign` files. Download them for verification purposes.

Firstly, you should use `SHA512SUMS` file in order to verify that `SHA-512` checksum of the selected `zip` files matches the one we calculated ourselves. On Linux, you can use `sha512sum` utility for that purpose.


```
$ sha512sum -c --ignore-missing SHA512SUMS
ASF-linux-x64.zip: OK
```

On Windows, we can do that from powershell, although you have to manually verify with `SHA512SUMS`:

```
PS > Get-Content SHA512SUMS | Select-String -Pattern ASF-linux-x64.zip

f605e573cc5e044dd6fadbc44f6643829d11360a2c6e4915b0c0b8f5227bc2a257568a014d3a2c0612fa73907641d0cea455138d2e5a97186a0b417abad45ed9  ASF-linux-x64.zip


PS > Get-FileHash -Algorithm SHA512 -Path ASF-linux-x64.zip

Algoritmo       Hash                                                                   Percorso
---------       ----                                                                   ----
SHA512          F605E573CC5E044DD6FADBC44F6643829D11360A2C6E4915B0C0B8F5227BC2A2575... ASF-linux-x64.zip
```

This way we ensured that whatever was written to `SHA512SUMS` matches the resulting files and they weren't tampered with. However, it doesn't prove yet that `SHA512SUMS` file you checked against really comes from us. For that, we'll use `SHA512SUMS.sign` file, which holds digital PGP signature proving the authenticity of `SHA512SUMS`. We can use `gpg` utility for that purpose, both on **[Linux](https://gnupg.org/download/index.html)** and **[Windows](https://gpg4win.org)** (change `gpg` command into `gpg.exe` on Windows).

```
$ gpg --verify SHA512SUMS.sign SHA512SUMS
gpg: Signature made Mon 02 Aug 2021 00:34:18 CEST
gpg:                using EDDSA key 224DA6DB47A3935BDCC3BE17A3D181DF2D554CCF
gpg: Can't check signature: No public key
```

As you can see, the file indeed holds a valid signature, but of unknown origin. You'll need to import ArchiBot's **[public key](https://raw.githubusercontent.com/JustArchi-ArchiBot/JustArchi-ArchiBot/main/ArchiBot_public.asc)** that we sign the `SHA-512` sums with for full validation.

```
$ curl https://raw.githubusercontent.com/JustArchi-ArchiBot/JustArchi-ArchiBot/main/ArchiBot_public.asc -o ArchiBot_public.asc
$ gpg --import ArchiBot_public.asc
gpg: /home/archi/.gnupg/trustdb.gpg: trustdb created
gpg: key A3D181DF2D554CCF: public key "ArchiBot <ArchiBot@JustArchi.net>" imported
gpg: Total number processed: 1
gpg:               imported: 1

```

Finally, you can verify the `SHA512SUMS` file again:

```
$ gpg --verify SHA512SUMS.sign SHA512SUMS
gpg: Signature made Mon 02 Aug 2021 00:34:18 CEST
gpg:                using EDDSA key 224DA6DB47A3935BDCC3BE17A3D181DF2D554CCF
gpg: Good signature from "ArchiBot <ArchiBot@JustArchi.net>" [unknown]
gpg: WARNING: This key is not certified with a trusted signature!
gpg:          There is no indication that the signature belongs to the owner.
Primary key fingerprint: 224D A6DB 47A3 935B DCC3  BE17 A3D1 81DF 2D55 4CCF
```

This has verified that the `SHA512SUMS.sign` holds a valid signature of our `224DA6DB47A3935BDCC3BE17A3D181DF2D554CCF` key for `SHA512SUMS` file that you've verified against.

You could be wondering where the last warning comes from. You've successfully imported our key, but didn't decide to trust it just yet. While this is not mandatory, we can cover it as well. Normally this includes verifying through different channel (e.g. phone call, SMS) that the key is valid, then signing the key with your own to trust it. For this example, you can consider this wiki entry as such (very weak) different channel, since the original key comes from **[ArchiBot's profile](https://github.com/JustArchi-ArchiBot)**. In any case we'll assume that you have enough of confidence as it is.

Firstly, **[generate private key for yourself](https://help.ubuntu.com/community/GnuPrivacyGuardHowto#Generating_an_OpenPGP_Key)**, if you don't have one just yet. We'll use `--quick-gen-key` as a quick example.

```
$ gpg --batch --passphrase '' --quick-gen-key "$(whoami)"
gpg: /home/archi/.gnupg/trustdb.gpg: trustdb created
gpg: key E4E763905FAD148B marked as ultimately trusted
gpg: directory '/home/archi/.gnupg/openpgp-revocs.d' created
gpg: revocation certificate stored as '/home/archi/.gnupg/openpgp-revocs.d/8E5D685F423A584569686675E4E763905FAD148B.rev'
```

Now you can sign our key with yours in order to trust it:

```
$ gpg --sign-key 224DA6DB47A3935BDCC3BE17A3D181DF2D554CCF

pub  ed25519/A3D181DF2D554CCF
     created: 2021-05-22  expires: never       usage: SC
     trust: unknown       validity: unknown
sub  cv25519/E527A892E05B2F38
     created: 2021-05-22  expires: never       usage: E
[ unknown] (1). ArchiBot <ArchiBot@JustArchi.net>


pub  ed25519/A3D181DF2D554CCF
     created: 2021-05-22  expires: never       usage: SC
     trust: unknown       validity: unknown
 Primary key fingerprint: 224D A6DB 47A3 935B DCC3  BE17 A3D1 81DF 2D55 4CCF

     ArchiBot <ArchiBot@JustArchi.net>

Are you sure that you want to sign this key with your
key "archi" (E4E763905FAD148B)

Really sign? (y/N) y
```

And done, after trusting our key, `gpg` should no longer display the warning when verifying:

```
$ gpg --verify SHA512SUMS.sign SHA512SUMS
gpg: Signature made Mon 02 Aug 2021 00:34:18 CEST
gpg:                using EDDSA key 224DA6DB47A3935BDCC3BE17A3D181DF2D554CCF
gpg: Good signature from "ArchiBot <ArchiBot@JustArchi.net>" [full]
```

Notice the `[unknown]` trust indicator changing into `[full]` once you signed our key with yours.

Congratulations, you've verified that nobody has tampered with the release you've downloaded! 👍

---

### Ho lanciato il programma il 1 di Aprile e la lingua di ASF è cambiata in qualcosa di strano, che succede?

CONGRATULASHUNS ON DISCOVERIN R APRIL FOOLS EASTR EGG! Se non hai impostato l'opzione personalizzata `CurrentCulture`, allora ASF lanciato il primo d'Aprile userà in realtà la lingua **[LOLcat](https://en.wikipedia.org/wiki/Lolcat)** invece della tua lingua di sistema. Se per qualche motivo vorresti disabilitare questo comportamento, puoi semplicemente impostare `CurrentCulture` al locale che vorresti piuttosto usare. Bello da notare anche che puoi abilitare incondizionatamente il nostro easter egg, impostando il tuo valore `CurrentCulture` a `qps-Ploc`.