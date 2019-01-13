# 插件

自 ASF V4 开始，程序支持在运行时加载自定义插件。 插件允许您自定义 ASF 行为，例如添加自定义命令、自定义交易逻辑或者与其他第三方服务与 API 进行整体集成。

* * *

## 致用户

ASF 会从 ASF 目录内的 `plugins` 文件夹加载插件。 It's recommended to create a dedicated folder for each plugin that you want to use, it can be based off its name, such as `MyPlugin`. Doing so will result in the final tree structure of `plugins/MyPlugin`. Finally, put all the binary files of the plugin you've downloaded inside the folder you've created and start ASF. Usually plugin developers will publish their plugins in form of a `zip` file with already-prepared structure for you, so it's enough to unpack that zip archive into `plugins` directory, which will create the appropriate folder automatically.

如果插件成功加载，您将会在日志内看到它的名称和版本。 You should consult your plugin developers in case of questions, issues or usage related to the plugins that you've decided to use.

**Please note that ASF plugins might be malicious**. You should always ensure that you're using plugins made by developers that you can trust.

* * *

## 致开发者

插件是继承 ASF 通用 `IPlugin` 接口的标准 .NET 库。 您可以完全独立于 ASF 主线开发插件，并在当前和未来 ASF 版本中复用它们，只要 API 仍保持兼容。 ASF 使用的插件系统基于 `System.Composition`，过去它被称为 **[Managed Extensibility Framework](https://docs.microsoft.com/dotnet/framework/mef)**，它可以使 ASF 在运行时动态检测并加载库文件。

* * *

### 入门指南

您的项目应该是一个标准 .NET 库，其目标指向对应 ASF 版本所使用框架的版本，如&#8203;**[编译](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compilation)**&#8203;章节所述。 我们建议您以 .NET Core 为目标，但 .NET Framework 框架插件也是可用的。

The project must reference main `ArchiSteamFarm` assembly, either its prebuilt `ArchiSteamFarm.dll` library that you've downloaded as part of the release, or the source project (e.g. if you decided to add ASF tree as submodule). 这样，您就可以访问与检查 ASF 的结构、方法和属性，特别是您接下来需要继承的核心 `IPlugin` 接口。 该项目还必须至少引用 `System.Composition.AttributedModel`，使您能够将插件的 `IPlugin` 实现 `[Export]`（导出）给 ASF 使用。 此外，您可能还需要引用其他公共库，以便解析某些接口提供给您的数据结构，但除非您明确需要它们，否则现在这样就足够了。

如果您正确执行了所有操作，则您的 `csproj` 文件结构应该类似：

```csproj
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>netcoreapp2.2</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="System.Composition.AttributedModel" Version="*" />
  </ItemGroup>

  <ItemGroup>
    <Reference Include="ArchiSteamFarm">
      <HintPath>C:\\Path\To\Downloaded\ArchiSteamFarm.dll</HintPath>
    </Reference>

    <!-- If building as part of ASF source tree, use this instead of <Reference> above -->
    <!-- <ProjectReference Include="..\ArchiSteamFarm\ArchiSteamFarm.csproj" /> -->
  </ItemGroup>
</Project>
```

而代码方面，您的插件类必须继承自 `IPlugin` 接口（可以是显式继承，或者通过 `IASF` 等更专门的接口进行隐式继承），并且 `[Export(typeof(IPlugin))]` 使 ASF 能够在运行时识别。 实现该目标的最简示例如下：

```csharp
using System;
using System.Composition;
using ArchiSteamFarm;
using ArchiSteamFarm.Plugins;

namespace YourNamespace.YourPluginName {
    [Export(typeof(IPlugin))]
    public sealed class YourPluginName : IPlugin {
        public string Name => nameof(YourPluginName);
        public Version Version => typeof(YourPluginName).Assembly.GetName().Version;

        public void OnLoaded() {
            ASF.ArchiLogger.LogGenericInfo("Meow");
        }
    }
}
```

要使用您的插件，就必须先进行编译。 You can do that either from your IDE, or from within the root directory of your project via a command:

```shell
# If your project is standalone (no need to define its name since it's the only one)
dotnet publish -c "Release" -o "out"

# If your project is part of ASF source tree (to avoid compiling unnecessary parts)
dotnet publish YourNamespace.YourPluginName -c "Release" -o "out"
```

Afterwards, your plugin is ready for deployment. It's up to you how exactly you want to distribute and publish your plugin, but we recommend creating a zip archive with a single folder named `YourNamespace.YourPluginName`, inside which you'll put your compiled plugin together with its **[dependencies](#plugin-dependencies)**. This way user will simply need to unpack your zip archive into his `plugins` directory and do nothing else.

这只是让您入门的最基本场景，我们提供了 **[ArchiSteamFarm.CustomPlugins.ExamplePlugin](https://github.com/JustArchiNET/ArchiSteamFarm/tree/master/ArchiSteamFarm.CustomPlugins.ExamplePlugin)** 项目，向您展示您可以在自己的插件内实现的接口和操作的示例，还有实用的注释。 如果您希望从现有的代码中学习，可以随意查看该项目，或者自行探索 `ArchiSteamFarm.Plugins` 命名空间，并且参考包含所有可用选项的文档。

* * *

### API 可用性

除了您有权在接口本身中访问的内容外，ASF 还向您公开了许多内部 API，您可以使用这些 API 来扩展功能。 例如，如果您想向 Steam Web 发送某种新请求，则无需从零开始实现一切，特别是自行处理我们已经处理过的一切。 只需要使用我们的 `Bot.ArchiWebHandler`，其中已经公开了许多 `UrlWithSession()` 方法供您使用，它们已经为您完成了一切底层工作，例如身份验证、会话刷新或者处理 Web 限制等。

我们对于 API 可用性方面的政策非常开放，所以如果您需要利用 ASF 代码中已有的内容，请&#8203;**[开启一个 Issue](https://github.com/JustArchiNET/ArchiSteamFarm/issues)**，并在其中解释您需要使用的 ASF 内部 API 以及您计划使用的场景。 只要您的使用场景有意义，我们就很可能不会反对。 我们不可能立刻公开一切可以利用的内容，所以我们只会公开目前对我们来说最有意义的部分，然后等待您的需求，如果您需要访问的内容尚未标记为 `public`（公开）。 这也包括所有关于新 `IPlugin` 接口的建议，只要它们能够合理地扩展现有功能。

实际上，ASF 内部 API 是插件功能的唯一限制。 没有什么能够阻止您，因为您的插件也可以有自己的依赖项，例如，您的应用可以引用 `Discord.Net` 库，在您的 Discord 机器人与 ASF 命令之间架起桥梁。 可能性是无穷无尽的，我们会尽力为您的插件提供最大的自由与灵活性，所以没有任何人为的限制，只是我们不能完全确定哪些 ASF 组件是插件开发所需的（您可以告知我们详情来解决这一点）。

* * *

### API 兼容性

需要强调的是，ASF 是一个面向用户的应用程序，而不是一个您可以无条件依赖的、具有稳定 API 接口的库。 这意味着，您无法假定您的插件一经编译就可以在未来所有 ASF 版本中可用，如果您想进一步开发程序，这是不可能的，我们无法仅仅为了向后兼容就放弃不断适应 Steam 的变化。 这对您来说应该是符合逻辑的，但强调这个事实很重要。

我们会尽最大努力保持 ASF 的公开部分能够正常、稳定，但如果有足够的理由，我们不会惧怕破坏兼容性，而是遵守&#8203;**[弃用](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Deprecation)**&#8203;政策进行变更。 这对于作为 ASF 基础设施的一部分公开给您的内部 ASF 结构来说尤为重要，如上文所述（例如 `ArchiWebHandler`），它们可能会在未来的某个版本中作为 ASF 的增强而被改进（或者说被重写）。 我们会尽全力在更新日志中为您提供适当的说明，并且在运行时显示适当的与过时功能有关的警告。 我们不会故意为了重写而重写，因此您可以确信，下一个次要 ASF 版本不会仅仅因为版本号更高就让插件完全失效，但您最好时刻关注更新日志，并且偶尔实际验证是否一切都正常工作。

* * *

### Plugin dependencies

Your plugin will include at least two dependencies by default, `ArchiSteamFarm` reference for internal API, and `PackageReference` of `System.Composition.AttributedModel` that is required for being recognized as ASF plugin to begin with. In addition to that, it might include more dependencies in regards to what you've decided to do in your plugin (e.g. `Discord.Net` library if you've decided to integrate with Discord).

The output of your build will include your core `YourNamespace.YourPluginName.dll` library, and all the dependencies that you've referenced, at the minimum `ArchiSteamFarm.dll` and `System.Composition.AttributedModel.dll`.

Since you're developing a plugin to already-working program, you don't have to, and even **shouldn't** include all the dependencies that were generated for you during build. This is because ASF already includes majority of those, for example `ArchiSteamFarm`, `SteamKit2` or `Newtonsoft.Json`. Stripping down your build off dependencies shared with ASF is not the absolute requirement for your plugin to work, but doing so will dramatically cut the memory footprint and the size of your plugin, together with increasing the performance, due to the fact that ASF will share its own dependencies with you, and will load only those libraries that it doesn't know about itself.

Therefore, it's a recommended practice to include only those libraries that ASF doesn't include, or includes in the wrong version. Examples of those would be obviously `YourNamespace.YourPluginName.dll`, but for example also `Discord.Net.dll` if you decided to depend on it. Bundling libraries that are shared with ASF can still make sense if you want to ensure API compatibility (e.g. being sure that `Newtonsoft.Json` which you depend on in your plugin will always be in version `X` and not the one that ASF ships with), but obviously doing that comes for a price of increased memory/size and worse performance.

If you're confused about above statement and you don't know better, check which `dll` libraries are included in `ASF-generic.zip` package and ensure that your plugin includes only those that are not part of it yet. This will be only `YourNamespace.YourPluginName.dll` for the most simple plugins. If you get any issues during runtime in regards to version mismatch, include the affected libraries as well.