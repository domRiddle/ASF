# 编译

编译是生成可执行文件的过程。 如果您希望对 ASF 作出自己的修改，或者出于某种原因不信任官方&#8203;**[发布](https://github.com/JustArchiNET/ArchiSteamFarm/releases)**&#8203;的可执行文件，编译就是您需要做的事。 如果您是用户而不是开发人员，则很可能您希望使用已预编译的二进制文件，但如果您想使用自己的二进制文件或学习新内容，请继续阅读。

只要您拥有所有需要的工具，ASF 就可以在任何当前支持的平台上进行编译。

* * *

## .NET Core SDK

无论使用什么平台，您都需要完整的 .NET Core SDK（不仅仅是运行时环境）才能编译 ASF。 您可以在 **[.NET Core 安装页面](https://www.microsoft.com/net/download)**&#8203;找到安装指南。 您需要安装适合您操作系统的 .NET Core SDK 版本。 安装成功后，`dotnet` 命令应该已经可以使用并且正常运行。 您可以执行 `dotnet --info` 命令进行验证。 同样需要确认您的 .NET Core SDK 匹配 ASF 的&#8203;**[运行时环境需求](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-zh-CN#运行时环境需求)**。

Windows 上的 `dotnet --info` 输出示例：

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

## 编译

假设您已安装适当版本的 .NET Core SDK，现在只需要前往 ASF 目录并执行：

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "netcoreapp2.1" -o "out/generic" "/p:LinkDuringPublish=false"
```

如果您在使用 Linux/OS X，您也可以使用 `cc.sh` 脚本，以稍复杂的方式实现同样的效果。

如果编译成功完成，您可以在 `ArchiSteamFarm/out/generic` 目录中找到您的 ASF `source` 包。 这与官方的 `generic` 构建相同，但它强制 `UpdateChannel` 和 `UpdatePeriod` 为 `0`。

### OS-specific（特定操作系统）

如果您需要，也可以生成特定操作系统的 .NET Core 包。 一般情况下，您不需要这样做，因为您刚刚编译了 `generic` 包，您可以使用已安装的用于编译的 .NET Core 运行时环境运行此包，但如果您确实需要操作系统包：

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "netcoreapp2.1" -o "out/linux-x64" -r "linux-x64" "/p:CrossGenDuringPublish=false"
```

当然，您需要将 `linux-x64` 替换成您需要的目标操作系统架构，例如 `win-x64`。 这一构建也将禁用自动更新。

### .NET 框架

在罕见的情况下，您可能需要构建 `generic-netf` 包，您可以将目标框架从 `netcoreapp2.1` 更改为 `net472`。 请注意，您需要合适的 **[.NET 框架](https://www.microsoft.com/net/download/visual-studio-sdks)**开发者工具包和 .NET Core SDK 才能编译 `netf` 包。

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net472" -o "out/generic-netf"
```

在更罕见的情况下，如果您无法安装 .NET 框架甚至 .NET Core SDK 本身（例如在 `linux-x86` 平台用 `mono` 构建），可以直接调用 `msbuild`：

```shell
msbuild /m /p:Configuration=Release /p:PublishDir=out/generic-netf /p:TargetFramework=net472 /r /t:Publish ArchiSteamFarm
```

* * *

## 开发

如果您想要编辑 ASF 代码，您可以使用任何兼容 .NET Core 的 IDE，但这也是可选的，因为您甚至可以用记事本编辑代码并用上述 `dotnet` 命令编译。 不过，对于 Windows 系统，我们推荐使用&#8203;**[最新版本的 Visual Studio](https://www.visualstudio.com/downloads)**（免费的社区版即可）。 我们还建议您配合使用 **[ReSharper](https://www.jetbrains.com/resharper)**，但它不是一个免费软件。

如果您要在 Linux/OS X 上开发 ASF 代码，我们推荐使用&#8203;**[最新版的 Visual Studio Code](https://code.visualstudio.com/download)**。 它没有经典的 Visual Studio 那么丰富的功能，但是应该足够了。

当然，以上的所有建议都仅仅是建议，您可以使用您想用的任何工具，最后您都要使用 `dotnet build` 命令进行构建。 我们使用 Visual Studio + ReSharper 进行 ASF 的开发，也使用了一部分第三方工具，您可以在仓库的 `tools` 目录中找到它们。

* * *

## 标签

`master` 分支并不保证能够成功编译或者正常运行 ASF，正如我们在**[发布周期](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle-zh-CN)**中所述，这是一个开发分支。 如果您希望从源代码编译 ASF，就应该为此选择适当的**[标签](https://github.com/JustArchiNET/ArchiSteamFarm/tags)**，这样能够保证编译成功，甚至可以正常运行（如果您选择稳定版）。 要检查代码库的“健康状态”，您可以检查我们的 CI——**[AppVeyor](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)** 或 **[Travis](https://travis-ci.com/JustArchiNET/ArchiSteamFarm)**。

* * *

## 官方发布版本

官方 ASF 发布版本由 Windows 上的带有满足 ASF **[&#8203;运行时环境需求](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-zh-CN#运行时环境需求)**的最新版 .NET Core SDK 的 **[AppVeyor](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)** 编译。 经过测试后，所有的包都会被部署在 GitHub 上。 这也保证了透明度，因为 AppVeyor 总是为所有构建使用官方公共源，并且您可以检查 AppVeyor 的 Artifacts 与 GitHub 附件的校验和。 除了私人的开发过程（包括调试）外，ASF 开发人员不会手动编译或发布构建版本。