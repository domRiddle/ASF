# Налаштування

Якщо ви тут вперше, ласкаво просимо! Ми дуже раді бачити ще одного мандрівника, який цікавиться нашим проектом, але не забувайте що з великою силою приходить велика відповідальність - ASF здатен зробити багато речей пов'язаних зі Steam, але лише за умови що **ви маєте бажання навчатися, як ним користуватися**. Крива навчання досить крута, тому ми очікуємо від вас, що ви прочитаєте цю вікі, яка детально описує як усе працює.

Якщо ви ще не кинули читати це означає що ви витримали текст вище, і це добре. Хіба що ви просто пройшли повз нього, у цьому разі для вас незабаром настануть **[погані часи](https://www.youtube.com/watch?v=WJgt6m6njVw)**... У будь якому разі, ASF це консольна програма, тож вона не має дружного GUI до яких ви звикли. ASF в першу чергу призначений для запуску на серверах, тому працює як сервіс (демон), а не як настільна програма.

Однак це не означає що ви не в змозі користуватися ним на вашому ПК, чи користуватися ним у якийсь більш складний спосіб ніж звичайно, нічого такого. ASF це автономна програма, яка не потребує установки, та працює відразу з коробки, але потребує конфігурації перш ніж бути корисною. Конфігурація це спосіб сказати ASF що вона має робити після запуску. Якщо ви запустите її без конфігурації, ASF просто нічого не буде робити.

* * *

## Налаштування для конкретної ОС

Взагалі, ось що ми з вами зараз зробимо за кілька хвилин:

- Встановимо **[передумови для .NET Core](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)**.
- Завантажимо **[останній випуск ASf](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** у варіанті відповідному конкретній ОС.
- Розпакуємо архів до нового місця (та зробимо `chmod +x ArchiSteamFarm` якщо ви під Linux/OS X).
- **[Сконфігуруємо ASF](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)**.
- Запустимо ASF та побачимо її магію.

Звучить досить просто, чи не так? Нумо зробимо це.

* * *

### Передумови для .NET Core

Перший крок це переконатися, що ваша ОС взагалі може коректно запустити ASF. ASF запрограмовано на C#, на основі .NET Core та може потребувати нативні бібліотеки, які ще недоступні для вашої платформи. Залежно від того, користуєтесь ви Windows, Linux чи OS X, у вас будуть різні вимоги, але усі вони приведені у документі **[.NET Core prerequisites](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)**, тож користуйтеся ним. Це наш довідковий матеріал, яким слід користуватися, але щоб зробити це простішим для вас ми також наводимо усі необхідні пакети нижче, тому вам немає необхідності читати повний документ.

Цілком нормально, якщо деякі (або навіть усі) залежності вже існують у вашій системі через те, що були встановлені якимось програмним забезпеченням, яким ви вже користуєтесь. Однак, вам слід переконатися що це саме так запустивши відповідний інсталятор для вашої ОС - без цих залежностей ASF взагалі не запуститься.

Пам'ятайте, що вам не потрібно більше нічого для запуску пакетів ASF для конкретної ОС, особливо встановлювати .NET Core SDK чи навіть середовище виконання, оскільки пакет для конкретної ОС вже включає все це до свого складу. Вам потрібні лише передумови для .NET Core (залежності), щоб запустити середовище виконання включене до ASF.

#### **[Windows](https://docs.microsoft.com/en-us/dotnet/core/windows-prerequisites?tabs=netcore2x)**:

- **[Microsoft Visual C++ 2015 Redistributable Update 3 RC](https://www.microsoft.com/en-us/download/details.aspx?id=52685)** (x64 для 64-bit Windows, x86 для 32-bit Windows)
- Наполегливо рекомендуємо переконатися, що усі оновлення Windows вже встановлені. Якнайменше вам потрібні пакети **[KB2533623](https://support.microsoft.com/en-us/help/2533623/microsoft-security-advisory-insecure-library-loading-could-allow-remot)** та **[KB2999226](https://support.microsoft.com/en-us/help/2999226/update-for-universal-c-runtime-in-windows)**, але можуть бути потрібні й інші. Усі вони вже встановлені якщо ваша Windows цілком оновлена. Переконайтеся що виконали ці вимоги перш ніж встановлювати пакет Visual C++.

#### **[Linux](https://docs.microsoft.com/en-us/dotnet/core/linux-prerequisites?tabs=netcore2x)**:

Назви пакетів залежать від обраного дистрибутиву Linux, тож ми наводимо найпоширеніші з них. Ви можете отримати усі з них через стандартний менеджер пакетів у вашій ОС (такий як `apt` для Debian чи `yum` для CentOS).

- libcurl3 (libcurl)
- libicu (остання версія для вашого дистрибутиву, наприклад `libicu57` для Debian 9)
- libkrb5-3 (krb5-libs)
- liblttng-ust0 (lttng-ust)
- libssl1.0.2 (libssl, openssl-libs, compat-openssl10, остання з версій 1.0.X для вашого дистрибутиву)
- zlib1g (zlib)

Якнайменше частина з них має бути вже встановлена у вашій системі (як наприклад zlib1g, яка сьогодні необхідна майже кожному дистрибутиву).

#### **[OS X](https://docs.microsoft.com/en-us/dotnet/core/macos-prerequisites?tabs=netcore2x)**:

- На даний момент передумови відсутні

* * *

### Завантаження

Оскільки ми вже маємо всі необхідні передумови, наступний крок це завантаження **[останнього випуску ASF](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)**. ASF наявний у декількох варіантах, але вам потрібен пакет який відповідає вашій операційній системі та архітектури. Наприклад, якщо ви користуєтесь `64`-розрядною `Win`dows, то вам потрібен пакет `ASF-win-x64`. Для отримання додаткової інформації щодо існуючих варіантів, дивіться розділ **[сумісність](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-uk-UA)**. ASF також може працювати на ОС, для яких ми не робимо пакет для конкретної ОС, як наприклад **32-розрядна Windows**, якщо вам це потрібно - переходьте до розділу **[універсальне налаштування](#user-content-Універсальне-налаштування)**.

![Assets](https://i.imgur.com/Ym2xPE5.png)

Одразу після того, як ви завантажите необхідний пакет та розпакуєте zip-файл (ми радимо користуватися **[7-zip](https://www.7-zip.org)**), ви побачите величезний безлад з файлів та папок. Не хвилюйтеся, зараз ми приберемося.

Якщо ви користуєтесь Linux/OS X, не забудьте виконати команду `chmod +x ArchiSteamFarm`, бо дозволи не встановлюються автоматично у zip-файлі. Це треба зробити лише один раз після початкового розпакування.

Радимо розпакувати ASF до **його власної директорії**, а не до якоїсь вже існуючої директорії яка має у собі щось інше - функція автоматичного оновлення ASF видаліть усі старі та непов'язані з ASF файли під час оновлення, що може призвести до втрати будь чого, що ви поклали до директорії ASF. Якщо ви маєте якісь додаткові скрипти чи інші файли, які бажаєте використовувати разом з ASF, покладіть їх на одну папку вище.

Приклад того, як може виглядати ця структура:

    C:\ASF (сюди ви покладете свої власні речі)
        ├── ASF shortcut.lnk (необов'язково)
        ├── конфігурації shortcut.lnk (необов'язково)
        ├── Commands.txt (необов'язково)
        ├── MyExtraScript.bat (необов'язково)
        ├──... (будь які інші файли на ваш вибір, необов'язково)
        └── Core (призначена лише для ASF, сюди ви розпакували архів)
             ├── ArchiSteamFarm.dll
             ├── config
             └── (...)
    

Ми рекомендуємо саме таку структуру, щоб вам не було потреби шукати щось у величезному масиві файлів та папок включених до ASF, оскільки вам потрібні лише ярлики до папки config та виконуваного файла.

Добре, а зараз приготуємо папку ASF до використання. Якщо бажаєте - можете пропустити наступний крок, бо очищення структури ASF не є обов'язковим, але воно зробить ваше життя трохи легшим.

Відкрийте папку ASF та знайдіть виконуваний файл, це буде `ArchiSteamFarm.exe` для Windows чи `ArchiSteamFarm` для Linux/OS X. Клацніть на ньому правою кнопкою миші та оберіть "Копіювати". Тепер перейдіть до місця, де ви бажаете помістити ярлик для ASF (наприклад на робочий стіл), клацніть правою кнопкою миші та оберіть "Вставити ярлик". Ви можете перейменувати ваш ярлик як бажаєте, наприклад давши йому ім'я "ASF". Тепер зробіть те ж саме з каталогом `config`, який ви можете знайти у тому ж місці де й виконуваний файл ASF.

Після невеличкого прибирання у вас тепер є зручна структура, схожа на таку:

![Structure](https://i.imgur.com/k85csaZ.png)

Це надасть вам легкий доступ до виконуваного файлу ASF та файлів конфігурації без зайвого клопоту. Я обрав структуру, описану вище, тому мої файли ASF знаходяться всередині каталогу "Core". Ви можете змінити цю структуру на свій смак, наприклад розмістити ярлики ASF так config на робочому столі, а каталог ASF наприклад у `C:\ASF`, вирішувати вам.

Користувачам Linux/OS X рекомендується зробити те ж саме, ви можете використати зручний механізм символьних посилань через `ln -s`.

* * *

### Конфігурація

Тепер ми готові зробити останній крок, конфігурацію. Це мабуть найскладніший крок, оскільки він включає в себе велику кількість нової інформації, тому ми спробуємо надати тут кілька простих для розуміння прикладів та спрощене пояснення.

Перше й найголовніше, у нас є сторінка присвячена **[конфігурації](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-uk-UA)**, яка описує **геть усе** зв'язане з конфігурацією, але це величезний обсяг нової інформації, більшість з якої вам не потрібна прямо зараз. Замість цього, ми навчимо вас, як отримати інформацію, яка вам зараз потрібна.

Конфігурацію ASF можна зробити двома шляхами - або за допомогою нашого веб генератора конфігурацій, або вручну. Це докладно пояснюється у розділі **[конфігурації](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-uk-UA)**, тому зверніться до нього якщо вам потрібна детальна інформація. Ми підемо шляхом використання веб генератора конфігурацій, бо це набагато простіше.

Перейдіть на сторінку нашого **[веб генератора конфігурацій](https://justarchinet.github.io/ASF-WebConfigGenerator)** за допомогою вашого улюбленого браузера, також вам потрібно щоб javascript було ввімкнено якщо ви раніше вимкнули його вручну. Ми рекомендуемо Chrome чи Firefox, але він має працювати в усіх найпопулярніших браузерах.

Після відкриття сторінки, перейдіть на вкладку "Бот". Ви маєте побачити сторінку схожу на приведену нижче:

![Bot tab](https://i.imgur.com/iQiqG13.png)

Якщо за якихось обставин завантажена вами версія ASF більш стара, ніж генератор конфігурацій використовує за замовчуванням, просто оберіть потрібну версію ASF з випадного меню. Це може статися тому, що генератор конфігурацій використовується для генерації конфігурацій новішої (підготовчої) версії, яка ще не позначена як стабільна. Ви завантажили останню стабільну версію ASF, яка перевірена щодо надійної роботи.

Почніть з введення імені боту до поля, яке виділено червоним. Це може бути будь яке ім'я, яким ви б хотіли користатися, наприклад нікнейм, ім'я акаунта, номер, чи щось інше. Є лише одно слово, яке ви не можете обрати, `ASF`, бо це є ключове слово, зарезервоване для файлу глобальної конфігурації. На додаток до цього, ім'я вашого бота не може починатися з крапки (ASF навмисно ігнорує такі файли). Ми також рекомендуємо уникати використання пробілів, якщо потрібно ви можете користуватися символом `_` для розділення слів.

Після того, як ви обрали ім'я, ввімкніть перемикач `Enabled`, це визначає що ASF має автоматично запускати вашого бота після запуску (програми).

Тепер вам треба обрати один з варіантів:

- Ви можете додати ваш логін до поля `SteamLogin` та ваш пароль до поля `SteamPassword`
- Чи ви можете залишити їх порожніми

Перший варіант дасть змогу ASF автоматично використовувати ваші облікові дані під час запуску, щоб вам не довелося вводити їх вручну при кожному запуску ASF. Однак ви можете вирішити пропустити їх, у цьому разі вони не будуть збережені і ASF не зможе автоматично стартувати без вашої допомоги, а вам доведеться вводити їх протягом роботи.

ASF потребує ваші облікові дані бо він має вбудовану реалізацію клієнта Steam, і для входу потребує те ж саме, що й офіційних клієнт яким ви користуєтесь. Ваші облікові дані не зберігаються у жодному місці окрім каталогу `config` у ASF, наш веб генератор конфігурації цілком виконується на стороні клієнта, що означає що ви навіть можете запустити його без підключення до інтернет і зробити собі конфігураційні файли, і дані, які ви в ньому вводити ніколи не залишають ваш ПК, тому немає потреби турбуватися про будь-який витік конфіденційних даних. Однак, якщо за якихось причин ви не хочете вводити в нього свої облікові дані - ми це розуміємо, і надаємо можливість додати їх до файла конфігурації пізніше вручну, або цілком пропустити їх і вводити їх лише по запиту ASF. Більше інформації щодо безпеки ви можете знайти у розділі "**[конфігурація](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-uk-UA)**".

Також ми можете вирішити залишити пустим лише одне поле, наприклад `SteamPassword`, у цьому разі ASF буде автоматично використовувати логін, але буде запитувати пароль (схоже на те, що робить офіційний клієнт Steam). Якщо ви користуєтесь сімейним режимом Steam щоб розблокувати акаунт, вам потрібно ввести код у поле `SteamParentalCode`.

Після прийняття рішень та додаткових даних, ваша веб-сторінка буде виглядати схоже на те, що показано нижче:

![Bot tab 2](https://i.imgur.com/yf54Ouc.png)

You can now hit "download" button and our web config generator will generate new `json` file based on your chosen name. Save that file into `config` directory of ASF. You can use previously-created `config` shortcut, or find `config` directory manually, directly in ASF file structure.

Your `config` directory will now look like this:

![Structure 2](https://i.imgur.com/2s7ZUUu.png)

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

Now you can download your ASF config and put it in your `config` directory, as usual. Afterwards, launch ASF again, and you should be able to confirm that it properly started IPC interface:

![IPC 2](https://i.imgur.com/ZmkO8pk.png)

If you did everything properly, you'll now be able to access ASF's IPC interface under **[this](http://127.0.0.1:1242)** link, as long as ASF is running. You can use ASF-ui for various purposes, e.g. sending **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. Feel free to take a look around in order to find out all ASF-ui functionalities.

![IPC 3](https://i.imgur.com/vCu2ZY5.png)

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

- Встановимо **[передумови для .NET Core](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)**.
- Install **[.NET Core SDK](https://www.microsoft.com/net/download)** (or at least runtime) appropriate for your OS. You most likely want to use an installer. Refer to **[runtime requirements](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)** if you're not sure which version to install.
- Download **[latest ASF release](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** in generic variant.
- Extract the archive into new location (and `chmod +x ArchiSteamFarm.sh` if you're on Linux/OS X).
- **[Сконфігуруємо ASF](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)**.
- Launch ASF by either using a helper script or executing `dotnet /path/to/ArchiSteamFarm.dll` manually from your favourite shell.

Helper scripts (such as `ArchiSteamFarm.cmd` for Windows and `ArchiSteamFarm.sh` for Linux/OS X) are located next to `ArchiSteamFarm.dll` binary - those are included in generic variant only. You can use them if you don't want to execute `dotnet` command manually. You can also make a shortcut to those scripts like showed above, since they're supposed to provide binary replacement in a script way. Obviously helper scripts won't work if you didn't install .NET Core SDK and you don't have `dotnet` executable available in your `PATH`. Helper scripts are entirely optional to use, you can always `dotnet /path/to/ArchiSteamFarm.dll` manually.