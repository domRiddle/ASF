# 发布周期

ASF 使用 A.B.C.D 形式的常用 C# 版本号。每当向 **[GitHub](https://github.com/JustArchiNET/ArchiSteamFarm/releases)** 发布新版本时，版本号就会增加。 到目前为止发布的每个版本都可以在 GitHub 上获得，它们总是保持发布时的状态，并且不会随着时间消失，因此用户总是可以回滚到其中任何一个版本，而无需复制一份。

一般来说，ASF 的版本控制非常简单——我们将 0 到 9 之间的数字填入 A、B、C 和 D，每次升级将会使 D 增加 1，一旦 D 满 10，就增加 C 并将 D 重设为 0. 新版本的发布应被视为 ASF 的里程碑，即一个实现了特定功能并准备好进行测试/使用的版本，同时不会导致与以前版本相比出现任何倒退。 有时候我们可能会认为引入的变化非常重要，会直接增加 C、B 甚至 A 的数值，尽管这种情况非常罕见（并且通常说明存在一些重大变化）。

ASF 的版本分为两类——稳定版和预览版。

与之前的版本相比，稳定版应该正常工作，没有任何已知的倒退（在发布时）。 我们会在较长时间的预览版测试之后发布稳定版，或者将稳定版作为修复上一个稳定版漏洞（并且不会引入新漏洞）的版本。 在极少数情况下（例如 Steam 发生重大变化），如果需要，我们也可能决定尽快发布新的稳定版本。 一般而言，这些版本应该非常适合平常使用，因为如果与先前的稳定版相比更不稳定，我们不会将此版本标记为稳定版。 当然，这种“稳定程度”是基于我们在发布前开发过程中收到的报告和反馈，遗憾的是，仍有可能一些错误会漏网，并在稳定版发布后被发现，因为没有人在开发阶段遇到此问题。 但事实上很少发生这种情况，并且即使出现，我们也会尽快发布一个新的稳定版修复。

预览版发布得更频繁，通常还会引入未完成的改变、建议或新实现。 我们不保证预览版是稳定的，但我们总是尝试在将它推送到 GitHub 之前做一些裸测试，因此预览版在实际使用方面通常不至于完全不可用。 预览版的主要目标是从高级用户那里获得反馈，并在发布稳定版之前找到新出现的漏洞（如果有的话）。 这项工作的质量很大程度上取决于测试人员、GitHub 上报告的漏洞和一般反馈的数量。

预览版版**通常**应该与稳定版一样好，这两者之间的唯一区别就是稳定版经过更多用户测试。 这是因为 ASF 是一个滚动项目，这意味着它应该可以在**任何**给定的时间点构建和使用，并且版本控制只是为了方便——作为两个版本之间变化的里程碑。 不过，如果您决定使用预览版，通常应该是更高级的用户，因为预览版通常是正在开发的较小的 ASF 里程碑，很可能即使有些功能看起来正常，但仍有可能有问题或者未通过测试——如果您仍要使用预览版，至少应该为了自己的利益，关注 ASF 在 GitHub 上的开发进程，并仔细阅读版本更新日志。 除此之外，有时我们会主动测试特定的内容，例如配置文件变更、代码重构或者核心功能的变化。 在这种情况下，您应该经常阅读更新日志，因为这样的预览版可能比其他版本更不稳定。

Please note that newly introduced features and changes might be undocumented (e.g. on the wiki) until some time later, as documentation is usually written once final code of given feature is ready (to save us time rewriting documentation each time we decide to modify the feature we're currently working on). Due to the fact that pre-release might contain work-in-progress code that doesn't have a final form yet, documentation might arrive at later stage of the development. Same thing applies to general changelog that might be unavailable for given pre-release until some time later. Therefore if you decide to use pre-release then be prepared for looking inside ASF **[commits](https://github.com/JustArchiNET/ArchiSteamFarm/commits/master)** from time to time.

Of course, lack of documentation applies **only** to pre-releases - each stable version must always have complete changelog and documentation on the wiki the moment it's being released.

A pre-release version might be considered stable after some time. This is especially true if there are no changes done in the meantime, and there is no point in version bump just for the sake of stable release. It's also done very often when pre-release is considered "stable release candidate", as it allows advanced users to test it before it gets marked as stable, so the risk of introducing bugs is much lower, therefore this is the most common pattern when it comes to ASF releases:

    Stable 1.0 -> Pre 1.1 -> Pre 1.2 -> ... -> Pre 1.7 (RC) -> Stable 1.7 (same as Pre 1.7)
    

In general though, ASF releases are being released when they're ready, which results in non-predictable release schedule. Usually there is a pre-release at the end of any major feature or change being done, and a stable release if no bugs are found after some time (a few days) since pre-release became available. We're aiming for more or less **one stable release per month**, unless there are some critical issues to deal with or likewise. Pre-releases are happening on as-needed basis when we feel like there is enough of stuff that needs to be tested since the release of the last one. Depending on how busy ASF development is in given moment, this can be from a few to a dozen of pre-releases between each stable release.

The precise changelog that compares one version to another is always available on GitHub - through commits and code changes. In release we tend to document only changes we consider important between last stable and current release. Such brief changelog is never a complete one, so if you'd like to see every change that happened between one version and another - please use GitHub for that.

ASF project is powered by continuous integration process and tested by two independent services - **[AppVeyor](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)** which tests ASF on Windows, and **[Travis](https://travis-ci.com/JustArchiNET/ArchiSteamFarm)** which tests ASF on Linux and OS X. Every build is supposed to be reproducible, therefore it should not be a problem to grab source (included in release) of given version and compile yourself receiving the same result as the one available through a binary.

As an extra, in addition to ASF stable releases and pre-releases, you can also find latest automated AppVeyor build **[here](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)** which is typically built from latest not-yet-included-anywhere commit. Due to automation and lack of any tests, those builds are **NOT** supported in any way, and typically are available only for developers that need to grab most recent ASF GitHub snapshot in object form, so they won't need to compile themselves. Nothing guarantees that ASF in `master` state will work properly. In rare cases, those builds can also be used for verifying if given not-yet-released commit indeed fixed particular issue or bug, without waiting for the change to land in pre-release. If you decide to use those automated builds, please ensure that you have decent knowledge in terms of ASF, as it's the highest "level" of bugged software. Unless you have a strong reason, usually you're already on the bleeding edge by tracking pre-release builds - AppVeyor builds are offered as an extra to pre-releases, mainly for verifying if given commit fixed particular issue we're working on right now. Those builds should not be used in any production environment.