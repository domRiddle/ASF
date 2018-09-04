# Comandos

O ASF suporta uma variedade de comandos, que podem ser usados para controlar o comportamento dos processos e dos bots.

Os comandos abaixo podem ser enviados para o bot de três formas diferentes:

- Através do chat privado do Steam
- Através do chat em grupo do Steam
- Através do **[IPC](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC#post-apicommandcommand)**

Tenha em mente que a interação do ASF requer de você a permissão para utilizar os comandos de acordo com as permissões do ASF. Confira as configurações em `SteamUserPermissions` e `SteamOwnerID` para mais detalhes.

Todos os comandos abaixo são afetados pela **[propriedade de configuração global](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#global-config)** `CommandPrefix`, que é, por padrão, `!`. Isto significa que para executar, por exemplo, o comando `status`, você, na verdade, deve escrever `!status` (ou o `CommandPrefix` configurado de sua escolha).

* * *

### Chat privado do Steam

Definitivamente o método mais fácil de interagir com o ASF - basta executar o comando para o bot ASF que está sendo executado no processo do ASF. Obviamente, não pode fazer isso se você estiver executando o ASF com uma única conta bot que é a sua própria conta.

![Captura da tela](https://i.imgur.com/PPxx7qV.png)

* * *

### Bate-papo em grupo pelo Steam

Muito similar ao anterior, mas desta vez no chat em grupo do Steam. Tenha em mente que esta opção requer que a propriedade `SteamMasterClanID` esteja definida corretamente, pois nesse caso o bot ouvirá os comandos também no bate-papo em grupo (e até entrará no mesmo, se necessário). Ele também pode ser usado para "falar sozinho" já que não exige uma conta bot dedicada.

* * *

### IPC

Provavelmente a forma mais "complexa" de chamar o ASF, perfeito para ferramentas de terceiros ou scripts, requer que o ASF seja executado em modo servidor e um cliente executando comandos através da interface **[IPC](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC)**.

![Captura da tela](https://i.imgur.com/TsAHcM0.png)

* * *

## Comandos

| Comando                                              | Permissão de acesso | Descrição                                                                                                                                                                                             |
| ---------------------------------------------------- | ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `2Fa <Bots>`                                   | `Master`            | Gera um código **[2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Two-factor-authentication)** temporário para os bot indicados.                                                                |
| `2Fano <Bots>`                                 | `Master`            | Nega todas as confirmações **[2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Two-factor-authentication)** pendentes para os bots indicados.                                                    |
| `2faok <Bots>`                                 | `Master`            | Aceita todas as confirmações **[2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Two-factor-authentication)** pendentes para os bots indicados.                                                  |
| `addlicense <Bots> <GameIDs>`            | `Operator`          | Ativa as `appIDs` (Rede Steam) ou `subIDs` (Loja Steam) indicadas para as contas bots determinadas (apenas jogos gratuitos).                                                                          |
| `bl <Bots>`                                    | `Master`            | Lista os usuários bloqueados no módulo de trocas dos bots indicados.                                                                                                                                  |
| `bladd <Bots> <SteamIDs64>`              | `Master`            | Bloqueia a `steamIDs` indicada no módulo de trocas dos bots determinados.                                                                                                                             |
| `blrm <Bots> <SteamIDs64>`               | `Master`            | Remove o bloqueio das `steamIDs` indicadas no módulo de trocas dos bots determinados.                                                                                                                 |
| `exit`                                               | `Owner`             | Interrompe todo o processo ASF.                                                                                                                                                                       |
| `farm <Bots>`                                  | `Master`            | Reinicia o modulo de coleta de cartas para os bots indicados.                                                                                                                                         |
| `help`                                               | `FamilySharing`     | Mostra a ajuda (link para esta página).                                                                                                                                                               |
| `input <Bots> <Type> <Value>`      | `Master`            | Define o tipo de entrada indicado para o valor dado para os bots indicados, funciona apenas no modo `Não-interativo` - melhor explicado **[abaixo](#input-command)**.                                 |
| `ib <Bots>`                                    | `Master`            | Lista os apps bloqueados para coleta automática nos bots indicados.                                                                                                                                   |
| `ibadd <Bots> <AppIDs>`                  | `Master`            | Adiciona os `appIDs` indicados à lista de apps bloqueados para coleta automática nos bots determinados.                                                                                               |
| `ibrm <Bots> <AppIDs>`                   | `Master`            | Remove os `appIDs` indicados da lista de apps bloqueados para coleta automática nos bots determinados.                                                                                                |
| `iq <Bots>`                                    | `Master`            | Mostra a lista de prioridade de coleta dos bots indicados.                                                                                                                                            |
| `iqadd <Bots> <AppIDs>`                  | `Master`            | Adiciona os `appIDs` na lista prioritária de coleta automática nos bots indicados.                                                                                                                    |
| `iqrm <Bots> <AppIDs>`                   | `Master`            | Remove os `appIDs` da lista prioritária de coleta automática nos bots indicados.                                                                                                                      |
| `loot <Bots>`                                  | `Master`            | Sends all `LootableTypes` Steam community items of given bot instances to `Master` user defined in their `SteamUserPermissions` (with lowest steamID if more than one).                               |
| `loot@ <Bots> <RealAppIDs>`              | `Master`            | Sends all `LootableTypes` Steam community items matching given `RealAppIDs` of given bot instances to `Master` user defined in their `SteamUserPermissions` (with lowest steamID if more than one).   |
| `loot^ <Bots> <AppID> <ContextID>` | `Master`            | Sends all Steam items from given `AppID` of `ContextID` of given bot instances to `Master` user defined in their `SteamUserPermissions` (with lowest steamID if more than one).                       |
| `loot& <Bots>`                             | `Master`            | Switches looting of given bot instances between enabled/disabled state.                                                                                                                               |
| `nickname <Bots> <Nickname>`             | `Master`            | Changes Steam nickname of given bot instances to given `nickname`.                                                                                                                                    |
| `owns <Bots> <AppIDsOrGameNames>`        | `Operator`          | Checks if given bot instances already own given `appIDs` and/or `gameNames` (can be part of the game's name). It can also be `*` to show all games available.                                         |
| `password <Bots>`                              | `Master`            | Prints encrypted password of given bot instances (in use with `PasswordFormat`).                                                                                                                      |
| `pause <Bots>`                                 | `Operator`          | Permanently pauses automatic cards farming module of given bot instances. ASF will not attempt to farm current account in this session, unless you manually `resume` it, or restart the process.      |
| `pause~ <Bots>`                                | `FamilySharing`     | Temporarily pauses automatic cards farming module of given bot instances. Farming will be automatically resumed on the next playing event, or bot disconnect. You can `resume` farming to unpause it. |
| `pause& <Bots> <Seconds>`            | `Operator`          | Temporarily pauses automatic cards farming module of given bot instances for given amount of `seconds`. After delay, cards farming module is automatically resumed.                                   |
| `play <Bots> <AppIDs,GameName>`          | `Master`            | Switches to manual farming - launches given `AppIDs` on given bot instances, optionally also with custom `GameName`. Use `resume` for returning to automatic farming.                                 |
| `privacy <Bots> <Settings>`              | `Master`            | Changes **[Steam privacy settings](https://steamcommunity.com/my/edit/settings)** of given bot instances, to appropriately selected options explained **[below](#privacy-settings)**.                 |
| `redeem <Bots> <Keys>`                   | `Operator`          | Redeems given `cd-keys` on given bot instances.                                                                                                                                                       |
| `redeem^ <Bots> <Modes> <Keys>`    | `Operator`          | Redeems given `cd-keys` on given bot instances, using given `modes` explained **[below](#redeem-modes)**.                                                                                             |
| `rejoinchat <Bots>`                            | `Operator`          | Forces given bot instances to rejoin their `SteamMasterClanID` group chat.                                                                                                                            |
| `restart`                                            | `Owner`             | Restarts ASF process.                                                                                                                                                                                 |
| `resume <Bots>`                                | `FamilySharing`     | Resumes automatic farming of given bot instances. Also see `pause`, `play`.                                                                                                                           |
| `start <Bots>`                                 | `Master`            | Starts given bot instances.                                                                                                                                                                           |
| `stats`                                              | `Owner`             | Prints process statistics, such as managed memory usage.                                                                                                                                              |
| `status <Bots>`                                | `FamilySharing`     | Prints status of given bot instances.                                                                                                                                                                 |
| `stop <Bots>`                                  | `Master`            | Stops given bot instances.                                                                                                                                                                            |
| `transfer <Bots> <Modes> <Bot>`    | `Master`            | Sends from given bot instances to given `Bot` instance, all inventory items that are matching given `modes`, explained **[below](#transfer-modes)**.                                                  |
| `unpack <Bots>`                                | `Master`            | Unpacks all booster packs stored in the inventory of given bot instances.                                                                                                                             |
| `update`                                             | `Owner`             | Checks GitHub for ASF updates (this is done automatically every 24 hours if `AutoUpdates`).                                                                                                           |
| `version`                                            | `FamilySharing`     | Prints version of ASF.                                                                                                                                                                                |

* * *

### Notas

Todos os comandos não diferenciam maiúsculas de minúsculas, mas seus argumentos (tais como nomes dos bots) geralmente sim.

O argumento `<Bots>` é opcional em todos os comandos. Quando especificado, o comando é executado no bot indicado. Quando omitido, o comando é executado no bot que recebe o comando. Em outras palavras, `status A` enviado para o bot `B` é o mesmo que enviar o `status` para o bot `A`.

O **acesso** ao comando se define com no **mínimo** `EPermission` em `SteamUserPermissions` que é requirido para usar o comando, com exceção do `Owner` que tem a `SteamOwnerID` definida no arquivo de configuração global (e é a maior permissão possível).

Múltiplos argumentos, tais como `<Bots>`, `<Keys>` ou `<AppIDs>` significam que o comando suporta diversos argumentos do tipo indicado, separados por vírgula. Por exemplo, `status <Bots>` pode ser usado da seguinte forma: `status MyBot,MyOtherBot,Primary`. Isso fará com que o comando seja executado em **todos os bots de destino** da mesma forma que seria caso você mandasse o comando `status` para cada bot em uma janela separada. Por favor, note que não há espaço após a `,`.

O ASF usa todos os caracteres em branco como possíveis delimitadores para um comando, como espaço e caracteres de quebra de linha. Isto significa que você não precisa usar o espaço para delimitar seus argumentos, você pode usar qualquer outro caractere de espaço em branco (tais como tab ou quebra de linha).

O ASF vai "combinar" os argumentos extras como sendo do tipo múltiplo do último argumento alcançado. Isso significa que `redeem bot key1 key2 key3` para `redeem <Bots> <Keys>` funcionará exatamente da mesma forma que `redeem bot key1,key2,key3`. Junto com o fato de aceitar a quebra de linha como comando delimitador, isso torna possível que você escreva `redeem bot` e então cole uma lista de keys separadas por qualquer caractere delimitador aceitável (tal qual a quebra de linha), ou o delimitador padrão do ASF `,`. Tenha em mente que este truque só pode ser usado para as variantes de comando que usam a maior quantidade de argumentos.

Como você leu acima, um caractere de espaço está sendo usado como um delimitador para um comando, portanto não pode ser usado nos argumentos. Porém, também como foi descrito acima, o ASF pode juntar argumentos fora do padrão, o que significa que você, na verdade, pode usar um caractere de espaço em um argumento que for definido como último em um determinado comando. Por exemplo,`nickname bob Great Bob` irá definir corretamente o apelido do bot `bob` como "Great Bob". De forma semelhante, você pode verificar nomes que contenham espaços no comando `owns`.

Por favor, note que enviar comandos para o chat em grupo funciona como uma repetidora - se você enviar `redeem X` para 3 de seus bots que estejam no chat junto com você, vai ter o mesmo resultado que enviar `redeem X` para cada um deles em particular. Na maioria casos, **não é isso que você quer**, e em vez disso, você deve usar o comando que está sendo enviado para o `bot desejado` para **um único bot em uma janela particular**. O ASF suporta chat em grupo, já que em muitos casos isso pode ser uma fonte útil de comunicação, mas você quase nunca deve executar qualquer comando no chat em grupo se tiverem 2 ou mais bots nele, a não ser que você entenda completamente o comportamento do ASF descrito aqui e você, de fato, queira repetir o mesmo comando para todo bot que esteja na mesma conversa.

*E mesmo nesse caso você deve usar o chat privado com a sintaxe `<Bots>`.*

* * *

Alguns comandos também estão disponíveis com seus pseudônimos, para facilitar a digitação:

| Comando      | Pseudônimo |
| ------------ | ---------- |
| `owns ASF`   | `oa`       |
| `status ASF` | `sa`       |
| `redeem`     | `r`        |
| `redeem^`    | `r^`       |

* * *

Não é necessário ter qualquer conta extra para executar comandos através do chat Steam - você pode criar um grupo, definir o `SteamMasterClanID` corretamente a esse grupo recém-criado e então se dar acesso através do `SteamOwnerID` ou ` SteamUserPermissions` do seu próprio bot. Desta forma, o bot ASF (você) vai se juntar ao grupo e ao chat do grupo selecionado e atender aos comandos da sua própria conta. Você pode se juntar a esta sala de chat a fim enviar comandos para si mesmo (já que você estará enviando comandos para a sala de bate-papo e o ASF, estando na mesma sala, os receberá, mesmo parecendo que apenas sua conta esteja lá). Além disso, você também pode usar o **[IPC](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC)**, mas usar a sala de chat é muito mais fácil, e se você tem acesso a uma conta alternativa, então usá-lo ao invés do outro é mais fácil ainda.

* * *

Ao usar **IPC**, tenha em mente que:

- Os comandos não precisam ser prefixados por `CommandPrefix`, o ASF faz isso para você automaticamente caso seja necessário
- Ao usar comandos que se baseiam em um `bot geral`, o ASF vai escolher **qualquer** um dos bots atualmente ativados, portanto é altamente recomendado usar comandos para o `bot desejado` em vez disso.

* * *

### Argumento `<Bots>`

O argumento `<Bots>` é uma variante especial de múltiplos argumentos, além de aceitar diversos valores ele também oferece funcionalidades extras.

Primeiro e acima de tudo, há uma palavra-chave especial do `ASF` que atua como "todos os bots no processo", então o comando `status ASF` é igual a `status de todos,os,seus,bots,listados,aqui`. Isso também pode ser usado para identificar facilmente os bots que você tem acesso, já que a palavra-chave `ASF`, apesar de se dirigir a todos os bots, resultará em resposta apenas daqueles bots para os quais você pode, de fato, enviar comandos.

O argumento `<Bots>` suporta uma sintaxe de "classe" especial, o que te permite escolher uma série de bots mais facilmente. A sintaxe geral para `<Bots>`, nesse caso, é `PrimeiroBot...ÚltimoBot`. Por exemplo, se você tem bots chamados `A, B, C, D, E, F`, você pode executar `status B..E`, que é igual a `status B, C, D, E`, neste caso. Ao usar essa sintaxe, o ASF usará a ordem alfabética a fim de determinar quais bots estão na classe especificada. Tanto o `PrimeiroBot` quanto o `ÚltimoBot` devem ser nomes válidos de bots reconhecidos por ASF, caso contrário a sintaxe é totalmente ignorada.

Além de sintaxe de classe descrita acima, o argumento `<Bots>` também suporta correspondência de **[expressão regular](https://en.wikipedia.org/wiki/Regular_expression)**. Você pode ativar o padrão de expressão regular usando `r!<pattern>` como um nome de bot, onde `r!` é o ativador ASF para correspondência de expressão regular e `<pattern>` é o seu padrão de expressão regular. Um exemplo de comando de bot baseado em expressão regular seria `status r! \d{3}` que enviará o comando `status` para bots que tenham o nome composto por 3 dígitos (por exemplo, `123` e `981`). Sinta-se a vontade para dar uma olhada nos **[documentos](https://docs.microsoft.com/en-us/dotnet/standard/base-types/regular-expression-language-quick-reference)** para mais explicações e mais exemplos de padrões de expressão regular disponíveis.

* * *

## Configurações de `privacidade`

`<Settings>` aceita argumentos com **até 6** opções diferentes, separadas, como de costume, por vírgula, que é o delimitador padrão do ASF. Esses argumentos são, em ordem:

| Argumento | Nome           | Filho de   |
| --------- | -------------- | ---------- |
| 1         | Profile        |            |
| 2         | OwnedGames     | Profile    |
| 3         | Playtime       | OwnedGames |
| 4         | Inventory      | Profile    |
| 5         | InventoryGifts | Inventory  |
| 6         | Comments       | Profile    |

Para a descrição dos campos acima, visite as **[configurações de privacidade do Steam](https://steamcommunity.com/my/edit/settings)**.

Enquanto os valores válidos para todas elas são:

| Valor | Nome          |
| ----- | ------------- |
| 1     | `Private`     |
| 2     | `FriendsOnly` |
| 3     | `Public`      |

Você pode tanto usar um nome que não distingue maiúsculas de minúsculas, quanto um valor numérico. Argumentos que foram omitidos serão definidos por padrão como `Privado`. É importante notar a relação entre argumentos filhos e pais especificados acima, uma vez que um filho nunca pode ter permissão mais ampla que o seu pai. Por exemplo, você **não pode** ter jogos `Públicos` se o seu perfil for `privado`.

### Exemplo

Se você deseja definir **todas as** configurações de privacidade do seu bot chamado `Main` para `Privado`, você pode usar qualquer um comandos abaixo:

    privacy Main 0
    privacy Main Private
    

Isso acontece porque o ASF assumirá automaticamente todas as outras configurações como sendo `Privada`, então não há nenhuma necessidade de defini-las. Por outro lado, se você gostaria de definir todas as configurações de privacidade para `Público`, então você deve usar qualquer um dos comandos abaixo:

    privacy Main 3,3,3,3,3,3
    privacy Main Public,Public,Public,Public,Public,Public
    

Desta forma você pode também definir opções independentes da forma que preferir:

    privacy Main Public,FriendsOnly,Private,Public,Private,Public
    

O comando acima vai definir o perfil como público, jogos na biblioteca como apenas amigos, tempo de jogo como privado, inventário como público, presentes no inventário como privado e os comentários no perfil como público. Você pode ter o mesmo resultado com valores numéricos, se você quiser.

Lembre-se que um argumento filho nunca pode ter permissão mais ampla que o seu pai. Consulte a relação de argumentos para as opções disponíveis.

* * *

## Métodos `redeem^`

O comando `redeem^`permite que você ajuste os métodos que serão usados em um cenário individual de resgate. Isso funciona como uma substituição temporária da **[propriedade de configuração do bot](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#bot-config)** `RedeemingPreferences`.

O argumento `<Modes>` aceita vários valores de métodos, como de costume separados por uma vírgula. Valores disponíveis de métodos são especificados abaixo:

| Valor | Nome                  | Descrição                                                                  |
| ----- | --------------------- | -------------------------------------------------------------------------- |
| FD    | ForceDistributing     | Força a ativação da preferência de resgate `Distributing`                  |
| FF    | ForceForwarding       | Força a ativação da preferência de resgate `Forwarding`                    |
| FKMG  | ForceKeepMissingGames | Força a ativação da preferência de resgate `KeepMissingGames`              |
| SD    | SkipDistributing      | Força a desativação da preferência de resgate `Distributing`               |
| SF    | SkipForwarding        | Força a desativação da preferência de resgate `Forwarding`                 |
| SI    | SkipInitial           | Pula o resgate de chave no bot inicial                                     |
| SKMG  | SkipKeepMissingGames  | Força a desativação da preferência de resgate `KeepMissingGames`           |
| V     | Validate              | Valida a formatação correta das keys e ignora automaticamente as inválidas |

Por exemplo, gostaríamos de resgatar 3 chaves em qualquer um dos nossos bots que ainda não possuem os jogos, mas não nosso bot `primário`. Para isso nós podemos usar:

`redeem^ primary FF,SI key1,key2,key3`

* * *

## Métodos `transfer`

O argumento `<Modes>` aceita vários valores de métodos, como de costume separados por uma vírgula. Valores disponíveis de métodos são especificados abaixo:

| Valor      | Pseudônimo | Descrição                                                                         |
| ---------- | ---------- | --------------------------------------------------------------------------------- |
| All        | A          | O mesmo que habilitar todos os itens abaixo                                       |
| Background | BG         | Fundo de perfil para usar em seu perfil Steam                                     |
| Booster    | BO         | Pacote de cartas                                                                  |
| Card       | C          | Cartas colecionáveis Steam, sendo usadas para fabricar insígnias (não brilhantes) |
| Emoticon   | E          | Emoticons para uso no Bate-papo Steam                                             |
| Foil       | F          | Variante brilhante da `carta`                                                     |
| Gems       | G          | Gemas Steam usadas para criar pacotes de cartas, incluindo as empacotadas         |
| Unknown    | U          | Todos os tipos que não se encaixam em nenhuma das opções acima                    |

Por exemplo, para enviar cartas colecionáveis e brilhantes do `MyBot` para o `MyMain`, você executaria:

`transfer MyBot C,F MyMain`

* * *

## Comando `input`

O comando input só pode ser usado no modo `Não-interativo`, para enviar os dados indicados via **[IPC](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC)** ou bate-papo Steam quando o ASF estiver rodando sem suporte para interação do usuário.

A sintaxe geral é `input<Bots><Type><Value>`.

`<Type>` diferencia maiúsculas de minúsculas e define o tipo de entrada reconhecida pelo ASF. Atualmente, o ASF reconhece os seguintes tipos:

| Tipo                    | Descrição                                                                                    |
| ----------------------- | -------------------------------------------------------------------------------------------- |
| DeviceID                | Identificador de dispositivo 2FA, caso esteja faltando no `.maFile`.                         |
| Login                   | Propriedade de configuração do bot `SteamLogin`, caso esteja faltando na config.             |
| Password                | Propriedade de configuração do bot `SteamPassword`, caso esteja faltando na config.          |
| SteamGuard              | Código de autenticação enviado para o seu-email se você não estiver usando o 2FA.            |
| SteamParentalPIN        | Propriedade de configuração do bot `SteamParentalPIN`, caso esteja faltando na config.       |
| TwoFactorAuthentication | Token de 2FA gerado a partir de seu celular, se você estiver usando o 2FA mas não o ASF 2FA. |

`<Value>` é o valor definido para o tipo indicado. Atualmente, todos os valores são strings.

### Exemplo

Digamos que temos um bot que é protegido pelo SteamGuard no modo não-2FA. Queremos iniciar esse bot com `Não-interativo` definido como true.

Para fazer isso, precisamos executar o seguintes comandos:

`start MySteamGuardBot`-> O bot irá tentar logar, falhar devido a necessidade do AuthCode, e, em seguida, parar devido à execução em modo `Não-interativo`. Precisamos disso para fazer a rede Steam nos enviar o código de autenticação no e-mail - se não houvesse necessidade disso, pularíamos essa etapa inteiramente.

`input MySteamGuardBot SteamGuard ABCDE` -> Definimos a entrada `SteamGuard` do bot `MySteamGuardBot` para `ABCDE`. Claro, `ABCDE` neste caso é o código de autenticação que recebemos no nosso e-mail.

`start MySteamGuardBot`-> Nós iniciamos nosso bot (parado) novamente, desta vez, ele usa automaticamente o código de autenticação que definimos no comando anterior, ele loga devidamente, limpando-o em seguida.

Da mesma forma, podemos acessar bots protegidos por 2FA (se eles não estiverem usando o ASF 2FA), bem como definir outras propriedades durante tempo de execução.