# Журналирование

ASF позволяет вам настроить свой собственный модуль журналирования, который будет использоваться в процессе работы. Вы можете сделать это добавив специальный файл с именем `NLog.config` в папку приложения. Полную документацию по NLog вы можете прочесть в **[NLog wiki](https://github.com/NLog/NLog/wiki/Configuration-file)**, но в дополнение к этому вы можете найти некоторые полезные примеры в этой статье.

* * *

## Журналирование по умолчанию

Использование пользовательской конфигурации NLog автоматически отключает модуль, используемый в ASF по умолчанию и включающий в себя `ColoredConsole` и `File`. Другими словами, ваша конфигурация **полностью** заменяет журналирование ASF по умолчанию, а это означает что если вы, например, хотите сохранить цель журналирования `ColoredConsole`, вам необходимо задать её самостоятельно. Это позволяет не только доабавлять **дополнительные** цели журналирования, но и отключать или модифицировать **используемые по умолчанию**.

Если вы хотите использовать журналирования ASF по умолчанию, без каких-либо модификаций, вам не надо ничего делать - вам даже не надо задавать их в пользовательском `NLog.config`. Не используйте файл `NLog.config` если вы не собираетесь модифицировать журналирование ASF по умолчанию. Однако для справок, эквивалент заданных в коде умолчаний журналирования ASF будет таким:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<nlog xmlns="http://www.nlog-project.org/schemas/NLog.xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <targets>
    <target xsi:type="ColoredConsole" name="ColoredConsole" layout="${date:format=yyyy-MM-dd HH\:mm\:ss}|${processname}-${processid}|${level:uppercase=true}|${logger}|${message}${onexception:inner= ${exception:format=toString,Data}}" />
    <target xsi:type="File" name="File" deleteOldFileOnStartup="true" fileName="log.txt" layout="${date:format=yyyy-MM-dd HH\:mm\:ss}|${processname}-${processid}|${level:uppercase=true}|${logger}|${message}${onexception:inner= ${exception:format=toString,Data}}" />
    <!-- Below becomes active when ASF's IPC interface is started -->
    <!-- <target type="History" name="History" layout="${date:format=yyyy-MM-dd HH\:mm\:ss}|${processname}-${processid}|${level:uppercase=true}|${logger}|${message}${onexception:inner= ${exception:format=toString,Data}}" maxCount="20" /> -->
  </targets>

  <rules>
    <logger name="*" minlevel="Debug" writeTo="ColoredConsole" />
    <logger name="*" minlevel="Debug" writeTo="File" />
    <!-- Below becomes active when ASF's IPC interface is started -->
    <!-- <logger name="*" minlevel="Debug" writeTo="History" /> -->
  </rules>
</nlog>
```

* * *

## Интеграция ASF

ASF имеет некоторые уловки в коде, которые позволяют улучшить интеграцию с NLog, позволяя вам гораздо проще отлавливать нужные вам сообщения.

Специфичная для NLog переменная `${logger}` всегда позволяет определить источник сообщения - это будет либо `BotName` одного из ваших ботов, или `ASF` если это сообщение напрямую от процесса ASF. Таким образом вы можете легко отлавливать сообщения от отдельных ботов, или (только) от процесса ASF, вместо всех сразу, основываясь на имени в этой переменной.

ASF старается отмечать сообщения в соответствии с различными уровнями предупреждений NLog, что позволяет вам отлавливать только сообщения с заданным уровнем журналирования, а не все сразу. Разумеется, уровень журналирования для отдельных сообщений не может быть изменён, поскольку в коде ASF жестко задано, насколько это сообщение серьёзно, но вы можете заставить ASF выводить меньше или больше информации, на своё усмотрение.

ASF журналирует дополнительные данные, такие как сообщения в чате на уровне журналирования `Trace`. Журналирование по-умолчанию в ASF записывает только сообщения с уровнем `Debug` или выше, что скрывает эту дополнительную информацию, поскольку это не нужно большинству пользователей, и в добавок засоряет выдачу в которой могут содержаться гораздо более важные сообщения. Вы, однако, можете использовать эту информацию, активировав уровень журналирования `Trace`, особенно в сочетании с журналированием только для выбранного бота, и только с событиями, которые вас интересуют.

В общем случае, ASF старается сделать вашу работу максимально простой и удобной, журналируя только сообщения которые вам нужны, вместо того чтобы заставлять вас вручную фильтровать журнал сторонними приложениями, такими как `grep` и тому подобное. Просто настройте NLog под свои потребности, как описано ниже, и вы сможете задать даже очень сложные правила журналирования, с пользовательскими целевыми журналами как например целые базы данных.

Относительно версии - мы всегда стараемся поставлять ASF с самой свежей версией NLog доступной в **[NuGet](https://www.nuget.org/packages/NLog)** на момент выпуска ASF. Зачастую это версия более новая, чем последняя стабильная версия, поэтому у вас не должно быть проблем с использованием в ASF любых функций которые вы найдёте в wiki NLog, даже тех которые находятся в активной разработке и в состоянии "ведётся работа" - главное убедитесь, что у вас последняя версия ASF.

Как часть интеграции ASF, ASF также включает в себя несколько дополнительных целей журналирования NLog, которые будут описаны ниже.

* * *

## Примеры

Начнем с чего-нибудь простого. Мы используем в качестве цели журналирования только **[ColoredConsole](https://github.com/nlog/nlog/wiki/ColoredConsole-target)**. Наш исходный файл `NLog.config` будет выглядеть так:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<nlog xmlns="http://www.nlog-project.org/schemas/NLog.xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <targets>
    <target xsi:type="ColoredConsole" name="ColoredConsole" />
  </targets>

  <rules>
    <logger name="*" minlevel="Debug" writeTo="ColoredConsole" />
  </rules>
</nlog>
```

Описание конфигурации выше очень простое - мы задали одну **цель журналирования** (тег `target`), которой будет `ColoredConsole`, затем мы перенаправили **все журналы** (`*`) уровня `Debug` и выше в цель `ColoredConsole`, которую мы задали выше. Вот и всё.

Если вы теперь запустите ASF с файлом `NLog.config`, описанным выше, будет активна только цель журналирования `ColoredConsole`, и ASF не будет ничего писать в файл (цель `File`), не смотря на записанную в коде ASF конфигурацию NLog.

Теперь допустим нам не нравится формат по умолчанию заданный `${longdate}|${level:uppercase=true}|${logger}|${message}` и мы хотим журналировать только сами сообщения. Мы можем это сделать изменив **[схему](https://github.com/nlog/nlog/wiki/Layouts)** (атрибут layout) для нашей цели журналирования.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<nlog xmlns="http://www.nlog-project.org/schemas/NLog.xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <targets>
    <target xsi:type="ColoredConsole" name="ColoredConsole" layout="${message}" />
  </targets>

  <rules>
    <logger name="*" minlevel="Debug" writeTo="ColoredConsole" />
  </rules>
</nlog>
```

Если вы теперь запустите ASF, вы заметите что дата, уровень предупреждения и источник сообщения исчезли - оставив вам только сообщения ASF в формате `Function() Message`.

Мы также можем изменить конфигурацию чтобы журналировать в больше чем одну цель. Давайте писать журнал одновременно в `ColoredConsole` и **[File](https://github.com/nlog/nlog/wiki/File-target)**.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<nlog xmlns="http://www.nlog-project.org/schemas/NLog.xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <targets>
    <target xsi:type="ColoredConsole" name="ColoredConsole" />
    <target xsi:type="File" name="File" fileName="NLog.txt" deleteOldFileOnStartup="true" />
  </targets>

  <rules>
    <logger name="*" minlevel="Debug" writeTo="ColoredConsole" />
    <logger name="*" minlevel="Debug" writeTo="File" />
  </rules>
</nlog>
```

Готово, теперь журнал будет выводить всё в `ColoredConsole` и `File`. Вы заметили что можете указать собственное имя файла (`fileName`) и другие настройки?

И наконец, в ASF используются разные уровни журналирования, чтобы вам было легче понять что происходит. Мы можем использовать эту информацию чтобы изменить строгость журналирования. Например мы хотим записывать всё (`Trace`) в `File`, но только сообщения **[уровня](https://github.com/NLog/NLog/wiki/Configuration-file#log-levels)** `Warning` и выше в `ColoredConsole`. Мы можем добиться этого изменив правила журналирования (тег `rules`):

```xml
<?xml version="1.0" encoding="utf-8" ?>
<nlog xmlns="http://www.nlog-project.org/schemas/NLog.xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <targets>
    <target xsi:type="ColoredConsole" name="ColoredConsole" />
    <target xsi:type="File" name="File" fileName="NLog.txt" deleteOldFileOnStartup="true" />
  </targets>

  <rules>
    <logger name="*" minlevel="Warn" writeTo="ColoredConsole" />
    <logger name="*" minlevel="Trace" writeTo="File" />
  </rules>
</nlog>
```

Вот и всё, теперь наша `ColoredConsole` будет показывать только предупреждения и выше, но при этом все сообщения будут записаны в `File`. Вы можете настраивать это и дальше, например включить туда только сообщения уровня `Info` и ниже, и тому подобное.

И наконец, давайте сделаем что-то более продвинутое и будем журналировать все сообщения в файл, но только от бота с именем `LogBot`.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<nlog xmlns="http://www.nlog-project.org/schemas/NLog.xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <targets>
    <target xsi:type="ColoredConsole" name="ColoredConsole" />
    <target xsi:type="File" name="LogBotFile" fileName="LogBot.txt" deleteOldFileOnStartup="true" />
  </targets>

  <rules>
    <logger name="*" minlevel="Debug" writeTo="ColoredConsole" />
    <logger name="LogBot" minlevel="Trace" writeTo="LogBotFile" />
  </rules>
</nlog>
```

Вы можете увидеть, как мы использовали интеграцию с ASF выше, и легко определили источник сообщения на основе свойства `${logger}`.

* * *

## Продвинутое использование

Примеры выше достаточно простые, и сделаны чтобы показать как легко задать свои правила журналирования для использования в ASF. Вы можете использовать NLog для множества различных вещей, включая сложные цели (такие как сохранение журнала `Database`), ротации журналов (как например удаления старых логов в `File`), использование пользовательских схем (`Layout`), задания собственных фильтров `<when>` и многое другое. Я настоятельно рекомендую прочитать целиком **[документацию по NLog](https://github.com/nlog/nlog/wiki/Configuration-file)** чтобы узнать все доступные вам опции, что позволит вам настроить ASF так, как вы хотите. Это действительно мощный инструмент, и персонализация журналов ASF никогда не была проще.

* * *

## Ограничения

ASF временно отключит **все** правила, включающие в себя цели `ColoredConsole` или `Console` на время ожидания ввода от пользователя. Поэтому если вы хотите, чтобы журналирование в другие цели велось даже когда ASF ожидает ввода от пользователя, вам нужно задать для этих целей собственные правила, как показано в примерах выше, вместо того чтобы добавлять несколько целей в атрибут `writeTo` одного правила (если конечно это не то, чего вы хотите). Временное отключение целей с консолью сделано с целью оставить консоль чистой при ожидании ввода от пользователя.

* * *

## Журналироване чата

ASF включает в себя расширенную поддержку журналирования чатов, которая заключается в том, что записываются не только все полученные и отосланные сообщения на уровне журналирования `Trace`, но также выдаётся дополнительная информация, связанная с ними, в **[свойствах события](https://github.com/NLog/NLog/wiki/EventProperties-Layout-Renderer)**. Это вызвано тем, что нам в любом случае приходится обрабатывать сообщения для отслеживания команд, поэтому нам ничего не стоит добавить в журнал эти события чтобы дать возможность использовать дополнительную логику (как например, создание собственного архива сообщений чата с помощью ASF).

### Свойства события

| Имя         | Описание                                                                                                                                                                                                                                                       |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Echo        | свойство типа `bool`. Имеет значение `true` когда сообщение отправляется от нас собеседники, и `false` в противном случае.                                                                                                                                     |
| Message     | свойство типа `string`. Содержит само отправленное/полученное сообщение.                                                                                                                                                                                       |
| ChatGroupID | свойство типа `ulong`. Содержит идентификатор группового чата в котором отправлено/получено сообщение. Будет равно `0` когда для передачи сообщения используется не групповой чат.                                                                             |
| ChatID      | свойство типа `ulong`. Содержит идентификатор канала в `ChatGroupID` в котором отправлено/получено сообщение. Будет равно `0` когда для передачи сообщения используется не групповой чат.                                                                      |
| SteamID     | свойство типа `ulong`. Это идентификатор пользователя Steam в чате с которым отправлено/получено сообщение. Может быть равно `0` когда ни один пользователь не задействован в передаче сообщения (например когда это мы отправляем сообщение в групповой чат). |

### Пример

Этот пример основан на нашем базовом примере с `ColoredConsole`, приведенном выше. Прежде чем пытаться разобраться в нём, я настоятельно рекомендую прочесть главу **[выше](#Примеры)**, чтобы сначала изучить основы журналирования с помощью NLog.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<nlog xmlns="http://www.nlog-project.org/schemas/NLog.xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <targets>
    <target xsi:type="ColoredConsole" name="ColoredConsole" />
    <target xsi:type="File" name="ChatLogFile" fileName="${event-properties:item=ChatGroupID}-${event-properties:item=ChatID}${when:when='${event-properties:item=ChatGroupID}' == 0:inner=-${event-properties:item=SteamID}}.txt" layout="${date:format=yyyy-MM-dd HH\:mm\:ss} ${event-properties:item=Message} ${when:when='${event-properties:item=Echo}' == 'true':inner=-&gt;:else=&lt;-} ${event-properties:item=SteamID}" />
  </targets>

  <rules>
    <logger name="*" minlevel="Debug" writeTo="ColoredConsole" />
    <logger name="MainAccount" level="Trace" writeTo="ChatLogFile">
      <filters>
        <when condition="not starts-with('${message}','OnIncoming') and not starts-with('${message}','SendMessage')" action="Ignore" />
      </filters>
    </logger>
  </rules>
</nlog>
```

Мы начали с нашего базового примера с `ColoredConsole` и расширили его. Первое и самое главное - мы приготовили постоянный файл для журнала чатов в каждой группе и с каждым пользователем - это возможно благодаря дополнительным свойствам, которые выдаёт для нас ASF. Мы решили использовать пользовательскую схему, которая будет записывать только текущую дату, сообщение, информацию получено/отправлено это сообщение, и самого пользователя Steam. И наконец, мы включили наше правило журналирования чата только для уровня `Trace`, только для нашего бота `MainAccount`, и только для функция связанных с жураналированием чата (`OnIncoming*`, которая используется для получения сообщений и эхо-ответов, и `SendMessage*` которая отправляет сообщения ASF).

Пример выше создаст файл `0-0-76561198069026042.txt` если мы будем общаться с **[ArchiBoT](https://steamcommunity.com/profiles/76561198069026042)**:

    2018-07-26 01:38:38 how are you doing? -> 76561198069026042
    2018-07-26 01:38:38 /me I'm doing great, how about you? <- 76561198069026042
    

Разумеется это просто рабочий пример чтобы показать несколько способов использования схем. Вы можете расширить эту идею под свои нужды, как например добавить дополнительные фильтры, пользовательский порядок, личную схему, запись только полученных сообщений и т. д.

* * *

## Цели журналирования ASF

В дополнение к стандартным целям журналирования NLog (таким как `ColoredConsole` и `File`, описанные выше), вы можете также использовать дополнительные цели журналирования ASF.

Для единообразия, описания целей журналирования ASF будут следовать стандартам документации NLog.

* * *

### SteamTarget

Как вы могли догадаться, эта цель использует сообщения в чате Steam для журналирования сообщений ASF. Вы можете настроить её на использование либо группового, либо личного чата. В дополнение к указании цели Steam для ваших сообщений, вы можете также указать `botName` - имя бота который должен их отсылать.

Поддерживается во всех средах используемых ASF.

* * *

#### Синтаксис конфигурации

```xml
<targets>
  <target type="Steam"
          name="String"
          layout="Layout"
          chatGroupID="Ulong"
          steamID="Ulong"
          botName="String" />
</targets>
```

Прочтите больше о использовании [конфигурационного файла](https://github.com/NLog/NLog/wiki/Configuration-file).

* * *

#### Параметры

##### Общие параметры

*name* - Имя цели журналирования.

* * *

##### Параметры схемы

*layout* -Схема вывода сообщений. Требуется [Layout](https://github.com/NLog/NLog/wiki/Layouts). По умолчанию: `${level:uppercase=true}|${logger}|${message}`

* * *

##### Параметры SteamTarget

*chatGroupID* - идентификатор группового чата в формате 64-битного беззнакового целого. Не обязательный параметр. По умолчанию имеет значение `0`, что отключает функционал группового чата и использует вместо этого личный чат. Когда этот параметр активирован (имеет отличное от нуля значение), параметр `steamID`, описанный ниже, выступает в роли `chatID` и указывает идентификатор канала в этом `chatGroupID`, куда бот должен отправлять сообщения.

*steamID* - SteamID задаётся как 64-битное беззнаковое целое, и представляет из себя идентификатор пользователя Steam (подобно `SteamOwnerID`), или целевой `chatID` (если установлен `chatGroupID`). Обязательный параметр. По умолчанию имеет значение 0, которое полностью отключает эту цель журналирования.

*botName* - Имя бота (в том виде, как его распознает ASF, чувствительно к регистру) который будет отсылать сообщения `steamID` заданному выше. Не обязательный параметр. По умолчанию имеет значение `null`, в этом случае автоматически выбирается **любой** из подключенных в данный момент ботов. Рекомендуется задавать значение параметра в явном виде, поскольку `SteamTarget` не учитывает многие ограничения Steam, как например тот факт, что целевой `steamID` должен быть у вас в списке друзей.

* * *

#### Примеры SteamTarget

In order to write all messages of `Debug` level and above, from bot named `MyBot` to steamID of `76561198006963719`, you should use `NLog.config` similar to below:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<nlog xmlns="http://www.nlog-project.org/schemas/NLog.xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
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

* * *

#### Скриншоты

![Скриншот](https://i.imgur.com/5juKHMt.png)

* * *

### HistoryTarget

This target is used internally by ASF for providing fixed-size logging history for IPC GUI usage. In general you should define this target only if you're using custom NLog config for other customizations and you also want logging history in IPC GUI. It can also be declared when you'd want to modify default value of `maxCount`.

Поддерживается во всех средах используемых ASF.

* * *

#### Синтаксис конфигурации

```xml
<targets>
  <target type="History"
          name="String"
          layout="Layout"
          maxCount="Byte" />
</targets>
```

Прочтите больше о использовании [конфигурационного файла](https://github.com/NLog/NLog/wiki/Configuration-file).

* * *

#### Параметры

##### Общие параметры

*name* - Имя цели журналирования.

* * *

##### Параметры схемы

*layout* -Схема вывода сообщений. Требуется [Layout](https://github.com/NLog/NLog/wiki/Layouts). Default: `${date:format=yyyy-MM-dd HH\:mm\:ss}|${processname}-${processid}|${level:uppercase=true}|${logger}|${message}${onexception:inner= ${exception:format=toString,Data}}`

* * *

##### HistoryTarget Options

*maxCount* - Maximum amount of stored logs for on-demand history. Не обязательный параметр. Defaults to `20` which is a good balance for providing initial history, while still keeping in mind memory usage that comes out of storage requirements. Must be greater than `0`.

* * *

## Caveats

Be careful when you decide to combine `Debug` logging level or below in your `SteamTarget` with `steamID` that is taking part in the ASF process. This can lead to potential `StackOverflowException` because you'll create an infinite loop of ASF receiving given message, then logging it through Steam, resulting in another message that needs to be logged. Currently the only possibility for it to happen is to log `Trace` level (where ASF records its own chat messages), or `Debug` level while also running ASF in `Debug` mode (where ASF records all Steam packets).

In short, if your `steamID` is taking part in the same ASF process, then the `minlevel` logging level of your `SteamTarget` should be `Info` (or `Debug` if you're also not running ASF in `Debug` mode) and above. Alternatively you can define your own `<when>` logging filters in order to avoid infinite logging loop, if modifying level is not appropriate for your case. This caveat also applies to group chats.