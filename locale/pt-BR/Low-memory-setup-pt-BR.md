# Configuração para baixo consumo de memória

Isso é exatamente o oposto da **[configuração de alta performance](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/High-performance-setup-pt-BT)** e provavelmente você vai querer seguir essas dicas se você quer diminuir o uso de memória pelo ASF, ao custo de diminuir o desempenho.

* * *

ASF is extremely lightweight on resources by definition, depending on your usage even 128 MB VPS with Linux is capable of running it, although going that low is not recommended and can lead to various issues. Mesmo sendo leve, o ASF pode pedir mais memória para o sistema operacional se ela for necessária para que o ASF opere com velocidade otimizada.

Como programa, o ASF tenta ser o mais otimizado e eficiente possível, o que também leva em conta os recursos usados durante a execução. Quando se trata de memória, o ASF prioriza sempre a performance sobre o consumo de memória, o que pode acarretar "picos" temporários de memória que podem ser notados em contas com mais de 3 páginas de insígnias, por exemplo, já que o ASF vai buscar e analisar a primeira página, ler dela o número total de páginas e então lançar uma tarefa de pedido para ada página extra, o que resulta no pedido e analise das demais páginas. That "extra" memory usage (compared to bare minimum for operation) can dramatically speed up execution and overall performance, for the cost of increased memory usage that is needed to do all of those things in parallel. Similar thing is happening to all other general ASF tasks that can be run in parallel, e.g. with parsing active trade offers, ASF can parse all of them at once, as they're all independent of each other. On top of that, ASF (C# runtime) will **not** return unused memory back to OS immediately afterwards, which you can quickly notice in form of ASF process only taking more and more memory, but never giving that memory back to the OS. Some people might already find it questionable, maybe even suspect a memory leak, but don't worry, all of this is to be expected.

O ASF é extremamente bem otimizado, e faz uso dos recursos disponíveis tanto quanto possível. Um alto consumo de memória pelo ASF não significa que ele ativamente **usa** essa memória e **precisa dela**. Very often ASF will keep allocated memory as "room" for future actions, because we can drastically improve performance if we don't need to ask OS for every memory chunk that we're about to use. O tempo de execução deve liberar automaticamente a memória não utilizada pelo ASF de volta ao sistema operacional quando ele **realmente** precisar. **[Unused memory is wasted memory](https://www.howtogeek.com/128130/htg-explains-why-its-good-that-your-computers-ram-is-full)**. You run into issues when the memory you **need** is higher than the memory that is available for you, not when ASF keeps some extra allocated with purpose of speeding up functions that will execute in a moment. Você tem problemas quando seu kernel do Linux está matando o processo do ASF devido a OOM (falta de memória), não quando você vê o processo do ASF como o usuário principal da memória em `htop`.

Garbage collector being used in ASF is a very complex mechanism, smart enough to take into account not only ASF itself, but also your OS and other processes. Quando você tem um monte de memória livre, o ASF vai pedir o quanto for necessário para melhorar o desempenho. Isso pode chegar até a 1 GB (com o coletor de lixo de servidor). When your OS memory is close to being full, ASF will automatically release some of it back to the OS to help things settle down, which can result in overall ASF memory usage as low as 50 MB. The difference between 50 MB and 1 GB is huge, but so is the difference between small 512 MB VPS and huge dedicated server with 32 GB. If ASF can guarantee that this memory will come useful, and at the same time nothing else requires it right now, it'll prefer to keep it and automatically optimize itself based on routines that were executed in the past. The GC used in ASF is self-tuning and will achieve better results the longer the process is running.

This is also why ASF process memory varies from setup to setup, as ASF will do its best to use available resources in **as efficient way as possible**, and not in a fixed way like it was done during Windows XP times. The actual (real) memory usage that ASF is using can be verified with `stats` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**, and is usually around 4 MB for just a few bots, up to 30 MB if you use stuff like **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** and other advanced features. Keep in mind that memory returned by `stats` command also includes free memory that hasn't been reclaimed by garbage collector yet. Everything else is shared runtime memory (around 40-50 MB) and room for execution (vary). This is also why the same ASF can use as little as 50 MB in low-memory VPS environment, while using even up to 1 GB on your desktop. ASF is actively adapting to your environment and will try to find optimal balance in order to neither put your OS under pressure, nor limit its own performance when you have a lot of unused memory that could be put in use.

* * *

Of course, there are a lot of ways how you can help point ASF at the right direction in terms of the memory you expect to use. In general if you don't need to do it, it's best to let garbage collector work in peace and do whatever it considers is best. But this is not always possible, for example if your Linux server is also hosting several websites, MySQL database and PHP workers, then you can't really afford ASF shrinking itself when you run close to OOM, as it's usually too late and performance degradation comes sooner. This is usually when you might be interested in further tuning, and therefore reading this page.

Below suggestions are divided into a few categories, with varied difficulty.

* * *

## Ajuste do ASF (fácil)

Below tricks **do not affect performance negatively** and can be safely applied to all setups.

- Nunca execute mais de uma cópia do ASF. O ASF destina-se a lidar com um número ilimitado de bots de uma vez e a menos que você esteja vinculando cada cópia do ASF para diferentes interfaces/endereços de IP, você deve ter exatamente **um** processo do ASF, com vários bots (se necessário).
- Utilize o `ShutdownOnFarmingFinished`. Um bot ativo ocupa mais recursos que um desativado. Não é um ganho significativo, já que o estado do bot ainda precisa ser mantido, mas você está salvando uma certa quantidade de recursos, especialmente de todos os recursos relacionados à rede, tais como soquetes TCP. Você precisa de apenas um bot ativo para manter o ASF em execução, e você sempre pode ativar outros bots se necessário.
- Mantenha uma quantidade baixa de bots. Um bot sem o parâmetro `Enabled` toma menos recursos, já que o ASF não se preocupa em iniciá-lo. Também tenha em mente que o ASF tem que criar um bot para cada uma das suas configurações, portanto se você não precisa iniciar um bot com o comando `start` e quer salvar alguma memória extra, você pode renomear temporariamente `Bot.json` para, por exemplo, `Bot.json.bak`, para que o ASF não crie um bot inativo no processo. Desta forma você não será capaz de iniciá-lo pelo comando `start` sem renomeá-lo de volta, mas o ASF também não se preocupa em manter o estado deste bot na memória deixando espaço para outras coisas (é um ganho muito pequeno, em 99,9% casos você não deveria se preocupar com isso, só mantér seus bot com o parâmetro `Enabled` com o valor `false`).
- Afine suas configurações. O arquivo de configuração global do ASF que tem muitas variáveis para ajustar, por exemplo, aumentando `LoginLimiterDelay` vai fazer com que o tempo entre o carregamento dos bots seja maior, o que permitirá que um bot já iniciado busque a página de insígnias nesse intervalo, ao contrário de carregá-los de uma vez que toma mais recursos, pois os bots fazem todo o trabalho (tal como analisar as insígnias) ao mesmo tempo. Quanto menos trabalho a ser feito ao mesmo tempo - menos memória é usada.

Those are a few things you can keep in mind when dealing with memory usage. However, those things don't have any "crucial" matter on memory usage, because memory usage comes mostly from things ASF has to deal with, and not from internal structures used for cards farming.

The most resources-heavy functions are:

- Análise da página de insígnias
- Análise do inventário

Which means that memory will spike the most when ASF is dealing with reading badge pages, and when it's dealing with its inventory (e.g. sending trade or working with STM). This is because ASF has to deal with really huge amount of data - the memory usage of your favourite browser launching those two pages will not be any lower than that. Sorry, that's how it works - decrease number of your badge pages, and keep number of your inventory items low, that can for sure help.

* * *

## Ajuste do tempo de execução (avançado)

Below tricks **involve performance degradation** and should be used with caution.

`ArchiSteamFarm.runtimeconfig.json` permite que você configure o tempo de execução do ASF, especialmente permitindo que você alterne entre coleta de lixo de servidor ou de estação de trabalho.

> O coletor de lixo tem autoajuste e pode trabalhar em uma ampla variedade de cenários. Você pode usar uma definição de arquivo de configuração para definir o tipo de coleta de lixo com base nas características da carga de trabalho. O CLR fornece os seguintes tipos de coleta de lixo:
> 
> Coleta de lixo de estação de trabalho, que serve para todas as estações de trabalho cliente e computadores autônomos. Essa é a configuração padrão para o <gcserver> elemento no esquema de configuração de tempo de execução.
> 
> A coleta de lixo de servidor, que serve para aplicativos de servidor que precisam de escalabilidade e taxa de transferência altas. A coleta de lixo de servidor pode ser não simultânea ou em segundo plano.

Mais pode ser lido em **[noções básicas da coleta de lixo](https://docs.microsoft.com/pt-br/dotnet/standard/garbage-collection/fundamentals)**.

ASF is already using workstation GC, but you can ensure that it's truly the case by checking if `System.GC.Server` property of `ArchiSteamFarm.runtimeconfig.json` is set to `false`.

In addition to verifying that workstation GC is active, there are also interesting **[configuration knobs](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/clr-configuration-knobs.md)** that you can use - `gcTrimCommitOnLowMemory` and `GCLatencyLevel`.

### `GCLatencyLevel`

> Especifica o nível de latência do coletor de lixo que você deseja otimizar.

This works exceptionally well by limiting size of GC generations and in result make GC purge them more frequently and more aggressively. Default (balanced) latency level is `1`, we'll want to use `0`, which will tune for memory usage.

### `gcTrimCommitOnLowMemory`

> Quando ativo, nós ajustamos a memória alocada de forma mais agressiva para os segmentos de curta duração. Isso é usado para rodar vários processos no servidor, onde há a necessidade do menor uso de memória possível.

This offers little improvement, but might make GC even more aggressive when system will be low on memory.

* * *

You can enable both by setting appropriate `COMPlus_` environment variables. For example, on Linux:

```shell
export COMPlus_GCLatencyLevel=0
export COMPlus_gcTrimCommitOnLowMemory=1
./ArchiSteamFarm
```

Or on Windows:

```bat
SET COMPlus_GCLatencyLevel=0
SET COMPlus_gcTrimCommitOnLowMemory=1
.\ArchiSteamFarm.exe
```

Especially `GCLatencyLevel` will come very useful as we verified that the runtime indeed optimizes code for memory and therefore drops average memory usage significantly, even with server GC. It's one of the best tricks that you can apply if you want to significantly lower ASF memory usage while not degrading performance too much with `OptimizationMode`.

* * *

## Ajuste do ASF (intermediário)

Below tricks **involve serious performance degradation** and should be used with caution.

- Como um último recurso você pode ajustar o ASF para `MinMemoryUsage` (uso mínimo de memória) através do **[parâmetro de configuração global](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-pt-BR#configura%C3%A7%C3%A3o-global)** `OptimizationMode`. Leia atentamente a sua finalidade, pois se trata de uma queda muito grande de desempenho e resulta em pouquíssimos benefícios de memória. Normalmente essa é **a última coisa que você quer fazer**, depois de ter feitos os **[ajustes do tempo de execução](#ajuste do-tempo-de-execução)** para garantir que você está sendo forçado a fazer isso.

* * *

## Otimização recomendada

- Comece com as dicas simples de configuração do ASF, talvez você só esteja usando o ASF de forma errada, como iniciando o processo várias vezes para todos os bots, ou mantendo todos ativos quando você precisa iniciar automaticamente apenas um ou dois.
- Se ainda não for o suficiente, habilite todos os controled de configuração listados acima definindo as variáveis de ambiente apropriadas em `COMPlus_`. Especialmente `GCLatencyLevel`, que oferece melhorias significativas de tempo de execução com pouca queda de desempenho.
- Se mesmo isso não ajudar, como um último recurso defina `MinMemoryUsage` em `OptimizationMode`. Isso força o ASF a executar quase tudo de forma síncrona, fazendo-o trabalhar muito mais devagar, mas também sem depender do pool de threads para equilibrar as coisas quando se trata de execução paralela.

It's physically impossible to decrease memory even further, your ASF is already heavily degraded in terms of performance and you depleted all your possibilities, both code-wise and runtime-wise. Consider adding some extra memory for ASF to use, even 128 MB would make a great difference.