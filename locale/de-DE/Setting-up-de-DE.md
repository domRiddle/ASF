# Installation

Wenn du hier zum ersten Mal angekommen bist, herzlich willkommen! Wir sind freuen uns mit dir einen weiteren Reisenden begrüßen zu dürfen, der sich für unser Projekt interessiert. Bedenke allerdings, dass mit großer Macht auch große Verantwortung einhergeht - ASF kann sehr viele verschiedene Aufgaben bezüglich Steam erledigen, allerdings setzt dies voraus, **dass Sorge trägst zu lernen, wie du ASF verwendest**. Es ist eine steile Lernkurve involviert, wenn du alles lernen willst, und wir erwarten von dir, dass du dieses Wiki liest, wenn du etwas brauchst, weil es im Detail beschreibt, wie alles funktioniert.

Wenn du immer noch hier bist, heißt das, dass du den Text oben überstanden hast, was toll ist. Außer du hast ihn übersprungen, was heißt, dass du bald **[schlechte Zeiten](https://www.youtube.com/watch?v=WJgt6m6njVw)** vor dir hast... Wie auch immer: ASF ist eine Konsolenanwendung, was heißt, dass das Programm selbst keine freundliche Benutzeroberfläche hat, welche du von so vielen Programmen gewohnt bist. ASF war hautpsächlich dafür gedacht auf einem Server zu laufen, und wie ein Service (daemon) verwendet zu werden und nicht als Desktopapplikation.

Dies heißt allerdings nicht, dass du ASF nicht an deinem normalen PC verwenden kannst oder dass es in irgendeiner Weise komplizierter ist als üblich. ASF ist ein eigenständiges Programm, das keine Installation benötigt und "out of the box" direkt funktioniert. Allerdings erfordert es immer noch einer Konfiguration bevor es nützlich wird. Die Konfiguration sagt ASF was es tatsächlich tun soll, nachdem du es gestartet hast. Wenn du ASF ohne Konfiguration startest, dann wird es genau nichts machen. Ganz einfach.

* * *

## betriebssystemabhängige Installation

Hier ist, was wir in den nächsten paar Minuten machen werden:

- **[.NET Core Abhängigkeiten](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)** installieren.
- Die **[neueste ASF Version](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** in der entsprechenden betriebssystemabhängigen Variante herunterladen.
- Das Archiv in einen neuen Ordner entpacken (und mit `chmod +x ArchiSteamFarm` ausführbar machen, wenn du dich auf Linux oder OS X befindest).
- **[ASF konfigurieren](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)**.
- ASF starten und der Magie ihren Lauf lassen.

Hört sich einfach genug an, richtig? Dann lass uns anfangen.

* * *

### .NET Core Abhängigkeiten

Der erste Schritt ist das versichern, dass dein Betriebssytem ASF ordnungsgemäß ausführen kann. ASF ist in C# programmiert, basierend auf .NET Core und benötigt möglicherweise native Bibliotheken, die auf deiner Plattform noch nicht verfügbar sind. Abhängig davon, ob du Windows, Linux oder OS X verwendest, wirst du unterschiedliche Voraussetzungen haben, welche allerdings alle im **[.NET Core Abhängigkeiten](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)**-Dokument, dem du folgen solltest, aufgelistet sind. Dieses ist Referenzmaterial, das verwendet werden sollte, allerdings haben wir im Sinne der Einfachheit auch alle benötigten Pakete unten aufgelistet, damit du nicht das gesamte Dokument lesen musst.

Es ist völlig normal, dass manche (oder sogar alle) Abhängigkeiten bereits in deinem System existieren, weil sie mit der Software Dritter, welche du verwendest, mitinstalliert wurden. Trotzdem solltest du sicherstellen, dass dies wirklich der Fall ist indem du das entsprechende Installationsprogramm für dein Betriebssytem ausführst - Ohne diese Abhängigkeiten wird ASF nicht einmal starten.

Behalte im Hinterkopf, dass du für betriebssystemspezifische ASF-Versionen nichts weiteres tun must. Insbesondere betrifft dies die Installation des .NET Core SDKs oder des Runtimes, da betriebssytemspezifische Versionen das alles bereits beinhalten. Du benötigst nur die .NET Core Abhängigkeiten um das .NET Core Runtime das bereits in ASF inkludiert ist auszuführen.

#### **[Windows](https://docs.microsoft.com/en-us/dotnet/core/windows-prerequisites?tabs=netcore2x)**:

- **[Microsoft Visual C++ 2015 Redistributable Update 3 RC](https://www.microsoft.com/en-us/download/details.aspx?id=52685)** (x64 for 64-bit Windows, x86 for 32-bit Windows)
- Wir empfehlen stark sicherzustellen, dass alle Windows updates installiert sind. Du benötigst zumindest **[KB2533623](https://support.microsoft.com/en-us/help/2533623/microsoft-security-advisory-insecure-library-loading-could-allow-remot)** und **[KB2999226](https://support.microsoft.com/en-us/help/2999226/update-for-universal-c-runtime-in-windows)**, aber es könnten weiteren Updates nötig sein. Es sind bereits alle installiert, wenn dein Windows aktuell ist. Versichere dich, dass du diese Voraussetzungen erfüllst, bevor du das Visual C++ Paket installierst.

#### **[Linux](https://docs.microsoft.com/en-us/dotnet/core/linux-prerequisites?tabs=netcore2x)**:

Paketnamen hängen von der Linux-Distribution, die du verwendest, ab. Wir listen nur die Gebräuchlichsten auf. Du kannst alle über den nativen Paketmanager für dein Betriebssystem (wie zum Beispiel `apt` unter Debian oder `yum` unter CentOS) installieren.

- libcurl3 (libcurl)
- libicu60 (libicu, neueste Version für deine Distribution, als Beispiel `libicu57` für Debian 9)
- libkrb5-3 (krb5-libs)
- liblttng-ust0 (lttng-ust)
- libssl1.0.2 (libssl, openssl-libs, neueste 1.0.X Version für deine Distribution)
- zlib1g (zlib)

Zumindest einige von diesen sollten bereits nativ auf dienem System verfügbar sein (wie zum Beispiel zlib1g, welches heute in fast jeder Linux Distribution verwendet wird).

Wenn du die `linux-arm`-Variante installieren willst, dann benötigst du temporär auch die .NET Core 2.0 Abhängigkeiten:

- libunwind8 (libunwind)
- libuuid1 (libuuid)

#### **[OS X](https://docs.microsoft.com/en-us/dotnet/core/macos-prerequisites?tabs=netcore2x)**:

- Bisher keine

* * *

### Herunterladen

Da wir nun alle benötigten Abhängigkeiten installiert haben, ist der nächste Schritt der Download der **[neuesten ASF-Version](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)**. ASF ist in mehreren Varianten verfügbar, aber du bist nur am Paket interessiert, das deinem Betriebssystem und der Architektur deines PCs entspricht. Zum Beispiel, wenn du `64`-bit `Win`dows verwendest, dann willst du die `ASF-win-x64`-Variante. Für mehr Information über die verfügbaren Varianten, besuche bitte den **[Kompabilitäts](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility)**-Artikel. ASF kann auch unter Betriebssystemen ausgeführt werden, für die wir kein betriebssystemspezifisches Paket zur Verfügung stellen. Ein Beispiel hierfür ist **32-bit Windows**. Dafür gehe bitte zur **[generischen Installation](#generic-setup)**.

![Assets](https://i.imgur.com/Ym2xPE5.png)

Wenn du dein Paket heruntergeladen und das zip-Archiv extrahiert hast (wir empfehlen die Verwendung von **[7-zip](https://www.7-zip.org)**), hast du ein riesiges Durcheinander von Ordnern und Dateien. Keine Angst. Wir werden das gleich aufräumen.

Wenn du Linux oder OS X verwendest, vergiss nicht mit `chmod +x ArchiSteamFarm`, die Berechtigungen zu setzen, da diese in einem zip-Archiv nicht gesetzt sind. Dies muss nur nach dem initialen Entpacken gemacht werden.

Stelle bitte sicher, dass du ASF in **einen eigenen Ordner** entpackst und nicht in einen bereits existenten, der für etwas anderes verwendet wird - ASFs automatische Updates werden alle alten Dateien in diesem Ordner löschen, was möglicherweise dazu führen könnte, dass du Dateien verlierst, die nichts mit ASF zu tun haben aber im selben Ordner sind. Solltest du zusätzliche Skripte oder Dateien haben, die du mit ASF verwenden willst, solltest du sie in den Ordner darüber tun.

Eine Beispiel-Struktur würde wie folgt aussehen:

    C:\ASF (hier tust du deine eigenen Dateien hin)
        ├── ASF Abkürzung.lnk (optional)
        ├── Config Abkürzung.lnk (optional)
        ├── Befehle.txt (optional)
        ├── MeinZusätzlichesSkript.bat (optional)
        ├── ... (Alle anderen Dateien deiner Wahl, optional)
        └── Core (nur ASF, hier extrahierst du das Archiv)
             ├── ArchiSteamFarm.dll
             ├── config
             └── (...)
    

This is also a structure we'd recommend, so you don't need to go through a massive number of files and folders included in ASF, since for usage you only need a shortcut to config folder and main binary.

Okay, we'll now prepare ASF folder for usage. If you want to, you can now skip to the next step, since cleaning up ASF structure is not required, but it will make your life a bit easier.

Open ASF folder and find core executable file, this will be `ArchiSteamFarm.exe` on Windows, and `ArchiSteamFarm` on Linux/OS X. Right click it and select "copy". Now navigate to the place you actually want to have ASF shortcut in (such as your desktop), right click and choose "paste shortcut here". You can rename your shortcut if you'd like to, such as giving it "ASF" name. Now do the same with `config` directory that you can find in the same place as ASF binary.

After a small cleanup, you'll now have a very convenient structure similar to the one below:

![Structure](https://i.imgur.com/k85csaZ.png)

This will allow you to easily access ASF binary and config files without much hassle. In my case I decided to use the structure mentioned above, so my ASF files are in "Core" directory directly inside. You can adapt this structure to your liking, such as having ASF + config shortcuts on the desktop and ASF directory e.g. in `C:\ASF` instead, it's up to you.

Linux/OS X users are advised to do the same, you can use excellent symbolic links mechanism available through `ln -s`.

* * *

### Konfiguration

We're now ready to do the very last step, the configuration. This is by far the most complicated step, since it involves a lot of new information you're not familiar with yet, so we'll try to provide some easy to understand examples and simplified explanation here.

First and foremost, there is **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** page that explains **everything** that relates to configuration, but it's a massive amount of new information, a lot of which we don't need to know right away. Instead, we'll teach you how to get the information you're actually looking for.

ASF configuration can be done in two ways - either by using our web config generator, or manually. This is explained in-depth in **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** section, so refer to that if you want more detailed information. We'll use web config generator way, since it's much easier.

Navigate to our **[web config generator](https://justarchinet.github.io/ASF-WebConfigGenerator)** page with your favourite browser, you'll need to have javascript enabled in case you manually disabled it. We recommend Chrome or Firefox, but it should work on all most popular browsers.

After opening the page, switch to "Bot" tab. You should now see a page similar to the one below:

![Bot tab](https://i.imgur.com/aF3k8Rg.png)

If by any chance the version of ASF that you've just downloaded is older than what config generator is set to use by default, simply choose your ASF version from the dropdown menu. This can happen as the config generator can be used for generating configs to newer (pre-release) ASF versions that weren't marked as stable yet. You've downloaded latest stable release of ASF that is verified to work reliably.

Start from putting name for your bot into the field highlighted as red. This can be any name you'd like to use, such as your nickname, account name, a number, or anything else. There are only 3 words you can't use, those are: `ASF`, `example` and `minimal`. In addition to that your bot name can't start with a dot (ASF intentionally ignores those files). We also recommend that you avoid using spaces, you can use `_` as a word separator if needed.

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

Dein `config` Verzeichnis sieht nun wie folgt aus:

![Structure 2](https://i.imgur.com/2s7ZUUu.png)

Glückwunsch! You've just finished the very basic ASF bot configuration. We'll extend this shortly, for now this is everything that you need.

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

### Erweiterte Konfiguration

#### Idling several accounts at once

ASF supports idling more than one account at a time, which is its primary function. You can add more accounts to ASF by generating more bot config files, in exactly the same way as you've generated your first one just a few minutes ago. You need to ensure only two things:

- Unique bot name, if you already have your first bot named "MainAccount", you can't have another one with the same name.
- Valid login details, such as `SteamLogin`, `SteamPassword` and `SteamParentalCode` (if using Steam parental settings)

In other words, simply jump to configuration again and do exactly the same, just for your second or third account. Remember to use unique names for all of your bots.

* * *

#### Einstellungen ändern

You change existing settings in exactly the same way - by generating a new config file. If you didn't close our web config generator yet, click on "toggle advanced settings" and see what is there for you to discover. For this tutorial we'll change `CustomGamePlayedWhileFarming` setting, which allows you to set custom name being displayed when ASF is idling, instead of showing actual game.

So let's do that, if you run ASF and start idling, in default settings you'll see that your Steam account is in-game now:

![Steam](https://i.imgur.com/sCdSMZj.png)

Let's change that then. Toggle advanced settings in web config generator and find `CustomGamePlayedWhileFarming`. Once you do that, put your own custom text there that you want to display, such as "Idling cards":

![Bot tab 4](https://i.imgur.com/gHqdEqb.png)

Now download the new config file in exactly the same way, then **overwrite** your old config file with new one. You can also delete your old config file and put new one in its place of course.

Once you do that and start ASF again, you'll notice that ASF now displays your custom text in previous place:

![Steam 2](https://i.imgur.com/NeFYrdU.png)

This confirms that you've successfully edited your config. In exactly the same way you can change global ASF properties, by switching from bot tab to "ASF" tab, then downloading generated config and replacing core `ASF.json` file.

* * *

#### Using ASF-ui

ASF is a console app and doesn't include a graphical user interface. However, there is ongoing work on **[ASF-ui](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#asf-ui)** frontend to our IPC interface which is still in preview state, but can be used without bigger issues.

In order to use ASF-ui, you should ensure that you set up `IPC` and `SteamOwnerID` global configuration properties (ASF tab).

For `SteamOwnerID`, you need to input unique Steam identificator in 64-bit form of your account. You can look it up in various different ways, we'll use **[SteamRep](https://steamrep.com)** for that purpose. Open the website, locate sign in through Steam button in top right corner, then log in. Afterwards, in the same place, click on your avatar, and look up `steamID64` field on your profile.

![SteamRep](https://i.imgur.com/RUuJ63i.png)

For my account, this is `76561198006963719` number. You'll have a similar one, also starting from `7656`. Copy it.

Now navigate once again to our web config generator and input that number as SteamOwnerID.

![SteamOwnerID](https://i.imgur.com/V6jslfQ.png)

You need to do only one more thing, toggle advanced settings, find `IPC` option, and enable it.

![IPC](https://i.imgur.com/NhujZCN.png)

Now you can generate your config by downloading it and replacing the original `ASF.json` in your `config` directory, as usual. Afterwards, launch ASF again, and you should be able to confirm that it properly started IPC interface:

![IPC 2](https://i.imgur.com/ZmkO8pk.png)

If you did everything properly, you'll now be able to access ASF's IPC interface under **[this](http://127.0.0.1:1242)** link, as long as ASF is running. You can use ASF-ui for various purposes, e.g. sending **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. Feel free to take a look around in order to find out all ASF-ui functionalities.

![IPC 3](https://i.imgur.com/vCu2ZY5.png)

Please note that ASF-ui is currently in preview state and not everything is available/working yet, but it's more than enough for simple ASF usage.

* * *

### Zusammenfassung

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

- Installiere **[.NET Core prerequisites](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)**.
- Install **[.NET Core SDK](https://www.microsoft.com/net/download)** (or at least runtime) appropriate for your OS. You most likely want to use an installer. Refer to **[runtime requirements](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)** if you're not sure which version to install.
- Download **[latest ASF release](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** in generic variant.
- Extract the archive into new location (and `chmod +x ArchiSteamFarm.sh` if you're on Linux/OS X).
- **[ASF konfigurieren](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)**.
- Launch ASF by either using a helper script or executing `dotnet /path/to/ArchiSteamFarm.dll` manually from your favourite shell.

Helper scripts (such as `ArchiSteamFarm.cmd` for Windows and `ArchiSteamFarm.sh` for Linux/OS X) are located next to `ArchiSteamFarm.dll` binary - those are included in generic variant only. You can use them if you don't want to execute `dotnet` command manually. You can also make a shortcut to those scripts like showed above, since they're supposed to provide binary replacement in a script way. Obviously helper scripts won't work if you didn't install .NET Core SDK and you don't have `dotnet` executable available in your `PATH`. Helper scripts are entirely optional to use, you can always `dotnet /path/to/ArchiSteamFarm.dll` manually.