# Ativador de jogos em segundo plano

O ativador de jogos em segundo plano é uma característica especial embutida no ASF que permite que você importe uma lista de cd-keys do Steam (juntamente com seus nomes) para serem resgatadas em segundo plano. Isso é especialmente útil se você tem um monte de keys para resgatar e é certo que você atingirá o **[status](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ#what-is-the-meaning-of-status-when-redeeming-a-key)** `RateLimited` antes de terminar.

Resgate de keys em segundo plano foi feito para uso em apenas um bot, o que significa que ele não faz uso de `RedeemingPreferences`. Esse recurso pode ser usado junto com (ou no lugar do) **[comando](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** `redeem`, se necessário.

* * *

## Importar

O processo de importar pode ser feito de duas maneiras - tanto usando um arquivo, quanto IPC.

### Arquivo

ASF reconhecerá em seu diretório `config`, um arquivo chamado `NomeDoBot.keys`, onde `NomeDoBot` é o nome do seu bot. Esse arquivo deve conter uma estrutura fixa contendo o nome do jogo e a cd-key, separados por um caractere de tabulação e terminando com uma nova linha. Se várias tabulações forem usadas, então a primeira entrada é considerada o nome do jogo, a última entrada é considerada uma cd-key e tudo no meio é ignorado. Por exemplo:

    POSTAL 2    ABCDE-EFGHJ-IJKLM
    Domino Craft VR 12345-67890-ZXCVB
    A Week of Circus Terror POIUY-KJHGD-QWERT
    Terraria    IssoÉIgnorado    IssoTambémÉIgnorado    ZXCVB-ASDFG-QWERT
    

O ASF importará esse arquivo, seja na inicialização do bot ou posteriormente durante a execução. Depois da análise bem sucedida do seu arquivo e eventual omissão de entradas inválidas, todos os jogos corretamente detectados serão adicionados à fila em segundo plano, e o arquivo `NomeDoBot.keys` será removido da pasta `config`.

### IPC

Além de usar o arquivo keys mencionado acima, o ASF também exibe **[API endpoint](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#post-apigamestoredeeminbackgroundbotname)** `GamesToRedeemInBackground` que pode ser executado por qualquer ferramenta IPC, incluindo nossa interface IPC. Usar o IPC pode ser mais poderoso, já que você pode fazê-lo analisando-se propriamente, tal como usando um delimitador customizado ao invés de ser forçado a um caractere de tabulação.

* * *

## Fila

Assim que os jogos são importados com êxito, eles são adicionados à fila. O ASF percorre automaticamente a fila em segundo plano enquanto o bot continuar conectado a rede Steam, e a fila não estiver vazia. Uma key que tentou ser resgatada e não resultou em `RateLimited` é removida da lista, com seu status propriamente escrito em um arquivo na pasta `config` - sendo `NomeDoBot.keys.used` se a chave foi usada no processo (por ex.: `NoDetail`, `BadActivationCode`, `DuplicateActivationCode`), ou `NomeDoBot.keys.unused` caso contrário. O ASF usa intencionalmente o nome do jogo que você forneceu uma vez que não é garantido que a key retorne um nome significativo pela Steam - dessa forma você pode marcar suas keys até mesmo com nomes customizados se precisar/quiser.

Se durante o processo a conta atingir o status `RateLimited`, a fila é suspensa temporariamente por uma hora inteira para esperar o fim do cooldown. Depois disso, o processo continua de onde parou, até que a fila inteira esteja vazia.

* * *

## Exemplo

Vamos supor que você tem uma lista de 100 chaves. Em primeiro lugar, você deve criar um novo arquivo `NomeDoBot.keys.new` na pasta `config` do ASF. Nós adicionamos a extensão `.new` para que o ASF, saiba que não deve pegar esse arquivo imediatamente no momento em que é criado (como é um novo arquivo vazio, não está pronto para importação ainda).

Agora você pode abrir o novo arquivo e copiar e colar a lista de 100 keys nele, arrumando a formatação se necessário. Após as correções, o arquivo `NomeDoBot.keys.new` terá exatamente 100 linhas (ou 101, com a última quebra de linha), cada linha, tendo uma estrutura de `NomeDoJogo\tcd-key\n`, onde `\t` é o caractere de tabulação e `\n` é a quebra de linha.

Agora você está pronto para renomear este arquivo de `NomeDoBot.keys.new` para `NomeDoBot.keys` para que o ASF saiba que está pronto para ser pego. No momento que você fizer isso, o ASF vai importar automaticamente o arquivo (sem necessidade de reiniciar) e deletá-lo depois, confirmando que todos os seus jogos foram analisados e adicionados à fila.

Em vez de usar o arquivo `NomeDoBot.keys`, você também pode usar o API endpoint, pelo IPC, ou até mesmo combinar ambos se quiser.

Depois de algum tempo, devem ser gerados os arquivos `NomeDoBot.keys.used` e `NomeDoBot.keys.unused`. Esses arquivos contêm os resultados de nosso processo de resgate. Por exemplo, você pode renomear o arquivo `NomeDoBot.keys.unused` para `NomeDoBot2.keys` e, portanto, passar as keys não utilizadas para outro bot, já que o bot anterior não fez uso delas. Ou você pode simplesmente copiar e colar as keys não utilizadas para algum outro arquivo e guardá-las para depois, como preferir. Tenha em mente que enquanto o ASF percorre a fila, novas entradas serão adicionadas aos arquivos `used` e `unused`, portanto é recomendado aguardar a fila ser totalmente esvaziada antes de fazer uso deles. Se você absolutamente precisar acessar esses arquivos antes da fila ser totalmente esvaziada, você deve primeiro **mover** o arquivo de saída que você deseja acessar para alguma outra pasta e, **em seguida**, analisá-lo. Isso porque o ASF pode adicionar alguns novos resultados enquanto você está mexendo no arquivo e isso pode potencialmente levar a perda de algumas keys caso você acesse um arquivo contendo, por exemplo, 3 chaves, e então apague o mesmo, sem perceber o fato de que o ASF havia adicionado 4 outras chaves nele durante esse tempo. Se você deseja acessar esses arquivos, certifique-se de tirá-los da pasta `config` do ASF antes de acessá-los, por exemplo por renomeá-los.

Também é possível adicionar jogos extras para importar, mesmo tendo alguns jogos já na fila, basta repetir todos os passos acima. O ASF vai adicionar corretamente nossas novas entradas na fila já em curso e tratá-las em tempo.

* * *

## Observações

O resgate de keys em segundo plano usa `OrderedDictionary`, o que significa que suas cd-keys terão a ordem especificada no arquivo (ou chamadas no API pelo IPC) preservadas. Isto significa que você pode (e deve) fornecer uma lista onde cada cd-key só pode ter dependências diretas de outra cd-key listada acima, e não abaixo. Por exemplo, isto significa que se você tem a DLC `D` que requer o jogo `G` para ser ativada, então a cd-key para o jogo `G` **sempre** deve ser incluída antes da cd-key para a DLC `D`. Da mesma forma, se a DLC `D` depender de `A`, `B` e `C`, todos os 3 devem ser incluídos antes (em qualquer ordem, a menos que eles tenham dependências entre si).

Não seguir o esquema acima resultará em sua DLC não sendo ativada com o status `DoesNotOwnRequiredApp`, mesmo que sua conta seja elegível para ativá-la depois que terminar a fila. Se você quiser evitar isso, então você deve se certificar que sua DLC sempre seja incluída depois do jogo base em sua fila.