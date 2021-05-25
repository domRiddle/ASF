# Estatísticas

O desenvolvimento do ASF é sustentado por 3 fatores mais importantes: doações, feedback dos usuários, e estatísticas. Doações influenciam diretamente a nossa vontade de trabalhar no projeto, os feedbacks dos usuários são sempre bons de ler (especialmente comentários positivos), e as estatísticas nos dão informações sobre como nosso software é usado e por quantas pessoas; assim nós podemos saber onde melhorar, o que consertar, e no que focar.

O ASF tem o parâmetro global `Statistics` ativado nas configurações por padrão. Se você quer ver novas versões sendo lançadas, bugs sendo consertados e novas funções sendo implementadas, você deve considerar deixar essa propriedade ativada, para que possamos usar esses dados para providenciar um melhor software para você (mas não apenas isso). Isto é especialmente importante porque o ASF é **gratuito**, e isso é o mínimo que você pode fazer para agradecer: **nos dizer que você está usando o ASF**, já que isso é o que nossas estatísticas estão fazendo. ASF não coleta ou faz uso de quaisquer dados que possam ser considerados privados e/ou usados contra você. Mantemos a utilização de estatísticas no mínimo, e cada informação recolhida exige ser afirmada aqui por nós em conjunto com explicações práticas. Isso te dá uma visão completa do que coletamos, para que propósito e como essa informação pode ajudar. Toda essa informação pode ser encontrada mais abaixo.

---

## Política de privacidade atual

Enquanto a propriedade `Statistics` está ativada, acontecerá o seguinte:

1. A contas em uso no ASF irá se juntar ao nosso **[grupo Steam](https://steamcommunity.com/gid/103582791440160998)**. Isso é feito por três motivos:

* Isso envia à **você** anúncios do grupo, especialmente versões novas, problemas críticos, problemas do Steam e outras coisas que são importantes para manter a comunidade atualizada (garantimos nenhum spam não relacionado ao ASF)
* Isso permite que **você** use nosso suporte técnico, fazendo perguntas, resolvendo problemas, relatando erros ou sugerindo melhorias
* Isso permite que nós vejamos quantas contas Steam estão sendo usadas pelo ASF

Consideramos o grupo Steam como uma parte crucial no que diz respeito a comunidade do ASF. Este é o nosso principal canal de comunicação, o qual utilizamos para todos os assuntos importantes no que se refere ao ASF, especialmente mantendo-o à par do desenvolvimento, de potenciais erros, avisos eventuais e todas as outras questões as quais você deve ter acesso como usuário. Não nos beneficiamos de manutenção desse grupo de forma alguma, é um lugar dedicado aos usuários do ASF e nós consideramos você parte da nossa comunidade. Uma vez que a adesão ao grupo não te identifica de forma alguma como usuário do ASF, não consideramos que este seja um problema em termos de privacidade.

2. If your account is **[unrestricted](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663)**, using **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication#asf-2fa)**, has **[public inventory](https://steamcommunity.com/my/edit/settings)** with at least 100 `MatchableTypes` items in it and you intentionally enabled `SteamTradeMatcher` in your `TradingPreferences`, then ASF will periodically communicate with our **[server](https://asf.justarchi.net)** in order to fulfill the enabled functionality. Os dados reais consistem no ID exclusivo do ASF (gerado por ele) e as seguintes informações relacionadas à conta:

* Seu identificador Steam (na forma de 64-bit, para gerar ligações, informação pública)
* Seu apelido (para fins de exibição, informação pública)
* Seu avatar (hash, para fins de exibição, informação pública)
* Your **[trading token](https://steamcommunity.com/my/tradeoffers/privacy)** (so people outside of your friendlist can send you trades)
* Seu `MatchableTypes` (para fins de exibição e combinação)
* O valor de `MatchEverything` em seu `TradingPreferences` (para fins de exibição e combinação)
* O número total de itens Steam correspondentes a `MatchableTypes` em seu inventário (para fins de exibição e combinação)
* O número total de jogos exclusivos dos quais os itens Steam acima correspondem aos tipos `MatchableTypes` foram obtidos (para fins de exibição e combinação)

ASF will **not** collect any other non-listed-above data without prior important notice in the changelog, and a very good practical reason in the first place. Não consideramos nada do que foi descrito acima como dados sensíveis, e mencionamos tudo para lhe informar o que o ASF faz exatamente, além do que você configurou para ser feito, para que as pessoas possam compreender melhor o nosso ponto de vista.

---

# Uso dos dados

All values specified in second point are being used for our **Public ASF STM listing**, which is explained below. Não usamos quaisquer outros dados para qualquer outro fim.

---

## Listagem pública do ASF STM

Our public ASF STM listing is located on **[our website](https://asf.justarchi.net/STM)** and used as a public service for both ASF users that make use of `MatchActively`, as well as ASF and non-ASF users for manual matching.

### How it exactly works

O ASF envia os dados iniciais uma vez após o login, que contém todas as propriedades que a listagem pública utiliza. Em seguida, a cada 10 minutos o ASF envia uma solicitação muito pequena de "pulso" que notifica o nosso servidor de que o bot ainda está sendo executado. Se por algum motivo o pulso não chegar, por exemplo devido a problemas de rede, então o ASF vai tentar reenviá-lo a cada minuto, até o servidor registrá-lo.

Isso permite que o nosso site registre quais contas podem ser usadas para correspondência, bem como se ainda estão ativas. Thanks to that, our website can show all ASF 2FA+STM accounts that were active in **last 15 minutes**.

Os usuários são classificados de acordo com seus inventários (em ordem decrescente) - bots `MatchEverything` configurados como `Any` que aceitam todas as trocas 1:1, depois pelo número de jogos exclusivos à partir dos quais são recebidos itens `MatchableTypes` e, por último, a contagem de itens `MatchableTypes`.

Please note that you will **not** be displayed on the website if you do not meet all of the requirements. O ASF nem se comunica com nossos servidores nesse caso, então o segundo ponto será inteiramente ignorado caso você não habilite o `SteamTradeMatcher` para te ajudar a trocar cartas duplicadas. A listagem pública é compatível somente com a última versão estável do ASF e pode se recusar a mostrar bots desatualizados, especialmente se faltar neles funcionalidades cruciais que só podem ser encontradas em novas versões.

### API

A listagem API do STM só aceita bots ASF no momento. Não há como inserir bots de terceiros em nossa listagem por enquanto (já que não podemos rever seus códigos facilmente e assegurar que eles satisfazem nossa lógica de trocas).

Se você está procurando por uma maneira fácil de acessar nossa lista de forma programática nós temos o endpoint **[/Api/Bots](https://asf.justarchi.net/Api/Bots)** muito simples que você pode usar. Este é também o endpoint que o ASF usa internamente para os usuários com `MatchActively` habilitado.

### Aviso

*Todo o conceito, juntamente com a integração entre o site e o ASF ainda está em versão beta - ele pode ser melhorado/mudado com o passar do tempo - até mesmo removido se acharmos que não há muito interesse por essa funcionalidade.*

---

## Optando por não participar

Participar das estatísticas **não é obrigatório**, embora altamente incentivado para o futuro do programa. Nós não te julgamos, e se você tem uma necessidade muito grande de esconder o fato de que você está usando o ASF, então você pode desabilitar as estatísticas **totalmente** mudando o parâmetro de configuração global `Statistics` para `false`. Desabilitar as estatísticas torna todo o módulo não-operacional, e ele não fará nenhuma das ações especificadas em nossa política de privacidade acima.

Desabilitar as estatísticas pode influenciar **nosso suporte técnico, funcionalidades do ASF e outras coisas que são oferecidas para você gratuitamente**. Por exemplo, você não pode fazer uso de `MatchActively` sem ter ativado `Statistics` (pois você se recusa a comunicar com o nosso servidor para que a funcionalidade funcione).