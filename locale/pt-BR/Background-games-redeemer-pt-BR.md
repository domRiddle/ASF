# Ativador de jogos em segundo plano

O ativador de jogos em segundo plano é uma funcionalidade especial embutida no ASF que permite que você importe uma lista de cd-keys do Steam (juntamente com seus nomes) para serem resgatadas em segundo plano. Ele é especialmente útil se você tem um monte de keys para resgatar e é certo que você atingirá o **[estado](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ-pt-BR#qual-o-significado-do-estado-quando-se-resgata-uma-key)** `RateLimited` antes de terminar.

O ativador de códigos em segundo plano foi feito para uso em apenas um bot, o que significa que ele não faz uso de `RedeemingPreferences`. Esse recurso pode ser usado junto com (ou no lugar do) **[comando](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-pt-BR)** `redeem`, caso necessário.

* * *

## Importar

O processo de importação das keys pode ser feito de duas maneiras: ou com um arquivo ou usando o IPC.

### Arquivo

O ASF reconhecerá em sua pasta `config` um arquivo chamado `BotName.keys`, onde `BotName` é o nome do seu bot. Esse arquivo deve conter uma estrutura fixa com o nome do jogo e a key separados por um caractere de tabulação e terminando com uma nova linha. Se várias tabulações forem usadas, então a primeira entrada será considerada o nome do jogo e a última será considerada uma key, tudo o que estiver no meio será ignorado. Por exemplo:

    POSTAL 2    ABCDE-EFGHJ-IJKLM
    Domino Craft VR 12345-67890-ZXCVB
    A Week of Circus Terror POIUY-KJHGD-QWERT
    Terraria    IssoÉIgnorado    IssoTambémÉIgnorado    ZXCVB-ASDFG-QWERT
    

O ASF importará esse arquivo, seja na inicialização do bot ou posteriormente durante a execução. Depois da análise bem sucedida do seu arquivo e eventual omissão de entradas inválidas, todos os jogos corretamente detectados serão adicionados à fila em segundo plano, e o arquivo `BotName.keys` será removido da pasta `config`.

### IPC

In addition to using keys file mentioned above, ASF also exposes `GamesToRedeemInBackground` **[ASF API endpoint](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#asf-api)** which can be executed by any IPC tool, including our ASF-ui. Using IPC might be more powerful, as you can do appropriate parsing yourself, such as using a custom delimiter instead of being forced to a tab character, or even entirely customized keys structure.

* * *

## Fila

Assim que os jogos são importados com êxito, eles são adicionados à fila. O ASF percorre automaticamente a fila em segundo plano enquanto o bot continuar conectado a rede Steam e a fila não estiver vazia. Uma key que tentou ser resgatada e não resultou em `RateLimited` é removida da lista, com seu status propriamente escrito em um arquivo na pasta `config` - sendo `BotName.keys.used` se a chave foi usada no processo (por ex.: `NoDetail`, `BadActivationCode`, `DuplicateActivationCode`), ou `BotName.keys.unused` caso contrário. O ASF usa intencionalmente o nome do jogo que você forneceu uma vez que não é garantido que a key retorne um nome correto pelo Steam; dessa forma você pode marcar suas keys com nomes personalizados se precisar/quiser.

Se durante o processo a conta atingir o estado `RateLimited`, a fila é suspensa temporariamente por uma hora inteira para esperar o fim do bloqueio. Depois disso, o processo continua de onde parou, até que a fila inteira esteja vazia.

* * *

## Exemplo

Vamos supor que você tem uma lista de 100 keys. Em primeiro lugar, você deve criar um novo arquivo `BotName.keys.new` na pasta `config` do ASF. Nós adicionamos a extensão `.new` para que o ASF saiba que não deve pegar esse arquivo imediatamente quando ele for criado (como ele é arquivo vazio ele não está pronto para importação ainda).

Agora você pode abrir o novo arquivo e colar a lista de 100 keys nele, arrumando a formatação se necessário. Após as correções o arquivo `BotName.keys.new` terá exatamente 100 linhas (ou 101, com a última quebra de linha), cada linha tendo a estrutura: `NomeDoJogo\tcd-key\n`, onde `\t` é o caractere de tabulação e `\n` é a quebra de linha.

Agora você está pronto para renomear este arquivo de `BotName.keys.new` para `BotName.keys` para que o ASF saiba que está pronto para ser carregado. No momento que você fizer isso, o ASF vai importar automaticamente o arquivo (sem necessidade de reiniciar) e deletá-lo depois, confirmando que todos os seus jogos foram analisados e adicionados à fila.

Em vez de usar o arquivo `BotName.keys`, você também pode usar o API endpoint IPC, ou até mesmo combinar ambos.

Depois de algum tempo devem ser gerados os arquivos `BotName.keys.used` e `BotName.keys.unused`. Esses arquivos contêm os resultados de nosso processo de resgate. Por exemplo, você pode renomear o arquivo `NomeDoBot.keys.unused` para `NomeDoBot2.keys` e, portanto, passar as keys não utilizadas para outro bot, já que o bot anterior não fez uso delas. Ou você pode simplesmente copiar e colar as keys não utilizadas para algum outro arquivo e guardá-las para depois, como preferir. Tenha em mente que enquanto o ASF percorre a fila, novas entradas serão adicionadas aos arquivos `used` e `unused`, portanto é recomendado aguardar a fila ser totalmente esvaziada antes usá-los. Se você realmente precisar acessar esses arquivos antes da fila ser totalmente esvaziada, você deve primeiro **mover** o arquivo de saída que você deseja acessar para alguma outra pasta e, **em seguida**, analisá-lo. Isso porque o ASF pode adicionar novos resultados enquanto você está mexendo no arquivoo que pode levar a perda de algumas keys. Por exemplo, digamos que você acesse um arquivo contendo 3 chaves e então o apague, pode ocorrer de o ASF ter adicionado 4 outras keys nele durante esse tempo e elas serão perdidas. Se você deseja acessar esses arquivos, certifique-se de tirá-los da pasta `config` do ASF antes de acessá-los, renomeando-os por exemplo.

Também é possível adicionar jogos extras para serem importados, mesmo tendo alguns jogos já na fila, basta repetir todos os passos acima. O ASF vai adicionar corretamente nossas novas entradas na fila já em curso e tratá-las em tempo.

* * *

## Observações

O resgate de keys em segundo plano usa `OrderedDictionary`, o que significa que suas cd-keys terão a ordem especificada no arquivo (ou chamadas no API pelo IPC) preservadas. Isto significa que você pode (e deve) fornecer uma lista onde cada cd-key só pode ter dependências diretas de outra cd-key listada acima, e não abaixo. Isto significa que se você tem a DLC `D` que requer o jogo `G` para ser ativada, então a cd-key para o jogo `G` **sempre** deve ser incluída antes da cd-key para a DLC `D`. Da mesma forma, se a DLC `D` depender de `A`, `B` e `C`, todos os 3 devem ser incluídos antes (em qualquer ordem, a não ser que eles tenham dependências entre si).

Não seguir o esquema acima resultará em sua DLC não sendo ativada com o estado `DoesNotOwnRequiredApp`, mesmo que sua conta seja elegível para ativá-la depois que terminar a fila. Se você quiser evitar isso você deve se certificar que sua DLC sempre seja incluída depois do jogo base em sua fila.