# 컴파일

컴파일은 실행가능한 파일을 만드는 과정입니다. 만약 ASF에 자신이 만든 변경사항을 추가하고 싶거나 공식 **[릴리즈](https://github.com/JustArchiNET/ArchiSteamFarm/releases-ko-KR)**에서 제공하는 실행파일을 무슨 이유에서건 신뢰하지 않는 경우 하게 됩니다. 만약 당신이 개발자가 아니라 사용자라면 대부분 이미 컴파일된 바이너리를 사용하길 원할겁니다. 하지만 자신만의 바이너리를 사용하고 싶거나 뭔가 새로운 것을 배우고 싶다면 계속 읽으시기 바랍니다.

ASF는 필요로 하는 도구를 모두 가지고만 있다면 현재 지원되는 어떠한 플랫폼에서도 컴파일 될 수 있습니다.

* * *

## .NET Core SDK

ASF를 컴파일하려면 플랫폼과 상관없이 런타임뿐아니라 전체 .NET Core SDK가 필요합니다. **[.NET Core 설치 페이지(영문)](https://www.microsoft.com/net/download)**에서 설치 방법을 찾을 수 있습니다. 당신의 OS에 맞는 .NET Core SDK 버전을 설치해야 합니다. 성공적인 설치 후에 `dotnet` 명령이 작동하고 실행되어야 합니다. `dotnet --info` 로 작동 여부를 확인할 수 있습니다. 또한 .NET Core SDK가 ASF의 **[런타임 요구사항](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-ko-KR#runtime-requirements)**과 일치하는지 확인하시기 바랍니다.

윈도우의 `dotnet --info` 예시입니다:

    .NET Core SDK (reflecting any global.json):
     Version:   2.1.300
     Commit:    adab45bf0c
    
    Runtime Environment:
     OS Name:     Windows
     OS Version:  10.0.17134
     OS Platform: Windows
     RID:         win10-x64
     Base Path:   C:\Program Files\dotnet\sdk\2.1.300\
    
    Host (useful for support):
      Version: 2.1.0
      Commit:  caa7b7e2ba
    
    .NET Core SDKs installed:
      2.1.300 [C:\Program Files\dotnet\sdk]
    
    .NET Core runtimes installed:
      Microsoft.AspNetCore.All 2.1.0 [C:\Program Files\dotnet\shared\Microsoft.AspNetCore.All]
      Microsoft.AspNetCore.App 2.1.0 [C:\Program Files\dotnet\shared\Microsoft.AspNetCore.App]
      Microsoft.NETCore.App 2.1.0 [C:\Program Files\dotnet\shared\Microsoft.NETCore.App]
    

* * *

## 컴파일

적절한 .NET Core SDK 버전이 실행되고 있다고 가정하고, ASF 디렉토리로 이동해서 다음을 실행합니다:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "netcoreapp2.1" -o "out/generic" "/p:LinkDuringPublish=false"
```

리눅스나 OS X를 사용한다면, 대신 `cc.sh` 스크립트를 사용할 수도 있습니다. 이는 좀 더 복잡한 방식이지만 동일하게 동작합니다.

컴파일이 성공적으로 끝나면 `ArchiSteamFarm/out/generic` 디렉토리에 `소스` 맛으로 된 ASF가 있습니다. 이것은 공식 `일반` ASF 빌드도 동일하지만, `UpdateChannel`과 `UpdatePeriod`이 `0` 값으로 되어있다는 점만 다릅니다.

### OS 특화

특정한 필요가 있다면 OS 특화 .NET Core 패키지를 생성할 수도 있습니다. 일반적으로 처음에 컴파일에 사용한 기 설치된 .NET Core 런타임으로 실행 가능한 `일반` 맛을 방금 컴파일 했기 때문에 그럴일은 없습니다만, 만일 필요하다면 다음과 같이 입력하십시오.

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "netcoreapp2.1" -o "out/linux-x64" -r "linux-x64" "/p:CrossGenDuringPublish=false"
```

물론 대상으로 하는 OS 아키텍쳐에 따라 `linux-x64`를 `win-x64` 등으로 변경하십시오. 이 빌드도 업데이트가 비활성화됩니다.

### .NET Framework

드문 경우지만 `generic-netf` 패키지를 빌드하려는 경우 대상 프레임워크를 `netcoreapp2.1`에서 `net472`로 변경할 수 있습니다. `netf` 변수를 컴파일하려면 .NET Core SDK 뿐 아니라 적절한 **[.NET Framework](https://www.microsoft.com/net/download/visual-studio-sdks)** 개발자 팩이 필요함을 명심하십시오.

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net472" -o "out/generic-netf"
```

더 드문 경우지만 .NET Framework나 심지어 .NET Core SDK도 설치할 수 없다면 `msbuild`를 직접 호출할 수 있습니다.(`linux-x86`에서 `mono`를 사용하여 빌드한 경우 등)

```shell
msbuild /m /p:Configuration=Release /p:PublishDir=out/generic-netf /p:TargetFramework=net472 /r /t:Publish ArchiSteamFarm
```

* * *

## 개발

ASF 코드를 편집하고 싶다면, 아무 .NET Core 호환 IDE나 사용할 수 있습니다. 옵션이긴 하지만 메모장으로 편집하고 위에서 설명한 `dotnet` 명령으로 컴파일 할 수도 있습니다. 하지만 윈도우의 경우 **[최신버전의 Visual Studio](https://www.visualstudio.com/downloads)**를 권장합니다. (무료 커뮤니티 버전이면 충분합니다) 또한 **[ReSharper](https://www.jetbrains.com/resharper)**를 같이 사용하는 것을 권장하지만 이것은 무료 제품은 아닙니다.

리눅스나 OS X에서 ASF 코드 작업을 하고 싶다면 **[최신 버전의 Visual Studio Code](https://code.visualstudio.com/download)**를 추천합니다. It's not as rich as classic Visual Studio, but it's good enough.

Of course all suggestions above are only recommendations, you can use whatever you want to, it comes down to `dotnet build` command anyway. We use Visual Studio + ReSharper for ASF development, with a small part of third-party `tools` that you can find in the repo.

* * *

## Tags

`master` branch is not guaranteed to be in a state that allows successful compilation or flawless ASF execution in the first place, since it's development branch just like stated in our **[release cycle](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle)**. If you want to compile ASF from source, then you should use appropriate **[tag](https://github.com/JustArchiNET/ArchiSteamFarm/tags)** for that purpose, which guarantees at least successful compilation, and very likely also flawless execution (if build was marked as stable release). In order to check the current "health" of the tree, you can use our CIs - **[AppVeyor](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)** or **[Travis](https://travis-ci.com/JustArchiNET/ArchiSteamFarm)**.

* * *

## 공식 릴리즈

공식 ASF 릴리즈는 윈도우에서 ASF의 **[런타임 요구사항](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-ko-KR#runtime-requirements)**과 일치하는 최신 .NET Core SDK를 사용하여 **[AppVeyor](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)** 로 컴파일됩니다. 테스트를 통과하면 모든 패키지는 GitHub에 배포됩니다. This also guarantees transparency, since AppVeyor always uses official public source for all builds, and you can compare checksums of AppVeyor artifacts with GitHub assets. ASF developers do not compile or publish builds manually, except for private development process, including debugging.