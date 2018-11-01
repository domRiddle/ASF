# 설치하기

여기 오신게 처음인가요? 환영합니다! 우리 프로젝트에 관심을 가진 분이 또 있다니 정말 행복합니다. 다만 큰 힘에는 큰 책임이 따른다는 것을 기억하세요. ASF는 Steam에 관련된 다양한 많은 것을 할 수 있지만 당신이 **어떻게 사용한하는지 충분히 아는 만큼**만 쓸 수 있습니다. 이에 대한 학습 곡선은 가파르고, 이 관점에서 우리는 모든것이 어떻게 동작하는지 세부적으로 설명하는 위키를 당신이 읽어주시길 바랍니다.

위의 텍스트를 견뎌내고 여전히 여기 있네요? 좋아요. 막 넘기지 않았다면 이제 곧 **[끔찍한 시간](https://www.youtube.com/watch?v=WJgt6m6njVw)**이 될겁니다... 어쨌건간에, ASF는 평소에 쓰건 친숙한 GUI가 없는 콘솔 프로그램입니다. ASF는 주로 서버에서 돌아가도록 되어있고, 데스크탑 프로그램이 아닌 서비스(대몬)으로 동작합니다.

하지만 PC에서 사용할 수 없다거나 사용법이 뭔가 더 복잡하다거나 뭐 그런 뜻은 아닙니다. ASF는 설치가 필요없는 독립실행 프로그램으로, 상자에서 꺼내면 바로 작동합니다. 하지만 쓸만해지려면 설정이 필요합니다. 설정은 ASF가 실행된 후에 실제로 뭘 해야하는지를 알려주는 일입니다. 설정하지 않고 실행하면 ASF는 아무 것도 하지 않습니다. 간단하죠.

* * *

## 특정 OS에 설치하기

일반적으로 다음 몇분동안 할 일의 목록입니다:

- **[.NET Core 필수 구성 요소](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)** 설치
- 특정 OS에 맞는 **[최신 버전 ASF](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** 다운로드
- 압축파일을 새 위치에 풀기(리눅스/OS X라면 `chmod +x ArchiSteamFarm` 실행)
- **[Configure ASF](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)**.
- ASF를 실행하고 마법을 경험하세요

꽤 간단한 것 같죠? 자 이제 해봅시다.

* * *

### .NET Core 필수 구성 요소

첫 번째 단계는 당신의 OS가 ASF를 제대로 실해할 수 있는지를 확인하는 것입니다. ASF는 C#으로 작성되었고 .NET Core를 기반으로 하므로 당신의 플랫폼에서 아직 사용할 수 없는 네이티브 라이브러리를 필요로 할 수도 있습니다. 윈도우, 리눅스, OS X 어떤 것을 쓰는지에 따라 필요조건이 달라지는데, **[.NET Core 필수 구성 요소](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)** 문서에 모두 나열되어 있으니 따라하시기 바랍니다.다 This is our reference material that should be used, but for the sake of simplicity we've also detailed all needed packages below, so you don't need to read the full document.

It's perfectly normal that some (or even all) dependencies already exist on your system due to being installed by third-party software that you're using. Still, you should ensure that it's truly the case by running appropriate installer for your OS - without those dependencies ASF won't launch at all.

특정 OS용 빌드는 이미 모든 것을 포함하고 있으므로 .NET Core SDK나 런타임의 설치 등 다른 어떤 것도 할 필요가 없다는 것을 명심하십시오. ASF에 포함된 .NET Core 런타임을 실행하기 위해서는 .NET Core 필수 구성 요소(종속 프로그램)만 필요합니다.

#### **[윈도우](https://docs.microsoft.com/ko-kr/dotnet/core/windows-prerequisites?tabs=netcore2x)**:

- **[Microsoft Visual C++ 2015 재배포 가능 패키지 Update 3 RC](https://www.microsoft.com/ko-kr/download/details.aspx?id=52685)** (64비트 윈도우는 x64를, 32비트 윈도우는 x86을 선택하십시오.)
- 모든 윈도우 업데이트를 미리 설치해 놓는 것을 매우 권장합니다. 적어도 **[KB2533623](https://support.microsoft.com/ko-kr/help/2533623/microsoft-security-advisory-insecure-library-loading-could-allow-remot)**과 **[KB2999226](https://support.microsoft.com/ko-kr/help/2999226/update-for-universal-c-runtime-in-windows)**은 필수이고 더 많은 업데이트가 필요할 수 있습니다. 윈도우가 최신 상태라면 모든 것이 설치되어 있을 것입니다. Visual C++ 패키지를 설치하기 전에 요구사항을 충족하는지 확인하십시오.

#### **[리눅스](https://docs.microsoft.com/ko-kr/dotnet/core/linux-prerequisites?tabs=netcore2x)**:

Package names depend on the Linux distribution that you're using, we've listed the most common ones. You can obtain all of them with native package manager for your OS (such as `apt` for Debian or `yum` for CentOS).

- libcurl3 (libcurl)
- libicu60 (libicu, latest version for your distribution, for example `libicu57` for Debian 9)
- libkrb5-3 (krb5-libs)
- liblttng-ust0 (lttng-ust)
- libssl1.0.2 (libssl, openssl-libs, 배포판의 최신 1.0.X 버전)
- zlib1g (zlib)

이 중 적어도 몇개는 이미 설치되어 있을 것입니다. (오늘날 거의 모든 리눅스 배포판의 필수요소인 zlib1g 같은 것들)

#### **[OS X](https://docs.microsoft.com/en-us/dotnet/core/macos-prerequisites?tabs=netcore2x)**:

- None for now

* * *

### 다운로드

Since we have all required dependencies already, the next step is downloading **[latest ASF release](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)**. ASF is available in many variants, but you're interested in package that matches your operating system and architecture. For example, if you're using `64`-bit `Win`dows, then you want `ASF-win-x64` package. For more information about available variants, visit **[compatibility](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility)** section. ASF is also able to run on OSes that we're not building OS-specific package for, such as **32-bit Windows**, head over to **[generic setup](#generic-setup)** for that.

![Assets](https://i.imgur.com/Ym2xPE5.png)

Once you get your package and extract the zip file (we recommend using **[7-zip](https://www.7-zip.org)**), you'll have a huge mess of folders and files. Don't worry, we'll clean it up in a second.

If you're using Linux/OS X, don't forget to `chmod +x ArchiSteamFarm`, since permissions are not automatically set in the zip file. This has to be done only once after initial unpack.

Be advised to unpack ASF to **its own directory** and not to any existing directory you're already using for something else - ASF's auto-updates feature will delete all old and unrelated files when upgrading, which might lead to you losing anything unrelated you put in ASF directory. If you have any extra scripts or files that you want to use with ASF, put them in one folder above.

An example structure would look like this:

    C:\ASF (필요한 스크립트나 파일은 여기에 넣으십시오)
        ├── ASF shortcut.lnk (선택사항)
        ├── Config shortcut.lnk (선택사항)
        ├── Commands.txt (선택사항)
        ├── MyExtraScript.bat (선택사항)
        ├── ... (그외 기타 등등, 선택사항)
        └── Core (압축을 푼 곳, ASF 전용)
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

### 설정

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

### 요약

You've successfully set up ASF to use your Steam accounts and you've already customized it to your liking a little. If you followed our entire guide, then you even managed to send a simple command through our ASF-ui interface. Now is a good time to read our entire **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** section in order to learn what all those different settings you've seen in advanced tab actually do, and what ASF can offer. If you've stumbled upon some issue or you have some generic question, read **[FAQ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ)** instead which should cover all, or at least majority of questions that you might have. If you want to learn everything about ASF and how it can make your life easier, head over to the rest of **[our wiki](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Home)**. Have fun!

* * *

## 일반 설치

This setup is for advanced users that want to set up ASF to run in **[generic](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#generic)** variant. It's not recommended for people that can use **[OS-specific setup](#os-specific-setup)**.

You want to use generic variant mainly in three situations (but of course you can use it regardless):

- When you're using OS that we don't build OS-specific package for (such as 32-bit Windows)
- When you already have .NET Core Runtime/SDK, or want to install and use one
- When you want to minimize ASF structure size by handling runtime requirements yourself

However, keep in mind that you're in charge of .NET Core runtime in this case. This means that if your .NET Core SDK (runtime) is unavailable, outdated or broken, ASF won't work. This is why we don't recommend this setup for casual users, since you now need to ensure that your .NET Core SDK (runtime) matches ASF requirements and can run ASF, as opposed to **us** ensuring that our .NET Core runtime bundled with ASF can do so.

For generic package, you can follow entire OS-specific guide above, with two small changes. In addition to installing .NET Core prerequisites, you also want to install .NET Core SDK, and instead of having OS-specific `ArchiSteamFarm(.exe)` executable file, you now have a generic `ArchiSteamFarm.dll` binary only. Everything else is exactly the same.

With extra steps:

- **[.NET Core 필수 구성 요소](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)** 설치
- Install **[.NET Core SDK](https://www.microsoft.com/net/download)** (or at least runtime) appropriate for your OS. You most likely want to use an installer. Refer to **[runtime requirements](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)** if you're not sure which version to install.
- Download **[latest ASF release](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** in generic variant.
- Extract the archive into new location (and `chmod +x ArchiSteamFarm.sh` if you're on Linux/OS X).
- **[Configure ASF](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)**.
- Launch ASF by either using a helper script or executing `dotnet /path/to/ArchiSteamFarm.dll` manually from your favourite shell.

Helper scripts (such as `ArchiSteamFarm.cmd` for Windows and `ArchiSteamFarm.sh` for Linux/OS X) are located next to `ArchiSteamFarm.dll` binary - those are included in generic variant only. You can use them if you don't want to execute `dotnet` command manually. You can also make a shortcut to those scripts like showed above, since they're supposed to provide binary replacement in a script way. Obviously helper scripts won't work if you didn't install .NET Core SDK and you don't have `dotnet` executable available in your `PATH`. Helper scripts are entirely optional to use, you can always `dotnet /path/to/ArchiSteamFarm.dll` manually.