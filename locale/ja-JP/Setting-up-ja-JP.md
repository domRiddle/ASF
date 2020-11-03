# 初期設定

はじめての方、ようこそ！ 私たちのプロジェクトに興味を持ってくださる方がまた一人増えたことをとても嬉しく思います。能力に努力はつきものだ、もしあなたが** ASF の使用方法を充分学んだのなら**、ASFはSteamに関連した様々なことを行うことができます。 ここには急な学習曲線がありますので、この点に関してはwikiを読んでいただくことを期待しています。

あなたがまだここにいるということは、上記のテキストに耐えてくれたということですね。嬉しい。 これをサボっていたら、すぐに**[嫌な思い](https://youtu.be/LCZUMw00XHU?t=1)**をすることになりますよ… ともかく、ASFはコンソールアプリなので、プログラムには一般的に使われているようなフレンドリーなGUIは持っていません。 ASF は主にサーバ上で実行されることを想定していたので、デスクトップアプリではなくサービス（デーモン）として動作します。

しかし、これはあなたの PC で使用できなかったり、使用することが通常よりも複雑だったりすることを意味するものではありません。 ASF はインストールを必要としないスタンドアロンのプログラムで、OOTB（箱から出してすぐに動作します）が、使えるようにするには設定が必要です。 設定とは、ASF を起動した後に何をさせるかを指示することです。 設定なしで起動しても 、ASF は何もしません。

* * *

## OS 固有の設定

一般的には、次のようにします。

- **[.NET Core の前提条件](#net-core-prerequisites)**をインストールします。
- **[最新の ASF リリース](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)**を適切なOS固有のバリアントでダウンロードします。
- アーカイブファイルを展開します(Linux/OS Xの場合は`chmod +x ArchiSteamFarm`を)。
- **[ASF を設定します](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)**
- ASF を起動し、奇跡の瞬間を目に焼き付けなさい。

簡単っしょ？ では、早速やってみましょう。

* * *

### .NET Core の前提条件

まず最初に、お使いのOSでASFが正しく起動できるかを確認して下さい。 ASF は .NET Core をベースに C# で書かれており、お使いのプラットフォームがまだサポートしていない、ネイティブライブラリを必要とする場合があります。 Windows、Linux、OS Xのいずれを使用しているかによって必要な要件は異なりますが、すべての要件は **[ .NET Core prerequisites](https://docs.microsoft.com/dotnet/core/install)** ページに記載されています これはマイクロソフトによる説明ですが、簡単にするために、必要なものを以下にも書きますので、リンク先の全文を読む必要はありません。

ASFが依存するソフトは、他のソフトによってすでにインストールされたことが普通です。 それでも、ASFが確実に作動するために、.NET Core prerequisites を再度インストールすることが必要とされることもある。

固有の OS-specific build（ダウンロードしたご利用の OS に対応してビルドされたソフト）に対し何もする必要はありません。特に .NET Core SDK のインストールやランタイムのインストールなど、これらは OS-specific package（ダウンロードしたやつ）に内包されています。 ご自身でやっていただくことは、 .NET Core が作動する前提条件（依存された環境）だけです。

#### **[Windows](https://docs.microsoft.com/dotnet/core/install/windows)**：

- **[Microsoft Visual C++ 2015 Redistributable Update](https://www.microsoft.com/en-us/download/details.aspx?id=53587)**（64ビットWindowsならx64、32ビットWindowsならx86）
- すべてのWindowsアップデートがすでにインストールされていることを確認することを強くお勧めします。 最低でも **[KB2533623](https://support.microsoft.com/en-us/help/2533623/microsoft-security-advisory-insecure-library-loading-could-allow-remot)** と **[KB2999226](https://support.microsoft.com/en-us/help/2999226/update-for-universal-c-runtime-in-windows)** が必要ですが、これ以外のアップデートが必要の場合もある。 Windowsが最新の状態であれば、これらはすべてインストールされています。 Visual C++パッケージをインストールする前に、これらの要件を満たしていることを確認してください。

#### **[Linux](https://docs.microsoft.com/dotnet/core/install/linux)**：

パッケージ名は使用している Linux ディストリビューションに依存しますが、ここでは最も一般的なものをリストアップしました。 あなたの OS のネイティブのパッケージマネージャ（例えば Debian なら `apt`、CentOS なら `yum`）を使っても、これらをインストールすることが可能です。

- `libcurl` (`libcurl4`, `libcurl3`)
- `libicu` (ご利用のディストリビューションの最新バージョン、例えば `libicu60`)
- `libkrb5-3` (`krb5-libs`)
- `liblttng-ust0` (`lttng-ust`)
- `libssl` (`libssl1.1`, `openssl-libs`, ご利用の OS の最新の1.1.X版)
- `zlib1g` (`zlib`)

少なくとも、これらのうちのいくつかはシステム上でネイティブに利用可能なはずです（最近ではほぼすべての Linux ディストリビューションで `zlib1g` がすでに入っている）。

#### **[OS X](https://docs.microsoft.com/dotnet/core/install/macos)**:

- 今のところはありませんが、OS X の最新バージョン、少なくとも 10.13+以上が必要です。

* * *

### ダウンロード

Since we have all required dependencies already, the next step is downloading **[latest ASF release](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)**. ASF is available in many variants, but you're interested in package that matches your operating system and architecture. For example, if you're using `64`-bit `Win`dows, then you want `ASF-win-x64` package. For more information about available variants, visit **[compatibility](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility)** section. ASF is also able to run on OSes that we're not building OS-specific package for, such as **32-bit Windows**, head over to **[generic setup](#generic-setup)** for that.

![Assets](https://i.imgur.com/Ym2xPE5.png)

After download, start from extracting the zip file into its own folder. We recommend using **[7-zip](https://www.7-zip.org)**, but all standard utilities like `unzip` from Linux/OS X should work without problems as well.

If you're using Linux/OS X, don't forget to `chmod +x ArchiSteamFarm` in the extracted folder, since permissions are not automatically set in the zip file. This has to be done only once after initial unpack.

Be advised to unpack ASF to **its own directory** and not to any existing directory you're already using for something else - ASF's auto-updates feature will delete all old and unrelated files when upgrading, which may lead to you losing anything unrelated you put in ASF directory. If you have any extra scripts or files that you want to use with ASF, put them in one folder above.

An example structure would look like this:

```text
C:\ASF (where you put your own things)
    ├── ASF shortcut.lnk (optional)
    ├── Config shortcut.lnk (optional)
    ├── Commands.txt (optional)
    ├── MyExtraScript.bat (optional)
    ├── (...) (any other files of your choice, optional)
    └── Core (dedicated to ASF only, where you extract the archive)
         ├── ArchiSteamFarm(.exe)
         ├── config
         ├── logs
         ├── plugins
         └── (...)
```

* * *

### 設定

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

![Structure 2](https://i.imgur.com/crWdjcp.png)

Congratulations! You've just finished the very basic ASF bot configuration. We'll extend this shortly, for now this is everything that you need.

* * *

### Running ASF

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

![プロセス間通信](https://i.imgur.com/NhujZCN.png)

Now you can download your ASF config and put it in your `config` directory, as usual. Afterwards, launch ASF again, and you should be able to confirm that it properly started IPC interface:

![IPC 2](https://i.imgur.com/ZmkO8pk.png)

If you did everything properly, you'll now be able to access ASF's IPC interface under **[this](http://localhost:1242)** link, as long as ASF is running. You can use ASF-ui for various purposes, e.g. sending **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. Feel free to take a look around in order to find out all ASF-ui functionalities.

![ASF-ui](https://raw.githubusercontent.com/JustArchiNET/ASF-ui/master/.github/previews/bots.png)

Please note that ASF-ui is currently in preview state and not everything is available/working yet, but it's more than enough for simple ASF usage.

* * *

### Summary

You've successfully set up ASF to use your Steam accounts and you've already customized it to your liking a little. If you followed our entire guide, then you even managed to send a simple command through our ASF-ui interface. Now is a good time to read our entire **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** section in order to learn what all those different settings you've seen in advanced tab actually do, and what ASF can offer. If you've stumbled upon some issue or you have some generic question, read **[FAQ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ)** instead which should cover all, or at least majority of questions that you may have. If you want to learn everything about ASF and how it can make your life easier, head over to the rest of **[our wiki](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Home)**. Have fun!

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

- Install **[.NET Core prerequisites](https://docs.microsoft.com/dotnet/core/install/dependencies?tabs=netcore31)**.
- Install **[.NET Core SDK](https://www.microsoft.com/net/download)** (or at least runtime) appropriate for your OS. You most likely want to use an installer. Refer to **[runtime requirements](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)** if you're not sure which version to install.
- Download **[latest ASF release](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** in generic variant.
- Extract the archive into new location (and `chmod +x ArchiSteamFarm.sh` if you're on Linux/OS X).
- **[ASF を設定します](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)**
- Launch ASF by either using a helper script or executing `dotnet /path/to/ArchiSteamFarm.dll` manually from your favourite shell.

Helper scripts (such as `ArchiSteamFarm.cmd` for Windows and `ArchiSteamFarm.sh` for Linux/OS X) are located next to `ArchiSteamFarm.dll` binary - those are included in generic variant only. You can use them if you don't want to execute `dotnet` command manually. Obviously helper scripts won't work if you didn't install .NET Core SDK and you don't have `dotnet` executable available in your `PATH`. Helper scripts are entirely optional to use, you can always `dotnet /path/to/ArchiSteamFarm.dll` manually.