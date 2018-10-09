# Compartilhamento de Biblioteca Steam

O ASF suporta o Compartilhamento de Biblioteca Steam desde a versão 2.1.5.5+. Para entender como funciona o ASF nesse caso, primeiro você deve ler como **[Compartilhamento de Biblioteca Steam funciona](https://store.steampowered.com/promotion/familysharing)**, que está disponível na loja Steam.

* * *

## ASF

O suporte para o recurso de Compartilhamento de Biblioteca Steam no ASF é transparente, o que significa que ele não introduz quaisquer novas propriedades de configuração do bot ou do processo e funciona diretamente como uma funcionalidade extra incorporada.

O ASF inclui uma lógica apropriada para estar ciente caso a biblioteca seja bloqueada por um usuário do compartilhamento, portanto ele não vai "expulsar" esses jogadores da sessão quando abrirem um jogo. O ASF agirá como se a conta principal tivesse ativado o bloqueio, portanto independente desse bloqueio ser mantido pelo seu cliente Steam ou por outro membro do compartilhamento, o ASF não tentará coletar, em vez disso, ele vai esperar que o bloqueio seja liberado.

Além disso, após o login, o ASF acessará suas **configurações de compartilhamento</a>**, da qual ele vai extrair até 5 `steamIDs` que podem usar a sua biblioteca. Esses usuários recebem a permissão `FamilySharing` para usarem **[comandos](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-pt-BR)**, especialmente o comando `pause~` na conta do bot que está compartilhando os jogos com eles, o que permite pausar o módulo de coleta automática para poderem abrir um jogo compartilhado.</p> 

Conectar as duas funcionalidades descritas acima permite que seus amigos pausem o processo de coleta, iniciem um jogo, joguem o tempo que desejarem e, depois que eles acabarem, o processo de coleta será automaticamente retomado pelo ASF. É claro, usar o comando `pause~` não é necessário se o ASF não estiver rodando nenhum jogo, pois seus amigos podem abrir o jogo normalmente e a lógica descrita acima garante que eles não serão expulsos da seção.

* * *

## Limitações

A rede Steam adora enganar o ASF compartilhando atualizações de status falsas, que pode fazer o ASF acreditar que pode retomar o processo, expulsando seu amigo da seção de jogo. Para lutar contra esse problema, é aconselhável que você tenha seu amigo na sua lista de amigos Steam; desta forma o ASF, além dos eventos da rede Steam, pode verificar o status Steam do seu amigo e deduzir pelo status "em jogo" se ele realmente está jogando ainda ou não. Isso não é obrigatório, mas uma vez que o Steam às vezes comete falhas, é recomendado, principalmente se você percebe tais erros.