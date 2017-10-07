# Docker

Starting with version 3.0.3.2, ASF is now also available as **[docker container](https://www.docker.com/what-container)**. Running ASF in docker container typically has no advantages for casual users, but it might be an excellent way of making use of ASF on servers, ensuring that ASF is being run in sandboxed environment separated from all other apps. Our docker repo can be found **[here](https://hub.docker.com/r/justarchi/archisteamfarm)**.

---

## Tags

ASF is available through 3 main types of **[tags](https://hub.docker.com/r/justarchi/archisteamfarm/tags)**:


### `master`

This tag always points to the ASF built from latest commit in master branch, which works the same as experimental AppVeyor build described in our **[release cycle](https://github.com/JustArchi/ArchiSteamFarm/wiki/Release-cycle)**. Typically you should avoid this tag, as it's the highest level of bugged software dedicated to developers and advanced users for development purposes.


### `latest`

This tag always points to the latest **[released](https://github.com/JustArchi/ArchiSteamFarm/releases)** ASF version, including pre-releases. It's the default and therefore recommended tag for grabbing latest ASF version for usage in docker container.

### `A.B.C.D`

In addition to `master` and `latest` tags that will typically change with time and always point to appropriate releases, ASF Docker repo also provides you with fixed frozen release that you can use if you want to have specific version installed. This version, once built, is considered frozen and will not be modified in any way, just like our GitHub releases.

---

## Usage

For complete reference you should use **[official docker documentation](https://docs.docker.com/engine/reference/run/)**, we'll cover only basic usage in this guide.

### Hello ASF!

Firstly we should verify if our docker is even working correctly, this will serve as our ASF "hello world":

```
docker pull justarchi/archisteamfarm
docker run -it --name asf justarchi/archisteamfarm
```

`docker pull` command ensures that you're using up-to-date `justarchi/archisteamfarm` image, just in case you had outdated local copy in your cache. `docker run` creates a new ASF docker container for you and runs it in the foreground.

If everything ended successfully, after pulling all layers and starting container, you should notice that ASF properly started and informed us that there are no defined bots, which is good - we verified that ASF in docker works properly. Hit `CTRL+P` then `CTRL+Q` in order to quit foreground docker container, then stop ASF container with `docker stop asf`, and remove it with `docker rm asf`.


### Using a volume

If you're using ASF in docker container then obviously you need to configure the program itself. You can do it in various different ways, but the recommended one would be to create ASF `config` directory on local machine, then mount it as shared volume in ASF docker container.

For example, we'll assume that your ASF config folder is in `/home/archi/ASF/config` directory. This directory contains core `ASF.json` as well as bots that we want to run. Now all we need to do is simply attaching that directory as shared volume in our docker container, where ASF expects its config directory (`/app/config`).

```
docker pull justarchi/archisteamfarm
docker run -dit -v /home/archi/ASF/config:/app/config --name asf justarchi/archisteamfarm
```

And that's it, now your ASF docker container will use shared directory with your local machine in read-write mode, which is everything you need for configuring ASF. We also added `-d` option in order to start ASF container in detached mode.

Of course, this is just one specific way to achieve what we want, nothing is stopping you from e.g. creating your own `Dockerfile` that will copy your config files into `/app/config` directory inside ASF docker container.

---

## Pro tips

When you already have your ASF docker container ready, you don't have to use `docker run` every time. You can easily stop/start ASF docker container with `docker stop asf` and `docker start asf`. Although keep in mind that updating ASF will still require from you to `docker stop`, `docker rm`, `docker pull` and `docker run` again. This is because you must rebuild your container from fresh ASF docker image every time you want to use latest version.

As hinted by above, ASF included in docker container won't automatically update itself, which means that **you** are in charge of using up-to-date `justarchi/archisteamfarm` repo. At least for now, as we might bring auto-update feature eventually.

You should typically run ASF in docker container with `AutoRestart: false` and `Headless: true` global settings. This will clearly tell ASF that you don't want ASF to spawn new processes and that it should not expect user input from you.