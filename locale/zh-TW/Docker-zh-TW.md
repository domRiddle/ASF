# Docker

從3.0.3.2版本開始，ASF現在也可用於&#8203;**[Docker容器](https://www.docker.com/what-container)**&#8203;中。 我們的Docker倉庫同時部署於&#8203;**[ghcr.io](https://github.com/orgs/JustArchiNET/packages/container/archisteamfarm/versions)**&#8203;及&#8203;**[Docker Hub](https://hub.docker.com/r/justarchi/archisteamfarm)**&#8203;。

重要的是，需注意在Docker容器中執行ASF被視為&#8203;**進階設定**&#8203;，對於絕大多數使用者來說是&#8203;**不需要的**&#8203;，且通常與非容器設定相比&#8203;**並無優勢**&#8203;。 若您考慮Docker當作執行ASF作為服務的一種解決方案，例如使它隨著您的作業系統自動啟動，那麼您應考慮閱讀&#8203;**[管理](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Management-zh-TW#linux-的-systemd-服務)**&#8203;章節，並適當設定&#8203;`systemd`&#8203;服務，這&#8203;**幾乎總比**&#8203;在Docker容器中執行ASF還要更好。

在Docker容器中執行ASF通常會涉及&#8203;**一些新問題及狀況**&#8203;，您必須自行面對並解決這些問題。 這就是為什麼我們&#8203;**強烈**&#8203;建議您避免使用它，除非您已經具備Docker的相關知識，且不需要其他人幫助您了解其內部結構，因為我們不會在ASF Wiki詳細說明這些。 本章節主要提供了非常複雜設定的有效範例，例如關於進階網路的設定，或安全性超過ASF在&#8203;`systemd`&#8203;服務中所附帶的標準沙盒（它已經透過非常先進的安全機制來確保卓越的程序隔離）。 對於那些少數人，在這裡我們著重解釋了關於ASF與Docker相容性的概念，僅此而已。若您決定將ASF與Docker一起使用，我們會假定您已擁有足夠的Docker知識。

---

## 標籤

ASF有4種主要類型的&#8203;**[標籤](https://hub.docker.com/r/justarchi/archisteamfarm/tags)**&#8203;：


### `main`

本標籤始終指向在&#8203;`main`&#8203;分支中ASF所提交的最新建置版本，其運作原理等同於直接從我們的&#8203;**[CI](https://github.com/JustArchiNET/ArchiSteamFarm/actions/workflows/publish.yml?query=branch%3Amain)**&#8203;管線中抓取最新產生的版本。 通常您應避免使用此標籤，因為它是以開發為目的，專為開發人員及進階使用者的軟體，含有大量的錯誤。 該映像檔會隨著GitHub的&#8203;`main`&#8203;分支每次提交而更新，因此您可以預期它會頻繁更新（且經常損壞）。 這是我們用來標示ASF專案的當前狀態，不一定能保證穩定或經過測試，就像在我們的發布週期中所說明的那樣。 此標籤不應在任何生產環境中使用。


### `released`

與上述標籤非常相似，本標籤始終指向ASF&#8203;**[最新發布](https://github.com/JustArchiNET/ArchiSteamFarm/releases)**&#8203;的版本，包含預覽版本。 與&#8203;`main`&#8203;標籤不同，此映像檔會在每次推播新的GitHub標籤時更新。 為喜歡冒險、敢於嘗試新事物但又可能穩定的進階使用者所準備。 若您不想使用&#8203;`latest`&#8203;標籤，我們建議您使用此標籤。 實際上，它的運作方式與滾動標籤在拉取時指向最新&#8203;`A.B.C.D`&#8203;版本的方式相同。 請注意，使用此標籤與使用我們的&#8203;**[預覽版本](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle-zh-TW)**&#8203;是相同的。


### `latest`

與其他標籤相比，只有本標籤包含了ASF的自動更新功能，且指向ASF最新的&#8203;**[穩定版本](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)**&#8203;。 此標籤的目的是提供一個預設的合理Docker容器，能執行適用於特定作業系統的ASF建置版本的自動更新。 因此，此映像檔不需要經常更新，因為所包含的ASF版本將會在需要時進行自動更新。 當然，可以安全關閉&#8203;`UpdatePeriod`&#8203;（設定成&#8203;`0`&#8203;），但在這種情形下，您可能更應使用固定的&#8203;`A.B.C.D`&#8203;版本。 同樣地，您可以修改預設的&#8203;`UpdateChannel`&#8203;，以改為自動更新&#8203;`released`&#8203;標籤。

由於&#8203;`latest`&#8203;映像檔具有自動更新功能，它具有&#8203;`linux`&#8203;作業系統特定的裸作業系統ASF版本，與所有其他標籤相反，包含含有.NET作業系統執行環境及&#8203;`generic`&#8203;的ASF版本。 這是因為較新（更新後）的ASF版本可能會需要比映像檔內建還要新的執行環境，這將需要從頭開始重新建置映像檔，並使使用計畫無效。

### `A.B.C.D`

與上述的標籤相比，本標籤是固定的，這代表映像檔一經發布就不再會更新。 這個運作方式與我們GitHub發布版本相似，在最初的版本發布後就不會變動，這將保證您的環境穩定。 通常，當您想使用特定的ASF版本，且不想使用任何類型的自動更新（例如&#8203;`latest`&#8203;標籤所提供的）時，您就應該使用此標籤。

---

## 哪個標籤最適合我？

這取決於您的需求。 對於大多數使用者來說，&#8203;`latest`&#8203;標籤應該是最好的，因為它提供了桌面執行ASF時所有的一切，區別只是作為服務執行在特殊的Docker容器中。 而那些經常重新建立映像檔，或想要自己完全控制ASF版本的人來說，可能會更喜歡&#8203;`released`&#8203;標籤。 若您想要使用某個固定的ASF版本，或在您沒有打算時永遠不會變更版本，&#8203;`A.B.C.D`&#8203;版本可供您使用，作為ASF固定的里程碑，隨時都可以回溯至此。

一般來說我們不建議使用&#8203;`main`&#8203;建置版本，因為這只是用來標示ASF專案當前狀態的。 這種狀態無法保證能正常運作，但是如果您對ASF的開發感興趣，非常歡迎您去嘗試。

---

## 架構

ASF Docker映像檔目前建立於&#8203;`linux`&#8203;平台上，針對3種架構：&#8203;`x64`&#8203;、&#8203;`arm`&#8203;及&#8203;`arm64`&#8203;。 您可以在&#8203;**[相容性](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-zh-TW)**&#8203;章節中了解更多。

從ASF V5.0.2.2版本開始，我們的標籤使用多平台清單，這代表安裝在您設備上的Docker會在拉取時自動選擇適合的映像檔。 若您需要拉取不符合您當前執行平台的特定映像檔，您可以透過適當的Docker命令&#8203;`--platform`&#8203;切換，例如&#8203;`docker run`&#8203;。 查看關於&#8203;**[映像檔清單](https://docs.docker.com/registry/spec/manifest-v2-2)**&#8203;的Docker文件以了解更多。

---

## 使用方法

若需完整資料，請使用&#8203;**[Docker官方文件](https://docs.docker.com/engine/reference/commandline/docker)**&#8203;，我們在本指南中只會介紹基礎用法，歡迎您去更深入的挖掘。

### 你好，ASF！

首先，我們應該驗證Docker是否運作正常，作為我們ASF的「Hello World」：

```shell
docker run -it --name asf --pull always --rm justarchi/archisteamfarm
```

`docker run`&#8203;會為您建立一個新的ASF Docker容器，並在前景中執行（&#8203;`-it`&#8203;）。 `--pull always`&#8203;確保會最先拉取最新的映像檔，而&#8203;`--rm`&#8203;確保我們的容器在停止之後會被清除，因為我們現在只是測試一切是否運作正常。

若一切運作正常，在拉取所有層並啟動容器後，您應該會注意到ASF已正確啟動，並通知我們目前沒有定義任何Bot，這很好⸺我們驗證了ASF在Docker中運作正常。 先按&#8203;`CTRL+P`&#8203;，再按&#8203;`CTRL+Q`&#8203;來退出Docker容器前景，然後使用&#8203;`docker stop asf`&#8203;停止ASF容器。

若仔細查看該命令，您會注意到我們沒有宣告任何標籤，它會自動預設成&#8203;`latest`&#8203;。 如果您想要使用與&#8203;`latest`&#8203;不同的標籤，例如&#8203;`released`&#8203;，那麼您應該顯性宣告：

```shell
docker run -it --name asf --pull always --rm justarchi/archisteamfarm:released
```

---

## 使用卷

若您在Docker容器中使用ASF，那麼很明顯您需要設定程式自身。 您可以透過不同方式做到，但建議是在本機電腦中建立&#8203;`config`&#8203;資料夾，然後在ASF Docker容器中掛載它作為共用卷。

舉例來說，我們假設您的ASF設定資料夾位於&#8203;`/home/archi/ASF/config`&#8203;資料夾中。 這個資料夾包含&#8203;`ASF.json`&#8203;核心與我們想要執行的Bot。 現在我們只需要將這個資料夾當作共用卷附加至Docker容器中，也就是ASF的設定資料夾（&#8203;`/app/config`&#8203;）。

```shell
docker run -it -v /home/archi/ASF/config:/app/config --name asf --pull always justarchi/archisteamfarm
```

就是這樣，現在您的ASF Docker容器將會以讀寫模式使用您本機電腦的共用資料夾，這是設定ASF的一切。 您可以使用相同的方式掛載您想要與ASF共用的其他卷，例如&#8203;`/app/logs`&#8203;或&#8203;`/app/plugins/MyCustomPluginDirectory`&#8203;。

當然，這只是達成我們目標的一種特定方式，沒有什麼阻止您，例如建立您自己的&#8203;`Dockerfile`&#8203;將您的設定檔複製至ASF Docker容器中的&#8203;`/app/config`&#8203;資料夾中。 本指南只會涵蓋基礎用法。

### 卷的權限

ASF容器預設是以預設的&#8203;`root`&#8203;使用者初始化，這使容器能夠處理內部權限問題，然後再切換成&#8203;`asf`&#8203;（UID &#8203;`1000`&#8203;）使用者來處理主程序的剩餘部分。 雖然這對絕大多數使用者來說應該能夠接受，但它確實會影響共用卷，因為新建立的檔案擁有者會是&#8203;`asf`&#8203;使用者，如果您想讓其他使用者使用您的共用卷，這可能就不是很理想了。

Docker允許您向&#8203;`docker run`&#8203;命令傳遞&#8203;`--user`&#8203;**[旗標](https://docs.docker.com/engine/reference/run/#user)**&#8203;，定義執行ASF的預設使用者。 您可以透過例如&#8203;`id`&#8203;之類的命令來查詢您的&#8203;`uid`&#8203;及&#8203;`gid`&#8203;，然後把它傳遞給剩餘的命令。 舉例來說，假設您的目標使用者的&#8203;`uid`&#8203;及&#8203;`gid`&#8203;皆為1001：

```shell
docker run -it -u 1001:1001 -v /home/archi/ASF/config:/app/config --name asf --pull always justarchi/archisteamfarm
```

記住，預設情形下，ASF使用的&#8203;`/app`&#8203;資料夾仍為&#8203;`asf`&#8203;所擁有。 若您使用自訂使用者執行ASF，那麼您的ASF程序不會擁有寫入自身檔案的權限。 這個權限並非操作所必需的，但對於某些功能來說相當重要，例如自動更新功能。 若要解決此問題，只需將ASF的所有檔案擁有者從預設的&#8203;`asf`&#8203;改成您新增的使用者即可。

```shell
docker exec -u root asf chown -hR 1001:1001 /app
```

在您使用&#8203;`docker run`&#8203;建立您的容器以後，只需執行一次，且只有在您決定為ASF程序使用自訂使用者時才需執行。 也不要忘記將上述命令中的&#8203;`1001:1001`&#8203;引數更改成您實際想要執行ASF的&#8203;`uid`&#8203;及&#8203;`gid`&#8203;。

### 在 SELinux 使用卷

若您在作業系統上以強制狀態使用SELinux，這是例如基於RHEL發行版的預設設定，那麼您應該在掛載卷時附加&#8203;`:Z`&#8203;選項，才能正確設定SELinux的上下文。

```
docker run -it -v /home/archi/ASF/config:/app/config:Z --name asf --pull always justarchi/archisteamfarm
```

這會允許ASF在Docker容器中建立以卷為目標的檔案。

---

## 多個實例同步

ASF includes support for multiple instances synchronization, as stated in **[compatibility](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#multiple-instances)** section. When running ASF in docker container, you can optionally "opt-in" into the process, in case you're running multiple containers with ASF and you'd like for them to synchronize with each other.

By default, each ASF running inside a docker container is standalone, which means that no synchronization takes place. In order to enable synchronization between them, you must bind `/tmp/ASF` path in every ASF container that you want to synchronize, to one, shared path on your docker host, in read-write mode. This is achieved exactly the same as binding a volume which was described above, just with different paths:

```shell
mkdir -p /tmp/ASF-g1
docker run -v /tmp/ASF-g1:/tmp/ASF -v /home/archi/ASF/config:/app/config --name asf1 --pull always justarchi/archisteamfarm
docker run -v /tmp/ASF-g1:/tmp/ASF -v /home/john/ASF/config:/app/config --name asf2 --pull always justarchi/archisteamfarm
# 以此類推，所有 ASF 容器現在相互同步
```

We recommend to bind ASF's `/tmp/ASF` directory also to a temporary `/tmp` directory on your machine, but of course you're free to choose any other one that satisfies your usage. Each ASF container that is expected to be synchronized should have its `/tmp/ASF` directory shared with other containers that are taking part in the same synchronization process.

As you've probably guessed from example above, it's also possible to create two or more "synchronization groups", by binding different docker host paths into ASF's `/tmp/ASF`.

Mounting `/tmp/ASF` is completely optional and actually not recommended, unless you explicitly want to synchronize two or more ASF containers. We do not recommend mounting `/tmp/ASF` for single-container usage, as it brings absolutely no benefits if you expect to run just one ASF container, and it might actually cause issues that could otherwise be avoided.

---

## 命令列引數

ASF allows you to pass **[command-line arguments](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments)** in docker container through environment variables. You should use specific environment variables for supported switches, and `ASF_ARGS` for the rest. This can be achieved with `-e` switch added to `docker run`, for example:

```shell
docker run -it -e "ASF_CRYPTKEY=MyPassword" -e "ASF_ARGS=--no-config-migrate" --name asf --pull always justarchi/archisteamfarm
```

This will properly pass your `--cryptkey` argument to ASF process being run inside docker container, as well as other args. Of course, if you're advanced user then you can also modify `ENTRYPOINT` or add `CMD` and pass your custom arguments yourself.

Unless you want to provide custom encryption key or other advanced options, usually you don't need to include any special environment variables, as our docker containers are already configured to run with a sane expected default options of `--no-restart` `--process-required` `--system-required`, so those flags do not need to be specified explicitly in `ASF_ARGS`.

---

## IPC

Assuming you didn't change the default value for `IPC` **[global configuration property](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#global-config)**, it's already enabled. However, you must do two additional things for IPC to work in Docker container. Firstly, you must use `IPCPassword` or modify default `KnownNetworks` in custom `IPC.config` to allow you to connect from the outside without using one. Unless you really know what you're doing, just use `IPCPassword`. Secondly, you have to modify default listening address of `localhost`, as docker can't route outside traffic to loopback interface. An example of a setting that will listen on all interfaces would be `http://*:1242`. Of course, you can also use more restrictive bindings, such as local LAN or VPN network only, but it has to be a route accessible from the outside - `localhost` won't do, as the route is entirely within guest machine.

For doing the above you should use **[custom IPC config](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#custom-configuration)** such as the one below:

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

Once we set up IPC on non-loopback interface, we need to tell docker to map ASF's `1242/tcp` port either with `-P` or `-p` switch.

For example, this command would expose ASF IPC interface to host machine (only):

```shell
docker run -it -p 127.0.0.1:1242:1242 -p [::1]:1242:1242 --name asf --pull always justarchi/archisteamfarm
```

If you set everything properly, `docker run` command above will make **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** interface work from your host machine, on standard `localhost:1242` route that is now properly redirected to your guest machine. It's also nice to note that we do not expose this route further, so connection can be done only within docker host, and therefore keeping it secure. Of course, you can expose the route further if you know what you're doing and ensure appropriate security measures.

---

### 完整的範例

Combining whole knowledge above, an example of a complete setup would look like this:

```shell
docker run -p 127.0.0.1:1242:1242 -p [::1]:1242:1242 -v /home/archi/ASF/config:/app/config --name asf --pull always justarchi/archisteamfarm
```

這假設您將使用單個 ASF 容器，並且所有 ASF 配置文件都位於 `/home/archi/ASF/config` 中。 You should modify the config path to the one that matches your machine. This setup is also ready for optional IPC usage if you've decided to include `IPC.config` in your config directory with a content like below:

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

---

## 專業小技巧

When you already have your ASF docker container ready, you don't have to use `docker run` every time. You can easily stop/start ASF docker container with `docker stop asf` and `docker start asf`. Keep in mind that if you're not using `latest` tag then using up-to-date ASF will still require from you to `docker stop`, `docker rm` and `docker run` again. This is because you must rebuild your container from fresh ASF docker image every time you want to use ASF version included in that image. In `latest` tag, ASF has included capability to auto-update itself, so rebuilding the image is not necessary for using up-to-date ASF (but it's still a good idea to do it from time to time in order to use fresh .NET runtime dependencies and the underlying OS).

As hinted by above, ASF in tag other than `latest` won't automatically update itself, which means that **you** are in charge of using up-to-date `justarchi/archisteamfarm` repo. This has many advantages as typically the app should not touch its own code when being run, but we also understand convenience that comes from not having to worry about ASF version in your docker container. If you care about good practices and proper docker usage, `released` tag is what we'd suggest instead of `latest`, but if you can't be bothered with it and you just want to make ASF both work and auto-update itself, then `latest` will do.

You should typically run ASF in docker container with `Headless: true` global setting. This will clearly tell ASF that you're not here to provide missing details and it should not ask for those. Of course, for initial setup you should consider leaving that option at `false` so you can easily set up things, but in long-run you're typically not attached to ASF console, therefore it'd make sense to inform ASF about that and use `input` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** if need arises. This way ASF won't have to wait infinitely for user input that will not happen (and waste resources while doing so). It will also allow ASF to run in non-interactive mode inside container, which is crucial e.g. in regards to forwarding signals, making it possible for ASF to gracefully close on `docker stop asf` request.