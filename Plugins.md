# This section is being worked on
## Translators are recommended to skip it until it's ready

# For users

# For developers

Your project should be a standard .NET library targetting appropriate framework of your target ASF version, as specified in the **[compilation](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compilation)**. It must reference main `ArchiSteamFarm` assembly, either its prebuilt `ArchiSteamFarm.dll` library that you've downloaded as part of the release, or the source project. This will allow you to access and discover ASF structures, methods and properties, especially core `IPlugin` interface which you'll need in the next step. The project must also reference `System.Composition.AttributedModel` at the minimum, which allows you to `[Export]` your `IPlugin` for ASF to use. In addition to that, you may want/need to reference other common libraries in order to interpret the data structures that are given to you in some interfaces, but unless you need them explicitly, that will be enough for now.

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

```c#
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

Afterwards you can build your project as usual, create `plugins` directory in your ASF and put `YourNamespace.CatPlugin.dll` inside. ASF will properly recognize your plugin, load it and use.

This is only the most basic scenario to get you started, we have **[ArchiSteamFarm.CustomPlugins.Example](https://github.com/JustArchiNET/ArchiSteamFarm/tree/master/ArchiSteamFarm.CustomPlugins.Example)** project that shows you example interfaces and actions that you can do within your own plugin, including helpful comments. Feel free to take a look if you'd like to learn from a working code, or discover `ArchiSteamFarm.Plugins` namespace yourself and refer to the included documentation.