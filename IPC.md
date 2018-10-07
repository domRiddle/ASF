# IPC

ASF includes its own unique IPC interface that can be used for further interaction with the process. IPC stands for **[inter-process communication](https://en.wikipedia.org/wiki/Inter-process_communication)** and in the most simple definition this is "ASF web interface" that can be used for further integration with the process, both as a frontend for end-user (ASF-ui), and backend for third-party integrations (ASF API).

IPC can be used for a lot of different things, depending on your needs and skills. For example, you can use it for fetching status of ASF and all bots, sending ASF commands, fetching and editing global/bot configs, adding new bots, deleting existing bots, submitting keys for **[BGR](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer)** or accessing ASF's log file. All of those actions are exposed by our API, which means that you can code your own tools and scripts that will be able to communicate with ASF and influence it during runtime. In addition to that, selected actions (such as sending commands) are also implemented by our ASF-ui which allows you to easily access them through a friendly web interface.

---

# Usage

You can enable our IPC interface by enabling `IPC` **[global configuration property](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#global-config)**. ASF will state IPC launch in its log, which you can use for verifying if IPC interface has started properly:

```
INFO|ASF|Start() Starting IPC server...
INFO|ASF|Start() IPC server ready!
```

ASF's http server is now listening on selected endpoints. If you didn't provide a custom configuration file for IPC, those will be IPv4-based **[127.0.0.1](http://127.0.0.1:1242)** and IPv6-based **[[::1]](http://[::1]:1242)** on default `1242` port. You can access our IPC interface by above links, from the same machine as the one running ASF process. If ASF's IPC interface was enabled properly, you should see our **[ASF-ui](#asf-ui)** frontend in your browser.

![IPC GUI](https://i.imgur.com/vCu2ZY5.png)

---

# ASF-ui

ASF-ui is a community project that aims to create user-friendly graphical web interface for end-users. In order to achieve that, it acts as a frontend to our **[ASF API](#asf-api)**, allowing you to do various actions with ease. This is the default UI that ASF comes with.

As stated above, ASF-ui is a community project that isn't maintained by core ASF developers. It follows its own flow in **[ASF-ui repo](https://github.com/JustArchiNET/ASF-ui)** which should be used for all related questions, issues, bug reports and suggestions.

---

# ASF API

Our IPC interface is based on **[Kestrel HTTP server](https://github.com/aspnet/KestrelHttpServer)** and comes with **[RESTful](https://en.wikipedia.org/wiki/Representational_state_transfer)** web API that is based on JSON as its primary data format. We're doing our best to precisely describe response, using both HTTP status codes (where appropriate), as well as a response you can parse yourself in order to know whether the request succeeded, and if not, then why.

Our ASF API can be accessed by sending appropriate requests to appropriate endpoints. You can use those API endpoints to make your own helper scripts, tools, GUIs and alike. This is exactly what our ASF-ui achieves under the hood, and every other tool can achieve the same. ASF API is officially supported and maintained by core ASF team.

Communication with IPC server provided by ASF can be done by using any http-compatible program, tool or code. Some API endpoints might require from you to specify extra data, such as providing appropriate structure as a body of the request. In this case, in addition to providing required input, you must also set `Content-Type` header to appropriate value, such as `application/json` if your input is provided in JSON.

---

## HTTP status codes

Our API makes use of standard HTTP status codes, and we use them according to the RFC. In the most simplified explanation, our API will return `200 OK` status if your API call succeeded, and non-200 otherwise. This should be used as a primary way to detect whether your ASF API call succeeded or not.

ASF can use various HTTP status codes to explain better what happened. Some of them include:

- `200 OK` - the request has completed successfully.
- `400 BadRequest` - the request has failed because of an error, parse our response body for actual reason. Most of the time this is ASF, understanding the request, but refusing to execute it due to provided reason.
- `401 Unauthorized` - ASF has `IPCPassword` set, but you've failed to **[authenticate](#authentication)** properly.
- `403 Forbidden` - ASF has `IPCPassword` set and you've failed to **[authenticate](#authentication)** properly too many times, try again in an hour.
- `404 NotFound` - the URL that you're trying to reach does not exist. Confirm with our **[swagger frontend](#swagger-frontend)** below whether you're accessing appropriate endpoint.
- `405 MethodNotAllowed` - the HTTP method you're trying to use is not allowed for this API endpoint. This can be caused e.g. by sending `GET` request where ASF expects `POST`, or vice-versa. This code can also be used when trying to access websocket endpoint without initiating a websocket connection (upgrade).
- `406 NotAcceptable` - your `Content-Type` header is not acceptable for this API endpoint. This is mainly used in requests that require from you a specific body as an input, and you didn't declare in what format it's provided.
- `411 LengthRequired` - your `POST` request is missing `Content-Length` header. Length must be defined even in requests that do not have a body (use length of 0 in this case).
- `500 InternalServerError` - IPC server ran into fatal condition, this indicates ASF issue that should be reported and corrected. We do not normally use this status anywhere in the code, please let us know.
- `503 ServiceUnavailable` - ASF ran into one of possible exceptions during execution of this request. If not explained in the response, ASF log should be able to tell more.

---

## Authentication

ASF IPC interface by default does not require any sort of authentication, as `IPCPassword` is set to `null`. However, if `IPCPassword` is enabled by being set to any non-empty value, every call to ASF's API requires the password that matches set `IPCPassword`. If you omit authentication or input wrong password, you'll get `401 - Unauthorized` error. If you continue sending requests without authentication, eventually you'll get temporarily blocked with `403 - Forbidden` error.

Authentication can be done through two separate ways.

### `Authentication` header

In general you should use HTTP request headers, by setting `Authentication` field with your password as a value. The way of doing that depends on the actual tool you're using for accessing ASF's IPC interface, for example if you're using `curl` then you should add `-H 'Authentication: MyPassword'` as a parameter. This way authentication is passed in the headers of the request, where it in fact should take place.

### `password` parameter in query string

Alternatively you can append `password` parameter to the end of the URL you're about to call, for example by calling `/Api/ASF?password=MyPassword` instead of `/Api/ASF` alone. This approach is good enough, but obviously it exposes password in the open, which is not necessarily always appropriate. In addition to that it's extra argument in the query string, which complicates the look of the URL and makes it feel like it's URL-specific, while password applies to entire ASF API communication.

---

Both ways are supported and it's totally up to you which one you want to choose. We recommend to use HTTP headers everywhere where you can, as usage-wise it's more appropriate than query string. However, we support query string as well, mainly because of various limitations related to request headers. A good example includes lack of custom headers while initiating a websocket connection in javascript (even though it's completely valid according to the RFC). In this situation query string is the only way to authenticate.

---

## Swagger frontend

TODO

---

## FAQ

### Is ASF's IPC interface secure and safe to use?

ASF by default listens only on `localhost` addresses, which means that accessing ASF IPC from any other machine but your own **is impossible**. Unless you modify default endpoints, attacker would need a direct access to your own machine in order to access ASF's IPC, therefore it's as secure as it can be and there is no possibility of anybody else accessing it, even from your own LAN.

However, if you decide to change default `localhost` bind addresses to something else, then you're supposed to set proper firewall rules **yourself** in order to allow only authorized IPs to access ASF's IPC interface. In addition to doing that, we strongly recommend to set up `IPCPassword`, that will add another layer of extra security. You might also want to run ASF's IPC interface behind a reverse proxy in this case, which is further explained below.

### Can I use ASF's IPC behind a reverse proxy such as Apache or Nginx?

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

Unless you truly need to specify a custom base path, it's best to leave it at default. In addition to that, custom base path is **[not supported by our IPC GUI yet](https://github.com/JustArchiNET/ArchiSteamFarm/issues/869#issuecomment-419596294)**. Our IPC API is fully compatible with it already.