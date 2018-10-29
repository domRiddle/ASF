# Partage Familial Steam

ASF prend en charge le partage de famille Steam depuis la version 2.1.5.5+. Pour comprendre comment ASF fonctionne avec cela, vous devez d’abord lire le fonctionnement du **[Partage familial](https://store.steampowered.com/promotion/familysharing)**, disponible sur le magasin Steam.

* * *

## ASF

La prise en charge de la fonctionnalité du partage familial dans ASF est transparente, ce qui signifie qu’elle ne présente aucune nouvelle propriété de configuration bot / process. Elle fonctionne immédiatement en tant que fonctionnalité intégrée supplémentaire.

ASF inclut une logique appropriée afin de s’assurer que la bibliothèque est verrouillée par les utilisateurs qui partagent en famille. Par conséquent, elle ne les "coupera" pas de la session en raison du lancement d’une partie. ASF agira exactement de la même manière que le compte principal qui détient le verrou. Par conséquent, si ce verrou est détenu par votre client steam ou par l’un de vos utilisateurs partageant la famille, ASF ne tentera pas de créer une ferme, mais attendra que le verrou soit libre.

En plus de ce qui précède, une fois connecté, ASF accédera à vos **[ paramètres de partage de jeux ](https://store.steampowered.com/account/managedevices)**, à partir desquels il extraira jusqu'à 5 ` steamIDs ` autorisés à utiliser votre bibliothèque. Ces utilisateurs reçoivent l’autorisation ` PartageFamilial` d’utiliser **[les commandes](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**, leur permettant notamment d’utiliser la commande ` pause ~ ` sur le compte bot qui partage des jeux avec eux, qui permet de mettre en pause le module de farm automatique de cartes afin de lancer un jeu pouvant être partagé.

La connexion des deux fonctionnalités décrites ci-dessus permet à vos amis de ` mettre en pause ~` le processus de farm de cartes, de démarrer une partie, de jouer aussi longtemps qu’ils le souhaitent, puis, une fois la lecture terminée, le processus de farm sera automatiquement repris par ASF. Bien entendu, l'émission de ` pause ~` n'est pas nécessaire si ASF n'effectue actuellement aucune activité de farm active, car vos amis peuvent lancer le jeu immédiatement, et la logique décrite ci-dessus garantit qu'ils ne seront pas expulsés du serveur.

* * *

## Restriction

Le réseau Steam aime induire en erreur ASF en diffusant de fausses mises à jour de statut, ce qui pourrait laisser croire qu'ASF pense qu'il est bon de reprendre le processus et que, par conséquent, expulse votre ami trop tôt. Afin de lutter contre ce problème, il est recommandé d’inscrire votre ami dans votre liste d’amis de Steam - ainsi ASF, en plus des événements du réseau Steam, peut vérifier le statut Steam de votre ami et deviner son statut de joueur s’il a déjà fini de jouer ou pas. Ce n'est pas obligatoire, mais comme Steam parle parfois de corbeille, c'est recommandé, surtout si vous remarquez de tels problèmes.