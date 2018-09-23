# Activateur de Jeux de Fond

L'activateur de jeux de fond est une fonction spéciale intégrée à ASF vous permettant d'importer un groupe donné de clés cd Steam (avec leurs noms) à activer en tâche de fond. This is especially useful if you have a lot of keys to redeem and you're guaranteed to hit `RateLimited` **[status](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ#what-is-the-meaning-of-status-when-redeeming-a-key)** before you're done with your entire batch.

L'activateur de jeux de fond est conçu pour n'utiliser qu'une seule commande de bot, et donc n'utilise pas `RedeemingPreferences`. Cette fonction peut être utilisée en même temps que (ou à la place de) **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**`redeem`, si besoin est.

* * *

## Importation

Le processus d'importation peut être effectué de deux façons - par fichier, ou par IPC.

### Fichier

ASF peut reconnaître dans son répertoire `config` un fichier nommé `BotName.keys`, où `BotName` est le nom de votre bot. Ce fichier dispose d'une structure fixe et attendue consistant en le nom du jeu avec la clé cd, séparée par une tabulation et se terminant par un retour à la ligne. Si plusieurs onglets sont utilisés, la première entrée est considérée comme étant le nom du jeu, la dernière entrée est considérée comme étant une clé cd, et tout ce qui est entre les deux est ignoré. Par exemple :

    POSTAL 2    ABCDE-EFGHJ-IJKLM
    Domino Craft VR 12345-67890-ZXCVB
    A Week of Circus Terror POIUY-KJHGD-QWERT
    Terraria    Ceciestignoré   Ceciestaussiignoré    ZXCVB-ASDFG-QWERT
    

ASF importera ce fichier au lancement du bot ou plus tard durant l'exécution. Une fois le fichier analysé et les éventuelles entrées invalides omises, tous les jeux correctement détectés seront ajoutés à la file d'attente de fond, et le fichier `BotName.keys` sera retiré du répertoire `config`.

### IPC

En plus d'utiliser le fichier de clés mentionné ci-dessus, ASF expose également l'**[API endpoint](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#post-apigamestoredeeminbackgroundbotname)**`GamesToRedeemInBackground` qui peut être exécuté par n'importe quel outil IPC, y compris notre GUI IPC. L'utilisation d'IPC est plus efficace car vous pouvez effectuer une meilleure analyse par vous-même, par exemple en utilisant un délimiteur personnalisé au lieu d'utiliser forcément une tabulation.

* * *

## File d'attente

Une fois les jeux importés, ils sont ajoutés à la file d'attente. ASF parcourt automatiquement sa file d'attente de fond tant que le bot est connecté au réseau Steam, et tant que la file d'attente n'est pas vide. Une clé qui tente d'être activée sans résulter en `RateLimited` est retirée de la file d'attente, avec son statut rédigé correctement dans un fichier dans le répertoire `config` - soit `BotName.keys.used` si la clé a été utilisée durant le processus (par exemple `NoDetail`, `BadActivationCode`, `DuplicateActivationCode`), sinon dans `BotName.keys.unused`. ASF utilise intentionnellement le nom du jeu que vous fournissez, car la clé n'a pas forcément de nom valable fourni par le réseau Steam - vous pouvez ainsi marquer vos clés avec des noms personnalisés si besoin est/si vous le désirez.

Si notre compte atteint le statut `RateLimited` durant le processus, la file d'attente est suspendue temporairement pendant une heure entière, jusqu'à la fin du délai d'attente. Une fois ce délai terminé, le processus continue jusqu'à la fin de la file d'attente.

* * *

## Exemple

Considérons que vous avez une liste de 100 clés. Tout d'abord, vous devez créer un fichier `BotName.keys.new` dans le répertoire `config` d'ASF. Nous ajoutons l'extension `.new` pour qu'ASF sache qu'il ne doit pas récupérer ce fichier dès sa création (car c'est un fichier vide, pas encore prêt à l'importation).

Maintenant, nous pouvons ouvrir notre nouveau fichier et copier-coller notre liste de 100 clés dedans, en réglant le format si besoin est. Après nos réglages, le fichier `BotName.keys.new` aura exactement 100 (ou 101, avec le dernier retour à la ligne) lignes, chaque ligne ayant une structure de `GameName\tcd-key\n`, où `\t` correspond à une tabulation et `\n` à un retour à la ligne.

You're now ready to rename this file from `BotName.keys.new` to `BotName.keys` in order to let ASF know that it's ready to be picked up. The moment you do this, ASF will automatically import the file (without a need of restart) and delete it afterwards, confirming that all our games were parsed and added to the queue.

Instead of using `BotName.keys` file, you could also use IPC API endpoint, or even combining both if you want to.

After some time, `BotName.keys.used` and `BotName.keys.unused` files might get generated. Those files contain results of our redeeming process. For example, you could rename `BotName.keys.unused` into `BotName2.keys` file and therefore submit our unused keys for some other bot, since previous bot didn't make use of those keys himself. Or you could simply copy-paste unused keys to some other file and keep it for later, your call. Keep in mind that as ASF goes through the queue, new entries will be added to our output `used` and `unused` files, therefore it's recommended to wait for the queue to be fully emptied before making use of them. If you absolutely must access those files before queue is fully emptied, you should firstly **move** output file you want to access to some other directory, **then** parse it. This is because ASF can append some new results while you're doing your thing, and that could potentially lead to loss of some keys if you read a file having e.g. 3 keys inside, then delete it, totally missing the fact that ASF added 4 other keys to your removed file in the meantime. If you want to access those files, ensure to move them away from ASF `config` directory before reading them, for example by rename.

It's also possible to add extra games to import while having some games already in our queue, by repeating all above steps. ASF will properly add our extra entries to already-ongoing queue and deal with it eventually.

* * *

## Remarques

Background keys redeemer uses `OrderedDictionary` under the hood, which means that your cd-keys will have preserved order as they were specified in the file (or IPC API call). This means that you can (and should) provide a list where given cd-key can only have direct dependencies on cd-keys listed above, but not below. For example, this means that if you have DLC `D` that requires game `G` to be activated firstly, then cd-key for game `G` should **always** be included before cd-key for DLC `D`. Likewise, if DLC `D` would have dependencies on `A`, `B` and `C`, then all 3 should be included before (in any order, unless they have dependencies on their own).

Not following the scheme above will result in your DLC not being activated with `DoesNotOwnRequiredApp`, even if your account would be eligible for activating it after going through its entire queue. If you want to avoid that then you must make sure that your DLC is always included after the base game in your queue.