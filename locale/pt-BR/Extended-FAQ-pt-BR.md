# Perguntas Frequentes Adicionais

Nosso FAQ (Perguntas Frequentes) estendido cobre perguntas e respostas um pouco menos comuns que você pode ter. Para questões mais comuns, visite o nosso **[FAQ básico](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ)**.

---

### Quem criou o ASF?

O ASF foi criado em outubro de 2015 por **[Archi](https://github.com/JustArchi)**. Caso você se pergunte, eu sou um **[usuário do Steam](https://steamcommunity.com/profiles/76561198006963719)** como você. Além de jogar eu também amo colocar em uso minhas habilidades e determinação que você pode explorar agora. Não há nenhuma grande companhia envolvida aqui, nenhum time de desenvolvedores e nenhum orçamento de milhões para cobrir tudo isso: apenas eu, consertando coisas que se quebram.

No entanto, o ASF é um projeto de código aberto e é preciso informar que eu não sou o único atrás de tudo o que você vê aqui. Nós temos alguns **[outros](https://github.com/JustArchiNET?q=ASF-)** projetos do ASF que estão sendo desenvolvidos quase exclusivamente por outros desenvolvedores. Até mesmo o núcleo do ASF tem muitos **[contribuidores](https://github.com/JustArchiNET/ArchiSteamFarm/graphs/contributors)** que me ajudaram a tornar isso possível. Além disso, há vários serviços de terceiros que apoiam o desenvolvimento do ASF, especialmente o **[GitHub](https://github.com)**, **[JetBrains](https://www.jetbrains.com)** e o **[Crowdin](https://crowdin.com)**. Também não podemos esquecer das incríveis bibliotecas e ferramentas que tornaram o ASF possível, tais como o **[Rider](https://www.jetbrains.com/rider)** que usamos como IDE (adoramos as adições do **[ReSharper](https://www.jetbrains.com/resharper)**)e especialmente o **[SteamKit2](https://github.com/SteamRE/SteamKit)** sem o qual náo haveria a menor chance do ASF existir. O ASF também não estaria aqui hoje sem meus **[patrocinadores](https://github.com/sponsors/JustArchi)**, **[patrons](https://www.patreon.com/JustArchi)** e vários outros doadores, que me ajudam com tudo o que faço aqui.

Obrigado por ajudar no desenvolvimento do ASF! Você é incrível.

---

### Por que o ASF foi criado?

ASF was created with primary purpose of being fully automated Steam farming tool for Linux, without a need of any external dependencies (such as Steam client). De fato, esse ainda é seu objetivo principal e seu foco, pois meu conceito do ASF não mudou desde lá e eu ainda estou usando da mesma forma que eu usava em 2015. É claro, houveram **muitas** mudanças desde lá, e eu fico feliz de ver o quanto o ASF evoluiu, a maioria por causa de seus usuários pois eu jamais escreveria metade dos recursos apenas por mim.

It's nice to note that ASF was never made to compete with other, similar programs, especially **[Idle Master](https://www.steamidlemaster.com)**, because ASF was never designed to be a desktop/user app, and it still isn't today. Se você analisar o objetivo primário do ASF descrito anteriormente, então você verá que o Idle Master é **o exato oposto** disso. While you can most definitely find similar to ASF programs today, nothing was good enough for me back then (and still isn't today), so I created my own software, the way I wanted it. Ao longo do tempo os usuários migraram para o ASF principalmente devido à robustez, estabilidade e segurança, mas também por conta de todas as características que eu desenvolvi ao longo de todos esses anos. Hoje, o ASF é melhor do que nunca.

---

### Ok, onde está a pegadinha? O que você ganha compartilhando o ASF?

Não há pegadinha, eu criei o ASF **para mim** e o compartilhei com o resto da comunidade na esperança dele ser útil. O mesmo aconteceu em 1991, quando Linus Torvalds **[compartilhou seu primeiro kernel do Linux](https://groups.google.com/forum/#!msg/comp.os.Minix/dlNtH7RRrGA/SwRavCzVE7gJ)** com o resto do mundo. Não há malware, mineração de dados, mineração de criptomoedas ou qualquer outra atividade escondida que gerasse qualquer benefício monetário para mim. O projeto ASF é suportado inteiramente por doações não obrigatórias enviadas por usuários contentes como você. Você pode usar o ASF exatamente da mesma forma que eu o uso, e se você gostar, você sempre pode me pagar um café, mostrando sua gratidão pelo que eu faço.

Também estou usando o ASF como um exemplo perfeito de um projeto moderno em C# que sempre busca a perfeição e as melhores práticas, seja com tecnologia, gerenciamento de projeto ou o próprio código. É minha definição de "fazer a coisa certa", então se você, por algum motivo, aprender algo novo com o meu projeto, isso me deixará mais feliz.

---

### Right after launching ASF I've lost all my accounts/items/friends/(...)!

Statistically speaking, regardless how sad it is, it's guaranteed that shortly after launching ASF there will be at least one guy who will die in a car accident. The difference is that nobody sane will blame ASF for causing it, but for some reason there are people who will accuse ASF of the same just because it happened to their Steam accounts instead. Of course we can understand the reasoning for that, after all ASF operates within Steam platform, so naturally people will accuse ASF of everything that happened to their Steam-related property regardless of lack of any evidence that the software they ran is even remotely connected with that whatsoever.

ASF, as stated in **[FAQ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ#is-it-safe)** as well as **[question above](#ok-where-is-the-catch-what-do-you-gain-from-sharing-asf)**, is free of malware, spyware, data mining and any other potentially unwanted activity, **especially** submission of your sensitive Steam details or taking over your digital property. If something like this has happened to you, we can only say that we're sorry for your loss and recommend you to contact **[Steam support](https://help.steampowered.com)** which hopefully will assist you in the recovery process - because we're not responsible for what happened to you in any way and our conscience is clear. If you believe otherwise, that's your decision, it's pointless to elaborate further, if the above resources providing objective and verifiable ways to confirm our statement didn't convince you, then it's not like anything we write here will anyway.

However, the above doesn't mean that **your actions** done without a common sense with ASF can't contribute to a security issue. For example, you could disregard our security guidelines, expose ASF's **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** interface to the whole internet, and then be surprised that somebody got in and robbed you out of all items. People do it all the time, they think that if there is no domain or any connection to their IP address then nobody will for sure find out their ASF instance. Right as you read it, there are **thousands** if not more fully-automated bots crawling through the web, including random IP addresses, searching for vulnerabilities to discover, and ASF as a quite popular program is also a target of those. We already had enough of people that got "hacked" through their own stupidity like that, so try to learn from their mistakes and be smarter instead of joining them.

Same goes for security of your PC. Yes, having malware on your PC ruins every single security aspect of ASF, as it can read sensitive details from ASF config files or process memory and even influence the program to do stuff that it wouldn't do otherwise. No, the last crack you've obtained from doubtful source was not a "false positive" as somebody has told you, it's one of the most effective ways to gain control over somebody's PC, the guy will infect himself and he'll even follow the instructions how to, fascinating.

Is using ASF completely safe and free of all risks then? No, we'd be bunch of hypocrites stating so, as **every** software has its security-oriented problems. Contrary to what a lot of companies are doing, we're trying to be as transparent as possible in our **[security advisories](https://github.com/JustArchiNET/ArchiSteamFarm/security/advisories)** and as soon as we find out even a *hypothetical* situation where ASF could contribute in any way to a potentially unwanted from security perspective situation, we announce it immediately. This is what happened with **[CVE-2021-32794](https://github.com/JustArchiNET/ArchiSteamFarm/security/advisories/GHSA-wxx4-66c2-vj2v)** for example, even though ASF didn't have any security flaw per-se, but rather a bug that could lead to user accidentally creating one.

As of today, there are no known, unpatched security flaws in ASF, and as the program is used by more and more people out of which both **[white hats](https://en.wikipedia.org/wiki/White_hat_(computer_security))** as well as **[black hats](https://en.wikipedia.org/wiki/Black_hat_(computer_security))** analyze its source code, the overall trust factor only increases with time, as the number of security flaws to find out is finite, and ASF as a program that focuses first and foremost on its security, definitely isn't making it easy for finding one. Regardless of our best intentions, we still recommend to stay cool-headed and always be wary of potential security threats, ones coming from ASF usage as well.

---

### How do I verify that the downloaded files are genuine?

As part of our releases on GitHub, we utilize a very similar verification process as the one used by **[Debian](https://www.debian.org/CD/verify)**. In every official release starting with ASF V5.1.3.3, in addition to `zip` files you can find `SHA512SUMS` and `SHA512SUMS.sign` files. Download them for verification purposes together with the `zip` files of your choice.

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

### It's April the 1st and the ASF language changed into something strange, what is going on?

PARABENZ POR DISHCOBRIRR NOSS EASTR EGG DO JIA PRIMEIR DO ABRIL! If you didn't set custom `CurrentCulture` option, then ASF on April the 1st will actually use **[LOLcat](https://en.wikipedia.org/wiki/Lolcat)** language instead of your system language. Se você quiser desativar esse comportamento, você pode simplesmente configurar `CurrentCulture` para seu idioma preferido. Também é interessante notar que você pode ativar nosso easter egg incondicionalmente configurando a opção `CurrentCulture` com o valor `qps-Ploc`.