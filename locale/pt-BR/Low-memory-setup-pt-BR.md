# Configuração para baixo consumo de memória

Isso é exatamente o oposto da **[configuração de alta performance](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/High-performance-setup-pt-BT)** e provavelmente você vai querer seguir essas dicas se você quer diminuir o uso de memória pelo ASF, ao custo de diminuir o desempenho.

* * *

O ASF é extremamente leve em recursos, dependendo do uso até um VPS de 128MB com Linux é capaz de executá-lo, apesar que usar uma configuração tão fraca não é recomendado e pode causar problemas. Mesmo sendo leve, o ASF pode pedir mais memória para o sistema operacional se ela for necessária para que o ASF opere com velocidade otimizada.

Como programa, o ASF tenta ser o mais otimizado e eficiente possível, o que também leva em conta os recursos usados durante a execução. Quando se trata de memória, o ASF prioriza sempre a performance sobre o consumo de memória, o que pode acarretar "picos" temporários de memória que podem ser notados em contas com mais de 3 páginas de insígnias, por exemplo, já que o ASF vai buscar e analisar a primeira página, ler dela o número total de páginas e então lançar uma tarefa de pedido para ada página extra, o que resulta no pedido e analise das demais páginas. Isso deixa e execução mais rápida ao custo do aumento do uso de memória. Coisa semelhante acontece, por exemplo, com a análise de ofertas de troca ativas, o ASF também as analisa simultaneamente. Acima de tudo, o ASF (tempo de execução C#) não retorna a memória não utilizada imediatamente ao sistema operacional. Hã? O que está acontecendo?

O ASF é extremamente bem otimizado, e faz uso dos recursos disponíveis tanto quanto possível. Um alto consumo de memória pelo ASF não significa que ele ativamente **usa** essa memória e **precisa dela**. Muitas vezes o ASF manterá alguma memória alocada para ter "espaço" para ações futuras, já que por não pedir para o sistema operacional cada pedaço de memória necessário estamos melhorando desempenho. O tempo de execução deve liberar automaticamente a memória não utilizada pelo ASF de volta ao sistema operacional quando ele **realmente** precisar. Lembre-se: **[memória não utilizada é memória desperdiçada](https://www.howtogeek.com/128130/htg-explains-why-its-good-that-your-computers-ram-is-full)**. Você tem problemas quando a memória **necessária** é maior que a memória disponível, não quando o ASF mantém alguma memória extra para ter espaço livre para as funções que serão executadas em determinado momento. Você tem problemas quando seu kernel do Linux está matando o processo do ASF devido a OOM (falta de memória), não quando você vê o processo do ASF como o usuário principal da memória em `htop`.

O coletor de lixo usado no ASF é inteligente o suficiente para levar em conta não só o ASF, mas também seu sistema operacional. Quando você tem um monte de memória livre, o ASF vai pedir o quanto for necessário para melhorar o desempenho. Isso pode chegar até a 1 GB (com o coletor de lixo de servidor). Quando a memória do sistema operacional estiver quase cheia, o ASF automaticamente liberará uma quantidade de volta ao sistema operacional para ajudar a resolver isso, o que pode resultar na memória usada pelo ASF baixar para até 50 MB. É por isso que a memória do processo do ASF varia de um computador para o outro, como o ASF fará o seu melhor para utilizar os recursos disponíveis **da melhor forma possível** e não de uma forma fixa, como era feito na época do Windows XP. A quantidade (real) de memória que o ASF está usando pode ser verificada com o **[comando](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-pt-BR)** `stats`, e geralmente é cerca de 4Mb para poucos bots. Tenha em mente que memória retornada pelo comando `stats` inclui memória livre que ainda não foi recuperada pelo coletor de lixo. O resto é memória compartilhada pelo tempo de execução (cerca de 40~50 MB) e espaço para execução (podem variar). Também é por esse motivo que o mesmo ASF pode usar tão pouca memória quanto 50 MB em um ambiente VPS, enquanto usa até 1 GB em seu desktop.

* * *

Claro, existem muitas formas de apontar o ASF na direção certa em termos de quantidade de memória que pretende usar. No geral, se você não precisa disso, é melhor deixar o coletor de lixo trabalhar em paz e fazer o que ele achar melhor. Mas nem sempre isso é possível, por exemplo, se seu servidor Linux também está hospedando vários sites, bancos de dados MySQL e manipuladores PHP, então você realmente não pode permitir que o ASF encolhendo apenas quando você estiver perto de ficar sem memória, pois geralmente é tarde demais e leva a queda de desempenho. É normalmente nesse caso que você pode estar interessado em uma configuração mais detalhada e, portanto, em ler essa página.

As sugestões abaixo estão divididas em algumas categorias, com dificuldade variada.

* * *

## Ajuste do ASF (fácil)

As dicas abaixo **não afetam negativamente o desempenho** e podem ser aplicados com segurança a todas as configurações.

- Nunca execute mais de uma cópia do ASF. O ASF destina-se a lidar com um número ilimitado de bots de uma vez e a menos que você esteja vinculando cada cópia do ASF para diferentes interfaces/endereços de IP, você deve ter exatamente **um** processo do ASF, com vários bots (se necessário).
- Utilize o `ShutdownOnFarmingFinished`. Um bot ativo ocupa mais recursos que um desativado. Não é um ganho significativo, já que o estado do bot ainda precisa ser mantido, mas você está salvando uma certa quantidade de recursos, especialmente de todos os recursos relacionados à rede, tais como soquetes TCP. Você precisa de apenas um bot ativo para manter o ASF em execução, e você sempre pode ativar outros bots se necessário.
- Mantenha uma quantidade baixa de bots. Um bot sem o parâmetro `Enabled` toma menos recursos, já que o ASF não se preocupa em iniciá-lo. Também tenha em mente que o ASF tem que criar um bot para cada uma das suas configurações, portanto se você não precisa iniciar um bot com o comando `start` e quer salvar alguma memória extra, você pode renomear temporariamente `Bot.json` para, por exemplo, `Bot.json.bak`, para que o ASF não crie um bot inativo no processo. Desta forma você não será capaz de iniciá-lo pelo comando `start` sem renomeá-lo de volta, mas o ASF também não se preocupa em manter o estado deste bot na memória deixando espaço para outras coisas (é um ganho muito pequeno, em 99,9% casos você não deveria se preocupar com isso, só mantér seus bot com o parâmetro `Enabled` com o valor `false`).
- Afine suas configurações. O arquivo de configuração global do ASF que tem muitas variáveis para ajustar, por exemplo, aumentando `LoginLimiterDelay` vai fazer com que o tempo entre o carregamento dos bots seja maior, o que permitirá que um bot já iniciado busque a página de insígnias nesse intervalo, ao contrário de carregá-los de uma vez que toma mais recursos, pois os bots fazem todo o trabalho (tal como analisar as insígnias) ao mesmo tempo. Quanto menos trabalho a ser feito ao mesmo tempo - menos memória é usada.

Essas são algumas coisas que você deve ter em mente ao lidar com o uso de memória. No entanto, essas coisas não têm qualquer papel "crucial" no uso de memória, porque o uso de memória vem principalmente de coisas com as quais o ASF tem de lidar e não de estruturas internas, utilizadas para a coleta.

As funções de recursos mais pesadas são:

- Análise da página de insígnias
- Análise do inventário

O que significa que a memória terá os maiores picos quando o ASF fizer a leitura das páginas de insignias, e quando ele está lidando com seu inventário (por exemplo, enviando trocas ou trabalhando com o STM). Isso acontece porque a ASF tem de lidar com uma enorme quantidade de dados - o uso de memória pelo seu navegador quando acessa essas duas páginas não será menor que isso. Desculpe, mas é assim que funciona - diminuir o número de páginas de insígnias e manter um número baixo de itens no seu inventário com certeza pode ajudar.

* * *

## Ajuste do tempo de execução (avançado)

As dicas abaixo **envolvem diminuição de performance** e devem ser usados com cautela.

`ArchiSteamFarm.runtimeconfig.json` permite que você configure o tempo de execução do ASF, especialmente permitindo que você alterne entre coleta de lixo de servidor ou de estação de trabalho.

> O coletor de lixo tem autoajuste e pode trabalhar em uma ampla variedade de cenários. Você pode usar uma definição de arquivo de configuração para definir o tipo de coleta de lixo com base nas características da carga de trabalho. O CLR fornece os seguintes tipos de coleta de lixo:
> 
> Coleta de lixo de estação de trabalho, que serve para todas as estações de trabalho cliente e computadores autônomos. Essa é a configuração padrão para o <gcserver> elemento no esquema de configuração de tempo de execução.
> 
> A coleta de lixo de servidor, que serve para aplicativos de servidor que precisam de escalabilidade e taxa de transferência altas. A coleta de lixo de servidor pode ser não simultânea ou em segundo plano.

Mais pode ser lido em **[noções básicas da coleta de lixo](https://docs.microsoft.com/pt-br/dotnet/standard/garbage-collection/fundamentals)**.

O ASF já usa a coleta de lixo de estação de trabalho, mas você pode garantir que esse é realmente o caso, verifique se o parâmetro `System.GC.Server` de `ArchiSteamFarm.runtimeconfig.json` está definido como `false`.

Além de verificar que o coletor de lixo estiver ativo, há também **[botões de configuração](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/clr-configuration-knobs.md)** interessantes que você pode usar: `gcTrimCommitOnLowMemory` e `GCLatencyLevel`.

### `GCLatencyLevel`

> Especifica o nível de latência do coletor de lixo que você deseja otimizar.

Funciona excepcionalmente bem limitando o tamanho das gerações de coleta de lixo e como resultado faz o coletor de lixo expurgá-los mais frequentemente e de forma mais agressiva. O atraso padrão (balanceado) de latência é `1`, devemos usar `0`, que vai reduzir o uso de memória.

### `gcTrimCommitOnLowMemory`

> Quando ativo, nós ajustamos a memória alocada de forma mais agressiva para os segmentos de curta duração. Isso é usado para rodar vários processos no servidor, onde há a necessidade do menor uso de memória possível.

Ele oferece pouca melhora mas pode tornar o coletor de lixo ainda mais agressivo quando o sistema ficar com pouca memória.

* * *

Você pode habilitar ambos definindo as variáveis `COMPlus_` apropriadas. Por exemplo, no linux:

```shell
export COMPlus_GCLatencyLevel=0
export COMPlus_gcTrimCommitOnLowMemory=1
./ArchiSteamFarm
```

Ou no Windows:

```bat
SET COMPlus_GCLatencyLevel=0
SET COMPlus_gcTrimCommitOnLowMemory=1
.\ArchiSteamFarm.exe
```

`GCLatencyLevel` será especialmente útil, pois verificamos que o tempo de execução de fato otimiza o código para a memória e portanto diminui significativamente o uso de memória, mesmo com o coletor de lixo do servidor. Esse é uma das melhores dicas que você pode aplicar se você deseja diminuir significativamente o uso de memória pelo ASF sem degradar demais o desempenho com `OptimizationMode`.

* * *

## Ajuste do ASF (intermediário)

As dicas abaixo **envolvem séria diminuição de performance** e devem ser usados com cautela.

- Como um último recurso você pode ajustar o ASF para `MinMemoryUsage` (uso mínimo de memória) através do **[parâmetro de configuração global](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-pt-BR#configura%C3%A7%C3%A3o-global)** `OptimizationMode`. Leia atentamente a sua finalidade, pois se trata de uma queda muito grande de desempenho e resulta em pouquíssimos benefícios de memória. Normalmente essa é **a última coisa que você quer fazer**, depois de ter feitos os **[ajustes do tempo de execução](#ajuste do-tempo-de-execução)** para garantir que você está sendo forçado a fazer isso.

* * *

## Otimização recomendada

- Comece com as dicas simples de configuração do ASF, talvez você só esteja usando o ASF de forma errada, como iniciando o processo várias vezes para todos os bots, ou mantendo todos ativos quando você precisa iniciar automaticamente apenas um ou dois.
- Se ainda não for o suficiente, habilite todos os controled de configuração listados acima definindo as variáveis de ambiente apropriadas em `COMPlus_`. Especialmente `GCLatencyLevel`, que oferece melhorias significativas de tempo de execução com pouca queda de desempenho.
- Se mesmo isso não ajudar, como um último recurso defina `MinMemoryUsage` em `OptimizationMode`. Isso força o ASF a executar quase tudo de forma síncrona, fazendo-o trabalhar muito mais devagar, mas também sem depender do pool de threads para equilibrar as coisas quando se trata de execução paralela.

É fisicamente impossível diminuir ainda mais o uso de memória, o ASF já estará com o desempenho seriamente afetado e você sem mais opções, tanto em termos de código quanto em termos de tempo de execução. Considere adicionar mais memória para o ASF usar, até 128MB faria uma grande diferença.