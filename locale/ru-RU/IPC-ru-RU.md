# IPC

Начиная с версии 3.0 в ASF добавлена система межпроцессного взаимодействия, основанная на http, которая может быть использована для управления процессом. Это альтернатива существующей системе управления через чат Steam.

Команды IPC всегда выполняются с правами `SteamOwnerID`, который по-умолчанию равен `0`. Чтобы ими пользоваться, вам нужно задать `SteamOwnerID` правильное ненулевое значение. Со значением по-умолчанию IPC будет работать, но ни у кого не будет прав отсылать команды (`400 BadRequest`). Больше о параметре `SteamOwnerID` вы можете прочитать в разделе "**[Конфигурация](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration-ru-RU)**".

* * *

## ЧаВО

### Что это вообще такое?

IPC это сокращение от "inter-process communication", межпроцессное взаимодействие, и имеет функционал очень похожий на выполнение **[команд](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands-ru-RU)** через чат Steam - это позволяет вам управлять процессом ASF во время работы. Однако IPC даёт гораздо больше чем просто отправка команд, поскольку он соединяет все основные возможности ASF в одном месте. Прямо сейчас IPC предлагает вам два "режима" использования - API и дружественный к пользователю графический интерфейс (GUI). API позволяет вам создавать свои утилиты и скрипты, взаимодействующие с ASF, а GUI позволяет вам пользоваться этими API вручную, через удобный интерфейс. Для банальной отправки команд вам будет легче взаимодействовать с ASF через чат Steam с одним из ботов. Однако вы можете пользоваться и IPC, если сочтете что это проще/полезнее для вас.

### Это безопасно?

ASF по умолчанию ожидает соединений только по адресу `127.0.0.1`, а значит доступ к IPC ASF невозможен с любой машины, кроме той где запущен ASF. Поэтому это настолько безопасно, насколько может быть безопасным IPC. Если вы решите изменить установленный по умолчанию адрес привязки `127.0.0.1` на что-то другое, например `*`, то вам следует установить соответствующие правила брандмауэра **самостоятельно**, чтобы разрешить только доверенным IP-адресам доступ к порту ASF. В добавок к этому, сервер должен иметь правильно настроенный ненулевой параметр `SteamOwnerID`, иначе от откажется выполнять любые команды в качестве дополнительной меры безопасности. И в добавок ко всему этому вы можете также установить в параметре `IPCPassword` пароль доступа к IPC, что добавит ещё один уровень дополнительной безопасности.

### Могу ли я использовать протокол HTTPS с шифрованием?

ASF предоставляет только базовый `HttpListener`, который не поддерживает использование протокола HTTPS и установку соответствующих сертификатов, и поддержка такого функционала в ASF только ещё больше усложнит его, и добавит сложностей с управлением сертификатами. Настоятельно рекомендуется использовать для этого **[Обратный прокси](https://ru.wikipedia.org/wiki/%D0%9E%D0%B1%D1%80%D0%B0%D1%82%D0%BD%D1%8B%D0%B9_%D0%BF%D1%80%D0%BE%D0%BA%D1%81%D0%B8)**, такой как **[nginx](https://nginx.org/en)**. Таким образом вы получите полный контроль над сервером http и настроить его как захотите, вместо того чтобы ограничиваться возможностями которые поддерживает `HttpListener` в ASF. Пример конфигурации nginx вы можете найти ниже. Мы включили полный блок `server`, хотя вам в основном будут интересны блоки `location`. Дальнейшую информацию вы можете найти в **[документации nginx](https://nginx.org/ru/docs/)**.

```nginx
server {
        listen *:443 ssl;
        server_name archi.justarchi.net;
        ssl_certificate /path/to/your/certificate.crt;
        ssl_certificate_key /path/to/your/certificate.key;

    location /Api/Log {
        proxy_pass http://127.0.0.1:1242;
#       proxy_set_header Host 127.0.0.1; # Только если надо переопределить настройки по-умолчанию
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Host $host:$server_port;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header X-Forwarded-Server $host;
        proxy_set_header X-Real-IP $remote_addr;

        # Мы добавляем эти 3 параметра для проксирования websockets см. https://nginx.org/en/docs/http/websocket.html
        proxy_http_version 1.1;
        proxy_set_header Connection "Upgrade";
        proxy_set_header Upgrade $http_upgrade;
    }

    location / {
        proxy_pass http://127.0.0.1:1242;
#       proxy_set_header Host 127.0.0.1; # Только если надо переопределить настройки по-умолчанию
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Host $host:$server_port;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header X-Forwarded-Server $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

Таким образом вы можете пользоваться полностью безопасным подключением к вашему ASF, как показано ниже.

![Изображение](https://i.imgur.com/ifoEmWs.png)

* * *

## ﻿Сервер

Чтобы запустить IPC вам нужно включить **[параметр глобальной конфигурации](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration-ru-RU#Файл-глобальной-конфигурации)** `IPC`. Дополнительную информацию вы можете найти в разделе "**[Конфигурация](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration-ru-RU)**". Возможно вы также захтите использовать **[аргумент командной строки](https://github.com/JustArchi/ArchiSteamFarm/wiki/Command-line-arguments-ru-RU)** `--process-required`, хотя это совершенно не обязательно, мы просто решили об этом упомянуть.

Если конфигурация сделана правильно, вы должны заметить что служба IPC активна:

    INFO|ASF|StartServer() Запуск IPC сервера на http://127.0.0.1:1242/...
    INFO|ASF|StartServer() IPC сервер готов!
    

ASF теперь ожидает входящих IPC-соединений по адресу `http://127.0.0.1:1242/` (или по тому адресу, который вы указали в параметре `IPCPrefixes`).

* * *

## Клиент

Для взаимодействия с сервером IPC, предоставляемым ASF, может быть использована любая программа совместимая с http, включая классические веб-браузеры, а также утилиты командной строки, такие как `curl`.

### API

IPC ASF предоставляет полное API для управления ботами, для использования которого необходимо отправлять соответствующие запросы на несколько имеющихся конечных точек. Вы можете использовать эти конечные точки API для создания своих вспомогательных скриптов, утилит, интерфейсов и тому подобного. Отсылка запросов API официально полностью поддерживается командой ASF.

### IPC GUI

В дополнение к запросам API, мы медленно разрабатывает графический интерфейс IPC GUI, который представляет из себя дружественный к пользователю фронтенд для API предоставляемых ASF. Доступ к этому GUI вы можете получить перейдя на главную страницу интерфейся IPC ASF, например `http://127.0.0.1:1242`.

![Изображение](https://user-images.githubusercontent.com/1069029/35774997-e3bceb38-097f-11e8-9e07-bb2d60a04618.png)

IPC GUI на данном этапе находится **на предварительном рассмотрении**, это значт что официально он **не поддерживается** и мы не принимаем сообщения об ошибках (отдельных проблемах) касающихся его. Вы можете делиться своими мыслями, предложениями и сообщениями об ошибках в соответствующих **[issue](https://github.com/JustArchi/ArchiSteamFarm/issues?q=is%3Aissue+is%3Aopen+label%3AIPC)**, посвященных разработке IPC GUI. Ведущий разработчик IPC GUI **[@MrBurrBurr](https://github.com/mrburrburr)**.

* * *

## Коды состояния HTTP

Наше API использует стандартные коды состояния HTTP, и они используются согласно RFC. На данный момент ASF может вернуть следующие коды состояния:

- `200 OK` - запрос завершен успешно.
- `400 BadRequest` - запрос не удался из-за ошибки, проанализируйте тело ответа для получения реальной причины. В большинстве случаев это означает что ASF понял запрос, но отказался его выполнить по той или иной причине (и в теле ответа объясняется, почему).
- `401 Unauthorized` - в ASF установлен `IPCPassword` но вы не смогли успешно пройти **[аутентификацию](#Аутентификация)**.
- `403 Forbidden` - Вы не смогли пройти **[аутентификацию](#Аутентификация)** слишком много раз, доступ к IPC запрещён, попробуйте снова через час.
- `404 NotFound` - URL к которому вы пытаетесь получить доступ, не существует.
- `405 MethodNotAllowed` - Метод HTTP, который вы пытаетесь использовать, недопустим для этой конечной точки API. Эта же ошибка используется если вы пытаетесь получить доступ к конечной точке websocket не инициализировав соединени websocket (upgrade).
- `406 NotAcceptable` - заголовок `Content-Type` вашего запроса недопустим для этой конечной точки API. В основном используется для запросов, в которых от вас требуется передать особое тело запроса, без указания его типа.
- `411 LengthRequired` - ваш запрос `POST` не имеет заголовка `Content-Length`.
- `500 InternalServerError` - сервер IPC попал в критическое состояние, это указывает на проблему в ASF, о которой следует сообщить чтобы её исправили. В нормальной ситуации этот статус нигде не используется. Для ожидаемых ошибок вместо этого используется статус `503`.
- `501 NotImplemented` - этот URL зарезервирован для будущего использования и на данный момент не реализован.
- `503 ServiceUnavailable` - в ASF произошла одна из возможных ошибок в процессе выполнения, и запрос выполнить не удалось. Проверьте журнал ASF чтобы узнать реальную причину.

* * *

## Аутентификация

Интерфейс IPC ASF по умолчанию не нуждается ни в какой аутентификации, поскольку `IPCPassword` установлено значение `null`. Однако, если вы активировали `IPCPassword`, указав ему не пустое значение, любой запрос к интерфейсу IPC ASF требует пароль, совпадающий с установленным в `IPCPassword`. Если вы пропустили аутентификацию или указали неверный пароль, вы получите ошибку `401 - Unauthorized`. Если вы продолжите отправлять запросы без аутентификации, со временем сработает ограничение частоты запросов и вы получите ошибку `403 - Forbidden`.

Аутентификация может быть выполнена двумя общепринятыми способами.

### Заголовок `Authentication`

В основном вам следует использовать заголовки запроса HTTP, установив значение поля `Authentication` равным вашему паролю. Как этого добиться зависит от того, каким инструментом вы пользуетесь для доступа к интерфейсу IPC ASF, например если вы пользуетесь `curl` вам следует добавить в качестве аргумента `-H 'Authentication: MyPassword'`. Таким способом аутентификация передаётся в заголовках запроса, где и должна быть.

### Параметр `password` в строке запроса

В качестве альтернативы вы можете добавить параметр `password` в конец URL, который хотите вызвать, например вызывая `/Api/Command/version?password=MyPassword` вместо `/Api/Command/version`. Этот подход достаточно хорош для большинства случаев, поскольку он дружественный к пользователю, и может быть сохранён в качестве закладки, но очевидно что он в открытом виде показывает ваш пароль, что не всегда допустимо. В добавок это дополнительный параметр в строке запроса, что загромождает вид URL, и создаёт впечатление что он применим только к этому URL, хотя на самом деле он применяется ко всему обмену с IPC.

* * *

Оба способа поддерживаются совершенно одинаково, и это вам решать каким из них пользоваться. Мы всё же рекомендуем использовать заголовки HTTP везде где только можно, поскольку с точки зрения использования это более правильно чем строка запроса - строка запроса в этом случае остаётся только для пользователя, как дружественный к пользователю способ создавать закладки с URL запросов IPC.

* * *

## Cross-Origin Resource Sharing

ASF по умолчанию имеет заголовок `Access-Control-Allow-Origin` равный `*`. Это позволяет, например, скриптам на JavaScript получать доступ к интерфейсу IPC ASF в сторонних GUI и утилитах. Однако, это также означает что кто-то может загрузить потенциально вредоносный скрипт который будет делать запрсы к AFS без вашего ведома и разрешения. Если вы хотеите удостоверится, что такая ситуация не случиться, рекомендуется соответсвенно настроить `IPCPassword`. Таким образом любой скрипт, которых захочет получить доступ к интерфейсу IPC ASF, ему потребуется сначала пройти аутентификацию для каждого запроса, как описано выше.

* * *

## API

В основном наше API является типичным REST API основанном на JSON в качестве основного метода сериализации/десериализации данных. Мы стараемся максимально точно описать ответ, используя как коды ошибок HTTP (когда это применимо), так и ответ JSON который вы можете самостоятельно разобрать чтобы выяснить, окончился ли запрос успешно, и если нет - то почему.

Некоторые конечные точки API могут требовать формирования дополнительных данных, как например соответствующей JSON-структуры в качестве тела запроса, и соответственно установки заголовка `Content-Type` равным `application/json`. Если конечная точка API требует особых входных данных, они будут перечислены вверху описания конечной точки.

Предоставленные ниже примеры запросов/ответов показывают возможное использование с помощью утилиты **[curl](https://curl.haxx.se)** - ожидается что вы измените параметр URL добавив в начало соответствующие `Protocol://Host:Port`, согласно настройкам вашего ASF, например `http://127.0.0.1:1242`.

Цифровые параметры определены с максимальными значениями, поэтому вы можете использовать для них строгую типизацию, как например `uint` для `AppID`, и `ulong` для `SteamID`. Некоторые поля типа `ulong`, сериализованные как числа, могут иметь дополнительные поля с префиксом `s_`, сериализованные как строки, что может быть использовано в JavaScript, который не способен точно представлять 64-разрядные числа (также верно для других языков с подобным ограничением).

* * *

### GenericResponse

Стандартный формат ответа (GenericResponse) это основной возвращаемый тип для всех вызовов API.

```json
{
    "Message": "string",
    "Result": {},
    "Success": true
}
```

`Message` - значение типа `string`, содержащее дополнительные подробности ответа. Это может быть просто "OK" если запрос успешен, или реальная причина отказа если нет. Мы используем это поле как способ дать вам знать что случилось с отправленным вами запросом. Помните, что это поле НЕ результат вашего запроса, а только описание того, что произошло. Может быть равным null если у нас нет определённого сообщения для вас, хотя мы стараемся по возможности избегать таких ситуаций.

`Result` - значение типа `object` содержащее результат вашего запроса. Тип этого поля зависит от вызванной конечной точки API- например это может быть `Bot` или `string`. Чаще всего используется в запросах `GET` для получения данных которые вы запросили. Хотя это поле имеет гибкий тип, конкретные конечные точки всегда гарантируют определённое количество возможных результатов, и очень часто можно использовать строгую типизацию для отдельно взятой конечной точки. Может быть равным null если у нас нет определённого результата для вас.

`Success` - значение типа `bool`, предоставляющее простой способ проверки результата. Это дополнение к кодам состояния HTTP, поскольку коды `2xx` считаются `true`, а всё остальное считается `false`. Обратите внимание, что это поле только указывается что **запрос API успешен** - вам необходимо разобрать поле `Result` чтобы проверить, успешно ли заданное действие, такое как отправка команды.

* * *

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
    "GlobalConfig": {
        "AutoRestart": false,
        "Blacklist": [ 440 ]
    },
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

`BuildVariant` - значение типа `string`, содержащее вариант сборки запущенного процесса ASF. Это может быть любой вариант доступный в разделе "**[releases](https://github.com/JustArchi/ArchiSteamFarm/releases)**", а также варианты не доступные для скачивания официально (такие как вариант `docker` для нашего образа Docker или вариант `source` для неофициальных сборок).

`GlobalConfig` - специальный объект C# используемый для доступа к конфигурации. Имеет точно такую же структуру как **[файл глобальной конфигурации](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration-ru-RU#Файл-глобальной-конфигурации)**, описанный в разделе "Конфигурация". Это свойство может быть использовано чтобы узнать с какими параметрами настроен работать процесс ASF. Обычно этот объект будет включать только часть параметров конфигурации - те, которые пользователь изменил (минимальная конфигурация). Конфиденциальная информация, указанная в конфигурации процесса (например, `WebProxyPassword`), всегда исключается из выдачи.

`MemoryUsage` - значение типа `uint`, содержащее объём памяти **управляемого** кода, занятый процессом ASF в целом, в килобайтах.

`ProcessStartTime` - значение типа `DateTime`, содержащее точное время запуска процесса ASF. Может использоваться, например, для вычисления общего времени работы программы. Для JSON ASF сериализует объект `DateTime` в строку стандарта **[ISO 8601](https://ru.wikipedia.org/wiki/ISO_8601)** содержащую дату, время и используемый часовой пояс.

`Version` - значение типа `Version`, содержащее версию запущенного двоичного файла ASF. Тип `Version` содержит 4 важных свойства `Major`, `Minor`, `Build` и `Revision`, которые соответствуют цифрам в записи `A.B.C.D` версии ASF.

* * *

### `POST /Api/ASF`

#### Тело:

Content-Type: application/json

```json
{
    "GlobalConfig": {},
    "KeepSensitiveDetails": true
}
```

Эта конечная точка API может использоваться для обновления **[файла глобальной конфигурации](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration-ru-RU#Файл-глобальной-конфигурации)** ASF. Другими словами, это обновит файл `ASF.json` в папке `config` содержимым **[GlobalConfig](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration-ru-RU#Файл-глобальной-конфигурации)** объекта JSON переданного в теле запроса. Возвращает **[GenericResponse](#genericresponse)** с полем `Result` заданным как `null`.

`GlobalConfig` - JSON-объект со структурой аналогичной **[файлу глобальной конфигурации](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration-ru-RU#Файл-глобальной-конфигурации)**. Это поле является обязательным и не может быть равным `null`. Параметры конфигурации со значением, равным значению по-умолчанию, могут быть опущены, как и в обычной конфигурации ASF.

`KeepSensitiveDetails` - это значение типа `bool`, указывающее должна ли конфиденциальная информация, такая как `WebProxyPassword`, быть скопирована из существующей конфигурации (если есть). Это поле необязательное, и по умолчанию считается равным `true`. Когда это поле включено, параметры с конфиденциальной информацией, указанные со значением `null`, будут скопированы из текущей конфигурации.

На данный момент следующие параметры считаются конфиденциальной ифнормацией и и могут быть заданы равными `null` для копирования из существующей конфигурации: `WebProxyPassword`.

```shell
curl -X POST -H "Content-Type: application/json" -d '{"GlobalConfig":{"AutoRestart":false,"BackgroundGCPeriod":0}}' /Api/ASF
{"Message":"OK","Result":null,"Success":true}
```

* * *

### `DELETE /Api/Bot/{BotNames}`

Эта конечная точка API может использоваться для полного удаления заданных ботов с указанными именами `BotNames`, вместе со всеми файлами. Иными словами, это удалит файлы `BotName.*` (включая, но не ограничиваясь `json`, `db`, `bin`, `maFile`, `keys` и т.д.) из вашей папки `config` для всех заданных ботов. Эта конечная точка принимает несколько имен ботов в `BotNames`, разделённых запятыми, а также ключевое слово `ASF` для удаления всех имеющихся ботов. Возвращает **[GenericResponse](#genericresponse)** с полем `Result` заданным как `null`.

```shell
curl -X DELETE /Api/Bot/archi
{"Message":"OK","Result":null,"Success":true}
```

* * *

### `GET /Api/Bot/{BotNames}`

This API endpoint can be used for fetching status of given bots specified by their `BotNames` - it returns basic statuses of the bots. This endpoint accepts multiple `BotNames` separated by a comma, as well as `ASF` keyword for returning all defined bots. Returns **[GenericResponse](#genericresponse)** with `Result` defined as `ImmutableHashSet<Bot>` - collection of bot statuses.

```shell
curl -X GET /Api/Bot/archi
{"Message":"OK","Result":[{"BotName":"archi","CardsFarmer":{"CurrentGamesFarming":[],"GamesToFarm":[],"TimeRemaining":"00:00:00","Paused":false},"AccountFlags":0,"AvatarHash":"99bf6df8ad1836c0205de22935f6fe4b1f96b0c6","IsPlayingPossible":true,"SteamID":0,"BotConfig":null,"KeepRunning":false}],"Success":true}
```

#### Бот

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
    "BotConfig": {
        "Enabled": true,
        "Paused": true
    },
    "KeepRunning": true
}
```

`BotName` is `string` type that defines name of the bot. This is the same identifier that is used for commands and all other identification-related bot activity.

`CardsFarmer` is specialized C# object used by Bot for cards-farming purpose. It provides information related to cards farming progress of given bot instance. Its structure is explained **[below](#cardsfarmer)**.

`AccountFlags` is `EAccountFlags` (`uint` flags) type, defined by SK2 **[here](https://github.com/SteamRE/SteamKit/blob/master/Resources/SteamLanguage/enums.steamd#L81-L116)**, that specifies Steam account flags of given account. This property can be used for getting more information about the status of Steam account being in ASF, for example if it's **[limited](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663)**, by checking if `LimitedUser` or `LimitedUserForce` flags are set. This property is initialized (and updated) the moment Bot logs in to Steam network, therefore it'll have a value of `0` before first login.

`AvatarHash` is `string` type that contains Steam avatar hash being used by given bot. It's possible to use this value for building URL pointing to user's avatar on Steam CDN. Can be `null` if user didn't set his avatar.

`IsPlayingPossible` is `bool` type that specifies if account being used by a bot can be used for automatic idling. This property will be `false` when Steam library of the account is being used elsewhere, either normally, or via family sharing. This property affects only remote logins and does not cover its own process (so if account is not being used anywhere else, `IsPlayingPossible` will always be `true`, even if ASF is actively idling games on it right now).

`SteamID` is `ulong` unique steamID identificator of currently logged in account in 64-bit form. This property will have a value of `0` if bot is not logged in to Steam Network (therefore it can be used for telling if account is logged in or not).

`BotConfig` is specialized C# object used by Bot for accessing to its config. It has exactly the same structure as **[bot config](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#bot-config)** explained in configuration. This property can be used for finding out options that the bot is configured to work with. Обычно этот объект будет включать только часть параметров конфигурации - те, которые пользователь изменил (минимальная конфигурация). Sensitive account-related information such as `SteamLogin`, `SteamPassword` and `SteamParentalPIN` are always skipped from being included.

`KeepRunning` is a `bool` type that specifies if bot is active. Active bot is a bot that has been started, either by ASF on startup, or by user later during execution. If bot is stopped, this property will be `false`. Keep in mind that this property has nothing to do with bot being connected to Steam network, or not (that is what `SteamID` can be used for).

#### CardsFarmer

`GamesToFarm` is an `ImmutableHashSet<Game>` (collection of `Game` elements) object that contains games pending to farm in current farming session. Please note that collection is updated on as-needed basis regarding performance. For example, when idling with `Simple` cards farming algorithm, ASF won't bother checking if we got any new games to farm when new game gets added (as we'd do that check anyway when we're out of queue, and by not doing so immediately we save requests and bandwidth). Therefore, this is data regarding current farming session, that might be different from overall data.

`CurrentGamesFarming` is an `ImmutableHashSet<Game>` (collection of `Game` elements) object that contains games being farmed right now. In comparison with `GamesToFarm`, this property defines current status instead of pending queue, and it's heavily affected by currently selected cards farming algorithm. This collection can contain only up to `32` games (`MaxGamesPlayedConcurrently` enforced by Steam Network). You also have a guarantee that only entries already existing in `GamesToFarm` can be included here.

`TimeRemaining` is a `TimeSpan` type that specifies approximated time required to farm all games specified in `GamesToFarm` collection. This is nowhere close to the actual time that will be required, but it's a nice indicator with accuracy that might be improved in future, therefore it can be used for various display purposes. It's not updated in real-time, but calculated from current `GamesToFarm` status, therefore it's re-calculated the moment `CardsRemaining` of any game changes.

`Paused` is a `bool` type that specifies if `CardsFarmer` is currently paused. CardsFarmer can be paused due to various events, mainly `pause` and `play` commands. Paused CardsFarmer will not attempt to farm anything in automatic mode, neither will check badges every `IdleFarmingPeriod` hours.

#### Игра

`AppID` is `uint` type that in unique way identifies game being played. ASF enforces this to be greater than `0`.

`GameName` is `string` type that provides friendly name of game identified by `AppID`. This is data returned by Steam Community. ASF enforces this to be `non-null` and `non-empty`.

`HoursPlayed` is `float` type that provides information how many hours the game has been played. This property is not updated in real time, but on as-needed basis, at least once per `FarmingDelay` minutes. Please note that initially this data is retrieved from Steam Community, but then updated according to ASF built-in timers, therefore it might not match what Steam Community is returning - this is because Steam Community data is not provided in real time either, and ASF requires such data for stopping farming for hours game as soon as it reaches `HoursUntilCardDrops` value. ASF enforces this property to be at least `0.0`.

`CardsRemaining` is `ushort` type that tells how many cards are remaining for the game. This property is updated as soon as possible and it should always have a value greater than `0`. However, it is possible for this property to have `0` value for a short moment when ASF is switching game.

* * *

### `POST /Api/Bot/{BotName}`

#### Тело:

Content-Type: application/json

```json
{
    "BotConfig": {},
    "KeepSensitiveDetails": true
}
```

This API endpoint can be used for creating/updating **[BotConfig](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#bot-config)** of given bot specified by its `BotName`. In other words, this will update `BotName.json` of `config` directory with **[BotConfig](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#bot-config)** JSON object supplied in request body. Возвращает **[GenericResponse](#genericresponse)** с полем `Result` заданным как `null`.

`BotConfig` is **[BotConfig](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#bot-config)** JSON object. Это поле является обязательным и не может быть равным `null`. Specifying config properties with their default values might be omitted, just like in regular (minimalistic) ASF config.

`KeepSensitiveDetails` is `bool` type that specifies whether sensitive details such as `SteamLogin` or `SteamPassword` should be inherited from existing config (if available). Это поле необязательное, и по умолчанию считается равным `true`. Когда это поле включено, параметры с конфиденциальной информацией, указанные со значением `null`, будут скопированы из текущей конфигурации.

Currently, following properties are considered sensitive and can be set to `null` in order to be inherited: `SteamLogin`, `SteamPassword`, `SteamParentalPIN`.

```shell
curl -X POST -H "Content-Type: application/json" -d '{"BotConfig":{"Enabled":false,"Paused":true}}' /Api/Bot/archi
{"Message":"OK","Result":null,"Success":true}
```

* * *

### `POST /Api/Command/{Command}`

#### Body: empty

This API endpoint can be used executing given command specified by its `{Command}`. It's recommended to always specify the bot that is supposed to execute the command, otherwise the first defined bot will be used instead. Returns **[GenericResponse](#genericresponse)** with `Result` defined as `string` - the output of the executed command.

```shell
curl -X POST -d '' /Api/Command/version
{"Message":"OK","Result":"\r\n<archi> ASF V3.0.5.3","Success":true}
```

* * *

### `DELETE /Api/GamesToRedeemInBackground/{BotName}`

This API endpoint can be used for completely erasing `.keys.used` and `.keys.unused` files of given bots specified by their `BotNames` in `config` directory. This endpoint accepts multiple `BotNames` separated by a comma, as well as `ASF` keyword for deleting those files of all defined bots. Возвращает **[GenericResponse](#genericresponse)** с полем `Result` заданным как `null`.

```shell
curl -X DELETE /Api/GamesToRedeemInBackground/archi
{"Message":"OK","Result":null,"Success":true}
```

* * *

### `GET /Api/GamesToRedeemInBackground/{BotName}`

This API endpoint can be used for fetching `.keys.used` and `.keys.unused` files of given bots specified by their `BotNames` in `config` directory. Returns **[GenericResponse](#genericresponse)** with `Result` defined as `ImmutableDictionary<string, GamesToRedeemInBackgroundResponse>` - a map that maps `BotName` to a **[GamesToRedeemInBackgroundResponse](#gamestoredeeminbackgroundresponse)** (explained below).

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

Both `UnusedKeys` and `UsedKeys` are `ImmutableDictionary<string, string>` objects that map redeemed cd-keys (`key`) with their names (`value`). This is the result of a `POST` call described below and can be used for remotely fetching keys without accessing ASF config directory. Both objects can be `null` in case of ASF error during fetching files (such as I/O), but empty or missing files will behave properly and produce empty dictionary.

* * *

### `POST /Api/GamesToRedeemInBackground/{BotName}`

#### Тело:

Content-Type: application/json

```json
{
    "GamesToRedeemInBackground": {
        "string": "string",
        "string": "string"
    }
}
```

This API endpoint can be used for adding extra **[games to redeem in background](https://github.com/JustArchi/ArchiSteamFarm/wiki/Background-games-redeemer)** to given bot specified by its `BotName`. Returns **[GenericResponse](#genericresponse)** with `Result` defined as `OrderedDictionary<string, string>`.

`GamesToRedeemInBackground` is `OrderedDictionary<string, string>` JSON object that maps cd-keys to redeem (`key`) with their names (`value`). Это поле является обязательным и не может быть равным `null`. Neither any `key` nor `value` in the dictionary can be `null` or empty. In addition to that, every `key` must have valid Steam cd-key structure, ASF will validate that using built-in regex. Invalid entries will be automatically removed during import process.

The `Result` is `GamesToRedeemInBackground` that were successfully added to the collection of games pending to redeem. Like specified above, invalid entries will be automatically removed during import process, so you can use this result for a comparison with your original request in order to check which entries were deemed as invalid and skipped during import process. If you didn't supply any invalid data, this result will be exactly the same as `GamesToRedeemInBackground` that you've just sent to ASF.

```shell
curl -X POST -H "Content-Type: application/json" -d '{"GamesToRedeemInBackground":{"AAAAA-BBBBB-CCCCC":"Orwell","XXXXX-YYYYY-ZZZZZ":"Factorio"}}' /Api/GamesToRedeemInBackground/archi
{"Message":"OK","Result":{"AAAAA-BBBBB-CCCCC":"Orwell","XXXXX-YYYYY-ZZZZZ":"Factorio"},"Success":true}
```

* * *

### `GET /Api/Log`

This API endpoint can be used for fetching real-time log messages being written by ASF. In comparison with other endpoints, this one uses **[websocket](https://en.wikipedia.org/wiki/WebSocket)** connection for providing real-time updates. Each message is encoded in **[UTF-8](https://en.wikipedia.org/wiki/UTF-8)** and has a **[GenericResponse](#genericresponse)** structure with `Result` defined as `string` - the message rendered in configured by user NLog-specific layout. On initial connection, ASF will also push a burst of last few logged messages as a short history (by default last 20, but user is free to change this number, as well as disabling history entirely).

The websocket connection established with this endpoint is **read-only** - ASF will accept only **[control frames](https://tools.ietf.org/html/rfc6455#section-5.5)**, especially `Close` frame indicating that websocket connection should be gracefully closed. Sending any data frame will result in connection being terminated.

```shell
curl -X GET -i -N -H "Connection: Upgrade" -H "Upgrade: websocket" /Api/Log
HTTP/1.1 200 OK

# Example of messages being sent by ASF, keep in mind that result string is affected by user-specified NLog logging layout
{"Message":"OK","Result":"2018-01-31 03:19:34|dotnet-2884|INFO|ASF|Start() IPC server ready!","Success":true}
```

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

`Body` - `ImmutableDictionary<string, string>` value that specifies properties that are possible to set for given type. This includes all public and non-public (but not private) fields and properties of object of given type. `Key` of the collection is defined as name of given field/property, while `Value` of that key is defined as C# type that is valid for it. This property can be empty if given type doesn't include any fields or properties. We also use this property for further decomposition of given type, for example `BaseType` of `System.Enum` will have valid enum values declared here, where `Key` will be name of given enum value, and `Value` will be actual value for that name.

`Properties` - `TypeProperties` type defined **[below](#typeproperties)** that holds metadata information about given type.

#### TypeProperties

`BaseType` - `string` value that specifies base type for this type. For example, it'll be `System.Object` for `ArchiSteamFarm.BotConfig` object, and `System.Enum` for `ArchiSteamFarm.BotConfig+ETradingPreferences`. Based on this property you can partially strong-type `Body` content by knowing in advance how you should parse it (for example for `System.Enum`, `Body` will include enum names and values, as specified above in `Body` description).

`CustomAttributes` - `ImmutableHashSet<string>` value that specifies what custom attributes apply to this type. This property is especially useful when `BaseType` is `System.Enum`, as in this case you can check if it's special `flags` enum by verifying that `System.FlagsAttribute` is defined in this collection. This value can be null when there are no custom attributes defined for this object. Together with `UnderlyingType`, this tells you that `ArchiSteamFarm.BotConfig+ETradingPreferences` is `byte flags` enum.

`UnderlyingType` - `string` value that specifies underlying type for this type. This is used mainly with `System.Enum` to know what underlying type this enum uses for data storage. For example in most ASF enums, this will be `System.Byte`. Together with `CustomAttributes`, this tells you that `ArchiSteamFarm.BotConfig+ETradingPreferences` is `byte flags` enum.

* * *

## WWW API

APIs below are dedicated for our IPC GUI usage and they should not be implemented by remote scripts or tools. This documentation is for our internal reference only and can change anytime, in any possible way. You should not rely on existance of below endpoints, neither implement them in your own tools.

* * *

### `GET /Api/WWW/Directory/{Directory}`

This API endpoint can be used for fetching directory's content specified by its local path relative to `www` directory. Returns **[GenericResponse](#genericresponse)** with `Result` defined as `ImmutableHashSet<string>` - collection of local filenames.

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