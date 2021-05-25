# Compilazione

La compilazione è il processo di creazione del file eseguibile. Questo è quanto vuoi fare se vuoi aggiungere le tue modifiche ad ASF, o se per qualche motivo non ti fidi dei file eseguibili forniti nelle **[versioni](https://github.com/JustArchiNET/ArchiSteamFarm/releases)** ufficiali. Se sei un utente e non uno sviluppatore, molto probabilmente vorrai usare i binari già precompilati, ma se vuoi usare i tuoi, o vuoi imparare qualcosa di nuovo, continua a leggere.

ASF è compilabile su ogni piattaforma correntemente supportata, finché hai tutti gli strumenti per farlo.

---

## SDK .NET Core

Indipendentemente dalla piattaforma, necessiterai del SDK .NET Core completa (non solo in esecuzione) per compilare ASF. Puoi trovare le istruzioni di installazione sulla **[pagina di installazione di .NET Core](https://dotnet.microsoft.com/download)**. Devi installare la versione appropriata del SDK di .NET Core per il tuo OS. Dopo l'installazione riuscita, il comando `dotnet` dovrebbe esser funzionante e operativo. Puoi verificare che funzioni con `dotnet --info`. Assicurati anche che l'SDK di .NET Core corrisponda ai **[requisiti d'esecuzione](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)** di ASF.

---

## Compilazione

Presumendo che tu abbia l'SDK di .NET Core operativa e nella versione appropriata, naviga semplicemente alla cartella sorgente di ASF (repository clonata o scaricata e decompressa di ASF) ed esegui:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net5.0" -o "out/generic"
```

Se usi LINUS/OS X, puoi invece usare lo script `cc.sh` che farà lo stesso, in un modo un po' più complesso.

Se la compilazione è terminata correttamente, puoi trovare il tuo ASF in forma di `sorgente` nella cartella `out/generic`. Ha pari valore alla build ufficiale `generica` di ASF, ma ha `UpdateChannel` e `UpdatePeriod` di `0` forzati, il che è appropriato per le build personali.

### Specifico all'OS

Puoi anche generare pacchetti di .NET Core specifici per l'OS se hai un'esigenza specifica. In generale non dovresti farlo perché hai appena compilato il tipo `generico` che puoi eseguire con l'esecuzione del .NET Core già installato usato in primo luogo per la compilazione, ma se volessi farlo:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net5.0" -o "out/linux-x64" -r "linux-x64"
```

Ovviamente, sostituisci `linux-64` con l'architettura OS a cui vuoi destinarlo, come `win-x64`. Questa build avrà inoltre gli aggiornamenti disabilitati.

### Framework .NET

In un caso molto raro, qualora volessi costruire il pacchetto `generic-netf`, puoi modificare il framework di destinazione da `net5.0` a `net48`. Tieni a mente che necessiterai del pacchetto sviluppatore appropriato del **[Framework di .NET](https://dotnet.microsoft.com/download/visual-studio-sdks)** per compilare la variante `netf`, oltre all'SDK di .NET Core, quindi il seguente funzionerà solo su Windows:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net48" -o "out/generic-netf"
```

In caso di incapacità di installare il Framework di .NET o persino l'SDK stesso di .NET Core (es. per la costruzione su `linux-x86` con `mono`), potrai direttamente chiamare `msbuild`. Dovrai anche specificare manualmente `ASFNetFramework`, poiché ASF di default disabilita la build `netf` su piattaforme non Windows:

```shell
msbuild /m /r /t:Publish /p:Configuration=Release /p:TargetFramework=net48 /p:PublishDir=out/generic-netf /p:ASFNetFramework=true ArchiSteamFarm
```

---

## Sviluppo

Se vorresti modificare il codice di ASF, potrai usare l'IDE compatibile con .NET Core per tale scopo, sebbene anche ciò sia opzionale, poiché puoi anche modificare con un blocco note e compilare con il comando `dotnet` descritto sopra. Tuttavia, per Windows consigliamo l'**[ultimo Visual Studio](https://visualstudio.microsoft.com/downloads)** (la versione gratuita della community è più che sufficiente).

Se vorresti lavorare con il codice di ASF su Linux/OS X, invece, consigliamo l'**[ultima versione di Visual Studio Code](https://code.visualstudio.com/download)**. Non è ricca come il classico Visual Studio, ma va abbastanza bene.

Ovviamente tutti i suggerimenti sopra sono solo consigli, puoi usare ciò che vuoi, si riduce comunque al comando `dotnet build`. Usiamo **[JetBrains Rider](https://www.jetbrains.com/rider)** per lo sviluppo di ASF, sebbene non sia una soluzione gratuita.

---

## Tag

Il ramo `main` non è garantito in uno stato che consenta una compilazione di successo o un'esecuzione impeccabile di ASF in primo luogo, poiché è il suo ramo di sviluppo, proprio come dichiarato nel nostro **[ciclo di rilascio](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle)**. Se vuoi compilare o riferirti ad ASF dalla sorgente, allora dovresti usare il **[tag](https://github.com/JustArchiNET/ArchiSteamFarm/tags)** appropriato per tale scopo, garantendo almeno una compilazione di successo, e molto probabilmente anche l'esecuzione impeccabile (se la build era segnata come versione stabile). Per controllare la "salute" corrente dell'albero, puoi usare le nostre integrazioni continue - **[GitHub](https://github.com/JustArchiNET/ArchiSteamFarm/actions)** o **[AppVeyor](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)**.

---

## Versioni ufficiali

Le versioni ufficiali di ASF sono compilate da **[GitHub](https://github.com/JustArchiNET/ArchiSteamFarm/actions)** su Windows, con l'ultima SDK di .NET Core che corrisponda ai **[requisiti d'esecuzione](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)** di ASF. Dopo aver superato i testi, tutti i pacchetti sono distribuiti come versione, anche su GitHub. Questo garantisce anche la trasparenza, dato che GitHub usa sempre sorgenti pubbliche ufficiali per tutte le build e poiché puoi comparare le somme di controllo degli artefatti di GitHub con le risorse di rilascio di GitHub. Gli sviluppatori di ASF non compilano o pubblicano loro stessi le build, eccetto per il processo di sviluppo privato e il debug.