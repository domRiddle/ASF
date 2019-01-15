# Plugins

Starting with ASF V4, the program includes support for custom plugins that can be loaded during runtime. Plugins allow you to customize ASF behaviour, for example by adding custom commands, custom trading logic or whole integration with third-party services and APIs.

---

## For users

ASF loads plugins from `plugins` directory located in your ASF folder. It's recommended to create a dedicated folder for each plugin that you want to use, it can be based off its name, such as `MyPlugin`. Doing so will result in the final tree structure of `plugins/MyPlugin`. Finally, put all the binary files of the plugin you've downloaded inside the folder you've created and start ASF. Usually plugin developers will publish their plugins in form of a `zip` file with already-prepared structure for you, so it's enough to unpack that zip archive into `plugins` directory, which will create the appropriate folder automatically.

If the plugin was loaded successfully, you'll see its name and version in your log. You should consult your plugin developers in case of questions, issues or usage related to the plugins that you've decided to use.

**Please note that ASF plugins might be malicious**. You should always ensure that you're using plugins made by developers that you can trust.

---

## For developers

Plugins are standard .NET libraries that inherit common `IPlugin` interface with ASF. You can develop plugins entirely independently of mainline ASF and reuse them in current and future ASF versions, as long as API remains compatible. Plugin system used in ASF is based on `System.Composition`, formerly known as **[Managed Extensibility Framework](https://docs.microsoft.com/dotnet/framework/mef)** which allows ASF to discover and load your libraries during runtime.

---

### Getting started

Your project should be a standard .NET library targetting appropriate framework of your target ASF version, as specified in the **[compilation](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compilation)**. We recommend you to target .NET Core, but .NET Framework plugins are also available.

The project must reference main `ArchiSteamFarm` assembly, either its prebuilt `ArchiSteamFarm.dll` library that you've downloaded as part of the release, or the source project (e.g. if you decided to add ASF tree as submodule). This will allow you to access and discover ASF structures, methods and properties, especially core `IPlugin` interface which you'll need to inherit from in the next step. The project must also reference `System.Composition.AttributedModel` at the minimum, which allows you to `[Export]` your `IPlugin` for ASF to use. In addition to that, you may want/need to reference other common libraries in order to interpret the data structures that are given to you in some interfaces, but unless you need them explicitly, that will be enough for now.

If you did everything properly, your `csproj` will be similar to below:

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

From the code side, your plugin class must inherit from `IPlugin` interface (either explicitly, or implicitly by inheriting from more specialized interface, such as `IASF`) and `[Export(typeof(IPlugin))]` in order to be recognized by ASF during runtime. The most bare example that achieves that would be the following:

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

In order to make use of your plugin, you must firstly compile it. You can do that either from your IDE, or from within the root directory of your project via a command:

```shell
# If your project is standalone (no need to define its name since it's the only one)
dotnet publish -c "Release" -o "out"

# If your project is part of ASF source tree (to avoid compiling unnecessary parts)
dotnet publish YourNamespace.YourPluginName -c "Release" -o "out"
```

Afterwards, your plugin is ready for deployment. It's up to you how exactly you want to distribute and publish your plugin, but we recommend creating a zip archive with a single folder named `YourNamespace.YourPluginName`, inside which you'll put your compiled plugin together with its **[dependencies](#plugin-dependencies)**. This way user will simply need to unpack your zip archive into his `plugins` directory and do nothing else.

This is only the most basic scenario to get you started, we have **[ArchiSteamFarm.CustomPlugins.ExamplePlugin](https://github.com/JustArchiNET/ArchiSteamFarm/tree/master/ArchiSteamFarm.CustomPlugins.ExamplePlugin)** project that shows you example interfaces and actions that you can do within your own plugin, including helpful comments. Feel free to take a look if you'd like to learn from a working code, or discover `ArchiSteamFarm.Plugins` namespace yourself and refer to the included documentation for all available options.

---

### API availability

ASF, apart from what you have access to in the interfaces themselves, exposes to you a lot of its internal APIs that you can make use in order to extend the functionality. For example, if you'd like to send some kind of new request to Steam web, then you do not need to implement everything from scratch, especially dealing with all the issues we've had to deal with before you. Simply use our `Bot.ArchiWebHandler` which already exposes a lot of `UrlWithSession()` methods for you to use, handling all the lower-level stuff such as authentication, session refresh or web limiting for you.

We have a very open policy in terms of our API availability, so if you'd like to make use of something that ASF code already includes, simply **[open an issue](https://github.com/JustArchiNET/ArchiSteamFarm/issues)** and explain in it your planned use case of our ASF's internal API. We most likely won't have anything against, as long as your use case makes sense. It's simply impossible for us to open everything that somebody would like to make use of, so we've opened what makes the most sense for us, and waiting for your requests in case you'd like to have access to something that isn't `public` yet. This also includes all suggestions in regards to new `IPlugin` interfaces that could make sense to be added in order to extend existing functionality.

In fact, internal ASF's API is the only real limitation in terms of what your plugin can do. Nothing is stopping you from e.g. including `Discord.Net` library in your application and creating a bridge between your Discord bot and ASF commands, since your plugin can also have dependencies on its own. The possibilities are endless, and we made our best to give you as much freedom and flexibility as possible within your plugin, so there are no artificial limits on anything, just us not being completely sure which ASF parts are crucial for your plugin development (which you can solve by letting us know).

---

### API compatibility

It's important to emphasize that ASF is a consumer application and not a typical library with fixed API surface that you can depend on unconditionally. This means that you can't assume that your plugin once compiled will keep working with all future ASF releases regardless, it's just impossible if you want to keep developing the program further, and being unable to adapt to ever-ongoing Steam changes for the sake of backwards compatibility is just not appropriate for our case. This should be logical for you, but it's important to highlight that fact.

We'll do our best to keep public parts of ASF working and stable, but we'll not be afraid to break the compatibility if good enough reasons arise, following our **[deprecation](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Deprecation)** policy in the process. This is especially important in regards to internal ASF structures that are exposed to you as part of ASF infrastructure, explained above (e.g. `ArchiWebHandler`) which might be improved (and therefore rewritten) as part of ASF enhancements in one of the future versions. We'll do our best to inform you appropriately in the changelogs, and include appropriate warnings during runtime about obsolete features. We do not intend to rewrite everything for the sake of rewriting it, so you can be fairly sure that the next minor ASF version won't just simply destroy your plugin entirely only because it has a higher version number, but keeping an eye on changelogs and occasional verification if everything works fine is a very good idea.

---

### Plugin dependencies

Your plugin will include at least two dependencies by default, `ArchiSteamFarm` reference for internal API, and `PackageReference` of `System.Composition.AttributedModel` that is required for being recognized as ASF plugin to begin with. In addition to that, it might include more dependencies in regards to what you've decided to do in your plugin (e.g. `Discord.Net` library if you've decided to integrate with Discord).

The output of your build will include your core `YourNamespace.YourPluginName.dll` library, and all the dependencies that you've referenced, at the minimum `ArchiSteamFarm.dll` and `System.Composition.AttributedModel.dll`.

Since you're developing a plugin to already-working program, you don't have to, and even **shouldn't** include all the dependencies that were generated for you during build. This is because ASF already includes majority of those, for example `ArchiSteamFarm`, `SteamKit2` or `Newtonsoft.Json`. Stripping down your build off dependencies shared with ASF is not the absolute requirement for your plugin to work, but doing so will dramatically cut the memory footprint and the size of your plugin, together with increasing the performance, due to the fact that ASF will share its own dependencies with you, and will load only those libraries that it doesn't know about itself.

Therefore, it's a recommended practice to include only those libraries that ASF doesn't include, or includes in the wrong version. Examples of those would be obviously `YourNamespace.YourPluginName.dll`, but for example also `Discord.Net.dll` if you decided to depend on it. Bundling libraries that are shared with ASF can still make sense if you want to ensure API compatibility (e.g. being sure that `Newtonsoft.Json` which you depend on in your plugin will always be in version `X` and not the one that ASF ships with), but obviously doing that comes for a price of increased memory/size and worse performance.

If you're confused about above statement and you don't know better, check which `dll` libraries are included in `ASF-generic.zip` package and ensure that your plugin includes only those that are not part of it yet. This will be only `YourNamespace.YourPluginName.dll` for the most simple plugins. If you get any issues during runtime in regards to version mismatch, include the affected libraries as well.