# Расширенное ЧАВО

Наш расширенный ЧаВО даёт ответы на менее распространённые вопросы, которые у вас могут возникнуть. Для получения ответов на более распространённые вопросы, вы можете посетить наш **[основной ЧаВО](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ-ru-RU)**.

---

### Кто создал ASF?

ASF был создан **[Archi](https://github.com/JustArchi)** в октябре 2015. Если Вам интересно, то я такой же **[пользователь Steam](https://steamcommunity.com/profiles/76561198006963719)** как и Вы. Помимо игр, я также люблю применять свои умения и целеустремлённость, и результаты вы можете увидеть прямо здесь. В разработке ASF не принимали участие крупные компании, или команда разработчиков, и не было бюджета в 1 млн. долларов - только я один, чинил вещи которые не сломаны.

Однако ASF - проект с открытым исходным кодом, и я не могу сказать что всё здесь сделано только мной. У нас есть несколько **[других](https://github.com/JustArchiNET?q=ASF-)** проектов связанных с ASF, работа над которыми ведётся практически исключительно другими разработчиками. Даже у основного проекта ASF есть много **[соавторов](https://github.com/JustArchiNET/ArchiSteamFarm/graphs/contributors)**, которые помогали мне сделать всё это возможным. On top of that, there are several third-party services supporting ASF development, especially **[GitHub](https://github.com)**, **[JetBrains](https://www.jetbrains.com)** and **[Crowdin](https://crowdin.com)**. You also can't forget about all the awesome libraries and tools that made ASF happen, such as **[Rider](https://www.jetbrains.com/rider)** that we use as IDE (we love **[ReSharper](https://www.jetbrains.com/resharper)** additions) and especially **[SteamKit2](https://github.com/SteamRE/SteamKit)** without which ASF would not exist in the first place. ASF не стал бы тем, чем он является сегодня, без помощи моих **[спонсоров](https://github.com/sponsors/JustArchi)**, **[патронов](https://www.patreon.com/JustArchi)** и других людей, сделавших пожертвования, поддерживавших меня во всём что я здесь делаю.

Спасибо вам всем за помощь в разработке ASF! Вы великолепны.

---

### Почему изначально был создан ASF?

ASF was created with primary purpose of being fully automated Steam farming tool for Linux, without a need of any external dependencies (such as Steam client). Фактически, это всё ещё остаётся его основной целью, поскольку моя концепция ASF с тех пор не менялась, и я всё ещё следую ей так же как и в 2015 году. Разумеется, с тех пор было сделано **много** изменений, и я рад видеть как развился ASF, в основном благодаря его пользователям, потому что я не стал бы писать и половины имеющихся сейчас функций если бы делал всё только для себя.

It's nice to note that ASF was never made to compete with other, similar programs, especially **[Idle Master](https://www.steamidlemaster.com)**, because ASF was never designed to be a desktop/user app, and it still isn't today. Если вы проанализируете основную цель ASF, описанную выше, вы поймёте что Idle master — **полная противоположность** этому. While you can most definitely find similar to ASF programs today, nothing was good enough for me back then (and still isn't today), so I created my own software, the way I wanted it. С течением времени пользователи переходили на ASF в основном из-за надёжности, стабильности и безопасности, а также из-за всех функций которые я разработал в течении всех этих лет. Сегодня ASF лучше, чем когда-либо.

---

### Хорошо, а в чём подвох? Что вы получаете распространяя ASF?

Нет никакого подвоха, я создал ASF **для себя** и поделился им с остальным сообществом в надежде, что он окажется полезным. То же самое случилось в 1991 году, когда Линус Торвальдс **[поделился своим первым ядром Linux](https://groups.google.com/forum/#!msg/comp.os.Minix/dlNtH7RRrGA/SwRavCzVE7gJ)** с остальным миром. В нём нет скрытых вредоносных программ, сбора данных, майнинга криптовалют или других функций, которые принесут мне денежную выгоду. Проект ASF поддерживается исключительно необязательными пожертвованиями от довольных пользователей, таких же, как вы. Вы можете использовать ASF точно так же как его использую я, и если он вам нравится - вы всегда можете купить мне чашечку кофе, чтобы показать свою благодарность за то, что я делаю.

Также я использую ASF как идеальный пример современного проекта на C#, который всегда стремится к совершенству и лучшим практикам, будь то касательно технологий, управления проектами или самого кода. Это моё определение того, как "делать всё правильно", поэтому если вы сможете чему-то научиться из моего проекта - это только сделает меня счастливее.

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