# Estatísticas

O desenvolvimento do ASF é sustentado por 3 fatores mais importantes: doações, feedback dos usuários, e estatísticas. Doações influenciam diretamente a nossa vontade de trabalhar no projeto, os feedbacks dos usuários são sempre bons de ler (especialmente comentários positivos), e as estatísticas nos dão informações sobre como nosso software é usado e por quantas pessoas; assim nós podemos saber onde melhorar, o que consertar, e no que focar.

O ASF tem o parâmetro global `Statistics` ativado nas configurações por padrão. Se você quer ver novas versões sendo lançadas, bugs sendo consertados e novas funções sendo implementadas, você deve considerar deixar essa propriedade ativada, para que possamos usar esses dados para providenciar um melhor software para você (mas não apenas isso). Isto é especialmente importante porque o ASF é **gratuito**, e isso é o mínimo que você pode fazer para agradecer: **nos dizer que você está usando o ASF**, já que isso é o que nossas estatísticas estão fazendo. ASF does not collect or make use of any data that could be considered private and/or used against you. We keep the usage of statistics to bare minimum, and every single information being collected requires from us to precisely state it here, together with practical explanation. This gives you full insight into what we collect, for what purpose, and how it's supposed to help. All of that info can be found further below.

* * *

## Política de privacidade atual

Enquanto a propriedade `Statistics` está ativada, acontecerá o seguinte:

1. Account being used in ASF will join our **[Steam group](https://steamcommunity.com/gid/103582791440160998)** upon login. Isso é feito por três motivos:

* Isso envia à **você** anúncios do grupo, especialmente versões novas, problemas críticos, problemas do Steam e outras coisas que são importantes para manter a comunidade atualizada (garantimos nenhum spam não relacionado ao ASF)
* Isso permite que **você** use nosso suporte técnico, fazendo perguntas, resolvendo problemas, relatando erros ou sugerindo melhorias
* Isso permite que nós vejamos quantas contas Steam estão sendo usadas pelo ASF

We consider Steam group as a crucial part in regards to ASF community. This is our main communication channel which we use for all important matters in regards to ASF, especially keeping you up with the development, potential issues, eventual warnings and all other matters that you should have access to as an user. We do not benefit from maintenance of that group in any way, it's the place dedicated to ASF users and we consider you part of our community. Since membership of the group in no way identifies you as ASF user, we do not consider this to be a problem in terms of privacy.

2. If your account is **[unrestricted](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663)**, using **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication#asf-2fa)**, has **[public inventory](https://steamcommunity.com/my/edit/settings)** with at least 100 `MatchableTypes` items in it and you intentionally enabled `SteamTradeMatcher` in your `TradingPreferences`, then ASF will periodically communicate with our **[server](https://asf.justarchi.net)** in order to fulfill the enabled functionality. Os dados reais consistem no ID exclusivo do ASF (gerado por ele) e as seguintes informações relacionadas à conta:

* Seu identificador Steam (na forma de 64-bit, para gerar ligações)
* Seu apelido (para fins de exibição)
* Seu avatar (hash, para fins de exibição)
* Seu **[token de trocas](https://steamcommunity.com/my/tradeoffers/privacy)** (para que pessoas fora da sua lista de amigos possam te enviar propostas de trocas)
* Your `MatchableTypes` (for display purposes and matching)
* Value of `MatchEverything` in your `TradingPreferences` (for display purposes and matching)
* Total number of `MatchableTypes` Steam items in your inventory (for display purposes and matching)
* Total number of unique games that above `MatchableTypes` Steam items are made of (for display purposes and matching)

O ASF **não** vai coletar quaisquer outros dados não listados acima sem um aviso prévio importante no changelog e sem uma razão prática e muito boa em primeiro lugar. We do not consider anything above to be a serious matter, and we mention it to let you know what precisely ASF does apart of what you configured it to do yourself, so people can better understand our point of view.

* * *

# Uso dos dados

All values specified in second point are being used for our **Public ASF STM listing**, which is explained below. We do not use any other data for any other purpose.

* * *

## Listagem pública do ASF STM

Our public ASF STM listing is located on **[our website](https://asf.justarchi.net/STM)** and used as a public service for both ASF users that make use of `MatchActively`, as well as ASF and non-ASF users for manual matching.

### Como exatamente isso funciona

O ASF envia os dados iniciais uma vez após o login, que contém todas as propriedades que a listagem pública utiliza. Em seguida, a cada 10 minutos o ASF envia uma solicitação muito pequena de "pulso" que notifica o nosso servidor de que o bot ainda está sendo executado. Se por algum motivo o pulso não chegar, por exemplo devido a problemas de rede, então o ASF vai tentar reenviá-lo a cada minuto, até o servidor registrá-lo.

Isso permite que o nosso site registre quais contas podem ser usadas para correspondência, bem como se ainda estão ativas. Graças a isso, nosso site pode mostrar todos as contas com ASF 2FA + STM que estavam ativas nos **últimos 15 minutos**.

Users are sorted according to their inventories (in descending order) - `MatchEverything` bots with `Any` banner that accept all 1:1 trades, then `MatchableTypes` unique games count, and finally `MatchableTypes` items count.

Note que você **não** vai ser exibido no site se você não cumprir todos os requisitos. ASF won't even bother communicating with our server in this case, so second point is entirely skipped for you if you didn't intentionally enable `SteamTradeMatcher` in order to help yourself match dupes. A listagem pública é compatível somente com a última versão estável do ASF e pode se recusar a mostrar bots desatualizados, especialmente se faltar neles funcionalidades cruciais que só podem ser encontradas em novas versões.

### API

A listagem API do STM só aceita bots ASF no momento. Não há como inserir bots de terceiros em nossa listagem por enquanto (já que não podemos rever seus códigos facilmente e assegurar que eles satisfazem nossa lógica de trocas).

Se você está procurando por uma maneira fácil de acessar nossa lista de forma programática nós temos o endpoint **[/Api/Bots](https://asf.justarchi.net/Api/Bots)** muito simples que você pode usar. This is also the endpoint that ASF uses internally for `MatchActively` users.

### Aviso

*Todo o conceito, juntamente com a integração entre o site e o ASF ainda está em versão beta - ele pode ser melhorado/mudado com o passar do tempo - até mesmo removido se acharmos que não há muito interesse por essa funcionalidade.*

* * *

## Optando por não participar

Participar das estatísticas **não é obrigatório**, embora altamente incentivado para o futuro do programa. Nós não te julgamos, e se você tem uma necessidade muito grande de esconder o fato de que você está usando o ASF, então você pode desabilitar as estatísticas **totalmente** mudando o parâmetro de configuração global `Statistics` para `false`. Desabilitar as estatísticas torna todo o módulo não-operacional, e ele não fará nenhuma das ações especificadas em nossa política de privacidade acima.

Disabling statistics might influence **our technical support, ASF functionality and other things that are offered to you for free**. For example, you can't make use of `SteamTradeMatcher` or `MatchActively` without having `Statistics` enabled.