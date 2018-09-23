# IPC

ASF включает в себя собственный, уникальный интерфейс IPC который может использоваться для дальнейшего взаимодействия с процессом. IPC расшифровывается как **[inter-process communication](https://ru.wikipedia.org/wiki/%D0%9C%D0%B5%D0%B6%D0%BF%D1%80%D0%BE%D1%86%D0%B5%D1%81%D1%81%D0%BD%D0%BE%D0%B5_%D0%B2%D0%B7%D0%B0%D0%B8%D0%BC%D0%BE%D0%B4%D0%B5%D0%B9%D1%81%D1%82%D0%B2%D0%B8%D0%B5)** (межпроцессное взаимодействие) и если описать его максимально просто - это "API ASF", которое может быть использовано для дальнейшей интеграции с процессом.

Наш интерфейс IPC основан на **[Kestrel HTTP server](https://github.com/aspnet/KestrelHttpServer)** и предоставляет собой веб-API, построенное с учётом **[REST](https://ru.wikipedia.org/wiki/REST)** и основанное на JSON как основном формате данных. Вы можете получить доступ к API ASF отсылая соответствующие веб-запросы на конечные точки `/Api`.

В дополнение к конечным точкам API, наш IPC также предоставляет возможность размещать статичные файлы, используемые нашим фронтэндом IPC GUI, созданным для пользователей как альтернативный, упрощённый способ доступа к API ASF.

IPC может использоваться для множества различных вещей, в зависимости от ваших нужд и способностей. For example, you can use it for fetching status of ASF and all bots, sending ASF commands, fetching and editing global/bot configs, adding new bots, deleting existing bots, submitting keys for **[BGR](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer)** or accessing ASF's log file. Все эти действия доступны через наше API, а значит вы можете создавать собственные утилиты и скрипты, которые смогут взаимодействовать с ASF и влиять на него в процессе работы. В добавок к этому, часть действий (таких как отправка команд) также реализованы в нашем IPC GUI, который позволяет вам легко получить к ним доступ из вашего любимого браузера.

* * *

# Использование

You can enable our IPC interface by enabling `IPC` **[global configuration property](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#global-config)**. ASF сообщит о запуске IPC в своём журнале, который вы можете проверить чтобы узнать что IPC интерфейс удачно запущен:

    INFO|ASF|Start() Запуск IPC сервера...
    INFO|ASF|Start() IPC сервер готов!
    

Http сервер ASF теперь ожидает входящих запросов на нескольких конечных точках. Если вы не создали пользовательский конфигурационный файл для IPC, это будут адрес формата IPv4 **[127.0.0.1](http://127.0.0.1:1242)** и адрес формата IPv6 **[[::1]](http://[::1]:1242)** с портом по умолчанию `1242`. Вы можете получить доступ к интерфейсу IPC по ссылкам выше с той же машины где запущен ASF. Если интерфейс IPC ASF был активирован правильно, вы должны увидеть в вашем браузере интерфейс **[IPC GUI](#ipc-gui)**.

![IPC GUI](https://i.imgur.com/VjHtWYu.png)

* * *

# IPC GUI

Не забывайте, что IPC GUI на данном этапе находится **на предварительном рассмотрении**, а это значит что официально он **не поддерживается** и мы не принимаем сообщения об ошибках (отдельных проблемах) касающихся его. Это так потому, что это проект сообщества, и мы не поддерживаем его официально, у ASF нет разработчика, отвечающего за это.

You're free to post your thoughts, suggestions and bug reports in appropriate **[issue](https://github.com/JustArchiNET/ArchiSteamFarm/issues?q=is%3Aissue+is%3Aopen+label%3AIPC)** that is fully dedicated to IPC GUI. На данный момент ведущий разработчик IPC GUI со стороны сообщества **[@MrBurrBurr](https://github.com/mrburrburr)**.

* * *

# IPC API

Доступ к нашему IPC API может быть получен путём отправки соответствующих запросов на соответствующие конечные точки. Вы можете использовать эти конечные точки API для создания своих вспомогательных скриптов, утилит, интерфейсов и тому подобного. Именно это делает IPC GUI "под капотом", и любая утилита может делать то же самое. Отсылка запросов API официально полностью поддерживается командой ASF.

Взаимодействие с IPC сервером, предоставленным ASF, может быть произведено с помощью любой http-совместимой программы, утилиты или кода. В наших примерах ниже мы будем использовать **[curl](https://curl.haxx.se)**, это кросс-платформенная программа с открытым кодом для передачи данных. Вы можете легко адаптировать наши примеры с curl для своих собственных скриптов, утилит или программ.

* * *

## Коды состояния HTTP

Наше API использует стандартные коды состояния HTTP, и мы используем их в соответствии с RFC. Если объяснить упрощённо, наше API будет возвращать статус `200 OK` или вызов API успешен, и отличный от 200 в противном случае. Это следует использовать как основной способ определить, был ли вызов API ASF успешным или нет.

ASF может использовать различные коды состояния HTTP чтобы лучше объяснить, что произошло. Некоторые из них включают в себя:

- `200 OK` - запрос завершен успешно.
- `400 BadRequest` - запрос не удался из-за ошибки, проанализируйте тело ответа для получения реальной причины. В большинстве случае это означает ASF понял запрос, но отказывается его выполнить из-за приведенной причины.
- `401 Unauthorized` - в ASF установлен `IPCPassword` но вы не смогли успешно пройти **[аутентификацию](#Аутентификация)**.
- `403 Forbidden` - в ASF установлен `IPCPassword` но вы не смогли успешно пройти **[аутентификацию](#Аутентификация)** слишком много раз, попробуйте снова через час.
- `404 NotFound` - URL к которому вы пытаетесь получить доступ, не существует. Проверьте по примерам IPC, приведенным ниже, что вы пытаетесь обратиться к правильной конечной точке.
- `405 MethodNotAllowed` - Метод HTTP, который вы пытаетесь использовать, недопустим для этой конечной точки API. Причиной может быть, например, отправка запроса `GET` там где ASF ожидает `POST`, или наоборот. Эта же ошибка также может использоваться если вы пытаетесь получить доступ к конечной точке websocket не инициализировав соединение websocket (upgrade).
- `406 NotAcceptable` - заголовок `Content-Type` вашего запроса недопустим для этой конечной точки API. В основном используется для запросов, в которых требуется сформированное особым образом тело запроса, а вы не объявили в каком формате оно предоставлено.
- `411 LengthRequired` - ваш запрос `POST` не имеет заголовка `Content-Length`. Длина должна быть определена даже в запросах, которые не имеют тела (используйте в этом случае длину равную 0).
- `500 InternalServerError` - сервер IPC попал в критическое состояние, это указывает на проблему в ASF, о которой следует сообщить чтобы её исправили. В нормальной ситуации этот статус нигде не используется, пожалуйста сообщите нам если встретите его.
- `501 NotImplemented` - этот URL зарезервирован для будущего использования и на данный момент не реализован. В некоторых, очень редких, ситуациях, ASF может также использовать этот статус чтобы показать что понимает что вы пытаетесь сделать, но это (пока) не поддерживается.
- `503 ServiceUnavailable` - в ASF произошла одна из возможных ошибок в процессе выполнения. Если причина не возвращена в ответе, журнал ASF может помочь вам узнать подробности.

* * *

## Аутентификация

Интерфейс IPC ASF по умолчанию не нуждается ни в какой аутентификации, поскольку `IPCPassword` установлено значение `null`. Однако, если вы активировали `IPCPassword`, указав ему не пустое значение, любой запрос к API ASF требует пароль, совпадающий с установленным в `IPCPassword`. Если вы пропустили аутентификацию или указали неверный пароль, вы получите ошибку `401 - Unauthorized`. Если вы продолжите отправлять запросы без аутентификации, со временем вы получите временную блокировку с ошибкой `403 - Forbidden`.

Аутентификация может быть выполнена двумя поддерживаемыми способами.

### Заголовок `Authentication`

В основном вам следует использовать заголовки запроса HTTP, установив значение поля `Authentication` равным вашему паролю. Как этого добиться зависит от того, каким инструментом вы пользуетесь для доступа к интерфейсу IPC ASF, например если вы пользуетесь `curl` вам следует добавить в качестве аргумента `-H 'Authentication: MyPassword'`. Таким способом аутентификация передаётся в заголовках запроса, где и должна быть.

### Параметр `password` в строке запроса

В качестве альтернативы вы можете добавить параметр `password` в конец URL, который хотите вызвать, например вызывая `/Api/Command/version?password=MyPassword` вместо `/Api/Command/version`. Этот подход достаточно хорош для большинства случаев, поскольку он дружественный к пользователю, и может быть сохранён в качестве закладки, но очевидно что он в открытом виде показывает ваш пароль, что не всегда допустимо. В добавок это дополнительный параметр в строке запроса, что загромождает вид URL, и создаёт впечатление что он применим только к этому URL, хотя на самом деле он применяется ко всему обмену с IPC.

* * *

Оба способа поддерживаются совершенно одинаково, и это вам решать каким из них пользоваться. Мы всё же рекомендуем использовать заголовки HTTP везде где возможно, поскольку с точки зрения использования это более подходящий метод, чем строка запроса. Однако, мы поддерживаем строку запроса, поскольку из-за различных ограничений нет возможности использовать заголовки везде - например, невозможно указать пользовательские заголовки при инициализации соединения по протоколу websocket из javascript (хотя это и совершенно корректно согласно RFC).

* * *

## API

В основном наше API является типичным REST API основанном на JSON в качестве основного метода сериализации/десериализации данных. Мы стараемся максимально точно описать ответ, используя как коды состояний HTTP (когда это применимо), так и ответ, который вы можете самостоятельно разобрать чтобы выяснить, окончился ли запрос успешно, и если нет - то почему.

Некоторые конечные точки API могут потребовать от вас указать дополнительные данные, как например предоставить соответствующие структуры в теле запроса. В этом случае, в дополнение к требуемым данным, вы также должны установить заголовку `Content-Type` соответствующее значение, такое как `application/json` если ваши данные в формате JSON. Если конечная точка API имеет особые требования к входным данным, они будут перечислены вверху описания конечной точки.

Во всех приведенных ниже примерах вы должны изменить параметр URL добавив ему в начало соответствующие `Protocol://Host:Port/OptionalPrefix`, в соответствии с конфигурацией вашего ASF. Например, если вы пытаетесь использовать `/Api/ASF`, и IPC запущен на стандартном адресе `http://127.0.0.1:1242`, вы должны вместо этого вызывать `http://127.0.0.1:1242/Api/ASF`.

Цифровые параметры определены с максимальными значениями, поэтому вы можете использовать для них строгую типизацию, как например `uint` для `AppID`, и `ulong` для `SteamID`. Некоторые поля типа `ulong`, сериализуемые как числа, могут иметь дополнительные поля с префиксом `s_`, сериализуемые как строки, которые могут использоваться в javascript (и других языках с аналогичными ограничениями), где нет возможности точного представления 64-разрядных чисел.

* * *

## GenericResponse

Стандартный формат ответа (GenericResponse) это основной возвращаемый тип для всех вызовов API. Мы используем два вида стандартных ответов - с результатом, и без.

Стандартный ответ без результата имеет два обязательных поля, которые всегда присутствуют - `Success` и `Message`. Этот вид ответа используется для всех вызовов API которые не возращают никакого особого результата, кроме самого факта успеха/неудачи. Этот ответ в основном используется для запросов типа `DELETE` и `POST`, например для вызова `DELETE /Api/Bot/{Bot}`.

```json
{
    "Message": "string",
    "Success": true
}
```

`Message` - значение типа `string`, содержащее дополнительные подробности ответа. Это может быть просто "OK" если запрос успешен, или реальная причина отказа если нет. Мы используем это поле как способ дать вам знать что случилось с отправленным вами запросом. Помните, что это поле НЕ результат вашего запроса, а только описание того, что произошло. Может быть равным `null` если у нас нет определённого сообщения для вас, хотя мы стараемся по возможности избегать таких ситуаций.

`Success` - `bool` value that specifies whether the request has been executed successfully. In comparison with HTTP status codes, this property determines the outcome of a particular action and not necessarily the call itself. Most of the time, that action will align nicely with HTTP status codes (`true` for `200 OK` and `false` otherwise), but not always, as the request might have a more complex intermediate state where the action itself is seen as failure but not the entire request. An example of such situation is trying to pause already paused bot, in which case the request will end with `200 OK` (the request was properly executed), but with `Success` of `false` as the bot was paused already. Depending on how you look at it, you might want to interpret it as a success or not, and this is why HTTP status code is more general way of saying whether something was achieved, and `Success` whether something that was actually responsible for that succeeded in doing so.

* * *

Стандартный ответ с результатом, в добавок к двум обязательным полям, описанным выше, также включает в себя обязательное поле `Result`, содержащее реальный результат вызова. Этот ответ используется в основном в вызовах `GET`, которые по определению должны возвращать результат, например `GET /Api/ASF`.

```json
{
    "Result": {},
    "Message": "string",
    "Success": true
}
```

`Result` - значение типа `object` содержащее результат вашего запроса. Тип этого поля зависит от вызванной конечной точки API- например это может быть `Bot` или `string`. Чаще всего используется в запросах `GET` для получения данных которые вы запросили. Хотя это поле имеет гибкий тип, конкретные конечные точки всегда гарантируют определённое количество возможных результатов, и очень часто можно использовать строгую типизацию для отдельно взятой конечной точки. Может быть равным `null` если у нас нет определённого результата для вас.

`Message`/`Success` - смотри выше.

* * *

## Публичные API

### `GET /Api/ASF`

Эта конечная точка API может быть использована для получения общих сведений о процессе ASF в целом. Возвращает **[GenericResponse](#genericresponse)** c полем `Result` определённым как **[ASFResponse](#asfresponse)**.

```shell
curl -X GET /Api/ASF
{"Message":"OK","Result":{"BuildVariant":"generic","GlobalConfig":{"AutoRestart":false,"Blacklist":[440]},"MemoryUsage":1843,"ProcessStartTime":"2018-01-30T21:32:01.8132984+01:00","Version":{"Major":3,"Minor":0,"Build":6,"Revision":1,"MajorRevision":0,"MinorRevision":1}},"Success":true}
```

#### ASFResponse

```json
{
    "BuildVariant": "string",
    "GlobalConfig": {},
    "MemoryUsage": 4294967295,
    "ProcessStartTime": "9999-12-31T23:59:59.9999999+12:00",
    "Version": {
        "Major": 2147483647,
        "Minor": 2147483647,
        "Build": 2147483647,
        "Revision": 2147483647,
        "MajorRevision": 32767,
        "MinorRevision": 32767
    }
}
```

`BuildVariant` - значение типа `string`, содержащее вариант сборки запущенного процесса ASF. This can be any variant available under **[releases](https://github.com/JustArchiNET/ArchiSteamFarm/releases)**, as well as variants that are not officially available for download (such as `docker` variant for our Docker images or `source` variant for unofficial builds).

`GlobalConfig` - specialized C# object used by ASF for accessing to its config. It has exactly the same structure as **[global config](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#global-config)** explained in configuration. Это свойство может быть использовано чтобы узнать с какими параметрами настроен работать процесс ASF. Обычно этот объект будет включать только часть параметров конфигурации - те, которые пользователь изменил (минимальная конфигурация). Конфиденциальная информация, указанная в конфигурации процесса (например, `WebProxyPassword`), всегда исключается из выдачи.

`MemoryUsage` - значение типа `uint`, содержащее объём памяти **управляемого** кода, занятый процессом ASF в целом, в килобайтах.

`ProcessStartTime` - значение типа `DateTime`, содержащее точное время запуска процесса ASF. Может использоваться, например, для вычисления общего времени работы программы. Для JSON ASF сериализует объект `DateTime` в строку стандарта **[ISO 8601](https://ru.wikipedia.org/wiki/ISO_8601)** содержащую дату, время и используемый часовой пояс.

`Version` - значение типа `Version`, содержащее версию запущенного двоичного файла ASF. Тип `Version` содержит 4 важных свойства `Major`, `Minor`, `Build` и `Revision`, которые соответствуют цифрам в записи `A.B.C.D` версии ASF.

* * *

### `POST /Api/ASF`

#### Тело:

```json
{
    "GlobalConfig": {},
    "KeepSensitiveDetails": true
}
```

This API endpoint can be used for updating **[GlobalConfig](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#global-config)** of ASF program. In other words, this will update `ASF.json` of `config` directory with **[GlobalConfig](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#global-config)** JSON object supplied in request body. Возвращает **[GenericResponse](#genericresponse)** без результата.

`GlobalConfig` is **[GlobalConfig](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#global-config)** JSON object. Это поле является обязательным и не может быть равным `null`. Параметры конфигурации со значением, равным значению по-умолчанию, могут быть опущены, как и в обычной конфигурации ASF.

`KeepSensitiveDetails` - это значение типа `bool`, указывающее должна ли конфиденциальная информация, такая как `WebProxyPassword`, быть скопирована из существующей конфигурации (если есть). Это поле необязательное, и по умолчанию считается равным `true`. Когда это поле включено, параметры с конфиденциальной информацией, указанные со значением `null`, будут скопированы из текущей конфигурации.

На данный момент следующие параметры считаются конфиденциальной информацией и могут быть заданы равными `null` для копирования из существующей конфигурации: `WebProxyPassword`.

```shell
curl -X POST -H "Content-Type: application/json" -d '{"GlobalConfig":{"AutoRestart":false,"Blacklist":[440]}}' /Api/ASF
{"Message":"OK","Success":true}
```

* * *

### `DELETE /Api/Bot/{BotNames}`

Эта конечная точка API может использоваться для полного удаления заданных ботов с указанными именами `BotNames`, вместе со всеми файлами. Иными словами, это удалит файлы `BotName.*` (включая, но не ограничиваясь `json`, `db`, `bin`, `maFile`, `keys` и т.д.) из вашей папки `config` для всех заданных ботов. Эта конечная точка принимает несколько имен ботов в `BotNames`, разделённых запятыми, а также ключевое слово `ASF` для удаления всех имеющихся ботов. Возвращает **[GenericResponse](#genericresponse)** без результата.

```shell
curl -X DELETE /Api/Bot/archi
{"Message":"OK","Success":true}
```

* * *

### `GET /Api/Bot/{BotNames}`

Эта конечная точка API может быть использована для получения сведений о состоянии ботов с указанными `BotNames` - она возвращает базовые сведения о состоянии ботов. Эта конечная точка принимает несколько имен ботов в `BotNames`, разделённых запятыми, а также кодовое слово `ASF` для выбора всех существующих ботов. Returns **[GenericResponse](#genericresponse)** с полем `Result` определённым как `HashSet<Bot>` - массив сообщений о состоянии ботов.

```shell
curl -X GET /Api/Bot/archi
{"Message":"OK","Result":[{"BotName":"archi","CardsFarmer":{"CurrentGamesFarming":[],"GamesToFarm":[],"TimeRemaining":"00:00:00","Paused":false},"AccountFlags":0,"AvatarHash":"99bf6df8ad1836c0205de22935f6fe4b1f96b0c6","IsPlayingPossible":true,"SteamID":0,"BotConfig":null,"KeepRunning":false}],"Success":true}
```

#### Bot

```json
{
    "BotName": "string",
    "CardsFarmer": {
        "GamesToFarm": [{
            "AppID": 4294967295,
            "GameName": "string",
            "HoursPlayed": 3.40282347E+38,
            "CardsRemaining": 65535
        }],
        "CurrentGamesFarming": [{
            "AppID": 4294967295,
            "GameName": "string",
            "HoursPlayed": 3.40282347E+38,
            "CardsRemaining": 65535
        }],
        "TimeRemaining": "02:30:00",
        "Paused": false
    },
    "AccountFlags": 4294967295,
    "AvatarHash": "string",
    "IsPlayingPossible": true,
    "SteamID": 18446744073709551615,
    "BotConfig": {},
    "KeepRunning": true
}
```

`BotName` это параметр типа `string`, содержащий имя бота. Это тот же самый идентификатор, который используется для команд и других действий с ботами.

`CardsFarmer` это специализированный объект C#, используемый ботом для фарма карточек. Он содержит информацию, связанную с процессом фарма карточек для данного бота. Его структура описана **[ниже](#cardsfarmer)**.

`AccountFlags` это параметр типа `EAccountFlags` (флаги `uint`), определённый в SK2 (**[тут](https://github.com/SteamRE/SteamKit/blob/e0c46a45d3fbf3bf1b0df3e2e0348dc1067458df/Resources/SteamLanguage/enums.steamd#L82-L117)**), который содержит флаги аккаунта Steam для данного аккаунта. Этот параметр может использоваться для получения дополнительной информации о состоянии аккаунта Steam, используемого в ASF, например чтобы узнать что он **[ограниченный](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663)** путём проверки флагов `LimitedUser` или `LimitedUserForce`. Этот параметр инициализируется (и обновляется) в тот момент когда бот входит в сеть Steam, поэтому он будет иметь значение `0` до первого входа.

`AvatarHash` это параметр типа `string`, содержащий хеш аватара Steam используемого данным ботом. Есть возможность использовать это значение для формирования URL, ведущего к аватару пользователя, сохранённому в CDN Steam. Может иметь значение `null` если пользователь не установил аватар.

`IsPlayingPossible` это параметр типа `bool`, указывающий на то, что аккаунт, используемый в боте, может быть использован для автоматического фарма. Этот параметр будет иметь значение `false`, если библиотека Steam этого аккаунта уже где-то используется, либо напрямую, либо через Family Sharing. Этот параметр учитывает только удалённые сеансы, и не включает в себя собственный процесс (поэтому если аккаунт не используется где-то ещё, `IsPlayingPossible` всегда будет равен `true`, даже если ASF активно фармит игры на нём прямо сейчас).

`SteamID` это параметр типа `ulong`, содержащий уникальный идентификатор steamID используемого аккаунта в 64-битном формате. Этот параметр будет иметь значение `0`, если бот не вошёл в сеть Steam (и поэтому может использоваться для определения того, произведен вход на аккаунте или нет).

`BotConfig` is specialized C# object used by Bot for accessing to its config. It has exactly the same structure as **[bot config](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#bot-config)** explained in configuration. Этот параметр может использоваться для того, чтобы определить настройки, с которыми бот сконфигурирован работать. Обычно этот объект будет включать только часть параметров конфигурации - те, которые пользователь изменил (минимальная конфигурация). Конфиденциальная информация об аккаунте, такая как `SteamLogin`, `SteamPassword` и `SteamParentalPIN` всегда исключается из выдачи.

`KeepRunning` это параметр типа `bool`, указывающий на то что заданный бот в данный момент активен. Активный бот - это бот, который был запущен, либо самим ASF при старте, либо пользователем в процессе работы. Если бот остановлен, этот параметр будет иметь значение `false`. Помните, что этот параметр не имеет ничего общего со входом в сеть Steam (для определения входа может использоваться параметр `SteamID`).

#### CardsFarmer

`GamesToFarm` это объект типа `HashSet<Game>` (массив элементов типа `Game`) содержащий игры, которые запланировано фармить в текущей сессии. Пожалуйста, обратите внимание, что этот массив обновляется по мере необходимости, с учётом производительности. Например, если вы фармите используя алгоритм фарма `Simple`, ASF не будет проверять есть ли у нас новые игры для фарма при добавлении игры на аккаунт (поскольку мы всё равно проверим это по окончании текущей очереди, а не делая этого немедленно мы уменьшим количество запросов и загрузку канала). Поэтому эти данные верны только для текущей сессии фарма, которая может отличаться от глобальных данных.

`CurrentGamesFarming` это объект типа `HashSet<Game>` (массив элементов типа `Game`) который содержит игры, которые фармятся прямо сейчас. По сравнению с `GamesToFarm`, этот параметр определяет текущее состояние, а не очередь ожидания, и существенно зависит от выбранного алгоритма фарма карточек. Этот массив может содержать не более `32` игр (задаётся параметром `MaxGamesPlayedConcurrently` со стороны сети Steam). Также, вы можете быть уверены, что в этом массиве могут быть только элементы, уже присутствующие в `GamesToFarm`.

`TimeRemaining` это параметр типа `TimeSpan`, содержащий приблизительное расчетное время, требуемое на окончание фарма игр, заданных в `GamesToFarm`. Этот параметр даже приблизительно не соответствует времени, которое реально потребуется, но это неплохой индикатор, точность которого может быть улучшена в дальнейшем, и подходит для различных задач отображения состояния. Этот параметр не обновляется в режиме реального времени, он вычисляется исходя из текущего значения `GamesToFarm`, и соответственно пересчитывается в момент изменения `CardsRemaining` в любой игре.

`Paused` это параметр типа `bool`, содержащий признак того, что модуль `CardsFarmer` находится в режиме паузы. CardsFarmer может быть в режиме паузы из-за различных событий, в основном из-за получения команд `pause` и `play`. Находясь в режиме паузы, CardsFarmer не будет пытаться фармить в автоматическом режиме, и не будет проверять страницы значков каждые `IdleFarmingPeriod` часов.

#### Game

`AppID` это параметр типа `uint`, содержащий уникальный идентификатор запущенной игры. ASF следит чтобы этот параметр всегда был больше `0`.

`GameName` это параметр типа `string`, содержащий название игры с идентификатором `AppID`. Эти данные возвращает Steam Community. ASF следит чтобы это значение соответствовало критериям `non-null` и `non-empty`.

`HoursPlayed` это параметр типа `float`, содержащий информацию о том, сколько часов была запущена игра. Этот параметр обновляется в режиме реального времени, но по необходимости, не реже чем каждые `FarmingDelay` минут. Обратите, пожалуйста, внимание, что исходно эти данные получаются от Steam Community, но затем обновляются согласно встроенным таймерам ASF, и поэтому могут не соответствовать тому, что вовзращает Steam Community, поскольку Steam Community также не обновляется в режиме реального времени, а ASF требуются эти данные чтобы остановить фарм для накрутки времени как только этот параметр достигает значения `HoursUntilCardDrops`. ASF следит чтобы этот параметр был не менее `0.0`.

`CardsRemaining` это параметр типа `ushort`, содержащий количество карточек, доступных для получения из этой игры. Этот параметр обновляется как можно быстрее, и всегда должен иметь значение больше `0`. Однако, возможна ситуация когда этот параметр будет иметь значение `0` в коротком интервале когда ASF переключается на следующую игру.

* * *

### `POST /Api/Bot/{BotName}`

#### Тело:

```json
{
    "BotConfig": {},
    "KeepSensitiveDetails": true
}
```

This API endpoint can be used for creating/updating **[BotConfig](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#bot-config)** of given bot specified by its `BotName`. In other words, this will update `BotName.json` of `config` directory with **[BotConfig](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#bot-config)** JSON object supplied in request body. Возвращает **[GenericResponse](#genericresponse)** без результата.

`BotConfig` is **[BotConfig](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#bot-config)** JSON object. Это поле является обязательным и не может быть равным `null`. Параметры конфигурации со значением, равным значению по-умолчанию, могут быть опущены, как и в обычной конфигурации ASF.

`KeepSensitiveDetails` это параметр типа `bool`, задающий, должна ли конфиденциальная информация, такая как `SteamLogin` или `SteamPassword` наследоваться из существующей конфигурации (если есть). Это поле необязательное, и по умолчанию считается равным `true`. Когда это поле включено, параметры с конфиденциальной информацией, указанные со значением `null`, будут скопированы из текущей конфигурации.

На данный момент следующие параметры считаются конфиденциальной информацией и могут быть заданы равными `null` для копирования из существующей конфигурации: `SteamLogin`, `SteamPassword`, `SteamParentalPIN`.

```shell
curl -X POST -H "Content-Type: application/json" -d '{"BotConfig":{"Enabled":false,"Paused":true}}' /Api/Bot/archi
{"Message":"OK","Success":true}
```

* * *

### `POST /Api/Command/{Command}`

#### Тело: пустое

Эта конечная точка API может использоваться для выполнения команды, заданной `{Command}`. Рекомендуется всегда указывать имя бота, который должен выполнить команду, иначе вместо этого будет использован первый заданный бот. Возвращает **[GenericResponse](#genericresponse)** c полем `Result` определённым как `string` и содержащим результат исполнения команды.

```shell
curl -X POST -d '' /Api/Command/version
{"Message":"OK","Result":"\r\n<archi> ASF V3.0.5.3","Success":true}
```

* * *

### `DELETE /Api/GamesToRedeemInBackground/{BotName}`

Эта конечная точка API может использоваться для полноо удаления файлов `.keys.used` и `.keys.unused` из папки `config` для бота заданного `BotNames`. Эта конечная точка принимает несколько имен ботов в `BotNames`, разделённых запятыми, а также ключевое слово `ASF` для удаления указанных файлов у всех имеющихся ботов. Возвращает **[GenericResponse](#genericresponse)** без результата.

```shell
curl -X DELETE /Api/GamesToRedeemInBackground/archi
{"Message":"OK","Success":true}
```

* * *

### `GET /Api/GamesToRedeemInBackground/{BotName}`

Эта конечная точка API может использоваться для получения файлов `.keys.used` и `.keys.unused` из папки `config` для бота заданного `BotNames`. Возвращает **[GenericResponse](#genericresponse)** с полем `Result` определенным как `Dictionary<string, GamesToRedeemInBackgroundResponse>` - ассоциативный массив, задающий соответствие `BotName` к **[GamesToRedeemInBackgroundResponse](#gamestoredeeminbackgroundresponse)** (описан ниже).

```shell
curl -X GET /Api/GamesToRedeemInBackground/archi
{"Message":"OK","Result":{"archi":{"UnusedKeys":{"AAAAA-BBBBB-CCCCC":"Orwell","XXXXX-YYYYY-ZZZZZ":"Factorio"},"UsedKeys":{}}},"Success":true}
```

#### GamesToRedeemInBackgroundResponse

```json
{
    "UnusedKeys": {
        "string": "string",
        "string": "string"
    },
    "UsedKeys": {
        "string": "string",
        "string": "string"
    }
}
```

`UnusedKeys` и `UsedKeys` это объекты типа `Dictionary<string, string>`, задающие соответствие ключей Steam (`key`) с их названиями (`value`). Это результат запроса `POST`, описанного ниже, и может использоваться для удаленного получения ключей без доступа к папке `config` ASF. Оба эти объекта могут иметь значение `null` в случае ошибки ASF при доступе к файлам (например, ошибки ввода-вывода), но пустые или отсутствующие файлы будут обработаны штатно и будет возвращен пустой массив.

* * *

### `POST /Api/GamesToRedeemInBackground/{BotName}`

#### Тело:

```json
{
    "GamesToRedeemInBackground": {
        "string": "string",
        "string": "string"
    }
}
```

This API endpoint can be used for adding extra **[games to redeem in background](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer)** to given bot specified by its `BotName`. Возвращает **[GenericResponse](#genericresponse)** with `Result` определённый как `OrderedDictionary<string, string>`.

`GamesToRedeemInBackground` это объект JSON типа `OrderedDictionary<string, string>` задающий соответствие между ключами для активации (`key`) и их названиями (`value`). Это поле является обязательным и не может быть равным `null`. Ни одно поле `key` или `value` в массиве не может быть пустым или равным `null`. В дополнение к этому, каждый `key` должен иметь правильную для ключа Steam структуру, ASF будет проверять это с помощью регулярного выражения. Неверные элементы будут автоматически удалены в процессе импорта.

`Result` это `GamesToRedeemInBackground` содержащий игры которые были успешно добавлены в очередь на активацию. Как указано выше, неверные элементы автоматически удаляются в процессе импорта, поэтому этот результат можно сравнить с исходным запросом для проверки, какие элементы были признаны неверными и пропущены в процессе импорта. Если в запросе не было неверных данных, этот результат должен быть в точности таким же, как `GamesToRedeemInBackground` который вы отправили ASF.

```shell
curl -X POST -H "Content-Type: application/json" -d '{"GamesToRedeemInBackground":{"AAAAA-BBBBB-CCCCC":"Orwell","XXXXX-YYYYY-ZZZZZ":"Factorio"}}' /Api/GamesToRedeemInBackground/archi
{"Message":"OK","Result":{"AAAAA-BBBBB-CCCCC":"Orwell","XXXXX-YYYYY-ZZZZZ":"Factorio"},"Success":true}
```

* * *

### `GET /Api/NLog`

Эта конечная точка API может использоваться для получения сообщений журнала ASF в реальном времени. По сравнению с другими конечными точками, эта использует соединение по протоколу **[websocket](https://ru.wikipedia.org/wiki/WebSocket)** для выдачи обновлений в реальном времени. Каждое сообщение кодируется в формате **[UTF-8](https://ru.wikipedia.org/wiki/UTF-8)** и содержит структуру **[GenericResponse](#genericresponse)** с полем `Result` определённым как `string` - в нём содержится сообщение, сформированное согласно заданной пользователем конфигурации NLog. В начале подключения, ASF также выдаст пакет из нескольких последних сообщений журнала в качестве краткой истории (по умолчанию последние 20, но пользователь может изменить это число, а также отключить выдачу истории полностью).

Подключение по протоколу websocket, установленное с помощью этой конечной точки, работает в режиме **только для чтения** - ASF будет принимать только **[управляющие пакеты](https://tools.ietf.org/html/rfc6455#section-5.5)**, в особенности пакет `Close` индицирующий что соединение должно быть штатно завершено. Попытка отправить любой пакет с данными приведёт к разрыву соединения.

```shell
curl -X GET -i -N -H "Connection: Upgrade" -H "Upgrade: websocket" /Api/NLog
HTTP/1.1 200 OK

# Example of messages being sent by ASF, keep in mind that result string is affected by user-specified NLog logging layout
{"Message":"OK","Result":"2018-01-31 03:19:34|dotnet-2884|INFO|ASF|Start() IPC server ready!","Success":true}
```

For backwards compatibility, this endpoint is also available under now deprecated `/Api/Log` call. We'll keep this call around until at least ASF V3.4.

* * *

### `GET /Api/Structure/{Structure}`

This API endpoint can be used for fetching structure of given JSON object specified by its `Structure` name - it returns JSON-serialized default object for given structure. Returns **[GenericResponse](#genericresponse)** with `Result` defined as `object`.

`{Structure}` can be any ASF or .NET Core structure qualified by its namespace and name, for example `ArchiSteamFarm.BotConfig`, `ArchiSteamFarm.GlobalConfig` or `ArchiSteamFarm.Json.Steam+Asset.`

In the example below the actual result was trimmed to keep it clean - normally you'll get full structure returned, which is the main purpose of this endpoint. The resulting structure always includes all public and non-public (but not private) fields and properties.

```shell
curl -X GET /Api/Structure/ArchiSteamFarm.BotConfig
{"Message":"OK","Result":{"AcceptGifts":false,"TradingPreferences":0},"Success":true}
```

In comparison with `GET /Api/Type`, this endpoint returns JSON representation of an object of given type, in its default state.

* * *

### `GET /Api/Type/{Type}`

This API endpoint can be used for fetching information about given type specified by its name. Returns **[GenericResponse](#genericresponse)** with `Result` defined as **[TypeResponse](#typeresponse)**.

`{Type}` can be any ASF or .NET Core type qualified by its namespace and name, for example `ArchiSteamFarm.BotConfig`, `ArchiSteamFarm.GlobalConfig` or `ArchiSteamFarm.Json.Steam+Asset.`

In the example below the actual result was trimmed to keep it clean - normally you'll get full structure returned, which is the main purpose of this endpoint. The resulting structure always includes all public and non-public fields and properties.

```shell
curl -X GET /Api/Type/ArchiSteamFarm.BotConfig
{"Message":"OK","Result":{"Body":{"AcceptGifts":"System.Boolean","TradingPreferences":"ArchiSteamFarm.BotConfig+ETradingPreferences"},"Properties":{"BaseType":"System.Object","CustomAttributes":null,"UnderlyingType":null}},"Success":true}
```

In comparison with `GET /Api/Structure`, this endpoint returns object of given type where all its values are encoded as string type of given property. You can also use this endpoint recursively, for example by checking how `ArchiSteamFarm.BotConfig+ETradingPreferences` in example above is exactly defined.

#### TypeResponse

```json
{
    "Body": {},
    "Properties": {
        "BaseType": "string",
        "CustomAttributes": [ "string" ],
        "UnderlyingType": "string"
    }
}
```

`Body` - `Dictionary<string, string>` value that specifies properties that are possible to set for given type. This includes all public and non-public (but not private) fields and properties of object of given type. `Key` of the collection is defined as name of given field/property, while `Value` of that key is defined as C# type that is valid for it. This property can be empty if given type doesn't include any fields or properties. We also use this property for further decomposition of given type, for example `BaseType` of `System.Enum` will have valid enum values declared here, where `Key` will be name of given enum value, and `Value` will be actual value for that name.

`Properties` - `TypeProperties` type defined **[below](#typeproperties)** that holds metadata information about given type.

#### TypeProperties

`BaseType` - `string` value that specifies base type for this type. For example, it'll be `System.Object` for `ArchiSteamFarm.BotConfig` object, and `System.Enum` for `ArchiSteamFarm.BotConfig+ETradingPreferences`. Based on this property you can partially strong-type `Body` content by knowing in advance how you should parse it (for example for `System.Enum`, `Body` will include enum names and values, as specified above in `Body` description).

`CustomAttributes` - `HashSet<string>` value that specifies what custom attributes apply to this type. This property is especially useful when `BaseType` is `System.Enum`, as in this case you can check if it's special `flags` enum by verifying that `System.FlagsAttribute` is defined in this collection. This value can be `null` when there are no custom attributes defined for this object. Together with `UnderlyingType`, this tells you that `ArchiSteamFarm.BotConfig+ETradingPreferences` is `byte flags` enum.

`UnderlyingType` - `string` value that specifies underlying type for this type. This is used mainly with `System.Enum` to know what underlying type this enum uses for data storage. For example in most ASF enums, this will be `System.Byte`. Together with `CustomAttributes`, this tells you that `ArchiSteamFarm.BotConfig+ETradingPreferences` is `byte flags` enum.

* * *

## Внутренние API

APIs below are dedicated for our IPC GUI usage and they should not be used by remote scripts or tools. This documentation is for our internal reference only and can change anytime, in any possible way. You should not rely on existence of below endpoints, neither implement them in your own tools.

* * *

### `GET /Api/WWW/Directory/{Directory}`

This API endpoint can be used for fetching directory's content specified by its local path relative to `www` directory. Returns **[GenericResponse](#genericresponse)** with `Result` defined as `HashSet<string>` - collection of local filenames.

```shell
curl -X GET /Api/WWW/Directory/css
{"Message":"OK","Result":["app.css","_all-skins.min.css","_nightmode.min.css"],"Success":true}
```

* * *

### `POST /Api/WWW/Send`

#### Тело:

Content-Type: application/json

```json
{
    "URL": "string"
}
```

This API endpoint is used internally for sending remote `GET` requests. This should be used only if **[CORS](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing)** doesn't permit sending the request in usual way. Returns **[GenericResponse](#genericresponse)** with `Result` defined as `string` - the inner html of the `GET` result.

`URL` is `string` type that specifies target URL to make a `GET` request. It must start with `https://`.

```shell
curl -X POST -H "Content-Type: application/json" -d '{"URL":"https://example.com"}' /Api/WWW/Send
{"Message":"OK","Result":"<!doctype html>\n<html>\n<head>\n    <title>Example Domain</title>...","Success":true}
```

* * *

## ЧАВО

### Безопасно ли использовать интерфейс IPC в ASF?

ASF by default listens only on `localhost` addresses, which means that accessing ASF IPC from any other machine but your own **is impossible**. Unless you modify default endpoints, attacker would need a direct access to your own machine in order to access ASF's IPC, therefore it's as secure as it can be and there is no possibility of anybody else accessing it, even from your own LAN.

However, if you decide to change default `localhost` bind addresses to something else, then you're supposed to set proper firewall rules **yourself** in order to allow only authorized IPs to access ASF's IPC interface. In addition to doing that, we strongly recommend to set up `IPCPassword`, that will add another layer of extra security. You might also want to run ASF's IPC interface behind a reverse proxy in this case, which is further explained below.

### Можно ли использовать IPC ASF за обратным прокси, таким как Apache или Nginx?

**Yes**, our IPC is fully compatible with such setup, so you're free to host it also in front of your own tools for extra security and compatibility, if you'd like to. In general ASF's Kestrel http server is very secure and possesses no risk when being connected directly to the internet, but putting it behind a reverse-proxy such as Apache or Nginx might provide extra functionality that wouldn't be possible to achieve otherwise, such as securing ASF's interface with a **[basic auth](https://en.wikipedia.org/wiki/Basic_access_authentication)**.

Example Nginx configuration can be found below. We included full `server` block, although you're interested mainly in `location` ones. Please refer to **[nginx documentation](https://nginx.org/en/docs)** if you need further explanation.

```nginx
server {
        listen *:443 ssl;
        server_name asf.mydomain.com;
        ssl_certificate /path/to/your/certificate.crt;
        ssl_certificate_key /path/to/your/certificate.key;

    location /Api/NLog {
        proxy_pass http://127.0.0.1:1242;
#       proxy_set_header Host 127.0.0.1; # Only if you need to override default host
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Host $host:$server_port;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header X-Forwarded-Server $host;
        proxy_set_header X-Real-IP $remote_addr;

        # We add those 3 extra options for websockets proxying, see https://nginx.org/en/docs/http/websocket.html
        proxy_http_version 1.1;
        proxy_set_header Connection "Upgrade";
        proxy_set_header Upgrade $http_upgrade;
    }

    location / {
        proxy_pass http://127.0.0.1:1242;
#       proxy_set_header Host 127.0.0.1; # Only if you need to override default host
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Host $host:$server_port;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header X-Forwarded-Server $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

### Могу ли я получить доступ к IPC через протокол HTTPS?

**Yes**, you can achieve it through two different ways. A recommended way would be to use a reverse proxy for that (described above) where you can access your web server through https like usual, and connect through it with ASF's IPC interface on the same machine. This way your traffic is fully encrypted and you don't need to modify IPC in any way to support such setup.

Second way includes specifying a **[custom config](#custom-configuration)** for ASF's IPC interface where you can enable https endpoint and provide appropriate certificate directly to our Kestrel http server. This way is recommended if you're not running any other web server and don't want to run one exclusively for ASF. Otherwise, it's much easier to achieve a satisfying setup by using a reverse proxy mechanism.

* * *

## Пользовательская конфигурация

Our IPC interface supports extra config file, `IPC.config` that should be put in standard ASF's `config` directory.

When available, this file specifies advanced configuration of ASF's Kestrel http server, together with other IPC-related tuning. Unless you have a particular need, there is no reason for you to use this file, as ASF is already using sensible defaults in this case.

The configuration file is based on following JSON structure:

```json
{
    "Kestrel": {
        "Endpoints": {
            "IPv4-http": {
                "Url": "http://127.0.0.1:1242"
            },
            "IPv6-http": {
                "Url": "http://[::1]:1242"
            },
            "IPv4-https": {
                "Url": "https://127.0.0.1:1242",
                "Certificate": {
                    "Path": "/path/to/certificate.pfx",
                    "Password": "passwordToPfxFileAbove"
                }
            },
            "IPv6-https": {
                "Url": "https://[::1]:1242",
                "Certificate": {
                    "Path": "/path/to/certificate.pfx",
                    "Password": "passwordToPfxFileAbove"
                }
            }
        },
        "PathBase": "/"
    }
}
```

There are 2 properties worth explanation/editing, those are `Endpoints` and `PathBase`.

`Endpoints` - This is a collection of endpoints, each endpoint having its own unique name (like `IPv4-http`) and `Url` property that specifies `Protocol://Host:Port` listening address. By default, ASF listens on IPv4 and IPv6 http addresses, but we've added https examples for you to use, if needed. You should declare only those endpoints that you need, we've included 4 example ones above so you can edit them easier.

`Host` accepts a variety of values, including `*` value that binds ASF's http server to all available interfaces. Be extremely careful when you use `Host` values that allow remote access. Doing so will enable access to ASF's IPC interface from other machines, which might pose a security risk. We strongly recommend to use `IPCPassword` and your own firewall at a minimum in this case.

`PathBase` - This is base path that will be used by IPC interface. This property is optional, defaults to `/` and shouldn't be required to modify for majority of use cases. By changing this property you'll host entire IPC interface on a custom prefix, for example `http://127.0.0.1:1242/MyPrefix` instead of `http://127.0.0.1:1242` alone. Using custom `PathBase` might be wanted in combination with specific setup of a reverse proxy where you'd like to proxy a specific URL only, for example `mydomain.com/ASF` instead of entire `mydomain.com` domain. Normally that would require from you to write a rewrite rule for your web server that would map `mydomain.com/ASF/Api/X` -> `127.0.0.1:1242/Api/X`, but instead you might define custom `PathBase` of `/ASF` and achieve easier setup of `mydomain.com/ASF/Api/X` -> `127.0.0.1:1242/ASF/Api/X`.

Unless you truly need to specify a custom base path, it's best to leave it at default. In addition to that, custom base path is **[not supported by our IPC GUI yet](https://github.com/JustArchiNET/ArchiSteamFarm/issues/869#issuecomment-419596294)**. Our IPC API is fully compatible with it already.