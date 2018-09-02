# Configuração

Esta página é dedicada a configuração do ASF. Ela serve como uma documentação completa da pasta `config`, permitindo que você modifique o ASF conforme suas necessidades.

- **[Introdução](#introduction)**
- **[Gerador de configuração web](#web-based-configgenerator)**
- **[Configuração manual](#manual-configuration)**
- **[Configuração global](#global-config)**
- **[Configuração de Bot](#bot-config)**
- **[Estrutura de arquivos](#file-structure)**
- **[Mapeamento JSON](#json-mapping)**
- **[Mapeamento de compatibilidade](#compatibility-mapping)**
- **[Configuração de compatibilidade](#configs-compatibility)**
- **[Recarregamento automático](#auto-reload)**

* * *

## Introdução

A configuração do ASF é dividida em duas partes principais - configuração global (do processo) e configuração individual dos bots. Cada bot tem seu próprio arquivo de configuração do chamado `NomeDoBot.json` (onde `NomeDoBot` é o nome dado ao bot), enquanto as configurações globais (do processo) do ASF é um único arquivo chamado `ASF.json`.

Um bot é uma conta comum da steam que tem participação no processo ASF. Para trabalhar corretamente, o ASF precisa de pelo menos **uma** conta bot definida. Não há limite imposto pelo processo em quantidade de contas bot, então você pode usar quantos bots (contas steam) você quiser.

O ASF usa o formato **[JSON](https://en.wikipedia.org/wiki/JSON)** para armazenar seus arquivos de configuração. É um formato amigável, legível e muito universal no qual você pode configurar o programa. No entanto não se preocupe, você não precisa saber JSON para configurar o ASF. Eu acabei de mencionar isso para o caso de você desejar criar configurações em massa do ASF com algum tipo de script.

A configuração pode ser feita manualmente - criando configurações JSON adequadas, ou usando o nosso **[Gerador de configuração web](https://justarchi.github.io/ArchiSteamFarm)**, que deverá ser muito mais fácil e conveniente. A menos que você seja um usuário acvançado, sugiro usar o gerador de configuração, que será descrito abaixo.

**[Voltar para o topo](#configuration)**

* * *

## Gerador de configuração web

A finalidade do gerador de configurações web é te proporcionar uma interface amigável que será usada para a geração de arquivos de configuração do ASF. O gerador de configuração Web é 100% baseada no cliente, o que significa que os detalhes que você está inserindo não estão sendo enviados para lugar algum, mas processados apenas localmente. Isto garante segurança e confiabilidade, pois ele pode até mesmo trabalhar **[offline](https://github.com/JustArchi/ArchiSteamFarm/tree/master/docs)** se você preferir baixar todos os arquivos e executar o `index.html` no seu navegador favorito.

O Gerador de configuração Web foi testado e roda perfeitamente no Chrome e no Firefox, e deve funcionar corretamente em todos os navegadores mais populares com javascript habilitado.

O uso é muito simples - selecione se você deseja gerar uma configuração `ASF` ou `Bot` alternando para a guia adequada, certifique-se de que a versão do arquivo de configuração corresponde a sua versão do ASF, então insira todos os dados e clique no botão "download". Mova este arquivo para a pasta `config` do ASF, substituindo os arquivos existentes, caso necessário. Repita esses passos para todas as eventuais modificações e recorra ao resto desta seção para a explicação de todas as opções disponíveis para configurar.

**[Voltar ao início](#configuration)**

* * *

## Configuração manual

Eu recomendo fortemente usar o gerador de configuração web, mas se por algum motivo você não quiser, então você pode criar configurações adequadas você mesmo. Confira `example.json` para ter uma boa ideia da estrutura adequada, você pode copiar esse arquivo e usar como base para seu recém-configurado bot. Já que que você não estará usando nossa interface, certifique-se que sua configuração é **[válida](https://jsonlint.com)**, pois ASF vai se recusar a carregá-la se ela não p uder ser analisada. Para estrutura JSON adequada de todos os campos disponíveis, consulte a seção de **[mapeamento JSON](#json-mapping)** e a documentação abaixo.

**[Voltar para o topo](#configuration)**

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
    "IPCPrefixes": [
        "http://127.0.0.1:1242/"
    ],
    "LoginLimiterDelay": 10,
    "MaxFarmingTime": 10,
    "MaxTradeHoldDuration": 15,
    "OptimizationMode": 0,
    "Statistics": true,
    "SteamMessagePrefix": "/me ",
    "SteamOwnerID": 0,
    "SteamProtocols": 5,
    "UpdateChannel": 1,
    "UpdatePeriod": 24,
    "WebLimiterDelay": 200,
    "WebProxy": null,
    "WebProxyPassword": null,
    "WebProxyUsername": null
}
```

**Dica:** A menos que você deseja alterar qualquer uma dessas opções, você pode manter tudo em seus valores padrões, portanto você pode fechar o `ASF.json` e proceder à configuração do bot.

* * *

Todas as opções são explicadas abaixo:

`AutoRestart` - tipo `bool` com valor padrão `true`. Esta propriedade define se o ASF pode reiniciar automaticamente, caso necessário. Existem alguns casos que exigirão que o ASF reinicie automaticamente, tais como a atualização do ASF (feita com o comando `UpdatePeriod` ou `update`), edições na configuração do `ASF.json`, assim como o comando `restart`. Normalmente, reiniciar inclui duas partes - criar um novo processo e encerrar o atual. A maioria dos usuários deve concordar com isso e manter essa propriedade com o valor padrão `true`, no entanto - se você estiver executando o ASF através de seu próprio código e/ou com `dotnet`, você pode querer ter controle total sobre o início do processo, e evitar uma situação tal como ter um novo processo (reiniciado) do ASF processo sendo executado silenciosamente em algum lugar em segundo plano e não no código em primeiro plano, que foi encerrado junto com o antigo processo do ASF. Isto é especialmente importante considerando o fato de que o novo processo deixará de ser seu filho direto, o que faria você não conseguir, por exemplo, usar entradas de console padrão para ele.

Se for esse o caso, essa propriedade existe especialmente para você e você pode definí-la como `false`. Contudo, tenha em mente que, nesse caso, ** você ** é o responsável por reiniciar o processo. Isto é importante pois o ASF vai apenas fechar, em vez de criar um novo processo (p ex, após uma atualização), então se não há nenhuma lógica adicionada por você, ele vai simplesmente parar de funcionar até que você o inicie novamente. O ASF sempre fecha com um código de erro apropriado indicando sucesso (zero) ou não-sucesso (diferente de zero), assim você pode adicionar a lógica correta em seu código, que deve evitar o reinício automático do ASF em caso de falha ou ao menos fazer uma cópia local do `log.txt` para análise posterior. Também tenha em mente que o comando `restart` irá sempre reiniciar o ASF, independentemente de como esta propriedade está configurada, pois esta propriedade define o comportamento padrão, enquanto o comando `restart ` reinicia o processo. A menos que você tenha um motivo para desativar esse recurso, você deve mantê-lo ativado.

* * *

`Blacklist` - tipo `ImmutableHashSet<uint>` com valor padrão vazio. Como o nome sugere, esta propriedade de configuração global define appIDs (jogos) que serão totalmente ignorados pelo processo automático de coleta do ASF. Infelizmente, a Steam adora sinalizar as insígnias das promoções de verão/inverno como "disponíveis para recebimento de cartas", o que confunde o processo do ASF, fazendo-o acreditar que é um jogo válido que deve ser coletado. Se não houvesse nenhum tipo de lista negra, o ASF acabaria por "travar" na coleta de um jogo que na verdade não é um jogo, e esperar infinitamente pelo recebimento de cartas que não acontecerão. A lista negra do ASF serve para marcar essas insígnias como não disponíveis para a coleta, de modo que podemos silenciosamente ignorá-las ao decidir o que coletar, e assim não cair na armadilha.

O ASF inclui duas listas negras por padrão - `GlobalBlacklist`, que está codificada dentro do código do ASF e não pode ser editada, e a `Blacklist` normal, que está definida aqui. A `GlobalBlacklist` é atualizada juntamente com a versão do ASF e normalmente inclui todos os "maus" appIDs no momento do lançamento, então se você estiver usando o ASF atualizado então você não precisa manter que sua própria `Blacklist` definida aqui. O principal objetivo desta propriedade é permitir que você adicione novos appIDs, ainda não conhecidos no momento do lançamento de uma nova versão do ASF, que não devam ser coletados. A `GlobalBlacklist` codificada é atualizada sempre o mais rápido possível, portanto você não precisa atualizar sua própria `Blacklist`, se estiver usando a versão mais recente do ASF, mas sem a `Blacklist` você seria forçado a atualizar o ASF para que o mesmo "continuasse rodando" quando a Valve liberasse novas insígnias de promoções - não quero forçá-lo a usar o código mais recente do ASF, portanto, essa propriedade está aqui para permitir que você "conserte" o ASF por sua conta se, por algum motivo não quer, ou não pode, atualizar para a nova `GlobalBlacklist` codificada em uma nova versão do ASF, mais ainda quer manter seu velho ASF funcionando. A menos que você tenha uma razão muito **forte** para editar essa propriedade, você deve mantê-la padrão.

Se você está procurando uma lista negra baseada em bot, dê uma olhada nos comandos `ib`, `ibadd` e `ibrm`**[](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)**.

* * *

`CommandPrefix` - tipo `string` com valor padrão `!`. Esta propriedade especifica o prefixo, **que diferencia maiúsculas de minúsculas**, usado para os **[comandos](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)** do ASF. Em outras palavras, isto é o que você precisa digitar antes de cada comando do ASF para fazer com que ele te ouça. É possível definir esse valor para `null` ou deixá-lo vazio para fazer com que o ASF não use prefixos de comando, nesse caso você pode enviar comandos simplesmente por seus identificadores. No entanto, fazer isso potencialmente irá diminuir o desempenho do ASF já que ele é otimizado para não analisar mensages se elas não forem iniciadas com o `CommandPrefix` - se você intencionalmente decidir não usar isso, o ASF será forçado a ler todas as mensagens e respondê-las, mesmo se elas não forem comandos ASF. Portanto, é recomendável usar algum `CommandPrefix`, tal como `/` se não gostar do valor padrão `!`. Para obter consistência, `CommandPrefix` afeta todo o processo do ASF. A menos que você tenha uma razão para editar essa propriedade, você deve mantê-la padrão.

* * *

`ConfirmationsLimiterDelay` - tipo `byte` com valor padrão de `10`. A rede Steam, em geral, inclui várias taxas-limite de pedidos semelhantes, portanto devemos adicionar algum atraso extra a fim de evitar ativar essa limitação que nos impediria de interagir com o serviço. O ASF irá garantir que haverá pelo menos o número de segundos de atrazo definido em `ConfirmationsLimiterDelay` entre duas solicitações consecutivas de confirmações 2FA - aquelas que estão sendo usados pelo **[ASF 2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Two-factor-authentication)** durante, por exemplo, o comando `2faok`, bem como na base necessiária quando se executa várias operações relacionadas a troca. O valor padrão foi definido com base em nossos testes e não deve ser diminuído se você não quiser ter problemas. A menos que você tenha uma razão muito **forte** para editar essa propriedade, você deve mantê-la padrão.

* * *

`ConnectionTimeout` - tipo `byte` com valor padrão de `60`. Essa propriedade define o tempo limite em segundos para várias ações de rede feitas pelo ASF. Em particular, `ConnectionTimeout` define o tempo limite em segundos para solicitações HTTP e IPC, `ConnectionTimeout / 10` define o número máximo de falhas de pulsação, enquanto `ConnectionTimeout / 30` define o número de minutos que permitimos para a solicitação inicial de conexão da rede Steam. O alor padrão de `60` deve ser bom para a maioria das pessoas, no entanto, se você tem uma conexão de rede ou PC muito lento, convém aumentar esse número para algo mais elevado (tipo uns `90`). Tenha em mente que valores maiores não vão magicamente resolver lentidões ou mesmo atingir servidores Steam inacessíveis, então nós não devemos esperar infinitamente por algo que não vai acontece, mas simplesmente tentar novamente mais tarde. Definir um valor muito alto irá resultar em um atraso excessivo na captura de problemas de rede, bem como na diminuição do desempenho global. Definir esse valor muito baixo irá diminuir a estabilidade total e também o desempenho, uma vez que o ASF abortará solicitações válidas que ainda estejam sendo analisadas. Portanto, definir esse valor mais baixo do que o padrão não tem nenhuma vantagem em geral, uma vez que os servidores Steam tendem a serem lento de vez em quando e podem exigir mais tempo para analisar pedidos do ASF. O valor padrão é um equilíbrio entre acreditar que nossa conexão de rede é estável, e duvidar que a rede Steam vai para manipular nosso pedido em dado tempo limite. Se você deseja detectar problemas mais cedo e fazer o ASF reconectar/responder mais rápido, o valor padrão deve bastar (ou um valor ligeiramente abaixo, tornando o ASF menos paciente). Se em vez disso você notar que o ASF está tendo problemas de rede, tais como falhas em solicitações, pulsações perdidas ou conexão com a rede Steam interrompida, pode fazer sentido aumentar esse valor se você tiver certeza que o problema **não** é causado pela sua rede, mas pela própria Steam, uma vez que tempos limite maiores tornam o ASF mais "paciente" e não decidido a se reconectar imediatamente. Também pode fazer sentido aumentar esse valor se você tem uma conexão um tanto lenta que requer mais tempo para a transmissão dos dados. Em resumo, o valor padrão deve ser decente para a maioria dos casos, mas você pode querer aumentá-lo se necessário. Ainda, indo muito acima do valor padrão não faz muito sentido, uma vez que limites de tempo maiores não consertam magicamente servidores Steam inacessíveis. A menos que você tenha uma razão para editar essa propriedade, você deve mantê-la padrão.

* * *

`CurrentCulture` - tipo `string` com valor padrão `null`. Por padrão o ASF tenta usar o idioma do seu sistema operacional e vai preferir usar linhas traduzidas para esse idioma se disponível. Isto é possível graças à nossa comunidade que tenta **[localizar](https://github.com/JustArchi/ArchiSteamFarm/wiki/Localization)** o ASF para quase todos os idiomas mais populares. Se por algum motivo você não quiser usar o idioma nativo do seu SO, você pode usar esta propriedade de configuração para escolher qualquer idioma válido que você queira usar. Para obter uma lista de todas as culturas disponíveis, visite o **[MSDN](https://msdn.microsoft.com/en-us/library/cc233982.aspx)** e procure a `aba Language`. É bom notar que o ASF aceita tanto culturas diferentes, tal qual `en-GB`, quanto neutros, como `en`. Especificando a cultura atual também pode afetar outros comportamento culturais específicos, tais como formato de moeda/data e outros. Por favor, note que você pode precisar de pacotes de idioma/fontes adicionais para exibir caracteres específicos do idioma corretamente, caso você tenha escolhido um idioma não-nativo que faça uso deles. Normalmente você fará uso desta propriedade de configuração se preferir o ASF em inglês ao invés de sua língua nativa.

* * *

`Debug` - tipo `bool` com valor padrão `false`. Esta propriedade define se o processo deve ser executado em modo de depuração. Quando em modo de depuração, o ASF cria uma pasta especial chamada `debug` ao lado da pasta `config`, que mantém o controle de toda a comunicação entre o ASF e os servidores Steam. Informações de depuração podem ajudar a encontrar comportamentos estranhos relatados ao trabalho da rede e do ASF em geral. Além disso, algumas rotinas do programa serão muito mais detalhadas, como o `WebBrowser` afirmando a razão exata pela falha de alguns pedidos - essas entradas são escritas no log normal do ASF. **Você não deve executar o ASF no modo de depuração, a menos que seja solicitado pelo desenvolvedor**. Executar o ASF em depuração modo **diminui o desempenho**, **afeta negativamente a estabilidade** e é **muito mais detalhado em vários lugares**, por isso deve ser usado **apenas** intencionalmente, num espaço curto de tempo, para depurar uma questão particular, reproduzindo a problema ou obtendo mais informação sobre uma solicitação falha entre outros, mas **não** para execução normal do programa. Você vai ver **um monte** de novos erros, problemas, e exceções - certifique-se de que você tenha um conhecimento razoável sobre o ASF, Steam e suas peculiaridades, caso você decida analisar tudo isso por conta própria, uma vez que nem tudo é relevante.

**AVISO:** ativar esse modo inclui o registro de informações **potencialmente confidenciais** tais como logins e senhas que você está usando para acessar a Steam (devido ao registro de rede). Os dados são gravados tanto na pasta `depuração` quanto no `log.txt` padrão (que agora é intencionalmente muito mais detalhado para registrar esta informação). Você não deve postar conteúdo de depuração gerado por ASF em qualquer local público, o desenvolvedor do ASF deve sempre lembrar de enviá-lo para seu e-mail, ou outro local seguro. Não estamos armazenando, nem fazendo uso desses detalhes confidenciais, eles são escritos como parte das rotinas de depuração, já que sua presença pode ser relevante para o problema que está afetando você. Nós preferimos que se você não altere a forma com que o ASF faz os registros, mas se você está preocupado, você está livre para editar esses detalhes confidenciais apropriadamente.

> Editar envolve a substituição de detalhes confidenciais, por exemplo, com estrelas. Você deve evitar remover linhas confidenciais inteiramente, já que sua existência pura pode ser relevante e deve ser preservada.

* * *

`FarmingDelay` - tipo `byte` com valor padrão de `15`. Para que o ASF funcione, ele vai checar os jogos coletados no intervalo de tempo em minutos informado em `FarmingDelay`, para ver se todas as cartas já foram obtidas. Definir um valor muito baixo para pode resultar em uma quantidade excessiva de solicitações sendo enviadas para a Steam, enquanto um valor muito alto pode resultar no ASF ainda coletando dado título até acabar os minutos do `FarmingDelay` sendo que o jogo já foi totalmente explorado. O valor padrão deve ser excelente para a maioria dos usuários, mas se você tem muitos bots em execução, você pode considerar a aumentá-lo para algo como `30` minutos, a fim de limitar a quantidade de pedidos sendo enviados para a Steam. É bom notar que o ASF usa o mecanismo baseado em evento e verifica a página de insígnia do jogo para cada item recebido, então em geral **não precisamos nem verificar isso em intervalos de tempo fixo**, mas como não confiamos plenamente na rede Steam - nós checamos a página de insígnia do jogo, caso não tenhamos checado através do recebimento de cartas a cada último minuto informado em `FarmingDelay` (no caso da rede Steam não nos informar sobre o item recebido ou coisas assim). Assumindo que a rede Steam está funcionando corretamente, diminuir este valor **não vai melhorar a eficiência da coleta de jeito algum**, enquanto **aumentará a significativamente a sobrecarga de rede** - é recomendado aumentá-lo apenas (se necessário) do padrão de `15` minutos. A menos que você tenha uma razão muito **forte** para editar essa propriedade, você deve mantê-la padrão.

* * *

`GiftsLimiterDelay` - tipo `byte` com valor padrão de `1`. A rede Steam, em geral, inclui várias taxas-limite de pedidos semelhantes, portanto devemos adicionar algum atraso extra a fim de evitar ativar essa limitação que nos impediria de interagir com o serviço. O ASF irá garantir que haverá pelo menos o número de segundos de atrazo definido em `ConfirmationsLimiterDelay` entre duas solicitações consecutivas de manipulação (resgate) de presentes/chaves/licenças. Além disso, ele também será usado como limitador global para solicitações de lista de jogos, tal qual emitida pelo **[comando](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)** `owns`. A menos que você tenha uma razão muito **forte** para editar essa propriedade, você deve mantê-la padrão.

* * *

`Headless` - tipo `bool` com valor padrão `false`. Esta propriedade define se o processo deve ser executado em modo não-interativo. Quando estiver no modo não-interativo, o ASF pressupõe que está sendo executado em um servidor ou em outro ambiente não-interativo, portanto ele não tentará ler credenciais cruciais de conta, como o código 2FA, código SteamGuard, senha ou qualquer outra variável necessária para o ASF operar. Isto tem a mesma função que deixar o console do ASF como somente leitura. O modo `Não-interativo` é útil principalmente para usuários que executam o ASF em seus servidores, já que em vez de pedir, p. ex., o código 2FA, o ASF silenciosamente abortará a operação, parando uma conta. A menos que você esteja executando o ASF em um servidor, e você confirmou anteriormente que o ASF é capaz de operar em modo interativo, você deve manter essa propriedade desabilitada. Qualquer interação do usuário será negada quando no modo não-interativo, e suas contas não serão executadas se elas exigem **qualquer** entrada no console durante a inicialização. Isso é útil para servidores, já que o ASF pode abortar tentando fazer logon para a conta quando solicitado por credenciais, em vez de esperar (infinitamente) para que o usuário os forneça. Ativando esse modo também permitirá que você use **[comando](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)** `input` que funciona como um substituto para a entrada do console padrão. Se não tem certeza de como definir essa propriedade, deixá-lo com o valor padrão `false`.

Se você estiver executando o ASF no servidor, convém usar essa opção em conjunto com o **[argumento de linha de comando](https://github.com/JustArchi/ArchiSteamFarm/wiki/Command-line-arguments)** `--process-required`.

* * *

`IdleFarmingPeriod` - tipo `byte` com valor padrão de `8`. Quanto o ASF não tem o que coletar, ele vai checar periodicamente a cada hora informada em `FarmingDelay`, para ver se todas as cartas já foram obtidas. Esse recurso não é necessário quando se fala em jogos novos, já que o ASF é inteligente o bastante para verificar automaticamente a página de insígnias neste caso. O `IdleFarmingPeriod` existe, principalmente, para situações onde jogos antigos que já possuímos passem a ter cartas. Neste caso não há evento algum, por isso o ASF deve verificar periodicamente a página de insígnias se queremos ter tudo sob controle. O valor de `0` desativa este recurso. Veja também: `ShutdownOnFarmingFinished`.

* * *

`InventoryLimiterDelay` - tipo `byte` com valor padrão de `3`. A rede Steam, em geral, inclui várias taxas-limite de pedidos semelhantes, portanto devemos adicionar algum atraso extra a fim de evitar ativar essa limitação que nos impediria de interagir com o serviço. O ASF irá garantir que haverá pelo menos o número de segundos de atrazo definido em `InventoryLimiterDelay` entre duas solicitações consecutivas do inventário - aquelas que estão sendo usadas para buscar seu próprio inventário (e apenas isso). O valor padrão de `3` foi definido com base na coleta de mais de 100 contas bot, e deve satisfazer a maioria dos usuários (se não todos). No entanto você pode querer reduzi-lo, ou até mesmo mudar para `0` se você tiver uma quantidade muito pequena de bots, então o ASF irá ignorar o atraso e coletar os inventários steam muito mais rápido. Esteja avisado, porém, que definir um valor muito baixo **irá** resultar na Steam banir temporariamente seu IP, o que irá te impedir de buscar seu inventário. Você talvez precise aumentar esse valor se você estiver executando um monte de bots com muitos pedidos de inventário, embora neste caso você deve provavelmente tentar limitar o número dessas solicitações em vez disso. A menos que você tenha uma razão muito **forte** para editar essa propriedade, você deve mantê-la padrão.

* * *

`IPC` - tipo `bool` com valor padrão `false`. Esta propriedade define se o servidor **[IPC](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC)** do ASF deve iniciar juntamente com o processo. IPC permite a comunicação entre processos inicializando um servidor HTTP local em `IPCPrefixes` especificados. Se você não vai fazer uso do servidor do IPC do ASF, então não há razão para que você habilite esta opção.

* * *

`IPCPassword` - tipo `string` com valor padrão `null`. Esta propriedade define a senha obrigatória para cada chamada feita através de IPC e serve como uma medida de segurança extra. Quando definido como valor não vazio, todas solicitações através do IPC exigirão uma propriedade extra `password` definida como a senha especificada aqui. O valor padrão `null` vai pular a necessidade de senha, fazendo o ASF respeitar todas as consultas. Adicionalmente, habilitar essa opção também habilita o mecanismo IPC anti-força bruta embutido, que vai banir temporariamente o `IPAddress` definido depois desse enviar muitos pedidos não autorizados em um curto espaço de tempo. A menos que você tenha uma razão para editar essa propriedade, você deve mantê-la padrão.

* * *

`IPCPrefixes` - tipo `ImmutableHashSet <string>` com valor padrão de `http://127.0.0.1:1242 /` item. Esta propriedade especifica os prefixos que serão usados pelo `HttpListener` do ASF na interface **[IPC](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC)**. Em outras palavras, esta propriedade especifica em quais endpoints a interface ASF IPC irá escutar solicitações de entrada. Para obter informações gerais sobre prefixos `HttpListener`, consulte o **[MSDN](https://docs.microsoft.com/en-us/dotnet/api/system.net.httplistener?view=netstandard-2.0#Remarks)**.

> Um prefixo URI é composto de um esquema (http ou https), um host, uma porta opcional e um caminho opcional. Um exemplo de uma cadeia de caracteres de prefixo completa é `http://www.contoso.com:8080/CustomerData/`. Prefixos devem terminar com uma barra (`/`). O objeto HttpListener com o prefixo que mais se aproxima de uma URI solicitada responde à solicitação. Múltiplos objetos HttpListener não podem adicionar o mesmo prefixo; uma exceção Win32Exception é acionada se um HttpListener adiciona um prefixo que já estiver em uso.
> 
> Quando uma porta é especificada, o elemento de host pode ser substituído com `*` para indicar que o HttpListener aceita solicitações enviadas para a porta se o URI solicitado não coincide com qualquer outro prefixo. Por exemplo, para receber todas as solicitações enviadas para a porta 8080 quando o URI solicitado não é tratado por qualquer HttpListener, o prefixo é `http://*:8080/`. Da mesma forma, para especificar que o HttpListener aceita todos os pedidos enviados para uma porta, substitua o elemento de host com o caractere `+`, `https://+:8080`. Os caracteres `*` e `+` podem estar presentes no prefixos que incluem caminhos.
> 
> À partir do .NET 4.5.3 e do Windows 10, subdomínios coringas são suportados em prefixos URI que são gerenciados por um objeto HttpListener. Para especificar um subdomínio coringa, use o caractere `*` como parte do nome do host em um prefixo URI: por exemplo, `http://*.foo.com/` e passe isso como argumento para o método HttpListenerPrefixCollection.Add. Isto funcionara no .NET 4.5.3 e Windows 10; em versões anteriores, isso geraria um HttpListenerException

O ASF, por padrão, escuta apenas o endereço `127.0.0.1` para garantir que nenhuma máquina além da sua possa acessá-lo. Esta é uma medida de segurança, já que acessar a interface IPC pode levar a um atacante assumir o seu processo ASF, que pode ter efeitos dramáticos. No entanto, se você sabe o que está fazendo, por exemplo, você irá restringir acesso ao IPC por sua conta, usando algo como o `iptables` ou outro tipo de firewall, você poderá alterar esta propriedade (por seu próprio risco) para algo menos restritivo, tal como `*` que permite o IPC em todas as interfaces de rede. Você pode querer também alterar a porta padrão do ASF IPC para qualquer outra, portas sugeridas são acima de `1024`, já que portas `0-1024` normalmente exigem privilégios de `root` em sistemas operacionais do tipo Unix. Também tenha em mente que alguns endereços para audição podem exigir privilégios extras, por exemplo, vinculando a interface pública no Windows requer que o ASF seja executado como administrador (ou política `netsh` adequada).

Se sua máquina suporta IPv6, você pode querer adicionar um valor de `http://[::1]:1242/` para esta coleção para escutar tanto no endereço IPv4 local `127.0.0.1`, quanto no IPv6 local `:: 1`. Isto é especialmente importante se você deseja acessar o ASF através do nome `localhost`, já que neste caso sua máquina pode querer usar IPv6 por padrão.

Tenha em mente que essa propriedade além de vincular endereço, também especifica **URLs** válidos sob os quais a interface IPC é acessível. Em outras palavras, se você deseja acessar sua interface IPC à partir do hostname `asf.example.com`, então você deve definir `http://asf.example.com:1242 /` em seu `IPCPrefixes`. Mesmo se você tornar sua interface IPC acessível (por exemplo, alterando o `127.0.0.1` para seu endereço de IP público), você receberá o erro `404 NotFound` se a URL que você está tentando acessr não estiver definida no `IPCPrefixes` do ASF. É por isso que você deve definir no `IPCPrefixes` todas as URLs sob as quais a interface IPC deve ser acessível e isso pode incluir IPv4 local e endereços IPv6, endereços públicos IPv4 e IPv6, e hostname personalizado, totalizando 5 `IPCPrefixes` diferentes no total, Se você exigir/quiser acessar a interface do IPC de todas essas 5 maneiras. Claro, você também pode definir apenas `http://*:1242/`, mas isso nem sempre deve ser apropriado.

Por favor, note que, atualmente, o ASF não aceita nenhum outro caminho de prefixo além da raiz (`/`). Usar um caminho não-raiz resultará em erro `404`. Esperamos resolver essa limitação eventualmente no futuro.

A menos que você tenha uma razão para editar essa propriedade, você deve mantê-la padrão.

* * *

`LoginLimiterDelay` - tipo `byte` com valor padrão de `10`. A rede Steam, em geral, inclui várias taxas-limite de pedidos semelhantes, portanto devemos adicionar algum atraso extra a fim de evitar ativar essa limitação que nos impediria de interagir com o serviço. O ASF irá garantir que haverá pelo menos o número de segundos de atrazo definido em `LoginLimiterDelay` entre duas solicitações consecutivas de conexão. O valor padrão de `10` foi definido com base na conexão de mais de 100 contas bot, e deve satisfazer a maioria dos usuários (se não todos). No entanto você pode querer reduzir/aumentar o valor, ou até mesmo mudar para `0` se você tiver uma quantidade muito pequena de bots, então o ASF irá ignorar o atraso e conectar com a Steam muito mais rápido. Esteja avisado, porém, que definir um valor muito baixo tendo muitos bots **irá** resultar na Steam banir temporariamente seu IP, e isso irá te impedir de logar **de qualquer jeito**, com o erro `InvalidPassword/RateLimitExceeded` - e isso também inclui seu cliente Steam normal, não apenas o ASF. Da mesma forma, se você estiver rodando um numero excessivo de bots, especialmente junto com outros clientes/ferramentas Steam usando o mesmo endereço IP, provavelmente você precisará aumentar esse valor para espalhar os logins em um período longo de tempo.

Como nota, esse valor também é utilizado como amortecedor de balanceamento de carga em todas as ações regulares do ASF, tais como trocas em `SendTradePeriod`. A menos que você tenha uma razão muito **forte** para editar essa propriedade, você deve mantê-la padrão.

* * *

`MaxFarmingTime` - tipo `byte` com valor padrão de `10`. Como você deve saber, a Steam não está sempre funcionando corretamente, às vezes situações estranhas podem acontecer como a steam não gravando nosso tempo de jogo, apesar de estarmos jogando. O ASF permitirá a coleta um único jogo no modo solo pelo tempo máximo, em horas, determinado em `MaxFarmingTime`, e vai considerá-lo totalmente explorado após esse período. Isso é necessário para que o processo de coleta não congele em caso de situações estranhas, mas também, se por algum motivo a Steam lançar uma nova insignia que poderia parar o progresso do ASF (veja: `Blacklist`). O valor padrão de `10` horas deve ser suficiente para adquirir todas as cartas Steam de um jogo. Definir um valor muito baixo para essa propriedade pode resultar em jogos válidos sendo ignorados (e sim, há jogos válidos que levam até 9 horas para coleta), enquanto definir um valor muito alto pode resultar no congelamento do processo de coleta. Por favor, note que esta propriedade afeta somente um único jogo em uma sessão de coleta simples (por isso, após passar por toda a fila, o ASF retornará a esse título), ela também não é baseada no total de tempo jogado, mas no tempo de coleta interno do ASF. A menos que você tenha uma razão muito **forte** para editar essa propriedade, você deve mantê-la padrão.

* * *

`MaxTradeHoldDuration` - tipo `byte` com valor padrão de `15`. Esta propriedade define a duração máxima da retenção de trocas, em dias, para analisarmos a disposição em aceitar - o ASF rejeitará trocas que estejam sendo retidas por mais dias que o definido em `MaxTradeHoldDuration`. Esta opção só faz sentido para bots com `TradingPreferences` do `SteamTradeMatcher`, já que isso não afeta as trocas entre `Master`/`SteamOwnerID`, nem doações. Trocas retidas são irritantes para todos, e ninguém quer lidar com elas. É suposto que o ASF funcione com regras liberais e ajude a todos, independentemente de trocas retidas ou não - é por isso que essa opção é definida como `15` por padrão. No entanto, se você preferir rejeitar todas as transações afetadas pela retenção de trocas, você pode definir `0` aqui. Por favor, considere o fato de que cartas com vida curta não são afetadas por essa opção e são automaticamente rejeitadas para pessoas com retenção de trocas, conforme descrito na seção **[trocas](https://github.com/JustArchi/ArchiSteamFarm/wiki/Trading)**, então não há nenhuma necessidade de rejeitar globalmente todo mundo só por causa disso. A menos que você tenha uma razão para editar essa propriedade, você deve mantê-la padrão.

* * *

`OptimizationMode` - tipo `byte` com valor padrão de `0`. Esta propriedade define o modo de otimização que o ASF dará preferência durante o tempo de execução. Atualmente, o ASF suporta dois modos - `0`, que é chamado de `MaxPerformance` e `1`, que é chamado `MinMemoryUsage`. Por padrão, o ASF prefere executar tantas coisas em paralelo (simultaneamente) quanto possível, melhorando o desempenho pelo balanceamento de carga de trabalho entre todos os núcleos de CPU, vários threads de CPU, soquetes múltiplos e várias tarefas de pool de threads. Por exemplo, o ASF pedirá por sua primeira página de insígnias quando procurar jogos para executar, e então uma vez que o pedido chegar, o ASF lerá quantas páginas de insígnias você tem, e em seguida, solicitará todas as outras simultaneamente. Isto é o que você deve querer **quase sempre**, já que a sobrecarga, na maioria dos casos, é mínima e os benefícios do código assíncrono do ASF pode ser visto até mesmo em hardwares mais antigos com um único núcleo de CPU e potência fortemente limitada. No entanto, com muitas tarefas sendo processadas em paralelo, o tempo de execução do ASF é responsável pela manutenção, p. ex., mantendo soquetes abertos, threads ativos e tarefas sendo processadas, que pode resultar no aumento do uso de memória de vez em quando, e se você é extremamente limitado pela memória disponível, você pode querer mudar esta propriedade para `1` (`MinMemoryUsage`) a fim de forçar o ASF a usar o mínimo de tarefas possível e normalmente executar possíveis códigos assíncronos paralelos de forma síncrona. Você deve considerar mudar essa propriedade somente se você leu anteriormente a **[configuração de pouca memória](https://github.com/JustArchi/ArchiSteamFarm/wiki/Low-memory-setup)** e intencionalmente quer sacrificar um aumento enorme de desempenho, para uma diminuição de sobrecarga de memória muito pequena. Normalmente, esta opção é **muito pior** do que o que se pode conseguir com outras possibilidades, tais como limitando o uso do ASF ou ajustando o coletor de lixo do tempo de execução, conforme explicado na **[configuração de pouca memória](https://github.com/JustArchi/ArchiSteamFarm/wiki/Low-memory-setup)**. Portanto, você deve usar `MinMemoryUsage` como **último recurso**, direitamente antes de recompilação do tempo de execução, caso você não tenha conseguido atingir resultados satisfatórios com outras opções (muito melhores). A menos que você tenha uma razão muito **forte** para editar essa propriedade, você deve mantê-la padrão.

* * *

`Statistics` - tipo `bool` com valor padrão `true`. Esta propriedade define se ASF deve ter estatísticas habilitadas. Uma explicação detalhada do que exatamente essa opção faz está disponível na seção de **[estatísticas](https://github.com/JustArchi/ArchiSteamFarm/wiki/Statistics)**. A menos que você tenha uma razão para editar essa propriedade, você deve mantê-la padrão.

* * *

`SteamMessagePrefix` - tipo `string` com valor padrão `"/me "`. Esta propriedade define um prefixo que será anexado a todas as mensagens da Steam sendo enviadas pelo ASF. Por padrão o ASF usa o prefixo `"/ me"` para distinguir as mensagens do bot mais facilmente, mostrando-os em cores diferentes no chat da Steam. Outro prefixo digno de menção é `"/ pre"` que alcança resultados semelhantes, mas usa uma formatação diferente. Você também pode definir essa propriedade como vazia ou `null` para desativar inteiramente o uso do prefixo e deixar todas as mensagens do ASF de forma tradicional. É bom notar que esta propriedade afeta apenas mensagens da Steam - as respostas retornadas através de outros canais (como IPC) não são afetadas. A menos que você deseja personalizar o comportamento padrão do ASF, é uma boa ideia deixá-lo assim.

* * *

`SteamOwnerID` - tipo `ulong` com valor padrão `0`. Esta propriedade define o ID Steam em formato de 64-bit do proprietário do processo ASF, e é muito semelhante a permissão `Master` de determinada conta bot (mas global mas em vez disso). Você vai querer definir quase sempre essa propriedade com o ID da sua própria conta principal da Steam. A permissão `Master` inclui controle total sobre sua conta bot, mas comandos globais como `exit`, `restart` ou `update` são reservados apenas para o `SteamOwnerID`. Isto é conveniente, pois você pode querer executar bots para seus amigos, mas não lhes permitir controle do processo ASF, tal como sair dele através do comando `exit`. O valor padrão de `0` especifica que não há proprietário do processo do ASF, o que significa que ninguém será capaz de emitir comandos globais do ASF. Tenha em mente que o **[IPC](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC)** trabalha com `SteamOwnerID`, então se você quiser usá-lo você deve fornecer um valor válido aqui.

* * *

`SteamProtocols` - tipo `byte flags` com valor padrão de `5`. Esta propriedade define os protocolos Steam que o ASF irá usar ao se conectar aos servidores Steam, que são definidos como abaixo:

| Valor | Nome      | Descrição                                                                                        |
| ----- | --------- | ------------------------------------------------------------------------------------------------ |
| 0     | None      | Sem protocolo                                                                                    |
| 1     | TCP       | **[Transmission Control Protocol](https://en.wikipedia.org/wiki/Transmission_Control_Protocol)** |
| 2     | UDP       | **[User Datagram Protocol](https://en.wikipedia.org/wiki/User_Datagram_Protocol)**               |
| 4     | WebSocket | **[WebSocket](https://en.wikipedia.org/wiki/WebSocket)**                                         |

Por favor note que esta propriedade é um campo do tipo `flags`, portanto é possível escolher qualquer combinação de valores disponíveis. Confira **[mapeamento flags](#json-mapping)** se você quiser aprender mais. Não habilitar nenhum flag resulta na opção `None`, e essa opção é inválida, por si só.

Por padrão ASF deve usar todos os protocolos Steam disponíveis como uma medida para lutar contra tempos de inatividade e outras questões semelhantes da Steam. Normalmente você vai querer alterar essa propriedade se você desejar limitar o ASF para usar apenas um ou dois protocolos específicos, em vez de todos os que estão disponíveis. Tal medida poderia ser necessário se você está habilitando, por exemplo, apenas tráfego TCP em seu firewall e você não quer que o ASF tente conectar via UDP. No entanto, a menos que você está depurando determinado problema ou questão, você quase sempre vai desejar garantir que o ASF esteja livre para usar qualquer protocolo que é atualmente suportado e não apenas um ou dois. A menos que você tenha uma razão muito **forte** para editar essa propriedade, você deve mantê-la padrão.

No momento, as configurações padrão não incluem o protocolo UDP devido ao problema **[#882](https://github.com/JustArchi/ArchiSteamFarm/issues/882)**.

* * *

`UpdateChannel` - tipo `byte` com valor padrão de `1`. Essa propriedade define o canal de atualização que está sendo usado, tanto para atualizações automáticas (se `UpdatePeriod` for maior que `0`), ou (caso contrário) para atualizar notificações. Atualmente, o ASF suporta três canais de atualização - `0`, que é chamado de `Nenhum`, `1`, que é chamado de `Estável` e `2`, que é chamado de `Experimental`. O canal `Estável` é o canal de lançamento padrão, que deve ser usado pela maioria dos usuários. O canal `Experimental`, além de versões `Estáveis`, também inclui versões de **pré-lançamento** dedicado a usuários avançados e outros desenvolvedores para testar novas funcionalidades, confirmar correções de bugs ou dar feedback sobre melhorias planejadas. **Versões experimentais frequentemente contém erros sem correção, trabalhos em andamento ou implementações reescritas**. Se você não se considera um usuário avançado, por favor, fique com o canal de atualização padrão `1` (Estável). O canal `Experimental` dedica-se aos usuários que sabem como relatar bugs, lidar com questões e dar feedback - nenhum suporte técnico será dado. Confira o **[ciclo de lançamento](https://github.com/JustArchi/ArchiSteamFarm/wiki/Release-cycle)** do ASF se você quiser aprender mais. Você também pode definir `UpdateChannel` como `0` (`Nenhum`), se você quiser remover completamente todas as verificações de versão. Definir `UpdateChannel` como `0` desativará inteiramente toda funcionalidade relacionada às atualizações, incluindo o comando `update`. Usando o canal `Nenhum` é **fortemente desencorajado** devido ao fato de te expor a todo tipo de problemas (mencionados na descrição de `UpdatePeriod` abaixo).

**A não ser que você saiba o que está fazendo**, nós recomendamos **fortemente** que você mantenha o valor padrão.

* * *

`UpdatePeriod` - tipo `byte` com valor padrão de `24`. Esta propriedade define quantas vezes o ASF deve buscar por atualizações automáticas. As atualizações são cruciais não só para receber novos recursos e correções de segurança críticas, mas também para receber correções de bugs, melhorias de desempenho, melhorias de estabilidade e muito mais. Quando um valor maior que `0` é definido, o ASF irá automaticamente baixar, substituir e se reiniciar (se a configuração `AutoRestart` permitir) quando uma nova atualização estiver disponível. Para isso, o ASF verificará a cada intervalo de horas indicado em `UpdatePeriod` se uma nova atualização está disponível no nosso repositório GitHub. Um valor de `0` desativa atualizações automáticas, mas ainda permite que você execute o comando `update` manualmente. Você também pode estar interessado em configurar adequadamente o `UpdateChannel` que o `UpdatePeriod` deve seguir.

O processo de atualização do ASF envolve atualização da estrutura inteira da pasta que o ASF está usando, mas sem mexer em suas configurações ou bancos de dados localizados na pasta `config` - isso significa que qualquer arquivos extras não relacionados com o ASF em sua pasta podem ser perdidos durante a atualização. O valor padrão de `24` é um bom equilíbrio entre verificações desnecessárias e uma versão do ASF suficientemente atual.

A menos que você tenha um motivo muito **forte** para desabilitar esse recurso, você deve manter as atualizações automáticas habilitadas dentro de um período de tempo razoável definido em `UpdatePeriod` **para o seu próprio bem**. Não só porque não damos suporte a outra versão do ASF senão a última estável, mas também porque **nós damos a nossa garantia de segurança somente para a versão mais recente**. Se você estiver usando uma versão desatualizada do ASF então está intencionalmente se expondo a todo o tipo de problemas, desde pequenos bugs, funcionalidades quebradas, até possíveis **[suspensões permanentes de conta Steam](https://github.com/JustArchi/ArchiSteamFarm/wiki/FAQ#did-somebody-get-banned-for-it)**, então nós **recomendamos fortemente**, para seu próprio bem, que você se certifique sempre de que a sua versão do ASF esteja atualizada. Atualizações automáticas nos permitem reagir rapidamente a todo o tipo de questões, desativando ou remendando códigos problemáticos antes que ele possa se agravar - se você optar por ficar fora disso, você perde todas nossas garantias de segurança e corre o risco das consequências de executar um código que pode ser potencialmente prejudicial, não somente a rede Steam, mas também (por definição) para sua própria conta Steam.

* * *

`WebLimiterDelay` - tipo `ushort` com valor padrão de `200`. Esta propriedade define, em milissegundos, a quantidade mínima de atraso entre o envio de dois pedidos consecutivos para o mesmo serviço web da Steam. Tal atraso é necessário pois o serviço **[AkamaiGhost](https://www.akamai.com)**, que a Steam usa internamente, inclui um limitador com base no número global de solicitações enviadas em dado período de tempo. Em circunstâncias normais o bloqueio da akamai é um pouco difícil de atingir, mas sob cargas de trabalho muito pesadas com uma enorme fila de pedidos em andamento, é possível acioná-lo, se continuarmos enviando muitas solicitações por um período muito curto de tempo.

O valor padrão foi definido com base no pressuposto de que o ASF é a única ferramenta acessando os serviços web da Steam, em particular `steamcommunity.com`, `api.steampowered.com` e `store.steampowered.com`. Se você estiver usando outras ferramentas que estejam enviando solicitações para os mesmos serviços web então certifique-se de que sua ferramenta inclui funcionalidade semelhante a `WebLimiterDelay` e defina ambos com o dobro do valor padrão, que seria `400`. Isso garante que, sob nenhuma circunstância, você vai exceder o envio de mais de 1 pedido por cada 200 ms.

Em geral, reduzir o `WebLimiterDelay` para um valor abaixo do padrão é **fortemente desencorajado**, pois pode levar a vários bloqueios relacionados ao IP, alguns passíveis de serem permanentes. O valor padrão é bom o suficiente para rodar uma instância única do ASF no servidor, bem como usar o ASF em um cenário normal juntamente com o cliente Steam original. Ele deve ser o correto para a maioria dos usos e você só deve aumentá-lo (nunca diminuir), se - além de usar o ASF, você também estiver usando outra ferramenta que pode enviar um número excessivo de pedidos para os mesmos serviços web que o ASF esteja usando. Em resumo, o número global de todas as solicitações enviadas a partir de um único IP para um único domínio da Steam nunca deve exceder mais de 1 pedido por 200 ms.

A menos que você tenha uma razão para editar essa propriedade, você deve mantê-la padrão.

* * *

`WebProxy` - tipo `string` com valor padrão `null`. Esta propriedade define um endereço proxy da web que será usado para todas as solicitações internas, http e https, enviadas pelo `HttpClient` do ASF, especialmente para serviços como `github.com`, `steamcommunity.com` e `store.steampowered.com`. Solicitações de proxy para o ASF em geral não tem vantagens, mas é extremamente útil para contornar vários tipos de firewalls, especialmente o grande firewall da China.

Essa propriedade é definida como uma sequência de caracteres uri:

> Uma sequência de caracteres URI é composta de um esquema (http ou https), um host, e uma porta opcional. Um exemplo de uri completa é `http://www.contoso.com:8080`.

Se seu proxy requer autenticação de usuário, você também precisará configurar `WebProxyUsername` e/ou `WebProxyPassword`. Se não há tal necessidade, configurar apenas esta propriedade é suficiente.

No momento o ASF usa o web proxy somente para solicitações `http` e `https`, que **não** incluem comunicação de rede interna da Steam feita dentro cliente Steam interno do ASF. Atualmente não existem planos para oferecer suporte a isso, principalmente devido à falta de funcionalidade do **[SK2](https://github.com/SteamRE/SteamKit)**. Se você precisa/quer que isso aconteça, eu sugiro que comece por lá.

A menos que você tenha uma razão para editar essa propriedade, você deve mantê-la padrão.

* * *

`WebProxyPassword` - tipo `string` com valor padrão `null`. Esta propriedade define a senha usada em autenticações basic, digest, NTLM e Kerberos que sejam suportadas pela máquina de `WebProxy` alvo, fornecendo a funcionalidade proxy. Se o seu proxy não requer credenciais de usuário, não há nenhuma necessidade de você colocar qualquer coisa. Usar esta opção só faz sentido se `WebProxy` for usado também, já que ele não tem efeito caso contrário.

A menos que você tenha uma razão para editar essa propriedade, você deve mantê-la padrão.

* * *

`WebProxyUsername` - tipo `string` com valor padrão `null`. Esta propriedade define o usuário usado em autenticações basic, digest, NTLM e Kerberos que sejam suportadas pela máquina de `WebProxy` alvo, fornecendo a funcionalidade proxy. Se o seu proxy não requer credenciais de usuário, não há nenhuma necessidade de você colocar qualquer coisa. Usar esta opção só faz sentido se `WebProxy` for usado também, já que ele não tem efeito caso contrário.

A menos que você tenha uma razão para editar essa propriedade, você deve mantê-la padrão.

* * *

**[Voltar para o topo](#configuration)**

* * *

## Configuração do Bot

Como você já deve saber, cada bot deve ter sua própria configuração. Um exemplo de configuração bot está incluso no arquivo `example.json`, que deve ser usado para configuração do bot. Simplesmente **copie e cole** o `example.json` para um novo arquivo e lembre-se de nomeá-la adequadamente, pois ela será sua conta bot. Você deve começar configurando sua conta **principal**, portanto, algumas boas sugestões para o nome do arquivo é `primary.json`, `1. json` ou `SeuApelido.json`.

**Observação:** Há alguns nomes que são reservados e não podem ser usados para nomear configurações de bot. São eles: **ASF**, **example** e **minimal**. O ASF vai ignorar arquivos de configuração com esses nomes. O ASF também ignora arquivos cujo nome comece com um ponto.

Depois de decidir como você deseja nomear seu bot, abra o arquivo e comece a configuração. Você vai notar a seguinte estrutura:

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
    "LootableTypes": [
        1,
        3,
        5
    ],
    "MatchableTypes": [
        5
    ],
    "OnlineStatus": 1,
    "PasswordFormat": 0,
    "Paused": false,
    "RedeemingPreferences": 0,
    "SendOnFarmingFinished": false,
    "SendTradePeriod": 0,
    "ShutdownOnFarmingFinished": false,
    "SteamLogin": null,
    "SteamMasterClanID": 0,
    "SteamParentalPIN": "0",
    "SteamPassword": null,
    "SteamTradeToken": null,
    "SteamUserPermissions": {},
    "TradingPreferences": 0,
    "UseLoginKeys": true
}
```

**Dica:** Para que o bot funcione corretamente, você deve editar ao menos as propriedades `Enabled`, `SteamLogin` e `SteamPassword`. Eu também sugiro dar uma olhada em alguns ajustes, tal qual `HoursUntilCardDrops`, mas isso é opcional. As configurações do ASF são avançadas o suficiente para permitir que você ajuste seus bots e o próprio ASF da forma desejada, se você não "faz questão" de um ajuste tão avançado, você realmente não precisa se aprofundar em cada propriedade de configuração. É você quem decide o quão simples ou complexo o ASF deve ser.

* * *

Todas as opções são explicadas abaixo:

`AcceptGifts` - tipo `bool` com valor padrão `false`. Quando habilitado, o ASF vai aceitar e resgatar automaticamente todos os presentes Steam (incluindo vales-presente da carteira) enviados para o bot. Isso também inclui presentes enviadas de usuários que não estejam os definidos em `SteamUserPermissions`. Tenha em mente que presentes enviados para o endereço de e-mail não são encaminhados diretamente para o cliente, então o ASF não aceitará esses sem a sua ajuda.

Esta opção é recomendada apenas para contas alternativas, já que é muito provável que você não queira resgatar automaticamente todos os presentes enviados para sua conta principal. Se você não tiver certeza se quer esse recurso habilitado ou não, mantenha-o com o valor `false` padrão.

* * *

`AutoSteamSaleEvent` - tipo `bool` com valor padrão `false`. Durante as promoções de verão/Inverno é comum que a Steam conceda cartas extras pela exploração diária da lista de descobrimento, bem como votando para os prêmios. Quando esta opção é habilitada, o ASF verificará automaticamente verificar a lista de descobrimento e os prêmios Steam a cada 6 horas e limpá-las, se necessário. Esta opção não é recomendada se você quer fazer essas ações manualmente, e normalmente ela deve fazer sentido apenas nas contas bot. Além disso, você precisa garantir que sua conta seja pelo menos de nível `8` se você espera receber essas cartas em primeiro lugar. Se você não tiver certeza se quer esse recurso habilitado ou não, mantenha-o com o valor `false` padrão.

Por favor, note que devido a constantes problemas e mudanças na Steam, **nós damos nenhuma garantia de que esta função vai funcionar corretamente**, portanto, é inteiramente possível que esta opção **não funcione de modo algum**. Não aceitamos **nenhum** relatórios de bugs, nem pedidos de suporte para essa opção. Ele é oferecido com absolutamente nenhuma garantia, você está usando por seu próprio risco.

* * *

`BotBehaviour` - tipo `byte flags` com valor padrão de `0`. Esta propriedade define o comportamento do bot do ASF durante vários eventos e é definida como abaixo:

| Valor | Nome                          | Descrição                                                                         |
| ----- | ----------------------------- | --------------------------------------------------------------------------------- |
| 0     | None                          | Nenhum comportamento especial do bot, o modo menos invasivo, padrão               |
| 1     | RejectInvalidFriendInvites    | Fará com que o ASF rejeite (ao invés de ignorar) convites de amizade inválidos    |
| 2     | RejectInvalidTrades           | Fará com que o ASF rejeite (ao invés de ignorar) ofertas de troca inválidas       |
| 4     | RejectInvalidGroupInvites     | Fará com que o ASF rejeite (ao invés de ignorar) convites inválidos para grupos   |
| 8     | DismissInventoryNotifications | Fará com que o ASF dispense automaticamente todas as notificações de inventário   |
| 16    | MarkReceivedMessagesAsRead    | Fará com que o ASF marque automaticamente todas as mensagens recebidas como lidas |

Por favor note que esta propriedade é um campo do tipo `flags`, portanto é possível escolher qualquer combinação de valores disponíveis. Confira **[mapeamento flags](#json-mapping)** se você quiser aprender mais. Não habilitar nem um flag resultará na opção `None`.

Em geral você vai desejar modificar esta propriedade se você espera que o ASF faça certa quantidade de automação relacionados à sua atividade, como seria de esperar de uma conta bot, mas não uma conta primária usada no ASF. Portanto, alterar esta propriedade faz sentido principalmente para contas alternativas, embora você seja livre para usar as opções selecionadas para contas principais também.

O comportamento normal do ASF (`None`) existe apenas para automatizar as coisas que o usuário quer (p. ex., coleta de cartas ou propostas do `SteamTradeMatcher`, se definido em `TradingPreferences`). Este é o modo menos invasivo, e é benéfico para a maioria dos usuários já que você permanece com controle completo sobre sua conta e pode decidir se deseja permitir certas interações fora do escopo ou não.

Convite de amizade inválido é um convite que não vem de um usuário com permissão de `FamilySharing` (definida em `SteamUserPermissions`) ou maior. No modo normal o ASF ignora esses convites, como seria de esperar, dando-lhe liberdade de escolha se deseja aceitá-las ou não. `RejectInvalidFriendInvites` fará com que esses convites sejam rejeitados automaticamente, que praticamente irá desativar a opção para outras pessoas te adicionarem lista de amigos delas (uma vez que o ASF negará todos esses pedidos, além das pessoas definidas em ` SteamUserPermissions`). A menos que você queira negar completamente todos os convites de amizade, você não deve habilitar esta opção.

Oferta de troca inválida é uma oferta que não é aceita pelo módulo interno do ASF. Mais sobre este assunto pode ser encontrado na seção **[trocas](https://github.com/JustArchi/ArchiSteamFarm/wiki/Trading)**, que define todos os tipos de trocas que o ASF está disposto a aceitar automaticamente. Trocas válidas também são definidas por outras configurações, especialmente `TradingPreferences`. `RejectInvalidTrades` fará com que todas as ofertas de trocas inválidas sejam rejeitadas, em vez de ser ignoradas. A menos que queira negar completamente todas as ofertas de troca que não são automaticamente aceitas pelo ASF, você não deve habilitar esta opção.

Convite inválidos para grupos são convites que não vem do grupo definido em `SteamMasterClanID`. No modo normal o ASF ignora esses convites, como seria de esperar, dando-lhe liberdade de escolha se deseja aceitar entrar em determinado grupo Steam ou não. `RejectInvalidGroupInvites` fará com que todos os convites para grupos sejam rejeitados automaticamente, efetivamente tornando impossível convidá-lo para qualquer outro grupo que não seja o `SteamMasterClanID`. A menos que você queira negar completamente todos os convites de grupo, você não deve habilitar esta opção.

Se você está inseguro de como configurar esta opção, é melhor deixá-la padrão.

* * *

`CustomGamePlayedWhileFarming` - tipo `string` com valor padrão `null`. Quando ASF está coletando, ele pode se mostrar como "Jogando jogo não Steam: `CustomGamePlayedWhileFarming`" em vez do jogo que realmente está sendo executado. Isso é útil para o caso de você querer que seus amigos saibam que você está coletando, e você não quer mudar o `OnlineStatus` padrão. Por favor, note que o ASF não pode garantir a ordem de exibição atual da rede Steam, portanto isso é apenas uma sugestão que pode, ou não, ser exibida corretamente. O valor `null` padrão desativa este recurso.

* * *

`CustomGamePlayedWhileIdle` - tipo `string` com valor padrão `null`. Similar a `CustomGamePlayedWhileFarming`, mas para a situação onde o ASF não tem nada para fazer (com todas as coletas da conta feitas). O valor `null` padrão desativa este recurso.

* * *

`Enabled` - tipo `bool` com valor padrão `false`. Essa propriedade define se o bot está habilitado. Uma conta bot habilitada (`true`) vai ser iniciada automaticamente quado o ASF for iniciado, enquanto uma conta bot desabilitada (`false`) terá que ser iniciada manualmente. Por padrão, todo bot é desabilitado, então provavelmente você vai querer mudar essa propriedade para `true` para todos os bots que devem ser iniciados automaticamente.

* * *

`FarmingOrders` - tipo `ImmutableHashSet<byte>` com valor padrão vazio. Essa propriedade define a ordem de coleta **preferida** a ser usada pelo ASF para determinada conta bot. Atualmente existem as seguintes ordens disponíveis:

| Valor | Nome                      | Descrição                                                                                                |
| ----- | ------------------------- | -------------------------------------------------------------------------------------------------------- |
| 0     | Unordered                 | Sem ordem, aumentando levemente a porformance da CPU                                                     |
| 1     | AppIDsAscending           | Tenta coletar dos jogos com o `appID` em ordem crescente                                                 |
| 2     | AppIDsDescending          | Tenta coletar dos jogos com o `appID` em ordem decrescente                                               |
| 3     | CardDropsAscending        | Tenta coletar dos jogos com o menor número de cartas faltantes primeiro                                  |
| 4     | CardDropsDescending       | Tenta coletar dos jogos com o maior número de cartas faltantes primeiro                                  |
| 5     | HoursAscending            | Tenta coletar dos jogos com o menor número de horas jogadas primeiro                                     |
| 6     | HoursDescending           | Tenta coletar dos jogos com o maior número de horas jogadas primeiro                                     |
| 7     | NamesAscending            | Tenta coletar dos jogos em ordem alfabética, começando do A                                              |
| 8     | NamesDescending           | Tenta coletar dos jogos em ordem alfabética reversa, começando do Z                                      |
| 9     | Random                    | Tenta coletar dos jogos em ordem completamente aleatória (diferente cada vez que o programa é executado) |
| 10    | BadgeLevelsAscending      | Tenta coletar dos jogos com o nível de insígnia em ordem crescente                                       |
| 11    | BadgeLevelsDescending     | Tenta coletar dos jogos com o nível de insígnia em ordem decrescente                                     |
| 12    | RedeemDateTimesAscending  | Tenta coletar dos jogos mais antigos em sua conta primeiro                                               |
| 13    | RedeemDateTimesDescending | Tenta coletar dos jogos mais novos em sua conta primeiro                                                 |
| 14    | MarketableAscending       | Tenta coletar dos jogos com cartas não comercializáveis primeiro                                         |
| 15    | MarketableDescending      | Tenta coletar dos jogos com cartas comercializáveis primeiro                                             |

Como essa essa propriedade é uma matriz, ela permite que você use várias configurações diferentes em sua ordem fixa. Por exemplo, você pode incluir os valores `15`, `11` e `7` a fim de classificar primeiro por jogos comercializáveis, em seguida, por aqueles com a insígnia de nível mais alto e, finalmente, em ordem alfabética. Como você pode imaginar, a ordem realmente importa, já que a ordem reversa (`7`, `11` e `15`) tem resultado inteiramente diferente. A maioria das pessoas provavelmente usará apenas uma dessas ordens, mas caso você queira, você pode também classificar por parâmetros extras.

Note também a palavra "tentar" em todas as descrições acima - a ordem atual do ASF é fortemente afetada pelo **[algorítimo de coleta de cartas](https://github.com/JustArchi/ArchiSteamFarm/wiki/Performance)** e a classificação afetará apenas os resultados que o ASF considera ser o mesmo com relação a performance. Por exemplo, no algoritmo `Simple`, a ordem de coleta selecionada em `FarmingOrders` deve ser inteiramente respeitada na sessão atual de coleta (uma vez que cada jogo tem o mesmo valor de desempenho), enquanto no `Complex` a ordem atual do algoritmo é afetada primeiro pelas horas, e depois classificada de acordo com escolhido em `FarmingOrders`. Isto levará a resultados diferentes, já que jogos com tempo de jogo existente terão prioridade sobre os outros, então o ASF efetivamente vai preferir jogos com maior tempo primeiro e só então classificando tudo o mais de acordo com sua escolha em `FarmingOrders`. Portanto, esta propriedade de configuração é apenas uma **sugestão** que o ASF tentará respeitar, desde que isso não afete negativamente o desempenho (neste caso, ASF sempre preferirá desempenho sobre `FarmingOrders`).

Há também uma lista de prioridade de coleta acessível através de **[comandos](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)** `iq`. Se ele é usado, a ordem de coleta é classificada em primeiro lugar por desempenho, em seguida pela lista de coleta e, finalmente por sua indicação em `FarmingOrders`.

* * *

`GamesPlayedWhileIdle` - tipo `ImmutableHashSet<uint>` com valor padrão vazio. Se o ASF não tem nada para coletar ele pode jogar seus jogos Steam (`appID`s). Jogar os jogos de tal forma aumenta suas horas "jogadas" nesses jogos, mas nada mais além disso. Esse recurso pode ser habilitado juntamente com `CustomGamePlayedWhileIdle` para poder jogar seus jogos selecionados e, ao mesmo tempo, mostrar um status personalizado na rede Steam, mas neste caso, como no caso de `CustomGamePlayedWhileFarming`, a ordem de exibição real é não é garantida. Por favor, note que a Steam permite que o ASF jogue apenas até `32` `appID`s, portanto, se você colocar mais jogos do que isso, somente os `32` primeiros serão respeitados (e os demais serão ignorados).

* * *

`HoursUntilCardDrops` - tipo `byte` com valor padrão de `3`. Esta propriedade define se a conta tem restrição na coleta de cartas, e caso sim, por quantas horas. Restrição de coleta de cartas significa que a conta não receberá cartas de determinados jogos até que os mesmos sejam jogados por pelo menos a quantidade de horas indicada em `HoursUntilCardDrops`. Infelizmente não existe uma fórmula mágica para detectar isso, então o ASF depende de você. Esta propriedade afeta o **[algorítimo de coleta de cartas](https://github.com/JustArchi/ArchiSteamFarm/wiki/Performance)** que será usado. Configurar essa propriedade corretamente vai maximizar os lucros e minimizar o tempo necessário para que as cartas sejam coletadas. Lembre-se que não há nenhuma resposta óbvia se você deve usar um ou outro valor, desde que depende totalmente da sua conta. Parece que as contas mais antigas que nunca pediram reembolso têm o recebimento de cartas sem restrição, então elas devem usar o valor `0`, enquanto novas contas e aqueles que pediram reembolso tem essa restrição e devem manter o valor em `3`. No entanto isso é apenas uma teoria e não deve ser tomada como regra. O valor padrão para essa propriedade foi definido com base no "mal menor" e a maioria dos casos de uso.

* * *

`IdlePriorityQueueOnly` - tipo `bool` com valor padrão `false`. Esta propriedade define se o ASF deve considerar para coleta automática apenas os apps que você adicionou a fila de prioridade disponível com o comando **[comando](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)** `iq`. Quando esta opção estiver habilitada, o ASF ignorará todos `appIDs` que não estejam na lista, permitindo que você escolha efetivamente os jogos que deseja que o ASF colete automáticamente ASF. Tenha em mente que se você não adicionar nenhum jogo na lista o ASF atuará como se não houvesse nada para coletar na sua conta. Se você não tiver certeza se quer esse recurso habilitado ou não, mantenha-o com o valor `false` padrão.

* * *

`IdleRefundableGames` - tipo `bool` com valor padrão `true`. Esta propriedade define se o ASF tem permissão para rodar jogos que ainda são reembolsáveis. Um jogo reembolsável é um jogo que compramos na loja Steam nas últimas duas semanas e que ainda não foi jogado por mais de 2 horas, como indicado **[aqui](https://store.steampowered.com/steam_refunds)**. Por padrão, quando esta opção estiver definida como `true`, o ASF ignora o período de reembolso inteiramente e coleta tudo, como a maioria das pessoas espera. No entanto, você pode alterar esta opção para `false` se você deseja garantir que o ASF não rode qualquer um dos seus jogos reembolsáveis cedo demais, permitindo que você os avalie peça reembolso caso necessário, sem se preocupar com o ASF afetando negativamente seu tempo de jogo. Observe que se você desabilitar essa opção os jogos que você comprou na loja Steam não serão coletados pelo ASF antes de 14 dias da data da ativação. Se você não tiver certeza se quer esse recurso habilitado ou não, mantenha-o com o valor `true` padrão.

* * *

`LootableTypes` - tipo `ImmutableHashSet <byte>` com valor padrão de tipos de itens Steam `1, 3, 5`. Essa propriedade define o comportamento do ASF quando estiver pilhando - tanto manual quanto automaticamente. O ASF garantirá que apenas itens definidos em `LootableTypes` serão incluídos na oferta de troca, portanto, essa propriedade permite que você escolha o que você deseja receber na uma oferta de troca que está sendo enviada a você.

| Valor | Nome              | Descrição                                                                   |
| ----- | ----------------- | --------------------------------------------------------------------------- |
| 0     | Unknown           | Qualquer tipo que não se encaixa em nenhuma das opções abaixo               |
| 1     | BoosterPack       | Pacote de cartas desempacotado                                              |
| 2     | Emoticon          | Emoticons para uso no Chat Steam                                            |
| 3     | FoilTradingCard   | Versão brilhante da `Carta Colecionável`                                    |
| 4     | ProfileBackground | Fundo de perfil para usar em seu perfil Steam                               |
| 5     | TradingCard       | Cartas colecionáveis Steam, usadas para fabricar insígnias (não brilhantes) |
| 6     | SteamGems         | Gemas Steam usadas para criar pacotes de cartas, incluindo as empacotadas   |

Observe que, independentemente das configurações acima, o ASF só pedirá por itens da comunidade (`contextID` de 6) Steam (`appID` de 753), então todos os itens de jogos, presentes e semelhantes, são excluídos da oferta de troca por definição.

A configuração padrão do ASF baseia-se no uso mais comum do bot, pilhando apenas pacotes de cartas e cartas colecionáveis (incluindo as brilhantes). A propriedade definida aqui permite que você mude esse comportamento da forma que te satisfaça. Please keep in mind that all types not defined above will show as `Unknown` type, which is especially important when Valve releases some new Steam item, that will be marked as `Unknown` by ASF as well, until it's added here (in the future release). That's why in general it's not recommended to include `Unknown` type in your `LootableTypes`, unless you know what you're doing, and you also understand that ASF will send your entire inventory in a trade offer if Steam Network gets broken again and reports all your items as `Unknown`. My strong suggestion is to not include `Unknown` type in the `LootableTypes`, even if you expect to loot everything.

* * *

`MatchableTypes` - `ImmutableHashSet<byte>` type with default value of `5` Steam item types. This property defines which Steam item types are permitted to be matched when `SteamTradeMatcher` option in `TradingPreferences` is enabled. Types are defined as below:

| Valor | Nome              | Descrição                                                                         |
| ----- | ----------------- | --------------------------------------------------------------------------------- |
| 0     | Unknown           | Qualquer tipo que não se encaixa em nenhuma das opções abaixo                     |
| 1     | BoosterPack       | Pacote de cartas desempacotado                                                    |
| 2     | Emoticon          | Emoticons para uso no Bate-papo Steam                                             |
| 3     | FoilTradingCard   | Versão brilhante da `Carta Colecionável`                                          |
| 4     | ProfileBackground | Fundo de perfil para usar em seu perfil Steam                                     |
| 5     | TradingCard       | Cartas colecionáveis Steam, sendo usadas para fabricar insígnias (não brilhantes) |
| 6     | SteamGems         | Gemas Steam usadas para criar pacotes de cartas, incluindo as empacotadas         |

Of course, types that you should use for this property typically include only `2`, `3`, `4` and `5`, as only those types are supported by STM. Please note that **ASF is not a trading bot** and **will NOT care about price or rarity**, which means that if you use it e.g. with `Emoticon` type, then ASF will be happy to trade your 2x rare emoticon for 1x rare 1x common, as that makes progress towards badge (in this case emoticons) completion. Please evaluate twice if you're fine with that. Unless you know what you're doing, you should keep it with default value of `5`.

* * *

`OnlineStatus` - `byte` type with default value of `1`. This property specifies Steam community status that the bot will be announced with after logging in to Steam network. Currently you can choose one of below statuses:

| Valor | Nome           |
| ----- | -------------- |
| 0     | Offline        |
| 1     | Online         |
| 2     | Busy           |
| 3     | Away           |
| 4     | Snooze         |
| 5     | LookingToTrade |
| 6     | LookingToPlay  |
| 7     | Invisible      |

`Offline` status is extremely useful for primary accounts. As you should know, farming a game actually shows your steam status as "Playing game: XXX", which can be misleading to your friends, confusing them that you're playing a game while actually you're only farming it. Using `Offline` status solves that issue - your account will never be shown as "in-game" when you're farming steam cards with ASF. This is possible thanks to the fact that ASF does not have to sign in into Steam Community in order to work properly, so we're in fact playing those games, connected to Steam network, but without announcing our online presence at all. Keep in mind that played games using offline status will still count towards your playtime, and show as "recently played" on your profile.

In addition to that, this feature is also important if you want to receive notifications and unread messages when ASF is running, while not keeping Steam client open at the same time. This is because ASF acts as a Steam client itself, and whether ASF would like it or not, Steam broadcasts all those messages and other events to it. This is not a problem if you have both ASF and your own Steam client running, as both clients receive exactly the same events. However, if just ASF is running, Steam network could mark certain events and messages as "delivered", despite of your traditional Steam client not receiving it due to not being present. Offline status also solves this problem, as ASF is never considered for any community events in this case, so all unread messages and other events will be properly marked as unread when you come back.

It's important to note that ASF running on `Offline` mode will **not** be able to receive commands in usual Steam chat way, as the chat, as well as entire community presence is in fact, entirely offline. A solution to this issue is using `Invisible` mode instead which works in a similar way (not exposing status), but keeps the ability to receive and respond to messages (so also a potential to dismiss notifications and unread messages as stated above). `Invisible` mode makes the most sense on alt accounts that you don't want to expose (status-wise), but still be able to send commands to.

However, there is one catch with `Invisible` mode - it doesn't go well with primary accounts. This is because **any** Steam session that is currently online **exposes** the status, even if ASF itself does not. This is caused by the current limitation/bug of the Steam network that isn't possible to be fixed on ASF side, so if you want to use `Invisible` mode you will also need to ensure that **all** other sessions to the same account use `Invisible` mode as well. This will be the case on alt accounts where ASF is hopefully the only active session, but on primary accounts you'll almost always prefer to show as `Online` to your friends, hiding only ASF activity, and in this case `Invisible` mode will be entirely useless for you (we recommend to use `Offline` mode instead). Hopefully this limitation/bug will be eventually solved in the future by Valve, but I wouldn't expect that to happen anytime soon...

If you're unsure how to set up this property, it's recommended to use a value of `0` (`Offline`) for primary accounts, and default `1` (`Online`) otherwise.

* * *

`PasswordFormat` - `byte` type with default value of `0`. This property defines the format of `SteamPassword` property, and currently supports - `0` for `PlainText`, `1` for `AES` and `2` for `ProtectedDataForCurrentUser`. Please refer to **[Security](https://github.com/JustArchi/ArchiSteamFarm/wiki/Security)** section if you want to learn more, as you'll need to ensure that `SteamPassword` property indeed includes password in matching `PasswordFormat`. In other words, when you change `PasswordFormat` then your `SteamPassword` should be **already** in that format, not just aiming to be. Unless you know what you're doing, you should keep it with default value of `0`.

* * *

`Paused` - `bool` type with default value of `false`. This property defines initial state of `CardsFarmer` module. With default value of `false`, bot will automatically start farming when it's started, either because of `Enabled` or `start` command. Switching this property to `true` should be done only if you want to manually `resume` automatic farming process, for example because you want to use `play` all the time and never use automatic `CardsFarmer` module - this works exactly the same as `pause` **[command](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)**. Se você não tiver certeza se quer esse recurso habilitado ou não, mantenha-o com o valor `false` padrão.

* * *

`RedeemingPreferences` - `byte flags` type with default value of `0`. This property defines ASF behaviour when redeeming cd-keys, and is defined as below:

| Valor | Nome             | Descrição                                                                      |
| ----- | ---------------- | ------------------------------------------------------------------------------ |
| 0     | Nenhum           | No redeeming preferences, typical                                              |
| 1     | Forwarding       | Forward keys unavailable to redeem to other bots                               |
| 2     | Distributing     | Distribute all keys among itself and other bots                                |
| 4     | KeepMissingGames | Keep keys for (potentially) missing games when forwarding, leaving them unused |

Por favor note que esta propriedade é um campo do tipo `flags`, portanto é possível escolher qualquer combinação de valores disponíveis. Confira **[mapeamento flags](#json-mapping)** se você quiser aprender mais. Não habilitar nem um flag resultará na opção `None`.

`Forwarding` will cause bot to forward a key that is not possible to redeem, to another connected and logged on bot that is missing that particular game (if possible to check). The most common situation is forwarding `AlreadyPurchased` game to another bot that is missing that particular game, but this option also covers other scenarios, such as `DoesNotOwnRequiredApp`, `RateLimited` or `RestrictedCountry`.

`Distributing` will cause bot to distribute all received keys among itself and other bots. This means that every bot will get a single key from the batch. Typically this is used only when you're redeeming many keys for the same game, and you want to evenly distribute them among your bots, as opposed to redeeming keys for various different games. This feature makes no sense if you're redeeming only one key in a single `redeem` action (as there are no extra keys to be distributed).

`KeepMissingGames` will cause bot to skip `Forwarding` when we can't be sure if key being redeemed is in fact owned by our bot, or not. This basically means that `Forwarding` will apply **only** to `AlreadyPurchased` keys, instead of covering also other cases such as `DoesNotOwnRequiredApp`, `RateLimited` or `RestrictedCountry`. Typically you might want to use this option on primary account, to ensure that keys being redeemed on it won't be forwarded further if your bot for example becomes temporarily `RateLimited`. As you can guess from the description, this field has absolutely no effect if `Forwarding` is not enabled.

Enabling both `Forwarding` and `Distributing` will add distributing feature on top of forwarding one, which makes ASF trying to redeem one key on all bots firstly (forwarding) before moving to the next one (distributing). Typically you want to use this option only when you want `Forwarding`, but with altered behaviour of switching the bot on key being used, instead of always going in-order with every key (which would be `Forwarding` alone). This behaviour can be beneficial if you know that majority or even all of your keys are tied to the same game, because in this situation `Forwarding` alone would try to redeem everything on one bot firstly (which makes sense if your keys are for unique games), and `Forwarding` + `Distributing` will switch the bot on the next key, "distributing" the task of redeeming new key onto another bot than the initial one (which makes sense if keys are for the same game, skipping one pointless attempt per key).

The actual bots order for all of the redeeming scenarios is alphabetical, excluding bots that are unavailable (not connected, stopped or likewise). Please keep in mind that there is per-IP and per-account hourly limit of redeeming tries, and every redeem try that didn't end with `OK` contributes to failed tries. ASF will do its best to minimize number of `AlreadyPurchased` failures, e.g. by trying to avoid forwarding a key to another bot that already owns that particular game, but it's not always guaranteed to work due to how Steam is handling licenses. Using redeeming flags such as `Forwarding` or `Distributing` will always increase your likelyhood to hit `RateLimited`.

Also keep in mind that you can't forward or distribute keys to bots that you do not have access to. This should be obvious, but ensure that you're at least `Operator` of all the bots you want to include in your redeeming process, for example with `status ASF` **[command](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)**.

* * *

`SendOnFarmingFinished` - `bool` type with default value of `false`. When ASF is done with farming given account, it can automatically send steam trade containing everything farmed up to this point to user with `Master` permission, which is very convenient if you don't want to bother with trades yourself. This option works the same as `loot` command, therefore keep in mind that it requires user with `Master` permission set, you might also require valid `SteamTradeToken`, including using an account that is actually eligible for trading. In addition to initiating `loot` after finishing farming, ASF will also initiate `loot` on each new items notification (when not farming), and after completing each trade that results in new items (always) when this option is active. This is especially useful for "forwarding" items received from other people to our account. It's strongly recommended to use this feature together with **[ASF 2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Two-factor-authentication)** being set, as there is no point in sending automatic trades if you need to manually confirm them. Se não tem certeza de como definir essa propriedade, deixá-lo com o valor padrão `false`.

* * *

`SendTradePeriod` - `byte` type with default value of `0`. This property works very similar to `SendOnFarmingFinished` property, with one difference - instead of sending trade when farming is done, we can also send it every `SendTradePeriod` hours, regardless of how much we have to farm left. This is useful if you want to `loot` your alt accounts on usual basis instead of waiting for it to finish farming. Default value of `0` disables this feature, if you want your bot to send you trade e.g. every day, you should put `24` here. It's strongly recommended to use this feature together with **[ASF 2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Two-factor-authentication)** being set, as there is no point in sending automatic trades if you need to manually confirm them. If you're not sure how to set this property, leave it with default value of `0`.

* * *

`ShutdownOnFarmingFinished` - `bool` type with default value of `false`. ASF is "occupying" an account for the whole time of process being active. When given account is done with farming, ASF periodically checks it (every `IdleFarmingPeriod` hours), if perhaps some new games with steam cards were added in the meantime, so it can resume farming of that account without a need to restart the process. This is useful for majority of people, as ASF can automatically resume farming when needed. However, you may actually want to stop the process when given account is fully farmed, you can achieve that by setting this property to `true`. When enabled, ASF will proceed with logging off when account is fully farmed, which means that it won't be periodically checked or occupied anymore. You should decide yourself if you prefer ASF to work on given bot instance for the whole time, or if perhaps ASF should stop it when farming process is done. When all accounts are stopped and process is not running in `--process-required` **[mode](https://github.com/JustArchi/ArchiSteamFarm/wiki/Command-Line-Arguments)**, ASF will shutdown as well. Se não tem certeza de como definir essa propriedade, deixá-lo com o valor padrão `false`.

* * *

`SteamLogin` - `string` type with default value of `null`. This property defines your steam login - the one you use for logging in to steam. In addition to defining steam login here, you may also keep default value of `null` if you want to enter your steam login on each ASF startup instead of putting it in the config. This may be useful for you if you don't want to save sensitive data in config file.

* * *

`SteamMasterClanID` - `ulong` type with default value of `0`. This property defines the steamID of the steam group that bot should automatically join, including its group chat. You can check steamID of your group by navigating to its **[page](https://steamcommunity.com/groups/ascfarm)**, then adding `/memberslistxml?xml=1` to the end of the link, so the link will look like **[this](https://steamcommunity.com/groups/ascfarm/memberslistxml?xml=1)**. Then you can get steamID of your group from the result, it's in `<groupID64>` tag. In above example it would be `103582791440160998`. In addition to trying to join given group at startup, the bot will also automatically accept group invites to this group, which makes it possible for you to invite your bot manually if your group has private membership. If you don't have any group dedicated for your bots, you should keep this property with default value of `0`.

* * *

`SteamParentalPIN` - `string` type with default value of `0`. This property defines your steam parental PIN. ASF requires an access to resources protected by steam parental, therefore if you use that feature, you need to provide ASF with parental unlock PIN, so it can operate normally. Default value of `0` means that there is no steam parental PIN required to unlock this account, and this is probably what you want if you don't use steam parental functionality. In addition to defining steam parental PIN here, you may also use value of `null` if you want to enter your steam parental PIN on each ASF startup instead of putting it in the config. This may be useful for you if you don't want to save sensitive data in config file.

* * *

`SteamPassword` - `string` type with default value of `null`. This property defines your steam password - the one you use for logging in to steam. In addition to defining steam password here, you may also keep default value of `null` if you want to enter your steam password on each ASF startup instead of putting it in the config. This may be useful for you if you don't want to save sensitive data in config file.

* * *

`SteamTradeToken` - `string` type with default value of `null`. When you have your bot on your friend list, then bot can send a trade to you right away without worrying about trade token, therefore you can leave this property at default value of `null`. If you however decide to NOT have your bot on your friend list, then you will need to generate and fill a trade token as the user that this bot is expecting to send trades to. In other words, this property should be filled with trade token of the account that is defined with `Master` permission in `SteamUserPermissions` of **this** bot instance.

In order to find your token, as logged in user with `Master` permission, navigate **[here](https://steamcommunity.com/my/tradeoffers/privacy)** and take a look at your trade URL. The token we're looking for is made out of 8 characters after `&token=` part in your trade URL. You should copy and put those 8 characters here, as `SteamTradeToken`. Do not include whole trading URL, neither `&token=` part, only token itself.

* * *

`SteamUserPermissions` - `ImmutableDictionary<ulong, byte>` type with default value of being empty. This property is a dictionary property which maps given Steam user identified by his 64-bit steam ID, to `byte` number that specifies his permission in ASF instance. Currently available bot permissions in ASF are defined as:

| Valor | Nome          | Descrição                                                                                                                                                                                          |
| ----- | ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0     | Nenhum        | No permission, this is mainly a reference value that is assigned to steam IDs missing in this dictionary - there is no need to define anybody with this permission                                 |
| 1     | FamilySharing | Provides minimum access for family sharing users. Once again, this is mainly a reference value since ASF is capable of automatically discovering steam IDs that we permitted for using our library |
| 2     | Operator      | Provides basic access to given bot instances, mainly adding licenses and redeeming keys                                                                                                            |
| 3     | Master        | Provides full access to given bot instance                                                                                                                                                         |

In short, this property allows you to handle permissions for given users. Permissions are important mainly for access to ASF **[commands](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)**, but also for enabling many ASF features, such as accepting trades. For example you might want to set your own account as `Master`, and give `Operator` access to 2-3 of your friends so they can easily redeem keys for your bot with ASF, while **not** being eligible e.g. for stopping it. Thanks to that you can easily assign permissions to given users and let them use your bot to some specified by you degree.

We recommend to set exactly one user as `Master`, and any amount you wish as `Operator`s and below. While it's technically possible to set multiple `Master`s and ASF will work correctly with them, for example by accepting all of their trades sent to the bot, ASF will use only one of them (with lowest steam ID) for every action that requires a single target, for example a `loot` request, so also properties like `SendOnFarmingFinished` or `SendTradePeriod`. If you perfectly understand those limitations, especially the fact that `loot` request will always send items to the `Master` with lowest steam ID, regardless of the `Master` that actually executed the command, then you can define multiple users with `Master` permission here, but we still recommend a single master scheme - multiple masters scheme is discouraged setup that is not supported.

It's nice to note that there is one more extra `Owner` permission, which is declared as `SteamOwnerID` global config property. You can't assign `Owner` permission to anybody here, as `SteamUserPermissions` property defines only permissions that are related to the bot instance, and not ASF as a process.

* * *

`TradingPreferences` - `byte flags` type with default value of `0`. This property defines ASF behaviour when in trading, and is defined as below:

| Valor | Nome                | Descrição                                                                                                                                                                    |
| ----- | ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0     | Nenhum              | No trading preferences - accepts only `Master` trades                                                                                                                        |
| 1     | AcceptDonations     | Accepts trades in which we're not losing anything                                                                                                                            |
| 2     | SteamTradeMatcher   | Accepts dupes-matching **[STM](https://www.steamtradematcher.com)**-like trades. Visit **[trading](https://github.com/JustArchi/ArchiSteamFarm/wiki/Trading)** for more info |
| 4     | MatchEverything     | Requires `SteamTradeMatcher` to be set, and in combination with it - also accepts bad trades in addition to good and neutral ones                                            |
| 8     | DontAcceptBotTrades | Doesn't automatically accept `loot` trades from other bot instances                                                                                                          |

Por favor note que esta propriedade é um campo do tipo `flags`, portanto é possível escolher qualquer combinação de valores disponíveis. Confira **[mapeamento flags](#json-mapping)** se você quiser aprender mais. Não habilitar nem um flag resultará na opção `None`.

For further explanation of ASF trading logic, and description of every available flag, please visit **[Trading](https://github.com/JustArchi/ArchiSteamFarm/wiki/Trading)** section.

* * *

`UseLoginKeys` - `bool` type with default value of `true`. This property defines if ASF should use login keys mechanism for this Steam account. Login keys mechanism works very similar to official Steam client's "remember me" option, which makes it possible for ASF to store and use temporary one-time use login key for next logon attempt, effectively skipping a need of providing password, Steam Guard or 2FA code as long as our login key is valid. Login key is stored in `BotName.db` file and updated automatically. This is why you don't need to provide password/SteamGuard/2FA code after logging in successfully with ASF just once.

Login keys are used by default for your convenience, so you don't need to input `SteamPassword`, SteamGuard or 2FA code (when not using **[ASF 2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Two-factor-authentication)**) on each login. It's also superior alternative since login key can be used only for a single time and does not reveal your original password in any way. Exactly the same method is being used by your original Steam client, which saves your account name and login key for your next logon attempt, effectively being the same as using `SteamLogin` with `UseLoginKeys` and empty `SteamPassword` in ASF.

However, some people might be concerned even about this little detail, therefore this option is available here for you if you'd like to ensure that ASF won't store any kind of token that would allow resuming previous session after being closed, which will result in full authentication being mandatory on each login attempt. Disabling this option will work exactly the same as not checking "remember me" in official Steam client. Unless you know what you're doing, you should keep it with default value of `true`.

**[Voltar para o topo](#configuration)**

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

`log.txt` file holds the log generated by your last ASF run. This file doesn't contain any sensitive information, and is extremely useful when it comes to issues, crashes or simply as an information to you what happened in last ASF run. We will very often ask about for file if you run into issues or bugs. ASF automatically manages this file for you, but you can further tweak ASF **[logging](https://github.com/JustArchi/ArchiSteamFarm/wiki/Logging)** module if you're advanced user.

`config` directory is the place that holds configuration for ASF, including all of its bots.

`ASF.json` is a global ASF configuration file. This config is used for specifying how ASF behaves as a process, which affects all of the bots and program itself. You can find global properties there, such as ASF process owner, auto-updates or debugging.

`BotName.json` is a config of given bot instance. This config is used for specifying how given bot instance behaves, therefore those settings are specific to that bot only and not shared across other ones. This allows you to configure bots with various different settings and not necessarily all of them working in exactly the same way.

Apart from config files, ASF also uses `config` directory for storing databases.

`ASF.db` is a global ASF database file. It acts as a global persistent storage and is used for saving various information related to ASF process, such as IPs of local Steam servers. **You should not edit this file**.

`BotName.db` is a database of given bot instance. This file is used for storing crucial data about given bot instance in persistent storage, such as login keys or ASF 2FA. **You should not edit this file**.

`BotName.bin` is a special file of given bot instance, which holds information about Steam sentry hash. Sentry hash is used for authenticating using `SteamGuard` mechanism, very similar to Steam `ssfn` file. **You should not edit this file**.

`BotName.keys` is a special file that can be used for importing keys into **[background games redeemer](https://github.com/JustArchi/ArchiSteamFarm/wiki/Background-games-redeemer)**. It's not mandatory and not generated, but recognized by ASF. This file is automatically deleted after keys are successfully imported.

`BotName.maFile` is a special file that can be used for importing **[ASF 2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Two-factor-authentication)**. It's not mandatory and not generated, but recognized by ASF if your `BotName` does not use ASF 2FA yet. This file is automatically deleted after ASF 2FA is successfully imported.

**[Voltar para o topo](#configuration)**

* * *

## Mapeamento de JSON

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

| Valor | Nome   |
| ----- | ------ |
| 0     | Nenhum |
| 1     | A      |
| 2     | B      |
| 4     | C      |

Using `B + C` would result in value of `6`, using `A + C` would result in value of `5`, using `C` would result in value of `4` and so on. This allows you to create any possible combination of enabled values - if you decided to enable all of them, making `None + A + B + C`, you'd get value of `7`. Also notice that flag with value of `0` is enabled by definition in all other available combinations, therefore very often it's a flag that doesn't enable anything specifically (such as `None`).

So as you can see, in above example we have 3 available flags to switch on/off (`A`, `B`, `C`), and 8 possible values overall (`None -> 0`, `A -> 1`, `B -> 2`, `A+B -> 3`, `C -> 4`, `A+C -> 5`, `B+C -> 6`, `A+B+C -> 7`).

**[Voltar para o topo](#configuration)**

* * *

## Mapeamento de compatibilidade

Due to JavaScript limitations of being unable to properly serialize simple `ulong` fields in JSON when using web-based ConfigGenerator, `ulong` fields will be rendered as strings with `s_` prefix in the resulting config. This includes for example `"SteamOwnerID": 76561198006963719` that will be written by our ConfigGenerator as `"s_SteamOwnerID": "76561198006963719"`. ASF includes proper logic for handling this string mapping automatically, so `s_` entries in your configs are actually valid and correctly generated. If you're generating configs yourself, we recommend to stick with original `ulong` fields if possible, but if you're unable to do so, you can also follow this scheme and encode them as strings with `s_` prefix added to their names. We hope to resolve this JavaScript limitation eventually.

**[Voltar para o topo](#configuration)**

* * *

## Configuração de compatibilidade

It's top priority for ASF to remain compatible with older configs. As you should already know, missing config properties are treated the same as they would be defined with their **default values**. Therefore, if new config property gets introduced in new version of ASF, all your configs will remain **compatible** with new version, and ASF will treat that new config property as it'd be defined with its **default value**. You can always add, remove or edit config properties according to your needs. We recommend to limit defined config properties only to those that you want to change, since this way you automatically inherit default values for all other ones, not only keeping your config clean but also increasing compatibility in case we decide to change a default value for property that you don't want to explicitly set yourself. Feel free to check `minimal.json` example configuration file that follows this concept.

**[Voltar para o topo](#configuration)**

* * *

## Recarregamento automático

Starting with ASF V2.1.6.2+, the program is now aware of configs being modified "on-the-fly" - thanks to that, ASF will automatically:

- Create (and start, if needed) new bot instance, when you create its config
- Stop (if needed) and remove old bot instance, when you delete its config
- Stop (and start, if needed) any bot instance, when you edit its config
- Restart (if needed) the bot under new name, when you rename its config

All of the above is transparent and will be done automatically without a need of restarting the program, or killing other (unaffected) bot instances.

In addition to that, ASF will also restart itself (if `AutoRestart` permits) if you modify core ASF `ASF.json` config. Likewise, program will quit if you delete or rename it.

**[Voltar para o topo](#configuration)**