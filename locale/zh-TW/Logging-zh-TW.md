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

一般來說，ASF試圖讓您盡可能地簡單且方便，只記錄您想要的訊息，而不是強迫您透過&#8203;`grep`&#8203;等第三方工具來手動過濾它。 只需依照下列說明正確地設定NLog，您就應該能夠使用自訂目標（例如整個資料庫）指定的非常複雜的記錄規則。

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

最後，ASF使用著各種記錄級別，讓您更容易理解發生的事情。 We can use that information for modifying severity logging. Let's say that we want to log everything (`Trace`) to `File`, but only `Warning` and above **[log level](https://github.com/NLog/NLog/wiki/Configuration-file#log-levels)** to the `ColoredConsole`. We can achieve that by modifying our `rules`:

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

That's it, now our `ColoredConsole` will show only warnings and above, while still logging everything to `File`. You can further tweak it to log e.g. only `Info` and below, and so on.

Lastly, let's do something a bit more advanced and log all messages to file, but only from bot named `LogBot`.

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

You can see how we used ASF integration above and easily distinguished source of the message based on `${logger}` property.

---

## 進階用法

The examples above are rather simple and made to show you how easy it is to define your own logging rules that can be used with ASF. You can use NLog for various different things, including complex targets (such as keeping logs in `Database`), logs rotation (such as removing old `File` logs), using custom `Layout`s, declaring your own `<when>` logging filters and much more. I encourage you to read through entire **[NLog documentation](https://github.com/nlog/nlog/wiki/Configuration-file)** to learn about every option that is available to you, allowing you to fine-tune ASF logging module in the way you want. It's a really powerful tool and customizing ASF logging was never easier.

---

## 限制

ASF will temporarily disable **all** rules that include `ColoredConsole` or `Console` targets when expecting user input. Therefore, if you want to keep logging for other targets even when ASF expects user input, you should define those targets with their own rules, as shown in examples above, instead of putting many targets in `writeTo` of the same rule (unless this is your wanted behaviour). Temporary disable of console targets is done in order to keep console clean when waiting for user input.

---

## 聊天紀錄

ASF includes extended support for chat logging by not only recording all received/sent messages on `Trace` logging level, but also exposing extra info related to them in **[event properties](https://github.com/NLog/NLog/wiki/EventProperties-Layout-Renderer)**. This is because we need to handle chat messages as commands anyway, so it doesn't cost us anything to log those events in order to make it possible for you to add extra logic (such as making ASF your personal Steam chatting archive).

### 事件屬性

| 名稱          | 描述                                                                                                                                                                                                               |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Echo        | `bool`&#8203;型別。 This is set to `true` when message is being sent from us to the recipient, and `false` otherwise.                                                                                               |
| Message     | `string`&#8203;型別。 This is the actual sent/received message.                                                                                                                                                     |
| ChatGroupID | `ulong`&#8203;型別。 This is the ID of the group chat for sent/received messages. Will be `0` when no group chat is used for transmitting this message.                                                             |
| ChatID      | `ulong`&#8203;型別。 This is the ID of the `ChatGroupID` channel for sent/received messages. Will be `0` when no group chat is used for transmitting this message.                                                  |
| SteamID     | `ulong`&#8203;型別。 This is the ID of the Steam user for sent/received messages. Can be `0` when no particular user is involved in the message transmission (e.g. when it's us sending a message to a group chat). |

### 範例

This example is based on our `ColoredConsole` basic example above. Before trying to understand it, I strongly recommend to take a look **[above](#examples)** in order to learn about basics of NLog logging firstly.

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

We've started from our basic `ColoredConsole` example and extended it further. First and foremost, we've prepared a permanent chat log file per each group channel and Steam user - this is possible thanks to extra properties that ASF exposes to us in a fancy way. We've also decided to go with a custom layout that writes only current date, the message, sent/received info and Steam user itself. Lastly, we've enabled our chat logging rule only for `Trace` level, only for our `MainAccount` bot and only for functions related to chat logging (`OnIncoming*` which is used for receiving messages and echos, and `SendMessage*` for ASF messages sending).

The example above will generate `0-0-76561198069026042.txt` file when talking with **[ArchiBot](https://steamcommunity.com/profiles/76561198069026042)**:

```text
2018-07-26 01:38:38 how are you doing? -> 76561198069026042
2018-07-26 01:38:38 I'm doing great, how about you? <- 76561198069026042
```

Of course this is just a working example with a few nice layout tricks showed in practical manner. You can further expand this idea to your own needs, such as extra filtering, custom order, personal layout, recording only received messages and so on.

---

## ASF 目標

In addition to standard NLog logging targets (such as `ColoredConsole` and `File` explained above), you can also use custom ASF logging targets.

For maximum completeness, definition of ASF targets will follow NLog documentation convention.

---

### SteamTarget

As you can guess, this target uses Steam chat messages for logging ASF messages. You can configure it to use either a group chat, or private chat. In addition to specifying a Steam target for your messages, you can also specify `botName` of the bot that is supposed to send those.

Supported in all environments used by ASF.

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

Read more about using the [Configuration File](https://github.com/NLog/NLog/wiki/Configuration-file).

---

#### 參數

##### 一般選項
_name_ - Name of the target.

---

##### 布局選項
_layout_ - Text to be rendered. [Layout](https://github.com/NLog/NLog/wiki/Layouts) Required. Default: `${level:uppercase=true}|${logger}|${message}`

---

##### SteamTarget 選項

_chatGroupID_ - ID of the group chat declared as 64-bit long unsigned integer. 非必要。 Defaults to `0` which will disable group chat functionality and use private chat instead. When enabled (set to non-zero value), `steamID` property below acts as `chatID` and specifies ID of the channel in this `chatGroupID` that the bot should send messages to.

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