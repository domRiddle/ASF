# Registros

O ASF permite que você configure seu próprio módulo de registro que será usado durante o tempo de execução. Você pode fazer isso colocando um arquivo especial chamado `NLog.config` na pasta do aplicativo. Você pode ler toda a documentação do NLog na **[wiki do NLog](https://github.com/NLog/NLog/wiki/Configuration-file)**, mas além disso você encontrará alguns exemplos disso aqui também.

* * *

## Registro padrão

Usar uma configuração NLog personalizada automaticamente desativa o registro padrão do ASF, que inclui `ColoredConsole` e `File`. Em outras palavras, sua configuração substitui **completamente** os registros padrões do ASF, o que significa que se você quiser manter, por exemplo, o destino `ColoredConsole`, você deve defini-lo você mesmo. Isso te permite não só adicionar destinos de registro **extras**, mas também a desabilitar ou modificar os **padrões**.

Se você quiser usar o registro padrão do ASF sem quaisquer modificações, você não precisa fazer nada; você também não precisa defini-lo em um `NLog.config` personalizado. Não use um `NLog.config` personalizado se você não quiser modificar o registro padrão do ASF. Para referência, o equivalente ao registro padrão do ASF codificado seria:

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

## Integração do ASF

ASF inclui alguns truques legais de código que melhoram sua integração com o NLog, permitindo que você capture mensagens específicas mais facilmente.

A variável `${logger}` específica do NLog vai sempre distinguir a fonte da mensagem - será sempre o `BotName` de um dos seus bots, ou `ASF` se a mensagem vier diretamente do processo do ASF. Assim você pode capturar facilmente mensagens de bots individuais ou (apenas) do processo do ASF, em vez de todas de uma vez, com base no nome dessa variável.

O ASF tenta marcar mensagens adequadamente com base nos níveis de aviso fornecidos pelo NLog, que torna possível para você pegar apenas mensagens específicas de níveis de registro específicos em vez de todos eles. Claro, o nível de registro de uma mensagem específica não pode ser personalizado, uma vez que é uma decisão codificada no ASF a seriedade de determinada mensagem, mas você pode tornar o ASF menos/mais silencioso conforme achar necessário.

O ASF registra informações extras, tais como mensagens de usuário/bate-papo no nível de log `Trace`. O registro padrão do ASF registra apenas o nível `Debug` e acima, que esconde essa informação extra, já que ela não é necessária para a maioria dos usuários, além de atravancar a saída contendo potencialmente mensagens mais importantes. Você pode no entanto fazer uso dessa informação reabilitando o nível de log `Trace`, especialmente em combinação com registrar apenas um bot específico, com o evento que você está interessado.

Em geral, o ASF tenta torná-lo tão fácil e conveniente para você quanto possível, registrando apenas as mensagens que você quer ao invés de forçá-lo a filtrá-las manualmente com alguma ferramenta de terceiros, tal como `grep` e semelhantes. Basta configurar o NLog corretamente conforme descrito abaixo, e você deverá ser capaz de especificar até mesmo regras complexas de registro com destinos personalizados como bancos de dados inteiros.

Em relação a versão o ASF tenta sempre ser fornecido com a versão mais atualizada do NLog disponível na **[NuGet](https://www.nuget.org/packages/NLog)** na data do lançamento do ASF. Muitas vezes é uma versão mais recente que a última versão estável, portanto você não deve ter problemas em usar qualquer recurso que você possa encontrar na wiki do NLog no ASF, mesmo recursos que estão em desenvolvimento e no estado WIP - apenas certifique-se de que você também está usando a última versão do ASF.

Como parte da integração, o ASF também inclui suporte para destinos de registros NLog adicionais, que serão explicados abaixo.

* * *

## Exemplos

Vamos começar com algo fácil. Nós usaremos apenas o alvo **[ColoredConsole](https://github.com/nlog/nlog/wiki/ColoredConsole-target)**. Nosso arquivo `NLog.config` inicial ficará assim:

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

A explicação da configuração acima é bastante simples - definimos um **alvo de registro** (na tag `target`) que é `ColoredConsole`, então nós redirecionamos **todos os registros** (`*`) de nível `Debug` e superior ao alvo `ColoredConsole` que havíamos definido anteriormente. É isso.

Se você iniciar o ASF com o arquivo `NLog.config` acima, apenas o alvo `ColoredConsole` estará ativo e o ASF não vai gravar nada no arquivo (`File`), independentemente da configuração de NLog copificada no ASF.

Agora vamos dizer que não gostamos do formato padrão `${longdate}|${level:uppercase=true}|${logger}|${message}` e queremos registrar apenas as mensagens. Podemos fazê-lo, modificando o **[Layout](https://github.com/nlog/nlog/wiki/Layouts)** do nosso alvo.

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

If you launch ASF now, you'll notice that date, level and logger name disappeared - leaving you only with ASF messages in format of `Function() Message`.

We can also modify the config to log to more than one target. Let's log to `ColoredConsole` and **[File](https://github.com/nlog/nlog/wiki/File-target)** at the same time.

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

And done, we'll now log everything to `ColoredConsole` and `File`. Did you notice that you can also specify custom `fileName` and extra options?

Finally, ASF uses various log levels, to make it easier for you to understand what is going on. We can use that information for modifying severity logging. Let's say that we want to log everything (`Trace`) to `File`, but only `Warning` and above **[log level](https://github.com/NLog/NLog/wiki/Configuration-file#log-levels)** to the `ColoredConsole`. We can achieve that by modifying our `rules`:

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

That's it, now our `ColoredConsole` will show only warnings and above, while still logging everything to `File`. You can further tweak it to log e.g. only `Info` and below, and so on.

Lastly, let's do something a bit more advanced and log all messages to file, but only from bot named `LogBot`.

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

You can see how we used ASF integration above and easily distinguished source of the message based on `${logger}` property.

* * *

## Advanced usage

The examples above are rather simple and made to show you how easy it is to define your own logging rules that can be used with ASF. You can use NLog for various different things, including complex targets (such as keeping logs in `Database`), logs rotation (such as removing old `File` logs), using custom `Layout`s, declaring your own `<when>` logging filters and much more. I encourage you to read through entire **[NLog documentation](https://github.com/nlog/nlog/wiki/Configuration-file)** to learn about every option that is available to you, allowing you to fine-tune ASF logging module in the way you want. It's a really powerful tool and customizing ASF logging was never easier.

* * *

## Limitations

ASF will temporarily disable **all** rules that include `ColoredConsole` or `Console` targets when expecting user input. Therefore, if you want to keep logging for other targets even when ASF expects user input, you should define those targets with their own rules, as shown in examples above, instead of putting many targets in `writeTo` of the same rule (unless this is your wanted behaviour). Temporary disable of console targets is done in order to keep console clean when waiting for user input.

* * *

## Chat logging

ASF includes extended support for chat logging by not only recording all received/sent messages on `Trace` logging level, but also exposing extra info related to them in **[event properties](https://github.com/NLog/NLog/wiki/EventProperties-Layout-Renderer)**. This is because we need to handle chat messages as commands anyway, so it doesn't cost us anything to log those events in order to make it possible for you to add extra logic (such as making ASF your personal Steam chatting archive).

### Event properties

| Nome        | Descrição                                                                                                                                                                                                    |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Echo        | `bool` type. This is set to `true` when message is being sent from us to the recipient, and `false` otherwise.                                                                                               |
| Message     | `string` type. This is the actual sent/received message.                                                                                                                                                     |
| ChatGroupID | `ulong` type. This is the ID of the group chat for sent/received messages. Will be `0` when no group chat is used for transmitting this message.                                                             |
| ChatID      | `ulong` type. This is the ID of the `ChatGroupID` channel for sent/received messages. Will be `0` when no group chat is used for transmitting this message.                                                  |
| SteamID     | `ulong` type. This is the ID of the Steam user for sent/received messages. Can be `0` when no particular user is involved in the message transmission (e.g. when it's us sending a message to a group chat). |

### Exemplo

This example is based on our `ColoredConsole` basic example above. Before trying to understand it, I strongly recommend to take a look **[above](#examples)** in order to learn about basics of NLog logging firstly.

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

We've started from our basic `ColoredConsole` example and extended it further. First and foremost, we've prepared a permanent chat log file per each group channel and Steam user - this is possible thanks to extra properties that ASF exposes to us in a fancy way. We've also decided to go with a custom layout that writes only current date, the message, sent/received info and Steam user itself. Lastly, we've enabled our chat logging rule only for `Trace` level, only for our `MainAccount` bot and only for functions related to chat logging (`OnIncoming*` which is used for receiving messages and echos, and `SendMessage*` for ASF messages sending).

The example above will generate `0-0-76561198069026042.txt` file when talking with **[ArchiBoT](https://steamcommunity.com/profiles/76561198069026042)**:

    2018-07-26 01:38:38 how are you doing? -> 76561198069026042
    2018-07-26 01:38:38 /me I'm doing great, how about you? <- 76561198069026042
    

Of course this is just a working example with a few nice layout tricks showed in practical manner. You can further expand this idea to your own needs, such as extra filtering, custom order, personal layout, recording only received messages and so on.

* * *

## ASF targets

In addition to standard NLog logging targets (such as `ColoredConsole` and `File` explained above), you can also use custom ASF logging targets.

For maximum completeness, definition of ASF targets will follow NLog documentation convention.

* * *

### SteamTarget

As you can guess, this target uses Steam chat messages for logging ASF messages. You can configure it to use either a group chat, or private chat. In addition to specifying a Steam target for your messages, you can also specify `botName` of the bot that is supposed to send those.

Supported in all environments used by ASF.

* * *

#### Configuration Syntax

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

Read more about using the [Configuration File](https://github.com/NLog/NLog/wiki/Configuration-file).

* * *

#### Parameters

##### General Options

*name* - Name of the target.

* * *

##### Layout Options

*layout* - Text to be rendered. [Layout](https://github.com/NLog/NLog/wiki/Layouts) Required. Default: `${level:uppercase=true}|${logger}|${message}`

* * *

##### SteamTarget Options

*chatGroupID* - ID of the group chat declared as 64-bit long unsigned integer. Not required. Defaults to `0` which will disable group chat functionality and use private chat instead. When enabled (set to non-zero value), `steamID` property below acts as `chatID` and specifies ID of the channel in this `chatGroupID` that the bot should send messages to.

*steamID* - SteamID declared as 64-bit long unsigned integer of target Steam user (like `SteamOwnerID`), or target `chatID` (when `chatGroupID` is set). Required. Defaults to 0 which disables logging target entirely.

*botName* - Name of the bot (as it's recognized by ASF, case-sensitive) of target bot that will be sending messages to `steamID` declared above. Not required. Defaults to `null` which will automatically select **any** currently connected bot. It's recommended to set this value appropriately, as `SteamTarget` does not take into account many Steam limitations, such as the fact that you must have `steamID` of the target on your friendlist.

* * *

#### SteamTarget Examples

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

#### Screenshots

![Captura da tela](https://i.imgur.com/5juKHMt.png)

* * *

### HistoryTarget

This target is used internally by ASF for providing fixed-size logging history for IPC GUI usage. In general you should define this target only if you're using custom NLog config for other customizations and you also want logging history in IPC GUI. It can also be declared when you'd want to modify default value of `maxCount`.

Supported in all environments used by ASF.

* * *

#### Configuration Syntax

```xml
<targets>
  <target type="History"
          name="String"
          layout="Layout"
          maxCount="Byte" />
</targets>
```

Read more about using the [Configuration File](https://github.com/NLog/NLog/wiki/Configuration-file).

* * *

#### Parameters

##### General Options

*name* - Name of the target.

* * *

##### Layout Options

*layout* - Text to be rendered. [Layout](https://github.com/NLog/NLog/wiki/Layouts) Required. Default: `${date:format=yyyy-MM-dd HH\:mm\:ss}|${processname}-${processid}|${level:uppercase=true}|${logger}|${message}${onexception:inner= ${exception:format=toString,Data}}`

* * *

##### HistoryTarget Options

*maxCount* - Maximum amount of stored logs for on-demand history. Not required. Defaults to `20` which is a good balance for providing initial history, while still keeping in mind memory usage that comes out of storage requirements. Must be greater than `0`.

* * *

## Caveats

Be careful when you decide to combine `Debug` logging level or below in your `SteamTarget` with `steamID` that is taking part in the ASF process. This can lead to potential `StackOverflowException` because you'll create an infinite loop of ASF receiving given message, then logging it through Steam, resulting in another message that needs to be logged. Currently the only possibility for it to happen is to log `Trace` level (where ASF records its own chat messages), or `Debug` level while also running ASF in `Debug` mode (where ASF records all Steam packets).

In short, if your `steamID` is taking part in the same ASF process, then the `minlevel` logging level of your `SteamTarget` should be `Info` (or `Debug` if you're also not running ASF in `Debug` mode) and above. Alternatively you can define your own `<when>` logging filters in order to avoid infinite logging loop, if modifying level is not appropriate for your case. This caveat also applies to group chats.