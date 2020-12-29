# Podešavanje

Ako ste ovdje prvi put, dobro došli! Srećni smo da vidimo još nekog ko je zainteresovan za naš projekat, ali zapamtite da sa velikom moći dolazi velika odgovornost - ASF ima mogućnost da kontroliše mnogo stvari na Steam-u, ali samo ako **pazite kako da ga naučite**. Strma je linija učenja, a mi očekujemo od vas da pročitate wiki-u zbog toga, koja objašnjava u detalju kako sve radi.

Ako ste još ovdje znači da ste izdržali tekst iznad, što je lijepo. Ako ste je preskočili, onda će vam uskoro biti **[loše](https://www.youtube.com/watch?v=WJgt6m6njVw)**... U svakom slučaju, ASF je konsolska aplikacija, što znači da program nema GUI interfejs na koji ste možda navikli. ASF je najviše namijenjen da se koristi na serveru, pa zbog toga izgleda kao servis (daemon) a ne kao desktop aplikacija.

Ovo ipak ne znači da ga ne možete koristiti na vašem PC-u ili na nečem više komplikovanom nego obično. ASF je zaseban program koji ne zahtijeva instalaciju, i odmah radi ali zahtijeva konfiguraciju da bi vam bio od koristi. Konfiguracija kazuje ASF-u šta u stvari treba da radi nakon što ga pokrenete. Ako ga pokrenete bez konfiguracije, ASF onda neće raditi ništa, tako jednostavno.

* * *

## OS-specifično postavljanje

Opšte kazano, ovo ćete raditi nekoliko sledećih minuta:

- instalirati **[.NET Core prerequisites](#net-core-prerequisites)**,
- preuzmite **[poslednje ASF izdanje](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** u odgovarajućoj OS-specifičnoj varijanti,
- ekstraktujte arhivu u novoj lokaciji (i `chmod +x ArchiSteamFarm` ako ste na Linux-u/OS X-u),
- **[Konfigurišete ASF](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)**.
- pokrenite ASF i vidite magiju.

Zvuči jednostavno, zar ne? Pa počnimo.

* * *

### .NET Core prerequisites

Prvo morate biti sigurni da ASF može biti pokrenut na vašem OS-u. ASF je napisan u C#, baziran na .NET Core i možda će zahtijevati dodatne nativne biblioteke koje još nisu prisutne na vašoj platformi. Razlika postoji u tome da li koristite Windows, Linux ili OS X, i zbog toga će imati različite zahtjeve, ali u svakom slučaju, svi su navedeni u **[.NET Core prerequisites](https://docs.microsoft.com/dotnet/core/install)** dokumentu kojeg trebate pratiti. Ovo je naš materijal za podsjećanje koji treba biti korišćen, ali zbog jednostavnosti mi smo naveli sve pakete ispod, pa ne morate čitati čitav dokument.

Normalno je da neki (ili svi) zahtjevi već postoje na vašem sistemu zbog toga što ih je neki treći softver, koji već koristite, instalirao. Ipak, budite sigurni da stvarno imate potrebne zahtjeve na vašem OS-u - bez tih zahtjeva ASF se uopšte neće pokrenuti.

Zapamtite da, osim ovog, ne morate ništa drugo raditi na vašem OS-u, posebno ne instalirati .NET Core SDK ili runtime, jer OS paket već sadrži to sve. Potrebni su vam samo .NET Core prerequisites (zahtjevi) da bi pokrenuli .NET Core runtime koje je u ASF-u.

#### **[Windows](https://docs.microsoft.com/dotnet/core/install/windows)**:

- **[Microsoft Visual C++ 2015 Redistributable Update](https://www.microsoft.com/en-us/download/details.aspx?id=53587)** (x64 za 64-bit Windows, x86 za 32-bit Windows)
- Veoma je preporučljivo da budete sigurni da su sva postojeća Windows ažurirana instalirana. Morate minimalno imati **[KB2533623](https://support.microsoft.com/en-us/help/2533623/microsoft-security-advisory-insecure-library-loading-could-allow-remot)** i **[KB2999226](https://support.microsoft.com/en-us/help/2999226/update-for-universal-c-runtime-in-windows)**, ali možda i još neko ažuriranje ako bude potrebno. Sve su već instalirane ako je Windows ažuriran do kraja. Budite sigurni da ste sve ovo uradili prije nego što instalirate Visual C++.

#### **[Linux](https://docs.microsoft.com/dotnet/core/install/linux)**:

Imena paketa zavise od Linux distribucije koju koristite, mi smo naveli najčešće varijacije. Možete ih instalirati sa nativnim paket menađerom na vašoj distribuciji (kao što je `apt` za Debian ili `yum` za CentOS).

- `libc6` (`libc`)
- `libgcc1` (`libgcc`)
- `libicu` (`icu-libs`, poslednju verziju za vašu distribuciju, npr. `libicu67`)
- `libgssapi-krb5-2` (`libkrb5-3`, `krb5-libs`)
- `libssl1.1` (`libssl`, `openssl-libs`, poslednju verziju na vašoj distribuciji, `1.1.X` or `1.0.X`)
- `libstdc++6` (`libstdc++`, u verziji `5.0` ili većoj)
- `zlib1g` (`zlib`)

Većina, ako ne i sve, bi trebalo da su već instalirane na vašem sistemi. Minimalna instalacija Debian stable zahtijeva samo `libicu63` od svih ovih.

#### **[OS X](https://docs.microsoft.com/dotnet/core/install/macos)**:

- Ništa za sada, ali trebate imati poslednju verziju OS X instaliranu, najmanje 10.13+

* * *

### Preuzimanje

Sada kada imate sve zahtjeve ispunjene, sledeći korak je da preuzmete **[poslednje ASF izdanje](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)**. ASF je dostupan u raznim varijantama, ali vama je potreban paket koji je isti kao vaš operativni sistem i vaša arhitektura. Npr. ako koristite `64`-bit `Win`dows, onda izaberite `ASF-win-x64` paket. Za više informacija o dostupnim varijacijama, posjetite **[kompatabilnost](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility)** sekciju. ASF je moguće pokrenuti i na OS-ovima za koje ne pravimo OS namijenjeni paket, kao što je **32-bit Windows**, za njih izaberite **[opšta podešavanja](#generic-setup)**.

![Assets](https://i.imgur.com/Ym2xPE5.png)

Nakon preuzimanja, ekstraktujte zip fajl u novom folderu. Mi predlažemo koršćenje **[7-zip](https://www.7-zip.org)**, ali svi standardni programi kao `unzip` na Linux/OS X-u će takođe raditi bez problema.

If you're using Linux/OS X, don't forget to `chmod +x ArchiSteamFarm` in the extracted folder, since permissions are not automatically set in the zip file. This has to be done only once after initial unpack.

Be advised to unpack ASF to **its own directory** and not to any existing directory you're already using for something else - ASF's auto-updates feature will delete all old and unrelated files when upgrading, which may lead to you losing anything unrelated you put in ASF directory. If you have any extra scripts or files that you want to use with ASF, put them in one folder above.

An example structure would look like this:

```text
C:\ASF (gdje stavljate vaše stvari)
    ├── ASF shortcut.lnk (opcionalno)
    ├── Config shortcut.lnk (opcionalno)
    ├── Commands.txt (opcionalno)
    ├── MyExtraScript.bat (opcionalno)
    ├── (...) (bilo koji fajl koji vi izaberete, opcionalno)
    └── Core (samo za ASF, ovdje ekstrakujete arhivu)
         ├── ArchiSteamFarm(.exe)
         ├── config
         ├── logs
         ├── plugins
         └── (...)
```

* * *

### Konfiguracija

We're now ready to do the very last step, the configuration. This is by far the most complicated step, since it involves a lot of new information you're not familiar with yet, so we'll try to provide some easy to understand examples and simplified explanation here.

First and foremost, there is **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** page that explains **everything** that relates to configuration, but it's a massive amount of new information, a lot of which we don't need to know right away. Instead, we'll teach you how to get the information you're actually looking for.

ASF configuration can be done in two ways - either by using our web config generator, or manually. This is explained in-depth in **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** section, so refer to that if you want more detailed information. We'll use web config generator way, since it's much easier.

Navigate to our **[web config generator](https://justarchinet.github.io/ASF-WebConfigGenerator)** page with your favourite browser, you'll need to have javascript enabled in case you manually disabled it. We recommend Chrome or Firefox, but it should work on all most popular browsers.

After opening the page, switch to "Bot" tab. You should now see a page similar to the one below:

![Bot tab](https://i.imgur.com/aF3k8Rg.png)

If by any chance the version of ASF that you've just downloaded is older than what config generator is set to use by default, simply choose your ASF version from the dropdown menu. This can happen as the config generator can be used for generating configs to newer (pre-release) ASF versions that weren't marked as stable yet. You've downloaded latest stable release of ASF that is verified to work reliably.

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

You can now hit "download" button and our web config generator will generate new `json` file based on your chosen name. Save that file into `config` directory which is located in the folder where you've extracted our zip file in the previous step.

Your `config` directory will now look like this:

![Struktura 2](https://i.imgur.com/crWdjcp.png)

Congratulations! You've just finished the very basic ASF bot configuration. We'll extend this shortly, for now this is everything that you need.

* * *

### Pokretanje ASF-a

You're now ready to launch the program for the first time. Simply double-click `ArchiSteamFarm` binary in ASF directory.

After doing so, assuming you installed all required dependencies in the first step, ASF should launch properly, notice your first bot (if you didn't forget to put generated config in `config` directory), and attempt to log in:

![ASF](https://i.imgur.com/u5hrSFz.png)

If you supplied `SteamLogin` and `SteamPassword` for ASF to use, you'll be asked for your SteamGuard token only (e-mail, 2FA or none, depending on your Steam settings). If you didn't, you'll also be asked for your Steam login and password.

Now is a good time to review our **[privacy policy](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Statistics#current-privacy-policy)** section if you're concerned about what will happen next, as stated by ASF itself.

After passing through initial login gate, assuming your details are correct, you'll successfully log in, and ASF will start idling using default settings that you didn't change as of now:

![ASF 2](https://i.imgur.com/Cb7DBl4.png)

This proves that ASF is now successfully doing its job on your account, so you can now minimize the program and do something else. After enough of time (depending on **[performance](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)**), you'll see Steam trading cards slowly being dropped. Of course, for that to happen you must have valid games to idle, showing as "you can get X more card drops from playing this game" on your **[badges page](https://steamcommunity.com/my/badges)** - if there are no games to idle, then ASF will state that there is nothing to do, as stated in our **[FAQ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ#what-is-asf)**.

This concludes our very basic setting up guide. You can now decide whether you want to configure ASF further, or let it do its job in default settings. We'll cover a few more basic details, then leave you entire wiki for discovery.

* * *

### Extended configuration

#### Idling several accounts at once

ASF supports idling more than one account at a time, which is its primary function. You can add more accounts to ASF by generating more bot config files, in exactly the same way as you've generated your first one just a few minutes ago. You need to ensure only two things:

- Unique bot name, if you already have your first bot named "MainAccount", you can't have another one with the same name.
- Valid login details, such as `SteamLogin`, `SteamPassword` and `SteamParentalCode` (if using Steam parental settings)

In other words, simply jump to configuration again and do exactly the same, just for your second or third account. Remember to use unique names for all of your bots.

* * *

#### Mijenjanje podešavanja

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

If you did everything properly, you'll now be able to access ASF's IPC interface under **[this](http://localhost:1242)** link, as long as ASF is running. You can use ASF-ui for various purposes, e.g. sending **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. Feel free to take a look around in order to find out all ASF-ui functionalities.

![ASF-ui](https://raw.githubusercontent.com/JustArchiNET/ASF-ui/master/.github/previews/bots.png)

Please note that ASF-ui is currently in preview state and not everything is available/working yet, but it's more than enough for simple ASF usage.

* * *

### Zaključak

You've successfully set up ASF to use your Steam accounts and you've already customized it to your liking a little. If you followed our entire guide, then you even managed to send a simple command through our ASF-ui interface. Now is a good time to read our entire **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** section in order to learn what all those different settings you've seen in advanced tab actually do, and what ASF can offer. If you've stumbled upon some issue or you have some generic question, read **[FAQ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ)** instead which should cover all, or at least majority of questions that you may have. If you want to learn everything about ASF and how it can make your life easier, head over to the rest of **[our wiki](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Home)**. Have fun!

* * *

## Opšta podešavanja

This setup is for advanced users that want to set up ASF to run in **[generic](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#generic)** variant. It's not recommended for people that can use **[OS-specific setup](#os-specific-setup)**.

You want to use generic variant mainly in three situations (but of course you can use it regardless):

- When you're using OS that we don't build OS-specific package for (such as 32-bit Windows)
- When you already have .NET Core Runtime/SDK, or want to install and use one
- When you want to minimize ASF structure size by handling runtime requirements yourself

However, keep in mind that you're in charge of .NET Core runtime in this case. This means that if your .NET Core SDK (runtime) is unavailable, outdated or broken, ASF won't work. This is why we don't recommend this setup for casual users, since you now need to ensure that your .NET Core SDK (runtime) matches ASF requirements and can run ASF, as opposed to **us** ensuring that our .NET Core runtime bundled with ASF can do so.

For generic package, you can follow entire OS-specific guide above, with two small changes. In addition to installing .NET Core prerequisites, you also want to install .NET Core SDK, and instead of having OS-specific `ArchiSteamFarm(.exe)` executable file, you now have a generic `ArchiSteamFarm.dll` binary only. Everything else is exactly the same.

With extra steps:

- Install **[.NET Core prerequisites](https://docs.microsoft.com/dotnet/core/install/dependencies?tabs=netcore31)**.
- Install **[.NET Core SDK](https://www.microsoft.com/net/download)** (or at least runtime) appropriate for your OS. You most likely want to use an installer. Refer to **[runtime requirements](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)** if you're not sure which version to install.
- Download **[latest ASF release](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** in generic variant.
- Extract the archive into new location (and `chmod +x ArchiSteamFarm.sh` if you're on Linux/OS X).
- **[Konfigurišete ASF](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)**.
- Launch ASF by either using a helper script or executing `dotnet /path/to/ArchiSteamFarm.dll` manually from your favourite shell.

Helper scripts (such as `ArchiSteamFarm.cmd` for Windows and `ArchiSteamFarm.sh` for Linux/OS X) are located next to `ArchiSteamFarm.dll` binary - those are included in generic variant only. You can use them if you don't want to execute `dotnet` command manually. Obviously helper scripts won't work if you didn't install .NET Core SDK and you don't have `dotnet` executable available in your `PATH`. Helper scripts are entirely optional to use, you can always `dotnet /path/to/ArchiSteamFarm.dll` manually.