# Compilation

La compilation est le processus de création d'un fichier exécutable. C’est ce que vous souhaitez faire si vous souhaitez ajouter vos propres modifications à ASF ou si, pour une raison quelconque, vous ne faites pas confiance aux fichiers exécutables fournis dans les ** versions officielles </ 0>. Si vous êtes utilisateur et non développeur, vous voudrez probablement utiliser des binaires déjà précompilés, mais si vous souhaitez utiliser vos propres binaires ou apprendre quelque chose de nouveau, continuez à lire.</p> 

ASF peut être compilé sur n’importe quelle plate-forme actuellement prise en charge, à condition que vous disposiez de tous les outils nécessaires pour le faire.

* * *

## .NET Core SDK

Quelle que soit la plate-forme, vous avez besoin du SDK .NET Core complet (pas seulement de l'exécutable) pour compiler ASF. Les instructions d'installation se trouvent sur la ** page d'installation de .NET Core </ 0>. Vous devez installer la version appropriée du .NET Core SDK pour votre système d'exploitation. Une fois l'installation réussie, la commande ` dotnet </ 0> devrait fonctionner et être opérationnelle. Vous pouvez vérifier si cela fonctionne avec <code> dotnet --info </ 0>. Assurez-vous également que votre SDK .NET Core répond aux exigences d'exécution ASF <strong><a href="https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements"> </ 0>.</p>

<hr />

<h2>Compilation</h2>

<p>En supposant que vous avez un SDK .NET Core opérationnel dans sa version appropriée, il vous suffit de naviguer vers le répertoire source ASF (cloné ou téléchargé et dés-archivé) et d'exécuter :</p>

<pre><code class="shell">dotnet publish ArchiSteamFarm -c "Release" -f "netcoreapp2.2" -o "out/generic" "/p:LinkDuringPublish=false"
`</pre> 

Si vous utilisez Linux / OS X, vous pouvez utiliser le script ` cc.sh </ 0> qui fera de même, de manière un peu plus complexe.</p>

<p>Si la compilation s'est terminée avec succès, vous pouvez trouver votre fichier ASF dans le répertoire <code> source </ 0> du répertoire <code>ArchiSteamFarm/out/generic</ 0>. Il s'agit de la même chose que la version officielle <code> générique </ 0> ASF, mais elle a imposé <code> UpdateChannel </ 0> et <code> UpdatePeriod </ 0> à <code> 0 </ 0>.</p>

<h3>OS-spécifique</h3>

<p>Vous pouvez également générer un package .NET Core spécifique au système d'exploitation si vous avez un besoin spécifique. En général, vous ne devriez pas le faire car vous venez de compiler un <code> générique </ 0> que vous pouvez exécuter avec votre environnement d'exécution .NET Core déjà installé que vous avez utilisé pour la compilation, mais uniquement si vous voulez:</p>

<pre><code class="shell">dotnet publish ArchiSteamFarm -c "Release" -f "netcoreapp2.2" -o "out/linux-x64" -r "linux-x64" "/p:CrossGenDuringPublish=false"
`</pre> 

Bien sûr, remplacez ` linux-x64 ` par l'architecture du système d'exploitation que vous souhaitez cibler, tel que ` win-x64 `. Cette mise à jour aura également des mises à jour désactivées.

### .NET Framework 

Dans de très rares cas où vous souhaitez créer le package ` generic-netf </ 0>, vous pouvez modifier le cadre cible de <code> netcoreapp2.2 </ 0> à <code> net472 </ 0>. N'oubliez pas que vous aurez besoin du pack de développeurs <strong><a href="https://dotnet.microsoft.com/download/visual-studio-sdks"> .NET Framework </a></strong> approprié pour la compilation de la variante <code> netf `, en plus du kit de développement .NET Core SDK, donc les éléments ci-dessous ne fonctionneront que sur Windows :

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net472" -o "out/generic-netf"
```

Dans des cas encore plus rares, si vous ne pouvez pas installer .NET Framework ou même .NET Core SDK lui-même (par exemple, en raison de la construction sur ` linux-x86 ` avec ` mono `), vous pouvez appeler ` msbuild ` directement. Vous devrez également spécifier `ASFNetFramework` manuellement, car ASF désactive par défaut la construction netf sur les plates-formes non Windows :

```shell
msbuild /m /p:Configuration=Release /p:PublishDir=out/generic-netf /p:TargetFramework=net472 /p:ASFNetFramework=true /r /t:Publish ArchiSteamFarm
```

* * *

## Développement

Si vous souhaitez modifier le code ASF, vous pouvez utiliser n'importe quel IDE compatible avec .NET Core à cette fin, même si cela reste facultatif. Vous pouvez également éditer avec un bloc-notes et compiler avec la commande  dotnet </ 0>. décrit ci-dessus. Néanmoins, pour Windows, nous recommandons la <strong><a href="https://visualstudio.microsoft.com/downloads"> dernière version de Visual Studio </ 0> (la version communautaire gratuite est largement suffisante). Nous vous suggérons également de l’utiliser avec <strong><a href="https://www.jetbrains.com/resharper"> ReSharper </a></strong> (facultatif), bien qu’il ne s’agisse pas d’un produit gratuit.</p>

<p>Si vous souhaitez plutôt utiliser du code ASF sous Linux / OS X, nous vous recommandons <strong><a href="https://code.visualstudio.com/download"> le dernier Visual Studio Code</ 0>. Ce n'est pas aussi complet que le classique Visual Studio, mais c'est suffisant.</p>

<p>Bien sûr, toutes les suggestions ci-dessus ne sont que des recommandations, vous pouvez utiliser ce que vous voulez, cela revient à la commande <code> dotnet build </ 0>. Nous utilisons Visual Studio + ReSharper pour le développement ASF, avec une petite partie des <code> outils tiers </ 0> que vous pouvez trouver dans le référentiel.</p>

<hr />

<h2>Tags</h2>

<p>Il n’est pas garanti que la branche <code> master </ 0> soit dans un état permettant une compilation réussie ou une exécution sans faille du fichier ASF en premier lieu, étant donné qu’elle est une branche de développement comme l’indique notre <strong><a href="https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle"> cycle de publication</ 1>. Si vous voulez compiler ASF à partir des sources, vous devez alors utiliser le <strong><a href="https://github.com/JustArchiNET/ArchiSteamFarm/tags">tag</ 0> appropriée, ce qui garantit au moins une compilation réussite et très probablement une exécution sans faille (si la construction a été marquée comme une version stable). Pour vérifier la "santé" actuelle de l’arbre, vous pouvez utiliser nos CIs - <strong><a href="https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm">AppVeyor</a></strong> ou <strong><a href="https://travis-ci.com/JustArchiNET/ArchiSteamFarm">Travis</a></strong>.</p>

<hr />

<h2>Versions Officielles</h2>

<p>Les versions officielles ASF sont compilées par <strong><a href="https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm"> AppVeyor </ 0> sous Windows, avec le dernier SDK .NET Core correspondant aux  <strong><a href="https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements">exigences d'exécution</ 1> ASF Une fois les tests passés, tous les packages sont déployés sur GitHub. Cela garantit également la transparence, car AppVeyor utilise toujours des sources publiques officielles pour toutes les versions, et vous pouvez comparer les taux de contrôle de AppVeyor avec les actifs GitHub. Les développeurs ASF ne compilent, ni ne publient les versions manuellement, sauf pour les processus de développement privés, y compris le débogage.</p>