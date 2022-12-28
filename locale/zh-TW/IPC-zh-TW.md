# IPC

ASF含有自己獨特的IPC介面，可以用來與程序進一步交互。 IPC全名為&#8203;**[行程間通訊](https://zh.wikipedia.org/zh-tw/行程間通訊)**&#8203;，在最簡單的定義中，這是基於&#8203;**[Kestrel HTTP伺服器](https://github.com/aspnet/KestrelHttpServer)**&#8203;的「ASF Web界面」，可用於與程序間的進一步整合，既作為終端使用者的前端（ASF-ui）， 也做為第三方整合的後端（ASF API）。

IPC可以用來做很多不同的事情，這取決於您的需求與能力。 舉例來說，您可以使用它來提取ASF與所有Bot的狀態、傳送ASF指令、提取或編輯全域／特定Bot設定、新增新的Bot、刪除現存的Bot、提交&#8203;**[背景序號啟動器](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer-zh-TW)**&#8203;序號，或存取ASF的紀錄檔。 所有這些操作都由我們的API公開，這代表您可以編寫自己的工具及腳本與ASF通訊，並在執行期間產生影響。 除此之外，我們的ASF-ui也實現了特定的操作（例如傳送指令），使您可以透過友善的Web界面輕鬆使用它們。

---

# 使用方法

除非您透過&#8203;`IPC`&#8203;**[全域設定屬性](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-TW#全域設定檔)**&#8203;手動停用IPC，否則預設情形下，它是啟用的。 ASF會在紀錄中說明IPC的啟動，您可以以此來驗證IPC介面是否已正常啟動：

```text
INFO|ASF|Start() 正在啟動 IPC 伺服器…
INFO|ASF|Start() IPC 伺服器已就緒！
```

ASF的http伺服器現在已在監聽指定的端點。 若您沒有為IPC提供自訂設定檔，預設是使用基於IPv4的&#8203;**[127.0.0.1](http://127.0.0.1:1242)**&#8203;及基於IPv6的&#8203;**[[::1]](http://[::1]:1242)**&#8203; &#8203;`1242`&#8203;連接埠。 在執行ASF程序的同一台設備上，您可以透過上述連結存取我們的IPC介面。

依據您的需求，ASF的IPC介面提供了三種不同的存取方法。

最低階的方式是&#8203;**[ASF API](#asf-api)**&#8203;，這是我們IPC介面的核心，允許其他所有操作。 這能在您自己的工具、實用程式及專案中使用，可以與ASF直接進行通訊。

中階的方式是使用我們的&#8203;**[Swagger文件](#swagger-文件)**&#8203;&#8203;作為ASF API的前端。 它具有完整的ASF API文件，並使您能夠更輕鬆地存取它。 This is what you want to check if you're planning on writing a tool, utility or other projects that are supposed to communicate with ASF through its API.

On the highest level there is **[ASF-ui](#asf-ui)** which is based on our ASF API and provides user-friendly way to execute various ASF actions. This is our default IPC interface designed for end-users, and a perfect example of what you can build with ASF API. If you'd like, you can use your own custom web UI to use with ASF, by specifying `--path` **[command-line argument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments#arguments)** and using custom `www` directory located there.

---

# ASF-ui

ASF-ui is a community project that aims to create user-friendly graphical web interface for end-users. In order to achieve that, it acts as a frontend to our **[ASF API](#asf-api)**, allowing you to do various actions with ease. This is the default UI that ASF comes with.

As stated above, ASF-ui is a community project that isn't maintained by core ASF developers. It follows its own flow in **[ASF-ui repo](https://github.com/JustArchiNET/ASF-ui)** which should be used for all related questions, issues, bug reports and suggestions.

You can use ASF-ui for general management of ASF process. It allows for example to manage bots, modify settings, send commands, and achieve selected other functionality normally available through ASF.

![ASF-ui](https://raw.githubusercontent.com/JustArchiNET/ASF-ui/main/.github/previews/bots.png)

---

# ASF API

Our ASF API is typical **[RESTful](https://en.wikipedia.org/wiki/Representational_state_transfer)** web API that is based on JSON as its primary data format. We're doing our best to precisely describe response, using both HTTP status codes (where appropriate), as well as a response you can parse yourself in order to know whether the request succeeded, and if not, then why.

Our ASF API can be accessed by sending appropriate requests to appropriate `/Api` endpoints. You can use those API endpoints to make your own helper scripts, tools, GUIs and alike. This is exactly what our ASF-ui achieves under the hood, and every other tool can achieve the same. ASF API is officially supported and maintained by core ASF team.

For complete documentation of available endpoints, descriptions, requests, responses, http status codes and everything else considering ASF API, please refer to our **[swagger documentation](#swagger-documentation)**.

![ASF API](https://i.imgur.com/yggjf5v.png)

---

# 自訂組態

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
        "KnownNetworks": [
            "10.0.0.0/8",
            "172.16.0.0/12",
            "192.168.0.0/16"
        ],
        "PathBase": "/"
    }
}
```

`Endpoints` - This is a collection of endpoints, each endpoint having its own unique name (like `example-http4`) and `Url` property that specifies `Protocol://Host:Port` listening address. By default, ASF listens on IPv4 and IPv6 http addresses, but we've added https examples for you to use, if needed. You should declare only those endpoints that you need, we've included 4 example ones above so you can edit them easier.

`Host` accepts either `localhost`, a fixed IP address of the interface it should listen on (IPv4/IPv6), or `*` value that binds ASF's http server to all available interfaces. Using other values like `mydomain.com` or `192.168.0.*` acts the same as `*`, there is no IP filtering implemented, therefore be extremely careful when you use `Host` values that allow remote access. Doing so will enable access to ASF's IPC interface from other machines, which may pose a security risk. We strongly recommend to use `IPCPassword` (and preferably your own firewall too) **at a minimum** in this case.

`KnownNetworks` - This **optional** variable specifies network addresses which we consider trustworthy. By default, ASF is configured to trust loopback interface (`localhost`, same machine) **only**. This property is used in two ways. Firstly, if you omit `IPCPassword`, then we'll allow only machines from known networks to access ASF's API, and deny everybody else as a security measure. Secondly, this property is crucial in regards to reverse-proxies accessing ASF, as ASF will honor its headers only if the reverse-proxy server is from within known networks. Honoring the headers is crucial in regards to ASF's anti-bruteforce mechanism, as instead of banning the reverse-proxy in case of a problem, it'll ban the IP specified by the reverse-proxy as the source of the original message. Be extremely careful with the networks you specify here, as it allows a potential IP spoofing attack and unauthorized access in case the trusted machine is compromised or wrongly configured.

`PathBase` - This is **optional** base path that will be used by IPC interface. Defaults to `/` and shouldn't be required to modify for majority of use cases. By changing this property you'll host entire IPC interface on a custom prefix, for example `http://localhost:1242/MyPrefix` instead of `http://localhost:1242` alone. Using custom `PathBase` may be wanted in combination with specific setup of a reverse proxy where you'd like to proxy a specific URL only, for example `mydomain.com/ASF` instead of entire `mydomain.com` domain. Normally that would require from you to write a rewrite rule for your web server that would map `mydomain.com/ASF/Api/X` -> `localhost:1242/Api/X`, but instead you can define a custom `PathBase` of `/ASF` and achieve easier setup of `mydomain.com/ASF/Api/X` -> `localhost:1242/ASF/Api/X`.

Unless you truly need to specify a custom base path, it's best to leave it at default.

## 設定範例

### 更改預設連接埠

The following config simply changes default ASF listening port from `1242` to `1337`. You can pick any port you like, but we recommend `1024-32767` range, as other ports are typically **[registered](https://en.wikipedia.org/wiki/Registered_port)**, and may for example require `root` access on Linux.

```json
{
    "Kestrel": {
        "Endpoints": {
            "HTTP4": {
                "Url": "http://127.0.0.1:1337"
            },
            "HTTP6": {
                "Url": "http://[::1]:1337"
            }
        }
    }
}
```

---

### 允許所有 IP 存取

The following config will allow remote access from all sources, therefore you should **ensure that you read and understood our security notice about that**, available above.

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

If you do not require access from all sources, but for example your LAN only, then it's much better idea to check local IP address of the machine hosting ASF, for example `192.168.0.10` and use it instead of `*` in example config above.

---

# 身分驗證

ASF IPC interface by default does not require any sort of authentication, as `IPCPassword` is set to `null`. However, if `IPCPassword` is enabled by being set to any non-empty value, every call to ASF's API requires the password that matches set `IPCPassword`. If you omit authentication or input wrong password, you'll get `401 - Unauthorized` error. After 5 failed authentication attempts (wrong password), you'll get temporarily blocked with `403 - Forbidden` error.

Authentication can be done through two separate ways.

## `Authentication` 標頭

In general you should use HTTP request headers, by setting `Authentication` field with your password as a value. The way of doing that depends on the actual tool you're using for accessing ASF's IPC interface, for example if you're using `curl` then you should add `-H 'Authentication: MyPassword'` as a parameter. This way authentication is passed in the headers of the request, where it in fact should take place.

## Query 字串中的 `password` 參數

Alternatively you can append `password` parameter to the end of the URL you're about to call, for example by calling `/Api/ASF?password=MyPassword` instead of `/Api/ASF` alone. This approach is good enough, but obviously it exposes password in the open, which is not necessarily always appropriate. In addition to that it's extra argument in the query string, which complicates the look of the URL and makes it feel like it's URL-specific, while password applies to entire ASF API communication.

---

Both ways are supported and it's totally up to you which one you want to choose. We recommend to use HTTP headers everywhere where you can, as usage-wise it's more appropriate than query string. However, we support query string as well, mainly because of various limitations related to request headers. A good example includes lack of custom headers while initiating a websocket connection in javascript (even though it's completely valid according to the RFC). In this situation query string is the only way to authenticate.

---

# Swagger 文件

Our IPC interface, in additon to ASF API and ASF-ui also includes swagger documentation, which is available under `/swagger` **[URL](http://localhost:1242/swagger)**. Swagger documentation serves as a middle-man between our API implementation and other tools using it (e.g. ASF-ui). It provides a complete documentation and availability of all API endpoints in **[OpenAPI](https://swagger.io/resources/open-api)** specification that can be easily consumed by other projects, allowing you to write and test ASF API with ease.

Apart from using our swagger documentation as a complete specification of ASF API, you can also use it as user-friendly way to execute various API endpoints, mainly those that are not implemented by ASF-ui. Since our swagger documentation is generated automatically from ASF code, you have a guarantee that the documentation will always be up-to-date with the API endpoints that your version of ASF includes.

![Swagger 文件](https://i.imgur.com/mLpd5e4.png)

---

# 常見問題

### ASF 的 IPC 介面安全嗎？

ASF by default listens only on `localhost` addresses, which means that accessing ASF IPC from any other machine but your own **is impossible**. Unless you modify default endpoints, attacker would need a direct access to your own machine in order to access ASF's IPC, therefore it's as secure as it can be and there is no possibility of anybody else accessing it, even from your own LAN.

However, if you decide to change default `localhost` bind addresses to something else, then you're supposed to set proper firewall rules **yourself** in order to allow only authorized IPs to access ASF's IPC interface. In addition to doing that, you will need to set up `IPCPassword`, as ASF will refuse to let other machines access ASF API without one, which adds another layer of extra security. You may also want to run ASF's IPC interface behind a reverse proxy in this case, which is further explained below.

### 我可以使用自己的工具或腳本存取 ASF API 嗎？

Yes, this is what ASF API was designed for and you can use anything capable of sending a HTTP request to access it. Local userscripts follow **[CORS](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing)** logic, and we allow access from all origins for them (`*`), as long as `IPCPassword` is set, as an extra security measure. This allows you to execute various authenticated ASF API requests, without allowing potentially malicious scripts to do that automatically (as they'd need to know your `IPCPassword` to do that).

### 我可以遠端存取 ASF 的 IPC 嗎？例如從另一台設備上？

Yes, we recommend to use a reverse proxy for that. This way you can access your web server in typical way, which will then access ASF's IPC on the same machine. Alternatively, if you don't want to run with a reverse proxy, you can use **[custom configuration](#custom-configuration)** with appropriate URL for that. For example, if your machine is in a VPN with `10.8.0.1` address, then you can set `http://10.8.0.1:1242` listening URL in IPC config, which would enable IPC access from within your private VPN, but not from anywhere else.

### 我可以在 Apache 或 Nginx 等反向代理後使用 ASF 的 IPC 嗎？

**Yes**, our IPC is fully compatible with such setup, so you're free to host it also in front of your own tools for extra security and compatibility, if you'd like to. In general ASF's Kestrel http server is very secure and possesses no risk when being connected directly to the internet, but putting it behind a reverse-proxy such as Apache or Nginx could provide extra functionality that wouldn't be possible to achieve otherwise, such as securing ASF's interface with a **[basic auth](https://en.wikipedia.org/wiki/Basic_access_authentication)**.

Example Nginx configuration can be found below. We've included full `server` block, although you're interested mainly in `location` ones. Please refer to **[nginx documentation](https://nginx.org/en/docs)** if you need further explanation.

```nginx
server {
    listen *:443 ssl;
    server_name asf.mydomain.com;
    ssl_certificate /path/to/your/certificate.crt;
    ssl_certificate_key /path/to/your/certificate.key;

    location ~* /Api/NLog {
        proxy_pass http://127.0.0.1:1242;

        # Only if you need to override default host
#       proxy_set_header Host 127.0.0.1;

        # X-headers should always be specified when proxying requests to ASF
        # They're crucial for proper identification of original IP, allowing ASF to e.g. ban the actual offenders instead of your nginx server
        # Specifying them allows ASF to properly resolve IP addresses of users making requests - making nginx work as a reverse proxy
        # Not specifying them will cause ASF to treat your nginx as the client - nginx will act as a traditional proxy in this case
        # If you're unable to host nginx service on the same machine as ASF, you most likely want to set KnownNetworks appropriately in addition to those
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

        # Only if you need to override default host
#       proxy_set_header Host 127.0.0.1;

        # X-headers should always be specified when proxying requests to ASF
        # They're crucial for proper identification of original IP, allowing ASF to e.g. ban the actual offenders instead of your nginx server
        # Specifying them allows ASF to properly resolve IP addresses of users making requests - making nginx work as a reverse proxy
        # Not specifying them will cause ASF to treat your nginx as the client - nginx will act as a traditional proxy in this case
        # If you're unable to host nginx service on the same machine as ASF, you most likely want to set KnownNetworks appropriately in addition to those
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Host $host:$server_port;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header X-Forwarded-Server $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

Example Apache configuration can be found below. Please refer to **[apache documentation](https://httpd.apache.org/docs)** if you need further explanation.

```apache
<IfModule mod_ssl.c>
    <VirtualHost *:443>
        ServerName asf.mydomain.com

        SSLEngine On
        SSLCertificateFile /path/to/your/fullchain.pem
        SSLCertificateKeyFile /path/to/your/privkey.pem

        # TODO: Apache can't do case-insensitive matching properly, so we hardcode two most commonly used cases
        ProxyPass "/api/nlog" "ws://127.0.0.1:1242/api/nlog"
        ProxyPass "/Api/NLog" "ws://127.0.0.1:1242/Api/NLog"

        ProxyPass "/" "http://127.0.0.1:1242/"
    </VirtualHost>
</IfModule>
```

### 我可以透過 HTTPS 協定存取 IPC 介面嗎？

**Yes**, you can achieve it through two different ways. A recommended way would be to use a reverse proxy for that, where you can access your web server through https like usual, and connect through it with ASF's IPC interface on the same machine. This way your traffic is fully encrypted and you don't need to modify IPC in any way to support such setup.

Second way includes specifying a **[custom config](#custom-configuration)** for ASF's IPC interface where you can enable https endpoint and provide appropriate certificate directly to our Kestrel http server. This way is recommended if you're not running any other web server and don't want to run one exclusively for ASF. Otherwise, it's much easier to achieve a satisfying setup by using a reverse proxy mechanism.

---

### During startup of IPC I'm getting an error: `System.IO.IOException: Failed to bind to address, An attempt was made to access a socket in a way forbidden by its access permissions`

This error indicates that something else on your machine is either already using that port, or reserved it for future use. This could be you if you're attempting to run second ASF instance on the same machine, but most often that's Windows excluding port `1242` from your usage, therefore you'll have to move ASF to another port. In order to do that, follow **[example config](#changing-default-port)** above, and simply try to pick another port, such as `12420`.

Of course you could also try to find out what is blocking port `1242` from ASF usage, and remove that, but that's usually far more troublesome than simply instructing ASF to use another port, so we'll skip elaborating further on that here.

---

### 為什麼我在不使用 `IPCPassword` 時，收到 `403 Forbidden` 錯誤？

Starting with ASF V5.1.2.1, we've added additional security measure that, by default, allows only loopback interface (`localhost`, your own machine) to access ASF API without `IPCPassword` set in the config. This is because using `IPCPassword` should be a **minimum** security measure set by everybody who decides to expose ASF interface further. You're still able to override this decision by specifying the networks which you trust to reach ASF without `IPCPassword` specified, you can set those in `KnownNetworks` property in custom config. However, unless you **really** know what you're doing and fully understand the risks, you should instead use `IPCPassword` as declaring `KnownNetworks` will allow everybody from those networks to access ASF API unconditionally.