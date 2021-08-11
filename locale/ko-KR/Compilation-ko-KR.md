# 컴파일

컴파일은 실행가능한 파일을 만드는 과정입니다. 만약 ASF에 자신이 만든 변경사항을 추가하고 싶거나 공식 **[릴리스](https://github.com/JustArchiNET/ArchiSteamFarm/releases-ko-KR)** 에서 제공하는 실행파일을 무슨 이유에서건 신뢰하지 않는 경우 하게 됩니다. 만약 당신이 개발자가 아니라 사용자라면 대부분 이미 컴파일된 바이너리를 사용하길 원할겁니다. 하지만 자신만의 바이너리를 사용하고 싶거나 뭔가 새로운 것을 배우고 싶다면 계속 읽으시기 바랍니다.

ASF는 필요로 하는 도구를 모두 가지고만 있다면 현재 지원되는 어떠한 플랫폼에서도 컴파일 될 수 있습니다.

---

## .NET SDK

Regardless of platform, you need full .NET SDK (not just runtime) in order to compile ASF. Installation instructions can be found on **[.NET download page](https://dotnet.microsoft.com/download)**. You need to install appropriate .NET SDK version for your OS. 성공적인 설치 후에 `dotnet` 명령이 작동하고 실행되어야 합니다. `dotnet --info` 로 작동 여부를 확인할 수 있습니다. Also ensure that your .NET SDK matches ASF **[runtime requirements](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)**.

---

## 컴파일

Assuming you have .NET SDK operative and in appropriate version, simply navigate to source ASF directory (cloned or downloaded and unpacked ASF repo) and execute:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net5.0" -o "out/generic"
```

리눅스나 OS X를 사용한다면, 대신 `cc.sh` 스크립트를 사용할 수도 있습니다. 이는 좀 더 복잡한 방식이지만 동일하게 동작합니다.

컴파일이 성공적으로 끝나면 `out/generic` 디렉토리에 `소스` 맛으로 된 ASF가 있습니다. 이것은 공식 `일반` ASF 빌드도 동일하지만, 자체 빌드에 적합하도록 `UpdateChannel`과 `UpdatePeriod`이 `0` 값으로 되어있다는 점만 다릅니다.

### OS 특화

You can also generate OS-specific .NET package if you have a specific need. In general you shouldn't do that because you've just compiled `generic` flavour that you can run with your already-installed .NET runtime that you've used for the compilation in the first place, but just in case you want to:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net5.0" -o "out/linux-x64" -r "linux-x64"
```

물론 대상으로 하는 OS 아키텍쳐에 따라 `linux-x64`를 `win-x64` 등으로 변경하십시오. 이 빌드도 업데이트가 비활성화됩니다.

### .NET Framework

In a very rare case when you'd want to build `generic-netf` package, you can change target framework from `net5.0` to `net48`. Keep in mind that you'll need appropriate **[.NET Framework](https://dotnet.microsoft.com/download/visual-studio-sdks)** developer pack for compiling `netf` variant, in addition to .NET SDK, so the below will work only on Windows:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net48" -o "out/generic-netf"
```

In case of being unable to install .NET Framework or even .NET SDK itself (e.g. because of building on `linux-x86` with `mono`), you can call `msbuild` directly. You'll also need to specify `ASFNetFramework` manually, as ASF by default disables `netf` build on non-Windows platforms:

```shell
msbuild /m /r /t:Publish /p:Configuration=Release /p:TargetFramework=net48 /p:PublishDir=out/generic-netf /p:ASFNetFramework=true ArchiSteamFarm
```

---

## 개발

If you'd like to edit ASF code, you can use any .NET compatible IDE for that purpose, although even that is optional, since you can as well edit with a notepad and compile with `dotnet` command described above. 하지만 윈도우의 경우 **[최신버전의 Visual Studio](https://visualstudio.microsoft.com/downloads)**를 권장합니다. (무료 커뮤니티 버전이면 충분합니다)

리눅스나 OS X에서 ASF 코드 작업을 하고 싶다면 **[최신 버전의 Visual Studio Code](https://code.visualstudio.com/download)**를 추천합니다. 고전 Visual Studio만큼 풍족하진 않지만, 충분히 좋습니다.

물론 위의 모든 제안은 단지 권장사항일 뿐이고, 당신은 원하는 모든 것을 사용할 수 있지만, 결국 `dotnet build` 명령으로 귀결됩니다. We use **[JetBrains Rider](https://www.jetbrains.com/rider)** for ASF development, although it's not a free solution.

---

## 태그

`main` branch is not guaranteed to be in a state that allows successful compilation or flawless ASF execution in the first place, since it's development branch just like stated in our **[release cycle](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle)**. ASF를 소스에서 컴파일하거나 참조하려면 목적에 맞는 적절한 **[태그](https://github.com/JustArchiNET/ArchiSteamFarm/tags)** 를 사용해야 합니다. 이는 최소한 성공적인 컴파일을 보장하고, 안정 릴리스로 표시된 빌드는 거의 흠없는 실행도 가능합니다. In order to check the current "health" of the tree, you can use our CI - **[GitHub](https://github.com/JustArchiNET/ArchiSteamFarm/actions/workflows/ci.yml?query=branch%3Amain)**.

---

## 공식 릴리스

Official ASF releases are compiled by **[GitHub](https://github.com/JustArchiNET/ArchiSteamFarm/actions)** on Windows, with latest .NET SDK that matches ASF **[runtime requirements](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)**. After passing tests, all packages are deployed as the release, also on GitHub. This also guarantees transparency, since GitHub always uses official public source for all builds, and you can compare checksums of GitHub artifacts with GitHub release assets. ASF 개발자는 개인 개발과정과 디버깅을 제외하고는 스스로 컴파일하거나 빌드를 게시하지 않습니다.