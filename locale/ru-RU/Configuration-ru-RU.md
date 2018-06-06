# Конфигурация

Эта статься посвящена конфигурированию ASF. Здесь содержится полная документация по содержимому папки `config`, прозволяющая настроить ASF под ваши нужды.

- **[Введение](#Введение)**
- **[Сетевой генератор конфигураций](#Сетевой-генератор-конфигураций)**
- **[Ручная настройка](#Ручная-настройка)**
- **[Файл глобальной конфигурации](#Файл-глобальной-конфигурации)**
- **[Конфигурация бота](#Конфигурация-бота)**
- **[Файловая структура](#Файловая-структура)**
- **[Типы параметров в JSON](#Типы-параметров-в-json)**
- **[Совместимость типов](#Совместимость-типов)**
- **[Совместимость конфигураций](#Совместимость-конфигураций)**
- **[Автоматическая перезагрузка](#Автоматическая-перезагрузка)**

* * *

## Введение

Конфигурация ASF разделена на две основные части - глобальная конфигурация (для всего процесса), и конфигурация каждого бота. У каждого бота есть собственный файл конфигурации бота с именем `BotName.json` (где `BotName` это имя, которое вы дали боту), а для конфигурации процесса ASF используется один файл с именем `ASF.json`.

Бот - это один аккаунт Steam используемый ASF. Для нормальной работы ASF требует чтобы был определён хотя бы **один** бот. Жёсткого ограничения на количество ботов нет, поэтому вы можете использовать столько ботов (аккаунтов Steam) сколько захотите.

ASF использует формат **[JSON](https://ru.wikipedia.org/wiki/JSON)** для сохранения настроек в файлах конфигурации. Это дружественный к пользователю, читаемый и довольно универсальный формат, в котором вы можете конфигурировать программу. Но не волнуйтесь, вам не обязательно знать JSON чтобы настроить ASF. Я упомянул это просто на случай если вы захотите массово создавать конфигурации ASF каким-нибудь скриптом.

Конфигурирование может быть проделано вручную - путём создания соответствующих файлов конфигруации в формате JSON, или с использованием нашего **[сетевого генератора конфигураций](https://justarchi.github.io/ArchiSteamFarm)**, что будет намного легче и удобнее. Если вы не продвинутый пользователь, я рекомендую вам использовать генератор конфигураций, описание которого приведено ниже.

**[Вернуться к началу](#Конфигурация)**

* * *

## Сетевой генератор конфигураций

Цель сетевого генератора конфигураций - предоставить вам удобный интерфейс для создания конфигурационных файлов ASF. Сетевой генератор конфигураций работает на 100% на стороне клиента, это значит что никакие данные, которые вы в нём вводите, никуда не пересылаются, и обрабатываются только на вашей машине. Это обеспечивает безопасность и надёжность, поскольку он может работать даже **[оффлайн](https://github.com/JustArchi/ArchiSteamFarm/tree/master/docs)** если вы скачаете все файлы и запустите `index.html` в своём любимом браузере.

Сетевой генератор конфигураций проверен в Chrome и Firefox, но он должен работать во всех популярных браузерах с включенным javascript.

Пользоваться им очень просто - выбираете, хотите ли вы создать файл конфигурации `ASF` или `Бот` выбрав соответствующую закладку, проверяете что выбранная в генераторе версия соответствует вашей версии ASF, вводите все необходимые поля и нажимаете кнопку "Скачать". Скачанный файл кладёте в папку `config` вашего ASF, при необходимости перезаписывая существующий. Повторяете всю процедуру в случае будущих модификаций, и используете остаток этой статьи для объяснения всех доступных параметров конфигурации.

**[Вернуться к началу](#Конфигурация)**

* * *

## Ручная настройка

Я настоятельно рекомендую использовать сетевой генератор конфигураций, но если по какой-то причине вы не хотите - вы можете создавать конфигурационные файлы сами. Ознакомьтесь с файлом `example.json` в качестве примера правильной структуры, вы можете скопировать этот файл и использовать как базу для вашего нового бота. Поскольку вы не пользуетесь программой, убедитесь что ваш файл **[валидный с точки зрения JSON](https://jsonlint.com)**, поскольку ASF не сможет его загрузить если его не удаётся разобрать. Чтобы узнать правильную структуру JSON для всех полей, ознакомьтесь с разделом "**[Типы параметров в JSON](#Типы-параметров-в-json)**" и приведенной ниже документацией.

**[Вернуться к началу](#Конфигурация)**

* * *

## Файл глобальной конфигурации

Файл глобальной конфигурации называется `ASF.json` и имеет следующую структуру:

```json
{
    "AutoRestart": true,
    "Blacklist": [],
    "CommandPrefix": "!",
    "ConfirmationsLimiterDelay": 10,
    "ConnectionTimeout": 60,
    "CurrentCulture": null,
    "Debug": false,
    "FarmingDelay": 15,
    "GiftsLimiterDelay": 1,
    "Headless": false,
    "IdleFarmingPeriod": 8,
    "InventoryLimiterDelay": 3,
    "IPC": false,
    "IPCPassword": null,
    "IPCPrefixes": [
        "http://127.0.0.1:1242/"
    ],
    "LoginLimiterDelay": 10,
    "MaxFarmingTime": 10,
    "MaxTradeHoldDuration": 15,
    "OptimizationMode": 0,
    "Statistics": true,
    "SteamOwnerID": 0,
    "SteamProtocols": 7,
    "UpdateChannel": 1,
    "UpdatePeriod": 24,
    "WebLimiterDelay": 200,
    "WebProxy": null,
    "WebProxyPassword": null,
    "WebProxyUsername": null
}
```

**Совет:** Если вы не собираетесь менять этих настроек, вы можете просто оставить всё по-умолчанию, а значит вы можете закрыть `ASF.json` и перейти к конфигурированию бота.

* * *

Все параметры описаны ниже:

`AutoRestart` - параметр типа `bool` со значением по-умолчанию `true`. Этот параметр определяет, разрешено ли ASF перезапускаться при необходимости. Существуют события, которые могут потребовать перезапуска ASF, такие как обновление (раз в `UpdatePeriod` или по команде `update`), редактирование файла `ASF.json`, команда `restart` и тому подобное. Перезапуск обычно включает в себя две части - создание нового процесса и завершение текущего. Для большинства пользователей подойдёт оставить этому параметро значение по-умолчанию `true`, однако если вы запускаете ASF через собственный скрипт и/или `dotnet`, возможно вы захотите иметь полный контроль над запуском процесса, и избежать ситуации когда новый (перезапущеный) процесс ASF работает где-то в фоне, а не в основном процессе скрипта, который завершится вместе со старым процессом ASF. Особенно важно то, что новый процесс не будет прямым потомком вашего скрипта, из-за чего вы не сможете например использовать его стандартный поток вывода.

В этом случае вам поможет этот параметр, вы можете установить его в `false`. Однако помните, что в этом случае **вы** несёте ответственность за перезапуск процесса. Это особенно важно поскольку в этом случае ASF просто выйдет вместо создания нового процесса (например, после обновления), поэтому если вы не добавили свою логику для этого случая он просто перестанет работать пока вы его не запустите снова. ASF всегда выходит с правильным кодом выхода, указывающим на удачу (ноль) или неудачу (не ноль), поэтому вы можете добавить правильную логику в своём скрипте чтобы избежать перезапуска ASF в случае отказа, или как минимум создавать резервную копию `log.txt` для дальнейшего анализа. Также помните, что команда `restart` выполнит перезапуск ASF независимо от значения этого параметра, поскольку этот параметр задаёт поведение по-умолчанию, а команда `restart` производит принудительный перезапуск процесса. Если у вас нет причин отключать эту функцию, рекомендуется оставить её включенной.

* * *

`Blacklist` - `HashSet<uint>` type with default value of being empty. As the name suggests, this global config property defines appIDs (games) that will be entirely ignored by automatic ASF idling process. Unfortunately Steam loves to flag summer/winter sale badges as "available for cards drop", which confuses ASF process by making it believe that it's a valid game that should be farmed. If there was no any kind of blacklist, ASF would eventually "hang" at farming a game which is in fact not a game, and wait infinitely for cards drop that will not happen. ASF blacklist serves a purpose of marking those badges as not available for farming, so we can silently ignore them when deciding what to farm, and not fall into the trap.

ASF includes two blacklists by default - `GlobalBlacklist`, which is hardcoded into the ASF code and not possible to edit, and normal `Blacklist`, which is defined here. `GlobalBlacklist` is updated together with ASF version and typically includes all "bad" appIDs at the time of release, so if you're using up-to-date ASF then you do not need to maintain your own `Blacklist` defined here. The main purpose of this property is to allow you blacklisting new, not-known at the time of ASF release appIDs, which should not be farmed. Hardcoded `GlobalBlacklist` is being updated as fast as possible, therefore you're not required to update your own `Blacklist` if you're using latest ASF version, but without `Blacklist` you'd be forced to update ASF in order to "keep running" when Valve releases new sale badge - I don't want to force you to use latest ASF code, therefore this property is here to allow you "fixing" ASF yourself if you for some reason don't want to, or can't, update to new hardcoded `GlobalBlacklist` in new ASF release, yet you want to keep your old ASF running. Если у вас нет **веских** причин редактировать этот параметр, рекомендуется оставить ему значение по-умолчанию.

If you're looking for bot-based blacklist instead, take a look at `ib`, `ibadd` and `ibrm` **[commands](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)**.

* * *

`CommandPrefix` - `string` type with default value of `!`. This property specifies **case-sensitive** prefix used for ASF **[commands](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)**. In other words, this is what you need to prepend to each ASF command in order to make ASF listen to you. It's possible to set this value to `null` or empty in order to make ASF use no command prefix, in which case you input commands with their plain identifiers. However, doing so will potentially decrease ASF's performance as ASF is optimized to not parse message further if it doesn't start with `CommandPrefix` - if you intentionally decide to not use it, ASF will be forced to read all messages and respond to them, even if they're not ASF commands. Therefore it's recommended to keep using some `CommandPrefix`, such as `/` if you don't like default value of `!`. For consistency, `CommandPrefix` affects the entire ASF process. Если у вас нет причин редактировать этот параметр, рекомендуется оставить ему значение по-умолчанию.

* * *

`ConfirmationsLimiterDelay` - `byte` type with default value of `10`. Сеть Steam в общем случае включает в себя различные ограничения на частоту одинаковых запросов, поэтому нам приходится добавлять дополнительные задержки, чтобы не сработали эти ограничения, поскольку такое срабатывание сделает сервисы Steam временно недоступными. ASF will ensure that there will be at least `ConfirmationsLimiterDelay` seconds in between of two consecutive 2FA confirmations fetching requests - those are being used by **[ASF 2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Two-factor-authentication)** during e.g. `2faok` command, as well as on as-needed basis when performing various trading-related operations. Default value was set based on our tests and should not be lowered if you don't want to run into issues. Если у вас нет **веских** причин редактировать этот параметр, рекомендуется оставить ему значение по-умолчанию.

* * *

`ConnectionTimeout` - `byte` type with default value of `60`. This property defines timeouts for various network actions done by ASF, in seconds. In particular, `ConnectionTimeout` defines timeout in seconds for HTTP and IPC requests, `ConnectionTimeout / 10` defines maximum number of failed heartbeats, while `ConnectionTimeout / 30` defines number of minutes we allow for initial Steam network connection request. Default value of `60` should be fine for majority of people, however, if you have rather slow network connection or PC, you might want to increase this number to something higher (like `90`). Keep in mind that bigger values will not magically fix slow or even inaccessible Steam servers, so we shouldn't infinitely wait for something that won't happen and simply try again later. Setting this value too high will result in excessive delay in catching network issues, as well as in decrease of overall performance. Setting this value too low will decrease overall stability and performance as well, as ASF will abort valid request still being parsed. Therefore setting this value lower than default has no advantage in general, as Steam servers tend to be slow from time to time, and might require more time for parsing ASF requests. Default value is a balance between believing that our network connection is stable, and doubting in Steam network to handle our request in given timeout. If you want to detect issues sooner and make ASF reconnect/respond faster, default value should do (or very slightly below, making ASF less patient). If you instead notice that ASF is running into network issues, such as failing requests, heartbeats being lost or connection to Steam interrupted, it might make sense to increase this value if you're sure that it's **not** caused by your network, but by Steam itself, as increasing timeouts makes ASF more "patient" and not deciding to reconnect right away. It might also make sense to increase this value if you have rather slow internet that requires more time to process the data being transmitted. In short, default value should be decent for most cases, but you might want to increase it if needed. Still, going far above the default value doesn't make much sense either, since bigger timeouts won't magically fix inaccessible Steam servers. Если у вас нет причин редактировать этот параметр, рекомендуется оставить ему значение по-умолчанию.

* * *

`CurrentCulture` - `string` type with default value of `null`. By default ASF attempts to use your operating system language, and will prefer to use translated strings in that language if available. This is possible thanks to our community that tries to **[localize](https://github.com/JustArchi/ArchiSteamFarm/wiki/Localization)** ASF in all most popular languages. If for some reason you don't want to use your OS native language, you can use this config property to pick any valid language you'd want to use instead. For a list of all available cultures, please visit **[MSDN](https://msdn.microsoft.com/en-us/library/cc233982.aspx)** and look for `Language tag`. It's nice to note that ASF accepts both specific cultures, such as `en-GB`, but also neutral ones, such as `en`. Specifying current culture might also affect other culture-specific behaviour, such as currency/date format and alike. Please note that you might need additional font/language packs for displaying language-specific characters properly, if you picked non-native culture that makes use of them. Typically you want to make use of this config property if you prefer ASF in English instead of your native language.

* * *

`Debug` - `bool` type with default value of `false`. This property defines if process should run in debug mode. When in debug mode, ASF creates a special `debug` directory next to the `config`, which keeps track of whole communication between ASF and Steam servers. Debug information can help spotting nasty issues related to networking and general ASF workflow. In addition to that, some program routines will be far more verbose, such as `WebBrowser` stating exact reason why some requests are failing - those entries are written to normal ASF log. **You should not run ASF in Debug mode, unless asked by developer**. Running ASF in debug mode **decreases performance**, **affects stability negatively** and is **far more verbose in various places**, so it should be used **only** intentionally, in short-run, for debugging particular issue, reproducing the problem or getting more info about a failing request, and alike, but **not** for normal program execution. You will see **a lot** of new errors, issues, and exceptions - make sure that you have a decent knowledge about ASF, Steam and its quirks if you decide to analyze all of that yourself, as not everything is relevant.

**WARNING:** enabling this mode includes logging of **potentially sensitive** information such as logins and passwords that you're using for logging in to Steam (due to network logging). That data is written to both `debug` directory, as well as standard `log.txt` (that is now intentionally much more verbose to log this info). You should not post debug content generated by ASF in any public location, ASF developer should always remind you of sending it to his e-mail, or other secure location.

* * *

`FarmingDelay` - `byte` type with default value of `15`. In order for ASF to work, it will check currently farmed game every `FarmingDelay` minutes, if it perhaps dropped all cards already. Setting this property too low can result in excessive amount of steam requests being sent, while setting it too high can result in ASF still "farming" given title for up to `FarmingDelay` minutes after it's fully farmed. Default value should be excellent for most users, but if you have many bots running, you might consider increasing it to something like `30` minutes in order to limit steam requests being sent. It's nice to note that ASF uses event-based mechanism and checks game badge page on each Steam item dropped, so in general **we don't even need to check it in fixed time intervals**, but as we don't fully trust Steam network - we check game badge page anyway, if we didn't check it through card being dropped event in last `FarmingDelay` minutes (in case Steam network didn't inform us about item dropped or stuff like that). Assuming that Steam network is working properly, decreasing this value **will not improve farming efficiency in any way**, while **increasing network overhead significantly** - it's recommended only to increase it (if needed) from default of `15` minutes. Если у вас нет **веских** причин редактировать этот параметр, рекомендуется оставить ему значение по-умолчанию.

* * *

`GiftsLimiterDelay` - `byte` type with default value of `1`. Сеть Steam в общем случае включает в себя различные ограничения на частоту одинаковых запросов, поэтому нам приходится добавлять дополнительные задержки, чтобы не сработали эти ограничения, поскольку такое срабатывание сделает сервисы Steam временно недоступными. ASF will ensure that there will be at least `GiftsLimiterDelay` seconds in between of two consecutive gift/key/license handling (redeeming) requests. In addition to that it'll also be used as global limiter for game list requests, such as the one issued by `owns` **[command](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)**. Если у вас нет **веских** причин редактировать этот параметр, рекомендуется оставить ему значение по-умолчанию.

* * *

`Headless` - `bool` type with default value of `false`. This property defines if process should run in headless mode. When in headless mode, ASF assumes that it's running on a server or in other non-interactive environment, therefore it will not attempt to read crucial account credentials such as 2FA code, SteamGuard code, password or any other variable required for ASF to operate. This is equal to making ASF console read-only. `Headless` mode is useful mainly for users running ASF on their servers, as instead of asking e.g. for 2FA code, ASF will silently abort the operation by stopping an account. Unless you're running ASF on a server, and you previously confirmed that ASF is able to operate in non-headless mode, you should keep this property disabled. Any user interaction will be denied when in headless mode, and your accounts will not run if they require **any** console input during starting. This is useful for servers, as ASF can abort trying to log onto the account when asked for credentials, instead of waiting (infinitely) for user to provide those. Enabling this mode will also allow you to use `input` **[command](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)** which acts as a replacement for standard console input. If you're not sure how to set this property, leave it with default value of `false`.

If you're running ASF on the server, you might want to use this option together with `--process-required` **[command-line argument](https://github.com/JustArchi/ArchiSteamFarm/wiki/Command-line-arguments)**.

* * *

`IdleFarmingPeriod` - `byte` type with default value of `8`. When ASF has nothing to farm, it will periodically check every `IdleFarmingPeriod` hours if perhaps account got some new games to farm. This feature is not needed when talking about new games we're getting, as ASF is smart enough to automatically check badge pages in this case. `IdleFarmingPeriod` is mainly for situations such as old games we already have having trading cards added. In this case there is no event, so ASF has to periodically check badge pages if we want to have this covered. Value of `0` disables this feature. Also check: `ShutdownOnFarmingFinished`.

* * *

`InventoryLimiterDelay` - `byte` type with default value of `3`. Сеть Steam в общем случае включает в себя различные ограничения на частоту одинаковых запросов, поэтому нам приходится добавлять дополнительные задержки, чтобы не сработали эти ограничения, поскольку такое срабатывание сделает сервисы Steam временно недоступными. ASF will ensure that there will be at least `InventoryLimiterDelay` seconds in between of two consecutive inventory requests - those are being used for fetching your own inventory (and only for that). Default value of `3` was set based on looting over 100 bot instances, and should satisfy most (if not all) of the users. You may however want to decrease it, or even change to `0` if you have very low amount of bots, so ASF will ignore the delay and loot steam inventories much faster. Be warned though, as setting it too low **will** result in Steam temporarily banning your IP, and that will prevent you from fetching your inventory at all. You also might need to increase this value if you're running a lot of bots with a lot of inventory requests, although in this case you should probably try to limit number of those requests instead. Если у вас нет **веских** причин редактировать этот параметр, рекомендуется оставить ему значение по-умолчанию.

* * *

`IPC` - `bool` type with default value of `false`. This property defines if ASF's **[IPC](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC)** server should start together with the process. IPC allows for inter-process communication by booting local HTTP server on specified `IPCPrefixes`. If you're not going to make use of ASF's IPC server, then there is no reason for you to enable this option.

* * *

`IPCPassword` - `string` type with default value of `null`. This property defines mandatory password for every call done via IPC and serves as an extra security measure. When set to non-empty value, all IPC requests will require extra `password` property set to the password specified here. Default value of `null` will skip a need of the password, making ASF respect all queries. In addition to that, enabling this option also enables built-in IPC anti-bruteforce mechanism which will temporarily ban given `IPAddress` after sending too many unauthorized requests in a very short time. Если у вас нет причин редактировать этот параметр, рекомендуется оставить ему значение по-умолчанию.

* * *

`IPCPrefixes` - `HashSet<string>` type with default value of `http://127.0.0.1:1242/` item. This property specifies prefixes that will be used by ASF's `HttpListener` in **[IPC](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC)** interface. In other words, this property specifies on which endpoints ASF IPC interface will listen for incoming requests. For general information about `HttpListener` prefixes, please refer to **[MSDN](https://docs.microsoft.com/en-us/dotnet/api/system.net.httplistener?view=netstandard-2.0#Remarks)**.

> Строка URI-префикса состоит из схемы (http и https), имени хоста, необязательного порта, и необязательного пути. Пример полной строки префикса `"http://www.contoso.com:8080/customerData/"`. Перфиксы должны заканчиваться прямым слешом ("/"). Объект `HttpListener` с префиксом, который максимально точно соответствует URI отвечает на запрос. Несколько объектов `HttpListener` не могут иметь одинаковый префикс. Если `HttpListener` добавляет уже используемый префикс, срабатывает исключение `Win32Exception`.
> 
> Когда указан порт, имя хоста можно заменить "\*" для указания что `HttpListener` должен принимать запросы на этот порт если URI запроса не соответствует ни одному другому префиксу. Например, чтобы получать все запросы на порт 8080 когда URI запроса не обрабатывается ни одним другим `HttpListener`, нужен префикс `"http://*:8080/"`. Аналогично, чтобы указать что `HttpListener` должен принимать все запросы на конкретный порт, замените имя хоста символом "+", `"https://+:8080"`. Символы "\*" и "+" могут присутствовать в префиксах в которых указан путь.
> 
> Начиная с .NET 4.5.3 и Windows 10, в префиксах URI, управляемых `HttpListener` поддерживаются подстановочные символы вместо под-доменов. В качестве символа подстановки в под-доменах используется символ "\*" как часть имени хоста в префиксе URI: например, `http://*.foo.com/`, этот аргумент передаётся методу `HttpListenerPrefixCollection.Add`. Это работает на .NET 4.5.3 и Windows 10; в более ранних версиях это вызовет `HttpListenerException`

ASF by default listens only on `127.0.0.1` address to ensure that no other machine but your own can access it. This is a security measure, as accessing IPC interface can lead to attacker taking over your ASF process, which can have dramatic effects. However, if you know what you're doing, e.g. you will restrict access to IPC yourself, using something like `iptables` or another form of firewall, you may change this property (at your own risk) to something less restrictive, such as `*` which enables IPC on all network interfaces. You may also want to change default ASF IPC port to any other port you want, suggested ports are above `1024`, as ports `0-1024` typically require `root` privileges on Unix-like operating systems. Also keep in mind that some listening addresses might require extra privileges, for example binding to public interface on Windows requires ASF to be run as administrator (or proper `netsh` policy).

If your machine supports IPv6, you might want to add a value of `http://[::1]:1242/` to this collection in order to listen on both local IPv4 `127.0.0.1` address, as well as local IPv6 `::1` one. This is especially important if you want to access ASF via `localhost` name, as in this case your machine might want to use IPv6 by default.

Keep in mind that this property apart from bind address, also specifies valid **URLs** under which IPC interface is accessible. In other words, if you want to access your IPC interface from `asf.example.com` hostname, then you should define `http://asf.example.com:1242/` in your `IPCPrefixes`. Even if you make your IPC interface reachable (e.g. by changing `127.0.0.1` to your public IP address), you'll get `404 NotFound` error if the URL under which you're trying to access it is not defined in `IPCPrefixes` of ASF. This is why you should define in `IPCPrefixes` all URLs under which the IPC interface should be accessible, and this can include local IPv4 and IPv6 addresses, public IPv4 and IPv6 addresses, and custom hostname, making it 5 different `IPCPrefixes` total, if you require/want to access IPC interface in all of those 5 ways. Of course, you can also define just `http://*:1242/`, but this might not always be appropriate.

Если у вас нет причин редактировать этот параметр, рекомендуется оставить ему значение по-умолчанию.

* * *

`LoginLimiterDelay` - параметр типа `byte` со значением по-умолчанию `10`. Сеть Steam в общем случае включает в себя различные ограничения на частоту одинаковых запросов, поэтому нам приходится добавлять дополнительные задержки, чтобы не сработали эти ограничения, поскольку такое срабатывание сделает сервисы Steam временно недоступными. ASF обеспечивает чтобы между двумя последовательными запросами на подключение прошло как минимум `LoginLimiterDelay` секунд. Значение по-умолчанию `10` было выбрано на основе подключения 100 ботов, и должно удовлетворить большинство (если не всех) пользователей. Вы можете, однако, захотеть уменьшить его, или даже сделать равным `​0​`, если у вас очень малое количество ботов, в этом случае ASF будет игнорировать задержку и подключаться к Steam намного быстрее. Но учтите, что слишком низкое значение при большом количестве ботов **приведёт** к временному бану по IP, что приведёт к **полной** невозможности подключиться и ошибке `InvalidPassword/RateLimitExceeded` - и это касается и обычного клиента Steam, не только ASF. Дополнительно, этот параметр также используется как буфер балансировки нагрузки всех запланированных действий ASF, таких как обмены по `SendTradePeriod`. Если у вас нет **веских** причин редактировать этот параметр, рекомендуется оставить ему значение по-умолчанию.

* * *

`MaxFarmingTime` - параметр типа `byte` со значением по-умолчанию `10`. Как вы должно быть знаете, Steam не всегда работает идеально, иногда происходят странные ситуации, как например Steam иногда не учитывает время в игре, хотя вы играете. ASF будет фармить одну игру в соло-режиме максимум `MaxFarmingTime` часов, и будет считать что фарм завершен по окончанию этого периода. Это необходимо не только чтобы процесс фарма не "зависал" в случае каких-то странных ситуаций, но и на случай если Steam выпускает новый значок, который не даёт ASF продолжить работу (см. `Blacklist`). Значение по-умолчанию в `10` часов должно быть достаточно для выпадения карточек из одной игры. Установка слишком маленького значения может привести к тому что обычные игры будут пропускаться (и да, известны случаи когда выпадение карт в игре занимает 9 часов), а слишком высокое значение может привести к "зависанию" процесса. Обратите внимание, что этот параметр влияет только на одну игру в одном сеансе фарма (поэтому после того пройдёт вся очередь ASF снова вернётся к той же игре), а также на то, что это не общее время в игре, а время фарма в ASF. Если у вас нет **веских** причин редактировать этот параметр, рекомендуется оставить ему значение по-умолчанию.

* * *

`MaxTradeHoldDuration` - параметр типа `byte` со значением по-умолчанию `15`. Этот параметр определяет максимальную длительность задержки обмена в днях, при которой мы согласны его принять - ASF будет отклонять обмены с задержкой более `MaxTradeHoldDuration` дней. Этот параметр влияет только на ботов у которых `TradingPreferences` имеет значение `SteamTradeMatcher`, и не затрагивает ни обменов с `Master`/`SteamOwnerID` ни пожертвований. Удержания обменов не нравятся никому, и никто не хочет иметь с ними дела. ASF предполагает работу по либеральным правилам и помощь каждому, независимо от удержания обмена - поэтому по-умолчанию значение этого параметра `15`. Однако, если вы предпочитаете отклонять все обмены с удержанием, вы можете указать здесь `​0​`. Также учитывайте, что карты с коротким сроком жизни не подвержены влиянию этого параметра, и обмены с ними отклоняются в случае наличия задержек, как описано в разделе "**[Обмены](https://github.com/JustArchi/ArchiSteamFarm/wiki/Trading-ru-RU)**", поэтому нет нужды глобально отклонять все обмены только из-за этого. Если у вас нет причин редактировать этот параметр, рекомендуется оставить ему значение по-умолчанию.

* * *

`OptimizationMode` - параметр типа `byte` со значением по-умолчанию `‌‌0‌‌`. Этот параметр определяет режим оптимизации, который ASF будет использовать при работе. На данный момент ASF поддерживает два режима - `​0​`, который называется `MaxPerformance`(Максимальная производительность), и `1` который называется `MinMemoryUsage`(Минимальное потребление памяти). По умолчанию ASF предпочитает как можно больше вещей делать параллельно (одновременно), что увеличивает производительность распределяя нагрузку на всех ядрах ЦПУ, нескольких потоках ЦПУ, нескольких сокетах и нескольких очередях задач. Например, ASF запросит первую страницу значков чтобы узнать какие игры фармить, и после завершения этого запроса прочитает сколько страниц со значками у вас есть, и запросит их все одновременно. Это то что вам нужно **почти всегда**, поскольку избыточность минимальна, а выгода от использования асинхронного кода ASF может быть заметна даже на очень старом железе с одноядерным CPU и ограниченной производительностью. Однако, когда много задач выполняется параллельно, ASF отвечает за их поддержку, как например поддержку открытых сокетов, запущенных потоков и обрабатываемых задач, что иногда может привести к увеличению использования памяти, и если вы очень ограничены доступной памятью, возможно вы захотите переключить значение этого параметра в `1` (`MinMemoryUsage`), что приведёт к тому что ASF будет последовательно выполнять код, который мог бы быть распараллелен. Вам стоит задумываться о переключении этой опции только если вы уже прочли раздел "**[Конфигурация для низкого потребления ОЗУ](https://github.com/JustArchi/ArchiSteamFarm/wiki/Low-memory-setup-ru-RU)**" и намеренно хотите пожертвовать огромным приростом производительности, ради очень небольшого выигрыша в объёме памяти. Обычно результаты от использования этого параметра **гораздо хуже** чем результаты, которых можно достичь другими путями, такими как ограниченное использование ASF или настройка сборщика мусора, как описано в разделе "**[Конфигурация для низкого потребления ОЗУ](https://github.com/JustArchi/ArchiSteamFarm/wiki/Low-memory-setup)**". Поэтому вам стоит использовать `MinMemoryUsage` только как **крайнюю меру**, перед перекомпиляцией исполняемых файлов, если вы не смогли достичь удовлетворительных результатов другими (гораздо лучшими) способами. Если у вас нет **веских** причин редактировать этот параметр, рекомендуется оставить ему значение по-умолчанию.

* * *

`Statistics` - параметр типа `bool` со значением по-умолчанию `true`. Этот параметр определяет, будет ли в ASF включена статистика. Подробное описание того, что делает этот параметр, доступно в разделе "**[Статистика](https://github.com/JustArchi/ArchiSteamFarm/wiki/Statistics-ru-RU)**". Если у вас нет причин редактировать этот параметр, рекомендуется оставить ему значение по-умолчанию.

* * *

`SteamOwnerID` - параметр типа `ulong` со значением по-умолчанию `‍‍0‍‍`. Этот параметр задаёт 64-битный SteamID владельца процеса ASF, и очень похож на права доступа `Master` в настройках конкретного бота (но работающий глобально). Почти всегда вам нужно задать в этом параметре ID вашего основного аккаунта Steam. Права доступа `Master` даёт полный контроль над конкретным ботом, но глобальные команды, такие как `exit`, `restart` или `update` доступны только для `SteamOwnerID`. Это удобно если вы захотите запускать у себя ботов ваших друзей, не давая им при этом контроля над процессом ASF, например не позволяя завершить его командой `exit`. Значение по-умолчанию `​0​` указывает что у процесса ASF нет владельца, то есть никто не сможет использовать глобальные команды. Помните, что **[IPC](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC-ru-RU)** работает с `SteamOwnerID`, поэтому если вы хотите им пользоваться вам нужно указать здесь правильное значение.

* * *

`SteamProtocols` - `byte flags` type with default value of `7`. Этот параметр определяет протоколы по которым ASF будет соединяться с серверами Steam, которые задаются следующим образом:

| Значение | Имя       | Описание                                                                                         |
| -------- | --------- | ------------------------------------------------------------------------------------------------ |
| ​0​      | None      | Отсутствие протокола                                                                             |
| 1        | TCP       | **[Transmission Control Protocol](https://ru.wikipedia.org/wiki/Transmission_Control_Protocol)** |
| 2        | UDP       | **[User Datagram Protocol](https://ru.wikipedia.org/wiki/UDP)**                                  |
| 4        | WebSocket | **[WebSocket](https://ru.wikipedia.org/wiki/WebSocket)**                                         |

Обратите внимание что это параметр типа `flags`, и следовательно ему может быть присвоена любая комбинация приведенных выше значений. Ознакомьтесь с **[типами параметров в JSON](#Типы-параметров-в-json)** если хотите узнать больше. Если вы не включите ни один флаг, результатом будет вариант `None`, который сам по себе не является верным.

По-умолчанию ASF использует все доступные протоколы в качестве меры борьбы с простоями и другими проблемами Steam. Возможно вы захотите изменить этот параметр если вы хотите ограничить ASF использованием только одного или двух определённых протоколов вместо всех доступных. Такие меры могут понадобиться например если у вас есть брандмауэр на котором разрешён только TCP-трафик и вы не хотите чтобы ASF пытался подключиться по UDP. Однако, если вы не пытаетесь отладить какую-то конкретную проблему, почти всегда будет лучше дать ASF возможность выбрать любой из доступных протоколов, а не только один или два. Если у вас нет **веских** причин редактировать этот параметр, рекомендуется оставить ему значение по-умолчанию.

* * *

`UpdateChannel` - `byte` type with default value of `1`. This property defines update channel which is being used, either for auto-updates (if `UpdatePeriod` is greater than `0`), or update notifications (otherwise). Currently ASF supports three update channels - `0` which is called `None`, `1`, which is called `Stable`, and `2`, which is called `Experimental`. `Stable` channel is the default release channel, which should be used by majority of users. `Experimental` channel in addition to `Stable` releases, also includes **pre-releases** dedicated for advanced users and other developers in order to test new features, confirm bugfixes or give feedback about planned enhancements. **Experimental versions often contain unpatched bugs, work-in-progress features or rewritten implementations**. If you don't consider yourself advanced user, please stay with default `1` (Stable) update channel. `Experimental` channel is dedicated to users who know how to report bugs, deal with issues and give feedback - no technical support will be given. Check out ASF **[release cycle](https://github.com/JustArchi/ArchiSteamFarm/wiki/Release-cycle)** if you'd like to learn more. You can also set `UpdateChannel` to `0` (`None`), if you want to completely remove all version checks, although this is not recommended, unless for some reason you don't want to even receive notifications about new versions. Setting `UpdateChannel` to `0` will entirely disable all functions related to updates, including `update` command.

* * *

`UpdatePeriod` - `byte` type with default value of `24`. This property defines how often ASF should check for auto-updates. Updates are crucial not only to receive new features and critical security patches, but also to receive bugfixes, performance enhancements, stability improvements and more. When a value greater than `0` is set, ASF will automatically download, replace, and restart itself (if `AutoRestart` permits) when new update is available. In order to achieve this, ASF will check every `UpdatePeriod` hours if new update is available on our GitHub repo. A value of `0` disables auto-updates, but still allows you to execute `update` command manually. You might also be interested in setting appropriate `UpdateChannel` that `UpdatePeriod` should follow.

Update process of ASF involves update of entire folder structure that ASF is using, but without touching your own configs or databases located in `config` directory - this means that any extra files unrelated to ASF in its directory might be lost during update. Default value of `24` is a good balance between unnecessary checks, and ASF that is fresh enough.

Unless you have a **strong** reason to disable this feature, you should keep auto-updates enabled within reasonable `UpdatePeriod` **for your own good**. This is not only because we don't support anything but latest stable ASF release, but also because **we give our security guarantee only for latest version**. If you're using outdated ASF version then you're on your own, including a need to deal with all potential unpatched bugs, such as **[notifications loop](https://github.com/JustArchi/ArchiSteamFarm/commit/2d767c41aacb33dda35a98fa4b41efb7d33f5ffe)** that was triggered by Steam network protocol change. This quickly made ASF flood Steam network infinitely, being an easy way to get a Steam community ban in notime due to **[DoS](https://en.wikipedia.org/wiki/Denial-of-service_attack)** potential. Auto-updates allow us to react quickly to such issues by disabling or patching problematic features before they can escalate - if you opt out of it, you lose all of our security guarantees and risk consequences from running code that could be potentially harmful, either to Steam network, or (also by definition) to your own Steam account.

* * *

`WebLimiterDelay` - `ushort` type with default value of `200`. This property defines, in miliseconds, the minimum amount of delay between sending two consecutive requests to the same Steam web-service. Such delay is required as **[AkamaiGhost](https://www.akamai.com)** service that Steam uses internally includes rate-limiting based on global number of requests sent across given time period. In normal circumstances akamai block is rather hard to achieve, but under very heavy workloads with a huge ongoing queue of requests, it's possible to trigger it if we keep sending too many requests across too short time period.

Default value was set based on assumption that ASF is the only tool accessing Steam web-services, in particular `steamcommunity.com`, `api.steampowered.com` and `store.steampowered.com`. If you're using another tool sending requests to the same web-services then you should make sure that your tool includes similar functionality of `WebLimiterDelay` and set both to double of default value, which would be `400`. This guarantees that under no circumstance you'll exceed sending more than 1 request per 200 ms.

In general, lowering `WebLimiterDelay` under default value is **strongly discouraged** as it might lead to various IP-related blocks, some of which are possible to be permanent. Default value is good enough for running a single ASF instance on the server, as well as using ASF in normal scenario along with original Steam client. It should be correct for majority of usages, and you should only increase it (never lower it), if - apart from using ASF, you're also using another tool that might send excessive number of requests to the same web-services that ASF is making use of. In short, global number of all requests sent from a single IP to a single Steam domain should never exceed more than 1 request per 200 ms.

Если у вас нет причин редактировать этот параметр, рекомендуется оставить ему значение по-умолчанию.

* * *

`WebProxy` - `string` type with default value of `null`. This property defines a web proxy address that will be used for all internal http and https requests sent by ASF's `HttpClient`, especially to services such as `github.com`, `steamcommunity.com` and `store.steampowered.com`. Proxying ASF requests in general has no advantages, but it's exceptionally useful for bypassing various kinds of firewalls, especially the great firewall in China.

This property is defined as uri string:

> Строка URI состоит из схемы (http или https), имени хоста, и необязательного порта. Пример полной строки uri: `"http://contoso.com:8080"`.

If your proxy requires user authentication, you will also need to set up `WebProxyUsername` and/or `WebProxyPassword`. If there is no such need, setting up this property alone is sufficient.

Right now ASF uses web proxy only for `http` and `https` requests, which **do not** include internal Steam network communication done within ASF's internal Steam client. There are currently no plans for supporting that, mainly due to missing **[SK2](https://github.com/SteamRE/SteamKit)** functionality. If you need/want it to happen, I'd suggest starting from there.

Если у вас нет причин редактировать этот параметр, рекомендуется оставить ему значение по-умолчанию.

* * *

`WebProxyPassword` - `string` type with default value of `null`. This property defines password field used in basic, digest, NTLM, and Kerberos authentication that is supported by a target `WebProxy` machine providing proxy functionality. If your proxy doesn't require user credentials, there is no need for you to input anything here. Using this option makes sense only if `WebProxy` is used as well, as it has no effect otherwise.

Если у вас нет причин редактировать этот параметр, рекомендуется оставить ему значение по-умолчанию.

* * *

`WebProxyUsername` - `string` type with default value of `null`. This property defines username field used in basic, digest, NTLM, and Kerberos authentication that is supported by a target `WebProxy` machine providing proxy functionality. If your proxy doesn't require user credentials, there is no need for you to input anything here. Using this option makes sense only if `WebProxy` is used as well, as it has no effect otherwise.

Если у вас нет причин редактировать этот параметр, рекомендуется оставить ему значение по-умолчанию.

* * *

**[Вернуться к началу](#Конфигурация)**

* * *

## Конфигурация бота

As you should know already, every bot should have its own config. Example bot config is included in `example.json` file, which should be used for bot configuration. Simply **copy paste** `example.json` to a new file, and remember to name it appropriately, as it will be your bot instance. You should start from configuring your **primary** account, so some good suggestions for filename is `primary.json`, `1.json` or `YourNickname.json`.

**Notice:** There are several names which are reserved and can't be used for bot configs. Those are: **ASF**, **example** and **minimal**. ASF will ignore such configuration files. ASF will also ignore configuration files starting with a dot.

After deciding how you want to name your bot, open its file, and start with configuration. You should notice following structure:

```json
{
    "AcceptGifts": false,
    "AutoSteamSaleEvent": false,
    "BotBehaviour": 0,
    "CustomGamePlayedWhileFarming": null,
    "CustomGamePlayedWhileIdle": null,
    "DismissInventoryNotifications": false,
    "Enabled": false,
    "FarmingOrder": 0,
    "FarmOffline": false,
    "GamesPlayedWhileIdle": [],
    "HandleOfflineMessages": false,
    "HoursUntilCardDrops": 3,
    "IdlePriorityQueueOnly": false,
    "IdleRefundableGames": true,
    "LootableTypes": [
        1,
        3,
        5
    ],
    "MatchableTypes": [
        5
    ],
    "PasswordFormat": 0,
    "Paused": false,
    "RedeemingPreferences": 0,
    "SendOnFarmingFinished": false,
    "SendTradePeriod": 0,
    "ShutdownOnFarmingFinished": false,
    "SteamLogin": null,
    "SteamMasterClanID": 0,
    "SteamParentalPIN": "0",
    "SteamPassword": null,
    "SteamTradeToken": null,
    "SteamUserPermissions": {},
    "TradingPreferences": 0,
    "UseLoginKeys": true
}
```

**Tip:** In order for bot to work properly, you should edit at least `Enabled`, `SteamLogin` and `SteamPassword` properties. I also suggest to take a look at some fine-tuning such as `HoursUntilCardDrops`, but all of that is optional. ASF configs are quite advanced to allow you tune your bots and ASF however you want, if you don't "require" such advanced setup, you don't really have to go deep into each config property. It's up to you how simple or how complex ASF should be.

* * *

Все параметры описаны ниже:

`AcceptGifts` - `bool` type with default value of `false`. When enabled, ASF will automatically accept and redeem all steam gifts received by the bot. This includes also gifts from users different than defined in `SteamUserPermissions`. This option is recommended only for alt accounts, as it's very likely that you don't want to automatically redeem all gifts sent to your primary account. Keep in mind that gifts sent to e-mail address are not directly forwarded to the client, so ASF won't accept those gifts (without your help), therefore you should be sending steam gifts to your bots directly. Если вы не уверены, нужна ли вам эта возможность, оставьте её равной значению по-умолчанию `false`.

* * *

`AutoSteamSaleEvent` - `bool` type with default value of `false`. During Steam summer/winter sale events Steam is known for providing you extra cards for browsing discovery queue each day, as well as voting in the Steam awards. When this option is enabled, ASF will automatically check Steam discovery queue and Steam awards each 6 hours, and clear them if needed. This option is not recommended if you want to do those actions yourself, and typically it should make sense only on bot accounts. Moreover, you need to ensure that your account is at least of level `8` if you expect to receive those cards in the first place. Если вы не уверены, нужна ли вам эта возможность, оставьте её равной значению по-умолчанию `false`.

Please note that due to constant Valve issues, changes and problems, **we give no guarantee whether this function will work properly**, therefore it's entirely possible that this option **will not work at all**. We do not accept **any** bug reports, neither support requests for this option. It's offered with absolutely no guarantees, you're using it at your own risk.

* * *

`BotBehaviour` - `byte flags` type with default value of `0`. This property defines ASF bot-like behaviour during various events, and is defined as below:

| Значение | Имя                        | Описание                                                                           |
| -------- | -------------------------- | ---------------------------------------------------------------------------------- |
| ​0​      | None                       | Бот не имеет особых реакций, наименее агрессивный режим, используется по умолчанию |
| 1        | RejectInvalidFriendInvites | ASF будет отклонять (а не игнорировать) неверные запросы на добавление в друзья    |
| 2        | RejectInvalidTrades        | ASF будет отклонять (а не игнорировать) неверные предложения обмена                |
| 4        | RejectInvalidGroupInvites  | ASF будет отклонять (а не игнорировать) неверные приглашения в группы              |

Обратите внимание что это параметр типа `flags`, и следовательно ему может быть присвоена любая комбинация приведенных выше значений. Ознакомьтесь с **[типами параметров в JSON](#Типы-параметров-в-json)** если хотите узнать больше. Not enabling any of flags results in `None` option.

In general you want to modify this property if you expect from ASF to do certain amount of automation related to invalid activity, as it'd be expected from a bot account, but not a primary account used in ASF. Therefore, changing this property makes sense mainly for alt accounts, although you're free to use it for main accounts too.

Normal (`None`) ASF behaviour is to only automate things that user wants (e.g. cards farming or `SteamTradeMatcher` offers, if set in `TradingPreferences`). This is the least invasive mode, and it's beneficial to majority of users since you remain in full control over your account and you can decide yourself whether to allow certain out-of-scope interactions, or not.

Invalid friend invite is an invite that doesn't come from user with `FamilySharing` permission (defined in `SteamUserPermissions`) or above. ASF in normal mode ignores those invites, as you'd expect, giving you free choice whether to accept them, or not. `RejectInvalidFriendInvites` will cause those invites to be automatically rejected, which will practically disable option for other people to add you to their friend list (as ASF will deny all such requests, apart from people defined in `SteamUserPermissions`). Unless you want to outright deny all friend invites, you shouldn't enable this option.

Invalid trade offer is an offer that isn't accepted through built-in ASF module. More on this matter can be found in **[trading](https://github.com/JustArchi/ArchiSteamFarm/wiki/Trading)** section which explicitly defines what types of trade ASF is willing to accept automatically. Valid trades are also defined by other settings, especially `TradingPreferences`. `RejectInvalidTrades` will cause all invalid trade offers to be rejected, instead of being ignored. Unless you want to outright deny all trade offers that aren't automatically accepted by ASF, you shouldn't enable this option.

Invalid group invite is an invite that doesn't come from `SteamMasterClanID` group. ASF in normal mode ignores those group invites, as you'd expect, allowing you to decide yourself if you want to join particular Steam group or not. `RejectInvalidGroupInvites` will cause all those group invites to be automatically rejected, effectively making it impossible to invite you to any other group than `SteamMasterClanID`. Unless you want to outright deny all group invites, you shouldn't enable this option.

If you're unsure how to configure this option, it's best to leave it at default.

* * *

`CustomGamePlayedWhileFarming` - `string` type with default value of `null`. When ASF is farming, it can display itself as "Playing non-steam game: `CustomGamePlayedWhileFarming`" instead of currently farmed game. This can be useful if you would like to let your friends know that you're farming, yet you don't want to use `FarmOffline` feature. Please note that ASF cannot guarantee the actual display order of Steam network, therefore this is only a suggestion that may, or may not, display properly. Default value of `null` disables this feature.

* * *

`CustomGamePlayedWhileIdle` - `string` type with default value of `null`. Similar to `CustomGamePlayedWhileFarming`, but for the situation when ASF has nothing to do (as account is fully farmed). Default value of `null` disables this feature.

* * *

`DismissInventoryNotifications` - `bool` type with default value of `false`. Every card drop triggers inventory notification - steam notification telling you that you received new items. This can get annoying pretty fast, and serves little to no purpose, therefore ASF offers dismissing those notifications automatically. When you enable this option, ASF will automatically dismiss all notifications related to new items being received - this also includes items you obtained through trading and other ways. Of course, this option affects only inventory notifications, so all other notification types, e.g. profile comments notifications, will stay in-tact.

* * *

`Enabled` - `bool` type with default value of `false`. This property defines if bot is enabled. Enabled bot instance (`true`) will automatically start on ASF run, while disabled bot instance (`false`) will need to be started manually. By default every bot is disabled, so you probably want to switch this property to `true` for all of your bots that should be started automatically.

* * *

`FarmingOrder` - `byte` type with default value of `0`. This property defines the **preferred** farming order of ASF. Currently there are following farming orders available:

| Значение | Имя                       | Описание                                                                                |
| -------- | ------------------------- | --------------------------------------------------------------------------------------- |
| ​0​      | Unordered                 | Без сортировки, незначительно улучшает производительность ЦПУ                           |
| 1        | AppIDsAscending           | Пытаться сначала фармить игры с самым маленьким `appID`                                 |
| 2        | AppIDsDescending          | Пытаться сначала фармить игры с самым большим `appID`                                   |
| 3        | CardDropsAscending        | Пытаться сначала фармить игры с наименьшим количеством доступных для выпадения карточек |
| 4        | CardDropsDescending       | Пытаться сначала фармить игры с наибольшим количеством доступных для выпадения карточек |
| 5        | HoursAscending            | Пытаться сначала фармить игры с наименьшим количеством наигранных часов                 |
| 6        | HoursDescending           | Пытаться сначала фармить игры с наибольшим количеством наигранных часов                 |
| 7        | NamesAscending            | Пытаться фармить игры в алфавитном порядке, начиная с A                                 |
| 8        | NamesDescending           | Пытаться фармить игры в обратном алфавитном порядке, начиная с Z                        |
| 9        | Random                    | Пытаться фармить игры в случайном порядке                                               |
| 10       | BadgeLevelsAscending      | Пытаться сначала фармить игры с самым маленьким уровнем значка                          |
| 11       | BadgeLevelsDescending     | Пытаться сначала фармить игры с самым большим уровнем значка                            |
| 12       | RedeemDateTimesAscending  | Пытаться сначала фармить игры, которые добавлены на аккаунт раньше других               |
| 13       | RedeemDateTimesDescending | Пытаться сначала фармить игры, которые добавлены на аккаунт позже других                |

Notice the word "try" in all above descriptions - the actual order is heavily affected by selected **[cards farming algorithm](https://github.com/JustArchi/ArchiSteamFarm/wiki/Performance)** and sorting will affect only results that ASF considers same performance-wise. For example, in `Simple` algorithm, selected `FarmingOrder` should be entirely respected in current farming session (as every game is treated the same), while in `Complex` algorithm actual order is affected by hours and then sorted according to chosen `FarmingOrder`. This will lead to different results, as post-`HoursUntilCardDrops` games have higher priority over pre-`HoursUntilCardDrops` ones. It effectively means that ASF will idle post-`HoursUntilCardDrops` in your `FarmingOrder` first, then adapting your `FarmingOrder` for choosing the next batch. Therefore, this config property is only a **suggestion** that ASF will try to respect, as long as it doesn't affect performance negatively (in this case, ASF will prefer performance over `FarmingOrder`).

There is also idling priority queue that is accessible through `iq` **[commands](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)**. If it's used, actual idling order is sorted firstly by performance, then by idling queue, and finally by `FarmingOrder`.

* * *

`FarmOffline` - `bool` type with default value of `false`. Offline farming is extremely useful for primary accounts. As you should know, farming a game actually shows your steam status as "Playing game: XXX", which can be misleading to your friends, confusing them that you're playing a game while actually you're only farming it. Offline farming solves that issue - your account will never be shown as "in-game" when you're farming steam cards with ASF. This is possible thanks to the fact that ASF does not have to sign in into Steam Community in order to work properly, so we're in fact playing those games, but in "semi-offline" mode. Keep in mind that played games using offline farming will still count towards your playtime, and show as "recently played" on your profile. In addition to that, this feature is also important if you want to receive notifications and unread messages, if you keep ASF open while not keeping Steam client open at the same time. This is because ASF acts as a Steam client itself, and you're not receiving e.g. unread messages if in fact your account is online for the entire time and receiving messages through ASF - farming offline in this case is extremely useful, as all messages that arrive while you were offline, even if ASF is running (farming offline), are properly marked as unread and forwarded to you when you come back. Also, bots with `FarmOffline` feature enabled can't react to commands (directly), which is important if you decide to use that feature with alt accounts (see: `HandleOfflineMessages`). If you're unsure whether you want this feature enabled or not, it's suggested to use a value of `true` for primary accounts, and `false` otherwise.

* * *

`GamesPlayedWhileIdle` - `HashSet<uint>` type with default value of being empty. If ASF has nothing to farm it can play your specified steam games (`appID`s) instead. Playing games in such manner increases your "hours played" of those games, but nothing else apart of it. This feature can be enabled at the same time with `CustomGamePlayedWhileIdle` in order to play your selected games while showing custom status in Steam network, but in this case, like in `CustomGamePlayedWhileFarming` case, the actual display order is not guaranteed. Please note that Steam allows ASF to play only up to `32` `appID`s, therefore if you put more games than that, only first `32` will be respected (and extra ones being ignored).

* * *

`HandleOfflineMessages` - `bool` type with default value of `false`. When `FarmOffline` feature is enabled, bot is not able to receive commands in usual way, as it's not logged into Steam Community. To overcome this problem, ASF has also support for Steam offline messages that can be activated here. If you use `FarmOffline` on your alt accounts, you can consider switching this property to `true` in order to still be able to send commands to your offline bots, and receive responses. Keep in mind that this feature is based on offline steam messages, and receiving them automatically marks them as read, therefore this option is NOT recommended for primary accounts, as ASF will be forced to read and mark all offline messages as received in order to listen for offline commands - this affects also offline messages from your friends that are not ASF commands.

It's also worth mentioning that this option is basically a hack that might, or might not work correctly, based on whether Steam network actually will save those unread messages as offline messages in the first place. We've already seen many situations when it did not, so it's entirely possible that your bot won't receive your commands regardless, unless you disable `FarmOffline` altogether. Если вы не уверены, нужна ли вам эта возможность, оставьте её равной значению по-умолчанию `false`.

* * *

`HoursUntilCardDrops` - `byte` type with default value of `3`. This property defines if account has card drops restricted, and if yes, for how many initial hours. Restricted card drops means that account is not receiving any card drops from given game until the game is played for at least `HoursUntilCardDrops` hours. Unfortunately there is no magical way to detect that, so ASF relies on you. This property affects **[cards farming algorithm](https://github.com/JustArchi/ArchiSteamFarm/wiki/Performance)** that will be used. Setting this property properly will maximize profits and minimize time required for cards to be farmed. Remember that there is no obvious answer whether you should use one or another value, since it fully depends on your account. It seems that older accounts which never asked for refund have unrestricted card drops, so they should use a value of `0`, while new accounts and those who did ask for refund have restricted card drops with a value of `3`. This is however only theory, and should not be taken as a rule. The default value for this property was set based on "lesser evil" and majority of use cases.

* * *

`IdlePriorityQueueOnly` - `bool` type with default value of `false`. This property defines if ASF should consider for automatic idling only apps that you added yourself to priority idling queue available with `iq` **[commands](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)**. When this option is enabled, ASF will skip all `appIDs` that are missing on the list, effectively allowing you to cherry-pick games for automatic ASF idling. Keep in mind that if you didn't add any games to the queue then ASF will act as if there is nothing to idle on your account. Если вы не уверены, нужна ли вам эта возможность, оставьте её равной значению по-умолчанию `false`.

* * *

`IdleRefundableGames` - `bool` type with default value of `true`. This property defines if ASF is permitted to idle games that are still refundable. A refundable game is a game that we bought in last 2 weeks through Steam Store and we didn't play it for longer than 2 hours yet, as stated **[here](https://store.steampowered.com/steam_refunds)**. By default when this option is set to `true`, ASF ignores Steam refunds entirely and idles everything, as most people expect. However, you can change this option to `false` if you want to ensure that ASF won't idle any of your refundable games too soon, allowing you to evaluate those games yourself and refund if needed without worrying about ASF affecting playtime negatively. Please note that if you disable this option then games you purchased from Steam Store won't be idled by ASF for up to 14 days since redeem date. If you're unsure whether you want this feature enabled or not, keep it with default value of `true`.

* * *

`LootableTypes` - `HashSet<byte>` type with default value of `1, 3, 5` steam item types. This property defines ASF behaviour when looting - both manual and automatic. ASF will ensure that only items from `LootableTypes` will be included in a trade offer, therefore this property allows you to choose what you want to receive in a trade offer that is being sent to you.

| Значение | Имя               | Описание                                                                  |
| -------- | ----------------- | ------------------------------------------------------------------------- |
| ​0​      | Unknown           | Типы предметов, которые не попадают ни в одну из категорий ниже           |
| 1        | BoosterPack       | Наборы карточек                                                           |
| 2        | Emoticon          | Смайлики, используемые в чате Steam                                       |
| 3        | FoilTradingCard   | Аналог `TradingCard`, но для металлических карточек                       |
| 4        | ProfileBackground | Фоны, используемые в вашем профиле Steam                                  |
| 5        | TradingCard       | Коллекционные карточки Steam, используемые для создания значков (обычных) |
| 6        | SteamGems         | Самоцветы и мешки самоцветов, используемые для создания наборов карточек  |

Please note that regardless of the settings above, ASF will only ask for Steam (`appID` of 753) community (`contextID` of 6) items, so all game items, gifts and likewise, are excluded from the trade offer by definition.

Default ASF setting is based on most common usage of the bot, with looting only booster packs, and trading cards (including foils). The property defined here allows you to alter that behaviour in whatever way that satisfies you. Please keep in mind that all types not defined above will show as `Unknown` type, which is especially important when Valve releases some new Steam item, that will be marked as `Unknown` by ASF as well, until it's added here (in the future release). That's why in general it's not recommended to include `Unknown` type in your `LootableTypes`, unless you know what you're doing, and you also understand that ASF will send your entire inventory in a trade offer if Steam Network gets broken again and reports all your items as `Unknown`. My strong suggestion is to not include `Unknown` type in the `LootableTypes`, even if you expect to loot everything.

* * *

`MatchableTypes` - `HashSet<byte>` type with default value of `5` Steam item types. This property defines which Steam item types are permitted to be matched when `SteamTradeMatcher` option in `TradingPreferences` is enabled. Types are defined as below:

| Значение | Имя               | Описание                                                                  |
| -------- | ----------------- | ------------------------------------------------------------------------- |
| ​0​      | Unknown           | Типы предметов, которые не попадают ни в одну из категорий ниже           |
| 1        | BoosterPack       | Наборы карточек                                                           |
| 2        | Emoticon          | Смайлики, используемые в чате Steam                                       |
| 3        | FoilTradingCard   | Аналог `TradingCard`, но для металлических карточек                       |
| 4        | ProfileBackground | Фоны, используемые в вашем профиле Steam                                  |
| 5        | TradingCard       | Коллекционные карточки Steam, используемые для создания значков (обычных) |
| 6        | SteamGems         | Самоцветы и мешки самоцветов, используемые для создания наборов карточек  |

Of course, types that you should use for this property typically include only `2`, `3`, `4` and `5`, as only those types are supported by STM. Please note that **ASF is not a trading bot** and **will NOT care about price or rarity**, which means that if you use it e.g. with `Emoticon` type, then ASF will be happy to trade your 2x rare emoticon for 1x rare 1x common, as that makes progress towards badge (in this case emoticons) completion. Please evaluate twice if you're fine with that. Unless you know what you're doing, you should keep it with default value of `5`.

* * *

`PasswordFormat` - `byte` type with default value of `0`. This property defines the format of `SteamPassword` property, and currently supports - `0` for `PlainText`, `1` for `AES` and `2` for `ProtectedDataForCurrentUser`. Please refer to **[Security](https://github.com/JustArchi/ArchiSteamFarm/wiki/Security)** section if you want to learn more, as you'll need to ensure that `SteamPassword` property indeed includes password in matching `PasswordFormat`. Unless you know what you're doing, you should keep it with default value of `0`.

* * *

`Paused` - `bool` type with default value of `false`. This property defines initial state of `CardsFarmer` module. With default value of `false`, bot will automatically start farming when it's started, either because of `Enabled` or `start` command. Switching this property to `true` should be done only if you want to manually `resume` automatic farming process, for example because you want to use `play` all the time and never use automatic `CardsFarmer` module - this works exactly the same as `pause` **[command](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)**. Если вы не уверены, нужна ли вам эта возможность, оставьте её равной значению по-умолчанию `false`.

* * *

`RedeemingPreferences` - `byte flags` type with default value of `0`. This property defines ASF behaviour when redeeming cd-keys, and is defined as below:

| Значение | Имя              | Описание                                                                                                |
| -------- | ---------------- | ------------------------------------------------------------------------------------------------------- |
| ​0​      | None             | Нет предпочтений по активации, по-умолчанию                                                             |
| 1        | Forwarding       | Передавать ключи, которые не удалось активировать, другим ботам                                         |
| 2        | Distributing     | Распределять все ключи между этим и другими ботами                                                      |
| 4        | KeepMissingGames | Не передавать ключи для игр, которые (возможно) отсутствуют на аккаунте, оставлять их неиспользованными |

Обратите внимание что это параметр типа `flags`, и следовательно ему может быть присвоена любая комбинация приведенных выше значений. Ознакомьтесь с **[типами параметров в JSON](#Типы-параметров-в-json)** если хотите узнать больше. Not enabling any of flags results in `None` option.

`Forwarding` will cause bot to forward a key that is not possible to redeem, to another connected and logged on bot that is missing that particular game (if possible to check). The most common situation is forwarding `AlreadyPurchased` game to another bot that is missing that particular game, but this option also covers other scenarios, such as `DoesNotOwnRequiredApp`, `RateLimited` or `RestrictedCountry`.

`Distributing` will cause bot to distribute all received keys among itself and other bots. This means that every bot will get a single key from the batch. Typically this is used only when you're redeeming many keys for the same game, and you want to evenly distribute them among your bots, as opposed to redeeming keys for various different games. This feature makes no sense if you're redeeming only one key in a single `redeem` action (as there are no extra keys to be distributed).

`KeepMissingGames` will cause bot to skip `Forwarding` when we can't be sure if key being redeemed is in fact owned by our bot, or not. This basically means that `Forwarding` will apply **only** to `AlreadyPurchased` keys, instead of covering also other cases such as `DoesNotOwnRequiredApp`, `RateLimited` or `RestrictedCountry`. Typically you might want to use this option on primary account, to ensure that keys being redeemed on it won't be forwarded further if your bot for example becomes temporarily `RateLimited`. As you can guess from the description, this field has absolutely no effect if `Forwarding` is not enabled.

Enabling both `Forwarding` and `Distributing` will add distributing feature on top of forwarding one, which makes ASF trying to redeem one key on all bots firstly (forwarding) before moving to the next one (distributing). Typically you want to use this option only when you want `Forwarding`, but with altered behaviour of switching the bot on key being used, instead of always going in-order with every key (which would be `Forwarding` alone). This behaviour can be beneficial if you know that majority or even all of your keys are tied to the same game, because in this situation `Forwarding` alone would try to redeem everything on one bot firstly (which makes sense if your keys are for unique games), and `Forwarding` + `Distributing` will switch the bot on the next key, "distributing" the task of redeeming new key onto another bot than the initial one (which makes sense if keys are for the same game, skipping one pointless attempt per key).

The actual bots order for all of the redeeming scenarios is alphabetical, excluding bots that are unavailable (not connected, stopped or likewise). Please keep in mind that there is per-IP and per-account hourly limit of redeeming tries, and every redeem try that didn't end with `OK` contributes to failed tries. ASF will do its best to minimize number of `AlreadyPurchased` failures, e.g. by trying to avoid forwarding a key to another bot that already owns that particular game, but it's not always guaranteed to work due to how Steam is handling licenses. Using redeeming flags such as `Forwarding` or `Distributing` will always increase your likehood to hit `RateLimited`.

Also keep in mind that you can't forward or distribute keys to bots that you do not have access to. This should be obvious, but ensure that you're at least `Operator` of all the bots you want to include in your redeeming process, for example with `status ASF` **[command](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)**.

* * *

`SendOnFarmingFinished` - `bool` type with default value of `false`. When ASF is done with farming given account, it can automatically send steam trade containing everything farmed up to this point to user with `Master` permission, which is very convenient if you don't want to bother with trades yourself. This option works the same as `loot` command, therefore keep in mind that it requires user with `Master` permission set, you might also require valid `SteamTradeToken`, including using an account that is actually eligible for trading. In addition to initiating `loot` after finishing farming, ASF will also initiate `loot` on each new items notification (when not farming), and after completing each trade that results in new items (always) when this option is active. This is especially useful for "forwarding" items received from other people to our account. It's strongly recommended to use this feature together with **[ASF 2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Two-factor-authentication)** being set, as there is no point in sending automatic trades if you need to manually confirm them. If you're not sure how to set this property, leave it with default value of `false`.

* * *

`SendTradePeriod` - `byte` type with default value of `0`. This property works very similar to `SendOnFarmingFinished` property, with one difference - instead of sending trade when farming is done, we can also send it every `SendTradePeriod` hours, regardless of how much we have to farm left. This is useful if you want to `loot` your alt accounts on usual basis instead of waiting for it to finish farming. Default value of `0` disables this feature, if you want your bot to send you trade e.g. every day, you should put `24` here. It's strongly recommended to use this feature together with **[ASF 2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Two-factor-authentication)** being set, as there is no point in sending automatic trades if you need to manually confirm them. If you're not sure how to set this property, leave it with default value of `0`.

* * *

`ShutdownOnFarmingFinished` - `bool` type with default value of `false`. ASF is "occupying" an account for the whole time of process being active. When given account is done with farming, ASF periodically checks it (every `IdleFarmingPeriod` hours), if perhaps some new games with steam cards were added in the meantime, so it can resume farming of that account without a need to restart the process. This is useful for majority of people, as ASF can automatically resume farming when needed. However, you may actually want to stop the process when given account is fully farmed, you can achieve that by setting this property to `true`. When enabled, ASF will proceed with logging off when account is fully farmed, which means that it won't be periodically checked or occupied anymore. You should decide yourself if you prefer ASF to work on given bot instance for the whole time, or if perhaps ASF should stop it when farming process is done. When all accounts are stopped and process is not running in `--process-required` **[mode](https://github.com/JustArchi/ArchiSteamFarm/wiki/Command-Line-Arguments)**, ASF will shutdown as well. If you're not sure how to set this property, leave it with default value of `false`.

* * *

`SteamLogin` - `string` type with default value of `null`. This property defines your steam login - the one you use for logging in to steam. In addition to defining steam login here, you may also keep default value of `null` if you want to enter your steam login on each ASF startup instead of putting it in the config. This may be useful for you if you don't want to save sensitive data in config file.

* * *

`SteamMasterClanID` - `ulong` type with default value of `0`. This property defines the steamID of the steam group that bot should automatically join, including group chat. You can check steamID of your group by navigating to its **[page](https://steamcommunity.com/groups/ascfarm)**, then adding `/memberslistxml/?xml=1` to the end of the link, so the link will look like **[this](https://steamcommunity.com/groups/ascfarm/memberslistxml/?xml=1)**. Then you can get steamID of your group from the result, it's in `<groupID64>` tag. In above example it would be `103582791440160998`. If you don't have any "farm group" for your bots, you should keep it at default.

* * *

`SteamParentalPIN` - `string` type with default value of `0`. This property defines your steam parental PIN. ASF requires an access to resources protected by steam parental, therefore if you use that feature, you need to provide ASF with parental unlock PIN, so it can operate normally. Default value of `0` means that there is no steam parental PIN required to unlock this account, and this is probably what you want if you don't use steam parental functionality. In addition to defining steam parental PIN here, you may also use value of `null` if you want to enter your steam parental PIN on each ASF startup instead of putting it in the config. This may be useful for you if you don't want to save sensitive data in config file.

* * *

`SteamPassword` - `string` type with default value of `null`. This property defines your steam password - the one you use for logging in to steam. In addition to defining steam password here, you may also keep default value of `null` if you want to enter your steam password on each ASF startup instead of putting it in the config. This may be useful for you if you don't want to save sensitive data in config file.

* * *

`SteamTradeToken` - `string` type with default value of `null`. When you have your bot on your friend list, then bot can send a trade to you right away without worrying about trade token, therefore you can leave this property at default value of `null`. If you however decide to NOT have your bot on your friend list, then you will need to generate and fill a trade token as the user that this bot is expecting to send trades to. In other words, this property should be filled with trade token of the account that is defined with `Master` permission in `SteamUserPermissions` of **this** bot instance.

In order to find your token, as logged in user with `Master` permission, navigate **[here](https://steamcommunity.com/my/tradeoffers/privacy)** and take a look at your trade URL. The token we're looking for is made out of 8 characters after `&token=` part in your trade URL. You should copy and put those 8 characters here, as `SteamTradeToken`. Do not include whole trading URL, neither `&token=` part, only token itself.

* * *

`SteamUserPermissions` - `Dictionary<ulong, byte>` type with default value of being empty. This property is a dictionary property which maps given Steam user identified by his 64-bit steam ID, to `byte` number that specifies his permission in ASF instance. Currently available bot permissions in ASF are defined as:

| Значение | Имя           | Описание                                                                                                                                                                                                                     |
| -------- | ------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ​0​      | None          | Никаких прав, это зарезервированное значение, которое нужно только чтобы описывать те steam ID, которые отсутствуют в этом массиве - нет необходимости задавать какой-то аккаунт с такими правами                            |
| 1        | FamilySharing | Даёт минимальный доступ для пользователей использующих семейный доступ. Опять же, это скорее зарезервированное значение, поскольку ASF способен автоматически обнаруживать каким steam ID разрешён доступ к вашей библиотеке |
| 2        | Operator      | Даёт основные права доступа к боту, в основном для активации ключей и добавления бесплатных лицензий                                                                                                                         |
| 3        | Master        | Даёт полный доступ к данному боту                                                                                                                                                                                            |

In short, this property allows you to handle permissions for given users. Permissions are important mainly for access to ASF **[commands](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)**, but also for enabling many ASF features, such as accepting trades. For example you might want to set your own account as `Master`, and give `Operator` access to 2-3 of your friends so they can easily redeem keys for your bot with ASF, while **not** being eligible e.g. for stopping it. Thanks to that you can easily assign permissions to given users and let them use your bot to some specified by you degree.

We recommend to set exactly one user as `Master`, and any amount you wish as `Operator`s and below. While it's technically possible to set multiple `Master`s and ASF will work correctly with them, for example by accepting all of their trades sent to the bot, ASF will use only one of them (with lowest steam ID) for every action that requires a single target, for example a `loot` request, so also properties like `SendOnFarmingFinished` or `SendTradePeriod`. If you perfectly understand those limitations, especially the fact that `loot` request will always send items to the `Master` with lowest steam ID, regardless of the `Master` that actually executed the command, then you can define multiple users with `Master` permission here, but we still recommend a single master scheme - multiple masters scheme is discouraged setup that is not supported.

It's nice to note that there is one more extra `Owner` permission, which is declared as `SteamOwnerID` global config property. You can't assign `Owner` permission to anybody here, as `SteamUserPermissions` property defines only permissions that are related to the bot instance, and not ASF as a process.

* * *

`TradingPreferences` - `byte flags` type with default value of `0`. This property defines ASF behaviour when in trading, and is defined as below:

| Значение | Имя                 | Описание                                                                                                                                                                                                                                 |
| -------- | ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ​0​      | None                | Нет предпочтений при обменах - принимаются только обмены от `Master`                                                                                                                                                                     |
| 1        | AcceptDonations     | Принимаются предложения обменов, в которых бот не отдаёт никаких предметов                                                                                                                                                               |
| 2        | SteamTradeMatcher   | Принимаются предложения обменов дубликатов карточек по правилам **[STM](https://www.steamtradematcher.com)**. Ознакомьтесь с разделом "**[Обмены](https://github.com/JustArchi/ArchiSteamFarm/wiki/Trading-ru-RU)**" чтобы узнать больше |
| 4        | MatchEverything     | Требует, чтобы был задан `SteamTradeMatcher`, и в сочетании с ним - также принимаются "плохие" сделки в дополнение к "хорошим" и "нейтральным"                                                                                           |
| 8        | DontAcceptBotTrades | Не принимаются автоматически предложения обмена, отправленные по команде `loot` другими ботами                                                                                                                                           |

Обратите внимание что это параметр типа `flags`, и следовательно ему может быть присвоена любая комбинация приведенных выше значений. Ознакомьтесь с **[типами параметров в JSON](#Типы-параметров-в-json)** если хотите узнать больше. Not enabling any of flags results in `None` option.

For further explanation of ASF trading logic, and description of every available flag, please visit **[Trading](https://github.com/JustArchi/ArchiSteamFarm/wiki/Trading)** section.

* * *

`UseLoginKeys` - `bool` type with default value of `true`. This property defines if ASF should use login keys mechanism for this Steam account. Login keys mechanism works very similar to official Steam client's "remember me" option, which makes it possible for ASF to store and use temporary one-time use login key for next logon attempt, effectively skipping a need of providing password, Steam Guard or 2FA code as long as our login key is valid. Login key is stored in `BotName.db` file and updated automatically. This is why you don't need to provide password/SteamGuard/2FA code after logging in successfully with ASF just once.

Login keys are used by default for your convenience, so you don't need to input `SteamPassword`, SteamGuard or 2FA code (when not using **[ASF 2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Two-factor-authentication)**) on each login. It's also superior alternative since login key can be used only for a single time and does not reveal your original password in any way. Exactly the same method is being used by your original Steam client, which saves your account name and login key for your next logon attempt, effectively being the same as using `SteamLogin` with `UseLoginKeys` and empty `SteamPassword` in ASF.

However, some people might be concerned even about this little detail, therefore this option is available here for you if you'd like to ensure that ASF won't store any kind of token that would allow resuming previous session after being closed, which will result in full authentication being mandatory on each login attempt. Disabling this option will work exactly the same as not checking "remember me" in official Steam client. Unless you know what you're doing, you should keep it with default value of `true`.

**[Вернуться к началу](#Конфигурация)**

* * *

## Файловая структура

ASF is using quite simple file structure.

    ├── config
    │     ├── ASF.json
    │     ├── ASF.db
    │     ├── Bot1.json
    │     ├── Bot1.db
    │     ├── Bot1.bin
    │     ├── Bot2.json
    │     ├── Bot2.db
    │     ├── Bot2.bin
    │     └── ...
    ├── ArchiSteamFarm.dll
    ├── log.txt
    └── ...
    

In order to move ASF to new location, for example another PC, it's enough to move/copy `config` directory alone, and that's the recommended way of doing any form of "ASF backups".

`log.txt` file holds the log generated by your last ASF run. This file doesn't contain any sensitive information, and is extremely useful when it comes to issues, crashes or simply as an information to you what happened in last ASF run. We will very often ask about for file if you run into issues or bugs. ASF automatically manages this file for you, but you can further tweak ASF **[logging](https://github.com/JustArchi/ArchiSteamFarm/wiki/Logging)** module if you're advanced user.

`config` directory is the place that holds configuration for ASF, including all of its bots.

`ASF.json` is a global ASF configuration file. This config is used for specifying how ASF behaves as a process, which affects all of the bots and program itself. You can find global properties there, such as ASF process owner, auto-updates or debugging.

`BotName.json` is a config of given bot instance. This config is used for specifying how given bot instance behaves, therefore those settings are specific to that bot only and not shared across other ones. This allows you to configure bots with various different settings and not necessarily all of them working in exactly the same way.

Apart from config files, ASF also uses `config` directory for storing databases.

`ASF.db` is a global ASF database file. It acts as a global persistent storage and is used for saving various information related to ASF process, such as IPs of local Steam servers. **Вам не следует редактировать этот файл**.

`BotName.db` is a database of given bot instance. This file is used for storing crucial data about given bot instance in persistent storage, such as login keys or ASF 2FA. **Вам не следует редактировать этот файл**.

`BotName.bin` is a special file of given bot instance, which holds information about Steam sentry hash. Sentry hash is used for authenticating using `SteamGuard` mechanism, very similar to Steam `ssfn` file. **Вам не следует редактировать этот файл**.

`BotName.keys` is a special file that can be used for importing keys into **[background games redeemer](https://github.com/JustArchi/ArchiSteamFarm/wiki/Background-games-redeemer)**. It's not mandatory and not generated, but recognized by ASF. This file is automatically deleted after keys are successfully imported.

`BotName.maFile` is a special file that can be used for importing **[ASF 2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Two-factor-authentication)**. It's not mandatory and not generated, but recognized by ASF if your `BotName` does not use ASF 2FA yet. This file is automatically deleted after ASF 2FA is successfully imported.

**[Вернуться к началу](#Конфигурация)**

* * *

## Типы параметров в JSON

Every configuration property has its type. Type of the property defines values that are valid for it. You can only use values that are valid for given type - if you use invalid value, then ASF won't be able to parse your config.

**We strongly recommend to use ConfigGenerator for generating configs** - it handles most of the low-level stuff (such as types validation) for you, so you only need to input proper values, and you also don't need to understand variable types specified below. This section is mainly for people generating/editing configs manually, so they know what values they can use.

Types used by ASF are native C# types, which are specified below:

* * *

`bool` - Boolean type accepting only `true` and `false` values.

Example: `"Enabled": true`.

* * *

`byte` - Unsigned byte type, accepting only integers from `0` to `255` (inclusive).

Example: `"FarmingOrder": 1`.

* * *

`uint` - Unsigned integer type, accepting only integers from `0` to `4294967295` (inclusive).

* * *

`ulong` - Unsigned long integer type, accepting only integers from `0` to `18446744073709551615` (inclusive).

Example: `"SteamMasterClanID": 103582791440160998`

* * *

`string` - String type, accepting any sequence of characters, including empty sequence `""` and `null`. Both empty sequence as well as `null` value is treated the same by ASF, so it's up to your preference which one you want to use.

Examples: `"SteamLogin": null`, `"SteamLogin": ""`, `"SteamLogin": "MyAccountName"`

* * *

`HashSet<valueType>` - Collection (set) of unique values in given `valueType`. In JSON, it's defined as array of elements in given `valueType`.

Example for `HashSet<uint>`: `"Blacklist": [267420, 303700, 335590]`

* * *

`Dictionary<keyType, valueType>` - A map that maps a key specified in its `keyType`, to value specified in its `valueType`. In JSON, it's defined as an object with key-value pairs. Keep in mind that `keyType` is always quoted in this case, even if it's value type such as `ulong`.

Example for `Dictionary<ulong, byte>`: `"SteamUserPermissions": { "76561198174813138": 3, "76561198174813137": 1 }`

* * *

`flags` - Flags attribute combines several different properties into one final value by applying bitwise operations. This allows you to choose any possible combination of various different allowed values at the same time. The final value is constructed as a sum of values of all enabled options.

For example, given following values:

| Значение | Имя  |
| -------- | ---- |
| ​0​      | None |
| 1        | A    |
| 2        | B    |
| 4        | C    |

Using `B + C` would result in value of `6`, using `A + C` would result in value of `5`, using `C` would result in value of `4` and so on. This allows you to create any possible combination of enabled values - if you decided to enable all of them, making `None + A + B + C`, you'd get value of `7`. Also notice that flag with value of `0` is enabled by definition in all other available combinations, therefore very often it's a flag that doesn't enable anything specifically (such as `None`).

So as you can see, in above example we have 3 available flags to switch on/off (`A`, `B`, `C`), and 8 possible values overall (`None -> 0`, `A -> 1`, `B -> 2`, `A+B -> 3`, `C -> 4`, `A+C -> 5`, `B+C -> 6`, `A+B+C -> 7`).

**[Вернуться к началу](#Конфигурация)**

* * *

## Совместимость типов

Due to JavaScript limitations of being unable to properly serialize simple `ulong` fields in JSON when using web-based ConfigGenerator, `ulong` fields will be rendered as strings with `s_` prefix in the resulting config. This includes for example `"SteamOwnerID": 76561198006963719` that will be written by our ConfigGenerator as `"s_SteamOwnerID": "76561198006963719"`. ASF includes proper logic for handling this string mapping automatically, so `s_` entries in your configs are actually valid and correctly generated. If you're generating configs yourself, we recommend to stick with original `ulong` fields if possible, but if you're unable to do so, you can also follow this scheme and encode them as strings with `s_` prefix added to their names. We hope to resolve this JavaScript limitation eventually.

**[Вернуться к началу](#Конфигурация)**

* * *

## Совместимость конфигураций

It's top priority for ASF to remain compatible with older configs. As you should already know, missing config properties are treated the same as they would be defined with their **default values**. Therefore, if new config property gets introduced in new version of ASF, all your configs will remain **compatible** with new version, and ASF will treat that new config property as it'd be defined with its **default value**. You can always add, remove or edit config properties according to your needs. We recommend to limit defined config properties only to those that you want to change, since this way you automatically inherit default values for all other ones, not only keeping your config clean but also increasing compatibility in case we decide to change a default value for property that you don't want to explicitly set yourself. Feel free to check `minimal.json` example configuration file that follows this concept.

**[Вернуться к началу](#Конфигурация)**

* * *

## Автоматическая перезагрузка

Starting with ASF V2.1.6.2+, the program is now aware of configs being modified "on-the-fly" - thanks to that, ASF will automatically:

- Создаёт (и при необходимости запускает) нового бота, если вы создали для него файл конфигурации
- Останавливает (при необходимости) и удаляет старого бота, если вы удалили его файл конфигурации
- Останавливает (и при необходимости запускает) любого бота, если вы изменили его файл конфигурации
- Перезапускает (при необходимости) бота с новым именем, если вы переименовали файл конфигурации

All of the above is transparent and will be done automatically without a need of restarting the program, or killing other (unaffected) bot instances.

In addition to that, ASF will also restart itself (if `AutoRestart` permits) if you modify core ASF `ASF.json` config. Likewise, program will quit if you delete or rename it.

**[Вернуться к началу](#Конфигурация)**