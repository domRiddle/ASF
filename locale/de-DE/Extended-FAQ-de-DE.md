# Erweiterte FAQ

In unserem erweiterten FAQ finden Sie Antworten auf die etwas weniger häufig gestellten Fragen die Sie eventuell haben. Für häufiger gestellte Fragen besuchen Sie bitte stattdessen unser **[FAQ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ-de-DE)**.

---

### Wer hat ASF erschaffen?

ASF wurde von **[Archi](https://github.com/JustArchi)** im Oktober 2015 erschaffen. Falls Sie sich wundern- ich bin ein **[Steam-Benutzer](https://steamcommunity.com/profiles/76561198006963719)** genau wie Sie. Neben dem Spielen stelle ich auch gerne meine Fähigkeiten und meine Entschlossenheit zur Verfügung, wie Sie in Kürze feststellen können. Hier ist weder eine große Firma, noch ein Team von Entwicklern noch ein Budget von einer Millionen-Dollar verfügbar um das alles abzudecken - Nur ich, der Dinge repariert, die noch nicht kaputt sind.

ASF ist jedoch ein Open-Source-Projekt, und ich kann nicht genug ausdrücken, dass ich nicht hinter allem stehe, was man hier sehen kann. Wir haben ein paar **[andere](https://github.com/JustArchiNET?q=ASF-)** ASF-Projekte, die fast ausschließlich von anderen Entwicklern entwickelt werden. Sogar das ASF-Kernprojekt hat eine Vielzahl von **[Mitwirkenden](https://github.com/JustArchiNET/ArchiSteamFarm/graphs/contributors)**, die mir geholfen haben, all dies zu erreichen. Darüber hinaus gibt es mehrere Drittanbieter-Dienste, welche die ASF-Entwicklung unterstützen, insbesondere **[GitHub](https://github.com)**, **[JetBrains](https://www.jetbrains.com)**, **[AppVeyor](https://www.appveyor.com)** und **[Crowdin](https://crowdin.com)**. Und natürlich darf man nicht all die fantastischen Bibliotheken und Programme vergessen die ASF überhaupt erst ermöglicht haben. Insbesondere **[Rider](https://www.jetbrains.com/rider)**, welches wir als IDE verwenden und wir lieben die **[ReSharper](https://www.jetbrains.com/resharper)**-Erweiterungen und besonders **[SteamKit2](https://github.com/SteamRE/SteamKit)** (ohne dessen ASF überhaupt nicht existieren würde). ASF wäre auch nicht dort, wo es heute ist, ohne dass meine **[Sponsoren](https://github.com/sponsors/JustArchi)**, **[Patreons](https://www.patreon.com/JustArchi)** und diverse Spender mich in allem, was ich hier tue, unterstützen.

Vielen Dank an alle, die bei der ASF-Entwicklung geholfen haben! Ihr seid großartig!

---

### Warum wurde ASF in erster Linie erstellt?

ASF was created with primary purpose of being fully automated Steam farming tool for Linux, without a need of any external dependencies (such as Steam client). Tatsächlich bleibt dies immer noch der Hauptzweck und Fokus, denn mein Konzept von ASF hat sich seitdem nicht geändert und ich benutze es immer noch genauso, wie ich es bereits 2015 benutzt habe. Natürlich gab es seitdem wirklich **viele** Änderungen, und ich bin sehr froh zu sehen wie weit ASF gekommen ist, vor allem dank seiner Benutzer, denn ich würde nie auch nur die Hälfte der Features programmieren wenn es für mich selbst wäre.

It's nice to note that ASF was never made to compete with other, similar programs, especially **[Idle Master](https://www.steamidlemaster.com)**, because ASF was never designed to be a desktop/user app, and it still isn't today. Wenn du den Hauptzweck von ASF, wie oben beschrieben, analysierst, dann wirst du sehen wie Idle master **das genaue Gegenteil** von all dem ist. While you can most definitely find similar to ASF programs today, nothing was good enough for me back then (and still isn't today), so I created my own software, the way I wanted it. Benutzer sind im Laufe der Zeit hauptsächlich aufgrund von Robustheit, Stabilität und Sicherheit auf ASF umgestiegen aber auch alle Funktionen, die ich in all diesen Jahren entwickelt habe. Heute ist ASF besser als je zuvor.

---

### Okay, wo ist der Haken? Was nützt Ihnen das Teilen von ASF?

Es gibt keinen Haken. Ich habe ASF **für mich selbst** erstellt und es mit dem Rest der Community geteilt in der Hoffnung, dass es nützlich sein wird. Genau das Gleiche geschah 1991, als Linus Torvalds **[seinen ersten Linux-Kernel](https://groups.google.com/forum/#!msg/comp.os.Minix/dlNtH7RRrGA/SwRavCzVE7gJ)** mit dem Rest der Welt teilte. Es gibt keine versteckte Malware, Data-Mining, Crypto-Mining oder andere Aktivitäten, die mir einen finanziellen Nutzen bringen würden. ASF Projekt wird vollständig durch nicht-obligatorische Spenden unterstützt, die von glücklichen Benutzern wie Ihnen gesendet werden. Sie können ASF genauso verwenden wie ich es benutze und wenn Sie es mögen, Sie können mir immer einen Kaffee kaufen, der Ihre Dankbarkeit für das, was ich tue, zeigt.

Ich benutze auch ASF als perfektes Beispiel für ein modernes C# Projekt, das immer für Perfektion und das bestmögliche Verfahren sorgt, sei es mit Technologie, Projektmanagement oder dem Code selbst. Es ist meine Definition von "Sachen richtig angehen/erledigen". Wenn Sie durch irgenIhren Zufall etwas Nützliches aus meinem Projekt lernen, dann wird das mich nur glücklicher machen.

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

### Ich habe das Programm am 1. April gestartet und die ASF-Sprache wechselte in etwas seltsames, was ist hier los?

CONGRATULASHUNS ZUR ENTDECKUNG UNSERES APRILSCHERZES! Sofern Sie `CurrentCulture` nicht definiert haben, verwendet ASF nach dem Start am 1. April die Lokalisation **[LOLcat](https://en.wikipedia.org/wiki/Lolcat)** anstatt der Systemsprache. Wenn Sie dieses Verhalten deaktivieren möchten, können Sie einfach `CurrentCulture` auf die Lokalisation setzen, das Sie stattdessen verwenden möchten. Schön ist auch, dass Sie unseren Aprilscherz bedingungslos aktivieren können, indem Sie Ihren `CurrentCulture`-Wert auf `qps-Ploc` setzen.