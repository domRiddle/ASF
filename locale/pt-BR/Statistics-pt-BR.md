# Estatísticas

O desenvolvimento do ASF é sustentado por 3 fatores mais importantes: doações, feedback dos usuários, e estatísticas. Doações influenciam diretamente a nossa vontade de trabalhar no projeto, os feedbacks dos usuários são sempre bons de ler (especialmente comentários positivos), e as estatísticas nos dão informações sobre como nosso software é usado e por quantas pessoas; assim nós podemos saber onde melhorar, o que consertar, e no que focar.

O ASF tem o parâmetro global `Statistics` ativado nas configurações por padrão. Se você quer ver novas versões sendo lançadas, bugs sendo consertados e novas funções sendo implementadas, você deve considerar deixar essa propriedade ativada, para que possamos usar esses dados para providenciar um melhor software para você (mas não apenas isso). Isto é especialmente importante porque o ASF é **gratuito**, e isso é o mínimo que você pode fazer para agradecer: **nos dizer que você está usando o ASF**, já que isso é o que nossas estatísticas estão fazendo.

Nós mantemos o uso de estatísticas no mínimo, e toda informação sendo coletada requer uma explicação prática: o que nós coletamos, para que propósito, e como isso pode ajudar. Tudo isso pode ser encontrado abaixo.

* * *

# Política de privacidade atual

Enquanto a propriedade `Statistics` está ativada, acontecerá o seguinte:

a) Todas as contas sendo usadas no ASF irão se juntar ao nosso **[grupo Steam](https://steamcommunity.com/groups/ascfarm)**. Isso é feito por três motivos:

* Isso envia à **você** anúncios do grupo, especialmente versões novas, problemas críticos, problemas do Steam e outras coisas que são importantes para manter a comunidade atualizada (garantimos nenhum spam não relacionado ao ASF)
* Isso permite que **você** use nosso suporte técnico, fazendo perguntas, resolvendo problemas, relatando erros ou sugerindo melhorias
* Isso permite que nós vejamos quantas contas Steam estão sendo usadas pelo ASF

b) Se sua conta **[não tiver restrições](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663)**, usar o **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication-pt-BR#asf-2fa)**, tiver um **[inventório público](https://steamcommunity.com/my/edit/settings)** com pelo menos 100 itens correspondentes a `MatchableTypes` e inclui `SteamTradeMatcher` em `TradingPreferences`, então o ASF vai se conectar periodicamente com nosso **[servidor](https://asf.justarchi.net)**. Os dados reais consistem no ID exclusivo do ASF (gerado por ele) e as seguintes informações relacionadas à conta:

* Seu identificador Steam (na forma de 64-bit, para gerar ligações)
* Seu apelido (para fins de exibição)
* Seu avatar (hash, para fins de exibição)
* Seu **[token de trocas](https://steamcommunity.com/my/tradeoffers/privacy)** (para que pessoas fora da sua lista de amigos possam te enviar propostas de trocas)
* Seu `MatchableTypes` (para fins de exibição)
* O valor de `MatchEverything` em seu `TradingPreferences` (para fins de exibição e classificação)
* O número total de itens Steam correspondentes a `MatchableTypes` em seu inventário (para fins de exibição e classificação)
* O número total de jogos exclusivos dos quais os itens Steam acima correspondem aos tipos `MatchableTypes` foram obtidos (para fins de exibição e classificação)

O ASF **não** vai coletar quaisquer outros dados não listados acima sem um aviso prévio importante no changelog e sem uma razão prática e muito boa em primeiro lugar. Nós não consideramos as informações acima muito sérias, já que o grupo Steam não identifica de forma alguma se a conta está usando o ASF ou não, enquanto nossa lista pública é habilitada apenas se você ativar intencionalmente o `SteamTradeMatcher`, quando você **deseja** que as pessoas saibam disso e te enviem propostas de troca.

* * *

# Uso dos dados

Todos os valores especificados no ponto b) são usados por nossa **Listagem publica do ASF STM** explicado abaixo e só por isso.

* * *

## Listagem pública do ASF STM

Nossa listagem pública do ASF STM está localizada **[aqui](https://asf.justarchi.net/STM)** e serve ao propósito muito simples de permitir que todos os usuários rapidamente encontrem bots do ASF para trocas de cartas repetidas.

Graças a nossa listagem, cada usuário do ASF ou não, que esteja interessado pode podem encontrar facilmente bots ativos e enviá-los oferta de troca STM, o que ajuda ambos os usuários, **inclusive você** de se livrar de cartas duplicadas e avançar para a conclusão da insígnia. Queríamos criar algo assim há muito tempo, já que **todos** apreciam uma resposta instantânea às ofertas de troca que o ASF inclui, o que pode melhorar drasticamente a eficiência das correspondências assim como as informações sobre a disponibilidade dos bots - até agora era muito difícil fazer uma listagem pública como esta e graças ao ASF isso se tornou muito mais fácil.

### Como exatamente isso funciona

O ASF envia os dados iniciais uma vez após o login, que contém todas as propriedades que a listagem pública utiliza. Em seguida, a cada 10 minutos o ASF envia uma solicitação muito pequena de "pulso" que notifica o nosso servidor de que o bot ainda está sendo executado. Se por algum motivo o pulso não chegar, por exemplo devido a problemas de rede, então o ASF vai tentar reenviá-lo a cada minuto, até o servidor registrá-lo.

Isso permite que o nosso site registre quais contas podem ser usadas para correspondência, bem como se ainda estão ativas. Graças a isso, nosso site pode mostrar todos as contas com ASF 2FA + STM que estavam ativas nos **últimos 15 minutos**.

Os usuários são classificados de acordo com seus inventários (em ordem descendente) - primeiro pelo número de jogos únicos dos quais os itens `MatchableTypes` são obtidos, então o total de itens que correspondem a `MatchableTypes`, além disso, os bots com `MatchEverything` são listados na parte superior com a marca `Any`.

Note que você **não** vai ser exibido no site se você não cumprir todos os requisitos. O ASF nem se comunica com nossos servidores nesse caso, então o ponto b) será inteiramente ignorado para você caso você não habilite o `SteamTradeMatcher` para te ajudar a trocar cartas duplicadas. A listagem pública é compatível somente com a última versão estável do ASF e pode se recusar a mostrar bots desatualizados, especialmente se faltar neles funcionalidades cruciais que só podem ser encontradas em novas versões.

### API

A listagem API do STM só aceita bots ASF no momento. Não há como inserir bots de terceiros em nossa listagem por enquanto (já que não podemos rever seus códigos facilmente e assegurar que eles satisfazem nossa lógica de trocas).

Se você está procurando por uma maneira fácil de acessar nossa lista de forma programática nós temos o endpoint **[/Api/Bots](https://asf.justarchi.net/Api/Bots)** muito simples que você pode usar.

### Aviso

*Todo o conceito, juntamente com a integração entre o site e o ASF ainda está em versão beta - ele pode ser melhorado/mudado com o passar do tempo - até mesmo removido se acharmos que não há muito interesse por essa funcionalidade.*

* * *

## Optando por não participar

Participar das estatísticas **não é obrigatório**, embora altamente incentivado para o futuro do programa. Nós não te julgamos, e se você tem uma necessidade muito grande de esconder o fato de que você está usando o ASF, então você pode desabilitar as estatísticas **totalmente** mudando o parâmetro de configuração global `Statistics` para `false`. Desabilitar as estatísticas torna todo o módulo não-operacional, e ele não fará nenhuma das ações especificadas em nossa política de privacidade acima.

> Por favor, note que desabilitar as estatísticas pode influenciar nosso suporte técnico entre outras coisas que são oferecidas (gratuitamente), como as funcionalidades do ASF e até o próprio programa.