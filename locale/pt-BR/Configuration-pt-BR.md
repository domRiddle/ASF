# Configuração

Esta página é dedicada a configuração do ASF. Ela serve como uma documentação completa da pasta `config`, permitindo que você modifique o ASF conforme suas necessidades.

- **[Introdução](#introdução)**
- **[Gerador de configuração web](#gerador-de-configuração-web)**
- **[Configuração manual](#configuração-manual)**
- **[Configuração global](#configuração-global)**
- **[Configuração do Bot](#configuração-do-bot)**
- **[Estrutura de arquivos](#estrutura-de-arquivos)**
- **[Mapeamento JSON](#mapeamento-json)**
- **[Mapeamento de compatibilidade](#mapeamento-de-compatibilidade)**
- **[Configuração de compatibilidade](#configuração-de-compatibilidade)**
- **[Recarregamento automático](#recarregamento-automático)**

* * *

## Introdução

A configuração do ASF é dividida em duas partes principais - a configuração global (do processo) e a configuração individual dos bots. Cada bot tem seu próprio arquivo de configuração do chamado `BotName.json` (onde `BotName` é o nome dado ao bot), enquanto as configurações globais (do processo) do ASF estão em um único arquivo chamado `ASF.json`.

Chamamos de bot uma conta comum da Steam usada pelo ASF. Para trabalhar corretamente, o ASF precisa de pelo menos **uma** conta bot definida. Não há limite imposto pelo processo quanto a quantidade, então você pode usar quantos bots (contas Steam) você quiser.

O ASF usa o formato **[JSON](https://pt.wikipedia.org/wiki/JSON)** para armazenar seus arquivos de configuração. Ele é um formato amigável, legível e universal, no qual você pode configurar o programa. Porém não se preocupe, você não precisa saber JSON para configurar o ASF. Apenas mencionei isso para o caso de você desejar criar configurações em massa do ASF com algum tipo de script.

A configuração pode ser feita manualmente - criando configurações JSON adequadas, ou usando o nosso **[Gerador de configuração web](https://justarchinet.github.io/ASF-WebConfigGenerator)**, que é muito mais fácil e conveniente. A menos que você seja um usuário avançado, sugiro usar o gerador de configuração, que é descrito abaixo.

**[Voltar ao topo](#configuração)**

* * *

## Gerador de configuração web

A finalidade do gerador de configurações web é te proporcionar uma interface amigável para gerar os arquivos de configuração do ASF. O gerador de configuração Web funciona 100% no lado do cliente, o que significa que os detalhes que você está inserindo não são enviados para nenhum lugar, mas processados apenas na sua máquina. Isto garante segurança e confiabilidade, pois ele pode até mesmo trabalhar **[offline](https://github.com/JustArchiNET/ASF-WebConfigGenerator/tree/master/docs)**, basta baixar todos os arquivos e executar o `index.html` no seu navegador favorito.

O Gerador de configuração Web foi testado e roda perfeitamente no Chrome e no Firefox, e deve funcionar corretamente em todos os navegadores mais populares com javascript habilitado.

O uso é muito simples - selecione se você deseja gerar uma configuração `ASF` ou `Bot` alternando para a guia adequada, certifique-se de que a versão do arquivo de configuração corresponde a sua versão do ASF, então insira todos os dados e clique no botão "download". Mova este arquivo para a pasta `config` do ASF, substituindo os arquivos existentes, caso necessário. Repita esses passos para todas as eventuais modificações e recorra ao resto desta seção para uma explicação de todas as opções disponíveis de configuração.

**[Voltar ao início](#configuração)**

* * *

## Configuração manual

Eu recomendo fortemente usar o gerador de configuração web, mas se por algum motivo você não quiser, então você pode criar configurações adequadas manualmente. Confira o arquivo `example.json` para ter uma boa ideia da estrutura adequada, você pode copiar esse arquivo e usar como base os bots que vai configurar. Já que que você não estará usando nossa interface, certifique-se que sua configuração é **[válida](https://jsonlint.com)**, pois o ASF não carregá-la se ela não puder ser analisada. Para conhecer a estrutura JSON correta de todos os campos disponíveis consulte a seção de **[mapeamento JSON](#mapeamento-json)** e a documentação abaixo.

**[Voltar ao topo](#configuração)**

* * *

## Configuração global

A configuração global esta localizada no arquivo `ASF.json` e tem a seguinte estrutura:

```json
{
    "AutoRestart": true,
    "Blacklist": [],
    "CommandPrefix": "!",
    "ConfirmationsLimiterDelay": 10,
    "ConnectionTimeout": 60,
    "CurrentCulture": null,
    "Debug": false,
    "FarmingDelay": 15,
    "GiftsLimiterDelay": 1,
    "Headless": false,
    "IdleFarmingPeriod": 8,
    "InventoryLimiterDelay": 3,
    "IPC": false,
    "IPCPassword": null,
    "LoginLimiterDelay": 10,
    "MaxFarmingTime": 10,
    "MaxTradeHoldDuration": 15,
    "OptimizationMode": 0,
    "Statistics": true,
    "SteamMessagePrefix": "/me ",
    "SteamOwnerID": 0,
    "SteamProtocols": 7,
    "UpdateChannel": 1,
    "UpdatePeriod": 24,
    "WebLimiterDelay": 200,
    "WebProxy": null,
    "WebProxyPassword": null,
    "WebProxyUsername": null
}
```

**Dica:** A menos que você queira alterar alguma dessas opções você deve deixá-las com seus valores padrão, então você pode fechar o `ASF.json` e seguir para a configuração do bot.

* * *

Todas as opções são explicadas abaixo:

### `AutoRestart`

`bool` type with default value of `true`. Esta propriedade define se o ASF pode auto-reiniciar caso seja necessário. Existem alguns eventos que requerem a reinicialização do ASF, tal como a atualização (feita com o comando `UpdatePeriod` ou `update`), edições na configuração do `ASF.json`, e o comando `restart`. A reinicialização geralmente envolve duas partes - criar um novo processo e encerrar o atual. O valor padrão `true` deve ser bom para a maioria dos usuários, no entanto - se você estiver executando o ASF através de seu próprio código e/ou com o `dotnet`, você pode querer ter controle total sobre o início do processo, e evitar a possibilidade de ter um novo processo (reiniciado) do ASF sendo executado em segundo plano em algum lugar e não no código em primeiro plano, que foi encerrado junto com o antigo processo do ASF. Isto é especialmente importante considerando o fato de que o novo processo deixaria de ser descendente direto do seu script, o que te impossibilitaria de usar as suas entradas de console para ele.

Se for esse o caso, essa propriedade existe especialmente para você e você pode definí-la como `false`. Contudo, tenha em mente que, assim, **você** é o responsável por reiniciar o processo. É importante citar isso pois o ASF vai apenas fechar ao invés de criar um novo processo (por exemplo, após uma atualização), então se você não adicionou uma lógica para esse caso, ele vai simplesmente parar de funcionar até que você o inicie novamente. O ASF sempre fecha com um código de erro apropriado indicando sucesso (zero) ou falha (diferente de zero), assim você pode adicionar a lógica correta em seu código, que deve evitar o reinício automático do ASF em caso de falha ou ao menos fazer uma cópia local do `log.txt` para análise posterior. Também tenha em mente que o comando `restart` sempre vai reiniciar o ASF, independente de como essa propriedade está configurada, pois ela define o comportamento padrão, enquanto o comando `restart ` sempre reinicia o processo. A menos que você tenha um motivo para desativar esse recurso, você deve mantê-lo ativado.

* * *

### `Blacklist`

`ImmutableHashSet<uint>` type with default value of being empty. Como o nome sugere, esta propriedade de configuração global define appIDs (jogos) que serão totalmente ignorados pelo processo de coleta automático do ASF. Infelizmente, a Steam adora marcar as insígnias das promoções de verão/inverno como "disponíveis para recebimento de cartas", o que confunde o processo do ASF, fazendo-o acreditar que é um jogo válido que deve ser coletado. Se não houvesse nenhum tipo de lista negra, o ASF acabaria por "travar" na coleta de um jogo que na verdade não é um jogo, e esperaria infinitamente pelo recebimento de cartas que nunca viriam. A lista negra do ASF serve para marcar essas insígnias como não disponíveis para a coleta, de modo que podemos silenciosamente ignorá-las e assim não cair na armadilha.

O ASF inclui duas listas negras por padrão - `GlobalBlacklist`, que está codificada dentro do código do ASF e não pode ser editada, e a `Blacklist` normal, que é definida aqui. A `GlobalBlacklist` é atualizada juntamente com o ASF e normalmente inclui todos os appIDs "ruins" no momento do lançamento, então se você estiver usando a última versão você não precisa manter a sua `Blacklist` própria, descrita aqui. O principal objetivo desta propriedade é permitir que você adicione novos appIDs, ainda não conhecidos no momento do lançamento de uma nova versão do ASF, que não devam ser coletados. A `GlobalBlacklist` é atualizada sempre o mais rápido possível, portanto não há necessidade de atualizar a sua própria `Blacklist` se você estiver usando a versão mais recente do ASF, mas sem a `Blacklist` você seria forçado a atualizar o ASF para que ele "continuasse rodando" sempre que a Valve liberasse novas insígnias de promoções - não quero forçá-lo a usar o código mais recente do ASF, portanto ela existe para que você "conserte" o ASF por sua conta se por algum motivo você não quer ou não pode atualizar a `GlobalBlacklist` disponível em uma nova versão do ASF, mais ainda quer manter seu velho ASF funcionando. A menos que você tenha uma razão muito **forte** para editar essa propriedade, você deve mantê-la padrão.

Se você está procurando uma lista negra personalizada para cada bot, dê uma olhada nos **[comandos](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-pt-BR)** `ib`, `ibadd` e `ibrm`.

* * *

### `CommandPrefix`

`string` type with default value of `!`. Esta propriedade define o prefixo, **que diferencia maiúsculas de minúsculas**, usado para os **[comandos](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-pt-BR)** do ASF. Em outras palavras, é isso que você precisa digitar antes de cada comando do ASF para fazer com que ele te ouça. Esse parâmetro pode receber o valor `null` ou uma string vazia para forçar o ASF a não usar o prefixo, neste caso os comandos são inseridos simplesmente por seus identificadores. No entanto, fazer isso provavelmente vai diminuir o desempenho do ASF já que ele é otimizado para não analisar mensagens que não forem iniciadas com o `CommandPrefix` - se você decidir não usar prefixos, o ASF será forçado a ler todas as mensagens e respondê-las, mesmo se elas não forem comandos. Portanto, é recomendável usar algum `CommandPrefix`, como `/` se você não gostar do valor padrão `!`. Para obter consistência, `CommandPrefix` afeta todo o processo do ASF. A menos que você tenha uma razão para editar essa propriedade, você deve mantê-la padrão.

* * *

### `ConfirmationsLimiterDelay`

`byte` type with default value of `10`. O ASF vai garantir que haverá pelo menos o número de segundos de atraso definido em `ConfirmationsLimiterDelay` entre duas solicitações consecutivas de confirmações 2FA para evitar ativar o limitador de tráfego - aquelas que são usadas pelo **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-pt-BR)** durante, por exemplo, o comando `2faok`, bem como em várias operações relacionadas a troca. O valor padrão foi definido com base em nossos testes e não deve ser diminuído se você não quiser ter problemas. A menos que você tenha uma razão muito **forte** para editar essa propriedade, você deve mantê-la padrão.

* * *

### `ConnectionTimeout`

`byte` type with default value of `60`. Essa propriedade define o tempo limite em segundos para várias ações de rede feitas pelo ASF. Em particular, `ConnectionTimeout` define o tempo limite em segundos para solicitações HTTP e IPC, `ConnectionTimeout / 10` define o número máximo de pulsos perdidos, enquanto `ConnectionTimeout / 30` define o número de minutos que permitimos para a solicitação inicial de conexão da rede Steam. O valor padrão de `60` deve ser adequado para a maioria dos usuários, no entanto, se você tem uma conexão de rede ou PC muito lento, convém aumentar esse número um pouco (uns `90`, por exemplo). Tenha em mente que valores maiores não vão magicamente resolver lentidões ou mesmo alcançar servidores Steam inacessíveis, então nós não devemos esperar infinitamente por algo que não vai acontecer, devemos simplesmente tentar novamente mais tarde. Definir um valor muito alto vai resultar em um atraso excessivo na captura de problemas de rede, bem como na diminuição do desempenho global. Definir um valor muito baixo vai diminuir a estabilidade total e o desempenho, uma vez que o ASF abortará solicitações válidas que ainda estejam sendo analisadas. Portanto, definir esse valor mais baixo do que o padrão não tem nenhuma vantagem em geral, uma vez que os servidores Steam tendem a ser lentos de vez em quando e podem exigir mais tempo para analisar pedidos do ASF. O valor padrão é um equilíbrio entre acreditar que nossa conexão de rede é estável e duvidar que a rede Steam vai manipular nosso pedido em determinado limite de tempo. Se você deseja detectar problemas de rede o mais rápido possível e fazer o ASF reconectar/responder mais rápido, o valor padrão deve bastar (você pode diminuir um pouco para que o ASF se torne menos paciente). Se você notar que o ASF está tendo problemas de rede, tais como falhas em solicitações, pulsos perdidos ou conexões com a rede Steam interrompidas, pode valer a pena aumentar esse valor se tiver certeza que o problema **não** é causado pela sua rede, mas pela própria Steam, uma vez que aumentar o tempo de espera torna o ASF mais "paciente" e não decidido a se reconectar imediatamente. Também pode fazer sentido aumentar esse valor se você tem uma conexão um tanto lenta que requer mais tempo para a transmissão dos dados. Em resumo, o valor padrão é suficiente para a maioria dos casos, mas você pode querer aumentá-lo se necessário. Ainda, indo muito acima do valor padrão não faz muito sentido, uma vez que limites de tempo maiores não consertam magicamente servidores Steam inacessíveis. A menos que você tenha uma razão para editar essa propriedade, você deve mantê-la padrão.

* * *

### `CurrentCulture`

`string` type with default value of `null`. Por padrão o ASF tenta usar o idioma do seu sistema operacional e vai preferir usar linhas traduzidas para esse idioma se disponível. Isto é possível graças à nossa comunidade que tenta **[localizar](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Localization)** o ASF para quase todos os idiomas mais populares. Se por algum motivo você não quiser usar o idioma nativo do seu SO, você pode usar esta propriedade de configuração para escolher qualquer outro idioma válido. Para obter uma lista de todas os idiomas disponíveis, visite o **[MSDN](https://msdn.microsoft.com/en-us/library/cc233982.aspx)** e procure por `Language tag`. É bom notar que o ASF aceita tanto instruções de idioma por região, como `en-GB`, quanto o neutros, como `en`. Especificar o idioma atual também pode afetar outros comportamentos regionais específicos, como formato de moeda/data e outros. Por favor, note que você pode precisar de pacotes de idioma/fontes adicionais para exibir caracteres específicos do idioma corretamente, caso você tenha escolhido um idioma não-nativo que faça uso deles. Normalmente você fará uso desta propriedade se preferir o ASF em inglês ao invés da sua língua nativa.

* * *

### `Debug`

`bool` type with default value of `false`. Esta propriedade define se o processo deve ser executado em modo de depuração. Quando em modo de depuração, o ASF cria uma pasta especial chamada `debug` ao lado da pasta `config`, que mantém o controle de toda a comunicação entre o ASF e os servidores Steam. Informações de depuração podem ajudar a encontrar comportamentos estranhos na rede e no ASF em geral. Além disso, algumas rotinas gerarão muito mais informações, por exemplo, o `WebBrowser` indicará um motivo específico pela falha nas solicitações- essas informações são gravadas no log usual do ASF. **Você não deve executar o ASF no modo de depuração, a menos que seja solicitado pelo desenvolvedor**. Executar o ASF nesse modo **diminui o desempenho**, **afeta negativamente a estabilidade** e **gera muito mais informações em alguns lugares**, portanto ele deve ser usado **apenas** intencionalmente em um espaço curto de tempo para depurar uma questão particular reproduzindo o problema ou obter mais informações sobre uma falha de solicitação, e assim por diante, mas **não** para execução normal do programa. Você verá **um monte** de erros novos, problemas, e exceções - certifique-se de que você tenha um conhecimento razoável sobre o ASF, o Steam e suas peculiaridades caso você decida analisar tudo isso por conta própria, uma vez que nem tudo é relevante.

**AVISO:** esse modo registra informações **potencialmente confidenciais** como logins e senhas que você usa para acessar o Steam (para se registrar na rede). Os dados são gravados tanto na pasta `depuração` quanto no `log.txt` padrão (que agora contém muito mais detalhes para registrar esta informação). Você não deve postar o conteúdo de depuração gerado pelo ASF em um local público, o desenvolvedor do ASF deverá sempre pedir para que você o envie por e-mail, ou outro local seguro. Não armazenamos nem fazemos uso desses detalhes confidenciais, eles são escritos como parte das rotinas de depuração, já que sua presença pode ser relevante para o problema que está te afetando. Nós preferimos que se você não altere a forma com que o ASF faz os registros, mas se você estiver preocupado você pode editar da forma correta esses detalhes confidenciais.

> Essa edição significa substituir detalhes confidenciais por asteriscos, por exemplo. Você não deve remover inteiramente as linhas confidenciais, já que sua existência pode ser relevante e deve ser preservada.

* * *

### `FarmingDelay`

`byte` type with default value of `15`. Para que o ASF funcione, é necessário verificar periodicamente num intervalo de minutos informado em `FarmingDelay`, para ver se todas as cartas já foram obtidas. Um valor muito baixo vai fazer com que uma quantidade excessiva de solicitações sejam enviadas para o Steam, enquanto um valor muito alto fará com que o ASF continue rodando o jogo até terminar o tempo do `FarmingDelay`, mesmo que ele já tenha sido totalmente explorado. O valor padrão deve ser excelente para a maioria dos usuários, mas se você tem muitos bots em execução, você pode considerar aumentá-lo para algo em torno de `30` minutos, a fim de limitar a quantidade de pedidos enviados para o Steam. Felizmente o ASF usa um manipulador de eventos e verifica a página de insígnia do jogo sempre que há notificação de item recebido, então em geral **não precisamos verificar isso em intervalos regulares**, mas como não confiamos plenamente na rede Steam - ainda checamos a página de insígnia do jogo caso não tenhamos verificado através das notificações de recebimento no intervalo de tempo do `FarmingDelay` (para p caso da rede Steam não nos informar sobre o item recebido ou coisas assim). Assumindo que a rede Steam esteja funcionando corretamente, diminuir este valor **não vai melhorar a eficiência da coleta**, mas **aumentará significativamente a sobrecarga de rede** - é recomendado aumentá-lo apenas (se necessário) em padrões de `15` minutos. A menos que você tenha uma razão muito **forte** para editar essa propriedade, você deve mantê-la padrão.

* * *

### `GiftsLimiterDelay`

`byte` type with default value of `1`. O ASF vai garantir que haverá um atraso em segundos, definido em `ConfirmationsLimiterDelay`, entre duas solicitações consecutivas de processamento (resgate) de presentes/chaves/licenças para evitar ativar o limitador de tráfego. Além disso, ele também será usado como limitador global para solicitações de listagem de jogos, tal qual a emitida pelo **[comando](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-pt-BR)** `owns`. A menos que você tenha uma razão muito **forte** para editar essa propriedade, você deve mantê-la padrão.

* * *

### `Headless`

`bool` type with default value of `false`. Esta propriedade define se o processo deve ser executado em modo não-interativo. Quando estiver nesse modo, o ASF pressupõe que está sendo executado em um servidor ou em outro ambiente não-interativo, portanto ele não tentará ler credenciais de conta, como o código de autenticação, o código SteamGuard, a senha ou qualquer outra variável necessária para o funcionamento do ASF. Isso é o mesmo que converter o console do ASF para o modo somente leitura. O modo `Headless` (não-interativo) é útil principalmente para usuários que executam o ASF em servidores, já que em vez de pedir, por exemplo, um código de autenticação, o ASF simplesmente abortará a operação parando a conta em questão. A menos que você esteja executando o ASF em um servidor e tenha confirmado anteriormente que o ASF é capaz de operar em modo não-interativo, você deve manter essa propriedade desabilitada. Qualquer interação do usuário será negada quando estiver nesse modo e suas contas não serão executadas se elas exigem **qualquer** entrada no console durante a inicialização. Ele é útil para servidores, já que o ASF vai abortar ao tentar se conectar a conta quando forem necessárias credenciais, em vez de esperar (infinitamente) para que o usuário os forneça. Ativando esse modo também permitirá que você use o **[comando](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-pt-BR)** `input` que funciona como um substituto para a entrada padrão do console. Se você não tem certeza de como definir essa propriedade, deixe-a com o valor padrão `false`.

Se você estiver executando o ASF no servidor, convém usar essa opção em conjunto com o **[argumento de linha de comando](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments-pt-BR)** `--process-required`.

* * *

### `IdleFarmingPeriod`

`byte` type with default value of `8`. Quanto o ASF não tem o que coletar, ele vai checar periodicamente a cada hora informada em `FarmingDelay`, para ver se todas as cartas já foram obtidas. Esse recurso não é necessário se tratando de jogos novos, já que neste caso o ASF verifica automaticamente a página de insígnias. O `IdleFarmingPeriod` existe, principalmente, para situações onde jogos antigos que já possuímos passem a ter cartas. Neste caso não há evento algum, por isso o ASF deve verificar periodicamente a página de insígnias para mantermos tudo sob controle. O valor `0` desativa este recurso. Veja também: `ShutdownOnFarmingFinished`.

* * *

### `InventoryLimiterDelay`

`byte` type with default value of `3`. O ASF vai garantir que haverá um atraso em segundos, definido em `InventoryLimiterDelay`, entre duas solicitações consecutivas do inventário para evitar ativar o limitador de tráfego - elas são usadas para obter uma lista dos itens em seu inventário (e apenas para isso). O valor padrão `3` foi definido com base na coleta de mais de 100 contas bot, e deve satisfazer a maioria dos usuários (se não todos). No entanto você pode querer reduzi-lo, ou até mesmo mudar para `0` se você tiver uma quantidade muito pequena de bots, então o ASF irá ignorar o atraso e listar os inventários Steam muito mais rápido. Porém fique ciente que definir um valor muito baixo **vai** fazer o Steam bloquear temporariamente seu IP, o que vai te impedir de acessar seu inventário. Você talvez precise aumentar esse valor se estiver executando muitos bots com muitos pedidos de acesso ao inventário, embora neste caso você deva provavelmente tentar limitar o número dessas solicitações em vez disso. A menos que você tenha uma razão muito **forte** para editar essa propriedade, você deve mantê-la padrão.

* * *

### `IPC`

`bool` type with default value of `false`. Esta propriedade define se o servidor **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** do ASF deve iniciar juntamente com o processo. IPC permite a comunicação entre processos inicializando um servidor HTTP local. Se você não vai fazer uso do servidor IPC do ASF, então não há razão para habilitar esta opção.

* * *

### `IPCPassword`

`string` type with default value of `null`. Esta propriedade define a senha obrigatória para cada chamada feita através do IPC e serve como uma medida de segurança extra. Quando definido como valor não vazio, todas solicitações através do IPC exigirão uma propriedade adicional `password` com o valor definido aqui. O valor padrão `null` elimina necessidade de senha, fazendo com que o ASF processe todas as consultas. Além disso, habilitar essa opção também habilita o mecanismo interno de prevenção de força bruta IPC, que bloqueia temporariamente o `IPAddress` que enviar muitas solicitações não autorizadas em um curto espaço de tempo. A menos que você tenha uma razão para editar essa propriedade, você deve mantê-la padrão.

* * *

### `LoginLimiterDelay`

`byte` type with default value of `10`. O ASF vai garantir que haverá um atraso em segundos, definido em `LoginLimiterDelay`, entre duas solicitações consecutivas de conexão para evitar ativar o limitador de tráfego. O valor padrão `10` foi definido com base na conexão de mais de 100 contas bot, e deve satisfazer a maioria dos usuários (se não todos). No entanto você pode querer reduzir/aumentar o valor, ou até mesmo mudar para `0` se você tiver uma quantidade muito pequena de bots, então o ASF vai ignorar o atraso e conectar com o Steam muito mais rápido. Esteja avisado, porém, que definir um valor muito baixo tendo muitos bots **vai** fazer o Steam bloquear temporariamente seu IP, e isso vai te impedir **completamente** de se conectar, retornando o erro `InvalidPassword/RateLimitExceeded` - e isso se aplica ao cliente Steam comum, não apenas ao ASF. Da mesma forma, se você estiver rodando um numero excessivo de bots, especialmente junto com outros clientes/ferramentas Steam usando o mesmo endereço IP, provavelmente você precisará aumentar esse valor para espalhar os logins em um período longo de tempo.

Como nota, esse valor também é utilizado como um balanceamento de carga em todas as ações regulares do ASF, tais como trocas em `SendTradePeriod`. A menos que você tenha uma razão muito **forte** para editar essa propriedade, você deve mantê-la padrão.

* * *

### `MaxFarmingTime`

`byte` type with default value of `10`. Como você deve saber, o Steam nem sempre funciona corretamente, às vezes situações estranhas podem acontecer como o Steam não gravar nosso tempo de jogo apesar de estarmos jogando. O ASF vai rodar um jogo no modo solo pelo tempo máximo, em horas, determinado em `MaxFarmingTime`, e vai considerá-lo totalmente explorado após esse período. Isso é necessário para que o processo de coleta não trave caso aconteça algo estranho, e também para o caso do Steam, por algum motivo, lançar uma nova insignia que poderia parar o progresso do ASF (veja: `Blacklist`). O valor padrão de `10` horas deve ser suficiente para adquirir todas as cartas Steam de um jogo. Definir um valor muito baixo para essa propriedade pode resultar em jogos válidos sendo ignorados (e sim, há jogos válidos que levam até 9 horas para coleta), enquanto definir um valor muito alto pode resultar no congelamento do processo de coleta. Por favor, note que esta propriedade afeta somente um único jogo em uma sessão de coleta simples (por isso, após passar por toda a fila, o ASF retornará a esse título), ela também não é baseada no total de tempo jogado, mas no tempo de coleta interno do ASF. A menos que você tenha uma razão muito **forte** para editar essa propriedade, você deve mantê-la padrão.

* * *

### `MaxTradeHoldDuration`

`byte` type with default value of `15`. Esta propriedade define a duração máxima da retenção de trocas, em dias, para analisarmos a disposição em aceitar - o ASF rejeitará trocas que estejam sendo retidas por mais dias que o definido em `MaxTradeHoldDuration`. Esta opção só faz sentido para bots com `SteamTradeMatcher` ativado em `TradingPreferences`, já que isso não afeta as trocas entre `Master`/`SteamOwnerID`, nem doações. Trocas retidas são irritantes para todos, e ninguém quer lidar com elas. Como supõe-se que o ASF funcione com regras liberais e ajude a todos, independentemente de trocas retidas ou não - essa opção é definida como `15` por padrão. No entanto, se você preferir rejeitar todas as transações afetadas pela retenção de trocas, você pode definir `0` aqui. Por favor, considere o fato de que cartas com vida curta não são afetadas por essa opção e são automaticamente rejeitadas para pessoas com retenção de trocas, conforme descrito na seção **[trocas](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading)**, então não há nenhuma necessidade de rejeitar globalmente todo mundo só por causa disso. A menos que você tenha uma razão para editar essa propriedade, você deve mantê-la padrão.

* * *

### `OptimizationMode`

`byte` type with default value of `0`. Esta propriedade define o modo de otimização que o ASF usará durante o tempo de execução. Atualmente, o ASF suporta dois modos - `0`, que é chamado de `MaxPerformance` e `1`, que é chamado `MinMemoryUsage`. Por padrão, o ASF executa tantas coisas em paralelo (simultaneamente) quanto possível, melhorando o desempenho pelo balanceamento de carga de trabalho entre todos os núcleos de CPU, vários threads de CPU, soquetes múltiplos e várias tarefas de pool de threads. Por exemplo, o ASF vai acessar a primeira página de insígnias quando procurar jogos para executar, e então uma vez que o pedido chegar, o ASF vai ler quantas páginas de insígnias você tem, e em seguida, solicitar todas as outras simultaneamente. Isto é o que você deve querer **quase sempre**, já que a sobrecarga, na maioria dos casos, é mínima e os benefícios do código assíncrono do ASF pode ser visto até mesmo em hardwares mais antigos com um único núcleo de CPU e potência fortemente limitada. No entanto, com muitas tarefas sendo processadas em paralelo, o tempo de execução do ASF é responsável pela manutenção, por exemplo, mantendo soquetes abertos, threads ativos e tarefas processando, que pode resultar no aumento do uso de memória de vez em quando, e se você tem uma memória extremamente limitada, você pode querer mudar esta propriedade para `1` (`MinMemoryUsage`) a fim de forçar o ASF a usar o mínimo de tarefas possível e normalmente executar possíveis códigos assíncronos paralelos de forma síncrona. Você deve considerar mudar essa propriedade somente se você leu anteriormente a **[configuração de pouca memória](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup)** e intencionalmente quer sacrificar um aumento enorme de desempenho, para uma diminuição de sobrecarga de memória muito pequena. Normalmente, esta opção é **muito pior** do que o que se pode conseguir com outras possibilidades, tais como limitando o uso do ASF ou ajustando o coletor de lixo do tempo de execução, conforme explicado na **[configuração de pouca memória](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup)**. Portanto, você deve usar `MinMemoryUsage` como **último recurso**, antes de recompilar o tempo de execução, caso você não tenha conseguido atingir resultados satisfatórios com outras opções (muito melhores). A menos que você tenha uma razão muito **forte** para editar essa propriedade, você deve mantê-la padrão.

* * *

### `Estatísticas`

`bool` type with default value of `true`. Esta propriedade define se as estatísticas do ASF serão habilitadas. Uma explicação detalhada do que exatamente essa opção faz está disponível na seção de **[estatísticas](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Statistics)**. A menos que você tenha uma razão para editar essa propriedade, você deve mantê-la padrão.

* * *

### `SteamMessagePrefix`

`string` type with default value of `"/me "`. Esta propriedade define um prefixo que será anexado a todas as mensagens da Steam enviadas pelo ASF. Por padrão o ASF usa o prefixo `"/me"` para distinguir as mensagens do bot facilmente, mostrando-as em cores diferentes no chat do Steam. Outro prefixo digno de menção é `"/pre"` que tem resultados semelhantes, mas com uma formatação diferente. Você também pode definir essa propriedade como vazia ou `null` para desativar inteiramente o uso do prefixo e deixar todas as mensagens do ASF de forma tradicional. É bom notar que esta propriedade afeta apenas mensagens do Steam - as respostas retornadas através de outros canais (como IPC) não são afetadas. A menos que você deseje personalizar o comportamento padrão do ASF, é uma boa ideia deixá-lo assim.

* * *

### `SteamOwnerID`

`ulong` type with default value of `0`. Esta propriedade define o SteamID em formato de 64-bit do proprietário do processo ASF, e é muito semelhante a permissão `Master` nas configurações específicas do bot (mas global mas em vez disso). Você vai querer definir quase sempre essa propriedade com o ID da sua conta Steam principal. A permissão `Master` inclui controle total sobre sua conta bot, mas comandos globais como `exit`, `restart` ou `update` são reservados apenas para o `SteamOwnerID`. Isto é conveniente, pois você pode querer executar bots para seus amigos, mas não lhes permitir controle do processo ASF, como sair dele através do comando `exit` por exemplo. O valor padrão `0` especifica que não há proprietário do processo ASF, o que significa que ninguém será capaz de emitir comandos globais nele. Tenha em mente que o **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** trabalha com `SteamOwnerID`, então se você quiser usá-lo você deve fornecer um valor válido aqui.

* * *

### `SteamProtocols`

`byte flags` type with default value of `7`. Esta propriedade define os protocolos Steam que o ASF irá usar ao se conectar aos servidores Steam, que são definidos abaixo:

| Valor | Nome      | Descrição                                                                                        |
| ----- | --------- | ------------------------------------------------------------------------------------------------ |
| 0     | None      | Sem protocolo                                                                                    |
| 1     | TCP       | **[Transmission Control Protocol](https://pt.wikipedia.org/wiki/Transmission_Control_Protocol)** |
| 2     | UDP       | **[User Datagram Protocol](https://pt.wikipedia.org/wiki/User_Datagram_Protocol)**               |
| 4     | WebSocket | **[WebSocket](https://pt.wikipedia.org/wiki/WebSocket)**                                         |

Por favor note que esta propriedade é um campo do tipo `flags`, portanto é possível escolher qualquer combinação de valores disponíveis. Confira **[mapeamento flags](#mapeamento-json)** se você quiser aprender mais. Não habilitar nenhum flag resulta na opção `None`, e essa opção, por si só, é inválida.

Por padrão ASF deve usar todos os protocolos Steam disponíveis como uma medida para lutar contra tempos de inatividade e outras questões semelhantes da Steam. Normalmente você vai querer alterar essa propriedade se desejar limitar o ASF para usar apenas um ou dois protocolos específicos, em vez de todos os que estão disponíveis. Tal medida seria necessária, por exemplo, se seu firewall permitir apenas tráfego TCP e você não quiser que o ASF tente conectar via UDP. No entanto, a menos que você esteja depurando um problema específico, você quase sempre vai desejar garantir que o ASF esteja livre para usar qualquer protocolo que seja suportado atualmente e não apenas um ou dois. A menos que você tenha uma razão muito **forte** para editar essa propriedade, você deve mantê-la padrão.

* * *

### `UpdateChannel`

`byte` type with default value of `1`. This property defines update channel which is being used, either for auto-updates (if `UpdatePeriod` is greater than `0`), or update notifications (otherwise). Currently ASF supports three update channels - `0` which is called `None`, `1`, which is called `Stable`, and `2`, which is called `Experimental`. `Stable` channel is the default release channel, which should be used by majority of users. `Experimental` channel in addition to `Stable` releases, also includes **pre-releases** dedicated for advanced users and other developers in order to test new features, confirm bugfixes or give feedback about planned enhancements. **Experimental versions often contain unpatched bugs, work-in-progress features or rewritten implementations**. If you don't consider yourself advanced user, please stay with default `1` (Stable) update channel. `Experimental` channel is dedicated to users who know how to report bugs, deal with issues and give feedback - no technical support will be given. Check out ASF **[release cycle](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle)** if you'd like to learn more. You can also set `UpdateChannel` to `0` (`None`), if you want to completely remove all version checks. Setting `UpdateChannel` to `0` will entirely disable entire functionality related to updates, including `update` command. Using `None` channel is **strongly discouraged** due to exposing yourself to all sort of problems (mentioned in `UpdatePeriod` description below).

**Unless you know what you're doing**, we **strongly** recommend to keep it at default.

* * *

### `UpdatePeriod`

`byte` type with default value of `24`. This property defines how often ASF should check for auto-updates. Updates are crucial not only to receive new features and critical security patches, but also to receive bugfixes, performance enhancements, stability improvements and more. When a value greater than `0` is set, ASF will automatically download, replace, and restart itself (if `AutoRestart` permits) when new update is available. In order to achieve this, ASF will check every `UpdatePeriod` hours if new update is available on our GitHub repo. A value of `0` disables auto-updates, but still allows you to execute `update` command manually. You might also be interested in setting appropriate `UpdateChannel` that `UpdatePeriod` should follow.

Update process of ASF involves update of entire folder structure that ASF is using, but without touching your own configs or databases located in `config` directory - this means that any extra files unrelated to ASF in its directory might be lost during update. Default value of `24` is a good balance between unnecessary checks, and ASF that is fresh enough.

Unless you have a **strong** reason to disable this feature, you should keep auto-updates enabled within reasonable `UpdatePeriod` **for your own good**. This is not only because we don't support anything but latest stable ASF release, but also because **we give our security guarantee only for latest version**. If you're using outdated ASF version then you're intentionally exposing yourself to all kind of issues, from small bugs, through broken functionality, ending with **[permanent Steam account suspensions](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ#did-somebody-get-banned-for-it)**, so we **strongly recommend**, for your own good, to always ensure that your ASF version is up to date. Auto-updates allow us to react quickly to all kind of issues by disabling or patching problematic code before it can escalate - if you opt out of it, you lose all of our security guarantees and risk consequences from running code that could be potentially harmful, not only to Steam network, but also (by definition) to your own Steam account.

* * *

### `WebLimiterDelay`

`ushort` type with default value of `200`. This property defines, in miliseconds, the minimum amount of delay between sending two consecutive requests to the same Steam web-service. Such delay is required as **[AkamaiGhost](https://www.akamai.com)** service that Steam uses internally includes rate-limiting based on global number of requests sent across given time period. In normal circumstances akamai block is rather hard to achieve, but under very heavy workloads with a huge ongoing queue of requests, it's possible to trigger it if we keep sending too many requests across too short time period.

Default value was set based on assumption that ASF is the only tool accessing Steam web-services, in particular `steamcommunity.com`, `api.steampowered.com` and `store.steampowered.com`. If you're using other tools sending requests to the same web-services then you should make sure that your tool includes similar functionality of `WebLimiterDelay` and set both to double of default value, which would be `400`. This guarantees that under no circumstance you'll exceed sending more than 1 request per 200 ms.

In general, lowering `WebLimiterDelay` under default value is **strongly discouraged** as it might lead to various IP-related blocks, some of which are possible to be permanent. Default value is good enough for running a single ASF instance on the server, as well as using ASF in normal scenario along with original Steam client. It should be correct for majority of usages, and you should only increase it (never lower it), if - apart from using ASF, you're also using another tool that might send excessive number of requests to the same web-services that ASF is making use of. In short, global number of all requests sent from a single IP to a single Steam domain should never exceed more than 1 request per 200 ms.

A menos que você tenha uma razão para editar essa propriedade, você deve mantê-la padrão.

* * *

### `WebProxy`

`string` type with default value of `null`. This property defines a web proxy address that will be used for all internal http and https requests sent by ASF's `HttpClient`, especially to services such as `github.com`, `steamcommunity.com` and `store.steampowered.com`. Proxying ASF requests in general has no advantages, but it's exceptionally useful for bypassing various kinds of firewalls, especially the great firewall in China.

This property is defined as uri string:

> Uma cadeia de caracteres URI é composta de um esquema (http ou https), um host, e uma porta opcional. Um exemplo de uri completa é `http://www.contoso.com:8080`.

If your proxy requires user authentication, you will also need to set up `WebProxyUsername` and/or `WebProxyPassword`. If there is no such need, setting up this property alone is sufficient.

Right now ASF uses web proxy only for `http` and `https` requests, which **do not** include internal Steam network communication done within ASF's internal Steam client. There are currently no plans for supporting that, mainly due to missing **[SK2](https://github.com/SteamRE/SteamKit)** functionality. If you need/want it to happen, I'd suggest starting from there.

A menos que você tenha uma razão para editar essa propriedade, você deve mantê-la padrão.

* * *

### `WebProxyPassword`

`string` type with default value of `null`. This property defines password field used in basic, digest, NTLM, and Kerberos authentication that is supported by a target `WebProxy` machine providing proxy functionality. If your proxy doesn't require user credentials, there is no need for you to input anything here. Using this option makes sense only if `WebProxy` is used as well, as it has no effect otherwise.

A menos que você tenha uma razão para editar essa propriedade, você deve mantê-la padrão.

* * *

### `WebProxyUsername`

`string` type with default value of `null`. This property defines username field used in basic, digest, NTLM, and Kerberos authentication that is supported by a target `WebProxy` machine providing proxy functionality. If your proxy doesn't require user credentials, there is no need for you to input anything here. Using this option makes sense only if `WebProxy` is used as well, as it has no effect otherwise.

A menos que você tenha uma razão para editar essa propriedade, você deve mantê-la padrão.

* * *

**[Voltar ao topo](#configuração)**

* * *

## Configuração do Bot

As you should know already, every bot should have its own config. Example bot config is included in `example.json` file, which should be used for bot configuration. Simply **copy paste** `example.json` to a new file, and remember to name it appropriately, as it will be your bot instance. You should start from configuring your **primary** account, so some good suggestions for filename is `primary.json`, `1.json` or `YourNickname.json`.

**Notice:** There are several names which are reserved and can't be used for bot configs. Those are: **ASF**, **example** and **minimal**. ASF will ignore such configuration files. ASF will also ignore configuration files starting with a dot.

After deciding how you want to name your bot, open its file, and start with configuration. You should notice following structure:

```json
{
    "AcceptGifts": false,
    "AutoSteamSaleEvent": false,
    "BotBehaviour": 0,
    "CustomGamePlayedWhileFarming": null,
    "CustomGamePlayedWhileIdle": null,
    "Enabled": false,
    "FarmingOrders": [],
    "GamesPlayedWhileIdle": [],
    "HoursUntilCardDrops": 3,
    "IdlePriorityQueueOnly": false,
    "IdleRefundableGames": true,
    "LootableTypes": [1, 3, 5],
    "MatchableTypes": [5],
    "OnlineStatus": 1,
    "PasswordFormat": 0,
    "Paused": false,
    "RedeemingPreferences": 0,
    "SendOnFarmingFinished": false,
    "SendTradePeriod": 0,
    "ShutdownOnFarmingFinished": false,
    "SteamLogin": null,
    "SteamMasterClanID": 0,
    "SteamParentalCode": null,
    "SteamPassword": null,
    "SteamTradeToken": null,
    "SteamUserPermissions": {},
    "TradingPreferences": 0,
    "TransferableTypes": [1, 3, 5],
    "UseLoginKeys": true
}
```

**Tip:** In order for bot to work properly, you should edit at least `Enabled`, `SteamLogin` and `SteamPassword` properties. I also suggest to take a look at some fine-tuning such as `HoursUntilCardDrops`, but all of that is optional. ASF configs are quite advanced to allow you tune your bots and ASF however you want, if you don't "require" such advanced setup, you don't really have to go deep into each config property. It's up to you how simple or how complex ASF should be.

* * *

Todas as opções são explicadas abaixo:

### `AcceptGifts`

`bool` type with default value of `false`. When enabled, ASF will automatically accept and redeem all steam gifts (including wallet gift cards) sent to the bot. This includes also gifts sent from users other than those defined in `SteamUserPermissions`. Keep in mind that gifts sent to e-mail address are not directly forwarded to the client, so ASF won't accept those without your help.

This option is recommended only for alt accounts, as it's very likely that you don't want to automatically redeem all gifts sent to your primary account. If you're unsure whether you want this feature enabled or not, keep it with default value of `false`.

* * *

### `AutoSteamSaleEvent`

`bool` type with default value of `false`. During Steam summer/winter sale events Steam is known for providing you extra cards for browsing discovery queue each day, as well as voting in the Steam awards. When this option is enabled, ASF will automatically check Steam discovery queue and Steam awards each 6 hours, and clear them if needed. This option is not recommended if you want to do those actions yourself, and typically it should make sense only on bot accounts. Moreover, you need to ensure that your account is at least of level `8` if you expect to receive those cards in the first place. If you're unsure whether you want this feature enabled or not, keep it with default value of `false`.

Please note that due to constant Valve issues, changes and problems, **we give no guarantee whether this function will work properly**, therefore it's entirely possible that this option **will not work at all**. We do not accept **any** bug reports, neither support requests for this option. It's offered with absolutely no guarantees, you're using it at your own risk.

* * *

### `BotBehaviour`

`byte flags` type with default value of `0`. This property defines ASF bot-like behaviour during various events, and is defined as below:

| Valor | Nome                          | Descrição                                                                         |
| ----- | ----------------------------- | --------------------------------------------------------------------------------- |
| 0     | None                          | Nenhum comportamento especial do bot, o modo menos invasivo, padrão               |
| 1     | RejectInvalidFriendInvites    | Fará com que o ASF rejeite (ao invés de ignorar) convites de amizade inválidos    |
| 2     | RejectInvalidTrades           | Fará com que o ASF rejeite (ao invés de ignorar) ofertas de troca inválidas       |
| 4     | RejectInvalidGroupInvites     | Fará com que o ASF rejeite (ao invés de ignorar) convites inválidos para grupos   |
| 8     | DismissInventoryNotifications | Fará com que o ASF dispense automaticamente todas as notificações de inventário   |
| 16    | MarkReceivedMessagesAsRead    | Fará com que o ASF marque automaticamente todas as mensagens recebidas como lidas |

Por favor note que esta propriedade é um campo do tipo `flags`, portanto é possível escolher qualquer combinação de valores disponíveis. Confira **[mapeamento flags](#mapeamento-json)** se você quiser aprender mais. Not enabling any of flags results in `None` option.

In general you want to modify this property if you expect from ASF to do certain amount of automation related to its activity, as it'd be expected from a bot account, but not a primary account used in ASF. Therefore, changing this property makes sense mainly for alt accounts, although you're free to use selected options for main accounts as well.

Normal (`None`) ASF behaviour is to only automate things that user wants (e.g. cards farming or `SteamTradeMatcher` offers, if set in `TradingPreferences`). This is the least invasive mode, and it's beneficial to majority of users since you remain in full control over your account and you can decide yourself whether to allow certain out-of-scope interactions, or not.

Invalid friend invite is an invite that doesn't come from user with `FamilySharing` permission (defined in `SteamUserPermissions`) or above. ASF in normal mode ignores those invites, as you'd expect, giving you free choice whether to accept them, or not. `RejectInvalidFriendInvites` will cause those invites to be automatically rejected, which will practically disable option for other people to add you to their friend list (as ASF will deny all such requests, apart from people defined in `SteamUserPermissions`). Unless you want to outright deny all friend invites, you shouldn't enable this option.

Invalid trade offer is an offer that isn't accepted through built-in ASF module. More on this matter can be found in **[trading](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading)** section which explicitly defines what types of trade ASF is willing to accept automatically. Valid trades are also defined by other settings, especially `TradingPreferences`. `RejectInvalidTrades` will cause all invalid trade offers to be rejected, instead of being ignored. Unless you want to outright deny all trade offers that aren't automatically accepted by ASF, you shouldn't enable this option.

Invalid group invite is an invite that doesn't come from `SteamMasterClanID` group. ASF in normal mode ignores those group invites, as you'd expect, allowing you to decide yourself if you want to join particular Steam group or not. `RejectInvalidGroupInvites` will cause all those group invites to be automatically rejected, effectively making it impossible to invite you to any other group than `SteamMasterClanID`. Unless you want to outright deny all group invites, you shouldn't enable this option.

If you're unsure how to configure this option, it's best to leave it at default.

* * *

### `CustomGamePlayedWhileFarming`

`string` type with default value of `null`. When ASF is farming, it can display itself as "Playing non-steam game: `CustomGamePlayedWhileFarming`" instead of currently farmed game. This can be useful if you would like to let your friends know that you're farming, yet you don't want to change default `OnlineStatus`. Please note that ASF cannot guarantee the actual display order of Steam network, therefore this is only a suggestion that may, or may not, display properly. Default value of `null` disables this feature.

* * *

### `CustomGamePlayedWhileIdle`

`string` type with default value of `null`. Similar to `CustomGamePlayedWhileFarming`, but for the situation when ASF has nothing to do (as account is fully farmed). Default value of `null` disables this feature.

* * *

### `Enabled`

`bool` type with default value of `false`. This property defines if bot is enabled. Enabled bot instance (`true`) will automatically start on ASF run, while disabled bot instance (`false`) will need to be started manually. By default every bot is disabled, so you probably want to switch this property to `true` for all of your bots that should be started automatically.

* * *

### `FarmingOrders`

`ImmutableHashSet<byte>` type with default value of being empty. This property defines the **preferred** farming order used by ASF for given bot account. Currently there are following farming orders available:

| Valor | Nome                      | Descrição                                                                                                |
| ----- | ------------------------- | -------------------------------------------------------------------------------------------------------- |
| 0     | Unordered                 | Sem ordem, aumentando levemente a porformance da CPU                                                     |
| 1     | AppIDsAscending           | Tenta coletar dos jogos com o `appID` em ordem crescente                                                 |
| 2     | AppIDsDescending          | Tenta coletar dos jogos com o `appID` em ordem decrescente                                               |
| 3     | CardDropsAscending        | Tenta coletar dos jogos com o menor número de cartas disponíveis primeiro                                |
| 4     | CardDropsDescending       | Tenta coletar dos jogos com o maior número de cartas disponíveis primeiro                                |
| 5     | HoursAscending            | Tenta coletar dos jogos com o menor número de horas jogadas primeiro                                     |
| 6     | HoursDescending           | Tenta coletar dos jogos com o maior número de horas jogadas primeiro                                     |
| 7     | NamesAscending            | Tenta coletar dos jogos em ordem alfabética, começando do A                                              |
| 8     | NamesDescending           | Tenta coletar dos jogos em ordem alfabética reversa, começando do Z                                      |
| 9     | Random                    | Tenta coletar dos jogos em ordem completamente aleatória (diferente cada vez que o programa é executado) |
| 10    | BadgeLevelsAscending      | Tenta coletar dos jogos com o nível de insígnia mais baixo primeiro                                      |
| 11    | BadgeLevelsDescending     | Tenta coletar dos jogos com o nível de insígnia mais alto primeiro                                       |
| 12    | RedeemDateTimesAscending  | Tenta coletar dos jogos mais antigos em sua conta primeiro                                               |
| 13    | RedeemDateTimesDescending | Tenta coletar dos jogos mais novos em sua conta primeiro                                                 |
| 14    | MarketableAscending       | Tenta coletar dos jogos com cartas não comercializáveis primeiro                                         |
| 15    | MarketableDescending      | Tenta coletar dos jogos com cartas comercializáveis primeiro                                             |

Since this property is an array, it allows you to use several different settings in your fixed order. For example, you can include values of `15`, `11` and `7` in order to sort by marketable games first, then by those with highest badge level, and finally alphabetically. As you can guess, the order actually matters, as reverse one (`7`, `11` and `15`) achieves something entirely different. Majority of people will probably use just one order out of all of them, but in case you want to, you can also sort further by extra parameters.

Also notice the word "try" in all above descriptions - the actual ASF order is heavily affected by selected **[cards farming algorithm](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)** and sorting will affect only results that ASF considers same performance-wise. For example, in `Simple` algorithm, selected `FarmingOrders` should be entirely respected in current farming session (as every game has the same performance value), while in `Complex` algorithm actual order is affected by hours first, and then sorted according to chosen `FarmingOrders`. This will lead to different results, as games with existing playtime will have a priority over others, so effectively ASF will prefer games with highest playtime first, and only then sorting everything further by your chosen `FarmingOrders`. Therefore, this config property is only a **suggestion** that ASF will try to respect, as long as it doesn't affect performance negatively (in this case, ASF will always prefer performance over `FarmingOrders`).

There is also idling priority queue that is accessible through `iq` **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. If it's used, actual idling order is sorted firstly by performance, then by idling queue, and finally by your `FarmingOrders`.

* * *

### `GamesPlayedWhileIdle`

`ImmutableHashSet<uint>` type with default value of being empty. If ASF has nothing to farm it can play your specified steam games (`appID`s) instead. Playing games in such manner increases your "hours played" of those games, but nothing else apart of it. This feature can be enabled at the same time with `CustomGamePlayedWhileIdle` in order to play your selected games while showing custom status in Steam network, but in this case, like in `CustomGamePlayedWhileFarming` case, the actual display order is not guaranteed. Please note that Steam allows ASF to play only up to `32` `appID`s, therefore if you put more games than that, only first `32` will be respected (and extra ones being ignored).

* * *

### `HoursUntilCardDrops`

`byte` type with default value of `3`. This property defines if account has card drops restricted, and if yes, for how many initial hours. Restricted card drops means that account is not receiving any card drops from given game until the game is played for at least `HoursUntilCardDrops` hours. Unfortunately there is no magical way to detect that, so ASF relies on you. This property affects **[cards farming algorithm](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)** that will be used. Setting this property properly will maximize profits and minimize time required for cards to be farmed. Remember that there is no obvious answer whether you should use one or another value, since it fully depends on your account. It seems that older accounts which never asked for refund have unrestricted card drops, so they should use a value of `0`, while new accounts and those who did ask for refund have restricted card drops with a value of `3`. No entanto isso é apenas uma teoria e não deve ser tomada como regra. The default value for this property was set based on "lesser evil" and majority of use cases.

* * *

### `IdlePriorityQueueOnly`

`bool` type with default value of `false`. This property defines if ASF should consider for automatic idling only apps that you added yourself to priority idling queue available with `iq` **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. When this option is enabled, ASF will skip all `appIDs` that are missing on the list, effectively allowing you to cherry-pick games for automatic ASF idling. Keep in mind that if you didn't add any games to the queue then ASF will act as if there is nothing to idle on your account. If you're unsure whether you want this feature enabled or not, keep it with default value of `false`.

* * *

### `IdleRefundableGames`

`bool` type with default value of `true`. This property defines if ASF is permitted to idle games that are still refundable. A refundable game is a game that we bought in last 2 weeks through Steam Store and we didn't play it for longer than 2 hours yet, as stated **[here](https://store.steampowered.com/steam_refunds)**. By default when this option is set to `true`, ASF ignores Steam refunds entirely and idles everything, as most people expect. However, you can change this option to `false` if you want to ensure that ASF won't idle any of your refundable games too soon, allowing you to evaluate those games yourself and refund if needed without worrying about ASF affecting playtime negatively. Please note that if you disable this option then games you purchased from Steam Store won't be idled by ASF for up to 14 days since redeem date. If you're unsure whether you want this feature enabled or not, keep it with default value of `true`.

* * *

### `LootableTypes`

`ImmutableHashSet<byte>` type with default value of `1, 3, 5` steam item types. This property defines ASF behaviour when looting - both manual and automatic. ASF will ensure that only items from `LootableTypes` will be included in a trade offer, therefore this property allows you to choose what you want to receive in a trade offer that is being sent to you.

| Valor | Nome              | Descrição                                                                   |
| ----- | ----------------- | --------------------------------------------------------------------------- |
| 0     | Unknown           | Qualquer item que não se encaixa em nenhuma das opções abaixo               |
| 1     | BoosterPack       | Pacote de cartas                                                            |
| 2     | Emoticon          | Emoticons para uso no Chat Steam                                            |
| 3     | FoilTradingCard   | Versão brilhante da `Carta Colecionável`                                    |
| 4     | ProfileBackground | Fundo de perfil para usar em seu perfil Steam                               |
| 5     | TradingCard       | Cartas colecionáveis Steam, usadas para fabricar insígnias (não brilhantes) |
| 6     | SteamGems         | Gemas e pacotes de gemas Steam usadas para criar pacotes de cartas          |

Please note that regardless of the settings above, ASF will only ask for Steam (`appID` of 753) community (`contextID` of 6) items, so all game items, gifts and likewise, are excluded from the trade offer by definition.

Default ASF setting is based on most common usage of the bot, with looting only booster packs, and trading cards (including foils). The property defined here allows you to alter that behaviour in whatever way that satisfies you. Please keep in mind that all types not defined above will show as `Unknown` type, which is especially important when Valve releases some new Steam item, that will be marked as `Unknown` by ASF as well, until it's added here (in the future release). That's why in general it's not recommended to include `Unknown` type in your `LootableTypes`, unless you know what you're doing, and you also understand that ASF will send your entire inventory in a trade offer if Steam Network gets broken again and reports all your items as `Unknown`. My strong suggestion is to not include `Unknown` type in the `LootableTypes`, even if you expect to loot everything.

* * *

### `MatchableTypes`

`ImmutableHashSet<byte>` type with default value of `5` Steam item types. This property defines which Steam item types are permitted to be matched when `SteamTradeMatcher` option in `TradingPreferences` is enabled. Types are defined as below:

| Valor | Nome              | Descrição                                                                   |
| ----- | ----------------- | --------------------------------------------------------------------------- |
| 0     | Unknown           | Qualquer tipo que não se encaixa em nenhuma das opções abaixo               |
| 1     | BoosterPack       | Pacote de cartas                                                            |
| 2     | Emoticon          | Emoticons para uso no Chat Steam                                            |
| 3     | FoilTradingCard   | Versão brilhante da `Carta Colecionável`                                    |
| 4     | ProfileBackground | Fundo de perfil para usar em seu perfil Steam                               |
| 5     | TradingCard       | Cartas colecionáveis Steam, usadas para fabricar insígnias (não brilhantes) |
| 6     | SteamGems         | Gemas e pacotes de gemas Steam usadas para criar pacotes de cartas          |

Of course, types that you should use for this property typically include only `2`, `3`, `4` and `5`, as only those types are supported by STM. Please note that **ASF is not a trading bot** and **will NOT care about price or rarity**, which means that if you use it e.g. with `Emoticon` type, then ASF will be happy to trade your 2x rare emoticon for 1x rare 1x common, as that makes progress towards badge (in this case emoticons) completion. Please evaluate twice if you're fine with that. Unless you know what you're doing, you should keep it with default value of `5`.

* * *

### `OnlineStatus`

`byte` type with default value of `1`. This property specifies Steam community status that the bot will be announced with after logging in to Steam network. Currently you can choose one of below statuses:

| Valor | Nome            |
| ----- | --------------- |
| 0     | Offline         |
| 1     | Disponível      |
| 2     | Ocupado         |
| 3     | Ausente         |
| 4     | Dormindo        |
| 5     | Querendo trocar |
| 6     | Querendo jogar  |
| 7     | Invisível       |

`Offline` status is extremely useful for primary accounts. As you should know, farming a game actually shows your steam status as "Playing game: XXX", which can be misleading to your friends, confusing them that you're playing a game while actually you're only farming it. Using `Offline` status solves that issue - your account will never be shown as "in-game" when you're farming steam cards with ASF. This is possible thanks to the fact that ASF does not have to sign in into Steam Community in order to work properly, so we're in fact playing those games, connected to Steam network, but without announcing our online presence at all. Keep in mind that played games using offline status will still count towards your playtime, and show as "recently played" on your profile.

In addition to that, this feature is also important if you want to receive notifications and unread messages when ASF is running, while not keeping Steam client open at the same time. This is because ASF acts as a Steam client itself, and whether ASF would like it or not, Steam broadcasts all those messages and other events to it. This is not a problem if you have both ASF and your own Steam client running, as both clients receive exactly the same events. However, if just ASF is running, Steam network could mark certain events and messages as "delivered", despite of your traditional Steam client not receiving it due to not being present. Offline status also solves this problem, as ASF is never considered for any community events in this case, so all unread messages and other events will be properly marked as unread when you come back.

It's important to note that ASF running on `Offline` mode will **not** be able to receive commands in usual Steam chat way, as the chat, as well as entire community presence is in fact, entirely offline. A solution to this issue is using `Invisible` mode instead which works in a similar way (not exposing status), but keeps the ability to receive and respond to messages (so also a potential to dismiss notifications and unread messages as stated above). `Invisible` mode makes the most sense on alt accounts that you don't want to expose (status-wise), but still be able to send commands to.

However, there is one catch with `Invisible` mode - it doesn't go well with primary accounts. This is because **any** Steam session that is currently online **exposes** the status, even if ASF itself does not. This is caused by the current limitation/bug of the Steam network that isn't possible to be fixed on ASF side, so if you want to use `Invisible` mode you will also need to ensure that **all** other sessions to the same account use `Invisible` mode as well. This will be the case on alt accounts where ASF is hopefully the only active session, but on primary accounts you'll almost always prefer to show as `Online` to your friends, hiding only ASF activity, and in this case `Invisible` mode will be entirely useless for you (we recommend to use `Offline` mode instead). Hopefully this limitation/bug will be eventually solved in the future by Valve, but I wouldn't expect that to happen anytime soon...

If you're unsure how to set up this property, it's recommended to use a value of `0` (`Offline`) for primary accounts, and default `1` (`Online`) otherwise.

* * *

### `PasswordFormat`

`byte` type with default value of `0`. This property defines the format of `SteamPassword` property, and currently supports - `0` for `PlainText`, `1` for `AES` and `2` for `ProtectedDataForCurrentUser`. Please refer to **[Security](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security)** section if you want to learn more, as you'll need to ensure that `SteamPassword` property indeed includes password in matching `PasswordFormat`. In other words, when you change `PasswordFormat` then your `SteamPassword` should be **already** in that format, not just aiming to be. Unless you know what you're doing, you should keep it with default value of `0`.

* * *

### `Paused`

`bool` type with default value of `false`. This property defines initial state of `CardsFarmer` module. With default value of `false`, bot will automatically start farming when it's started, either because of `Enabled` or `start` command. Switching this property to `true` should be done only if you want to manually `resume` automatic farming process, for example because you want to use `play` all the time and never use automatic `CardsFarmer` module - this works exactly the same as `pause` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. If you're unsure whether you want this feature enabled or not, keep it with default value of `false`.

* * *

### `RedeemingPreferences`

`byte flags` type with default value of `0`. This property defines ASF behaviour when redeeming cd-keys, and is defined as below:

| Valor | Nome             | Descrição                                                                                 |
| ----- | ---------------- | ----------------------------------------------------------------------------------------- |
| 0     | None             | Sem preferências de resgate, situação padrão                                              |
| 1     | Forwarding       | Encaminha keys indisponíveis para serem resgatadas por outros bots                        |
| 2     | Distributing     | Distribui todas as keys entre si e os outros bots                                         |
| 4     | KeepMissingGames | Guarda as keys de jogos que (possivelmente) não estejam em sua conta, deixando-as sem uso |

Por favor note que esta propriedade é um campo do tipo `flags`, portanto é possível escolher qualquer combinação de valores disponíveis. Confira **[mapeamento flags](#mapeamento-json)** se você quiser aprender mais. Not enabling any of flags results in `None` option.

`Forwarding` will cause bot to forward a key that is not possible to redeem, to another connected and logged on bot that is missing that particular game (if possible to check). The most common situation is forwarding `AlreadyPurchased` game to another bot that is missing that particular game, but this option also covers other scenarios, such as `DoesNotOwnRequiredApp`, `RateLimited` or `RestrictedCountry`.

`Distributing` will cause bot to distribute all received keys among itself and other bots. This means that every bot will get a single key from the batch. Typically this is used only when you're redeeming many keys for the same game, and you want to evenly distribute them among your bots, as opposed to redeeming keys for various different games. This feature makes no sense if you're redeeming only one key in a single `redeem` action (as there are no extra keys to be distributed).

`KeepMissingGames` will cause bot to skip `Forwarding` when we can't be sure if key being redeemed is in fact owned by our bot, or not. This basically means that `Forwarding` will apply **only** to `AlreadyPurchased` keys, instead of covering also other cases such as `DoesNotOwnRequiredApp`, `RateLimited` or `RestrictedCountry`. Typically you might want to use this option on primary account, to ensure that keys being redeemed on it won't be forwarded further if your bot for example becomes temporarily `RateLimited`. As you can guess from the description, this field has absolutely no effect if `Forwarding` is not enabled.

Enabling both `Forwarding` and `Distributing` will add distributing feature on top of forwarding one, which makes ASF trying to redeem one key on all bots firstly (forwarding) before moving to the next one (distributing). Typically you want to use this option only when you want `Forwarding`, but with altered behaviour of switching the bot on key being used, instead of always going in-order with every key (which would be `Forwarding` alone). This behaviour can be beneficial if you know that majority or even all of your keys are tied to the same game, because in this situation `Forwarding` alone would try to redeem everything on one bot firstly (which makes sense if your keys are for unique games), and `Forwarding` + `Distributing` will switch the bot on the next key, "distributing" the task of redeeming new key onto another bot than the initial one (which makes sense if keys are for the same game, skipping one pointless attempt per key).

The actual bots order for all of the redeeming scenarios is alphabetical, excluding bots that are unavailable (not connected, stopped or likewise). Please keep in mind that there is per-IP and per-account hourly limit of redeeming tries, and every redeem try that didn't end with `OK` contributes to failed tries. ASF will do its best to minimize number of `AlreadyPurchased` failures, e.g. by trying to avoid forwarding a key to another bot that already owns that particular game, but it's not always guaranteed to work due to how Steam is handling licenses. Using redeeming flags such as `Forwarding` or `Distributing` will always increase your likelyhood to hit `RateLimited`.

Also keep in mind that you can't forward or distribute keys to bots that you do not have access to. This should be obvious, but ensure that you're at least `Operator` of all the bots you want to include in your redeeming process, for example with `status ASF` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**.

* * *

### `SendOnFarmingFinished`

`bool` type with default value of `false`. When ASF is done with farming given account, it can automatically send steam trade containing everything farmed up to this point to user with `Master` permission, which is very convenient if you don't want to bother with trades yourself. This option works the same as `loot` command, therefore keep in mind that it requires user with `Master` permission set, you might also require valid `SteamTradeToken`, including using an account that is actually eligible for trading. In addition to initiating `loot` after finishing farming, ASF will also initiate `loot` on each new items notification (when not farming), and after completing each trade that results in new items (always) when this option is active. This is especially useful for "forwarding" items received from other people to our account. It's strongly recommended to use this feature together with **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** being set, as there is no point in sending automatic trades if you need to manually confirm them. Se você não tem certeza de como definir essa propriedade, deixe-a com o valor padrão `false`.

* * *

### `SendTradePeriod`

`byte` type with default value of `0`. This property works very similar to `SendOnFarmingFinished` property, with one difference - instead of sending trade when farming is done, we can also send it every `SendTradePeriod` hours, regardless of how much we have to farm left. This is useful if you want to `loot` your alt accounts on usual basis instead of waiting for it to finish farming. Default value of `0` disables this feature, if you want your bot to send you trade e.g. every day, you should put `24` here. It's strongly recommended to use this feature together with **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** being set, as there is no point in sending automatic trades if you need to manually confirm them. If you're not sure how to set this property, leave it with default value of `0`.

* * *

### `ShutdownOnFarmingFinished`

`bool` type with default value of `false`. ASF is "occupying" an account for the whole time of process being active. When given account is done with farming, ASF periodically checks it (every `IdleFarmingPeriod` hours), if perhaps some new games with steam cards were added in the meantime, so it can resume farming of that account without a need to restart the process. This is useful for majority of people, as ASF can automatically resume farming when needed. However, you may actually want to stop the process when given account is fully farmed, you can achieve that by setting this property to `true`. When enabled, ASF will proceed with logging off when account is fully farmed, which means that it won't be periodically checked or occupied anymore. You should decide yourself if you prefer ASF to work on given bot instance for the whole time, or if perhaps ASF should stop it when farming process is done. When all accounts are stopped and process is not running in `--process-required` **[mode](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-Line-Arguments)**, ASF will shutdown as well. Se você não tem certeza de como definir essa propriedade, deixe-a com o valor padrão `false`.

* * *

### `SteamLogin`

`string` type with default value of `null`. This property defines your steam login - the one you use for logging in to steam. In addition to defining steam login here, you may also keep default value of `null` if you want to enter your steam login on each ASF startup instead of putting it in the config. This may be useful for you if you don't want to save sensitive data in config file.

* * *

### `SteamMasterClanID`

`ulong` type with default value of `0`. This property defines the steamID of the steam group that bot should automatically join, including its group chat. You can check steamID of your group by navigating to its **[page](https://steamcommunity.com/groups/ascfarm)**, then adding `/memberslistxml?xml=1` to the end of the link, so the link will look like **[this](https://steamcommunity.com/groups/ascfarm/memberslistxml?xml=1)**. Then you can get steamID of your group from the result, it's in `<groupID64>` tag. In above example it would be `103582791440160998`. In addition to trying to join given group at startup, the bot will also automatically accept group invites to this group, which makes it possible for you to invite your bot manually if your group has private membership. If you don't have any group dedicated for your bots, you should keep this property with default value of `0`.

* * *

### `SteamParentalCode`

`string` type with default value of `null`. This property defines your steam parental PIN. ASF requires an access to resources protected by steam parental, therefore if you use that feature, you need to provide ASF with parental unlock PIN, so it can operate normally. Default value of `null` means that there is no steam parental PIN required to unlock this account, and this is probably what you want if you don't use steam parental functionality. In addition to defining steam parental PIN here, you may also use value of `0` if you want to enter your steam parental PIN on each ASF startup instead of putting it in the config. This may be useful for you if you don't want to save sensitive data in config file.

* * *

### `SteamPassword`

`string` type with default value of `null`. This property defines your steam password - the one you use for logging in to steam. In addition to defining steam password here, you may also keep default value of `null` if you want to enter your steam password on each ASF startup instead of putting it in the config. This may be useful for you if you don't want to save sensitive data in config file.

* * *

### `SteamTradeToken`

`string` type with default value of `null`. When you have your bot on your friend list, then bot can send a trade to you right away without worrying about trade token, therefore you can leave this property at default value of `null`. If you however decide to NOT have your bot on your friend list, then you will need to generate and fill a trade token as the user that this bot is expecting to send trades to. In other words, this property should be filled with trade token of the account that is defined with `Master` permission in `SteamUserPermissions` of **this** bot instance.

In order to find your token, as logged in user with `Master` permission, navigate **[here](https://steamcommunity.com/my/tradeoffers/privacy)** and take a look at your trade URL. The token we're looking for is made out of 8 characters after `&token=` part in your trade URL. You should copy and put those 8 characters here, as `SteamTradeToken`. Do not include whole trading URL, neither `&token=` part, only token itself.

* * *

### `SteamUserPermissions`

`ImmutableDictionary<ulong, byte>` type with default value of being empty. This property is a dictionary property which maps given Steam user identified by his 64-bit steam ID, to `byte` number that specifies his permission in ASF instance. Currently available bot permissions in ASF are defined as:

| Valor | Nome          | Descrição                                                                                                                                                                                                               |
| ----- | ------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0     | None          | Sem permissão, esse é apenas um valor de referência que é atribuído quando não há IDs Steam nesse parâmetro - não há necessidade de definir alguém com esta permissão                                                   |
| 1     | FamilySharing | Fornece acesso mínimo para usuários do modo família. Novamente, esse é apenas um valor de referência, uma vez que o ASF é capaz de descobrir automaticamente os IDs Steam que estão autorizados a usar nossa biblioteca |
| 2     | Operator      | Fornece acesso básico a determinadas contas bot, principalmente adicionar licenças e resgatar keys                                                                                                                      |
| 3     | Master        | Fornece acesso completo a determinada conta bot                                                                                                                                                                         |

In short, this property allows you to handle permissions for given users. Permissions are important mainly for access to ASF **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**, but also for enabling many ASF features, such as accepting trades. For example you might want to set your own account as `Master`, and give `Operator` access to 2-3 of your friends so they can easily redeem keys for your bot with ASF, while **not** being eligible e.g. for stopping it. Thanks to that you can easily assign permissions to given users and let them use your bot to some specified by you degree.

We recommend to set exactly one user as `Master`, and any amount you wish as `Operator`s and below. While it's technically possible to set multiple `Master`s and ASF will work correctly with them, for example by accepting all of their trades sent to the bot, ASF will use only one of them (with lowest steam ID) for every action that requires a single target, for example a `loot` request, so also properties like `SendOnFarmingFinished` or `SendTradePeriod`. If you perfectly understand those limitations, especially the fact that `loot` request will always send items to the `Master` with lowest steam ID, regardless of the `Master` that actually executed the command, then you can define multiple users with `Master` permission here, but we still recommend a single master scheme - multiple masters scheme is discouraged setup that is not supported.

It's nice to note that there is one more extra `Owner` permission, which is declared as `SteamOwnerID` global config property. You can't assign `Owner` permission to anybody here, as `SteamUserPermissions` property defines only permissions that are related to the bot instance, and not ASF as a process.

* * *

### `TradingPreferences`

`byte flags` type with default value of `0`. This property defines ASF behaviour when in trading, and is defined as below:

| Valor | Nome                | Descrição                                                                                                                                                                                                          |
| ----- | ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 0     | None                | Sem preferências de troca - aceita somente trocas do `Master`                                                                                                                                                      |
| 1     | AcceptDonations     | Aceita trocas em que não estamos perdendo nada                                                                                                                                                                     |
| 2     | SteamTradeMatcher   | Aceita cartas duplicadas de forma semelhante ao **[STM](https://www.steamtradematcher.com)**. Visite a seção **[trocas](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading-pt-BR)** para mais informações |
| 4     | MatchEverything     | Requer que `SteamTradeMatcher` seja definido, e em combinação com ele - também aceita trocas ruins além de boas e neutras                                                                                          |
| 8     | DontAcceptBotTrades | Não aceita automaticamente as trocas `loot` de outras contas bot                                                                                                                                                   |

Por favor note que esta propriedade é um campo do tipo `flags`, portanto é possível escolher qualquer combinação de valores disponíveis. Confira **[mapeamento flags](#mapeamento-json)** se você quiser aprender mais. Not enabling any of flags results in `None` option.

For further explanation of ASF trading logic, and description of every available flag, please visit **[Trading](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading)** section.

* * *

### `TransferableTypes`

`ImmutableHashSet<byte>` type with default value of `1, 3, 5` steam item types. This property defines which Steam item types will be considered for transferring between bots, during `transfer` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. ASF will ensure that only items from `TransferableTypes` will be included in a trade offer, therefore this property allows you to choose what you want to receive in a trade offer that is being sent to one of your bots.

| Valor | Nome              | Descrição                                                                   |
| ----- | ----------------- | --------------------------------------------------------------------------- |
| 0     | Unknown           | Qualquer item que não se encaixa em nenhuma das opções abaixo               |
| 1     | BoosterPack       | Pacote de cartas                                                            |
| 2     | Emoticon          | Emoticons para uso no Chat Steam                                            |
| 3     | FoilTradingCard   | Versão brilhante da `Carta Colecionável`                                    |
| 4     | ProfileBackground | Fundo de perfil para usar em seu perfil Steam                               |
| 5     | TradingCard       | Cartas colecionáveis Steam, usadas para fabricar insígnias (não brilhantes) |
| 6     | SteamGems         | Gemas Steam usadas para criar pacotes de cartas, incluindo as empacotadas   |

Please note that regardless of the settings above, ASF will only ask for Steam (`appID` of 753) community (`contextID` of 6) items, so all game items, gifts and likewise, are excluded from the trade offer by definition.

Default ASF setting is based on most common usage of the bot, with transfering only booster packs, and trading cards (including foils). The property defined here allows you to alter that behaviour in whatever way that satisfies you. Please keep in mind that all types not defined above will show as `Unknown` type, which is especially important when Valve releases some new Steam item, that will be marked as `Unknown` by ASF as well, until it's added here (in the future release). That's why in general it's not recommended to include `Unknown` type in your `TransferableTypes`, unless you know what you're doing, and you also understand that ASF will send your entire inventory in a trade offer if Steam Network gets broken again and reports all your items as `Unknown`. My strong suggestion is to not include `Unknown` type in the `TransferableTypes`, even if you expect to transfer everything.

* * *

### `UseLoginKeys`

`bool` type with default value of `true`. This property defines if ASF should use login keys mechanism for this Steam account. Login keys mechanism works very similar to official Steam client's "remember me" option, which makes it possible for ASF to store and use temporary one-time use login key for next logon attempt, effectively skipping a need of providing password, Steam Guard or 2FA code as long as our login key is valid. Login key is stored in `BotName.db` file and updated automatically. This is why you don't need to provide password/SteamGuard/2FA code after logging in successfully with ASF just once.

Login keys are used by default for your convenience, so you don't need to input `SteamPassword`, SteamGuard or 2FA code (when not using **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)**) on each login. It's also superior alternative since login key can be used only for a single time and does not reveal your original password in any way. Exactly the same method is being used by your original Steam client, which saves your account name and login key for your next logon attempt, effectively being the same as using `SteamLogin` with `UseLoginKeys` and empty `SteamPassword` in ASF.

However, some people might be concerned even about this little detail, therefore this option is available here for you if you'd like to ensure that ASF won't store any kind of token that would allow resuming previous session after being closed, which will result in full authentication being mandatory on each login attempt. Disabling this option will work exactly the same as not checking "remember me" in official Steam client. Unless you know what you're doing, you should keep it with default value of `true`.

**[Voltar ao topo](#configuração)**

* * *

## Estrutura de arquivos

ASF is using quite simple file structure.

    ├── config
    │     ├── ASF.json
    │     ├── ASF.db
    │     ├── Bot1.json
    │     ├── Bot1.db
    │     ├── Bot1.bin
    │     ├── Bot2.json
    │     ├── Bot2.db
    │     ├── Bot2.bin
    │     └── ...
    ├── ArchiSteamFarm.dll
    ├── log.txt
    └── ...
    

In order to move ASF to new location, for example another PC, it's enough to move/copy `config` directory alone, and that's the recommended way of doing any form of "ASF backups".

`log.txt` file holds the log generated by your last ASF run. This file doesn't contain any sensitive information, and is extremely useful when it comes to issues, crashes or simply as an information to you what happened in last ASF run. We will very often ask about for file if you run into issues or bugs. ASF automatically manages this file for you, but you can further tweak ASF **[logging](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Logging)** module if you're advanced user.

`config` directory is the place that holds configuration for ASF, including all of its bots.

`ASF.json` is a global ASF configuration file. This config is used for specifying how ASF behaves as a process, which affects all of the bots and program itself. You can find global properties there, such as ASF process owner, auto-updates or debugging.

`BotName.json` is a config of given bot instance. This config is used for specifying how given bot instance behaves, therefore those settings are specific to that bot only and not shared across other ones. This allows you to configure bots with various different settings and not necessarily all of them working in exactly the same way.

Apart from config files, ASF also uses `config` directory for storing databases.

`ASF.db` is a global ASF database file. It acts as a global persistent storage and is used for saving various information related to ASF process, such as IPs of local Steam servers. **Não edite este arquivo**.

`BotName.db` is a database of given bot instance. This file is used for storing crucial data about given bot instance in persistent storage, such as login keys or ASF 2FA. **Não edite este arquivo**.

`BotName.bin` is a special file of given bot instance, which holds information about Steam sentry hash. Sentry hash is used for authenticating using `SteamGuard` mechanism, very similar to Steam `ssfn` file. **Não edite este arquivo**.

`BotName.keys` is a special file that can be used for importing keys into **[background games redeemer](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer)**. It's not mandatory and not generated, but recognized by ASF. This file is automatically deleted after keys are successfully imported.

`BotName.maFile` is a special file that can be used for importing **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)**. It's not mandatory and not generated, but recognized by ASF if your `BotName` does not use ASF 2FA yet. This file is automatically deleted after ASF 2FA is successfully imported.

**[Voltar ao topo](#configuração)**

* * *

## Mapeamento JSON

Every configuration property has its type. Type of the property defines values that are valid for it. You can only use values that are valid for given type - if you use invalid value, then ASF won't be able to parse your config.

**We strongly recommend to use ConfigGenerator for generating configs** - it handles most of the low-level stuff (such as types validation) for you, so you only need to input proper values, and you also don't need to understand variable types specified below. This section is mainly for people generating/editing configs manually, so they know what values they can use.

Types used by ASF are native C# types, which are specified below:

* * *

`bool` - Boolean type accepting only `true` and `false` values.

Example: `"Enabled": true`

* * *

`byte` - Unsigned byte type, accepting only integers from `0` to `255` (inclusive).

Example: `"ConnectionTimeout": 60`

* * *

`uint` - Unsigned integer type, accepting only integers from `0` to `4294967295` (inclusive).

* * *

`ulong` - Unsigned long integer type, accepting only integers from `0` to `18446744073709551615` (inclusive).

Example: `"SteamMasterClanID": 103582791440160998`

* * *

`string` - String type, accepting any sequence of characters, including empty sequence `""` and `null`. Both empty sequence as well as `null` value is treated the same by ASF, so it's up to your preference which one you want to use.

Examples: `"SteamLogin": null`, `"SteamLogin": ""`, `"SteamLogin": "MyAccountName"`

* * *

`ImmutableHashSet<valueType>` - Immutable collection (set) of unique values in given `valueType`. In JSON, it's defined as array of elements in given `valueType`.

Example for `ImmutableHashSet<uint>`: `"Blacklist": [267420, 303700, 335590]`

* * *

`ImmutableDictionary<keyType, valueType>` - Immutable dictionary (map) that maps a key specified in its `keyType`, to value specified in its `valueType`. In JSON, it's defined as an object with key-value pairs. Keep in mind that `keyType` is always quoted in this case, even if it's value type such as `ulong`.

Example for `ImmutableDictionary<ulong, byte>`: `"SteamUserPermissions": { "76561198174813138": 3, "76561198174813137": 1 }`

* * *

`flags` - Flags attribute combines several different properties into one final value by applying bitwise operations. This allows you to choose any possible combination of various different allowed values at the same time. The final value is constructed as a sum of values of all enabled options.

For example, given following values:

| Valor | Nome |
| ----- | ---- |
| 0     | None |
| 1     | A    |
| 2     | B    |
| 4     | C    |

Using `B + C` would result in value of `6`, using `A + C` would result in value of `5`, using `C` would result in value of `4` and so on. This allows you to create any possible combination of enabled values - if you decided to enable all of them, making `None + A + B + C`, you'd get value of `7`. Also notice that flag with value of `0` is enabled by definition in all other available combinations, therefore very often it's a flag that doesn't enable anything specifically (such as `None`).

So as you can see, in above example we have 3 available flags to switch on/off (`A`, `B`, `C`), and 8 possible values overall (`None -> 0`, `A -> 1`, `B -> 2`, `A+B -> 3`, `C -> 4`, `A+C -> 5`, `B+C -> 6`, `A+B+C -> 7`).

**[Voltar ao topo](#configuração)**

* * *

## Mapeamento de compatibilidade

Due to JavaScript limitations of being unable to properly serialize simple `ulong` fields in JSON when using web-based ConfigGenerator, `ulong` fields will be rendered as strings with `s_` prefix in the resulting config. This includes for example `"SteamOwnerID": 76561198006963719` that will be written by our ConfigGenerator as `"s_SteamOwnerID": "76561198006963719"`. ASF includes proper logic for handling this string mapping automatically, so `s_` entries in your configs are actually valid and correctly generated. If you're generating configs yourself, we recommend to stick with original `ulong` fields if possible, but if you're unable to do so, you can also follow this scheme and encode them as strings with `s_` prefix added to their names. We hope to resolve this JavaScript limitation eventually.

**[Voltar ao topo](#configuração)**

* * *

## Configuração de compatibilidade

It's top priority for ASF to remain compatible with older configs. As you should already know, missing config properties are treated the same as they would be defined with their **default values**. Therefore, if new config property gets introduced in new version of ASF, all your configs will remain **compatible** with new version, and ASF will treat that new config property as it'd be defined with its **default value**. You can always add, remove or edit config properties according to your needs. We recommend to limit defined config properties only to those that you want to change, since this way you automatically inherit default values for all other ones, not only keeping your config clean but also increasing compatibility in case we decide to change a default value for property that you don't want to explicitly set yourself. Feel free to check `minimal.json` example configuration file that follows this concept.

**[Voltar ao topo](#configuração)**

* * *

## Recarregamento automático

Starting with ASF V2.1.6.2+, the program is now aware of configs being modified "on-the-fly" - thanks to that, ASF will automatically:

- Criar (e iniciar, se necessário) um novo bot, quando você criar a configuração do mesmo
- Parar (se necessário) e remover o bot antigo, quando você excluir a sua configuração
- Parar (e iniciar, se necessário) qualquer bot, quando você editar a configuração do mesmo
- Reiniciar (se necessário) o bot com novo nome, quando você renomear sua configuração

All of the above is transparent and will be done automatically without a need of restarting the program, or killing other (unaffected) bot instances.

In addition to that, ASF will also restart itself (if `AutoRestart` permits) if you modify core ASF `ASF.json` config. Likewise, program will quit if you delete or rename it.

**[Voltar ao topo](#configuração)**