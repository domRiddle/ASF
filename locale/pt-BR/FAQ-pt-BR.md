# Perguntas frequentes (FAQ)

A seção de perguntas frequentes cobre respostas a questões comuns que você pode ter. For a less common matters, please visit our **[extended FAQ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Extended-FAQ)** instead.

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

O ASF se conecta a sua conta Steam através deum mini cliente Steam embutido usando as credenciais fornecidas. Após se conectar com sucesso, ele analisa sua página de **[insígnias](https://steamcommunity.com/my/badges)** a fim de encontrar jogos que estão disponíveis para coleta (Jogo pode dar mais X cartas). Após analisar todas as páginas e fazer a lista final de jogos que estão aptos, o ASF escolhe o algoritmo de coleta mais eficiente e inicia o processo. The process depends upon chosen **[cards farming algorithm](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)** but usually it consists of playing eligible game and periodically (plus on each item drop) checking if game is fully idled already - if yes, ASF can proceed with the next title, using the same procedure, until all games are fully farmed.

Tenha em mente que a explicação acima é simplificada e não descreve as dezenas de recursos e funções extras que o ASF oferece. Visit the rest of **[our wiki](https://github.com/JustArchiNET/ArchiSteamFarm/wiki)** if you want to know every ASF detail. Eu tentei fazê-la simples o bastante para que todos a entendam, sem entrar em detalhes técnicos - usuários avançados são encorajados a cavar mais fundo.

Agora como programa - o ASF oferece um pouco de magia. Em primeiro lugar, ele não precisa baixar nenhum arquivo de jogo, ele pode jogar os jogos imediatamente. Em segundo lugar, ele é inteiramente independente do seu cliente Steam normal - você não precisa ter o cliente Steam rodando, nem mesmo instalado. Em terceiro lugar, é uma solução automatizada - o que significa que o ASF faz tudo sozinho para você, sem a necessidade de dizer a ele o que fazer - o que te poupa aborrecimento e tempo. Por último, ele não tem que enganar a rede Steam emulando o processo (que, por exemplo, é o método usado pelo Idle Master), já que ele pode se comunicar diretamente com ele. Ele também é super rápido e leve, sendo uma solução surpreendente para aqueles que querem obter cartas facilmente e sem muita trabalheira - é especialmente útil deixá-lo rodando em segundo plano enquanto está fazendo outra coisa, ou mesmo jogando no modo offline.

Tudo isso é bom, mas o ASF também tem algumas limitações técnicas que são aplicadas pela Steam - nós não podemos rodar jogos que você não possui, nós não podemos rodar jogos para sempre a fim de obter cartas extras após o limite imposto e nós não podemos rodar jogos enquanto você está jogando. Tudo isso deve ser "óbvio", considerando a maneira como o ASF funciona, mas é bom notar que o ASF não tem super poderes e não vai fazer algo que é fisicamente impossível, então tenha isso em mente - é basicamente a mesma coisa que se você dissesse a alguém para se conectar na sua conta de outro PC e jogar esses jogos para você.

Então resumindo - o ASF é um programa que ajuda a pegar as cartas que você é apto a pegar, mas sem muita trabalheira. Ele também oferece várias outras funções, mas vamos nos ater a esta por enquanto.

* * *

### Tenho que colocar minhas credenciais de conta?

**Sim**. O ASF exige suas credenciais de conta da mesma forma que o cliente oficial da Steam, já que ele está usando o mesmo método para interagir com a rede Steam. No entanto, isso não significa que você tenha que colocar suas credenciais de conta nos arquivos de configuração do ASF, você pode usar o ASF com `SteamLogin` e/ou `SteamPassword` `null`/vazio, e colocar esses dados cada vez que abrir o ASF, quando for preciso (assim como várias outras credenciais de login, veja a seção configuração). Desta forma, seus dados não são salvos em lugar nenhum, mas é claro, assim o ASF não poderá auto-reiniciar sem a sua ajuda. ASF also offers several other ways of increasing your **[security](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security)**, so feel free to read that part of the wiki if you're advanced user. Se você não é, e não quer colocar suas credenciais de conta nas configurações do ASF, então simplesmente não faça isso, e coloque-as quando o ASF as pedir.

Tenha em mente que o ASF é uma ferramenta para seu uso pessoal e as suas credenciais nunca deixarão seu computador. Você também não estará compartilhando elas com ninguém, o que cumpre os termos de serviço da Steam - uma coisa muito importante da qual as pessoas esquecem. Você não vai mandar seus dados para nossos servidores ou o servidor de algum terceiro, somente diretamente para os servidores da Steam operados pela Valve.

* * *

### Quanto tempo tenho que esperar para ganhar as cartas?

**O quanto for preciso** - sério. Cada jogo tem uma dificuldade de coleta diferente, definida pelo(a) desenvolvedor(a) e/ou editora, e cabe totalmente a eles decidir o quão rápido as cartas serão fornecidas. A maioria dos jogos segue o padrão de 1 carta a cada 30 minutos de jogo, mas também há jogos que exigem que você jogar várias horas antes de dar uma carta. In addition to that, your account might be restricted from receiving card drops from games you didn't play for enough time yet, as stated in **[performance](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)** section. Não tente supor quanto tempo o ASF deve coletar de determinado título - não cabe a você, nem ao ASF decidir. Não há nada que você possa fazer para torná-lo mais rápido, e não há "bugs" relacionados a cartas não serem recebidas em tempo hábil - você não controla o processo de recebimento de cartas, nem o ASF. Na melhor das hipóteses, você receberá a média de 1 carta a cada 30 minutos. Na pior, você não receberá qualquer carta em menos de 4 horas desde que iniciou o ASF. Both of those situations are normal and covered in our **[performance](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)** section.

* * *

### A coleta está demorando muito, posso adiantá-la de alguma forma?

The only thing which heavily affects speed of farming is selected **[cards farming algorithm](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)** for your bot instance. Tudo o resto tem efeitos insignificantes e não vai deixar o processo de coletas mais rápido, enquanto algumas ações, tal como iniciar o processo do ASF várias vezes pode até **piorar**. If you really have an urge of making every damn second from farming process, then ASF allows you to fine-tune some core farming variables such as `FarmingDelay` - all of them are explained in **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)**. No entanto, como eu disse, o efeito é insignificante, e escolher o algorítimo de coleta de cartas adequado para determinada conta é a única escolha crucial que pode afetar fortemente a velocidade da coleta, tudo o resto é pura cosmética. Em vez de se preocupar com a velocidade de coleta, apenas abra o ASF e deixe ele fazer seu trabalho - posso assegurar-lhe que ele está fazendo isso da forma mais eficaz que eu poderia conseguir. Quanto menos você se importar, mais você ficará satisfeito.

* * *

### Mas o ASF disse que vai levar cerca de X!

O ASF te mostra um tempo aproximado com base no número de cartas que você precisa coletar e seu algoritmo escolhido - isso não garante em nada o tempo real que você gastará coletando, que geralmente é maior que esse, como o ASF assume a melhor das hipóteses, ele ignora falhas na rede Steam, quedas de internet, sobrecarga dos servidores Steam entre outros. Esse tempo deve ser considerado simplesmente como um indicador de quanto você deve esperar na melhor das hipóteses, e esse tempo real pode ser diferente, às vezes até de maneira significativa. Como dito acima, não tente adivinhar por quanto tempo um jogo ficará coletando, não cabe a você, nem ao ASF decidir.

* * *

### O ASF funciona no meu android/smartphone?

O ASF é um programa C# que requer a implementação funcional do .NET Core. Atualmente não há nenhuma compilação nativa do .NET Core própria para o Android, mas existem compilações para linux na arquitetura ARM próprias e funcionais, portanto, é totalmente possível usar algo como o **[Linux Deploy](https://play.google.com/store/apps/details?id=ru.meefik.linuxdeploy)** para instalar o Linux e, usar o ASF nesse Linux chroot normalmente.

É muito provável que no futuro vejamos .NET Core rodando diretamente no Android.

* * *

### O ASF pode coletar itens de jogos Steam, tais como CS:GO ou Unturned?

**Não**, isso é contra os Termos de Serviço e a Valve afirmou isso claramente com a última onda de banimentos da comunidade por coleta de item do TF2. O ASF é um programa de coleta automática de Cartas Colecionáveis Steam, não de intens de jogos - ele não tem qualquer capacidade de coletar itens de jogo e não temos planos de adicionar tal característica no futuro, nunca, principalmente por violar os termos de uso da Steam. Não peça isso - o melhor que você vai conseguir é uma denúncia de algum usuário ofendido e um problema por conta disso. O mesmo se aplica a todos os outros tipos de coleta, como os souvenires das transmissões de CS:GO por exemplo. O ASF é focado exclusivamente em cartas colecionáveis Steam.

* * *

### Posso escolher de quais jogos coletar?

**Sim**, de diversas formas. If you want to alter the default order of idling queue, then that's what `FarmingOrders` **[bot configuration property](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#bot-config)** can be used for. If you want to manually blacklist given games from being idled automatically, you can use idling blacklist which is available with `ib` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. If you'd like to idle everything but give some apps priority over everything else, that is what idling priority queue available with `iq` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** can be used for. And finally, if you want to idle specific games of your choice only, then you can use `IdlePriorityQueueOnly` **[bot configuration property](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#bot-config)** in order to achieve this, together with adding your selected apps to idling priority queue.

In addition to managing automatic cards farming module which was described above, you can also switch ASF to manual farming mode with `play` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**, or use some other misc external settings such as `GamesPlayedWhileIdle` **[bot configuration property](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#bot-config)**.

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

### Vale a pena usar o ASF se eu estiver usando o Idle Master e ele funciona bem para mim?

**Sim**. O ASF é muito mais confiável e inclui muitas funções **cruciais**, independente de como você coleta, e isso o IM simplesmente não oferece.

O ASF implementa uma lógica correta para **jogos não lançados** - o IM tentará rodar esses jogos, mesmo que eles ainda não tenham sido lançados. Claro, não há como rodar esses jogos até a data de lançamento, então seu processo de coleta ficará travado. Isso fará com que você tenha que adicioná-lo a blacklist, esperar pelo lançamento ou pular manualmente. Nenhuma dessas é uma boa solução e todos requerem sua atenção - o ASF pula (temporariamente) a coleta desses jogos e a retoma assim que forem lançados, evitando completamente problemas e lidando eficientemente com isso.

O ASF também trata corretamente vídeos de qualquer **espécie**. Há muitos videos na Steam que tem cartas mas são anunciados com o `appID` errado na página de insígnias, como o **[Double Fine Adventure](https://store.steampowered.com/app/402590)** por exemplo - o IM tentará rodar o `appID` errado, então você não vai conseguir nenhuma carta e o processo vai travar. Novamente, você vai ter que bloquear ou pular manualmente. Já o ASF descobre automaticamente o `appID` correto e faz a coleta.

Além disso, o ASF é **muito mais estável e confiável** quando se trata de problemas de rede e peculiaridades da Steam - ele funciona na maioria das vezes e, uma vez configurado, não requer a sua atenção em tudo, enquanto o IM trava muitas vezes, requer correções extras ou simplesmente não funciona. Ele também é totalmente dependente do seu cliente Steam, o que significa que ele pode não funcionar se você estiver com problemas no cliente. O ASF funcionará corretamente enquanto puder se conectar à rede Steam e não necessita que o cliente Steam esteja sendo executado, nem mesmo instalado.

Essas são 3 razões **muito importantes** pelas quais você deve considerar usar o ASF, já que elas afetam diretamente todos que coletam cartas Steam e não há como alguém dizer "isso não me preocupa", pois as manutenções do Steam e suas falhas acontecem com todos. Há outras dezenas de razão mais e menos importantes que você verá no resto desse FAQ. Então resumindo, **sim**, você deve usar o ASF mesmo se não precisar de nenhuma função extra do ASF que não exista no IM.

Ainda, o IM está oficialmente descontinuado e pode parar de funcionar completamente no futuro, sem ninguém se preocupando em consertá-lo, considerando a existência de soluções muito mais poderosas (não apenas o ASF). O IM já não funciona mais para um monte de gente, e esse número só está aumentando, não diminuindo. Você sempre deve evitar usar softwares obsoletos, não apenas o IM, mas qualquer outro programa descontinuado. Não ter manutenção ativa significa que ninguém se importa se ele funciona ou não, ninguém verifica isso e **ninguém se responsabiliza pela sua funcionalidade**, o que é uma questão crucial em termos de segurança.

* * *

### Que funcionalidades interessantes o ASF oferece que o Idle Master não?

Depende do que é "interessante" para você. Se você planeja rodar automaticamente mais que uma conta então a resposta é obvia já que o ASF permite que você faça isso da melhor forma, poupando recursos, aborrecimento e problemas de compatibilidade. No entanto, se você está perguntando isso é por que provavelmente você não tem essa necessidade, então vamos avalir outros benefícios que se aplicam a uma única conta usada no ASF.

Primeiro e mais importante, você tem algumas funcionalidades embutidas mencionadas **[acima](#vale-a-pena-usar-o-ASF-se-eu-estiver-usando-o-idle-master-e-ele-funciona-bem-parar-mim)** que são cruciais para o processo de coleta automático independente do seu objetivo final, e muitas vezes isso já é o suficiente para considerar usar o ASF. Mas você já sabe disso, então vamos avançar para algumas características mais interessantes:

- **Você pode coletar off-line** (função `Offline` de `OnlineStatus`). Coletar offline permite que você evite o estado "Em jogo" do Steam, rodando jogos automaticamente com o ASF enquanto mostra "Online" no Steam, assim seus amigos não saberão que o ASF está rodando um jogo para você. Essa é uma característica muito boa, pois ela permite que você fique online no seu cliente Steam sem atormentar seus amigos com constantes mudanças de jogos, ou sem confundi-los a acharem que você está jogando um jogo quando de fato não está. Se você respeita seus amigos esse ponto sozinho já faz valer a pena usar o ASF, mas é apenas o começo. Também é bom notar que esse recurso não tem nada a ver com as configurações de privacidade do Steam - se você iniciar um jogo o estado "Em jogo" vai aparecer normalmente para seus amigos, ele torna apenas a parte do ASF invisível e não afeta em nada a sua conta.

- **Você pode pular jogos reembolsáveis** (função `IdleRefundableGames`). O ASF tem uma lógica embutida para tratar jogos reembolsáveis e você pode configurá-lo para não rodar automaticamente esses jogos. Isso permite que você avalie se um jogo recém comprado na loja Steam valeu o seu dinheiro, sem que o ASF tente coletar cartas dele cedo demais. Se você jogar um jogo por mais de 2 horas ou se passarem 2 semanas desde sua compra, então o ASF vai rodar ele, pois ele já não será mais reembolsável. Até lá você já tem tempo suficiente para saber se gostou dele ou não e você pode facilmente pedir reembolso se for preciso, sem ter que bloqueá-lo manualmente ou não usar o ASF durante esse período.

- **Itens podem ser marcados automaticamente como recebidos** (função `DismissInventoryNotifications` de `BotBehaviour`). Rodar um jogo automaticamente com o ASF fará que sua conta receba cartas. Você já sabe que isso vai acontecer, então você pode deixar que o ASF limpe essas notificações inúteis, certificando-se que apenas notificações importantes chamem sua atenção. É claro, isso apenas se você quiser.

- **Você pode receber cartas dos eventos Steam automaticamente** (função `AutoSteamSaleEvent`). O ASF proporciona automação da lista de descobrimento e das votações nos Prêmios Steam durante as promoções do Steam, claro, se você quiser que ele faça isso. Isso poupa muito tempo durante os dias em que a promoção estiver ativa, e garante que você nunca perca as cartas diárias.

- **Você pode personalizar a ordem de coleta com diversas opções** (função `FarmingOrders`). Talvez você queira rodar seus jogos mais novos primeiro? Ou os mais velhos? De acordo com a quantidade de cartas disponíveis? Pelo nível das insígnias que você já fabricou? Horas jogadas? Em Ordem Alfabética? De acordo com os AppIDs? Ou talvez em ordem totalmente aleatória? Isto é inteiramente uma escolha sua.

- **O ASF pode te ajudar a completar seus conjuntos** (função `TradingPreferences` de `SteamTradeMatcher`). Com um pouco de configuração avançada, você pode transformar seu ASF em um bot que automaticamente aceita ofertas **[STM](https://www.steamtradematcher.com)**, te ajudando diariamente a completar seus conjuntos sem nenhuma interação de usuário. O ASF ainda incluis seu próprio módulo ASF 2FA que te permite importar seus autenticador móvel Steam e tornar o processo totalmente automático. Ou, talvez você queira aceitar manualmente e deixar que o ASF apenas prepare essas trocas para você? Mais uma vez, essa escolha é inteiramente sua.

- **ASF can redeem keys in background for you** (**[background games redeemer](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer)** feature). Talvez você tenha centenas de keys de vários bundles e está com preguiça de ativar todas, passando por um monte de janelas e concordando com termos e condições do Steam todas as vezes. Por que não copiar e colar sua lista de keys no ASF e deixr ele fazer o seu trabalho? O ASF vai ativar automaticamente todas essas keys em segundo plano, fornecendo depois umas lista indicando qual o resultado de cada tentativa de ativação. Além disso, se você tem centenas de keys, é certo que você atingirá o limite de ativações permitido pelo Steam uma hora ou outra e o ASF sabe disso, então ele vai esperar pacientemente o fim do bloqueio e retomar de onde parou.

We could now go on and on with entire **[ASF wiki](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Home)**, pointing out every single feature of the program, but we have to draw a line somewhere. É isso, esta é uma lista de recursos que você pode desfrutar como usuário do ASF, onde só um desses poderia facilmente ser considerado a principal razão para nunca olhar para trás, e na verdade listamos **vários** deles, há muito mais que você pode aprender na nossa wiki. E nós ainda nem entramos em detalhes de coisas como a API do ASF, que te permite codificar seus próprios comandos ou gestões gloriosas de bots, pois quisemos manter isso simples.

* * *

### O ASF é mais rápido que o Idle Master?

**Sim**, embora a explicação seja bastante complicada.

Em cada novo processo gerado e finalizado no seu sistema, o cliente Steam automaticamente envia uma solicitação que contém todos os jogos que você está jogando atualmente - dessa forma a rede Steam pode calcular as horas jogadas e liberar as cartas. No entanto, a rede Steam conta seu tempo de jogo em intervalos de 1 segundo, e enviar uma nova solicitação redefine o estado atual. Em outras palavras, se você gerar/matar um novo processo a cada 0,5 segundos você nunca vai conseguir uma carta porque o cliente Steam vai enviar uma nova solicitação a cada 0,5 segundos e a rede Steam nunca vai contar nem 1 segundo de jogo. Além disso, por conta da forma que o sistema operacional funciona, é muito comum ver novos processos sendo gerados/finalizados sem que você tenha feito algo, então mesmo que você não esteja usando seu PC - há muitos processos trabalhando em segundo plano, iniciando/terminando outros processos o tempo todo. O Idle Master é baseado no cliente Steam, então esse mecanismo é afetado se você o estiver usando.

O ASF não é baseado no cliente Steam, ele tem sua própria implementação do cliente Steam. Graças a isso, o ASF não cria nenhum processo, mas simplesmente envia um pedido real para a rede Steam indicando que começamos a jogar um jogo. Esse é o mesmo pedido que seria enviado pelo cliente Steam, mas como nós temos total controle sobre o cliente Steam do ASF nós não precisamos criar novos processos, nem imitar o cliente Steam no envio de solicitações em cada mudança de processo, então o mecanismo descrito acima não nos afeta. Graças a isso, nós nunca interrompemos aquele intervalo de 1 segundo no lado do servidor Steam, e isso aumenta nossa velocidade de coleta.

* * *

### Mas a diferença é realmente notável?

Não. As interrupções que acontecem com o cliente Steam normal e com o Idle Master tem muito pouco efeito na coleta de cartas, portanto não é uma diferença perceptível que tornaria o ASF superior.

No entanto, **há** uma diferença, que pode ser notada, pois dependendo de como seu SO for carregado, as cartas **serão** coletadas mais rapidamente, os ganhos podem variar de alguns segundos a vários minutos na pior das hipóteses. Embora eu não consideraria usar o ASF só porque ele coleta mais rápido já que tanto o ASF quanto o Idle Master são afetados pela for que a rede Steam funciona, o ASF apenas interage com a redem steam de forma mais eficaz, enquanto o Idle Master não pode controlar o que o cliente Steam faz (por isso é culpa do Idle Master, mas do próprio cliente Steam).

* * *

### O ASF consegue rodar vários jogos ao mesmo tempo?

**Yes**, although ASF knows better when to use that feature, based on selected **[cards farming algorithm](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)**. Você não tem escolha direta sobre o algorítimo, mas você pode sugerir um ao ASF configurando a propriedade corretamente. Você deve se focar na configuração do ASF e deixar os algorítimos decidirem qual a forma melhor otimizada para atingir o objetivo.

* * *

### O ASF pode alternar rapidamente entre jogos?

**No**, ASF doesn't support, neither encourages usage of **[Steam glitches](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance#steam-glitches)**.

* * *

### O ASF pode rodar cada jogo por X horas antes de cartas serem adicionadas?

**Não**, a mudança no sistema de cartas do Steam foi feito para lutar contra falsas estatísticas e jogadores fantasmas. O ASF não contrubui pra isso mais que o necessário, adicionar tal recurso não está planejado e não vai acontecer.

* * *

### Posso jogar algum jogo enquanto o ASF estiver coletando?

**Não**. Diferentemente do IM, o ASF tem um cliente independente do Steam embutido, e a rede Steam só permite **um cliente Steam por vez** jogando um jogo. No entanto você pode desconectar o ASF sempre que quiser apenas iniciando um jogo (e clicando em "OK" quando perguntado se a rede Steam deve desconectar o outro Cliente) - O ASF então vai esperar pacientemente até que você termine o jogo, e retomará o processo após isso. Como alternativa, você pode jogar no modo offline sempre que quiser, se isso for satisfatório para você.

Tenha em mente que a taxa de coleta de cartas quando se joga múltiplos jogos é próxima a 0, portanto não há benefícios diretos em existir essa possibilidade como no IM, enquanto há boas razões de não interferir com outros jogos em execução no ASF, o que pode ser importante, por exemplo, por causa do VAC.

* * *

## Segurança / Privacidade / VAC / Banimentos / Termos de serviço

* * *

### Posso ser banido pelo VAC por isso?

Não, isso não é possível porque o ASF (diferente do Idle Master ou SAM) não interfere com o cliente Steam nem cou seus processos. É fisicamente impossível tomar um banimento VAC por usar o ASF, mesmo jogando em servidores seguros enquanto o ASF estiver rodando - isso porque **o ASF nem mesmo precisa que o cliente Steam esteja instalado** para funcionar corretamente. O ASF é o único programa de coleta que pode garantir ser livre de banimento VAC.

* * *

### Usar o ASF pode impedir que eu jogue em servidores protegidos pelo VAC, como descrito **[aqui](https://support.steampowered.com/kb_article.php?ref=2117-ilzv-2837)**?

O ASF não precisa que o cliente Steam esteja sendo executado e nem mesmo que ele seja instalado. De acordo com esse conceito, ele **não** deve causar nenhum problema relacionado ao VAC, pois o ASF garante não interferir com o cliente Steam nem seus processos - esse é o ponto principal quando se fala na garantia que o ASF oferece em termos do VAC.

De acordo com usuários e pelo que sei, esse é o melhor caso agora, já que ninguém reportou qualquer problemas como os descritos no link acima enquanto usava o ASF. Tamb~em não pudemos reproduzir tal comportamento com o ASF, enquanto pudemos com o Idle Master.

No entanto, tenha em mente que a Valve ainda pode adicionar o ASF a sua lista de bloqueio um dia, mas seria completamente absurdo, pois você ainda poderia jogar jogos protegidos pelo VAC em seu PC enquanto roda o ASF em um servidor por exemplo. Portanto tenho plena certeza de que eles sabem que o ASF não é suspeito e eles não vão dificultar nossas vidas bloqueando o ASF sem motivo. Então, no pior dos casos você seria impossibilitado de jogar, como descrito acima, pois a garantia de o ASF ser livre do VAC ainda permanece se a Steam bloquear o binário do ASF ou não (e você ainda poderia executar o ASF em outra máquina onde o cliente Steam não estiver instalado). No momento não há motivo para fazer isso e esperamos que continue assim.

* * *

### Ele é seguro?

Se você pergunta se o ASF é seguro como um software, que ele não vai causar qualquer dano ao seu computador, não vai roubar seus dados privados, instalar vírus ou qualquer outra coisa assim - a resposta é sim. Seu código é aberto e os executáveis distribuídos são sempre compilados por **[ ferramentas de integração contínua automáticas e confiáveis](https://pt.wikipedia.org/wiki/Automa%C3%A7%C3%A3o_de_compila%C3%A7%C3%A3o)** de **[ fontes disponíveis publicamente](https://pt.wikipedia.org/wiki/Software_de_c%C3%B3digo_aberto)** e não pelos próprios desenvolvedores. Cada compilação pode ser reproduzida seguindo nosso script de compilação e terá resultado exatamente igual, um executável de código IL (binário) **[determinístico](https://en.wikipedia.org/wiki/Deterministic_system)**.

Se por algum motivo você não acredita em nossas compilações você pode compilar e usar o ASF pela fonte, incluindo todas as bibliotecas que o ASF usa (como o SteamKit2), que também são de código aberto. Porém, no fim, é sempre uma questão de confiança no(s) desenvolvedor(es) do programa, então é você quem deve decidir se considera o ASF seguro ou não, provavelmente apoiando sua decisão com argumentos técnicos. Não acredite cegamente em algo só porque eu disse - verifique por sua conta, já que essa é a única maneira de ter certeza.

* * *

### Posso banido por isso?

Para poder responder essa pergunta nós devemos dar uma olhada no **[Acordo de Assinatura do Steam](https://store.steampowered.com/subscriber_agreement)**. O Steam não proíbe o uso de várias contas, na verdade **[ela permite isso](https://support.steampowered.com/kb_article.php?ref=8625-WRAH-9030#share)** quando diz que você pode usar o mesmo autenticador móvel em mais de uma conta. O que ela não permite é compartilhar sua conta com outras pessoas, e isso nós não fazemos.

O único ponto que realmente considera o ASF é o seguinte:

> Você não poderá usar métodos de Trapaça, software de automação (bots), mods, hacks, ou qualquer software não autorizado de terceiros, para modificar ou automatizar qualquer processo do Mercado de Assinaturas.

A questão é, de fato, o que é o Mercado de Assinaturas. Como se pode ler:

> Um exemplo de Mercado de Assinaturas é o Mercado da Comunidade Steam

Não estamos modificando ou automatizando o processo do Mercado de Assinaturas se entendamos por Mercado de Assinaturas o Mercado da Comunidade ou a Loja Steam. No entanto...

> A Valve poderá cancelar sua Conta ou qualquer Assinatura específica a qualquer momento, caso (a) a Valve não forneça mais tais Assinaturas a Assinantes em geral em igual situação, ou (b) você violar quaisquer termos deste Acordo (incluindo quaisquer Termos de Assinatura ou Regras de Uso).

Portanto, assim como todos os softwares da Valve, o ASF não é autorizado por ela e não posso garantir que você não vai ser suspenso se a Valve decidir de repente que vão banir contas que usem o ASF. Isso é extremamente improvável, considerando o fato de que o ASF está sendo usado em mais de meio milhão de contas Steam, mas ainda é uma possibilidade, independentemente da probabilidade real.

Especialmente porque:

> Em relação a todas as Assinaturas, Conteúdos e Serviços que não sejam de autoria da Valve, a Valve não examina esses conteúdos de terceiros disponíveis no Steam ou por meio de outras fontes. A Valve não assume qualquer responsabilidade ou passivo por esses conteúdos de terceiros. É possível que determinados softwares aplicativos de terceiros sejam usados pelas empresas para propósitos empresariais; contudo, você poderá adquirir esse Software apenas por meio do Steam para uso privado.

No entanto, a Valve reconhece claramente a existência de "Steam Idlers" (programas que rodam jogos automaticamente) conforme afirmado **[aqui](https://support.steampowered.com/kb_article.php?ref=2117-ilzv-2837)**, então se você me perguntasse eu diria que tenho certeza de que se a Valve não os quer, eles já teriam feito algo mais concreto que apenas apontar que isso pode causar problemas com o VAC. A palavra chave aqui é **Steam** idlers, por exemplo o ASF, e não **game** idlers.

Note que o que foi citado acima é apenas a nossa interpretação do Acordo de Assinatura do Steam e vários pontos; o ASF é licenciado pela Licença Apache 2.0, que afirma claramente:

> A menos que exigido por lei ou acordado por escrito, o ASF é distribuído "COMO ESTÁ", SEM GARANTIAS OU CONDIÇÕES DE QUALQUER TIPO, expressas ou implícitas.

Você está usando este software por sua conta e risco. É muito improvável que você seja banido por isso, mas se isso acontecer, você pode culpar apenas a si mesmo.

* * *

### Alguém já foi banido por isso?

**Sim**, tivemos pelo menos alguns incidentes até agora que resultaram em algum tipo de suspensão do Steam. Se o ASF em si foi a causa principal ou não é outra história que nós provavelmente nunca saberemos.

Um dos casos foi de um cara com mais de 1000 bots que teve um bloqueio de trocas (juntamente com todos os bots), provavelmente devido ao uso excessivo do `loot ASF` executado em todos os bots de uma vez, ou por muitas trocas suspeitas onde apenas um lado tinha vantagem em um curtíssimo período de tempo.

> Olá, XXX, Obrigado por contatar o suporte Steam. Parece que esta conta foi usada para gerenciar uma rede de contas bot. O uso de bots viola o Acordo de Assinatura do Steam.

Por favor, use o bom senso e não assuma que você pode fazer essas coisas malucas só porque o ASF permite que você faça isso. Usar o `loot ASF` em mais de mil de bots pode ser facilmente considerado um ataque **[DDoS](https://pt.wikipedia.org/wiki/Ataque_de_nega%C3%A7%C3%A3o_de_servi%C3%A7o#Ataque_distribu%C3%ADdo)** e eu pessoalmente não estou chocado que alguém foi banido para uma coisa dessas. Por favor, use sempre o bom senso e um mínimo de uso justo em relação ao serviço do Steam, e *provavelmente* você estará seguro.

Outro caso foi de um cara com mais de 170 bots ser banido durante a Promoção de Inverno 2017 do Steam.

> Sua conta foi bloqueada por violação do Acordo de Assinatura do Steam. Julgando pelas trocas e outros fatores, essa conta foi usada para coletar ilegalmente cartas colecionáveis do Steam, assim como atividades comerciais relacionadas, entre outras. A conta foi bloqueada permanentemente e o suporte Steam não pode fornecer suporte adicional sobre esta questão.

Este é mais um caso muito difícil de analisar, por causa resposta vaga do suporte Steam que mal oferece os detalhes. Baseado em meus pensamentos pessoais, este usuário provavelmente trocou cartas Steam para algum tipo de dinheiro (algum bot para subir de nível?) ou de alguma outra maneira tentou retirar fundos do Steam. Caso você não saiba isso também é ilegal de acordo com o Acordo de Assinatura do Steam.

O último caso envolveu um usuário com mais de 120 bots ser banido por quebrar o **[Código de Conduta Online do Steam](https://store.steampowered.com/online_conduct?l=brasilian)**.

> Olá, XXX, Obrigado por contatar o suporte Steam. Essa e outras contas foram usadas para inundar nossa infraestrutura de rede, o que é uma violação da conduta online do Steam. A conta foi bloqueada permanentemente e o suporte Steam não pode fornecer suporte adicional sobre esta questão.

Este caso é um pouco mais fácil de analisar por causa de detalhes extras fornecidos pelo usuário. Aparentemente o usuário estava usando **uma versão muito desatualizada do ASF** que incluía um bug que fazia com que o ASF enviasse um número excessivo de solicitações para os servidores Steam. O bug em si não existia no início, mas foi ativado devido a uma mudança no Steam que causou falhas e que foi corrigida na próxima versão. **ASF is supported only in **[latest stable version](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** released on GitHub**. O software é escrito por humanos e os seres humanos podem cometer erros. Se o erro tem um escopo global ele é rapidamente consertado e lançado a todos os usuários em uma versão revisada. A Valve não vai banir meio milhão de usuários do ASF repentinamente devido a um erro meu, por razões óbvias. No entanto, se você intencionalmente não quer usar a versão atualizada do ASF, então você faz parte de uma minoria muito pequena de usuários que estão **expostos a incidentes como esse** devido a **falta de suporte**, já que ninguém está se preocupando com a sua versão desatualizada, ninguém vai consertá-la nem garantir que você não seja banido apenas por executá-la. **Por favor, utilize o software atualizado**, não só o ASF, mas todos os outros aplicativos também.

* * *

Todos os incidentes acima têm uma coisa em comum - o ASF é apenas uma ferramenta e é **sua** a decisão de como utilizá-la. Você não será banido por usar o ASF, mas por **como** você o usa. Ele pode ser uma ferramenta auxiliar na coleta de apenas uma conta ou de uma rede massiva de coleta com milhares de bots. Em qualquer dos casos, eu não estou oferecendo aconselhamento legal e você deve decidir por sua conta como você usará o ASF. Não estou escondendo nenhuma informação que poderia te ajudar, o fato do ASF ter causado o banimento de algumas pessoas por exemplo, uma vez que eu não tenho motivo para isso; é escolha sua o que você quer fazer com essa informação. Se você me perguntar - use o bom senso, evite criar uma rede gigantesca de bots de coleta, não envie centenas de trocas ao mesmo tempo, sempre utilize o ASF atualizado e você *deverá* ficar tranquilo. Todo incidente dessa natureza aconteceu por **algum motivo** com pessoas que rodavam centenas de contas. Se isso é apenas coincidência ou algum fator real, cabe a você decidir. Não estou oferecendo qualquer aconselhamento legal, estou apenas compartilhando meus pensamentos, você pode achá-los úteis ou ignorá-los totalmente e considerar apenas os fatos acima relacionados.

* * *

### Quais informações de privacidade o ASF divulga?

You can find detailed explanation in **[statistics](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Statistics)** section. Você deve revê-la se você se preocupa com sua privacidade, por exemplo, se você está se perguntando por que contas usadas no ASF estão entrando em nosso grupo Steam. O ASF não coleta quaisquer informações confidenciais e não as compartilha com quaisquer terceiros.

* * *

## Diversos

* * *

### Estou usando um sistema operacional não suportado, como Windows 32-bit por exemplo, ainda posso usar o ASF V3?

Sim e essa versão não tem falta de suporte, ela só não é oficialmente compilada. Check out **[compatibility](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility)** section for generic variant.

* * *

### O ASF é maravilhoso! Posso fazer uma doação?

Sim, e estamos muito felizes em saber que você está gostando do nosso projeto! You can find various donation possibilities under every **[release](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** and also **[on the main page](https://github.com/JustArchiNET/ArchiSteamFarm)**. Vale notar que além de doações em dinheiro nós também aceitamos itens do Steam, então nada te impede de doar skins, chaves ou uma pequena parte das cartas que você coletou com o ASF caso você queira. Obrigado desde já por sua generosidade!

* * *

### Eu uso o PIN do modo família para proteger minha conta, eu preciso colocá-lo em algum lugar?

Yes, you must set it in `SteamParentalCode` bot config property. Isso porque o ASF acessa muitas partes da sua conta Steam que são protegidas e é impossível que o ASF opere sem isso.

* * *

### Eu não quero que o ASF colete de nenhum jogo por padrão, mas ainda quero usar os recursos extras dele. É possível?

Sim, você pode definir o parâmetro `Paused` na configuração do bot para `true` para que o ASF seja iniciado com o módulo de coleta de cartas parado, então você pode usar os recursos extras normalmente, tal como `GamesPlayedWhileIdle`.

* * *

### Posso minimizar o ASF para a bandeja?

O ASF é um aplicativo de console, não há janela para ser minimizada pois as janelas são criadas para você pelo seu SO. No entanto você pode usar qualquer ferramenta de terceiros capaz de fazer isso, tal como **[RBTray](http://rbtray.sourceforge.net)** para Windows ou **[screen](https://linux.die.net/man/1/screen)** para Linux/OS X. Essas são apenas exemplos, há muitas outras ferramentas com o mesmo recurso.

* * *

### Usar o ASF mantém a elegibilidade para receber pacotes de cartas?

**Sim**. O ASF usa o mesmo método para se conectar a rede Steam que o cliente oficial, então ele também preserva a habilidade de receber pacotes para as contas que estão sendo usadas. No entanto, não há confirmação de que há a necessidade de se conectar a rede Steam para isso, então para ter certeza eu sugiro manter `OnlineStatus` em `OnlineStatus`, a menos até que alguém confirme que apenas usar a rede Steam já é o suficiente. Deveria ser, mas não tenho certeza.

* * *

### Existe alguma maneira de se comunicar com o ASF?

Yes, through steam chat, or by using **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)**. Check out **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** section for more info.

* * *

### Eu gostaria de ajudar com a tradução do ASF, o que eu preciso fazer?

Obrigado pelo seu interesse! You can find all details in our **[localization](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Localization)** section.

* * *

### Tenho apenas uma conta (principal) adicionada ao ASF, eu posso emitir comandos através do chat Steam?

**Yes**, it's explained in **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands#notes)** section. Você pode fazer isso através do chat em grupo do Steam.

* * *

### O ASF parece estar funcionando, mas eu não estou conseguindo cartas!

Cards farming rate differs from game to game, as you can read in **[performance](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)**. Demora um pouco, geralmente **várias horas por jogo**, e você não deve esperar que as cartas apareçam em seu inventário apenas alguns minutos após inciar o programa. Se você ver que o ASF checa ativamente o estado das cartas e troca de jogo após o atual ser totalmente explorado, então tudo está correto; você provavelmente deve estar se referindo as notificações do seu inventário que são dispensadas automaticamente pelo ASF através de `DismissInventoryNotifications` do parâmetro de configuração do bot `BotBehaviour`. Check out **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** for details.

* * *

### Como parar completamente o processo ASF para minha conta?

Simplesmente feche o processo ASF, por exemplo, clicando em [X] no Window. If instead you want to stop a particular bot of your choice but keep other ones running, then take a look at `Enabled` **[bot config property](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#bot-config)**, or `stop` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. If you instead want to stop automatic idling process, yet keep ASF running for your account, then that's what `Paused` **[bot config property](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#bot-config)** and `pause` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** is for.

* * *

### Quantos bots posso rodar com o ASF?

O ASF em si não tem qualquer limite máximo de contas bot, no entanto você será limitado pela rede Steam. Atualmente você pode rodar cerca de 100 a 110 bots por IP e por instância do ASF. É possível executar mais bots com mais IPs e mais instâncias do ASF. Tenha em mente que se você estiver usando uma grande quantidade de bots, você deve controlar o número deles por sua conta (por exemplo, certificando-se de que todos eles são de fato se conectando e trabalhando ao mesmo tempo). Também note que o limite acima em geral depende de muitos fatores internos - ele é uma aproximação e não um limite estrito - você provavelmente será capaz de executar mais/menos bots do que o especificado acima.

* * *

### Então eu posso rodar mais instâncias do ASF?

Você pode executar quantas instâncias do ASF você quiser em uma máquina, assumindo que cada instância tenha sua própria pasta e suas próprias configurações, e que uma conta usada em uma instância não seja usada em outra. No entanto, pergunte-se por que você quer fazer isso. O ASF é otimizado para lidar com uma dúzia, ou até mesmo uma centena de contas ao mesmo tempo, e iniciar essas dúzias de bots em suas próprias instâncias do ASF afeta o desempenho, toma mais recursos do SO e cria uma falta de sincronização entre os bots; assim, por exemplo você é mais susceptível de atingir os limites `InvalidPassword/RateLimitExceeded` descritos abaixo, uma vez que os os registo de pedidos não estão sendo sincronizados entre instâncias ASF. Portanto, eu **sugiro fortemente** sempre executar o máximo de uma instância ASF por IP/interface. Se você tiver mais IPs/interfaces, você pode livremente executar mais instâncias do ASF, cada instância usando seu próprio IP/interface. Caso contrário, executar mais instâncias do ASF é totalmente inútil, e não faz nada além de diminuir a performance e tomar mais recursos do SO (como memória), e também causa falta de sincronização e aumenta a chance de causar problemas. Você não ganha nada em executar mais que 1 instância por IP/interface.

* * *

### Qual o significado do estado quando se resgata uma key?

O estado indica como determinada tentativa de resgate acabou. Há muitos estados possíveis, os mais comuns incluem:

| Estado                  | Descrição                                                                                                                                                                                               |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| NoDetail                | Estado "OK" que indica sucesso; a key foi resgatada com sucesso.                                                                                                                                        |
| Timeout                 | A rede Steam não respondeu no tempo limite, não sabemos se a key foi resgatada ou não (provavelmente sim, mas você pode tentar de novo).                                                                |
| BadActivationCode       | A key fornecida é inválida (não reconhecida como uma key válida pela rede Steam).                                                                                                                       |
| DuplicateActivationCode | A key fornecida já foi resgatada por alguma outra conta, ou revogada pelo desenvolvedor/editora.                                                                                                        |
| AlreadyPurchased        | Sua conta já possui o `packageID` que está conectado a essa key. Note que isso não indica se a key é um caso de `DuplicateActivationCode` ou não - só que ela é válida e não foi usada nesta tentativa. |
| RestrictedCountry       | Esta key tem bloqueio regional e sua conta não é da região que tem permissão para resgatá-la.                                                                                                           |
| DoesNotOwnRequiredApp   | Você não pode resgatar essa key pois está faltando algum outro aplicativo necessário; isso ocorre principalmente quando você tenta resgatar uma DLC sem ter o jogo base.                                |
| RateLimited             | Você fez muitas tentativas de resgate e sua conta foi temporariamente bloqueada. Tente novamente em 1 hora.                                                                                             |

* * *

### Vocês são afiliados a algum serviço de coleta de cartas?

**Não**. O ASF não é afiliado a nenhum serviço e qualquer afirmação acerca disso é falsa. A sua conta Steam é propriedade sua e você pode usar sua conta da maneira que quiser, mas a Valve diz claramente no **[Acordo de Assinatura do Steam](https://store.steampowered.com/subscriber_agreement/english)** que:

> Você é o responsável pela confidencialidade de seu nome de login e senha assim como pela segurança do seu sistema de computador. A Valve não é responsável pelo uso da sua senha e Conta nem por todas as comunicações e atividades no Steam que resultem do uso de seu nome de login e senha por você, por qualquer pessoa a qual tenha, de forma intencional ou não, divulgado o seu login e/ou senha em violação à disposição de confidencialidade.

O ASF é licenciado pela a licença liberal Apache 2.0, que permite que outros desenvolvedores integrem legalmente o ASF com seus próprios projetos e serviços. No entanto, não garantimos que tais projetos de terceiros utilizando o ASF sejam seguros, revisados, apropriados ou legais de acordo com o **[Acordo de Assinatura do Steam](https://store.steampowered.com/subscriber_agreement/?l=brazilian)**. Se você quer saber a nossa opinião, **nós incentivamos fortemente você a NÃO compartilhar QUAISQUER detalhes de conta com serviços de terceiros**. Se acontecer de tal serviço ser um **tipo de fraude**, você vai estar sozinho com o problema, provavelmente sem sua conta Steam e o ASF não assume qualquer responsabilidade por serviços de terceiros que aleguem ser seguros, pois a equipe do ASF não autorizou nem revisou qualquer um desses. Em outras palavras, **você os esta usando por sua conta e risco, contra a sugestão feita acima**.

Além disso, o Acordo de Assinatura do Steam diz claramente que:

> Você não poderá revelar, compartilhar ou de outra forma permitir que outras pessoas usem sua senha ou Conta, exceto se for especificamente autorizado de outra forma pela Valve.

É seu problema e sua escolha. Só não diga que ninguém te avisou. O ASF em si cumpre todas as regras mencionadas acima, já que você não está compartilhando detalhes da sua conta com ninguém, e você está usando o programa para seu uso pessoal, mas qualquer outro "serviço de coleta de cartas" exigirá suas credenciais de conta, portanto, também viola a regra acima (na verdade várias delas). Assim como na avaliação do Acordo de Assinatura do Steam, não estamos oferecendo qualquer aconselhamento legal, e você deve decidir por sua conta se você quer usar esses serviços ou não; de acordo com o que dissemos ele **viola diretamente o Acordo de Assinatura do Steam** e pode resultar em suspensão caso a Valve descubra. Conforme salientado acima, ** recomendamos NÃO usar nenhum dos tais serviços**.

* * *

## Problemas

* * *

### O ASF não detecta o jogo `X` como disponível para coleta, embora eu saiba que ele inclui cartas colecionáveis Steam!

Existem duas razões principais aqui. A primeira e mais óbvia é o fato de que você está se referindo a **loja Steam**, onde o jogo é anunciado como contendo Cartas Colecionáveis. Esta é a suposição **errada**, uma vez que ela simplesmente assume que o jogo **tem** cartas inclusas, mas não quer dizer que essa função para esse jogo esteja **habilitada** imediatamente. Você pode ler mais sobre isso no **[anúncio oficial](https://steamcommunity.com/games/593110/announcements/detail/1954971077935370845)**.

Em suma, o ícone de Cartas Colecionáveis na loja Steam não significa nada, verifique sua **[página de insígnias](https://steamcommunity.com/my/badges)** para confirmar se um jogo tem cartas ou não; é isso que o ASF faz. Se o seu jogo não aparece na lista como um jogo com possibilidade de conseguir cartas, então **não** é possível coletar desse jogo, independentemente do motivo.

A segunda questão é menos óbvia, e é a situação onde o seu jogo aparece na página de insígnia como disponível para recebimento de cartas e, no entanto, não está sendo executado pelo ASF. A menos que você esteja se deparando com algum bug, tal como o ASF ser incapaz de verificar as páginas de insígnias (descritas abaixo), é simplesmente um efeito de cache e do lado do ASF a Steam ainda está mostrando a página de insígnias desatualizada. Esse problema deve resolver-se mais cedo ou mais tarde, quando o cache for invalidado. Também não dá para consertar isso do nosso lado.

Claro, tudo isso pressupõe que você está executando o ASF com as configurações padrão intocadas, uma vez que você também pode ter adicionado este jogo à lista de bloqueio, usado `false` em `IdleRefundableGames` entre outras coisas.

* * *

### Qual é a diferença entre um aviso e um erro no registro (log)?

O ASF grava em seu registro um monte de informações de vários níveis. Nosso objetivo é explicar **precisamente** o que o ASF está fazendo, incluindo quais problemas no Steam que ele tem de lidar, ou outros problemas a superar. Na maioria das vezes nem tudo é relevante, é por isso que temos dois níveis principais usados no ASF em termos de problemas - um nível de aviso e um nível de erro.

A regra geral do ASF é que avisos **não** são erros, portanto eles **não** devem ser relatados. Um aviso é um indicador de que algo potencialmente indesejado aconteceu. Se é falta de resposta do Steam, a API retornando erros ou queda na sua conexão de rede; é um aviso, e significa que esperávamos que acontecesse, então não se incomode os desenvolvedores com isso. Claro, você é livre para perguntar sobre eles ou obter ajuda usando o nosso suporte, mas você não deve assumir que são erros do ASF e que vale a pena serem relatados (a menos que nós confirmemos o contrário).

Erros, por outro lado, indicam uma situação que não deveria acontecer, porém eles devem ser relatados se você se certificou de que não é você quem os está causando. Se é uma situação comum e que esperamos que aconteça, então ele será convertido em um aviso. Caso contrário, possivelmente é um erro que deve ser corrigido e não ignorado em silêncio, supondo que não seja resultado de problema técnico seu. Por exemplo, remover o o arquivo `ASF.json` acarretará um erro, já que é um arquivo crucial e o ASF não pode operar sem ele, mas foi você quem o retirou, então você não deve reportar esse erro para nós (a menos que você tenha confirmado que o ASF está errado e o arquivo está lá).

Em suma - reporte erros, não reporte avisos. Porém você ainda perguntar sobre os avisos e receber ajuda em nossas seções de suporte.

* * *

### O ASF não inicia, a janela do programa fecha imediatamente após ser lançado!

If even `log.txt` is not being generated then you most likely forgot to install .NET Core prerequisites, as stated in **[setting up](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up#os-specific-setup)** guide. Outros problemas comuns podem incluir tentar rodar a variante errada do ASF para seu SO, ou de alguma outra forma a falta de dependências nativas do .NET Core. Se a janela de console facha rápido demais para você conseguir ler a mensagem, então abra um console independente e rode o executável do ASF por lá. Por exemplo, no Windows, abra a pasta do ASF, segure `Shift`, clique com o botão direito dentro da pasta e escolha "Abrir janela de comando (ou powershell) aqui", digite no console `.\ArchiSteamFarm.exe` e tecle enter. Desta forma você obterá a mensagem correta do motivo do ASF é não iniciar corretamente.

* * *

### O ASF está encerrando minha sessão no cliente Steam enquanto eu jogo! / `Você já iniciou a sessão em outro computador`

Isto aparece como uma mensagem sobreposta na Steam indicando que a conta está sendo usada em outro lugar enquanto você está jogando. Esse problema pode ter dois motivos diferentes.

Um dos motivos é causado por pacotes "quebrados" (jogos) que não sabem como manter o bloqueio do jogo corretamente, mas esperam que o bloqueio seja controlado pelo cliente. Um exemplo de tal pacote seria o Skyrim SE. Seu cliente Steam executa o jogo normalmente, mas o jogo não se registra como sendo executado. Por conta disso o ASF assume que está livre para resumir o processo, e fazendo isso ele te desliga da rede Steam, uma vez que o Steam detecta de repente que a conta está sendo usada em outro lugar.

Outro motivo pode ser causado se você estiver jogando em seu PC enquanto o ASF espera (especialmente em outra máquina) e você perde a conexão com a internet. Nesse caso, a rede Steam te marca como offline e libera o bloqueio de jogo (como no caso acima), o que faz com que o ASF (por exemplo, em outra máquina) retome a coleta. Quando seu PC se reconecta o Steam não consegue acionar o bloqueio de jogo novamente (que agora está com o ASF, também semelhante ao caso acima) e mostra a mesma mensagem.

Ambas as causas são muito difíceis de serem contornadas pelo lado do ASF, já que ele simplesmente retoma a coleta uma vez que a rede Steam informe que a conta está livre para ser usada. É isso que acontece normalmente quando você fecha o jogo, mas com pacotes "quebrados" isso pode acontecer imediatamente, mesmo se o seu jogo ainda estiver sendo executado. O ASF não tem como saber se sua conexão caiu, se você parou de jogar, ou se ainda está jogando um jogo que não segurou o bloqueio apropriadamente.

A única solução para este problema é pausar manualmente seu bot com o comando `pause` antes de começar a jogar e o reiniciar com o comando `resume` assim que estiver pronto. Ou você pode ignorar o problema e agir como se tivesse jogado com o cliente Steam off-line.

* * *

### `Desconectado do Steam!` - não consigo conectar aos servidores Steam.

O ASF pode apenas **tentar** se conectar aos servidores Steam e pode falhar por vários motivos, incluindo a falta de conexão com a internet, Steam offline, seu firewall bloqueando a conexão, ferramentas de terceiros, rotas configuradas incorretamente ou falhas temporárias. Você pode habilitar o modo `Debug` para verificar um registro mais detalhado que indica a falha exata, embora ela geralmente seja causada por suas próprias ações, por exemplo, usando "CS:GO MM Server Picker" que bloqueia vários IPs do Steam, tornando muito difícil que você realmente alcance a rede Steam.

O ASF fará o seu melhor para se conectar, que inclui não só pedir uma lista atualizada de servidores, mas também tentar outro IP quando o último falhar, então se realmente for um problema temporário com um servidor ou rota específica, o ASF vai se conectar mais cedo ou mais tarde. No entanto, se você está atrás de um firewall ou impossibilitado de alguma forma de alcançar os servidores Steam, então obviamente você precisa consertar o problema por sua conta, com a potencial ajuda do modo modo de depuração - `Debug`.

Também é possível que sua máquina não seja capaz de estabelecer conexão com servidores Steam usando o protocolo padrão do ASF. Você pode alterar protocolos que ASF tem permissão de usar modificando o parâmetro `SteamProtocols` na configuração global. Por exemplo, se você está tendo problemas em alcançar os servidores Steam com o protocolo `TCP`, você pode tentar o `UDP` ou `WebSocket`.

No caso improvável de você ter armazenado em cache os endereços incorretos dos servidores, por exemplo, por ter movido a pasta `config` do ASF de uma maquina para outra que esteja localizada em outro país, deletar o arquivo `ASF.db` a fim de atualizar a lista de servidores na próxima execução do programa pode ajudar. Na maioria das vezes isso não é necessário e não deve ser feito, já que essa lista é atualizada automaticamente na primeira inicialização, bem como quando a conexão é estabelecida.

* * *

### `Não foi possível obter informações das insígnias, tentaremos novamente mais tarde!`

Usually it means that you're using Steam parental PIN to access your account, yet you forgot to put it in ASF config. You must put valid PIN in `SteamParentalCode` bot config property, otherwise ASF will not be able to access most of web content, therefore will not be able to work properly. Head over to **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** in order to learn more about `SteamParentalCode`.

Outras razões podem incluir um problema temporário do Steam, um problema de rede ou coisas assim. Se o problema não se resolver depois de várias horas e você tem certeza de que configurou o ASF corretamente, sinta-se livre para nos informar.

* * *

### O ASF está falhando com o erro: `Falha na solicitação mesmo após 5 tentativas`!

Usually it means that you're using Steam parental PIN to access your account, yet you forgot to put it in ASF config. You must put valid PIN in `SteamParentalCode` bot config property, otherwise ASF will not be able to access most of web content, therefore will not be able to work properly. Head over to **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** in order to learn more about `SteamParentalCode`.

Se PIN do modo família não é a razão, então este é um erro mais comum e você deve se acostumar com isso; significa simplesmente que o ASF enviou uma solicitação para a rede Steam e não recebeu uma resposta válida, mesmo após 4 tentativas. Normalmente, isso significa que a Steam caiu, está tendo algumas dificuldades ou está em manutenção; o ASF está ciente de tais problemas e você não deve se preocupar com eles, a menos que aconteçam constantemente e por mais que algumas horas, e que outros usuários não estejam tendo esses problemas.

Como verificar se o Steam caiu? **[Steam Status](https://steamstat.us)** é uma excelente fonte para verificar se o Steam **deveria estar online**, se você notar erros, especialmente relacionados com a Comunidade ou o API Web, então o Steam está tendo problemas, então você pode deixar o ASF fazer seu trabalho sozinho depois de um tempo, ou esperar.

No entanto esse nem sempre é o caso, pois em alguns casos os problemas com o Steam podem não ser detectados pelo Steam Status, um exemplo disso foi quando a Valve quebrou o suporte HTTPS para a Comunidade Steam em 7 de junho de 2016 - acessar**[Comunidade Steam](https://steamcommunity.com)** com o https retornava um erro. Portanto, não confie cegamente no Steam Status também, é melhor você mesmo verificar se tudo funciona como deveria.

Por último, se nada ajudar, você sempre pode habilitar o modo `Debug` e ver no registro do ASF porque exatamente as solicitações estão falhando. Por exemplo, o problema de HTTPS mencionado acima causava:

    <HTML><HEAD><TITLE>Error</TITLE></HEAD><BODY>
    An error occurred while processing your request.<p>
    

O que é claramente um problema do Steam e nada a ser corrigido no ASF. Você sempre pode tentar visitar o link mencionado pelo ASF e verificar se ele funciona; se não funcionar, então você sabe por que o ASF também não pode acessá-lo. Se funcionar e o erro não desaparecer depois de um dia ou dois, valeria a pena investigar e reportar.

Antes de fazer isso você deve **certificar-se de que vale a pena relatar o erro**. Se ele estiver mencionado nesse FAQ, uma questão relacionada a troca por exemplo, então não. Se for problema temporário que aconteceu uma ou duas vezes, principalmente quando sua rede estava instável ou o Steam offline - então, não. No entanto, se você teve esse problema várias vezes seguidas no espaço de 2 dias, reiniciou tanto ASF quanto sua máquina e certificou-se que não há nenhuma resposta pra ele no FAQ, então vale a pena pedir suporte.

* * *

### O ASF parece travar e não mostra nada no console até que eu pressione uma tecla!

Você provavelmente está usando o Windows e seu console está com modo de edição rápida ativado. Consulte **[esse artigo](https://stackoverflow.com/questions/30418886/how-and-why-does-quickedit-mode-in-command-prompt-freeze-applications)** no StackOverflow para obter uma explicação técnica. Você deve desabilitar o modo de edição rápida clicando com o botão direito na sua janela de console do ASF, indo até propriedades e desmarcando a caixa de seleção apropriada.

* * *

### O ASF não pode aceita ou envia trocas!

O óbvio primeiro: novas contas são limitadas. Até que você desbloqueie a conta colocando pelo menos $5 na sua carteira Steam ou gastando esse valor na loja, o ASF não pode aceitar nem enviar trocas usando essa conta. Neste caso, o ASF indicará que esse inventário parece vazio, porque todas as cartas nele não são trocáveis. Também não será possível receber qualquer troca, já que para isso o ASF precisa ser capaz de obter uma chave de API e esse recurso é desabilitado em contas limitadas. Resumindo, trocas estão fora de questão para contas limitadas, sem exceções.

Next, if you do not use **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)**, it's possible that ASF in fact accepted/sent trade, but you need to confirm it via your e-mail. Do mesmo jeito, se você usa o 2FA comum, você precisa confirmar a troca pelo autenticador. As confirmações são **obrigatórias**, então se você não quer aceitá-las manualmente, considere adicionar ou importar seu autenticador para o ASF 2FA.

Observe também que você só pode trocar com seus amigos e pessoas que tenham seu link de troca. Se você está tentando fazer troca de um bot para a conta master, tal como `loot`, então você precisa ter seu bot na sua lista de amigos, ou seu `SteamTradeToken` declarado na configuração do bot. Certifique-se de que o token é válido, caso contrário, você não será capaz de enviar uma troca.

Por último, lembre-se que novos dispositivos tem um bloqueio de trocas de 7 dias, então se você acabou de adicionar sua conta ao ASF, espere pelo menos esses 7 dias - tudo deve funcionar corretamente após esse período. Essa limitação inclui **tanto** aceitar **quanto** enviar trocas. Nem sempre ele funciona e há pessoas que conseguem enviar e aceitar trocas instantaneamente. No entanto a maioria das pessoas são afetadas e o bloqueio **vai** acontecer, mesmo que você consiga enviar e aceitar trocas através de seu cliente Steam na mesma máquina. Tenha paciencia, não há nada que você possa fazer para acelerar isso.

E finalmente, tenha em mente que uma conta pode ter apenas 5 trocas pendentes para outra, então o ASF vai falhar ao enviar trocas se você tem 5 (ou mais) pendentes naquele bot para serem aceitas. Raramente isso é um problema, mas vale mencionar especialmente se você configurou o ASF para enviar trocas automaticamente sem usar o ASF 2FA e esqueceu de confirmá-las.

Se nada aqui te ajudou, você sempre pode habilitar o modo `Debug` e ver porque as solicitações estão falhando. Note que o Steam gera muitas bobagens na maioria das vezes e o motivo pode não fazer nenhum sentido lógico ou pode estar totalmente incorreto - se você decidir interpretar esse motivo você deve ter um conhecimento profundo sobre o Steam e suas falhas. Também é muito comum ver esse problema com nenhuma razão lógica, e a única solução sugerida neste caso é adicionar novamente a conta no ASF (e esperar 7 dias novamente). Às vezes esse problema se corrige *magicamente*, da mesma forma que ele quebra. No entanto, geralmente é só i bloqueio de 7 dias, um problema temporário do Steam ou ambos. É melhor esperar uns dias antes de verificar manualmente o que está errado, a menos que você tem um desejo grande de depurar a causa real (e geralmente você será forçado a esperar mesmo assim, porque a mensagem de erro não vai fazer nenhum sentido, nem te ajudar em nada).

In any case, ASF can only **try** to send a proper request to Steam in order to accept/send trade. Whether Steam accepts that request, or not, is out of the scope of ASF, and ASF will not magically make it work. There's no bug related to that feature, and there is also nothing to improve, because logic is happening outside of ASF. Therefore, do not ask for fixing stuff that is not broken, and also do not ask why ASF can't accept or send trades - **I don't know, and ASF doesn't know either**. Either deal with it, or fix yourself.

* * *

### Why do I have to put 2FA/SteamGuard code on each login? / `Removed expired login key`

ASF uses login keys (if you kept `UseLoginKeys` enabled) for keeping credentials valid, the same mechanism that Steam uses - 2FA/SteamGuard token is required only once. However, due to Steam network issues and quirks, it's entirely possible that login key is not saved in the network, I've already seen such issues not only with ASF, but with regular steam client as well (a need to input login + password on each run, regardless of "remember me" option).

You could remove `BotName.db` (+ `BotName.bin`, if exists) of affected account and try to link ASF to your account once again, but that doesn't have to succeed. The real ASF-based solution is to import your authenticator as **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** - this way ASF can generate tokens automatically when they're needed, and you don't have to input them manually. Usually the issue magically solves itself after some time, so you can simply wait for that to happen. Of course you can also ask GabeN for solution, because I can't force Steam network to accept our login keys.

As a side note, you can also turn off login keys with `UseLoginKeys` config property set to `false`, but you should do that only if ASF has fully automated way to make initial login. Right now this is possible only with valid `SteamPassword` and **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)**, since in this case we don't need to rely on login keys at all, as we have required login credentials (password and 2FA key) available on each login.

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

**Ensure that you downloaded ASF from trusted source**. The only official and trusted source is **[ASF releases](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** page on GitHub (and this is also the source for ASF auto-updates) - **any other source is untrusted by definition and might contain malware added by other people** - you should not trust any other download location by definition, and ensure that your ASF always comes from us.

If you confirmed that ASF is downloaded from trusted source, then very likely it's simply a false positive. This **happened in the past**, **is happening right now**, and **will happen in the future**. We're already used to that and you shouldn't notify us about new false positives - notify developers of your AV instead, since it's not ASF issue in the first place.

If you're worried about actual safety when using ASF, then I suggest scanning ASF with many different AVs for actual detection ratio, for example through **[VirusTotal](https://www.virustotal.com)** (or any other web service of your choice like this).

If the AV that you're using falsely detects ASF as a malware, then **it's a good idea to send this file sample back to developers of your AV, so they can analyze it and improve their detection engine**, as clearly it's not working as good as you think it does. There is no issue in ASF code, and there is also nothing to fix for us, since we're not distributing malware in the first place, therefore it doesn't make any sense to report those false-positives to us. We highly recommend to send ASF sample for further analysis like stated above, but if you don't want to bother with it, then you can always add ASF to some kind of AV exceptions, disable your AV or simply use another one. Sadly, we're used to AVs being stupid, as every once in a while some AV detects ASF as a virus, which usually lasts very short and is being patched up quickly by the devs, but like we pointed out above - **it happened**, **happens** and **will happen** all the time. ASF doesn't include any malicious code, you can review ASF code and even compile from source yourself. We're not hackers to obfuscate ASF code in order to hide from AV heuristics and false positives, so do not expect from us to fix what is not broken - there is no "virus" for us to fix.