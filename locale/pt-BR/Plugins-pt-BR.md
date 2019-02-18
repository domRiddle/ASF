# Plugins

À partir do ASF V4, o programa inclui suporte para plugins customizados que podem ser carregados durante a execução. Plugins permitem que você personalize o comportamento do ASF, adicionando comandos e lógicas de troca personalizados ou mesmo uma completa integração com serviços de terceiros e APIs.

* * *

## Para usuários

O ASF carrega os plugins localizados na pasta `plugins`, dentro da pasta do ASF. Uma prática recomendada é criar uma pasta para cada plugin que você queira usar, que pode ser baseada no nome do plugin, tal como `MyPlugin`. A estrutura então será `plugins/MyPlugin`. Todos os arquivos binários do plugin devem ser colocados dentro dessa pasta, o ASF vai carregar e usar seu plugin corretamente após reiniciar.

Geralmente os desenvolvedores publicarão seus plugins em um arquivo `zip` com uma estrutura determinada, então basta descompactar o arquivo zip na pasta `plugins`, que criará a pasta apropriada automaticamente.

Se o plugin foi carregado com êxito, você verá seu nome e versão no log. Você deve consultar os desenvolvedores dos plugins em caso de dúvidas, problemas ou forma de uso relacionado com os plugins que você decidiu usar.

Você pode encontrar alguns plugins em destaque na seção **[terceiros](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Third-party-pt-br#plugins-para-o-asf)**.

**Por favor, note que alguns plugins podem ser mal intencionados**. Você deve sempre usar apenas plugins de desenvolvedores confiáveis. Os desenvolvedores do ASF não podem garantir os benefícios padrões do ASF (tal como a não presença de malwares ou ser livre de VAC ban) caso você decida usar qualquer plugin personalizado. Também não podemos oferecer suporte a configurações que utilizam esses plugins, já que você não estará mais rodando a versão original do código do ASF.

* * *

## Para desenvolvedores

Plugins são bibliotecas padrão .NET que herdam a interface `IPlugin` em comum com o ASF. Você pode desenvolver plugins inteiramente independentes da linha principal do ASF e reutilizá-los nas suas versões atuais e futuras, contando que a API permaneça compatível. O sistema de plugins usado no ASF é baseado em `System.Composition`, conhecido anteriormente como **[Managed Extensibility Framework](https://docs.microsoft.com/dotnet/framework/mef)** que permite que o ASF descubra e carregue suas bibliotecas durante o tempo de execução.

* * *

### Como começar

Seu projeto deve ser uma biblioteca padrão .NET que vise o framework da sua versão alvo do ASF, como especificado em **[compilação](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compilation-pt-BR)**. Recomendamos que você busque o .NET Core, mas plugins .NET Framework também são aceitos.

O projeto deve fazer referência a arquitetura principal `ArchiSteamFarm`, a biblioteca pré-compilada `ArchiSteamFarm.dll` que você baixou como parte do lançamento, ou o projeto fonte (por exemplo, se você decidiu adicionar a árvore do ASF como submódulo). Isso permitirá que você acesse e descubra as estruturas do ASF, seus métodos e propriedades, especialmente a interface núcleo do `IPlugin` que você precisará herdar para o próximo passo. O projeto deve também fazer referência, no mínimo, ao `System.Composition.AttributedModel`, que permite que você use o `[Export]` para exportar seu `IPlugin` para ser usado no ASF. Além disso, você pode querer/precisar fazer referência a outras bibliotecas comuns a fim de interpretar as estruturas de dados que são dadas a você em algumas interfaces, mas a menos que você realmente precise delas, isso é suficiente por enquanto.

Se você fez tudo certo, seu `csproj` será semelhante ao exemplo abaixo:

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
    <!-- <ProjectReference Include="C:\\Path\To\ArchiSteamFarm\ArchiSteamFarm.csproj" /> -->
  </ItemGroup>
</Project>
```

Da parte do código, sua classe de plugin deve herdar da interface `IPlugin` (seja explicitamente ou implicitamente herdando de uma interface mais especializada, tal como `IASF`) e `[Export(typeof(IPlugin))]` para que seja reconhecido pelo ASF durante o tempo de execução. E exemplo mais puro disso seria:

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

Para utilizar seu plugin você deve primeiro compila-lo. Você pode fazer isso pela sua IDE, ou de dentro da pasta raiz do seu projeto através do comando:

```shell
# Se o seu projeto for independente (não é preciso definir um nome já que ele será único)
dotnet publish -c "Release" -o "out"


# Se o seu projeto fizer parte da árvore de código do ASF (para evitar a compilação de partes desnecessárias)
dotnet publish YourPluginName -c "Release" -o "out"
```

Após isso seu plugin estará pronto. Como exatamente você quer distribuir e publicar seu plugin é por sua conta, mas recomendamos criar um arquivo zip com uma pasta apenas, chamada `YourNamespace.YourPluginName`, dentro da qual você colocará seu plugin compilado juntamente com suas **[dependências](#dependências -do-plugin)**. Dessa forma o usuário precisará apenas descompactar o arquivo zip dentro da pasta `plugins` e nada mais.

Este é apenas o cenário mais básico para começar, temos o projeto **[`ExamplePlugin`](https://github.com/JustArchiNET/ArchiSteamFarm/tree/master/ArchiSteamFarm.CustomPlugins.ExamplePlugin)** que mostra interfaces e ações que você pode fazer dentro do seu próprio plugin, incluindo comentários úteis. Sinta-se livre para dar uma olhada se você quiser aprender com um código funcional, ou explore o namespace `ArquiSteamFarm.Plugins` por sua conta e consulte a documentação incluída para todas as opções disponíveis.

* * *

### Disponibilidade de API

O ASF, além do que você tem acesso pela interface, expõe muito de suas APIs internas, as quais você pode usar para estender sua funcionalidade. Por exemplo, se você quiser enviar algum tipo de novo pedido para o Steam web você não precisa implementar tudo a partir do zero, tratando todos os problemas com os quais tivemos de lidar antes de você. Simplesmente use nosso `Bot.ArchiWebHandler` que já expõe muitos métodos `UrlWithSession()` para você usar, cuidando de todas as coisas de baixo nível como autenticação, atualização da sessão ou limitação de tráfego para você.

Temos uma política muito aberta em questão da disponibilidade da nossa API, então se você quiser usar algo já incluso no código do ASF basta **[abrir um chamado](https://github.com/JustArchiNET/ArchiSteamFarm/issues)** e explicar nele o uso planejado da API interna do ASF. É muito provável que não tenhamos nada contra, desde que o seu uso preterido faça sentido. É simplesmente impossível abrirmos tudo o que alguém gostaria de usar, então abrimos o que mais faz sentido para nós e esperamos por pedidos caso haja necessidade de acesso a algo que não é `público` ainda. Isso inclui as sugestões a respeito nas novas interfaces `IPlugin` que podem fazer sentido de serem adicionadas para estender funcionalidades existentes.

Na verdade, a API interna do ASF é a única limitação real quanto ao que seu plugin pode fazer. Nada te impede de, por exemplo, incluir a biblioteca `Discord.Net` no seu aplicativo e criar uma ponte entre seu bot do Discord e os comandos do ASF, pois seu plugin também pode ter dependências próprias. As possibilidades são infinitas, e nós fizemos o possível para dar o máximo de liberdade e flexibilidade possível para seu plugin, então não há limites artificiais em nada, apenas não temos completamente certeza de quais partes do ASF são cruciais para o desenvolvimento do seu plugin (que você pode resolver ao nos informar).

* * *

### Compatibilidade de API

It's important to emphasize that ASF is a consumer application and not a typical library with fixed API surface that you can depend on unconditionally. This means that you can't assume that your plugin once compiled will keep working with all future ASF releases regardless, it's just impossible if you want to keep developing the program further, and being unable to adapt to ever-ongoing Steam changes for the sake of backwards compatibility is just not appropriate for our case. This should be logical for you, but it's important to highlight that fact.

We'll do our best to keep public parts of ASF working and stable, but we'll not be afraid to break the compatibility if good enough reasons arise, following our **[deprecation](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Deprecation)** policy in the process. This is especially important in regards to internal ASF structures that are exposed to you as part of ASF infrastructure, explained above (e.g. `ArchiWebHandler`) which might be improved (and therefore rewritten) as part of ASF enhancements in one of the future versions. We'll do our best to inform you appropriately in the changelogs, and include appropriate warnings during runtime about obsolete features. We do not intend to rewrite everything for the sake of rewriting it, so you can be fairly sure that the next minor ASF version won't just simply destroy your plugin entirely only because it has a higher version number, but keeping an eye on changelogs and occasional verification if everything works fine is a very good idea.

* * *

### Plugin dependencies

Your plugin will include at least two dependencies by default, `ArchiSteamFarm` reference for internal API, and `PackageReference` of `System.Composition.AttributedModel` that is required for being recognized as ASF plugin to begin with. In addition to that, it might include more dependencies in regards to what you've decided to do in your plugin (e.g. `Discord.Net` library if you've decided to integrate with Discord).

The output of your build will include your core `YourPluginName.dll` library, and all the dependencies that you've referenced, at the minimum `ArchiSteamFarm.dll` and `System.Composition.AttributedModel.dll`.

Since you're developing a plugin to already-working program, you don't have to, and even **shouldn't** include all the dependencies that were generated for you during build. This is because ASF already includes majority of those, for example `ArchiSteamFarm`, `SteamKit2` or `Newtonsoft.Json`. Stripping down your build off dependencies shared with ASF is not the absolute requirement for your plugin to work, but doing so will dramatically cut the memory footprint and the size of your plugin, together with increasing the performance, due to the fact that ASF will share its own dependencies with you, and will load only those libraries that it doesn't know about itself.

Therefore, it's a recommended practice to include only those libraries that ASF either doesn't include, or includes in the wrong/incompatible version. Examples of those would be obviously `YourPluginName.dll`, but for example also `Discord.Net.dll` if you decided to depend on it. Bundling libraries that are shared with ASF can still make sense if you want to ensure API compatibility (e.g. being sure that `Newtonsoft.Json` which you depend on in your plugin will always be in version `X` and not the one that ASF ships with), but obviously doing that comes for a price of increased memory/size and worse performance.

If you're confused about above statement and you don't know better, check which `dll` libraries are included in `ASF-generic.zip` package and ensure that your plugin includes only those that are not part of it yet. This will be only `YourPluginName.dll` for the most simple plugins. If you get any issues during runtime in regards to some libraries, include those affected libraries as well. If all else fails, you can always decide to bundle everything.

* * *

### Native dependencies

Native dependencies are generated as part of OS-specific builds, as there is no .NET Core runtime available on the host and ASF is running through its own .NET Core runtime that is bundled as part of OS-specific build. In order to minimize the build size, ASF trims its native dependencies to include only the code that can be possibly reached within the program, which effectively cuts the unused parts of the runtime. This can create a potential problem for you in regards to your plugin, if suddenly you find out yourself in a situation where your plugin depends on some .NET Core feature that isn't being used in ASF, and therefore OS-specific builds can't execute it properly.

This is never a problem with generic builds, because those are never dealing with native dependencies in the first place (as they have complete working runtime on the host, executing ASF). It's also automatically one solution to the problem, **use your plugin in generic builds exclusively**, but obviously that has its own downside of cutting your plugin from users that are running OS-specific builds of ASF. If you're wondering if your issue is related to native dependencies, you can also use this method for verification, load your plugin in ASF generic build and see if it works. If it does, you have plugin dependencies covered, so only native dependencies to work on.

Luckily, the real solution to the problem, very similar to general plugin dependencies, is once again bundling your plugin with the dependencies that ASF either doesn't have itself, or has in a wrong/incompatible (e.g. trimmed) version. Compared to plugin dependencies, you can't be sure whether the trimmed version of ASF native dependency has everything you need, so you can either decide to go easy way and just bundle everything, or go hard way and manually verify which parts are missing from ASF and include only those parts.

This also means that **you might need to have a dedicated build of your plugin for each ASF variant**, as each OS-specific ASF build can miss different features that you'll need to provide yourself, and your generic plugin build can't provide all of them on its own. This greatly depends on what your plugin in fact does and what it depends on, since very simple plugins that are based purely on ASF functions are guaranteed to work fine in all setups, as they do not bring any dependencies on their own and have all native dependencies covered by definition. More complicated plugins (especially those that have dependencies on their own) might need to take extra measures to ensure that they indeed provide all required code parts, not only high-level plugin dependencies (described in section above), but low-level native dependencies as well. If all else fails, like above, you can always compile your plugin for the same OS-specific variant that you want to use, and just bundle all the generated dependencies.