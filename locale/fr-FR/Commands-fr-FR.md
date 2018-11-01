# Commandes

ASF prend en charge diverses commandes pouvant être utilisées pour contrôler le comportement du processus et des instances de bot.

Les commandes ci-dessous peuvent être envoyées au bot de trois manières différentes:

- Par le chat privé steam
- Par le chat groupe steam
- Par l’intermédiaire de l' **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)**

N'oubliez pas que l'interaction ASF nécessite que vous soyez éligible pour la commande conformément aux autorisations ASF. Consultez les propriétés de configuration `SteamUserPermissions` et `SteamOwnerID` pour plus de détails.

All commands below are affected by `CommandPrefix` **[global configuration property](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#commandprefix)**, which is `!` by default. Cela signifie que pour exécuter par exemple `status`, vous devez écrire `!Status` (ou un ` préfixe de commande personnalisé` de votre choix que vous avez défini à la place).

* * *

### Chat privé steam

La méthode la plus simple pour interagir avec ASF - Il suffit d'exécuter la commande sur le bot ASF en cours d'exécution dans le processus ASF. Évidemment, vous ne pouvez pas faire cela si vous utilisez ASF avec un seul compte bot qui vous est propre.

![Capture d"écran](https://i.imgur.com/PPxx7qV.png)

* * *

### Chat groupe steam

Très similaire à ci-dessus, mais cette fois sur le chat d'un groupe Steam donné. N'oubliez pas que cette option nécessite de définir correctement la propriété `SteamMasterClanID`, auquel cas le bot écoutera les commandes également sur le chat du groupe (et le rejoindra si nécessaire). Cela peut également être utilisé pour "parler à vous-même" puisqu'il ne nécessite pas de compte bot dédié. Vous ne voudrez probablement pas utiliser cette méthode pour plus de bots que 1.

* * *

### IPC

La manière la plus avancée et la plus souple d’exécution de commandes, idéale pour les interactions utilisateur (ASF-ui) ainsi que pour les outils tiers ou les scripts (API ASF), requiert que ASF soit exécuté en mode ` IPC `. une commande d'exécution client via l'interface **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)**.

![Capture d"écran](https://i.imgur.com/pzKE4EJ.png)

* * *

## Commandes

| Commande                                                                   | Accès             | Description                                                                                                                                                                                                                                                                                                                                                     |
| -------------------------------------------------------------------------- | ----------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `2fa <Bots>`                                                         | `Maître`          | Génère un jeton temporaire **[2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** pour des instances de bot donnés.                                                                                                                                                                                                           |
| `2fano <Bots>`                                                       | `Maître`          | Refuse toutes les confirmations en attente **[2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** pour des instances de bot donnés.                                                                                                                                                                                           |
| `2faok <Bots>`                                                       | `Maître`          | Accepte toutes les confirmations en attente **[2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** pour des instances de bot donnés.                                                                                                                                                                                          |
| `addlicense <Bots> <GameIDs>`                                  | `Opérateur`       | Active les `appID` (réseaux Steam) ou les `subIDs` (Steam Store) donnés sur des instances de bot données (jeux gratuits uniquement).                                                                                                                                                                                                                            |
| `bl <Bots>`                                                          | `Maître`          | Liste les utilisateurs en liste noire du module de négociation d'instances de bot données.                                                                                                                                                                                                                                                                      |
| `bladd <Bots> <SteamIDs64>`                                    | `Maître`          | La liste noire de `steamIDs` à partir du module de négociation d'instances de bot données.                                                                                                                                                                                                                                                                      |
| `blrm <Bots> <SteamIDs64>`                                     | `Maître`          | Supprimer de la liste noire des` steamIDs ` donnés au module de négociation des instances de bot données.                                                                                                                                                                                                                                                       |
| `exit`                                                                     | `Propriétaire`    | Arrête tout le processus ASF.                                                                                                                                                                                                                                                                                                                                   |
| `farm <Bots>`                                                        | `Maître`          | Redémarre le module farm de cartes pour des instances de bot données.                                                                                                                                                                                                                                                                                           |
| `help`                                                                     | `PartageFamilial` | Affiche l'aide (lien vers cette page).                                                                                                                                                                                                                                                                                                                          |
| `input <Bots> <Type> <Value>`                            | `Maître`          | Définit le type d'entrée donné sur la valeur donnée pour des instances de bot données, fonctionne uniquement en mode `Headless` - pour plus d'explications **[ci-dessous](#input-command)**.                                                                                                                                                                    |
| `ib <Bots>`                                                          | `Maître`          | Répertorie les applications sur la liste noire à partir (automatic idling) d'instances de bot données.                                                                                                                                                                                                                                                          |
| `ibadd <Bots> <AppIDs>`                                        | `Maître`          | Ajoute `appID` donné aux applications de la liste noire à partir de (automatic idling) de certaines instances de bot.                                                                                                                                                                                                                                           |
| `ibrm <Bots> <AppIDs>`                                         | `Maître`          | Supprime les `appID` donnés des applications de la liste noire des applications inactives des instances de bot données.                                                                                                                                                                                                                                         |
| `iq <Bots>`                                                          | `Maître`          | Répertorie la file d'attente idling prioritaire d'instances de bot données.                                                                                                                                                                                                                                                                                     |
| `iqadd <Bots> <AppIDs>`                                        | `Maître`          | Ajoute `appIDs` donné à la priority idling d'instances de bot données.                                                                                                                                                                                                                                                                                          |
| `iqrm <Bots> <AppIDs>`                                         | `Maître`          | Supprime les `appIDs` donnés de la file d'attente priority idling des instances de bot données.                                                                                                                                                                                                                                                                 |
| `loot <Bots>`                                                        | `Maître`          | Envoie tous les ` LootableTypes` éléments de communauté Steam d'instances de bot données à l'utilisateur `Maître` défini dans leur ` SteamUserPermissions` (avec le plus bas steamID, s'il y en a plusieurs).                                                                                                                                                   |
| `loot@ <Bots> <RealAppIDs>`                                    | `Maître`          | Envoie tous les ` LootableTypes` éléments de la communauté Steam correspondant à ` RealAppIDs` des instances de bot données données à l'utilisateur ` Maître` défini dans leurs `SteamUserPermissions` (avec steamID le plus bas s'il y en a plus d'un).                                                                                                        |
| `loot^ <Bots> <AppID> <ContextID>`                       | `Maître`          | Envoie tous les éléments Steam de ` AppID` donné dans ` ContextID` des instances de bot données à l'utilisateur ` Maître` défini dans leur ` SteamUserPermissions` (avec steamID le plus bas s'il y en a plus d'un).                                                                                                                                            |
| `loot& <Bots>`                                                   | `Maître`          | Bascule le loot d'instances de bot données entre l'état activé / désactivé.                                                                                                                                                                                                                                                                                     |
| `nickname <Bots> <Nickname>`                                   | `Maître`          | Remplace le surnom Steam d’instances de bot données par `nickname`.                                                                                                                                                                                                                                                                                             |
| `owns <Bots> <AppIDsOrGameNames>`                              | `Opérateur`       | Vérifie si des instances de bot données possèdent déjà ` appIDs` et / ou ` gameNames` (peuvent faire partie du nom du jeu). Vous pouvez également indiquer `*` pour afficher tous les jeux disponibles.                                                                                                                                                         |
| `password <Bots>`                                                    | `Maître`          | Imprime le mot de passe crypté d'instances de bot données (utilisé avec `PasswordFormat`).                                                                                                                                                                                                                                                                      |
| `pause <Bots>`                                                       | `Opérateur`       | Met définitivement en pause le module de gestion automatique de cartes de certaines instances de bot. ASF ne tentera pas de mettre à jour le compte courant dans cette session, sauf si vous `resume` manuellement ou si vous redémarrez le processus.                                                                                                          |
| `pause~ <Bots>`                                                      | `PartageFamilial` | Suspend temporairement le module de gestion automatique de cartes de certaines instances de bot. Le farming sera automatiquement reprise au prochain événement en cours de lecture ou à la déconnexion du bot. Vous pouvez `reprendre</ 0> le farming pour le suspendre.</td>
</tr>
<tr>
  <td><code>pause& <Bots> <Seconds>` | `Opérateur` | Passe temporairement en pause le module de gestion automatique des cartes des instances de bot pour une de ` secondes </ 0> indiquée. Après un délai, le module de farming des cartes est automatiquement repris.</td>
</tr>
<tr>
  <td><code>play <Bots> <AppIDs,GameName>` | `Maître` | Bsculement vers le farming manuell - lancez `AppIDs` sur des instances de bot, éventuellement avec ` GameName </ 0> de façcon personnalisé. Utilisez <code>resume</ 0> pour revenir à un farming automatique.</td>
</tr>
<tr>
  <td><code>privacy <Bots> <Settings>` | `Maître` | Remplace **[Steam privacy settings](https://steamcommunity.com/my/edit/settings)** d’instances de bot données par des options correctement sélectionnées, expliquées **[ci-dessous](#privacy-settings)**. |
| `redeem <Bots> <Keys>`                                         | `Opérateur`       | Réclamer `clés-CD` dans des instances de bot données.                                                                                                                                                                                                                                                                                                           |
| `redeem^ <Bots> <Modes> <Keys>`                          | `Opérateur`       | Utilise `clés-CD` sur des instances de bot données, en utilisant les `modes` donnés **[ci-dessous](#redeem-modes)**.                                                                                                                                                                                                                                            |
| `rejoinchat <Bots>`                                                  | `Opérateur`       | Force les instances de bot spécifiées à rejoindre leur discussion de groupe `SteamMasterClanID`.                                                                                                                                                                                                                                                                |
| `restart`                                                                  | `Propriétaire`    | Redémarre le processus ASF.                                                                                                                                                                                                                                                                                                                                     |
| `resume <Bots>`                                                      | `PartageFamilial` | Reprend le farm automatique d'instances de bot données. Voir aussi ` pause`, ` play`.                                                                                                                                                                                                                                                                           |
| `start <Bots>`                                                       | `Maître`          | Démarre les instances de bot données.                                                                                                                                                                                                                                                                                                                           |
| `stats`                                                                    | `Propriétaire`    | Indique les statistiques de processus, telles que l'utilisation de la mémoire utilisée                                                                                                                                                                                                                                                                          |
| `status <Bots>`                                                      | `PartageFamilial` | Imprime le statut d'instances de bot données.                                                                                                                                                                                                                                                                                                                   |
| `stop <Bots>`                                                        | `Maître`          | Arrête les instances de bot données.                                                                                                                                                                                                                                                                                                                            |
| `transfer <Bots> <TargetBot>`                                  | `Maître`          | Envoie tous les `TransferableTypes` éléments de la communauté Steam d’instances de bot données à un bot cible.                                                                                                                                                                                                                                                  |
| `transfer@ <Bots> <RealAppIDs> <TargetBot>`              | `Maître`          | Envoie tous les `TransferableTypes` éléments de la communauté Steam correspondant à `RealAppID` donnés, à partir d'instances de bot données, au bot cible.                                                                                                                                                                                                      |
| `transfer^ <Bots> <AppID> <ContextID> <TargetBot>` | `Maître`          | Envoie tous les objets Steam de `AppID` donné dans `ContextID` d'occurrences de bot données au bot cible.                                                                                                                                                                                                                                                       |
| `unpack <Bots>`                                                      | `Maître`          | Déballé tous les boosters packs stockés dans l'inventaire d'instances de bot données.                                                                                                                                                                                                                                                                           |
| `update`                                                                   | `Propriétaire`    | Checks GitHub for ASF updates (this is done automatically every `UpdatePeriod`).                                                                                                                                                                                                                                                                                |
| `version`                                                                  | `PartageFamilial` | Imprimer la version d'ASF.                                                                                                                                                                                                                                                                                                                                      |

* * *

### Remarques

Toutes les commandes sont sensibles à la case, mais leurs arguments (tels que les noms de bot) sont généralement sensibles à la case.

L'argument `<Bots>` est facultatif dans toutes les commandes. Lorsque spécifié, la commande est exécutée sur des bots. Lorsqu'elle est omise, la commande est exécutée sur le bot actuel qui reçoit la commande. En d'autres termes, ` statut A ` envoyé au bot ` B ` correspond à l'envoi de ` statut ` au bot ` A `.

**Access** de la commande définit ** minimum** ` EPermission` sur `SteamUserPermissions` requis pour utiliser la commande, à l'exception de `Owner` qui est `SteamOwnerID` défini dans le fichier de configuration global (et la permission la plus élevée disponible).

Des arguments multiples, tels que `<Bots>`, `<Keys>` ou `<AppIDs>`, signifient que la commande prend en charge plusieurs arguments de type donné, séparés par une virgule. Par exemple, `status <Bots>` peut être utilisé comme `status MyBot, MyOtherBot, Primary`. Ainsi, la commande donnée sera exécutée sur ** tous les bots cibles </ 0> de la même manière que si vous envoyiez ` status </ 1> à chaque bot dans une fenêtre de discussion distincte. Veuillez noter qu'il n'y a pas d'espace après <code>, </ 0>.</p>

<p>ASF utilise tous les caractères de ponctuation comme délimiteurs possibles pour une commande, tels que les caractères de ponctuation et de nouvelle ligne. Cela signifie que vous ne devez pas utiliser d'espace pour délimiter vos arguments, vous pouvez également utiliser n'importe quel autre caractère de ponctuation (tel que tabulation ou nouvelle ligne).</p>

<p>ASF "joindra" des arguments supplémentaires hors de la plage au type pluriel du dernier argument de la gamme. Cela signifie que <code>redeem bot clé1 clé2 clé3` pour `échanger de <Bots> <Keys>` fonctionnera exactement de la même façon que `redeem bot clé1,clé2,clé3` . En même temps que l’acceptation de newline en tant que délimiteur de commande, cela vous permet d’écrire `redeem bot`, puis de coller une liste de clés séparées par tout caractère séparateur acceptable (tel que newline) ou standard `,` délimiteur ASF. Gardez à l'esprit que cette astuce ne peut être utilisée que pour la variante de commande qui utilise le plus grand nombre d'arguments (par conséquent, spécifier `<Bots>` est obligatoire dans ce cas).</p> 

Comme vous l'avez lu plus haut, un caractère d'espacement est utilisé comme délimiteur pour une commande. Par conséquent, il ne peut pas être utilisé dans les arguments. Toutefois, comme indiqué ci-dessus, ASF peut également joindre des arguments hors de portée, ce qui signifie que vous pouvez réellement utiliser un caractère d'espacement dans un argument défini comme dernier pour une commande donnée. Par exemple, `nickname bob Great Bob` définit correctement le surnom de ` bob` à "Great Bob". De la même manière, vous pouvez vérifier les noms contenant des espaces dans `owns`.

Notez que l'envoi d'une commande à la discussion de groupe agit comme un relais: si vous dites `redeem X` à 3 de vos robots assis avec vous sur la discussion de groupe, le résultat sera le même. comme vous le dites `redeem X` à chacun d’entre eux en privé. Dans la plupart des cas, ** ce n’est pas ce que vous souhaitez**. Vous devez plutôt utiliser` la commande de bot donnée` qui est envoyée à ** un seul bot dans une fenêtre privée**. . ASF prend en charge les discussions de groupe, car dans de nombreux cas, cela peut être une source de communication utile, mais vous ne devriez presque jamais exécuter d’aucune commande sur la discussion de groupe s’il ya au moins 2 robots ASF assis, sauf si vous comprenez parfaitement le comportement d’ASF écrit ici. et vous voulez en fait relayer la même commande à tous les robots qui vous écoutent.

*Et même dans ce cas, vous devriez plutôt utiliser le chat privé avec la syntaxe `<Bots>`.*

* * *

Certaines commandes sont également disponibles avec leurs alias, pour vous éviter de taper:

| Commande     | Alias |
| ------------ | ----- |
| `owns ASF`   | `oa`  |
| `status ASF` | `sa`  |
| `redeem`     | `r`   |
| `redeem^`    | `r^`  |

* * *

Il n’est pas nécessaire d’avoir un compte supplémentaire pour exécuter des commandes avec le chat Steam - vous pouvez créer un groupe, définir `SteamMasterClanID` correctement à ce groupe nouvellement créé, puis donnez-vous accès soit par `SteamOwnerID` ou ` SteamUserPermissions </ 0> de votre propre bot. De cette façon, ASF bot (vous) rejoindra le groupe et le chat de votre groupe sélectionné, et écoutera les commandes de votre propre compte. Vous pouvez rejoindre le même groupe de discussion afin de vous envoyer des commandes (car vous enverrez des commandes à la salle de discussion, et l'instance ASF siégeant dans la même salle de discussion les recevra, même si cela ne s'affiche que lorsque votre compte est présent). En dehors de cela, vous pouvez également utiliser <strong><a href="https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC"> IPC </a></strong>, mais le mode de discussion en ligne est beaucoup plus simple. Si vous avez accès à un compte alternatif, son utilisation est encore plus simple.</p>

<hr />

<h3><code><Bots>` argument</h3> 

`<Bots>` argument est une variante de l’argument du pluriel, car en plus d’accepter plusieurs valeurs, il offre également des fonctionnalités supplémentaires.

Tout d’abord, il y a le mot-clé `ASF` spécial qui sert pour « tous les bots dans le processus », donc la commande `status ASF` est égal à `État de tous vos bots répertoriés ici`. Cela peut également être utilisé pour identifier facilement les bots auxquels vous avez accès. En tant que mot clé ` ASF `, malgré le ciblage de tous les bots, seuls les bots auxquels vous pouvez envoyer la commande ne répondent.

`<Bots>` argument prend en charge une syntaxe spéciale "range", qui vous permet de choisir plus facilement une gamme de robots. La syntaxe générale pour `<Bots>` dans ce cas est `firstBot..lastBot`. Par exemple, si vous avez des robots nommés ` A, B, C, D, E, F `, vous pouvez exécuter ` le statut B..E`, ce qui correspond à `. statut B, C, D, E ` dans ce cas. En utilisant cette syntaxe, ASF utilisera un tri alphabétique afin de déterminer les bots qui se trouvent dans la plage spécifiée. `firstBot` et `lastBot` doivent être des noms de bot valides reconnus par ASF, sinon la syntaxe de la plage est entièrement ignorée.

En plus de la syntaxe de plage ci-dessus, l'argument `<Bots>` prend également en charge la correspondance ** regex </ 1>. Vous pouvez activer le motif de regex en utilisant `r!<pattern>` comme nom de bot, où `r!` est un activateur ASF pour la correspondance de regex et `<pattern>` votre motif de regex. Un exemple de commande de bot basé sur regex serait ` status r! \ D {3} ` qui enverra la commande ` status ` aux bots dont le nom est composé de 3 chiffres ( par exemple ` 123 ` et ` 981 `). N’hésitez pas à jeter un oeil à la **[documentation](https://docs.microsoft.com/en-us/dotnet/standard/base-types/regular-expression-language-quick-reference)** pour plus d’explications et plus d’exemples de modèles regex disponibles.</p> 

* * *

## ` Paramètres de confidentialité </ 0></h2>

<p><code><Settings>`argument accepté **jusqu'à 7**différentes options, séparées comme d'habitude avec le séparateur ASF standard par virgule. Ce sont, dans l'ordre:</p> 

| Argument | Nom            | Verrouillage |
| -------- | -------------- | ------------ |
| 1        | Profile        |              |
| 2        | OwnedGames     | Profile      |
| 3        | Playtime       | OwnedGames   |
| 4        | FriendsList    | Profile      |
| 5        | Inventory      | Profile      |
| 6        | InventoryGifts | Inventory    |
| 7        | Comments       | Profile      |

Pour obtenir une description des champs ci-dessus, veuillez consulter les **[ Paramètres de confidentialité de Steam ](https://steamcommunity.com/my/edit/settings)**.

Tandis que les valeurs valides pour tous sont:

| Valeur  | Nom             |
| ------- | --------------- |
| 1       | `Privé`         |
| 2       | `AmisSeulement` |
| 3       | `Publique`      |

Vous pouvez utiliser un nom ne tenant pas compte de la casse ou une valeur numérique. Les arguments qui ont été omis seront par défaut définis sur ` Privé </ 0>. Il est important de noter la relation entre l'enfant et le parent des arguments spécifiés ci-dessus, car l'enfant ne peut jamais avoir plus de permission ouverte que ses parent. Par exemple, vous <strong> ne pouvez pas </strong> posséder des jeux <code> publics ` avec un profil ` privé `.

### Exemple

Si vous souhaitez définir ** tous ** les paramètres de confidentialité de votre bot nommé ` Principal ` sur ` Privé `, vous pouvez utiliser l'un des éléments ci-dessous:

    privacy Main 1
    privacy Main Privat
    

En effet, ASF supposera automatiquement que tous les autres paramètres sont ` Privés `. Il n'est donc pas nécessaire de les saisir. Par contre, si vous souhaitez définir tous les paramètres de confidentialité sur ` Public `, vous devez utiliser l'un des éléments ci-dessous:

    privacy Main 3,3,3,3,3,3,3
    privacy Main Public,Public,Public,Public,Public,Public,Public
    

De cette façon, vous pouvez également définir des options indépendantes comme bon vous semble:

    privacy Main Public,FriendsOnly,Private,Public,Public,Private,Public
    

Ce qui précède définira le profil comme public, les jeux appartenant à seulement des amis, le temps de jeu privé, la liste d'amis public, l'inventaire public, les cadeaux d'inventaire privé et les commentaires de profil public. Vous pouvez obtenir la même chose avec des valeurs numériques si vous le souhaitez.

Rappelez-vous que l'enfant ne peut jamais avoir plus de permission ouverte que ses parent. Reportez-vous aux arguments pour les options disponibles.

* * *

## Modes `redeem^`

La commande `redeem^</ 0> vous permet d’affiner les modes qui seront utilisés pour un scénario d’échange unique. Cela fonctionne comme une substitution temporaire de <code> RedeemingPreferences </ 0> <strong><a href="https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#bot-config"> une propriété de configuration du bot </ 1>.</p>

<p>L'argument <code><Modes>` accepte plusieurs valeurs de mode, séparées comme d'habitude par une virgule. Les valeurs de mode disponibles sont spécifiées ci-dessous:

| Valeur  | Nom                   | Description                                                                             |
| ------- | --------------------- | --------------------------------------------------------------------------------------- |
| FD      | ForceDistributing     | Force l'activation de la distribution `Distributing`                                    |
| FF      | ForceForwarding       | Force l'activation du transfert `Forwarding`                                            |
| FKMG    | ForceKeepMissingGames | Force l'activation de `KeepMissingGames`                                                |
| SD      | SkipDistributing      | Force la désactivation de la préférence ` Distributing `                                |
| SF      | SkipForwarding        | Force la désactivation de la préférence ` Forwarding`                                   |
| SI      | SkipInitial           | Ignore l'utilisation des clés sur le bot initial                                        |
| SKMG    | SkipKeepMissingGames  | Force le blocage de la préférence `KeepMissingGames`                                    |
| V       | Validate              | Valide les clés pour le format approprié et ignore automatiquement les clés non valides |

Par exemple, nous aimerions échanger 3 clés sur n’importe quel de nos robots ne possédant pas encore de jeux, mais pas sur notre bot ` principal </ 0>. Pour y parvenir, nous pouvons utiliser:</p>

<p><code>redeem^ primary FF,SI key1,key2,key3`

* * *

## Commande `input`

La commande Input ne peut être utilisée qu'en mode ` Headless </ 0>, pour la saisie de données via <strong><a href="https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC"> IPC </ 1> ou le chat Steam lorsque ASF est en cours d'exécution sans prise en charge de l'interaction utilisateur.</p>

<p>La syntaxe générale est <code>input <Bots> <Type> <Value>`.

`<Type>` est insensible à la casse et définit le type d'entrée reconnu par ASF. Actuellement, ASF reconnaît les types suivants:

| Type                    | Description                                                                                |
| ----------------------- | ------------------------------------------------------------------------------------------ |
| DeviceID                | Identificateur de périphérique 2FA, s'il en manque `.maFile`.                              |
| Login                   | `SteamLogin` propriété de config bot, si absente de config.                                |
| Password                | `SteamPassword` propriété de config bot, si absente de config.                             |
| SteamGuard              | Code d'authentification envoyé sur votre courrier électronique si vous n'utilisez pas 2FA. |
| SteamParentalCode       | `SteamParentalCode` propriété de config bot, si absente de config.                         |
| TwoFactorAuthentication | Jeton 2FA généré à partir de votre mobile, si vous utilisez 2FA mais pas ASF 2FA.          |

`<Value>` est la valeur définie pour le type donné. Actuellement, toutes les valeurs sont des chaînes.

### Exemple

Disons que nous avons un bot protégé par SteamGuard en mode non-2FA. Nous voulons lancer ce bot avec `Headless` défini sur true.

Pour ce faire, nous devons exécuter les commandes suivantes:

` start MySteamGuardBot` -> Le bot tentera de se connecter, échouera car un code d'authentification est requis, puis s'arrêtera en raison d'une exécution en mode `headless`. Nous en avons besoin pour que le réseau Steam nous envoie le code d'autorisation sur notre adresse électronique. Si cela n'était pas nécessaire, nous ignorerions cette étape.

`input MySteamGuardBot SteamGuard ABCDE` -> Nous mettons `SteamGuard` entrée de `MySteamGuardBot` bot à `ABCDE`. Bien entendu, ` ABCDE </ 0> est dans ce cas le code d'autorisation que nous avons reçu par courrier électronique.</p>

<p><code>start MySteamGuardBot` -> Nous redémarrons notre bot (arrêté). Cette fois, il utilise automatiquement le code d'autorisation que nous avons défini dans la commande précédente, en se connectant correctement, puis en l'effaçant.

De la même manière, nous pouvons accéder à des bots protégés par 2FA (s'ils n'utilisent pas ASF 2FA), ainsi que définir d'autres propriétés requises lors de l'exécution.