# EXTENDD FAQ

R EXTENDD FAQ COVERS BIT LES COMMON QUESHUNS AN ANZWERS DAT U CUD HAS. 4 MOAR COMMON MATTERS, PLZ VISIT R **[BASIC FAQ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ-lol-US)** INSTEAD.

---

### HOO HAS CREATD ASF?

ASF WUZ CREATD BY **[ARCHI](https://github.com/JustArchi)** IN OCTOBR 2015. IN CASE U WUZ WONDERIN, IM **[STEAM USR](https://steamcommunity.com/profiles/76561198006963719)** JUS LIEK U. APART FRUM PLAYIN GAMEZ, I ALSO LUV 2 PUT MAH SKILLS AN DETERMINASHUN 2 USE, WHICH U CAN EXPLORE RITE NAO. THAR IZ NO HOOJ COMPANY INVOLVD HER, NO TEAM OV DEVELOPERS AN NO $1M OV BUDGET 2 COVR ALL OV DAT - JUS ME, FIXIN THINGS DAT R NOT BROKD.

HOWEVR, ASF IZ OPEN-SOURCE PROJECT, AN I CANT EXPRES ENOUGH DAT IM NOT BEHIND EVRYTHIN DAT U CAN C HER. WE HAS FEW **[OTHR](https://github.com/JustArchiNET?q=ASF-)** ASF PROJECTS DAT R BEAN DEVELOPD ALMOST EXCLUSIVELY BY OTHR DEVELOPERS. EVEN CORE ASF PROJECT HAS LOT OV **[CONTRIBUTORS](https://github.com/JustArchiNET/ArchiSteamFarm/graphs/contributors)** DAT HELPD ME 2 MAK ALL OV DIS HAPPEN. On top of that, there are several third-party services supporting ASF development, especially **[GitHub](https://github.com)**, **[JetBrains](https://www.jetbrains.com)** and **[Crowdin](https://crowdin.com)**. U ALSO CANT FORGET BOUT ALL TEH AWSUM LIBRARIEZ AN TOOLS DAT MADE ASF HAPPEN, SUCH AS **[RIDR](https://www.jetbrains.com/rider)** DAT WE USE AS IDE (WE LUV **[RESHARPR](https://www.jetbrains.com/resharper)** ADDISHUNS) AN ESPECIALLY **[STEAMKIT2](https://github.com/SteamRE/SteamKit)** WITHOUT WHICH ASF WUD NOT EXIST IN DA FURST PLACE. ASF ALSO WOULDNT BE WER IT TODAI WITHOUT MAH **[SPONSORS](https://github.com/sponsors/JustArchi)**, **[PATRONS](https://www.patreon.com/JustArchi)** AN VARIOUS DONATORS, SUPPORTIN ME IN EVRYTHIN DAT IM DOIN HER.

THANK U ALL 4 HELPIN IN ASF DEVELOPMENT! URE AWSUM.

---

### Y WUZ ASF CREATD IN DA FURST PLACE?

ASF WUZ CREATD WIF PRIMARY PURPOSE OV BEAN FULLY AUTOMATD STEAM FARMIN TOOL 4 LINUX, WITHOUT NED OV ANY EXTERNAL DEPENDENCIEZ (SUCH AS STEAM CLIENT). IN FACT, DIS STILL REMAINS ITZ PRIMARY PURPOSE AN FOCUS, CUZ MAH CONCEPT OV ASF DIDNT CHANGE SINCE DEN AN IM STILL USIN IT IN EGSAKTLY TEH SAME WAI AS I USD IT BAK IN 2015. OV COURSE, THAR WUZ RLY **A LOT** OV CHANGEZ SINCE DEN, AN IM VRY HAPPEH 2 C HOW FAR ASF HAS PROGRESD, MOSTLY THX 2 ITZ USERS, CUZ ID NEVR CODE EVEN HALF OV TEH FEATUREZ IF IT WUZ 4 MYSELF ONLY.

IZ NICE 2 NOWT DAT ASF WUZ NEVR MADE 2 COMPETE WIF OTHR, SIMILAR PROGRAMS, ESPECIALLY **[IDLE MASTAH](https://www.steamidlemaster.com)**, CUZ ASF WUZ NEVR DESIGND 2 BE DESKTOP/USR APP, AN IT STILL ISNT TODAI. IF U ANALYZE PRIMARY PURPOSE OV ASF DESCRIBD ABOOV, DEN ULL C HOW IDLE MASTAH IZ **TEH EGSAKT OPPOSIET** OV ALL OV DAT. WHILE U CAN MOST DEFINITELY FIND SIMILAR 2 ASF PROGRAMS TODAI, NOTHIN WUZ GUD ENOUGH 4 ME BAK DEN (AN STILL ISNT TODAI), SO I CREATD MAH OWN SOFTWARE, TEH WAI I WANTD IT. OVAR TIEM USERS HAS MIGRATD 2 ASF MAINLY DUE 2 ROBUSTNES, STABILITY AN SECURITY, BUT ALSO ALL TEH FEATUREZ DAT I HAS DEVELOPD ACROS ALL DOSE YEERS. TODAI, ASF IZ BETTR THAN EVR BEFORE.

---

### K, WER IZ TEH KATCH? WUT DO U GAIN FRUM SHARIN ASF?

THAR IZ NO KATCH, I CREATD ASF **4 MYSELF** AN SHARD IT WIF TEH REST OV TEH COMMUNITY IN HOPE DAT ITLL COME USEFUL. EGSAKTLY TEH SAME TING HAPPEND BAK IN 1991, WHEN LINUS TORVALDZ **[SHARD HIS FURST LINUX KERNEL](https://groups.google.com/forum/#!msg/comp.os.Minix/dlNtH7RRrGA/SwRavCzVE7gJ)** WIF TEH REST OV TEH WURLD. THAR IZ NO HIDDEN MALWARE, DATA MININ, CRYPTO MININ OR ANY OTHR ACTIVITY DAT WUD GENERATE ANY MONETARY BENEFIT 4 ME. ASF PROJECT IZ SUPPORTD ENTIRELY BY NON-OBLIGATORY DONASHUNS SENT BY HAPPEH USERS SUCH AS U. U CAN USE ASF IN EGSAKTLY TEH SAME WAI HOW IM USIN IT, AN IF U LIEK IT, U CAN ALWAYS BUY ME COFFEH, SHOWIN UR GRATITUDE 4 WUT IM DOIN.

IM ALSO USIN ASF AS PERFIK EXAMPLE OV MODERN C# PROJECT DAT ALWAYS STRIKEZ 4 PERFECSHUN AN BEST PRACTICEZ, BE IT WIF TECHNOLOGY, PROJECT MANAGEMENT OR TEH CODE ITSELF. IZ MAH DEFINISHUN OV "THINGS DUN RITE", SO IF BY ANY CHANCE U MANAGE 2 LERN SOMETHIN USEFUL FRUM MAH PROJECT, DEN DAT WILL MAK ME ONLY MOAR HAPPEH.

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

### I HAS LAUNCHD TEH PROGRAM ON APRIL TEH 1ST AN TEH ASF LANGUAGE CHANGD INTO SOMETHIN STRANGE, WUT IZ GOIN ON?

CONGRATULASHUNS ON DISCOVERIN R APRIL FOOLS EASTR EGG! IF U DIDNT SET CUSTOM `CurrentCulture` OPSHUN, DEN ASF LAUNCHD ON APRIL TEH 1ST WILL AKSHULLY USE **[LOLCAT](https://en.wikipedia.org/wiki/Lolcat)** LANGUAGE INSTEAD OV UR SISTEM LANGUAGE. IF BY ANY CHANCE UD LIEK 2 DISABLE DAT BEHAVIOUR, U CAN SIMPLY SET `CurrentCulture` 2 TEH LOCALE DAT UD LIEK 2 USE INSTEAD. IZ ALSO NICE 2 NOWT DAT U CAN ENABLE R EASTR EGG UNCONDISHUNALLY, BY SETTIN UR `CurrentCulture` 2 `qps-Ploc` VALUE.