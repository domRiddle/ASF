# Échanges

ASF inclut la prise en charge des  échanges non interactifs (hors ligne) de Steam. La réception (acceptation / refus) ainsi que l’envoi de transactions sont disponibles immédiatement et ne nécessitent pas de configuration particulière, mais nécessitent évidemment un compte Steam sans restriction (Dépensé 5 $ dans le magasin). Le système d'échange n'est pas disponible pour les comptes restreints.

---

## Logique

ASF acceptera toujours tous les échanges, quels que soient les éléments, envoyés par les utilisateurs ayant un accès ` Master</ 0> (ou supérieur) au bot. Cela permet non seulement de récupérer facilement les cartes Steam collectées par le bot, mais également de gérer facilement les objets Steam stockés dans l'inventaire, y compris ceux provenant d'autre jeux (comme CS:GO).</p>

<p spaces-before="0">ASF rejettera l'offre d'échange, quel que soit le contenu, de tout utilisateur (non master) inscrit sur la liste noire à partir du système d'échange. La liste noire est stockée dans la base de données standard <code>BotName.db` et peut être gérée via les **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** : `tb`, `tbadd` and `tbrm`. Cela devrait servir comme une alternative au blocage utilisateur standard offert par Steam - à utiliser avec prudence.

ASF acceptera toutes les transactions de type `loot` envoyées par des bots sauf si `DontAcceptBotTrades` est spécifié dans `TradingPreferences`. En bref, `TradingPreferences` par défaut de `None` obligera ASF à accepter automatiquement les transactions de l'utilisateur disposant d'un accès `Master` au bot (expliqué ci-dessus). comme tous les dons se négocient à partir de d'autres bots qui participent au processus ASF. Si vous souhaitez désactiver les échanges de dons provenant d’autres bots, c’est à cela que sert `DontAcceptBotTrades` dans vos `TradingPreferences`.

Lorsque vous activez `AcceptDonations` dans vos `TradingPreferences`, ASF acceptera également toute donations, le compte bot ne perd aucun éléments. Cette propriété affecte uniquement les comptes sans bot, car les comptes bot sont affectés par `DontAcceptBotTrades`. `AcceptDonations` vous permet d’accepter facilement les dons d’autres personnes, ainsi que des bots ne participant pas au processus ASF.

Il est appréciable de noter que `AcceptDonations` ne nécessite pas de **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)**, car aucune confirmation n'est nécessaire si nous ne perdons aucun élément.

Vous pouvez également personnaliser davantage les capacités d'échange ASF en modifiant `TradingPreferences`. L’une des principales caractéristiques de `TradingPreferences` est `SteamTradeMatcher`, qui obligera ASF à utiliser la logique intégrée pour accepter les transactions qui vous aident à compléter les badges non complet, ce qui est particulièrement utile en coopération avec la liste publique de **[SteamTradeMatcher](https://www.steamtradematcher.com)**, mais peut également fonctionner sans ce dernier. Il est décrit plus en détail ci-dessous.

---

## `SteamTradeMatcher`

Lorsque `SteamTradeMatcher` est actif, ASF utilisera un algorithme assez complexe pour vérifier si le commerce respecte les règles du STM et est au moins neutre envers nous. La logique actuelle est la suivante:

- La transaction est rejetée si nous perdons autre chose que les types d’articles spécifiés dans notre `MatchableTypes`.
- La transaction est rejetée si nous ne recevons pas au moins le même nombre d’objets par jeu et par type.
- La transaction est rejetée si l'utilisateur demande des cartes spéciales Soldes été / hiver de Steam et si cette transaction est suspendue.
- La transaction est rejetée si la durée de la suspension dépasse la propriété de configuration globale `MaxTradeHoldDuration`.
- La transaction est rejetée si nous n'avons pas `MatchEverything`, et c'est moins bien que neutre pour nous.
- La transaction est acceptée si nous ne la rejetons pas à cause de l’un des points ci-dessus.

Il est intéressant de noter qu'ASF prend également en charge les supplément ; la logique fonctionnera correctement lorsque l'utilisateur ajoutera quelque chose de plus au commerce, à condition que toutes les conditions ci-dessus soient remplies.

Les 4 premiers motifs de rejet devraient être évidents pour tout le monde. Le dernier comprend la logique des  doubles qui vérifie l’état actuel de nos stocks et décide de l’état du commerce.

- Le commerce est ** bon </ 0> si nos progrès vers l’achèvement fixé progressent. Exemple: A A (avant) -> A B (après)</li>
- Le commerce est ** bon </ 0> si nos progrès vers l’achèvement fixé progressent. Exemple: A B (avant) -> A C (après)</li>
- La transaction est **mauvais** si la progression vers l'objectif fixé diminue. Exemple: A C (avant) -> A A (après)</ul>

STM ne fonctionne que pour des offres correctes, ce qui signifie que l'utilisateur qui utilise STM pour les doubles ne devrait que suggérer des offres correctes pour nous. Cependant, ASF est indulgent et accepte également les transactions neutres, car dans ces transactions, nous ne perdons rien, il n’y a donc aucune raison de les refuser. Ceci est particulièrement utile pour vos amis, car ils peuvent échanger vos cartes en trop sans utiliser la technologie STM, tant que vous ne perdez pas la progression définie.

Par défaut, ASF rejettera les mauvaises transactions - c’est généralement ce que vous recherchez en tant qu’utilisateur. Cependant, vous pouvez éventuellement activer `MatchEverything` dans vos `TradingPreferences` afin de permettre à ASF d'accepter tous les échanges doubles, y compris **les mauvais**. Cela n’est utile que si vous souhaitez exécuter un bot commercial 1: 1 sous votre compte, car vous comprenez que ** ASF ne vous aidera plus à progresser vers la réalisation de vos badges et cela rend susceptible la possibilité de perdre toute votre set fini pour un certains nombres de doubles de la même carte </ 0>. À moins que vous ne vouliez délibérément exécuter un bot commercial qui est ** jamais </ 0> censé terminer un ensemble, vous ne souhaitez pas activer cette option.</p>

Quels que soient les `TradingPreferences` que vous avez choisies , une transaction rejetée par ASF ne signifie pas que vous ne pouvez l’accepter vous-même. Si vous avez conservé la valeur par défaut de `BotBehaviour`, qui est `None`, ASF ignorera simplement ces transactions, vous permettant ainsi de décider vous-même si elles vous intéressent ou non. Il en va de même pour les transactions avec des éléments en dehors de `MatchableTypes`, ainsi que pour tout le reste ; le module est censé vous aider à automatiser les transactions STM, sans décider de ce qui est une bonne transaction ou non. La seule exception à cette règle concerne les utilisateurs que vous avez inscrits sur la liste noire du module d'échange à l'aide de la commande `tbadd`. Les transactions de ces utilisateurs sont immédiatement rejetées, quels que soient les paramètres `BotBehaviour`.

Il est vivement recommandé d'utiliser **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** lorsque vous activez cette option, car cette fonction perd tout son potentiel si vous décidez de confirmer manuellement chaque transaction. `SteamTradeMatcher` fonctionnera correctement même si vous ne pouvez pas confirmer les transactions, mais cela pourrait générer un retard de confirmations si vous ne les acceptez pas à temps.

---

### `MatchActively`

`MatchActively` setting is active version of `SteamTradeMatcher` which includes interactive matching in which the bot will send trades to other people. It can work standalone, or together with `SteamTradeMatcher` setting.

In order to make use of that option, you have a set of requirements to meet. You have to be eligible for our **[ASF STM listing](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Remote-communication#public-asf-stm-listing)**, with a bit relaxed requirements. At the minimum you must have **[unrestricted](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663)** account, **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication#asf-2fa)** active and at least one valid type in `MatchableTypes`, such as trading cards.

If you meet all of the requirements above, ASF will periodically communicate with our **[public ASF STM listing](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Remote-communication#public-asf-stm-listing)** in order to actively match bots that are currently available.

- Each matching session is composed of "rounds", with `10` being maximum in a single matching session.
- In each round ASF will fetch our inventory and inventory of selected bots that are listed in order to find `MatchableTypes` items that can be matched. If match is found, ASF will send and confirm trade offer automatically.
- Each set (composition of appID, type and rarity of the item) can be matched in a single round only once. This is implemented in order to minimize "items no longer available" and avoid a need to wait for each bot to react before sending all the trades. It's also the primary reason why matching is composed of rounds and not one ongoing process.
- ASF will send no more than `255` items in a single trade, and no more than `5` trades to a single user in a single round. This is imposed by Steam limits, as well as our own load-balancing.
- ASF has a hard limit of `40` unique bots that can be matched in a single round, if not cancelled before due to running out of sets to match - in this case, during the next round ASF will try to match bots that weren't matched yet firstly.
- If ASF determines that the matching should continue, next round starts within `5` minutes since the last one (to add some cooldown and allow all bots to react to our trades), otherwise matching session ends and repeats itself in `8` hours.

This module is supposed to be transparent. Matching will start in approximately `1` hour since ASF start, and will repeat itself each `8` hours (if needed). `MatchActively` feature is aimed to be used as a long-run, periodical measure to ensure that we're actively heading towards sets completion, but without a short-term time and resources pressure that would happen if this was offered as a command. The target users of this module are primary accounts and "stash" alt accounts, although it can be used by any bot that is not set to `MatchEverything`.

ASF does its best to minimize the amount of requests and pressure generated by using this option, while at the same time maximizing efficiency of matching to the upper limit. The exact algorithm of choosing the bots to match and otherwise organize the whole process, is ASF's implementation detail and can change in regards to feedback, situation and possible future ideas.

The current version of the algorithm makes ASF prioritize `Any` bots first, especially those with better diversity of games that their items are from. When running out of `Any` bots, ASF will move on to the fair ones upon same diversity rule, with those owning excessive number of items further deprioritized due to higher chance of possible inventory-related problems compared to other bots. Regardless of that, ASF will try to match every available bot at least once, to ensure that we're not missing on a possible set progress.

`MatchActively` takes into account bots that you blacklisted from trading through `tbadd` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** and will not attempt to actively match them. This can be used for telling ASF which bots it should never match, even if they'd have potential dupes for us to use.