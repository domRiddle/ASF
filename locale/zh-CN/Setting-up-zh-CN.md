# 初始设置

If you arrived here for the first time, welcome! We're very happy to see yet another traveler that is interested in our project, although bear in mind that with great power comes great responsibility - ASF is capable of doing a lot of different Steam-related things, but only as long as you **care enough to learn how to use it**. There is a steep learning curve involved here, and we expect from you to read the wiki in this regard, which explains in detail how everything operates.

If you're still here then it means that you endured our text above, which is nice. Unless you skipped over it, then you're going to have a **[bad time](https://www.youtube.com/watch?v=WJgt6m6njVw)** soon enough... Anyway, ASF is a console app, which means that the program itself doesn't have a friendly GUI that you're in general used to. ASF was mainly supposed to be run on servers, so it acts as a service (daemon) and not a desktop app.

This however doesn't mean that you can't use it on your PC or using it is in some way more complicated than usual, nothing like that. ASF is a standalone program that doesn't need installation, and works out of the box right away, but requires configuration prior to becoming useful. Configuration is telling ASF what it should in fact do after you launch it. If you launch it without configuration, then ASF won't do anything, simple.

* * *

## OS-specific 设置

通常，我们只需要花费几分钟进行下列操作：

- 安装 **[.NET Core 依赖项](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)**。
- 在 &#8203;**[ASF 发布页面](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)**&#8203;下载合适的 OS-specific 包。
- 将下载的压缩包解压到新位置，如果您使用 Linux/OS X，还需要执行命令 `chmod +x ArchiSteamFarm`。
- **[配置 ASF](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-CN)**。
- 运行并开始使用 ASF。

看起来很简单吧？ 现在我们开始逐项解释。

* * *

### .NET Core 依赖

First step is ensuring that your OS can even launch ASF properly. ASF is written in C#, based on .NET Core and might require native libraries that are not available on your platform yet. Depending on whether you use Windows, Linux or OS X, you will have different requirements, although all of them are listed in **[.NET Core prerequisites](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)** document that you should follow. This is our reference material that should be used, but for the sake of simplicity we've also detailed all needed packages below, so you don't need to read the full document.

It's perfectly normal that some (or even all) dependencies already exist on your system due to being installed by third-party software that you're using. Still, you should ensure that it's truly the case by running appropriate installer for your OS - without those dependencies ASF won't launch at all.

Keep in mind that you don't need to do anything else for OS-specific builds, especially installing .NET Core SDK or even runtime, since OS-specific package includes all of that already. You need only .NET Core prerequisites (dependencies) to run .NET Core runtime included in ASF.

#### **[Windows](https://docs.microsoft.com/en-us/dotnet/core/windows-prerequisites?tabs=netcore2x)**：

- **[Microsoft Visual C++ 2015 Redistributable Update 3 RC](https://www.microsoft.com/en-us/download/details.aspx?id=52685)** (x64 for 64-bit Windows, x86 for 32-bit Windows)
- It's highly recommended to ensure that all Windows updates are already installed. At the very least you need **[KB2533623](https://support.microsoft.com/en-us/help/2533623/microsoft-security-advisory-insecure-library-loading-could-allow-remot)** and **[KB2999226](https://support.microsoft.com/en-us/help/2999226/update-for-universal-c-runtime-in-windows)**, but more updates might be needed. All of them are already installed if your Windows is up-to-date. Ensure that you meet those requirements prior to installing Visual C++ package.

#### **[Linux](https://docs.microsoft.com/en-us/dotnet/core/linux-prerequisites?tabs=netcore2x)**：

Package names depend on the Linux distribution that you're using, we've listed the most common ones. You can obtain all of them with native package manager for your OS (such as `apt` for Debian or `yum` for CentOS).

- libcurl3（libcurl）
- libicu60（libicu，您使用的发行版上的最新版，例如 Debian 9 上的 `libicu57`）
- libkrb5-3（krb5-libs）
- liblttng-ust0（lttng-ust）
- libssl1.0.2（libssl，openssl-libs，您使用的发行版上的最新 1.0.X 版本）
- zlib1g（zlib）

At least a few of those should be already natively available on your system (such as zlib1g that is required in almost every Linux distro today).

#### **[OS X](https://docs.microsoft.com/en-us/dotnet/core/macos-prerequisites?tabs=netcore2x)**：

- 暂无

* * *

### 下载

Since we have all required dependencies already, the next step is downloading **[latest ASF release](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)**. ASF is available in many variants, but you're interested in package that matches your operating system and architecture. For example, if you're using `64`-bit `Win`dows, then you want `ASF-win-x64` package. 请阅读&#8203;**[兼容性](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-zh-CN)**&#8203;章节了解关于不同操作系统版本的详情。 ASF 也可以运行在我们尚未提供官方版本的操作系统上，例如 **32 位 Windows**，请参考 **[Generic 包设置](#generic-包设置)**。

![Assets](https://i.imgur.com/Ym2xPE5.png)

Once you get your package and extract the zip file (we recommend using **[7-zip](https://www.7-zip.org)**), you'll have a huge mess of folders and files. Don't worry, we'll clean it up in a second.

If you're using Linux/OS X, don't forget to `chmod +x ArchiSteamFarm`, since permissions are not automatically set in the zip file. This has to be done only once after initial unpack.

Be advised to unpack ASF to **its own directory** and not to any existing directory you're already using for something else - ASF's auto-updates feature will delete all old and unrelated files when upgrading, which might lead to you losing anything unrelated you put in ASF directory. If you have any extra scripts or files that you want to use with ASF, put them in one folder above.

An example structure would look like this:

    C:\ASF (放置您所有与 ASF 相关的东西)
        ├── ASF shortcut.lnk (ASF 的快捷方式，可选)
        ├── Config shortcut.lnk (配置的快捷方式，可选)
        ├── Commands.txt (您记录的一些命令，可选)
        ├── MyExtraScript.bat (一些您使用的相关脚本，可选)
        ├── ... (总之这里是您自己存放的一些与 ASF 有关的东西，都是可选的)
        └── Core (ASF 本身使用的文件夹，解压 ASF 安装包的地方)
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

### 配置

We're now ready to do the very last step, the configuration. This is by far the most complicated step, since it involves a lot of new information you're not familiar with yet, so we'll try to provide some easy to understand examples and simplified explanation here.

首先，**[配置](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-CN)**&#8203;页面解释了关于配置 ASF 的**一切**，但是其中的选项太多了，我们现在不需要马上全部理解。 Instead, we'll teach you how to get the information you're actually looking for.

ASF configuration can be done in two ways - either by using our web config generator, or manually. 这已经在&#8203;**[配置](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-CN)**&#8203;章节中进行了深入解释，所以如果您想了解详情可以前往阅读。 We'll use web config generator way, since it's much easier.

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

ASF requires your login credentials because it includes its own implementation of Steam client and needs the same details to log in as the one that you use yourself. Your login credentials are not saved anywhere but on your PC in ASF `config` directory only, our web config generator is client-based which means that the code is run locally in your browser to generate valid ASF configs, without details you're inputting ever leaving your PC in the first place, so there is no need to worry about any possible sensitive data leak. Still, if you for whatever reason don't want to put your credentials there, we understand that, and you can put them manually later in generated files, or omit them entirely and put them only in ASF command prompt. 更多相关的安全性问题可以在&#8203;**[配置](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-CN)**&#8203;章节中找到。

You can also decide to leave just one field empty, such as `SteamPassword`, ASF will then be able to use your login automatically, but will still ask for password (similar to Steam Client). If you're using Steam parental to unlock the account, you'll need to put it into `SteamParentalCode` field.

After the decision and optional details, your web page will now look similar to the one below:

![Bot tab 2](https://i.imgur.com/yf54Ouc.png)

You can now hit "download" button and our web config generator will generate new `json` file based on your chosen name. Save that file into `config` directory of ASF. You can use previously-created `config` shortcut, or find `config` directory manually, directly in ASF file structure.

Your `config` directory will now look like this:

![Structure 2](https://i.imgur.com/2s7ZUUu.png)

Congratulations! You've just finished the very basic ASF bot configuration. We'll extend this shortly, for now this is everything that you need.

* * *

### 运行 ASF

You're now ready to launch the program for the first time. Simply double-click ASF shortcut, or `ArchiSteamFarm(.exe)` binary in ASF directory.

After doing so, assuming you installed all required dependencies in the first step, ASF should launch properly, notice your first bot (if you didn't forget to put generated config in `config` directory), and attempt to log in:

![ASF](https://i.imgur.com/u5hrSFz.png)

If you supplied `SteamLogin` and `SteamPassword` for ASF to use, you'll be asked for your SteamGuard token only (e-mail, 2FA or none, depending on your Steam settings). If you didn't, you'll also be asked for your Steam login and password.

如果担心如 ASF 所述的接下来会发生的事情，现在您可以审查我们的&#8203;**[隐私政策](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Statistics#current-privacy-policy)**。

After passing through initial login gate, assuming your details are correct, you'll successfully log in, and ASF will start idling using default settings that you didn't change as of now:

![ASF 2](https://i.imgur.com/Cb7DBl4.png)

This proves that ASF is now successfully doing its job on your account, so you can now minimize the program and do something else. 经过足够的时间之后（取决于&#8203;**[性能](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance-zh-CN)**），您将会看到 Steam 卡牌逐渐掉落。 当然，要做到这一点，您必须有可以挂卡的游戏，您可以在您的&#8203;**[徽章页面](https://steamcommunity.com/my/badges)**&#8203;上看到这些游戏会标注“X 张剩余卡牌掉落”——如果没有可挂卡的游戏，ASF 就会无事可做，如&#8203;**[常见问题](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ-zh-CN#那么它到底是如何工作的呢)**&#8203;中所述。

This concludes our very basic setting up guide. You can now decide whether you want to configure ASF further, or let it do its job in default settings. We'll cover a few more basic details, then leave you entire wiki for discovery.

* * *

### 进一步配置

#### 同时挂多个帐户

ASF supports idling more than one account at a time, which is its primary function. You can add more accounts to ASF by generating more bot config files, in exactly the same way as you've generated your first one just a few minutes ago. You need to ensure only two things:

- Unique bot name, if you already have your first bot named "MainAccount", you can't have another one with the same name.
- Valid login details, such as `SteamLogin`, `SteamPassword` and `SteamParentalCode` (if using Steam parental settings)

In other words, simply jump to configuration again and do exactly the same, just for your second or third account. Remember to use unique names for all of your bots.

* * *

#### 更改设置

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

#### 使用 ASF-ui

ASF is a console app and doesn't include a graphical user interface. 然而，现在已有 IPC 接口的前端 **[ASF-ui](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-zh-CN#asf-ui)**，它仍然处于预览状态，但是一般使用应该已经没有问题了。

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

If you did everything properly, you'll now be able to access ASF's IPC interface under **[this](http://127.0.0.1:1242)** link, as long as ASF is running. 您可以使用 ASF-ui 进行各种操作，例如发送&#8203;**[命令](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-CN)**。 Feel free to take a look around in order to find out all ASF-ui functionalities.

![IPC 3](https://i.imgur.com/vCu2ZY5.png)

Please note that ASF-ui is currently in preview state and not everything is available/working yet, but it's more than enough for simple ASF usage.

* * *

### 总结

You've successfully set up ASF to use your Steam accounts and you've already customized it to your liking a little. If you followed our entire guide, then you even managed to send a simple command through our ASF-ui interface. 现在您可以阅读完整的&#8203;**[配置](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-CN)**&#8203;章节了解所有 ASF 的高级选项，以及 ASF 有哪些功能。 If you've stumbled upon some issue or you have some generic question, read **[FAQ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ)** instead which should cover all, or at least majority of questions that you might have. 如果您希望了解 ASF 的一切以及 ASF 如何为您提供帮助，请继续阅读我们的 **[Wiki](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Home-zh-CN)**。 Have fun!

* * *

## Generic 包设置

这些设置是为想要使用 ASF **[Generic](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-zh-CN#generic)** 包的高级用户准备的。 如果您可以使用 **[OS-specific 设置](#os-specific-设置)**，就不建议您使用 Generic 包。

You want to use generic variant mainly in three situations (but of course you can use it regardless):

- When you're using OS that we don't build OS-specific package for (such as 32-bit Windows)
- When you already have .NET Core Runtime/SDK, or want to install and use one
- When you want to minimize ASF structure size by handling runtime requirements yourself

However, keep in mind that you're in charge of .NET Core runtime in this case. This means that if your .NET Core SDK (runtime) is unavailable, outdated or broken, ASF won't work. This is why we don't recommend this setup for casual users, since you now need to ensure that your .NET Core SDK (runtime) matches ASF requirements and can run ASF, as opposed to **us** ensuring that our .NET Core runtime bundled with ASF can do so.

For generic package, you can follow entire OS-specific guide above, with two small changes. In addition to installing .NET Core prerequisites, you also want to install .NET Core SDK, and instead of having OS-specific `ArchiSteamFarm(.exe)` executable file, you now have a generic `ArchiSteamFarm.dll` binary only. Everything else is exactly the same.

With extra steps:

- 安装 **[.NET Core 依赖项](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)**。
- 安装适合您操作系统的 **[.NET Core SDK](https://www.microsoft.com/net/download)**（或至少安装运行时环境）。 您可能需要使用一个安装器。 如果您不确定应该安装哪个版本，请参考&#8203;**[运行时环境需求](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-zh-CN#运行时环境需求)**。
- 在 **[ASF 发布页面](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)**&#8203;下载 Generic 包。
- 将下载的压缩包解压到新位置，如果您使用 Linux/OS X，还需要执行命令 `chmod +x ArchiSteamFarm.sh`。
- **[配置 ASF](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-CN)**。
- 使用助手脚本或者在 shell 中执行 `dotnet /path/to/ArchiSteamFarm.dll` 启动 ASF。

Helper scripts (such as `ArchiSteamFarm.cmd` for Windows and `ArchiSteamFarm.sh` for Linux/OS X) are located next to `ArchiSteamFarm.dll` binary - those are included in generic variant only. You can use them if you don't want to execute `dotnet` command manually. You can also make a shortcut to those scripts like showed above, since they're supposed to provide binary replacement in a script way. Obviously helper scripts won't work if you didn't install .NET Core SDK and you don't have `dotnet` executable available in your `PATH`. Helper scripts are entirely optional to use, you can always `dotnet /path/to/ArchiSteamFarm.dll` manually.