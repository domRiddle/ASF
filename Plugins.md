# This section is being worked on
## Translators are recommended to skip it until it's ready

# Plugins

Starting with ASF V4, the program includes support for custom plugins that can be loaded during runtime. Plugins allow you to customize ASF behaviour, for example by adding custom commands, custom trading logic or whole integration with third-party services and APIs.

---

## For users

ASF loads plugins from `plugins` directory located in your ASF folder. This folder doesn't exist by default, so you may need to create it if you intend to use custom plugins. Afterwards, you should copy all `dll` libraries of the plugin that you intend to use inside, then restart ASF. If the plugin was loaded successfully, you'll see its name and version in your log. You should consult your plugin developers in case of questions or issues related to the plugins that you've decided to use.

---

## For developers

Plugins are standard .NET libraries that inherit common `IPlugin` interface with ASF. You can develop plugins entirely independently of mainline ASF and reuse them in current and future ASF versions, as long as API remains compatible. Plugin system used in ASF is based on `System.Composition`, formerly known as **[Managed Extensibility Framework](https://docs.microsoft.com/dotnet/framework/mef)** which allows ASF to discover and load your libraries during runtime.

---

### Getting started

Your project should be a standard .NET library targetting appropriate framework of your target ASF version, as specified in the **[compilation](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compilation)**. We recommend you to target .NET Core, but .NET Framework plugins are also available.

The project must reference main `ArchiSteamFarm` assembly, either its prebuilt `ArchiSteamFarm.dll` library that you've downloaded as part of the release, or the source project. This will allow you to access and discover ASF structures, methods and properties, especially core `IPlugin` interface which you'll need to inherit from in the next step. The project must also reference `System.Composition.AttributedModel` at the minimum, which allows you to `[Export]` your `IPlugin` for ASF to use. In addition to that, you may want/need to reference other common libraries in order to interpret the data structures that are given to you in some interfaces, but unless you need them explicitly, that will be enough for now.

If you did everything properly, your `csproj` will be similar to below:

```csproj
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>netcoreapp2.2</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="System.Composition.AttributedModel" Version="1.2.0" />
  </ItemGroup>

  <ItemGroup>
    <Reference Include="ArchiSteamFarm">
      <HintPath>C:\\Path\To\Downloaded\ArchiSteamFarm.dll</HintPath>
    </Reference>

    <!-- Or, in case you decided to build as part of ASF's source tree -->
    <!-- <ProjectReference Include="..\ArchiSteamFarm\ArchiSteamFarm.csproj" /> -->
  </ItemGroup>
</Project>
```

From the code side, your plugin class must inherit from `IPlugin` interface (either explicitly, or implicitly by inheriting from more specialized interface, such as `IASF`) and `[Export(typeof(IPlugin))]` in order to be recognized by ASF during runtime. The most bare example that achieves that would be the following:

```csharp
using System;
using System.Composition;
using ArchiSteamFarm.Plugins;

namespace YourNamespace.CatPlugin {
	[Export(typeof(IPlugin))]
	public sealed class CatPlugin : IPlugin {
		public string Name => nameof(CatPlugin);
		public Version Version => typeof(CatPlugin).Assembly.GetName().Version;

		public void OnLoaded() {
			ASF.ArchiLogger.LogGenericInfo("Meow");
		}
	}
}
```

In order to make use of your plugin, you must firstly compile it. You can do that either from your IDE, or command-line:

```shell
dotnet publish YourNamespace.CatPlugin -c "Release" -o "out"
```

Afterwards, create `plugins` directory in your ASF folder (if needed) and put `YourNamespace.CatPlugin.dll` inside, together with all its optional libraries that you decided to use. Libraries that are natively available in ASF, such as `SteamKit2` or `Newtonsoft.Json` do not need to be included, as they're bundled with ASF already. ASF will properly recognize your plugin, load it (together with all its dependencies in the same folder) and use during runtime.

This is only the most basic scenario to get you started, we have **[ArchiSteamFarm.CustomPlugins.ExamplePlugin](https://github.com/JustArchiNET/ArchiSteamFarm/tree/master/ArchiSteamFarm.CustomPlugins.ExamplePlugin)** project that shows you example interfaces and actions that you can do within your own plugin, including helpful comments. Feel free to take a look if you'd like to learn from a working code, or discover `ArchiSteamFarm.Plugins` namespace yourself and refer to the included documentation for all available options.

---

### API availability

ASF, apart from what you have access to in the interfaces themselves, exposes to you a lot of its internal APIs that you can make use in order to extend the functionality. For example, if you'd like to send some kind of new request to Steam web, then you do not need to implement everything from scratch, especially dealing with all the crap we've had to deal with before you. Simply use our `Bot.ArchiWebHandler` which already exposes a lot of `UrlWithSession()` methods for you to use, handling all the lower-level stuff such as authentication, session refresh or web limiting for you.

We have a very open policy in terms of our API availability, so if you'd like to make use of something that ASF code already includes, simply **[open an issue](https://github.com/JustArchiNET/ArchiSteamFarm/issues)** and explain in it your planned use case of our ASF's internal API. We most likely won't have anything against, as long as your use case makes sense. It's simply impossible for us to open everything that somebody would like to make use of, so we've opened what makes the most sense for us, and waiting for your requests in case you'd like to have access to something that isn't `public` yet. This also includes all suggestions in regards to new `IPlugin` interfaces that could make sense to be added in order to extend existing functionality.

In fact, internal ASF's API is the only real limitation in terms of what your plugin can do. Nothing is stopping you from e.g. including `Discord.Net` library in your application and creating a bridge between your Discord bot and ASF commands, since your plugin can also have dependencies on its own. The possibilities are endless, and we made our best to give you as much freedom and flexibility as possible within your plugin, so there are no artificial limits on anything, just us not being completely sure which ASF parts are crucial for your plugin development (which you can solve by letting us know).

---

### API compatibility

It's important to emphasize that ASF is a consumer application and not a typical library with fixed API surface that you can depend on unconditionally. This means that you can't assume that your plugin once compiled will keep working with all future ASF releases regardless, it's just impossible if you want to keep developing the program further, and being unable to adapt to ever-ongoing Steam changes for the sake of backwards compatibility is just not appropriate for our case. This should be logical for you, but it's important to highlight that fact.

We'll do our best to keep public parts of ASF working and stable, but we'll not be afraid to break the compatibility if good enough reasons arise, following our **[deprecation](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Deprecation)** policy in the process. This is especially important in regards to internal ASF structures that are exposed to you as part of ASF infrastructure, explained above (e.g. `ArchiWebHandler`) which might be improved (and therefore rewritten) as part of ASF enhancements in one of the future versions. We'll do our best to inform you appropriately in the changelogs, and include appropriate warnings during runtime about obsolete features. We do not intend to rewrite everything for the sake of rewriting it, so you can be fairly sure that the next minor ASF version won't just simply destroy your plugin due to having higher version number, but keeping an eye on changelogs and occasional verification if everything works fine is a very good idea.