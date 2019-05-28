# Arguments de ligne de commande

ASF prend en charge plusieurs arguments de ligne de commande qui peuvent affecter l’exécution du programme. Ils peuvent être utilisés par des utilisateurs avancés pour spécifier au programme son mode de fonctionnement. Par rapport au fichier de configuration `ASF.json` par défaut, les arguments de ligne de commande sont utilisés pour l'initialisation principale (e.g. `--path`). paramètres spécifiques à la plate-forme (e.g. `--system-required`) ou données sensibles (e.g. `--cryptkey`).

* * *

## Utilisation

Son utilisation dépend de votre OS et ASF.

Générique :

```shell
dotnet ArchiSteamFarm.dll --argument --otherOne
```

Windows :

```powershell
.\ArchiSteamFarm.exe --argument --otherOne
```

Linux/OS X

```shell
./ArchiSteamFarm --argument --otherOne
```

Les arguments de ligne de commande sont également pris en charge dans les scripts d'assistance génériques tels que ` ArchiSteamFarm.cmd </ 0> ou <code> ArchiSteamFarm.sh </ 0>. De plus, lorsque vous utilisez des scripts d'assistance, vous pouvez également utiliser la propriété d'environnement <code> ASF_ARGS </ 0>, comme indiqué dans notre section <strong><a href="https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker#command-line-arguments">docker</ 1>.</p>

<p>Si votre argument comprend des espaces, n'oubliez pas de le citer. Ces deux exemples sont faux:</p>

<pre><code class="shell">./ArchiSteamFarm --path /home/archi/My Downloads/ASF # Bad
./ArchiSteamFarm --path=/home/archi/My Downloads/ASF # Bad!
`</pre> 

Cependant, ces deux la sont complètement valides:

```shell
./ArchiSteamFarm --path "/home/archi/My Downloads/ASF" # OK
./ArchiSteamFarm "--path=/home/archi/My Downloads/ASF" # OK
```

## Arguments

`--cryptkey <key>` ou `--cryptkey=<key>` - démarrera ASF avec une clé cryptographique personnalisée de la valeur `<key>`. Cette option affecte la ** sécurité </ 0> et obligera ASF à utiliser votre clé `<key>` fournie à la place de la clé par défaut codée dans l'exécutable. Gardez à l'esprit que les mots de passe chiffrés avec cette clé nécessiteront sa transmission à chaque exécution du fichier ASF.</p> 

Due to the nature of this property, it's also possible to set cryptkey by declaring `ASF_CRYPTKEY` environment variable, which may be more appropriate for people that would want to avoid sensitive details in the process arguments.

* * *

`--no-restart` - cette fonctionnalité est principalement utilisé par nos **[docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker)** ` et force  <code>AutoRestart` de <0> false</ 0>. Sauf si vous avez un besoin particulier, vous devriez plutôt configurer la propriété `AutoRestart` directement dans votre configuration. Cette fonctionalité est ici pour que notre script docker n'ait pas besoin de toucher votre configuration globale afin de l'adapter à son propre environnement. Of course, if you're running ASF inside a script, you may also make use of this switch (otherwise you're better with global config property).

* * *

`--path <path>` ou `--path=<path>` - ASF navigue toujours vers son propre répertoire au démarrage. By specifying this argument, ASF will navigate to given directory after initialization, which allows you to use custom path for various application parts (including `config`, `plugins` and `www` directories, as well as `NLog.config` file), without a need of duplicating binary in the same place. It may come especially useful if you'd like to separate binary from actual config, as it's done in Linux-like packaging - this way you can use one (up-to-date) binary with several different setups. Le chemin peut être relatif en fonction de l'emplacement actuel du binaire ASF ou en absolu. Lorsque vous exécutez plusieurs instances du même fichier binaire, n'oubliez pas que vous devez généralement désactiver les mises à jour automatiques, car elles ne sont pas synchronisées. N'oubliez pas non plus que cette commande pointe vers le nouveau "ASF home" - le répertoire qui a la même structure que l'ASF d'origine, avec le répertoire `config` à l'intérieur.

Exemple :

```shell
dotnet /opt/ASF/ArchiSteamFarm.dll --path /opt/TargetDirectory # Chemin 'absolu'
dotnet /opt/ASF/ArchiSteamFarm.dll --path ../TargetDirectory # Chemin alternatif, fonctionne aussi
```

    ├── /opt
    │     ├── ASF
    │     │     ├── ArchiSteamFarm.dll
    │     │     └── ...
    │     └── TargetDirectory
    │           ├── config
    │           ├── plugins (optional)
    │           ├── www (optional)
    │           └── NLog.config (optional)
    └── ...
    

* * *

`--process-required` la fonctionnalité de ce commutateur désactivera le la méthode ASF par défaut d'arrêt lorsque aucun bot ne s'exécute. Aucun comportement d’arrêt automatique n’est particulièrement utile en combinaison avec **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)**, où la majorité des utilisateurs s’attendraient à ce que leur service Web s’exécute, quel que soit le nombre de bots activés. Si vous utilisez l'option IPC ou si vous avez besoin que le processus ASF soit actif jusqu'à ce que vous le fermiez vous-même, c'est la bonne option.

Si vous n'avez pas l'intention de faire fonctionner IPC, cette option sera plutôt inutile pour vous, car vous pouvez simplement relancer le processus si nécessaire (contrairement au serveur Web d'ASF où vous avez besoin de l'écouter tout le temps pour pouvoir envoyer des commandes).

* * *

`--system-required</ 0> - la déclaration de ce commutateur incitera ASF à signaler au système d'exploitation que le processus nécessite que le système soit opérationnel pendant toute sa durée de vie. Actuellement, ce commutateur n'a d'effet que sur les machines Windows où il sera interdit à votre système de passer en mode veille tant que le processus est en cours d'exécution. Cela peut s’avérer particulièrement utile lorsque vous marchez au ralenti sur votre PC ou votre ordinateur portable pendant la nuit, car ASF sera en mesure de maintenir votre système en veille pendant son repos, puis, une fois cette opération effectuée, elle s’arrêtera comme d’habitude, ce qui permettra à votre système de fonctionner correctement. entrer à nouveau en mode veille, donc économiser de l'énergie immédiatement une fois que la phase de repos est terminé.</p>

<p>Gardez à l’esprit que pour un arrêt automatique correct de la fonction ASF, il vous faut d’autres paramètres, en particulier pour éviter <code>--process-required` et en veillant à ce que tous vos robots suivent `ShutdownOnFarmingFinished`. Bien sûr, l’arrêt automatique n’est qu’une possibilité pour cette fonction, et non une obligation, car vous pouvez également utiliser cet fonction avec, par exemple, `--process-required`, ce qui rend votre système éveillé à l'infini après le démarrage d'ASF.