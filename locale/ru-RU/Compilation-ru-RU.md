# Компиляция

Компиляция - это процесс создания исполняемого файла. Это то, что вы хотите сделать, если хотите добавить свои собственные изменения в ASF или если по какой-либо причине вы не доверяете исполняемым файлам, указанным в официальных **[выпусках](https://github.com/JustArchiNET/ArchiSteamFarm/releases)**. Если вы пользователь, а не разработчик, скорее всего, вы хотите использовать уже предварительно скомпилированные двоичные файлы, но если вы хотите использовать свои собственные или узнать что-то новое, продолжайте читать.

ASF может быть скомпилирован на любой поддерживаемой платформе, если у вас есть все необходимые для этого инструменты.

* * *

## .NET Core SDK 

Независимо от платформы, для компиляции ASF необходим полный .NET Core SDK (а не только среда выполнения). Инструкции по установке можно найти на странице **[.NET Core](https://dotnet.microsoft.com/download)**. Вам необходимо установить подходящую версию .NET Core SDK для вашей ОС. После успешной установки, команда `dotnet` должна быть полностью работоспособна. Вы можете проверить, работает ли она, вызвав `dotnet --info`. Также убедитесь, что ваш .NET Core SDK соответствует **[требованиям к среде выполнения](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-ru-RU#user-content-Требования-среды-выполнения)** ASF .

* * *

## Компиляция

Если у вас есть работоспособное .NET Core SDK нужной версии, просто перейдите в папку с исходниками ASF (клонированный или скачанный и распакованный репозиторий ASF) и запустите:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net5.0" -o "out/generic"
```

Если вы используете Linux/OS X, вместо этого вы можете использовать скрипт `cc.sh`, который будет делать то же самое, но несколько сложнее.

Если компиляция завершилась успешно, вы можете найти свой ASF в варианте `source` в папке `out/generic`. Это то же самое, что и официальная сборка ASF в варианте `generic`, но в ней принудительно установлены равными `0` параметры `UpdateChannel` и `UpdatePeriod`, как и положено для самостоятельной сборки.

### Сборка под конкретные ОС

Вы также можете создать пакеты .NET Core для конкретных ОС, если у вас есть такая потребность. Обычно вы не должны этого делать, потому что вы только что скомпилировали вариант `generic`, который вы можете запустить с уже установленной средой выполнения .NET Core, которую вы только что использовали для компиляции, но на случай, если вы этого хотите:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net5.0" -o "out/linux-x64" -r "linux-x64"
```

Разумеется, замените `linux-x64` на нужное вам сочетание ОС и архитектуры, например `win-x64`. Обновления этой сборки также будут отключены.

### .NET Framework

В очень редком случае, когда вы захотите создать сборку `generic-netf`, вы можете изменить целевой фреймворк с `net5.0` на `net48`. Имейте в виду, что для компиляции в варианте `netf`, в дополнение к .NET Core SDK, вам потребуется соответствующий пакет **[ .NET Framework](https://dotnet.microsoft.com/download/visual-studio-sdks)**, поэтому следующая команда сработает только под Windows:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net48" -o "out/generic-netf"
```

В случае, если вы не можете установить .NET Framework или даже сам .NET Core SDK (например, из-за сборки в системе `linux-x86` под `mono`), вы можете вызвать `msbuild` напрямую. You'll also need to specify `ASFNetFramework` manually, as ASF by default disables `netf` build on non-Windows platforms:

```shell
msbuild /m /r /t:Publish /p:Configuration=Release /p:TargetFramework=net48 /p:PublishDir=out/generic-netf /p:ASFNetFramework=true ArchiSteamFarm
```

* * *

## Разработка

Если вы хотите изменить код ASF, для этой цели вы можете использовать любую совместимую с .NET Core IDE, хотя даже это необязательно, поскольку вы также можете редактировать код с помощью блокнота и компилировать его командой `dotnet`, как описано выше. Тем не менее, для Windows мы рекомендуем **[последнюю версию Visual Studio](https://visualstudio.microsoft.com/downloads)** (бесплатной community version более чем достаточно).

Если вы хотите работать с кодом ASF на Linux/OS X, мы рекомендуем **[ последнюю версию Visual Studio Code](https://code.visualstudio.com/download)**. Оно не обладает такими возможностями как классическая Visual Studio, но и этого вполне достаточно.

Разумеется, всё предложенное выше это только наши рекоммендации, вы можете использовать что угодно, всё равно это в конце концов сводится к команде `dotnet build`. We use **[JetBrains Rider](https://www.jetbrains.com/rider)** for ASF development, although it's not a free solution.

* * *

## Теги

`main` branch is not guaranteed to be in a state that allows successful compilation or flawless ASF execution in the first place, since it's development branch just like stated in our **[release cycle](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle)**. Если вы хотите скомпилировать ASF из исходного кода, или сослаться на исходный код ASF в своём проекте, вам следует использовать для этого соответствующий **[тег](https://github.com/JustArchiNET/ArchiSteamFarm/tags)**, что гарантирует как минимум успешную компиляцию, и скорее всего - безошибочную работу (если эта сборка отмечена как стабильная). In order to check the current "health" of the tree, you can use our continuous integrations - **[GitHub](https://github.com/JustArchiNET/ArchiSteamFarm/actions)** or **[AppVeyor](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)**.

* * *

## Официальные версии

Официальные выпуски ASF компилируются с помощью **[GitHub](https://github.com/JustArchiNET/ArchiSteamFarm/actions)** под Windows, с последним .NET Core SDK удовлетворяющим **[требования среды выполнения](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-ru-RU#user-content-Требования-среды-выполнения)** ASF. После успешного прохождения тестов, все пакеты загружаются в виде готового выпуска, также на GitHub. Это гарантирует прозрачность, поскольку GitHub всегда использует официальный публичный исходный код для всех сборок, и вы можете сравнить контрольные суммы артефактов GitHub с файлами выпуска на GitHub. Разработчики ASF не компилируют и не публикуют сборки самостоятельно, за исключением индивидуального процесса разработки и отладки.