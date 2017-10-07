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

Firstly we should verify if our Docker is even working correctly, this will serve as our ASF "hello world":

```
docker run -it --name asf justarchi/archisteamfarm
```

If everything ended successfully, after pulling all layers and starting container, you should notice that ASF properly started and informed us that there are no defined bots, which is good - we verified that ASF in docker works properly. Hit `CTRL+P` then `CTRL+Q` in order to quit foreground docker container, then stop ASF container with `docker stop asf`.