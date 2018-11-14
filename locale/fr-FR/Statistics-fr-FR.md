# Statistiques

Le développement d’ASF est pris en charge par 3 choses principales : dons, avis des utilisateurs et statistiques. Les dons influencent directement nos possibilités pour travailler sur le projet, l'avis des utilisateurs est toujours agréable à lire (surtout positive), alors que les statistiques nous fournissent des informations d’utilisation de notre logiciel ainsi que le nombre de personnes - de cette façon nous pouvons savoir ce qu’il faut améliorer, résoudre et voir sur ce qu’il faut se concentrer.

ASF, par défaut, dans les paramètres de configuration global a d'activer `Statistics` . Si vous voulez voir de nouvelles versions à venir, avec bugs corrigés et des nouveautés cette mise en place est préférable d'être garder, afin que nous fassions usage de ces données pour vous fournir un meilleur logiciel (mais pas seulement). Ceci est particulièrement important parce que ASF est proposé **gratuitement**, et c’est le moins que vous pouvez faire pour dire Merci - **dites-nous que vous utilisez ASF**, car c’est ce que font principalement nos statistiques actuelles.

Nous gardons l’utilisation des statistiques au strict minimum, et chaque information qui est recueillie nécessite explication concrète - ce que nous collectons, pour quel but, et comment il est censé pour aider. Tout cela se trouvent ci-dessous.

* * *

# Politique de confidentialité actuelle

Lorsque les `statistiques` sont actives, les choses suivantes arriveront :

a) tout compte utilisé dans ASF rejoindra notre **[groupe steam](https://steamcommunity.com/gid/103582791440160998)**. Pour trois raisons :

* Il **vous** fournit des annonces, surtout concernant les nouvelles versions, les questions cruciales, problèmes Steam et autres choses qui sont importantes afin de maintenir la communauté à jour (pas de spam avec ASF garanti)
* Il **vous**permet d’utiliser notre support technique, en posant des questions, résoudre les problèmes, ou suggérer des améliorations
* Il nous permet de voir combien de comptes Steam réels est utilisés par ASF

b) Si votre compte est **[sans restriction](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663)**, que vous utilisez **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication#asf-2fa)** , vous avez un **[inventaire public](https://steamcommunity.com/my/edit/settings)** contenant au moins 100 `MatchableTypes` et inclut ` SteamTradeMatcher` dans `TradingPreferences`, alors ASF communiquera périodiquement avec notre **[serveur](https://asf.justarchi.net)**. Les données réelles se compose de l’ID unique de ASF (généré par ASF) et des informations suivantes concernant le compte :

* Votre identificateur Steam (sous forme 64 bits, pour générer des liens)
* Votre pseudo (pour l'affichage)
* Votre avatar (hash, pour l'affichage)
* Votre ** jeton de d'échange </ 0> (afin que des personnes ne faisant pas partie de votre liste d'amis puissent vous envoyer des échanges)</li> 
    
    * Vos ` MatchableTypes </ 0> (à des fins d'affichage)</li>
<li>Valeur de <code> MatchEverything </ 0> dans vos <code> TradingPreferences </ 0> (à des fins d'affichage et de tri)</li>
<li>Nombre total d'articles <code> MatchableTypes </ 0> Steam dans votre inventaire (à des fins d'affichage et de tri)</li>
<li>Nombre total de jeux uniques composés de plus de <code> MatchableTypes </ 0> Steam (à des fins d'affichage et de tri)</li>
</ul>

<p>ASF <strong> ne </ 0> collectera aucune autre donnée non répertoriée ci-dessus sans préavis important dans le journal des modifications, ce qui constitue une très bonne raison en premier lieu Nous ne considérons rien de ce qui précède comme suffisamment sérieux, car le groupe Steam n’identifie en rien le programme d’utilisation du programme ASF, alors que notre liste publique n’est activée que si vous activez intentionnellement la fonctionnalité <code> SteamTradeMatcher </ 0>. Si vous <strong> voulez réellement </ 1> que les gens sachent cela et vous envoient des offres commerciales en premier lieu.</p>

<hr />

<h1>Données d'utilisation</h1>

<p>Toutes les valeurs spécifiées au point b) sont utilisées pour notre liste <strong> Public ASF STM </ 0> expliquée ci-dessous, et uniquement pour cela.</p>

<hr />

<h2>Liste publique ASF STM</h2>

<p>Notre liste publique ASF STM se trouve <strong><a href="https://asf.justarchi.net/STM"> ici </ 0> et a pour objectif très simple de permettre à tous les utilisateurs de faire rapidement correspondre les robots ASF.</p>

<p>Grâce à notre liste, tous les utilisateurs ASF et non-ASF intéressés peuvent facilement remarquer les robots actuellement actifs et leur envoyer une offre d'échange STM, ce qui aide les deux utilisateurs, <strong> et vous </ 0>, à éliminer les cartes dupliquées. et dirigez-vous plus loin vers l'achèvement des badges. Nous voulions créer quelque chose comme cela pendant longtemps, car <strong> tout le monde </ 0> apprécie la réponse instantanée aux offres commerciales incluses dans ASF, qui peut considérablement améliorer l'efficacité, ainsi que les informations sur la disponibilité des bots - jusqu'à maintenant. Il était très difficile de faire une liste publique comme celle-ci, et grâce à ASF, c'est beaucoup plus facile.</p>

<h3>Comment ça marche exactement</h3>

<p>ASF envoie les données initiales une fois après la connexion, contenant toutes les propriétés que la liste publique utilise. Ensuite, toutes les 10 minutes, ASF envoie une très petite requête de type "pulsation" qui informe notre serveur que le bot est toujours opérationnel. Si, pour une raison quelconque, la pulsation n’arrivait pas, par exemple à cause de problèmes de réseau, ASF tentera de l’envoyer à chaque minute, jusqu’à ce que le serveur l’enregistre.</p>

<p>Cela permet à notre site Web d'enregistrer les comptes pouvant être utilisés pour le matching, ainsi que s'ils sont toujours actifs. Grâce à cela, notre site Web peut afficher tous les comptes ASF 2FA + STM actifs au cours des <strong> 15 dernières minutes </ 0>.</p>

<p>Les utilisateurs sont triés selon leur inventaire (par ordre décroissant) - <code>MatchableTypes` compte de jeux unique, puis `MatchableTypes` compte, avec ajout de `MatchEverything` répertorié en haut avec `N'importe quel` bannière.</p> 
        Veuillez noter que vous ne serez **pas** affiché sur le site si vous ne remplissez pas toutes les exigences. ASF ne se souciera même pas de communiquer avec notre serveur dans ce cas. Le point b) est donc entièrement ignoré si vous n’avez pas activé intentionnellement ` SteamTradeMatcher` afin de vous aider à faire correspondre les doubles. De plus, les listes publiques sont compatibles uniquement avec la dernière version stable d'ASF et peuvent refuser d'afficher les robots obsolètes, en particulier s'ils ne disposent pas des fonctionnalités essentielles qui ne peuvent être trouvées que dans les versions les plus récentes.
        
        ### API
        
        ASF STM accepte seulement les bots ASF pour l’instant. Il n’y a aucun moyen pour répertorier les bots de tiers sur notre liste pour l’instant (nous ne pouvons pas revoir leur code facilement et s’assurer qu’ils répondent à notre toute logique commerciale).
        
        Si vous cherchez un moyen facile de notre liste d’accès de manière programmatique, nous avons un point de **/Api/Bots</0 très simple que vous pouvez utiliser.</p> 
        
        ### Notice
        
        *L'ensemble du concept, ainsi que l'intégration du site Web et les rapports ASF, sont toujours en version bêta - ils peuvent être améliorés / modifiés au fil du temps - également supprimés si nous estimons que l'intérêt pour cette fonctionnalité n'est pas suffisante.*
        
        * * *
        
        ## Désinscription
        
        La participation dans les statistiques n’est **pas obligatoire**, mais fortement recommandé pour l’avenir du programme. Nous ne vous jugeons pas, et si vous avez l'envie de cacher le fait que vous êtes un utilisateur ASF, vous pouvez désactiver complètement les statistiques en changeant `Statistics`de la configuration global par `false`. Les statistiques désactivées rendent tout le module non opérationnel, et ne feront aucune des actions spécifiées dans notre politique de confidentialité ci-dessus.
        
        > Veuillez noter que désactiver les statistiques peut influencer notre support technique et autres choses qui vous sont offerts (gratuitement), telles que les fonctionnalités ASF et le programme lui-même.