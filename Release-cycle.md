# Release cycle

***

ASF uses common C# versioning which is A.B.C.D. Version is being incremented after releasing current one on **[GitHub](https://github.com/JustArchi/ArchiSteamFarm/releases)**. Every single version released so far is available on GitHub, in frozen state, and will not disappear with time, therefore it's always possible to roll back to any of them, without a need of making self copies.

In general ASF versioning is very simple - we use numbers from 0 to 9 in place of A, B, C and D. Version bump increments D, if new D would result in number 10, we increment C instead and set D back to 0, and so on. Release of a new version is supposed to be treated as ASF milestone, a version with given feature set implemented and ready for tests/usage, while not causing any regressions compared to previous one.

ASF releases are divided into two categories - stable releases and pre-releases.

Stable releases are supposed to be working properly and without any known regressions (at time of release) compared to previous ones. We tend to release stable version either after a longer period of pre-release tests, or as a version that fixes bugs of previous stable release, without introducing new ones. In very rare scenario (e.g. Steam breaking change) we might also decide of releasing new stable version ASAP, if needed.

Pre-releases are more often and usually introduce work-in-progress changes, suggestions or new implementations. Pre-release is not guaranteed to be stable, although we're always trying to do some bare tests before pushing it to GitHub, so it should never be a version that is completely broken in terms of practical usage. The main point of pre-releases is to get feedback from more advanced users and catch newly introduced bugs (if any) before they hit stable release. The quality of that work highly depends on number of testers, bugs being reported on GitHub and general feedback. Pre-releases should **usually** work as good as stable releases, and the only difference between those two is simply a fact of being tested by more users. This is because ASF is a rolling project, which means that it should be possible to build and use at **any** given point of time, and versioning is made for your convenience - as a milestone of changes between one version and another.

In addition to that, a pre-release version might be considered stable after some time. This is especially true if there are no changes done in the meantime, and there is no point in version bump just for the sake of stable release. It's also done very often when pre-release is considered "stable release candidate", as it allows advanced users to test it before it gets marked as stable, so the risk of introducing bugs is much lower, therefore this is the most common pattern when it comes to ASF releases:

```
Stable 0.1 -> Pre 0.2 -> Pre 0.3 -> ... -> Pre 0.7 (RC) -> Stable 0.7 (same as Pre 0.7)
```

In general though, ASF releases are being released when they're ready, which results in non-predictable release schedule. Usually there is a pre-release at the end of any major feature or change being done, and a stable release if no bugs are found after some time (a few days) since pre-release became available. Unless there are some critical issues that must be dealt with, or current ASF code includes some not-yet-ready-for-shipping code, a stable release should come at least once per month.

The precise changelog that compares one version to another is always available on GitHub - through commits and code changes. In release we tend to document only changes we consider important between last stable and current release. Such brief changelog is never a complete one, so if you'd like to see every change that happened between one version and another - please use GitHub for that.

ASF project is powered by continuous integration process and tested by two independent services - **[AppVeyor](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)** which tests ASF on Windows via native .NET, and **[Travis](https://travis-ci.org/JustArchi/ArchiSteamFarm)** which tests ASF on Linux via Mono (latest and weekly). Every build is supposed to be reproduciable, therefore it should not be a problem to grab source (included in release) of given version and compile yourself receiving the same result as the one available through a binary.