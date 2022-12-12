# Note

This page is work-in-progress. For translators: you may want to wait a bit (until stable release), as the page is still being written and corrected.

---

# Plugin

`ItemsMatcherPlugin` is official ASF plugin that extends ASF with ASF STM listing features. In particular, this includes `PublicListing` in **[`RemoteCommunication`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#remotecommunication)** and `MatchActively` in **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#tradingpreferences)**.

---

## `PublicListing`

Nossa listagem pública do ASF STM está localizada em **[nosso site](https://asf-backend.justarchi.net/STM)** e é usada como um serviço público tanto para os usuários do ASF que usam o `MatchActively`, bem como usuários e não-usuários do ASF para correspondência manual.

Note que você **não** vai ser exibido no site se você não cumprir todos os requisitos. ASF won't even bother communicating with our server in this case, so this section is entirely skipped for you if you didn't intentionally enable `SteamTradeMatcher` in order to help yourself match dupes. A listagem pública é compatível somente com a última versão estável do ASF e pode se recusar a mostrar bots desatualizados, especialmente se faltar neles funcionalidades cruciais que só podem ser encontradas em novas versões.

### Como exatamente isso funciona

O ASF envia os dados iniciais uma vez após o login, que contém todas as propriedades que a listagem pública utiliza. Em seguida, a cada 10 minutos o ASF envia uma solicitação muito pequena de "pulso" que notifica o nosso servidor de que o bot ainda está sendo executado. Se por algum motivo o pulso não chegar, por exemplo devido a problemas de rede, então o ASF vai tentar reenviá-lo a cada minuto, até o servidor registrá-lo.

Isso permite que o nosso site registre quais contas podem ser usadas para correspondência, bem como se ainda estão ativas. Graças a isso, nosso site pode mostrar todos as contas com ASF 2FA + STM que estavam ativas nos **últimos 15 minutos**.

Os usuários são classificados de acordo com seus inventários (em ordem decrescente) - bots `MatchEverything` configurados como `Any` que aceitam todas as trocas 1:1, depois pelo número de jogos exclusivos à partir dos quais são recebidos itens `MatchableTypes` e, por último, a contagem de itens `MatchableTypes`.

### API

A listagem API do STM só aceita bots ASF no momento. Não há como inserir bots de terceiros em nossa listagem por enquanto (já que não podemos rever seus códigos facilmente e assegurar que eles satisfazem nossa lógica de trocas).

If you're looking for easy way to access our listing in programmatic way, we have a very simple **[`/Api/Listing/Bots`](https://asf-backend.justarchi.net/Api/Listing/Bots)** endpoint that you can use. Este é também o endpoint que o ASF usa internamente para os usuários com `MatchActively` habilitado.

### Privacy policy

If you agree to being listed in our listing, by enabling `SteamTradeMatcher` and not refusing `PublicListing`, as specified above, we'll temporarily store some of your Steam account details on our server in order to provide the core functionality.

Public info (exposed by Steam to every interested party) includes:
- Seu identificador Steam (na forma de 64-bit, para gerar ligações)
- Seu apelido (para fins de exibição)
- Seu avatar (hash, para fins de exibição)

Private info (selected data required for providing the functionality) includes:
- Your **[inventory](https://steamcommunity.com/my/inventory/#753_6)** limited to item types that you've picked in `MatchableTypes` (so people can use `MatchActively` against your items).
- Seu **[token de trocas](https://steamcommunity.com/my/tradeoffers/privacy)** (para que pessoas fora da sua lista de amigos possam te enviar propostas de trocas)
- Seu `MaxTradeHoldDuration` (para que outras pessoas saibam se você está disposto a aceitar as trocas delas)
- Seu `MatchableTypes` (para fins de exibição e combinação)
- Total number of Steam items in your inventory (for display purposes and matching)

---

## `MatchActively`

`MatchActively` setting is active version of **[`SteamTradeMatcher`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading#steamtradematcher)** which includes interactive matching in which the bot will send trades to other people. Isso pode funcionar em espera sozinho, ou junto com a configuração `SteamTradeMatcher`. This feature requires `LicenseID` to be set, as it uses third-party server.

Para usar essa opção há uma série de requisitos para serem atendidas. Você deve ter no minímo uma conta **[sem restrições](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663)**, **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication#asf-2fa)** ativo e pelo menos um tipo válido em `MatchableTypes`, tal como cartas colecionáveis.

Se você cumprir todos os requisitos acima, o ASF periodicamente irá se comunicar com a nossa **[Listagem STM pública do ASF](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Remote-communication#public-asf-stm-listing)** a fim de combinar bots que estão atualmente disponíveis.

- A cada rodada o ASF vai buscar nosso inventário e o inventário dos bots listados selecionados para encontrar itens `MatchableTypes` que possam ser combinados. Se for encontrada uma correspondência, o ASF vai enviar e confirmar a oferta de troca automaticamente.
- Cada conjunto (composto de appID, tipo de item e raridade do mesmo) pode ser combinado apenas uma vez em cada rodada. Isso foi implementado para minimizar o problema de "itens indisponíveis" e evitar a necessidade de esperar que cada bot reaja antes de enviar todas as trocas. É também a principal razão pela qual a correspondência é feita em rodadas e não por um processo constante.
- O ASF não enviará mais que `255` itens em uma única troca, e não mais que `5` trocas para um mesmo usuário em uma única rodada. Isso é imposto pelos limites do Steam, bem como por nosso próprio balanceamento.

Esse módulo deve ser transparente. As correspondências devem começar em aproximadamente `1` hora desde a ativação do ASF, e repetirá automaticamente a cada `6` horas (caso necessário). O recurso `MatchActively` foi feito para ser usado a longo prazo, verificando periodicamente para garantir que estamos na direção de completar o set, sem o curto espaço de tempo e a quantidade de pressão sobre os recursos necessários que ocorreria se isso fosse oferecido como um comando. Os usuários alvos desse módulo são principalmente contas principais e contas alternativas "ocultas", embora ele possa ser usado em qualquer bot que não foi configurado para `MatchEverything`.

O ASF faz o seu melhor para minimizar a quantidade de solicitações e a pressão gerada por usar esta opção, enquanto maximiza a eficiência das correspondências até o limite possível. O algoritmo exato de escolha dos bots para combinar e organizar todo o processo é um detalhe de implementação do ASF e pode mudar de acordo com os feedbacks, situações e possíveis ideias futuras.

A versão atual do algoritmo faz com que o ASF priorize os bots `Any`, especialmente aqueles que têm uma maior diversidade de jogos dos quais seus itens provêm. Quando os bots `Any` se esgotarem, o ASF vai passar para os próximos seguindo a mesma regra de diversidade, priorizando aqueles que possuem um número menor de itens devido a possibilidade de erros serem maior em inventários grandes. Independente disso, o ASF vai tentar combinar com cada bot disponível ao menos uma vez, garantindo que você não perca a possibilidade de fechar um set.

O `MatchActively` leva em conta os bots que você pôs na lista negra de trocas através do **[comando](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-pt-BR)** `tbadd` e não vai tentar trocas com eles. Pode ser usado para dizer ao ASF quais bots nunca devem ser combinados, mesmo se eles tiverem potenciais duplicatas que poderíamos usar.

---

### Why do I need a `LicenseID` to use the plugin? Wasn't `MatchActively` free before?

ASF is, and remains, free and open-source, as it was established at the start of the project back in October 2015. Our program is also entirely non-commercial, we do not earn anything from contributions to it, building or publishing. Over those past 7+ years ASF has received tremendous amount of development, and it's still being improved and enhanced with every monthly stable release mostly by a single person, **[JustArchi](https://github.com/JustArchi)** - with no strings attached. The only funding we receive is from non-obligatory donations that come from our users.

For a very long time, until October 2022, `MatchActively` feature was part of ASF core and available for everyone to use. In October 2022, Valve, the company behind Steam, has put very severe rate limits that rendered previous functionality entirely broken, with no solution available. The feature therefore has been removed from ASF core in version 5.4.1.0.

`MatchActively` was resurrected as part of official `ItemsMatcher` plugin that further enhances ASF with active cards matching functionality. Resurrecting `MatchActively` feature required from us extraordinary amount of work to create ASF backend, entirely new service hosted on a server, with more than a thousand of proxies attached for resolving inventories, all exclusively to allow ASF clients to make use of `MatchActively` like before. Due to the amount of work involved, as well as resources that are not free and require to be paid on monthly basis by us (domain, server, proxies), we've decided to offer this plugin to our sponsors, that is, people that already support ASF project on monthly basis. Our goal isn't to profit from it, but rather, cover the **monthly costs** that are exclusively linked with offering this functionality - that's why we offer it basically for nothing, but we do have to charge a little for it as we can't pay hundreds of dollars from our own pockets just to make it available for you. We hope that you understand.

---

### How can I get an access?

`ItemsMatcher` is offered as part of $5+ sponsor tier on **[JustArchi's GitHub](https://github.com/sponsors/JustArchi)**. Simply become a sponsor of $5 tier (or higher), then click **[here](https://asf-backend.justarchi.net/user/status)** to obtain your `LicenseID`. You'll need to sign in with GitHub for confirming your identity.

The license allows you to send limited amount of requests to the server. $5 tier allows you to use `MatchActively` for one account, which should be suitable for majority of people. $10 tier allows you to use it on three accounts. If you require more resources, **[let us know](mailto:ASF@JustArchi.net)**.

`LicenseID` is made out of 32 hexadecimal characters, such as `f6a0529813f74d119982eb4fe43a9a24`. Simply put it in `LicenseID` property of ASF global config.