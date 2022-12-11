# 外掛程式

從ASF V4版本開始，程式支援可在執行期間載入的自訂外掛程式。 外掛程式使您能夠自訂ASF的行為，例如加入自訂指令、自訂交易邏輯，或與第三方服務或是API進行整體整合。

---

## 給使用者

ASF會從您的ASF資料夾中的&#8203;`plugins`&#8203;資料夾載入外掛程式。 建議為您使用的每個外掛程式提供一個專屬資料夾，該資料夾可以依據外掛程式的名稱命名，例如&#8203;`MyPlugin`&#8203;。 最終會產生&#8203;`plugins/MyPlugin`&#8203;的樹狀結構。 最後，外掛程式的所有二進制檔案都應該放在那個專屬資料夾中，ASF會在重新啟動後成功偵測並使用您的外掛程式。

通常外掛程式的開發人員會以&#8203;`zip`&#8203;檔的形式來發布他們的外掛程式，該檔案會具有已經為您準備好的檔案結構，因此只需將該.zip檔解壓縮至&#8203;`plugins`&#8203;資料夾中，就能自動建立適當的資料夾。

若外掛程式被成功載入，您將會在紀錄中看到它的名稱及版本。 若遇到與您使用的外掛程式相關的問題、或對程式內容或使用方式有疑問時，您應諮詢相應的外掛程式開發人員。

您可以在我們的&#8203;**[第三方工具](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Third-party-zh-TW#asf-外掛程式)**&#8203;章節中找到一些精選的外掛程式。

**請注意，ASF外掛程式可能包含惡意功能**&#8203;。 您應始終確保您所使用的外掛程式來自您可以信任的開發人員。 若您決定使用任何自訂外掛程式，ASF開發人員將不再保證您正常的ASF權益（例如無惡意程式或不被VAC）。 我們亦無法支援使用自訂外掛程式的設定，因為您不再執行原版的ASF程式碼。

---

## 給開發人員

外掛程式是標準的.NET程式庫，繼承了ASF的通用&#8203;`IPlugin`&#8203;介面。 只要API保持相容，您就可以完全獨立於主線ASF來開發外掛程式，並可以在現在及未來的ASF版本中重複使用它們。 ASF使用的外掛程式系統基於&#8203;`System.Composition`&#8203;，前稱&#8203;**[Managed Extensibility Framework](https://learn.microsoft.com/zh-tw/dotnet/framework/mef/)**&#8203;，可以使ASF在執行期間偵測並載入您的程式庫。

---

### 開始使用

我們為您準備了&#8203;**[ASF外掛程式模板](https://github.com/JustArchiNET/ASF-PluginTemplate)**&#8203;，您可以把它當作您外掛程式專案的基礎。 使用模板並非強制性（因為您可以從頭開始建立），但我們強烈建議使用，因為它能夠極大加速您的開發過程，節省各種事情所需的時間。 參閱模板的&#8203;**[README](https://github.com/JustArchiNET/ASF-PluginTemplate/blob/main/README.md)**&#8203;，來進一步了解詳細資訊。 不論如何，若您仍想從頭開始，或希望更理解外掛程式模板裡面所使用的概念，我們也會在接下來介紹相關基礎。

您的專案應該是一個標準.NET程式庫，以您想要的ASF版本為目標來選取適合的Framework，如&#8203;**[編譯](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compilation-zh-TW)**&#8203;中所述。 我們建議您以.NET (Core)作為目標，但.NET Framework外掛程式也是可以使用的。

專案必須引用&#8203;`ArchiSteamFarm`&#8203;主程式集，或您先前下載的發布中所包含的預建置&#8203;`ArchiSteamFarm.dll`&#8203;程式庫，或原始專案（例如您決定將ASF Tree作為子模組）。 這將使您可以存取與檢查ASF的結構、方法及屬性，特別是您接下來將需要繼承的&#8203;`IPlugin`&#8203;核心介面。 專案也必須至少引用&#8203;`System.Composition.AttributedModel`&#8203;，使您能夠&#8203;`[Export]`&#8203;您的&#8203;`IPlugin`&#8203;給ASF使用。 除此之外，您可能還希望／需要引用其他公開程式庫，來解譯在某些介面中提供給您的資料結構，但除非您確實需要它們，否則現在這樣就夠了。

若您所做一切正確，您的&#8203;`csproj`&#8203;應類似於下面：

```csproj
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>net7.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="System.Composition.AttributedModel" IncludeAssets="compile" Version="7.0.0" />
  </ItemGroup>

  <ItemGroup>
    <Reference Include="ArchiSteamFarm" HintPath="C:\\Path\To\Downloaded\ArchiSteamFarm.dll" />

    <!-- 若要作為ASF的Source Tree建置的一部份，使用這個來取代上面的<Reference> -->
    <!-- <ProjectReference Include="C:\\Path\To\ArchiSteamFarm\ArchiSteamFarm.csproj" ExcludeAssets="all" Private="false" /> -->
  </ItemGroup>
</Project>
```

從程式碼方面來說，您的外掛程式類別必須繼承自&#8203;`IPlugin`&#8203;介面（顯式，或透過例如&#8203;`IASF`&#8203;等更專門的介面來進行隱式繼承）與&#8203;`[Export(typeof(IPlugin))]`&#8203;，使ASF能在執行期間進行辨識。 達成這一點最簡單的範例如下：

```csharp
using System;
using System.Composition;
using System.Threading.Tasks;
using ArchiSteamFarm;
using ArchiSteamFarm.Plugins;

namespace YourNamespace.YourPluginName;

[Export(typeof(IPlugin))]
public sealed class YourPluginName : IPlugin {
    public string Name => nameof(YourPluginName);
    public Version Version => typeof(YourPluginName).Assembly.GetName().Version;

    public Task OnLoaded() {
        ASF.ArchiLogger.LogGenericInfo("Meow");

        return Task.CompletedTask;
    }
}
```

為了使用您的外掛程式，您必須先編譯它。 您可以使用您的IDE，或在您的專案的根目錄下執行此命令來編譯：

```shell
# 若您的專案是獨立的（不需要定義它的名稱，因為它是唯一的）
dotnet publish -c "Release" -o "out"

# 若您的專案屬於ASF的Source Tree的一部份（用以防止編譯不需要的部分）
dotnet publish 您外掛程式的名稱 -c "Release" -o "out"
```

在這之後，您的外掛程式就已準備好部署。 如何轉發及發布外掛程式完全取決於您自己，但我們建議建立一個.zip壓縮檔，其中只含有一個名為&#8203;`您的命名空間.您的外掛程式名稱`&#8203;的資料夾，並在裡面放入您已編譯好的外掛程式及它的&#8203;**[相依程式](#外掛程式相依性)**&#8203;。 這樣，使用者只需要將您的.zip解壓縮至他的&#8203;`plugins`&#8203;資料夾中即可，而不需要進行其他動作。

這只是讓您入門的最基礎情境。 我們提供&#8203;**[`外掛程式範例`](https://github.com/JustArchiNET/ArchiSteamFarm/tree/main/ArchiSteamFarm.CustomPlugins.ExamplePlugin)**&#8203;專案，來向您展示您在自己的外掛程式中可以實作的介面與操作的範例，及實用的註解。 若您想從工作碼中學習，或自行探索&#8203;`ArchiSteamFarm.Plugins`&#8203;命名空間，可以隨意閱覽並參考所包含的文件，以了解所有可用的選項。

若您想從真實的專案而不是從範例外掛程式中學習，可以使用我們開發的&#8203;**[`SteamTokenDumper`](https://github.com/JustArchiNET/ArchiSteamFarm/tree/main/ArchiSteamFarm.OfficialPlugins.SteamTokenDumper)**&#8203;外掛程式，該外掛程式附隨在ASF中。 除此之外，還有其他開發者開發的外掛程式，列在我們的&#8203;**[第三方工具](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Third-party-zh-TW#asf-外掛程式)**&#8203;章節中。

---

### API 可用性

除了您可以在介面中存取的內容之外，ASF還向您公開了許多您可以使用的內部API，以便擴展功能。 舉例來說，若您想向Steam網站傳送某種新請求，您不需從頭開始實作所有內容，特別是處理我們先前已處理過的所有問題。 只需使用我們的&#8203;`Bot.ArchiWebHandler`&#8203;，它已經公開了許多&#8203;`UrlWithSession()`&#8203;方法以供您使用，並已為您完成了所有的底層工作，例如身分驗證、連線階段重新整理或處理網路限制等。 同樣地，若要向Steam平台以外傳送Web請求，您可以使用標準的.NET &#8203;`HttpClient`&#8203;類別，但最好還是使用我們提供給您的&#8203;`Bot.ArchiWebHandler.WebBrowser`&#8203;，它能為您提供許多幫助，例如重試失敗的請求。

我們在API可用性的方面採取非常開放的政策，所以若您想使用ASF程式碼中已有的功能，請&#8203;**[提出一個Issue](https://github.com/JustArchiNET/ArchiSteamFarm/issues)**&#8203;，在裡面說明您需要使用的ASF內部API，並解釋您計劃使用的範例情境。 只要您的範例情境有意義，我們不太可能會反對。 我們根本不可能開放全部可以使用的東西，所以我們只能開放對我們來說最有意義的地方，然後等待著您的請求，來防止您需要存取尚未&#8203;`public`&#8203;的部分。 這也包含關於新的&#8203;`IPlugin`&#8203;介面的所有建議，只要用來擴展現有功能時加入它是有意義的。

實際上，ASF的內部API是您外掛程式功能的唯一限制。 因為您的外掛程式也可以擁有自己的相依程式，所以沒有什麼能夠阻止您。例如您可以在您的應用程式中加入&#8203;`Discord.Net`&#8203;程式庫，並在您的Discord Bot及ASF指令間搭起一座橋梁。 外掛程式的可能性是無限的，我們會盡最大努力為您的外掛程式提供盡可能多的自由及靈活性，因此沒有給予任何人為限制。只是我們不能完全確定哪些ASF的部分是在您外掛程式開發中所必需的（您可以經由告訴我們來解決此問題，或是無須通知，您隨時都可以自行實作所需的功能）。

---

### API 相容性

需要特別為您強調，ASF是一個使用者應用程式，而非一個您能無條件依賴具有穩定API介面的程式庫。 這代表您無法假定您的外掛程式一經編譯，就能夠在未來所有的ASF版本中持續運作。若您想進一步開發程式，這將是不可能的，我們無法只為了反向相容性，就放棄去適應不斷變化的Steam。 對您來說這應該合乎邏輯，但強調這一點事實很重要。

我們會盡最大努力，保持ASF公開的部分能夠正常且穩定運作。但如果有足夠的理由，我們不會害怕去破壞相容性，並且在這個過程中，會遵循我們的&#8203;**[棄用](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Deprecation-zh-TW)**&#8203;政策。 這對於作為ASF基礎架構的一部份公開給您的內部ASF結構來說特別重要，如上文所述（例如&#8203;`ArchiWebHandler`&#8203;），在未來某個版本中，它們可能會作為ASF增強的一部份而被改進（或因此而被重寫）。 我們將會盡最大努力在更新日誌中適當通知您，並在執行期間適時顯示與過時功能相關的警告。 我們不會故意為了重寫而重寫，因此您可以相信，下一個ASF次版更新不會只因為版本號碼增加，而讓您的外掛程式完全失效。但仍最好留意更新日誌，並偶爾驗證一切是否正常運作。

---

### 外掛程式相依性

預設情形下，您的外掛程式會至少含有兩個相依程式，&#8203;`ArchiSteamFarm`&#8203;用於內部API，以及&#8203;`System.Composition.AttributedModel`&#8203;的&#8203;`PackageReference`&#8203;，這是被辨識成ASF外掛程式所必需的。 In addition to that, it may include more dependencies in regards to what you've decided to do in your plugin (e.g. `Discord.Net` library if you've decided to integrate with Discord).

The output of your build will include your core `YourPluginName.dll` library, as well as all the dependencies that you've referenced. Since you're developing a plugin to already-working program, you don't have to, and even **shouldn't** include dependencies that ASF already includes, for example `ArchiSteamFarm`, `SteamKit2` or `Newtonsoft.Json`. Stripping down your build off dependencies shared with ASF is not the absolute requirement for your plugin to work, but doing so will dramatically cut the memory footprint and the size of your plugin, together with increasing the performance, due to the fact that ASF will share its own dependencies with you, and will load only those libraries that it doesn't know about itself.

In general, it's a recommended practice to include only those libraries that ASF either doesn't include, or includes in the wrong/incompatible version. Examples of those would be obviously `YourPluginName.dll`, but for example also `Discord.Net.dll` if you decided to depend on it, as ASF doesn't include it itself. Bundling libraries that are shared with ASF can still make sense if you want to ensure API compatibility (e.g. being sure that `Newtonsoft.Json` which you depend on in your plugin will always be in version `X` and not the one that ASF ships with), but obviously doing that comes for a price of increased memory/size and worse performance, and therefore should be carefully evaluated.

If you know that the dependency which you need is included in ASF, you can mark it with `IncludeAssets="compile"` as we showed you in the example `csproj` above. This will tell the compiler to avoid publishing referenced library itself, as ASF already includes that one. Likewise, notice that we reference the ASF project with `ExcludeAssets="all" Private="false"` which works in a very similar way - telling the compiler to not produce any ASF files (as the user already has them). This applies only when referencing ASF project, since if you reference a `dll` library, then you're not producing ASF files as part of your plugin.

If you're confused about above statement and you don't know better, check which `dll` libraries are included in `ASF-generic.zip` package and ensure that your plugin includes only those that are not part of it yet. This will be only `YourPluginName.dll` for the most simple plugins. If you get any issues during runtime in regards to some libraries, include those affected libraries as well. If all else fails, you can always decide to bundle everything.

---

### 本機相依性

Native dependencies are generated as part of OS-specific builds, as there is no .NET runtime available on the host and ASF is running through its own .NET runtime that is bundled as part of OS-specific build. In order to minimize the build size, ASF trims its native dependencies to include only the code that can be possibly reached within the program, which effectively cuts the unused parts of the runtime. This can create a potential problem for you in regards to your plugin, if suddenly you find out yourself in a situation where your plugin depends on some .NET feature that isn't being used in ASF, and therefore OS-specific builds can't execute it properly, usually throwing `System.MissingMethodException` or `System.Reflection.ReflectionTypeLoadException` in the process.

This is never a problem with generic builds, because those are never dealing with native dependencies in the first place (as they have complete working runtime on the host, executing ASF). It's also automatically one solution to the problem, **use your plugin in generic builds exclusively**, but obviously that has its own downside of cutting your plugin from users that are running OS-specific builds of ASF. If you're wondering if your issue is related to native dependencies, you can also use this method for verification, load your plugin in ASF generic build and see if it works. If it does, you have plugin dependencies covered, and it's the native dependencies causing issues.

Unfortunately, we had to make a hard choice between publishing whole runtime as part of our OS-specific builds, and deciding to cut it out of unused features, making the build over 50 MB smaller compared to the full one. We've picked the second option, and it's unfortunately impossible for you to include the missing runtime features together with your plugin. If your project requires access to runtime features that are left out, you have to include full .NET runtime that you depend on, and that means running your plugin together with `generic` ASF flavour. You can't run your plugin in OS-specific builds, as those builds are simply missing a runtime feature that you need, and .NET runtime as of now is unable to "merge" native dependency that you could've provided with our own. Perhaps it'll improve one day in the future, but as of now it's simply not possible.

ASF's OS-specific builds include the bare minimum of additional functionality which is required to run our official plugins. Apart of that being possible, this also slightly extends the surface to extra dependencies required for the most basic plugins. Therefore not all plugins will need to worry about native dependencies to begin with - only those that go beyond what ASF and our official plugins directly need. This is done as an extra, since if we need to include additional native dependencies ourselves for our own use cases anyway, we can as well ship them directly with ASF, making them available, and therefore easier to cover, also for you. Unfortunately, this is not always enough, and as your plugin gets bigger and more complex, the likelihood of running into trimmed functionality increases. Therefore, we usually recommend you to run your custom plugins in `generic` ASF flavour exclusively. You can still manually verify that OS-specific build of ASF has everything that your plugin requires for its functionality - but since that changes on your updates as well as ours, it might be tricky to maintain.