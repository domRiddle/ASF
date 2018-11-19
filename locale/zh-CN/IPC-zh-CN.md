# IPC

ASF includes its own unique IPC interface that can be used for further interaction with the process. IPC stands for **[inter-process communication](https://en.wikipedia.org/wiki/Inter-process_communication)** and in the most simple definition this is "ASF web interface" based on **[Kestrel HTTP server](https://github.com/aspnet/KestrelHttpServer)** that can be used for further integration with the process, both as a frontend for end-user (ASF-ui), and backend for third-party integrations (ASF API).

IPC can be used for a lot of different things, depending on your needs and skills. 例如，您可以用它获取 ASF 和所有机器人的状态、向 ASF 发送命令、获取和修改全局/机器人配置、添加新机器人、删除机器人、向 **[BGR](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer-zh-CN)**（后台游戏激活器）导入游戏序列号或者访问 ASF 的日志文件。 All of those actions are exposed by our API, which means that you can code your own tools and scripts that will be able to communicate with ASF and influence it during runtime. In addition to that, selected actions (such as sending commands) are also implemented by our ASF-ui which allows you to easily access them through a friendly web interface.

* * *

# 用法

您可以编辑 `IPC` **[全局配置属性](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-CN#全局配置)**启用 IPC 接口。 ASF will state IPC launch in its log, which you can use for verifying if IPC interface has started properly:

    INFO|ASF|Start() Starting IPC server...
    INFO|ASF|Start() IPC server ready!
    

ASF's http server is now listening on selected endpoints. If you didn't provide a custom configuration file for IPC, those will be IPv4-based **[127.0.0.1](http://127.0.0.1:1242)** and IPv6-based **[[::1]](http://[::1]:1242)** on default `1242` port. You can access our IPC interface by above links, from the same machine as the one running ASF process.

ASF's IPC interface exposes three different ways to access it, depending on your planned usage.

最底层的方式是 **[ASF API](#asf-api)**，这是我们 IPC 接口的核心功能，允许被其他任何方式调用。 This is what you want to use in your own tools, utilities and projects in order to communicate with ASF directly.

中层的使用方式是 **[Swagger 文档](#swagger-文档)**，它是 ASF API 的直接前端。 It features a complete documentation of ASF API and also allows you to access it more easily. This is what you want to check if you're planning on writing a tool, utility or other projects that are supposed to communicate with ASF through its API.

**[ASF-ui](#asf-ui)** 是最高层的使用方式，它基于 ASF API，为用户提供了一种简单的方式进行各种 ASF 的操作。 This is our default IPC interface designed for end-users, and a perfect example of what you can build with ASF API. 如果您愿意，可以使用您的自定义 WebUI，即指定 `--path` **[命令行参数](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments-zh-CN#参数)**选择从指定位置的 `www` 目录启用 Web 服务。

* * *

# ASF-ui

ASF-ui is a community project that aims to create user-friendly graphical web interface for end-users. 为了达成这一目的，它被设计为 **[ASF API](#asf-api)** 的前端，使您可以轻松操作 ASF。 This is the default UI that ASF comes with.

As stated above, ASF-ui is a community project that isn't maintained by core ASF developers. It follows its own flow in **[ASF-ui repo](https://github.com/JustArchiNET/ASF-ui)** which should be used for all related questions, issues, bug reports and suggestions.

![ASF-ui](https://i.imgur.com/vCu2ZY5.png)

* * *

# ASF API

Our ASF API is typical **[RESTful](https://en.wikipedia.org/wiki/Representational_state_transfer)** web API that is based on JSON as its primary data format. We're doing our best to precisely describe response, using both HTTP status codes (where appropriate), as well as a response you can parse yourself in order to know whether the request succeeded, and if not, then why.

Our ASF API can be accessed by sending appropriate requests to appropriate `/Api` endpoints. You can use those API endpoints to make your own helper scripts, tools, GUIs and alike. This is exactly what our ASF-ui achieves under the hood, and every other tool can achieve the same. ASF API is officially supported and maintained by core ASF team.

请阅读我们的 **[Swagger 文档](#swagger-文档)**，获取关于 ASF API 端点、描述、请求、响应、HTTP 状态代码等内容的完整文档。

![ASF API](https://i.imgur.com/yggjf5v.png)

* * *

## 身份认证

ASF IPC interface by default does not require any sort of authentication, as `IPCPassword` is set to `null`. However, if `IPCPassword` is enabled by being set to any non-empty value, every call to ASF's API requires the password that matches set `IPCPassword`. If you omit authentication or input wrong password, you'll get `401 - Unauthorized` error. If you continue sending requests without authentication, eventually you'll get temporarily blocked with `403 - Forbidden` error.

Authentication can be done through two separate ways.

### `Authentication` 请求头

In general you should use HTTP request headers, by setting `Authentication` field with your password as a value. The way of doing that depends on the actual tool you're using for accessing ASF's IPC interface, for example if you're using `curl` then you should add `-H 'Authentication: MyPassword'` as a parameter. This way authentication is passed in the headers of the request, where it in fact should take place.

### `password` 请求参数

Alternatively you can append `password` parameter to the end of the URL you're about to call, for example by calling `/Api/ASF?password=MyPassword` instead of `/Api/ASF` alone. This approach is good enough, but obviously it exposes password in the open, which is not necessarily always appropriate. In addition to that it's extra argument in the query string, which complicates the look of the URL and makes it feel like it's URL-specific, while password applies to entire ASF API communication.

* * *

Both ways are supported and it's totally up to you which one you want to choose. We recommend to use HTTP headers everywhere where you can, as usage-wise it's more appropriate than query string. However, we support query string as well, mainly because of various limitations related to request headers. A good example includes lack of custom headers while initiating a websocket connection in javascript (even though it's completely valid according to the RFC). In this situation query string is the only way to authenticate.

* * *

## Swagger 文档

Our IPC interface, in additon to ASF API and ASF-ui also includes swagger documentation, which is available under `/swagger` **[URL](http://127.0.0.1:1242/swagger)**. Swagger documentation serves as a middle-man between our API implementation and other tools using it (e.g. ASF-ui). It provides a complete documentation and availability of all API endpoints in **[OpenAPI](https://swagger.io/resources/open-api)** specification that can be easily consumed by other projects, allowing you to write and test ASF API with ease.

Apart from using our swagger documentation as a complete specification of ASF API, you can also use it as user-friendly way to execute various API endpoints, mainly those that are not implemented by ASF-ui. Since our swagger documentation is generated automatically from ASF code, you have a guarantee that the documentation will always be up-to-date with the features that ASF exposes.

![Swagger 文档](https://i.imgur.com/mLpd5e4.png)

* * *

# 常见问题

### ASF 的 IPC 接口安全吗？

ASF by default listens only on `localhost` addresses, which means that accessing ASF IPC from any other machine but your own **is impossible**. Unless you modify default endpoints, attacker would need a direct access to your own machine in order to access ASF's IPC, therefore it's as secure as it can be and there is no possibility of anybody else accessing it, even from your own LAN.

However, if you decide to change default `localhost` bind addresses to something else, then you're supposed to set proper firewall rules **yourself** in order to allow only authorized IPs to access ASF's IPC interface. In addition to doing that, we strongly recommend to set up `IPCPassword`, that will add another layer of extra security. You might also want to run ASF's IPC interface behind a reverse proxy in this case, which is further explained below.

### 我可以在我的工具或者用户脚本中访问 ASF API 吗？

Yes, this is what ASF API was designed for and you can use anything capable of sending a HTTP request to access it. Local userscripts follow **[CORS](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing)** logic, and we allow access from all origins (`*`) as long as `IPCPassword` is set, as an extra security measure. This allows you to execute various authenticated ASF API requests, without allowing potentially malicious scripts to do that automatically (as they'd need to know your `IPCPassword` to do that).

### 我可以远程访问 ASF 的 IPC 吗，例如从另一台电脑访问？

Yes, we recommend to use a reverse proxy for that (explained below). This way you can access your web server in typical way, which will then access ASF's IPC on the same machine. 或者，如果您不打算运行反向代理，您可以将**[自定义配置](#自定义配置)**中的 URL 修改为合适的地址，例如 `http://*:1242`。

### 我可以将 ASF 的 IPC 部署在 Apache 或者 Nginx 的反向代理后吗？

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

### 我可以通过 HTTPS 协议访问 IPC 吗？

**Yes**, you can achieve it through two different ways. A recommended way would be to use a reverse proxy for that (described above) where you can access your web server through https like usual, and connect through it with ASF's IPC interface on the same machine. This way your traffic is fully encrypted and you don't need to modify IPC in any way to support such setup.

另一种方法是为 ASF IPC 指定**[自定义配置](#自定义配置)**，直接为我们使用的 Kestrel HTTP 服务器启用 HTTPS 端点，并且提供合适的 SSL 证书。 This way is recommended if you're not running any other web server and don't want to run one exclusively for ASF. Otherwise, it's much easier to achieve a satisfying setup by using a reverse proxy mechanism.

* * *

## 自定义配置

Our IPC interface supports extra config file, `IPC.config` that should be put in standard ASF's `config` directory.

When available, this file specifies advanced configuration of ASF's Kestrel http server, together with other IPC-related tuning. Unless you have a particular need, there is no reason for you to use this file, as ASF is already using sensible defaults in this case.

The configuration file is based on following JSON structure:

```json
{
    "Kestrel": {
        "Endpoints": {
            "example-http4": {
                "Url": "http://127.0.0.1:1242"
            },
            "example-http6": {
                "Url": "http://[::1]:1242"
            },
            "example-https4": {
                "Url": "https://127.0.0.1:1242",
                "Certificate": {
                    "Path": "/path/to/certificate.pfx",
                    "Password": "passwordToPfxFileAbove"
                }
            },
            "example-https6": {
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

`Host` accepts a variety of values, including `*` value that binds ASF's http server to all available interfaces. Be extremely careful when you use `Host` values that allow remote access. Doing so will enable access to ASF's IPC interface from other machines, which might pose a security risk. We strongly recommend to use `IPCPassword` (and preferably your own firewall too) at a minimum in this case.

`PathBase` - This is base path that will be used by IPC interface. This property is optional, defaults to `/` and shouldn't be required to modify for majority of use cases. By changing this property you'll host entire IPC interface on a custom prefix, for example `http://127.0.0.1:1242/MyPrefix` instead of `http://127.0.0.1:1242` alone. Using custom `PathBase` might be wanted in combination with specific setup of a reverse proxy where you'd like to proxy a specific URL only, for example `mydomain.com/ASF` instead of entire `mydomain.com` domain. Normally that would require from you to write a rewrite rule for your web server that would map `mydomain.com/ASF/Api/X` -> `127.0.0.1:1242/Api/X`, but instead you might define custom `PathBase` of `/ASF` and achieve easier setup of `mydomain.com/ASF/Api/X` -> `127.0.0.1:1242/ASF/Api/X`.

Unless you truly need to specify a custom base path, it's best to leave it at default.

### 示例配置

```json
{
    "Kestrel": {
        "Endpoints": {
            "HTTP": {
                "Url": "http://*:1242"
            }
        }
    }
}
```