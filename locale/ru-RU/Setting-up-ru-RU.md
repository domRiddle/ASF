# Установка

Если вы пришли сюда впервые - Добро Пожаловать! Рады видеть что ещё один путник заинтересован в нашем проекте, но просим вас не забывать, что с большой силой приходит большая ответственность - ASF может сделать в Steam многое, но только если **вам не лень учиться, как им пользоваться**. Кривая обучения довольно крутая, и поэтому от вас ожидается что вы прочтёте эту wiki, которая подробно описывает как всё работает. Если вы ищете программу для фарма в Steam, которая не требует чтения и понимания - воспользуйтесь **[Idle Master](https://www.steamidlemaster.com)**, потому что никто не будет делать за вас домашнее задание по чтению wiki.

Если вы всё ещё читаете, значит вас не напугало вступление выше, и это радует. Конечно, если вы не пропустили его, в противном случае у вас скоро будут **[большие проблемы](https://www.youtube.com/watch?v=WJgt6m6njVw)**... В любом случае, ASF это консольное приложение, а это значит что у этой программы отсутствует дружественный графический интерфейс к которому большинство из вас привыкло. ASF разрабатывался в первую очередь для работы на серверах, и поэтому работает как служба (демон), а не как настольное приложение.

Однако это не означает что вы не можете пользоваться им на своём ПК, или пользоваться им каким-то более сложным способом, ничего такого. ASF это отдельная программа не требующая инсталляции, и работает сразу "из коробки", однако требует конфигурирования чтобы быть полезной. Конфигурирование это способ указать ASF, что она должна делать после запуска. Если вы запустите ASF без конфигурирования она просто ничего не будет делать.

* * *

## Видеоинструкция

Если вы ненавидите читать, и предпочитаете вместо этого посмотреть видео, вы можете посмотреть ролик который снял для вас **[@GamingTaylor](https://www.youtube.com/channel/UCTjrsQgjZmBzYzWaAh0zI3Q)** по **[этой ссылке](https://www.youtube.com/watch?v=gi2UjXtGWgc)**. Обратите внимание, что вам всё же придётся для справок обращаться к wiki, чтобы получить более подробное объяснение и актуальную инструкцию по установке. Хотя видео на YouTube это хороший материал чтобы показать, как именно конфигурируется и запускается ASF, его довольно сложно обновлять если что-то поменялось, поэтому мы приводим его здесь просто для справки. Если вы хотите получить подробные объяснения, документацию и полную инструкцию по установке, вам стоит продолжить чтение в разделе **[OS-specific setup](#установка-для-конкретных-ос)**, а видео использовать просто для общей информации. Мы приводим его, потому что оно полезно в **некоторых** случаях, но всё же рекомендуем прочитать wiki.

* * *

## Установка для конкретных ОС

В общем случае, вот что нам придётся сделать в следующие несколько минут:

- Установить **[предусловия для .NET Core](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)**.
- Скачать **[последнюю версию ASF](https://github.com/JustArchi/ArchiSteamFarm/releases/latest)** в варианте для конкретной ОС.
- Распаковать архив в новую папку (и выполнить `chmod +x ArchiSteamFarm` если вы работаете в Linux/OS X).
- **[Сконфигурировать ASF](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration-ru-RU)**.
- Запустить ASF и увидеть магию.

Звучит довольно просто, правда? Давайте займёмся этим.

* * *

### Предусловия для .NET Core

Первое что надо сделать - убедиться, что ваша ОС вообще может запустить ASF. ASF написана на C#, основаный на .NET Core, и может требовать нативные библиотеки, которые пока недоступны для вашей платформы. В зависимости от того, используете вы Windows, Linux или OS X, вам подребуется выполнить различные требования, но все они перечислены в документе **[.NET Core prerequisites](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)** (предусловия для .NET Core), которому вы должны следовать. Просто следуйте инструкции, вполне возможно что у вас уже установлены все нужные библиотеки, но вы должны это перепроверить.

Например, для Windows всё что вам понадобиться это установить `Microsoft Visual C++ 2015 Redistributable Update 3 RC`, которое **может быть уже установлено какой-то игрой или приложением которым вы пользуетесь**. Для Linux вам понадобится установить библиотеки из списка с помощью `apt-get install` или другого менеджера пакетов который используется в вашем дистрибутиве. Для OS X на данный момент нет никаких обязательных условий, но это может измениться в будущем.

Помните, что для установки варианта под конкретную ОС вам больше ничего не надо делать, особенно устанавливать .NET Core SDK или даже среду исполнения, поскольку пакеты для конкретных ОС уже включают всё это в себя. Вам нужны только предусловия для .NET Core (зависимости) чтобы запустить среду исполнения включенную в ASF.

Поскольку вычленить нужную вам информацию из документа может быть сложно, мы приводим список зависимостей прямо здесь, но не забывайте сверяться с основным документом для .NET Core на случай если зависимости изменяться в будущем.

#### **[Windows](https://docs.microsoft.com/en-us/dotnet/core/windows-prerequisites?tabs=netcore2x)**:

- **[Microsoft Visual C++ 2015 Redistributable Update 3 RC](https://www.microsoft.com/en-us/download/details.aspx?id=52685)** (x64 для 64-битного Windows, x86 для 32-битного Windows)
- Настоятельно рекомендуется убедиться, что у вас уже установлены все обновления Windows. Как минимум вам потребуются **[KB2533623](https://support.microsoft.com/en-us/help/2533623/)** и **[KB2999226](https://support.microsoft.com/en-us/help/2999226/)**, но могут понадобиться и другие обновления. Все они уже должны быть установлены если ваша Windows полностью обновлена. Убедитесть что вы выполнили эти требования до того как устанавливать пакет Visual C++.

Вполне возможно что пакет Redistributable уже был установлен какой-то другой программой или игрой, но вам стоит перепроверить это запустив установщик, просто для уверенности. ASF не запустится если эта зависимость отсутствует.

#### **[Linux](https://docs.microsoft.com/en-us/dotnet/core/linux-prerequisites?tabs=netcore2x)**:

Имена пакетов зависят от дистрибутива, мы приводим наиболее распространённые. Вам нужно получить их с помощью встроенного менеджера пакетов для вашей ОС (таким как `apt-get` для Debian или `yum` для CentOS).

- libunwind8 (libunwind)
- liblttng-ust0 (lttng-ust)
- libcurl3 (libcurl)
- libssl1.0.2 (libssl, openssl-libs, последняя версия 1.0.X для вашего дистрибутива)
- libuuid1 (libuuid)
- libkrb5-3 (krb5-libs)
- libicu57 (libicu, последняя версия для вашего дистрибутива)
- zlib1g (zlib)

Как минимум часть из них должна быть уже доступна для вашей системы (как например zlib1g которая в наши дни требуется практически в любом дистрибутиве Linux).

#### **[OS X](https://docs.microsoft.com/en-us/dotnet/core/macos-prerequisites)**:

- На данный момент требований нет, хотя возможно вам понадобится **[увеличить максимальный предел открытых файлов](https://docs.microsoft.com/en-us/dotnet/core/macos-prerequisites?tabs=netcore2x#increase-the-maximum-open-file-limit)**. Это не должно быть необходимым для самого ASF, но имейте это в виду если у вас будут какие-то проблемы.

* * *

### Скачивание

Теперь, раз у нас уже есть все необходимые зависимости, следующим шагом будет скачать **[последнюю версию ASF](https://github.com/JustArchi/ArchiSteamFarm/releases/latest)**. ASF доступен в разных вариантах, вас интересует пакет, который подходит для вашей операционной системы и архитектуры. Например, если вы пользуетесь `64`-битной `Win`dows, вам нужен пакет `ASF-win-x64`. Чтобы получить подробную информацию обо всех доступных вариантах, ознакомьтесь с разделом **[совместимость](https://github.com/JustArchi/ArchiSteamFarm/wiki/Compatibility-ru-RU)**. ASF так же можно запустить на ОС для которых мы не делаем пакетов под конкретную ОС, таких как **32-битная Windows**, для этого перейдите в раздел **[универсальная установка](#универсальная-установка)**.

![Assets](https://i.imgur.com/Ym2xPE5.png)

После того, как вы скачаете нужный вам пакет и распакуете его из zip-файла (мы рекомендуем использовать **[7-zip](https://www.7-zip.org)**), вы получите совершенно запутанную кучу папок и файлов. Не волнуйтесь, сейчас мы всё это подчистим.

Если вы пользуетесь Linux/OS X, не забудьте выполнить команду `chmod +x ArchiSteamFarm`, поскольку права запуска файлов не сохраняются в zip-архивах. Это нужно сделать только один раз после первоначальной распаковки.

Рекомендуем распаковывать ASF в **отдельную папку** а не в какую-то папку которую вы используете для других целей - система авто-обновления ASF удалит все файлы не связанные с ASF при обновлении, и вы потеряете все свои файлы, находящиеся в папке ASF. Если у вас есть дополнительные скрипты или файлы, которые вы хотите использовать вместе с ASF, положите их на одну папку выше.

Cnруктура папок должна быть примерно такой:

    C:\ASF (здесь лежат ваши собственные файлы)
        ├── ярлык ASF.lnk (не обязательно)
        ├── ярлык Config.lnk (не обязательно)
        ├── Commands.txt (не обязательно)
        ├── MyExtraScript.bat (не обязательно)
        ├── ... (любые другие файлы на ваш выбор, не обязательно)
        └── Core (выделенная только для ASF папка, куда вы распакуете архив)
             ├── ArchiSteamFarm.dll
             ├── config
             └── (...)
    

Мы рекомендуем такую структуру папок ещё и для того, чтобы вам не пришлось путаться в большом количестве файлов в папке ASF, поскольку для использования вам понадобятся только ярлыки для папки с конфигурациями и для основного запускаемого файла.

Хорошо, теперь подготовим папку ASF для использования. Если хотите, этот шаг можно пропустить, поскольку очистка структуры папок ASF не является необходимой, но это сделает вашу жизнь немножко легче.

Откройте папку ASF и найдите основной запускаемый файл, это будет`ArchiSteamFarm.exe` для Windows, и `ArchiSteamFarm` для Linux/OS X. Нажмите на него правой кнопкой мыши и выберите "Копировать". Теперь перейдите в папку, в которой вы планируете сохранить ярлык для ASF (например на рабочий стол), нажмите на свободном месте правой кнопкой мыши и выберите "Вставить ярлык". Если хотите, можете переименовать ярлык, например назвав его "ASF". Теперь повторите то же самое для папки `config`, которая находится там же, где и запускаемый файл ASF.

После такой небольшой очистки, у вас будет очень удобная структура, подобная показанной ниже:

![Structure](https://i.imgur.com/k85csaZ.png)

Это даст вам лёгкий доступ к запускаемому файлу ASF и конфигурационным файлам без лишних проблем. Я решил использовать структуру описанную выше, поэтому мои файлы ASF находятся непосредственно в папке "Core". Вы можете переделать эту структуру на свой вкус, как например поместить ярлыки для ASF и папки config на рабочий стол а папку с ASF например в `C:\ASF`, решать вам.

Пользователям Linux/OS X рекомендуется поступить точно так же, вы можете воспользоваться удобным механизмом символьных ссылок, доступным с помощью команды `ln -s`.

* * *

### Конфигурирование

Теперь мы готовы к последнему шагу, конфигурированию. Это пожалуй самый сложный шаг, поскольку включает в себя много новой информации с которой вы пока не знакомы, поэтому здесь мы предоставим несколько понятных примеров и упрощённое объяснение.

Первое и главное, у нас есть раздел "**[Конфигурация](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration-ru-RU)**" в котором подробно разъясняется **всё** что связано с конфигурированием, но это огромный объём новой информации, значительная часть которой всё равно вам пока не потребуется. Вместо этого, мы научим вас, как получить ту информацию, которую вы ищете.

Конфигурирование ASF может быть сделано двумя методами - либо используя сетевой генератор конфигураций, либо вручную. Это объяснено в деталях в разделе "**[Конфигурация](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration-ru-RU)**", поэтому если вам нужна подробная информация - обращайтесь для справок к этому разделу. Здесь мы пойдём по пути сетевого генератора конфигураций, поскольку это намного легче.

Перейдите на страницу **[сетевого генератора конфигураций](https://justarchi.github.io/ArchiSteamFarm)** в своём любимом браузере, вам понадобится включенный javascript (на случай если вы его отключили вручную). Мы рекомендуем Chrome или Firefox, но генератор должен работать в любом из современных популярных браузеров.

После того, как вы перешли на страницу генератора, переключитесь на закладку "Бот". Вы должны увидеть страницу, похожую на показанную ниже:

![Bot tab](https://i.imgur.com/VYkT5Mo.png)

If by any chance the version of ASF that you've just downloaded is older than what config generator is set to use by default, simply choose your ASF version from the dropdown menu. This can happen as the config generator can be used for generating configs to newer (pre-release) ASF versions that weren't marked as stable yet. You've downloaded latest stable release of ASF that is verified to work reliably.

Start from putting name for your bot into the field highlighted as red. This can be any name you'd like to use, such as your nickname, account name, a number, or anything else. There are only 3 words you can't use, those are: `ASF`, `example` and `minimal`. In addition to that your bot name can't start with a dot (ASF intentionally ignores those files). We also recommend that you avoid using spaces, you can use `_` as a word separator if needed.

After you decided about your name, change `Enabled` switch to be on, this defines whether your bot is supposed to be started by ASF automatically after launch (of the program).

Now you can decide upon two things:

- You can put your login in `SteamLogin` field and your password in `SteamPassword` field
- Or you can leave them empty

Doing the first thing will allow ASF to automatically use your account credentials during startup, so you won't need to input them manually each time ASF needs them. You can however decide to omit them, in which case they're not being saved, so ASF won't be able to automatically start without your help and you'll need to input them during runtime.

ASF requires your login credentials because it includes its own implementation of Steam client and needs the same details to log in as the one that you use yourself. Your login credentials are not saved anywhere but on your PC in ASF `config` directory only, our web config generator is client-based which means that the code is run locally in your browser to generate valid ASF configs, without details you're inputting ever leaving your PC in the first place, so there is no need to worry about any possible sensitive data leak. Still, if you for whatever reason don't want to put your credentials there, we understand that, and you can put them manually later in generated files, or omit them entirely and put them only in ASF command prompt. More on security matter can be found in **[configuration](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration)** section.

You can also decide to leave just one field empty, such as `SteamPassword`, ASF will then be able to use your login automatically, but will still ask for password (similar to Steam Client). If you're using Steam parental PIN to unlock the account, you'll need to toggle advanced settings and put it into `SteamParentalPIN` field.

After the decision and optional details, your web page will now look similar to the one below:

![Bot tab 2](https://i.imgur.com/BUmF0Wr.png)

You can now hit "download" button and our web config generator will generate new `json` file based on your chosen name:

![Bot tab 3](https://i.imgur.com/ylyvzvL.png)

Save that file into `config` directory of ASF. You can use previously-created `config` shortcut, or find `config` directory manually, directly in ASF file structure.

Your `config` directory will now look like this:

![Structure 2](https://i.imgur.com/doYnbB9.png)

Congratulations! You've just finished the very basic ASF bot configuration. We'll extend this shortly, for now this is everything that you need.

* * *

### Running ASF

You're now ready to launch the program for the first time. Simply double-click ASF shortcut, or `ArchiSteamFarm(.exe)` binary in ASF directory.

After doing so, assuming you installed all required dependencies in the first step, ASF should launch properly, notice your first bot (if you didn't forget to put generated config in `config` directory), and attempt to log in:

![ASF](https://i.imgur.com/u5hrSFz.png)

If you supplied `SteamLogin` and `SteamPassword` for ASF to use, you'll be asked for your SteamGuard token only (e-mail, 2FA or none, depending on your Steam settings). If you didn't, you'll also be asked for your Steam login and password.

Now is a good time to review our **[privacy policy](https://github.com/JustArchi/ArchiSteamFarm/wiki/Statistics#current-privacy-policy)** section if you're concerned about what will happen next, as stated by ASF itself.

After passing through initial login gate, assuming your details are correct, you'll successfully log in, and ASF will start idling using default settings that you didn't change as of now:

![ASF 2](https://i.imgur.com/Cb7DBl4.png)

This proves that ASF is now successfully doing its job on your account, so you can now minimize the program and do something else. After enough of time (depending on **[performance](https://github.com/JustArchi/ArchiSteamFarm/wiki/Performance)**), you'll see Steam trading cards slowly being dropped. Of course, for that to happen you must have valid games to idle, showing as "you can get X more card drops from playing this game" on your **[badges page](https://steamcommunity.com/my/badges)** - if there are no games to idle, then ASF will state that there is nothing to do, as stated in our **[FAQ](https://github.com/JustArchi/ArchiSteamFarm/wiki/FAQ#so-how-it-exactly-works)**.

This concludes our very basic setting up guide. You can now decide whether you want to configure ASF further, or let it do its job in default settings. We'll cover a few more basic details, then leave you entire wiki for discovery.

* * *

### Extended configuration

#### Idling several accounts at once

ASF supports idling more than one account at a time, which is its primary function. You can add more accounts to ASF by generating more bot config files, in exactly the same way as you've generated your first one just a few minutes ago. You need to ensure only two things:

- Unique bot name, if you already have your first bot named "MainAccount", you can't have another one with the same name.
- Valid login details, such as `SteamLogin`, `SteamPassword` and `SteamParentalPIN` (if using Steam parental settings)

In other words, simply jump to configuration again and do exactly the same, just for your second or third account. Remember to use unique names for all of your bots.

#### Changing settings

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

### Summary

You've successfully set up ASF to use your Steam accounts and you've even customized it slightly to your liking already. Now is a good time to read entire **[configuration](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration)** section in order to learn what all those different settings you've seen in advanced tab actually do, and what ASF can offer. If you've stumbled upon some issue or you have some generic question, read **[FAQ](https://github.com/JustArchi/ArchiSteamFarm/wiki/FAQ)** instead which should cover all, or at least majority of questions that you might have. If you want to learn everything about ASF and how it can make your life easier, head over the rest of **[our wiki](https://github.com/JustArchi/ArchiSteamFarm/wiki/Home)**. Have fun!

* * *

## Generic setup

This setup is for advanced users that want to set up ASF to run in **[generic](https://github.com/JustArchi/ArchiSteamFarm/wiki/Compatibility#generic)** variant. It's not recommended for people that can use **[OS-specific setup](#os-specific-setup)**.

You want to use generic variant mainly in three situations (but of course you can use it regardless):

- When you're using OS that we don't build OS-specific package for (such as 32-bit Windows)
- When you already have .NET Core Runtime/SDK, or want to install and use one
- When you want to minimize ASF structure size by handling runtime requirements yourself

However, keep in mind that you're in charge of .NET Core runtime in this case. This means that if your .NET Core SDK (runtime) is unavailable, outdated or broken, ASF won't work. This is why we don't recommend this setup for casual users, since you now need to ensure that your .NET Core SDK (runtime) matches ASF requirements and can run ASF, as opposed to **us** ensuring that our .NET Core runtime bundled with ASF can do so.

For generic package, you can follow entire OS-specific guide above, with two small changes. In addition to installing .NET Core prerequisites, you also want to install .NET Core SDK, and instead of having OS-specific `ArchiSteamFarm(.exe)` executable file, you now have a generic `ArchiSteamFarm.dll` binary only. Everything else is exactly the same.

With extra steps:

- Установить **[предусловия для .NET Core](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)**.
- Install **[.NET Core SDK](https://www.microsoft.com/net/download/core#/sdk)** (or at least runtime) appropriate for your OS. You most likely want to use an installer. Refer to **[runtime requirements](https://github.com/JustArchi/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)** if you're not sure which version to install.
- Download **[latest ASF release](https://github.com/JustArchi/ArchiSteamFarm/releases/latest)** in generic variant.
- Extract the archive into new location (and `chmod +x ArchiSteamFarm.sh` if you're on Linux/OS X).
- **[Сконфигурировать ASF](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration-ru-RU)**.
- Launch ASF by either using a helper script or executing `dotnet /path/to/ArchiSteamFarm.dll` manually from your favourite shell.

Helper scripts (such as `ArchiSteamFarm.cmd` for Windows and `ArchiSteamFarm.sh` for Linux/OS X) are located next to `ArchiSteamFarm.dll` binary - those are included in generic variant only. You can use them if you don't want to execute `dotnet` command manually. You can also make shortcut to those scripts like showed above, since they're supposed to provide binary replacement in a script way. Obviously helper scripts won't work if you didn't install .NET Core SDK and you don't have `dotnet` executable available in your `PATH`. Helper scripts are entirely optional to use, you can always `dotnet /path/to/ArchiSteamFarm.dll` manually.