# 补充常见问题

“补充常见问题”涵盖了一些您可能想了解的不太常见的问题以及它们的答案。 对于更常见的问题，请访问&#8203;**[基本常见问题](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ-zh-CN)**。

---

### 谁创作了 ASF？

ASF 是由 **[Archi](https://github.com/JustArchi)** 在 2015 年 10 月创作的。 您可能已经知道，我和您一样，是一名 **[Steam 用户](https://steamcommunity.com/profiles/76561198006963719)**。 正如您所见，除了玩游戏，我也很愿意将我的技能与意志付诸实践。 这里没有大公司参与，没有开发者团队，也没有一百万美元的预算——只有我自己维护这个项目。

然而，ASF 是一个开源项目，您在这里看到的一切并非完全出自我手。 我们有一些&#8203;**[其他](https://github.com/JustArchiNET?q=ASF-)** ASF 项目，几乎全部由其他开发者开发。 即使是 ASF 核心，也有许多&#8203;**[贡献者](https://github.com/JustArchiNET/ArchiSteamFarm/graphs/contributors)**&#8203;参与帮助我完成。 最重要的是，有一些第三方服务支持了 ASF 的开发，特别是 **[GitHub](https://github.com)**、**[JetBrains](https://www.jetbrains.com)** 与 **[Crowdin](https://crowdin.com)**。 当然也不能忘记实现 ASF 所用的优秀的库和工具，例如我们使用的 IDE **[Rider](https://www.jetbrains.com/rider)**（我们还特别喜爱 **[ReSharper](https://www.jetbrains.com/resharper)** 插件），特别是 **[SteamKit2](https://github.com/SteamRE/SteamKit)**，没有这个库，ASF 也就不复存在。 同样，如果没有 **[GitHub](https://github.com/sponsors/JustArchi)**、**[Patreons](https://github.com/sponsors/JustArchi)** 或其他渠道的支持者为我们捐款，ASF 也不会像现在这样优秀。

感谢所有帮助 ASF 开发的人！ 你们太棒啦！

---

### 起初为什么要创作 ASF？

创作 ASF 的主要目标是成为 Linux 的全自动 Steam 挂机工具，无需任何外部依赖（例如 Steam 客户端）。 事实上，这仍然是它的最主要目标，因为我的 ASF 概念从那以后没有改变，我现在使用它的方式仍然与我在 2015 年的使用方式完全相同。 当然，自那时起，ASF 已经发生了**相当多**的变化，我很高兴看到 ASF 取得了如此大的进展，这主要得益于我们的用户，如果我只是为自己开发，ASF 恐怕连现在的一半功能都没有。

需要指出，ASF 从未试图与其他相似工具竞争，特别是 **[Idle Master](https://www.steamidlemaster.com)**，因为 ASF 从来没有被设计为一个桌面/面向用户的应用，现在亦然。 如果您仔细分析上述的 ASF 主要目标，您就会发现 Idle Master 处于**完全相反**的方向。 虽然您现在肯定能找到类似于 ASF 的工具，但对我来说，当时还没有合适的（目前仍然没有），所以我按照我想要的方式创造了自己的软件。 随着时间的推移，用户迁移到 ASF 的主要原因是其强健、稳定和安全的特性，但也有我这些年开发的所有特性的功劳。 如今，ASF 是史上最棒的 ASF。

---

### 那么用户需要付出什么？ 分享 ASF 为你带来了什么利益？

您不需要付出代价，我**为自己**创作了 ASF，并将其分享给社区，希望对其他人有用。 1991 年发生过同样的事情，Linus Torvalds 将他的&#8203;**[第一份 Linux 内核](https://groups.google.com/forum/#!msg/comp.os.Minix/dlNtH7RRrGA/SwRavCzVE7gJ)**&#8203;分享给了全世界。 这里没有暗藏恶意软件、数据挖掘、恶意挖矿或者其他任何能够为我产生利益的活动。 ASF 项目的支持完全来自于非强制性的捐款，这些捐款由像您一样的普通用户捐赠。 您可以像我一样使用 ASF，并且如果您喜欢它，您可以随时为我捐赠一杯咖啡来表达您对我工作的感激。

此外，我也将 ASF 视为一个现代 C# 项目的完美示例，始终追求完美和最佳实践，无论是技术、项目管理还是代码本身。 这是我对于“把事情做好”的定义，所以假如您打算从我的项目中学到一些有用东西，那只会使我更高兴。

---

### 我怎样确认下载的文件是真实的？

我们使用与 **[Debian](https://www.debian.org/CD/verify)** 非常类似的简易验证方式，以此作为 GitHub Release 的一部分。 自 ASF V5.1.3.3 的每个官方版本开始，除了 `zip` 文件以外，还包括 `SHA512SUMS` 和 `SHA512SUMS.sign` 文件。 如果需要验证，则下载这些文件。

首先，您应该使用 `SHA512SUMS` 文件来验证指定 `zip` 文件 `SHA-512` 校验和的值与我们的计算相符。 在 Linux 上，您可以使用 `sha512sum` 工具做这件事。


```
$ sha512sum -c --ignore-missing SHA512SUMS
ASF-linux-x64.zip: OK
```

在 Windows 上，我们可以在 Powershell 里实现，但您需要人工确认 `SHA512SUMS`。

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

这样我们就保证了 `SHA512SUMS` 中记录的内容与实际文件是符合的，这些文件没有被篡改。 然而，这还不能证明您使用的 `SHA512SUMS` 文件真的来自我们。 为此，我们将使用 `SHA512SUMS.sign` 文件，里面记录了能证明 `SHA512SUMS` 真实性的数字 PGP 签名。 我们可以使用 `gpg` 工具，无论是在  **[Linux](https://gnupg.org/download/index.html)** 还是 **[Windows](https://gpg4win.org)** 上（在 Windows 上需要将 `gpg` 命令换成 `gpg.exe`）。

```
$ gpg --verify SHA512SUMS.sign SHA512SUMS
gpg: Signature made Mon 02 Aug 2021 00:34:18 CEST
gpg:                using EDDSA key 224DA6DB47A3935BDCC3BE17A3D181DF2D554CCF
gpg: Can't check signature: No public key
```

您可以看到，此文件确实有一个有效的签名，但来源未知。 您需要导入我们用于签名 `SHA-512` 校验和的 ArchiBot **[公钥](https://raw.githubusercontent.com/JustArchi-ArchiBot/JustArchi-ArchiBot/main/ArchiBot_public.asc)**，来进行完整的验证。

```
$ curl https://raw.githubusercontent.com/JustArchi-ArchiBot/JustArchi-ArchiBot/main/ArchiBot_public.asc -o ArchiBot_public.asc
$ gpg --import ArchiBot_public.asc
gpg: /home/archi/.gnupg/trustdb.gpg: trustdb created
gpg: key A3D181DF2D554CCF: public key "ArchiBot <ArchiBot@JustArchi.net>" imported
gpg: Total number processed: 1
gpg:               imported: 1

```

最后，您可以再次验证 `SHA512SUMS` 文件：

```
$ gpg --verify SHA512SUMS.sign SHA512SUMS
gpg: Signature made Mon 02 Aug 2021 00:34:18 CEST
gpg:                using EDDSA key 224DA6DB47A3935BDCC3BE17A3D181DF2D554CCF
gpg: Good signature from "ArchiBot <ArchiBot@JustArchi.net>" [unknown]
gpg: WARNING: This key is not certified with a trusted signature!
gpg:          There is no indication that the signature belongs to the owner.
Primary key fingerprint: 224D A6DB 47A3 935B DCC3  BE17 A3D1 81DF 2D55 4CCF
```

这次我们验证了 `SHA512SUMS.sign` 持有我们 `224DA6DB47A3935BDCC3BE17A3D181DF2D554CCF` 密钥的有效签名，对应您已验证过的 `SHA512SUMS` 文件。

您可能想知道最后的警告是怎么来的。 您已经成功导入我们的密钥，但尚未决定信任它。 虽然这不是强制性的，但我们也可以说明这一点。 通常，您需要通过另一个沟通渠道（例如电话或短信）来验证此密钥是有效的，然后使用您自己的密钥为其签名来信任。 在此示例中，您可以认为此 Wiki 条目就是“另一个”（非常弱的）渠道，因为原始密钥来自于 **[ArchiBot 的个人资料](https://github.com/JustArchi-ArchiBot)**。 无论如何，我们假定您有足够的信心认为密钥是可信的。

首先，如果您还没有自己的私钥，则&#8203;**[生成一个](https://help.ubuntu.com/community/GnuPrivacyGuardHowto#Generating_an_OpenPGP_Key)**。 我们在此使用 `--quick-gen-key` 参数作为一个快速的示例。

```
$ gpg --batch --passphrase '' --quick-gen-key "$(whoami)"
gpg: /home/archi/.gnupg/trustdb.gpg: trustdb created
gpg: key E4E763905FAD148B marked as ultimately trusted
gpg: directory '/home/archi/.gnupg/openpgp-revocs.d' created
gpg: revocation certificate stored as '/home/archi/.gnupg/openpgp-revocs.d/8E5D685F423A584569686675E4E763905FAD148B.rev'
```

现在您可以使用您自己的密钥为我们的密钥签名，也就是信任它：

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

完成信任密钥之后，`gpg` 应该不会在验证时再显示上面的警告了：

```
$ gpg --verify SHA512SUMS.sign SHA512SUMS
gpg: Signature made Mon 02 Aug 2021 00:34:18 CEST
gpg:                using EDDSA key 224DA6DB47A3935BDCC3BE17A3D181DF2D554CCF
gpg: Good signature from "ArchiBot <ArchiBot@JustArchi.net>" [full]
```

注意在信任之后，`[unknown]` 信任状态已经变成 `[full]`。

恭喜，您已经验证了您下载的文件没有经过篡改！ 👍

---

### 我在 4 月 1 日启动了这个程序，ASF 的语言变得很奇怪，发生了什么？

喵！我们藏起来的愚人节彩蛋被你发现了喵！ 如果您没有设置自定义的 `CurrentCulture`，那么 ASF 在 4 月 1 日运行时将会使用 **[LOLcat](https://en.wikipedia.org/wiki/Lolcat)** 语言而不是系统语言。 如果您的确希望禁用此行为，您可以直接将 `CurrentCulture` 改为您要使用的语言。 实际上，您还可以强制启用这个彩蛋，只需要将 `CurrentCulture` 设置为 `qps-Ploc`。