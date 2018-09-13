# Perguntas frequentes

A seção de perguntas frequentes cobre respostas a questões comuns que você pode ter. Para questões menos comuns, por favor visite a seção **[Perguntas Frequentes Adicionais](https://github.com/JustArchi/ArchiSteamFarm/wiki/Extended-FAQ)**.

# Tabela de conteúdos

- [Geral](#general)
- [Comparação com ferramentas similares](#comparison-with-similar-tools)
- [Segurança / Privacidade / VAC / Banimentos / Termos de serviço](#security--privacy--vac--bans--tos)
- [Diversos](#misc)
- [Problemas](#issues)

* * *

## Geral

### Então, como exatamente ele funciona?

Antes de tentar entender o que é o ASF, certifique-se de que você entende o que são as Cartas Colecionáveis Steam e como obtê-las, o que é muito bem descrito na seção oficial de perguntas frequentes **[aqui](https://steamcommunity.com/tradingcards/faq)**.

Em resumo, as Cartas Colecionáveis são itens que você obtém ao jogar certos jogos, e podem ser usadas para criar insígnias, vender no mercado da Steam ou qualquer outro objetivo de sua escolha.

Os pontos principais são apresentados aqui novamente, porque as pessoas em geral não querem concordar com eles:

- **Sim, você precisa possuir o jogo para ser elegível a ganhar cartas dele. Compartilhamento de biblioteca não funciona.**
- **Não, você não pode coletar de um jogo infinitamente, cada jogo tem um número fixo de cartas. Uma vez que você conseguir todas as cartas de um jogo, ele não será mais apto a coleta.**
- **Não, não é possível coletar cartas de jogos gratuitos sem gastar nenhum dinheiro neles. Isso inclui jogos permanentemente gratuitos como Team Fortress 2 ou Dota 2.**
- **Não, não é possível coletar cartas em contas limitadas (aquelas que nunca gastaram 5 dólares na loja Steam), independentemente dos jogos que possuir. Isso foi possível no passado, mas não é mais o caso.**

Então como você pode ver, Cartas Colecionáveis Steam são concedidas a você por jogar um jogo que você comprou, ou um jogo gratuito no qual você colocou dinheiro. Em outras palavras, se você jogar um jogo por tempo suficiente, todas as cartas daquele jogo aparecerão no seu inventário, possibilitando que você complete uma insígnia, venda, ou faça o que quiser.

O ASF como um programa é bastante complexo para entender totalmente, então ao invés de explicar todos os detalhes técnicos, vamos oferecer uma explicação muito simples abaixo.

O ASF se conecta a sua conta Steam através deum mini cliente Steam embutido usando as credenciais fornecidas. Após se conectar com sucesso, ele analisa sua página de **[insígnias](https://steamcommunity.com/my/badges)** a fim de encontrar jogos que estão disponíveis para coleta (Jogo pode dar mais X cartas). Após analisar todas as páginas e fazer a lista final de jogos que estão aptos, o ASF escolhe o algoritmo de coleta mais eficiente e inicia o processo. O processo depende do **[algorítimo de coleta de cartas](https://github.com/JustArchi/ArchiSteamFarm/wiki/Performance)** escolhido, mas geralmente consiste em jogar um jogo elegível e, periodicamente (e a cada item recebido), verificar se o jogo já está totalmente coletado - se sim, o ASF pode prosseguir para o próximo título, usando o mesmo procedimento, até que todos os jogos sejam totalmente explorados.

Tenha em mente que a explicação acima é simplificada e não descreve as dezenas de recursos e funções extras que o ASF oferece. Visite o resto da **[nossa wiki](https://github.com/JustArchi/ArchiSteamFarm/wiki)** se você quiser conhecer cada detalhe do ASF. Eu tentei fazê-la simples o bastante para que todos a entendam, sem entrar em detalhes técnicos - usuários avançados são encorajados a cavar mais fundo.

Agora como programa - o ASF oferece um pouco de magia. Em primeiro lugar, ele não precisa baixar nenhum arquivo de jogo, ele pode jogar os jogos imediatamente. Em segundo lugar, ele é inteiramente independente do seu cliente Steam normal - você não precisa ter o cliente Steam rodando, nem mesmo instalado. Em terceiro lugar, é uma solução automatizada - o que significa que o ASF faz tudo sozinho para você, sem a necessidade de dizer a ele o que fazer - o que te poupa aborrecimento e tempo. Por último, ele não tem que enganar a rede Steam emulando o processo (que, por exemplo, é o método usado pelo Idle Master), já que ele pode se comunicar diretamente com ele. Ele também é super rápido e leve, sendo uma solução surpreendente para aqueles que querem obter cartas facilmente e sem muita trabalheira - é especialmente útil deixá-lo rodando em segundo plano enquanto está fazendo outra coisa, ou mesmo jogando no modo offline.

Tudo isso é bom, mas o ASF também tem algumas limitações técnicas que são aplicadas pela Steam - nós não podemos rodar jogos que você não possui, nós não podemos rodar jogos para sempre a fim de obter cartas extras após o limite imposto e nós não podemos rodar jogos enquanto você está jogando. Tudo isso deve ser "óbvio", considerando a maneira como o ASF funciona, mas é bom notar que o ASF não tem super poderes e não vai fazer algo que é fisicamente impossível, então tenha isso em mente - é basicamente a mesma coisa que se você dissesse a alguém para se conectar na sua conta de outro PC e jogar esses jogos para você.

Então resumindo - o ASF é um programa que ajuda a pegar as cartas que você é apto a pegar, mas sem muita trabalheira. Ele também oferece várias outras funções, mas vamos nos ater a esta por enquanto.

* * *

### Tenho que colocar minhas credenciais de conta?

**Sim**. O ASF exige suas credenciais de conta da mesma forma que o cliente oficial da Steam, já que ele está usando o mesmo método para interagir com a rede Steam. No entanto, isso não significa que você tenha que colocar suas credenciais de conta nos arquivos de configuração do ASF, você pode usar o ASF com `SteamLogin` e/ou `SteamPassword` `null`/vazio, e colocar esses dados cada vez que abrir o ASF, quando for preciso (assim como várias outras credenciais de login, veja a seção configuração). Desta forma, seus dados não são salvos em lugar nenhum, mas é claro, assim o ASF não poderá auto-reiniciar sem a sua ajuda. O ASF também oferece várias outras formas de aumentar a sua **[segurança](https://github.com/JustArchi/ArchiSteamFarm/wiki/Security)**, então sinta-se a a vontade para ler essa parte da wiki se você for um usuário avançado. Se você não é, e não quer colocar suas credenciais de conta nas configurações do ASF, então simplesmente não faça isso, e coloque-as quando o ASF as pedir.

Tenha em mente que o ASF é uma ferramenta para seu uso pessoal e as suas credenciais nunca deixarão seu computador. Você também não estará compartilhando elas com ninguém, o que cumpre os termos de serviço da Steam - uma coisa muito importante da qual as pessoas esquecem. Você não vai mandar seus dados para nossos servidores ou o servidor de algum terceiro, somente diretamente para os servidores da Steam operados pela Valve.

* * *

### Quanto tempo tenho que esperar para ganhar as cartas?

**O quanto for preciso** - sério. Cada jogo tem uma dificuldade de coleta diferente, definida pelo(a) desenvolvedor(a) e/ou editora, e cabe totalmente a eles decidir o quão rápido as cartas serão fornecidas. A maioria dos jogos segue o padrão de 1 carta a cada 30 minutos de jogo, mas também há jogos que exigem que você jogar várias horas antes de dar uma carta. Além disso, sua conta pode ser impedida de receber cartas de jogos que você não jogou por determinado tempo ainda, como consta na seção **[desempenho](https://github.com/JustArchi/ArchiSteamFarm/wiki/Performance)**. Não tente supor quanto tempo o ASF deve coletar de determinado título - não cabe a você, nem ao ASF decidir. Não há nada que você possa fazer para torná-lo mais rápido, e não há "bugs" relacionados a cartas não serem recebidas em tempo hábil - você não controla o processo de recebimento de cartas, nem o ASF. Na melhor das hipóteses, você receberá a média de 1 carta a cada 30 minutos. Na pior, você não receberá qualquer carta em menos de 4 horas desde que iniciou o ASF. Ambas as situações são normais e estão explicadas na seção **[desempenho](https://github.com/JustArchi/ArchiSteamFarm/wiki/Performance)**.

* * *

### A coleta está demorando muito, posso adiantá-la de alguma forma?

A única coisa que afeta fortemente a velocidade de coleta é o **[algorítimo de coleta de cartas](https://github.com/JustArchi/ArchiSteamFarm/wiki/Performance)** escolhido para sua conta bot. Tudo o resto tem efeitos insignificantes e não vai deixar o processo de coletas mais rápido, enquanto algumas ações, tal como iniciar o processo do ASF várias vezes pode até **piorar**. Se você realmente deseja fazer uso de cada maldito segundo do processo de coleta, então o ASF permite que você configure algumas variáveis principais, tal como `FarmingDelay` - todas elas são explicadas em **[configuração](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration-pt-BR)**. No entanto, como eu disse, o efeito é insignificante, e escolher o algorítimo de coleta de cartas adequado para determinada conta é a única escolha crucial que pode afetar fortemente a velocidade da coleta, tudo o resto é pura cosmética. Em vez de se preocupar com a velocidade de coleta, apenas abra o ASF e deixe ele fazer seu trabalho - posso assegurar-lhe que ele está fazendo isso da forma mais eficaz que eu poderia conseguir. Quanto menos você se importar, mais você ficará satisfeito.

* * *

### Mas o ASF disse que vai levar cerca de X!

O ASF te mostra um tempo aproximado com base no número de cartas que você precisa coletar e seu algoritmo escolhido - isso não garante em nada o tempo real que você gastará coletando, que geralmente é maior que esse, como o ASF assume a melhor das hipóteses, ele ignora falhas na rede Steam, quedas de internet, sobrecarga dos servidores Steam entre outros. Esse tempo deve ser considerado simplesmente como um indicador de quanto você deve esperar na melhor das hipóteses, e esse tempo real pode ser diferente, às vezes até de maneira significativa. Como dito acima, não tente adivinhar por quanto tempo um jogo ficará coletando, não cabe a você, nem ao ASF decidir.

* * *

### O ASF funciona no meu android/smartphone?

O ASF é um programa C# que requer a implementação funcional do .NET Core. Atualmente não há nenhuma compilação nativa do .NET Core própria para o Android, mas existem compilações para linux na arquitetura ARM próprias e funcionais, portanto, é totalmente possível usar algo como o **[Linux Deploy](https://play.google.com/store/apps/details?id=ru.meefik.linuxdeploy)** para instalar o Linux e, usar o ASF nesse Linux chroot normalmente.

É muito provável que no futuro vejamos .NET Core rodando diretamente no Android.

* * *

### O ASF pode coletar itens de jogos Steam, tais como CS:GO ou Unturned?

**Não**, isso é contra os Termos de Serviço e a Valve afirmou isso claramente com a última onda de banimentos da comunidade por coleta de item do TF2. O ASF é um programa de coleta automática de Cartas Colecionáveis Steam, não de intens de jogos - ele não tem qualquer capacidade de coletar itens de jogo e não temos planos de adicionar tal característica no futuro, nunca, principalmente por violar os termos de uso da Steam. Não peça isso - o melhor que você vai conseguir é uma denúncia de algum usuário ofendido e um problema por conta disso. The same goes for all other types of idling, such as idling drops from CS:GO broadcasts. ASF is focusing on Steam trading cards exclusively.

* * *

### Posso escolher de quais jogos coletar?

**Sim**, de diversas formas. Se você quiser mudar a lista padrão de coleta é só usar a **[propriedade de configuração do bot](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration-pt-BR#configura%C3%A7%C3%A3o-do-bot)** `FarmingOrders`. Se você quiser bloquear manualmente alguns jogos, você pode usar a blacklist disponível com o **[comando](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)** `ib`. Se você quiser coletar tudo, mas dar preferência a alguns jogos antes, você pode usar a lista de prioridade disponível com o **[comando](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)** `iq`. E ainda, se você quer coletar apenas de determinados jogos, então você pode usar a **[propriedade de configuração do bot](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration-pt-BR#configura%C3%A7%C3%A3o-do-bot)** `IdlePriorityQueueOnly`, juntamente com a adição dos seus jogos na lista de prioridade de coleta.

Além de gerenciar o módulo de coleta automática que foi descrito acima, você também pode mudar o ASF para o modo manual como **[comando](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)** `play`, ou usar uma mistura de configurações externas como a **[propriedade de configuração do bot](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration-pt-BR#configura%C3%A7%C3%A3o-do-bot)** `GamesPlayedWhileIdle`.

* * *

### Eu sou usuário do Linux / OS X, o ASF roda jogos que não estão disponíveis para a minha plataforma? O vai ASF roda jogos 64-bit se eu rodo um SO de 32-bit?

Sim, o ASF não vai baixar nenhum arquivo de jogo, então ele funciona com todas as licenças que estejam ligadas a sua conta Steam, independente de qualquer requisito técnico ou de plataforma. Ele também deve funcionar com jogos que tenham bloqueio regional, mesmo quando você não estiver na região correta, embora não tenhamos testado isso.

* * *

## Comparação com ferramentas similares

* * *

### O ASF é similar ao Idle Master?

A única semelhança é o propósito geral dos dois programas, que é rodar jogos Steam para coletar cartas. Tudo o mais, incluindo o método usado para rodar os jogos, algorítimos utilizados, estrutura do programa, funcionalidades, compatibilidade, e o próprio código, são completamente diferentes e os dois programas não tem nada mais em comum, até o sistema base (o IM roda no .NET Framework e o ASF no .NET Core). O ASF foi criado para resolver alguns problemas do IM que não eram possíveis de serem resolvidos com uma simples edição no código - é por isso que o ASF foi escrito do zero, sem usar nem uma simples linha de código ou mesmo a ideia geral do IM, pois aquele código e aquelas idéias eram totalmente falhas para começo de conversa. O IM e o ASF são como Windows e Linux - ambos são sistemas operacionais e ambos podem ser instalados no seu PC, mas eles não compartilham nada entre si além de servirem para o mesma finalidade.

Por esse motivo também você não deve comparar o ASF com o IM baseado no que é esperado do IM. Você deve tratar o ASF e o IM como programas completamente diferentes com suas características próprias e exclusivas. Algumas dessas características podem se sobrepor e você poderá encontrar uma característica particular em ambos, mas muito raramente, uma vez que o ASF serve a esse propósito de uma madeira muito diferente comparado ao IM.

* * *

### Vale a pena usar o ASF se eu estiver usando o Idle Master e ele funcionar bem para mim?

**Sim**. O ASF é muito mais confiável e inclui muitas funções **cruciais**, independente de como você coleta, e isso o IM simplesmente não oferece.

O ASF implementa uma lógica correta para **jogos não lançados** - o IM tentará rodar esses jogos, mesmo que eles ainda não tenham sido lançados. Claro, não há como rodar esses jogos até a data de lançamento, então seu processo de coleta ficará travado. Isso fará com que você tenha que adicioná-lo a blacklist, esperar pelo lançamento ou pular manualmente. Nenhuma dessas é uma boa solução e todos requerem sua atenção - o ASF pula (temporariamente) a coleta desses jogos e a retoma assim que forem lançados, evitando completamente problemas e lidando eficientemente com isso.

O ASF também tem uma lida corretamente com vídeos de qualquer **espécie**. Há muitos videos na Steam que tem cartas mas são anunciados com o `appID` errado na página de insígnias, como o **[Double Fine Adventure](https://store.steampowered.com/app/402590)** por exemplo - o IM tentará rodar o `appID` errado, então você não vai conseguir nenhuma carta e o processo vai travar. Novamente, você vai ter que bloquear ou pular manualmente. Já o ASF descobre automaticamente o `appID` correto e faz a coleta.

Além disso, o ASF é **muito mais estável e confiável** quando se trata de problemas de rede e peculiaridades da Steam - ele funciona na maioria das vezes e, uma vez configurado, não requer a sua atenção em tudo, enquanto o IM trava muitas vezes, requer correções extras ou simplesmente não funciona. Ele também é totalmente dependente do seu cliente Steam, o que significa que ele pode não funcionar se você estiver com problemas no cliente. ASF is working properly as long as it can connect to Steam network, and doesn't require Steam client running or even being installed.

Those are 3 **very important** points why you should consider using ASF, as they directly affect everybody idling Steam cards and there is no way to say "this doesn't consider me", since Steam maintenances and quirks are happening to everybody. There are dozen of extra less and more important reasons which you might learn about in the rest of the FAQ. So shortly speaking, **yes**, you should use ASF even when you don't need any extra ASF feature that is available when compared to IM.

In addition to that, IM is officially discontinued and might break completely in the future, without anybody bothering to fix it, considering existence of much more powerful solutions (not only ASF). IM already doesn't work for a lot of people, and that number is only going up, not down. You should avoid using obsolete software in the first place, not only IM but all other deprecated programs as well. No active maintainer means that nobody cares whether it works or not, nobody verifies if it does and **nobody is responsible for its functionality**, which is a crucial matter in terms of security.

* * *

### What interesting features ASF offers that Idle Master does not?

It depends what you consider "interesting" for you. If you plan to idle more accounts than one then the answer is already obvious since ASF allows you to idle all of them with one superior solution, saving resources, hassle, and compatibility issues. However, if you're asking that question then most likely you don't have this particular need, so let's evaluate other benefits that apply to one single account used in ASF.

First and foremost, you have some built-in features mentioned **[above](#is-it-worth-it-to-use-asf-if-im-currently-using-idle-master-and-it-works-fine-for-me)** that are core for idling regardless of your end-goal, and very often that alone is already enough to consider using ASF. But you already know that, so let's move onto some more interesting features:

- **You can idle offline** (`OnlineStatus` of `Offline` feature). Idling offline makes it possible for you to skip your Steam in-game status entirely, which allows you to idle with ASF while showing "Online" on Steam at the same time, without your friends even noticing that ASF is playing a game on your behalf. This is superior feature, since it allows you to remain online in your Steam client, while not annoying your friends with constant game changes, or misleading them into thinking that you're playing a game while in reality you're not. This point alone makes it worthwhile to use ASF if you respect your own friends, but it's only the beginning. It's also nice to note that this feature has nothing to do with Steam privacy settings - if you launch the game yourself, then you'll properly show as in-game to your friends, making only ASF part invisible and not affecting your account at all.

- **You can skip refundable games** (`IdleRefundableGames` feature). ASF has proper built-in logic for refundable games and you can configure ASF to not idle refundable games automatically. This allows you to evaluate yourself if your newly-bought game from Steam store was worth your money, without ASF trying to drop cards from it as soon as possible. If you play it for 2+ hours, or 2 weeks pass since your purchase, then ASF will proceed with that game as it's not refundable anymore. Until then you have full control whether you enjoy it or not and you can easily refund it if needed, without having to manually blacklist that game or not use ASF for entire duration.

- **You can automatically mark new items as received** (`BotBehaviour` of `DismissInventoryNotifications` feature). Idling with ASF will result in your account receiving new card drops. You already know that this is going to happen, so let ASF clear that useless notification for you, ensuring that only important things will raise your attention. Of course, only if you want to.

- **You can automatically receive cards from Steam events** (`AutoSteamSaleEvent` feature). ASF allows you to automate going through discovery queue and voting in Steam Awards during Steam sale, of course only if you'd like to make use of that. This saves enormous amount of time each day while Steam sale is on, and ensures that you'll never miss your daily card drops again.

- **You can customize preferred farming order with more available options** (`FarmingOrders` feature). Perhaps you want to idle your newly bought games first? Or your oldest ones? According to number of card drops? Badge levels you already crafted? Played hours? Alphabetically? According to AppIDs? Or maybe fully random? That's entirely up to you to decide.

- **ASF can help you complete your sets** (`TradingPreferences` with `SteamTradeMatcher` feature). With a bit more advanced tinkering, you can convert your ASF into fully-featured user-bot that will automatically accept **[STM](https://www.steamtradematcher.com)** offers, helping you each day to match your sets without any user interaction. ASF even includes its very own ASF 2FA module allowing you to import your Steam mobile authenticator and let you fully automate the entire process with accepting confirmations as well. Or, maybe you want to accept manually and let ASF only prepare those trades for you? That's once again, fully up to you to decide.

- **ASF can redeem keys in background for you** (**[background games redeemer](https://github.com/JustArchi/ArchiSteamFarm/wiki/Background-games-redeemer)** feature). Maybe you have a hundred of keys from various bundles that you're too lazy to redeem yourself, going through bunch of windows and agreeing to Steam terms and conditions over and over again. Why not copy-paste your list of keys into ASF and let it do its job? ASF will automatically redeem all of those keys in background, providing you with appropriate output to let you know how each redeem attempt turned out. Moreover, if you have hundreds of keys, you're guaranteed to get rate-limited by Steam sooner or later, and ASF also knows about that, it'll patiently wait for the rate-limit to go away, and resume where it left.

We could now go on and on with entire **[ASF wiki](https://github.com/JustArchi/ArchiSteamFarm/wiki/Home)**, pointing out every single feature of the program, but we have to draw a line somewhere. This is it, this is a list of features that you can enjoy as ASF user, where just one of those could easily be considered a major reason to never look back, and we actually listed **a lot** of them, with even more you can learn about on the rest of our wiki. Ah yes, and we didn't even go into detail with things like ASF's API allowing you to script your own commands, or glorious bots management, since we wanted to keep it simple.

* * *

### O ASF é mais rápido que o Idle Master?

**Yes**, although the explanation is rather complicated.

On each new process spawned and terminated on your system, steam client automatically sends a request containing all of your games that you're currently playing - this way steam network can calculate hours and make cards drop. However, steam network counts your time played in 1-second intervals, and sending new request resets the current status. In other words, if you did spawn/kill new process every 0.5 second, you'd never drop any card because every 0.5 second steam client would send a new request and steam network would never count even 1 second of play time. Moreover, because of how operating system works, it's actually quite common to see new processes being spawned/terminated without you even doing anything, so even if you're doing nothing on your PC - there are many processes still working in the background, spawning/terminating other processes all the time. Idle master is based on steam client, so this mechanism affects you if you're using it.

ASF is not based on steam client, it has its own steam client implementation. Thanks to that, what ASF is doing, is not spawning any process, but actually sending one, real request to steam network that we started playing a game. This is the same request that would be sent by steam client, but because we have actual control over the ASF steam client, we don't need to spawn new processes, and we're not mimicking steam client regarding send request on every process change, so the mechanism explained above doesn't affect us. Thanks to that, we never interrupt that 1 second interval on steam web side, and that enhances our farming speed.

* * *

### But is the difference really noticeable?

No. The interrupts that are happening with normal steam client and idle master have negligible effect on the card drops, so it's not any noticeable difference that would make ASF superior.

However, there **is** a difference, and you can clearly notice that, as depending on how busy your OS is, cards **will** drop faster, from a few seconds to even a few minutes, if you're extremely unlucky. Although I wouldn't consider using ASF only because it drops cards faster, as both ASF and Idle Master are affected by how steam web works, ASF just interacts with steam web more effectively, while Idle Master can't control what steam client is actually doing (so it's not Idle Master's fault, but steam client's itself).

* * *

### Can ASF idle multiple games at once?

**Yes**, although ASF knows better when to use that feature, based on selected **[cards farming algorithm](https://github.com/JustArchi/ArchiSteamFarm/wiki/Performance)**. You do not have direct choice on cards farming algorithm, but you can suggest ASF one, via setting config properties properly. You should focus on configuration part of the ASF, and let algorithms decide what is the most optimal way to achieve the goal.

* * *

### Can ASF skip through games fast?

**No**, ASF doesn't support, neither encourages usage of **[Steam glitches](https://github.com/JustArchi/ArchiSteamFarm/wiki/Performance#steam-glitches)**.

* * *

### Can ASF automatically idle each game for X hours before cards are added?

**No**, the whole point of Steam cards system change was to fight with false statistics and ghost players. ASF won't contribute towards that more than necessary, adding such feature is not planned and won't happen.

* * *

### Can I play a game while ASF is farming?

**Não**. ASF unlike IM has independent Steam client included, and Steam network allows only **one Steam client at a time** to play a game. You can however disconnect ASF any time you like by starting a game (and clicking "OK" when asked if Steam network should disconnect other client) - ASF will then patiently wait till you're done playing, and resume the process afterwards. Alternatively, you can still play in offline mode anytime you like, if that is satisfying for you.

Keep in mind that cards drop rate when playing multiple games is close to 0 anyway, therefore there are no direct benefits from being able to do that with IM, while there are strong benefits of no interfering with other games launched with ASF, which is crucial e.g. VAC-wise.

* * *

## Segurança / Privacidade / VAC / Banimentos / Termos de serviço

* * *

### Can I get VAC ban for using this?

No, it's not possible because ASF (unlike Idle Master or SAM) does not interfere in any way with steam client nor its processes. It's physically impossible to get VAC ban for using ASF, even during playing on secured servers while ASF is running - this is because **ASF doesn't even require Steam Client being installed at all** in order to work properly. ASF is the only farming program that can currently guarantee being VAC-free.

* * *

### Can using ASF prevent me from playing on VAC-secured servers, as stated **[here](https://support.steampowered.com/kb_article.php?ref=2117-ilzv-2837)**?

ASF does not require Steam client being running or even installed at all. According to this concept, it should **not** cause any VAC-related issues, because ASF guarantees lack of interfering with Steam client and all its processes - this is the main point when talking about VAC-free guarantee that ASF offers.

According to users and best of my knowledge, this is the case right now, as nobody reported any issues like stated in the link above while using ASF. We couldn't reproduce the issue above with ASF as well, while clearly reproducing it with Idle Master.

However, keep in mind that Valve might still add ASF to the blacklist at some point, but it's a complete nonsense as even if they do that, you could still play VAC-secured games from your PC, and use ASF at the same time e.g. on your server, so I'm pretty sure that they know very well that ASF should not be a suspect VAC-wise, and they won't make our lifes harder by blacklisting ASF for no reason. Still, in the worst case you'll be unable to play, like stated above, because VAC-free guarantee of ASF is still here regardless if Steam blacklists ASF binary, or not (and you can still launch ASF on any other machine without Steam client being installed at all). Right now there is no need to do any of that, and let's hope it stays like this.

* * *

### Is it safe?

If you ask if ASF is safe as a software, which means that it won't cause any damage to your computer, won't steal your private data, install viruses or any other stuff like that - it is safe. Code is open-source, and distributed binaries are always compiled from **[publicly available sources](https://en.wikipedia.org/wiki/Open-source_software)** by **[automated and trusted continuous integration systems](https://en.wikipedia.org/wiki/Build_automation)**, and not even developers themselves. Each build is reproducible by following our build script and will result in exactly the same, **[deterministic](https://en.wikipedia.org/wiki/Deterministic_system)** IL (binary) code.

If you for whatever reason don't trust our builds, you can always compile and use ASF from source, including all libraries that ASF is using (such as SteamKit2), which are open-source too. In the end however, it's always a matter of trust to the developer(s) of your application, so you should decide yourself if you consider ASF safe or not, potentially supporting your decision with technical arguments. Do not blindly believe something only because I said so - check yourself, as that's the only way to make sure.

* * *

### Can I get banned for this?

In order to answer that question, we should take a closer look at **[Steam ToS](https://store.steampowered.com/subscriber_agreement)**. Steam doesn't prohibit using of multiple accounts, in fact, **[it allows it](https://support.steampowered.com/kb_article.php?ref=8625-WRAH-9030#share)** implying that you can use same mobile authenticator on more than one account. What it however doesn't allow is sharing accounts with other people, but we're not doing that here.

The only real point that considers ASF is the following:

> You may not use Cheats, automation software (bots), mods, hacks, or any other unauthorized third-party software, to modify or automate any Subscription Marketplace process.

The question is what in fact is Subscription Marketplace process. As we can read:

> An example of a Subscription Marketplace is the Steam Community Market

We're not modifying or automating subscription marketplace process, if by subscription marketplace we understand steam community market or steam store. However...

> Valve may cancel your Account or any particular Subscription(s) at any time in the event that (a) Valve ceases providing such Subscriptions to similarly situated Subscribers generally, or (b) you breach any terms of this Agreement (including any Subscription Terms or Rules of Use).

Therefore, as with every Steam software, ASF is not authorized by Valve and I cannot guarantee that you won't be suspended if Valve suddenly decides that they're banning accounts using ASF now. This is exceptionally unlikely considering the fact that ASF is being used on more than half a million of Steam accounts, but still a possibility, regardless of actual probability.

Especially because:

> In regard to all Subscriptions, Contents and Services that are not authored by Valve, Valve does not screen such third party content available on Steam or through other sources. Valve assumes no responsibility or liability for such third party content. Some third party application software is capable of being used by businesses for business purposes - however, you may only acquire such software via Steam for private personal use.

However, Valve clearly acknowledges "Steam idlers" existing, as stated **[here](https://support.steampowered.com/kb_article.php?ref=2117-ilzv-2837)**, so if you asked me, I'm pretty sure that if they weren't fine with them, they'd already do something instead of pointing out that they might cause problems VAC-wise. The key word here is **Steam** idlers, for example ASF, and not **game** idlers.

Please note that above is only our interpretation of Steam ToS and various points - ASF is licensed under Apache 2.0 License, which clearly states:

> Unless required by applicable law or agreed to in writing, ASF is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.

You're using this software at your own risk. It's very unlikely that you can get banned for that, but if you do, you can blame only yourself for that.

* * *

### Did somebody get banned for it?

**Yes**, we had at least a few incidents so far that resulted in some kind of Steam suspension. Whether ASF itself was the root cause or not is entirely different story that we'll probably never get to know.

One case was of a guy with over 1000+ bots getting trade banned (together with all bots), most likely due to excessive usage of `loot ASF` executed on all bots at once, or other suspicious one-side amount of trades in very short time.

> Hello XXX, Thank you for contacting Steam Support. It looks like this account was used to manage a network of bot accounts. Botting is a violation of the Steam Subscriber Agreement.

Please, use some common sense and don't assume that you can do such crazy things only because ASF allows you to do that. Doing `loot ASF` on over 1k of bots can be easily considered a **[DDoS](https://en.wikipedia.org/wiki/DDoS)** attack, and personally I'm not shocked that somebody got banned for such a thing. Please keep in mind some bare common sense and minimum of fair use in regards to Steam service, and *most likely* you'll be fine.

Another case was a guy with 170+ bots getting banned during Steam's 2017 Winter Sale.

> Your account was blocked for violation of the agreement of the subscriber Steam. Judging by the exchanges and other factors, this account was used to illegally collect collectible cards on Steam, as well as related and not only commercial activities. The account has been permanently blocked and Steam Support can not provide additional support on this issue.

This case is once again very hard to analyze, because of vague response from Steam support that barely offers any details. Based on my personal thoughts, this user probably exchanged Steam cards for some kind of money (level up bot?) or in some other way tried to cash out on Steam. In case you were unaware, this is also illegal according to Steam ToS.

Last case involved user with 120+ bots being banned for breach of **[Steam online conduct](https://store.steampowered.com/online_conduct?l=english)**.

> Hello XXX, Thank you for contacting Steam Support. This and other accounts were used for flooding our network infrastructure, which is a violation of Steam online conduct. The account has been permanently blocked and Steam Support can not provide additional support on this issue.

This case is a bit easier to analyze because of extra details provided by the user. Apparently the user was using **a very outdated ASF version** that included a bug causing ASF to send excessive number of requests to Steam servers. The bug itself did not exist at first but was activated due to Steam breaking change that was fixed in future version. **ASF is supported only in **[latest stable version](https://github.com/JustArchi/ArchiSteamFarm/releases/latest)** released on GitHub**. Software is written by humans, and humans tend to make mistakes. If the mistake has a global scope, it's quickly being patched up and released to all users as a bugfix. Valve won't suddenly ban half a million of ASF users due to my mistake, for obvious reasons. However, if you intentionally resign from using up-to-date ASF, then by definition you're in a very small minority of users that are **exposed to incidents like these** due to **no support**, as there is nobody watching over your outdated version of ASF, nobody fixing it and nobody ensuring that you won't get outright banned by just launching it. **Please use up-to-date software**, not only ASF, but all other applications as well.

* * *

All of the incidents above have one thing in common - ASF is just a tool and it's **your** decision how you're going to make use of it. You do not get banned for using ASF directly, but for **how** you're using it. It can be a helper tool idling just one single account, or a massive farming network made from thousands of bots. In any of those cases, I'm not offering legal advice, and you should decide yourself about your ASF usage in the first place. I'm not hiding any information that could help you, e.g. the fact that ASF got some people banned, as I have no reason to - it's your choice what you want to do with that information. If you ask me - use some common sense, avoid creating a massive network of idling bots, do not send hundreds of trades at the same time, always use up-to-date ASF version and you *should* be fine. Every single incident of this nature for **some reason** always happened to people running hundreds of accounts in the first place. Whether it's just a coincidence or some actual factor, that's up to you to decide. I'm not offering any legal advice, only giving you my thoughts that you can find useful, or disregard them entirely and operate only on facts linked above.

* * *

### What privacy information ASF discloses?

You can find detailed explanation in **[statistics](https://github.com/JustArchi/ArchiSteamFarm/wiki/Statistics)** section. You should review it if you care about your privacy, e.g. if you're wondering why accounts being used in ASF are joining our Steam group. ASF doesn't collect any sensitive information, and doesn't share it with any third-parties.

* * *

## Diversos

* * *

### I'm using unsupported OS such as 32-bit Windows, can I still use ASF V3?

Yes, and that version is not unsupported in any way, just not officially built. Check out **[compatibility](https://github.com/JustArchi/ArchiSteamFarm/wiki/Compatibility)** section for generic variant.

* * *

### ASF is great! Can I make a donation?

Yes, and we're very happy to hear that you're enjoying our project! You can find various donation possibilities under every **[release](https://github.com/JustArchi/ArchiSteamFarm/releases/latest)** and also **[on the main page](https://github.com/JustArchi/ArchiSteamFarm)**. It's nice to note that in addition to generic money donations we also accept Steam items, so nothing is stopping you from donating skins, keys or a small part of the cards that you've idled with ASF if you'd like to. Thank you in advance for your generosity!

* * *

### I'm using Steam parental PIN to protect my account, do I need to input it somewhere?

Yes, you must set it in `SteamParentalPIN` bot config property. This is mainly because ASF does access many protected parts of your Steam account and it's impossible for ASF to operate without it.

* * *

### I don't want ASF to farm any games by default, yet I want to use extra ASF features. Is this possible?

Yes, you can set `Paused` bot config property to `true` in order to launch ASF with paused cards farming module, then you can make use of extra ASF features, such as `GamesPlayedWhileIdle`.

* * *

### Can ASF minimize to tray?

ASF is a console app, there is no window to be minimized, because window is created for you by your OS. You can however use any third-party tool capable of doing so, such as **[RBTray](http://rbtray.sourceforge.net)** for Windows, or **[screen](https://linux.die.net/man/1/screen)** for Linux/OS X. Those are only examples, there are many other apps with similar functionality.

* * *

### Does using ASF preserve eligibility for receiving booster packs?

**Sim**. ASF is using the same method to log in to Steam network as the official client, therefore it also preserves ability to receive booster packs for accounts that are being used. However, it's not confirmed if logging in to Steam community is actually required or not, therefore in order to be sure I suggest to keep `OnlineStatus` on `Online`, at least until somebody confirms that using Steam network alone is enough. It should be, but I'm not sure.

* * *

### Is there any way to communicate with ASF?

Yes, through steam chat, or by using **[IPC](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC)**. Check out **[commands](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)** section for more info.

* * *

### I'd like to help with ASF translation, what do I need to do?

Thank you for your interest! You can find all details in our **[localization](https://github.com/JustArchi/ArchiSteamFarm/wiki/Localization)** section.

* * *

### I have only one (main) account added to ASF, can I still issue commands through steam chat?

**Yes**, it's explained in **[commands](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands#notes)** section. You can do so through Steam group chat.

* * *

### ASF seems to be working, but I'm not receiving any card drops!

Cards farming rate differs from game to game, as you can read in **[performance](https://github.com/JustArchi/ArchiSteamFarm/wiki/Performance)**. It takes a while, usually **several hours per game**, and you shouldn't expect cards to drop in a few minutes since launching a program. If you can see that ASF actively checks cards status, and switches the game after current one is fully idled, then everything works fine - you're probably referring to inventory notifications, which are automatically dismissed by ASF through `BotBehaviour` of `DismissInventoryNotifications` bot config property. Check out **[configuration](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration)** for details.

* * *

### How to completely stop ASF process for my account?

Simply shutdown the ASF process, for example by clicking [X] on Windows. If instead you want to stop a particular bot of your choice but keep other ones running, then take a look at `Enabled` **[bot config property](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#bot-config)**, or `stop` **[command](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)**. If you instead want to stop automatic idling process, yet keep ASF running for your account, then that's what `Paused` **[bot config property](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#bot-config)** and `pause` **[command](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)** is for.

* * *

### How many bots can I run with ASF?

ASF as a program doesn't have any upper limit of bot instances, however you're still being limited by Steam Network. Currently you can run up to 100-110 bots with single IP and single ASF instance. It is possible to run more bots with more IPs and more ASF instances. Keep in mind that if you're using that big amount of bots, you should control their number yourself (such as making sure that all of them in fact are logging in and working at the same time). Also notice that the limit above in general depends on many internal factors - it's approximation rather than strict limit - you will most likely be able to run more/less bots than specified above.

* * *

### Can I run more ASF instances then?

You can run as many ASF instances on one machine as you like, assuming every instance has its own directory and its own configs, and account used in one instance is not used in another one. However, ask yourself why you want to do that. ASF is optimized to handle a dozen, even a hundred of accounts at the same time, and launching those dozen of bots in their own ASF instances degrades performance, takes more OS resources, and causes lack of synchronization between bots - so for example you're more likely to hit `InvalidPassword/RateLimitExceeded` issue described below, as logging in requests are not being synchronized between ASF instances. Therefore, my **strong suggestion** is, always run maximum of one ASF instance per one IP/interface. If you have more IPs/interfaces, by all means you can run more ASF instances, every instance using its own IP/interface. If you don't, launching more ASF instances is totally pointless, and does not only degrade performance and takes more OS resources (such as memory), but also causes lack of synchronization and increased likehood of causing issues. You won't gain anything from launching more than 1 instance per a single IP/interface.

* * *

### What is the meaning of status when redeeming a key?

Status indicates how given redeem attempt turned out. There are many different statuses possible, most common ones include:

| Status                  | Descrição                                                                                                                                                                                                                      |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| NoDetail                | "OK" status indicating success - the key was successfully redemeed.                                                                                                                                                            |
| Timeout                 | Steam network didn't respond in given time, we don't know if the key was redeemed, or not (most likely was, but you can try again).                                                                                            |
| BadActivationCode       | The provided key is invalid (not recognized as any valid key by Steam network).                                                                                                                                                |
| DuplicateActivationCode | The provided key was already redeemed by some other account, or revoked by developer/publisher.                                                                                                                                |
| AlreadyPurchased        | Your account already owns `packageID` that is connected with this key. Keep in mind that this does not indicate whether the key is `DuplicateActivationCode` or not - only that it's valid and it wasn't used in this attempt. |
| RestrictedCountry       | This is region-locked key and your account is not in the valid region that is permitted to redeem it.                                                                                                                          |
| DoesNotOwnRequiredApp   | You can't redeem that key as you're missing some other app - mainly base game when you're attempting to redeem DLC package.                                                                                                    |
| RateLimited             | You made too many redeem attempts and your account was temporarily blocked. Try again in an hour.                                                                                                                              |

* * *

### Are you affiliated with any cards farming/idling service?

**Não**. ASF is not affiliated with any service and all such claims are false. Your Steam account is your property and you can use your account in whatever way you wish, but Valve clearly stated in **[official ToS](https://store.steampowered.com/subscriber_agreement/english)** that:

> You are responsible for the confidentiality of your login and password and for the security of your computer system. Valve is not responsible for the use of your password and Account or for all of the communication and activity on Steam that results from use of your login name and password by you, by any person to whom you may have intentionally or by negligence disclosed your login and/or password in violation of this confidentiality provision.

ASF is licensed on liberal Apache 2.0 License, which allows other developers to further integrate ASF with their own projects and services legally. However, such third-party projects utilizing ASF are not guaranteed to be secure, reviewed, appropriate or legal according to **[Steam ToS](https://store.steampowered.com/subscriber_agreement/english)**. If you want to know our opinion, **we strongly encourage you to NOT share ANY of your account details with third-party services**. If such service turns out to be a **typical scam**, you'll be left alone with the problem, most likely without your Steam account and ASF won't take any responsibility for third-party services claiming to be safe and secure, because ASF team did not authorize neither reviewed any of those. In other words, **you're using them at your own risk, against our suggestion made above**.

In addition to that, official Steam ToS clearly states that:

> You may not reveal, share or otherwise allow others to use your password or Account except as otherwise specifically authorized by Valve.

It's your account and your choice. Just don't say that nobody warned you. ASF as a program meets all rules mentioned above, as you're not sharing your account details with anyone, and you're using the program for your own personal use, but any other "cards farming service" does require from you your account credentials, so it also violates the rule above (actually several of them). Like with Steam ToS evaluation, we're not offering any legal advice, and you should decide yourself if you want to use those services, or not - according to us **it directly violates Steam ToS** and might result in suspension if Valve finds out. Like pointed out above, **we strongly recommend to NOT use any of such services**.

* * *

## Problemas

* * *

### ASF doesn't detect game `X` as available for farming, yet I know it includes Steam trading cards!

There are two main reasons here. First and most obvious reason is the fact that you're referring to **Steam store** where given game is announced as card drops enabled game. This is **wrong** assumption, as it simply states that the game **has** card drops included, but not necessarily this function for that game is **enabled** right away. You can read more about this in **[official announcement](https://steamcommunity.com/games/593110/announcements/detail/1954971077935370845)**.

In short, card drops icon in Steam store doesn't mean anything, check your **[badge pages](https://steamcommunity.com/my/badges)** for confirmation whether a game has card drops enabled or not - this is also what ASF is doing. If your game doesn't appear on the list as a game with cards possible to drop, then this game is **not** possible to idle, regardless of reason.

Second issue is less obvious, and it's the situation when you can see that your game indeed is available with card drops on your badge page, yet it's not being idled by ASF right away. Unless you're hitting some other bug, such as ASF being unable to check badge pages (described below), it's simply a cache effect and on ASF side Steam is still reporting outdated badges page. This issue should solve itself sooner or later, when cache gets invalidated. There is also no way to fix this on our side.

Of course, all of that assumes that you're running ASF with default untouched settings, since you could also add this game to idling blacklist, use `IdleRefundableGames` of `false` and so on.

* * *

### What is the difference between a warning and an error in the log?

ASF writes to its log a bunch of information on various logging levels. Our objective is to explain **precisely** what ASF is doing, including what Steam issues it has to deal with, or other problems to overcome. Most of the time not everything is relevant, this is why we have two major levels being used in ASF in terms of problems - a warning level, and error level.

General ASF rule is that warnings are **not** errors, therefore they should **not** be reported. A warning is an indicator to you that something potentially unwanted happen. Whether it was Steam not reacting, API throwing errors or your network connection being down - it's a warning, and it means we expected it to happen, so don't bother ASF development with it. Of course you're free to ask about them or get help by using our support, but you shouldn't assume that those are ASF errors worth reporting (unless we confirm otherwise).

Errors on the other hand indicate a situation that should not happen, therefore they're worth reporting as long as you made sure that it's not you who is causing them. If it's a common situation that we expect to happen, then it'll be converted to a warning instead. Otherwise, it's possibly a bug that should be corrected, not silently ignored, assuming it's not a result of your own technical issue. For example, removing core `ASF.json` file will throw an error, as ASF can't operate without that file, but it was you who removed it, so you should not report that error to us (unless you confirmed that ASF is wrong and the file is there).

In one TL;DR sentence - report errors, don't report warnings. You can still ask about warnings and receive help in our support sections.

* * *

### ASF can't start, the program window immediately closes after being launched!

If even `log.txt` is not being generated then you most likely forgot to install .NET Core prerequisites, as stated in **[setting up](https://github.com/JustArchi/ArchiSteamFarm/wiki/Setting-up#os-specific-setup)** guide. Other common problems might include trying to launch wrong ASF variant for your OS, or in other way missing native .NET Core runtime dependencies. If the console window closes too soon for you to read the message, then open independent console and launch ASF binary from there. For example on Windows, open ASF directory, hold `Shift`, right click inside the folder and choose "open command window here" (or powershell), then type into the console `.\ArchiSteamFarm.exe` and hit enter. This way you'll get precise message why ASF is not starting properly.

* * *

### ASF is kicking my Steam Client session while I'm playing! / `This account is logged on another PC`

This shows up as a message in Steam overlay that the account is being used somewhere else while you're playing. This issue can have two different reasons.

One reason is caused by broken packages (games) that specifically don't hold a playing lock properly, yet expect that lock to be possesed by the client. An example of such package would be Skyrim SE. Your Steam client launches the game properly, but that game doesn't register itself as being used. Because of that, ASF sees that it's free to resume the process, which it does, and that kicks you out of Steam network, as Steam suddenly detects that the account is being used in another place.

Second reason might come up if you're playing on your PC while ASF is waiting (especially on another machine) and you lose your network connection. In this case, Steam network marks you as offline and releases playing lock (like above), which triggers ASF (e.g. on another machine) into resuming farming. When your PC comes back online, Steam can't acquire playing lock anymore (that is now held by ASF, also similar to above) and shows the same message.

Both causes on the ASF side are actually very hard to workaround, as ASF simply resumes farming once Steam network informs it that account is free to be used again. This is what is happening normally when you close the game, but with broken packages this can happen immediately, even if your game is still running. ASF has no way to know whether you got disconnected, stopped playing a game or that you're still playing a game that doesn't hold playing lock appropriately.

The only proper solution to this problem is manually pausing your bot with `pause` before you start playing, and resuming it with `resume` once you're done. Alternatively you can just ignore the problem and act the same as if you played with offline Steam client.

* * *

### `Disconnected from Steam!` - I can't establish connection with Steam servers.

ASF can only **try** to establish connection with Steam servers, and it can fail due to many reasons, including lack of internet connection, Steam being down, your firewall blocking connection, third-party tools, incorrectly configured routes or temporary failures. You can enable `Debug` mode to check out more verbose log stating exact failure reasons, although usually it's simply caused by your own actions, such as using "CS:GO MM Server Picker" that blacklists a lot of Steam IPs, making it very hard for you to actually reach Steam network.

ASF will do its best to establish connection, which includes not only asking about updated list of servers but also trying another IP when last one fails, so if it's truly a temporary problem with some specific server or route, ASF will connect sooner or later. However, if you're behind firewall or in some other way unable to reach Steam servers, then obviously you need to fix it yourself, with potential help of `Debug` mode.

It's also possible that your machine is not able to establish connection with Steam servers using default protocol in ASF. You can alter protocols that ASF is permitted to use by modifying `SteamProtocols` global configuration property. For example, if you have problems reaching Steam with `TCP` protocol, then you can try `UDP` or `WebSocket`.

In a very unlikely situation of having incorrect servers being cached, for example because of moving ASF `config` folder from one machine to machine located in another country, deleting `ASF.db` in order to refresh Steam servers on next launch might help. Very often it's not needed and doesn't have to be done, as that list is automatically refreshed on first launch, as well as when the connection is established.

* * *

### `Could not get badges information, will try again later!`

Usually it means that you're using Steam parental PIN to access your account, yet you forgot to put it in ASF config. You must put valid PIN in `SteamParentalPIN` bot config property, otherwise ASF will not be able to access most of web content, therefore will not be able to work properly. Head over to **[configuration](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration)** in order to learn more about `SteamParentalPIN`.

Other reasons might include temporary Steam problem, network issue or likewise. If issue won't solve itself after several hours and you're sure that you configured ASF appropriately, feel free to let us know about that.

* * *

### ASF is failing with `Request failed despite of 5 tries` errors!

Usually it means that you're using Steam parental PIN to access your account, yet you forgot to put it in ASF config. You must put valid PIN in `SteamParentalPIN` bot config property, otherwise ASF will not be able to access most of web content, therefore will not be able to work properly. Head over to **[configuration](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration)** in order to learn more about `SteamParentalPIN`.

If parental PIN is not the reason, then this is a most common error, and you should get used to that - it simply means that ASF sent a request to Steam Network, and didn't get a valid response, in addition to that - in 4 retries. Usually it means that Steam is either down or is having some difficulties or maintenance - ASF is aware of such issues and you should not worry about them, unless they're happening constantly for longer than several hours, and other users do not have such problems.

How to check if Steam is being down? **[Steam Status](https://steamstat.us)** is an excellent source of checking if Steam **should be** up, if you notice errors, especially related to Community or Web API, then Steam is having difficulties, either leave ASF alone and let it do its job after a short while, or wait yourself.

That's however not always the case, as in some situations Steam issues might not be detected by Steam Status, for example such case happened when Valve broke HTTPS support for Steam Community 7th June 2016 - accessing **[SteamCommunity](https://steamcommunity.com)** through HTTPS was throwing an error. Therefore, do not blindly trust Steam Status either, it's best to check yourself if everything works as supposed to.

Lastly, if nothing helps you can always enable `Debug` mode and see yourself in ASF log why exactly requests are failing. For example, above HTTPS issue caused:

    <HTML><HEAD><TITLE>Error</TITLE></HEAD><BODY>
    An error occurred while processing your request.<p>
    

Which is clearly Steam issue and nothing to fix in ASF. You can always try to visit the link mentioned by ASF yourself and check if it works - if it doesn't, then you know why ASF can't access that either. If it does, and error doesn't go away after a day or two, it might be worth investigating and reporting.

Before doing that you should **make sure that the error is worth reporting in the first place**. If it's mentioned in this FAQ, such as trading-related issue, then that's out. If it's temporary issue that happened once or twice, especially when your network was unstable or Steam was down - that's out. However, if you were able to reproduce your issue several times in a row, across 2 days, restarted ASF as well as your machine in the process and made sure that there is no FAQ entry here to help resolve it, then this might be worth asking about.

* * *

### ASF seems to freeze and doesn't print anything on the console until I press a key!

You're most likely using Windows and your console has QuickEdit mode enabled. Refer to **[this](https://stackoverflow.com/questions/30418886/how-and-why-does-quickedit-mode-in-command-prompt-freeze-applications)** question on StackOverflow for technical explanation. You should disable QuickEdit mode by right clicking your ASF console window, opening properties, and unchecking appropriate checkbox.

* * *

### ASF can't accept or send trades!

Obvious thing first - new accounts start as limited. Until you unlock account by loading its wallet or spending 5$ in the store, ASF can't accept neither send trades using this account. In this case, ASF will state that inventory seems empty, because every card that is in it is non-tradable. It also won't be possible to receive any trade, as that part requires ASF to be able to fetch API key, and API key functionality is disabled for limited accounts. In short - trading is off for all limited accounts, no exceptions.

Next, if you do not use **[ASF 2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Two-factor-authentication)**, it's possible that ASF in fact accepted/sent trade, but you need to confirm it via your e-mail. Likewise, if you use classic 2FA, you need to confirm the trade via your authenticator. Confirmations are **mandatory** now, so if you don't want to accept them by yourself, consider either adding or importing your authenticator into ASF 2FA.

Also notice that you can trade only with your friends, and people with known trade link. If you're trying to initiate Bot->Master trade, such as `loot`, then you need to either have your bot on your friendlist, or your `SteamTradeToken` declared in Bot's config. Make sure that the token is valid - otherwise, you won't be able to send a trade.

Lastly, remember that new devices have 7-days trade lock, so if you've just added your account to ASF, wait at least 7 days - everything should work after that period. That limitation includes **both** accepting **and** sending trades. It does not always trigger, and there are people who can send and accept trades instantly. Majority of the people are affected though, and the lock **will** happen, even if you can send and accept trades through your steam client on the same machine. Just wait patiently, there's nothing you can do to make it faster.

And finally, keep in mind that one account can have only 5 pending trades to another one, so ASF will fail to send trades if you have 5 (or more) pending ones from that one bot to accept already. This is rarely a problem, but it's also worth mentioning, especially if you set ASF to auto-send trades, yet you're not using ASF 2FA and forgot to actually confirm them.

If nothing helped, you can always enable `Debug` mode and check yourself why requests are failing. Please note that Steam talks nonsense most of the time, and provided reason might not make any logical sense, or can be even entirely incorrect - if you decide to interpret that reason, make sure you have decent knowledge about Steam and its quirks. It's also quite common to see that issue with no logical reason, and the only suggested solution in this case is to re-add account to ASF (and wait 7 days again). Sometimes this issue also fixes itself *magically*, the same way it breaks. However, usually it's just either 7-days trade lock, temporary steam problem, or both. It's best to give it a few days before manually checking what is wrong, unless you have some urge to debug the real cause (and usually you'll be forced to wait anyway, because error message won't make any sense, neither help you in the slightest).

In any case, ASF can only **try** to send a proper request to Steam in order to accept/send trade. Whether Steam accepts that request, or not, is out of the scope of ASF, and ASF will not magically make it work. There's no bug related to that feature, and there is also nothing to improve, because logic is happening outside of ASF. Therefore, do not ask for fixing stuff that is not broken, and also do not ask why ASF can't accept or send trades - **I don't know, and ASF doesn't know either**. Either deal with it, or fix yourself.

* * *

### Why do I have to put 2FA/SteamGuard code on each login? / `Removed expired login key`

ASF uses login keys (if you kept `UseLoginKeys` enabled) for keeping credentials valid, the same mechanism that Steam uses - 2FA/SteamGuard token is required only once. However, due to Steam network issues and quirks, it's entirely possible that login key is not saved in the network, I've already seen such issues not only with ASF, but with regular steam client as well (a need to input login + password on each run, regardless of "remember me" option).

You could remove `BotName.db` (+ `BotName.bin`, if exists) of affected account and try to link ASF to your account once again, but that doesn't have to succeed. The real ASF-based solution is to import your authenticator as **[ASF 2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Two-factor-authentication)** - this way ASF can generate tokens automatically when they're needed, and you don't have to input them manually. Usually the issue magically solves itself after some time, so you can simply wait for that to happen. Of course you can also ask GabeN for solution, because I can't force Steam network to accept our login keys.

As a side note, you can also turn off login keys with `UseLoginKeys` config property set to `false`, but you should do that only if ASF has fully automated way to make initial login. Right now this is possible only with valid `SteamPassword` and **[ASF 2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Two-factor-authentication)**, since in this case we don't need to rely on login keys at all, as we have required login credentials (password and 2FA key) available on each login.

* * *

### I'm getting error: `Unable to login to Steam: InvalidPassword or RateLimitExceeded`

This error can mean a lot of things, some of them include:

- Invalid Login/Password combination (obviously)
- Expired login key used by ASF for logging in
- Too many failed login attempts in short period of time (anti-bruteforce)
- Too many login attempts in short period of time (rate-limiting)
- Requirement of captcha to log in (very likely to be caused by two reasons above)
- Any other reason Steam Network might have preventing you from logging in.

In case of anti-bruteforce and rate-limiting, problem will disappear after some time, so just wait and don't attempt to log in in the meantime. If you hit that issue frequently, perhaps it's wise to increase `LoginLimiterDelay` config property of ASF. Excessive program restarts and other intentional/non-intentional login requests definitely won't help with that issue, so try to avoid it if possible.

In case of expired login key - ASF will remove old one and ask for new one on next login (which will require from you putting 2FA token if your account is 2FA-protected. If your account is using ASF 2FA, token will be generated and used automatically). If you get this issue often, it's possible that Steam for some reason decided to ignore our login key save requests, as mentioned in the issue above. You might avoid this issue by not using login keys at all with `UseLoginKeys` config property, but we do not recommend going that way.

And lastly, if you used wrong login + password combination, obviously you need to correct this, or disable bot that is attempting to connect using those credentials. ASF can't guess on its own whether `InvalidPassword` means invalid credentials, or any of the reasons listed above, therefore it'll keep trying until it succeeds.

Keep in mind that ASF has its own built-in system to react accordingly to steam quirks, eventually it will connect and resume its job, therefore it's not required to do anything if the issue is temporary. Restarting ASF in order to magically fix problems will only make things worse (as new ASF won't know previous ASF state of not being able to log in, and try to connect instead of waiting), so avoid doing that unless you know what you're doing.

Finally, as with every Steam request - ASF can only **try** to log in, using your provided credentials. Whether that request will succeed or not is out of the scope and logic of ASF - there is no bug, and nothing can be fixed neither improved in this regard.

* * *

### `System.Threading.Tasks.TaskCanceledException: A task was canceled.`

This warning means that Steam did not answer to ASF request in given time. Usually it's caused by Steam networking hiccups and does not affect ASF in any way. In other cases it's the same as request failing despite of 5 tries. Reporting this issue makes no sense most of the time, as we can't force Steam to respond to our requests.

* * *

### `System.Net.Http.WinHttpException: A security error occurred`

This error happens when ASF can't establish secure connection with given server, almost exclusively because of SSL certificate mistrust.

In almost all cases this error is caused by **wrong date/time on your machine**. Every SSL certificate has issued date and expiry date. If your date is invalid and out of those two bounds then the certificate can't be trusted as potential MITM attack and ASF refuses to make a connection.

Obvious solution is to set the date on your machine appropriately. It's highly recommended to use automatic date synchronization, such as native synchronization available on Windows, or `ntpd` on Linux.

If you made sure that the date on your machine is appropriate and the error doesn't want to go away, then assuming it's not a temporary issue that should go away soon, SSL certificates that your system trusts might be out-of-date or invalid. In this case you should ensure that your machine can establish secure connections, for example by checking if you can access `https://github.com` with any browser of your choice, or CLI tool such as `curl`. If you confirmed that this works properly, feel free to post issue on our Steam group.

* * *

### ASF is being detected as a malware by my AntiVirus! What's going on?

**Ensure that you downloaded ASF from trusted source**. The only official and trusted source is **[ASF releases](https://github.com/JustArchi/ArchiSteamFarm/releases/latest)** page on GitHub (and this is also the source for ASF auto-updates) - **any other source is untrusted by definition and might contain malware added by other people** - you should not trust any other download location by definition, and ensure that your ASF always comes from us.

If you confirmed that ASF is downloaded from trusted source, then very likely it's simply a false positive. This **happened in the past**, **is happening right now**, and **will happen in the future**. We're already used to that and you shouldn't notify us about new false positives - notify developers of your AV instead, since it's not ASF issue in the first place.

If you're worried about actual safety when using ASF, then I suggest scanning ASF with many different AVs for actual detection ratio, for example through **[VirusTotal](https://www.virustotal.com)** (or any other web service of your choice like this).

If the AV that you're using falsely detects ASF as a malware, then **it's a good idea to send this file sample back to developers of your AV, so they can analyze it and improve their detection engine**, as clearly it's not working as good as you think it does. There is no issue in ASF code, and there is also nothing to fix for us, since we're not distributing malware in the first place, therefore it doesn't make any sense to report those false-positives to us. We highly recommend to send ASF sample for further analysis like stated above, but if you don't want to bother with it, then you can always add ASF to some kind of AV exceptions, disable your AV or simply use another one. Sadly, we're used to AVs being stupid, as every once in a while some AV detects ASF as a virus, which usually lasts very short and is being patched up quickly by the devs, but like we pointed out above - **it happened**, **happens** and **will happen** all the time. ASF doesn't include any malicious code, you can review ASF code and even compile from source yourself. We're not hackers to obfuscate ASF code in order to hide from AV heuristics and false positives, so do not expect from us to fix what is not broken - there is no "virus" for us to fix.