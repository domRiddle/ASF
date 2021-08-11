# Perguntas Frequentes Adicionais

Nosso FAQ (Perguntas Frequentes) estendido cobre perguntas e respostas um pouco menos comuns que você pode ter. Para questões mais comuns, visite o nosso **[FAQ básico](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ)**.

---

### Quem criou o ASF?

O ASF foi criado em outubro de 2015 por **[Archi](https://github.com/JustArchi)**. Caso você se pergunte, eu sou um **[usuário do Steam](https://steamcommunity.com/profiles/76561198006963719)** como você. Além de jogar eu também amo colocar em uso minhas habilidades e determinação que você pode explorar agora. Não há nenhuma grande companhia envolvida aqui, nenhum time de desenvolvedores e nenhum orçamento de milhões para cobrir tudo isso: apenas eu, consertando coisas que se quebram.

No entanto, o ASF é um projeto de código aberto e é preciso informar que eu não sou o único atrás de tudo o que você vê aqui. Nós temos alguns **[outros](https://github.com/JustArchiNET?q=ASF-)** projetos do ASF que estão sendo desenvolvidos quase exclusivamente por outros desenvolvedores. Até mesmo o núcleo do ASF tem muitos **[contribuidores](https://github.com/JustArchiNET/ArchiSteamFarm/graphs/contributors)** que me ajudaram a tornar isso possível. On top of that, there are several third-party services supporting ASF development, especially **[GitHub](https://github.com)**, **[JetBrains](https://www.jetbrains.com)** and **[Crowdin](https://crowdin.com)**. Também não podemos esquecer das incríveis bibliotecas e ferramentas que tornaram o ASF possível, tais como o **[Rider](https://www.jetbrains.com/rider)** que usamos como IDE (adoramos as adições do **[ReSharper](https://www.jetbrains.com/resharper)**)e especialmente o **[SteamKit2](https://github.com/SteamRE/SteamKit)** sem o qual náo haveria a menor chance do ASF existir. O ASF também não estaria aqui hoje sem meus **[patrocinadores](https://github.com/sponsors/JustArchi)**, **[patrons](https://www.patreon.com/JustArchi)** e vários outros doadores, que me ajudam com tudo o que faço aqui.

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

### How do I verify that the downloaded files are genuine?

As part of our releases on GitHub, we utilize a very similar verification process as the one used by **[Debian](https://www.debian.org/CD/verify)**. In every official release starting with ASF V5.1.3.3, in addition to `zip` files you can find `SHA512SUMS` and `SHA512SUMS.sign` files. Download them for verification purposes.

Firstly, you should use `SHA512SUMS` file in order to verify that `SHA-512` checksum of the selected `zip` files matches the one we calculated ourselves. On Linux, you can use `sha512sum` utility for that purpose.


```
$ sha512sum -c --ignore-missing SHA512SUMS
ASF-linux-x64.zip: OK
```

On Windows, we can do that from powershell, although you have to manually verify with `SHA512SUMS`:

```
PS > Get-Content SHA512SUMS
b8d54f7b82823650632cbaf323b5619e264b24c98f815d5b6ccb4095f51708221717e1b07542f3676a28853571f7b634c7071eadd9c3eb1dc902f64dee66a241  ASF-generic-netf.zip
07e0b4e6a73d6c62b6b516588148e9787dba66ec221ab07e14ab43f43172ae85da858eefb5b66c06b5f7320b34f6c6b96435de6df3aaf437239a6a48faad61ae  ASF-generic.zip
de1c105252efc65d428edd7d1fb696bfaae719fd79d75c5c21fd2d56d0a7e5c62e45d818d75fad0c06f9b17cfb392b3d13a2af58b8c9f83fe1db98e325b4e4f1  ASF-linux-arm.zip
d97276a68db32cab8b33c1552141fcf057d913b36de7db1c0b393be888a9528b88f0f958153924d8434a518715a5de7500e0bde846a7ea54e26ee3724c119b6f  ASF-linux-arm64.zip
f605e573cc5e044dd6fadbc44f6643829d11360a2c6e4915b0c0b8f5227bc2a257568a014d3a2c0612fa73907641d0cea455138d2e5a97186a0b417abad45ed9  ASF-linux-x64.zip
eee87dd072d0c63ca13e374ae55fec76ae0ab9aab95deb403945a8d35b9bb5be34362eb64c3b75c27cbc6f4df3a17a5ef3e0169a7038b6bb284288b39e7dec65  ASF-osx-x64.zip
7152fdaf715147fee5c4f675e62b9c34c2787f93bca7fd4f9e6e12a59b75e6e1caba7b3641f24248a58eefa5ed3fdbb79d89572061118e09ea8161c17b7923e1  ASF-win-x64.zip

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

### Eu iniciei o programa no dia 1 de abril e o idioma do ASF mudou para algo estranho, o que está acontecendo?

PARABENZ POR DISHCOBRIRR NOSS EASTR EGG DO JIA PRIMEIR DO ABRIL! Se você não definiu a opção `CurrentCulture` personalizada, então o ASF iniciado no dia 1 de abril usará o idioma **[LOLcat](https://en.wikipedia.org/wiki/Lolcat)** em vez do idioma do seu sistema. Se você quiser desativar esse comportamento, você pode simplesmente configurar `CurrentCulture` para seu idioma preferido. Também é interessante notar que você pode ativar nosso easter egg incondicionalmente configurando a opção `CurrentCulture` com o valor `qps-Ploc`.