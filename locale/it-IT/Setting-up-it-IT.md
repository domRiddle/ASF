# Primi passi

Se sei qui per la prima volta, benvenuto! Siamo felici di vedere un altro viaggiatore interessato nel nostro progetto, ma tieni a mente che da un grande potere derivano grandi responsabilità - ASF è in grado di fare innumerevoli cose con Steam, ma soltanto finché **hai la volontà di imparare ad utilizzarlo**. C'è da considerare una ripida curva d'apprendimento e per questo ci aspettiamo che tu legga a dovere questo wiki, dove viene spiegato nel dettaglio il funzionamento di ogni cosa.

Se sei ancora qui vuol dire che hai già letto l'introduzione, il che è positivo. A meno che non ci sei passato sopra, e in tal caso sappi che presto **[avrai il tuo bel da fare](https://www.youtube.com/watch?v=WJgt6m6njVw)**... Ad ogni modo, ASF è un programma con interfaccia a riga di comando, ovvero privo di un'interfaccia grafica (o GUI), contrariamente quindi ai programmi che generalmente usi. ASF è stato principalmente pensato per essere eseguito su server, quindi si comporta più come un servizio (o daemon) che come un'applicazione classica.

Tuttavia ciò non significa che tu non possa usarlo sul tuo PC o che farlo funzionare sia più complicato del solito, per niente. ASF è un programma standalone che non necessita di installazione, subito pronto per essere utilizzato, ma che necessita d'essere configurato per poter essere di qualche uso. È infatti la sua configurazione che dirà ad ASF quello che deve fare una volta avviato. Se lo avvii senza averlo configurato, ASF semplicemente non farà nulla.

* * *

## Installazione per ogni OS

Il linea generale, ecco cosa faremo nei prossimi minuti:

- Installare i **[prerequisiti per .NET Core](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)**.
- Scaricare **[l'ultima versione di ASF](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** specifica per il tuo sistema operativo.
- Estrarre l'archivio in una nuova cartella (ed usare il comando `chmod +x ArchiSteamFarm` se sei su Linux/OS X).
- **[Configurare ASF](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)**.
- Avviare ASF e vederlo all'opera.

Suona piuttosto semplice, vero? Allora iniziamo.

* * *

### Prerequisiti per .NET Core

Il primo passo consiste nell'assicurarsi che il proprio OS possa eseguire ASF correttamente. ASF è scritto in C# ed è basato su .NET Core, quindi potrebbe richiedere librerie native che non sono ancora disponibili sulla tua piattaforma. In base al tuo OS (Windows, Linux o OS X) ci saranno requisiti differenti, elencati nel documento **[Prerequisiti per .NET Core](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)** che dovresti proprio leggere. È la guida di riferimento che dovresti usare, ma per rendere le cose più semplici ed evitare di farti leggere troppo, sono anche riassunte in dettaglio qui sotto.

È perfettamente normale che alcuni, o anche tutti i prerequisiti siano già presenti nel tuo sistema, magari perché installati da applicazioni di terze parti che stai usando. Nonostante ciò, devi assicurarti che sia davvero questo il caso eseguendo l'installazione di ogni requisito per il tuo OS - senza di essi, ASF non funzionerà.

Tieni a mente che non hai bisogno di fare altro per le build specifiche per il vostro OS, come installare il .NET Core SDK, dacché i pacchetti specifici per ogni OS includono tutto ciò di cui si ha bisogno. Hai solo bisogno dei prerequisiti (o dipendenze) per .NET Core per far funzionare il runtime .NET Core incluso in ASF.

#### **[Windows](https://docs.microsoft.com/en-us/dotnet/core/windows-prerequisites?tabs=netcore2x)**:

- **[Microsoft Visual C++ 2015 Redistributable Update 3 RC](https://www.microsoft.com/en-us/download/details.aspx?id=52685)** (x64 per Windows a 64 bit, x86 per Windows a 32 bit)
- È fortemente consigliato assicurarsi gli aggiornamenti di Windows siano tutti installati. Hai assolutamente bisogno degli aggiornamenti **[KB2533623](https://support.microsoft.com/en-us/help/2533623/microsoft-security-advisory-insecure-library-loading-could-allow-remot)** e **[KB2999226](https://support.microsoft.com/en-us/help/2999226/update-for-universal-c-runtime-in-windows)**, ma potrebbero esserne richiesti anche degli altri. Se mantieni il tuo Windows aggiornato, saranno già installati. Assicurati di soddisfare tali requisiti prima di installare il pacchetto Visual C++.

#### **[Linux](https://docs.microsoft.com/en-us/dotnet/core/linux-prerequisites?tabs=netcore2x)**:

Il nome del pacchetto varia in base alla distribuzione di Linux che stai usando, ma elencheremo quelli che sono i più comuni. Li puoi ottenere dal tuo gestore di pacchetti nativo in base all'OS che usi (come `apt` per Debian, o `yum` per CentOS).

- libcurl3 (libcurl)
- libicu (la versione più recente disponibile per la tua distribuzione, ad esempio `libicu57` per Debian 9)
- libkrb5-3 (krb5-libs)
- liblttng-ust0 (lttng-ust)
- libssl1.0.2 (libssl, openssl-libs, compat-openssl10, la versione 1.0.X più recente la tua distribuzione)
- zlib1g (zlib)

Almeno qualcuno di questi dovrebbe essere già presente nativamente sul tuo sistema (come il zlib1g, che ormai è richiesto per quasi ogni distro di Linux).

#### **[OS X](https://docs.microsoft.com/en-us/dotnet/core/macos-prerequisites?tabs=netcore2x)**:

- Per ora nessuno.

* * *

### Download

Ora che abbiamo i requisiti necessari, il prossimo passo sarà scaricare **[l'ultima versione di ASF](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)**. ASF è disponibile in molte varianti, ma tu sei interessato al pacchetto corrispondente al tuo sistema sperativo ed architettura. Ad esempio, se stai usando `Win`dows a `64` bit, allora ti servirà il pacchetto `ASF-win-x64`. Per maggiori informazioni sulle versioni disponibili, consulta la sezione **[compatibilità](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility)**. ASF è anche in grado di funzionare su sistemi operativi per i quali non è disponibile un pacchetto specifico, come **Windows a 32 bit**; se questo è il tuo caso, vai direttamente alla sezione **[setup generico](#generic-setup)**.

![Assets](https://i.imgur.com/Ym2xPE5.png)

Una volta scaricato il pacchetto giusto per te ed avendolo estratto (possibilmente usando **[7-zip](https://www.7-zip.org)**), avrai un gran miscuglio di file e cartelle. Non temere, puliremo tutto a breve.

Se stai usando Linux od OS X, non dimenticare di usare il comando `chmod +x ArchiSteamFarm`, dato che i permessi non vengono impostati nel file zip. Ciò va fatto solo dopo aver estratto l'archivio per la prima volta.

Assicurati di estrarre ASF **in una cartella ad esso destinata** e non in una che stai già usando per qualcos'altro - gli aggiornamenti automatici di ASF elimineranno ogni altro file già presente e non necessario al programma, il che significa perdere ogni file salvato nella cartella di ASF. Se hai script aggiuntivi o file che vuoi usare con ASF, mettili in una cartella superiore.

Una struttura ideale è simile a questa:

    C:\ASF (dove metterai le tue cose)
        ├── Collegamento ad ASF.lnk (opzionale)
        ├── Collegamento a Config.lnk (opzionale)
        ├── Comandi.txt (opzionale)
        ├── IlMioScript.bat (opzionale)
        ├── ... (opzionalmente, ogni altro file di cui hai bisogno)
        └── Core (chiamala come preferisci, estrarrai l'archivio qui)
             ├── ArchiSteamFarm.dll
             ├── config
             └── (...)
    

Tale è la struttura che raccomandiamo, così che tu non debba passare attraverso lo sproposito di file e sottocartelle inclusi in ASF, dacché per utilizzarlo hai bisogno giusto di un collegamento al file eseguibile e alla cartella config.

Bene, ora prepareremo la cartella di ASF per l'utilizzo. Se lo preferisci, puoi anche passare saltare questa parte, poiché organizzare come si deve la struttura di cartelle e file per ASF non è necessario, ma ti faciliterebbe la vita.

Apri la cartella di ASF e trova il file eseguibile principale, che su Windows sarà `ArchiSteamFarm.exe`, mentre su Linux/OS X sarà `ArchiSteamFarm`. Fai click destro su di esso e seleziona "Copia". Ora vai nella cartella in cui vorresti avere il collegamento ad ASF (come il tuo Desktop), fai click destro in un punto vuoto e seleziona "Incolla collegamento". Puoi anche rinominare il collegamento se lo vuoi, chiamandolo ad esempio "ASF". Ora fai anche la stessa cosa con la cartella `config` che trovi sempre nella cartella dell'eseguibile di ASF.

Dopo aver messo un po' d'ordine, avrai una comoda struttura di file e cartelle come la seguente:

![Structure](https://i.imgur.com/k85csaZ.png)

Ciò ti permetterà di accedere con facilità all'eseguibile di ASF oltre che ai file di configurazione. Nel mio caso ho deciso di usare la struttura menzionata all'inizio, cosicché i file di ASF risiedono direttamente nella cartella "Core". Puoi adattare tale struttura come meglio preferisci, avendo per esempio i collegamenti ad ASF e config sul Desktop, e la cartella dell'applicazione in `C:\ASF`.

Gli utenti Linux/OS X dovrebbero fare la stessa cosa, usando magari l'eccellente sistema di link simbolici con `ln -s`.

* * *

### Configurazione

Siamo ora pronti per affrontare l'ultima tappa, la configurazione. È di gran lunga la fase più complessa, poiché involve un gran numero di informazioni con le quali non hai ancora familiarità. Per questo ti forniremo diversi esempi e spiegazioni semplici da seguire.

Innanzitutto, è presente la pagina **[configurazione](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** che spiega **tutto quel che c'è da sapere** sul processo di configurazione; tuttavia offre molte più informazioni di cui al momento abbiamo bisogno. Perciò, ti spiegheremo come ottenere le informazioni che stai cercando.

La configurazione di ASF può essere eseguita in due modi - sia con il nostro generatore web, sia manualmente. Ciò è spiegato nel dettaglio nella sezione **[configurazione](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)**, quindi fai riferimento ad essa se vuoi informazioni più dettagliate. Essendo molto più semplice, utilizzeremo il configuratore web.

Apri la pagina del nostro **[generatore di configurazioni web](https://justarchinet.github.io/ASF-WebConfigGenerator)** con il browser che preferisci, avrai bisogno di abilitare javascript in caso tu lo abbia disabilitato manualmente. Raccomandiamo Chrome o Firefox, ma dovrebbe funzionare con i browser più usati.

Una volta caricata la pagina, clicca su "Bot". Dovresti avere davanti una pagina simile a questa:

![Bot tab](https://i.imgur.com/aF3k8Rg.png)

Se per qualche motivo la versione di ASF che hai appena scaricato è più vecchia di quella preselezionata dal generatore di configurazioni, seleziona la versione corretta dal menu a discesa. Ciò può accadere dacché il configuratore può essere usato anche per versioni più recenti di ASF, le "pre-release", che però non sono ancora ritenute stabili. Tu hai invece scaricato la versione stabile più recente di ASF, la cui affidabilità è stata comprovata.

Start from putting name for your bot into the field highlighted as red. This can be any name you'd like to use, such as your nickname, account name, a number, or anything else. There is only one word that you can't use, `ASF`, as that keyword is reserved for global config file. In addition to that your bot name can't start with a dot (ASF intentionally ignores those files). We also recommend that you avoid using spaces, you can use `_` as a word separator if needed.

After you decided about your name, change `Enabled` switch to be on, this defines whether your bot is supposed to be started by ASF automatically after launch (of the program).

Now you can decide upon two things:

- You can put your login in `SteamLogin` field and your password in `SteamPassword` field
- Or you can leave them empty

Doing the first thing will allow ASF to automatically use your account credentials during startup, so you won't need to input them manually each time ASF needs them. You can however decide to omit them, in which case they're not being saved, so ASF won't be able to automatically start without your help and you'll need to input them during runtime.

ASF requires your login credentials because it includes its own implementation of Steam client and needs the same details to log in as the one that you use yourself. Your login credentials are not saved anywhere but on your PC in ASF `config` directory only, our web config generator is client-based which means that the code is run locally in your browser to generate valid ASF configs, without details you're inputting ever leaving your PC in the first place, so there is no need to worry about any possible sensitive data leak. Still, if you for whatever reason don't want to put your credentials there, we understand that, and you can put them manually later in generated files, or omit them entirely and put them only in ASF command prompt. More on security matter can be found in **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** section.

You can also decide to leave just one field empty, such as `SteamPassword`, ASF will then be able to use your login automatically, but will still ask for password (similar to Steam Client). If you're using Steam parental to unlock the account, you'll need to put it into `SteamParentalCode` field.

After the decision and optional details, your web page will now look similar to the one below:

![Bot tab 2](https://i.imgur.com/yf54Ouc.png)

You can now hit "download" button and our web config generator will generate new `json` file based on your chosen name. Save that file into `config` directory of ASF. You can use previously-created `config` shortcut, or find `config` directory manually, directly in ASF file structure.

Your `config` directory will now look like this:

![Structure 2](https://i.imgur.com/crWdjcp.png)

Congratulations! You've just finished the very basic ASF bot configuration. We'll extend this shortly, for now this is everything that you need.

* * *

### Running ASF

You're now ready to launch the program for the first time. Simply double-click ASF shortcut, or `ArchiSteamFarm(.exe)` binary in ASF directory.

After doing so, assuming you installed all required dependencies in the first step, ASF should launch properly, notice your first bot (if you didn't forget to put generated config in `config` directory), and attempt to log in:

![ASF](https://i.imgur.com/u5hrSFz.png)

If you supplied `SteamLogin` and `SteamPassword` for ASF to use, you'll be asked for your SteamGuard token only (e-mail, 2FA or none, depending on your Steam settings). If you didn't, you'll also be asked for your Steam login and password.

Now is a good time to review our **[privacy policy](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Statistics#current-privacy-policy)** section if you're concerned about what will happen next, as stated by ASF itself.

After passing through initial login gate, assuming your details are correct, you'll successfully log in, and ASF will start idling using default settings that you didn't change as of now:

![ASF 2](https://i.imgur.com/Cb7DBl4.png)

This proves that ASF is now successfully doing its job on your account, so you can now minimize the program and do something else. After enough of time (depending on **[performance](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)**), you'll see Steam trading cards slowly being dropped. Of course, for that to happen you must have valid games to idle, showing as "you can get X more card drops from playing this game" on your **[badges page](https://steamcommunity.com/my/badges)** - if there are no games to idle, then ASF will state that there is nothing to do, as stated in our **[FAQ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ#so-how-it-exactly-works)**.

This concludes our very basic setting up guide. You can now decide whether you want to configure ASF further, or let it do its job in default settings. We'll cover a few more basic details, then leave you entire wiki for discovery.

* * *

### Extended configuration

#### Idling several accounts at once

ASF supports idling more than one account at a time, which is its primary function. You can add more accounts to ASF by generating more bot config files, in exactly the same way as you've generated your first one just a few minutes ago. You need to ensure only two things:

- Unique bot name, if you already have your first bot named "MainAccount", you can't have another one with the same name.
- Valid login details, such as `SteamLogin`, `SteamPassword` and `SteamParentalCode` (if using Steam parental settings)

In other words, simply jump to configuration again and do exactly the same, just for your second or third account. Remember to use unique names for all of your bots.

* * *

#### Changing settings

You change existing settings in exactly the same way - by generating a new config file. If you didn't close our web config generator yet, click on "toggle advanced settings" and see what is there for you to discover. For this tutorial we'll change `CustomGamePlayedWhileFarming` setting, which allows you to set custom name being displayed when ASF is idling, instead of showing actual game.

So let's do that, if you run ASF and start idling, in default settings you'll see that your Steam account is in-game now:

![Steam](https://i.imgur.com/1VCDrGC.png)

Let's change that then. Toggle advanced settings in web config generator and find `CustomGamePlayedWhileFarming`. Once you do that, put your own custom text there that you want to display, such as "Idling cards":

![Bot tab 3](https://i.imgur.com/gHqdEqb.png)

Now download the new config file in exactly the same way, then **overwrite** your old config file with new one. You can also delete your old config file and put new one in its place of course.

Once you do that and start ASF again, you'll notice that ASF now displays your custom text in previous place:

![Steam 2](https://i.imgur.com/vZg0G8P.png)

This confirms that you've successfully edited your config. In exactly the same way you can change global ASF properties, by switching from bot tab to "ASF" tab, downloading generated `ASF.json` config file and putting it in your `config` directory.

* * *

#### Using ASF-ui

ASF is a console app and doesn't include a graphical user interface. However, we're actively working on **[ASF-ui](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#asf-ui)** frontend to our IPC interface, which can be a very decent and user-friendly way to access various ASF features.

In order to use ASF-ui, you should ensure that you set up `IPC` and `SteamOwnerID` global configuration properties (ASF tab).

For `SteamOwnerID`, you need to input unique Steam identificator in 64-bit form of your account. You can look it up in various different ways, we'll use **[SteamRep](https://steamrep.com)** for that purpose. Open the website, locate sign in through Steam button in top right corner, then log in. Afterwards, in the same place, click on your avatar, and look up `steamID64` field on your profile.

![SteamRep](https://i.imgur.com/RUuJ63i.png)

For my account, this is `76561198006963719` number. You'll have a similar one, also starting from `7656`. Copy it.

Now navigate once again to our web config generator and input that number as SteamOwnerID.

![SteamOwnerID](https://i.imgur.com/V6jslfQ.png)

You need to do only one more thing, toggle advanced settings, find `IPC` option, and enable it.

![IPC](https://i.imgur.com/NhujZCN.png)

Now you can download your ASF config and put it in your `config` directory, as usual. Afterwards, launch ASF again, and you should be able to confirm that it properly started IPC interface:

![IPC 2](https://i.imgur.com/ZmkO8pk.png)

If you did everything properly, you'll now be able to access ASF's IPC interface under **[this](http://127.0.0.1:1242)** link, as long as ASF is running. You can use ASF-ui for various purposes, e.g. sending **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. Feel free to take a look around in order to find out all ASF-ui functionalities.

![ASF-ui](https://raw.githubusercontent.com/JustArchiNET/ASF-ui/master/.github/previews/bots.png)

Please note that ASF-ui is currently in preview state and not everything is available/working yet, but it's more than enough for simple ASF usage.

* * *

### Summary

You've successfully set up ASF to use your Steam accounts and you've already customized it to your liking a little. If you followed our entire guide, then you even managed to send a simple command through our ASF-ui interface. Now is a good time to read our entire **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** section in order to learn what all those different settings you've seen in advanced tab actually do, and what ASF can offer. If you've stumbled upon some issue or you have some generic question, read **[FAQ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ)** instead which should cover all, or at least majority of questions that you might have. If you want to learn everything about ASF and how it can make your life easier, head over to the rest of **[our wiki](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Home)**. Have fun!

* * *

## Generic setup

This setup is for advanced users that want to set up ASF to run in **[generic](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#generic)** variant. It's not recommended for people that can use **[OS-specific setup](#os-specific-setup)**.

You want to use generic variant mainly in three situations (but of course you can use it regardless):

- When you're using OS that we don't build OS-specific package for (such as 32-bit Windows)
- When you already have .NET Core Runtime/SDK, or want to install and use one
- When you want to minimize ASF structure size by handling runtime requirements yourself

However, keep in mind that you're in charge of .NET Core runtime in this case. This means that if your .NET Core SDK (runtime) is unavailable, outdated or broken, ASF won't work. This is why we don't recommend this setup for casual users, since you now need to ensure that your .NET Core SDK (runtime) matches ASF requirements and can run ASF, as opposed to **us** ensuring that our .NET Core runtime bundled with ASF can do so.

For generic package, you can follow entire OS-specific guide above, with two small changes. In addition to installing .NET Core prerequisites, you also want to install .NET Core SDK, and instead of having OS-specific `ArchiSteamFarm(.exe)` executable file, you now have a generic `ArchiSteamFarm.dll` binary only. Everything else is exactly the same.

With extra steps:

- Installare i **[prerequisiti per .NET Core](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)**.
- Install **[.NET Core SDK](https://www.microsoft.com/net/download)** (or at least runtime) appropriate for your OS. You most likely want to use an installer. Refer to **[runtime requirements](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)** if you're not sure which version to install.
- Download **[latest ASF release](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** in generic variant.
- Extract the archive into new location (and `chmod +x ArchiSteamFarm.sh` if you're on Linux/OS X).
- **[Configurare ASF](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)**.
- Launch ASF by either using a helper script or executing `dotnet /path/to/ArchiSteamFarm.dll` manually from your favourite shell.

Helper scripts (such as `ArchiSteamFarm.cmd` for Windows and `ArchiSteamFarm.sh` for Linux/OS X) are located next to `ArchiSteamFarm.dll` binary - those are included in generic variant only. You can use them if you don't want to execute `dotnet` command manually. You can also make a shortcut to those scripts like showed above, since they're supposed to provide binary replacement in a script way. Obviously helper scripts won't work if you didn't install .NET Core SDK and you don't have `dotnet` executable available in your `PATH`. Helper scripts are entirely optional to use, you can always `dotnet /path/to/ArchiSteamFarm.dll` manually.