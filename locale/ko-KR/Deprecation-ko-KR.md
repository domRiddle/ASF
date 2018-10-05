# 폐기(Deprecation)

ASF V3.1.2.2 버전부터 개발과 사용이 더욱 일관되도록 일관된 폐기(deprecation) 정책을 따르고 있습니다.

* * *

## 폐기란?

폐기는 이전에 사용되던 옵션, 인자, 기능이나 사용되지 않는 사용례를 변경하는 작거나 큰 변화의 과정입니다. Deprecation usually means that given thing was simply rewritten into another (similar) form, and you should ensure in timely manner that you'll make appropriate switch to it. In this case, it's simply moving given functionality to more appropriate place.

ASF changes rapidly and always strikes for becoming better. This sadly means that we might change or move some existing functionality into another segment of the program in order for it to benefit from new features, compatibility or stability. Thanks to that we don't need to stick with obsolete or simply painfully wrong development decisions that we made years ago. We're always trying to provide reasonable replacement that fits expected usage of previously-available functionality, which is why deprecation is mostly harmless and requires small fixes to previous usage.

* * *

## 폐기의 단계

ASF는 폐기를 2단계로 하여 번역을 더욱 쉽고 문제가 덜 생기도록 합니다.

### 1단계

1단계는 기존에 있던 기능을 폐기할때로, 다른 해결책이 즉시 필요합니다.(혹은 그 기능을 다시 도입할 계획이 없다면 해결책은 필요없습니다.)

이 단계에서, 폐기된 기능이 사용되고 있다면 ASF는 적절한 경고를 표시합니다. 가능한한 ASF는 예전의 행위를 흉내내고 호환성을 가지려고 합니다. ASF는 적어도 다음 안정버전까지는 그 기능에 대해 1단계를 유지할 것입니다. 이 때가 호환성을 깨지 않고 모든 도구와 패턴을 새 행위에 맞게 적절하게 전환할 수 있는 순간입니다. 폐기 경고가 안보인다면 모든 적절한 조치를 취한 것입니다.

### 2단계

2단계는 위에서 설명한 1단계가 일어난 후 안정화 버전이 배포되는 때입니다. This stage introduces complete removal of deprecated feature existence, which means that ASF will not even acknowledge that you're attempting to use a deprecated feature, let alone respect it, since it simply doesn't exist in the current code. ASF will no longer print any warning, since it no longer recognizes what you're attempting to do.

* * *

## 요약

You have more or less a **full month** in order to make appropriate switch, which should be more than enough even if you're a casual ASF user. After that period, ASF no longer guarantees that old settings will have any effect (stage 2), effectively making certain features to stop functioning altogether without you noticing. If you're launching ASF after more than a month of inactivity, it's recommended for you to **[start from scratch](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up)** again, or read all the changelogs that you've missed and manually adapt your usage to current one.

In most cases, disregarding deprecation warning will not render general ASF functionality unusable, but rather falling back to default behaviour (which might or might not match your personal preferences).

* * *

## 예시

We moved pre-V3.1.2.2 `--server` **[command-line argument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments)** into `IPC` **[global configuration property](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#global-config)**.

### 1단계

Stage 1 happened in version V3.1.2.2 where we added appropriate warning to usage of `--server`. Now-obsolete `--server` argument was automatically mapped into `IPC: true` global config property, effectively acting exactly the same as old `--server` switch for time being. This allowed everybody to do appropriate switch before ASF stops accepting old argument.

### 2단계

Stage 2 happened in version V3.1.3.0, right after V3.1.2.9 stable with stage 1 explained above. Stage 2 caused ASF to stop recognizing the `--server` argument at all, treating it like every other invalid argument being passed, which no longer has any effect on the program. For people that still didn't change their usage of `--server` into `IPC: true`, it caused IPC to stop functioning altogether, as ASF no longer did appropriate mapping.