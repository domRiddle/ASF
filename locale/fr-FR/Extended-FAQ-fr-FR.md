# FAQ Supplémentaires

Notre FAQ étendue couvre les réponses à certaines questions moins courantes que vous pourriez avoir. Pour des informations plus courantes, consultez plutôt notre **[FAQ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ)** de base.

---

### Qui a créé ASF ?

ASF a été créée par **

Archi</ 0> en octobre 2015. Si vous vous posez la question, je suis un **[utilisateur de Steam](https://steamcommunity.com/profiles/76561198006963719)** tout comme vous. En plus de jouer à des jeux, j'aime aussi utiliser mes compétences et ma détermination, que vous pouvez découvrir dès maintenant. Aucune grande entreprise n'est impliquée ici, aucune équipe de développeurs et aucun budget d'un million de dollars pour couvrir tout cela - juste moi, pour réparer des choses qui ne sont pas cassées.</p> 

Cependant, ASF est un projet open-source, et je ne saurais trop dire que je ne suis pas derrière tout ce que vous pouvez voir ici. Nous avons quelques **[autres](https://github.com/JustArchiNET?q=ASF-)** projets ASF qui sont développées presque exclusivement par d’autres développeurs. En dehors de cela, ASF a beaucoup d'autres **[contributeurs](https://github.com/JustArchiNET/ArchiSteamFarm/graphs/contributors)** qui ont également beaucoup aidé à faire en sorte que tout cela se produise. On top of that, there are several third-party services supporting ASF development, especially **[GitHub](https://github.com)**, **[JetBrains](https://www.jetbrains.com)** and **[Crowdin](https://crowdin.com)**. You also can't forget about all the awesome libraries and tools that made ASF happen, such as **[Rider](https://www.jetbrains.com/rider)** that we use as IDE (we love **[ReSharper](https://www.jetbrains.com/resharper)** additions) and especially **[SteamKit2](https://github.com/SteamRE/SteamKit)** without which ASF would not exist in the first place. ASF also wouldn't be where it is today without my **[sponsors](https://github.com/sponsors/JustArchi)**, **[patrons](https://www.patreon.com/JustArchi)** and various donators, supporting me in everything that I'm doing here.

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



### How do I verify that the downloaded files are genuine?

As part of our releases on GitHub, we utilize a very similar verification process as the one used by **[Debian](https://www.debian.org/CD/verify)**. In every official release starting with ASF V5.1.3.3, in addition to `zip` files you can find `SHA512SUMS` and `SHA512SUMS.sign` files. Download them for verification purposes.

Firstly, you should use `SHA512SUMS` file in order to verify that `SHA-512` checksum of the selected `zip` files matches the one we calculated ourselves. On Linux, you can use `sha512sum` utility for that purpose.




```
$ sha512sum -c --ignore-missing SHA512SUMS
ASF-linux-x64.zip: OK
```


On Windows, we can do that from powershell, although you have to manually verify with `SHA512SUMS`:



```
PS > Get-Content SHA512SUMS | Select-String -Pattern ASF-linux-x64.zip

f605e573cc5e044dd6fadbc44f6643829d11360a2c6e4915b0c0b8f5227bc2a257568a014d3a2c0612fa73907641d0cea455138d2e5a97186a0b417abad45ed9  ASF-linux-x64.zip


PS > Get-FileHash -Algorithm SHA512 -Path ASF-linux-x64.zip

Algorithm       Hash                                                                   Path
---------       ----                                                                   ----
SHA512          F605E573CC5E044DD6FADBC44F6643829D11360A2C6E4915B0C0B8F5227BC2A2575... ASF-linux-x64.zip
```


This way we ensured that whatever was written to `SHA512SUMS` matches the resulting files and they weren't tampered with. However, it doesn't prove yet that `SHA512SUMS` file you checked against really comes from us. For that, we'll use `SHA512SUMS.sign` file, which holds digital PGP signature proving the authenticity of `SHA512SUMS`. We can use `gpg` utility for that purpose, both on **[Linux](https://gnupg.org/download/index.html)** and **[Windows](https://gpg4win.org)** (change `gpg` command into `gpg.exe` on Windows).



```
$ gpg --verify SHA512SUMS.sign SHA512SUMS
gpg: Signature made Mon 02 Aug 2021 00:34:18 CEST
gpg:                using EDDSA key 224DA6DB47A3935BDCC3BE17A3D181DF2D554CCF
gpg: Can't check signature: No public key
```


As you can see, the file indeed holds a valid signature, but of unknown origin. You'll need to import ArchiBot's **[public key](https://raw.githubusercontent.com/JustArchi-ArchiBot/JustArchi-ArchiBot/main/ArchiBot_public.asc)** that we sign the `SHA-512` sums with for full validation.



```
$ curl https://raw.githubusercontent.com/JustArchi-ArchiBot/JustArchi-ArchiBot/main/ArchiBot_public.asc -o ArchiBot_public.asc
$ gpg --import ArchiBot_public.asc
gpg: /home/archi/.gnupg/trustdb.gpg: trustdb created
gpg: key A3D181DF2D554CCF: public key "ArchiBot <ArchiBot@JustArchi.net>" imported
gpg: Total number processed: 1
gpg:               imported: 1

```


Finally, you can verify the `SHA512SUMS` file again:



```
$ gpg --verify SHA512SUMS.sign SHA512SUMS
gpg: Signature made Mon 02 Aug 2021 00:34:18 CEST
gpg:                using EDDSA key 224DA6DB47A3935BDCC3BE17A3D181DF2D554CCF
gpg: Good signature from "ArchiBot <ArchiBot@JustArchi.net>" [unknown]
gpg: WARNING: This key is not certified with a trusted signature!
gpg:          There is no indication that the signature belongs to the owner.
Primary key fingerprint: 224D A6DB 47A3 935B DCC3  BE17 A3D1 81DF 2D55 4CCF
```


This has verified that the `SHA512SUMS.sign` holds a valid signature of our `224DA6DB47A3935BDCC3BE17A3D181DF2D554CCF` key for `SHA512SUMS` file that you've verified against.

You could be wondering where the last warning comes from. You've successfully imported our key, but didn't decide to trust it just yet. While this is not mandatory, we can cover it as well. Normally this includes verifying through different channel (e.g. phone call, SMS) that the key is valid, then signing the key with your own to trust it. For this example, you can consider this wiki entry as such (very weak) different channel, since the original key comes from **[ArchiBot's profile](https://github.com/JustArchi-ArchiBot)**. In any case we'll assume that you have enough of confidence as it is.

Firstly, **[generate private key for yourself](https://help.ubuntu.com/community/GnuPrivacyGuardHowto#Generating_an_OpenPGP_Key)**, if you don't have one just yet. We'll use `--quick-gen-key` as a quick example.



```
$ gpg --batch --passphrase '' --quick-gen-key "$(whoami)"
gpg: /home/archi/.gnupg/trustdb.gpg: trustdb created
gpg: key E4E763905FAD148B marked as ultimately trusted
gpg: directory '/home/archi/.gnupg/openpgp-revocs.d' created
gpg: revocation certificate stored as '/home/archi/.gnupg/openpgp-revocs.d/8E5D685F423A584569686675E4E763905FAD148B.rev'
```


Now you can sign our key with yours in order to trust it:



```
$ gpg --sign-key 224DA6DB47A3935BDCC3BE17A3D181DF2D554CCF

pub  ed25519/A3D181DF2D554CCF
     created: 2021-05-22  expires: never       usage: SC
     trust: unknown       validity: unknown
sub  cv25519/E527A892E05B2F38
     created: 2021-05-22  expires: never       usage: E
[ unknown] (1). ArchiBot <ArchiBot@JustArchi.net>


pub  ed25519/A3D181DF2D554CCF
     created: 2021-05-22  expires: never       usage: SC
     trust: unknown       validity: unknown
 Primary key fingerprint: 224D A6DB 47A3 935B DCC3  BE17 A3D1 81DF 2D55 4CCF

     ArchiBot <ArchiBot@JustArchi.net>

Are you sure that you want to sign this key with your
key "archi" (E4E763905FAD148B)

Really sign? (y/N) y
```


And done, after trusting our key, `gpg` should no longer display the warning when verifying:



```
$ gpg --verify SHA512SUMS.sign SHA512SUMS
gpg: Signature made Mon 02 Aug 2021 00:34:18 CEST
gpg:                using EDDSA key 224DA6DB47A3935BDCC3BE17A3D181DF2D554CCF
gpg: Good signature from "ArchiBot <ArchiBot@JustArchi.net>" [full]
```


Notice the `[unknown]` trust indicator changing into `[full]` once you signed our key with yours.

Congratulations, you've verified that nobody has tampered with the release you've downloaded! 👍



---



### I've launched the program on April the 1st and the ASF language changed into something strange, what is going on?

CONGRATULASHUNS ON DISCOVERIN R APRIL FOOLS EASTR EGG! If you didn't set custom `CurrentCulture` option, then ASF launched on April the 1st will actually use **[LOLcat](https://en.wikipedia.org/wiki/Lolcat)** language instead of your system language. If by any chance you'd like to disable that behaviour, you can simply set `CurrentCulture` to the locale that you'd like to use instead. It's also nice to note that you can enable our easter egg unconditionally, by setting your `CurrentCulture` to `qps-Ploc` value.