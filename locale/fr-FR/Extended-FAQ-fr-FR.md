# FAQ Supplémentaires

Notre FAQ étendue couvre les réponses à certaines questions moins courantes que vous pourriez avoir. Pour des informations plus courantes, consultez plutôt notre **[FAQ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ)** de base.

---

### Qui a créé ASF ?

ASF a été créée par **

Archi</ 0> en octobre 2015. Si vous vous posez la question, je suis un **[utilisateur de Steam](https://steamcommunity.com/profiles/76561198006963719)** tout comme vous. En plus de jouer à des jeux, j'aime aussi utiliser mes compétences et ma détermination, que vous pouvez découvrir dès maintenant. Aucune grande entreprise n'est impliquée ici, aucune équipe de développeurs et aucun budget d'un million de dollars pour couvrir tout cela - juste moi, pour réparer des choses qui ne sont pas cassées.</p> 

Cependant, ASF est un projet open-source, et je ne saurais trop dire que je ne suis pas derrière tout ce que vous pouvez voir ici. Nous avons quelques **[autres](https://github.com/JustArchiNET?q=ASF-)** projets ASF qui sont développées presque exclusivement par d’autres développeurs. En dehors de cela, ASF a beaucoup d'autres **[contributeurs](https://github.com/JustArchiNET/ArchiSteamFarm/graphs/contributors)** qui ont également beaucoup aidé à faire en sorte que tout cela se produise. On top of that, there are several third-party services supporting ASF development, especially **[GitHub](https://github.com)**, **[JetBrains](https://www.jetbrains.com)**, **[AppVeyor](https://www.appveyor.com)** and **[Crowdin](https://crowdin.com)**. You also can't forget about all the awesome libraries and tools that made ASF happen, such as **[Rider](https://www.jetbrains.com/rider)** that we use as IDE (we love **[ReSharper](https://www.jetbrains.com/resharper)** additions) and especially **[SteamKit2](https://github.com/SteamRE/SteamKit)** without which ASF would not exist in the first place. ASF also wouldn't be where it is today without my **[sponsors](https://github.com/sponsors/JustArchi)**, **[patrons](https://www.patreon.com/JustArchi)** and various donators, supporting me in everything that I'm doing here.

Je vous remercie tous pour votre au développement de ASF ! You're awesome.



---



### Pourquoi ASF a-t-elle été créée ?

ASF was created with primary purpose of being fully automated Steam farming tool for Linux, without a need of any external dependencies (such as Steam client). En fait, cela reste toujours son objectif principal, car mon concept d'ASF n'a pas changé depuis et je l'utilise toujours exactement de la même manière que je l'avais utilisé en 2015. Bien sûr, il y a eu **beaucoup** de changements depuis, et je suis très heureux de voir à quel point ASF a progressé, principalement grâce à ses utilisateurs, car je ne coderais même pas la moitié des fonctionnalités si c'était pour moi seulement.

It's nice to note that ASF was never made to compete with other, similar programs, especially **[Idle Master](https://www.steamidlemaster.com)**, because ASF was never designed to be a desktop/user app, and it still isn't today. Si vous analysez l'objectif principal d'ASF décrit ci-dessus, vous verrez alors comment Idle master est **l'opposé total** de tout cela. While you can most definitely find similar to ASF programs today, nothing was good enough for me back then (and still isn't today), so I created my own software, the way I wanted it. Over time users have migrated to ASF mainly due to robustness, stability and security, but also all the features that I've developed across all those years. Today, ASF is better than ever before.



---



### OK, où est le piège? What do you gain from sharing ASF?

There is no catch, I created ASF **for myself** and shared it with the rest of the community in hope that it'll come useful. La même chose s’est produite en 1991, lorsque Linus Torvalds **[a partagé son premier noyau Linux](https://groups.google.com/forum/#!msg/comp.os.Minix/dlNtH7RRrGA/SwRavCzVE7gJ)** avec le reste du monde. There is no hidden malware, data mining, crypto mining or any other activity that would generate any monetary benefit for me. ASF project is supported entirely by non-obligatory donations sent by happy users such as you. You can use ASF in exactly the same way how I'm using it, and if you like it, you can always buy me a coffee, showing your gratitude for what I'm doing.

I'm also using ASF as a perfect example of a modern C# project that always strikes for perfection and best practices, be it with technology, project management or the code itself. It's my definition of "things done right", so if by any chance you manage to learn something useful from my project, then that will make me only more happy.



---



### I've launched the program on April the 1st and the ASF language changed into something strange, what is going on?

CONGRATULASHUNS ON DISCOVERIN R APRIL FOOLS EASTR EGG! If you didn't set custom `CurrentCulture` option, then ASF launched on April the 1st will actually use **[LOLcat](https://en.wikipedia.org/wiki/Lolcat)** language instead of your system language. If by any chance you'd like to disable that behaviour, you can simply set `CurrentCulture` to the locale that you'd like to use instead. It's also nice to note that you can enable our easter egg unconditionally, by setting your `CurrentCulture` to `qps-Ploc` value.