# 릴리스 주기

ASF 는 A.B.C.D. 버전으로 된 일반적인 C# 버전관리를 사용합니다. 이는 **[GitHub](https://github.com/JustArchiNET/ArchiSteamFarm/releases)**의 현재 버전이 릴리스 된 후 하나씩 증가합니다. 현재까지의 모든 버전은 얼려진 상태로 GitHub에 있고 시간이 지나도 사라지지 않습니다. 따라서 자가 복제를 할 필요 없이 그중 어느 것으로라도 항상 롤백이 가능합니다.

일반적으로 ASF 버전관리는 매우 단순합니다. A, B, C, D의 자리에 0에서 9의 숫자를 사용합니다. 버전이 올라가면 D자리 숫자가 하나씩 증가합니다. D의 숫자가 10이 된다면 C를 하나 증가시키고 D는 0으로 만듭니다. 이를 반복합니다. 새 버전의 릴리스는 ASF 마일스톤으로 취급합니다. 이는 이전 버전과 비교하여 퇴보한 것이 없고, 테스트와 이용 준비가 된 기능의 집합이 구현된 버전을 뜻합니다. 가끔 매우 중요한 변경사항을 소개하기로 결정하고 C나 B, 심지어 A를 증가시킬 때도 있습니다. 이는 매우 드물고 보통 몇몇 주요 변경사항이 있는 경우입니다.

ASF의 릴리스는 두개의 범주로 나뉩니다. 안정 릴리스와 사전 릴리스입니다.

안정 릴리스는 제대로 동작하고 릴리스 시점에서 이전 버전과 비교하여 퇴보한 사항이 없습니다. 오랜 기간동안 사전 릴리스 테스트를 한 후에 안정 버전을 릴리스 하거나, 새로운 버그 없이 이전 안정 릴리스의 버그를 수정하는 버전을 릴리스하는 경향이 있습니다. 매우 드문 경우지만 Steam 주요 변경사항 등의 경우 필요하다면 가능한한 빨리 새로운 안정 버전을 릴리스하기로 결정하기도 합니다. 하지만 일반적으로 그러한 버전도 보통 매우 잘 작동해야 합니다. 우리는 이전 안정 릴리스에 비해 전반적 건강상태가 나빠진다면 안정으로 표시하지 않습니다. 물론 그 "전반적 건강"은 사전 릴리스 개발기간 동안 받는 보고와 피드백에 기초합니다. 단순히 개발단계에서 우리중 누구도 오류를 발생하지 않는다면 안정 릴리스 이후에 버그가 새어나와 발견될 수도 있습니다. 다행히도 그런 일이 일어나는 것은 매우 드물고 후속 안정 릴리스에서 가능한한 빨리 수정하려고 합니다.

사전 릴리스는 더 자주 있고 보통 개발중인 변경사항이나 제안, 새로운 구현을 소개합니다. 사전 릴리스가 안정을 보장하지는 않습니다. GitHub에 올리기 전에 테스트하려 노력하므로 실제 사용 측면에서 완전히 깨진 버전은 절대 아닙니다. 사전 릴리스의 주안점은 더욱 고급 사용자로부터 피드백을 받고 안정 릴리스에서 나오기 전에 새로 생긴 버그를 잡는 것입니다. 이 작업의 품질은 테스터와 GitHub에 보고된 버그, 그리고 일반 피드백의 숫자에 달려있습니다.

사전 릴리스는 **보통** 안정 릴리스처럼 잘 동작해야 하며, 유일한 차이점은 더 많은 사용자가 테스트 했는지 여부입니다. 이는 ASF가 단계적 프로젝트이기 때문입니다. 즉 **어느** 시점에서라도 빌드하고 사용할 수 있어야 한다는 것이며, 버전관리는 한 버전과 다른 버전의 변경사항의 마일스톤으로서 편의에 따라 정해집니다. 사전 릴리스를 사용하기로 결정했다면 당신은 조금 더 고급 사용자여야 합니다. 사전 릴리스는 보통 개발중인 작은 ASF 마일스톤이기 때문이고, 뭔가 잘 작동할 때에도 작동하거나 테스트되지 않은 것들이 있을 가능성이 있습니다. GitHub에서 ASF 개발사항을 추적하고 변경사항을 주의깊게 읽는 것은 사전 릴리스를 사용하려면 자신을 위해 반드시 해야할 최소한입니다. 게다가 우리가 환경설정 변경, 새로 작성한 코드, 코드 변화 등 뭔가 특정한 것을 적극적으로 테스트하는 때가 있습니다. 이 경우 항상 변경사항을 읽으십시오. 이러한 사전 릴리스는 다른 것 보다 훨씬 불안정할 수 있습니다.

새로 도입되는 기능과 변경은 시간이 지난 후에도 위키 등에 문서화되지 않을 수도 있음을 양지하십시오. 작업중인 기능을 수정하기로 결정할 때마다 문서를 다시 작성하는 시간을 절약하기 위해 문서는 해당 기능의 최종 코드가 준비된 후에 작성됩니다. 사전 릴리스가 최종 형태가 아닌 개발중인 코드를 포함할 수 있다는 사실 때문에 문서화는 개발의 마지막 단계에서 이루어질 수 있습니다. 일반 변경사항도 동일하게 적용되어 사전 릴리스의 변경사항이 시간이 지난 후에도 없을 수 있습니다. 따라서 사전 릴리스를 사용하기로 결정했다면 ASF **[커밋](https://github.com/JustArchiNET/ArchiSteamFarm/commits/master)**을 시시때때로 들여다볼 준비를 하십시오.

물론, 문서가 없다는 것은 **오직** 사전 릴리스에만 해당합니다. 모든 안정 버전은 릴리스되는 때에 완전한 변경사항과 위키 문서를 가지고 있습니다.

사전 릴리스 버전은 시간이 지나면 안정 버전이 될 수도 있습니다. 그 사이에 변화가 없다면, 그리고 안정 릴리스로 올라갈 버전에 초점이 없다면 특히 맞는 이야기입니다. 사전 릴리스가 "안정 릴리스 후보"로 간주될때 매우 자주 안정버전이 됩니다. 안정적으로 표시되기 전에 고급 사용자들이 테스트할수 있게 하여 버그가 나올 위험을 낮춥니다. 따라서 이것이 ASF 릴리스의 가장 일반적 패턴입니다.

    안정 1.0 -> 사전 1.1 -> 사전 1.2 -> ... -> 사전 1.7 (RC)(안정 후보) -> 안정 1.7 (사전 1.7과 동일)
    

하지만 일반적으로 ASF 릴리스는 준비가 되면 릴리스되므로 릴리스 일정은 예측가능하지 않습니다. Usually there is a pre-release at the end of any major feature or change being done, and a stable release if no bugs are found after some time (a few days) since pre-release became available. We're aiming for more or less **one stable release per month**, unless there are some critical issues to deal with or likewise. Pre-releases are happening on as-needed basis when we feel like there is enough of stuff that needs to be tested since the release of the last one. Depending on how busy ASF development is in given moment, this can be from a few to a dozen of pre-releases between each stable release.

The precise changelog that compares one version to another is always available on GitHub - through commits and code changes. In release we tend to document only changes we consider important between last stable and current release. Such brief changelog is never a complete one, so if you'd like to see every change that happened between one version and another - please use GitHub for that.

ASF project is powered by continuous integration process and tested by two independent services - **[AppVeyor](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)** which tests ASF on Windows, and **[Travis](https://travis-ci.com/JustArchiNET/ArchiSteamFarm)** which tests ASF on Linux and OS X. Every build is supposed to be reproducible, therefore it should not be a problem to grab source (included in release) of given version and compile yourself receiving the same result as the one available through a binary.

As an extra, in addition to ASF stable releases and pre-releases, you can also find latest automated AppVeyor build **[here](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)** which is typically built from latest not-yet-included-anywhere commit. Due to automation and lack of any tests, those builds are **NOT** supported in any way, and typically are available only for developers that need to grab most recent ASF GitHub snapshot in object form, so they won't need to compile themselves. Nothing guarantees that ASF in `master` state will work properly. In rare cases, those builds can also be used for verifying if given not-yet-released commit indeed fixed particular issue or bug, without waiting for the change to land in pre-release. If you decide to use those automated builds, please ensure that you have decent knowledge in terms of ASF, as it's the highest "level" of bugged software. Unless you have a strong reason, usually you're already on the bleeding edge by tracking pre-release builds - AppVeyor builds are offered as an extra to pre-releases, mainly for verifying if given commit fixed particular issue we're working on right now. Those builds should not be used in any production environment.