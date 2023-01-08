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

中階的方式是使用我們的&#8203;**[Swagger文件](#swagger-文件)**&#8203;&#8203;作為ASF API的前端。 它具有完整的ASF API文件，並使您能夠更輕鬆地存取它。 若您計畫開發使用API與ASF通訊的工具、實用程式或專案，就需要查閱它。

在最高階，&#8203;**[ASF-ui](#asf-ui)**&#8203;基於我們的ASF API，並提供使用者友善的方式來執行各種ASF操作。 這是我們為最終使用者設計的預設IPC介面，也是您可以使用ASF API建置的完美範例。 若您願意，您可以與您自己的自訂Web UI一同使用，方法是指定&#8203;`--path`&#8203;**[命令列引數](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments-zh-TW#引數)**&#8203;，使用該處的自訂&#8203;`www`&#8203;資料夾。

---

# ASF-ui

ASF-ui是一個社群專案，旨在為最終使用者建立使用者友善的圖形Web介面。 為了實現這一點，它被作為我們&#8203;**[ASF API](#asf-api)**&#8203;的前端，使您能夠輕鬆執行各種操作。 這是ASF自帶的預設UI。

如上所述，ASF-ui是一個社群專案，並非由ASF核心開發人員所維護。 它在&#8203;**[ASF-ui儲存庫](https://github.com/JustArchiNET/ASF-ui)**&#8203;中遵循自身的流程，適用於所有的相關問題、討論、錯誤回報及建議。

您可以使用ASF-ui對ASF程序進行一般管理。 舉例來說，它使您能夠管理Bot、修改設定、傳送指令，及使用ASF提供的其他各種功能。

![ASF-ui](https://raw.githubusercontent.com/JustArchiNET/ASF-ui/main/.github/previews/bots.png)

---

# ASF API

我們的ASF API是典型的&#8203;**[RESTful](https://zh.wikipedia.org/zh-tw/表现层状态转换)**&#8203; Web API，基於JSON作為其主要的資料格式。 我們盡最大努力準確描述回應，不只透過（在適當的情形下）使用HTTP狀態碼，您也可以自行剖析回應來了解請求是否成功，以及如果不成功的原因。

您可以透過向適當的&#8203;`/Api`&#8203;端點傳送適當的請求來存取我們的ASF API。 您可以使用這些API端點來製作您自己的輔助腳本、工具、GUI等。 這正是我們的ASF-ui在幕後所實現的目標，而其他所有工具都可以實現相同目標。 ASF API由ASF核心開發團隊所支援與維護。

有關可用端點、描述、請求、回應、HTTP狀態碼及所有關於ASF API的完整文件，請參閱我們的&#8203;**[Swagger文件](#swagger-文件)**&#8203;。

![ASF API](https://i.imgur.com/yggjf5v.png)

---

# 自訂組態

我們的IPC介面支援額外的設定檔&#8203;`IPC.config`&#8203;，應該放置於ASF的&#8203;`config`&#8203;資料夾中。

如果可用，本檔案將指定ASF的Kestrel http伺服器的進階設定，以及其他與IPC相關的調整。 除非您有特殊需求，否則沒有理由使用此檔案，因為ASF在這個情形下已經使用了合理的預設值。

設定檔基於以下JSON結構：

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

`Endpoints`&#8203;：這是端點的集合，每個端點都有自己的唯一名稱（像是&#8203;`example-http4`&#8203;）與&#8203;`Protocol://Host:Port`&#8203;指定監聽地址的&#8203;`Url`&#8203;屬性。 預設情形下，ASF監聽IPv4與IPv6的HTTP位址，但如果需要，我們加入了HTTP範例以供您使用。 您應只宣告您所需的端點，我們在上面包含了4個範例端點，讓您能夠輕鬆地編輯它們。

`Host`&#8203;接受&#8203;`localhost`&#8203;、監聽介面的固定IP位址（IPv4/IPv6），或將ASF的HTTP伺服器綁定至所有可用介面的&#8203;`*`&#8203;值。 使用其他值，像是&#8203;`mydomain.com`&#8203;或&#8203;`192.168.0.*`&#8203;的作用與&#8203;`*`&#8203;相同，沒有實現IP篩選，因此當您使用允許遠端存取的&#8203;`Host`&#8203;值時應格外小心。 這樣將會允許其他設備存取ASF的IPC介面，並可能會帶來安全風險。 在這種情形下，我們強烈建議您&#8203;**至少**&#8203;使用&#8203;`IPCPassword`&#8203;（最好也使用您自己的防火牆）。

`KnownNetworks`&#8203;：本&#8203;**選擇性**&#8203;變數指定我們能夠信任的網路位址。 預設情形下，ASF被設定成&#8203;**只**&#8203;信任回送介面（&#8203;`localhost`&#8203;，同一台設備上）。 這個屬性有兩種用途。 第一，若您省略了&#8203;`IPCPassword`&#8203;，那麼我們將只允許來自已知網路的設備存取ASF的API，並拒絕其他所有設備，以作為一項安全措施。 第二，此屬性對於存取ASF的反向代理至關重要，因為只有當反向代理伺服器來自已知網路時，ASF才會遵守其表頭資料。 遵守表頭資料對於ASF的反暴力破解機制至關重要，因為它不會在出現問題時封鎖反向代理，而是封鎖反向代理原始訊息來源指定的IP。 需格外小心您在此處指定的網路，因為一旦受信任的設備被破解或被錯誤設定，就可能導致欺騙攻擊或未授權的存取。

`PathBase`&#8203;：本&#8203;**選擇性**&#8203;基底路徑將在IPC介面中使用。 預設是&#8203;`/`&#8203;，且在大多數情形下不需修改。 透過修改此屬性，您可以使用自訂前綴代管整個IPC介面，例如使用&#8203;`http://localhost:1242/MyPrefix`&#8203;代替&#8203;`http://localhost:1242`&#8203;。 若您只想代理特定的URL，使用自訂&#8203;`PathBase`&#8203;可能還需要結合特定設定的反向代理，例如代理&#8203;`mydomain.com/ASF`&#8203;而不是整個&#8203;`mydomain.com`&#8203;網域。 通常您需要為您的Web伺服器編寫一個重寫規則，映射&#8203;`mydomain.com/ASF/Api/X`&#8203; -> &#8203;`localhost:1242/Api/X`&#8203;；但您可以定義&#8203;`/ASF`&#8203;的自訂&#8203;`PathBase`&#8203;，來更輕鬆地設定&#8203;`mydomain.com/ASF/Api/X`&#8203; -> &#8203;`localhost:1242/ASF/Api/X`&#8203;。

除非您確實需要指定自訂基底路徑，否則最好保留預設值。

## 設定範例

### 更改預設連接埠

以下設定只是將ASF預設的監聽通訊埠從&#8203;`1242`&#8203;改成&#8203;`1337`&#8203;。 您可以使用任何想要的埠號，但我們的建議範圍是&#8203;`1024-32767`&#8203;，因為其他埠號通常是&#8203;**[註冊的](https://en.wikipedia.org/wiki/Registered_port)**&#8203;，且例如在Linux上可能需要&#8203;`root`&#8203;權限。

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

以下設定將允許任何來源的遠端存取，因此您應&#8203;**確保您已閱讀並理解我們關於這些的安全須知**&#8203;，見上文。

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

若您不需從所有來源存取，例如只需您的LAN，那麼最好是檢查代管ASF設備的本機IP位址，例如使用&#8203;`192.168.0.10`&#8203;來代替上述範例設定的&#8203;`*`&#8203;。

---

# 身分驗證

預設情形下，ASF IPC介面不需要任何形式的身分驗證，因為&#8203;`IPCPassword`&#8203;設定成&#8203;`null`&#8203;。 但是，如果&#8203;`IPCPassword`&#8203;被設定成任意非空值來啟用，每個ASF的API呼叫都需要匹配&#8203;`IPCPassword`&#8203;的密碼。 若您省略了認證資訊或輸入了錯誤的密碼，您將會收到&#8203;`401 - Unauthorized`&#8203;錯誤。 在5次身分驗證的嘗試失敗後（密碼錯誤），您將收到&#8203;`403 - Forbidden`&#8203;錯誤並會被暫時封鎖。

Authentication can be done through two separate ways.

## `Authentication` 表頭資料

在一般情形下，您應使用HTTP請求頭欄位，將您的密碼設定成&#8203;`Authentication`&#8203;欄位的值。 具體方式取決於您用於存取ASF的IPC介面的實際工具，例如假設您使用&#8203;`curl`&#8203;，那麼您應加入&#8203;`-H 'Authentication: MyPassword'`&#8203;作為參數。 這種方式的驗證資訊會在請求的頭欄位中傳遞，這也是它應該在的地方。

## Query 字串中的 `password` 參數

另一種方式，您可以將&#8203;`password`&#8203;參數附加到您要呼叫的URL的尾端，例如呼叫&#8203;`/Api/ASF?password=MyPassword`&#8203;來取代&#8203;`/Api/ASF`&#8203;。 這種方式夠好用，但顯然它會公開密碼，這在一些情形下並不適合。 除此之外，它是Query字串中的額外引數，這使URL看起來更加複雜，並讓人感覺它是特定的URL，但實際上這個密碼適用於整個ASF API通訊。

---

兩種方式都受到支援，您可以選擇使用其中一種。 我們建議盡可能使用HTTP頭欄位，因為從使用方面它比Query字串更適合。 但是，我們也支援Query字串，主要的原因是請求頭欄位有各種相關限制。 一個很好的範例是在Javascript中初始化Websocket連線時，缺少自訂的表頭資料（即使這完全符合RFC）。 在這種情形下，Query字串是進行身分驗證的唯一方式。

---

# Swagger 文件

除了ASF API及ASF-ui以外，我們的IPC介面還包含了Swagger文件，可以在&#8203;`/swagger`&#8203; &#8203;**[URL](http://localhost:1242/swagger)**&#8203;下找到。 Swagger文件充當我們的API實施及使用它的其他工具（例如ASF-ui）之間的中間人。 它提供了&#8203;**[OpenAPI](https://swagger.io/resources/open-api)**&#8203;規範中所有API端點的完整文件與功能，其他專案可以輕鬆使用這些端點，使您能夠輕鬆編寫及測試ASF API。

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

Starting with ASF V5.1.2.1, we've added additional security measure that, by default, allows only loopback interface (`localhost`, your own machine) to access ASF API without `IPCPassword` set in the config. This is because using `IPCPassword` should be a **minimum** security measure set by everybody who decides to expose ASF interface further.

The change was dictated by the fact that massive amount of ASFs hosted globally by unaware users were being taken over for malicious intents, usually leaving people without accounts and without items on them. Now we could say "they could read this page before opening ASF to the entire world", but instead it makes more sense to disallow insecure ASF setups by default, and require from users an action if they explicitly want to allow it, which we elaborate about below.

In particular, you're able to override our decision by specifying the networks which you trust to reach ASF without `IPCPassword` specified, you can set those in `KnownNetworks` property in custom config. However, unless you **really** know what you're doing and fully understand the risks, you should instead use `IPCPassword` as declaring `KnownNetworks` will allow everybody from those networks to access ASF API unconditionally. We're serious, people were already shooting themselves in the foot believing their reverse proxies and iptables rules were secure, but they weren't, `IPCPassword` is the first and sometimes the last guardian, if you decide to opt out of this simple, yet very effective and secure mechanism, you'll have only yourself to blame.