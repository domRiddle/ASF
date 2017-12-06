# IPC

Starting with version 3.0, ASF offers http-based inter-process communication that can be used to communicate with the process. This is offered as an alternative to already existing steam chat communication.

IPC is always executed with `SteamOwnerID` permissions, which is `0` by default. In order to use it, you should set `SteamOwnerID` to the proper non-zero value. Default value will make IPC work, but not authorizing any client to execute any command. For more info about `SteamOwnerID`, visit **[configuration](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration)**.

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

Currently ASF IPC offers minimalistic API for bots management that can be accessed by appropriate endpoints. In the future perhaps we'll succeed in making fully-featured IPC GUI that will access that API in user-friendly way (**[#610](https://github.com/JustArchi/ArchiSteamFarm/issues/610)**), but until then you'll need to access those endpoints manually.

---

## HTTP status codes

Our API makes use of following HTTP status codes:

- `200 OK` - the request completed successfully.
- `400 BadRequest` - the request failed because of an error, parse response body for actual reason.
- `401 Unauthorized` - ASF has `IPCPassword` set and you failed to **[authenticate](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC#authentication)** properly.
- `404 NotFound` - the URL you're trying to reach does not exist.
- `405 NotAllowed` - the HTTP method you're trying to use is not allowed for this API endpoint.
- `406 NotAcceptable` - your `ContentType` header is not acceptable for this endpoint.
- `501 NotImplemented` - this URL is reserved for future use, not implemented yet.
- `503 ServiceUnavailable` - ASF doesn't have `SteamOwnerID` properly set, command access is prohibited.

---

## API

In general our API is a typical REST API that is based on JSON as a primary way of serializing/deserializing data. We're doing our best to precisely describe response, using both HTTP error codes (where appropriate), as well as JSON response you can parse yourself in order to know whether the request suceeded, and if not, then why.

---

## Screenshots

TODO

---

## Authentication

ASF IPC interface by default does not require any sort of authentication, as `IPCPassword` is set to `null`. However, if `IPCPassword` is enabled by being set to any non-empty value, every call to ASF IPC interface requires the password that matches set `IPCPassword`. If you omit authentication or input wrong password, you'll get `401 - Unauthorized` error.

Authentication can be done through two generally-acceptable ways.

### `password` parameter in query string

You can append `password` parameter to the end of the URL you're about to call, for example by calling `/API/Command/version?password=MyPassword` instead of `/API/Command/version` alone. This approach is good enough for majority of use cases, as it's user-friendly and can be even saved as a bookmark, but obviously it exposes password in the open, which is not necessarily appropriate for all cases.

### `Authentication` header

Alternatively you can use HTTP request headers, by setting `Authentication` field with your password as a value. The way of doing that depends on the actual tool you'll use for accessing ASF's IPC interface, for example if you're using `curl` then you should add `-H 'Authentication: MyPassword` as a parameter.

---

Both ways are supported in exactly the same way and it's totally up to you which one you want to choose. We recommend query string for users that just want to access protected ASF IPC interface or save link as a bookmark, and we recommend http header for all tools, code, scripts and otherwise dev-related things where you have more freedom in terms of http headers and actual communication.

---

## Cross-Origin Resource Sharing

ASF by default has `Access-Control-Allow-Origin` header set to `*`. This allows e.g. JavaScript scripts  to access ASF IPC interface in third-party web GUIs or tools. However, this also means that somebody could potentially upload malicious script that would make calls to ASF without your awareness or approval. If you'd like to ensure that such situation won't happen, consider setting up `IPCPassword` appropriately. This way if any script wants to access ASF's IPC interface, it'll need to authenticate each request, as described above.

---

## FAQ

**Q:** Why should I consider using IPC in ASF?

**A:** You should consider using IPC in ASF only if you have a strong reason. If you don't know what it is about, most likely you don't have one, and you can safely ignore this page along with the content. IPC stands for inter-process communication, which allows you to control the ASF process during execution, e.g. through a PHP script or similar. By using IPC you're able to control how ASF behaves, which might be useful if you're planning on integrating it further. If you're not running ASF on the server, and you're not planning on integrating it further with your own scripts/applications, it should be easier for you to communicate with ASF through steam chat with one of the bots. However, you can use IPC too, if you consider it useful/easier for you.

---

**Q:** Is this secure?

**A:** ASF by default listens only on `127.0.0.1` address, which means that accessing ASF IPC from any other machine but your own is impossible. Therefore, it's as secure as IPC can be. If you decide to change default `127.0.0.1` bind address to something else, such as `*`, then you're supposed to set proper firewall rules **yourself** in order to allow only authorized IPs to access ASF port. In addition to that, server must include properly set non-zero `SteamOwnerID`, otherwise it'll refuse to execute any command, as an extra security measure. On top of all of that, you can also set `IPCPassword`, which would add another layer of extra security.