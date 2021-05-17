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

---

## Configuração avançada

A partir da versão 5.1.0.0 do ASF, nosso plugin suporta configuração avançada que pode ser útil para pessoas que gostariam de ajustar os internamente à sua preferência.

A configuração avançada tem a seguinte estrutura localizada dentro do `ASF.json`:

```json
{
  "SteamTokenDumperPlugin": {
    "Enabled": false,
    "SecretAppIDs": [],
    "SecretDepotIDs": [],
    "SecretPackageIDs": [],
    "SkipAutoGrantPackages": false
  }
}
```

Todas as opções são explicadas abaixo:

### `Enabled`

Tipo `bool` com valor padrão `false`. This property acts the same as `SteamTokenDumperPluginEnabled` root-level property explained above, and can be used instead, dedicated to people that would prefer to have entire plugin-related config in its own structure (so most likely those already using other advanced properties explained below).

---

### `SecretAppIDs`

Tipo `ImmutableHashSet<uint>` com valor padrão vazio. This property specifies `appIDs` that the plugin won't resolve, and if they're already resolved, won't submit the token for. This property can be useful for people with access to potentially-sensitive information about unpublished items, especially the developers, publishers or closed beta testers.

---

### `SecretDepotIDs`

Tipo `ImmutableHashSet<uint>` com valor padrão vazio. This property specifies `depotIDs` that the plugin won't resolve, and if they're already resolved, won't submit the key for. This property can be useful for people with access to potentially-sensitive information about unpublished items, especially the developers, publishers or closed beta testers.

---

### `SecretPackageIDs`

Tipo `ImmutableHashSet<uint>` com valor padrão vazio. This property specifies `packageIDs` (also known as `subIDs`) that the plugin won't resolve, and if they're already resolved, won't submit the token for. This property can be useful for people with access to potentially-sensitive information about unpublished items, especially the developers, publishers or closed beta testers.

---

### `SkipAutoGrantPackages`

Tipo `bool` com valor padrão `false`. This property acts very similar to `SecretPackageIDs` and when enabled, will cause packages with `EPaymentMethod` of `AutoGrant` to be skipped during resolve routine explained below. `AutoGrant` payment method is used by **[Steamworks](https://partner.steamgames.com)** to automatically grant packages on developer accounts. While this is not as explicit as other `Secret` options, and therefore doesn't guarantee anything (since you might have other packages than `AutoGrant` that you still don't want to submit), it should be good enough for skipping majority, if not all, of the secret packages.

---

## Further explanation

At the root level, every Steam account owns a set of packages (licenses, subscriptions), which are classified by their `packageID` (also known as `subID`). Every package may contain several apps classified by their `appID`. Every app may then include several depots classified by their `depotID`.

```text
├── sub/124923
│     ├── app/292030
│     │     ├── depot/292031
│     │     ├── depot/378648
│     │     └── ...
│     ├── app/378649
│     └── ...
└── ...
```

Our plugin includes two routines which take into account skipped items - the resolve routine and submission routine.

The resolve routine is responsible for resolving the tree you can see above. By blacklisting the packages/apps/depots in advance, you'll effectively cut the tree in the place of blacklisted branch/leaf without additional need of specifying the remaining parts of it. In our example above, if `124923` package was ignored, whether by `SecretPackageIDs` or `SkipAutoGrantPackages`, and it was the only package you own which linked to the `292030` appID, then appID `292030` wouldn't get resolved either, and by definition, if there were no other resolved apps which linked to the `292031` and `378648` depots, then they wouldn't get resolved either. However, keep in mind that if the plugin has already resolved the tree, then effectively this will only stop given item from being updated (e.g. new apps added), it will not make the plugin "forget" about the existing items that were already resolved (e.g. apps found in that package before you decided to blacklist it).

The submission routine is responsible for submitting package tokens, app tokens and depot keys of already resolved items (by the resolve routine above). Here your blacklist has immediate effect, as even if the plugin has already resolved the info, the submission routine will not actually submit it over to SteamDB if you have it blacklisted, regardless if it has been resolved or not. Keep in mind however that we're not talking about the tree anymore at this point, the submission routine does not know whether the information about the app comes from this or that package, so it only skips information about particular, blacklisted items, regardless of the relation they're in with other.

For majority of the developers and publishers, it should be enough to enable `SkipAutoGrantPackages`, potentially empowered with `SecretPackageIDs` only, as it effectively cuts the tree at the beginning branch and guarantees that the apps and depots included further will not get submitted as long as there is no other package linking to the same app. If you want to be double sure, in addition to that you can also use `SecretAppIDs`, which will skip the resolve of the app even if there are some other licenses you didn't blacklist linking to it. Using `SecretDepotIDs` should not be needed, unless you have a particular, specific need (such as skipping only a particular depot while still submitting info about packages and apps), or if you want to add yet another layer to be triple safe.