# Docker

Starting with version 3.0.3.2, ASF is now also available as **[Docker container](https://www.docker.com/what-container)**. Running ASF in Docker container typically has no advantages for casual users, but it might be an excellent way of making use of ASF on servers, ensuring that ASF is being run in sandboxed environment separated from all other apps. Our Docker repo can be found **[here](https://hub.docker.com/r/justarchi/archisteamfarm)**.

---

## Tags

ASF is available through 3 main types of **[tags](https://hub.docker.com/r/justarchi/archisteamfarm/tags)**:


### `master`

This tag always points to the ASF built from latest commit in master branch, which works the same as experimental AppVeyor build described in our **[release cycle](https://github.com/JustArchi/ArchiSteamFarm/wiki/Release-cycle)**. Typically you should avoid this tag, as it's the highest level of bugged software dedicated to developers and advanced users for development purposes.


### `latest`

This tag always points to the latest **[released](https://github.com/JustArchi/ArchiSteamFarm/releases)** ASF version, including pre-releases. It's the default and therefore recommended tag for grabbing latest ASF version for usage in Docker container.

### `A.B.C.D`

In addition to `master` and `latest` tags that will typically change with time and always point to appropriate releases, ASF docker repo also provides you with fixed frozen release that you can use if you want to have specific version installed. This version, once built, is considered frozen and will not be modified in any way, just like our GitHub releases.

---

## Usage

For complete reference you should use **[Docker documentation](https://docs.docker.com/engine/reference/run/)**, we'll cover only basic usage in this guide.

### Hello ASF!

Firstly we should verify if our Docker is even working correctly, this will serve as our ASF "hello world":

```
docker run -it --name asf justarchi/archisteamfarm
```

If everything ended successfully, after pulling all layers and starting container, you should notice that ASF properly started and informed us that there are no defined bots, which is good - we verified that ASF in docker works properly. Hit `CTRL+P` then `CTRL+Q` in order to quit foreground docker container, then stop ASF container with `docker stop asf`.


### Using a volume

If you're using ASF in Docker container then obviously you need to configure the program itself. You can do it in various different ways, but the recommended one would be to create ASF `config` directory on local machine, then mount it as shared volume in ASF Docker container.

For example, we'll assume that your ASF config folder is in `/home/archi/ASF/config` directory. This directory contains core `ASF.json` as well as bots that we want to run. Now all we need to do is simply to attach that directory as shared volume in our Docker container, where ASF expects its config directory (`/app/config`).

```
docker run -dit -v /home/archi/ASF/config:/app/config --name asf justarchi/archisteamfarm
```

And that's it, now your ASF Docker container will use shared directory with your local machine in read-write mode, which is everything you need for configuring ASF. We also added `-d` option in order to start ASF container in detached mode.

Of course, this is just one specific way to achieve what we want, nothing is stopping you from e.g. creating your own `Dockerfile` that will copy your config files into `/app/config` directory inside ASF Docker container.

---

## Pro tips

- You should typically run ASF in Docker container with `AutoRestart: false` and `Headless: true` global settings.
- ASF included in Docker container won't automatically update itself, which means that **you** are in charge of using up-to-date `justarchi/archisteamfarm` repo.