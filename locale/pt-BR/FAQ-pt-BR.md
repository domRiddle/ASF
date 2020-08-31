# Perguntas frequentes (FAQ)

A seção de perguntas frequentes cobre respostas a questões comuns que você pode ter. Para questões menos comuns, por favor visite a seção **[Perguntas frequentes adicionais](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Extended-FAQ)**.

# Tabela de conteúdos

- [Geral](#geral)
- [Comparação com ferramentas similares](#comparação-com-ferramentas-similares)
- [Segurança / Privacidade / VAC / Banimentos / Termos de serviço](#segurança--privacidade--vac--banimentos--termos-de-serviço)
- [Diversos](#diversos)
- [Problemas](#problemas)

* * *

## Geral

### O que é o ASF?

### Por que o programa afirma que não há nada para coletar na minha conta?

### Por que minha conta está limitada?

Antes de tentar entender o que é o ASF, certifique-se de que você entende o que são as cartas Colecionáveis Steam e como obtê-las, o que é muito bem descrito na seção oficial de perguntas frequentes **[aqui](https://steamcommunity.com/tradingcards/faq)**.

Em resumo, as Cartas Colecionáveis são itens que você obtém ao jogar certos jogos, e podem ser usadas para criar insígnias, vender no mercado da Steam ou qualquer outro objetivo de sua escolha.

Os pontos principais são apresentados aqui novamente, porque geralmente as pessoas não querem concordar com eles e gostam de fingir que eles não existem:

- **Você precisa ter o jogo na sua biblioteca Steam para ser elegível a ganhar cartas dele. Jogos compartilhados não contam.**
- **Você não pode coletar de um jogo infinitamente, cada jogo tem um número fixo de cartas. Uma vez que você consiga todas elas (cerca de metade do jogo de cartas completo), o jogo não dará mais cartas. It doesn't matter whether you've sold, traded, crafted or forgot what happened to those cards you've obtained, once you run out of card drops, the game is finished.**
- **Não é possível coletar cartas de jogos F2P (gratuitos para jogar) sem gastar nenhum dinheiro neles. Isso inclui jogos permanentemente gratuitos como Team Fortress 2 ou Dota 2. Possuir jogos F2P não te garante cartas.**
- **Você não pode receber cartas em [contas limitadas](https://support.steampowered.com/kb_article.php?ref=3330-iagk-7663&l=portuguese), independentemente dos jogos que possua. Isso foi possível no passado, mas não é mais o caso.**
- **Jogos pagos que você obteve de graça durante alguma promoção não podem ser coletados, independente do que esteja escrito na página da loja. Isso foi possível no passado, mas não é mais o caso.**

Então, como você pode ver, Cartas Colecionáveis Steam são concedidas a você por jogar um jogo que você comprou, ou um jogo gratuito no qual você colocou dinheiro. Se você jogar tais jogos por tempo suficiente, todas as cartas para aquele jogo vão eventualmente aparecer no seu inventário, tornando possível que você complete uma insígnia (após obter a metade restante do jogo de cartas), venda-as, ou faça o que mais você quiser.

Agora que nós explicamos o básico do Steam, nós podemos explicar o ASF. O programa em si é bastante complexo para se entender totalmente, então ao invés de explicar todos os detalhes técnicos, vamos oferecer uma explicação mais simples abaixo.

O ASF se conecta à sua conta Steam através de nosso Cliente Steam personalizado embutido no código usando as credenciais que você forneceu. Após se conectar com sucesso, ele analisa sua página de **[insígnias](https://steamcommunity.com/my/badges)** a fim de encontrar jogos que estão disponíveis para coleta (Jogo pode dar mais X cartas). Após analisar todas as páginas e fazer a lista final de jogos que estão aptos, o ASF escolhe o algoritmo de coleta mais eficiente e inicia o processo. O processo depende do **[algorítimo de coleta de cartas](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance-pt-BR)** escolhido, mas geralmente consiste em jogar um jogo elegível e, periodicamente (e a cada item recebido), verificar se o jogo já está totalmente coletado - se sim, o ASF pode prosseguir para o próximo título, usando o mesmo procedimento, até que todos os jogos sejam totalmente explorados.

Tenha em mente que a explicação acima é simplificada e não descreve as dezenas de recursos e funções extras que o ASF oferece. Visite o resto da **[nossa wiki](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Home-pt-BR)** se você quiser conhecer cada detalhe do ASF. Eu tentei fazê-la simples o bastante para que todos a entendam, sem entrar em detalhes técnicos - usuários avançados são encorajados a cavar mais fundo.

Agora como programa - o ASF oferece um pouco de magia. Em primeiro lugar, ele não precisa baixar nenhum arquivo de jogo, ele pode jogar os jogos imediatamente. Em segundo lugar, ele é inteiramente independente do seu cliente Steam normal - você não precisa ter o cliente Steam rodando, nem mesmo instalado. Em terceiro lugar, é uma solução automatizada - o que significa que o ASF faz tudo sozinho para você, sem a necessidade de dizer a ele o que fazer - o que te poupa aborrecimento e tempo. Por último, ele não tem que enganar a rede Steam emulando o processo (que, por exemplo, é o método usado pelo Idle Master), já que ele pode se comunicar diretamente com ele. Ele também é super rápido e leve, sendo uma solução surpreendente para aqueles que querem obter cartas facilmente e sem muita trabalheira - é especialmente útil deixá-lo rodando em segundo plano enquanto está fazendo outra coisa, ou mesmo jogando no modo offline.

Tudo isso é bom, mas o ASF também tem algumas limitações técnicas que são aplicadas pela Steam - nós não podemos rodar jogos que você não possui, nós não podemos rodar jogos para sempre a fim de obter cartas extras após o limite imposto e nós não podemos rodar jogos enquanto você está jogando. Tudo isso deve ser "óbvio", considerando a maneira como o ASF funciona, mas é bom notar que o ASF não tem super poderes e não vai fazer algo que é fisicamente impossível, então tenha isso em mente - é basicamente a mesma coisa que se você dissesse a alguém para se conectar na sua conta de outro PC e jogar esses jogos para você.

Então resumindo - o ASF é um programa que ajuda a pegar as cartas que você é apto a pegar, mas sem muita trabalheira. Ele também oferece várias outras funções, mas vamos nos ater a esta por enquanto.

* * *

### Tenho que colocar minhas credenciais de conta?

**Sim**. O ASF exige suas credenciais de conta da mesma forma que o cliente oficial da Steam, já que ele está usando o mesmo método para interagir com a rede Steam. No entanto, isso não significa que você tenha que colocar suas credenciais de conta nos arquivos de configuração do ASF, você pode usar o ASF com `SteamLogin` e/ou `SteamPassword` `null`/vazio, e colocar esses dados cada vez que abrir o ASF, quando for preciso (assim como várias outras credenciais de login, veja a seção configuração). Desta forma, seus dados não são salvos em lugar nenhum, mas é claro, assim o ASF não poderá auto-reiniciar sem a sua ajuda. O ASF também oferece várias outras formas de aumentar a sua **[segurança](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security)**, então sinta-se a a vontade para ler essa parte da wiki se você for um usuário avançado. Se você não for, e não quiser colocar suas credenciais nas configurações do ASF, então simplesmente não o faça, apenas insira-as quando o ASF as pedir.

Tenha em mente que o ASF é uma ferramenta para seu uso pessoal e as suas credenciais nunca deixarão seu computador. Você também não estará compartilhando elas com ninguém, o que cumpre os termos de serviço da Steam - uma coisa muito importante da qual as pessoas esquecem. Você não vai mandar seus dados para nossos servidores ou o servidor de algum terceiro, somente diretamente para os servidores da Steam operados pela Valve. Nós não sabemos suas credenciais e não somos capazes de recuperá-las para você, independentemente de você as ter colocado nas configurações ou não.

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

### Não estou interessado em receber cartas. Gostaria apenas de aumentar minhas horas de jogo, isso é possível?

Sim, o ASF permite que você faça isso de várias maneiras.

A forma ideal para isso é configurar **[`GamesPlayedWhileIdle`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-pt-BR#gamesplayedwhileidle)** que vai rodar seus appIDs escolhidos quando o ASF não tiver cartas para coletar. Caso você queira rodar determinados jogos o tempo todo mesmo tendo cartas para receber de outros jogos, então você pode combiná-lo com **[`IdlePriorityQueueOnly`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-pt-BR#idlepriorityqueueonly)**, assim o ASF vai rodar apenas os jogos que você definiu para receber cartas, ou **[`Paused`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-pt-BR#paused)**, que fará com que o módulo de coleta de cartas seja pausado até que você o reinicie.

Como alternativa, você pode usar o comando **[`play`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-pt-BR#comandos-1)**, que vai fazer o ASF jogar seus jogos selecionados. No entanto, tenha em mente que `play` deve ser usado apenas para jogos que você deseja rodar temporariamente, pois como ele não é um estado persistente o ASF reverterá para o estado padrão após desconexão da rede Steam, por exemplo. Portanto, recomendamos que você use o `GamesPlayedWhileIdle`, a menos que você realmente deseje iniciar seus jogos selecionados apenas por um curto período de tempo e depois reverter para o fluxo padrão.

* * *

### Eu sou usuário do Linux / OS X, o ASF roda jogos que não estão disponíveis para a minha plataforma? O vai ASF roda jogos 64-bit se eu rodo um SO de 32-bit?

Sim, o ASF não vai baixar nenhum arquivo de jogo, então ele funciona com todas as licenças que estejam ligadas a sua conta Steam, independente de qualquer requisito técnico ou de plataforma. Ele também deve funcionar com jogos que tenham bloqueio regional, mesmo quando você não estiver na região correta, embora não garantimos isso (funcionou na última vez que testamos).

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

Essas são 3 razões **muito importantes** pelas quais você deve considerar usar o ASF, já que elas afetam diretamente todos que coletam cartas Steam e não há como alguém dizer "isso não me preocupa", pois as manutenções do Steam e suas falhas acontecem com todos. Há outras dezenas de motivos mais ou menos importantes que você verá no resto desse FAQ. Então resumindo, **sim**, você deve usar o ASF mesmo se não precisar de nenhuma função extra do ASF que não exista no IM.

Ainda, o IM está oficialmente descontinuado e pode parar de funcionar completamente no futuro, sem ninguém se preocupando em consertá-lo, considerando a existência de soluções muito mais poderosas (não apenas o ASF). O IM já não funciona mais para um monte de gente, e esse número só está aumentando, não diminuindo. Você sempre deve evitar usar softwares obsoletos, não apenas o IM, mas qualquer outro programa descontinuado. Não ter manutenção ativa significa que ninguém se importa se ele funciona ou não, ninguém verifica isso e **ninguém se responsabiliza pela sua funcionalidade**, o que é uma questão crucial em termos de segurança. Pode vir a ocorrer algum bug crítico que cause problemas na infraestrutura do Steam - sem que haja alguém para consertá-lo o Steam pode emitir outra onda de banimento na qual você será atingido sem sequer estar ciente do problema, como já aconteceu no passado com pessoas usando... uma versão obsoleta do ASF.

* * *

### Que funcionalidades interessantes o ASF oferece que o Idle Master não?

Depende do que é "interessante" para você. Se você planeja rodar automaticamente mais que uma conta então a resposta é obvia já que o ASF permite que você faça isso da melhor forma, poupando recursos, aborrecimento e problemas de compatibilidade. No entanto, se você está perguntando isso é por que provavelmente você não tem essa necessidade, então vamos avalir outros benefícios que se aplicam a uma única conta usada no ASF.

Primeiro e mais importante, você tem algumas funcionalidades embutidas mencionadas **[acima](#vale-a-pena-usar-o-ASF-se-eu-estiver-usando-o-idle-master-e-ele-funciona-bem-parar-mim)** que são cruciais para o processo de coleta automático independente do seu objetivo final, e muitas vezes isso já é o suficiente para considerar usar o ASF. Mas você já sabe disso, então vamos avançar para algumas características mais interessantes:

- **Você pode coletar off-line** (função `Offline` de `OnlineStatus`). Coletar offline permite que você evite o estado "Em jogo" do Steam, rodando jogos automaticamente com o ASF enquanto mostra "Online" no Steam, assim seus amigos não saberão que o ASF está rodando um jogo para você. Essa é uma característica muito boa, pois ela permite que você fique online no seu cliente Steam sem atormentar seus amigos com constantes mudanças de jogos, ou sem confundi-los a acharem que você está jogando um jogo quando de fato não está. Se você respeita seus amigos esse ponto sozinho já faz valer a pena usar o ASF, mas é apenas o começo. Também é bom notar que esse recurso não tem nada a ver com as configurações de privacidade do Steam - se você iniciar um jogo o estado "Em jogo" vai aparecer normalmente para seus amigos, ele torna apenas a parte do ASF invisível e não afeta em nada a sua conta.

- **Você pode pular jogos reembolsáveis** (função `IdleRefundableGames`). O ASF tem uma lógica embutida para tratar jogos reembolsáveis e você pode configurá-lo para não rodar automaticamente esses jogos. Isso permite que você avalie se um jogo recém comprado na loja Steam valeu o seu dinheiro, sem que o ASF tente coletar cartas dele cedo demais. Se você jogar um jogo por mais de 2 horas ou se passarem 2 semanas desde sua compra, então o ASF vai rodar ele, pois ele já não será mais reembolsável. Até lá você já tem tempo suficiente para saber se gostou dele ou não e você pode facilmente pedir reembolso se for preciso, sem ter que bloqueá-lo manualmente ou não usar o ASF durante esse período.

- **Itens podem ser marcados automaticamente como recebidos** (função `DismissInventoryNotifications` de `BotBehaviour`). Rodar um jogo automaticamente com o ASF fará que sua conta receba cartas. Você já sabe que isso vai acontecer, então você pode deixar que o ASF limpe essas notificações inúteis, certificando-se que apenas notificações importantes chamem sua atenção. É claro, isso apenas se você quiser.

- **Você pode receber cartas dos eventos Steam automaticamente** (função `AutoSteamSaleEvent`). O ASF proporciona automação da lista de descobrimento durante as promoções do Steam, claro, se você quiser que ele faça isso. Isso poupa muito tempo durante os dias em que a promoção estiver ativa, e garante que você nunca perca as cartas diárias.

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

**Sim**, embora o ASF saiba quando é útil usar esse recurso, baseado no **[algorítimo de coleta de cartas](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)** selecionado. A taxa de coleta de cartas quando se roda diversos jogos ao mesmo tempo tende a zero, portanto o ASF só roda vários jogos ao mesmo tempo para aumentar o número de horas nesses jogos e alcançar o valor de `HoursUntilCardDrops` mais rapidamente, fazendo isso para até `32` jogos ao mesmo tempo. É por esse motivo também que você deve se concentrar na confituração do ASF, e deixar os algorítimos decidirem qual a melhor forma de se alcançar o objetivo, o que você pensa ser melhor pode, na realidade, não ser. Rodar vários jogos ao mesmo tempo não vai te trazer nenhuma carta.

* * *

### O ASF pode alternar rapidamente entre jogos?

**Não**, o ASF não suporta e não incentiva o uso de **[falhas do Steam](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance#steam-glitches)**.

* * *

### O ASF pode rodar cada jogo por X horas antes de cartas serem adicionadas?

**Não**, a mudança no sistema de cartas do Steam foi feito para lutar contra falsas estatísticas e jogadores fantasmas. O ASF não contrubui pra isso mais que o necessário, adicionar tal recurso não está planejado e não vai acontecer. Se o seu jogo libera cartas da maneira padrão, o ASF vai executá-lo o quanto antes possível.

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

De acordo com usuários e pelo que sei, esse é o melhor caso agora, já que ninguém reportou qualquer problemas como os descritos no link acima enquanto usava o ASF. Também não pudemos reproduzir tal comportamento com o ASF, enquanto pudemos com o Idle Master.

No entanto, tenha em mente que a Valve ainda pode adicionar o ASF a sua lista de bloqueio um dia, mas seria completamente absurdo, pois você ainda poderia jogar jogos protegidos pelo VAC em seu PC enquanto roda o ASF em um servidor por exemplo. Portanto tenho plena certeza de que eles sabem que o ASF não é suspeito e eles não vão dificultar nossas vidas bloqueando o ASF sem motivo. Então, no pior dos casos você seria impossibilitado de jogar, como descrito acima, pois a garantia de o ASF ser livre do VAC ainda permanece se a Steam bloquear o binário do ASF ou não (e você ainda poderia executar o ASF em outro computador onde o cliente Steam não estiver instalado). No momento não há motivo para fazer isso e esperamos que continue assim.

* * *

### Ele é seguro?

Se você pergunta se o ASF é seguro como um software, que ele não vai causar qualquer dano ao seu computador, não vai roubar seus dados privados, instalar vírus ou qualquer outra coisa nesse sentido - a resposta é sim. ASF é livre de malware, roubo de dados, mineradores de criptomoeda e quaisquer outros comportamentos duvidosos que possam ser considerados maliciosos ou indesejados pelo usuário. Além disso, temos uma seção dedicada às **[estatísticas](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Statistics-pt-BR)** que cobre nossa política de privacidade e comportamento do ASF que vai além da configuração que você fez no programa.

Nosso código é aberto e os executáveis distribuídos são sempre compilados por **[ ferramentas de integração contínua automáticas e confiáveis](https://pt.wikipedia.org/wiki/Automa%C3%A7%C3%A3o_de_compila%C3%A7%C3%A3o)** de **[ fontes disponíveis publicamente](https://pt.wikipedia.org/wiki/Software_de_c%C3%B3digo_aberto)** e não pelos próprios desenvolvedores. Cada compilação pode ser reproduzida seguindo nosso script de compilação e terá resultado exatamente igual, um executável de código IL (binário) **[determinístico](https://en.wikipedia.org/wiki/Deterministic_system)**. Se por algum motivo você não acredita em nossas compilações você pode compilar e usar o ASF pela fonte, incluindo todas as bibliotecas que o ASF usa (como o SteamKit2), que também são de código aberto.

Porém, no fim, é sempre uma questão de confiança no(s) desenvolvedor(es) do programa, então é você quem deve decidir se considera o ASF seguro ou não, provavelmente apoiando sua decisão com argumentos técnicos especificados acima. Não acredite cegamente em algo só porque eu disse - verifique por sua conta, já que essa é a única maneira de ter certeza.

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

> Em relação a todas as Assinaturas, Conteúdo e Serviços que não sejam de autoria da Valve, a Valve não examina esses conteúdos de terceiros disponíveis no Steam ou por meio de outras fontes. A Valve não assume qualquer responsabilidade ou passivo por esses conteúdos de terceiros. É possível que determinados softwares aplicativos de terceiros sejam usados pelas empresas para propósitos empresariais; contudo, você poderá adquirir esse Software apenas por meio do Steam para uso privado.

No entanto, a Valve reconhece claramente a existência de "Steam Idlers" (programas que rodam jogos automaticamente) conforme afirmado **[aqui](https://support.steampowered.com/kb_article.php?ref=2117-ilzv-2837)**, então se você me perguntasse eu diria que tenho certeza de que se a Valve não os quer, eles já teriam feito algo mais concreto que apenas apontar que isso pode causar problemas com o VAC. A palavra chave aqui é **Steam** idlers, por exemplo o ASF, e não **game** idlers.

Note que o que foi citado acima é apenas a nossa interpretação do Acordo de Assinatura do Steam e vários pontos; o ASF é licenciado pela Licença Apache 2.0, que afirma claramente:

> A menos que exigido por lei ou acordado por escrito, o ASF é distribuído "COMO ESTÁ", SEM GARANTIAS OU CONDIÇÕES DE QUALQUER TIPO, expressas ou implícitas.

Você está usando este software por sua conta e risco. É muito improvável que você seja banido por isso, mas se isso acontecer, você pode culpar apenas a si mesmo.

* * *

### Alguém já foi banido por isso?

**Sim**, tivemos pelo menos alguns incidentes até agora que resultaram em algum tipo de suspensão do Steam. Se o ASF em si foi a causa principal ou não é outra história que nós provavelmente nunca saberemos.

Um dos casos foi de um cara com mais de 1000 bots que teve um bloqueio de trocas (juntamente com todos os bots), provavelmente devido ao uso excessivo do `loot ASF` executado em todos os bots de uma vez, ou por muitas trocas suspeitas onde apenas um lado tinha vantagem em um curtíssimo período de tempo.

> Olá, XXX, Obrigado por contatar o suporte Steam. Parece que esta conta foi usada para gerenciar uma rede de contas bot. O uso de bots viola o Acordo de Assinatura do Steam.

Por favor, use o bom senso e não assuma que você pode fazer essas coisas malucas só porque o ASF permite que você faça isso. Usar o `loot ASF` em mais de mil de bots pode ser facilmente considerado um ataque **[DDoS](https://pt.wikipedia.org/wiki/Ataque_de_nega%C3%A7%C3%A3o_de_servi%C3%A7o#Ataque_distribu%C3%ADdo)** e eu pessoalmente não estou chocado que alguém foi banido para uma coisa dessas. Use sempre o bom senso e um mínimo de uso justo em relação ao serviço do Steam, e *provavelmente* você estará seguro.

Outro caso foi de um cara com mais de 170 bots ser banido durante a Promoção de Inverno 2017 do Steam.

> Sua conta foi bloqueada por violação do Acordo de Assinatura do Steam. Julgando pelas trocas e outros fatores, essa conta foi usada para coletar ilegalmente cartas colecionáveis do Steam, assim como atividades comerciais relacionadas, entre outras. A conta foi bloqueada permanentemente e o suporte Steam não pode fornecer suporte adicional sobre esta questão.

Este é mais um caso muito difícil de analisar, por causa resposta vaga do suporte Steam que mal oferece os detalhes. Baseado em meus pensamentos pessoais, este usuário provavelmente trocou cartas Steam para algum tipo de dinheiro (algum bot para subir de nível?) ou de alguma outra maneira tentou retirar fundos do Steam. Caso você não saiba isso também é ilegal de acordo com o Acordo de Assinatura do Steam.

O último caso envolveu um usuário com mais de 120 bots ser banido por quebrar o **[Código de Conduta Online do Steam](https://store.steampowered.com/online_conduct?l=brasilian)**.

> Olá, XXX, Obrigado por contatar o suporte Steam. Essa e outras contas foram usadas para inundar nossa infraestrutura de rede, o que é uma violação da conduta online do Steam. A conta foi bloqueada permanentemente e o suporte Steam não pode fornecer suporte adicional sobre esta questão.

Este caso é um pouco mais fácil de analisar por causa de detalhes extras fornecidos pelo usuário. Aparentemente o usuário estava usando **uma versão muito desatualizada do ASF** que incluía um bug que fazia com que o ASF enviasse um número excessivo de solicitações para os servidores Steam. O bug em si não existia no início, mas foi ativado devido a uma mudança no Steam que causou falhas e que foi corrigida na próxima versão. **O ASF oferece suporte apenas para a **[versão estável mais recente](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** lançada no GitHub**. O software é escrito por humanos e os seres humanos podem cometer erros. Se o erro tem um escopo global ele é rapidamente consertado e lançado a todos os usuários em uma versão revisada. A Valve não vai banir meio milhão de usuários do ASF repentinamente devido a um erro meu, por razões óbvias. No entanto, se você intencionalmente não quer usar a versão atualizada do ASF, então você faz parte de uma minoria muito pequena de usuários que estão **expostos a incidentes como esse** devido a **falta de suporte**, já que ninguém está se preocupando com a sua versão desatualizada, ninguém vai consertá-la nem garantir que você não seja banido apenas por executá-la. **Por favor, utilize o software atualizado**, não só o ASF, mas todos os outros aplicativos também.

* * *

Todos os incidentes acima têm uma coisa em comum - o ASF é apenas uma ferramenta e é **sua** a decisão de como utilizá-la. Você não será banido por usar o ASF, mas por **como** você o usa. Ele pode ser uma ferramenta auxiliar na coleta de apenas uma conta ou de uma rede massiva de coleta com milhares de bots. Em qualquer dos casos, eu não estou oferecendo aconselhamento legal e você deve decidir por sua conta como você usará o ASF. Não estou escondendo nenhuma informação que poderia te ajudar, o fato do ASF ter causado o banimento de algumas pessoas por exemplo, uma vez que eu não tenho motivo para isso; é escolha sua o que você quer fazer com essa informação. Se quiser minha opinião: use o bom senso, evite ter mais bot que a quantidade que recomendamos, não envie centenas de trocas ao mesmo tempo, sempre utilize o ASF atualizado e tudo *deverá* dar certo. Todo incidente dessa natureza, por **alguma razão**, sempre aconteceu com pessoas que ignoraram nossa recomendação e decidiram que sabem melhor que nós quantos bots podem ser executados. Se isso é apenas coincidência ou algum fator real, cabe a você decidir. Não estou oferecendo qualquer aconselhamento legal, estou apenas compartilhando meus pensamentos, você pode achá-los úteis ou ignorá-los totalmente e considerar apenas os fatos acima relacionados.

* * *

### Quais informações de privacidade o ASF divulga?

Você pode encontrar uma explicação detalhada do que exatamente essa opção faz na seção de **[estatísticas](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Statistics)**. Você deve revê-la se você se preocupa com sua privacidade, por exemplo, se você está se perguntando por que contas usadas no ASF estão entrando em nosso grupo Steam. O ASF não coleta quaisquer informações confidenciais e não as compartilha com quaisquer terceiros.

* * *

## Diversos

* * *

### Estou usando um sistema operacional não suportado, como Windows 32-bit por exemplo, ainda posso usar a última versão do ASF?

Sim e essa versão não tem falta de suporte, ela só não é oficialmente compilada. Procure na seção de **[compatibilidade](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-pt-BR)** pela variante genérica. O ASF não tem nenhuma grande dependência em relação ao SO, e pode rodar em qualquer lugar onde você consiga rodar o tempo de execução .NET Core, o que inclui o Windows 32-bit, mesmo que não tenhamos um pacote específico `win-x86`.

* * *

### O ASF é maravilhoso! Posso fazer uma doação?

Sim, e estamos muito felizes em saber que você está gostando do nosso projeto! Você pode encontrar várias maneiras de doar a cada **[lançamento](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** e também **[na página principal](https://github.com/JustArchiNET/ArchiSteamFarm)**. Vale notar que além de doações em dinheiro nós também aceitamos itens do Steam, então nada te impede de doar skins, chaves ou uma pequena parte das cartas que você coletou com o ASF caso você queira. Obrigado desde já por sua generosidade!

* * *

### Eu uso o PIN do modo família para proteger minha conta, eu preciso colocá-lo em algum lugar?

Sim, você deve colocá-lo no parâmetro `SteamParentalCode` na configuração do bot. Isso porque o ASF acessa muitas partes da sua conta Steam que são protegidas e é impossível que o ASF opere sem isso.

* * *

### Eu não quero que o ASF colete de nenhum jogo por padrão, mas ainda quero usar os recursos extras dele. É possível?

Sim, se você quiser iniciar o ASF com o módulo de coleta pausado, você pode setar a propriedade de configuração do bot `Paused` para `true`. Isso permitirá que você `retome` a coleta durante o tempo de execução.

Se você deseja desabilitar completamente o módulo de coleta e garantir que ele nunca seja executado sem que você diga explicitamente o contrário, então recomendamos definir `IdlePriorityQueueOnly` como `verdadeiro`, que em vez de apenas pausá-lo, desativará a coleta completamente até que você adicione os jogos na fila de prioridade.

Com o módulo de coleta pausado/desabilitado, você pode utilizar as demais funcionalidades do ASF normalmente, tais como `GamesPlayedWhileIdle`.

* * *

### Posso minimizar o ASF para a bandeja?

O ASF é um aplicativo de console, não há janela para ser minimizada pois as janelas são criadas para você pelo seu SO. No entanto você pode usar qualquer ferramenta de terceiros capaz de fazer isso, tal como **[RBTray](http://rbtray.sourceforge.net)** para Windows ou **[screen](https://linux.die.net/man/1/screen)** para Linux/OS X. Essas são apenas exemplos, há muitas outras ferramentas com o mesmo recurso.

* * *

### Usar o ASF mantém a elegibilidade para receber pacotes de cartas?

**Sim**. O ASF usa o mesmo método para se conectar a rede Steam que o cliente oficial, então ele também preserva a habilidade de receber pacotes para as contas que estão sendo usadas. No entanto, não há confirmação de que há a necessidade de se conectar a rede Steam para isso, então para ter certeza eu sugiro manter `OnlineStatus` em `OnlineStatus`, a menos até que alguém confirme que apenas usar a rede Steam já é o suficiente. Deveria ser, mas não tenho certeza.

* * *

### Existe alguma maneira de se comunicar com o ASF?

Sim, de diversas formas. Visite a seção **[comandos](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-pt-BR)** para mais informações.

* * *

### Eu gostaria de ajudar com a tradução do ASF, o que eu preciso fazer?

Obrigado pelo seu interesse! Você pode encontrar todos os detalhes na seção **[localização (idioma)](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Localization)**.

* * *

### Tenho apenas uma conta (principal) adicionada ao ASF, eu posso emitir comandos através do chat Steam?

**Sim**, e isso é explicado na seção **[](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-pt-BR#notas)comandos**. Você pode enviá-los através do chat em grupo do Steam, embora usar o **[ASF-ui](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-pt-BR#asf-ui)** possa ser a maneira mais fácil.

* * *

### O ASF parece estar funcionando, mas eu não estou conseguindo cartas!

A taxa de coleta de cartas difere de um jogo para outro, como você pode ler em **[desempenho](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)**. Demora um pouco, geralmente **várias horas por jogo**, e você não deve esperar que as cartas apareçam em seu inventário apenas alguns minutos após inciar o programa. Se você notar que o ASF analisa o estado das cartas ativamente e troca de jogo quando o atual estiver totalmente coletado, então tudo está funcionando bem. É possível que você tenha habilitado alguma opção tal como `DismissInventoryNotifications` em `BotBehaviour` que dispensa automaticamente as nitificações de inventário. Visite a seção **[configuração](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-pt-BR)** para mais informações.

* * *

### Como parar completamente o processo ASF para minha conta?

Simplesmente feche o processo ASF, por exemplo, clicando em [X] no Window. Se ao invés disso você quer parar um bot em particular enquanto mantém os outros rodando, então dê uma olhada no **[parâmetro de configuração do bot](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-pt-BR#configura%C3%A7%C3%A3o-do-bot)** `Enabled`, ou no **[comando](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-pt-BR)** `stop`. Se você quer parar o processo de coleta automática, mas manter o ASF rodando sua conta, então é isso que o **[parâmetro de configuração do bot](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-pt-BR#configura%C3%A7%C3%A3o-do-bot)** `Paused` e o **[comando](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-pt-BR)** `pause` fazem.

* * *

### Quantos bots posso rodar com o ASF?

Como programa, o ASF não tem nenhum limite máximo de contas bot, então você pode rodar o quanto quiser, enquanto você tiver memória disponível em seu computador, no entanto, você ainda estará limitado pela rede e pelos outros serviços do Steam. Atualmente você pode rodar de 100 a 200 bots por IP e por instância do ASF. É possível rodar mais bots com mais IPs e mais instâncias do ASF, contornando as limitações de IP. Tenha em mente que se você estiver usando uma grande quantidade de bots, você deve controlar a quantidade deles por sua conta, por exemplo, certificando-se de que todos eles são de fato se conectando e trabalhando ao mesmo tempo. O ASF não foi desenvolvido para uma quantidade enorme de bots, e a regra que se aplica é que **quanto mais bots você tiver, mais problemas você encontrará**. Também note que o limite acima em geral depende de muitos fatores internos, ele é uma aproximação e não um limite exato, você provavelmente será capaz de executar mais/menos bots do que o especificado acima.

A equipe do ASF sugere rodar (e **possuir**) **no máximo 10 bots**, não há suporte para qualquer quantidade acima disso e caso você insista é por sua conta e risco e contra nossas sugestões. Essa recomendação é feita tanto com base em diretrizes internas da Valve quanto nossas sugestões. Se você vai concordar com essa regra ou não é escolha sua, o ASF, como ferramenta, não vai te proibir de fazer isso, mesmo se isso resultar na suspensão de suas contas no Steam. O ASF vai exibir um aviso se você passar da quantidade recomendada, porém vai permitir que você rode quantos bots quiser, por sua própria conta e risco e ciente da falta de suporte.

* * *

### Então eu posso rodar mais instâncias do ASF?

Você pode executar quantas instâncias do ASF você quiser em um computador, assumindo que cada instância tenha sua própria pasta e suas próprias configurações, e que uma conta usada em uma instância não seja usada em outra. No entanto, pergunte-se por que você quer fazer isso. O ASF é otimizado para lidar com uma dezena, ou até uma centena de contas ao mesmo tempo, e iniciar essas dezenas de bots em suas próprias instâncias do ASF diminui o desempenho, toma mais recursos do sistema operacional (como CPU e memória), e causa possíveis problemas de sincronização entre diferentes instâncias do ASF, já que o ASF é forçado a compartilhar seus limitadores com outras instâncias.

Portanto, eu **sugiro fortemente** sempre executar o máximo de uma instância ASF por IP/interface. Se você tiver mais IPs/interfaces, você pode executar tranquilamente mais instâncias do ASF com cada instância usando seu próprio IP/interface ou uma configuração de `WebProxy` individual. Caso contrário, abrir mais instâncias do ASF é totalmente inútil já que você não ganhará nada ao executar mais de 1 instância no mesmo IP/interface. O Steam não vai permitir magicamente que você rode mais bots apenas porque você os iniciou em outra instância do ASF, e pra começar, o ASF não te limita quanto a isso.

Claro, existem casos válidos de uso para várias instâncias do ASF na mesma interface de rede, tal como hospedar o serviço ASF para seus amigos quando cada amigo tiver sua própria instância exclusiva do ASF para garantir o isolamento entre bots e até mesmo entre os processos do ASF em si, no entanto, você não está contornando nenhuma limitação do Steam fazendo isso, esse é um propósito totalmente diferente.

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

> Você é o responsável pela confidencialidade de seu nome de login e senha assim como pela segurança do seu sistema de computador. A Valve não é responsável pelo uso da sua senha e conta nem por todas as comunicações e atividades no Steam que resultem do uso do seu nome de usuário e senha por você, ou por qualquer pessoa a qual você tenha, de forma intencional ou não, divulgado o seu login e/ou senha em violação à disposição de confidencialidade.

O ASF é licenciado pela a licença liberal Apache 2.0, que permite que outros desenvolvedores integrem legalmente o ASF com seus próprios projetos e serviços. No entanto, não garantimos que tais projetos de terceiros utilizando o ASF sejam seguros, revisados, apropriados ou legais de acordo com o **[Acordo de Assinatura do Steam](https://store.steampowered.com/subscriber_agreement/?l=brazilian)**. Se você quer saber a nossa opinião, **nós incentivamos fortemente você a NÃO compartilhar QUAISQUER detalhes de conta com serviços de terceiros**. Se acontecer de tal serviço ser um **tipo de fraude**, você vai estar sozinho com o problema, provavelmente sem sua conta Steam e o ASF não assume qualquer responsabilidade por serviços de terceiros que aleguem ser seguros, pois a equipe do ASF não autorizou nem revisou qualquer um desses. Em outras palavras, **você os esta usando por sua conta e risco, contra a sugestão feita acima**.

Além disso, o Acordo de Assinatura do Steam diz claramente que:

> Você não poderá revelar, compartilhar ou de outra forma permitir que outras pessoas usem sua senha ou Conta, exceto se for especificamente autorizado de outra forma pela Valve.

É seu problema e sua escolha. Só não diga que ninguém te avisou. O ASF em si cumpre todas as regras mencionadas acima, já que você não está compartilhando detalhes da sua conta com ninguém, e você está usando o programa para seu uso pessoal, mas qualquer outro "serviço de coleta de cartas" exigirá suas credenciais de conta, portanto, também viola a regra acima (na verdade várias delas). Assim como na avaliação do Acordo de Assinatura do Steam, não estamos oferecendo qualquer aconselhamento legal, e você deve decidir por sua conta se você quer usar esses serviços ou não; de acordo com o que dissemos ele **viola diretamente o Acordo de Assinatura do Steam** e pode resultar em suspensão caso a Valve descubra. Conforme salientado acima, ** recomendamos NÃO usar nenhum dos tais serviços**.

* * *

## Problemas

* * *

### Um dos meus jogos está há mais de 10 horas coletando, mas eu ainda não recebi nenhuma carta dele!

O motivo pode estar relacionado a um problema conhecido do Steam que ocorre quando você tem duas licenças para o mesmo jogo, uma das quais tem limite de drops de cartas. Isso geralmente acontece quando você ativa o jogo gratuitamente durante uma doação em massa no Steam, e depois ativa uma chave para o mesmo jogo (mas sem limitações), de um pacote pago por exemplo. Se tal situação acontecer, o Steam reporta na página de insígnias que o jogo ainda tem cartas a receber, mas não importa o quanto você jogue o jogo - as cartas nunca aparecerão devido à licença gratuita na sua conta. Uma vez que não é um problema do ASF e sim do Steam, não podemos de forma alguma contorná-lo do lado do ASF, e você mesmo precisa resolvê-lo.

Há duas maneiras de resolver o problema. Primeiro, você pode colocar esse jogo na lista negrado ASF, seja com o **[comando](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-pt-BR)** `ibadd` ou com a **[propriedade de configuração](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-pt-BR)** `Blacklist`. Isso impedirá que o ASF tente coletar cartas deste jogo, mas não resolverá o problema real, que impede que você obtenha cartas do jogo em questão. Em segundo lugar, você pode usar a ferramenta de suporte Steam para remover a licença gratuita da sua conta, deixando apenas a licença completa que inclui os recebimentos de cartas. Para fazer isso, primeiro visite sua página de **[licenças e ativações de códigos de produto](https://store.steampowered.com/account/licenses)** e localize tanto a licença gratuita quanto a paga para o jogo em questão. Geralmente é bem fácil - ambos têm nome semelhante, mas o gratuito tem "Limited Free Promotional Package" ou outra "promo" no nome da licença, além de "Cortesia" na coluna "Método de aquisição". Às vezes pode ser mais complicado, por exemplo, se o pacote gratuito estivesse em algum outro pacote e com um nome diferente. Se você encontrou duas licenças como descrito - então é de fato o problema descrito aqui, e você pode remover com segurança a licença gratuita sem perder o jogo.

Para remover a licença gratuita da sua conta, visite **[página de suporte Steam](https://help.steampowered.com/pt-br/wizard/HelpWithGame)** e coloque o nome do jogo afetado no campo de busca o jogo deverá estar disponível na seção "produtos", clique nele. Como alternativa, você pode simplesmente usar o link `https://help.steampowered.com/pt-br/wizard/HelpWithGame?appid=<appID>` e substituir `<appID>` pelo appID do jogo em questão. Depois, clique em "Desejo remover este jogo da minha conta permanentemente" e então selecione a licença gratuita que você encontrou acima, geralmente aquela com "pacote promocional gratuito limitado" no nome (ou semelhante). Após a remoção da licença gratuita o ASF deverá conseguir coletar cartas do jogo afetado sem problemas, você deve reiniciar a operação de coleta após a remoção, apenas para ter certeza de que o Steam retirou a licença certa desta vez.

* * *

### O ASF não detecta o jogo `X` como disponível para coleta, embora eu saiba que ele inclui cartas colecionáveis Steam!

Existem duas razões principais aqui. A primeira e mais óbvia é o fato de que você está se referindo a **loja Steam**, onde o jogo é anunciado como contendo Cartas Colecionáveis. Esta é a suposição **errada**, uma vez que ela simplesmente assume que o jogo **tem** cartas inclusas, mas não quer dizer que essa função para esse jogo esteja **habilitada** imediatamente. Você pode ler mais sobre isso no **[anúncio oficial](https://steamcommunity.com/games/593110/announcements/detail/1954971077935370845)**.

Em suma, o ícone de Cartas Colecionáveis na loja Steam não significa nada, verifique sua **[página de insígnias](https://steamcommunity.com/my/badges)** para confirmar se um jogo tem cartas ou não; é isso que o ASF faz. Se o seu jogo não aparece na lista como um jogo com possibilidade de conseguir cartas, então **não** é possível coletar desse jogo, independentemente do motivo.

A segunda questão é menos óbvia, e é a situação onde o seu jogo aparece na página de insígnia como disponível para recebimento de cartas e, no entanto, não está sendo executado pelo ASF. A menos que você esteja se deparando com algum bug, tal como o ASF ser incapaz de verificar as páginas de insígnias (descritas abaixo), é simplesmente um efeito de cache e do lado do ASF a Steam ainda está mostrando a página de insígnias desatualizada. Esse problema deve resolver-se mais cedo ou mais tarde, quando o cache for invalidado. Também não dá para consertar isso do nosso lado.

Claro, tudo isso pressupõe que você está executando o ASF com as configurações padrão intocadas, uma vez que você também pode ter adicionado este jogo à lista de bloqueio, usado `true` em `IdlePriorityQueueOnly`, `false` em `IdleRefundableGames` entre outras coisas.

* * *

### Por que as horas de jogo não aumentam nos jogos executados pelo ASF?

Elas aumentam, mas **não em tempo real**. O Steam registra seu tempo de jogo em intervalos fixos e agenda uma atualização para isso, mas não há garantia de que isso ocorra imediatamente após o fim da sessão de jogo. Se fosse possível pular a contagem de tempo de jogo durante o processo de coleta, tenha certeza de que teríamos implementado isso há muito tempo, e usaríamos como comportamento padrão. Mas não implementamos justamente porque não é possível; só porque as horas em jogo não foram atualizadas em tempo real não significa que não estão registradas.

* * *

### Qual é a diferença entre um aviso e um erro no registro (log)?

O ASF grava em seu registro um monte de informações de vários níveis. Nosso objetivo é explicar **precisamente** o que o ASF está fazendo, incluindo quais problemas no Steam que ele tem de lidar, ou outros problemas a superar. Na maioria das vezes nem tudo é relevante, é por isso que temos dois níveis principais usados no ASF em termos de problemas - um nível de aviso e um nível de erro.

A regra geral do ASF é que avisos **não** são erros, portanto eles **não** devem ser relatados. Um aviso é um indicador de que algo potencialmente indesejado aconteceu. Se é falta de resposta do Steam, a API retornando erros ou queda na sua conexão de rede; é um aviso, e significa que esperávamos que acontecesse, então não se incomode os desenvolvedores com isso. Claro, você é livre para perguntar sobre eles ou obter ajuda usando o nosso suporte, mas você não deve assumir que são erros do ASF e que vale a pena serem relatados (a menos que nós confirmemos o contrário).

Erros, por outro lado, indicam uma situação que não deveria acontecer, porém eles devem ser relatados se você se certificou de que não é você quem os está causando. Se é uma situação comum e que esperamos que aconteça, então ele será convertido em um aviso. Caso contrário, possivelmente é um erro que deve ser corrigido e não ignorado em silêncio, supondo que não seja resultado de problema técnico seu. Por exemplo, colocar conteúdo inválido no arquivo `ASF.json` acarretará um erro já que o ASF não vai conseguir processá-lo, mas como foi você quem o editou, você não deve nos reportar esse erro (a menos que você tenha confirmado que o ASF está errado e que a estrutura está absolutamente certa).

Em suma - reporte erros, não reporte avisos. Porém você ainda perguntar sobre os avisos e receber ajuda em nossas seções de suporte.

* * *

### O ASF não funciona, a janela do programa fecha imediatamente!

Em condições normais, qualquer falha ou fechamento do ASF vai gerar um arquivo `log.txt` na pasta do programa que pode ser usado para encontrar a causa. Além disso, alguns arquivos de logs mais recentes serão arquivados na pasta `logs`, uma vez que o arquivo `log.txt` é reescrito toda vêz que o ASF é executado.

No entanto, se nem mesmo o tempo de execução .NET Core for capaz de inicializar no seu computador, então o arquivo `log.txt` não será gerado. Se isso acontecer com você, é provável que você tenha esquecido de instalar o .NET Core, como consta no guia de **[instalação](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up-pt-BR#instalador-para-sistemas-operacionais-espec%C3%ADficos)**. Outros problemas comuns incluem tentar rodar a variante errada do ASF para seu SO, ou de alguma outra forma a falta de dependências nativas do .NET Core. Se a janela de console facha rápido demais para você conseguir ler a mensagem, então abra um console independente e rode o executável do ASF por lá. Por exemplo, no Windows, abra a pasta do ASF, segure `Shift`, clique com o botão direito dentro da pasta e escolha "Abrir janela de comando (ou powershell) aqui", digite no console `.\ArchiSteamFarm.exe` e confirme com enter. Desta forma você obterá a mensagem correta do motivo do ASF é não iniciar corretamente.

* * *

### O ASF está encerrando minha sessão no cliente Steam enquanto eu jogo! / `Você já iniciou a sessão em outro computador`

Isto aparece como uma mensagem sobreposta na Steam indicando que a conta está sendo usada em outro lugar enquanto você está jogando. Esse problema pode ter dois motivos diferentes.

Um dos motivos é causado por pacotes "quebrados" (jogos) que não sabem como manter o bloqueio do jogo corretamente, mas esperam que o bloqueio seja controlado pelo cliente. Um exemplo de tal pacote seria o Skyrim SE. Seu cliente Steam executa o jogo normalmente, mas o jogo não se registra como sendo executado. Por conta disso o ASF assume que está livre para resumir o processo, e fazendo isso ele te desliga da rede Steam, uma vez que o Steam detecta de repente que a conta está sendo usada em outro lugar.

Outro motivo pode ser causado se você estiver jogando em seu PC enquanto o ASF espera (especialmente em outro computador) e você perde a conexão com a internet. Nesse caso, a rede Steam te marca como offline e libera o bloqueio de jogo (como no caso acima), o que faz com que o ASF (por exemplo, em outro computador) retome a coleta. Quando seu PC se reconecta o Steam não consegue acionar o bloqueio de jogo novamente (que agora está com o ASF, também semelhante ao caso acima) e mostra a mesma mensagem.

Ambas as causas são muito difíceis de serem contornadas pelo lado do ASF, já que ele simplesmente retoma a coleta uma vez que a rede Steam informe que a conta está livre para ser usada. É isso que acontece normalmente quando você fecha o jogo, mas com pacotes "quebrados" isso pode acontecer imediatamente, mesmo se o seu jogo ainda estiver sendo executado. O ASF não tem como saber se sua conexão caiu, se você parou de jogar, ou se ainda está jogando um jogo que não segurou o bloqueio apropriadamente.

A única solução para este problema é pausar manualmente seu bot com o comando `pause` antes de começar a jogar e o reiniciar com o comando `resume` assim que estiver pronto. Ou você pode ignorar o problema e agir como se tivesse jogado com o cliente Steam off-line.

* * *

### `Desconectado do Steam!` - não consigo conectar aos servidores Steam.

O ASF pode apenas **tentar** se conectar aos servidores Steam e pode falhar por vários motivos, incluindo a falta de conexão com a internet, Steam offline, seu firewall bloqueando a conexão, ferramentas de terceiros, rotas configuradas incorretamente ou falhas temporárias. Você pode habilitar o modo `Debug` para verificar um registro mais detalhado que indica a falha exata, embora ela geralmente seja causada por suas próprias ações, por exemplo, usando "CS:GO MM Server Picker" que bloqueia vários IPs do Steam, tornando muito difícil que você realmente alcance a rede Steam.

O ASF fará o seu melhor para se conectar, que inclui não só pedir uma lista atualizada de servidores, mas também tentar outro IP quando o último falhar, então se realmente for um problema temporário com um servidor ou rota específica, o ASF vai se conectar mais cedo ou mais tarde. No entanto, se você está atrás de um firewall ou impossibilitado de alguma forma de alcançar os servidores Steam, então obviamente você precisa consertar o problema por sua conta, com a potencial ajuda do modo modo de depuração - `Debug`.

Também é possível que seu computador não seja capaz de estabelecer conexão com servidores Steam usando o protocolo padrão do ASF. Você pode alterar protocolos que ASF tem permissão de usar modificando o parâmetro `SteamProtocols` na configuração global. Por exemplo, se você está tendo problemas em alcançar os servidores Steam com o protocolo `UDP` (por exemplo, devido a firewalls), talvez você tenha mais sorte com `TCP` ou `WebSocket`.

No caso improvável de você ter armazenado em cache os endereços incorretos dos servidores, por exemplo, por ter movido a pasta `config` do ASF de um computador para outro que esteja localizado em outro país, deletar o arquivo `ASF.db` a fim de atualizar a lista de servidores na próxima execução do programa pode ajudar. Na maioria das vezes isso não é necessário e não precisa ser feito, já que essa lista é atualizada automaticamente quando o programa é executado, assim como quando a conexão é estabelecida; mencionamos isso aqui apenas para mostrar uma forma de se eliminar qualquer coisa relacionada à lista de servidores Steam armazenada pelo ASF.

* * *

### `Não foi possível obter informações das insígnias, tentaremos novamente mais tarde!`

Normalmente isso significa que você está usando o PIN do modo familia para acessar sua conta e esqueceu de colocá-lo na configuração do ASF. Voce deve colocar um PIN válido no parâmetro de configuração do bot `SteamParentalCode`, caso contrário o ASF não conseguirá acessar a maioria do conteúdo web, por isso não conseguirá trabalhar corretamente. Vá até **[configuração](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-pt-BR)** para saber mais sobre o `SteamParentalCode`.

Outras razões incluem: um problema temporário do Steam, um problema de rede ou coisas assim. Se o problema não se resolver depois de várias horas e você tem certeza de que configurou o ASF corretamente, sinta-se livre para nos informar.

* * *

### O ASF está falhando com o erro: `Falha na solicitação após 5 tentativas`!

Normalmente isso significa que você está usando o PIN do modo familia para acessar sua conta e esqueceu de colocá-lo na configuração do ASF. Voce deve colocar um PIN válido no parâmetro de configuração do bot `SteamParentalCode`, caso contrário o ASF não conseguirá acessar a maioria do conteúdo web, por isso não conseguirá trabalhar corretamente. Vá até **[configuração](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-pt-BR)** para saber mais sobre o `SteamParentalCode`.

Se PIN do modo família não for o motivo, então este é um erro mais comum e você deve se acostumar com isso, significa simplesmente que o ASF enviou uma solicitação para a rede Steam e não recebeu uma resposta válida 5 vezes seguidas. Normalmente, isso significa que a Steam caiu, está tendo algumas dificuldades ou está em manutenção; o ASF está ciente de tais problemas e você não deve se preocupar com eles, a menos que aconteçam constantemente e por mais que algumas horas, e que outros usuários não estejam tendo esses problemas.

Como verificar se o Steam caiu? **[Steam Status](https://steamstat.us)** é uma excelente fonte para verificar se o Steam **deveria estar online**, se você notar erros, especialmente relacionados com a Comunidade ou o API Web, então o Steam está tendo problemas, então você pode deixar o ASF fazer seu trabalho sozinho depois de um tempo, ou esperar.

No entanto esse nem sempre é o caso, pois em alguns casos os problemas com o Steam podem não ser detectados pelo Steam Status, um exemplo disso foi quando a Valve quebrou o suporte HTTPS para a Comunidade Steam em 7 de junho de 2016 - acessar**[Comunidade Steam](https://steamcommunity.com)** com o https retornava um erro. Portanto, não confie cegamente no Steam Status também, é melhor você mesmo verificar se tudo funciona como deveria.

Além disso, a rede Steam inclui várias medidas para limitar o envio de requisições que vão banir temporariamente seu IP caso você faça muitas solicitações de uma vez. O ASF está a par disso e oferece vários limitadores diferentes nas configurações, os quais você deve usar. As configurações padrão se baseiam em uma quantidade **sensata** de bots, se você estiver usando tantos bots que até mesmo o Steam está te desconectando você deve ajustá-los até que que ele pare ou fazer o que dissemos. Presumimos que a segunda opção não lhe serve, então vá até o tópico de configuração e preste atenção especialmente em `WebLimiterDelay` que é um limitador geral aplicado a todas as solicitações de rede.

Não há nenhuma "regra de ouro" que funcione para todo mundo, porque os bloqueios são fortemente influenciados por fatores de terceiros, é por isso que você tem que experimentar por conta e encontrar um valor que funcione para você. Você também pode ignorar o que eu disse e usar algo como `10000` e é certo que tudo funcionará, mas depois não reclame que seu ASF leva 10 segundos para reagir a tudo e que a análise da página de insígnias demora 5 minutos. Além disso, é inteiramente possível que nenhum limitador traga resultado se você tem uma quantidade tão grande de bots que você esteja atingindo o **[limite máximo](#quantos-bots-posso-rodar-com-o-asf)** que foi mencionado acima. Sim, é inteiramente possível que você consiga se conectar sem problemas na rede Steam, mas o Steam web vai se recusar a te ouvir se você tiver 100 sessões rodando ao mesmo tempo. O ASF necessita que tanto a rede Steam quanto o Steam web sejam cooperativos, basta que um caia para que você não consiga mais recuperar de um problema.

Se nada ajudar e você não tiver idéia do que possa estar errado, você pode habilitar o modo `Debug` e ver no registro do ASF porque exatamente as solicitações estão falhando. Por exemplo:

```text
InternalRequest() HEAD https://steamcommunity.com/my/edit/settings
InternalRequest() Forbidden <- HEAD https://steamcommunity.com/my/edit/settings
```

Vê esse código `Forbidden` (`Proibido`)? Isso significa que você foi banido temporariamente por excesso de solicitações, porque você ainda não configurou o `WebLimiterDelay` corretamente (assumindo que você obteve o mesmo código de erro para todas as outras solicitações). Podem haver também outros motivos listados aqui, tais como `InternalServerError` e `ServiceUnavailable` e limites de tempo excedidos que indicam manutenção/problemas no Steam. Você sempre pode tentar visitar o link mencionado pelo ASF e verificar se ele funciona; se não funcionar, então você sabe por que o ASF também não pode acessá-lo. Se funcionar e o mesmo erro não desaparecer depois de um dia ou dois, pode valer a pena investigar e reportar.

Antes de fazer isso você deve **certificar-se de que vale a pena relatar o erro**. Se ele estiver mencionado nesse FAQ, uma questão relacionada a troca por exemplo, então não. Se for problema temporário que aconteceu uma ou duas vezes, principalmente quando sua rede estava instável ou o Steam offline - então, não. No entanto, se você teve esse problema várias vezes seguidas no espaço de 2 dias, reiniciou tanto ASF quanto seu computador e certificou-se que não há nenhuma resposta pra ele no FAQ, então pode valer a pena pedir suporte.

* * *

### O ASF parece travar e não mostra nada no console até que eu pressione uma tecla!

Você provavelmente está usando o Windows e seu console está com modo de edição rápida ativado. Consulte **[esse artigo](https://stackoverflow.com/questions/30418886/how-and-why-does-quickedit-mode-in-command-prompt-freeze-applications)** no StackOverflow para obter uma explicação técnica. Você deve desabilitar o modo de edição rápida clicando com o botão direito na sua janela de console do ASF, indo até propriedades e desmarcando a caixa de seleção apropriada.

* * *

### O ASF não consegue aceitar ou enviar trocas!

O óbvio primeiro: novas contas são limitadas. Até que você desbloqueie a conta colocando pelo menos $5 (dólares) na sua carteira Steam ou gastando esse valor na loja, o ASF não pode aceitar nem enviar trocas usando essa conta. Neste caso, o ASF indicará que esse inventário parece vazio, porque todas as cartas nele não são trocáveis. Também não será possível receber qualquer troca, já que para isso o ASF precisa ser capaz de obter uma chave de API e esse recurso é desabilitado em contas limitadas. Resumindo, trocas estão fora de questão para contas limitadas, sem exceções.

Depois, se você não usa o **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-pt-BR)**, é possível que o ASF tenha aceitado/enviado a troca, mas você precisa confirmar ela através de seu e-mail. Do mesmo jeito, se você usa o 2FA padrão, você precisa confirmar a troca pelo autenticador. As confirmações são **obrigatórias**, então se você não quer aceitá-las manualmente, considere importar seu autenticador para o ASF 2FA.

Observe também que você só pode trocar com seus amigos e pessoas que tenham seu link de troca. Se você está tentando fazer troca de um bot para a conta master, tal como `loot`, então você precisa ter seu bot na sua lista de amigos, ou seu `SteamTradeToken` declarado na configuração do bot. Certifique-se de que o token é válido, caso contrário, você não será capaz de enviar uma troca.

Por último, lembre-se que novos dispositivos tem um bloqueio de trocas de 7 dias, então se você acabou de adicionar sua conta ao ASF, espere pelo menos esses 7 dias - tudo deve funcionar corretamente após esse período. Essa limitação inclui **tanto** aceitar **quanto** enviar trocas. Nem sempre ele funciona e há pessoas que conseguem enviar e aceitar trocas instantaneamente. No entanto a maioria das pessoas são afetadas e o bloqueio **vai** acontecer, mesmo que você consiga enviar e aceitar trocas através de seu cliente Steam no mesmo computador. Tenha paciencia, não há nada que você possa fazer para acelerar isso. Da mesma forma, você pode ser bloqueado por remover/mudar várias configurações do Steam relacionados à segurança, tal como o 2FA, senha, e-mail e afins. Em geral, verifique se você pode enviar uma troca dessa conta manualmente, caso positivo é provavelmente o bloqueio clássico de 7 dias por conta de um novo dispositivo.

E finalmente, tenha em mente que uma conta pode ter apenas 5 trocas pendentes para outra, então o ASF vai falhar ao enviar trocas se você tem 5 (ou mais) pendentes naquele bot para serem aceitas. Raramente isso é um problema, mas vale mencionar especialmente se você configurou o ASF para enviar trocas automaticamente sem usar o ASF 2FA e esqueceu de confirmá-las.

Se nada aqui te ajudou, você sempre pode habilitar o modo `Debug` e ver porque as solicitações estão falhando. Note que o Steam gera muitas bobagens na maioria das vezes e o motivo pode não fazer nenhum sentido lógico ou pode estar totalmente incorreto - se você decidir interpretar esse motivo você deve ter um conhecimento profundo sobre o Steam e suas falhas. Também é muito comum ver esse problema com nenhuma razão lógica, e a única solução sugerida neste caso é adicionar novamente a conta no ASF (e esperar 7 dias novamente). Às vezes esse problema se corrige *magicamente*, da mesma forma que ele quebra. No entanto, geralmente é só i bloqueio de 7 dias, um problema temporário do Steam ou ambos. É melhor esperar uns dias antes de verificar manualmente o que está errado, a menos que você tem um desejo grande de depurar a causa real (e geralmente você será forçado a esperar mesmo assim, porque a mensagem de erro não vai fazer nenhum sentido, nem te ajudar em nada).

Em todo caso, o ASF pode apenas **tentar** enviar uma solicitação adequada para o Steam para aceitar/enviar trocas. Se o Steam aceita esse pedido ou não, está fora do alcance do ASF, e o ASF não vai fazer funcionar magicamente. Não há nenhum bug relacionado a isso e também não há o que melhorar pois a lógica acontece fora do ASF. Portanto, não peça para consertarmos algo que não está quebrado e também não pergunte por que o ASF não consegue aceitar ou enviar trocas - **Eu não sei, e o ASF também não**. Lide com isso ou conserte você mesmo, se você souber como.

* * *

### Por que tenho que colocar o código 2FA/SteamGuard em cada início de sessão? / `Chave de sessão expirada removida`

O ASF usa chaves de sessão (se você manteve `UseLoginKeys` habilitado) para manter as credenciais válidas, o mesmo mecanismo que o Steam usa - por isso o token 2FA/SteamGuard é necessário apenas uma vez. Mas devido a comportamentos estranhos e problemas comuns na rede Steam é totalmente possível que essa chave de sessão não seja salva na rede, eu já vi tais problemas não somente no ASF, mas no cliente padrão do Steam também (a necessidade de colocar usuário + senha toda vez, mesmo com a opção "lembre-me neste computador" marcada).

Você pode remover o arquivo `BotName.db` (e `BotName.bin`, se ele existir) da conta afetada e tentar vincular ASF a sua conta novamente, mas isso provavelmente não vai adiantar. A solução real baseada no ASF é importar seu autenticador como **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-pt-BR)** - desta forma o ASF pode gerar tokens automaticamente quando eles forem necessários, e você não precisa colocá-los manualmente. Geralmente esse problema se resolve magicamente sozinho depois de algum tempo, então você pode simplesmente esperar. É claro, você também pode pedir uma solução pra Valve, pois eu não posso forçar a rede Steam a aceitar nossas chaves de registro.

Além disso você também pode desativar as chaves de sessão definindo o parâmetro `UseLoginKeys` no arquivo de configuração como `false`, mas isso não vai resolver o problema, vai apenas pular a falha inicial acerca das chaves de login. ASF já está ciente do problema explicado aqui e vai tentar o possível para não usar as credenciais de login se ele conseguir garantir ele mesmo as credenciais de login, então não há necessidade de configurar `UseLoginKeys` manualmente se você puder indicar todos os detalhes de login além de usar o ASF 2FA.

* * *

### Estou recebendo o erro: `Não foi possível iniciar a sessão no Steam: InvalidPassword ou RateLimitExceeded`

Este erro pode ocorrer por muitos motivos, alguns deles podem ser:

- Combinação de usuário/senha inválidos (obviamente)
- A chave de sessão usada pelo ASF expirou
- Muitas tentativas de conexão em um curto período de tempo (prevenção de força bruta)
- Muitas tentativas falhas de conexão em um curto período de tempo (limite de tentativas)
- Exigência de captcha para se conectar (muito provavelmente causado por um dos dois motivos acima)
- Qualquer outro motivo pelo qual a rede Steam pode estar te impedindo de se conectar.

No caso da força bruta e do limitador de tentativas, o problema vai desaparecer depois de algum tempo, então espere. Se você tem esse problema com frequência, talvez seja aconselhável aumentar o parâmetro `LoginLimiterDelay` na configuração do ASF. Reiniciar excessivamente o programa e outras solicitações de conexão, intencionais ou não, definitivamente não vai ajudar em nada, então tente evitá-las se possível.

No caso de a chave de sessão ter expirado o ASF vai remover a antiga e pedir uma nova na próxima conexão (o que vai exigir seu token 2FA se sua conta estiver protegido por ele. Se sua conta está usando o ASF 2FA, o token será gerado e usado automaticamente). Isso pode acabar acontecendo as vezes, porém se você se depara com esse problema a cada login, é possível que o Steam tenha decidido, por algum motivo, ignorar seus pedidos para se manter conectado, como mencionado no problema **[acima](#por-que-tenho-que-colocar-o-código-2fasteamguard-em-cada-início-de-sessão--chave-de-sessão-expirada-removida)**. É claro que você pode desativar `UseLoginKeys`, mas isso só vai tirar a necessidade de remover chaves de sessão espiradas a cada login e não vai resolver o problema. A única solução real para o problema acima é usar o ASF 2FA.

E por último, se você usou o a combinação de usuário + senha errada, obviamente você precisa arrumar ou desabilitar o bot que está tentando se conectar usando essas credenciais. O ASF não consegue adivinhar se `InvalidPassword` significa que as credenciais são inválidas, ou qualquer um dos motivos listados acima, portanto ele continuar tentando até que tenha êxito.

Tenha em mente que o ASF tem seu próprio sistema interno para reagir adequadamente a alguns problemas do Steam, provavelmente ele vai se conectar e retomar o trabalho, portanto não precisa fazer nada se o problema for temporário. Reiniciar o ASF na tentativa de corrigir problemas magicamente só vai piorar as coisas (já que o novo processo do ASF não sabe que o processo anterior não pôde se conectar, e vai tentar se conectar em vez de esperar), então evite fazer isso a não ser que você saiba o que está fazendo.

Finalmente, assim como todas as solicitações Steam, o ASF pode apenas **tentar** se conectar, usando as credenciais fornecidas. Se essa tentativa será bem-sucedida ou não está fora do escopo e da lógica do ASF; não existe nenhum bug, e nada pode ser corrigido ou melhorado com relação a isso.

* * *

### `System.IO.IOException: Input/output error`

Se esse erro ocorreu durante uma entrada no ASF (por exemplo, `Console.ReadLine()` é mostrado no stacktrace) então ele foi causado por seu ambiente que não permitiu que o ASF lesse uma entrada no seu console. Isso pode ocorrer por muitos motivos, mas o mais comum é você rodar o ASF no ambiente errado (por exemplo, no segundo plano `nohup` ou `&` em vez da `screen` no Linux). Se o ASF não puder acessar sua entrada padrão, então você verá este erro no registro e a incapacidade do ASF de usar seus dados durante o tempo de execução.

Se você **espera** que isso aconteça é porque você **pretende** executar o ASF em um ambiente sem entradas, então você deve explicitamente dizer isso ao ASF, definindo o modo **[`Headless`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-pt-BR#headless)** de forma apropriada. This will tell ASF to never ask for user input under any circumstance, allowing you to run ASF in input-less environments safely.

* * *

### `System.Net.Http.WinHttpException: Ocorreu um erro de segurança`

Esse erro acontece quando o ASF não consegue estabelecer conexão segura com o servidor especificado, quase sempre por problemas com certificados SSL.

Em quase todos os casos esse erro é causado por **data/hora errados em seu computador**. Todo certificado SSL tem uma data de emissão e uma de validade. Se o seu computador tiver uma data incorreta e fora do intervalo especificado no certificado, ele não poderá ser confiável, pois isso pode ser um sinal de ataque MITM e o ASF se recusa a estabelecer uma conexão.

A solução arrumar a data no seu computador. É altamente recomendado usar a sincronização automática de data, como a sincronização nativa do Windows ou o `ntpd` no Linux.

Se você tem certeza de que a data no seu computador está certa e mesmo assim o erro persiste, então, assumindo que não é uma questão temporária, os certificados SSL em que seu sistema confia podem estar desatualizados ou inválidos. Neste caso você deve garantir que seu computador pode estabelecer conexões seguras, por exemplo, verificando se você pode acessar `https://github.com` com qualquer navegador de sua escolha, ou uma ferramenta CLI como `curl`. Se você confirmou que funciona, sinta-se livre para postar a questão no nosso grupo Steam.

* * *

### `System.Threading.Tasks.TaskCanceledException: Uma tarefa foi cancelada.`

Este aviso significa que o Steam não respondeu à solicitação do ASF no tempo esperado. Isso normalmente é causado por falhas da rede Steam e não afeta em nada o ASF. Em outros casos é o mesmo que pedidos falhando após 5 tentativas. Informar esses problemas não faz sentido na maioria das vezes, já que não podemos forçar o Steam a responder nossas solicitações.

* * *

### `O inicializador de tipo de 'System.Security.Cryptography.CngKeyLite' acionou uma exceção`

Esse problema é quase exclusivamente causado pelo serviço do windows `CNG Key Isolation` estar parado/desabilitado, ele fornece a funcionalidade de criptografia básica para o ASF, sem o qual o programa não consegue rodar. Você pode resolver este problema abrindo `services.msc` e garantindo que o serviço `CNG Key Isolation` não esteja desativado na inicialização e está atualmente em execução.

* * *

### `Um erro fatal foi encontrado. Não foi possível extrair conteúdos do pacote`

### `System.BadImageFormatException: Could not load file or assembly`

### `System.IO.FileNotFoundException: Could not load file or assembly`

O ASF usa a publicação de arquivo único em variantes específicas para Sistema Operacional, fazendo com que o aplicativo seja extraído para uma pasta temporária `<tmp>/.net` na inicialização (caso necessário). No Windows, é a pasta `%TEMP%\.net` (geralmente `C:\Users\<SeuUsuário>\AppData\Local\Temp\.net`), no Linux, é `/var/tmp/.net`. A pasta `.net` pode não existir por padrão, ela será criada na primeira vez que for necessária.

O primeiro problema é causado por o ASF não poder extrair seus próprios arquivos na pasta, o segundo por uma extração corrompida - provavelmente você fechou o ASF antes que ele fosse capaz de extrair tudo. Geralmente, a solução mais simples para esse problema, seja no Windows ou no Linux, é excluir a pasta `.net` indicado acima e tentar de novo, o que geralmente deve resolver o problema.

O processo do ASF precisa de permissão para gravação na pasta especificada acima. No Windows, geralmente isso não é um problema, mas no Linux você deve se assegurar que o usuário que o processo do ASF está rodando possui acesso ao diretório `/var/tmp/.net`, que é o padrão, porém pode exigir passos extras caso você não esteja usando as permissões padrões ou semelhantes.

Como alternativa, você também pode definir uma variável de ambiente personalizada `DOTNET_BUNDLE_EXTRACT_BASE_DIR`, que apontará para o diretório de extração de sua escolha. Isso pode ser especialmente útil se você não quiser que o ASF use a pasta padrão para extração do pacote. Tenha em mente que as mesmas permissões necessárias também se aplicam à nova localização.

* * *

### O ASF está sendo detectado como um malware pelo meu antivírus! O que está acontecendo?

**Certifique-se de que você baixou o ASF de uma fonte confiável**. A única fonte oficial e confiável a página de **[lançamentos ](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** no GitHub (e essa também é a fonte para as atualizações automáticas do ASF) - **qualquer outra fonte não é confiável e pode conter malware adicionado por outras pessoas** - você não deve confiar em nenhuma outra fonte de dowload e garantir que sua cópia do ASF sempre venha de nós.

Se você confirmou que o ASF foi baixado de fonte confiável, então muito provavelmente é simplesmente um falso positivo. Isso **já aconteceu no passado**, **está acontecendo agora, **e **vai acontecer no futuo**. Se você está preocupado com a segurança ao usar ASF, sugiro verificar o ASF com vários antivírus diferentes para ver a proporção real de detecção, por exemplo através do **[VirusTotal](https://www.virustotal.com)** (ou qualquer outro serviço da web semelhante que preferir).

Se o antivírus que você está usando detectar erroneamente o ASF como um malware, então **é uma boa ideia enviar esta amostra do arquivo para os desenvolvedores do seu antivírus, para que possam analisá-lo e melhorar sua ferramente de detecção**, já que claramente ela não está funcionando tão bem quanto você acha que ele deveria. Isso não é um problema no código do ASF e também não há nada para para nós consertarmos, já que não estamos distribuindo malware algum, portanto não faz qualquer sentido nos relatar esses falsos positivos. Nós recomendamos fortemente enviar uma amostra do ASF para posterior análise como dito acima, mas se você não quiser se preocupar com isso, então você pode adicionar o ASF para algum tipo de lista de exceções do antivírus, desativar seu antivírus ou simplesmente usar outro. Infelizmente, estamos acostumados com antivírus sendo estúpidos, já que de vez em quando alguns antivírus detectam o ASF como um vírus, o que normalmente dura muito pouco tempo até ser arrumado pelos desenvolvedores, mas como nós apontamos acima - **aconteceu**, **acontece** e **acontecerá** o tempo todo. O ASF não inclui qualquer código malicioso, você pode revisar o código do ASF e mesmo compilar da fonte se quiser. Nós não somos hackers para esconder o código do ASF de uma forma que ele não seja detectado pelo antivírus e não retorne falsos positivos, então não espere que nós arrumemos o que não está estragado; não há "vírus" para ser arrumado.