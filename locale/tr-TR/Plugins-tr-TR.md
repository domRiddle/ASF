# Eklentiler

ASF V4 ile başlarken, programın kendisi runtime sırasında özel eklentiler için destek vermeyi içermektedir. Eklentiler ASF'in davranışlarını özelleştirmeye yarar, örnek olarak özel komutlar ekleyerek, özel takas mantığı ekleyerek veya üçüncü parti servisleri ve uygulama programlama arayüzü ile entegrasyon ekleyerek.

* * *

## Kullanıcılar için

ASF eklentileri ASF klasöründe bulunan `plugins` dizininden yükler. Kullanmak istediğiniz her eklenti için, adına bağlı olabilen özel bir dizin tutmanız tavsiye edilen bir uygulamadır.`MyPlugin` gibi. Bunu yapmak, bu dizin yapısıyla sonuçlanacaktır `plugins/MyPlugin`. Son olarak, eklentinin tüm ikili dosyalarını bu belirtilen klasöre koymalısınız, ve ASF yeniden başlatıldıktan sonra eklentinizi doğru bir şekilde keşfedecek ve kullanacaktır.

Genellikle eklenti geliştiricileri, eklentilerini sizin için önceden hazırlanmış bir yapıya sahip olan `zip` dosyası biçiminde yayınlar, bu yüzden bu zip arşivini `plugins` dizinine çıkarmak yeterlidir, bu da uygun klasörü otomatik olarak oluşturur.

Eğer eklenti başarılı bir şekilde yüklendi ise, çıktıda ismini ve versiyonunu göreceksiniz. Kullanmaya karar verdiğiniz eklentilerle ilgili sorularınızı, sorunlarınızı veya kullanıma yönelik şeyleri eklenti geliştiricilerinize danışmalısınız.

Öne çıkan bazı eklentileri **[üçüncü-taraf](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Third-party#asf-plugins)** bölümümüzde bulabilirsiniz.

**Lütfen ASF eklentilerinin kötü amaçlı olabileceğini unutmayın.**. Her zaman güvenebileceğiniz geliştiriciler tarafından yapılan eklentileri kullandığınızdan emin olmalısınız. Herhangi bir özel eklenti kullanmaya karar verirseniz, ASF geliştiricileri artık size normal ASF avantajlarını (kötü amaçlı yazılımın olmaması veya VAC'siz olmama gibi şeyleri) garanti edemez. Ayrıca, ham ASF kodunu çalıştırmadığınız için özel eklentileri kullanan kurulumları da destekleyemiyoruz.

* * *

## Geliştiriciler için

Eklentiler, ASF ile ortak `IPlugin` arayüzünü devralan standart .NET kütüphaneleridir. Eklentileri ana hatları ASF'den tamamen bağımsız olarak geliştirebilir ve API(Uygulama Programlama Arayüzü) uyumlu kaldığı sürece bunları mevcut ve gelecekteki ASF sürümlerinde yeniden kullanabilirsiniz. ASF'de kullanılan eklenti sistemi, daha önce **[Yönetilen Genişletilebilirlik Çerçevesi](https://docs.microsoft.com/dotnet/framework/mef)** olarak bilinen ve ASF'in runtime sırasında kitaplıklarınızı keşfetmesine ve yüklemesine olanak tanıyan `System.Composition` 'a dayanır.

* * *

### Başlarken

**[Derlemede](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compilation)** belirtildiği gibi, projeniz hedef ASF sürümünüzün uygun çerçevesini hedefleyen standart bir .NET kitaplığı olmalıdır. .NET Core'u hedeflemenizi öneririz, ama .NET Framework eklentileri de mevcuttur.

Proje esas `ArchiSteamFarm` birleştirmesine veya sürümün bir parçası olarak indirdiğiniz önceden oluşturulmuş `ArchiSteamFarm.dl ` kitaplığına başvurmalıdır, ya da projenin kaynağına (örneğin. ASF dizin ağacını alt modül olarak eklemeye karar verdiyseniz). Bu, ASF yapılarına, metodlarına ve özelliklerine, özellikle bir sonraki adımda devralmanız gereken çekirdek `IPlugin` arayüzüne erişmenize ve keşfetmenize olanak tanır. Proje aynı zamanda minimum olarak `System.Composition.AttributedModel`i referans almalıdır; bu, ASF'in kullanılması için `[Export]` `IPlugin`' i sağlar. Ek olarak, bazı arayüzlerde size verilen veri yapılarını yorumlamak için diğer ortak kütüphanelere başvurmak isteyebilir/ihtiyaç duyabilirsiniz, ancak bunlara açıkça ihtiyaç duymadığınız sürece şimdilik yeterli olacaktır.

Eğer her şeyi doğru bir şekilde yaptıysanız, `csproj` dosyanız aşağıdakine benzer olacaktır:

```csproj
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>net5.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="System.Composition.AttributedModel" IncludeAssets="compile" Version="*" />
  </ItemGroup>

  <ItemGroup>
    <Reference Include="ArchiSteamFarm" HintPath="C:\\Path\To\Downloaded\ArchiSteamFarm.dll" />

    <!-- 
ASF kaynak ağacının bir parçası olarak oluşturuyorsanız, yukarıdakiler yerine bunu kullanın <Reference> yukarıdakiler -->
    <!-- <ProjectReference Include="C:\\Path\To\ArchiSteamFarm\ArchiSteamFarm.csproj" ExcludeAssets="all" Private="false" /> -->
  </ItemGroup>
</Project>
```

Kod açısından, eklenti sınıfınız `IPlugin` arayüzünden (açık veya dolaylı olarak `IASF` gibi daha özel bir arabirimden devralarak) ve `[Export (typeof (IPlugin))]`, runtime sırasında ASF tarafından tanınmak için. Bunu başaran en açık örnek şudur:

```csharp
using System;
using System.Composition;
using ArchiSteamFarm;
using ArchiSteamFarm.Plugins;

namespace CalismaAlaniIsmi.EklentiIsminiz {
    [Export(typeof(IPlugin))]
    public sealed class EklentiIsminiz : IPlugin {
        public string Name => nameof(EklentiIsminiz);
        public Version Version => typeof(EklentiIsminiz).Assembly.GetName().Version;

        public void OnLoaded() {
            ASF.ArchiLogger.LogGenericInfo("Meow");
        }
    }
}
```

Eklentinizi kullanmak için öncelikle onu derlemelisiniz. Bunu IDE'nizden veya projenizin kök dizininden bir komutla yapabilirsiniz:

```shell
# Projeniz tek başına çalışan bileşen ise (tek proje olduğu için adını tanımlamanıza gerek yoktur.)
dotnet publish -c "Release" -o "out"
# Projeniz ASF kaynak ağacının parçasıysa (gereksiz parçaları derlemekten kaçınmak için)
dotnet publish YourPluginName -c "Release" -o "out"
```

Daha sonra eklentiniz dağıtım için hazırdır. Eklentinizi tam olarak nasıl dağıtmak ve yayınlamak istediğiniz size kalmış, ancak `Calisma AlaniIsmi.EklentiIsminiz` adlı tek bir klasörle bir zip arşivi oluşturmanızı öneririz.**[Gereksinimler](#plugin-dependencies)**. Bu şekilde kullanıcının zip arşivinizi `plugins` dizinine açması yeterli olacaktır.

Bu, başlamanıza yardımcı olacak en basit senaryodur. Yardımcı yorumlar da dahil olmak üzere kendi eklentiniz içinde yapabileceğiniz örnek arayüzleri ve eylemleri gösteren **[`OrnekEklenti`](https://github.com/JustArchiNET/ArchiSteamFarm/tree/main/ArchiSteamFarm.CustomPlugins.ExamplePlugin)** projemiz var. Çalışan bir koddan öğrenmek isterseniz göz atmaya çekinmeyin, veya `ArchiSteamFarm.Plugins` ad alanını kendiniz keşfedin ve mevcut tüm seçenekler için birlikte verilen belgelere bakın.

Örnek eklentiler yerine gerçek projelerden öğrenmek istiyorsanız, bizim tarafımızdan geliştirilen **[`SteamTokenDumper`](https://github.com/JustArchiNET/ArchiSteamFarm/tree/main/ArchiSteamFarm.OfficialPlugins.SteamTokenDumper)** eklentisi var, ASF ile birlikte paketlenmiş bir eklenti. Buna ek olarak, **[üçüncü-taraf](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Third-party#asf-plugins)** bölümümüzde diğer geliştiriciler tarafından geliştirilen eklentiler de var.

* * *

### API kullanılabilirliği

ASF, arabirimlerde eriştiğiniz şeylerin yanı sıra, işlevselliği genişletmek için kullanabileceğiniz birçok dahili API'yi size sunar. Örneğin, Steam web'e bir tür yeni istek göndermek istiyorsanız, her şeyi en baştan uygulamanıza gerek yoktur, özellikle de sizden uğraşmak zorunda olduğumuz tüm sorunlarla. Sizin için kimlik doğrulama, oturum yenileme veya web sınırlama gibi daha düşük seviyeli şeyleri idare ederek, kullanmanız için halihazırda birçok `UrlWithSession()` yöntemini açığa çıkaran `Bot.ArchiWebHandler` kullanın. Benzer şekilde, Steam platformu dışında web istekleri göndermek için standart .NET `HttpClient` sınıfını kullanabilirsiniz, ancak sizin için mevcut olan `Bot.ArchiWebHandler.WebBrowser`'i kullanmak çok daha iyi bir fikirdir, bu da size bir kez daha yardımcı bir el sunar, örneğin gönderdiğiniz başarısız istekleri yeniden denemesi gibi.

API kullanılabilirliğimiz açısından çok açık bir politikamız var, bu nedenle ASF kodunun zaten içerdiği bir şeyden yararlanmak istiyorsanız, **[bir sorun açın](https://github.com/JustArchiNET/ArchiSteamFarm/issues)** ve planlanan kullanım durumunuzu açıklayın. ASF'in dahili API'si dahilinde. Kullanım durumunuz mantıklı olduğu sürece, büyük olasılıkla buna karşı bir şeyimiz olmayacak. Birinin kullanmak istediği her şeyi açmamız imkansız, bu yüzden bizim için en mantıklı olanı açtık ve henüz `herkese açık olmayan` bir şeye erişmek istemeniz durumunda isteklerinizi bekliyoruz. Bu aynı zamanda, mevcut işlevselliği genişletmek için eklenmesi anlamlı olabilecek yeni `IPlugin` arabirimleriyle ilgili tüm önerileri de içerir.

Aslında, dahili ASF'in API'si, eklentinizin yapabilecekleri açısından tek gerçek sınırlamadır. Örnek olarak sizi hiçbir şey uygulamanıza `Discord.Net` kitaplığı dahil etmek ve Discord botunuz ile ASF komutları arasında bir köprü oluşturmaktan alı koyamaz, çünkü eklentinizin kendi başına gereksinimleri olabilir. İmkanlar sonsuzdur, ve eklentiniz dahilinde size olabildiğince fazla özgürlük ve esneklik sağlamak için elimizden gelenin en iyisini yaptık. yani hiçbir şeyde yapay bir sınır yok, sadece eklenti geliştirmeniz için hangi ASF parçalarının çok önemli olduğundan tam olarak emin değiliz (bunu bize bildirerek çözebilirsiniz ve bu olmadan bile ihtiyacınız olan işlevselliği her zaman yeniden uygulayabilirsiniz).

* * *

### API uyumluluğu

ASF'in bir tüketici uygulaması olduğunu ve koşulsuz olarak güvenebileceğiniz sabit API yüzeyine sahip tipik bir kitaplık olmadığını vurgulamak önemlidir. Bu, eklentinizin derlendikten sonra gelecekteki tüm ASF sürümleriyle çalışmaya devam edeceğini varsayamayacağınız anlamına gelir. Programı daha da geliştirmeye devam etmek istiyorsanız ve geriye dönük uyumluluk uğruna sürekli devam eden Steam değişikliklerine uyum sağlayamamak bizim durumumuz için uygun değildir. Bu sizin için mantıklı olmalı, ama bu gerçeği vurgulamak önemli.

ASF'in herkese açık kısımlarını çalışır durumda ve istikrarlı tutmak için elimizden gelenin en iyisini yapacağız, ancak süreçte **[kullanımdan kaldırma](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Deprecation)** politikamızı izleyerek yeterince iyi nedenler ortaya çıkarsa uyumluluğu bozmaktan korkmayacağız. Bu, ASF altyapısının bir parçası olarak size maruz kalan ve yukarıda açıklanan(örneğin `ArchiWebHandler`) ve ASF geliştirmelerinin bir parçası olarak iyileştirilebilen (ve dolayısıyla yeniden yazılan) dahili ASF yapıları açısından özellikle önemlidir gelecek sürümlerin bir parçası olarak. Değişiklik çıktılarında sizi uygun şekilde bilgilendirmek için elimizden gelenin en iyisini yapacağız ve eski özellikler hakkında çalışma süresi boyunca uygun uyarıları dahil edeceğiz. Yeniden yazma uğruna her şeyi yeniden yazma niyetinde değiliz, bu nedenle bir sonraki küçük ASF sürümünün yalnızca daha yüksek bir sürüm numarasına sahip olduğu için eklentinizi tamamen yok etmeyeceğinden emin olabilirsiniz, ancak değişiklik çıktılarına göz atıp her şeyin yolunda olup olmadığını kontrol etmek iyi bir fikirdir.

* * *

### Eklenti gereksinimleri

Your plugin will include at least two dependencies by default, `ArchiSteamFarm` reference for internal API, and `PackageReference` of `System.Composition.AttributedModel` that is required for being recognized as ASF plugin to begin with. In addition to that, it may include more dependencies in regards to what you've decided to do in your plugin (e.g. `Discord.Net` library if you've decided to integrate with Discord).

The output of your build will include your core `YourPluginName.dll` library, as well as all the dependencies that you've referenced. Since you're developing a plugin to already-working program, you don't have to, and even **shouldn't** include dependencies that ASF already includes, for example `ArchiSteamFarm`, `SteamKit2` or `Newtonsoft.Json`. Stripping down your build off dependencies shared with ASF is not the absolute requirement for your plugin to work, but doing so will dramatically cut the memory footprint and the size of your plugin, together with increasing the performance, due to the fact that ASF will share its own dependencies with you, and will load only those libraries that it doesn't know about itself.

In general, it's a recommended practice to include only those libraries that ASF either doesn't include, or includes in the wrong/incompatible version. Examples of those would be obviously `YourPluginName.dll`, but for example also `Discord.Net.dll` if you decided to depend on it, as ASF doesn't include it itself. Bundling libraries that are shared with ASF can still make sense if you want to ensure API compatibility (e.g. being sure that `Newtonsoft.Json` which you depend on in your plugin will always be in version `X` and not the one that ASF ships with), but obviously doing that comes for a price of increased memory/size and worse performance, and therefore should be carefully evaluated.

If you know that the dependency which you need is included in ASF, you can mark it with `IncludeAssets="compile"` as we showed you in the example `csproj` above. This will tell the compiler to avoid publishing referenced library itself, as ASF already includes that one. Likewise, notice that we reference the ASF project with `ExcludeAssets="all" Private="false"` which works in a very similar way - telling the compiler to not produce any ASF files (as the user already has them). This applies only when referencing ASF project, since if you reference a `dll` library, then you're not producing ASF files as part of your plugin.

If you're confused about above statement and you don't know better, check which `dll` libraries are included in `ASF-generic.zip` package and ensure that your plugin includes only those that are not part of it yet. This will be only `YourPluginName.dll` for the most simple plugins. If you get any issues during runtime in regards to some libraries, include those affected libraries as well. If all else fails, you can always decide to bundle everything.

* * *

### Yerel gereksinimler

Native dependencies are generated as part of OS-specific builds, as there is no .NET Core runtime available on the host and ASF is running through its own .NET Core runtime that is bundled as part of OS-specific build. In order to minimize the build size, ASF trims its native dependencies to include only the code that can be possibly reached within the program, which effectively cuts the unused parts of the runtime. This can create a potential problem for you in regards to your plugin, if suddenly you find out yourself in a situation where your plugin depends on some .NET Core feature that isn't being used in ASF, and therefore OS-specific builds can't execute it properly, usually throwing `System.MissingMethodException` or `System.Reflection.ReflectionTypeLoadException` in the process.

This is never a problem with generic builds, because those are never dealing with native dependencies in the first place (as they have complete working runtime on the host, executing ASF). It's also automatically one solution to the problem, **use your plugin in generic builds exclusively**, but obviously that has its own downside of cutting your plugin from users that are running OS-specific builds of ASF. If you're wondering if your issue is related to native dependencies, you can also use this method for verification, load your plugin in ASF generic build and see if it works. If it does, you have plugin dependencies covered, and it's the native dependencies causing issues.

Unfortunately, we had to make a hard choice between publishing whole runtime as part of our OS-specific builds, and deciding to cut it out of unused features, making the build over 50 MB smaller compared to the full one. We've picked the second option, and it's unfortunately impossible for you to include the missing runtime features together with your plugin. If your project requires access to runtime features that are left out, you have to include full .NET runtime that you depend on, and that means running your plugin together with `generic` ASF flavour. You can't run your plugin in OS-specific builds, as those builds are simply missing a runtime feature that you need, and .NET Core runtime as of now is unable to "merge" native dependency that you could've provided with our own. Perhaps it'll improve one day in the future, but as of now it's simply not possible.

ASF's OS-specific builds include the bare minimum of additional functionality which is required to run our official plugins. Apart of that being possible, this also slightly extends the surface to extra dependencies required for the most basic plugins. Therefore not all plugins will need to worry about native dependencies to begin with - only those that go beyond what ASF and our official plugins directly need. This is done as an extra, since if we need to include additional native dependencies ourselves for our own use cases anyway, we can as well ship them directly with ASF, making them available, and therefore easier to cover, also for you.