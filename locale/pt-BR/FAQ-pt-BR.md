# Perguntas frequentes (FAQ)

A seção de perguntas frequentes cobre respostas a questões comuns que você pode ter. Para questões menos comuns, por favor visite a seção **[Perguntas Frequentes Adicionais](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Extended-FAQ)**.

# Tabela de conteúdos

- [Geral](#geral)
- [Comparação com ferramentas similares](#comparação-com-ferramentas-similares)
- [Segurança / Privacidade / VAC / Banimentos / Termos de serviço](#segurança--privacidade--vac--banimentos--termos-de-serviço)
- [Diversos](#diversos)
- [Problemas](#problemas)

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

O ASF se conecta a sua conta Steam através deum mini cliente Steam embutido usando as credenciais fornecidas. Após se conectar com sucesso, ele analisa sua página de **[insígnias](https://steamcommunity.com/my/badges)** a fim de encontrar jogos que estão disponíveis para coleta (Jogo pode dar mais X cartas). Após analisar todas as páginas e fazer a lista final de jogos que estão aptos, o ASF escolhe o algoritmo de coleta mais eficiente e inicia o processo. O processo depende do **[algorítimo de coleta de cartas](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)** escolhido, mas geralmente consiste em jogar um jogo elegível e, periodicamente (e a cada item recebido), verificar se o jogo já está totalmente coletado - se sim, o ASF pode prosseguir para o próximo título, usando o mesmo procedimento, até que todos os jogos sejam totalmente explorados.

Tenha em mente que a explicação acima é simplificada e não descreve as dezenas de recursos e funções extras que o ASF oferece. Visite o resto da **[nossa wiki](https://github.com/JustArchiNET/ArchiSteamFarm/wiki)** se você quiser conhecer cada detalhe do ASF. Eu tentei fazê-la simples o bastante para que todos a entendam, sem entrar em detalhes técnicos - usuários avançados são encorajados a cavar mais fundo.

Agora como programa - o ASF oferece um pouco de magia. Em primeiro lugar, ele não precisa baixar nenhum arquivo de jogo, ele pode jogar os jogos imediatamente. Em segundo lugar, ele é inteiramente independente do seu cliente Steam normal - você não precisa ter o cliente Steam rodando, nem mesmo instalado. Em terceiro lugar, é uma solução automatizada - o que significa que o ASF faz tudo sozinho para você, sem a necessidade de dizer a ele o que fazer - o que te poupa aborrecimento e tempo. Por último, ele não tem que enganar a rede Steam emulando o processo (que, por exemplo, é o método usado pelo Idle Master), já que ele pode se comunicar diretamente com ele. Ele também é super rápido e leve, sendo uma solução surpreendente para aqueles que querem obter cartas facilmente e sem muita trabalheira - é especialmente útil deixá-lo rodando em segundo plano enquanto está fazendo outra coisa, ou mesmo jogando no modo offline.

Tudo isso é bom, mas o ASF também tem algumas limitações técnicas que são aplicadas pela Steam - nós não podemos rodar jogos que você não possui, nós não podemos rodar jogos para sempre a fim de obter cartas extras após o limite imposto e nós não podemos rodar jogos enquanto você está jogando. Tudo isso deve ser "óbvio", considerando a maneira como o ASF funciona, mas é bom notar que o ASF não tem super poderes e não vai fazer algo que é fisicamente impossível, então tenha isso em mente - é basicamente a mesma coisa que se você dissesse a alguém para se conectar na sua conta de outro PC e jogar esses jogos para você.

Então resumindo - o ASF é um programa que ajuda a pegar as cartas que você é apto a pegar, mas sem muita trabalheira. Ele também oferece várias outras funções, mas vamos nos ater a esta por enquanto.

* * *

### Tenho que colocar minhas credenciais de conta?

**Sim**. O ASF exige suas credenciais de conta da mesma forma que o cliente oficial da Steam, já que ele está usando o mesmo método para interagir com a rede Steam. No entanto, isso não significa que você tenha que colocar suas credenciais de conta nos arquivos de configuração do ASF, você pode usar o ASF com `SteamLogin` e/ou `SteamPassword` `null`/vazio, e colocar esses dados cada vez que abrir o ASF, quando for preciso (assim como várias outras credenciais de login, veja a seção configuração). Desta forma, seus dados não são salvos em lugar nenhum, mas é claro, assim o ASF não poderá auto-reiniciar sem a sua ajuda. O ASF também oferece várias outras formas de aumentar a sua **[segurança](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security)**, então sinta-se a a vontade para ler essa parte da wiki se você for um usuário avançado. Se você não é, e não quer colocar suas credenciais de conta nas configurações do ASF, então simplesmente não faça isso, e coloque-as quando o ASF as pedir.

Tenha em mente que o ASF é uma ferramenta para seu uso pessoal e as suas credenciais nunca deixarão seu computador. Você também não estará compartilhando elas com ninguém, o que cumpre os termos de serviço da Steam - uma coisa muito importante da qual as pessoas esquecem. Você não vai mandar seus dados para nossos servidores ou o servidor de algum terceiro, somente diretamente para os servidores da Steam operados pela Valve.

* * *

### Quanto tempo tenho que esperar para ganhar as cartas?

**O quanto for preciso** - sério. Cada jogo tem uma dificuldade de coleta diferente, definida pelo(a) desenvolvedor(a) e/ou editora, e cabe totalmente a eles decidir o quão rápido as cartas serão fornecidas. A maioria dos jogos segue o padrão de 1 carta a cada 30 minutos de jogo, mas também há jogos que exigem que você jogar várias horas antes de dar uma carta. Além disso, sua conta pode ser impedida de receber cartas de jogos que você não jogou por determinado tempo ainda, como consta na seção **[desempenho](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)**. Não tente supor quanto tempo o ASF deve coletar de determinado título - não cabe a você, nem ao ASF decidir. Não há nada que você possa fazer para torná-lo mais rápido, e não há "bugs" relacionados a cartas não serem recebidas em tempo hábil - você não controla o processo de recebimento de cartas, nem o ASF. Na melhor das hipóteses, você receberá a média de 1 carta a cada 30 minutos. Na pior, você não receberá qualquer carta em menos de 4 horas desde que iniciou o ASF. Ambas as situações são normais e estão explicadas na seção **[desempenho](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)**.

* * *

### A coleta está demorando muito, posso adiantá-la de alguma forma?

A única coisa que afeta fortemente a velocidade de coleta é o **[algorítimo de coleta de cartas](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)** escolhido para sua conta bot. Tudo o resto tem efeitos insignificantes e não vai deixar o processo de coletas mais rápido, enquanto algumas ações, tal como iniciar o processo do ASF várias vezes pode até **piorar**. Se você realmente deseja fazer uso de cada segundo do processo de coleta, então o ASF permite que você configure algumas variáveis principais, como `FarmingDelay`; todas elas são explicadas em **[configuração](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration-pt-BR)**. No entanto, como eu disse, o efeito é insignificante, e escolher o algorítimo de coleta de cartas adequado para determinada conta é a única escolha crucial que pode afetar fortemente a velocidade da coleta, tudo o resto é pura cosmética. Em vez de se preocupar com a velocidade de coleta, apenas abra o ASF e deixe ele fazer seu trabalho - posso assegurar-lhe que ele está fazendo isso da forma mais eficaz que eu poderia conseguir. Quanto menos você se importar, mais você ficará satisfeito.

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

**Sim**, de diversas formas. Se você quiser mudar a lista padrão de coleta é só usar a **[parâmetro de configuração do bot](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-pt-BR#configura%C3%A7%C3%A3o-do-bot)** `FarmingOrders`. Se você quiser bloquear manualmente alguns jogos, você pode usar a blacklist disponível com o **[comando](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** `ib`. Se você quiser coletar tudo, mas dar preferência a alguns jogos antes, você pode usar a lista de prioridade disponível com o **[comando](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** `iq`. E ainda, se você quer coletar apenas de determinados jogos, então você pode usar a **[parâmetro de configuração do bot](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-pt-BR#configura%C3%A7%C3%A3o-do-bot)** `IdlePriorityQueueOnly`, juntamente com a adição dos seus jogos na lista de prioridade de coleta.

Além de gerenciar o módulo de coleta automática que foi descrito acima, você também pode mudar o ASF para o modo manual com o **[comando](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-pt-BR)** `play`, ou usar uma mistura de configurações externas como o **[parâmetro de configuração do bot](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-pt-BR#configura%C3%A7%C3%A3o-do-bot)** `GamesPlayedWhileIdle`.

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

- **O ASF pode te ajudar a completar seus conjuntos** (função `TradingPreferences` de `SteamTradeMatcher`). Com um pouco de configuração avançada, você pode transformar seu ASF em um bot que automaticamente aceita ofertas **[STM](https://www.steamtradematcher.com)**, te ajudando diariamente a completar seus conjuntos sem nenhuma interação de usuário. O ASF ainda inclui seu próprio módulo ASF 2FA que te permite importar seu autenticador móvel Steam e tornar o processo totalmente automático. Ou, talvez você queira aceitar manualmente e deixar que o ASF apenas prepare essas trocas para você? Mais uma vez, essa escolha é inteiramente sua.

- **O ASF pode resgatar keys em segundo plano para você** (função **[ativador de keys em segundo plano](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer-pt-BR)**). Talvez você tenha centenas de keys de vários bundles e está com preguiça de ativar todas, passando por um monte de janelas e concordando com termos e condições do Steam todas as vezes. Por que não copiar e colar sua lista de keys no ASF e deixr ele fazer o seu trabalho? O ASF vai ativar automaticamente todas essas keys em segundo plano, fornecendo depois umas lista indicando qual o resultado de cada tentativa de ativação. Além disso, se você tem centenas de keys, é certo que você atingirá o limite de ativações permitido pelo Steam uma hora ou outra e o ASF sabe disso, então ele vai esperar pacientemente o fim do bloqueio e retomar de onde parou.

Nós poderíamos continuar com toda a **[wiki do ASF](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Home-pt-BR)**, apontando cada função do programa, porém temos que por um limite em algum ligar. É isso, esta é uma lista de recursos que você pode desfrutar como usuário do ASF, onde só um desses poderia facilmente ser considerado a principal razão para nunca olhar para trás, e na verdade listamos **vários** deles, há muito mais que você pode aprender na nossa wiki. E nós ainda nem entramos em detalhes de coisas como a API do ASF, que te permite codificar seus próprios comandos ou gestões gloriosas de bots, pois quisemos manter isso simples.

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

**Sim**, embora o ASF saiba quando é útil usar esse recurso, baseado no **[algorítimo de coleta de cartas](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)** selecionado. Você não tem escolha direta sobre o algorítimo, mas você pode sugerir um ao ASF configurando a propriedade corretamente. Você deve se focar na configuração do ASF e deixar os algorítimos decidirem qual a forma melhor otimizada para atingir o objetivo.

* * *

### O ASF pode alternar rapidamente entre jogos?

**Não**, o ASF não suporta e não incentiva o uso de **[falhas do Steam](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance#steam-glitches)**.

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

No entanto, tenha em mente que a Valve ainda pode adicionar o ASF a sua lista de bloqueio um dia, mas seria completamente absurdo, pois você ainda poderia jogar jogos protegidos pelo VAC em seu PC enquanto roda o ASF em um servidor por exemplo. Portanto tenho plena certeza de que eles sabem que o ASF não é suspeito e eles não vão dificultar nossas vidas bloqueando o ASF sem motivo. Então, no pior dos casos você seria impossibilitado de jogar, como descrito acima, pois a garantia de o ASF ser livre do VAC ainda permanece se a Steam bloquear o binário do ASF ou não (e você ainda poderia executar o ASF em outro computador onde o cliente Steam não estiver instalado). No momento não há motivo para fazer isso e esperamos que continue assim.

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

Este caso é um pouco mais fácil de analisar por causa de detalhes extras fornecidos pelo usuário. Aparentemente o usuário estava usando **uma versão muito desatualizada do ASF** que incluía um bug que fazia com que o ASF enviasse um número excessivo de solicitações para os servidores Steam. O bug em si não existia no início, mas foi ativado devido a uma mudança no Steam que causou falhas e que foi corrigida na próxima versão. **O ASF oferece suporte apenas para a **[versão estável mais recente](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** lançada no GitHub**. O software é escrito por humanos e os seres humanos podem cometer erros. Se o erro tem um escopo global ele é rapidamente consertado e lançado a todos os usuários em uma versão revisada. A Valve não vai banir meio milhão de usuários do ASF repentinamente devido a um erro meu, por razões óbvias. No entanto, se você intencionalmente não quer usar a versão atualizada do ASF, então você faz parte de uma minoria muito pequena de usuários que estão **expostos a incidentes como esse** devido a **falta de suporte**, já que ninguém está se preocupando com a sua versão desatualizada, ninguém vai consertá-la nem garantir que você não seja banido apenas por executá-la. **Por favor, utilize o software atualizado**, não só o ASF, mas todos os outros aplicativos também.

* * *

Todos os incidentes acima têm uma coisa em comum - o ASF é apenas uma ferramenta e é **sua** a decisão de como utilizá-la. Você não será banido por usar o ASF, mas por **como** você o usa. Ele pode ser uma ferramenta auxiliar na coleta de apenas uma conta ou de uma rede massiva de coleta com milhares de bots. Em qualquer dos casos, eu não estou oferecendo aconselhamento legal e você deve decidir por sua conta como você usará o ASF. Não estou escondendo nenhuma informação que poderia te ajudar, o fato do ASF ter causado o banimento de algumas pessoas por exemplo, uma vez que eu não tenho motivo para isso; é escolha sua o que você quer fazer com essa informação. Se você me perguntar - use o bom senso, evite criar uma rede gigantesca de bots de coleta, não envie centenas de trocas ao mesmo tempo, sempre utilize o ASF atualizado e você *deverá* ficar tranquilo. Todo incidente dessa natureza aconteceu por **algum motivo** com pessoas que rodavam centenas de contas. Se isso é apenas coincidência ou algum fator real, cabe a você decidir. Não estou oferecendo qualquer aconselhamento legal, estou apenas compartilhando meus pensamentos, você pode achá-los úteis ou ignorá-los totalmente e considerar apenas os fatos acima relacionados.

* * *

### Quais informações de privacidade o ASF divulga?

Você pode encontrar uma explicação detalhada do que exatamente essa opção faz na seção de **[estatísticas](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Statistics)**. Você deve revê-la se você se preocupa com sua privacidade, por exemplo, se você está se perguntando por que contas usadas no ASF estão entrando em nosso grupo Steam. O ASF não coleta quaisquer informações confidenciais e não as compartilha com quaisquer terceiros.

* * *

## Diversos

* * *

### Estou usando um sistema operacional não suportado, como Windows 32-bit por exemplo, ainda posso usar o ASF V3?

Sim e essa versão não tem falta de suporte, ela só não é oficialmente compilada. Procure na seção de **[compatibilidade](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-pt-BR)** pela variante genérica.

* * *

### O ASF é maravilhoso! Posso fazer uma doação?

Sim, e estamos muito felizes em saber que você está gostando do nosso projeto! Você pode encontrar várias maneiras de doar a cada **[lançamento](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** e também **[na página principal](https://github.com/JustArchiNET/ArchiSteamFarm)**. Vale notar que além de doações em dinheiro nós também aceitamos itens do Steam, então nada te impede de doar skins, chaves ou uma pequena parte das cartas que você coletou com o ASF caso você queira. Obrigado desde já por sua generosidade!

* * *

### Eu uso o PIN do modo família para proteger minha conta, eu preciso colocá-lo em algum lugar?

Sim, você deve colocá-lo no parâmetro `SteamParentalCode` na configuração do bot. Isso porque o ASF acessa muitas partes da sua conta Steam que são protegidas e é impossível que o ASF opere sem isso.

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

Sim, pelo chat Steam ou pelo **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)**. Visite a seção **[comandos](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-pt-BR)** para mais informações.

* * *

### Eu gostaria de ajudar com a tradução do ASF, o que eu preciso fazer?

Obrigado pelo seu interesse! Você pode encontrar todos os detalhes na seção **[localização (idioma)](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Localization)**.

* * *

### Tenho apenas uma conta (principal) adicionada ao ASF, eu posso emitir comandos através do chat Steam?

**Sim**, e isso é explicado na seção **[](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-pt-BR#notas)comandos**. Você pode fazer isso através do chat em grupo do Steam.

* * *

### O ASF parece estar funcionando, mas eu não estou conseguindo cartas!

A taxa de coleta de cartas difere de um jogo para outro, como você pode ler em **[desempenho](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)**. Demora um pouco, geralmente **várias horas por jogo**, e você não deve esperar que as cartas apareçam em seu inventário apenas alguns minutos após inciar o programa. Se você ver que o ASF checa ativamente o estado das cartas e troca de jogo após o atual ser totalmente explorado, então tudo está correto; você provavelmente deve estar se referindo as notificações do seu inventário que são dispensadas automaticamente pelo ASF através de `DismissInventoryNotifications` do parâmetro de configuração do bot `BotBehaviour`. Visite a seção **[configuração](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-pt-BR)** para mais informações.

* * *

### Como parar completamente o processo ASF para minha conta?

Simplesmente feche o processo ASF, por exemplo, clicando em [X] no Window. Se ao invés disso você quer parar um bot em particular enquanto mantém os outros rodando, então dê uma olhada no **[parâmetro de configuração do bot](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-pt-BR#configura%C3%A7%C3%A3o-do-bot)** `Enabled`, ou no **[comando](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-pt-BR)** `stop`. Se você quer parar o processo de coleta automática, mas manter o ASF rodando sua conta, então é isso que o **[parâmetro de configuração do bot](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-pt-BR#configura%C3%A7%C3%A3o-do-bot)** `Paused` e o **[comando](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-pt-BR)** `pause` fazem.

* * *

### Quantos bots posso rodar com o ASF?

O ASF em si não tem qualquer limite máximo de contas bot, no entanto você será limitado pela rede Steam. Atualmente você pode rodar cerca de 100 a 110 bots por IP e por instância do ASF. É possível executar mais bots com mais IPs e mais instâncias do ASF. Tenha em mente que se você estiver usando uma grande quantidade de bots, você deve controlar o número deles por sua conta (por exemplo, certificando-se de que todos eles são de fato se conectando e trabalhando ao mesmo tempo). Também note que o limite acima em geral depende de muitos fatores internos - ele é uma aproximação e não um limite estrito - você provavelmente será capaz de executar mais/menos bots do que o especificado acima.

* * *

### Então eu posso rodar mais instâncias do ASF?

Você pode executar quantas instâncias do ASF você quiser em um computador, assumindo que cada instância tenha sua própria pasta e suas próprias configurações, e que uma conta usada em uma instância não seja usada em outra. No entanto, pergunte-se por que você quer fazer isso. O ASF é otimizado para lidar com uma dúzia, ou até mesmo uma centena de contas ao mesmo tempo, e iniciar essas dúzias de bots em suas próprias instâncias do ASF afeta o desempenho, toma mais recursos do SO e cria uma falta de sincronização entre os bots; assim, por exemplo você é mais susceptível de atingir os limites `InvalidPassword/RateLimitExceeded` descritos abaixo, uma vez que os os registo de pedidos não estão sendo sincronizados entre instâncias ASF. Portanto, eu **sugiro fortemente** sempre executar o máximo de uma instância ASF por IP/interface. Se você tiver mais IPs/interfaces, você pode livremente executar mais instâncias do ASF, cada instância usando seu próprio IP/interface. Caso contrário, executar mais instâncias do ASF é totalmente inútil, e não faz nada além de diminuir a performance e tomar mais recursos do SO (como memória), e também causa falta de sincronização e aumenta a chance de causar problemas. Você não ganha nada em executar mais que 1 instância por IP/interface.

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

### Why playtime of games idled through ASF doesn't increase?

It does, but **not in real-time**. Steam records your playtime in fixed intervals and schedules update for it, but you're not guaranteed to have it updated immediately the moment you quit the session, let alone during such. If it was possible to skip playtime while idling cards then you can be sure that we'd have it implemented in ASF long time ago, and actually use it in default settings. But we don't, and we don't exactly because it's not possible - just because the playtime isn't updated in real-time doesn't mean that it's not recorded.

* * *

### What is the difference between a warning and an error in the log?

ASF writes to its log a bunch of information on various logging levels. Our objective is to explain **precisely** what ASF is doing, including what Steam issues it has to deal with, or other problems to overcome. Most of the time not everything is relevant, this is why we have two major levels being used in ASF in terms of problems - a warning level, and error level.

General ASF rule is that warnings are **not** errors, therefore they should **not** be reported. A warning is an indicator to you that something potentially unwanted happen. Whether it was Steam not reacting, API throwing errors or your network connection being down - it's a warning, and it means we expected it to happen, so don't bother ASF development with it. Of course you're free to ask about them or get help by using our support, but you shouldn't assume that those are ASF errors worth reporting (unless we confirm otherwise).

Errors on the other hand indicate a situation that should not happen, therefore they're worth reporting as long as you made sure that it's not you who is causing them. If it's a common situation that we expect to happen, then it'll be converted to a warning instead. Otherwise, it's possibly a bug that should be corrected, not silently ignored, assuming it's not a result of your own technical issue. For example, removing core `ASF.json` file will throw an error, as ASF can't operate without that file, but it was you who removed it, so you should not report that error to us (unless you confirmed that ASF is wrong and the file is there).

In one TL;DR sentence - report errors, don't report warnings. You can still ask about warnings and receive help in our support sections.

* * *

### ASF can't start, the program window immediately closes after being launched!

If even `log.txt` is not being generated then you most likely forgot to install .NET Core prerequisites, as stated in **[setting up](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up#os-specific-setup)** guide. Other common problems might include trying to launch wrong ASF variant for your OS, or in other way missing native .NET Core runtime dependencies. If the console window closes too soon for you to read the message, then open independent console and launch ASF binary from there. For example on Windows, open ASF directory, hold `Shift`, right click inside the folder and choose "open command window here" (or powershell), then type into the console `.\ArchiSteamFarm.exe` and hit enter. This way you'll get precise message why ASF is not starting properly.

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

Usually it means that you're using Steam parental PIN to access your account, yet you forgot to put it in ASF config. You must put valid PIN in `SteamParentalCode` bot config property, otherwise ASF will not be able to access most of web content, therefore will not be able to work properly. Head over to **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** in order to learn more about `SteamParentalCode`.

Other reasons might include temporary Steam problem, network issue or likewise. If issue won't solve itself after several hours and you're sure that you configured ASF appropriately, feel free to let us know about that.

* * *

### ASF is failing with `Request failed despite of 5 tries` errors!

Usually it means that you're using Steam parental PIN to access your account, yet you forgot to put it in ASF config. You must put valid PIN in `SteamParentalCode` bot config property, otherwise ASF will not be able to access most of web content, therefore will not be able to work properly. Head over to **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** in order to learn more about `SteamParentalCode`.

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

Next, if you do not use **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)**, it's possible that ASF in fact accepted/sent trade, but you need to confirm it via your e-mail. Likewise, if you use classic 2FA, you need to confirm the trade via your authenticator. Confirmations are **mandatory** now, so if you don't want to accept them by yourself, consider either adding or importing your authenticator into ASF 2FA.

Also notice that you can trade only with your friends, and people with known trade link. If you're trying to initiate Bot->Master trade, such as `loot`, then you need to either have your bot on your friendlist, or your `SteamTradeToken` declared in Bot's config. Make sure that the token is valid - otherwise, you won't be able to send a trade.

Lastly, remember that new devices have 7-days trade lock, so if you've just added your account to ASF, wait at least 7 days - everything should work after that period. That limitation includes **both** accepting **and** sending trades. It does not always trigger, and there are people who can send and accept trades instantly. Majority of the people are affected though, and the lock **will** happen, even if you can send and accept trades through your steam client on the same machine. Just wait patiently, there's nothing you can do to make it faster.

And finally, keep in mind that one account can have only 5 pending trades to another one, so ASF will fail to send trades if you have 5 (or more) pending ones from that one bot to accept already. This is rarely a problem, but it's also worth mentioning, especially if you set ASF to auto-send trades, yet you're not using ASF 2FA and forgot to actually confirm them.

If nothing helped, you can always enable `Debug` mode and check yourself why requests are failing. Please note that Steam talks nonsense most of the time, and provided reason might not make any logical sense, or can be even entirely incorrect - if you decide to interpret that reason, make sure you have decent knowledge about Steam and its quirks. It's also quite common to see that issue with no logical reason, and the only suggested solution in this case is to re-add account to ASF (and wait 7 days again). Sometimes this issue also fixes itself *magically*, the same way it breaks. However, usually it's just either 7-days trade lock, temporary steam problem, or both. It's best to give it a few days before manually checking what is wrong, unless you have some urge to debug the real cause (and usually you'll be forced to wait anyway, because error message won't make any sense, neither help you in the slightest).

In any case, ASF can only **try** to send a proper request to Steam in order to accept/send trade. Whether Steam accepts that request, or not, is out of the scope of ASF, and ASF will not magically make it work. There's no bug related to that feature, and there is also nothing to improve, because logic is happening outside of ASF. Therefore, do not ask for fixing stuff that is not broken, and also do not ask why ASF can't accept or send trades - **I don't know, and ASF doesn't know either**. Either deal with it, or fix yourself.

* * *

### Why do I have to put 2FA/SteamGuard code on each login? / `Removed expired login key`

ASF uses login keys (if you kept `UseLoginKeys` enabled) for keeping credentials valid, the same mechanism that Steam uses - 2FA/SteamGuard token is required only once. However, due to Steam network issues and quirks, it's entirely possible that login key is not saved in the network, I've already seen such issues not only with ASF, but with regular steam client as well (a need to input login + password on each run, regardless of "remember me" option).

You could remove `BotName.db` (+ `BotName.bin`, if exists) of affected account and try to link ASF to your account once again, but that doesn't have to succeed. The real ASF-based solution is to import your authenticator as **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** - this way ASF can generate tokens automatically when they're needed, and you don't have to input them manually. Usually the issue magically solves itself after some time, so you can simply wait for that to happen. Of course you can also ask GabeN for solution, because I can't force Steam network to accept our login keys.

As a side note, you can also turn off login keys with `UseLoginKeys` config property set to `false`, but you should do that only if ASF has fully automated way to make initial login. Right now this is possible only with valid `SteamPassword` and **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)**, since in this case we don't need to rely on login keys at all, as we have required login credentials (password and 2FA key) available on each login.

* * *

### I'm getting error: `Unable to login to Steam: InvalidPassword or RateLimitExceeded`

This error can mean a lot of things, some of them include:

- Combinação de Usuário/Senha inválidos (obviamente)
- A chave de sessão usada pelo ASF expirou
- Muitas tentativas de conexão em um curto período de tempo (prevenção de força bruta)
- Muitas tentativas falhas de conexão em um curto período de tempo (limite de tentativas)
- Exigência de captcha para se conectar (muito provavelmente causado por um dos dois motivos acima)
- Qualquer outro motivo pelo qual a rede Steam pode estar te impedindo de se conectar.

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

### ASF is being detected as a malware by my AntiVirus! O que está acontecendo?

**Ensure that you downloaded ASF from trusted source**. The only official and trusted source is **[ASF releases](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** page on GitHub (and this is also the source for ASF auto-updates) - **any other source is untrusted by definition and might contain malware added by other people** - you should not trust any other download location by definition, and ensure that your ASF always comes from us.

If you confirmed that ASF is downloaded from trusted source, then very likely it's simply a false positive. This **happened in the past**, **is happening right now**, and **will happen in the future**. If you're worried about actual safety when using ASF, then I suggest scanning ASF with many different AVs for actual detection ratio, for example through **[VirusTotal](https://www.virustotal.com)** (or any other web service of your choice like this).

If the AV that you're using falsely detects ASF as a malware, then **it's a good idea to send this file sample back to developers of your AV, so they can analyze it and improve their detection engine**, as clearly it's not working as good as you think it does. There is no issue in ASF code, and there is also nothing to fix for us, since we're not distributing malware in the first place, therefore it doesn't make any sense to report those false-positives to us. We highly recommend to send ASF sample for further analysis like stated above, but if you don't want to bother with it, then you can always add ASF to some kind of AV exceptions, disable your AV or simply use another one. Sadly, we're used to AVs being stupid, as every once in a while some AV detects ASF as a virus, which usually lasts very short and is being patched up quickly by the devs, but like we pointed out above - **it happened**, **happens** and **will happen** all the time. ASF doesn't include any malicious code, you can review ASF code and even compile from source yourself. We're not hackers to obfuscate ASF code in order to hide from AV heuristics and false positives, so do not expect from us to fix what is not broken - there is no "virus" for us to fix.