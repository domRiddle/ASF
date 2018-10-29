# Компиляция

Компиляция - это процесс создания исполняемого файла. Это то, что вы хотите сделать, если хотите добавить свои собственные изменения в ASF или если по какой-либо причине вы не доверяете исполняемым файлам, указанным в официальных **[выпусках](https://github.com/JustArchiNET/ArchiSteamFarm/releases)**. Если вы пользователь, а не разработчик, скорее всего, вы хотите использовать уже предварительно скомпилированные двоичные файлы, но если вы хотите использовать свои собственные или узнать что-то новое, продолжайте читать.

ASF может быть скомпилирован на любой поддерживаемой платформе, если у вас есть все необходимые для этого инструменты.

* * *

## .NET Core SDK 

Независимо от платформы, для компиляции ASF необходим полный .NET Core SDK (а не только время выполнения). Инструкции по установке можно найти на странице **[.NET Core ](https://www.microsoft.com/net/download)**. Вам необходимо установить соответствующую версию .NET Core SDK для вашей ОС. После успешной установки, команда ` dotnet ` должна работать и действовать. Вы можете проверить, работает ли она с ` dotnet -info `. Также убедитесь, что ваш .NET Core SDK соответствует требованиям ASF **[ требования к среде выполнения ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)**.

Пример ` dotnet --info ` в Windows:

    .NET Core SDK (отражающий любой global.json):
     Версия: 2.1.300
     Commit: adab45bf0c
    
    Среда выполнения:
     Название ОС: Windows
     Версия ОС: 10.0.17134
     Платформа ОС: Windows
     RID: win10-x64
     Базовый путь: C: \ Program Files \ dotnet \ sdk \ 2.1.300 \
    
    Хост (полезен для поддержки):
      Версия: 2.1.0
      Commit: caa7b7e2ba
    
    .NET Core SDK установлены:
      2.1.300 [C: \ Program Files \ dotnet \ sdk]
    
    Установленные среды выполнения .NET Core:
      Microsoft.AspNetCore.All 2.1.0 [C: \ Program Files \ dotnet \ shared \ Microsoft.AspNetCore.All]
      Microsoft.AspNetCore.App 2.1.0 [C: \ Program Files \ dotnet \ shared \ Microsoft.AspNetCore.App]
      Microsoft.NETCore.App 2.1.0 [C: \ Program Files \ dotnet \ shared \ Microsoft.NETCore.App]
    

* * *

## Компиляция

Предполагая, что у вас есть ОС .NET Core SDK и как раз в подходящей версии, просто перейдите в каталог ASF и выполните:

```shell
dotnet publish ArchiSteamFarm -c "Релиз" -f "netcoreapp2.1" -o "out/generic" "/p:LinkDuringPublish=false"
```

Если вы используете Linux/OS X, вместо этого вы можете использовать скрипт ` cc.sh `, который будет делать то же самое, но несколько сложнее.

Если компиляция завершилась успешно, вы можете найти свой ASF в ` источник ` flavour в каталоге ` ArchiSteamFarm / out / generic `. Это то же самое, что и официальная сборка ` generic ` ASF, но она заставила ` UpdateChannel ` и ` UpdatePeriod ` ` 0 `.

### Пакеты под конкретные ОС

Вы также можете создать специальный пакет .NET Core для вашей ОС, если у вас есть такая потребность. В общем, вы не должны этого делать, потому что вы только что скомпилировали ` generic ` flavour, который вы можете запустить с уже установленной версией .NET Core, которую вы использовали для компиляции в первую очередь, но только в если вы хотите:

```shell
dotnet publish ArchiSteamFarm -c "Релиз" -f "netcoreapp2.1" -o "out/linux-x64" -r "linux-x64" "/p:CrossGenDuringPublish=false"
```

Конечно, замените ` linux-x64 ` на OS-архитектуру, на которую вы хотите настроить таргетинг, например ` win-x64 `. Обновления этой сборки также будут отключены.

### .NET Framework

В очень редком случае, когда вы захотите создать билд ` generic-netf `, вы можете изменить целевую структуру с ` netcoreapp2.1 ` на ` net472 `. Имейте в виду, что вам потребуется соответствующий пакет **[ .NET Framework ](https://www.microsoft.com/net/download/visual-studio-sdks)** для компиляции варианта ` netf `, в дополнение к .NET Core SDK.

```shell
dotnet publish ArchiSteamFarm -c "Релиз" -f "net472" -o "out/generic-netf"
```

В еще более редком случае, если вы не можете установить .NET Framework или даже сам .NET Core SDK (например, из-за построения билда на ` linux-x86 ` с ` mono `), вы может вызвать ` msbuild ` напрямую:

```shell
msbuild /m /p:Configuration=Release /p:PublishDir=out/generic-netf /p:TargetFramework=net472 /r /t:Publish ArchiSteamFarm
```

* * *

## Разработка

Если вы хотите отредактировать код ASF, для этой цели вы можете использовать любую совместимую с .NET Core IDE, хотя это необязательно, поскольку вы также можете редактировать с помощью блокнота и скомпилировать команду ` dotnet ` как описано выше. Тем не менее, для Windows мы рекомендуем **[ последнюю версию Visual Studio ](https://www.visualstudio.com/downloads)** (бесплатной community version более чем достаточно). Мы также предлагаем использовать его вместе с **[ ReSharper ](https://www.jetbrains.com/resharper)**, хотя это не бесплатный продукт.

Если вы хотите работать с кодом ASF на Linux/OS X, мы рекомендуем **[ последнюю версию Visual Studio Code ](https://code.visualstudio.com/download)**. Оно не обладает такими возможностями как классическая Visual Studio, но и этого вполне достаточно.

Of course all suggestions above are only recommendations, you can use whatever you want to, it comes down to `dotnet build` command anyway. We use Visual Studio + ReSharper for ASF development, with a small part of third-party `tools` that you can find in the repo.

* * *

## Метки

`master` branch is not guaranteed to be in a state that allows successful compilation or flawless ASF execution in the first place, since it's development branch just like stated in our **[release cycle](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle)**. If you want to compile ASF from source, then you should use appropriate **[tag](https://github.com/JustArchiNET/ArchiSteamFarm/tags)** for that purpose, which guarantees at least successful compilation, and very likely also flawless execution (if build was marked as stable release). In order to check the current "health" of the tree, you can use our CIs - **[AppVeyor](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)** or **[Travis](https://travis-ci.com/JustArchiNET/ArchiSteamFarm)**.

* * *

## Официальные версии

Official ASF releases are compiled by **[AppVeyor](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)** on Windows, with latest .NET Core SDK that matches ASF **[runtime requirements](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)**. After passing tests, all packages are deployed on GitHub. This also guarantees transparency, since AppVeyor always uses official public source for all builds, and you can compare checksums of AppVeyor artifacts with GitHub assets. ASF developers do not compile or publish builds manually, except for private development process, including debugging.