# 初期設定

はじめての方、ようこそ！ 私たちのプロジェクトに興味を持ってくださる方がまた一人増えたことをとても嬉しく思います。能力に努力はつきものだ、もしあなたが** ASF の使用方法を充分学んだのなら**&#8203;、ASFはSteamに関連した様々なことを行うことができます。 ここには急な学習曲線がありますので、この点に関してはwikiを読んでいただくことを期待しています。

あなたがまだここにいるということは、上記のテキストに耐えてくれたということですね。嬉しい。 これをサボっていたら、すぐに&#8203;**[嫌な思い](https://youtu.be/LCZUMw00XHU?t=1)**&#8203;をすることになりますよ… Anyway, ASF is a console app, which means that the program itself doesn't have a friendly GUI that you're in general used to, at least out of the box. ASF は主にサーバ上で実行されることを想定していたので、デスクトップアプリではなくサービス（デーモン）として動作します。

しかし、これはあなたの PC で使用できなかったり、使用することが通常よりも複雑だったりすることを意味するものではありません。 ASF はインストールを必要としないスタンドアロンのプログラムで、OOTB（箱から出してすぐに動作します）が、使えるようにするには設定が必要です。 設定とは、ASF を起動した後に何をさせるかを指示することです。 設定なしで起動しても 、ASF は何もしません。<br><br> 訳注（用語説明）：<br> ・.NET Core の要件・依存関係 (.NET Core prerequisites)、.NET Coreアプリが作動する環境。<br> ・OS固有バリアント・OS固有パッケージ(OS-specific variant/OS-specific package)、コードをOSが実行できるよう予め作られたやつ、ユーザーがダウンロードするやつ。<br> （もっといい訳し方を知っている方はぜひ拙訳を直していただきたい。）<br>

---

## OS 固有の設定

一般的には、次のようにします。
- **[.NET Core の前提条件](#net-core-prerequisites)**&#8203;をインストールします。
- **[最新の ASF リリース](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)**&#8203;を適切な OS 固有バリアントでダウンロードします。
- アーカイブファイルを展開します(Linux/OS X の場合は `chmod +x ArchiSteamFarm` を)。
- **[ASF を設定します](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-ja-JP)**
- ASF を起動し、奇跡の瞬間を目に焼き付けなさい。

簡単っしょ？ では、早速やってみましょう。

---

### .NET Core の要件

まずは、お使いの OS で ASF が正しく起動できるかを確認して下さい。 ASF は .NET Core をベースに C# で書かれており、お使いのプラットフォームがまだサポートしていない、ネイティブライブラリを必要とする場合があります。 Windows、Linux、OS X のいずれを使用しているかによって必要な要件は異なりますが、すべての要件は **[ .NET Core prerequisites](https://docs.microsoft.com/dotnet/core/install)** ページに記載されています。 これはマイクロソフトによる公式的な説明ですが、簡単にするために、必要なものを以下にも書きますので、リンク先の全文を読まなくて結構です。

ASF が依存するソフトが、他のソフトによってすでに一部または全てインストールされていることが普通です。 それでも、ASFが確実に作動するために、依存関係を再度インストールすることが必要とされることもある。

既存の OS 固有ビルドに対し何もする必要はありません。特に .NET Core SDK のインストールやランタイムのインストールなど、これらは OS 固有パッケージ（ダウンロードしたやつ）に内包されています。 ご自身でやっていただくことは、.NET Core が作動する要件（依存された環境）だけです。

#### **[Windows](https://docs.microsoft.com/dotnet/core/install/windows)**
- **[Microsoft Visual C++ 2015 Redistributable Update](https://www.microsoft.com/en-us/download/details.aspx?id=53587)**&#8203;（64ビットWindowsならx64、32ビットWindowsならx86）
- すべてのWindowsアップデートがすでにインストールされていることを確認することを強くお勧めします。 最低でも **[KB2533623](https://support.microsoft.com/en-us/help/2533623/microsoft-security-advisory-insecure-library-loading-could-allow-remot)** と **[KB2999226](https://support.microsoft.com/en-us/help/2999226/update-for-universal-c-runtime-in-windows)** が必要ですが、これ以外のアップデートが必要の場合もある。 Windowsが最新の状態であれば、これらはすべてインストールされています。 Visual C++ パッケージをインストールする前に、これらの要件を満たしていることを確認してください。

#### **[Linux](https://docs.microsoft.com/dotnet/core/install/linux)**
パッケージ名は使用している Linux ディストリビューションに依存しますが、ここでは最も一般的なものをリストアップしました。 あなたの OS のネイティブのパッケージマネージャ（例えば Debian なら `apt`、CentOS なら `yum`）を使っても、これらをインストールすることが可能です。

- `libc6` (`libc`)
- `libgcc1` (`libgcc`)
- `libicu` (`icu-libs`, latest version for your distribution, for example `libicu67`)
- `libgssapi-krb5-2` (`libkrb5-3`, `krb5-libs`)
- `libssl1.1` (`libssl`, `openssl-libs`, latest version for your distribution, `1.1.X` or `1.0.X`)
- `libstdc++6` (`libstdc++`, in version `5.0` or higher)
- `zlib1g` (`zlib`)

At least a majority of those should be already natively available on your system. The minimal installation of Debian stable required only `libicu63`.

#### **[OS X](https://docs.microsoft.com/dotnet/core/install/macos)**
- ユーザーは何もせずとも、アップルが用意してくれている。ただし、OS X の最新バージョン、少なくとも 10.13 以上が必要です。

---

### ダウンロード

前の節で必要な依存関係はすべて揃っているはずので、本節では&#8203;**[最新の ASF リリース](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)**&#8203;をダウンロードします。 ASF には多くの種類がありますが、ご利用の OS やアーキテクチャに合うパッケージを選んで下さい。 例えば、`64` ビット `Win`dows を使用している場合、`ASF-win-x64` パッケージをダウンロードして下さい。 利用可能なバリアントの詳細については、&#8203;**[互換性](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-ja-JP)**&#8203;セクションを参照してください。 ASFは **32ビットWindows** のような、我々がOS固有パッケージを提供していないOSでも動作することができます。詳しくは、&#8203;**[汎用設定](#generic-setup)**&#8203;を参照して下さい。

![Assets](https://i.imgur.com/Ym2xPE5.png)

ダウンロードしたら、まずは zip ファイルをお好きなフォルダに解凍して下さい。 We recommend using **[7-zip](https://www.7-zip.org)**, but all standard utilities like `unzip` from Linux/OS X should work without problems as well.

Linux/OS X を使っている場合は、解凍したフォルダで`chmod +x ArchiSteamFarm`を忘れずにやってください。 権限変更は解凍のあとに一回だけ必要です。

ASF を&#8203;**ほかに何も入っていないディレクトリ**&#8203;に解凍することをお勧めします。ASF の自動更新機能はアップグレード時に古いファイルや関係のないファイルをすべて削除するようになっているので、ASF ディレクトリにある関係のないファイルが削除される可能性があります。 ASFで使用するほかのスクリプトやファイルは、親ディレクトリに入れてください。

構造はこんな感じ。

```text
C:\ASF（ご自身のものを置く場所、例）
    ├── ASFのショートカット.lnk（例）
    ├── 設定のショートカット.lnk（例）
    ├── コマンドの下書き.txt（例）
    ├── 自分が書いたスクリプト.bat（例）
    ├── (...)（任意のほかのファイル）
    └── Core（ASF専用のディレクトリ、圧縮ファイルが解凍される場所）
         ├── ArchiSteamFarm(.exe）
         ├── config
         ├── logs
         ├── plugins
         └──(...)
```

---

### 設定

これで残ったのは配置することだけだ。 このステップが結構面倒くて、新しい点も多いから、ここでは分かりやすく、例を交えて説明しますね。

まず、&#8203;**[設定](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-ja-JP)**&#8203;ページでこれに関する&#8203;**全て**&#8203;が書かれているが、あまりにも情報量が莫大で、今はまだ読まなくていい。 代わりに、実際に必要な情報を得る方法を教えよう。

ASF configuration can be done in at least three ways - through our web config generator, ASF-ui or manually. この部分も&#8203;**[設定](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-ja-JP)**&#8203;ページで詳しく書かれている、詳しく知りたい方はそちらを。 We'll use web config generator as a starting point.

ブラウザで&#8203;**[ウェブ設定生成器](https://justarchinet.github.io/ASF-WebConfigGenerator)**&#8203;のページにアクセスして下さい。javascript を手動で無効化していたら有効にして下さい。 Chrome か Firefox を使うことをお勧めしますが、他のブラウザでも問題ないはず。

ページを開いたら、「Bot」タブに切り替えて下さい。 下のようなページが表示されているはずです。

![Bot tab](https://i.imgur.com/aF3k8Rg.png)

もしダウンロードした ASF のバージョンがウェブ設定生成器のデフォルトのバージョンよりも古い場合は、ドロップダウンメニューからお使いの ASF のバージョンを選択してください。 これは、設定生成器は、まだ安定していないプレリリース版の ASF の設定も生成できるからである。 安定版 ASF は検証済みなので、そちらをダウンロードして下さい。

まずは赤くハイライトされた入力欄にボットの名前を入力する。 ニックネーム、ユーザー名、数字、何でもよくて、お好きなように名前を付けて下さい。 但し、`ASF` とだけは名付けてはいけません。 その他にも、ASFはこれを無視するので、ボット名を「`.`」（点）で始まる名前にしてはいけません。 また、スペースの使用も避けたほうがいいと思われます。スペースの代わりに `_` を使って下さい。

名前が決まったら、`Enabled` スイッチをオンにしてください。このスイッチは、ASF 起動後に自動的にこのボットを起動するかどうかを定義するものです。

ここで、下の 2 つの中のどれにするか決めて下さい。
- Steamのアカウント名とパスワードをそれぞれ `SteamLogin` と `SteamPassword` 欄に入力する。
- これらを空白のままにする。

前者を選んだ場合、ASFは起動時に自動的にあなたのアカウント認証情報を使用し、毎回毎回手動で入力する必要はありません。 後者なら、アカウント情報は保存されませんので、ASFが起動するたびに手動でこれらを入力する手間が掛かる。

ASF には Steam クライアントの独自の実装が含まれているため、Steam 公式アプリと同じくログイン認証情報を必要とします。 ログイン認証情報はユーザーの PC にある ASF の下の `config` ディレクトリ内にのみ保存されます。ウェブ設定生成器はクライアント・ベースで、ブラウザ上でローカルにコードを実行します。ユーザーの認証情報はどこにも送られませんので、ユーザー情報が知られる心配はありません。 それでも、何らかの理由で認証情報をそこに書きたくない場合は（うん、わかる）、後で生成されたファイルに手動で書き入れるか、あるいは完全に省略して、起動するたんびに ASF コマンド・プロンプトに手動で入力することもできます。 セキュリティに関する詳細は**[設定&#8203;](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-ja-JP)**&#8203;ページを参照して下さい。

また、`SteamPassword` のように 1 つだけを空欄にするのも可能だ。その場合、ASF は自動的にログインしようとするが、Steam アプリと同様にパスワードを要求してきます。 ファミリービュー（保護者による制限）が有効化されたアカウントなら、`SteamParentalCode` 欄にそれに必要な コード を入力して下さい。

必要な操作をすべて完了させたら、Webページは以下のようになります。

![Bot tab 2](https://i.imgur.com/yf54Ouc.png)

「ダウンロード」ボタンを押すと、ボットの名前に基づいて新しい `json` ファイルが生成されます。 このファイルを前に解凍したフォルダにある `config` ディレクトリに保存して下さい。

`config` ディレクトリはこのようになります。

![Structure 2](https://i.imgur.com/crWdjcp.png)

ヨシッ！ これで ASF ボットの基本的な設定が完了しました。 このあとで詳しく説明するが、今のところこれで大丈夫です。

---

### ASF の実行

これでプログラムを初めて起動する準備ができました。 ASF ディレクトリ内の `ArchiSteamFarm` バイナリをダブルクリックして下さい。

最初のステップで必要な依存関係をすべてインストールしていたら、正常なら ASF は起動します。生成された設定を `config` ディレクトリに置くのを忘れていなければ、最初のボットを検出し、ASF はログインしようとします。

![ASF](https://i.imgur.com/u5hrSFz.png)

`SteamLogin` と `SteamPassword` を記入していたら、ASF は設定により Steamガード（メール、２ファクタ認証若しくはなし）だけを要求してくる。 それを記入していない場合、パスワードも要求してきます。

ASFが表示したこれから起きることが気になる方は、&#8203;**[プライバシー・ポリシー](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Statistics-ja-JP#現在のプライバシー・ポリシー)**&#8203;の章を確認して下さい。

入力した情報がすべて正しかったら、ログインは成功し、ASF はデフォルトの設定を使用してアイドリングを開始します。

![ASF 2](https://i.imgur.com/Cb7DBl4.png)

つまり、ASF はあなたのアカウントを使って、正常に動作していることを意味します。プログラムを最小化にして他のことをしてもいいってわけね。 十分な時間（&#8203;**[パフォーマンス](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance-ja-JP)**&#8203;による）が経過すると、Steam トレーディングカードがドロップしてきます。 もちろん、そのためには、アイドリングに必要な有効なゲームが必要だ。&#8203;**[バッジ](https://steamcommunity.com/my/badges)**&#8203;ページから「残りカードドロップX枚」を確認することができます。もしアイドリング可能なゲームがない場合、ASF はなにもしません。詳しくは&#8203;**[よくある質問](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ#what-is-asf-ja-JP)**&#8203;を参照して下さい。

基本的な初期設定はここまでだ。 ASF の設定をさらにいじるのもよし、デフォルト設定のままでも使えます。 ここからはより高度的な設定になりますが、ASF を完全にマスターしたいなら、Wiki のほかのページもご覧下さい（まだ訳していないけど）。

---

### Extended configuration

#### Idling several accounts at once

ASF supports idling more than one account at a time, which is its primary function. You can add more accounts to ASF by generating more bot config files, in exactly the same way as you've generated your first one just a few minutes ago. You need to ensure only two things:

- Unique bot name, if you already have your first bot named "MainAccount", you can't have another one with the same name.
- Valid login details, such as `SteamLogin`, `SteamPassword` and `SteamParentalCode` (if using Steam parental settings)

In other words, simply jump to configuration again and do exactly the same, just for your second or third account. Remember to use unique names for all of your bots.

---

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

Editing your ASF configs can be done much easier by using our ASF-ui frontend, which will be explained further below.

---

#### Using ASF-ui

ASF is a console app and doesn't include a graphical user interface. However, we're actively working on **[ASF-ui](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#asf-ui)** frontend to our IPC interface, which can be a very decent and user-friendly way to access various ASF features.

In order to use ASF-ui, you need to have `IPC` enabled, which is the default option starting with ASF V5.1.0.0. Once you launch ASF, you should be able to confirm that it properly started the IPC interface automatically:

![IPC](https://i.imgur.com/ZmkO8pk.png)

You can access ASF's IPC interface under **[this](http://localhost:1242)** link, as long as ASF is running, from the same machine. You can use ASF-ui for various purposes, e.g. editing the config files in-place or sending **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. Feel free to take a look around in order to find out all ASF-ui functionalities.

![ASF-ui](https://raw.githubusercontent.com/JustArchiNET/ASF-ui/main/.github/previews/bots.png)

Please note that some features, such as sending commands, require a properly set `SteamOwnerID` global config property. Now that you have ASF-ui up and running, why not give it a try and set it from the frontend itself? You'll need to input unique Steam identificator in 64-bit form of your account. You can look it up in various different ways, for example through **[STEAMID I/O](https://steamid.io)** or **[SteamRep](https://steamrep.com)**. The number you're looking for should be similar to `76561198006963719`, which is my account's ID.

---

### Summary

You've successfully set up ASF to use your Steam accounts and you've already customized it to your liking a little. If you followed our entire guide, then you also managed to tweak ASF through our ASF-ui interface and found out that ASF actually has a GUI of some sort. Now is a good time to read our entire **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** section in order to learn what all those different settings you've seen actually do, and what ASF has to offer. If you've stumbled upon some issue or you have some generic question, read our **[FAQ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ)** instead which should cover all, or at least a vast majority of questions that you may have. If you want to learn everything about ASF and how it can make your life easier, head over to the rest of **[our wiki](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Home)**. If you found out our program to be useful for you and you're feeling generous, you can also consider donating to our project. In any case, have fun!

---

## Generic setup

This setup is for advanced users that want to set up ASF to run in **[generic](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#generic)** variant. It's not recommended for people that can use **[OS-specific setup](#os-specific-setup)**.

You want to use `generic` variant mainly in those situations (but of course you can use it regardless):
- When you're using OS that we don't build OS-specific package for (such as 32-bit Windows)
- When you already have .NET Core Runtime/SDK, or want to install and use one
- When you want to minimize ASF structure size by handling runtime requirements yourself
- When you want to use a custom **[plugin](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins)** which requires a `generic` setup of ASF to run properly (due to missing native dependencies)

However, keep in mind that you're in charge of .NET Core runtime in this case. This means that if your .NET Core SDK (runtime) is unavailable, outdated or broken, ASF won't work. This is why we don't recommend this setup for casual users, since you now need to ensure that your .NET Core SDK (runtime) matches ASF requirements and can run ASF, as opposed to **us** ensuring that our .NET Core runtime bundled with ASF can do so.

For `generic` package, you can follow entire OS-specific guide above, with two small changes. In addition to installing .NET Core prerequisites, you also want to install .NET Core SDK, and instead of having OS-specific `ArchiSteamFarm(.exe)` executable file, you now have a generic `ArchiSteamFarm.dll` binary only. Everything else is exactly the same.

With extra steps:
- **[.NET Core の前提条件](https://docs.microsoft.com/dotnet/core/install/dependencies?tabs=netcore31)**&#8203;をインストールします。
- Install **[.NET Core SDK](https://www.microsoft.com/net/download)** (or at least runtime) appropriate for your OS. You most likely want to use an installer. Refer to **[runtime requirements](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)** if you're not sure which version to install.
- Download **[latest ASF release](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** in `generic` variant.
- Extract the archive into new location (and `chmod +x ArchiSteamFarm.sh` if you're on Linux/OS X).
- **[Configure ASF](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)**.
- Launch ASF by either using a helper script or executing `dotnet /path/to/ArchiSteamFarm.dll` manually from your favourite shell.

Helper scripts (such as `ArchiSteamFarm.cmd` for Windows and `ArchiSteamFarm.sh` for Linux/OS X) are located next to `ArchiSteamFarm.dll` binary - those are included in `generic` variant only. You can use them if you don't want to execute `dotnet` command manually. Obviously helper scripts won't work if you didn't install .NET Core SDK and you don't have `dotnet` executable available in your `PATH`. Helper scripts are entirely optional to use, you can always `dotnet /path/to/ArchiSteamFarm.dll` manually.