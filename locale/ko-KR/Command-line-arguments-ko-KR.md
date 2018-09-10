# 명령줄 인자

ASF는 프로그램 실행에 영향을 미칠 수 있는 여러 명령줄 인자에 대한 지원을 포함합니다. 이것은 프로그램이 어떻게 동작해야하는지 특정하기위한 고급사용자가 이용할 수 있습니다. `ASF.json` 설정 파일의 기본 방식과 비교하면, 명령줄 인자는 `--path` 등 주요 초기설정, `--system-required` 등 플랫폼 특화 설정, `--cryptkey` 등 민감한 데이터에 사용합니다.

* * *

## 사용법

사용중인 OS와 ASF 취향에 따라 사용법이 다릅니다.

일반:

```shell
dotnet ArchiSteamFarm.dll --인자1 --인자2
```

윈도우:

```powershell
.\ArchiSteamFarm.exe --인자1 --인자2
```

리눅스/OS X:

```shell
./ArchiSteamFarm --인자1 --인자2
```

명령줄 인자는 `ArchiSteamFarm.cmd`나 `ArchiSteamFarm.sh` 같은 일반 도우미 스크립트에서도 지원합니다. 그외에 도우미 스크립트를 사용할때 **[도커](https://github.com/JustArchi/ArchiSteamFarm/wiki/Docker-ko-KR#command-line-arguments)** 항목에 명시된 것 처럼 `ASF_ARGS` 환경변수를 사용할 수 있습니다.

인자에 공백이 들어간다면 따옴표로 표시하는 것을 잊지마십시오. 아래 두개는 잘못되었습니다:

```shell
./ArchiSteamFarm --path /home/archi/My Downloads/ASF # Bad!
./ArchiSteamFarm --path=/home/archi/My Downloads/ASF # Bad!
```

하지만, 다음 두개는 완전히 정상입니다.

```shell
./ArchiSteamFarm --path "/home/archi/My Downloads/ASF" # OK
./ArchiSteamFarm "--path=/home/archi/My Downloads/ASF" # OK
```

## 인자

`--cryptkey <key>` or `--cryptkey=<key>` - will start ASF with custom cryptographic key of `<key>` value. This option affects **[security](https://github.com/JustArchi/ArchiSteamFarm/wiki/Security)** and will cause ASF to use your custom provided `<key>` key instead of default one hardcoded into the executable. Keep in mind that passwords encrypted with this key will require it to be passed on each ASF run.

* * *

`--no-restart` - this switch is mainly used by our **[docker](https://github.com/JustArchi/ArchiSteamFarm/wiki/Docker)** containers and forces `AutoRestart` of `false`. Unless you have a particular need, you should instead configure `AutoRestart` property directly in your config. This switch is here so our docker script won't need to touch your global config in order to adapt it to its own environment. Of course, if you're running ASF inside a script, you might also make use of this switch (otherwise you're better with global config property).

* * *

`--path <path>` or `--path=<path>` - ASF always navigates to its own directory on startup. By specifying this argument, ASF will navigate to given directory after initialization, which allows you to use custom path for `config` directory without a need of duplicating binary in the same place. It might come especially useful if you'd like to separate binary from actual config, as it's done in Linux-like packaging - this way you can use one (up-to-date) binary with several different setups. The path can be either relative according to current place of ASF binary, or absolute. When running multiple instances of the same binary, keep in mind that you should typically disable auto-updates, as there is no synchronization between them. Also keep in mind that this command points to new "ASF home" - the directory that has the same structure as original ASF, with `config` directory inside.

예시:

```shell
dotnet /opt/ASF/ArchiSteamFarm.dll --path /opt/TargetDirectory # Absolute path
dotnet /opt/ASF/ArchiSteamFarm.dll --path ../TargetDirectory # Relative path works as well
```

    ├── /opt
    │     ├── ASF
    │     │     ├── ArchiSteamFarm.dll
    │     │     └── ...
    │     └── TargetDirectory
    │           └── config
    └── ...
    

* * *

`--process-required` - declaring this switch will disable default ASF behaviour of shutting down when no bots are running. No auto-shutdown behaviour is especially useful in combination with **[IPC](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC)** where majority of users would expect their web service to be running regardless of the amount of bots that are enabled. If you're using IPC option or otherwise need ASF process to be running all the time until you close it yourself, this is the right option.

If you do not intend to run IPC, this option will be rather useless for you, as you can just start the process again when needed (as opposed to ASF's web server where you require it listening all the time in order to send commands).

* * *

`--system-required` - declaring this switch will cause ASF to try signalizing the OS that the process requires system to be up and running for its entire lifetime. Currently this switch has effect only on Windows machines where it'll forbid your system from going into sleep mode as long as the process is running. This can be proven especially useful when idling on your PC or laptop during night, as ASF will be able to keep your system awake while it's idling, then, once ASF is done, it'll shutdown itself like usual, making your system allowed to enter into sleep mode again, therefore saving power immediately once idling is finished.

Keep in mind that for proper auto-shutdown of ASF you need other settings - especially avoiding `--process-required` and ensuring that all your bots are following `ShutdownOnFarmingFinished`. Of course, auto-shutdown is only a possibility for this feature, not a requirement, since you can also use this flag together with e.g. `--process-required`, effectively making your system awake infinitely after starting ASF.