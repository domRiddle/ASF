# 紀錄日誌

ASF允許您自訂執行期間使用的紀錄日誌模組。 您可以將叫做&#8203;`NLog.config`&#8203;的特定檔案存放至應用程式的資料夾中來達成自訂。 您可以在&#8203;**[NLog Wiki](https://github.com/NLog/NLog/wiki/Configuration-file)**&#8203;中閱讀完整的文件，但除此之外，您也可以在這裡找到一些有用的範例。

---

## 預設紀錄

預設情形下，ASF會記錄於&#8203;`ColoredConsole`&#8203;（標準輸出）及&#8203;`File`中。 `File`&#8203;紀錄包含在程式資料夾中的&#8203;`log.txt`&#8203;檔案及用於歸檔的&#8203;`logs`&#8203;資料夾中。

使用自訂NLog設定會自動停用預設的ASF設定，您的設定會&#8203;**完全**&#8203;覆寫預設的ASF紀錄，這代表例如您想要保留我們的&#8203;`ColoredConsole`&#8203;目標，就必須&#8203;**自行定義**&#8203;。 這使您不只能夠新增&#8203;**額外的**&#8203;紀錄目標，還可以停用或修改&#8203;**預設的**&#8203;目標。

若您想要使用不受任何修改的預設ASF紀錄，就什麼也不需要做⸺您也不需要在自訂的&#8203;`NLog.config`&#8203;中定義它。 若您不想要修改預設ASF紀錄，就不要使用自訂的&#8203;`NLog.config`&#8203;。 不過做為參考，硬編碼的ASF預設紀錄等同於：

```xml
<?xml version="1.0" encoding="utf-8" ?>
<nlog xmlns="https://nlog-project.org/schemas/NLog.xsd" xsi:schemaLocation="NLog NLog.xsd" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <targets>
    <target xsi:type="ColoredConsole" name="ColoredConsole" layout="${date:format=yyyy-MM-dd HH\:mm\:ss}|${processname}-${processid}|${level:uppercase=true}|${logger}|${message}${onexception:inner= ${exception:format=toString,Data}}" />
    <target xsi:type="File" name="File" archiveFileName="${currentdir}/logs/log.{#}.txt" archiveNumbering="Rolling" archiveOldFileOnStartup="true" cleanupFileName="false" concurrentWrites="false" deleteOldFileOnStartup="true" fileName="${currentdir}/log.txt" layout="${date:format=yyyy-MM-dd HH\:mm\:ss}|${processname}-${processid}|${level:uppercase=true}|${logger}|${message}${onexception:inner= ${exception:format=toString,Data}}" maxArchiveFiles="10" />

    <!-- 在ASF的IPC介面啟動時，下列將變為活動狀態 -->
    <target type="History" name="History" layout="${date:format=yyyy-MM-dd HH\:mm\:ss}|${processname}-${processid}|${level:uppercase=true}|${logger}|${message}${onexception:inner= ${exception:format=toString,Data}}" maxCount="20" />
  </targets>

  <rules>
    <!-- 下列項指定了ASP.NET（IPC）紀錄，我們宣告它們，因此我們最頂層的Debug catch-all在預設情形下不包含ASP.NET紀錄 -->
    <logger name="Microsoft*" finalMinLevel="Warn" writeTo="ColoredConsole" />
    <logger name="Microsoft.Hosting.Lifetime*" finalMinLevel="Info" writeTo="ColoredConsole" />
    <logger name="System*" finalMinLevel="Warn" writeTo="ColoredConsole" />

    <logger name="*" minlevel="Debug" writeTo="ColoredConsole" />

    <!-- 下列項指定了ASP.NET（IPC）紀錄，我們宣告它們，因此我們最頂層的Debug catch-all在預設情形下不包含ASP.NET紀錄 -->
    <logger name="Microsoft*" finalMinLevel="Warn" writeTo="File" />
    <logger name="Microsoft.Hosting.Lifetime*" finalMinLevel="Info" writeTo="File" />
    <logger name="System*" finalMinLevel="Warn" writeTo="File" />

    <logger name="*" minlevel="Debug" writeTo="File" />

    <!-- 在ASF的IPC介面啟用時，下列將變為活動狀態 -->
    <!-- 下列項指定了ASP.NET（IPC）紀錄，我們宣告它們，因此我們最頂層的Debug catch-all在預設情形下不包含ASP.NET紀錄 -->
    <logger name="Microsoft*" finalMinLevel="Warn" writeTo="History" />
    <logger name="Microsoft.Hosting.Lifetime*" finalMinLevel="Info" writeTo="History" />
    <logger name="System*" finalMinLevel="Warn" writeTo="History" />

    <logger name="*" minlevel="Debug" writeTo="History" />
  </rules>
</nlog>
```

---

## ASF 整合

ASF含有一些不錯的程式碼技巧，可以增強與NLog的整合，使您可以更輕鬆地抓取特定訊息。

NLog特定的&#8203;`${logger}`&#8203;變數將始終用來識別訊息來源⸺可以是您的Bot之一的&#8203;`BotName`&#8203;；若訊息直接來自ASF程序，也可以是&#8203;`ASF`&#8203;。 透過這種方式，您可以依據記錄器的名稱輕鬆抓取特定Bot或（只抓取）ASF程序的訊息，而不是全部。

ASF會嘗試依據NLog提供的記錄級別適當地標示訊息，使您可以只抓取來自特定記錄級別的特定訊息，而不是全部。 當然，特定訊息的記錄級別無法自訂，因為這是ASF硬編碼所決定的訊息嚴重程度，但您仍絕對可以使ASF更加／更少的「沉默」，來符合您的需求。

ASF也會記錄額外資訊，例如在&#8203;`Trace`&#8203;的記錄級別中就包含使用者使用者／聊天訊息。 預設的ASF紀錄只記錄了&#8203;`Debug`&#8203;級別及以上的訊息，它隱藏了額外訊息，因為大多數使用者並不需要它們，況且這些雜項輸出可能會掩蓋掉重要訊息。 但是您可以透過重新啟用&#8203;`Trace`&#8203;記錄級別來使用該訊息，特別是您只需要記錄某個特定Bot及您感興趣的特定事件的時候。

一般來說，ASF試圖讓您盡可能地簡單且方便，只記錄您想要的訊息，而不是強迫您透過&#8203;`grep`&#8203;等第三方工具來手動篩選它。 只需依照下列說明正確地設定NLog，您就應該能夠使用自訂目標（例如整個資料庫）指定的非常複雜的記錄規則。

關於版本控制⸺ASF嘗試始終在發布時提供當時&#8203;**[NuGet](https://www.nuget.org/packages/NLog)**&#8203;上最新版的NLog。 您應該能夠使用所有您能在NLog Wiki上找到的ASF功能⸺只需要確保您也使用最新版的ASF。

作為ASF整合的一部份，ASF也支援包含其他ASF NLog紀錄的目標，這將在下列說明。

---

## 範例

讓我們從簡單的地方開始。 我們先只使用&#8203;**[ColoredConsole](https://github.com/nlog/nlog/wiki/ColoredConsole-target)**&#8203;目標。 我們初始的&#8203;`NLog.config`&#8203;看起來像這樣：

```xml
<?xml version="1.0" encoding="utf-8" ?>
<nlog xmlns="https://nlog-project.org/schemas/NLog.xsd" xsi:schemaLocation="NLog NLog.xsd" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <targets>
    <target xsi:type="ColoredConsole" name="ColoredConsole" />
  </targets>

  <rules>
    <logger name="*" minlevel="Debug" writeTo="ColoredConsole" />
  </rules>
</nlog>
```

上述設定的解釋十分簡單：我們定義一個&#8203;**記錄目標**&#8203;，為&#8203;`ColoredConsole`&#8203;，然後我們將&#8203;`Debug`&#8203;及更高級別的&#8203;**所有記錄器**&#8203;（&#8203;`*`&#8203;）重新導向至先前定義的&#8203;`ColoredConsole`&#8203;目標。 就是這樣。

若您現在以上述的&#8203;`NLog.config`&#8203;啟動ASF，只有&#8203;`ColoredConsole`&#8203;目標會被啟用，且不論ASF硬編碼的NLog設定為何，ASF都不會寫入&#8203;`File`&#8203;。

現在，假設我們不喜歡預設格式&#8203;`${longdate}|${level:uppercase=true}|${logger}|${message}`&#8203;，我們只想記錄訊息。 我們可以透過修改目標的&#8203;**[布局](https://github.com/nlog/nlog/wiki/Layouts)**&#8203;來做到。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<nlog xmlns="https://nlog-project.org/schemas/NLog.xsd" xsi:schemaLocation="NLog NLog.xsd" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <targets>
    <target xsi:type="ColoredConsole" name="ColoredConsole" layout="${message}" />
  </targets>

  <rules>
    <logger name="*" minlevel="Debug" writeTo="ColoredConsole" />
  </rules>
</nlog>
```

若您現在啟動ASF，就會注意到日期、級別及記錄器名稱都消失了⸺只留下格式為&#8203;`Function() Message`&#8203;的ASF訊息。

我們還可以修改設定以記錄多個目標。 讓我們同時記錄到&#8203;`ColoredConsole`&#8203;和&#8203;**[File](https://github.com/nlog/nlog/wiki/File-target)**&#8203;中。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<nlog xmlns="https://nlog-project.org/schemas/NLog.xsd" xsi:schemaLocation="NLog NLog.xsd" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <targets>
    <target xsi:type="ColoredConsole" name="ColoredConsole" />
    <target xsi:type="File" name="File" fileName="${currentdir}/NLog.txt" deleteOldFileOnStartup="true" />
  </targets>

  <rules>
    <logger name="*" minlevel="Debug" writeTo="ColoredConsole" />
    <logger name="*" minlevel="Debug" writeTo="File" />
  </rules>
</nlog>
```

現在，我們已將全部內容記錄到&#8203;`ColoredConsole`&#8203;和&#8203;`File`&#8203;中了。 您是否注意到您還可以指定自訂的&#8203;`fileName`&#8203;與額外選項呢？

最後，ASF使用著各種記錄級別，讓您更容易理解發生的事情。 我們可以利用它，來修改紀錄的嚴重程度。 假設我們要將所有訊息（&#8203;`Trace`&#8203;）記錄至&#8203;`File`中，但只記錄&#8203;`Warning`&#8203;及更高的&#8203;**[訊息級別](https://github.com/NLog/NLog/wiki/Configuration-file#log-levels)**&#8203;至&#8203;`ColoredConsole`&#8203;。 我們可以修改&#8203;`rules`&#8203;來達成：

```xml
<?xml version="1.0" encoding="utf-8" ?>
<nlog xmlns="https://nlog-project.org/schemas/NLog.xsd" xsi:schemaLocation="NLog NLog.xsd" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <targets>
    <target xsi:type="ColoredConsole" name="ColoredConsole" />
    <target xsi:type="File" name="File" fileName="${currentdir}/NLog.txt" deleteOldFileOnStartup="true" />
  </targets>

  <rules>
    <logger name="*" minlevel="Warn" writeTo="ColoredConsole" />
    <logger name="*" minlevel="Trace" writeTo="File" />
  </rules>
</nlog>
```

就是這樣，現在我們的&#8203;`ColoredConsole`&#8203;將只會顯示警告及更高級別的訊息，但同時仍將所有訊息記錄到&#8203;`File`&#8203;中。 您還可以進一步調整它，例如只記錄&#8203;`Info`&#8203;及更低級別等等。

最後，我們來做一些更進階的操作，將所有來自名為&#8203;`LogBot`&#8203;的Bot的訊息記錄至檔案中。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<nlog xmlns="https://nlog-project.org/schemas/NLog.xsd" xsi:schemaLocation="NLog NLog.xsd" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <targets>
    <target xsi:type="ColoredConsole" name="ColoredConsole" />
    <target xsi:type="File" name="LogBotFile" fileName="${currentdir}/LogBot.txt" deleteOldFileOnStartup="true" />
  </targets>

  <rules>
    <logger name="*" minlevel="Debug" writeTo="ColoredConsole" />
    <logger name="LogBot" minlevel="Trace" writeTo="LogBotFile" />
  </rules>
</nlog>
```

您可以看到我們如何使用上述的ASF整合，並依據&#8203;`${logger}`&#8203;屬性輕鬆區分訊息來源。

---

## 進階用法

上述範例非常簡單，向您展示了定義您自己的ASF日誌記錄規則是多麼的容易。 您可以使用NLog來做到各種不同的事情，包括複雜目標（例如將紀錄儲存至&#8203;`Database`&#8203;中）、紀錄輪替（例如移除舊的&#8203;`File`&#8203;紀錄）、使用自訂&#8203;`Layout`&#8203;、宣告您自己的&#8203;`<when>`&#8203;紀錄篩選器等等。 我建議您閱讀整個&#8203;**[NLog文件](https://github.com/nlog/nlog/wiki/Configuration-file)**&#8203;，了解每個可用選項，使您能夠以所需的方式來調整ASF紀錄日誌模組。 這是一個非常強大的工具，自訂ASF紀錄日誌從未如此簡單。

---

## 限制

在需要使用者輸入時，ASF將暫時停用包含&#8203;`ColoredConsole`&#8203;或&#8203;`Console`&#8203;目標的&#8203;**所有**&#8203;規則。 因此，若您希望在ASF等待使用者輸入時繼續記錄其他目標，您應該使用自己的規則來定義這些目標，如上述範例所示，而不是將很多目標放在相同規則的&#8203;`writeTo`&#8203;中（除非這就是您想要的行為）。 臨時停用控制台目標是為了在等待使用者輸入時保持控制台乾淨。

---

## 聊天紀錄

ASF包含對聊天紀錄的延伸支援，不只在&#8203;`Trace`&#8203;記錄級別中記錄所有發送／接收的訊息，還會在&#8203;**[事件屬性](https://github.com/NLog/NLog/wiki/EventProperties-Layout-Renderer)**&#8203;中暴露與它們相關的額外資訊。 這是因為我們不論如何都需要將聊天訊息作為指令處理，因此記錄這些事件並不會增加任何處理成本，但您可以因此來加入額外的邏輯（例如將ASF當作您的個人Steam聊天存檔）。

### 事件屬性

| 名稱          | 描述                                                                                             |
| ----------- | ---------------------------------------------------------------------------------------------- |
| Echo        | `bool`&#8203;型別。 當訊息由我們發送給收件人時設定為&#8203;`true`&#8203;，否則為&#8203;`false`&#8203;。                |
| Message     | `string`&#8203;型別。 這是實際發送／接收的訊息。                                                               |
| ChatGroupID | `ulong`&#8203;型別。 這是發送／接收訊息的群組聊天ID。 若訊息不是經由群組聊天傳輸則為&#8203;`0`&#8203;。                          |
| ChatID      | `ulong`&#8203;型別。 這是發送／接收訊息的&#8203;`ChatGroupID`&#8203;頻道ID。 若訊息不是經由群組聊天傳輸則為&#8203;`0`&#8203;。 |
| SteamID     | `ulong`&#8203;型別。 這是發送／接收訊息的Steam使用者ID。 在沒有特定使用者參與訊息傳輸時（例如我們傳送訊息給群組聊天），可為&#8203;`0`&#8203;。    |

### 範例

本範例基於上述的&#8203;`ColoredConsole`&#8203;基本範例。 在嘗試理解它之前，我強烈建議您先閱讀&#8203;**[上文](#範例)**&#8203;，來了解NLog紀錄的基礎。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<nlog xmlns="https://nlog-project.org/schemas/NLog.xsd" xsi:schemaLocation="NLog NLog.xsd" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <targets>
    <target xsi:type="ColoredConsole" name="ColoredConsole" />
    <target xsi:type="File" name="ChatLogFile" fileName="${currentdir}/${event-properties:item=ChatGroupID}-${event-properties:item=ChatID}${when:when='${event-properties:item=ChatGroupID}' == 0:inner=-${event-properties:item=SteamID}}.txt" layout="${date:format=yyyy-MM-dd HH\:mm\:ss} ${event-properties:item=Message} ${when:when='${event-properties:item=Echo}' == true:inner=-&gt;:else=&lt;-} ${event-properties:item=SteamID}" />
  </targets>

  <rules>
    <logger name="*" minlevel="Debug" writeTo="ColoredConsole" />
    <logger name="MainAccount" level="Trace" writeTo="ChatLogFile">
      <filters defaultAction="Log">
        <when condition="not starts-with('${message}','OnIncoming') and not starts-with('${message}','SendMessage')" action="Ignore" />
      </filters>
    </logger>
  </rules>
</nlog>
```

我們以基本的&#8203;`ColoredConsole`&#8203;為例，並在之後對其延伸。 首先，我們為每個群組頻道及Steam使用者準備了一個永久的聊天紀錄檔案⸺這要歸功於ASF向我們公開額外屬性。 我們還決定使用一種自訂布局，只寫入當前日期、訊息、發送／接收資訊及Steam使用者本身。 最後，我們啟用的聊天紀錄規則只適用於&#8203;`Trace`&#8203;級別、&#8203;`MainAccount`&#8203; Bot及聊天紀錄相關的函數（用於接收訊息的&#8203;`OnIncoming*`&#8203;與發送ASF訊息的&#8203;`SendMessage*`&#8203;）。

上述範例會在與&#8203;**[ArchiBot](https://steamcommunity.com/profiles/76561198069026042)**&#8203;交談時生成&#8203;`0-0-76561198069026042.txt`&#8203;檔案：

```text
2018-07-26 01:38:38 how are you doing? -> 76561198069026042
2018-07-26 01:38:38 I'm doing great, how about you? <- 76561198069026042
```

當然，這只是一個可以使用的範例，以實際方式展示了一些不錯的布局技巧。 您可以將這個方法進一步延伸至您自己的需要，例如額外篩選器、自訂順序、個人布局、只記錄收到的訊息等。

---

## ASF 目標

除了標準的NLog記錄目標（例如上述解釋的&#8203;`ColoredConsole`&#8203;及&#8203;`File`&#8203;），您還可以使用自訂ASF記錄目標。

為了最大程度的完整性，ASF目標將遵循NLog文件的約定。

---

### SteamTarget

正如您所猜到的，這個目標使用Steam聊天訊息來記錄ASF訊息。 您可以將它設定成使用群組聊天或私人聊天。 除了為您的訊息指定Steam目標以外，您還可以指定發送這些消息的Bot的&#8203;`botName`&#8203;。

在所有ASF使用的環境中支援。

---

#### 組態設定語法
```xml
<targets>
  <target type="Steam"
          name="String"
          layout="Layout"
          chatGroupID="Ulong"
          steamID="Ulong"
          botName="Layout" />
</targets>
```

閱讀有關使用&#8203;[設定檔](https://github.com/NLog/NLog/wiki/Configuration-file)的更多資訊。

---

#### 參數

##### 一般選項
_name_&#8203;：目標的名稱。

---

##### 布局選項
_layout_&#8203;：要呈現的本文。 必須為&#8203;[布局](https://github.com/NLog/NLog/wiki/Layouts)&#8203;。 預設值：&#8203;`${level:uppercase=true}|${logger}|${message}`

---

##### SteamTarget 選項

_chatGroupID_&#8203;：以64位元無號長整數宣告的群組聊天ID。 非必須項。 預設為&#8203;`0`&#8203;，這將停用群組聊天功能並改用私人聊天。 啟用時（設定成非0的值），下面的&#8203;`steamID`&#8203;屬性會表現成&#8203;`chatID`&#8203;，且Bot會向指定的&#8203;`chatGroupID`&#8203;頻道ID發送訊息。

_steamID_ - SteamID declared as 64-bit long unsigned integer of target Steam user (like `SteamOwnerID`), or target `chatID` (when `chatGroupID` is set). Required. Defaults to `0` which disables logging target entirely.

_botName_ - Name of the bot (as it's recognized by ASF, case-sensitive) that will be sending messages to `steamID` declared above. 非必要。 Defaults to `null` which will automatically select **any** currently connected bot. It's recommended to set this value appropriately, as `SteamTarget` does not take into account many Steam limitations, such as the fact that you must have `steamID` of the target on your friendlist. This variable is defined as [layout](https://github.com/NLog/NLog/wiki/Layouts) type, therefore you can use special syntax in it, such as `${logger}` in order to use the bot that generated the message.

---

#### SteamTarget 範例

In order to write all messages of `Debug` level and above, from bot named `MyBot` to steamID of `76561198006963719`, you should use `NLog.config` similar to below:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<nlog xmlns="https://nlog-project.org/schemas/NLog.xsd" xsi:schemaLocation="NLog NLog.xsd" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <targets>
    <target type="Steam" name="Steam" steamID="76561198006963719" botName="MyBot" />
  </targets>

  <rules>
    <logger name="*" minlevel="Debug" writeTo="Steam" />
  </rules>
</nlog>
```

**Notice:** Our `SteamTarget` is custom target, so you should make sure that you're declaring it as `type="Steam"`, NOT `xsi:type="Steam"`, as xsi is reserved for official targets supported by NLog.

When you launch ASF with `NLog.config` similar to above, `MyBot` will start messaging `76561198006963719` Steam user with all usual ASF log messages. Keep in mind that `MyBot` must be connected in order to send messages, so all initial ASF messages that happened before our bot could connect to Steam network, won't be forwarded.

Of course, `SteamTarget` has all typical functions that you could expect from generic `TargetWithLayout`, so you can use it in conjunction with e.g. custom layouts, names or advanced logging rules. The example above is only the most basic one.

---

#### 擷圖

![擷圖](https://i.imgur.com/5juKHMt.png)

---

### HistoryTarget

This target is used internally by ASF for providing fixed-size logging history in `/Api/NLog` endpoint of **[ASF API](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#asf-api)** that can be afterwards consumed by ASF-ui and other tools. In general you should define this target only if you're already using custom NLog config for other customizations and you also want the log to be exposed in ASF API, e.g. for ASF-ui. It can also be declared when you'd want to modify default layout or `maxCount` of saved messages.

Supported in all environments used by ASF.

---

#### 組態設定語法
```xml
<targets>
  <target type="History"
          name="String"
          layout="Layout"
          maxCount="Byte" />
</targets>
```

Read more about using the [Configuration File](https://github.com/NLog/NLog/wiki/Configuration-file).

---

#### 參數

##### 一般選項
_name_ - Name of the target.

---

##### 布局選項
_layout_ - Text to be rendered. [Layout](https://github.com/NLog/NLog/wiki/Layouts) Required. Default: `${date:format=yyyy-MM-dd HH\:mm\:ss}|${processname}-${processid}|${level:uppercase=true}|${logger}|${message}${onexception:inner= ${exception:format=toString,Data}}`

---

##### HistoryTarget 選項

_maxCount_ - Maximum amount of stored logs for on-demand history. 非必要。 Defaults to `20` which is a good balance for providing initial history, while still keeping in mind memory usage that comes out of storage requirements. Must be greater than `0`.

---

## 警語

Be careful when you decide to combine `Debug` logging level or below in your `SteamTarget` with `steamID` that is taking part in the ASF process. This can lead to potential `StackOverflowException` because you'll create an infinite loop of ASF receiving given message, then logging it through Steam, resulting in another message that needs to be logged. Currently the only possibility for it to happen is to log `Trace` level (where ASF records its own chat messages), or `Debug` level while also running ASF in `Debug` mode (where ASF records all Steam packets).

In short, if your `steamID` is taking part in the same ASF process, then the `minlevel` logging level of your `SteamTarget` should be `Info` (or `Debug` if you're also not running ASF in `Debug` mode) and above. Alternatively you can define your own `<when>` logging filters in order to avoid infinite logging loop, if modifying level is not appropriate for your case. This caveat also applies to group chats.