# IPC

Starting with version 3.0, ASF offers http-based inter-process communication that can be used to communicate with the process. This is offered as an alternative to already existing steam chat communication.

IPC is always executed with `SteamOwnerID` permissions, which is `0` by default. In order to use it, you should set `SteamOwnerID` to the proper non-zero value. Default value will make IPC work, but not authorizing any client to execute any command. For more info about `SteamOwnerID`, visit **[configuration](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration)**.

---

## FAQ

### Why should I consider using IPC in ASF?

You should consider using IPC in ASF only if you have a strong reason. If you don't know what it is about, most likely you don't have one, and you can safely ignore this page along with the content. IPC stands for inter-process communication, which allows you to control the ASF process during execution, e.g. through a PHP script or similar. By using IPC you're able to control how ASF behaves, which might be useful if you're planning on integrating it further. If you're not running ASF on the server, and you're not planning on integrating it further with your own scripts/applications, it should be easier for you to communicate with ASF through steam chat with one of the bots. However, you can use IPC too, if you consider it useful/easier for you.

### Is this secure?

ASF by default listens only on `127.0.0.1` address, which means that accessing ASF IPC from any other machine but your own is impossible. Therefore, it's as secure as IPC can be. If you decide to change default `127.0.0.1` bind address to something else, such as `*`, then you're supposed to set proper firewall rules **yourself** in order to allow only authorized IPs to access ASF port. In addition to that, server must include properly set non-zero `SteamOwnerID`, otherwise it'll refuse to execute any command, as an extra security measure. On top of all of that, you can also set `IPCPassword`, which would add another layer of extra security.

---

## Server

To start IPC, ASF must be started with `--server` parameter. For example in OS-specific package on Windows:
```
ArchiSteamFarm.exe --server
```

Or on Linux/OS X:
```
./ArchiSteamFarm --server
```

Or in generic package on any platform:
```
dotnet ArchiSteamFarm.dll --server
```

If parameter was passed correctly, you should notice that IPC service is active:
```
INFO|ASF|StartServer() Starting IPC server on http://127.0.0.1:1242/IPC/...
INFO|ASF|StartServer() IPC server ready!
```

ASF is now listening on `http://127.0.0.1:1242/IPC` for incoming IPC connections (or whatever `IPCHost` and `IPCPort` you specified in the config).

---

## Client

Communication with IPC server provided by ASF can be done via any http-compatible program, including classical web browsers, as well as CLI utilities such as `curl`.

Currently ASF IPC offers minimalistic API for bots management that can be accessed by appropriate endpoints. In the future perhaps we'll succeed in making fully-featured IPC GUI that will access that API in user-friendly way (**[#610](https://github.com/JustArchi/ArchiSteamFarm/issues/610)**), but until then you need to access those endpoints manually.

---

## HTTP status codes

Our API makes use of following HTTP status codes:

- `200 OK` - the request completed successfully.
- `400 BadRequest` - the request failed because of an error, parse response body for actual reason.
- `401 Unauthorized` - ASF has `IPCPassword` set and you failed to **[authenticate](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC#authentication)** properly.
- `404 NotFound` - the URL you're trying to reach does not exist.
- `405 NotAllowed` - the HTTP method you're trying to use is not allowed for this API endpoint.
- `406 NotAcceptable` - your `Content-Type` header is not acceptable for this endpoint.
- `411 LengthRequired` - your request is missing `Content-Length` header.
- `501 NotImplemented` - this URL is reserved for future use, not implemented yet.
- `503 ServiceUnavailable` - ASF doesn't have `SteamOwnerID` properly set, command access is prohibited.

---

## Screenshots

TODO

---

## API

In general our API is a typical REST API that is based on JSON as a primary way of serializing/deserializing data. We're doing our best to precisely describe response, using both HTTP error codes (where appropriate), as well as JSON response you can parse yourself in order to know whether the request suceeded, and if not, then why.

Some API endpoints might require from you to specify extra data, such as providing appropriate JSON structure as a body of the request, together with setting `Content-Type` header to `application/json`. Provided examples of requests/responses show possible usage with **[curl](https://curl.haxx.se/)** tool - you're expected to modify URL parameter by prepending appropriate `protocol://HOST:PORT` for your ASF usage, such as `http://127.0.0.1:1242`.

### `GET /Api/Bot/{BotNames}`

This API endpoint can be used for fetching status of given bots specified by their `BotNames` - it returns basic statuses of the bots. This endpoint accepts multiple `BotNames` separated by a comma, as well as `ASF` keyword for returning all defined bots. Returns **[GenericResponse](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC#genericresponse)** with `Result` defined as HashSet<**[Bot](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC#bot)**> - collection of bot statuses.

```
curl -X GET /Api/Bot/archi
{"Message":"OK","Result":[{"BotName":"archi","CardsFarmer":{"CurrentGamesFarming":[],"GamesToFarm":[],"TimeRemaining":"00:00:00","Paused":false},"AccountFlags":0,"SteamID":0,"BotConfig":null,"KeepRunning":false}],"Success":true}
```

### `DELETE /Api/Bot/{BotNames}`

This API endpoint can be used for completely erasing given bots specified by their `BotNames`, together with all their files. In other words, this will remove `BotName.json`, `BotName.db`, `BotName.bin` and `BotName.maFile` from your `config` directory of all chosen bots. This endpoint accepts multiple `BotNames` separated by a comma, as well as `ASF` keyword for deleting all defined bots. Returns **[GenericResponse](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC#genericresponse)** with `Result` defined as `null`.

```
curl -X DELETE /Api/Bot/archi
{"Message":"OK","Result":null,"Success":true}
```

### `POST /Api/Bot/{BotName}`

#### Body:

Content-Type: application/json

```
{
	"BotConfig": {},
	"KeepSensitiveDetails": true
}
```

This API endpoint can be used for creating/updating **[BotConfig](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#bot-config)** of given bot specified by its `BotName`. In other words, this will update `BotName.json` of `config` directory with **[BotConfig](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#bot-config)** JSON object supplied in request body. Returns **[GenericResponse](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC#genericresponse)** with `Result` defined as `null`.

`BotConfig` is **[BotConfig](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#bot-config)** JSON object. This field is mandatory and cannot be `null`. Specifying config properties with their default values might be omitted, just like in regular ASF config.

`KeepSensitiveDetails` is `bool` type that specifies whether sensitive details such as `SteamLogin` or `SteamPassword` should be inherited from existing config (if available). This field is optional and defaults to `true`. When enabled, sensitive properties defined with value of `null` will be inherited from current config.

Currently, following properties are considered sensitive and can be set to `null` in order to be inherited: `SteamLogin`, `SteamPassword, `SteamParentalPIN`.

```
curl -X POST -H "Content-Type: application/json" -d '{"BotConfig":{"Enabled": false,"Paused":true}}' /Api/Bot/archi
{"Message":"OK","Result":null,"Success":true}
```

### **[Obsolete]** `GET /Api/Command/{Command}`
### `POST /Api/Command/{Command}`

This API endpoint can be used executing given command specified by its `{Command}`. It's recommended to always specify the bot that is supposed to execute the command, otherwise the first defined bot will be used instead. Returns **[GenericResponse](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC#genericresponse)** with `Result` defined as `string` - the output of the executed command.

```
curl -X POST -d '' /Api/Command/version
{"Message":"OK","Result":"\r\n<archi> ASF V3.0.5.3","Success":true}
```

**We recommend accessing this API endpoint via POST request only - GET request is designed as user-friendly version and will be considered for removal when IPC GUI is ready.**

---

## Authentication

ASF IPC interface by default does not require any sort of authentication, as `IPCPassword` is set to `null`. However, if `IPCPassword` is enabled by being set to any non-empty value, every call to ASF IPC interface requires the password that matches set `IPCPassword`. If you omit authentication or input wrong password, you'll get `401 - Unauthorized` error.

Authentication can be done through two generally-acceptable ways.

### `password` parameter in query string

You can append `password` parameter to the end of the URL you're about to call, for example by calling `/Api/Command/version?password=MyPassword` instead of `/Api/Command/version` alone. This approach is good enough for majority of use cases, as it's user-friendly and can be even saved as a bookmark, but obviously it exposes password in the open, which is not necessarily appropriate for all cases.

### `Authentication` header

Alternatively you can use HTTP request headers, by setting `Authentication` field with your password as a value. The way of doing that depends on the actual tool you're using for accessing ASF's IPC interface, for example if you're using `curl` then you should add `-H 'Authentication: MyPassword` as a parameter.

---

Both ways are supported in exactly the same way and it's totally up to you which one you want to choose. We recommend query string for users that just want to access protected ASF IPC interface or save link as a bookmark, and we recommend HTTP header for all tools, code, scripts and otherwise dev-related things where you have more freedom in terms of HTTP headers and actual communication. It's recommended to use `Authentication` header whenever possible.

---

## Cross-Origin Resource Sharing

ASF by default has `Access-Control-Allow-Origin` header set to `*`. This allows e.g. JavaScript scripts  to access ASF IPC interface in third-party web GUIs or tools. However, this also means that somebody could potentially upload malicious script that would make calls to ASF without your awareness or approval. If you'd like to ensure that such situation won't happen, consider setting up `IPCPassword` appropriately. This way if any script wants to access ASF's IPC interface, it'll need to authenticate each request, as described above.

---

## Structures

In case of numbers, we always provide maximum allowed value in example structures, so you can specify strong-defined expected type.

### Bot

```
{
	"BotName":"string",
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
		"TimeRemaining":"02:30:00",
		"Paused": false
	},
	"AccountFlags": 4294967295,
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

`AccountFlags` is `EAccountFlags` (`uint` flags) type, defined by SK2 **[here](https://github.com/SteamRE/SteamKit/blob/master/Resources/SteamLanguage/enums.steamd#L81)**, that specifies Steam account flags of given account. This property can be used for getting more information about the status of Steam account being in ASF, for example if it's **[limited](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663)**, by checking if `LimitedUser` or `LimitedUserForce` flags are set. This property is initialized (and updated) the moment Bot logs in to Steam network, therefore it'll have a value of `0` before first login.

`SteamID` is `ulong` unique steamID identificator of currently logged in account in 64-bit form. This property will have a value of `0` if bot is not logged in to Steam Network (therefore it can be used for telling if account is logged in or not).

`BotConfig` is specialized C# object used by Bot for accessing to its config. It has exactly the same structure as **[bot config](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#bot-config)** explained in configuration, and it also exposes majority of available config variables. This property can be used for determining with what options the bot is configured to work. Sensitive account-related information such as `SteamLogin`, `SteamPassword` and `SteamParentalPIN` are intentionally omitted from being included. In example structure above, only a subset of all properties is shown in order to keep it clean.

`KeepRunning` is a `bool` type that specifies if bot is active. Active bot is a bot that has been `!start`ed, either by ASF on startup, or by user later during execution. If bot is stopped, this property will be `false`. Keep in mind that this property has nothing to do with bot being connected to Steam network, or not (that is what `SteamID` can be used for).

#### CardsFarmer

`GamesToFarm` is a `HashSet<Game>` (collection of `Game` elements) object that contains games pending to farm in current farming session. Please note that collection is updated on as-needed basis regarding performance. For example, in `Simple` cards farming algorithm ASF won't bother checking if we got any new games to farm when new game gets added (as we'd do that check anyway when we're out of queue, and by not doing so immediately we save requests and bandwidth). Therefore, this is data regarding current farming session, that might be different from overall data.

`CurrentGamesFarming` is a `HashSet<Game>` (collection of `Game` elements) object that contains games being farmed right now. In comparison with `GamesToFarm`, this property defines current status instead of pending queue, and it's heavily affected by currently selected cards farming algorithm. This collection can contain only up to `32` games (`MaxGamesPlayedConcurrently` enforced by Steam Network).

`TimeRemaining` is a `TimeSpan` type that specifies approximated time required to farm all games specified in `GamesToFarm` collection. This is nowhere close to the actual time that will be required, but it's a nice indicator with accuracy that might be improved in future, therefore it can be used for various display purposes. It's not updated in real-time, but calculated from current `GamesToFarm` status, therefore it's re-calculated the moment `CardsRemaining` of any game changes.

`Paused` is a `bool` type that specifies if `CardsFarmer` is currently paused. CardsFarmer can be paused due to various events, mainly `!pause` and `!play` commands. Paused CardsFarmer will not attempt to farm anything in automatic mode, neither will check badges every `IdleFarmingPeriod` hours.

#### Game

`AppID` is `uint` type that in unique way identifies game being played. ASF enforces this to be greater than `0`.

`GameName` is `string` type that provides friendly name of game identified by `AppID`. This is data returned by Steam Community. ASF enforces this to be `non-null` and `non-empty`.

`HoursPlayed` is `float` type that provides information how many hours the game has been played. This property is not updated in real time, but on as-needed basis, at least once per `FarmingDelay` minutes. Please note that initially this data is retrieved from Steam Community, but then updated according to ASF built-in timers, therefore it might not match what Steam Community is returning - this is because Steam Community data is not provided in real time either, and ASF requires such data for stopping farming for hours game as soon as it reaches `2.0` value. ASF enforces this property to be at least `0.0`.

`CardsRemaining` is `ushort` type that tells how many cards are remaining for the game. This property is updated as soon as possible and it should always have a value greater than `0`. However, it is possible for this property to have `0` value for a short moment when ASF is switching game.

---

### GenericResponse

```
{
	"Message": "string",
	"Result": {},
	"Success": true
}
```

`Message` - `string` value providing extra details about the response. This could be simple "OK" when request succeeded, or actual failure reason if it didn't. We use this field as a general help for you to know what happened about the request you've sent. Keep in mind that this field is NOT a result of your request, only a description of what happened. Can be null if we don't have any specific message for you to retrieve.

`Result` - `object` value providing actual result of your request. The type of this field depends on API endpoint that you called - for example it can be a `BotResult` or a `string`. Most commonly used in `GET` requests for fetching actual data that you asked for. While type of this field is flexible, API guarantees that there can be only one fixed type per API endpoint, so you're always guaranteed parsable strong-typed output on per-endpoint basis. Can be null if we don't have any specific result for you to retrieve.

`Success` - `bool` value providing a simple way to check the result. This is offered as an extra to HTTP status codes, since `2xx` codes are considered `true`, while everything else is considered `false`. Please note that this property only indicates if **API request succeeded** - you should parse `Result` property for verifying if particular action was completed successfully, such as sending a command.