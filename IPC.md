# IPC

ASF includes its own unique IPC interface that can be used for further interaction with the process. IPC stands for **[inter-process communication](https://en.wikipedia.org/wiki/Inter-process_communication)** and in the most simple definition this is "ASF API" that can be used for further integration with the process.

Our IPC interface is based on **[Kestrel HTTP server](https://github.com/aspnet/KestrelHttpServer)** and offers **[RESTful](https://en.wikipedia.org/wiki/Representational_state_transfer)** web API that is based on JSON as its primary data format. You can access ASF API by sending appropriate web requests to `/Api` endpoint.

In addition to API endpoints, our IPC also offers static files hosting used by our IPC GUI frontend that is designed for endusers as an alternative, simplified way to access ASF API.

IPC can be used for a lot of different things, depending on your needs and skills. For example, you can use it for fetching status of ASF and all bots, sending ASF commands, fetching and editing global/bot configs, adding new bots, deleting existing bots, submitting keys for **[BGR](https://github.com/JustArchi/ArchiSteamFarm/wiki/Background-games-redeemer)** or accessing ASF's log file. All of those actions are exposed by our API, which means that you can code your own tools and scripts that will be able to communicate with ASF and influence it during runtime. In addition to that, selected actions (such as sending commands) are also implemented by our IPC GUI which allows you to easily access them from your favourite web browser.

---

# Usage

You can enable our IPC interface by enabling `IPC` **[global configuration property](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#global-config)**. ASF will state IPC launch in its log, which you can use for verifying if IPC interface has started properly:

```
INFO|ASF|Start() Starting IPC server...
INFO|ASF|Start() IPC server ready!
```

ASF's http server is now listening on selected endpoints. If you didn't provide a custom configuration file for IPC, those will be IPv4-based **[127.0.0.1](http://127.0.0.1:1242)** and IPv6-based **[[::1]](http://[::1]:1242)** on default `1242` port. You can access our IPC interface by above links, from the same machine. If ASF's IPC interface was enabled properly, you should see our **[IPC GUI](https://i.imgur.com/VjHtWYu.png)** frontend in your browser.

---

# IPC GUI

Please note that IPC GUI is currently in **preview** state, which means that officially it's **unsupported** and we're also not accepting bug reports (standalone issues) for it. This is because it's a community project and we're not maintaining it officially without ASF developer in charge of it.

You're free to post your thoughts, suggestions and bug reports in appropriate **[issue](https://github.com/JustArchi/ArchiSteamFarm/issues?q=is%3Aissue+is%3Aopen+label%3AIPC)** that is fully dedicated to IPC GUI. Right now, our lead community developer of IPC GUI is **[@MrBurrBurr](https://github.com/mrburrburr)**.

---

# IPC API

Our IPC API can be accessed by sending appropriate requests to appropriate endpoints. You can use those API endpoints to make your own helper scripts, tools, GUIs and alike. This is exactly what our IPC GUI does under the hood, and every other tool can achieve the same. Sending API calls is officially and fully supported by ASF team.

Communication with IPC server provided by ASF can be done by using any http-compatible program, tool or code. In our examples below we'll use **[curl](https://curl.haxx.se)** which is cross-platform, open-source tool for data transfer. You can very easily adapt our curl examples for your own scripts, tools or programs. 

---

## HTTP status codes

Our API makes use of standard HTTP status codes, and we use them according to the RFC. In the most simplified explanation, our API will return `200 OK` status if your API call succeeded, and non-200 otherwise. This should be used as a primary way to detect whether your ASF API call succeeded or not.

ASF can use various HTTP status codes to explain better what happened. Some of them include:

- `200 OK` - the request has completed successfully.
- `400 BadRequest` - the request has failed because of an error, parse our response body for actual reason. Most of the time this is ASF, understanding the request, but refusing to execute it due to provided reason.
- `401 Unauthorized` - ASF has `IPCPassword` set, but you've failed to **[authenticate](#authentication)** properly.
- `403 Forbidden` - ASF has `IPCPassword` set and you've failed to **[authenticate](#authentication)** properly too many times, try again in an hour.
- `404 NotFound` - the URL that you're trying to reach does not exist. Confirm with IPC examples below whether you're accessing appropriate endpoint.
- `405 MethodNotAllowed` - the HTTP method you're trying to use is not allowed for this API endpoint. This can be caused e.g. by sending `GET` request where ASF expects `POST`, or vice-versa. This code can also be used when trying to access websocket endpoint without initiating a websocket connection (upgrade).
- `406 NotAcceptable` - your `Content-Type` header is not acceptable for this API endpoint. This is mainly used in requests that require from you a specific body as an input, and you didn't declare in what format it's provided.
- `411 LengthRequired` - your `POST` request is missing `Content-Length` header. Length must be defined even in requests that do not have a body (use length of 0 in this case).
- `500 InternalServerError` - IPC server ran into fatal condition, this indicates ASF issue that should be reported and corrected. We do not normally use this status anywhere in the code, please let us know.
- `501 NotImplemented` - this URL is reserved for future use and not implemented yet. In some very rare situations, ASF might also use this status to indicate that it understands what you're trying to do, but it's not supported (yet).
- `503 ServiceUnavailable` - ASF ran into one of possible exceptions during execution of this request. If not returned in the response, ASF log should be able to tell more.

---

## Authentication

ASF IPC interface by default does not require any sort of authentication, as `IPCPassword` is set to `null`. However, if `IPCPassword` is enabled by being set to any non-empty value, every call to ASF's API requires the password that matches set `IPCPassword`. If you omit authentication or input wrong password, you'll get `401 - Unauthorized` error. If you continue sending requests without authentication, eventually you'll get temporarily blocked with `403 - Forbidden` error.

Authentication can be done through two supported ways.

### `Authentication` header

In general you should use HTTP request headers, by setting `Authentication` field with your password as a value. The way of doing that depends on the actual tool you're using for accessing ASF's IPC interface, for example if you're using `curl` then you should add `-H 'Authentication: MyPassword'` as a parameter. This way authentication is passed in the headers of the request, where it in fact should take place.

### `password` parameter in query string

Alternatively you can append `password` parameter to the end of the URL you're about to call, for example by calling `/Api/Command/version?password=MyPassword` instead of `/Api/Command/version` alone. This approach is good enough for majority of use cases, as it's user-friendly and can be even saved as a bookmark, but obviously it exposes password in the open, which is not necessarily always appropriate. In addition to that it's extra argument in the query string, which complicates the look of the URL and makes it feel like it's URL-specific, while it applies to the entire IPC communication.

---

Both ways are supported in exactly the same way and it's totally up to you which one you want to choose. We still recommend to use HTTP headers everywhere where you can, as usage-wise it's more appropriate than query string. However, we support query string, as due to various limitations it's not possible to use headers everywhere - for example it's impossible to specify custom headers while initiating a websocket connection in javascript (even though it's completely valid according to the RFC).

---

## API

In general our API is a typical REST API that is based on JSON as a primary way of serializing/deserializing data. We're doing our best to precisely describe response, using both HTTP status codes (where appropriate), as well as a response you can parse yourself in order to know whether the request succeeded, and if not, then why.

Some API endpoints might require from you to specify extra data, such as providing appropriate structure as a body of the request. In this case, in addition to providing required input, you must also set `Content-Type` header to appropriate value, such as `application/json` if your input is provided in JSON. If API endpoint includes some special requirements for an input, it'll be listed on the top of the endpoint description.

In all provided examples below, you're expected to modify URL parameter by prepending appropriate `Protocol://Host:Port/OptionalPrefix` to the URL, according to your ASF usage. For example, if you're trying to execute `/Api/ASF` and you're running your IPC on standard `http://127.0.0.1:1242` endpoint, call `http://127.0.0.1:1242/Api/ASF` instead.

Numeric properties are defined with their maximum values, so you can also use strong-typing for them, such as `uint` for `AppID`, and `ulong` for `SteamID`. Selected `ulong` fields that are serialized as numbers might include extra `s_` fields serialized as strings that can be consumed by javascript (and other languages with similar limitations) which can't represent 64-bit numbers precisely.

---

## GenericResponse

Generic response is a primary return type that we use for all API calls. We use two kinds of generic response - with a result, and without.

Generic response without a result has two mandatory fields that are always provided - `Success` and `Message`. This kind is used in all API calls that do not return any specific result, apart from generic success/failure. This response is used mainly in `DELETE` and `POST` calls, for example in `DELETE /Api/Bot/{Bot}` call.

```json
{
	"Message": "string",
	"Success": true
}
```

`Message` - `string` value providing extra details about the response. This could be simple "OK" when request succeeded, or actual failure reason if it didn't. We use this field as a general help for you to know what happened about the request you've sent. Keep in mind that this field is NOT a result of your request, only a description of what happened. Can be `null` if we don't have any specific message for you to retrieve, although we try to avoid that as much as possible.

`Success` - `bool` value providing a simple way to check the result. This is offered as an extra to HTTP status codes, since `2xx` codes are considered `true`, while everything else is considered `false`. Please note that this property only indicates if **API call succeeded**, you should parse `Result` property for checking if a particular action was completed successfully (such as a result of a command).

---

Generic response with result, in addition to two mandatory fields described above, also includes mandatory `Result` field that contains actual result of the call. This response is used mainly in `GET` calls that return a result by definition, for example `GET /Api/ASF`.

```json
{
	"Result": {},
	"Message": "string",
	"Success": true
}
```

`Result` - `object` value providing actual result of your request. The type of this field depends on API endpoint that you called - for example it can be a `Bot` or a `string`. Most commonly used in `GET` requests for fetching actual data that you asked for. While type of this field is flexible, specific API endpoint always guarantees fixed amount of possible outcomes, and very often it can be strong-typed on per-endpoint basis. Can be `null` if we don't have any specific result for you to retrieve.

`Message`/`Success` - as above.

---

## Public API

### `GET /Api/ASF`

This API endpoint can be used for fetching general data about ASF process as a whole. Returns **[GenericResponse](#genericresponse)** with `Result` defined as **[ASFResponse](#asfresponse)**.

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

`BuildVariant` - `string` value that specifies build variant of currently running ASF process. This can be any variant available under **[releases](https://github.com/JustArchi/ArchiSteamFarm/releases)**, as well as variants that are not officially available for download (such as `docker` variant for our Docker images or `source` variant for unofficial builds).

`GlobalConfig` - specialized C# object used by ASF for accessing to its config. It has exactly the same structure as **[global config](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#global-config)** explained in configuration. This property can be used for finding out options that the ASF process is configured to work with. Typically this object will include only a subset of all config properties - those that the user has modified (minimalistic config). Sensitive process-related information such as `WebProxyPassword` are always skipped from being included.

`MemoryUsage` - `uint` value that specifies **managed** runtime memory used by ASF process as a whole, in kilobytes.

`ProcessStartTime` - `DateTime` value that specifies when exactly the ASF process has been started. This can be used for calculating e.g. program uptime. In JSON, ASF serializes `DateTime` object to **[ISO 8601](https://en.wikipedia.org/wiki/ISO_8601)** string that contains date, time, as well as timezone being used.

`Version` - `Version` value that specifies version of the currently running ASF binary. `Version` type contains 4 important `Major`, `Minor`, `Build` and `Revision` properties that correspond to appropriate digits in ASF `A.B.C.D` notation.

---

### `POST /Api/ASF`

#### Body:

```json
{
	"GlobalConfig": {},
	"KeepSensitiveDetails": true
}
```

This API endpoint can be used for updating **[GlobalConfig](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#global-config)** of ASF program. In other words, this will update `ASF.json` of `config` directory with **[GlobalConfig](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#global-config)** JSON object supplied in request body. Returns **[GenericResponse](#genericresponse)** with no result.

`GlobalConfig` is **[GlobalConfig](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#global-config)** JSON object. This field is mandatory and cannot be `null`. Specifying config properties with their default values might be omitted, just like in regular ASF config.

`KeepSensitiveDetails` is `bool` type that specifies whether sensitive details such as `WebProxyPassword` should be inherited from existing config (if available). This field is optional and defaults to `true`. When enabled, sensitive properties defined with value of `null` will be inherited from current config.

Currently, following properties are considered sensitive and can be set to `null` in order to be inherited: `WebProxyPassword`.

```shell
curl -X POST -H "Content-Type: application/json" -d '{"GlobalConfig":{"AutoRestart":false,"Blacklist":[440]}}' /Api/ASF
{"Message":"OK","Success":true}
```

---

### `DELETE /Api/Bot/{BotNames}`

This API endpoint can be used for completely erasing given bots specified by their `BotNames`, together with all their files. In other words, this will remove `BotName.*` files (included, but not limited to: `json`, `db`, `bin`, `maFile`, `keys` and likewise) from your `config` directory of all chosen bots. This endpoint accepts multiple `BotNames` separated by a comma, as well as `ASF` keyword for deleting all defined bots. Returns **[GenericResponse](#genericresponse)** with no result.

```shell
curl -X DELETE /Api/Bot/archi
{"Message":"OK","Success":true}
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
	"BotConfig": {},
	"KeepRunning": true
}
```

`BotName` is `string` type that defines name of the bot. This is the same identifier that is used for commands and all other identification-related bot activity.

`CardsFarmer` is specialized C# object used by Bot for cards-farming purpose. It provides information related to cards farming progress of given bot instance. Its structure is explained **[below](#cardsfarmer)**.

`AccountFlags` is `EAccountFlags` (`uint` flags) type, defined by SK2 **[here](https://github.com/SteamRE/SteamKit/blob/e0c46a45d3fbf3bf1b0df3e2e0348dc1067458df/Resources/SteamLanguage/enums.steamd#L82-L117)**, that specifies Steam account flags of given account. This property can be used for getting more information about the status of Steam account being in ASF, for example if it's **[limited](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663)**, by checking if `LimitedUser` or `LimitedUserForce` flags are set. This property is initialized (and updated) the moment Bot logs in to Steam network, therefore it'll have a value of `0` before first login.

`AvatarHash` is `string` type that contains Steam avatar hash being used by given bot. It's possible to use this value for building URL pointing to user's avatar on Steam CDN. Can be `null` if user didn't set his avatar.

`IsPlayingPossible` is `bool` type that specifies if account being used by a bot can be used for automatic idling. This property will be `false` when Steam library of the account is being used elsewhere, either normally, or via family sharing. This property affects only remote logins and does not cover its own process (so if account is not being used anywhere else, `IsPlayingPossible` will always be `true`, even if ASF is actively idling games on it right now).

`SteamID` is `ulong` unique steamID identificator of currently logged in account in 64-bit form. This property will have a value of `0` if bot is not logged in to Steam Network (therefore it can be used for telling if account is logged in or not).

`BotConfig` is specialized C# object used by Bot for accessing to its config. It has exactly the same structure as **[bot config](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#bot-config)** explained in configuration. This property can be used for finding out options that the bot is configured to work with. Typically this object will include only a subset of all config properties - those that the user has modified (minimalistic config). Sensitive account-related information such as `SteamLogin`, `SteamPassword` and `SteamParentalPIN` are always skipped from being included.

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

```json
{
	"BotConfig": {},
	"KeepSensitiveDetails": true
}
```

This API endpoint can be used for creating/updating **[BotConfig](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#bot-config)** of given bot specified by its `BotName`. In other words, this will update `BotName.json` of `config` directory with **[BotConfig](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#bot-config)** JSON object supplied in request body. Returns **[GenericResponse](#genericresponse)** with no result.

`BotConfig` is **[BotConfig](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#bot-config)** JSON object. This field is mandatory and cannot be `null`. Specifying config properties with their default values might be omitted, just like in regular (minimalistic) ASF config.

`KeepSensitiveDetails` is `bool` type that specifies whether sensitive details such as `SteamLogin` or `SteamPassword` should be inherited from existing config (if available). This field is optional and defaults to `true`. When enabled, sensitive properties defined with value of `null` will be inherited from current config.

Currently, following properties are considered sensitive and can be set to `null` in order to be inherited: `SteamLogin`, `SteamPassword`, `SteamParentalPIN`.

```shell
curl -X POST -H "Content-Type: application/json" -d '{"BotConfig":{"Enabled":false,"Paused":true}}' /Api/Bot/archi
{"Message":"OK","Success":true}
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

### `DELETE /Api/GamesToRedeemInBackground/{BotName}`

This API endpoint can be used for completely erasing `.keys.used` and `.keys.unused` files of given bots specified by their `BotNames` in `config` directory. This endpoint accepts multiple `BotNames` separated by a comma, as well as `ASF` keyword for deleting those files of all defined bots. Returns **[GenericResponse](#genericresponse)** with no result.

```shell
curl -X DELETE /Api/GamesToRedeemInBackground/archi
{"Message":"OK","Success":true}
```

---

### `GET /Api/GamesToRedeemInBackground/{BotName}`

This API endpoint can be used for fetching `.keys.used` and `.keys.unused` files of given bots specified by their `BotNames` in `config` directory. Returns **[GenericResponse](#genericresponse)** with `Result` defined as `Dictionary<string, GamesToRedeemInBackgroundResponse>` - a map that maps `BotName` to a **[GamesToRedeemInBackgroundResponse](#gamestoredeeminbackgroundresponse)** (explained below).

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

Both `UnusedKeys` and `UsedKeys` are `Dictionary<string, string>` objects that map redeemed cd-keys (`key`) with their names (`value`). This is the result of a `POST` call described below and can be used for remotely fetching keys without accessing ASF config directory. Both objects can be `null` in case of ASF error during fetching files (such as I/O), but empty or missing files will behave properly and produce empty dictionary.

---

### `POST /Api/GamesToRedeemInBackground/{BotName}`

#### Body:

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

The websocket connection established with this endpoint is **read-only** - ASF will accept only **[control frames](https://tools.ietf.org/html/rfc6455#section-5.5)**, especially `Close` frame indicating that websocket connection should be gracefully closed. Sending any data frame will result in connection being terminated.

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

`CustomAttributes` - `HashSet<string>` value that specifies what custom attributes apply to this type. This property is especially useful when `BaseType` is `System.Enum`, as in this case you can check if it's special `flags` enum by verifying that `System.FlagsAttribute` is defined in this collection. This value can be `null` when there are no custom attributes defined for this object. Together with `UnderlyingType`, this tells you that `ArchiSteamFarm.BotConfig+ETradingPreferences` is `byte flags` enum.

`UnderlyingType` - `string` value that specifies underlying type for this type. This is used mainly with `System.Enum` to know what underlying type this enum uses for data storage. For example in most ASF enums, this will be `System.Byte`. Together with `CustomAttributes`, this tells you that `ArchiSteamFarm.BotConfig+ETradingPreferences` is `byte flags` enum.

---

## Internal API

APIs below are dedicated for our IPC GUI usage and they should not be used by remote scripts or tools. This documentation is for our internal reference only and can change anytime, in any possible way. You should not rely on existence of below endpoints, neither implement them in your own tools.

---

### `GET /Api/WWW/Directory/{Directory}`

This API endpoint can be used for fetching directory's content specified by its local path relative to `www` directory. Returns **[GenericResponse](#genericresponse)** with `Result` defined as `HashSet<string>` - collection of local filenames.

```shell
curl -X GET /Api/WWW/Directory/css
{"Message":"OK","Result":["app.css","_all-skins.min.css","_nightmode.min.css"],"Success":true}
```

---

### `POST /Api/WWW/Send`

#### Body:

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

---

## FAQ

### Is ASF's IPC interface secure and safe to use?

ASF by default listens only on `localhost` addresses, which means that accessing ASF IPC from any other machine but your own **is impossible**. Unless you modify default endpoints, attacker would need a direct access to your own machine in order to access ASF's IPC, therefore it's as secure as it can be and there is no possibility of anybody else accessing it, even from your own LAN.

However, if you decide to change default `localhost` bind addresses to something else, such as `any`, then you're supposed to set proper firewall rules **yourself** in order to allow only authorized IPs to access ASF's IPC interface. In addition to doing that, we strongly recommend to set up `IPCPassword`, that will add another layer of extra security. You might also want to run ASF's IPC interface behind a reverse proxy in this case, which is further explained below.

### Can I use ASF's IPC behind a reverse proxy such as Apache or Nginx?

**Yes**, our IPC is fully compatible with such setup, so you're free to host it also in front of your own tools for extra security and compatibility, if you'd like to. In general ASF's Kestrel http server is very secure and possesses no risk when being connected directly to the internet, but putting it behind a reverse-proxy such as Apache or Nginx might provide extra functionality that wouldn't be possible to achieve otherwise, such as securing ASF's interface with a **[basic auth](https://en.wikipedia.org/wiki/Basic_access_authentication)**.

Example Nginx configuration can be found below. We included full `server` block, although you're interested mainly in `location` ones. Please refer to **[nginx documentation](https://nginx.org/en/docs)** if you need further explanation.

```nginx
server {
        listen *:443 ssl;
        server_name asf.mydomain.com;
        ssl_certificate /path/to/your/certificate.crt;
        ssl_certificate_key /path/to/your/certificate.key;

	location /Api/Log {
		proxy_pass http://127.0.0.1:1242;
#		proxy_set_header Host 127.0.0.1; # Only if you need to override default host
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
#		proxy_set_header Host 127.0.0.1; # Only if you need to override default host
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		proxy_set_header X-Forwarded-Host $host:$server_port;
		proxy_set_header X-Forwarded-Proto $scheme;
		proxy_set_header X-Forwarded-Server $host;
		proxy_set_header X-Real-IP $remote_addr;
	}
}
```

### Can I access IPC interface through HTTPS protocol?

**Yes**, you can achieve it through two different ways. A recommended way would be to use a reverse proxy for that (described above) where you can access your web server through https like usual, and connect through it with ASF's IPC interface on the same machine. This way your traffic is fully encrypted and you don't need to modify IPC in any way to support such setup.

Second way includes specifying a **[custom config](#custom-configuration)** for ASF's IPC interface where you can enable https endpoint and provide appropriate certificate directly to our Kestrel http server. This way is recommended if you're not running any other web server and don't want to run one exclusively for ASF. Otherwise, it's much easier to achieve a satisfying setup by using a reverse proxy mechanism.

---

## Custom configuration

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

Unless you truly need to specify a custom base path, it's best to leave it at default. In addition to that, custom base path is **[not supported by our IPC GUI yet](https://github.com/JustArchi/ArchiSteamFarm/issues/869#issuecomment-419596294)**. Our IPC API is fully compatible with it already.