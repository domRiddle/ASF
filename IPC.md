# IPC

Starting with version 3.0, ASF offers http-based inter-process communication that can be used to communicate with the process. This is offered as an alternative to already existing steam chat communication.

IPC is always executed with `SteamOwnerID` permissions, which is `0` by default. In order to use it, you should set `SteamOwnerID` to the proper non-zero value. Default value will make IPC work, but not authorizing anyone to send commands (`400 BadRequest`). For more info about `SteamOwnerID`, visit **[configuration](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration)**.

---

## FAQ

### What is this all about?

IPC stands for inter-process communication and has a very similar functionality to issuing **[commands](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)** through Steam chat - it allows you to control ASF process during execution. However, IPC offers much more than just issuing commands, as it integrates all major ASF features in one place. Right now IPC offers two "modes" for you to use - the API, and user-friendly GUI. API allows you to code your own tools and scripts that communicate with ASF, while GUI allows you to consume those APIs in user-friendly way. For casual commands execution it should be easier for you to communicate with ASF through steam chat with one of the bots. However, you can use IPC too, if you consider it useful/easier for you.

### Is this secure?

ASF by default listens only on `127.0.0.1` address, which means that accessing ASF IPC from any other machine but your own is impossible. Therefore, it's as secure as IPC can be. If you decide to change default `127.0.0.1` bind address to something else, such as `*`, then you're supposed to set proper firewall rules **yourself** in order to allow only authorized IPs to access ASF port. In addition to that, server must include properly set non-zero `SteamOwnerID`, otherwise it'll refuse to execute any command, as an extra security measure. On top of all of that, you can also set `IPCPassword`, which would add another layer of extra security.

### Can I use HTTPS protocol with proper encryption?

ASF deploys only very minimalistic `HttpListener`, which itself does support using HTTPS protocol and setting appropriate certificates, but supporting that feature in ASF would make it far more complex than it already is, and would still be problematic for certificates management. It's strongly suggested to use **[reverse proxy](https://en.wikipedia.org/wiki/Reverse_proxy)** for that, such as **[nginx](https://nginx.org/en)**. This way you can have full control over your http server and you can set it up however you wish instead of being limited to given set of features ASF's `HttpListener` decided to support. Example nginx configuration can be found below. We included full `server` block, although you're interested mainly in `location` one.

```nginx
server {
        listen *:443 ssl;
        server_name archi.justarchi.net;
        ssl_certificate /path/to/your/certificate.crt;
        ssl_certificate_key /path/to/your/certificate.key;

	upstream asf {
		server 127.0.0.1:1242;
	}

	location /Api/Log {
		proxy_pass http://asf;
		proxy_set_header Connection "upgrade";
		proxy_set_header Upgrade $http_upgrade;
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		proxy_set_header X-Forwarded-Proto $scheme;
		proxy_set_header X-Real-IP $remote_addr;
	}

	location / {
		proxy_pass http://asf;
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		proxy_set_header X-Forwarded-Proto $scheme;
		proxy_set_header X-Real-IP $remote_addr;
	}
}
```

This way you can use fully secured connection to your ASF instance, as shown below.

![Image](https://i.imgur.com/ifoEmWs.png)

---

## Server

To start IPC, you must enable `IPC` **[global configuration property](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#global-config)**. Refer to **[configuration](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration)** for more info. You might also want to make use of `--process-required` **[command-line argument](https://github.com/JustArchi/ArchiSteamFarm/wiki/Command-line-arguments)**, although that is entirely optional, just a mere mention.

If configuration was set correctly, you should notice that IPC service is active:

```
INFO|ASF|StartServer() Starting IPC server on http://127.0.0.1:1242/...
INFO|ASF|StartServer() IPC server ready!
```

ASF is now listening on `http://127.0.0.1:1242/` for incoming IPC connections (or whatever `IPCPrefixes` you specified in the config).

---

## Client

Communication with IPC server provided by ASF can be done via any http-compatible program, including classical web browsers, as well as CLI utilities such as `curl`.

### API

ASF IPC offers a full API for bots management that can be accessed by sending appropriate requests to appropriate endpoints. You can use those API endpoints to make your own helper scripts, tools, GUIs and alike. Sending API calls is officially and fully supported by ASF team.

### IPC GUI

In addition to API calls, we're slowly coding IPC GUI that is a nice user-friendly frontend to API features that ASF offers. This GUI can be accessed by navigating to main index of the ASF's IPC interface, such as `http://127.0.0.1:1242`.

![Image](https://user-images.githubusercontent.com/1069029/35774997-e3bceb38-097f-11e8-9e07-bb2d60a04618.png)

IPC GUI is currently in **preview** state, which means that officially it's **unsupported** and we're also not accepting bug reports (standalone issues) for it. You're free to post your thoughts, suggestions and bug reports in our **[#773](https://github.com/JustArchi/ArchiSteamFarm/issues/773)** issue that is fully dedicated to IPC GUI. Lead developer of IPC GUI is **[@MrBurrBurr](https://github.com/mrburrburr)**.

---

## HTTP status codes

Our API makes use of standard HTTP status codes, and they're used according to the RFC. Currently ASF can return following status codes:

- `200 OK` - the request completed successfully.
- `400 BadRequest` - the request failed because of an error, parse response body for actual reason. Most of the time this is ASF, understanding the request, but refusing to fulfill it for one reason or another (hence response body telling you why).
- `401 Unauthorized` - ASF has `IPCPassword` set, but you failed to **[authenticate](#authentication)** properly.
- `403 Forbidden` - You failed to **[authenticate](#authentication)** properly too many times, IPC access is forbidden, try again in an hour.
- `404 NotFound` - the URL you're trying to reach does not exist.
- `405 MethodNotAllowed` - the HTTP method you're trying to use is not allowed for this API endpoint. This error is also used when trying to access websocket endpoint without initiating a websocket connection (upgrade).
- `406 NotAcceptable` - your `Content-Type` header is not acceptable for this API endpoint. Mainly used in requests that require from you a specific body, without you explicitly stating type of it.
- `411 LengthRequired` - your `POST` request is missing `Content-Length` header.
- `500 InternalServerError` - IPC server ran into fatal condition, this indicates ASF issue that should be reported and corrected. We do not normally use this status anywhere in the code. For expected errors we use `503` instead.
- `501 NotImplemented` - this URL is reserved for future use and not implemented yet.
- `503 ServiceUnavailable` - ASF ran into one of possible exceptions during execution of this request, and can't fulfill it. Check ASF log for actual reason.

---

## Authentication

ASF IPC interface by default does not require any sort of authentication, as `IPCPassword` is set to `null`. However, if `IPCPassword` is enabled by being set to any non-empty value, every call to ASF IPC interface requires the password that matches set `IPCPassword`. If you omit authentication or input wrong password, you'll get `401 - Unauthorized` error. If you continue sending requests without authentication, eventually you'll get rate-limited with `403 - Forbidden` error.

Authentication can be done through two generally-acceptable ways.

### `Authentication` header

In general you should use HTTP request headers, by setting `Authentication` field with your password as a value. The way of doing that depends on the actual tool you're using for accessing ASF's IPC interface, for example if you're using `curl` then you should add `-H 'Authentication: MyPassword'` as a parameter. This way authentication is passed in the headers of the request, where it in fact should take place.

### `password` parameter in query string

Alternatively you can append `password` parameter to the end of the URL you're about to call, for example by calling `/Api/Command/version?password=MyPassword` instead of `/Api/Command/version` alone. This approach is good enough for majority of use cases, as it's user-friendly and can be even saved as a bookmark, but obviously it exposes password in the open, which is not necessarily always appropriate. In addition to that it's extra argument in the query string, which complicates the look of the URL and makes it feel like it's URL-specific, while it applies to the entire IPC communication.

---

Both ways are supported in exactly the same way and it's totally up to you which one you want to choose. We still recommend to use HTTP headers everywhere where you can, as usage-wise it's more appropriate than query string - query string is mainly left only for users as user-friendly way of bookmarking IPC URLs.

---

## Cross-Origin Resource Sharing

ASF by default has `Access-Control-Allow-Origin` header set to `*`. This allows e.g. JavaScript scripts  to access ASF IPC interface in third-party web GUIs or tools. However, this also means that somebody could potentially upload malicious script that would make calls to ASF without your awareness or approval. If you'd like to ensure that such situation won't happen, consider setting up `IPCPassword` appropriately. This way if any script wants to access ASF's IPC interface, it'll need to authenticate each request firstly, as described above.

---

## API

In general our API is a typical REST API that is based on JSON as a primary way of serializing/deserializing data. We're doing our best to precisely describe response, using both HTTP error codes (where appropriate), as well as JSON response you can parse yourself in order to know whether the request suceeded, and if not, then why.

Some API endpoints might require from you to specify extra data, such as providing appropriate JSON structure as a body of the request, together with setting `Content-Type` header to `application/json`. If API endpoint has some special requirements for an input, it'll be listed on the top of the endpoint description.

Provided examples of requests/responses below show possible usage with **[curl](https://curl.haxx.se)** tool - you're expected to modify URL parameter by prepending appropriate `Protocol://Host:Port` to the URL, according to your ASF usage, such as `http://127.0.0.1:1242`.

Numeric properties are defined with their maximum values, so you can also use strong-typing for them, such as `uint` for `AppID`, and `ulong` for `SteamID`. Selected `ulong` fields that are serialized as numbers might include extra `s_` fields serialized as strings that can be consumed by JavaScript which can't represent 64-bit numbers precisely (and other languages with similar limitations).

---

### GenericResponse

Generic response is a primary return type that we use for all API calls.

```json
{
	"Message": "string",
	"Result": {},
	"Success": true
}
```

`Message` - `string` value providing extra details about the response. This could be simple "OK" when request succeeded, or actual failure reason if it didn't. We use this field as a general help for you to know what happened about the request you've sent. Keep in mind that this field is NOT a result of your request, only a description of what happened. Can be null if we don't have any specific message for you to retrieve, although we try to avoid that as much as possible.

`Result` - `object` value providing actual result of your request. The type of this field depends on API endpoint that you called - for example it can be a `Bot` or a `string`. Most commonly used in `GET` requests for fetching actual data that you asked for. While type of this field is flexible, specific API endpoint always guarantees fixed amount of possible outcomes, and very often it can be strong-typed on per-endpoint basis. Can be null if we don't have any specific result for you to retrieve.

`Success` - `bool` value providing a simple way to check the result. This is offered as an extra to HTTP status codes, since `2xx` codes are considered `true`, while everything else is considered `false`. Please note that this property only indicates if **API request succeeded** - you should parse `Result` property for verifying if particular action was completed successfully, such as sending a command.

---

### `GET /Api/ASF`

This API endpoint can be used for fetching general data about ASF process as a whole. Returns **[GenericResponse](#genericresponse)** with `Result` defined as **[ASFResponse](#asfresponse)**.

```shell
curl -X GET /Api/ASF
{"Message":"OK","Result":{"GlobalConfig":{"AutoRestart":true,"BackgroundGCPeriod":0},"MemoryUsage":1843,"ProcessStartTime":"2018-01-30T21:32:01.8132984+01:00","Version":{"Major":3,"Minor":0,"Build":6,"Revision":1,"MajorRevision":0,"MinorRevision":1}},"Success":true}
```

#### ASFResponse

```json
{
	"GlobalConfig": {
		"AutoRestart": true,
		"BackgroundGCPeriod": 0
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

`GlobalConfig` is specialized C# object used by ASF for accessing to its config. It has exactly the same structure as **[global config](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#global-config)** explained in configuration, and it also exposes all available config variables. This property can be used for determining with what options the ASF program is configured to work. In example structure above, only a subset of all properties is shown in order to keep it clean.

`MemoryUsage` - `uint` value that specifies **managed** runtime memory used by ASF process as a whole, in kilobytes.

`ProcessStartTime` - `DateTime` value that specifies when exactly the ASF process has been started. This can be used for calculating e.g. program uptime. In JSON, ASF serializes `DateTime` object to **[ISO 8601](https://en.wikipedia.org/wiki/ISO_8601)** string that contains date, time, as well as timezone being used.

`Version` - `Version` value that specifies version of the currently running ASF binary. `Version` type contains 4 important `Major`, `Minor`, `Build` and `Revision` properties that correspond to appropriate digits in ASF `A.B.C.D` notation.

---

### `POST /Api/ASF`

#### Body:

Content-Type: application/json

```json
{
	"GlobalConfig": {},
	"KeepSensitiveDetails": true
}
```

This API endpoint can be used for updating **[GlobalConfig](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#global-config)** of ASF program. In other words, this will update `ASF.json` of `config` directory with **[GlobalConfig](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#global-config)** JSON object supplied in request body. Returns **[GenericResponse](#genericresponse)** with `Result` defined as `null`.

`GlobalConfig` is **[GlobalConfig](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#global-config)** JSON object. This field is mandatory and cannot be `null`. Specifying config properties with their default values might be omitted, just like in regular ASF config.

`KeepSensitiveDetails` is `bool` type that specifies whether sensitive details such as `WebProxyPassword` should be inherited from existing config (if available). This field is optional and defaults to `true`. When enabled, sensitive properties defined with value of `null` will be inherited from current config.

Currently, following properties are considered sensitive and can be set to `null` in order to be inherited: `WebProxyPassword`.

```shell
curl -X POST -H "Content-Type: application/json" -d '{"GlobalConfig":{"AutoRestart":false,"BackgroundGCPeriod":0}}' /Api/ASF
{"Message":"OK","Result":null,"Success":true}
```

---

### `DELETE /Api/Bot/{BotNames}`

This API endpoint can be used for completely erasing given bots specified by their `BotNames`, together with all their files. In other words, this will remove `BotName.*` files (included, but not limited to: `json`, `db`, `bin`, `maFile` and likewise) from your `config` directory of all chosen bots. This endpoint accepts multiple `BotNames` separated by a comma, as well as `ASF` keyword for deleting all defined bots. Returns **[GenericResponse](#genericresponse)** with `Result` defined as `null`.

```shell
curl -X DELETE /Api/Bot/archi
{"Message":"OK","Result":null,"Success":true}
```

---

### `GET /Api/Bot/{BotNames}`

This API endpoint can be used for fetching status of given bots specified by their `BotNames` - it returns basic statuses of the bots. This endpoint accepts multiple `BotNames` separated by a comma, as well as `ASF` keyword for returning all defined bots. Returns **[GenericResponse](#genericresponse)** with `Result` defined as `HashSet<Bot>` - collection of bot statuses.

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
	"BotConfig": {
		"Enabled": true,
		"Paused": false
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

`BotConfig` is specialized C# object used by Bot for accessing to its config. It has exactly the same structure as **[bot config](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#bot-config)** explained in configuration, and it also exposes majority of available config variables. This property can be used for determining with what options the bot is configured to work. Sensitive account-related information such as `SteamLogin`, `SteamPassword` and `SteamParentalPIN` are intentionally omitted from being included. In example structure above, only a subset of all properties is shown in order to keep it clean.

`KeepRunning` is a `bool` type that specifies if bot is active. Active bot is a bot that has been started, either by ASF on startup, or by user later during execution. If bot is stopped, this property will be `false`. Keep in mind that this property has nothing to do with bot being connected to Steam network, or not (that is what `SteamID` can be used for).

#### CardsFarmer

`GamesToFarm` is a `HashSet<Game>` (collection of `Game` elements) object that contains games pending to farm in current farming session. Please note that collection is updated on as-needed basis regarding performance. For example, when idling with `Simple` cards farming algorithm, ASF won't bother checking if we got any new games to farm when new game gets added (as we'd do that check anyway when we're out of queue, and by not doing so immediately we save requests and bandwidth). Therefore, this is data regarding current farming session, that might be different from overall data.

`CurrentGamesFarming` is a `HashSet<Game>` (collection of `Game` elements) object that contains games being farmed right now. In comparison with `GamesToFarm`, this property defines current status instead of pending queue, and it's heavily affected by currently selected cards farming algorithm. This collection can contain only up to `32` games (`MaxGamesPlayedConcurrently` enforced by Steam Network). You also have a guarantee that only entries already existing in `GamesToFarm` can be included here.

`TimeRemaining` is a `TimeSpan` type that specifies approximated time required to farm all games specified in `GamesToFarm` collection. This is nowhere close to the actual time that will be required, but it's a nice indicator with accuracy that might be improved in future, therefore it can be used for various display purposes. It's not updated in real-time, but calculated from current `GamesToFarm` status, therefore it's re-calculated the moment `CardsRemaining` of any game changes.

`Paused` is a `bool` type that specifies if `CardsFarmer` is currently paused. CardsFarmer can be paused due to various events, mainly `pause` and `play` commands. Paused CardsFarmer will not attempt to farm anything in automatic mode, neither will check badges every `IdleFarmingPeriod` hours.

#### Game

`AppID` is `uint` type that in unique way identifies game being played. ASF enforces this to be greater than `0`.

`GameName` is `string` type that provides friendly name of game identified by `AppID`. This is data returned by Steam Community. ASF enforces this to be `non-null` and `non-empty`.

`HoursPlayed` is `float` type that provides information how many hours the game has been played. This property is not updated in real time, but on as-needed basis, at least once per `FarmingDelay` minutes. Please note that initially this data is retrieved from Steam Community, but then updated according to ASF built-in timers, therefore it might not match what Steam Community is returning - this is because Steam Community data is not provided in real time either, and ASF requires such data for stopping farming for hours game as soon as it reaches `HoursUntilCardDrops` value. ASF enforces this property to be at least `0.0`.

`CardsRemaining` is `ushort` type that tells how many cards are remaining for the game. This property is updated as soon as possible and it should always have a value greater than `0`. However, it is possible for this property to have `0` value for a short moment when ASF is switching game.

---

### `POST /Api/Bot/{BotName}`

#### Body:

Content-Type: application/json

```json
{
	"BotConfig": {},
	"KeepSensitiveDetails": true
}
```

This API endpoint can be used for creating/updating **[BotConfig](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#bot-config)** of given bot specified by its `BotName`. In other words, this will update `BotName.json` of `config` directory with **[BotConfig](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#bot-config)** JSON object supplied in request body. Returns **[GenericResponse](#genericresponse)** with `Result` defined as `null`.

`BotConfig` is **[BotConfig](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#bot-config)** JSON object. This field is mandatory and cannot be `null`. Specifying config properties with their default values might be omitted, just like in regular ASF config.

`KeepSensitiveDetails` is `bool` type that specifies whether sensitive details such as `SteamLogin` or `SteamPassword` should be inherited from existing config (if available). This field is optional and defaults to `true`. When enabled, sensitive properties defined with value of `null` will be inherited from current config.

Currently, following properties are considered sensitive and can be set to `null` in order to be inherited: `SteamLogin`, `SteamPassword`, `SteamParentalPIN`.

```shell
curl -X POST -H "Content-Type: application/json" -d '{"BotConfig":{"Enabled":false,"Paused":true}}' /Api/Bot/archi
{"Message":"OK","Result":null,"Success":true}
```

---

### `POST /Api/Command/{Command}`

#### Body: empty

This API endpoint can be used executing given command specified by its `{Command}`. It's recommended to always specify the bot that is supposed to execute the command, otherwise the first defined bot will be used instead. Returns **[GenericResponse](#genericresponse)** with `Result` defined as `string` - the output of the executed command.

```shell
curl -X POST -d '' /Api/Command/version
{"Message":"OK","Result":"\r\n<archi> ASF V3.0.5.3","Success":true}
```

---

### `POST /Api/GamesToRedeemInBackground/{BotName}`

#### Body:

Content-Type: application/json

```json
{
	"GamesToRedeemInBackground": {
		"string": "string",
		"string": "string"
	}
}
```

This API endpoint can be used for adding extra  **[games to redeem in background](https://github.com/JustArchi/ArchiSteamFarm/wiki/Background-games-redeemer)** to given bot specified by its `BotName`. Returns **[GenericResponse](#genericresponse)** with `Result` defined as `OrderedDictionary<string, string>`.

`GamesToRedeemInBackground` is `OrderedDictionary<string, string>` JSON object that maps cd-keys to redeem (`key`) with their names (`value`). This field is mandatory and cannot be `null`. Neither any `key` nor `value` in the dictionary can be `null` or empty. In addition to that, every `key` must have valid Steam cd-key structure, ASF will validate that using built-in regex. Invalid entries will be automatically removed during import process.

The `Result` is `GamesToRedeemInBackground` that were successfully added to the collection of games pending to redeem. Like specified above, invalid entries will be automatically removed during import process, so you can use this result for a comparison with your original request in order to check which entries were deemed as invalid and skipped during import process. If you didn't supply any invalid data, this result will be exactly the same as `GamesToRedeemInBackground` that you've just sent to ASF.

```shell
curl -X POST -H "Content-Type: application/json" -d '{"GamesToRedeemInBackground":{"AAAAA-BBBBB-CCCCC":"Orwell","XXXXX-YYYYY-ZZZZZ":"Factorio"}}' /Api/GamesToRedeemInBackground/archi
{"Message":"OK","Result":{"AAAAA-BBBBB-CCCCC":"Orwell","XXXXX-YYYYY-ZZZZZ":"Factorio"},"Success":true}
```

---

### `GET /Api/Log`

This API endpoint can be used for fetching real-time log messages being written by ASF. In comparison with other endpoints, this one uses **[websocket](https://en.wikipedia.org/wiki/WebSocket)** connection for providing real-time updates. Each message is encoded in **[UTF-8](https://en.wikipedia.org/wiki/UTF-8)** and has a **[GenericResponse](#genericresponse)** structure with `Result` defined as `string` - the message rendered in configured by user NLog-specific layout. On initial connection, ASF will also push a burst of last few logged messages as a short history (by default last 20, but user is free to change this number, as well as disabling history entirely).

The websocket connection established with this endpoint is **read-only** - ASF will accept only `Close` **[frame](https://tools.ietf.org/html/rfc6455#section-5.5.1)** indicating that websocket connection should be gracefully closed. Any other data frame will result in connection being terminated.

```shell
curl -X GET -i -N -H "Connection: Upgrade" -H "Upgrade: websocket" /Api/Log
HTTP/1.1 200 OK

# Example of messages being sent by ASF, keep in mind that result string is affected by user-specified NLog logging layout
{"Message":"OK","Result":"2018-01-31 03:19:34|dotnet-2884|INFO|ASF|Start() IPC server ready!","Success":true}
```

---

### `GET /Api/Structure/{Structure}`

This API endpoint can be used for fetching structure of given JSON object specified by its `Structure` name - it returns JSON-serialized default object for given structure. Returns **[GenericResponse](#genericresponse)** with `Result` defined as `object`.

`{Structure}` can be any ASF or .NET Core structure qualified by its namespace and name, for example `ArchiSteamFarm.BotConfig`, `ArchiSteamFarm.GlobalConfig` or `ArchiSteamFarm.Json.Steam+Asset.`

In the example below the actual result was trimmed to keep it clean - normally you'll get full structure returned, which is the main purpose of this endpoint. The resulting structure always includes all public and non-public (but not private) fields and properties.

```shell
curl -X GET /Api/Structure/ArchiSteamFarm.BotConfig
{"Message":"OK","Result":{"AcceptGifts":false,"TradingPreferences":0},"Success":true}
```

In comparison with `GET /Api/Type`, this endpoint returns JSON representation of an object of given type, in its default state.

---

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

`CustomAttributes` - `HashSet<string>` value that specifies what custom attributes apply to this type. This property is especially useful when `BaseType` is `System.Enum`, as in this case you can check if it's special `flags` enum by verifying that `System.FlagsAttribute` is defined in this collection. This value can be null when there are no custom attributes defined for this object. Together with `UnderlyingType`, this tells you that `ArchiSteamFarm.BotConfig+ETradingPreferences` is `byte flags` enum.

`UnderlyingType` - `string` value that specifies underlying type for this type. This is used mainly with `System.Enum` to know what underlying type this enum uses for data storage. For example in most ASF enums, this will be `System.Byte`. Together with `CustomAttributes`, this tells you that `ArchiSteamFarm.BotConfig+ETradingPreferences` is `byte flags` enum.

---

## WWW API

APIs below are dedicated for our IPC GUI usage and they should not be implemented by remote scripts or tools. This documentation is for our internal reference only and can change anytime, in any possible way. You should not rely on existance of below endpoints, neither implement them in your own tools.

---

### `GET /Api/WWW/Directory/{Directory}`

This API endpoint can be used for fetching directory's content specified by its local path relative to `www` directory. Returns **[GenericResponse](#genericresponse)** with `Result` defined as `HashSet<string>` - collection of local filenames.

```shell
curl -X GET /Api/WWW/Directory/css
{"Message":"OK","Result":["app.css","_all-skins.min.css","_nightmode.min.css"],"Success":true}
```