# Компиляция

Компиляция - это процесс создания исполняемого файла. Это то, что вы хотите сделать, если хотите добавить свои собственные изменения в ASF или если по какой-либо причине вы не доверяете исполняемым файлам, указанным в официальных **[выпусках](https://github.com/JustArchiNET/ArchiSteamFarm/releases)**. Если вы пользователь, а не разработчик, скорее всего, вы хотите использовать уже предварительно скомпилированные двоичные файлы, но если вы хотите использовать свои собственные или узнать что-то новое, продолжайте читать.

ASF может быть скомпилирован на любой поддерживаемой платформе, если у вас есть все необходимые для этого инструменты.

---

## .NET SDK

Regardless of platform, you need full .NET SDK (not just runtime) in order to compile ASF. Installation instructions can be found on **[.NET download page](https://dotnet.microsoft.com/download)**. You need to install appropriate .NET SDK version for your OS. После успешной установки, команда `dotnet` должна быть полностью работоспособна. Вы можете проверить, работает ли она, вызвав `dotnet --info`. Also ensure that your .NET SDK matches ASF **[runtime requirements](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)**.

---

## Компиляция

Assuming you have .NET SDK operative and in appropriate version, simply navigate to source ASF directory (cloned or downloaded and unpacked ASF repo) and execute:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net5.0" -o "out/generic"
```

Если вы используете Linux/OS X, вместо этого вы можете использовать скрипт `cc.sh`, который будет делать то же самое, но несколько сложнее.

Если компиляция завершилась успешно, вы можете найти свой ASF в варианте `source` в папке `out/generic`. Это то же самое, что и официальная сборка ASF в варианте `generic`, но в ней принудительно установлены равными `0` параметры `UpdateChannel` и `UpdatePeriod`, как и положено для самостоятельной сборки.

### Сборка под конкретные ОС

You can also generate OS-specific .NET package if you have a specific need. In general you shouldn't do that because you've just compiled `generic` flavour that you can run with your already-installed .NET runtime that you've used for the compilation in the first place, but just in case you want to:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net5.0" -o "out/linux-x64" -r "linux-x64"
```

Разумеется, замените `linux-x64` на нужное вам сочетание ОС и архитектуры, например `win-x64`. Обновления этой сборки также будут отключены.

### .NET Framework

В очень редком случае, когда вы захотите создать сборку `generic-netf`, вы можете изменить целевой фреймворк с `net5.0` на `net48`. Keep in mind that you'll need appropriate **[.NET Framework](https://dotnet.microsoft.com/download/visual-studio-sdks)** developer pack for compiling `netf` variant, in addition to .NET SDK, so the below will work only on Windows:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net48" -o "out/generic-netf"
```

In case of being unable to install .NET Framework or even .NET SDK itself (e.g. because of building on `linux-x86` with `mono`), you can call `msbuild` directly. You'll also need to specify `ASFNetFramework` manually, as ASF by default disables `netf` build on non-Windows platforms:

```shell
msbuild /m /r /t:Publish /p:Configuration=Release /p:TargetFramework=net48 /p:PublishDir=out/generic-netf /p:ASFNetFramework=true ArchiSteamFarm
```

---

## Разработка

If you'd like to edit ASF code, you can use any .NET compatible IDE for that purpose, although even that is optional, since you can as well edit with a notepad and compile with `dotnet` command described above. Тем не менее, для Windows мы рекомендуем **[последнюю версию Visual Studio](https://visualstudio.microsoft.com/downloads)** (бесплатной community version более чем достаточно).

Если вы хотите работать с кодом ASF на Linux/OS X, мы рекомендуем **[ последнюю версию Visual Studio Code](https://code.visualstudio.com/download)**. Оно не обладает такими возможностями как классическая Visual Studio, но и этого вполне достаточно.

Разумеется, всё предложенное выше это только наши рекоммендации, вы можете использовать что угодно, всё равно это в конце концов сводится к команде `dotnet build`. We use **[JetBrains Rider](https://www.jetbrains.com/rider)** for ASF development, although it's not a free solution.

---

## Теги

`main` branch is not guaranteed to be in a state that allows successful compilation or flawless ASF execution in the first place, since it's development branch just like stated in our **[release cycle](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle)**. Если вы хотите скомпилировать ASF из исходного кода, или сослаться на исходный код ASF в своём проекте, вам следует использовать для этого соответствующий **[тег](https://github.com/JustArchiNET/ArchiSteamFarm/tags)**, что гарантирует как минимум успешную компиляцию, и скорее всего - безошибочную работу (если эта сборка отмечена как стабильная). In order to check the current "health" of the tree, you can use our CI - **[GitHub](https://github.com/JustArchiNET/ArchiSteamFarm/actions/workflows/ci.yml?query=branch%3Amain)**.

---

## Официальные версии

Official ASF releases are compiled by **[GitHub](https://github.com/JustArchiNET/ArchiSteamFarm/actions)** on Windows, with latest .NET SDK that matches ASF **[runtime requirements](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)**. После успешного прохождения тестов, все пакеты загружаются в виде готового выпуска, также на GitHub. Это гарантирует прозрачность, поскольку GitHub всегда использует официальный публичный исходный код для всех сборок, и вы можете сравнить контрольные суммы артефактов GitHub с файлами выпуска на GitHub. Разработчики ASF не компилируют и не публикуют сборки самостоятельно, за исключением индивидуального процесса разработки и отладки.