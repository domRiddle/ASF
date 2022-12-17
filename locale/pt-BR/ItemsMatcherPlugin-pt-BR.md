# ItemsMatcherPlugin

`ItemsMatcherPlugin` is official ASF **[plugin](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins)** that extends ASF with ASF STM listing features. In particular, this includes `PublicListing` in **[`RemoteCommunication`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#remotecommunication)** and `MatchActively` in **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#tradingpreferences)**. ASF comes with `ItemsMatcherPlugin` bundled together with the release, therefore it's ready for usage right away.

---

## `PublicListing`

Public listing, as the name implies, is listing of currently available ASF STM bots. It's located on **[our website](https://asf.justarchi.net/STM)**, managed automatically and used as a public service for both ASF users that make use of `MatchActively`, as well as ASF and non-ASF users for manual matching.

While `PublicListing` is enabled by default, please note that you will **not** be displayed on the website if you do not meet all of the requirements, especially `SteamTradeMatcher`, which isn't enabled by default. For people that do not meet the criteria, even if they kept `PublicListing` enabled, ASF doesn't communicate with the server in any way. Public listing is also compatible only with latest stable version of ASF and may refuse to display outdated bots, especially if they're missing core functionality that can be found only in newer versions.

### Como exatamente isso funciona

O ASF envia os dados iniciais uma vez após o login, que contém todas as propriedades que a listagem pública utiliza. Em seguida, a cada 10 minutos o ASF envia uma solicitação muito pequena de "pulso" que notifica o nosso servidor de que o bot ainda está sendo executado. Se por algum motivo o pulso não chegar, por exemplo devido a problemas de rede, então o ASF vai tentar reenviá-lo a cada minuto, até o servidor registrá-lo. This way our server knows precisely which bots are still running and ready to accept trade offers. ASF will also send initial announcement on as-needed basis, for example if it detects that our inventory has changed since the previous one.

We display all ASF 2FA+STM accounts that were active in the **last 15 minutes**. Users are sorted according to their relative usefulness - `MatchEverything` bots which are shown with `Any` banner that accept all 1:1 trades, then unique games count, and finally items count.

### API

A listagem API do STM só aceita bots ASF no momento. There is no way to list third-party bots on our listing for now, as we can't review their code easily and ensure they meet our entire trading logic. Participation in the listing therefore requires latest stable ASF version, although it can run with custom **[plugins](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins)**.

For consumers of the listing, we have a very simple **[`/Api/Listing/Bots`](https://asf.justarchi.net/Api/Listing/Bots)** endpoint that you can use. It includes all the data we have, apart from inventories of users which are part of `MatchActively` feature exclusively.

### Privacy policy

If you agree to being listed in our listing, by enabling `SteamTradeMatcher` and not refusing `PublicListing`, as specified above, we'll temporarily store some of your Steam account details on our server in order to provide the expected functionality.

Public info (exposed by Steam to every interested party) includes:
- Seu identificador Steam (na forma de 64-bit, para gerar ligações)
- Seu apelido (para fins de exibição)
- Seu avatar (hash, para fins de exibição)

Semi-public info (exposed by Steam to every interested party if you meet listing requirements) includes:
- Your **[inventory](https://steamcommunity.com/my/inventory/#753_6)** (so people can use `MatchActively` against your items).

Private info (selected data required for providing the functionality) includes:
- Seu **[token de trocas](https://steamcommunity.com/my/tradeoffers/privacy)** (para que pessoas fora da sua lista de amigos possam te enviar propostas de trocas)
- Your `MatchableTypes` setting (for display purposes and matching)
- Your `MatchEverything` setting (for display purposes and matching)
- Your `MaxTradeHoldDuration` setting (so other people know whether you're willing to accept their trades)

---

## `MatchActively`

`MatchActively` setting is active version of **[`SteamTradeMatcher`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Trading#steamtradematcher)** including interactive matching in which the bot will send trades to other people. Isso pode funcionar em espera sozinho, ou junto com a configuração `SteamTradeMatcher`. This feature requires `LicenseID` to be set, as it uses third-party server and paid resources to operate.

Para usar essa opção há uma série de requisitos para serem atendidas. Você deve ter no minímo uma conta **[sem restrições](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663)**, **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication#asf-2fa)** ativo e pelo menos um tipo válido em `MatchableTypes`, tal como cartas colecionáveis.

If you meet all of the requirements above, ASF will periodically communicate with our **[public ASF STM listing](#publiclisting)** in order to actively match bots that are currently available.

- In each round ASF will fetch our inventory and inventory of all available bots listed in order to find `MatchableTypes` items that can be matched. Thanks to communicating directly with our server, this process requires a single request and we have immediate information whether any available bot offers something interesting for us - if match is found, ASF will send and confirm trade offer automatically.
- Cada conjunto (composto de appID, tipo de item e raridade do mesmo) pode ser combinado apenas uma vez em cada rodada. Isso foi implementado para minimizar o problema de "itens indisponíveis" e evitar a necessidade de esperar que cada bot reaja antes de enviar todas as trocas. É também a principal razão pela qual a correspondência é feita em rodadas e não por um processo constante.
- O ASF não enviará mais que `255` itens em uma única troca, e não mais que `5` trocas para um mesmo usuário em uma única rodada. Isso é imposto pelos limites do Steam, bem como por nosso próprio balanceamento.

Esse módulo deve ser transparente. As correspondências devem começar em aproximadamente `1` hora desde a ativação do ASF, e repetirá automaticamente a cada `6` horas (caso necessário). O recurso `MatchActively` foi feito para ser usado a longo prazo, verificando periodicamente para garantir que estamos na direção de completar o set, sem o curto espaço de tempo e a quantidade de pressão sobre os recursos necessários que ocorreria se isso fosse oferecido como um comando. Os usuários alvos desse módulo são principalmente contas principais e contas alternativas "ocultas", embora ele possa ser usado em qualquer bot que não foi configurado para `MatchEverything`.

O ASF faz o seu melhor para minimizar a quantidade de solicitações e a pressão gerada por usar esta opção, enquanto maximiza a eficiência das correspondências até o limite possível. O algoritmo exato de escolha dos bots para combinar e organizar todo o processo é um detalhe de implementação do ASF e pode mudar de acordo com os feedbacks, situações e possíveis ideias futuras.

A versão atual do algoritmo faz com que o ASF priorize os bots `Any`, especialmente aqueles que têm uma maior diversidade de jogos dos quais seus itens provêm. When running out of `Any` bots, ASF will move on to the `Fair` ones upon same diversity rule. ASF will try to match every available bot at least once, to ensure that we're not missing on a possible set progress.

O `MatchActively` leva em conta os bots que você pôs na lista negra de trocas através do **[comando](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-pt-BR)** `tbadd` e não vai tentar trocas com eles. Pode ser usado para dizer ao ASF quais bots nunca devem ser combinados, mesmo se eles tiverem potenciais duplicatas que poderíamos usar.

---

### Why do I need a `LicenseID` to use `MatchActively`? Wasn't it free before?

ASF is, and remains, free and open-source, as it was established at the start of the project back in October 2015. Our program is also entirely non-commercial, we do not earn anything from contributions to it, building or publishing. Over those past 7+ years ASF has received tremendous amount of development, and it's still being improved and enhanced with every monthly stable release mostly by a single person, **[JustArchi](https://github.com/JustArchi)** - with no strings attached. The only funding we receive is from non-obligatory donations that come from our users.

For a very long time, until October 2022, `MatchActively` feature was part of ASF core and available for everyone to use. In October 2022, Valve, the company behind Steam, has put very severe rate limits that rendered previous functionality entirely broken, with no solution available. The feature therefore had to be removed from ASF core in version 5.4.1.0.

`MatchActively` was resurrected as part of official `ItemsMatcher` plugin that further enhances ASF with active cards matching functionality. Resurrecting `MatchActively` feature required from us **extraordinary amount of work** to create ASF backend, entirely new service hosted on a server, with more than a thousand of proxies attached for resolving inventories, all exclusively to allow ASF clients to make use of `MatchActively` like before. Due to the amount of work involved, as well as resources that are not free and require to be paid on monthly basis by us (domain, server, proxies), we've decided to offer this functionality to our sponsors, that is, people that already support ASF project on monthly basis. Our goal isn't to profit from it, but rather, cover the **monthly costs** that are exclusively linked with offering this option - that's why we offer it basically for nothing, but we do have to charge a little for it as we can't pay hundreds of dollars from our own pockets just to make it available for you. We hope that you understand.

---

### How can I get an access?

`ItemsMatcher` is offered as part of $5+ sponsor tier on **[JustArchi's GitHub](https://github.com/sponsors/JustArchi)**. Simply become a sponsor of $5 tier (or higher), then read **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#licenseid)** section to obtain and fill `LicenseID`.

The license allows you to send limited amount of requests to the server. $5 tier allows you to use `MatchActively` for one account, which should be suitable for majority of people. $10 tier allows you to use it on three accounts. If you require more resources, **[let us know](mailto:ASF@JustArchi.net)**.