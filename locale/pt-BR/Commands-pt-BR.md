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

![Screenshot](https://i.imgur.com/PPxx7qV.png)

* * *

### Bate-papo em grupo pelo Steam

Muito similar ao anterior, mas desta vez no chat em grupo do Steam. Keep in mind that this option requires properly set `SteamMasterClanID` property, in which case bot will listen for commands also on group's chat (and join it if needed). This can also be used for "talking to yourself" since it doesn't require a dedicated bot account.

* * *

### IPC

Provavelmente a forma mais "complexa" de chamar o ASF, perfeito para ferramentas de terceiros ou scripts, requer que o ASF seja executado em modo servidor e um cliente executando comandos através da interface **[IPC](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC)**.

![Screenshot](https://i.imgur.com/TsAHcM0.png)

* * *

## Comandos

| Comando                                              | Permissão de acesso | Descrição                                                                                                                                                                                                                                                        |
| ---------------------------------------------------- | ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `2Fa <Bots>`                                   | `Master`            | Gera um código **[2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Two-factor-authentication)** temporário para os bot indicados.                                                                                                                           |
| `2Fano <Bots>`                                 | `Master`            | Nega todas as confirmações **[2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Two-factor-authentication)** pendentes para os bots indicados.                                                                                                               |
| `2faok <Bots>`                                 | `Master`            | Aceita todas as confirmações **[2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Two-factor-authentication)** pendentes para os bots indicados.                                                                                                             |
| `addlicense <Bots> <GameIDs>`            | `Operator`          | Ativa as `appIDs` (Rede Steam) ou `subIDs` (Loja Steam) indicadas para as contas bots determinadas (apenas jogos gratuitos).                                                                                                                                     |
| `bl <Bots>`                                    | `Master`            | Lista os usuários bloqueados no módulo de trocas dos bots indicados.                                                                                                                                                                                             |
| `bladd <Bots> <SteamIDs64>`              | `Master`            | Bloqueia a `steamIDs` indicada no módulo de trocas dos bots determinados.                                                                                                                                                                                        |
| `blrm <Bots> <SteamIDs64>`               | `Master`            | Remove o bloqueio das `steamIDs` indicadas no módulo de trocas dos bots determinados.                                                                                                                                                                            |
| `exit`                                               | `Owner`             | Interrompe todo o processo ASF.                                                                                                                                                                                                                                  |
| `farm <Bots>`                                  | `Master`            | Reinicia o modulo de coleta de cartas para os bots indicados.                                                                                                                                                                                                    |
| `help`                                               | `FamilySharing`     | Mostra a ajuda (link para esta página).                                                                                                                                                                                                                          |
| `input <Bots> <Type> <Value>`      | `Master`            | Define o tipo de entrada indicado para o valor dado para os bots indicados, funciona apenas no modo `Headless` - melhor explicado **[abaixo](#input-command)**.                                                                                                  |
| `ib <Bots>`                                    | `Master`            | Lista os apps bloqueados para coleta automática nos bots indicados.                                                                                                                                                                                              |
| `ibadd <Bots> <AppIDs>`                  | `Master`            | Adiciona os `appIDs` indicados à lista de apps bloqueados para coleta automática nos bots determinados.                                                                                                                                                          |
| `ibrm <Bots> <AppIDs>`                   | `Master`            | Remove os `appIDs` indicados da lista de apps bloqueados para coleta automática nos bots determinados.                                                                                                                                                           |
| `iq <Bots>`                                    | `Master`            | Mostra a lista de prioridade de coleta dos bots indicados.                                                                                                                                                                                                       |
| `iqadd <Bots> <AppIDs>`                  | `Master`            | Adiciona os `appIDs` na lista prioritária de coleta automática nos bots indicados.                                                                                                                                                                               |
| `iqrm <Bots> <AppIDs>`                   | `Master`            | Remove os `appIDs` da lista prioritária de coleta automática nos bots indicados.                                                                                                                                                                                 |
| `leave <Bots>`                                 | `Master`            | Faz os bots indicados saírem do chat em grupo. Por razões óbvias, esse comando funciona apenas em chat em grupo.                                                                                                                                                 |
| `loot <Bots>`                                  | `Master`            | Envia todos itens da comunidade Steam que se enquadram como `LootableTypes` dos bots indicados para o usuário definido como `Master` em `SteamUserPermissions` (com o steamID mais baixo caso haja mais de um).                                                  |
| `loot@ <Bots> <RealAppIDs>`              | `Master`            | Envia todos itens da comunidade Steam que se enquadram como `LootableTypes`, e cujo `RealAppIDs` corresponda ao determinado, dos bots indicados para o usuário definido como `Master` em `SteamUserPermissions` (com o steamID mais baixo caso haja mais de um). |
| `loot^ <Bots> <AppID> <ContextID>` | `Master`            | Envia todos itens Steam do `AppID` de `ContextID` determinados dos bots indicados para o usuário definido como `Master` em `SteamUserPermissions` (com o steamID mais baixo caso haja mais de um).                                                               |
| `loot& <Bots>`                             | `Master`            | Muda o status de "looting" dos bots indicados entre habilitado/desabilitado.                                                                                                                                                                                     |
| `nickname <Bots> <Nickname>`             | `Master`            | Muda o apelido Steam dos bots indicados para o informado em `nickname`.                                                                                                                                                                                          |
| `owns <Bots> <AppIDsOrGameNames>`        | `Operator`          | Verifica se os bots indicados já possuem os `appIDs` e/ou `NomeDoJogo` citados (pode ser apenas parte do nome do jogo). Também pode ser `*` para mostrar todos os jogos disponíveis.                                                                             |
| `password <Bots>`                              | `Master`            | Mostra a senha criptografada dos bots indicados (de acordo com `PasswordFormat`).                                                                                                                                                                                |
| `pause <Bots>`                                 | `Operator`          | Para permanentemente o módulo de coleta automática de cartas dos bots indicados. ASF não tentará coletar nessa conta durante essa sessão, a menos que você retome manualmente por meio do comando `resume` ou reinicie o processo.                               |
| `pause~ <Bots>`                                | `FamilySharing`     | Para temporariamente o módulo de coleta automática de cartas dos bots indicados. A coleta será retomada automaticamente na próxima vez que você jogar algum jogo ou quando o bot desconectar. Você pode usar o comando `resume` para voltar a coletar.           |
| `pause& <Bots> <Seconds>`            | `Operator`          | Para temporariamente o módulo de coleta automática de cartas dos bots indicados pela quantidade de segundos indicada em `seconds`. Após esse tempo, a coleta automática de carta é retomada automaticamente.                                                     |
| `play <Bots> <AppIDs,GameName>`          | `Master`            | Muda para o modo de coleta manual - abre o jogo indicado pela `AppIds` no bot determinado, opcionalmente pode-se usar um nome customizado `GameName`. Use o comando `resume` para retornar a coleta automática.                                                  |
| `privacy <Bots> <Settings>`              | `Master`            | Muda as **[configurações de privacidade da Steam](https://steamcommunity.com/my/edit/settings)** dos bots indicados, para opções selecionadas adequadamente conforme explicado **[abaixo](#privacy-settings)**.                                                  |
| `redeem <Bots> <Keys>`                   | `Operator`          | Resgata as `cd-keys` indicadas para a conta dos bots determinados.                                                                                                                                                                                               |
| `redeem^ <Bots> <Modes> <Keys>`    | `Operator`          | Resgata as `cd-keys` indicadas para a conta dos bots determinados, utilizando modos `modes` conforme explicados **[abaixo](#redeem-modes)**.                                                                                                                     |
| `rejoinchat <Bots>`                            | `Operator`          | Força os bots indicados a voltarem para o chat em grupo do grupo indicado em `SteamMasterClanID`.                                                                                                                                                                |
| `restart`                                            | `Owner`             | Reinicia o processo ASF.                                                                                                                                                                                                                                         |
| `resume <Bots>`                                | `FamilySharing`     | Retoma a coleta automática dos bots indicados. Veja também `pause`, `play`.                                                                                                                                                                                      |
| `start <Bots>`                                 | `Master`            | Inicia os bots indicados.                                                                                                                                                                                                                                        |
| `stats`                                              | `Owner`             | Mostra estatísticas do processo, tais como uso de memória gerenciada.                                                                                                                                                                                            |
| `status <Bots>`                                | `FamilySharing`     | Mostra o status dos bots indicados.                                                                                                                                                                                                                              |
| `stop <Bots>`                                  | `Master`            | Para os bots indicados.                                                                                                                                                                                                                                          |
| `transfer <Bots> <Modes> <Bot>`    | `Master`            | Envia dos bots indicados para outro `Bot` indicado, todos os itens do inventário que correspondam com os modos `modes` determinados, conforme explicado **[abaixo](#transfer-modes)**.                                                                           |
| `unpack <Bots>`                                | `Master`            | Abre todos os pacotes de cartas armazenados no inventario dos bots indicados.                                                                                                                                                                                    |
| `update`                                             | `Owner`             | Verifica atualizações para o ASF no GitHub (isso é feito automaticamente a cada 24 horas se `AutoUpdates` estiver ativo).                                                                                                                                        |
| `version`                                            | `FamilySharing`     | Mostra a versão do ASF.                                                                                                                                                                                                                                          |

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

| Valor      | Pseudônimo | Descrição                                                     |
| ---------- | ---------- | ------------------------------------------------------------- |
| All        | A          | O mesmo que habilitar todos os itens abaixo                   |
| Background | BG         | Fundo de perfil para usar em seu perfil Steam                 |
| Booster    | BO         | Pacote de cartas                                              |
| Card       | C          | Steam trading card, being used for crafting badges (non-foil) |
| Emoticon   | E          | Emoticon to use in Steam Chat                                 |
| Foil       | F          | Foil variant of `Card`                                        |
| Gems       | G          | Steam gems being used for crafting boosters, sacks included   |
| Unknown    | U          | Every type that doesn't fit in any of the above               |

For example, in order to send trading cards and foils from `MyBot` to `MyMain`, you'd execute:

`transfer MyBot C,F MyMain`

* * *

## `Entrada` de comando

Input command can be used only in `Headless` mode, for inputting given data via **[IPC](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC)** or Steam chat when ASF is running without support for user interaction.

General syntax is `input <Bots> <Type> <Value>`.

`<Type>` is case-insensitive and defines input type recognized by ASF. Currently ASF recognizes following types:

| Type                    | Descrição                                                                  |
| ----------------------- | -------------------------------------------------------------------------- |
| DeviceID                | 2FA device identificator, if missing from `.maFile`.                       |
| Login                   | `SteamLogin` bot config property, if missing from config.                  |
| Password                | `SteamPassword` bot config property, if missing from config.               |
| SteamGuard              | Auth code sent on your e-mail if you're not using 2FA.                     |
| SteamParentalPIN        | `SteamParentalPIN` bot config property, if missing from config.            |
| TwoFactorAuthentication | 2FA token generated from your mobile, if you're using 2FA but not ASF 2FA. |

`<Value>` is value set for given type. Currently all values are strings.

### Exemplo

Let's say that we have a bot that is protected by SteamGuard in non-2FA mode. We want to launch that bot with `Headless` set to true.

In order to do that, we need to execute following commands:

`start MySteamGuardBot` -> Bot will attempt to log in, fail due to AuthCode needed, then stop due to running in `Headless` mode. We need this in order to make Steam network send us auth code on our e-mail - if there was no need for that, we'd skip this step entirely.

`input MySteamGuardBot SteamGuard ABCDE` -> We set `SteamGuard` input of `MySteamGuardBot` bot to `ABCDE`. Of course, `ABCDE` in this case is auth code that we got on our e-mail.

`start MySteamGuardBot` -> We start our (stopped) bot again, this time it automatically uses auth code that we set in previous command, properly logging in, then clearing it.

In the same way we can access 2FA-protected bots (if they're not using ASF 2FA), as well as setting other required properties during runtime.