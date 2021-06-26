# Compilazione

La compilazione è il processo di creazione del file eseguibile. Questo è quanto vuoi fare se vuoi aggiungere le tue modifiche ad ASF, o se per qualche motivo non ti fidi dei file eseguibili forniti nelle **[versioni](https://github.com/JustArchiNET/ArchiSteamFarm/releases)** ufficiali. Se sei un utente e non uno sviluppatore, molto probabilmente vorrai usare i binari già precompilati, ma se vuoi usare i tuoi, o vuoi imparare qualcosa di nuovo, continua a leggere.

ASF è compilabile su ogni piattaforma correntemente supportata, finché hai tutti gli strumenti per farlo.

---

## .NET SDK

Regardless of platform, you need full .NET SDK (not just runtime) in order to compile ASF. Installation instructions can be found on **[.NET download page](https://dotnet.microsoft.com/download)**. You need to install appropriate .NET SDK version for your OS. Dopo l'installazione riuscita, il comando `dotnet` dovrebbe esser funzionante e operativo. Puoi verificare che funzioni con `dotnet --info`. Also ensure that your .NET SDK matches ASF **[runtime requirements](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)**.

---

## Compilazione

Assuming you have .NET SDK operative and in appropriate version, simply navigate to source ASF directory (cloned or downloaded and unpacked ASF repo) and execute:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net5.0" -o "out/generic"
```

Se usi LINUS/OS X, puoi invece usare lo script `cc.sh` che farà lo stesso, in un modo un po' più complesso.

Se la compilazione è terminata correttamente, puoi trovare il tuo ASF in forma di `sorgente` nella cartella `out/generic`. Ha pari valore alla build ufficiale `generica` di ASF, ma ha `UpdateChannel` e `UpdatePeriod` di `0` forzati, il che è appropriato per le build personali.

### Specifico all'OS

You can also generate OS-specific .NET package if you have a specific need. In general you shouldn't do that because you've just compiled `generic` flavour that you can run with your already-installed .NET runtime that you've used for the compilation in the first place, but just in case you want to:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net5.0" -o "out/linux-x64" -r "linux-x64"
```

Ovviamente, sostituisci `linux-64` con l'architettura OS a cui vuoi destinarlo, come `win-x64`. Questa build avrà inoltre gli aggiornamenti disabilitati.

### Framework .NET

In un caso molto raro, qualora volessi costruire il pacchetto `generic-netf`, puoi modificare il framework di destinazione da `net5.0` a `net48`. Keep in mind that you'll need appropriate **[.NET Framework](https://dotnet.microsoft.com/download/visual-studio-sdks)** developer pack for compiling `netf` variant, in addition to .NET SDK, so the below will work only on Windows:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net48" -o "out/generic-netf"
```

In case of being unable to install .NET Framework or even .NET SDK itself (e.g. because of building on `linux-x86` with `mono`), you can call `msbuild` directly. Dovrai anche specificare manualmente `ASFNetFramework`, poiché ASF di default disabilita la build `netf` su piattaforme non Windows:

```shell
msbuild /m /r /t:Publish /p:Configuration=Release /p:TargetFramework=net48 /p:PublishDir=out/generic-netf /p:ASFNetFramework=true ArchiSteamFarm
```

---

## Sviluppo

If you'd like to edit ASF code, you can use any .NET compatible IDE for that purpose, although even that is optional, since you can as well edit with a notepad and compile with `dotnet` command described above. Tuttavia, per Windows consigliamo l'**[ultimo Visual Studio](https://visualstudio.microsoft.com/downloads)** (la versione gratuita della community è più che sufficiente).

Se vorresti lavorare con il codice di ASF su Linux/OS X, invece, consigliamo l'**[ultima versione di Visual Studio Code](https://code.visualstudio.com/download)**. Non è ricca come il classico Visual Studio, ma va abbastanza bene.

Ovviamente tutti i suggerimenti sopra sono solo consigli, puoi usare ciò che vuoi, si riduce comunque al comando `dotnet build`. Usiamo **[JetBrains Rider](https://www.jetbrains.com/rider)** per lo sviluppo di ASF, sebbene non sia una soluzione gratuita.

---

## Tag

Il ramo `main` non è garantito in uno stato che consenta una compilazione di successo o un'esecuzione impeccabile di ASF in primo luogo, poiché è il suo ramo di sviluppo, proprio come dichiarato nel nostro **[ciclo di rilascio](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle)**. Se vuoi compilare o riferirti ad ASF dalla sorgente, allora dovresti usare il **[tag](https://github.com/JustArchiNET/ArchiSteamFarm/tags)** appropriato per tale scopo, garantendo almeno una compilazione di successo, e molto probabilmente anche l'esecuzione impeccabile (se la build era segnata come versione stabile). Per controllare la "salute" corrente dell'albero, puoi usare le nostre integrazioni continue - **[GitHub](https://github.com/JustArchiNET/ArchiSteamFarm/actions)** o **[AppVeyor](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)**.

---

## Versioni ufficiali

Official ASF releases are compiled by **[GitHub](https://github.com/JustArchiNET/ArchiSteamFarm/actions)** on Windows, with latest .NET SDK that matches ASF **[runtime requirements](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)**. Dopo aver superato i testi, tutti i pacchetti sono distribuiti come versione, anche su GitHub. Questo garantisce anche la trasparenza, dato che GitHub usa sempre sorgenti pubbliche ufficiali per tutte le build e poiché puoi comparare le somme di controllo degli artefatti di GitHub con le risorse di rilascio di GitHub. Gli sviluppatori di ASF non compilano o pubblicano loro stessi le build, eccetto per il processo di sviluppo privato e il debug.