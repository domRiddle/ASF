# Configuration à hautes performances

C’est exactement le contraire de **[low-memory setup](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup)** et vous souhaitez généralement suivre ces conseils si vous souhaitez augmenter d'avantage les performances ASF (en termes de vitesse du processeur), pour un coût potentiel d’utilisation accrue de la mémoire.

* * *

ASF essaie déjà de privilégier les performances en matière de réglage équilibré général. Par conséquent, vous ne pouvez rien faire pour augmenter ses performances, même si vous n’êtes pas non plus complètement à court d’options. Cependant, gardez à l'esprit que ces options ne sont pas activées par défaut, ce qui signifie qu'elles ne sont pas suffisantes pour être considérées comme équilibrées pour la majorité des utilisations. Vous devez donc décider vous-même si l'augmentation de mémoire apportée par ses options est acceptable pour vous.

* * *

## Runtime tuning (avancé)

Les astuces ci-dessous **impliquent une augmentation importante de la mémoire** et doivent être utilisées avec prudence.

`ArchiSteamFarm.runtimeconfig.json` vous permet d’ajuster l’exécution d’ASF, ce qui vous permet notamment de basculer entre les serveurs GC et ceux du poste de travail.

> Le garbage collector est à réglage automatique et peut fonctionner dans une grande variété de scénarios. Vous pouvez utiliser un paramètre de fichier de configuration pour définir le type de garbage collection en fonction des caractéristiques du poste de travail. Le CLR fournit les types de récupération de place suivants:
> 
> Collecte des ordures de poste de travail, qui concerne tous les postes de travail clients et les ordinateurs autonomes. C'est le réglage par défaut pour l' <gcserver> élément dans le schéma de configuration d'exécution.
> 
> La récupération de place du serveur, destinée aux applications serveur nécessitant un débit et une évolutivité élevés. La récupération de place du serveur peut être non simultanée ou en arrière-plan.

Vous pouvez en lire plus à **[ les principes de base de la collecte des déchets ](https://docs.microsoft.com/en-us/dotnet/standard/garbage-collection/fundamentals)**.

ASF utilise le garbage collection du poste de travail par défaut. Ceci est principalement dû au bon équilibre entre l'utilisation de la mémoire et les performances, ce qui est amplement suffisant pour seulement quelques robots, puisqu'un seul thread GC simultané en arrière-plan est suffisamment rapide pour gérer toute la mémoire allouée par ASF.

Cependant, nous disposons aujourd'hui de nombreux cœurs de processeur dont ASF peut grandement tirer parti, en disposant d'un thread GC dédié pour chaque vCore de processeur disponible. Cela peut considérablement améliorer les performances lors de tâches ASF lourdes telles que l'analyse des pages de badges ou de l'inventaire, car chaque processeur vCore peut aider, par opposition à 2 (principal et GC). Serveur GC est recommandé pour les machines avec 3 vCores de processeur et plus, workstation GC est automatiquement forcé si votre ordinateur ne dispose que d'un processeur vCore et si vous en avez exactement 2, vous pouvez envisager d'essayer les deux (les résultats peuvent varier).

Vous pouvez activer le CPG du serveur en remplaçant la propriété ` System.GC.Server `de` ArchiSteamFarm.runtimeconfig.json` de ` false` à `true</0 >. N'oubliez pas que vous devrez peut-être le faire plusieurs fois car ASF utilisera toujours <code> false ` par défaut après la mise à jour automatique.

Le serveur GC lui-même n'entraîne pas une très forte augmentation de la mémoire en étant simplement actif, mais il a des tailles de génération beaucoup plus grandes et est donc beaucoup plus paresseux lorsqu'il s'agit de restituer de la mémoire à un système d'exploitation. Vous vous trouverez peut-être dans une situation idéale où le GC serveur augmente considérablement les performances et vous souhaitez continuer à l'utiliser, mais vous ne pouvez en même temps pas vous permettre cette énorme augmentation de mémoire résultant de son utilisation. Heureusement pour vous, il existe un paramètre "meilleur des deux mondes", en utilisant le serveur GC avec **[ niveau de latence du GC ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup#gclatencylevel)** défini sur ` 0 `, ce qui permettra toujours d'activer le serveur GC, mais limite tailles de génération et se concentrer davantage sur la mémoire.

Toutefois, si la mémoire ne vous pose pas de problème (car GC tient toujours compte de votre mémoire disponible et s’ajuste lui-même), il est préférable de ne pas modifier `GCLatencyLevel`, afin d’obtenir des performances supérieures.

* * *

## Optimisation recommandée

- Ensure that you're using default value of `OptimizationMode` which is `MaxPerformance`. This is by far the most important setting, as using `MinMemoryUsage` value has dramatic effects on performance.
- Activez le serveur GC en commutant la propriété ` System.GC.Server ` de ` ArchiSteamFarm.runtimeconfig.json ` de ` false ` à ` true `. Cela permettra au serveur GC qui peut être immédiatement considéré comme actif par augmentation de la mémoire par rapport au poste de travail GC.
- Si vous ne pouvez pas vous permettre une telle augmentation de mémoire, envisagez d’utiliser ` GCLatencyLevel ` sur ` 0 ` pour obtenir «le meilleur des deux mondes». Toutefois, si votre mémoire le permet, il est préférable de la conserver par défaut. Le serveur GC se peaufine déjà pendant l'exécution et est suffisamment intelligent pour utiliser moins de mémoire lorsque votre système d'exploitation en a réellement besoin.

Si vous avez activé le CPG du serveur et maintenu le paramètre par défaut sur ` GCLatencyLevel `, vous bénéficiez de performances ASF supérieures qui devraient être rapides, même avec des centaines ou des milliers de robots activés. Le processeur ne devrait plus être un goulet d'étranglement, car ASF est en mesure d'utiliser toute la puissance de son processeur en cas de besoin, réduisant ainsi le temps requis au strict minimum.