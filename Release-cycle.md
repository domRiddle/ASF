# Release cycle

***

ASF uses common C# versioning which is A.B.C.D. Version is being incremented after releasing current one on **[GitHub](https://github.com/JustArchi/ArchiSteamFarm/releases)**. Every single version released so far is available on GitHub, in frozen state, and will not disappear with time, therefore it's always possible to roll back to any given version, without a need of making self copies.

In general ASF versioning is very simple - we use numbers from 0 to 9 in place of A, B, C and D. Version bump increments D, if new D would result in number 10, we increment C instead and set D back to 0, and so on. In addition to that, ASF releases are not being released in any timely manner - release on GitHub is supposed to be treated as ASF milestone, a version with given feature set implemented and ready for tests/usage, while not causing any regressions compared to previous one.

ASF releases are divided into two categories - stable releases and pre-releases.

Stable release is supposed to be tested and without any known regressions (at time of release) compared to previous stable version. We tend to release stable version either after a longer period of pre-release tests, or as a version that fixes bugs of previous stable release, without introducing new ones.

Pre-releases are more often and usually introduce work-in-progress changes, suggestions or new implementations. Pre-release is not guaranteed to be stable, although I'm always trying to do some bare tests before pushing it to GitHub, so it should never be a version that is completely broken in terms of practical usage. The main point of pre-releases is to get feedback from more advanced users and catch newly introduced bugs (if any) before they hit stable release. The quality of that work highly depends on number of testers, bugs being reported on GitHub and general feedback. Pre-releases should **usually** work as good as stable releases, and the only difference between those two is simply a fact of being tested by more users. This is because ASF is a rolling project, which means that it should be possible to build and use at **any** given point of time, and versioning is made for your convienience and as set of milestones of changes between one version and another.

In general though, ASF releases are being released when they're ready, which results in non-predictable release schedule. Usually there is a pre-release at the end of any major feature or change being done, and a stable release if no bugs are found after some time.

The precise changelog that compares one version to another is always available on GitHub - through commits. In release we tend to document only changes we consider important between last stable and current release.

ASF project is powered by continuous integration process and tested by two independent services - **[AppVeyor](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)** which tests ASF on Windows via native .NET, and **[Travis](https://travis-ci.org/JustArchi/ArchiSteamFarm)** which tests ASF on Linux via Mono (latest and weekly). Every build is supposed to be reproduciable, therefore it should not be a problem to grab source (included in release) of given version and compile yourself receiving the same result as the one available through a binary.