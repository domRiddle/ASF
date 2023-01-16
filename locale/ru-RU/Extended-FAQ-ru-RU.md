# Расширенное ЧАВО

Наш расширенный ЧаВО даёт ответы на менее распространённые вопросы, которые у вас могут возникнуть. Для получения ответов на более распространённые вопросы, вы можете посетить наш **[основной ЧаВО](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ-ru-RU)**.

---

### Кто создал ASF?

ASF был создан **[Archi](https://github.com/JustArchi)** в октябре 2015. Если Вам интересно, то я такой же **[пользователь Steam](https://steamcommunity.com/profiles/76561198006963719)** как и Вы. Помимо игр, я также люблю применять свои умения и целеустремлённость, и результаты вы можете увидеть прямо здесь. В разработке ASF не принимали участие крупные компании, или команда разработчиков, и не было бюджета в 1 млн. долларов - только я один, чинил вещи которые не сломаны.

Однако ASF - проект с открытым исходным кодом, и я не могу сказать что всё здесь сделано только мной. У нас есть несколько **[других](https://github.com/JustArchiNET?q=ASF-)** проектов связанных с ASF, работа над которыми ведётся практически исключительно другими разработчиками. Даже у основного проекта ASF есть много **[соавторов](https://github.com/JustArchiNET/ArchiSteamFarm/graphs/contributors)**, которые помогали мне сделать всё это возможным. Кроме того, существует несколько сторонних сервисов, поддерживающих разработку ASF, особенно **[GitHub](https://github.com)**, **[JetBrains](https://www.jetbrains.com)** и **[Crowdin](https://crowdin.com)**. Вы также не можете забыть обо всех потрясающих библиотеках и инструментах, благодаря которым появился ASF, таких как **[Rider](https://www.jetbrains.com/rider)**, который мы используем в качестве IDE (мы любим дополнения **[ReSharper](https://www.jetbrains.com/resharper)**) и особенно <2 >SteamKit2</a></strong>, без которого ASF вообще не существовал бы. ASF also wouldn't be where it is today without my **[sponsors](https://github.com/sponsors/JustArchi)** and various donators, supporting me in everything that I'm doing here.

Спасибо вам всем за помощь в разработке ASF! You're awesome :heart:.

---

### Почему изначально был создан ASF?

Основной целью создания ASF было получить полностью автоматический инструмент для фарма карточек в Steam под Linux, без необходимости использовать внешние зависимости (такие как клиент Steam). Фактически, это всё ещё остаётся его основной целью, поскольку моя концепция ASF с тех пор не менялась, и я всё ещё следую ей так же как и в 2015 году. Разумеется, с тех пор было сделано **много** изменений, и я рад видеть как развился ASF, в основном благодаря его пользователям, потому что я не стал бы писать и половины имеющихся сейчас функций если бы делал всё только для себя.

Приятно отметить, что ASF никогда не стремился соперничать с другими, похожими программами, в особенности с **[Idle Master](https://www.steamidlemaster.com)**, поскольку ASF никогда не планировался как приложение для использования на домашнем ПК, и это так по сей день. Если вы проанализируете основную цель ASF, описанную выше, вы поймёте что Idle master — **полная противоположность** этому. Хотя вы почти наверняка сейчас найдёте программы для фарма, похожие на ASF, на момент начала разработки не было ничего, что бы меня устраивало (и нет по сей день), поэтому я создал свою собственную программу для фарма такой, как мне хотелось. С течением времени пользователи переходили на ASF в основном из-за надёжности, стабильности и безопасности, а также из-за всех функций которые я разработал в течении всех этих лет. Сегодня ASF лучше, чем когда-либо.

---

### Хорошо, а в чём подвох? Что вы получаете распространяя ASF?

Нет никакого подвоха, я создал ASF **для себя** и поделился им с остальным сообществом в надежде, что он окажется полезным. То же самое случилось в 1991 году, когда Линус Торвальдс **[поделился своим первым ядром Linux](https://groups.google.com/forum/#!msg/comp.os.Minix/dlNtH7RRrGA/SwRavCzVE7gJ)** с остальным миром. В нём нет скрытых вредоносных программ, сбора данных, майнинга криптовалют или других функций, которые принесут мне денежную выгоду. Проект ASF поддерживается исключительно необязательными пожертвованиями от довольных пользователей, таких же, как вы. Вы можете использовать ASF точно так же как его использую я, и если он вам нравится - вы всегда можете купить мне чашечку кофе, чтобы показать свою благодарность за то, что я делаю.

Также я использую ASF как идеальный пример современного проекта на C#, который всегда стремится к совершенству и лучшим практикам, будь то касательно технологий, управления проектами или самого кода. Это моё определение того, как "делать всё правильно", поэтому если вы сможете чему-то научиться из моего проекта - это только сделает меня счастливее.

---

### Right after launching ASF I've lost all my accounts/items/friends/(...)!

Statistically speaking, regardless how sad it is, it's guaranteed that shortly after launching ASF there will be at least one guy who will die in a car accident. The difference is that nobody sane will blame ASF for causing it, but for some reason there are people who will accuse ASF of the same just because it happened to their Steam accounts instead. Of course we can understand the reasoning for that, after all ASF operates within Steam platform, so naturally people will accuse ASF of everything that happened to their Steam-related property regardless of lack of any evidence that the software they ran is even remotely connected with that whatsoever.

ASF, as stated in **[FAQ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ#is-it-safe)** as well as **[question above](#ok-where-is-the-catch-what-do-you-gain-from-sharing-asf)**, is free of malware, spyware, data mining and any other potentially unwanted activity, **especially** submission of your sensitive Steam details or taking over your digital property. Если с вами случилось что-то подобное, единственное что бы можем, это сказать, что сожалеем о вашей потере и рекомендуем связаться с **[поддержкой Steam](https://help.steampowered.com)**, которая, как мы надеемся, поможет вам в восстановлении, потому что мы не несем ответственности за то, что произошло с вами, и наша совесть чиста. If you believe otherwise, that's your decision, it's pointless to elaborate further, if the above resources providing objective and verifiable ways to confirm our statement didn't convince you, then it's not like anything we write here will anyway.

However, the above doesn't mean that **your actions** done without a common sense with ASF can't contribute to a security issue. For example, you could disregard our security guidelines, expose ASF's **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** interface to the whole internet, and then be surprised that somebody got in and robbed you out of all items. People do it all the time, they think that if there is no domain or any connection to their IP address then nobody will for sure find out their ASF instance. Right as you read it, there are **thousands** if not more fully-automated bots crawling through the web, including random IP addresses, searching for vulnerabilities to discover, and ASF as a quite popular program is also a target of those. We already had enough of people that got "hacked" through their own stupidity like that, so try to learn from their mistakes and be smarter instead of joining them.

Same goes for security of your PC. Yes, having malware on your PC ruins every single security aspect of ASF, as it can read sensitive details from ASF config files or process memory and even influence the program to do stuff that it wouldn't do otherwise. No, the last crack you've obtained from doubtful source was not a "false positive" as somebody has told you, it's one of the most effective ways to gain control over somebody's PC, the guy will infect himself and he'll even follow the instructions how to, fascinating.

Is using ASF completely safe and free of all risks then? No, we'd be bunch of hypocrites stating so, as **every** software has its security-oriented problems. Contrary to what a lot of companies are doing, we're trying to be as transparent as possible in our **[security advisories](https://github.com/JustArchiNET/ArchiSteamFarm/security/advisories)** and as soon as we find out even a *hypothetical* situation where ASF could contribute in any way to a potentially unwanted from security perspective situation, we announce it immediately. This is what happened with **[CVE-2021-32794](https://github.com/JustArchiNET/ArchiSteamFarm/security/advisories/GHSA-wxx4-66c2-vj2v)** for example, even though ASF didn't have any security flaw per-se, but rather a bug that could lead to user accidentally creating one.

As of today, there are no known, unpatched security flaws in ASF, and as the program is used by more and more people out of which both **[white hats](https://en.wikipedia.org/wiki/White_hat_(computer_security))** as well as **[black hats](https://en.wikipedia.org/wiki/Black_hat_(computer_security))** analyze its source code, the overall trust factor only increases with time, as the number of security flaws to find out is finite, and ASF as a program that focuses first and foremost on its security, definitely isn't making it easy for finding one. Regardless of our best intentions, we still recommend to stay cool-headed and always be wary of potential security threats, ones coming from ASF usage as well.

---

### Как проверить подлинность загруженных файлов?

В рамках наших выпусков на GitHub мы используем процесс проверки, очень похожий на тот, который используется в **[Debian](https://www.debian.org/CD/verify)**. В каждом официальном выпуске, начиная с ASF V5.1.3.3, помимо файлов `zip` вы можете найти файлы `SHA512SUMS` и `SHA512SUMS.sign`. Загрузите их для проверки вместе с файлами `zip` по вашему выбору.

Во-первых, вы должны использовать файл `SHA512SUMS`, чтобы проверить, что контрольная сумма `SHA-512` выбранных файлов `zip` совпадает с той, которую мы вычислили сами. В Linux для этой цели можно использовать утилиту `sha512sum`.


```
$ sha512sum -c --ignore-missing SHA512SUMS
ASF-linux-x64.zip: OK
```

В Windows мы можем сделать это из PowerShell, правда вам придется вручную проверить с помощью `SHA512SUMS`:

```
PS > Get-Content SHA512SUMS | Select-String -Pattern ASF-linux-x64.zip

f605e573cc5e044dd6fadbc44f6643829d11360a2c6e4915b0c0b8f5227bc2a257568a014d3a2c0612fa73907641d0cea455138d2e5a97186a0b417abad45ed9  ASF-linux-x64.zip


PS > Get-FileHash -Algorithm SHA512 -Path ASF-linux-x64.zip

Algorithm       Hash                                                                   Path
---------       ----                                                                   ----
SHA512          F605E573CC5E044DD6FADBC44F6643829D11360A2C6E4915B0C0B8F5227BC2A2575... ASF-linux-x64.zip
```

Таким образом мы гарантируем, что все, что было записано в `SHA512SUMS`, соответствует скачанным файлам, и что они не были изменены. Однако это еще не доказывает, что файл `SHA512SUMS`, который вы проверили, действительно исходит от нас. Для этого мы будем использовать файл `SHA512SUMS.sign`, который содержит цифровую подпись PGP, подтверждающую подлинность `SHA512SUMS`. Для этой цели мы можем использовать утилиту `gpg` как в **[Linux](https://gnupg.org/download/index.html)**, так и в **[Windows](https://gpg4win.org)** (замените команду `gpg` на `gpg.exe` в Windows).

```
$ gpg --verify SHA512SUMS.sign SHA512SUMS
gpg: Signature made Mon 02 Aug 2021 00:34:18 CEST
gpg:                using EDDSA key 224DA6DB47A3935BDCC3BE17A3D181DF2D554CCF
gpg: Can't check signature: No public key
```

Как видите, файл действительно содержит действительную подпись, но неизвестного происхождения. Вам нужно будет импортировать **[открытый ключ](https://raw.githubusercontent.com/JustArchi-ArchiBot/JustArchi-ArchiBot/main/ArchiBot_public.asc)** ArchiBot, которым мы подписываем суммы `SHA-512` для полной проверки.

```
$ curl https://raw.githubusercontent.com/JustArchi-ArchiBot/JustArchi-ArchiBot/main/ArchiBot_public.asc -o ArchiBot_public.asc
$ gpg --import ArchiBot_public.asc
gpg: /home/archi/.gnupg/trustdb.gpg: trustdb created
gpg: key A3D181DF2D554CCF: public key "ArchiBot <ArchiBot@JustArchi.net>" imported
gpg: Total number processed: 1
gpg:               imported: 1

```

Наконец, вы можете снова проверить файл `SHA512SUMS`:

```
$ gpg --verify SHA512SUMS.sign SHA512SUMS
gpg: Signature made Mon 02 Aug 2021 00:34:18 CEST
gpg:                using EDDSA key 224DA6DB47A3935BDCC3BE17A3D181DF2D554CCF
gpg: Good signature from "ArchiBot <ArchiBot@JustArchi.net>" [unknown]
gpg: WARNING: This key is not certified with a trusted signature!
gpg:          There is no indication that the signature belongs to the owner.
Primary key fingerprint: 224D A6DB 47A3 935B DCC3  BE17 A3D1 81DF 2D55 4CCF
```

Это подтверждает, что `SHA512SUMS.sign` содержит действительную подпись нашего ключа `224DA6DB47A3935BDCC3BE17A3D181DF2D554CCF` для файла `SHA512SUMS`, по которому вы проверяли.

Вам может быть интересно, откуда пришло последнее предупреждение. Вы успешно импортировали наш ключ, но пока не сделали его доверенным. Хотя это не обязательно, мы также можем это исправить. Обычно это включает проверку по другому каналу (например, телефонный звонок, SMS), что ключ действителен, а затем подписание ключа своим собственным, чтобы доверять ему. В этом примере вы можете рассматривать эту вики-запись как такой (очень слабый) другой канал, поскольку исходный ключ берется из профиля **[ArchiBot](https://github.com/JustArchi-ArchiBot)**. В любом случае будем считать, что уверенности у вас и так достаточно.

Во-первых, **[сгенерируйте для себя закрытый ключ](https://help.ubuntu.com/community/GnuPrivacyGuardHowto#Generating_an_OpenPGP_Key)**, если у вас его еще нет. В качестве простого примера мы воспользуемся `--quick-gen-key`.

```
$ gpg --batch --passphrase '' --quick-gen-key "$(whoami)"
gpg: /home/archi/.gnupg/trustdb.gpg: trustdb created
gpg: key E4E763905FAD148B marked as ultimately trusted
gpg: directory '/home/archi/.gnupg/openpgp-revocs.d' created
gpg: revocation certificate stored as '/home/archi/.gnupg/openpgp-revocs.d/8E5D685F423A584569686675E4E763905FAD148B.rev'
```

Теперь вы можете подписать наш ключ своим, чтобы доверять ему:

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

И готово, доверяя вашему ключу, `gpg` больше не должен отображать предупреждение при проверке:

```
$ gpg --verify SHA512SUMS.sign SHA512SUMS
gpg: Signature made Mon 02 Aug 2021 00:34:18 CEST
gpg:                using EDDSA key 224DA6DB47A3935BDCC3BE17A3D181DF2D554CCF
gpg: Good signature from "ArchiBot <ArchiBot@JustArchi.net>" [full]
```

Обратите внимание, что индикатор доверия `[unknown]` меняется на `[full]`, когда вы подписываете наш ключ своим.

Поздравляем, вы убедились, что никто не вмешивался в скачанный вами выпуск! 👍

---

### Сегодня 1 апреля, и язык ASF изменился на что-то странное, что происходит?

CONGRATULASHUNS ON DISCOVERIN R APRIL FOOLS EASTR EGG! (ПОЗДРАВЛЯЕТСЯ НА СКОРВЕРИНЕ Р ФОРМЫ ЗАРАБОТКИ EGG!) Если вы не установили собственный параметр `CurrentCulture`, то 1 апреля ASF фактически будет использовать язык **[LOLcat](https://ru.wikipedia.org/wiki/Lolcat)** вместо языка вашей системы. Если вы вдруг хотите отключить это поведение, вы можете просто установить `CurrentCulture` на языковой стандарт, который вы хотите использовать вместо этого. Также приятно отметить, что вы можете включить наше пасхальное яйцо без каких-либо условий, установив для вашего `CurrentCulture` значение `qps-Ploc`.