# 常見問題擴展

我們的常見問題擴展涵蓋了您可能遇到的一些不常見的問題和答案。 有關更常見的事項，請訪問我們的**[常見問題基礎](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ)**。

---

### 誰創造了 ASF？

ASF于2015年10月由 **[Archi](https://github.com/JustArchi)** 創造。 如果您好奇的話，我和您一樣，是個**[Steam用戶](https://steamcommunity.com/profiles/76561198006963719)**。 如您所見，除了玩遊戲，我也喜歡將我的技能和決心付諸行動，歡迎探索我的作品！ 這裏沒有大公司參與，沒有開發商團隊，也沒有100萬美元的預算來支援，只有我自己，在維護並優化這些項目。

然而，ASF是開源項目，您在這裏看到的一切並非完全出自我手。 我們有一些 **[其他](https://github.com/JustArchiNET?q=ASF-)** ASF項目，它們幾乎完全由其他開發人員開發。 即使是核心 ASF 項目，也有很多 **[貢獻者](https://github.com/JustArchiNET/ArchiSteamFarm/graphs/contributors)** 幫助我實現。 On top of that, there are several third-party services supporting ASF development, especially **[GitHub](https://github.com)**, **[JetBrains](https://www.jetbrains.com)** and **[Crowdin](https://crowdin.com)**. You also can't forget about all the awesome libraries and tools that made ASF happen, such as **[Rider](https://www.jetbrains.com/rider)** that we use as IDE (we love **[ReSharper](https://www.jetbrains.com/resharper)** additions) and especially **[SteamKit2](https://github.com/SteamRE/SteamKit)** without which ASF would not exist in the first place. ASF also wouldn't be where it is today without my **[sponsors](https://github.com/sponsors/JustArchi)**, **[patrons](https://www.patreon.com/JustArchi)** and various donators, supporting me in everything that I'm doing here.

感謝大家協助ASF開發！ You're awesome.

---

### 創造ASF的初心？

ASF was created with primary purpose of being fully automated Steam farming tool for Linux, without a need of any external dependencies (such as Steam client). 時至今日，這仍然是ASF的主要目的和重點，因為我的ASF理念從未改變，我使用它的方式仍然和2015年完全一樣。 當然，自那時以來，確實發生了**很多** 的變化， 我很高興地看到ASF已經取得了巨大的進展，這要感謝它的用戶們，因為如果它僅由我使用，我可能甚至不會寫出當前一半的功能。

It's nice to note that ASF was never made to compete with other, similar programs, especially **[Idle Master](https://www.steamidlemaster.com)**, because ASF was never designed to be a desktop/user app, and it still isn't today. 如果您分析上述ASF的主要用途，您會發現Idle Master與此**截然相反**。 While you can most definitely find similar to ASF programs today, nothing was good enough for me back then (and still isn't today), so I created my own software, the way I wanted it. Over time users have migrated to ASF mainly due to robustness, stability and security, but also all the features that I've developed across all those years. Today, ASF is better than ever before.

---

### 好吧，問我有何訴求？ What do you gain from sharing ASF?

There is no catch, I created ASF **for myself** and shared it with the rest of the community in hope that it'll come useful. 這樣的事情層出不窮，回溯至1991年，Linus Torvalds向全世界**[分享了他的第一個Linux kernel](https://groups.google.com/forum/#!msg/comp.os.Minix/dlNtH7RRrGA/SwRavCzVE7gJ)**。 There is no hidden malware, data mining, crypto mining or any other activity that would generate any monetary benefit for me. ASF project is supported entirely by non-obligatory donations sent by happy users such as you. You can use ASF in exactly the same way how I'm using it, and if you like it, you can always buy me a coffee, showing your gratitude for what I'm doing.

I'm also using ASF as a perfect example of a modern C# project that always strikes for perfection and best practices, be it with technology, project management or the code itself. It's my definition of "things done right", so if by any chance you manage to learn something useful from my project, then that will make me only more happy.

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

CONGRATULASHUNS ON DISCOVERIN R APRIL FOOLS EASTR EGG! If you didn't set custom `CurrentCulture` option, then ASF on April the 1st will actually use **[LOLcat](https://en.wikipedia.org/wiki/Lolcat)** language instead of your system language. If by any chance you'd like to disable that behaviour, you can simply set `CurrentCulture` to the locale that you'd like to use instead. It's also nice to note that you can enable our easter egg unconditionally, by setting your `CurrentCulture` to `qps-Ploc` value.