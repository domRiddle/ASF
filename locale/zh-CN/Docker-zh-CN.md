# Docker

从 3.0.3.2 版本开始，ASF 也可以在 **[Docker 容器](https://www.docker.com/what-container)**&#8203;中运行。 对于普通用户来说，在 Docker 容器中运行 ASF 并没有什么特别的好处。但对于服务器用户来说，这可能是运行 ASF 的最佳方式，因为这样可以确保 ASF 运行在与其他应用分离的沙盒中。 我们的 Docker 仓库可以在&#8203;**[这里](https://hub.docker.com/r/justarchi/archisteamfarm)**&#8203;找到。

* * *

## 分支

ASF 有 4 种主要的&#8203;**[分支](https://hub.docker.com/r/justarchi/archisteamfarm/tags)**。

### `master`

这个分支始终指向 GitHub 中 master 分支的最新提交构建的 ASF，其工作原理与&#8203;**[发布周期](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle-zh-CN)**&#8203;中描述的实验性 AppVeyor 构建相同。 一般而言，您应该避免使用该分支，因为它是用于开发目的，为开发人员和高级用户准备的，有最高的漏洞风险。 该映像会在每次提交到 GitHub master 分支后更新，因此您会发现它的更新十分频繁（并且经常出错），就像 AppVeyor 构建一样。 该分支记录了 ASF 项目的当前状态，但该状态不一定稳定或者经过测试，就像我们在发布周期中描述的那样。 这个分支不应该在任何的生产环境中使用。

### `released`

与上述分支类似，这个分支始终指向最新的 **[released](https://github.com/JustArchiNET/ArchiSteamFarm/releases)** ASF 版本，包括预览版本。 与 `master` 分支不同，该映像会在推送新的 GitHub 版本标签时更新。 一些高级用户喜欢立刻尝试最新的功能，选择处于稳定边缘的版本，这一分支就是为他们准备的。 如果您不想使用 `latest` 分支的话，我们推荐您使用这个分支。 请注意，使用此分支等同于使用我们的&#8203;**[预览版](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle-zh-CN)**。

### `latest`

This tag in comparison with previous two, as the first one includes ASF auto-updates feature and will typically point to the one of the stable versions, but not necessarily the latest one. The objective of this tag is to provide a sane default Docker container that is capable of running self-updating ASF. Because of that, the image doesn't have to be updated as often as possible, as included ASF version will always be capable of updating itself if needed. Of course, `UpdatePeriod` can be safely turned off (set to `0`), but in this case you should probably use frozen `A.B.C.D` release instead. Likewise, you can modify default `UpdateChannel` in order to make auto-updating `released` tag instead.

### `A.B.C.D`

In comparison with above tags, this tag is completely frozen, which means that the image won't be updated once published. This works similar to our GitHub releases that are never touched after the initial release, which guarantees you stable and frozen environment. Typically you should use this tag when you want to use some specific ASF release and you don't want to use auto-updates that are offered in `latest` tag.

* * *

## 哪个分支最适合我？

That depends on what you're looking for. For majority of users, `latest` tag should be the best one as it offers exactly what desktop ASF does, just in special Docker container as a service. People that are rebuilding their images quite often and would instead prefer to have ASF version tied to given release are welcome to use `released` tag. If you instead want to use some specific frozen ASF version that will never change without your clear intention, `A.B.C.D` releases are available for you as fixed ASF milestones you can always fall back to.

We generally discourage trying `master` builds, just like automated AppVeyor builds - this build is here for us to mark current state of ASF project. Nothing guarantees that such state will work properly, but of course you're more than welcome to give them a try if you're interested in ASF development.

* * *

## 架构

ASF docker image is currently available for 2 architectures - `x64` and `arm`. 您可以阅读&#8203;**[兼容性](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-zh-CN)**&#8203;章节了解更多。

Since multi-arch docker tags are still work-in-progress, builds for other architectures than default `x64` are currently available with `-{ARCH}` appended to the tag name. In other words, if you want to use `latest` tag for `arm` architecture, simply use `latest-arm`.

* * *

## 用法

For complete reference you should use **[official docker documentation](https://docs.docker.com/engine/reference/commandline/docker)**, we'll cover only basic usage in this guide, you're more than welcome to dig deeper.

### 你好，ASF！

Firstly we should verify if our docker is even working correctly, this will serve as our ASF "hello world":

```shell
docker pull justarchi/archisteamfarm
docker run -it --name asf justarchi/archisteamfarm
```

`docker pull` command ensures that you're using up-to-date `justarchi/archisteamfarm` image, just in case you had outdated local copy in your cache. `docker run` creates a new ASF docker container for you and runs it in the foreground (`-it`).

If everything ended successfully, after pulling all layers and starting container, you should notice that ASF properly started and informed us that there are no defined bots, which is good - we verified that ASF in docker works properly. Hit `CTRL+P` then `CTRL+Q` in order to quit foreground docker container, then stop ASF container with `docker stop asf`, and remove it with `docker rm asf`.

If you take a closer look at the command then you'll notice that we didn't declare any tag, which automatically defaulted to `latest` one. If you want to use other tag than `latest`, for example `latest-arm`, then you should declare it explicitly:

```shell
docker pull justarchi/archisteamfarm:latest-arm
docker run -it --name asf justarchi/archisteamfarm:latest-arm
```

* * *

## 使用数据卷

If you're using ASF in docker container then obviously you need to configure the program itself. You can do it in various different ways, but the recommended one would be to create ASF `config` directory on local machine, then mount it as a shared volume in ASF docker container.

For example, we'll assume that your ASF config folder is in `/home/archi/ASF/config` directory. This directory contains core `ASF.json` as well as bots that we want to run. Now all we need to do is simply attaching that directory as shared volume in our docker container, where ASF expects its config directory (`/app/config`).

```shell
docker pull justarchi/archisteamfarm
docker run -it -v /home/archi/ASF/config:/app/config --name asf justarchi/archisteamfarm
```

And that's it, now your ASF docker container will use shared directory with your local machine in read-write mode, which is everything you need for configuring ASF.

Of course, this is just one specific way to achieve what we want, nothing is stopping you from e.g. creating your own `Dockerfile` that will copy your config files into `/app/config` directory inside ASF docker container. We're only covering basic usage in this guide.

### 卷权限

ASF is by default run with default `root` user inside a container. This is not a problem security-wise, since we're already inside Docker container, but it does affect the shared volume as newly-generated files will be normally owned by `root`, which might not be desired situation when using a shared volume.

Docker allows you to pass `--user` **[flag](https://docs.docker.com/engine/reference/run/#user)** to `docker run` command which will define default user that ASF will run under. You can check your `uid` and `gid` for example with `id` command, then pass it to the rest of the command. For example, if your target user has `uid` and `gid` of 1000:

```shell
docker pull justarchi/archisteamfarm
docker run -it -u 1000:1000 -v /home/archi/ASF/config:/app/config --name asf justarchi/archisteamfarm
```

Remember that by default `/app` directory used by ASF is still owned by `root`. If you run ASF under custom user, then your ASF process won't have write access to its own files. This access is not mandatory for operation, but it is crucial e.g. for auto-updates feature. In order to fix this, it's enough to change ownership of all ASF files from default `root` to your new custom user.

```shell
docker exec -u root asf chown -hR 1000:1000 /app
```

This has to be done only once after you created your container with `docker run`, and only if you decided to use custom user for ASF process. Also don't forget to change `1000:1000` argument in both commands above to the `uid` and `gid` you actually want to run ASF under.

* * *

## 命令行参数

ASF 允许您通过设定 `ASF_ARGS` 环境变量，来向 Docker 容器内传递&#8203;**[命令行参数](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments-zh-CN)**。 This can be added on top of `docker run` with `-e` switch. 例如：

```shell
docker pull justarchi/archisteamfarm
docker run -it -e "ASF_ARGS=--cryptkey MyPassword" --name asf justarchi/archisteamfarm
```

This will properly pass your `--cryptkey` argument to ASF process being run inside docker container. Of course, if you're advanced user then you can also modify `ENTRYPOINT` and pass your custom arguments yourself.

Unless you want to provide custom encryption key or other advanced options, usually you don't need to include any special `ASF_ARGS` as our docker containers are already configured to run with a sane expected default options of `--no-restart` `--process-required` `--system-required`.

* * *

## IPC

若要使用 IPC，首先您需要将 `IPC` **[全局配置属性](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-zh-CN#全局配置)**&#8203;设置为 `true`。 In addition to that, you **must** modify default listening address of `localhost`, as docker can't route outside traffic to loopback interface. An example of a setting that will listen on all interfaces would be `http://*:1242`. Of course, you can also use more restrictive bindings, such as local LAN or VPN network only, but it has to be a route accessible from the outside - `localhost` won't do, as the route is entirely within guest machine.

要做到这一点，您需要使用以下形式的&#8203;**[自定义 IPC 配置](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-zh-CN#自定义配置)**：

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
docker pull justarchi/archisteamfarm
docker run -it -p 127.0.0.1:1242:1242 -p [::1]:1242:1242 --name asf justarchi/archisteamfarm
```

如果一切设置正确，上面的 `docker run` 命令将会使 **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-zh-CN)** 接口在宿主机上正常工作，标准的 `localhost:1242` 路由已经被正确重定向到客户机上。 It's also nice to note that we do not expose this route further, so connection can be done only within docker host, and therefore keeping it secure.

* * *

### 完整示例

Combining whole knowledge above, an example of a complete setup would look like this:

```shell
docker pull justarchi/archisteamfarm
docker run -it -p 127.0.0.1:1242:1242 -p [::1]:1242:1242 -v /home/archi/asf:/app/config --name asf justarchi/archisteamfarm
```

This assumes that you have all ASF config files in `/home/archi/asf`, if not, you should modify the path to the one that matches. This setup is also ready for optional IPC usage if you've decided to include `IPC.config` in your config directory with a content like below:

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

* * *

## 高级技巧

When you already have your ASF docker container ready, you don't have to use `docker run` every time. You can easily stop/start ASF docker container with `docker stop asf` and `docker start asf`. Keep in mind that if you're not using `latest` tag then updating ASF will still require from you to `docker stop`, `docker rm`, `docker pull` and `docker run` again. This is because you must rebuild your container from fresh ASF docker image every time you want to use ASF version included in that image. In `latest` tag, ASF has included capability to auto-update itself, so rebuilding the image is not necessary for using up-to-date ASF (but it might still be a good idea to do it from time to time in order to use fresh .NET Core runtime and underlying OS).

As hinted by above, ASF in tag other than `latest` won't automatically update itself, which means that **you** are in charge of using up-to-date `justarchi/archisteamfarm` repo. This has many advantages as typically the app should not touch its own code when being run, but we also understand convenience that comes from not having to worry about ASF version in your docker container. If you care about good practices and proper docker usage, `released` tag is what we'd suggest instead of `latest`, but if you can't be bothered with it and you just want to make ASF both work and auto-update itself, then `latest` will do.

You should typically run ASF in docker container with `Headless: true` global setting. This will clearly tell ASF that you're not here to provide missing details and it should not ask for those. 当然，对于初次设置来说，您应该保持这个选项为 `false` 以便于设置，但长期来看，您很少需要连接到 ASF 控制台上，因此使 ASF 这样做是很合理的。当需要向 ASF 输入内容时，使用 `input` **[命令](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-CN)**。 This way ASF won't have to wait infinitely for user input that will not happen (and waste resources while doing so).