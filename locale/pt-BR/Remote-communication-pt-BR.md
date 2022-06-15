# Comunicação remota

This section elaborates on remote communication that ASF includes, including further explanation on how one can influence it. While we don't consider anything below as malicious or otherwise unwanted, and neither we're legally obliged to disclose it, we want you to better understand the program functionality especially in regards to your privacy and data being shared.

## Steam

ASF communicates with Steam network (**[CM servers](https://api.steampowered.com/ISteamDirectory/GetCMList/v1?cellid=0)**), as well as **[Steam API](https://steamcommunity.com/dev)**, **[Steam store](https://store.steampowered.com)** and **[Steam community](https://steamcommunity.com)**.

It's not possible to disable any of the above communication, as it's the core foundation ASF is based on in order to provide its basic functionality. You'll need to refrain from using ASF if you're not comfortable with the above.

## Grupo Steam

ASF communicates with our **[Steam group](https://steamcommunity.com/groups/archiasf)**. The group provides you with announcements, especially new versions, critical issues, Steam problems and other things that are important to keep community updated. It also allows you to use our technical support, by asking questions, resolving problems, reporting issues or suggesting improvements. By default, accounts used in ASF will automatically join the group upon login.

You can decide to opt-out of joining the group by disabling `SteamGroup` flag in bot's **[`RemoteCommunication`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#remotecommunication)** settings.

## GitHub

ASF communicates with **[GitHub's API](https://api.github.com)** in order to fetch **[ASF releases](https://github.com/JustArchiNET/ArchiSteamFarm/releases)** for the update functionality. This is done as part of auto-updates (if you've kept **[`UpdatePeriod`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#updateperiod)** enabled), as well as `update` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. You can influence ASF's communication with GitHub through **[`UpdateChannel`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#updatechannel)** property - setting it to `None` will result in disabling entire update functionality, including GitHub communication in this regard.

## ASF's server

ASF communicates with **[our own server](https://asf.justarchi.net)** for more advanced functionality. In particular, this includes:
- Verifying checksums of ASF builds downloaded from GitHub against our own independent database to ensure that all downloaded builds are legitimate (free of malware, MITM attacks or other tampering)
- Anunciando seu bot em **[nossa listagem](https://asf.justarchi.net/STM)** se você tiver habilitado `SteamTradeMatcher` em **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#tradingpreferences)** e que atenda aos outros critérios
- Baixando bots disponíveis para troca da **[nossa listagem](https://asf.justarchi.net/STM)** se você tiver habilitado `MatchActively` em **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#tradingpreferences)** e que atenda aos outros critérios

As a security measure, it's not possible to disable checksum verification for ASF builds. However, you can disable auto-updates entirely if you'd like to avoid this, as described above in the GitHub section.

You can decide to opt-out of being announced in the listing by disabling `PublicListing` flag in bot's **[`RemoteCommunication`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#remotecommunication)** settings. This might be useful if you'd like to run `SteamTradeMatcher` bot without being announced at the same time.

Downloading bots from our listing is mandatory for `MatchActively` setting, you'll need to disable that setting if you're unwilling to accept that.

---

## Listagem pública do ASF STM

Nossa listagem pública do ASF STM está localizada em **[nosso site](https://asf.justarchi.net/STM)** e é usada como um serviço público tanto para os usuários do ASF que usam o `MatchActively`, bem como usuários e não-usuários do ASF para correspondência manual.

Note que você **não** vai ser exibido no site se você não cumprir todos os requisitos. ASF won't even bother communicating with our server in this case, so this section is entirely skipped for you if you didn't intentionally enable `SteamTradeMatcher` in order to help yourself match dupes. A listagem pública é compatível somente com a última versão estável do ASF e pode se recusar a mostrar bots desatualizados, especialmente se faltar neles funcionalidades cruciais que só podem ser encontradas em novas versões.

### Como exatamente isso funciona

O ASF envia os dados iniciais uma vez após o login, que contém todas as propriedades que a listagem pública utiliza. Em seguida, a cada 10 minutos o ASF envia uma solicitação muito pequena de "pulso" que notifica o nosso servidor de que o bot ainda está sendo executado. Se por algum motivo o pulso não chegar, por exemplo devido a problemas de rede, então o ASF vai tentar reenviá-lo a cada minuto, até o servidor registrá-lo.

Isso permite que o nosso site registre quais contas podem ser usadas para correspondência, bem como se ainda estão ativas. Graças a isso, nosso site pode mostrar todos as contas com ASF 2FA + STM que estavam ativas nos **últimos 15 minutos**.

Os usuários são classificados de acordo com seus inventários (em ordem decrescente) - bots `MatchEverything` configurados como `Any` que aceitam todas as trocas 1:1, depois pelo número de jogos exclusivos à partir dos quais são recebidos itens `MatchableTypes` e, por último, a contagem de itens `MatchableTypes`.

### API

A listagem API do STM só aceita bots ASF no momento. Não há como inserir bots de terceiros em nossa listagem por enquanto (já que não podemos rever seus códigos facilmente e assegurar que eles satisfazem nossa lógica de trocas).

Se você está procurando por uma maneira fácil de acessar nossa lista de forma programática nós temos o endpoint **[/Api/Bots](https://asf.justarchi.net/Api/Bots)** muito simples que você pode usar. Este é também o endpoint que o ASF usa internamente para os usuários com `MatchActively` habilitado.

### Privacy policy

If you agree to being listed in our listing, by enabling `SteamTradeMatcher` and not refusing `PublicListing`, as specified above, we'll temporarily store some of your Steam account details on our server in order to provide the core functionality.

Public info (exposed by Steam to every interested party) includes:
- Seu identificador Steam (na forma de 64-bit, para gerar ligações)
- Seu apelido (para fins de exibição)
- Seu avatar (hash, para fins de exibição)

Private info (selected data required for providing the functionality) includes:
- Seu **[token de trocas](https://steamcommunity.com/my/tradeoffers/privacy)** (para que pessoas fora da sua lista de amigos possam te enviar propostas de trocas)
- Seu `MaxTradeHoldDuration` (para que outras pessoas saibam se você está disposto a aceitar as trocas delas)
- Seu `MatchableTypes` (para fins de exibição e combinação)
- O número total de itens Steam correspondentes a `MatchableTypes` em seu inventário (para fins de exibição e combinação)
- O número total de jogos exclusivos dos quais os itens Steam acima correspondem aos tipos `MatchableTypes` foram obtidos (para fins de exibição e combinação)
- O valor de `MatchEverything` em **[`TradingPreferences`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#tradingpreferences)** (para fins de exibição e combinação)

ASF server will **not** collect, store or otherwise process any other data not listed above, without prior important notice in the changelog, and a very good practical reason in the first place. We do not consider anything above to be a serious matter, and we mention it to let you know what precisely ASF does apart of what you configured it to do yourself, so people can better understand the process.

Your data will be automatically hidden from general public in up to 15 minutes since the moment you stop using our listing, whether due to change of settings or not having ASF launched anymore. In addition to that, it'll be automatically deleted from our server (including all backup copies) in up to 7 days since the above happening.