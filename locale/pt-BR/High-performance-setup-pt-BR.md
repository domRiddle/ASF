# Configuração de alto desempenho

Isto é exatamente o oposto da **[configuração de pouca memória](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup)** e provavelmente você vai querer seguir essas dicas se você quer aumentar a performance do ASF (em termos de velocidade da CPU), ao custo de aumentar o uso de memória.

* * *

O ASF tenta preferir desempenho quando se trata equilíbrio, portanto não há muito que você possa fazer para aumentar sua performance, embora não estejamos totalmente sem opções. No entanto, tenha em mente que essas opções não são habilitadas por padrão, o que significa que elas não são boas o bastante para considerá-las equilibradas para a maioria das situações de uso, portanto você deve decidir por si mesmo se o aumento de memória que elas trazem valem a pena para você.

* * *

## Ajuste do tempo de execução (avançado)

Os truques abaixo **envolve um grande aumento do uso de memória** e devem ser usados com cautela.

`ArchiSteamFarm.runtimeconfig.json` permite que você configure o tempo de execução do ASF, especialmente permitindo que você alterne entre coleta de lixo de servidor ou de estação de trabalho.

> O coletor de lixo tem autoajuste e pode trabalhar em uma ampla variedade de cenários. Você pode usar uma definição de arquivo de configuração para definir o tipo de coleta de lixo com base nas características da carga de trabalho. O CLR fornece os seguintes tipos de coleta de lixo:
> 
> Coleta de lixo de estação de trabalho, que serve para todas as estações de trabalho cliente e computadores autônomos. Essa é a configuração padrão para o <gcserver> elemento no esquema de configuração de tempo de execução.
> 
> A coleta de lixo de servidor, que serve para aplicativos de servidor que precisam de escalabilidade e taxa de transferência altas. A coleta de lixo de servidor pode ser não simultânea ou em segundo plano.

Mais pode ser lido em **[noções básicas da coleta de lixo](https://docs.microsoft.com/pt-br/dotnet/standard/garbage-collection/fundamentals)**.

O ASF usa a coleta de lixo de estação de trabalho por padrão. Principalmente por causa do balanço entre uso de memória e desempenho, o que é mais que suficiente para alguns bots, já que um único coletor de lixo simultaneamente em segundo plano é rápido o bastante para cuidar de toda a memória alocada pelo ASF.

No entanto, hoje nós temos um monte de núcleos de CPU dos quais o ASF pode se beneficiar por ter um thread de coleta de lixo dedicado para cada núcleo de CPU virtual disponível. Isto poderá melhorar muito o desempenho durante tarefas pesadas do ASF tais como análise de páginas de insígnias ou inventário, já que cada núcleo virtual de CPU pode ajudar, ao invés de apenas 2 (o principal e o de coleta de lixo). A coleta de lixo do servidor é recomendado para computadores com 3 núcleos virtuais de CPU ou mais e a coleta de lixo de estação de trabalho é forçada automaticamente se seu computador tem apenas um núcleo virtual de CPU, e se você tem exatamente 2, considere tentar ambos (os resultados podem variar).

Você pode habilitar a coleta de lixo de servidor mudando o parâmetro `System.GC.Server` em `ArchiSteamFarm.runtimeconfig.json` de `false` para `true`. Tenha em mente que talvez você precise fazer isso mais de uma vez, pois o ASF usará sempre `false` por padrão após uma atualização automática.

A coleta de lixo de servidor em si não resulta em um aumento de memória muito grande apenas por estar ativo, mas tem muito mais capacidade de geração e portanto é muito mais lento quando se trata de retorno de memória ao sistema operacional. Você pode achar que encontrou um bom ponto quando o coletor de lixo de servidor aumenta significativamente o desempenho e você deseja usa-lo, mas ao mesmo tempo você não pode permitir um aumento significativo do uso de memória que acompanha o uso dele. Felizmente há uma configuração que proporciona o "melhor dos dois mundos": usar o coletor de lixo do servidor com **[nível de latência do coletor de lixo](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup#gclatencylevel)** definido como `0`, que vai permitir o coletor de lixo do servidor, mas limitar os tamanhos de geração e focar mais na memória.

No entanto, se memória não é um problema para você (como o coletor de lixo leva em conta sua memória disponível e se auto-ajusta), é melhor não alterar `GCLatencyLevel`, alcançando um desempenho superior como resultado.

* * *

## Otimização recomendada

- Habilite a coleta de lixo de servidor mudando o parâmetro `System.GC.Server` em `ArchiSteamFarm.runtimeconfig.json` de `false` para `true`. Isso habilitará o coletor de lixo de servidor que pode ser visto imediatamente pelo aumento de uso de memória comparado com o coletor de lixo de estação de trabalho.
- Se não puder aceitar esse aumento no uso de memória, considere colocar o valor `0` em `GCLatencyLevel` para ter "o melhor dos dois mundos". No entanto, se sua memória não aguenta é melhor manter tudo nos valores padrão; o coletor de lixo de servidor se auto-ajusta durante o tempo de execução e é inteligente o bastante para usar menos memória quando seu sistema operacional necessita dela.

Se você habilitou o coletor de lixo de servidor e manteve `GCLatencyLevel` com o valor padrão, então você terá uma performance superior do ASF que deverá estar funcionando rapidamente mesmo com centenas ou milhares de bots ativos. O CPU não deverá mais ser um gargalo, já que o ASF pode usar todo o desempenho do seu CPU caso necessário, reduzindo o tempo necessário ao mínimo possível.