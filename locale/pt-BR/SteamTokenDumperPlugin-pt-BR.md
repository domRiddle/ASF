# Plugin SteamTokenDumper

`SteamTokenDumperPlugin` é o **[plugin](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins)** oficial do ASF, desenvolvido por nós e disponível desde o ASF V.4.2.2.2, ele permite que você contríbua para o projeto **[SteamDB](https://steamdb.info)**, compartilhando pacotes de tokens, tokens de aplicativos e códigos de armazenamento aos quais sua conta Steam tem acesso. As informações na integra sobre o porque o SteamDB precisa de tais dados podem ser encontrado na página de **[Depósito de Tokens](https://steamdb.info/tokendumper)** da SteamDB. Os dados enviados não incluem qualquer informação potencialmente sensível, e não possui qualquer risco à sua segurança ou privacidade, como especificado acima.

---

## Ativando o Plugin

O ASF vem com p `SteamTokenDumperPlugin` incluso no pacote de lançamento, mas tal plugin vem desativado por padrão. Você pode ativar o plugin setando `SteamTokenDumperPluginEnabled` nas configurações globais do ASF para `true`, na syntax JSON:

```json
{
  "SteamTokenDumperPluginEnabled": true
}
```

Ao abrir o programa do ASF, o plugin lhe dirá se ele foi ativado com sucesso por meio do mecanismo padrão de registro do ASF. Você também pode ativar o plugin através do nosso gerador de configuração baseado na web.

---

## Detalhes técnicos

Ao habilitar, o plugin usará os bots que você está executando no ASF para coletar dados sob a forma de pacotes de tokens, tokens de aplicativos e códigos de armazenamento aos quais seus bots tem acesso. O módulo de coleta de dados inclui rotinas ativas e passivas que devem minimizar a sobrecarga adicional causada pela coleta de dados.

Para atender ao caso de uso planejado, além de coleta de dados explicados acima, rotina de submissão é inicializada como responsável por determinar quais dados devem ser enviados ao SteamDB periodicamente. Essa rotina disparará em até `1` horas após o início do ASF, e se repetirá a cada `24` horas. O plugin fará o seu melhor para minimizar a quantidade de dados que precisam ser enviados, portanto é possível que alguns dados que o plugin colete sejam marcados como inúteis, e, portanto, ignorados (por exemplo, atualização do aplicativo que não altera o token de acesso).

O plugin usa um banco de dados de cache persistente salvo no local `config/SteamTokenDumper.cache`, que serve uma finalidade similar ao `config/ASF.db` para o ASF. O arquivo é usado para gravar os dados reunidos e enviados e minimizar a quantidade de trabalho que precisa ser feito através de diferentes execuções do ASF. Remoção do arquivo faz com que o processo seja reiniciado do zero, o que deve ser evitado se possível.

---

## Dados

O ASF inclui o colaborador `steamID` na solicitação, o que é determinado como `SteamOwnerID` que você definiu no ASF, ou caso não tenha, o Steam ID do bot, que possui a maioria das licenças. O contribuidor anunciado pode receber algumas vantagens adicionais do SteamDB para ajuda contínua (e. . rank do doador no site), mas isso fica inteiramente a critério do SteamDB.

De qualquer forma, a equipe do SteamDB gostaria de agradecer antecipadamente pela sua ajuda. Os dados enviados permitem que o SteamDB opere, em particular para rastrear informações sobre pacotes, aplicativos e depósitos, o que não seria mais possível sem a sua ajuda.