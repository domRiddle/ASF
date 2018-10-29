# IPC

ASF beinhaltet eine eigene einzigartige IPC-Schnittstelle, die für die weitere Interaktion mit dem Prozess verwendet werden kann. IPC steht für **[inter-process communication](https://en.wikipedia.org/wiki/Inter-process_communication)** und in der einfachsten Definition ist es ein "ASF-Weboberfläche" basierend auf **[Kestrel HTTP Server](https://github.com/aspnet/KestrelHttpServer)**, welches für die weitere Integration mit dem Prozess, sowohl als Frontend für Endbenutzer (ASF-ui) als auch Backend für Drittanbieter-Integrationen (ASF-API) verwendet werden kann.

IPC kann für viele verschiedene Dinge verwendet werden, je nach deinen Fähigkeiten und Bedürfnissen. Du kannst es beispielsweise dafür verwenden, um den Status von ASF und allen Bots abzurufen, ASF-Befehle zu senden, Globale- und Bot-Konfigurationen abzurufen und zu bearbeiten, neue Bots hinzuzufügen, bestehende Bots zu löschen, Produktschlüssel an den **[BGR](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer)** zu senden oder auf das ASF-Protokoll zuzugreifen. Alle diese Aktionen werden durch unsere API offengelegt, was bedeutet, dass du deine eigenen Programme und Skripte schreiben kannst, die in der Lage sind, mit ASF zu kommunizieren und während der Laufzeit zu beeinflussen. Darüber hinaus sind ausgewählte Aktionen (z.B. das Senden von Befehlen) auch in unserem ASF-ui vorhanden, so dass du über eine benutzerfreundliche Weboberfläche einfach darauf zugreifen kannst.

* * *

# Gebrauchsweise

Du kannst unsere IPC-Schnittstelle aktivieren, indem du `IPC` **[globale Konfigurationseigenschaft](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#global-config)** aktivierst. ASF gibt den IPC-Start in seinem Protokoll an, mit dem du überprüfen kannst, ob die IPC-Schnittstelle ordnungsgemäß gestartet wurde:

    INFO|ASF|Start() Starte IPC-Server...
    INFO|ASF|Start() IPC-Server bereit!
    

ASF's http-Server hört nun auf den ausgewählten Endpunkten. Falls du keine benutzerdefinierte Konfigurationsdatei für IPC angegeben hast wird für IPv4 **[127.0.0.1](http://127.0.0.1:1242)** und für IPv6 **[[::1]](http://[::1]:1242)** auf Standard-Port `1242` verwendet. Du kannst auf unsere IPC-Schnittstelle über die obigen Links von derselben Maschine aus zugreifen, auf der auch der ASF-Prozess läuft.

Die IPC-Schnittstelle von ASF bietet je nach der von dir geplanten Nutzung drei verschiedene Möglichkeiten, darauf zuzugreifen.

Auf der untersten Ebene gibt es **[ASF API](#asf-api)**, das ist der Kern unserer IPC-Schnittstelle und ermöglicht den Betrieb aller anderen. Dies ist es, was du in deinen eigenen Programmen, Dienstprogrammen und Projekten verwenden solltest, um direkt mit ASF zu kommunizieren.

Auf der mittleren Ebene befindet sich unsere **[Swagger-Dokumentation](#swagger-documentation)**, die als Frontend für die ASF-API dient. Es bietet eine vollständige Dokumentation der ASF-API und ermöglicht einen einfacheren Zugriff darauf. Dies ist es, was du überprüfen solltest, wenn du vorhast ein Programm, ein Dienstprogramm oder andere Projekte zu schreiben, die über die API mit ASF kommunizieren sollen.

Auf der obersten Ebene gibt es **[ASF-ui](#asf-ui)**, das auf unserer ASF-API basiert und eine benutzerfreundliche Möglichkeit bietet, verschiedene ASF-Aktionen auszuführen. Dies ist unsere Standard IPC-Schnittstelle für Endanwender und ein perfektes Beispiel dafür, was du mit der ASF-API erstellen kannst. Wenn du möchtest, kannst du deine eigene benutzerdefinierte Web-Benutzeroberfläche für die Verwendung mit ASF benutzen, indem du `--path` **[Befehlszeilenargument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments#arguments)** angibst und das benutzerdefinierten `www` Verzeichnis verwendest.

* * *

# ASF-ui

ASF-ui ist ein Gemeinschaftsprojekt, das darauf abzielt, eine benutzerfreundliche grafische Weboberfläche für Endbenutzer zu erstellen. Um dies zu erreichen, fungiert es als Frontend für unsere **[ASF API](#asf-api)**, so dass du verschiedene Aktionen mit Leichtigkeit durchführen kannst. Dies ist die Standardoberfläche, mit der ASF ausgeliefert wird.

Wie bereits erwähnt, ist ASF-ui ein Community-Projekt, das nicht von kern ASF-Entwicklern betreut wird. Es folgt seinem eigenen Fluss in der **[ASF-ui Repository](https://github.com/JustArchiNET/ASF-ui)**, welches für alle damit verbundenen Fragen, Probleme, Fehlerberichte und Vorschläge verwendet werden sollte.

![ASF-ui](https://i.imgur.com/vCu2ZY5.png)

* * *

# ASF API

Unsere ASF-API ist eine typische **[RESTful](https://en.wikipedia.org/wiki/Representational_state_transfer)** Web-API, die auf JSON als primärem Datenformat basiert. Wir tun unser Bestes, um die Antwort genau zu beschreiben, indem wir sowohl HTTP-Statuscodes (wo zutreffend) als auch eine Antwort verwenden, die du selbst analysieren kannst, um zu wissen, ob die Anfrage erfolgreich war und wenn nicht, warum.

Auf unsere ASF-API kann zugegriffen werden, indem entsprechende Anfragen an entsprechende `/Api` Endpunkte gesendet werden. Du kannst diese API-Endpunkte verwenden um deine eigenen Hilfsskripte, Programme, Benutzeroberflächen und ähnliches zu erstellen. Das ist genau das, was unser ASF-ui unter der Haube leistet und jedes andere Programm kann das gleiche erreichen. ASF-API wird offiziell vom ASF-Kernteam unterstützt und gepflegt.

Für eine vollständige Dokumentation der verfügbaren Endpunkte, Beschreibungen, Anfragen, Antworten, http-Statuscodes und alles andere was die ASF-API berücksichtigt, ließ bitte unsere **[swagger-Dokumentation](#swagger-dokumentation)**.

![ASF API](https://i.imgur.com/yggjf5v.png)

* * *

## Authentifizierung

Die ASF IPC-Schnittstelle erfordert standardmäßig keine Art von Authentifizierung, da `IPCPassword` auf `null` gesetzt ist. Wenn jedoch `IPCPassword` durch das Setzen eines beliebigen nicht leeren Wertes aktiviert wird, erfordert jeder Aufruf der ASF-API das Passwort, das mit dem eingestellten `IPCPassword` übereinstimmt. Wenn du die Authentifizierung weglässt oder ein falsches Passwort eingibst, bekommst du einen `401 - Unauthorized` Fehler. Wenn du weiterhin Anfragen ohne Authentifizierung sendest, wirst du schließlich vorübergehend mit dem Fehler `403 - Forbidden` gesperrt.

Die Authentifizierung kann auf zwei verschiedene Arten erfolgen.

### `Authentication` Header

Im Allgemeinen solltest du HTTP-Request-Header verwenden, indem du das Feld `Authentication` mit deinem Passwort als Wert setzt. Die Vorgehensweise hängt davon ab, mit welchem Programm du auf die IPC-Schnittstelle von ASF zugreifst, z.B. wenn du `curl` verwendest, dann solltest du `-H 'Authentication: MeinPasswort'` als Parameter hinzufügen. Auf diese Weise wird die Authentifizierung in den Headern der Anfrage übergeben, wo sie tatsächlich stattfinden soll.

### `password` Parameter im Query-String

Alternativ kannst du den Parameter `password` an das Ende der URL anhängen die du aufrufen möchtest, z.B. indem du `/Api/ASF?password=MeinPasswort` anstelle von `/Api/ASF` allein aufrufst. Dieser Ansatz ist gut genug, aber offensichtlich enthüllt er das Passwort, was nicht unbedingt immer angemessen ist. Darüber hinaus ist es ein zusätzliches Argument im Query-String, das das Aussehen der URL verkompliziert und das Gefühl gibt, dass sie URL-spezifisch ist, während das Passwort für die gesamte ASF-API-Kommunikation gilt.

* * *

Beide Wege werden unterstützt und es liegt ganz bei dir, welche du wählen möchtest. Wir empfehlen HTTP-Header überall dort zu verwenden, wo es möglich ist, da es in der Verwendung sinnvoller ist als Query-String. Wir unterstützen jedoch auch Query-Strings, hauptsächlich wegen verschiedener Einschränkungen in Bezug auf Request-Header. Ein gutes Beispiel ist das Fehlen von benutzerdefinierten Headern beim Einleiten einer Websocket-Verbindung in Javascript (obwohl sie nach RFC vollständig gültig ist). In dieser Situation ist ein Query-String die einzige Möglichkeit sich zu authentifizieren.

* * *

## Swagger Dokumentation

Unsere IPC-Schnittstelle, zusätzlich zu ASF API und ASF-ui, beinhaltet auch die Swagger-Dokumentation, die unter `/swagger` **[URL](http://127.0.0.1:1242/swagger)** verfügbar ist. Die Swagger-Dokumentation dient als Mittelsmann zwischen unserer API-Implementierung und anderen Programmen, die sie verwenden (z.B. ASF-ui). Es bietet eine vollständige Dokumentation und Verfügbarkeit aller API-Endpunkte in der Spezifikation **[OpenAPI](https://swagger.io/resources/open-api)**, die von anderen Projekten problemlos genutzt werden kann, so dass du ASF-API mit Leichtigkeit schreiben und testen kannst.

Neben der Verwendung unserer Swagger-Dokumentation als komplette Spezifikation der ASF-API kannst du sie auch als benutzerfreundliche Möglichkeit verwenden, verschiedene API-Endpunkte auszuführen, vor allem solche die nicht von ASF-ui implementiert werden. Da unsere Swagger-Dokumentation automatisch aus ASF-Quelltext generiert wird, hast du die Garantie, dass die Dokumentation immer auf dem neuesten Stand mit den Features ist die ASF bietet.

![Swagger Dokumentation](https://i.imgur.com/mLpd5e4.png)

* * *

# FAQ (oft gestellte Fragen)

### Ist die IPC-Schnittstelle von ASF sicher und sicher in der Anwendung?

ASF hört standardmäßig nur auf `localhost` Adressen, was bedeutet, dass der Zugriff auf ASF IPC von jedem anderen Rechner als deinem eigenen **nicht möglich ist**. Wenn du keine Standardendpunkte änderst, bräuchte der Angreifer einen direkten Zugriff auf deinen eigenen Computer, um auf ASF IPC zugreifen zu können, daher ist er so sicher wie möglich und es besteht keine Möglichkeit, dass andere Personen auf ihn zugreifen, auch nicht aus deinem eigenen LAN.

Wenn du dich jedoch dazu entscheidest die standardmäßig eingestellten `localhost` Adressen an etwas anderes zu binden, dann solltest du die richtigen Firewall-Regeln **selbst** festlegen, um nur autorisierten IPs den Zugriff auf die IPC-Schnittstelle von ASF zu ermöglichen. Zusätzlich dazu empfehlen wir dringend, `IPCPassword` einzurichten, was eine weitere Ebene der zusätzlichen Sicherheit bietet. Es ist auch sinnvoll, die IPC-Schnittstelle von ASF in diesem Fall hinter einem Reverse-Proxy auszuführen, was im Folgenden näher erläutert wird.

### Can I access ASF API through my own tools or userscripts?

Yes, this is what ASF API was designed for and you can use anything capable of sending a HTTP request to access it. Local userscripts follow **[CORS](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing)** logic, and we allow access from all origins (`*`) as long as `IPCPassword` is set, as an extra security measure. This allows you to execute various authenticated ASF API requests, without allowing potentially malicious scripts to do that automatically (as they'd need to know your `IPCPassword` to do that).

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

### Can I access IPC interface through HTTPS protocol?

**Yes**, you can achieve it through two different ways. A recommended way would be to use a reverse proxy for that (described above) where you can access your web server through https like usual, and connect through it with ASF's IPC interface on the same machine. This way your traffic is fully encrypted and you don't need to modify IPC in any way to support such setup.

Second way includes specifying a **[custom config](#custom-configuration)** for ASF's IPC interface where you can enable https endpoint and provide appropriate certificate directly to our Kestrel http server. This way is recommended if you're not running any other web server and don't want to run one exclusively for ASF. Otherwise, it's much easier to achieve a satisfying setup by using a reverse proxy mechanism.

* * *

## Benutzerdefinierte Konfiguration

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

`Host` accepts a variety of values, including `*` value that binds ASF's http server to all available interfaces. Be extremely careful when you use `Host` values that allow remote access. Doing so will enable access to ASF's IPC interface from other machines, which might pose a security risk. We strongly recommend to use `IPCPassword` (and preferably your own firewall too) at a minimum in this case.

`PathBase` - This is base path that will be used by IPC interface. This property is optional, defaults to `/` and shouldn't be required to modify for majority of use cases. By changing this property you'll host entire IPC interface on a custom prefix, for example `http://127.0.0.1:1242/MyPrefix` instead of `http://127.0.0.1:1242` alone. Using custom `PathBase` might be wanted in combination with specific setup of a reverse proxy where you'd like to proxy a specific URL only, for example `mydomain.com/ASF` instead of entire `mydomain.com` domain. Normally that would require from you to write a rewrite rule for your web server that would map `mydomain.com/ASF/Api/X` -> `127.0.0.1:1242/Api/X`, but instead you might define custom `PathBase` of `/ASF` and achieve easier setup of `mydomain.com/ASF/Api/X` -> `127.0.0.1:1242/ASF/Api/X`.

Unless you truly need to specify a custom base path, it's best to leave it at default.