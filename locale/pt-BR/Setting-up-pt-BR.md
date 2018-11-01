# Primeiros passos

Se você está aqui pela primeira vez, bem-vindo! Estamos felizes em saber que outro viajante está interessado em nosso projeto, no entanto tenha em mente que grandes poderes trazem grandes responsabilidades; o ASF é capaz de fazer muitas coisas relacionadas ao Steam, mas somente enquanto você se **preocupar em aprender como usá-lo**. Há uma curva de aprendizagem aqui e esperamos que você leia a wiki a este respeito, que explica detalhadamente como tudo funciona.

Se você ainda está aqui significa que você suportou o texto acima, o que é bom. A não ser que você tenha pulado ele, então você vai ter alguns **[problemas](https://www.youtube.com/watch?v=WJgt6m6njVw)** logo mais... De qualquer forma, o ASF é um aplicativo de console, o que significa que o programa não tem uma interface amigável que você geralmente está acostumado. O ASF foi projetado principalmente para ser executado em servidores e, portanto, funciona como um serviço (daemon) e não como um aplicativo da área de trabalho.

Isso não significa que você não possa usá-lo no seu computador ou que o uso é mais complicado que o normal, não é nada disso. O ASF é um programa autônomo que funciona imediatamente e não necessita de instalação, mas necessita de uma configuração para se tornar útil. É a configuração que vai dizer ao ASF o que ele deve fazer depois que você executá-lo. Se você o iniciar sem configurar antes ele simplesmente não fará nada.

* * *

## Instalador para sistemas operacionais específicos

Em geral, é isso que vamos fazer nos próximos minutos:

- Instalar o **[.NET Core](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)**.
- Baixar a **[última versão do ASF](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** na versão correta para o seu SO.
- Extrair o arquivo em um novo local (e `chmod +x ArchiSteamFarm` se você usar Lunux/OS X).
- **[Configurar o ASF](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-pt-BR)**.
- Executar o ASF e ver a mágica.

Parece simples o bastante, certo? Então, vamos ver.

* * *

### Pré-requisitos .NET Core

O primeiro passo é garantir que seu sistema operacional pode mesmo executar o ASF corretamente. O ASF é escrito em C#, com base no .NET Core e pode requerer bibliotecas nativas que ainda não estão disponíveis na sua plataforma Os requisitos serão diferentes se você usa Windows, Linux ou OS X, embora todos estejam listados no documento **[pré-requisitos .NET Core](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)** que você deve seguir Esse é nosso material de referência e ele deve ser usado, mas para simplificar nós detalhamos todos os pacotes necessários abaixo para que você não precise ler todo o documento.

É perfeitamente normal que algumas dependências (ou mesmo todas) já tenham sido instaladas no seu sistema por algum outro software que você use. Ainda assim, você deve garantir executando o instalador apropriado para seu sistema operacional - sem essas dependências o ASF não vai nem iniciar.

Você não precisa fazer mais nada para as versões para SOs específicos, especialmente instalar o .NET Core SDK ou mesmo o tempo de execução, já que o pacote específico para SO já inclui tudo. Você precisa apenas dos pré-requisitos (dependências) .NET Core para rodar o tempo de execução .NET Core incluído no ASF.

#### **[Windows](https://docs.microsoft.com/pt-BR/dotnet/core/windows-prerequisites?tabs=netcore2x)**:

- **<a href="[">Pacotes Redistribuíveis do Microsoft Visual C++ 2015 Update 3 RC](https://www.microsoft.com/pt-br/download/details.aspx?id=52685)** (x64 para Windows 64-bit, x86 para Windows 32-bit)
- É altamente recomendado garantir que todas as atualizações do Windows estejam instaladas. Você precisa pelo menos das atualizações **[KB2533623](https://support.microsoft.com/pt-br/help/2533623/microsoft-security-advisory-insecure-library-loading-could-allow-remot)** e **[KB2999226](https://support.microsoft.com/pt-br/help/2999226/update-for-universal-c-runtime-in-windows)**, mas outras atualizações podem ser necessárias. Todas elas já estarão instaladas se o seu Windows estiver atualizado. Certifique-se de que você atende a esses requisitos antes de instalar o pacote do Visual C++.

#### **[Linux](https://docs.microsoft.com/pt-br/dotnet/core/linux-prerequisites?tabs=netcore2x)**:

Os nomes dos pacotes dependem da distribuição do Linux que você esteja usando, nós listamos as mais comuns. Você pode obter todas elas com o gerenciador de pacotes nativo do seu SO (como `apt` para Debian ou `yum` por CentOS).

- libcurl3 (libcurl)
- libicu60 (libicu, versão atualizada para a sua distribuição, poe exemplo `libicu57` para o Debian 9)
- libkrb5-3 (krb5-libs)
- liblttng-ust0 (lttng-ust)
- libssl1.0.2 (libssl, openssl-libs, versão 1.0.X atualizada para a sua distribuição)
- zlib1g (zlib)

Pelo menos alguns desses já devem estar disponíveis nativamente no seu sistema (como o zlib1g que é exigido em quase todas as distribuições do Linux hoje).

#### **[OS X](https://docs.microsoft.com/pt-br/dotnet/core/macos-prerequisites?tabs=netcore2x)**:

- Nenhuma por enquanto

* * *

### Baixando

Uma vez que já tenhamos todas as dependências, o próximo passo é baixar a **[última versão do ASF](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)**. ASF está disponível em diversas variantes, mas você está interessado no pacote que corresponde ao seu sistema operacional e a arquitetura dele. Por exemplo, se você estiver usando o `Win`dows `64`-bit, então você vai baixar o pacote `ASF-win-x64`. Para obter mais informações sobre as variantes disponíveis, visite a seção</strong> **[compatibilidade](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-pt-BR). O ASF é também capaz de rodar em SOs para os quais ele não tem um pacote específico, tal como o **Windows 32-bit**, vá até **[configuração genérica](#configuração-genérica)** para saber mais.</p> 

![Arquivos](https://i.imgur.com/Ym2xPE5.png)

Uma vez que você baixar o pacote e extrair o arquivo (nós recomendamos usar o **[7-zip](https://www.7-zip.org)**), você terá uma enorme bagunça de pastas e arquivos. Não se preocupe, nós vamos resolver isso em um segundo.

Se você estiver usando Linux/OS X não se esqueça do `chmod + x ArchiSteamFarm`, já que as permissões não são definidas automaticamente no arquivo zip. Isso tem que ser feito somente uma vez após descompactar.

Certifique-se de descompactar o ASF para a **sua própria pasta** e não para outra existente que você esteja usando para outra coisa - as atualizações automáticas do ASF vão excluir todos os arquivos velhos e não relacionados, o que vao fazer você perder qualquer coisa não relacionada que esteja na mesma pasta. Se você tiver qualquer scripts ou arquivos extras que você quer usar com o ASF, coloque-os uma pasta acima.

Uma exemplo de extrutura seria assim:

    C:\ASF (onde você pode colocar suas coisas)
        ├── ASF shortcut.lnk (opcional)
        ├── Config shortcut.lnk (opcional)
        ├── Commands.txt (opcional)
        ├── MyExtraScript.bat (opcional)
        ├── ... (qualquer outro arquivo que você quiser, opcional)
        └── Core (dedicado apenas ao ASF, onde você vai extrair o arquivo)
             ├── ArchiSteamFarm.dll
             ├── config
             └── (...)
    

Essa também é a estrutura que recomendamos, então você não precisa passar por um grande número de arquivos e pastas incluídas no ASF, já que para o uso você só precisa de um atalho para a pasta de configuração e para o executável principal.

Certo, agora vamos preparar a pasta do ASF para o uso. Se você quiser você já pode pular para o próximo passo, uma vez que limpar a estrutura do ASF não é necessário, mas vai deixar sua vida um pouco mais fácil.

Abra a pasta do ASF e encontre arquivo executável principal, é o `ArchiSteamFarm.exe` no Windows e `ArchiSteamFarm` no Linux/OS X. Clique com o botão direito do mouse e selecione "copiar". Agora, navegue até o local que você quer colocar o atalho do ASF (por exemplo, sua área de trabalho), clique com o botão direito e escolha "colar atalho aqui". Você pode renomear o atalho se você quiser, dando-lhe o nome de "ASF" por exemplo. Agora faça o mesmo com a pasta `config` que você pode encontrar no mesmo lugar que o executável do ASF.

Após uma pequena limpeza você terá uma estrutura muito conveniente, similar a essa abaixo:

![Estrutura](https://i.imgur.com/k85csaZ.png)

Isso te permite acessar facilmente o executável do ASF e os arquivos de configuração sem muita trabalheira. No meu caso que eu decidi usar a estrutura mencionada acima, então meus arquivos do ASF estão diretamente dentro da pasta "Core". Você pode adaptar essa estrutura ao seu gosto, tal como colocar atalhos na área de trabalho para o ASF e para a pasta config e a pasta do ASF em `C:\ASF` por exemplo, isso fica a seu critério.

Aconselhamos o mesmo para usuários do Linux/OS X, você pode usar o mecanismo de links simbólicos disponível através de `ln -s`.

* * *

### Configuração

Agora estamos prontos para a última etapa, a configuração. This is by far the most complicated step, since it involves a lot of new information you're not familiar with yet, so we'll try to provide some easy to understand examples and simplified explanation here.

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

### Executando o ASF

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

### Configuração estendida

#### Coletando de várias contas ao mesmo tempo

ASF supports idling more than one account at a time, which is its primary function. You can add more accounts to ASF by generating more bot config files, in exactly the same way as you've generated your first one just a few minutes ago. You need to ensure only two things:

- Unique bot name, if you already have your first bot named "MainAccount", you can't have another one with the same name.
- Valid login details, such as `SteamLogin`, `SteamPassword` and `SteamParentalCode` (if using Steam parental settings)

In other words, simply jump to configuration again and do exactly the same, just for your second or third account. Remember to use unique names for all of your bots.

* * *

#### Alterando configurações

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

#### Usando a ASF-ui

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

### Resumo

You've successfully set up ASF to use your Steam accounts and you've already customized it to your liking a little. If you followed our entire guide, then you even managed to send a simple command through our ASF-ui interface. Now is a good time to read our entire **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** section in order to learn what all those different settings you've seen in advanced tab actually do, and what ASF can offer. If you've stumbled upon some issue or you have some generic question, read **[FAQ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ)** instead which should cover all, or at least majority of questions that you might have. If you want to learn everything about ASF and how it can make your life easier, head over to the rest of **[our wiki](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Home)**. Have fun!

* * *

## Configuração genérica

This setup is for advanced users that want to set up ASF to run in **[generic](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#generic)** variant. It's not recommended for people that can use **[OS-specific setup](#os-specific-setup)**.

You want to use generic variant mainly in three situations (but of course you can use it regardless):

- When you're using OS that we don't build OS-specific package for (such as 32-bit Windows)
- When you already have .NET Core Runtime/SDK, or want to install and use one
- When you want to minimize ASF structure size by handling runtime requirements yourself

However, keep in mind that you're in charge of .NET Core runtime in this case. This means that if your .NET Core SDK (runtime) is unavailable, outdated or broken, ASF won't work. This is why we don't recommend this setup for casual users, since you now need to ensure that your .NET Core SDK (runtime) matches ASF requirements and can run ASF, as opposed to **us** ensuring that our .NET Core runtime bundled with ASF can do so.

For generic package, you can follow entire OS-specific guide above, with two small changes. In addition to installing .NET Core prerequisites, you also want to install .NET Core SDK, and instead of having OS-specific `ArchiSteamFarm(.exe)` executable file, you now have a generic `ArchiSteamFarm.dll` binary only. Everything else is exactly the same.

With extra steps:

- Instalar o **[.NET Core](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)**.
- Install **[.NET Core SDK](https://www.microsoft.com/net/download)** (or at least runtime) appropriate for your OS. You most likely want to use an installer. Refer to **[runtime requirements](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)** if you're not sure which version to install.
- Download **[latest ASF release](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** in generic variant.
- Extract the archive into new location (and `chmod +x ArchiSteamFarm.sh` if you're on Linux/OS X).
- **[Configurar o ASF](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-pt-BR)**.
- Launch ASF by either using a helper script or executing `dotnet /path/to/ArchiSteamFarm.dll` manually from your favourite shell.

Helper scripts (such as `ArchiSteamFarm.cmd` for Windows and `ArchiSteamFarm.sh` for Linux/OS X) are located next to `ArchiSteamFarm.dll` binary - those are included in generic variant only. You can use them if you don't want to execute `dotnet` command manually. You can also make a shortcut to those scripts like showed above, since they're supposed to provide binary replacement in a script way. Obviously helper scripts won't work if you didn't install .NET Core SDK and you don't have `dotnet` executable available in your `PATH`. Helper scripts are entirely optional to use, you can always `dotnet /path/to/ArchiSteamFarm.dll` manually.